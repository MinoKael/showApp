<script setup>
import axios from 'axios';
import { ref, watch, toRaw } from 'vue';
import Card from './Card.vue';

const pokemon = ref([]);
const page = ref(0);

const response = await axios.get(
  'https://pokeapi.co/api/v2/pokemon?limit=8&offset=0'
);

// FETCH AND PUSH DATA TO POKEMON VARIABLE //
const fetchData = (endpoints, storeData) => {
  Promise.all(endpoints.map(endpoint => axios.get(endpoint.url))).then(data =>
    storeData.push(...data)
  );
};
fetchData(response.data.results, pokemon.value);
// ======= //

// WATCH //
watch(page, async () => {
  const res = await axios.get(
    `https://pokeapi.co/api/v2/pokemon?limit=8&offset=${page.value * 8}`
  );
  pokemon.value = [];
  fetchData(res.data.results, pokemon.value);
});
const capitalize = word => {
  return word.charAt(0).toUpperCase() + word.slice(1);
};
// ======= //
</script>
<template>
  <div class="container">
    <div class="cards">
      {{ pokemon.value }}
      <Card
        v-for="pokemon in pokemon"
        :key="pokemon.data.order"
        :image="pokemon.data.sprites.other['official-artwork'].front_default"
        :name="capitalize(pokemon.data.name)"
      >
        <slot
          ><div class="jobs">
            <p
              v-for="(type, index) in pokemon.data.types"
              :key="pokemon.data.order + type.type.name"
            >
              {{ capitalize(type.type.name)
              }}<span v-if="index < pokemon.data.types.length - 1">,&nbsp</span>
            </p>
          </div></slot
        >
      </Card>
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
p {
  font-size: 10px;
}

.jobs {
  display: flex;
  flex-wrap: wrap;
}
</style>
