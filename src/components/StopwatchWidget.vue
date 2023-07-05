<template>
  <div class="stopwatch-widget">
    <h1>Stopwatch</h1>
    <div class="content">
      <div class="time">
        {{ formatTime(elapsedTime) }}
        <div v-if="isRunning" class="loader"></div>
      </div>
      <div class="buttons">
        <button @click="startStopwatch" :disabled="isRunning" class="start-button">Start</button>
        <button @click="stopStopwatch" :disabled="!isRunning || elapsedTime === 0" class="stop-button">Stop</button>
        <button @click="resetStopwatch" :disabled="isRunning || elapsedTime === 0" class="reset-button">Reset</button>
      </div>
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
.loader {
  width: 20px;
  height: 20px;
  margin-left: 5px;
  border-radius: 50%;
  border: 2px solid #fff;
  border-top-color: #1f79a1;
  animation: spin 0.8s infinite linear;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.stopwatch-widget {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.buttons {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.start-button {
  background-color: #1f79a1;
}

.stop-button {
  background-color: #e85a71;
}

.reset-button {
  background-color: #4caf50;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Transitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
