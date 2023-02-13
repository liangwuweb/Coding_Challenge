<template>
  <div>
    <div class="container">
      <form id="main-form" @submit.prevent="handleSubmit">
        <div class="input-group mb-3">
          <label class="input-group-text" for="city">City</label>
          <select class="form-select inline-block" aria-label="Default select example" name="city" id="city" v-model="selectedCity">
            <option value="milwaukee">Milwaukee</option>
          </select>
        </div>


        <div class="input-group mb-3">
          <label class="input-group-text" for="type">Type</label>
          <select class="form-select" aria-label="Default select example" name="type" id="type" v-model="selectedType">
            <option value="all">All</option>
            <option v-for="item, index in breweryType" :value="item" :key="index">
              {{ item }}
            </option>

            
          </select>
        </div>
        <button class="btn btn-success" type=submit>Submit</button>
      </form>
    </div>

    
    <div class="wrapper">
      <p v-for="item in data" :key="item.id">{{ item.name }}</p>
    </div>
  </div>
</template>

<script>
import {ref, onMounted} from 'vue';


export default {

  setup() {

    let results = ref(null);
    let data = ref(null);
    let selectedCity = ref('milwaukee'),
        selectedType = ref('all');

    const breweryType = ['micro', 'nano', 'regional', 'brewpub', 'large', 'planning','bar', 'contract', 'proprietor', 'closed'];

    const handleSubmit = () => {
      const type = selectedType.value;

      // filter data based on type
      if (type === 'all') 
        fetchData(`https://api.openbrewerydb.org/breweries?by_city=milwaukee`);
       else 
        fetchData(`https://api.openbrewerydb.org/breweries?by_city=milwaukee&by_type=${type}`);
      
    }

    const fetchData = async(url) => {
      const response = await fetch(url);
      data.value = await response.json();
    }

    onMounted(async() => {
      const response = await fetch("https://api.openbrewerydb.org/breweries?by_city=milwaukee");
      results.value = await response.json();
    })

    return {
      selectedCity,
      results,
      selectedType,
      data,
      breweryType,
      handleSubmit
    }
  }
}
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


  .form-select {
    width: 200px !important;
  }

</style>
