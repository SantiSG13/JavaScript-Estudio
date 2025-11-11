# 📚 Métodos de Arrays en JavaScript

## Métodos de Iteración

### forEach()

**📝 Descripción:** Ejecuta una función para cada elemento del array. No retorna nada.

**✍️ Sintaxis:**
```javascript
array.forEach((elemento, indice, array) => {
  // código
});
```

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5];

// Básico
numeros.forEach(numero => {
  console.log(numero);
});
// 1, 2, 3, 4, 5

// Con índice
numeros.forEach((numero, indice) => {
  console.log(`Índice ${indice}: ${numero}`);
});

// Modificar elementos externos
const suma = [];
numeros.forEach(n => suma.push(n * 2));
console.log(suma); // [2, 4, 6, 8, 10]
```

---

### map()

**📝 Descripción:** Crea un nuevo array con los resultados de aplicar una función a cada elemento.

**📤 Retorna:** Nuevo array transformado

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5];

// Duplicar valores
const dobles = numeros.map(n => n * 2);
console.log(dobles); // [2, 4, 6, 8, 10]

// Transformar objetos
const usuarios = [
  { nombre: 'Ana', edad: 25 },
  { nombre: 'Juan', edad: 30 }
];

const nombres = usuarios.map(u => u.nombre);
console.log(nombres); // ['Ana', 'Juan']

// Agregar propiedad
const conId = usuarios.map((u, i) => ({
  ...u,
  id: i + 1
}));
console.log(conId);
// [{ id: 1, nombre: 'Ana', edad: 25 }, { id: 2, nombre: 'Juan', edad: 30 }]

// Template literals
const etiquetas = usuarios.map(u => `${u.nombre} (${u.edad} años)`);
console.log(etiquetas); // ['Ana (25 años)', 'Juan (30 años)']
```

---

### filter()

**📝 Descripción:** Crea un nuevo array con los elementos que cumplan una condición.

**📤 Retorna:** Nuevo array filtrado

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Números pares
const pares = numeros.filter(n => n % 2 === 0);
console.log(pares); // [2, 4, 6, 8, 10]

// Mayores que 5
const mayores = numeros.filter(n => n > 5);
console.log(mayores); // [6, 7, 8, 9, 10]

// Filtrar objetos
const productos = [
  { nombre: 'Laptop', precio: 1200, stock: 5 },
  { nombre: 'Mouse', precio: 25, stock: 0 },
  { nombre: 'Teclado', precio: 80, stock: 10 }
];

const enStock = productos.filter(p => p.stock > 0);
console.log(enStock); // Laptop y Teclado

const baratos = productos.filter(p => p.precio < 100);
console.log(baratos); // Mouse y Teclado

// Múltiples condiciones
const disponiblesBaratos = productos.filter(p => 
  p.stock > 0 && p.precio < 500
);
```

---

### find()

**📝 Descripción:** Retorna el primer elemento que cumpla la condición.

**📤 Retorna:** Elemento encontrado o `undefined`

**💡 Ejemplos:**
```javascript
const usuarios = [
  { id: 1, nombre: 'Ana', activo: true },
  { id: 2, nombre: 'Juan', activo: false },
  { id: 3, nombre: 'María', activo: true }
];

// Encontrar por id
const usuario = usuarios.find(u => u.id === 2);
console.log(usuario); // { id: 2, nombre: 'Juan', activo: false }

// Primer usuario activo
const activo = usuarios.find(u => u.activo);
console.log(activo); // { id: 1, nombre: 'Ana', activo: true }

// No encontrado
const noExiste = usuarios.find(u => u.id === 999);
console.log(noExiste); // undefined

// Con validación
const buscarUsuario = id => {
  const usuario = usuarios.find(u => u.id === id);
  return usuario || { error: 'Usuario no encontrado' };
};
```

---

### findIndex()

**📝 Descripción:** Retorna el índice del primer elemento que cumpla la condición.

**📤 Retorna:** Índice o `-1` si no se encuentra

**💡 Ejemplos:**
```javascript
const numeros = [10, 20, 30, 40, 50];

const indice = numeros.findIndex(n => n === 30);
console.log(indice); // 2

const indice2 = numeros.findIndex(n => n > 25);
console.log(indice2); // 2 (primer número > 25 es 30)

const noExiste = numeros.findIndex(n => n === 100);
console.log(noExiste); // -1

// Encontrar y modificar
const productos = [
  { id: 1, nombre: 'Laptop', precio: 1200 },
  { id: 2, nombre: 'Mouse', precio: 25 }
];

