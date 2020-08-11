<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="8" offset="2">
          <questionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :back="back"
            :next="next"
            :increment="increment"
            :input="questionInput[index]"
            :numCorrect="numCorrect"
            :numTotal="numTotal"
          />
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>

import Header from './components/header.vue'
import questionBox from './components/questionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    questionBox
  },
  data(){
    return{
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      questionInput:[]
    }
  },
  methods:{
    back(){
      if(this.index > 0)
        this.index--
    },
    next(){
      if(this.index < 10)
        this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    },
    startMemory(){
      for(var i = 0; i < 10; i++){
        this.questionInput.push({
          selectedIndex: null,
          correctIndex: null,
          shuffled: false,
          shuffledAnswers: [],
          answered: false
      })
      }
    }
  },
  mounted: function(){
    this.startMemory()
    fetch("https://opentdb.com/api.php?amount=10&category=15&type=multiple", {
      method: "get"
    })
      .then((response) => {
        return response.json()
      })
      .then((jsonData) => {
        this.questions = jsonData.results
      })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
