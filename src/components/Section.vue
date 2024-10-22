<template>
  <div class="section">
    <h2>{{ section.title }}</h2>
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
      selectedOptions: [] ,
      scopeData: {}
    };
  },
  methods: {
    handleOptionSelected(option) {
      if (this.section.type === 'single') {
        this.selectedOptions = [option.id];
      } else if (this.section.type === 'multi') {
        if (this.selectedOptions.includes(option.id)) {
          this.selectedOptions = this.selectedOptions.filter(id => id !== option.id);
        } else {
          this.selectedOptions.push(option.id);
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
  margin: 20px;
  padding: 20px;
  background-color: red;
  color: white;
}
</style>
