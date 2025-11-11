# 🔗 Prototipos en JavaScript

## ¿Qué son los Prototipos?

Los **prototipos** son el mecanismo por el cual los objetos en JavaScript heredan características de otros objetos. Cada objeto tiene una propiedad interna `[[Prototype]]` que apunta a otro objeto.

---

## Prototype Chain (Cadena de Prototipos)

**💡 Ejemplo Básico:**
```javascript
const persona = {
  saludar() {
    console.log(`Hola, soy ${this.nombre}`);
  }
};

const ana = Object.create(persona);
ana.nombre = 'Ana';

ana.saludar(); // 'Hola, soy Ana'
// ana hereda el método saludar de persona

console.log(ana.hasOwnProperty('nombre')); // true (propiedad propia)
console.log(ana.hasOwnProperty('saludar')); // false (heredada)
```

---

## __proto__ y prototype

**💡 Ejemplos:**
```javascript
// Función constructora
function Persona(nombre, edad) {
  this.nombre = nombre;
  this.edad = edad;
}

// Agregar método al prototipo
Persona.prototype.saludar = function() {
  console.log(`Hola, soy ${this.nombre}`);
};

Persona.prototype.cumpleanos = function() {
  this.edad++;
  console.log(`Ahora tengo ${this.edad} años`);
};

// Crear instancias
const ana = new Persona('Ana', 25);
const juan = new Persona('Juan', 30);

ana.saludar(); // 'Hola, soy Ana'
juan.saludar(); // 'Hola, soy Juan'

ana.cumpleanos(); // 'Ahora tengo 26 años'

// Verificar prototipo
console.log(ana.__proto__ === Persona.prototype); // true
console.log(Object.getPrototypeOf(ana) === Persona.prototype); // true
```

---

## Herencia Prototipal

**💡 Ejemplo: Sistema de Usuarios**
```javascript
// Constructor base
function Usuario(nombre, email) {
  this.nombre = nombre;
  this.email = email;
  this.fechaRegistro = new Date();
}

Usuario.prototype.mostrarInfo = function() {
  return `${this.nombre} (${this.email})`;
};

Usuario.prototype.tiempoRegistrado = function() {
  const dias = Math.floor((Date.now() - this.fechaRegistro) / (1000 * 60 * 60 * 24));
  return `${dias} días`;
};

// Constructor hijo: Admin
function Admin(nombre, email, permisos) {
  Usuario.call(this, nombre, email); // Llamar constructor padre
  this.permisos = permisos;
  this.rol = 'admin';
}

// Heredar de Usuario
Admin.prototype = Object.create(Usuario.prototype);
Admin.prototype.constructor = Admin;

// Métodos específicos de Admin
Admin.prototype.agregarPermiso = function(permiso) {
  this.permisos.push(permiso);
};

Admin.prototype.mostrarInfo = function() {
  return `[ADMIN] ${this.nombre} - Permisos: ${this.permisos.length}`;
};

// Constructor hijo: Cliente
function Cliente(nombre, email, compras) {
  Usuario.call(this, nombre, email);
  this.compras = compras || [];
  this.rol = 'cliente';
}

Cliente.prototype = Object.create(Usuario.prototype);
Cliente.prototype.constructor = Cliente;

Cliente.prototype.agregarCompra = function(compra) {
  this.compras.push(compra);
};

Cliente.prototype.totalGastado = function() {
  return this.compras.reduce((total, compra) => total + compra.monto, 0);
};

// Uso
const admin = new Admin('Ana', 'ana@admin.com', ['leer', 'escribir']);
const cliente = new Cliente('Juan', 'juan@cliente.com', []);

console.log(admin.mostrarInfo()); // '[ADMIN] Ana - Permisos: 2'
console.log(cliente.mostrarInfo()); // 'Juan (juan@cliente.com)'

admin.agregarPermiso('eliminar');
cliente.agregarCompra({ producto: 'Laptop', monto: 1200 });

console.log(admin.permisos); // ['leer', 'escribir', 'eliminar']
console.log(cliente.totalGastado()); // 1200

// Verificar herencia
console.log(admin instanceof Admin); // true
console.log(admin instanceof Usuario); // true
console.log(cliente instanceof Cliente); // true
console.log(cliente instanceof Usuario); // true
```

---

## Clases (ES6) - Sintaxis Moderna

