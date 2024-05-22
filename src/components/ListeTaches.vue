<template>
  <div class="liste-taches">
    <ul>
      <li v-for="task in localTasks" :key="task.id" @click="selectTask(task)">
        {{ task.name }}
      </li>
    </ul>
    <TaskDetails :listTypes="listTypes" v-if="selectedTask" :task="selectedTask"/>
  </div>
</template>

<script>
import TaskDetails from './TaskDetails.vue';

export default {
  components: {
    TaskDetails
  },
  props: ['tasks'],
  data() {
    return {
      localTasks: this.tasks,
      selectedTask: 'Tous'
    };
  },
  methods: {
    ajouterTache() {
      // Émettre un événement pour indiquer qu'on veut ajouter une nouvelle tâche
      this.$emit('ajouterTache');
    },
    addTask(task) {
      // Ajouter la nouvelle tâche à la liste des tâches
      this.localTasks.push(task);
      // Émettre un événement pour indiquer qu'une nouvelle tâche a été ajoutée
      this.$emit('addTask', task);
    },
    selectTask(task) {
      if (this.selectedTask === task) {
        this.selectedTask = null; // Si la tâche sélectionnée est déjà affichée, la faire disparaître
      } else {
        this.selectedTask = task; // Sinon, afficher les détails de la tâche sélectionnée
      }
    },
    updateSelectedType(newType) {
      this.selectedType = newType;
    },
    filterTasks() {
      if (this.selectedType === 'Tous') {
        return this.localTasks;
      } else {
        return this.localTasks.filter(task => task.listType === this.selectedType);
      }
    }
  },
  beforeUnmount() {
    // Arrêter d'écouter l'événement 'addTask' lorsque le composant est détruit
    this.$parent.$off('addTask', this.addTask);
  }
}
</script>

<style scoped>
/* Styles spécifiques au composant ListeTaches */
.liste-taches li {
  cursor: pointer;
}
</style>