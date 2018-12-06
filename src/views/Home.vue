<template>
  <div class="home">
    <div class="mt-4 mb-4 text-center">
        Task: <input v-model="newTask" type="text">
        Notes: <input v-model="newTaskNotes" type="text">
        Priority: <select v-model="newTaskPriority">
                  <option disabled value="">Please select one</option>
                  <option>1</option>
                  <option>2</option>
                  <option>3</option>
                  <option>4</option>
                  <option>5</option>
                  </select>
        <button v-on:click="createTask()" class="btn btn-primary mt-2 ml-5">New Task</button>
      </div>
    <div class="container">
      <h1 class="text-center"><u>Tasks:</u></h1>
      <div v-for="task in tasks" class="card mt-3 shadow p-3 mb-2 bg-white rounded w-50 p-3 text-center mx-auto">
          <div class="card-body">

            <button v-on:click="deleteTask(task)" type="button" class="close text-danger" aria-label="Close">
            <span aria-hidden="true">&times;</span>
            </button>

            <div class="btn-group-vertical float-left">
             <button type="button" class="btn btn-outline-dark mb-3">&#9650</button>
             <button type="button" class="btn btn-outline-dark">&#9660</button>
            </div>

            <h5 class="card-title">{{ task.task }}</h5>
             <p class="card-text">{{ "Notes:" + " " + task.notes }}</p>
            <p class="card-text"><small class="text-muted">{{ task.updated_at }}</small></p>
            <div class="text-center">
              Completed: 
              <input v-if="task.completed" type="checkbox" v-on:change="updateCompleted(task)" checked>
              <input v-else type="checkbox" v-on:change="updateCompleted(task)">
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      tasks: [],
      newTask: "",
      newTaskNotes: "",
      newTaskPriority: ""
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/tasks").then(
      function(response) {
        console.log(response.data);
        this.tasks = response.data;
      }.bind(this)
    );
  },
  methods: {
    createTask: function() {
      console.log("createTask");
      var params = {
        input_task: this.newTask,
        input_notes: this.newTaskNotes,
        input_priority: this.newTaskPriority
      };
      axios.post("http://localhost:3000/api/tasks", params).then(
        function(response) {
          console.log(response);
          this.tasks.push(response.data);
          this.newTask = "";
          this.newTaskNotes = "";
          this.newTaskPriority = "";
        }.bind(this)
      )
      .catch(
        function(error) {
          console.log(error.response);
        }.bind(this)
      );
    },
    updateCompleted: function(task) {
      var params = {
       input_completed: task.completed
      };
      console.log(params);
      axios.patch("http://localhost:3000/api/tasks/" + task.id, params).then(
        function(response) {
          console.log(response.data);
        }.bind(this)
      );
    },
     deleteTask: function(task) {
      axios.delete('http://localhost:3000/api/tasks/' + task.id).then(
        function(response) {
          console.log(response.data);
          var index = this.tasks.indexOf(task);
          this.tasks.splice(index, 1);
      }.bind(this)
      );
    }, 
  },  
  computed: {}
};
</script>

<style>
  
</style>
