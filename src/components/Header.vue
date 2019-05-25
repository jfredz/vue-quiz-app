<template>
	<div>
  <b-nav tabs>
    <b-nav-item disabled><b>Quiz App</b></b-nav-item>
    <b-nav-item disabled>Correct: {{ totalCorrect }} Submited: {{ totalAnswers }}</b-nav-item>
  </b-nav>
</div>
</template>

<script>
	export default {
		props: {
			totalCorrect: Number,
			totalAnswers: Number
		}
	}
</script>