<template>
  <!-- ref: https://bootstrap-vue.js.org/docs/components/jumbotron-->
  <div>
    <b-jumbotron lead="Bootstrap v4 Components for Vue.js 2">
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <!-- ref: https://bootstrap-vue.js.org/docs/components/list-group-->
      <b-list-group>
        <b-list-group-item
            v-for="answer, index in answers"
            :key="index"
            :class="answerClass(index)"
            @click="selectAnswer(index)">
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
          variant="primary"
          @click="submitAnswer"
          :disabled="selectedIndex === null || answered"
      >Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
  import _ from 'lodash'  // https://github.com/lodash/lodash
  export default {
    props: {
      currentQuestion: Object,
      next: Function,
      increment: Function
    },
    data() {
      return {
        selectedIndex: null,
        correctIndex:null,
        shuffledAnswers: [],
        answered: false
      }
    },
    computed: {
      answers() {
        let answers = [...this.currentQuestion.incorrect_answers]
         answers.push(this.currentQuestion.correct_answer);
        return answers
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler() {
          this.selectedIndex = null
          this.answered = false
          this.shuffleAnswers()
        }
      }
      // currentQuestion () {
      //   this.selectedIndex = null
      //   this.shuffleAnswers()
      // }
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
        // console.log('selectAnswer')
        // console.log(this.selectedIndex)
      },
      submitAnswer() {
        let isCorrect = false
        // console.log(
        //   'this.selectedIndex = ', this.selectedIndex,
        //   ', this.correctIndex = ', this.correctIndex)

        if (this.selectedIndex === this.correctIndex) {

          isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
      },
      shuffleAnswers() {
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      },
      answerClass(index) {
        let k = ""

        if(!this.answered && this.selectedIndex === index)
          k = 'selected'
        else if (this.answered && this.correctIndex === index)
          k = 'correct'
        else if (this.answered && this.selectedIndex === index && this.correctIndex !== index)
          k = 'incorrect'

        return k
      },
    },
    mounted() {
      this.shuffleAnswers()
      console.log(this.currentQuestion)
    }
  }
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px;
  }
  .list-group-item:hover {
    background-color: #EEE;
    cursor: pointer;
  }
 .btn {
   margin: 0 10px;
 }
  .selected {
    background-color: lightblue;
  }
  .correct {
    background-color: lightgreen;
  }
  .incorrect {
    background-color: red;
  }
</style>