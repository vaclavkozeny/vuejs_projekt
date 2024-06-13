<script setup>
import {ref} from "vue";
// import Map from "@/components/Map.vue";
const API = ref("https://restcountries.com/v3.1/name/");
const srch = ref("");
let data = ref(null);
let curr = ref(null)

function fetchData() {
  if(srch.value.trim() != ''){
    fetch(`${API.value}${srch.value}`)
        .then(res => res.json())
        .then(d => {
          console.log(d);
          data.value = d;
        })
        .catch(err => console.log(err));
  }
  else{
    console.log("Nic nebylo zadáno")
  }
}
function fetchCurrencies(){
  if(srch.value.trim() != ''){
    fetch("https://cdn.jsdelivr.net/npm/@fawazahmed0/currency-api@latest/v1/currencies/eur.json")
        .then(res => res.json())
        .then(d => {
          console.log(d);
          curr.value = d;
        })
        .catch(err => console.log(err));
  }
  else{
    console.log("Nic nebylo zadáno")
  }
}
</script>


<template>
  <div id="search">
    <input type="text" v-model="srch">
    <button @click.prevent="fetchData">🔍</button>
  </div>
  <div id="dataContainer" v-if="data">
    <div v-for="country in data" :key="country.name">
      <div class="card">
        <h2>{{ country.name.official }}</h2>
        <p>Hlavní město: {{ country.capital[0] }}</p>
        <p>Počet obyvatel: {{ country.population }}</p>
        <p>Rozloha: {{country.area}} km^2</p>
        <img :src="country.flags.svg" alt="Vlajka" width="100">
      </div>
      <div class="card">
        <ul v-for="i in country.borders" :key="i">
          {{i}}
        </ul>
      </div>
      <div class="card">
        <p>{{country.currencies}}</p>
      </div>

<!--      <Map :longitude="country.capitalInfo.latlng[0]" :latitude="country.capitalInfo.latlng[1]"/>-->
    </div>

  </div>

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
  justify-content: center;
  margin-top: 3rem;
  color: white;
  border: 2px solid purple;
  padding: 5px;

}
#search{
  display: flex;
  flex-direction: row;
}
</style>