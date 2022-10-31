<template>
  <div>
    <div class="result">
      <h1 class="score">Your score {{ goodAnswers }}</h1>
      <div class="boxQ" v-for="({question, correct}, i) in questions">
        <h3 class="result-question">{{ question }}</h3>
        <div class="desc">
          <p class="correct">Correct answer : {{ correct }}</p>
          <p
            class="user-answer"
            :style="{
              backgroundColor:
                correct === users[i] ? 'lightgreen' : 'lightcoral',
            }">
            Your Answer : {{ users[i] }}
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
    questions: {
      type: Array,
      required: true,
    },
    users: {
      type: Array,
      required: true,
    },
    goodAnswers: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style></style>
