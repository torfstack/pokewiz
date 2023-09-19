<script lang="ts" setup>
import {ref} from 'vue';
import PokeIcon from 'components/PokeIcon.vue';

let id1 = randomPokemon()
let id2 = randomPokemon()

let btn1Classes = ref('')
let btn2Classes = ref('')

async function submitChoice() {
  const weights: number[] = await Promise.all(
    [
      weightOfId(id1),
      weightOfId(id2)
    ]
  )

  const maxIndex = weights.indexOf(Math.max(...weights))
  if (0 === maxIndex) {
    btn1Classes.value = 'correct'
    btn2Classes.value = 'wrong'
  } else {
    btn1Classes.value = 'wrong'
    btn2Classes.value = 'correct'
  }

  setTimeout(() => {
    reset()
  }, 1000)
}

function randomPokemon(): string {
  return '' + Math.floor(Math.random() * 151)
}

function reset() {
  id1 = randomPokemon()
  id2 = randomPokemon()
  btn1Classes.value = ''
  btn2Classes.value = ''
}

async function weightOfId(id: string): Promise<number> {
  const base = 'https://pokeapi.co/api/v2/pokemon/'
  let resp = await fetch(base + id);
  let json = await resp.json();
  return json['weight'] as number;
}

</script>

<template>
  <div class="full-width">
    <div class="row flex-center">
      <div class="col-6 col-md-4 col-xl-2">
        <PokeIcon :id="id1"/>
      </div>
      <div class="col-6 col-md-4 col-xl-2">
        <PokeIcon :id="id2"/>
      </div>
    </div>
    <div class="row flex-center">
      <div class="col-6 col-md-4 col-xl-2 q-pa-sm">
        <q-btn id="btn1" :class="btn1Classes" @click="submitChoice()">
          This beautiful angel
        </q-btn>
      </div>
      <div class="col-6 col-md-4 col-xl-2 q-pa-sm">
        <q-btn id="btn2" :class="btn2Classes" @click="submitChoice()">
          Or this angry lettuce?
        </q-btn>
      </div>
    </div>
  </div>

</template>

<style scoped>
.correct {
  background: #21BA45;
}

.wrong {
  background: #C10015;
}
</style>
