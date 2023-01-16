<template>
  <!-- v-bind:class adds class .reminder if condition before '?' is true
    else, does not add class ('')  -->
  <!-- Since we always want to add class .task, we can add it after the comma -->
  <div
    @dblclick="$emit('toggle-reminder', task.id)"
    :class="[task.reminder ? 'reminder' : '', 'task']"
  >
    <!-- task.reminder is a value inside task Object props -->
    <h3>
      {{ task.text }}
      <!-- The @click is a v-bind that fires when a click event is triggered -->
      <!-- Here it's calling the method onDelete(id) with the task id as the param -->
      <!-- Since this component does not have direct access to the data we want to delete, the function simply emits the task id -->
      <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods: {
    /**
     * onDelete takes the id of the clicked task, and emits it to the next level of depth - Tasks component
     * The final goal is to emit upwards into App.vue
     * The name 'delete-task' is custom named
     * @param {*} id
     */
    // onDelete(id) {
    //   this.$emit("delete-task", id);
    // },
  },
};
</script>

<style scoped>
.fas {
  color: red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>