<template>
  <q-page class="flex flex-center">
<!--    <img alt="Quasar logo" src="../assets/logo.svg" style="width: 200px; height: 200px">-->
    <div class="row">
      <div class="text-h2">{{ primer.primer}}</div>
      <q-input class="q-ml-md" outlined v-model="maybeanswer">
        <template v-slot:after>
          <q-btn color="white" text-color="black" label="ОК" @click="checkanswer()" />
        </template>
      </q-input>
    </div>
  </q-page>
</template>

<script>

function generatePrimer(currentAlgoritm, maxFirst, maxSecond, maxAns) {
  let pr = "";
  let ans = 0;
  let first = 0;
  let second = 0;
  do {
    first = getRandomInt(maxFirst);
    second = getRandomInt(maxSecond);
    if (getRandomInt(2) == 0) {
      //minus
      ans = first - second;
      pr = first + " - " + second;
    } else {
      //plus
      ans = first + second;
      pr = first + " + " + second;
    }
    if (currentAlgoritm.logic.call(this, {
      first: first,
      second: second,
      ans: ans,
      maxAns: maxAns
    })) break;
  }while (true);

  return {
    primer: pr + ' = ',
    answer: ans
  };
}
function getRandomInt(max) {
  return Math.floor(Math.random() * max);
}



export default {
  name: "Primer",
  props: ['current-algoritm', 'max-first', 'max-second', 'max-answer'],
  watch: {
    // эта функция запускается при любом изменении вопроса
    currentAlgoritm: function (newCurrentAlgoritm, oldCurrentAlgoritm) {
      this.drawNewPrimer.call(this);
    }
  },
  methods: {
    checkanswer() {
      let number = parseInt(this.maybeanswer);
      if (number === this.primer.answer) {
        console.log("+");
        this.drawNewPrimer();
      }else{
        console.log("-");
        this.drawNewPrimer();
      }
      this.maybeanswer = '';
    },
    drawNewPrimer() {
      this.primer = generatePrimer(
          this.currentAlgoritm, this.maxFirst, this.maxSecond, this.maxAnswer
      );
    }
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