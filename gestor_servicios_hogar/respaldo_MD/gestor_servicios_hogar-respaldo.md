# Bundle del Proyecto para IA: gestor_servicios_hogar

## Estructura de Archivos
- README.md
- css/styles.css
- documentacion/LEEME.md
- index.html
- js/app.js
- js/backends/backendGithub.js
- js/backends/backendLocalStorage.js
- js/crypto.js
- js/dataStore.js
- js/export-word.js
- js/session.js
- mio.txt
- netlify.toml
- netlify/functions/data.js

---

## Código Fuente

### Archivo: `README.md`
```md
# Registro de Servicios del Hogar

Aplicación web personal para tener en un solo lugar los servicios y cuentas de la casa: UTE, ANTEL, inmueble, vehículos, colegio y otros. Para cada uno se anotan el número de cuenta, contrato o padrón y notas. Los datos se guardan cifrados.

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/gestor_servicios_hogar/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor_servicios_hogar/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## Funcionalidades

- **Acceso con contraseña**: la misma contraseña se usa para cifrar y descifrar los datos.
- **Categorías**: Hogar (UTE, ANTEL, etc.), Inmueble, Vehículos, Colegio y Varios.
- **Alta, edición y borrado** de servicios, con nombres sugeridos según la categoría.
- Campos de cada servicio: concepto, número de cuenta / contrato / padrón y notas.
- **Exportar a Word**, en versión completa o compacta, para imprimir o archivar.
- Indicador del estado de sincronización.

## Cómo se usa

1. Abrí la app e ingresá la contraseña.
2. Elegí una categoría y cargá un servicio con **Guardar Servicio**.
3. Para modificar o borrar, usá las acciones de la tabla.
4. Para tener una copia en papel o en archivo, usá **Exportar Servicios (Word)** o la versión compacta.

## Privacidad

- La contraseña vive solo en la memoria de la pestaña. No se guarda en el navegador ni se envía a ningún servidor.
- Los datos se cifran en el navegador (AES-GCM con clave derivada por PBKDF2) antes de salir de la computadora. Lo que se guarda en GitHub es solo el paquete cifrado.
- Si se pierde la contraseña, los datos no se pueden recuperar.

## Cómo funciona

- El frontend es HTML, JavaScript modular y Tailwind (desde CDN), sin build.
- `js/dataStore.js` es la única capa de datos: cifra antes de guardar y descifra al leer.
- `js/backends/backendGithub.js` habla con la Netlify Function `netlify/functions/data.js`, que guarda el archivo en un repositorio de GitHub. El token nunca llega al navegador.
- `js/backends/backendLocalStorage.js` es una alternativa para usar sin conexión o durante el desarrollo.

## Publicación en Netlify

`netlify.toml` ya define la carpeta de funciones. Configurar estas variables de entorno:

| Variable | Para qué sirve |
|---|---|
| `GITHUB_TOKEN` | Token fine-grained con permiso *Contents: Read and write* sobre el repositorio de datos. |
| `GITHUB_OWNER` | Dueño del repositorio de datos (por ejemplo `FABIOR1981`). |
| `GITHUB_REPO` | Repositorio de datos (por ejemplo `bd`). |
| `GITHUB_BASE_PATH` | Carpeta dentro de ese repositorio (por ejemplo `gestor_Servicios`). |

## Estructura

```
index.html                         Pantalla de acceso y registro de servicios
css/styles.css                     Estilos
js/app.js                          Lógica de la interfaz
js/dataStore.js                    Cifrado y acceso a datos
js/crypto.js                       Cifrado AES-GCM + PBKDF2
js/session.js                      Contraseña en memoria
js/export-word.js                  Exportación a Word
js/backends/                       Backend GitHub (vía Netlify) y backend local
netlify/functions/data.js          Lee y guarda el archivo cifrado en GitHub
netlify.toml                       Configuración de Netlify
```

```

