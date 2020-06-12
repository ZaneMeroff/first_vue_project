<template>
  <div id="app">
    <Header
      :numCorrect='numCorrect'
      :numTotal='numTotal'
      :roundNumber='index'
      :rulesVisible='rulesVisible'
      :handleRulesVisible='handleRulesVisible'
    />
    <QuestionBox
      v-if='questions.length && !endOfGame '
      :currentQuestion='questions[index]'
      :next='next'
      :increment='increment'
      :numTotal='numTotal'
      :handleEndOfGame='handleEndOfGame'
      :class="[ rulesVisible ? 'hidden' : '']"
    />
    <Rules v-if='rulesVisible'/>
    <h1 v-if='!questions.length' class='loading-text'>loading...</h1>
    <div v-if='endOfGame' class='game-over-container'>
      <p class='game-over-text'>game over!</p>
      <p class='number-correct-text'>You got <span class='num-correct'>{{numCorrect}}</span> correct!</p>
      <button class='play-again-button' @click='startGame'>play again!</button>
    </div>
  </div>
</template>

<script>
import Header from './components/Header/Header.vue'
import QuestionBox from './components/QuestionBox/QuestionBox.vue'
import Rules from './components/Rules/Rules.vue'
export default {
  name: 'App',
  components: {
    Header,
    QuestionBox,
    Rules
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
      this.index = 0
      this.numCorrect = 0
      this.numTotal = 0
      fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple')
        .then(response => response.json())
        .then(apiData => this.questions = apiData.results)
        .then(() => this.endOfGame = false)
    },
    handleRulesVisible() {
      if (this.rulesVisible) {
        this.rulesVisible = false
      } else {
        this.rulesVisible = true
      }
    }
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      endOfGame: false,
      rulesVisible: false,
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
  @import './components/Rules/Rules.css';
  @import './App.css';
</style>
