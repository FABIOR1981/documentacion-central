# Bundle del Proyecto para IA: base_smconsultores

## Estructura de Archivos
- README.md
- build-html.js
- css/styles.css
- documentacion/LEEME.md
- html-src/README.md
- html-src/dashboard.html
- html-src/empresas.html
- html-src/layout.html
- html-src/llamados.html
- html-src/login.html
- html-src/modales.html
- html-src/postulantes.html
- index.html
- js/app.js
- netlify.toml
- netlify/functions/_auth.js
- netlify/functions/get-table.js
- netlify/functions/login.js
- netlify/functions/update-stats.js
- netlify/functions/update-table.js
- package.json
- scripts/generar-usuario.js

---

## Código Fuente

### Archivo: `README.md`
```md
# base_smconsultores

App de gestión de Empresas, Llamados y Postulantes (SM Consultores), desplegada en Netlify.
Los datos se guardan como archivos JSON en el repo `FABIOR1981/bd` (carpeta `smconsultores`), leídos/escritos
por las funciones serverless a través de la API de contenidos de GitHub.

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/base_smconsultores/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/base_smconsultores/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## ⚠️ Antes de tocar el frontend (`index.html`)

**`index.html` NO se edita directamente — se genera automáticamente a partir de `html-src/`.**
Cualquier cambio hecho a mano en `index.html` se **pierde** en el próximo build (local o en Netlify).

👉 Leer **[`html-src/README.md`](./html-src/README.md) primero**, siempre, antes de modificar cualquier
parte de la interfaz (formularios, pestañas, modales). Ahí está explicado cómo está armado, cómo editar
cada sección, cómo agregar una pestaña nueva, y por qué se hizo así.

## Variables de entorno (configurar en Netlify > Site settings > Environment variables)

- `GITHUB_TOKEN` — token con permiso de escritura (`contents:write`) sobre el repo `FABIOR1981/bd`.
- `SESSION_SECRET` — string aleatoria larga (ej: 40+ caracteres), usada para firmar los tokens de sesión del login.
  Generar una con: `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`

## Login

La app pide usuario y contraseña antes de mostrar cualquier dato. Las credenciales se validan contra
`smconsultores/usuarios.json` dentro del repo `FABIOR1981/bd` (mismo lugar que las demás tablas).

Formato de `usuarios.json`:
```json
[
  { "usuario": "admin", "salt": "....", "hash": "...." }
]
```

La contraseña **nunca se guarda en texto plano**: se guarda `sha256(salt + password)`. El `salt` es aleatorio
por usuario para que dos usuarios con la misma contraseña no tengan el mismo hash, y para dificultar ataques
de tablas precomputadas (rainbow tables).

### Cómo dar de alta un usuario

```bash
node scripts/generar-usuario.js <usuario> <password>
```

Esto imprime un objeto `{ usuario, salt, hash }`. Agregalo al array en `smconsultores/usuarios.json` (en el
repo `bd`) y subilo — a mano, o con la misma API de contenidos de GitHub que usan las funciones.

### Notas de seguridad

- El login devuelve un token firmado (HMAC-SHA256 con `SESSION_SECRET`) válido por 8 horas, que el frontend
  guarda en `sessionStorage` y envía en el header `Authorization: Bearer <token>` en cada request.
- Las funciones `get-table`, `update-table` y `update-stats` rechazan cualquier request sin un token válido.
- `SHA-256 + salt` es aceptable para este caso de uso (pocos usuarios, app interna), pero para un sistema con
  más usuarios o expuesto públicamente conviene usar un algoritmo pensado para contraseñas (bcrypt, scrypt o
  Argon2), que son deliberadamente lentos para dificultar ataques de fuerza bruta — SHA-256 es rápido por
  diseño y no fue pensado para hashear contraseñas.

```

### Archivo: `build-html.js`
```javascript
// build-html.js
// Arma /index.html juntando html-src/layout.html + los fragmentos que referencia
// con marcadores <!--INCLUDE:archivo.html-->.
//
// Se corre solo (sin dependencias): node build-html.js
// Netlify lo corre automáticamente antes de cada deploy (ver netlify.toml).
const fs = require('fs');
const path = require('path');

const SRC_DIR = path.join(__dirname, 'html-src');
const LAYOUT = path.join(SRC_DIR, 'layout.html');
const SALIDA = path.join(__dirname, 'index.html');

function build() {
  let contenido = fs.readFileSync(LAYOUT, 'utf8');

  contenido = contenido.replace(/<!--INCLUDE:(.+?)-->/g, (match, nombreArchivo) => {
    const rutaFragmento = path.join(SRC_DIR, nombreArchivo.trim());
    if (!fs.existsSync(rutaFragmento)) {
      throw new Error(`build-html.js: no se encontró el fragmento "${nombreArchivo}" (${rutaFragmento})`);
    }
    return fs.readFileSync(rutaFragmento, 'utf8');
  });

  fs.writeFileSync(SALIDA, contenido, 'utf8');
  console.log(`✅ index.html generado a partir de html-src/ (${contenido.length} caracteres)`);
}

build();

```

