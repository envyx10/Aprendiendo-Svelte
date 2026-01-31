# 05 - Reactividad Profunda (Arrays y Objetos)

Hasta ahora hemos visto reactividad con números simples (reasignación). Pero, ¿qué pasa con estructuras complejas como Arrays u Objetos?

### Reasignación vs. Mutación
* **Reasignación:** Cambias el valor entero (`count = count + 1`).
* **Mutación:** Cambias solo una parte de lo que hay dentro (`numbers.push(5)`).

En Svelte 5, el estado tiene **Reactividad Profunda (Deep Reactivity)**. Esto significa que Svelte detecta cambios incluso cuando "mutas" un objeto internamente.

### La Magia de los Proxies
Cuando declaras `let numbers = $state([])`, Svelte no crea un array normal. Envuelve tu array en un **Proxy**.

* **¿Qué es un Proxy?** Es un intermediario o "guardaespaldas" que vigila tu array.
* **¿Cómo funciona?** Cuando haces `numbers.push(...)`, el Proxy intercepta la orden, avisa a Svelte para que actualice el HTML, y luego ejecuta el push.

### Comparativa con React
* **En React:** Está prohibido mutar. Tienes que clonar el array: `setNumbers([...numbers, nuevo])`.
* **En Svelte 5:** Puedes usar métodos estándar de JavaScript:
    ```javascript
    // ¡Válido y reactivo!
    numbers.push(nuevoValor); 
    
    // ¡También válido! Cambiar una posición específica
    numbers[0] = 99;
    ```