<script setup>
import { ref } from 'vue';

const tasks = ref([
  { name: 'Task 1', priority: 'High' },
  { name: 'Task 2', priority: 'Medium' },
  { name: 'Task 3', priority: 'Low' }
]);
</script>
<template>
    <form @submit.prevent="addTask">
      <input v-model="newTaskName" placeholder="Task Name" required />
      <select v-model="newTaskPriority">
        <option value="High">High</option>
        <option value="Medium">Medium</option>
        <option value="Low">Low</option>
      </select>
      <button type="submit">Add Task</button>
    </form>
  </template>
  