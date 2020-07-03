<template>
  <div id="app" class= "container">
    <b-container class="bv-example-row">
      <b-row>
    <b-col sm="12">
      <Header
      v-if="questions.length > 0"
      :currentQuestion="this.index + 1"
      :totalQuestions="this.questions.length"
      :numCorrectAnswers="this.numCorrectAnswers"
      />
      </b-col>
    
  
    <b-col sm="12" lg="6" offset-lg= "3">
      <QuestionBox 
      v-if="questions.length > 0"
      :question="this.questions[index]"
      :next="this.next"
      :questionIndex="this.index"
      :increment="this.increment"
      />
      </b-col>
  </b-row>
</b-container>
   

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
  data(){
    return {
      questions: [],
      index: 0,
      numCorrectAnswers: 0
    }
  },
  methods:{
    next(){
      if ((this.index + 1) < this.questions.length){
        this.index++;
      }
  },
    increment(isCorrect) {
      if(isCorrect){
        this.numCorrectAnswers++;
      }
    }
  },
  mounted: function () {
    fetch('https://opentdb.com/api.php?amount=10&category=17', {
      method: 'get'
    }).then(resp => {
      return resp.json();
    }).then(data => {
      this.questions = data.results
    });
    
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