const indiceProducto = productos.findIndex(p => p.id === 2);
if (indiceProducto !== -1) {
  productos[indiceProducto].precio = 30;
}
console.log(productos);
```

---

### reduce()

**📝 Descripción:** Reduce el array a un solo valor aplicando una función acumuladora.

**✍️ Sintaxis:**
```javascript
array.reduce((acumulador, elemento, indice, array) => {
  // retornar nuevo acumulador
}, valorInicial);
```

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5];

// Suma total
const suma = numeros.reduce((acc, n) => acc + n, 0);
console.log(suma); // 15

// Producto
const producto = numeros.reduce((acc, n) => acc * n, 1);
console.log(producto); // 120

// Valor máximo
const max = numeros.reduce((acc, n) => n > acc ? n : acc, numeros[0]);
console.log(max); // 5

// Contar ocurrencias
const frutas = ['manzana', 'pera', 'manzana', 'uva', 'pera', 'manzana'];
const conteo = frutas.reduce((acc, fruta) => {
  acc[fruta] = (acc[fruta] || 0) + 1;
  return acc;
}, {});
console.log(conteo); // { manzana: 3, pera: 2, uva: 1 }

// Aplanar arrays
const anidado = [[1, 2], [3, 4], [5, 6]];
const plano = anidado.reduce((acc, arr) => acc.concat(arr), []);
console.log(plano); // [1, 2, 3, 4, 5, 6]

// Agrupar por propiedad
const personas = [
  { nombre: 'Ana', edad: 25 },
  { nombre: 'Juan', edad: 30 },
  { nombre: 'María', edad: 25 }
];

const porEdad = personas.reduce((acc, persona) => {
  const edad = persona.edad;
  if (!acc[edad]) acc[edad] = [];
  acc[edad].push(persona);
  return acc;
}, {});
console.log(porEdad);
// { 25: [{Ana}, {María}], 30: [{Juan}] }
```

---

### some()

**📝 Descripción:** Verifica si al menos un elemento cumple la condición.

**📤 Retorna:** `true` o `false`

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5];

// ¿Hay algún par?
const hayPar = numeros.some(n => n % 2 === 0);
console.log(hayPar); // true

// ¿Hay alguno mayor que 10?
const hayMayor = numeros.some(n => n > 10);
console.log(hayMayor); // false

// Validar permisos
const usuarios = [
  { nombre: 'Ana', rol: 'usuario' },
  { nombre: 'Juan', rol: 'admin' }
];

const hayAdmin = usuarios.some(u => u.rol === 'admin');
console.log(hayAdmin); // true

// Validar formulario
const campos = [
  { nombre: 'email', valido: true },
  { nombre: 'password', valido: false }
];

const hayError = campos.some(c => !c.valido);
console.log(hayError); // true
```

---

### every()

**📝 Descripción:** Verifica si todos los elementos cumplen la condición.

**📤 Retorna:** `true` o `false`

**💡 Ejemplos:**
```javascript
const numeros = [2, 4, 6, 8, 10];

// ¿Todos son pares?
const todosPares = numeros.every(n => n % 2 === 0);
console.log(todosPares); // true

// ¿Todos mayores que 5?
const todosMayores = numeros.every(n => n > 5);
console.log(todosMayores); // false

// Validar edad
const usuarios = [
  { nombre: 'Ana', edad: 25 },
  { nombre: 'Juan', edad: 30 },
  { nombre: 'María', edad: 28 }
];

const todosMayores18 = usuarios.every(u => u.edad >= 18);
console.log(todosMayores18); // true

// Validar campos completos
const formulario = {
  nombre: 'Ana',
  email: 'ana@example.com',
  password: '12345'
};

const todosCompletos = Object.values(formulario).every(v => v !== '');
console.log(todosCompletos); // true
```

---

## Métodos de Modificación

### push()

**📝 Descripción:** Agrega uno o más elementos al final del array.

**📤 Retorna:** Nueva longitud del array

**💡 Ejemplos:**
```javascript
const frutas = ['manzana', 'pera'];

frutas.push('uva');
console.log(frutas); // ['manzana', 'pera', 'uva']

// Múltiples elementos
frutas.push('naranja', 'sandía');
console.log(frutas); // ['manzana', 'pera', 'uva', 'naranja', 'sandía']

// Retorna la nueva longitud
const longitud = frutas.push('melón');
console.log(longitud); // 6
```

---

### pop()

**📝 Descripción:** Elimina y retorna el último elemento del array.

**📤 Retorna:** Elemento eliminado o `undefined`

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5];

const ultimo = numeros.pop();
console.log(ultimo); // 5
console.log(numeros); // [1, 2, 3, 4]

// Array vacío
const vacio = [];
console.log(vacio.pop()); // undefined
```

---

### unshift()

**📝 Descripción:** Agrega uno o más elementos al inicio del array.

**📤 Retorna:** Nueva longitud del array

**💡 Ejemplos:**
```javascript
const numeros = [3, 4, 5];

numeros.unshift(2);
console.log(numeros); // [2, 3, 4, 5]

numeros.unshift(0, 1);
console.log(numeros); // [0, 1, 2, 3, 4, 5]
```

