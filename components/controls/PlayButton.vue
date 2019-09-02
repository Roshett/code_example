<template>
  <div
    :class="['control play', {disabled: disabled}]"
    @click="handlePlay"
    v-show="isVisible"
  >
    <div class="flex column align-items-center justify-content-center">
      <div
        class="button"
        :class="currentImage"
        @mouseenter="onMouseEnter"
        @mouseleave="onMouseLeave"
      >
        <span class="text white centered">{{title}}</span>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  data() {
    return {
      hovered: false,
    };
  },
  computed: {
    ...mapGetters({
      isProcessing: 'isProcessing',
      betMultiplier: 'betAmount',
      betType: 'coinSide',
      isAnyPopupOpened: 'isAnyPopupOpened',
    }),
    disabled() {
      return this.isProcessing;
    },
    isVisible() {
      return !this.isAnyPopupOpened;
    },
    currentImage() {
      if (this.hovered) {
        return 'image-play-hover';
      }
      return 'image-play-base';
    },
    isChoiceDone() {
      return this.betMultiplier != null && this.betType != null;
    },
    title() {
      const processing = this.$t('message.buttons.processing');
      const play = this.$t('message.buttons.play');

      if (this.isProcessing) {
        return processing;
      }

      return play;
    },
  },
  methods: {
    ...mapActions({
      play: 'play',
      setBackgroundFaded: 'setBackgroundFaded',
    }),
    handlePlay() {
      if (this.isChoiceDone) {
        this.play();
      } else {
        this.setBackgroundFaded(true);
      }
    },
    onMouseEnter() {
      this.hovered = true;
    },
    onMouseLeave() {
      this.hovered = false;
    },
  },
};
</script>

<style lang="scss" scoped>

  .button {
    $k-ratio: calc(298 / 174);
    $button-width: 14vw;
    $button-height: calc(#{$button-width} / #{$k-ratio});

    @media (min-aspect-ratio: 16/9) {
      $button-height-h: 14vh;
      $button-width-h: calc(#{$button-height-h} * #{$k-ratio});

      width: $button-width-h !important;
      height: $button-height-h !important;

      .text {
        font-size: 2.0vh !important;
      }

      span {
        width: $button-width-h !important;
        line-height: $button-height-h !important;
      }
    }

    width: $button-width;
    height: $button-height;
    display: block;
    background: url("../../assets/images/button-big-sprite.png") 0 0 no-repeat;
    background-size: 200% 100%;
    text-align: center;
    text-transform: uppercase;
    cursor: pointer;

    .text {
      font-size: calc((9 / 16) * 2vw);
    }

    &.disabled {
      opacity: 0.5;
      cursor: default;
    }
        &:hover:not(.disabled) {
      width: $button-width;
      height: $button-height;
      background: url("../../assets/images/button-big-sprite.png") 100% 0 no-repeat;
      background-size: 200% 100%;

      span {
        opacity: 0.8;
      }
    }

    span {
      display: block;
      width: $button-width;
      line-height: $button-height;
    }
  }
</style>
