<template>
    <div class="task-list">
      <h2>Liste des tâches</h2>
      <div class="infos">
        <p> Cliquez sur une tâche pour voir les détails</p>
        <p> Cliquez sur l'état d'une tâche pour changer son état</p>
      </div>
      <ul>
        <li v-for="task in tasks" :key="task.id" @click="toggleDetails(task.id)">
          <div class="task-header">
            <h4>{{ task.description }}<span :class="getTaskStatusColor(task.status)" @click.stop="changeTaskStatus(task)" class="task-status">{{ task.status }}</span></h4>
            <img src="../assets/mdi--chevron-down.svg" alt="Toggle details" :class="{ rotate: selectedTask === task.id }" />
          </div>
          <div class="details" v-if="selectedTask === task.id">
            <p>Date de début : {{ task.startDate }}</p>
            <p>Date de fin : {{ task.endDate }}</p>
            <p>Etat : {{ task.status }}</p>
            <p>Priorité : {{ task.priority }}</p>
          </div>
        </li>
      </ul>
      <v-btn class="add-task" @click="toggleAddTaskSidebar">Ajouter une tâche</v-btn>
      <aside v-if="showAddTaskSidebar" class="add-task-sidebar">
        <AddTaskForm @add-task="addTask" @close-sidebar="showAddTaskSidebar = false" />
      </aside>
    </div>
  </template>
  
  <script>
  import AddTaskForm from './AddTaskForm.vue';
  
  export default {
    name: 'TaskList',
    components: {
      AddTaskForm
    },
    data() {
      return {
        tasks: [
          { id: 1, description: 'Task 1', startDate: '2024-01-01', endDate: '2024-01-10', status: 'Terminée', priority: 'High' },
        ],
        showAddTaskSidebar: false,
        selectedTask: null // To track which task's details are shown
      }
    },
    methods: {
      addTask(newTask) {
        newTask.id = this.tasks.length + 1;
        this.tasks.push(newTask);
        this.showAddTaskSidebar = false; // Close the sidebar after adding task
      },
      toggleAddTaskSidebar() {
        this.showAddTaskSidebar = !this.showAddTaskSidebar; // Toggle the sidebar visibility
      },
      toggleDetails(taskId) {
        if (this.selectedTask === taskId) {
          this.selectedTask = null; // Hide details if already shown
        } else {
          this.selectedTask = taskId; // Show details of clicked task
        }
      },
      getTaskStatusColor(status) {
        switch (status) {
          case 'A faire':
            return 'task-todo-color';
          case 'En cours':
            return 'task-in-progress-color';
          case 'Terminée':
            return 'task-finished-color';
          default:
            return '';
        }
      },
      changeTaskStatus(task) {
        switch (task.status) {
          case 'A faire':
            task.status = 'En cours';
            break;
          case 'En cours':
            task.status = 'Terminée';
            break;
          case 'Terminée':
            task.status = 'A faire';
            break;
          default:
            break;
        }
      }
    }
  }
  </script>
  
  <style>
  /* Styles specific to TaskList component */
  .infos p:last-child {
    margin-bottom: 0;
  }

  .task-list {
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
  }
  
  .task-list h4 {
    margin-bottom: 0;
  }
  
  .task-list ul {
    list-style-type: none;
    padding: 0;
  }
  
  .task-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  
  .task-description {
    display: inline;
    margin-right: 10px;
  }
  
  
  .details {
    margin-top: 15px;
  }
  
  .task-list li {
    margin: 3rem 0;
    padding: 1rem;
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    cursor: pointer;
  }
  
  .task-list li:last-child {
    border-bottom: none;
  }

  .task-list .task-status {
    user-select: none;
    }
  
  .add-task-sidebar {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    width: 33%;
    background-color: #fff;
    box-shadow: -5px 0 15px rgba(0, 0, 0, 0.2);
    z-index: 1000;
  }
  
  .add-task {
    padding: 7.5px 15px;
    width: 100px;
    background-color: #3f7ffe;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.5s;
    margin-right: 10px;
    margin-top: 10px;
  }
  
  .add-task:hover {
    background-color: #a541fe;
  }
  
  .rotate {
    transform: rotate(180deg);
  }
  
  .task-todo-color {
    background-color: #3f7ffe;
    padding: 0.25rem;
    color: white;
    border-radius: 5px;
    font-size: 1rem;
  }
  
  .task-in-progress-color {
    background-color: #f7b500;
    padding: 0.25rem;
    color: white;
    border-radius: 5px;
    font-size: 1rem;
  }
  
  .task-finished-color {
    background-color: #00b74a;
    padding: 0.25rem;
    color: white;
    border-radius: 5px;
    font-size: 1rem;
  }

  .task-status {
    margin-left: 1rem
  }

  </style>
  