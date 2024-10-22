<template>
  <div class="section">
    <h2>{{ section.title }}</h2>
    <p v-if="section.options.length > 0">
      <span v-if="section.options[0].type === 'single'">Select one option:</span>
      <span v-if="section.options[0].type === 'multi'">Select multiple options:</span>
    </p>
    <div v-for="option in section.options" :key="option.id">
      <CalculatorOption 
        :option="option"
        :isSelected="selectedOptions.includes(option.id)"  
        @option-selected="handleOptionSelected"
        @scope-updated="handleScopeUpdated"
      />
    </div>
  </div>
</template>

<script>
import CalculatorOption from './Option.vue';

export default {
  name: 'CalculatorSection',
  props: {
    section: Object
  },
  components: {
    CalculatorOption
  },
  data() {
    return {
      selectedOptions: [],
      scopeData: {}
    };
  },
  methods: {
    handleOptionSelected(option) {
    if (option.type === 'single') {
      if (this.selectedOptions.includes(option.id)) {
        this.selectedOptions = []; 
      } else {
        this.selectedOptions = [option.id]; 
      }
    } else if (option.type === 'multi') {
      const index = this.selectedOptions.findIndex(id => id === option.id);
      if (index === -1) {
        this.selectedOptions.push(option.id); 
      } else {
        this.selectedOptions.splice(index, 1); 
      }
    }
    this.$emit('option-selected', option);
    },
    handleScopeUpdated(scope) {
      this.$set(this.scopeData, scope.id, scope.value); 
      this.$emit('scope-updated', { id: scope.id, value: scope.value });
    }
  }
};
</script>

<style scoped>
.section {
  margin: 20px 0;
  padding: 20px;
  background-color: #f0f0f0;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  color: #333;
}

h2 {
  margin-bottom: 10px;
  font-size: 1.4rem;
  color: #007BFF;
}

</style>
