# 🔄 Callbacks en JavaScript

## ¿Qué es un Callback?

Un **callback** es una función que se pasa como argumento a otra función y se ejecuta después de que se complete alguna operación.

---

## Concepto Básico

**✍️ Sintaxis:**
```javascript
function hacerAlgo(callback) {
  // Hacer algo...
  callback(); // Ejecutar el callback
}
```

**💡 Ejemplo Simple:**
```javascript
function saludar(nombre) {
  console.log(`Hola ${nombre}`);
}

function procesarEntrada(callback) {
  const nombre = 'Ana';
  callback(nombre);
}

procesarEntrada(saludar); // 'Hola Ana'
```

---

## Callbacks Síncronos

**📝 Descripción:** Se ejecutan inmediatamente, en orden.

**💡 Ejemplos:**

### Array Methods
```javascript
const numeros = [1, 2, 3, 4, 5];

// forEach - ejecuta callback para cada elemento
numeros.forEach(function(numero) {
  console.log(numero * 2);
});
// 2, 4, 6, 8, 10

// map - crea nuevo array con los resultados
const dobles = numeros.map(function(numero) {
  return numero * 2;
});
console.log(dobles); // [2, 4, 6, 8, 10]

// filter - filtra elementos
const pares = numeros.filter(function(numero) {
  return numero % 2 === 0;
});
console.log(pares); // [2, 4]

// reduce - reduce a un solo valor
const suma = numeros.reduce(function(acumulador, numero) {
  return acumulador + numero;
}, 0);
console.log(suma); // 15
```

### Con Arrow Functions
```javascript
const numeros = [1, 2, 3, 4, 5];

// Más compacto
numeros.forEach(num => console.log(num * 2));

const dobles = numeros.map(num => num * 2);
const pares = numeros.filter(num => num % 2 === 0);
const suma = numeros.reduce((acc, num) => acc + num, 0);
```

### Función Personalizada con Callback
```javascript
function procesarArray(array, callback) {
  const resultado = [];
  for (let i = 0; i < array.length; i++) {
    resultado.push(callback(array[i]));
  }
  return resultado;
}

const numeros = [1, 2, 3, 4];

const cuadrados = procesarArray(numeros, function(n) {
  return n ** 2;
});
console.log(cuadrados); // [1, 4, 9, 16]

const dobles = procesarArray(numeros, n => n * 2);
console.log(dobles); // [2, 4, 6, 8]
```

---

## Callbacks Asíncronos

**📝 Descripción:** Se ejecutan después de completarse una operación asíncrona.

### setTimeout
```javascript
console.log('Inicio');

setTimeout(function() {
  console.log('Después de 2 segundos');
}, 2000);

console.log('Fin');

// Salida:
// Inicio
// Fin
// Después de 2 segundos (después de 2 seg)

// Con arrow function
setTimeout(() => {
  console.log('Callback después de 1 segundo');
}, 1000);
```

### setInterval
```javascript
let contador = 0;

const intervalo = setInterval(function() {
  contador++;
  console.log(`Contador: ${contador}`);
  
  if (contador >= 5) {
    clearInterval(intervalo);
    console.log('Intervalo detenido');
  }
}, 1000);

// Salida:
// Contador: 1 (después de 1 seg)
// Contador: 2 (después de 2 seg)
// Contador: 3 (después de 3 seg)
// Contador: 4 (después de 4 seg)
// Contador: 5 (después de 5 seg)
// Intervalo detenido
```

### Event Listeners
```javascript
const boton = document.getElementById('btn');

// Callback se ejecuta cuando se hace click
boton.addEventListener('click', function() {
  console.log('Botón clickeado!');
});

// Con arrow function
boton.addEventListener('click', () => {
  console.log('Click!');
});

// Callback con parámetro de evento
boton.addEventListener('click', function(evento) {
  console.log('Tipo de evento:', evento.type);
  console.log('Target:', evento.target);
});
```

---

## Callbacks Anidados (Callback Hell)

**⚠️ Problema:** Múltiples callbacks anidados pueden volverse difíciles de leer.

```javascript
// ❌ Callback Hell
setTimeout(() => {
  console.log('1. Primera operación');
  setTimeout(() => {
    console.log('2. Segunda operación');
    setTimeout(() => {
      console.log('3. Tercera operación');
      setTimeout(() => {
        console.log('4. Cuarta operación');
      }, 1000);
    }, 1000);
  }, 1000);
}, 1000);

// Difícil de leer y mantener! 😵
```

**✅ Solución: Funciones Nombradas**
```javascript
function primeraOperacion() {
  console.log('1. Primera operación');
  setTimeout(segundaOperacion, 1000);
}

function segundaOperacion() {
  console.log('2. Segunda operación');
  setTimeout(terceraOperacion, 1000);
}

function terceraOperacion() {
  console.log('3. Tercera operación');
  setTimeout(cuartaOperacion, 1000);
}

function cuartaOperacion() {
  console.log('4. Cuarta operación');
}

setTimeout(primeraOperacion, 1000);

// Más legible ✅
```

