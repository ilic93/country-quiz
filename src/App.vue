<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row>
        <b-col cols="8" offset="2" md="6" offset-md="3" style="position: relative;">
          <h3>Country Quiz</h3>
          <img src="./assets/undraw_adventure_4hum 1.svg" class="quiz-photo" />
          <Questions
          v-if="countries.length"
          :country="country"
          :countries="countries"
          :increment="increment"
          :fetchQuestion="fetchQuestion"
          :score="score" />
        </b-col>
      </b-row>
    </b-container>    
  </div>
</template>

<script>
import Questions from './components/Questions.vue'

export default {
  name: 'App',
  components: {
    Questions
  },
  data () {
    return {
      countries: [],
      country: {},
      score: 0
    }
  },
  methods: {
    fetchQuestion() {
      const index = Math.floor(Math.random() * this.countries.length)
      this.country = this.countries[index]
    },
    increment(isCorrect) {
      if(isCorrect) {
        this.score++
      } else {
        this.score = 0
      }
    }
  },
  mounted () {
    fetch('https://restcountries.eu/rest/v2/all?fields=name;capital;flag').then(x => x.json()).then(x => {
      this.countries = x
      this.fetchQuestion()
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: url(./assets/background.png);
  background-size: cover;
  min-height: 100vh;
}

.bv-example-row {
  text-align: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.bv-example-row h3 {
  text-align: left;
  color: white;
  font-weight: bold;
  text-transform: uppercase;
}

.quiz-photo {
  position: absolute;
  top: 0;
  right: 0;
  transform: scale(0.75) translate(0, -50px);
}

@media screen and (min-width: 992px) {
  .quiz-photo {
    transform: scale(1) translate(-10px, -50px);
  }
}

.jumbotron {
  background-color: white;
  padding: 80px 20px 30px 20px;
  border-radius: 15px;
  overflow: hidden;
}
</style>
