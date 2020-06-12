<template>
  <div class='header-container'>
    <h1 class='title'>animal trivia</h1>
    <p class='header-text'>correct answers: {{ numCorrect }}/{{ numTotal }}</p>
    <p class='header-text'>round number: {{ roundNumber + 1 }}</p>
    <button @click='onRulesButtonClick' class='view-rules-button'>{{ !rulesVisible  ? 'how to play' : 'back to game' }}</button>
  </div>
</template>

<script>
  export default {
    props: [
      'numCorrect',
      'numTotal',
      'roundNumber',
      'rulesVisible',
      'handleRulesVisible'
    ],
    methods: {
      onRulesButtonClick() {
        this.handleRulesVisible()
      }
    }
  }
</script>
