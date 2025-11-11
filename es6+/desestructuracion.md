# 📦 Desestructuración en JavaScript

## ¿Qué es la Desestructuración?

La **desestructuración** es una sintaxis que permite extraer valores de arrays u objetos y asignarlos a variables de forma rápida y elegante (ES6+).

---

## Desestructuración de Objetos

### Sintaxis Básica

**💡 Ejemplos:**
```javascript
// Sin desestructuración
const usuario = {
  nombre: 'Ana',
  edad: 25,
  email: 'ana@example.com'
};

const nombre = usuario.nombre;
const edad = usuario.edad;
const email = usuario.email;

// ✅ Con desestructuración
const { nombre, edad, email } = usuario;
console.log(nombre); // 'Ana'
console.log(edad);   // 25
console.log(email);  // 'ana@example.com'

// Extraer solo algunas propiedades
const { nombre, edad } = usuario;

// Con nombres diferentes
const { nombre: nombreUsuario, edad: añosUsuario } = usuario;
console.log(nombreUsuario); // 'Ana'
console.log(añosUsuario);   // 25
```

### Valores por Defecto

**💡 Ejemplos:**
```javascript
const usuario = {
  nombre: 'Juan',
  edad: 30
};

// Con valores por defecto
const { nombre, edad, ciudad = 'Madrid' } = usuario;
console.log(ciudad); // 'Madrid' (valor por defecto)

// Combinar renombrado y valores por defecto
const { nombre: name, pais: country = 'España' } = usuario;
console.log(name);    // 'Juan'
console.log(country); // 'España'
```

### Desestructuración Anidada

**💡 Ejemplos:**
```javascript
const usuario = {
  nombre: 'Ana',
  direccion: {
    calle: 'Principal',
    ciudad: 'Madrid',
    codigo: '28001'
  },
  contacto: {
    email: 'ana@example.com',
    telefono: '123456789'
  }
};

// Desestructuración anidada
const {
  nombre,
  direccion: { ciudad, codigo },
  contacto: { email }
} = usuario;

console.log(nombre); // 'Ana'
console.log(ciudad); // 'Madrid'
console.log(codigo); // '28001'
console.log(email);  // 'ana@example.com'

// ⚠️ direccion y contacto no son variables
// console.log(direccion); // Error!

// Para tener ambos
const {
  direccion,
  direccion: { ciudad: ciudadUsuario }
} = usuario;
console.log(direccion);       // Objeto completo
console.log(ciudadUsuario);   // 'Madrid'
```

### Rest en Objetos

**💡 Ejemplos:**
```javascript
const usuario = {
  id: 1,
  nombre: 'Ana',
  edad: 25,
  email: 'ana@example.com',
  ciudad: 'Madrid'
};

// Extraer algunas y agrupar el resto
const { id, nombre, ...restoInfo } = usuario;

console.log(id);        // 1
console.log(nombre);    // 'Ana'
console.log(restoInfo); // { edad: 25, email: '...', ciudad: '...' }

// Útil para remover propiedades
const { password, ...usuarioSinPassword } = {
  nombre: 'Juan',
  email: 'juan@example.com',
  password: '12345'
};
console.log(usuarioSinPassword); // { nombre: 'Juan', email: '...' }
```

---

## Desestructuración de Arrays

### Sintaxis Básica

**💡 Ejemplos:**
```javascript
// Sin desestructuración
const colores = ['rojo', 'verde', 'azul'];
const primero = colores[0];
const segundo = colores[1];
const tercero = colores[2];

// ✅ Con desestructuración
const [primero, segundo, tercero] = colores;
console.log(primero);  // 'rojo'
console.log(segundo);  // 'verde'
console.log(tercero);  // 'azul'

// Saltar elementos
const [, , tercerColor] = colores;
console.log(tercerColor); // 'azul'

// Solo primeros elementos
const [primer, segundo] = colores;
console.log(primer);  // 'rojo'
console.log(segundo); // 'verde'
```

### Valores por Defecto

**💡 Ejemplos:**
```javascript
const numeros = [1, 2];

const [a, b, c = 0, d = 0] = numeros;
console.log(a); // 1
console.log(b); // 2
console.log(c); // 0 (valor por defecto)
console.log(d); // 0 (valor por defecto)

// Con undefined
const valores = [1, undefined, 3];
const [x, y = 10, z] = valores;
console.log(y); // 10 (usa el valor por defecto porque es undefined)
```

### Rest en Arrays

**💡 Ejemplos:**
```javascript
const numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const [primero, segundo, ...resto] = numeros;
console.log(primero);  // 1
console.log(segundo);  // 2
console.log(resto);    // [3, 4, 5, 6, 7, 8, 9, 10]

// Omitir elementos y capturar resto
const [, , tercero, ...otros] = numeros;
console.log(tercero); // 3
console.log(otros);   // [4, 5, 6, 7, 8, 9, 10]
```

