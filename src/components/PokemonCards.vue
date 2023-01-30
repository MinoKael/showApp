<script setup>
import axios from 'axios';
import { ref, watch } from 'vue';

const pokemon = ref([]);
const page = ref(0);

const response = await axios.get(
  'https://pokeapi.co/api/v2/pokemon?limit=8&offset=0'
);
const fetchData = async url => {
  return await axios.get(`${url}`);
};
response.data.results.map(el =>
  fetchData(el.url).then(data => {
    pokemon.value.push(data);
  })
);
console.log(pokemon.value);

watch(page, async () => {
  const res = await axios.get(
    `https://pokeapi.co/api/v2/pokemon?limit=8&offset=${page.value * 8}`
  );
  pokemon.value = [];
  res.data.results.map(el =>
    fetchData(el.url).then(data => {
      pokemon.value.push(data);
    })
  );
});
</script>
<template>
  <div class="container">
    <div class="cards">
      <ul>
        <li v-for="pokemon in pokemon" :key="pokemon.data.id">
          {{ pokemon.data.name }}
          <img src="" alt="" />
        </li>
      </ul>
      <button @click="page = page + 1">Next</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  background-color: rgb(27, 26, 26);
  padding: 30px;
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
