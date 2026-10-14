# Bundle del Proyecto para IA: llamadosControl

## Estructura de Archivos
- README.md
- css/llamados.css
- data/llamados.json
- documentacion/LEEME.md
- index.html
- js/config.js
- js/llamados.js
- netlify/functions/update-llamados.js
- package.json

---

## Código Fuente

### Archivo: `README.md`
```md
# Gestión de Llamados Laborales

Aplicación web para hacer el seguimiento de llamados laborales (procesos de selección). Registra cada etapa del proceso y calcula indicadores como los días activos y el porcentaje de conversión final.

Sitio publicado: https://llamadoscontrol.netlify.app

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/llamadosControl/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/llamadosControl/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## Funcionalidades

- **Tabla de llamados** con ID, publicado, empresa, puesto, fechas de inicio y fin, finalistas, estado, días activos y % de conversión final.
- **Estados**: Iniciado, Abierto, En Curso, Pausado y Cerrado.
- **Detalle por etapa**: Postulación, Selección, Entrevista y Psicotécnico, con fecha, cantidad de personas y observaciones.
- **Alta y edición** de llamados.
- **Cálculos automáticos**:
  - días que el llamado estuvo activo;
  - conversión final, es decir, la proporción de finalistas sobre postulantes.

## Cómo se usa

1. Abrí la app. Se muestran los llamados ingresados.
2. Agregá un llamado o tocá **Editar** en uno existente.
3. Completá los datos generales y las etapas a medida que avanza el proceso.
4. Guardá. Los cambios quedan disponibles para todos los que usan la app.

## Configuración

En `js/config.js` se definen:

- los estados posibles y el estado por defecto;
- qué columnas se ven en la tabla principal;
- qué etapas se ven en el detalle.

## Cómo funciona

- El frontend es HTML, CSS y JavaScript, sin build.
- Los datos están en `data/llamados.json`.
- Para guardar, la app llama a la Netlify Function `netlify/functions/update-llamados.js`, que actualiza el JSON en este repositorio (rama `main`) a través de la API de GitHub.

## Publicación en Netlify

Configurar esta variable de entorno:

| Variable | Para qué sirve |
|---|---|
| `GITHUB_TOKEN` | Token con permiso de escritura sobre este repositorio. |

El repositorio, la ruta del archivo y la rama están definidos al principio de `update-llamados.js`.

## Estructura

```
index.html                              Pantalla principal
css/llamados.css                        Estilos
js/config.js                            Estados y columnas configurables
js/llamados.js                          Lógica: tabla, detalle, cálculos y guardado
data/llamados.json                      Datos de los llamados
netlify/functions/update-llamados.js    Guarda el JSON en GitHub
```

```

