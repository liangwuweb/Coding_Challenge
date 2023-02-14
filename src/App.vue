<template>
  <div>
    <div class="container border-bottom pb-5">
      <div class="row">
        <div class="col-md-3"></div>

        <div class="col-md-6">
          <form id="main-form" @submit.prevent="handleSubmit">

            <div class="input-group mb-3">
              <label class="input-group-text" for="city">City</label>
              <select class="form-select inline-block" aria-label="Default select example" name="city" id="city"
                v-model="selectedCity">
                <option value="milwaukee">Milwaukee</option>
              </select>
            </div>

            <div class="input-group mb-3">
              <label class="input-group-text" for="type">Type</label>
              <select class="form-select" aria-label="Default select example" name="type" id="type"
                v-model="selectedType">
                <option value="all">All</option>
                <option v-for="item, index in breweryType" :value="item" :key="index">
                  {{ item }}
                </option>
              </select>
            </div>

            <button class="btn btn-success" type=submit>Submit</button>

          </form>
        </div>

        <div class="col-md-3"></div>
      </div>

    </div>

    <h3 v-if="error" class="alert alert-danger">{{ error }}</h3>

    <div v-if="data != null" class="container mt-5 d-flex flex-wrap">
      <div  v-for="item in data" :key="item.id" class="card m-2" style="width: 18rem;">
        <div class="card-body">
          <h5>{{ item.name }}</h5>
          <ul class="list-group list-group-flush">
            <li class="list-group-item">{{ item.brewery_type }}</li>
            <li class="list-group-item">street: {{ item.street }}</li>
            <li class="list-group-item">phone: {{ item.phone }}</li>
          </ul>
          
          <a target="_blank" :href="item.website_url != null ?  item.website_url : '#'" :class="item.website_url != null ?  'btn btn-primary' : 'd-none'">website</a> 
        </div>
      </div>
      
    </div>
    <div class="m-3" v-if="data === null">No data yet...</div>
    <div class="m-3" v-if="data != null && data.length === 0">No data found...</div>
  </div>
</template>

<script>
import { ref } from 'vue';


export default {

  setup() {
    let data = ref(null);
    let selectedCity = ref('milwaukee'),
      selectedType = ref('all');
    let error = ref('');

    const breweryType = ['micro', 'nano', 'regional', 'brewpub', 'large', 'planning', 'bar', 'contract', 'proprieter', 'closed'];

    // event handler for sumbit
    const handleSubmit = () => {
      const type = selectedType.value;

      // filter data based on type
      if (type === 'all')
        fetchData(`https://api.openbrewerydb.org/breweries?by_city=milwaukee`);
      else {
        fetchData(`https://api.openbrewerydb.org/breweries?by_city=milwaukee&by_type=${type}`);
      }
    }

    // Fetch data from api
    const fetchData = async (url) => {
      try {
        const response = await fetch(url);
        data.value = await response.json();
      } catch (err) {
            console.log(err);
            error.value = 'invalid type';
      }
    }

    return {
      selectedCity,
      selectedType,
      data,
      breweryType,
      error,
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
</style>
