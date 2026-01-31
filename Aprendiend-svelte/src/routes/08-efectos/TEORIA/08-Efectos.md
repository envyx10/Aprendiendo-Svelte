# 08 - Efectos Secundarios (`$effect`)

Hasta ahora hemos visto cómo reacciona el HTML a los cambios de estado. Pero a veces necesitamos que **otras cosas** (fuera del HTML) reaccionen. Aquí entra `$effect`.

### ¿Qué es un Efecto?
Es un bloque de código que Svelte ejecuta automáticamente **cada vez que cambia una variable reactiva** que esté dentro de él.

### ¿Cuándo usarlo? (La Regla de Oro)
* ❌ **NO** lo uses para calcular datos (usa `$derived`).
* ❌ **NO** lo uses para acciones directas del usuario como guardar al hacer clic (usa `onclick`).
* ✅ **SÍ** úsalo para sincronizar el estado con sistemas externos:
    * Temporizadores (`setInterval`).
    * Manipulación directa del DOM (Canvas, scroll).
    * Analíticas o suscripciones a APIs.

---

### Sintaxis Básica y Limpieza (Cleanup)

Cuando un efecto crea algo persistente (como un intervalo que se repite infinito), debemos asegurarnos de **destruirlo** antes de crear uno nuevo. Si no, acumularemos procesos "fantasmas" (memory leaks).

#### El Patrón Correcto:
```javascript
let interval = $state(1000);

$effect(() => {
    // 1. INICIO: Creamos el proceso y guardamos su ID (el "ticket")
    // Svelte ejecutará esto al montar y cada vez que cambie 'interval'.
    const id = setInterval(() => {
        console.log('Ejecutando...');
    }, interval);

    // 2. LIMPIEZA: Devolvemos una función para "apagar" lo anterior
    // Svelte ejecutará esto SIEMPRE justo antes de volver a correr el efecto
    // o cuando el componente se destruya.
    return () => {
        clearInterval(id); // "Despedimos" al proceso usando su ID
    };
});