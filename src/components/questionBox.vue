<template>
    <div class="question-box-container">
        <div v-if="numTotal < 10">
            <b-jumbotron class="mt-5'">
                <template v-slot:lead>
                    <h3 v-html="currentQuestion.question"></h3>
                </template>

                <hr class="my-4">

                <b-list-group class="mb-4">
                    <b-list-group-item v-for="(answer, index) in this.input.shuffledAnswers" 
                    :key="index"
                    @click="selectAnswer(index)"
                    :class="answerClass(index)">
                        <b v-html="answer"></b>
                    </b-list-group-item>
                </b-list-group>

                <b-row>
                    <b-col>
                        <b-button class="mb-3" variant="primary"
                        v-on:click="submitAnswer"
                        :disabled="this.input.selectedIndex === null || this.input.answered"
                        >Submit</b-button>
                    </b-col>
                </b-row>

                <b-button class="mr-2" @click="back" variant="success" href="#">&lt;&lt;</b-button>
                <b-button class="ml-2" @click="next" variant="success" href="#">&gt;&gt;</b-button>

            </b-jumbotron>
        </div>

        <div v-else>
            <h2>Congratulations! You got {{numCorrect}} questions out of 10!</h2>
        </div>


    </div>
</template>

<script>
    export default{
        props: {
            currentQuestion: Object,
            back: Function,
            next: Function,
            increment: Function,
            input: Object,
            numCorrect: Number,
            numTotal: Number

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
                    if(!this.input.shuffled){
                        this.shuffleAnswers()
                        this.input.shuffled = true
                    }
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
                this.input.shuffledAnswers = answers
                this.input.correctIndex = this.input.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            selectAnswer(index){
                if(!this.input.answered){
                    if(this.input.selectedIndex === index)
                        this.input.selectedIndex = null
                    else
                        this.input.selectedIndex = index
                }
            },
            submitAnswer(){
                let isCorrect = false

                if(this.input.selectedIndex === this.input.correctIndex)
                    isCorrect = true

                this.input.answered = true

                this.increment(isCorrect)
            },
            answerClass(index){
                let answerClass = ''

                if(!this.input.answered && this.input.selectedIndex === index)
                    answerClass = 'selected'

                else if(this.input.answered && this.input.correctIndex === index)
                    answerClass = 'correct'

                else if(this.input.answered && this.input.selectedIndex === index && this.input.correctIndex != index)
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