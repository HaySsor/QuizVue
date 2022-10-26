<template>
  <div class="result">
    <div class="title">
      {{ results[resultIndex].title }}
    </div>
    <div class="desc">{{ results[resultIndex].desc }}</div>
  </div>
</template>

<script>
export default {
  props: ['totalCorrent', 'results'],
  data() {
    return {
      title: '',
      desc: '',
    };
  },
  computed: {
    resultIndex() {
      let index = 0;
      this.results.forEach((e, i) => {
        if (e.min <= this.totalCorrent && e.max >= this.totalCorrent) {
          index = i;
        }
      });
      return index;
    },
  },
};
</script>

<style></style>
