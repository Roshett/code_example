<template>
  <transition name="fade">
    <div
      v-show="visible"
      :class="['popup-wrapper', size]">
      <div class="content">
        <div class="text caption centered header white">
          <div class="flex stretch" v-if="noTitle"></div>
          <div class="title" v-else>{{title}}</div>
          <div v-if="closable" class="close">
            <div @click="onCloseHandler">
              <icon-close></icon-close>
            </div>
          </div>
        </div>
        <slot></slot>
      </div>
    </div>
  </transition>
</template>

<script>
import IconClose from '@/assets/images/close.svg';
import { mapGetters } from 'vuex';

export default {
  props: {
    visible: Boolean,
    closable: Boolean,
    title: String,
    size: String,
    noTitle: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    IconClose,
  },
  computed: {
    ...mapGetters({
      isAnyPopupOpened: 'isAnyPopupOpened',
    }),
    isVisible() {
      return this.isAnyPopupOpened;
    },
  },
  methods: {
    onCloseHandler() {
      this.$emit('close');
    },
    onCloseEscapeHandler(event) {
      if (event.key === 'Escape' && this.closable) {
        this.onCloseHandler();
      }
    },
  },
  watch: {
    visible(visible) {
      if (visible) {
        window.addEventListener('keydown', this.onCloseEscapeHandler, false);
      } else {
        window.removeEventListener('keydown', this.onCloseEscapeHandler, false);
      }
    },
  },
};
</script>

<style scoped lang="scss">
  .popup-wrapper {
    position: absolute;
    flex-direction: column;
    display: flex;

    .content {
      position: relative;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-self: center;
      font-family: Roboto, sans-serif;
      padding: 1vw;
      border-radius: 1vw;
      box-shadow: 10px 10px 5px rgba(0, 0, 0, 0.5);
      background: url("../../assets/images/window-big.png") no-repeat center;
      background-size: 150% 150%;

      @media (min-aspect-ratio: 16/9) {
        padding: calc((16 / 9) * 1vh);
        border-radius: calc((16 / 9) * 1vh);
      }
    }

    .close {
      display: inline-flex;
      justify-content: flex-end;

      .svg-container {
        display: inline-block;
        width: 1vw;
        margin-right: 1vw;

        @media (min-aspect-ratio: 16/9) {
          width: calc((16 / 9) * 1vh);
          margin-right: calc((16 / 9) * 1vh);
        }

        path {
          fill: rgb(255, 255, 255);
        }

        &:hover {
          cursor: pointer;

          path {
            fill: rgb(138, 107, 245);
          }
        }
      }
    }

    .header {
      font-family: "Bahnschrift", sans-serif;
      text-align: center;
      justify-content: center;
      display: flex;
      flex-direction: row;
      font-size: 1.25vw !important;

      @media (min-aspect-ratio: 16/9) {
        font-size: calc((16 / 9) * 1.25vh) !important;
      }

      .title {
        flex: 1;
      }
    }
  }

  .fading {
    position: absolute;
    opacity: 0;
  }
</style>
