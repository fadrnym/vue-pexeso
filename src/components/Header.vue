<template>
  <div class="greetings">
    <h1 class="green">
      {{ h1msg }}
    </h1>
    <h3>
      {{ h3msg }}
    </h3>
  </div>
</template>

<script setup>
  defineProps({
    h1msg: {
      type: String
    },
    h3msg: {
      type: String
    },
  })
</script>

<style scoped>
  h1 {
    font-weight: 500;
    font-size: 2.6rem;
    top: -.625rem;
  }

  h3 {
    font-size: 1.2rem;
  }
</style>
