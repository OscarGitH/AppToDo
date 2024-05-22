<template>
  <div class="todo-list">
    <div v-for="task in tasks" :key="task.id">
      {{ task.description }}
    </div>
  </div>
</template>

<script>
export default {
  props: ['tasks']
}
</script>

<style scoped>
/* Styles spécifiques au composant TodoList */
</style>
