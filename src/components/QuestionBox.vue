<template>
    <div>
        <b-jumbotron>
            <template #lead>
                {{questions.question}}
            </template>

            <hr class="my-4">
            <b-list-group>
                <b-list-group-item button
                                   v-for="(item,index) in shuffledAnswers"
                                   :key="index"
                                   @click="onButtonClick(index)"
                                   :class="answerClass(index)">
                    {{item}} 
                </b-list-group-item>
            </b-list-group>
            <b-button variant="primary" 
                      @click="onSubmit"
                      :disabled="selectedIndex ===null || isAnswered">
            Submit
            </b-button>
            <b-button variant="success" 
                      @click="getNextQuestion">
            Next
            </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    import _ from 'lodash';
    export default {
        props: {
            questions: {},
            getNextQuestion: Function,
            increment: Function
        },
        data: function () {
            
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledAnswers: [],
                isAnswered: false,
                eligibleClass: ''
            }
        },

        //DO NOT UPDATE DATA PROPERTIES INSIDE COMPUTED. RETURN A NEW VARIABLE INSTEAD.
        computed:{
             answerList(){
                var answers = []
                answers = [...this.questions.incorrect_answers]
                answers.push(this.questions.correct_answer)
                return answers
            }
        },
        watch: {
            questions: {
                immediate: true,
                handler() {
                    this.selectedIndex = null
                    this.isAnswered = false
                    this.shuffleAnswers()
                }
            }


        },
        methods: {
            onButtonClick: function (index) {
                this.selectedIndex = index
                
            },
            shuffleAnswers() {
                this.selectedIndex = null
                var answers = [...this.questions.incorrect_answers, this.questions.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.questions.correct_answer)
                return this.shuffledAnswers
            },
            onSubmit() {
                var isCorrect = false;
                if (this.selectedIndex === this.correctIndex) { isCorrect = true }
                this.increment(isCorrect)
                this.isAnswered = true
            },
            answerClass(index) {
                this.eligibleClass = 
                !this.isAnswered && this.selectedIndex == index ? "selected" :
                    this.isAnswered && index === this.correctIndex ? "correct" :
                        this.isAnswered && index === this.selectedIndex && index !== this.correctIndex ? "wrong" :
                            ""
             return this.eligibleClass
            }
        }
    }
</script>
<style scoped>
    .list-group {
        margin-bottom:10px;
    }
    .list-group-item:hover {
        background: aliceblue;
        cursor: pointer;
    }
    .btn {
        margin: 0 10px;
    }
    .selected {
        background-color: dodgerblue
    }
    .correct {
        background-color: green;
    }
    .wrong {
        background-color: red
    }
</style>