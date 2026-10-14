# Bundle del Proyecto para IA: Espacia_reportes

## Estructura de Archivos
- README.md
- css/styles.css
- index.html
- js/apiService.js
- js/app.js
- js/charts.js
- js/config.js
- js/utils.js
- netlify/functions/get-backups.js

---

## Código Fuente

### Archivo: `README.md`
```md
# Espacia_reportes

Espacia_reportes/
├── README.md
├── index.html
├── css/
│   └── styles.css
├── js/
│   ├── apiService.js  <-- Interfaz / Capa de Datos
│   ├── app.js         <-- Orquestador principal
│   ├── charts.js      <-- Lógica de Chart.js
│   ├── config.js      <-- Configuración de entorno
│   └── utils.js       <-- Funciones auxiliares y de negocio
└── netlify/
    └── functions/
        └── get-backups.js
```

### Archivo: `css/styles.css`
```css
/* styles.css - Estilos personalizados y reglas de impresión */

.print-only {
  display: none;
}

.img-bn {
  display: none;
}

@media print {
  body.print-bn .charts-grid canvas,
  body.print-bn .prof-card canvas {
    display: none !important;
  }
  body.print-bn .charts-grid .img-bn,
  body.print-bn .prof-card .img-bn {
    display: block !important;
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
  .print-only {
    display: block !important;
    font-size: 11px;
    margin-top: 4px;
  }
  @page {
    size: A4 portrait;
    margin: 6mm;
  }
  body {
    background-color: white !important;
    padding: 0 !important;
    font-size: 9px !important;
    color: #111 !important;
  }
  header button,
  header .flex.flex-wrap,
  #seccion-archivos,
  #modalDetalle,
  #searchInput {
    display: none !important;
  }
  html, body {
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
    min-height: 0 !important;
  }
  .space-y-6 > * + * {
    margin-top: 8px !important;
  }
  .max-w-7xl {
    max-width: 100% !important;
  }
  header {
    padding: 4px 10px !important;
    margin-bottom: 4px !important;
    display: block !important;
  }
  header > div:not(:first-child),
  header p:not(.print-only) {
    display: none !important;
  }
  header > div:first-child {
    display: flex !important;
    justify-content: space-between;
    align-items: baseline;
    gap: 12px;
  }
  header h1 {
    font-size: 15px !important;
  }
  header .print-only {
    font-size: 10px !important;
    margin: 0 !important;
    white-space: nowrap;
  }
  /* Tarjetas KPI en una sola línea horizontal de 6 columnas */
  .kpi-grid {
    display: grid !important;
    grid-template-columns: repeat(6, 1fr) !important;
    gap: 6px !important;
  }
  .kpi-grid > div {
    padding: 6px !important;
    grid-column: span 1 !important;
    min-width: 0 !important;
  }
  .kpi-grid .text-xl {
    font-size: 16px !important;
  }
  .kpi-grid .text-xs {
    font-size: 9px !important;
  }
  /* Donut y consultorios lado a lado */
  .charts-grid {
    display: grid !important;
    grid-template-columns: 1fr 1fr !important;
    gap: 8px !important;
  }
  .charts-grid > div,
  .prof-card {
    padding: 10px !important;
    break-inside: avoid;
  }
  .charts-grid h3,
  .prof-card h3 {
    font-size: 12px !important;
    margin-bottom: 4px !important;
  }
  .prof-card p {
    font-size: 9px !important;
    margin-bottom: 4px !important;
  }
  .charts-grid .relative {
    height: 260px !important;
    width: 100% !important;
  }
  .prof-card .relative {
    height: 380px !important;
    width: 100% !important;
  }
  .shadow {
    box-shadow: none !important;
    border: 1px solid #d1d5db !important;
  }
}
```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Reporte de Reservas - Espacia</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <!-- CSS Externo -->
  <link rel="stylesheet" href="css/styles.css">
