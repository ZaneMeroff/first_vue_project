<template>
  <div class='question-box-container'>
    <div class='question-container'>
      <h3>{{ currentQuestion.question }}</h3>
      <p v-for='answer in answers' :key='answer' class='possible-answer'>
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
