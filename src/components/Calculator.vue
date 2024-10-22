<template>
  <div class="calculator">
    <h1>Dynamic Calculator</h1>
    <div v-for="section in sections" :key="section.id">
      <CalculatorSection 
        :section="section" 
        @option-selected="updateSelection" 
      />
    </div>

    <!-- Pole na godziny pracy, pojawia się po wyborze opcji z obu sekcji -->
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
      workingHours: 0, // Nowa zmienna przechowująca godziny pracy
      hourlyRate: 50 // Stawka godzinowa
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
  workingHours() { // Usunięcie parametru newHours
    this.updateSummary(); // Aktualizuj podsumowanie za każdym razem, gdy liczba godzin się zmienia
  }
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
            this.selectedOptions.splice(index, 1); // Usuń z listy
          }
        }

        // Zastosuj operacje po zaktualizowaniu wybranych opcji
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
      this.scopeValues = scopeValues; // Zaktualizuj wartości zakresu
      this.currentValue = finalValue; // Zaktualizuj bieżącą wartość
      this.updateSummary(); // Aktualizuj podsumowanie
    },
    applyOperationsFromSelectedOptions() {
      this.currentValue = 0; // Reset current value

      this.selectedOptions.forEach(option => {
        if (option.operationsIfEnabled) {
          this.currentValue += option.operationsIfEnabled.add || 0;
        }
      });
    },
    applyEndingOperations() {
      let finalValue = this.currentValue;

      // Jeśli użytkownik wprowadził godziny pracy, uwzględniamy je w kalkulacji
      if (this.selectedOptions.length > 0 && this.endingOperations && this.workingHours > 0) {
        finalValue = (finalValue || 0) * this.endingOperations.finalMultiplier + this.endingOperations.finalAdder;
        finalValue = finalValue * this.workingHours * this.hourlyRate; // Przemnożenie przez godziny pracy i stawkę
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
      console.log("Updated Summary:", this.summary); // Log the updated summary
    }
  }
};
</script>

<style scoped>
.calculator {
  text-align: center;
}

.working-hours {
  margin-top: 20px;
}
</style>
