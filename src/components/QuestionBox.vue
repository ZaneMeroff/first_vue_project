<template>
  <div class='question-box-container'>
    <div class='question-container'>
      <h3>{{ currentQuestion.question }}</h3>
      <p v-for='(answer, index) in answers'
        :key='answer'
        class='possible-answer'
        @click.prevent='selectAnswer(index)'
        :class="answerClass(index)"
      >
        {{ answer }}
      </p>
      <div class='buttons-container'>
        <button
          @click='submitAnswer'
          :disabled='selectedIndex === null || answered'
          class='submit-button'
        >
          submit
        </button>
        <button
          @click='next'
          class='next-button'
          :disabled='!answered'
        >
          next
        </button>
      </div>
    </div>
  </div>
</template>

<script>
  import _ from 'lodash';
  export default {
    props: {
      currentQuestion: Object,
      next: Function,
      increment: Function
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
      },
      shuffleAnswers() {
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
      },
      submitAnswer() {
        let isCorrect = false
        if (this.selectedIndex === this.correctIndex) {
          isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
      },
      answerClass(index) {
        let answerClass = ''
        if (!this.answered && this.selectedIndex === index) {
          answerClass = 'selected'
        } else if (this.answered && this.correctIndex === index) {
          answerClass = 'correct'
        } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
          answerClass = 'incorrect'
        }
        return answerClass
      }
    },
    watch: {
      currentQuestion() {
        this.selectedIndex = null
        this.shuffleAnswers()
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        this.answered = false
      }
    },
    data() {
      return {
        selectedIndex: null,
        correctIndex: null,
        shuffledAnswers: [],
        answered: false
      }
    },
    computed: {
      answers() {
        let answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)
        return answers;
      }
    },
    mounted() {
      this.shuffleAnswers()
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    }
  }
</script>
