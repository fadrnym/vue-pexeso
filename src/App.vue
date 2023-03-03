<script>
  import HelloWorld from './components/HelloWorld.vue';
  import GameBoard2 from "./components/GameBoard2.vue";

  export default {
    components: {
      GameBoard2,
      HelloWorld,
    },
    setup() {
      const mainTitle = 'Vue pexeso';
      const subTitle = 'Projekt pro Quanti';

      return {
        mainTitle,
        subTitle
      }
    }
  }
</script>

<template>
<!--  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld :h1msg="mainTitle" :h3msg="subTitle" />
    </div>
  </header>-->

  <main>
    <GameBoard2 />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    justify-content: center;
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
