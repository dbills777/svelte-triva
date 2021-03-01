<script>
    import {
    Button,
    Card,
    CardBody,
    CardFooter,
    CardHeader,
    CardText,
    CardTitle
  } from 'sveltestrap';

export let difficulty;
export let question;
export let correct_answer;
export let incorrect_answers;
export let score;
export let arrayIndex;
export let ShowNext
let incorrect = 0
let myList = []

$: all_items = myList
$: score
$: all_answers = [...incorrect_answers, correct_answer]
$: shuffle(all_answers)

function shuffle(array) {
  array.sort(() => Math.random() - 0.5);
}

function checkAnswer(e){
  const currentInfo = [{correctAnswer: correct_answer, yourGuess: e.target.value, question: question}]
  if(arrayIndex < 11 ){
    // console.log("correct: ", correct_answer)
    // console.log("selected answer: ", e.target.value)
    if(e.target.value === correct_answer){
        ShowNext()
        myList = currentInfo
        return score +=1
    }
    else{
      ShowNext()
        myList = currentInfo
      return  incorrect +=1
    }
  }else{return}
}

</script>
<div class="container">
  <Card class="mb-3">
  <CardHeader class = "card-header">
    <CardTitle >  <h2> {question} </h2></CardTitle>
  </CardHeader>
  <CardBody>
    {#each all_answers as answers }
    <Button class="wide-button" bind:value ={answers} on:click={checkAnswer}>
      {answers}
    </Button>
    <br>
      {/each}
      </CardBody>
      <CardFooter class='footer-display'> <p>  Difficulty Level: <span> {difficulty}  </span> </p> <h3 class=Score> Correct: {score}</h3><h3 class=Score> Incorrect: {incorrect}</h3></CardFooter>
      </Card>
      {#each all_items as answers }
      <div class="card-header">
       Previous question:
       {answers.question}
       <br>
        You picked: {answers.yourGuess}
        <br>
        Answer: {answers.correctAnswer}
      </div>
      <br>
        {/each}
</div>

<style>
span{
  font-weight: bold;
  text-transform: uppercase;
}
</style>