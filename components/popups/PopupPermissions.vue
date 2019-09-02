<template>
  <popup
    :visible="visible"
    :closable="true"
    :title="$t('message.permissions.title')"
    @close="hidePopup"
  >
    <div
      style="text-transform: none; letter-spacing: 0.1em"
      class="text details centered flex stretch align-items-center justify-content-center">
      {{$t('message.permissions.details')}}
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
      return this.popupsVisibility[Popups.PERMISSIONS];
    },
  },
  methods: {
    ...mapActions({
      setPopupVisibility: 'setPopupVisibility',
    }),
    hidePopup() {
      this.setPopupVisibility({
        popup: Popups.PERMISSIONS,
        visible: false,
      });
    },
  },
};
</script>

<style scoped lang="scss">
  .details {
    padding: 3vw;
    color: rgb(125, 202, 250);
    font-family: "Bahnschrift", sans-serif;
    font-size: 1.13vw;

    @media (min-aspect-ratio: 16/9) {
      font-size: calc((16 / 9) * 1.13vh);
      padding: calc((16 / 9) * 3vh);
    }
  }
</style>
