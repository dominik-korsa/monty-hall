<template>
  <div class="game">
    <door :number="1" />
    <door :number="2" />
    <door :number="3" />
  </div>
</template>

<script>
import Door from '@/components/game/Door.vue';
export default {
  components: {
    Door,
  }
};
</script>

<style lang="scss" scoped>
  .game {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    background-color: #0d47a1;
  }
</style>
