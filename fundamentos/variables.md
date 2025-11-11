# 📦 Variables en JavaScript

## ¿Qué son las Variables?

Las variables son contenedores que almacenan datos. Puedes pensar en ellas como "cajas" con etiquetas donde guardas información que puedes usar y modificar más adelante.

---

## var

**📝 Descripción:** La forma antigua de declarar variables. Tiene scope de función y permite redeclaración.

**✍️ Sintaxis:**
```javascript
var nombreVariable = valor;
```

**💡 Ejemplos:**
```javascript
var nombre = 'Juan';
var edad = 25;
var esEstudiante = true;

console.log(nombre); // 'Juan'
console.log(edad);   // 25

// Se puede redeclarar (no recomendado)
var nombre = 'María';
console.log(nombre); // 'María'

// Scope de función
function ejemplo() {
  var x = 10;
  if (true) {
    var x = 20; // Misma variable!
    console.log(x); // 20
  }
  console.log(x); // 20 (no 10)
}
```

**⚠️ Notas importantes:**
- No usar en código moderno (usar `let` o `const`)
- Tiene hoisting (se eleva al inicio)
- No respeta el scope de bloque

---

## let

**📝 Descripción:** Forma moderna de declarar variables que pueden cambiar. Tiene scope de bloque.

**✍️ Sintaxis:**
```javascript
let nombreVariable = valor;
```

**💡 Ejemplos:**
```javascript
let nombre = 'Ana';
let edad = 30;
let ciudad;

// Reasignar valores
nombre = 'Pedro';
edad = 31;
ciudad = 'Madrid';

console.log(nombre); // 'Pedro'
console.log(edad);   // 31
console.log(ciudad); // 'Madrid'

// Scope de bloque
{
  let x = 10;
  console.log(x); // 10
}
// console.log(x); // Error: x no está definido

// En loops
for (let i = 0; i < 3; i++) {
  console.log(i); // 0, 1, 2
}
// console.log(i); // Error: i no está definido

// No se puede redeclarar
let numero = 5;
// let numero = 10; // Error!
numero = 10; // ✅ Correcto (reasignación)
```

**⚠️ Notas importantes:**
- Usa `let` cuando el valor va a cambiar
- Respeta el scope de bloque `{}`
- No se puede redeclarar en el mismo scope
- Ideal para contadores y valores temporales

---

## const

**📝 Descripción:** Declara constantes (valores que no cambian). También tiene scope de bloque.

**✍️ Sintaxis:**
```javascript
const NOMBRE_CONSTANTE = valor;
```

**💡 Ejemplos:**
```javascript
// Valores primitivos
const PI = 3.14159;
const MAX_USUARIOS = 100;
const API_URL = 'https://api.ejemplo.com';

console.log(PI); // 3.14159

// No se pueden reasignar
// PI = 3.14; // Error!

// Objetos con const
const persona = {
  nombre: 'Carlos',
  edad: 28
};

// Puedes modificar propiedades
persona.edad = 29; // ✅ Correcto
persona.ciudad = 'Barcelona'; // ✅ Correcto

console.log(persona); // { nombre: 'Carlos', edad: 29, ciudad: 'Barcelona' }

// Pero no reasignar el objeto completo
// persona = {}; // Error!

// Arrays con const
const colores = ['rojo', 'verde', 'azul'];

// Puedes modificar el array
colores.push('amarillo'); // ✅ Correcto
colores[0] = 'negro';     // ✅ Correcto

console.log(colores); // ['negro', 'verde', 'azul', 'amarillo']

// Pero no reasignar
// colores = []; // Error!

// Scope de bloque
{
  const TEMP = 100;
  console.log(TEMP); // 100
}
// console.log(TEMP); // Error
```

**⚠️ Notas importantes:**
- Usa `const` por defecto
- Solo usa `let` si realmente necesitas cambiar el valor
- `const` no hace objetos/arrays inmutables, solo la referencia
- Convención: MAYÚSCULAS para constantes verdaderas

---

## Comparación: var vs let vs const

```javascript
// var - Scope de función
function testVar() {
  var x = 1;
  if (true) {
    var x = 2;  // Misma variable!
    console.log(x); // 2
  }
  console.log(x); // 2
}

// let - Scope de bloque
function testLet() {
  let x = 1;
  if (true) {
    let x = 2;  // Variable diferente
    console.log(x); // 2
  }
  console.log(x); // 1
}

// const - Scope de bloque + no reasignable
function testConst() {
  const x = 1;
  // x = 2; // Error!
  console.log(x); // 1
}
```

## Tabla Comparativa

| Característica | var | let | const |
|---------------|-----|-----|-------|
| Scope | Función | Bloque | Bloque |
| Redeclaración | ✅ Sí | ❌ No | ❌ No |
| Reasignación | ✅ Sí | ✅ Sí | ❌ No |
| Hoisting | ✅ Sí | Sí* | Sí* |
| Uso moderno | ❌ No | ✅ Sí | ✅✅ Preferido |

*Aunque `let` y `const` tienen hoisting, no se pueden usar antes de su declaración (temporal dead zone)

---

## Buenas Prácticas

```javascript
// ✅ Bueno: const por defecto
const usuario = 'Admin';
const roles = ['user', 'admin'];
const configuracion = { tema: 'oscuro' };

// ✅ Bueno: let cuando necesites cambiar
let contador = 0;
contador++;

let mensaje = 'Cargando...';
mensaje = 'Completado!';

// ❌ Malo: var
var x = 10; // No usar

// ❌ Malo: let innecesario
let PI = 3.14159; // Debería ser const

// ✅ Bueno: nombres descriptivos
const precioTotal = 100;
const nombreUsuario = 'Ana';

// ❌ Malo: nombres confusos
const x = 100;
const data = 'Ana';
```

---

## Ejemplo Práctico: Calculadora Simple

```javascript
const resultado = document.getElementById('resultado');
const btnCalcular = document.getElementById('calcular');

btnCalcular.addEventListener('click', () => {
  // const para valores que no cambian
  const numero1 = parseFloat(document.getElementById('num1').value);
  const numero2 = parseFloat(document.getElementById('num2').value);
  const operacion = document.getElementById('operacion').value;
  
  // let para el resultado que calculamos
  let resultadoFinal;
  
  switch(operacion) {
    case 'suma':
      resultadoFinal = numero1 + numero2;
      break;
    case 'resta':
      resultadoFinal = numero1 - numero2;
      break;
    case 'multiplicacion':
      resultadoFinal = numero1 * numero2;
      break;
    case 'division':
      resultadoFinal = numero2 !== 0 ? numero1 / numero2 : 'Error';
      break;
  }
  
  resultado.textContent = `Resultado: ${resultadoFinal}`;
});
```

---

## Regla de Oro

1. **Usa `const` por defecto** - Para todo lo que no necesite cambiar
2. **Usa `let`** - Solo cuando necesites reasignar el valor
3. **Nunca uses `var`** - Es código antiguo y problemático

```javascript
// Perfecto ✅
const API_KEY = 'abc123';
const usuarios = [];
let contador = 0;
let mensaje = '';

// Evitar ❌
var x = 10;
var y = 20;
```
