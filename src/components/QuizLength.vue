<template>
  <form>
    <label>Choose a number of questions min 1 max 50</label>
    <div class="option">
      <input
        :value="modelValue"
        @input="passValueToParent"
        class="input-number"
        :class="addError"
        type="number" />
      <button @click.prevent="loadHowManyQuestionProps">Let's Go</button>
    </div>
    <p v-show="error" class="error">Sorry must be more than 0 🤨</p>
  </form>
</template>

<script>
export default {
  name: 'QuizLength',
  model: {
    event: 'change',
  },
  props: {
    modelValue: {
      type: Number,
      required: true,
    },
    error: {
      type: Boolean,
      required: true,
    },
  },
  computed: {
    addError() {
      return {
        'error-input': this.error,
      };
    },
  },
  methods: {
    loadHowManyQuestionProps() {
      this.$emit('loadHowManyQuestionProps');
    },
    passValueToParent(e) {
      this.$emit('update:modelValue', parseInt(e.target.value));
    },
  },
};
</script>
