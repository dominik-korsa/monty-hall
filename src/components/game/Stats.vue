<template>
  <div class="stats" :class="{ hidden }" @click="onclick">
    <div class="card">
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    hidden: true,
  }),
  created() {
    window.addEventListener('keydown', this.onkey);
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.onkey);
  },
  methods: {
    onkey(event) {
      if (event.key === 's') this.hidden = !this.hidden;
    },
    onclick() {
      if (this.hidden) {
        this.hidden = false;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
  .stats {
    z-index: 1;
    transition: margin-top 750ms, margin-bottom 750ms, padding-bottom 750ms;
    width: 100%;
    padding-bottom: 24px;
    margin-bottom: -24px;

    &.hidden {
      margin-top: -128px;
      cursor: pointer;

      &:hover {
        margin-top: -104px;
        padding-bottom: 0;
        margin-bottom: 0;
      }
    }
  }

  .card {
    height: 128px;
    background-color: #222;
    border-bottom: 1px solid white;
    box-sizing: border-box;
  }
</style>
