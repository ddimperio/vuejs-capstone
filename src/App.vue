<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg bg-secondary fixed-top text-uppercase" id="mainNav">
      <div class="container">
       <a class="navbar-brand js-scroll-trigger" href="">
          To Do List
          <img src="https://img.clipartxtras.com/15a393584dc436aeb415bccd83d5c64e_pencil-free-stock-photo-illustration-of-a-pencil-14223-pencil-clipart-no-background_800-800.png" width="30" height="30" class="d-inline-block align-top" alt="">
        </a>

        <div class="collapse navbar-collapse" id="navbarResponsive">
          <ul class="navbar-nav ml-auto">
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
      </div>
    </nav>

    <router-view/>
  </div>
</template>

<style>


</style>