### Archivo: `css/llamados.css`
```css
/* Anchos personalizados para inputs */
.w-80 { width: 80px; }
.w-120 { width: 120px; }
.w-130 { width: 130px; }
.w-140 { width: 140px; }
.w-70 { width: 70px; }
.w-110 { width: 110px; }

/* Centrado de texto */
.text-center { text-align: center; }

/* Altura para textarea */
.h-2em { height: 2em; }

/* Fondo personalizado para filas */
.bg-white { background: #fff; }
.bg-f1f5f9 { background: #f1f5f9; }

/* Detalle block y tabla */
.detalle-block {
  padding: 0.5em 1em;
  background: #f8fafc;
  border-radius: 0.5em;
}
.tabla-detalle {
  width: auto;
  margin-top: 0.5em;
  font-size: 1em;
  border-radius: 0.75em;
  overflow: hidden;
  border-collapse: separate;
  border-spacing: 0;
  box-shadow: 0 2px 8px rgba(30,64,175,0.06);
}
.tr-header {
  font-weight: 700;
  background: #dbeafe;
  color: #2563eb;
}
.td-etapa, .td-fecha, .td-cantidad, .td-obs {
  padding: 6px 18px;
  font-family: 'Segoe UI',Arial,sans-serif;
}
.detalle-postulacion td, .detalle-seleccion td, .detalle-entrevista td, .detalle-psicotecnico td {
  padding: 4px 12px;
}
/* Estilos personalizados para la app de llamados */

.input-tw {
  border: 1px solid #bfdbfe;
  background: #fff;
  border-radius: 0.5rem;
  padding: 0.5rem 0.75rem;
  width: 100%;
  box-shadow: 0 1px 2px rgba(30,64,175,0.04);
  outline: none;
  transition: border 0.2s, box-shadow 0.2s;
  font-size: 1rem;
}
.input-tw:focus {
  border-color: #2563eb;
  box-shadow: 0 0 0 2px #93c5fd;
}

button[type="submit"] {
  background: linear-gradient(90deg, #2563eb 0%, #60a5fa 100%);
  color: #fff;
  padding: 0.5rem 2rem;
  border: none;
  border-radius: 0.75rem;
  font-weight: 600;
  font-size: 1.1rem;
  box-shadow: 0 2px 8px rgba(30,64,175,0.08);
  cursor: pointer;
  transition: background 0.2s;
}
button[type="submit"]:hover {
  background: linear-gradient(90deg, #1d4ed8 0%, #3b82f6 100%);
}

body {
  background: #f1f5f9;
  font-family: 'Segoe UI', Arial, sans-serif;
}

.container {
  max-width: 1500px;
  margin: 0 auto;
  padding: 2rem 1rem;
}

.rounded-2xl {
  border-radius: 1.25rem;
}

.shadow-lg {
  box-shadow: 0 8px 32px rgba(30,64,175,0.10);
}

.mb-8 { margin-bottom: 2rem; }
.mb-10 { margin-bottom: 2.5rem; }
.mb-6 { margin-bottom: 1.5rem; }
.mb-4 { margin-bottom: 1rem; }
.mt-4 { margin-top: 1rem; }
.py-8 { padding-top: 2rem; padding-bottom: 2rem; }
.p-8 { padding: 2rem; }
.gap-6 { gap: 1.5rem; }
.flex { display: flex; }
.flex-col { flex-direction: column; }
.items-center { align-items: center; }
.justify-end { justify-content: flex-end; }
.font-bold { font-weight: bold; }
.font-semibold { font-weight: 600; }
.font-extrabold { font-weight: 800; }
.text-blue-800 { color: #1e40af; }
.text-blue-700 { color: #1d4ed8; }
.text-blue-600 { color: #2563eb; }
.text-gray-600 { color: #475569; }
.text-4xl { font-size: 2.25rem; }
.text-2xl { font-size: 1.5rem; }
.text-lg { font-size: 1.125rem; }
.rounded-lg { border-radius: 0.75rem; }
.rounded-xl { border-radius: 1rem; }
.border { border: 1px solid #e0e7ef; }
.border-blue-100 { border-color: #dbeafe; }
.border-blue-200 { border-color: #bfdbfe; }
.bg-white { background: #fff; }
.bg-blue-100 { background: #dbeafe; }
.bg-blue-50 { background: #eff6ff; }
.shadow { box-shadow: 0 2px 8px rgba(30,64,175,0.06); }
.overflow-x-auto { overflow-x: auto; }
.w-full { width: 100%; }
.min-w-full { min-width: 100%; }
.grid { display: grid; }
.grid-cols-1 { grid-template-columns: 1fr; }
.md\:grid-cols-3 { grid-template-columns: repeat(3, 1fr); }
.col-span-3 { grid-column: span 3 / span 3; }
.md\:col-span-3 { grid-column: span 3 / span 3; }
.tracking-tight { letter-spacing: -0.015em; }
.drop-shadow { filter: drop-shadow(0 2px 4px rgba(30,64,175,0.10)); }
.transition-all { transition: all 0.2s; }
.z-10 { z-index: 10; }
.sticky { position: sticky; }
.top-0 { top: 0; }
.h-6 { height: 1.5rem; }
.w-6 { width: 1.5rem; }
.gap-2 { gap: 0.5rem; }

/* Mejorar la tabla */
table {
  border-collapse: separate;
  border-spacing: 0;
}
thead th {
  font-weight: 700;
  color: #2563eb;
  background: #e0e7ff;
  border-bottom: 2px solid #c7d2fe;
}
tbody tr {
  transition: background 0.2s;
}
tbody tr:hover {
  background: #f0f9ff;
}
td, th {
  border-right: 1px solid #e0e7ef;
}
td:last-child, th:last-child {
  border-right: none;
}

#tablaLlamados th, #tablaLlamados td {
  padding-left: 10px;
  padding-right: 10px;
  padding-top: 6px;
  padding-bottom: 6px;
  font-size: 1em;
}
#tablaLlamados input.input-inline, #tablaLlamados select.input-inline {
  min-width: 60px;
  max-width: 120px;
  font-size: 1em;
  padding: 0.3em 0.5em;
}
.btn-guardar, .btn-cancelar, .btn-guardar-nuevo {
  color: #2563eb;
}

```

