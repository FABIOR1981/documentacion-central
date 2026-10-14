# Bundle del Proyecto para IA: monitor-status-test

## Estructura de Archivos
- abm-webs.html
- css/alertas_error.css
- css/leyenda_base.css
- css/leyenda_claro.css
- css/leyenda_oscuro.css
- css/monitor_base.css
- css/monitor_def.css
- css/monitor_min.css
- css/monitor_osc.css
- css/monitor_pro.css
- css/monitor_pro2.css
- data/webs.json
- documentacion/LEEME.md
- index.html
- js/alertas_error.js
- js/config.js
- js/i18n.js
- js/leyenda_script.js
- js/rum.js
- js/script.js
- lang/i18n_en.js
- lang/i18n_es.js
- leyenda.html
- netlify.toml
- netlify/functions/check-status.js
- netlify/functions/rum-aggregate.js
- netlify/functions/rum-collect.js
- netlify/functions/update-webs.js
- package.json
- readme.md

---

## Código Fuente

### Archivo: `abm-webs.html`
```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ABM - Gestión de Sitios Web</title>
    <link
      rel="icon"
      href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>⚙️</text></svg>"
    />
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }
      body {
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
          'Helvetica Neue', Arial, sans-serif;
        background: #f4f7f9;
        min-height: 100vh;
        padding: 20px;
      }
      .container {
        max-width: 1200px;
        margin: 0 auto;
        background: white;
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        overflow: hidden;
      }
      header {
        background: #007bff;
        color: white;
        padding: 30px;
        text-align: center;
      }
      header h1 {
        font-size: 2em;
        margin-bottom: 10px;
      }
      header p {
        opacity: 0.9;
      }
      .nav-links {
        text-align: center;
        padding: 15px;
        background: #f8f9fa;
      }
      .nav-links a {
        color: #007bff;
        text-decoration: none;
        margin: 0 15px;
        font-weight: 500;
      }
      .nav-links a:hover {
        text-decoration: underline;
      }
      .content {
        padding: 30px;
      }
      .form-section {
        background: #f8f9fa;
        padding: 25px;
        border-radius: 8px;
        margin-bottom: 30px;
      }
      .form-section h2 {
        color: #333;
        margin-bottom: 20px;
        font-size: 1.5em;
      }
      .form-group {
        margin-bottom: 15px;
      }
      .form-group label {
        display: block;
        margin-bottom: 5px;
        color: #555;
        font-weight: 500;
      }
      .form-group input,
      .form-group select {
        width: 100%;
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 6px;
        font-size: 14px;
      }
      .form-group input:focus,
      .form-group select:focus {
        outline: none;
        border-color: #007bff;
        box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);
      }
      .form-group small {
        display: block;
        margin-top: 5px;
        color: #777;
        font-size: 12px;
      }
      .btn {
        padding: 12px 24px;
        border: none;
        border-radius: 6px;
        font-size: 14px;
        font-weight: 600;
        cursor: pointer;
        transition: all 0.3s;
      }
      .btn-primary {
        background: #007bff;
        color: white;
      }
      .btn-primary:hover {
        background: #0056b3;
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 123, 255, 0.3);
      }
      .btn-danger {
        background: #dc3545;
        color: white;
        padding: 8px 16px;
        font-size: 12px;
      }
      .btn-danger:hover {
        background: #c82333;
      }
      .btn-edit {
        background: #ffc107;
        color: #333;
        padding: 8px 16px;
        font-size: 12px;
        margin-right: 5px;
      }
      .btn-edit:hover {
        background: #e0a800;
      }
      .table-section {
        margin-top: 30px;
      }
      .table-section h2 {
        color: #333;
        margin-bottom: 20px;
        font-size: 1.5em;
      }
      table {
        width: 100%;
        border-collapse: collapse;
        background: white;
        border-radius: 8px;
        overflow: hidden;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      }
      thead {
        background: #004085;
        color: white;
      }
      th,
      td {
        padding: 15px;
        text-align: left;
      }
      th {
        font-weight: 600;
      }
      tbody tr {
        border-bottom: 1px solid #eee;
      }
      tbody tr:hover {
        background: #f8f9fa;
      }
      .actions {
        white-space: nowrap;
      }
      .badge {
        display: inline-block;
        padding: 4px 10px;
        border-radius: 12px;
        font-size: 11px;
        font-weight: 600;
        background: #007bff;
        color: white;
      }
      .badge-critico {
        background: #dc3545;
      }
      .alert {
        padding: 15px;
        border-radius: 6px;
        margin-bottom: 20px;
      }
      .alert-success {
        background: #d4edda;
        color: #155724;
        border: 1px solid #c3e6cb;
      }
      .alert-danger {
        background: #f8d7da;
        color: #721c24;
        border: 1px solid #f5c6cb;
      }
      .download-section {
        text-align: center;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
        margin-top: 20px;
      }
      .download-section p {
        margin-bottom: 15px;
        color: #555;
      }
      .hidden {
        display: none;
      }
      @media (max-width: 768px) {
        .container {
          border-radius: 0;
        }
        table {
          font-size: 12px;
        }
        th,
        td {
          padding: 10px 5px;
        }
        .btn {
          padding: 8px 12px;
          font-size: 12px;
        }
      }
    </style>
  </head>
  <body>
    <div class="container">
      <header>
        <h1>⚙️ Panel de Administración</h1>
        <p>Gestión de Sitios Web del Monitor de Estado</p>
      </header>

      <div class="nav-links">
        <a href="index.html">← Volver al Monitor</a>
        <a href="leyenda.html">Ver Leyenda</a>
      </div>

      <div class="content">
        <div id="mensaje" class="hidden"></div>

        <!-- Formulario ABM -->
        <div class="form-section">
          <h2 id="form-title">➕ Agregar Nuevo Sitio</h2>
          <form id="abm-form">
            <input type="hidden" id="edit-index" value="" />

            <div class="form-group">
              <label for="nombre">Nombre del Sitio *</label>
              <input
                type="text"
                id="nombre"
                required
                placeholder="Ej: Google"
              />
            </div>

            <div class="form-group">
              <label for="url">URL *</label>
              <input
                type="url"
                id="url"
                required
                placeholder="Ej: https://www.google.com"
              />
              <small>Debe incluir https:// o http://</small>
            </div>

            <div class="form-group">
              <label for="orden">Orden (opcional)</label>
              <input
                type="number"
                id="orden"
                min="1"
                placeholder="Deja vacío para agregar al final"
              />
              <small>Define la posición en la lista</small>
            </div>

            <div class="form-group">
              <label for="grupo">Grupo (opcional)</label>
              <select id="grupo">
                <option value="">Sin grupo</option>
                <option value="CRITICO">CRÍTICO</option>
              </select>
              <small>Los sitios críticos se destacan en el monitor</small>
            </div>

            <button type="submit" class="btn btn-primary" id="btn-submit">
              Agregar Sitio
            </button>
            <button
              type="button"
              class="btn btn-edit hidden"
              id="btn-cancel"
              onclick="cancelarEdicion()"
            >
              Cancelar
            </button>
          </form>
        </div>

        <!-- Tabla de sitios -->
        <div class="table-section">
          <h2>📋 Sitios Configurados (<span id="total-sitios">0</span>)</h2>
          <div style="overflow-x: auto">
            <table>
              <thead>
                <tr>
                  <th>#</th>
                  <th>Nombre</th>
                  <th>URL</th>
                  <th>Orden</th>
                  <th>Grupo</th>
                  <th>Acciones</th>
                </tr>
              </thead>
              <tbody id="tabla-sitios">
                <tr>
                  <td colspan="6" style="text-align: center; padding: 40px">
                    Cargando datos...
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- Sección de guardado -->
        <div class="download-section">
          <p>
            <strong>💡 Opciones de guardado:</strong> Puedes guardar los cambios
            directamente en GitHub o descargar el archivo JSON.
          </p>
          <div
            style="
              display: flex;
              gap: 10px;
              justify-content: center;
              flex-wrap: wrap;
              margin-top: 15px;
            "
          >
            <button
              class="btn btn-primary"
              onclick="guardarEnGitHub()"
              id="btn-guardar-github"
            >
              🚀 Guardar en GitHub
            </button>
            <button class="btn btn-edit" onclick="descargarJSON()">
              💾 Descargar JSON
            </button>
          </div>
          <div
            id="estado-guardado"
            style="margin-top: 15px; font-size: 0.9em; color: #666"
          ></div>
        </div>
      </div>
    </div>

    <script>
      let sitios = [];

      // Cargar datos al iniciar
      async function cargarDatos() {
        try {
          const response = await fetch('data/webs.json');
          sitios = await response.json();
          guardarEnLocalStorage();
          renderizarTabla();
        } catch (error) {
          mostrarMensaje(
            'Error al cargar los datos: ' + error.message,
            'danger'
          );
        }
      }

      // Guardar en localStorage
      function guardarEnLocalStorage() {
        localStorage.setItem('webs_abm', JSON.stringify(sitios));
      }

      // Cargar desde localStorage si existe
      function cargarDesdeLocalStorage() {
        const guardado = localStorage.getItem('webs_abm');
        if (guardado) {
          sitios = JSON.parse(guardado);
          renderizarTabla();
        } else {
          cargarDatos();
        }
      }

      // Renderizar tabla
      function renderizarTabla() {
        const tbody = document.getElementById('tabla-sitios');
        document.getElementById('total-sitios').textContent = sitios.length;

        if (sitios.length === 0) {
          tbody.innerHTML = `
            <tr>
              <td colspan="6" style="text-align: center; padding: 40px; color: #777;">
                No hay sitios configurados. ¡Agrega el primero!
              </td>
            </tr>
          `;
          return;
        }

        tbody.innerHTML = sitios
          .map(
            (sitio, index) => `
          <tr>
            <td>${index + 1}</td>
            <td><strong>${sitio.nombre}</strong></td>
            <td><a href="${
              sitio.url
            }" target="_blank" style="color: #007bff;">${sitio.url}</a></td>
            <td>${sitio.orden || '-'}</td>
            <td>
              ${
                sitio.grupo
                  ? `<span class="badge badge-critico">${sitio.grupo}</span>`
                  : '-'
              }
            </td>
            <td class="actions">
              <button class="btn btn-edit" onclick="editarSitio(${index})">✏️ Editar</button>
              <button class="btn btn-danger" onclick="eliminarSitio(${index})">🗑️ Eliminar</button>
            </td>
          </tr>
        `
          )
          .join('');
      }

      // Mostrar mensaje
      function mostrarMensaje(texto, tipo = 'success') {
        const mensaje = document.getElementById('mensaje');
        mensaje.className = `alert alert-${tipo}`;
        mensaje.textContent = texto;
        mensaje.classList.remove('hidden');

        setTimeout(() => {
          mensaje.classList.add('hidden');
        }, 5000);

        // Scroll al inicio
        window.scrollTo({ top: 0, behavior: 'smooth' });
      }

      // Agregar o actualizar sitio
      document
        .getElementById('abm-form')
        .addEventListener('submit', function (e) {
          e.preventDefault();

          const editIndex = document.getElementById('edit-index').value;
          const sitio = {
            nombre: document.getElementById('nombre').value.trim(),
            url: document.getElementById('url').value.trim(),
          };

          const orden = document.getElementById('orden').value;
          if (orden) {
            sitio.orden = parseInt(orden);
          }

          const grupo = document.getElementById('grupo').value;
          if (grupo) {
            sitio.grupo = grupo;
          }

          if (editIndex !== '') {
            // Editar
            sitios[parseInt(editIndex)] = sitio;
            mostrarMensaje('✅ Sitio actualizado correctamente', 'success');
          } else {
            // Agregar
            sitios.push(sitio);
            mostrarMensaje('✅ Sitio agregado correctamente', 'success');
          }

          guardarEnLocalStorage();
          renderizarTabla();
          this.reset();
          cancelarEdicion();
        });

      // Editar sitio
      function editarSitio(index) {
        const sitio = sitios[index];

        document.getElementById('edit-index').value = index;
        document.getElementById('nombre').value = sitio.nombre;
        document.getElementById('url').value = sitio.url;
        document.getElementById('orden').value = sitio.orden || '';
        document.getElementById('grupo').value = sitio.grupo || '';

        document.getElementById('form-title').textContent = '✏️ Editar Sitio';
        document.getElementById('btn-submit').textContent = 'Actualizar Sitio';
        document.getElementById('btn-cancel').classList.remove('hidden');

        // Scroll al formulario
        window.scrollTo({ top: 0, behavior: 'smooth' });
      }

      // Cancelar edición
      function cancelarEdicion() {
        document.getElementById('edit-index').value = '';
        document.getElementById('form-title').textContent =
          '➕ Agregar Nuevo Sitio';
        document.getElementById('btn-submit').textContent = 'Agregar Sitio';
        document.getElementById('btn-cancel').classList.add('hidden');
        document.getElementById('abm-form').reset();
      }

      // Eliminar sitio
      function eliminarSitio(index) {
        const sitio = sitios[index];
        if (confirm(`¿Estás seguro de eliminar "${sitio.nombre}"?`)) {
          sitios.splice(index, 1);
          guardarEnLocalStorage();
          renderizarTabla();
          mostrarMensaje('✅ Sitio eliminado correctamente', 'success');
        }
      }

      // Guardar directamente en GitHub
      async function guardarEnGitHub() {
        const btn = document.getElementById('btn-guardar-github');
        const estadoDiv = document.getElementById('estado-guardado');

        try {
          btn.disabled = true;
          btn.textContent = '⏳ Guardando...';
          estadoDiv.textContent = 'Enviando cambios al servidor...';

          const response = await fetch('/.netlify/functions/update-webs', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify({
              data: sitios,
            }),
          });

          const result = await response.json();

          if (response.ok) {
            mostrarMensaje(
              '✅ ¡Cambios guardados exitosamente en GitHub! El sitio se actualizará automáticamente.',
              'success'
            );
            estadoDiv.innerHTML = `<span style="color: #28a745;">✓ Última actualización: ${new Date().toLocaleString()}</span>`;

            // Limpiar localStorage después de guardar exitosamente
            localStorage.removeItem('webs_abm');
          } else {
            throw new Error(result.error || 'Error desconocido');
          }
        } catch (error) {
          mostrarMensaje(
            '❌ Error al guardar: ' +
              error.message +
              '. Intenta descargar el JSON manualmente.',
            'danger'
          );
          estadoDiv.innerHTML = `<span style="color: #dc3545;">✗ Error: ${error.message}</span>`;
        } finally {
          btn.disabled = false;
          btn.textContent = '🚀 Guardar en GitHub';
        }
      }

      // Descargar JSON
      function descargarJSON() {
        const dataStr = JSON.stringify(sitios, null, 2);
        const blob = new Blob([dataStr], { type: 'application/json' });
        const url = URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.href = url;
        link.download = 'webs.json';
        link.click();
        URL.revokeObjectURL(url);

        mostrarMensaje(
          '💾 Archivo descargado. Reemplaza data/webs.json en tu servidor.',
          'success'
        );
      }

      // Inicializar
      cargarDesdeLocalStorage();
    </script>
  </body>
</html>

```

### Archivo: `css/alertas_error.css`
```css
/* Estilos para las notificaciones de error creadas por js/alertas_error.js */
#notificaciones-errores {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 9999;
  max-width: 400px;
  font-family: monospace;
}

.notif-alarma {
  background: #fff3cd;
  border: 1px solid #ffeeba;
  color: #856404;
  padding: 12px 12px 8px 12px;
  margin-bottom: 10px;
  border-radius: 6px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  white-space: pre-line;
  position: relative;
  overflow: hidden;
}

.notif-alarma .texto {
  user-select: text;
  word-break: break-word;
}

.notif-alarma .btn-copiar {
  margin-top: 8px;
  margin-right: 8px;
  float: right;
}

.notif-alarma .btn-cerrar {
  position: absolute;
  top: 4px;
  right: 6px;
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
}

/* Asegurar que los botones sean accesibles en teclado */
.notif-alarma button:focus {
  outline: 2px solid rgba(0, 123, 255, 0.25);
  outline-offset: 2px;
}

```

### Archivo: `css/leyenda_base.css`
```css
/* ======================================================= */
/* LEYENDA BASE: Estilos Estructurales (Comunes a todos los temas) */
/* Los colores, bordes y fondos son definidos por los CSS de tema (leyenda_def/min/pro.css) */
/* ======================================================= */

body {
  /* Layout principal */
  min-height: 100vh;
  margin: 0;
  padding: 20px;
  font-family: Arial, sans-serif; /* Fuente base */
  display: flex;
  flex-direction: column;
  align-items: center;
}

header {
  width: 100%;
  max-width: 800px;
  margin-bottom: 20px;
  padding-bottom: 10px;
  text-align: center;
  /* El borde inferior (color) será definido por el CSS del tema */
}

#titulo-leyenda {
  font-size: 2em;
  font-weight: bold;
  margin: 0;
}

#contenido-leyenda {
  width: 100%;
  max-width: 800px;
  padding: 20px;
  border-radius: 8px;
  line-height: 1.6;
  text-align: justify;
}

/* --- Estilos de Tabla de Umbrales (Estructura fija) --- */
.leyenda-tabla-umbrales {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

.leyenda-tabla-umbrales th,
.leyenda-tabla-umbrales td {
  padding: 10px;
  text-align: left;
  /* Los bordes se definen en los CSS de tema (leyenda_def/pro/min.css) */
}

.leyenda-tabla-umbrales th {
  font-weight: bold;
}

.leyenda-tabla-umbrales td:nth-child(2) {
  /* Umbral de latencia: centrar */
  text-align: center;
  font-weight: bold;
  white-space: nowrap;
}

/* Pie de página y enlace de retorno */
footer {
  margin-top: 30px;
  padding-top: 10px;
  width: 100%;
  max-width: 800px;
  text-align: center;
}

#enlace-volver {
  display: inline-block;
  padding: 10px 20px;
  font-size: 1em;
  text-decoration: none;
  font-weight: bold;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

/* ======================================================= */
/* NUEVOS ESTILOS DE PRESENTACIÓN PARA EXPANSIÓN Y ERRORES */
/* Estructura y apariencia independiente del color del tema */
/* ======================================================= */

/* Estilos de título para las secciones internas */
.leyenda-section h3 {
  font-size: 1.5em;
  margin-top: 30px;
  margin-bottom: 10px;
  padding-bottom: 5px;
  /* El color y el borde se definirán en el tema (usando var() ) */
}

/* Estilos de la nueva tabla de errores (Estructura fija) */
.leyenda-tabla-errores {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

.leyenda-tabla-errores th,
.leyenda-tabla-errores td {
  padding: 10px;
  text-align: left;
  /* Los bordes y fondos se definen en los CSS de tema */
}

.leyenda-tabla-errores th {
  font-weight: bold;
}

/* --- Estilos para la justificación (details/summary) --- */

.leyenda-tabla-umbrales details {
  border: none;
  padding: 0;
  margin: 0;
}

.leyenda-tabla-umbrales summary {
  cursor: pointer;
  font-weight: bold;
  list-style: none; /* Elimina la flecha predeterminada */
  position: relative;
  padding-left: 18px;
  transition: color 0.2s ease;
  /* El color se definirá en el tema (usando var() ) */
}

/* Indicador de expansión personalizado */
.leyenda-tabla-umbrales summary::before {
  content: '▶';
  position: absolute;
  left: 0;
  font-size: 0.8em;
  top: 50%;
  transform: translateY(-50%);
  transition: transform 0.2s;
  /* El color se definirá en el tema (usando var() ) */
}

.leyenda-tabla-umbrales details[open] summary::before {
  content: '▼';
}

/* Estilos para el contenido expandido */
.leyenda-tabla-umbrales details p {
  margin: 5px 0 0 0;
  padding-left: 18px;
  font-size: 0.9em;
  /* El color y el borde se definirán en el tema (usando var() ) */
}

/* === Botón Toggle Modo Oscuro (Esquina Superior Derecha) === */
.theme-toggle-button-corner {
  position: fixed;
  top: 20px;
  right: 20px;
  background-color: transparent;
  border: 2px solid currentColor;
  color: inherit;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.5em;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  z-index: 1000;
  width: 45px;
  height: 45px;
}

.theme-toggle-button-corner:hover {
  transform: translateY(-3px) rotate(20deg);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.25);
}

.theme-toggle-button-corner:active {
  transform: translateY(0) rotate(0deg);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

.theme-toggle-button-corner #theme-icon {
  font-size: 1em;
  transition: transform 0.3s ease;
}

/* Responsive: mover botón en móviles */
@media (max-width: 768px) {
  .theme-toggle-button-corner {
    top: 10px;
    right: 10px;
    width: 40px;
    height: 40px;
    font-size: 1.3em;
  }
}

```

### Archivo: `css/leyenda_claro.css`
```css
/* ======================================================= */
/* LEYENDA TEMA: CLARO (Light Theme) - v2025.12.15 */
/* Colores: Fondo Claro (#f4f7f9), Texto Oscuro (#333), Acento Azul (#007bff) */
/* ======================================================= */

/* --- Colores de Base --- */
body {
  background-color: #f4f7f9 !important;
  color: #333;
}

header {
  border-bottom: 2px solid #007bff; /* Color de acento */
}

#titulo-leyenda {
  color: #007bff;
}

#contenido-leyenda {
  background-color: #fff; /* Fondo del contenedor de contenido */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

/* --- Estilos de Tabla de Umbrales --- */
.leyenda-tabla-umbrales th {
  background-color: #e6f0ff; /* Azul muy claro para header de tabla */
  color: #0056b3; /* Azul oscuro para texto */
  border: 1px solid #d1e7ff;
}

.leyenda-tabla-umbrales td {
  border: 1px solid #dee2e6;
}

/* --- Colores de Estado (variables) --- */
:root {
  --status-very-fast-bg: #d4edda;
  --status-very-fast-color: #0f5132;
  --status-fast-bg: #c3e6cb;
  --status-fast-color: #155724;
  --status-normal-bg: #cce8ff;
  --status-normal-color: #044385;
  --status-slow-bg: #ffe0b2;
  --status-slow-color: #9c5a00;
  --status-critical-bg: #f8d7da;
  --status-critical-color: #721c24;
  --status-risk-bg: #e9a3af;
  --status-risk-color: #58151c;
  --status-extreme-risk-bg: #dc3545;
  --status-extreme-risk-color: white;
  --status-down-bg: #343a40;
  --status-down-color: white;
}

/* Keep font-weight where needed */
.status-normal {
  font-weight: bold;
}
.status-extreme-risk,
.status-down {
  font-weight: bold;
}

footer {
  border-top: 1px solid #dee2e6;
}
#enlace-volver {
  color: #007bff;
}

/* ======================================================= */
/* NUEVOS ESTILOS PARA CÓDIGOS DE ERROR Y EXPANSIÓN */
/* ======================================================= */

/* Estilo para el título de la nueva sección */
.leyenda-section h3 {
  color: #007bff; /* Color primario */
  border-bottom: 1px dashed #ccc;
}

/* Estilos de la nueva tabla de errores */
.leyenda-tabla-errores th {
  background-color: #e6f0ff; /* Fondo de header de tabla, igual que la tabla de umbrales */
  color: #0056b3;
  border: 1px solid #d1e7ff;
}

.leyenda-tabla-errores td {
  border: 1px solid #dee2e6;
  background-color: #fff; /* Fondo de celda blanca */
}

/* Colores para los elementos expandibles (details/summary) */
.leyenda-tabla-umbrales summary {
  color: #007bff; /* Color de acento (azul) */
}

.leyenda-tabla-umbrales summary:hover {
  color: #0056b3;
}

.leyenda-tabla-umbrales summary::before {
  color: #007bff; /* Color de acento */
}

.leyenda-tabla-umbrales details p {
  color: #444; /* Color de texto para el contenido expandido */
  border-left: 2px solid #007bff; /* Línea lateral de acento */
}

```

