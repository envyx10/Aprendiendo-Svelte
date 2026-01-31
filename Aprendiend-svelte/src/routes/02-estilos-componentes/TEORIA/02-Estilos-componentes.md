# Estilos y Componentes en Svelte

## 📦 Importación de Componentes

A diferencia de otros frameworks, **no hace falta registrar componentes globalmente**.

### Sintaxis de Importación

```svelte
<script>
  import Nombre from './Archivo.svelte';
</script>

<Nombre />
```

### ⚠️ Convención de Nombres

**Importante**: El nombre del componente **debe empezar por Mayúscula** (`Prueba`, `Nested`) para que Svelte sepa que no es una etiqueta HTML normal.

```svelte
<!-- ✅ CORRECTO -->
<Prueba />
<Nested />
<MiComponente />

<!-- ❌ INCORRECTO (Svelte lo tratará como HTML) -->
<prueba />
<nested />
<micomponente />
```

### Comparación con Otros Frameworks

| Framework | Registro Necesario | Sintaxis |
|-----------|-------------------|----------|
| **Svelte** | ❌ No | `import Component from './Component.svelte'` |
| **Vue 3** | ⚠️ Opcional | `components: { Component }` o Composition API |
| **React** | ❌ No | `import Component from './Component'` |
| **Vue 2** | ✅ Sí | `components: { Component }` obligatorio |

## 🎨 Scoped CSS (Estilos Encapsulados)

En Svelte, el CSS que escribes en un componente **se queda en ese componente**. Es automático y no requiere configuración adicional.

### Reglas de Encapsulamiento

```svelte
<!-- Padre.svelte -->
<style>
  p {
    color: gold;
  }
</style>

<p>Este párrafo es dorado</p>
<Hijo />
```

```svelte
<!-- Hijo.svelte -->
<style>
  p {
    color: red;
  }
</style>

<p>Este párrafo es rojo</p>
```

### ✅ Resultados

- Si pones `p { color: red }` en el **hijo**, NO afecta a los párrafos del **padre**
- Si pones `p { color: gold }` en el **padre**, NO afecta a los párrafos del **hijo**
- Cada componente mantiene sus estilos **completamente aislados**

## 🔧 ¿Cómo Funciona por Detrás?

Si inspeccionas el elemento en el navegador, verás que Svelte añade una **clase única** (un "hash") a los elementos.

### Proceso de Compilación

```svelte
<!-- Lo que escribes en Padre.svelte -->
<p>Texto del padre</p>

<!-- Lo que Svelte genera -->
<p class="svelte-abc123">Texto del padre</p>
```

```svelte
<!-- Lo que escribes en Hijo.svelte -->
<p>Texto del hijo</p>

<!-- Lo que Svelte genera -->
<p class="svelte-xyz789">Texto del hijo</p>
```

### CSS Transformado

```css
/* CSS del padre */
p { color: gold; }

/* Se convierte en: */
p.svelte-abc123 { color: gold; }
```

```css
/* CSS del hijo */
p { color: red; }

/* Se convierte en: */
p.svelte-xyz789 { color: red; }
```

Esto crea **muros impermeables** entre componentes automáticamente. 🛡️

## 💡 Ventajas del Scoped CSS

| Ventaja | Descripción |
|---------|-------------|
| **Sin colisiones** | Los estilos nunca se pisarán entre componentes |
| **Automático** | No necesitas BEM, CSS Modules o CSS-in-JS |
| **Predecible** | Los estilos solo afectan donde los defines |
| **Limpio** | No contamina el scope global |
| **Performante** | Se eliminan estilos no usados en producción |

## 🌍 Estilos Globales (Cuando los Necesitas)

Si necesitas estilos globales, puedes usar `:global()`:

```svelte
<style>
  /* Estilo local (solo este componente) */
  p {
    color: red;
  }

  /* Estilo global (afecta toda la aplicación) */
  :global(body) {
    margin: 0;
    font-family: Arial, sans-serif;
  }

  /* Selector mixto */
  :global(.external-class) {
    color: blue;
  }
</style>
```

## 📚 Recursos Adicionales

- [Documentación oficial: Scoped styles](https://svelte.dev/docs/svelte-components#style)
- [Tutorial: Component basics](https://learn.svelte.dev)