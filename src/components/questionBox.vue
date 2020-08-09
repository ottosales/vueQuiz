<template>
    <div class="question-box-container">
        <b-jumbotron class="mt-5'">
            <template v-slot:lead>
                <h3 v-html="currentQuestion.question"></h3>
            </template>

            <hr class="my-4">

            <b-list-group class="mb-4">
                <b-list-group-item v-for="(answer, index) in shuffledAnswers" 
                :key="index"
                @click="selectAnswer(index)"
                :class="answerClass(index)">
                    <b>{{ answer }}</b>
                </b-list-group-item>
            </b-list-group>

            <b-row>
                <b-col>
                    <b-button class="mb-3" variant="primary"
                    v-on:click="submitAnswer"
                    :disabled="selectedIndex === null || answered"
                    >Submit</b-button>
                </b-col>
            </b-row>

            <b-button class="mr-1" @click="back" variant="success" href="#">&lt;&lt;</b-button>
           
            <b-button class="ml-1" @click="next" variant="success" href="#">&gt;&gt;</b-button>

            

        </b-jumbotron>
    </div>
</template>

<script>
    export default{
        props: {
            currentQuestion: Object,
            back: Function,
            next: Function,
            increment: Function
        },
        data: function(){
            return{
                selectedIndex: null,
                correctAnswer: null,
                correctIndex: null,
                shuffledAnswers: [],
                answered: false
            }
        },
        computed:{
            answers(){
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers
            }
        },
        watch:{
            currentQuestion: {
                immediate: true,
                handler(){
                    this.selectedIndex = null
                    this.correctAnswer = null
                    this.selectIndex = null
                    this.shuffleAnswers()
                    this.answered = false
                }
            }
        },
        methods:{
            shuffleAnswers(){
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                
                for(let i = answers.length - 1; i > 0; i--){
                    const j = Math.floor(Math.random() * i)
                    const temp = answers[i]
                    answers[i] = answers[j]
                    answers[j] = temp
                }
                this.shuffledAnswers = answers
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            selectAnswer(index){
                if(!this.answered){
                    if(this.selectedIndex === index)
                        this.selectedIndex = null
                    else
                        this.selectedIndex = index
                }
            },
            submitAnswer(){
                let isCorrect = false

                if(this.selectedIndex === this.correctIndex)
                    isCorrect = true

                this.answered = true

                this.increment(isCorrect)
            },
            answerClass(index){
                let answerClass = ''

                if(!this.answered && this.selectedIndex === index)
                    answerClass = 'selected'

                else if(this.answered && this.correctIndex === index)
                    answerClass = 'correct'

                else if(this.answered && this.selectedIndex === index && this.correctIndex != index)
                    answerClass = 'incorrect'

                return answerClass
            }
        }
    }
</script>

<style>

.list-group-item:hover{
    background-color: #EEE;
    cursor: pointer;
}

.selected{
    background-color: lightblue;
}

.selected:hover{
    background-color: lightblue;
}

.correct{
    background-color: lightgreen;
}

.correct:hover{
    background-color: lightgreen;
}

.incorrect{
    background-color: #ff0033;
    color: #EEE;
}

.incorrect:hover{
    background-color: #ff0033;
    color: #EEE
}

</style>