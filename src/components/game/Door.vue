<template>
  <div
    class="door"
    @click="click"
    :style="`
      --gradient-color: ${animatedGradientColor};
      --selected-opacity: ${selectedGradientOpacity};`">
    <div
      class="card"
      :class="{
        selected: selected,
        flipped: flipped,
        right: right,
        small: small
      }">
      <div class="card__face card__face--front">{{ number }}</div>
      <div class="card__face card__face--back" v-if="right">
        100 000<br/>
        PLN
      </div>
      <div class="card__face card__face--back" v-else></div>
    </div>
  </div>
</template>

<script>
import { TweenLite } from 'gsap/TweenMax';

export default {
  name: 'door',
  data: () => ({
    tweenedGradientColor: {},
    selectedGradientOpacity: 0,
  }),
  props: {
    number: Number,
    selected: {
      type: Boolean,
      default: false,
    },
    flipped: {
      type: Boolean,
      default: false,
    },
    right: {
      type: Boolean,
      default: false,
    },
    small: {
      type: Boolean,
      default: false,
    },
    gradientColor: {
      type: Object,
      default: () => ({
        red: 0,
        green: 0,
        blue: 0,
        alpha: 0.25,
      }),
    },
  },
  methods: {
    click() {
      this.$emit('click');
    },
  },
  created() {
    this.tweenedGradientColor = Object.assign({}, this.gradientColor);
    this.selectedGradientOpacity = this.selected ? 1 : 0;
  },
  computed: {
    animatedGradientColor() {
      const {
        red,
        green,
        blue,
        alpha,
      } = this.tweenedGradientColor;
      return `rgba(${red}, ${green}, ${blue}, ${alpha})`;
    },
    animatedSelectedGradientOpacity() {
      return this.selectedGradientOpacity.toFixed(2);
    },
  },
  watch: {
    gradientColor(value) {
      TweenLite.to(this.$data.tweenedGradientColor, 1, value);
    },
    selected(value) {
      TweenLite.to(this.$data, 0.35, { selectedGradientOpacity: value ? 1 : 0 });
    },
  },
};
</script>

<style lang="scss" scoped>
  .door {
    perspective: 600px;
    margin: 0 16px;
    height: 100%;
    display: flex;
    align-items: center;
    transition: background 1s;
    background: linear-gradient(
      90deg,
      rgba(0,0,0,0) 0%,
      var(--gradient-color) 50%,
      rgba(0,0,0,0) 100%);
  }

  .card {
    width: 192px;
    height: 192px;
    position: relative;
    transition: transform 1s, width 1s, height 1s;
    transform-style: preserve-3d;
    cursor: pointer;
  }

  .card.small {
    width: 160px;
    height: 160px;

    &:not(.right) .card__face--back {
      background-color: #b71c1c;
    }

    &.right .card__face--back {
      background-color: #1b5e20;
      font-size: 24px;
    }
  }

  .card__face {
    position: absolute;
    height: 100%;
    width: 100%;
    box-sizing: border-box;
    backface-visibility: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: 'Roboto', sans-serif;
  }

  .card__face--front {
    background: #2196f3;
    box-shadow:
      0px 5px 5px -3px rgba(0, 0, 0, 0.2),
      0px 8px 10px 1px rgba(0, 0, 0, 0.14),
      0px 3px 14px 2px rgba(0, 0, 0, 0.12);
    border-radius: 4;
    border-radius: 8px;
    font-size: 64px;
    color: white;
    border: yellow solid;
    border-width: 0;
    transition: border-width 350ms;
  }

  .card.selected .card__face--front {
    border-width: 4px;
    background: #2196f3 radial-gradient(
      circle,
      rgba(255, 255, 0, calc( var(--selected-opacity) * 0.25 )) 0%,
      rgba(255, 255, 0, calc( var(--selected-opacity) * 0.75 )) 100%
    );
  }

  .card__face--back {
    background: #f44336;
    transform: rotateY(180deg);
    box-shadow:
      0px 5px 5px -3px rgba(0, 0, 0, 0.2),
      0px 8px 10px 1px rgba(0, 0, 0, 0.14),
      0px 3px 14px 2px rgba(0, 0, 0, 0.12);
    border-radius: 8px;
    transition: background-color 1000ms, font-size 1000ms;
  }

  .card.right .card__face--back {
    background: #4caf50;
    color: white;
    font-size: 32px;
    font-weight: 300;
    text-align: center;
  }

  .card.flipped {
    transform: rotateY(180deg);
  }
</style>
