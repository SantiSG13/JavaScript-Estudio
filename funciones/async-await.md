# ⏳ Async/Await en JavaScript

## ¿Qué es Async/Await?

**Async/Await** es una sintaxis moderna (ES2017) que hace que el código asíncrono se vea y comporte más como código síncrono, haciéndolo más fácil de leer y escribir.

---

## async

**📝 Descripción:** La palabra clave `async` convierte una función en asíncrona. Siempre retorna una Promise.

**✍️ Sintaxis:**
```javascript
async function nombreFuncion() {
  // código
}
```

**💡 Ejemplos:**

```javascript
// Función async básica
async function saludar() {
  return 'Hola';
}

// Es equivalente a:
function saludar() {
  return Promise.resolve('Hola');
}

// Usar la función
saludar().then(mensaje => console.log(mensaje)); // 'Hola'

// Con arrow function
const obtenerNumero = async () => {
  return 42;
};

obtenerNumero().then(num => console.log(num)); // 42

// Retorna automáticamente una Promise
async function obtenerDatos() {
  return { nombre: 'Ana', edad: 25 };
}

console.log(obtenerDatos()); // Promise { { nombre: 'Ana', edad: 25 } }
```

---

## await

**📝 Descripción:** La palabra clave `await` pausa la ejecución de una función `async` hasta que la Promise se resuelva.

**⚠️ Solo se puede usar dentro de funciones `async`**

**💡 Ejemplos:**

```javascript
// Función que retorna una Promise
function esperar(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Usar await
async function ejemplo() {
  console.log('Inicio');
  await esperar(2000); // Espera 2 segundos
  console.log('Después de 2 segundos');
}

ejemplo();
// Salida:
// Inicio
// (espera 2 segundos)
// Después de 2 segundos

// Obtener el valor resuelto
async function obtenerDatos() {
  const resultado = await Promise.resolve('Datos cargados');
  console.log(resultado); // 'Datos cargados'
  return resultado;
}

obtenerDatos();
```

---

## Comparación: Callbacks vs Promises vs Async/Await

### Con Callbacks
```javascript
function obtenerUsuario(id, callback) {
  setTimeout(() => {
    callback({ id: id, nombre: 'Usuario ' + id });
  }, 1000);
}

obtenerUsuario(1, function(usuario) {
  console.log(usuario);
  obtenerUsuario(2, function(usuario2) {
    console.log(usuario2); // Callback hell 😵
  });
});
```

### Con Promises
```javascript
function obtenerUsuario(id) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve({ id: id, nombre: 'Usuario ' + id });
    }, 1000);
  });
}

obtenerUsuario(1)
  .then(usuario => {
    console.log(usuario);
    return obtenerUsuario(2);
  })
  .then(usuario2 => {
    console.log(usuario2);
  });
```

### Con Async/Await (Más limpio ✅)
```javascript
function obtenerUsuario(id) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve({ id: id, nombre: 'Usuario ' + id });
    }, 1000);
  });
}

async function obtenerUsuarios() {
  const usuario1 = await obtenerUsuario(1);
  console.log(usuario1);
  
  const usuario2 = await obtenerUsuario(2);
  console.log(usuario2);
}

obtenerUsuarios();
```

---

## Manejo de Errores con try/catch

**💡 Ejemplos:**

```javascript
// Promise que puede fallar
function obtenerDatos(exito) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (exito) {
        resolve('Datos obtenidos');
      } else {
        reject('Error al obtener datos');
      }
    }, 1000);
  });
}

// Manejo de errores con try/catch
async function cargarDatos() {
  try {
    const datos = await obtenerDatos(true);
    console.log(datos); // 'Datos obtenidos'
  } catch (error) {
    console.error('Error:', error);
  }
}

cargarDatos();

// Con error
async function cargarDatosError() {
  try {
    const datos = await obtenerDatos(false);
    console.log(datos);
  } catch (error) {
    console.error('Error:', error); // 'Error: Error al obtener datos'
  }
}

cargarDatosError();

// Try/catch con finally
async function procesarDatos() {
  try {
    console.log('Cargando...');
    const datos = await obtenerDatos(true);
    console.log(datos);
  } catch (error) {
    console.error('Error:', error);
  } finally {
    console.log('Proceso finalizado'); // Siempre se ejecuta
  }
}

procesarDatos();
```

---

## Fetch API con Async/Await

**💡 Ejemplos:**

```javascript
// GET request
async function obtenerUsuarios() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users');
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const usuarios = await response.json();
    console.log(usuarios);
    return usuarios;
  } catch (error) {
    console.error('Error al obtener usuarios:', error);
  }
}

obtenerUsuarios();

// POST request
async function crearUsuario(datos) {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(datos)
    });
    
    const usuario = await response.json();
    console.log('Usuario creado:', usuario);
    return usuario;
  } catch (error) {
    console.error('Error:', error);
  }
}

crearUsuario({ nombre: 'Ana', email: 'ana@example.com' });

// Múltiples requests en secuencia
async function obtenerDatosCompletos(userId) {
  try {
    // Obtener usuario
    const userResponse = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
    const usuario = await userResponse.json();
    
    // Obtener posts del usuario
    const postsResponse = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`);
    const posts = await postsResponse.json();
    
    return { usuario, posts };
  } catch (error) {
    console.error('Error:', error);
  }
}

