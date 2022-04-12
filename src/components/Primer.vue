<template>
  <q-page class="flex flex-center">
<!--    <img alt="Quasar logo" src="../assets/logo.svg" style="width: 200px; height: 200px">-->
    <div class="flex-break">
      <div id="primerrow" v-if="!isSuccess && !isFail && currentAlgoritm">
        <div class="row text-h5 text-center q-mb-lg">
          <div class="col">Правильно:{{this.currentAlgoritm.countSuccess}}</div>
          <div class="col">Неправильно: {{this.currentAlgoritm.countFail}}</div>
        </div>
        <div class="row">
          <div class="text-h2">{{ primer.primer}}</div>
          <q-input class="q-ml-md" outlined v-model="maybeanswer" id="ansinput" v-on:keyup.enter="checkanswer()">
            <template v-slot:after>
              <q-btn color="white" text-color="black" label="ОК" @click="checkanswer()" />
            </template>
          </q-input>
        </div>
      </div>
    </div>
    <div class="result-s text-h1" v-if="isSuccess">
      {{ getSuccessText() }}
    </div>
    <div class="result-f text-h2" v-if="isFail">
      {{ primer.primer }} {{ primer.answer }}
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
      if (this.maybeanswer === null || this.maybeanswer === '') return;
      let number = parseInt(this.maybeanswer);
      if (number === this.primer.answer) {
        this.currentAlgoritm.countSuccess++;
        this.success();
        this.drawNewPrimer();
      }else{
        this.currentAlgoritm.countFail++;
        this.fail();
        this.drawNewPrimer();
      }
      this.maybeanswer = '';
    },
    drawNewPrimer() {
      this.primer = generatePrimer(
          this.currentAlgoritm, this.maxFirst, this.maxSecond, this.maxAnswer
      );
    },
    success() {
      this.isSuccess = true;
      setTimeout(function(th){
        th.isSuccess = false;
        th.setFocus();
      }, 1000, this);
    },
    fail() {
      this.isFail = true;
      setTimeout(function(th){
        th.isFail = false;
        th.setFocus(th);
      }, 3500, this);
    },
    setFocus(th) {
      setTimeout(function(th){
        document.getElementById("primerrow").getElementsByTagName("input")[0].focus();
      }, 100, this)
    },
    getSuccessText(){
      let randomIndex = Math.trunc(Math.random() * this.successTexts.length)
      return this.successTexts[randomIndex];
    }
  },
  data() {
    return {
      isSuccess: false,
      isFail: false,
      maybeanswer: null,
      primer: {
        primer: '7 - 7',
        answer: 0
      },
      successTexts: [
        'Молодец!',
        'Умница!',
        'Так держать!',
        'Талант!',
        'Здорово!',
        'Великолепно!',
        'Правильно!',
        'Отлично!',
        'Замечательно!',
        'Класс!',
        'Шедеврально!'
      ]
    }
  }
}
</script>

<style scoped>
  .result-s {
    color: #21BA45;
  }
  .result-f {
    color: #C10015;
  }
</style>