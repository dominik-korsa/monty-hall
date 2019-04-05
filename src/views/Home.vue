<template>
  <div class="home">
    <div class="links">
      <router-link to="/game" class="link game">Game</router-link>
      <router-link to="/panel" class="link panel">Control panel</router-link>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src

@Component({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {}
</script>

<style lang="scss" scoped>
  .links {
    display: flex;
    background-color: #263238;
    height: 100vh;
    width: 100vw;
    justify-content: center;
    align-items: center;
    flex-direction: column;

    .link {
      display: block;
      width: 336px;
      max-width: calc(90vw - 96px);
      background-color: #ff5722;
      margin: 24px 16px;
      font-family: 'Roboto', sans-serif;
      font-weight: 300;
      color: #fff;
      text-decoration: none;
      line-height: 64px;
      padding: 16px 24px;
      font-size: 32px;
      border-radius: 4px;
      text-align: center;
      box-shadow: 0px 5px 5px -3px rgba(0, 0, 0, 0.2), 0px 8px 10px 1px rgba(0, 0, 0, 0.14), 0px 3px 14px 2px rgba(0, 0, 0, 0.12);
      transition: background-color 250ms, box-shadow 250ms;

      &:hover {
        background-color: #d84315;
        box-shadow: 0px 11px 15px -7px rgba(0, 0, 0, 0.2), 0px 24px 38px 3px rgba(0, 0, 0, 0.14), 0px 9px 46px 8px rgba(0, 0, 0, 0.12);
      }
    }
  }
</style>