### Archivo: `css/leyenda_oscuro.css`
```css
/* ======================================================= */
/* LEYENDA TEMA: OSCURO (Dark Theme) - v2025.12.15 */
/* Colores: Fondo Oscuro (#1a1a1a), Texto Claro (#e0e0e0), Acento Azul (#4a9eff) */
/* ======================================================= */

/* --- Colores de Base --- */
body {
  background-color: #1a1a1a !important;
  color: #e0e0e0;
}

header {
  border-bottom: 2px solid #4a9eff; /* Color de acento */
}

#titulo-leyenda {
  color: #4a9eff;
}

#contenido-leyenda {
  background-color: #2d2d2d; /* Fondo del contenedor de contenido */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
}

/* --- Estilos de Tabla de Umbrales --- */
.leyenda-tabla-umbrales th {
  background-color: #1f3a5f; /* Azul oscuro para header de tabla */
  color: #ffffff; /* Texto blanco */
  border: 1px solid #2a4a7f;
}

.leyenda-tabla-umbrales td {
  border: 1px solid #404040;
  background-color: #2d2d2d;
}

/* --- Colores de Estado (tema oscuro) --- */
:root {
  --status-very-fast-bg: #1e4620;
  --status-very-fast-color: #7cfc7c;
  --status-fast-bg: #2d5a2f;
  --status-fast-color: #90ee90;
  --status-normal-bg: #1a3d5f;
  --status-normal-color: #87ceeb;
  --status-slow-bg: #5f4a1a;
  --status-slow-color: #ffa500;
  --status-critical-bg: #5f1a1a;
  --status-critical-color: #ff6b6b;
  --status-risk-bg: #6b1a2f;
  --status-risk-color: #ff4757;
  --status-extreme-risk-bg: #8b0000;
  --status-extreme-risk-color: #ffcccc;
  --status-down-bg: #000000;
  --status-down-color: #ffffff;
}

/* Keep font-weight where needed */
.status-normal {
  font-weight: bold;
}
.status-extreme-risk,
.status-down {
  font-weight: bold;
}

footer {
  border-top: 1px solid #404040;
}

#enlace-volver {
  color: #4a9eff;
}

#enlace-volver:hover {
  color: #6bb3ff;
}

/* ======================================================= */
/* NUEVOS ESTILOS PARA CÓDIGOS DE ERROR Y EXPANSIÓN */
/* ======================================================= */

/* Estilo para el título de la nueva sección */
.leyenda-section h3 {
  color: #4a9eff; /* Color primario del tema oscuro */
  border-bottom: 1px dashed #555;
}

/* Estilos de la nueva tabla de errores */
.leyenda-tabla-errores th {
  background-color: #1f3a5f; /* Fondo de header de tabla */
  color: #ffffff;
  border: 1px solid #2a4a7f;
}

.leyenda-tabla-errores td {
  border: 1px solid #404040;
  background-color: #2d2d2d; /* Fondo de celda oscura */
}

/* Colores para los elementos expandibles (details/summary) */
.leyenda-tabla-umbrales summary {
  color: #4a9eff; /* Color de acento (azul) */
  background-color: #2d2d2d;
}

.leyenda-tabla-umbrales summary:hover {
  color: #6bb3ff;
  background-color: #3a3a3a;
}

.leyenda-tabla-umbrales summary::before {
  color: #4a9eff; /* Color de acento */
}

.leyenda-tabla-umbrales details p {
  color: #b0b0b0; /* Color de texto para el contenido expandido */
  background-color: #2d2d2d;
  border-left: 2px solid #4a9eff; /* Línea lateral de acento */
}

/* --- Enlaces generales --- */
a {
  color: #4a9eff;
}

a:hover {
  color: #6bb3ff;
  text-decoration: underline;
}

/* --- Títulos (h2, h3) --- */
h2,
h3 {
  color: #4a9eff;
}

/* --- Listas --- */
ul,
ol {
  color: #e0e0e0;
}

/* --- Párrafos --- */
p {
  color: #d0d0d0;
}

/* --- Código inline y bloques --- */
code {
  background-color: #3a3a3a;
  color: #ffcccc;
  padding: 2px 6px;
  border-radius: 4px;
}

pre {
  background-color: #2d2d2d;
  border: 1px solid #404040;
  padding: 10px;
  border-radius: 6px;
  overflow-x: auto;
}

pre code {
  background-color: transparent;
  color: #e0e0e0;
}

```

### Archivo: `css/monitor_base.css`
```css
/* Estilos base y variables usadas por todos los temas */
:root {
  --bg: #ffffff;
  --text: #222222;
  --accent: #007bff;
  --container-max-width: 1000px;
  --container-padding: 30px;
  --container-radius: 8px;
  --container-shadow: 0 4px 8px rgba(0, 0, 0, 0.08);
  --table-header-bg: #eeeeee;
  --table-header-text: #222222;
  --link-color: var(--accent);

  /* Colores de estado por defecto; los temas pueden sobrescribirlos */
  --status-very-fast-bg: transparent;
  --status-very-fast-color: inherit;
  --status-fast-bg: transparent;
  --status-fast-color: inherit;
  --status-normal-bg: transparent;
  --status-normal-color: inherit;
  --status-slow-bg: transparent;
  --status-slow-color: inherit;
  --status-critical-bg: transparent;
  --status-critical-color: inherit;
  --status-risk-bg: transparent;
  --status-risk-color: inherit;
  --status-extreme-risk-bg: transparent;
  --status-extreme-risk-color: inherit;
  --status-down-bg: transparent;
  --status-down-color: inherit;
}

/* Aplicar colores de estado usando variables */
.status-very-fast {
  background-color: var(--status-very-fast-bg);
  color: var(--status-very-fast-color);
}
.status-fast {
  background-color: var(--status-fast-bg);
  color: var(--status-fast-color);
}
.status-normal {
  background-color: var(--status-normal-bg);
  color: var(--status-normal-color);
}
.status-slow {
  background-color: var(--status-slow-bg);
  color: var(--status-slow-color);
}
.status-critical {
  background-color: var(--status-critical-bg);
  color: var(--status-critical-color);
}
.status-risk {
  background-color: var(--status-risk-bg);
  color: var(--status-risk-color);
}
.status-extreme-risk {
  background-color: var(--status-extreme-risk-bg);
  color: var(--status-extreme-risk-color);
}
.status-down {
  background-color: var(--status-down-bg);
  color: var(--status-down-color);
}

body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 20px;
  background-color: var(--bg);
  color: var(--text);
}

.container {
  max-width: var(--container-max-width);
  margin: 0 auto;
  background-color: var(--container-bg, #fff);
  padding: var(--container-padding);
  border-radius: var(--container-radius);
  box-shadow: var(--container-shadow);
  min-height: 60vh; /* Predeterminado más responsivo */
}

h1 {
  text-align: center;
  margin-bottom: 5px;
}
.info-bar {
  text-align: center;
  margin-bottom: 20px;
  font-size: 0.9em;
}
.update-info {
  text-align: center;
  font-weight: bold;
  margin-bottom: 20px;
}

/* Menú horizontal */
.menu-horizontal {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 15px;
  flex-wrap: wrap;
}

.menu-item {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  padding: 8px 16px;
  font-size: 0.9em;
  color: #007bff;
  text-decoration: none;
  border-radius: 6px;
  transition: all 0.2s ease;
  background-color: rgba(0, 123, 255, 0.05);
  border: 1px solid rgba(0, 123, 255, 0.2);
}

.menu-item:hover {
  background-color: rgba(0, 123, 255, 0.1);
  border-color: rgba(0, 123, 255, 0.4);
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(0, 123, 255, 0.15);
}

.menu-item:active {
  transform: translateY(0);
}

/* Contenedor compartido de la tabla */
.table-scroll-container {
  overflow-x: auto;
  min-height: 450px;
}

/* Estilos base compartidos de la tabla */
#monitor-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
  border-radius: 8px;
  overflow: hidden;
}
#monitor-table th,
#monitor-table td {
  padding: 12px 10px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}
#monitor-table td a {
  color: var(--link-color);
  text-decoration: none;
  font-size: 0.9em;
  word-break: break-all;
}

#monitor-table thead th {
  background-color: var(--table-header-bg);
  color: var(--table-header-text);
  font-weight: bold;
}
#monitor-table tr:hover {
  background-color: rgba(0, 0, 0, 0.03);
}
#monitor-table td:nth-child(3),
#monitor-table td:nth-child(4),
#monitor-table td:nth-child(5),
#monitor-table td:nth-child(6),
#monitor-table td:nth-child(7),
#monitor-table td:nth-child(8) {
  text-align: center;
}

/* Botones y métricas */
.psi-button,
.error-button,
.btn-primary {
  border: none;
  padding: 5px 8px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8em;
  transition: background-color 0.2s;
}

/* Botón PSI específico */
.psi-button {
  display: inline-block;
  vertical-align: middle;
}

/* Botón de Errores */
.error-button {
  background-color: #c92a2a;
  color: #ffffff;
  border: none;
  padding: 5px 8px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8em;
  display: inline-block;
  vertical-align: middle;
  margin-left: 5px;
  transition: all 0.2s;
}

.error-button:hover {
  background-color: #a61e1e;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(201, 42, 42, 0.3);
}

.error-button:active {
  transform: translateY(0);
  box-shadow: none;
}

.latency-metric {
  font-weight: bold;
  font-size: 1em;
}

/* Clases de estado base (colores definidos por el tema) */
.status-very-fast,
.status-fast,
.status-normal,
.status-slow,
.status-critical,
.status-risk,
.status-extreme-risk,
.status-down {
  font-weight: bold;
}

/* Visibilidad de columnas por defecto (los temas pueden sobrescribir) */
#monitor-table th:nth-child(2),
#monitor-table td:nth-child(2) {
  display: none;
}
#monitor-table th:nth-child(7),
#monitor-table td:nth-child(7) {
  display: none;
}
#monitor-table th:nth-child(8),
#monitor-table td:nth-child(8) {
  display: none;
}

/* Emojis para estado visual rápido (presentacionales - no para lectores de pantalla) */
.status-very-fast::after {
  content: ' 🚀';
}
.status-fast::after {
  content: ' ⭐';
}
.status-normal::after {
  content: ' ✅';
}
.status-slow::after {
  content: ' ⚠️';
}
.status-critical::after {
  content: ' 🐌';
}
.status-risk::after {
  content: ' 🚨';
}
.status-extreme-risk::after {
  content: ' 🔥';
}
.status-down::after {
  content: ' ❌';
}

/* Ayudante de accesibilidad: solo para lectores de pantalla (mantiene el texto legible para tecnologías de asistencia) */
.sr-only {
  position: absolute !important;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* ====================================
   ESTILOS PARA EXPANSIÓN DE ERRORES
   ==================================== */

/* Efecto hover para badges clickeables */
.status-very-fast[style*='cursor: pointer']:hover,
.status-fast[style*='cursor: pointer']:hover,
.status-normal[style*='cursor: pointer']:hover,
.status-slow[style*='cursor: pointer']:hover,
.status-critical[style*='cursor: pointer']:hover,
.status-risk[style*='cursor: pointer']:hover,
.status-extreme-risk[style*='cursor: pointer']:hover,
.status-down[style*='cursor: pointer']:hover {
  opacity: 0.8;
  text-decoration: underline;
}

/* Botón PSI */
.psi-button {
  display: inline-block;
  vertical-align: middle;
}

/* Fila de detalle de errores */
.error-detail-row {
  background: #fff5f5;
  border-left: 4px solid #ff6b6b;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.3s ease-out;
}

.error-detail-row.expanded {
  max-height: 500px;
  transition: max-height 0.3s ease-in;
}

.error-detail-row td {
  padding: 0 !important;
}

/* Contenedor de errores */
.error-detail-container {
  padding: 15px 20px;
  font-size: 0.9em;
}

.error-detail-header {
  font-weight: bold;
  color: #c92a2a;
  margin-bottom: 10px;
  font-size: 1.05em;
}

.error-detail-list {
  list-style: none;
  padding: 0;
  margin: 10px 0;
}

.error-detail-list li {
  padding: 8px 12px;
  margin: 5px 0;
  background: white;
  border-radius: 4px;
  border-left: 3px solid #ff6b6b;
  font-family: 'Courier New', monospace;
  font-size: 0.95em;
  transition: background 0.2s;
}

.error-detail-list li:hover {
  background: #ffe0e0;
}

.error-time {
  color: #666;
  font-weight: bold;
}

.error-code {
  color: #c92a2a;
  font-weight: bold;
  background: #ffe0e0;
  padding: 2px 6px;
  border-radius: 3px;
}

.error-msg {
  color: #333;
}

.error-latency {
  color: #999;
  font-size: 0.9em;
}

.error-detail-footer {
  margin-top: 10px;
  color: #999;
  font-style: italic;
  font-size: 0.9em;
  text-align: center;
}

/* === Botón Toggle Modo Oscuro (Esquina Superior Derecha) === */
.theme-toggle-button-corner {
  position: fixed;
  top: 20px;
  right: 20px;
  background-color: var(--container-bg, #fff);
  border: 2px solid var(--accent, #007bff);
  color: var(--text, #333);
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.5em;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  z-index: 1000;
  width: 45px;
  height: 45px;
}

.theme-toggle-button-corner:hover {
  transform: translateY(-3px) rotate(20deg);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.25);
}

.theme-toggle-button-corner:active {
  transform: translateY(0) rotate(0deg);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

.theme-toggle-button-corner #theme-icon {
  font-size: 1em;
  transition: transform 0.3s ease;
}

/* Responsive: mover botón en móviles */
@media (max-width: 768px) {
  .theme-toggle-button-corner {
    top: 10px;
    right: 10px;
    width: 40px;
    height: 40px;
    font-size: 1.3em;
  }
}

```

### Archivo: `css/monitor_def.css`
```css
:root {
  --bg: #f4f7f9;
  --text: #333;
  --accent: #007bff;
  --container-bg: #fff;
  --container-radius: 8px;
  --container-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  --header-color: #007bff;
  --table-header-bg: #004085;
  --table-header-color: #ffffff;
  --link-color: #007bff;
}

/* Sobrescrituras específicas del tema: uso simple de las variables definidas en monitor_base */
body {
  background-color: var(--bg);
  color: var(--text);
}
h1 {
  color: var(--header-color);
}

/* Colores de info definidos a nivel de tema; la estructura se movió a styles_base */
.info-bar {
  color: var(--info-color, #6c757d);
}
.update-info {
  color: var(--update-info-color, #495057);
}

/* table-scroll-container movido a monitor_base.css */

/* --- Estilos de la Tabla (Ajuste de Filas y Columnas) --- */
#monitor-table {
  width: 100%;
  border-collapse: collapse;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.05);
  overflow: hidden;
  border-radius: 8px;
  margin-bottom: 20px;
}
#monitor-table th,
#monitor-table td {
  padding: 16px 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
  transition: all 0.2s ease;
}
#monitor-table td a {
  color: #007bff;
  text-decoration: none;
  font-size: 0.9em;
  word-break: break-all;
  transition: color 0.2s ease;
}
#monitor-table td a:hover {
  color: #0056b3;
  text-decoration: underline;
}

/* Establecer ANCHOS FIJOS para las 7 columnas (se muestran 5 en total) */
/* Orden: Servicio(1), URL(2), Latencia Actual(3), Estado Actual(4), Promedios(5), Estado Promedio(6), Status HTTP(7), Acción(8) */
#monitor-table thead th:nth-child(1) {
  width: 15%;
}
#monitor-table thead th:nth-child(2) {
  width: 30%;
}
#monitor-table thead th:nth-child(3) {
  width: 10%;
}
#monitor-table thead th:nth-child(4) {
  width: 15%;
}
#monitor-table thead th:nth-child(5) {
  width: 10%;
}
#monitor-table thead th:nth-child(6) {
  width: 15%;
}
#monitor-table thead th:nth-child(7) {
  width: 5%;
}
#monitor-table thead th:nth-child(8) {
  width: 5%;
}

#monitor-table thead th {
  background-color: #004085;
  color: white;
  font-weight: bold;
}
#monitor-table tbody tr {
  cursor: pointer;
}
#monitor-table tbody tr:hover {
  background-color: #e8f4f8;
  transform: scale(1.002);
  box-shadow: 0 2px 8px rgba(0, 123, 255, 0.15);
}

/* Centrar el texto en las columnas de datos (3 a 8) */
#monitor-table td:nth-child(3),
#monitor-table td:nth-child(4),
#monitor-table td:nth-child(5),
#monitor-table td:nth-child(6),
#monitor-table td:nth-child(7),
#monitor-table td:nth-child(8) {
  text-align: center;
  vertical-align: middle;
}

/* --- Estilos del Botón de Acción (PSI) --- */
.psi-button,
.btn-primary {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
}
.psi-button:hover,
.btn-primary:hover {
  background-color: #218838;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(40, 167, 69, 0.3);
}

.error-button {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
  margin-left: 5px;
}
.error-button:hover {
  background-color: #c82333;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(220, 53, 69, 0.3);
}

/* --- Colores de Estado según Tramos de Velocidad (7 Niveles) --- */

/* --- Colores de Estado (NUEVA PALETA DE LATENCIA) --- */
/* Colores de estado definidos mediante variables */
:root {
  --status-very-fast-bg: #d4edda;
  --status-very-fast-color: #0f5132;
  --status-fast-bg: #c3e6cb;
  --status-fast-color: #155724;
  --status-normal-bg: #cce8ff;
  --status-normal-color: #044385;
  --status-slow-bg: #ffe0b2;
  --status-slow-color: #9c5a00;
  --status-critical-bg: #f8d7da;
  --status-critical-color: #721c24;
  --status-risk-bg: #e9a3af;
  --status-risk-color: #58151c;
  --status-extreme-risk-bg: #dc3545;
  --status-extreme-risk-color: white;
  --status-down-bg: #343a40;
  --status-down-color: white;
}

/* Mantener el peso de fuente y otras presentaciones por encima de las reglas base */
.status-very-fast,
.status-fast,
.status-normal,
.status-slow,
.status-critical,
.status-risk,
.status-extreme-risk,
.status-down {
  font-weight: bold;
  padding: 6px 12px;
  border-radius: 4px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Excepción: CAÍDA/ERROR puede tener ancho variable para mostrar todo */
.status-down {
  white-space: normal;
  overflow: visible;
  text-overflow: clip;
}

/* Destacar estados críticos - TOP 2: Jerarquía Visual */
.status-critical,
.status-risk,
.status-extreme-risk {
  border: 2px solid #dc3545;
  box-shadow: 0 0 8px rgba(220, 53, 69, 0.3);
}

/* Servicios caídos con animación de alerta */
.status-down {
  border: 2px solid #721c24;
  animation: pulse-alert 2s ease-in-out infinite;
}

@keyframes pulse-alert {
  0%,
  100% {
    box-shadow: 0 0 0 rgba(220, 53, 69, 0);
  }
  50% {
    box-shadow: 0 0 12px rgba(220, 53, 69, 0.6);
  }
}

/* --- Estilo para la Latencia (Métricas) --- */
.latency-metric {
  font-weight: bold;
  font-size: 1em;
  background-color: #fff;
}

/* --- Responsive (Ajustes para scroll horizontal en móvil) --- */
@media (max-width: 768px) {
  .container {
    padding: 10px;
    border-radius: 0;
    box-shadow: none;
  }
  h1 {
    font-size: 1.5em;
  }
  #monitor-table {
    font-size: 0.8em;
  }
}

/* ======================================================= */
/* REGLAS FINALES: Control de Visibilidad de Columnas */
/* ======================================================= */

/* Oculta la columna URL (2da columna) en el tema standard */
#monitor-table th:nth-child(2),
#monitor-table td:nth-child(2) {
  display: none;
}

/* NUEVO: Oculta la columna Status HTTP (7ma columna) en el tema standard */
#monitor-table th:nth-child(7),
#monitor-table td:nth-child(7) {
  display: none;
}

/* NUEVO: Oculta la columna de Acción (8va columna) en el tema standard */
#monitor-table th:nth-child(8),
#monitor-table td:nth-child(8) {
  display: none;
}

/* Pseudo-elementos Emoji centralizados en monitor_base.css; no se duplican aquí. */
/* ======================================================= */

```

### Archivo: `css/monitor_min.css`
```css
/* ======================================================= */
/* VERSÍON MINIMALISTA Y NEUTRA DE monitor.css */
/* (Base: Versión minimalista solicitada) */
/* ======================================================= */

:root {
  --bg: #f7f7f7;
  --text: #333;
  --accent: #6c757d;
  --container-bg: #fff;
  --container-radius: 4px;
  --container-shadow: none;
  --header-color: #444;
  --link-color: #007bff;
  --table-header-bg: #eee;
  --table-header-color: #444;

  --status-very-fast-bg: transparent;
  --status-very-fast-color: #104e10;
  --status-fast-bg: transparent;
  --status-fast-color: #1a571a;
  --status-normal-bg: transparent;
  --status-normal-color: #004085;
  --status-slow-bg: transparent;
  --status-slow-color: #d45a00;
  --status-critical-bg: transparent;
  --status-critical-color: #e65100;
  --status-risk-bg: transparent;
  --status-risk-color: #b71c1c;
  --status-extreme-risk-bg: transparent;
  --status-extreme-risk-color: #880e4f;
  --status-down-bg: #dc3545;
  --status-down-color: white;
}

body {
  background-color: var(--bg);
  color: var(--text);
}
h1 {
  color: var(--header-color);
}

/* Colores de información definidos a nivel de tema; la estructura se movió a monitor_base */
.info-bar {
  color: var(--info-color, #6c757d);
}
.update-info {
  color: var(--update-info-color, #495057);
}

/* table-scroll-container movido a monitor_base.css */

/* --- Estilos de la Tabla --- */
#monitor-table {
  width: 100%;
  border-collapse: collapse;
  /* Eliminada la sombra de la tabla */
  border-radius: 4px;
  border: 1px solid #ccc; /* Borde simple para definir la tabla */
  margin-bottom: 20px;
}
#monitor-table th,
#monitor-table td {
  padding: 10px 8px;
  text-align: left;
  border-bottom: 1px solid #eee; /* Borde muy sutil entre filas */
}
#monitor-table td a {
  color: #007bff;
  text-decoration: none;
  font-size: 0.9em;
  word-break: break-all;
}

/* Establecer ANCHOS FIJOS (Se mantiene la estructura) */
#monitor-table thead th:nth-child(1) {
  width: 15%;
}
#monitor-table thead th:nth-child(2) {
  width: 30%;
}
#monitor-table thead th:nth-child(3) {
  width: 10%;
}
#monitor-table thead th:nth-child(4) {
  width: 15%;
}
#monitor-table thead th:nth-child(5) {
  width: 10%;
}
#monitor-table thead th:nth-child(6) {
  width: 15%;
}
#monitor-table thead th:nth-child(7) {
  width: 5%;
}
#monitor-table thead th:nth-child(8) {
  width: 5%;
}

#monitor-table thead th {
  background-color: #eee; /* Fondo de encabezado muy claro */
  color: #444; /* Texto oscuro */
  font-weight: bold;
  border-bottom: 2px solid #ccc; /* Separador más notorio */
  /* 🚀 CORRECCIÓN DE ALINEACIÓN Y WRAPPING 🚀 */
  white-space: nowrap; /* Evita que el texto de la cabecera se rompa en múltiples líneas */
  text-align: center; /* Centra el texto de la cabecera */
}
#monitor-table tr:hover {
  background-color: #f5f5f5; /* Efecto hover muy sutil */
}

/* Centrar el texto en las columnas de datos (se mantiene) */
#monitor-table td:nth-child(3),
#monitor-table td:nth-child(4),
#monitor-table td:nth-child(5),
#monitor-table td:nth-child(6),
#monitor-table td:nth-child(7),
#monitor-table td:nth-child(8) {
  text-align: center;
}

/* --- Estilos del Botón de Acción (PSI) --- */
.psi-button,
.btn-primary {
  background-color: #333; /* Color funcional más neutro */
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
}
.psi-button:hover,
.btn-primary:hover {
  background-color: #555;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(85, 85, 85, 0.3);
}

.error-button {
  background-color: #d9534f; /* Rojo Bootstrap */
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
  margin-left: 5px;
}
.error-button:hover {
  background-color: #c9302c;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(217, 83, 79, 0.3);
}

/* ======================================================= */
/* --- Colores de Estado (Minimalista con Corrección de Contraste) --- */
/* ======================================================= */

/* Todos los colores ahora provienen de variables definidas en :root */
.status-very-fast,
.status-fast,
.status-normal,
.status-slow,
.status-critical,
.status-risk,
.status-extreme-risk,
.status-down {
  font-weight: bold;
}

/* Las diferencias de presentación están centralizadas en styles_base usando variables CSS */

/* --- Estilo para la Latencia (Métricas) --- (se mantiene) */
.latency-metric {
  font-weight: bold;
  font-size: 1em;
  background-color: #fff;
}

/* --- Responsive (se mantiene) --- */
@media (max-width: 768px) {
  .container {
    padding: 10px;
    border-radius: 0;
    box-shadow: none;
  }
  h1 {
    font-size: 1.5em;
  }
  #monitor-table {
    font-size: 0.8em;
  }
}

/* ======================================================= */
/* REGLAS FINALES: Control de Visibilidad de Columnas (Tema Metrics) */
/* ======================================================= */

/* Oculta la columna URL (2da columna) */
#monitor-table th:nth-child(2),
#monitor-table td:nth-child(2) {
  display: none;
}

/* Muestra la columna Status HTTP (7ma columna) en el tema metrics */
#monitor-table th:nth-child(7),
#monitor-table td:nth-child(7) {
  display: table-cell;
}

/* Oculta la columna de Acción (8va columna) */
#monitor-table th:nth-child(8),
#monitor-table td:nth-child(8) {
  display: none;
}

/* ======================================================= */
/* NUEVO: ESTILO DE ENLACES EN COLUMNA DE SERVICIO (TEMA MINIMALISTA) */
/* =JOV: Esto hace que el nombre del servicio se vea como texto normal, pero sea clicable */
/* ======================================================= */
#monitor-table td:nth-child(1) a {
  text-decoration: none; /* Elimina el subrayado */
  color: inherit; /* Hereda el color del texto de la tabla */
  font-weight: bold;
}

```

