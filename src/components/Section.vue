<template>
  <div class="section">
    <h2>{{ section.title }}</h2>
    <div v-for="option in section.options" :key="option.id">
      <CalculatorOption :option="option" @option-selected="handleOptionSelected" />
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
  methods: {
    handleOptionSelected(option) {
      this.$emit('option-selected', option);
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