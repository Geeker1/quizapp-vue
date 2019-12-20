<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="feedback(index)"
          >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button 
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >Submit</b-button>
      <b-button @click='next' variant="success" :disabled="available" href="#">
        Next
    </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
  import _ from 'lodash';
  export default {
    props:{
      currentQuestion: Object,
      next: Function,
      increment: Function,
      available: Boolean
    },
    data(){
      return {
        selectedIndex: null,
        answered: false,
        shuffledAnswers: [],
        correctIndex: null
      }
    },
    watch:{
      currentQuestion:{
        immediate: true,
        handler(){
          this.selectedIndex = null;
          this.answered = false;
          this.shuffleAnswers();
        }
      }
    },
    methods:{
      selectAnswer(index){
        this.selectedIndex = index;
      },
      feedback(index){
        let answerClass = ''

        if(!this.answered && this.selectedIndex === index){
          answerClass = 'selected'
        }else if(this.answered && this.correctIndex === index){
          answerClass = 'correct'
        }else if(this.answered && this.selectedIndex == index && this.correctIndex !== index){
          answerClass = 'incorrect'
        }

        return answerClass
      },
      submitAnswer(){
        let isCorrect = false;
        if (this.selectedIndex === this.correctIndex){
          isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect);
      },
      shuffleAnswers(){
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers);
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      }
    }
  }
</script>


<style scoped>
  .list-group{
    margin-bottom: 15px;
  }
  .list-group-item:hover{
    cursor: pointer;
    background-color: #eee;
  }
  .selected{
    background-color: lightblue;
  }
  .correct{
    background-color: lightgreen;
  }
  .incorrect{
    background-color: red;
  }
  .btn{
    margin: 0 5px;
  }
</style>