### Archivo: `css/monitor_osc.css`
```css
:root {
  --bg: #121212;
  --text: #e8e8e8;
  --accent: #5cb3ff;
  --container-bg: #1e1e1e;
  --container-radius: 8px;
  --container-shadow: 0 8px 24px rgba(0, 0, 0, 0.7);
  --header-color: #5cb3ff;
  --table-header-bg: #1f3a5f;
  --table-header-color: #ffffff;
  --link-color: #5cb3ff;
}

/* Sobrescrituras específicas del tema oscuro */
body {
  background-color: var(--bg);
  color: var(--text);
}
h1 {
  color: var(--header-color);
}

/* Colores de info definidos a nivel de tema */
.info-bar {
  color: var(--info-color, #a0a0a0);
}
.update-info {
  color: var(--update-info-color, #b0b0b0);
}

/* --- Estilos de la Tabla (Ajuste de Filas y Columnas) --- */
#monitor-table {
  width: 100%;
  border-collapse: collapse;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.7);
  overflow: hidden;
  border-radius: 8px;
  margin-bottom: 20px;
  background-color: #2d2d2d;
}
#monitor-table th,
#monitor-table td {
  padding: 16px 12px;
  text-align: left;
  border-bottom: 1px solid #404040;
  transition: all 0.2s ease;
}
#monitor-table td a {
  color: #5cb3ff;
  text-decoration: none;
  font-size: 0.9em;
  word-break: break-all;
  transition: color 0.2s ease;
}
#monitor-table td a:hover {
  color: #8cccff;
  text-decoration: underline;
}

/* Establecer ANCHOS FIJOS para las 7 columnas (se muestran 5 en total) */
/* Orden: Servicio(1), URL(2), Latencia Actual(3), Estado Actual(4), Promedios(5), Estado Promedio(6), Status HTTP(7), Acción(8) */
#monitor-table thead th:nth-child(1) {
  width: 15%;
}
#monitor-table thead th:nth-child(2) {
  width: 30%;
}
#monitor-table thead th:nth-child(3) {
  width: 10%;
}
#monitor-table thead th:nth-child(4) {
  width: 15%;
}
#monitor-table thead th:nth-child(5) {
  width: 10%;
}
#monitor-table thead th:nth-child(6) {
  width: 15%;
}
#monitor-table thead th:nth-child(7) {
  width: 5%;
}
#monitor-table thead th:nth-child(8) {
  width: 5%;
}

#monitor-table thead th {
  background-color: #1f3a5f;
  color: white;
  font-weight: bold;
}
#monitor-table tbody tr {
  cursor: pointer;
}
#monitor-table tbody tr:hover {
  background-color: #2a2a2a;
  transform: scale(1.002);
  box-shadow: 0 4px 12px rgba(92, 179, 255, 0.2);
}

/* Centrar el texto en las columnas de datos (3 a 8) */
#monitor-table td:nth-child(3),
#monitor-table td:nth-child(4),
#monitor-table td:nth-child(5),
#monitor-table td:nth-child(6),
#monitor-table td:nth-child(7),
#monitor-table td:nth-child(8) {
  text-align: center;
  vertical-align: middle;
}

/* --- Contenedores y elementos principales --- */
.container {
  background-color: var(--container-bg);
  border-radius: var(--container-radius);
  box-shadow: var(--container-shadow);
}

/* --- Botones y Controles --- */
button,
select {
  background-color: #3a3a3a;
  color: #e0e0e0;
  border: 1px solid #4a9eff;
}

button:hover,
select:hover {
  background-color: #4a4a4a;
  border-color: #6bb3ff;
}

button:active {
  background-color: #2a2a2a;
}

/* --- Sección de Duración --- */
.duration-section {
  background-color: #2d2d2d;
  border: 1px solid #404040;
}

/* --- Barra de Información --- */
.info-bar {
  background-color: #2d2d2d;
  border: 1px solid #404040;
}

/* --- Spinner de Carga --- */
.spinner {
  border: 4px solid #3a3a3a;
  border-top: 4px solid #4a9eff;
}

/* --- Colores de Estado (tema oscuro optimizado) --- */
:root {
  --status-very-fast-bg: #1e4620;
  --status-very-fast-color: #7cfc7c;
  --status-fast-bg: #2d5a2f;
  --status-fast-color: #90ee90;
  --status-normal-bg: #1a3d5f;
  --status-normal-color: #87ceeb;
  --status-slow-bg: #5f4a1a;
  --status-slow-color: #ffa500;
  --status-critical-bg: #5f1a1a;
  --status-critical-color: #ff6b6b;
  --status-risk-bg: #6b1a2f;
  --status-risk-color: #ff4757;
  --status-extreme-risk-bg: #8b0000;
  --status-extreme-risk-color: #ffcccc;
  --status-down-bg: #000000;
  --status-down-color: #ffffff;
}

/* Mantener font-weight donde sea necesario */
.status-very-fast,
.status-fast,
.status-normal,
.status-slow,
.status-critical,
.status-risk,
.status-extreme-risk,
.status-down {
  font-weight: bold;
  padding: 6px 12px;
  border-radius: 4px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Excepción: CAÍDA/ERROR puede tener ancho variable para mostrar todo */
.status-down {
  white-space: normal;
  overflow: visible;
  text-overflow: clip;
}

/* Destacar estados críticos - TOP 2: Jerarquía Visual */
.status-critical,
.status-risk,
.status-extreme-risk {
  border: 2px solid #ff6b6b;
  box-shadow: 0 0 8px rgba(255, 107, 107, 0.4);
}

/* Servicios caídos con animación de alerta */
.status-down {
  border: 2px solid #ff4757;
  animation: pulse-alert-dark 2s ease-in-out infinite;
}

@keyframes pulse-alert-dark {
  0%,
  100% {
    box-shadow: 0 0 0 rgba(255, 107, 107, 0);
  }
  50% {
    box-shadow: 0 0 16px rgba(255, 107, 107, 0.7);
  }
}

/* --- Enlaces --- */
a {
  color: #5cb3ff;
  transition: color 0.2s ease;
}

a:hover {
  color: #8cccff;
  text-decoration: underline;
}

/* --- PSI Button --- */
.psi-button,
.btn-primary {
  background-color: #1f3a5f;
  color: #ffffff;
  border: 1px solid #4a9eff;
  padding: 5px 10px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
}

.psi-button:hover,
.btn-primary:hover {
  background-color: #2a4a7f;
  border-color: #6bb3ff;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(74, 158, 255, 0.3);
}

.error-button {
  background-color: #8b0000;
  color: #ffffff;
  border: 1px solid #ff4444;
  padding: 5px 10px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
  margin-left: 5px;
}

.error-button:hover {
  background-color: #a00000;
  border-color: #ff6666;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(255, 68, 68, 0.3);
}

/* --- Reiniciar Monitoreo Button --- */
#reiniciar-btn {
  background-color: #3a3a3a;
  color: #e0e0e0;
  border: 1px solid #4a9eff;
}

#reiniciar-btn:hover {
  background-color: #4a4a4a;
}

```

### Archivo: `css/monitor_pro.css`
```css
/* cSpell:disable */
/* ======================================================= */
/* VERSIÓN PROFESIONAL Y DE ÉNFASIS (monitor_pro.css) */
/* ======================================================= */

:root {
  --bg: #1e2124;
  --text: #f4f7f9;
  --accent: #7289da;
  --container-bg: #282b2f;
  --container-radius: 8px;
  --container-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
  --header-color: #7289da;
  --link-color: #7289da;
  --table-header-bg: #36393f;
  --table-header-color: #ffffff;
}

body {
  background-color: var(--bg);
  color: var(--text);
}
h1 {
  color: var(--header-color);
}

/* Colores de información definidos a nivel de tema; la estructura se movió a monitor_base */
.info-bar {
  color: var(--info-color, #99aab5);
}
.update-info {
  color: var(--update-info-color, #ffffff);
}

/* table-scroll-container movido a monitor_base.css */

/* --- Estilos de la Tabla --- */
#monitor-table {
  width: 100%;
  border-collapse: collapse;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 20px;
}
#monitor-table th,
#monitor-table td {
  padding: 12px 10px;
  text-align: left;
  border-bottom: 1px solid #36393f; /* Líneas divisorias oscuras */
}
#monitor-table td a {
  color: #7289da;
  text-decoration: none;
  font-size: 0.9em;
  word-break: break-all;
}

/* Encabezados */
#monitor-table thead th {
  background-color: #36393f; /* Fondo oscuro */
  color: #ffffff;
  font-weight: bold;
  text-transform: uppercase;
}

/* Filas alternas (Cebra) */
#monitor-table tbody tr:nth-child(odd) {
  background-color: #2f3136; /* Fila par */
}
#monitor-table tbody tr:nth-child(even) {
  background-color: #282b2f; /* Fila impar */
}
#monitor-table tr:hover {
  background-color: #3e4147; /* Resaltado en oscuro */
}

/* Centrar el texto en las columnas de datos (se mantiene) */
#monitor-table td:nth-child(3),
#monitor-table td:nth-child(4),
#monitor-table td:nth-child(5),
#monitor-table td:nth-child(6),
#monitor-table td:nth-child(7),
#monitor-table td:nth-child(8) {
  text-align: center;
}

/* --- Estilos del Botón de Acción (PSI) --- */
.psi-button,
.btn-primary {
  background-color: #43b581; /* Verde de éxito */
  color: #23272a;
  border: none;
  padding: 5px 10px;
  border-radius: 12px; /* Botón más redondeado */
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
}
.psi-button:hover,
.btn-primary:hover {
  background-color: #57d498;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(67, 181, 129, 0.3);
}

.error-button {
  background-color: #f04747; /* Rojo Discord */
  color: #ffffff;
  border: none;
  padding: 5px 10px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
  margin-left: 5px;
}
.error-button:hover {
  background-color: #d84040;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(240, 71, 71, 0.3);
}

/* ======================================================= */
/* --- Colores de Estado (PRO: Énfasis en el Borde Izquierdo) --- */
/* ======================================================= */

/* Presentación: mantener el texto de estado en negrita; los visuales (color/fondo) se definen mediante variables en monitor_base/tema */
.status-very-fast,
.status-fast,
.status-normal,
.status-slow,
.status-critical,
.status-risk,
.status-extreme-risk {
  font-weight: bold;
}
/* El fondo se mantiene transparente en el tema pro para preservar el color de la fila; el borde izquierdo indica la gravedad */

/* 1. Muy Rápido (0 - 300 ms) -> VERDE */
/* Colores de estado asignados mediante variables (tema pro); conservar borde izquierdo y animación cuando aplique */
:root {
  --status-very-fast-bg: transparent;
  --status-very-fast-color: #43b581;
  --status-fast-bg: transparent;
  --status-fast-color: #57d498;
  --status-normal-bg: transparent;
  --status-normal-color: #7289da;
  --status-slow-bg: transparent;
  --status-slow-color: #faa61a;
  --status-critical-bg: transparent;
  --status-critical-color: #e65100;
  --status-risk-bg: transparent;
  --status-risk-color: #dc3545;
  --status-extreme-risk-bg: transparent;
  --status-extreme-risk-color: #ff0000;
  --status-down-bg: #ff0000;
  --status-down-color: white;
}

/* Conservar comportamiento de borde izquierdo/animación para el tema pro */
.status-critical {
  border-left: 3px solid var(--status-critical-color);
}
.status-risk {
  border-left: 3px solid var(--status-risk-color);
}
.status-extreme-risk {
  border-left: 3px solid var(--status-extreme-risk-color);
  animation: pulse-border 2s infinite;
}
.status-down {
  border-radius: 2px;
}

/* --- Keyframe para animación de extremo riesgo --- */
@keyframes pulse-border {
  0% {
    border-color: #ff0000;
  }
  50% {
    border-color: rgba(255, 0, 0, 0.4);
  }
  100% {
    border-color: #ff0000;
  }
}

/* --- Estilo para la Latencia (Métricas) --- */
.latency-metric {
  font-weight: bold;
  font-size: 1em;
  /* Sombra de texto sutil para destacar el número */
  text-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
  color: #ffffff; /* Asegura color blanco para el número */
}

/* --- Responsive (se mantiene) --- */
@media (max-width: 768px) {
  .container {
    padding: 10px;
    border-radius: 0;
    box-shadow: none;
  }
  h1 {
    font-size: 1.5em;
  }
  #monitor-table {
    font-size: 0.8em;
  }
}

/* ======================================================= */
/* REGLAS FINALES: Control de Visibilidad de Columnas */
/* ======================================================= */

/* Oculta la columna URL (2da columna) */
#monitor-table th:nth-child(2),
#monitor-table td:nth-child(2) {
  display: none;
}

/* Muestra la columna Status HTTP (7ma columna) en el tema pro */
#monitor-table th:nth-child(7),
#monitor-table td:nth-child(7) {
  display: table-cell;
}

/* Oculta la columna de Acción (8va columna) */
#monitor-table th:nth-child(8),
#monitor-table td:nth-child(8) {
  display: none;
}

/* ======================================================= */
/* ENLACES (mantener la legibilidad en tema oscuro) */
/* ======================================================= */
#monitor-table td:nth-child(1) a {
  text-decoration: none;
  color: #ffffff; /* Texto blanco en la columna de servicio */
  font-weight: bold;
}

```

### Archivo: `css/monitor_pro2.css`
```css
/* cSpell:disable */
/* ======================================================= */
/* VERSIÓN PROFESIONAL CLARA (monitor_pro2.css) */
/* Paleta de colores clara manteniendo el estilo profesional */
/* ======================================================= */

:root {
  --bg: #f0f2f5;
  --text: #1a1a1a;
  --accent: #5865f2;
  --container-bg: #ffffff;
  --container-radius: 8px;
  --container-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
  --header-color: #5865f2;
  --link-color: #5865f2;
  --table-header-bg: #e3e5e8;
  --table-header-color: #2c2f33;
}

body {
  background-color: var(--bg);
  color: var(--text);
}
h1 {
  color: var(--header-color);
}

/* Colores de información definidos a nivel de tema */
.info-bar {
  color: var(--info-color, #5b6270);
}
.update-info {
  color: var(--update-info-color, #2c2f33);
}

/* table-scroll-container movido a monitor_base.css */

/* --- Estilos de la Tabla --- */
#monitor-table {
  width: 100%;
  border-collapse: collapse;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 20px;
}
#monitor-table th,
#monitor-table td {
  padding: 12px 10px;
  text-align: left;
  border-bottom: 1px solid #e3e5e8;
}
#monitor-table td a {
  color: #5865f2;
  text-decoration: none;
  font-size: 0.9em;
  word-break: break-all;
}

/* Encabezados */
#monitor-table thead th {
  background-color: #e3e5e8;
  color: #2c2f33;
  font-weight: bold;
  text-transform: uppercase;
}

/* Filas alternas (Cebra) */
#monitor-table tbody tr:nth-child(odd) {
  background-color: #ffffff;
}
#monitor-table tbody tr:nth-child(even) {
  background-color: #f8f9fa;
}
#monitor-table tr:hover {
  background-color: #e8eaf0;
}

/* Centrar el texto en las columnas de datos */
#monitor-table td:nth-child(3),
#monitor-table td:nth-child(4),
#monitor-table td:nth-child(5),
#monitor-table td:nth-child(6),
#monitor-table td:nth-child(7),
#monitor-table td:nth-child(8) {
  text-align: center;
}

/* --- Estilos del Botón de Acción (PSI) --- */
.psi-button,
.btn-primary {
  background-color: #3ba55d;
  color: #ffffff;
  border: none;
  padding: 5px 10px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
}
.psi-button:hover,
.btn-primary:hover {
  background-color: #2d8049;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(59, 165, 93, 0.3);
}

.error-button {
  background-color: #ed4245;
  color: #ffffff;
  border: none;
  padding: 5px 10px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 0.8em;
  font-weight: 500;
  transition: all 0.2s;
  margin-left: 5px;
}
.error-button:hover {
  background-color: #c03537;
  transform: translateY(-2px);
  box-shadow: 0 2px 8px rgba(237, 66, 69, 0.3);
}

/* ======================================================= */
/* --- Colores de Estado (PRO2: Énfasis en el Borde Izquierdo con Paleta Clara) --- */
/* ======================================================= */

/* Presentación: mantener el texto de estado en negrita */
.status-very-fast,
.status-fast,
.status-normal,
.status-slow,
.status-critical,
.status-risk,
.status-extreme-risk {
  font-weight: bold;
}

/* Colores de estado optimizados para tema claro */
:root {
  --status-very-fast-bg: transparent;
  --status-very-fast-color: #2d8049;
  --status-fast-bg: transparent;
  --status-fast-color: #3ba55d;
  --status-normal-bg: transparent;
  --status-normal-color: #5865f2;
  --status-slow-bg: transparent;
  --status-slow-color: #f59e0b;
  --status-critical-bg: transparent;
  --status-critical-color: #dc6900;
  --status-risk-bg: transparent;
  --status-risk-color: #c03537;
  --status-extreme-risk-bg: transparent;
  --status-extreme-risk-color: #d32f2f;
  --status-down-bg: #d32f2f;
  --status-down-color: white;
}

/* Conservar comportamiento de borde izquierdo/animación */
.status-critical {
  border-left: 3px solid var(--status-critical-color);
}
.status-risk {
  border-left: 3px solid var(--status-risk-color);
}
.status-extreme-risk {
  border-left: 3px solid var(--status-extreme-risk-color);
  animation: pulse-border 2s infinite;
}
.status-down {
  border-radius: 2px;
}

/* --- Keyframe para animación de extremo riesgo --- */
@keyframes pulse-border {
  0% {
    border-color: #d32f2f;
  }
  50% {
    border-color: rgba(211, 47, 47, 0.4);
  }
  100% {
    border-color: #d32f2f;
  }
}

/* --- Estilo para la Latencia (Métricas) --- */
.latency-metric {
  font-weight: bold;
  font-size: 1em;
  text-shadow: 0 0 5px rgba(0, 0, 0, 0.05);
  color: #1a1a1a;
}

/* --- Responsive --- */
@media (max-width: 768px) {
  .container {
    padding: 10px;
    border-radius: 0;
    box-shadow: none;
  }
  h1 {
    font-size: 1.5em;
  }
  #monitor-table {
    font-size: 0.8em;
  }
}

/* ======================================================= */
/* REGLAS FINALES: Control de Visibilidad de Columnas */
/* ======================================================= */

/* Oculta la columna URL (2da columna) */
#monitor-table th:nth-child(2),
#monitor-table td:nth-child(2) {
  display: none;
}

/* Muestra la columna Status HTTP (7ma columna) en el tema pro2 */
#monitor-table th:nth-child(7),
#monitor-table td:nth-child(7) {
  display: table-cell;
}
/* Muestra la columna de Latencia Promedio Extra */
#monitor-table th:nth-child(8),
#monitor-table td:nth-child(8) {
  display: table-cell;
}

```

