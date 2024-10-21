<template>
  <div class="option">
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
  }
};
</script>

<style scoped>
.option {
  margin: 10px;
  padding: 10px;
  background-color: blue;
  color: white;
}
</style>