### Archivo: `css/styles.css`
```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap');

/* ===== RESET & BASE ===== */
* { box-sizing: border-box; margin: 0; padding: 0; }

html { scroll-behavior: smooth; }

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  background: linear-gradient(135deg, #f0f4f8 0%, #e8eef5 50%, #f5f0f8 100%);
  min-height: 100vh;
  color: #1e293b;
  line-height: 1.6;
  padding-bottom: 3rem;
}

/* ===== CONTAINER ===== */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem 1rem;
}

/* ===== HEADER ===== */
.app-header {
  text-align: center;
  margin-bottom: 2.5rem;
  padding: 1.5rem;
  background: linear-gradient(135deg, #1e3a5f 0%, #2563eb 50%, #3b82f6 100%);
  border-radius: 1rem;
  color: white;
  box-shadow: 0 10px 40px rgba(37, 99, 235, 0.3);
  position: relative;
  overflow: hidden;
}
.app-header::before {
  content: '';
  position: absolute;
  top: -50%;
  right: -20%;
  width: 400px;
  height: 400px;
  background: rgba(255,255,255,0.08);
  border-radius: 50%;
}
.app-header h1 {
  font-size: 2rem;
  font-weight: 800;
  position: relative;
  z-index: 1;
  text-shadow: 0 2px 4px rgba(0,0,0,0.2);
}
.app-header p {
  opacity: 0.85;
  margin-top: 0.25rem;
  font-weight: 300;
  font-size: 0.95rem;
}

/* ===== TABS ===== */
.tabs {
  display: flex;
  gap: 0.5rem;
  background: rgba(255,255,255,0.7);
  backdrop-filter: blur(10px);
  padding: 0.5rem;
  border-radius: 1rem;
  margin-bottom: 2rem;
  box-shadow: 0 4px 20px rgba(0,0,0,0.06);
  border: 1px solid rgba(255,255,255,0.5);
}
.tab-btn {
  flex: 1;
  padding: 0.85rem 1rem;
  border: none;
  background: transparent;
  border-radius: 0.75rem;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.9rem;
  color: #64748b;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  font-family: 'Inter', sans-serif;
}
.tab-btn:hover {
  background: rgba(37, 99, 235, 0.08);
  color: #2563eb;
  transform: translateY(-1px);
}
.tab-btn.active {
  background: linear-gradient(135deg, #2563eb, #3b82f6);
  color: white;
  box-shadow: 0 4px 15px rgba(37, 99, 235, 0.35);
}

/* ===== SECTIONS / CARDS ===== */
.section {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(12px);
  padding: 1.75rem;
  border-radius: 1rem;
  box-shadow: 0 4px 24px rgba(0,0,0,0.06), 0 1px 3px rgba(0,0,0,0.04);
  margin-bottom: 1.5rem;
  border: 1px solid rgba(226, 232, 240, 0.6);
  transition: box-shadow 0.3s, transform 0.3s;
}
.section:hover {
  box-shadow: 0 8px 32px rgba(0,0,0,0.08);
}
.section-title {
  font-size: 1.1rem;
  font-weight: 700;
  margin-bottom: 1.25rem;
  color: #1e3a5f;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding-bottom: 0.75rem;
  border-bottom: 2px solid #e2e8f0;
}

/* ===== FORM ELEMENTS ===== */
.input {
  width: 100%;
  border: 1.5px solid #e2e8f0;
  border-radius: 0.625rem;
  padding: 0.65rem 0.875rem;
  font-size: 0.9rem;
  font-family: 'Inter', sans-serif;
  background: #f8fafc;
  color: #1e293b;
  transition: all 0.25s;
}
.input:hover {
  border-color: #cbd5e1;
  background: #fff;
}
.input:focus {
  outline: none;
  border-color: #3b82f6;
  background: #fff;
  box-shadow: 0 0 0 4px rgba(59, 130, 246, 0.12);
}
.label {
  display: block;
  font-size: 0.8rem;
  font-weight: 600;
  color: #475569;
  margin-top: 1rem;
  margin-bottom: 0.35rem;
  text-transform: uppercase;
  letter-spacing: 0.03em;
}

/* ===== BUTTONS ===== */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.65rem 1.25rem;
  border-radius: 0.625rem;
  border: none;
  cursor: pointer;
  font-size: 0.875rem;
  font-weight: 600;
  font-family: 'Inter', sans-serif;
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}
.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.12);
}
.btn:active {
  transform: translateY(0);
}
.btn-primary {
  background: linear-gradient(135deg, #2563eb, #3b82f6);
  color: white;
}
.btn-primary:hover {
  background: linear-gradient(135deg, #1d4ed8, #2563eb);
}
.btn-secondary {
  background: linear-gradient(135deg, #64748b, #94a3b8);
  color: white;
}
.btn-secondary:hover {
  background: linear-gradient(135deg, #475569, #64748b);
}
.btn-success {
  background: linear-gradient(135deg, #16a34a, #22c55e);
  color: white;
}
.btn-success:hover {
  background: linear-gradient(135deg, #15803d, #16a34a);
}
.btn-warning {
  background: linear-gradient(135deg, #d97706, #f59e0b);
  color: white;
}
.btn-warning:hover {
  background: linear-gradient(135deg, #b45309, #d97706);
}
.btn-danger {
  background: linear-gradient(135deg, #dc2626, #ef4444);
  color: white;
}
.btn-danger:hover {
  background: linear-gradient(135deg, #b91c1c, #dc2626);
}
.btn-sm { padding: 0.4rem 0.875rem; font-size: 0.8rem; }
.btn-lg { padding: 0.875rem 2.5rem; font-size: 1rem; }

/* ===== GRID ===== */
.grid-2 {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
}
.grid-4 {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
}
@media (max-width: 768px) {
  .grid-2, .grid-4 { grid-template-columns: 1fr; }
  .tabs { flex-wrap: wrap; }
  .tab-btn { font-size: 0.8rem; padding: 0.6rem; }
}

/* ===== TABLES ===== */
.table-wrap {
  overflow-x: auto;
  border-radius: 0.75rem;
  border: 1px solid #e2e8f0;
  background: #f8fafc;
}
.table-wrap table {
  width: 100%;
  font-size: 0.85rem;
  border-collapse: separate;
  border-spacing: 0;
}
.table-wrap th {
  background: linear-gradient(135deg, #f1f5f9, #e2e8f0);
  color: #475569;
  font-weight: 700;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.04em;
  padding: 0.75rem 1rem;
  border-bottom: 2px solid #cbd5e1;
  position: sticky;
  top: 0;
  z-index: 10;
}
.table-wrap td {
  padding: 0.75rem 1rem;
  border-bottom: 1px solid #e2e8f0;
  color: #334155;
}
.table-wrap tr:last-child td { border-bottom: none; }
.table-wrap tr:hover td {
  background: rgba(59, 130, 246, 0.04);
}

/* ===== BADGES ===== */
.badge {
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
  padding: 0.3rem 0.75rem;
  border-radius: 9999px;
  font-size: 0.75rem;
  font-weight: 700;
}
.badge-blue {
  background: linear-gradient(135deg, #dbeafe, #bfdbfe);
  color: #1e40af;
}
.badge-yellow {
  background: linear-gradient(135deg, #fef3c7, #fde68a);
  color: #92400e;
}
.badge-green {
  background: linear-gradient(135deg, #d1fae5, #a7f3d0);
  color: #065f46;
}
.badge-red {
  background: linear-gradient(135deg, #fee2e2, #fecaca);
  color: #991b1b;
}
.badge-gray {
  background: linear-gradient(135deg, #f1f5f9, #e2e8f0);
  color: #475569;
}

/* ===== STATS / CARDS ===== */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1.25rem;
}
.stat-card {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  padding: 1.75rem;
  border-radius: 1rem;
  box-shadow: 0 4px 20px rgba(0,0,0,0.06);
  border: 1px solid rgba(226, 232, 240, 0.6);
  text-align: center;
  transition: transform 0.3s, box-shadow 0.3s;
  position: relative;
  overflow: hidden;
}
.stat-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #2563eb, #3b82f6, #60a5fa);
  border-radius: 1rem 1rem 0 0;
}
.stat-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 40px rgba(0,0,0,0.1);
}
.stat-number {
  font-size: 2.75rem;
  font-weight: 800;
  background: linear-gradient(135deg, #1e3a5f, #2563eb);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  line-height: 1.1;
}
.stat-label {
  font-size: 0.85rem;
  color: #64748b;
  margin-top: 0.5rem;
  font-weight: 500;
}

/* ===== SEARCH BAR ===== */
.search-bar {
  display: flex;
  gap: 0.75rem;
  margin-bottom: 1.25rem;
  flex-wrap: wrap;
}
.search-bar .input {
  flex: 1;
  min-width: 200px;
}

/* ===== RADIO GROUP ===== */
.radio-group {
  display: flex;
  gap: 1.5rem;
  margin: 0.75rem 0 1.25rem;
  flex-wrap: wrap;
}
.radio-group label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
  font-size: 0.9rem;
  color: #475569;
  font-weight: 500;
  padding: 0.5rem 0.75rem;
  border-radius: 0.5rem;
  transition: background 0.2s;
}
.radio-group label:hover {
  background: rgba(37, 99, 235, 0.06);
}
.radio-group input[type="radio"] {
  width: 18px;
  height: 18px;
  accent-color: #2563eb;
  cursor: pointer;
}

/* ===== MODALS ===== */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(15, 23, 42, 0.6);
  backdrop-filter: blur(4px);
  z-index: 50;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  animation: fadeIn 0.2s ease;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
.modal-box {
  background: white;
  border-radius: 1rem;
  box-shadow: 0 25px 60px -12px rgba(0,0,0,0.25);
  width: 100%;
  max-width: 1200px;
  max-height: 90vh;
  overflow-y: auto;
  padding: 1.75rem;
  animation: slideUp 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}
.modal-box-sm { max-width: 650px; }
@keyframes slideUp {
  from { opacity: 0; transform: translateY(20px) scale(0.98); }
  to { opacity: 1; transform: translateY(0) scale(1); }
}
.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.25rem;
  padding-bottom: 0.75rem;
  border-bottom: 2px solid #e2e8f0;
}
.modal-close {
  font-size: 1.75rem;
  color: #94a3b8;
  background: #f1f5f9;
  border: none;
  cursor: pointer;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}
.modal-close:hover {
  background: #e2e8f0;
  color: #475569;
  transform: rotate(90deg);
}

/* ===== COMPARE CELLS (DUPLICADOS) ===== */
.compare-cell {
  font-size: 0.75rem;
  font-family: 'SF Mono', Monaco, monospace;
  background: #f8fafc;
  padding: 0.6rem;
  border-radius: 0.5rem;
  max-height: 140px;
  overflow-y: auto;
  white-space: pre-wrap;
  word-break: break-word;
  border: 1px solid #e2e8f0;
  color: #475569;
}

/* ===== ROW COLORS DUPLICADOS ===== */
.row-ignorar { background: #f8fafc; }
.row-actualizar { background: #fffbeb; }
.row-nuevo { background: #eff6ff; }

/* ===== ACTION SELECT ===== */
.action-select {
  width: 100%;
  padding: 0.4rem 0.6rem;
  border-radius: 0.5rem;
  border: 1.5px solid #e2e8f0;
  font-size: 0.8rem;
  font-family: 'Inter', sans-serif;
  background: #f8fafc;
  cursor: pointer;
  transition: border-color 0.2s;
}
.action-select:focus {
  outline: none;
  border-color: #3b82f6;
}

/* ===== DETAIL PANEL ===== */
.detail-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
}
.detail-item {
  background: #f8fafc;
  padding: 0.875rem;
  border-radius: 0.625rem;
  border: 1px solid #e2e8f0;
}
.detail-item dt {
  font-size: 0.7rem;
  color: #94a3b8;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  font-weight: 700;
  margin-bottom: 0.25rem;
}
.detail-item dd {
  font-size: 0.9rem;
  font-weight: 600;
  color: #1e293b;
}

/* ===== WARNING / INFO BOXES ===== */
.info-box {
  background: linear-gradient(135deg, #eff6ff, #dbeafe);
  border: 1px solid #bfdbfe;
  border-radius: 0.75rem;
  padding: 0.875rem 1rem;
  font-size: 0.875rem;
  color: #1e40af;
  margin-top: 0.75rem;
}
.warning-box {
  background: linear-gradient(135deg, #fffbeb, #fef3c7);
  border: 1px solid #fde68a;
  border-radius: 0.75rem;
  padding: 0.875rem 1rem;
  font-size: 0.875rem;
  color: #92400e;
  margin-top: 0.75rem;
}

/* ===== EXTRA FIELDS TAGS ===== */
.field-tag {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  background: linear-gradient(135deg, #dbeafe, #bfdbfe);
  color: #1e40af;
  padding: 0.35rem 0.875rem;
  border-radius: 9999px;
  font-size: 0.8rem;
  font-weight: 600;
  box-shadow: 0 1px 4px rgba(37, 99, 235, 0.15);
}
.field-tag button {
  background: none;
  border: none;
  color: #1e40af;
  cursor: pointer;
  font-weight: 700;
  font-size: 1rem;
  line-height: 1;
  opacity: 0.7;
  transition: opacity 0.2s;
}
.field-tag button:hover { opacity: 1; }

/* ===== FILE INPUT ===== */
input[type="file"] {
  padding: 0.5rem;
  background: #f8fafc;
  border: 2px dashed #cbd5e1;
  border-radius: 0.625rem;
  cursor: pointer;
  transition: all 0.2s;
}
input[type="file"]:hover {
  border-color: #3b82f6;
  background: #eff6ff;
}

/* ===== UTILITIES ===== */
.hidden { display: none !important; }
.text-center { text-align: center; }
.text-red { color: #dc2626; }
.text-gray { color: #64748b; }
.text-sm { font-size: 0.875rem; }
.text-xs { font-size: 0.75rem; }
.font-bold { font-weight: 700; }
.mt-1 { margin-top: 0.25rem; }
.mt-2 { margin-top: 0.5rem; }
.mb-2 { margin-bottom: 0.5rem; }
.mb-4 { margin-bottom: 1rem; }
.flex { display: flex; }
.gap-2 { gap: 0.5rem; }
.gap-3 { gap: 0.75rem; }
.justify-center { justify-content: center; }
.justify-end { justify-content: flex-end; }
.items-center { align-items: center; }
.flex-wrap { flex-wrap: wrap; }
.p-3 { padding: 0.75rem; }
.bg-gray-50 { background: #f8fafc; }
.rounded { border-radius: 0.5rem; }

/* ===== SCROLLBAR ===== */
::-webkit-scrollbar { width: 8px; height: 8px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
::-webkit-scrollbar-thumb:hover { background: #94a3b8; }

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/base_smconsultores/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `html-src/README.md`
```md
# Cómo se arma index.html (a partir de julio 2026)

`index.html` **ya no se edita a mano**. Se genera automáticamente juntando los
fragmentos de `html-src/` con el script `build-html.js`.

## Estructura

```
html-src/
  layout.html       <- esqueleto: <head>, header, tabs, <script>, con marcadores <!--INCLUDE:archivo-->
  login.html        <- pantalla de login
  dashboard.html     <- pestaña Dashboard
  empresas.html      <- pestaña Empresas
  llamados.html      <- pestaña Llamados
  postulantes.html   <- pestaña Postulantes
  modales.html        <- todos los modales (duplicados, cargando, llamado, postulante, resultados)
build-html.js       <- script que junta todo y escribe /index.html
index.html          <- SE GENERA SOLO. No editar directamente, se pisa en cada build.
```

## Cómo editar algo

1. Encontrá el fragmento correspondiente en `html-src/` (ej: para cambiar el formulario
   de Llamados, editás `html-src/llamados.html`).
2. Corré el build para regenerar `index.html` y ver el resultado:
   ```bash
   node build-html.js
   # o, equivalente:
   npm run build
   ```
3. Probá localmente abriendo `index.html` en el navegador (o con un servidor local).

## Cómo agregar una pestaña/sección nueva

1. Creá el archivo nuevo en `html-src/` (ej: `html-src/reportes.html`).
2. Agregá el marcador `<!--INCLUDE:reportes.html-->` en `html-src/layout.html`, en el
   lugar donde tiene que aparecer.
3. Corré `node build-html.js` para confirmar que arma bien.

## Deploy en Netlify

No hace falta correr el build a mano antes de subir a GitHub — **Netlify lo corre
solo**, configurado en `netlify.toml`:

```toml
[build]
  command = "node build-html.js"
```

Cuando hacés push, Netlify: 1) corre `node build-html.js` (regenera `index.html`
fresco a partir de `html-src/`), 2) publica el sitio con ese `index.html` ya armado.
El comportamiento para quien usa la app no cambia en nada — sigue siendo un solo
HTML servido de una sola vez, sin pedidos extra al navegar entre pestañas.

## Por qué se hizo así (y no con fetch() en el navegador)

Se evaluaron 3 caminos:

1. **Multi-página real** (cada sección un `.html` con links): se descartó porque
   perdía el estado de sesión/login y los datos ya cargados en memoria al navegar
   entre páginas.
