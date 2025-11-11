# ⚡ Eventos del DOM

## ¿Qué son los Eventos?

Los eventos son acciones o sucesos que ocurren en el navegador: clicks, movimientos del mouse, pulsaciones de teclas, etc. JavaScript puede "escuchar" estos eventos y ejecutar código en respuesta.

---

## addEventListener()

**📝 Descripción:** El método moderno para escuchar eventos. Permite agregar múltiples listeners al mismo elemento.

**✍️ Sintaxis:**
```javascript
element.addEventListener(evento, función, opciones)
```

**📥 Parámetros:**
- `evento`: Tipo de evento ('click', 'submit', 'keydown', etc.)
- `función`: Función que se ejecuta cuando ocurre el evento
- `opciones` (opcional): Objeto con configuraciones

---

## Eventos de Mouse

### click

**📝 Descripción:** Se dispara cuando se hace click en un elemento.

**💡 Ejemplos:**
```html
<button id="btn">Click me</button>
<div id="box" class="box">Click en el div</div>
```

```javascript
const btn = document.getElementById('btn');

// Forma básica
btn.addEventListener('click', function() {
  console.log('Click!');
});

// Arrow function (más común)
btn.addEventListener('click', () => {
  console.log('Click con arrow function');
});

// Con objeto de evento
btn.addEventListener('click', (event) => {
  console.log('Evento:', event);
  console.log('Elemento clickeado:', event.target);
});

// Función externa
function handleClick() {
  console.log('Función externa');
}
btn.addEventListener('click', handleClick);
```

---

### dblclick

**📝 Descripción:** Doble click.

**💡 Ejemplo:**
```javascript
const box = document.querySelector('.box');

box.addEventListener('dblclick', () => {
  console.log('Doble click!');
  box.style.backgroundColor = 'red';
});
```

---

### mouseenter y mouseleave

**📝 Descripción:** 
- `mouseenter`: Mouse entra al elemento
- `mouseleave`: Mouse sale del elemento

**💡 Ejemplos:**
```javascript
const box = document.querySelector('.box');

box.addEventListener('mouseenter', () => {
  box.style.backgroundColor = 'lightblue';
  console.log('Mouse entró');
});

box.addEventListener('mouseleave', () => {
  box.style.backgroundColor = 'white';
  console.log('Mouse salió');
});
```

---

### mouseover y mouseout

**📝 Descripción:** Similar a mouseenter/mouseleave pero se dispara también en elementos hijos.

**💡 Ejemplo:**
```javascript
const container = document.querySelector('.container');

container.addEventListener('mouseover', (e) => {
  console.log('Over:', e.target);
});

container.addEventListener('mouseout', (e) => {
  console.log('Out:', e.target);
});
```

---

### mousemove

**📝 Descripción:** Se dispara mientras el mouse se mueve sobre el elemento.

**💡 Ejemplo:**
```javascript
const box = document.querySelector('.box');

box.addEventListener('mousemove', (e) => {
  // Posición del mouse relativa a la ventana
  console.log(`X: ${e.clientX}, Y: ${e.clientY}`);
  
  // Posición relativa al elemento
  console.log(`Offset X: ${e.offsetX}, Offset Y: ${e.offsetY}`);
});
```

**🔥 Ejemplo práctico - Seguir el mouse:**
```javascript
const cursor = document.querySelector('.cursor');

document.addEventListener('mousemove', (e) => {
  cursor.style.left = e.clientX + 'px';
  cursor.style.top = e.clientY + 'px';
});
```

---

### mousedown y mouseup

**📝 Descripción:**
- `mousedown`: Cuando se presiona el botón del mouse
- `mouseup`: Cuando se suelta el botón del mouse

