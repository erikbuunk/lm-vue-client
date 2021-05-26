<template>
  <div id="app">

<nav class="navbar navbar-expand-lg navbar-dark bg-primary">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Logic Mill</a>
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
// import LogicMillSearch from './components/LogicMillSearch.vue';

export default {
  name: 'LogicMill',
  components: {
    // LogicMillSearch,
  },
};
</script>

<style>
</style>