2. **Fragmentos cargados en vivo con `fetch()`** en el navegador: agregaba una demora
   chica al cambiar de pestaña y complejidad para manejar el estado de carga.
3. **Este enfoque (build-time)**: el split solo existe como código fuente; en
   producción sigue siendo un único HTML, sin cambios de comportamiento ni riesgo
   para el login/sesión ya armados.

```

### Archivo: `html-src/dashboard.html`
```html
    <!-- ==================== DASHBOARD ==================== -->
    <div id="tab-dashboard" class="tab-content">
      <div class="stats-grid" id="dash-stats"></div>
      <div class="section" style="margin-top:1.5rem;">
        <h2 class="section-title">🕐 Últimos registros</h2>
        <div class="grid-2">
          <div>
            <h3 style="font-size:0.9rem; color:#64748b; margin-bottom:0.5rem; text-transform:uppercase;">Últimos Llamados</h3>
            <div class="table-wrap"><table><thead><tr><th>ID</th><th>Nombre</th><th>Cargo</th><th>Empresa</th><th>Fecha</th></tr></thead><tbody id="dash-ult-llamados"></tbody></table></div>
          </div>
          <div>
            <h3 style="font-size:0.9rem; color:#64748b; margin-bottom:0.5rem; text-transform:uppercase;">Últimos Postulantes</h3>
            <div class="table-wrap"><table><thead><tr><th>ID</th><th>Nombre</th><th>Apellido</th><th>Email</th></tr></thead><tbody id="dash-ult-postulantes"></tbody></table></div>
          </div>
        </div>
      </div>
    </div>

```

### Archivo: `html-src/empresas.html`
```html
    <!-- ==================== EMPRESAS ==================== -->
    <div id="tab-empresas" class="tab-content hidden">
      <div class="grid-2">
        <div class="section">
          <h2 class="section-title">➕ Nueva Empresa</h2>
          <label class="label">Nombre *</label>
          <input type="text" id="empNombre" class="input" placeholder="Ej: ACME S.A.">
          <label class="label">Rubro / Detalle</label>
          <input type="text" id="empRubro" class="input" placeholder="Ej: Tecnología">
          <p id="empWarning" class="hidden warning-box"></p>
          <div style="margin-top:1.25rem;">
            <button onclick="guardarEmpresa()" class="btn btn-success">💾 Guardar Empresa</button>
          </div>
        </div>
        <div class="section">
          <h2 class="section-title">📋 Empresas registradas</h2>
          <div class="table-wrap">
            <table>
              <thead><tr><th>ID</th><th>Nombre</th><th>Rubro</th><th>Llamados</th></tr></thead>
              <tbody id="tablaEmpresas"></tbody>
            </table>
          </div>
          <div class="flex justify-center items-center gap-2 mt-2 text-sm">
            <button onclick="cambiarPag('empresa', -1)" class="btn btn-sm btn-secondary">← Ant</button>
            <span id="infoPagEmpresa" class="text-gray font-bold">Página 1 de 1</span>
            <button onclick="cambiarPag('empresa', 1)" class="btn btn-sm btn-secondary">Sig →</button>
            <select id="tamPagEmpresa" onchange="cambiarTamPag('empresa')" class="input" style="width:auto;padding:0.4rem;">
              <option value="10">10</option>
              <option value="25" selected>25</option>
              <option value="50">50</option>
              <option value="100">100</option>
            </select>
          </div>
        </div>
      </div>
    </div>

```

### Archivo: `html-src/layout.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SM Consultores</title>
  <link rel="stylesheet" href="./css/styles.css">
<base target="_blank">
</head>
<body>
  <!-- ==================== LOGIN ==================== -->
  <!--INCLUDE:login.html-->

  <!-- ==================== APP ==================== -->
  <div id="appContainer" class="container hidden">
    <div class="app-header">
      <h1>SM Consultores</h1>
      <p>Gestión de Empresas, Llamados y Postulantes</p>
      <button onclick="logout()" class="btn btn-sm btn-secondary" style="position:absolute; top:1rem; right:1rem;">Salir</button>
    </div>

    <div class="tabs">
      <button id="btn-dashboard" class="tab-btn active" onclick="switchTab('dashboard')">📊 Dashboard</button>
      <button id="btn-empresas" class="tab-btn" onclick="switchTab('empresas')">🏢 Empresas</button>
      <button id="btn-llamados" class="tab-btn" onclick="switchTab('llamados')">📢 Llamados</button>
      <button id="btn-postulantes" class="tab-btn" onclick="switchTab('postulantes')">👤 Postulantes</button>
    </div>

    <!--INCLUDE:dashboard.html-->
    <!--INCLUDE:empresas.html-->
    <!--INCLUDE:llamados.html-->
    <!--INCLUDE:postulantes.html-->
  </div>

  <!-- MODALES -->
  <!--INCLUDE:modales.html-->

  <script src="./js/app.js"></script>
</body>
</html>

```

### Archivo: `html-src/llamados.html`
```html
    <!-- ==================== LLAMADOS ==================== -->
    <div id="tab-llamados" class="tab-content hidden">
      <div class="grid-2">
        <div class="section">
          <h2 class="section-title">➕ Nuevo Llamado</h2>
          <label class="label">Empresa *</label>
          <select id="llamEmpresa" class="input"><option value="">-- Seleccionar empresa --</option></select>
          <label class="label">Nombre del Llamado *</label>
          <input type="text" id="llamNombre" class="input" placeholder="Ej: Búsqueda de Desarrollador">
          <div class="grid-2">
            <div>
              <label class="label">Fecha *</label>
              <input type="date" id="llamFecha" class="input">
            </div>
            <div>
              <label class="label">Cargo *</label>
              <input type="text" id="llamCargo" class="input" placeholder="Ej: Desarrollador">
            </div>
          </div>
          <label class="label">Área *</label>
          <input type="text" id="llamArea" class="input" placeholder="Ej: Sistemas">
          <label class="label">Observaciones</label>
          <textarea id="llamObservaciones" class="input" rows="2" placeholder="Ej: Publicado en LinkedIn, Bumeran y grupo de Facebook 'Empleos Montevideo'"></textarea>
          <div style="margin-top:1.25rem;">
            <button onclick="guardarLlamado()" class="btn btn-success">💾 Guardar Llamado</button>
          </div>
        </div>
        <div class="section">
          <h2 class="section-title">📋 Llamados registrados</h2>
          <div class="search-bar">
            <select id="filtroEmpresaLlam" class="input" style="width:auto;min-width:200px;" onchange="filtrarLlamados()"><option value="">Todas las empresas</option></select>
            <input type="text" id="buscarLlamado" class="input" placeholder="Buscar..." oninput="filtrarLlamados()">
          </div>
          <div class="table-wrap">
            <table>
              <thead><tr><th>ID</th><th>Nombre</th><th>Cargo</th><th>Área</th><th>Empresa</th><th>Fecha</th><th>Post.</th><th></th></tr></thead>
              <tbody id="tablaLlamados"></tbody>
            </table>
          </div>
          <div class="flex justify-center items-center gap-2 mt-2 text-sm">
            <button onclick="cambiarPag('llamado', -1)" class="btn btn-sm btn-secondary">← Ant</button>
            <span id="infoPagLlamado" class="text-gray font-bold">Página 1 de 1</span>
            <button onclick="cambiarPag('llamado', 1)" class="btn btn-sm btn-secondary">Sig →</button>
            <select id="tamPagLlamado" onchange="cambiarTamPag('llamado')" class="input" style="width:auto;padding:0.4rem;">
              <option value="10">10</option>
              <option value="25" selected>25</option>
              <option value="50">50</option>
              <option value="100">100</option>
            </select>
          </div>
        </div>
      </div>
    </div>

```

### Archivo: `html-src/login.html`
```html
  <div id="pantallaLogin" class="container" style="max-width:400px;">
    <div class="app-header">
      <h1>SM Consultores</h1>
      <p>Iniciar sesión</p>
    </div>
    <div class="section">
      <label class="label">Usuario</label>
      <input type="text" id="loginUsuario" class="input" placeholder="Usuario" autocomplete="username">
      <label class="label">Contraseña</label>
      <input type="password" id="loginPassword" class="input" placeholder="Contraseña" autocomplete="current-password" onkeydown="if(event.key==='Enter') login()">
      <p id="loginError" class="hidden warning-box"></p>
      <div style="margin-top:1.25rem;">
        <button onclick="login()" class="btn btn-success" style="width:100%;">🔒 Ingresar</button>
      </div>
    </div>
  </div>

```

### Archivo: `html-src/modales.html`
```html
  <div id="modalDuplicados" class="modal-overlay hidden">
    <div class="modal-box">
      <div class="modal-header">
        <div>
          <h2 style="font-size:1.4rem;font-weight:800;color:#dc2626;">⚠️ Duplicados detectados</h2>
          <p class="text-gray text-sm mt-1"><strong id="dupCantidad">0</strong> postulantes ya existen.</p>
        </div>
        <button onclick="cerrarModalDuplicados()" class="modal-close">×</button>
      </div>
      <div class="bg-gray-50 rounded p-3 mb-4 flex items-center gap-3 flex-wrap">
        <span class="font-bold text-sm">Aplicar a todos:</span>
        <button onclick="aplicarATodos('ignorar')" class="btn btn-secondary btn-sm">🚫 Ignorar</button>
        <button onclick="aplicarATodos('actualizar')" class="btn btn-warning btn-sm">🔄 Actualizar</button>
        <button onclick="aplicarATodos('nuevo')" class="btn btn-primary btn-sm">➕ Nuevo</button>
      </div>
      <div style="overflow-x:auto;max-height:50vh;">
        <table><thead><tr><th>#</th><th>Nombre</th><th>Existente</th><th>Nuevos (CSV)</th><th>Acción</th><th>Estado</th></tr></thead><tbody id="dupTablaBody"></tbody></table>
      </div>
      <div class="flex justify-end gap-3" style="margin-top:1.5rem;">
        <button onclick="cerrarModalDuplicados()" class="btn btn-secondary">Cancelar</button>
        <button onclick="confirmarDuplicados()" class="btn btn-success" style="font-weight:700;">✅ Confirmar y guardar</button>
      </div>
    </div>
  </div>

  <div id="modalCargando" class="modal-overlay hidden">
    <div style="background:white;padding:2.5rem;border-radius:1rem;text-align:center;box-shadow:0 25px 60px rgba(0,0,0,0.2);">
      <div style="font-size:3rem;margin-bottom:0.75rem;animation:spin 1s linear infinite;">⏳</div>
      <h3 style="font-weight:800;font-size:1.1rem;color:#1e293b;">Guardando en GitHub...</h3>
      <p class="text-gray text-sm mt-2">No cierres esta ventana.</p>
    </div>
  </div>
  <style>@keyframes spin{from{transform:rotate(0)}to{transform:rotate(360deg)}}</style>

  <div id="modalLlamado" class="modal-overlay hidden">
    <div class="modal-box">
      <div class="modal-header">
        <h2 id="modalLlamadoTitulo" style="font-size:1.1rem;font-weight:800;color:#1e3a5f;"></h2>
        <button onclick="cerrarModalLlamado()" class="modal-close">×</button>
      </div>
      <div id="modalLlamadoBody"></div>
    </div>
  </div>

  <div id="modalPostulante" class="modal-overlay hidden">
    <div class="modal-box">
      <div class="modal-header">
        <h2 id="modalPostulanteTitulo" style="font-size:1.1rem;font-weight:800;color:#1e3a5f;"></h2>
        <button onclick="cerrarModalPostulante()" class="modal-close">×</button>
      </div>
      <div id="modalPostulanteBody"></div>
    </div>
  </div>

  <div id="modalResultados" class="modal-overlay hidden">
    <div class="modal-box" style="max-width:500px;">
      <div class="modal-header">
        <h2 id="modalResultadosTitulo" style="font-size:1.1rem;font-weight:800;color:#1e3a5f;"></h2>
        <button onclick="cerrarModalResultados()" class="modal-close">×</button>
      </div>
      <div style="padding:0.5rem 0;">
        <label class="label">Estado</label>
        <select id="resEstado" class="input">
          <option value="pendiente">⏳ Pendiente</option>
          <option value="recomendado">✅ Recomendado</option>
          <option value="observado">⚠️ Observado</option>
          <option value="rechazado">❌ Rechazado</option>
        </select>

        <label class="label" style="margin-top:1rem;">Psicolaboral</label>
        <label class="label" style="margin-top:0;font-weight:400;font-size:0.8rem;">Fecha</label>
        <input type="date" id="resFechaPsicolaboral" class="input" style="max-width:200px;">
        <textarea id="resPsicolaboral" class="input" rows="3" placeholder="Resultado / notas de la entrevista psicolaboral" style="margin-top:0.5rem;"></textarea>

        <label class="label" style="margin-top:1rem;">Prueba de conocimiento</label>
        <label class="label" style="margin-top:0;font-weight:400;font-size:0.8rem;">Fecha</label>
        <input type="date" id="resFechaPrueba" class="input" style="max-width:200px;">
        <textarea id="resPrueba" class="input" rows="3" placeholder="Resultado / notas de la prueba de conocimiento" style="margin-top:0.5rem;"></textarea>

        <div class="flex gap-2" style="margin-top:1.25rem;">
          <button onclick="guardarResultados()" class="btn btn-success">💾 Guardar</button>
          <button onclick="cerrarModalResultados()" class="btn btn-secondary">Cancelar</button>
        </div>
      </div>
    </div>
  </div>

