<template>
    <footer>
        <div class="play-buttons">
            <audio id="swap-sound" ref="swap-sound">
                <source id="swap-sound" src="/swap.mp3" type="audio/mp3" />
            </audio>
            <audio id="end-sound" ref="end-sound">
                <source id="end-sound" src="/end.mp3" type="audio/mp3" />
            </audio>
            <button @click="playSwapSound" class="play-button">↔️</button>
            <button @click="playEndSound" class="play-button">🏁</button>
        </div>
    </footer>
</template>

<script>
export default {
    methods: {
        playSwapSound() {
            document.getElementById("swap-sound").play();
        },
        playEndSound() {
            document.getElementById("end-sound").play();
        },
    },
};
</script>

<style>
footer {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: center;
    align-items: center;
}

.play-buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 1rem;
}

.play-button {
  margin-left: 1rem;
  font-size: 1.5rem;
  padding: 0.5rem;
  border-radius: 50%;
  color: #fff;
  border: none;
  cursor: pointer;
}
</style>