</head>
<body class="bg-gray-50 text-gray-800 font-sans min-h-screen p-6">

  <div class="max-w-7xl mx-auto space-y-6">
    <!-- Encabezado -->
    <header class="bg-white shadow rounded-xl p-6 flex flex-col lg:flex-row justify-between items-center gap-4">
      <div>
        <h1 class="text-2xl font-bold text-indigo-900">📊 Reporte Analítico de Reservas</h1>
        <p class="text-sm text-gray-500">Auditoría detallada por estados, cancelaciones y profesionales</p>
        <p class="print-only"><strong id="print-periodo"></strong> &nbsp;|&nbsp; <span id="print-fecha"></span></p>
      </div>

      <!-- Filtros de Fecha y Controles -->
      <div class="flex flex-wrap items-center gap-3">
        <div class="flex items-center space-x-2 bg-gray-50 px-3 py-1.5 rounded-lg border text-sm">
          <label class="text-gray-600 font-medium text-xs">Desde:</label>
          <input type="date" id="filtroFechaInicio" class="bg-transparent focus:outline-none text-gray-700">
        </div>
        <div class="flex items-center space-x-2 bg-gray-50 px-3 py-1.5 rounded-lg border text-sm">
          <label class="text-gray-600 font-medium text-xs">Hasta:</label>
          <input type="date" id="filtroFechaFin" class="bg-transparent focus:outline-none text-gray-700">
        </div>
        <button onclick="aplicarFiltroFechas()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-semibold rounded-lg transition-colors">
          Consultar Período
        </button>
      </div>

      <div class="flex items-center space-x-3">
        <div id="status-badge" class="px-4 py-2 text-sm font-medium rounded-full bg-indigo-50 text-indigo-700">
          Cargando...
        </div>
        <!-- Botón de Imprimir -->
        <button onclick="imprimirReporte(false)" class="px-4 py-2 bg-gray-800 hover:bg-gray-900 text-white text-xs font-semibold rounded-lg transition-colors flex items-center gap-1.5">
          🖨️ Imprimir Color
        </button>
        <button onclick="imprimirReporte(true)" class="px-4 py-2 bg-white hover:bg-gray-100 text-gray-800 border border-gray-800 text-xs font-semibold rounded-lg transition-colors flex items-center gap-1.5">
          🖨️ Imprimir B/N
        </button>
      </div>
    </header>

    <!-- Tarjetas de Métricas (KPIs Desglosados) -->
    <div class="kpi-grid grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-6 gap-4">
      <div class="bg-white p-4 rounded-xl shadow border-l-4 border-indigo-500">
        <p class="text-xs text-gray-500 font-medium">Total Registradas</p>
        <p id="kpi-total" class="text-xl font-bold text-gray-800 mt-1">0</p>
      </div>
      <div class="bg-white p-4 rounded-xl shadow border-l-4 border-emerald-500">
        <p class="text-xs text-gray-500 font-medium">Efectuadas (Usuario)</p>
        <p id="kpi-efec-user" class="text-xl font-bold text-emerald-600 mt-1">0</p>
      </div>
      <div class="bg-white p-4 rounded-xl shadow border-l-4 border-emerald-800">
        <p class="text-xs text-gray-500 font-medium">Efectuadas (Admin)</p>
        <p id="kpi-efec-admin" class="text-xl font-bold text-emerald-800 mt-1">0</p>
      </div>
      <div class="bg-white p-4 rounded-xl shadow border-l-4 border-rose-400">
        <p class="text-xs text-gray-500 font-medium">Canceladas (Usuario)</p>
        <p id="kpi-canc-user" class="text-xl font-bold text-rose-500 mt-1">0</p>
      </div>
      <div class="bg-white p-4 rounded-xl shadow border-l-4 border-rose-700">
        <p class="text-xs text-gray-500 font-medium">Canceladas (Admin)</p>
        <p id="kpi-canc-admin" class="text-xl font-bold text-rose-700 mt-1">0</p>
      </div>
      <div class="bg-white p-4 rounded-xl shadow border-l-4 border-blue-400">
        <p class="text-xs text-gray-500 font-medium">Archivos Resguardo</p>
        <p id="kpi-files" class="text-xl font-bold text-gray-800 mt-1">0</p>
      </div>
    </div>

    <!-- Gráficos Principales -->
    <div class="charts-grid grid grid-cols-1 lg:grid-cols-2 gap-6">
      <div class="bg-white p-6 rounded-xl shadow">
        <h3 class="text-lg font-semibold text-gray-700 mb-4">Estado de Reservas</h3>
        <div class="relative h-72">
          <canvas id="chartEstado"></canvas>
          <img id="bn-estado" class="img-bn" alt="Estado de Reservas">
        </div>
      </div>
      <div class="bg-white p-6 rounded-xl shadow">
        <h3 class="text-lg font-semibold text-gray-700 mb-4">Distribución por Consultorio (Apilada)</h3>
        <div class="relative h-72">
          <canvas id="chartConsultorio"></canvas>
          <img id="bn-consultorio" class="img-bn" alt="Distribución por Consultorio">
        </div>
      </div>
    </div>

    <!-- Gráfico de Evolución Temporal (Tendencia de Autogestión) -->
    <div class="bg-white p-6 rounded-xl shadow">
      <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-4 gap-2">
        <div>
          <h3 class="text-lg font-semibold text-gray-700">Evolución Temporal de Reservas y Autogestión</h3>
          <p class="text-xs text-gray-500">Tendencia histórica agrupada por mes para evaluar la adopción de autogestión</p>
        </div>
      </div>
      <div class="relative h-80">
        <canvas id="chartEvolucion"></canvas>
        <img id="bn-evolucion" class="img-bn" alt="Evolución Temporal de Reservas">
      </div>
    </div>

    <!-- Contenedor Dinámico: Gráficos de Rendimiento por Profesional (Bloques de 10) -->
    <div id="profesionales-container" class="space-y-6">
      <!-- Inyectado dinámicamente desde js/charts.js -->
    </div>

    <!-- Listado de Archivos de Resguardo con Paginación -->
    <div id="seccion-archivos" class="bg-white rounded-xl shadow overflow-hidden">
      <div class="p-6 border-b border-gray-100 flex flex-col sm:flex-row justify-between items-center gap-4">
        <h3 class="text-lg font-semibold text-gray-700">Archivos de Resguardo en GitHub</h3>
        <input type="text" id="searchInput" placeholder="Filtrar archivos por fecha..." class="px-4 py-2 border rounded-lg text-sm w-full sm:w-72 focus:outline-none focus:ring-2 focus:ring-indigo-500">
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr class="bg-gray-50 text-xs uppercase text-gray-500 font-semibold border-b">
              <th class="p-4">Fecha de Reservas</th>
              <th class="p-4">Timestamp del Backup</th>
              <th class="p-4 text-center">Total</th>
              <th class="p-4 text-center">Efec. (Usu)</th>
              <th class="p-4 text-center">Efec. (Adm)</th>
              <th class="p-4 text-center">Canc. (Usu)</th>
              <th class="p-4 text-center">Canc. (Adm)</th>
              <th class="p-4 text-center">Acción</th>
            </tr>
          </thead>
          <tbody id="tabla-archivos" class="divide-y divide-gray-100 text-sm">
            <!-- Datos inyectados por JS -->
          </tbody>
        </table>
      </div>
      <!-- Controles de Paginación para la Tabla -->
      <div class="p-4 border-t bg-gray-50 flex flex-col sm:flex-row justify-between items-center gap-3">
        <span id="paginacion-info" class="text-xs text-gray-500">Mostrando 0 de 0 registros</span>
        <div class="flex items-center space-x-2">
          <button onclick="cambiarPaginaArchivos(-1)" id="btn-pagina-anterior" class="px-3 py-1.5 bg-white border hover:bg-gray-100 text-gray-700 text-xs font-semibold rounded-lg transition-colors disabled:opacity-50 disabled:cursor-not-allowed">
            ← Anterior
          </button>
          <span id="paginacion-actual" class="text-xs font-semibold text-gray-700 px-2">Página 1 de 1</span>
          <button onclick="cambiarPaginaArchivos(1)" id="btn-pagina-siguiente" class="px-3 py-1.5 bg-white border hover:bg-gray-100 text-gray-700 text-xs font-semibold rounded-lg transition-colors disabled:opacity-50 disabled:cursor-not-allowed">
            Siguiente →
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- Modal Amigable para Ver el Detalle del Archivo -->
  <div id="modalDetalle" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center p-4 z-50">
    <div class="bg-white rounded-2xl max-w-4xl w-full max-h-[90vh] flex flex-col shadow-2xl overflow-hidden">
      <div class="p-6 border-b flex justify-between items-center bg-indigo-900 text-white">
        <div>
          <h3 id="modalTitulo" class="text-lg font-bold">Detalle del Resguardo</h3>
          <p id="modalSubtitulo" class="text-xs text-indigo-200 mt-0.5">Información amigable del archivo JSON</p>
        </div>
        <button onclick="cerrarModal()" class="text-indigo-200 hover:text-white text-2xl font-bold">&times;</button>
      </div>
      <div class="p-6 overflow-y-auto space-y-4 flex-1 bg-gray-50" id="modalContenido"></div>
      <div class="p-4 border-t bg-white flex justify-end">
        <button onclick="cerrarModal()" class="px-5 py-2 bg-gray-200 hover:bg-gray-300 text-gray-700 text-sm font-semibold rounded-lg transition-colors">
          Cerrar
        </button>
      </div>
    </div>
  </div>

  <!-- Script Principal como Módulo ES6 -->
  <script type="module" src="js/app.js"></script>
