<template>
  <div>
    <div class="result">
      <h1 class="score">Your score {{ totalCorrent }}</h1>
      <div class="boxq" v-for="({question, correct}, i) in questions">
        <h3 class="result-question">{{ question }}</h3>
        <div class="desc">
          <p class="corrent">Corrent answer : {{ correct }}</p>
          <p
            class="user-answer"
            :style="{
              backgroundColor:
                correct === user[i] ? 'lightgreen' : 'lightcoral',
            }">
            Your Answer : {{ user[i] }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['questions', 'user', 'totalCorrent'],
  data() {
    return {
      title: '',
      desc: '',
    };
  },
};
</script>

<style></style>
