<template>
  <div class="calculator">
    <h1>Dynamic Calculator</h1>
    <div v-for="section in sections" :key="section.id">
      <CalculatorSection 
        :section="section" 
        @option-selected="updateSelection" 
      />
    </div>
    <CalculatorSummary 
      :summary="summary" 
      @update-final-value="updateFinalValue" 
    />
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
      summary: {
        selectedOptions: '',
        finalValue: null
      },
      currentValue: 0,
      endingOperations: null,
      scopeValues: {}
    };
  },
  mounted() {
    fetch("/data.json")
      .then(response => response.json())
      .then(data => {
        this.sections = data.sections;
        this.endingOperations = data.endingOperationsDefinition;
      })
      .catch(error => {
        console.error("Error loading data:", error);
      });
  },
  methods: {
    updateSelection(option) {
      const section = this.sections.find(sec => sec.options.some(opt => opt.id === option.id));
      if (section) {
        if (option.type === 'single') {
          this.selectedOptions = this.selectedOptions.filter(selected => {
            return !section.options.some(opt => opt.id === selected.id);
          });
          this.selectedOptions.push(option);
        } else if (option.type === 'multi') {
          const index = this.selectedOptions.findIndex(selected => selected.id === option.id);
          if (index === -1) {
            this.selectedOptions.push(option);
          } else {
            this.selectedOptions.splice(index, 1); 
          }
        }
        
        
        this.applyOperationsFromSelectedOptions();
      }
      this.updateSummary();
    },
    updateScope(scope) {
      if (scope.value !== undefined) {
        this.$set(this.scopeValues, scope.id, scope.value);
        this.updateSummary();
      }
    },
    updateFinalValue({ scopeValues, finalValue }) {
      this.scopeValues = scopeValues; 
      this.currentValue = finalValue; 
      this.updateSummary(); 
    },
    applyOperationsFromSelectedOptions() {
      this.currentValue = 0; 

      this.selectedOptions.forEach(option => {
        if (option.operationsIfEnabled) {
          this.currentValue += option.operationsIfEnabled.add || 0;
        }
      });
    },
    applyEndingOperations() {
      let finalValue = this.currentValue;

      if (this.selectedOptions.length > 0 && this.endingOperations) {
        finalValue = (finalValue || 0) * this.endingOperations.finalMultiplier + this.endingOperations.finalAdder;
      } else {
        finalValue = 0; 
      }

      this.summary = {
        selectedOptions: this.selectedOptions.map(option => option.name).join(', '),
        scopeValues: this.scopeValues,
        finalValue: finalValue 
      };
    },
    updateSummary() {
      this.applyEndingOperations();
      console.log("Updated Summary:", this.summary);
    }
  }
};
</script>

<style scoped>
.calculator {
  text-align: center;
}
</style>
