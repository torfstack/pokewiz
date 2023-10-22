<script lang="ts" setup>
import {ref} from 'vue';
import PokeIcon from 'components/PokeIcon.vue';
import {cacheExchange, Client, fetchExchange} from '@urql/core'

let counter = ref(0)
let id1 = randomPokemon()
let id2 = randomPokemon(id1)
let weightsPromise = weightsOfIds(id1, id2)

let submitDisabled = false

let btn1Classes = ref('')
let btn2Classes = ref('')

async function submitChoice(idx: number) {
  if (submitDisabled) {
    return
  }

  submitDisabled = true
  const weights: number[] = await weightsPromise
  console.log(weights)

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

function randomPokemon(except: string | null = null): string {
  let rnd = '' + (Math.floor(Math.random() * 151) + 1)
  return rnd === except ? randomPokemon(except) : rnd
}

function reset() {
  id1 = randomPokemon()
  id2 = randomPokemon(id1)
  btn1Classes.value = ''
  btn2Classes.value = ''
  weightsPromise = weightsOfIds(id1, id2)
  submitDisabled = false
}

async function weightsOfIds(id1: string, id2: string): Promise<number[]> {
  const graphqlEndpoint = 'https://beta.pokeapi.co/graphql/v1beta'
  const query = `
  query weightPokemonQuery($id1: Int!, $id2: Int!) {
    first: pokemon_v2_pokemon(where: {id: {_eq: $id1}}) {
      id
      weight
    }
    second: pokemon_v2_pokemon(where: {id: {_eq: $id2}}) {
      id
      weight
    }
  }
  `
  const client = new Client({
    url: graphqlEndpoint,
    exchanges: [cacheExchange, fetchExchange]
  })
  const result = client.query(query, {id1: id1, id2: id2})
  return result.toPromise().then(result => {
    return [
      result.data.first.map((singleResult: { weight: number; }) => singleResult.weight)[0],
      result.data.second.map((singleResult: { weight: number; }) => singleResult.weight)[0]
    ]
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
