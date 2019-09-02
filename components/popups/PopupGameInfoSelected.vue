<template>
  <popup
    :visible="visible"
    :closable="true"
    :title="$t('message.information.title')"
    :no-title="true"
    size="medium"
    @close="hidePopup"
  >
    <div class="container flex column stretch" v-if="gameInfoSelected">
      <ul>
        <li class="flex row bigger">
          <div class="title">{{$t('message.gameInfoSelected.player')}}:</div>
          <div>
            <a target="_blank" :href="gameInfoSelected.playerAddressLink">
              {{gameInfoSelected.playerAddress}}
            </a>
          </div>
        </li>
        <li class="flex row bigger">
          <div class="title">{{$t('message.gameInfoSelected.score')}}:</div>
          <div class="rating-wrapper">
            {{gameInfoSelected.playerScore}}
            <span :class="['rating', {increased: gameInfoSelected.ratingPoints >= 0}]">
              {{ratingPoints}}
            </span>
          </div>
        </li>
        <li class="flex row">
          <div class="title">{{$t('message.gameInfoSelected.bet')}}:</div>
          <div>
            {{gameInfoSelected.bet}}
            <img class="wave" src="../../assets/images/wave-small.png" alt="">
          </div>
        </li>
        <li class="flex row">
          <div class="title">{{$t('message.gameInfoSelected.playerChoice')}}:</div>
          <img class="thumbnail" :src="gameInfoSelected.thumbnailPlayer" alt="">
        </li>
        <li class="flex row">
          <div class="title">{{$t('message.gameInfoSelected.systemChoice')}}:</div>
          <img class="thumbnail" :src="serverThumbnail" alt="">
        </li>
        <li class="title flex align-items-center">
          <div class="title">{{$t('message.gameInfoSelected.won')}}:</div>
          <div v-if="gameInfoSelected.serverWinner">
            <span style="opacity: 0.5">&#8212;</span>
          </div>
          <div v-else>
            {{gameInfoSelected.cleanWonAmount}}
            <img class="wave" src="../../assets/images/wave-small.png" alt="">
          </div>
        </li>
      </ul>
      <div class="flex row buttons justify-content-center">
        <a target="_blank" :href="gameInfoSelected.smartAddressLink">
          <button-small :title="$t('message.gameInfoSelected.smartContract')"></button-small>
        </a>
      </div>
    </div>
  </popup>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';
import ThumbnailWaveBlue from '@/assets/images/thumb-wave-blue.png';
import ThumbnailWaveYellow from '@/assets/images/thumb-wave-yellow.png';
import ThumbnailRocketBlue from '@/assets/images/thumb-rocket-blue.png';
import ThumbnailRocketYellow from '@/assets/images/thumb-rocket-yellow.png';
import ButtonSmall from '@/components/controls/SmallButton.vue';
import Popup from './Popup.vue';
import { Popups } from '@/utils';

export default {
  components: {
    Popup,
    ButtonSmall,
  },
  computed: {
    ...mapGetters({
      popupsVisibility: 'popupsVisibility',
      gameInfoSelected: 'gameInfoSelected',
    }),
    visible() {
      return this.popupsVisibility[Popups.GAME_INFORMATION];
    },
    ratingPoints() {
      if (this.gameInfoSelected.ratingPoints > 0) {
        return `+${this.gameInfoSelected.ratingPoints}`;
      }

      return this.gameInfoSelected.ratingPoints;
    },
    playerThumbnail() {
      return this.gameInfoSelected.playerChoice === 0 ? ThumbnailRocketBlue : ThumbnailWaveBlue;
    },
    serverThumbnail() {
      if (this.gameInfoSelected.serverWinner) {
        return this.gameInfoSelected.serverChoice === 0
          ? ThumbnailRocketYellow : ThumbnailWaveYellow;
      }

      return this.gameInfoSelected.serverChoice === 0 ? ThumbnailRocketBlue : ThumbnailWaveBlue;
    },
  },
  methods: {
    ...mapActions({
      setPopupVisibility: 'setPopupVisibility',
    }),
    hidePopup() {
      this.setPopupVisibility({
        popup: Popups.GAME_INFORMATION,
        visible: false,
      });
    },
  },
};
</script>

<style scoped lang="scss">
  .container {
    font-family: "Bahnschrift", sans-serif;

    .thumbnail {
      height: 1.25vw;
      padding-left: 0.2vw;
    }

    a:hover {
      color: rgb(125, 202, 250);
    }

    @media (min-aspect-ratio: 16/9) {
      .thumbnail {
        height: calc((16 / 9) * 1.25vh);
        padding-left: calc((16 / 9) * 0.2vh);
      }

      ul {
        margin: 0 0 calc((16 / 9) * 1.5vh) 0 !important;
        padding: 0 calc((16 / 9) * 2vh) !important;

        li {
          height: calc((16 / 9) * 1.6vh) !important;
          font-size: calc((16 / 9) * 1vh) !important;
          letter-spacing: calc((16 / 9) * 0.03vh) !important;

          &.bigger {
            font-size: calc((16 / 9) * 1.25vh) !important;
          }

          div:last-child {
            padding-left: calc((16 / 9) * 0.5vh) !important;
          }
        }

        .rating-wrapper {

          .rating {
            font-size: calc((16 / 9) * 0.9vh) !important;
            padding-left: calc((16 / 9) * 0.2vh) !important;
            top: calc((16 / 9) * -0.5vh) !important;
          }
        }

        li:nth-child(2) {
          margin-bottom: calc((16 / 9) * 1.2vh) !important;
        }
      }
    }

    ul {
      list-style-type: none;
      text-transform: uppercase;
      margin: 0 0 1.5vw 0;
      padding: 0 2vw;

      li {
        height: 1.6vw;
        display: flex;
        align-items: center;
        font-size: 1vw;
        letter-spacing: 0.03vw;

        &.bigger {
          font-size: 1.25vw;
        }

        .rating-wrapper {
          position: relative;

          .rating {
            font-size: 0.9vw;
            padding-left: 0.2vw;
            top: -0.5vw;
            position: relative;
            color: rgba(255, 30, 0, 0.7);

            &.increased {
              color: rgba(30, 255, 0, 0.7);
            }
          }
        }

        div:first-child {
          color: rgb(125, 202, 250);
        }

        div:last-child {
          color: white;
          padding-left: 0.5vw;
        }
      }

      li:nth-child(2) {
        margin-bottom: 1.2vw;
      }
    }

    .buttons {
      a {
        text-decoration: none;
      }
    }
  }
</style>
