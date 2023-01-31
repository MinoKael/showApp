<script setup>
import axios from 'axios';
import { ref, watch, onMounted } from 'vue';
import Card from './Card.vue';

const digimon = ref([]);
const page = ref(1);

onMounted(async () => {
  const response = await axios.get(
    'https://www.digi-api.com/api/v1/digimon?pageSize=8'
  );
  fetchData(response.data.content, digimon.value);
});

const fetchData = (endpoints, storeData) => {
  Promise.all(endpoints.map(endpoint => axios.get(endpoint.href))).then(data =>
    storeData.push(...data)
  );
};

// WATCH //
watch(page, async () => {
  const res = await axios.get(
    `https://www.digi-api.com/api/v1/digimon?pageSize=8&page=${page.value}`
  );
  digimon.value = [];
  fetchData(res.data.content, digimon.value);
});
// ======= //
</script>
<template>
  <div class="container">
    <div class="cards">
      {{ digimon }}
      <!--<Card
        v-for="pokemon in pokemon"
        :key="pokemon.data.order"
        :image="pokemon.data.sprites.other['official-artwork'].front_default"
        :name="pokemon.data.name"
        :types="pokemon.data.types"
        :id="pokemon.data.order"
      /> -->
    </div>
    <div class="button-container">
      <button @click="if (page > 0) page--;">&lt</button>
      <button @click="page++">&gt</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  background-color: rgb(27, 26, 26);
  padding: 30px;
  margin-top: 100px;
}
.cards {
  max-width: 1000px;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  height: 700px;
}
.cards h3 {
  font-weight: bold;
}
.cards p {
  font-size: 10px;
}
.jobs {
  display: flex;
  flex-wrap: wrap;
}
.button-container {
  display: flex;
  justify-content: center;
  padding-top: 30px;
}
.button-container button {
  border: none;
  width: 50px;
  height: 50px;
  border-radius: 100%;
  margin: 0 5px;
  cursor: pointer;
}
.spinner {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