---

### shift()

**📝 Descripción:** Elimina y retorna el primer elemento del array.

**📤 Retorna:** Elemento eliminado o `undefined`

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5];

const primero = numeros.shift();
console.log(primero); // 1
console.log(numeros); // [2, 3, 4, 5]
```

---

### splice()

**📝 Descripción:** Agrega, elimina o reemplaza elementos en cualquier posición.

**✍️ Sintaxis:**
```javascript
array.splice(inicio, cantidadEliminar, elemento1, elemento2, ...)
```

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5];

// Eliminar elementos
numeros.splice(2, 1); // Elimina 1 elemento desde índice 2
console.log(numeros); // [1, 2, 4, 5]

// Agregar elementos
numeros.splice(2, 0, 3); // Agrega 3 en índice 2 sin eliminar
console.log(numeros); // [1, 2, 3, 4, 5]

// Reemplazar elementos
numeros.splice(0, 2, 10, 20); // Reemplaza primeros 2
console.log(numeros); // [10, 20, 3, 4, 5]

// Eliminar desde una posición hasta el final
numeros.splice(2); // Elimina desde índice 2 hasta el final
console.log(numeros); // [10, 20]
```

---

### slice()

**📝 Descripción:** Retorna una copia de una porción del array (no modifica el original).

**✍️ Sintaxis:**
```javascript
array.slice(inicio, fin) // fin no incluido
```

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5];

// Del índice 1 al 3 (no incluido)
const porcion = numeros.slice(1, 3);
console.log(porcion); // [2, 3]
console.log(numeros); // [1, 2, 3, 4, 5] (sin cambios)

// Desde un índice hasta el final
const resto = numeros.slice(2);
console.log(resto); // [3, 4, 5]

// Clonar array
const copia = numeros.slice();
console.log(copia); // [1, 2, 3, 4, 5]

// Índices negativos (desde el final)
const ultimos = numeros.slice(-2);
console.log(ultimos); // [4, 5]
```

---

## Métodos de Búsqueda

### includes()

**📝 Descripción:** Verifica si un elemento existe en el array.

**📤 Retorna:** `true` o `false`

**💡 Ejemplos:**
```javascript
const frutas = ['manzana', 'pera', 'uva'];

console.log(frutas.includes('pera')); // true
console.log(frutas.includes('sandía')); // false

// Con números
const numeros = [1, 2, 3, 4, 5];
console.log(numeros.includes(3)); // true

// Case sensitive
const nombres = ['Ana', 'Juan', 'María'];
console.log(nombres.includes('ana')); // false
```

---

### indexOf()

**📝 Descripción:** Retorna el índice de la primera ocurrencia del elemento.

**📤 Retorna:** Índice o `-1` si no existe

**💡 Ejemplos:**
```javascript
const frutas = ['manzana', 'pera', 'uva', 'pera'];

console.log(frutas.indexOf('pera')); // 1 (primera ocurrencia)
console.log(frutas.indexOf('sandía')); // -1

// Buscar desde un índice
console.log(frutas.indexOf('pera', 2)); // 3 (segunda ocurrencia)

// Verificar si existe
if (frutas.indexOf('manzana') !== -1) {
  console.log('Manzana encontrada');
}
```

---

### lastIndexOf()

**📝 Descripción:** Retorna el índice de la última ocurrencia del elemento.

**📤 Retorna:** Índice o `-1`

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 2, 1];

console.log(numeros.lastIndexOf(2)); // 3 (última posición de 2)
console.log(numeros.lastIndexOf(1)); // 4
console.log(numeros.lastIndexOf(5)); // -1
```

---

## Métodos de Ordenamiento

### sort()

**📝 Descripción:** Ordena los elementos del array (modifica el original).

**💡 Ejemplos:**
```javascript
// Strings (orden alfabético)
const frutas = ['pera', 'manzana', 'uva', 'banana'];
frutas.sort();
console.log(frutas); // ['banana', 'manzana', 'pera', 'uva']

// Números (requiere función comparadora)
const numeros = [40, 1, 5, 200, 10];

// ❌ Malo: convierte a string
numeros.sort();
console.log(numeros); // [1, 10, 200, 40, 5]

// ✅ Bueno: función comparadora
numeros.sort((a, b) => a - b); // Ascendente
console.log(numeros); // [1, 5, 10, 40, 200]

numeros.sort((a, b) => b - a); // Descendente
console.log(numeros); // [200, 40, 10, 5, 1]

// Ordenar objetos
const usuarios = [
  { nombre: 'Juan', edad: 30 },
  { nombre: 'Ana', edad: 25 },
  { nombre: 'María', edad: 28 }
];

usuarios.sort((a, b) => a.edad - b.edad);
console.log(usuarios); // Ordenados por edad

usuarios.sort((a, b) => a.nombre.localeCompare(b.nombre));
console.log(usuarios); // Ordenados por nombre
```

