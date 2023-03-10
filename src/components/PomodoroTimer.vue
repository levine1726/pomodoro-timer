<script setup lang="ts">
import { reactive, computed } from 'vue'

interface Timer {
  minutesLeft: number
  secondsLeft: number
  isActive: boolean
}

interface PomodoroInfo {
  completedPomodoros: number
  desiredPomodoros: number
}

const timer: Timer = reactive({ minutesLeft: 0, secondsLeft: 3, isActive: false })
const pomodoroInfo: PomodoroInfo = reactive({ completedPomodoros: 0, desiredPomodoros: 6 })

const timeLeft = computed<string>(() => {
  let minutesText: string = `${timer.minutesLeft}`
  let secondsText: string = `${timer.secondsLeft}`

  if (timer.minutesLeft < 10) {
    minutesText = '0' + minutesText
  }

  if (timer.secondsLeft < 10) {
    secondsText = '0' + secondsText
  }

  return `${minutesText}:${secondsText}`
})

function startTimer(): void {
  timer.isActive = true
}

let countdownHandler: Function = () => {
  if (!timer.isActive) {
    return
  }

  if (timer.minutesLeft == 0 && timer.secondsLeft == 0) {
    alert("You pomodoro'd!")
    timer.isActive = false
    pomodoroInfo.completedPomodoros++
    return
  }

  if (timer.secondsLeft == 0) {
    timer.secondsLeft = 60
    timer.minutesLeft--
  }

  timer.secondsLeft--
}

setInterval(countdownHandler, 1000)
</script>

<template>
  <h1 class="greetings">Ready to Pomo Those Doro's?</h1>
  <h2>{{ timeLeft }}</h2>
  <h2>Is Active: {{ timer.isActive }}</h2>
  <h2>Completed Pomodoros: {{ pomodoroInfo.completedPomodoros }}</h2>
  <button @click="startTimer">Start Pomodoro!</button>
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
