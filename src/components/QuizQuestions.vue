<template>
  <div class="questions-ctr">
    <TransitionGroup name="fade">
      <div
        class="single-question"
        v-for="({question, answers, correct}, index) in questions"
        v-show="displayedQuestion === index"
        :key="`question-${index}`">
        <div class="question">{{ question }}</div>
        <div class="answers">
          <div
            class="answer"
            v-for="answer in answers"
            :key="`answer-${index}-${idx}`"
            @click="selectAnswer(question, correct, $event)">
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
    selectAnswer(question, correct, e) {
      this.$emit('userQuestionAnswer', {
        select: e.target.textContent,
        question: question,
        current: correct,
        goodAnswer: e.target.textContent === correct,
      });
    },
  },
  components: {Transition, TransitionGroup},
};
</script>

<style></style>
