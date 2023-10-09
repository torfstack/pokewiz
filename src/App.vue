<template>
  <router-view/>
</template>

<script lang="ts">
import {createApp, defineComponent} from 'vue';
import {createPinia} from 'pinia';
import App from './App.vue'

export default defineComponent({
  name: 'App'
});

const pinia = createPinia()
const app = createApp(App)

app.use(pinia)
app.mount('#app')
</script>