```

### Archivo: `html-src/postulantes.html`
```html
    <!-- ==================== POSTULANTES ==================== -->
    <div id="tab-postulantes" class="tab-content hidden">
      <div class="section">
        <h2 class="section-title">📄 Cargar Postulantes desde CSV</h2>
        <div class="grid-2">
          <div>
            <label class="label">Archivo CSV</label>
            <input type="file" id="csvFile" accept=".csv" class="input" onchange="handleCSV()">
            <div id="avisoArchivoRepetido" class="hidden warning-box" style="margin-top:1rem;">
              <p id="avisoArchivoTexto"></p>
              <div class="flex gap-2" style="margin-top:0.75rem;">
                <button onclick="continuarConArchivoRepetido()" class="btn btn-warning btn-sm">Continuar igual</button>
                <button onclick="cancelarArchivoRepetido()" class="btn btn-secondary btn-sm">Cancelar</button>
              </div>
            </div>
            <div id="csvPreviewBox" class="hidden" style="margin-top:1rem;">
              <p class="text-gray text-sm mb-2">Mapeo de columnas:</p>
              <div class="grid-4" id="columnMap"></div>
              <div class="flex gap-2 mt-2">
                <input type="text" id="newFieldName" class="input" style="width:auto;min-width:180px;" placeholder="Campo extra">
                <button onclick="addExtraField()" class="btn btn-secondary btn-sm">Agregar</button>
              </div>
              <div id="extraFields" class="flex flex-wrap gap-2 mt-2"></div>
            </div>
          </div>
          <div>
            <label class="label">Asociar a llamado (opcional)</label>
            <select id="postuLlamado" class="input"><option value="">-- Sin llamado --</option></select>
            <div id="postuLlamadoInfo" class="hidden info-box mt-2"></div>
          </div>
        </div>
        <div id="csvTableWrap" class="hidden table-wrap mt-4">
          <table><thead id="previewHead"></thead><tbody id="previewBody"></tbody></table>
          <p class="text-xs text-gray mt-2" id="previewCount"></p>
        </div>
        <div class="flex justify-center mt-4">
          <button onclick="guardarPostulantesCSV()" class="btn btn-success btn-lg">💾 Guardar Postulantes</button>
        </div>
      </div>

      <div class="section">
        <h2 class="section-title">👤 Postulantes registrados</h2>
        <div class="search-bar">
          <input type="text" id="buscarPostulante" class="input" placeholder="Buscar por nombre, apellido, email o teléfono..." oninput="filtrarPostulantes()">
        </div>
        <div class="table-wrap">
          <table>
            <thead><tr><th>ID</th><th>Nombre</th><th>Apellido</th><th>Email</th><th>Teléfono</th><th>Llamados</th><th></th></tr></thead>
            <tbody id="tablaPostulantes"></tbody>
          </table>
        </div>
        <div class="flex justify-center items-center gap-2 mt-2 text-sm">
          <button onclick="cambiarPag('postulante', -1)" class="btn btn-sm btn-secondary">← Ant</button>
          <span id="infoPagPostulante" class="text-gray font-bold">Página 1 de 1</span>
          <button onclick="cambiarPag('postulante', 1)" class="btn btn-sm btn-secondary">Sig →</button>
          <select id="tamPagPostulante" onchange="cambiarTamPag('postulante')" class="input" style="width:auto;padding:0.4rem;">
            <option value="10">10</option>
            <option value="25" selected>25</option>
            <option value="50">50</option>
            <option value="100">100</option>
          </select>
        </div>
      </div>
    </div>

```

### Archivo: `index.html`
```html

```

### Archivo: `js/app.js`
```javascript
const API = '/.netlify/functions';

// ========== SESIÓN ==========
let authToken = sessionStorage.getItem('sm_token') || null;

async function login() {
  const usuario = document.getElementById('loginUsuario').value.trim();
  const password = document.getElementById('loginPassword').value;
  const errEl = document.getElementById('loginError');
  errEl.classList.add('hidden');

  if (!usuario || !password) {
    errEl.textContent = 'Completá usuario y contraseña';
    errEl.classList.remove('hidden');
    return;
  }

  try {
    const res = await fetch(`${API}/login`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ usuario, password })
    });
    const body = await res.json();
    if (!res.ok) {
      errEl.textContent = body.error || 'No se pudo iniciar sesión';
      errEl.classList.remove('hidden');
      return;
    }
    authToken = body.token;
    sessionStorage.setItem('sm_token', authToken);
    sessionStorage.setItem('sm_usuario', body.usuario);
    mostrarApp();
  } catch (e) {
    errEl.textContent = 'Error de conexión';
    errEl.classList.remove('hidden');
  }
}

function logout() {
  authToken = null;
  sessionStorage.removeItem('sm_token');
  sessionStorage.removeItem('sm_usuario');
  document.getElementById('appContainer').classList.add('hidden');
  document.getElementById('pantallaLogin').classList.remove('hidden');
  document.getElementById('loginPassword').value = '';
}

function mostrarApp() {
  document.getElementById('pantallaLogin').classList.add('hidden');
  document.getElementById('appContainer').classList.remove('hidden');
  cargarTablas();
  document.getElementById('llamFecha').valueAsDate = new Date();
  switchTab('dashboard');
}

// Wrapper de fetch que agrega el token de sesión y maneja el 401 (sesión vencida/ inválida)
async function apiFetch(path, options = {}) {
  const headers = { ...(options.headers || {}), 'Authorization': `Bearer ${authToken}` };
  const res = await fetch(`${API}${path}`, { ...options, headers });
  if (res.status === 401) {
    logout();
    throw new Error('Sesión vencida, iniciá sesión de nuevo');
  }
  return res;
}

let tablas = { empresa: [], llamado: [], postulante: [], 'llamado-empresa': [], 'llamado-postulante': [], archivosImportados: [] };
let csvData = [], csvHeaders = [], columnMapping = {}, extraFields = [];
let duplicadosLista = [];
let relacionesPendientes = [];
let llamadoIdParaPostulantes = null;

// ========== PAGINACIÓN GLOBAL ==========
const paginacion = {
  empresa:    { pag: 1, tam: 25, total: 0 },
  llamado:    { pag: 1, tam: 25, total: 0 },
  postulante: { pag: 1, tam: 25, total: 0 }
};

function paginarArray(arr, tipo) {
  const p = paginacion[tipo];
  p.total = Math.max(1, Math.ceil(arr.length / p.tam));
  if (p.pag > p.total) p.pag = p.total;
  if (p.pag < 1) p.pag = 1;
  const inicio = (p.pag - 1) * p.tam;
  return arr.slice(inicio, inicio + p.tam);
}

function actualizarInfoPag(tipo, mostrados, total) {
  const id = 'infoPag' + tipo.charAt(0).toUpperCase() + tipo.slice(1);
  const el = document.getElementById(id);
  if (el) el.textContent = `Pág ${paginacion[tipo].pag} de ${paginacion[tipo].total} (${total} total)`;
}

function cambiarPag(tipo, delta) {
  paginacion[tipo].pag += delta;
  if (tipo === 'empresa') renderEmpresas();
  if (tipo === 'llamado') renderLlamados();
  if (tipo === 'postulante') renderPostulantes();
}

function cambiarTamPag(tipo) {
  const selectId = 'tamPag' + tipo.charAt(0).toUpperCase() + tipo.slice(1);
  const val = document.getElementById(selectId)?.value || 25;
  paginacion[tipo].tam = parseInt(val);
  paginacion[tipo].pag = 1;
  if (tipo === 'empresa') renderEmpresas();
  if (tipo === 'llamado') renderLlamados();
  if (tipo === 'postulante') renderPostulantes();
}

// ========== INICIO ==========
document.addEventListener('DOMContentLoaded', () => {
  if (authToken) {
    mostrarApp();
  } else {
    document.getElementById('pantallaLogin').classList.remove('hidden');
  }
});

// ========== TABS ==========
function switchTab(name) {
  document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
  document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
  document.getElementById('tab-' + name).classList.remove('hidden');
  document.getElementById('btn-' + name).classList.add('active');
  if (name === 'dashboard') renderDashboard();
  if (name === 'empresas') renderEmpresas();
  if (name === 'llamados') renderLlamados();
  if (name === 'postulantes') renderPostulantes();
}

// ========== CARGAR TABLAS ==========
async function cargarTablas() {
  const entries = await Promise.all(
    Object.keys(tablas).map(async (t) => {
      try {
        const res = await apiFetch(`/get-table?table=${t}`);
        return [t, await res.json()];
      } catch (e) {
        return [t, []];
      }
    })
  );
  tablas = Object.fromEntries(entries);
  llenarSelectsEmpresa();
  llenarSelectLlamados();
}

