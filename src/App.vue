<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length && index < 20"
            :question="questions[index]"
            :next="next"
            :increment="increment"
          />
          <Completed v-else :result="numCorrect/numTotal" />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import Completed from "./components/Completed.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
    Completed,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
        //this.numTotal++
      }
      this.numTotal++;
    },
  },
  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=20&type=multiple")
      .then((response) => response.json())
      .then((response) => (this.questions = response.results));
  },
};
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
