<script>
  export default{
    data(){
      return {
        name: 'John Doe',
        status: 'Actiscfsve',
        tasks: ['Task 1', 'Task 2', 'Task 3'],
        link: 'https://www.example.com',
      };
    },
    methods:{
      toggleStatus(){
        if(this.status === "Active"){
          this.status = "Pending";
        }
        else if(this.status === "Pending"){
          this.status = "Inactive";
        }
        else{
          this.status = "Active";
        }
      }
    }
  };
</script>

<template>
  <h1>Vue Jobs</h1>
  <p>Hello, {{ name }}!</p>
  <p v-if="status === 'Active'"> User is Active </p>
  <p v-else-if="status === 'Pending'"> User is Pending </p>
  <p v-else> User is Not Active </p>

  <h3>Tasks</h3>
  <ul>
    <li v-for="task in tasks" :key="task">{{ task }}</li>
  </ul>

  <!-- <a v-bind:href="link">Click for example</a> -->
  <a :href="link">Click here for example.com</a>
  <br><br>
  <!-- <button v-on:click="toggleStatus">Change Status</button> -->
  <button @click="toggleStatus">Change Status</button>
</template>

<style>

</style>