### Archivo: `css/styles.css`
```css
body {
    font-family: 'Inter', sans-serif;
    background-color: #f1f5f9;
}

/* Tabs de categoría */
.tab-active {
    border-bottom: 3px solid #0038a8;
    color: #0038a8;
    font-weight: 600;
    background-color: #eff6ff;
}

.transition-all-200 {
    transition: all 0.2s ease-in-out;
}

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/gestor_servicios_hogar/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro de Servicios del Hogar</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: { sans: ['Inter', 'sans-serif'] },
                    colors: { uyblue: '#0038a8', uygold: '#fcd116', uybg: '#f8fafc' }
                }
            }
        }
    </script>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body class="text-slate-800 min-h-screen flex flex-col">

    <!-- Portón de contraseña: bloquea todo hasta ingresar la clave de sesión -->
    <div id="password-gate" class="fixed inset-0 bg-slate-900/95 z-[100] flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-2xl p-8 max-w-sm w-full">
            <div class="text-center mb-5">
                <i class="fa-solid fa-lock text-4xl text-uyblue mb-3"></i>
                <h2 class="text-lg font-bold text-slate-800">Acceso protegido</h2>
                <p class="text-xs text-slate-500 mt-2 leading-relaxed">
                    Los datos están cifrados. Si es la primera vez, la contraseña que pongas acá va a ser la que cifre todo de acá en adelante.
                    <span class="text-amber-700 font-semibold">Si la olvidás, los datos no se pueden recuperar.</span>
                </p>
            </div>
            <form id="password-form">
                <input type="password" id="password-input" placeholder="Contraseña" autocomplete="current-password" class="w-full border border-slate-300 rounded-lg p-3 text-sm mb-2 outline-none focus:ring-2 focus:ring-blue-500" required autofocus>
                <p id="password-error" class="hidden text-xs text-red-600 mb-3"></p>
                <button type="submit" id="password-submit" class="w-full bg-blue-700 hover:bg-blue-800 text-white font-semibold py-2.5 rounded-lg flex items-center justify-center gap-2">
                    <i class="fa-solid fa-unlock"></i> Entrar
                </button>
            </form>
        </div>
    </div>

    <div id="app-content" class="hidden flex flex-col min-h-screen w-full">

    <!-- Header -->
    <header class="bg-gradient-to-r from-slate-900 via-uyblue to-blue-900 text-white shadow-lg sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-4 sm:px-6 lg:px-8 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3">
                <div class="bg-white/10 p-3 rounded-xl backdrop-blur-md">
                    <i class="fa-solid fa-house-user text-2xl text-amber-300"></i>
                </div>
                <div>
                    <h1 class="text-2xl font-bold tracking-tight">Registro de Servicios del Hogar</h1>
                    <p class="text-xs text-blue-200">Uruguay</p>
                </div>
            </div>
            <div id="sync-status" class="text-xs text-blue-200 flex items-center gap-2">
                <i class="fa-solid fa-circle-notch fa-spin"></i> Cargando datos...
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 flex-1 w-full">

        <!-- Stats -->
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-6">
            <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-200 flex items-center gap-4">
                <div class="bg-blue-100 text-blue-700 p-3 rounded-xl"><i class="fa-solid fa-plug text-xl"></i></div>
                <div>
                    <p class="text-xs text-slate-500 uppercase font-bold tracking-wider">Servicios Registrados</p>
                    <p id="stat-servicios-count" class="text-2xl font-bold text-slate-800">0</p>
                </div>
            </div>
        </div>

        <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden mb-6">
            <div class="flex overflow-x-auto border-b border-slate-200" id="category-tabs">
                <button onclick="app.switchCategory('hogar')" id="tab-hogar" class="tab-btn tab-active px-6 py-4 text-sm font-medium flex items-center gap-2 whitespace-nowrap">
                    <i class="fa-solid fa-bolt text-yellow-500"></i> Hogar (UTE, ANTEL, etc.)
                </button>
                <button onclick="app.switchCategory('inmueble')" id="tab-inmueble" class="tab-btn px-6 py-4 text-sm font-medium flex items-center gap-2 whitespace-nowrap text-slate-600">
                    <i class="fa-solid fa-building text-blue-500"></i> Inmueble
                </button>
                <button onclick="app.switchCategory('vehiculos')" id="tab-vehiculos" class="tab-btn px-6 py-4 text-sm font-medium flex items-center gap-2 whitespace-nowrap text-slate-600">
                    <i class="fa-solid fa-car text-red-500"></i> Vehículos
                </button>
                <button onclick="app.switchCategory('colegio')" id="tab-colegio" class="tab-btn px-6 py-4 text-sm font-medium flex items-center gap-2 whitespace-nowrap text-slate-600">
                    <i class="fa-solid fa-graduation-cap text-emerald-500"></i> Colegio
                </button>
                <button onclick="app.switchCategory('varios')" id="tab-varios" class="tab-btn px-6 py-4 text-sm font-medium flex items-center gap-2 whitespace-nowrap text-slate-600">
                    <i class="fa-solid fa-asterisk text-purple-500"></i> Varios
                </button>
            </div>

            <div class="p-6">
                <form id="servicio-form" class="bg-slate-50 p-5 rounded-xl border border-slate-200 mb-8">
                    <div class="flex justify-between items-center mb-4">
                        <h3 id="servicio-form-title" class="text-lg font-bold text-slate-800 flex items-center gap-2">
                            <i class="fa-solid fa-plus-circle text-blue-600"></i> NUEVO SERVICIO
                        </h3>
                        <span id="servicio-editing-badge" class="hidden bg-amber-100 text-amber-800 text-xs px-2.5 py-1 rounded-full font-semibold">Modificando</span>
                    </div>
                    <input type="hidden" id="servicio-id" value="">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <div>
                            <label class="block text-xs font-semibold text-slate-600 mb-1">Servicio / Concepto *</label>
                            <select id="field-service" onchange="app.checkCustomService(this.value)" class="w-full bg-white border border-slate-300 rounded-lg p-2.5 text-sm outline-none" required></select>
                            <input type="text" id="field-custom-service" placeholder="Especifique el servicio" class="w-full mt-2 bg-white border border-slate-300 rounded-lg p-2.5 text-sm outline-none hidden">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-600 mb-1">Nº Cuenta / Contrato / Padrón</label>
                            <input type="text" id="field-account" placeholder="Ej: 123456789" class="w-full bg-white border border-slate-300 rounded-lg p-2.5 text-sm outline-none">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-600 mb-1">Notas</label>
                            <input type="text" id="field-servicio-notes" placeholder="Observaciones" class="w-full bg-white border border-slate-300 rounded-lg p-2.5 text-sm outline-none">
                        </div>
                    </div>
                    <div class="mt-4 flex gap-2 justify-end">
                        <button type="button" id="servicio-btn-cancel" onclick="app.resetServicioForm()" class="hidden px-4 py-2 text-sm font-medium text-slate-600 bg-slate-200 hover:bg-slate-300 rounded-lg">Cancelar</button>
                        <button type="submit" id="servicio-btn-submit" class="px-5 py-2 text-sm font-semibold text-white bg-blue-700 hover:bg-blue-800 rounded-lg flex items-center gap-2">
                            <i class="fa-solid fa-save"></i> Guardar Servicio
                        </button>
                    </div>
                </form>

                <div class="flex justify-end gap-2 mb-3">
                    <button onclick="app.exportServiciosCompacto()" class="bg-slate-600 hover:bg-slate-500 text-white font-medium px-4 py-2 rounded-xl shadow-md flex items-center gap-2 text-sm">
                        <i class="fa-solid fa-file-word"></i> Exportar Servicios (Word compacto)
                    </button>
                    <button onclick="app.exportServicios()" class="bg-emerald-600 hover:bg-emerald-500 text-white font-medium px-4 py-2 rounded-xl shadow-md flex items-center gap-2 text-sm">
                        <i class="fa-solid fa-file-word"></i> Exportar Servicios (Word)
                    </button>
                </div>

                <div class="overflow-x-auto rounded-xl border border-slate-200 shadow-sm bg-white">
                    <table class="w-full text-left text-sm text-slate-600">
                        <thead class="bg-slate-100 text-xs uppercase text-slate-500 font-bold border-b border-slate-200">
                            <tr>
                                <th class="p-3.5">Servicio</th>
                                <th class="p-3.5">Cuenta / Contrato</th>
                                <th class="p-3.5">Notas</th>
                                <th class="p-3.5 text-center">Acciones</th>
                            </tr>
                        </thead>
                        <tbody id="servicios-table-body" class="divide-y divide-slate-100"></tbody>
                    </table>
                </div>
                <div id="servicios-empty-state" class="hidden text-center py-12">
                    <i class="fa-regular fa-folder-open text-4xl text-slate-300 mb-2"></i>
                    <p class="text-slate-500 font-medium">No hay servicios en esta categoría.</p>
                </div>
            </div>
        </div>
    </main>

    <div id="toast" class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 bg-slate-900 text-white px-4 py-3 rounded-xl shadow-2xl flex items-center gap-3 border border-slate-700">
        <i id="toast-icon" class="fa-solid fa-circle-check text-emerald-400"></i>
        <span id="toast-message" class="text-sm font-medium">Operación realizada con éxito</span>
    </div>
    </div> <!-- cierre de #app-content -->

    <script type="module" src="js/app.js"></script>
</body>
</html>

```

