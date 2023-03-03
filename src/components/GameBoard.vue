<template>

  <div class="aside-panel">

    <div class="form-inline">
      <div class="form-group mb-2">
        <label for="player">Zadejte jméno {{ playerName }}</label>
        <input type="text" class="form-control" id="player" v-model="playerName" placeholder="Jméno hráče">
      </div>

      <button @click="startGame" ref="startButton" class="btn btn-primary mb-2 aside-panel__start-btn">
        Potvrdit a začít hru
      </button>
    </div>
  </div>

  <div class="gameboard">
    <Card v-for="card in shuffledCards" :cardElement="card.element" :clickCount="clickedCount" :removeActiveClass="removeActiveClass" @clicked-card="clickHandler" @reset-count="resetCount" />
  </div>
</template>

<script setup>
  import Card from './Card.vue';
  import { shuffle } from 'lodash';
  import { ref } from 'vue';

  const playerName = ref(null);
  const shuffledCards = ref(null);
  const removeActiveClass = ref(false);
  const clickedCount = ref(0);
  const clickedCards = ref([]);

  const gameBoard = [
    {
      id: 0,
      element: 'A',
    },
    {
      id: 1,
      element: 'B',
    },
    {
      id: 2,
      element: 'C',
    },
    {
      id: 3,
      element: 'D',
    },
    {
      id: 4,
      element: 'E',
    },
    {
      id: 5,
      element: 'F',
    },
    {
      id: 6,
      element: 'G',
    },
    {
      id: 7,
      element: 'H',
    },
  ]

  function startGame() {
    const cards = [...gameBoard];
    const doubledCards = gameBoard.concat(cards);

    shuffledCards.value = shuffleCards(doubledCards);
  }

  function clickHandler(cardName) {
    increaseClick();

    clickedCards.value.push(cardName);

    if (clickedCount.value === 2) {
      compareCards(clickedCards);
    }

    console.log(clickedCards.value);
  }

  function compareCards(clickedCards) {
    console.log(removeActiveClass)
    if (clickedCards.value[0] === clickedCards.value[1]) {
      removeActiveClass.value = false;
      return true;
    }

    removeActiveClass.value = true;
  }

  function increaseClick() {
    clickedCount.value += 1;
  }

  function resetCount() {
    clickedCount.value = 0;
    removeActiveClass.value = false;
    clickedCards.value = [];
  }

  function shuffleCards(cards) {
    return shuffle(cards);
  }
</script>

<style scoped>
  .gameboard {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    margin-top: 2rem;
    gap: .5rem;
    border: 1px solid #dee2e6;
    grid-auto-rows: 9.3125rem;
    padding: .625rem;
  }
</style>
