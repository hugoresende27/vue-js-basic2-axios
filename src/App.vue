<template>
<!--  <HelloWorld msg="Hugo Boss"/>-->
  <div>
    <h1 v-html="this.question">
    </h1>
    <input
        type="radio"
        name="options"
        value="True">
    <label>True</label><br>
    <input
        type="radio"
        name="options"
        value="False">
    <label>False</label><br>
    <button class="send" type="button">Send</button>

  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  data() {
    return {
     question : undefined,
      incorrectAnswers : [],
      correctAnswer : undefined
    }
  },
  computed: {
    answers() {
      let answers = [...this.incorrectAnswers];
      // let answers = JSON.parse( JSON.stringify(this.incorrectAnswers));
      answers.push(this.correctAnswer);
      return answers;
    }
  },
  created() {
    let apiUrl = 'https://opentdb.com/api.php?amount=1&category=18';
    this.axios.
    get(apiUrl).
    then((response) => {
      this.question = response.data.results[0].question;
      this.correctAnswer = response.data.results[0].correct_answer;
      this.incorrectAnswers = response.data.results[0].incorrect_answers;
      console.log(response.data.results[0])
      // let res = response.data.results;
    })
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
      padding: 8px;
      font-weight: bold;
      border: 1px solid black;
    }
  }
}
</style>
