<template>
  <div class="door">
    <div class="card" :class="{ selected: selected, flipped: flipped, right: right, small: small }">
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
export default {
  name: 'door',
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
  }
}
</script>

<style lang="scss" scoped>
  .door {
    perspective: 600px;
    margin: 16px;
  }

  .card {
    width: 192px;
    height: 192px;
    position: relative;
    transition: transform 1s, width 1s, height 1s;
    transform-style: preserve-3d;
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
    transition: border-width 250ms;
  }

  .card.selected .card__face--front {
    border-width: 4px;
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
