<template>
  <div id="app">
    <Header @toggle-add-task-panel="onShowOrHideAddTaskPanel" />
    <AddTask
      v-if="showAddTaskPanel"
      @add-task="onAddTask"
      @toggle-add-task-panel="onShowOrHideAddTaskPanel"
    />
    <Tasks
      :tasks="tasks"
      @delete-task="onDeleteTask"
      @toggle-reminder="onToggleReminder"
    />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      showAddTaskPanel: false,
      tasks: [],
    };
  },
  created() {
    this.tasks = [
      {
        id: "3479aefafy91",
        title: "Clean bedroom",
        time: "周六 / 下午 2 点",
        reminder: true,
      },
      {
        id: "3479aefafy92",
        title: "Cook breakfast for girlfriend",
        time: "周日 / 中午 11 点",
        reminder: false,
      },
    ];
  },
  methods: {
    onShowOrHideAddTaskPanel(showOrHide) {
      this.showAddTaskPanel = showOrHide;
    },
    onAddTask(task) {
      this.tasks = [task, ...this.tasks];
    },
    onToggleReminder(id) {
      const task = this.tasks.find((task) => task.id === id);
      task.reminder = !task.reminder;
    },
    onDeleteTask(id) {
      if (confirm("Are you sure you want to delete this task?")) {
        this.tasks = this.tasks.filter((task) => {
          return task.id !== id;
        });
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Open+Sans&family=Poppins&display=swap");

#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  font-family: "Poppins", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 24px;
  padding: 16px;
  border: 1px solid rgba(0, 0, 0, 0.12);
  border-radius: 4px;
}

.button {
  font-size: 14px;
  color: rgba(255, 255, 255, 1);
  background-color: rgba(0, 0, 0, 0.87);
  padding: 8px 16px;
  border: none;
}

.button:hover {
  color: rgba(255, 255, 255, 0.6);
}
</style>
