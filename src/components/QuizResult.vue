<template>
  <div>
    <div class="result">
      <h1 class="score">Your score {{ calculateUserScore }}</h1>
      <div class="boxQ" v-for="({question, current, select}) in usersAnswers">
        <h3 class="result-question">{{ question }}</h3>
        <div class="desc">
          <p class="correct">Correct answer : {{ current }}</p>
          <p
            class="user-answer"
            :style="{
              backgroundColor: current === select ? 'lightgreen' : 'lightcoral',
            }">
            Your Answer : {{ select }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuizResult',
  props: {
    usersAnswers: {
      type: Array,
      required: true,
    },
  },
  computed: {
    calculateUserScore() {
      return this.usersAnswers.filter((item) => item.goodAnswer).length;
    },
  },
};
</script>

<style></style>
