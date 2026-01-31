
# 06 - Estado Derivado (`$derived`)

A menudo, necesitas un valor que dependa directamente de otro. Por ejemplo, si tienes una lista de números, la "suma total" depende de esa lista.

---

## ❌ El Error Común (Lo que NO hay que hacer)

En otros frameworks, a veces caemos en la trampa de crear un estado para todo:

```javascript
let numbers = $state([1, 2]);
let total = $state(3); // ¡MAL! Esto se desincronizará fácil.
```

**Problema:** Si añades un número al array pero se te olvida actualizar `total`, tu app tendrá un bug.

---

## ✅ La Solución Svelte: `$derived(...)`

La runa `$derived` crea una variable que se recalcula automáticamente cada vez que sus dependencias cambian.

```javascript
let numbers = $state([1, 2, 3]);
// 'total' siempre será la suma correcta. Svelte se encarga.
let total = $derived(numbers.reduce((total_acumulado, numero_actual) => total_acumulado + numero_actual, 0));
```

---

## 🎯 Puntos Clave

- 📊 **Siempre sincronizado**: `$derived` recalcula el valor automáticamente
- 🚀 **Sin trabajo manual**: No necesitas actualizar el valor manualmente
- 🔗 **Reactividad automática**: Svelte detecta las dependencias por ti
- ⚡ **Optimizado**: Solo recalcula cuando cambian las dependencias