**💡 Ejemplo - Drag simple:**
```javascript
const box = document.querySelector('.box');
let isDragging = false;

box.addEventListener('mousedown', () => {
  isDragging = true;
  box.style.cursor = 'grabbing';
});

document.addEventListener('mouseup', () => {
  isDragging = false;
  box.style.cursor = 'grab';
});

document.addEventListener('mousemove', (e) => {
  if (isDragging) {
    box.style.left = e.clientX + 'px';
    box.style.top = e.clientY + 'px';
  }
});
```

---

## Eventos de Teclado

### keydown

**📝 Descripción:** Se dispara cuando se presiona una tecla.

**💡 Ejemplos:**
```javascript
// Escuchar en todo el documento
document.addEventListener('keydown', (e) => {
  console.log('Tecla presionada:', e.key);
  console.log('Código:', e.code);
  console.log('Ctrl presionado:', e.ctrlKey);
  console.log('Shift presionado:', e.shiftKey);
});

// Detectar teclas específicas
document.addEventListener('keydown', (e) => {
  if (e.key === 'Enter') {
    console.log('Enter presionado');
  }
  
  if (e.key === 'Escape') {
    console.log('Escape presionado');
  }
  
  if (e.code === 'Space') {
    e.preventDefault(); // Evitar scroll
    console.log('Espacio presionado');
  }
});

// Combinaciones de teclas
document.addEventListener('keydown', (e) => {
  // Ctrl + S
  if (e.ctrlKey && e.key === 's') {
    e.preventDefault();
    console.log('Guardar (Ctrl+S)');
  }
  
  // Ctrl + Shift + P
  if (e.ctrlKey && e.shiftKey && e.key === 'P') {
    console.log('Command Palette');
  }
});
```

**🔥 Ejemplo práctico - Navegación con flechas:**
```javascript
const player = document.querySelector('.player');
let x = 0, y = 0;

document.addEventListener('keydown', (e) => {
  const step = 10;
  
  switch(e.key) {
    case 'ArrowUp':
      y -= step;
      break;
    case 'ArrowDown':
      y += step;
      break;
    case 'ArrowLeft':
      x -= step;
      break;
    case 'ArrowRight':
      x += step;
      break;
  }
  
  player.style.transform = `translate(${x}px, ${y}px)`;
});
```

---

### keyup

**📝 Descripción:** Se dispara cuando se suelta una tecla.

**💡 Ejemplo:**
```javascript
const input = document.querySelector('input');

input.addEventListener('keyup', (e) => {
  console.log('Valor actual:', e.target.value);
});
```

---

### keypress (Deprecated)

**⚠️ Nota:** Este evento está obsoleto. Usar `keydown` en su lugar.

---

## Eventos de Formulario

### submit

**📝 Descripción:** Se dispara cuando se envía un formulario.

**💡 Ejemplos:**
```html
<form id="myForm">
  <input type="text" name="username" required>
  <input type="email" name="email" required>
  <button type="submit">Enviar</button>
</form>
```

```javascript
const form = document.getElementById('myForm');

form.addEventListener('submit', (e) => {
  e.preventDefault(); // ¡IMPORTANTE! Evita recargar la página
  
  // Obtener datos del formulario
  const formData = new FormData(form);
  const username = formData.get('username');
  const email = formData.get('email');
  
  console.log('Username:', username);
  console.log('Email:', email);
  
  // O usar FormData directamente
  for (let [key, value] of formData.entries()) {
    console.log(`${key}: ${value}`);
  }
});
```

---

### input

**📝 Descripción:** Se dispara cada vez que el valor de un input cambia.

**💡 Ejemplos:**
```javascript
const input = document.querySelector('input');

// Buscar en tiempo real
input.addEventListener('input', (e) => {
  const searchTerm = e.target.value;
  console.log('Buscando:', searchTerm);
  // Realizar búsqueda...
});

// Contador de caracteres
const textarea = document.querySelector('textarea');
const counter = document.querySelector('.counter');

textarea.addEventListener('input', (e) => {
  const length = e.target.value.length;
  counter.textContent = `${length} / 280`;
});
```

---

### change

