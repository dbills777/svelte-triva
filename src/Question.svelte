<script>
export let difficulty;
export let question;
export let correct_answer;
export let incorrect_answers;
$: all_answers = [...incorrect_answers, correct_answer]
function shuffle(array) {
  array.sort(() => Math.random() - 0.5);
}
$: console.log(all_answers)
$: shuffle(all_answers)
 import {
    Card,
    CardBody,
    CardFooter,
    CardHeader,
    CardText,
    CardTitle
  } from 'sveltestrap';
</script>

<div class="container">
    <Card class="mb-3">
  <CardHeader>
    <CardTitle >{question}</CardTitle>
  </CardHeader>
  <CardBody>
    {#each all_answers as answers }
    <CardText>
      {answers}
    </CardText>
      {/each}
  </CardBody>
  <CardFooter>Difficulty Level: {difficulty}</CardFooter>
</Card>
</div>



<style>

</style>