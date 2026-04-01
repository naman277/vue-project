<script setup>
import { onMounted, ref } from 'vue';

const name = ref('John Doe');
const status = ref('Active');
const tasks = ref(['Task 1', 'Task 2', 'Task 3']);
const newTask = ref('naman'); //write something in the input field and it will be stored in newTask variable and displayed in the input field of the form by default

const toggleStatus = () => {
  if(status.value === "Active"){
    status.value = "Pending";
  }
  else if(status.value === "Pending"){
    status.value = "Inactive";
  }
  else{
    status.value = "Active";
  }
}

const addTask = ()=>{
  if(newTask.value.trim() !== ''){
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try{
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  }
  catch(error){
    console.log('Error fetching tasks');
  }
});
</script>

<template>
  <h1>Vue Jobs</h1>
  <p>Hello, {{ name }}!</p>
  <p v-if="status === 'Active'"> User is Active </p>
  <p v-else-if="status === 'Pending'"> User is Pending </p>
  <p v-else> User is Not Active </p>

  <form @submit.prevent="addTask"> <!-- .prevent is used to prevent the default form submission behavior. It saves us from writing event.preventDefault() inside the function handler -->
    <label for="newTask">Add Task: </label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <br>
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }} 
      </span> <span></span>
      <button @click="deleteTask(index)">X</button>  
    </li>
  </ul>
  <br>
  <!-- <button v-on:click="toggleStatus">Change Status</button> -->
  <button @click="toggleStatus">Change Status</button>
</template>

<style>

</style>
