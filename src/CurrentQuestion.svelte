
<script>
import { listen } from "svelte/internal";
const list =[]

  const fetchQuestions = (async () => {
    const response = await fetch('https://opentdb.com/api.php?amount=10&category=11&difficulty=easy&type=multiple')
	list.push(response)
    return await response.json()
  })()
  let index = 1
console.log(list)
</script>

{#await fetchQuestions}
  <p>...waiting</p>
{:then data}
  <p>{data.results[index].question}</p>
{/await}