<template>
  <section>
    <div class="qustion-box-container">
      <b-jumbotron>
        <template v-slot:lead>{{ question.question }}</template>

        <hr class="my-4" />
        <b-list-group>
          <b-list-group-item
            :class="[
            !answered && selected === index
              ? 'selected'
              : answered && answers[index] === question.correct_answer
              ? 'correct'
              : answered && index === selected
              ? 'incorrect'
              : '',
          ]"
            @click="!answered ? selectAnswer(index) : ''"
            v-for="(answer, index) of answers"
            :key="index"
          >{{ answer }}</b-list-group-item>
        </b-list-group>
        <b-button @click="submit" variant="primary" :disabled="selected === null || answered">Submit</b-button>
        <b-button @click="next" :disabled="!answered" variant="success">Next</b-button>
      </b-jumbotron>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    question: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selected: null,
      answered: false,
    };
  },
  computed: {
    answers() {
      let answers = [...this.question.incorrect_answers];
      answers.push(this.question.correct_answer);
      for (var i = answers.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1));
        var temp = answers[i];
        answers[i] = answers[j];
        answers[j] = temp;
      }
      for (var x = 0; x < answers.length; x++) {
        answers[x] = answers[x].replace(/&quot;/g, '"');
        answers[x] = answers[x].replace(/&#039;/g, "'");
        answers[x] = answers[x].replace(/&amp;/g, "&");
      }
      return answers;
    },
  },
  watch: {
    question: {
      immediate: true,
      handler() {
        this.selected = null;
        this.answered = false;
        this.question.question = this.question.question.replace(/&quot;/g, '"');
        this.question.question = this.question.question.replace(/&#039;/g, "'");
        this.question.correct_answer = this.question.correct_answer.replace(
          /&quot;/g,
          '"'
        );
        this.question.correct_answer = this.question.correct_answer.replace(
          /&#039;/g,
          "'"
        );
        this.question.question = this.question.question.replace(/&amp;/g, "&");
        this.question.correct_answer = this.question.correct_answer.replace(
          /&amp;/g,
          "&"
        );
      },
    },
  },
  methods: {
    selectAnswer(index) {
      this.selected = index;
    },
    submit() {
      let isCorrect = false;
      this.answered = true;
      this.answers[this.selected] === this.question.correct_answer
        ? (isCorrect = true)
        : (isCorrect = false);

      this.increment(isCorrect);
    },
  },
};
</script>

<style scoped>
* {
  transition-duration: 0.3s;
}
.list-group {
  margin-bottom: 1rem;
}
.list-group :hover {
  background: #eee;
  cursor: pointer;
}
.btn {
  margin-left: 10px;
}

.selected,
.selected:hover {
  background-color: #5bc0de;
}
.correct,
.correct:hover {
  background-color: #5cb85c;
}
.incorrect,
.incorrect:hover {
  background-color: #d9534f;
}
</style>
