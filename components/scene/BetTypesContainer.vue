<template>
  <div :class="{'shadow-off': backgroundFaded && !isBetTypeSet}">
    <div style="flex: 1;" class="flex row justify-content-center">
      <p :class="captionStyle" class="text caption white centered">
        {{ $t("message.chooseSide") }}
      </p>
    </div>
    <div style="flex: 1;" class="flex row justify-content-center">
      <bet-type-chooser :type="betType.HEADS"></bet-type-chooser>
      <bet-type-chooser :type="betType.TAILS"></bet-type-chooser>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import { BetType } from '@/models';
import BetTypeChooser from '@/components/controls/BetTypeChooser.vue';

export default {
  components: {
    BetTypeChooser,
  },
  computed: {
    ...mapGetters({
      currentBetType: 'coinSide',
      backgroundFaded: 'backgroundFaded',
      betAmount: 'betAmount',
    }),
    isBetTypeSet() {
      return this.currentBetType !== null;
    },
    betType() {
      return BetType;
    },
    captionStyle() {
      return ['caption text uppercase white glow', {
        'shadow-off': this.backgroundFaded && !this.isBetTypeSet,
        flicker: this.backgroundFaded && !this.isBetTypeSet,
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
