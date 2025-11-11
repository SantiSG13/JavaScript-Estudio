# 🔥 Ejemplos Prácticos de Arrays

## 1. Sistema de Carrito de Compras

```javascript
class CarritoCompras {
  constructor() {
    this.items = [];
  }
  
  // Agregar producto
  agregar(producto, cantidad = 1) {
    const existe = this.items.find(item => item.producto.id === producto.id);
    
    if (existe) {
      existe.cantidad += cantidad;
    } else {
      this.items.push({ producto, cantidad });
    }
  }
  
  // Eliminar producto
  eliminar(idProducto) {
    this.items = this.items.filter(item => item.producto.id !== idProducto);
  }
  
  // Actualizar cantidad
  actualizarCantidad(idProducto, cantidad) {
    const item = this.items.find(item => item.producto.id === idProducto);
    if (item) {
      item.cantidad = cantidad;
    }
  }
  
  // Calcular subtotal de un item
  subtotalItem(item) {
    return item.producto.precio * item.cantidad;
  }
  
  // Calcular total
  calcularTotal() {
    return this.items.reduce((total, item) => {
      return total + this.subtotalItem(item);
    }, 0);
  }
  
  // Obtener cantidad de items
  cantidadTotal() {
    return this.items.reduce((total, item) => total + item.cantidad, 0);
  }
  
  // Vaciar carrito
  vaciar() {
    this.items = [];
  }
  
  // Resumen del carrito
  resumen() {
    return {
      items: this.items.length,
      productos: this.cantidadTotal(),
      total: this.calcularTotal().toFixed(2)
    };
  }
}

// Uso
const carrito = new CarritoCompras();

const productos = [
  { id: 1, nombre: 'Laptop', precio: 1200 },
  { id: 2, nombre: 'Mouse', precio: 25 },
  { id: 3, nombre: 'Teclado', precio: 80 }
];

carrito.agregar(productos[0], 1);
carrito.agregar(productos[1], 2);
carrito.agregar(productos[2], 1);

console.log('Total:', carrito.calcularTotal()); // 1330
console.log('Resumen:', carrito.resumen());
// { items: 3, productos: 4, total: '1330.00' }

carrito.eliminar(2);
console.log('Después de eliminar:', carrito.resumen());
```

---

## 2. Buscador y Filtro de Productos

```javascript
const productos = [
  { id: 1, nombre: 'Laptop HP', precio: 1200, categoria: 'Electrónica', marca: 'HP', rating: 4.5 },
  { id: 2, nombre: 'Mouse Logitech', precio: 25, categoria: 'Electrónica', marca: 'Logitech', rating: 4.2 },
  { id: 3, nombre: 'Teclado Mecánico', precio: 80, categoria: 'Electrónica', marca: 'Razer', rating: 4.8 },
  { id: 4, nombre: 'Monitor Samsung', precio: 300, categoria: 'Electrónica', marca: 'Samsung', rating: 4.3 },
  { id: 5, nombre: 'Silla Gamer', precio: 250, categoria: 'Muebles', marca: 'DXRacer', rating: 4.6 }
];

class BuscadorProductos {
  constructor(productos) {
    this.productos = productos;
  }
  
  // Buscar por texto
  buscar(termino) {
    termino = termino.toLowerCase();
    return this.productos.filter(p => 
      p.nombre.toLowerCase().includes(termino) ||
      p.marca.toLowerCase().includes(termino)
    );
  }
  
  // Filtrar por categoría
  porCategoria(categoria) {
    return this.productos.filter(p => p.categoria === categoria);
  }
  
  // Filtrar por rango de precio
  porRangoPrecio(min, max) {
    return this.productos.filter(p => p.precio >= min && p.precio <= max);
  }
  
  // Filtrar por rating mínimo
  porRating(minRating) {
    return this.productos.filter(p => p.rating >= minRating);
  }
  
  // Ordenar
  ordenar(productos, campo, direccion = 'asc') {
    return [...productos].sort((a, b) => {
      if (direccion === 'asc') {
        return a[campo] > b[campo] ? 1 : -1;
      } else {
        return a[campo] < b[campo] ? 1 : -1;
      }
    });
  }
  
  // Filtro combinado
  filtrar(opciones) {
    let resultado = this.productos;
    
    if (opciones.busqueda) {
      resultado = this.buscar(opciones.busqueda);
    }
    
    if (opciones.categoria) {
      resultado = resultado.filter(p => p.categoria === opciones.categoria);
    }
    
    if (opciones.precioMin !== undefined && opciones.precioMax !== undefined) {
      resultado = resultado.filter(p => 
        p.precio >= opciones.precioMin && p.precio <= opciones.precioMax
      );
    }
    
    if (opciones.ratingMin) {
      resultado = resultado.filter(p => p.rating >= opciones.ratingMin);
    }
    
    if (opciones.ordenarPor) {
      resultado = this.ordenar(
        resultado,
        opciones.ordenarPor,
        opciones.direccion || 'asc'
      );
    }
    
    return resultado;
  }
}

// Uso
const buscador = new BuscadorProductos(productos);

console.log('Buscar "mouse":', buscador.buscar('mouse'));

console.log('Filtro combinado:');
const resultado = buscador.filtrar({
  categoria: 'Electrónica',
  precioMin: 50,
  precioMax: 500,
  ratingMin: 4.0,
  ordenarPor: 'precio',
  direccion: 'asc'
});
console.log(resultado);
```

