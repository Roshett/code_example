
<template>
  <div
    class="w-100vw h-100vh flex row justify-content-center
          align-items-center wh-100-aspect absolute scaled">
    <div class="flex column justify-content-center align-items-center" style="height: 70%">
      <div class="flex row" style="height: 90%">
        <div
          class="flex column"
          :class="{ unavailable: isUnavailable }"
        >
          <scene-left-block></scene-left-block>
        </div>
        <div
          class="flex column align-items-center"
          :class="{ unavailable: isUnavailable }"
        >
          <div class="flex column justify-content-center align-items-center">
            <div id="tutorial-step-1">
              <scene-bet-type-container></scene-bet-type-container>
            </div>
            <div id="tutorial-step-2">
              <scene-bet-multiplier-container></scene-bet-multiplier-container>
            </div>
          </div>
          <div id="tutorial-step-3" class="flex column justify-content-center align-items-center">
            <component :is="component"></component>
            <control-current-game v-show="!isUnavailable"></control-current-game>
          </div>
        </div>
        <div class="flex row">
          <scene-right-block></scene-right-block>
          <control-locale-changer></control-locale-changer>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ControlCurrentGame from '@/components/controls/CurrentGame.vue';
import ControlProcessing from '@/components/controls/Processing.vue';
import ControlPlayButton from '@/components/controls/PlayButton.vue';
import ControlGetKeeperButton from '@/components/controls/InstallWavesKeeper.vue';
import SceneLeftBlock from '@/components/scene/LeftBlock.vue';
import SceneRightBlock from '@/components/scene/RightBlock.vue';
import SceneBetTypeContainer from '@/components/scene/BetTypesContainer.vue';
import SceneBetMultiplierContainer from '@/components/scene/BetMultiplierContainer.vue';
import ControlLocaleChanger from '@/components/controls/LocaleChanger.vue';
import { mapGetters } from 'vuex';

export default {
  components: {
    ControlCurrentGame,
    ControlProcessing,
    ControlPlayButton,
    ControlGetKeeperButton,
    ControlLocaleChanger,
    SceneBetMultiplierContainer,
    SceneBetTypeContainer,
    SceneLeftBlock,
    SceneRightBlock,
  },
  computed: {
    ...mapGetters({
      isProcessing: 'isProcessing',
      isKeeperInitialized: 'wavesKeeperInitialized',
      isUnavailable: 'isUnavailable',
    }),
    component() {
      if (this.isProcessing && !this.isUnavailable) {
        return ControlProcessing;
      }

      return this.isKeeperInitialized ? ControlPlayButton : ControlGetKeeperButton;
    },
  },
};
</script>

<style lang="scss" scoped>
  .unavailable {
    pointer-events: none;
  }
</style>
