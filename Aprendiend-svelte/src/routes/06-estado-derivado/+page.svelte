<script>
	let numbers = $state([1, 2, 3, 4]);
	
	// Estado Derivado: Svelte "escucha" a numbers.
	// Cada vez que numbers cambia, esta línea se ejecuta sola.
	let total = $derived(numbers.reduce((t, n) => t + n, 0));

	function addNumber() {
		// Al hacer push, disparamos la reactividad profunda...
		numbers.push(numbers.length + 1);
		// ...y automáticamente 'total' se actualizará.
	}
</script>

<h1>Práctica 6: Estado Derivado</h1>

<p>
	Números: {numbers.join(' + ')} 
	= 
	<strong class="total">{total}</strong>
</p>

<button onclick={addNumber}>
	Añadir un número
</button>

<style>
	.total {
		color: #ff3e00;
		font-size: 1.5em;
	}
	button {
		padding: 10px;
		cursor: pointer;
		font-size: 1rem;
	}
</style>