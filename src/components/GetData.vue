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
            data.value = d;
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
      <input type="text" v-model="srch">
      <button @click.prevent="fetchData">🔍</button>
    </div>
  </div>
  <div id="dataContainer" v-if="data && data.length > 0">
    <div id="innerContainer" v-for="country in data" :key="country.name">
      <div class="card">
        <h2>{{ country.name.official }}</h2>
        <p>Hlavní město: {{ country.capital[0] }}</p>
        <p>Počet obyvatel: {{ country.population }}</p>
        <p>Rozloha: {{country.area}} km^2</p>
        <img :src="country.flags.svg" alt="Vlajka" width="100">
        <img class="coat" :src="country.coatOfArms.svg" alt="Znak armády" width="50">
        <div style="display: none">
          {{lat = country.capitalInfo.latlng[0]}}
          {{lon = country.capitalInfo.latlng[1]}}
        </div>
      </div>
      <div class="card" v-if="country.borders">
        <h2>Sousedé</h2>
        <ul v-for="i in country.borders" :key="i">
          {{i}}
        </ul>
      </div>
      <div class="card">
        <h2>Měna</h2>
        <p>{{Object.keys(country.currencies)[0].valueOf()}}</p>
        <div style="display: none">
          {{currency = Object.keys(country.currencies)[0].toLocaleLowerCase()}}
        </div>
        <p>1 EUR = {{curr.eur[currency].toFixed(2)}} {{currency.toUpperCase()}}</p>
      </div>

    </div>
    <div class="card" id="map">
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
input{
  border: 2px solid purple;
}
button{
  background-color: #282828;
  border: 1px solid purple;
  margin-left: 3px;
}
.card{
  width: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  margin-top: 3rem;
  color: white;
  border: 2px solid purple;
  padding: 5px;
  margin-left: 0;
  margin-right: 0.5rem;
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
}
#search{
  display: flex;
  flex-direction: row;
}
#dataContainer{
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
}
#innerContainer {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  margin: 0;
}
ul{
  align-self: center;
  padding: 0;
}
#map{
  margin-left: 0px;
}
.coat{
  margin-top: 1rem;
}
@media (max-width: 768px) {
  .card {
    width: 100%;
  }

  #dataContainer {
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  #map {
    margin-left: 0px;
  }
}
</style>