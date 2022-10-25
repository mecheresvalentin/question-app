<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
	question: 'What is heavier, 1kg of stones or 1kg of paper?',
	answer: 2,
	options: [
		'1kg of stones',
		'1kg of paper',
		'None'
	],
	selected: null
  },
  {
	question: 'Abbreviation of the WWW is',
	answer: 2,
	options: [
		'I don/t know',
		'World War Wine',
		'World Wide Web'
	],
	selected: null
  },
  {
	question: 'What is Vue Router?',
	answer: 1,
	options: [
		'An ice cream maker',
		'A routing library for Vue',
		'Burger sauce'
	],
	selected: null
  }
  ,
  {
	question: 'Which of the tags defines bold text?',
	answer: 0,
	options: [
		'<b></b>',
		'<bold></bold>',
		'<big></big>'
	],
	selected: null
  }
  ,
  {
	question: 'Which attribute is required for the <img> tag?',
	answer: 1,
	options: [
		'class',
		'alt',
		'title'
	],
	selected: null
  }
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})

const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})

const SetAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}

const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}
	
	quizCompleted.value = true
}
function refreshPage() {
    window.location.reload(false);
}
</script>

<template>
	<main class="app">
		<h1>Questions</h1>
		
		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Your score {{ score }}/{{ questions.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" 
					:for="'option' + index" 
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'option' + index" 
						:name="getCurrentQuestion.index" 
						:value="index" 
						v-model="getCurrentQuestion.selected" 
						:disabled="getCurrentQuestion.selected"
						@change="SetAnswer" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			
			<button 
				@click="NextQuestion" 
				:disabled="!getCurrentQuestion.selected">
				{{ 
					getCurrentQuestion.index == questions.length - 1 
						? 'Finish' 
						: getCurrentQuestion.selected == null
							? 'Select an option'
							: 'Next question'
				}}
			</button>
		</section>

		<section class="finished" v-else>
			<h2>You have finished the quiz!</h2>
			<p>Your score is {{ score }}/{{ questions.length }}</p>
			<button @click="refreshPage" class="btnRefresh">Refresh</button>
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

.finished {
	display: flex;
	flex-direction: column;
	text-align: center;
	justify-content: center;
}

.btnRefresh {
	margin-top: 2rem;
}

body {
	background-color: #fff;
	color: #222222;
}

.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
}

h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
}

.quiz {
	background-color: #ffffff;
	padding: 1rem;
	width: 100%;
	max-width: 640px;
	border-radius: 15px;
	box-shadow: 0px 0px 12px 0px rgba(0,0,0,0.26);
	-webkit-box-shadow: 0px 0px 12px 0px rgba(0,0,0,0.26);
	-moz-box-shadow: 0px 0px 12px 0px rgba(0,0,0,0.26);
}

.quiz-info {
	display: flex;
	justify-content: space-between;
	margin-bottom: 1rem;
}

.quiz-info .question {
	color: #8F8F8F;
	font-size: 1.25rem;
}

.quiz-info.score {
	color: #FFF;
	font-size: 1.25rem;
}

.options {
	margin-bottom: 1rem;
}

.option {
	padding: 1rem;
	display: block;
	background-color: #e0e0e0;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
	transition: all .5s;
}

.option:hover {
	background-color: #a7a7a7;
}

.option.correct {
	background-color: #5d06cf;
}

.option.wrong {
	background-color: #e43e44;
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
	background-color: #5d06cf;
	color: #fff;
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
}

p {
	color: #8F8F8F;
	font-size: 1.5rem;
	text-align: center;
}
</style>