### Archivo: `data/llamados.json`
```json
[
  {
    "id_llamado": "L-01A",
    "empresa": "A.C.E.",
    "nombre_puesto": "Vendedor para salón",
    "fecha_inicio": "2026-01-15",
    "fecha_fin": "2026-01-24",
    "cant_finalistas": "1",
    "estado": "Pausado",
    "fecha_postulacion": "2026-01-19",
    "cant_postulantes": 150,
    "fecha_seleccion": "2026-01-22",
    "cant_seleccionados": 20,
    "fecha_entrevista": "2026-01-23",
    "cant_entrevistados": 10,
    "fecha_psicotecnico": "",
    "cant_psicotecnico": "",
    "obs_postulacion": "",
    "obs_seleccion": "",
    "obs_entrevista": "Precencial",
    "obs_psicotecnico": "",
    "publicado": "BuscoJobs"
  },
  {
    "id_llamado": "L-01AA",
    "empresa": "Prueba",
    "nombre_puesto": "Gerencia",
    "fecha_inicio": "2026-01-21",
    "fecha_fin": "2026-01-26",
    "cant_finalistas": "1",
    "estado": "Cerrado",
    "fecha_postulacion": "2026-01-21",
    "cant_postulantes": 300,
    "fecha_seleccion": "2026-01-23",
    "cant_seleccionados": 15,
    "fecha_entrevista": "2026-01-24",
    "cant_entrevistados": 10,
    "fecha_psicotecnico": "2026-01-26",
    "cant_psicotecnico": 3,
    "obs_postulacion": "",
    "obs_seleccion": "Algunos",
    "obs_entrevista": "Presencial,",
    "obs_psicotecnico": "Bender, MBTI ,Dibujo, Roshard,etc",
    "publicado": "Cartelera interna"
  }
]
```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/llamadosControl/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gestión de Llamados Laborales</title>
  <link rel="stylesheet" href="css/llamados.css">
</head>
<body class="bg-gray-100 min-h-screen">
  <div class="container mx-auto py-8 font-sans">
    <div class="flex flex-col items-center mb-8">
      <h1 class="text-4xl font-extrabold text-blue-800 mb-2 tracking-tight drop-shadow">Gestión de Llamados Laborales</h1>
      <p class="text-lg text-gray-600">Tablero de seguimiento de procesos de selección</p>
    </div>
    <div class="bg-white p-8 rounded-2xl shadow-lg border border-blue-100">
      <h2 class="text-2xl font-bold mb-6 text-blue-700">Llamados Ingresados</h2>
      <div class="overflow-x-auto rounded-xl border border-blue-100">
        <table id="tablaLlamados" class="min-w-full text-sm text-left bg-white rounded-xl">
          <thead class="sticky top-0 z-10 bg-blue-100" id="theadLlamados">
            <!-- Render dinámico por JS -->
          </thead>
          <tbody>
            <!-- Filas generadas por JS -->
          </tbody>
        </table>
      </div>
    </div>
    <!-- Templates para filas y detalles -->
    <template id="template-fila-nueva">
      <tr class="llamado-row" id="fila-nueva-dinamica">
        <!-- Render dinámico por JS -->
      </tr>
    </template>
    <template id="template-fila-llamado">
      <tr class="llamado-row" id="fila-llamado-dinamica">
        <!-- Render dinámico por JS -->
      </tr>
    </template>
    <template id="template-detalle-llamado">
      <tr class="detalle-row" style="display:none;">
        <td colspan="100">
          <div class="detalle-block">
            <table class="tabla-detalle" id="tablaDetalleDinamica">
              <!-- Render dinámico por JS -->
            </table>
          </div>
        </td>
      </tr>
    </template>
          </tbody>
        </table>
      </div>
    </div>
  </div>
  <script src="js/config.js"></script>
  <script src="js/llamados.js"></script>
