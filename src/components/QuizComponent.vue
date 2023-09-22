<script lang="ts" setup>

import PokemonView from 'components/PokemonView.vue';

const id1 = '' + (Math.floor(Math.random() * 151) + 1)
const id2 = '' + (Math.floor(Math.random() * 151) + 1)

</script>

<template>
  <div class="full-width col q-mt-none">
    <div class="row-12">
      <h2>Who is heavier?</h2>
    </div>
    <div class="row-12">
      <PokemonView :id1="id1" :id2="id2"/>
    </div>
  </div>
</template>

<style scoped>
.row-12 {
  text-align: center;
}

h1 {
  font-family: 'Roboto', sans-serif;
}
</style>