### Archivo: `data/webs.json`
```json
[
  {
    "nombre": "Google",
    "url": "https://www.google.com",
    "orden": 1,
    "grupo": "CRITICO"
  },
  {
    "nombre": "AhiVa",
    "url": "https://ahiva.correo.com.uy"
  },
  {
    "nombre": "CasillaMia",
    "url": "https://www.casillamia.uy"
  },
  {
    "nombre": "Correo",
    "url": "https://www.correo.com.uy/home",
    "grupo": "CRITICO"
  },
  {
    "nombre": "ExportaFacil",
    "url": "http://www.exportafacil.com.uy/exportafacil"
  },
  {
    "nombre": "Aduanas Clientes",
    "url": "https://ahiva.correo.com.uy/aduanas-web/login?1"
  },
  {
    "nombre": "ProdAgenda",
    "url": "https://prodagenda.correo.com.uy/sae/agendarReserva/Paso1.xhtml?e=2&a=2&r=6",
    "grupo": "CRITICO"
  },
  {
    "nombre": "Notas",
    "url": "https://notas.correo.com.uy/notas"
  }
]
```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/monitor-status-test/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Monitor de Servicios</title>
    <link
      rel="icon"
      href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>📊</text></svg>"
    />

    <style>
      body {
        margin: 0;
        padding: 20px;
      }
      .container {
        max-width: 1000px;
        margin: 0 auto;
        padding: 30px;
        min-height: 60vh;
      }
      h1 {
        text-align: center;
        margin-bottom: 5px;
      }
      .info-bar {
        text-align: center;
        margin-bottom: 20px;
        font-size: 0.9em;
      }
      .update-info {
        text-align: center;
        margin-bottom: 20px;
      }
      #monitor-table {
        width: 100%;
        border-collapse: collapse;
        margin-bottom: 20px;
      }
      #monitor-table th,
      #monitor-table td {
        text-align: left;
      }
      #monitor-table td a {
        text-decoration: none;
        font-size: 0.9em;
        word-break: break-all;
      }
      #monitor-table td:nth-child(n + 3):nth-child(-n + 8) {
        text-align: center;
      }
      .psi-button,
      .btn-primary {
        border: none;
        padding: 5px 8px;
        border-radius: 4px;
        cursor: pointer;
        font-size: 0.8em;
        transition: background-color 0.2s;
      }
      .latency-metric {
        font-size: 1em;
      }
    </style>

    <link rel="stylesheet" href="css/monitor_base.css" />
    <link id="estilo-principal" rel="stylesheet" href="css/monitor_def.css" />
  </head>
  <body>
    <main>
      <!-- Botón Toggle Modo Oscuro - Esquina Superior Derecha -->
      <button
        id="theme-toggle-btn"
        class="theme-toggle-button-corner"
        onclick="toggleDarkMode()"
        title="Cambiar modo de visualización"
      >
        <span id="theme-icon">🌙</span>
      </button>

      <div class="container">
        <h1 id="titulo-principal">Monitor de Estado de Servicios</h1>

        <div style="text-align: center; margin-bottom: 15px">
          <label
            for="duracion-selector"
            style="font-size: 0.9em; margin-right: 8px"
            >Duración del historial:</label
          >
          <select
            id="duracion-selector"
            style="
              padding: 5px 10px;
              border-radius: 4px;
              border: 1px solid #ccc;
              font-size: 0.9em;
              cursor: pointer;
            "
            aria-label="Seleccionar duración del historial"
          ></select>
          <button
            id="btn-reiniciar"
            class="btn-primary"
            style="margin-left: 15px; padding: 5px 15px; font-size: 0.9em"
            onclick="reiniciarMonitoreo()"
            aria-label="Reiniciar monitoreo de servicios"
          >
            <span aria-hidden="true">🔄</span>
            <span id="texto-btn-reiniciar">Reiniciar Monitoreo</span>
          </button>
        </div>

        <div
          class="info-bar"
          id="info-bar-msg"
          role="status"
          aria-live="polite"
        ></div>

        <div style="text-align: center; margin-bottom: 10px">
          <div class="menu-horizontal">
            <a
              href="leyenda.html"
              id="enlace-leyenda"
              target="_blank"
              rel="noopener noreferrer"
              class="menu-item"
              title="Ver rangos y colores de latencia"
            >
              📊 Leyenda
            </a>
            <a
              href="abm-webs.html"
              id="enlace-abm"
              class="menu-item"
              style="display: none"
              title="Administrar sitios web monitoreados"
            >
              ⚙️ ABM Webs
            </a>
          </div>
        </div>

        <div
          class="update-info"
          id="ultima-actualizacion"
          role="status"
          aria-live="polite"
        ></div>

        <!-- Los filtros se insertarán aquí dinámicamente -->

        <div class="table-scroll-container">
          <table
            id="monitor-table"
            role="table"
            aria-label="Tabla de monitoreo de servicios"
          >
            <thead>
              <tr>
                <th id="header-service" scope="col"></th>
                <th id="header-url" scope="col"></th>
                <th id="header-latency-actual" scope="col"></th>
                <th id="header-status-actual" scope="col"></th>
                <th id="header-promedio-ms" scope="col"></th>
                <th id="header-promedio-status" scope="col"></th>
                <th id="header-action" scope="col"></th>
              </tr>
            </thead>
            <tbody id="status-table-body"></tbody>
          </table>
        </div>
      </div>
    </main>

    <!-- Contenedor de notificaciones (se crea dinámicamente) -->

    <script defer src="js/config.js"></script>
    <script defer src="js/i18n.js"></script>
    <script defer src="js/rum.js"></script>
    <script defer src="js/script.js"></script>
  </body>
</html>

```

### Archivo: `js/alertas_error.js`
```javascript
// alertas_error.js
// Muestra un alert en el frontend solo la primera vez que un sitio da error en una hora
// Requiere que se llame a window.registrarErrorSitio(nombre, url, latencia, codigo, descripcion) cuando se detecta un error
(function () {
  // Guarda el último error notificado por sitio y hora (en memoria, por sesión)
  // Usa localStorage para persistir los errores notificados por sitio y hora
  // (Eliminada función duplicada getErroresNotificados)
  // Usamos sessionStorage para mantener el comportamiento por sesión
  function getErroresNotificados() {
    try {
      return (
        JSON.parse(sessionStorage.getItem('erroresNotificadosMonitor')) || {}
      );
    } catch (e) {
      console.warn('Error leyendo erroresNotificadosMonitor:', e);
      return {};
    }
  }
  function setErroresNotificados(obj) {
    try {
      sessionStorage.setItem('erroresNotificadosMonitor', JSON.stringify(obj));
    } catch (e) {
      console.warn('Error guardando erroresNotificadosMonitor:', e);
    }
  }

  // Asegura que el CSS de notificaciones esté cargado
  function ensureAlertasCss() {
    try {
      if (typeof document === 'undefined' || !document.head) return;
      if (document.getElementById('alertas-error-css')) return;
      const link = document.createElement('link');
      link.id = 'alertas-error-css';
      link.rel = 'stylesheet';
      link.href = 'css/alertas_error.css';
      document.head.appendChild(link);
    } catch (e) {
      console.warn('No se pudo cargar css alertas:', e);
    }
  }

  // Función para limpiar el registro de error de un sitio (cuando se recupera)
  window.limpiarErrorSitio = function (nombre) {
    const erroresNotificados = getErroresNotificados();
    if (erroresNotificados[nombre]) {
      delete erroresNotificados[nombre];
      setErroresNotificados(erroresNotificados);
    }
  };

  // Función global para registrar un error y mostrar alert si corresponde
  window.registrarErrorSitio = function (
    nombre,
    url,
    latencia,
    codigo,
    descripcion
  ) {
    const clave = nombre;
    const erroresNotificados = getErroresNotificados();
    if (erroresNotificados[clave]) return; // Ya se notificó este sitio hasta que se recupere
    erroresNotificados[clave] = true;
    setErroresNotificados(erroresNotificados);
    let label = '',
      desc = '';
    if (
      window.TEXTOS_ACTUAL &&
      window.TEXTOS_ACTUAL.httpStatus &&
      window.TEXTOS_ACTUAL.httpCodes
    ) {
      const info = window.TEXTOS_ACTUAL.httpCodes.find(
        (e) => e.code === codigo
      );
      if (info) {
        label = info.label;
        desc = info.description;
      }
    }
    let mensaje = `ALERTA: Error detectado en "${nombre}"\n`;
    mensaje += `URL: ${url}\n`;
    if (latencia !== undefined) mensaje += `Latencia: ${latencia} ms\n`;
    mensaje += `Código: ${codigo}`;
    if (label) mensaje += ` - ${label}`;
    mensaje += `\n`;
    if (desc) mensaje += `Descripción: ${desc}`;
    else if (descripcion) mensaje += `Descripción: ${descripcion}`;
    mostrarNotificacionError(mensaje);
  };

  // Crea una notificación flotante con el mensaje y botón de copiar
  function mostrarNotificacionError(mensaje) {
    // Crear contenedor si no existe
    if (typeof document === 'undefined' || !document.body) return;

    ensureAlertasCss();

    let contenedor = document.getElementById('notificaciones-errores');
    if (!contenedor) {
      contenedor = document.createElement('div');
      contenedor.id = 'notificaciones-errores';
      document.body.appendChild(contenedor);
    }

    // Crear la notificación
    const notif = document.createElement('div');
    notif.classList.add('notif-alarma');

    // Texto seleccionable
    const texto = document.createElement('div');
    texto.textContent = mensaje;
    texto.classList.add('texto');
    notif.appendChild(texto);

    // Botón copiar
    const btnCopiar = document.createElement('button');
    const LABEL_COPIAR =
      (window.TEXTOS_ACTUAL &&
        window.TEXTOS_ACTUAL.general &&
        window.TEXTOS_ACTUAL.general.BTN_COPIAR) ||
      'Copiar';
    const LABEL_COPIADO =
      (window.TEXTOS_ACTUAL &&
        window.TEXTOS_ACTUAL.general &&
        window.TEXTOS_ACTUAL.general.BTN_COPIADO) ||
      '¡Copiado!';
    btnCopiar.textContent = LABEL_COPIAR;
    btnCopiar.classList.add('btn-copiar');
    btnCopiar.onclick = function () {
      // Intentar usar clipboard API, con fallback a execCommand
      if (navigator && navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard
          .writeText(mensaje)
          .then(() => {
            btnCopiar.textContent = LABEL_COPIADO;
            setTimeout(
              () => {
                btnCopiar.textContent = LABEL_COPIAR;
              },
              typeof ALERTAS_COPY_LABEL_RESET_MS !== 'undefined'
                ? ALERTAS_COPY_LABEL_RESET_MS
                : 1500
            );
          })
          .catch((err) => {
            console.warn('Clipboard API falló, intentando fallback:', err);
            fallbackCopyText(mensaje, btnCopiar, LABEL_COPIAR, LABEL_COPIADO);
          });
      } else {
        fallbackCopyText(mensaje, btnCopiar, LABEL_COPIAR, LABEL_COPIADO);
      }
    };
    notif.appendChild(btnCopiar);

    // Botón cerrar
    const btnCerrar = document.createElement('button');
    btnCerrar.textContent = '×';
    btnCerrar.classList.add('btn-cerrar');
    btnCerrar.onclick = function () {
      contenedor.removeChild(notif);
    };
    btnCerrar.setAttribute('aria-label', 'Cerrar notificación');
    notif.appendChild(btnCerrar);

    contenedor.appendChild(notif);
    // Auto-cerrar después del tiempo configurado
    setTimeout(
      () => {
        if (contenedor.contains(notif)) contenedor.removeChild(notif);
      },
      typeof ALERTAS_AUTO_CLOSE_MS !== 'undefined'
        ? ALERTAS_AUTO_CLOSE_MS
        : 30000
    );
  }

  // Fallback de copia para navegadores sin Clipboard API
  function fallbackCopyText(text, btn, labelCopiar, labelCopiado) {
    try {
      const textarea = document.createElement('textarea');
      textarea.value = text;
      textarea.setAttribute('readonly', '');
      textarea.style.position = 'absolute';
      textarea.style.left = '-9999px';
      document.body.appendChild(textarea);
      textarea.select();
      textarea.setSelectionRange(0, textarea.value.length);
      const successful = document.execCommand('copy');
      document.body.removeChild(textarea);
      if (successful) {
        btn.textContent = labelCopiado;
        setTimeout(
          () => {
            btn.textContent = labelCopiar;
          },
          typeof ALERTAS_COPY_LABEL_RESET_MS !== 'undefined'
            ? ALERTAS_COPY_LABEL_RESET_MS
            : 1500
        );
      } else {
        console.warn('fallbackCopyText: execCommand(copy) no fue exitoso');
      }
    } catch (e) {
      console.warn('fallbackCopyText error:', e);
    }
  }
})();

```

### Archivo: `js/config.js`
```javascript
// Umbrales de latencia (en milisegundos)
// Estos valores sirven para clasificar qué tan rápido o lento responde un servicio.
// Si querés saber más, mirá el archivo docs/justificacion_rangos_latencia.md
const UMBRALES_LATENCIA = {
  MUY_RAPIDO: 300, // Excelente: responde casi al instante
  RAPIDO: 500, // Bueno: rápido, se nota poco el retraso
  NORMAL: 800, // Aceptable: algo lento pero usable
  LENTO: 1500, // Lento: se nota el retraso
  CRITICO: 3000, // Muy lento: ya es un problema serio
  RIESGO: 5000, // Al borde del fallo: casi no responde
  PENALIZACION_FALLO: 99999, // Valor especial para marcar fallos (no cuenta en el promedio)
};

// -----------------------------
// 1) Temas y archivos CSS (UI)
// -----------------------------
// Nombres de temas disponibles en la UI
const TEMA_DEFAULT = 'def';
const TEMA_PRO = 'pro';
const TEMA_PRO2 = 'pro2'; // Versión clara de PRO
const TEMA_MIN = 'min';
const TEMA_OSC = 'osc';

// Archivo CSS asociado a cada tema
const TEMA_FILES = {
  [TEMA_DEFAULT]: 'css/monitor_def.css',
  [TEMA_PRO]: 'css/monitor_pro.css',
  [TEMA_PRO2]: 'css/monitor_pro2.css',
  [TEMA_MIN]: 'css/monitor_min.css',
  [TEMA_OSC]: 'css/monitor_osc.css',
};

// Pares de temas para alternar (botón cambiar tema)
// Si un tema no tiene pareja, el botón de alternar se oculta
const TEMA_TOGGLE_PAIRS = {
  [TEMA_DEFAULT]: TEMA_OSC, // def (claro) → osc (oscuro)
  [TEMA_OSC]: TEMA_DEFAULT, // osc (oscuro) → def (claro)
  [TEMA_PRO2]: TEMA_PRO, // pro2 (claro) → pro (oscuro)
  [TEMA_PRO]: TEMA_PRO2, // pro (oscuro) → pro2 (claro)
};

// Temas que no muestran funciones avanzadas (ej. detalles de error)
const TEMAS_BASICOS = [TEMA_DEFAULT];

const DEFAULT_LEYENDA_TEMA = TEMA_DEFAULT;
const LEYENDA_TEMA_FILES = {
  default: 'css/leyenda_claro.css',
  def: 'css/leyenda_claro.css',
  pro2: 'css/leyenda_claro.css',
  min: 'css/leyenda_claro.css',
  pro: 'css/leyenda_oscuro.css',
  osc: 'css/leyenda_oscuro.css',
};

// -----------------------------
// 2) Umbrales y estados
// -----------------------------
const ESTADO_ERROR_CONEXION = 0;

const HTTP_STATUS_SUCCESS = {
  OK: 200,
  CREATED: 201,
  NO_CONTENT: 204,
  MOVED_PERMANENTLY: 301,
  FOUND: 302,
  NOT_MODIFIED: 304,
};

const HTTP_STATUS_ERROR = {
  NO_CONNECTION: 0,
  BAD_REQUEST: 400,
  UNAUTHORIZED: 401,
  PAYMENT_REQUIRED: 402,
  FORBIDDEN: 403,
  NOT_FOUND: 404,
  METHOD_NOT_ALLOWED: 405,
  NOT_ACCEPTABLE: 406,
  TIMEOUT: 408,
  CONFLICT: 409,
  GONE: 410,
  IM_A_TEAPOT: 418,
  TOO_MANY_REQUESTS: 429,
  INTERNAL_ERROR: 500,
  NOT_IMPLEMENTED: 501,
  BAD_GATEWAY: 502,
  SERVICE_UNAVAILABLE: 503,
  GATEWAY_TIMEOUT: 504,
};

// -----------------------------
// 3) Proxy, frecuencia y archivos
// -----------------------------
const PROXY_ENDPOINT = '/.netlify/functions/check-status';
// Frecuencia de monitoreo en milisegundos (por defecto: 5 minutos)
const FRECUENCIA_MONITOREO_MS = 5 * 60 * 1000;

const WEBSITES_FILE = 'data/webs.json';
const PSI_BASE_URL = 'https://pagespeed.web.dev/report?url=';

// -----------------------------
// 4) Opciones de historial/duración
// -----------------------------
const HORAS_DISPONIBLES = [1, 2, 3, 4, 5, 6, 7, 8, 9];

// Cada hora equivale a 12 mediciones (una cada 5 minutos)
const DURACION_OPCIONES = {};
HORAS_DISPONIBLES.forEach((horas) => {
  const key = `${horas}h`;
  DURACION_OPCIONES[key] = {
    mediciones: horas * 12,
    etiqueta: horas === 1 ? '1 hora' : `${horas} horas`,
  };
});

const DURACION_DEFAULT = '1h';
const MAX_HISTORIAL_ENTRIES = DURACION_OPCIONES[DURACION_DEFAULT].mediciones;

// -----------------------------
// 5) I18N (archivos de idioma)
// -----------------------------
// Lista de archivos de traducción disponibles (clave de idioma -> archivo)
const I18N_FILES = {
  es: 'lang/i18n_es.js',
  en: 'lang/i18n_en.js',
  fr: 'lang/i18n_fr.js',
};

const DEFAULT_LANG = 'es'; // Idioma por defecto

// -----------------------------
// 6) Detección de fallo global
// -----------------------------
// Se activa si el 80% o más de los servicios críticos fallan o tardan más de 9 segundos
const GRUPO_CRITICO_NOMBRE = 'CRITICO'; // Nombre del grupo de servicios críticos
const UMBRAL_FALLO_GLOBAL_MS = 9000; // Si tarda más de 9 segundos, se considera fallo
const PORCENTAJE_FALLO_GLOBAL = 0.8; // Umbral en proporción (0.8 = 80%)

// -----------------------------
// 7) Configuraciones de alertas en frontend (UI)
// -----------------------------
// Tiempo por defecto para auto-cerrar las notificaciones (ms)
const ALERTAS_AUTO_CLOSE_MS = 30000;
// Tiempo para restaurar la etiqueta del botón copiar (ms)
const ALERTAS_COPY_LABEL_RESET_MS = 1500;

// -----------------------------
// 8) Umbrales de latencia (definición colocada al final por visibilidad)
// -----------------------------
// (Se mantiene la definición arriba; esta sección es referencial)

```

### Archivo: `js/i18n.js`
```javascript
// Sistema de idiomas (i18n)
// Acá está la lógica para cargar y manejar los textos traducidos
// Los textos están en archivos como: i18n_es.js, i18n_en.js, etc.

// Detecta qué idioma hay que cargar según el parámetro ?lang= de la URL
function obtenerIdiomaSeleccionado() {
  const params = new URLSearchParams(window.location.search);
  const langUrl = params.get('lang');

  if (langUrl && I18N_FILES[langUrl]) {
    return langUrl;
  }

  return DEFAULT_LANG;
}

// Carga el archivo JS del idioma de forma dinámica
// Crea un <script> y lo mete en el <head> del documento
function cargarIdiomaScript(idiomaACargar) {
  const filePath = I18N_FILES[idiomaACargar];

  if (!filePath) {
    return Promise.reject(
      new Error(
        `Error de configuración: Archivo de idioma no definido para ${idiomaACargar}`
      )
    );
  }

  return new Promise((resolve, reject) => {
    const script = document.createElement('script');
    script.src = filePath;
    script.type = 'text/javascript';

    script.onload = () => {
      if (window.TEXTOS_ACTUAL) {
        resolve();
      } else {
        reject(
          new Error(
            `El archivo de idioma ${filePath} no asignó la variable TEXTOS_ACTUAL`
          )
        );
      }
    };

    script.onerror = () => {
      reject(new Error(`Fallo al cargar el script de idioma: ${filePath}`));
    };

    document.head.appendChild(script);
  });
}

// Función principal para cargar el idioma, con intento de respaldo automático
// Si el idioma que pediste falla, intenta cargar el idioma por defecto
async function cargarIdioma() {
  const idiomaSolicitado = obtenerIdiomaSeleccionado();
  const idiomaDefault = DEFAULT_LANG;

  try {
    await cargarIdiomaScript(idiomaSolicitado);
    return;
  } catch (errorSolicitado) {
    if (idiomaSolicitado !== idiomaDefault) {
      try {
        await cargarIdiomaScript(idiomaDefault);
        return;
      } catch (errorDefault) {
        throw new Error(
          `Fallo crítico: El idioma solicitado (${idiomaSolicitado}) y el de reserva (${idiomaDefault}) fallaron en la carga.`
        );
      }
    }

    throw new Error(
      `Fallo crítico: No se pudo cargar el idioma por defecto (${idiomaDefault}).`
    );
  }
}

```

### Archivo: `js/leyenda_script.js`
```javascript
// Script para la página de leyenda (leyenda.html)
// Carga el tema y el idioma según los parámetros de la URL
// y muestra la tabla de umbrales de forma dinámica
document.addEventListener('DOMContentLoaded', async () => {
  const params = new URLSearchParams(window.location.search);
  let temaParam = params.get('tema') ? params.get('tema').toLowerCase() : null;
  const idioma = params.get('lang') || DEFAULT_LANG;

  // Si no hay tema en la URL o el tema no existe, usamos el tema por defecto
  if (!temaParam || !LEYENDA_TEMA_FILES[temaParam]) {
    temaParam = DEFAULT_LEYENDA_TEMA;
  }

  // Cargamos el CSS del tema elegido
  const temaBaseLink = document.getElementById('tema-base-css');
  const rutaCssTema =
    typeof LEYENDA_TEMA_FILES !== 'undefined'
      ? LEYENDA_TEMA_FILES[temaParam] ||
        LEYENDA_TEMA_FILES[DEFAULT_LEYENDA_TEMA]
      : null;

  if (temaBaseLink && rutaCssTema) {
    // Le ponemos un número único para que el navegador no use el cache
    temaBaseLink.href = `${rutaCssTema}?v=2025121501`;
    document.body.classList.add(`theme-${temaParam}`);
  } else if (!temaBaseLink) {
    console.warn("Elemento con ID 'tema-base-css' no encontrado.");
  }

  // Actualizamos el botón para cambiar de tema
  actualizarBotonToggle(temaParam);

  // Cargamos el archivo de traducción del idioma elegido
  async function loadI18n(language) {
    const file =
      typeof I18N_FILES !== 'undefined' && I18N_FILES[language]
        ? I18N_FILES[language]
        : typeof I18N_FILES !== 'undefined'
        ? I18N_FILES[DEFAULT_LANG]
        : null;
    if (!file) return Promise.resolve();
    return new Promise((resolve, reject) => {
      const script = document.createElement('script');
      script.src = file;
      script.onload = () => resolve();
      script.onerror = () =>
        reject(new Error(`Failed to load i18n file: ${file}`));
      document.head.appendChild(script);
    });
  }

  try {
    await loadI18n(idioma);
  } catch (err) {
    console.warn(
      'Language script failed to load, falling back to default lang.',
      err
    );
    if (idioma !== DEFAULT_LANG) {
      try {
        await loadI18n(DEFAULT_LANG);
      } catch (e) {
        /* ignore */
      }
    }
  }
  // Muestra la tabla de umbrales de latencia con detalles que se pueden desplegar
  function renderLeyendaContent(container, leyendaData) {
    if (!container || !leyendaData) return;

    container.innerHTML = '';

    const introP = document.createElement('p');
    introP.textContent = leyendaData.intro || '';
    container.appendChild(introP);

    const umbralesSection = document.createElement('div');
    umbralesSection.className = 'leyenda-section umbrales-latencia';

    const table = document.createElement('table');
    table.className = 'leyenda-tabla-umbrales';
    const thead = document.createElement('thead');
    thead.innerHTML = `
            <tr>
                <th>Estado / Nivel</th>
                <th>Umbral de Latencia (ms)</th>
                <th>Justificación de los Umbrales de Latencia</th>
            </tr>`;
    table.appendChild(thead);

    const tbody = document.createElement('tbody');
    (leyendaData.umbrales || []).forEach((u) => {
      const tr = document.createElement('tr');

      const tdStatus = document.createElement('td');
      tdStatus.className = u.className || '';
      tdStatus.textContent = `${u.emoji || ''} ${u.label || ''}`;
      tr.appendChild(tdStatus);

      const tdRange = document.createElement('td');
      tdRange.textContent = u.range_text || '';
      tr.appendChild(tdRange);

      const tdJustif = document.createElement('td');
      const details = document.createElement('details');
      const summary = document.createElement('summary');
      summary.textContent = u.summary || '';
      const p = document.createElement('p');
      p.textContent = u.details || '';
      details.appendChild(summary);
      details.appendChild(p);
      tdJustif.appendChild(details);
      tr.appendChild(tdJustif);

      tbody.appendChild(tr);
    });

    table.appendChild(tbody);
    umbralesSection.appendChild(table);
    container.appendChild(umbralesSection);

    const codesSection = document.createElement('div');
    codesSection.className = 'leyenda-section codigos-error-section';
    codesSection.style.marginTop = '30px';

    const h3 = document.createElement('h3');
    h3.textContent = leyendaData.http_codes_title || '';
    codesSection.appendChild(h3);

    const pDesc = document.createElement('p');
    pDesc.textContent = leyendaData.http_codes_description || '';
    codesSection.appendChild(pDesc);

    const tableCodes = document.createElement('table');
    tableCodes.className = 'leyenda-tabla-errores';
    const theadCodes = document.createElement('thead');
    theadCodes.innerHTML = `
            <tr>
                <th>Código</th>
                <th>Rango/Nombre Común</th>
                <th>Significado Operacional</th>
            </tr>`;
    tableCodes.appendChild(theadCodes);

    const tbodyCodes = document.createElement('tbody');
    (leyendaData.codigos_error || []).forEach((c) => {
      const tr = document.createElement('tr');
      const tdCode = document.createElement('td');
      tdCode.textContent = c.code || '';
      tr.appendChild(tdCode);
      const tdLabel = document.createElement('td');
      tdLabel.textContent = c.label || '';
      tr.appendChild(tdLabel);
      const tdDesc = document.createElement('td');
      tdDesc.textContent = c.description || '';
      tr.appendChild(tdDesc);
      tbodyCodes.appendChild(tr);
    });
    tableCodes.appendChild(tbodyCodes);
    codesSection.appendChild(tableCodes);

    container.appendChild(codesSection);
  }

  // Cambiamos los textos de la página con las traducciones cargadas
  if (typeof window.TEXTOS_ACTUAL !== 'undefined') {
    const titleEl = document.getElementById('leyenda-titulo');
    const headerEl = document.getElementById('titulo-leyenda');
    const contentEl = document.getElementById('contenido-leyenda');

    if (titleEl)
      titleEl.textContent = window.TEXTOS_ACTUAL.leyenda.title_browser;
    if (headerEl)
      headerEl.textContent = window.TEXTOS_ACTUAL.leyenda.main_header;
    if (contentEl)
      renderLeyendaContent(contentEl, window.TEXTOS_ACTUAL.leyenda);
  } else {
    console.error('No se cargaron las traducciones para la leyenda.');
  }
});

