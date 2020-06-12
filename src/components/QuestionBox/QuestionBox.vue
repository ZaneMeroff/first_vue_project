<template>
  <div class='question-box-container'>
    <div class='question-container'>
      <h3 class='question-text'>{{ cleanData(currentQuestion.question) }}</h3>
      <button v-for='(answer, index) in shuffledAnswers'
        :key='answer'
        class='possible-answer'
        @click.prevent='selectAnswer(index)'
        :class="answerClass(index)"
      > {{ answer }} </button>
      <div class='buttons-container'>
        <button
          @click='submitAnswer'
          :disabled='selectedIndex === null || answered'
          class='submit-button'
        > submit </button>
        <button
          @click='onNextButtonClick'
          class='next-button'
          :disabled='!answered'
        > next </button>
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
      increment: Function,
      numTotal: Number,
      handleEndOfGame: Function
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
      },
      shuffleAnswers() {
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      },
      submitAnswer() {
        let isCorrect = false
        if (this.selectedIndex === this.correctIndex) {
          isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
      },
      onNextButtonClick() {
        if (this.numTotal === 10) {
          this.handleEndOfGame()
        } else {
          this.next()
        }
      },
      answerClass(index) {
        let answerClass = ''
        this.answered ? answerClass = 'disabled' : ''
        if (!this.answered && this.selectedIndex === index) {
          answerClass = 'selected disabled'
        } else if (this.answered && this.correctIndex === index) {
          answerClass = 'correct disabled'
        } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
          answerClass = 'incorrect disabled'
        }
        return answerClass
      },
      cleanData(data) {
        if (data.includes('&quot;') ||
            data.includes('&Uuml;') ||
            data.includes('&#039;') ||
            data.includes('&amp;')) {
          data = data.split('&quot;').join('')
          data = data.split('&#039;').join('')
          data = data.split('&amp;').join(' ')
          data = data.split('&Uuml;').join('U')
          return data
        } else {
          return data
        }
      }
    },
    watch: {
      currentQuestion() {
        this.selectedIndex = null
        this.shuffleAnswers()
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
    }
  }
</script>
