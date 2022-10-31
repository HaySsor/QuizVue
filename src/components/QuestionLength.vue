<template>
  <div>
    <label>Choose a number of questions min 1 max 50</label>
    <div class="option">
      <input
        :value="modelValue"
        @input="passValueToParent"
        class="input-number"
        :class="{
          'error-input': error,
        }"
        type="number" />
      <button @click="loadHowManyQuestionProps">Let's Go</button>
    </div>
    <p v-show="error" class="error">Sory must be more than 0 🤨</p>
  </div>
</template>

<script>
export default {
  model: {
    event: 'change',
  },
  props: {
    modelValue: Number,
    error: Boolean,
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
