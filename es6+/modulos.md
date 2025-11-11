# 📦 Módulos en JavaScript (ES6+)

## ¿Qué son los Módulos?

Los **módulos** permiten dividir el código en archivos separados, facilitando la organización, reutilización y mantenimiento del código.

---

## Export (Exportar)

### Named Exports (Exportaciones Nombradas)

**💡 Ejemplos:**

```javascript
// utils.js
export const PI = 3.14159;
export const E = 2.71828;

export function sumar(a, b) {
  return a + b;
}

export function restar(a, b) {
  return a - b;
}

export class Calculadora {
  sumar(a, b) {
    return a + b;
  }
}

// Exportar varias al final
const multiplicar = (a, b) => a * b;
const dividir = (a, b) => a / b;

export { multiplicar, dividir };

// Exportar con otro nombre
const potencia = (base, exponente) => base ** exponente;
export { potencia as power };
```

### Default Export (Exportación por Defecto)

**💡 Ejemplos:**

```javascript
// Usuario.js
export default class Usuario {
  constructor(nombre) {
    this.nombre = nombre;
  }
  
  saludar() {
    return `Hola, soy ${this.nombre}`;
  }
}

// config.js
export default {
  apiUrl: 'https://api.ejemplo.com',
  timeout: 5000,
  retries: 3
};

// helpers.js
export default function formatearFecha(fecha) {
  return fecha.toLocaleDateString('es-ES');
}

// Combinar default y named
export default class Producto {
  constructor(nombre, precio) {
    this.nombre = nombre;
    this.precio = precio;
  }
}

export const IVA = 0.21;
export function calcularPrecioFinal(precio) {
  return precio * (1 + IVA);
}
```

---

## Import (Importar)

### Importar Named Exports

**💡 Ejemplos:**

```javascript
// main.js
import { sumar, restar } from './utils.js';

console.log(sumar(5, 3));   // 8
console.log(restar(10, 4)); // 6

// Importar múltiples
import { PI, E, Calculadora } from './utils.js';

const calc = new Calculadora();
console.log(calc.sumar(2, 3)); // 5

// Importar todo con alias
import * as Utils from './utils.js';

console.log(Utils.PI); // 3.14159
console.log(Utils.sumar(1, 2)); // 3

// Importar con otro nombre
import { power as potencia } from './utils.js';
console.log(potencia(2, 3)); // 8

// Renombrar al importar
import { sumar as add, restar as subtract } from './utils.js';
console.log(add(5, 3)); // 8
```

### Importar Default Export

**💡 Ejemplos:**

```javascript
// Puede usar cualquier nombre
import Usuario from './Usuario.js';
import Config from './config.js';
import formatearFecha from './helpers.js';

const usuario = new Usuario('Ana');
console.log(usuario.saludar());

console.log(Config.apiUrl);
console.log(formatearFecha(new Date()));

// Combinar default y named
import Producto, { IVA, calcularPrecioFinal } from './Producto.js';

const producto = new Producto('Laptop', 1000);
console.log(calcularPrecioFinal(producto.precio));
```

---

## Estructura de Proyecto Típica

```
proyecto/
├── index.html
├── src/
│   ├── main.js
│   ├── config/
│   │   └── config.js
│   ├── utils/
│   │   ├── math.js
│   │   ├── string.js
│   │   └── date.js
│   ├── models/
│   │   ├── Usuario.js
│   │   └── Producto.js
│   ├── services/
│   │   ├── api.js
│   │   └── auth.js
│   └── components/
│       ├── Header.js
│       └── Footer.js
```

---

## Ejemplos Completos

### 1. Sistema de Utilidades Matemáticas

**math.js:**
```javascript
// math.js
export const PI = 3.14159;

export function sumar(...numeros) {
  return numeros.reduce((a, b) => a + b, 0);
}

export function restar(a, b) {
  return a - b;
}

export function multiplicar(...numeros) {
  return numeros.reduce((a, b) => a * b, 1);
}

export function dividir(a, b) {
  if (b === 0) throw new Error('División por cero');
  return a / b;
}

export function promedio(numeros) {
  return sumar(...numeros) / numeros.length;
}

export function maximo(numeros) {
  return Math.max(...numeros);
}

export function minimo(numeros) {
  return Math.min(...numeros);
}

export default {
  sumar,
  restar,
  multiplicar,
  dividir,
  promedio,
  maximo,
  minimo
};
```

**main.js:**
```javascript
// Importar funciones específicas
import { sumar, promedio, maximo } from './math.js';

console.log(sumar(1, 2, 3, 4, 5)); // 15
console.log(promedio([10, 20, 30])); // 20
console.log(maximo([5, 2, 9, 1])); // 9

// O importar todo
import * as Math from './math.js';

console.log(Math.PI); // 3.14159
console.log(Math.multiplicar(2, 3, 4)); // 24
```

---

### 2. Modelo de Usuario