</body>
</html>
```

### Archivo: `js/apiService.js`
```javascript
// js/apiService.js - Interfaz de Datos (Preparada para futura Base de Datos)

export const ApiService = {
  async obtenerResguardos(fechaInicio, fechaFin) {
    try {
      const params = new URLSearchParams();
      if (fechaInicio) params.set("fechaInicio", fechaInicio);
      if (fechaFin) params.set("fechaFin", fechaFin);

      const consulta = params.toString();
      const url = `/.netlify/functions/get-backups${consulta ? "?" + consulta : ""}`;

      const response = await fetch(url);
      if (!response.ok) throw new Error(`Error en Netlify Function: ${response.status} ${response.statusText}`);

      // Retorna los resguardos normalizados.
      // .parcial = true si el servidor cortó por tiempo y faltan resguardos antiguos.
      const datos = await response.json();
      datos.parcial = response.headers.get("X-Resultado-Parcial") === "true";
      return datos;
    } catch (error) {
      console.error("Error en ApiService:", error);
      throw error;
    }
  }
};

```

### Archivo: `js/app.js`
```javascript
// js/app.js - Lógica principal y flujo de la aplicación
import { ApiService } from './apiService.js';
import { extraerProfesional, clasificarReserva, escaparHtml } from './utils.js';
import { charts, TAMANOS_IMPRESION, renderizarGraficos, generarImagenesBN } from './charts.js';

let cacheBackups = [];
let paginaActualArchivos = 1;
const elementosPorPagina = 10;
let backupsFiltradosActuales = [];

window.imprimirReporte = function(bn) {
  if (!bn) {
    document.body.classList.remove('print-bn');
    window.print();
    return;
  }
  if (!charts.estado || !charts.consultorio) return;
  generarImagenesBN();
  const imgs = [...document.querySelectorAll('.img-bn')];
  Promise.all(imgs.map(img => img.decode())).then(() => {
    document.body.classList.add('print-bn');
    window.print();
  });
};

window.addEventListener("beforeprint", () => {
  Object.entries(charts).forEach(([clave, c]) => {
    if (TAMANOS_IMPRESION[clave]) c.resize(...TAMANOS_IMPRESION[clave]);
  });
  document.getElementById("print-fecha").textContent =
    `Generado: ${new Date().toLocaleString("es-UY")}`;
});

window.addEventListener("afterprint", () => {
  document.body.classList.remove('print-bn');
  Object.values(charts).forEach(c => { c.resize(); c.update('none'); });
});

document.addEventListener("DOMContentLoaded", () => {
  const hoy = new Date();
  const hace7Dias = new Date();
  hace7Dias.setDate(hoy.getDate() - 7);

  const formatDate = (date) => date.toISOString().split('T')[0];

  document.getElementById("filtroFechaInicio").value = formatDate(hace7Dias);
  document.getElementById("filtroFechaFin").value = formatDate(hoy);

  inicializarDatos();
});

window.aplicarFiltroFechas = function() {
  inicializarDatos();
};

async function inicializarDatos() {
  const statusBadge = document.getElementById("status-badge");
  const fechaInicio = document.getElementById("filtroFechaInicio").value;
  const fechaFin = document.getElementById("filtroFechaFin").value;
  cacheBackups = [];

  const fmt = (iso) => iso ? iso.split('-').reverse().join('/') : '—';
  document.getElementById("print-periodo").textContent =
    `Período: ${fmt(fechaInicio)} al ${fmt(fechaFin)}`;

  statusBadge.textContent = "Sincronizando con base de datos...";
  statusBadge.className = "px-4 py-2 bg-indigo-50 text-indigo-700 text-sm font-medium rounded-full";

  try {
    cacheBackups = await ApiService.obtenerResguardos(fechaInicio, fechaFin);
    
    document.getElementById("kpi-files").textContent = cacheBackups.length;

    if (cacheBackups.length === 0) {
      statusBadge.textContent = "No hay archivos en el período seleccionado.";
      statusBadge.className = "px-4 py-2 bg-amber-50 text-amber-700 text-sm font-medium rounded-full";
      processAndRenderData([]);
      return;
    }

    statusBadge.textContent = `Sincronizado (${cacheBackups.length} archivos)` + (cacheBackups.parcial ? " — parcial: acotá el período" : "");
    statusBadge.className = "px-4 py-2 bg-emerald-50 text-emerald-700 text-sm font-medium rounded-full";
    processAndRenderData(cacheBackups);

  } catch (error) {
    console.error(error);
    statusBadge.textContent = "Error de conexión con la fuente de datos";
    statusBadge.className = "px-4 py-2 bg-rose-50 text-rose-700 text-sm font-medium rounded-full";
  }
}

