<template>
  <div>

    <ScoreBoard :winCount="this.winCount" :lossCount="this.lossCount"/>

    <template v-if="this.question">

      <h1 v-html="this.question">
      </h1>

      <template v-for="(answer, index) in this.answers" :key="index">
        <input
            :disabled="this.answerSubmitted"
            type="radio"
            name="options"
            :id="'option-' + index"
            :value=answer
            v-model="chosenAnswer">

        <label :for="'option-' + index" v-html="answer"></label><br />
      </template>

      <button @click="this.submitAnswer"  class="send" type="button">Send</button>

      <section class="result" v-if="this.answerSubmitted">
        <template v-if="this.chosenAnswer === this.correctAnswer">
          <h4 v-html="'&#9989; Congratulations, the answer' + this.correctAnswer + ' is correct.'"></h4>
        </template>
        <template v-else>
          <h4 v-html="'&#10060;  I`m sorry, you picked the wrong answer. The correct is ' + this.correctAnswer +'.'"></h4>
        </template>
        <button @click="this.getNewQuestion()" class="send" type="button">Next question</button>
      </section>

    </template>


  </div>
</template>

<script>

import ScoreBoard from "@/components/ScoreBoard.vue";
export default {
  name: 'App',
  components: {
    ScoreBoard
  },
  data() {
    return {
     question : undefined,
      incorrectAnswers : [],
      correctAnswer : undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      winCount: 0,
      lossCount: 0
    }
  },
  computed: {
    answers() {
      let answers = [...this.incorrectAnswers];
      // let answers = JSON.parse( JSON.stringify(this.incorrectAnswers));
      // answers.push(this.correctAnswer);
      answers.splice(Math.round(Math.random() * answers.length),0,this.correctAnswer);
      return answers;
    },
  },
  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) {
        alert('Pick one of the options')
      } else {
        this.answerSubmitted = true;
        if(this.chosenAnswer === this.correctAnswer) {
          console.log('Win')
          this.winCount ++;
        } else {
          console.log('Loose')
          this.lossCount ++;
        }
      }
    },
    getNewQuestion() {
      this.answerSubmitted = false;
      this.chosenAnswer = undefined;
      this.question = undefined;

      let apiUrl = 'https://opentdb.com/api.php?amount=1&category=18';
      this.axios.
      get(apiUrl).
      then((response) => {
        this.question = response.data.results[0].question;
        this.correctAnswer = response.data.results[0].correct_answer;
        this.incorrectAnswers = response.data.results[0].incorrect_answers;
        console.log(this.correctAnswer)
      })
    }
  },
  created() {
    this.getNewQuestion()
  }
}


//https://opentdb.com/api.php?amount=10&difficulty=easy&type=multiple



</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;
  h1 {
    margin-top: 40px;
  }

  input[type='radio']{
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }

  section.score {
    border-bottom: 1px solid black;
    padding: 24px;
    font-size: 18px;
    span {
      padding: 10px;
      font-weight: bold;
      border: 1px solid black;
      margin: 5px;
    }
  }
}
</style>
