<template>
  <div class="p-4 bg-green-545">
    <div class="flex items-center justify-between">
      <h1 class="text-white text-[40px] items-center font-bold mb-4 font-extrabold">Список Задач</h1>
      <div class="flex center-items justify-between"> 
        <div class="flex items-center justify-center ml-2  w-10 h-10 bg-violet-700 rounded-full"> </div>
        <div class="flex items-center justify-center ml-2 w-10 h-10 bg-violet-700 rounded-full"> </div>
        <div class="flex items-center justify-center ml-2 w-10 h-10 bg-white rounded-full"> </div>
      </div>
    </div>
    <button
      @click="createTask"
      class="bg-violet-700 text-white font-extrabold text-[20px] uppercase px-4 py-2 rounded-full"
    >
      Создание задач
    </button>

    <ul class="mt-4">
      <li v-for="task in sortedTasks" :key="task.id" class="my-2 p-2 border rounded">
        <span>{{ task.title }} - {{ task.created }}</span>
        <button @click="editTask(task)" class="ml-2 bg-yellow-500 text-white px-2 py-1 rounded">
          Редактировать
        </button>
        <button @click="deleteTask(task.id)" class="ml-2 bg-red-500 text-white px-2 py-1 rounded">
          Удалить
        </button>
      </li>
    </ul>

    <div
      v-if="isModalOpen"
      class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
    >
      <div class="bg-white p-4 rounded w-2/5">
        <h2 class="text-lg font-semibold mb-4">{{ editingTask ? 'Edit Task' : 'Create Task' }}</h2>
        <label class="block mb-2">
          Заголовок:
          <input v-model="taskTitle" class="border p-2 rounded w-full" />
        </label>
        <label class="block mb-4">
          Дата:
          <input type="date" v-model="taskDate" class="border p-2 rounded w-full" />
        </label>
        <div class="flex justify-end">
          <button @click="saveTask" class="bg-blue-500 text-white px-4 py-2 rounded">
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
        { id: 1, title: 'Task 1', created: '2024-01-24' },
        { id: 2, title: 'Task 2', created: '2024-01-25' }
      ],
      isModalOpen: false,
      editingTask: null,
      taskTitle: '',
      taskDate: ''
    }
  },
  computed: {
    sortedTasks() {
      return this.tasks.sort((a, b) => new Date(b.created) - new Date(a.created))
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
    }
  }
}
</script>
<style scoped>
.custom-font-width {
  font-weight: 900;
}
</style>
