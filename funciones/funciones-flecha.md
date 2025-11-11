# 🏹 Arrow Functions (Funciones Flecha)

## ¿Qué son las Arrow Functions?

Las **Arrow Functions** son una sintaxis moderna y compacta para escribir funciones en JavaScript (ES6+). Usan el símbolo `=>` (flecha).

---

## Sintaxis Básica

**✍️ Sintaxis:**
```javascript
// Función tradicional
function sumar(a, b) {
  return a + b;
}

// Arrow function
const sumar = (a, b) => {
  return a + b;
};

// Arrow function compacta (return implícito)
const sumar = (a, b) => a + b;
```

---

## Diferentes Formas

### Sin parámetros
```javascript
// Función tradicional
function saludar() {
  return 'Hola';
}

// Arrow function
const saludar = () => 'Hola';

console.log(saludar()); // 'Hola'
```

### Un parámetro
```javascript
// Función tradicional
function doble(n) {
  return n * 2;
}

// Arrow function (paréntesis opcionales con 1 parámetro)
const doble = n => n * 2;

console.log(doble(5)); // 10
```

### Múltiples parámetros
```javascript
// Función tradicional
function suma(a, b) {
  return a + b;
}

// Arrow function
const suma = (a, b) => a + b;

console.log(suma(3, 7)); // 10
```

### Cuerpo con múltiples líneas
```javascript
const calcularDescuento = (precio, porcentaje) => {
  const descuento = precio * (porcentaje / 100);
  const precioFinal = precio - descuento;
  return precioFinal;
};

console.log(calcularDescuento(100, 20)); // 80
```

### Retornar objetos
```javascript
// ⚠️ Error: JavaScript confunde {} con el cuerpo de la función
const crearPersona = (nombre, edad) => { nombre: nombre, edad: edad };

// ✅ Correcto: envolver el objeto en paréntesis
const crearPersona = (nombre, edad) => ({ nombre: nombre, edad: edad });

// Con shorthand property
const crearPersona = (nombre, edad) => ({ nombre, edad });

console.log(crearPersona('Ana', 25));
// { nombre: 'Ana', edad: 25 }
```

---

## Diferencias con Funciones Tradicionales

### 1. this (contexto)

**⚠️ Importante:** Arrow functions NO tienen su propio `this`, heredan el `this` del contexto donde fueron creadas.

```javascript
// Función tradicional
const persona = {
  nombre: 'Ana',
  saludar: function() {
    console.log(`Hola, soy ${this.nombre}`);
  }
};

persona.saludar(); // 'Hola, soy Ana' ✅

// Arrow function en método (⚠️ problema)
const persona2 = {
  nombre: 'Juan',
  saludar: () => {
    console.log(`Hola, soy ${this.nombre}`); // this no es persona2
  }
};

persona2.saludar(); // 'Hola, soy undefined' ❌

// ✅ Buen uso de arrow functions (callbacks)
const persona3 = {
  nombre: 'María',
  amigos: ['Pedro', 'Luis'],
  mostrarAmigos: function() {
    // Arrow function hereda this de mostrarAmigos
    this.amigos.forEach(amigo => {
      console.log(`${this.nombre} es amigo de ${amigo}`);
    });
  }
};

persona3.mostrarAmigos();
// María es amigo de Pedro
// María es amigo de Luis

// Sin arrow function (problema clásico)
const persona4 = {
  nombre: 'Carlos',
  amigos: ['Ana', 'Luis'],
  mostrarAmigos: function() {
    this.amigos.forEach(function(amigo) {
      console.log(`${this.nombre} es amigo de ${amigo}`); // this es undefined
    });
  }
};

persona4.mostrarAmigos();
// undefined es amigo de Ana
// undefined es amigo de Luis
```

### 2. arguments

**Arrow functions NO tienen el objeto `arguments`**

```javascript
// Función tradicional
function sumarTodos() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

console.log(sumarTodos(1, 2, 3, 4)); // 10

// Arrow function (❌ no tiene arguments)
const sumarTodos2 = () => {
  // console.log(arguments); // Error!
};

// ✅ Solución: rest parameters
const sumarTodos3 = (...numeros) => {
  return numeros.reduce((total, num) => total + num, 0);
};

console.log(sumarTodos3(1, 2, 3, 4)); // 10
```

### 3. No se pueden usar como constructores

