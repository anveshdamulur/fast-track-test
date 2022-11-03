<script setup>
import { ref, computed } from 'vue';
import jsonData from '../data/quiz.json';

const questions = ref(jsonData.results);

const quizCompleted = ref(false);
const currentQuestion = ref(0);

const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected != null && q.correct_answer == q.selected) {
      value++;
    }
  });

  return value;
});
const checkScore = (scr) => {
  let status = '';
  if (scr <= 5) {
    status = 'Bad Luck😔. You need to work hard for better results!!!';
  }
  if (scr > 5 && scr <= 8) {
    status = 'Keep working 📈 for best results !!!';
  }
  if (scr > 8) {
    status = 'WOWWWWW!!! Congrats 🎉';
  }
  return status;
};
const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});
const displayNumOfQuestions = () => {
  let displayNum = '';
  if (currentQuestion.value == questions.value.length) {
    return;
  }
  displayNum = ` Currently at question ${currentQuestion.value + 1} of
            ${questions.value.length}`;
  return displayNum;
};
const Setcorrect_answer = (evt) => {
  questions.value[currentQuestion.value].selected = evt.target.value;
  evt.target.value = null;
};

const nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
    return;
  }
  quizCompleted.value = true;
};
</script>

<template>
  <main class="app">
    <h1>Quiz app 😊</h1>
    <section class="quiz" v-if="!quizCompleted">
      <h3>{{ displayNumOfQuestions() }}</h3>

      <div class="quiz-info">
        <span class="question">
          {{ getCurrentQuestion.question }}
        </span>
        <span class="score">{{ score }} /{{ questions.length }}</span>
      </div>
      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.correct_answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`"
        >
          <input
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="Setcorrect_answer"
          />
          <span>{{ option }}</span>
        </label>
      </div>
      <button @click="nextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? 'FINISH'
            : getCurrentQuestion.selected == null
            ? 'Select an option'
            : 'Go to next'
        }}
      </button>
    </section>
    <section v-else>
      <h2>You have finished quiz</h2>
      <p>Your score is {{ score }} / {{ questions.length }}</p>
      <p>{{ checkScore(score) }}</p>
    </section>
  </main>
</template>

<style>
@import '../assets/css/appStyle.css';
</style>
