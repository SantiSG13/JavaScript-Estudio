# 🎁 Métodos de Objetos en JavaScript

## Crear y Acceder a Objetos

### Object Literal
```javascript
// Crear objeto
const persona = {
  nombre: 'Ana',
  edad: 25,
  ciudad: 'Madrid'
};

// Acceder a propiedades
console.log(persona.nombre); // 'Ana'
console.log(persona['edad']); // 25

// Agregar propiedades
persona.profesion = 'Ingeniera';
persona['pais'] = 'España';

// Modificar
persona.edad = 26;

// Eliminar
delete persona.ciudad;
```

---

## Object.keys()

**📝 Descripción:** Retorna un array con las claves (keys) del objeto.

**📤 Retorna:** Array de strings

**💡 Ejemplos:**
```javascript
const usuario = {
  nombre: 'Juan',
  edad: 30,
  email: 'juan@example.com'
};

const claves = Object.keys(usuario);
console.log(claves); // ['nombre', 'edad', 'email']

// Iterar sobre las claves
Object.keys(usuario).forEach(clave => {
  console.log(`${clave}: ${usuario[clave]}`);
});

// Contar propiedades
console.log(Object.keys(usuario).length); // 3

// Verificar si está vacío
const vacio = {};
console.log(Object.keys(vacio).length === 0); // true
```

---

## Object.values()

**📝 Descripción:** Retorna un array con los valores del objeto.

**📤 Retorna:** Array de valores

**💡 Ejemplos:**
```javascript
const producto = {
  nombre: 'Laptop',
  precio: 1200,
  stock: 5
};

const valores = Object.values(producto);
console.log(valores); // ['Laptop', 1200, 5]

// Sumar valores numéricos
const precios = {
  laptop: 1200,
  mouse: 25,
  teclado: 80
};

const total = Object.values(precios).reduce((sum, precio) => sum + precio, 0);
console.log(total); // 1305

// Verificar si todos son verdaderos
const permisos = {
  leer: true,
  escribir: true,
  eliminar: false
};

const tienePermiso = Object.values(permisos).every(v => v === true);
console.log(tienePermiso); // false
```

---

## Object.entries()

**📝 Descripción:** Retorna un array de pares [clave, valor].

**📤 Retorna:** Array de arrays [key, value]

**💡 Ejemplos:**
```javascript
const persona = {
  nombre: 'María',
  edad: 28,
  ciudad: 'Barcelona'
};

const entradas = Object.entries(persona);
console.log(entradas);
// [['nombre', 'María'], ['edad', 28], ['ciudad', 'Barcelona']]

// Iterar con destructuring
Object.entries(persona).forEach(([clave, valor]) => {
  console.log(`${clave}: ${valor}`);
});

// Convertir a Map
const mapa = new Map(Object.entries(persona));
console.log(mapa.get('nombre')); // 'María'

// Filtrar propiedades
const filtrado = Object.entries(persona)
  .filter(([clave, valor]) => typeof valor === 'string')
  .reduce((obj, [clave, valor]) => {
    obj[clave] = valor;
    return obj;
  }, {});
console.log(filtrado); // { nombre: 'María', ciudad: 'Barcelona' }
```

---

## Object.assign()

**📝 Descripción:** Copia propiedades de uno o más objetos fuente a un objeto destino.

**✍️ Sintaxis:**
```javascript
Object.assign(objetoDestino, objeto1, objeto2, ...)
```

**💡 Ejemplos:**
```javascript
// Copiar objeto
const original = { a: 1, b: 2 };
const copia = Object.assign({}, original);
console.log(copia); // { a: 1, b: 2 }

// Fusionar objetos
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const fusionado = Object.assign({}, obj1, obj2);
console.log(fusionado); // { a: 1, b: 2, c: 3, d: 4 }

// Sobreescribir propiedades
const defaults = { color: 'rojo', tamaño: 'mediano' };
const opciones = { color: 'azul' };
const config = Object.assign({}, defaults, opciones);
console.log(config); // { color: 'azul', tamaño: 'mediano' }

// Agregar propiedades
const usuario = { nombre: 'Ana' };
Object.assign(usuario, { edad: 25, ciudad: 'Madrid' });
console.log(usuario); // { nombre: 'Ana', edad: 25, ciudad: 'Madrid' }
```

