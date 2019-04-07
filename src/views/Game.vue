<template>
  <div class="game">
    <stats></stats>
    <div class="doors">
      <door
        :number="1"
        :selected="selected === 1"
        :right="right === 1"
        :flipped="wrongDismissed === 1 || !!finalSelected"
        :small="wrongDismissed === 1 ||  (finalSelected && finalSelected !== 1)"
        :gradient-color="getGradientColor(1)"
        @click="select(1)" />
      <door
        :number="2"
        :selected="selected === 2"
        :right="right === 2"
        :flipped="wrongDismissed === 2  || !!finalSelected"
        :small="wrongDismissed === 2 || (finalSelected && finalSelected !== 2)"
        :gradient-color="getGradientColor(2)"
        @click="select(2)" />
      <door
        :number="3"
        :selected="selected === 3"
        :right="right === 3"
        :flipped="wrongDismissed === 3  || !!finalSelected"
        :small="wrongDismissed === 3 || (finalSelected && finalSelected !== 3)"
        :gradient-color="getGradientColor(3)"
        @click="select(3)" />
    </div>
  </div>
</template>

<script>
import Door from '@/components/game/Door.vue';
import Stats from '@/components/game/Stats.vue';

export default {
  components: {
    Door,
    Stats,
  },
  data: () => ({
    disabled: false,
    selected: null,
    right: Math.floor(Math.random() * 3) + 1,
    wrongDismissed: null,
    finalSelected: null,
    statsHidden: true,
    stats: {
      switched: {
        wins: 0,
        loses: 0,
      },
      notSwitched: {
        wins: 0,
        loses: 0,
      },
    },
  }),
  computed: {
    switchedWinRate() {
      const { wins, loses } = this.stats.switched;
      return wins / (wins + loses);
    },
    notSwitchedWinRate() {
      const { wins, loses } = this.stats.notSwitched;
      return wins / (wins + loses);
    },
  },
  created() {
    window.addEventListener('keydown', this.onkey);
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.onkey);
  },
  methods: {
    onkey(event) {
      if (event.key === '1') this.select(1);
      else if (event.key === '2') this.select(2);
      else if (event.key === '3') this.select(3);
    },
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
        if (number === this.wrongDismissed) return;
        this.disabled = true;
        this.finalSelected = number;
        this.saveStat();

        setTimeout(this.reset, 3500);
      }
    },
    saveStat() {
      if (this.selected === this.finalSelected) {
        if (this.finalSelected === this.right) this.stats.notSwitched.wins += 1;
        else this.stats.notSwitched.loses += 1;
      } else if (this.finalSelected === this.right) this.stats.switched.wins += 1;
      else this.stats.switched.loses += 1;
    },
    dismiss() {
      const doors = [1, 2, 3];
      doors.splice(doors.indexOf(this.selected), 1);
      if (this.selected !== this.right) doors.splice(doors.indexOf(this.right), 1);
      this.wrongDismissed = doors[Math.floor(doors.length * Math.random())];

      setTimeout(() => {
        this.disabled = false;
      }, 1000);
    },
    getGradientColor(number) {
      if (this.finalSelected === number) {
        if (this.right === number) {
          return {
            red: 76,
            green: 175,
            blue: 80,
            alpha: 0.5,
          };
        }
        return {
          red: 244,
          green: 67,
          blue: 54,
          alpha: 0.5,
        };
      } if (this.selected === number) {
        if (this.finalSelected === null) {
          return {
            red: 255,
            green: 255,
            blue: 0,
            alpha: 0.25,
          };
        }
      } if (
        this.wrongDismissed === number
        || (this.finalSelected !== null && this.finalSelected === this.right)
      ) {
        return {
          red: 183,
          green: 28,
          blue: 28,
          alpha: 0.25,
        };
      } if (this.finalSelected !== null && this.finalSelected !== this.right) {
        return {
          red: 76,
          green: 175,
          blue: 80,
          alpha: 0.25,
        };
      }
      return {
        red: 33,
        green: 150,
        blue: 243,
        alpha: 0.25,
      };
    },
  },
};
</script>

<style lang="scss" scoped>
  .game {
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
  }

  .doors {
    display: flex;
    justify-content: center;
    flex-direction: row;
    background-color: #052454;
    flex-grow: 1;
  }
</style>
