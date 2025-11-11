# 🎲 Tipos de Datos en JavaScript

## Tipos Primitivos

JavaScript tiene 7 tipos de datos primitivos (inmutables):

---

## String (Cadenas de Texto)

**📝 Descripción:** Representa texto. Se puede usar comillas simples, dobles o backticks.

**✍️ Sintaxis:**
```javascript
'texto'
"texto"
`texto`
```

**💡 Ejemplos:**
```javascript
const nombre = 'Juan';
const apellido = "Pérez";
const saludo = `Hola Mundo`;

// Concatenación
const nombreCompleto = nombre + ' ' + apellido;
console.log(nombreCompleto); // 'Juan Pérez'

// Template literals (backticks)
const edad = 25;
const mensaje = `Me llamo ${nombre} y tengo ${edad} años`;
console.log(mensaje); // 'Me llamo Juan y tengo 25 años'

// Multilínea con template literals
const parrafo = `
  Esta es una línea
  Esta es otra línea
  Y otra más
`;

// Escape de caracteres
const texto = 'Ella dijo: "Hola"';
const texto2 = "Él dijo: 'Adiós'";
const texto3 = 'It\'s a beautiful day'; // Escape con \
```

**⚠️ Propiedades y métodos:**
```javascript
const texto = 'JavaScript';

// Longitud
console.log(texto.length); // 10

// Mayúsculas/Minúsculas
console.log(texto.toLowerCase()); // 'javascript'
console.log(texto.toUpperCase()); // 'JAVASCRIPT'

// Buscar
console.log(texto.includes('Script')); // true
console.log(texto.indexOf('a')); // 1

// Cortar
console.log(texto.slice(0, 4)); // 'Java'
console.log(texto.substring(4)); // 'Script'
```

---

## Number (Números)

**📝 Descripción:** Representa números enteros y decimales.

**💡 Ejemplos:**
```javascript
// Enteros
const edad = 30;
const temperatura = -5;

// Decimales
const precio = 19.99;
const pi = 3.14159;

// Operaciones matemáticas
const suma = 10 + 5;        // 15
const resta = 20 - 8;       // 12
const multiplicacion = 4 * 3; // 12
const division = 15 / 3;    // 5
const modulo = 17 % 5;      // 2 (resto)
const exponente = 2 ** 3;   // 8 (2³)

// Números especiales
const infinito = Infinity;
const negInfinito = -Infinity;
const noEsNumero = NaN; // Not a Number

// Conversiones
const texto = '42';
const numero = Number(texto);     // 42
const numero2 = parseInt('42px'); // 42
const decimal = parseFloat('3.14abc'); // 3.14

// Verificaciones
console.log(isNaN('hola')); // true
console.log(isFinite(100)); // true
console.log(isFinite(Infinity)); // false
```

**⚠️ Métodos útiles:**
```javascript
const numero = 3.14159;

// Redondeo
console.log(Math.round(numero));  // 3
console.log(Math.floor(numero));  // 3 (hacia abajo)
console.log(Math.ceil(numero));   // 4 (hacia arriba)
console.log(numero.toFixed(2));   // '3.14' (string con 2 decimales)

// Aleatorio
const aleatorio = Math.random(); // 0 a 1
const dado = Math.floor(Math.random() * 6) + 1; // 1 a 6

// Máximo y mínimo
console.log(Math.max(5, 10, 3, 8)); // 10
console.log(Math.min(5, 10, 3, 8)); // 3
```

---

## Boolean (Booleanos)

**📝 Descripción:** Representa valores lógicos: `true` (verdadero) o `false` (falso).

**💡 Ejemplos:**
```javascript
const esMayorDeEdad = true;
const tieneLicencia = false;

// Comparaciones
const edad = 18;
console.log(edad >= 18); // true
console.log(edad < 18);  // false

const nombre = 'Ana';
console.log(nombre === 'Ana');  // true
console.log(nombre !== 'Juan'); // true

// Operadores lógicos
const a = true;
const b = false;

console.log(a && b); // false (AND - ambos deben ser true)
console.log(a || b); // true  (OR - al menos uno debe ser true)
console.log(!a);     // false (NOT - invierte el valor)

// En condicionales
if (esMayorDeEdad && tieneLicencia) {
  console.log('Puede conducir');
} else {
  console.log('No puede conducir');
}

// Valores truthy y falsy
// Falsy: false, 0, '', null, undefined, NaN
// Truthy: todo lo demás

console.log(Boolean(0));    // false
console.log(Boolean(''));   // false
console.log(Boolean(null)); // false
console.log(Boolean(1));    // true
console.log(Boolean('hola')); // true
console.log(Boolean([]));   // true
```

