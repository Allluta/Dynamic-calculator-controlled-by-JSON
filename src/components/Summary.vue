<template>
  <div class="summary">
    <h2>Summary</h2>
    <p v-if="summary.selectedOptions">Selected Options: {{ summary.selectedOptions }}</p>
    <p v-else>No options selected.</p>

    <div v-if="summary.scopeValues && Object.keys(summary.scopeValues).length > 0">
      <h3>Scope Values:</h3>
      <ul>
        <li v-for="(value, id) in summary.scopeValues" :key="id">
          Option {{ id }} Scope: {{ value }}
        </li>
      </ul>
    </div>

    <div v-if="summary.finalValue !== null">
      <h3>Total Cost: {{ summary.finalValue.toFixed(2) }} zł</h3>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CalculatorSummary',
  props: {
    summary: {
      type: Object,
      default: () => ({
        selectedOptions: '',
        scopeValues: {},
        finalValue: null
      }) 
    }
  }
};
</script>

<style scoped>
.summary {
  margin-top: 30px;
  padding: 15px;
  background-color: #e9ecef;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

h3 {
  color: #007BFF;
  margin-bottom: 10px;
}

p, ul {
  color: #555;
}

</style>
