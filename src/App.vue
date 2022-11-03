<script setup>
import { ref, computed } from 'vue';
import jsonData from '../data/quiz.json';

const questions = ref(jsonData.results);

const quizCompleted = ref(false);
const currentQuestion = ref(0);

const score = computed(() => {
    let value = 0;
    questions.value.map((q) => {
        console.log(q);
        if (q.selected != null && q.correct_answer == q.selected) {
            console.log('correct');
            value++;
        }
    });

    return value;
});
const checkScore = (scr) => {
    let status = '';
    if (scr < 5) {
        status = 'Bad Luck😔. You need to work hard for better results!!!';
    }
    if (scr >= 5 && arg <= 8) {
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
    displayNum = ` Currently at question ${currentQuestion.value} of
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
            <button
                @click="nextQuestion"
                :disabled="!getCurrentQuestion.selected"
            >
                {{
                    getCurrentQuestion.index == questions.length - 1
                        ? 'FINISH'
                        : getCurrentQuestion.selected == null
                        ? 'Select an option'
                        : 'Next Question'
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
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Montserrat', sans-serif;
}

body {
    background: #ffdae0;
    color: #fff;
}

.app {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    height: 100vh;
    max-width: 120%;
}

h1 {
    font-size: 2rem;
    margin-bottom: 2rem;
    color: #030b07;
}
h3 {
    color: #030b07;
    margin-bottom: 10px;
}
.quiz {
    background-color: white;
    border-radius: 10px;
    padding: 2rem;
    width: 120%;
}

.quiz-info {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1rem;
}

.quiz-info .question {
    color: #030b07;
    font-size: 1.25rem;
}

.quiz-info.score {
    color: #fff;
    font-size: 1.25rem;
}

.options {
    margin-bottom: 1rem;
}

.option {
    padding: 1rem;
    display: block;
    background-color: #3385d6;
    margin-bottom: 0.5rem;
    border-radius: 0.5rem;
    cursor: pointer;
}

.option:hover {
    background-color: #2d213f;
}

.option.correct {
    background-color: rgba(3, 229, 116, 0.5);
    color: #030b07;
}

.option.wrong {
    background-color: #ff5a5f;
    color: #030b07;
}

.option:last-of-type {
    margin-bottom: 0;
}

.option.disabled {
    opacity: 0.5;
}

.option input {
    display: none;
}

button {
    appearance: none;
    outline: none;
    border: none;
    cursor: pointer;
    padding: 0.5rem 1rem;
    background-color: #2cce7d;
    color: #2d213f;
    font-weight: 700;
    text-transform: uppercase;
    font-size: 1.2rem;
    border-radius: 0.5rem;
}

button:disabled {
    opacity: 0.5;
}

h2 {
    font-size: 2rem;
    margin-bottom: 2rem;
    text-align: center;
    color: #030b07;
}

p {
    color: #8f8f8f;
    font-size: 1.5rem;
    text-align: center;
    padding: 1rem;
    color: #030b07;
}
</style>
