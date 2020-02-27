<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
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
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    };
  },
  methods: {
    reset() {
      this.$alert(
        `Congratulations you have ${this.numCorrect} correct answers!`
      );
      this.numTotal = 0;
      this.numCorrect = 0;
      fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
        method: "GET"
      })
        .then(response => {
          return response.json();
        })
        .then(jsonData => {
          this.questions = jsonData.results;
        });
      this.index = 0;
    },
    next(isAnswered) {
      if (!isAnswered) {
        this.numTotal++;
      }
      if (this.numTotal === 10) {
        this.reset();
      }
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
      if (this.numTotal === 10) {
        this.reset();
      }
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
      method: "GET"
    })
      .then(response => {
        return response.json();
      })
      .then(jsonData => {
        this.questions = jsonData.results;
      });
  }
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