### Archivo: `js/app.js`
```javascript
import { dataStore } from './dataStore.js';
import { exportServiciosToWord } from './export-word.js';
import { setClaveSesion } from './session.js';

// Catálogo de nombres sugeridos por categoría (solo para poblar el <select>,
// no es la lista de servicios ya cargados por el usuario).
const catalogoServicios = {
    hogar: ["UTE (Energía Eléctrica)", "ANTEL (Fibra / Fijo)", "Ancel / Movistar / Claro", "OSE (Agua)", "Gas Natural / Garrafa", "Servicio Técnico / Mantenimiento"],
    inmueble: ["Alquiler de Apartamento", "Gastos Comunes", "Tributos Inmobiliarios (IMM/Intendencia)", "Impuesto de Enseñanza Primaria", "Fondo de Garantía"],
    vehiculos: ["Seguro de Auto", "Patente de Rodados (SUCIVE)", "Cochera / Garaje", "Mantenimiento / Taller", "Peajes / Telepeaje"],
    colegio: ["Cuota Mensual Colegio / Liceo", "Matrícula Anual", "Materiales / Libros / Uniforme", "Actividades Extraescolares", "Transporte Escolar / Comedor"],
    varios: ["Tarjeta de Crédito", "Gimnasio / Club", "Servicios Streaming", "Préstamo / Cuota", "Gastos Médicos / Mutualista"],
};

let servicios = [];
let currentCategory = 'hogar';

async function init() {
    setSyncStatus('cargando');
    try {
        servicios = await dataStore.getServicios();
        setSyncStatus('ok');
    } catch (err) {
        setSyncStatus('error', err.message);
        throw err; // lo maneja intentarIngresar() para decidir si la contraseña era incorrecta
    }
    populateServiceNameOptions();
    renderServiciosTable();
    updateStats();

    document.getElementById('servicio-form').addEventListener('submit', handleServicioSubmit);
}

function setSyncStatus(state, detail) {
    const el = document.getElementById('sync-status');
    if (state === 'cargando') {
        el.innerHTML = `<i class="fa-solid fa-circle-notch fa-spin"></i> Cargando datos...`;
    } else if (state === 'guardando') {
        el.innerHTML = `<i class="fa-solid fa-circle-notch fa-spin"></i> Guardando...`;
    } else if (state === 'ok') {
        el.innerHTML = `<i class="fa-solid fa-circle-check"></i> Datos al día`;
    } else if (state === 'error') {
        el.innerHTML = `<i class="fa-solid fa-triangle-exclamation text-amber-300"></i> Error: ${detail || 'no se pudo sincronizar'}`;
    }
}

// ===================== SERVICIOS =====================

function switchCategory(category) {
    currentCategory = category;
    document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('tab-active'));
    document.getElementById(`tab-${category}`).classList.add('tab-active');
    populateServiceNameOptions();
    resetServicioForm();
    renderServiciosTable();
}

function populateServiceNameOptions() {
    const select = document.getElementById('field-service');
    select.innerHTML = '';
    (catalogoServicios[currentCategory] || []).forEach(nombre => {
        const opt = document.createElement('option');
        opt.value = nombre;
        opt.textContent = nombre;
        select.appendChild(opt);
    });
    const customOpt = document.createElement('option');
    customOpt.value = 'OTRO_CUSTOM';
    customOpt.textContent = '+ Otro (especificar)...';
    select.appendChild(customOpt);
    checkCustomService(select.value);
}

function checkCustomService(val) {
    const customInput = document.getElementById('field-custom-service');
    const isCustom = val === 'OTRO_CUSTOM';
    customInput.classList.toggle('hidden', !isCustom);
    customInput.required = isCustom;
}

async function handleServicioSubmit(e) {
    e.preventDefault();
    const id = document.getElementById('servicio-id').value;
    let serviceVal = document.getElementById('field-service').value;
    if (serviceVal === 'OTRO_CUSTOM') {
        serviceVal = document.getElementById('field-custom-service').value.trim() || 'Servicio Personalizado';
    }

    const nuevo = {
        id: id || `srv-${Date.now()}`,
        category: currentCategory,
        service: serviceVal,
        account: document.getElementById('field-account').value.trim(),
        notes: document.getElementById('field-servicio-notes').value.trim(),
    };

    if (id) {
        const idx = servicios.findIndex(s => s.id === id);
        if (idx !== -1) servicios[idx] = nuevo;
    } else {
        servicios.push(nuevo);
    }

    await persistirServicios();
    resetServicioForm();
    renderServiciosTable();
    updateStats();
    showToast(id ? 'Servicio actualizado' : 'Servicio agregado');
}

function editServicio(id) {
    const s = servicios.find(x => x.id === id);
    if (!s) return;

    document.getElementById('servicio-id').value = s.id;
    const select = document.getElementById('field-service');
    const found = Array.from(select.options).some(o => o.value === s.service);
    if (found) {
        select.value = s.service;
        checkCustomService(s.service);
    } else {
        select.value = 'OTRO_CUSTOM';
        checkCustomService('OTRO_CUSTOM');
        document.getElementById('field-custom-service').value = s.service;
    }
    document.getElementById('field-account').value = s.account || '';
    document.getElementById('field-servicio-notes').value = s.notes || '';

    document.getElementById('servicio-form-title').innerHTML = `<i class="fa-solid fa-pen-to-square text-amber-600"></i> EDITAR SERVICIO`;
    document.getElementById('servicio-btn-submit').innerHTML = `<i class="fa-solid fa-sync"></i> Actualizar`;
    document.getElementById('servicio-btn-cancel').classList.remove('hidden');
    document.getElementById('servicio-editing-badge').classList.remove('hidden');
    window.scrollTo({ top: 180, behavior: 'smooth' });
}

async function deleteServicio(id) {
    if (!confirm('¿Eliminar este servicio?')) return;
    servicios = servicios.filter(s => s.id !== id);
    await persistirServicios();
    renderServiciosTable();
    updateStats();
    showToast('Servicio eliminado', 'trash');
}

function resetServicioForm() {
    document.getElementById('servicio-form').reset();
    document.getElementById('servicio-id').value = '';
    document.getElementById('field-custom-service').classList.add('hidden');
    document.getElementById('servicio-form-title').innerHTML = `<i class="fa-solid fa-plus-circle text-blue-600"></i> NUEVO SERVICIO`;
    document.getElementById('servicio-btn-submit').innerHTML = `<i class="fa-solid fa-save"></i> Guardar Servicio`;
    document.getElementById('servicio-btn-cancel').classList.add('hidden');
    document.getElementById('servicio-editing-badge').classList.add('hidden');
    populateServiceNameOptions();
}

function renderServiciosTable() {
    const tbody = document.getElementById('servicios-table-body');
    const emptyState = document.getElementById('servicios-empty-state');
    const filtrados = servicios.filter(s => s.category === currentCategory);

    tbody.innerHTML = '';
    emptyState.classList.toggle('hidden', filtrados.length > 0);

    filtrados.forEach(s => {
        const tr = document.createElement('tr');
        tr.className = 'hover:bg-slate-50';
        tr.innerHTML = `
            <td class="p-3.5 font-semibold text-slate-800">${escapeHtml(s.service)}</td>
            <td class="p-3.5 font-mono text-xs">${escapeHtml(s.account || '-')}</td>
            <td class="p-3.5 text-slate-500 text-xs max-w-xs truncate">${escapeHtml(s.notes || '-')}</td>
            <td class="p-3.5 text-center">
                <div class="flex items-center justify-center gap-2">
                    <button onclick="app.editServicio('${s.id}')" title="Editar" class="text-slate-400 hover:text-blue-600 p-1.5 rounded-lg hover:bg-blue-50"><i class="fa-solid fa-pen-to-square"></i></button>
                    <button onclick="app.deleteServicio('${s.id}')" title="Eliminar" class="text-slate-400 hover:text-red-600 p-1.5 rounded-lg hover:bg-red-50"><i class="fa-solid fa-trash-can"></i></button>
                </div>
            </td>
        `;
        tbody.appendChild(tr);
    });
}

async function persistirServicios() {
    setSyncStatus('guardando');
    try {
        await dataStore.saveServicios(servicios);
        setSyncStatus('ok');
    } catch (err) {
        setSyncStatus('error', err.message);
        throw err;
    }
}

// ===================== STATS / UTILS =====================

function updateStats() {
    document.getElementById('stat-servicios-count').textContent = servicios.length;
}

function escapeHtml(text) {
    const div = document.createElement('div');
    div.innerText = text;
    return div.innerHTML;
}

function showToast(msg, icon = 'check') {
    const toast = document.getElementById('toast');
    document.getElementById('toast-message').textContent = msg;
    document.getElementById('toast-icon').className = icon === 'trash' ? 'fa-solid fa-trash-can text-red-400' : 'fa-solid fa-circle-check text-emerald-400';
    toast.classList.remove('translate-y-20', 'opacity-0');
    setTimeout(() => toast.classList.add('translate-y-20', 'opacity-0'), 3000);
}

// Exponer las funciones que usan los onclick del HTML
window.app = {
    switchCategory, checkCustomService, editServicio, deleteServicio, resetServicioForm,
    exportServicios: () => exportServiciosToWord(servicios),
    exportServiciosCompacto: () => exportServiciosToWord(servicios, true),
};

// ===================== PORTÓN DE CONTRASEÑA =====================

document.getElementById('password-form').addEventListener('submit', async (e) => {
    e.preventDefault();
    const passwordInput = document.getElementById('password-input');
    const errorEl = document.getElementById('password-error');
    const submitBtn = document.getElementById('password-submit');

    errorEl.classList.add('hidden');
    submitBtn.disabled = true;
    submitBtn.innerHTML = `<i class="fa-solid fa-circle-notch fa-spin"></i> Verificando...`;

    setClaveSesion(passwordInput.value);

    try {
        await init();
        document.getElementById('password-gate').classList.add('hidden');
        document.getElementById('app-content').classList.remove('hidden');
    } catch (err) {
        setClaveSesion(null); // no dejamos una contraseña incorrecta cargada en memoria
        errorEl.textContent = err.message === 'Contraseña incorrecta.'
            ? 'Contraseña incorrecta.'
            : `No se pudo acceder: ${err.message}`;
        errorEl.classList.remove('hidden');
        passwordInput.value = '';
        passwordInput.focus();
    } finally {
        submitBtn.disabled = false;
        submitBtn.innerHTML = `<i class="fa-solid fa-unlock"></i> Entrar`;
    }
});

```