</body>
</html>

```

### Archivo: `js/config.js`
```javascript
// Configuración global para llamadosControl
// Estados posibles para el combo de estado en la cabecera

window.ESTADOS_LLAMADO = [
  'Iniciado',
  'Abierto',
  'En Curso',
  'Pausado',
  'Cerrado'
];

// Estado por defecto al crear un llamado
window.ESTADO_LLAMADO_DEFAULT = 'Iniciado';

// Columnas visibles en la tabla principal (cabecera)
window.LLAMADOS_COLUMNAS_CABECERA = [
  { key: 'id_llamado', label: 'ID', visible: true },
  { key: 'publicado', label: 'Publicado', visible: true },
  { key: 'empresa', label: 'Empresa', visible: true },
  { key: 'nombre_puesto', label: 'Puesto', visible: true },
  { key: 'fecha_inicio', label: 'Inicio', visible: true },
  { key: 'fecha_fin', label: 'Fin', visible: true },
  { key: 'cant_finalistas', label: 'Finalistas', visible: true },
  { key: 'estado', label: 'Estado', visible: true },
  { key: 'dias_activos', label: 'Días Activos', visible: true },
  { key: 'conversion_final', label: '% Conversión Final', visible: true },
  { key: 'acciones', label: 'Editar', visible: true }
];

// Columnas visibles en el detalle
window.LLAMADOS_COLUMNAS_DETALLE = [
  { etapa: 'Postulación', fecha: 'fecha_postulacion', cantidad: 'cant_postulantes', obs: 'obs_postulacion', visible: true },
  { etapa: 'Selección', fecha: 'fecha_seleccion', cantidad: 'cant_seleccionados', obs: 'obs_seleccion', visible: true },
  { etapa: 'Entrevista', fecha: 'fecha_entrevista', cantidad: 'cant_entrevistados', obs: 'obs_entrevista', visible: true },
  { etapa: 'Psicotécnico', fecha: 'fecha_psicotecnico', cantidad: 'cant_psicotecnico', obs: 'obs_psicotecnico', visible: true }
];

```

### Archivo: `js/llamados.js`
```javascript

// Lógica de gestión de llamados laborales
const FORM_ID = 'llamadoForm';
const TABLE_ID = 'tablaLlamados';
// Importar estados desde config.js
let ESTADOS_LLAMADO = ['Abierto','En Curso','Pausado','Cerrado'];
try {
  // Si se usa import/export, usar: import { ESTADOS_LLAMADO } from './config.js';
  // Pero para compatibilidad, cargar dinámicamente si existe window.ESTADOS_LLAMADO
  if (window.ESTADOS_LLAMADO) {
    ESTADOS_LLAMADO = window.ESTADOS_LLAMADO;
  }
} catch(e) {}

let llamados = [];

async function obtenerLlamados() {
  try {
    const resp = await fetch('data/llamados.json');
    llamados = await resp.json();
  } catch {
    llamados = [];
  }
  renderTabla();
}

async function guardarLlamados() {
  await fetch('/.netlify/functions/update-llamados', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ data: llamados })
  });
}

function calcularDiasActivos(inicio, fin) {
  if (!inicio || !fin) return '';
  const d1 = new Date(inicio);
  const d2 = new Date(fin);
  const diff = (d2 - d1) / (1000 * 60 * 60 * 24);
  return diff >= 0 ? diff : '';
}

function calcularConversionFinal(cant_postulantes, cant_finalistas) {
  if (!cant_postulantes || cant_postulantes == 0) return '0%';
  return ((cant_finalistas / cant_postulantes) * 100).toFixed(1) + '%';
}


