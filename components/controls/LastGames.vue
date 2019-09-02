<template>
  <div class="last-games">
    <div class="text white centered caption">{{$t('message.lastGames.title')}}</div>
    <table id="tutorial-step-4">
      <thead>
      <tr>
        <th>{{$t('message.lastGames.player')}}</th>
        <th>{{$t('message.lastGames.bet')}}</th>
        <th>{{$t('message.lastGames.playerChoice')}}</th>
        <th>{{$t('message.lastGames.systemChoice')}}</th>
        <th>{{$t('message.lastGames.won')}}</th>
      </tr>
      </thead>
      <tbody is="transition-group" name="list">
      <template v-for="(game, id) in statsLastGamesFormatted">
        <tr :class="{odd: id % 2 === 0}" :key="game.smartAddress" @click="showPopup(game)">
          <td class="data">{{game.playerAddress}}</td>
          <td class="data centered">
            {{game.bet}}
            <img class="wave" src="../../assets/images/wave-small.png" alt="">
          </td>
          <td class="data choice">
            <img class="thumbnail" :src="game.thumbnailPlayer" alt="">
          </td>
          <td class="data choice">
            <img class="thumbnail" :src="game.thumbnailServer" alt="">
          </td>
          <td class="data centered" v-if="game.serverWinner">
            <span style="opacity: 0.5">&#8212;</span>
          </td>
          <td class="data centered" v-else>
            {{game.cleanWonAmount}}
            <img class="wave" src="../../assets/images/wave-small.png" alt="">
          </td>
        </tr>
        <tr class="spacer" :key="`${game.smartAddress}-space`">
          <td class="spacer" colspan="5"></td>
        </tr>
      </template>
      </tbody>
    </table>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import {
  thumbnails,
  addressTruncate,
  Popups,
  States,
} from '@/utils';
import config from '@/config';
import urlJoin from 'url-join';

export default {
  components: {},
  computed: {
    ...mapGetters({
      statsLastGames: 'statsCurrentLastGames',
    }),
    statsLastGamesFormatted() {
      const arr = Array.from(this.statsLastGames)
        .slice(0, 5);
      return arr.map((game) => {
        const serverWinner = game.winner !== States.PLAYER;
        const playerChoice = parseInt(game.playerChoice, 10);
        const serverChoice = parseInt(game.serverChoice, 10);
        const [thumbnailPlayer, thumbnailServer] = thumbnails(serverWinner, serverChoice);
        return {
          playerAddress: addressTruncate(game.playerAddress, 5, 5, '***'),
          playerAddressLink: urlJoin(
            config.api.explorer.url,
            config.api.explorer.address,
            game.playerAddress,
          ),
          playerScore: game.playerScore,
          ratingPoints: game.ratingPoints,
          smartAddress: game.smartAddress,
          smartAddressLink: urlJoin(
            config.api.explorer.url,
            config.api.explorer.address,
            config.dapp.address,
          ),
          cleanWonAmount: game.cleanWonAmount.toFixed(2),
          bet: game.bet,
          thumbnailPlayer,
          thumbnailServer,
          serverWinner,
          playerChoice,
          serverChoice,
        };
      });
    },
  },
  methods: {
    ...mapActions({
      setGameInformation: 'setGameInformation',
      setPopupVisibility: 'setPopupVisibility',
    }),
    showPopup(game) {
      this.setGameInformation(game);
      this.setPopupVisibility({
        popup: Popups.GAME_INFORMATION,
        visible: true,
      });
    },
  },
};
</script>

<style scoped lang="scss">
  $td-border-radius: 0.2vw;

  .last-games {
    position: absolute;
    display: inline-flex;
    flex-direction: column;
    left: 3vw;
    width: 29vw;
    bottom: 2vw;

    table {
      width: 100%;
      font-weight: bold;
      font-family: Roboto, sans-serif;
      font-style: italic;
      border-collapse: collapse;
      border-spacing: 0;

      thead {
        text-align: center;
        font-size: 0.9vw;
        text-transform: capitalize;
        color: rgb(125, 202, 250);

        tr th {
          padding: 0.3vw 0.2vw;
        }
      }

      tbody {
        text-align: justify;
        font-size: 0.8vw;
        color: white;

        &:hover {
          cursor: pointer;
        }

        tr {
          transition: all 0.5s;

          td {
            line-height: 1.8;
            padding: 0.2vw 0.6vw;

            &.choice {
              text-align: center;
            }

            &.centered {
              text-align: center;
            }

            .thumbnail {
              height: 1.3vw;
              padding-top: 0.3vw;
            }
          }

          &:hover:not(.spacer) {
            transform: translate(0.5vw);
          }

          td:first-child:not(.spacer) {
            border-bottom-left-radius: $td-border-radius;
            border-top-left-radius: $td-border-radius;
          }

          td:last-child:not(.spacer) {
            border-bottom-right-radius: $td-border-radius;
            border-top-right-radius: $td-border-radius;
          }
        }

        tr.odd:not(.spacer) {
          background-color: rgba(27, 56, 112, 0.8);
          box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
        }

        tr:not(.spacer) {
          background-color: rgba(6, 29, 71, 0.8);
          box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
        }
      }
    }

    .caption {
      text-align: center;
      font-size: 1.25vw;
      margin-bottom: 1vw;
    }

    @media (min-aspect-ratio: 16/9) {
      left: calc((16 / 9) * 3vh);
      width: calc((16 / 9) * 29vh);
      bottom: calc((16 / 9) * 2vh);

      .caption {
        font-size: calc((16 / 9) * 1.25vh) !important;
        margin-bottom: calc((16 / 9) * 1vh) !important;
      }

      table {

        thead {
          font-size: calc((16 / 9) * 0.8vh);

          tr th {
            padding: calc((16 / 9) * 0.3vh) calc((16 / 9) * 0.2vh);
          }
        }

        tbody {
          text-align: justify;
          font-size: calc((16 / 9) * 0.75vh);

          tr {
            td {
              padding: calc((16 / 9) * 0.2vh) calc((16 / 9) * 0.6vh);

              .thumbnail {
                height: calc((16 / 9) * 1.3vh);
                padding-top: calc((16 / 9) * 0.3vh);
              }
            }

            td:first-child {
              border-bottom-left-radius: $td-border-radius;
              border-top-left-radius: $td-border-radius;
            }

            td:last-child {
              border-bottom-right-radius: $td-border-radius;
              border-top-right-radius: $td-border-radius;
            }
          }
        }
      }
    }
  }
</style>