---

## Object.freeze()

**📝 Descripción:** Congela un objeto, impidiendo modificaciones.

**💡 Ejemplos:**
```javascript
const config = {
  apiUrl: 'https://api.ejemplo.com',
  timeout: 5000
};

Object.freeze(config);

// Intentar modificar (en modo estricto daría error)
config.apiUrl = 'https://otra-api.com'; // No funciona
config.nuevaPropiedad = 'valor'; // No funciona
delete config.timeout; // No funciona

console.log(config); // Sin cambios

// Verificar si está congelado
console.log(Object.isFrozen(config)); // true

// ⚠️ Solo congela el primer nivel (shallow freeze)
const obj = {
  nombre: 'Ana',
  datos: { edad: 25 }
};

Object.freeze(obj);
obj.nombre = 'Juan'; // No funciona
obj.datos.edad = 30; // ✅ Sí funciona (objeto anidado)
console.log(obj.datos.edad); // 30
```

---

## Object.seal()

**📝 Descripción:** Sella un objeto, permitiendo modificar pero no agregar/eliminar propiedades.

**💡 Ejemplos:**
```javascript
const usuario = {
  nombre: 'Juan',
  edad: 30
};

Object.seal(usuario);

// Modificar (permitido)
usuario.nombre = 'Pedro'; // ✅ Funciona
usuario.edad = 31; // ✅ Funciona

// Agregar (no permitido)
usuario.email = 'juan@example.com'; // No funciona

// Eliminar (no permitido)
delete usuario.edad; // No funciona

console.log(usuario); // { nombre: 'Pedro', edad: 31 }

// Verificar
console.log(Object.isSealed(usuario)); // true
```

---

## Object.create()

**📝 Descripción:** Crea un nuevo objeto con el prototipo especificado.

**💡 Ejemplos:**
```javascript
// Crear objeto con prototipo específico
const personaProto = {
  saludar() {
    console.log(`Hola, soy ${this.nombre}`);
  }
};

const ana = Object.create(personaProto);
ana.nombre = 'Ana';
ana.edad = 25;

ana.saludar(); // 'Hola, soy Ana'

// Crear objeto sin prototipo
const obj = Object.create(null);
console.log(obj.toString); // undefined (no hereda de Object)

// Con propiedades iniciales
const juan = Object.create(personaProto, {
  nombre: { value: 'Juan', writable: true },
  edad: { value: 30, writable: true }
});

juan.saludar(); // 'Hola, soy Juan'
```

---

## Object.defineProperty()

**📝 Descripción:** Define o modifica una propiedad con descriptores específicos.

**💡 Ejemplos:**
```javascript
const persona = {};

// Definir propiedad con descriptores
Object.defineProperty(persona, 'nombre', {
  value: 'Ana',
  writable: true,      // Se puede modificar
  enumerable: true,    // Aparece en for...in y Object.keys()
  configurable: true   // Se puede eliminar o reconfigurar
});

console.log(persona.nombre); // 'Ana'

// Propiedad de solo lectura
Object.defineProperty(persona, 'id', {
  value: 12345,
  writable: false,
  enumerable: true,
  configurable: false
});

persona.id = 99999; // No funciona
console.log(persona.id); // 12345

// Getter y Setter
Object.defineProperty(persona, 'nombreCompleto', {
  get() {
    return `${this.nombre} ${this.apellido}`;
  },
  set(valor) {
    const partes = valor.split(' ');
    this.nombre = partes[0];
    this.apellido = partes[1];
  }
});

persona.apellido = 'García';
console.log(persona.nombreCompleto); // 'Ana García'

persona.nombreCompleto = 'María López';
console.log(persona.nombre); // 'María'
console.log(persona.apellido); // 'López'
```

