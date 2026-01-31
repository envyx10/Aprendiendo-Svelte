<script>
	// 1. Declaramos el estado reactivo con la Runa
	let count = $state(0);

	function increment() {
		// 2. Modificamos la variable directamente (sin setters)
		count += 1;
	}
</script>

<h1>Práctica 4: Reactividad Básica</h1>

<button onclick={increment}>
	Clicks: {count}
</button>

<p>
	Has hecho clic 
	<strong>
		{count} 
		{count === 1 ? 'vez' : 'veces'}
	</strong>
</p>

<style>
	button {
		font-size: 1.5rem;
		padding: 10px 20px;
		cursor: pointer;
		background-color: #3e8eff;
		color: white;
		border: none;
		border-radius: 8px;
	}
	button:active {
		transform: scale(0.95);
	}
</style>