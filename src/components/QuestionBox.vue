<template>
<div>
	<b-jumbotron>
  <template slot="lead">
  	<div>
    Question <b-badge variant="light">{{ index + 1}}</b-badge>
  	</div>
  	<div v-html="question.question"></div>
  </template>
  <hr class="my-4">
  <b-list-group>
    <b-list-group-item 
    :class="[
    	!answered && selectedIndex === index ? 'selected' : 
    	answered && correctIndex === index && selectedIndex === index ? 'correct' :
    	answered && correctIndex !== index && selectedIndex === index ? 'wrong' : ''
    ]"
    v-for="(answer, index) in shuffledAnswers" 
    :key="index"
    @click="selectAnswer(index)"
    >
    <div v-html="answer"></div>
    </b-list-group-item>
  </b-list-group>
  <b-button variant="primary" 
  	@click="submitAnswer"
  	:disabled="selectedIndex === null || answered"
  	>
  	Submit
  </b-button>
  <b-button variant="success"
  	@click="next" 
  	:disabled="index === 9"
  	>
  	Next
  </b-button>
	</b-jumbotron>
</div>
</template>

<script>
	import _ from 'lodash'

	export default {
		props: {
			question: Object,
			next: Function,
			increment: Function,
			index: Number

		},
		data() {
			return {
				selectedIndex: null,
				correctIndex: null,
				shuffledAnswers: [],
				answered: false,
			}
		},
		computed: {
			answers() {
				let answers = [...this.question.incorrect_answers];
				answers.push(this.question.correct_answer);
				return answers;
			}
		},
		mounted() {
			this.shuffleAnswers();
		},
		watch: {
			question() {
				this.selectedIndex = null;
				this.answered = false;
				this.shuffleAnswers();
			}
		}, 
		methods: {
			selectAnswer(index) {
				this.selectedIndex = index;

			},
			shuffleAnswers() {
				let answers = [...this.question.incorrect_answers, this.question.correct_answer];
				this.shuffledAnswers = _.shuffle(answers);
				this.correctIndex = this.shuffledAnswers.indexOf(this.question.correct_answer); 
			},
			submitAnswer() {
				let isCorrect = false;
				if(this.selectedIndex === this.correctIndex){
					isCorrect = true;
				}
				this.increment(isCorrect);
				this.answered = true;
			}
		}
	}
</script>

<style scoped>
	.list-group {
		margin-bottom: 15px;
	}
	.list-group-item:hover {
		background-color: #eee;
		cursor: pointer;
	}
	.btn {
		margin: 0 5px;
	}
	.selected {
		background-color: lightblue;
	}
	.correct {
		background-color: lightgreen;
	}
	.wrong {
		background-color: #ff7f92;
	}
</style>