---

### reverse()

**📝 Descripción:** Invierte el orden de los elementos (modifica el original).

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5];

numeros.reverse();
console.log(numeros); // [5, 4, 3, 2, 1]

// Combinar con sort
const letras = ['c', 'a', 'd', 'b'];
letras.sort().reverse();
console.log(letras); // ['d', 'c', 'b', 'a']
```

---

## Métodos de Concatenación

### concat()

**📝 Descripción:** Combina arrays (no modifica los originales).

**📤 Retorna:** Nuevo array

**💡 Ejemplos:**
```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const combinado = arr1.concat(arr2);
console.log(combinado); // [1, 2, 3, 4, 5, 6]

// Múltiples arrays
const arr3 = [7, 8, 9];
const todos = arr1.concat(arr2, arr3);
console.log(todos); // [1, 2, 3, 4, 5, 6, 7, 8, 9]

// Con valores individuales
const conValores = arr1.concat(10, 11, arr2);
console.log(conValores); // [1, 2, 3, 10, 11, 4, 5, 6]
```

---

### join()

**📝 Descripción:** Une todos los elementos en un string.

**📤 Retorna:** String

**💡 Ejemplos:**
```javascript
const palabras = ['Hola', 'Mundo', 'JavaScript'];

console.log(palabras.join(' ')); // 'Hola Mundo JavaScript'
console.log(palabras.join('-')); // 'Hola-Mundo-JavaScript'
console.log(palabras.join('')); // 'HolaMundoJavaScript'

// Array de números
const numeros = [1, 2, 3, 4, 5];
console.log(numeros.join(', ')); // '1, 2, 3, 4, 5'
```

---

## Métodos Estáticos

### Array.from()

**📝 Descripción:** Crea un array desde un iterable o array-like object.

**💡 Ejemplos:**
```javascript
// Desde string
const letras = Array.from('hola');
console.log(letras); // ['h', 'o', 'l', 'a']

// Desde Set
const numeros = Array.from(new Set([1, 2, 2, 3, 3, 3]));
console.log(numeros); // [1, 2, 3]

// Con función de mapeo
const cuadrados = Array.from([1, 2, 3, 4, 5], n => n ** 2);
console.log(cuadrados); // [1, 4, 9, 16, 25]

// Crear rango
const rango = Array.from({ length: 5 }, (_, i) => i + 1);
console.log(rango); // [1, 2, 3, 4, 5]
```

---

### Array.isArray()

**📝 Descripción:** Verifica si un valor es un array.

**💡 Ejemplos:**
```javascript
console.log(Array.isArray([1, 2, 3])); // true
console.log(Array.isArray('hola')); // false
console.log(Array.isArray({ length: 2 })); // false
console.log(Array.isArray(null)); // false
```

---

## Ejemplo Práctico: Sistema de Productos

```javascript
let productos = [
  { id: 1, nombre: 'Laptop', precio: 1200, categoria: 'Electrónica', stock: 5 },
  { id: 2, nombre: 'Mouse', precio: 25, categoria: 'Electrónica', stock: 20 },
  { id: 3, nombre: 'Teclado', precio: 80, categoria: 'Electrónica', stock: 15 },
  { id: 4, nombre: 'Monitor', precio: 300, categoria: 'Electrónica', stock: 8 },
  { id: 5, nombre: 'Silla', precio: 150, categoria: 'Muebles', stock: 10 }
];

// Filtrar por categoría
const electronicos = productos.filter(p => p.categoria === 'Electrónica');
console.log('Electrónicos:', electronicos.length);

// Obtener nombres
const nombres = productos.map(p => p.nombre);
console.log('Productos:', nombres.join(', '));

// Calcular total de inventario
const valorTotal = productos.reduce((total, p) => {
  return total + (p.precio * p.stock);
}, 0);
console.log('Valor total:', valorTotal);

// Aplicar descuento del 10%
const conDescuento = productos.map(p => ({
  ...p,
  precioDescuento: p.precio * 0.9
}));

// Ordenar por precio
const ordenados = [...productos].sort((a, b) => a.precio - b.precio);

// Verificar stock bajo
const hayStockBajo = productos.some(p => p.stock < 10);
console.log('¿Hay productos con stock bajo?', hayStockBajo);

// Buscar producto
const producto = productos.find(p => p.id === 2);
console.log('Producto encontrado:', producto.nombre);

// Agregar nuevo producto
productos.push({
  id: 6,
  nombre: 'Escritorio',
  precio: 250,
  categoria: 'Muebles',
  stock: 5
});

console.log('Total productos:', productos.length);
```
