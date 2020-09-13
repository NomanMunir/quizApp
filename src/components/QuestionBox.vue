<template>
  <div class="container">
    <div class="box">
      <h2>Some question here?</h2>
      <h4>{{ currentQuestion.question}}</h4>
      <div class="options">
        <a
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >{{ answer }}</a>
      </div>
      <div class="btn-group">
        <button
          class="btn-info"
          @click="submitAnswer"
          :disabled="selectedIndex === null || answered"
        >Submit</button>
        <button @click="next" class="btn-primary">Next</button>
      </div>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      answered: false,
      shuffledAnswers: [],
    };
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCurrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCurrect = true;
      }
      this.answered = true;
      this.increment(isCurrect);
    },
    answerClass(index) {
      let answerClass = "";
      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = "incorrect";
      }
      return answerClass;
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.answered = false;
        this.selectedIndex = null;
        this.shuffleAnswers();
      },
    },
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
}
.box {
  border-radius: 15px;
  color: #979797;
  font-family: Arial, Helvetica, sans-serif;
  display: flex;
  flex-direction: column;
  width: 500px;
  height: auto;
  background-color: rgb(220, 233, 233);
  margin: 20px;
}
.box h2 {
  font-family: inherit;
  border-bottom: 1px solid #979797;
  padding: 25px 25px 30px 25px;
  margin: 0 15px;
}
.box h4 {
  font-family: inherit;
  padding: 15px 0;
  margin: 0 15px;
}
.box .options {
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: white;
  margin: 30px;
}
.box .options a {
  text-decoration: none;
  padding: 20px;
  font-family: inherit;
  cursor: pointer;
  border-bottom: 1px solid #979797;
}
.box a:hover {
  color: #979797;
  background-color: #eee;
}

.btn-group {
  display: flex;
  justify-content: center;
  flex-direction: row;
  gap: 10px;
  margin-bottom: 20px;
}
.btn-info {
  border-radius: 5px;
  border: none;
  color: white;
  padding: 10px;
  background-color: rgb(9, 158, 9);
}
.btn-primary {
  border-radius: 5px;
  border: none;
  color: white;
  padding: 10px;
  background-color: blueviolet;
}
.selected {
  color: white;
  background: rgb(125, 106, 143);
}
.correct {
  color: white;
  background: green;
}
.incorrect {
  color: white;
  background: red;
}
:disabled {
  background: rgba(125, 106, 143, 0.253);
}
</style>