**📝 Descripción:** Se dispara cuando el valor cambia y el elemento pierde el foco.

**💡 Ejemplos:**
```javascript
// Select
const select = document.querySelector('select');
select.addEventListener('change', (e) => {
  console.log('Opción seleccionada:', e.target.value);
});

// Checkbox
const checkbox = document.querySelector('input[type="checkbox"]');
checkbox.addEventListener('change', (e) => {
  console.log('Checked:', e.target.checked);
});

// Radio buttons
const radios = document.querySelectorAll('input[type="radio"]');
radios.forEach(radio => {
  radio.addEventListener('change', (e) => {
    if (e.target.checked) {
      console.log('Seleccionado:', e.target.value);
    }
  });
});
```

---

### focus y blur

**📝 Descripción:**
- `focus`: Elemento recibe el foco
- `blur`: Elemento pierde el foco

**💡 Ejemplos:**
```javascript
const input = document.querySelector('input');

input.addEventListener('focus', () => {
  input.style.borderColor = 'blue';
  console.log('Input tiene el foco');
});

input.addEventListener('blur', () => {
  input.style.borderColor = 'gray';
  console.log('Input perdió el foco');
  
  // Validar cuando pierde el foco
  if (input.value.length < 3) {
    input.classList.add('error');
  }
});
```

---

## Eventos de Ventana

### load

**📝 Descripción:** Se dispara cuando la página completa está cargada (HTML, CSS, imágenes, etc.).

**💡 Ejemplo:**
```javascript
window.addEventListener('load', () => {
  console.log('Página completamente cargada');
  // Todas las imágenes están cargadas
});
```

---

### DOMContentLoaded

**📝 Descripción:** Se dispara cuando el HTML está cargado (más rápido que `load`).

**💡 Ejemplo:**
```javascript
document.addEventListener('DOMContentLoaded', () => {
  console.log('DOM listo');
  // Las imágenes pueden no estar cargadas aún
  // Pero ya puedes manipular el DOM
});
```

---

### resize

**📝 Descripción:** Se dispara cuando se cambia el tamaño de la ventana.

**💡 Ejemplos:**
```javascript
window.addEventListener('resize', () => {
  console.log('Ancho:', window.innerWidth);
  console.log('Alto:', window.innerHeight);
});

// Con debounce (evitar demasiadas ejecuciones)
let resizeTimeout;
window.addEventListener('resize', () => {
  clearTimeout(resizeTimeout);
  resizeTimeout = setTimeout(() => {
    console.log('Resize finalizado');
  }, 250);
});
```

---

### scroll

**📝 Descripción:** Se dispara al hacer scroll.

**💡 Ejemplos:**
```javascript
window.addEventListener('scroll', () => {
  const scrollY = window.scrollY;
  console.log('Scroll position:', scrollY);
});

// Mostrar botón "volver arriba" al hacer scroll
const backToTop = document.querySelector('.back-to-top');

window.addEventListener('scroll', () => {
  if (window.scrollY > 300) {
    backToTop.classList.add('visible');
  } else {
    backToTop.classList.remove('visible');
  }
});

// Scroll en un elemento específico
const container = document.querySelector('.scrollable');
container.addEventListener('scroll', () => {
  console.log('Scroll en container:', container.scrollTop);
});
```

---

## Objeto Event

**📝 Propiedades importantes:**

```javascript
element.addEventListener('click', (e) => {
  // Elemento que disparó el evento
  console.log('Target:', e.target);
  
  // Elemento al que se le agregó el listener
  console.log('Current Target:', e.currentTarget);
  
  // Tipo de evento
  console.log('Type:', e.type); // 'click'
  
  // Posición del mouse
  console.log('Client X:', e.clientX);
  console.log('Client Y:', e.clientY);
  
  // Teclas modificadoras
  console.log('Ctrl:', e.ctrlKey);
  console.log('Shift:', e.shiftKey);
  console.log('Alt:', e.altKey);
  
  // Para eventos de teclado
  console.log('Key:', e.key);
  console.log('Code:', e.code);
});
```

