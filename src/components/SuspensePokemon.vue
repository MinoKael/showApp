<script setup>
import PokemonCards from './PokemonCards.vue';
</script>
<template>
  <Suspense>
    <template #default><PokemonCards /></template>
    <template #fallback>
      <div class="cards">
        <NSpin size="large" />
      </div>
    </template>
  </Suspense>
</template>
<style scoped>
.cards {
  height: 700px;
  background-color: rgb(27, 26, 26);
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
