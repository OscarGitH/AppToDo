<template>
  <div class="task-list">
    <h2>Liste des tâches</h2>
    <div class="infos">
      <p> Cliquez sur une tâche pour voir les détails</p>
      <p> Cliquez sur l'état d'une tâche pour changer son état</p>
    </div>
    <div class="sort-buttons">
      <h4>Trier par :</h4>
      <button @click="sortByEndDate()" :class="{ active: activeSort === 'end-date' }">
        Date de fin
        <span :class="sortCriteria.enddate ? 'arrow-down' : 'arrow-up'"></span>
      </button>
      <button @click="sortByStartDate()" :class="{ active: activeSort === 'start-date' }">
        Date de début
        <span :class="sortCriteria.startdate ? 'arrow-down' : 'arrow-up'"></span>
      </button>
      <button @click="sortByPriority()" :class="{ active: activeSort === 'priority' }">
        Priorité
        <span :class="sortCriteria.priority ? 'arrow-down' : 'arrow-up'"></span>
      </button>
    </div>

    <div class="hide-finished">
      <input type="checkbox" id="hide-finished" v-model="hideFinished" />
      <label for="hide-finished">Masquer les tâches terminées</label>
    </div>

    <div class="expand-buttons">
      <button @click="toggleAllDetails">{{ allDetailsVisible ? 'Fermer toutes les tâches &times;' : 'Ouvrir toutes les tâches &plus;' }}</button>
    </div>
    <ul>
      <li v-for="task in sortedTasks" :key="task.id" @click="toggleDetails(task.id)">
        <div class="task-header">
          <h4>
          <span :class="getTaskStatusColor(task.status)" @click.stop="changeTaskStatus(task)" class="task-status">{{ task.status }}</span>
          <span class="task-description">{{ task.description }}</span>
          <span class="task-details">{{ formatDate(task.endDate) }} - {{ task.priority }}</span>
        </h4>

          <div class="task-actions">
            <button @click.stop="confirmDeleteTask(task.id)" class="delete-task">
              <span class='delete-task'>Supprimer cette tâche</span>
            </button>
            <img src="../assets/mdi--chevron-down.svg" alt="Toggle details" :class="{ rotate: selectedTask === task.id || allDetailsVisible }" />
          </div>
        </div>


        <div class="details" v-if="selectedTask === task.id || allDetailsVisible">
          <p>Date de début : {{ formatDate(task.startDate) }}</p>
          <p>Date de fin : {{ formatDate(task.endDate) }}</p>
          <p>Priorité : {{ task.priority }}</p>
        </div>
      </li>
    </ul>
    <v-btn class="add-task" @click="toggleAddTaskSidebar">&#43; Ajouter une tâche</v-btn>
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
        { id: 1, description: 'Tache Exemple', startDate: '2024-01-01', endDate: '2024-01-10', status: 'Terminée', priority: 'Haute' },
        ],
      showAddTaskSidebar: false,
      selectedTask: null,
      allDetailsVisible: false,
      sortCriteria: {
        startdate: null,
        enddate: null,
        priority: false
      },
      activeSort: 'end-date',
      hideFinished: false // Propriété pour masquer les tâches terminées
    };
  },
  created() {
    // Charger les tâches depuis le stockage local
    const storedTasks = localStorage.getItem('tasks');
    if (storedTasks) {
      this.tasks = JSON.parse(storedTasks);
    }
  },
  watch: {
    // Surveiller les changements dans les tâches et mettre à jour le stockage local
    tasks: {
      handler: function(updatedTasks) {
        localStorage.setItem('tasks', JSON.stringify(updatedTasks));
      },
      deep: true
    }
  },
  methods: {
    addTask(newTask) {
      newTask.id = this.tasks.length + 1;
      this.tasks.push(newTask);
      this.showAddTaskSidebar = false;
    },
    toggleAddTaskSidebar() {
      this.showAddTaskSidebar = !this.showAddTaskSidebar;
    },
    toggleDetails(taskId) {
      this.selectedTask = this.selectedTask === taskId ? null : taskId;
    },
    getTaskStatusColor(status) {
      switch (status) {
        case 'À faire':
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
        case 'À faire':
          task.status = 'En cours';
          break;
        case 'En cours':
          task.status = 'Terminée';
          break;
        case 'Terminée':
          task.status = 'À faire';
          break;
        default:
          break;
      }
    },
    sortByEndDate() {
      this.sortCriteria.enddate = !this.sortCriteria.enddate;
      this.sortCriteria.startdate = false;
      this.sortCriteria.priority = false;
      this.activeSort = 'end-date';
    },
    sortByStartDate() {
      this.sortCriteria.startdate = !this.sortCriteria.startdate;
      this.sortCriteria.enddate = false;
      this.sortCriteria.priority = false;
      this.activeSort = 'start-date';
    },

    sortByPriority() {
      this.sortCriteria.startdate = null;
      this.sortCriteria.enddate = null;
      this.sortCriteria.priority = !this.sortCriteria.priority;
      this.activeSort = 'priority';
    },
    formatDate(date) {
      return new Date(date).toLocaleDateString('fr-FR');
    },
    toggleAllDetails() {
      this.allDetailsVisible = !this.allDetailsVisible;
    },
    confirmDeleteTask(taskId) {
      if (confirm("Voulez-vous vraiment supprimer cette tâche ?")) {
        this.deleteTask(taskId);
      }
    },

    deleteTask(taskId) {
      const index = this.tasks.findIndex(task => task.id === taskId);
      if (index !== -1) {
        this.tasks.splice(index, 1);
      }
    }
  },
  computed: {
    sortedTasks() {
      let sortedTasks = [...this.tasks];

      // Filtrer les tâches terminées si hideFinished est vrai
      if (this.hideFinished) {
        sortedTasks = sortedTasks.filter(task => task.status !== 'Terminée');
      }

      // Appliquer le tri
      if (this.sortCriteria.enddate) {
        sortedTasks.sort((a, b) => {
          const dateA = new Date(this.sortCriteria.startdate ? a.startDate : a.endDate);
          const dateB = new Date(this.sortCriteria.startdate ? b.startDate : b.endDate);
          return this.sortCriteria.startdate ? dateA - dateB : dateB - dateA;
        });
      } else if (this.sortCriteria.startdate) {
        sortedTasks.sort((a, b) => {
          const dateA = new Date(this.sortCriteria.enddate ? a.endDate : a.startDate);
          const dateB = new Date(this.sortCriteria.enddate ? b.endDate : b.startDate);
          return this.sortCriteria.enddate ? dateA - dateB : dateB - dateA;
        });
      } else if (this.sortCriteria.priority) {
        sortedTasks.sort((a, b) => {
          const priorityOrder = {
            'Haute': 0,
            'Moyenne': 1,
            'Basse': 2
          };
          return priorityOrder[b.priority] - priorityOrder[a.priority];
        });
      }

      return sortedTasks;
    }
  }
};
</script>

