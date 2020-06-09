<template>
  <div class='question-box-container'>
    <div class='question-container'>
      <h3>{{ currentQuestion.question }}</h3>
      <p v-for='(answer, index) in answers'
        :key='answer'
        class='possible-answer'
        @click.prevent='selectAnswer(index)'
        :class="[ selectedIndex === index ? 'selected' : '' ]"
      >
        {{ answer }}
      </p>
      <div class='buttons-container'>
        <button class='submit-button'>submit</button>
        <button @click='next' class='next-button'>next</button>
      </div>
    </div>
  </div>
</template>

<script>
  import _ from 'lodash';
  export default {
    props: {
      currentQuestion: Object,
      next: Function
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
      },
      shuffleAnswers() {
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
      }
    },
    watch: {
      currentQuestion() {
        this.selectedIndex = null
        this.shuffleAnswers()
      }
    },
    data() {
      return {
        selectedIndex: null,
        shuffledAnswers: []
      }
    },
    computed: {
      answers() {
        let answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)
        return answers;
      }
    }
  }
</script>