async function subirTabla(nombre) {
  const res = await apiFetch(`/update-table`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ table: nombre, data: tablas[nombre] })
  });
  if (!res.ok) {
    const err = await res.json();
    throw new Error(`${nombre}: ${JSON.stringify(err)}`);
  }
}

async function actualizarStats() {
  try {
    await apiFetch(`/update-stats`, { method: 'POST' });
  } catch (e) {
    // Silencioso: si falla, el dashboard usa fallback local
  }
}

function generarId(tabla) {
  const arr = tablas[tabla];
  return arr.length > 0 ? Math.max(...arr.map(x => x.id)) + 1 : 1;
}

function normalizar(t) { return (t || '').toLowerCase().trim().replace(/\s+/g, ' '); }

// ========== LLENAR SELECTS ==========
function llenarSelectsEmpresa() {
  ['empresaSelect', 'llamEmpresa', 'filtroEmpresaLlam'].forEach(id => {
    const sel = document.getElementById(id);
    if (!sel) return;
    const prev = sel.value;
    const firstOpt = sel.options[0] ? sel.options[0].outerHTML : '';
    sel.innerHTML = firstOpt;
    tablas.empresa.forEach(e => {
      sel.innerHTML += `<option value="${e.id}">${e.nombre}</option>`;
    });
    sel.value = prev;
  });
}

function llenarSelectLlamados() {
  const sel = document.getElementById('postuLlamado');
  if (!sel) return;
  const prev = sel.value;
  sel.innerHTML = '<option value="">-- Sin llamado --</option>';
  tablas.llamado.forEach(l => {
    const emp = tablas.empresa.find(e => e.id === l.empresa_id);
    sel.innerHTML += `<option value="${l.id}">#${l.id} - ${l.nombre_llamado} (${l.cargo})${emp ? ' | ' + emp.nombre : ''}</option>`;
  });
  sel.value = prev;
}

// ============================================================
// EMPRESAS
// ============================================================
function renderEmpresas() {
  let lista = [...tablas.empresa];
  const paginada = paginarArray(lista, 'empresa');
  const tbody = document.getElementById('tablaEmpresas');
  tbody.innerHTML = paginada.map(e => {
    const count = tablas.llamado.filter(l => l.empresa_id === e.id).length;
    return `<tr>
      <td>#${e.id}</td>
      <td><strong>${e.nombre}</strong></td>
      <td>${e.rubro || '-'}</td>
      <td><span class="badge badge-blue">${count}</span></td>
    </tr>`;
  }).join('');
  actualizarInfoPag('empresa', paginada.length, lista.length);
}

async function guardarEmpresa() {
  const nombre = document.getElementById('empNombre').value.trim();
  const rubro = document.getElementById('empRubro').value.trim();
  if (!nombre) return alert('Ingresa el nombre de la empresa.');

  const existe = tablas.empresa.find(e => normalizar(e.nombre) === normalizar(nombre));
  if (existe) {
    document.getElementById('empWarning').classList.remove('hidden');
    document.getElementById('empWarning').textContent = `⚠️ La empresa "${existe.nombre}" ya existe.`;
    return;
  }

  document.getElementById('empWarning').classList.add('hidden');
  document.getElementById('modalCargando').classList.remove('hidden');

  tablas.empresa.push({ id: generarId('empresa'), nombre, rubro });
  await subirTabla('empresa');
  await actualizarStats();

  document.getElementById('empNombre').value = '';
  document.getElementById('empRubro').value = '';
  llenarSelectsEmpresa();
  renderEmpresas();
  document.getElementById('modalCargando').classList.add('hidden');
  alert('Empresa guardada correctamente.');
}

// ============================================================
// LLAMADOS
// ============================================================
function renderLlamados() {
  filtrarLlamados();
}

function filtrarLlamados() {
  const empresaId = document.getElementById('filtroEmpresaLlam').value;
  const busqueda = normalizar(document.getElementById('buscarLlamado').value);

  let lista = tablas.llamado;
  if (empresaId) lista = lista.filter(l => l.empresa_id == empresaId);
  if (busqueda) lista = lista.filter(l =>
    normalizar(l.nombre_llamado).includes(busqueda) ||
    normalizar(l.cargo).includes(busqueda) ||
    normalizar(l.area).includes(busqueda)
  );

  const paginada = paginarArray(lista, 'llamado');
  const tbody = document.getElementById('tablaLlamados');
  tbody.innerHTML = paginada.map(l => {
    const emp = tablas.empresa.find(e => e.id === l.empresa_id);
    const count = tablas['llamado-postulante'].filter(r => r.llamado_id === l.id).length;
    return `<tr>
      <td>#${l.id}</td>
      <td>${l.nombre_llamado}</td>
      <td>${l.cargo}</td>
      <td>${l.area}</td>
      <td>${emp ? emp.nombre : '-'}</td>
      <td>${l.fecha}</td>
      <td><span class="badge badge-green">${count}</span></td>
      <td><button class="btn btn-primary btn-sm" onclick="verLlamado(${l.id})">Ver</button></td>
    </tr>`;
  }).join('');
  actualizarInfoPag('llamado', paginada.length, lista.length);
}

async function guardarLlamado() {
  const empresaId = document.getElementById('llamEmpresa').value;
  const nombre = document.getElementById('llamNombre').value.trim();
  const fecha = document.getElementById('llamFecha').value;
  const cargo = document.getElementById('llamCargo').value.trim();
  const area = document.getElementById('llamArea').value.trim();
  const observaciones = document.getElementById('llamObservaciones').value.trim();

  if (!empresaId) return alert('Selecciona una empresa.');
  if (!nombre || !fecha || !cargo || !area) return alert('Completa todos los campos.');

  document.getElementById('modalCargando').classList.remove('hidden');

  const id = generarId('llamado');
  tablas.llamado.push({ id, nombre_llamado: nombre, fecha, cargo, area, observaciones, empresa_id: parseInt(empresaId) });
  tablas['llamado-empresa'].push({ llamado_id: id, empresa_id: parseInt(empresaId) });

  await subirTabla('llamado');
  await subirTabla('llamado-empresa');
  await actualizarStats();

  document.getElementById('llamNombre').value = '';
  document.getElementById('llamCargo').value = '';
  document.getElementById('llamArea').value = '';
  document.getElementById('llamObservaciones').value = '';
  llenarSelectLlamados();
  renderLlamados();
  document.getElementById('modalCargando').classList.add('hidden');
  alert('Llamado guardado correctamente.');
}

