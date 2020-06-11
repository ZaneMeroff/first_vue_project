<template>
  <div class='header-container'>
    <h1 class='title'>🐛 ANIMAL QUIZ 🤨</h1>
    <p class='question-counter'>correct answers: {{ numCorrect }}/{{ numTotal }}</p>
    <p class='question-counter'>round number: {{ roundNumber + 1 }}</p>
  </div>
</template>

<script>
  export default {
    props: [
      'numCorrect',
      'numTotal',
      'roundNumber'
    ]
  }
</script>
