# ➗ Operadores en JavaScript

## Operadores Aritméticos

**📝 Descripción:** Realizan operaciones matemáticas básicas.

**💡 Ejemplos:**

### Suma (+)
```javascript
const suma = 10 + 5;
console.log(suma); // 15

const a = 20;
const b = 15;
console.log(a + b); // 35

// Concatenación con strings
console.log('Hola' + ' ' + 'Mundo'); // 'Hola Mundo'
console.log('5' + 3); // '53' (string)
```

### Resta (-)
```javascript
const resta = 20 - 8;
console.log(resta); // 12

const temperatura = 25;
const disminucion = temperatura - 5;
console.log(disminucion); // 20

console.log('10' - 5); // 5 (convierte a número)
```

### Multiplicación (*)
```javascript
const multiplicacion = 4 * 5;
console.log(multiplicacion); // 20

const precio = 10;
const cantidad = 3;
const total = precio * cantidad;
console.log(total); // 30
```

### División (/)
```javascript
const division = 20 / 4;
console.log(division); // 5

const mitad = 100 / 2;
console.log(mitad); // 50

// División por cero
console.log(10 / 0); // Infinity
```

### Módulo/Resto (%)
```javascript
const resto = 17 % 5;
console.log(resto); // 2

// Verificar si es par
const numero = 8;
if (numero % 2 === 0) {
  console.log('Es par'); // ✅
}

// Verificar si es impar
const numero2 = 7;
if (numero2 % 2 !== 0) {
  console.log('Es impar'); // ✅
}

// Ciclos alternados
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) {
    console.log(`${i} es par`);
  }
}
```

### Exponenciación (**)
```javascript
const cuadrado = 5 ** 2;
console.log(cuadrado); // 25

const cubo = 2 ** 3;
console.log(cubo); // 8

// Raíz cuadrada (exponente 0.5)
const raiz = 16 ** 0.5;
console.log(raiz); // 4

// Equivalente a Math.pow()
console.log(Math.pow(2, 3)); // 8
```

---

## Operadores de Asignación

**📝 Descripción:** Asignan valores a variables.

**💡 Ejemplos:**

```javascript
// Asignación simple (=)
let x = 10;

// Suma y asignación (+=)
x += 5;  // x = x + 5
console.log(x); // 15

// Resta y asignación (-=)
x -= 3;  // x = x - 3
console.log(x); // 12

// Multiplicación y asignación (*=)
x *= 2;  // x = x * 2
console.log(x); // 24

// División y asignación (/=)
x /= 4;  // x = x / 4
console.log(x); // 6

// Módulo y asignación (%=)
x %= 4;  // x = x % 4
console.log(x); // 2

// Exponenciación y asignación (**=)
x **= 3;  // x = x ** 3
console.log(x); // 8

// Ejemplo práctico: contador
let contador = 0;
contador += 1;  // Incrementar
console.log(contador); // 1

// Acumular valores
let total = 0;
total += 10;
total += 20;
total += 30;
console.log(total); // 60
```

---

## Operadores de Comparación

**📝 Descripción:** Comparan valores y retornan `true` o `false`.

**💡 Ejemplos:**

### Igualdad (==) vs Igualdad Estricta (===)
```javascript
// == (igualdad con coerción de tipo)
console.log(5 == '5');   // true ⚠️
console.log(0 == false); // true ⚠️
console.log(null == undefined); // true ⚠️

// === (igualdad estricta - tipo y valor)
console.log(5 === '5');   // false ✅
console.log(5 === 5);     // true
console.log(0 === false); // false
console.log(null === undefined); // false

// Siempre usa === (buena práctica)
const edad = 18;
if (edad === 18) {
  console.log('Tiene 18 años exactos');
}
```

### Desigualdad (!=) vs Desigualdad Estricta (!==)
```javascript
// != (desigualdad con coerción)
console.log(5 != '5');   // false
console.log(5 != 10);    // true

// !== (desigualdad estricta)
console.log(5 !== '5');  // true ✅
console.log(5 !== 5);    // false

const nombre = 'Ana';
if (nombre !== 'Juan') {
  console.log('No es Juan'); // ✅
}
```

### Mayor que (>) y Mayor o igual (>=)
```javascript
console.log(10 > 5);   // true
console.log(5 > 5);    // false
console.log(5 >= 5);   // true
console.log(3 >= 5);   // false

// Validar edad
const edad = 20;
if (edad >= 18) {
  console.log('Es mayor de edad'); // ✅
}

// Comparar strings (alfabético)
console.log('b' > 'a');  // true
console.log('Z' > 'a');  // false (mayúsculas < minúsculas)
```

### Menor que (<) y Menor o igual (<=)
```javascript
console.log(3 < 5);    // true
console.log(5 < 5);    // false
console.log(5 <= 5);   // true
console.log(7 <= 5);   // false

// Validar rango
const temperatura = 15;
if (temperatura < 20) {
  console.log('Hace frío'); // ✅
}

// Límite de caracteres
const mensaje = 'Hola';
if (mensaje.length <= 100) {
  console.log('Mensaje válido');
}
```

