# 🛠️ Manipulación del DOM

## Modificar Contenido

### textContent

**📝 Descripción:** Obtiene o establece el texto de un elemento (sin HTML).

**💡 Ejemplos:**
```html
<div id="message">Hola Mundo</div>
```

```javascript
const message = document.getElementById('message');

// Leer contenido
console.log(message.textContent); // 'Hola Mundo'

// Cambiar contenido
message.textContent = 'Nuevo mensaje';

// Limpiar contenido
message.textContent = '';
```

**⚠️ Nota:** Es más seguro que `innerHTML` porque no interpreta HTML.

---

### innerHTML

**📝 Descripción:** Obtiene o establece el contenido HTML de un elemento.

**💡 Ejemplos:**
```html
<div id="container"></div>
```

```javascript
const container = document.getElementById('container');

// Agregar HTML
container.innerHTML = '<h1>Título</h1><p>Párrafo</p>';

// Agregar más contenido (reemplaza todo)
container.innerHTML = container.innerHTML + '<button>Click</button>';

// Mejor forma de agregar (con template literals)
container.innerHTML = `
  <div class="card">
    <h2>Título</h2>
    <p>Descripción</p>
    <button>Ver más</button>
  </div>
`;
```

**⚠️ Advertencia:** 
- Puede ser vulnerable a ataques XSS si usas contenido de usuario
- Reemplaza todo el contenido (pierde event listeners)

---

### innerText

**📝 Descripción:** Similar a `textContent` pero respeta estilos CSS (no muestra elementos ocultos).

**💡 Ejemplo:**
```html
<div id="box">
  Texto visible
  <span style="display: none;">Texto oculto</span>
</div>
```

```javascript
const box = document.getElementById('box');
console.log(box.textContent); // 'Texto visible Texto oculto'
console.log(box.innerText);   // 'Texto visible'
```

---

## Modificar Atributos

### getAttribute() y setAttribute()

**📝 Descripción:** Lee y modifica atributos de elementos.

**💡 Ejemplos:**
```html
<img id="photo" src="old.jpg" alt="Foto antigua">
<a id="link" href="https://example.com">Enlace</a>
```

```javascript
const photo = document.getElementById('photo');
const link = document.getElementById('link');

// Leer atributos
console.log(photo.getAttribute('src'));  // 'old.jpg'
console.log(photo.getAttribute('alt'));  // 'Foto antigua'

// Modificar atributos
photo.setAttribute('src', 'new.jpg');
photo.setAttribute('alt', 'Foto nueva');

// Agregar nuevos atributos
link.setAttribute('target', '_blank');
link.setAttribute('rel', 'noopener');

// Verificar si existe un atributo
if (photo.hasAttribute('alt')) {
  console.log('Tiene atributo alt');
}

// Eliminar atributo
photo.removeAttribute('alt');
```

---

### Propiedades directas

**💡 Ejemplos:**
```javascript
const input = document.querySelector('input');
const link = document.querySelector('a');
const img = document.querySelector('img');

// Acceso directo (más común)
input.value = 'Nuevo valor';
input.type = 'email';
input.placeholder = 'Tu email';
input.disabled = true;

link.href = 'https://google.com';
link.target = '_blank';

img.src = 'image.jpg';
img.alt = 'Descripción';
```

---

### Data Attributes

**📝 Descripción:** Atributos personalizados que comienzan con `data-`.

**💡 Ejemplos:**
```html
<button 
  id="btn" 
  data-user-id="123" 
  data-action="delete"
  data-confirm-message="¿Estás seguro?">
  Eliminar
</button>
```

```javascript
const btn = document.getElementById('btn');

// Acceder con dataset (convierte kebab-case a camelCase)
console.log(btn.dataset.userId);         // '123'
console.log(btn.dataset.action);         // 'delete'
console.log(btn.dataset.confirmMessage); // '¿Estás seguro?'

// Modificar
btn.dataset.userId = '456';
btn.dataset.status = 'active'; // Crea data-status="active"

// Eliminar
delete btn.dataset.action;
```

---

## Modificar Clases CSS

### className

**📝 Descripción:** Obtiene o establece todas las clases como un string.

**💡 Ejemplos:**
```html
<div id="box" class="card active"></div>
```

```javascript
const box = document.getElementById('box');

// Leer clases
console.log(box.className); // 'card active'

// Reemplazar todas las clases
box.className = 'card inactive';

// Agregar clase (manera antigua)
box.className = box.className + ' highlight';
```

**⚠️ Nota:** Usar `classList` es mejor en la mayoría de casos.

---

### classList (Recomendado)

**📝 Descripción:** Proporciona métodos para manipular clases fácilmente.

