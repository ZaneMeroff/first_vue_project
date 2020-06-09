<template>
  <div class='question-box-container'>
    <div class='question-container'>
      <h3>{{ currentQuestion.question }}</h3>
      <p v-for='answer in answers' :key='answer'>
        {{ answer }}
      </p>
      <button>submit</button>
      <button @click='next'>next</button>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      currentQuestion: Object,
      next: Function
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
