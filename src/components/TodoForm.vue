<template>
  <div class="todo-form">
    <input type="text" v-model="name" placeholder="Description">
    <!-- Ajoutez un élément de formulaire pour sélectionner les types de liste -->
    <select v-model="types" multiple>
      <option v-for="type in listTypes" :key="type" :value="type">{{ type }}</option>
    </select>
    <button @click="addTask">Ajouter</button>
  </div>
</template>

<script>
export default {
  props: ['listTypes'],
  data() {
    return {
      name: '',
      types: [] // Initialisez 'types' en tant que tableau vide
    };
  },
  methods: {
    addTask() {
      if (this.name.trim() !== '') {
        this.$emit('addTask', {
          id: Math.random(), // Generate a unique ID (you can use a library for this)
          name: this.name,
          types: this.types,
          // Add other properties like start date, end date, status, priority as per your needs
        });
        this.name = ''; // Reset the name field after adding the task
        this.types = [];
      }
    }
  }
}
</script>

<style scoped>
/* Styles specific to TodoForm component */
</style>