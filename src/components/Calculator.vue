<template>
  <div class="calculator">
    <h1>Dynamic Calculator</h1>
    <div v-for="section in sections" :key="section.id">
      <CalculatorSection :section="section" />
    </div>
    <CalculatorSummary :summary="summary" />
  </div>
</template>

<script>
import CalculatorSection from './Section.vue';
import CalculatorSummary from './Summary.vue';

export default {
  name: 'MainCalculator',
  components: {
    CalculatorSection,
    CalculatorSummary
  },
  data() {
    return {
      sections: [],
      summary: {}
    };
  },
  mounted() {
    // Wczytanie danych z pliku JSON
    fetch("/data.json")
      .then(response => response.json())
      .then(data => {
        this.sections = data.sections;
      })
      .catch(error => {
        console.error("Error loading data:", error);
      });
  }
};
</script>

<style scoped>
.calculator {
  text-align: center;
}
</style>
