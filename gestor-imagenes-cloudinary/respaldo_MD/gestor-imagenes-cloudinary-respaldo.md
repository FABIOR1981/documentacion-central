# Bundle del Proyecto para IA: gestor-imagenes-cloudinary

## Estructura de Archivos
- admin.html
- css/styles.css
- documentacion/LEEME.md
- galeria.html
- herramientas/generar-hash-admin.js
- index.html
- js/acceso.js
- js/admin.js
- js/dashboard.js
- js/galeria.js
- js/seguridad.js
- js/subir.js
- netlify/functions/eliminar-imagen.js
- netlify/functions/firmar-subida.js
- netlify/functions/generar-enlace.js
- netlify/functions/generar-hash-clave.js
- netlify/functions/iniciar-sesion-admin.js
- netlify/functions/listar-imagenes.js
- netlify/functions/modificar-imagen.js
- netlify/functions/utilidades/autorizacion.js
- netlify/functions/utilidades/cloudinary.js
- netlify/functions/utilidades/imagenes.js
- netlify/functions/utilidades/tokens.js
- netlify/functions/verificar-enlace.js
- proyectos.json
- readme.md
- seguridad.html
- subir-cliente.html
- subir.html

---

## Código Fuente

### Archivo: `admin.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Administrar imágenes | Upload Hub</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="css/styles.css">
</head>
<body class="min-h-screen p-4 md:p-8">

    <!-- Aplicación -->
    <div id="app" class="max-w-6xl mx-auto p-4 interfaz-bloqueada">
        <header class="flex flex-wrap items-center gap-3 mb-4">
            <h1 class="text-xl font-bold mr-auto">🖼️️ Administrar imágenes</h1>
            <a href="index.html" class="text-sm underline">Generador de enlaces</a>
            <button id="btnSalir" type="button" class="text-sm border rounded px-3 py-1.5 bg-white">Cerrar sesión</button>
        </header>

        <section class="bg-white rounded-xl shadow p-4 mb-4 flex flex-wrap gap-3 items-end">
            <label class="text-sm">Proyecto<br>
                <select id="selProyecto" class="border rounded px-3 py-2 w-auto min-w-48"></select>
            </label>
            <label class="text-sm">Carpeta<br>
                <select id="selCarpeta" class="border rounded px-3 py-2 w-auto min-w-40"></select>
            </label>
            <button id="btnRecargar" type="button" class="border rounded px-3 py-2 bg-white">↻ Recargar</button>
            <button id="btnSubir" type="button" class="rounded px-3 py-2 bg-emerald-600 text-white">⬆ Subir imágenes</button>
            <button id="btnBorrarSel" type="button" class="hidden rounded px-3 py-2 bg-red-600 text-white">🗑 Eliminar seleccionadas (<span id="contSel">0</span>)</button>
        </section>

        <p id="estado" class="text-sm text-slate-500 mb-3"></p>
        <section id="grilla" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4"></section>
        <div class="text-center mt-5"><button id="btnMas" type="button" class="hidden border rounded px-4 py-2 bg-white">Cargar más</button></div>
    </div>

    <!-- Edición -->
    <div id="modal" class="hidden fixed inset-0 bg-black/50 flex items-center justify-center p-4 z-50">
        <form id="formEditar" class="bg-white rounded-xl shadow-xl w-full max-w-lg max-h-[90vh] overflow-y-auto p-5 space-y-3">
            <h2 class="text-lg font-bold">Editar imagen</h2>
            <img id="editVista" alt="" class="w-full h-44 object-contain bg-slate-100 rounded">
            <div class="grid grid-cols-2 gap-3">
                <label class="text-sm">Nombre (sin extensión)
                    <input id="editNombre" class="border rounded px-3 py-2 w-full" required maxlength="100">
                </label>
                <label class="text-sm">Carpeta
                    <input id="editCarpeta" list="listaCarpetas" class="border rounded px-3 py-2 w-full" required>
                    <datalist id="listaCarpetas"></datalist>
                </label>
            </div>
            <label class="text-sm block">Título
                <input id="editTitulo" class="border rounded px-3 py-2 w-full" maxlength="300">
            </label>
            <label class="text-sm block">Descripción
                <textarea id="editDescripcion" rows="2" class="border rounded px-3 py-2 w-full" maxlength="1000"></textarea>
            </label>
            <div>
                <p class="text-sm font-semibold mb-1">Metadatos</p>
                <div id="editMetadatos" class="space-y-2"></div>
                <button id="btnAgregarMeta" type="button" class="text-sm mt-2 underline">＋ Agregar metadato</button>
            </div>
            <p id="errorEditar" class="text-sm text-red-600"></p>
            <div class="flex justify-end gap-2 pt-2">
                <button id="btnCancelar" type="button" class="border rounded px-4 py-2">Cancelar</button>
                <button id="btnGuardar" type="submit" class="rounded px-4 py-2 bg-slate-900 text-white">Guardar</button>
            </div>
        </form>
    </div>

    <script src="js/acceso.js"></script>
    <script src="js/admin.js"></script>
</body>
</html>
```

### Archivo: `css/styles.css`
```css
/* ==========================================================
   Solo estética. No depende de la lógica (JS) ni de los IDs.
   Orden de capas: 'app' queda por encima del preflight de
   Tailwind y por debajo de sus utilidades (p-4, text-xs, etc.)
   ========================================================== */
@layer theme, base, components, app, utilities;

:root {
    --primary-color: #2563EB;
    --primary-hover: #1D4ED8;
    --acento: #6366F1;
    --bg-color: #F1F5F9;
    --card-bg: #FFFFFF;
    --text-main: #0F172A;
    --text-muted: #64748B;
    --border-color: #E2E8F0;
    --input-bg: #FFFFFF;
    --radius: 12px;
    --grad: linear-gradient(135deg, #2563EB 0%, #6366F1 100%);
    --sombra: 0 10px 30px -14px rgba(30, 41, 59, .28);
}

.hidden { display: none !important; }

@layer app {
    *, *::before, *::after { box-sizing: border-box; }

    body {
        margin: 0;
        font-family: 'Plus Jakarta Sans', system-ui, -apple-system, sans-serif;
        color: var(--text-main);
        min-height: 100vh;
        padding: clamp(16px, 4vw, 48px) 16px;
        background:
            radial-gradient(900px 420px at 10% -10%, #DBEAFE 0%, transparent 60%),
            radial-gradient(800px 380px at 100% 0%, #E0E7FF 0%, transparent 55%),
            var(--bg-color);
        background-attachment: fixed;
    }

    button, input, select, textarea { font-family: inherit; }

    label { font-weight: 600; color: var(--text-main); }

    select, input[type="datetime-local"], input[type="text"], textarea {
        width: 100%;
        padding: 11px 14px;
        background-color: var(--input-bg);
        border: 1.5px solid var(--border-color);
        border-radius: var(--radius);
        font-size: .95rem;
        color: var(--text-main);
        transition: border-color .2s, box-shadow .2s;
    }
    select:focus, input:focus, textarea:focus, .focus-ring:focus {
        outline: none;
        border-color: var(--primary-color);
        box-shadow: 0 0 0 4px rgba(37, 99, 235, .14);
    }

    button {
        border: none;
        border-radius: var(--radius);
        font-weight: 600;
        cursor: pointer;
        transition: transform .15s, filter .2s, box-shadow .2s, background-color .2s;
    }
    button:active { transform: scale(.98); }

    .card {
        background: var(--card-bg);
        padding: 28px;
        border-radius: 22px;
        border: 1px solid var(--border-color);
        box-shadow: var(--sombra);
        margin-bottom: 20px;
    }

    .btn-primary {
        background: var(--grad);
        color: #fff;
        width: 100%;
        padding: 14px 18px;
        font-size: 1rem;
        box-shadow: 0 10px 20px -10px rgba(37, 99, 235, .7);
    }
    .btn-primary:hover { filter: brightness(1.08); transform: translateY(-1px); }

    .btn-secondary {
        background: #EEF2FF;
        color: #4338CA;
        width: auto;
        padding: 0 20px;
        font-size: .9rem;
    }
    .btn-secondary:hover { background: #E0E7FF; }
}

/* ==========================================================
   DASHBOARD (index.html)
   ========================================================== */
.dashboard-container { width: 100%; max-width: 560px; margin: 0 auto; }

.dashboard-container > header { text-align: center; margin-bottom: 28px; }
.dashboard-container > header h1 {
    font-size: 1.75rem; font-weight: 800; margin: 0 0 8px; letter-spacing: -.02em;
}
.dashboard-container > header p { margin: 0; color: var(--text-muted); font-size: 1rem; }

.hero-icono {
    width: 68px; height: 68px; margin: 0 auto 16px;
    display: grid; place-items: center; font-size: 32px;
    border-radius: 22px; background: var(--grad);
    box-shadow: 0 14px 28px -12px rgba(79, 70, 229, .7);
}

.form-group { margin-bottom: 22px; }
.form-group label {
    display: flex; align-items: center; gap: 10px;
    margin-bottom: 10px; font-size: .95rem;
}
.numero-paso {
    width: 24px; height: 24px; flex: none; display: grid; place-items: center;
    border-radius: 50%; background: #DBEAFE; color: #1D4ED8;
    font-size: .8rem; font-weight: 800;
}
.ayuda { margin: 8px 0 0; font-size: .8rem; color: var(--text-muted); }

.result-card {
    border: 1.5px solid #86EFAC;
    background: linear-gradient(180deg, #FFFFFF 0%, #F0FDF4 100%);
}
.result-card h2 { font-size: 1.2rem; margin: 0 0 6px; }
.result-card p { color: var(--text-muted); font-size: .92rem; margin: 0 0 12px; }
.copy-wrapper { display: flex; gap: 10px; margin-top: 12px; }
.copy-wrapper input {
    background: #F8FAFC; font-size: .85rem; text-overflow: ellipsis;
}
.copy-wrapper .btn-secondary { padding: 0 22px; white-space: nowrap; }
.status-msg {
    margin-top: 12px; font-size: .88rem; color: #16A34A;
    text-align: center; min-height: 20px; font-weight: 600;
}

/* ==========================================================
   PORTAL DE SUBIDA (subir.html)
   ========================================================== */
.spinner {
    width: 44px; height: 44px; margin: 0 auto 16px; border-radius: 50%;
    border: 4px solid #DBEAFE; border-top-color: var(--primary-color);
    animation: girar .9s linear infinite;
}
@keyframes girar { to { transform: rotate(360deg); } }
@keyframes aparecer { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: none; } }

.icono-estado { font-size: 42px; margin-bottom: 10px; }

#errorState { border-radius: 24px; }

/* Encabezado */
#mainInterface > header {
    padding: 24px; border-radius: 24px; margin-bottom: 24px;
    background: linear-gradient(135deg, #FFFFFF 0%, #EFF6FF 100%);
    animation: aparecer .4s ease both;
}
#mainInterface > header select,
#mainInterface > header input[type="text"] {
    padding: 10px 12px; font-size: .85rem; border-radius: 12px; background: #fff;
}
#mainInterface > header label { font-size: .75rem; margin-bottom: 6px; }

/* Titulos de paso */
.paso-titulo {
    display: flex; align-items: center; gap: 10px;
    margin: 0 0 12px; font-size: 1rem; font-weight: 700;
}

/* Zona para elegir imagenes (compacta; también acepta arrastrar y soltar) */
label[for="fileInput"] {
    min-height: 0; padding: 12px 18px; margin: 0;
    border: 2px dashed #93C5FD; border-radius: 16px;
    background: linear-gradient(180deg, #FFFFFF 0%, #F0F7FF 100%);
    transition: border-color .2s, box-shadow .2s, background .2s;
}
label[for="fileInput"]:hover,
label[for="fileInput"].arrastrando {
    border-color: var(--primary-color); background: #EFF6FF;
    box-shadow: 0 10px 22px -16px rgba(37, 99, 235, .6);
}
label[for="fileInput"] > div:first-child {
    width: 36px; height: 36px; margin: 0; background: #DBEAFE;
}

.cliente-page label[for="fileInput"] { min-height: 0; padding: 12px 18px; }
.cliente-page label[for="fileInput"] > div:first-child { width: 36px; height: 36px; margin: 0; }

.cliente-header {
    display: grid;
    grid-template-columns: minmax(0, .85fr) minmax(360px, 1.15fr);
    gap: 24px;
    align-items: center;
    padding: 20px 24px;
}
.cliente-upload-panel { min-width: 0; }
.cliente-upload-panel .paso-titulo { margin: 0 0 8px; }
.cliente-info .paso-label { display: flex; align-items: center; gap: 8px; }
.cliente-info .paso-label .numero-paso { margin: 0; }
.cliente-page #cardsStepTitle { margin: 0 0 12px; }
.cliente-file-picker {
    min-height: 86px;
    padding: 10px 16px;
    border-radius: 20px;
}
.cliente-file-picker > div:first-child {
    width: 36px;
    height: 36px;
    margin-bottom: 4px;
}
.cliente-file-picker .text-sm { font-size: .85rem; }
.cliente-file-picker .text-xs { margin-top: 2px; }
.cliente-page #mainInterface { max-width: 920px; margin-inline: auto; }
.cliente-page #batchActions {
    position: static;
    padding: 0;
    border: 0;
    box-shadow: none;
    background: transparent;
    backdrop-filter: none;
}

@media (max-width: 760px) {
    .cliente-header { grid-template-columns: 1fr; gap: 22px; }
    .cliente-info .max-w-xs { max-width: none; }
}

/* Barra de acciones (queda visible al bajar) */
#batchActions {
    position: sticky; top: 12px; z-index: 30;
    padding: 14px 18px; border-radius: 20px;
    background: rgba(255, 255, 255, .92); backdrop-filter: blur(10px);
    border: 1px solid #BFDBFE; border-left: 1px solid #BFDBFE;
    box-shadow: 0 16px 32px -16px rgba(30, 64, 175, .35);
}
#batchActions .btn-primary {
    width: auto; margin: 0; padding: 12px 22px; border-radius: 999px; font-size: .9rem;
}
#batchActions .btn-secondary { padding: 12px 20px; border-radius: 999px; }

/* Tarjetas de imagen */
#cardsContainer {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
    justify-content: stretch;
    gap: 14px;
}
#cardsContainer > .upload-tile {
    display: flex; flex-direction: column; gap: 8px; min-width: 0;
    margin: 0; padding: 10px; border-radius: 18px;
    transition: transform .2s, box-shadow .2s;
}
#cardsContainer > .upload-tile:hover { transform: translateY(-2px); box-shadow: 0 18px 34px -20px rgba(30, 41, 59, .4); }

/* Miniatura de cada imagen (todas del mismo tamaño) */
.upload-tile-thumb {
    display: block; width: 100%; padding: 0; border: 0; border-radius: 12px; overflow: hidden;
    aspect-ratio: 4 / 3; background: #EEF2F7; box-shadow: inset 0 0 0 1px #E2E8F0; cursor: pointer;
}
.upload-tile-thumb canvas { display: block; width: 100%; height: 100%; object-fit: cover; }
.upload-tile-info { min-width: 0; }
.upload-tile-name {
    margin: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
    color: var(--text-main); font-size: .82rem; font-weight: 700;
}
.upload-tile-sub {
    margin: 2px 0 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
    color: var(--text-muted); font-size: .72rem;
}
.upload-tile-actions { display: flex; flex-direction: column; align-items: flex-start; gap: 6px; margin-top: auto; }
.upload-tile-buttons { display: flex; flex-wrap: wrap; gap: 6px; width: 100%; }
.upload-tile-buttons button { flex: 1 1 auto; }

.preview-lightbox {
    position: fixed;
    inset: 0;
    z-index: 130;
    display: grid;
    place-items: center;
    padding: 24px;
    background: rgba(15, 23, 42, .78);
    animation: aparecer .18s ease both;
    cursor: zoom-out;
}
.preview-lightbox img {
    display: block;
    max-width: min(94vw, 1100px);
    max-height: 88vh;
    object-fit: contain;
    border-radius: 14px;
    box-shadow: 0 24px 60px rgba(15, 23, 42, .45);
}

.upload-error-modal {
    position: fixed;
    inset: 0;
    z-index: 110;
    display: grid;
    place-items: center;
    padding: 20px;
    background: rgba(15, 23, 42, .58);
}
.upload-error-panel {
    width: min(100%, 520px);
    max-height: min(80vh, 620px);
    overflow: auto;
    padding: 24px;
    border: 1px solid #FECACA;
    border-radius: 20px;
    background: #FFFFFF;
    box-shadow: 0 24px 60px rgba(15, 23, 42, .3);
}
.upload-error-heading {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    margin-bottom: 18px;
}
.upload-error-heading .icono-estado { margin: 0; font-size: 30px; }
.upload-error-heading h2 { margin: 0 0 4px; font-size: 1.1rem; color: #991B1B; }
.upload-error-heading p { margin: 0; color: var(--text-muted); font-size: .85rem; }
.upload-error-list { display: flex; flex-direction: column; gap: 10px; margin: 0 0 20px; padding: 0; list-style: none; }
.upload-error-list li { display: flex; flex-direction: column; gap: 3px; padding: 10px 12px; border-radius: 10px; background: #FEF2F2; color: #7F1D1D; font-size: .82rem; }
.upload-error-list strong { overflow-wrap: anywhere; }
.upload-error-list span { color: #991B1B; }
.upload-error-close { width: auto; padding: 10px 24px; font-size: .85rem; }
#cardsContainer select,
#cardsContainer input[type="text"],
#cardsContainer textarea {
    padding: 9px 12px; font-size: .85rem; border-radius: 12px;
}
#cardsContainer label { font-size: .75rem; margin-bottom: 4px; }
#cardsContainer .font-mono { border-radius: 10px; }

/* Botones de cada tarjeta */
#cardsContainer .upload-btn,
#cardsContainer .download-single-btn,
#cardsContainer .delete-btn,
#cardsContainer .upload-card-edit {
    padding: 7px 12px; border-radius: 999px; font-size: .75rem;
}
#cardsContainer .upload-btn { background: var(--grad); color: #fff; box-shadow: 0 8px 16px -8px rgba(37, 99, 235, .7); }
#cardsContainer .upload-btn:hover { filter: brightness(1.08); }
#cardsContainer .download-single-btn { background: #EEF2FF; color: #4338CA; }
#cardsContainer .download-single-btn:hover { background: #E0E7FF; }
#cardsContainer .delete-btn { background: #FEF2F2; color: #DC2626; }
#cardsContainer .delete-btn:hover { background: #FEE2E2; }

/* Etiquetas de estado y peso */
#cardsContainer [id^="upload-status-"] {
    border-radius: 999px; padding: 4px 10px; font-size: .7rem;
}
#cardsContainer [id^="size-badge-"] { border-radius: 999px; font-size: .7rem; }

@media (max-width: 640px) {
    .card { padding: 20px; }
    #cardsContainer { grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 10px; }
    #batchActions { top: 8px; }
    #cardsContainer > .upload-tile:hover { transform: none; }
}


/* Efecto Glassmorphism para la interfaz bloqueada */
.interfaz-bloqueada {
    filter: blur(8px);
    opacity: 0.4;
    pointer-events: none; /* Evita que el usuario haga clic en los botones de atrás */
    user-select: none;
    transition: filter 0.4s ease, opacity 0.4s ease;
}

/* Contenedor flotante para la tarjeta de acceso */
.acceso-overlay {
    position: fixed;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 100;
    /* Un ligero oscurecimiento adicional sobre el fondo */
    background: rgba(241, 245, 249, 0.2); 
}

