<template functional>
  <div class="scene-planet">
    <img src="../../assets/images/first-plane.png" alt="">
  </div>
</template>

<script>
export default {
};
</script>

<style lang="scss" scoped>
  .scene-planet {
    position: absolute;
    right: 0;
    bottom: 0;

    img {
      position: absolute;
      right: 0;
      bottom: 0;
      width: 60vw;
      height: calc((9 / 16) * 60vw);
    }
  }

  @media (min-aspect-ratio: 16/9) {
    img {
      height: 100vh;
      width: calc((16 / 9) * 100vh);
    }
  }
</style>
