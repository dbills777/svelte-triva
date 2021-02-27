<script>
import {Button} from 'sveltestrap';
import  QuestionCard  from "./Question.svelte";
// import Options from './Options.svelte'

let questions = []
let questionsArray = [
  {
    category: 'General Knowledge',
    type: 'multiple',
    difficulty: 'easy',
    question: 'Are you Ready for Trivia? Hit Next Question to Begin',
    correct_answer: 'Go',
    incorrect_answers: ['1', '2', '3', 'GO'],
  },

];

const fetchQuestions = async( cat, diff) =>{
	const res = await fetch(`https://opentdb.com/api.php?amount=11&category=${cat}&difficulty=${diff}&type=multiple`);


	const data = await res.json()
	const questions = data.results
	let newQuestions = questions.map((apiQuestion) => {
		const question = {
			question: apiQuestion.question.replace(/&#039;/g, " ").replace(/&quot;/g, '"').replace(/&rsquo;/g, `'`).replace(/&amp;/g, '&'),
			difficulty: apiQuestion.difficulty,
			correct_answer: apiQuestion.correct_answer,
			incorrect_answers: apiQuestion.incorrect_answers,
		};
		return question
	});
	return newQuestions
}
let isReady = false
	function toggle(cat, diff) {
		isReady = !isReady;
		if(isReady){
		fetchQuestions(cat, diff).then((questions)=>{
			questionsArray = questions

		})
	}
	}



let value = ['9', '14']
let items = ["General Knowlege"]
let arrayIndex = 0
let score = 0
let remaining = 11
let answered = -1

const ShowNext = () =>{
	arrayIndex +=1
	bannerUpdate()
}
const bannerUpdate = ()=>{
	remaining -=1
	answered +=1
}
console.log(questions)
$: currentQuestion = questionsArray[arrayIndex]
console.log(questionsArray.length)

let categories = [
		{ id: "9", text: `General Knowledge` },
		{ id: "14", text: `Entertainment: TV` },
		{ id: "21", text: `Sports` },
		{ id: "22", text: `Geography` },
		{ id: "23", text: `History` },
		{ id: "15", text: `Entertainment: Video Games` },
	];
let difficulty = [
		{ id: "easy", text: `Easy` },
		{ id: "medium", text: `Medium` },
		{ id: "hard", text: `Hard` }
	];

	let category;
	let difficultyLevel;

	let answer = '';

	function handleSubmit() {
		// alert(`answered question ${category.id} (${difficultyLevel.text}) with`);
		toggle(category.id, difficultyLevel.id)
	}
</script>

<main>
	{#if isReady}
	<Button class='warning' on:click={toggle} > Quit Game </Button>
	<QuestionCard
	question ={currentQuestion.question}
	difficulty = {currentQuestion.difficulty}
	correct_answer={currentQuestion.correct_answer}
	incorrect_answers={currentQuestion.incorrect_answers}
	score = {score}
	ShowNext= {ShowNext}
	/>
	{#if arrayIndex <= 9 }
	<Button class='next-btn' on:click={ShowNext} bind:value={arrayIndex}> Next quesion: {arrayIndex} </Button>
	{/if}

	{#if arrayIndex >= 9 }
	<Button class='next-btn' on:click={fetchQuestions} bind:value={arrayIndex}> NewGame {arrayIndex} </Button>
	{/if}

	{/if}
	{#if !isReady }
	<h2>Choose A Category</h2>
	<form on:submit|preventDefault={handleSubmit}>
	<select bind:value={category} on:blur="{() => answer = ''}">
		{#each categories as option}
			<option value={option}>
				{option.text}
			</option>
		{/each}
	</select>
	<select bind:value={difficultyLevel} on:blur="{() => answer = ''}">
		{#each difficulty as option}
			<option value={option}>
				{option.text}
			</option>
		{/each}
	</select>
	<Button disabled={!category} type=submit>
		Start Game
	</Button>
</form>

	{/if}


</main>

<style>
main{
	max-width: 900px;
	margin: 0 auto;
}


</style>