// Formatea fecha para mostrar en tabla, sin errores de zona horaria
function formatFecha(fecha) {
  if (!fecha) return '';
  // yyyy-mm-dd
  if (/^\d{4}-\d{2}-\d{2}$/.test(fecha)) {
    const [y, m, d] = fecha.split('-');
    return `${d}/${m}/${y}`;
  }
  // dd/mm/yyyy
  if (/^\d{2}\/\d{2}\/\d{4}$/.test(fecha)) {
    return fecha;
  }
  // Otro formato, intentar parsear
  const dt = new Date(fecha);
  if (!isNaN(dt)) {
    const day = String(dt.getDate()).padStart(2, '0');
    const month = String(dt.getMonth() + 1).padStart(2, '0');
    const year = dt.getFullYear();
    return `${day}/${month}/${year}`;
  }
  return fecha;
}


// Convierte fecha dd/mm/yyyy o yyyy-mm-dd a yyyy-mm-dd para input type date
function toInputDate(fecha) {
  if (!fecha) return '';
  // Si ya es formato yyyy-mm-dd
  if (/^\d{4}-\d{2}-\d{2}$/.test(fecha)) return fecha;
  // Si es formato dd/mm/yyyy
  if (/^\d{2}\/\d{2}\/\d{4}$/.test(fecha)) {
    const [d, m, y] = fecha.split('/');
    return `${y}-${m.padStart(2, '0')}-${d.padStart(2, '0')}`;
  }
  // Si es formato Date
  const d = new Date(fecha);
  if (!isNaN(d)) return d.toISOString().slice(0,10);
  return '';
}


let editIdLlamado = null;

window.editarLlamado = function(id_llamado) {
  editIdLlamado = id_llamado;
  renderTabla();
};

