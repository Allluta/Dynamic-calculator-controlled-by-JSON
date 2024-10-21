<template>
    <div class="calculator">
      <h1>Dynamic Calculator</h1>
      <Section />
      <Option />
      <Summary />
    </div>
  </template>
  
  <script>
  import Section from './Section.vue';
  import Option from './Option.vue';
  import Summary from './Summary.vue';
  
  export default {
    name: 'MainCalculator',
    components: {
      Section,
      Option,
      Summary,
    },
  };
  </script>
  
  <style scoped>
  .calculator {
    text-align: center;
  }
  </style>
  