/* Editor de detalles de cada imagen del portal de subida */
.upload-edit-modal {
    position: fixed;
    inset: 0;
    z-index: 120;
    display: grid;
    place-items: center;
    padding: 20px;
    background: rgba(15, 23, 42, .58);
}
.upload-edit-panel {
    width: min(100%, 560px);
    max-height: min(92vh, 780px);
    overflow: auto;
    display: flex;
    flex-direction: column;
    gap: 12px;
    padding: 22px;
    border: 1px solid var(--border-color);
    border-radius: 20px;
    background: #fff;
    box-shadow: 0 24px 60px rgba(15, 23, 42, .3);
}
.upload-edit-header, .upload-edit-label-row, .upload-edit-actions {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
}
.upload-edit-kicker { margin: 0 0 2px; color: var(--text-muted); font-size: .72rem; font-weight: 700; text-transform: uppercase; }
.upload-edit-header h2 { margin: 0; font-size: 1.15rem; }
.upload-edit-close { width: 34px; height: 34px; padding: 0; background: #F1F5F9; color: var(--text-muted); font-size: 1.5rem; line-height: 1; }
.upload-edit-preview { min-height: 120px; display: grid; place-items: center; overflow: hidden; border-radius: 12px; background: #0F172A; }
.upload-edit-preview img { display: block; max-width: 100%; max-height: 180px; object-fit: contain; }
.upload-edit-panel label { display: flex; flex-direction: column; gap: 5px; font-size: .78rem; }
.upload-edit-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.upload-edit-label-row { color: var(--text-muted); font-size: .78rem; font-weight: 700; }
.upload-edit-link { padding: 0; background: transparent; color: var(--primary-color); font-size: .78rem; }
.upload-edit-metadata { display: flex; flex-direction: column; gap: 8px; margin-top: 8px; }
.upload-edit-meta-row { display: grid; grid-template-columns: 1fr 1fr auto; gap: 8px; }
.upload-edit-actions { justify-content: flex-end; margin-top: 4px; }
.upload-edit-actions .btn-primary { width: auto; }
.upload-edit-actions .btn-secondary { padding: 12px 18px; }
.upload-card-edit { width: auto; background: #EEF2FF; color: #4338CA; }
.upload-edit-preview img { cursor: zoom-in; }
@media (max-width: 520px) { .upload-edit-grid, .upload-edit-meta-row { grid-template-columns: 1fr; } .upload-edit-meta-row button { justify-self: end; } }
```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/gestor-imagenes-cloudinary/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `galeria.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mis imágenes | Upload Hub</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="css/styles.css">
</head>
<body class="min-h-screen p-4 md:p-8">

    <p id="estadoCarga" class="text-center text-slate-500 mt-24">Validando acceso…</p>

    <div id="estadoError" class="hidden max-w-md mx-auto mt-24 bg-white p-6 rounded-xl shadow text-center">
        <p class="text-3xl mb-2">⛔</p>
        <p id="mensajeError" class="text-slate-700">Enlace inválido o vencido.</p>
    </div>

    <div id="app" class="hidden max-w-6xl mx-auto p-4">
        <!-- Cabecera unificada con el estilo de tarjeta -->
        <header class="mb-6 card p-5 md:p-6 shadow-sm">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                <div>
                    <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-blue-50 text-blue-700 inline-block mb-2">🖼️ Galería del proyecto</span>
                    <h1 class="text-2xl md:text-3xl font-bold tracking-tight" style="color: var(--text-main);"><span id="nombreProyecto"></span></h1>
                    <p class="text-xs mt-1 font-medium" style="color: #D97706;">Enlace activo hasta <span id="fechaVence"></span></p>
                </div>
                <a id="enlaceSubir" href="#" class="hidden btn-primary sm:w-auto py-2.5 px-5 text-xs md:text-sm text-center">⬆ Subir imágenes</a>
            </div>
        </header>

        <p id="avisoSinPermiso" class="hidden bg-amber-50 text-amber-800 rounded-lg p-3 text-sm mb-4">Este enlace no permite ver las imágenes.</p>

        <section id="barraControles" class="bg-white rounded-xl shadow p-4 mb-4 flex flex-wrap gap-3 items-end">
            <label class="text-sm">Carpeta<br>
                <select id="selCarpeta" class="border rounded px-3 py-2 w-auto min-w-40"></select>
            </label>
            <button id="btnRecargar" type="button" class="border rounded px-3 py-2 bg-white">↻ Recargar</button>
            <button id="btnBorrarSel" type="button" class="hidden rounded px-3 py-2 bg-red-600 text-white">🗑 Eliminar seleccionadas (<span id="contSel">0</span>)</button>
        </section>

        <p id="estado" class="text-sm text-slate-500 mb-3"></p>
        <section id="grilla" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4"></section>
        <div class="text-center mt-5"><button id="btnMas" type="button" class="hidden border rounded px-4 py-2 bg-white">Cargar más</button></div>
    </div>

    <!-- Edición -->
    <div id="modal" class="hidden fixed inset-0 bg-black/50 flex items-center justify-center p-4 z-50">
        <form id="formEditar" class="bg-white rounded-xl shadow-xl w-full max-w-lg max-h-[90vh] overflow-y-auto p-5 space-y-3">
            <h2 class="text-lg font-bold">Editar imagen</h2>
            <img id="editVista" alt="" class="w-full h-44 object-contain bg-slate-100 rounded">
            <div class="grid grid-cols-2 gap-3">
                <label class="text-sm">Nombre (sin extensión)
                    <input id="editNombre" class="border rounded px-3 py-2 w-full" required maxlength="100">
                </label>
                <label class="text-sm">Carpeta
                    <input id="editCarpeta" list="listaCarpetas" class="border rounded px-3 py-2 w-full" required>
                    <datalist id="listaCarpetas"></datalist>
                </label>
            </div>
            <label class="text-sm block">Título
                <input id="editTitulo" class="border rounded px-3 py-2 w-full" maxlength="300">
            </label>
            <label class="text-sm block">Descripción
                <textarea id="editDescripcion" rows="2" class="border rounded px-3 py-2 w-full" maxlength="1000"></textarea>
            </label>
            <div>
                <p class="text-sm font-semibold mb-1">Metadatos</p>
                <div id="editMetadatos" class="space-y-2"></div>
                <button id="btnAgregarMeta" type="button" class="text-sm mt-2 underline">＋ Agregar metadato</button>
            </div>
            <p id="errorEditar" class="text-sm text-red-600"></p>
            <div class="flex justify-end gap-2 pt-2">
                <button id="btnCancelar" type="button" class="border rounded px-4 py-2">Cancelar</button>
                <button id="btnGuardar" type="submit" class="rounded px-4 py-2 bg-slate-900 text-white">Guardar</button>
            </div>
        </form>
    </div>

    <script src="js/acceso.js"></script>
    <script src="js/galeria.js"></script>
</body>
</html>
```

### Archivo: `herramientas/generar-hash-admin.js`
```javascript
// Uso (en tu PC, no en Netlify):  node herramientas/generar-hash-admin.js "tu contraseña de admin"
// Copiá el resultado a la variable ADMIN_PASSWORD_HASH en Netlify.
const crypto = require('crypto');

const clave = process.argv[2];
if (!clave || clave.length < 12) {
    console.error('Uso: node herramientas/generar-hash-admin.js "contraseña de al menos 12 caracteres"');
    process.exit(1);
}

const sal = crypto.randomBytes(16).toString('hex');
const hash = crypto.scryptSync(clave, sal, 64).toString('hex');
console.log(`scrypt$${sal}$${hash}`);

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard | Upload Hub</title>
    <link rel="stylesheet" href="css/styles.css">
    <style>
        /* Página compacta: formulario a la izquierda y tarjeta del enlace fija a la derecha, sin scroll vertical en pantallas comunes */
        body { padding-block: clamp(12px, 3vh, 28px); }
        .dashboard-container { max-width: 980px; }

        .dashboard-container > header.cabecera {
            display: flex; align-items: center; gap: 14px; flex-wrap: wrap;
            text-align: left; margin-bottom: 16px;
        }
        .cabecera .hero-icono { margin: 0; width: 46px; height: 46px; font-size: 22px; border-radius: 14px; flex: none; }
        .dashboard-container > header.cabecera h1 { font-size: 1.35rem; margin: 0 0 2px; }
        .dashboard-container > header.cabecera p { font-size: .85rem; margin: 0; }
        #navAdmin { margin-left: auto; display: flex; gap: 8px; flex-wrap: wrap; }
        .nav-boton {
            display: inline-flex; align-items: center; padding: 9px 14px; border-radius: 12px; border: none;
            background: #EEF2FF; color: #4338CA; font-size: .85rem; font-weight: 600; text-decoration: none; cursor: pointer;
        }
        .nav-boton:hover { background: #E0E7FF; }

        .rejilla { display: grid; grid-template-columns: minmax(0, 1.05fr) minmax(0, 1fr); gap: 18px; align-items: stretch; }
        .rejilla > .card { margin-bottom: 0; }
        @media (max-width: 820px) { .rejilla { grid-template-columns: 1fr; } }

        /* Formulario compacto */
        .form-card { padding: 22px 24px; }
        .form-card .form-group { margin-bottom: 14px; }
        .form-card .form-group label { margin-bottom: 7px; font-size: .88rem; }
        .form-card select, .form-card input[type="datetime-local"] { padding: 9px 12px; font-size: .9rem; }
        .form-card .btn-primary { padding: 12px 16px; margin-top: 4px; }
        .fila-dos { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
        @media (max-width: 520px) { .fila-dos { grid-template-columns: 1fr; } }
        .form-card .ayuda { margin-top: 6px; }

        /* Las cuatro casillas de permisos en una sola fila (pasan a la siguiente solo si no entran) */
        .grupo-permisos { display: flex; flex-wrap: wrap; gap: 8px 22px; }
        .form-group .grupo-permisos label { margin: 0; font-size: .92rem; font-weight: 500; cursor: pointer; }
        .grupo-permisos input[type="checkbox"] { width: 17px; height: 17px; margin: 0; cursor: pointer; }

        /* Tarjeta del enlace: siempre está en su lugar; al generar sale y vuelve con el enlace nuevo */
        .tarjeta-enlace { display: flex; flex-direction: column; justify-content: center; transition: opacity .16s ease, transform .16s ease; }
        .tarjeta-enlace.saliendo { opacity: 0; transform: translateY(8px) scale(.99); }
        .tarjeta-enlace.nuevo { animation: destello .9s ease; }
        @keyframes destello {
            0%   { box-shadow: 0 0 0 0 rgba(34, 197, 94, .55); }
            60%  { box-shadow: 0 0 0 12px rgba(34, 197, 94, 0); }
            100% { box-shadow: var(--sombra); }
        }
        @media (prefers-reduced-motion: reduce) {
            .tarjeta-enlace { transition: none; }
            .tarjeta-enlace.nuevo { animation: none; }
        }
        .resultado-vacio { text-align: center; color: var(--text-muted); }
        .resultado-vacio p { margin: 0 0 4px; }
        .resumen { font-size: .85rem; margin: 0 0 12px; }
        .tarjeta-enlace textarea {
            width: 100%; resize: none; background: #F8FAFC; font-size: .8rem; line-height: 1.35;
            font-family: ui-monospace, Menlo, Consolas, monospace; word-break: break-all;
        }
        .fila-copiar { display: flex; align-items: center; gap: 12px; margin-top: 12px; }
        .fila-copiar .btn-secondary { padding: 10px 18px; white-space: nowrap; }
        .fila-copiar .status-msg { margin: 0; min-height: 0; text-align: left; }
    </style>
</head>
<body>
    <main id="contenedorPrincipal" class="dashboard-container interfaz-bloqueada">
        <header class="cabecera">
            <div class="hero-icono">🔗</div>
            <div>
                <h1>Gestor de Accesos Temporales</h1>
                <p>Crea enlaces para subir imágenes a Cloudinary.</p>
            </div>
            <nav id="navAdmin">
                <a href="admin.html" class="nav-boton">🖼️ Imágenes</a>
                <a href="seguridad.html" class="nav-boton">🔑 Claves</a>
                <button type="button" id="logoutBtn" class="nav-boton">Salir</button>
            </nav>
        </header>

        <!-- Generador (visible solo con sesión de administrador; el login lo muestra js/acceso.js) -->
        <div id="panelGenerador" class="rejilla">
            <section class="card form-card">
                <form id="linkGeneratorForm">
                    <div class="fila-dos">
                        <div class="form-group">
                            <label for="projectSelect"><span class="numero-paso">1</span> ¿Para qué proyecto?</label>
                            <select id="projectSelect" name="projectSelect" required>
                                <option value="">Cargando proyectos...</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="modeSelect"><span class="numero-paso">2</span> ¿Qué pantalla verá?</label>
                            <select id="modeSelect" name="modeSelect" required>
                                <option value="cliente">Carga simplificada</option>
                                <option value="galeria">Galería(+Carga simplificada)</option>
                                <option value="admin">Carga completa(+Galeria)</option>
                            </select>
                        </div>
                    </div>

                    <div class="form-group">
                        <label title="Los permisos los valida el servidor. Por proyecto se preseleccionan los de proyectos.json."><span class="numero-paso">3</span> ¿Qué podrá hacer?</label>
                        <div id="permisosGrupo" class="grupo-permisos">
                            <label><input type="checkbox" name="permisos" value="alta"> Subir</label>
                            <label><input type="checkbox" name="permisos" value="listar"> Ver</label>
                            <label><input type="checkbox" name="permisos" value="modificar"> Modificar</label>
                            <label><input type="checkbox" name="permisos" value="eliminar"> Eliminar</label>
                        </div>
                    </div>

                    <div class="form-group">
                        <label for="expirationDate"><span class="numero-paso">4</span> ¿Hasta cuándo estará activo?</label>
                        <input type="datetime-local" id="expirationDate" name="expirationDate" required>
                        <p class="ayuda">Por defecto 24 horas. Más de 6 meses: el enlace pide contraseña.</p>
                    </div>

                    <button type="submit" class="btn-primary">✨ Generar enlace</button>
                </form>
            </section>

            <section id="resultContainer" class="card tarjeta-enlace">
                <div id="resultadoVacio" class="resultado-vacio">
                    <div class="icono-estado">🔗</div>
                    <p><strong>Tu enlace aparecerá acá</strong></p>
                    <p class="ayuda">Completá el formulario y tocá «Generar enlace».</p>
                </div>

                <div id="resultadoListo" class="hidden">
                    <h2>✅ ¡Enlace listo!</h2>
                    <p id="resumenEnlace" class="resumen"></p>
                    <textarea id="generatedLink" rows="5" readonly></textarea>
                    <div class="fila-copiar">
                        <button type="button" id="copyBtn" class="btn-secondary">📋 Copiar enlace</button>
                        <span id="copyStatus" class="status-msg"></span>
                    </div>
                    <p id="avisoLargo" class="ayuda hidden"></p>
                </div>
            </section>
        </div>
    </main>

    <script src="js/acceso.js"></script>
    <script src="js/dashboard.js"></script>
</body>
</html>
```

### Archivo: `js/acceso.js`
```javascript
// Pantalla de contraseña y sesión de administrador, compartidas por TODAS las páginas.
// El aspecto y los textos de la pantalla de contraseña se cambian solo acá.
// Los estilos van incluidos en este archivo, así que se ve igual con o sin Tailwind.
// Usa las variables de color y las clases card / btn-primary de css/styles.css.
const Acceso = (() => {
    const ESTILOS = `
        .acceso-overlay { position: fixed; inset: 0; display: flex; align-items: center; justify-content: center; z-index: 100; background: rgba(241, 245, 249, 0.2); }
        .acceso-tarjeta { width: 100%; max-width: 400px; padding: 32px 28px; text-align: center; background: #fff; border-radius: 22px; box-shadow: 0 10px 30px -14px rgba(30,41,59,.3); border-top: 4px solid var(--primary-color, #2563EB); box-sizing: border-box; }
        .acceso-tarjeta .icono-estado { font-size: 42px; margin-bottom: 10px; }
        .acceso-tarjeta h1 { font-size: 1.4rem; font-weight: 700; margin: 0 0 10px; color: var(--text-main, #0F172A); }
        .acceso-tarjeta .acceso-texto { font-size: .9rem; margin: 0 0 22px; color: var(--text-muted, #64748B); }
        .acceso-tarjeta form { display: flex; flex-direction: column; gap: 14px; margin: 0; }
        .acceso-tarjeta input[type="password"] { width: 100%; box-sizing: border-box; padding: 12px 14px; font-size: 1rem;
            text-align: center; background: #fff; color: var(--text-main, #0F172A);
            border: 1.5px solid var(--border-color, #E2E8F0); border-radius: var(--radius, 12px); transition: border-color 0.2s, box-shadow 0.2s; }
        .acceso-tarjeta input[type="password"]:focus { outline: none; border-color: var(--primary-color, #2563EB);
            box-shadow: 0 0 0 4px rgba(37, 99, 235, .14); }
        .acceso-tarjeta .acceso-error { margin: 0; font-size: .82rem; font-weight: 600; color: #DC2626; }
        .acceso-overlay.hidden, .acceso-tarjeta .acceso-error.hidden { display: none !important; }
    `;
    function inyectarEstilos() {
        if (document.getElementById('acceso-estilos')) return;
        const estilo = document.createElement('style');
        estilo.id = 'acceso-estilos';
        estilo.textContent = ESTILOS;
        document.head.appendChild(estilo);
    }

    const CLAVE_SESION = 'gestor_sesion_admin';
    const PREFIJO_CLAVE_ENLACE = 'gestor_clave_enlace_';

    // ---- Sesión de administrador (dura 2 horas; vive solo en esta pestaña) ----
    const sesionAdmin = {
        leer() {
            try {
                const s = JSON.parse(sessionStorage.getItem(CLAVE_SESION));
                if (s && s.exp > Date.now()) return s;
            } catch {}
            sessionStorage.removeItem(CLAVE_SESION);
            return null;
        },
        guardar(datos) { sessionStorage.setItem(CLAVE_SESION, JSON.stringify(datos)); },
        borrar() { sessionStorage.removeItem(CLAVE_SESION); }
    };

    // ---- Contraseña de un enlace largo: se escribe una sola vez por pestaña ----
    const claveDe = token => PREFIJO_CLAVE_ENLACE + String(token).slice(-24);
    function leerClave(token) {
        try { return sessionStorage.getItem(claveDe(token)) || ''; } catch { return ''; }
    }
    function guardarClave(token, clave) {
        try { sessionStorage.setItem(claveDe(token), clave); } catch {}
    }

    // ---- Tarjeta de contraseña (misma para admin y para enlaces) ----
    // alEnviar(valor) es async: si la contraseña no sirve debe lanzar Error('mensaje'), que se muestra en la tarjeta.
    function tarjeta({ icono = '🔐', titulo, texto, placeholder, boton, recortar = false, alEnviar }) {
        inyectarEstilos();
        const overlay = document.createElement('div');
        overlay.className = 'acceso-overlay hidden';
        overlay.innerHTML = `
            <div class="card acceso-tarjeta">
                <div class="icono-estado"></div>
                <h1></h1>
                <p class="acceso-texto"></p>
                <form>
                    <input type="password" autocomplete="current-password" required>
                    <button type="submit" class="btn-primary"></button>
                    <p class="acceso-error hidden"></p>
                </form>
            </div>`;

        const el = overlay.querySelector('.acceso-tarjeta');
        el.querySelector('.icono-estado').textContent = icono;
        el.querySelector('h1').textContent = titulo;
        el.querySelector('.acceso-texto').textContent = texto;
        const form = el.querySelector('form');
        const entrada = el.querySelector('input');
        const enviar = el.querySelector('button');
        const error = el.querySelector('.acceso-error');
        entrada.placeholder = placeholder;
        enviar.textContent = boton;

        const api = {
            mostrar(aviso) {
                overlay.classList.remove('hidden');
                if (aviso) api.mostrarError(aviso); else error.classList.add('hidden');
                entrada.focus();
            },
            ocultar() { overlay.classList.add('hidden'); },
            mostrarError(mensaje) { error.textContent = mensaje; error.classList.remove('hidden'); }
        };

        form.addEventListener('submit', async (e) => {
            e.preventDefault();
            error.classList.add('hidden');
            enviar.disabled = true;
            try {
                await alEnviar(recortar ? entrada.value.trim() : entrada.value);
                entrada.value = '';
            } catch (err) {
                api.mostrarError(err.message || 'No se pudo verificar la contraseña.');
            } finally {
                enviar.disabled = false;
            }
        });

        document.body.appendChild(overlay);
        return api;
    }

    // ---- Login de administrador: valida en el servidor, guarda la sesión y avisa con alEntrar() ----
    function loginAdmin(alEntrar) {
        const t = tarjeta({
            titulo: 'Acceso de administrador',
            texto: 'Ingresá la contraseña de administrador para continuar.',
            placeholder: 'Contraseña de administrador',
            boton: '🔓 Ingresar',
            alEnviar: async (clave) => {
                const res = await fetch('/.netlify/functions/iniciar-sesion-admin', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ clave })
                });
                const data = await res.json().catch(() => ({}));
                if (!res.ok) throw new Error(data.error || 'No se pudo iniciar sesión');
                sesionAdmin.guardar({ sesion: data.sesion, exp: data.exp });
                t.ocultar();
                await alEntrar();
            }
        });
        return t;
    }

    return { sesionAdmin, leerClave, guardarClave, tarjeta, loginAdmin };
})();
```

### Archivo: `js/admin.js`
```javascript
document.addEventListener('DOMContentLoaded', () => {
    const FUNCIONES = '/.netlify/functions';

    const $ = id => document.getElementById(id);
    const app = $('app');
    const selProyecto = $('selProyecto'), selCarpeta =$('selCarpeta');
    const grilla = $('grilla'), estado =$('estado');
    const btnMas = $('btnMas'), btnBorrarSel = $('btnBorrarSel'), contSel =$('contSel');
    const modal = $('modal');

    let proyectos = [];
    let imagenes = [];
    let siguiente = null;
    let carpetasConocidas = new Set();
    const seleccion = new Set();
    let editando = null;

    const esc = t => String(t ?? '').replace(/[&<>"']/g, c => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c]));
    const proyectoActual = () => selProyecto.value;
    const relCarpeta = img => img.carpeta.slice(proyectoActual().length + 1);
    const nombreDe = img => img.public_id.slice(img.public_id.lastIndexOf('/') + 1);
    const miniatura = url => url.replace('/upload/', '/upload/c_fill,w_360,h_270,f_auto,q_auto/');
    const mensaje = (t, error = false) => { estado.textContent = t; estado.className = `text-sm mb-3 ${error ? 'text-red-600' : 'text-slate-500'}`; };

    // ---------- Sesión (pantalla de contraseña compartida: js/acceso.js) ----------
    const login = Acceso.loginAdmin(() => iniciar());
    const leerSesion = Acceso.sesionAdmin.leer;
    function mostrar(logueado) {
        app.classList.toggle('interfaz-bloqueada', !logueado);
        if (logueado) login.ocultar(); else login.mostrar();
    }
    function cerrarSesion(aviso) {
        Acceso.sesionAdmin.borrar();
        app.classList.add('interfaz-bloqueada');
        login.mostrar(aviso);
    }
    $('btnSalir').addEventListener('click', () => cerrarSesion());

    async function api(funcion, cuerpo) {
        const s = leerSesion();
        if (!s) { cerrarSesion('La sesión venció. Ingresá de nuevo.'); throw new Error('Sesión vencida'); }
        const res = await fetch(`${FUNCIONES}/${funcion}`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json', Authorization: `Bearer ${s.sesion}` },
            body: JSON.stringify({ proyecto: proyectoActual(), ...cuerpo })
        });
        const data = await res.json().catch(() => ({}));
        if (res.status === 401) { cerrarSesion('La sesión venció. Ingresá de nuevo.'); throw new Error('Sesión vencida'); }
        if (!res.ok) throw new Error(data.error || 'Error del servidor');
        return data;
    }

    // ---------- Carpetas ----------
    function pintarCarpetas() {
        const actual = selCarpeta.value;
        const lista = [...carpetasConocidas].sort((a, b) => a.localeCompare(b, 'es'));
        selCarpeta.innerHTML = '<option value="">Todas las carpetas</option>' +
            lista.map(c => `<option value="${esc(c)}">${esc(c)}</option>`).join('');
        selCarpeta.value = lista.includes(actual) ? actual : '';
        $('listaCarpetas').innerHTML = lista.map(c => `<option value="${esc(c)}"></option>`).join('');
    }
    function carpetasDelProyecto() {
        const p = proyectos.find(x => x.id === proyectoActual());
        carpetasConocidas = new Set(((p && p.carpetas) || []).map(c => c.value));
        pintarCarpetas();
    }

    // ---------- Listado ----------
    async function cargar(reiniciar) {
        if (reiniciar) { imagenes = []; siguiente = null; seleccion.clear(); grilla.innerHTML = ''; }
        mensaje('Cargando…');
        try {
            const data = await api('listar-imagenes', { carpeta: selCarpeta.value || undefined, cursor: siguiente || undefined });
            imagenes = imagenes.concat(data.imagenes);
            siguiente = data.siguiente;
            imagenes.forEach(i => { const r = relCarpeta(i); if (r) carpetasConocidas.add(r); });
            if (!selCarpeta.value) pintarCarpetas();
            dibujar();
            mensaje(imagenes.length ? `${imagenes.length} imagen(es) cargadas.` : 'No hay imágenes en esta selección.');
        } catch (err) {
            if (err.message !== 'Sesión vencida') mensaje(err.message, true);
        }
    }

    function tarjeta(img) {
        const nombre = nombreDe(img);
        const chips = img.metadatos.map(m => `<span class="text-[11px] bg-slate-100 rounded px-1.5 py-0.5">${esc(m.name)}: ${esc(m.value)}</span>`).join('');
        return `<article class="bg-white rounded-xl shadow overflow-hidden flex flex-col" data-id="${esc(img.public_id)}">
            <div class="relative">
                <img src="${esc(miniatura(img.url))}" alt="${esc(img.descripcion || nombre)}" loading="lazy" class="w-full h-40 object-cover bg-slate-100">
                <input type="checkbox" class="sel absolute top-2 left-2 w-5 h-5" ${seleccion.has(img.public_id) ? 'checked' : ''}>
            </div>
            <div class="p-3 text-sm flex-1 space-y-1">
                <p class="font-semibold truncate" title="${esc(nombre)}">${esc(img.titulo || nombre)}</p>
                <p class="text-xs text-slate-500 truncate">${esc(relCarpeta(img))} · ${esc(nombre)}</p>
                ${img.descripcion ? `<p class="text-xs text-slate-600">${esc(img.descripcion)}</p>` : ''}
                <div class="flex flex-wrap gap-1">${chips}</div>
            </div>
            <div class="flex border-t text-sm">
                <button type="button" class="editar flex-1 py-2 hover:bg-slate-50">✏️ Editar</button>
                <button type="button" class="borrar flex-1 py-2 text-red-600 hover:bg-red-50 border-l">🗑 Eliminar</button>
            </div>
        </article>`;
    }

    function dibujar() {
        grilla.innerHTML = imagenes.map(tarjeta).join('');
        btnMas.classList.toggle('hidden', !siguiente);
        actualizarSeleccion();
    }
    function actualizarSeleccion() {
        contSel.textContent = seleccion.size;
        btnBorrarSel.classList.toggle('hidden', seleccion.size === 0);
    }

    grilla.addEventListener('click', (e) => {
        const art = e.target.closest('article');
        if (!art) return;
        const img = imagenes.find(i => i.public_id === art.dataset.id);
        if (!img) return;
        if (e.target.closest('.editar')) abrirEdicion(img);
        if (e.target.closest('.borrar')) eliminar([img.public_id], `"${img.titulo || nombreDe(img)}"`);
    });
    grilla.addEventListener('change', (e) => {
        if (!e.target.classList.contains('sel')) return;
        const id = e.target.closest('article').dataset.id;
        e.target.checked ? seleccion.add(id) : seleccion.delete(id);
        actualizarSeleccion();
    });

    // ---------- Eliminar ----------
    async function eliminar(ids, descripcion) {
        if (!confirm(`¿Eliminar ${descripcion}? Esta acción no se puede deshacer.`)) return;
        try {
            for (let i = 0; i < ids.length; i += 50) {
                await api('eliminar-imagen', { public_ids: ids.slice(i, i + 50) });
            }
            imagenes = imagenes.filter(i => !ids.includes(i.public_id));
            ids.forEach(id => seleccion.delete(id));
            dibujar();
            mensaje('Eliminado.');
        } catch (err) {
            if (err.message !== 'Sesión vencida') mensaje(err.message, true);
        }
    }
    btnBorrarSel.addEventListener('click', () => eliminar([...seleccion], `${seleccion.size} imagen(es) seleccionadas`));

    // ---------- Editar ----------
    function agregarFila(nombre = '', valor = '') {
        const fila = document.createElement('div');
        fila.className = 'flex gap-2 fila-meta';
        fila.innerHTML = `<input class="meta-nombre border rounded px-2 py-1.5 w-2/5" placeholder="etiqueta" maxlength="40" value="${esc(nombre)}">
            <input class="meta-valor border rounded px-2 py-1.5 flex-1" placeholder="valor" maxlength="200" value="${esc(valor)}">
            <button type="button" class="quitar text-red-600 px-2" title="Quitar">✕</button>`;
        $('editMetadatos').appendChild(fila);
    }
    $('btnAgregarMeta').addEventListener('click', () => agregarFila());$('editMetadatos').addEventListener('click', (e) => { if (e.target.classList.contains('quitar')) e.target.closest('.fila-meta').remove(); });

    function abrirEdicion(img) {
        editando = img;
        $('errorEditar').textContent = '';
        $('editVista').src = miniatura(img.url);$('editNombre').value = nombreDe(img);
        $('editCarpeta').value = relCarpeta(img);$('editTitulo').value = img.titulo;
        $('editDescripcion').value = img.descripcion;
        $('editMetadatos').innerHTML = '';
        img.metadatos.forEach(m => agregarFila(m.name, m.value));
        modal.classList.remove('hidden');
    }
    const cerrarEdicion = () => { modal.classList.add('hidden'); editando = null; };
    $('btnCancelar').addEventListener('click', cerrarEdicion);
    modal.addEventListener('click', (e) => { if (e.target === modal) cerrarEdicion(); });

    $('formEditar').addEventListener('submit', async (e) => {
        e.preventDefault();
        if (!editando) return;
        const carpeta = $('editCarpeta').value.trim();
        const metadatos = [...document.querySelectorAll('.fila-meta')]
            .map(f => ({ name: f.querySelector('.meta-nombre').value.trim(), value: f.querySelector('.meta-valor').value.trim() }))
            .filter(m => m.name && m.value);

        // Aviso: valores numéricos repetidos en la misma carpeta (ej. orden=4 ya usado)
        for (const m of metadatos) {
            if (!/^-?\d+(?:\.\d+)?$/.test(m.value)) continue;
            const choque = imagenes.some(o => o.public_id !== editando.public_id && relCarpeta(o) === carpeta &&
                o.metadatos.some(x => x.name === m.name && x.value === m.value));
            if (choque && !confirm(`Otra imagen de "${carpeta}" ya tiene ${m.name}=${m.value}. ¿Guardar igual?`)) return;
        }

        $('btnGuardar').disabled = true;
        try {
            await api('modificar-imagen', {
                public_id: editando.public_id,
                nombre: $('editNombre').value.trim(),
                carpeta,
                titulo: $('editTitulo').value.trim(),
                descripcion: $('editDescripcion').value.trim(),
                metadatos
            });
            cerrarEdicion();
            await cargar(true);
            mensaje('Cambios guardados.');
        } catch (err) {
            if (err.message !== 'Sesión vencida') $('errorEditar').textContent = err.message;
        } finally {
            $('btnGuardar').disabled = false;
        }
    });

    // ---------- Subir (abre el portal de carga con un enlace corto de 2 horas) ----------
    $('btnSubir').addEventListener('click', async () => {
        const ventana = window.open('', '_blank');
        try {
            const data = await api('generar-enlace', { permisos: ['alta'], exp: Date.now() + 2 * 60 * 60 * 1000 });
            const url = `${location.origin}/subir.html?token=${encodeURIComponent(data.token)}&modo=admin`;
            if (ventana) ventana.location = url; else location.href = url;
        } catch (err) {
            if (ventana) ventana.close();
            if (err.message !== 'Sesión vencida') mensaje(err.message, true);
        }
    });

    // ---------- Controles ----------
    selProyecto.addEventListener('change', () => { carpetasDelProyecto(); cargar(true); });
    selCarpeta.addEventListener('change', () => cargar(true));
    $('btnRecargar').addEventListener('click', () => cargar(true));
    btnMas.addEventListener('click', () => cargar(false));

    async function iniciar() {
        mostrar(true);
        try {
            const r = await fetch('proyectos.json');
            proyectos = (await r.json()).proyectos || [];
        } catch { proyectos = []; }
        selProyecto.innerHTML = proyectos.map(p => `<option value="${esc(p.id)}">${esc(p.nombre)}</option>`).join('');
        if (!proyectos.length) { mensaje('No se pudo leer proyectos.json', true); return; }
        carpetasDelProyecto();
        await cargar(true);
    }

    leerSesion() ? iniciar() : mostrar(false);
});
```

### Archivo: `js/dashboard.js`
```javascript
document.addEventListener('DOMContentLoaded', async () => {
    const FUNCIONES = '/.netlify/functions';

    const contenedorPrincipal = document.getElementById('contenedorPrincipal');
    const panel = document.getElementById('panelGenerador');
    const navAdmin = document.getElementById('navAdmin');
    const logoutBtn = document.getElementById('logoutBtn');
    const form = document.getElementById('linkGeneratorForm');
    const projectSelect = document.getElementById('projectSelect');
    const modeSelect = document.getElementById('modeSelect');
    const expirationInput = document.getElementById('expirationDate');
    const resultContainer = document.getElementById('resultContainer');
    const resultadoVacio = document.getElementById('resultadoVacio');
    const resultadoListo = document.getElementById('resultadoListo');
    const resumenEnlace = document.getElementById('resumenEnlace');
    const generatedLinkInput = document.getElementById('generatedLink');
    const copyBtn = document.getElementById('copyBtn');
    const copyStatus = document.getElementById('copyStatus');
    const avisoLargo = document.getElementById('avisoLargo');
    const checks = [...document.querySelectorAll('input[name="permisos"]')];

    let proyectosData = [];

    // --- Sesión de administrador (pantalla de contraseña compartida: js/acceso.js) ---
    const login = Acceso.loginAdmin(async () => {
        mostrarPanel(true);
        await cargarProyectos();
    });

    function mostrarPanel(si) {
        contenedorPrincipal.classList.toggle('interfaz-bloqueada', !si);
        if (si) login.ocultar(); else login.mostrar();
    }

    function sesionVencida() {
        Acceso.sesionAdmin.borrar();
        reiniciarResultado();
        contenedorPrincipal.classList.add('interfaz-bloqueada');
        login.mostrar('La sesión venció. Ingresá de nuevo.');
    }

    logoutBtn.addEventListener('click', () => {
        Acceso.sesionAdmin.borrar();
        reiniciarResultado();
        mostrarPanel(false);
    });

    // --- Tarjeta del enlace: siempre en su lugar; al generar sale y vuelve con el enlace nuevo ---
    const esperar = ms => new Promise(r => setTimeout(r, ms));

    function reiniciarResultado() {
        generatedLinkInput.value = '';
        resultadoListo.classList.add('hidden');
        resultadoVacio.classList.remove('hidden');
        resultContainer.classList.remove('result-card', 'saliendo', 'nuevo');
    }

    async function mostrarResultado({ url, resumen, larga }) {
        resultContainer.classList.add('saliendo');            // la tarjeta se va...
        await esperar(160);
        resultadoVacio.classList.add('hidden');
        resultadoListo.classList.remove('hidden');
        resultContainer.classList.add('result-card');
        generatedLinkInput.value = url;
        resumenEnlace.textContent = resumen;
        copyStatus.textContent = '';
        avisoLargo.classList.toggle('hidden', !larga);
        if (larga) avisoLargo.textContent = 'Enlace de larga duración: al abrirlo se pedirá la contraseña guardada en la variable TOKEN_SECRET_..._LARGO de este proyecto.';
        resultContainer.classList.remove('saliendo');         // ...y vuelve con el enlace nuevo
        resultContainer.classList.remove('nuevo');
        void resultContainer.offsetWidth;                     // reinicia la animation del destello
        resultContainer.classList.add('nuevo');
    }

    // --- Proyectos y permisos por defecto ---
    function aplicarPermisosPorDefecto() {
        const proj = proyectosData.find(p => p.id === projectSelect.value);
        const defecto = (proj && proj.permisosCliente) || ['alta'];
        checks.forEach(c => { c.checked = defecto.includes(c.value); });
    }

    async function cargarProyectos() {
        try {
            const response = await fetch('proyectos.json');
            if (!response.ok) throw new Error('No se pudo cargar proyectos.json');
            proyectosData = (await response.json()).proyectos || [];
        } catch (error) {
            console.warn('No se pudo leer proyectos.json:', error);
            proyectosData = [];
        }
        projectSelect.innerHTML = '<option value="">-- Selecciona un proyecto --</option>';
        proyectosData.forEach(proj => {
            const opt = document.createElement('option');
            opt.value = proj.id;
            opt.textContent = proj.nombre;
            projectSelect.appendChild(opt);
        });
        aplicarPermisosPorDefecto();
    }
    projectSelect.addEventListener('change', aplicarPermisosPorDefecto);

    // Al elegir "Galería" se marca "Ver imágenes", que necesita para funcionar
    modeSelect.addEventListener('change', () => {
        if (modeSelect.value !== 'galeria') return;
        checks.forEach(c => { if (c.value === 'listar') c.checked = true; });
    });

    // --- Vencimiento por defecto: 24 horas ---
    const ahora = new Date();
    const en24h = new Date(ahora.getTime() + 24 * 60 * 60 * 1000);
    ahora.setMinutes(ahora.getMinutes() - ahora.getTimezoneOffset());
    en24h.setMinutes(en24h.getMinutes() - en24h.getTimezoneOffset());
    expirationInput.min = ahora.toISOString().slice(0, 16);
    expirationInput.value = en24h.toISOString().slice(0, 16);

    // --- Generar enlace ---
    form.addEventListener('submit', async (e) => {
        e.preventDefault();
        const sesion = Acceso.sesionAdmin.leer();
        if (!sesion) { sesionVencida(); return; }

        const proyecto = projectSelect.value;
        if (!proyecto) return;
        const permisos = checks.filter(c => c.checked).map(c => c.value);
        if (!permisos.length) { alert('Elegí al menos un permiso.'); return; }
        if (modeSelect.value === 'galeria' && !permisos.includes('listar')) { alert('La galería necesita el permiso "Ver imágenes".'); return; }
        if (modeSelect.value !== 'galeria' && !permisos.includes('alta')) { alert('Esa pantalla de carga necesita el permiso "Subir imágenes".'); return; }

        try {
            const res = await fetch(`${FUNCIONES}/generar-enlace`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json', Authorization: `Bearer ${sesion.sesion}` },
                body: JSON.stringify({ proyecto, permisos, exp: new Date(expirationInput.value).getTime() })
            });
            const data = await res.json();
            if (res.status === 401) { sesionVencida(); return; }
            if (!res.ok) throw new Error(data.error || 'Error al generar el enlace');

            const paginas = { admin: 'subir.html', cliente: 'subir-cliente.html', galeria: 'galeria.html' };
            const pagina = paginas[modeSelect.value];
            const modo = modeSelect.value === 'galeria' ? 'cliente' : modeSelect.value;
            const url = `${window.location.origin}/${pagina}?token=${encodeURIComponent(data.token)}&modo=${encodeURIComponent(modo)}`;

            const nombresPermisos = { alta: 'subir', listar: 'ver', modificar: 'modificar', eliminar: 'eliminar' };
            const nombreProyecto = projectSelect.options[projectSelect.selectedIndex].textContent;
            const nombrePantalla = modeSelect.options[modeSelect.selectedIndex].textContent;
            const vence = new Date(data.exp).toLocaleString('es-UY', { dateStyle: 'short', timeStyle: 'short' });
            await mostrarResultado({
                url,
                larga: data.larga,
                resumen: `${nombreProyecto} · ${nombrePantalla} · ${permisos.map(p => nombresPermisos[p] || p).join(', ')} · vence ${vence}`
            });
        } catch (err) {
            alert('Hubo un error al generar el enlace: ' + err.message);
        }
    });

    copyBtn.addEventListener('click', async () => {
        try {
            await navigator.clipboard.writeText(generatedLinkInput.value);
            copyStatus.textContent = '¡Enlace copiado al portapapeles!';
            setTimeout(() => { copyStatus.textContent = ''; }, 3000);
        } catch (err) {
            copyStatus.textContent = 'Error al copiar.';
        }
    });

    // --- Arranque ---
    if (Acceso.sesionAdmin.leer()) {
        mostrarPanel(true);
        await cargarProyectos();
    } else {
        mostrarPanel(false);
    }
});
```

### Archivo: `js/galeria.js`
```javascript
// Galería para quien entra con un ENLACE (no administrador).
// Muestra solo lo que los permisos del enlace permiten: ver, modificar, eliminar y subir.
// La validación real de cada acción la hace el servidor; los botones ocultos son solo comodidad.
document.addEventListener('DOMContentLoaded', async () => {
    const FUNCIONES = '/.netlify/functions';
    const params = new URLSearchParams(window.location.search);
    const TOKEN = params.get('token');
    const MODO = params.get('modo') === 'admin' ? 'admin' : 'cliente';

    const $ = id => document.getElementById(id);
    const selCarpeta = $('selCarpeta'), grilla = $('grilla'), estado = $('estado');
    const btnMas = $('btnMas'), btnBorrarSel = $('btnBorrarSel'), contSel = $('contSel');
    const modal = $('modal');

    let CLAVE = '';
    let PROYECTO = '';
    let PERMISOS = [];
    let proyectos = [];
    let imagenes = [];
    let siguiente = null;
    let carpetasConocidas = new Set();
    const seleccion = new Set();
    let editando = null;

    const puede = permiso => PERMISOS.includes(permiso);
    const esc = t => String(t ?? '').replace(/[&<>"']/g, c => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c]));
    const relCarpeta = img => img.carpeta.slice(PROYECTO.length + 1);
    const nombreDe = img => img.public_id.slice(img.public_id.lastIndexOf('/') + 1);
    const miniatura = url => url.replace('/upload/', '/upload/c_fill,w_360,h_270,f_auto,q_auto/');
    const mensaje = (t, error = false) => { estado.textContent = t; estado.className = `text-sm mb-3 ${error ? 'text-red-600' : 'text-slate-500'}`; };

    function verSolo(id) {
        ['estadoCarga', 'estadoError', 'app'].forEach(x => $(x).classList.toggle('hidden', x !== id));
    }
    function mostrarError(texto) {
        $('mensajeError').textContent = texto;
        verSolo('estadoError');
    }

    // ---------- Acceso ----------
    // Devuelve 'ok', 'clave' (hay que pedir la contraseña) o { error }
    async function verificar(clave) {
        const res = await fetch(`${FUNCIONES}/verificar-enlace`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ token: TOKEN, clave })
        });
        const data = await res.json().catch(() => ({}));

        if (res.status === 401 && (data.requiereClave || (clave && data.error === 'Contraseña incorrecta'))) return 'clave';
        if (!res.ok || !data.valida) return { error: data.error || 'Enlace inválido o vencido.' };

        CLAVE = clave || '';
        PROYECTO = data.proyecto;
        PERMISOS = data.permisos || [];
        await iniciar(data.exp);
        return 'ok';
    }

    // Pantalla de contraseña compartida (js/acceso.js)
    const tarjetaClave = Acceso.tarjeta({
        titulo: 'Acceso protegido',
        texto: 'Ingresá la contraseña proporcionada por el administrador.',
        placeholder: 'Contraseña',
        boton: '🔓 Desbloquear acceso',
        recortar: true,
        alEnviar: async (valor) => {
            const r = await verificar(valor);
            if (r === 'clave') throw new Error('Contraseña incorrecta.');
            if (r !== 'ok') throw new Error(r.error);
            Acceso.guardarClave(TOKEN, valor);
            tarjetaClave.ocultar();
        }
    });

    async function api(funcion, cuerpo) {
        const res = await fetch(`${FUNCIONES}/${funcion}`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ token: TOKEN, clave: CLAVE, ...cuerpo })
        });
        const data = await res.json().catch(() => ({}));
        if (!res.ok) throw new Error(data.error || 'Error del servidor');
        return data;
    }

    // ---------- Carpetas ----------
    function pintarCarpetas() {
        const actual = selCarpeta.value;
        const lista = [...carpetasConocidas].sort((a, b) => a.localeCompare(b, 'es'));
        selCarpeta.innerHTML = '<option value="">Todas las carpetas</option>' +
            lista.map(c => `<option value="${esc(c)}">${esc(c)}</option>`).join('');
        selCarpeta.value = lista.includes(actual) ? actual : '';
        $('listaCarpetas').innerHTML = lista.map(c => `<option value="${esc(c)}"></option>`).join('');
    }

    // ---------- Listado ----------
    async function cargar(reiniciar) {
        if (reiniciar) { imagenes = []; siguiente = null; seleccion.clear(); grilla.innerHTML = ''; }
        mensaje('Cargando…');
        try {
            const data = await api('listar-imagenes', { carpeta: selCarpeta.value || undefined, cursor: siguiente || undefined });
            imagenes = imagenes.concat(data.imagenes);
            siguiente = data.siguiente;
            imagenes.forEach(i => { const r = relCarpeta(i); if (r) carpetasConocidas.add(r); });
            if (!selCarpeta.value) pintarCarpetas();
            dibujar();
            mensaje(imagenes.length ? `${imagenes.length} imagen(es) cargadas.` : 'No hay imágenes en esta selección.');
        } catch (err) {
            mensaje(err.message, true);
        }
    }

    function tarjeta(img) {
        const nombre = nombreDe(img);
        const chips = img.metadatos.map(m => `<span class="text-[11px] bg-slate-100 rounded px-1.5 py-0.5">${esc(m.name)}: ${esc(m.value)}</span>`).join('');
        const botones = [
            puede('modificar') ? '<button type="button" class="editar flex-1 py-2 hover:bg-slate-50">✏️ Editar</button>' : '',
            puede('eliminar') ? '<button type="button" class="borrar flex-1 py-2 text-red-600 hover:bg-red-50 border-l">🗑 Eliminar</button>' : ''
        ].join('');
        return `<article class="bg-white rounded-xl shadow overflow-hidden flex flex-col" data-id="${esc(img.public_id)}">
            <div class="relative">
                <img src="${esc(miniatura(img.url))}" alt="${esc(img.descripcion || nombre)}" loading="lazy" class="w-full h-40 object-cover bg-slate-100">
                ${puede('eliminar') ? `<input type="checkbox" class="sel absolute top-2 left-2 w-5 h-5" ${seleccion.has(img.public_id) ? 'checked' : ''}>` : ''}
            </div>
            <div class="p-3 text-sm flex-1 space-y-1">
                <p class="font-semibold truncate" title="${esc(nombre)}">${esc(img.titulo || nombre)}</p>
                <p class="text-xs text-slate-500 truncate">${esc(relCarpeta(img))} · ${esc(nombre)}</p>
                ${img.descripcion ? `<p class="text-xs text-slate-600">${esc(img.descripcion)}</p>` : ''}
                <div class="flex flex-wrap gap-1">${chips}</div>
            </div>
            ${botones ? `<div class="flex border-t text-sm">${botones}</div>` : ''}
        </article>`;
    }

    function dibujar() {
        grilla.innerHTML = imagenes.map(tarjeta).join('');
        btnMas.classList.toggle('hidden', !siguiente);
        actualizarSeleccion();
    }
    function actualizarSeleccion() {
        contSel.textContent = seleccion.size;
        btnBorrarSel.classList.toggle('hidden', seleccion.size === 0 || !puede('eliminar'));
    }

    grilla.addEventListener('click', (e) => {
        const art = e.target.closest('article');
        if (!art) return;
        const img = imagenes.find(i => i.public_id === art.dataset.id);
        if (!img) return;
        if (e.target.closest('.editar')) abrirEdicion(img);
        if (e.target.closest('.borrar')) eliminar([img.public_id], `"${img.titulo || nombreDe(img)}"`);
    });
    grilla.addEventListener('change', (e) => {
        if (!e.target.classList.contains('sel')) return;
        const id = e.target.closest('article').dataset.id;
        e.target.checked ? seleccion.add(id) : seleccion.delete(id);
        actualizarSeleccion();
    });

    // ---------- Eliminar ----------
    async function eliminar(ids, descripcion) {
        if (!confirm(`¿Eliminar ${descripcion}? Esta acción no se puede deshacer.`)) return;
        try {
            for (let i = 0; i < ids.length; i += 50) {
                await api('eliminar-imagen', { public_ids: ids.slice(i, i + 50) });
            }
            imagenes = imagenes.filter(i => !ids.includes(i.public_id));
            ids.forEach(id => seleccion.delete(id));
            dibujar();
            mensaje('Eliminado.');
        } catch (err) {
            mensaje(err.message, true);
        }
    }
    btnBorrarSel.addEventListener('click', () => eliminar([...seleccion], `${seleccion.size} imagen(es) seleccionadas`));

    // ---------- Editar ----------
    function agregarFila(nombre = '', valor = '') {
        const fila = document.createElement('div');
        fila.className = 'flex gap-2 fila-meta';
        fila.innerHTML = `<input class="meta-nombre border rounded px-2 py-1.5 w-2/5" placeholder="etiqueta" maxlength="40" value="${esc(nombre)}">
            <input class="meta-valor border rounded px-2 py-1.5 flex-1" placeholder="valor" maxlength="200" value="${esc(valor)}">
            <button type="button" class="quitar text-red-600 px-2" title="Quitar">✕</button>`;
        $('editMetadatos').appendChild(fila);
    }
    $('btnAgregarMeta').addEventListener('click', () => agregarFila());
    $('editMetadatos').addEventListener('click', (e) => { if (e.target.classList.contains('quitar')) e.target.closest('.fila-meta').remove(); });

    function abrirEdicion(img) {
        editando = img;
        $('errorEditar').textContent = '';
        $('editVista').src = miniatura(img.url);
        $('editNombre').value = nombreDe(img);
        $('editCarpeta').value = relCarpeta(img);
        $('editTitulo').value = img.titulo;
        $('editDescripcion').value = img.descripcion;
        $('editMetadatos').innerHTML = '';
        img.metadatos.forEach(m => agregarFila(m.name, m.value));
        modal.classList.remove('hidden');
    }
    const cerrarEdicion = () => { modal.classList.add('hidden'); editando = null; };
    $('btnCancelar').addEventListener('click', cerrarEdicion);
    modal.addEventListener('click', (e) => { if (e.target === modal) cerrarEdicion(); });

    $('formEditar').addEventListener('submit', async (e) => {
        e.preventDefault();
        if (!editando) return;
        const carpeta = $('editCarpeta').value.trim();
        const metadatos = [...document.querySelectorAll('.fila-meta')]
            .map(f => ({ name: f.querySelector('.meta-nombre').value.trim(), value: f.querySelector('.meta-valor').value.trim() }))
            .filter(m => m.name && m.value);

        // Aviso: valores numéricos repetidos en la misma carpeta (ej. orden=4 ya usado)
        for (const m of metadatos) {
            if (!/^-?\d+(?:\.\d+)?$/.test(m.value)) continue;
            const choque = imagenes.some(o => o.public_id !== editando.public_id && relCarpeta(o) === carpeta &&
                o.metadatos.some(x => x.name === m.name && x.value === m.value));
            if (choque && !confirm(`Otra imagen de "${carpeta}" ya tiene ${m.name}=${m.value}. ¿Guardar igual?`)) return;
        }

        $('btnGuardar').disabled = true;
        try {
            await api('modificar-imagen', {
                public_id: editando.public_id,
                nombre: $('editNombre').value.trim(),
                carpeta,
                titulo: $('editTitulo').value.trim(),
                descripcion: $('editDescripcion').value.trim(),
                metadatos
            });
            cerrarEdicion();
            await cargar(true);
            mensaje('Cambios guardados.');
        } catch (err) {
            $('errorEditar').textContent = err.message;
        } finally {
            $('btnGuardar').disabled = false;
        }
    });

    // ---------- Controles ----------
    selCarpeta.addEventListener('change', () => cargar(true));
    $('btnRecargar').addEventListener('click', () => cargar(true));
    btnMas.addEventListener('click', () => cargar(false));

    async function iniciar(exp) {
        try {
            const r = await fetch('proyectos.json');
            proyectos = (await r.json()).proyectos || [];
        } catch { proyectos = []; }
        const def = proyectos.find(p => p.id === PROYECTO);
        $('nombreProyecto').textContent = (def && def.nombre) || PROYECTO;
        $('fechaVence').textContent = new Date(exp).toLocaleString('es-UY', { dateStyle: 'medium', timeStyle: 'short' });
        carpetasConocidas = new Set(((def && def.carpetas) || []).map(c => c.value));
        pintarCarpetas();

        if (puede('alta')) {
            const pagina = MODO === 'cliente' ? 'subir-cliente.html' : 'subir.html';
            $('enlaceSubir').href = `${pagina}?token=${encodeURIComponent(TOKEN)}&modo=${MODO}`;
            $('enlaceSubir').classList.remove('hidden');
        }

        verSolo('app');
        if (!puede('listar')) {
            $('avisoSinPermiso').classList.remove('hidden');
            $('barraControles').classList.add('hidden');
            return;
        }
        await cargar(true);
    }

    // ---------- Arranque ----------
    if (!TOKEN) { mostrarError('Falta el enlace de acceso.'); return; }
    try {
        const r = await verificar(Acceso.leerClave(TOKEN) || undefined); // usa la contraseña ya escrita en esta pestaña
        if (r === 'clave') { verSolo(null); tarjetaClave.mostrar(); }
        else if (r !== 'ok') mostrarError(r.error);
    } catch {
        mostrarError('No se pudo validar el acceso con el servidor.');
    }
});

