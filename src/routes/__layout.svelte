<script>
	import '../style.scss';
</script>

<svelte:head>
	<title>Voltron - Virtuality</title>
</svelte:head>

<main>
	<slot />
</main>

<style lang="scss">
	main {
		font-size: 2em;

		:global(h1),
		:global(h2),
		:global(h3),
		:global(h4),
		:global(h5),
		:global(h6),
		:global(p) {
			margin-bottom: 1em;
		}
	}
</style>
