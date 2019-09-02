<template>
  <div class="loading w-100 h-100">
    <div class="text caption lightblue">{{ $t('message.loading') }}...</div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped lang="scss">
  .loading {
    position: absolute;
    background-color: black;
    display: flex;
    justify-content: center;
    align-items: center;

    .text {
      text-transform: capitalize;
      font-size: 3vw;

      @media (min-aspect-ratio: 16/9) {
        font-size: calc((16 / 9) * 3vh);
      }
    }
  }
</style>
