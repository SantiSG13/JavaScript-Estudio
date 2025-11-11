# 🔥 Ejemplos Prácticos del DOM

## 1. Todo List (Lista de Tareas)

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Todo List</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: Arial, sans-serif; padding: 20px; background: #f5f5f5; }
    .container { max-width: 600px; margin: 0 auto; background: white; padding: 30px; border-radius: 10px; }
    h1 { margin-bottom: 20px; color: #333; }
    .input-container { display: flex; gap: 10px; margin-bottom: 20px; }
    input { flex: 1; padding: 10px; border: 2px solid #ddd; border-radius: 5px; font-size: 16px; }
    button { padding: 10px 20px; background: #4CAF50; color: white; border: none; border-radius: 5px; cursor: pointer; }
    button:hover { background: #45a049; }
    .task-list { list-style: none; }
    .task-item { display: flex; align-items: center; padding: 15px; margin-bottom: 10px; background: #f9f9f9; border-radius: 5px; }
    .task-item.completed { text-decoration: line-through; opacity: 0.6; }
    .task-item input[type="checkbox"] { margin-right: 10px; cursor: pointer; }
    .task-text { flex: 1; }
    .delete-btn { background: #f44336; padding: 5px 10px; font-size: 12px; }
    .delete-btn:hover { background: #da190b; }
  </style>
</head>
<body>
  <div class="container">
    <h1>📝 Todo List</h1>
    
    <div class="input-container">
      <input type="text" id="taskInput" placeholder="Agregar nueva tarea...">
      <button id="addBtn">Agregar</button>
    </div>
    
    <ul id="taskList" class="task-list"></ul>
  </div>

  <script>
    // Selectores
    const taskInput = document.getElementById('taskInput');
    const addBtn = document.getElementById('addBtn');
    const taskList = document.getElementById('taskList');

    // Cargar tareas del localStorage
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];

    // Función para renderizar tareas
    function renderTasks() {
      taskList.innerHTML = '';
      
      tasks.forEach((task, index) => {
        const li = document.createElement('li');
        li.className = `task-item ${task.completed ? 'completed' : ''}`;
        li.innerHTML = `
          <input type="checkbox" ${task.completed ? 'checked' : ''} data-index="${index}">
          <span class="task-text">${task.text}</span>
          <button class="delete-btn" data-index="${index}">Eliminar</button>
        `;
        taskList.appendChild(li);
      });
    }

    // Agregar tarea
    function addTask() {
      const text = taskInput.value.trim();
      
      if (text === '') {
        alert('Por favor escribe una tarea');
        return;
      }
      
      tasks.push({ text, completed: false });
      saveTasks();
      renderTasks();
      taskInput.value = '';
      taskInput.focus();
    }

    // Guardar en localStorage
    function saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }

    // Event listeners
    addBtn.addEventListener('click', addTask);

    taskInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        addTask();
      }
    });

    // Event delegation para checkbox y delete
    taskList.addEventListener('click', (e) => {
      const index = e.target.dataset.index;
      
      // Toggle completed
      if (e.target.type === 'checkbox') {
        tasks[index].completed = e.target.checked;
        saveTasks();
        renderTasks();
      }
      
      // Delete task
      if (e.target.classList.contains('delete-btn')) {
        tasks.splice(index, 1);
        saveTasks();
        renderTasks();
      }
    });

    // Renderizar tareas iniciales
    renderTasks();
  </script>
</body>
</html>
```

---

## 2. Modal / Popup

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Modal Example</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: Arial, sans-serif; padding: 50px; }
    
    .modal-overlay {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.7);
      z-index: 999;
    }
    
    .modal {
      display: none;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: white;
      padding: 30px;
      border-radius: 10px;
      max-width: 500px;
      width: 90%;
      z-index: 1000;
      animation: slideDown 0.3s ease;
    }
    
    @keyframes slideDown {
      from {
        opacity: 0;
        transform: translate(-50%, -60%);
      }
      to {
        opacity: 1;
        transform: translate(-50%, -50%);
      }
    }
    
    .modal.active,
    .modal-overlay.active {
      display: block;
    }
    
    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }
    
    .close-btn {
      background: none;
      border: none;
      font-size: 24px;
      cursor: pointer;
      color: #999;
    }
    
    .close-btn:hover {
      color: #333;
    }
    
    button {
      padding: 10px 20px;
      background: #4CAF50;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }
    
    button:hover {
      background: #45a049;
    }
  </style>
</head>
<body>
  <h1>Ejemplo de Modal</h1>
  <button id="openModal">Abrir Modal</button>
  
  <div class="modal-overlay" id="overlay"></div>
  
  <div class="modal" id="modal">
    <div class="modal-header">
      <h2>Título del Modal</h2>
      <button class="close-btn" id="closeModal">&times;</button>
    </div>
    <div class="modal-body">
      <p>Este es el contenido del modal.</p>
      <p>Puedes cerrarlo con:</p>
      <ul>
        <li>El botón X</li>
        <li>Click en el overlay</li>
        <li>Tecla Escape</li>
      </ul>
    </div>
  </div>

  <script>
    const openBtn = document.getElementById('openModal');
    const closeBtn = document.getElementById('closeModal');
    const modal = document.getElementById('modal');
    const overlay = document.getElementById('overlay');

    function openModal() {
      modal.classList.add('active');
      overlay.classList.add('active');
      document.body.style.overflow = 'hidden'; // Prevenir scroll
    }

    function closeModal() {
      modal.classList.remove('active');
      overlay.classList.remove('active');
      document.body.style.overflow = ''; // Restaurar scroll
    }

    // Abrir modal
    openBtn.addEventListener('click', openModal);

    // Cerrar con botón X
    closeBtn.addEventListener('click', closeModal);

    