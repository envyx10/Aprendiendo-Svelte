# Reactividad Básica (`$state`)

## ⚡ El Corazón de Svelte

En el corazón de Svelte hay un **sistema poderoso** para mantener el **DOM** (lo que ves en pantalla) **sincronizado** con el **estado de tu aplicación** (tus variables).

## 🆕 El Cambio en Svelte 5: Las Runas

### Antes vs Ahora

| Svelte 4 | Svelte 5 |
|----------|----------|
| Cualquier `let` era reactiva automáticamente | Reactividad **explícita** con runas |
| Implícito y "mágico" | Explícito y predecible |
| `let count = 0` → reactivo | `let count = $state(0)` → reactivo |

Antiguamente, cualquier variable `let` era reactiva. En **Svelte 5**, somos **explícitos**. **Tú eliges** qué variables deben vigilarse y cuáles no.

### Sintaxis de `$state`

Para hacer que una variable sea reactiva, usamos la **runa** `$state(...)`:

```svelte
<script>
    let count = $state(0);
</script>

<button onclick={() => count++}>
    Clicks: {count}
</button>
```

## 🔍 ¿Qué es Realmente `$state`?

Aunque **parece una función**, no lo es. Es una **instrucción para el compilador**.

### Proceso de Compilación

```svelte
<!-- Lo que escribes -->
<script>
    let count = $state(0);
</script>

<!-- Lo que Svelte genera internamente -->
<script>
    let count = __svelte_state(0);
    // + código invisible para vigilar cambios
</script>
```

Cuando Svelte construye tu app y ve el símbolo `$`, sabe que tiene que **inyectar código invisible** para "vigilar" esa variable.

### 🛡️ Regla de Oro

`$state` es una **palabra reservada** (runa oficial de Svelte 5). No puedes inventarte runas como `$pepito` o `$miVariable`.

#### Runas Oficiales de Svelte 5

| Runa | Propósito |
|------|-----------|
| `$state` | Variables reactivas |
| `$derived` | Valores calculados/derivados |
| `$effect` | Efectos secundarios |
| `$props` | Props de componentes |
| `$bindable` | Props bidireccionales |

## ✏️ Cómo Actualizar el Estado

A diferencia de React (donde necesitas `useState` y `setCount`), en Svelte **la asignación ES la actualización**.

### Comparación con React

```javascript
// ❌ React - Necesitas setState
const [count, setCount] = useState(0);
setCount(count + 1);
setCount(prev => prev + 1);

// ✅ Svelte - Asignación directa
let count = $state(0);
count += 1;
count++;
```

### Ejemplos de Actualización

```svelte
<script>
    let count = $state(0);
    
    function increment() {
        // Simplemente usamos matemáticas de JavaScript
        count += 1; 
    }
    
    function decrement() {
        count -= 1;
    }
    
    function reset() {
        count = 0;
    }
    
    function multiplyByTwo() {
        count *= 2;
    }
</script>

<button onclick={increment}>+1</button>
<button onclick={decrement}>-1</button>
<button onclick={reset}>Reset</button>
<button onclick={multiplyByTwo}>×2</button>

<p>Contador: {count}</p>
```

## 🎨 Expresiones en el HTML

Cualquier cosa que pongas entre llaves `{ ... }` en el HTML se considera **JavaScript vivo**. Svelte evalúa lo que hay dentro y lo imprime.

### Expresiones Simples

```svelte
<script>
    let count = $state(5);
    let name = $state("Ana");
</script>

<!-- Variables directas -->
<p>{count}</p>
<p>{name}</p>

<!-- Operaciones matemáticas -->
<p>{count * 2}</p>
<p>{count + 10}</p>

<!-- Concatenación de strings -->
<p>Hola {name}!</p>
```

### Lógica Condicional Directa

```svelte
<script>
    let count = $state(0);
</script>

<button onclick={() => count++}>
    clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

#### Explicación del Operador Ternario

```javascript
// Sintaxis: condición ? valorSiTrue : valorSiFalse
count === 1 ? 'time' : 'times'

// Si count es 1 → "time" (singular)
// Si count es cualquier otro número → "times" (plural)
```

### Expresiones Más Complejas

```svelte
<script>
    let price = $state(100);
    let quantity = $state(3);
    let discount = $state(0.1);
</script>

<!-- Cálculos inline -->
<p>Subtotal: ${price * quantity}</p>
<p>Descuento: ${(price * quantity * discount).toFixed(2)}</p>
<p>Total: ${(price * quantity * (1 - discount)).toFixed(2)}</p>

<!-- Métodos de string -->
<p>{name.toUpperCase()}</p>
<p>{name.length} caracteres</p>
```

## 🔄 Resumen del Flujo Reactivo

### Ciclo de Vida de la Reactividad

```
1. Inicialización
   ↓
   let count = $state(0)
   
2. Renderizado Inicial
   ↓
   DOM muestra: "clicked 0 times"
   
3. Interacción del Usuario
   ↓
   Usuario hace clic → onclick ejecuta count++
   
4. Actualización del Estado
   ↓
   count pasa de 0 a 1
   
5. Detección Automática
   ↓
   Svelte detecta el cambio
   
6. Actualización Selectiva del DOM
   ↓
   Solo actualiza "0" → "1" y "times" → "time"
   (No re-renderiza todo el componente)
```

### Pasos Detallados

| Paso | Descripción |
|------|-------------|
| **1. Inicialización** | Se declara `let count = $state(0)` |
| **2. Interacción** | El usuario hace clic (`onclick`) |
| **3. Lógica** | Se ejecuta `count += 1` |
| **4. Reactividad** | Svelte detecta el cambio en la variable |
| **5. DOM Update** | Actualiza **automáticamente** solo la parte del texto que cambió |

## 💡 Ventajas de `$state`

| Ventaja | Descripción |
|---------|-------------|
| **Simple** | Usa sintaxis JavaScript normal |
| **Performante** | Solo actualiza lo que cambió |
| **Explícito** | Sabes exactamente qué es reactivo |
| **Type-safe** | Compatible con TypeScript |
| **Sin boilerplate** | No necesitas getters/setters |

## 🎯 Buenas Prácticas

### ✅ Hacer

```svelte
<script>
    // Variables reactivas al inicio
    let count = $state(0);
    let name = $state("Usuario");
    
    // Funciones que modifican el estado
    function updateCount() {
        count += 1;
    }
</script>
```

### ❌ Evitar

```svelte
<script>
    // ❌ No uses $state dentro de funciones
    function crearContador() {
        let count = $state(0); // Error: las runas deben estar en el nivel superior
    }
    
    // ❌ No uses $state en condicionales
    if (condicion) {
        let count = $state(0); // Error
    }
</script>
```

## 📚 Recursos Adicionales

- [Documentación oficial: $state](https://svelte-5-preview.vercel.app/docs/runes#$state)
- [Svelte 5 Runes RFC](https://github.com/sveltejs/rfcs/blob/master/text/0000-runes.md)
- [Tutorial interactivo](https://learn.svelte.dev)

## 🔜 Próximos Pasos

Ahora que dominas `$state`, aprende sobre:
- `$derived` - Para valores calculados
- `$effect` - Para efectos secundarios
- Arrays y objetos reactivos con `$state`