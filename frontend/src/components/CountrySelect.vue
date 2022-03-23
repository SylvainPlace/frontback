<template>
    <div class="container mt-3">
        <select id="sel" class="form-control"  @change="$emit(`eventSelect`)">
            <option disabled value="0" selected>Choisissez un pays</option>
          <option v-for="country in data.countries" :key="country.id" :value="country._links.cities.href">{{ country.name }}</option>
        </select>
    </div>
</template>

<script setup>
import { reactive, onMounted } from "vue";

// Déclaration des variables de la vue
let data = reactive({
    countries: [],
});

defineExpose({ // On expose la méthode 'refresh' pour être utilisée par le parent
    refresh,
})

function refresh() {
    fetch("api/countries")
        .then((response) => {
            if (!response.ok) { // status != 2XX
                throw new Error(response.status);
            }
            return response.json();
        })
        .then((json) => {
            data.countries = json._embedded.countries;
        })
        .catch((error) => alert(error));
}

onMounted(refresh);
</script>