**💡 Ejemplo Equivalente con Clases:**
```javascript
class Usuario {
  constructor(nombre, email) {
    this.nombre = nombre;
    this.email = email;
    this.fechaRegistro = new Date();
  }
  
  mostrarInfo() {
    return `${this.nombre} (${this.email})`;
  }
  
  tiempoRegistrado() {
    const dias = Math.floor((Date.now() - this.fechaRegistro) / (1000 * 60 * 60 * 24));
    return `${dias} días`;
  }
}

class Admin extends Usuario {
  constructor(nombre, email, permisos) {
    super(nombre, email); // Llamar constructor padre
    this.permisos = permisos;
    this.rol = 'admin';
  }
  
  agregarPermiso(permiso) {
    this.permisos.push(permiso);
  }
  
  mostrarInfo() {
    return `[ADMIN] ${this.nombre} - Permisos: ${this.permisos.length}`;
  }
}

class Cliente extends Usuario {
  constructor(nombre, email, compras = []) {
    super(nombre, email);
    this.compras = compras;
    this.rol = 'cliente';
  }
  
  agregarCompra(compra) {
    this.compras.push(compra);
  }
  
  totalGastado() {
    return this.compras.reduce((total, compra) => total + compra.monto, 0);
  }
}

// Uso igual que antes
const admin = new Admin('Ana', 'ana@admin.com', ['leer', 'escribir']);
const cliente = new Cliente('Juan', 'juan@cliente.com');
```

---

## Métodos Estáticos

**💡 Ejemplos:**
```javascript
class Utilidades {
  static formatearFecha(fecha) {
    return fecha.toLocaleDateString('es-ES');
  }
  
  static generarId() {
    return '_' + Math.random().toString(36).substr(2, 9);
  }
  
  static validarEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }
}

// Uso sin instanciar
console.log(Utilidades.formatearFecha(new Date()));
console.log(Utilidades.generarId());
console.log(Utilidades.validarEmail('test@example.com')); // true

// Ejemplo práctico: Modelo de datos
class Usuario {
  constructor(nombre, email) {
    this.id = Usuario.generarId();
    this.nombre = nombre;
    this.email = email;
  }
  
  static generarId() {
    return Date.now() + '_' + Math.random().toString(36).substr(2, 9);
  }
  
  static validar(datos) {
    return datos.nombre && datos.email && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(datos.email);
  }
  
  static desde JSON(json) {
    const datos = JSON.parse(json);
    return new Usuario(datos.nombre, datos.email);
  }
}

const usuario = new Usuario('Ana', 'ana@example.com');
console.log(usuario.id);

console.log(Usuario.validar({ nombre: 'Juan', email: 'juan@example.com' })); // true
```

---

## Getters y Setters

**💡 Ejemplos:**
```javascript
class Persona {
  constructor(nombre, apellido, nacimiento) {
    this._nombre = nombre;
    this._apellido = apellido;
    this._nacimiento = nacimiento;
  }
  
  // Getter
  get nombreCompleto() {
    return `${this._nombre} ${this._apellido}`;
  }
  
  // Setter
  set nombreCompleto(valor) {
    const partes = valor.split(' ');
    this._nombre = partes[0];
    this._apellido = partes[1] || '';
  }
  
  get edad() {
    const hoy = new Date();
    const nacimiento = new Date(this._nacimiento);
    let edad = hoy.getFullYear() - nacimiento.getFullYear();
    const mes = hoy.getMonth() - nacimiento.getMonth();
    
    if (mes < 0 || (mes === 0 && hoy.getDate() < nacimiento.getDate())) {
      edad--;
    }
    
    return edad;
  }
}

const persona = new Persona('Ana', 'García', '1998-05-15');

console.log(persona.nombreCompleto); // 'Ana García'
console.log(persona.edad); // Calcula la edad

persona.nombreCompleto = 'María López';
console.log(persona._nombre); // 'María'
console.log(persona._apellido); // 'López'

// Ejemplo: Validación con Setters
class Producto {
  constructor(nombre, precio) {
    this.nombre = nombre;
    this._precio = precio;
  }
  
  get precio() {
    return this._precio;
  }
  
  set precio(valor) {
    if (valor < 0) {
      throw new Error('El precio no puede ser negativo');
    }
    this._precio = valor;
  }
  
  get precioConIVA() {
    return this._precio * 1.21;
  }
}

const producto = new Producto('Laptop', 1000);
console.log(producto.precio); // 1000
console.log(producto.precioConIVA); // 1210

producto.precio = 1200;
// producto.precio = -100; // Error!
```

---

## Propiedades Privadas (ES2022)

**💡 Ejemplos:**
```javascript
class CuentaBancaria {
  #saldo = 0; // Propiedad privada
  #historial = [];
  
  constructor(titular) {
    this.titular = titular;
  }
  
  depositar(cantidad) {
    if (cantidad <= 0) {
      throw new Error('Cantidad inválida');
    }
    this.#saldo += cantidad;
    this.#historial.push({ tipo: 'deposito', cantidad, fecha: new Date() });
  }
  
  retirar(cantidad) {
    if (cantidad > this.#saldo) {
      throw new Error('Saldo insuficiente');
    }
    this.#saldo -= cantidad;
    this.#historial.push({ tipo: 'retiro', cantidad, fecha: new Date() });
  }
  
  get saldo() {
    return this.#saldo;
  }
  
  get historial() {
    return [...this.#historial]; // Retornar copia
  }
  
  #calcularInteres() { // Método privado
    return this.#saldo * 0.02;
  }
  
  aplicarInteres() {
    const interes = this.#calcularInteres();
    this.#saldo += interes;
    this.#historial.push({ tipo: 'interes', cantidad: interes, fecha: new Date() });
  }
}

const cuenta = new Cuenta Bancaria('Ana García');

cuenta.depositar(1000);
cuenta.retirar(200);

console.log(cuenta.saldo); // 800
// console.log(cuenta.#saldo); // Error! Privado
console.log(cuenta.historial); // Acceso al historial

cuenta.aplicarInteres();
console.log(cuenta.saldo); // 816
```

