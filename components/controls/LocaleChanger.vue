<template>
  <div class="change-lang text caption">
    <div
      class="lang current"
      @mouseover="mouseOverCurrent = true"
      @mouseleave="mouseOverCurrent = false"
    >
      <span class="flag">{{current.flag}}</span> {{current.name}} ▼
    </div>
    <transition name="fade">
      <div
        class="list"
        v-show="mouseOverList || mouseOverCurrent"
        @mouseover="mouseOverList = true"
        @mouseleave="mouseOverList = false"
      >
        <div
          v-for="loc in localesExceptCurrent"
          :key="loc.code"
          :class="['lang', {active: locale === loc.code}]"
          @click="changeLanguage(loc.code)"
        >
          <span class="flag">{{loc.flag}}</span> {{loc.name}}
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      locales: [{
        name: 'eng',
        code: 'en',
        flag: String.fromCodePoint(0x1F1EC, 0x1F1E7),
      }, {
        name: 'рус',
        code: 'ru',
        flag: String.fromCodePoint(0x1F1F7, 0x1F1FA),
      }],
      mouseOverList: false,
      mouseOverCurrent: false,
    };
  },
  computed: {
    current() {
      return this.locales.find(loc => loc.code === this.locale);
    },
    locale() {
      return this.$i18n.locale;
    },
    localesExceptCurrent() {
      return this.locales.filter(loc => loc.code !== this.locale);
    },
  },
  methods: {
    changeLanguage(locale) {
      this.$i18n.locale = locale;
      localStorage.setItem('language', locale);
    },
  },
};
</script>

<style scoped lang="scss">
  .change-lang {
    font-family: Roboto, sans-serif;
    font-style: italic;
    font-weight: bold;
    text-transform: uppercase;

    @media (min-aspect-ratio: 16/9) {
      margin: 1vh;

      .lang {
        font-size: 1vh !important;
        padding: calc((16 / 9) * 0.5vh) calc((16 / 9) * 1vh) !important;
      }

      .flag {
        bottom: calc((16 / 9) * -0.1vh);
        font-size: calc((16 / 9) * 0.7vh);
      }
    }

    .list {
      position: absolute;
      display: flex;
      flex-direction: column;
      background-color: rgba(0, 0, 0, 0.1);
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 10px;

      .lang:hover {
        background-color: rgba(0, 0, 0, 0.2);
        color: rgb(76, 138, 230);
      }
    }

    color: rgb(255, 255, 255);
    margin: calc((9 / 16) * 1vw);

    .lang {
      font-size: calc((9 / 16) * 1.0vw);
    }

    .lang {
      cursor: pointer;
      padding: 0.5vw 1vw;
    }

    .active {
      color: rgb(76, 138, 230);
    }

    .flag {
      bottom: -0.1vw;
      font-size: 0.7vw;
      position: relative;
    }
  }
</style>