---

## Undefined

**📝 Descripción:** Valor automático de variables declaradas pero no inicializadas.

**💡 Ejemplos:**
```javascript
let variable;
console.log(variable); // undefined

function saludar(nombre) {
  console.log(nombre); // undefined si no se pasa argumento
}
saludar(); // undefined

const persona = { nombre: 'Ana' };
console.log(persona.edad); // undefined (propiedad no existe)

// Verificar undefined
if (variable === undefined) {
  console.log('Variable no inicializada');
}

// O con typeof
if (typeof variable === 'undefined') {
  console.log('Variable no inicializada');
}
```

---

## Null

**📝 Descripción:** Representa la ausencia intencional de valor.

**💡 Ejemplos:**
```javascript
let usuario = null; // Intencionalmente sin valor

// Diferencia con undefined
let a;          // undefined (no inicializado)
let b = null;   // null (intencionalmente vacío)

// Limpiar valor
let contador = 10;
contador = null; // Resetear

// En objetos
const datos = {
  nombre: 'Juan',
  apellido: null, // Intencionalmente sin apellido
  edad: 30
};

// Verificar null
if (usuario === null) {
  console.log('No hay usuario');
}

// ⚠️ Peculiaridad de null
console.log(typeof null); // 'object' (bug histórico de JavaScript)
```

---

## Symbol

**📝 Descripción:** Tipo primitivo único e inmutable. Útil para propiedades privadas.

**💡 Ejemplos:**
```javascript
// Crear símbolos
const sym1 = Symbol();
const sym2 = Symbol('descripcion');
const sym3 = Symbol('descripcion');

// Cada símbolo es único
console.log(sym2 === sym3); // false

// Uso en objetos
const PROPIEDAD_PRIVADA = Symbol('privada');

const objeto = {
  nombre: 'Público',
  [PROPIEDAD_PRIVADA]: 'Secreto'
};

console.log(objeto.nombre); // 'Público'
console.log(objeto[PROPIEDAD_PRIVADA]); // 'Secreto'

// No aparecen en Object.keys()
console.log(Object.keys(objeto)); // ['nombre']
```

---

## BigInt

**📝 Descripción:** Para números enteros muy grandes (más allá de Number.MAX_SAFE_INTEGER).

**💡 Ejemplos:**
```javascript
// Crear BigInt
const grande1 = 1234567890123456789012345678901234567890n;
const grande2 = BigInt('9007199254740991');

console.log(grande1); // 1234567890123456789012345678901234567890n

// Operaciones
const suma = 100n + 50n;
console.log(suma); // 150n

// No se pueden mezclar con Number
// const error = 100n + 50; // Error!
const correcto = 100n + BigInt(50); // ✅ Correcto

// Comparaciones
console.log(10n === 10); // false (diferentes tipos)
console.log(10n == 10);  // true (coerción de tipo)
```

---

## Tipos de Referencia (Objetos)

### Object (Objetos)

**📝 Descripción:** Colección de pares clave-valor.

**💡 Ejemplos:**
```javascript
// Crear objeto
const persona = {
  nombre: 'Ana',
  edad: 28,
  ciudad: 'Madrid',
  saludar: function() {
    console.log(`Hola, soy ${this.nombre}`);
  }
};

// Acceder a propiedades
console.log(persona.nombre);    // 'Ana'
console.log(persona['edad']);   // 28

// Agregar/Modificar
persona.profesion = 'Ingeniera';
persona.edad = 29;

// Eliminar
delete persona.ciudad;

// Métodos
persona.saludar(); // 'Hola, soy Ana'

// Object literal moderno
const nombre = 'Juan';
const edad = 30;

const usuario = {
  nombre,  // Shorthand
  edad,
  saludar() { // Método abreviado
    console.log('Hola');
  }
};
```

### Array (Arreglos)

**📝 Descripción:** Lista ordenada de valores.

**💡 Ejemplos:**
```javascript
// Crear arrays
const numeros = [1, 2, 3, 4, 5];
const mixto = [1, 'texto', true, null, { nombre: 'Ana' }];
const vacio = [];

// Acceder a elementos
console.log(numeros[0]);    // 1
console.log(numeros[2]);    // 3
console.log(numeros.length); // 5

// Modificar
numeros[0] = 10;
numeros.push(6);      // Agregar al final
numeros.unshift(0);   // Agregar al inicio
numeros.pop();        // Eliminar último
numeros.shift();      // Eliminar primero

// Iterar
numeros.forEach(num => console.log(num));

// Métodos útiles
const dobles = numeros.map(num => num * 2);
const pares = numeros.filter(num => num % 2 === 0);
const suma = numeros.reduce((acc, num) => acc + num, 0);
```