<style>
  /* Styles spécifiques au composant TaskList */
  .infos p:last-child {
    margin-bottom: 1em;
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
    margin: 1rem 0 2rem 0;
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
    font-size: 0.9rem;
  }

  .content li+li {
    margin-top: 0.5rem;
  }

  .task-list li {
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
    transition: background-color 0.3s;
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
    margin-right: 1rem;
    width: 100px;
    display: inline-block;
    white-space: nowrap;
    text-align: center;
  }

  .sort-buttons {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
  }

  .sort-buttons button {
    padding: 5px 10px;
    background-color: #3f7ffe;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .sort-buttons button:hover {
    background-color: #a541fe;
  }

  .sort-buttons .active {
    background-color: #a541fe;
  }

  .expand-buttons {
    display: flex;
    justify-content: flex-end;
  }

  .expand-buttons button {
    padding: 5px 10px;
    background-color: #3f7ffe;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .expand-buttons button:hover {
    background-color: #a541fe;
  }

  .arrow-up::before {
    content: "▲";
    display: inline-block;
    margin-left: 5px;
  }

  .arrow-down::before {
    content: "▼";
    display: inline-block;
    margin-left: 5px;
  }

  .sort-buttons .arrow-up,
  .sort-buttons .arrow-up-priority {
    content: "▲";
    display: inline-block;
    margin-left: 5px;
  }

  .sort-buttons .arrow-down,
  .sort-buttons .arrow-down-priority {
    content: "▼";
    display: inline-block;
    margin-left: 5px;
  }

  .hide-finished {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }

  .hide-finished input[type="checkbox"] {
    margin-right: 5px;
    cursor: pointer;
  }

  .hide-finished label {
    cursor: pointer;
  }

  .delete-task {
    color: #2b2b2b;
    border: none;
    cursor: pointer;
    background-color: transparent;
    margin-right: 10px;
  }

  .task-details {
    font-weight: lighter;
    font-size: 0.9rem;
  }

  @media (max-width: 768px) {
    .task-header {
      flex-direction: column;
      align-items: flex-start;
    }

    .task-header h4 {
      margin-bottom: 0;
      font-size: 1.1rem;
    }

    .task-header .task-actions {
      display: flex;
      justify-content: flex-end;
      width: 100%;
      font-size: 0.9rem;

    }

    .task-header .task-actions button {
      margin-right: 0;
      margin-bottom: 5px;
    }

    .task-list li {
      flex-direction: column;
    }

    .task-list .task-header h4 {
      margin-bottom: 10px;
    }

    .task-list .task-header img {
      margin-top: 5px;
    }

    .task-details {
      display: none;
    }

    .task-description {
      display: block;
      margin: 0.5rem 0;
    }

    .sort-buttons {
      flex-direction: column;
      align-items: flex-start;
    }

    .sort-buttons button {
      width: 100%; /* Occupe toute la largeur disponible */
    }
  }

</style>