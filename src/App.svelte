<script>
import {Button} from 'sveltestrap';
import  QuestionCard  from "./Question.svelte";

let questions = []

const fetchQuestions = async( cat, diff) =>{
	const res = await fetch(`https://opentdb.com/api.php?amount=10&category=${cat}&difficulty=${diff}&type=multiple`);
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

fetchQuestions('11', 'easy').then((questions)=>{
	questionsArray = questions
})


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