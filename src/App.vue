<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3" >
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
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
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      // data: じゃなくて data(){} にする理由は
      // ただデータを返すだけじゃなくて、複雑な処理が入ってきても対応できるようにするため
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    // Require Object Literal Shorthand Syntax
    // ref. https://eslint.org/docs/rules/object-shorthand
    next() {
      this.index++
    },
    increment(isCorrect) {
      if(isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get'
    }).then(response => {
      // response => {type: "cors", url: "https://opentdb.com/api.php?amount=10&category=27&type=multiple", redirected: false, status: 200, ok: true, ...}
      // response.json() => Promise object
      // Promise objects have then method.
      // ref. https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
      return response.json()
    }).then((jsonData) => {
      this.questions = jsonData.results
    })
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
