<template>
  <header>
      <div>
          <b-nav>
            <b-nav-item disabled>Quiz game</b-nav-item>
            <b-nav-item disabled>Question {{currentQuestion}}/{{totalQuestions}}</b-nav-item>
            <b-nav-item disabled>Correct Answers {{numCorrectAnswers}}/{{totalQuestions}}</b-nav-item>
          </b-nav>
      </div>
  </header>
</template>

<script>
export default {
  props:{
    totalQuestions: Number,
    currentQuestion: Number,
    numCorrectAnswers: Number
  }
}
</script>

<style scoped>

    b-nav {
        width: 300px;
    }
    
</style>