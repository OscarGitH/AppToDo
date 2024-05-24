<template>
    <div class="task-form">
      <h3>Ajouter une tâche <span class="close" @click="close">&#10006;</span></h3>
      <form @submit.prevent="submitTask">
        <div>
          <label for="description">Description :</label>
          <input v-model="task.description" id="description" type="text" required />
        </div>
        <div>
          <label for="startDate">Date de début :</label>
          <input v-model="task.startDate" id="startDate" type="date" required />
        </div>
        <div>
          <label for="endDate">Date de fin :</label>
          <input v-model="task.endDate" id="endDate" type="date" required />
        </div>
        <div>
          <label for="status">Etat :</label>
          <select v-model="task.status" id="status" required>
            <option value="À faire" selected>À faire</option>
            <option value="En cours">En cours</option>
            <option value="Terminée">Terminée</option>
          </select>
        </div>
        <div>
          <label for="priority">Priorité :</label>
          <select v-model="task.priority" id="priority" required>
            <option value="Haute">Haute</option>
            <option value="Moyenne">Moyenne</option>
            <option value="Basse">Basse</option>
          </select>
        </div>
        <div>
          <label for="tags">Notes (optionnel) :</label>
          <textarea v-model="task.notes" id="notes" rows="4"></textarea>
        </div>

        <button type="submit">Ajouter</button>
        <button type="button" @click="close">Annuler</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'AddTaskForm',
    data() {
      return {
        task: {
          description: '',
          startDate: '',
          endDate: '',
          status: 'À faire',
          priority: 'Moyenne',
        }
      }
    },
    methods: {
      submitTask() {
        this.$emit('add-task', { ...this.task });
        this.clearForm();
      },
      clearForm() {
        this.task = {
          description: '',
          startDate: '',
          endDate: '',
          status: 'À faire',
          priority: 'Moyenne'
        };
      },
      close() {
        this.$emit('close-sidebar');
      }
    }
  }
  </script>
  
  <style>
  /* Styles specific to AddTaskForm component */
  .task-form {
    margin-top: 20px;
    width: 100%;
    padding: 20px;
  }
  
  .task-form div {
    margin-bottom: 10px;
  }
  
  .task-form label {
    display: inline-block;
    width: 100%;
  }
  
  .task-form input,
  .task-form select {
    width: calc(100% - 12px); /* Subtract padding */
    padding: 5px;
  }
  
  .task-form button {
    padding: 5px 10px;
    width: 100px;
    background-color: #3f7ffe;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.5s;
    margin-right: 10px;
    margin-top: 10px;
  }
  
  .task-form button:last-child {
    margin-right: 0;
  }
  
  .task-form button:hover {
    background-color: #a541fe;
  }

  .close {
    float: right;
    cursor: pointer;
    margin-right: 10px;
  }

  #notes {
    max-width: 100%; /* Largeur maximale du textarea */
    width: calc(100% - 12px); /* Largeur du textarea - padding */
    resize: vertical; /* Autorise seulement le redimensionnement vertical */
  }

  .task-form textarea {
    width: calc(100% - 12px); /* Prendre en compte le padding */
    padding: 5px;
    resize: vertical; /* Autoriser le redimensionnement vertical */
  }
  </style>
  