<template>
  <div>
    <b-jumbotron>
      <img src="../assets/undraw_winners_ao2o 2.svg"/>
      <h1 class="score-head">Results</h1>
      <p class="score">You got <span>{{ result }}</span> correct answers</p>
      <b-button variant="outline-primary" @click="start()">Try Again</b-button>
    </b-jumbotron>
  </div>
</template>

<script>

export default {
  props: ['result', 'start']
}

</script>

<style>

.score-head {
  margin-top: 50px;
}

.score {
  margin: 20px 0;
  font-size: 20px;
}

.score span {
  color: blue;
  font-weight: bold;
  font-size: 40px;
}

</style>