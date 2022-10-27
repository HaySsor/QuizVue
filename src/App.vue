<template>
  <h1>Test your knowledge 🤔</h1>
  <div v-if="!loadedData">
    <label>How many Question you want ?</label>
    <div class="option">
      <input v-model="howManyQuestion" class="input-number" type="number" />
      <button @click="loadHowManyQuestion">Let's Go</button>
    </div>
  </div>
  <div v-else>
    <div class="app-body" v-if="fetchDataloading">
      <div class="progress">
        <div class="bar" :style="progresBar"></div>
        <div class="status">
          {{ questionsAnswers }} out of {{ questions.length }} questions
          answered
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
  </div>
</template>

<script>
import Questions from './components/Questions.vue';
import Result from './components/Result.vue';
import LoadingDataSpinner from './components/LoadingDataSpinner.vue';

export default {
  name: 'App',
  components: {Questions, Result, LoadingDataSpinner},
  data() {
    return {
      questions: [],
      totalCorrent: 0,
      questionsAnswers: 0,
      user: [],
      loadedData: false,
      howManyQuestion: 1,
      fetchDataloading: false,
    };
  },
  computed: {
    progresBar() {
      let color = '#ff63738e';
      if (this.questionsAnswers >= (this.questions.length / 2)){
        color = '#b4bc0e8e'
      }
      if(this.questionsAnswers  == this.howManyQuestion  ){
        color = 'lightgreen'
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
    async loadHowManyQuestion() {
      this.loadedData = true;
      await this.fetchData(this.howManyQuestion);
    },
    async reset() {
      this.questionsAnswers = 0;
      this.totalCorrent = 0;
      this.user = [];
      this.loadedData = false;
      this.howManyQuestion = 1,
      this.fetchDataloading = false
      await this.fetchData();
    },
    async fetchData(number) {
      const URL = `https://opentdb.com/api.php?amount=${number}&type=multiple`;
      try {
        const res = await fetch(URL);
        const data = await res.json();
        this.fetchDataloading = true
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
};
</script>

<style></style>
