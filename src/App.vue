<template>
  <div>
    <div class="container border-bottom pb-5">
      <div class="row mb-5">
        <h1 class="fw-bold display-5">Breweries in Milwaukee</h1>
        <p class="fs-4">A dropdown filter that filters the breweries in Milwaukee by brewery type</p>
      </div>
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

            <button class="btn btn-success" type=submit><span class="text">Submit</span></button>

          </form>
        </div>

        <div class="col-md-3"></div>
      </div>

    </div>

    <h3 v-if="error" class="alert alert-danger">{{ error }}</h3>

    <div v-if="data != null" class="container mt-5 d-flex flex-wrap mb-5">
      <Card v-for="item in data" :key="item.id" :item="item" />
    </div>

    <div class="m-3 fw-bold" v-if="data === null">No data yet...</div>
    <div class="m-3 fw-bold" v-if="data != null && data.length === 0">No data found...</div>
  </div>
</template>

<script>
import { ref } from 'vue';
import Card from './components/Card.vue';


export default {
  name: "App",
  components: {
    Card
  },
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
  },

}
</script>


<style>
body {
  background-color: #fdf6dc !important;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

button {
  position: relative;
  overflow: hidden;
  background-color: #4ca728 !important;
  border: none !important;
  padding: .75rem 1rem !important;
  height: 52px;
  width: 106px;
}

button,
button:before {
  transition: all 0.35s;
  transition-timing-function: cubic-bezier(0.31, -0.105, 0.43, 1.59);
}

.text {
  font-weight: 600;
  transition: all 0.35s;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

button:hover::before,
button:focus::before {
  top: -62%;
  left: -10%;
}

button:hover .text {
  font-size: 18px;
}

button:before {
  content: "";
  width: 259%;
  height: 420%;
  position: absolute;
  transform: rotate(67deg);
  top: -53%;
  left: -250%;
  background: #60cf33;
}
</style>
