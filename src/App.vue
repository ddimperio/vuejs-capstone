<template>
  <div id="app">
    <div class="header">
      <nav class="navbar navbar-expand-lg bg-secondary fixed-top text-uppercase" id="mainNav">
        <div class="container">
         <a class="navbar-brand js-scroll-trigger" href="">
            To Do List
            <img src="../images/pencil.png" width="30" height="30" class="d-inline-block align-top" alt="">
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
    </div>

    <router-view/>

    <!-- Footer -->
    <footer class="footer text-center">
      <div class="container">
        <div class="row">
          <div class="col-md-4 mb-5 mb-lg-0">
          </div>
          <div class="col-md-4 mb-5 mb-lg-0">
            <h4 class="text-uppercase mb-4">Around the Web</h4>
            <ul class="list-inline mb-0">
              <li class="list-inline-item">
                <a class="btn btn-outline-light btn-social text-center rounded-circle" href="#">
                  <i class="fab fa-fw fa-facebook-f"></i>
                </a>
              </li>
              <li class="list-inline-item">
                <a class="btn btn-outline-light btn-social text-center rounded-circle" href="#">
                  <i class="fab fa-fw fa-twitter"></i>
                </a>
              </li>
              <li class="list-inline-item">
                <a class="btn btn-outline-light btn-social text-center rounded-circle" href="www.linkedin.com/in/daviddimperio">
                  <i class="fab fa-fw fa-linkedin-in"></i>
                </a>
              </li>
            </ul>
          </div>
          <div class="col-md-4">
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<style>

footer {
  width: 100%;
}




</style>