### Archivo: `js/backends/backendGithub.js`
```javascript
// Backend que habla con nuestra propia Netlify Function, nunca directo con GitHub.
// Trabaja con texto plano: quien decide si ese texto es JSON normal o un
// paquete cifrado es dataStore.js, no este archivo.

const BASE = '/.netlify/functions/data';

async function get(resource) {
    const res = await fetch(`${BASE}?resource=${resource}`);
    if (!res.ok) {
        throw new Error(`No se pudo leer "${resource}" (HTTP ${res.status})`);
    }
    return res.text();
}

async function save(resource, texto) {
    const res = await fetch(`${BASE}?resource=${resource}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'text/plain' },
        body: texto,
    });
    if (!res.ok) {
        const err = await res.json().catch(() => ({}));
        throw new Error(err.error || `No se pudo guardar "${resource}" (HTTP ${res.status})`);
    }
    return texto;
}

export const githubBackend = { get, save };

```

### Archivo: `js/backends/backendLocalStorage.js`
```javascript
// Backend offline/de desarrollo. Mismo contrato get/save que backendGithub,
// así se puede intercambiar en dataStore.js sin tocar el resto de la app.

const KEYS = {
    servicios: 'gsh_servicios',
};

async function get(resource) {
    const raw = localStorage.getItem(KEYS[resource]);
    return raw ? JSON.parse(raw) : [];
}

