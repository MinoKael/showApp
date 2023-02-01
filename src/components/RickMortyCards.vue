<script setup>
import axios from 'axios';
import { ref, watch, onMounted } from 'vue';
import Card from './Card.vue';

const characters = ref(null);
const page = ref(1);

onMounted(async () => {
  const response = await axios.get('https://rickandmortyapi.com/api/character');
  characters.value = response.data.results;
});

// WATCH //
watch(page, async () => {
  const res = await axios.get(
    `https://rickandmortyapi.com/api/character/?page=${page.value}`
  );
  console.log(res);
  characters.value = res.data.results;
});
// ======= //
</script>
<template>
  <div class="container">
    <div v-if="characters" class="cards">
      <Card
        v-for="characters in characters"
        :key="characters.id"
        :image="characters.image"
        :name="characters.name"
      >
        <slot>
          <p>{{ characters.location.name }}</p>
        </slot></Card
      >
    </div>
    <div v-else class="cards spinner">
      <NSpin size="large" />
    </div>
    <div class="button-container">
      <button @click="if (page > 1) page--;">&lt</button>
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
  /* height: 700px; */
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
