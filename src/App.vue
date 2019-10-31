<template>
  <div id="app">
    <Header
      :total="total"
      :correctNum="correctNum"
    ></Header>
    <QuestionBox
      v-if="list.length"
      :question="list[index]"
      :next="next"
      :incrementCount="incrementCount"
      :total="total"
    />
  </div>
</template>

<script>

import QuestionBox from './components/QuestionBox.vue'
import Header from './components/Header.vue'

export default {
  name: 'app',
  components: {
    QuestionBox,
    Header
  },
  data() {
    return {
      list: [],
      index: 0,
      isCorrect: false,
      correctNum: 0,
      total: 0
    }
  },
  methods: {
    next() {
      if (this.total < this.list.length) {
        this.index++
      }
    },
    incrementCount(checkVar) {
      if (checkVar) {
        this.correctNum++
      }
      this.total++
    }
  },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&type=multiple', {
      method: 'get'
    })
    .then(res => {
      return res.json()
    })
    .then(dataJson => {
      this.list = dataJson.results
    })
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
</style>
