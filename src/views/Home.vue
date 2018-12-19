<template>
  <div class="home">
    <div class="row">
      <div class="col">
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
              <div class="modal-body text-center">
                Task: <br> <input v-model="newTask" type="text" /> <br>
                Notes: <br> <input v-model="newTaskNotes" type="text" /> 
                <!-- <select v-model="newTaskPriority">
                  <option disabled value="">Please select one</option>
                  <option>1</option>
                  <option>2</option>
                  <option>3</option>
                  <option>4</option>
                  <option>5</option>
                </select> -->
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
              class="card mt-3 mb-5 bg-white rounded w-50 p-3 border-secondary text-center mx-auto"
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
                <!-- <h6>Priority: {{ task.priority }}</h6> -->
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
                  v-on:click="slackOut(task);"
                  class="btn btn-link float-left btn-sm btn-circle"
                >
                  <img
                    src="https://cdn-images-1.medium.com/max/1200/1*HbLMI2zIy5y39RZ4lA_RPg.png"
                    width="30"
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
                          Task: <br><input v-model="editTask" type="text" /> <br>
                          Notes: <br><input v-model="editTaskNotes" type="text" />
    <!--                       <select v-model="editTaskPriority">
                            <option disabled value="">Please select one</option>
                            <option>1</option>
                            <option>2</option>
                            <option>3</option>
                            <option>4</option>
                            <option>5</option>
                          </select> -->
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
      <div class="col">
        <h2 class="text-center text-uppercase text-secondary mt-3">Completed</h2>
        <hr class="star-dark mb-5" />
      </div> 
    </div>
  </div>
</template>

<style></style>

<script>
var axios = require("axios");
import draggable from "vuedraggable";

$(document).ready(function() {
 // executes when HTML-Document is loaded and DOM is ready
console.log("document is ready");
  

  $( ".card" ).hover(
  function() {
    $(this).addClass('shadow-lg').css('cursor', 'pointer'); 
  }, function() {
    $(this).removeClass('shadow-lg');
  }
);
  
// document ready  
});





export default {
  components: {
    draggable
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

        this.tasks = this.tasks.filter(task => task.completed == false );
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
      axios
        .post("http://localhost:3000/api/tasks", params)
        .then(
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
      var self = this;
      console.log("editTask");
      this.errors = [];
      var params = {
        input_task_id: this.editId,
        input_task: this.editTask,
        input_notes: this.editTaskNotes,
        input_priority: this.editTaskPriority
      };
      axios
        .patch("http://localhost:3000/api/tasks/" + this.editId, params)
        .then(
          function(response) {

            // loop through tasks and updated current selected
            this.tasks.forEach(function(t, index) {
              if (t.id == parseInt(self.editId)) {
                self.tasks[index].task = self.editTask; 
                self.tasks[index].notes = self.editTaskNotes; 
              }
            });

            // clear modal
            this.editTask = "";
            this.editTaskNotes = "";
            this.editTaskPriority = "";
          }.bind(this)
        )
        .catch(
          function(error) {
            // console.log(error.response);
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
          

        }.bind(this)
      ).catch(function(error) {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
      });
    },
    deleteTask: function(task) {
      axios.delete("http://localhost:3000/api/tasks/" + task.id).then(
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
        console.log(task.task, task.priority);
      });
      var params = this.tasks;
      axios
        .patch("http://localhost:3000/api/tasks/sort", params)
        .then(response => {
          console.log("Updated sort order", response.data);

        });
      // send a web request to the backend with the array of tasks
      // the backend will update all priorities according to the given order
    },
    slackOut: function(task) {
      var params = {
        'data': task
      };
      axios.post("http://localhost:3000/api/slack", params).then(function(response) {
        console.log(response);
      });
    }
  },
  computed: {}
};
</script>

<style>
.home {
  padding-top: 120px;
  background-image: url(../../images/memphis-colorful.png);
  background-size: auto;
  height: 200vh;
}

.btn-circle {
       position:absolute;
    transition: .5s ease;
    top: 78%;
    left: 1%;
}

.card-body {
  color: #2B3E50;
}

body {margin:2rem;}

/*
####################################################
M E D I A  Q U E R I E S
####################################################
*/

/*
::::::::::::::::::::::::::::::::::::::::::::::::::::
Bootstrap 4 breakpoints
*/

/* 
Extra small devices (portrait phones, less than 544px) 
No media query since this is the default in Bootstrap because it is "mobile first"
*/


/* Small devices (landscape phones, 544px and up) */
@media (min-width: 544px) {  

}

/* Medium devices (tablets, 768px and up) 
The navbar toggle appears at this breakpoint */
@media (min-width: 768px) {  

}

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) { 

}

/* Extra large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {  
    
}



/*
::::::::::::::::::::::::::::::::::::::::::::::::::::
Custom media queries
*/

@media (max-width: 950px) { 

}

</style>
