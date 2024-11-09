<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <TaskForm @add-task="addNewTask" />
    <TaskList :tasks="tasks" @edit-task="editTask" @delete-task="deleteTask" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';

const title = 'To Do App';
const tasks = ref([]);

onMounted(async () => {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/todos');
    tasks.value = response.data;
  } catch (error) {
    console.error('Vazifalarni olishda xato:', error);
  }
});

const addNewTask = async (task) => {
  try {
    const response = await axios.post('https://jsonplaceholder.typicode.com/todos', task);
    tasks.value.push(response.data);
  } catch (error) {
    console.error('Vazifani qo\'shishda xato:', error);
  }
};

const editTask = async (task) => {
  try {
    const response = await axios.put(`https://jsonplaceholder.typicode.com/todos/${task.id}`, task);
    const index = tasks.value.findIndex(t => t.id === task.id);
    tasks.value[index] = response.data;
  } catch (error) {
    console.error('Vazifani tahrirlashda xato:', error);
  }
};

const deleteTask = async (taskId) => {
  try {
    await axios.delete(`https://jsonplaceholder.typicode.com/todos/${taskId}`);
    tasks.value = tasks.value.filter(task => task.id !== taskId);
  } catch (error) {
    console.error('Vazifani o\'chirishda xato:', error);
  }
};
</script>

