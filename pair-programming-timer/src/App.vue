<script setup>
import Timer from "./components/Timer.vue";
import PageFooter from "./components/PageFooter.vue";
import PageHeader from "./components/PageHeader.vue";
import BackgroundGame from "./components/BackgroundGame.vue";
</script>

<template>
  <div>
    <PageHeader :title="'Global Day of Code Retreat'" />
    <Timer :pair-programming-duration="300" :session-duration="2700" />
    <BackgroundGame :height="50" />
    <PageFooter />
  </div>
</template>