async function save(resource, data) {
    localStorage.setItem(KEYS[resource], JSON.stringify(data, null, 2));
    return data;
}

export const localBackend = { get, save };

```

### Archivo: `js/crypto.js`
```javascript
// Cifrado simétrico AES-GCM con clave derivada por PBKDF2, tal como en
// cifrar.js / decifrar.js. Acá quedan como funciones de módulo reutilizables.

export async function encriptarJSON(datosObjeto, claveSecreta) {
    const encoder = new TextEncoder();
    const datosTexto = JSON.stringify(datosObjeto);

    const keyMaterial = await crypto.subtle.importKey(
        "raw", encoder.encode(claveSecreta), "PBKDF2", false, ["deriveKey"]
    );

    const salt = crypto.getRandomValues(new Uint8Array(16));
    const key = await crypto.subtle.deriveKey(
        { name: "PBKDF2", salt, iterations: 100000, hash: "SHA-256" },
        keyMaterial,
        { name: "AES-GCM", length: 256 },
        false,
        ["encrypt"]
    );

    const iv = crypto.getRandomValues(new Uint8Array(12));
    const encryptedContent = await crypto.subtle.encrypt(
        { name: "AES-GCM", iv },
        key,
        encoder.encode(datosTexto)
    );

    // Empaquetamos salt, iv y contenido cifrado en un único JSON de texto,
    // que es lo que termina guardado como servicios.json en el repo bd.
    return JSON.stringify({
        salt: Array.from(salt),
        iv: Array.from(iv),
        data: btoa(String.fromCharCode(...new Uint8Array(encryptedContent)))
    });
}

