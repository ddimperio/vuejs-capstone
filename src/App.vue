<template>
  <div id="app">
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">
    ToDoList
    <img src="https://img.clipartxtras.com/15a393584dc436aeb415bccd83d5c64e_pencil-free-stock-photo-illustration-of-a-pencil-14223-pencil-clipart-no-background_800-800.png" width="30" height="30" class="d-inline-block align-top" alt="">
  </a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNavDropdown">
    <ul class="navbar-nav">
      <li class="nav-item active">
        <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Account
        </a>
        <div class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
          <a class="dropdown-item" href="/#/signup">Signup</a>
          <a class="dropdown-item" href="/#/login">Login</a>
          <a class="dropdown-item" href="/#/logout">Logout</a>
        </div>
      </li>
    </ul>
  </div>
</nav>
    <router-view/>
  </div>
</template>

<style>


</style>
