<template>
  <div class="calculator">
    <h1>Dynamic Calculator</h1>
    <div v-for="section in sections" :key="section.id">
      <CalculatorSection 
        :section="section" 
        @option-selected="updateSelection" 
      />
    </div>

    <div v-if="selectedOptions.length >= 2" class="working-hours">
      <label for="hours">Enter working hours:</label>
      <input type="number" v-model.number="workingHours" id="hours" min="0" />
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
      scopeValues: {},
      workingHours: 0, 
      hourlyRate: 50 
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
  watch: {
  workingHours() { 
    this.updateSummary(); 
  }
  },
  methods: {
  updateSelection(option) {
    const section = this.sections.find(sec => sec.options.some(opt => opt.id === option.id));
    if (section) {
      if (option.type === 'single') {
        const isSelected = this.selectedOptions.some(selected => selected.id === option.id);
        if (isSelected) {
          this.selectedOptions = this.selectedOptions.filter(selected => selected.id !== option.id);
        } else {
          this.selectedOptions = this.selectedOptions.filter(selected => {
            return !section.options.some(opt => opt.id === selected.id);
          });
          this.selectedOptions.push(option);
        }
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

  const section1Selected = this.selectedOptions.some(option => option.id >= 101 && option.id <= 103); 
  const section2Selected = this.selectedOptions.some(option => option.id >= 201 && option.id <= 203); 

  if (section1Selected && section2Selected && this.workingHours > 0) {
    finalValue = (finalValue || 0) * this.endingOperations.finalMultiplier + this.endingOperations.finalAdder;
    finalValue = finalValue * this.workingHours * this.hourlyRate; 
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
  max-width: 600px;
  margin: 0 auto;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  font-family: Arial, sans-serif;
}

h1, h2 {
  color: #333;
}

.working-hours {
  margin-top: 20px;
}

.working-hours input {
  margin-left: 10px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100px;
}

input[type="number"]:focus {
  border-color: #007BFF;
  outline: none;
}

</style>