---

## Operadores Lógicos

**📝 Descripción:** Combinan expresiones booleanas.

**💡 Ejemplos:**

### AND Lógico (&&)
```javascript
// Ambas condiciones deben ser true
const edad = 25;
const tieneLicencia = true;

if (edad >= 18 && tieneLicencia) {
  console.log('Puede conducir'); // ✅
}

// Cortocircuito: si la primera es false, no evalúa la segunda
const resultado = false && console.log('No se ejecuta');

// Múltiples condiciones
const usuario = 'admin';
const password = '1234';
const recordar = true;

if (usuario === 'admin' && password === '1234' && recordar) {
  console.log('Login exitoso'); // ✅
}

// Asignación condicional
const nombre = 'Ana';
const saludo = nombre && `Hola ${nombre}`; // 'Hola Ana'

const nombreVacio = '';
const saludo2 = nombreVacio && `Hola ${nombreVacio}`; // ''
```

### OR Lógico (||)
```javascript
// Al menos una condición debe ser true
const esFinDeSemana = true;
const esVacaciones = false;

if (esFinDeSemana || esVacaciones) {
  console.log('Puedes descansar'); // ✅
}

// Cortocircuito: si la primera es true, no evalúa la segunda
const resultado = true || console.log('No se ejecuta');

// Valores por defecto (antes de ??)
const nombreUsuario = '';
const nombre = nombreUsuario || 'Invitado';
console.log(nombre); // 'Invitado'

const edad = 0;
const edadMostrar = edad || 18;
console.log(edadMostrar); // 18 (⚠️ 0 es falsy)

// Múltiples opciones
const idioma = localStorage.getItem('idioma') || navigator.language || 'es';
```

### NOT Lógico (!)
```javascript
// Invierte el valor booleano
const activo = true;
console.log(!activo); // false

const desactivado = false;
console.log(!desactivado); // true

// Doble negación para convertir a boolean
console.log(!!1);      // true
console.log(!!'hola'); // true
console.log(!!'');     // false
console.log(!!0);      // false

// En condicionales
const usuario = null;
if (!usuario) {
  console.log('No hay usuario'); // ✅
}

const lista = [];
if (!lista.length) {
  console.log('Lista vacía'); // ✅
}
```

### Tabla de Verdad

```javascript
// AND (&&)
console.log(true  && true);  // true
console.log(true  && false); // false
console.log(false && true);  // false
console.log(false && false); // false

// OR (||)
console.log(true  || true);  // true
console.log(true  || false); // true
console.log(false || true);  // true
console.log(false || false); // false

// NOT (!)
console.log(!true);  // false
console.log(!false); // true
```

---

## Operadores de Incremento/Decremento

**📝 Descripción:** Aumentan o disminuyen un valor en 1.

**💡 Ejemplos:**

```javascript
// Incremento (++)
let contador = 0;

// Post-incremento (usa el valor, luego incrementa)
console.log(contador++); // 0
console.log(contador);   // 1

// Pre-incremento (incrementa, luego usa el valor)
console.log(++contador); // 2
console.log(contador);   // 2

// Decremento (--)
let vidas = 3;

// Post-decremento
console.log(vidas--); // 3
console.log(vidas);   // 2

// Pre-decremento
console.log(--vidas); // 1
console.log(vidas);   // 1

// En loops
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

// Contador de clicks
let clicks = 0;
button.addEventListener('click', () => {
  clicks++;
  console.log(`Clicks: ${clicks}`);
});
```

---

## Operador Ternario (?)

**📝 Descripción:** Operador condicional compacto (if-else en una línea).

**✍️ Sintaxis:**
```javascript
condicion ? valorSiTrue : valorSiFalse
```

**💡 Ejemplos:**

```javascript
// Básico
const edad = 20;
const mensaje = edad >= 18 ? 'Mayor de edad' : 'Menor de edad';
console.log(mensaje); // 'Mayor de edad'

// Asignar valores
const hora = 14;
const saludo = hora < 12 ? 'Buenos días' : 'Buenas tardes';
console.log(saludo); // 'Buenas tardes'

// En return
function obtenerDescuento(esMiembro) {
  return esMiembro ? 0.20 : 0;
}
console.log(obtenerDescuento(true)); // 0.20

// Ternario anidado (menos legible)
const nota = 85;
const calificacion = nota >= 90 ? 'A' :
                     nota >= 80 ? 'B' :
                     nota >= 70 ? 'C' :
                     nota >= 60 ? 'D' : 'F';
console.log(calificacion); // 'B'

// Ejecutar funciones
const esAdmin = true;
esAdmin ? cargarPanelAdmin() : cargarPanelUsuario();

// En JSX (React)
const MiComponente = ({ mostrar }) => (
  <div>
    {mostrar ? <p>Visible</p> : <p>Oculto</p>}
  </div>
);
```

---

## Operador Nullish Coalescing (??)

**📝 Descripción:** Retorna el valor derecho si el izquierdo es `null` o `undefined`.

**💡 Ejemplos:**

