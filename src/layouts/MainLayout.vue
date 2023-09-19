<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          aria-label="Menu"
          dense
          flat
          icon="menu"
          round
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          PokeWiz
        </q-toolbar-title>

        <div>Poke-A-Centaur</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Route 11
        </q-item-label>

        <q-img fit="contain" ratio="1.4" src="Snorlax2.png"/>
        <div class="text-center">
          <p>A sleeping POKèMON blocks the way!</p>
        </div>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view/>
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import {defineComponent, ref} from 'vue';

export default defineComponent({
  name: 'MainLayout',

  components: {},

  setup() {
    const leftDrawerOpen = ref(false)

    return {
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
});
</script>

<style scoped>
.q-img {
  image-rendering: pixelated;
}
</style>