---

## Métodos del Objeto Event

### preventDefault()

**📝 Descripción:** Previene el comportamiento por defecto del evento.

**💡 Ejemplos:**
```javascript
// Prevenir envío de formulario
form.addEventListener('submit', (e) => {
  e.preventDefault();
  console.log('Formulario no se envió');
});

// Prevenir click en enlace
link.addEventListener('click', (e) => {
  e.preventDefault();
  console.log('Enlace no navegó');
});

// Prevenir menú contextual
document.addEventListener('contextmenu', (e) => {
  e.preventDefault();
  console.log('Click derecho deshabilitado');
});
```

---

### stopPropagation()

**📝 Descripción:** Detiene la propagación del evento hacia arriba (bubbling).

**💡 Ejemplo:**
```html
<div class="parent">
  <button class="child">Click</button>
</div>
```

```javascript
const parent = document.querySelector('.parent');
const child = document.querySelector('.child');

parent.addEventListener('click', () => {
  console.log('Click en parent');
});

child.addEventListener('click', (e) => {
  e.stopPropagation(); // Evita que el evento llegue al parent
  console.log('Click en child');
});
// Al hacer click en el botón, solo se imprime "Click en child"
```

---

## Event Delegation (Delegación de Eventos)

**📝 Descripción:** Técnica de agregar un listener al padre para manejar eventos de los hijos.

**💡 Ventajas:**
- Mejor rendimiento (un solo listener en lugar de muchos)
- Funciona con elementos agregados dinámicamente

**🔥 Ejemplos prácticos:**

### Ejemplo 1: Lista de tareas
```html
<ul id="task-list">
  <li>Tarea 1 <button class="delete">X</button></li>
  <li>Tarea 2 <button class="delete">X</button></li>
  <li>Tarea 3 <button class="delete">X</button></li>
</ul>
```

```javascript
const taskList = document.getElementById('task-list');

// ❌ Mal - Listener en cada botón
/*
const deleteButtons = document.querySelectorAll('.delete');
deleteButtons.forEach(btn => {
  btn.addEventListener('click', function() {
    this.parentElement.remove();
  });
});
*/

// ✅ Bien - Event delegation
taskList.addEventListener('click', (e) => {
  if (e.target.classList.contains('delete')) {
    e.target.parentElement.remove();
  }
});

// Ahora funciona incluso con elementos agregados dinámicamente
const addButton = document.querySelector('.add-task');
addButton.addEventListener('click', () => {
  const li = document.createElement('li');
  li.innerHTML = 'Nueva tarea <button class="delete">X</button>';
  taskList.appendChild(li);
  // El botón delete automáticamente tiene el evento
});
```

### Ejemplo 2: Galería de imágenes
```javascript
const gallery = document.querySelector('.gallery');

gallery.addEventListener('click', (e) => {
  if (e.target.tagName === 'IMG') {
    const src = e.target.src;
    openLightbox(src);
  }
});
```

---

## removeEventListener()

**📝 Descripción:** Elimina un event listener.

**⚠️ Importante:** Debes usar la misma función que usaste en `addEventListener`.

**💡 Ejemplos:**
```javascript
// ❌ No funciona - funciones diferentes
btn.addEventListener('click', () => {
  console.log('Click');
});
btn.removeEventListener('click', () => {
  console.log('Click');
});

// ✅ Funciona - misma función
function handleClick() {
  console.log('Click');
}
btn.addEventListener('click', handleClick);
btn.removeEventListener('click', handleClick);

// Ejemplo práctico: Click una sola vez
function handleOnce() {
  console.log('Solo una vez');
  btn.removeEventListener('click', handleOnce);
}
btn.addEventListener('click', handleOnce);

// Alternativa moderna: { once: true }
btn.addEventListener('click', () => {
  console.log('Solo una vez');
}, { once: true });
```

