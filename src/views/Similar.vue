<template>
  <div >
    <!-- input for search -->
    <div class="p-5 mb-4 bg-light rounded-3">

      <div class="container-fluid py-5">
        <h1 class="display-3">Find similar documents</h1>

        <!-- <p class="lead">Enter Doc ID</p> -->
        <input class="form-control form-control-lg mb-4" type="text" id="message"
        v-model="selectedDocID"
          placeholder="Enter Doc ID" v-on:keyup="getResults" style="background-color: white" />

        <div class="row">

          <p class="lead">
            <a class="btn btn-primary btn-lg" href="#" role="button"
              @click="getResults('button')">Search</a>
          </p>
        </div>
      </div>

    </div>

    <!-- results -->

<ul class="list-group" v-for="item in results" v-bind:key="item.id">
  <li class="list-group-item d-flex justify-content-between align-items-center">
    <div>

   {{ titleCase(item.document) }}
    <span class="badge bg-primary rounded-pill">{{item.type}}</span>
    <br/>
   <small class="text-muted">Score: {{item.score.toFixed(3)}}</small>
    </div>

  </li>

</ul>
    <div class="alert alert-dismissible alert-warning results" v-if="results.length === 0">
      Oh no 😢. No results found!
    </div>
  </div>
</template>

<script>
export default {
  name: 'LogicMillSimilar',
  props: {
    docid: String,
  },
  data() {
    return {
      results: [],
      API_ENDPOINT: 'https://api.logic-mill.net',
      INDEX: 'logic_mill_initial',
      indices: [],
      selectedDocID: '',
      selectedIndex: '',
      amount: 10,
    };
  },
  created() {
    this.selectedDocID = this.$route.params.docid;
    if (this.selectedDocID !== '') {
      this.getResults('button');
    }
  },
  methods: {
    titleCase(str) {
      return str.split(' ').map((word) => word.replace(word[0], word[0].toUpperCase())).join(' ');
    },
    getResults(e) {
      if (e.keyCode === 13 || e === 'button') {
        this.results = {};

        const currentIndex = this.selectedIndex !== '' ? this.selectedIndex : this.INDEX;
        const searchURL = `${this.API_ENDPOINT}/indices/${currentIndex}/doc/${this.selectedDocID}`
        + `/similarity?amount=${this.amount}`;

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
            const { hits } = response.hits;

            const tmpResults = [];

            hits.forEach((r) => {
              // eslint-disable-next-line no-underscore-dangle
              const id = r._id;
              // eslint-disable-next-line no-underscore-dangle
              const score = r._score - 1;
              // eslint-disable-next-line no-underscore-dangle
              const { document } = r._source;
              // eslint-disable-next-line no-underscore-dangle
              const { type } = r._source.metadata;
              tmpResults.push({
                id, document, type, score,
              });
            });
            this.results = tmpResults;
          })
          .catch((error) => {
            // eslint-disable-next-line
              console.log('Error');
            // eslint-disable-next-line
              console.log(error);
            this.results = [];
          });
      }
    },
  },
};

</script>

<style scoped>
  .card_containter {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: flex-start;
    flex-direction: row;
    align-content: stretch;
  }

  .card {
    flex: flex-basis;
    align-self: auto;
    width: 350px;
    margin: 25px;
    /* max-height: 500px; */
  }

  .card-header {
    overflow: hidden;
    max-height: 200px;
    font-size: 1.6em;
    font-style: bold;
  }

  .card-text {
    max-height: 200px;
    overflow: hidden;
    display: -webkit-box;
    -webkit-line-clamp: 5;
    -webkit-box-orient: vertical;
  }

  .results {
    text-align: center;
  }
  .similar {
    font-size:20px;
    font-weight: bolder;
    /* padding-top: 0px; */
    /* margin: 0px; */
    /* padding-bottom: 0px; */
    /* padding: 0px; */
  }
</style>
