<template>
  <div class="popup-mask" @click="closePanel()">
    <div class="panel" @click.stop="">
      <form @submit="onSubmit">
        <div class="input-item">
          <div class="input-item-label">Task</div>
          <input
            type="text"
            placeholder="Describe your task"
            name="title"
            v-model="title"
          />
        </div>
        <div class="input-item">
          <div class="input-item-label">Time</div>
          <input
            type="text"
            placeholder="When to do this task"
            name="time"
            v-model="time"
          />
        </div>
        <div>
          <input type="checkbox" name="reminder" v-model="reminder" />
          Set as a reminder
        </div>
        <input type="submit" value="Save Task" class="button" />
      </form>
      <i
        class="btn-close fas fa-times"
        @click="$emit('toggle-add-task-panel', false)"
      ></i>
    </div>
  </div>
</template>

<script>
export default {
  props: {},
  data() {
    return {
      title: "",
      time: "",
      reminder: false,
    };
  },
  methods: {
    closePanel() {
      this.$emit("toggle-add-task-panel", false);
    },
    onTapPanel(e) {},
    onSubmit(e) {
      e.preventDefault();

      if (!this.title) {
        alert("Please describe your task");
        return;
      }
      if (!this.time) {
        alert("Please specify the time");
        return;
      }

      const task = {
        // id: Math.floor(Math.random() * 1000000),
        title: this.title,
        time: this.time,
        reminder: this.reminder,
      };
      this.$emit("add-task", task);

      this.title = "";
      this.time = "";
      this.reminder = false;

      this.closePanel();
    },
  },
};
</script>

<style scoped>
.popup-mask {
  position: absolute;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 10;
}

.panel {
  background-color: white;
  width: 280px;
  padding: 24px;
  position: relative;
}

.btn-close {
  position: absolute;
  top: 0;
  right: 0;
  padding: 24px;
}

form {
  text-align: left;
  margin-bottom: 16px;
}

.input-item {
  padding: 8px 0;
  width: 100%;
  display: flex;
  flex-direction: column;
}

.input-item-label {
  font-size: 14px;
  margin-bottom: 4px;
}

input[type="text"] {
  padding: 8px 12px;
}

.button {
  padding: 12px;
  width: 100%;
  margin-top: 12px;
}
</style>