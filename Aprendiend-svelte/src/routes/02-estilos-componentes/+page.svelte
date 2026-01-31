
<!-- Componente Padre -->

<script lang="ts">
    // Importamos el archivo que está en la misma carpeta (./)
    // Podemos ponerle el nombre que queramos (ej: Prueba)
	import Prueba from './Nested.svelte';
</script>

<p>This is a paragraph (Soy el Padre).</p>

<Prueba />

<style>
    /* Estos estilos SOLO afectan a este archivo (+page.svelte) */
	p {
		color: goldenrod;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
</style>