<template>
  <div class="more-games flex column">
    <div class="text uppercase glow white">
      {{$t('message.moreGames')}}
    </div>
    <div class="flex column align-items-center placeholders">
      <div class="placeholder">
        <a target="_blank" href="https://www.dice-roller.io/">
          <img class="logo" src="../../assets/images/logo-dice-roller.png" alt="">
        </a>
      </div>
      <div class="placeholder">
        <a target="_blank" href="https://www.waves-rider.io/">
          <img class="logo" src="../../assets/images/logo-row.png" alt="">
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped lang="scss">
  .more-games {
    margin-top: 1vw;
    text-transform: none;
    display: inline-flex;

    @media (min-aspect-ratio: 16/9) {
      margin-top: calc((16 / 9) * 1vh);
      border-top-right-radius: calc((16 / 9) * 1vh);
      border-bottom-right-radius: calc((16 / 9) * 1vh);
    }

    .placeholders {
      padding: 0.5vw;
      align-self: start;
      opacity: 0.8;
      transition: opacity 0.3s ease-in-out;

      background-color: rgba(6, 29, 71, 0.8);
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
      border-top-right-radius: 1vw;
      border-bottom-right-radius: 1vw;
      color: rgb(125, 202, 250);

      .placeholder {
        margin: 0.2vw;
        padding: 0.5vw;
        border-radius: 0.5vw;
        background-color: rgba(33, 66, 120, 0.7);
        box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
        transition: opacity, transform 0.1s ease-in-out;

        @media (min-aspect-ratio: 16/9) {
          margin: calc((16 / 9) * 0.2vh);
          padding: calc((16 / 9) * 0.5vh);
          border-radius: calc((16 / 9) * 0.5vh);
        }

        &:hover {
          background-color: rgba(33, 66, 120, 1);
          cursor: pointer;
          transform: scale(1.1);
        }

        .logo {
          height: 2.7vw;

          @media (min-aspect-ratio: 16/9) {
            height: calc((16 / 9) * 2.7vh);
          }
        }
      }
    }

    .text {
      font-size: 0.7vw;
      padding-bottom: 0.5vw;
      padding-left: 0.5vw;
      text-align: center;

      @media (min-aspect-ratio: 16/9) {
        font-size: calc((16 / 9) * 0.7vh);
        padding-bottom: calc((16 / 9) * 0.5vh);
        padding-left: calc((16 / 9) * 0.5vh);
      }
    }


    &:hover {
      opacity: 1;
    }
  }
</style>