---

## 3. Gestor de Tareas con Prioridades

```javascript
class GestorTareas {
  constructor() {
    this.tareas = [];
    this.idCounter = 1;
  }
  
  agregar(titulo, prioridad = 'media', categoria = 'general') {
    const tarea = {
      id: this.idCounter++,
      titulo,
      prioridad, // 'alta', 'media', 'baja'
      categoria,
      completada: false,
      fechaCreacion: new Date(),
      fechaCompletada: null
    };
    
    this.tareas.push(tarea);
    return tarea;
  }
  
  completar(id) {
    const tarea = this.tareas.find(t => t.id === id);
    if (tarea) {
      tarea.completada = true;
      tarea.fechaCompletada = new Date();
    }
  }
  
  eliminar(id) {
    this.tareas = this.tareas.filter(t => t.id !== id);
  }
  
  obtenerPendientes() {
    return this.tareas.filter(t => !t.completada);
  }
  
  obtenerCompletadas() {
    return this.tareas.filter(t => t.completada);
  }
  
  porPrioridad(prioridad) {
    return this.tareas.filter(t => 
      t.prioridad === prioridad && !t.completada
    );
  }
  
  porCategoria(categoria) {
    return this.tareas.filter(t => t.categoria === categoria);
  }
  
  ordenarPorPrioridad(tareas) {
    const orden = { alta: 1, media: 2, baja: 3 };
    return [...tareas].sort((a, b) => orden[a.prioridad] - orden[b.prioridad]);
  }
  
  estadisticas() {
    const total = this.tareas.length;
    const completadas = this.obtenerCompletadas().length;
    const pendientes = this.obtenerPendientes().length;
    
    const porPrioridad = {
      alta: this.porPrioridad('alta').length,
      media: this.porPrioridad('media').length,
      baja: this.porPrioridad('baja').length
    };
    
    return {
      total,
      completadas,
      pendientes,
      porcentajeCompletado: total > 0 ? ((completadas / total) * 100).toFixed(1) : 0,
      porPrioridad
    };
  }
}

// Uso
const gestor = new GestorTareas();

gestor.agregar('Estudiar JavaScript', 'alta', 'educacion');
gestor.agregar('Hacer ejercicio', 'media', 'salud');
gestor.agregar('Comprar comida', 'alta', 'hogar');
gestor.agregar('Ver serie', 'baja', 'entretenimiento');

console.log('Pendientes:', gestor.obtenerPendientes());
console.log('Alta prioridad:', gestor.porPrioridad('alta'));

gestor.completar(1);
gestor.completar(3);

console.log('Estadísticas:', gestor.estadisticas());
```

---

## 4. Análisis de Datos de Ventas