**💡 Métodos:**
```javascript
const element = document.querySelector('.box');

// add() - Agregar una o más clases
element.classList.add('active');
element.classList.add('highlight', 'important');

// remove() - Eliminar una o más clases
element.classList.remove('active');
element.classList.remove('highlight', 'important');

// toggle() - Alternar clase (agregar si no existe, eliminar si existe)
element.classList.toggle('active');
element.classList.toggle('active'); // La quita
element.classList.toggle('active'); // La agrega

// contains() - Verificar si tiene una clase
if (element.classList.contains('active')) {
  console.log('El elemento está activo');
}

// replace() - Reemplazar una clase por otra
element.classList.replace('old-class', 'new-class');
```

**🔥 Ejemplos prácticos:**
```javascript
// Ejemplo 1: Toggle de menú
const menuBtn = document.querySelector('.menu-btn');
const menu = document.querySelector('.menu');

menuBtn.addEventListener('click', () => {
  menu.classList.toggle('open');
});

// Ejemplo 2: Activar/desactivar tabs
const tabs = document.querySelectorAll('.tab');

tabs.forEach(tab => {
  tab.addEventListener('click', () => {
    // Remover 'active' de todos
    tabs.forEach(t => t.classList.remove('active'));
    // Agregar 'active' al clickeado
    tab.classList.add('active');
  });
});

// Ejemplo 3: Validación de formulario
const input = document.querySelector('input');

input.addEventListener('blur', () => {
  if (input.value.length < 3) {
    input.classList.add('error');
    input.classList.remove('success');
  } else {
    input.classList.add('success');
    input.classList.remove('error');
  }
});
```

---

## Modificar Estilos CSS

### style (Estilos inline)

**📝 Descripción:** Modifica estilos CSS directamente en el elemento.

**💡 Ejemplos:**
```javascript
const box = document.querySelector('.box');

// Modificar estilos individuales
box.style.color = 'red';
box.style.backgroundColor = 'blue';  // camelCase para propiedades con guión
box.style.fontSize = '20px';
box.style.border = '2px solid black';
box.style.display = 'none';

// Múltiples estilos
box.style.cssText = `
  color: red;
  background-color: blue;
  font-size: 20px;
  padding: 10px;
`;

// Leer estilos
console.log(box.style.color); // 'red'

// Remover estilo
box.style.backgroundColor = '';
```

**⚠️ Notas:**
- Usa camelCase para propiedades CSS (`background-color` → `backgroundColor`)
- Solo lee estilos inline, no estilos de CSS externo
- Mejor usar clases CSS cuando sea posible

---

### getComputedStyle()

**📝 Descripción:** Obtiene todos los estilos aplicados al elemento (incluye CSS externo).

**💡 Ejemplos:**
```html
<style>
  .box {
    color: blue;
    font-size: 16px;
  }
</style>
<div class="box" style="color: red;"></div>
```

```javascript
const box = document.querySelector('.box');
const styles = getComputedStyle(box);

console.log(styles.color);      // 'rgb(255, 0, 0)' (rojo, estilo inline)
console.log(styles.fontSize);   // '16px' (del CSS)
console.log(styles.display);    // 'block' (valor por defecto)

// Obtener propiedad específica
const bgColor = styles.getPropertyValue('background-color');
```

---

## Crear Elementos

### createElement()

**📝 Descripción:** Crea un nuevo elemento HTML.

**💡 Ejemplos:**
```javascript
// Crear elemento
const div = document.createElement('div');
const p = document.createElement('p');
const button = document.createElement('button');

// Configurar el elemento
div.className = 'card';
div.id = 'main-card';
div.textContent = 'Contenido';

// Crear elemento más complejo
const card = document.createElement('div');
card.classList.add('card', 'highlight');
card.innerHTML = `
  <h2>Título</h2>
  <p>Descripción</p>
  <button>Ver más</button>
`;

// Agregar atributos
const img = document.createElement('img');
img.src = 'photo.jpg';
img.alt = 'Descripción';
img.setAttribute('loading', 'lazy');
```

---

## Agregar Elementos al DOM

### appendChild()

**📝 Descripción:** Agrega un elemento como último hijo.

**💡 Ejemplos:**
```javascript
const container = document.querySelector('.container');
const newDiv = document.createElement('div');
newDiv.textContent = 'Nuevo elemento';

// Agregar al final
container.appendChild(newDiv);
```

---

### append()

**📝 Descripción:** Agrega uno o más elementos/texto al final (más flexible que appendChild).

**💡 Ejemplos:**
```javascript
const container = document.querySelector('.container');

// Agregar elemento
const div = document.createElement('div');
container.append(div);

// Agregar múltiples elementos
const p1 = document.createElement('p');
const p2 = document.createElement('p');
container.append(p1, p2);

// Agregar texto directamente
container.append('Texto simple');

// Mezclar elementos y texto
container.append('Título: ', div, ' - Fin');
```

---

### prepend()

**📝 Descripción:** Agrega elementos al inicio.

**💡 Ejemplos:**
```javascript
const list = document.querySelector('ul');
const newItem = document.createElement('li');
newItem.textContent = 'Primer item';

list.prepend(newItem); // Agrega al inicio
```