function verLlamado(id) {
  const l = tablas.llamado.find(x => x.id === id);
  if (!l) return;
  const emp = tablas.empresa.find(e => e.id === l.empresa_id);
  const rels = tablas['llamado-postulante'].filter(r => r.llamado_id === id);
  const postulantes = rels.map(r => tablas.postulante.find(p => p.id === r.postulante_id)).filter(Boolean);

  document.getElementById('modalLlamadoTitulo').textContent = `Llamado #${l.id}`;
  document.getElementById('modalLlamadoBody').innerHTML = `
    <div class="detail-grid">
      <div class="detail-item"><dt>Nombre</dt><dd>${l.nombre_llamado}</dd></div>
      <div class="detail-item"><dt>Cargo</dt><dd>${l.cargo}</dd></div>
      <div class="detail-item"><dt>Área</dt><dd>${l.area}</dd></div>
      <div class="detail-item"><dt>Fecha</dt><dd>${l.fecha}</dd></div>
      <div class="detail-item"><dt>Empresa</dt><dd>${emp ? emp.nombre : '-'}</dd></div>
    </div>
    ${l.observaciones ? `<div style="margin-top:1rem;"><div class="label">Observaciones</div><div style="white-space:pre-wrap;">${l.observaciones}</div></div>` : ''}
    <h3 style="margin-top:1.5rem;margin-bottom:0.75rem;font-size:1rem;font-weight:700;">Postulantes (${postulantes.length})</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>ID</th><th>Nombre</th><th>Apellido</th><th>Email</th><th>Teléfono</th></tr></thead>
        <tbody>${postulantes.map(p => `<tr><td>#${p.id}</td><td>${p.nombre}</td><td>${p.apellido}</td><td>${p.email || '-'}</td><td>${p.telefono || '-'}</td></tr>`).join('')}</tbody>
      </table>
    </div>
  `;
  document.getElementById('modalLlamado').classList.remove('hidden');
}

function cerrarModalLlamado() {
  document.getElementById('modalLlamado').classList.add('hidden');
}

// ============================================================
// POSTULANTES
// ============================================================
function renderPostulantes() {
  filtrarPostulantes();
}

function filtrarPostulantes() {
  const busqueda = normalizar(document.getElementById('buscarPostulante').value);
  let lista = tablas.postulante;
  if (busqueda) {
    lista = lista.filter(p =>
      normalizar(p.nombre).includes(busqueda) ||
      normalizar(p.apellido).includes(busqueda) ||
      normalizar(p.email).includes(busqueda) ||
      normalizar(p.telefono).includes(busqueda)
    );
  }
  const paginada = paginarArray(lista, 'postulante');
  const tbody = document.getElementById('tablaPostulantes');
  tbody.innerHTML = paginada.map(p => {
    const rels = tablas['llamado-postulante'].filter(r => r.postulante_id === p.id);
    return `<tr>
      <td>#${p.id}</td>
      <td>${p.nombre}</td>
      <td>${p.apellido}</td>
      <td>${p.email || '-'}</td>
      <td>${p.telefono || '-'}</td>
      <td><span class="badge badge-green">${rels.length}</span></td>
      <td><button class="btn btn-primary btn-sm" onclick="verPostulante(${p.id})">Ver</button></td>
    </tr>`;
  }).join('');
  actualizarInfoPag('postulante', paginada.length, lista.length);
}

// ========== CONTROL DE ARCHIVOS YA IMPORTADOS ==========
let nombreArchivoActual = null;
let textoCSVPendiente = null;

function normalizarNombreArchivo(nombre) {
  return (nombre || '').trim().toLowerCase();
}

function buscarArchivoImportado(nombre) {
  const clave = normalizarNombreArchivo(nombre);
  return tablas.archivosImportados.find(a => normalizarNombreArchivo(a.archivo) === clave);
}

function handleCSV() {
  const file = document.getElementById('csvFile').files[0];
  if (!file) return;

  nombreArchivoActual = file.name;
  const yaImportado = buscarArchivoImportado(file.name);

  const reader = new FileReader();
  reader.onload = (e) => {
    if (yaImportado) {
      textoCSVPendiente = e.target.result;
      mostrarAvisoArchivoRepetido(yaImportado);
    } else {
      parseCSV(e.target.result);
    }
  };
  reader.readAsText(file);
}

function mostrarAvisoArchivoRepetido(info) {
  const fecha = new Date(info.fecha).toLocaleString('es-AR');
  document.getElementById('avisoArchivoTexto').textContent =
    `⚠️ El archivo "${nombreArchivoActual}" ya fue importado el ${fecha} por ${info.usuario} (${info.cantidadRegistros} filas procesadas). ¿Querés continuar de todas formas?`;
  document.getElementById('avisoArchivoRepetido').classList.remove('hidden');
  document.getElementById('csvPreviewBox').classList.add('hidden');
  document.getElementById('csvTableWrap').classList.add('hidden');
}

function cancelarArchivoRepetido() {
  textoCSVPendiente = null;
  nombreArchivoActual = null;
  document.getElementById('avisoArchivoRepetido').classList.add('hidden');
  document.getElementById('csvFile').value = '';
}

function continuarConArchivoRepetido() {
  document.getElementById('avisoArchivoRepetido').classList.add('hidden');
  parseCSV(textoCSVPendiente);
  textoCSVPendiente = null;
}

function registrarArchivoImportado(cantidadFilas) {
  if (!nombreArchivoActual) return;
  tablas.archivosImportados.push({
    archivo: nombreArchivoActual,
    fecha: new Date().toISOString(),
    usuario: sessionStorage.getItem('sm_usuario') || 'desconocido',
    cantidadRegistros: cantidadFilas
  });
  nombreArchivoActual = null;
}


function parseCSV(text) {
  const lines = text.split(/\r?\n/).filter(l => l.trim());
  if (lines.length < 2) return alert('CSV vacío o inválido');
  const sep = lines[0].includes(';') ? ';' : ',';
  csvHeaders = lines[0].split(sep).map(h => h.trim().replace(/^"|"$/g, ''));
  csvData = lines.slice(1).map(line => {
    const vals = line.split(sep).map(v => v.trim().replace(/^"|"$/g, ''));
    const row = {};
    csvHeaders.forEach((h, i) => row[h] = vals[i] || '');
    return row;
  });

  columnMapping = {};
  ['nombre', 'apellido', 'email', 'telefono'].forEach(campo => {
    const match = csvHeaders.find(h => h.toLowerCase().includes(campo));
    if (match) columnMapping[campo] = match;
  });

  renderColumnMap();
  renderPreview();
  document.getElementById('csvPreviewBox').classList.remove('hidden');
  document.getElementById('csvTableWrap').classList.remove('hidden');
}

function renderColumnMap() {
  const box = document.getElementById('columnMap');
  const campos = ['nombre', 'apellido', 'email', 'telefono'];
  box.innerHTML = campos.map(campo => `
    <div>
      <label class="label" style="margin-top:0;">${campo.toUpperCase()}</label>
      <select id="map-${campo}" class="input" onchange="columnMapping['${campo}']=this.value; renderPreview()">
        <option value="">-- Ignorar --</option>
        ${csvHeaders.map(h => `<option value="${h}" ${columnMapping[campo] === h ? 'selected' : ''}>${h}</option>`).join('')}
      </select>
    </div>
  `).join('');
}

function addExtraField() {
  const name = document.getElementById('newFieldName').value.trim();
  if (!name || extraFields.includes(name)) return;
  extraFields.push(name);
  document.getElementById('newFieldName').value = '';
  renderExtraFields();
}

function renderExtraFields() {
  const box = document.getElementById('extraFields');
  box.innerHTML = extraFields.map(f => `
    <div class="field-tag">${f} <button onclick="removeExtraField('${f}')">×</button></div>
  `).join('');
}

function removeExtraField(f) {
  extraFields = extraFields.filter(x => x !== f);
  renderExtraFields();
}

function renderPreview() {
  const thead = document.getElementById('previewHead');
  const tbody = document.getElementById('previewBody');
  const displayCols = ['nombre', 'apellido', 'email', 'telefono', ...extraFields].filter(c => columnMapping[c] || extraFields.includes(c));
  thead.innerHTML = '<tr>' + displayCols.map(c => `<th>${c}</th>`).join('') + '</tr>';
  tbody.innerHTML = csvData.slice(0, 5).map(row => {
    return '<tr>' + displayCols.map(c => {
      const val = columnMapping[c] ? row[columnMapping[c]] : (extraFields.includes(c) ? row[c] || '' : '');
      return `<td>${val}</td>`;
    }).join('') + '</tr>';
  }).join('');
  document.getElementById('previewCount').textContent = `Mostrando 5 de ${csvData.length} registros.`;
}

function construirPostulanteObj(row, id) {
  const obj = {
    id,
    nombre: columnMapping.nombre ? row[columnMapping.nombre] : '',
    apellido: columnMapping.apellido ? row[columnMapping.apellido] : '',
    email: columnMapping.email ? row[columnMapping.email] : '',
    telefono: columnMapping.telefono ? row[columnMapping.telefono] : ''
  };
  extraFields.forEach(f => { if (row[f] !== undefined) obj[f] = row[f]; });
  return obj;
}

function buscarDuplicado(nombre, apellido) {
  return tablas.postulante.find(p => normalizar(p.nombre) === normalizar(nombre) && normalizar(p.apellido) === normalizar(apellido));
}

async function guardarPostulantesCSV() {
  if (csvData.length === 0) return alert('Carga un archivo CSV primero.');

  llamadoIdParaPostulantes = document.getElementById('postuLlamado').value;
  if (llamadoIdParaPostulantes) llamadoIdParaPostulantes = parseInt(llamadoIdParaPostulantes);

  duplicadosLista = [];
  relacionesPendientes = [];
  const noDuplicados = [];

  for (const row of csvData) {
    const nom = columnMapping.nombre ? row[columnMapping.nombre] : '';
    const ape = columnMapping.apellido ? row[columnMapping.apellido] : '';
    const existente = buscarDuplicado(nom, ape);
    if (existente) {
      duplicadosLista.push({ row, existente, accion: 'ignorar' });
    } else {
      noDuplicados.push(row);
    }
  }

  noDuplicados.forEach(row => {
    const nuevo = construirPostulanteObj(row, generarId('postulante'));
    tablas.postulante.push(nuevo);
    if (llamadoIdParaPostulantes) {
      relacionesPendientes.push({ llamado_id: llamadoIdParaPostulantes, postulante_id: nuevo.id });
    }
  });

  if (duplicadosLista.length === 0) {
    await subirPostulantes();
    return;
  }

  renderModalDuplicados();
  document.getElementById('modalDuplicados').classList.remove('hidden');
}

// ========== MODAL DUPLICADOS ==========
function renderModalDuplicados() {
  document.getElementById('dupCantidad').textContent = duplicadosLista.length;
  const tbody = document.getElementById('dupTablaBody');
  tbody.innerHTML = '';
  duplicadosLista.forEach((d, i) => {
    const nuevoObj = construirPostulanteObj(d.row, d.existente.id);
    const tr = document.createElement('tr');
    tr.id = `dup-row-${i}`;
    tr.className = 'row-ignorar';
    tr.innerHTML = `
      <td>${i + 1}</td>
      <td><strong>${d.existente.nombre} ${d.existente.apellido}</strong></td>
      <td><div class="compare-cell">${JSON.stringify(d.existente, null, 2)}</div></td>
      <td><div class="compare-cell">${JSON.stringify(nuevoObj, null, 2)}</div></td>
      <td>
        <select class="action-select" onchange="cambiarAccion(${i}, this.value)">
          <option value="ignorar" selected>🚫 Ignorar</option>
          <option value="actualizar">🔄 Actualizar</option>
          <option value="nuevo">➕ Nuevo</option>
        </select>
      </td>
      <td id="dup-estado-${i}"><span class="badge badge-yellow">Ignorar</span></td>
    `;
    tbody.appendChild(tr);
  });
}

function cambiarAccion(index, accion) {
  duplicadosLista[index].accion = accion;
  const row = document.getElementById(`dup-row-${index}`);
  const estado = document.getElementById(`dup-estado-${index}`);
  row.className = accion === 'ignorar' ? 'row-ignorar' : accion === 'actualizar' ? 'row-actualizar' : 'row-nuevo';
  const badges = { ignorar: '<span class="badge badge-yellow">Ignorar</span>', actualizar: '<span class="badge badge-green">Actualizar</span>', nuevo: '<span class="badge badge-blue">Nuevo</span>' };
  estado.innerHTML = badges[accion];
}

function aplicarATodos(accion) {
  duplicadosLista.forEach((d, i) => {
    const sel = document.querySelector(`#dup-row-${i} select`);
    if (sel) { sel.value = accion; cambiarAccion(i, accion); }
  });
}

function cerrarModalDuplicados() {
  document.getElementById('modalDuplicados').classList.add('hidden');
}

async function confirmarDuplicados() {
  document.getElementById('modalDuplicados').classList.add('hidden');
  document.getElementById('modalCargando').classList.remove('hidden');

  for (const d of duplicadosLista) {
    if (d.accion === 'ignorar') {
      if (llamadoIdParaPostulantes) {
        relacionesPendientes.push({ llamado_id: llamadoIdParaPostulantes, postulante_id: d.existente.id });
      }
    } else if (d.accion === 'actualizar') {
      const actualizado = construirPostulanteObj(d.row, d.existente.id);
      const idx = tablas.postulante.findIndex(p => p.id === d.existente.id);
      if (idx !== -1) tablas.postulante[idx] = actualizado;
      if (llamadoIdParaPostulantes) {
        relacionesPendientes.push({ llamado_id: llamadoIdParaPostulantes, postulante_id: d.existente.id });
      }
    } else if (d.accion === 'nuevo') {
      const nuevo = construirPostulanteObj(d.row, generarId('postulante'));
      tablas.postulante.push(nuevo);
      if (llamadoIdParaPostulantes) {
        relacionesPendientes.push({ llamado_id: llamadoIdParaPostulantes, postulante_id: nuevo.id });
      }
    }
  }

  await subirPostulantes();
  document.getElementById('modalCargando').classList.add('hidden');
}

// Trae la versión más reciente de llamado-postulante desde GitHub y agrega ahí las
// relaciones nuevas (sin pisar lo que ya había, aunque la copia en memoria del navegador
// estuviera desactualizada). Así una persona puede pertenecer a varios llamados sin que
// una carga posterior borre las relaciones anteriores.
async function guardarRelacionesLlamadoPostulante(nuevasRelaciones) {
  if (nuevasRelaciones.length === 0) return;

  const res = await apiFetch(`/get-table?table=llamado-postulante`);
  const actual = await res.json();

  const yaExiste = (r) => actual.some(x => x.llamado_id === r.llamado_id && x.postulante_id === r.postulante_id);

  const vistos = new Set();
  const aAgregar = [];
  for (const r of nuevasRelaciones) {
    const clave = `${r.llamado_id}-${r.postulante_id}`;
    if (vistos.has(clave) || yaExiste(r)) continue;
    vistos.add(clave);
    aAgregar.push(r);
  }

  tablas['llamado-postulante'] = [...actual, ...aAgregar];
  await subirTabla('llamado-postulante');
}

async function subirPostulantes() {
  registrarArchivoImportado(csvData.length);
  await subirTabla('postulante');
  await guardarRelacionesLlamadoPostulante(relacionesPendientes);
  relacionesPendientes = [];
  await subirTabla('archivosImportados');
  await actualizarStats();

  csvData = []; csvHeaders = []; columnMapping = {}; extraFields = [];
  document.getElementById('csvFile').value = '';
  document.getElementById('csvPreviewBox').classList.add('hidden');
  document.getElementById('csvTableWrap').classList.add('hidden');
  document.getElementById('postuLlamado').value = '';
  document.getElementById('postuLlamadoInfo').classList.add('hidden');

  renderPostulantes();
  alert('Postulantes guardados correctamente.');
}