function renderTabla() {
  const tbody = document.querySelector(`#${TABLE_ID} tbody`);
  const thead = document.getElementById('theadLlamados');
  if (!tbody || !thead) return;
  tbody.innerHTML = '';
  thead.innerHTML = '';

  // Renderizar encabezado dinámico
  const trHead = document.createElement('tr');
  // Botón de expandir/collapse
  const thExpand = document.createElement('th');
  thExpand.className = 'px-3 py-2';
  trHead.appendChild(thExpand);
  (window.LLAMADOS_COLUMNAS_CABECERA || []).filter(c => c.visible).forEach(col => {
    const th = document.createElement('th');
    th.className = 'px-3 py-2';
    th.textContent = col.label;
    trHead.appendChild(th);
  });
  thead.appendChild(trHead);

  // Fila para nuevo llamado
  const nuevo = {};
  (window.LLAMADOS_COLUMNAS_CABECERA || []).forEach(col => { nuevo[col.key] = ''; });
  // Detalle también
  (window.LLAMADOS_COLUMNAS_DETALLE || []).forEach(etapa => {
    nuevo[etapa.fecha] = '';
    nuevo[etapa.cantidad] = '';
    nuevo[etapa.obs] = '';
  });
  const tplNuevo = document.getElementById('template-fila-nueva');
  const trNuevo = tplNuevo.content.cloneNode(true).children[0];
  trNuevo.innerHTML = '';
  // Botón vacío para expandir
  const tdExpandNuevo = document.createElement('td');
  trNuevo.appendChild(tdExpandNuevo);
  (window.LLAMADOS_COLUMNAS_CABECERA || []).filter(c => c.visible).forEach(col => {
    let td = document.createElement('td');
    if (col.key === 'acciones') {
      const btn = document.createElement('button');
      btn.className = 'btn-guardar-nuevo';
      btn.textContent = 'Nuevo';
      td.appendChild(btn);
    } else if (col.key === 'estado') {
      const select = document.createElement('select');
      select.setAttribute('data-field', col.key);
      select.className = 'input-tw input-inline w-110';
      (window.ESTADOS_LLAMADO || []).forEach(e => {
        const opt = document.createElement('option');
        opt.value = e;
        opt.textContent = e;
        select.appendChild(opt);
      });
      td.appendChild(select);
    } else if (col.key === 'fecha_inicio' || col.key === 'fecha_fin') {
      const input = document.createElement('input');
      input.type = 'date';
      input.setAttribute('data-field', col.key);
      input.className = 'input-tw input-inline w-130';
      td.appendChild(input);
    } else if (col.key === 'cant_finalistas') {
      const input = document.createElement('input');
      input.type = 'number';
      input.setAttribute('data-field', col.key);
      input.className = 'input-tw input-inline w-70';
      td.appendChild(input);
    } else {
      const input = document.createElement('input');
      input.type = 'text';
      input.setAttribute('data-field', col.key);
      input.className = 'input-tw input-inline w-120';
      td.appendChild(input);
    }
    trNuevo.appendChild(td);
  });
  tbody.appendChild(trNuevo);
  const btnGuardarNuevo = trNuevo.querySelector('.btn-guardar-nuevo');
  if (btnGuardarNuevo) {
    btnGuardarNuevo.onclick = async function() {
      const inputs = trNuevo.querySelectorAll('input, select');
      const nuevoLlamado = { ...nuevo };
      inputs.forEach(input => {
        const field = input.getAttribute('data-field');
        let val = input.value;
        if (input.type === 'number') val = val ? Number(val) : '';
        nuevoLlamado[field] = val;
      });
      if (!nuevoLlamado.id_llamado || !nuevoLlamado.nombre_puesto) {
        alert('Debe completar al menos ID y Nombre Puesto');
        return;
      }
      llamados.push(nuevoLlamado);
      await guardarLlamados();
      renderTabla();
    };
  }

  // Renderizar llamados ordenados usando template
  const llamadosOrdenados = [...llamados].sort((a, b) => {
    const fa = a.fecha_inicio ? new Date(a.fecha_inicio) : new Date(0);
    const fb = b.fecha_inicio ? new Date(b.fecha_inicio) : new Date(0);
    return fb - fa;
  });
  // Detectar si la columna acciones está visible
  const accionesVisible = (window.LLAMADOS_COLUMNAS_CABECERA || []).some(c => c.key === 'acciones' && c.visible);
  llamadosOrdenados.forEach((l) => {
    const tplFila = document.getElementById('template-fila-llamado');
    const tr = tplFila.content.cloneNode(true).children[0];
    tr.innerHTML = '';
    // Solo permitir edición si la columna acciones está visible
    let isEditing = accionesVisible && (editIdLlamado === l.id_llamado);
    // Botón expandir/collapse
    const tdExpand = document.createElement('td');
    const btnExpand = document.createElement('button');
    btnExpand.className = 'toggle-detalle';
    btnExpand.title = 'Ver detalle';
    btnExpand.textContent = '▶';
    tdExpand.appendChild(btnExpand);
    tr.appendChild(tdExpand);
    // Renderizar columnas dinámicas
    (window.LLAMADOS_COLUMNAS_CABECERA || []).filter(c => c.visible).forEach(col => {
      let td = document.createElement('td');
      if (col.key === 'acciones') {
        const btnEditar = document.createElement('button');
        btnEditar.className = 'btn-editar text-blue-600 underline hover:text-blue-900';
        btnEditar.textContent = 'Editar';
        const btnGuardar = document.createElement('button');
        btnGuardar.className = 'btn-guardar';
        btnGuardar.textContent = 'Guardar';
        btnGuardar.style.display = 'none';
        const btnCancelar = document.createElement('button');
        btnCancelar.className = 'btn-cancelar';
        btnCancelar.textContent = 'Cancelar';
        btnCancelar.style.display = 'none';
        td.appendChild(btnEditar);
        td.appendChild(btnGuardar);
        td.appendChild(btnCancelar);
      } else if (col.key === 'estado') {
        const select = document.createElement('select');
        select.className = 'input-tw input-inline input-estado w-110';
        select.setAttribute('data-field', col.key);
        (window.ESTADOS_LLAMADO || []).forEach(e => {
          const opt = document.createElement('option');
          opt.value = e;
          opt.textContent = e;
          select.appendChild(opt);
        });
        select.value = l.estado || (window.ESTADO_LLAMADO_DEFAULT || 'Abierto');
        td.appendChild(select);
      } else if (col.key === 'fecha_inicio' || col.key === 'fecha_fin') {
        const input = document.createElement('input');
        input.type = 'date';
        input.className = `input-tw input-inline input-${col.key} w-130`;
        input.setAttribute('data-field', col.key);
        input.value = toInputDate(l[col.key]);
        td.appendChild(input);
      } else if (col.key === 'cant_finalistas') {
        const input = document.createElement('input');
        input.type = 'number';
        input.className = 'input-tw input-inline input-finalistas w-70';
        input.setAttribute('data-field', col.key);
        input.value = l[col.key] || '';
        td.appendChild(input);
      } else if (col.key === 'dias_activos') {
        td.textContent = calcularDiasActivos(l.fecha_inicio, l.fecha_fin);
      } else if (col.key === 'conversion_final') {
        td.textContent = calcularConversionFinal(l.cant_postulantes, l.cant_finalistas);
      } else {
        td.textContent = l[col.key] || '';
      }
      tr.appendChild(td);
    });
    // Habilitar edición solo si existen los botones (columna acciones visible)
    const btnEditar = tr.querySelector('.btn-editar');
    const btnGuardar = tr.querySelector('.btn-guardar');
    const btnCancelar = tr.querySelector('.btn-cancelar');
    const inputs = tr.querySelectorAll('input, select');
    if (accionesVisible && (btnEditar || btnGuardar || btnCancelar)) {
      if (isEditing) {
        inputs.forEach(el => el.disabled = false);
        if (btnEditar) btnEditar.style.display = 'none';
        if (btnGuardar) btnGuardar.style.display = '';
        if (btnCancelar) btnCancelar.style.display = '';
      } else {
        inputs.forEach(el => el.disabled = true);
        if (btnEditar) btnEditar.style.display = '';
        if (btnGuardar) btnGuardar.style.display = 'none';
        if (btnCancelar) btnCancelar.style.display = 'none';
      }
      if (btnEditar) btnEditar.onclick = () => {
        editIdLlamado = l.id_llamado;
        renderTabla();
      };
      if (btnGuardar) btnGuardar.onclick = async () => {
        const idxOriginal = llamados.findIndex(x => x.id_llamado === l.id_llamado);
        if (idxOriginal !== -1) {
          // Guardar campos del cabezal
          (window.LLAMADOS_COLUMNAS_CABECERA || []).forEach(col => {
            if (col.key !== 'acciones' && col.key !== 'dias_activos' && col.key !== 'conversion_final') {
              const input = tr.querySelector(`[data-field='${col.key}']`);
              if (input) llamados[idxOriginal][col.key] = input.value;
            }
          });
          // Guardar campos del detalle
          const detalle = tr.nextElementSibling.querySelector('.detalle-block');
          if (detalle) {
            const detalleInputs = detalle.querySelectorAll('input, textarea');
            detalleInputs.forEach(input => {
              const field = input.getAttribute('data-field');
              let val = input.value;
              if (input.type === 'number') val = val ? Number(val) : '';
              llamados[idxOriginal][field] = val;
            });
          }
        }
        await guardarLlamados();
        editIdLlamado = null;
        renderTabla();
      };
      if (btnCancelar) btnCancelar.onclick = () => {
        editIdLlamado = null;
        renderTabla();
      };
    }
    tbody.appendChild(tr);

    // Fila detalle usando template
    const tplDetalle = document.getElementById('template-detalle-llamado');
    const trDetalle = tplDetalle.content.cloneNode(true).children[0];
    trDetalle.style.display = 'none'; // Siempre iniciar oculto
    const tablaDetalle = trDetalle.querySelector('.tabla-detalle');
    tablaDetalle.innerHTML = '';
    // Header detalle
    const trHeader = document.createElement('tr');
    ['Etapa', 'Fecha', 'Cantidad', 'Observaciones'].forEach(label => {
      const td = document.createElement('td');
      td.textContent = label;
      trHeader.appendChild(td);
    });
    tablaDetalle.appendChild(trHeader);
    // Renderizar etapas visibles
    (window.LLAMADOS_COLUMNAS_DETALLE || []).filter(e => e.visible).forEach(etapa => {
      const trEtapa = document.createElement('tr');
      trEtapa.className = `detalle-${etapa.etapa.toLowerCase()}`;
      // Etapa
      const tdEtapa = document.createElement('td');
      tdEtapa.textContent = etapa.etapa;
      trEtapa.appendChild(tdEtapa);
      // Fecha
      const tdFecha = document.createElement('td');
      const inputFecha = document.createElement('input');
      inputFecha.type = 'date';
      inputFecha.className = `input-tw input-inline w-130`;
      inputFecha.setAttribute('data-field', etapa.fecha);
      inputFecha.value = toInputDate(l[etapa.fecha]);
      trEtapa.appendChild(tdFecha);
      tdFecha.appendChild(inputFecha);
      // Cantidad
      const tdCant = document.createElement('td');
      const inputCant = document.createElement('input');
      inputCant.type = 'number';
      inputCant.className = 'input-tw input-inline w-70';
      inputCant.setAttribute('data-field', etapa.cantidad);
      inputCant.value = l[etapa.cantidad] || '';
      tdCant.appendChild(inputCant);
      trEtapa.appendChild(tdCant);
      // Observaciones
      const tdObs = document.createElement('td');
      const inputObs = document.createElement('textarea');
      inputObs.className = 'input-tw input-inline h-2em';
      inputObs.setAttribute('data-field', etapa.obs);
      inputObs.value = l[etapa.obs] || '';
      tdObs.appendChild(inputObs);
      trEtapa.appendChild(tdObs);
      // Habilitar edición si corresponde
      [inputFecha, inputCant, inputObs].forEach(el => { el.disabled = !isEditing; });
      tablaDetalle.appendChild(trEtapa);
    });
    tbody.appendChild(trDetalle);
    // Evento para expandir/colapsar solo su propio detalle
    btnExpand.onclick = function() {
      const expanded = trDetalle.style.display !== 'none' && trDetalle.style.display !== '';
      if (trDetalle.style.display === 'none' || trDetalle.style.display === '') {
        trDetalle.style.display = 'table-row';
        btnExpand.textContent = '▼';
      } else {
        trDetalle.style.display = 'none';
        btnExpand.textContent = '▶';
      }
    };
    // Sincronizar ícono al crear (por si acaso)
    btnExpand.textContent = (trDetalle.style.display === 'none' || trDetalle.style.display === '') ? '▶' : '▼';
  });
}
document.addEventListener('DOMContentLoaded', () => {
  obtenerLlamados();
  
});

