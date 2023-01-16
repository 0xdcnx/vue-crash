<template>
  <!-- Iterating tasks prop which is an Array of objects -->
  <!-- :key (shorthand for v-bind:key) is required when using v-for  -->

  <div :key="task.id" v-for="task in tasks">
    <!-- For each iteration, we pass 'task' to component Task (wiht name 'task')-->

    <!-- @delete-task also emits up one level an event with the id of the task-->
    <!-- Here the id is the one received from the previous level, Task.vue -->
    <Task
      @toggle-reminder="$emit('toggle-reminder', task.id)"
      @delete-task="$emit('delete-task', task.id)"
      :task="task"
    />
  </div>
</template>

<script>
import Task from "./Task";

export default {
  name: "Tasks",
  props: {
    tasks: Array,
  },
  components: {
    Task,
  },
  /**
   * Because in this case a method is not being created to emit the id(like in Task.vue), I need to define emits (which is a array of events being emited). Otherwise, a non-emits warning is shown
   */
  emits: ["delete-task", "toggle-reminder"],
};
</script>   