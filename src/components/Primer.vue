<template>
  <q-page class="flex flex-center">
<!--    <img alt="Quasar logo" src="../assets/logo.svg" style="width: 200px; height: 200px">-->
    <div class="row">
      <div class="text-h2">{{ primer.primer + ' ='}}</div>
      <q-input class="q-ml-md" outlined v-model="maybeanswer">
        <template v-slot:after>
          <q-btn color="white" text-color="black" label="ОК" @click="checkanswer()" />
        </template>
      </q-input>
    </div>
  </q-page>
</template>

<script>
function drawNewPrimer() {
  this.currentAlgoritm.logic.call();
}
let checkanswerfnc = function checkanswer() {
  let number = parseInt(this.maybeanswer);
  if (number === this.primer.answer) {
    console.log("+");
  }else{
    console.log("-");
  }
  this.maybeanswer = '';
}

export default {
  name: "Primer",
  props: ['current-algoritm'],
  watch: {
    // эта функция запускается при любом изменении вопроса
    currentAlgoritm: function (newCurrentAlgoritm, oldCurrentAlgoritm) {
      drawNewPrimer();
    }
  },
  methods: {
    checkanswer: checkanswerfnc
  },
  data() {
    return {
      maybeanswer: null,
      primer: {
        primer: '7 - 7',
        answer: 0
      }
    }
  }
}
</script>

<style scoped>

</style>