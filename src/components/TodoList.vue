
<template>
  <div class="p-4">
    <h1 class="text-2xl font-bold mb-4">Todo List</h1>
    <button @click="createTask" class="bg-blue-500 text-white px-4 py-2 rounded">
      Create Task
    </button>

    <ul class="mt-4">
      <li
        v-for="task in sortedTasks"
        :key="task.id"
        class="mb-2 p-2 border border-gray-300 rounded"
      >
        {{ task.title }} - {{ task.createdAt }}
        <button @click="editTask(task)" class="ml-2 bg-yellow-500 text-white px-2 py-1 rounded">
          Edit
        </button>
        <button @click="deleteTask(task.id)" class="ml-2 bg-red-500 text-white px-2 py-1 rounded">
          Delete
        </button>
      </li>
    </ul>

    <div
      v-if="isModalOpen"
      class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
    >
      <div class="bg-white p-4 rounded w-1/3">
        <h2 class="text-lg font-bold mb-4">{{ editingTask ? 'Edit Task' : 'Create Task' }}</h2>
        <label class="block mb-2">
          Title: <input v-model="taskTitle" class="border border-gray-300 p-2 rounded w-full" />
        </label>
        <label class="block mb-4">
          Date:
          <input type="date" v-model="taskDate" class="border border-gray-300 p-2 rounded w-full" />
        </label>
        <div class="flex justify-end">
          <button @click="saveTask" class="bg-blue-500 text-white px-4 py-2 rounded">Save</button>
          <button @click="closeModal" class="ml-2 bg-gray-300 text-black px-4 py-2 rounded">
            Cancel
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
        { id: 1, title: 'Task 1', createdAt: '2024-01-24' },
        { id: 2, title: 'Task 2', createdAt: '2024-01-25' }
     
      ],
      isModalOpen: false,
      editingTask: null,
      taskTitle: '',
      taskDate: ''
    }
  },
  computed: {
    sortedTasks() {
      return this.tasks.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
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
      this.taskDate = task.createdAt
      this.isModalOpen = true
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id)
    },
    saveTask() {
      if (this.editingTask) {
        
        this.editingTask.title = this.taskTitle
        this.editingTask.createdAt = this.taskDate
      } else {
       
        const newTask = {
          id: this.tasks.length + 1,
          title: this.taskTitle,
          createdAt: this.taskDate
        }
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

</style>