```

### Archivo: `js/seguridad.js`
```javascript
document.addEventListener('DOMContentLoaded', () => {
    const $ = id => document.getElementById(id);
    const app = $('app');
    let proyectos = [];

    // ---------- Sesión de administrador (pantalla compartida: js/acceso.js) ----------
    const login = Acceso.loginAdmin(() => iniciar());
    function mostrar(logueado) {
        app.classList.toggle('interfaz-bloqueada', !logueado);
        if (logueado) login.ocultar(); else login.mostrar();
    }
    function sesionVencida() {
        Acceso.sesionAdmin.borrar();
        app.classList.add('interfaz-bloqueada');
        login.mostrar('La sesión venció. Ingresá de nuevo.');
    }

    // ---------- Utilidades ----------
    async function copiar(texto, boton) {
        const original = boton.textContent;
        try {
            await navigator.clipboard.writeText(texto);
            boton.textContent = '¡Copiado!';
        } catch {
            boton.textContent = 'No se pudo copiar';
        }
        setTimeout(() => { boton.textContent = original; }, 2000);
    }

    // Selección sin sesgo con el generador aleatorio del navegador
    const ALFABETO = 'ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz23456789'; // sin 0/O, 1/l/I
    function claveAleatoria(largo = 16) {
        const limite = 256 - (256 % ALFABETO.length);
        const salida = [];
        while (salida.length < largo) {
            const bytes = new Uint8Array(32);
            crypto.getRandomValues(bytes);
            for (const b of bytes) {
                if (b < limite && salida.length < largo) salida.push(ALFABETO[b % ALFABETO.length]);
            }
        }
        return salida.join('');
    }
    function secretoBase64Url(bytes = 48) {
        const datos = new Uint8Array(bytes);
        crypto.getRandomValues(datos);
        let texto = '';
        datos.forEach(b => { texto += String.fromCharCode(b); });
        return btoa(texto).replace(/\+/g, '-').replace(/\//g, '_').replace(/=+$/, '');
    }
    // Misma regla que nombreVariableClaveLarga() en netlify/functions/utilidades/tokens.js
    const nombreVariable = id => `TOKEN_SECRET_${String(id).replace(/[^a-zA-Z0-9]/g, '_').toUpperCase()}_LARGO`;

    // ---------- 1) ADMIN_PASSWORD_HASH (se calcula en el servidor) ----------
    $('btnHash').addEventListener('click', async () => {
        $('errorHash').textContent = '';$('resultadoHash').classList.add('hidden');
        const clave = $('claveNueva').value;
        if (clave.length < 12) { $('errorHash').textContent = 'Usá al menos 12 caracteres.'; return; }
        if (clave !== $('claveRepetida').value) {$('errorHash').textContent = 'Las dos contraseñas no coinciden.'; return; }

        const sesion = Acceso.sesionAdmin.leer();
        if (!sesion) { sesionVencida(); return; }
        $('btnHash').disabled = true;
        try {
            const res = await fetch('/.netlify/functions/generar-hash-clave', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json', Authorization: `Bearer ${sesion.sesion}` },
                body: JSON.stringify({ clave })
            });
            const data = await res.json().catch(() => ({}));
            if (res.status === 401) { sesionVencida(); return; }
            if (!res.ok) throw new Error(data.error || 'No se pudo generar el valor');
            $('valorHash').value = data.valor;
            $('resultadoHash').classList.remove('hidden');
            $('claveNueva').value = '';$('claveRepetida').value = '';
        } catch (err) {
            $('errorHash').textContent = err.message;
        } finally {
            $('btnHash').disabled = false;
        }
    });
    $('btnCopiarHash').addEventListener('click', e => copiar($('valorHash').value, e.currentTarget));

    // ---------- 2) TOKEN_SECRET (se genera en el navegador) ----------
    $('btnSecreto').addEventListener('click', () => {
        $('valorSecreto').value = secretoBase64Url(48);$('resultadoSecreto').classList.remove('hidden');
    });
    $('btnCopiarSecreto').addEventListener('click', e => copiar($('valorSecreto').value, e.currentTarget));

    // ---------- 3) TOKEN_SECRET_{PROYECTO}_LARGO ----------
    const actualizarNombre = () => { $('nombreVariable').value =$('selProyecto').value ? nombreVariable($('selProyecto').value) : ''; };$('selProyecto').addEventListener('change', actualizarNombre);
    $('btnAleatoria').addEventListener('click', () => {$('claveLarga').value = claveAleatoria(16); });
    $('btnCopiarNombre').addEventListener('click', e => copiar($('nombreVariable').value, e.currentTarget));
    $('btnCopiarClaveLarga').addEventListener('click', e => copiar($('claveLarga').value, e.currentTarget));

    async function iniciar() {
        mostrar(true);
        try {
            const r = await fetch('proyectos.json');
            proyectos = (await r.json()).proyectos || [];
        } catch { proyectos = []; }
        $('selProyecto').innerHTML = proyectos.map(p => `<option value="${p.id.replace(/"/g, '&quot;')}">${p.nombre.replace(/[&<>]/g, c => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;' }[c]))}</option>`).join('');
        actualizarNombre();
    }

    Acceso.sesionAdmin.leer() ? iniciar() : mostrar(false);
});
```

### Archivo: `js/subir.js`
```javascript
let PROYECTO_ACTUAL = "";
let BASE_FOLDER = "";
let TOKEN_ACTUAL = "";
let TOKEN_PASSWORD = "";
const IS_CLIENT_MODE = new URLSearchParams(window.location.search).get('modo') === 'cliente';