function processAndRenderData(backupsArray) {
  let todasLasReservas = [];

  backupsArray.forEach(fileData => {
    if (fileData.reservas && Array.isArray(fileData.reservas)) {
      fileData.reservas.forEach(r => {
        r._tipoClasificado = clasificarReserva(r);
        r._profesional = extraerProfesional(r);
        todasLasReservas.push(r);
      });
    }
  });

  const total = todasLasReservas.length;
  const efecUser = todasLasReservas.filter(r => r._tipoClasificado === "efectuada_user").length;
  const efecAdmin = todasLasReservas.filter(r => r._tipoClasificado === "efectuada_admin").length;
  const cancUser = todasLasReservas.filter(r => r._tipoClasificado === "cancelada_user").length;
  const cancAdmin = todasLasReservas.filter(r => r._tipoClasificado === "cancelada_admin").length;

  document.getElementById("kpi-total").textContent = total;
  document.getElementById("kpi-efec-user").textContent = efecUser;
  document.getElementById("kpi-efec-admin").textContent = efecAdmin;
  document.getElementById("kpi-canc-user").textContent = cancUser;
  document.getElementById("kpi-canc-admin").textContent = cancAdmin;

  renderizarGraficos(efecUser, efecAdmin, cancUser, cancAdmin, todasLasReservas, backupsArray);

  backupsFiltradosActuales = [...backupsArray].sort((a, b) => b.fechaReservas.localeCompare(a.fechaReservas));
  paginaActualArchivos = 1;
  renderArchivosTablePaginated();

  const searchInput = document.getElementById("searchInput");
  const newSearchInput = searchInput.cloneNode(true);
  searchInput.parentNode.replaceChild(newSearchInput, searchInput);

  newSearchInput.addEventListener("input", (e) => {
    const term = e.target.value.toLowerCase();
    backupsFiltradosActuales = backupsArray.filter(b => 
      b.fechaReservas.toLowerCase().includes(term) ||
      b.fechaDelBackup.toLowerCase().includes(term)
    ).sort((a, b) => b.fechaReservas.localeCompare(a.fechaReservas));
    
    paginaActualArchivos = 1;
    renderArchivosTablePaginated();
  });
}

function renderArchivosTablePaginated() {
  const tbody = document.getElementById("tabla-archivos");
  tbody.innerHTML = "";

  const totalRegistros = backupsFiltradosActuales.length;
  if (totalRegistros === 0) {
    tbody.innerHTML = `<tr><td colspan="8" class="p-4 text-center text-gray-400">No se encontraron archivos de respaldo</td></tr>`;
    document.getElementById("paginacion-info").textContent = "Mostrando 0 de 0 registros";
    document.getElementById("paginacion-actual").textContent = "Página 1 de 1";
    document.getElementById("btn-pagina-anterior").disabled = true;
    document.getElementById("btn-pagina-siguiente").disabled = true;
    return;
  }

  const totalPaginas = Math.ceil(totalRegistros / elementosPorPagina);
  if (paginaActualArchivos > totalPaginas) paginaActualArchivos = totalPaginas;
  if (paginaActualArchivos < 1) paginaActualArchivos = 1;

  const inicio = (paginaActualArchivos - 1) * elementosPorPagina;
  const fin = Math.min(inicio + elementosPorPagina, totalRegistros);
  const paginaItems = backupsFiltradosActuales.slice(inicio, fin);

  paginaItems.forEach((fileData) => {
    const reservas = fileData.reservas || [];
    const efecU = reservas.filter(r => clasificarReserva(r) === "efectuada_user").length;
    const efecA = reservas.filter(r => clasificarReserva(r) === "efectuada_admin").length;
    const cUser = reservas.filter(r => clasificarReserva(r) === "cancelada_user").length;
    const cAdmin = reservas.filter(r => clasificarReserva(r) === "cancelada_admin").length;
    const totalReservas = reservas.length;

    const tr = document.createElement("tr");
    tr.className = "hover:bg-gray-50 transition-colors";
    tr.innerHTML = `
      <td class="p-4 font-semibold text-indigo-900">${escaparHtml(fileData.fechaReservas)}</td>
      <td class="p-4 text-gray-500 text-xs">${new Date(fileData.fechaDelBackup).toLocaleString()}</td>
      <td class="p-4 text-center font-bold text-gray-700">${totalReservas}</td>
      <td class="p-4 text-center"><span class="px-2 py-0.5 rounded-full text-xs font-semibold bg-emerald-100 text-emerald-800">${efecU}</span></td>
      <td class="p-4 text-center"><span class="px-2 py-0.5 rounded-full text-xs font-semibold bg-emerald-900 text-white">${efecA}</span></td>
      <td class="p-4 text-center"><span class="px-2 py-0.5 rounded-full text-xs font-semibold bg-rose-100 text-rose-800">${cUser}</span></td>
      <td class="p-4 text-center"><span class="px-2 py-0.5 rounded-full text-xs font-semibold bg-red-100 text-red-900">${cAdmin}</span></td>
      <td class="p-4 text-center">
        <button class="btn-detalle px-3 py-1.5 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 text-xs font-semibold rounded-lg transition-colors">
          🔍 Ver Detalle
        </button>
      </td>
    `;
    tr.querySelector(".btn-detalle").addEventListener("click", () => window.verDetalleArchivo(fileData.fechaReservas));
    tbody.appendChild(tr);
  });

  document.getElementById("paginacion-info").textContent = `Mostrando registros ${inicio + 1} al ${fin} de ${totalRegistros}`;
  document.getElementById("paginacion-actual").textContent = `Página ${paginaActualArchivos} de ${totalPaginas}`;
  document.getElementById("btn-pagina-anterior").disabled = paginaActualArchivos === 1;
  document.getElementById("btn-pagina-siguiente").disabled = paginaActualArchivos === totalPaginas;
}

window.cambiarPaginaArchivos = function(direccion) {
  paginaActualArchivos += direccion;
  renderArchivosTablePaginated();
};

