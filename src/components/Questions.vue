<template>
  <div class="questions-ctr">
    <TransitionGroup name="fade">
      <div
        class="single-question"
        v-for="(question, qi) in questions"
        v-show="questionsAnswersd === qi"
        :key="question.q">
        <div class="question">{{ question.q }}</div>
        <div class="answers">
          <div
            class="answer"
            v-for="answer in question.answers"
            :key="answer.text"
            @click.prevent="selectAnswer(answer.is_correct)">
            {{ answer.text }}
          </div>
        </div>
      </div>
    </TransitionGroup>
  </div>
</template>

<script>
import {Transition, TransitionGroup} from 'vue';

export default {
  props: ['questions', 'questionsAnswersd'],
  emits: ['question-answered'],
  computed: {
    progresBar() {
      return {
        width: `${(this.questionsAnswersd / this.questions.length) * 100}%`,
      };
    },
  },
  methods: {
    selectAnswer(correct) {
      this.$emit('question-answered', correct);
    },
  },
  components: {Transition, TransitionGroup},
};
</script>

<style></style>
