<template>
  <q-page class="flex flex-center q-mx-md">
<!--    <img alt="Quasar logo" src="../assets/logo.svg" style="width: 200px; height: 200px">-->
    <div class="flex-break">
      <div id="primerrow" v-if="!showSuccess && !showFail && currentAlgoritm">
        <div class="row text-h5 text-center q-mb-lg">
          <div class="col-xs-12 col-sm-6">Правильно:{{this.currentAlgoritm.countSuccess}}</div>
          <div class="col-xs-12 col-sm-6">Неправильно: {{this.currentAlgoritm.countFail}}</div>
        </div>
        <div class="row">
          <div class="text-h2 col-xs-12 col-sm-6 text-center">{{ primer.primer}}</div>
          <div class="col-xs-12 col-sm-6">
            <q-input outlined v-model="maybeanswer" id="ansinput" v-on:keyup.enter="checkanswer()">
              <template v-slot:after>
                <q-btn color="white" text-color="black" label="ОК" @click="checkanswer()" />
              </template>
            </q-input>
          </div>
        </div>
      </div>
    </div>
    <div class="result-s text-h1 gt-xs" v-if="showSuccess">
      {{ textSuccess }}
    </div>
    <div class="result-f text-h2 gt-xs" v-if="showFail">
      {{ textFail }}
    </div>

    <div class="result-s text-h1 lt-sm" v-if="showSuccess" style="font-size: 12.5vw;">
      {{ textSuccess }}
    </div>
    <div class="result-f text-h2 lt-sm" v-if="showFail" style="font-size: 12.5vw;">
      {{ textFail }}
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
      console.log("check answer");
      if (this.maybeanswer === null || this.maybeanswer === '') return;
      let number = parseInt(this.maybeanswer);
      this.maybeanswer = '';
      if (number === this.primer.answer) {
        this.currentAlgoritm.countSuccess++;
        this.success();
      }else{
        this.currentAlgoritm.countFail++;
        this.fail();
      }
    },
    drawNewPrimer() {
      console.log("draw new primer");
      this.primer = generatePrimer(
          this.currentAlgoritm, this.maxFirst, this.maxSecond, this.maxAnswer
      );
    },
    success() {
      console.log("show success");
      this.textSuccess = this.getSuccessText();
      this.showSuccess = true;
      setTimeout(function(th){
        th.showSuccess = false;
        this.textSuccess = '';
        th.drawNewPrimer();
        th.setFocus(th);
        console.log("hide success");
      }, 1000, this);
    },
    fail() {
      console.log("show fail");
      this.textFail = this.primer.primer + this.primer.answer;
      this.showFail = true;
      setTimeout(function(th){
        th.showFail = false;
        this.textFail = '';
        th.drawNewPrimer();
        th.setFocus(th);
        console.log("hide fail");
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
      showSuccess: false,
      textSuccess: '',
      showFail: false,
      textFail: '',
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