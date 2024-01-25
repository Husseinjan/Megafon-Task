<template>
  <div class="p-4 bg-green-545">
    <div class="flex items-center justify-between">
      <h1 class="text-white text-[40px] items-center font-bold mb-4 font-bold">Список Задач</h1>
      <div class="flex center-items justify-between">
        <div
          class="flex items-center justify-center ml-2 w-10 h-10 bg-violet-700 rounded-full"
        ></div>
        <div
          class="flex items-center justify-center ml-2 w-10 h-10 bg-violet-700 rounded-full"
        ></div>
        <div class="flex items-center justify-center ml-2 w-10 h-10 bg-white rounded-full"></div>
      </div>
    </div>
    <div class="flex items-center justify-between">
      <button
        @click="createTask"
        class="bg-violet-700 text-white font-medium text-[20px] uppercase px-4 py-2 rounded-full shadow-lg border-2 border-violet-400 font-sans p-6 bg-opacity-97"
      >
        Создание задач
      </button>
      <div>
        <button @click="sortTasksByDate" class="ml-2 bg-violet-700 text-white px-2 py-1 rounded">
          Сортировать по дате
        </button>
        <button @click="sortTasksById" class="ml-2 bg-violet-700 text-white px-2 py-1 rounded">
          Сортировать по ID
        </button>
      </div>
    </div>

    <ul class="mt-4">
      <li
        v-for="task in sortedTasks"
        :key="task.id"
        class="my-2 p-2 border-2 bg-white rounded flex items-center justify-between"
      >
        <span class="text-black font-medium text-[20px]"
          >{{ task.title }} - {{ task.created }}</span
        >

        <div>
          <button
            @click="editTask(task)"
            class="ml-2 bg-violet-700 text-white text-[15px] px-2 py-1 rounded"
          >
            <i class="pi pi-pencil"></i>
          </button>
          <button
            @click="deleteTask(task.id)"
            class="ml-2 bg-red-500 text-white text-[15px] px-2 py-1 rounded"
          >
            <i class="pi pi-trash"></i>
          </button>
        </div>
      </li>
    </ul>

    <div
      v-if="isModalOpen"
      class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
    >
      <div class="bg-white p-4 rounded w-2/5">
        <h2 class="text-lg font-semibold text-[25px] mb-4 text-green-700">
          {{ editingTask ? 'Редактирование задачи' : 'Создание задачи' }}
        </h2>
        <label class="block mb-2">
          Заголовок:
          <input v-model="taskTitle" class="border p-2 rounded w-full border-green-600" />
        </label>
        <label class="block mb-4">
          Дата:
          <input
            type="date"
            v-model="taskDate"
            class="border-green-600 border p-2 rounded w-full"
          />
        </label>
        <div class="flex justify-end">
          <button @click="saveTask" class="bg-green-600 text-white px-4 py-2 rounded">
            Сохранить
          </button>
          <button @click="closeModal" class="ml-2 bg-gray-300 text-black px-4 py-2 rounded">
            Отмена
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tasks: [
        { id: 1, title: 'Задача 1', created: '2024-01-23' },
        { id: 2, title: 'Задача 2', created: '2024-01-25' }
      ],
      isModalOpen: false,
      editingTask: null,
      taskTitle: '',
      taskDate: ''
    }
  },
  computed: {
    sortedTasks() {
      return this.tasks.sort((a, b) => new Date(b.created) + new Date(a.created))
    }
  },
  methods: {
    createTask() {
      this.editingTask = null
      this.taskTitle = ''
      this.taskDate = ''
      this.isModalOpen = true
    },
    editTask(task) {
      this.editingTask = task
      this.taskTitle = task.title
      this.taskDate = task.created
      this.isModalOpen = true
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id)
    },
    saveTask() {
      if (this.editingTask) {
        this.editingTask.title = this.taskTitle
        this.editingTask.created = this.taskDate
      } else {
        const newTask = { id: this.tasks.length + 1, title: this.taskTitle, created: this.taskDate }
        this.tasks.push(newTask)
      }
      this.closeModal()
    },
    closeModal() {
      this.isModalOpen = false
    },
    sortTasksByDate() {
      this.tasks = this.tasks.sort((a, b) => new Date(b.created) - new Date(a.created))
    },

    sortTasksById() {
      this.tasks = this.tasks.sort((a, b) => a.id - b.id)
    }
  }
}
</script>
<style scoped>
.custom-font-width {
  font-weight: 900;
}
</style>