/**
 * Actualiza el icono del botón toggle según el tema actual
 * Oculta el botón si el tema no tiene pareja en TEMA_TOGGLE_PAIRS
 */
function actualizarBotonToggle(temaActual) {
  const themeIcon = document.getElementById('theme-icon');
  const themeBtn = document.getElementById('theme-toggle-btn');

  if (!themeBtn) return;

  // Normalizar temaActual: aceptar 'theme-xxx', rutas CSS o claves
  let tema = temaActual || '';
  if (typeof tema === 'string' && tema.startsWith('theme-')) {
    tema = tema.replace('theme-', '');
  }
  if (
    typeof tema === 'string' &&
    (tema.indexOf('/') !== -1 || tema.indexOf('.css') !== -1)
  ) {
    for (const k in TEMA_FILES) {
      if (
        TEMA_FILES[k] &&
        tema.indexOf(TEMA_FILES[k].split('/').pop()) !== -1
      ) {
        tema = k;
        break;
      }
    }
  }

  const tieneParejaToggle =
    typeof TEMA_TOGGLE_PAIRS !== 'undefined' &&
    TEMA_TOGGLE_PAIRS.hasOwnProperty(tema);

  if (!tieneParejaToggle) {
    themeBtn.style.display = 'none';
    return;
  }

  themeBtn.style.display = 'block';
  if (!themeIcon) return;

  const temaDestino = TEMA_TOGGLE_PAIRS[tema];
  if (!temaDestino) {
    themeIcon.textContent = '🔄';
    themeBtn.setAttribute('title', 'Alternar tema');
    return;
  }

  const destinosOscuros = [TEMA_OSC, TEMA_PRO];
  if (destinosOscuros.includes(temaDestino)) {
    themeIcon.textContent = '🌙';
    themeBtn.setAttribute(
      'title',
      `Cambiar a modo oscuro (${temaDestino.toUpperCase()})`
    );
  } else {
    themeIcon.textContent = '☀️';
    themeBtn.setAttribute(
      'title',
      `Cambiar a modo claro (${temaDestino.toUpperCase()})`
    );
  }
}

/**
 * Alterna entre temas usando TEMA_TOGGLE_PAIRS (consistente con script.js)
 */
function toggleDarkMode() {
  const temaBaseLink = document.getElementById('tema-base-css');
  const params = new URLSearchParams(window.location.search);
  const temaUrl = params.get('tema');

  // Vemos el tema actual desde la URL o usamos el default
  let temaActual = DEFAULT_LEYENDA_TEMA;
  if (temaUrl && LEYENDA_TEMA_FILES[temaUrl]) {
    temaActual = temaUrl;
  }

  // Buscamos la pareja del tema actual en TEMA_TOGGLE_PAIRS
  const nuevoTema =
    typeof TEMA_TOGGLE_PAIRS !== 'undefined'
      ? TEMA_TOGGLE_PAIRS[temaActual]
      : null;

  // Si no hay pareja, no hacemos nada
  if (!nuevoTema) return;

  // Cambiamos al nuevo tema
  if (LEYENDA_TEMA_FILES[nuevoTema]) {
    temaBaseLink.href = LEYENDA_TEMA_FILES[nuevoTema];

    // Cambiamos las clases del body
    document.body.classList.remove(`theme-${temaActual}`);
    document.body.classList.add(`theme-${nuevoTema}`);

    // Cambiamos la URL para reflejar el nuevo tema
    params.set('tema', nuevoTema);
    const nuevaUrl = `${window.location.pathname}?${params.toString()}`;
    window.history.replaceState({}, '', nuevaUrl);

    // Actualizamos el botón para el nuevo tema
    actualizarBotonToggle(nuevoTema);
  }
}

```

### Archivo: `js/rum.js`
```javascript
// RUM (Real User Monitoring) minimal client
(function () {
  const ENDPOINT = '/.netlify/functions/rum-collect';

  function collectAndSend(label) {
    try {
      const nav = (performance.getEntriesByType && performance.getEntriesByType('navigation') && performance.getEntriesByType('navigation')[0]) || {};
      const paints = {};
      if (performance.getEntriesByType) {
        performance.getEntriesByType('paint').forEach((p) => {
          paints[p.name] = Math.round(p.startTime);
        });
      }

      const resources = (performance.getEntriesByType
        ? performance.getEntriesByType('resource').slice(0, 10).map((r) => ({
            name: r.name,
            initiatorType: r.initiatorType,
            duration: Math.round(r.duration),
            transferSize: r.transferSize || null,
            responseStart: Math.round(r.responseStart || 0),
          }))
        : []);

      const connection = navigator.connection || {};

      const payload = {
        url: location.href,
        ts: Date.now(),
        label: label || null,
        nav: {
          responseStart: Math.round(nav.responseStart || 0),
          domContentLoaded: Math.round(nav.domContentLoadedEventEnd || 0),
          load: Math.round(nav.loadEventEnd || 0),
          fetchStart: Math.round(nav.fetchStart || 0),
        },
        paints: paints,
        resources: resources,
        connection: {
          effectiveType: connection.effectiveType || null,
          rtt: connection.rtt || null,
          downlink: connection.downlink || null,
        },
        ua: navigator.userAgent,
      };

      const body = JSON.stringify(payload);

      if (navigator.sendBeacon) {
        const blob = new Blob([body], { type: 'application/json' });
        navigator.sendBeacon(ENDPOINT, blob);
      } else {
        // fallback
        fetch(ENDPOINT, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: body,
          keepalive: true,
        }).catch(() => {});
      }
    } catch (e) {
      // no-op
    }
  }

  window.addEventListener('load', function () {
    // Small delay to allow paint entries to settle
    setTimeout(function () {
      collectAndSend('load');
    }, 1200);
  });

  document.addEventListener('visibilitychange', function () {
    if (document.visibilityState === 'hidden') {
      collectAndSend('hidden-unload');
    }
  });

  if (typeof PerformanceObserver !== 'undefined' && PerformanceObserver.supportedEntryTypes && PerformanceObserver.supportedEntryTypes.includes('paint')) {
    try {
      const obs = new PerformanceObserver((list) => {
        list.getEntries().forEach((entry) => {
          if (entry.name === 'first-contentful-paint') {
            collectAndSend('fcp');
          }
        });
      });
      obs.observe({ type: 'paint', buffered: true });
    } catch (e) {
      // ignore
    }
  }
})();

```

### Archivo: `js/script.js`
```javascript
let temaProActivo = false;
let websitesData = [];
let historialStatus = {};
let maxHistorialActual = MAX_HISTORIAL_ENTRIES;

// Cargar alertas_error.js para alertas de error por hora
(function () {
  const scriptAlertas = document.createElement('script');
  scriptAlertas.src = 'js/alertas_error.js';
  document.head.appendChild(scriptAlertas);
})();

function configurarEnlaceLeyenda() {
  const enlaceLeyenda = document.getElementById('enlace-leyenda');
  if (enlaceLeyenda) {
    enlaceLeyenda.href = `leyenda.html${window.location.search}`;
  }
}

function obtenerDuracionSeleccionada() {
  const guardado = localStorage.getItem('duracionMonitoreo');
  return guardado && DURACION_OPCIONES[guardado] ? guardado : DURACION_DEFAULT;
}

function guardarDuracionSeleccionada(duracion) {
  localStorage.setItem('duracionMonitoreo', duracion);
}

function inicializarSelectorDuracion() {
  const selector = document.getElementById('duracion-selector');
  if (!selector) return;

  selector.innerHTML = '';

  Object.keys(DURACION_OPCIONES).forEach((key) => {
    const opcion = DURACION_OPCIONES[key];
    const option = document.createElement('option');
    option.value = key;

    const horas = parseInt(key);
    const textoHoras =
      horas === 1
        ? window.TEXTOS_ACTUAL.general.DURACION_HORA_SINGULAR
        : window.TEXTOS_ACTUAL.general.DURACION_HORA_PLURAL;

    option.textContent = `${horas} ${textoHoras} (${opcion.mediciones} ${window.TEXTOS_ACTUAL.general.DURACION_MEDICIONES})`;
    selector.appendChild(option);
  });

  const duracionGuardada = obtenerDuracionSeleccionada();
  selector.value = duracionGuardada;
  maxHistorialActual = DURACION_OPCIONES[duracionGuardada].mediciones;

  const label = document.getElementById('duracion-label');
  if (label && window.TEXTOS_ACTUAL) {
    label.textContent = window.TEXTOS_ACTUAL.general.DURACION_LABEL;
  }

  selector.addEventListener('change', (e) => {
    const nuevaDuracion = e.target.value;
    guardarDuracionSeleccionada(nuevaDuracion);
    maxHistorialActual = DURACION_OPCIONES[nuevaDuracion].mediciones;
    // Borramos el historial porque los datos viejos ya no sirven si cambiaste la duración
    historialStatus = {};
    guardarHistorial();
    monitorearTodosWebsites();
  });
}

// ===============================
// 2. Idiomas y manejo del DOM
// ===============================
//
async function cargarIdioma() {
  const idiomaSolicitado = obtenerIdiomaSeleccionado();
  const idiomaDefault = DEFAULT_LANG;

  try {
    await cargarIdiomaScript(idiomaSolicitado);
    return;
  } catch (errorSolicitado) {
    if (idiomaSolicitado !== idiomaDefault) {
      try {
        await cargarIdiomaScript(idiomaDefault);
        return;
      } catch (errorDefault) {
        throw new Error(
          `Fallo crítico: El idioma solicitado (${idiomaSolicitado}) y el de reserva (${idiomaDefault}) fallaron en la carga.`
        );
      }
    }

    throw new Error(
      `Fallo crítico: No se pudo cargar el idioma por defecto (${idiomaDefault}).`
    );
  }
}
function obtenerIdiomaSeleccionado() {
  const params = new URLSearchParams(window.location.search);
  const langUrl = params.get('lang');

  if (langUrl && I18N_FILES[langUrl]) {
    return langUrl;
  }

  return DEFAULT_LANG;
}

function cargarIdiomaScript(idiomaACargar) {
  const filePath = I18N_FILES[idiomaACargar];

  if (!filePath) {
    return Promise.reject(
      new Error(
        `Error de configuración: Archivo de idioma no definido para ${idiomaACargar}`
      )
    );
  }

  return new Promise((resolve, reject) => {
    const script = document.createElement('script');
    script.src = filePath;
    script.type = 'text/javascript';

    script.onload = () => {
      if (window.TEXTOS_ACTUAL) {
        resolve();
      } else {
        reject(
          new Error(
            `El archivo de idioma ${filePath} no asignó la variable TEXTOS_ACTUAL`
          )
        );
      }
    };

    script.onerror = () => {
      reject(new Error(`Fallo al cargar el script de idioma: ${filePath}`));
    };

    document.head.appendChild(script);
  });
}

function actualizarEncabezadoPromedio(count) {
  const elemento = document.getElementById('header-promedio-ms');
  if (elemento) {
    elemento.textContent = `${window.TEXTOS_ACTUAL.tabla.HEADER_PROMEDIO_MS} [${count}/${maxHistorialActual}]`;
  }
}

function inicializarEtiquetas() {
  const tituloEl = document.getElementById('titulo-principal');
  if (tituloEl) tituloEl.textContent = window.TEXTOS_ACTUAL.general.PAGE_TITLE;

  const infoBar = document.getElementById('info-bar-msg');
  if (infoBar) infoBar.textContent = window.TEXTOS_ACTUAL.general.INFO_BAR;

  const headers = [
    { id: 'header-service', text: window.TEXTOS_ACTUAL.tabla.HEADER_SERVICE },
    { id: 'header-url', text: window.TEXTOS_ACTUAL.tabla.HEADER_URL },
    {
      id: 'header-latency-actual',
      text: window.TEXTOS_ACTUAL.tabla.HEADER_LATENCY_ACTUAL,
    },
    {
      id: 'header-status-actual',
      text: window.TEXTOS_ACTUAL.tabla.HEADER_STATUS_ACTUAL,
    },
    {
      id: 'header-promedio-ms',
      text: window.TEXTOS_ACTUAL.tabla.HEADER_PROMEDIO_MS,
    },
    {
      id: 'header-promedio-status',
      text: window.TEXTOS_ACTUAL.tabla.HEADER_PROMEDIO_STATUS,
    },
    { id: 'header-action', text: window.TEXTOS_ACTUAL.tabla.HEADER_ACTION },
  ];

  headers.forEach((h) => {
    const element = document.getElementById(h.id);
    if (element) element.textContent = h.text;
  });

  const btnReiniciar = document.getElementById('texto-btn-reiniciar');
  if (btnReiniciar)
    btnReiniciar.textContent = window.TEXTOS_ACTUAL.general.BTN_REINICIAR;

  actualizarUltimaActualizacion(null);

  // Mostrar/ocultar enlace ABM según el tema
  actualizarVisibilidadABM();

  // Mostrar/ocultar columna de acción según el tema
  actualizarVisibilidadColumnaAccion();
}

function actualizarUltimaActualizacion(fecha) {
  const elemento = document.getElementById('ultima-actualizacion');
  if (!elemento) return;

  if (fecha) {
    const opciones = {
      hour: '2-digit',
      minute: '2-digit',
      second: '2-digit',
      day: '2-digit',
      month: '2-digit',
      year: 'numeric',
    };
    const fechaFormateada = fecha.toLocaleTimeString('es-ES', opciones);
    elemento.innerHTML = `${window.TEXTOS_ACTUAL.general.LAST_UPDATE} <strong>${fechaFormateada}</strong>`;
  } else {
    elemento.innerHTML = `
      ${window.TEXTOS_ACTUAL.general.LAST_UPDATE} 
      <span class="loading-text">${window.TEXTOS_ACTUAL.general.LOADING}</span><span class="spinner" title="${window.TEXTOS_ACTUAL.general.LOADING}"></span>
    `;
  }
}

function obtenerEstadoVisual(tiempo, estado = 200) {
  const tiempoNum = parseFloat(tiempo);

  if (estado !== 200 || tiempoNum >= UMBRALES_LATENCIA.PENALIZACION_FALLO) {
    const descripcionEstado =
      window.TEXTOS_ACTUAL.httpStatus?.[estado] ||
      window.TEXTOS_ACTUAL.httpStatus?.GENERIC;

    const textoFallo =
      estado !== 200
        ? `${window.TEXTOS_ACTUAL.estados.DOWN_ERROR} (${estado} - ${descripcionEstado})`
        : window.TEXTOS_ACTUAL.estados.DOWN_ERROR;

    return {
      text: textoFallo,
      className: 'status-down',
    };
  }

  const estadosVelocidad = [
    {
      umbral: UMBRALES_LATENCIA.MUY_RAPIDO,
      text: window.TEXTOS_ACTUAL.velocidad.VERY_FAST,
      className: 'status-very-fast',
    },
    {
      umbral: UMBRALES_LATENCIA.RAPIDO,
      text: window.TEXTOS_ACTUAL.velocidad.FAST,
      className: 'status-fast',
    },
    {
      umbral: UMBRALES_LATENCIA.NORMAL,
      text: window.TEXTOS_ACTUAL.velocidad.NORMAL,
      className: 'status-normal',
    },
    {
      umbral: UMBRALES_LATENCIA.LENTO,
      text: window.TEXTOS_ACTUAL.velocidad.SLOW,
      className: 'status-slow',
    },
    {
      umbral: UMBRALES_LATENCIA.CRITICO,
      text: window.TEXTOS_ACTUAL.velocidad.CRITICAL,
      className: 'status-critical',
    },
    {
      umbral: UMBRALES_LATENCIA.RIESGO,
      text: window.TEXTOS_ACTUAL.velocidad.RISK,
      className: 'status-risk',
    },
  ];

  for (const estadoVelocidad of estadosVelocidad) {
    if (tiempoNum <= estadoVelocidad.umbral) {
      return {
        text: estadoVelocidad.text,
        className: estadoVelocidad.className,
      };
    }
  }

  return {
    text: window.TEXTOS_ACTUAL.velocidad.EXTREME_RISK,
    className: 'status-extreme-risk',
  };
}
// Ordena los servicios: primero los críticos (orden=1), después el resto en orden alfabético
function ordenarServiciosPersonalizado(servicios) {
  // Los críticos van primero, el resto se ordena por nombre para que sea fácil de encontrar
  const fijos = servicios.filter((servicio) => servicio.orden === 1);
  const ordenables = servicios.filter((servicio) => servicio.orden !== 1);

  ordenables.sort((a, b) => {
    const nombreA = a.nombre.toUpperCase();
    const nombreB = b.nombre.toUpperCase();
    if (nombreA < nombreB) return -1;
    if (nombreA > nombreB) return 1;
    return 0;
  });

  return fijos.concat(ordenables);
}

function cargarHistorial() {
  const data = sessionStorage.getItem('monitorStatusHistorial');
  if (data) {
    historialStatus = JSON.parse(data);
  }
}

function guardarHistorial() {
  sessionStorage.setItem(
    'monitorStatusHistorial',
    JSON.stringify(historialStatus)
  );
}

function historialCompleto() {
  // Verificar si al menos un servicio alcanzó el máximo de monitoreos
  for (const url in historialStatus) {
    if (
      historialStatus[url] &&
      historialStatus[url].length >= maxHistorialActual
    ) {
      return true;
    }
  }
  return false;
}

function actualizarHistorial(url, time, status) {
  if (!historialStatus[url]) {
    historialStatus[url] = [];
  }

  // No agregar si ya alcanzamos el máximo configurado
  if (historialStatus[url].length >= maxHistorialActual) {
    return;
  }

  historialStatus[url].push({ time, status, timestamp: Date.now() });

  guardarHistorial();
}

// Calcula el promedio de latencia, pero solo cuenta los éxitos (status 200)
// Los fallos (99999ms) no afectan el promedio
function calcularPromedio(url) {
  const historial = historialStatus[url] || [];

  if (historial.length === 0) {
    return {
      promedio: 0,
      estadoPromedio: obtenerEstadoVisual(0, 200),
      validCount: 0,
      historial: historial,
    };
  }

  let totalTime = 0;
  let medicionesExitosas = 0;
  let fallos = 0;
  let ultimoCodigoError = 200;
  const tiemposExitosos = [];

  historial.forEach((entry) => {
    const esFallo =
      entry.status !== 200 ||
      entry.time >= UMBRALES_LATENCIA.PENALIZACION_FALLO;

    if (esFallo) {
      fallos++;
      ultimoCodigoError = entry.status;
    } else {
      // Solo sumar mediciones exitosas para el promedio
      totalTime += entry.time;
      medicionesExitosas++;
      tiemposExitosos.push(entry.time);
    }
  });

  const validCount = historial.length;

  // Si más del 50% son fallos, mostrar como caída total
  if (fallos / validCount > 0.5 && validCount > 3) {
    return {
      promedio: 0,
      estadoPromedio: obtenerEstadoVisual(
        UMBRALES_LATENCIA.PENALIZACION_FALLO + 1,
        ultimoCodigoError
      ),
      validCount: validCount,
      historial: historial,
    };
  }

  // Calcular promedio solo de mediciones exitosas
  const promedioMs =
    medicionesExitosas > 0 ? Math.round(totalTime / medicionesExitosas) : 0;

  // Calcular percentiles (p50, p95) sobre tiempos exitosos
  function percentile(arr, p) {
    if (!arr || arr.length === 0) return 0;
    const sorted = arr.slice().sort((a, b) => a - b);
    const idx = (p / 100) * (sorted.length - 1);
    const lower = Math.floor(idx);
    const upper = Math.ceil(idx);
    if (lower === upper) return Math.round(sorted[lower]);
    const weight = idx - lower;
    return Math.round(sorted[lower] * (1 - weight) + sorted[upper] * weight);
  }

  const p50 = percentile(tiemposExitosos, 50);
  const p95 = percentile(tiemposExitosos, 95);

  const failureRate = validCount > 0 ? Math.round((fallos / validCount) * 100) : 0;

  // Si no hay mediciones exitosas (todas fallaron), mostrar como error
  if (medicionesExitosas === 0) {
    return {
      promedio: 0,
      estadoPromedio: obtenerEstadoVisual(
        UMBRALES_LATENCIA.PENALIZACION_FALLO + 1,
        ultimoCodigoError
      ),
      validCount: validCount,
      historial: historial,
    };
  }

  return {
    promedio: promedioMs,
    estadoPromedio: obtenerEstadoVisual(promedioMs, 200),
    validCount: validCount,
    historial: historial,
    p50: p50,
    p95: p95,
    failureRate: failureRate,
  };
}

function mostrarAdvertenciaGlobal(esFalloCritico, motivoFallo = '') {
  const infoBar = document.getElementById('info-bar-msg');

  if (esFalloCritico) {
    let mensajeBase =
      window.TEXTOS_ACTUAL.general.ADVERTENCIA_FALLO_GLOBAL_HTML;

    if (temaProActivo && motivoFallo) {
      mensajeBase += `<br><small class="motivo-fallo">${window.TEXTOS_ACTUAL.general.MOTIVO_FALLO_PRO} ${motivoFallo}</small>`;
    }

    infoBar.innerHTML = `<strong>🚨 ${mensajeBase}🚨</strong>`;
    infoBar.classList.add('error-critical');
    sessionStorage.setItem('LAST_RUN_CRITICAL', 'true');
  } else {
    infoBar.textContent = window.TEXTOS_ACTUAL.general.INFO_BAR;
    infoBar.classList.remove('error-critical');
    sessionStorage.removeItem('LAST_RUN_CRITICAL');
  }
}