obtenerDatosCompletos(1);
```

---

## Promise.all con Async/Await

**📝 Descripción:** Ejecutar múltiples Promises en paralelo.

**💡 Ejemplos:**

```javascript
// Funciones que retornan Promises
function obtenerUsuario(id) {
  return fetch(`https://jsonplaceholder.typicode.com/users/${id}`)
    .then(res => res.json());
}

function obtenerPosts(userId) {
  return fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`)
    .then(res => res.json());
}

function obtenerComentarios(postId) {
  return fetch(`https://jsonplaceholder.typicode.com/comments?postId=${postId}`)
    .then(res => res.json());
}

// Ejecutar en paralelo (más rápido)
async function obtenerTodoDatos() {
  try {
    const [usuario, posts, comentarios] = await Promise.all([
      obtenerUsuario(1),
      obtenerPosts(1),
      obtenerComentarios(1)
    ]);
    
    console.log('Usuario:', usuario);
    console.log('Posts:', posts);
    console.log('Comentarios:', comentarios);
  } catch (error) {
    console.error('Error:', error);
  }
}

obtenerTodoDatos();

// Comparación: Secuencial vs Paralelo
// ❌ Secuencial (más lento - 3 segundos total)
async function secuencial() {
  const a = await esperar(1000); // 1 seg
  const b = await esperar(1000); // 1 seg
  const c = await esperar(1000); // 1 seg
  // Total: 3 segundos
}

// ✅ Paralelo (más rápido - 1 segundo total)
async function paralelo() {
  const [a, b, c] = await Promise.all([
    esperar(1000),
    esperar(1000),
    esperar(1000)
  ]);
  // Total: 1 segundo (se ejecutan al mismo tiempo)
}
```

---

## Promise.race con Async/Await

**📝 Descripción:** Retorna la primera Promise que se resuelva o rechace.

**💡 Ejemplos:**

```javascript
function obtenerDatos(nombre, delay) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(`Datos de ${nombre}`);
    }, delay);
  });
}

async function primeraRespuesta() {
  try {
    const resultado = await Promise.race([
      obtenerDatos('API 1', 2000),
      obtenerDatos('API 2', 1000), // Esta gana
      obtenerDatos('API 3', 3000)
    ]);
    
    console.log(resultado); // 'Datos de API 2'
  } catch (error) {
    console.error('Error:', error);
  }
}

primeraRespuesta();

// Timeout personalizado
function timeout(ms) {
  return new Promise((_, reject) => {
    setTimeout(() => reject('Timeout!'), ms);
  });
}

async function conTimeout() {
  try {
    const resultado = await Promise.race([
      fetch('https://api.ejemplo.com/datos'),
      timeout(5000) // Si tarda más de 5 seg, falla
    ]);
    
    const datos = await resultado.json();
    console.log(datos);
  } catch (error) {
    console.error('Error o timeout:', error);
  }
}
```

---

## Ejemplos Prácticos

### 1. Login con Validación
```javascript
async function login(username, password) {
  try {
    // Mostrar loading
    console.log('Iniciando sesión...');
    
    // Validar credenciales
    const response = await fetch('https://api.ejemplo.com/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ username, password })
    });
    
    if (!response.ok) {
      throw new Error('Credenciales incorrectas');
    }
    
    const datos = await response.json();
    
    // Guardar token
    localStorage.setItem('token', datos.token);
    
    console.log('Login exitoso!');
    return datos;
  } catch (error) {
    console.error('Error en login:', error.message);
    throw error;
  }
}

// Usar
login('admin', '1234')
  .then(datos => console.log('Usuario:', datos))
  .catch(error => console.error('Fallo:', error));
```

### 2. Carga de Múltiples Recursos
```javascript
async function cargarRecursos() {
  const loadingEl = document.getElementById('loading');
  
  try {
    loadingEl.textContent = 'Cargando recursos...';
    
    const [usuarios, productos, configuracion] = await Promise.all([
      fetch('/api/usuarios').then(r => r.json()),
      fetch('/api/productos').then(r => r.json()),
      fetch('/api/config').then(r => r.json())
    ]);
    
    // Procesar datos
    console.log('Usuarios:', usuarios.length);
    console.log('Productos:', productos.length);
    console.log('Configuración:', configuracion);
    
    loadingEl.textContent = 'Recursos cargados!';
    
    return { usuarios, productos, configuracion };
  } catch (error) {
    loadingEl.textContent = 'Error al cargar recursos';
    console.error('Error:', error);
  }
}

cargarRecursos();
```

### 3. Procesamiento en Lote
```javascript
async function procesarLote(items) {
  const resultados = [];
  
  for (const item of items) {
    try {
      console.log(`Procesando ${item}...`);
      const resultado = await fetch(`/api/procesar/${item}`);
      const datos = await resultado.json();
      resultados.push(datos);
    } catch (error) {
      console.error(`Error procesando ${item}:`, error);
      resultados.push({ error: error.message });
    }
  }
  
  return resultados;
}

