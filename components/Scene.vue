<template>
  <div class="container flex column justify-content-center align-items-center">
    <div class="overlay overlay-space"></div>
    <div class="scene flex justify-content-center align-items-center">
      <transition name="fade">
        <div v-show="backgroundFaded" class="shadow"></div>
      </transition>
      <div class="overlay" style="margin-right: -3vh;"></div>
      <div class="prallax-wrapper">
        <div data-relative-input="true" id="px-1">
          <div data-depth="0.05">
            <scene-background></scene-background>
          </div>
        </div>
        <scene-planet-earth></scene-planet-earth>
        <img class="planet block absolute" src="../assets/images/first-plane.png" alt="">
      </div>
      <div class="wh-100-aspect absolute">
        <scene-comet :variant="2"></scene-comet>
        <scene-comet :variant="2"></scene-comet>
        <scene-comet :variant="2"></scene-comet>
      </div>
      <div class="overlay" style="margin-left: -3vh;"></div>
      <div class="wh-100-aspect absolute">
        <scene-central></scene-central>
        <control-last-games></control-last-games>
        <control-more-games></control-more-games>
        <control-socials></control-socials>
      </div>
      <div class="popup">
        <popup-leaderboard></popup-leaderboard>
        <popup-information></popup-information>
        <popup-add-account></popup-add-account>
        <popup-permissions></popup-permissions>
        <popup-game-info-selected></popup-game-info-selected>
        <popup-insufficient-funds></popup-insufficient-funds>
        <popup-switch-network></popup-switch-network>
        <popup-unavailable></popup-unavailable>
      </div>
    </div>
    <div class="overlay overlay-space"></div>
  </div>
</template>

<script>
import ControlLastGames from '@/components/controls/LastGames.vue';
import ControlMoreGames from '@/components/controls/WindowMoreGames.vue';
import SceneCentral from '@/components/scene/Central.vue';
import SceneBackground from '@/components/scene/Background.vue';
import SceneComet from '@/components/scene/Comet.vue';
import ScenePlanetEarth from '@/components/scene/PlanetEarth.vue';
import PopupLeaderboard from '@/components/popups/PopupLeaderboard.vue';
import PopupInformation from '@/components/popups/PopupInformation.vue';
import PopupInsufficientFunds from '@/components/popups/PopupInsufficientFunds.vue';
import PopupSwitchNetwork from '@/components/popups/PopupSwitchNetwork.vue';
import PopupGameInfoSelected from '@/components/popups/PopupGameInfoSelected.vue';
import PopupAddAccount from '@/components/popups/PopupAddAccount.vue';
import PopupPermissions from '@/components/popups/PopupPermissions.vue';
import PopupUnavailable from '@/components/popups/PopupUnavailable.vue';
import ControlSocials from '@/components/controls/Socials.vue';
import Parallax from 'parallax-js';
import { mapGetters } from 'vuex';

export default {
  components: {
    SceneComet,
    SceneCentral,
    SceneBackground,
    ScenePlanetEarth,
    PopupLeaderboard,
    PopupInformation,
    PopupGameInfoSelected,
    PopupInsufficientFunds,
    PopupSwitchNetwork,
    PopupPermissions,
    PopupAddAccount,
    PopupUnavailable,
    ControlMoreGames,
    ControlLastGames,
    ControlSocials,
  },
  mounted() {
    this.createParallaxAt(document.getElementById('px-1'));
  },
  methods: {
    createParallaxAt(el) {
      return new Parallax(el, {
        relativeInput: true,
      });
    },
  },
  computed: {
    ...mapGetters({
      backgroundFaded: 'backgroundFaded',
    }),
    captionStyle() {
      return ['caption text uppercase white glow shadow-off', { flicker: this.backgroundFaded }];
    },
  },
};
</script>

<style lang="scss" scoped>
  .container {
    position: absolute;
    height: 100%;
    width: 100%;

    .scene {
      position: relative;
      z-index: 1;
      width: 100%;
      height: calc((9 / 16) * 100vw);

      .absolute {
        position: absolute;
      }

      .prallax-wrapper::after {
        content: "";
        background-color: #fff;
        height: 100%;
        width: 2vw;
      }

      .center-wrapper {
        position: absolute;
        margin: auto;
        top: 15%;
        right: 0;
        left: 0;
      }

      .planet {
        position: absolute;
        width: 75vw;
        height: calc((9 / 16) * 64vw);
        bottom: 0;
        right: 0;
      }

      @media (min-aspect-ratio: 16/9) {
        .planet {
          height: 63vh;
          width: calc((16 / 9) * 74vh);
        }
      }

      .prallax-wrapper {
        position: relative;
      }

      .popup {
        width: 60%;
        display: inline-flex;
        position: fixed;
        align-items: center;
        justify-content: center;
        z-index: 5;
      }
    }

    .overlay {
      position: relative;
      z-index: 2;
      width: 100%;
      flex: 1;
      background-color: #000;
    }

    @media (min-aspect-ratio: 16/9) {
      .overlay {
        height: 100vh;
        width: 2vw;
        z-index: 2;
      }

      .overlay-space {
        visibility: hidden;
      }
    }
  }
</style>
