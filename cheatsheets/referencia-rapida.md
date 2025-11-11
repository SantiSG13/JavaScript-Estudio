# ⚡ Referencia Rápida de JavaScript

## Variables y Tipos de Datos

```javascript
// Variables
const constante = 'no cambia';
let variable = 'puede cambiar';

// Tipos Primitivos
'texto'          // String
42               // Number
true / false     // Boolean
undefined        // Undefined
null             // Null
Symbol('id')     // Symbol
100n             // BigInt

// Tipos de Referencia
{ clave: 'valor' }  // Object
[1, 2, 3]          // Array
function() {}      // Function
```

## Operadores

```javascript
// Aritméticos
+ - * / % **

// Asignación
= += -= *= /= %=

// Comparación
== === != !== > >= < <=

// Lógicos
&& || !

// Ternario
condicion ? siTrue : siFalse

// Nullish
?? ?.
```

## Strings

```javascript
const str = 'Hola Mundo';

str.length                    // 10
str.toLowerCase()             // 'hola mundo'
str.toUpperCase()             // 'HOLA MUNDO'
str.includes('Hola')          // true
str.indexOf('Mundo')          // 5
str.slice(0, 4)              // 'Hola'
str.split(' ')               // ['Hola', 'Mundo']
str.replace('Hola', 'Hi')    // 'Hi Mundo'
str.trim()                   // Elimina espacios
str.padStart(15, '0')        // '00000Hola Mundo'

// Template literals
`Hola ${nombre}`
```

## Arrays

```javascript
const arr = [1, 2, 3, 4, 5];

// Modificar
arr.push(6)           // Agregar al final
arr.pop()             // Eliminar último
arr.unshift(0)        // Agregar al inicio
arr.shift()           // Eliminar primero
arr.splice(2, 1)      // Eliminar desde índice

// Buscar
arr.includes(3)       // true
arr.indexOf(3)        // 2
arr.find(n => n > 2)  // 3
arr.findIndex(n => n > 2) // 2

// Iterar
arr.forEach(n => console.log(n))
arr.map(n => n * 2)        // [2, 4, 6, 8, 10]
arr.filter(n => n > 2)     // [3, 4, 5]
arr.reduce((a, b) => a + b, 0) // 15
arr.some(n => n > 4)       // true
arr.every(n => n > 0)      // true

// Ordenar
arr.sort((a, b) => a - b)  // Ascendente
arr.reverse()              // Invertir

// Otros
arr.slice(1, 3)      // [2, 3] (copia)
arr.concat([6, 7])   // [1, 2, 3, 4, 5, 6, 7]
arr.join(', ')       // '1, 2, 3, 4, 5'
[...arr]             // Copia shallow
```

## Objetos

```javascript
const obj = { a: 1, b: 2, c: 3 };

// Acceder
obj.a              // 1
obj['b']           // 2

// Métodos
Object.keys(obj)          // ['a', 'b', 'c']
Object.values(obj)        // [1, 2, 3]
Object.entries(obj)       // [['a',1], ['b',2], ['c',3]]
Object.assign({}, obj)    // Copiar
{ ...obj }                // Copiar (spread)
Object.freeze(obj)        // Congelar
Object.seal(obj)          // Sellar
obj.hasOwnProperty('a')   // true

// Desestructuración
const { a, b } = obj;
const { a: x, b: y } = obj;  // Renombrar
```

## Funciones

```javascript
// Declaración
function sumar(a, b) {
  return a + b;
}

// Expresión
const restar = function(a, b) {
  return a - b;
};

// Arrow function
const multiplicar = (a, b) => a * b;

// Parámetros por defecto
function saludar(nombre = 'Usuario') {
  return `Hola ${nombre}`;
}

// Rest parameters
function sumarTodos(...nums) {
  return nums.reduce((a, b) => a + b, 0);
}

// Callback
arr.map(n => n * 2);

// Async/Await
async function obtenerDatos() {
  const res = await fetch(url);
  return res.json();
}
```

## Condicionales

```javascript
// if/else
if (condicion) {
  // código
} else if (otraCondicion) {
  // código
} else {
  // código
}

// Switch
switch(valor) {
  case 1:
    // código
    break;
  case 2:
    // código
    break;
  default:
    // código
}

// Ternario
const resultado = condicion ? 'si' : 'no';
```

## Loops

