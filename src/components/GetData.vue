<script setup>
import {onMounted, ref} from "vue";
// import Map from "@/components/Map.vue";
const API = ref("https://restcountries.com/v3.1/name/");
const srch = ref('');
let data = ref(null);
let curr = ref(null);
let currency = ref('');
import Map from './Map.vue';
let lat = ref(0);
let lon = ref(0);




function fetchData() {
  //reset data
  data.value = null;
  if(srch.value.trim() != ''){
    fetch(`${API.value}${srch.value}`)
        .then(res => res.json())
        .then(d => {
          console.log(d);
          if(d !== null){
            data.value = d[0];
            console.log(data.value.area)
          }else{
            console.log("Nic nebylo nalezeno");
            data = null;
          }
        })
        .catch(err => console.log(err));
  }
  else{
    console.log("Nic nebylo zadáno")
  }
}
function fetchCurrencies(){
    fetch("https://cdn.jsdelivr.net/npm/@fawazahmed0/currency-api@latest/v1/currencies/eur.json")
        .then(res => res.json())
        .then(d => {
          console.log(d);
          curr.value = d;
        })
        .catch(err => console.log(err));
}
onMounted(() => {
  fetchCurrencies();
});
</script>


<template>
  <div>
    <div id="search">
      <input type="text" v-model="srch" class="form-label">
      <button @click.prevent="fetchData" type="button" class="btn">🔍</button>
    </div>
  </div>
  <div id="dataContainer" v-if="data" >
      <div class="card" style="width: 18rem;">
        <h2>{{ data.name.official }}</h2>
        <p>Capital: {{ data.capital[0] }}</p>
        <p>Population: {{ data.population }}</p>
        <p>Area: {{data.area}} km^2</p>
        <img :src="data.flags.svg" alt="Vlajka" width="100">
        <img class="coat" :src="data.coatOfArms.svg" alt="Znak armády" width="50">
        <div style="display: none">
          {{lat = data.capitalInfo.latlng[0]}}
          {{lon = data.capitalInfo.latlng[1]}}
        </div>
      </div>
      <div class="card" v-if="data.borders" style="width: 18rem;">
        <h2>Neighbours</h2>
        <ul v-for="i in data.borders" :key="i">
          {{i}}
        </ul>
      </div>
      <div class="card" style="width: 18rem;">
        <h2>Currency</h2>
        <p>{{Object.keys(data.currencies)[0].valueOf()}}</p>
        <div style="display: none">
          {{currency = Object.keys(data.currencies)[0].toLocaleLowerCase()}}
        </div>
        <p>1 EUR = {{curr.eur[currency].toFixed(2)}} {{currency.toUpperCase()}}</p>
      </div>

    <div class="card" id="map" style="display: none">
      {{lat}}
      {{lon}}
      <Map :lat = lat :lon = lon />
    </div>
  </div>


  <!-- if not data -->
  <div v-if="data && data.status == 404">Nic nebylo nalezeno</div>

</template>

<style scoped>
*{
  border-radius: 4px;
}

</style>