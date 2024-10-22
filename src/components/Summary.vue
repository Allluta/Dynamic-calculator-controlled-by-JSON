<template>
  <div class="summary">
    <h2>Summary</h2>
    <p v-if="summary.selectedOptions">Selected Options: {{ summary.selectedOptions }}</p>
    <p v-else>No options selected.</p>
    
    <div v-if="summary && summary.scopeValues && Object.keys(summary.scopeValues).length > 0">
      <h3>Scope Values:</h3>
      <ul>
        <li v-for="(value, id) in summary.scopeValues" :key="id">
          Option {{ id }} Scope: {{ value }}
        </li>
      </ul>
    </div>
  
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
