<template>
  <div class="question-box-container">
    <b-jumbotron>

      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers" 
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
          >
            {{ answer }}
        </b-list-group-item>
        
      </b-list-group>

      <b-button
        class="btn" 
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || isAnswered"
        >
          Submit
      </b-button>
      <b-button
        class="btn" 
        variant="success"
        @click="next" 
        >
          Next
      </b-button>
  </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      isAnswered: false
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers    
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null
        this.shuffleAnswers(),
        this.isAnswered = false
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
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer() {
      let isCorrect = false

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      
      this.isAnswered = true
      this.increment(isCorrect)
    },
    answerClass(index) {
      let answerClass = ''

      if (!this.isAnswered && this.selectedIndex === index) {
        answerClass = 'selected'
      } else if (this.isAnswered && this.correctIndex === index) {
        answerClass = 'correct'
      } else if (this.isAnswered && 
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = 'incorrect'
      }
      return answerClass
    }
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
  margin: 0 5px;
}
.selected {
  background-color: skyblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}

</style>