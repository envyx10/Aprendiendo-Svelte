

<script>
	
	let elapsed = $state(0);
	let interval = $state(1000);

    
	$effect(() => {
        
        // Tenemos que asignar la const id como contrato para poder despedirle.
		const id = setInterval(() => { // SetInterval tiene - ¿Qué tiene que hacer la funcion?
			elapsed += 1;
		}, interval); // Cada cuanto tiene que hacerlo ( interval vale 1000 , cada 1 segundo )
        
         return () => clearInterval(id); // Esto seria la manera de despedir el contrato inicial que teniamos con interval que seria 1000 en caso de querer cambiarlo a 500
	});

    

</script>

<button onclick={() => interval /= 2}>speed up</button>
<button onclick={() => interval *= 2}>slow down</button>

<p>elapsed: {elapsed}</p>
