# Conceptos Fundamentales de Svelte

## 🔤 Interpolación `{}`

Las **llaves** le dicen a Svelte "aquí va JavaScript, no texto plano".

```svelte
<script>
  let nombre = "Mundo";
</script>

<h1>Hola {nombre}!</h1>
```

## ⚡ Atributos Dinámicos

A diferencia de React (que usa `src={src}` siempre), Svelte permite **atajos sintácticos**.

Si la variable se llama igual que el atributo (`src`), puedes escribir simplemente:

```svelte
<img {src} />
```

En lugar de:

```svelte
<img src={src} />
```

Esto es **"azúcar sintáctico"** (syntax sugar) para escribir menos código.

### Ejemplos de atajos

```svelte
<script>
  let src = "imagen.jpg";
  let alt = "Descripción";
  let disabled = true;
</script>

<!-- Sintaxis completa -->
<img src={src} alt={alt} />

<!-- Sintaxis abreviada (shorthand) -->
<img {src} {alt} />

<!-- También funciona con atributos booleanos -->
<button {disabled}>Enviar</button>
```

## 🎨 Estilos Scoped (Con Alcance Local)

Los estilos en Svelte están **automáticamente encapsulados** dentro del componente.

```svelte
<style>
  h1 {
    color: #ff3e00;
  }
</style>

<h1>Este título será naranja</h1>
```

**Importante**: Si vuelves al menú principal, el `h1` de allí **NO será naranja**. El estilo **"muere"** dentro del componente donde fue definido.

### ¿Cómo funciona?

Svelte agrega clases únicas a cada elemento durante la compilación:

```html
<!-- Lo que escribes -->
<h1>Hola</h1>

<!-- Lo que genera Svelte -->
<h1 class="svelte-xyz123">Hola</h1>
```

Y el CSS se convierte en:

```css
h1.svelte-xyz123 {
  color: #ff3e00;
}
```

Esto garantiza que los estilos **nunca colisionen** entre componentes.