function verPostulante(id) {
  const p = tablas.postulante.find(x => x.id === id);
  if (!p) return;

  const rels = tablas['llamado-postulante'].filter(r => r.postulante_id === id);
  const llamadosInfo = rels.map(r => {
    const l = tablas.llamado.find(ll => ll.id === r.llamado_id);
    if (!l) return null;
    const emp = tablas.empresa.find(e => e.id === l.empresa_id);
    return { ...l, empresa_nombre: emp ? emp.nombre : '-', rel: r };
  }).filter(Boolean);

  const extras = Object.keys(p).filter(k => !['id', 'nombre', 'apellido', 'email', 'telefono', 'tipoDocumento', 'numeroDocumento'].includes(k));

  const badgesEstado = {
    pendiente:   '<span class="badge badge-yellow">Pendiente</span>',
    recomendado: '<span class="badge badge-green">Recomendado</span>',
    observado:   '<span class="badge badge-yellow">Observado</span>',
    rechazado:   '<span class="badge badge-red">Rechazado</span>',
  };

  document.getElementById('modalPostulanteTitulo').textContent = `${p.nombre} ${p.apellido}`;
  document.getElementById('modalPostulanteBody').innerHTML = `
    <div class="detail-grid">
      <div class="detail-item"><dt>ID</dt><dd>#${p.id}</dd></div>
      <div class="detail-item"><dt>Email</dt><dd>${p.email || '-'}</dd></div>
      <div class="detail-item"><dt>Teléfono</dt><dd>${p.telefono || '-'}</dd></div>
      ${extras.map(k => `<div class="detail-item"><dt>${k}</dt><dd>${p[k] || '-'}</dd></div>`).join('')}
    </div>

    <h3 style="margin-top:1.5rem;margin-bottom:0.75rem;font-size:1rem;font-weight:700;">Documento</h3>
    <div class="grid-2">
      <div>
        <label class="label">Tipo</label>
        <select id="docTipo" class="input">
          <option value="">-- Sin especificar --</option>
          <option value="CI" ${p.tipoDocumento === 'CI' ? 'selected' : ''}>CI</option>
          <option value="Pasaporte" ${p.tipoDocumento === 'Pasaporte' ? 'selected' : ''}>Pasaporte</option>
          <option value="Otro" ${p.tipoDocumento === 'Otro' ? 'selected' : ''}>Otro</option>
        </select>
      </div>
      <div>
        <label class="label">Número</label>
        <input type="text" id="docNumero" class="input" value="${p.numeroDocumento || ''}" placeholder="Número de documento">
      </div>
    </div>
    <button onclick="guardarDocumento(${p.id})" class="btn btn-secondary btn-sm" style="margin-top:0.75rem;">💾 Guardar documento</button>

    <h3 style="margin-top:1.5rem;margin-bottom:0.75rem;font-size:1rem;font-weight:700;">Llamados asociados (${llamadosInfo.length})</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>ID</th><th>Nombre</th><th>Cargo</th><th>Empresa</th><th>Fecha</th><th>Estado</th><th></th></tr></thead>
        <tbody>${llamadosInfo.map(l => `
          <tr>
            <td>#${l.id}</td><td>${l.nombre_llamado}</td><td>${l.cargo}</td><td>${l.empresa_nombre}</td><td>${l.fecha}</td>
            <td>${badgesEstado[l.rel.estadoEntrevista || 'pendiente']}</td>
            <td><button class="btn btn-primary btn-sm" onclick="abrirModalResultados(${l.id}, ${p.id})">Resultados</button></td>
          </tr>`).join('')}</tbody>
      </table>
    </div>
  `;
  document.getElementById('modalPostulante').classList.remove('hidden');
}

async function guardarDocumento(postulanteId) {
  const tipoDocumento = document.getElementById('docTipo').value;
  const numeroDocumento = document.getElementById('docNumero').value.trim();

  const idx = tablas.postulante.findIndex(p => p.id === postulanteId);
  if (idx === -1) return;
  tablas.postulante[idx] = { ...tablas.postulante[idx], tipoDocumento, numeroDocumento };

  await subirTabla('postulante');
  alert('Documento guardado.');
}

// ========== RESULTADOS DE ENTREVISTA (por llamado-postulante) ==========
let resLlamadoId = null;
let resPostulanteId = null;

function abrirModalResultados(llamadoId, postulanteId) {
  const rel = tablas['llamado-postulante'].find(r => r.llamado_id === llamadoId && r.postulante_id === postulanteId);
  if (!rel) return;

  resLlamadoId = llamadoId;
  resPostulanteId = postulanteId;

  const l = tablas.llamado.find(x => x.id === llamadoId);
  document.getElementById('modalResultadosTitulo').textContent = `Resultados — ${l ? l.nombre_llamado : 'Llamado #' + llamadoId}`;
  document.getElementById('resEstado').value = rel.estadoEntrevista || 'pendiente';
  document.getElementById('resPsicolaboral').value = rel.observacionPsicolaboral || '';
  document.getElementById('resPrueba').value = rel.observacionPrueba || '';
  document.getElementById('resFechaPsicolaboral').value = rel.fechaPsicolaboral || '';
  document.getElementById('resFechaPrueba').value = rel.fechaPrueba || '';

  document.getElementById('modalResultados').classList.remove('hidden');
}

function cerrarModalResultados() {
  document.getElementById('modalResultados').classList.add('hidden');
  resLlamadoId = null;
  resPostulanteId = null;
}

async function guardarResultados() {
  if (resLlamadoId === null || resPostulanteId === null) return;

  const estadoEntrevista = document.getElementById('resEstado').value;
  const observacionPsicolaboral = document.getElementById('resPsicolaboral').value.trim();
  const observacionPrueba = document.getElementById('resPrueba').value.trim();
  const fechaPsicolaboral = document.getElementById('resFechaPsicolaboral').value;
  const fechaPrueba = document.getElementById('resFechaPrueba').value;

  // Traer la versión más reciente antes de guardar, para no pisar cambios de otra sesión
  const res = await apiFetch(`/get-table?table=llamado-postulante`);
  const actual = await res.json();

  const idx = actual.findIndex(r => r.llamado_id === resLlamadoId && r.postulante_id === resPostulanteId);
  if (idx === -1) {
    alert('No se encontró la relación llamado-postulante (¿fue eliminada?).');
    return;
  }
  actual[idx] = { ...actual[idx], estadoEntrevista, observacionPsicolaboral, observacionPrueba, fechaPsicolaboral, fechaPrueba };

  tablas['llamado-postulante'] = actual;
  await subirTabla('llamado-postulante');

  cerrarModalResultados();
  verPostulante(resPostulanteId); // refresca la tabla del modal con el estado nuevo
}

function cerrarModalPostulante() {
  document.getElementById('modalPostulante').classList.add('hidden');
}

// ============================================================
// DASHBOARD
// ============================================================
async function renderDashboard() {
  try {
    const res = await apiFetch(`/get-table?table=stats`);
    const stats = await res.json();
    if (!stats || typeof stats.empresas !== 'number') throw new Error('No stats');

    document.getElementById('dash-stats').innerHTML = `
      <div class="stat-card"><div class="stat-number">${stats.empresas}</div><div class="stat-label">Empresas</div></div>
      <div class="stat-card"><div class="stat-number">${stats.llamados}</div><div class="stat-label">Llamados</div></div>
      <div class="stat-card"><div class="stat-number">${stats.postulantes}</div><div class="stat-label">Postulantes</div></div>
      <div class="stat-card"><div class="stat-number">${stats.postulaciones}</div><div class="stat-label">Postulaciones</div></div>
    `;
  } catch (e) {
    document.getElementById('dash-stats').innerHTML = `
      <div class="stat-card"><div class="stat-number">${tablas.empresa.length}</div><div class="stat-label">Empresas</div></div>
      <div class="stat-card"><div class="stat-number">${tablas.llamado.length}</div><div class="stat-label">Llamados</div></div>
      <div class="stat-card"><div class="stat-number">${tablas.postulante.length}</div><div class="stat-label">Postulantes</div></div>
      <div class="stat-card"><div class="stat-number">${tablas['llamado-postulante'].length}</div><div class="stat-label">Postulaciones</div></div>
    `;
  }

  const ultLlam = [...tablas.llamado].reverse().slice(0, 5);
  document.getElementById('dash-ult-llamados').innerHTML = ultLlam.map(l => {
    const emp = tablas.empresa.find(e => e.id === l.empresa_id);
    return `<tr><td>#${l.id}</td><td>${l.nombre_llamado}</td><td>${l.cargo}</td><td>${emp ? emp.nombre : '-'}</td><td>${l.fecha}</td></tr>`;
  }).join('');

  const ultPost = [...tablas.postulante].reverse().slice(0, 5);
  document.getElementById('dash-ult-postulantes').innerHTML = ultPost.map(p =>
    `<tr><td>#${p.id}</td><td>${p.nombre}</td><td>${p.apellido}</td><td>${p.email || '-'}</td></tr>`
  ).join('');
}

```

### Archivo: `netlify.toml`
```toml
[build]
  command = "node build-html.js"
  functions = "netlify/functions"
  publish = "."

# Nota: "command" corre ANTES de publicar el sitio. Junta los fragmentos de html-src/
# en un único index.html (ver build-html.js). El comportamiento en el navegador no
# cambia en nada: sigue siendo un solo archivo HTML servido de una, sin fetches extra.
#
# Variables de entorno requeridas (configurar en Netlify > Site settings > Environment variables):
#   GITHUB_TOKEN    -> token con permiso de escritura sobre FABIOR1981/bd
#   SESSION_SECRET  -> string aleatoria larga, usada para firmar los tokens de sesión (login)

```

### Archivo: `netlify/functions/_auth.js`
```javascript
// Helper de autenticación compartido por las funciones.
// Genera y valida un token firmado (HMAC-SHA256) sin necesidad de guardar sesiones en ningún lado.
const crypto = require('crypto');

const SECRET = process.env.SESSION_SECRET; // OBLIGATORIO: configurar en Netlify (Site settings > Environment variables)
const DURACION_MS = 8 * 60 * 60 * 1000; // 8 horas

