<template>
  <popup
    :visible="visible"
    :closable="true"
    :title="$t('message.leaderboard.title')"
    @close="hidePopup"
  >
    <div :class="['container flex column', {loading: loadingStatistics}]">
      <transition name="fade">
        <div class="loading-title" v-show="loadingStatistics">
          {{ $t('message.loading') }}...
        </div>
      </transition>
      <div class="flex row justify-content-left tabs">
        <div
          @click="setGeneral(false)"
          :class="['tab flex stretch text caption justify-content-center', {active: !general}]"
        >
          {{ $t('message.leaderboard.current') }}
        </div>
        <div
          @click="setGeneral(true)"
          :class="['tab flex stretch text caption justify-content-center', {active: general}]"
        >
          {{ $t('message.leaderboard.general') }}
        </div>
      </div>
      <div class="wrapper">
        <control-leaderboard v-show="!general" :general="false"></control-leaderboard>
        <control-leaderboard v-show="general" :general="true"></control-leaderboard>
      </div>
    </div>
  </popup>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import ControlLeaderboard from '@/components/controls/Leaderboard.vue';
import Popup from '@/components/popups/Popup.vue';
import {
  Popups,
} from '@/utils';

export default {
  components: {
    Popup,
    ControlLeaderboard,
  },
  data() {
    return {
      general: false,
    };
  },
  computed: {
    ...mapGetters({
      playerAddress: 'playerAddress',
      loadingStatistics: 'loadingStatistics',
      popupsVisibility: 'popupsVisibility',
    }),
    visible() {
      return this.popupsVisibility[Popups.LEADERBOARD];
    },
  },
  methods: {
    ...mapActions({
      setPopupVisibility: 'setPopupVisibility',
    }),
    hidePopup() {
      this.setPopupVisibility({
        popup: Popups.LEADERBOARD,
        visible: false,
      });
    },
    setGeneral(value) {
      this.general = value;
    },
  },
};
</script>

<style scoped lang="scss">
  $td-border-radius: 5px;
  $stroke-color: rgba(125, 202, 250, 0.3);

  .container {
    margin: 0 1vw;
    color: white;
    font-family: "Bahnschrift", sans-serif;
    transition: opacity 0.5s ease-in-out;

    @media (min-aspect-ratio: 16/9) {
      margin: 0 calc((16 / 9) * 1vh) !important;
    }

    .wrapper {
      padding: 1vw;
      border-left: 1px solid $stroke-color;
      border-right: 1px solid $stroke-color;
      border-bottom: 1px solid $stroke-color;
      background-color: rgba(125, 202, 250, 0.1);

      @media (min-aspect-ratio: 16/9) {
        padding: calc((16 / 9) * 1vh);
      }
    }

    .tabs {
      padding-top: 1vw;

      @media (min-aspect-ratio: 16/9) {
        padding-top: calc((16 / 9) * 1vh);
      }

      .tab {
        padding: 1vw 0;
        border-left: 1px solid $stroke-color;
        border-right: 1px solid $stroke-color;
        border-top: 1px solid $stroke-color;
        border-top-left-radius: 10px;
        border-top-right-radius: 10px;
        transition: background-color 0.1s ease-in-out;

        @media (min-aspect-ratio: 16/9) {
          padding: calc((16 / 9) * 1vh) 0;
        }

        &.text {
          font-size: 0.8vw !important;

          @media (min-aspect-ratio: 16/9) {
            font-size: calc((16 / 9) * 0.8vh) !important;
          }
        }

        &.active {
          background-color: rgba(125, 202, 250, 0.1);
        }

        &:not(.active) {
          cursor: pointer;
          border-bottom: 1px solid $stroke-color;
        }

        &:not(.active):nth-child(1) {
          border-right: transparent;
        }

        &:not(.active):nth-child(2) {
          border-left: transparent;
        }

        &:not(.active):hover {
          background-color: rgba(125, 202, 250, 0.05);
        }
      }
    }

    &.loading {
      opacity: 0.5;
    }

    .loading-title {
      font-size: 0.8vw;
      position: absolute;
      text-transform: uppercase;

      @media (min-aspect-ratio: 16/9) {
        font-size: calc((16 / 9) * 0.8vh);
      }
    }
  }
</style>
