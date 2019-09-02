<template functional>
  <img class="background-cosmos block" src="../../assets/images/bg.jpg" alt="">
</template>

<script>
export default {};
</script>

<style lang="scss">
  .background-cosmos {
    position: relative;
    object-fit: cover;
    width: 103vw;
    height: calc((9 / 16) * 103vw);
  }

  @media (min-aspect-ratio: 16/9) {
    .background-cosmos {
      height: 103vh;
      width: calc((16 / 9) * 103vh);
    }
  }

  @keyframes brightness {
    0%   {
      filter: brightness(1);
    }
    50%  {
      filter: brightness(1.5);
    }
    100% {
      filter: brightness(1);
    }
  }
</style>