```javascript
const ventas = [
  { id: 1, producto: 'Laptop', cantidad: 2, precio: 1200, fecha: '2024-01-15', vendedor: 'Ana' },
  { id: 2, producto: 'Mouse', cantidad: 5, precio: 25, fecha: '2024-01-15', vendedor: 'Juan' },
  { id: 3, producto: 'Teclado', cantidad: 3, precio: 80, fecha: '2024-01-16', vendedor: 'Ana' },
  { id: 4, producto: 'Monitor', cantidad: 1, precio: 300, fecha: '2024-01-16', vendedor: 'María' },
  { id: 5, producto: 'Laptop', cantidad: 1, precio: 1200, fecha: '2024-01-17', vendedor: 'Juan' }
];

class AnalizadorVentas {
  constructor(ventas) {
    this.ventas = ventas;
  }
  
  // Total de ventas
  totalVentas() {
    return this.ventas.reduce((total, venta) => {
      return total + (venta.cantidad * venta.precio);
    }, 0);
  }
  
  // Ventas por vendedor
  porVendedor() {
    return this.ventas.reduce((acc, venta) => {
      const vendedor = venta.vendedor;
      if (!acc[vendedor]) {
        acc[vendedor] = { cantidad: 0, total: 0 };
      }
      acc[vendedor].cantidad += venta.cantidad;
      acc[vendedor].total += venta.cantidad * venta.precio;
      return acc;
    }, {});
  }
  
  // Producto más vendido
  productoMasVendido() {
    const conteo = this.ventas.reduce((acc, venta) => {
      if (!acc[venta.producto]) {
        acc[venta.producto] = 0;
      }
      acc[venta.producto] += venta.cantidad;
      return acc;
    }, {});
    
    const productos = Object.entries(conteo);
    productos.sort((a, b) => b[1] - a[1]);
    
    return { producto: productos[0][0], cantidad: productos[0][1] };
  }
  
  // Promedio de venta
  promedioVenta() {
    if (this.ventas.length === 0) return 0;
    return (this.totalVentas() / this.ventas.length).toFixed(2);
  }
  
  // Ventas por fecha
  porFecha(fecha) {
    return this.ventas.filter(v => v.fecha === fecha);
  }
  
  // Top vendedores
  topVendedores(n = 3) {
    const porVendedor = this.porVendedor();
    const vendedores = Object.entries(porVendedor);
    
    vendedores.sort((a, b) => b[1].total - a[1].total);
    
    return vendedores.slice(0, n).map(([nombre, datos]) => ({
      vendedor: nombre,
      ...datos
    }));
  }
  
  // Reporte completo
  generarReporte() {
    return {
      totalVentas: this.totalVentas(),
      cantidadTransacciones: this.ventas.length,
      promedioVenta: this.promedioVenta(),
      productoMasVendido: this.productoMasVendido(),
      ventasPorVendedor: this.porVendedor(),
      topVendedores: this.topVendedores(3)
    };
  }
}

// Uso
const analizador = new AnalizadorVentas(ventas);

console.log('Total ventas: $', analizador.totalVentas());
console.log('Por vendedor:', analizador.porVendedor());
console.log('Producto más vendido:', analizador.productoMasVendido());
console.log('Reporte completo:', analizador.generarReporte());
```

---

## 5. Procesador de Datos CSV

```javascript
class ProcesadorCSV {
  // Convertir CSV a array de objetos
  static parsear(csvString) {
    const lineas = csvString.trim().split('\n');
    const headers = lineas[0].split(',').map(h => h.trim());
    
    return lineas.slice(1).map(linea => {
      const valores = linea.split(',').map(v => v.trim());
      
      return headers.reduce((obj, header, index) => {
        obj[header] = valores[index];
        return obj;
      }, {});
    });
  }
  
  // Convertir array de objetos a CSV
  static aCSV(datos) {
    if (datos.length === 0) return '';
    
    const headers = Object.keys(datos[0]);
    const headerRow = headers.join(',');
    
    const rows = datos.map(objeto => {
      return headers.map(header => objeto[header]).join(',');
    });
    
    return [headerRow, ...rows].join('\n');
  }
  
  // Filtrar columnas
  static seleccionarColumnas(datos, columnas) {
    return datos.map(fila => {
      return columnas.reduce((obj, col) => {
        obj[col] = fila[col];
        return obj;
      }, {});
    });
  }
  
  // Agrupar por campo
  static agruparPor(datos, campo) {
    return datos.reduce((acc, item) => {
      const valor = item[campo];
      if (!acc[valor]) acc[valor] = [];
      acc[valor].push(item);
      return acc;
    }, {});
  }
}

// Uso
const csvData = `
nombre,edad,ciudad,profesion
Ana,25,Madrid,Ingeniera
Juan,30,Barcelona,Doctor
María,28,Madrid,Diseñadora
Pedro,35,Valencia,Profesor
`;

const datos = ProcesadorCSV.parsear(csvData);
console.log('Datos parseados:', datos);

const soloNombreEdad = ProcesadorCSV.seleccionarColumnas(datos, ['nombre', 'edad']);
console.log('Solo nombre y edad:', soloNombreEdad);

const porCiudad = ProcesadorCSV.agruparPor(datos, 'ciudad');
console.log('Agrupados por ciudad:', porCiudad);

const nuevoCSV = ProcesadorCSV.aCSV(datos);
console.log('De vuelta a CSV:\n', nuevoCSV);
```

---

## 6. Sistema de Notificaciones

