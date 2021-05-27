<template>
  <div >
    <!-- input for search -->
    <div class="p-5 mb-4 bg-light rounded-3">

      <div class="container-fluid py-5">
        <h1 class="display-3">Encode your own</h1>

          <label for="title" class="form-label mt-4">Title</label>
        <input class="form-control form-control-lg mb-4" type="text" id="title" v-model="title"
          placeholder="Enter Title" v-on:keyup="getResults" style="background-color: white" />

      <div class="form-group">
          <label for="abstract" class="form-label mt-4">Abstract</label>
          <textarea class="form-control" id="abstract" rows="10"
          spellcheck="false" placeholder="Copy Abstract" v-model="abstract"></textarea>
        </div>

    <br>
        <div class="row">

          <p class="lead">
            <a class="btn btn-primary btn-lg" href="#" role="button"
              @click="getResults('button')">Encode</a>
          </p>
        </div>
      </div>

    </div>

    <!-- results -->
    <div  v-if="result !== ''">

      <label for="vector" class="form-label mt-4">Embedding</label>
      <textarea class="form-control" id="vector" rows="10"
          spellcheck="false" placeholder="Resulting vector" v-model="result"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Encode',
  data() {
    return {
      result: '',
      API_ENDPOINT: 'https://api.logic-mill.net',
      INDEX: 'logic_mill_initial',
      selectedIndex: '',
      title: 'Inventive Step in European Patent Applications - Evidence from an Inventor Survey',
      abstract: `The importance of the inventive-step or non-obviousness criterion in patent
examination is widely acknowledged. Yet, there has been very little work seeking to
measure and analyze the inventive step of patented inventions empirically. In this paper,
we use data from a large-scale survey in which European inventors were asked to rate the
inventive step of their inventions. The paper discusses potential antecedents of inventive
step such as applicant size and type, characteristics of the inventor team, as well as
subsequent consequences such as the intended use of the invention, its value and
indicators relating to the patent’s paper trail in the examination process. Inventive
step is found to be strongly related to inventor and organizational characteristics,
as well as to some indicators obtained from search reports. Surprisingly, there is no
strong relationship between inventive step as survey in our data and examination outcomes.`,
    };
  },
  created() {
    // this.getStatus();
  },
  methods: {
    getResults(e) {
      if (e.keyCode === 13 || e === 'button') {
        this.results = {};

        const currentIndex = this.selectedIndex !== '' ? this.selectedIndex : this.INDEX;
        const searchURL = `${this.API_ENDPOINT}/indices/${currentIndex}/encode/bulk`;

        const documents = [{
          id: currentIndex,
          metadata: {},
          document: this.title,
          documentParts: {
            title: this.title,
            abstract: this.abstract,
          },
        }];

        const dataSearch = { data: documents };
        const body = JSON.stringify(dataSearch);

        const request = new Request(searchURL, {
          method: 'PUT',
          body,
        });

        fetch(request)
          .then((response) => {
            if (response.status === 200) {
              return response.json();
            }
            throw new Error('Something went wrong on api server!');
          })
          .then((response) => {
            // console.log(response[0]);
            this.result = response[0].vector;
          })
          .catch((error) => {
            // eslint-disable-next-line
              console.log('Error');
            // eslint-disable-next-line
              console.log(error);
          });
      }
    },
  },
};

</script>

<style scoped>

</style>
