<template>
  <div class="calculator">
    <h1>Dynamic Calculator</h1>
    <div v-for="section in sections" :key="section.id">
      <CalculatorSection :section="section" @option-selected="updateSelection" />
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
      selectedOptions: [],
      summary: {}
    };
  },
  mounted() {
    fetch("/data.json")
      .then(response => response.json())
      .then(data => {
        this.sections = data.sections;
      })
      .catch(error => {
        console.error("Error loading data:", error);
      });
  },
  methods: {
    updateSelection(option) {
      const index = this.selectedOptions.findIndex(selected => selected.id === option.id);
      if (index === -1) {
        this.selectedOptions.push(option);
      } else {
        this.selectedOptions.splice(index, 1);
      }

      this.updateSummary();
    },
    updateSummary() {
      this.summary = {
        selectedOptions: this.selectedOptions.map(option => option.name).join(', ')
      };
    }
  }
};
</script>

<style scoped>
.calculator {
  text-align: center;
}
</style>
