<script lang="ts" setup>
defineProps<{
  id: number
}>()

function pokeSprite(id: number): string {
  const url = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`
  console.log(`looking up ${url}`)
  return url
}
</script>

<template>
  <q-img :src="pokeSprite(id)" fit="fill"/>
</template>

<style scoped>
.q-img {
  image-rendering: pixelated;
}
</style>
