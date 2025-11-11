# 🎯 Spread y Rest Operators

## Spread Operator (...)

**📝 Descripción:** El operador spread (`...`) expande o "desparrama" elementos de un iterable (array, string, objeto).

---

## Spread con Arrays

### Copiar Arrays

**💡 Ejemplos:**
```javascript
const original = [1, 2, 3];

// Copiar array (shallow copy)
const copia = [...original];
copia.push(4);

console.log(original); // [1, 2, 3]
console.log(copia);    // [1, 2, 3, 4]

// Sin spread (referencia)
const referencia = original;
referencia.push(5);
console.log(original); // [1, 2, 3, 5] ⚠️ Se modificó!
```

### Concatenar Arrays

**💡 Ejemplos:**
```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

// Con spread
const combinado = [...arr1, ...arr2];
console.log(combinado); // [1, 2, 3, 4, 5, 6]

// Agregar elementos en medio
const conExtra = [...arr1, 99, ...arr2];
console.log(conExtra); // [1, 2, 3, 99, 4, 5, 6]

// Múltiples arrays
const arr3 = [7, 8, 9];
const todos = [...arr1, ...arr2, ...arr3];
console.log(todos); // [1, 2, 3, 4, 5, 6, 7, 8, 9]

// Comparación con concat
const conConcat = arr1.concat(arr2); // [1, 2, 3, 4, 5, 6]
```

### Agregar Elementos

**💡 Ejemplos:**
```javascript
const numeros = [2, 3, 4];

// Agregar al inicio
const conInicio = [1, ...numeros];
console.log(conInicio); // [1, 2, 3, 4]

// Agregar al final
const conFinal = [...numeros, 5];
console.log(conFinal); // [2, 3, 4, 5]

// Agregar al inicio y final
const ambos = [0, ...numeros, 5, 6];
console.log(ambos); // [0, 2, 3, 4, 5, 6]
```

### Pasar como Argumentos

**💡 Ejemplos:**
```javascript
const numeros = [1, 5, 3, 9, 2];

// Math.max con spread
const maximo = Math.max(...numeros);
console.log(maximo); // 9

// Math.min
const minimo = Math.min(...numeros);
console.log(minimo); // 1

// Sin spread (no funciona)
const max2 = Math.max(numeros);
console.log(max2); // NaN

// Función personalizada
function sumar(a, b, c) {
  return a + b + c;
}

const valores = [1, 2, 3];
console.log(sumar(...valores)); // 6
```

### Convertir String a Array

**💡 Ejemplos:**
```javascript
const texto = 'Hola';

const letras = [...texto];
console.log(letras); // ['H', 'o', 'l', 'a']

// Útil para contar caracteres únicos
const palabra = 'javascript';
const unicos = [...new Set([...palabra])];
console.log(unicos); // ['j', 'a', 'v', 's', 'c', 'r', 'i', 'p', 't']
```

---

## Spread con Objetos

### Copiar Objetos

**💡 Ejemplos:**
```javascript
const original = {
  nombre: 'Ana',
  edad: 25
};

// Copiar objeto (shallow copy)
const copia = { ...original };
copia.edad = 26;

console.log(original.edad); // 25
console.log(copia.edad);    // 26

// ⚠️ Shallow copy: objetos anidados son referencias
const usuario = {
  nombre: 'Juan',
  datos: { edad: 30 }
};

const copiaUsuario = { ...usuario };
copiaUsuario.datos.edad = 31;

console.log(usuario.datos.edad); // 31 ⚠️ Se modificó el original!
```

### Fusionar Objetos

**💡 Ejemplos:**
```javascript
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };

const fusionado = { ...obj1, ...obj2 };
console.log(fusionado); // { a: 1, b: 2, c: 3, d: 4 }

// Sobreescribir propiedades
const defaults = { color: 'rojo', tamaño: 'M' };
const custom = { color: 'azul' };

const config = { ...defaults, ...custom };
console.log(config); // { color: 'azul', tamaño: 'M' }

// Orden importa
const config2 = { ...custom, ...defaults };
console.log(config2); // { color: 'rojo', tamaño: 'M' }
```

