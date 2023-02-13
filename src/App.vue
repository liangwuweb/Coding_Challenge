<template>
  <div>
    <!-- <form>
      <label>City</label>
      <select name="cars" id="cars">
        <option value="all">All</option>
        <option value="saab">Saab</option>
        <option value="mercedes">Mercedes</option>
        <option value="audi">Audi</option>
      </select>
      <label>Type</label>
      <select name="cars" id="cars">
        <option value="all">All</option>
        <option value="saab">Saab</option>
        <option value="mercedes">Mercedes</option>
        <option value="audi">Audi</option>
      </select>
      <button type=submit>Submit</button>
    </form> -->
  </div>
</template>

<script>
// import {ref, onMounted, coumpted, watch} from 'vue';
// import axios from 'axios';

export default {
  name: "App",
  data() {
    return {
      results: null,
    };
  },
  setup() {
    console.log("hello world");
  },
  methods: {
    async fetchData() {
      const response = await fetch("https://api.openbrewerydb.org/breweries");
      this.result = await response.json();
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
