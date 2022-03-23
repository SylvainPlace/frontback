<script setup>
import { reactive } from "vue";
// @ is an alias to /src
import CityListSansPays from "@/components/CityListSansPays.vue";
import CountrySelect from "@/components/CountrySelect.vue";

const data = reactive({
  cities: [],
});


function handlerSelect() {
  fetchCities(document.getElementById("sel").value);
}


function fetchCities(url) {
  // Utilise l'API ad-hoc pour avoir le pays de chaque ville
  fetch(url)
    .then((response) => response.json())
    .then((json) => {
      data.cities = json._embedded.cities;
    })
    .catch((error) => alert(error));
}


</script>

<template>
  <div class="home">
    <h2>Villes dans chaque pays</h2>
      <CountrySelect @eventSelect="handlerSelect" />
      
    <hr>
      <CityListSansPays :cities="data.cities" />
  </div>
</template>