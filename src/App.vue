<template>
  <div id="app">
    <h1 style="color: #DEB887;">Things To Do ✅</h1>
    <div class="input-container">
      <input v-model="newTask.title" placeholder="Add new task..." style="border: 1px solid #F5F5DC; padding: 5px; margin-bottom: 10px;"> <br>
      <textarea v-model="newTask.description" placeholder="Description" style="border: 1px solid #F5F5DC; padding: 5px; margin-bottom: 10px;"></textarea> <br>
      <input type="date" v-model="newTask.dueDate" style="border: 1px solid #F5F5DC; padding: 5px; margin-bottom: 10px;"> <br>
      <button @click="addTask" style="background-color: #5F9EA0; color: #fff; border: none; padding: 5px 10px; cursor: pointer;">Add Task</button>
    </div>
    <div class="filter-container" style="margin-top: 10px;">
      <span>Show:</span>
      <button @click="setFilter('all')" style="background-color: #6495ED; color: #fff; border: none; padding: 5px 10px; cursor: pointer;">All</button>
      <button @click="setFilter('completed')" style="background-color: #6495ED; color: #fff; border: none; padding: 5px 10px; cursor: pointer;">Completed</button>
      <button @click="setFilter('pending')" style="background-color: #6495ED; color: #fff; border: none; padding: 5px 10px; cursor: pointer;">Pending</button>
    </div>
    <div v-if="tasks.length > 0" style="margin-top: 10px;">
      <button @click="clearCompleted" style="background-color: #dc3545; color: #fff; border: none; padding: 5px 10px; cursor: pointer;">Clear Completed</button>
    </div>
    <ul>
      <li v-for="(task, index) in filteredTasks" :key="index" class="task-item" style="border-bottom: 1px solid #ccc; padding: 10px 0;">
        <div class="task-info">
          <h3 :style="{ 'color': task.completed ? '#28a745' : '#DEB887' }">{{ task.title }}</h3>
          <p>{{ task.description }}</p>
          <p><strong>Due Date:</strong> {{ task.dueDate }}</p>
          <p><strong>Status:</strong> <span :style="{ 'color': task.completed ? '#28a745' : '#dc3545' }">{{ task.completed ? 'Completed' : 'Pending' }}</span></p>
        </div>
        <div class="task-actions">
          <button @click="editTask(index)" style="background-color: #87CEFA; color: #333; border: none; padding: 5px 10px; cursor: pointer;">Edit</button>
          <button @click="toggleStatus(index)" style="background-color: #28a745; color: #fff; border: none; padding: 5px 10px; cursor: pointer;">{{ task.completed ? 'Undo' : 'Complete' }}</button>
          <button @click="deleteTask(index)" style="background-color: #dc3545; color: #fff; border: none; padding: 5px 10px; cursor: pointer;">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: {
        title: '',
        description: '',
        dueDate: '',
        completed: false
      },
      editedTaskIndex: null,
      filter: 'all'
    };
  },
  computed: {
    filteredTasks() {
      if (this.filter === 'completed') {
        return this.tasks.filter(task => task.completed);
      } else if (this.filter === 'pending') {
        return this.tasks.filter(task => !task.completed);
      } else {
        return this.tasks;
      }
    }
  },
  methods: {
    addTask() {
      if (this.newTask.title.trim() !== '') {
        this.tasks.push({ ...this.newTask });
        this.newTask = {
          title: '',
          description: '',
          dueDate: '',
          completed: false
        };
      }
    },
    editTask(index) {
      this.newTask = { ...this.tasks[index] };
      this.editedTaskIndex = index;
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleStatus(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(task => !task.completed);
    },
    setFilter(filter) {
      this.filter = filter;
    },
  },
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  margin-top: 50px;
  text-align: center;
}
.input-container {
  margin-bottom: 20px;
}
input[type="date"] {
  margin-top: 5px;
}
textarea {
  margin-bottom: 5px;
}
ul {
  list-style-type: none;
  padding: 0;
}
.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.task-info {
  flex-grow: 1;
}
.task-actions button {
  margin-left: 5px;
}
</style>
