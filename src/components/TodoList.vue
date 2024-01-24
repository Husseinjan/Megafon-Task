<!-- src/components/TodoList.vue -->
<template>
    <div>
      <h1>Todo List</h1>
      <button @click="createTask">Create Task</button>
  
      <ul>
        <li v-for="task in sortedTasks" :key="task.id">
          {{ task.title }} - {{ task.createdAt }}
          <button @click="editTask(task)">Edit</button>
          <button @click="deleteTask(task.id)">Delete</button>
        </li>
      </ul>
  
      <!-- Модальное окно для создания/редактирования задачи -->
      <div v-if="isModalOpen">
        <h2>{{ editingTask ? 'Edit Task' : 'Create Task' }}</h2>
        <label>Title: <input v-model="taskTitle" /></label>
        <label>Date: <input type="date" v-model="taskDate" /></label>
        <button @click="saveTask">Save</button>
        <button @click="closeModal">Cancel</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tasks: [
          { id: 1, title: 'Task 1', createdAt: '2024-01-24' },
          { id: 2, title: 'Task 2', createdAt: '2024-01-25' },
          // Другие задачи...
        ],
        isModalOpen: false,
        editingTask: null,
        taskTitle: '',
        taskDate: '',
      };
    },
    computed: {
      sortedTasks() {
        return this.tasks.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
      },
    },
    methods: {
      createTask() {
        this.editingTask = null;
        this.taskTitle = '';
        this.taskDate = '';
        this.isModalOpen = true;
      },
      editTask(task) {
        this.editingTask = task;
        this.taskTitle = task.title;
        this.taskDate = task.createdAt;
        this.isModalOpen = true;
      },
      deleteTask(id) {
        this.tasks = this.tasks.filter(task => task.id !== id);
      },
      saveTask() {
        if (this.editingTask) {
          // Редактирование задачи
          this.editingTask.title = this.taskTitle;
          this.editingTask.createdAt = this.taskDate;
        } else {
          // Создание новой задачи
          const newTask = {
            id: this.tasks.length + 1,
            title: this.taskTitle,
            createdAt: this.taskDate,
          };
          this.tasks.push(newTask);
        }
  
        this.closeModal();
      },
      closeModal() {
        this.isModalOpen = false;
      },
    },
  };
  </script>
  
  <style scoped>
  /* Стили с использованием Tailwind CSS */
  </style>
  