```javascript
// for
for (let i = 0; i < 10; i++) {
  console.log(i);
}

// for...of (valores)
for (const item of array) {
  console.log(item);
}

// for...in (claves)
for (const key in objeto) {
  console.log(key, objeto[key]);
}

// while
while (condicion) {
  // código
}

// do...while
do {
  // código
} while (condicion);
```

## Desestructuración

```javascript
// Arrays
const [a, b, c] = [1, 2, 3];
const [first, ...rest] = [1, 2, 3, 4];

// Objetos
const { nombre, edad } = usuario;
const { nombre: n, edad: e } = usuario;
const { ciudad = 'Madrid' } = usuario;

// Parámetros
function crear({ nombre, edad }) {
  return { nombre, edad };
}
```

## Spread & Rest

```javascript
// Spread en arrays
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];      // [1, 2, 3, 4, 5]
const copia = [...arr1];

// Spread en objetos
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 };    // {a: 1, b: 2, c: 3}

// Rest en funciones
function sumar(...nums) {
  return nums.reduce((a, b) => a + b);
}

// Rest en desestructuración
const [first, ...rest] = [1, 2, 3, 4];
const { a, ...resto } = { a: 1, b: 2, c: 3 };
```

## Clases

```javascript
class Persona {
  constructor(nombre, edad) {
    this.nombre = nombre;
    this.edad = edad;
  }
  
  saludar() {
    return `Hola, soy ${this.nombre}`;
  }
  
  static crear(datos) {
    return new Persona(datos.nombre, datos.edad);
  }
  
  get info() {
    return `${this.nombre} (${this.edad})`;
  }
  
  set edad(valor) {
    if (valor < 0) throw new Error('Edad inválida');
    this._edad = valor;
  }
}

// Herencia
class Empleado extends Persona {
  constructor(nombre, edad, puesto) {
    super(nombre, edad);
    this.puesto = puesto;
  }
}
```

## Promesas

```javascript
// Crear promesa
const promesa = new Promise((resolve, reject) => {
  if (exito) {
    resolve(datos);
  } else {
    reject(error);
  }
});

// Usar promesa
promesa
  .then(datos => console.log(datos))
  .catch(error => console.error(error))
  .finally(() => console.log('Fin'));

// Promise.all (paralelo)
Promise.all([promesa1, promesa2])
  .then(([res1, res2]) => console.log(res1, res2));

// Promise.race (primera en completar)
Promise.race([promesa1, promesa2])
  .then(res => console.log(res));
```

## Async/Await

```javascript
// Función async
async function obtenerDatos() {
  try {
    const res = await fetch(url);
    const datos = await res.json();
    return datos;
  } catch (error) {
    console.error(error);
  }
}

// Await en paralelo
const [datos1, datos2] = await Promise.all([
  obtenerDatos1(),
  obtenerDatos2()
]);
```

## Fetch API

```javascript
// GET
fetch('https://api.ejemplo.com/datos')
  .then(res => res.json())
  .then(datos => console.log(datos))
  .catch(error => console.error(error));

// POST
fetch('https://api.ejemplo.com/datos', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ nombre: 'Ana' })
})
  .then(res => res.json())
  .then(datos => console.log(datos));

// Con async/await
async function obtener() {
  const res = await fetch(url);
  return await res.json();
}
```

## DOM

```javascript
// Seleccionar
document.getElementById('id')
document.querySelector('.clase')
document.querySelectorAll('.clase')

// Crear
const div = document.createElement('div');
div.textContent = 'Hola';
div.innerHTML = '<p>Párrafo</p>';
div.classList.add('clase');

// Agregar
parent.appendChild(hijo);
parent.append(hijo1, hijo2);

// Eliminar
elemento.remove();
parent.removeChild(hijo);

// Atributos
elemento.getAttribute('href');
elemento.setAttribute('href', 'url');
elemento.removeAttribute('href');

// Estilos
elemento.style.color = 'red';
elemento.style.backgroundColor = 'blue';

// Clases
elemento.classList.add('clase');
elemento.classList.remove('clase');
elemento.classList.toggle('clase');
elemento.classList.contains('clase');
```

## Eventos

