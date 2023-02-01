<script setup>
import SuspensePokemon from './components/SuspensePokemon.vue';
import RickMortyCards from './components/RickMortyCards.vue';
import Hero from './components/Hero.vue';
import { ref } from 'vue';

const isPokemon = ref(true);
</script>

<template>
  <main>
    <Hero :isPokemon="isPokemon" @selectShow="isPokemon = !isPokemon" />
    <KeepAlive>
      <Component :is="isPokemon ? SuspensePokemon : RickMortyCards" />
    </KeepAlive>
  </main>
</template>
