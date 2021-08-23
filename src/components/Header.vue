<template>
  <header>
    <h1>{{ title }}</h1>
    <a class="button" @click="$emit('toggle-add-task-panel', true)">Add Task</a>
  </header>
</template>

<script>
import Button from "./Button.vue";

export default {
  name: "Header",
  props: {
    title: {
      type: String,
      default: "Task Tracker",
    },
  },
  components: {
    Button,
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>