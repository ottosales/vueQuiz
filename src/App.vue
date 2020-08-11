<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
      :highScore="highScore"
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
            :playAgain="playAgain"
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
      questionInput:[],
      highScore: 0
    }
  },
  methods:{
    startGame(){
      this.questions = []
      this.index = 0
      this.numCorrect = 0
      this.numTotal = 0
      this.questionInput = []
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
    },
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
      if(this.numTotal === 10){
        this.saveHighScore()
      }
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
    },
    saveHighScore(){
      this.highScore = this.numCorrect > this.highScore ? this.numCorrect : this.highScore
    },
    playAgain(){
      this.startGame()
    }
  },
  mounted: function(){
    this.startGame()
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
