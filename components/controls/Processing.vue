<template>
  <div class="processing text uppercase caption white flicker centered">
    {{ $t('message.processing') }}
  </div>
</template>

<script>
export default {
};
</script>

<style scoped lang="scss">
  .processing {
    margin-top: 3vw;
    font-size: 5vw;

    @media (min-aspect-ratio: 16/9) {
      margin-top: calc((16 / 9) * 3vh);
    }
  }
</style>
