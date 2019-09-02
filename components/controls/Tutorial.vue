<template>
  <div
    class="flex column justify-content-center align-items-center w-100vw h-100vh container"
  >
    <div class="wh-100-aspect" style="position: relative">
      <div @click="skip" class="skip">{{$t('message.tutorial.skip')}}</div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import Driver from 'driver.js';

const options = {
  className: 'tutorial-popover',
  animate: true,
  opacity: 0.75,
  padding: 10,
  showButtons: false,
  allowClose: false,
  overlayClickNext: false,
  stageBackground: 'transparent',
};

export default {
  data() {
    return {
      driver: new Driver(options),
      stepsDone: 0,
      steps: [
        {
          element: '#tutorial-step-1',
          popover: {
            title: this.$t('message.tutorial.step-1.title'),
            position: 'left',
          },
        },
        {
          element: '#tutorial-step-2',
          popover: {
            title: this.$t('message.tutorial.step-2.title'),
            position: 'top-center',
          },
        },
        {
          element: '#tutorial-step-3',
          popover: {
            title: this.$t('message.tutorial.step-3.title'),
            position: 'top-center',
          },
        },
        {
          element: '#tutorial-step-4',
          popover: {
            title: this.$t('message.tutorial.step-4.title'),
            position: 'top',
          },
        },
        {
          element: '#tutorial-step-5',
          popover: {
            title: this.$t('message.tutorial.step-5.title'),
            position: 'top',
          },
        },
        {
          element: '#tutorial-step-6',
          popover: {
            title: this.$t('message.tutorial.step-6.title'),
            position: 'top',
          },
        },
      ],
    };
  },
  computed: {
    ...mapGetters({
      isTutorialActive: 'isTutorialActive',
    }),
  },
  methods: {
    ...mapActions({
      setTutorial: 'setTutorial',
    }),
    nextStep() {
      this.stepsDone += 1;
      const step = this.steps.shift();
      if (step) {
        this.driver.highlight(step);
      } else {
        this.closeTutorial();
      }
    },
    skip(event) {
      if (event.key === 'Escape' || event.type === 'click') {
        this.closeTutorial();
      }
    },
    closeTutorial() {
      window.removeEventListener('click', this.nextStep, true);
      window.removeEventListener('keydown', this.skip, false);
      this.driver.reset(true);
      this.setTutorial(false);
    },
  },
  mounted() {
    this.nextStep();
    window.addEventListener('click', this.nextStep, true);
    window.addEventListener('keydown', this.skip, false);
  },
};
</script>

<style lang="scss">
  .skip {
    display: inline-flex;
    color: white;
    z-index: 100000000000;
    font-family: Roboto, sans-serif;
    font-size: 1vw;
    position: absolute;
    margin: 3vw;
    padding: 0.5vw 2vw;
    right: 50%;
    transform: translate(50%, 0);
    bottom: 0;
    background-color: #1c6aff;
    border-radius: 10px;
    opacity: 0.9;

    &:hover {
      cursor: pointer;
      background-color: #3091ff;
    }

    @media (min-aspect-ratio: 16/9) {
      font-size: calc((16 / 9) * 1vh);
      margin: calc((16 / 9) * 3vh);
      padding: calc((16 / 9) * 0.5vh) calc((16 / 9) * 2vh);
    }
  }

  .help {
    position: absolute;
    right: 0;
    bottom: 0;
    width: 5vw;
    height: 2vw;

    @media (min-aspect-ratio: 16/9) {
    }
  }
</style>
