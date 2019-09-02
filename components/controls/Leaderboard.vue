<template>
  <div>
    <div class="top flex row">
      <div class="best">
        <div class="pace-1 flex column justify-content-center align-items-center">
          <template v-if="statsLeadersFirst">
            <div class="address">
              <a target="_blank" :href="statsLeadersFirst.playerAddressLink">
                {{statsLeadersFirst.playerAddress}}
              </a>
            </div>
            <div class="data">
              {{$t('message.leaderboard.best.score')}}:
              {{statsLeadersFirst.score}}
            </div>
            <div class="data">
              {{$t('message.leaderboard.best.timePlayed')}}:
              {{statsLeadersFirst.timePlayed}}
            </div>
            <div class="data">
              {{$t('message.leaderboard.best.won')}}:
              {{statsLeadersFirst.cleanWonAmount}}
              <img class="wave" src="../../assets/images/wave-small-white.png" alt="">
            </div>
          </template>
        </div>
        <div class="pace-2 flex column justify-content-center align-items-center">
          <template v-if="statsLeadersSecond">
            <div class="address">
              <a target="_blank" :href="statsLeadersSecond.playerAddressLink">
                {{statsLeadersSecond.playerAddress}}
              </a>
            </div>
            <div class="data">
              {{$t('message.leaderboard.best.score')}}:
              {{statsLeadersSecond.score}}
            </div>
            <div class="data">
              {{$t('message.leaderboard.best.timePlayed')}}:
              {{statsLeadersSecond.timePlayed}}
            </div>
            <div class="data">
              {{$t('message.leaderboard.best.won')}}:
              {{statsLeadersSecond.cleanWonAmount}}
              <img class="wave" src="../../assets/images/wave-small-white.png" alt="">
            </div>
          </template>
        </div>
        <div class="pace-3 flex column justify-content-center align-items-center">
          <template v-if="statsLeadersThird">
            <div class="address">
              <a target="_blank" :href="statsLeadersThird.playerAddressLink">
                {{statsLeadersThird.playerAddress}}
              </a>
            </div>
            <div class="data">
              {{$t('message.leaderboard.best.score')}}:
              {{statsLeadersThird.score}}
            </div>
            <div class="data">
              {{$t('message.leaderboard.best.timePlayed')}}:
              {{statsLeadersThird.timePlayed}}
            </div>
            <div class="data">
              {{$t('message.leaderboard.best.won')}}:
              {{statsLeadersThird.cleanWonAmount}}
              <img class="wave" src="../../assets/images/wave-small-white.png" alt="">
            </div>
          </template>
        </div>
      </div>
      <div class="total flex column justify-content-center stretch align-items-start">
        <div class="element title">
          {{$t('message.leaderboard.total.title')}}
        </div>
        <div>
          <span class="element">
            {{$t('message.leaderboard.total.totalPlayers')}}:
          </span>
          <span class="value">
            {{statsTotalFormatted.totalPlayers}}
          </span>
        </div>
        <div>
          <span class="element">
            {{$t('message.leaderboard.total.timesPlayed')}}:
          </span>
          <span class="value">
            {{statsTotalFormatted.timesPlayed}}
          </span>
        </div>
        <div>
          <span class="element">
            {{$t('message.leaderboard.total.totalCleanWonAmount')}}:
          </span>
          <span class="value">
            {{statsTotalFormatted.totalCleanWonAmount}}
            <img class="wave" src="../../assets/images/wave-small.png" alt="">
          </span>
        </div>
      </div>
    </div>
    <div class="bottom scrollbar-custom">
      <table>
        <thead>
        <tr>
          <th>{{$t('message.leaderboard.other.rank')}}</th>
          <th>{{$t('message.leaderboard.other.player')}}</th>
          <th>{{$t('message.leaderboard.other.score')}}</th>
          <th>{{$t('message.leaderboard.other.timesPlayed')}}</th>
          <th>{{$t('message.leaderboard.other.won')}}</th>
          <th>{{$t('message.leaderboard.other.winRate')}}</th>
        </tr>
        </thead>
        <tbody>
        <template v-for="(row, id) in statsLeadersOther">
          <tr :key="`${row.playerAddress}-${id}-row`" :class="['player', {active: row.active}]">
            <td class="rank">{{row.rank}}</td>
            <td class="address">
              <a target="_blank" :href="row.playerAddressLink">
                {{row.playerAddress}}
              </a>
            </td>
            <td class="data">
              {{row.score}}
            </td>
            <td class="data">
              {{row.timePlayed}}
            </td>
            <td class="data">
              {{row.cleanWonAmount}}
              <img class="wave" src="../../assets/images/wave-small.png" alt="">
            </td>
            <td class="data">
              {{row.winRate}}%
            </td>
          </tr>
        </template>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import config from '@/config';
import urlJoin from 'url-join';
import {
  formatNumber,
  addressTruncate,
  getElementAtOr,
} from '@/utils';

