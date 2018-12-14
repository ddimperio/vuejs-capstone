<template>
  <div class="home">
    <h2 class="text-center text-uppercase text-secondary mt-3">Tasks</h2>
    <hr class="star-dark mb-5" />
    <!-- Button trigger modal -->
    <button
      id="newbtn"
      type="button"
      class="btn btn-primary btn-lg mx-auto d-block"
      data-toggle="modal"
      data-target="#exampleModal"
    >
      New Task
    </button>

    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Create New Task</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            Task: <input v-model="newTask" type="text" /> Notes:
            <input v-model="newTaskNotes" type="text" /> Priority:
            <select v-model="newTaskPriority">
              <option disabled value="">Please select one</option>
              <option>1</option>
              <option>2</option>
              <option>3</option>
              <option>4</option>
              <option>5</option>
            </select>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              v-on:click="createTask();"
              class="btn btn-primary"
              data-dismiss="modal"
            >
              Save changes
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <draggable v-model="tasks" v-on:end="endDrag">
        <div
          v-for="task in tasks"
          class="card mt-3 shadow p-3 mb-5 bg-white rounded w-50 p-3 border-secondary text-center mx-auto"
        >
          <div class="card-body">
            <button
              v-on:click="deleteTask(task);"
              type="button"
              class="close text-danger"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>

            <h5 class="card-title">{{ task.task }}</h5>
            <h6>Priority: {{ task.priority }}</h6>
            <p class="card-text">{{ "Notes:" + " " + task.notes }}</p>
            <p class="card-text">
              <small class="text-muted">{{ task.updated_at }}</small>
            </p>
            <div class="text-center">
              Completed:
              <input
                v-if="task.completed"
                type="checkbox"
                v-on:change="updateCompleted(task);"
                checked
              />
              <input
                v-else
                type="checkbox"
                v-on:change="updateCompleted(task);"
              />
            </div>

            <button
              type="button"
              v-on:click="slackOut();"
              class="btn btn-dark float-left btn-sm btn-circle"
            >
              <img
                src="https://cdn.freebiesupply.com/logos/large/2x/slack-1-logo-png-transparent.png"
                width="20"
              />
            </button>

            <button
              v-on:click="setCurrentlyEditingTask(task);"
              type="button"
              class="btn btn-warning float-right"
              data-toggle="modal"
              data-target="#exampleModalCenter"
            >
              Edit
            </button>
            <div
              class="modal fade"
              id="exampleModalCenter"
              tabindex="-1"
              role="dialog"
              aria-labelledby="exampleModalCenterTitle"
              aria-hidden="true"
            >
              <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalCenterTitle">
                      Edit Task
                    </h5>
                    <button
                      type="button"
                      class="close"
                      data-dismiss="modal"
                      aria-label="Close"
                    >
                      <span aria-hidden="true">&times;</span>
                    </button>
                  </div>
                  <div class="modal-body">
                    <div class="mt-4 mb-4 mx-auto">
                      Task: <input v-model="editTask" type="text" /> Notes:
                      <input v-model="editTaskNotes" type="text" /> Priority:
                      <select v-model="editTaskPriority">
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
                    <button
                      type="button"
                      v-on:click="updateTask();"
                      class="btn btn-primary"
                      data-dismiss="modal"
                    >
                      Save changes
                    </button>
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
    endDrag: function(event) {
      console.log("endDrag", event);
      this.tasks.forEach(task => {
        console.log(task.task, task.priority)
      })
      var params = this.tasks;
      axios.patch("http://localhost:3000/api/tasks/sort", params).then(response => {
        console.log("Updated sort order", response.data);
      })
      // send a web request to the backend with the array of tasks
      // the backend will update all priorities according to the given order
    },
    slackOut: function(){
      axios.post("https://hooks.slack.com/services/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").then(
        function(response){
          {"text": "Hello, world."}
        }
      );
    }

  },
  computed: {}
};
</script>

<style>
.home {
  padding-top: 120px;
}

.btn-circle {
  border-radius: 50%;
}
</style>
