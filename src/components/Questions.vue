<template>
  <div>
    <b-jumbotron>
      <template v-if="!guessFlag">
        {{ country.capital }} is the capital of
      </template>

      <template v-if="guessFlag">
        <img id="flag" :src="`${country.flag}`"/>
        <br>
        Which country does this flag belong to?
      </template> 

      <b-list-group>
        <b-list-group-item v-for="(answer, index) in answers" :key="index" @click="selectAnswer(index)" :class="answerClass(index)" :disabled="answered">
          {{ answer }}
          </b-list-group-item>
      </b-list-group>
    
      <!-- <b-button variant="primary" @click="submitAnswer" :disabled="selectedAnswer == null || answered">Submit</b-button> -->
      <b-button variant="success" v-if="answered" @click="fetchQuestion()">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: ['country', 'countries', 'increment', 'fetchQuestion', 'score'],
  data () {
    return {
      correctAnswer: null,
      answered: false,
      selectedAnswer: null,
      guessFlag: false
    }
  },
  methods: {
    shuffle(array) {
      let currentIndex = array.length, temporaryValue, randomIndex;
    
      // While there remain elements to shuffle...
      while (0 !== currentIndex) {
    
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;
    
        // And swap it with the current element.
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }
    
      return array;
    },
    getCorrectIndex(array, x) {
      this.correctAnswer = array.indexOf(x)
    },
    selectAnswer(index) {
      let isCorrect = false
      this.selectedAnswer = index
      if(index == this.correctAnswer) {
        isCorrect = true
      }
      this.increment(isCorrect)
      this.answered = true
    },
    answerClass(index) {
      let answerClass = (this.answered && index == this.correctAnswer) ? 'correct' : 
      (this.answered && this.selectedAnswer !== this.correctAnswer && index === this.selectedAnswer) ? 'incorrect' : ''
      return answerClass
    }
  },
  computed: {
    answers() {
      let answers = [this.country.name]
      for(let i = 1; i < 4; i++) {
        let index = Math.floor(Math.random() * this.countries.length)
        while(answers.includes(this.countries[index].name)) { index = Math.floor(Math.random() * this.countries.length) }
        answers.push(this.countries[index].name)
        this.shuffle(answers)
        this.getCorrectIndex(answers, this.country.name)
      }
      return answers
    }
  },
  watch: {
    country() {
      this.correctAnswer = null,
      this.answered = false,
      this.selectedAnswer = null
      this.guessFlag = this.score % 2 !== 0 ? true : false
    }
  }
}
</script>

<style>

#flag {
  width: 100px;
  height: 50px;
  margin-bottom: 15px;
}

@media screen and (min-width: 992px) {
  #flag {
    width: 200px;
    height: 100px;
  }
}

.list-group-item {
  border: 2px solid grey !important;
  border-radius: 10px !important;
  margin: 10px 0;
}

.list-group-item:hover {
  background-color: orange;
  color: white;
  cursor: pointer;
}

.btn-success {
  float: right;
}

.correct { 
  background-color: lightgreen !important;
}

.incorrect { 
  background-color: red !important;
  color: white !important;
}
</style>

