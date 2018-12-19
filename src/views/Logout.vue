<template>
  <div class="logout">
  </div>
</template>

<script>
import axios from "axios";

export default {
  created: function() {
    delete axios.defaults.headers.common["Authorization"];
    localStorage.removeItem("jwt");
    this.$router.push("/");
  }
};
</script>

<style>
  
.logout {
  padding-top: 120px;
  background-image: url(../../images/memphis-colorful.png);
  background-size: auto;
  height: 100vh;
}

</style>