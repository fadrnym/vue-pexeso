<template>
  <div class="gameboard__card" :disabled="cardsAreSame" :class="{'gameboard__card--active': isActive, 'gameboard__card--same': cardsAreSame}">
    <div class="gameboard__side gameboard__side--front">
      <div class="gameboard__logo">
        {{ cardElement }}
        <img alt="Vue logo" class="logo" src="../assets/logo.svg" width="60" height="60" />
      </div>
    </div>
    <div class="gameboard__side gameboard__side--back">
      <div>{{ cardElement }}</div>
    </div>
  </div>
</template>

<script setup>
import { toRefs, computed } from 'vue';

  const props = defineProps({
    cardElement: String,
    index: Number,
    visibleCards: Array,
    active: Array,
  })

  const {cardElement, index, visibleCards, active} = toRefs(props);

  const isActive = computed(() => {
    if (visibleCards.value.includes(cardElement.value)) {
      return true;
    }
    return active.value.some(item => item.index === index.value);
  })

  const cardsAreSame = computed( () => {
    return visibleCards.value.length > 0 && visibleCards.value.includes(cardElement.value);
  })
</script>

<style scoped>
  .gameboard__card {
    perspective: 150rem;
    position: relative;
    cursor: pointer;
  }

  .gameboard__logo {
    background: #fff;
    border-radius: 999rem;
  }

  .gameboard__side {
    transition: all 0.4s ease;
    backface-visibility: hidden;
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    padding: 2rem;
    font-size: 1.5rem;
    font-weight: 700;
    color: #fff;
  }

  .gameboard__side--back {
    font-size: 3.5rem;
    transform: rotateY(-180deg);
    background-color: #4158D0;
    background-image: linear-gradient(43deg, #4158D0 0%,#C850C0 46%, #FFCC70 100%);
  }

  .gameboard__side--front {
    background-color: #0093E9;
    background-image: linear-gradient(160deg, #0093E9 0%, #80D0C7 100%);
  }

  .gameboard__card.gameboard__card--active .gameboard__side.gameboard__side--front {
    transform: rotateY(180deg);
  }

  .gameboard__card.gameboard__card--active .gameboard__side.gameboard__side--back {
    transform: rotateY(0deg);
  }

  .gameboard__card--same {
    opacity: .25;
    transition: opacity .5s;
  }

  .gameboard__card--active,
  .gameboard__card--same {
    pointer-events: none;
  }
</style>
