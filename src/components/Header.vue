<template>
  <header>
    <!-- We use title props inside h3 tag -->
    <h1>{{ title }}</h1>
    <!-- Using Button component with props 'text' and 'color' -->
    <Button
      v-show="homePage"
      @btn-click="$emit('toggle-add-task')"
      :text="showAddTask ? 'Close' : 'Add Task'"
      :color="showAddTask ? 'red' : 'green'"
    />
  </header>
</template>

<script>
// Importing Components
import Button from "./Button";

export default {
  name: "Header",
  props: {
    title: String,
    showAddTask: Boolean,
  },
  // Registering Components
  components: {
    Button,
  },
  computed: {
    homePage() {
      if (this.$route.path === "/") {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: spaced space-between;
  align-items: right;
  margin-bottom: 20px;
}
</style>