---

### insertBefore()

**📝 Descripción:** Inserta un elemento antes de otro elemento específico.

**💡 Ejemplos:**
```javascript
const list = document.querySelector('ul');
const newItem = document.createElement('li');
newItem.textContent = 'Item insertado';

const thirdItem = list.children[2];
list.insertBefore(newItem, thirdItem);
```

---

### insertAdjacentHTML()

**📝 Descripción:** Inserta HTML en una posición específica.

**💡 Posiciones:**
- `'beforebegin'`: Antes del elemento
- `'afterbegin'`: Dentro del elemento, antes del primer hijo
- `'beforeend'`: Dentro del elemento, después del último hijo
- `'afterend'`: Después del elemento

**💡 Ejemplos:**
```html
<div id="container">
  <p>Contenido existente</p>
</div>
```

```javascript
const container = document.getElementById('container');

// Antes del elemento
container.insertAdjacentHTML('beforebegin', '<h1>Título antes</h1>');

// Al inicio dentro del elemento
container.insertAdjacentHTML('afterbegin', '<p>Primer párrafo</p>');

// Al final dentro del elemento
container.insertAdjacentHTML('beforeend', '<p>Último párrafo</p>');

// Después del elemento
container.insertAdjacentHTML('afterend', '<footer>Footer</footer>');

/* Resultado:
<h1>Título antes</h1>
<div id="container">
  <p>Primer párrafo</p>
  <p>Contenido existente</p>
  <p>Último párrafo</p>
</div>
<footer>Footer</footer>
*/
```

---

## Eliminar Elementos

### remove()

**📝 Descripción:** Elimina el elemento del DOM.

**💡 Ejemplos:**
```javascript
const element = document.querySelector('.to-remove');
element.remove(); // Elimina el elemento
```

---

### removeChild()

**📝 Descripción:** Elimina un hijo específico.

**💡 Ejemplos:**
```javascript
const parent = document.querySelector('.parent');
const child = parent.querySelector('.child');

parent.removeChild(child);
```

---

### Vaciar contenido

**💡 Ejemplos:**
```javascript
const container = document.querySelector('.container');

// Opción 1: innerHTML
container.innerHTML = '';

// Opción 2: textContent
container.textContent = '';

// Opción 3: Eliminar hijos uno por uno
while (container.firstChild) {
  container.removeChild(container.firstChild);
}

// Opción 4: Moderna
container.replaceChildren(); // Elimina todos los hijos
```

---

## Clonar Elementos

### cloneNode()

**📝 Descripción:** Crea una copia de un elemento.

**💡 Ejemplos:**
```javascript
const original = document.querySelector('.card');

// Clonar solo el elemento (sin hijos)
const shallowClone = original.cloneNode();

// Clonar elemento con todos sus hijos (más común)
const deepClone = original.cloneNode(true);

// Agregar el clon al DOM
document.body.appendChild(deepClone);
```

**⚠️ Nota:** Los event listeners no se clonan.

---

## Reemplazar Elementos

### replaceChild()

**💡 Ejemplos:**
```javascript
const parent = document.querySelector('.parent');
const oldChild = parent.querySelector('.old');
const newChild = document.createElement('div');
newChild.textContent = 'Nuevo contenido';

parent.replaceChild(newChild, oldChild);
```

---

### replaceWith()

**💡 Ejemplos:**
```javascript
const oldElement = document.querySelector('.old');
const newElement = document.createElement('div');
newElement.textContent = 'Reemplazo';

oldElement.replaceWith(newElement);
```

---

## 🔥 Ejemplos Prácticos Completos

### Ejemplo 1: Crear y agregar una tarjeta
```javascript
function createCard(title, description) {
  const card = document.createElement('div');
  card.className = 'card';
  
  card.innerHTML = `
    <h3>${title}</h3>
    <p>${description}</p>
    <button class="btn">Ver más</button>
  `;
  
  document.querySelector('.container').appendChild(card);
}

createCard('Título 1', 'Descripción de la tarjeta');
```

### Ejemplo 2: Lista dinámica
```javascript
const tasks = ['Tarea 1', 'Tarea 2', 'Tarea 3'];
const ul = document.querySelector('#task-list');

tasks.forEach(task => {
  const li = document.createElement('li');
  li.textContent = task;
  li.classList.add('task-item');
  ul.appendChild(li);
});
```

### Ejemplo 3: Toggle de dark mode
```javascript
const toggleBtn = document.querySelector('.theme-toggle');
const body = document.body;

toggleBtn.addEventListener('click', () => {
  body.classList.toggle('dark-mode');
  
  const isDark = body.classList.contains('dark-mode');
  toggleBtn.textContent = isDark ? '☀️ Light' : '🌙 Dark';
});
```

---

[⬅️ Anterior: Selectores](./events.md) | [➡️ Siguiente: Eventos](./examples.md) | [🏠 Inicio](../README.md)