<script setup>
import PokemonCards from './components/PokemonCards.vue';
import DigimonCards from './components/DigimonCards.vue';
</script>

<template>
  <main>
    <h1>HERO</h1>
    <Suspense>
      <template #default><PokemonCards /></template>
      <template #fallback>
        <div>
          <p>Loading...</p>
        </div>
      </template>
    </Suspense>
    <DigimonCards />
  </main>
</template>
