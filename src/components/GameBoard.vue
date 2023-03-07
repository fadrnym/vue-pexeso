<template>
  <div class="row mt-5">
    <div class="col-12 col-md-4">
      <div class="aside-panel border p-3 rounded">
        <div class="mb-4">
          <div class="form-group w-100 mb-3">
            <div class="input-group w-100">
              <div class="input-group-prepend">
                <div class="input-group-text">👦</div>
              </div>
              <input type="text" class="form-control" :disabled="playerIsSet" v-model="playerNameInput" @input="displaySaveBtn" placeholder="Jméno hráče">
            </div>
          </div>
          <div>
            <button v-if="showDisplayBtn && !playerIsSet" @click="savePlayer" class="btn btn-block btn-success">
              Uložit hráče
            </button>

            <button v-if="playerName.length && !gameWasStarted" @click="startGame" ref="startGameBtn" class="btn btn-block btn-outline-success">
              Začít hru
            </button>

            <button v-if="playerName.length" @click="resetPlayer" class="btn btn-block btn-outline-success">
              Zadat nového hráče
            </button>
          </div>
        </div>
        <div class="mb-4">
          <h3 class="mb-3">Hráč</h3>
          <ul v-if="playerName.length" class="list-group">
            <li class="list-group-item text-dark" v-for="(player, index) in playerName" :key="index">
              <span class="badge badge-pill badge-success mr-1"> {{ index + 1 }} </span> {{ player }}
            </li>
          </ul>
          <div v-else class="alert alert-warning">
            Hráč nezadal jméno
          </div>
        </div>

        <h3 class="mb-3">Správně otočené kartičky</h3>
        <ul v-if="visibleCards.length > 0" class="list-group flex-row flex-wrap">
          <li class="list-group-item m-1" v-for="(visibleCard, index) in visibleCards" :key="index">
            {{ visibleCard }}
          </li>
        </ul>
        <div v-else class="alert alert-warning">
          Prozatím zde nejsou žádné kartičky
        </div>
      </div>

      <div v-if="winner">
        <div class="alert alert-info text-center mt-3">{{ playerName[0] }} vyhrál/a hru!</div>
        <button class="btn btn-info btn-block" @click="restartGame">Hrát znovu?</button>
      </div>
    </div>
    <div class="col-12 col-md-8 d-flex justify-content-end">
      <div v-if="gameWasStarted" class="gameboard">
        <template v-for="(card, index) in shuffledCards" :key="index">
          <Card :cardElement="card" :visibleCards="visibleCards" :active="active" :index="index" @click="handleCardClick(card, index)" />
        </template>
      </div>

      <div v-else class="gameboard--empty rounded">
        <h2 class="green">Vyplňte jméno a zahajte hru</h2>
      </div>
    </div>
  </div>
</template>

<script setup>
  import Card from './Card.vue';
  import { shuffle } from 'lodash';
  import { ref } from 'vue';

  const shuffledCards = ref();
  const clickedCount = ref(0);
  const active = ref( []);
  const visibleCards = ref([]);
  const playerName = ref([]);
  const playerNameInput = ref();
  const gameWasStarted = ref(false);
  const showDisplayBtn = ref(false);
  const winner = ref(false);
  const playerIsSet = ref(false);
  const gameBoard = ['🍉','🍋','🍍','🍎','🥝','🥥','🍒','🍊'];

  const displaySaveBtn = () => {
    if (playerNameInput.value) {
      showDisplayBtn.value = true;
    }
  }

  const savePlayer = () => {
    playerName.value.push(playerNameInput.value);
    playerNameInput.value = '';
    playerIsSet.value = true;
  }

  const resetPlayer = () => {
    playerIsSet.value = false;
    playerName.value = [];
  }

  const startGame = () => {
    const doubledCards = gameBoard.concat(gameBoard);
    shuffledCards.value = shuffleCards(doubledCards);
    gameWasStarted.value = true;
  }

  const handleCardClick = (element, index) => {
    increaseClick();
    active.value.push({
      index: index,
      element: element,
    });
    if (clickedCount.value === 2) {
      compareCards();
    }

    isWinner();
  }

  const isWinner = () => {
    if (visibleCards.value.length === gameBoard.length) {
      winner.value = true;
    }
  }

  const restartGame = () => {
    winner.value = false;
    visibleCards.value = [];
    clickedCount.value = 0;
    active.value = [];
    shuffledCards.value = [];

    setTimeout(() => {
      startGame();
    }, 0)
  }

  const compareCards = () => {
    if (active.value[0].element === active.value[1].element) {
      visibleCards.value.push(active.value[0].element);
    }

    setTimeout(() => {
      active.value = []
    }, 500)

    clickedCount.value = 0;
  }

  const increaseClick = () => {
    clickedCount.value += 1;
  }

  const shuffleCards = (cards) => {
    return shuffle(cards);
  }
</script>

<style scoped>
  .gameboard,
  .gameboard--empty {
    display: grid;
    grid-template-columns: repeat(4, 9.3125rem);
    gap: .5rem;
    grid-auto-rows: 9.3125rem;
  }

  .gameboard--empty {
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid #dee2e6;
    width: 38.75rem;
    height: 38.75rem;
  }
</style>
