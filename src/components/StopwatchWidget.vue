<template>
  <div class="stopwatch" style="margin-top: 100px;">
    <h1>Stopwatch</h1>
    <div class="time" :class="{ 'countdown': isRunning && elapsedTime > 0 }">
      {{ formatTime(elapsedTime) }}
      <div v-if="isRunning" class="loader"></div>
    </div>
    <div class="buttons">
      <button class="start-button" @click="startStopwatch" :disabled="isRunning">
        <span v-if="!isRunning">Start</span>
        <span v-else>Resume</span>
      </button>
      <button class="stop-button" @click="stopStopwatch" :disabled="!isRunning">Stop</button>
      <button class="reset-button" @click="resetStopwatch">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Stopwatch',
  data() {
    return {
      isRunning: false,
      startTime: null,
      elapsedTime: 0,
      timerInterval: null,
    };
  },
  methods: {
    startStopwatch() {
      if (!this.isRunning) {
        this.isRunning = true;
        this.startTime = Date.now() - this.elapsedTime;
        this.timerInterval = setInterval(() => {
          this.elapsedTime = Date.now() - this.startTime;
        }, 10);
      }
    },
    stopStopwatch() {
      if (this.isRunning) {
        this.isRunning = false;
        clearInterval(this.timerInterval);
      }
    },
    resetStopwatch() {
      this.isRunning = false;
      this.startTime = null;
      this.elapsedTime = 0;
      clearInterval(this.timerInterval);
    },
    formatTime(milliseconds) {
      const minutes = Math.floor(milliseconds / 60000);
      const seconds = Math.floor((milliseconds % 60000) / 1000);
      const centiseconds = Math.floor((milliseconds % 1000) / 10);
      return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}.${centiseconds
        .toString()
        .padStart(2, '0')}`;
    },
  },
};
</script>

<style>
.stopwatch {
  text-align: center;
}

h1 {
  font-size: 32px;
  font-weight: bold;
}

.time {
  font-size: 72px;
  font-weight: bold;
  margin: 20px 0;
  transition: color 0.3s ease;
  position: relative;
}

.countdown {
  color: #dc3545;
}

.loader {
  position: absolute;
  top: calc(50% - 200px);
  left: calc(50% - 200px);
  width: 400px;
  height: 400px;
  border-radius: 50%;
  border-top: 4px solid #dc3545;
  border-right: 4px solid transparent;
  animation: loaderAnimation 1s linear infinite;
}

@keyframes loaderAnimation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.buttons {
  margin-top: 20px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  border: none;
  outline: none;
  transition: background-color 0.3s ease;
}

.start-button {
  background-color: #28a745;
  color: #fff;
}

.start-button:disabled {
  background-color: #73c6b6;
  cursor: not-allowed;
}

.start-button:hover:not(:disabled) {
  background-color: #218838;
}

.stop-button {
  background-color: #dc3545;
  color: #fff;
}

.stop-button:disabled {
  background-color: #e6b3a1;
  cursor: not-allowed;
}

.stop-button:hover:not(:disabled) {
  background-color: #c82333;
}

.reset-button {
  background-color: #ffc107;
  color: #212529;
}

.reset-button:hover {
  background-color: #e0a800;
}
</style>