window.verDetalleArchivo = function(fechaReservas) {
  const fileData = cacheBackups.find(b => b.fechaReservas === fechaReservas);
  if (!fileData) return;

  document.getElementById("modalTitulo").textContent = `Resguardo del día: ${fileData.fechaReservas}`;
  document.getElementById("modalSubtitulo").textContent = `Backup generado el: ${new Date(fileData.fechaDelBackup).toLocaleString()} — Total registros: ${fileData.total}`;

  const contenedor = document.getElementById("modalContenido");
  contenedor.innerHTML = "";

  if (!fileData.reservas || fileData.reservas.length === 0) {
    contenedor.innerHTML = `<p class="text-center text-gray-400 py-6">No hay reservas registradas en este archivo.</p>`;
  } else {
    fileData.reservas.forEach(r => {
      const tipo = clasificarReserva(r);
      const prof = extraerProfesional(r);
      const emailLine = r.description.find(l => l.toLowerCase().includes("email:")) || "";
      const email = emailLine.split(":")[1] ? emailLine.split(":")[1].trim() : "No especificado";
      const consultorioLine = r.description.find(l => l.toLowerCase().includes("consultorio:")) || "";
      const consultorio = consultorioLine ? consultorioLine.split(":")[1].trim() : "-";

      let badgeClass = "bg-emerald-100 text-emerald-800";
      let badgeText = "EFECTUADA (USUARIO)";
      if (tipo === "efectuada_admin") {
        badgeClass = "bg-emerald-900 text-white";
        badgeText = "EFECTUADA (ADMIN)";
      } else if (tipo === "cancelada_user") {
        badgeClass = "bg-rose-100 text-rose-800";
        badgeText = "CANC. USUARIO";
      } else if (tipo === "cancelada_admin") {
        badgeClass = "bg-red-100 text-red-900";
        badgeText = "CANC. ADMIN";
      }

      const card = document.createElement("div");
      card.className = "bg-white p-4 rounded-xl border shadow-sm space-y-2";
      card.innerHTML = `
        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-2 border-b pb-2">
          <div>
            <span class="text-xs font-bold px-2 py-0.5 rounded bg-indigo-100 text-indigo-800">Consultorio ${escaparHtml(consultorio)}</span>
            <span class="font-bold text-gray-800 ml-2">${escaparHtml(r.summary)}</span>
          </div>
          <span class="px-2.5 py-0.5 rounded-full text-xs font-semibold ${badgeClass}">
            ${badgeText}
          </span>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-2 text-xs text-gray-600">
          <div>👤 <strong>Profesional:</strong> ${escaparHtml(prof)}</div>
          <div>📧 <strong>Email:</strong> ${escaparHtml(email)}</div>
          <div>🕒 <strong>Horario:</strong> ${r.inicio.split("T")[1].substring(0,5)} - ${r.fin.split("T")[1].substring(0,5)} hs</div>
        </div>
        <div class="text-[11px] text-gray-400 pt-1 border-t space-y-0.5">
          ${r.description.map(line => `<div>• ${escaparHtml(line)}</div>`).join('')}
        </div>
      `;
      contenedor.appendChild(card);
    });
  }

  document.getElementById("modalDetalle").classList.remove("hidden");
  document.getElementById("modalDetalle").classList.add("flex");
};

window.cerrarModal = function() {
  document.getElementById("modalDetalle").classList.remove("flex");
  document.getElementById("modalDetalle").classList.add("hidden");
};
```

### Archivo: `js/charts.js`
```javascript
// js/charts.js - Gestión de gráficos Chart.js y estilos B/N
import { crearTextura, clasificarReserva } from './utils.js';

export let charts = {};
const COLORES = ['#10B981', '#047857', '#F43F5E', '#BE123C'];

export const TAMANOS_IMPRESION = { 
  estado: [345, 260], 
  consultorio: [345, 260],
  evolucion: [725, 280] 
};

export function aplicarEstiloGraficos(bn) {
  const fuente = bn ? [crearTextura("puntos"), crearTextura("rayas"), crearTextura("cuadricula"), crearTextura("diagonal")] : COLORES;
  
  if (charts.estado) {
    const ds = charts.estado.data.datasets[0];
    ds.backgroundColor = fuente;
    ds.borderColor = bn ? '#000000' : '#ffffff';
    ds.borderWidth = bn ? 1.5 : 2;
  }
  if (charts.consultorio) {
    charts.consultorio.data.datasets.forEach((d, i) => {
      d.backgroundColor = fuente[i];
      d.borderColor = bn ? '#000000' : 'transparent';
      d.borderWidth = bn ? 1.2 : 0;
    });
  }
  if (charts.evolucion) {
    charts.evolucion.data.datasets.forEach((d, i) => {
      d.backgroundColor = fuente[i];
      d.borderColor = bn ? '#000000' : 'transparent';
      d.borderWidth = bn ? 1.2 : 0;
    });
  }
  Object.keys(charts).forEach(clave => {
    if (clave.startsWith('profesionales_')) {
      charts[clave].data.datasets.forEach((d, i) => {
        d.backgroundColor = fuente[i];
        d.borderColor = bn ? '#000000' : 'transparent';
        d.borderWidth = bn ? 1.2 : 0;
      });
    }
  });
}

export function generarImagenesBN() {
  aplicarEstiloGraficos(true);
  Object.entries(charts).forEach(([clave, c]) => {
    if (TAMANOS_IMPRESION[clave]) {
      c.resize(...TAMANOS_IMPRESION[clave]);
      c.update('none');
      const imgElem = document.getElementById(`bn-${clave}`);
      if (imgElem) imgElem.src = c.toBase64Image('image/png', 1);
    } else if (clave.startsWith('profesionales_')) {
      c.resize(725, 380);
      c.update('none');
      const imgElem = document.getElementById(`bn-${clave}`);
      if (imgElem) imgElem.src = c.toBase64Image('image/png', 1);
    }
  });
  aplicarEstiloGraficos(false);
  Object.values(charts).forEach(c => { c.resize(); c.update('none'); });
}

