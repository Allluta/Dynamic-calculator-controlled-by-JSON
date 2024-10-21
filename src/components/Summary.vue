<template>
  <div class="summary">
    <h2>Summary</h2>
    <p v-if="summary.selectedOptions">Selected Options: {{ summary.selectedOptions }}</p>
    <p v-else>No options selected.</p>
  </div>
</template>

<script>
export default {
  name: 'CalculatorSummary',
  props: {
    summary: Object
  }
};
</script>

<style scoped>
.summary {
  margin-top: 20px;
  padding: 10px;
  background-color: lightgray;
}
</style>