export default {
  props: {
    general: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    ...mapGetters({
      playerAddress: 'playerAddress',
      statsCurrentTotal: 'statsCurrentTotal',
      statsGeneralTotal: 'statsGeneralTotal',
      statsCurrentLeaderboard: 'statsCurrentLeaderboard',
      statsGeneralLeaderboard: 'statsGeneralLeaderboard',
    }),
    statsTotal() {
      if (this.general) {
        return this.statsGeneralTotal;
      }

      return this.statsCurrentTotal;
    },
    statsLeaderboard() {
      if (this.general) {
        return this.statsGeneralLeaderboard;
      }

      return this.statsCurrentLeaderboard;
    },
    statsTopLeaders() {
      const arr = Array.from(this.statsLeaderboard)
        .slice(0, 3);
      if (arr.length > 0) {
        return arr.map(leader => ({
          rank: leader.rank,
          score: leader.score,
          timePlayed: formatNumber(leader.timePlayed),
          winRate: leader.winRate,
          cleanWonAmount: formatNumber(leader.cleanWonAmount),
          playerAddress: addressTruncate(leader.playerAddress, 4, 4, '***'),
          playerAddressLink: urlJoin(
            config.api.explorer.url,
            config.api.explorer.address,
            leader.playerAddress,
          ),
        }));
      }

      return [];
    },
    statsTotalFormatted() {
      return {
        totalPlayers: formatNumber(this.statsTotal.totalPlayers),
        totalCleanWonAmount: formatNumber(this.statsTotal.totalCleanWonAmount),
        timesPlayed: formatNumber(this.statsTotal.timesPlayed),
      };
    },
    statsLeadersOther() {
      return Array.from(this.statsLeaderboard)
        .slice(3)
        .map(player => ({
          ...player,
          playerAddressLink: urlJoin(
            config.api.explorer.url,
            config.api.explorer.address,
            player.playerAddress,
          ),
          active: this.playerAddress === player.playerAddress || false,
          cleanWonAmount: formatNumber(player.cleanWonAmount),
        }));
    },
    statsLeadersFirst() {
      return getElementAtOr(this.statsTopLeaders, 0, null);
    },
    statsLeadersSecond() {
      return getElementAtOr(this.statsTopLeaders, 1, null);
    },
    statsLeadersThird() {
      return getElementAtOr(this.statsTopLeaders, 2, null);
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
    font-family: "Serpentin", sans-serif;
    transition: opacity 0.5s ease-in-out;

    a:hover {
      color: rgb(125, 202, 250);
    }

    .top {
      .best {
        position: relative;
        width: 28.75vw;
        height: 13.75vw;
        display: block;
        background: url("../../assets/images/leaders-top.png") no-repeat center;
        background-size: contain;
        margin-bottom: -0.5vw;

        .pace-1 {
          position: absolute;
          width: 30%;
          top: 40%;
          left: 35%;
        }

        .pace-2 {
          position: absolute;
          top: 50%;
          left: 5%;
          width: 28%;
        }

        .pace-3 {
          position: absolute;
          top: 53%;
          left: 67%;
          width: 28%;
        }

        .address {
          letter-spacing: 0.05vw;
          font-size: 0.8vw;
          margin-bottom: 0.4vw;
        }

        .data {
          font-family: Roboto, sans-serif;
          font-style: italic;
          font-size: 0.8vw;
        }
      }

      .total {
        font-size: 0.9vw;
        letter-spacing: 0.03vw;

        div {
          padding: 0.3vw 0;
        }

        span {
          padding: 0.2vw;
        }

        .element {
          text-transform: capitalize;
          color: rgb(125, 202, 250);

          &.title {
            color: white;
            font-size: 1.25vw;
            margin-bottom: 0.3vw;
          }
        }
      }
    }

    .bottom {
      overflow-y: auto;
      height: 13.5vw;

      table {
        width: 100%;
        font-weight: bold;
        font-family: Roboto, sans-serif;
        font-style: italic;
        border-collapse: collapse;

        thead {
          text-align: center;
          font-size: 0.9vw;
          color: rgb(125, 202, 250);
        }

        tbody {
          text-align: center;
          font-size: 0.8vw;

          tr {
            &.active {
              background-color: #78d1ff;

              a:hover {
                color: cornflowerblue;
              }
            }

            td {
              padding: 0.4vw 0;
              text-align: center;
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

          tr:nth-child(odd):not(.active) {
            background-color: rgba(125, 202, 250, 0.3);
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
          }
        }
      }
    }

    @media (min-aspect-ratio: 16/9) {
      .top {
        .best {
          width: calc((16 / 9) * 28.75vh) !important;
          height: calc((16 / 9) * 13.75vh) !important;
          margin-bottom: calc((16 / 9) * -0.5vh) !important;

          .address {
            font-size: calc((16 / 9) * 0.8vh) !important;
            letter-spacing: calc((16 / 9) * 0.05vh) !important;
            margin-bottom: calc((16 / 9) * 0.4vh) !important;
          }

          .data {
            font-size: calc((16 / 9) * 0.8vh) !important;
          }
        }

        .total {
          font-size: calc((16 / 9) * 0.9vh) !important;
          letter-spacing: calc((16 / 9) * 0.03vh) !important;

          div {
            padding: calc((16 / 9) * 0.3vh) 0 !important;
          }

          span {
            padding: calc((16 / 9) * 0.2vh) !important;
          }

          .element {
            color: rgb(125, 202, 250);

            &.title {
              font-size: calc((16 / 9) * 1.25vh) !important;
              margin-bottom: calc((16 / 9) * 0.3vh) !important;
            }
          }
        }
      }

      .bottom {
        height: calc((16 / 9) * 13.5vh);

        table {
          thead {
            font-size: calc((16 / 9) * 0.9vh) !important;
            color: rgb(125, 202, 250);
          }

          tbody {
            font-size: calc((16 / 9) * 0.8vh) !important;

            tr {
              td {
                padding: calc((16 / 9) * 0.4vh) 0;
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
  }
</style>
