<template>
  <div class="app-body">
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

        <Result v-else :totalCorrent="totalCorrent" :results="results" />
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
</template>

<script>
import Questions from './components/Questions.vue';
import Result from './components/Result.vue';
import data from './data.json';

export default {
  name: 'App',
  components: {Questions, Result},
  data() {
    return {
      questions: data.questions,
      totalCorrent: 0,
      results: data.results,
      questionsAnswers: 0,
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
    answeredQuestion(correct) {
      if (correct) {
        this.totalCorrent++;
      }
      this.questionsAnswers++;
    },
    reset() {
      this.questionsAnswers = 0;
      this.totalCorrent = 0;
    },
  },
};
</script>

<style></style>
