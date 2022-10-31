<template>
  <div class="questions-ctr">
    <TransitionGroup name="fade">
      <div
        class="single-question"
        v-for="({question, answers}, qi) in questions"
        v-show="displayedQuestion === qi"
        :key="qi">
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
  name: 'QuizQuestions',
  props: {
    questions: {
      type: Array,
      required: true,
    },
    displayedQuestion: {
      type: Number,
      required: true,
    },
  },
  emits: ['userQuestionAnswer'],
  methods: {
    selectAnswer(question, e) {
      this.$emit('userQuestionAnswer', {
        select: e.target.textContent,
        question: question,
      });
    },
  },
  components: {Transition, TransitionGroup},
};
</script>

<style></style>
