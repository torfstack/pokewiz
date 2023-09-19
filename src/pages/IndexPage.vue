<template>
  <q-page class="row items-center justify-evenly">
    <QuizComponent/>
  </q-page>
</template>

<script lang="ts">
import {defineComponent} from 'vue';
import QuizComponent from 'components/QuizComponent.vue';

export default defineComponent({
  name: 'IndexPage',
  // eslint-disable-next-line vue/no-unused-components
  components: {QuizComponent}
});
</script>
