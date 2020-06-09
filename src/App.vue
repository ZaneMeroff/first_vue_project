<template>
  <div id="app">
    <Header />
    <QuestionBox
      v-if='questions.length'
      :currentQuestion='questions[index]'
      :next='next'
    />
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
    }
  },
  data() {
    return {
      questions: [],
      index: 0
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
