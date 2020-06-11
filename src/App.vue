<template>
  <div id="app">
    <Header
      :numCorrect='numCorrect'
      :numTotal='numTotal'
      :roundNumber='index'
    />
    <QuestionBox
      v-if='questions.length && !endOfGame'
      :currentQuestion='questions[index]'
      :next='next'
      :increment='increment'
      :numTotal='numTotal'
      :handleEndOfGame='handleEndOfGame'
    />
    <h1 v-if='!questions.length'>
      LOADING...
    </h1>
    <div v-if='endOfGame'>
      <h1>GAME OVER! You got {{numCorrect}} correct!</h1>
      <button @click='startGame'>PLAY AGAIN!</button>
    </div>
  </div>
</template>

<script>
import Header from './components/Header/Header.vue'
import QuestionBox from './components/QuestionBox/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    },
    handleEndOfGame() {
      this.endOfGame = true
    },
    startGame() {
      fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple')
        .then(response => response.json())
        .then(apiData => this.questions = apiData.results)
        .then(() => this.endOfGame = false)
    }
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      endOfGame: false
    }
  },
  mounted() {
    this.startGame()
  }
}
</script>

<style>
  @import './components/Header/Header.css';
  @import './components/QuestionBox/QuestionBox.css';
  #app {
    align-items: center;
    background: center/100% url(./images/green_fern_background.jpeg);
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
    flex-direction: column;
    height: 100vh;
    padding-bottom: 100px;
    width: 100%;
  }
</style>
