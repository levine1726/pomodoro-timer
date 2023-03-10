<script setup lang="ts">
import { reactive, computed } from 'vue'

type minutes = number

interface Timer {
  minutesLeft: minutes
  secondsLeft: minutes
  isActive: boolean
}

interface PomodoroInfo {
  completedPomodoros: number
  desiredPomodoros: number
  isBreakActive: boolean
  pomodoroLength: minutes
  shortBreakLength: minutes
}

const timer: Timer = reactive({
  minutesLeft: 0,
  secondsLeft: 3,
  isActive: false
})

const pomodoroInfo: PomodoroInfo = reactive({
  completedPomodoros: 0,
  desiredPomodoros: 6,
  isBreakActive: false,
  pomodoroLength: 2,
  shortBreakLength: 1
})

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

function pauseTimer(): void {
  timer.isActive = false
}

function handleTimerEnd(): void {
  if (pomodoroInfo.isBreakActive) {
    console.log('Starting new pomodoro')
    pomodoroInfo.isBreakActive = false
    timer.minutesLeft = pomodoroInfo.pomodoroLength
  } else {
    console.log("You pomodoro'd!")
    pomodoroInfo.completedPomodoros++
    pomodoroInfo.isBreakActive = true
    timer.minutesLeft = pomodoroInfo.shortBreakLength
  }
}

let countdownHandler: Function = () => {
  if (!timer.isActive) {
    return
  }

  if (timer.minutesLeft == 0 && timer.secondsLeft == 0) {
    handleTimerEnd()
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
  <h2>Is Break Active: {{ pomodoroInfo.isBreakActive }}</h2>
  <h2>Completed Pomodoros: {{ pomodoroInfo.completedPomodoros }}</h2>
  <button @click="startTimer">Start Pomodoro!</button>
  <button @click="pauseTimer">Pause Timer</button>
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