### Function (Funciones)

**📝 Descripción:** Bloques de código reutilizables.

**💡 Ejemplos:**
```javascript
// Declaración de función
function sumar(a, b) {
  return a + b;
}

// Expresión de función
const restar = function(a, b) {
  return a - b;
};

// Arrow function
const multiplicar = (a, b) => a * b;

// Usar funciones
console.log(sumar(5, 3));      // 8
console.log(restar(10, 4));    // 6
console.log(multiplicar(3, 4)); // 12
```

---

## Verificar Tipos

```javascript
// typeof - para tipos primitivos
console.log(typeof 'hola');      // 'string'
console.log(typeof 42);          // 'number'
console.log(typeof true);        // 'boolean'
console.log(typeof undefined);   // 'undefined'
console.log(typeof Symbol());    // 'symbol'
console.log(typeof 10n);         // 'bigint'

// typeof con objetos (menos preciso)
console.log(typeof {});          // 'object'
console.log(typeof []);          // 'object' ⚠️
console.log(typeof null);        // 'object' ⚠️ (bug histórico)
console.log(typeof function(){}); // 'function'

// Array.isArray() - para arrays
console.log(Array.isArray([]));     // true
console.log(Array.isArray({}));     // false

// instanceof - para objetos
console.log([] instanceof Array);   // true
console.log({} instanceof Object);  // true
console.log(new Date() instanceof Date); // true
```

---

## Conversión de Tipos

```javascript
// String a Number
const texto = '42';
const num1 = Number(texto);      // 42
const num2 = parseInt(texto);    // 42
const num3 = parseFloat('3.14'); // 3.14
const num4 = +'42';              // 42 (operador unario +)

// Number a String
const numero = 42;
const str1 = String(numero);     // '42'
const str2 = numero.toString();  // '42'
const str3 = '' + numero;        // '42'

// A Boolean
console.log(Boolean(1));         // true
console.log(Boolean(0));         // false
console.log(Boolean(''));        // false
console.log(Boolean('hola'));    // true
console.log(!!'hola');           // true (doble negación)

// Coerción implícita (automática)
console.log('5' + 3);   // '53' (string)
console.log('5' - 3);   // 2 (number)
console.log('5' * '2'); // 10 (number)
console.log(true + 1);  // 2
console.log(false + 1); // 1
```

---

## Ejemplo Práctico: Validador de Formulario

```javascript
function validarFormulario(datos) {
  // Validar string
  if (typeof datos.nombre !== 'string' || datos.nombre.trim() === '') {
    return 'El nombre es requerido';
  }
  
  // Validar number
  if (typeof datos.edad !== 'number' || isNaN(datos.edad)) {
    return 'La edad debe ser un número';
  }
  
  if (datos.edad < 18) {
    return 'Debes ser mayor de 18 años';
  }
  
  // Validar boolean
  if (typeof datos.aceptaTerminos !== 'boolean' || !datos.aceptaTerminos) {
    return 'Debes aceptar los términos';
  }
  
  // Validar array
  if (!Array.isArray(datos.intereses) || datos.intereses.length === 0) {
    return 'Selecciona al menos un interés';
  }
  
  // Validar object
  if (typeof datos.direccion !== 'object' || datos.direccion === null) {
    return 'La dirección es requerida';
  }
  
  return null; // Sin errores
}

// Usar validador
const formulario = {
  nombre: 'Ana García',
  edad: 25,
  aceptaTerminos: true,
  intereses: ['música', 'deportes'],
  direccion: {
    calle: 'Principal',
    ciudad: 'Madrid'
  }
};

const error = validarFormulario(formulario);
if (error) {
  console.log('Error:', error);
} else {
  console.log('Formulario válido ✅');
}
```

---

## Resumen Visual

```javascript
// PRIMITIVOS (inmutables)
const texto = 'Hola';           // String
const numero = 42;              // Number
const esVerdad = true;          // Boolean
const indefinido = undefined;   // Undefined
const vacio = null;             // Null
const simbolo = Symbol('id');   // Symbol
const grande = 123n;            // BigInt

// OBJETOS (mutables)
const objeto = { nombre: 'Ana' };      // Object
const lista = [1, 2, 3];               // Array
const funcion = () => console.log();   // Function
const fecha = new Date();              // Date
const regex = /abc/;                   // RegExp
```
