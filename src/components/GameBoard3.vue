<template>

  <div class="aside-panel">
    <pre>{{shuffledCards}}</pre>

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
    <Card3 v-for="(card, index) in shuffledCards" :card="card" :index="index" @click="handleCardClick(card.element, index)" />
  </div>
</template>

<script setup>
  import Card3 from './Card3.vue';
  import { shuffle } from 'lodash';
  import { ref } from 'vue';

  const playerName = ref(null);
  const shuffledCards = ref();
  const clickedCount = ref(0);
  const clickedCards = ref([]);
  const activeCards = ref([]);
  const visibleCards = ref([]);

  const gameBoard2 = [
    {
      id: 0,
      active: false,
      resolved: false,
      element: 'A',
    },
    {
      id: 1,
      active: false,
      resolved: false,
      element: 'B',
    },
    {
      id: 2,
      active: false,
      resolved: false,
      element: 'C',
    },
    {
      id: 3,
      resolved: false,
      active: false,
      element: 'D',
    },
    {
      id: 4,
      active: false,
      resolved: false,
      element: 'E',
    },
    {
      id: 5,
      active: false,
      resolved: false,
      element: 'F',
    },
    {
      id: 6,
      active: false,
      resolved: false,
      element: 'G',
    },
    {
      id: 7,
      active: false,
      resolved: false,
      element: 'H',
    },
  ]

  function startGame() {
    const doubledCards = [
      ...gameBoard2,
      ...gameBoard2,
    ]

    console.log(doubledCards)

    shuffledCards.value = doubledCards;
  }

  startGame()

  function handleCardClick(element, index) {
    increaseClick();
    shuffledCards.value[index].active = true;
    clickedCards.value.push(element);
    activeCards.value.push(index);
    if (clickedCount.value === 2) {
      compareCards();
    }
  }

  function compareCards() {
    if (clickedCards.value[0] === clickedCards.value[1]) {
      visibleCards.value.push(clickedCards.value[0]);
    }

    setTimeout(() => {
      activeCards.value = []
      clickedCards.value = []
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
