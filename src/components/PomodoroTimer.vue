<template>
  <div class="pomodoro-timer">
    <h3>Pomodoro Timer</h3>
    
    <div class="timer-display">
      <div class="time">{{ formattedTime }}</div>
      <div class="session-type">{{ currentSession }}</div>
    </div>
    
    <div class="timer-controls">
      <button 
        @click="toggleTimer" 
        :class="['control-button', isRunning ? 'pause' : 'start']"
      >
        {{ isRunning ? 'Pause' : 'Start' }}
      </button>
      <button @click="resetTimer" class="control-button reset">
        Reset
      </button>
    </div>
    
    <div class="timer-settings">
      <div class="setting">
        <label>Work: {{ workMinutes }}min</label>
        <input 
          type="range" 
          v-model="workMinutes" 
          min="1" 
          max="60"
          :disabled="isRunning"
        />
      </div>
      <div class="setting">
        <label>Break: {{ breakMinutes }}min</label>
        <input 
          type="range" 
          v-model="breakMinutes" 
          min="1" 
          max="30"
          :disabled="isRunning"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onUnmounted } from 'vue'

// Reactive state with proper typing
const timeLeft = ref<number>(25 * 60) // 25 minutes in seconds
const isRunning = ref<boolean>(false)
const isWorkSession = ref<boolean>(true)
const workMinutes = ref<number>(25)
const breakMinutes = ref<number>(5)
let intervalId: number | null = null
// Computed properties
const formattedTime = computed((): string => {
  const minutes = Math.floor(timeLeft.value / 60)
  const seconds = timeLeft.value % 60
  return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
})

const currentSession = computed((): string => {
  return isWorkSession.value ? 'Work Session' : 'Break Time'
})

// Methods
const toggleTimer = (): void => {
  if (isRunning.value) {
    if (intervalId !== null) {
      clearInterval(intervalId)
      intervalId = null
    }
    isRunning.value = false
  } else {
    isRunning.value = true
    intervalId = window.setInterval(() => {
      if (timeLeft.value > 0) {
        timeLeft.value--
      } else {
        // Session completed
        if (intervalId !== null) {
          clearInterval(intervalId)
          intervalId = null
        }
        isRunning.value = false
        
        // Switch between work and break
        isWorkSession.value = !isWorkSession.value
        timeLeft.value = isWorkSession.value 
          ? workMinutes.value * 60 
          : breakMinutes.value * 60
          
        // Optional: Add notification sound here
        alert(isWorkSession.value ? 'Break finished! Time to work!' : 'Work session completed! Time for a break!')
      }
    }, 1000)
  }
}

const resetTimer = (): void => {
  if (intervalId !== null) {
    clearInterval(intervalId)
    intervalId = null
  }
  isRunning.value = false
  isWorkSession.value = true
  timeLeft.value = workMinutes.value * 60
}

// Cleanup interval on component unmount
onUnmounted(() => {
  if (intervalId !== null) {
    clearInterval(intervalId)
  }
})
</script>

<style scoped>
.pomodoro-timer h3 {
  color: #2c3e50;
  margin-bottom: 1.5rem;
  font-weight: 300;
  font-size: 1.5rem;
}

.timer-display {
  text-align: center;
  margin-bottom: 2rem;
}

.time {
  font-size: 4rem;
  font-weight: 300;
  color: #2c3e50;
  margin-bottom: 0.5rem;
  font-family: 'Courier New', monospace;
}

.session-type {
  font-size: 1.2rem;
  color: #34495e;
  opacity: 0.8;
}

.timer-controls {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 2rem;
}

.control-button {
  padding: 1rem 2rem;
  border: none;
  border-radius: 15px;
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  color: white;
  font-weight: 500;
}

.control-button.start {
  background: rgba(46, 204, 113, 0.8);
}

.control-button.start:hover {
  background: rgba(46, 204, 113, 1);
}

.control-button.pause {
  background: rgba(241, 196, 15, 0.8);
}

.control-button.pause:hover {
  background: rgba(241, 196, 15, 1);
}

.control-button.reset {
  background: rgba(52, 152, 219, 0.8);
}

.control-button.reset:hover {
  background: rgba(52, 152, 219, 1);
}

.timer-settings {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.setting {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.setting label {
  color: #2c3e50;
  font-size: 0.9rem;
}

.setting input[type="range"] {
  width: 100%;
  height: 6px;
  border-radius: 3px;
  background: rgba(255, 255, 255, 0.3);
  outline: none;
}

.setting input[type="range"]::-webkit-slider-thumb {
  appearance: none;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: rgba(52, 152, 219, 0.8);
  cursor: pointer;
}

.setting input[type="range"]:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>