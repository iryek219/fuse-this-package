<script>
	import Search from './components/Search.svelte';
	import ResultList from './components/ResultList.svelte';
	let items = [];
	let loading = false;

	async function onSearchChange(e) {
		if (e.detail.value) {
			loading = true;
			const result = await fetch(`http://localhost:3000/query/${e.detail.value}`, { mode: 'cors' });
			items = await result.json();
			items = items.sort((a, b) => a.our_score > b.our_score);
			loading = false;
		} else {
			items = [];
		}
	}
</script>

<main>
	<section>
		<h1><div>use</div><div>&nbsp;this&nbsp;</div><div>package</div></h1>
		<Search on:change={onSearchChange} />
		{#if loading}
		<span>Loading...</span>
		{:else}
		<ResultList items={items} />
		{/if}
	</section>
</main>

<style>
	section {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-bottom: 50px;
	}

	h1 {
		font-size: 60px;
		display: flex;
	}

	h1 div:not(:first-child):not(:last-child) {
		transform: rotate(-15deg);
		background-color: red;
		color: white;
		margin: 0 20px;
	}
</style>