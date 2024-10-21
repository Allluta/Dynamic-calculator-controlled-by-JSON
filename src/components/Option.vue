<template>
  <div class="option" @click="selectOption">
    <h3>{{ option.name }}</h3>
    <p>Type: {{ option.type }}</p>
    <p>Must Select: {{ option.mustSelectRequirement }}</p>
    <p>Enable Scope: {{ option.enableScope }}</p>
  </div>
</template>

<script>
export default {
  name: 'CalculatorOption',
  props: {
    option: Object
  },
  methods: {
    selectOption() {
      this.$emit('option-selected', this.option);
    }
  }
};
</script>

<style scoped>
.option {
  margin: 10px;
  padding: 10px;
  background-color: blue;
  color: white;
  cursor: pointer;
}
.option.selected {
  background-color: green;
}
</style>
