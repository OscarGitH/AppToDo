<template>
  <div class="app">
    <!-- NavBar -->
    <NavBar/>

    <!-- Main content -->
    <div class="content">
      <TaskList/>
    </div>

    <!-- Footer -->
    <AppFooter/>
  </div>
</template>

<script>
import NavBar from './NavBar.vue';
import AppFooter from './AppFooter.vue';
import TaskList from './TaskList.vue';

export default {
  components: {
    NavBar,
    AppFooter,
    TaskList
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

.content {
  padding: 20px;
}
</style>