function base64url(buf) {
  return Buffer.from(buf).toString('base64').replace(/\+/g, '-').replace(/\//g, '_').replace(/=+$/, '');
}

function firmar(usuario) {
  if (!SECRET) throw new Error('Falta configurar SESSION_SECRET en las variables de entorno de Netlify');
  const payload = { usuario, exp: Date.now() + DURACION_MS };
  const data = base64url(JSON.stringify(payload));
  const sig = base64url(crypto.createHmac('sha256', SECRET).update(data).digest());
  return `${data}.${sig}`;
}

function verificar(token) {
  if (!SECRET || !token) return null;
  const [data, sig] = token.split('.');
  if (!data || !sig) return null;

  const sigEsperada = base64url(crypto.createHmac('sha256', SECRET).update(data).digest());
  const a = Buffer.from(sig);
  const b = Buffer.from(sigEsperada);
  if (a.length !== b.length || !crypto.timingSafeEqual(a, b)) return null;

  try {
    const payload = JSON.parse(Buffer.from(data.replace(/-/g, '+').replace(/_/g, '/'), 'base64').toString('utf8'));
    if (!payload.exp || Date.now() > payload.exp) return null;
    return payload; // { usuario, exp }
  } catch {
    return null;
  }
}

// Extrae y valida el token del header Authorization: Bearer <token>
function requireAuth(event) {
  const header = event.headers.authorization || event.headers.Authorization || '';
  const token = header.startsWith('Bearer ') ? header.slice(7) : null;
  return verificar(token);
}

function hashPassword(password, salt) {
  return crypto.createHash('sha256').update(salt + password).digest('hex');
}

function generarSalt() {
  return crypto.randomBytes(16).toString('hex');
}

module.exports = { firmar, verificar, requireAuth, hashPassword, generarSalt };

```

### Archivo: `netlify/functions/get-table.js`
```javascript
const { requireAuth } = require('./_auth');

const OWNER = 'FABIOR1981';
const REPO  = 'bd';
const PATH  = 'smconsultores';

exports.handler = async (event) => {
  const sesion = requireAuth(event);
  if (!sesion) {
    return { statusCode: 401, body: JSON.stringify({ error: 'No autorizado' }) };
  }

  const table = event.queryStringParameters.table;
  if (!table) {
    return { statusCode: 400, body: JSON.stringify({ error: 'Falta parámetro table' }) };
  }

  const token = process.env.GITHUB_TOKEN;
  const url = `https://api.github.com/repos/${OWNER}/${REPO}/contents/${PATH}/${table}.json`;

  try {
    const res = await fetch(url, {
      headers: {
        'Authorization': `Bearer ${token}`,
        'Accept': 'application/vnd.github+json',
        'X-GitHub-Api-Version': '2022-11-28'
      }
    });

    if (res.status === 404) {
      return { statusCode: 200, body: JSON.stringify([]) };
    }

    if (!res.ok) throw new Error(`GitHub ${res.status}`);

    const file = await res.json();
    const content = Buffer.from(file.content, 'base64').toString('utf8');
    return {
      statusCode: 200,
      headers: { 'Content-Type': 'application/json' },
      body: content
    };
  } catch (err) {
    return { statusCode: 500, body: JSON.stringify({ error: err.message }) };
  }
};
```

### Archivo: `netlify/functions/login.js`
```javascript
const { firmar, hashPassword } = require('./_auth');

const OWNER = 'FABIOR1981';
const REPO  = 'bd';
const PATH  = 'smconsultores';

exports.handler = async (event) => {
  if (event.httpMethod !== 'POST') {
    return { statusCode: 405, body: 'Method Not Allowed' };
  }

  let usuario, password;
  try {
    ({ usuario, password } = JSON.parse(event.body || '{}'));
  } catch {
    return { statusCode: 400, body: JSON.stringify({ error: 'Body inválido' }) };
  }

  if (!usuario || !password) {
    return { statusCode: 400, body: JSON.stringify({ error: 'Faltan usuario o password' }) };
  }

  const token = process.env.GITHUB_TOKEN;
  const url = `https://api.github.com/repos/${OWNER}/${REPO}/contents/${PATH}/usuarios.json`;

  try {
    const res = await fetch(url, {
      headers: {
        'Authorization': `Bearer ${token}`,
        'Accept': 'application/vnd.github+json',
        'X-GitHub-Api-Version': '2022-11-28'
      }
    });

    // Mensaje genérico siempre, para no revelar si el usuario existe o no
    const credencialesInvalidas = { statusCode: 401, body: JSON.stringify({ error: 'Usuario o contraseña incorrectos' }) };

    if (!res.ok) return credencialesInvalidas;

    const file = await res.json();
    const usuarios = JSON.parse(Buffer.from(file.content, 'base64').toString('utf8'));

    const u = usuarios.find(x => x.usuario === usuario);
    if (!u) return credencialesInvalidas;

    const hashCalculado = hashPassword(password, u.salt);
    if (hashCalculado !== u.hash) return credencialesInvalidas;

    const jwt = firmar(usuario);
    return {
      statusCode: 200,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ token: jwt, usuario })
    };
  } catch (err) {
    return { statusCode: 500, body: JSON.stringify({ error: err.message }) };
  }
};

```

### Archivo: `netlify/functions/update-stats.js`
```javascript
const { requireAuth } = require('./_auth');

const OWNER = 'FABIOR1981';
const REPO  = 'bd';
const PATH  = 'smconsultores';

exports.handler = async (event) => {
  const sesion = requireAuth(event);
  if (!sesion) {
    return { statusCode: 401, body: JSON.stringify({ error: 'No autorizado' }) };
  }

  if (event.httpMethod !== 'POST') {
    return { statusCode: 405, body: 'Method Not Allowed' };
  }

  const token = process.env.GITHUB_TOKEN;
  const filePath = `${PATH}/stats.json`;
  const url = `https://api.github.com/repos/${OWNER}/${REPO}/contents/${filePath}`;

  try {
    // 1. Leer tablas para calcular totales
    const tablas = ['empresa', 'llamado', 'postulante', 'llamado-postulante'];
    const counts = {};

    for (const t of tablas) {
      const res = await fetch(`https://api.github.com/repos/${OWNER}/${REPO}/contents/${PATH}/${t}.json`, {
        headers: {
          'Authorization': `Bearer ${token}`,
          'Accept': 'application/vnd.github+json',
          'X-GitHub-Api-Version': '2022-11-28'
        }
      });
      if (res.status === 200) {
        const file = await res.json();
        const data = JSON.parse(Buffer.from(file.content, 'base64').toString('utf8'));
        counts[t] = Array.isArray(data) ? data.length : 0;
      } else {
        counts[t] = 0;
      }
    }

    const stats = {
      empresas: counts.empresa,
      llamados: counts.llamado,
      postulantes: counts.postulante,
      postulaciones: counts['llamado-postulante'],
      actualizado: new Date().toISOString()
    };

    // 2. Obtener SHA de stats.json si existe
    let sha = null;
    const getRes = await fetch(url, {
      headers: {
        'Authorization': `Bearer ${token}`,
        'Accept': 'application/vnd.github+json',
        'X-GitHub-Api-Version': '2022-11-28'
      }
    });
    if (getRes.status === 200) {
      const fileData = await getRes.json();
      sha = fileData.sha;
    }

    // 3. Guardar stats.json
    const contentB64 = Buffer.from(JSON.stringify(stats, null, 2)).toString('base64');
    const putRes = await fetch(url, {
      method: 'PUT',
      headers: {
        'Authorization': `Bearer ${token}`,
        'Accept': 'application/vnd.github+json',
        'X-GitHub-Api-Version': '2022-11-28',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        message: 'Actualizar stats.json',
        content: contentB64,
        ...(sha && { sha })
      })
    });

    if (!putRes.ok) throw new Error('Error guardando stats');

    return {
      statusCode: 200,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(stats)
    };

  } catch (err) {
    return { statusCode: 500, body: JSON.stringify({ error: err.message }) };
  }
};

```

### Archivo: `netlify/functions/update-table.js`
```javascript
const { requireAuth } = require('./_auth');

const OWNER = 'FABIOR1981';
const REPO  = 'bd';
const PATH  = 'smconsultores';

// Solo se permite escribir estas tablas desde la app (usuarios.json queda fuera de este endpoint)
const TABLAS_PERMITIDAS = ['empresa', 'llamado', 'postulante', 'llamado-empresa', 'llamado-postulante', 'archivosImportados'];
const MAX_REGISTROS = 5000; // límite razonable para evitar payloads gigantes por error o abuso

exports.handler = async (event) => {
  const sesion = requireAuth(event);
  if (!sesion) {
    return { statusCode: 401, body: JSON.stringify({ error: 'No autorizado' }) };
  }

  if (event.httpMethod !== 'POST') {
    return { statusCode: 405, body: 'Method Not Allowed' };
  }

  let table, data;
  try {
    ({ table, data } = JSON.parse(event.body));
  } catch {
    return { statusCode: 400, body: JSON.stringify({ error: 'Body inválido' }) };
  }

  if (!table || !Array.isArray(data)) {
    return { statusCode: 400, body: JSON.stringify({ error: 'Faltan table o data' }) };
  }
  if (!TABLAS_PERMITIDAS.includes(table)) {
    return { statusCode: 400, body: JSON.stringify({ error: 'Tabla no permitida' }) };
  }
  if (data.length > MAX_REGISTROS) {
    return { statusCode: 400, body: JSON.stringify({ error: `Demasiados registros (máx ${MAX_REGISTROS})` }) };
  }

  const token = process.env.GITHUB_TOKEN;
  const filePath = `${PATH}/${table}.json`;
  const url = `https://api.github.com/repos/${OWNER}/${REPO}/contents/${filePath}`;

  try {
    // 1. Obtener SHA si el archivo ya existe
    let sha = null;
    const getRes = await fetch(url, {
      headers: {
        'Authorization': `Bearer ${token}`,
        'Accept': 'application/vnd.github+json',
        'X-GitHub-Api-Version': '2022-11-28'
      }
    });

    if (getRes.status === 200) {
      const fileData = await getRes.json();
      sha = fileData.sha;
    }

    // 2. Preparar contenido
    const contentB64 = Buffer.from(JSON.stringify(data, null, 2)).toString('base64');

    const body = {
      message: `Actualizar ${table}.json desde app Netlify`,
      content: contentB64,
      ...(sha && { sha })
    };

    // 3. Subir
    const putRes = await fetch(url, {
      method: 'PUT',
      headers: {
        'Authorization': `Bearer ${token}`,
        'Accept': 'application/vnd.github+json',
        'X-GitHub-Api-Version': '2022-11-28',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(body)
    });

    if (!putRes.ok) {
      const err = await putRes.json();
      return { statusCode: putRes.status, body: JSON.stringify(err) };
    }

    return { statusCode: 200, body: JSON.stringify({ ok: true }) };
  } catch (err) {
    return { statusCode: 500, body: JSON.stringify({ error: err.message }) };
  }
};
```

### Archivo: `package.json`
```json
{
  "name": "smconsultores-app",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "build": "node build-html.js"
  }
}

```

### Archivo: `scripts/generar-usuario.js`
```javascript
// Uso: node scripts/generar-usuario.js <usuario> <password>
// Imprime el JSON listo para agregar al array de usuarios.json
const crypto = require('crypto');

const [, , usuario, password] = process.argv;

if (!usuario || !password) {
  console.error('Uso: node scripts/generar-usuario.js <usuario> <password>');
  process.exit(1);
}

const salt = crypto.randomBytes(16).toString('hex');
const hash = crypto.createHash('sha256').update(salt + password).digest('hex');

console.log(JSON.stringify({ usuario, salt, hash }, null, 2));

```