```javascript
class SistemaNotificaciones {
  constructor() {
    this.notificaciones = [];
    this.idCounter = 1;
  }
  
  agregar(mensaje, tipo = 'info', prioridad = 'normal') {
    const notificacion = {
      id: this.idCounter++,
      mensaje,
      tipo, // 'info', 'warning', 'error', 'success'
      prioridad, // 'alta', 'normal', 'baja'
      leida: false,
      fecha: new Date()
    };
    
    this.notificaciones.unshift(notificacion); // Agregar al inicio
    return notificacion;
  }
  
  marcarComoLeida(id) {
    const notif = this.notificaciones.find(n => n.id === id);
    if (notif) notif.leida = true;
  }
  
  marcarTodasLeidas() {
    this.notificaciones.forEach(n => n.leida = true);
  }
  
  eliminar(id) {
    this.notificaciones = this.notificaciones.filter(n => n.id !== id);
  }
  
  obtenerNoLeidas() {
    return this.notificaciones.filter(n => !n.leida);
  }
  
  obtenerPorTipo(tipo) {
    return this.notificaciones.filter(n => n.tipo === tipo);
  }
  
  obtenerRecientes(cantidad = 5) {
    return this.notificaciones.slice(0, cantidad);
  }
  
  limpiarLeidas() {
    this.notificaciones = this.notificaciones.filter(n => !n.leida);
  }
  
  contador() {
    return {
      total: this.notificaciones.length,
      noLeidas: this.obtenerNoLeidas().length,
      porTipo: {
        info: this.obtenerPorTipo('info').length,
        warning: this.obtenerPorTipo('warning').length,
        error: this.obtenerPorTipo('error').length,
        success: this.obtenerPorTipo('success').length
      }
    };
  }
}

// Uso
const notificaciones = new SistemaNotificaciones();

notificaciones.agregar('Nuevo mensaje recibido', 'info', 'normal');
notificaciones.agregar('Actualización disponible', 'warning', 'alta');
notificaciones.agregar('Error al guardar', 'error', 'alta');
notificaciones.agregar('Cambios guardados exitosamente', 'success', 'normal');

console.log('No leídas:', notificaciones.obtenerNoLeidas().length);
console.log('Recientes:', notificaciones.obtenerRecientes(3));

notificaciones.marcarComoLeida(1);
console.log('Contador:', notificaciones.contador());
```

---

## 7. Validador de Formularios

```javascript
class ValidadorFormulario {
  constructor() {
    this.errores = [];
  }
  
  validarRequerido(campo, valor) {
    if (!valor || valor.trim() === '') {
      this.errores.push(`El campo ${campo} es requerido`);
      return false;
    }
    return true;
  }
  
  validarEmail(email) {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!regex.test(email)) {
      this.errores.push('Email inválido');
      return false;
    }
    return true;
  }
  
  validarLongitud(campo, valor, min, max) {
    if (valor.length < min || valor.length > max) {
      this.errores.push(`${campo} debe tener entre ${min} y ${max} caracteres`);
      return false;
    }
    return true;
  }
  
  validarNumero(campo, valor, min, max) {
    const num = Number(valor);
    if (isNaN(num) || num < min || num > max) {
      this.errores.push(`${campo} debe ser un número entre ${min} y ${max}`);
      return false;
    }
    return true;
  }
  
  validarFormulario(datos, reglas) {
    this.errores = [];
    
    Object.keys(reglas).forEach(campo => {
      const valor = datos[campo];
      const reglaCampo = reglas[campo];
      
      if (reglaCampo.requerido) {
        if (!this.validarRequerido(campo, valor)) return;
      }
      
      if (reglaCampo.tipo === 'email' && valor) {
        this.validarEmail(valor);
      }
      
      if (reglaCampo.longitudMin && reglaCampo.longitudMax && valor) {
        this.validarLongitud(campo, valor, reglaCampo.longitudMin, reglaCampo.longitudMax);
      }
      
      if (reglaCampo.min !== undefined && reglaCampo.max !== undefined && valor) {
        this.validarNumero(campo, valor, reglaCampo.min, reglaCampo.max);
      }
    });
    
    return this.errores.length === 0;
  }
  
  obtenerErrores() {
    return this.errores;
  }
}

// Uso
const validador = new ValidadorFormulario();

const formulario = {
  nombre: 'Ana',
  email: 'ana@example.com',
  edad: '25',
  password: '12345678'
};

const reglas = {
  nombre: { requerido: true, longitudMin: 2, longitudMax: 50 },
  email: { requerido: true, tipo: 'email' },
  edad: { requerido: true, min: 18, max: 100 },
  password: { requerido: true, longitudMin: 8, longitudMax: 20 }
};

if (validador.validarFormulario(formulario, reglas)) {
  console.log('Formulario válido ✅');
} else {
  console.log('Errores:', validador.obtenerErrores());
}
```
