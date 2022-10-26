<template>
  <div class="app-body" v-if="loadedData">
    <div class="progress">
      <div class="bar" :style="progresBar"></div>
      <div class="status">
        {{ questionsAnswers }} out of {{ questions.length }} questions answered
      </div>
    </div>
    <div class="ctr">
      <Transition name="fade" mode="out-in">
        <Questions
          v-if="questionsAnswers < questions.length"
          :questions="questions"
          :questionsAnswersd="questionsAnswers"
          @question-answered="answeredQuestion" />

        <Result
          v-else
          :totalCorrent="totalCorrent"
          :questions="questions"
          :user="user" />
      </Transition>
      <button
        v-if="questionsAnswers === questions.length"
        type="button"
        class="reset-btn"
        @click.prevent="reset">
        Reset
      </button>
    </div>
  </div>
  <LoadingDataSpinner v-else />
</template>

<script>
import Questions from './components/Questions.vue';
import Result from './components/Result.vue';
import LoadingDataSpinner from './components/LoadingDataSpinner.vue'

export default {
  name: 'App',
  components: {Questions, Result,LoadingDataSpinner},
  data() {
    return {
      questions: [],
      totalCorrent: 0,
      questionsAnswers: 0,
      user: [],
      loadedData: false,
    };
  },
  computed: {
    progresBar() {
      let color = '#ff63738e';
      if (this.questionsAnswers === 2) {
        color = '#b4bc0e8e';
      } else if (this.questionsAnswers === 3) {
        color = '#0de5148e';
      }
      return {
        width: `${(this.questionsAnswers / this.questions.length) * 100}%`,
        backgroundColor: color,
      };
    },
  },
  methods: {
    answeredQuestion(selected) {
      console.log(selected);
      this.user.push(selected.selet);
      this.questions.forEach((item) => {
        if (item.question === selected.question) {
          console.log(selected.selet);
          if (item.correct === selected.selet) {
            this.totalCorrent++;
          }
        }
      });
      this.questionsAnswers++;
    },
    async reset() {
      this.questionsAnswers = 0;
      this.totalCorrent = 0;
      this.user = [];
      this.loadedData = false;
      await this.fetchData();
    },
    async fetchData() {
      const URL = 'https://opentdb.com/api.php?amount=3&type=multiple';
      try {
        const res = await fetch(URL);
        const data = await res.json();
        this.loadedData = true;
        this.questions = data.results.map((item) => {
          return {
            question: item.question,
            answers: [...item.incorrect_answers, item.correct_answer].sort(),
            correct: item.correct_answer,
          };
        });
      } catch (err) {
        console.log(err);
      }
    },
  },
  async mounted() {
    await this.fetchData();
  },
};
</script>

<style></style>
