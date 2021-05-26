<template>
  <div id="app">

<nav class="navbar navbar-expand-lg navbar-light">
  <div class="container-fluid">
    <a class="navbar-brand logo" href="#"><img src="@/assets/logo1024.png"
    alt="Logic Mill Logo" width="200px"></a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
    data-bs-target="#navbarColor01" aria-controls="navbarColor01"
    aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarColor01">
      <ul class="navbar-nav me-auto">
        <li class="nav-item">
          <router-link class="nav-link" to="/">Search</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/about">About</router-link>
        </li>

      </ul>

    </div>
  </div>
</nav>

    <router-view/>

  </div>
</template>

<script>

export default {
  name: 'LogicMill',
  components: {
  },
};
</script>

<style scoped>
.bg-primary,
.btn-primary,
.btn-primary:hover {

    background-color: #00479F !important;
    border-color: #00479F !important;
}

.logo{
  padding-left: 50px;
}
</style>
