<template>
  <div class="questions-ctr">
    <TransitionGroup name="fade">
      <div
        class="single-question"
        v-for="({question, answers}, qi) in questions"
        v-show="displayedQuestion === qi"
        :key="Math.random()">
        <div class="question">{{ question }}</div>
        <div class="answers">
          <div
            class="answer"
            v-for="answer in answers"
            :key="Math.random()"
            @click="selectAnswer(question, $event)">
            {{ answer }}
          </div>
        </div>
      </div>
    </TransitionGroup>
  </div>
</template>

<script>
import {Transition, TransitionGroup} from 'vue';

export default {
  props: ['questions', 'displayedQuestion'],
  emits: ['question-answered'],
  computed: {
    progresBar() {
      return {
        width: `${(this.displayedQuestion / this.questions.length) * 100}%`,
      };
    },
  },
  methods: {
    selectAnswer(question, e) {
      this.$emit('question-answered', {
        selet: e.target.textContent,
        question: question,
      });
    },
  },
  components: {Transition, TransitionGroup},
};
</script>

<style></style>
