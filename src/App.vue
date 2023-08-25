<template>
  <div>
    <Header />
    <MultipleChoiceQuestion v-for="(question, index) in questions" :key="index" :question="question.question"
      :choices="question.choices" @update-selection="updateSelection(index, $event)" class="question" />
    <div class="finalizar">
      <button @click="showModal = true" v-if="allFieldsSelected">Enviar escolhas</button>
    </div>
    <br>
    <Footer />
    <div class="modal" v-if="showModal" :selectedCounts="selectedCounts" @close-modal="showModal = false">
      <div class="container">
        <div class="title">
          <h1>🎉 Confira seus resultados</h1>
        </div>
        <div class="chart-container">
          <div class="grid-bars">
            <p>Dominante</p>
            <div class="bar a" :style="{ '--a': selectedCounts['a'] * 20 + 'px' }"> 
              {{ selectedCounts['a'] }} ({{ (selectedCounts['a'] / totalSelections * 100).toFixed(2) }}%)
            </div>
          </div>
          <div class="grid-bars">
            <p>Influente</p>
            <div class="bar b" :style="{ '--b': selectedCounts['b'] * 20 + 'px' }">
              {{ selectedCounts['b'] }} ({{ (selectedCounts['b'] / totalSelections * 100).toFixed(2) }}%)
            </div>
          </div>
          <div class="grid-bars">
            <p>Estável</p>
            <div class="bar c" :style="{ '--c': selectedCounts['c'] * 20 + 'px' }">
              {{ selectedCounts['c'] }} ({{ (selectedCounts['c'] / totalSelections * 100).toFixed(2) }}%)
            </div>
          </div>
          <div class="grid-bars">
            <p>Conforme</p>
            <div class="bar d" :style="{ '--d': selectedCounts['d'] * 20 + 'px' }">
              {{ selectedCounts['d'] }} ({{ (selectedCounts['d'] / totalSelections * 100).toFixed(2) }}%)
            </div>
          </div>
        </div>        
        <div class="close">
          <button @click="closeModal">Fechar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import MultipleChoiceQuestion from './components/MultipleChoiceQuestion.vue';
import Footer from './components/Footer.vue';
import { questions } from './mock/mockData.js';

export default {
  components: {
    Header,
    MultipleChoiceQuestion,
    Footer,
  },
  data() {
    return {
      showModal: false,
      questions: questions,
      selectedCounts: {
        a: 0,
        b: 0,
        c: 0,
        d: 0
      }
    };
  },
  computed: {
    allFieldsSelected() {
      return this.questions.every(question => question.selectedLetter);
    },
    totalSelections() {
      return this.selectedCounts['a'] + this.selectedCounts['b'] + this.selectedCounts['c'] + this.selectedCounts['d'];
    }
  },
  methods: {
    closeModal() {
      window.scrollTo({ top: 0, behavior: 'smooth' });
      this.showModal = false;
      setTimeout(() => {
        window.location.reload();
      }, 1200);
    },
    updateSelection(questionIndex, selectedChoice) {
      const previousChoice = this.questions[questionIndex].selectedLetter;
      this.questions[questionIndex].selectedLetter = selectedChoice ? selectedChoice.charAt(0) : '';

      if (previousChoice) {
        this.selectedCounts[previousChoice] -= 1;
      }
      if (selectedChoice) {
        this.selectedCounts[selectedChoice.charAt(0)] += 1;
      }
    }
  }
};
</script>

<style>
.modal {
  background-color: rgba(0, 0, 0, 0.367);
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
}

.container {
  text-align: center;
  background-color: rgb(255, 255, 255);
  border-radius: 10px;
  width: 80%;
  height: 80%;
  margin: 0 auto;
}

.finalizar {
  display: flex;
  justify-content: center;
  margin: 30px 0;

}

.finalizar>button {
  padding: 10px 20px;
  background-color: gold;
  border-radius: 5px;
  font-weight: bold;
  cursor: pointer;
}

.finalizar>button:hover {
  background-color: #9aff98;
  font-weight: bold;
}

.question {
  margin: 0 auto;
  width: 50%;
}

.title {
  margin: 30px 0;
}

.close {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 50%;
}

.close>button {
  padding: 10px 20px;
  background-color: gold;
  border-radius: 5px;
  font-weight: bold;
  cursor: pointer;
}

.chart-container {
  display: grid;
  justify-content: space-around;
  margin-top: 20px;
}

.bar {
  width: 20px;
  height: 30px;
  background-color: blue;
  margin: 10px 0;
  transition: height 0.3s;
}

.a {
  width: var(--a, 0);
  background: rgb(255, 255, 255);
  background: linear-gradient(90deg, rgba(255, 255, 255, 1) 2%, #ff0000 100%);
  border-radius: 0 20px 20px 0;
  display: flex;
  align-items: center;
  font-size: 13px;
}

.b {
  width: var(--b, 0);
  background: rgb(255, 255, 255);
  background: linear-gradient(90deg, rgba(255, 255, 255, 1) 2%, #0000ff 100%);
  border-radius: 0 20px 20px 0;
  display: flex;
  align-items: center;
  font-size: 13px;
}

.c {
  width: var(--c, 0);
  background: rgb(255, 255, 255);
  background: linear-gradient(90deg, rgba(255, 255, 255, 1) 2%, #008000 100%);
  border-radius: 0 20px 20px 0;
  display: flex;
  align-items: center;
  font-size: 13px;
}

.d {
  width: var(--d, 0);
  background: rgb(255, 255, 255);
  background: linear-gradient(90deg, rgba(255, 255, 255, 1) 2%, #ffa500 100%);
  border-radius: 0 20px 20px 0;
  display: flex;
  align-items: center;
  font-size: 13px;
}


.grid-bars {
  display: flex;
  align-items: center;
}

.grid-bars p {
  width: 120px;
  background-color: #f0f0f0;
  box-shadow: 3px 3px 5px 0 #ccc;
  margin-right: 10px;
  padding: 3px;
}</style>