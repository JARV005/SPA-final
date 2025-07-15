## 4. Crear Evento (/dashboard/events/create)
Solo para administradores.

🔸 src/views/create-event.html
----------------------------------
<div class="container mt-5">
  <h2>Crear Evento</h2>
  <form id="createEventForm">
    <div class="mb-3">
      <label class="form-label">Nombre del Evento</label>
      <input type="text" class="form-control" id="nombre" required />
    </div>
    <div class="mb-3">
      <label class="form-label">Lugar</label>
      <input type="text" class="form-control" id="lugar" required />
    </div>
    <div class="mb-3">
      <label class="form-label">Capacidad</label>
      <input type="number" class="form-control" id="capacidad" required />
    </div>
    <button type="submit" class="btn btn-success">Crear</button>
  </form>
</div>

🔸 events.js (crear evento)
----------------------------------
export async function createEvent(evento) {
  const res = await fetch("http://localhost:3000/eventos", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ ...evento, asistentes: [] })
  });
  return await res.json();
}

🔸 main.js (handler)
----------------------------------
import { createEvent } from './events.js';

if (window.location.hash === "#/dashboard/events/create") {
  document.getElementById("createEventForm").addEventListener("submit", async (e) => {
    e.preventDefault();
    const nombre = document.getElementById("nombre").value;
    const lugar = document.getElementById("lugar").value;
    const capacidad = parseInt(document.getElementById("capacidad").value);

    await createEvent({ nombre, lugar, capacidad });
    alert("Evento creado correctamente");
    window.location.hash = "#/dashboard";
  });
}


## 5. Editar Evento (/dashboard/events/edit?id=X)

🔸 src/views/edit-event.html
----------------------------------
<div class="container mt-5">
  <h2>Editar Evento</h2>
  <form id="editEventForm">
    <input type="hidden" id="id" />
    <div class="mb-3">
      <label class="form-label">Nombre</label>
      <input type="text" class="form-control" id="nombre" required />
    </div>
    <div class="mb-3">
      <label class="form-label">Lugar</label>
      <input type="text" class="form-control" id="lugar" required />
    </div>
    <div class="mb-3">
      <label class="form-label">Capacidad</label>
      <input type="number" class="form-control" id="capacidad" required />
    </div>
    <button type="submit" class="btn btn-warning">Guardar Cambios</button>
  </form>
</div>

🔸 events.js
----------------------------------
export async function getEventById(id) {
  const res = await fetch(`http://localhost:3000/eventos/${id}`);
  return await res.json();
}

export async function updateEvent(id, data) {
  const res = await fetch(`http://localhost:3000/eventos/${id}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(data)
  });
  return await res.json();
}

🔸 main.js
----------------------------------
import { getEventById, updateEvent } from './events.js';

if (window.location.hash.startsWith("#/dashboard/events/edit")) {
  const params = new URLSearchParams(window.location.hash.split("?")[1]);
  const id = params.get("id");

  getEventById(id).then(evento => {
    document.getElementById("id").value = evento.id;
    document.getElementById("nombre").value = evento.nombre;
    document.getElementById("lugar").value = evento.lugar;
    document.getElementById("capacidad").value = evento.capacidad;
  });

  document.getElementById("editEventForm").addEventListener("submit", async (e) => {
    e.preventDefault();
    const updated = {
      nombre: document.getElementById("nombre").value,
      lugar: document.getElementById("lugar").value,
      capacidad: parseInt(document.getElementById("capacidad").value),
    };
    await updateEvent(id, updated);
    alert("Evento actualizado");
    window.location.hash = "#/dashboard";
  });
}


## 6. Página de Ruta no Encontrada (/not-found)

🔸 src/views/not-found.html
----------------------------------
<div class="container mt-5 text-center">
  <h2>😵 Página no encontrada</h2>
  <p>La ruta que estás buscando no existe.</p>
  <a href="#/dashboard" class="btn btn-primary">Ir al Dashboard</a>
</div>
