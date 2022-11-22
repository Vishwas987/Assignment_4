<template>
  <div id="app">
    <AppHeader @open-add-form="isOpen = !isOpen" />
    <AddTasks v-if="isOpen" @add-new-task="addTask" @close-form="isOpen = !isOpen" />
    <div id="active-task-area">
      <ShowTask :tasks="itTasks(false)" @checkbox-changed="onCheckboxChanged" @edit-task="editTask" title="IT" />
      <ShowTask :tasks="newHireTasks(false)" @checkbox-changed="onCheckboxChanged" @edit-task="editTask" title="New Hire Paperwork" />
      <ShowTask :tasks="cultureTasks(false)" @checkbox-changed="onCheckboxChanged" @edit-task="editTask" title="Culture Orientation" />
      <ShowTask :tasks="otherTasks(false)" @checkbox-changed="onCheckboxChanged" @edit-task="editTask" title="Other" />
    </div>
    <div id="inactive-task-area">
      <ShowTask :tasks="itTasks(true)" @checkbox-changed="onCheckboxChanged" @delete-task="deleteTask" title="IT" /><hr />
      <ShowTask :tasks="newHireTasks(true)" @checkbox-changed="onCheckboxChanged" @delete-task="deleteTask" title="New Hire Paperwork" /><hr />
      <ShowTask :tasks="cultureTasks(true)" @checkbox-changed="onCheckboxChanged" @delete-task="deleteTask" title="Culture Orientation" /><hr />
      <ShowTask :tasks="otherTasks(true)" @checkbox-changed="onCheckboxChanged" @delete-task="deleteTask" title="Other" />
    </div>
  </div>
</template>

<script>
import AddTasks from './components/AddTasks.vue';
import ShowTask from './components/ShowTask.vue';
import AppHeader from './components/AppHeader.vue';
export default {
  name: 'App',
  data(){
    return {
      tasksList: [],
      isOpen: false
    }
  },
  components: {
    AddTasks,
    ShowTask,
    AppHeader
  },
  methods: {
    async addTask(task){
      const res = await fetch("http://localhost:5000/tasks", {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task)
      });

      const data = await res.json();

      this.tasksList = [...this.tasksList, data];
      //console.log(data);
    },

    itTasks(status){
      if(status === false){
        return this.tasksList.filter((task) => task.module === 'IT' && task.done === false);
      }
      else{
        return this.tasksList.filter((task) => task.module === 'IT' && task.done === true);
      }
    },

    cultureTasks(status){
      if(status === false){
        return this.tasksList.filter((task) => task.module === 'Culture Orientation' && task.done === false);
      }
      else{
        return this.tasksList.filter((task) => task.module === 'Culture Orientation' && task.done === true);
      }
    },

    newHireTasks(status){
      if(status === false){
        return this.tasksList.filter((task) => task.module === 'New Hire Paperwork' && task.done === false);
      }
      else{
        return this.tasksList.filter((task) => task.module === 'New Hire Paperwork' && task.done === true);
      } 
    },
    
    otherTasks(status){
      if(status === false){
        return this.tasksList.filter((task) => task.module === 'Other' && task.done === false);
      }
      else{
        return this.tasksList.filter((task) => task.module === 'Other' && task.done === true);
      }
    },

    async onCheckboxChanged(id){
      const taskToUpdate = await this.fetchTask(id);
      const updatedTask = { ...taskToUpdate, done: !taskToUpdate.done };

      const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updatedTask),
      });

      const data = await res.json();

      this.tasksList = this.tasksList.map((task) =>
        task.id === id ? { ...task, done: data.done } : task
      );
    },

    async editTask(newTask){
      let id = newTask.id;
      const taskToUpdate = await this.fetchTask(id);
      const updatedTask = { ...taskToUpdate, dueDate: newTask.dueDate, content: newTask.content };

      const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updatedTask),
      });

      const data = await res.json();

      this.tasksList = this.tasksList.map((task) =>
        task.id === id ? { ...task, dueDate: data.dueDate, content: data.content } : task
      );
    },

    async fetchTask(id){
      const res = await fetch(`http://localhost:5000/tasks/${id}`);
      const data = await res.json();

      return data;
    },

    async deleteTask(id){
      // const res = await fetch(`http://localhost:5000/tasks/${id}`, {
      //   method: 'DELETE'
      // });

      // res.status === 200 ? this.tasksList = this.tasksList.filter((task) => task.id !== id) : alert("Error deleting task");
      this.tasksList = this.tasksList.filter((task) => task.id !== id);
    }
  },
  async created(){
    const res = await fetch("http://localhost:5000/tasks");
      const data = await res.json();

      this.tasksList = data;
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 800px;
  padding: 0.5rem 1rem;
  margin: 8px auto;
  background-color: rgb(248, 248, 248);
  border: 1px solid lightgray;
}
#active-task-area{
  background-color: white;
  border: 1px solid lightgray;
  border-top: 3px solid rgb(228, 19, 0);
}
#inactive-task-area{
  margin-top: 2rem;
}

</style>
