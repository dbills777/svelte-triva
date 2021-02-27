<script>
import {Button} from 'sveltestrap';
import  QuestionCard  from "./Question.svelte";


let questions = []

async function getAPIData(url) {
	try {
		const response = await fetch(url);
		const data = await response.json();
		return data;
	} catch (error) {}
}

const theData = getAPIData(`https://opentdb.com/api.php?amount=10&category=9&difficulty=hard&type=multiple`)
	.then((data) => {
		questions = data.results.map((apiQuestion) => {
		const question = {
			question: apiQuestion.question.replace(/&#039;/g, " ").replace(/&quot;/g, '"').replace(/&rsquo;/g, `'`).replace(/&amp;/g, '&'),
			difficulty: apiQuestion.difficulty,
			correct_answer: apiQuestion.correct_answer,
			incorrect_answers: apiQuestion.incorrect_answers,
			};
		questionsArray.push(question)
		return question;
		});
	});

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


let arrayIndex = 0
let score = 0
let remaining = 11
let answered = -1

$: console.log()
const ShowNext = () =>{
	arrayIndex +=1
	bannerUpdate()
}
const bannerUpdate = ()=>{
	remaining -=1
	answered +=1
}
// console.log(questions)
$: currentQuestion = questionsArray[arrayIndex]
console.log(questionsArray.length)
</script>

<main>
	<QuestionCard
	question ={currentQuestion.question}
	difficulty = {currentQuestion.difficulty}
	correct_answer={currentQuestion.correct_answer}
	incorrect_answers={currentQuestion.incorrect_answers}
	score = {score}
	ShowNext= {ShowNext}
	/>
	<Button class='next-btn' on:click={ShowNext} bind:value={arrayIndex}> Next quesion: {arrayIndex} </Button>

</main>

<style>
main{
	max-width: 900px;
	margin: 0 auto;

}

</style>