<template>
  <div class="todo-list">
    <div v-for="task in tasks" :key="task.id">
      <p>{{ task.description }}</p>
      <!-- Affichez d'autres détails de la tâche ici -->
    </div>
  </div>
</template>

<script>
export default {
  props: ['tasks'] // Recevoir les tâches en tant que prop depuis le composant parent (TodoApp.vue)
}
</script>

<style scoped>
/* Styles spécifiques au composant TodoList */
</style>