### Agregar/Actualizar Propiedades

**💡 Ejemplos:**
```javascript
const usuario = {
  nombre: 'Ana',
  edad: 25
};

// Agregar propiedad
const conEmail = {
  ...usuario,
  email: 'ana@example.com'
};

// Actualizar propiedad
const actualizado = {
  ...usuario,
  edad: 26
};

// Actualizar múltiples
const multipleActualizacion = {
  ...usuario,
  edad: 26,
  ciudad: 'Madrid',
  activo: true
};

console.log(multipleActualizacion);
// { nombre: 'Ana', edad: 26, ciudad: 'Madrid', activo: true }
```

### Actualizar Objetos Anidados

**💡 Ejemplos:**
```javascript
const estado = {
  usuario: {
    nombre: 'Ana',
    perfil: {
      avatar: 'avatar1.jpg',
      bio: 'Desarrolladora'
    }
  },
  configuracion: {
    tema: 'claro'
  }
};

// Actualizar propiedad anidada
const nuevoEstado = {
  ...estado,
  usuario: {
    ...estado.usuario,
    perfil: {
      ...estado.usuario.perfil,
      avatar: 'avatar2.jpg'
    }
  }
};

console.log(nuevoEstado.usuario.perfil.avatar); // 'avatar2.jpg'
console.log(estado.usuario.perfil.avatar);      // 'avatar1.jpg' (sin cambios)
```

---

## Rest Operator (...)

**📝 Descripción:** El operador rest (`...`) agrupa elementos restantes en un array o objeto.

### Rest en Parámetros de Función

**💡 Ejemplos:**
```javascript
// Función con cantidad variable de argumentos
function sumar(...numeros) {
  return numeros.reduce((total, num) => total + num, 0);
}

console.log(sumar(1, 2, 3));       // 6
console.log(sumar(1, 2, 3, 4, 5)); // 15

// Combinar con parámetros normales
function crearMensaje(titulo, ...puntos) {
  let mensaje = `${titulo}:\n`;
  puntos.forEach((punto, i) => {
    mensaje += `${i + 1}. ${punto}\n`;
  });
  return mensaje;
}

console.log(crearMensaje('Tareas', 'Estudiar', 'Ejercicio', 'Compras'));
// Tareas:
// 1. Estudiar
// 2. Ejercicio
// 3. Compras

// ⚠️ Rest debe ser el último parámetro
function ejemplo(a, b, ...resto, c) {} // Error!
function ejemplo(a, b, ...resto) {}    // ✅ Correcto
```

### Rest en Desestructuración de Arrays

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Extraer primeros y agrupar resto
const [primero, segundo, ...resto] = numeros;
console.log(primero);  // 1
console.log(segundo);  // 2
console.log(resto);    // [3, 4, 5, 6, 7, 8, 9, 10]

// Ignorar algunos y capturar resto
const [, , tercero, ...otros] = numeros;
console.log(tercero); // 3
console.log(otros);   // [4, 5, 6, 7, 8, 9, 10]

// Solo el resto
const [, ...sinPrimero] = numeros;
console.log(sinPrimero); // [2, 3, 4, 5, 6, 7, 8, 9, 10]
```

### Rest en Desestructuración de Objetos

**💡 Ejemplos:**
```javascript
const usuario = {
  id: 1,
  nombre: 'Ana',
  edad: 25,
  email: 'ana@example.com',
  ciudad: 'Madrid',
  pais: 'España'
};

// Extraer algunas propiedades y agrupar el resto
const { id, nombre, ...resto } = usuario;
console.log(id);     // 1
console.log(nombre); // 'Ana'
console.log(resto);  // { edad: 25, email: '...', ciudad: '...', pais: '...' }

// Útil para remover propiedades
const { password, ...usuarioSinPassword } = {
  id: 1,
  nombre: 'Juan',
  email: 'juan@example.com',
  password: 'secreto123'
};

