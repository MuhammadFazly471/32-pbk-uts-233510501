<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const newTask = ref('')
const filtered = ref("all")

const addTask = () => {
  tasks.value.push({
    id: tasks.value.length + 1,
    title: newTask.value,
    completed: false
  })
  newTask.value = ''
}

const toggleTask = (task) => {
  task.completed == !task.completed
  console.log(task.completed)
}

const filteredTasks = computed(() => {
  if (filtered.value === 'completed') {
    return tasks.value.filter(t => t.completed)
  }
  if (filtered.value === 'incomplete') {
    return tasks.value.filter(t => !t.completed)
  }
  return tasks.value
})

</script>

<template>
  <div>
    <input type="text" v-model="newTask" @keyup.enter="addTask">
    <button @click="addTask">Add Task</button>
  </div>

  <select v-model="filtered">
    <option value="all">All</option>
    <option value="completed">Complete</option>
    <option value="incomplete">incomplete</option>
  </select>

  <div>
    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" @change="toggleTask(task)" />
        {{ task.title }}
      </li>
    </ul>
  </div>

</template>

<style scoped></style>
