<template>
  <div>
    <p v-html="question.question"></p>

    <b-list-group>
      <b-list-group-item
            v-for="(item, i) in answerList"
            :key="i"
            @click="selectAnswer(i)"
            :class="answerClass(i)"
            v-html="item"

      ></b-list-group-item>
    </b-list-group>

    <b-button
            variant="info"
            :disabled="selectedIndex === null || !enableClick"
            @click="checkAnswer"
    >
      Check Result
    </b-button>

    <b-button
            variant="primary"
            :disabled="isDisabled"
            @click="next"
    >
      Next
    </b-button>
  </div>
</template>

<script>
  import _ from 'lodash'

export default {
  name: 'QuestionBox',
  props: {
    question: Object,
    next: Function,
    incrementCount: Function
  },
  data() {
    return {
      indexCorrect: null,
      shuffledAnswers: [],
      selectedIndex: null,
      enableClick: true,
      isDisabled: true
    }
  },
  watch: {
    question: {
      immediate: true,
      handler() {
        this.enableClick = true
        this.indexCorrect = null
        this.selectedIndex = null
        this.shuffleAnswers()
        this.isDisabled = true
      }
    }
  },
  computed: {
    answerList() {
      const answerList = [...this.question['incorrect_answers']]
      answerList.push(this.question['correct_answer'])
      return answerList
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },
    shuffleAnswers() {
      let answers = [...this.question['incorrect_answers'], this.question['correct_answer']]
      this.shuffledAnswers = _.shuffle(answers)
      this.indexCorrect = this.shuffledAnswers.indexOf(this.question['correct_answer'])
    },
    checkAnswer() {
      this.enableClick = false
      this.incrementCount(this.shuffledAnswers[this.selectedIndex] === this.question['correct_answer'])
      this.isDisabled = false
    },
    answerClass(i) {
      let a = ''
      if ( this.enableClick && this.selectedIndex === i ) {
        a = 'selected'
      } else if ( !this.enableClick && i === this.indexCorrect ) {
        a = 'correct'
      } else if ( !this.enableClick && this.selectedIndex === i && this.indexCorrect !== i ) {
        a = 'incorrect'
      }
      return a
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .list-group {
    width: 50%;
    margin: 20px auto;
  }

  .list-group-item:hover {
    background: salmon;
    cursor: pointer;
  }

  .btn {
    margin: 0 5px;
  }

  .selected,
  .selected:hover {
    background: salmon;
  }
  .correct,
  .correct:hover {
    background: green;
  }
  .incorrect,.incorrect:hover {
    background: red;
  }
</style>
