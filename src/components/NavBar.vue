<template>
  <nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
      <!-- Logo ou icône -->
      <a class="navbar-item" href="/">
        <img src="../assets/logo.png" alt="Logo">
        <span class="title is-4">TO DO APP</span>        
      </a>
    </div>
  </nav>
</template>


<style scoped>
/* Styles spécifiques au composant NavBar */
.navbar {
  background-color: #f5f5f5;
  border-bottom: 1px solid #dbdbdb;
  padding: 1rem;
}

.navbar-brand {
  padding: 0 1rem;
}

.navbar-item {
  display: flex;
  align-items: center;
}

.title {
  margin-left: 1rem;
}
</style>
