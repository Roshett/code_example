<template>
  <div :class="['button', {disabled: disabled}]" @click="onClick">
    <div class="text white centered">{{title}}</div>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    onClick() {
      this.$emit('click');
    },
  },
};
</script>

<style scoped lang="scss">
  $button-height: 4;

  .button {
    z-index: 5;
    height: #{$button-height}vw;
    width: 12.5vw;
    display: block;
    background: url("../../assets/images/button-small-sprite.png") 0 0 no-repeat;
    background-size: 200% 100%;
    cursor: pointer;

    @media (min-aspect-ratio: 16/9) {
      height: calc((16 / 9) * #{$button-height}vh) !important;
      width: 20vh;
      .text {
        padding: calc((16 / 9) * 1.6vh) calc((16 / 9) * 1vh) !important;
        font-size: calc((16 / 9) * 0.9vh) !important;
      }
    }

    .text {
      text-align: center;
      padding: 1.6vw 1vw;
      font-size: 0.9vw;
      text-transform: uppercase;
    }

    &.disabled {
      opacity: 0.5;
      cursor: default;
    }

    &:hover:not(.disabled) {
      background: url("../../assets/images/button-small-sprite.png") 100% 0 no-repeat;
      background-size: 200% 100%;

      .text {
        opacity: 0.8;
      }
    }
  }
</style>
