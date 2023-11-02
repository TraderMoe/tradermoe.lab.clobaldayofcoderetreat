<script setup>
import Timer from "./components/Timer.vue";
import PageFooter from "./components/PageFooter.vue";
import PageHeader from "./components/PageHeader.vue";
</script>

<template>
  <div>
    <PageHeader :title="'Global Day of Code Retreat'" />
    <Timer :pair-programming-duration="300" :session-duration="2700" />
    <PageFooter />
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
