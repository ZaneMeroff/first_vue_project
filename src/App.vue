<template>
  <div id="app">
    <Header
      :numCorrect='numCorrect'
      :numTotal='numTotal'
    />
    <QuestionBox
      v-if='questions.length'
      :currentQuestion='questions[index]'
      :next='next'
      :increment='increment'
      :numTotal='numTotal'
    />
    <h1 v-if='!questions.length'>
      LOADING...
    </h1>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

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
    }
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple')
      .then(response => response.json())
      .then(apiData => this.questions = apiData.results)
  }
}
</script>

<style>
  @import './components/Header.css';
  @import './components/QuestionBox.css';
</style>
