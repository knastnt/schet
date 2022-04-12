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
      <Primer
          :current-algoritm="currentAlgoritm"
          :max-first="maxFirst"
          :max-second="maxSecond"
          :max-answer="maxAnswer"
      />
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
      maxFirst: 18,
      maxSecond: 13,
      maxAnswer: 20,
      currentAlgoritm: null,
      algoritms: [
          {
            countSuccess: 0,
            countFail: 0,
            name: 'Сложение с переходом через десяток',
            logic: function (obj) {
              return obj.first < 10 && obj.ans > 10 && obj.second < 10;
            }
          },
          {
            countSuccess: 0,
            countFail: 0,
            name: 'Вычитание с переходом через десяток',
            logic: function (obj) {
              return obj.first > 10 && obj.ans < 10 && obj.ans > 0 && obj.second < 10;
            }
          },
          {
            countSuccess: 0,
            countFail: 0,
            name: 'Вперемешку',
            logic: function (obj) {
              return obj.ans >= 0 && obj.ans <= obj.maxAns;
            }
          },
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