<script>
import {Button} from 'sveltestrap';
import  QuestionCard  from "./Question.svelte";
import { difficulty, categories } from "./data";

let questions = [];
let questionsArray = [];

const fetchQuestions = async (cat = '9', diff='easy') => {
  const res = await fetch(`https://opentdb.com/api.php?amount=11&category=${cat}&difficulty=${diff}&type=multiple`);
  const data = await res.json();
  const questions = data.results;
  let newQuestions = questions.map((apiQuestion) => {
    const question = {
      question: apiQuestion.question
        .replace(/&#039;/g, ' ')
        .replace(/&quot;/g, '"')
        .replace(/&rsquo;/g, `'`)
        .replace(/&amp;/g, '&'),
      difficulty: apiQuestion.difficulty,
      correct_answer: apiQuestion.correct_answer,
      incorrect_answers: apiQuestion.incorrect_answers,
    };
    return question;
  });
  return newQuestions;
};
let isReady = false;
function toggle(cat, diff) {
  isReady = !isReady;
  arrayIndex = 0;
  if (isReady) {
    fetchQuestions(cat, diff).then((questions) => {
      questionsArray = questions;
    });
  }
}

let arrayIndex = 0;
let score = 0;

const ShowNext = () => {
  if (arrayIndex < 10) {
    arrayIndex += 1;
  } else {
    return;
  }
};

$: currentQuestion = questionsArray[arrayIndex];

// select box info
let category;
let difficultyLevel;

// selectbox submit
function handleSubmit() {
  toggle(category.id, difficultyLevel.id);
}

</script>
{#await currentQuestion}
	<p>...loading</p>
{:then currentQuestion}

<main>
	{#if isReady}
	<Button class='warning' on:click={toggle} > Quit Game </Button>
	<QuestionCard
	arrayIndex ={arrayIndex}
	question ={currentQuestion.question}
	difficulty = {currentQuestion.difficulty}
	correct_answer={currentQuestion.correct_answer}
	incorrect_answers={currentQuestion.incorrect_answers}
	score = {score}
	ShowNext= {ShowNext}
	/>
	{#if arrayIndex < 11 }
	<Button class='next-btn'  bind:value={arrayIndex }>  Questions Answered: {arrayIndex}/10 </Button>
	{/if}

	{#if arrayIndex > 9 }
	<Button class='next-btn' on:click={toggle} bind:value={arrayIndex}> NewGame {arrayIndex} </Button>
	{/if}

	{/if}
	{#if !isReady }
	<h2 class="title">Trivia</h2>
	<form on:submit|preventDefault={handleSubmit}>
	<h3 class="directions">Pick a Category and a level to start playing Trivia</h3>
	<select bind:value={category}>
		{#each categories as option}
			<option value={option}>
				{option.name}
			</option>
		{/each}
	</select>
	<select bind:value={difficultyLevel}>
		{#each difficulty as option}
			<option value={option}>
				{option.id}
			</option>
		{/each}
	</select>
	<Button disabled={!category} type=submit>
		Start Game
	</Button>
</form>
	{/if}
</main>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}


<style>
main{
	max-width: 900px;
	margin: 0 auto;
}
</style>