<template>
  <div>
    <div class="result">
      <h1 class="score">Your score {{ goodAnswers }}</h1>
      <div class="boxq" v-for="({question, correct}, i) in questions">
        <h3 class="result-question">{{ question }}</h3>
        <div class="desc">
          <p class="corrent">Corrent answer : {{ correct }}</p>
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
  props: ['questions', 'users', 'goodAnswers'],
  data() {
    return {
      title: '',
      desc: '',
    };
  },
};
</script>

<style></style>
