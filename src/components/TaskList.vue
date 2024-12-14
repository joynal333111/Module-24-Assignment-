<template>
    <div class="task-manager">
      <h1 class="title">Task Manager</h1>
      <button class="toggle-button" @click="state.showForm = !state.showForm">
        {{ state.showForm ? 'Hide Task Form' : 'Add New Task' }}
      </button>
  
      <div v-if="state.showForm" class="task-form">
        <input
          v-model="state.newTaskName"
          class="task-input"
          placeholder="Enter a task name"
        />
        <button class="add-button" @click="addTask">Add Task</button>
      </div>
  
      <p v-if="state.tasks.length === 0" class="no-tasks">
        No tasks available. Add some tasks to get started!
      </p>
  
      <ul v-else class="task-list">
        <li
          v-for="(task, index) in state.tasks"
          :key="index"
          :class="{ completed: task.completed }"
          class="task-item"
        >
          <label>
            <input
              type="checkbox"
              v-model="task.completed"
              class="task-checkbox"
              @change="toggleCompleted(task)"
            />
            <span class="task-name">{{ task.name }}</span>
          </label>
          <button class="delete-button" @click="deleteTask(index)">
            Delete
          </button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import { reactive } from "vue";
  
  export default {
    setup() {
      const state = reactive({
        tasks: [],
        newTaskName: "",
        showForm: false,
      });
  
      const addTask = () => {
        if (state.newTaskName.trim().length >= 3) {
          state.tasks.push({ name: state.newTaskName, completed: false });
          state.newTaskName = "";
          saveTasks();
        } else {
          alert("Task name must be at least 3 characters long.");
        }
      };
  
      const deleteTask = (index) => {
        state.tasks.splice(index, 1);
        saveTasks();
      };
  
      const toggleCompleted = (task) => {
        task.completed = !task.completed;
        saveTasks();
      };
  
      const saveTasks = () => {
        localStorage.setItem("tasks", JSON.stringify(state.tasks));
      };
  
      const loadTasks = () => {
        const storedTasks = localStorage.getItem("tasks");
        if (storedTasks) {
          state.tasks = JSON.parse(storedTasks);
        }
      };
  
      loadTasks();
  
      return { state, addTask, deleteTask, toggleCompleted };
    },
  };
  </script>
  
  <style scoped>
  /* General Layout */
  .task-manager {
    max-width: 600px;
    margin: 50px auto;
    padding: 20px;
    border-radius: 10px;
    background: #ffffff;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    font-family: Arial, sans-serif;
  }
  
  /* Title */
  .title {
    text-align: center;
    color: #333;
    font-size: 28px;
    font-weight: bold;
    margin-bottom: 20px;
  }
  
  /* Toggle Button */
  .toggle-button {
    display: block;
    margin: 0 auto 20px;
    padding: 10px 20px;
    font-size: 16px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .toggle-button:hover {
    background-color: #0056b3;
  }
  
  /* Task Form */
  .task-form {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .task-input {
    flex: 1;
    padding: 10px;
    font-size: 16px;
    border: 2px solid #ddd;
    border-radius: 5px;
    margin-right: 10px;
  }
  
  .add-button {
    padding: 10px 20px;
    font-size: 16px;
    background-color: #28a745;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .add-button:hover {
    background-color: #218838;
  }
  
  /* No Tasks Message */
  .no-tasks {
    text-align: center;
    color: #888;
    font-size: 16px;
    margin: 20px 0;
  }
  
  /* Task List */
  .task-list {
    list-style: none;
    padding: 0;
  }
  
  .task-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 15px;
    margin-bottom: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
    background-color: #f9f9f9;
    transition: background-color 0.3s ease;
  }
  
  .task-item.completed {
    background-color: #d4edda;
    border: 1px solid #c3e6cb;
  }
  
  .task-item label {
    display: flex;
    align-items: center;
  }
  
  .task-checkbox {
    margin-right: 10px;
  }
  
  .task-name {
    font-size: 16px;
    color: #333;
  }
  
  /* Delete Button */
  .delete-button {
    padding: 5px 10px;
    font-size: 14px;
    background-color: #dc3545;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .delete-button:hover {
    background-color: #c82333;
  }
  </style>
  