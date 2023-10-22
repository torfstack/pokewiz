<script lang="ts" setup>
import {ref} from 'vue';
import PokeIcon from 'components/PokeIcon.vue';
import {cacheExchange, Client, fetchExchange} from '@urql/core'

let counter = ref(0)
let id1 = randomPokemon()
let id2 = randomPokemon(id1)
let weightsPromise = weights()

let submitDisabled = false

let btn1Classes = ref('')
let btn2Classes = ref('')

async function submitChoice(idx: number) {
  if (submitDisabled) {
    return
  }

  submitDisabled = true
  const allWeights: number[] = await weightsPromise
  const weights = [allWeights[id1 - 1], allWeights[id2 - 1]]
  console.log('weights are ' + weights)

  const maxIndex = weights.indexOf(Math.max(...weights))
  if (0 === maxIndex) {
    btn1Classes.value = 'correct'
    btn2Classes.value = 'wrong'
  } else {
    btn1Classes.value = 'wrong'
    btn2Classes.value = 'correct'
  }

  if (idx == maxIndex) {
    counter.value++
  } else {
    counter.value = 0
  }

  setTimeout(() => {
    reset()
  }, 1000)
}

function randomPokemon(except: number | null = null): number {
  let rnd = (Math.floor(Math.random() * 151) + 1)
  return rnd === except ? randomPokemon(except) : rnd
}

function reset() {
  id1 = randomPokemon()
  id2 = randomPokemon(id1)
  btn1Classes.value = ''
  btn2Classes.value = ''
  submitDisabled = false
}

async function weights(): Promise<number[]> {
  const graphqlEndpoint = 'https://beta.pokeapi.co/graphql/v1beta'
  const query = `
  query weightPokemonQuery($ids: [Int!]) {
    pokemon: pokemon_v2_pokemon(where: {id: {_in: $ids}}) {
      id
      weight
    }
  }
  `
  const client = new Client({
    url: graphqlEndpoint,
    exchanges: [cacheExchange, fetchExchange]
  })

  const ids: number[] = new Array(151);
  for (let i = 1; i <= 151; i++) {
    ids[i - 1] = i
  }

  const result = client.query(query, {ids: ids})
  return result.toPromise().then(result => {
    return result.data.pokemon.map((pokemon: { weight: number; }) => pokemon.weight)
  })
}

</script>

<template>
  <div class="full-width">
    <h3>{{ counter }}</h3>
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
        <q-btn id="btn1" :class="btn1Classes" @click="submitChoice(0)">
          This beautiful angel
        </q-btn>
      </div>
      <div class="col-6 col-md-4 col-xl-2 q-pa-sm">
        <q-btn id="btn2" :class="btn2Classes" @click="submitChoice(1)">
          This angry lettuce
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
