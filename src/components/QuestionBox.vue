<template>
  <div>
  <b-jumbotron>

    <template>
      <h1>Question</h1>
    </template>
    <p>
      {{this.question.question}}
    </p>
    <hr class="my-4">

    
    <b-card no-body class="my-3">
    <b-list-group flush :key="index" v-for="(a, index) in this.answers">
      <b-list-group-item
      @click="selectAnswer(index);"
      :class="selectClass(index)"
      >{{a}}</b-list-group-item>
    </b-list-group>
  </b-card>
     <!-- <b-list-group> -->
  <!-- <b-list-group-item variant="success">Success list group item</b-list-group-item> -->
  <!-- <b-list-group-item variant="danger">Danger list group item</b-list-group-item> -->
  <!-- <b-list-group-item variant="dark">Dark list group item</b-list-group-item> -->
<!-- </b-list-group> -->

    <b-button variant="primary" class="mx-2" @click="submit()" :disabled="this.selectedIndex === null || submitted">Submit</b-button>
    <b-button variant="success" class="mx-2" @click="next" :disabled="this.questionIndex >= 9 || !submitted">Next</b-button>
  </b-jumbotron>
</div>
</template>


<script>
import _ from 'lodash'

export default {
  data(){
    return{
      selectedIndex: null,
      shuffledAnswers: [],
      correctAnswerIndex: null,
      submitted: false
    }
  },
  props:{
    question: Object,
    next: Function,
    questionIndex: Number,
    increment : Function
  },
  computed:{
    answers() {
      let answers = this.question.incorrect_answers;
      answers.push(this.question.correct_answer);
      let answersShuffled = _.shuffle(answers);
      answersShuffled.forEach((a, index) => {
        if (a === this.question.correct_answer){
          this.correctAnswerIndex = index;
        }
      });

      return answersShuffled;
    }
  },
  watch:{
    question(){
      this.selectedIndex = null;
      this.shuffleAnswers();
      this.submitted = false;
      this.clase = ''
    }
  },
  methods: {
    selectAnswer(index){
      this.selectedIndex = index;
      
    },
    shuffleAnswers() {
      let answers = [...this.question.incorrect_answers, this.question.correct_answer];
      let shuffledAnswers = _.shuffle(answers);
      shuffledAnswers.forEach((a, index) => {
        if (a === this.question.correct_answer){
          this.correctAnswerIndex = index;
        }
      });

      this.shuffledAnswers = shuffledAnswers;
    },
    selectClass(index){
      let selectedClass = '';
      if (this.submitted){
        if (index === this.correctAnswerIndex) {
          selectedClass = 'correct'
        }else if (this.selectedIndex !== this.correctAnswerIndex && this.selectedIndex === index){
          selectedClass = 'incorrect'
        }
      }else if (this.selectedIndex === index){
        selectedClass = 'selected'
      }
      return selectedClass;
      
    },
    submit(){
      let isCorrect = this.correctAnswerIndex === this.selectedIndex;
      this.increment(isCorrect);
      this.submitted = true;
      
    },
    showCorrect(){
        
      }
  }
}
</script>

<style scoped>

  .list-group-item {
    cursor: pointer;
  }

  .list-group-item:not(.selected):not(.correct):not(.incorrect):hover{
    background-color: #efefef;
  }
  
  .selected {
    background-color: lightblue;
  }

  .correct {
    background-color: lightgreen;
  }

  .incorrect {
    background-color: rgb(223, 100, 84);
  }


</style>