**✅ Mejor Solución: Promises o Async/Await**
```javascript
// Con Promises
function esperar(ms, mensaje) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log(mensaje);
      resolve();
    }, ms);
  });
}

esperar(1000, '1. Primera operación')
  .then(() => esperar(1000, '2. Segunda operación'))
  .then(() => esperar(1000, '3. Tercera operación'))
  .then(() => esperar(1000, '4. Cuarta operación'));

// Con Async/Await (aún mejor)
async function ejecutarOperaciones() {
  await esperar(1000, '1. Primera operación');
  await esperar(1000, '2. Segunda operación');
  await esperar(1000, '3. Tercera operación');
  await esperar(1000, '4. Cuarta operación');
}

ejecutarOperaciones();
```

---

## Manejo de Errores en Callbacks

### Patrón Error-First Callback (Node.js)
```javascript
function leerArchivo(nombre, callback) {
  // Simular lectura de archivo
  setTimeout(() => {
    if (nombre === '') {
      callback('Error: nombre vacío', null);
    } else {
      callback(null, `Contenido de ${nombre}`);
    }
  }, 1000);
}

// Usar con error-first callback
leerArchivo('datos.txt', function(error, datos) {
  if (error) {
    console.error('Error:', error);
    return;
  }
  console.log('Datos:', datos);
});

// Error: nombre vacío
leerArchivo('', function(error, datos) {
  if (error) {
    console.error('Error:', error); // Se ejecuta esto
    return;
  }
  console.log('Datos:', datos);
});
```

### Try-Catch con Callbacks Síncronos
```javascript
function procesarDatos(datos, callback) {
  try {
    const resultado = callback(datos);
    return resultado;
  } catch (error) {
    console.error('Error en callback:', error.message);
    return null;
  }
}

// Callback que puede fallar
const resultado = procesarDatos([1, 2, 3], function(arr) {
  return arr.map(n => n * 2);
});
console.log(resultado); // [2, 4, 6]

// Callback con error
const resultado2 = procesarDatos(null, function(arr) {
  return arr.map(n => n * 2); // Error: arr es null
});
console.log(resultado2); // null
```

---

## Ejemplos Prácticos

### 1. Sistema de Login
```javascript
function validarUsuario(usuario, password, callbackExito, callbackError) {
  // Simular validación asíncrona
  setTimeout(() => {
    if (usuario === 'admin' && password === '1234') {
      callbackExito({ id: 1, nombre: 'Admin', rol: 'administrador' });
    } else {
      callbackError('Usuario o contraseña incorrectos');
    }
  }, 1000);
}

// Usar
validarUsuario(
  'admin',
  '1234',
  function(datosUsuario) {
    console.log('Login exitoso!');
    console.log('Bienvenido:', datosUsuario.nombre);
  },
  function(error) {
    console.error('Error de login:', error);
  }
);
```

### 2. Cargar Datos con Loading
```javascript
function cargarDatos(callback) {
  console.log('Cargando datos...');
  
  // Simular carga de datos
  setTimeout(() => {
    const datos = [
      { id: 1, nombre: 'Producto 1' },
      { id: 2, nombre: 'Producto 2' },
      { id: 3, nombre: 'Producto 3' }
    ];
    
    callback(datos);
  }, 2000);
}

// Usar
cargarDatos(function(productos) {
  console.log('Datos cargados!');
  productos.forEach(p => console.log(p.nombre));
});
```

### 3. Procesador de Formulario
```javascript
function procesarFormulario(datos, callbacks) {
  // Validar
  if (!datos.nombre || !datos.email) {
    callbacks.onError('Campos requeridos faltantes');
    return;
  }
  
  callbacks.onValidate();
  
  // Simular envío
  setTimeout(() => {
    const exito = Math.random() > 0.3;
    
    if (exito) {
      callbacks.onSuccess('Formulario enviado exitosamente');
    } else {
      callbacks.onError('Error al enviar formulario');
    }
  }, 1500);
}

// Usar
const formularioDatos = {
  nombre: 'Ana García',
  email: 'ana@example.com'
};

procesarFormulario(formularioDatos, {
  onValidate: function() {
    console.log('Validando...');
  },
  onSuccess: function(mensaje) {
    console.log('✅', mensaje);
  },
  onError: function(mensaje) {
    console.error('❌', mensaje);
  }
});
```

