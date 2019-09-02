<template>
  <div
    :class="['control chooser', {disabled: isDisabled, active: isActive}]"
    @click="handleChoose"
  >
    <div
      :style="style"
      :class="['coin-wrapper', {thumbnail: thumbnail, faded: faded}]">
      <div
        class="coin"
        :class="currentImage"
        @mouseenter="onMouseEnter"
        @mouseleave="onMouseLeave">
      </div>
    </div>
  </div>
</template>

<script>
import { BetType } from '@/models';
import { mapGetters, mapActions } from 'vuex';

export default {
  props: {
    type: {
      type: Number,
      required: true,
    },
    thumbnail: {
      type: Boolean,
      default: false,
    },
    thumbnailScale: {
      type: Number,
      default: 1,
    },
    active: {
      type: Boolean,
      default: false,
    },
    faded: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      hovered: false,
    };
  },
  computed: {
    ...mapGetters({
      isProcessing: 'isProcessing',
      betType: 'coinSide',
    }),
    style() {
      return `transform: scale(${this.thumbnailScale},${this.thumbnailScale})`;
    },
    currentImage() {
      if (this.type === BetType.HEADS) {
        if (this.isActive) {
          return 'image-rocket-active';
        }
        if (this.hovered) {
          return 'image-rocket-hover';
        }
        return 'image-rocket-base';
      }

      if (this.type === BetType.TAILS) {
        if (this.isActive) {
          return 'image-waves-active';
        }
        if (this.hovered) {
          return 'image-waves-hover';
        }
        return 'image-waves-base';
      }

      return null;
    },
    isActive() {
      if (this.thumbnail) {
        return this.active;
      }

      return this.betType === this.type;
    },
    isDisabled() {
      return this.isProcessing || this.thumbnail;
    },
  },
  methods: {
    ...mapActions({
      setBetType: 'setBetType',
    }),
    handleChoose() {
      this.setBetType(this.type);
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
  .chooser {
    opacity: 0.7;
    transition: all 0.1s ease-in-out;

    &:hover {
      transform: scale(1.05);
      opacity: 0.8;
    }

    &.active {
      transform: scale(1.1);
      opacity: 1;
    }

    .coin-wrapper {
      $coin-size-w: 22vw;

      &.faded {
        opacity: 0.7;
      }

      @media (min-aspect-ratio: 16/9) {
        $coin-size-h: 22vh;

        width: $coin-size-h !important;
        height: $coin-size-h !important;

        .coin {
          width: $coin-size-h !important;
          height: $coin-size-h !important;
        }
      }

      width: calc((9 / 16) * #{$coin-size-w});
      height: calc((9 / 16) * #{$coin-size-w});
      display: block;
      position: relative;
      cursor: pointer;

      .coin {
        width: calc((9 / 16) * #{$coin-size-w});
        height: calc((9 / 16) * #{$coin-size-w});
        position: absolute;
        display: block;
      }

      &.thumbnail {
        $coin-size: 3;

        @media (min-aspect-ratio: 16/9) {

          width: #{$coin-size}vh !important;
          height: #{$coin-size}vh !important;

          .coin {
            width: #{$coin-size}vh !important;
            height: #{$coin-size}vh !important;
          }
        }

        width: calc((9 / 16) * #{$coin-size}vw);
        height: calc((9 / 16) * #{$coin-size}vw);

        .coin {
          width: calc((9 / 16) * #{$coin-size}vw);
          height: calc((9 / 16) * #{$coin-size}vw);
        }
      }
    }

    .image-rocket-base {
      background: url("../../assets/images/coin-rocket-sprite.png") 0 0 no-repeat;
      background-size: 300% 100%;
    }

    .image-rocket-hover {
      background: url("../../assets/images/coin-rocket-sprite.png") 50% 0 no-repeat;
      background-size: 300% 100%;
    }

    .image-rocket-active {
      background: url("../../assets/images/coin-rocket-sprite.png") 100% 0 no-repeat;
      background-size: 300% 100%;
    }

    .image-waves-base {
      background: url("../../assets/images/coin-waves-sprite.png") 0 0 no-repeat;
      background-size: 300% 100%;
    }

    .image-waves-hover {
      background: url("../../assets/images/coin-waves-sprite.png") 50% 0 no-repeat;
      background-size: 300% 100%;
    }

    .image-waves-active {
      background: url("../../assets/images/coin-waves-sprite.png") 100% 0 no-repeat;
      background-size: 300% 100%;
    }
  }
</style>
