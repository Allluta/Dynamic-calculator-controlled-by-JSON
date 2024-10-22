<template>
  <div class="option" @click="selectOption">
    <h3>{{ option.name }}</h3> 

    <div v-if="option.enableScope && isSelected" class="scope">
      <label for="scopeInput">Scope Value:</label>
      <input type="text" v-model="scopeValue" id="scopeInput" @input="updateScope" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'CalculatorOption',
  props: {
    option: Object,
    isSelected: Boolean 
  },
  data() {
    return {
      scopeValue: ''  
    };
  },
  methods: {
    selectOption() {
      this.$emit('option-selected', this.option);
    },
    updateScope() {
      this.$emit('scope-updated', { id: this.option.id, value: this.scopeValue });
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
.scope {
  margin-top: 10px;
  background-color: #fff;
  padding: 10px;
  border-radius: 5px;
}
</style>
