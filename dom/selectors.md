# 🎯 Selectores del DOM

## ¿Qué es el DOM?

El **DOM (Document Object Model)** es una representación en forma de árbol de todos los elementos HTML de una página web. JavaScript puede acceder y manipular este árbol para cambiar el contenido, estructura y estilos de la página.

---

## Métodos de Selección

### getElementById()

**📝 Descripción:** Selecciona un elemento por su atributo `id`. Retorna un único elemento.

**✍️ Sintaxis:**
```javascript
document.getElementById('idDelElemento')
```

**📤 Retorna:** El elemento con ese ID o `null` si no existe

**💡 Ejemplo:**
```html
<div id="header">Encabezado</div>
<button id="btn-submit">Enviar</button>
```

```javascript
const header = document.getElementById('header');
console.log(header.textContent); // 'Encabezado'

const button = document.getElementById('btn-submit');
console.log(button); // <button id="btn-submit">Enviar</button>
```

**⚠️ Notas importantes:**
- Solo funciona con IDs (no clases ni etiquetas)
- Los IDs deben ser únicos en el documento
- No necesitas el símbolo `#`
- Es el método más rápido para seleccionar elementos

---

### getElementsByClassName()

**📝 Descripción:** Selecciona **todos** los elementos con una clase específica. Retorna una colección HTML.

**✍️ Sintaxis:**
```javascript
document.getElementsByClassName('nombreClase')
```

**📤 Retorna:** HTMLCollection (parecido a un array pero no es un array)

**💡 Ejemplo:**
```html
<div class="card">Card 1</div>
<div class="card">Card 2</div>
<div class="card active">Card 3</div>
```

```javascript
const cards = document.getElementsByClassName('card');
console.log(cards.length); // 3

// Acceder a elementos individuales
console.log(cards[0].textContent); // 'Card 1'
console.log(cards[1].textContent); // 'Card 2'

// Convertir a array para usar métodos de array
const cardsArray = Array.from(cards);
cardsArray.forEach(card => {
  console.log(card.textContent);
});

// Múltiples clases (ambas deben estar presentes)
const activeCards = document.getElementsByClassName('card active');
```

**⚠️ Notas importantes:**
- Retorna una colección "viva" (se actualiza automáticamente)
- No es un array real, no tiene métodos como `.map()` o `.filter()`
- No necesitas el símbolo `.`

---

### getElementsByTagName()

**📝 Descripción:** Selecciona **todos** los elementos de una etiqueta específica.

**✍️ Sintaxis:**
```javascript
document.getElementsByTagName('nombreEtiqueta')
```

**📤 Retorna:** HTMLCollection

**💡 Ejemplo:**
```html
<p>Párrafo 1</p>
<p>Párrafo 2</p>
<div>
  <p>Párrafo 3</p>
</div>
```

```javascript
const paragraphs = document.getElementsByTagName('p');
console.log(paragraphs.length); // 3

// Seleccionar todos los divs
const divs = document.getElementsByTagName('div');

// Seleccionar todos los elementos
const allElements = document.getElementsByTagName('*');
console.log(allElements.length); // Todos los elementos del documento
```

---

### querySelector()

**📝 Descripción:** Selecciona el **primer elemento** que coincida con un selector CSS. Es el método moderno más versátil.

**✍️ Sintaxis:**
```javascript
document.querySelector('selectorCSS')
```

**📤 Retorna:** El primer elemento que coincide o `null`

**💡 Ejemplos:**
```html
<div id="app">
  <h1 class="title">Título</h1>
  <p class="text">Párrafo 1</p>
  <p class="text highlight">Párrafo 2</p>
  <button data-action="submit">Enviar</button>
</div>
```

```javascript
// Por ID
const app = document.querySelector('#app');

// Por clase
const title = document.querySelector('.title');
const text = document.querySelector('.text'); // Solo el primero

// Por etiqueta
const firstParagraph = document.querySelector('p');

// Múltiples clases
const highlighted = document.querySelector('.text.highlight');

// Selectores complejos
const button = document.querySelector('[data-action="submit"]');
const firstChild = document.querySelector('#app > h1');
const descendant = document.querySelector('#app p');

// Pseudo-clases
const firstP = document.querySelector('p:first-child');
const lastP = document.querySelector('p:last-child');
const nthP = document.querySelector('p:nth-child(2)');
```

**⚠️ Notas importantes:**
- Usa sintaxis de CSS (necesitas `#` para IDs y `.` para clases)
- Solo retorna el primer elemento que coincide
- Es más lento que `getElementById` pero más flexible

---

### querySelectorAll()

**📝 Descripción:** Selecciona **todos** los elementos que coincidan con un selector CSS.

**✍️ Sintaxis:**
```javascript
document.querySelectorAll('selectorCSS')
```

**📤 Retorna:** NodeList (estático, no se actualiza automáticamente)

**💡 Ejemplos:**
```html
<ul id="menu">
  <li class="item">Item 1</li>
  <li class="item active">Item 2</li>
  <li class="item">Item 3</li>
</ul>
```

```javascript
// Seleccionar todos los elementos con clase 'item'
const items = document.querySelectorAll('.item');
console.log(items.length); // 3

// Iterar sobre los elementos
items.forEach(item => {
  console.log(item.textContent);
});

// Convertir a array
const itemsArray = [...items];
const textos = itemsArray.map(item => item.textContent);

// Selectores complejos
const menuItems = document.querySelectorAll('#menu .item');
const activeItems = document.querySelectorAll('.item.active');
const allParagraphs = document.querySelectorAll('div p');

// Filtrar después de seleccionar
const inactiveItems = [...items].filter(item => 
  !item.classList.contains('active')
);
```

