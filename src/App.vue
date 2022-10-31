<template>
  <h1>Test your knowledge 🤔</h1>
  <QuizLength
    v-if="!loadedData"
    v-model="howManyQuestion"
    :error="error"
    @loadHowManyQuestionProps="loadHowManyQuestion" />
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
          <QuizResult
            v-else
            :goodAnswers="goodAnswers"
            :questions="questions"
            :users="users" />
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
</template>

<script>
import QuizQuestions from './components/QuizQuestions.vue';
import QuizResult from './components/QuizResult.vue';
import QuizLoadingDataSpinner from './components/QuizLoadingDataSpinner.vue';
import QuizLength from './components/QuizLength.vue';

export default {
  name: 'App',
  components: {QuizQuestions, QuizResult, QuizLoadingDataSpinner, QuizLength},
  data() {
    return {
      questions: [],
      goodAnswers: 0,
      questionsAnswers: 0,
      users: [],
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
    answeredQuestion(selected) {
      console.log(selected);
      this.users.push(selected.select);
      this.questions.forEach((item) => {
        if (item.question === selected.question) {
          if (item.correct === selected.select) {
            this.goodAnswers++;
          }
        }
      });
      this.questionsAnswers++;
    },
    async loadHowManyQuestion() {
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
      this.users = [];
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