export function renderizarGraficos(efecUser, efecAdmin, cancUser, cancAdmin, reservas, backupsArray) {
  if (charts.estado) charts.estado.destroy();
  if (charts.consultorio) charts.consultorio.destroy();
  if (charts.evolucion) charts.evolucion.destroy();

  Object.keys(charts).forEach(k => {
    if (k.startsWith('profesionales_')) {
      charts[k].destroy();
      delete charts[k];
    }
  });

  // 1. Gráfico de Anillo (Estado)
  const ctxEstado = document.getElementById("chartEstado").getContext("2d");
  charts.estado = new Chart(ctxEstado, {
    type: 'doughnut',
    data: {
      labels: ['Efectuadas (Usuario)', 'Efectuadas (Admin)', 'Canceladas por Usuario', 'Canceladas por Admin'],
      datasets: [{
        data: [efecUser, efecAdmin, cancUser, cancAdmin],
        backgroundColor: COLORES,
        borderWidth: 2
      }]
    },
    options: { responsive: true, maintainAspectRatio: false, animation: false }
  });

  // 2. Gráfico de Consultorios (Apilado)
  const consultoriosMap = {};
  reservas.forEach(r => {
    const descConsultorio = r.description.find(line => line.includes("Consultorio:"));
    const cons = descConsultorio ? descConsultorio.split(":")[1].trim() : "Desconocido";
    const key = `Consultorio ${cons}`;

    if (!consultoriosMap[key]) {
      consultoriosMap[key] = { efectuada_user: 0, efectuada_admin: 0, cancelada_user: 0, cancelada_admin: 0 };
    }
    consultoriosMap[key][r._tipoClasificado]++;
  });

  const sortedConsKeys = Object.keys(consultoriosMap).sort((a, b) => {
    return a.localeCompare(b, undefined, { numeric: true, sensitivity: 'base' });
  });

  const ctxConsultorio = document.getElementById("chartConsultorio").getContext("2d");
  charts.consultorio = new Chart(ctxConsultorio, {
    type: 'bar',
    data: {
      labels: sortedConsKeys,
      datasets: [
        { label: 'Efectuadas (Usuario)', data: sortedConsKeys.map(k => consultoriosMap[k].efectuada_user), backgroundColor: '#10B981', borderRadius: 4 },
        { label: 'Efectuadas (Admin)', data: sortedConsKeys.map(k => consultoriosMap[k].efectuada_admin), backgroundColor: '#047857', borderRadius: 4 },
        { label: 'Canceladas por Usuario', data: sortedConsKeys.map(k => consultoriosMap[k].cancelada_user), backgroundColor: '#F43F5E', borderRadius: 4 },
        { label: 'Canceladas por Admin', data: sortedConsKeys.map(k => consultoriosMap[k].cancelada_admin), backgroundColor: '#BE123C', borderRadius: 4 }
      ]
    },
    options: { 
      responsive: true, 
      maintainAspectRatio: false,
      animation: false,
      scales: { x: { stacked: true }, y: { stacked: true, beginAtZero: true, ticks: { stepSize: 1 } } }
    }
  });

  // 3. Gráfico de Evolución Temporal (Mes a Mes)
  const evolucionMap = {};
  backupsArray.forEach(fileData => {
    if (!fileData.fechaReservas) return;
    const mesKey = fileData.fechaReservas.substring(0, 7); // YYYY-MM
    if (!evolucionMap[mesKey]) {
      evolucionMap[mesKey] = { efectuada_user: 0, efectuada_admin: 0, cancelada_user: 0, cancelada_admin: 0 };
    }
    if (fileData.reservas && Array.isArray(fileData.reservas)) {
      fileData.reservas.forEach(r => {
        const tipo = clasificarReserva(r);
        if (evolucionMap[mesKey][tipo] !== undefined) {
          evolucionMap[mesKey][tipo]++;
        }
      });
    }
  });

  const sortedMeses = Object.keys(evolucionMap).sort();
  const mesesLabels = sortedMeses.map(m => {
    const [anio, mes] = m.split('-');
    const nombresMeses = ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov', 'Dic'];
    return `${nombresMeses[parseInt(mes, 10) - 1]} ${anio}`;
  });

  const ctxEvolucion = document.getElementById("chartEvolucion").getContext("2d");
  charts.evolucion = new Chart(ctxEvolucion, {
    type: 'bar',
    data: {
      labels: mesesLabels.length > 0 ? mesesLabels : ['Sin datos'],
      datasets: [
        { label: 'Efectuadas (Usuario)', data: sortedMeses.map(m => evolucionMap[m].efectuada_user), backgroundColor: '#10B981', borderRadius: 4 },
        { label: 'Efectuadas (Admin)', data: sortedMeses.map(m => evolucionMap[m].efectuada_admin), backgroundColor: '#047857', borderRadius: 4 },
        { label: 'Canceladas por Usuario', data: sortedMeses.map(m => evolucionMap[m].cancelada_user), backgroundColor: '#F43F5E', borderRadius: 4 },
        { label: 'Canceladas por Admin', data: sortedMeses.map(m => evolucionMap[m].cancelada_admin), backgroundColor: '#BE123C', borderRadius: 4 }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      animation: false,
      scales: { x: { stacked: true }, y: { stacked: true, beginAtZero: true, ticks: { stepSize: 1 } } }
    }
  });

  // 4. Gráficos de Profesionales divididos en bloques de 10
  const profMap = {};
  reservas.forEach(r => {
    const prof = r._profesional;
    if (!profMap[prof]) {
      profMap[prof] = { efectuada_user: 0, efectuada_admin: 0, cancelada_user: 0, cancelada_admin: 0 };
    }
    profMap[prof][r._tipoClasificado]++;
  });

  const sortedProfs = Object.keys(profMap).sort((a, b) => {
    const totalA = profMap[a].efectuada_user + profMap[a].efectuada_admin + profMap[a].cancelada_user + profMap[a].cancelada_admin;
    const totalB = profMap[b].efectuada_user + profMap[b].efectuada_admin + profMap[b].cancelada_user + profMap[b].cancelada_admin;
    return totalB - totalA;
  });

  const containerProfs = document.getElementById("profesionales-container");
  containerProfs.innerHTML = "";

  if (sortedProfs.length === 0) return;

  const tamBloque = 10;
  const totalBloques = Math.ceil(sortedProfs.length / tamBloque);

  for (let i = 0; i < totalBloques; i++) {
    const inicio = i * tamBloque;
    const fin = Math.min(inicio + tamBloque, sortedProfs.length);
    const bloqueProfs = sortedProfs.slice(inicio, fin);
    const chartKey = `profesionales_${i}`;

    const cardDiv = document.createElement("div");
    cardDiv.className = "prof-card bg-white p-6 rounded-xl shadow";
    cardDiv.innerHTML = `
      <h3 class="text-lg font-semibold text-gray-700 mb-1">Actividad por Profesional (Ranking ${inicio + 1} al ${fin})</h3>
      <p class="text-xs text-gray-500 mb-4">Bloque ${i + 1} de ${totalBloques} — Efectuadas (Usuario/Admin) y Cancelaciones</p>
      <div class="relative h-96">
        <canvas id="chart-${chartKey}"></canvas>
        <img id="bn-${chartKey}" class="img-bn" alt="Actividad por Profesional Bloque ${i + 1}">
      </div>
    `;
    containerProfs.appendChild(cardDiv);

    const ctxProf = document.getElementById(`chart-${chartKey}`).getContext("2d");
    charts[chartKey] = new Chart(ctxProf, {
      type: 'bar',
      data: {
        labels: bloqueProfs,
        datasets: [
          { label: 'Efectuadas (Usuario)', data: bloqueProfs.map(p => profMap[p].efectuada_user), backgroundColor: '#10B981', borderRadius: 4 },
          { label: 'Efectuadas (Admin)', data: bloqueProfs.map(p => profMap[p].efectuada_admin), backgroundColor: '#047857', borderRadius: 4 },
          { label: 'Canceladas por Usuario', data: bloqueProfs.map(p => profMap[p].cancelada_user), backgroundColor: '#F43F5E', borderRadius: 4 },
          { label: 'Canceladas por Admin', data: bloqueProfs.map(p => profMap[p].cancelada_admin), backgroundColor: '#BE123C', borderRadius: 4 }
        ]
      },
      options: { 
        indexAxis: 'y',
        responsive: true, 
        maintainAspectRatio: false,
        animation: false,
        scales: { 
          x: { stacked: true, beginAtZero: true, ticks: { stepSize: 1 } }, 
          y: { stacked: true } 
        }
      }
    });
  }
}
```

### Archivo: `js/config.js`
```javascript
// js/config.js
export const CONFIG = {
  // false = usa datos de ejemplo locales (si aplica)
  // true = se conecta a la Netlify Function para leer el repositorio de GitHub real
  usarGitHubReal: true, 

};
```

### Archivo: `js/utils.js`
```javascript
// js/utils.js - Funciones auxiliares y de clasificación

// Escapa texto para insertarlo de forma segura dentro de innerHTML o de atributos HTML
export function escaparHtml(valor) {
  const mapa = { "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" };
  return String(valor ?? "").replace(/[&<>"']/g, c => mapa[c]);
}

export function extraerProfesional(r) {
  let summaryClean = r.summary.replace("Cancelada - ", "").trim();
  if (summaryClean.includes(":")) {
    const parts = summaryClean.split(":");
    if (parts.length > 1) {
      return parts[1].trim();
    }
  }
  const lineaPara = r.description.find(l => l.toLowerCase().includes("reserva para:"));
  if (lineaPara) {
    return lineaPara.split(":")[1].trim();
  }
  return "Desconocido";
}

export function clasificarReserva(r) {
  if (r.estado === "cancelada") {
    const descCancelacion = r.description.find(l => l.toLowerCase().includes("cancelada por:")) || "";
    const textoLower = descCancelacion.toLowerCase();
    if (textoLower.includes("gestor") || textoLower.includes("admin") || textoLower.includes("administracion")) {
      return "cancelada_admin";
    }
    return "cancelada_user";
  }

  // Comparamos "Reserva realizada por" / "Creada por" con "Reserva para:"
  const lineaPara = r.description.find(l => l.toLowerCase().includes("reserva para:")) || "";
  const lineaRealizada = r.description.find(l => 
    l.toLowerCase().includes("reserva realizada por:") || 
    l.toLowerCase().includes("creada por:") || 
    l.toLowerCase().includes("reservado por:")
  ) || "";

  const nombrePara = lineaPara.includes(":") ? lineaPara.split(":")[1].trim().toLowerCase() : "";
  const nombreRealizada = lineaRealizada.includes(":") ? lineaRealizada.split(":")[1].trim().toLowerCase() : "";

  // Si ambos campos existen y los nombres son distintos, la realizó la administración
  if (nombreRealizada && nombrePara && nombreRealizada !== nombrePara) {
    return "efectuada_admin";
  }

  // Validación de respaldo por palabras clave
  const textoCreacionLower = lineaRealizada.toLowerCase();
  if (textoCreacionLower.includes("gestor") || textoCreacionLower.includes("admin") || textoCreacionLower.includes("administracion")) {
    return "efectuada_admin";
  }

  return "efectuada_user";
}

export function crearTextura(tipo) {
  const t = document.createElement("canvas");
  t.width = t.height = 10;
  const g = t.getContext("2d");
  g.fillStyle = "#ffffff";
  g.fillRect(0, 0, 10, 10);
  g.strokeStyle = g.fillStyle = "#000000";
  g.lineWidth = 1.2;
  if (tipo === "puntos") {
    g.beginPath(); g.arc(5, 5, 1.2, 0, Math.PI * 2); g.fill();
  } else if (tipo === "rayas") {
    g.beginPath(); g.moveTo(-1, 11); g.lineTo(11, -1); g.stroke();
  } else if (tipo === "cuadricula") {
    g.lineWidth = 1.6;
    g.beginPath(); g.moveTo(-1, -1); g.lineTo(11, 11); g.moveTo(11, -1); g.lineTo(-1, 11); g.stroke();
  } else if (tipo === "diagonal") {
    g.lineWidth = 1.2;
    g.beginPath(); g.moveTo(0, 5); g.lineTo(5, 10); g.moveTo(5, 0); g.lineTo(10, 5); g.stroke();
  }
  return document.createElement("canvas").getContext("2d").createPattern(t, "repeat");
}
```

### Archivo: `netlify/functions/get-backups.js`
```javascript
// netlify/functions/get-backups.js
// Lee los resguardos JSON del repo de datos y los devuelve filtrados por fecha.
// El repo/carpeta/rama están fijos acá: el cliente NO puede elegirlos.

const PROPIETARIO = process.env.BACKUPS_OWNER || "fabior1981";
const REPOSITORIO = process.env.BACKUPS_REPO || "bd";
const RAMA = process.env.BACKUPS_BRANCH || "main";
const CARPETA = process.env.BACKUPS_FOLDER || "espacia/backups";

const TAMANO_LOTE = 20;          // descargas simultáneas
const PRESUPUESTO_MS = 8000;     // Netlify corta a los 10 s: cortamos antes y devolvemos lo que haya
const REGEX_FECHA = /^\d{4}-\d{2}-\d{2}$/;

const responder = (statusCode, datos, extra = {}) => ({
  statusCode,
  headers: { "Content-Type": "application/json", ...extra },
  body: JSON.stringify(datos)
});

const fechaDeNombre = (nombre) => {
  const m = nombre.match(/\d{4}-\d{2}-\d{2}/);
  return m ? m[0] : null;
};

// Lista los .json de la carpeta. Usa Git Trees (hasta 100.000 entradas);
// si falla, cae a la API de contents (tope de 1000 archivos).
async function listarArchivos(headers) {
  const urlTree =
    `https://api.github.com/repos/${PROPIETARIO}/${REPOSITORIO}/git/trees/${RAMA}:${CARPETA}`;
  const resTree = await fetch(urlTree, { headers });

  if (resTree.ok) {
    const tree = await resTree.json();
    if (tree.truncated) console.error("Git Trees devolvió una lista truncada");
    return (tree.tree || [])
      .filter(e => e.type === "blob" && e.path.endsWith(".json"))
      .map(e => ({
        name: e.path,
        url: `https://raw.githubusercontent.com/${PROPIETARIO}/${REPOSITORIO}/${RAMA}/${CARPETA}/${encodeURIComponent(e.path)}`
      }));
  }

  console.error("Git Trees falló (", resTree.status, "), uso contents como respaldo");
  const urlContents =
    `https://api.github.com/repos/${PROPIETARIO}/${REPOSITORIO}/contents/${CARPETA}?ref=${RAMA}`;
  const resContents = await fetch(urlContents, { headers });
  if (!resContents.ok) {
    console.error("GitHub respondió", resContents.status, await resContents.text());
    throw new Error("No se pudo leer el repositorio de datos");
  }
  const archivos = await resContents.json();
  return archivos
    .filter(a => a.type === "file" && a.name.endsWith(".json"))
    .map(a => ({ name: a.name, url: a.download_url }));
}

exports.handler = async function (event) {
  const inicio = Date.now();

  if (event.httpMethod !== "GET") {
    return responder(405, { error: "Método no permitido" });
  }

  const { fechaInicio, fechaFin } = event.queryStringParameters || {};

  if ((fechaInicio && !REGEX_FECHA.test(fechaInicio)) ||
      (fechaFin && !REGEX_FECHA.test(fechaFin))) {
    return responder(400, { error: "Formato de fecha inválido (use AAAA-MM-DD)" });
  }

  const token = process.env.GITHUB_TOKEN_ESPACIA_REPORTES_BD;
  const headers = {
    "User-Agent": "Netlify-Serverless-App",
    "Accept": "application/vnd.github.v3+json"
  };
  if (token) headers["Authorization"] = `Bearer ${token}`;

  try {
    let archivos = await listarArchivos(headers);

    // 1) Filtrar por la fecha del nombre ANTES de descargar
    if (fechaInicio || fechaFin) {
      archivos = archivos.filter(a => {
        const f = fechaDeNombre(a.name);
        if (!f) return true;
        if (fechaInicio && f < fechaInicio) return false;
        if (fechaFin && f > fechaFin) return false;
        return true;
      });
    }

    // 2) Más recientes primero: si hay que cortar por tiempo, se pierden los más viejos
    archivos.sort((a, b) => (fechaDeNombre(b.name) || "").localeCompare(fechaDeNombre(a.name) || ""));

    const descargar = async (archivo) => {
      try {
        const res = await fetch(archivo.url, { headers });
        if (!res.ok) return null;
        return await res.json();
      } catch (e) {
        console.error("Error descargando", archivo.name, e.message);
        return null;
      }
    };

    // 3) Descargar en paralelo, por lotes, respetando el presupuesto de tiempo
    const resultados = [];
    let parcial = false;
    for (let i = 0; i < archivos.length; i += TAMANO_LOTE) {
      if (Date.now() - inicio > PRESUPUESTO_MS) {
        parcial = true;
        break;
      }
      const lote = archivos.slice(i, i + TAMANO_LOTE);
      resultados.push(...await Promise.all(lote.map(descargar)));
    }

    // 4) Filtro final por fechaReservas y descarte de resguardos vacíos
    const datos = resultados.filter(d => {
      if (!d || !Array.isArray(d.reservas) || d.reservas.length === 0) return false;
      const f = d.fechaReservas;
      if (f) {
        if (fechaInicio && f < fechaInicio) return false;
        if (fechaFin && f > fechaFin) return false;
      }
      return true;
    });

    return responder(200, datos, {
      "Cache-Control": "private, max-age=60",
      "X-Resultado-Parcial": parcial ? "true" : "false"
    });

  } catch (error) {
    console.error("Error en get-backups:", error);
    return responder(502, { error: "No se pudo leer el repositorio de datos" });
  }
};

```

