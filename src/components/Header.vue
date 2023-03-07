<template>
  <div class="greetings">
    <h1 class="green">
      {{ mainTitle }}
    </h1>
    <h3>
      {{ subTitle }}
    </h3>
  </div>
</template>

<script setup>
  defineProps({
    mainTitle: {
      type: String,
      required: true
    },
    subTitle: {
      type: String,
      required: true
    },
  })
</script>

<style scoped>
  h1 {
    font-weight: 500;
    font-size: 2.6rem;
  }

  h3 {
    font-size: 1.2rem;
  }
</style>
