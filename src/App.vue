<template>
    <div id="app">
        <Header 
                :numTotal="numTotal"
                :numCorrect="numCorrect"
                />
        <b-container>
            <b-row >
                <b-col sm="6" offset="3"> 
                <QuestionBox 
                             v-if="apiResult.length > 0"
                             :questions="apiResult[index]"
                             :getNextQuestion="getNextQuestion"
                             :increment="increment"
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
        mounted: function () {
            fetch('https://opentdb.com/api.php?amount=10&category=17&type=multiple', { method: 'GET' })
                .then((response) => { return response.json() })
                .then((data) => {
                    console.log(data)
                    this.apiResult = data.results
                    console.log(this.apiResult)
                }
            );
        },
        data(){
            return {
                apiResult: [],
                index: 0,
                numTotal: 0,
                numCorrect:0
            }
        },
        methods:
        {
            getNextQuestion() {
                this.index++;
            },
            increment(isCorrect) {
                if (isCorrect) { this.numCorrect++ }
                this.numTotal++
            }
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
