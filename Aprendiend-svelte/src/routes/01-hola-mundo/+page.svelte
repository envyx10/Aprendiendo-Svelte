
<script>
	// 1. Variable simple
	let name = 'Svelte';

	// 2. Variable para atributos (La imagen)
	// Usamos una imagen de internet para que te funcione directo
	let src = 'https://svelte.dev/svelte-logo-horizontal.svg';
</script>

<h1>Hello {name}!</h1>

<h2>Gritando: {name.toUpperCase()}!!!</h2>

<img {src} alt="{name} logo" width="300" />

<style>
	h1 {
		color: #ff3e00;
	}
	h2 {
		color: #333;
		font-family: 'Courier New', Courier, monospace;
	}
</style>