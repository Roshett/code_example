<template>
  <div class="right-block row">
    <div class="flex column">

      <div class="space-between">
        <p class="text uppercase centred white glow header">
          {{ $t('message.sections.winningChance') }}
        </p>
        <p class="text uppercase centred yellow glow value">50.00%</p>
      </div>

      <div class="space-between">
        <p class="text uppercase centred white glow header">
          {{$t('message.sections.winningCoefficient')}}
        </p>
        <p class="text centred yellow glow value"><span class="x-pointer">x</span>1.90</p>
      </div>

      <div class="space-between">
        <p class="text uppercase centred white glow header">
          {{$t('message.sections.youMayWin')}}
        </p>
        <p class="text uppercase centred yellow glow value">
          {{winningAmountFormatted}}
        </p>
        <p class="text uppercase centred lightblue glow header">
          WAVES
        </p>
      </div>

    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
  computed: {
    ...mapGetters({
      winningAmount: 'winningAmount',
    }),
    winningAmountFormatted() {
      return this.winningAmount.toFixed(3)
        .slice(0, -1);
    },
  },
};
</script>

<style lang="scss" scoped>
  .right-block {
    padding-left: 1vw;
    min-width: 10vw;

    .x-pointer {
      font-size: 3vw;
      padding-right: 1vw;
    }

    .space-between {
      margin: 0 0 2vw 0;

      .header {
        font-weight: bold;
        font-size: 1.0vw;
        padding: 0.1vw 0;
      }

      .value {
        font-family: "ChicagoFlat", monospace;
        font-weight: normal;
        font-size: 3.6vw;
      }

      .subscription {
        font-size: 0.7vw;
        padding: 0.1vw 0;
      }
    }

    @media (min-aspect-ratio: 16/9) {
      padding-left: calc((16 / 9) * 1vh);
      min-width: calc((16 / 9) * 10vh);

      .x-pointer {
        font-size: calc((16 / 9) * 3vh);
        padding-right: calc((16 / 9) * 1vh);
      }

      .space-between {
        margin: 0 0 calc((16 / 9) * 2vh) 0;

        .header {
          font-size: calc((16 / 9) * 1.0vh);
          padding: calc((16 / 9) * 0.1vh) 0;
        }

        .value {
          font-size: calc((16 / 9) * 3.6vh);
        }

        .subscription {
          font-size: calc((16 / 9) * 0.7vh);
          padding: calc((16 / 9) * 0.1vh) 0;
        }
      }
    }
  }
</style>
