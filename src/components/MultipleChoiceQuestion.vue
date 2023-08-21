<template>
  <div>
    <p class="titulo">{{ question }}</p>
    <label v-for="(choice, index) in choices" :key="index">
      <input
        type="radio"
        :id="'choice-' + index"
        :value="choice"
        v-model="selectedChoice"
        @change="updateSelections"
      />
      {{ choice }}
    </label>
    <p>Sua escolha: <span class="escolha">{{ selectedLetters }}</span></p>
  </div>

</template>

<script>
export default {
  props: {
    question: String,
    choices: Array
  },
  data() {
    return {
      selectedChoice: null
    };
  },
  computed: {
    selectedLetters() {
      return this.selectedChoice ? this.selectedChoice.charAt(0).toUpperCase() : '';
    }
  },
  methods: {
    updateSelections() {
      this.$emit('update-selection', this.selectedChoice);
    }
  }
};
</script>

<style scoped>
div {
  display: grid;
  width: 80%;
  border-radius: 6px;
  box-shadow: 3px 3px 5px 0 #ccc;
  padding: 20px;
  margin: 20px auto;
  background-color: rgb(240, 240, 240);
}

.titulo {
  padding-bottom: 10px;
  color: #000;
  font-weight: bold;
}

label {
  padding: 5px 0;
  margin-left: 10px;
  cursor: pointer;
}

input {
  margin-right: 5px;
}

.escolha {
  font-weight: bold;
  color: blue;
}
</style>