// Usar
const items = [1, 2, 3, 4, 5];
procesarLote(items).then(r => console.log('Resultados:', r));

// Procesar en paralelo (más rápido)
async function procesarLoteParalelo(items) {
  const promesas = items.map(item =>
    fetch(`/api/procesar/${item}`)
      .then(r => r.json())
      .catch(error => ({ error: error.message }))
  );
  
  return await Promise.all(promesas);
}

procesarLoteParalelo(items).then(r => console.log('Resultados:', r));
```

### 4. Retry Logic (Reintentos)
```javascript
async function fetchConReintentos(url, opciones = {}, intentos = 3) {
  for (let i = 0; i < intentos; i++) {
    try {
      console.log(`Intento ${i + 1} de ${intentos}...`);
      const response = await fetch(url, opciones);
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      
      return await response.json();
    } catch (error) {
      console.error(`Intento ${i + 1} falló:`, error.message);
      
      if (i === intentos - 1) {
        throw error; // Último intento, lanzar error
      }
      
      // Esperar antes del siguiente intento
      await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
    }
  }
}

// Usar
fetchConReintentos('https://api.ejemplo.com/datos')
  .then(datos => console.log('Datos:', datos))
  .catch(error => console.error('Todos los intentos fallaron:', error));
```

### 5. Sistema de Caché
```javascript
class CacheAPI {
  constructor() {
    this.cache = new Map();
    this.tiempoExpiracion = 60000; // 1 minuto
  }
  
  async obtener(url) {
    // Verificar caché
    const cached = this.cache.get(url);
    if (cached && Date.now() - cached.timestamp < this.tiempoExpiracion) {
      console.log('Retornando desde caché');
      return cached.data;
    }
    
    // Obtener datos nuevos
    console.log('Obteniendo datos frescos...');
    try {
      const response = await fetch(url);
      const data = await response.json();
      
      // Guardar en caché
      this.cache.set(url, {
        data,
        timestamp: Date.now()
      });
      
      return data;
    } catch (error) {
      console.error('Error:', error);
      
      // Retornar caché antigua si existe
      if (cached) {
        console.log('Retornando caché antigua por error');
        return cached.data;
      }
      
      throw error;
    }
  }
  
  limpiar() {
    this.cache.clear();
  }
}

// Usar
const api = new CacheAPI();

async function obtenerUsuarios() {
  const usuarios = await api.obtener('https://jsonplaceholder.typicode.com/users');
  console.log('Usuarios:', usuarios);
}

obtenerUsuarios(); // Obtiene de la API
obtenerUsuarios(); // Obtiene del caché (instantáneo)
```

---

## Patrones Avanzados

### Async IIFE (Immediately Invoked Function Expression)
```javascript
// Ejecutar código async al nivel superior
(async () => {
  const datos = await fetch('/api/datos').then(r => r.json());
  console.log(datos);
})();

// O con try/catch
(async () => {
  try {
    const datos = await fetch('/api/datos').then(r => r.json());
    console.log(datos);
  } catch (error) {
    console.error('Error:', error);
  }
})();
```

### Async en Clases
```javascript
class API {
  constructor(baseURL) {
    this.baseURL = baseURL;
  }
  
  async get(endpoint) {
    const response = await fetch(`${this.baseURL}${endpoint}`);
    return await response.json();
  }
  
  async post(endpoint, data) {
    const response = await fetch(`${this.baseURL}${endpoint}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    return await response.json();
  }
}

// Usar
const api = new API('https://jsonplaceholder.typicode.com');

(async () => {
  const usuarios = await api.get('/users');
  console.log(usuarios);
})();
```

---

## Buenas Prácticas

```javascript
// ✅ Bueno: Usar try/catch
async function obtenerDatos() {
  try {
    const response = await fetch('/api/datos');
    return await response.json();
  } catch (error) {
    console.error('Error:', error);
  }
}

// ❌ Malo: No manejar errores
async function obtenerDatos() {
  const response = await fetch('/api/datos'); // Puede fallar
  return await response.json();
}

// ✅ Bueno: Operaciones en paralelo cuando sea posible
const [usuarios, posts] = await Promise.all([
  obtenerUsuarios(),
  obtenerPosts()
]);

// ❌ Malo: Secuencial innecesario
const usuarios = await obtenerUsuarios(); // Espera
const posts = await obtenerPosts();       // Espera

// ✅ Bueno: Validar respuestas
async function obtenerDatos() {
  const response = await fetch('/api/datos');
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  return await response.json();
}
```

---

## Resumen

**Ventajas de Async/Await:**
- ✅ Código más limpio y legible
- ✅ Manejo de errores con try/catch
- ✅ Debugging más fácil
- ✅ Se parece a código síncrono

**Cuándo usar:**
- ✅ Operaciones asíncronas complejas
- ✅ Múltiples requests encadenados
- ✅ Fetch API
- ✅ Cualquier operación con Promises

**Recuerda:**
- ⚠️ `await` solo funciona en funciones `async`
- ⚠️ Siempre manejar errores con try/catch
- ⚠️ Usar Promise.all para operaciones en paralelo
