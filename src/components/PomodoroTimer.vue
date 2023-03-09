<script setup lang="ts">
import { reactive } from 'vue'

interface Timer {
  time: string
}

const timer: Timer = reactive({ time: '25:00' })
</script>

<template>
  <h1 class="greetings">Hello!</h1>
  <h2>{{ timer.time }}</h2>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