---

## Opciones de addEventListener

```javascript
element.addEventListener('click', handler, {
  capture: false,   // false = bubbling (default), true = capturing
  once: true,       // Se ejecuta solo una vez y se remueve automáticamente
  passive: true     // No puede llamar preventDefault() (mejor rendimiento)
});

// Ejemplos prácticos:

// Solo una vez
btn.addEventListener('click', () => {
  console.log('Solo una vez');
}, { once: true });

// Passive (para scroll)
window.addEventListener('scroll', () => {
  // No usa preventDefault()
}, { passive: true });
```

---

## 🔥 Ejemplos Prácticos Completos

### Ejemplo 1: Formulario de login con validación
```javascript
const form = document.querySelector('#login-form');
const email = document.querySelector('#email');
const password = document.querySelector('#password');
const errorMsg = document.querySelector('.error-message');

form.addEventListener('submit', (e) => {
  e.preventDefault();
  
  // Validar
  if (!email.value.includes('@')) {
    errorMsg.textContent = 'Email inválido';
    email.classList.add('error');
    return;
  }
  
  if (password.value.length < 6) {
    errorMsg.textContent = 'Password debe tener al menos 6 caracteres';
    password.classList.add('error');
    return;
  }
  
  // Enviar
  console.log('Enviando formulario...');
  errorMsg.textContent = '';
});

// Limpiar errores al escribir
[email, password].forEach(input => {
  input.addEventListener('input', () => {
    input.classList.remove('error');
    errorMsg.textContent = '';
  });
});
```

### Ejemplo 2: Modal con escape
```javascript
const modal = document.querySelector('.modal');
const openBtn = document.querySelector('.open-modal');
const closeBtn = document.querySelector('.close-modal');
const overlay = document.querySelector('.overlay');

function openModal() {
  modal.classList.add('active');
  overlay.classList.add('active');
}

function closeModal() {
  modal.classList.remove('active');
  overlay.classList.remove('active');
}

openBtn.addEventListener('click', openModal);
closeBtn.addEventListener('click', closeModal);
overlay.addEventListener('click', closeModal);

// Cerrar con Escape
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape' && modal.classList.contains('active')) {
    closeModal();
  }
});
```

### Ejemplo 3: Búsqueda en tiempo real con debounce
```javascript
const searchInput = document.querySelector('#search');
const results = document.querySelector('.results');
let timeout;

searchInput.addEventListener('input', (e) => {
  clearTimeout(timeout);
  
  timeout = setTimeout(() => {
    const query = e.target.value;
    
    if (query.length > 2) {
      // Realizar búsqueda
      console.log('Buscando:', query);
      // fetch(`/api/search?q=${query}`)...
    }
  }, 500); // Espera 500ms después de que el usuario deje de escribir
});
```

---

## 📊 Tabla de Eventos Comunes

| Evento | Descripción | Cancelable |
|--------|-------------|------------|
| `click` | Click del mouse | ✅ |
| `dblclick` | Doble click | ✅ |
| `mouseenter` | Mouse entra | ❌ |
| `mouseleave` | Mouse sale | ❌ |
| `mousemove` | Mouse se mueve | ✅ |
| `keydown` | Tecla presionada | ✅ |
| `keyup` | Tecla soltada | ✅ |
| `submit` | Formulario enviado | ✅ |
| `input` | Input cambia | ❌ |
| `change` | Valor cambia (blur) | ❌ |
| `focus` | Elemento recibe foco | ❌ |
| `blur` | Elemento pierde foco | ❌ |
| `scroll` | Scroll | ❌ |
| `resize` | Ventana redimensionada | ❌ |
| `load` | Recurso cargado | ❌ |
| `DOMContentLoaded` | DOM listo | ❌ |

---

[⬅️ Anterior: Manipulación](./selectors.md) | [➡️ Siguiente: Ejemplos](./manipulation.md) | [🏠 Inicio](../README.md)