<script setup>
import { ref } from 'vue'

const tasks = ref([])
const newTask = ref('')

const addTask = () => {
  tasks.value.push({
    id: tasks.value.length + 1,
    title: newTask.value,
    completed: false
  })
  newTask.value = ''
}

</script>

<template>
  <div>
    <input type="text" v-model="newTask" @keyup.enter="addTask">
    <button @click="addTask">Add Task</button>
  </div>

  <div>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        {{ task.title }}
      </li>
    </ul>
  </div>

</template>

<style scoped>

</style>