function determinarFalloGlobal(websitesData, resultados) {
  if (resultados.length === 0 || websitesData.length === 0) {
    return {
      esFallo: true,
      motivo: window.TEXTOS_ACTUAL.general.FALLO_CRITICO_RED,
    };
  }

  let totalSitios = websitesData.length;
  let sitiosEnFalloCritico = 0;
  let motivoFallo = '';

  const resultadosMap = resultados.reduce((map, item) => {
    map[item.url] = item;
    return map;
  }, {});

  const sitiosCriticos = websitesData.filter(
    (web) => web.grupo === GRUPO_CRITICO_NOMBRE
  );
  let criticosConFalloExtremo = 0;

  if (sitiosCriticos.length > 0) {
    sitiosCriticos.forEach((web) => {
      const res = resultadosMap[web.url];
      if (res && res.time > UMBRAL_FALLO_GLOBAL_MS) {
        criticosConFalloExtremo++;
      }
    });

    if (
      criticosConFalloExtremo === sitiosCriticos.length &&
      sitiosCriticos.length > 0
    ) {
      console.warn(
        `Alerta Global: Fallo del 100% en el grupo crítico "${GRUPO_CRITICO_NOMBRE}".`
      );
      motivoFallo = `${window.TEXTOS_ACTUAL.general.FALLO_CRITICO_GRUPO} "${GRUPO_CRITICO_NOMBRE}".`;
      return { esFallo: true, motivo: motivoFallo };
    }
  }

  resultados.forEach((res) => {
    if (res.time > UMBRAL_FALLO_GLOBAL_MS) {
      sitiosEnFalloCritico++;
    }
  });

  const porcentajeFallo = sitiosEnFalloCritico / totalSitios;
  const falloPorPorcentaje = porcentajeFallo >= PORCENTAJE_FALLO_GLOBAL;

  if (falloPorPorcentaje) {
    const porcentaje = Math.round(porcentajeFallo * 100);
    console.warn(
      `Alerta Global: ${porcentaje}% de los servicios superaron el umbral de ${UMBRAL_FALLO_GLOBAL_MS}ms.`
    );
    motivoFallo = `${porcentaje}${window.TEXTOS_ACTUAL.general.FALLO_CRITICO_LATENCIA_PARTE1} ${UMBRAL_FALLO_GLOBAL_MS}ms.`;
    return { esFallo: true, motivo: motivoFallo };
  }

  return { esFallo: false, motivo: '' };
}

// =======================================================
// 5. LÓGICA PRINCIPAL DE MONITOREO Y RENDERING ASÍNCRONO
// =======================================================

/**
 * Llama al proxy de Netlify para saber el estado y la latencia de una URL.
 */
async function verificarEstado(url) {
  try {
    const response = await fetch(
      `${PROXY_ENDPOINT}?url=${encodeURIComponent(url)}`
    );

    if (!response.ok) {
      console.error(
        `${window.TEXTOS_ACTUAL.estados.DOWN_ERROR} en la función Serverless para ${url}: ${response.status}`
      );
      // Retornamos un objeto en lugar de lanzar error para que Promise.allSettled
      // trate todos los servicios de forma consistente
      return {
        time: UMBRALES_LATENCIA.PENALIZACION_FALLO,
        status: ESTADO_ERROR_CONEXION,
      };
    }

    const data = await response.json();

    // Normalizar la estructura para mantener compatibilidad con versiones anteriores
    // Preferimos `time`, si no está usamos `totalTime`, luego `ttfb`.
    const rawTime =
      data && (data.time ?? data.totalTime ?? data.ttfb ?? data.total_time);

    const normalizedTime = (() => {
      const n = Number(rawTime);
      if (Number.isFinite(n)) return n;
      // si no es numérico, aplicar penalización
      return UMBRALES_LATENCIA.PENALIZACION_FALLO;
    })();

    const normalizedStatus = Number.isFinite(Number(data?.status))
      ? Number(data.status)
      : ESTADO_ERROR_CONEXION;

    return {
      // Campos esperados por el resto del sistema
      time: normalizedTime,
      status: normalizedStatus,
      // Mantener campos adicionales para debugging si están presentes
      ttfb: data?.ttfb ?? null,
      totalTime: data?.totalTime ?? data?.time ?? null,
      bytesRead: data?.bytesRead ?? null,
      error: data?.error ?? null,
    };
  } catch (error) {
    console.error(
      `${window.TEXTOS_ACTUAL.estados.DOWN_ERROR} al conectar con el proxy para ${url}:`,
      error
    );
    return {
      time: UMBRALES_LATENCIA.PENALIZACION_FALLO,
      status: ESTADO_ERROR_CONEXION,
    };
  }
}

/**
 * Dibuja las filas iniciales con los datos de carga (placeholders).
 */
function dibujarFilasIniciales(servicios) {
  const tbody = document.getElementById('status-table-body');
  tbody.innerHTML = ''; // Limpiar tabla

  // Calcular el conteo máximo del historial para actualizar el encabezado
  let maxValidCount = 0;
  servicios.forEach((web) => {
    const { validCount } = calcularPromedio(web.url);
    maxValidCount = Math.max(maxValidCount, validCount);

    const row = tbody.insertRow();
    // ID que nos permite encontrar la fila para la actualización asíncrona
    row.dataset.url = web.url;

    // Columna 1: Servicio (AHORA CON HIPERVÍNCULO)
    row.insertCell().innerHTML = `<a href="${web.url}" target="_blank">${web.nombre}</a>`;

    // Columna 2: URL (Oculta en styles.css)
    row.insertCell().innerHTML = `<a href="${web.url}" target="_blank">${web.url}</a>`;

    // Columna 3: Latencia Actual (Placeholder)
    row.insertCell().textContent = window.TEXTOS_ACTUAL.general.LOADING;

    // Columna 4: Estado Actual (Placeholder)
    row.insertCell().textContent = window.TEXTOS_ACTUAL.general.LOADING;

    // Columna 5: Promedio (ms) - Placeholder
    row.insertCell().textContent = window.TEXTOS_ACTUAL.general.LOADING;

    // Columna 6: Estado Promedio (Placeholder)
    row.insertCell().textContent = window.TEXTOS_ACTUAL.general.LOADING;

    // Columna 7: Acción (Placeholder)
    row.insertCell().textContent = '';

    // Accesibilidad: añadir aria-label y role a las celdas de estado (placeholders)
    // (Es más robusto añadirlo aquí para que estén presentes antes de la actualización)
    // Aplicar atributos de accesibilidad a las celdas de estado en la fila
    aplicarAccesibilidadEstadoEnFila(row, {
      actual: window.TEXTOS_ACTUAL.general.LOADING,
      promedio: window.TEXTOS_ACTUAL.general.LOADING,
    });
  });

  // Actualizar el encabezado una vez con el historial guardado (puede ser 0/12 si está vacío)
  actualizarEncabezadoPromedio(maxValidCount);
}

/**
 * [Accesibilidad] Agrega roles y etiquetas ARIA a las celdas de estado de una fila.
 * @param {HTMLTableRowElement} row - Fila de la tabla con celdas de estado en las posiciones 3 y 5.
 * @param {Object} labels - Opcional: {actual: string, promedio: string} con textos accesibles.
 */
function aplicarAccesibilidadEstadoEnFila(row, labels = {}) {
  if (!row) return;
  const statusActual = row.cells[3];
  const statusProm = row.cells[5];

  const actualText =
    labels.actual !== undefined
      ? labels.actual
      : statusActual
      ? statusActual.textContent.trim()
      : '';
  const promText =
    labels.promedio !== undefined
      ? labels.promedio
      : statusProm
      ? statusProm.textContent.trim()
      : '';

  if (statusActual) {
    statusActual.setAttribute('role', 'status');
    statusActual.setAttribute(
      'aria-label',
      actualText || window.TEXTOS_ACTUAL.general.LOADING
    );
  }
  if (statusProm) {
    statusProm.setAttribute('role', 'status');
    statusProm.setAttribute(
      'aria-label',
      promText || window.TEXTOS_ACTUAL.general.LOADING
    );
  }
}

/**
 * Devuelve la lista de errores del historial para una URL
 */
function obtenerHistorialErrores(url) {
  const historial = historialStatus[url] || [];
  return historial.filter(
    (entry) =>
      entry.status !== 200 || entry.time >= UMBRALES_LATENCIA.PENALIZACION_FALLO
  );
}

/**
 * Convierte un timestamp a un formato fácil de leer: "14/12 10:45"
 */
function formatearFecha(timestamp) {
  const fecha = new Date(timestamp);
  const dia = String(fecha.getDate()).padStart(2, '0');
  const mes = String(fecha.getMonth() + 1).padStart(2, '0');
  const hora = String(fecha.getHours()).padStart(2, '0');
  const min = String(fecha.getMinutes()).padStart(2, '0');
  return `${dia}/${mes} ${hora}:${min}`;
}

/**
 * Muestra u oculta el detalle de errores en una fila
 */
function toggleErroresDetalle(url) {
  const tbody = document.getElementById('status-table-body');
  const row = tbody.querySelector(`tr[data-url="${CSS.escape(url)}"]`);
  if (!row) return;

  const toggleBtn = row.querySelector('.toggle-errors-button');

  // Buscar si ya existe una fila de detalle para esta URL
  let detalleRow = null;
  let nextRow = row.nextElementSibling;
  if (
    nextRow &&
    nextRow.classList.contains('error-detail-row') &&
    nextRow.getAttribute('data-parent-url') === url
  ) {
    detalleRow = nextRow;
  }

  // Si ya existe la fila de detalle, colapsar
  if (detalleRow) {
    detalleRow.classList.remove('expanded');
    if (toggleBtn) toggleBtn.textContent = '▼';
    setTimeout(() => {
      if (detalleRow && detalleRow.parentNode) {
        detalleRow.remove();
      }
    }, 200); // Esperar animación
    return;
  }

  // Crear nueva fila de detalle
  const errores = obtenerHistorialErrores(url);
  if (errores.length === 0) return;

  // Crear fila usando createElement para mejor control
  const newRow = document.createElement('tr');
  newRow.classList.add('error-detail-row');
  newRow.setAttribute('data-parent-url', url);

  const cell = document.createElement('td');
  cell.colSpan = 7; // Todas las columnas

  const maxErrores = 10;
  const erroresLimitados = errores.slice(-maxErrores);
  const hayMas = errores.length > maxErrores;

  let html = '<div class="error-detail-container">';
  html += `<div class="error-detail-header">⚠️ Errores detectados (${errores.length} de ${historialStatus[url].length} mediciones):</div>`;
  html += '<ul class="error-detail-list">';

  erroresLimitados.reverse().forEach((error) => {
    const fecha = formatearFecha(error.timestamp);
    const codigo = error.status;
    const latencia = error.time;
    const mensaje = codigo === 200 ? 'Timeout' : obtenerMensajeError(codigo);

    html += `<li>`;
    html += `<span class="error-time">${fecha}</span>`;
    html += ` → `;
    html += `<span class="error-code">${codigo}</span> `;
    html += `<span class="error-msg">${mensaje}</span> `;
    html += `<span class="error-latency">(${latencia}ms)</span>`;
    html += `</li>`;
  });

  html += '</ul>';

  if (hayMas) {
    html += `<div class="error-detail-footer">...mostrando últimos ${maxErrores} errores</div>`;
  }

  html += '</div>';

  cell.innerHTML = html;
  newRow.appendChild(cell);

  // Insertar la fila inmediatamente después de la fila padre
  if (row.nextSibling) {
    tbody.insertBefore(newRow, row.nextSibling);
  } else {
    tbody.appendChild(newRow);
  }

  // Cambiar ícono del botón a expandido
  if (toggleBtn) toggleBtn.textContent = '▲';

  // Trigger animación
  setTimeout(() => newRow.classList.add('expanded'), 10);
}

// Hacer función accesible globalmente
window.toggleErroresDetalle = toggleErroresDetalle;

/**
 * Devuelve un mensaje entendible para cada código de error
 */
function obtenerMensajeError(codigo) {
  const mensajes = {
    0: 'Sin conexión',
    301: 'Redireccionamiento',
    302: 'Redireccionamiento',
    400: 'Solicitud incorrecta',
    401: 'No autorizado',
    403: 'Prohibido',
    404: 'No encontrado',
    408: 'Timeout',
    418: 'Tetera',
    429: 'Demasiadas solicitudes',
    500: 'Error servidor',
    502: 'Gateway error',
    503: 'No disponible',
    504: 'Gateway timeout',
  };
  return mensajes[codigo] || `Error ${codigo}`;
}

/**
 * Actualiza una fila de la tabla con los datos reales.
 */
function actualizarFila(web, resultado) {
  const tbody = document.getElementById('status-table-body');
  // Escapar caracteres especiales en la URL para la selección del atributo data-url
  const row = tbody.querySelector(`tr[data-url="${CSS.escape(web.url)}"]`);

  if (!row) return;

  // --- Lógica de cálculo y estado ---
  const estadoActual = obtenerEstadoVisual(resultado.time, resultado.status);
  // Nota: calcularPromedio() obtiene los datos del historial que ACABA de ser actualizado
  // Calcular una sola vez y reutilizar valores para evitar llamadas múltiples
  const stats = calcularPromedio(web.url);
  const { promedio, estadoPromedio, p50, p95, failureRate } = stats;

  // ALERTA: Si hay un error (status 0 o >=400), mostrar alerta solo la primera vez
  if (resultado && (resultado.status === 0 || resultado.status >= 400)) {
    window.registrarErrorSitio &&
      window.registrarErrorSitio(
        web.nombre || web.url,
        web.url,
        resultado.time,
        resultado.status,
        resultado.error || ''
      );
  } else if (resultado && resultado.status === 200) {
    // Si el sitio se recupera, limpiar el registro para permitir futuras alertas
    window.limpiarErrorSitio && window.limpiarErrorSitio(web.nombre || web.url);
  }

  // --- Actualización de celdas (Columnas 3 a 7) ---

  // Columna 3: Latencia Actual (índice 2)
  // Mostrar latencia principal y datos adicionales (ttfb / totalTime) si están presentes
  const mainLatency = Number.isFinite(Number(resultado.time))
    ? `${resultado.time} ms`
    : window.TEXTOS_ACTUAL.general.LOADING;

  const extraParts = [];
  if (resultado.ttfb || resultado.ttfb === 0) {
    extraParts.push(`ttfb: ${resultado.ttfb} ms`);
  }
  if (resultado.totalTime || resultado.totalTime === 0) {
    extraParts.push(`total: ${resultado.totalTime} ms`);
  }

  const sourceBadge = resultado && resultado._source === 'rum'
    ? '<span class="source-badge" style="display:inline-block;background:#2b8;color:#072;padding:2px 6px;border-radius:10px;font-size:0.7em;margin-left:8px;">RUM</span>'
    : '';

  const extrasHtml =
    extraParts.length > 0
      ? `<div class="small-stats" style="font-size:0.8em;color:var(--muted-color,#ccc);">${extraParts.join(' · ')}</div>`
      : '';

  row.cells[2].innerHTML = `<div>${mainLatency}${sourceBadge}</div>${extrasHtml}`;

  // Columna 4: Estado Actual (índice 3)
  row.cells[3].textContent = estadoActual.text;
  row.cells[3].className = estadoActual.className;

  // Obtener tema actual y verificar si permite expansión (todos menos DEF y OSC)
  const params = new URLSearchParams(window.location.search);
  const temaActual = params.get('tema') || TEMA_DEFAULT;
  const permiteExpansion = !TEMAS_BASICOS.includes(temaActual);

  // Hacer clickeable el badge si hay errores y el tema lo permite
  const errores = obtenerHistorialErrores(web.url);
  if (errores.length > 0 && permiteExpansion) {
    row.cells[3].style.cursor = 'pointer';
    row.cells[3].title = 'Click para ver detalles de errores';
    row.cells[3].onclick = () => toggleErroresDetalle(web.url);
  } else {
    row.cells[3].style.cursor = '';
    row.cells[3].title = '';
    row.cells[3].onclick = null;
  }

  // Columna 5: Promedio (ms) (índice 4)
  // Agregar contador de errores si existen y el tema lo permite
  const totalMediciones = (historialStatus[web.url] || []).length;
  const contadorErrores =
    errores.length > 0 && permiteExpansion
      ? ` ⚠️ ${errores.length}/${totalMediciones}`
      : '';

  const statsHtml = `
    <div>${promedio} ms${contadorErrores}</div>
    <div class="small-stats" style="font-size:0.75em; color:var(--muted-color,#ccc);">
      p50: ${p50} ms · p95: ${p95} ms · fallos: ${failureRate}%
    </div>
  `;

  row.cells[4].innerHTML = statsHtml;

  // Columna 6: Estado Promedio (índice 5)
  row.cells[5].textContent = estadoPromedio.text;
  row.cells[5].className = estadoPromedio.className;

  // Hacer clickeable el badge promedio si hay errores y el tema lo permite
  if (errores.length > 0 && permiteExpansion) {
    row.cells[5].style.cursor = 'pointer';
    row.cells[5].title = 'Click para ver detalles de errores';
    row.cells[5].onclick = () => toggleErroresDetalle(web.url);
  } else {
    row.cells[5].style.cursor = '';
    row.cells[5].title = '';
    row.cells[5].onclick = null;
  }

  // Accesibilidad: actualizar atributos de forma consistente después de actualizar el texto
  aplicarAccesibilidadEstadoEnFila(row, {
    actual: estadoActual.text,
    promedio: estadoPromedio.text,
  });

  // Columna 7: Acción (índice 6)
  let actionsHTML = '';

  // Botón PSI (solo en temas PRO/MIN)
  if (permiteExpansion) {
    actionsHTML += `<button class="psi-button" onclick="window.open('https://pagespeed.web.dev/report?url=${web.url}', '_blank')" title="PageSpeed Insights">PSI</button>`;
  }

  row.cells[6].innerHTML = actionsHTML;
}

/**
 * Función principal: pide los datos al proxy, procesa los resultados y actualiza la pantalla.
 */
async function monitorearTodosWebsites() {
  // 0. Limpiar el temporizador anterior
  if (window.monitorTimeout) {
    clearTimeout(window.monitorTimeout);
    window.monitorTimeout = null;
  }

  // 1. Cargar la lista de websites desde data/webs.json y ordenar
  try {
    const response = await fetch('data/webs.json');
    websitesData = await response.json();
  } catch (e) {
    console.error('Error al cargar webs.json.', e);
    actualizarUltimaActualizacion(new Date());
    window.monitorTimeout = setTimeout(
      monitorearTodosWebsites,
      FRECUENCIA_MONITOREO_MS
    );
    return;
  }

  if (websitesData.length === 0) {
    actualizarUltimaActualizacion(new Date());
    window.monitorTimeout = setTimeout(
      monitorearTodosWebsites,
      FRECUENCIA_MONITOREO_MS
    );
    return;
  }

  // Dibuja los placeholders y pone 'Cargando...'
  websitesData = ordenarServiciosPersonalizado(websitesData);
  dibujarFilasIniciales(websitesData);
  actualizarUltimaActualizacion(null);

  // Intentar obtener agregados RUM en memoria (por instancia)
  let rumAggregados = {};
  try {
    const r = await fetch('/.netlify/functions/rum-aggregate');
    if (r.ok) {
      rumAggregados = await r.json();
    }
  } catch (e) {
    // no-op, seguiremos con probes
  }

  // Usamos Promise.allSettled; si hay datos RUM para una URL preferimos esos (fallback a proxy)
  const promesas = websitesData.map((web) => {
    const rum = rumAggregados[web.url];
    if (rum && rum.count > 0) {
      // Construir un resultado compatible: usamos p50 como latencia representativa
      return Promise.resolve({ time: rum.p50, status: 200, _source: 'rum' });
    }
    return verificarEstado(web.url);
  });

  const allResults = await Promise.allSettled(promesas);

  // Convertimos los resultados a un formato simple para analizar si hay fallo global
  const resultadosMonitoreo = [];
  allResults.forEach((result, index) => {
    const web = websitesData[index];
    let res;

    if (result.status === 'fulfilled') {
      res = result.value;
    } else {
      // Penalizamos con PENALIZACION_FALLO para que los errores de red
      // aparezcan claramente como servicios caídos en la UI
      res = {
        time: UMBRALES_LATENCIA.PENALIZACION_FALLO,
        status: ESTADO_ERROR_CONEXION,
      };
    }

    // Agregar al array para el análisis global
    resultadosMonitoreo.push({
      url: web.url,
      time: res.time,
      status: res.status,
    });
  });

  // =======================================================
  // 3. LÓGICA DE FALLO GLOBAL
  // =======================================================
  //const esFalloCritico = determinarFalloGlobal(websitesData, resultadosMonitoreo);
  const { esFallo: esFalloCritico, motivo: motivoFallo } =
    determinarFalloGlobal(websitesData, resultadosMonitoreo);
  mostrarAdvertenciaGlobal(esFalloCritico);

  if (allResults.every((r) => r.status === 'rejected')) {
    mostrarAdvertenciaGlobal(
      true,
      'Fallo total de red: El proxy no respondió para ningún sitio.'
    );
  } else {
    mostrarAdvertenciaGlobal(esFalloCritico, motivoFallo);
  }

  if (esFalloCritico) {
    console.warn(
      'Se detectó un Fallo Global Crítico. Se omite la actualización de la tabla y historial con estos datos. El usuario verá el aviso.'
    );

    // Solo actualizamos el timestamp. La tabla mantiene los datos del historial ANTERIOR
    actualizarUltimaActualizacion(new Date());

    // Programar la próxima ejecución y retornar
    window.monitorTimeout = setTimeout(
      monitorearTodosWebsites,
      FRECUENCIA_MONITOREO_MS
    );
    return;
  }

  // =======================================================
  // 4. SI NO ES CRÍTICO, APLICAR DATOS Y ACTUALIZAR UI NORMALMENTE
  // =======================================================
  let maxValidCount = 0;

  // Recorremos los resultados y actualizamos historial y tabla
  resultadosMonitoreo.forEach((res) => {
    const web = websitesData.find((w) => w.url === res.url);

    // 4.1. Guardar el historial
    actualizarHistorial(res.url, res.time, res.status);

    // 4.2. Actualizar la fila en la pantalla
    actualizarFila(web, res);

    // 4.3. Recalcular el contador para el encabezado
    const { validCount } = calcularPromedio(res.url);
    maxValidCount = Math.max(maxValidCount, validCount);
  });

  // 5. Terminar y programar el próximo monitoreo
  actualizarEncabezadoPromedio(maxValidCount);
  actualizarUltimaActualizacion(new Date());

  // Solo programamos el siguiente monitoreo si todavía no llegamos al máximo
  if (!historialCompleto()) {
    window.monitorTimeout = setTimeout(
      monitorearTodosWebsites,
      FRECUENCIA_MONITOREO_MS
    );
  } else {
    console.log(
      'Historial completo. Monitoreo pausado. Use el botón Reiniciar para continuar.'
    );
  }
}

// ===============================
// 6. Temas y arranque de la app
// ===============================

/**
 * Obtiene el tema de los parámetros de la URL.
 * @returns {string | null} El nombre del tema o null.
 */
function obtenerTemaDeURL() {
  const params = new URLSearchParams(window.location.search);
  const tema = params.get('tema');
  if (tema === TEMA_DEFAULT) return TEMA_DEFAULT;
  if (tema === TEMA_PRO) return TEMA_PRO;
  if (tema === TEMA_PRO2) return TEMA_PRO2;
  if (tema === TEMA_MIN) return TEMA_MIN;
  if (tema === TEMA_OSC) return TEMA_OSC;
  return null;
}

/**
 * Lógica de cambio de tema: Prioriza la URL. Si no hay parámetro,
 * usa TEMA_DEFAULT.
 */
