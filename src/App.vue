<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const newTask = ref('')
const filtered = ref("all")

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      id: tasks.value.length + 1,
      title: newTask.value,
      completed: false
    })
    newTask.value = ''
  }
}

const toggleTask = (task) => {
  task.completed == !task.completed
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

const deleteTask = (task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
}
</script>

<template>
  <div class="flex items-center justify-center w-screen h-screen bg-gradient-to-br from-purple-100 to-indigo-200 p-4">
    <div class="w-full max-w-md bg-white rounded-xl shadow-xl overflow-hidden flex flex-col h-full max-h-full">
      <!-- Header -->
      <div class="bg-gradient-to-r from-violet-500 to-fuchsia-500 px-6 py-4">
        <h1 class="text-2xl font-bold text-white text-center">To-Do List</h1>
      </div>

      <div class="p-6 flex flex-col flex-1">
        <!-- Input dan tombol tambah (fixed size) -->
        <div class="flex mb-4 space-x-2">
          <input
            type="text"
            v-model="newTask"
            @keyup.enter="addTask"
            class="flex-1 px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-violet-400 transition-all"
            placeholder="Tambahkan tugas baru..."
          />
          <button
            @click="addTask"
            class="bg-violet-500 hover:bg-violet-600 text-white px-4 py-2 rounded-lg transition-colors duration-300"
          >
            Tambah
          </button>
        </div>

        <!-- Filter (fixed size) -->
        <div class="mb-4">
          <select
            v-model="filtered"
            class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-violet-400 transition-all bg-gray-50"
          >
            <option value="all">Semua Tugas</option>
            <option value="completed">Selesai</option>
            <option value="incomplete">Belum Selesai</option>
          </select>
        </div>

        <!-- Daftar task dengan scroll (flexible size) -->
        <div class="flex-1 overflow-hidden flex flex-col">
          <div class="overflow-y-auto flex-1">
            <ul class="space-y-2 pr-2">
              <li
                v-for="task in filteredTasks"
                :key="task.id"
                class="flex items-center justify-between p-3 rounded-lg transition-all duration-300 bg-gray-50 hover:bg-gray-100"
                :class="[task.completed ? 'border-l-4 border-green-500' : 'border-l-4 border-orange-500']"
              >
                <div class="flex items-center space-x-3">
                  <input
                    type="checkbox"
                    v-model="task.completed"
                    @change="toggleTask(task)"
                    class="w-5 h-5 rounded text-violet-500 focus:ring-violet-400 transition-colors"
                  />
                  <span :class="{'line-through text-gray-400': task.completed, 'text-gray-700': !task.completed}">
                    {{ task.title }}
                  </span>
                </div>
                <button
                  @click="deleteTask(task)"
                  class="text-gray-400 hover:text-red-500 transition-colors p-1 rounded-lg"
                >
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
                  </svg>
                </button>
              </li>
              <li v-if="filteredTasks.length === 0" class="text-center py-4 text-gray-500 italic">
                Tidak ada tugas yang ditampilkan
              </li>
            </ul>
          </div>
        </div>

        <!-- Counter task (fixed size) -->
        <div class="mt-4 text-sm text-gray-500 flex justify-between">
          <div>Total: {{ tasks.length }} tugas</div>
          <div>Selesai: {{ tasks.filter(t => t.completed).length }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Animasi untuk task baru */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

li {
  animation: fadeIn 0.3s ease-out;
}

/* Animasi untuk hover pada checkbox */
input[type="checkbox"] {
  cursor: pointer;
  transition: all 0.2s;
}

input[type="checkbox"]:hover {
  transform: scale(1.1);
}

/* Animasi untuk tombol delete */
button:active {
  transform: scale(0.95);
}
</style>