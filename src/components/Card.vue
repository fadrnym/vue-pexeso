<template>
  {{visibleCards}}
  <div class="gameboard__card" @click="activateCard(cardElement)" :class="{'gameboard__card--active': activedCard}">
    <div class="gameboard__side gameboard__side--front">
      <div>PEXESO {{ cardElement }} {{removeActiveClass}}</div>
    </div>
    <div class="gameboard__side gameboard__side--back">
      <div>{{ cardElement }} {{removeActiveClass}}</div>
    </div>
  </div>
</template>

<script setup>
import {ref, toRefs, watch} from 'vue';

  const props = defineProps({
    visibleCards: () => [],
    cardElement: String,
    clickCount: Number,
    removeActiveClass: Boolean
  })

  const emit = defineEmits(['clickedCard', 'resetCount'])
  const activedCard = ref(false);

  const {removeActiveClass, visibleCards} = toRefs(props)

  watch(removeActiveClass, (newValue) => {
    checkState(newValue);
  });

  function checkState(newValue) {
    if (newValue) {
      setTimeout(() => {
        activedCard.value = false;
      }, 1500)
    }

    emit('resetCount', true);
  }

  function activateCard(cardElement) {
    if (props.clickCount < 2) {
      activedCard.value = true;
    }

    emit('clickedCard', cardElement);
  }
</script>

<style scoped>
  .gameboard__card {
    perspective: 150rem;
    position: relative;
    box-shadow: none;
    background: none;
  }

  .gameboard__side {
    transition: all 0.8s ease;
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

  .gameboard__side.gameboard__side--back {
    transform: rotateY(-180deg);
    background-color: #4158D0;
    background-image: linear-gradient(43deg, #4158D0 0%,#C850C0 46%, #FFCC70 100%);
  }

  .gameboard__side.gameboard__side--front {
    background-color: #0093E9;
    background-image: linear-gradient(160deg, #0093E9 0%, #80D0C7 100%);
  }

  .gameboard__card.gameboard__card--active .gameboard__side.gameboard__side--front {
    transform: rotateY(180deg);
  }

  .gameboard__card.gameboard__card--active .gameboard__side.gameboard__side--back {
    transform: rotateY(0deg);
  }
</style>