console.log(usuarioSinPassword);
// { id: 1, nombre: 'Juan', email: 'juan@example.com' }
```

---

## Casos de Uso Prácticos

### 1. Inmutabilidad (Estado en React/Redux)

**💡 Ejemplo:**
```javascript
// Estado inicial
let estado = {
  usuarios: [
    { id: 1, nombre: 'Ana' },
    { id: 2, nombre: 'Juan' }
  ],
  contador: 0
};

// Agregar usuario (inmutable)
estado = {
  ...estado,
  usuarios: [...estado.usuarios, { id: 3, nombre: 'María' }]
};

// Actualizar contador
estado = {
  ...estado,
  contador: estado.contador + 1
};

// Actualizar usuario específico
estado = {
  ...estado,
  usuarios: estado.usuarios.map(u =>
    u.id === 2 ? { ...u, nombre: 'Juan Pérez' } : u
  )
};

// Eliminar usuario
estado = {
  ...estado,
  usuarios: estado.usuarios.filter(u => u.id !== 1)
};
```

### 2. Combinar Configuraciones

**💡 Ejemplo:**
```javascript
const configPorDefecto = {
  timeout: 5000,
  retries: 3,
  headers: {
    'Content-Type': 'application/json'
  },
  cache: true
};

function crearCliente(configUsuario = {}) {
  const config = {
    ...configPorDefecto,
    ...configUsuario,
    headers: {
      ...configPorDefecto.headers,
      ...(configUsuario.headers || {})
    }
  };
  
  return config;
}

const cliente = crearCliente({
  timeout: 10000,
  headers: {
    'Authorization': 'Bearer token123'
  }
});

console.log(cliente);
// {
//   timeout: 10000,
//   retries: 3,
//   headers: {
//     'Content-Type': 'application/json',
//     'Authorization': 'Bearer token123'
//   },
//   cache: true
// }
```

### 3. Clonar Arrays con Filtrado

**💡 Ejemplo:**
```javascript
const tareas = [
  { id: 1, titulo: 'Estudiar', completada: false },
  { id: 2, titulo: 'Ejercicio', completada: true },
  { id: 3, titulo: 'Compras', completada: false }
];

// Marcar como completada
function completarTarea(tareas, id) {
  return tareas.map(tarea =>
    tarea.id === id
      ? { ...tarea, completada: true }
      : tarea
  );
}

const tareasActualizadas = completarTarea(tareas, 1);
console.log(tareasActualizadas);

// Filtrar completadas
const pendientes = tareas.filter(t => !t.completada);
const copiaSegura = [...pendientes];
```

### 4. Función con Opciones Ilimitadas

**💡 Ejemplo:**
```javascript
function registrarEvento(tipo, ...detalles) {
  const timestamp = new Date().toISOString();
  
  console.log(`[${timestamp}] Evento: ${tipo}`);
  
  if (detalles.length > 0) {
    console.log('Detalles:', ...detalles);
  }
}

registrarEvento('LOGIN', 'usuario123', '192.168.1.1');
registrarEvento('ERROR', 'Database connection failed', 'timeout', 5000);
registrarEvento('INFO');
```

### 5. Merge de Arrays sin Duplicados

**💡 Ejemplo:**
```javascript
function mergeUnico(...arrays) {
  return [...new Set([...arrays.flat()])];
}

const arr1 = [1, 2, 3];
const arr2 = [2, 3, 4];
const arr3 = [3, 4, 5];

const unicos = mergeUnico(arr1, arr2, arr3);
console.log(unicos); // [1, 2, 3, 4, 5]

// Alternativa sin flat
function mergeUnicoAlt(...arrays) {
  const combinado = [];
  arrays.forEach(arr => combinado.push(...arr));
  return [...new Set(combinado)];
}
```

### 6. Props en Componentes (React-style)

**💡 Ejemplo:**
```javascript
function Boton({ texto, color, ...restoProps }) {
  return {
    elemento: 'button',
    texto,
    estilos: { backgroundColor: color },
    atributos: restoProps
  };
}

