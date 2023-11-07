<script setup>
import Timer from "./components/Timer.vue";
import PageHeader from "./components/PageHeader.vue";
import BackgroundGame from "./components/BackgroundGame.vue";
</script>

<template>
  <div>
    <PageHeader :title="'Global Day of Code Retreat'" />
    <Timer :pair-programming-duration="300" :session-duration="2700" />
    <BackgroundGame :board-size="50" />
  </div>
</template>