**Usuario.js:**
```javascript
// Usuario.js
export default class Usuario {
  constructor(nombre, email) {
    this.id = Usuario.generarId();
    this.nombre = nombre;
    this.email = email;
    this.fechaCreacion = new Date();
  }
  
  static generarId() {
    return Date.now() + '_' + Math.random().toString(36).substr(2, 9);
  }
  
  static validarEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }
  
  obtenerInfo() {
    return {
      id: this.id,
      nombre: this.nombre,
      email: this.email
    };
  }
  
  toJSON() {
    return JSON.stringify(this.obtenerInfo());
  }
}

// Named exports adicionales
export const ROLES = {
  ADMIN: 'admin',
  USER: 'user',
  GUEST: 'guest'
};

export function crearUsuario(datos) {
  if (!Usuario.validarEmail(datos.email)) {
    throw new Error('Email inválido');
  }
  return new Usuario(datos.nombre, datos.email);
}
```

**main.js:**
```javascript
import Usuario, { ROLES, crearUsuario } from './Usuario.js';

const usuario = new Usuario('Ana', 'ana@example.com');
console.log(usuario.obtenerInfo());

const nuevoUsuario = crearUsuario({
  nombre: 'Juan',
  email: 'juan@example.com'
});

console.log(ROLES.ADMIN); // 'admin'
```

---

### 3. Servicio de API

**api.js:**
```javascript
// api.js
class ApiService {
  constructor(baseURL) {
    this.baseURL = baseURL;
    this.headers = {
      'Content-Type': 'application/json'
    };
  }
  
  setToken(token) {
    this.headers['Authorization'] = `Bearer ${token}`;
  }
  
  async get(endpoint) {
    const response = await fetch(`${this.baseURL}${endpoint}`, {
      method: 'GET',
      headers: this.headers
    });
    return response.json();
  }
  
  async post(endpoint, data) {
    const response = await fetch(`${this.baseURL}${endpoint}`, {
      method: 'POST',
      headers: this.headers,
      body: JSON.stringify(data)
    });
    return response.json();
  }
  
  async put(endpoint, data) {
    const response = await fetch(`${this.baseURL}${endpoint}`, {
      method: 'PUT',
      headers: this.headers,
      body: JSON.stringify(data)
    });
    return response.json();
  }
  
  async delete(endpoint) {
    const response = await fetch(`${this.baseURL}${endpoint}`, {
      method: 'DELETE',
      headers: this.headers
    });
    return response.json();
  }
}

// Default export
export default ApiService;

// Named exports
export const API_URL = 'https://api.ejemplo.com';

export function crearApi(baseURL = API_URL) {
  return new ApiService(baseURL);
}
```

**usuarios-service.js:**
```javascript
import ApiService, { API_URL } from './api.js';

class UsuariosService {
  constructor() {
    this.api = new ApiService(API_URL);
  }
  
  async obtenerTodos() {
    return this.api.get('/usuarios');
  }
  
  async obtenerPorId(id) {
    return this.api.get(`/usuarios/${id}`);
  }
  
  async crear(datos) {
    return this.api.post('/usuarios', datos);
  }
  
  async actualizar(id, datos) {
    return this.api.put(`/usuarios/${id}`, datos);
  }
  
  async eliminar(id) {
    return this.api.delete(`/usuarios/${id}`);
  }
}

export default new UsuariosService();
```

**main.js:**
```javascript
import usuariosService from './usuarios-service.js';

// Usar el servicio
async function cargarUsuarios() {
  try {
    const usuarios = await usuariosService.obtenerTodos();
    console.log('Usuarios:', usuarios);
  } catch (error) {
    console.error('Error:', error);
  }
}

cargarUsuarios();
```

---

### 4. Sistema de Configuración

**config/desarrollo.js:**
```javascript
export default {
  apiUrl: 'http://localhost:3000',
  debug: true,
  timeout: 10000
};
```

**config/produccion.js:**
```javascript
export default {
  apiUrl: 'https://api.produccion.com',
  debug: false,
  timeout: 5000
};
```

**config/index.js:**
```javascript
import desarrollo from './desarrollo.js';
import produccion from './produccion.js';

const ENV = process.env.NODE_ENV || 'desarrollo';

const configs = {
  desarrollo,
  produccion
};

export default configs[ENV];

export const esProduccion = ENV === 'produccion';
export const esDesarrollo = ENV === 'desarrollo';
```

**main.js:**
```javascript
import config, { esDesarrollo } from './config/index.js';

console.log('API URL:', config.apiUrl);
console.log('Debug:', config.debug);

if (esDesarrollo) {
  console.log('Modo desarrollo activado');
}
```

---

### 5. Componentes Reutilizables

**components/Boton.js:**
```javascript
export default class Boton {
  constructor({ texto, color, onClick }) {
    this.texto = texto;
    this.color = color;
    this.onClick = onClick;
  }
  
  render() {
    const button = document.createElement('button');
    button.textContent = this.texto;
    button.style.backgroundColor = this.color;
    button.addEventListener('click', this.onClick);
    return button;
  }
}

export const colores = {
  PRIMARIO: '#007bff',
  SECUNDARIO: '#6c757d',
  EXITO: '#28a745',
  PELIGRO: '#dc3545'
};
```

