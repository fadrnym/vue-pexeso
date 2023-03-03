<template>

  <div class="aside-panel">
    <div class="form-inline">
      <div class="form-group mb-2">
        <label for="player">Zadejte jméno {{ playerName }}</label>
        <input type="text" class="form-control" id="player" v-model="playerName" placeholder="Jméno hráče">
      </div>
      {{ visibleCards }}
      <button @click="startGame" ref="startButton" class="btn btn-primary mb-2 aside-panel__start-btn">
        Potvrdit a začít hru
      </button>
    </div>
  </div>
  <div class="gameboard">
    <template v-for="(card, index) in shuffledCards" :key="index">
      <Card2 :cardElement="card.element" :visibleCards="visibleCards" :active="active" :index="index" @click="handleCardClick(card.element, index)" />
    </template>
  </div>
</template>

<script setup>
  import Card2 from './Card2.vue';
  import { shuffle } from 'lodash';
  import { ref } from 'vue';

  const playerName = ref(null);
  const shuffledCards = ref();
  const clickedCount = ref(0);
  const active = ref( []);
  const visibleCards = ref([]);

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
    const doubledCards = gameBoard.concat(gameBoard);

    shuffledCards.value = shuffleCards(doubledCards);
  }

  startGame()

  function handleCardClick(element, index) {
    increaseClick();
    active.value.push({
      index: index,
      element: element,
    });
    if (clickedCount.value === 2) {
      compareCards();
    }
  }

  function compareCards() {
    if (active.value[0].element === active.value[1].element) {
      visibleCards.value.push(active.value[0].element);
    }

    setTimeout(() => {
      active.value = []
    }, 2000)

    clickedCount.value = 0;
  }

  function increaseClick() {
    clickedCount.value += 1;
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
