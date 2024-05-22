<template>
  <nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
      <!-- Logo ou icône -->
      <a class="navbar-item" href="#">
        <img src="votre-logo.png" alt="Logo">
      </a>
    </div>

    <div class="navbar-menu">
      <div class="navbar-start">
        <!-- Boutons de navigation -->
        <a class="navbar-item" href="#">Accueil</a>
        <a class="navbar-item" href="#">Profil</a>
        <a class="navbar-item" href="#">Paramètres</a>
      </div>
      <div class="navbar-end">
        <!-- Barre de recherche -->
        <div class="navbar-item">
          <div class="field">
            <div class="control has-icons-left">
              <input class="input" type="text" placeholder="Rechercher...">
              <span class="icon is-small is-left">
                <i class="fas fa-search"></i>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '' // Stocker la requête de recherche
    };
  },
  methods: {
    searchTasks() {
      // Émettre un événement avec la requête de recherche
      this.$emit('search', this.searchQuery);
    }
  }
}
</script>

<style scoped>
/* Styles spécifiques au composant NavBar */
</style>
