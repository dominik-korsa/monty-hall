<template>
  <div class="game">
    <door
      :number="1"
      :selected="selected === 1"
      :right="right === 1"
      :flipped="wrongDismissed === 1 || !!finalSelected"
      :small="wrongDismissed === 1 ||  (finalSelected && finalSelected !== 1)"
      @click="select(1)" />
    <door
      :number="2"
      :selected="selected === 2"
      :right="right === 2"
      :flipped="wrongDismissed === 2  || !!finalSelected"
      :small="wrongDismissed === 2 || (finalSelected && finalSelected !== 2)"
      @click="select(2)" />
    <door
      :number="3"
      :selected="selected === 3"
      :right="right === 3"
      :flipped="wrongDismissed === 3  || !!finalSelected"
      :small="wrongDismissed === 3 || (finalSelected && finalSelected !== 3)"
      @click="select(3)" />
  </div>
</template>

<script>
import Door from '@/components/game/Door.vue';
export default {
  components: {
    Door,
  },
  data: () => ({
    disabled: false,
    selected: null,
    right: Math.floor(Math.random() * 3) + 1,
    wrongDismissed: null,
    finalSelected: null,
  }),
  methods: {
    reset() {
      this.selected = null;
      this.wrongDismissed = null;
      this.finalSelected = null;
      setTimeout(() => {
        this.right = Math.floor(Math.random() * 3) + 1;
        this.disabled = false;
      }, 1000);
    },
    select(number) {
      if (this.disabled) return;

      if (this.selected === null) {
        this.selected = number;
        this.disabled = true;

        setTimeout(this.dismiss, 1000);
      } else if (this.finalSelected === null) {
        if (number == this.wrongDismissed) return;
        this.disabled = true;
        this.finalSelected = number;

        setTimeout(this.reset, 3500);
      }
    },
    dismiss() {
      const doors = [1, 2, 3];
      doors.splice(doors.indexOf(this.selected), 1);
      if (this.selected !== this.right) doors.splice(doors.indexOf(this.right), 1);
      this.wrongDismissed = doors[Math.floor(doors.length * Math.random())];

      setTimeout(() => {
        this.disabled = false;
      }, 1000);
    }
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
