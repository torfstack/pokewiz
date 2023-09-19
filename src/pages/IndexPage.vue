<template>
  <q-page class="row items-center justify-evenly">
    <QuizComponent/>
  </q-page>
</template>

<script lang="ts">
import {Meta, Todo} from 'components/models';
import {defineComponent, ref} from 'vue';
import QuizComponent from 'components/QuizComponent.vue';

export default defineComponent({
  name: 'IndexPage',
  // eslint-disable-next-line vue/no-unused-components
  components: {QuizComponent},
  setup() {
    const todos = ref<Todo[]>([
      {
        id: 1,
        content: 'ct1'
      },
      {
        id: 2,
        content: 'ct2'
      },
      {
        id: 3,
        content: 'ct3'
      },
      {
        id: 4,
        content: 'ct4'
      },
      {
        id: 5,
        content: 'ct5'
      }
    ]);
    const meta = ref<Meta>({
      totalCount: 1200
    });
    return {todos, meta};
  }
});
</script>
