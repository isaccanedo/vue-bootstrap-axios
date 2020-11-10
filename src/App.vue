<template>
  <div id="app">
    <Header
        :numCorrect="numCorrect"
        :numTotal="numTotal"
    />

    <!-- ref: https://bootstrap-vue.js.org/docs/components/layout#how-it-works -->
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="12" offset="0.5">
          <QuestonBox
              v-if="questions.length"
              :currentQuestion="questions[index]"
              :next="next"
              :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>


    <div id="referece link">
      <a href="https://bootstrap-vue.js.org/docs/">https://bootstrap-vue.js.org/docs/</a>
      <a href="https://bootstrap-vue.js.org/docs/components/nav">https://bootstrap-vue.js.org/docs/components/nav</a>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Header from './components/Header.vue'
import QuestonBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestonBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      this.index++
      // console.log("    ---- next")
      // console.log(this.index)
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }

      this.numTotal++
    }
  },
  mounted: function () {
    axios.get('https://opentdb.com/api.php?amount=10&category=18&difficulty=hard&type=multiple').then(
      response => {
        console.log(response);
        this.questions = response.data.results;
      }
    )
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
