# Bundle del Proyecto para IA: monitor-status2

## Estructura de Archivos
- abm-webs.html
- css/abm_base.css
- css/abm_claro.css
- css/abm_oscuro.css
- css/alertas_error.css
- css/leyenda_base.css
- css/leyenda_claro.css
- css/leyenda_oscuro.css
- css/monitor_base.css
- css/monitor_def.css
- css/monitor_osc.css
- css/monitor_tarjetas.css
- data/webs.json
- documentacion/LEEME.md
- index.html
- js/alertas_error.js
- js/app.js
- js/config.js
- js/estado.js
- js/i18n.js
- js/leyenda_script.js
- js/monitoreo.js
- js/render-tarjetas.js
- js/temas.js
- js/ui-textos.js
- lang/i18n_en.js
- lang/i18n_es.js
- lang/i18n_fr.js
- leyenda.html
- netlify.toml
- netlify/functions/check-status.js
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
    <link rel="stylesheet" href="css/abm_base.css" />
    <link id="tema-base-css" rel="stylesheet" href="" />
  </head>
  <body>
    <!-- Botón Toggle Modo Oscuro - Esquina Superior Derecha -->
    <button
      id="theme-toggle-btn"
      class="theme-toggle-button-corner"
      onclick="toggleDarkModeABM()"
      title="Cambiar modo de visualización"
    >
      <span id="theme-icon">🌙</span>
    </button>

    <!-- Pantalla de acceso: se muestra hasta ingresar la contraseña correcta -->
    <div id="login-gate" class="login-gate">
      <div class="login-box">
        <h1>🔒 Acceso Restringido</h1>
        <p>Ingresá la contraseña para administrar los sitios monitoreados.</p>
        <form id="login-form" autocomplete="off">
          <div class="password-wrapper">
            <input
              type="password"
              id="login-password"
              placeholder="Contraseña"
              autocomplete="current-password"
              required
            />
            <button
              type="button"
              id="btn-ver-password"
              class="btn-ver-password"
              title="Mostrar contraseña"
            >
              👁️
            </button>
          </div>
          <button type="submit">Ingresar</button>
        </form>
        <p id="login-error" class="login-error hidden">
          ❌ Contraseña incorrecta.
        </p>
      </div>
    </div>

    <div class="container" id="admin-container">
      <header>
        <h1>⚙️ Panel de Administración</h1>
        <p>Gestión de Sitios Web del Monitor de Estado</p>
      </header>

      <div class="nav-links">
        <a href="index.html">← Volver al Monitor</a>
        <a href="leyenda.html">Ver Leyenda</a>
        <a href="#" id="btn-cerrar-sesion">🔒 Cerrar sesión</a>
      </div>

      <div class="content">
        <div id="mensaje" class="hidden"></div>

        <!-- Formulario ABM -->
        <div class="form-section">
          <button type="button" class="btn btn-primary" id="btn-abrir-modal-sitio">
            ➕ Agregar Nuevo Sitio
          </button>
        </div>

        <!-- Modal de alta/edición de sitio -->
        <div id="sitio-modal-backdrop" class="sitio-modal-backdrop hidden">
          <div class="sitio-modal-box">
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
                <p id="url-duplicada-error" class="login-error hidden">
                  ⚠️ Esa URL ya está en la lista.
                </p>
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

              <div style="display: flex; gap: 10px">
                <button type="submit" class="btn btn-primary" id="btn-submit">
                  Agregar Sitio
                </button>
                <button
                  type="button"
                  class="btn btn-edit"
                  id="btn-cancel"
                  onclick="cerrarModalSitio()"
                >
                  Cancelar
                </button>
              </div>
            </form>
          </div>
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

    <script src="js/config.js"></script>
    <script>
      // =======================================================
      // TEMA (claro/oscuro) — mismo mecanismo que index.html/leyenda.html
      // =======================================================
      function inicializarTemaABM() {
        const params = new URLSearchParams(window.location.search);
        let temaParam = params.get('tema')
          ? params.get('tema').toLowerCase()
          : null;

        if (!temaParam || !ABM_TEMA_FILES[temaParam]) {
          temaParam = DEFAULT_LEYENDA_TEMA; // 'def', mismo default que el resto de la app
        }

        const temaLink = document.getElementById('tema-base-css');
        const rutaCss = ABM_TEMA_FILES[temaParam] || ABM_TEMA_FILES.default;
        temaLink.href = rutaCss;

        actualizarBotonToggleABM(temaParam);
      }

      function actualizarBotonToggleABM(temaActual) {
        const icono = document.getElementById('theme-icon');
        const btn = document.getElementById('theme-toggle-btn');
        if (!icono || !btn) return;

        const nuevoTema = TEMA_TOGGLE_PAIRS[temaActual];
        if (!nuevoTema) {
          btn.style.display = 'none';
          return;
        }
        btn.style.display = 'flex';
        icono.textContent = nuevoTema === TEMA_OSC ? '🌙' : '☀️';
        btn.title =
          nuevoTema === TEMA_OSC
            ? 'Cambiar a modo oscuro'
            : 'Cambiar a modo claro';
      }

      function toggleDarkModeABM() {
        const params = new URLSearchParams(window.location.search);
        const temaUrl = params.get('tema');
        let temaActual = DEFAULT_LEYENDA_TEMA;
        if (temaUrl && ABM_TEMA_FILES[temaUrl]) {
          temaActual = temaUrl;
        }

        const nuevoTema = TEMA_TOGGLE_PAIRS[temaActual];
        if (!nuevoTema) return;

        document.getElementById('tema-base-css').href = ABM_TEMA_FILES[nuevoTema];

        params.set('tema', nuevoTema);
        const nuevaUrl = `${window.location.pathname}?${params.toString()}`;
        window.history.replaceState({}, '', nuevaUrl);

        actualizarBotonToggleABM(nuevoTema);
      }

      inicializarTemaABM();

      // =======================================================
      // ACCESO RESTRINGIDO (contraseña embebida como hash SHA-256)
      // =======================================================
      // HASH_ACCESO_SHA256 viene de js/config.js (fuente única): la misma
      // constante la valida también netlify/functions/update-webs.js del
      // lado del servidor. Nunca se guarda ni se transmite la contraseña en
      // texto plano salvo, dentro de esta misma sesión de navegador, al
      // llamar a la función serverless para guardar cambios.

      async function calcularHashSHA256(texto) {
        const datos = new TextEncoder().encode(texto);
        const buffer = await crypto.subtle.digest('SHA-256', datos);
        return Array.from(new Uint8Array(buffer))
          .map((b) => b.toString(16).padStart(2, '0'))
          .join('');
      }

      async function verificarAcceso(password) {
        const hashIngresado = await calcularHashSHA256(password);
        return hashIngresado === HASH_ACCESO_SHA256;
      }

      function ocultarModalLogin() {
        document.getElementById('login-gate').classList.add('hidden');
      }

      function mostrarModalLogin() {
        document.getElementById('login-gate').classList.remove('hidden');
        document.body.classList.add('modal-abierto');
      }

      // Si ya se autenticó antes en esta pestaña (sessionStorage: se borra al
      // cerrar la pestaña/navegador), no volver a pedir la contraseña.
      if (sessionStorage.getItem('abmAuthToken')) {
        ocultarModalLogin();
      } else {
        document.body.classList.add('modal-abierto');
      }

      document
        .getElementById('btn-ver-password')
        .addEventListener('click', () => {
          const input = document.getElementById('login-password');
          const btn = document.getElementById('btn-ver-password');
          const oculto = input.type === 'password';
          input.type = oculto ? 'text' : 'password';
          btn.textContent = oculto ? '🙈' : '👁️';
          btn.title = oculto ? 'Ocultar contraseña' : 'Mostrar contraseña';
        });

      document
        .getElementById('login-form')
        .addEventListener('submit', async (e) => {
          e.preventDefault();
          const input = document.getElementById('login-password');
          const errorMsg = document.getElementById('login-error');
          const password = input.value;

          if (await verificarAcceso(password)) {
            // Se guarda en sessionStorage (no localStorage) para que se
            // necesite el envío del header x-admin-password al guardar
            // cambios; el servidor vuelve a validar su propio hash.
            sessionStorage.setItem('abmAuthToken', password);
            errorMsg.classList.add('hidden');
            input.value = '';
            document.body.classList.remove('modal-abierto');
            ocultarModalLogin();
          } else {
            errorMsg.classList.remove('hidden');
            input.value = '';
            input.focus();
          }
        });

      document
        .getElementById('btn-cerrar-sesion')
        .addEventListener('click', (e) => {
          e.preventDefault();
          sessionStorage.removeItem('abmAuthToken');
          document.getElementById('login-password').value = '';
          document.getElementById('login-error').classList.add('hidden');
          mostrarModalLogin();
          document.getElementById('login-password').focus();
        });

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

      // Normaliza una URL para poder comparar (ignora mayúsculas, espacios
      // y una barra final) sin importar cómo la haya tipeado el usuario.
      function normalizarUrl(url) {
        return url.trim().toLowerCase().replace(/\/+$/, '');
      }

      // Agregar o actualizar sitio
      document
        .getElementById('abm-form')
        .addEventListener('submit', function (e) {
          e.preventDefault();

          const editIndex = document.getElementById('edit-index').value;
          const indiceEditando = editIndex !== '' ? parseInt(editIndex) : -1;

          const sitio = {
            nombre: document.getElementById('nombre').value.trim(),
            url: document.getElementById('url').value.trim(),
          };

          // Validar que la URL no esté ya en la lista (ignorando el propio
          // sitio si estamos editándolo, para no chocar contra sí mismo).
          const urlNormalizada = normalizarUrl(sitio.url);
          const yaExiste = sitios.some(
            (s, i) => i !== indiceEditando && normalizarUrl(s.url) === urlNormalizada
          );
          const errorDuplicada = document.getElementById('url-duplicada-error');

          if (yaExiste) {
            errorDuplicada.classList.remove('hidden');
            return; // No cerrar el modal ni guardar nada
          }
          errorDuplicada.classList.add('hidden');

          const orden = document.getElementById('orden').value;
          if (orden) {
            sitio.orden = parseInt(orden);
          }

          const grupo = document.getElementById('grupo').value;
          if (grupo) {
            sitio.grupo = grupo;
          }

          if (indiceEditando !== -1) {
            // Editar
            sitios[indiceEditando] = sitio;
            mostrarMensaje('✅ Sitio actualizado correctamente', 'success');
          } else {
            // Agregar
            sitios.push(sitio);
            mostrarMensaje('✅ Sitio agregado correctamente', 'success');
          }

          guardarEnLocalStorage();
          renderizarTabla();
          cerrarModalSitio();
        });

      // Abrir el modal para agregar un sitio nuevo (formulario en blanco)
      function abrirModalSitio() {
        document.getElementById('edit-index').value = '';
        document.getElementById('abm-form').reset();
        document.getElementById('url-duplicada-error').classList.add('hidden');
        document.getElementById('form-title').textContent =
          '➕ Agregar Nuevo Sitio';
        document.getElementById('btn-submit').textContent = 'Agregar Sitio';
        document.getElementById('sitio-modal-backdrop').classList.remove('hidden');
        document.getElementById('nombre').focus();
      }

      // Cerrar el modal sin guardar
      function cerrarModalSitio() {
        document.getElementById('sitio-modal-backdrop').classList.add('hidden');
        document.getElementById('abm-form').reset();
        document.getElementById('edit-index').value = '';
        document.getElementById('url-duplicada-error').classList.add('hidden');
      }

      document
        .getElementById('btn-abrir-modal-sitio')
        .addEventListener('click', abrirModalSitio);

      // Cerrar el modal si se hace click fuera de la caja (en el fondo)
      document
        .getElementById('sitio-modal-backdrop')
        .addEventListener('click', function (e) {
          if (e.target === this) cerrarModalSitio();
        });

      // Editar sitio
      function editarSitio(index) {
        const sitio = sitios[index];

        document.getElementById('edit-index').value = index;
        document.getElementById('nombre').value = sitio.nombre;
        document.getElementById('url').value = sitio.url;
        document.getElementById('orden').value = sitio.orden || '';
        document.getElementById('grupo').value = sitio.grupo || '';
        document.getElementById('url-duplicada-error').classList.add('hidden');

        document.getElementById('form-title').textContent = '✏️ Editar Sitio';
        document.getElementById('btn-submit').textContent = 'Actualizar Sitio';
        document.getElementById('sitio-modal-backdrop').classList.remove('hidden');
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
              'x-admin-password': sessionStorage.getItem('abmAuthToken') || '',
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

### Archivo: `css/abm_base.css`
```css
/* abm_base.css
   Estructura y layout de abm-webs.html, independiente del tema.
   Los colores usan variables CSS (--abm-*) que definen abm_claro.css y
   abm_oscuro.css — este archivo solo trae valores de resguardo (fallback)
   por si alguno de los dos no llegara a cargar. */

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
    'Helvetica Neue', Arial, sans-serif;
  background: var(--abm-bg, #f4f7f9);
  color: var(--abm-text, #333333);
  min-height: 100vh;
  padding: 20px;
}
.container {
  max-width: 1200px;
  margin: 0 auto;
  background: var(--abm-container-bg, #ffffff);
  border-radius: 8px;
  box-shadow: var(--abm-container-shadow, 0 4px 8px rgba(0, 0, 0, 0.1));
  overflow: hidden;
}
header {
  background: var(--abm-header-bg, #007bff);
  color: var(--abm-header-text, #ffffff);
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
  background: var(--abm-nav-bg, #f8f9fa);
}
.nav-links a {
  color: var(--abm-link-color, #007bff);
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
  background: var(--abm-form-section-bg, #f8f9fa);
  padding: 25px;
  border-radius: 8px;
  margin-bottom: 30px;
}
.form-section h2 {
  color: var(--abm-text, #333333);
  margin-bottom: 20px;
  font-size: 1.5em;
}
.form-group {
  margin-bottom: 15px;
}
.form-group label {
  display: block;
  margin-bottom: 5px;
  color: var(--abm-label-color, #555555);
  font-weight: 500;
}
.form-group input,
.form-group select {
  width: 100%;
  padding: 10px;
  border: 1px solid var(--abm-input-border, #dddddd);
  border-radius: 6px;
  font-size: 14px;
  background: var(--abm-input-bg, #ffffff);
  color: var(--abm-text, #333333);
}
.form-group input:focus,
.form-group select:focus {
  outline: none;
  border-color: var(--abm-input-focus-border, #007bff);
  box-shadow: 0 0 0 3px var(--abm-input-focus-shadow, rgba(0, 123, 255, 0.1));
}
.form-group small {
  display: block;
  margin-top: 5px;
  color: var(--abm-muted-text, #777777);
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
  color: var(--abm-text, #333333);
  margin-bottom: 20px;
  font-size: 1.5em;
}
table {
  width: 100%;
  border-collapse: collapse;
  background: var(--abm-container-bg, #ffffff);
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
thead {
  background: var(--abm-table-header-bg, #004085);
  color: var(--abm-table-header-text, #ffffff);
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
  border-bottom: 1px solid var(--abm-table-row-border, #eeeeee);
}
tbody tr:hover {
  background: var(--abm-table-row-hover-bg, #f8f9fa);
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
  background: var(--abm-form-section-bg, #f8f9fa);
  border-radius: 8px;
  margin-top: 20px;
}
.download-section p {
  margin-bottom: 15px;
  color: var(--abm-label-color, #555555);
}
.hidden {
  display: none !important;
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

/* --- Botón flotante de alternar tema (mismo estilo que index.html/leyenda.html) --- */
.theme-toggle-button-corner {
  position: fixed;
  top: 15px;
  right: 15px;
  z-index: 1100;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: none;
  background: var(--abm-container-bg, #ffffff);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  font-size: 1.3em;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.2s;
}
.theme-toggle-button-corner:hover {
  transform: scale(1.08);
}

/* --- Pantalla de acceso --- */
body.modal-abierto {
  overflow: hidden;
}
.login-gate {
  position: fixed;
  inset: 0;
  z-index: 1000;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #1a1a2e;
  padding: 20px;
}
.login-box {
  background: var(--abm-container-bg, #ffffff);
  color: var(--abm-text, #333333);
  border-radius: 12px;
  padding: 40px 32px;
  max-width: 360px;
  width: 100%;
  text-align: center;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
}
.login-box h1 {
  margin: 0 0 8px;
  font-size: 1.4em;
}
.login-box p {
  color: var(--abm-muted-text, #666666);
  margin-bottom: 20px;
  font-size: 0.9em;
}
.login-box form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.login-box input[type='password'],
.login-box input[type='text'] {
  padding: 10px 40px 10px 12px;
  border: 1px solid var(--abm-input-border, #cccccc);
  border-radius: 8px;
  font-size: 1em;
  width: 100%;
  box-sizing: border-box;
  background: var(--abm-input-bg, #ffffff);
  color: var(--abm-text, #333333);
}
.password-wrapper {
  position: relative;
  display: flex;
}
.btn-ver-password {
  position: absolute;
  right: 4px;
  top: 50%;
  transform: translateY(-50%);
  border: none;
  background: transparent;
  cursor: pointer;
  font-size: 1.1em;
  padding: 4px 8px;
  line-height: 1;
}
.login-box button {
  padding: 10px 12px;
  border: none;
  border-radius: 8px;
  background: #1976d2;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s;
}
.login-box button:hover {
  background: #1565c0;
}
.login-error {
  color: #dc3545;
  margin-top: 12px;
  margin-bottom: 0;
  font-size: 0.9em;
}

/* --- Modal de alta/edición de sitio --- */
.sitio-modal-backdrop {
  position: fixed;
  inset: 0;
  z-index: 900;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  background: rgba(0, 0, 0, 0.55);
  padding: 40px 20px;
  overflow-y: auto;
}
.sitio-modal-box {
  background: var(--abm-container-bg, #ffffff);
  color: var(--abm-text, #333333);
  border-radius: 12px;
  padding: 28px 32px;
  max-width: 480px;
  width: 100%;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.3);
}
.sitio-modal-box h2 {
  margin-top: 0;
}

```

### Archivo: `css/abm_claro.css`
```css
/* abm_claro.css — Tema claro (por defecto) para abm-webs.html */
:root {
  --abm-bg: #f4f7f9;
  --abm-text: #333333;
  --abm-container-bg: #ffffff;
  --abm-container-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  --abm-header-bg: #007bff;
  --abm-header-text: #ffffff;
  --abm-nav-bg: #f8f9fa;
  --abm-link-color: #007bff;
  --abm-form-section-bg: #f8f9fa;
  --abm-label-color: #555555;
  --abm-muted-text: #777777;
  --abm-input-bg: #ffffff;
  --abm-input-border: #dddddd;
  --abm-input-focus-border: #007bff;
  --abm-input-focus-shadow: rgba(0, 123, 255, 0.1);
  --abm-table-header-bg: #004085;
  --abm-table-header-text: #ffffff;
  --abm-table-row-border: #eeeeee;
  --abm-table-row-hover-bg: #f8f9fa;
}

```

### Archivo: `css/abm_oscuro.css`
```css
/* abm_oscuro.css — Tema oscuro para abm-webs.html
   Misma paleta azul marino que monitor_osc.css, para que se sienta
   consistente con el resto de la app en modo oscuro. */
:root {
  --abm-bg: #1a1a2e;
  --abm-text: #e0e0e0;
  --abm-container-bg: #16213e;
  --abm-container-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
  --abm-header-bg: #12182e;
  --abm-header-text: #ffffff;
  --abm-nav-bg: #1f2b4d;
  --abm-link-color: #5cb3ff;
  --abm-form-section-bg: #1f2b4d;
  --abm-label-color: #c0c0c0;
  --abm-muted-text: #999999;
  --abm-input-bg: #22304f;
  --abm-input-border: #3a4a6e;
  --abm-input-focus-border: #5cb3ff;
  --abm-input-focus-shadow: rgba(92, 179, 255, 0.2);
  --abm-table-header-bg: #0d1b3d;
  --abm-table-header-text: #ffffff;
  --abm-table-row-border: #2a3b5e;
  --abm-table-row-hover-bg: #1c2b4d;
}

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

/* --- Sección de iconos de fuente (🌐 proxy / 🖥️ directo) --- */
.leyenda-lista-iconos {
  list-style: disc;
  padding-left: 22px;
  margin: 10px 0;
}

.leyenda-lista-iconos li {
  margin-bottom: 10px;
  line-height: 1.5;
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
  --status-bloqueo-externo-bg: transparent;
  --status-bloqueo-externo-color: inherit;

  /* Tarjetas de servicio (vista "Tarjetas") */
  --card-bg: #ffffff;
  --card-gradient-start: #ffffff;
  --card-gradient-ok-end: #f0f9f0;
  --card-gradient-lento-end: #fff8f0;
  --card-gradient-critico-end: #fff5f5;
  --card-gradient-caido-end: #f9f9f9;
  --card-gradient-bloqueo-end: #fff3e0;
  --card-text: #333333;
  --card-text-secondary: #888888;
  --card-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
  --card-shadow-hover: 0 4px 12px rgba(0, 0, 0, 0.12);
  --card-border-subtle: rgba(0, 0, 0, 0.06);
  --card-footer-link: #1976d2;
  --skeleton-shimmer: rgba(0, 0, 0, 0.12);
  --aviso-medicion-bg: #eef6fc;
  --aviso-medicion-border: #90caf9;
  --aviso-medicion-text: #0d47a1;
  --aviso-medicion-link: #1565c0;

  /* Contadores del resumen superior */
  --contador-bg: #ffffff;
  --contador-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
  --contador-label: #666666;

  /* Barra de controles y toggles de vista */
  --control-select-bg: #ffffff;
  --control-select-border: #cccccc;
  --control-select-text: inherit;
  --btn-reiniciar-bg: #28a745;
  --btn-reiniciar-bg-hover: #218838;
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
.status-bloqueo-externo {
  background-color: var(--status-bloqueo-externo-bg);
  color: var(--status-bloqueo-externo-color);
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
.estado-linea {
  display: flex;
  justify-content: center;
  align-items: baseline;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 20px;
  font-size: 0.9em;
}
.info-bar {
  /* El color lo define cada tema (monitor_def.css / monitor_osc.css) */
}
.separador-punto {
  opacity: 0.5;
}
.update-info {
  font-weight: bold;
}

.aviso-medicion {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
  justify-content: center;
  text-align: center;
  background: var(--aviso-medicion-bg, #eef6fc);
  border: 1px solid var(--aviso-medicion-border, #90caf9);
  color: var(--aviso-medicion-text, #0d47a1);
  border-radius: 8px;
  padding: 6px 36px 6px 12px;
  font-size: 0.78em;
  margin-bottom: 15px;
  position: relative;
}
.aviso-medicion a {
  color: var(--aviso-medicion-link, #1565c0);
  font-weight: 600;
  text-decoration: none;
  white-space: nowrap;
}
.aviso-medicion a:hover {
  text-decoration: underline;
}
.btn-cerrar-aviso {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  border: none;
  background: transparent;
  color: var(--aviso-medicion-text, #0d47a1);
  opacity: 0.6;
  cursor: pointer;
  font-size: 1em;
  padding: 4px 6px;
  line-height: 1;
}
.btn-cerrar-aviso:hover {
  opacity: 1;
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

/* Clases de estado base (colores definidos por el tema) */
.status-very-fast,
.status-fast,
.status-normal,
.status-slow,
.status-critical,
.status-risk,
.status-extreme-risk,
.status-down,
.status-bloqueo-externo {
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
.status-bloqueo-externo::after {
  content: ' 🟠';
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
.status-down[style*='cursor: pointer']:hover,
.status-bloqueo-externo[style*='cursor: pointer']:hover {
  opacity: 0.8;
  text-decoration: underline;
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
  --status-bloqueo-externo-bg: #fff3e0;
  --status-bloqueo-externo-color: #e65100;
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

### Archivo: `css/monitor_osc.css`
```css
:root {
  /*(azul marino) */
  --bg: #1a1a2e;
  --text: #e0e0e0;
  --accent: #5cb3ff;
  --container-bg: #16213e;
  --container-radius: 8px;
  --container-shadow: 0 8px 24px rgba(0, 0, 0, 0.4);
  --header-color: #5cb3ff;
  --table-header-bg: #1f3a5f;
  --table-header-color: #ffffff;
  --link-color: #5cb3ff;

  /* Tarjetas de servicio (vista "Tarjetas") */
  --card-bg: #16213e;
  --card-gradient-start: #16213e;
  --card-gradient-ok-end: #1b5e20;
  --card-gradient-lento-end: #e65100;
  --card-gradient-critico-end: #b71c1c;
  --card-gradient-caido-end: #424242;
  --card-gradient-bloqueo-end: #7a3e00;
  --card-text: #e0e0e0;
  --card-text-secondary: #aaaaaa;
  --card-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  --card-shadow-hover: 0 6px 20px rgba(0, 0, 0, 0.4);
  --card-border-subtle: rgba(255, 255, 255, 0.08);
  --card-footer-link: #5cb3ff;
  --skeleton-shimmer: rgba(255, 255, 255, 0.18);
  --aviso-medicion-bg: #16213e;
  --aviso-medicion-border: #2a3b5e;
  --aviso-medicion-text: #a8d4ff;
  --aviso-medicion-link: #5cb3ff;

  /* Contadores del resumen superior */
  --contador-bg: #16213e;
  --contador-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  --contador-label: #aaaaaa;

  /* Barra de controles y toggles de vista */
  --control-select-bg: #3a3a3a;
  --control-select-border: #4a9eff;
  --control-select-text: #e0e0e0;
  --btn-reiniciar-bg: #1976d2;
  --btn-reiniciar-bg-hover: #1565c0;
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
  background-color: #16213e;
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
  background-color: #1c2b4d;
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
  --status-bloqueo-externo-bg: #7a3e00;
  --status-bloqueo-externo-color: #ffcc80;
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



```

### Archivo: `css/monitor_tarjetas.css`
```css
/* =======================================================
   BARRA DE CONTROLES COMPACTA (selector + botones)
   ======================================================= */
.controls-bar {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
}

.control-group {
  display: inline-flex;
  align-items: center;
  gap: 8px;
}

.control-label {
  font-size: 0.9em;
}

.control-select {
  padding: 8px 14px;
  border-radius: 20px;
  border: 1px solid var(--control-select-border, #ccc);
  background-color: var(--control-select-bg, #ffffff);
  color: var(--control-select-text, inherit);
  font-size: 0.9em;
  cursor: pointer;
}

.btn-reiniciar {
  border: none;
  padding: 8px 20px;
  border-radius: 20px;
  cursor: pointer;
  font-size: 0.95em;
  font-weight: 600;
  background: var(--btn-reiniciar-bg, #28a745);
  color: white;
  display: inline-flex;
  align-items: center;
  gap: 6px;
  transition: background-color 0.2s, transform 0.15s;
}

.btn-reiniciar:hover {
  background: var(--btn-reiniciar-bg-hover, #218838);
  transform: translateY(-1px);
}

/* =======================================================
   ESTILOS PARA VISTA DE TARJETAS
   ======================================================= */

/* Grid de tarjetas - COMPACTO */
.grid-tarjetas {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 12px;
  margin-bottom: 20px;
  display: none;
}

.grid-tarjetas.visible {
  display: grid;
}

.tarjeta-servicio {
  background: var(--card-bg, #ffffff);
  color: var(--card-text, #333333);
  border-radius: 10px;
  padding: 12px 14px;
  box-shadow: var(--card-shadow, 0 1px 4px rgba(0, 0, 0, 0.08));
  border-left: 4px solid;
  transition: transform 0.15s, box-shadow 0.15s;
  position: relative;
  font-size: 0.9em;
  animation: tarjetaAparecer 0.4s ease-out backwards;
}

/* Aparición escalonada: cada tarjeta hace fade-in + leve desplazamiento hacia
   arriba. El delay real por tarjeta lo define el JS (style="animation-delay") */
@keyframes tarjetaAparecer {
  from {
    opacity: 0;
    transform: translateY(10px) scale(0.97);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

@media (prefers-reduced-motion: reduce) {
  .tarjeta-servicio {
    animation: none;
  }
}

/* =======================================================
   TARJETAS ESQUELETO (placeholder mientras llegan los datos)
   ======================================================= */
.tarjeta-skeleton {
  background: var(--card-bg, #ffffff);
  border-radius: 10px;
  padding: 12px 14px;
  box-shadow: var(--card-shadow, 0 1px 4px rgba(0, 0, 0, 0.08));
  border-left: 4px solid var(--card-border-subtle, rgba(0, 0, 0, 0.06));
  display: flex;
  flex-direction: column;
  gap: 10px;
  min-height: 108px;
  animation: tarjetaAparecer 0.3s ease-out backwards;
}

.skeleton-linea {
  border-radius: 6px;
  height: 12px;
  background: linear-gradient(
    90deg,
    var(--card-border-subtle, rgba(0, 0, 0, 0.08)) 25%,
    var(--skeleton-shimmer, rgba(0, 0, 0, 0.15)) 37%,
    var(--card-border-subtle, rgba(0, 0, 0, 0.08)) 63%
  );
  background-size: 400% 100%;
  animation: skeletonShimmer 1.4s ease-in-out infinite;
}

.skeleton-titulo {
  width: 55%;
  height: 14px;
}

.skeleton-numero {
  width: 40%;
  height: 22px;
}

.skeleton-corta {
  width: 70%;
}

@keyframes skeletonShimmer {
  0% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

@media (prefers-reduced-motion: reduce) {
  .tarjeta-skeleton,
  .skeleton-linea {
    animation: none;
  }
}

.tarjeta-servicio:hover {
  transform: translateY(-2px);
  box-shadow: var(--card-shadow-hover, 0 4px 12px rgba(0, 0, 0, 0.12));
}

.tarjeta-servicio.estado-ok {
  border-left-color: #4caf50;
  background: linear-gradient(
    135deg,
    var(--card-gradient-start, #ffffff) 0%,
    var(--card-gradient-ok-end, #f0f9f0) 100%
  );
}
.tarjeta-servicio.estado-lento {
  border-left-color: #ff9800;
  background: linear-gradient(
    135deg,
    var(--card-gradient-start, #ffffff) 0%,
    var(--card-gradient-lento-end, #fff8f0) 100%
  );
}
.tarjeta-servicio.estado-critico {
  border-left-color: #f44336;
  background: linear-gradient(
    135deg,
    var(--card-gradient-start, #ffffff) 0%,
    var(--card-gradient-critico-end, #fff5f5) 100%
  );
}
.tarjeta-servicio.estado-caido {
  border-left-color: #9e9e9e;
  background: linear-gradient(
    135deg,
    var(--card-gradient-start, #ffffff) 0%,
    var(--card-gradient-caido-end, #f9f9f9) 100%
  );
}

.tarjeta-servicio.directo {
  border-right: 2px solid #3498db;
}

/* Header compacto */
.tarjeta-header-compacto {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 6px;
  border-bottom: 1px solid var(--card-border-subtle, rgba(0, 0, 0, 0.06));
  padding-bottom: 5px;
}

.tarjeta-nombre-compacto {
  font-weight: 700;
  font-size: 0.95em;
  color: var(--card-text, #333);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 85%;
}

.tarjeta-fuente-compacto {
  font-size: 0.85em;
  opacity: 0.6;
}

/* Body compacto */
.tarjeta-body-compacto {
  margin-bottom: 6px;
}

.tarjeta-latencia-compacto {
  display: flex;
  align-items: baseline;
  gap: 4px;
  margin-bottom: 4px;
}

.latencia-numero {
  font-size: 1.8em;
  font-weight: 700;
  color: var(--card-text, #333);
  line-height: 1.1;
}

.latencia-unidad {
  font-size: 0.65em;
  color: var(--card-text-secondary, #888);
  font-weight: 400;
}

.latencia-fuente {
  font-size: 0.75em;
  margin-left: auto;
  opacity: 0.5;
}

.tarjeta-fila-datos {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
  font-size: 0.8em;
  line-height: 1.4;
  margin-bottom: 2px;
}

.dato-promedio {
  color: var(--card-text-secondary, #666);
  font-weight: 500;
}

.dato-estado-prom {
  font-weight: 600;
  font-size: 0.85em;
}

.tendencia {
  font-size: 1em;
  letter-spacing: 1px;
}

.estado-actual {
  font-weight: 600;
  font-size: 0.9em;
}

.tarjeta-errores {
  color: #c92a2a;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.85em;
}

.tarjeta-errores:hover {
  text-decoration: underline;
}

/* Footer compacto */
.tarjeta-footer-compacto {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid var(--card-border-subtle, rgba(0, 0, 0, 0.06));
  padding-top: 5px;
  font-size: 0.75em;
}

.tarjeta-footer-compacto a {
  color: var(--card-footer-link, #1976d2);
  text-decoration: none;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 75%;
}

.tarjeta-footer-compacto a:hover {
  text-decoration: underline;
}

.psi-compacto {
  padding: 2px 8px !important;
  font-size: 0.7em !important;
  border-radius: 10px !important;
}

/* Detalle de errores dentro de la vista de tarjetas */
.error-detail-card {
  grid-column: 1 / -1;
  background: #fff5f5;
  border-left: 4px solid #ff6b6b;
  border-radius: 8px;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.3s ease-out;
}

.error-detail-card.expanded {
  max-height: 500px;
  transition: max-height 0.3s ease-in;
}

/* Resumen superior para tarjetas */
.resumen-superior {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 10px;
  margin-bottom: 15px;
  display: none;
}

.resumen-superior.visible {
  display: grid;
}

.contador {
  background: var(--contador-bg, #ffffff);
  border-radius: 8px;
  padding: 10px 8px;
  text-align: center;
  box-shadow: var(--contador-shadow, 0 1px 4px rgba(0, 0, 0, 0.08));
  border-top: 3px solid;
  transition: transform 0.15s;
}

.contador:hover {
  transform: translateY(-1px);
}

.contador.ok {
  border-top-color: #4caf50;
}
.contador.ok .contador-numero {
  color: #4caf50;
}
.contador.lento {
  border-top-color: #ff9800;
}
.contador.lento .contador-numero {
  color: #ff9800;
}
.contador.critico {
  border-top-color: #f44336;
}
.contador.critico .contador-numero {
  color: #f44336;
}
.contador.caido {
  border-top-color: #9e9e9e;
}
.contador.caido .contador-numero {
  color: #9e9e9e;
}

.contador-numero {
  display: block;
  font-size: 1.6em;
  font-weight: 700;
  line-height: 1;
  margin-bottom: 2px;
}

.contador-label {
  font-size: 0.75em;
  color: var(--contador-label, #666);
  font-weight: 500;
}

/* Ocultar tabla cuando tarjetas están activas */
.table-scroll-container.tarjetas-activas {
  display: none;
}

@media (max-width: 768px) {
  .resumen-superior {
    grid-template-columns: repeat(2, 1fr);
  }
  .grid-tarjetas {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 480px) {
  .resumen-superior {
    grid-template-columns: 1fr;
  }
}

```

### Archivo: `data/webs.json`
```json
[
  {
    "nombre": "AhiVa",
    "url": "https://ahiva.correo.com.uy"
  },
  {
    "nombre": "CasillaMia",
    "url": "https://www.casillamia.uy"
  },
  {
    "nombre": "Consulta de giros",
    "url": "https://www.correo.com.uy/consulta-giros"
  },
  {
    "nombre": "Correo",
    "url": "https://www.correo.com.uy/home",
    "grupo": "CRITICO"
  },
  {
    "nombre": "ExportaFacil",
    "url": "https://www.exportafacil.com.uy/exportafacil"
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
  },
  {
    "nombre": "Geopostal",
    "url": "https://geo.correo.com.uy/geopostal/"
  },
  {
    "nombre": "Tienda Filatélica",
    "url": "https://www.stamps.uy.post/"
  },
  {
    "nombre": "Búsqueda de códigos postales",
    "url": "https://www.correo.com.uy/web/guest/codigospostales"
  },
  {
    "nombre": "Seguimiento de envíos",
    "url": "https://www.correo.com.uy/seguimientodeenvios"
  }
]
```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/monitor-status2/documentacion

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

    <link rel="stylesheet" href="css/monitor_base.css" />
    <link id="estilo-principal" rel="stylesheet" href="css/monitor_def.css" />
    <link rel="stylesheet" href="css/monitor_tarjetas.css" />
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

        <!-- NUEVO: Barra de controles compacta -->
        <div class="controls-bar">
          <div class="control-group">
            <label
              for="duracion-selector"
              class="control-label"
              >Duración del historial:</label
            >
            <select
              id="duracion-selector"
              class="control-select"
              aria-label="Seleccionar duración del historial"
            ></select>
          </div>

          <button
            id="btn-reiniciar"
            class="btn-reiniciar"
            onclick="reiniciarMonitoreo()"
            aria-label="Reiniciar monitoreo de servicios"
          >
            <span aria-hidden="true">🔄</span>
            <span id="texto-btn-reiniciar">Reiniciar Monitoreo</span>
          </button>

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
            title="Administrar sitios web monitoreados"
          >
            ⚙️ ABM Webs
          </a>
        </div>

        <!-- Aviso permanente sobre qué representa (y qué no) la medición 🌐 -->
        <div class="aviso-medicion" id="aviso-medicion" role="note">
          <span aria-hidden="true">ℹ️</span>
          <span>🌐 = medición externa, no tu experiencia real · 🖥️ = solo acceso interno.</span>
          <a href="leyenda.html" target="_blank" rel="noopener noreferrer">Ver más</a>
          <button
            id="btn-cerrar-aviso"
            class="btn-cerrar-aviso"
            aria-label="Cerrar aviso"
            title="Cerrar (no volver a mostrar)"
          >
            ✕
          </button>
        </div>

        <!-- NUEVO: Resumen superior (solo visible en vista tarjetas) -->
        <div id="resumen-superior" class="resumen-superior visible">
          <div class="contador ok" data-estado="ok">
            <span class="contador-numero" id="contador-ok">0</span>
            <span class="contador-label">🟢 OK</span>
          </div>
          <div class="contador lento" data-estado="lento">
            <span class="contador-numero" id="contador-lento">0</span>
            <span class="contador-label">🟡 Lentos</span>
          </div>
          <div class="contador critico" data-estado="critico">
            <span class="contador-numero" id="contador-critico">0</span>
            <span class="contador-label">🔴 Críticos</span>
          </div>
          <div class="contador caido" data-estado="caido">
            <span class="contador-numero" id="contador-caido">0</span>
            <span class="contador-label">⚫ Caídos</span>
          </div>
          <div class="contador bloqueo" data-estado="bloqueo" title="Responde por la red interna pero el proxy externo no pudo acceder: un usuario externo podría no poder entrar.">
            <span class="contador-numero" id="contador-bloqueo">0</span>
            <span class="contador-label">🟠 Bloqueo ext.</span>
          </div>
        </div>

        <div class="estado-linea" role="status" aria-live="polite">
          <span class="update-info" id="ultima-actualizacion"></span>
          <span class="separador-punto" aria-hidden="true">·</span>
          <span class="info-bar" id="info-bar-msg"></span>
        </div>

        <!-- Los filtros se insertarán aquí dinámicamente -->

        <!-- VISTA TABLA (original) -->
        <div class="table-scroll-container tarjetas-activas" id="tabla-container">
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

        <!-- NUEVO: VISTA TARJETAS -->
        <div id="grid-tarjetas" class="grid-tarjetas visible"></div>
      </div>
    </main>

    <!-- Contenedor de notificaciones (se crea dinámicamente) -->

    <script defer src="js/config.js"></script>
    <script defer src="js/i18n.js"></script>
    <script defer src="js/estado.js"></script>
    <script defer src="js/ui-textos.js"></script>
    <script defer src="js/monitoreo.js"></script>
    <script defer src="js/temas.js"></script>
    <script defer src="js/app.js"></script>
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
      descripcion,
      diagnostics
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
    // Priorizar la descripción específica enviada por la función (`descripcion`),
    // si existe. Si no, usar la descripción genérica del código (`desc`).
    if (descripcion) mensaje += `Descripción: ${descripcion}`;
    else if (desc) mensaje += `Descripción: ${desc}`;

    // Añadir detalles de diagnóstico (DNS, TCP, attempts) si están presentes
    try {
      if (diagnostics) {
        mensaje += `\n\nDiagnostics: ${JSON.stringify(diagnostics)}`;
      }
    } catch (e) {
      // ignore JSON errors
    }
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

### Archivo: `js/app.js`
```javascript
// app.js
// Orquestador de la app: la vista de tarjetas es la única vista disponible
// (con esqueletos de carga y el cargador lazy de render-tarjetas.js), pintar
// el historial ya guardado al recargar la página, y el arranque general
// (DOMContentLoaded). Se carga último porque su listener de arranque llama
// funciones definidas en el resto de los archivos (estado.js, ui-textos.js,
// monitoreo.js, temas.js).

// La app siempre muestra la vista de tarjetas (no hay alternancia con tabla).
const vistaActual = 'tarjetas';

function inicializarVista() {
  // Arrancamos la descarga del módulo de tarjetas ya mismo (en paralelo con
  // el resto del arranque), para que esté listo cuando lleguen los primeros
  // datos monitoreados.
  cargarModuloTarjetas().catch((e) =>
    console.error('No se pudo cargar el módulo de tarjetas:', e)
  );
  mostrarEsqueletosTarjetas();
}

// Muestra tarjetas "esqueleto" (placeholders con efecto de carga) apenas
// arranca la app, para entretener el tiempo entre que el usuario entra y
// llegan los datos reales. renderizarTarjetas() (en el módulo lazy) las
// reemplaza por las tarjetas reales y marca data-cargado="1".
function mostrarEsqueletosTarjetas() {
  const grid = document.getElementById('grid-tarjetas');
  if (!grid || grid.dataset.cargado === '1') return;

  const cantidad = websitesData.length > 0 ? websitesData.length : 6;
  grid.innerHTML = Array.from({ length: cantidad })
    .map(
      () => `
    <div class="tarjeta-skeleton">
      <div class="skeleton-linea skeleton-titulo"></div>
      <div class="skeleton-linea skeleton-numero"></div>
      <div class="skeleton-linea skeleton-corta"></div>
      <div class="skeleton-linea skeleton-corta"></div>
    </div>`
    )
    .join('');
}

// El render de la vista de tarjetas (clasificarEstadoDashboard, calcularTendencia,
// obtenerInfoEstado, truncarURL, renderizarTarjetas, crearTarjetaHTML) vive ahora
// en js/render-tarjetas.js, que se carga de forma diferida (lazy) apenas
// arranca la app. Sigue siendo un archivo aparte (en vez de sumarlo acá)
// para mantener este archivo corto y la responsabilidad separada.
let tarjetasModuloPromesa = null;

function cargarModuloTarjetas() {
  if (typeof window.renderizarTarjetas === 'function') {
    return Promise.resolve();
  }
  if (tarjetasModuloPromesa) {
    return tarjetasModuloPromesa;
  }
  tarjetasModuloPromesa = new Promise((resolve, reject) => {
    const script = document.createElement('script');
    script.src = 'js/render-tarjetas.js';
    script.onload = () => resolve();
    script.onerror = () => {
      tarjetasModuloPromesa = null;
      reject(new Error('No se pudo cargar js/render-tarjetas.js'));
    };
    document.head.appendChild(script);
  });
  return tarjetasModuloPromesa;
}

// Renderiza la vista de tarjetas, cargando el módulo bajo demanda la primera
// vez que hace falta.
function renderizarTarjetasSiNecesario() {
  if (typeof window.renderizarTarjetas === 'function') {
    window.renderizarTarjetas();
  } else {
    cargarModuloTarjetas()
      .then(() => window.renderizarTarjetas && window.renderizarTarjetas())
      .catch((e) => console.error('No se pudo cargar el módulo de tarjetas:', e));
  }
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

    // Borde izquierdo naranja SOLO si la última medición fue "bloqueo
    // externo" (proxy externo falló, pero respondió por red interna)
    if (ultimaMedicion && ultimaMedicion.source === 'direct' && ultimaMedicion.status === 200) {
      row.style.borderLeft = '4px solid #e65100';
      row.title = 'El proxy externo falló; responde solo por la red interna. Un usuario externo real podría no poder acceder.';
    }

    // Obtener tema actual y verificar si permite expansión (todos menos DEF y OSC)
    const params = new URLSearchParams(window.location.search);
    const temaActual = params.get('tema') || TEMA_DEFAULT;
    const permiteExpansion = !TEMAS_BASICOS.includes(temaActual);
    const errores = obtenerHistorialErrores(web.url);

    if (ultimaMedicion) {
      const estadoActual = obtenerEstadoVisual(
        ultimaMedicion.time,
        ultimaMedicion.status,
        ultimaMedicion.verifiedDirect
      );
      const { promedio, estadoPromedio, validCount } = calcularPromedio(
        web.url
      );

      maxValidCount = Math.max(maxValidCount, validCount);

      const cellLat = row.insertCell();
      cellLat.textContent = `${ultimaMedicion.time} ms ${ultimaMedicion.source === 'direct' ? '🖥️' : '🌐'}`;
      cellLat.title = ultimaMedicion.source === 'direct' 
        ? 'Medición por red interna (el proxy externo falló primero)' 
        : 'Medición vía proxy serverless (simula acceso externo)';

      const cellEstadoActual = row.insertCell();
      cellEstadoActual.textContent = estadoActual.text;
      cellEstadoActual.title = ultimaMedicion.source === 'direct' 
        ? 'Solo se confirmó acceso por red interna — no confirma que un usuario externo pueda entrar' 
        : 'Estado vía proxy serverless (la señal más parecida a un usuario externo)';
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

      const cellProm = row.insertCell();
      cellProm.textContent = `${promedio} ms${contadorErrores} ${ultimaMedicion.source === 'direct' ? '🖥️' : '🌐'}`;
      cellProm.title = ultimaMedicion.source === 'direct' 
        ? 'Promedio afectado por mediciones solo internas (posible bloqueo externo)' 
        : 'Promedio vía proxy serverless';

      const cellEstadoPromedio = row.insertCell();
      cellEstadoPromedio.textContent = estadoPromedio.text;
      cellEstadoPromedio.title = ultimaMedicion.source === 'direct' 
        ? 'Promedio afectado por mediciones solo internas (posible bloqueo externo)' 
        : 'Estado promedio vía proxy';
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

  // NUEVO: Renderizar tarjetas si está activa esa vista
  renderizarTarjetasSiNecesario();

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
// Muestra/oculta el aviso sobre 🌐/🖥️, recordando si el usuario ya lo cerró
// (en ese caso no vuelve a aparecer, ni siquiera en otra visita).
function inicializarAvisoMedicion() {
  const aviso = document.getElementById('aviso-medicion');
  const btnCerrar = document.getElementById('btn-cerrar-aviso');
  if (!aviso || !btnCerrar) return;

  if (localStorage.getItem('avisoMedicionCerrado') === '1') {
    aviso.style.display = 'none';
    return;
  }

  btnCerrar.addEventListener('click', () => {
    aviso.style.display = 'none';
    localStorage.setItem('avisoMedicionCerrado', '1');
  });
}

document.addEventListener('DOMContentLoaded', async () => {
  inicializarTema();
  cargarHistorial();
  configurarEnlaceLeyenda();
  inicializarVista();
  inicializarAvisoMedicion();

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

### Archivo: `js/config.js`
```javascript
// Umbrales de latencia (en milisegundos)
// Estos valores sirven para clasificar qué tan rápido o lento responde un servicio.
// Si querés saber más, mirá el archivo docs/justificacion_rangos_latencia.md
//
// IMPORTANTE: hay DOS escalas porque el proxy (Netlify) y la verificación
// directa (tu navegador) miden cosas distintas. El proxy corre en un
// datacenter (a veces en otro país/continente) y esa distancia agrega
// latencia real de red que NO indica que el sitio esté lento para tus
// usuarios. La verificación directa sí refleja tu experiencia real.

// Umbrales para verificación DIRECTA (navegador → red del usuario)
const UMBRALES_LATENCIA_DIRECTO = {
  MUY_RAPIDO: 300, // Excelente: responde casi al instante
  RAPIDO: 500, // Bueno: rápido, se nota poco el retraso
  NORMAL: 800, // Aceptable: algo lento pero usable
  LENTO: 1500, // Lento: se nota el retraso
  CRITICO: 3000, // Muy lento: ya es un problema serio
  RIESGO: 5000, // Al borde del fallo: casi no responde
  PENALIZACION_FALLO: 99999, // Valor especial para marcar fallos (no cuenta en el promedio)
};

// Umbrales para verificación vía PROXY (Netlify → internet público)
// Más permisivos: la propia documentación del proyecto ya estimaba que el
// proxy típicamente da 50-500ms más que una medición directa, así que estos
// umbrales están corridos hacia arriba para no marcar como "lento/crítico"
// algo que es simplemente distancia geográfica normal.
const UMBRALES_LATENCIA_PROXY = {
  MUY_RAPIDO: 600,
  RAPIDO: 1000,
  NORMAL: 1600,
  LENTO: 3000,
  CRITICO: 6000,
  RIESGO: 10000,
  PENALIZACION_FALLO: 99999,
};

// Se mantiene por compatibilidad con el resto del código que todavía
// referencia UMBRALES_LATENCIA directamente (p. ej. para PENALIZACION_FALLO,
// que es igual en ambas escalas).
const UMBRALES_LATENCIA = UMBRALES_LATENCIA_DIRECTO;

// -----------------------------
// 1) Temas y archivos CSS (UI)
// -----------------------------
// Nombres de temas disponibles en la UI
const TEMA_DEFAULT = 'def';
const TEMA_OSC = 'osc';

// Archivo CSS asociado a cada tema
// NOTA: los temas pro/pro2/min se eliminaron por completo (no se usaban en
// producción). Solo quedan def (claro) y osc (oscuro).
const TEMA_FILES = {
  [TEMA_DEFAULT]: 'css/monitor_def.css',
  [TEMA_OSC]: 'css/monitor_osc.css',
};

// Pares de temas para alternar (botón cambiar tema)
// Si un tema no tiene pareja, el botón de alternar se oculta
const TEMA_TOGGLE_PAIRS = {
  [TEMA_DEFAULT]: TEMA_OSC, // def (claro) → osc (oscuro)
  [TEMA_OSC]: TEMA_DEFAULT, // osc (oscuro) → def (claro)
};

// Temas que no muestran funciones avanzadas (ej. detalles de error)
const TEMAS_BASICOS = [TEMA_DEFAULT];

const DEFAULT_LEYENDA_TEMA = TEMA_DEFAULT;
const LEYENDA_TEMA_FILES = {
  default: 'css/leyenda_claro.css',
  def: 'css/leyenda_claro.css',
  osc: 'css/leyenda_oscuro.css',
};

const ABM_TEMA_FILES = {
  default: 'css/abm_claro.css',
  def: 'css/abm_claro.css',
  osc: 'css/abm_oscuro.css',
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

// -----------------------------
// 9) Acceso al panel de administración (abm-webs.html)
// -----------------------------
// Hash SHA-256 de la contraseña de acceso. Se valida tanto en abm-webs.html
// (cliente) como en netlify/functions/update-webs.js (servidor) contra ESTA
// misma constante, así que para cambiar la contraseña alcanza con
// actualizarla acá y calcular el nuevo hash — no hay que tocar los otros
// dos archivos.
const HASH_ACCESO_SHA256 =
  'f25b5391c1c2d9a1480a8e8c829fa6cd549b3ecd8c51a1f36ce5ef8ba624e65a';

// Este archivo se carga de dos formas distintas:
// - Como <script> normal en el navegador (index.html, leyenda.html, abm-webs.html)
// - Con require() desde una función serverless de Node (update-webs.js)
// El bloque de abajo solo se ejecuta en el segundo caso (en el navegador,
// "module" no existe, así que este "if" nunca corre ahí).
if (typeof module !== 'undefined' && module.exports) {
  module.exports = { HASH_ACCESO_SHA256 };
}

```

### Archivo: `js/estado.js`
```javascript
// estado.js
// Estado global de la app y capa de persistencia/cálculo sobre ese estado:
// historial en sessionStorage, promedios, duración de monitoreo seleccionada
// y la lógica de "fallo global" (detección de caídas masivas del sistema de
// monitoreo). No maneja DOM de tabla/tarjetas (eso vive en monitoreo.js).

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

function actualizarHistorial(url, time, status, source = 'proxy') {
  if (!historialStatus[url]) {
    historialStatus[url] = [];
  }

  // No agregar si ya alcanzamos el máximo configurado
  if (historialStatus[url].length >= maxHistorialActual) {
    return;
  }

  historialStatus[url].push({ time, status, source, timestamp: Date.now() });

  guardarHistorial();
}

// Calcula el promedio de latencia, pero solo cuenta los éxitos (status 200)
// Los fallos (99999ms) no afectan el promedio
function calcularPromedio(url) {
  const historial = historialStatus[url] || [];

  if (historial.length === 0) {
    return {
      promedio: 0,
      promedioProxy: null,
      promedioDirecto: null,
      estadoPromedio: obtenerEstadoVisual(0, 200),
      validCount: 0,
      historial: historial,
      fuentes: { proxy: 0, direct: 0 },
    };
  }

  // Separar mediciones por fuente
  const medicionesProxy = [];
  const medicionesDirectas = [];
  let fallos = 0;
  let ultimoCodigoError = 200;

  historial.forEach((entry) => {
    const esFallo =
      entry.status !== 200 ||
      entry.time >= UMBRALES_LATENCIA.PENALIZACION_FALLO;

    if (esFallo) {
      fallos++;
      ultimoCodigoError = entry.status;
    } else {
      if (entry.source === 'direct') {
        medicionesDirectas.push(entry.time);
      } else {
        medicionesProxy.push(entry.time);
      }
    }
  });

  const validCount = historial.length;
  const fuentes = {
    proxy: medicionesProxy.length,
    direct: medicionesDirectas.length,
  };

  // Calcular promedios por fuente
  const calcProm = (arr) => arr.length > 0 ? Math.round(arr.reduce((a,b) => a+b, 0) / arr.length) : null;
  const promedioProxy = calcProm(medicionesProxy);
  const promedioDirecto = calcProm(medicionesDirectas);

  // Promedio general (ponderado o solo el que exista)
  const promedioMs = promedioDirecto !== null && promedioProxy !== null
    ? Math.round((promedioDirecto + promedioProxy) / 2)
    : (promedioDirecto !== null ? promedioDirecto : promedioProxy);

  // Si más del 50% son fallos, mostrar como caída total
  if (fallos / validCount > 0.5 && validCount > 3) {
    return {
      promedio: 0,
      promedioProxy: promedioProxy,
      promedioDirecto: promedioDirecto,
      estadoPromedio: obtenerEstadoVisual(
        UMBRALES_LATENCIA.PENALIZACION_FALLO + 1,
        ultimoCodigoError
      ),
      validCount: validCount,
      historial: historial,
      fuentes: fuentes,
    };
  }

  // Si no hay mediciones exitosas (todas fallaron), mostrar como error
  if (promedioMs === null || promedioMs === undefined) {
    return {
      promedio: 0,
      promedioProxy: null,
      promedioDirecto: null,
      estadoPromedio: obtenerEstadoVisual(
        UMBRALES_LATENCIA.PENALIZACION_FALLO + 1,
        ultimoCodigoError
      ),
      validCount: validCount,
      historial: historial,
      fuentes: fuentes,
    };
  }

  return {
    promedio: promedioMs,
    promedioProxy: promedioProxy,
    promedioDirecto: promedioDirecto,
    estadoPromedio: obtenerEstadoVisual(promedioMs, 200),
    validCount: validCount,
    historial: historial,
    fuentes: fuentes,
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

    // Sección: explicación de los iconos de fuente (🌐 proxy / 🖥️ directo)
    if (leyendaData.iconos_title) {
      const iconosSection = document.createElement('div');
      iconosSection.className = 'leyenda-section iconos-fuente-section';
      iconosSection.style.marginTop = '30px';

      const h3Iconos = document.createElement('h3');
      h3Iconos.textContent = leyendaData.iconos_title;
      iconosSection.appendChild(h3Iconos);

      if (leyendaData.iconos_intro) {
        const pIntro = document.createElement('p');
        pIntro.textContent = leyendaData.iconos_intro;
        iconosSection.appendChild(pIntro);
      }

      const listaIconos = document.createElement('ul');
      listaIconos.className = 'leyenda-lista-iconos';
      (leyendaData.iconos || []).forEach((icono) => {
        const li = document.createElement('li');
        const strong = document.createElement('strong');
        strong.textContent = `${icono.emoji || ''} ${icono.label || ''}: `;
        li.appendChild(strong);
        li.appendChild(document.createTextNode(icono.desc || ''));
        listaIconos.appendChild(li);
      });
      iconosSection.appendChild(listaIconos);

      if (leyendaData.iconos_nota_borde) {
        const pBorde = document.createElement('p');
        pBorde.textContent = leyendaData.iconos_nota_borde;
        iconosSection.appendChild(pBorde);
      }

      if (leyendaData.iconos_nota_promedio) {
        const pProm = document.createElement('p');
        pProm.textContent = leyendaData.iconos_nota_promedio;
        iconosSection.appendChild(pProm);
      }

      container.appendChild(iconosSection);
    }

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

  if (temaDestino === TEMA_OSC) {
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

### Archivo: `js/monitoreo.js`
```javascript
// monitoreo.js
// El corazón de la app: pedir el estado real de cada sitio (proxy + fallback
// directo desde el navegador), dibujar/actualizar la tabla, y manejar el
// detalle expandible de errores (tanto en la tabla como en la vista de
// tarjetas).

/**
 * Llama al proxy de Netlify para saber el estado y la latencia de una URL.
 */
async function verificarEstado(url) {
  // =======================================================
  // PASO 1: Intentar via proxy (Netlify Function)
  // =======================================================
  try {
    const response = await fetch(
      `${PROXY_ENDPOINT}?url=${encodeURIComponent(url)}`
    );

    if (!response.ok) {
      console.warn(`Proxy error HTTP ${response.status} para ${url}`);
      // Proxy falló, intentar verificación directa
      return await verificarDirecto(url);
    }

    const data = await response.json();

    // Si el DNS falló, el dominio realmente no existe/no resuelve.
    // Esto NO es algo que un WAF pueda "fingir": la resolución DNS es
    // pública e igual para todo el mundo. NO tiene sentido pasar a
    // verificación directa acá, porque el truco del <img> del navegador
    // no puede distinguir "DNS inexistente" de "404 normal" (ambos fallan
    // rápido) y terminaría marcando como "arriba" un sitio que no existe.
    if (data.errorType === 'DNS_ERROR') {
      console.log(`DNS inexistente para ${url}, se marca como caído sin verificación directa.`);
      return data;
    }

    // Si el proxy dice que el sitio está caído (status 0) por otro motivo
    // (bloqueo, timeout, conexión rechazada), sí verificamos directamente
    // porque el proxy puede estar bloqueado por el WAF
    if (data.status === 0 || data.status === ESTADO_ERROR_CONEXION) {
      console.log(`Proxy reporta caído para ${url}, verificando directamente...`);
      return await verificarDirecto(url);
    }

    return data;

  } catch (error) {
    console.warn(`Proxy no disponible para ${url}:`, error.message);
    return await verificarDirecto(url);
  }
}

/**
 * Verificación directa desde el navegador usando una imagen.
 * El navegador del usuario tiene IP "normal" no bloqueada por WAF.
 * No hay restricciones CORS para cargar imágenes.
 */
async function verificarDirecto(url) {
  return new Promise((resolve) => {
    const startTime = performance.now();
    const img = new Image();
    let resolved = false;

    // Timeout de 10 segundos para la imagen
    const timeout = setTimeout(() => {
      if (!resolved) {
        resolved = true;
        img.onload = img.onerror = null;
        // La imagen no cargó en 10s → probablemente caído
        resolve({
          time: UMBRALES_LATENCIA.PENALIZACION_FALLO,
          status: 0,
          error: 'Sin respuesta (verificación directa)',
          verifiedDirect: true,
        });
      }
    }, 10000);

    // La imagen cargó (aunque sea error 404) → el servidor responde
    img.onload = function() {
      if (resolved) return;
      resolved = true;
      clearTimeout(timeout);
      const time = Math.round(performance.now() - startTime);
      console.log(`✅ Verificación directa OK para ${url}: ${time}ms`);
      resolve({
        time: time,
        status: 200,
        verifiedDirect: true,
      });
    };

    // Error al cargar la imagen → puede ser 404 (favicon no existe) pero servidor responde
    // o puede ser que realmente no hay conexión
    img.onerror = function() {
      if (resolved) return;
      resolved = true;
      clearTimeout(timeout);
      const time = Math.round(performance.now() - startTime);

      // Si tardó menos de 8 segundos, probablemente es 404 (favicon no existe)
      // pero el servidor respondió → el sitio funciona
      if (time < 8000) {
        console.log(`✅ Verificación directa OK (404 favicon) para ${url}: ${time}ms`);
        resolve({
          time: time,
          status: 200,
          verifiedDirect: true,
        });
      } else {
        // Tardó mucho → probablemente timeout real
        console.log(`❌ Verificación directa falló para ${url}: timeout`);
        resolve({
          time: UMBRALES_LATENCIA.PENALIZACION_FALLO,
          status: 0,
          error: 'Timeout en verificación directa',
          verifiedDirect: true,
        });
      }
    };

    // Usamos favicon.ico con timestamp para evitar cache
    const faviconUrl = new URL('/favicon.ico', url).href + '?_t=' + Date.now();
    img.src = faviconUrl;
  });
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
  // NUEVO: en vista de tarjetas, el detalle debe insertarse junto a la
  // tarjeta correspondiente (la tabla está oculta y no sirve como destino).
  if (typeof vistaActual !== 'undefined' && vistaActual === 'tarjetas') {
    toggleErroresDetalleTarjeta(url);
    return;
  }

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

/**
 * Muestra u oculta el detalle de errores para la VISTA DE TARJETAS.
 * Inserta el bloque de detalle como elemento propio del grid (spanning
 * todo el ancho), justo después de la tarjeta correspondiente.
 */
function toggleErroresDetalleTarjeta(url) {
  const grid = document.getElementById('grid-tarjetas');
  if (!grid) return;

  const card = grid.querySelector(
    `.tarjeta-servicio[data-url="${CSS.escape(url)}"]`
  );
  if (!card) return;

  // Si ya existe el detalle abierto para esta tarjeta, colapsar y quitar
  const detalleExistente = grid.querySelector(
    `.error-detail-card[data-parent-url="${CSS.escape(url)}"]`
  );
  if (detalleExistente) {
    detalleExistente.classList.remove('expanded');
    setTimeout(() => {
      if (detalleExistente && detalleExistente.parentNode) {
        detalleExistente.remove();
      }
    }, 200);
    return;
  }

  const errores = obtenerHistorialErrores(url);
  if (errores.length === 0) return;

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

  const detalleDiv = document.createElement('div');
  detalleDiv.className = 'error-detail-card';
  detalleDiv.setAttribute('data-parent-url', url);
  detalleDiv.innerHTML = html;

  // Insertar justo después de la tarjeta correspondiente
  card.insertAdjacentElement('afterend', detalleDiv);

  // Trigger animación
  setTimeout(() => detalleDiv.classList.add('expanded'), 10);
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
  return mensajes[codigo] || obtenerDescripcionPorRango(codigo);
}

/**
 * Actualiza una fila de la tabla con los datos reales.
 */
function actualizarFila(web, resultado) {
  const tbody = document.getElementById('status-table-body');
  // Escapar caracteres especiales en la URL para la selección del atributo data-url
  const row = tbody.querySelector(`tr[data-url="${CSS.escape(web.url)}"]`);

  if (!row) return;

  // Borde izquierdo naranja SOLO para el caso "bloqueo externo" (proxy
  // externo falló pero la red interna sí responde) — es el caso que más
  // nos importa destacar, para que no se confunda con un "todo bien".
  // Si también falló la verificación directa, es una caída total real:
  // ya queda claro con los colores normales de "caído", sin borde extra.
  if (resultado.verifiedDirect && resultado.status === 200) {
    row.style.borderLeft = '4px solid #e65100';
    row.title = 'El proxy externo falló; responde solo por la red interna. Un usuario externo real podría no poder acceder.';
  } else {
    row.style.borderLeft = '';
    row.title = '';
  }

  // --- Lógica de cálculo y estado ---
  const estadoActual = obtenerEstadoVisual(resultado.time, resultado.status, resultado.verifiedDirect);
  // Nota: calcularPromedio() obtiene los datos del historial que ACABA de ser actualizado
  const { promedio, promedioProxy, promedioDirecto, estadoPromedio, fuentes } = calcularPromedio(web.url);

  // IMPORTANTE: resultado.verifiedDirect significa "se intentó una verificación
  // directa", NO "la verificación directa tuvo éxito". Si el proxy externo
  // falla y la verificación directa TAMBIÉN falla (status sigue en 0), es una
  // caída total real — hay que alertar igual. Solo se considera "acceso
  // solo interno" cuando la verificación directa realmente respondió 200.
  const exitoSoloInterno = resultado.verifiedDirect && resultado.status === 200;

  // ALERTA: Solo alertar como "caída real" si NI el proxy NI (si se intentó)
  // la verificación directa lograron un 200.
  const sitioRealmenteCaido = resultado &&
    (resultado.status === 0 || resultado.status >= 400) &&
    !exitoSoloInterno;

  if (sitioRealmenteCaido) {
    window.registrarErrorSitio &&
      window.registrarErrorSitio(
        web.nombre || web.url,
        web.url,
        resultado.time,
        resultado.status,
        resultado.error || '',
        resultado.diagnostics || resultado.attempts || null
      );
  } else if (exitoSoloInterno) {
    // Responde por la red interna, pero el proxy externo (la ruta que más
    // se parece a un usuario común) había fallado antes. Esto NO es "todo
    // bien" — un usuario externo real podría no poder entrar. Se registra
    // como su propia alerta en vez de limpiar las anteriores.
    window.registrarErrorSitio &&
      window.registrarErrorSitio(
        web.nombre || web.url,
        web.url,
        resultado.time,
        resultado.status,
        'Posible bloqueo externo: el sitio respondió por la red interna, pero el proxy externo no pudo acceder. Un usuario externo real podría no poder entrar.',
        null
      );
  } else if (resultado && resultado.status === 200 && !resultado.verifiedDirect) {
    // Éxito genuino por la ruta externa (proxy): ahí sí limpiar alertas previas.
    window.limpiarErrorSitio && window.limpiarErrorSitio(web.nombre || web.url);
  }

  // --- Actualización de celdas (Columnas 3 a 7) ---

  // Columna 3: Latencia Actual (índice 2)
  row.cells[2].textContent = `${resultado.time} ms ${resultado.verifiedDirect ? '🖥️' : '🌐'}`;
  row.cells[2].title = resultado.verifiedDirect 
    ? 'Medición por red interna (el proxy externo falló primero)' 
    : 'Medición vía proxy serverless (simula acceso externo)';

  // Columna 4: Estado Actual (índice 3)
  row.cells[3].textContent = estadoActual.text;
  row.cells[3].title = resultado.verifiedDirect 
    ? 'Solo se confirmó acceso por red interna — no confirma que un usuario externo pueda entrar' 
    : 'Estado vía proxy serverless (la señal más parecida a un usuario externo)';
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
  // Mostrar promedios separados por fuente si hay mediciones mixtas
  let textoPromedio = '';
  let tooltipPromedio = '';

  if (promedioProxy !== null && promedioDirecto !== null) {
    // Hay mediciones mixtas: mostrar ambos promedios
    textoPromedio = `${promedioProxy} ms 🌐 / ${promedioDirecto} ms 🖥️`;
    tooltipPromedio = `Promedio proxy: ${promedioProxy}ms (${fuentes.proxy} mediciones) | Promedio directo: ${promedioDirecto}ms (${fuentes.direct} mediciones)`;
  } else if (promedioDirecto !== null) {
    // Solo mediciones directas
    textoPromedio = `${promedioDirecto} ms 🖥️`;
    tooltipPromedio = `Promedio directo: ${promedioDirecto}ms (${fuentes.direct} mediciones)`;
  } else if (promedioProxy !== null) {
    // Solo mediciones proxy
    textoPromedio = `${promedioProxy} ms 🌐`;
    tooltipPromedio = `Promedio proxy: ${promedioProxy}ms (${fuentes.proxy} mediciones)`;
  } else {
    // Sin mediciones exitosas
    textoPromedio = '0 ms';
    tooltipPromedio = 'Sin mediciones exitosas';
  }

  // Agregar contador de errores si existen y el tema lo permite
  const totalMediciones = (historialStatus[web.url] || []).length;
  const contadorErrores =
    errores.length > 0 && permiteExpansion
      ? ` ⚠️ ${errores.length}/${totalMediciones}`
      : '';
  row.cells[4].textContent = textoPromedio + contadorErrores;
  row.cells[4].title = tooltipPromedio;

  // Columna 6: Estado Promedio (índice 5)
  row.cells[5].textContent = estadoPromedio.text;
  row.cells[5].title = resultado.verifiedDirect 
    ? 'Promedio afectado por mediciones solo internas (posible bloqueo externo)' 
    : 'Estado promedio vía proxy';
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

  // Usamos Promise.allSettled para que si un servicio falla, no corte el monitoreo de los otros
  const promesas = websitesData.map((web) => verificarEstado(web.url));
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
        proxyError: true,
      };
    }

    // Agregar al array para el análisis global
    resultadosMonitoreo.push({
      url: web.url,
      time: res.time,
      status: res.status,
      verifiedDirect: res.verifiedDirect || false,
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

    // 4.1. Guardar el historial (con fuente: proxy o direct)
    actualizarHistorial(res.url, res.time, res.status, res.verifiedDirect ? 'direct' : 'proxy');

    // 4.2. Actualizar la fila en la pantalla
    actualizarFila(web, res);

    // 4.3. Recalcular el contador para el encabezado
    const { validCount } = calcularPromedio(res.url);
    maxValidCount = Math.max(maxValidCount, validCount);
  });

  // 5. Terminar y programar el próximo monitoreo
  actualizarEncabezadoPromedio(maxValidCount);
  actualizarUltimaActualizacion(new Date());

  // NUEVO: Si la vista de tarjetas está activa, actualizarla también
  renderizarTarjetasSiNecesario();

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

```

### Archivo: `js/render-tarjetas.js`
```javascript
// render-tarjetas.js
// Módulo de carga diferida (lazy) para la vista de "Tarjetas" del monitor.
// Se carga dinámicamente desde script.js (ver cargarModuloTarjetas) recién
// cuando el usuario abre esa vista por primera vez, para no sumar peso al
// arranque inicial de la página si nunca la usa.
//
// Comparte el scope global de script.js/config.js (ambos <script> clásicos
// cargados antes que este), por eso puede leer directamente variables como
// websitesData, historialStatus, maxHistorialActual, calcularPromedio,
// obtenerHistorialErrores, UMBRALES_LATENCIA_*, TEMA_DEFAULT y TEMAS_BASICOS
// sin necesidad de pasarlas como parámetros.
(function () {
  function clasificarEstadoDashboard(tiempo, status, esDirecto = false) {
    if (status === 0 || status === 599) return 'caido';
    if (status === 408) return 'critico';

    // Si esta medición llegó por la ruta directa/interna (es decir, el
    // proxy externo ya había fallado antes) y respondió 200: NO es un
    // éxito común. Significa que el sitio anda por dentro pero podría
    // estar inaccesible para alguien de afuera — se marca aparte para
    // que no quede tapado como si estuviera todo bien.
    if (esDirecto && status === 200) return 'bloqueo';

    const t = parseFloat(tiempo);

    const fallback = { MUY_RAPIDO: 300, RAPIDO: 500, NORMAL: 800, LENTO: 1500, CRITICO: 3000, RIESGO: 5000 };
    const fallbackProxy = { MUY_RAPIDO: 600, RAPIDO: 1000, NORMAL: 1600, LENTO: 3000, CRITICO: 6000, RIESGO: 10000 };

    const umbrales = esDirecto
      ? ((typeof UMBRALES_LATENCIA_DIRECTO !== 'undefined') ? UMBRALES_LATENCIA_DIRECTO : fallback)
      : ((typeof UMBRALES_LATENCIA_PROXY !== 'undefined') ? UMBRALES_LATENCIA_PROXY : fallbackProxy);

    // Códigos de error HTTP (4xx/5xx, excluyendo 408 y 599 ya manejados arriba):
    // el servicio respondió pero con error. Antes esto se mostraba SIEMPRE como
    // "lento" sin importar cuánto tardó. Ahora, si además tardó mucho, se
    // escala la severidad en vez de ocultarla detrás de un simple "lento".
    if (status >= 400 && status < 600) {
      if (!isNaN(t) && t > umbrales.RIESGO) return 'caido';
      if (!isNaN(t) && t > umbrales.CRITICO) return 'critico';
      return 'lento';
    }

    if (isNaN(t) || t <= 0) return 'caido';

    if (t <= umbrales.MUY_RAPIDO) return 'ok';
    if (t <= umbrales.RAPIDO) return 'ok';
    if (t <= umbrales.NORMAL) return 'ok';
    if (t <= umbrales.LENTO) return 'lento';
    if (t <= umbrales.CRITICO) return 'critico';
    if (t <= umbrales.RIESGO) return 'critico';
    return 'caido';
  }

  function calcularTendencia(historial) {
    if (!historial || historial.length < 2) {
      return { flechas: '─', tooltip: 'Sin datos suficientes' };
    }

    const ultimas = historial.slice(-3);
    const tiempos = ultimas.map(h => h.time).filter(t => t > 0 && t < 99999);

    if (tiempos.length < 2) {
      return { flechas: '─', tooltip: 'Datos insuficientes' };
    }

    const primera = tiempos[0];
    const ultima = tiempos[tiempos.length - 1];
    const diff = ultima - primera;
    const porcentaje = primera > 0 ? Math.round((diff / primera) * 100) : 0;

    if (diff > 100) return { flechas: '▲▲▲', tooltip: `Empeorando +${porcentaje}%` };
    if (diff > 50) return { flechas: '▲▲', tooltip: `Empeorando +${porcentaje}%` };
    if (diff > 10) return { flechas: '▲', tooltip: `Empeorando +${porcentaje}%` };
    if (diff < -100) return { flechas: '▼▼▼', tooltip: `Mejorando ${porcentaje}%` };
    if (diff < -50) return { flechas: '▼▼', tooltip: `Mejorando ${porcentaje}%` };
    if (diff < -10) return { flechas: '▼', tooltip: `Mejorando ${porcentaje}%` };
    return { flechas: '─', tooltip: 'Estable' };
  }

  function obtenerInfoEstado(estado) {
    const mapa = {
      ok: { icono: '🟢', texto: 'OK', color: '#4caf50' },
      lento: { icono: '🟡', texto: 'LENTO', color: '#ff9800' },
      critico: { icono: '🔴', texto: 'CRÍTICO', color: '#f44336' },
      caido: { icono: '⚫', texto: 'CAÍDO', color: '#9e9e9e' },
      bloqueo: { icono: '🟠', texto: 'BLOQUEO EXTERNO', color: '#e65100' },
    };
    return mapa[estado] || mapa.ok;
  }

  function truncarURL(url, maxLen) {
    if (url.length <= maxLen) return url;
    return url.substring(0, maxLen - 3) + '...';
  }

  function renderizarTarjetas() {
    const grid = document.getElementById('grid-tarjetas');
    if (!grid) return;

    const contadores = { ok: 0, lento: 0, critico: 0, caido: 0, bloqueo: 0 };

    const tarjetas = websitesData.map((web, index) => {
      const historial = historialStatus[web.url] || [];
      const ultima = historial[historial.length - 1];
      const estado = ultima ? clasificarEstadoDashboard(ultima.time, ultima.status, ultima.source === 'direct') : 'caido';
      contadores[estado]++;

      return crearTarjetaHTML(web, ultima, estado, historial, index);
    }).join('');

    grid.innerHTML = tarjetas;
    grid.dataset.cargado = '1';

    // Actualizar contadores superiores
    document.getElementById('contador-ok').textContent = contadores.ok;
    document.getElementById('contador-lento').textContent = contadores.lento;
    document.getElementById('contador-critico').textContent = contadores.critico;
    document.getElementById('contador-caido').textContent = contadores.caido;
    const contadorBloqueo = document.getElementById('contador-bloqueo');
    if (contadorBloqueo) contadorBloqueo.textContent = contadores.bloqueo;
  }

  function crearTarjetaHTML(web, ultima, estado, historial, index = 0) {
    const tiempo = ultima ? ultima.time : '--';
    const esDirecto = ultima && ultima.source === 'direct';
    const tendencia = calcularTendencia(historial);
    const estadoInfo = obtenerInfoEstado(estado);

    // Obtener promedio
    const { promedio, estadoPromedio, validCount } = calcularPromedio(web.url);
    const promedioTexto = validCount > 0 ? `${promedio}ms [${validCount}/${maxHistorialActual}]` : 'Sin datos';

    // Obtener errores
    const errores = obtenerHistorialErrores(web.url);
    const totalMediciones = historial.length;
    const erroresHTML = errores.length > 0
      ? `<span class="tarjeta-errores" onclick="toggleErroresDetalle('${web.url}')" title="Ver errores">⚠️${errores.length}/${totalMediciones}</span>`
      : '';

    // Botón PSI (solo en temas avanzados)
    const params = new URLSearchParams(window.location.search);
    const temaActual = params.get('tema') || TEMA_DEFAULT;
    const permiteExpansion = !TEMAS_BASICOS.includes(temaActual);

    const fuenteIcono = esDirecto ? '🖥️' : '🌐';
    const fuenteTitle = esDirecto
      ? 'Red interna (el proxy externo falló primero) — no confirma acceso externo'
      : 'Proxy externo (simula acceso de un usuario externo)';

    // Animación de entrada escalonada: cada tarjeta aparece un poco después
    // que la anterior (tope de 600ms para que no se demore con muchos sitios)
    const animationDelay = Math.min(index * 45, 600);

    return `
    <div class="tarjeta-servicio estado-${estado} ${esDirecto ? 'directo' : ''}" data-url="${web.url}" style="animation-delay: ${animationDelay}ms">
      <div class="tarjeta-header-compacto">
        <span class="tarjeta-nombre-compacto">${web.nombre}</span>
        <span class="tarjeta-fuente-compacto" title="${fuenteTitle}">${fuenteIcono}</span>
      </div>
      <div class="tarjeta-body-compacto">
        <div class="tarjeta-latencia-compacto">
          <span class="latencia-numero">${tiempo}</span><span class="latencia-unidad">ms</span>
          <span class="latencia-fuente" title="${fuenteTitle}">${fuenteIcono}</span>
        </div>
        <div class="tarjeta-fila-datos">
          <span class="dato-promedio" title="Promedio">Ø ${promedioTexto}</span>
          <span class="dato-estado-prom" style="color:${estadoPromedio.className ? '' : '#666'}">${estadoPromedio.text}</span>
        </div>
        <div class="tarjeta-fila-datos">
          <span class="tendencia" title="${tendencia.tooltip}">${tendencia.flechas}</span>
          <span class="estado-actual" style="color:${estadoInfo.color}">${estadoInfo.icono} ${estadoInfo.texto}</span>
          ${erroresHTML}
        </div>
      </div>
      <div class="tarjeta-footer-compacto">
        <a href="${web.url}" target="_blank" rel="noopener" title="${web.url}">${truncarURL(web.url, 35)}</a>
        ${permiteExpansion ? `<button class="psi-button psi-compacto" onclick="window.open('https://pagespeed.web.dev/report?url=${web.url}', '_blank')" title="PageSpeed Insights">PSI</button>` : ''}
      </div>
    </div>
  `;
  }

  // Único punto de entrada expuesto: script.js lo llama vía window.renderizarTarjetas()
  // una vez que confirma que este archivo ya se cargó.
  window.renderizarTarjetas = renderizarTarjetas;
})();

```

### Archivo: `js/temas.js`
```javascript
// temas.js
// Todo lo relacionado a la selección/alternancia de tema (claro/oscuro),
// la visibilidad de elementos que dependen del tema activo (enlace ABM,
// columna de acción de la tabla) y el botón de reiniciar monitoreo.

/**
 * Obtiene el tema de los parámetros de la URL.
 * @returns {string | null} El nombre del tema o null.
 */
function obtenerTemaDeURL() {
  const params = new URLSearchParams(window.location.search);
  const tema = params.get('tema');
  if (tema === TEMA_DEFAULT) return TEMA_DEFAULT;
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
  if (temaDestino === TEMA_OSC) {
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
 * Muestra el enlace ABM (visible en todos los temas)
 */
function actualizarVisibilidadABM() {
  const enlaceABM = document.getElementById('enlace-abm');
  if (!enlaceABM) return;
  enlaceABM.style.display = 'inline-flex';
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

```

### Archivo: `js/ui-textos.js`
```javascript
// ui-textos.js
// Todo lo relacionado a idioma (i18n), textos/etiquetas del DOM, y el
// formateo de "estado visual" (velocidad/color según latencia y código
// HTTP). No toca la tabla ni el monitoreo en sí (eso vive en monitoreo.js).

function configurarEnlaceLeyenda() {
  const enlaceLeyenda = document.getElementById('enlace-leyenda');
  if (enlaceLeyenda) {
    enlaceLeyenda.href = `leyenda.html${window.location.search}`;
  }
}

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

/**
 * Devuelve una descripción legible para un código de estado HTTP.
 * Primero busca una etiqueta explícita en el diccionario de idioma
 * (lang/i18n_es.js / i18n_en.js). Si el código no está mapeado ahí
 * (por ejemplo 520-530, que son extensiones propias de Cloudflare y
 * no códigos HTTP oficiales), clasifica por RANGO en vez de exigir
 * que cada código nuevo se agregue a mano.
 */
function obtenerDescripcionPorRango(codigo) {
  // Rango 520-530: extensiones no estándar usadas por Cloudflare (y CDNs
  // similares) para indicar que el proxy/CDN funcionó, pero el servidor
  // de origen detrás no respondió correctamente o a tiempo.
  if (codigo >= 520 && codigo <= 530) {
    return 'Error de origen vía CDN/Proxy (Cloudflare u similar) — el servidor real no respondió';
  }
  if (codigo >= 500 && codigo < 600) {
    return 'Error del servidor';
  }
  if (codigo >= 400 && codigo < 500) {
    return 'Error del cliente';
  }
  if (codigo >= 300 && codigo < 400) {
    return 'Redireccionamiento';
  }
  return window.TEXTOS_ACTUAL?.httpStatus?.GENERIC || 'Error HTTP';
}

function obtenerDescripcionEstadoHttp(codigo) {
  return (
    window.TEXTOS_ACTUAL?.httpStatus?.[codigo] ||
    obtenerDescripcionPorRango(codigo)
  );
}

function obtenerEstadoVisual(tiempo, estado = 200, esVerificadoDirecto = false) {
  const tiempoNum = parseFloat(tiempo);

  // Si fue verificado directamente y el proxy decía "caído", pero la
  // verificación directa confirmó que funciona (status 200): esto YA NO
  // significa "todo bien". Significa que el sitio respondió por la ruta
  // interna/directa pero no por la ruta externa (proxy) — es decir, un
  // usuario externo (el caso que más nos importa) podría no poder entrar,
  // aunque desde acá adentro sí funcione. Se marca como advertencia
  // distinta en vez de mostrarlo como si fuera un éxito normal.
  if (esVerificadoDirecto && estado === 200) {
    return {
      text: window.TEXTOS_ACTUAL.estados.BLOQUEO_EXTERNO,
      className: 'status-bloqueo-externo',
    };
  }

  if (estado !== 200 || tiempoNum >= UMBRALES_LATENCIA.PENALIZACION_FALLO) {
    const descripcionEstado = obtenerDescripcionEstadoHttp(estado);

    const textoFallo =
      estado !== 200
        ? `${window.TEXTOS_ACTUAL.estados.DOWN_ERROR} (${estado} - ${descripcionEstado})`
        : window.TEXTOS_ACTUAL.estados.DOWN_ERROR;

    return {
      text: textoFallo,
      className: 'status-down',
    };
  }

  // A esta altura la medición viene del proxy (si fuera directa y status 200,
  // ya se resolvió más arriba). Se usa la escala PROXY, más permisiva, porque
  // incluye la latencia de red real entre el datacenter de Netlify y el sitio.
  const estadosVelocidad = [
    {
      umbral: UMBRALES_LATENCIA_PROXY.MUY_RAPIDO,
      text: window.TEXTOS_ACTUAL.velocidad.VERY_FAST,
      className: 'status-very-fast',
    },
    {
      umbral: UMBRALES_LATENCIA_PROXY.RAPIDO,
      text: window.TEXTOS_ACTUAL.velocidad.FAST,
      className: 'status-fast',
    },
    {
      umbral: UMBRALES_LATENCIA_PROXY.NORMAL,
      text: window.TEXTOS_ACTUAL.velocidad.NORMAL,
      className: 'status-normal',
    },
    {
      umbral: UMBRALES_LATENCIA_PROXY.LENTO,
      text: window.TEXTOS_ACTUAL.velocidad.SLOW,
      className: 'status-slow',
    },
    {
      umbral: UMBRALES_LATENCIA_PROXY.CRITICO,
      text: window.TEXTOS_ACTUAL.velocidad.CRITICAL,
      className: 'status-critical',
    },
    {
      umbral: UMBRALES_LATENCIA_PROXY.RIESGO,
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
    INFO_BAR: 'updates every 5 min',
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
    BLOQUEO_EXTERNO: 'POSSIBLE EXTERNAL BLOCK',
  },
  httpCodes: [
    {
      code: 0,
      label: 'No connection',
      description:
        'Could not connect to the server. Can be a non-existent DNS (the domain does not exist, marked down immediately) or a timeout/refused connection (in these cases the system also verifies directly from your browser before confirming the outage).',
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
      description:
        'Access denied, even with valid authentication. In this monitor it usually means a WAF/firewall blocked the proxy (because it comes from a cloud server) — the site may be working fine for real users. The system verifies directly from your browser (🖥️) to confirm.',
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
      description:
        'The site responded, but the proxy took more than 25 seconds to get a response (SLOW_RESPONSE). Not an outage: the service works but extremely slowly.',
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
      description:
        'The service rate limit was exceeded. Like 403, this can be a WAF throttling the proxy — the site is not necessarily down for real users.',
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
    'Colors and symbols reflect the measured response time (latency). The justification is based on Interaction Psychology and the Operational Meaning of performance. IMPORTANT: the system measures in two different ways (\ud83c\udf10 proxy and \ud83d\udda5\ufe0f direct), and each one uses its own threshold scale because they are not directly comparable — see the "What do the \ud83c\udf10 and \ud83d\udda5\ufe0f icons mean?" section below.',
  umbrales: [
    {
      key: 'very_fast',
      className: 'status-very-fast',
      emoji: '🚀',
      label: 'VERY FAST',
      range_text: '\ud83d\udda5\ufe0f Direct: < 300 ms  \u00b7  \ud83c\udf10 Proxy: < 600 ms',
      summary: 'Optimal Performance (Instantaneous to the User)',
      details:
        "Golden Standard. The human brain perceives responses under 100 ms as instant (Nielsen's guideline). Keeping the threshold up to 300 ms ensures a smooth experience. Operational Meaning: The system is operating in optimal conditions.",
    },
    {
      key: 'fast',
      className: 'status-fast',
      emoji: '⭐',
      label: 'FAST',
      range_text: '\ud83d\udda5\ufe0f Direct: 300\u2013500 ms  \u00b7  \ud83c\udf10 Proxy: 600\u20131000 ms',
      summary: 'Fluid Interaction without Noticeable Wait',
      details:
        'Limit of Unconscious Perception. The delay is noticeable but not perceived as a bothersome wait. Operational Meaning: Excellent performance; good control point for fast backend processes.',
    },
    {
      key: 'normal',
      className: 'status-normal',
      emoji: '✅',
      label: 'NORMAL',
      range_text: '\ud83d\udda5\ufe0f Direct: 500\u2013800 ms  \u00b7  \ud83c\udf10 Proxy: 1000\u20131600 ms',
      summary: 'Acceptable Performance (User Maintains Focus)',
      details:
        'Begin of Distraction. From 500 ms the user may start to divert attention, although they can maintain their thought flow. Operational Meaning: Acceptable performance but approaching where wait sensation consolidates.',
    },
    {
      key: 'slow',
      className: 'status-slow',
      emoji: '⚠️',
      label: 'SLOW',
      range_text: '\ud83d\udda5\ufe0f Direct: 800\u20131500 ms  \u00b7  \ud83c\udf10 Proxy: 1600\u20133000 ms',
      summary: 'Annoying Delay (Active Distractor / Early Alert)',
      details:
        '1 second limit. The delay becomes an active distractor. Experience is noticeably degraded. Operational Meaning: Early Alert. Server or network under stress; investigate.',
    },
    {
      key: 'critical',
      className: 'status-critical',
      emoji: '🐌',
      label: 'CRITICAL',
      range_text: '\ud83d\udda5\ufe0f Direct: 1500\u20133000 ms  \u00b7  \ud83c\udf10 Proxy: 3000\u20136000 ms',
      summary: 'Abandonment Risk (3 Seconds / Imminent Failure)',
      details:
        'Loss of Focus and Frustration. The critical boundary (3 seconds) where users may abandon pages. Operational Meaning: Imminent Failure. Heavy load or severe bottlenecks.',
    },
    {
      key: 'risk',
      className: 'status-risk',
      emoji: '🚨',
      label: 'RISK',
      range_text: '\ud83d\udda5\ufe0f Direct: 3000\u20135000 ms  \u00b7  \ud83c\udf10 Proxy: 6000\u201310000 ms',
      summary: 'Functional Failure and Collapse (5 Seconds / Alarm)',
      details:
        'Functional Failure. Delays longer than 5 seconds are often considered a functional failure. Operational Meaning: ALARM. Service near collapse or unreliable request handling.',
    },
    {
      key: 'extreme_risk',
      className: 'status-extreme-risk',
      emoji: '🔥',
      label: 'EXTREME RISK',
      range_text: '\ud83d\udda5\ufe0f Direct: 5000\u201399999 ms  \u00b7  \ud83c\udf10 Proxy: 10000\u201399999 ms',
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
  iconos_title: 'What do the 🌐 and 🖥️ icons mean?',
  iconos_intro:
    'This monitor checks each site in two possible ways. That is why there are two different threshold scales (see table above): comparing a 🌐 time against the 🖥️ thresholds (or vice versa) gives a wrong reading.',
  iconos: [
    {
      emoji: '🌐',
      label: 'Proxy (public internet)',
      desc: 'Measurement taken from a Netlify server. This is the default measurement. Since it travels over the internet (sometimes across countries/continents), it naturally has more latency — that is why its threshold scale is more lenient.',
    },
    {
      emoji: '🖥️',
      label: 'Direct (your browser)',
      desc: 'Measurement taken from your own browser, usually when the proxy reported an outage (to rule out a WAF blocking the proxy rather than a real outage). Reflects your real experience, hence the stricter scale.',
    },
  ],
  iconos_nota_borde:
    'A blue left border on a row indicates that the last measurement for that site was direct (🖥️).',
  iconos_nota_promedio:
    'If a site has mixed measurements (some via proxy, some direct), the "Average" column shows both values separately, e.g.: 620 ms 🌐 / 45 ms 🖥️.',
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
    INFO_BAR: 'se actualiza cada 5 min',
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
    BLOQUEO_EXTERNO: 'POSIBLE BLOQUEO EXTERNO',
  },
  httpCodes: [
    {
      code: 0,
      label: 'Sin conexión',
      description:
        'No se pudo establecer conexión con el servidor. Puede ser DNS inexistente (el dominio no existe, se marca caído directamente), timeout o conexión rechazada (en estos casos el sistema además verifica directamente desde tu navegador antes de confirmar la caída).',
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
      description:
        'Acceso prohibido, incluso con autenticación válida. En este monitor suele significar que un WAF/firewall bloqueó al proxy (por venir de un servidor cloud) — el sitio puede estar funcionando normalmente para usuarios reales. El sistema verifica directamente desde tu navegador (🖥️) para confirmarlo.',
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
      description:
        'El sitio respondió, pero el proxy tardó más de 25 segundos en obtener la respuesta (SLOW_RESPONSE). No es una caída: el servicio funciona pero de forma extremadamente lenta.',
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
        'Se ha superado el límite de tasa (Rate Limit) del servicio. Al igual que el 403, puede tratarse de un WAF limitando al proxy — no necesariamente el sitio está caído para usuarios reales.',
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
    'Los colores y símbolos reflejan el tiempo de respuesta (latencia) medido. La justificación se basa en la Psicología de la Interacción y el Significado Operacional del rendimiento. IMPORTANTE: el sistema mide de dos formas distintas (🌐 proxy e 🖥️ directo), y cada una usa su propia escala de umbrales porque no son comparables entre sí — mirá la sección "¿Qué significan los iconos 🌐 y 🖥️?" más abajo.',
  umbrales: [
    {
      key: 'very_fast',
      className: 'status-very-fast',
      emoji: '🚀',
      label: 'MUY RÁPIDO',
      range_text: '🖥️ Directo: < 300 ms  ·  🌐 Proxy: < 600 ms',
      summary: 'Rendimiento Óptimo (Instantáneo para el Usuario)',
      details:
        'Estándar Dorado. El cerebro humano percibe cualquier respuesta por debajo de los 100 ms como instantánea (Regla de Nielsen). Mantener el umbral hasta 300 ms asegura una experiencia fluida. Significado Operacional: El sistema está operando en condiciones óptimas y con alta eficiencia.',
    },
    {
      key: 'fast',
      className: 'status-fast',
      emoji: '⭐',
      label: 'RÁPIDO',
      range_text: '🖥️ Directo: 300–500 ms  ·  🌐 Proxy: 600–1000 ms',
      summary: 'Interacción Fluida sin Molestias (Percepción Inconsciente)',
      details:
        'Límite de la Percepción Inconsciente. La demora es notable pero el usuario no la percibe como una espera molesta. Significado Operacional: Rendimiento excelente, buen punto de control para procesos rápidos de backend.',
    },
    {
      key: 'normal',
      className: 'status-normal',
      emoji: '✅',
      label: 'NORMAL',
      range_text: '🖥️ Directo: 500–800 ms  ·  🌐 Proxy: 1000–1600 ms',
      summary: 'Rendimiento Aceptable (El Foco se Mantiene)',
      details:
        'La Distracción Comienza. A partir de 500 ms el usuario puede comenzar a desviarse, aunque puede mantener su hilo de pensamiento. Significado Operacional: Rendimiento aceptable, pero acercándose a donde la sensación de espera se consolida.',
    },
    {
      key: 'slow',
      className: 'status-slow',
      emoji: '⚠️',
      label: 'LENTO',
      range_text: '🖥️ Directo: 800–1500 ms  ·  🌐 Proxy: 1600–3000 ms',
      summary: 'Demora Molesta (Distractor Activo / Alerta Temprana)',
      details:
        'Límite del 1 Segundo. La demora se convierte en un distractor activo. La experiencia está notablemente degradada. Significado Operacional: Alerta Temprana. El servidor o la red experimentan estrés. Momento de investigar.',
    },
    {
      key: 'critical',
      className: 'status-critical',
      emoji: '🐌',
      label: 'CRÍTICO',
      range_text: '🖥️ Directo: 1500–3000 ms  ·  🌐 Proxy: 3000–6000 ms',
      summary: 'Riesgo de Abandono del Usuario (3 Segundos / Fallo Inminente)',
      details:
        'Pérdida de Foco y Frustración. El límite crítico (3 segundos) donde los usuarios abandonan una página web. Significado Operacional: Fallo Inminente. Indica carga extremadamente pesada o cuellos de botella severos.',
    },
    {
      key: 'risk',
      className: 'status-risk',
      emoji: '🚨',
      label: 'RIESGO',
      range_text: '🖥️ Directo: 3000–5000 ms  ·  🌐 Proxy: 6000–10000 ms',
      summary: 'Fallo Funcional y Colapso (5 Segundos / Alarma)',
      details:
        'Fallo Funcional. Las demoras superiores a 5 segundos son consideradas un fallo funcional en muchos sistemas. Significado Operacional: ALARMA. El servicio está al borde del colapso o no sirve peticiones de manera confiable.',
    },
    {
      key: 'extreme_risk',
      className: 'status-extreme-risk',
      emoji: '🔥',
      label: 'RIESGO EXTREMO',
      range_text: '🖥️ Directo: 5000–99999 ms  ·  🌐 Proxy: 10000–99999 ms',
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
  iconos_title: '¿Qué significan los iconos 🌐 y 🖥️?',
  iconos_intro:
    'Este monitor verifica cada sitio de dos formas posibles. Por eso hay dos escalas de umbrales distintas (ver tabla de arriba): comparar un tiempo 🌐 contra los umbrales 🖥️ (o viceversa) da una lectura equivocada.',
  iconos: [
    {
      emoji: '🌐',
      label: 'Proxy (internet pública)',
      desc: 'Medición hecha desde un servidor de Netlify. Es la medición por defecto. Como viaja por internet (a veces cruzando países/continentes), naturalmente tiene más latencia — por eso su escala de umbrales es más permisiva.',
    },
    {
      emoji: '🖥️',
      label: 'Directo (tu navegador)',
      desc: 'Medición hecha desde tu propio navegador, generalmente cuando el proxy reportó una caída (para descartar que sea un WAF bloqueando al proxy y no una caída real). Refleja tu experiencia real, por eso usa la escala más estricta.',
    },
  ],
  iconos_nota_borde:
    'Un borde azul a la izquierda de una fila indica que la última medición de ese sitio fue directa (🖥️).',
  iconos_nota_promedio:
    'Si un sitio tiene mediciones mixtas (algunas por proxy, otras directas), la columna "Promedio" muestra ambos valores por separado, por ejemplo: 620 ms 🌐 / 45 ms 🖥️.',
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

### Archivo: `lang/i18n_fr.js`
```javascript
// Fichier de traduction - Français
// Ce fichier contient uniquement les textes en français
// Il ne doit pas contenir de logique, seulement des données
const TEXTOS_FR = {
  general: {
    PAGE_TITLE: "Moniteur d'État des Services",
    LAST_UPDATE: 'Dernière mise à jour :',
    LOADING: 'Chargement...',
    INFO_BAR: 'mise à jour toutes les 5 min',
    ADVERTENCIA_FALLO_GLOBAL_HTML:
      'Données de surveillance indisponibles/non fiables. Une latence critique généralisée a été détectée, probablement due à une surcharge du système de surveillance. Veuillez attendre le prochain cycle ou actualiser la page.',
    MOTIVO_FALLO_PRO: 'Motif Pro :',
    FALLO_CRITICO_GRUPO: 'Échec de 100 % du groupe critique :',
    FALLO_CRITICO_LATENCIA_PARTE1: '% des services ont dépassé le seuil de',
    FALLO_CRITICO_RED: 'Aucun résultat disponible (Échec réseau total)',
    DURACION_LABEL: "Durée de l'historique :",
    DURACION_HORA_SINGULAR: 'heure',
    DURACION_HORA_PLURAL: 'heures',
    DURACION_MEDICIONES: 'mesures',
    BTN_REINICIAR: 'Redémarrer la surveillance',
    BTN_COPIAR: 'Copier',
    BTN_COPIADO: 'Copié !',
    BTN_CERRAR: 'Fermer',
  },
  velocidad: {
    VERY_FAST: 'TRÈS RAPIDE',
    FAST: 'RAPIDE',
    NORMAL: 'NORMAL',
    SLOW: 'LENT',
    CRITICAL: 'CRITIQUE',
    RISK: 'RISQUE',
    EXTREME_RISK: 'RISQUE EXTRÊME',
  },
  estados: {
    DOWN: 'HORS SERVICE',
    DOWN_ERROR: 'HORS SERVICE/ERREUR',
    BLOQUEO_EXTERNO: 'BLOCAGE EXTERNE POSSIBLE',
  },
  httpCodes: [
    {
      code: 0,
      label: 'Aucune connexion',
      description:
        "Impossible d'établir une connexion avec le serveur. Cela peut être un DNS inexistant (le domaine n'existe pas, marqué directement comme hors service), un délai d'attente dépassé ou une connexion refusée (dans ces cas, le système vérifie aussi directement depuis votre navigateur avant de confirmer la panne).",
    },
    {
      code: 301,
      label: 'Redirection permanente',
      description: "La ressource a été déplacée de façon permanente vers une nouvelle URL.",
    },
    {
      code: 302,
      label: 'Redirection temporaire',
      description: 'La ressource se trouve temporairement à une URL différente.',
    },
    {
      code: 304,
      label: 'Non modifié',
      description: "La ressource n'a pas changé depuis la dernière requête.",
    },
    {
      code: 400,
      label: 'Requête incorrecte',
      description: 'Requête mal formée ou invalide.',
    },
    {
      code: 401,
      label: 'Non autorisé',
      description: "Une authentification est requise pour accéder à la ressource.",
    },
    {
      code: 402,
      label: 'Paiement requis',
      description: "Réservé pour un usage futur dans les systèmes de paiement.",
    },
    {
      code: 403,
      label: 'Accès interdit',
      description:
        "Accès interdit, même avec une authentification valide. Dans ce moniteur, cela signifie généralement qu'un WAF/pare-feu a bloqué le proxy (car il provient d'un serveur cloud) — le site peut fonctionner normalement pour les utilisateurs réels. Le système vérifie directement depuis votre navigateur (🖥️) pour le confirmer.",
    },
    {
      code: 404,
      label: 'Introuvable',
      description: "La ressource demandée n'existe pas sur le serveur.",
    },
    {
      code: 405,
      label: 'Méthode non autorisée',
      description: "La méthode HTTP utilisée n'est pas autorisée pour cette ressource.",
    },
    {
      code: 406,
      label: 'Non acceptable',
      description:
        "Le serveur ne peut pas générer une réponse acceptable selon les en-têtes Accept.",
    },
    {
      code: 408,
      label: "Délai d'attente dépassé",
      description:
        "Le site a répondu, mais le proxy a mis plus de 25 secondes à obtenir la réponse (SLOW_RESPONSE). Ce n'est pas une panne : le service fonctionne mais de façon extrêmement lente.",
    },
    {
      code: 409,
      label: 'Conflit',
      description:
        "La requête entre en conflit avec l'état actuel du serveur.",
    },
    {
      code: 410,
      label: 'Disparu',
      description:
        "La ressource a été supprimée de façon permanente et n'a pas d'adresse de redirection.",
    },
    {
      code: 418,
      label: 'Je suis une théière',
      description:
        "Code humoristique (RFC 2324). Certains services l'utilisent pour rejeter des requêtes.",
    },
    {
      code: 429,
      label: 'Trop de requêtes',
      description:
        "La limite de requêtes (Rate Limit) du service a été dépassée. Comme pour le 403, il peut s'agir d'un WAF limitant le proxy — le site n'est pas nécessairement hors service pour les utilisateurs réels.",
    },
    {
      code: 500,
      label: 'Erreur du serveur',
      description: 'Erreur interne générique du serveur.',
    },
    {
      code: 501,
      label: 'Non implémenté',
      description:
        "Le serveur ne prend pas en charge la fonctionnalité requise pour compléter la requête.",
    },
    {
      code: 502,
      label: 'Passerelle incorrecte',
      description:
        "Un proxy/passerelle a reçu une réponse invalide du serveur d'origine.",
    },
    {
      code: 503,
      label: 'Service indisponible',
      description:
        'Le serveur est temporairement surchargé, en maintenance ou inactif.',
    },
    {
      code: 504,
      label: "Délai d'attente de la passerelle dépassé",
      description:
        "Un proxy/passerelle n'a pas reçu de réponse à temps du serveur d'origine.",
    },
  ],
};

// Construire l'objet httpStatus automatiquement à partir du tableau de codes
// Cela évite de dupliquer l'information et garde tout synchronisé
TEXTOS_FR.httpStatus = {};
TEXTOS_FR.httpCodes.forEach((item) => {
  TEXTOS_FR.httpStatus[item.code] = item.label;
});
TEXTOS_FR.httpStatus.GENERIC = 'Erreur HTTP';

TEXTOS_FR.tabla = {
  HEADER_SERVICE: 'Service',
  HEADER_URL: 'URL',
  HEADER_LATENCY_ACTUAL: 'Latence Actuelle',
  HEADER_STATUS_ACTUAL: 'État Actuel',
  HEADER_PROMEDIO_MS: 'Moyenne ',
  HEADER_PROMEDIO_STATUS: 'État Moyen',
  HEADER_ACTION: 'Action',
};

TEXTOS_FR.leyenda = {
  title_browser: "Légende du Moniteur d'État",
  main_header: 'Seuils de Latence et Justification Opérationnelle',
  link_volver: "Retour à l'Application",
  intro:
    "Les couleurs et symboles reflètent le temps de réponse (latence) mesuré. La justification est basée sur la Psychologie de l'Interaction et la Signification Opérationnelle de la performance. IMPORTANT : le système mesure de deux façons différentes (🌐 proxy et 🖥️ direct), et chacune utilise sa propre échelle de seuils car elles ne sont pas comparables entre elles — voir la section « Que signifient les icônes 🌐 et 🖥️ ? » ci-dessous.",
  umbrales: [
    {
      key: 'very_fast',
      className: 'status-very-fast',
      emoji: '🚀',
      label: 'TRÈS RAPIDE',
      range_text: '🖥️ Direct : < 300 ms  ·  🌐 Proxy : < 600 ms',
      summary: "Performance Optimale (Instantané pour l'Utilisateur)",
      details:
        "Standard d'Or. Le cerveau humain perçoit toute réponse inférieure à 100 ms comme instantanée (Règle de Nielsen). Maintenir le seuil jusqu'à 300 ms garantit une expérience fluide. Signification Opérationnelle : Le système fonctionne dans des conditions optimales et avec une haute efficacité.",
    },
    {
      key: 'fast',
      className: 'status-fast',
      emoji: '⭐',
      label: 'RAPIDE',
      range_text: '🖥️ Direct : 300–500 ms  ·  🌐 Proxy : 600–1000 ms',
      summary: 'Interaction Fluide sans Gêne (Perception Inconsciente)',
      details:
        "Limite de la Perception Inconsciente. Le délai est notable mais l'utilisateur ne le perçoit pas comme une attente gênante. Signification Opérationnelle : Excellente performance, bon point de contrôle pour les processus rapides de backend.",
    },
    {
      key: 'normal',
      className: 'status-normal',
      emoji: '✅',
      label: 'NORMAL',
      range_text: '🖥️ Direct : 500–800 ms  ·  🌐 Proxy : 1000–1600 ms',
      summary: "Performance Acceptable (L'Attention se Maintient)",
      details:
        "Le Début de la Distraction. À partir de 500 ms, l'utilisateur peut commencer à se déconcentrer, bien qu'il puisse maintenir le fil de sa pensée. Signification Opérationnelle : Performance acceptable, mais s'approchant du point où la sensation d'attente s'installe.",
    },
    {
      key: 'slow',
      className: 'status-slow',
      emoji: '⚠️',
      label: 'LENT',
      range_text: '🖥️ Direct : 800–1500 ms  ·  🌐 Proxy : 1600–3000 ms',
      summary: 'Délai Gênant (Distraction Active / Alerte Précoce)',
      details:
        "Limite de la Seconde. Le délai devient une distraction active. L'expérience est notablement dégradée. Signification Opérationnelle : Alerte Précoce. Le serveur ou le réseau subissent un stress. Moment d'enquêter.",
    },
    {
      key: 'critical',
      className: 'status-critical',
      emoji: '🐌',
      label: 'CRITIQUE',
      range_text: '🖥️ Direct : 1500–3000 ms  ·  🌐 Proxy : 3000–6000 ms',
      summary: "Risque d'Abandon de l'Utilisateur (3 Secondes / Échec Imminent)",
      details:
        "Perte d'Attention et Frustration. La limite critique (3 secondes) où les utilisateurs abandonnent une page web. Signification Opérationnelle : Échec Imminent. Indique une charge extrêmement lourde ou des goulots d'étranglement sévères.",
    },
    {
      key: 'risk',
      className: 'status-risk',
      emoji: '🚨',
      label: 'RISQUE',
      range_text: '🖥️ Direct : 3000–5000 ms  ·  🌐 Proxy : 6000–10000 ms',
      summary: 'Échec Fonctionnel et Effondrement (5 Secondes / Alarme)',
      details:
        "Échec Fonctionnel. Les délais supérieurs à 5 secondes sont considérés comme un échec fonctionnel dans de nombreux systèmes. Signification Opérationnelle : ALARME. Le service est au bord de l'effondrement ou ne répond pas aux requêtes de manière fiable.",
    },
    {
      key: 'extreme_risk',
      className: 'status-extreme-risk',
      emoji: '🔥',
      label: 'RISQUE EXTRÊME',
      range_text: '🖥️ Direct : 5000–99999 ms  ·  🌐 Proxy : 10000–99999 ms',
      summary: 'Latence Inacceptable (CHAOS / Abandon Assuré)',
      details:
        "CHAOS/Limbes. Plage avant le délai d'attente maximal. Il est presque certain que l'utilisateur a abandonné l'action. Signification Opérationnelle : Le serveur ne peut pas traiter la requête dans un délai raisonnable. Nécessite une attention IMMÉDIATE.",
    },
    {
      key: 'down',
      className: 'status-down',
      emoji: '❌',
      label: 'ÉCHEC TOTAL',
      range_text: '≥ 99999 ms',
      summary: "Panne Confirmée (Délai d'Attente Dépassé)",
      details:
        "Panne Confirmée. La valeur de PENALIZACION_FALLO a été dépassée. Signification Opérationnelle : Le service est hors service, la route est inaccessible, ou le serveur a refusé de répondre.",
    },
  ],
  http_codes_title: "Codes d'État HTTP et Échecs du Système",
  http_codes_description:
    "Lorsqu'un service renvoie un code d'état hors de la plage 2xx (Succès), le moniteur le classe visuellement comme ❌ ÉCHEC TOTAL, mais affiche le code réel entre parenthèses (ex : ❌ Hors service (404)).",
  iconos_title: 'Que signifient les icônes 🌐 et 🖥️ ?',
  iconos_intro:
    "Ce moniteur vérifie chaque site de deux façons possibles. C'est pourquoi il existe deux échelles de seuils différentes (voir tableau ci-dessus) : comparer un temps 🌐 aux seuils 🖥️ (ou inversement) donne une lecture erronée.",
  iconos: [
    {
      emoji: '🌐',
      label: 'Proxy (internet public)',
      desc: "Mesure effectuée depuis un serveur Netlify. C'est la mesure par défaut. Comme elle transite par internet (parfois en traversant des pays/continents), elle a naturellement plus de latence — c'est pourquoi son échelle de seuils est plus permissive.",
    },
    {
      emoji: '🖥️',
      label: 'Direct (votre navigateur)',
      desc: "Mesure effectuée depuis votre propre navigateur, généralement lorsque le proxy a signalé une panne (pour écarter l'hypothèse d'un WAF bloquant le proxy plutôt qu'une vraie panne). Reflète votre expérience réelle, c'est pourquoi elle utilise l'échelle la plus stricte.",
    },
  ],
  iconos_nota_borde:
    "Une bordure bleue à gauche d'une ligne indique que la dernière mesure de ce site était directe (🖥️).",
  iconos_nota_promedio:
    "Si un site a des mesures mixtes (certaines par proxy, d'autres directes), la colonne « Moyenne » affiche les deux valeurs séparément, par exemple : 620 ms 🌐 / 45 ms 🖥️.",
};

TEXTOS_FR.leyenda.codigos_error = [
  {
    code: '2xx',
    label: 'OK / Succès',
    description:
      'La connexion et le service ont répondu correctement (Latence mesurée).',
  },
];

TEXTOS_FR.httpCodes
  .filter((item) => item.code === 0 || item.code >= 400)
  .forEach((item) => {
    TEXTOS_FR.leyenda.codigos_error.push({
      code: item.code.toString(),
      label: item.label,
      description: item.description,
    });
  });

// Assigner uniquement les textes à la variable globale
// La logique de chargement se trouve dans i18n.js
window.TEXTOS_ACTUAL = TEXTOS_FR;

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
const dns = require('dns').promises;

const httpsAgent = new https.Agent({
  rejectUnauthorized: true,
  keepAlive: true,
  maxSockets: 10,
  timeout: 30000,
});
const httpAgent = new http.Agent({
  keepAlive: true,
  maxSockets: 10,
  timeout: 30000,
});

const TIMEOUT_MS = 25000;

// Rotar User-Agents para evitar fingerprinting
const USER_AGENTS = [
  'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36',
  'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36',
  'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0',
];

exports.handler = async (event, context) => {
  const targetUrl = event.queryStringParameters && event.queryStringParameters.url;

  if (!targetUrl) {
    return {
      statusCode: 400,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: "Parámetro 'url' requerido." }),
    };
  }

  const headers = {
    'Access-Control-Allow-Origin': '*',
    'Content-Type': 'application/json',
  };

  const diagnostics = { url: targetUrl, attempts: [] };

  async function tryFetch(url, method, useMinimalHeaders, attemptNum) {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), TIMEOUT_MS);
    const startTime = Date.now();

    try {
      const parsed = new URL(url);
      const agentToUse = parsed.protocol === 'http:' ? httpAgent : httpsAgent;
      const userAgent = USER_AGENTS[attemptNum % USER_AGENTS.length];

      const requestHeaders = useMinimalHeaders
        ? { 'User-Agent': userAgent }
        : {
            'User-Agent': userAgent,
            'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
            'Accept-Language': 'es-419,es;q=0.9',
            'Accept-Encoding': 'gzip, deflate, br',
          };

      const resp = await fetch(url, {
        method: method,
        signal: controller.signal,
        redirect: 'follow',
        agent: agentToUse,
        headers: requestHeaders,
        compress: true,
      });

      const totalTime = Date.now() - startTime;
      clearTimeout(timeoutId);

      return {
        success: true,
        status: resp.status,
        time: totalTime,
        attempt: attemptNum,
        method: method,
      };

    } catch (error) {
      clearTimeout(timeoutId);
      const elapsed = Date.now() - startTime;
      let errorType = 'UNKNOWN';
      if (error.name === 'AbortError') errorType = 'TIMEOUT';
      else if (error.code === 'ECONNREFUSED') errorType = 'CONN_REFUSED';
      else if (error.code === 'ENOTFOUND' || error.code === 'EAI_AGAIN') errorType = 'DNS_ERROR';
      else if (error.code === 'ETIMEDOUT') errorType = 'CONN_TIMEOUT';
      else if (error.message && error.message.includes('timeout')) errorType = 'TIMEOUT';

      return {
        success: false,
        error: error.message,
        errorType: errorType,
        elapsed: elapsed,
        attempt: attemptNum,
        method: method,
      };
    }
  }

  // PASO 1: DNS
  try {
    const parsed = new URL(targetUrl);
    await dns.lookup(parsed.hostname);
  } catch (dnsErr) {
    return {
      statusCode: 200, headers,
      body: JSON.stringify({
        status: 0, time: 99999,
        error: 'DNS failed: ' + dnsErr.message,
        errorType: 'DNS_ERROR', down: true, diagnostics,
      }),
    };
  }

  // PASO 2: GET con headers mínimos (antes era HEAD; se cambió porque un
  // navegador real nunca manda HEAD solo, y algunos sitios/WAF procesan HEAD
  // de forma distinta —a veces más lenta— que un GET normal, lo que inflaba
  // la latencia medida sin relación con la experiencia real del usuario)
  let result = await tryFetch(targetUrl, 'GET', true, 1);
  diagnostics.attempts.push(result);

  if (result.success) {
    return {
      statusCode: 200, headers,
      body: JSON.stringify({ status: result.status, time: result.time, diagnostics }),
    };
  }

  // PASO 3: GET con headers mínimos
  if (result.errorType === 'TIMEOUT' || result.errorType === 'CONN_TIMEOUT') {
    await new Promise(r => setTimeout(r, 1000));
    result = await tryFetch(targetUrl, 'GET', true, 2);
    diagnostics.attempts.push(result);

    if (result.success) {
      return {
        statusCode: 200, headers,
        body: JSON.stringify({ status: result.status, time: result.time, diagnostics }),
      };
    }
  }

  // PASO 4: GET con headers completos (último intento)
  if (result.errorType === 'TIMEOUT' || result.errorType === 'CONN_TIMEOUT') {
    await new Promise(r => setTimeout(r, 1000));
    result = await tryFetch(targetUrl, 'GET', false, 3);
    diagnostics.attempts.push(result);

    if (result.success) {
      return {
        statusCode: 200, headers,
        body: JSON.stringify({ status: result.status, time: result.time, diagnostics }),
      };
    }
  }

  // CAÍDO REAL
  const lastAttempt = diagnostics.attempts[diagnostics.attempts.length - 1];

  if (lastAttempt.errorType === 'TIMEOUT' && lastAttempt.elapsed >= TIMEOUT_MS - 1000) {
    return {
      statusCode: 200, headers,
      body: JSON.stringify({
        status: 408, time: lastAttempt.elapsed,
        error: 'El sitio responde muy lentamente (timeout)',
        errorType: 'SLOW_RESPONSE', diagnostics,
      }),
    };
  }

  return {
    statusCode: 200, headers,
    body: JSON.stringify({
      status: 0, time: 99999,
      error: lastAttempt.error,
      errorType: lastAttempt.errorType,
      down: true, diagnostics,
    }),
  };
};

```

### Archivo: `netlify/functions/update-webs.js`
```javascript
const fetch = require('node-fetch');
const crypto = require('crypto');
// Fuente única del hash de acceso: la misma constante la valida también
// abm-webs.html del lado del cliente. Para cambiar la contraseña alcanza
// con actualizar js/config.js.
const { HASH_ACCESO_SHA256 } = require('../../js/config.js');

function calcularHashSHA256(texto) {
  return crypto.createHash('sha256').update(texto || '').digest('hex');
}

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
        'Access-Control-Allow-Headers': 'Content-Type, Authorization, x-admin-password',
        'Access-Control-Allow-Methods': 'POST, OPTIONS',
      },
      body: '',
    };
  }

  // Validar contraseña ANTES de mirar el body: sin esto, cualquiera con la
  // URL de esta función podría escribir en el repo sin pasar por el login.
  const passwordRecibida =
    event.headers['x-admin-password'] || event.headers['X-Admin-Password'];
  if (calcularHashSHA256(passwordRecibida) !== HASH_ACCESO_SHA256) {
    return {
      statusCode: 401,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Contraseña incorrecta o faltante' }),
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
# Monitor de Disponibilidad de Servicios 🚀

## ¿Qué es?

Un sistema de monitoreo en tiempo real que verifica automáticamente cada 5 minutos el **estado** y **tiempo de respuesta** de tus servicios web. Te alerta cuando algo va mal antes de que tus usuarios lo noten.

## Características principales

- ✅ **Monitoreo automático** cada 5 minutos
- 📊 **7 niveles de clasificación** desde "Muy Rápido" hasta "Caído"
- 🎨 **5 temas visuales** con alternancia claro/oscuro
- 🌍 **Multiidioma** (Español, Inglés, extensible)
- 📈 **Historial configurable** de 1 a 9 horas
- 🚨 **Detección inteligente** de fallos globales vs individuales
- 📱 **Diseño responsive** para cualquier dispositivo
- 🔍 **Expansión de errores** con detalles completos (temas avanzados)
- 🛡️ **Verificación dual**: proxy serverless + verificación directa por navegador
- 🌐/🖥️ **Indicadores de fuente**: distingue mediciones por proxy (internet) vs directas (red interna)
- 🧱 **Manejo de WAF**: detecta y evita falsos positivos por bloqueos de firewall

---

El sistema revisa automáticamente tus servicios y te muestra su estado de forma visual e intuitiva, para que siempre sepas si todo funciona bien.

## Inicio Rápido

### Desplegar en Netlify

# Monitor de Disponibilidad de Servicios

Este documento reúne la información principal sobre el proyecto: qué hace, cómo empezar rápido y dónde encontrar la documentación técnica.

Qué hace

- Monitorea periódicamente tus sitios y APIs, mide latencia y estado, y muestra un tablero visual con alertas y un historial por sesión.

Principales características

- Monitoreo automático con intervalo configurable.
- Clasificación de latencia en niveles claros (desde "Muy rápido" hasta "Caído").
- Historial por sesión para seguir tendencias recientes.
- Temas visuales (incluye alternancia claro/oscuro) y soporte i18n (es/en).

Inicio rápido

1. Agrega tus servicios en `data/webs.json` (nombre, URL, grupo, orden).
2. Despliega en Netlify (o abre `index.html` localmente) y accede al tablero.
3. Para cambiar tema o idioma usa los parámetros `?tema=` y `?lang=` en la URL.

### ¿Cómo funciona la verificación dual?

El monitor intenta verificar tus servicios de **dos formas**:

1. **Proxy serverless** (🌐): desde los servidores de Netlify en internet pública.
2. **Verificación directa** (🖥️): desde tu navegador, por la red interna.

**¿Por qué dos?** Algunos sitios usan WAF (firewall) que bloquea requests desde servidores cloud como Netlify. Cuando el proxy falla, el sistema verifica directamente desde tu navegador para confirmar si el sitio realmente está caído o solo bloqueó el proxy.

**Indicadores visuales:**
- 🌐 = Medición vía proxy (internet pública)
- 🖥️ = Medición directa desde tu navegador (red interna)
- Borde azul izquierdo en la fila = última medición fue directa
- Promedios separados: si hay mediciones mixtas, muestra ambos

Ejemplo de acceso:

```
https://tu-monitor.netlify.app/           ← Tema por defecto
https://tu-monitor.netlify.app/?tema=pro   ← Tema profesional
https://tu-monitor.netlify.app/?lang=en   ← Interfaz en inglés
```

Dónde encontrar la documentación técnica

- Manual de usuario (uso de la pantalla, sin datos técnicos): [MANUAL_USUARIO.md](https://github.com/FABIOR1981/documentacion-central/blob/main/monitor-status2/documentacion/MANUAL_USUARIO.md) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/monitor-status2/documentacion/MANUAL_USUARIO.pdf)).
- En [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/monitor-status2/documentacion) (principal: `readme.md`):
- Guías específicas:
  - `configuracion.md` — ajustes y personalización
  - `arquitectura.md` — cómo funciona internamente
  - `estructura.md` — organización de archivos
  - `justificacion_rangos_latencia.md` — por qué esos umbrales
  - `resolución de problemas.md` — solución de problemas comunes

Licencia

- MIT — consultá el archivo `LICENSE` para detalles.



```

