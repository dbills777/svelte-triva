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

$: score
$: all_answers = [...incorrect_answers, correct_answer]
$: shuffle(all_answers)
// functions
function shuffle(array) {
  array.sort(() => Math.random() - 0.5);
}

function checkAnswer(e){
    console.log("correct: ", correct_answer)
    console.log("selected answer: ", e.target.value)
    if(e.target.value === correct_answer){
        return score +=1
    }
    else{
      return  score +=0
    }
}

</script>

<div class="container">
    <Card class="mb-3">
  <CardHeader>
    <CardTitle > <h2> {question} </h2></CardTitle>
  </CardHeader>
  <CardBody>
    {#each all_answers as answers }
    <Button class="wide-button" bind:value ={answers} on:click={checkAnswer}>
      {answers}
    </Button>
    <br>
      {/each}

  </CardBody>
  <CardFooter class='footer-display'> <p>  Difficulty Level:<span> {difficulty}  </span> </p> <h3 class=Score> Score: {score}</h3></CardFooter>
</Card>
</div>



<style>

</style>