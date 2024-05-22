<template>
  <div class="task-details slide-in">
    <h1><input v-model="localTask.name" type="text"></h1>
    <p><textarea v-model="localTask.description"></textarea></p>
    <p>Date de fin : <input v-model="localTask.endDate" type="date"></p>
    <p>Type de liste :
      <select v-model="localTask.listType">
        <option v-for="type in listTypes" :key="type" :value="type">
          {{ type }}
        </option>
      </select>
    </p>
    <p>État :
      <select v-model="localTask.status">
        <option value="terminé">Terminé</option>
        <option value="en cours">En cours</option>
        <option value="à commencer">À commencer</option>
      </select>
    </p>
    <p>Priorité :
      <select v-model="localTask.priority">
        <option value="moyenne">Moyenne</option>
        <option value="haut">Haut</option>
        <option value="faible">Faible</option>
      </select>
    </p>
    <button @click="updateTask">Valider les modifications</button>
  </div>
</template>

<script>
export default {
  props: ['task', 'listTypes'],
  data() {
    return {
      localTask: {
        ...this.task, // spread the task prop to localTask
        listType: this.task.listType || 'default', // add listType to localTask
      }// Créer une copie locale de la tâche pour les modifications
    };
  },
  methods: {
    updateTask() {
      // Mettre à jour les données de la tâche avec les données locales
      Object.assign(this.task, this.localTask);
      // Émettre un événement pour informer le composant parent des modifications
      this.$emit('taskUpdated', this.task);
    }
  }
}
</script>