### Intercambiar Variables

**💡 Ejemplos:**
```javascript
let a = 1;
let b = 2;

// Intercambiar valores
[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1

// Útil para algoritmos de ordenamiento
let x = 10, y = 20, z = 30;
[x, y, z] = [z, x, y]; // Rotar
console.log(x, y, z); // 30, 10, 20
```

---

## Desestructuración en Parámetros de Funciones

### Con Objetos

**💡 Ejemplos:**
```javascript
// Sin desestructuración
function crearUsuario(config) {
  const nombre = config.nombre;
  const edad = config.edad;
  const rol = config.rol || 'usuario';
  // ...
}

// ✅ Con desestructuración
function crearUsuario({ nombre, edad, rol = 'usuario' }) {
  console.log(`Usuario: ${nombre}, Edad: ${edad}, Rol: ${rol}`);
}

crearUsuario({ nombre: 'Ana', edad: 25 });
// Usuario: Ana, Edad: 25, Rol: usuario

crearUsuario({ nombre: 'Juan', edad: 30, rol: 'admin' });
// Usuario: Juan, Edad: 30, Rol: admin

// Ejemplo práctico: Configuración de componente
function crearBoton({
  texto = 'Click',
  color = 'azul',
  tamaño = 'mediano',
  onClick = () => {}
}) {
  return {
    texto,
    color,
    tamaño,
    onClick
  };
}

const boton = crearBoton({ texto: 'Enviar', color: 'verde' });
console.log(boton);
```

### Con Arrays

**💡 Ejemplos:**
```javascript
function sumarPrimerosDos([a, b]) {
  return a + b;
}

console.log(sumarPrimerosDos([10, 20, 30])); // 30

// Con rest
function imprimirLista([primero, ...resto]) {
  console.log('Primero:', primero);
  console.log('Resto:', resto);
}

imprimirLista(['Tarea 1', 'Tarea 2', 'Tarea 3']);
// Primero: Tarea 1
// Resto: ['Tarea 2', 'Tarea 3']
```

---

## Casos de Uso Prácticos

### 1. API Responses

**💡 Ejemplo:**
```javascript
async function obtenerUsuario(id) {
  const response = await fetch(`/api/usuarios/${id}`);
  const { data: usuario, error, status } = await response.json();
  
  if (error) {
    console.error('Error:', error);
    return null;
  }
  
  const { nombre, email, perfil: { avatar, bio } } = usuario;
  
  return { nombre, email, avatar, bio };
}
```

### 2. Opciones de Configuración

**💡 Ejemplo:**
```javascript
function inicializarApp({
  apiUrl = 'https://api.default.com',
  timeout = 5000,
  headers = {},
  retries = 3,
  debug = false
} = {}) {
  console.log('API URL:', apiUrl);
  console.log('Timeout:', timeout);
  console.log('Debug:', debug);
  
  return {
    apiUrl,
    timeout,
    headers,
    retries,
    debug
  };
}

// Usar con opciones parciales
const app = inicializarApp({
  apiUrl: 'https://mi-api.com',
  debug: true
});

// Usar con valores por defecto
const appDefecto = inicializarApp();
```

### 3. Retornar Múltiples Valores

**💡 Ejemplo:**
```javascript
function calcularEstadisticas(numeros) {
  const suma = numeros.reduce((a, b) => a + b, 0);
  const promedio = suma / numeros.length;
  const max = Math.max(...numeros);
  const min = Math.min(...numeros);
  
  return { suma, promedio, max, min };
}

// Desestructurar el resultado
const { suma, promedio, max, min } = calcularEstadisticas([1, 2, 3, 4, 5]);

console.log('Suma:', suma);         // 15
console.log('Promedio:', promedio); // 3
console.log('Máximo:', max);        // 5
console.log('Mínimo:', min);        // 1

// Extraer solo lo que necesitas
const { promedio: avg } = calcularEstadisticas([10, 20, 30]);
console.log(avg); // 20
```

### 4. Iterar sobre Arrays de Objetos

**💡 Ejemplo:**
```javascript
const usuarios = [
  { id: 1, nombre: 'Ana', edad: 25, ciudad: 'Madrid' },
  { id: 2, nombre: 'Juan', edad: 30, ciudad: 'Barcelona' },
  { id: 3, nombre: 'María', edad: 28, ciudad: 'Valencia' }
];

// Desestructurar en forEach
usuarios.forEach(({ nombre, ciudad }) => {
  console.log(`${nombre} vive en ${ciudad}`);
});

// Desestructurar en map
const nombres = usuarios.map(({ nombre }) => nombre);
console.log(nombres); // ['Ana', 'Juan', 'María']

// Desestructurar en filter
const mayores = usuarios.filter(({ edad }) => edad > 26);
console.log(mayores);
```

