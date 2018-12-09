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
      <draggable v-model="tasks">
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
                <h6>Priority: {{ task.priority }}</h6>
                 <p class="card-text">{{ "Notes:" + " " + task.notes }}</p>
                <p class="card-text"><small class="text-muted">{{ task.updated_at }}</small></p>
                <div class="text-center">
                  Completed: 
                  <input v-if="task.completed" type="checkbox" v-on:change="updateCompleted(task)" checked>
                  <input v-else type="checkbox" v-on:change="updateCompleted(task)">
                </div>
                <button v-on:click="setCurrentlyEditingTask(task)" type="button" class="btn btn-warning float-right btn-sm" data-toggle="modal" data-target="#exampleModalCenter">Edit</button>
                  <div class="modal fade" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
                    <div class="modal-dialog modal-dialog-centered" role="document">
                      <div class="modal-content">
                        <div class="modal-header">
                          <h5 class="modal-title" id="exampleModalCenterTitle">Edit Task</h5>
                          <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                          </button>
                        </div>
                        <div class="modal-body">
                          <div class="mt-4 mb-4 mx-auto">
                              Task: <input v-model="editTask" type="text"></br>
                              Notes: <input v-model="editTaskNotes" type="text"></br>
                              Priority: <select v-model="editTaskPriority">
                                        <option disabled value="">Please select one</option>
                                        <option>1</option>
                                        <option>2</option>
                                        <option>3</option>
                                        <option>4</option>
                                        <option>5</option>
                                        </select>
                            </div>
                        </div>
                        <div class="modal-footer">
                          <button type="button" v-on:click="updateTask()" class="btn btn-primary" data-dismiss="modal">Save changes</button>
                        </div>
                      </div>
                    </div>
                  </div>
            </div>
          </div>
      </draggable>
    </div>
  </div>
</template>

<style></style>

 
<script>
var axios = require("axios");
import draggable from 'vuedraggable'


export default {
    components: {
        draggable,
    },
  data: function() {
    return {
      tasks: [],
      newTask: "",
      newTaskNotes: "",
      newTaskPriority: "",
      editId: "",
      editTask: "",
      editTaskNotes: "",
      editTaskPriority: "",
      errors: []
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
      this.errors = [];
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
    setCurrentlyEditingTask: function(inputTask) {
      this.editId = inputTask.id;
      this.editTask = inputTask.task;
      this.editTaskNotes = inputTask.notes;
      this.editTaskPriority = inputTask.priority;
    },
    updateTask: function() {
      console.log("editTask");
      this.errors = [];
      var params = {
        input_task: this.editTask,
        input_notes: this.editTaskNotes,
        input_priority: this.editTaskPriority
      };
      axios.patch("http://localhost:3000/api/tasks/" + this.editId, params).then(
        function(response) {
          console.log(response);
          // this.tasks.push(response.data);
          this.editTask = "";
          this.editTaskNotes = "";
          this.editTaskPriority = "";
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
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
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

<style></style>
