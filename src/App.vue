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
    <Footer />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
    Footer,
  },
  data() {
    return {
      showAddTaskPanel: false,
      tasks: [],
    };
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
  methods: {
    onShowOrHideAddTaskPanel(showOrHide) {
      this.showAddTaskPanel = showOrHide;
    },
    async onToggleReminder(id) {
      const task = await this.fetchTask(id);
      const updateToTask = { ...task, reminder: !task.reminder };

      const resp = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updateToTask),
      });
      console.log(await resp.json(), resp.status);
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: !task.reminder } : task
      );
    },
    async fetchTasks() {
      const resp = await fetch(`api/tasks`);
      const data = await resp.json();
      return data;
    },
    async fetchTask(id) {
      const resp = await fetch(`api/tasks/${id}`);
      const data = await resp.json();
      return data;
    },
    async onAddTask(task) {
      const resp = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await resp.json();
      this.tasks = [...this.tasks, data];
    },
    async onDeleteTask(id) {
      if (confirm("Are you sure you want to delete this task?")) {
        const resp = await fetch(`api/tasks/${id}`, {
          method: "delete",
        });
        if (resp.status === 200) {
          this.tasks = this.tasks.filter((task) => {
            return task.id !== id;
          });
        } else {
          alert("Delete task failed!");
        }
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
