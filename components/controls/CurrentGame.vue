<template>
  <transition key="t-current-game" name="fade" mode="out-in">
    <div v-if="visible" class="current flex justify-content-center align-items-center">
      <transition name="fade" mode="out-in">
        <div
          key="t-process"
          v-if="isWinnerIsUndefined"
          class="container-wrapper flex column align-items-center">
          <ul>
            <li class="flex row">
              <div class="title">ID:</div>
              <div>
                <a target="_blank" href="#" @click.prevent="copyGameId">
                  {{currentGameId}}
                  <span class="copy">(copy)</span>
                </a>
              </div>
            </li>
            <li class="flex row">
              <div class="title">{{$t('message.gameInfoSelected.bet')}}:</div>
              <div>
                {{currentGameBetAmount}}
                <img class="wave" src="../../assets/images/wave-small.png" alt="">
              </div>
            </li>
            <li class="flex row">
              <div class="title">{{$t('message.gameInfoSelected.playerChoice')}}:</div>
              <img class="thumbnail" src="../../assets/images/thumb-rocket-blue.png"
                   alt=""
                   v-if="!currentGameCoinSide">
              <img class="thumbnail" src="../../assets/images/thumb-wave-blue.png"
                    alt=""
                   v-else>
            </li>
          </ul>
          <div class="flex row buttons">
            <a target="_blank" :href="smartAddressLink">
              <control-small-button :title="$t('message.gameInfoSelected.smartContract')">
              </control-small-button>
            </a>
          </div>
        </div>
        <div key="t-winner" v-else class="winner white text caption centered">
          <div v-if="isWinnerPlayer">
            {{$t('message.won.title')}}
          </div>
          <div v-else>
            {{$t('message.lost.title')}}
          </div>
        </div>
      </transition>
    </div>
  </transition>
</template>

<script>
import { mapGetters } from 'vuex';
import urlJoin from 'url-join';
import config from '@/config';
import { addressTruncate, ProcessSteps, States } from '@/utils';
import ControlSmallButton from '@/components/controls/SmallButton.vue';

export default {
  components: {
    ControlSmallButton,
  },
  props: {
    title: String,
  },
  computed: {
    ...mapGetters({
      betAmount: 'betAmount',
      playerAddress: 'playerAddress',
      selectedDices: 'selectedDices',
      processingStep: 'processingStep',
      gameInfoCurrent: 'gameInfoCurrent',
    }),
    visible() {
      return this.processingStep >= ProcessSteps.PLACE_BET;
    },
    currentGameId() {
      const { gameId } = this.gameInfoCurrent;
      return addressTruncate(gameId, 7, 7, '***');
    },
    currentGameBetAmount() {
      const { betAmount } = this.gameInfoCurrent;
      return betAmount;
    },
    currentGameCoinSide() {
      const { coinSide } = this.gameInfoCurrent;
      return coinSide;
    },
    playerAddressLink() {
      return urlJoin(
        config.api.explorer.url,
        config.api.explorer.address,
        this.playerAddress,
      );
    },
    smartAddressLink() {
      return urlJoin(
        config.api.explorer.url,
        config.api.explorer.address,
        config.dapp.address,
      );
    },
    isWinnerIsUndefined() {
      return this.gameInfoCurrent.winner === null;
    },
    isWinnerPlayer() {
      return this.gameInfoCurrent.winner === States.WON;
    },
  },
  methods: {
    async copyGameId() {
      const { gameId } = this.gameInfoCurrent;
      await navigator.clipboard.writeText(gameId);
    },
  },
};
</script>

<style scoped lang="scss">
  .current {
    bottom: 7vw;
    position: absolute;
    background: url("../../assets/images/window-small.png") no-repeat center;
    background-size: contain;
    height: 10vw;
    width: 18vw;

    .thumbnail {
      height: 1vw;
      padding-left: 0.2vw;
    }

    .copy {
      padding: 0 0.3vw;
      font-size: 0.5vw;
      font-family: Arial, sans-serif;
    }

    @media (min-aspect-ratio: 16/9) {
      bottom: calc((16 / 9) * 7vh) !important;
      width: calc((16 / 9) * 18vh) !important;
      height: calc((16 / 9) * 10vh) !important;

      .thumbnail {
        height: calc((16 / 9) * 1vh);
        padding-left: calc((16 / 9) * 0.2vh);
      }

      .copy {
        padding: 0 calc((16 / 9) * 0.3vh);
        font-size: calc((16 / 9) * 0.5vh);
      }

      .content {
        width: calc((16 / 9) * 30vh) !important;
        margin-top: calc((16 / 9) * 1vh) !important;
      }

      .container-wrapper {
        margin-top: calc((16 / 9) * 1vh) !important;

        ul {
          margin: 0;

          li {
            height: calc((16 / 9) * 1.6vh) !important;
            font-size: calc((16 / 9) * 1vh) !important;
            letter-spacing: calc((16 / 9) * 0.03vh) !important;

            &.bigger {
              font-size: calc((16 / 9) * 1.2vh) !important;
            }

            div:last-child {
              padding-left: calc((16 / 9) * 0.5vh) !important;
            }
          }
        }
      }
    }

    .content {
      width: 30vw;
      margin-top: 1vw;
    }

    .container-wrapper {
      margin-top: 1vw;

      a:hover {
        color: rgb(125, 202, 250);
      }

      ul {
        list-style-type: none;
        text-transform: uppercase;
        margin: 0;
        padding-left: 0;

        li {
          height: 1.6vw;
          display: flex;
          align-items: center;
          font-size: 1vw;
          letter-spacing: 0.03vw;

          &.bigger {
            font-size: 1.2vw;
          }

          div:first-child {
            color: rgb(125, 202, 250);
          }

          div:last-child {
            color: white;
            padding-left: 0.5vw;
          }
        }
      }

      .buttons {

        a {
          text-decoration: none;
        }
      }
    }
  }
</style>
