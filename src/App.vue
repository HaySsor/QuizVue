<template>
  <div class="section">
    <h1 class="home-title">Test your knowledge 🤔</h1>
    <QuizIntro
      v-if="!loadedData"
      :error="error"
      @on-submit="loadHowManyQuestion" />
    <div v-else>
      <div class="app-body" v-if="fetchDataLoading">
        <div class="progress">
          <div
            class="bar"
            :class="progressBar"
            :style="{
              width: `${(questionsAnswers / questions.length) * 100}%`,
            }"></div>
          <div class="status">
            {{ questionsAnswers }} out of {{ questions.length }} questions
            answered
          </div>
        </div>
        <div class="ctr">
          <Transition name="fade" mode="out-in">
            <QuizQuestions
              v-if="questionsAnswers < questions.length"
              :questions="questions"
              :displayedQuestion="questionsAnswers"
              @userQuestionAnswer="answeredQuestion" />
            <QuizResult v-else :usersAnswers="usersAnswers" />
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
      <QuizLoadingDataSpinner v-else />
    </div>
  </div>
</template>

<script>
import QuizQuestions from './components/QuizQuestions.vue';
import QuizResult from './components/QuizResult.vue';
import QuizLoadingDataSpinner from './components/QuizLoadingDataSpinner.vue';
import QuizIntro from './components/QuizIntro.vue';

export default {
  name: 'App',
  components: {QuizQuestions, QuizResult, QuizLoadingDataSpinner, QuizIntro},
  data() {
    return {
      questions: [],
      goodAnswers: 0,
      questionsAnswers: 0,
      usersAnswers: [],
      loadedData: false,
      howManyQuestion: 1,
      fetchDataLoading: false,
      error: false,
    };
  },
  computed: {
    progressBar() {
      return {
        half: this.questionsAnswers >= this.questions.length / 2,
        full: this.questionsAnswers == this.howManyQuestion,
      };
    },
  },
  methods: {
    answeredQuestion(userAnswerObj) {
      this.usersAnswers.push(userAnswerObj);
      this.questionsAnswers++;
    },
    async loadHowManyQuestion(value) {
      this.howManyQuestion = value;
      if (this.howManyQuestion <= 0) {
        this.error = true;
        return;
      }
      this.error = false;
      this.loadedData = true;
      await this.fetchData(this.howManyQuestion);
    },
    reset() {
      this.questionsAnswers = 0;
      this.goodAnswers = 0;
      this.usersAnswers = [];
      this.loadedData = false;
      this.howManyQuestion = 1;
      this.fetchDataLoading = false;
    },
    async fetchData(number) {
      const URL = `https://opentdb.com/api.php?amount=${number}&type=multiple`;
      try {
        const res = await fetch(URL);
        const {results} = await res.json();
        this.fetchDataLoading = true;
        this.questions = results.map(
          ({question, incorrect_answers, correct_answer}) => {
            return {
              question: question,
              answers: [...incorrect_answers, correct_answer].sort(),
              correct: correct_answer,
            };
          }
        );
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>