document.addEventListener('DOMContentLoaded', async () => {
    const urlParams = new URLSearchParams(window.location.search);
    const token = urlParams.get('token');
    const loadingState = document.getElementById('loadingState');
    const errorState = document.getElementById('errorState');

    if (!token) {
        loadingState.classList.add('hidden');
        errorState.classList.remove('hidden');
        return;
    }

    TOKEN_ACTUAL = token;

    // Consulta al servidor de Netlify si el enlace es válido (y si hace falta contraseña)
    async function verificarEnlace(clave) {
        const res = await fetch('/.netlify/functions/verificar-enlace', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ token, clave })
        });
        const data = await res.json().catch(() => ({}));
        return { res, data };
    }

    // Pantalla de contraseña compartida (js/acceso.js), para enlaces de larga duración
    const tarjetaClave = Acceso.tarjeta({
        titulo: 'Acceso protegido',
        texto: 'Ingresá la contraseña proporcionada por el administrador.',
        placeholder: 'Contraseña',
        boton: '🔓 Desbloquear acceso',
        recortar: true,
        alEnviar: async (password) => {
            const { res, data } = await verificarEnlace(password);
            if (!res.ok || !data.valida) throw new Error(data.error || 'Contraseña incorrecta.');
            TOKEN_PASSWORD = password;
            Acceso.guardarClave(token, password);
            tarjetaClave.ocultar();
            await cargarInterfazProyecto(data.proyecto, data.exp, data.permisos);
        }
    });

    try {
        const claveGuardada = Acceso.leerClave(token); // contraseña ya escrita en esta pestaña (p. ej. en la galería)
        const { res, data } = await verificarEnlace(claveGuardada || undefined);

        // Enlace largo sin contraseña (o con la guardada ya inválida): se pide la contraseña
        const claveInvalida = res.status === 401 && claveGuardada && data.error === 'Contraseña incorrecta';
        if (res.status === 401 && (data.requiereClave || claveInvalida)) {
            loadingState.classList.add('hidden');
            tarjetaClave.mostrar();
            return;
        }

        if (!res.ok || !data.valida) {
            loadingState.classList.add('hidden');
            errorState.classList.remove('hidden');
            return;
        }

        TOKEN_PASSWORD = claveGuardada || '';
        await cargarInterfazProyecto(data.proyecto, data.exp, data.permisos);

    } catch (error) {
        loadingState.classList.add('hidden');
        errorState.classList.remove('hidden');
        document.getElementById('errorMessage').textContent = "No se pudo validar el acceso con el servidor o el enlace no es válido.";
    }
});

// Función auxiliar para inicializar la app una vez validado el acceso y permisos
async function cargarInterfazProyecto(projectName, expDate, permisos = []) {
    const loadingState = document.getElementById('loadingState');
    const mainInterface = document.getElementById('mainInterface');

    PROYECTO_ACTUAL = projectName; 
    BASE_FOLDER = PROYECTO_ACTUAL; 

    document.getElementById('displayExpDate').textContent = new Date(expDate).toLocaleString('es-UY', { dateStyle: 'medium', timeStyle: 'short' });

    // Si el enlace incluye el permiso de listar (ver), habilitamos el botón para volver a la galería
    if (!IS_CLIENT_MODE && Array.isArray(permisos) && permisos.includes('listar')) {
        const btnGaleria = document.getElementById('btnVolverGaleria');
        if (btnGaleria) {
            const modoGaleria = IS_CLIENT_MODE ? 'cliente' : 'admin';
            btnGaleria.href = `galeria.html?token=${encodeURIComponent(TOKEN_ACTUAL)}&modo=${modoGaleria}`;
            btnGaleria.classList.remove('hidden');
        }
    }

    let projectFolders = [
        { value: 'galeria', label: 'Galería' },
        { value: 'instalaciones', label: 'Instalaciones' }
    ];

    try {
        const response = await fetch('proyectos.json');
        if (response.ok) {
            const projData = await response.json();
            const foundProj = projData.proyectos.find(p => p.id === PROYECTO_ACTUAL);
            document.getElementById('displayProjectName').textContent = foundProj?.nombre || PROYECTO_ACTUAL;
            if (foundProj && Array.isArray(foundProj.carpetas) && foundProj.carpetas.length > 0) {
                projectFolders = foundProj.carpetas;
            }
        }
    } catch (err) {
        console.warn('Usando carpetas por defecto', err);
        document.getElementById('displayProjectName').textContent = PROYECTO_ACTUAL;
    }

    loadingState.classList.add('hidden');
    mainInterface.classList.remove('hidden');
    initApp(projectFolders); 
}

