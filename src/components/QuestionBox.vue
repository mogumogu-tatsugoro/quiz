<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <!-- :class に配列を渡すことで class属性 を複数指定できる -->
        <b-list-group-item
          v-for="(answer, index) in this.shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
// npm i lodash
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: []
    }
  },
  computed: {
    answers() {
      return [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
    }
  },
  // watch は props が変更されたときに発火
  // updated は DOM が変更されたときに発火
  watch: {
    // 初回の処理が不要な場合は
    // e.g. watch: { currentQuestion() { process } }
    currentQuestion: {
      // immediate: true にすることで初回の prop が入った時点で発火する
      immediate: true,
      // handler に処理を記述
      handler() {
        this.selectedIndex = null
        this.shuffleAnswers()
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
    }
  }
}
</script>

<style scoped>
  .list-group {
    margin-bottom: 10px;
  }
  .list-group-item:hover {
    background-color: #EEE;
    cursor: pointer;
  }
  .btn {
    margin: 0 5px;
  }
  .selected {
    background-color: lightblue;
  }
  .correct {
    background-color: lightgreen;
  }
  .incorrect {
    background-color: lightpink;
  }
</style>