export async function descifrarJSON(paqueteCifradoTexto, claveSecreta) {
    let paquete;
    try {
        paquete = JSON.parse(paqueteCifradoTexto);
    } catch {
        throw new Error('El archivo de datos no tiene un formato válido.');
    }

    if (!paquete || !paquete.salt || !paquete.iv || !paquete.data) {
        throw new Error('El archivo no contiene datos cifrados reconocibles.');
    }

    const encoder = new TextEncoder();
    const salt = new Uint8Array(paquete.salt);
    const iv = new Uint8Array(paquete.iv);
    const encryptedData = Uint8Array.from(atob(paquete.data), c => c.charCodeAt(0));

    const keyMaterial = await crypto.subtle.importKey(
        "raw", encoder.encode(claveSecreta), "PBKDF2", false, ["deriveKey"]
    );

    const key = await crypto.subtle.deriveKey(
        { name: "PBKDF2", salt, iterations: 100000, hash: "SHA-256" },
        keyMaterial,
        { name: "AES-GCM", length: 256 },
        false,
        ["decrypt"]
    );

    try {
        const decryptedContent = await crypto.subtle.decrypt(
            { name: "AES-GCM", iv },
            key,
            encryptedData
        );
        const decoder = new TextDecoder();
        return JSON.parse(decoder.decode(decryptedContent));
    } catch {
        // AES-GCM falla la verificación de integridad si la clave es incorrecta:
        // es la forma normal de detectar "contraseña equivocada".
        throw new Error('Contraseña incorrecta.');
    }
}

```

### Archivo: `js/dataStore.js`
```javascript
// Capa de acceso a datos. app.js SOLO llama a las funciones de acá abajo.
// Acá es donde se cifra/descifra — el backend nunca ve los datos en claro,
// y app.js nunca sabe que existe cifrado.

import { githubBackend } from './backends/backendGithub.js';
// import { localBackend } from './backends/backendLocalStorage.js'; // alternativa offline/dev

import { encriptarJSON, descifrarJSON } from './crypto.js';
import { getClaveSesion } from './session.js';

const backend = githubBackend; // <- único punto donde se elige el backend activo

async function leerYDescifrar(resource) {
    const clave = getClaveSesion();
    if (!clave) throw new Error('No hay una sesión iniciada (falta la contraseña).');

    const texto = await backend.get(resource);
    if (!texto || !texto.trim() || texto.trim() === '[]') {
        return []; // archivo nuevo/vacío: todavía no hay nada cifrado que leer
    }
    return descifrarJSON(texto, clave);
}

async function cifrarYGuardar(resource, datos) {
    const clave = getClaveSesion();
    if (!clave) throw new Error('No hay una sesión iniciada (falta la contraseña).');

    const paqueteCifrado = await encriptarJSON(datos, clave);
    await backend.save(resource, paqueteCifrado);
    return datos;
}

export const dataStore = {
    getServicios: () => leerYDescifrar('servicios'),
    saveServicios: (servicios) => cifrarYGuardar('servicios', servicios),
};

```

### Archivo: `js/export-word.js`
```javascript
const categoryLabels = {
    hogar: 'Servicios del Hogar (UTE, ANTEL, OSE, etc.)',
    inmueble: 'Gastos de Inmueble',
    vehiculos: 'Vehículos',
    colegio: 'Colegio / Educación',
    varios: 'Otros Gastos Varios',
};

