<script>
	import CoinCard from "./CoinCard.svelte";
	export let name;
	let coins = [];
	let skip = 0;
	let limit = 20;
	async function fetchMoreCoins() {
		coins = [];
		const response = await fetch(
			`https://api.coinstats.app/public/v1/coins?skip=${skip}&limit=${limit}&currency=BRL`
		);
		const data = await response.json();
		coins = data.coins;
		skip += limit;
	}
	async function fetchLessCoins() {
		coins = [];
		skip -= limit;
		const response = await fetch(
			`https://api.coinstats.app/public/v1/coins?skip=${skip}&limit=${limit}&currency=BRL`
		);
		const data = await response.json();
		coins = data.coins;
	}
</script>

<main>
	<h1>{name}</h1>
	{#if coins.length === 0}
		<button on:click={fetchMoreCoins}>Carregar informações!</button>
	{:else}
		<div class="grid">
			{#each coins as coin}
			<CoinCard {coin} />
			{/each}
		</div>
		<button on:click={fetchLessCoins}>&#5130; Anterior</button>
		<button on:click={fetchMoreCoins}>Proxima &#5125;</button>
	{/if}
</main>

<footer>
	<p>Jeferson Ferreira &copy; {new Date().getFullYear()}</p>
</footer>

<style>
	main {
		text-align: center;
		padding: 40px 0;
		margin: 0 auto;
	}
	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}
	button {
		margin-top: 1rem;
		margin-left: 2em;
		margin-right: 2em;
		min-width: 5em;
	}
	.grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
		gap: 30px;
	}
	footer {
		margin-top: 2em;
		text-align: center;
	}
	@media (min-width: 640px) {
		main {
			max-width: 1600px;
			padding: 40px 20px;
		}
	}
</style>
