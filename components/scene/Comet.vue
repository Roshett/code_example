<template>
  <transition
    appear
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
    v-bind:css="false"
  >
    <img v-show="show" class="comet" :src="model" alt="">
  </transition>
</template>

<script>
import Velocity from 'velocity-animate';
import CometModel1 from '@/assets/images/comet-1.png';
import CometModel2 from '@/assets/images/comet-2.png';
import CometModel3 from '@/assets/images/comet-3.png';
import CometModel4 from '@/assets/images/comet-4.png';

const cometModels = [
  CometModel1,
  CometModel2,
  CometModel3,
  CometModel4,
];

export default {
  props: {
    variant: Number,
  },
  data() {
    return {
      show: false,
      model: cometModels[this.variant],
    };
  },
  mounted() {
    this.show = true;
  },
  methods: {
    beforeEnter(el) {
      const { style } = el;
      const [r, t] = [Math.floor(Math.random() * 20) + 10, Math.floor(Math.random() * 20) + 10];
      style.right = `-${r}%`;
      style.top = `-${t}%`;
    },
    enter(el, done) {
      const [r, t] = [Math.floor(Math.random() * 30) + 100, Math.floor(Math.random() * 30) + 100];
      Velocity(el, { duration: Math.floor(Math.random() * 10000) });
      Velocity(el, {
        opacity: 1,
        right: `${r}%`,
        top: `${t}%`,
      }, { duration: Math.floor(Math.random() * 3000) + 20000 });
      Velocity(el, {
        complete: () => {
          done();
          this.show = false;
        },
      });
    },
    leave(el, done) {
      Velocity(el, {
        complete: () => {
          done();
          this.show = true;
        },
      });
    },
  },
};
</script>

<style scoped lang="scss">
  .comet {
    position: absolute;
  }
</style>
