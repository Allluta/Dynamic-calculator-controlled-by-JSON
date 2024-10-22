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
  margin: 10px 0;
  padding: 15px;
  background-color: #007BFF;
  color: white;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s ease;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.option.selected {
  background-color: #28a745;
}

.option:hover {
  background-color: #0056b3;
}

.scope {
  margin-top: 10px;
  padding: 10px;
  background-color: #fff;
  border-radius: 5px;
  border: 1px solid #ccc;
}

</style>
