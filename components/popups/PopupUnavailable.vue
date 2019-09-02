<template>
  <popup
    :visible="visible"
    :closable="false"
    :title="$t('message.unavailable.title')"
    size="small"
  >
    <div
      class="text details flex column stretch">
      <div
        class="text details">
        <p>
          {{$t('message.unavailable.details')}}
        </p>
<!--        <p>-->
<!--          {{$t('message.unavailable.more')}}-->
<!--          <a @click="showPopupInformation" class="link">-->
<!--            {{$t('message.unavailable.link')}}-->
<!--          </a>.-->
<!--        </p>-->
      </div>
    </div>
  </popup>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import Popup from '@/components/popups/Popup.vue';
import { Popups } from '@/utils';

export default {
  components: {
    Popup,
  },
  computed: {
    ...mapGetters({
      popupsVisibility: 'popupsVisibility',
    }),
    visible() {
      return this.popupsVisibility[Popups.UNAVAILABLE];
    },
  },
  methods: {
    ...mapActions({
      setPopupVisibility: 'setPopupVisibility',
    }),
    hidePopup() {
      this.setPopupVisibility({
        popup: Popups.UNAVAILABLE,
        visible: false,
      });
    },
    showPopupInformation() {
      this.hidePopup();
      this.setPopupVisibility({
        popup: Popups.INFORMATION,
        visible: true,
      });
    },
  },
};
</script>

<style scoped lang="scss">
  .details {
    color: rgb(125, 202, 250);
    font-family: "Bahnschrift", sans-serif;
    text-transform: none;
    justify-content: space-evenly;
    padding: 0.5vw 1vw;

    @media (min-aspect-ratio: 16/9) {
      font-size: calc((16 / 9) * 1vh);
      padding: calc((16 / 9) * 0.5vh) calc((16 / 9) * 1vh);
    }

    .link {
      text-decoration: underline;
      cursor: pointer;
    }

    p {
      color: #7dcafa;
      font-size: 1vw;
      padding: 0.6vw 0.5vw;
      font-style: italic;

      @media (min-aspect-ratio: 16/9) {
        padding: calc((16 / 9) * 0.6vh) calc((16 / 9) * 0.5vh);
        font-size: calc((16 / 9) * 1vh);
      }
    }
  }
</style>
