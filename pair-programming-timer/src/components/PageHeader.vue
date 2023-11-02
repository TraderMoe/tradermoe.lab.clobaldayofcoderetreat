<template>
  <header>
    <h1>{{ title }}</h1>
  </header>
</template>

<script>
export default {
  name: "PageHeader",
  props: {
    title: {
      type: String,
      required: true,
    },
  },
};
</script>

<style scoped>
header {
  padding: 1rem;
  text-align: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