**⚠️ Notas importantes:**
- Retorna NodeList, que tiene `.forEach()` pero no `.map()` ni `.filter()`
- Es una colección "estática" (no se actualiza si el DOM cambia)
- Puedes convertirla a array con `[...nodeList]` o `Array.from(nodeList)`

---

## Selección desde un elemento específico

Todos los métodos anteriores también funcionan desde un elemento específico, no solo desde `document`:

```html
<div id="container">
  <div class="box">Box 1</div>
  <div class="box">Box 2</div>
</div>
<div class="box">Box 3</div>
```

```javascript
const container = document.getElementById('container');

// Buscar solo dentro del container
const boxes = container.querySelectorAll('.box');
console.log(boxes.length); // 2 (no incluye Box 3)

const firstBox = container.querySelector('.box');
console.log(firstBox.textContent); // 'Box 1'
```

---

## Navegación por el DOM

### Propiedades de navegación

```javascript
const element = document.querySelector('.item');

// Padres
element.parentElement           // Elemento padre directo
element.parentNode             // Nodo padre (incluye text nodes)
element.closest('.container')   // Ancestro más cercano que coincida

// Hijos
element.children               // HTMLCollection de hijos (solo elementos)
element.childNodes             // NodeList de hijos (incluye text nodes)
element.firstElementChild      // Primer hijo (elemento)
element.lastElementChild       // Último hijo (elemento)

// Hermanos
element.nextElementSibling     // Siguiente hermano (elemento)
element.previousElementSibling // Hermano anterior (elemento)
```

**💡 Ejemplo práctico:**
```html
<ul id="list">
  <li class="item">Item 1</li>
  <li class="item active">Item 2</li>
  <li class="item">Item 3</li>
</ul>
```

```javascript
const activeItem = document.querySelector('.active');

// Ir al padre
const list = activeItem.parentElement;
console.log(list.id); // 'list'

// Ir al siguiente hermano
const nextItem = activeItem.nextElementSibling;
console.log(nextItem.textContent); // 'Item 3'

// Ir al hermano anterior
const prevItem = activeItem.previousElementSibling;
console.log(prevItem.textContent); // 'Item 1'

// Buscar ancestro con clase específica
const container = activeItem.closest('.container');
```

---

## Verificar si un elemento existe

```javascript
const element = document.querySelector('.no-existe');

if (element) {
  // El elemento existe
  console.log('Encontrado');
} else {
  // El elemento no existe
  console.log('No encontrado');
}

// Manera moderna con optional chaining
element?.classList.add('active'); // No da error si element es null
```

---

## 📊 Comparación de Métodos

| Método | Selectores | Retorna | Vivo/Estático | Velocidad |
|--------|-----------|---------|---------------|-----------|
| `getElementById()` | Solo ID | Elemento | - | ⚡⚡⚡ Muy rápido |
| `getElementsByClassName()` | Solo clases | HTMLCollection | Vivo | ⚡⚡ Rápido |
| `getElementsByTagName()` | Solo etiquetas | HTMLCollection | Vivo | ⚡⚡ Rápido |
| `querySelector()` | CSS completo | Elemento | - | ⚡ Moderado |
| `querySelectorAll()` | CSS completo | NodeList | Estático | ⚡ Moderado |

---

## ✅ Mejores Prácticas

```javascript
// ✅ Bueno - Específico y rápido
const header = document.getElementById('header');

// ✅ Bueno - Flexible y moderno
const buttons = document.querySelectorAll('.btn');

// ❌ Malo - Muy general
const allDivs = document.getElementsByTagName('div');

// ✅ Bueno - Guardar referencia si se usa múltiples veces
const container = document.querySelector('.container');
const items = container.querySelectorAll('.item');

// ❌ Malo - Buscar en el DOM repetidamente
for (let i = 0; i < 10; i++) {
  document.querySelector('.item').textContent = i; // Busca cada vez
}

// ✅ Bueno - Buscar una vez
const item = document.querySelector('.item');
for (let i = 0; i < 10; i++) {
  item.textContent = i;
}
```

---

## 🔥 Ejemplos Prácticos

### Ejemplo 1: Resaltar todos los párrafos
```javascript
const paragraphs = document.querySelectorAll('p');
paragraphs.forEach(p => {
  p.style.backgroundColor = 'yellow';
});
```

### Ejemplo 2: Contar elementos
```javascript
const items = document.querySelectorAll('.item');
console.log(`Total de items: ${items.length}`);
```

### Ejemplo 3: Buscar dentro de un contenedor específico
```javascript
const sidebar = document.querySelector('.sidebar');
const sidebarLinks = sidebar.querySelectorAll('a');
console.log(`Links en sidebar: ${sidebarLinks.length}`);
```

### Ejemplo 4: Seleccionar y modificar
```javascript
// Seleccionar todos los botones
const buttons = document.querySelectorAll('button');

// Agregar clase a cada uno
buttons.forEach(btn => {
  btn.classList.add('styled-button');
});
```

---

[⬅️ Volver al inicio](../README.md) | [➡️ Siguiente: Manipulación del DOM](./events.md)