<template>
  <div id="app">
    <Header />
    <b-container class="bv-example-row">
      <b-row class="flex">
        <b-col sm="6">
          <QuestionBox 
            v-if="questions.length"
            :question = "questions[index]"
            :next = "next"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

import Vue from 'vue'
import BootstrapVue from 'bootstrap-vue'

Vue.use(BootstrapVue)

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0
    }
  },
  methods: {
    next() {
      return this.index++;
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&type=multiple', {
      method: 'get'
    })
    .then((result) => result.json())
    .then(jsonData => this.questions = jsonData.results)
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.flex {
  display: flex;
  justify-content: center;
}
</style>