---

## Object.hasOwnProperty()

**📝 Descripción:** Verifica si un objeto tiene una propiedad propia (no heredada).

**💡 Ejemplos:**
```javascript
const persona = {
  nombre: 'Juan',
  edad: 30
};

console.log(persona.hasOwnProperty('nombre')); // true
console.log(persona.hasOwnProperty('email')); // false

// Heredadas vs propias
console.log(persona.hasOwnProperty('toString')); // false (heredada)
console.log('toString' in persona); // true (existe en la cadena)

// Iterar solo propiedades propias
const obj = Object.create({ heredada: 'valor' });
obj.propia = 'mi valor';

for (let key in obj) {
  if (obj.hasOwnProperty(key)) {
    console.log(key); // Solo imprime 'propia'
  }
}
```

---

## Object.getOwnPropertyNames()

**📝 Descripción:** Retorna todas las propiedades propias (incluidas no enumerables).

**💡 Ejemplos:**
```javascript
const obj = { a: 1, b: 2 };

Object.defineProperty(obj, 'c', {
  value: 3,
  enumerable: false
});

console.log(Object.keys(obj)); // ['a', 'b'] (solo enumerables)
console.log(Object.getOwnPropertyNames(obj)); // ['a', 'b', 'c'] (todas)
```

---

## Spread Operator (...)

**📝 Descripción:** Copia propiedades de un objeto a otro (ES6+).

**💡 Ejemplos:**
```javascript
// Copiar objeto
const original = { a: 1, b: 2 };
const copia = { ...original };
console.log(copia); // { a: 1, b: 2 }

// Fusionar objetos
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const fusionado = { ...obj1, ...obj2 };
console.log(fusionado); // { a: 1, b: 2, c: 3, d: 4 }

// Sobreescribir propiedades
const defaults = { color: 'rojo', tamaño: 'M' };
const custom = { color: 'azul' };
const config = { ...defaults, ...custom };
console.log(config); // { color: 'azul', tamaño: 'M' }

// Agregar propiedades
const usuario = { nombre: 'Ana' };
const conEmail = { ...usuario, email: 'ana@example.com' };
console.log(conEmail); // { nombre: 'Ana', email: 'ana@example.com' }

// Actualizar propiedades anidadas
const estado = {
  usuario: { nombre: 'Ana', edad: 25 },
  configuracion: { tema: 'oscuro' }
};

const nuevoEstado = {
  ...estado,
  usuario: { ...estado.usuario, edad: 26 }
};
```

---

## Ejemplo Práctico: Gestión de Configuración

```javascript
class ConfigManager {
  constructor() {
    this.defaultConfig = Object.freeze({
      apiUrl: 'https://api.ejemplo.com',
      timeout: 5000,
      retries: 3,
      headers: {
        'Content-Type': 'application/json'
      }
    });
    
    this.config = { ...this.defaultConfig };
  }
  
  // Actualizar configuración
  actualizar(opciones) {
    this.config = {
      ...this.config,
      ...opciones,
      headers: {
        ...this.config.headers,
        ...(opciones.headers || {})
      }
    };
  }
  
  // Obtener valor
  obtener(clave) {
    return this.config[clave];
  }
  
  // Resetear a valores por defecto
  resetear() {
    this.config = { ...this.defaultConfig };
  }
  
  // Validar configuración
  validar() {
    const requeridos = ['apiUrl', 'timeout'];
    return requeridos.every(key => 
      Object.keys(this.config).includes(key)
    );
  }
  
  // Exportar configuración
  exportar() {
    return JSON.stringify(this.config, null, 2);
  }
  
  // Importar configuración
  importar(jsonString) {
    try {
      const opciones = JSON.parse(jsonString);
      this.actualizar(opciones);
    } catch (error) {
      console.error('Error al importar:', error);
    }
  }
}

// Uso
const config = new ConfigManager();

config.actualizar({
  timeout: 10000,
  headers: { 'Authorization': 'Bearer token123' }
});

console.log('Timeout:', config.obtener('timeout')); // 10000
console.log('Config completa:', config.config);
```

