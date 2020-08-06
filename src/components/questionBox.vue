<template>
    <div class="question-box-container">
        <b-jumbotron class="mt-5'">
            <template v-slot:lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group class="mb-4">
                <b-list-group-item v-for="(answer, index) in answers" :key="index"
                @click="selectAnswer(index)">
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" href="#">Submit</b-button>
            <b-button @click="next" variant="success" href="#">Next Question</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    export default{
        props: {
            currentQuestion: Object,
            next: Function
        },
        computed:{
            answers(){
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                this.shuffle(answers)
                return answers
            }
        },
        methods:{
            shuffle(answers){
                for(let i = answers.length - 1; i > 0; i--){
                    const j = Math.floor(Math.random() * i)
                    const temp = answers[i]
                    answers[i] = answers[j]
                    answers[j] = temp
                }
            },
            selectAnswer(index){
                console.log(index)
            }
        }
    }
</script>