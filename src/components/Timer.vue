<template>
  <div class="timer">
    <div class="progress-bar">
      <div class="progress" :style="{ width: progressWidth }"></div>
    </div>
    <div>
      <button @click="startTimer" :disabled="timer">Start</button>
      <button @click="stopTimer" :disabled="!timer">Stop</button>
      <audio id="swap-sound" ref="swap-sound">
        <source id="swap-sound" src="/swap.mp3" type="audio/mp3" />
      </audio>
      <audio id="end-sound" ref="end-sound">
        <source id="end-sound" src="/end.mp3" type="audio/mp3" />
      </audio>
      <button @click="playSwapSound" class="play-button">↔️</button>
      <button @click="playEndSound" class="play-button">🏁</button>
      <div class="message">{{ message }}</div>
    </div>
  </div>
</template>

<script>
const swapTimeDefault = 10; // 10 seconds

export default {
  props: {
    sessionDuration: {
      type: Number,
      required: true,
      default: 2700, // 45 minutes in seconds
    },
    pairProgrammingDuration: {
      type: Number,
      required: true,
      default: 300, // 5 minutes in seconds
    },
  },
  data() {
    return {
      timeLeft: this.sessionDuration,
      timer: null,
      swapTimer: null,
      isSwapping: false,
      message: " ",
      swapTime: swapTimeDefault,
    };
  },
  filters: {
    formatTime(time) {
      const minutes = Math.floor(time / 60);
      const seconds = time % 60;
      return `${minutes.toString().padStart(2, "0")}:${seconds
        .toString()
        .padStart(2, "0")}`;
    },
  },
  computed: {
    progressWidth() {
      const timeUntilNextSwap = this.timeLeft % this.pairProgrammingDuration;
      const timeUntilEndOfCurrentSwapInterval =
        this.pairProgrammingDuration - timeUntilNextSwap;
      return `${
        (timeUntilEndOfCurrentSwapInterval / this.pairProgrammingDuration) * 100
      }%`;
    },
  },
  methods: {
    startTimer() {
      if (!this.timer) {
        this.timer = setInterval(() => {
          if (this.timeLeft > 0) {
            this.timeLeft--;
            if (this.timeLeft === 0) {
              this.playEndSound();
              this.message = "Finish!";
            } else if (this.timeLeft % this.pairProgrammingDuration === 0) {
              this.playSwapSound();
              this.startSwapTimer();
              this.message = "Change!";
            } else {
              if (this.isSwapping) {
                this.message = "Change!";
              } else {
                this.message = " ";
              }
            }
          }
        }, 1000);
      }
    },
    stopTimer() {
      clearInterval(this.timer);
      clearInterval(this.swapTimer);
      this.timer = null;
      this.swapTimer = null;
      this.isSwapping = false;
      this.message = " ";
    },
    startSwapTimer() {
      this.isSwapping = true;
      this.swapTimer = setInterval(() => {
        if (this.swapTime > 0) {
          this.swapTime--;
        } else {
          clearInterval(this.swapTimer);
          this.isSwapping = false;
          this.swapTime = swapTimeDefault;
        }
      }, 1000);
    },
    playSwapSound() {
      document.getElementById("swap-sound").play();
    },
    playEndSound() {
      document.getElementById("end-sound").play();
    },
  },
  beforeDestroy() {
    clearInterval(this.timer);
    clearInterval(this.swapTimer);
  },
};
</script>

<style>
.timer {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  min-width: 500px;
}

.time {
  font-size: 5rem;
}

.message {
  font-size: 2rem;
  margin-top: 1rem;
  height: 3rem;
}

button {
  margin: 1rem;
  padding: 0.5rem 1rem;
  font-size: 1.5rem;
}

.progress-bar {
  width: 100%;
  height: 1rem;
  background-color: #ddd;
  margin-top: 1rem;
}

.progress {
  height: 100%;
  background-color: #1e5031;
  transition: width 1s linear;
}
</style>
