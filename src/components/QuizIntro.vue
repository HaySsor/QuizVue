<template>
  <form @submit.prevent="$emit('onSubmit', value)">
    <label>Choose a number of questions min 1 max 50</label>
    <div class="option">
      <input
        v-model="value"
        @input="passValueToParent"
        class="input-number"
        :class="hasError"
        type="number" />
      <button>Let's Go</button>
    </div>
    <p v-show="error" class="error">Sorry must be more than 0 🤨</p>
  </form>
</template>

<script>
export default {
  name: 'QuizLength',
  data() {
    return {
      value: 1,
    };
  },
  props: {
    error: {
      type: Boolean,
      required: true,
    },
  },
  computed: {
    hasError() {
      return {
        'error-input': this.error,
      };
    },
  },
};
</script>
