<template>
  <div class="app">
    <div class="nav">
      <h1 class="title">Wikipedia Viewer</h1>
    </div>
    <div class="center">
      <p>click <a href="https://en.wikipedia.org/wiki/Special:Random" target="_blank" class="random">here</a> to read a random article.</p>
      <form :class="{active: active}" v-on:submit.prevent="handleFormSubmit">
        <input type="text" v-on:focus="toggleActive" v-on:blur="toggleActive" v-model="searchField">
      </form>
      <p>Click the icon to search</p>
    </div>
    <div class="results">
      <div v-if="isLoading" class="loading-icon-parent">
        <div class="loading-icon"></div>
      </div>
      <div v-else-if="isError">
        <h6>Error Loading results from wikipedia, please try again... </h6>
      </div>
      <div v-else-if="!isLoading" class="result-list">
        <Results v-for="(result, index) in results" :key="result.pageid" :result="result" :index="index"></Results>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from 'axios';
import Results from './components/Results';

export default {
  name: 'app',
  components: {
    Results: Results
  },
  data () {
    return {
      active: false,
      searchField: "",
      isLoading: false,
      results: null,
      isError: false,
    }
  },
  methods: {
    toggleActive () {
      this.active = !this.active;
      if (this.searchField.length > 0) this.searchField = "";
    },
    async handleFormSubmit () {
      this.results = null;
      this.isError = false;
      let searchTerm = this.searchField;
      this.searchField = "";
      this.isLoading = true;
      let res = await Axios.get(`https://cors-anywhere.herokuapp.com/https://en.wikipedia.org/w/api.php?action=query&format=json&list=search&utf8=1&srsearch=${encodeURI(searchTerm)}`);
      console.log(res);
      if (!res.data.query) {
        this.isLoading = false;
        this.isError = true;
        return;
      }
      this.isLoading = false;
      this.results = res.data.query.search;
    }
  }
}
</script>

<style>
</style>