```javascript
// Función tradicional
function Persona(nombre) {
  this.nombre = nombre;
}

const persona = new Persona('Ana'); // ✅ Funciona

// Arrow function
const Persona2 = (nombre) => {
  this.nombre = nombre;
};

// const persona2 = new Persona2('Juan'); // ❌ Error!
```

---

## Casos de Uso Comunes

### 1. Métodos de Arrays

```javascript
const numeros = [1, 2, 3, 4, 5];

// map
const dobles = numeros.map(n => n * 2);
console.log(dobles); // [2, 4, 6, 8, 10]

// filter
const pares = numeros.filter(n => n % 2 === 0);
console.log(pares); // [2, 4]

// reduce
const suma = numeros.reduce((acc, n) => acc + n, 0);
console.log(suma); // 15

// find
const numero = numeros.find(n => n > 3);
console.log(numero); // 4

// every
const todosMayores = numeros.every(n => n > 0);
console.log(todosMayores); // true

// some
const hayPares = numeros.some(n => n % 2 === 0);
console.log(hayPares); // true
```

### 2. Event Listeners

```javascript
const boton = document.getElementById('btn');

// ✅ Bueno para funciones simples
boton.addEventListener('click', () => {
  console.log('Click!');
});

// ⚠️ Problema con this
boton.addEventListener('click', () => {
  console.log(this); // Window, no el botón
});

// ✅ Usar función tradicional si necesitas this
boton.addEventListener('click', function() {
  console.log(this); // El botón
  this.textContent = 'Clickeado!';
});
```

### 3. setTimeout y setInterval

```javascript
const usuario = {
  nombre: 'Ana',
  mostrarNombreDespues: function() {
    // Arrow function hereda this
    setTimeout(() => {
      console.log(this.nombre);
    }, 1000);
  }
};

usuario.mostrarNombreDespues(); // 'Ana' después de 1 segundo

// Sin arrow function
const usuario2 = {
  nombre: 'Juan',
  mostrarNombreDespues: function() {
    setTimeout(function() {
      console.log(this.nombre); // undefined
    }, 1000);
  }
};
```

### 4. Promises y Async/Await

```javascript
// Promises
fetch('https://api.ejemplo.com/datos')
  .then(response => response.json())
  .then(datos => console.log(datos))
  .catch(error => console.error(error));

// Async/Await
const obtenerDatos = async () => {
  try {
    const response = await fetch('https://api.ejemplo.com/datos');
    const datos = await response.json();
    return datos;
  } catch (error) {
    console.error(error);
  }
};
```

### 5. Funciones de Orden Superior

```javascript
// Retornar funciones
const multiplicador = factor => numero => numero * factor;

const doble = multiplicador(2);
const triple = multiplicador(3);

console.log(doble(5));  // 10
console.log(triple(5)); // 15

// Funciones como argumentos
const procesarArray = (arr, callback) => {
  return arr.map(callback);
};

const numeros = [1, 2, 3];
const cuadrados = procesarArray(numeros, n => n ** 2);
console.log(cuadrados); // [1, 4, 9]
```

---

## Ejemplo Práctico: Filtro de Productos

```javascript
const productos = [
  { id: 1, nombre: 'Laptop', precio: 1200, categoria: 'Electrónica', stock: 5 },
  { id: 2, nombre: 'Mouse', precio: 25, categoria: 'Electrónica', stock: 0 },
  { id: 3, nombre: 'Teclado', precio: 80, categoria: 'Electrónica', stock: 10 },
  { id: 4, nombre: 'Monitor', precio: 300, categoria: 'Electrónica', stock: 3 },
  { id: 5, nombre: 'Silla', precio: 150, categoria: 'Muebles', stock: 8 }
];

// Filtrar por categoría
const filtrarPorCategoria = (productos, categoria) =>
  productos.filter(p => p.categoria === categoria);

// Productos en stock
const productosEnStock = productos => 
  productos.filter(p => p.stock > 0);

// Ordenar por precio
const ordenarPorPrecio = (productos, ascendente = true) =>
  productos.sort((a, b) => ascendente ? a.precio - b.precio : b.precio - a.precio);

// Calcular total
const calcularTotal = productos =>
  productos.reduce((total, p) => total + p.precio, 0);

// Agregar descuento
const aplicarDescuento = (productos, porcentaje) =>
  productos.map(p => ({
    ...p,
    precio: p.precio * (1 - porcentaje / 100),
    precioOriginal: p.precio
  }));

// Buscar por nombre
const buscarProducto = (productos, termino) =>
  productos.find(p => p.nombre.toLowerCase().includes(termino.toLowerCase()));

// Usar las funciones
const electronica = filtrarPorCategoria(productos, 'Electrónica');
console.log(electronica);

const disponibles = productosEnStock(productos);
console.log(disponibles);

const ordenados = ordenarPorPrecio([...productos], true);
console.log(ordenados);

const total = calcularTotal(productos);
console.log(`Total: $${total}`);

const conDescuento = aplicarDescuento(productos, 10);
console.log(conDescuento);

const laptop = buscarProducto(productos, 'laptop');
console.log(laptop);
```

