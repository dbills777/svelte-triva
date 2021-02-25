<script>
	export let name;
	import { Button } from 'sveltestrap';
	import  Nav  from "./fetchTest.svelte";
	const handleClick = () => alert('I warned you!');
</script>
<Nav></Nav>

<main>
	<h1>Hello {name}!</h1>
  	<Button color="success" on:click={handleClick}>Do Not Press</Button>

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>