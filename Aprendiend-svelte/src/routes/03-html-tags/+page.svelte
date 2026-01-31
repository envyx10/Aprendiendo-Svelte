<script>
	// 1. Definimos un fragmento de HTML en una variable
	let negrita = `<strong>HTML!!!</strong>`;
	
	// 2. Construimos el string completo usando JavaScript puro
	// Nota: Usamos comillas invertidas ` ` y ${} para interpolar en JS
	let mensaje = `Esta cadena contiene algo de ${negrita}`;
</script>

<h1>Práctica 3: Etiquetas HTML</h1>

<h3>Forma Segura (Por defecto)</h3>
<p>
	Svelte te protege y muestra el código literal: <br>
	{mensaje}
</p>

<h3>Forma "Raw" (Cruda) con @html</h3>

<p>
	Forzamos el renderizado del HTML: <br>
	{@html mensaje}
</p>

<style>
	h3 { margin-top: 20px; color: #555; }
	p { 
		border: 1px solid #ccc; 
		padding: 10px; 
		border-radius: 4px; 
		background: #f9f9f9;
	}
</style>