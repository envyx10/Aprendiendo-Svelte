
<script>
	let numbers = $state([1, 2, 3, 4]);
	let total = $derived(numbers.reduce((t, n) => t + n, 0));

	function addNumber() {
		numbers.push(numbers.length + 1);
        // Esto te dice en consola a modo advertencia que el objeto no puede ser clonado
        // Esto sucede porque numbers es un proxy reactivo. Hay un par de cosas que podemos hacer.
        // Primero, podemos crear una instantanea no reactiva del esctado con .snapshot 
		console.log($state.snapshot(numbers));
		console.log(`Esto es una prueba ${numbers}`);
        
	}

    // Puedes personalizar cómo se muestra la información usando: 
    // — por ejemplo, puedes usar console.trace para ver desde dónde se originó el cambio de estado.
    $inspect(numbers).with(console.trace);
    
    $inspect(numbers);

</script>


<p>{numbers.join(' + ')} = {total}</p>

<button onclick={addNumber}>
	Add a number
</button>
