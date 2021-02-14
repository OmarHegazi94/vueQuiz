<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in answers" :key="index"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
        >
        {{ answer }}
        </b-list-group-item>
      </b-list-group>


      <div class="btn-container mt-5">
        <b-button 
        class="mr-3" 
        variant="primary" 
        @click="submitAnswer"
        :disabled="selectedIndex === null || answerd">Submit</b-button>

        <b-button @click="next" variant="outline-secondary"
          >Next</b-button>
      </div>


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
      answerd: false
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  watch: {
    // currentQuestion() {
    //   this.selectedIndex = null
    //   this.shuffleAnswers()
    // }

    currentQuestion: {
      immediate: true, // run the watch function when its first passed as prop + every time it updates
      handler() {
        this.selectedIndex = null
        this.answerd = false
        this.shuffleAnswers()
      }

    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer() {
      let isCorrect = false

      if( this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }

      this.answerd = true;

      this.increment(isCorrect)
    }
  }
};
</script>

<style scoped>
  .list-group-item:hover {
    background-color: #eee;
    color: #000;
    cursor: pointer;
  }

  .selected {
    background-color: lightblue;
    color: #000;
  }

  .selected:hover {
    background-color: lightblue;
    color: #000;
  }

  .correct {
    background-color: lightgreen;
  }

  .incorrect {
    background-color: lightred;
  }
</style>