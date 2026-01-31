# Etiquetas HTML en Svelte (`@html`)

## 🔒 Comportamiento Seguro por Defecto

Por defecto, Svelte es muy **seguro**. Si intentas imprimir una cadena de texto que contiene etiquetas HTML (como `<strong>` o `<h1>`), Svelte las **"escapará"**. Esto significa que las tratará como texto plano y verás los símbolos `<` y `>` escritos en la pantalla en lugar de ver el efecto visual (negrita, título, etc.).

## ❌ El Problema: HTML Escapado

```svelte
<script>
    let texto = "<strong>Soy negrita</strong>";
</script>

<p>{texto}</p>
```

### Resultado en el navegador:
```
<strong>Soy negrita</strong>
```

En lugar de ver el texto en **negrita**, verás literalmente los símbolos `<strong>` y `</strong>`.

## ✅ La Solución: La Etiqueta `{@html ...}`

Para decirle a Svelte *"confía en mí, esto es código HTML real"*, usamos esta **etiqueta especial** antes de la variable.

```svelte
<script>
    let texto = "<strong>Soy negrita</strong>";
</script>

<p>{@html texto}</p>
```

### Resultado en el navegador:
**Soy negrita** ← Se renderiza con el formato HTML

## ⚠️ Advertencia de Seguridad (CRÍTICO)

> **Importante**: Svelte **NO realiza ninguna limpieza** (sanitización) de la expresión dentro de `{@html ...}` antes de insertarla en el DOM.

### 🛡️ Regla de Oro

Usa `{@html}` **SOLO** para contenido que:
- ✅ Tú controlas
- ✅ Confías completamente
- ✅ Proviene de fuentes seguras (tus propios artículos, textos estáticos, decoraciones)

### 💀 El Peligro: Ataques XSS (Cross-Site Scripting)

**Nunca uses esto para contenido generado por usuarios desconocidos** (ej: comentarios de un blog, entradas de formularios).

#### Ejemplo de Ataque

```svelte
<script>
    // ⚠️ PELIGRO: Contenido de usuario sin sanitizar
    let comentarioUsuario = `<img src=x onerror=alert('Te he hackeado')>`;
</script>

<!-- ❌ NUNCA HAGAS ESTO -->
<div>{@html comentarioUsuario}</div>
```

Si un atacante inyecta un script malicioso como el anterior, el código se **ejecutará en el navegador de tus usuarios**, pudiendo:
- Robar cookies y tokens de sesión
- Redirigir a sitios maliciosos
- Modificar el contenido de la página
- Capturar información sensible

### 🔐 Alternativas Seguras

Si necesitas renderizar HTML de usuarios:

```javascript
// Opción 1: Usar una librería de sanitización
import DOMPurify from 'dompurify';

let htmlSeguro = DOMPurify.sanitize(contenidoUsuario);
```

```svelte
<!-- Opción 2: Usar texto plano (más seguro) -->
<p>{contenidoUsuario}</p>

<!-- Opción 3: Markdown seguro -->
<!-- Usa librerías como marked + DOMPurify -->
```

## 💡 JavaScript vs Svelte Syntax

Es común confundir cómo mezclar variables dentro de strings.

### 📍 Reglas por Contexto

| Contexto | Sintaxis | Ejemplo |
|----------|----------|---------|
| **`<script>` (JavaScript)** | Template Strings: `` `texto ${variable}` `` | `` let msg = `Hola ${nombre}` `` |
| **HTML (Svelte)** | Llaves simples: `{variable}` | `<p>{mensaje}</p>` |
| **HTML raw (Svelte)** | Directiva especial: `{@html variable}` | `<p>{@html contenido}</p>` |

### Ejemplo Completo y Correcto

```svelte
<script>
    // ✅ Zona JavaScript: Usamos backticks y ${variable}
    let strong = `<strong>HTML!!!</strong>`;
    
    // Construimos la frase completa con template strings
    let string = `this string contains some ${strong}`; 
    
    // Equivalente a: "this string contains some <strong>HTML!!!</strong>"
</script>

<!-- ✅ Zona HTML: Usamos {@html} para renderizar HTML -->
<p>{@html string}</p>
```

### Resultado:
```
this string contains some HTML!!!
                          ^^^^^^^^ (en negrita)
```

## 📝 Casos de Uso Válidos

### ✅ Contenido Estático y Seguro

```svelte
<script>
    // Tu propio contenido HTML
    let articulo = `
        <h2>Título del Artículo</h2>
        <p>Este es un <em>párrafo</em> de ejemplo.</p>
        <ul>
            <li>Punto 1</li>
            <li>Punto 2</li>
        </ul>
    `;
</script>

<article>{@html articulo}</article>
```

### ✅ Contenido de CMS o Backend Confiable

```svelte
<script>
    // HTML generado por tu backend controlado
    export let contenidoCMS = "";
</script>

<div class="contenido-cms">
    {@html contenidoCMS}
</div>
```

## 🎯 Resumen

| Aspecto | Detalle |
|---------|---------|
| **Sintaxis** | `{@html variable}` |
| **Propósito** | Renderizar HTML sin escapar |
| **Seguridad** | ⚠️ Sin sanitización automática |
| **Cuándo usar** | Solo con contenido confiable |
| **Cuándo NO usar** | Nunca con input de usuarios |
| **Alternativa segura** | DOMPurify + sanitización |

## 📚 Referencias

- [Documentación oficial: {@html}](https://svelte.dev/docs/special-tags#html)
- [OWASP: XSS Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [DOMPurify Library](https://github.com/cure53/DOMPurify)