### 5. Actualizar Estado (React-style)

**💡 Ejemplo:**
```javascript
const estadoInicial = {
  usuario: { nombre: 'Ana', edad: 25 },
  configuracion: { tema: 'claro', idioma: 'es' },
  datos: [1, 2, 3]
};

// Actualizar propiedad anidada
const nuevoEstado = {
  ...estadoInicial,
  usuario: {
    ...estadoInicial.usuario,
    edad: 26
  }
};

// Extraer y modificar
const { usuario, ...restoEstado } = estadoInicial;
const estadoActualizado = {
  ...restoEstado,
  usuario: { ...usuario, nombre: 'María' }
};
```

### 6. Destructuring con Regex Matches

**💡 Ejemplo:**
```javascript
const email = 'usuario@dominio.com';
const regex = /^([^@]+)@([^@]+)$/;
const match = email.match(regex);

if (match) {
  const [, usuario, dominio] = match;
  console.log('Usuario:', usuario);   // 'usuario'
  console.log('Dominio:', dominio);   // 'dominio.com'
}

// Extraer datos de URL
const url = 'https://api.ejemplo.com/usuarios/123';
const urlRegex = /\/usuarios\/(\d+)/;
const [, userId] = url.match(urlRegex) || [];
console.log('User ID:', userId); // '123'
```

### 7. Import/Export con Desestructuración

**💡 Ejemplo:**
```javascript
// utils.js
export const sumar = (a, b) => a + b;
export const restar = (a, b) => a - b;
export const multiplicar = (a, b) => a * b;

// main.js
import { sumar, restar } from './utils.js';

console.log(sumar(5, 3));   // 8
console.log(restar(10, 4)); // 6

// Renombrar al importar
import { sumar as add, restar as subtract } from './utils.js';

console.log(add(2, 3)); // 5
```

---

## Ejemplo Completo: Procesador de Formularios

```javascript
class FormularioProcesador {
  procesarEnvio({
    nombre,
    email,
    mensaje,
    opciones: {
      newsletter = false,
      terminos = false
    } = {},
    timestamp = Date.now()
  }) {
    // Validar campos requeridos
    if (!nombre || !email || !mensaje) {
      return {
        exito: false,
        error: 'Campos requeridos faltantes'
      };
    }
    
    // Validar términos
    if (!terminos) {
      return {
        exito: false,
        error: 'Debe aceptar los términos'
      };
    }
    
    // Procesar envío
    console.log(`Procesando formulario de ${nombre}`);
    
    return {
      exito: true,
      datos: { nombre, email, mensaje, newsletter },
      timestamp
    };
  }
  
  generarRespuesta({ exito, error, datos, timestamp }) {
    if (!exito) {
      return `Error: ${error}`;
    }
    
    const { nombre, newsletter } = datos;
    let mensaje = `Gracias ${nombre}!`;
    
    if (newsletter) {
      mensaje += ' Te has suscrito al newsletter.';
    }
    
    return mensaje;
  }
}

// Uso
const procesador = new FormularioProcesador();

const resultado = procesador.procesarEnvio({
  nombre: 'Ana García',
  email: 'ana@example.com',
  mensaje: 'Hola!',
  opciones: {
    newsletter: true,
    terminos: true
  }
});

console.log(procesador.generarRespuesta(resultado));
```

---

## Buenas Prácticas

```javascript
// ✅ Bueno: Desestructuración para mejorar legibilidad
const { nombre, edad } = usuario;

// ❌ Evitar: Acceso repetitivo
console.log(usuario.nombre);
console.log(usuario.edad);
console.log(usuario.email);

// ✅ Bueno: Valores por defecto
function configurar({ tema = 'claro', idioma = 'es' } = {}) {
  // ...
}

// ✅ Bueno: Renombrar para evitar conflictos
const { nombre: nombreUsuario } = usuario;
const { nombre: nombreProducto } = producto;

// ✅ Bueno: Rest para agrupar propiedades
const { id, ...datosActualizables } = usuario;

// ✅ Bueno: Intercambiar variables
[a, b] = [b, a];

// ❌ Evitar: Desestructuración muy anidada
const {
  a: {
    b: {
      c: {
        d: valor
      }
    }
  }
} = objeto; // Difícil de leer
```

---

## Resumen

**Ventajas:**
- ✅ Código más limpio y legible
- ✅ Menos código repetitivo
- ✅ Valores por defecto fáciles
- ✅ Ideal para parámetros de funciones
- ✅ Facilita trabajar con APIs

**Casos de Uso:**
- ✅ Extraer datos de objetos/arrays
- ✅ Parámetros de funciones
- ✅ Returns de funciones
- ✅ Imports/Exports
- ✅ Iterar sobre datos
