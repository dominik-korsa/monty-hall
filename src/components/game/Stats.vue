<template>
  <div class="stats" :class="{ hidden }">

  </div>
</template>

<script>
export default {
  props: {
    hidden: {
      type: Boolean,
      default: true,
    },
  },
};
</script>

<style lang="scss" scoped>
  .stats {
    z-index: 1;
    transition: margin-top 750ms;
    height: 128px;
    width: 100%;
    background-color: #222;
    border-bottom: 1px solid white;
    box-sizing: border-box;

    &.hidden {
      margin-top: -128px;
    }
  }
</style>
