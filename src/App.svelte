<script>
	import TextPathAnimation from './lib/TextPathAnimation.svelte';
</script>

<main>
	<TextPathAnimation />
</main>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		overflow: hidden;
		background: black;
	}

	:global(#app) {
		width: 100vw;
		height: 100vh;
		position: relative;
	}

	main {
		width: 100%;
		height: 100%;
		position: relative;
	}
</style>