---

## Ejemplo Práctico: Sistema de Caché

```javascript
class CacheObject {
  constructor(ttl = 60000) { // TTL en milisegundos
    this.cache = {};
    this.ttl = ttl;
  }
  
  // Guardar en caché
  set(key, value) {
    this.cache[key] = {
      value,
      timestamp: Date.now()
    };
  }
  
  // Obtener de caché
  get(key) {
    const item = this.cache[key];
    
    if (!item) return null;
    
    // Verificar si expiró
    if (Date.now() - item.timestamp > this.ttl) {
      delete this.cache[key];
      return null;
    }
    
    return item.value;
  }
  
  // Verificar si existe y es válido
  has(key) {
    return this.get(key) !== null;
  }
  
  // Eliminar
  delete(key) {
    delete this.cache[key];
  }
  
  // Limpiar todo
  clear() {
    this.cache = {};
  }
  
  // Limpiar expirados
  limpiarExpirados() {
    const ahora = Date.now();
    
    Object.entries(this.cache).forEach(([key, item]) => {
      if (ahora - item.timestamp > this.ttl) {
        delete this.cache[key];
      }
    });
  }
  
  // Estadísticas
  stats() {
    return {
      items: Object.keys(this.cache).length,
      keys: Object.keys(this.cache),
      size: JSON.stringify(this.cache).length
    };
  }
}

// Uso
const cache = new CacheObject(5000); // 5 segundos TTL

cache.set('usuario', { nombre: 'Ana', edad: 25 });
cache.set('productos', [1, 2, 3]);

console.log(cache.get('usuario')); // { nombre: 'Ana', edad: 25 }
console.log(cache.stats()); // { items: 2, keys: [...], size: ... }

setTimeout(() => {
  console.log(cache.get('usuario')); // null (expiró)
}, 6000);
```

---

## Ejemplo Práctico: Deep Clone

```javascript
function deepClone(obj) {
  // Casos base
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }
  
  // Arrays
  if (Array.isArray(obj)) {
    return obj.map(item => deepClone(item));
  }
  
  // Dates
  if (obj instanceof Date) {
    return new Date(obj);
  }
  
  // Objetos
  const clonado = {};
  
  Object.keys(obj).forEach(key => {
    clonado[key] = deepClone(obj[key]);
  });
  
  return clonado;
}

// Uso
const original = {
  nombre: 'Ana',
  datos: {
    edad: 25,
    hobbies: ['leer', 'correr']
  },
  fecha: new Date()
};

const copia = deepClone(original);
copia.datos.edad = 30;
copia.datos.hobbies.push('nadar');

console.log(original.datos.edad); // 25 (sin cambios)
console.log(original.datos.hobbies); // ['leer', 'correr'] (sin cambios)
```

---

## Buenas Prácticas

```javascript
// ✅ Bueno: Usar const para objetos que no se reasignan
const usuario = { nombre: 'Ana' };
usuario.edad = 25; // Modificar propiedades está bien

// ❌ Evitar: let innecesario
let config = { apiUrl: '...' }; // Usar const si no se reasigna

// ✅ Bueno: Usar spread para copias shallow
const copia = { ...original };

// ✅ Bueno: Destructuring para extraer propiedades
const { nombre, edad } = usuario;

// ✅ Bueno: Shorthand property names
const nombre = 'Ana';
const edad = 25;
const persona = { nombre, edad }; // En lugar de { nombre: nombre, edad: edad }

// ✅ Bueno: Métodos abreviados
const obj = {
  saludar() { // En lugar de saludar: function() {
    console.log('Hola');
  }
};

// ✅ Bueno: Optional chaining para propiedades anidadas
const email = usuario?.contacto?.email ?? 'Sin email';
```
