<template>
  <div class="ctr">
    <Questions
      v-if="questionsAnswers < questions.length"
      :questions="questions" />
    <Result v-else />
    <button type="button" class="reset-btn">Reset</button>
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
      results: data.results,
      questionsAnswers: 0,
    };
  },
};
</script>

<style></style>
