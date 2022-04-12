<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="toggleLeftDrawer" />

        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>
          {{ currentAlgoritm ? currentAlgoritm.name : 'Выберите задание в меню' }}
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" side="left" overlay>
      <q-list>
        <q-item-label header>Задания</q-item-label>
        <q-item clickable v-for="algoritm in algoritms" @click="currentAlgoritm = algoritm; toggleLeftDrawer()">
          <q-item-section>
            <q-item-label>{{ algoritm.name }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <Primer />
    </q-page-container>

  </q-layout>
</template>

<script>
import { ref } from 'vue'
import Primer from "@/components/Primer";

export default {
  name: 'LayoutDefault',

  components: {
    Primer
  },

  data() {
    return {
      currentAlgoritm: null,
      algoritms: [
          {
            name: 'Сложение до 10',
            logic: function () {

            }
          },
          {
            name: 'Вычитание до 10',
            logic: function () {

            }
          }
      ]
    }
  },

  setup () {
    const leftDrawerOpen = ref(false)

    return {
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
}
</script>