<template>
  <div :class="{'shadow-off': backgroundFaded && !isBetMultiplierSet}">
    <div class="flex row justify-content-center item-space">
      <p :class="captionStyle" class="text caption white centered">
        {{ $t("message.makeYourBetNow") }}
      </p>
    </div>
    <div class="flex row justify-content-center">
      <bet-multiplier-chooser :factor="betMultiplier.X1"></bet-multiplier-chooser>
      <bet-multiplier-chooser :factor="betMultiplier.X2"></bet-multiplier-chooser>
      <bet-multiplier-chooser :factor="betMultiplier.X4"></bet-multiplier-chooser>
      <bet-multiplier-chooser :factor="betMultiplier.X8"></bet-multiplier-chooser>
      <bet-multiplier-chooser :factor="betMultiplier.X12"></bet-multiplier-chooser>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import { BetMultiplier } from '@/models';
import BetMultiplierChooser from '@/components/controls/BetMultiplierChooser.vue';

export default {
  components: {
    BetMultiplierChooser,
  },
  computed: {
    ...mapGetters({
      currentBetMultiplier: 'betAmount',
      backgroundFaded: 'backgroundFaded',
      coinSie: 'coinSide',
    }),
    isBetMultiplierSet() {
      return this.currentBetMultiplier !== null;
    },
    betMultiplier() {
      return BetMultiplier;
    },
    captionStyle() {
      return ['caption text uppercase white glow', {
        'shadow-off': this.backgroundFaded && !this.isBetMultiplierSet,
        flicker: this.backgroundFaded && !this.isBetMultiplierSet,
      }];
    },
  },
  methods: {
    ...mapActions({
      setBackgroundFaded: 'setBackgroundFaded',
    }),
  },
};
</script>