### 4. Filtro Personalizado
```javascript
function filtrarProductos(productos, criterios, callback) {
  let resultado = productos;
  
  // Filtrar por categoría
  if (criterios.categoria) {
    resultado = resultado.filter(p => p.categoria === criterios.categoria);
  }
  
  // Filtrar por precio
  if (criterios.precioMax) {
    resultado = resultado.filter(p => p.precio <= criterios.precioMax);
  }
  
  // Aplicar callback personalizado
  if (callback) {
    resultado = callback(resultado);
  }
  
  return resultado;
}

const productos = [
  { nombre: 'Laptop', precio: 1200, categoria: 'Electrónica' },
  { nombre: 'Mouse', precio: 25, categoria: 'Electrónica' },
  { nombre: 'Silla', precio: 150, categoria: 'Muebles' },
  { nombre: 'Monitor', precio: 300, categoria: 'Electrónica' }
];

// Usar con callback de ordenamiento
const resultado = filtrarProductos(
  productos,
  { categoria: 'Electrónica', precioMax: 500 },
  function(productos) {
    return productos.sort((a, b) => a.precio - b.precio);
  }
);

console.log(resultado);
// [
//   { nombre: 'Mouse', precio: 25, categoria: 'Electrónica' },
//   { nombre: 'Monitor', precio: 300, categoria: 'Electrónica' }
// ]
```

### 5. Animación con Callbacks
```javascript
function animar(elemento, propiedad, valor, duracion, callback) {
  let inicio = null;
  const valorInicial = parseFloat(getComputedStyle(elemento)[propiedad]);
  
  function paso(timestamp) {
    if (!inicio) inicio = timestamp;
    const progreso = timestamp - inicio;
    const porcentaje = Math.min(progreso / duracion, 1);
    
    const valorActual = valorInicial + (valor - valorInicial) * porcentaje;
    elemento.style[propiedad] = valorActual + 'px';
    
    if (porcentaje < 1) {
      requestAnimationFrame(paso);
    } else {
      if (callback) callback();
    }
  }
  
  requestAnimationFrame(paso);
}

// Usar
const caja = document.getElementById('caja');

animar(caja, 'left', 300, 1000, function() {
  console.log('Animación completada!');
  animar(caja, 'top', 200, 1000, function() {
    console.log('Segunda animación completada!');
  });
});
```

---

## Diferencia: Callbacks vs Promises vs Async/Await

```javascript
// 1. CALLBACKS
function obtenerUsuario(id, callback) {
  setTimeout(() => {
    callback({ id: id, nombre: 'Usuario ' + id });
  }, 1000);
}

obtenerUsuario(1, function(usuario) {
  console.log(usuario);
  obtenerUsuario(2, function(usuario2) {
    console.log(usuario2); // Callback hell
  });
});

// 2. PROMISES
function obtenerUsuarioPromise(id) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ id: id, nombre: 'Usuario ' + id });
    }, 1000);
  });
}

obtenerUsuarioPromise(1)
  .then(usuario => {
    console.log(usuario);
    return obtenerUsuarioPromise(2);
  })
  .then(usuario2 => {
    console.log(usuario2);
  });

// 3. ASYNC/AWAIT (más limpio)
async function obtenerUsuarios() {
  const usuario1 = await obtenerUsuarioPromise(1);
  console.log(usuario1);
  
  const usuario2 = await obtenerUsuarioPromise(2);
  console.log(usuario2);
}

obtenerUsuarios();
```

---

## Buenas Prácticas

```javascript
// ✅ Bueno: Callback nombrado y reutilizable
const duplicar = n => n * 2;
const numeros = [1, 2, 3].map(duplicar);

// ❌ Evitar: Callback muy complejo
numeros.forEach(function(n) {
  if (n > 0) {
    if (n % 2 === 0) {
      console.log(n * 2);
    } else {
      console.log(n * 3);
    }
  }
});

// ✅ Mejor: Extraer lógica
function procesarNumero(n) {
  if (n <= 0) return;
  return n % 2 === 0 ? n * 2 : n * 3;
}
numeros.forEach(n => console.log(procesarNumero(n)));

// ✅ Bueno: Manejo de errores claro
function procesarDatos(datos, onSuccess, onError) {
  try {
    const resultado = datos.map(d => d * 2);
    onSuccess(resultado);
  } catch (error) {
    onError(error);
  }
}

// ✅ Bueno: Usar Promises para operaciones asíncronas complejas
// En lugar de callbacks anidados
```

---

## Resumen

**Ventajas de Callbacks:**
- ✅ Simples de entender
- ✅ Ampliamente soportados
- ✅ Perfectos para operaciones simples

**Desventajas:**
- ❌ Callback hell (anidamiento profundo)
- ❌ Difícil manejo de errores
- ❌ Menos legible en operaciones complejas

**Cuándo usar Callbacks:**
- ✅ Array methods (map, filter, etc.)
- ✅ Event listeners
- ✅ Operaciones asíncronas simples

**Cuándo usar alternativas:**
- ✅ Promesas para operaciones asíncronas encadenadas
- ✅ Async/Await para código asíncrono complejo
