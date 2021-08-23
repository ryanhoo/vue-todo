<template>
  <div
    :class="['task', task.reminder ? 'reminder' : '']"
    @dblclick="$emit('toggle-reminder', task.id)"
  >
    <div class="title">{{ task.title }}</div>
    <div class="time">{{ task.time }}</div>
    <i class="fas fa-times" @click="$emit('delete-task', task.id)"></i>
  </div>
</template>

<script>
export default {
  props: {
    task: Object,
  },
};
</script>

<style scoped>
.task {
  background-color: rgba(0, 0, 0, 0.06);
  padding: 12px 16px;
  margin-bottom: 12px;
  position: relative;
  border-left: 4px solid transparent;
}

.reminder {
  border-left: 4px solid rgba(0, 191, 108, 0.42);
}

.title {
  font-size: 22px;
  color: rgba(0, 0, 0, 0.87);
  margin-bottom: 8px;
}

.time {
  font-size: 16px;
  color: rgba(0, 0, 0, 0.24);
}

.task .fas {
  position: absolute;
  padding: 16px;
  right: 0;
  top: 0;
  color: red;
}
</style>