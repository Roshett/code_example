<template>
  <div class="panel flex column">
    <div class="flex row align-items-center">
      <div class="flex column align-items-center justify-content-center">
        <img class="logo" src="../../assets/images/logo.png" alt="">
        <div class="steps">
          <scene-step
            id="tutorial-step-5"
            icon="fa-trophy"
            @click="showPopupLeaderboard"
            :title="$t('message.sideMenu.leaders')">
          </scene-step>
          <scene-step
            id="tutorial-step-6"
            icon="fa-book"
            @click="showPopupInformation"
            :title="$t('message.sideMenu.rules')">
          </scene-step>
          <scene-step
            icon="fa-graduation-hat"
            @click="showTutorial"
            :title="$t('message.sideMenu.tutorial')">
          </scene-step>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SceneStep from '@/components/scene/Step.vue';
import { mapActions } from 'vuex';
import {
  Popups,
} from '@/utils';

export default {
  components: {
    SceneStep,
  },
  methods: {
    ...mapActions({
      requestStatisticsUpdate: 'requestStatisticsUpdate',
      setPopupVisibility: 'setPopupVisibility',
      setTutorial: 'setTutorial',
    }),
    showPopupLeaderboard() {
      this.requestStatisticsUpdate();
      this.setPopupVisibility({
        popup: Popups.LEADERBOARD,
        visible: true,
      });
    },
    showPopupInformation() {
      this.setPopupVisibility({
        popup: Popups.INFORMATION,
        visible: true,
      });
    },
    showTutorial() {
      this.setTutorial(true);
    },
  },
};
</script>

<style scoped lang="scss">
  .panel {

    .logo {
      width: calc((9 / 16) * 13vw);
      height: calc((9 / 16) * 13vw);
    }

    .steps {
      margin: 2vw 0;
    }

    @media (min-aspect-ratio: 16/9) {

      .logo {
        width: 13vh;
        height: 13vh;
      }

      .steps {
        margin: 1vh 0;
      }
    }
  }
</style>
