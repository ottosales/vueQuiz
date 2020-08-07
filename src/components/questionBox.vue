<template>
    <div class="question-box-container">
        <b-jumbotron class="mt-5'">
            <template v-slot:lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group class="mb-4">
                <b-list-group-item v-for="(answer, index) in answers" 
                :key="index"
                @click="selectAnswer(index)"
                :class="[selectedIndex === index ? 'selected' : '']">
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button class="mr-1" variant="primary"
                v-on:click="submitAnswer"
            >Submit</b-button>
            <b-button class="ml-1" @click="next" variant="success" href="#">Next Question</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    export default{
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function
        },
        computed:{
            answers(){
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                this.shuffle(answers)
                return answers
            }
        },
        data: function(){
            return{
                selectedIndex: null
            }
        },
        methods:{
            shuffle(answers){
                this.currentQuestion.correct_index = 3
                for(let i = answers.length - 1; i > 0; i--){
                    const j = Math.floor(Math.random() * i)
                    const temp = answers[i]
                    answers[i] = answers[j]
                    answers[j] = temp
                    if(answers[i] == this.currentQuestion.correct_answer)
                        this.currentQuestion.correct_index = i
                    else if(answers[j] == this.currentQuestion.correct_answer)
                        this.currentQuestion.correct_index = j
                }
                
            },
            selectAnswer(index){
                this.selectedIndex = index                
            },
            submitAnswer(){
                let isCorrect = false

                if(this.selectedIndex === this.currentQuestion.correct_index)
                    isCorrect = true

                this.increment(isCorrect)
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

.correct{
    background-color: lightgreen;
}

.incorrect{
    background-color: red;
}

</style>