```javascript
// Solo null o undefined son considerados
const valor1 = null ?? 'Por defecto';
console.log(valor1); // 'Por defecto'

const valor2 = undefined ?? 'Por defecto';
console.log(valor2); // 'Por defecto'

// Diferencia con ||
const edad1 = 0 || 18;      // 18 (0 es falsy)
const edad2 = 0 ?? 18;      // 0 (0 no es null/undefined) ✅

const texto1 = '' || 'Vacío';  // 'Vacío' ('' es falsy)
const texto2 = '' ?? 'Vacío';  // '' ('' no es null/undefined) ✅

// Casos prácticos
const configuracion = {
  tema: null,
  idioma: undefined,
  volumen: 0
};

const tema = configuracion.tema ?? 'claro';
console.log(tema); // 'claro'

const idioma = configuracion.idioma ?? 'es';
console.log(idioma); // 'es'

const volumen = configuracion.volumen ?? 50;
console.log(volumen); // 0 (no usa el default) ✅
```

---

## Optional Chaining (?.)

**📝 Descripción:** Accede a propiedades anidadas sin error si son `null` o `undefined`.

**💡 Ejemplos:**

```javascript
// Sin optional chaining
const usuario = {
  nombre: 'Ana',
  direccion: {
    calle: 'Principal',
    ciudad: 'Madrid'
  }
};

// ⚠️ Puede dar error si usuario.direccion no existe
// console.log(usuario.direccion.calle);

// Con optional chaining ✅
const usuario2 = {
  nombre: 'Juan'
  // Sin dirección
};

console.log(usuario2.direccion?.calle); // undefined (no hay error)
console.log(usuario2.direccion?.ciudad); // undefined

// En métodos
const obj = {
  metodo: () => 'Hola'
};

console.log(obj.metodo?.()); // 'Hola'
console.log(obj.otro?.()); // undefined (no hay error)

// En arrays
const lista = [1, 2, 3];
console.log(lista?.[0]); // 1

const listaVacia = null;
console.log(listaVacia?.[0]); // undefined (no hay error)

// Combinado con ??
const ciudad = usuario2.direccion?.ciudad ?? 'No especificada';
console.log(ciudad); // 'No especificada'

// API responses
const response = await fetch('/api/usuario');
const data = await response.json();
const email = data?.usuario?.contacto?.email ?? 'Sin email';
```

---

## Operadores de Tipo

### typeof
```javascript
console.log(typeof 'hola');      // 'string'
console.log(typeof 42);          // 'number'
console.log(typeof true);        // 'boolean'
console.log(typeof undefined);   // 'undefined'
console.log(typeof null);        // 'object' ⚠️ (bug)
console.log(typeof {});          // 'object'
console.log(typeof []);          // 'object'
console.log(typeof function(){}); // 'function'

// Uso práctico
function procesar(valor) {
  if (typeof valor === 'string') {
    return valor.toUpperCase();
  } else if (typeof valor === 'number') {
    return valor * 2;
  }
}
```

### instanceof
```javascript
const fecha = new Date();
console.log(fecha instanceof Date); // true

const arr = [1, 2, 3];
console.log(arr instanceof Array); // true

const obj = {};
console.log(obj instanceof Object); // true

// Uso práctico
function procesar(valor) {
  if (valor instanceof Array) {
    return valor.length;
  } else if (valor instanceof Date) {
    return valor.getFullYear();
  }
}
```

---

## Ejemplo Práctico: Calculadora de Descuentos

```javascript
function calcularPrecioFinal(precio, cantidad, esMiembro, codigoDescuento) {
  // Operadores aritméticos
  let subtotal = precio * cantidad;
  
  // Operador ternario para descuento de membresía
  const descuentoMiembro = esMiembro ? 0.10 : 0;
  
  // Optional chaining y nullish coalescing
  const descuentoCodigo = codigoDescuento?.toUpperCase() === 'VERANO' ? 0.15 : 0;
  
  // Operadores de comparación y lógicos
  const descuentoTotal = descuentoMiembro + descuentoCodigo;
  
  // Aplicar descuento
  subtotal -= subtotal * descuentoTotal;
  
  // Envío gratis si es mayor a 50
  const envio = subtotal >= 50 ? 0 : 5;
  
  // Total final
  const total = subtotal + envio;
  
  return {
    subtotal: subtotal.toFixed(2),
    descuento: (descuentoTotal * 100).toFixed(0) + '%',
    envio: envio.toFixed(2),
    total: total.toFixed(2)
  };
}

// Usar la función
const resultado = calcularPrecioFinal(25, 3, true, 'VERANO');
console.log(resultado);
/* 
{
  subtotal: '56.25',
  descuento: '25%',
  envio: '0.00',
  total: '56.25'
}
*/
```

---

## Resumen de Operadores

```javascript
// Aritméticos
+ - * / % **

// Asignación
= += -= *= /= %= **=

// Comparación
== === != !== > >= < <=

// Lógicos
&& || !

// Incremento/Decremento
++ --

// Ternario
? :

// Nullish & Optional
?? ?.

// Tipo
typeof instanceof
```
