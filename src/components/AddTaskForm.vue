<template>
  <div class="task-form">
    <h3>{{ task.id ? 'Modifier une tâche' : 'Ajouter une tâche' }} <span class="close" @click="close">&#10006;</span>
    </h3>
    <form @submit.prevent="submitForm">
      <div>
        <label for="description">Description :</label>
        <input v-model="taskData.description" id="description" type="text" required/>
      </div>
      <div>
        <label for="startDate">Date de début :</label>
        <input v-model="taskData.startDate" id="startDate" type="date" required/>
      </div>
      <div>
        <label for="endDate">Date de fin :</label>
        <input v-model="taskData.endDate" id="endDate" type="date" required/>
        <span v-if="isEndDateInvalid"
              class="error-message">La date de fin doit être postérieure à la date de début.</span>
      </div>
      <div>
        <label for="status">Etat :</label>
        <select v-model="taskData.status" id="status" required>
          <option value="À faire" selected>À faire</option>
          <option value="En cours">En cours</option>
          <option value="Terminée">Terminée</option>
        </select>
      </div>
      <div>
        <label for="priority">Priorité :</label>
        <select v-model="taskData.priority" id="priority" required>
          <option value="Haute">Haute</option>
          <option value="Moyenne">Moyenne</option>
          <option value="Basse">Basse</option>
        </select>
      </div>
      <div>
        <textarea v-model="taskData.notes" id="notes" rows="4"></textarea>
      </div>

      <button @click="submitForm">
        {{ task.id ? 'Enregistrer' : 'Ajouter' }}
      </button>
      <button type="button" @click="close">Annuler</button>
    </form>
  </div>
</template>

<script>

export default {
  name: 'AddTaskForm',
  props: {
    task: {
      type: Object,
      default: () => ({
        description: '',
        startDate: '',
        endDate: '',
        status: 'À faire',
        priority: 'Moyenne',
        notes: ''
      })
    }
  },
  data() {
    return {
      taskData: {...this.task}
    };
  },
  methods: {
    clearForm() {
      this.taskData = {
        description: '',
        startDate: '',
        endDate: '',
        status: 'À faire',
        priority: 'Moyenne',
        notes: ''
      };
    },
    close() {
      this.$emit('close-sidebar');
    },
    isEndDateInvalid() {
      return new Date(this.taskData.endDate) <= new Date(this.taskData.startDate);
    },
    submitForm() {
      if (this.isEndDateInvalid()) {
        alert("La date de fin doit être postérieure à la date de début.");
        return;
      }
      if (this.taskData.description && this.taskData.startDate && this.taskData.endDate) {
        if (this.taskData.id) {
          this.$emit('edit-task', this.taskData);
        } else {
          this.$emit('add-task', {...this.taskData});
          this.clearForm();
        }
      }
    }
  }
};
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
  max-width: 100%;
  width: calc(100% - 12px);
  resize: vertical;
}

.task-form textarea {
  width: calc(100% - 12px);
  padding: 5px;
  resize: vertical;
}

.error-message {
  font-size: 0.8rem;
}
</style>
  