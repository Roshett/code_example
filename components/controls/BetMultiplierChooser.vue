<template>
  <div
    :class="['control multiplier', {disabled: disabled, active: active}]"
    @click="handleChoose"
  >
    <div class="button-wrapper">
      <div
        class="button"
        :class="currentImage"
        @mouseenter="onMouseEnter"
        @mouseleave="onMouseLeave"
      >
      </div>
    </div>
    <div class="text uppercase centred black">
      <p class="amount">{{factor}}</p>
      <p class="waves">WAVES</p>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  props: {
    factor: {
      number: Number,
      required: true,
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
      betMultiplier: 'betAmount',
    }),
    active() {
      return this.betMultiplier === this.factor;
    },
    disabled() {
      return this.isProcessing;
    },
    currentImage() {
      if (this.active) {
        return 'image-multiplier-chooser-active';
      }
      if (this.hovered) {
        return 'image-multiplier-chooser-hover';
      }
      return 'image-multiplier-chooser-base';
    },
  },
  methods: {
    ...mapActions({
      setBetMultiplier: 'setBetMultiplier',
    }),
    handleChoose() {
      this.setBetMultiplier(this.factor);
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
  .multiplier {
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;

    .text {
      position: absolute;
      font-family: 'ChicagoFlat', monospace;
      padding-bottom: 1.0vw;
      pointer-events: none;
      letter-spacing: 0;
      font-weight: 300;

      .amount {
        font-size: calc((9 / 16) * 2.2vw);
        margin: 0;
      }

      .waves {
        font-size: calc((9 / 16) * 1.2vw);
        margin: 0;
      }
    }

    &:hover:not(.active) {
      .text {
        padding-top: calc((9 / 16) * 1.0vw);
      }
    }

    &.active {
      .text {
        padding-top: calc((9 / 16) * 1.5vw);
      }
    }

    @media (min-aspect-ratio: 16/9) {
      .text {
        position: absolute;
        font-family: 'ChicagoFlat', monospace;
        padding-bottom: calc((16 / 9) * 1.0vh);
        pointer-events: none;

        .amount {
          font-size: 2.2vh;
          margin: 0;
        }

        .waves {
          font-size: 1.2vh;
          margin: 0;
        }
      }

      &:hover:not(.active) {
        .text {
          padding-top: 1.0vh;
        }
      }

      &.active {
        .text {
          padding-top: 1.5vh;
        }
      }
    }
  }

  .button-wrapper {
    $button-size-w: 12vw;

    @media (min-aspect-ratio: 16/9) {
      $button-size-h: 12vh;

      width: $button-size-h !important;
      height: $button-size-h !important;

      .button {
        width: $button-size-h !important;
        height: $button-size-h !important;
      }
    }

    width: calc((9 / 16) * #{$button-size-w});
    height: calc((9 / 16) * #{$button-size-w});
    display: block;
    position: relative;
    cursor: pointer;

    .button {
      width: calc((9 / 16) * #{$button-size-w});
      height: calc((9 / 16) * #{$button-size-w});
      position: absolute;
      display: block;
    }
  }

  .image-multiplier-chooser-base {
    background: url("../../assets/images/button-yellow-sprite.png") 0 0 no-repeat;
    background-size: 300% 100%;
  }

  .image-multiplier-chooser-hover {
    background: url("../../assets/images/button-yellow-sprite.png") 50% 0 no-repeat;
    background-size: 300% 100%;
  }

  .image-multiplier-chooser-active {
    background: url("../../assets/images/button-yellow-sprite.png") 100% 0 no-repeat;
    background-size: 300% 100%;
  }
</style>
