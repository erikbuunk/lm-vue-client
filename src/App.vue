<template>
  <div id="app" class="container">

    <nav class="navbar navbar-expand-lg navbar-light">
      <div class="container-fluid">
        <a class="navbar-brand logo" href="#">
          <img src="@/assets/logo1024.png" alt="Logic Mill Logo" width="200px"></a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
          data-bs-target="#navbarColor01" aria-controls="navbarColor01" aria-expanded="false"
          aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarColor01">
          <ul class="navbar-nav me-auto">
            <li class="nav-item">
              <router-link class="nav-link" to="/">Search</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/similar">Similar</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/encode">Encode</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/about">About</router-link>
            </li>
          </ul>

          <form class="d-flex">
            <div class="form-group">
              <select class="form-select" id="indexSelect" v-model="selectedIndex">
                <option disabled value="">Please select Index</option>
                <option v-for="index in indices" v-bind:value="index.value" :key="index.id">
                  {{ index.text }}
                </option>
              </select>

            </div>
        </form>
        </div>
      </div>
    </nav>

    <router-view />

  </div>
</template>

<script>
export default {
  name: 'LogicMill',
  components: {},
  data() {
    return {
      indices: [],
      selectedIndex: '',
      API_ENDPOINT: 'https://api.logic-mill.net',
    };
  },
  created() {
    // this.getStatus();
    this.getIndices();
  },
  methods: {
    getIndices() {
      const searchURL = `${this.API_ENDPOINT}/indices`;

      const request = new Request(searchURL, {
        method: 'GET',
      });

      fetch(request)
        .then((response) => {
          if (response.status === 200) {
            return response.json();
          }
          throw new Error('Something went wrong on api server!');
        })
        .then((response) => {
          response.forEach((r) => {
            if (r.index[0] !== '.') {
              this.indices.push({ id: r.uuid, value: r.index, text: `${r.index} (${r['docs.count']} documents)` });
            }
          });
          this.selectedIndex = 'logic_mill_initial';
        })
        .catch((error) => {
          // eslint-disable-next-line
           console.log(error);
        });
    },
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

</style>