```

### Archivo: `netlify/functions/update-llamados.js`
```javascript
// netlify/functions/update-llamados.js
// Función serverless para guardar llamados en llamados.json


const fetch = require('node-fetch');

const GITHUB_TOKEN = process.env.GITHUB_TOKEN;
const GITHUB_REPO = 'FABIOR1981/llamadosControl'; // Cambia por tu repo si es diferente
const FILE_PATH = 'data/llamados.json'; // Ruta relativa en el repo
const BRANCH = 'main'; // Cambia si usas otra rama

exports.handler = async function(event, context) {
  if (event.httpMethod !== 'POST') {
    return {
      statusCode: 405,
      body: 'Method Not Allowed',
    };
  }

  try {
    const body = JSON.parse(event.body);
    const nuevosDatos = JSON.stringify(body.data, null, 2);

    // 1. Obtener el SHA actual del archivo
    const getUrl = `https://api.github.com/repos/${GITHUB_REPO}/contents/${FILE_PATH}?ref=${BRANCH}`;
    const getResp = await fetch(getUrl, {
      headers: { Authorization: `token ${GITHUB_TOKEN}` }
    });
    const getData = await getResp.json();
    const sha = getData.sha;

    // 2. Actualizar el archivo
    const updateUrl = `https://api.github.com/repos/${GITHUB_REPO}/contents/${FILE_PATH}`;
    const updateResp = await fetch(updateUrl, {
      method: 'PUT',
      headers: {
        Authorization: `token ${GITHUB_TOKEN}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        message: 'Actualización de llamados.json desde Netlify',
        content: Buffer.from(nuevosDatos).toString('base64'),
        sha,
        branch: BRANCH
      })
    });

    if (!updateResp.ok) {
      const error = await updateResp.text();
      return { statusCode: updateResp.status, body: error };
    }

    return { statusCode: 200, body: JSON.stringify({ success: true }) };
  } catch (err) {
    return { statusCode: 500, body: JSON.stringify({ error: err.message }) };
  }
};

```

### Archivo: `package.json`
```json
{
  "name": "llamadoscontrol",
  "version": "1.0.0",
  "description": "Gestión de llamados laborales con funciones serverless en Netlify",
  "main": "index.js",
  "scripts": {},
  "author": "",
  "license": "MIT",
  "dependencies": {
    "node-fetch": "^2.6.12"
  }
}

```

