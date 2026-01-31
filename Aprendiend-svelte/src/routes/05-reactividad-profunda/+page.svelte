<script>
	// 1. Array reactivo (envuelto en Proxy)
	let numbers = $state([1, 2, 3, 4]);

	function addNumber() {
		// 2. MUTACIÓN DIRECTA
		// En React esto no refrescaría la pantalla. En Svelte 5 sí.
		// Usamos .push() como en JS vanilla.
		numbers.push(numbers.length + 1);
	}

    function removeLast() {
        // También podemos usar .pop()
        numbers.pop();
    }
</script>

<h1>Práctica 5: Reactividad Profunda</h1>

<p>
    El array actual es: 
    <strong>[{numbers.join(', ')}]</strong>
</p>

<div class="buttons">
    <button onclick={addNumber} class="add">
        Añadir Número (.push)
    </button>

    <button onclick={removeLast} class="remove" disabled={numbers.length === 0}>
        Quitar Número (.pop)
    </button>
</div>

<style>
    .buttons {
        display: flex;
        gap: 10px;
        margin-top: 20px;
    }
	button {
		padding: 10px 15px;
        cursor: pointer;
        border: none;
        border-radius: 4px;
        color: white;
        font-weight: bold;
	}
    .add { background-color: #28a745; }
    .remove { background-color: #dc3545; }
    
    button:disabled {
        background-color: #ccc;
        cursor: not-allowed;
    }
</style>