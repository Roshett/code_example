<template>
  <div class="socials">
    <a
      @click.prevent="shareFacebook">
      <img src="../../assets/images/socials/ico-facebook.png" alt="">
    </a>
    <a
      @click.prevent="shareTwitter">
      <img src="../../assets/images/socials/ico-twitter.png" alt="">
    </a>
    <a
      @click.prevent="shareTelegram">
      <img src="../../assets/images/socials/ico-telegram.png" alt="">
    </a>
    <a
      @click.prevent="shareVkontakte">
      <img src="../../assets/images/socials/ico-vk.png" alt="">
    </a>
    <!--<a href="https://github.com/tradisys/fhloston-paradise" target="_blank">-->
    <!--<img src="../../assets/images/socials/ico-github.png" alt="">-->
    <!--</a>-->
  </div>
</template>

<script>
import urlJoin from 'url-join';

const objectToGetParams = object => `?${Object.keys(object)
  .filter(key => !!object[key])
  .map(key => `${key}=${encodeURIComponent(object[key])}`)
  .join('&')}`;

const url = 'https://www.coin-flip.io/';
const hashtags = [];

const shareFacebook = quote => urlJoin(
  'https://www.facebook.com/sharer/sharer.php',
  objectToGetParams({
    u: url,
    quote,
    hashtag: hashtags.map(t => `#${t}`)
      .join(),
  }),
);

const shareTwitter = quote => urlJoin(
  'https://twitter.com/share',
  objectToGetParams({
    url,
    text: quote,
    hashtags: hashtags.join(','),
  }),
);

const shareTelegram = quote => urlJoin(
  'https://telegram.me/share/',
  objectToGetParams({
    url,
    text: `${quote} ${hashtags.map(t => `#${t}`)
      .join()}`,
  }),
);

const shareVkontakte = quote => urlJoin(
  'https://vk.com/share.php',
  objectToGetParams({
    url,
    title: 'Coin Flip',
    description: `${quote} ${hashtags.join()}`,
  }),
);


export default {
  computed: {
    quote() {
      return this.$t('message.socials.quote');
    },
  },
  methods: {
    shareFacebook() {
      this.openDialog(shareFacebook(this.quote), 550, 400);
    },
    shareTwitter() {
      this.openDialog(shareTwitter(this.quote), 550, 400);
    },
    shareTelegram() {
      this.openDialog(shareTelegram(this.quote), 550, 400);
    },
    shareVkontakte() {
      this.openDialog(shareVkontakte(this.quote), 660, 460);
    },
    openDialog(openUrl, width, height) {
      const left = (window.outerWidth / 2)
        + (window.screenX || window.screenLeft || 0) - (width / 2);
      const top = (window.outerHeight / 2)
        + (window.screenY || window.screenTop || 0) - (height / 2);

      const config = {
        height,
        width,
        left,
        top,
        location: 'no',
        toolbar: 'no',
        status: 'no',
        directories: 'no',
        menubar: 'no',
        scrollbars: 'yes',
        resizable: 'no',
        centerscreen: 'yes',
        chrome: 'yes',
      };

      window.open(
        openUrl,
        '_blank',
        Object.keys(config)
          .map(key => `${key}=${config[key]}`)
          .join(', '),
      );
    },
  },
};
</script>

<style scoped lang="scss">
  .socials {
    position: absolute;
    bottom: 16vw;
    right: 1vw;
    display: inline-flex;
    flex-wrap: wrap;
    flex-direction: column;
    margin: 1vw;
    justify-content: flex-end;

    @media (min-aspect-ratio: 16/9) {
      margin: calc((16 / 9) * 1vh);
      bottom: calc((16 / 9) * 16vh);
      right: calc((16 / 9) * 1vh);

      a {
        img {
          width: calc((16 / 9) * 2vh) !important;
        }
      }
    }

    a {
      cursor: pointer;

      img {
        width: 2vw;
      }
    }

    a:hover {

      img {
        transform: scale(1.2);
      }
    }
  }
</style>
