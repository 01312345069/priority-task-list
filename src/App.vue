<script setup>
import { reactive, computed, onMounted, watch } from 'vue';

const tasks = reactive({
  High: [],
  Medium: [],
  Low: []
});

const newTask = reactive({
  name: '',
  priority: 'High'
});

onMounted(() => {
  const savedTasks = JSON.parse(localStorage.getItem('tasks'));
  if (savedTasks) {
    Object.keys(tasks).forEach(priority => {
      tasks[priority] = savedTasks[priority] || [];
    });
  }
});

watch(
  tasks,
  (newTasks) => {
    localStorage.setItem('tasks', JSON.stringify(newTasks));
  },
  { deep: true }
);

const addTask = () => {
  if (newTask.name.trim() !== '') {
    tasks[newTask.priority].push(newTask.name.trim());
    tasks[newTask.priority].sort();
    newTask.name = '';
  }
};
</script>

<template>
  <div class="app">
    <h1>Priority-Based Task List</h1>

    <form @submit.prevent="addTask" class="task-form">
      <input v-model="newTask.name" type="text" placeholder="Task Name" required />
      <select v-model="newTask.priority">
        <option value="High">High</option>
        <option value="Medium">Medium</option>
        <option value="Low">Low</option>
      </select>
      <button type="submit">Add Task</button>
    </form>

    <div class="task-list">
      <div v-for="priority in ['High', 'Medium', 'Low']" :key="priority" class="task-group">
        <h2>{{ priority }} Priority</h2>
        <ul>
          <li v-for="task in tasks[priority]" :key="task">{{ task }}</li>
        </ul>
        <p v-if="tasks[priority].length === 0">No tasks available in this category</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.app {
  max-width: 600px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
  color: black;
}

.task-form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.task-form input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.task-form select,
.task-form button {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.task-list {
  display: grid;
  gap: 20px;
}

.task-group {
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 4px;
  background: #f9f9f9;
}

.task-group h2 {
  margin-top: 0;
}

.task-group ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-group ul li {
  padding: 5px 0;
  border-bottom: 1px solid #ddd;
}

.task-group ul li:last-child {
  border-bottom: none;
}

.task-group p {
  margin: 0;
  color: #888;
  font-style: italic;
}
</style>
