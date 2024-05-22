<template>
  <div class="TypeList">
    <h1>TypeList</h1>

    <!-- Liste des types de tâches -->
    <ul>
      <li v-for="type in types" :key="type" @click="selectType(type)">{{ type }}</li>
    </ul>

    <!-- Zone de texte pour créer un nouveau type de tâche -->
    <input v-model="newType" type="text" placeholder="Nouveau type de tâche">

    <!-- Bouton pour ajouter le nouveau type de tâche -->
    <button @click="addType">Ajouter</button>
  </div>
</template>
<TypeList @typeSelected="updateSelectedType"/>
<ListeTaches :selectedType="selectedType" .../>

<script>
export default {
  data() {
    return {
      types: ['Tous', 'Ma journée', 'Important', 'Autre'],
      newType: '',
      selectedType: 'Tous'
    };
  },
  watch: {
    selectedType(type) {
      // Émettre un événement chaque fois que le type sélectionné change
      this.$emit('typeSelected', type);
    }
  },
  methods: {
    addType() {
      // Ajouter le nouveau type à la liste des types
      this.types.push(this.newType);
      // Réinitialiser la zone de texte
      this.newType = '';
      // Émettre un événement avec la liste mise à jour des types
      this.$emit('typesUpdated', this.types);
    },
    selectType(type) {
      this.selectedType = type;
    },
    updateSelectedType(newType) {
      this.selectedType = newType;
    }
  }
}
</script>