function wordStyles(compact) {
    if (!compact) {
        return `
            body { font-family: 'Calibri', 'Arial', sans-serif; color: #1e293b; line-height: 1.5; padding: 20px; }
            .header-title { color: #0038a8; font-size: 24pt; font-weight: bold; text-align: center; margin-bottom: 5px; }
            .subtitle { text-align: center; color: #64748b; font-size: 11pt; margin-bottom: 25px; border-bottom: 2px solid #0038a8; padding-bottom: 10px; }
            .section-title { color: #0f172a; font-size: 14pt; font-weight: bold; margin-top: 25px; margin-bottom: 10px; background-color: #f1f5f9; padding: 6px 10px; border-left: 4px solid #0038a8; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 20px; font-size: 10pt; }
            th { background-color: #0038a8; color: #ffffff; font-weight: bold; text-align: left; padding: 8px; border: 1px solid #0038a8; }
            td { padding: 8px; border: 1px solid #cbd5e1; }
            tr:nth-child(even) { background-color: #f8fafc; }
            .summary-box { margin-top: 30px; background-color: #f8fafc; border: 1px solid #cbd5e1; padding: 15px; border-radius: 5px; }
            .footer { margin-top: 40px; text-align: center; font-size: 9pt; color: #94a3b8; }
        `;
    }
    // Variante compacta: fuentes más chicas, menos padding, sin cajas ni márgenes grandes.
    return `
        body { font-family: 'Calibri', 'Arial', sans-serif; color: #1e293b; line-height: 1.15; padding: 10px; font-size: 8pt; }
        .header-title { color: #0038a8; font-size: 14pt; font-weight: bold; text-align: center; margin-bottom: 2px; }
        .subtitle { text-align: center; color: #64748b; font-size: 8pt; margin-bottom: 8px; border-bottom: 1px solid #0038a8; padding-bottom: 3px; }
        .section-title { color: #0f172a; font-size: 9pt; font-weight: bold; margin-top: 10px; margin-bottom: 3px; border-bottom: 1px solid #0038a8; padding: 2px 0; }
        table { width: 100%; border-collapse: collapse; margin-bottom: 8px; font-size: 7.5pt; }
        th { background-color: #0038a8; color: #ffffff; font-weight: bold; text-align: left; padding: 2px 4px; border: 1px solid #0038a8; }
        td { padding: 2px 4px; border: 1px solid #cbd5e1; }
        tr:nth-child(even) { background-color: #f8fafc; }
        .summary-box { margin-top: 8px; padding: 4px 0; border-top: 1px solid #cbd5e1; }
        .summary-box p { margin: 1px 0; font-size: 7.5pt; }
        .footer { margin-top: 15px; text-align: center; font-size: 6.5pt; color: #94a3b8; }
    `;
}

function escapeHtml(text) {
    const div = document.createElement('div');
    div.innerText = text ?? '';
    return div.innerHTML;
}

function fechaHoy() {
    return new Date().toLocaleDateString('es-UY', { year: 'numeric', month: 'long', day: 'numeric' });
}