```javascript
// Agregar evento
elemento.addEventListener('click', (e) => {
  console.log('Click!', e.target);
});

// Eventos comunes
'click', 'dblclick'
'mouseenter', 'mouseleave'
'keydown', 'keyup', 'keypress'
'submit', 'change', 'input'
'focus', 'blur'
'load', 'DOMContentLoaded'

// Remover evento
function handler(e) { }
elemento.addEventListener('click', handler);
elemento.removeEventListener('click', handler);

// Prevenir default
e.preventDefault();

// Detener propagación
e.stopPropagation();
```

## JSON

```javascript
// A JSON string
const json = JSON.stringify(objeto);

// A objeto
const obj = JSON.parse(jsonString);

// Con formato
JSON.stringify(obj, null, 2);
```

## LocalStorage

```javascript
// Guardar
localStorage.setItem('clave', 'valor');
localStorage.setItem('usuario', JSON.stringify(obj));

// Obtener
const valor = localStorage.getItem('clave');
const obj = JSON.parse(localStorage.getItem('usuario'));

// Eliminar
localStorage.removeItem('clave');

// Limpiar todo
localStorage.clear();
```

## Temporizadores

```javascript
// setTimeout (una vez)
const id = setTimeout(() => {
  console.log('Después de 2 segundos');
}, 2000);

clearTimeout(id);

// setInterval (repetir)
const intervalo = setInterval(() => {
  console.log('Cada segundo');
}, 1000);

clearInterval(intervalo);
```

## Math

```javascript
Math.round(4.7)      // 5
Math.floor(4.7)      // 4
Math.ceil(4.3)       // 5
Math.abs(-5)         // 5
Math.max(1, 5, 3)    // 5
Math.min(1, 5, 3)    // 1
Math.pow(2, 3)       // 8
Math.sqrt(16)        // 4
Math.random()        // 0 a 1
Math.floor(Math.random() * 10)  // 0 a 9
```

## Date

```javascript
const ahora = new Date();

ahora.getFullYear()   // 2024
ahora.getMonth()      // 0-11
ahora.getDate()       // 1-31
ahora.getDay()        // 0-6 (0=domingo)
ahora.getHours()      // 0-23
ahora.getMinutes()    // 0-59
ahora.getSeconds()    // 0-59

// Crear fecha específica
new Date('2024-01-15')
new Date(2024, 0, 15)  // Mes es 0-indexed

// Timestamp
Date.now()            // Milisegundos desde 1970
ahora.getTime()       // Milisegundos
```

## Try/Catch

```javascript
try {
  // Código que puede fallar
  const resultado = funcionPeligrosa();
} catch (error) {
  // Manejar error
  console.error('Error:', error.message);
} finally {
  // Siempre se ejecuta
  console.log('Limpieza');
}

// Lanzar errores
throw new Error('Mensaje de error');
```

## Módulos (ES6)

```javascript
// Exportar
export const variable = 'valor';
export function funcion() {}
export default class MiClase {}

// Importar
import { variable, funcion } from './modulo.js';
import MiClase from './modulo.js';
import * as Todo from './modulo.js';

// Import dinámico
const modulo = await import('./modulo.js');
```

## Consejos de Performance

```javascript
// ✅ Bueno
const arr = [1, 2, 3];
for (let i = 0, len = arr.length; i < len; i++) {}

// ❌ Evitar acceder a .length en cada iteración
for (let i = 0; i < arr.length; i++) {}

// ✅ Bueno: Desestructuración
const { nombre, edad } = usuario;

// ❌ Evitar: Acceso repetitivo
console.log(usuario.nombre);
console.log(usuario.edad);

// ✅ Bueno: const por defecto
const config = { ... };

// ✅ Bueno: === en lugar de ==
if (valor === 5) {}
```

## Patrones Comunes

```javascript
// Debounce
function debounce(func, delay) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func(...args), delay);
  };
}

// Throttle
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func(...args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

// Memoization
function memoize(fn) {
  const cache = {};
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache[key]) return cache[key];
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}
```

## Atajos Útiles

```javascript
// Convertir a boolean
!!valor

// Convertir a número
+valor
Number(valor)

// Valores por defecto
const nombre = nombreUsuario || 'Invitado';
const edad = edadUsuario ?? 18;

// Remover duplicados
const unicos = [...new Set(array)];

// Clonar array
const copia = [...array];

// Clonar objeto (shallow)
const copia = { ...objeto };

// Ordenar números
arr.sort((a, b) => a - b);

// Generar ID simple
const id = Date.now() + Math.random().toString(36);
```
