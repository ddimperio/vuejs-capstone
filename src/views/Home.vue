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
      <div v-for="task in tasks" class="card h-25 mt-3 shadow p-3 mb-5 bg-white rounded">
          <div class="card-body">
              <a class="text-right" href="#">
              <img src="http://reverse.keepthetech.com/wp-content/uploads/2018/09/Cancel-Subscription.png" width="30" height="30" alt="">
              </a>
            <h5 class="card-title">{{ task.task }}</h5>
             <p class="card-text">{{ "Notes:" + " " + task.notes }}</p>
            <p class="card-text"><small class="text-muted">{{ task.updated_at }}</small></p>
            <div class="text-right">
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
    }
  },  
  computed: {}
};
</script>