const boton = Boton({
  texto: 'Enviar',
  color: 'azul',
  onClick: () => console.log('Click'),
  disabled: false,
  id: 'btn-enviar'
});

console.log(boton);
// {
//   elemento: 'button',
//   texto: 'Enviar',
//   estilos: { backgroundColor: 'azul' },
//   atributos: { onClick: [Function], disabled: false, id: 'btn-enviar' }
// }
```

### 7. Dividir Arrays

**💡 Ejemplo:**
```javascript
function dividirArray(array, ...indices) {
  const partes = [];
  let inicio = 0;
  
  indices.forEach(indice => {
    partes.push(array.slice(inicio, indice));
    inicio = indice;
  });
  
  partes.push(array.slice(inicio));
  
  return partes;
}

const numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const dividido = dividirArray(numeros, 3, 6, 8);
console.log(dividido);
// [[1, 2, 3], [4, 5, 6], [7, 8], [9, 10]]
```

---

## Ejemplo Completo: Sistema de Carrito

```javascript
class Carrito {
  constructor() {
    this.items = [];
  }
  
  // Agregar items (rest operator)
  agregarMultiple(...productos) {
    this.items = [...this.items, ...productos];
  }
  
  // Actualizar item (spread operator)
  actualizarCantidad(id, cantidad) {
    this.items = this.items.map(item =>
      item.id === id
        ? { ...item, cantidad }
        : item
    );
  }
  
  // Remover items y retornar resto
  remover(id) {
    const [itemEliminado, ...resto] = this.items.filter(i => i.id === id);
    this.items = this.items.filter(i => i.id !== id);
    return itemEliminado;
  }
  
  // Obtener resumen (desestructuración con rest)
  obtenerResumen() {
    const [primero, segundo, ...resto] = this.items;
    
    return {
      primerItem: primero,
      segundoItem: segundo,
      cantidadRestante: resto.length,
      items: [...this.items] // Copia segura
    };
  }
  
  // Clonar carrito
  clonar() {
    const nuevoCarrito = new Carrito();
    nuevoCarrito.items = this.items.map(item => ({ ...item }));
    return nuevoCarrito;
  }
}

// Uso
const carrito = new Carrito();

carrito.agregarMultiple(
  { id: 1, nombre: 'Laptop', precio: 1200, cantidad: 1 },
  { id: 2, nombre: 'Mouse', precio: 25, cantidad: 2 },
  { id: 3, nombre: 'Teclado', precio: 80, cantidad: 1 }
);

console.log(carrito.obtenerResumen());

const carritoClonado = carrito.clonar();
carritoClonado.actualizarCantidad(1, 2);

console.log('Original:', carrito.items[0].cantidad);  // 1
console.log('Clonado:', carritoClonado.items[0].cantidad); // 2
```

---

## Diferencias Clave

```javascript
// SPREAD: Expande elementos
const arr = [1, 2, 3];
console.log(...arr); // 1 2 3 (elementos separados)
const copia = [...arr]; // [1, 2, 3] (nuevo array)

// REST: Agrupa elementos
function sumar(...numeros) {
  // numeros es un array
  return numeros.reduce((a, b) => a + b, 0);
}

// Contexto determina el significado
const [primero, ...resto] = [1, 2, 3]; // REST
const combinado = [...arr, 4, 5]; // SPREAD
```

---

## Buenas Prácticas

```javascript
// ✅ Bueno: Usar spread para copias inmutables
const nuevo = [...original, nuevoItem];

// ❌ Evitar: Mutar directamente
original.push(nuevoItem);

// ✅ Bueno: Rest para parámetros variables
function logear(...mensajes) {
  console.log(...mensajes);
}

// ✅ Bueno: Spread para merge de objetos
const config = { ...defaults, ...userConfig };

// ⚠️ Cuidado: Spread es shallow copy
const obj = { a: { b: 1 } };
const copia = { ...obj };
copia.a.b = 2; // Modifica el original!

// ✅ Mejor: Deep clone cuando sea necesario
const copiaP rofunda = JSON.parse(JSON.stringify(obj));
```
