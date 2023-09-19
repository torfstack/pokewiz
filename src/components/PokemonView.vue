<script lang="ts" setup>
import {ref} from 'vue';
import PokeIcon from 'components/PokeIcon.vue';

const props = defineProps<{
  id1: string,
  id2: string
}>()

const btn1Classes = ref('')
const btn2Classes = ref('')

async function submitChoice(choice: number) {
  const weights: number[] = await Promise.all(
    [
      weightOfId(props.id1),
      weightOfId(props.id2)
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
}

async function weightOfId(id: string): Promise<number> {
  const base = 'https://pokeapi.co/api/v2/pokemon/'
  let resp = await fetch(base + id);
  let json = await resp.json();
  return json['weight'] as number;
}

</script>

<template>
  <div class="full-width row flex-center">
    <div class="col-4">
      <PokeIcon :id="id1"/>
      <q-btn id="btn1" :class="btn1Classes" @click="submitChoice(0 )">
        This beautiful angel
      </q-btn>
    </div>
    <div class="col-4">
      <PokeIcon :id="id2"/>
      <q-btn id="btn2" :class="btn2Classes" @click="submitChoice(1)">
        Or this one?
      </q-btn>
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
