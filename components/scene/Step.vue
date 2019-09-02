<template>
  <div class="step-wrapper" @click="click">
    <div :class="['step', { active: this.active }]"></div>
    <div :class="['text caption', { active: this.active }]">
      <div class="order">
        <component class="icon" :is="icon"></component>
      </div>
      <div class="title">{{title}}</div>
    </div>
    <div @mouseenter="mouseEnter" @mouseleave="mouseLeave" class="hitbox">
    </div>
  </div>
</template>

<script>
import faGraduationHat from '@fortawesome/fontawesome-free/svgs/solid/graduation-cap.svg';
import faTrophy from '@fortawesome/fontawesome-free/svgs/solid/trophy.svg';
import faBook from '@fortawesome/fontawesome-free/svgs/solid/book.svg';

export default {
  components: {
    faGraduationHat,
    faTrophy,
    faBook,
  },

  props: {
    title: {
      type: String,
      required: true,
    },
    icon: {
      type: String,
    },
  },
  data() {
    return {
      active: false,
    };
  },
  methods: {
    click() {
      this.$emit('click');
    },
    mouseEnter() {
      this.active = true;
    },
    mouseLeave() {
      this.active = false;
    },
  },
};
</script>

<style lang="scss" scoped>
  .step-wrapper {
    $k-ratio: calc(181 / 113);
    $step-width: 18vw;
    $step-height: calc(#{$step-width} / #{$k-ratio});

    width: calc((9 / 16) * #{$step-width});
    height: calc((9 / 16) * #{$step-height});
    display: block;
    margin-bottom: -2.5vw;
    margin-left: 1vw;

    .hitbox {
      z-index: 3;
      cursor: pointer;
      position: absolute;
      margin-top: 1.8vw;
      margin-left: 1.5vw;
      width: 7vw;
      height: 2.7vw;
      transform: rotate(-6deg);

      @media (min-aspect-ratio: 16/9) {
        margin-top: calc((16 / 9) * 2vh);
        margin-left: calc((16 / 9) * 1.8vh);
        width: calc((16 / 9) * 7vh);
        height: calc((16 / 9) * 2.7vh);
      }
    }

    .text {
      z-index: 2;
      pointer-events: none;
      position: absolute;
      text-align: left;
      display: flex;
      flex-direction: row;
      height: calc((9 / 16) * #{$step-height});
      margin-left: 2.7vw;
      align-items: center;
      color: black;
      transform: rotate(-6deg);

      &.active {
        color: white;

        .order {
          .icon {
            fill: white;
          }
        }
      }

      .order {
        width: 0.7vw;
        text-align: center;
        font-size: calc((9 / 16) * 1.5vw);

        .icon {
          width: 0.6vw;
          height: 0.6vw;
          fill: black;

          @media (min-aspect-ratio: 16/9) {
            width: calc((16 / 9) * 0.6vh);
            height: calc((16 / 9) * 0.6vh);
          }
        }
      }

      .title {
        width: 4vw;
        text-align: left;
        margin-left: calc((9 / 16) * 1.4vw);
        font-size: calc((9 / 16) * 1.4vw);
      }
    }

    .step {
      z-index: 1;
      width: calc((9 / 16) * #{$step-width});
      height: calc((9 / 16) * #{$step-height});
      position: absolute;
      display: block;
      background: url("../../assets/images/step-sprite.png") 0 0 no-repeat;
      background-size: 200% 100%;

      &.active {
        background: url("../../assets/images/step-sprite.png") 100% 0 no-repeat;
        background-size: 200% 100%;
      }
    }

    @media (min-aspect-ratio: 16/9) {
      $step-height-h: 12vh;
      $step-width-h: calc(#{$step-height-h} * #{$k-ratio});
      margin-bottom: -5vh;
      margin-left: 1vh;
      width: $step-width-h !important;
      height: $step-height-h !important;

      .step {
        width: $step-width-h !important;
        height: $step-height-h !important;
      }

      .text {
        height: $step-height-h !important;
        margin-left: 4.9vh !important;

        .order {
          width: 2vh !important;
          margin-bottom: 0.2vh !important;
          font-size: 1.5vh !important;
        }

        .title {
          width: 7vh !important;
          margin-left: 1vh !important;
          font-size: 1.4vh !important;
        }
      }
    }
  }
</style>
