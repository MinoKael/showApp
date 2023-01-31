<script setup>
const { image, name, types, id } = defineProps([
  'image',
  'name',
  'types',
  'id'
]);
const capitalize = word => {
  return word.charAt(0).toUpperCase() + word.slice(1);
};
</script>

<template>
  <NCard>
    <template #cover>
      <img :src="image" />
    </template>
    <h3>{{ capitalize(name) }}</h3>
    <div class="jobs">
      <p v-for="(type, index) in types" :key="id + type.type.name">
        {{ capitalize(type.type.name)
        }}<span v-if="index < types.length - 1">,&nbsp</span>
      </p>
    </div>
  </NCard>
</template>
<style scoped>
.n-card {
  width: 200px;
  margin: 10px 20px;
}
.n-card img {
  height: 250px;
}

p {
  font-size: 10px;
}

.jobs {
  display: flex;
  flex-wrap: wrap;
}
</style>