function initApp(initialFolders) {
    const NEW_FOLDER_OPTION = '__nueva__';
    const safeProjKey = PROYECTO_ACTUAL.replace(/\//g, '_');
    const LAST_FOLDER_KEY = `hub_ultima_carpeta_${safeProjKey}`;
    const FOLDERS_KEY = `hub_subcarpetas_${safeProjKey}`;
    const DEFAULT_FOLDERS = initialFolders;

    const fileInput = document.getElementById('fileInput');
    const cardsContainer = document.getElementById('cardsContainer');
    const globalCategory = document.getElementById('globalCategory');
    const globalMaxWidth = document.getElementById('globalMaxWidth');
    const globalQuality = document.getElementById('globalQuality');
    const globalQualityVal = document.getElementById('globalQualityVal');
    const globalTitle = document.getElementById('globalTitle');
    const globalKeepOriginal = document.getElementById('globalKeepOriginal');
    const batchActions = document.getElementById('batchActions');
    const counterText = document.getElementById('counterText');
    const cardsStepTitle = document.getElementById('cardsStepTitle');
    const downloadAllBtn = document.getElementById('downloadAllBtn');
    const uploadAllBtn = document.getElementById('uploadAllBtn');

    let imageFiles = [];
    let metadataFields = [];
    let previewOverlay = null;
    let previewTimeout = null;
    let editingIndex = null;

    const uploadEditModal = document.getElementById('uploadEditModal');
    const uploadEditForm = document.getElementById('uploadEditForm');
    const uploadEditMetadata = document.getElementById('uploadEditMetadata');
    const uploadEditCategory = document.getElementById('uploadEditCategory');

    function closeUploadEditor() {
        editingIndex = null;
        if (uploadEditModal) uploadEditModal.classList.add('hidden');
    }

    // Una fila de metadato: etiqueta (con las ya usadas como sugerencia) y valor (con los de esa etiqueta)
    function addUploadMetaRow(name = '', value = '') {
        const row = document.createElement('div');
        row.className = 'upload-edit-meta-row';
        const listId = `uploadMetaValores-${Date.now()}-${Math.floor(Math.random() * 100000)}`;
        row.innerHTML = `
            <input type="text" class="upload-meta-name" list="uploadMetaNombres" placeholder="Etiqueta" value="${escapeAttribute(name)}">
            <input type="text" class="upload-meta-value" list="${listId}" placeholder="Valor" value="${escapeAttribute(value)}">
            <datalist id="${listId}">${buildMetadataOptions(getMetadataValues(name))}</datalist>
            <button type="button" class="upload-edit-close upload-meta-remove" aria-label="Quitar metadato">×</button>`;
        const nameInput = row.querySelector('.upload-meta-name');
        const valueInput = row.querySelector('.upload-meta-value');
        const valueList = row.querySelector('datalist');
        nameInput.addEventListener('change', () => {
            const currentName = nameInput.value.trim();
            valueList.innerHTML = buildMetadataOptions(getMetadataValues(currentName));
            if (currentName && !valueInput.value.trim()) valueInput.value = getSuggestedMetadataValue(currentName);
        });
        row.querySelector('.upload-meta-remove').addEventListener('click', () => row.remove());
        uploadEditMetadata.appendChild(row);
    }

    function openUploadEditor(index) {
        const item = imageFiles[index];
        if (!item || !uploadEditModal) return;
        editingIndex = index;
        document.getElementById('uploadEditPreview').innerHTML = `<img src="${escapeAttribute(item.imgElement.src)}" alt="Vista previa" title="Toca para ampliar">`;
        document.getElementById('uploadEditName').value = getFinalName(item);
        uploadEditCategory.innerHTML = buildFolderOptionsHtml(item.category);
        document.getElementById('uploadEditMaxWidth').value = String(item.maxWidth);
        document.getElementById('uploadEditQuality').value = String(item.quality);
        // En el modo simplificado no se elige carpeta, ancho ni calidad por imagen (rige la carpeta general)
        uploadEditCategory.closest('label').classList.toggle('hidden', IS_CLIENT_MODE);
        document.querySelector('.upload-edit-grid').classList.toggle('hidden', IS_CLIENT_MODE);
        document.getElementById('uploadEditTitulo').value = item.customTitle || '';
        document.getElementById('uploadEditDescription').value = item.customDescription || '';
        document.getElementById('uploadMetaNombres').innerHTML = metadataFields.map(field => `<option value="${escapeAttribute(field.name)}"></option>`).join('');
        uploadEditMetadata.innerHTML = '';
        item.customMetadata.forEach(meta => addUploadMetaRow(meta.name, meta.value));
        uploadEditModal.classList.remove('hidden');
    }

    if (uploadEditForm) uploadEditForm.addEventListener('submit', event => {
        event.preventDefault();
        if (editingIndex === null || !imageFiles[editingIndex]) return;
        const item = imageFiles[editingIndex];

        const name = document.getElementById('uploadEditName').value.trim().replace(/\.webp$/i, '');
        if (!name) { alert('El nombre no puede quedar vacío.'); return; }

        const metadata = [...uploadEditMetadata.querySelectorAll('.upload-edit-meta-row')].map(row => ({
            name: row.querySelector('.upload-meta-name').value.trim(),
            value: row.querySelector('.upload-meta-value').value.trim()
        })).filter(meta => meta.name || meta.value);

        // Mismas reglas de siempre: etiqueta y valor completos, sin repetir, y números únicos
        for (let i = 0; i < metadata.length; i++) {
            const meta = metadata[i];
            if (!meta.name || !meta.value) { alert('Cada metadato necesita etiqueta y valor. Completalo o quítalo.'); return; }
            if (metadata.some((other, j) => j !== i && other.name === meta.name && other.value === meta.value)) {
                alert(`«${meta.name}: ${meta.value}» está repetido en esta imagen.`); return;
            }
            if (hasNumericMetadataConflict(editingIndex, meta.name, meta.value)) {
                alert(`El valor ${meta.value} de «${meta.name}» ya está en uso. Elige otro número.`); return;
            }
        }

        item.keepOriginal = name === item.originalName;
        const generatedPrefix = `${item.dateStr}_`;
        item.customName = name.startsWith(generatedPrefix) ? name.slice(generatedPrefix.length) : name;
        if (!IS_CLIENT_MODE) {
            if (uploadEditCategory.value && uploadEditCategory.value !== NEW_FOLDER_OPTION) item.category = uploadEditCategory.value;
            item.maxWidth = parseInt(document.getElementById('uploadEditMaxWidth').value, 10);
            item.quality = parseFloat(document.getElementById('uploadEditQuality').value);
        }
        item.customTitle = document.getElementById('uploadEditTitulo').value.trim();
        item.customDescription = document.getElementById('uploadEditDescription').value.trim();
        item.customMetadata = metadata;
        closeUploadEditor();
        renderCards();
    });

    // "+ Nueva carpeta…" también funciona dentro del editor
    if (uploadEditCategory) uploadEditCategory.addEventListener('change', () => {
        handleFolderSelectChange(uploadEditCategory, value => {
            uploadEditCategory.innerHTML = buildFolderOptionsHtml(value);
            uploadEditCategory.value = value;
        });
    });

    document.getElementById('uploadEditCancel')?.addEventListener('click', closeUploadEditor);
    document.getElementById('uploadEditClose')?.addEventListener('click', closeUploadEditor);
    uploadEditModal?.addEventListener('click', event => { if (event.target === uploadEditModal) closeUploadEditor(); });
    document.addEventListener('keydown', event => { if (event.key === 'Escape' && editingIndex !== null) closeUploadEditor(); });
    document.getElementById('uploadEditAddMeta')?.addEventListener('click', () => { if (editingIndex !== null) addUploadMetaRow(); });
    document.getElementById('uploadEditPreview')?.addEventListener('click', event => {
        if (event.target.tagName === 'IMG' && editingIndex !== null) showExpandedPreview(imageFiles[editingIndex].imgElement);
    });

    function showExpandedPreview(imgElement) {
        if (previewOverlay) previewOverlay.remove();
        if (previewTimeout) clearTimeout(previewTimeout);

        previewOverlay = document.createElement('div');
        previewOverlay.className = 'preview-lightbox';
        previewOverlay.setAttribute('role', 'dialog');
        previewOverlay.setAttribute('aria-label', 'Vista ampliada de la imagen');

        const expandedImage = document.createElement('img');
        expandedImage.src = imgElement.src;
        expandedImage.alt = 'Vista ampliada';
        previewOverlay.appendChild(expandedImage);
        document.body.appendChild(previewOverlay);

        const closePreview = () => {
            if (previewOverlay) previewOverlay.remove();
            previewOverlay = null;
            previewTimeout = null;
        };

        previewOverlay.addEventListener('click', closePreview);
        previewTimeout = setTimeout(closePreview, 4000);
    }

    function showUploadErrors(errors) {
        const previousModal = document.getElementById('uploadErrorModal');
        if (previousModal) previousModal.remove();

        const modal = document.createElement('div');
        modal.id = 'uploadErrorModal';
        modal.className = 'upload-error-modal';
        modal.setAttribute('role', 'alertdialog');
        modal.setAttribute('aria-modal', 'true');

        const panel = document.createElement('section');
        panel.className = 'upload-error-panel';
        panel.innerHTML = '<div class="upload-error-heading"><span class="icono-estado">⚠️</span><div><h2>No se pudieron subir algunas imágenes</h2><p>Las demás imágenes se procesaron normalmente.</p></div></div>';

        const list = document.createElement('ul');
        list.className = 'upload-error-list';
        errors.forEach(error => {
            const item = document.createElement('li');
            const name = document.createElement('strong');
            name.textContent = error.name;
            const reason = document.createElement('span');
            reason.textContent = error.reason;
            item.append(name, reason);
            list.appendChild(item);
        });
        panel.appendChild(list);

        const closeButton = document.createElement('button');
        closeButton.type = 'button';
        closeButton.className = 'btn-primary upload-error-close';
        closeButton.textContent = 'Cerrar';
        closeButton.addEventListener('click', () => modal.remove());
        panel.appendChild(closeButton);
        modal.appendChild(panel);
        document.body.appendChild(modal);
    }

    function reportUploadError(index, reason, errorCollector) {
        const error = { name: `${getFinalName(imageFiles[index])}.webp`, reason };
        if (errorCollector) errorCollector.push(error);
        else showUploadErrors([error]);
    }

    function loadFolders() {
        try {
            const stored = JSON.parse(localStorage.getItem(FOLDERS_KEY));
            if (Array.isArray(stored) && stored.length) return stored;
        } catch {}
        return [...DEFAULT_FOLDERS];
    }

    function saveFolders(list) {
        try { localStorage.setItem(FOLDERS_KEY, JSON.stringify(list)); } catch (e) {}
    }

    function getLastFolder() {
        const last = localStorage.getItem(LAST_FOLDER_KEY);
        const folders = loadFolders();
        return (last && folders.find(f => f.value === last)) ? last : folders[0].value;
    }

    function setLastFolder(value) {
        try { localStorage.setItem(LAST_FOLDER_KEY, value); } catch {}
    }

    function slugifyFolder(name) {
        return name.trim().toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '').replace(/[^a-z0-9]+/g, '_').replace(/^_+|_+$/g, '');
    }

    function buildFolderOptionsHtml(selectedValue) {
        const folders = loadFolders();
        const opts = folders.map(f => `<option value="${f.value}" ${f.value === selectedValue ? 'selected' : ''}>${f.label}</option>`).join('');
        return opts + `<option value="${NEW_FOLDER_OPTION}">+ Nueva carpeta…</option>`;
    }

    function populateGlobalCategory(selectedValue) {
        globalCategory.innerHTML = buildFolderOptionsHtml(selectedValue || getLastFolder());
    }
    populateGlobalCategory(getLastFolder());

    async function loadMetadataFields(category) {
        try {
            const response = await fetch('/.netlify/functions/listar-imagenes', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ token: TOKEN_ACTUAL, clave: TOKEN_PASSWORD, carpeta: category, sugerencias: true })
            });
            const data = await response.json();
            metadataFields = response.ok && Array.isArray(data.fields) ? data.fields : [];
        } catch (error) {
            metadataFields = [];
        }
        renderCards();
    }
    loadMetadataFields(getLastFolder());

    function handleFolderSelectChange(selectEl, onResolved) {
        if (selectEl.value !== NEW_FOLDER_OPTION) {
            onResolved(selectEl.value);
            return;
        }
        const raw = prompt('Nombre de la nueva carpeta:');
        const slug = raw ? slugifyFolder(raw) : '';
        if (!slug) {
            selectEl.value = getLastFolder();
            onResolved(selectEl.value);
            return;
        }
        const folders = loadFolders();
        if (!folders.find(f => f.value === slug)) {
            folders.push({ value: slug, label: raw.trim() });
            saveFolders(folders);
        }
        onResolved(slug);
    }

    if (globalCategory) globalCategory.addEventListener('change', () => {
        handleFolderSelectChange(globalCategory, (finalValue) => {
            populateGlobalCategory(finalValue);
            setLastFolder(finalValue);
            imageFiles.forEach(item => { item.category = finalValue; });
            loadMetadataFields(finalValue);
            renderCards();
        });
    });

    if (globalMaxWidth) globalMaxWidth.addEventListener('change', () => {
        const maxWidthVal = parseInt(globalMaxWidth.value);
        imageFiles.forEach(item => { item.maxWidth = maxWidthVal; });
        renderCards();
    });

    if (globalQuality) {
        globalQuality.addEventListener('input', () => {
            const qVal = parseFloat(globalQuality.value);
            if (globalQualityVal) globalQualityVal.textContent = Math.round(qVal * 100);
            imageFiles.forEach(item => { item.quality = qVal; });
            renderCards();
        });
    }

    function agregarArchivos(lista) {
        const files = Array.from(lista).filter(file => file.type.startsWith('image/'));
        if (!files.length) return;
        let processedCount = 0;
        const defaultFolder = globalCategory.value === NEW_FOLDER_OPTION ? getLastFolder() : globalCategory.value;

        // Se cuenta también cada archivo que falla, para que las demás miniaturas aparezcan igual
        const terminado = () => {
            processedCount++;
            if (processedCount === files.length) {
                imageFiles.sort((a, b) => b.timestamp - a.timestamp);
                renderCards();
            }
        };

        files.forEach(file => {
            const reader = new FileReader();
            reader.onload = (event) => {
                const img = new Image();
                img.onload = () => {
                    const fileDate = file.lastModified ? new Date(file.lastModified) : new Date();
                    const rawName = file.name.substring(0, file.name.lastIndexOf('.')) || file.name;

                    const yyyy = fileDate.getFullYear();
                    const mm = String(fileDate.getMonth() + 1).padStart(2, '0');
                    const dd = String(fileDate.getDate()).padStart(2, '0');
                    const hh = String(fileDate.getHours()).padStart(2, '0');
                    const min = String(fileDate.getMinutes()).padStart(2, '0');
                    const ss = String(fileDate.getSeconds()).padStart(2, '0');
                    const dateStr = `${yyyy}${mm}${dd}${hh}${min}${ss}`;

                    imageFiles.push({
                        fileObj: file,
                        imgElement: img,
                        originalSize: file.size,
                        timestamp: fileDate.getTime(),
                        keepOriginal: globalKeepOriginal ? globalKeepOriginal.checked : true,
                        originalName: rawName,
                        customName: 'imagen',
                        category: defaultFolder,
                        customTitle: globalTitle ? globalTitle.value.trim() : '',
                        customDescription: '',
                        customMetadata: [],
                        maxWidth: globalMaxWidth ? parseInt(globalMaxWidth.value) : 1200,
                        dateStr: dateStr,
                        quality: globalQuality ? parseFloat(globalQuality.value) : 0.8
                    });
                    terminado();
                };
                img.onerror = terminado;
                img.src = event.target.result;
            };
            reader.onerror = terminado;
            reader.readAsDataURL(file);
        });
    }

    fileInput.addEventListener('change', (e) => {
        agregarArchivos(e.target.files);
        fileInput.value = '';
    });

    // Arrastrar y soltar sobre la zona de carga
    const zonaCarga = document.querySelector('label[for="fileInput"]');
    if (zonaCarga) {
        ['dragenter', 'dragover'].forEach(evento => zonaCarga.addEventListener(evento, (e) => {
            e.preventDefault();
            zonaCarga.classList.add('arrastrando');
        }));
        ['dragleave', 'drop'].forEach(evento => zonaCarga.addEventListener(evento, (e) => {
            e.preventDefault();
            zonaCarga.classList.remove('arrastrando');
        }));
        zonaCarga.addEventListener('drop', (e) => {
            if (e.dataTransfer && e.dataTransfer.files.length) agregarArchivos(e.dataTransfer.files);
        });
    }

    if (globalKeepOriginal) globalKeepOriginal.addEventListener('change', () => {
        const val = globalKeepOriginal.checked;
        imageFiles.forEach(item => { item.keepOriginal = val; });
        renderCards();
    });

    function formatBytes(bytes) {
        if (bytes === 0) return '0 Bytes';
        const k = 1024, dm = 2;
        const sizes = ['Bytes', 'KB', 'MB'];
        const i = Math.floor(Math.log(bytes) / Math.log(k));
        return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + ' ' + sizes[i];
    }

    function getFinalName(item) {
        return item.keepOriginal ? item.originalName : `${item.dateStr}_${item.customName || 'imagen'}`;
    }

    function getScaledDimensions(img, maxWidth) {
        let width = img.naturalWidth;
        let height = img.naturalHeight;
        if (maxWidth > 0 && width > maxWidth) {
            height = Math.round((height * maxWidth) / width);
            width = maxWidth;
        }
        return { width, height };
    }

    function escapeAttribute(value) {
        return String(value || '').replace(/[&<>'"]/g, character => ({
            '&': '&amp;', '<': '&lt;', '>': '&gt;', "'": '&#39;', '"': '&quot;'
        }[character]));
    }

    function getMetadataValues(name) {
        const field = metadataFields.find(item => item.name === name);
        return field ? field.values : [];
    }

    function buildMetadataOptions(values) {
        return values.map(value => `<option value="${escapeAttribute(value)}"></option>`).join('');
    }

    function buildMetadataNameOptions(item, metadataIndex) {
        const currentName = item.customMetadata[metadataIndex].name;
        const isExistingName = metadataFields.some(field => field.name === currentName);
        const options = metadataFields.map(field => {
            const isSelected = field.name === currentName;
            return `<option value="${escapeAttribute(field.name)}" ${isSelected ? 'selected' : ''}>${escapeAttribute(field.name)}</option>`;
        }).join('');
        return `<option value="__new__" ${!isExistingName ? 'selected' : ''}>Nueva etiqueta...</option>${options}`;
    }

    function hasDuplicateMetadataPair(item, metadataIndex, name, value) {
        if (!name || !value) return false;
        return item.customMetadata.some((metadata, index) => index !== metadataIndex && metadata.name.trim() === name.trim() && metadata.value.trim() === value.trim());
    }

    function isNumericMetadata(name, value) {
        const normalizedValue = String(value || '').trim();
        if (!normalizedValue) return false;
        if (/^-?\d+(?:\.\d+)?$/.test(normalizedValue)) return true;
        const knownValues = getMetadataValues(name).map(item => String(item).trim());
        return knownValues.length > 0 && knownValues.every(item => /^-?\d+(?:\.\d+)?$/.test(item));
    }

    function hasNumericMetadataConflict(itemIndex, name, value) {
        if (!isNumericMetadata(name, value)) return false;
        const normalizedName = name.trim();
        const normalizedValue = String(value).trim();
        if (getMetadataValues(normalizedName).some(item => String(item).trim() === normalizedValue)) return true;

        return imageFiles.some((item, index) => index !== itemIndex && item.customMetadata.some(metadata =>
            metadata.name.trim() === normalizedName && metadata.value.trim() === normalizedValue
        ));
    }

    function getSuggestedMetadataValue(name) {
        const values = getMetadataValues(name).map(value => String(value).trim());
        if (!values.length || values.some(value => !/^\d+(?:\.\d+)?$/.test(value))) return '';
        return String(Math.max(...values.map(Number)) + 1);
    }

    function renderCards() {
        cardsContainer.innerHTML = '';
        if (imageFiles.length > 0) {
            batchActions.classList.remove('hidden');
            if (cardsStepTitle) cardsStepTitle.classList.remove('hidden');
            counterText.textContent = `${imageFiles.length} ${imageFiles.length === 1 ? 'imagen cargada' : 'imágenes cargadas'}`;
        } else {
            batchActions.classList.add('hidden');
            if (cardsStepTitle) cardsStepTitle.classList.add('hidden');
        }

        imageFiles.forEach((item, index) => {
            const card = document.createElement('div');
            card.className = 'card upload-tile';
            const finalGeneratedName = `${getFinalName(item)}.webp`;
            const metaCount = item.customMetadata.filter(meta => meta.name && meta.value).length;
            const resumen = [];
            if (!IS_CLIENT_MODE) resumen.push(`${escapeAttribute(item.category)} · <span id="size-badge-${index}">Calc...</span>`);
            if (metaCount) resumen.push(`${metaCount} ${metaCount === 1 ? 'etiqueta' : 'etiquetas'}`);

            // Miniatura + nombre/resumen + estado y botones. El detalle completo se edita en el modal.
            card.innerHTML = `
                <button type="button" data-index="${index}" class="upload-tile-thumb edit-btn" title="Editar detalles" aria-label="Editar detalles de ${escapeAttribute(finalGeneratedName)}">
                    <canvas id="canvas-${index}" class="preview-canvas"></canvas>
                </button>
                <div class="upload-tile-info">
                    <p class="upload-tile-name" title="${escapeAttribute(finalGeneratedName)}">${escapeAttribute(item.customTitle || finalGeneratedName)}</p>
                    <p class="upload-tile-sub">${resumen.join(' · ') || '&nbsp;'}</p>
                </div>
                <div class="upload-tile-actions">
                    <span id="upload-status-${index}" class="text-xs px-2.5 py-1.5 rounded shrink-0 bg-slate-100 font-medium" style="color: var(--text-muted);">Pendiente</span>
                    <div class="upload-tile-buttons">
                        <button type="button" data-index="${index}" class="upload-card-edit edit-btn font-semibold cursor-pointer">✏️ Editar</button>
                        <button type="button" data-index="${index}" class="upload-btn font-semibold transition-colors cursor-pointer bg-blue-600 hover:bg-blue-700 text-white shadow-sm">Subir</button>
                        ${IS_CLIENT_MODE ? '' : `<button type="button" data-index="${index}" class="download-single-btn font-semibold transition-colors cursor-pointer bg-slate-200 hover:bg-slate-300 text-slate-700" title="Descargar .webp" aria-label="Descargar .webp">⬇</button>`}
                        <button type="button" data-index="${index}" class="delete-btn font-semibold cursor-pointer" title="Quitar de la lista" aria-label="Quitar de la lista">🗑</button>
                    </div>
                </div>
            `;
            cardsContainer.appendChild(card);
            drawPreviewAndMeasure(index);
        });
        attachEvents();
    }

    function drawPreviewAndMeasure(index) {
        const item = imageFiles[index];
        const canvas = document.getElementById(`canvas-${index}`);
        if (!canvas) return;
        const ctx = canvas.getContext('2d');
        const { width, height } = getScaledDimensions(item.imgElement, item.maxWidth);
        canvas.width = width; canvas.height = height;
        ctx.drawImage(item.imgElement, 0, 0, width, height);
        canvas.toBlob((blob) => {
            const badge = document.getElementById(`size-badge-${index}`);
            if (badge) {
                const newSize = blob.size;
                const diff = newSize - item.originalSize;
                const diffPercent = Math.round((diff / item.originalSize) * 100);
                badge.textContent = `${formatBytes(newSize)} (${diffPercent > 0 ? '+' : ''}${diffPercent}%)`;
                badge.style.color = newSize < item.originalSize ? '#16A34A' : '#D97706';
            }
        }, 'image/webp', item.quality);
    }

    function buildProcessedBlob(item) {
        return new Promise((resolve) => {
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            const { width, height } = getScaledDimensions(item.imgElement, item.maxWidth);
            canvas.width = width; canvas.height = height;
            ctx.drawImage(item.imgElement, 0, 0, width, height);
            canvas.toBlob((blob) => resolve(blob), 'image/webp', item.quality);
        });
    }

    function attachEvents() {
        cardsContainer.querySelectorAll('.edit-btn').forEach(b => b.addEventListener('click', () => openUploadEditor(Number(b.dataset.index))));
        cardsContainer.querySelectorAll('.delete-btn').forEach(b => b.addEventListener('click', () => { imageFiles.splice(Number(b.dataset.index), 1); renderCards(); }));
        cardsContainer.querySelectorAll('.upload-btn').forEach(b => b.addEventListener('click', () => uploadToCloudinary(Number(b.dataset.index))));
        cardsContainer.querySelectorAll('.download-single-btn').forEach(b => b.addEventListener('click', async () => {
            const item = imageFiles[Number(b.dataset.index)];
            const blob = await buildProcessedBlob(item);
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a'); a.href = url; a.download = `${getFinalName(item)}.webp`;
            a.click(); URL.revokeObjectURL(url);
        }));

        if (uploadAllBtn) uploadAllBtn.onclick = async () => {
            const errors = [];
            const indexes = imageFiles.map((_, index) => index);
            for (const index of indexes) await uploadToCloudinary(index, errors);
            if (errors.length) showUploadErrors(errors);
        };
        if (downloadAllBtn) downloadAllBtn.onclick = () => imageFiles.forEach((_, idx) => setTimeout(async () => {
            const item = imageFiles[idx];
            const blob = await buildProcessedBlob(item);
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a'); a.href = url; a.download = `${getFinalName(item)}.webp`;
            a.click(); URL.revokeObjectURL(url);
        }, idx * 250));
    }

    async function uploadToCloudinary(index, errorCollector = null) {
        const item = imageFiles[index];
        const statusEl = document.getElementById(`upload-status-${index}`);
        const metadataPairs = item.customMetadata
            .map(metadata => `${metadata.name.trim()}\u0000${metadata.value.trim()}`)
            .filter(pair => pair !== '\u0000');
        const hasDuplicateMetadata = new Set(metadataPairs).size !== metadataPairs.length;
        const usedNumericPairs = new Set();
        metadataFields.forEach(field => {
            const values = field.values.map(value => String(value).trim());
            if (values.length && values.every(value => /^-?\d+(?:\.\d+)?$/.test(value))) {
                values.forEach(value => usedNumericPairs.add(`${field.name.trim()}\u0000${value}`));
            }
        });
        let hasNumericConflict = false;
        imageFiles.forEach(batchItem => batchItem.customMetadata.forEach(metadata => {
            if (!isNumericMetadata(metadata.name, metadata.value)) return;
            const pair = `${metadata.name.trim()}\u0000${metadata.value.trim()}`;
            if (usedNumericPairs.has(pair)) hasNumericConflict = true;
            usedNumericPairs.add(pair);
        }));

        if (hasDuplicateMetadata || hasNumericConflict) {
            if (statusEl) {
                statusEl.textContent = 'Error ✕';
                statusEl.className = "text-xs px-2 py-1 rounded shrink-0 bg-red-50 text-red-600 font-semibold";
            }
            reportUploadError(index, 'Hay etiquetas o valores numéricos repetidos.', errorCollector);
            return false;
        }

        if (statusEl) {
            statusEl.textContent = 'Subiendo…'; 
            statusEl.className = "text-xs px-2.5 py-1.5 rounded shrink-0 bg-blue-50 text-blue-600 font-semibold";
        }

        try {
            const blob = await buildProcessedBlob(item);
            const finalName = getFinalName(item);
            // El servidor valida el enlace y firma carpeta, nombre, tags y metadatos
            const resFirma = await fetch('/.netlify/functions/firmar-subida', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    token: TOKEN_ACTUAL,
                    clave: TOKEN_PASSWORD,
                    carpeta: item.category,
                    nombre: finalName,
                    titulo: item.customTitle || '',
                    descripcion: item.customDescription || '',
                    metadatos: item.customMetadata
                })
            });
            const firma = await resFirma.json();
            if (!resFirma.ok) throw new Error(firma.error || 'No se pudo autorizar la subida');

            const formData = new FormData();
            formData.append('file', blob, `${finalName}.webp`);
            Object.entries(firma.parametros).forEach(([clave, valor]) => formData.append(clave, valor));
            formData.append('api_key', firma.apiKey);
            formData.append('signature', firma.firma);

            const res = await fetch(`https://api.cloudinary.com/v1_1/${firma.cloudName}/image/upload`, {
                method: 'POST', body: formData
            });
            const data = await res.json();

            if (!res.ok) throw new Error(data.error?.message || 'Error de Cloudinary');
            if (data.existing) throw new Error('Ya existe una imagen con ese nombre en la carpeta');

            if (statusEl) {
                statusEl.textContent = `✓ Éxito`; 
                statusEl.className = "text-xs px-2.5 py-1.5 rounded shrink-0 bg-emerald-50 text-emerald-700 font-semibold";
            }
            return true;
        } catch (err) {
            if (statusEl) {
                statusEl.textContent = 'Error ✕'; 
                statusEl.className = "text-xs px-2.5 py-1.5 rounded shrink-0 bg-red-50 text-red-600 font-semibold";
                statusEl.title = err.message;
            }
            reportUploadError(index, err.message || 'Error desconocido al subir a Cloudinary.', errorCollector);
            return false;
        }
    }
}
```

### Archivo: `netlify/functions/eliminar-imagen.js`
```javascript
const { responder } = require('./utilidades/tokens');
const { autorizar } = require('./utilidades/autorizacion');
const { llamarApiAdmin } = require('./utilidades/cloudinary');
const { leerCuerpo, idPermitido } = require('./utilidades/imagenes');

