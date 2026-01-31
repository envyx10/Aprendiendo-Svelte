# 07 - Inspeccionar Estado

A menudo es útil poder rastrear el valor de un estado a medida que cambia con el tiempo.

---

## 🐛 El Problema

Dentro de la función `addNumber`, hemos añadido una instrucción `console.log`. Pero si haces clic en el botón y abres la consola del navegador (**F12**), verás una advertencia, y un mensaje diciendo que el objeto no pudo ser clonado.

**¿Por qué?** Esto sucede porque `numbers` es un **proxy reactivo**.

---

## 📸 Solución 1: `$state.snapshot(...)`

Podemos crear una instantánea (snapshot) no reactiva del estado:

```javascript
console.log($state.snapshot(numbers));
```

**¿Cuándo usarla?**
- Dentro de funciones cuando quieres ver un dato puntual
- Obtienes los datos puros sin el envoltorio del proxy
- **Resultado:** Ves `[1, 2, 3]` limpio, en lugar de `Proxy { <target>: ... }`

---

## 🔍 Solución 2: `$inspect(...)` (Recomendada)

Alternativamente, podemos usar la runa `$inspect` para registrar automáticamente una instantánea del estado cada vez que cambie:

```javascript
$inspect(numbers);
```

### ✨ Ventajas de `$inspect`

- 🤖 **Automático:** Registra cambios sin que escribas `console.log` manualmente
- 🗑️ **Se auto-elimina:** Este código será eliminado automáticamente de tu versión de producción
- 👁️ **Vigilancia constante:** Lo pones una vez en tu `<script>` (fuera de funciones) y se queda vigilando
- 📊 **Siempre actualizado:** Cada vez que `numbers` cambia, él solo hace el log por ti

### 🎨 Personalización Avanzada

Puedes personalizar cómo se muestra la información usando `$inspect(...).with(fn)`:

```javascript
$inspect(numbers).with(console.trace); // Ver desde dónde se originó el cambio
```

---

## 🧠 Resumen: ¿Cuál usar?

| Herramienta | Cuándo usarla | Ventaja principal |
|-------------|---------------|-------------------|
| 📸 `$state.snapshot()` | Logs puntuales en funciones | Control manual, datos puros |
| 🔍 `$inspect()` | Debug durante desarrollo | Automático, se elimina en producción |

---

## 🕵️‍♂️ ¿Por qué existe el Proxy?

Cuando haces `console.log(numbers)`, estás intentando imprimir el **sistema de seguridad de Svelte**, no solo los datos. Los navegadores a veces se confunden con esto. El proxy es lo que permite a Svelte detectar cambios y actualizar la UI automáticamente.