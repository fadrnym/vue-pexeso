<template>
  <header class="header">
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <Header :mainTitle="mainTitle" :subTitle="subTitle" />
    </div>
  </header>

  <main>
    <GameBoard />
  </main>
</template>

<script setup>
  import Header from './components/Header.vue';
  import GameBoard from "./components/GameBoard.vue";

  const mainTitle = 'Vue pexeso';
  const subTitle = 'Ukázkový projekt';
</script>

<style scoped>
  .header {
    display: flex;
    place-items: center;
    justify-content: center;
  }

  .logo {
    display: block;
    margin: 0 2rem 0 0;
  }

  .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
</style>
