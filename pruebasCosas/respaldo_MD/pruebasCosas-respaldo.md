# Bundle del Proyecto para IA: pruebasCosas

## Estructura de Archivos
- README.md
- app.js
- config.js
- estilos.css
- index.html

---

## Código Fuente

### Archivo: `README.md`
```md
# pruebasCosas

Proyecto de prueba para usar **biblioFaRo**, un servicio propio de almacenamiento de archivos JSON publicado como Netlify Functions en `bibliofaro.netlify.app`.

Sirve como ejemplo mínimo de cómo otra aplicación puede leer y guardar datos en biblioFaRo sin tener su propio backend.

## Qué hace

- Formulario simple para **guardar o modificar registros**, identificados por ID.
- Botón **Cargar registros** que lee los datos guardados y los muestra en una lista con acciones.

## Cómo funciona

- `config.js` define las direcciones de biblioFaRo:
  - `get-archivo?proyecto=pruebasCosas&archivo=pruebasCosas.json` para leer;
  - `update-archivo` para guardar.
- `app.js` usa esas direcciones para leer los registros, verificar si un ID ya existe y guardar los cambios.

Para usar el mismo patrón en otro proyecto, copiá `config.js` y cambiá el nombre del proyecto y del archivo.

## Ejecutar localmente

No necesita instalación. Abrí `index.html` en el navegador. Necesita conexión, porque los datos están en biblioFaRo.

## Estructura

```
index.html    Página de prueba
config.js     Direcciones de biblioFaRo
app.js        Lectura y guardado de registros
estilos.css   Estilos
```

```

### Archivo: `app.js`
```javascript
// Cargar configuración global
// (asegúrate de incluir <script src="config.js"></script> antes de app.js en index.html)
const { PROYECTO, ARCHIVO, URL_UPDATE, URL_GET } = CONFIG;

// Guardar o modificar registro
const form = document.getElementById('form-guardar');
let registrosActuales = [];
form.onsubmit = async function(e) {
  e.preventDefault();
  const id = document.getElementById('input-id').value.trim();
  const nombre = document.getElementById('input-nombre').value.trim();
  if (!id || !nombre) return;
  // Verificar si el ID ya existe en los registros actuales
  const existe = registrosActuales.some(r => r.id === id && r.activo !== false);
  const body = {
    proyecto: PROYECTO,
    archivo: ARCHIVO,
    accion: existe ? 'modificar' : 'agregar',
    id,
    datos: { nombre }
  };
  const resp = await fetch(URL_UPDATE, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(body)
  });
  alert(await resp.text());
  form.reset();
  cargarRegistros();
};

// Leer registros activos y renderizar con acciones
const btnCargar = document.getElementById('btn-cargar');
btnCargar.onclick = cargarRegistros;

async function cargarRegistros() {
  const resp = await fetch(URL_GET);
  const div = document.getElementById('resultados');
  let data;
  if (resp.ok) {
    try {
      data = await resp.json();
    } catch {
      data = [];
    }
  } else {
    const errorText = await resp.text();
    div.innerHTML = '<span style="color:red">' + errorText + '</span>';
    return;
  }
  registrosActuales = Array.isArray(data) ? data : [];
  if (!Array.isArray(data) || data.length === 0) {
    div.innerHTML = '<em>No hay registros activos.</em>';
    return;
  }
  function escapeJSString(str) {
    return String(str)
      .replace(/\\/g, "\\\\")
      .replace(/'/g, "\\'")
      .replace(/\n/g, "\\n")
      .replace(/\r/g, "\\r");
  }
  div.innerHTML = '<table><tr><th>ID</th><th>Nombre</th><th>Acciones</th></tr>' +
    data.map(r => `<tr><td>${r.id}</td><td>${r.nombre}</td><td>
      <button onclick="editarRegistro('${r.id}','${escapeJSString(r.nombre)}')">Editar</button>
      <button onclick="eliminarRegistro('${r.id}')">Eliminar</button>
    </td></tr>`).join('') + '</table>';
}

window.editarRegistro = function(id, nombre) {
  document.getElementById('input-id').value = id;
  document.getElementById('input-nombre').value = nombre;
};

window.eliminarRegistro = async function(id) {
  if (!confirm('¿Eliminar este registro?')) return;
  const body = {
    proyecto: PROYECTO,
    archivo: ARCHIVO,
    accion: 'eliminar',
    id,
    datos: {}
  };
  const resp = await fetch(URL_UPDATE, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(body)
  });
  alert(await resp.text());
  cargarRegistros();
};

// Cargar registros al inicio
cargarRegistros();

```

### Archivo: `config.js`
```javascript
// Configuración global para pruebasCosas
const CONFIG = {
  PROYECTO: 'pruebasCosas',
  ARCHIVO: 'pruebasCosas.json',
  URL_BASE_API: 'https://bibliofaro.netlify.app/.netlify/functions/',
  URL_UPDATE: 'https://bibliofaro.netlify.app/.netlify/functions/update-archivo',
  URL_GET: 'https://bibliofaro.netlify.app/.netlify/functions/get-archivo?proyecto=pruebasCosas&archivo=pruebasCosas.json',
  ACCIONES: {
    AGREGAR: 'agregar',
    MODIFICAR: 'modificar',
    ELIMINAR: 'eliminar'
  },
  CAMPOS: ['id', 'nombre'],
  MENSAJES: {
    EXITO: 'Operación realizada correctamente.',
    ERROR: 'Ocurrió un error. Intente nuevamente.'
  },
  DEBUG: false,
  TIMEOUT: 10000 // ms
};

```

### Archivo: `estilos.css`
```css
body {
  font-family: 'Segoe UI', Arial, sans-serif;
  background: #f8fafc;
  padding: 2em;
}
h1 {
  color: #2563eb;
  margin-bottom: 1em;
}
form {
  margin-bottom: 1em;
  display: flex;
  gap: 1em;
}
input[type="text"] {
  border: 1px solid #93c5fd;
  border-radius: 0.5em;
  padding: 0.4em 0.8em;
  font-size: 1em;
}
button {
  background: #2563eb;
  color: #fff;
  border: none;
  border-radius: 0.5em;
  padding: 0.5em 1.5em;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
}
button:hover {
  background: #1d4ed8;
}
#resultados {
  margin-top: 2em;
  background: #fff;
  border-radius: 0.5em;
  padding: 1em;
  box-shadow: 0 2px 8px rgba(30,64,175,0.06);
  font-size: 1em;
}

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Prueba biblioFaRo</title>
  <link rel="stylesheet" href="estilos.css">
</head>
<body>
  <h1>Prueba de biblioFaRo (pruebasCosas)</h1>
  <form id="form-guardar">
    <input type="text" id="input-id" placeholder="ID" required style="width:80px;">
    <input type="text" id="input-nombre" placeholder="Nombre" required style="width:140px;">
    <button type="submit">Guardar</button>
  </form>
  <button id="btn-cargar">Cargar registros</button>
  <div id="resultados"></div>
  <script src="config.js"></script>
  <script src="app.js"></script>
</body>
</html>

```