function descargarComoWord(htmlContent, nombreArchivo) {
    const blob = new Blob(['\ufeff' + htmlContent], { type: 'application/msword' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = nombreArchivo;
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
}

function envoltorioHtml(tituloDoc, subtitulo, cuerpo, compact) {
    return `
    <html xmlns:o='urn:schemas-microsoft-com:office:office' xmlns:w='urn:schemas-microsoft-com:office:word' xmlns='http://www.w3.org/TR/REC-html40'>
    <head><meta charset='utf-8'><title>${tituloDoc}</title><style>${wordStyles(compact)}</style></head>
    <body>
        <div class="header-title">${tituloDoc}</div>
        <div class="subtitle">${subtitulo} | Generado el ${fechaHoy()}</div>
        ${cuerpo}
        <div class="footer">Documento generado mediante la aplicación de Registro de Servicios del Hogar (Uruguay).</div>
    </body>
    </html>`;
}

function construirCuerpoServicios(servicios, compact) {
    let cuerpo = '';
    const categorias = ['hogar', 'inmueble', 'vehiculos', 'colegio', 'varios'];

    categorias.forEach(cat => {
        const items = servicios.filter(s => s.category === cat);
        if (items.length === 0) return;

        cuerpo += `<div class="section-title">${categoryLabels[cat]}</div>`;
        cuerpo += `
            <table>
                <thead>
                    <tr><th>Servicio</th><th>Cuenta / Contrato</th><th>Notas</th></tr>
                </thead>
                <tbody>
                    ${items.map(s => `
                        <tr>
                            <td><b>${escapeHtml(s.service)}</b></td>
                            <td>${escapeHtml(s.account || '-')}</td>
                            <td>${escapeHtml(s.notes || '-')}</td>
                        </tr>
                    `).join('')}
                </tbody>
            </table>
        `;
    });

    cuerpo += compact
        ? `<div class="summary-box"><p>Total de servicios registrados: <b>${servicios.length}</b></p></div>`
        : `<div class="summary-box"><p style="font-size:10pt; color:#64748b;">Total de servicios registrados: <b>${servicios.length}</b></p></div>`;

    return cuerpo;
}

export function exportServiciosToWord(servicios, compact = false) {
    if (!servicios || servicios.length === 0) {
        alert('No hay servicios registrados para exportar.');
        return;
    }
    const cuerpo = construirCuerpoServicios(servicios, compact);
    const html = envoltorioHtml('REGISTRO DE SERVICIOS DEL HOGAR', 'Catálogo de servicios registrados', cuerpo, compact);
    const sufijo = compact ? '_compacto' : '';
    descargarComoWord(html, `Servicios_Hogar${sufijo}_${new Date().toISOString().slice(0, 10)}.doc`);
}

```

### Archivo: `js/session.js`
```javascript
// La contraseña vive solo acá, en memoria, mientras la pestaña esté abierta.
// Nunca se persiste en localStorage/sessionStorage ni se envía a ningún lado:
// solo se usa en el navegador para cifrar/descifrar antes de hablar con el backend.

let claveSesion = null;

export function setClaveSesion(nuevaClave) {
    claveSesion = nuevaClave;
}

export function getClaveSesion() {
    return claveSesion;
}

export function hayClaveSesion() {
    return !!claveSesion;
}

```

### Archivo: `mio.txt`
```txt
300415RFc
```

### Archivo: `netlify.toml`
```toml
[build]
  functions = "netlify/functions"
  publish = "."

[functions]
  node_bundler = "esbuild"

```

### Archivo: `netlify/functions/data.js`
```javascript
// Proxy entre el front-end y GitHub. El token nunca sale de acá.
// Variables de entorno esperadas en Netlify:
//   GITHUB_TOKEN      -> fine-grained token con Contents: Read and write, scoped al repo bd
//   GITHUB_OWNER      -> FABIOR1981
//   GITHUB_REPO       -> bd
//   GITHUB_BASE_PATH  -> gestor_Servicios

const GITHUB_API = 'https://api.github.com';
const RECURSOS_VALIDOS = ['servicios'];

exports.handler = async (event) => {
    const { GITHUB_TOKEN, GITHUB_OWNER, GITHUB_REPO, GITHUB_BASE_PATH } = process.env;
    const resource = event.queryStringParameters && event.queryStringParameters.resource;

    if (!RECURSOS_VALIDOS.includes(resource)) {
        return jsonResponse(400, { error: `Parámetro "resource" inválido. Debe ser uno de: ${RECURSOS_VALIDOS.join(', ')}` });
    }

    if (!GITHUB_TOKEN || !GITHUB_OWNER || !GITHUB_REPO || !GITHUB_BASE_PATH) {
        return jsonResponse(500, { error: 'Faltan variables de entorno de GitHub en Netlify.' });
    }

    const filePath = `${GITHUB_BASE_PATH}/${resource}.json`;
    const fileUrl = `${GITHUB_API}/repos/${GITHUB_OWNER}/${GITHUB_REPO}/contents/${filePath}`;
    const headers = {
        Authorization: `Bearer ${GITHUB_TOKEN}`,
        Accept: 'application/vnd.github+json',
    };

    try {
        if (event.httpMethod === 'GET') {
            return await leerArchivo(fileUrl, headers);
        }

        if (event.httpMethod === 'PUT') {
            return await guardarArchivo(fileUrl, headers, event.body, resource);
        }

        return jsonResponse(405, { error: 'Método no soportado. Usá GET o PUT.' });
    } catch (err) {
        return jsonResponse(500, { error: err.message || 'Error inesperado en la function.' });
    }
};

async function leerArchivo(fileUrl, headers) {
    const res = await fetch(fileUrl, { headers });

    if (res.status === 404) {
        // El archivo todavía no existe en el repo: devolvemos lista vacía en vez de error.
        return jsonResponse(200, []);
    }
    if (!res.ok) {
        return jsonResponse(res.status, { error: 'No se pudo leer el archivo desde GitHub.' });
    }

    const data = await res.json();
    const content = Buffer.from(data.content, 'base64').toString('utf-8');
    return { statusCode: 200, headers: { 'Content-Type': 'application/json' }, body: content };
}

async function guardarArchivo(fileUrl, headers, body, resource) {
    // Hay que traer el sha actual del archivo antes de poder pisarlo.
    const actual = await fetch(fileUrl, { headers });
    const shaActual = actual.ok ? (await actual.json()).sha : undefined;
    // Si actual.status es 404, shaActual queda undefined y GitHub crea el archivo nuevo.

    const contenidoBase64 = Buffer.from(body, 'utf-8').toString('base64');

    const putRes = await fetch(fileUrl, {
        method: 'PUT',
        headers: { ...headers, 'Content-Type': 'application/json' },
        body: JSON.stringify({
            message: `Actualiza ${resource}.json desde gestor-servicios-hogar`,
            content: contenidoBase64,
            sha: shaActual,
        }),
    });

    if (!putRes.ok) {
        const err = await putRes.json().catch(() => ({}));
        return jsonResponse(putRes.status, { error: err.message || 'No se pudo guardar el archivo en GitHub.' });
    }

    return { statusCode: 200, headers: { 'Content-Type': 'application/json' }, body };
}

function jsonResponse(statusCode, obj) {
    return { statusCode, headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(obj) };
}

```