---

## Ejemplo Completo: Sistema de Formas Geométricas

```javascript
class Forma {
  constructor(color) {
    this.color = color;
  }
  
  area() {
    throw new Error('El método area() debe ser implementado');
  }
  
  perimetro() {
    throw new Error('El método perimetro() debe ser implementado');
  }
  
  descripcion() {
    return `Forma de color ${this.color}`;
  }
}

class Circulo extends Forma {
  constructor(color, radio) {
    super(color);
    this.radio = radio;
  }
  
  area() {
    return Math.PI * this.radio ** 2;
  }
  
  perimetro() {
    return 2 * Math.PI * this.radio;
  }
  
  descripcion() {
    return `Círculo ${this.color} con radio ${this.radio}`;
  }
}

class Rectangulo extends Forma {
  constructor(color, ancho, alto) {
    super(color);
    this.ancho = ancho;
    this.alto = alto;
  }
  
  area() {
    return this.ancho * this.alto;
  }
  
  perimetro() {
    return 2 * (this.ancho + this.alto);
  }
  
  descripcion() {
    return `Rectángulo ${this.color} de ${this.ancho}x${this.alto}`;
  }
}

class Triangulo extends Forma {
  constructor(color, base, altura) {
    super(color);
    this.base = base;
    this.altura = altura;
  }
  
  area() {
    return (this.base * this.altura) / 2;
  }
  
  perimetro() {
    // Simplificado (asume triángulo isósceles)
    const lado = Math.sqrt((this.base / 2) ** 2 + this.altura ** 2);
    return this.base + 2 * lado;
  }
  
  descripcion() {
    return `Triángulo ${this.color} con base ${this.base} y altura ${this.altura}`;
  }
}

// Uso
const formas = [
  new Circulo('rojo', 5),
  new Rectangulo('azul', 4, 6),
  new Triangulo('verde', 3, 4)
];

formas.forEach(forma => {
  console.log(forma.descripcion());
  console.log(`Área: ${forma.area().toFixed(2)}`);
  console.log(`Perímetro: ${forma.perimetro().toFixed(2)}`);
  console.log('---');
});

// Calcular área total
const areaTotal = formas.reduce((total, forma) => total + forma.area(), 0);
console.log(`Área total: ${areaTotal.toFixed(2)}`);
```

---

## Polimorfismo

**💡 Ejemplo:**
```javascript
class Animal {
  constructor(nombre) {
    this.nombre = nombre;
  }
  
  hacerSonido() {
    return 'Algún sonido';
  }
  
  presentarse() {
    return `Soy ${this.nombre} y hago: ${this.hacerSonido()}`;
  }
}

class Perro extends Animal {
  hacerSonido() {
    return 'Guau guau';
  }
}

class Gato extends Animal {
  hacerSonido() {
    return 'Miau';
  }
}

class Vaca extends Animal {
  hacerSonido() {
    return 'Muuu';
  }
}

// Polimorfismo en acción
const animales = [
  new Perro('Firulais'),
  new Gato('Michi'),
  new Vaca('Lola')
];

animales.forEach(animal => {
  console.log(animal.presentarse());
});

// Soy Firulais y hago: Guau guau
// Soy Michi y hago: Miau
// Soy Lola y hago: Muuu
```

---

## Buenas Prácticas

```javascript
// ✅ Bueno: Usar clases para código más limpio
class Usuario {
  constructor(nombre) {
    this.nombre = nombre;
  }
}

// ❌ Evitar: Constructor functions en código nuevo
function Usuario(nombre) {
  this.nombre = nombre;
}

// ✅ Bueno: Propiedades privadas para encapsulación
class BankAccount {
  #balance = 0;
  
  deposit(amount) {
    this.#balance += amount;
  }
}

// ✅ Bueno: Métodos estáticos para utilidades
class Utils {
  static formatDate(date) {
    return date.toLocaleDateString();
  }
}

// ✅ Bueno: Getters/Setters para lógica de acceso
class Product {
  #price;
  
  get price() {
    return this.#price;
  }
  
  set price(value) {
    if (value < 0) throw new Error('Invalid price');
    this.#price = value;
  }
}
```