function inicializarTema() {
  const estiloPrincipal = document.getElementById('estilo-principal');
  let temaFinal = TEMA_DEFAULT;

  // 1. Intentar obtener el tema de la URL (MÁXIMA PRIORIDAD)
  const temaUrl = obtenerTemaDeURL();

  if (temaUrl) {
    temaFinal = temaUrl;
  }

  // 2. Aplicar el tema
  if (TEMA_FILES[temaFinal]) {
    estiloPrincipal.href = TEMA_FILES[temaFinal];
    temaProActivo = temaFinal !== TEMA_DEFAULT;
  } else {
    // Fallback de seguridad
    estiloPrincipal.href = TEMA_FILES[TEMA_DEFAULT];
    temaProActivo = false;
  }

  // 3. Actualizar el botón toggle
  actualizarBotonToggle(temaFinal);
}

/**
 * Actualiza el icono del botón toggle según el tema actual
 * Oculta el botón si el tema no tiene pareja de alternancia
 */
function actualizarBotonToggle(temaActual) {
  const themeIcon = document.getElementById('theme-icon');
  const themeBtn = document.getElementById('theme-toggle-btn');

  if (!themeBtn) return;
  // Normalizar temaActual: aceptar 'theme-xxx', rutas CSS o claves
  let tema = temaActual || '';
  if (typeof tema === 'string' && tema.startsWith('theme-')) {
    tema = tema.replace('theme-', '');
  }
  // Si nos pasaron una ruta CSS, buscar la clave correspondiente
  if (
    typeof tema === 'string' &&
    (tema.indexOf('/') !== -1 || tema.indexOf('.css') !== -1)
  ) {
    for (const k in TEMA_FILES) {
      if (
        TEMA_FILES[k] &&
        tema.indexOf(TEMA_FILES[k].split('/').pop()) !== -1
      ) {
        tema = k;
        break;
      }
    }
  }

  // Verificar si el tema actual tiene pareja de alternancia
  const tieneParejaToggle =
    typeof TEMA_TOGGLE_PAIRS !== 'undefined' &&
    TEMA_TOGGLE_PAIRS.hasOwnProperty(tema);

  if (!tieneParejaToggle) {
    themeBtn.style.display = 'none';
    return;
  }

  themeBtn.style.display = 'block';
  if (!themeIcon) return;

  // Determinar tema destino (acción) y mostrar ícono según la acción
  const temaDestino = TEMA_TOGGLE_PAIRS[tema];
  if (!temaDestino) {
    themeIcon.textContent = '🔄';
    themeBtn.setAttribute('title', 'Alternar tema');
    return;
  }

  // Si el tema destino es oscuro, mostrar luna (acción: pasar a oscuro)
  const destinosOscuros = [TEMA_OSC, TEMA_PRO];
  if (destinosOscuros.includes(temaDestino)) {
    themeIcon.textContent = '🌙';
    themeBtn.setAttribute(
      'title',
      `Cambiar a modo oscuro (${temaDestino.toUpperCase()})`
    );
  } else {
    // Tema destino claro -> mostrar sol
    themeIcon.textContent = '☀️';
    themeBtn.setAttribute(
      'title',
      `Cambiar a modo claro (${temaDestino.toUpperCase()})`
    );
  }
}

/**
 * Alterna entre temas configurados en TEMA_TOGGLE_PAIRS
 */
function toggleDarkMode() {
  const estiloPrincipal = document.getElementById('estilo-principal');
  const params = new URLSearchParams(window.location.search);
  const temaUrl = params.get('tema');

  // Determinar tema actual: priorizar URL, luego tomar el default
  let temaActual = TEMA_DEFAULT;
  if (temaUrl && TEMA_FILES[temaUrl]) {
    temaActual = temaUrl;
  }

  // Obtener la pareja del tema actual
  const nuevoTema = TEMA_TOGGLE_PAIRS[temaActual];

  // Si no hay pareja configurada, no hacer nada
  if (!nuevoTema) return;

  // Aplicar el nuevo tema
  if (TEMA_FILES[nuevoTema]) {
    estiloPrincipal.href = TEMA_FILES[nuevoTema];
    temaProActivo = nuevoTema !== TEMA_DEFAULT;

    // Actualizar la URL con el nuevo tema
    params.set('tema', nuevoTema);
    const nuevaUrl = `${window.location.pathname}?${params.toString()}`;
    window.history.replaceState({}, '', nuevaUrl);
    actualizarVisibilidadABM();

    // Actualizar visibilidad de columna acción
    actualizarVisibilidadColumnaAccion();

    // Actualizar enlace de leyenda con nuevo tema
    configurarEnlaceLeyenda();
    // Actualizar el icono del botón toggle para reflejar la nueva acción
    actualizarBotonToggle(nuevoTema);
  }
}

/**
 * Muestra u oculta el enlace ABM según el tema activo
 */
function actualizarVisibilidadABM() {
  const enlaceABM = document.getElementById('enlace-abm');
  if (!enlaceABM) return;

  const params = new URLSearchParams(window.location.search);
  const temaActual = params.get('tema') || TEMA_DEFAULT;

  // Ocultar solo en temas básicos: def y osc
  if (temaActual === TEMA_DEFAULT || temaActual === TEMA_OSC) {
    enlaceABM.style.display = 'none';
  } else {
    enlaceABM.style.display = 'inline-flex';
  }
}

/**
 * Muestra u oculta la columna de acción según el tema activo
 */
function actualizarVisibilidadColumnaAccion() {
  const params = new URLSearchParams(window.location.search);
  const temaUrl = params.get('tema');

  let temaActual = TEMA_DEFAULT;
  if (temaUrl && TEMA_FILES[temaUrl]) {
    temaActual = temaUrl;
  }

  const headerAccion = document.getElementById('header-action');
  const tabla = document.getElementById('monitor-table');

  // Ocultar columna en temas básicos: def y osc
  if (temaActual === TEMA_DEFAULT || temaActual === TEMA_OSC) {
    // Ocultar header
    if (headerAccion) {
      headerAccion.style.display = 'none';
    }

    // Ocultar todas las celdas de acción (7ma columna)
    if (tabla) {
      const rows = tabla.querySelectorAll('tr');
      rows.forEach((row) => {
        const cells = row.children;
        if (cells.length >= 7) {
          cells[6].style.display = 'none';
        }
      });
    }
  } else {
    // Mostrar columna en temas avanzados
    if (headerAccion) {
      headerAccion.style.display = '';
    }

    if (tabla) {
      const rows = tabla.querySelectorAll('tr');
      rows.forEach((row) => {
        const cells = row.children;
        if (cells.length >= 7) {
          cells[6].style.display = '';
        }
      });
    }
  }
}

function reiniciarMonitoreo() {
  // Limpiar historial
  historialStatus = {};
  guardarHistorial();

  // Cancelar timeout pendiente si existe
  if (window.monitorTimeout) {
    clearTimeout(window.monitorTimeout);
  }

  const tbody = document.getElementById('status-table-body');
  if (tbody) {
    tbody.innerHTML = '';
  }

  // Reiniciar monitoreo
  monitorearTodosWebsites();
}

async function cargarYMostrarHistorialExistente() {
  // Cargar lista de websites
  let websitesData = [];
  try {
    const response = await fetch(WEBSITES_FILE);
    websitesData = await response.json();
  } catch (e) {
    console.error('Error al cargar data/webs.json.', e);
    return;
  }

  if (websitesData.length === 0) return;

  websitesData = ordenarServiciosPersonalizado(websitesData);

  // Dibujar filas con datos del historial existente
  const tbody = document.getElementById('status-table-body');
  tbody.innerHTML = '';

  let maxValidCount = 0;

  websitesData.forEach((web) => {
    const row = tbody.insertRow();
    row.setAttribute('data-url', web.url);

    const cellNombre = row.insertCell();
    cellNombre.textContent = web.nombre;

    const cellUrl = row.insertCell();
    const a = document.createElement('a');
    a.href = web.url;
    a.target = '_blank';
    a.rel = 'noopener noreferrer';
    a.textContent = web.url;
    cellUrl.appendChild(a);

    // Obtener última medición del historial
    const historial = historialStatus[web.url] || [];
    const ultimaMedicion =
      historial.length > 0 ? historial[historial.length - 1] : null;

    // Obtener tema actual y verificar si permite expansión (todos menos DEF y OSC)
    const params = new URLSearchParams(window.location.search);
    const temaActual = params.get('tema') || TEMA_DEFAULT;
    const permiteExpansion = !TEMAS_BASICOS.includes(temaActual);
    const errores = obtenerHistorialErrores(web.url);

    if (ultimaMedicion) {
      const estadoActual = obtenerEstadoVisual(
        ultimaMedicion.time,
        ultimaMedicion.status
      );
      const { promedio, estadoPromedio, validCount, p50, p95 } = calcularPromedio(
        web.url
      );

      maxValidCount = Math.max(maxValidCount, validCount);

      row.insertCell().textContent = `${ultimaMedicion.time} ms`;

      const cellEstadoActual = row.insertCell();
      cellEstadoActual.textContent = estadoActual.text;
      cellEstadoActual.className = estadoActual.className;

      // Hacer clickeable el badge si hay errores y el tema lo permite
      if (errores.length > 0 && permiteExpansion) {
        cellEstadoActual.style.cursor = 'pointer';
        cellEstadoActual.title = 'Click para ver detalles de errores';
        cellEstadoActual.onclick = () => toggleErroresDetalle(web.url);
      }

      // Agregar contador de errores si existen y el tema lo permite
      const totalMediciones = historial.length;

      const contadorErrores =
        errores.length > 0 && permiteExpansion
          ? ` ⚠️ ${errores.length}/${totalMediciones}`
          : '';

      const statsHtml = `
        <div>${promedio} ms${contadorErrores}</div>
        <div class="small-stats" style="font-size:0.75em; color:var(--muted-color,#ccc);">
          p50: ${p50} ms · p95: ${p95} ms · fallos: ${Math.round((historial.filter(e => e.status !== 200 || e.time >= UMBRALES_LATENCIA.PENALIZACION_FALLO).length / (historial.length||1)) * 100)}%
        </div>
      `;
      row.insertCell().innerHTML = statsHtml;

      const cellEstadoPromedio = row.insertCell();
      cellEstadoPromedio.textContent = estadoPromedio.text;
      cellEstadoPromedio.className = estadoPromedio.className;

      // Hacer clickeable el badge promedio si hay errores y el tema lo permite
      if (errores.length > 0 && permiteExpansion) {
        cellEstadoPromedio.style.cursor = 'pointer';
        cellEstadoPromedio.title = 'Click para ver detalles de errores';
        cellEstadoPromedio.onclick = () => toggleErroresDetalle(web.url);
      }
    } else {
      row.insertCell().textContent = '-';
      row.insertCell().textContent = '-';
      row.insertCell().textContent = '-';
      row.insertCell().textContent = '-';
    }

    const cellAccion = row.insertCell();
    let actionsHTML = '';

    // Botón PSI (solo en temas PRO/MIN)
    if (permiteExpansion) {
      actionsHTML += `<button class="psi-button" onclick="window.open('https://pagespeed.web.dev/report?url=${web.url}', '_blank')" title="PageSpeed Insights">PSI</button>`;
    }

    cellAccion.innerHTML = actionsHTML;
  });

  actualizarEncabezadoPromedio(maxValidCount);

  // NO actualizar la fecha de última actualización - mantener la guardada
  // Buscar la última fecha en el historial
  let ultimaFecha = null;
  for (const url in historialStatus) {
    const historial = historialStatus[url];
    if (historial && historial.length > 0) {
      const ultimaMedicion = historial[historial.length - 1];
      if (ultimaMedicion.timestamp) {
        if (!ultimaFecha || ultimaMedicion.timestamp > ultimaFecha) {
          ultimaFecha = ultimaMedicion.timestamp;
        }
      }
    }
  }

  if (ultimaFecha) {
    actualizarUltimaActualizacion(new Date(ultimaFecha));
  }
}

// Cuando se carga la página, arranca todo el sistema
document.addEventListener('DOMContentLoaded', async () => {
  inicializarTema();
  cargarHistorial();
  configurarEnlaceLeyenda();

  try {
    // 1. Cargar dinámicamente el diccionario de idioma
    await cargarIdioma();

    // 2. Inicializar elementos estáticos AHORA que TEXTOS_ACTUAL tiene valor
    inicializarEtiquetas();
    inicializarSelectorDuracion();

    // 3. Verificar si el historial ya está completo
    if (historialCompleto()) {
      // Si está completo, solo cargar y mostrar datos existentes
      console.log(
        'Historial completo detectado. Mostrando datos guardados sin nuevas mediciones.'
      );
      await cargarYMostrarHistorialExistente();
    } else {
      // Si no está completo, iniciar el monitoreo normal
      monitorearTodosWebsites();
    }
  } catch (e) {
    console.error('Fallo crítico: No se pudo cargar el idioma.', e);
    document.getElementById(
      'info-bar-msg'
    ).textContent = `ERROR: No se pudo cargar el idioma. Verifique la consola.`;
  }
});

```

### Archivo: `lang/i18n_en.js`
```javascript
// Translation file - English
// This file only contains English texts
// Should not contain logic, only data
const TEXTOS_EN = {
  general: {
    PAGE_TITLE: 'Service Status Monitor',
    LAST_UPDATE: 'Last Update:',
    LOADING: 'Loading...',
    INFO_BAR:
      'Data is automatically updated every 5 minutes using a Serverless Proxy.',
    ADVERTENCIA_FALLO_GLOBAL_HTML:
      'Monitoring data unavailable/unreliable. Widespread critical latency detected, possibly due to a monitoring system overload. Please wait for the next cycle or refresh the page.',
    MOTIVO_FALLO_PRO: 'Pro Reason:',
    FALLO_CRITICO_GRUPO: '100% of the critical group failed:',
    FALLO_CRITICO_LATENCIA_PARTE1: '% of services exceeded the threshold of',
    FALLO_CRITICO_RED: 'No results available (Total network failure)',
    DURACION_LABEL: 'History duration:',
    DURACION_HORA_SINGULAR: 'hour',
    DURACION_HORA_PLURAL: 'hours',
    DURACION_MEDICIONES: 'measurements',
    BTN_REINICIAR: 'Restart Monitoring',
    BTN_COPIAR: 'Copy',
    BTN_COPIADO: 'Copied!',
    BTN_CERRAR: 'Close',
  },

  velocidad: {
    VERY_FAST: 'VERY FAST',
    FAST: 'FAST',
    NORMAL: 'NORMAL',
    SLOW: 'SLOW',
    CRITICAL: 'CRITICAL',
    RISK: 'RISK',
    EXTREME_RISK: 'EXTREME RISK',
  },

  estados: {
    DOWN: 'DOWN',
    DOWN_ERROR: 'DOWN/ERROR',
  },
  httpCodes: [
    {
      code: 0,
      label: 'No connection',
      description:
        'Network failure, DNS, timeout, CORS blocking, or server did not respond.',
    },
    {
      code: 301,
      label: 'Moved permanently',
      description: 'The resource has been permanently moved to a new URL.',
    },
    {
      code: 302,
      label: 'Moved temporarily',
      description: 'The resource is temporarily located at a different URL.',
    },
    {
      code: 304,
      label: 'Not modified',
      description: 'The resource has not changed since the last request.',
    },
    {
      code: 400,
      label: 'Bad request',
      description: 'Malformed or invalid request.',
    },
    {
      code: 401,
      label: 'Unauthorized',
      description: 'Authentication required to access the resource.',
    },
    {
      code: 402,
      label: 'Payment required',
      description: 'Reserved for future use in payment systems.',
    },
    {
      code: 403,
      label: 'Forbidden',
      description: 'Access denied, even with valid authentication.',
    },
    {
      code: 404,
      label: 'Not found',
      description: 'The requested resource does not exist on the server.',
    },
    {
      code: 405,
      label: 'Method not allowed',
      description: 'The HTTP method used is not allowed for this resource.',
    },
    {
      code: 406,
      label: 'Not acceptable',
      description:
        'The server cannot generate an acceptable response according to Accept headers.',
    },
    {
      code: 408,
      label: 'Request timeout',
      description: 'The server timed out waiting for the request.',
    },
    {
      code: 409,
      label: 'Conflict',
      description:
        'The request conflicts with the current state of the server.',
    },
    {
      code: 410,
      label: 'Gone',
      description:
        'The resource was permanently deleted and has no forwarding address.',
    },
    {
      code: 418,
      label: "I'm a teapot",
      description:
        'Humorous code (RFC 2324). Some services use it to reject requests.',
    },
    {
      code: 429,
      label: 'Too many requests',
      description: 'The service rate limit was exceeded.',
    },
    {
      code: 500,
      label: 'Internal server error',
      description: 'Generic internal server error.',
    },
    {
      code: 501,
      label: 'Not implemented',
      description:
        'The server does not support the functionality required to complete the request.',
    },
    {
      code: 502,
      label: 'Bad gateway',
      description:
        'A proxy/gateway received an invalid response from the origin server.',
    },
    {
      code: 503,
      label: 'Service unavailable',
      description:
        'The server is temporarily overloaded, under maintenance, or offline.',
    },
    {
      code: 504,
      label: 'Gateway timeout',
      description:
        'A proxy/gateway did not receive a timely response from the origin server.',
    },
  ],
};

// Construir el objeto httpStatus automáticamente desde el array de códigos
// Esto evita duplicar la información y mantiene todo sincronizado
TEXTOS_EN.httpStatus = {};
TEXTOS_EN.httpCodes.forEach((item) => {
  TEXTOS_EN.httpStatus[item.code] = item.label;
});
TEXTOS_EN.httpStatus.GENERIC = 'HTTP Error';

TEXTOS_EN.tabla = {
  HEADER_SERVICE: 'Service',
  HEADER_URL: 'URL',
  HEADER_LATENCY_ACTUAL: 'Current Latency',
  HEADER_STATUS_ACTUAL: 'Current Status',
  HEADER_PROMEDIO_MS: 'Average',
  HEADER_PROMEDIO_STATUS: 'Average Status',
  HEADER_ACTION: 'Action',
};

TEXTOS_EN.leyenda = {
  title_browser: 'Status Monitor Legend',
  main_header: 'Latency Thresholds and Operational Justification',
  link_volver: 'Back to Application',
  intro:
    'Colors and symbols reflect the measured response time (latency). The justification is based on Interaction Psychology and the Operational Meaning of performance.',
  umbrales: [
    {
      key: 'very_fast',
      className: 'status-very-fast',
      emoji: '🚀',
      label: 'VERY FAST',
      range_text: '< 300 ms',
      summary: 'Optimal Performance (Instantaneous to the User)',
      details:
        "Golden Standard. The human brain perceives responses under 100 ms as instant (Nielsen's guideline). Keeping the threshold up to 300 ms ensures a smooth experience. Operational Meaning: The system is operating in optimal conditions.",
    },
    {
      key: 'fast',
      className: 'status-fast',
      emoji: '⭐',
      label: 'FAST',
      range_text: '300 ms ≤ Latency < 500 ms',
      summary: 'Fluid Interaction without Noticeable Wait',
      details:
        'Limit of Unconscious Perception. The delay is noticeable but not perceived as a bothersome wait. Operational Meaning: Excellent performance; good control point for fast backend processes.',
    },
    {
      key: 'normal',
      className: 'status-normal',
      emoji: '✅',
      label: 'NORMAL',
      range_text: '500 ms ≤ Latency < 800 ms',
      summary: 'Acceptable Performance (User Maintains Focus)',
      details:
        'Begin of Distraction. From 500 ms the user may start to divert attention, although they can maintain their thought flow. Operational Meaning: Acceptable performance but approaching where wait sensation consolidates.',
    },
    {
      key: 'slow',
      className: 'status-slow',
      emoji: '⚠️',
      label: 'SLOW',
      range_text: '800 ms ≤ Latency < 1500 ms',
      summary: 'Annoying Delay (Active Distractor / Early Alert)',
      details:
        '1 second limit. The delay becomes an active distractor. Experience is noticeably degraded. Operational Meaning: Early Alert. Server or network under stress; investigate.',
    },
    {
      key: 'critical',
      className: 'status-critical',
      emoji: '🐌',
      label: 'CRITICAL',
      range_text: '1500 ms ≤ Latency < 3000 ms',
      summary: 'Abandonment Risk (3 Seconds / Imminent Failure)',
      details:
        'Loss of Focus and Frustration. The critical boundary (3 seconds) where users may abandon pages. Operational Meaning: Imminent Failure. Heavy load or severe bottlenecks.',
    },
    {
      key: 'risk',
      className: 'status-risk',
      emoji: '🚨',
      label: 'RISK',
      range_text: '3000 ms ≤ Latency < 5000 ms',
      summary: 'Functional Failure and Collapse (5 Seconds / Alarm)',
      details:
        'Functional Failure. Delays longer than 5 seconds are often considered a functional failure. Operational Meaning: ALARM. Service near collapse or unreliable request handling.',
    },
    {
      key: 'extreme_risk',
      className: 'status-extreme-risk',
      emoji: '🔥',
      label: 'EXTREME RISK',
      range_text: '5000 ms ≤ Latency < 99999 ms',
      summary: 'Unacceptable Latency (Chaos / Guaranteed Abandonment)',
      details:
        'Chaos/Limbo. Range before maximum timeout. The user likely abandoned the action. Operational Meaning: Server cannot process requests in reasonable time. Immediate attention required.',
    },
    {
      key: 'down',
      className: 'status-down',
      emoji: '❌',
      label: 'DOWN',
      range_text: '≥ 99999 ms',
      summary: 'Confirmed Outage (Timeout Exceeded)',
      details:
        'Confirmed Outage. The PENALIZACION_FALLO value was exceeded. Operational Meaning: Service is down, route is unreachable, or server refused to respond.',
    },
  ],
  http_codes_title: 'HTTP Status Codes and System Failures',
  http_codes_description:
    'When a service returns a status code outside the 2xx range (Success), the monitor visually classifies it as ❌ DOWN, but it displays the real status code (e.g., ❌ Down (404)).',
};

TEXTOS_EN.leyenda.codigos_error = [
  {
    code: '2xx',
    label: 'OK / Success',
    description:
      'The connection and service responded correctly (latency measured).',
  },
];

TEXTOS_EN.httpCodes
  .filter((item) => item.code === 0 || item.code >= 400)
  .forEach((item) => {
    TEXTOS_EN.leyenda.codigos_error.push({
      code: item.code.toString(),
      label: item.label,
      description: item.description,
    });
  });

// Solo asignar los textos a la variable global
// La lógica de carga está en i18n.js
window.TEXTOS_ACTUAL = TEXTOS_EN;