**components/Formulario.js:**
```javascript
export default class Formulario {
  constructor({ campos, onSubmit }) {
    this.campos = campos;
    this.onSubmit = onSubmit;
  }
  
  render() {
    const form = document.createElement('form');
    
    this.campos.forEach(campo => {
      const input = document.createElement('input');
      input.name = campo.nombre;
      input.type = campo.tipo;
      input.placeholder = campo.placeholder;
      form.appendChild(input);
    });
    
    form.addEventListener('submit', (e) => {
      e.preventDefault();
      const datos = new FormData(form);
      this.onSubmit(Object.fromEntries(datos));
    });
    
    return form;
  }
}

export function validarFormulario(datos, reglas) {
  const errores = [];
  
  Object.keys(reglas).forEach(campo => {
    const valor = datos[campo];
    const regla = reglas[campo];
    
    if (regla.requerido && !valor) {
      errores.push(`${campo} es requerido`);
    }
    
    if (regla.min && valor.length < regla.min) {
      errores.push(`${campo} debe tener mínimo ${regla.min} caracteres`);
    }
  });
  
  return errores;
}
```

**main.js:**
```javascript
import Boton, { colores } from './components/Boton.js';
import Formulario, { validarFormulario } from './components/Formulario.js';

// Crear botón
const boton = new Boton({
  texto: 'Enviar',
  color: colores.PRIMARIO,
  onClick: () => console.log('Click!')
});

document.body.appendChild(boton.render());

// Crear formulario
const form = new Formulario({
  campos: [
    { nombre: 'nombre', tipo: 'text', placeholder: 'Nombre' },
    { nombre: 'email', tipo: 'email', placeholder: 'Email' }
  ],
  onSubmit: (datos) => {
    const errores = validarFormulario(datos, {
      nombre: { requerido: true, min: 3 },
      email: { requerido: true }
    });
    
    if (errores.length > 0) {
      console.error('Errores:', errores);
    } else {
      console.log('Datos válidos:', datos);
    }
  }
});

document.body.appendChild(form.render());
```

---

## Usar Módulos en HTML

**index.html:**
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Mi App</title>
</head>
<body>
  <div id="app"></div>
  
  <!-- Importante: type="module" -->
  <script type="module" src="./src/main.js"></script>
</body>
</html>
```

---

## Dynamic Imports (Importación Dinámica)

**💡 Ejemplos:**

```javascript
// Importar solo cuando se necesita
async function cargarModulo() {
  const { sumar } = await import('./math.js');
  console.log(sumar(1, 2, 3));
}

// Cargar condicionalmente
if (usuario.esAdmin) {
  const AdminPanel = await import('./AdminPanel.js');
  new AdminPanel.default().render();
}

// Lazy loading
button.addEventListener('click', async () => {
  const modulo = await import('./funcionalidad-pesada.js');
  modulo.ejecutar();
});

// Con manejo de errores
async function cargarComponente() {
  try {
    const { Componente } = await import('./Componente.js');
    new Componente().render();
  } catch (error) {
    console.error('Error al cargar:', error);
  }
}
```

---

## Re-exportar (Re-export)

**💡 Ejemplos:**

```javascript
// utils/index.js - Punto de entrada único
export { sumar, restar } from './math.js';
export { mayusculas, minusculas } from './string.js';
export { formatearFecha } from './date.js';

// O todo de una vez
export * from './math.js';
export * from './string.js';
export * from './date.js';

// main.js - Importar todo desde un lugar
import { sumar, mayusculas, formatearFecha } from './utils/index.js';
```

---

## Buenas Prácticas

```javascript
// ✅ Bueno: Un archivo, una responsabilidad
// Usuario.js - Solo la clase Usuario
// api.js - Solo lógica de API

// ✅ Bueno: Named exports para múltiples funciones
export function funcion1() {}
export function funcion2() {}

// ✅ Bueno: Default export para el principal
export default class MiClase {}

// ✅ Bueno: Importar solo lo necesario
import { sumar, restar } from './math.js';

// ❌ Evitar: Importar todo si solo necesitas una función
import * as Math from './math.js'; // Solo para usar Math.sumar()

// ✅ Bueno: Nombres descriptivos
import { calcularTotal } from './carrito.js';

// ❌ Evitar: Nombres genéricos
import { calc } from './utils.js';

// ✅ Bueno: Agrupar imports relacionados
import { API_URL, ApiService } from './api.js';
import Usuario from './Usuario.js';
import Producto from './Producto.js';
```

---

## Resumen

**Ventajas de los Módulos:**
- ✅ Código organizado y mantenible
- ✅ Reutilización de código
- ✅ Evita conflictos de nombres
- ✅ Carga bajo demanda (lazy loading)
- ✅ Tree-shaking (eliminar código no usado)

**Tipos de Export:**
- `export` - Named export
- `export default` - Default export
- `export * from` - Re-export

**Tipos de Import:**
- `import { nombre } from` - Named import
- `import nombre from` - Default import
- `import * as nombre from` - Import todo
- `import()` - Dynamic import