// Permiso requerido: "eliminar". Recibe { public_ids: [...] } (hasta 50). Todas deben ser del proyecto.
exports.handler = async (event) => {
    if (event.httpMethod !== 'POST') return responder(405, { error: 'Método no permitido' });
    try {
        const cuerpo = leerCuerpo(event);
        if (!cuerpo) return responder(400, { error: 'El cuerpo debe ser JSON válido' });

        const aut = await autorizar(event, cuerpo, 'eliminar');
        if (aut.error) return aut.error;

        const ids = Array.isArray(cuerpo.public_ids) ? cuerpo.public_ids : [];
        if (!ids.length || ids.length > 50) return responder(400, { error: 'Indicá entre 1 y 50 imágenes' });
        if (!ids.every(id => idPermitido(id, aut.proyecto))) {
            return responder(403, { error: 'Alguna imagen no pertenece a este proyecto' });
        }

        const consulta = new URLSearchParams({ invalidate: 'true' }); // limpia también la caché del CDN
        ids.forEach(id => consulta.append('public_ids[]', id));
        const datos = await llamarApiAdmin('DELETE', '/resources/image/upload', consulta);

        return responder(200, { ok: true, resultado: datos.deleted || {} });
    } catch (err) {
        console.error('eliminar-imagen:', err.message);
        return responder(500, { error: 'Error interno al eliminar' });
    }
};

```

### Archivo: `netlify/functions/firmar-subida.js`
```javascript
const { responder } = require('./utilidades/tokens');
const { autorizar } = require('./utilidades/autorizacion');
const { credenciales, firmar } = require('./utilidades/cloudinary');
const { leerCuerpo, normalizarCarpeta, normalizarNombre, etiquetaDe, construirContexto } = require('./utilidades/imagenes');

// Permiso requerido: "alta". El navegador sube directo a Cloudinary con ESTOS parámetros firmados;
// si cambia carpeta, nombre, tags o context, la firma deja de valer.
exports.handler = async (event) => {
    if (event.httpMethod !== 'POST') return responder(405, { error: 'Método no permitido' });
    try {
        const cuerpo = leerCuerpo(event);
        if (!cuerpo) return responder(400, { error: 'El cuerpo debe ser JSON válido' });

        const aut = await autorizar(event, cuerpo, 'alta');
        if (aut.error) return aut.error;

        const carpeta = normalizarCarpeta(cuerpo.carpeta, aut.definicion);
        if (!carpeta) return responder(400, { error: 'Carpeta destino inválida para este proyecto' });
        const nombre = normalizarNombre(cuerpo.nombre);
        if (!nombre) return responder(400, { error: 'Nombre de archivo inválido' });
        const ctx = construirContexto(cuerpo);
        if (ctx.error) return responder(400, { error: ctx.error });

        const { cloudName, apiKey, apiSecret } = credenciales();
        const parametros = {
            folder: `${aut.proyecto}/${carpeta}`,
            public_id: nombre,
            tags: etiquetaDe(aut.proyecto, carpeta),
            overwrite: 'false', // nunca pisar una imagen existente
            timestamp: Math.floor(Date.now() / 1000)
        };
        if (ctx.texto) parametros.context = ctx.texto;

        return responder(200, { cloudName, apiKey, parametros, firma: firmar(parametros, apiSecret) });
    } catch (err) {
        console.error('firmar-subida:', err.message);
        return responder(500, { error: 'Error interno al firmar la subida' });
    }
};

```

### Archivo: `netlify/functions/generar-enlace.js`
```javascript
const crypto = require('crypto');
const {
    SEIS_MESES_MS, CINCO_ANIOS_MS, PERMISOS_VALIDOS,
    crearToken, nombreVariableClaveLarga, sesionAdmin, cargarProyectos, responder
} = require('./utilidades/tokens');

exports.handler = async (event) => {
    if (event.httpMethod !== 'POST') return responder(405, { error: 'Método no permitido' });

    try {
        // Solo el administrador con sesión vigente puede generar enlaces
        if (!sesionAdmin(event)) return responder(401, { error: 'Sesión de administrador requerida' });

        const { proyecto, exp, permisos } = JSON.parse(event.body || '{}');
        const ahora = Date.now();

        if (typeof exp !== 'number' || exp < ahora + 60 * 1000 || exp > ahora + CINCO_ANIOS_MS) {
            return responder(400, { error: 'Vencimiento inválido (mínimo 1 minuto, máximo 5 años)' });
        }

        const proyectos = await cargarProyectos(event);
        const definicion = proyectos.find(p => p.id === proyecto);
        if (!definicion) return responder(400, { error: 'Proyecto inexistente en proyectos.json' });

        // Permisos del enlace: los pedidos, o los que define proyectos.json (permisosCliente), o solo "alta"
        const solicitados = Array.isArray(permisos) ? permisos : (definicion.permisosCliente || ['alta']);
        const permisosFinales = [...new Set(solicitados)];
        if (!permisosFinales.length || permisosFinales.some(p => !PERMISOS_VALIDOS.includes(p))) {
            return responder(400, { error: `Permisos inválidos. Válidos: ${PERMISOS_VALIDOS.join(', ')}` });
        }

        // Enlaces de más de 6 meses: exigen contraseña, guardada en la variable de Netlify del proyecto
        const larga = exp - ahora > SEIS_MESES_MS;
        if (larga) {
            const variable = nombreVariableClaveLarga(proyecto);
            if (!process.env[variable]) {
                return responder(400, { error: `Falta configurar la variable ${variable} en Netlify` });
            }
        }

        const token = crearToken({
            t: 'enlace',
            id: crypto.randomUUID(),
            proyecto,
            permisos: permisosFinales,
            larga,
            exp
        });

        return responder(200, { token, exp, larga, permisos: permisosFinales });
    } catch (err) {
        return responder(500, { error: 'Error interno al generar el enlace' });
    }
};

```

### Archivo: `netlify/functions/generar-hash-clave.js`
```javascript
const crypto = require('crypto');
const { sesionAdmin, responder } = require('./utilidades/tokens');

// Solo admin. Convierte una contraseña nueva en el valor para la variable ADMIN_PASSWORD_HASH de Netlify
// (mismo formato que herramientas/generar-hash-admin.js). No guarda ni registra la contraseña.
exports.handler = async (event) => {
    if (event.httpMethod !== 'POST') return responder(405, { error: 'Método no permitido' });
    try {
        if (!sesionAdmin(event)) return responder(401, { error: 'Sesión de administrador requerida' });

        let cuerpo;
        try { cuerpo = JSON.parse(event.body || '{}'); } catch { return responder(400, { error: 'El cuerpo debe ser JSON válido' }); }
        const { clave } = cuerpo;
        if (typeof clave !== 'string' || clave.length < 12 || clave.length > 200) {
            return responder(400, { error: 'La contraseña debe tener entre 12 y 200 caracteres' });
        }

        const sal = crypto.randomBytes(16).toString('hex');
        const hash = crypto.scryptSync(clave, sal, 64).toString('hex');
        return responder(200, { valor: `scrypt$${sal}$${hash}` });
    } catch (err) {
        console.error('generar-hash-clave:', err.message);
        return responder(500, { error: 'Error interno al generar el valor' });
    }
};

```

### Archivo: `netlify/functions/iniciar-sesion-admin.js`
```javascript
const crypto = require('crypto');
const { crearToken, responder } = require('./utilidades/tokens');

const DURACION_SESION_MS = 2 * 60 * 60 * 1000; // 2 horas
const espera = ms => new Promise(r => setTimeout(r, ms));

function claveCorrecta(clave) {
    // ADMIN_PASSWORD_HASH = scrypt$<sal>$<hash>  (se genera con herramientas/generar-hash-admin.js)
    const guardado = process.env.ADMIN_PASSWORD_HASH || '';
    const [algoritmo, sal, hashHex] = guardado.split('$');
    if (algoritmo !== 'scrypt' || !sal || !hashHex) return false;
    const esperado = Buffer.from(hashHex, 'hex');
    const calculado = crypto.scryptSync(clave, sal, esperado.length);
    return crypto.timingSafeEqual(esperado, calculado);
}

exports.handler = async (event) => {
    if (event.httpMethod !== 'POST') return responder(405, { error: 'Método no permitido' });

    try {
        let cuerpo;
        try {
            cuerpo = JSON.parse(event.body || '{}');
        } catch {
            return responder(400, { error: 'El cuerpo debe ser JSON válido' });
        }
        const { clave } = cuerpo;
        if (typeof clave !== 'string' || !clave || clave.length > 200) {
            return responder(400, { error: 'Falta la contraseña' });
        }

        if (!claveCorrecta(clave)) {
            await espera(800); // frena la fuerza bruta básica
            return responder(401, { error: 'Contraseña incorrecta' });
        }

        const exp = Date.now() + DURACION_SESION_MS;
        const sesion = crearToken({ t: 'admin', exp });
        return responder(200, { sesion, exp });
    } catch (err) {
        // El motivo real queda en Netlify > Logs > Functions (no se muestra al público)
        console.error('iniciar-sesion-admin:', err.message);
        return responder(500, { error: 'Error interno al iniciar sesión' });
    }
};

```

### Archivo: `netlify/functions/listar-imagenes.js`
```javascript
const { responder } = require('./utilidades/tokens');
const { autorizar } = require('./utilidades/autorizacion');
const { llamarApiAdmin } = require('./utilidades/cloudinary');
const { leerCuerpo, normalizarCarpeta, mapearRecurso } = require('./utilidades/imagenes');

// Permiso requerido: "listar" (o "alta" si se piden solo sugerencias de metadatos)
exports.handler = async (event) => {
    if (event.httpMethod !== 'POST') return responder(405, { error: 'Método no permitido' });
    try {
        const cuerpo = leerCuerpo(event);
        if (!cuerpo) return responder(400, { error: 'El cuerpo debe ser JSON válido' });

        const aut = await autorizar(event, cuerpo, cuerpo.sugerencias ? 'alta' : 'listar');
        if (aut.error) return aut.error;

        let prefijo = `${aut.proyecto}/`;
        if (cuerpo.carpeta) {
            const carpeta = normalizarCarpeta(cuerpo.carpeta, aut.definicion);
            if (!carpeta) return responder(400, { error: 'Carpeta inválida para este proyecto' });
            prefijo = `${aut.proyecto}/${carpeta}/`;
        }

        const consulta = (cursor) => {
            const p = new URLSearchParams({
                type: 'upload', prefix: prefijo, max_results: cuerpo.sugerencias ? '500' : '100',
                context: 'true', tags: 'true', metadata: 'true'
            });
            if (cursor) p.set('next_cursor', cursor);
            return p;
        };

        // Modo sugerencias: recorre todas las páginas y devuelve etiquetas/valores existentes (reemplaza consultar-metadatos)
        if (cuerpo.sugerencias) {
            const campos = {};
            let cursor = '';
            do {
                const datos = await llamarApiAdmin('GET', '/resources/image/upload', consulta(cursor));
                (datos.resources || []).forEach(r => {
                    const custom = r.context?.custom || {};
                    Object.entries(custom).forEach(([nombre, valor]) => {
                        if (['alt', 'caption'].includes(nombre.toLowerCase()) || valor === '') return;
                        (campos[nombre] = campos[nombre] || new Set()).add(String(valor));
                    });
                });
                cursor = datos.next_cursor || '';
            } while (cursor);

            const lista = Object.entries(campos)
                .map(([name, valores]) => ({ name, values: [...valores].sort((a, b) => a.localeCompare(b, 'es', { numeric: true })) }))
                .sort((a, b) => a.name.localeCompare(b.name, 'es'));
            return responder(200, { fields: lista });
        }

        const datos = await llamarApiAdmin('GET', '/resources/image/upload', consulta(cuerpo.cursor));
        return responder(200, {
            imagenes: (datos.resources || []).map(mapearRecurso),
            siguiente: datos.next_cursor || null
        });
    } catch (err) {
        console.error('listar-imagenes:', err.message);
        return responder(500, { error: 'Error interno al listar imágenes' });
    }
};

```

### Archivo: `netlify/functions/modificar-imagen.js`
```javascript
const { responder } = require('./utilidades/tokens');
const { autorizar } = require('./utilidades/autorizacion');
const { llamarApiFirmada } = require('./utilidades/cloudinary');
const {
    leerCuerpo, normalizarCarpeta, normalizarNombre, etiquetaDe, idPermitido, construirContexto
} = require('./utilidades/imagenes');

// Permiso requerido: "modificar". Cambia título/descripción/metadatos, renombra y/o mueve de carpeta.
// Si se envía titulo, descripcion o metadatos, el context completo se reemplaza: el front debe mandar los tres.
exports.handler = async (event) => {
    if (event.httpMethod !== 'POST') return responder(405, { error: 'Método no permitido' });
    try {
        const cuerpo = leerCuerpo(event);
        if (!cuerpo) return responder(400, { error: 'El cuerpo debe ser JSON válido' });

        const aut = await autorizar(event, cuerpo, 'modificar');
        if (aut.error) return aut.error;

        const idActual = cuerpo.public_id;
        if (!idPermitido(idActual, aut.proyecto)) {
            return responder(403, { error: 'La imagen no pertenece a este proyecto' });
        }

        const resto = idActual.slice(aut.proyecto.length + 1);
        const corte = resto.lastIndexOf('/');
        const carpetaActual = corte >= 0 ? resto.slice(0, corte) : '';
        const nombreActual = corte >= 0 ? resto.slice(corte + 1) : resto;

        const carpetaNueva = cuerpo.carpeta !== undefined ? normalizarCarpeta(cuerpo.carpeta, aut.definicion) : carpetaActual;
        if (!carpetaNueva) return responder(400, { error: 'Carpeta destino inválida para este proyecto' });
        const nombreNuevo = cuerpo.nombre !== undefined ? normalizarNombre(cuerpo.nombre) : nombreActual;
        if (!nombreNuevo) return responder(400, { error: 'Nombre inválido' });

        const actualizarContexto = ['titulo', 'descripcion', 'metadatos'].some(k => cuerpo[k] !== undefined);
        const ctx = actualizarContexto ? construirContexto(cuerpo) : null;
        if (ctx && ctx.error) return responder(400, { error: ctx.error });

        let idFinal = idActual;
        const idDestino = `${aut.proyecto}/${carpetaNueva}/${nombreNuevo}`;

        // 1) Renombrar / mover (nunca pisa una imagen existente)
        if (idDestino !== idActual) {
            await llamarApiFirmada('image/rename', {
                from_public_id: idActual, to_public_id: idDestino, overwrite: 'false', type: 'upload'
            });
            idFinal = idDestino;

            // Si cambió de carpeta, el tag de carpeta (que usan los sitios) también cambia
            if (carpetaNueva !== carpetaActual) {
                if (carpetaActual) {
                    await llamarApiFirmada('image/tags', {
                        command: 'remove', tag: etiquetaDe(aut.proyecto, carpetaActual), public_ids: idFinal, type: 'upload'
                    });
                }
                await llamarApiFirmada('image/tags', {
                    command: 'add', tag: etiquetaDe(aut.proyecto, carpetaNueva), public_ids: idFinal, type: 'upload'
                });
            }
        }

        // 2) Reemplazar context completo
        if (ctx) {
            await llamarApiFirmada('image/context', { command: 'remove_all', public_ids: idFinal, type: 'upload' });
            if (ctx.texto) {
                await llamarApiFirmada('image/context', {
                    command: 'add', public_ids: idFinal, context: ctx.texto, type: 'upload'
                });
            }
        }

        return responder(200, { ok: true, public_id: idFinal });
    } catch (err) {
        console.error('modificar-imagen:', err.message);
        return responder(500, { error: 'Error interno al modificar la imagen' });
    }
};

```

### Archivo: `netlify/functions/utilidades/autorizacion.js`
```javascript
const { sesionAdmin, validarEnlace, cargarProyectos, responder } = require('./tokens');

// Devuelve { proyecto, definicion, rol } o { error } (respuesta HTTP lista para retornar).
// - Admin (header Authorization: Bearer <sesion>): puede todo, en el proyecto indicado en el cuerpo.
// - Enlace (cuerpo.token [+ cuerpo.clave]): solo su proyecto y solo los permisos que lleva el token.
async function autorizar(event, cuerpo, permisoRequerido) {
    const proyectos = await cargarProyectos(event);

    if (sesionAdmin(event)) {
        const definicion = proyectos.find(p => p.id === cuerpo.proyecto);
        if (!definicion) return { error: responder(400, { error: 'Proyecto inexistente' }) };
        return { rol: 'admin', proyecto: definicion.id, definicion };
    }

    if (cuerpo.token) {
        const r = validarEnlace(cuerpo.token, cuerpo.clave);
        if (!r.ok) {
            return { error: responder(r.status, r.requiereClave ? { requiereClave: true } : { error: r.error }) };
        }
        if (!r.carga.permisos.includes(permisoRequerido)) {
            return { error: responder(403, { error: 'Este enlace no tiene permiso para esa acción' }) };
        }
        const definicion = proyectos.find(p => p.id === r.carga.proyecto);
        if (!definicion) return { error: responder(400, { error: 'Proyecto inexistente' }) };
        return { rol: 'enlace', proyecto: definicion.id, definicion };
    }

    return { error: responder(401, { error: 'No autorizado' }) };
}

module.exports = { autorizar };

```

### Archivo: `netlify/functions/utilidades/cloudinary.js`
```javascript
const crypto = require('crypto');