```

### Archivo: `lang/i18n_es.js`
```javascript
// Archivo de traducción - Español
// Este archivo solo contiene los textos en español
// No debe contener lógica, solo datos
const TEXTOS_ES = {
  general: {
    PAGE_TITLE: 'Monitor de Estado de Servicios',
    LAST_UPDATE: 'Última actualización:',
    LOADING: 'Cargando...',
    INFO_BAR:
      'Los datos se actualizan automáticamente cada 5 minutos usando un Proxy Serverless.',
    ADVERTENCIA_FALLO_GLOBAL_HTML:
      'Datos de monitoreo no disponibles/no confiables. Se detectó una latencia crítica generalizada, posiblemente debido a una sobrecarga del sistema de monitoreo. Por favor, espere el próximo ciclo o actualice la página.',
    MOTIVO_FALLO_PRO: 'Motivo Pro:',
    FALLO_CRITICO_GRUPO: 'Falló el 100% del grupo crítico:',
    FALLO_CRITICO_LATENCIA_PARTE1: '% de los servicios superaron el umbral de',
    FALLO_CRITICO_RED: 'No hay resultados disponibles (Fallo de red total)',
    DURACION_LABEL: 'Duración del historial:',
    DURACION_HORA_SINGULAR: 'hora',
    DURACION_HORA_PLURAL: 'horas',
    DURACION_MEDICIONES: 'mediciones',
    BTN_REINICIAR: 'Reiniciar Monitoreo',
    BTN_COPIAR: 'Copiar',
    BTN_COPIADO: '¡Copiado!',
    BTN_CERRAR: 'Cerrar',
  },
  velocidad: {
    VERY_FAST: 'MUY RÁPIDO',
    FAST: 'RÁPIDO',
    NORMAL: 'NORMAL',
    SLOW: 'LENTO',
    CRITICAL: 'CRÍTICO',
    RISK: 'RIESGO',
    EXTREME_RISK: 'RIESGO EXTREMO',
  },
  estados: {
    DOWN: 'CAÍDA',
    DOWN_ERROR: 'CAÍDA/ERROR',
  },
  httpCodes: [
    {
      code: 0,
      label: 'Sin conexión',
      description:
        'No se pudo establecer conexión con el servidor (timeout, DNS, red).',
    },
    {
      code: 301,
      label: 'Redireccionamiento permanente',
      description: 'El recurso se ha movido permanentemente a una nueva URL.',
    },
    {
      code: 302,
      label: 'Redireccionamiento temporal',
      description: 'El recurso está temporalmente en una URL diferente.',
    },
    {
      code: 304,
      label: 'No modificado',
      description: 'El recurso no ha cambiado desde la última solicitud.',
    },
    {
      code: 400,
      label: 'Solicitud incorrecta',
      description: 'Solicitud mal formada o inválida.',
    },
    {
      code: 401,
      label: 'No autorizado',
      description: 'Se requiere autenticación para acceder al recurso.',
    },
    {
      code: 402,
      label: 'Pago requerido',
      description: 'Reservado para uso futuro en sistemas de pago.',
    },
    {
      code: 403,
      label: 'Acceso prohibido',
      description: 'Acceso prohibido, incluso con autenticación válida.',
    },
    {
      code: 404,
      label: 'No encontrado',
      description: 'El recurso solicitado no existe en el servidor.',
    },
    {
      code: 405,
      label: 'Método no permitido',
      description: 'El método HTTP usado no está permitido para este recurso.',
    },
    {
      code: 406,
      label: 'No aceptable',
      description:
        'El servidor no puede generar una respuesta aceptable según los headers Accept.',
    },
    {
      code: 408,
      label: 'Tiempo agotado',
      description: 'El servidor agotó el tiempo de espera para la solicitud.',
    },
    {
      code: 409,
      label: 'Conflicto',
      description:
        'La solicitud entra en conflicto con el estado actual del servidor.',
    },
    {
      code: 410,
      label: 'Ya no disponible',
      description:
        'El recurso fue eliminado permanentemente y no tiene dirección de reenvío.',
    },
    {
      code: 418,
      label: 'Soy una tetera',
      description:
        'Código humorístico (RFC 2324). Algunos servicios lo usan para rechazar solicitudes.',
    },
    {
      code: 429,
      label: 'Demasiadas solicitudes',
      description:
        'Se ha superado el límite de tasa (Rate Limit) del servicio.',
    },
    {
      code: 500,
      label: 'Error del servidor',
      description: 'Error interno genérico del servidor.',
    },
    {
      code: 501,
      label: 'No implementado',
      description:
        'El servidor no soporta la funcionalidad requerida para completar la solicitud.',
    },
    {
      code: 502,
      label: 'Puerta de enlace incorrecta',
      description:
        'Un proxy/gateway recibió una respuesta inválida del servidor de origen.',
    },
    {
      code: 503,
      label: 'Servicio no disponible',
      description:
        'El servidor está temporalmente sobrecargado, en mantenimiento o inactivo.',
    },
    {
      code: 504,
      label: 'Tiempo agotado de puerta de enlace',
      description:
        'Un proxy/gateway no recibió respuesta a tiempo del servidor de origen.',
    },
  ],
};

// Construir el objeto httpStatus automáticamente desde el array de códigos
// Esto evita duplicar la información y mantiene todo sincronizado
TEXTOS_ES.httpStatus = {};
TEXTOS_ES.httpCodes.forEach((item) => {
  TEXTOS_ES.httpStatus[item.code] = item.label;
});
TEXTOS_ES.httpStatus.GENERIC = 'Error HTTP';

TEXTOS_ES.tabla = {
  HEADER_SERVICE: 'Servicio',
  HEADER_URL: 'URL',
  HEADER_LATENCY_ACTUAL: 'Latencia Actual',
  HEADER_STATUS_ACTUAL: 'Estado Actual',
  HEADER_PROMEDIO_MS: 'Promedio ',
  HEADER_PROMEDIO_STATUS: 'Estado Promedio',
  HEADER_ACTION: 'Acción',
};

TEXTOS_ES.leyenda = {
  title_browser: 'Leyenda del Monitor de Estado',
  main_header: 'Umbrales de Latencia y Justificación Operacional',
  link_volver: 'Volver a la Aplicación',
  intro:
    'Los colores y símbolos reflejan el tiempo de respuesta (latencia) medido. La justificación se basa en la Psicología de la Interacción y el Significado Operacional del rendimiento.',
  umbrales: [
    {
      key: 'very_fast',
      className: 'status-very-fast',
      emoji: '🚀',
      label: 'MUY RÁPIDO',
      range_text: '< 300 ms',
      summary: 'Rendimiento Óptimo (Instantáneo para el Usuario)',
      details:
        'Estándar Dorado. El cerebro humano percibe cualquier respuesta por debajo de los 100 ms como instantánea (Regla de Nielsen). Mantener el umbral hasta 300 ms asegura una experiencia fluida. Significado Operacional: El sistema está operando en condiciones óptimas y con alta eficiencia.',
    },
    {
      key: 'fast',
      className: 'status-fast',
      emoji: '⭐',
      label: 'RÁPIDO',
      range_text: '300 ms ≤ Latencia < 500 ms',
      summary: 'Interacción Fluida sin Molestias (Percepción Inconsciente)',
      details:
        'Límite de la Percepción Inconsciente. La demora es notable pero el usuario no la percibe como una espera molesta. Significado Operacional: Rendimiento excelente, buen punto de control para procesos rápidos de backend.',
    },
    {
      key: 'normal',
      className: 'status-normal',
      emoji: '✅',
      label: 'NORMAL',
      range_text: '500 ms ≤ Latencia < 800 ms',
      summary: 'Rendimiento Aceptable (El Foco se Mantiene)',
      details:
        'La Distracción Comienza. A partir de 500 ms el usuario puede comenzar a desviarse, aunque puede mantener su hilo de pensamiento. Significado Operacional: Rendimiento aceptable, pero acercándose a donde la sensación de espera se consolida.',
    },
    {
      key: 'slow',
      className: 'status-slow',
      emoji: '⚠️',
      label: 'LENTO',
      range_text: '800 ms ≤ Latencia < 1500 ms',
      summary: 'Demora Molesta (Distractor Activo / Alerta Temprana)',
      details:
        'Límite del 1 Segundo. La demora se convierte en un distractor activo. La experiencia está notablemente degradada. Significado Operacional: Alerta Temprana. El servidor o la red experimentan estrés. Momento de investigar.',
    },
    {
      key: 'critical',
      className: 'status-critical',
      emoji: '🐌',
      label: 'CRÍTICO',
      range_text: '1500 ms ≤ Latencia < 3000 ms',
      summary: 'Riesgo de Abandono del Usuario (3 Segundos / Fallo Inminente)',
      details:
        'Pérdida de Foco y Frustración. El límite crítico (3 segundos) donde los usuarios abandonan una página web. Significado Operacional: Fallo Inminente. Indica carga extremadamente pesada o cuellos de botella severos.',
    },
    {
      key: 'risk',
      className: 'status-risk',
      emoji: '🚨',
      label: 'RIESGO',
      range_text: '3000 ms ≤ Latencia < 5000 ms',
      summary: 'Fallo Funcional y Colapso (5 Segundos / Alarma)',
      details:
        'Fallo Funcional. Las demoras superiores a 5 segundos son consideradas un fallo funcional en muchos sistemas. Significado Operacional: ALARMA. El servicio está al borde del colapso o no sirve peticiones de manera confiable.',
    },
    {
      key: 'extreme_risk',
      className: 'status-extreme-risk',
      emoji: '🔥',
      label: 'RIESGO EXTREMO',
      range_text: '5000 ms ≤ Latencia < 99999 ms',
      summary: 'Latencia Inaceptable (CAOS / Abandono Asegurado)',
      details:
        'CAOS/Limbo. Rango antes del timeout máximo. Es casi seguro que el usuario abandonó la acción. Significado Operacional: El servidor no puede procesar la solicitud en un tiempo razonable. Requiere atención INMEDIATA.',
    },
    {
      key: 'down',
      className: 'status-down',
      emoji: '❌',
      label: 'FALLO TOTAL',
      range_text: '≥ 99999 ms',
      summary: 'Caída Confirmada (Timeout Excedido)',
      details:
        'Caída Confirmada. El valor de PENALIZACION_FALLO ha sido superado. Significado Operacional: El servicio está caído, la ruta es inaccesible, o el servidor se negó a responder.',
    },
  ],
  http_codes_title: 'Códigos de Estado HTTP y Fallos del Sistema',
  http_codes_description:
    'Cuando un servicio devuelve un código de estado fuera del rango 2xx (Éxito), el monitor lo clasifica visualmente como ❌ FALLO TOTAL, pero muestra el código real entre paréntesis (ej: ❌ Caída (404)).',
};

TEXTOS_ES.leyenda.codigos_error = [
  {
    code: '2xx',
    label: 'OK / Éxito',
    description:
      'La conexión y el servicio respondieron correctamente (Latencia medida).',
  },
];

TEXTOS_ES.httpCodes
  .filter((item) => item.code === 0 || item.code >= 400)
  .forEach((item) => {
    TEXTOS_ES.leyenda.codigos_error.push({
      code: item.code.toString(),
      label: item.label,
      description: item.description,
    });
  });

// Solo asignar los textos a la variable global
// La lógica de carga está en i18n.js
window.TEXTOS_ACTUAL = TEXTOS_ES;

```

### Archivo: `leyenda.html`
```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title id="leyenda-titulo">Cargando Leyenda...</title>

    <link rel="stylesheet" href="css/leyenda_base.css?v=2025121501" />

    <link id="tema-base-css" rel="stylesheet" href="" />

    <script src="js/config.js?v=2025121501"></script>
    <script src="js/i18n.js?v=2025121501"></script>
  </head>
  <body>
    <!-- Botón Toggle Modo Oscuro - Esquina Superior Derecha -->
    <button
      id="theme-toggle-btn"
      class="theme-toggle-button-corner"
      onclick="toggleDarkMode()"
      title="Cambiar modo de visualización"
    >
      <span id="theme-icon">🌙</span>
    </button>

    <header>
      <h1 id="titulo-leyenda"></h1>
    </header>

    <main id="contenido-leyenda"></main>

    <script defer src="js/leyenda_script.js"></script>
  </body>
</html>

```

### Archivo: `netlify.toml`
```toml
[build]
  # Carpeta donde se encuentran las funciones serverless
  functions = "netlify/functions"
```

### Archivo: `netlify/functions/check-status.js`
```javascript
const fetch = require('node-fetch');
const AbortController = require('abort-controller');
const https = require('https');
const http = require('http');

// Configurables vía ENV
const VERIFY_TLS = process.env.VERIFY_TLS !== 'false'; // por defecto true
const MAX_READ_BYTES = parseInt(process.env.MAX_READ_BYTES || '65536', 10); // 64KB
const REQUEST_TIMEOUT_MS = parseInt(process.env.REQUEST_TIMEOUT_MS || '9000', 10);

// Agents: mantener conexiones vivas mejora realismo (simula keep-alive de clientes)
const httpsAgent = new https.Agent({
  rejectUnauthorized: VERIFY_TLS,
  keepAlive: true,
});

const httpAgent = new http.Agent({
  keepAlive: true,
});

// Heurística simple para detectar cold-start aproximado (no 100% fiable en serverless)
if (typeof global._lastInvocationAt === 'undefined') {
  global._lastInvocationAt = 0;
}

exports.handler = async (event, context) => {
  const targetUrl = event.queryStringParameters?.url;

  if (!targetUrl) {
    return {
      statusCode: 400,
      body: JSON.stringify({ error: "Parámetro 'url' requerido." }),
    };
  }

  const headers = {
    'Access-Control-Allow-Origin': '*',
    'Content-Type': 'application/json',
  };

  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), REQUEST_TIMEOUT_MS);

  const invocationNow = Date.now();
  const coldStart = invocationNow - (global._lastInvocationAt || 0) > 5 * 60 * 1000; // 5 min
  global._lastInvocationAt = invocationNow;

  try {
    const startTime = Date.now();

    const response = await fetch(targetUrl, {
      method: 'GET',
      signal: controller.signal,
      redirect: 'follow',
      // Selección de agente según protocolo
      agent: (_parsedURL) => {
        if (_parsedURL.protocol === 'http:') {
          return httpAgent;
        } else {
          return httpsAgent;
        }
      },
      headers: {
        'User-Agent': 'Mozilla/5.0 (Monitor-Status-Check)'
      },
    });

    const ttfb = Date.now() - startTime;

    // Intentamos leer hasta MAX_READ_BYTES para medir parte de la transferencia
    let bytesRead = 0;
    try {
      if (response.body && typeof response.body.on === 'function') {
        await new Promise((resolve) => {
          const stream = response.body;
          function cleanup() {
            stream.removeAllListeners('data');
            stream.removeAllListeners('end');
            stream.removeAllListeners('error');
          }

          stream.on('data', (chunk) => {
            try {
              bytesRead += chunk.length || chunk.byteLength || 0;
            } catch (e) {
              // ignore
            }
            if (bytesRead >= MAX_READ_BYTES) {
              try {
                stream.destroy();
              } catch (e) {
                // ignore
              }
              cleanup();
              resolve();
            }
          });

          stream.on('end', () => {
            cleanup();
            resolve();
          });

          stream.on('error', () => {
            cleanup();
            resolve();
          });
        });
      } else {
        // Fallback: leer todo (puede ser pesado)
        await response.arrayBuffer().then((b) => {
          bytesRead = b.byteLength || 0;
        });
      }
    } catch (e) {
      // No fallamos la función por errores de stream
    }

    clearTimeout(timeoutId);
    const endTime = Date.now();
    const totalTime = endTime - startTime;

    console.log(
      `URL: ${targetUrl} - Status: ${response.status} - TTFB: ${ttfb}ms - Total: ${totalTime}ms - BytesRead: ${bytesRead}`
    );

    return {
      statusCode: 200,
      headers,
      body: JSON.stringify({
        status: response.status,
        ttfb: ttfb,
        totalTime: totalTime,
        bytesRead: bytesRead,
        protocol: response.url?.startsWith('https') ? 'https' : 'http',
        tlsVerified: VERIFY_TLS,
        coldStart: !!coldStart,
      }),
    };
  } catch (error) {
    clearTimeout(timeoutId);

    console.error(
      `Error de conexión para ${targetUrl}: ${error.name} - ${error.message}`
    );

    return {
      statusCode: 200,
      headers,
      body: JSON.stringify({
        status: 0,
        ttfb: null,
        totalTime: 99999,
        bytesRead: 0,
        error: `${error.name}: ${error.message}`,
        tlsVerified: VERIFY_TLS,
        coldStart: !!coldStart,
      }),
    };
  }
};

```

### Archivo: `netlify/functions/rum-aggregate.js`
```javascript
// Returns aggregated RUM metrics from in-memory cache (per-instance)
exports.handler = async (event, context) => {
  const headers = {
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Methods': 'GET, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type',
    'Content-Type': 'application/json',
  };

  if (event.httpMethod === 'OPTIONS') {
    return { statusCode: 200, headers, body: '' };
  }

  // Use the same global cache used by rum-collect
  const cache = global.__rumCache || {};

  function percentile(arr, p) {
    if (!arr || arr.length === 0) return 0;
    const sorted = arr.slice().sort((a, b) => a - b);
    const idx = (p / 100) * (sorted.length - 1);
    const lower = Math.floor(idx);
    const upper = Math.ceil(idx);
    if (lower === upper) return Math.round(sorted[lower]);
    const weight = idx - lower;
    return Math.round(sorted[lower] * (1 - weight) + sorted[upper] * weight);
  }

  const result = {};

  for (const url in cache) {
    const entries = cache[url] || [];
    const times = [];
    let failures = 0;

    entries.forEach((e) => {
      // Prefer paint FCP if available, else navigation.responseStart, else nav.load
      let t = null;
      try {
        if (e.paints && e.paints['first-contentful-paint']) {
          t = Number(e.paints['first-contentful-paint']);
        } else if (e.nav && e.nav.responseStart) {
          t = Number(e.nav.responseStart);
        } else if (e.nav && e.nav.load) {
          t = Number(e.nav.load);
        }
      } catch (ex) {
        t = null;
      }

      if (t && Number.isFinite(t) && t > 0) {
        times.push(t);
      } else {
        failures++;
      }
    });

    const count = entries.length;
    const p50 = percentile(times, 50);
    const p95 = percentile(times, 95);
    const failureRate = count > 0 ? Math.round((failures / count) * 100) : 0;

    result[url] = {
      count,
      p50,
      p95,
      failureRate,
    };
  }

  return { statusCode: 200, headers, body: JSON.stringify(result) };
};

```

### Archivo: `netlify/functions/rum-collect.js`
```javascript
exports.handler = async (event, context) => {
  const headers = {
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Methods': 'POST, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type',
    'Content-Type': 'application/json',
  };

  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers,
      body: '',
    };
  }

  let payload = null;
  try {
    if (event.body) {
      payload = JSON.parse(event.body);
    } else if (event.queryStringParameters) {
      payload = event.queryStringParameters;
    }
  } catch (e) {
    console.error('RUM parse error', e);
    return { statusCode: 400, headers, body: JSON.stringify({ error: 'Invalid JSON' }) };
  }

  if (!payload || !payload.url) {
    return { statusCode: 400, headers, body: JSON.stringify({ error: 'url required in payload' }) };
  }

  // Ensure global cache exists. In serverless this is ephemeral and per-instance.
  if (!global.__rumCache) global.__rumCache = {};

  const urlKey = payload.url;
  if (!Array.isArray(global.__rumCache[urlKey])) global.__rumCache[urlKey] = [];

  const entry = {
    ts: payload.ts || Date.now(),
    nav: payload.nav || null,
    paints: payload.paints || null,
    resources: (payload.resources || []).slice(0, 8),
    connection: payload.connection || null,
    ua: (payload.ua || '').slice(0, 256),
  };

  global.__rumCache[urlKey].push(entry);

  // Keep only recent entries to limit memory
  const MAX_PER_URL = 200;
  if (global.__rumCache[urlKey].length > MAX_PER_URL) {
    global.__rumCache[urlKey] = global.__rumCache[urlKey].slice(-MAX_PER_URL);
  }

  try {
    console.log('RUM cached for', urlKey, 'entries:', global.__rumCache[urlKey].length);
  } catch (e) {}

  return { statusCode: 204, headers, body: '' };
};

```

### Archivo: `netlify/functions/update-webs.js`
```javascript
const fetch = require('node-fetch');

exports.handler = async (event, context) => {
  // Solo permitir POST
  if (event.httpMethod !== 'POST') {
    return {
      statusCode: 405,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Método no permitido' }),
    };
  }

  // Manejar CORS preflight
  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization',
        'Access-Control-Allow-Methods': 'POST, OPTIONS',
      },
      body: '',
    };
  }

  try {
    const { data, token } = JSON.parse(event.body);

    if (!data) {
      return {
        statusCode: 400,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Datos requeridos' }),
      };
    }

    // Validar que sea un array válido
    if (!Array.isArray(data)) {
      return {
        statusCode: 400,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Los datos deben ser un array' }),
      };
    }

    // Validar estructura de cada elemento
    for (const item of data) {
      if (!item.nombre || !item.url) {
        return {
          statusCode: 400,
          headers: {
            'Access-Control-Allow-Origin': '*',
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            error: 'Cada sitio debe tener nombre y url',
          }),
        };
      }
    }

    // Token de GitHub (debe estar en variables de entorno de Netlify)
    const githubToken = process.env.GITHUB_TOKEN || token;
    const repo = process.env.GITHUB_REPO || 'FABIOR1981/monitor-status-test';
    const branch = process.env.GITHUB_BRANCH || 'main';
    const filePath = 'data/webs.json';

    if (!githubToken) {
      return {
        statusCode: 401,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          error: 'Token de GitHub no configurado',
          help: 'Configura GITHUB_TOKEN en las variables de entorno de Netlify',
        }),
      };
    }

    // 1. Obtener el SHA actual del archivo
    const fileUrl = `https://api.github.com/repos/${repo}/contents/${filePath}?ref=${branch}`;
    const fileResponse = await fetch(fileUrl, {
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3+json',
      },
    });

    if (!fileResponse.ok) {
      const error = await fileResponse.text();
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          error: 'Error al obtener el archivo de GitHub',
          details: error,
        }),
      };
    }

    const fileData = await fileResponse.json();
    const currentSha = fileData.sha;

    // 2. Preparar el nuevo contenido
    const newContent = JSON.stringify(data, null, 2);
    const encodedContent = Buffer.from(newContent).toString('base64');

    // 3. Actualizar el archivo en GitHub
    const updateResponse = await fetch(fileUrl, {
      method: 'PUT',
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3+json',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        message: `🔧 Update webs.json via Admin Panel\n\nTotal sites: ${
          data.length
        }\nTimestamp: ${new Date().toLocaleString('es-UY', {
          timeZone: 'America/Montevideo',
        })}`,
        content: encodedContent,
        sha: currentSha,
        branch: branch,
      }),
    });

    if (!updateResponse.ok) {
      const error = await updateResponse.text();
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          error: 'Error al actualizar el archivo en GitHub',
          details: error,
        }),
      };
    }

    const updateResult = await updateResponse.json();

    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        success: true,
        message: 'Archivo actualizado correctamente en GitHub',
        commit: updateResult.commit.sha,
      }),
    };
  } catch (error) {
    return {
      statusCode: 500,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        error: 'Error interno del servidor',
        details: error.message,
      }),
    };
  }
};

```

### Archivo: `package.json`
```json
{
  "name": "monitor-status",
  "version": "1.0.0",
  "description": "Monitor de disponibilidad web con Netlify Functions.",
  "scripts": {
    "lint": "eslint . --ext .js,.html",
    "lint:fix": "eslint . --ext .js,.html --fix",
    "format": "prettier --write \"**/*.{js,css,html,json,md}\""
  },
  "dependencies": {
    "node-fetch": "^2.7.0",
    "abort-controller": "^3.0.0"
  },
  "devDependencies": {
    "eslint": "^8.43.0",
    "prettier": "^2.8.0",
    "eslint-config-prettier": "^8.8.0",
    "eslint-plugin-html": "^7.1.0"
  }
}

```

### Archivo: `readme.md`
```md
# Monitor de Estado de Servicios

Monitor visual y ligero que muestra el estado (OK / error) y la latencia (ms) de tus sitios web. Guarda un historial corto por sesión y alerta cuando detecta fallos o latencias altas.

Características principales:

- Panel con estado y latencia en tiempo real por sitio.
- Historial corto por sesión para seguir tendencias recientes.
- Alertas visuales y registro de errores, con opción para copiar detalles.
- Temas claro/oscuro y leyenda de umbrales para interpretar latencias.

Uso rápido:

- Abrí `index.html` en el navegador para ver el tablero en tiempo real.
- Consultá la leyenda en `leyenda.html` para entender colores y umbrales.

Dónde ver documentación y configuración:

- Documentación técnica y opciones de despliegue: en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/monitor-status-test/documentacion) (ver `readme.md`).
- Funciones serverless relacionadas: `netlify/functions/`.

Contribuir:

- Reportá issues o enviá pull requests si querés mejorar algo. Puedo agregar una guía corta de contribución si querés.

```

