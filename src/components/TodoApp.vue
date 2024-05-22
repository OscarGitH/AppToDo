<template>
  <div class="app">
    <!-- NavBar -->
    <NavBar/>

    <!-- Zones separated horizontally -->
    <div class="zones">
      <TypeList class="TypeList" @typeSelected="filterTasks"/>
      <ListeTaches class="liste-taches" :tasks="filteredTasks" :listTypes="listTypes"/>
      <TodoForm class="TodoForm" @addTask="ajouterTache" :listTypes="listTypes"/>
      <TaskDetails :listTypes="listTypes" v-if="selectedTask" :task="selectedTask"/>
    </div>
  </div>
</template>

<script>
import NavBar from './NavBar.vue';
import TypeList from './TypeList.vue';
import ListeTaches from './ListeTaches.vue';
import TodoForm from './TodoForm.vue';
import TaskDetails from './TaskDetails.vue';

export default {
  components: {
    NavBar,
    TypeList,
    ListeTaches,
    TodoForm,
    TaskDetails
  },
  data() {
    return {
      tasks: [], // Initialize an empty array to store tasks
      filteredTasks: [], // Initialize an empty array to store filtered tasks
      listTypes: TypeList.data().types // Ajoutez 'listTypes' aux données
    };
  },
  methods: {
    ajouterTache(task) {
      // Add the new task to the tasks array
      this.tasks.push(task);
      this.filterTasks();
    },
    handleTypesUpdated(types) {
      this.listTypes = types;
    },
    filterTasks(type = 'Tous') {
      if (type === 'Tous') {
        this.filteredTasks = this.tasks;
      } else {
        this.filteredTasks = this.tasks.filter(task => task.types.includes(type));
      }
    }
  },
  created() {
    this.filterTasks();
  }
}
</script>

<style>
/* Styles specific to App component */
.zones {
  display: flex;
  flex-direction: row;
}

.TypeList, .liste-taches, .TodoForm {
  flex: 1;
}
</style>