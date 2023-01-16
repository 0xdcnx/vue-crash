<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <!-- Passing tasks data as props to Tasks component with :tasks -->
  <!-- @delete-task in this level has access to the data I need to manipulate. So deleteTask() is called -->
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";

export default {
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },

    async deleteTask(id) {
      // Prompt for user confirmation before deleting a task
      if (confirm("Are you sure?")) {
        /** Tasks array will be equal to tasks with a filter:
         * for each task, only grab task with id !== @param id
         */
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });
        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error deleting task");
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };
      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });
      const data = await res.json();
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },

    /**
     * Grabs all tasks
     */
    async fetchTasks() {
      const res = await fetch("api/tasks");
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    },

    // Builtin function
    // Read more about created()
    // This function is called when components/App is created
    // Loads objects as array into tasks (defined in data() function above)
  },

  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>