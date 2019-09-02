<template>
  <div class="control get-keeper">
    <div class="text title white glow uppercase centred w-100">
      {{ $t('message.getWavesKeeper.title') }}
    </div>
    <div class="text subscription white centred w-100">
      {{ $t('message.getWavesKeeper.details') }}
    </div>
    <small-button
      :title="$t('message.getWavesKeeper.button')"
      @click="installWavesKeeper">
    </small-button>
  </div>
</template>

<script>
import SmallButton from '@/components/controls/SmallButton.vue';
import { getAgentExtLink } from '@/utils';

export default {
  data() {
    return {
      hovered: false,
    };
  },
  components: {
    SmallButton,
  },
  computed: {
    currentImage() {
      if (this.hovered) {
        return 'image-get-keeper-hover';
      }
      return 'image-get-keeper-base';
    },
  },
  methods: {
    handleInstall() {
      if (this.isChoiceDone) {
        this.play();
      } else {
        this.setBackgroundFaded(true);
      }
    },
    extensionLink() {
      return getAgentExtLink();
    },
    installWavesKeeper() {
      const link = this.extensionLink();
      window.open(
        link,
        '_blank',
      );
    },
  },
};
</script>

<style lang="scss" scoped>
  $k-ratio: calc(200 / 68);
  $button-width: 12vw;
  $button-height: calc(#{$button-width} / #{$k-ratio});

  .get-keeper {
    cursor: default;
    background-size: 100% 100% !important;
    background: url("../../assets/images/bg-window.png") 0 0 no-repeat;

    .text {
      position: absolute;
      width: inherit;
      height: inherit;
    }

    .button {
      margin-top: 5vw;
    }

    .title {
      margin-top: 2.8vw;
      font-size: 1.3vw;
      font-weight: bold;
    }

    .subscription {
      margin-top: 5vw;
      font-size: 0.9vw;
      color: whitesmoke;
      opacity: 0.6;
    }

    @supports (-moz-appearance:none) {
      .title {
        margin-top: 4.5vw;
      }

      .subscription {
        margin-top: 9vw;
      }
      a {
        margin-top: 7.6vw;
        font-size: 1vw;
        color: white;
        text-decoration: none;
      }
    }

    width: 27vw;
    height: calc((9 / 16) * 20vw);
    background-size: cover;

    @media (min-aspect-ratio: 16/9) {

      width: 48vh;
      height: 20vh;

      .title {
        margin-top: 4vh;
        font-size: 2.3vh;
        font-weight: bold;
      }

      .subscription {
        margin-top: 8.5vh;
        font-size: 1.5vh;
      }

      .button {
        margin-top: 9vh;
      }


      @supports (-moz-appearance:none) {
        .title {
          margin-top: 9vh;
        }

        .subscription {
          margin-top: 18vh;
        }

      }
    }

    &.disabled {
      pointer-events: none;
    }

    &.selected {
      background-color: aquamarine;
    }
  }
</style>