// Acepta los mismos nombres de variables que ya usaba consultar-metadatos.js
function credenciales() {
    let cloudName = process.env.CLOUDINARY_CLOUD_NAME || process.env.CLOUD_NAME;
    let apiKey = process.env.CLOUDINARY_API_KEY || process.env.API_KEY;
    let apiSecret = process.env.CLOUDINARY_API_SECRET || process.env.API_SECRET;

    if ((!apiKey || !apiSecret) && process.env.CLOUDINARY_URL) {
        const u = new URL(process.env.CLOUDINARY_URL.replace(/^cloudinary:\/\//, 'https://'));
        cloudName = cloudName || u.hostname;
        apiKey = apiKey || decodeURIComponent(u.username);
        apiSecret = apiSecret || decodeURIComponent(u.password);
    }
    cloudName = cloudName || 'p0qlmlor'; // nombre público de tu cloud (ya está en el JS del sitio)

    if (!apiKey || !apiSecret) {
        throw new Error('Faltan CLOUDINARY_API_KEY / CLOUDINARY_API_SECRET en Netlify');
    }
    return { cloudName, apiKey, apiSecret };
}

// Firma de Cloudinary: parámetros ordenados "k=v&k=v" + secret, SHA-1
function firmar(params, apiSecret) {
    const base = Object.keys(params)
        .filter(k => params[k] !== undefined && params[k] !== null && params[k] !== '')
        .sort()
        .map(k => `${k}=${params[k]}`)
        .join('&');
    return crypto.createHash('sha1').update(base + apiSecret).digest('hex');
}

// API de subida/edición firmada (rename, context, tags)
async function llamarApiFirmada(ruta, params) {
    const { cloudName, apiKey, apiSecret } = credenciales();
    const todos = { ...params, timestamp: Math.floor(Date.now() / 1000) };
    const cuerpo = new URLSearchParams();
    Object.entries(todos).forEach(([k, v]) => {
        if (v !== undefined && v !== null && v !== '') cuerpo.append(k, String(v));
    });
    cuerpo.append('api_key', apiKey);
    cuerpo.append('signature', firmar(todos, apiSecret));

    const res = await fetch(`https://api.cloudinary.com/v1_1/${cloudName}/${ruta}`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
        body: cuerpo
    });
    const datos = await res.json();
    if (!res.ok) throw new Error(datos.error?.message || 'Error de Cloudinary');
    return datos;
}

// Admin API (listar, borrar) con autenticación básica
async function llamarApiAdmin(metodo, ruta, consulta) {
    const { cloudName, apiKey, apiSecret } = credenciales();
    const qs = consulta ? `?${consulta.toString()}` : '';
    const res = await fetch(`https://api.cloudinary.com/v1_1/${cloudName}${ruta}${qs}`, {
        method: metodo,
        headers: { Authorization: `Basic ${Buffer.from(`${apiKey}:${apiSecret}`).toString('base64')}` }
    });
    const datos = await res.json();
    if (!res.ok) throw new Error(datos.error?.message || 'Error de Cloudinary');
    return datos;
}

module.exports = { credenciales, firmar, llamarApiFirmada, llamarApiAdmin };

```

### Archivo: `netlify/functions/utilidades/imagenes.js`
```javascript
const PATRON_SEGMENTO = /^[A-Za-z0-9_\-ñáéíóúÑÁÉÍÓÚ ]{1,60}$/;
const RESERVADOS = ['caption', 'alt'];

function leerCuerpo(event) {
    try {
        const c = JSON.parse(event.body || '{}');
        return c && typeof c === 'object' ? c : null;
    } catch {
        return null;
    }
}

// Valida la carpeta (hasta 3 niveles, solo caracteres seguros). Por defecto se pueden crear carpetas nuevas
// dentro del proyecto, como hace el selector "+ Nueva carpeta"; si un proyecto tiene
// "soloCarpetasDefinidas": true en proyectos.json, solo valen las carpetas listadas allí.
function normalizarCarpeta(carpeta, definicion) {
    const limpia = String(carpeta || '').trim().replace(/^\/+|\/+$/g, '');
    if (!limpia) return null;
    const segmentos = limpia.split('/');
    if (segmentos.length > 3) return null;
    if (!segmentos.every(s => PATRON_SEGMENTO.test(s))) return null;
    if (definicion.soloCarpetasDefinidas === true && !(definicion.carpetas || []).some(c => c.value === segmentos[0])) return null;
    return limpia;
}

// Nombre sin extensión; los caracteres raros se reemplazan por "_"
function normalizarNombre(nombre) {
    const limpio = String(nombre || '').trim().replace(/[^A-Za-z0-9_\-ñáéíóúÑÁÉÍÓÚ ]/g, '_').slice(0, 100);
    return limpio || null;
}

function etiquetaDe(proyecto, carpeta) {
    return `${proyecto.replace(/\//g, '_')}_${carpeta}`; // misma regla que el front actual
}

function idPermitido(publicId, proyecto) {
    return typeof publicId === 'string' && publicId.startsWith(`${proyecto}/`) && !publicId.includes('..');
}

const valorLimpio = (v, max) => String(v ?? '').replace(/[|=\\\r\n]/g, ' ').trim().slice(0, max);

// Arma el "context" de Cloudinary: caption=Título|alt=Descripción|etiqueta=valor
function construirContexto({ titulo, descripcion, metadatos }) {
    const partes = [];
    const t = valorLimpio(titulo, 300);
    const d = valorLimpio(descripcion, 1000);
    if (t) partes.push(`caption=${t}`);
    if (d) partes.push(`alt=${d}`);

    if (metadatos !== undefined && !Array.isArray(metadatos)) return { error: 'metadatos debe ser una lista' };
    const lista = metadatos || [];
    if (lista.length > 20) return { error: 'Máximo 20 metadatos por imagen' };

    for (const m of lista) {
        const nombre = String(m?.name ?? '').replace(/[|=\\]/g, '_').trim().slice(0, 40);
        const valor = valorLimpio(m?.value, 200);
        if (!nombre || !valor) continue;
        if (RESERVADOS.includes(nombre.toLowerCase())) return { error: `"${nombre}" es un nombre reservado` };
        partes.push(`${nombre}=${valor}`);
    }
    return { texto: partes.join('|') };
}

function mapearRecurso(r) {
    const custom = r.context?.custom || {};
    const { caption, alt, ...metadatos } = custom;
    const idx = r.public_id.lastIndexOf('/');
    return {
        public_id: r.public_id,
        carpeta: idx >= 0 ? r.public_id.slice(0, idx) : '',
        url: r.secure_url,
        formato: r.format,
        bytes: r.bytes,
        ancho: r.width,
        alto: r.height,
        creado: r.created_at,
        tags: r.tags || [],
        titulo: caption || '',
        descripcion: alt || '',
        metadatos: Object.entries(metadatos).map(([name, value]) => ({ name, value: String(value) }))
    };
}

module.exports = {
    leerCuerpo, normalizarCarpeta, normalizarNombre, etiquetaDe,
    idPermitido, construirContexto, mapearRecurso
};

```

### Archivo: `netlify/functions/utilidades/tokens.js`
```javascript
const crypto = require('crypto');

const SEIS_MESES_MS = 6 * 30 * 24 * 60 * 60 * 1000;
const CINCO_ANIOS_MS = 5 * 365 * 24 * 60 * 60 * 1000;
const PERMISOS_VALIDOS = ['alta', 'listar', 'modificar', 'eliminar'];

function obtenerSecreto() {
    const secreto = process.env.TOKEN_SECRET;
    if (!secreto || secreto.length < 32) {
        throw new Error('TOKEN_SECRET no está configurado o tiene menos de 32 caracteres');
    }
    return secreto;
}

function firmar(cargaB64) {
    return crypto.createHmac('sha256', obtenerSecreto()).update(cargaB64).digest('base64url');
}

function resumen(texto) {
    return crypto.createHash('sha256').update(String(texto)).digest();
}

// Comparación en tiempo constante (compara resúmenes, así no filtra el largo)
function iguales(a, b) {
    return crypto.timingSafeEqual(resumen(a), resumen(b));
}

function crearToken(carga) {
    const cargaB64 = Buffer.from(JSON.stringify(carga)).toString('base64url');
    return `${cargaB64}.${firmar(cargaB64)}`;
}

// tipoEsperado: 'admin' | 'enlace'. Un token de un tipo nunca sirve como el otro.
function verificarToken(token, tipoEsperado) {
    if (typeof token !== 'string') return null;
    const partes = token.split('.');
    if (partes.length !== 2) return null;
    const [cargaB64, firma] = partes;
    if (!iguales(firma, firmar(cargaB64))) return null;

    let carga;
    try {
        carga = JSON.parse(Buffer.from(cargaB64, 'base64url').toString('utf8'));
    } catch {
        return null;
    }
    if (!carga || carga.t !== tipoEsperado) return null;
    if (typeof carga.exp !== 'number' || Date.now() > carga.exp) return null;
    return carga;
}

// Misma convención que ya tenés en Netlify: TOKEN_SECRET_ENBLANCO_RESIDENCIAL_LARGO
function nombreVariableClaveLarga(proyecto) {
    return `TOKEN_SECRET_${String(proyecto).replace(/[^a-zA-Z0-9]/g, '_').toUpperCase()}_LARGO`;
}

function claveLargaValida(proyecto, claveIngresada) {
    const esperada = process.env[nombreVariableClaveLarga(proyecto)];
    if (!esperada || !claveIngresada) return false;
    return iguales(claveIngresada, esperada);
}

// Para usar en TODAS las funciones que reciben un enlace de cliente
function validarEnlace(token, clave) {
    const carga = verificarToken(token, 'enlace');
    if (!carga) return { ok: false, status: 401, error: 'Enlace inválido o vencido' };
    if (carga.larga) {
        if (!clave) return { ok: false, status: 401, requiereClave: true };
        if (!claveLargaValida(carga.proyecto, clave)) {
            return { ok: false, status: 401, error: 'Contraseña incorrecta' };
        }
    }
    return { ok: true, carga };
}

function sesionAdmin(event) {
    const cabecera = (event.headers && (event.headers.authorization || event.headers.Authorization)) || '';
    const m = cabecera.match(/^Bearer (.+)$/);
    return m ? verificarToken(m[1], 'admin') : null;
}

async function cargarProyectos(event) {
    const base = process.env.DEPLOY_URL || process.env.URL || `https://${event.headers.host}`;
    const res = await fetch(`${base}/proyectos.json`);
    if (!res.ok) throw new Error('No se pudo leer proyectos.json');
    const datos = await res.json();
    return datos.proyectos || [];
}

function responder(statusCode, cuerpo) {
    return {
        statusCode,
        headers: { 'Content-Type': 'application/json', 'Cache-Control': 'no-store' },
        body: JSON.stringify(cuerpo)
    };
}

module.exports = {
    SEIS_MESES_MS,
    CINCO_ANIOS_MS,
    PERMISOS_VALIDOS,
    crearToken,
    verificarToken,
    validarEnlace,
    nombreVariableClaveLarga,
    sesionAdmin,
    cargarProyectos,
    responder
};

```

### Archivo: `netlify/functions/verificar-enlace.js`
```javascript
const { validarEnlace, responder } = require('./utilidades/tokens');

exports.handler = async (event) => {
    if (event.httpMethod !== 'POST') return responder(405, { error: 'Método no permitido' });

    try {
        const { token, clave } = JSON.parse(event.body || '{}');
        const r = validarEnlace(token, clave);

        if (!r.ok) {
            return responder(r.status, r.requiereClave
                ? { valida: false, requiereClave: true }
                : { valida: false, error: r.error });
        }

        const { proyecto, permisos, exp } = r.carga;
        return responder(200, { valida: true, proyecto, permisos, exp });
    } catch (err) {
        return responder(500, { valida: false, error: 'Error al verificar el enlace' });
    }
};

```

### Archivo: `proyectos.json`
```json
{
  "proyectos": [
    {
      "id": "monarca",
      "nombre": "Residencial Monarca",
      "permisosCliente": [
        "alta",
        "listar", 
        "modificar", 
        "eliminar"
        
      ],
      "soloCarpetasDefinidas": true,
      "carpetas": [
        {
          "value": "galeria",
          "label": "Galería"
        },
        {
          "value": "instalaciones",
          "label": "Instalaciones"
        }
      ]
    },
    {
      "id": "enBlanco/residencial",
      "nombre": "Marca Blanca - Residencial",
      "permisosCliente": [
        "alta"
      ],
      "carpetas": [
        {
          "value": "galeria",
          "label": "Galería"
        },
        {
          "value": "instalaciones",
          "label": "Instalaciones"
        },
        {
          "value": "personal",
          "label": "Personal"
        }
      ]
    },
    {
      "id": "patriciaBertoche",
      "nombre": "patriciaBertoche",
      "permisosCliente": [
        "alta"
      ],
      "carpetas": [
        {
          "value": "galeria",
          "label": "Galería"
        },
        {
          "value": "instalaciones",
          "label": "Instalaciones"
        },
        {
          "value": "imagenes",
          "label": "Imagenes"
        },
        {
          "value": "personal",
          "label": "Personal"
        }
      ]
    },
    {
      "id": "pruebas",
      "nombre": "Pruebas (borrar luego)",
      "permisosCliente": [
        "alta"
      ],
      "carpetas": [
        {
          "value": "ensayos",
          "label": "Ensayos"
        }
      ]
    },
    {
      "id": "brochures",
      "nombre": "Brochures",
      "permisosCliente": [
        "alta"
      ],
      "carpetas": [
        {
          "value": "logos",
          "label": "Logos"
        }
      ]
    }
  ]
}
```

### Archivo: `readme.md`
```md
# Gestor de imágenes con Cloudinary

Aplicación web estática (HTML, CSS y JS, sin build) con funciones de Netlify. El administrador gestiona las imágenes de cada proyecto (alta, baja y modificación) y genera enlaces temporales para que terceros suban o gestionen imágenes con los permisos que él decida.

## Manuales de usuario

Hay un manual para el administrador y uno para cada tipo de enlace que puede recibir un cliente. Están en el repositorio [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/gestor-imagenes-cloudinary/documentacion), en Markdown y en PDF:

| Manual | Para quién | Markdown | PDF |
|---|---|---|---|
| Manual del Administrador | Quien ingresa con la contraseña de administrador: crea enlaces, administra imágenes y claves | [MD](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor-imagenes-cloudinary/documentacion/MANUAL_ADMINISTRADOR.md) | [PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor-imagenes-cloudinary/documentacion/Manual_Administrador.pdf) |
| Cliente: carga simplificada | Quien recibe un enlace de *Carga simplificada* (solo subir) | [MD](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor-imagenes-cloudinary/documentacion/MANUAL_CLIENTE_CARGA_SIMPLIFICADA.md) | [PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor-imagenes-cloudinary/documentacion/Manual_Cliente_Carga_Simplificada.pdf) |
| Cliente: galería | Quien recibe un enlace de *Galería* (ver y, según permisos, subir, modificar o eliminar) | [MD](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor-imagenes-cloudinary/documentacion/MANUAL_CLIENTE_GALERIA.md) | [PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor-imagenes-cloudinary/documentacion/Manual_Cliente_Galeria.pdf) |
| Cliente: carga completa | Quien recibe un enlace de *Carga completa* (subir con opciones de tamaño y calidad) | [MD](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor-imagenes-cloudinary/documentacion/MANUAL_CLIENTE_CARGA_COMPLETA.md) | [PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor-imagenes-cloudinary/documentacion/Manual_Cliente_Carga_Completa.pdf) |

Las capturas de los manuales usan proyectos e imágenes de ejemplo.

## Principios de seguridad

- El navegador **nunca** tiene `CLOUDINARY_API_SECRET`. Todo lo que modifica Cloudinary pasa por funciones de Netlify.
- La autorización la decide el servidor: el token del enlace lleva `proyecto`, `permisos` y `exp`, firmados con HMAC-SHA256. El parámetro `modo` de la URL es solo de presentación.
- La carpeta de destino se valida y se firma en el servidor. No existe preset unsigned: las subidas son firmadas y no pueden pisar imágenes existentes (`overwrite=false`).
- Un enlace solo puede actuar sobre su proyecto (`public_id` debe empezar por `proyecto/`).
- Sin `TOKEN_SECRET` (mínimo 32 caracteres) las funciones fallan; no hay clave de respaldo.

## Estructura

```text
index.html                              Login de admin y generador de enlaces
admin.html + js/admin.js                Panel ABM: miniaturas, editar, mover, eliminar, subir
seguridad.html + js/seguridad.js        Genera los valores de las variables de Netlify (contraseña de admin, TOKEN_SECRET, claves largas)
galeria.html + js/galeria.js            Galería para quien entra con enlace: ver/editar/eliminar/subir según permisos
subir.html / subir-cliente.html         Portal de carga (pantalla completa / simplificada); requiere permiso "alta"
js/subir.js                             Portal: validación del enlace, tarjetas, metadatos, subida firmada
js/acceso.js                            Pantalla de contraseña flotante y sesión de admin, COMPARTIDAS por todas las páginas
js/dashboard.js                         Login y generación de enlaces
proyectos.json                          Proyectos, carpetas y permisos por defecto
herramientas/generar-hash-admin.js      Genera ADMIN_PASSWORD_HASH desde la PC: solo para la instalación inicial o si se pierde el acceso
herramientas/probar-*.ps1               Pruebas contra el sitio desde Windows
netlify/functions/
  iniciar-sesion-admin.js               Contraseña de admin -> sesión de 2 horas
  generar-hash-clave.js                 (admin) convierte una contraseña nueva en el valor de ADMIN_PASSWORD_HASH
  generar-enlace.js                     (admin) crea el token del enlace
  verificar-enlace.js                   Valida enlace (+ contraseña si es largo)
  firmar-subida.js                      Permiso "alta": firma la subida a Cloudinary
  listar-imagenes.js                    Permiso "listar" (o "alta" con sugerencias:true)
  modificar-imagen.js                   Permiso "modificar": título, metadatos, nombre, carpeta
  eliminar-imagen.js                    Permiso "eliminar": hasta 50 por llamada
  utilidades/                           tokens, autorizacion, cloudinary, imagenes (compartidas)
```

## Autorización

Cada función acepta una de dos credenciales:

- **Admin**: header `Authorization: Bearer <sesion>` (sesión de `iniciar-sesion-admin`) y `proyecto` en el cuerpo. Puede todo.
- **Enlace**: `token` (y `clave` si es de larga duración) en el cuerpo. Solo su proyecto y solo los permisos del token: `alta`, `listar`, `modificar`, `eliminar`.

Los enlaces con más de 6 meses de vigencia piden una contraseña: el valor de la variable `TOKEN_SECRET_{PROYECTO}_LARGO` (el proyecto sin caracteres especiales y en mayúsculas, p. ej. `TOKEN_SECRET_ENBLANCO_RESIDENCIAL_LARGO`). Cambiar esa variable deja sin acceso a los enlaces largos de ese proyecto.

## Variables de entorno (Netlify)

```text
TOKEN_SECRET                 mínimo 32 caracteres
ADMIN_PASSWORD_HASH          scrypt$sal$hash (se genera en seguridad.html)
CLOUDINARY_API_KEY
CLOUDINARY_API_SECRET
CLOUDINARY_CLOUD_NAME        (opcional si usa CLOUDINARY_URL)
TOKEN_SECRET_*_LARGO         una por proyecto con enlaces largos
```

Tras cambiar variables hay que volver a desplegar.

## proyectos.json

```json
{
  "id": "monarca",
  "nombre": "Residencial Monarca",
  "permisosCliente": ["alta"],
  "soloCarpetasDefinidas": false,
  "carpetas": [{ "value": "galeria", "label": "Galería" }]
}
```

`id` es el prefijo de carpeta en Cloudinary. `permisosCliente` preselecciona los permisos al generar un enlace. Con `soloCarpetasDefinidas: true` solo valen las carpetas listadas; por defecto se pueden crear carpetas nuevas dentro del proyecto (hasta 3 niveles, caracteres seguros).

## Datos de cada imagen

- Carpeta Cloudinary: `proyecto/carpeta`. Tag: `proyecto_carpeta` (con `/` reemplazado por `_`); los sitios que leen por tag dependen de esto, al mover de carpeta el tag se actualiza.
- `context`: `caption` = título, `alt` = descripción, el resto son metadatos personalizados (`etiqueta=valor`). `caption` y `alt` son nombres reservados.
- Nombre generado: `AAAAMMDDHHMMSS_nombre` (editable). Si ya existe una imagen con ese nombre, la subida se rechaza.
- Reglas de metadatos (portal de carga): los textos pueden repetirse; los valores numéricos de una etiqueta no pueden repetirse en la carpeta. El panel de administración avisa pero permite continuar.
- `modificar-imagen`: si se envía `titulo`, `descripcion` o `metadatos`, el context completo se reemplaza; el cliente debe enviar los tres.

## Guía de uso

### Pantallas

| Página | Quién la usa | Para qué |
| --- | --- | --- |
| `index.html` | Administrador | Ingresar con la contraseña de admin y generar enlaces |
| `admin.html` | Administrador | ABM completo: ver, subir, editar, mover y eliminar imágenes de cualquier proyecto |
| `seguridad.html` | Administrador | Generar los valores de `ADMIN_PASSWORD_HASH`, `TOKEN_SECRET` y `TOKEN_SECRET_*_LARGO` |
| `galeria.html` | Quien tiene un enlace | Ver, editar y eliminar imágenes de su proyecto, según los permisos del enlace; botón para subir si tiene `alta` |
| `subir.html` | Quien tiene un enlace con `alta` | Carga con todas las opciones |
| `subir-cliente.html` | Quien tiene un enlace con `alta` | Carga simplificada |

### Permisos de un enlace

| Permiso Interno (JSON/Backend) | Etiqueta en Interfaz | Qué permite |
| --- | --- | --- |
| `alta` | **Subir** | Subir imágenes (y ver las sugerencias de metadatos al subir) |
| `listar` | **Ver** | Ver las imágenes del proyecto (necesario para la galería) |
| `modificar` | **Modificar** | Cambiar título, descripción, metadatos, nombre y carpeta |
| `eliminar` | **Eliminar** | Borrar imágenes |

> **Nota sobre la terminología:** El sistema utiliza términos técnicos estándar (`alta`, `listar`) en el código backend y en el archivo `proyectos.json` para definir la lógica de seguridad. Sin embargo, en la interfaz visual se utilizan verbos cotidianos (`Subir`, `Ver`) para facilitar la experiencia de usuario. Esta separación asegura que el núcleo de seguridad permanezca intacto aunque se cambien los textos o el idioma de la interfaz en el futuro.

El administrador (sesión de `index.html`) no necesita permisos: puede todo en cualquier proyecto.

### Ejemplo: darle a un cliente (Monarca) enlace para ver, editar, modificar y subir

En `index.html`, con la sesión de admin iniciada:

1. **¿Para qué proyecto es?** Residencial Monarca.
2. **¿Qué pantalla verá?** Galería - ver, editar y eliminar (según permisos).
3. **¿Qué podrá hacer?** Marcar las cuatro casillas: subir, ver, modificar y eliminar.
4. **¿Hasta cuándo estará activo?**
   - Corto (recomendado, por ejemplo 30 días): no pide contraseña.
   - Largo (más de 6 meses): el cliente debe ingresar la contraseña, que es el valor de la variable `TOKEN_SECRET_MONARCA_LARGO` en Netlify. La variable debe existir antes de generar el enlace; la contraseña se le pasa al cliente por otro medio.
5. **Generar enlace**, copiarlo y enviarlo.

El cliente verá las miniaturas de Monarca con los botones Editar y Eliminar, selección múltiple para borrar y un botón **Subir imágenes** que lo lleva a la pantalla de carga con el mismo enlace.

Editar y eliminar actúan sobre las imágenes reales, que son las que leen los sitios por tag.

Para que las cuatro casillas aparezcan marcadas por defecto en Monarca, usar `permisosCliente` en `proyectos.json` (ver el ejemplo siguiente).

### Ejemplo: restringir un proyecto a sus carpetas (`soloCarpetasDefinidas`)

Por defecto cualquier enlace puede crear carpetas nuevas dentro de su proyecto. Para que Monarca acepte solo `galeria` e `instalaciones`:

```json
{
  "id": "monarca",
  "nombre": "Residencial Monarca",
  "permisosCliente": ["alta", "listar", "modificar", "eliminar"],
  "soloCarpetasDefinidas": true,
  "carpetas": [
    { "value": "galeria", "label": "Galería" },
    { "value": "instalaciones", "label": "Instalaciones" }
  ]
}
```

- El servidor rechaza cualquier otra carpeta con "Carpeta destino inválida para este proyecto".
- Los proyectos sin `soloCarpetasDefinidas` siguen aceptando carpetas nuevas.
- La opción "+ Nueva carpeta" de la pantalla de carga y el campo de carpeta de la galería siguen visibles, pero con la restricción activa el servidor las rechaza.
- Para sumar una carpeta, agregarla a la lista `carpetas`.

### Agregar un proyecto nuevo

1. Agregarlo a `proyectos.json` con `id` (prefijo de carpeta en Cloudinary), `nombre`, `permisosCliente` y `carpetas`.
2. Si va a tener enlaces largos, crear en Netlify la variable `TOKEN_SECRET_{ID}_LARGO` (el id sin caracteres especiales y en mayúsculas; `enBlanco/residencial` pasa a `ENBLANCO_RESIDENCIAL`) con la contraseña elegida, y volver a desplegar.

### Mantenimiento

- **Cambiar claves:** entrar a `seguridad.html` (botón "Claves" de `index.html`). Genera el valor de cada variable, pero **no la cambia en Netlify**: hay que copiarlo en Netlify → Site configuration → Environment variables y volver a desplegar.
  - `ADMIN_PASSWORD_HASH`: se escribe la contraseña nueva y se obtiene el valor cifrado. La contraseña actual sigue valiendo hasta cambiar la variable y redesplegar.
  - `TOKEN_SECRET`: clave aleatoria de 64 caracteres. Al cambiarla se invalidan todos los enlaces y sesiones.
  - `TOKEN_SECRET_{PROYECTO}_LARGO`: se elige el proyecto, se escribe o se genera la contraseña y se copian el nombre de la variable y su valor. Al cambiarla dejan de funcionar los enlaces largos de ese proyecto.
  - Nada de lo que se genera se guarda. La página no puede modificar Netlify por sí sola a propósito: para eso la aplicación necesitaría un token de Netlify con permisos sobre todo el sitio.
- **Instalación inicial o acceso perdido:** `seguridad.html` exige entrar como admin, así que la primera vez (o si se pierde la contraseña) el valor de `ADMIN_PASSWORD_HASH` hay que generarlo con `node herramientas/generar-hash-admin.js "clave"` y pegarlo en Netlify. Por eso conviene conservar ese archivo.
- **Cortar enlaces en una emergencia:** cambiar `TOKEN_SECRET` invalida todos los enlaces y sesiones; cambiar la variable `_LARGO` de un proyecto invalida solo sus enlaces largos. No hay revocación individual.
- **Después de cambiar variables de entorno** siempre hay que volver a desplegar.
- **Pantalla de contraseña:** las cinco páginas que piden contraseña (`index`, `admin`, `galeria`, `subir`, `subir-cliente`) usan la misma tarjeta flotante central, definida en `js/acceso.js`. Para cambiar su aspecto o sus textos se edita solo ese archivo. La contraseña de un enlace largo se escribe una vez por pestaña (se guarda en `sessionStorage` y se reutiliza al pasar de la galería a la carga).

### Lista de verificación de seguridad

- [ ] `TOKEN_SECRET` cargado (32 caracteres o más) y `ADMIN_PASSWORD_HASH` generado.
- [ ] Credenciales de Cloudinary cargadas (`CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`).
- [ ] Eliminados del repo `generar-token.js`, `verificar-token.js` y `consultar-metadatos.js` (versión anterior, sin autenticación).
- [ ] Preset `subir_gestor` de Cloudinary en modo **Signed** o eliminado (si sigue unsigned, cualquiera puede subir sin enlace).
- [ ] Contraseña de admin que no haya sido compartida en ningún chat o documento.
- [ ] Enlaces nuevos con vencimientos cortos siempre que sea posible.

## Cómo probar

- `herramientas/probar-generar-enlace.ps1`: login y generación de enlace.
- `herramientas/probar-imagenes.ps1`: ciclo completo (subir, listar, modificar, eliminar) en el proyecto `pruebas`.

## Limitaciones

- No hay pruebas automatizadas ni build; se prueba contra Netlify.
- No hay bloqueo por intentos fallidos de login (solo una espera de 0,8 s).
- Los enlaces no se pueden revocar uno por uno: vencen, o se invalidan cambiando `TOKEN_SECRET` (todos) o la variable `_LARGO` (los largos de un proyecto). Por eso conviene usar vencimientos cortos.
- Las funciones no usan almacenamiento externo: todo el estado está en los tokens firmados.
```

### Archivo: `seguridad.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Claves y seguridad | Upload Hub</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="css/styles.css">
</head>
<body class="min-h-screen p-4 md:p-8">

    <div id="app" class="max-w-2xl mx-auto space-y-4 interfaz-bloqueada">
        <header class="flex flex-wrap items-center gap-3">
            <h1 class="text-xl font-bold mr-auto">🔑 Claves y seguridad</h1>
            <a href="index.html" class="text-sm underline">Generador de enlaces</a>
            <a href="admin.html" class="text-sm underline">Administrar imágenes</a>
        </header>

        <p class="text-sm bg-blue-50 text-blue-900 rounded-lg p-3">
            Esta página <b>genera</b> los valores, pero no cambia nada sola: copiá cada valor en
            Netlify → Site configuration → Environment variables y después volvé a desplegar
            (Deploys → Trigger deploy). Nada de lo que se genera acá se guarda.
        </p>

        <!-- ADMIN_PASSWORD_HASH -->
        <section class="bg-white rounded-xl shadow p-5 space-y-3">
            <h2 class="font-bold">Contraseña de administrador</h2>
            <p class="text-sm text-slate-500">Variable: <code>ADMIN_PASSWORD_HASH</code>. Escribí la contraseña nueva (mínimo 12 caracteres) y se genera el valor cifrado para pegar en Netlify. Hasta que lo cambies y redespliegues, sigue valiendo la contraseña actual.</p>
            <input type="password" id="claveNueva" placeholder="Contraseña nueva" autocomplete="new-password" class="border rounded px-3 py-2 w-full">
            <input type="password" id="claveRepetida" placeholder="Repetí la contraseña nueva" autocomplete="new-password" class="border rounded px-3 py-2 w-full">
            <p id="errorHash" class="text-sm text-red-600"></p>
            <button type="button" id="btnHash" class="btn-primary">Generar valor</button>
            <div id="resultadoHash" class="hidden space-y-2">
                <textarea id="valorHash" rows="3" readonly class="border rounded px-3 py-2 w-full font-mono text-xs"></textarea>
                <button type="button" id="btnCopiarHash" class="btn-secondary py-2">📋 Copiar</button>
            </div>
        </section>

        <!-- TOKEN_SECRET -->
        <section class="bg-white rounded-xl shadow p-5 space-y-3">
            <h2 class="font-bold">Clave de firma de enlaces y sesiones</h2>
            <p class="text-sm text-slate-500">Variable: <code>TOKEN_SECRET</code>. Genera una clave aleatoria de 64 caracteres. <b>Al cambiarla se invalidan todos los enlaces ya entregados y tu sesión actual.</b></p>
            <button type="button" id="btnSecreto" class="btn-primary">Generar clave aleatoria</button>
            <div id="resultadoSecreto" class="hidden space-y-2">
                <textarea id="valorSecreto" rows="2" readonly class="border rounded px-3 py-2 w-full font-mono text-xs"></textarea>
                <button type="button" id="btnCopiarSecreto" class="btn-secondary py-2">📋 Copiar</button>
            </div>
        </section>

        <!-- TOKEN_SECRET_*_LARGO -->
        <section class="bg-white rounded-xl shadow p-5 space-y-3">
            <h2 class="font-bold">Contraseña de enlaces largos de un proyecto</h2>
            <p class="text-sm text-slate-500">Es la contraseña que tendrá que escribir quien abra un enlace de más de 6 meses. <b>Al cambiarla dejan de funcionar los enlaces largos de ese proyecto.</b> Se la pasás al cliente por otro medio.</p>
            <label class="text-sm block">Proyecto
                <select id="selProyecto" class="border rounded px-3 py-2 w-full"></select>
            </label>
            <label class="text-sm block">Nombre de la variable en Netlify
                <div class="flex gap-2">
                    <input type="text" id="nombreVariable" readonly class="border rounded px-3 py-2 w-full font-mono text-xs">
                    <button type="button" id="btnCopiarNombre" class="btn-secondary py-2">📋</button>
                </div>
            </label>
            <label class="text-sm block">Contraseña (escribila vos o generá una)
                <div class="flex gap-2">
                    <input type="text" id="claveLarga" autocomplete="off" class="border rounded px-3 py-2 w-full font-mono text-sm">
                    <button type="button" id="btnAleatoria" class="btn-secondary py-2 whitespace-nowrap">🎲 Generar</button>
                    <button type="button" id="btnCopiarClaveLarga" class="btn-secondary py-2">📋</button>
                </div>
            </label>
        </section>
    </div>

    <script src="js/acceso.js"></script>
    <script src="js/seguridad.js"></script>
</body>
</html>
```

### Archivo: `subir-cliente.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cargar imágenes | Gestor</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="css/styles.css">
</head>
<body class="cliente-page min-h-screen p-4 md:p-8">
    <div class="max-w-4xl mx-auto">
        <div id="loadingState" class="text-center py-24"><div class="spinner"></div><p class="text-base font-medium animate-pulse" style="color: var(--text-muted);">Validando acceso seguro...</p></div>
        <div id="errorState" class="card max-w-md mx-auto text-center hidden mt-12 p-8 shadow-xl" style="border-top: 4px solid #EF4444;"><div class="icono-estado">🔒</div><h1 class="text-2xl font-bold mb-3" style="color: #EF4444;">Enlace no válido o caducado</h1><p id="errorMessage" style="color: var(--text-muted); font-size: 0.95rem;">Por seguridad, solicita un enlace nuevo al administrador.</p></div>
        
        <div id="mainInterface" class="hidden">
            <!-- TARJETA 1: Cabecera Principal unificada -->
            <header class="mb-4 card p-5 md:p-6 shadow-sm" style="border-top: 4px solid var(--primary-color, #2563EB);">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                    <div class="flex items-start gap-3.5">
                        <div class="hero-icono w-11 h-11 text-xl rounded-xl flex items-center justify-center shrink-0" style="background: var(--bg-color, #F1F5F9);">⬆️</div>
                        <div>
                            <div class="flex items-center gap-2 mb-1 flex-wrap">
                                <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-blue-50 text-blue-700 inline-block"> 🔒 Portal seguro</span>
                                <a id="btnVolverGaleria" href="#" class="hidden text-xs font-semibold px-2.5 py-1 rounded-full bg-slate-100 text-slate-700 hover:bg-slate-200 transition-colors">← Volver a la galería</a>
                            </div>
                            <h1 class="text-2xl md:text-3xl font-bold tracking-tight" style="color: var(--text-main);">Sube tus imágenes</h1>
                            <p class="text-sm mt-1" style="color: var(--text-muted);">Proyecto: <strong id="displayProjectName" class="capitalize" style="color: var(--text-main);"></strong></p>
                            <p class="text-xs mt-1 font-medium" style="color: #D97706;">Válido hasta: <span id="displayExpDate"></span></p>
                        </div>
                    </div>
                </div>
            </header>

            <!-- TARJETA 2: Panel Fino de Carpeta y Selector Horizontal -->
            <section class="card p-4 md:p-5 shadow-sm mb-4">
                <div class="space-y-4">
                    <div>
                        <label for="globalCategory" class="paso-label text-xs font-semibold block mb-1.5" style="color: var(--text-muted);"><span class="numero-paso">1</span> Carpeta destino</label>
                        <select id="globalCategory" class="border rounded px-3 py-2 text-xs focus-ring w-full sm:max-w-xs bg-slate-50 font-medium"></select>
                    </div>

                    <hr class="border-slate-100 my-2">

                    <div>
                        <h2 class="paso-titulo text-xs uppercase tracking-wider mb-2" style="color: var(--text-muted);"><span class="numero-paso">2</span> Elige tus imágenes</h2>
                        <label for="fileInput" class="border-2 border-dashed py-3 px-5 flex items-center justify-center gap-3 cursor-pointer transition-all duration-200 group hover:border-blue-600 hover:bg-blue-50/20 text-center rounded-xl bg-slate-50/40" style="border-color: var(--border-color); min-height: unset;">
                            <div class="w-8 h-8 rounded-full bg-blue-50 flex items-center justify-center text-blue-600 shrink-0"><span class="text-sm">▧</span></div>
                            <div class="text-left flex items-center gap-2 flex-wrap">
                                <span class="text-xs font-semibold" style="color: var(--text-main);">Toca o arrastra tus imágenes aquí</span>
                                <span class="text-[11px]" style="color: var(--text-muted);">· Puedes elegir varias a la vez</span>
                            </div>
                            <input type="file" id="fileInput" multiple accept="image/*" class="hidden">
                        </label>
                    </div>

                    <div id="batchActions" class="hidden mt-3 pt-3 border-t border-slate-100 flex justify-between items-center gap-3">
                        <div class="flex items-center gap-2">
                            <span class="numero-paso">3</span>
                            <span id="counterText" class="text-sm font-bold" style="color: var(--text-main);">0 imágenes cargadas</span>
                        </div>
                        <button id="uploadAllBtn" class="btn-primary py-2 px-4 text-xs md:text-sm w-auto">Subir todas</button>
                    </div>
                </div>
            </section>

            <h2 id="cardsStepTitle" class="paso-titulo hidden text-sm font-semibold mb-3"><span class="numero-paso">3</span> Revisa tus imágenes</h2>
            <div id="cardsContainer" class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-4"></div>
        </div>
    </div>
    <div id="uploadEditModal" class="upload-edit-modal hidden" role="dialog" aria-modal="true" aria-labelledby="uploadEditTitle">
        <form id="uploadEditForm" class="upload-edit-panel">
            <div class="upload-edit-header"><div><p class="upload-edit-kicker">Editar imagen</p><h2 id="uploadEditTitle">Detalles de la imagen</h2></div><button type="button" id="uploadEditClose" class="upload-edit-close" aria-label="Cerrar">×</button></div>
            <div id="uploadEditPreview" class="upload-edit-preview"></div>
            <label>Nombre<input id="uploadEditName" type="text" required></label>
            <label>Carpeta<select id="uploadEditCategory"></select></label>
            <div class="upload-edit-grid"><label>Ancho máximo<select id="uploadEditMaxWidth"><option value="800">800 px</option><option value="1200">1200 px</option><option value="1600">1600 px</option><option value="1920">1920 px</option><option value="0">Original</option></select></label><label>Calidad WebP<input id="uploadEditQuality" type="range" min="0.1" max="1" step="0.05"></label></div>
            <label>Título<input id="uploadEditTitulo" type="text" maxlength="300"></label><label>Descripción<textarea id="uploadEditDescription" rows="3" maxlength="1000"></textarea></label>
            <div><div class="upload-edit-label-row"><span>Metadatos</span><button type="button" id="uploadEditAddMeta" class="upload-edit-link">+ Agregar</button></div><div id="uploadEditMetadata" class="upload-edit-metadata"></div><datalist id="uploadMetaNombres"></datalist></div>
            <div class="upload-edit-actions"><button type="button" id="uploadEditCancel" class="btn-secondary">Cancelar</button><button type="submit" class="btn-primary">Guardar cambios</button></div>
        </form>
    </div>
    <script src="js/acceso.js"></script>
    <script src="js/subir.js"></script>
</body>
</html>
```

### Archivo: `subir.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Subir Archivos | Gestor de Imágenes</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="css/styles.css">
</head>
<body class="min-h-screen p-4 md:p-8">

    <div class="max-w-4xl mx-auto">
        <!-- Mensaje de Validación Inicial -->
        <div id="loadingState" class="text-center py-24">
            <div class="spinner"></div>
            <p class="text-base font-medium animate-pulse" style="color: var(--text-muted);">Validando acceso seguro...</p>
        </div>

        <!-- Pantalla de Error -->
        <div id="errorState" class="card max-w-md mx-auto text-center hidden mt-12 p-8 shadow-xl" style="border-top: 4px solid #EF4444;">
            <div class="icono-estado">🔒</div>
            <h1 class="text-2xl font-bold mb-3" style="color: #EF4444;">Enlace no válido o caducado</h1>
            <p id="errorMessage" style="color: var(--text-muted); font-size: 0.95rem;">Por seguridad, los enlaces de subida tienen una fecha límite estricta. Solicita uno nuevo al administrador.</p>
        </div>

        <!-- Interfaz Principal -->
        <div id="mainInterface" class="hidden">
            
            <!-- TARJETA 1: Cabecera Principal unificada -->
            <header class="mb-4 card p-5 md:p-6 shadow-sm" style="border-top: 4px solid var(--primary-color, #2563EB);">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                    <div class="flex items-start gap-3.5">
                        <div class="hero-icono w-11 h-11 text-xl rounded-xl flex items-center justify-center shrink-0" style="background: var(--bg-color, #F1F5F9);">⬆️</div>
                        <div>
                            <div class="flex items-center gap-2 mb-1 flex-wrap">
                                <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-blue-50 text-blue-700 inline-block">🔒 Portal seguro</span>
                                <a id="btnVolverGaleria" href="#" class="hidden text-xs font-semibold px-2.5 py-1 rounded-full bg-slate-100 text-slate-700 hover:bg-slate-200 transition-colors">← Volver a la galería</a>
                            </div>
                            <h1 class="text-2xl md:text-3xl font-bold tracking-tight" style="color: var(--text-main);">Sube tus imágenes</h1>
                            <p class="text-sm mt-1" style="color: var(--text-muted);">Proyecto: <strong id="displayProjectName" class="capitalize" style="color: var(--text-main);"></strong></p>
                            <p class="text-xs mt-1 font-medium" style="color: #D97706;">Válido hasta: <span id="displayExpDate"></span></p>
                        </div>
                    </div>
                </div>
            </header>

            <!-- TARJETA 2: Panel Fino y Compacto -->
            <section class="card p-4 md:p-5 shadow-sm mb-4">
                <div class="space-y-4">
                    <div>
                        <h2 class="paso-titulo text-xs uppercase tracking-wider mb-2" style="color: var(--text-muted);"><span class="numero-paso">1</span> Configuración general</h2>
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 items-end gap-3 w-full">
                            <div>
                                <label for="globalCategory" class="text-xs font-semibold block mb-1" style="color: var(--text-muted);">Carpeta destino</label>
                                <select id="globalCategory" class="border rounded px-2.5 py-2 text-xs focus-ring w-full bg-slate-50 font-medium"></select>
                            </div>
                            <div>
                                <label for="globalMaxWidth" class="text-xs font-semibold block mb-1" style="color: var(--text-muted);">Ancho Máx.</label>
                                <select id="globalMaxWidth" class="border rounded px-2.5 py-2 text-xs focus-ring w-full bg-slate-50 font-medium">
                                    <option value="800">800 px (Liviana)</option>
                                    <option value="1200" selected>1200 px (Estándar)</option>
                                    <option value="1600">1600 px (Intermedia)</option>
                                    <option value="1920">1920 px (Alta / HD)</option>
                                    <option value="0">Original</option>
                                </select>
                            </div>
                            <div>
                                <label for="globalQuality" class="text-xs font-semibold block mb-1" style="color: var(--text-muted);">Compresión (<span id="globalQualityVal">80</span>%)</label>
                                <input type="range" id="globalQuality" min="0.1" max="1.0" step="0.05" value="0.8" class="w-full accent-blue-600 cursor-pointer h-7">
                            </div>
                            <div>
                                <label for="globalTitle" class="text-xs font-semibold block mb-1" style="color: var(--text-muted);">Título global</label>
                                <input type="text" id="globalTitle" placeholder="Ej.: Actividad 2026" class="border rounded px-2.5 py-2 text-xs focus-ring w-full bg-slate-50">
                            </div>
                        </div>
                        <div class="flex items-center gap-2 mt-2.5">
                            <input type="checkbox" id="globalKeepOriginal" checked class="w-4 h-4 cursor-pointer accent-blue-600">
                            <label for="globalKeepOriginal" class="text-xs cursor-pointer font-medium" style="color: var(--text-main);">Respetar nombre original</label>
                        </div>
                    </div>

                    <hr class="border-slate-100 my-2">

                    <!-- Selector de archivos ultra fino y horizontal -->
                    <div>
                        <h2 class="paso-titulo text-xs uppercase tracking-wider mb-2" style="color: var(--text-muted);"><span class="numero-paso">2</span> Elige tus imágenes</h2>
                        <label for="fileInput" class="border-2 border-dashed py-3 px-5 flex items-center justify-center gap-3 cursor-pointer transition-all duration-200 group hover:border-blue-600 hover:bg-blue-50/20 text-center rounded-xl bg-slate-50/40" style="border-color: var(--border-color); min-height: unset;">
                            <div class="w-8 h-8 rounded-full bg-blue-50 flex items-center justify-center text-blue-600 group-hover:scale-110 transition-transform shrink-0">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
                                </svg>
                            </div>
                            <div class="text-left flex items-center gap-2 flex-wrap">
                                <span class="text-xs font-semibold" style="color: var(--text-main);">Toca o arrastra tus imágenes aquí</span>
                                <span class="text-[11px]" style="color: var(--text-muted);">· Puedes elegir varias a la vez</span>
                            </div>
                            <input type="file" id="fileInput" multiple accept="image/*" class="hidden">
                        </label>
                    </div>
                </div>
            </section>

            <!-- Acciones Globales Flotantes / de Lote -->
            <div id="batchActions" class="card hidden mb-4 flex flex-col sm:flex-row justify-between items-center gap-4 p-4 shadow-md bg-white border-l-4 border-l-blue-600">
                <span id="counterText" class="text-sm font-bold" style="color: var(--text-main);">0 imágenes cargadas</span>
                <div class="flex flex-wrap gap-2.5 w-full sm:w-auto justify-end">
                    <button id="uploadAllBtn" class="btn-primary flex-1 sm:flex-none py-2 px-4 text-xs md:text-sm">
                        🚀 Subir todas a Cloudinary
                    </button>
                    <button id="downloadAllBtn" class="btn-secondary flex-1 sm:flex-none py-2 px-3 text-xs md:text-sm">
                        📥 Descargar .WebP
                    </button>
                </div>
            </div>

            <!-- Contenedor de Tarjetas (Grid Optimizado) -->
            <div id="cardsContainer" class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-4"></div>
        </div>
    </div>

    <div id="uploadEditModal" class="upload-edit-modal hidden" role="dialog" aria-modal="true" aria-labelledby="uploadEditTitle">
        <form id="uploadEditForm" class="upload-edit-panel">
            <div class="upload-edit-header">
                <div>
                    <p class="upload-edit-kicker">Editar imagen</p>
                    <h2 id="uploadEditTitle">Detalles de la imagen</h2>
                </div>
                <button type="button" id="uploadEditClose" class="upload-edit-close" aria-label="Cerrar">×</button>
            </div>
            <div id="uploadEditPreview" class="upload-edit-preview"></div>
            <label>Nombre<input id="uploadEditName" type="text" required></label>
            <label>Carpeta<select id="uploadEditCategory"></select></label>
            <div class="upload-edit-grid">
                <label>Ancho máximo<select id="uploadEditMaxWidth"><option value="800">800 px</option><option value="1200">1200 px</option><option value="1600">1600 px</option><option value="1920">1920 px</option><option value="0">Original</option></select></label>
                <label>Calidad WebP<input id="uploadEditQuality" type="range" min="0.1" max="1" step="0.05"></label>
            </div>
            <label>Título<input id="uploadEditTitulo" type="text" maxlength="300"></label>
            <label>Descripción<textarea id="uploadEditDescription" rows="3" maxlength="1000"></textarea></label>
            <div><div class="upload-edit-label-row"><span>Metadatos</span><button type="button" id="uploadEditAddMeta" class="upload-edit-link">+ Agregar</button></div><div id="uploadEditMetadata" class="upload-edit-metadata"></div><datalist id="uploadMetaNombres"></datalist></div>
            <div class="upload-edit-actions"><button type="button" id="uploadEditCancel" class="btn-secondary">Cancelar</button><button type="submit" class="btn-primary">Guardar cambios</button></div>
        </form>
    </div>

    <!-- Scripts externos -->
    <script src="js/acceso.js"></script>
    <script src="js/subir.js"></script>
</body>
</html>
```