---

## Ejemplo Práctico: Gestión de Tareas

```javascript
class GestorTareas {
  constructor() {
    this.tareas = [];
  }

  // Agregar tarea
  agregar = (titulo, prioridad = 'media') => {
    const tarea = {
      id: Date.now(),
      titulo,
      prioridad,
      completada: false,
      fecha: new Date()
    };
    this.tareas.push(tarea);
    return tarea;
  };

  // Completar tarea
  completar = id => {
    this.tareas = this.tareas.map(tarea =>
      tarea.id === id ? { ...tarea, completada: true } : tarea
    );
  };

  // Eliminar tarea
  eliminar = id => {
    this.tareas = this.tareas.filter(tarea => tarea.id !== id);
  };

  // Obtener pendientes
  obtenerPendientes = () =>
    this.tareas.filter(tarea => !tarea.completada);

  // Obtener por prioridad
  obtenerPorPrioridad = prioridad =>
    this.tareas.filter(tarea => tarea.prioridad === prioridad);

  // Contar tareas
  contar = () => ({
    total: this.tareas.length,
    completadas: this.tareas.filter(t => t.completada).length,
    pendientes: this.tareas.filter(t => !t.completada).length
  });
}

// Usar el gestor
const gestor = new GestorTareas();

gestor.agregar('Estudiar JavaScript', 'alta');
gestor.agregar('Hacer ejercicio', 'media');
gestor.agregar('Comprar comida', 'baja');

console.log(gestor.obtenerPendientes());
console.log(gestor.contar());

gestor.completar(gestor.tareas[0].id);
console.log(gestor.contar());
```

---

## Buenas Prácticas

```javascript
// ✅ Bueno: Arrow functions para callbacks
const numeros = [1, 2, 3, 4, 5];
const dobles = numeros.map(n => n * 2);

// ✅ Bueno: Funciones cortas y expresivas
const esPar = n => n % 2 === 0;
const esMayorDeEdad = edad => edad >= 18;

// ❌ Malo: Arrow function como método de objeto
const persona = {
  nombre: 'Ana',
  saludar: () => console.log(this.nombre) // undefined
};

// ✅ Bueno: Función tradicional para métodos
const persona2 = {
  nombre: 'Ana',
  saludar() {
    console.log(this.nombre); // 'Ana'
  }
};

// ✅ Bueno: Arrow function en callback
persona2.amigos = ['Luis', 'María'];
persona2.mostrarAmigos = function() {
  this.amigos.forEach(amigo => {
    console.log(`${this.nombre} - ${amigo}`);
  });
};

// ✅ Bueno: Nombre descriptivo incluso siendo compacto
const calcularImpuesto = precio => precio * 0.21;
const esEmailValido = email => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);

// ❌ Evitar: Demasiado compacto y confuso
const x = a => b => c => a + b + c;

// ✅ Mejor: Más legible
const sumar = a => b => c => a + b + c;
// O mejor aún:
const sumar = (a, b, c) => a + b + c;
```

---

## Resumen

| Característica | Arrow Function | Función Tradicional |
|---------------|----------------|---------------------|
| Sintaxis | Compacta | Más verbosa |
| `this` | Heredado | Propio |
| `arguments` | ❌ No tiene | ✅ Sí tiene |
| Constructor | ❌ No | ✅ Sí |
| Return implícito | ✅ Sí (una línea) | ❌ No |
| Hoisting | ❌ No | ✅ Sí |

**Cuándo usar Arrow Functions:**
- ✅ Callbacks (map, filter, reduce, etc.)
- ✅ Funciones cortas y simples
- ✅ Cuando necesitas heredar `this`
- ✅ Promises y async/await

**Cuándo usar Funciones Tradicionales:**
- ✅ Métodos de objetos
- ✅ Cuando necesitas `this` propio
- ✅ Cuando necesitas `arguments`
- ✅ Constructores
