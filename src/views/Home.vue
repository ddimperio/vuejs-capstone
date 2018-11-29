<template>
  <div class="home">
    <div class="container">
      <div v-for="task in tasks" class="card">
          <div class="card-body">
            <h5 class="card-title">{{ task.task }}</h5>
            <p class="card-text">{{ task.notes }}</p>
            <p class="card-text"><small class="text-muted">{{ task.updated_at }}</small></p>
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
      message: "Welcome to Vue.js!",
      tasks: []
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
  methods: {},
  computed: {}
};
</script>
