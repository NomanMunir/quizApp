<template>
  <div id="app">
    <Header :numCurrect="numCurrect" :numTotal="numTotal" />

    <QuestionBox
      v-if="questions.length"
      :currentQuestion="questions[index]"
      :next="next"
      :increment="increment"
    />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      numCurrect: 0,
      numTotal: 0,
      index: 0,
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCurrect) {
      if (isCurrect) {
        this.numCurrect++;
      }
      this.numTotal++;
    },
  },
  mounted: function () {
    fetch(
      "https://opentdb.com/api.php?amount=10&category=15&difficulty=easy&type=multiple"
    )
      .then((res) => res.json())
      .then((json) => {
        this.questions = json.results;
      });
  },
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
