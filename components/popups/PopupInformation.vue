<template>
  <popup
    :visible="visible"
    :closable="true"
    @close="hidePopup"
  >
    <div class="container scrollbar-custom">
      <div style="text-align: justify; text-transform: none;" class="text caption white">
        <p class="title">{{$t('message.information.about.title')}}</p>
        <div class="description">
          <p>{{$t('message.information.about.text.p-1')}}</p>
          <p>{{$t('message.information.about.text.p-2')}}</p>
          <p>{{$t('message.information.about.text.p-3')}}</p>
        </div>
        <p class="title">{{$t('message.information.toPlayYouNeed.title')}}</p>
        <ol class="description">
          <li>{{$t('message.information.toPlayYouNeed.li-1')}}</li>
          <li>{{$t('message.information.toPlayYouNeed.li-2')}}</li>
          <li>{{$t('message.information.toPlayYouNeed.li-3')}}</li>
        </ol>
        <p class="title">{{$t('message.information.gameInstructions.title')}}</p>
        <ol class="description">
          <li>
            {{$t('message.information.gameInstructions.li-1')}}
          </li>
          <li>
            {{$t('message.information.gameInstructions.li-2')}}
          </li>
          <li>
            {{$t('message.information.gameInstructions.li-3')}}
          </li>
          <li>
            {{$t('message.information.gameInstructions.li-4')}}
          </li>
          <li>
            {{$t('message.information.gameInstructions.li-5')}}
          </li>
          <li>
            {{$t('message.information.gameInstructions.li-6')}}
          </li>
        </ol>
        <p class="description">{{$t('message.information.goodLuck')}}</p>
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
      return this.popupsVisibility[Popups.INFORMATION];
    },
  },
  methods: {
    ...mapActions({
      setPopupVisibility: 'setPopupVisibility',
    }),
    hidePopup() {
      this.setPopupVisibility({
        popup: Popups.INFORMATION,
        visible: false,
      });
    },
  },
};
</script>

<style scoped lang="scss">
  .container {
    display: flex;
    overflow-y: auto;
    font-family: Roboto, sans-serif;

    p.title {
      color: white;
      font-size: 1.25vw;

      font-family: "Bahnschrift", sans-serif;
    }

    p.title, p.description {
      padding: 0.5vw 0;
    }

    ol {
      margin: 0;
    }

    .description {
      font-style: italic;
    }

    .text {
      color: #7dcafa;
      font-size: 0.9vw;
      padding: 0 0.5vw;
    }

    @media (min-aspect-ratio: 16/9) {
      p.title, p.description {
        padding: calc((16 / 9) * 0.5vh) 0 !important;
      }

      p.title {
        font-size: calc((16 / 9) * 1.25vh) !important;
      }

      .text {
        font-size: calc((16 / 9) * 0.9vh) !important;
        padding: 0 calc((16 / 9) * 0.5vh) !important;
      }
    }
  }
</style>
