# Bundle del Proyecto para IA: generador-documento

## Estructura de Archivos
- README.md
- app.js
- documentacion/LEEME.md
- index.html
- styles.css

---

## Código Fuente

### Archivo: `README.md`
```md
# Generador Universal de Informes

Herramienta web para armar informes formales y descargarlos como documento Word (`.docx`). Se completan los datos en un formulario y la app genera un documento con portada, secciones, tabla de métricas y firmas.

Sitio publicado: https://generador-documento.netlify.app

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/generador-documento/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/generador-documento/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## Funcionalidades

- **Estilos visuales**: Moderno, Corporativo Oscuro, Ecológico, Tecnológico y Clásico Papel.
- **Nivel de formalidad**: Estándar, Interno / Operativo, Ejecutivo y Solemne / Legal.
- **Alineación del cuerpo**: izquierda o justificado.
- **Marca de agua** opcional: BORRADOR, CONFIDENCIAL o PRELIMINAR.
- **Imagen de encabezado** (PNG o JPG, hasta 5 MB).
- **Bloque de firmas**: sin firmas, 1 o 2 firmas.
- **Metadatos**: título, subtítulo o área, autor, departamento, destinatario y fecha.
- **Contenido**: resumen ejecutivo, tabla de métricas o indicadores (se pueden agregar las filas que hagan falta), conclusiones y siguientes pasos, y estado final (BORRADOR, APROBADO, REQUIERE ACCIÓN o CONFIDENCIAL).
- **Vista previa** antes de descargar.

## Cómo se usa

1. Elegí el estilo, la formalidad y las opciones de formato.
2. Completá los datos del documento y el contenido. Los campos con `*` son obligatorios.
3. Con **+ Agregar métrica** sumás filas a la tabla de indicadores.
4. Tocá **Vista Previa** para revisarlo.
5. Tocá **Generar Documento Word** para descargar el `.docx`.

## Ejecutar localmente

No necesita instalación. Abrí `index.html` en el navegador. Hace falta conexión a internet, porque las librerías `docx` y `FileSaver` se cargan desde unpkg.

## Estructura

```
index.html   Formulario y vista previa
styles.css   Estilos
app.js       Lógica: vista previa, KPIs, generación del .docx
```

```

### Archivo: `app.js`
```javascript
/* ============================================================
   GENERADOR UNIVERSAL DE INFORMES — APP V2
   ============================================================ */

// ─── CONFIGURACIONES ─────────────────────────────────────────
const ESTILOS = {
  moderno: {
    name: 'Moderno',
    headerBg: 'F1F5F9',
    headerTitleColor: '0F172A',
    headerSubtitleColor: '0284C7',
    headerBorderLeft: { style: 'SINGLE', size: 36, color: '0284C7' },
    secBg: 'F1F5F9',
    secTextColor: '0F172A',
    bodyTextColor: '1E293B',
    secBorders: {
      top: 'NONE', bottom: 'NONE', left: 'SINGLE', right: 'NONE',
      leftColor: '0284C7', leftSize: 36
    },
    kpiHeaderBg: '0F172A',
    kpiHeaderText: 'FFFFFF',
    estadoColors: {
      BORRADOR: 'E2E8F0',
      APROBADO: 'D1FAE5',
      'REQUIERE ACCIÓN': 'FEE2E2',
      CONFIDENCIAL: 'FEF3C7'
    },
    metaBg: 'F8FAFC',
    metaBorder: 'E2E8F0',
    quoteBg: 'F1F5F9',
    quoteBorder: '0284C7'
  },
  corporativo: {
    name: 'Corporativo Oscuro',
    headerBg: '1E3A5F',
    headerTitleColor: 'FFFFFF',
    headerSubtitleColor: 'C9A227',
    headerBorderLeft: { style: 'SINGLE', size: 36, color: 'C9A227' },
    secBg: '1E3A5F',
    secTextColor: 'FFFFFF',
    bodyTextColor: '1E293B',
    secBorders: {
      top: 'NONE', bottom: 'SINGLE', left: 'NONE', right: 'NONE',
      bottomColor: 'C9A227', bottomSize: 24
    },
    kpiHeaderBg: '1E3A5F',
    kpiHeaderText: 'FFFFFF',
    estadoColors: {
      BORRADOR: 'E2E8F0',
      APROBADO: 'D1FAE5',
      'REQUIERE ACCIÓN': 'FEE2E2',
      CONFIDENCIAL: 'FEF3C7'
    },
    metaBg: 'F1F5F9',
    metaBorder: '1E3A5F',
    quoteBg: 'F1F5F9',
    quoteBorder: 'C9A227'
  },
  ecologico: {
    name: 'Ecológico',
    headerBg: 'F0FDF4',
    headerTitleColor: '14532D',
    headerSubtitleColor: '16A34A',
    headerBorderLeft: { style: 'SINGLE', size: 36, color: '15803D' },
    secBg: 'F0FDF4',
    secTextColor: '14532D',
    bodyTextColor: '1E293B',
    secBorders: {
      top: 'NONE', bottom: 'NONE', left: 'SINGLE', right: 'NONE',
      leftColor: '15803D', leftSize: 36
    },
    kpiHeaderBg: '15803D',
    kpiHeaderText: 'FFFFFF',
    estadoColors: {
      BORRADOR: 'E2E8F0',
      APROBADO: 'DCFCE7',
      'REQUIERE ACCIÓN': 'FEE2E2',
      CONFIDENCIAL: 'FEF9C3'
    },
    metaBg: 'F0FDF4',
    metaBorder: '16A34A',
    quoteBg: 'F0FDF4',
    quoteBorder: '16A34A'
  },
  tecnologico: {
    name: 'Tecnológico',
    headerBg: '0F172A',
    headerTitleColor: 'FFFFFF',
    headerSubtitleColor: '38BDF8',
    headerBorderLeft: { style: 'SINGLE', size: 36, color: '38BDF8' },
    secBg: '1E293B',
    secTextColor: 'FFFFFF',
    bodyTextColor: '1E293B',
    secBorders: {
      top: 'NONE', bottom: 'NONE', left: 'SINGLE', right: 'NONE',
      leftColor: '0EA5E9', leftSize: 36
    },
    kpiHeaderBg: '0F172A',
    kpiHeaderText: '38BDF8',
    estadoColors: {
      BORRADOR: 'E2E8F0',
      APROBADO: 'D1FAE5',
      'REQUIERE ACCIÓN': 'FEE2E2',
      CONFIDENCIAL: 'FEF3C7'
    },
    metaBg: 'F1F5F9',
    metaBorder: '0F172A',
    quoteBg: 'F1F5F9',
    quoteBorder: '0EA5E9'
  },
  clasico: {
    name: 'Clásico Papel',
    headerBg: 'FFFBEB',
    headerTitleColor: '78350F',
    headerSubtitleColor: '92400E',
    headerBorderLeft: { style: 'DOUBLE', size: 24, color: '92400E' },
    secBg: 'FEF3C7',
    secTextColor: '78350F',
    bodyTextColor: '451A03',
    secBorders: {
      top: 'SINGLE', bottom: 'SINGLE', left: 'NONE', right: 'NONE',
      topColor: '92400E', topSize: 12,
      bottomColor: '92400E', bottomSize: 12
    },
    kpiHeaderBg: '92400E',
    kpiHeaderText: 'FFFBEB',
    estadoColors: {
      BORRADOR: 'E2E8F0',
      APROBADO: 'DCFCE7',
      'REQUIERE ACCIÓN': 'FEE2E2',
      CONFIDENCIAL: 'FEF9C3'
    },
    metaBg: 'FFFBEB',
    metaBorder: '92400E',
    quoteBg: 'FFFBEB',
    quoteBorder: '92400E'
  }
};

const FORMALIDADES = {
  estandar: {
    font: 'Calibri',
    lineSpacing: 276,
    sectionUppercase: false,
    addQuotes: false,
    compact: false,
    spacious: false,
    romanNumbers: false,
    sectionSpacing: { before: 120, after: 120 }
  },
  interno: {
    font: 'Arial',
    lineSpacing: 240,
    sectionUppercase: false,
    addQuotes: false,
    compact: true,
    spacious: false,
    romanNumbers: false,
    sectionSpacing: { before: 80, after: 80 }
  },
  ejecutivo: {
    font: 'Georgia',
    lineSpacing: 300,
    sectionUppercase: false,
    addQuotes: true,
    compact: false,
    spacious: true,
    romanNumbers: false,
    sectionSpacing: { before: 200, after: 160 }
  },
  solemne: {
    font: 'Times New Roman',
    lineSpacing: 360,
    sectionUppercase: true,
    addQuotes: false,
    compact: false,
    spacious: true,
    romanNumbers: true,
    sectionSpacing: { before: 240, after: 200 }
  }
};

const ROMAN_NUMERALS = ['I', 'II', 'III', 'IV', 'V', 'VI'];

// ─── ESTADO ──────────────────────────────────────────────────
let kpis = [
  { label: 'Presupuesto Ejecutado', value: '84%' },
  { label: 'Nivel de Cumplimiento', value: 'Conforme a Cronograma' },
  { label: 'Satisfacción del Cliente', value: '4.7 / 5.0' },
  { label: 'Tiempo Medio de Respuesta', value: '18 minutos' },
  { label: 'Procesos Automatizados', value: '85%' },
  { label: 'Personal Certificado', value: '94%' }
];
let sectionCounter = 0;

// ─── INICIALIZACIÓN ──────────────────────────────────────────
document.addEventListener('DOMContentLoaded', () => {
  document.getElementById('fechaDoc').valueAsDate = new Date();
  renderKPIs();
  updateStylePreview();
  updateFormalityHint();
  updateSubChecks();
  initPreview();

  document.getElementById('btnAddKpi').addEventListener('click', addKpi);
  document.getElementById('reportForm').addEventListener('submit', (e) => {
    e.preventDefault();
    generarWord();
  });
  document.getElementById('headerImage').addEventListener('change', onImageChange);
  document.getElementById('estiloVisual').addEventListener('change', updateStylePreview);
  document.getElementById('nivelFormalidad').addEventListener('change', updateFormalityHint);
  document.getElementById('incluirContenido').addEventListener('change', updateSubChecks);
});

function onImageChange(e) {
  const file = e.target.files[0];
  const hint = document.getElementById('imageHint');
  if (file) {
    const sizeMB = (file.size / (1024 * 1024)).toFixed(2);
    hint.textContent = `Seleccionado: ${file.name} (${sizeMB} MB)`;
    hint.style.color = '#0284c7';
  } else {
    hint.textContent = 'Ninguna imagen seleccionada.';
    hint.style.color = '#64748b';
  }
}

function updateStylePreview() {
  const estilo = document.getElementById('estiloVisual').value;
  const preview = document.getElementById('stylePreview');
  preview.className = 'style-preview ' + estilo;
  preview.textContent = ESTILOS[estilo].name;
}

function updateFormalityHint() {
  const f = document.getElementById('nivelFormalidad').value;
  const cfg = FORMALIDADES[f];
  const hints = {
    estandar: `Fuente ${cfg.font}, interlineado normal`,
    interno: `Fuente ${cfg.font}, formato compacto`,
    ejecutivo: `Fuente ${cfg.font}, espaciado amplio, citas en bloque`,
    solemne: `Fuente ${cfg.font}, doble espacio, numeración romana, blanco y negro`
  };
  document.getElementById('formalityHint').textContent = hints[f];
}

function updateSubChecks() {
  const checked = document.getElementById('incluirContenido').checked;
  const container = document.getElementById('subChecksContent');
  container.classList.toggle('disabled', !checked);
  const groups = ['resumenGroup', 'desarrolloGroup', 'kpisGroup', 'conclusionesGroup', 'estadoGroup'];
  groups.forEach(id => {
    const el = document.getElementById(id);
    if (el) el.style.opacity = checked ? '1' : '0.4';
  });
}

// ─── KPIs DINÁMICOS ──────────────────────────────────────────
function renderKPIs() {
  const container = document.getElementById('kpiContainer');
  container.innerHTML = '';
  kpis.forEach((kpi, index) => {
    const row = document.createElement('div');
    row.className = 'kpi-row';

    const inputLabel = document.createElement('input');
    inputLabel.type = 'text';
    inputLabel.placeholder = 'Métrica / Indicador';
    inputLabel.value = kpi.label;
    inputLabel.required = true;
    inputLabel.addEventListener('input', (e) => { kpis[index].label = e.target.value; });

    const inputValue = document.createElement('input');
    inputValue.type = 'text';
    inputValue.placeholder = 'Valor / Estado';
    inputValue.value = kpi.value;
    inputValue.required = true;
    inputValue.addEventListener('input', (e) => { kpis[index].value = e.target.value; });

    row.appendChild(inputLabel);
    row.appendChild(inputValue);

    if (kpis.length > 1) {
      const btnRemove = document.createElement('button');
      btnRemove.type = 'button';
      btnRemove.className = 'btn-remove';
      btnRemove.textContent = '×';
      btnRemove.setAttribute('aria-label', 'Eliminar métrica');
      btnRemove.addEventListener('click', () => {
        kpis.splice(index, 1);
        renderKPIs();
      });
      row.appendChild(btnRemove);
    }
    container.appendChild(row);
  });
}

function addKpi() {
  kpis.push({ label: '', value: '' });
  renderKPIs();
  const rows = document.querySelectorAll('.kpi-row');
  if (rows.length > 0) {
    const lastRow = rows[rows.length - 1];
    const firstInput = lastRow.querySelector('input');
    if (firstInput) firstInput.focus();
  }
}

// ─── HELPERS ─────────────────────────────────────────────────
function readImageAsBuffer(file) {
  return new Promise((resolve, reject) => {
    if (!file) return resolve(null);
    const reader = new FileReader();
    reader.onload = () => resolve(reader.result);
    reader.onerror = () => reject(new Error('No se pudo leer la imagen seleccionada.'));
    reader.readAsArrayBuffer(file);
  });
}

function getImageDimensions(file) {
  return new Promise((resolve, reject) => {
    if (!file) return resolve(null);
    const img = new Image();
    const url = URL.createObjectURL(file);
    img.onload = () => {
      URL.revokeObjectURL(url);
      resolve({ width: img.naturalWidth, height: img.naturalHeight });
    };
    img.onerror = () => {
      URL.revokeObjectURL(url);
      reject(new Error('No se pudo obtener las dimensiones de la imagen.'));
    };
    img.src = url;
  });
}

function calcProportionalSize(imgW, imgH, maxW, maxH) {
  const ratio = Math.min(maxW / imgW, maxH / imgH, 1);
  return { width: Math.round(imgW * ratio), height: Math.round(imgH * ratio) };
}

function formatDateES(dateString) {
  if (!dateString) return '';
  const d = new Date(dateString + 'T00:00:00');
  if (isNaN(d.getTime())) return dateString;
  return d.toLocaleDateString('es-ES', { day: 'numeric', month: 'long', year: 'numeric' });
}

function showToast(message, type = 'error') {
  const box = document.getElementById('toastBox');
  box.innerHTML = `<div class="toast ${type}">${message}</div>`;
  box.classList.add('visible');
  setTimeout(() => {
    box.classList.remove('visible');
    box.innerHTML = '';
  }, 5000);
}

function createWatermarkImage(text) {
  return new Promise((resolve) => {
    const canvas = document.createElement('canvas');
    const ctx = canvas.getContext('2d');
    const W = 1200, H = 1700;
    canvas.width = W;
    canvas.height = H;
    ctx.clearRect(0, 0, W, H);
    const fontSize = 130;
    ctx.font = `bold ${fontSize}px Arial, sans-serif`;
    ctx.fillStyle = 'rgba(160, 160, 160, 0.18)';
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    ctx.save();
    ctx.translate(W / 2, H / 2);
    ctx.rotate(-Math.PI / 4);
    ctx.fillText(text, 0, 0);
    ctx.fillText(text, 0, -fontSize * 2.2);
    ctx.fillText(text, 0, fontSize * 2.2);
    ctx.restore();
    canvas.toBlob((blob) => {
      const reader = new FileReader();
      reader.onload = () => resolve(reader.result);
      reader.readAsArrayBuffer(blob);
    }, 'image/png');
  });
}

// ─── CONSTRUCCIÓN DEL DOCUMENTO ──────────────────────────────
async function generarWord() {
  const btn = document.getElementById('btnExport');
  const originalText = btn.textContent;
  sectionCounter = 0;

  const form = document.getElementById('reportForm');
  if (!form.checkValidity()) {
    form.reportValidity();
    return;
  }

  if (!window.docx || !window.docx.Document) {
    showToast('Error: la librería docx.js no se cargó correctamente. Verificá tu conexión.', 'error');
    return;
  }

  btn.disabled = true;
  btn.textContent = 'Generando documento…';

  try {
    const {
      Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
      WidthType, AlignmentType, ImageRun, BorderStyle, Header, Footer, PageNumber
    } = window.docx;

    // ── 1. Lectura de campos ────────────────────────────────
    const estiloKey = document.getElementById('estiloVisual').value;
    const formalidadKey = document.getElementById('nivelFormalidad').value;
    const marcaAgua = document.getElementById('marcaAgua').value;
    const alineacionSel = document.getElementById('alineacionTexto').value;
    const numFirmas = parseInt(document.getElementById('numFirmas').value, 10);

    const incluirMetadatos = document.getElementById('incluirMetadatos').checked;
    const incluirContenido = document.getElementById('incluirContenido').checked;
    const incluirResumen = document.getElementById('incluirResumen').checked;
    const incluirDesarrollo = document.getElementById('incluirDesarrollo').checked;
    const incluirKPIs = document.getElementById('incluirKPIs').checked;
    const incluirConclusiones = document.getElementById('incluirConclusiones').checked;
    const incluirEstado = document.getElementById('incluirEstado').checked;

    const imageInput = document.getElementById('headerImage')?.files[0];
    const titulo = document.getElementById('tituloInforme').value;
    const subtitulo = document.getElementById('subtituloInforme').value;
    const autor = document.getElementById('autor').value;
    const departamento = document.getElementById('departamento').value;
    const destinatario = document.getElementById('destinatario').value;
    const fechaRaw = document.getElementById('fechaDoc').value;
    const fechaStr = formatDateES(fechaRaw);
    const resumen = document.getElementById('resumenEjecutivo').value;
    const desarrollo = document.getElementById('desarrolloText').value;
    const conclusiones = document.getElementById('conclusionesText').value;
    const estado = document.getElementById('estadoDoc').value;

    // ── 2. Configuraciones ──────────────────────────────────
    let CFG = { ...ESTILOS[estiloKey] };
    let FMT = { ...FORMALIDADES[formalidadKey] };

    // Solemne anula colores del estilo
    if (formalidadKey === 'solemne') {
      CFG = {
        ...CFG,
        headerBg: 'FFFFFF',
        headerTitleColor: '000000',
        headerSubtitleColor: '333333',
        headerBorderLeft: { style: 'SINGLE', size: 36, color: '000000' },
        secBg: 'FFFFFF',
        secTextColor: '000000',
        bodyTextColor: '000000',
        secBorders: {
          top: 'NONE', bottom: 'SINGLE', left: 'NONE', right: 'NONE',
          bottomColor: '000000', bottomSize: 12
        },
        kpiHeaderBg: '000000',
        kpiHeaderText: 'FFFFFF',
        metaBg: 'FFFFFF',
        metaBorder: '000000',
        quoteBg: 'FFFFFF',
        quoteBorder: '000000'
      };
    }

    function alignmentMap(val) {
      return val === 'JUSTIFY' ? AlignmentType.JUSTIFY : AlignmentType.LEFT;
    }

    // ── 3. Validación de imagen ─────────────────────────────
    let headerImageBuffer = null;
    let headerImageSize = { width: 120, height: 60 };
    if (imageInput) {
      const ALLOWED_TYPES = ['image/png', 'image/jpeg'];
      const MAX_SIZE_MB = 5;
      if (!ALLOWED_TYPES.includes(imageInput.type)) {
        throw new Error('Solo se permiten imágenes PNG o JPG.');
      }
      if (imageInput.size > MAX_SIZE_MB * 1024 * 1024) {
        throw new Error(`La imagen no debe superar los ${MAX_SIZE_MB} MB.`);
      }
      const dims = await getImageDimensions(imageInput);
      headerImageSize = calcProportionalSize(dims.width, dims.height, 150, 90);
      headerImageBuffer = await readImageAsBuffer(imageInput);
    }

    // ── 4. Marca de agua ────────────────────────────────────
    let headerConfig = undefined;
    if (marcaAgua !== 'NINGUNA') {
      const watermarkBuffer = await createWatermarkImage(marcaAgua.toUpperCase());
      headerConfig = new Header({
        children: [
          new Paragraph({
            alignment: AlignmentType.CENTER,
            children: [
              new ImageRun({
                data: watermarkBuffer,
                transformation: { width: 520, height: 720 },
                floating: {
                  horizontalPosition: { relative: 'page', align: 'center' },
                  verticalPosition: { relative: 'page', align: 'center' },
                  wrap: { type: 'none' },
                  behindDocument: true
                }
              })
            ]
          })
        ]
      });
    }

    // ── 5. Helpers de construcción ──────────────────────────
    function makeBorder(style, size, color) {
      if (style === 'NONE' || !style) return { style: BorderStyle.NONE, size: 0, color: 'auto' };
      const bs = BorderStyle[style] || BorderStyle.SINGLE;
      return { style: bs, size: size || 12, color: color || '000000' };
    }

    function buildCellBorders(cfg) {
      const b = cfg.secBorders;
      return {
        top: makeBorder(b.top, b.topSize, b.topColor),
        bottom: makeBorder(b.bottom, b.bottomSize, b.bottomColor),
        left: makeBorder(b.left, b.leftSize, b.leftColor),
        right: makeBorder(b.right, b.rightSize, b.rightColor)
      };
    }

    function buildHeaderCellBorders(cfg) {
      const bl = cfg.headerBorderLeft;
      return {
        top: makeBorder('NONE', 0, 'auto'),
        bottom: makeBorder('NONE', 0, 'auto'),
        left: makeBorder(bl.style, bl.size, bl.color),
        right: makeBorder('NONE', 0, 'auto')
      };
    }

    function sectionTitle(text) {
      sectionCounter++;
      let display = text;
      if (FMT.romanNumbers) {
        display = `${ROMAN_NUMERALS[sectionCounter - 1] || sectionCounter}. ${text}`;
      }
      if (FMT.sectionUppercase) {
        display = display.toUpperCase();
      }
      return display;
    }

    function tr(opts) {
      return new TextRun({
        text: opts.text || '',
        bold: opts.bold || false,
        italics: opts.italics || false,
        color: opts.color || '000000',
        size: opts.size || 20,
        font: opts.font || FMT.font
      });
    }

    function cellShading(fill) {
      return fill && fill !== 'FFFFFF' ? { fill } : undefined;
    }

    // ── 6. Banner Header ────────────────────────────────────
    const bannerHeader = new Table({
      width: { size: 100, type: WidthType.PERCENTAGE },
      rows: [
        new TableRow({
          children: [
            new TableCell({
              width: { size: headerImageBuffer ? 70 : 100, type: WidthType.PERCENTAGE },
              shading: cellShading(CFG.headerBg),
              margins: { top: 180, bottom: 180, left: 200, right: 200 },
              borders: buildHeaderCellBorders(CFG),
              children: [
                new Paragraph({
                  spacing: { after: 60 },
                  children: [tr({ text: subtitulo.toUpperCase(), color: CFG.headerSubtitleColor, size: 16, bold: true })]
                }),
                new Paragraph({
                  children: [tr({ text: titulo, color: CFG.headerTitleColor, size: 28, bold: true })]
                })
              ]
            }),
            ...(headerImageBuffer ? [
              new TableCell({
                width: { size: 30, type: WidthType.PERCENTAGE },
                shading: cellShading(CFG.headerBg),
                margins: { top: 100, bottom: 100, left: 100, right: 100 },
                borders: {
                  top: makeBorder('NONE', 0, 'auto'),
                  bottom: makeBorder('NONE', 0, 'auto'),
                  left: makeBorder('NONE', 0, 'auto'),
                  right: makeBorder('NONE', 0, 'auto')
                },
                children: [
                  new Paragraph({
                    alignment: AlignmentType.CENTER,
                    children: [new ImageRun({
                      data: headerImageBuffer,
                      transformation: { width: headerImageSize.width, height: headerImageSize.height }
                    })]
                  })
                ]
              })
            ] : [])
          ]
        })
      ]
    });

    // ── 7. Metadatos ────────────────────────────────────────
    const tablaMetadatos = new Table({
      width: { size: 100, type: WidthType.PERCENTAGE },
      rows: [
        new TableRow({
          children: [
            new TableCell({
              width: { size: 50, type: WidthType.PERCENTAGE },
              shading: cellShading(CFG.metaBg),
              margins: { top: 80, bottom: 80, left: 120, right: 120 },
              borders: {
                top: makeBorder('NONE', 0, 'auto'),
                bottom: makeBorder('NONE', 0, 'auto'),
                left: makeBorder('NONE', 0, 'auto'),
                right: makeBorder('NONE', 0, 'auto')
              },
              children: [
                new Paragraph({ children: [
                  tr({ text: 'Elaborado por: ', bold: true, size: 18, color: '334155' }),
                  tr({ text: autor, size: 18 })
                ]}),
                new Paragraph({ children: [
                  tr({ text: 'Área: ', bold: true, size: 18, color: '334155' }),
                  tr({ text: departamento, size: 18 })
                ]})
              ]
            }),
            new TableCell({
              width: { size: 50, type: WidthType.PERCENTAGE },
              shading: cellShading(CFG.metaBg),
              margins: { top: 80, bottom: 80, left: 120, right: 120 },
              borders: {
                top: makeBorder('NONE', 0, 'auto'),
                bottom: makeBorder('NONE', 0, 'auto'),
                left: makeBorder('NONE', 0, 'auto'),
                right: makeBorder('NONE', 0, 'auto')
              },
              children: [
                new Paragraph({ children: [
                  tr({ text: 'Dirigido a: ', bold: true, size: 18, color: '334155' }),
                  tr({ text: destinatario, size: 18 })
                ]}),
                new Paragraph({ children: [
                  tr({ text: 'Fecha: ', bold: true, size: 18, color: '334155' }),
                  tr({ text: fechaStr, size: 18 })
                ]})
              ]
            })
          ]
        })
      ]
    });

    // ── 8. Barra de sección ─────────────────────────────────
    function crearBarraSeccion(texto) {
      return new Table({
        width: { size: 100, type: WidthType.PERCENTAGE },
        rows: [
          new TableRow({
            children: [
              new TableCell({
                shading: cellShading(CFG.secBg),
                margins: { top: 100, bottom: 100, left: 140, right: 140 },
                borders: buildCellBorders(CFG),
                children: [
                  new Paragraph({
                    children: [tr({ text: sectionTitle(texto), bold: true, color: CFG.secTextColor, size: 22 })]
                  })
                ]
              })
            ]
          })
        ]
      });
    }

    // ── 9. Cita en bloque (ejecutivo) ───────────────────────
    function crearCitaBloque(texto) {
      return new Table({
        width: { size: 100, type: WidthType.PERCENTAGE },
        rows: [
          new TableRow({
            children: [
              new TableCell({
                shading: cellShading(CFG.quoteBg),
                margins: { top: 120, bottom: 120, left: 200, right: 200 },
                borders: {
                  top: makeBorder('NONE', 0, 'auto'),
                  bottom: makeBorder('NONE', 0, 'auto'),
                  left: makeBorder('SINGLE', 36, CFG.quoteBorder),
                  right: makeBorder('NONE', 0, 'auto')
                },
                children: [
                  new Paragraph({
                    alignment: alignmentMap(alineacionSel),
                    spacing: { line: FMT.lineSpacing },
                    children: [tr({ text: texto, size: 20, italics: true, color: CFG.bodyTextColor })]
                  })
                ]
              })
            ]
          })
        ]
      });
    }

    // ── 10. KPIs ────────────────────────────────────────────
    const kpiRows = [];
    kpiRows.push(
      new TableRow({
        children: [
          new TableCell({
            shading: cellShading(CFG.kpiHeaderBg),
            children: [new Paragraph({
              alignment: AlignmentType.CENTER,
              children: [tr({ text: 'VARIABLE / INDICADOR', bold: true, color: CFG.kpiHeaderText, size: 18 })]
            })]
          }),
          new TableCell({
            shading: cellShading(CFG.kpiHeaderBg),
            children: [new Paragraph({
              alignment: AlignmentType.CENTER,
              children: [tr({ text: 'VALOR / ESTADO', bold: true, color: CFG.kpiHeaderText, size: 18 })]
            })]
          })
        ]
      })
    );
    kpis.forEach(kpi => {
      kpiRows.push(
        new TableRow({
          children: [
            new TableCell({
              margins: { top: 80, bottom: 80, left: 100, right: 100 },
              borders: {
                top: makeBorder('NONE', 0, 'auto'),
                bottom: makeBorder('NONE', 0, 'auto'),
                left: makeBorder('NONE', 0, 'auto'),
                right: makeBorder('NONE', 0, 'auto')
              },
              children: [new Paragraph({ children: [tr({ text: kpi.label, size: 18 })] })]
            }),
            new TableCell({
              margins: { top: 80, bottom: 80, left: 100, right: 100 },
              borders: {
                top: makeBorder('NONE', 0, 'auto'),
                bottom: makeBorder('NONE', 0, 'auto'),
                left: makeBorder('NONE', 0, 'auto'),
                right: makeBorder('NONE', 0, 'auto')
              },
              children: [new Paragraph({
                alignment: AlignmentType.CENTER,
                children: [tr({ text: kpi.value, bold: true, size: 18 })]
              })]
            })
          ]
        })
      );
    });

    const tablaKPIs = new Table({
      width: { size: 100, type: WidthType.PERCENTAGE },
      rows: kpiRows
    });

    // ── 11. Estado ──────────────────────────────────────────
    const colorFondoEstado = CFG.estadoColors[estado] || 'E2E8F0';
    const tablaEstado = new Table({
      width: { size: 100, type: WidthType.PERCENTAGE },
      rows: [
        new TableRow({
          children: [
            new TableCell({
              width: { size: 40, type: WidthType.PERCENTAGE },
              shading: cellShading(CFG.kpiHeaderBg),
              margins: { top: 100, bottom: 100, left: 100, right: 100 },
              borders: {
                top: makeBorder('NONE', 0, 'auto'),
                bottom: makeBorder('NONE', 0, 'auto'),
                left: makeBorder('NONE', 0, 'auto'),
                right: makeBorder('NONE', 0, 'auto')
              },
              children: [new Paragraph({
                alignment: AlignmentType.CENTER,
                children: [tr({ text: 'ESTADO DEL DOCUMENTO', bold: true, color: CFG.kpiHeaderText, size: 18 })]
              })]
            }),
            new TableCell({
              width: { size: 60, type: WidthType.PERCENTAGE },
              shading: cellShading(colorFondoEstado),
              margins: { top: 100, bottom: 100, left: 100, right: 100 },
              borders: {
                top: makeBorder('NONE', 0, 'auto'),
                bottom: makeBorder('NONE', 0, 'auto'),
                left: makeBorder('NONE', 0, 'auto'),
                right: makeBorder('NONE', 0, 'auto')
              },
              children: [new Paragraph({
                alignment: AlignmentType.CENTER,
                children: [tr({ text: estado, bold: true, size: 20, color: CFG.bodyTextColor })]
              })]
            })
          ]
        })
      ]
    });

    // ── 12. Firmas ──────────────────────────────────────────
    let bloqueFirmas = [];
    if (numFirmas > 0) {
      const celdasFirma = [];
      for (let i = 0; i < numFirmas; i++) {
        celdasFirma.push(
          new TableCell({
            borders: {
              top: makeBorder('SINGLE', 12, '000000'),
              bottom: makeBorder('NONE', 0, 'auto'),
              left: makeBorder('NONE', 0, 'auto'),
              right: makeBorder('NONE', 0, 'auto')
            },
            children: [
              new Paragraph({
                alignment: AlignmentType.CENTER,
                children: [tr({ text: i === 0 ? autor : destinatario, bold: true, size: 18 })]
              }),
              new Paragraph({
                alignment: AlignmentType.CENTER,
                children: [tr({ text: i === 0 ? departamento : 'Revisado / Aprobado', size: 16, color: '64748B' })]
              })
            ]
          })
        );
      }
      bloqueFirmas = [
        new Paragraph({ spacing: { before: 600 } }),
        new Table({
          width: { size: 100, type: WidthType.PERCENTAGE },
          rows: [new TableRow({ children: celdasFirma })]
        })
      ];
    }

    // ── 13. Párrafos del cuerpo ─────────────────────────────
    function bodyParagraph(text, extraSpacing = {}) {
      return new Paragraph({
        alignment: alignmentMap(alineacionSel),
        spacing: { line: FMT.lineSpacing, after: 180, ...extraSpacing },
        children: [tr({ text: text.trim(), size: 20, color: CFG.bodyTextColor })]
      });
    }

    const párrafosDesarrollo = desarrollo.split('\n\n')
      .filter(p => p.trim() !== '')
      .map(p => bodyParagraph(p.trim()));

    // ── 14. ENSAMBLADO ──────────────────────────────────────
    const docChildren = [];

    // Banner
    docChildren.push(bannerHeader);
    docChildren.push(new Paragraph({ spacing: { before: 180, after: 100 } }));

    // Metadatos
    if (incluirMetadatos) {
      docChildren.push(tablaMetadatos);
      docChildren.push(new Paragraph({ spacing: { before: 200, after: 150 } }));
    }

    // Contenido
    if (incluirContenido) {
      if (incluirResumen) {
        docChildren.push(crearBarraSeccion('Resumen Ejecutivo'));
        if (FMT.addQuotes && resumen.trim()) {
          docChildren.push(new Paragraph({ spacing: { before: 120 } }));
          docChildren.push(crearCitaBloque(resumen));
        } else {
          docChildren.push(bodyParagraph(resumen, {
            before: FMT.sectionSpacing.before,
            after: FMT.sectionSpacing.after
          }));
        }
      }

      if (incluirDesarrollo) {
        docChildren.push(crearBarraSeccion('Desarrollo y Análisis'));
        docChildren.push(new Paragraph({ spacing: { before: FMT.sectionSpacing.before } }));
        docChildren.push(...párrafosDesarrollo);
        docChildren.push(new Paragraph({ spacing: { before: 100, after: 100 } }));
      }

      if (incluirKPIs) {
        docChildren.push(crearBarraSeccion('Tabla de Métricas'));
        docChildren.push(new Paragraph({ spacing: { before: FMT.sectionSpacing.before } }));
        docChildren.push(tablaKPIs);
        docChildren.push(new Paragraph({ spacing: { before: 240, after: 100 } }));
      }

      if (incluirConclusiones) {
        docChildren.push(crearBarraSeccion('Conclusiones y Recomendaciones'));
        docChildren.push(bodyParagraph(conclusiones, {
          before: FMT.sectionSpacing.before,
          after: FMT.sectionSpacing.after
        }));
      }

      if (incluirEstado) {
        docChildren.push(tablaEstado);
      }
    }

    // Firmas
    if (bloqueFirmas.length > 0) {
      docChildren.push(...bloqueFirmas);
    }

    // ── 15. Documento final ─────────────────────────────────
    const doc = new Document({
      sections: [{
        properties: {
          page: {
            margin: {
              top: FMT.compact ? 900 : 1150,
              right: 1150,
              bottom: FMT.compact ? 900 : 1150,
              left: 1150
            }
          }
        },
        headers: headerConfig ? { default: headerConfig } : undefined,
        footers: {
          default: new Footer({
            children: [
              new Paragraph({
                alignment: AlignmentType.CENTER,
                children: [
                  tr({ text: 'Página ', size: 18, color: '64748B' }),
                  new TextRun({ children: [PageNumber.CURRENT], size: 18, color: '64748B', font: FMT.font }),
                  tr({ text: ' de ', size: 18, color: '64748B' }),
                  new TextRun({ children: [PageNumber.TOTAL_PAGES], size: 18, color: '64748B', font: FMT.font })
                ]
              })
            ]
          })
        },
        children: docChildren
      }]
    });

    // ── 16. Descarga ────────────────────────────────────────
    const blob = await Packer.toBlob(doc);
    const safeTitle = titulo.trim().replace(/[\\/:*?"<>|]/g, '').replace(/\s+/g, '_');
    const nombreArchivo = `Informe_${safeTitle}_${estiloKey}_${formalidadKey}.docx`;
    window.saveAs(blob, nombreArchivo);

    showToast('Documento generado correctamente.', 'success');

  } catch (err) {
    console.error(err);
    showToast(err.message || 'Ocurrió un error al generar el documento.', 'error');
  } finally {
    btn.disabled = false;
    btn.textContent = originalText;
  }
}


/* ============================================================
   VISTA PREVIA DEL DOCUMENTO
   ============================================================ */

function initPreview() {
  const btnPreview = document.getElementById('btnPreview');
  const btnClose = document.getElementById('btnClosePreview');
  const btnDownload = document.getElementById('btnDownloadFromPreview');
  const modal = document.getElementById('previewModal');
  const overlay = modal.querySelector('.modal-overlay');

  btnPreview.addEventListener('click', () => {
    generarVistaPrevia();
    modal.classList.remove('hidden');
    document.body.style.overflow = 'hidden';
  });

  function closeModal() {
    modal.classList.add('hidden');
    document.body.style.overflow = '';
  }

  btnClose.addEventListener('click', closeModal);
  overlay.addEventListener('click', closeModal);
  document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape' && !modal.classList.contains('hidden')) closeModal();
  });

  btnDownload.addEventListener('click', () => {
    generarWord();
  });
}

function generarVistaPrevia() {
  const estiloKey = document.getElementById('estiloVisual').value;
  const formalidadKey = document.getElementById('nivelFormalidad').value;
  const marcaAgua = document.getElementById('marcaAgua').value;
  const numFirmas = parseInt(document.getElementById('numFirmas').value, 10);

  const incluirMetadatos = document.getElementById('incluirMetadatos').checked;
  const incluirContenido = document.getElementById('incluirContenido').checked;
  const incluirResumen = document.getElementById('incluirResumen').checked;
  const incluirDesarrollo = document.getElementById('incluirDesarrollo').checked;
  const incluirKPIs = document.getElementById('incluirKPIs').checked;
  const incluirConclusiones = document.getElementById('incluirConclusiones').checked;
  const incluirEstado = document.getElementById('incluirEstado').checked;

  const titulo = document.getElementById('tituloInforme').value;
  const subtitulo = document.getElementById('subtituloInforme').value;
  const autor = document.getElementById('autor').value;
  const departamento = document.getElementById('departamento').value;
  const destinatario = document.getElementById('destinatario').value;
  const fechaRaw = document.getElementById('fechaDoc').value;
  const fechaStr = formatDateES(fechaRaw);
  const resumen = document.getElementById('resumenEjecutivo').value;
  const desarrollo = document.getElementById('desarrolloText').value;
  const conclusiones = document.getElementById('conclusionesText').value;
  const estado = document.getElementById('estadoDoc').value;
  const alineacion = document.getElementById('alineacionTexto').value === 'JUSTIFY' ? 'justify' : 'left';

  // Configuraciones
  let CFG = { ...ESTILOS[estiloKey] };
  let FMT = { ...FORMALIDADES[formalidadKey] };

  if (formalidadKey === 'solemne') {
    CFG = {
      ...CFG,
      headerBg: 'FFFFFF', headerTitleColor: '000000', headerSubtitleColor: '333333',
      headerBorderLeft: { style: 'SINGLE', size: 36, color: '000000' },
      secBg: 'FFFFFF', secTextColor: '000000', bodyTextColor: '000000',
      secBorders: { top: 'NONE', bottom: 'SINGLE', left: 'NONE', right: 'NONE', bottomColor: '000000', bottomSize: 12 },
      kpiHeaderBg: '000000', kpiHeaderText: 'FFFFFF',
      metaBg: 'FFFFFF', metaBorder: '000000',
      quoteBg: 'FFFFFF', quoteBorder: '000000',
      estadoColors: { BORRADOR: 'E2E8F0', APROBADO: 'D1FAE5', 'REQUIERE ACCIÓN': 'FEE2E2', CONFIDENCIAL: 'FEF3C7' }
    };
  }

  // Actualizar label de config
  document.getElementById('previewConfigLabel').textContent =
    `${ESTILOS[estiloKey].name} + ${formalidadKey.charAt(0).toUpperCase() + formalidadKey.slice(1)}`;

  // Construir HTML de preview
  let html = '<div style="position:relative;">';

  // Marca de agua
  if (marcaAgua !== 'NINGUNA') {
    html += `<div class="preview-watermark">${marcaAgua}</div>`;
  }

  // Banner
  const bannerBorder = CFG.headerBorderLeft.color
    ? `border-left: ${CFG.headerBorderLeft.size / 8}px solid #${CFG.headerBorderLeft.color};`
    : '';
  html += `
    <div class="preview-banner" style="background:#${CFG.headerBg}; ${bannerBorder}">
      <div class="preview-banner-text">
        <div class="preview-banner-subtitle" style="color:#${CFG.headerSubtitleColor}">${subtitulo.toUpperCase()}</div>
        <h2 class="preview-banner-title" style="color:#${CFG.headerTitleColor}; font-family:${FMT.font}, sans-serif">${titulo}</h2>
      </div>
    </div>
  `;

  // Metadatos
  if (incluirMetadatos) {
    html += `
      <div class="preview-meta">
        <div class="preview-meta-cell" style="background:#${CFG.metaBg}">
          <strong>Elaborado por:</strong> ${autor}<br>
          <strong>Área:</strong> ${departamento}
        </div>
        <div class="preview-meta-cell" style="background:#${CFG.metaBg}">
          <strong>Dirigido a:</strong> ${destinatario}<br>
          <strong>Fecha:</strong> ${fechaStr}
        </div>
      </div>
    `;
  }

  // Contenido
  if (incluirContenido) {
    let sectionNum = 0;
    const romans = ['I', 'II', 'III', 'IV', 'V', 'VI'];

    function secTitle(text) {
      sectionNum++;
      let t = text;
      if (FMT.romanNumbers) t = `${romans[sectionNum - 1] || sectionNum}. ${t}`;
      if (FMT.sectionUppercase) t = t.toUpperCase();
      return t;
    }

    function secBar(text) {
      const b = CFG.secBorders;
      let borderStyle = '';
      if (b.bottom === 'SINGLE') borderStyle += `border-bottom: ${b.bottomSize / 8}px solid #${b.bottomColor};`;
      if (b.left === 'SINGLE') borderStyle += `border-left: ${b.leftSize / 8}px solid #${b.leftColor};`;
      if (b.top === 'SINGLE') borderStyle += `border-top: ${b.topSize / 8}px solid #${b.topColor};`;

      return `<div class="preview-section-bar" style="background:#${CFG.secBg}; color:#${CFG.secTextColor}; ${borderStyle}">${secTitle(text)}</div>`;
    }

    // Resumen
    if (incluirResumen && resumen.trim()) {
      html += secBar('Resumen Ejecutivo');
      if (FMT.addQuotes) {
        html += `<div class="preview-quote" style="background:#${CFG.quoteBg}; border-left: 4px solid #${CFG.quoteBorder}; text-align:${alineacion}; font-family:${FMT.font}, serif">${resumen}</div>`;
      } else {
        html += `<p class="preview-paragraph" style="text-align:${alineacion}; font-family:${FMT.font}, sans-serif; line-height:${FMT.lineSpacing / 200}">${resumen}</p>`;
      }
    }

    // Desarrollo
    if (incluirDesarrollo && desarrollo.trim()) {
      html += secBar('Desarrollo y Análisis');
      const paras = desarrollo.split('\n\n').filter(p => p.trim());
      paras.forEach(p => {
        html += `<p class="preview-paragraph" style="text-align:${alineacion}; font-family:${FMT.font}, sans-serif; line-height:${FMT.lineSpacing / 200}">${p.trim()}</p>`;
      });
    }

    // KPIs
    if (incluirKPIs && kpis.length > 0) {
      html += secBar('Tabla de Métricas');
      html += `
        <table class="preview-table" style="font-family:${FMT.font}, sans-serif">
          <thead>
            <tr style="background:#${CFG.kpiHeaderBg}; color:#${CFG.kpiHeaderText}">
              <th>VARIABLE / INDICADOR</th>
              <th>VALOR / ESTADO</th>
            </tr>
          </thead>
          <tbody>
      `;
      kpis.forEach(kpi => {
        html += `<tr><td>${kpi.label}</td><td><strong>${kpi.value}</strong></td></tr>`;
      });
      html += '</tbody></table>';
    }

    // Conclusiones
    if (incluirConclusiones && conclusiones.trim()) {
      html += secBar('Conclusiones y Recomendaciones');
      html += `<p class="preview-paragraph" style="text-align:${alineacion}; font-family:${FMT.font}, sans-serif; line-height:${FMT.lineSpacing / 200}">${conclusiones}</p>`;
    }

    // Estado
    if (incluirEstado) {
      const estadoBg = CFG.estadoColors[estado] || 'E2E8F0';
      html += `
        <div class="preview-estado" style="font-family:${FMT.font}, sans-serif">
          <div class="preview-estado-label" style="background:#${CFG.kpiHeaderBg}; color:#${CFG.kpiHeaderText}">ESTADO DEL DOCUMENTO</div>
          <div class="preview-estado-value" style="background:#${estadoBg}; color:#${CFG.bodyTextColor}">${estado}</div>
        </div>
      `;
    }
  }

  // Firmas
  if (numFirmas > 0) {
    html += '<div class="preview-firmas">';
    for (let i = 0; i < numFirmas; i++) {
      html += `
        <div class="preview-firma" style="font-family:${FMT.font}, sans-serif">
          <div class="preview-firma-nombre">${i === 0 ? autor : destinatario}</div>
          <div class="preview-firma-cargo">${i === 0 ? departamento : 'Revisado / Aprobado'}</div>
        </div>
      `;
    }
    html += '</div>';
  }

  html += '</div>';

  document.getElementById('previewDocument').innerHTML = html;
}

// Inicializar vista previa al cargar
initPreview();

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/generador-documento/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Generador Universal de Informes</title>
  <link rel="stylesheet" href="styles.css">
  <script src="https://unpkg.com/docx@7.1.0/build/index.js"></script>
  <script src="https://unpkg.com/file-saver@2.0.5/dist/FileSaver.min.js"></script>
</head>
<body>
  <div class="container">
    <header class="app-header">
      <h1>📄 Generador Universal de Informes</h1>
      <p class="subtitle">Crea documentos .docx profesionales con estilo propio</p>
    </header>

    <div id="toastBox" class="toast-box"></div>

    <form id="reportForm" novalidate>
      <!-- CONFIGURACIÓN VISUAL -->
      <fieldset>
        <legend>🎨 Configuración Visual</legend>
        <div class="form-row">
          <div class="col">
            <label for="estiloVisual">Estilo Visual</label>
            <select id="estiloVisual" required>
              <option value="moderno" selected>⚡ Moderno (Azul corporativo)</option>
              <option value="corporativo">🌑 Corporativo Oscuro (Navy + Dorado)</option>
              <option value="ecologico">🌿 Ecológico (Verdes)</option>
              <option value="tecnologico">🚀 Tecnológico (Dark + Cian)</option>
              <option value="clasico">📜 Clásico Papel (Cálido)</option>
            </select>
            <div id="stylePreview" class="style-preview moderno">Moderno</div>
          </div>
          <div class="col">
            <label for="nivelFormalidad">Nivel de Formalidad</label>
            <select id="nivelFormalidad" required>
              <option value="estandar" selected>📋 Estándar</option>
              <option value="interno">💼 Interno / Operativo</option>
              <option value="ejecutivo">👔 Ejecutivo</option>
              <option value="solemne">⚖️ Solemne / Legal</option>
            </select>
            <span class="hint" id="formalityHint">Fuente Calibri, interlineado normal</span>
          </div>
        </div>

        <div class="form-row">
          <div class="col">
            <label for="alineacionTexto">Alineación del cuerpo</label>
            <select id="alineacionTexto">
              <option value="LEFT">Izquierda</option>
              <option value="JUSTIFY" selected>Justificado</option>
            </select>
          </div>
          <div class="col">
            <label for="marcaAgua">Marca de Agua</label>
            <select id="marcaAgua">
              <option value="NINGUNA" selected>Ninguna</option>
              <option value="BORRADOR">BORRADOR</option>
              <option value="CONFIDENCIAL">CONFIDENCIAL</option>
              <option value="PRELIMINAR">PRELIMINAR</option>
            </select>
          </div>
        </div>

        <div class="form-row">
          <div class="col">
            <label for="headerImage">Imagen de Encabezado (PNG/JPG, máx 5 MB)</label>
            <input type="file" id="headerImage" accept="image/png,image/jpeg">
            <span class="hint" id="imageHint">Ninguna imagen seleccionada.</span>
          </div>
          <div class="col">
            <label for="numFirmas">Bloque de Firmas</label>
            <select id="numFirmas">
              <option value="0">Sin firmas</option>
              <option value="1">1 firma</option>
              <option value="2" selected>2 firmas</option>
            </select>
          </div>
        </div>
      </fieldset>

      <!-- METADATOS -->
      <fieldset>
        <legend class="legend-with-check">
          <label class="checkbox-label inline">
            <input type="checkbox" id="incluirMetadatos" checked>
            <span>📇 Metadatos del Documento</span>
          </label>
        </legend>
        <div class="form-row">
          <div class="col">
            <label for="tituloInforme">Título del Informe *</label>
            <input type="text" id="tituloInforme" value="Informe de Desempeño Q3 2026" placeholder="Ej: Informe de Desempeño Q3" required>
          </div>
          <div class="col">
            <label for="subtituloInforme">Subtítulo / Área *</label>
            <input type="text" id="subtituloInforme" value="Departamento de Operaciones" placeholder="Ej: Departamento de Operaciones" required>
          </div>
        </div>
        <div class="form-row">
          <div class="col">
            <label for="autor">Elaborado por *</label>
            <input type="text" id="autor" value="Ing. Fabio Rodríguez" placeholder="Nombre completo" required>
          </div>
          <div class="col">
            <label for="departamento">Área / Departamento</label>
            <input type="text" id="departamento" value="Operaciones" placeholder="Ej: Operaciones">
          </div>
        </div>
        <div class="form-row">
          <div class="col">
            <label for="destinatario">Dirigido a</label>
            <input type="text" id="destinatario" value="Dra. María González" placeholder="Ej: Dirección General">
          </div>
          <div class="col">
            <label for="fechaDoc">Fecha del Documento</label>
            <input type="date" id="fechaDoc" required>
          </div>
        </div>
      </fieldset>

      <!-- CONTENIDO -->
      <fieldset>
        <legend class="legend-with-check">
          <label class="checkbox-label inline">
            <input type="checkbox" id="incluirContenido" checked>
            <span>📝 Contenido del Informe</span>
          </label>
        </legend>

        <div class="sub-checks" id="subChecksContent">
          <label class="checkbox-label small">
            <input type="checkbox" id="incluirResumen" checked>
            <span>Resumen Ejecutivo</span>
          </label>
          <label class="checkbox-label small">
            <input type="checkbox" id="incluirDesarrollo" checked>
            <span>Desarrollo y Análisis</span>
          </label>
          <label class="checkbox-label small">
            <input type="checkbox" id="incluirKPIs" checked>
            <span>Tabla de Métricas</span>
          </label>
          <label class="checkbox-label small">
            <input type="checkbox" id="incluirConclusiones" checked>
            <span>Conclusiones</span>
          </label>
          <label class="checkbox-label small">
            <input type="checkbox" id="incluirEstado" checked>
            <span>Estado del Documento</span>
          </label>
        </div>

        <div id="resumenGroup" class="form-group">
          <label for="resumenEjecutivo">Resumen Ejecutivo / Introducción</label>
          <textarea id="resumenEjecutivo" rows="4" placeholder="Breve síntesis del informe...">El presente informe detalla el desempeño operativo del tercer trimestre de 2026. Se observa una mejora del 12% en tiempos de respuesta y una reducción del 8% en costos operativos. Los indicadores clave muestran tendencia positiva en todas las áreas evaluadas, con especial énfasis en la satisfacción del cliente que alcanzó un 4.7 sobre 5.0.</textarea>
        </div>

        <div id="desarrolloGroup" class="form-group">
          <label for="desarrolloText">Desarrollo / Hallazgos Principales</label>
          <textarea id="desarrolloText" rows="6" placeholder="Desarrolla los hallazgos. Usa doble salto de línea para separar párrafos.">Durante el período evaluado, el equipo de operaciones implementó tres iniciativas clave que impactaron directamente en la eficiencia del departamento. La primera de ellas consistió en la reestructuración de los turnos de trabajo, logrando una cobertura más uniforme durante las horas pico y reduciendo los tiempos de espera en un 18%.

En segundo lugar, se completó la migración del 85% de los procesos manuales hacia flujos automatizados. Esta transición redujo significativamente los errores humanos y aceleró los tiempos de procesamiento en un promedio de 23 minutos por solicitud, liberando al equipo para tareas de mayor valor agregado.

Finalmente, el programa de capacitación continua logró que el 94% del personal obtuviera certificaciones en herramientas de gestión de proyectos, fortaleciendo las competencias técnicas del equipo y mejorando la calidad de las entregas en un 15% respecto al trimestre anterior.</textarea>
        </div>

        <div id="kpisGroup" class="form-group">
          <label>Tabla de Métricas / Indicadores Clave</label>
          <div id="kpiContainer"></div>
          <button type="button" id="btnAddKpi" class="btn-secondary">+ Agregar métrica</button>
        </div>

        <div id="conclusionesGroup" class="form-group">
          <label for="conclusionesText">Conclusiones y Siguientes Pasos</label>
          <textarea id="conclusionesText" rows="4" placeholder="Conclusiones y recomendaciones...">Se recomienda continuar con la inversión en automatización y ampliar el programa de capacitación al resto de las áreas de la organización. Los resultados obtenidos respaldan la estrategia actual y proyectan un crecimiento sostenido del 10% para el próximo trimestre. Se sugiere además la implementación de un sistema de monitoreo en tiempo real para mantener la tendencia positiva.</textarea>
        </div>

        <div id="estadoGroup" class="form-group">
          <label for="estadoDoc">Estado / Clasificación Final</label>
          <select id="estadoDoc">
            <option value="BORRADOR">BORRADOR</option>
            <option value="APROBADO" selected>APROBADO</option>
            <option value="REQUIERE ACCIÓN">REQUIERE ACCIÓN</option>
            <option value="CONFIDENCIAL">CONFIDENCIAL</option>
          </select>
        </div>
      </fieldset>

      <div class="btn-group">
        <button type="button" id="btnPreview" class="btn-preview">👁️ Vista Previa</button>
        <button type="submit" id="btnExport">📥 Generar Documento Word</button>
      </div>
    </form>
  </div>

  <!-- MODAL VISTA PREVIA -->
  <div id="previewModal" class="modal hidden">
    <div class="modal-overlay"></div>
    <div class="modal-content">
      <div class="modal-header">
        <h3>👁️ Vista Previa del Documento</h3>
        <button type="button" class="modal-close" id="btnClosePreview">&times;</button>
      </div>
      <div class="modal-body">
        <div class="preview-config-bar">
          <span id="previewConfigLabel">Moderno + Estándar</span>
          <button type="button" id="btnDownloadFromPreview" class="btn-secondary-small">📥 Descargar .docx</button>
        </div>
        <div id="previewDocument" class="preview-document">
          <!-- Aquí se inyecta la vista previa -->
        </div>
      </div>
    </div>
  </div>

  <script src="app.js"></script>
</body>
</html>

```

### Archivo: `styles.css`
```css
/* ============================================================
   GENERADOR UNIVERSAL DE INFORMES — ESTILOS V2
   ============================================================ */

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  background: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%);
  color: #1e293b;
  padding: 30px 15px;
  min-height: 100vh;
}

.container {
  max-width: 860px;
  margin: 0 auto;
  background: #ffffff;
  padding: 40px;
  border-radius: 14px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.08);
}

/* Header de la app */
.app-header {
  text-align: center;
  margin-bottom: 28px;
  padding-bottom: 20px;
  border-bottom: 2px solid #e2e8f0;
}

.app-header h1 {
  color: #0f172a;
  font-size: 28px;
  font-weight: 800;
  margin-bottom: 6px;
  letter-spacing: -0.5px;
}

.app-header .subtitle {
  color: #64748b;
  font-size: 15px;
  font-weight: 500;
}

/* Toast */
.toast-box {
  min-height: 0;
  margin-bottom: 0;
  transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
  overflow: hidden;
}
.toast-box.visible {
  min-height: 52px;
  margin-bottom: 20px;
}
.toast {
  padding: 14px 18px;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 12px;
  animation: slideDown 0.3s ease;
}
@keyframes slideDown {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}
.toast.error {
  background-color: #fee2e2;
  color: #991b1b;
  border: 1px solid #fecaca;
}
.toast.success {
  background-color: #d1fae5;
  color: #065f46;
  border: 1px solid #a7f3d0;
}
.toast.info {
  background-color: #e0f2fe;
  color: #075985;
  border: 1px solid #bae6fd;
}

/* Fieldsets y formularios */
fieldset {
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  padding: 24px;
  margin-bottom: 24px;
  background-color: #f8fafc;
  transition: border-color 0.2s ease;
}
fieldset:focus-within {
  border-color: #0284c7;
}

legend {
  font-weight: 700;
  color: #0284c7;
  padding: 0 10px;
  font-size: 14px;
}

.legend-with-check {
  display: flex;
  align-items: center;
  gap: 6px;
}

.form-group {
  margin-bottom: 18px;
}

.form-row {
  display: flex;
  gap: 18px;
  margin-bottom: 14px;
  align-items: flex-start;
}

.col {
  flex: 1;
  min-width: 0;
}

label {
  display: block;
  font-weight: 600;
  font-size: 13px;
  margin-bottom: 7px;
  color: #334155;
}

input[type="text"],
input[type="date"],
input[type="file"],
select,
textarea {
  width: 100%;
  padding: 10px 14px;
  border: 1.5px solid #cbd5e1;
  border-radius: 8px;
  font-size: 14px;
  background-color: #ffffff;
  font-family: inherit;
  transition: border-color 0.2s, box-shadow 0.2s;
}

input:focus, select:focus, textarea:focus {
  outline: none;
  border-color: #0284c7;
  box-shadow: 0 0 0 4px rgba(2, 132, 199, 0.12);
}

input:invalid:not(:placeholder-shown) {
  border-color: #ef4444;
}

.hint {
  display: block;
  font-size: 12px;
  color: #64748b;
  margin-top: 5px;
}

/* Preview de estilo */
.style-preview {
  margin-top: 10px;
  padding: 10px 14px;
  border-radius: 8px;
  font-size: 12px;
  font-weight: 600;
  text-align: center;
  transition: all 0.3s ease;
  border: 2px dashed transparent;
}
.style-preview.moderno {
  background: #f1f5f9;
  color: #0f172a;
  border-color: #0284c7;
}
.style-preview.corporativo {
  background: #1e3a5f;
  color: #ffffff;
  border-color: #c9a227;
}
.style-preview.ecologico {
  background: #f0fdf4;
  color: #14532d;
  border-color: #16a34a;
}
.style-preview.tecnologico {
  background: #0f172a;
  color: #38bdf8;
  border-color: #0ea5e9;
}
.style-preview.clasico {
  background: #fffbeb;
  color: #78350f;
  border-color: #92400e;
}

/* Checkboxes personalizados */
.checkbox-label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-weight: 600;
  font-size: 13px;
  color: #334155;
  margin-bottom: 0;
}

.checkbox-label.inline {
  font-size: 14px;
  color: #0284c7;
}

.checkbox-label.small {
  font-weight: 500;
  font-size: 12px;
  color: #475569;
}

.checkbox-label input[type="checkbox"] {
  width: 18px;
  height: 18px;
  accent-color: #0284c7;
  cursor: pointer;
  flex-shrink: 0;
}

.sub-checks {
  display: flex;
  flex-wrap: wrap;
  gap: 12px 20px;
  margin-bottom: 20px;
  padding: 12px 14px;
  background: #ffffff;
  border: 1.5px dashed #cbd5e1;
  border-radius: 8px;
  transition: opacity 0.3s ease;
}
.sub-checks.disabled {
  opacity: 0.4;
  pointer-events: none;
}

/* Botones */
button[type="submit"] {
  width: 100%;
  background: linear-gradient(135deg, #0284c7 0%, #0369a1 100%);
  color: white;
  padding: 16px;
  border: none;
  border-radius: 10px;
  font-size: 17px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 4px 14px rgba(2, 132, 199, 0.3);
}

button[type="submit"]:hover:not(:disabled) {
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(2, 132, 199, 0.4);
}

button[type="submit"]:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

.btn-secondary {
  width: auto;
  background-color: #f1f5f9;
  color: #0284c7;
  border: 1.5px solid #cbd5e1;
  padding: 9px 16px;
  font-size: 13px;
  font-weight: 600;
  border-radius: 8px;
  margin-top: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-secondary:hover:not(:disabled) {
  background-color: #e2e8f0;
  border-color: #94a3b8;
}

/* KPIs dinámicos */
.kpi-row {
  display: flex;
  gap: 10px;
  align-items: center;
  margin-bottom: 10px;
}

.kpi-row input {
  flex: 1;
}

.kpi-row .btn-remove {
  width: 34px;
  height: 34px;
  padding: 0;
  background-color: #fee2e2;
  color: #991b1b;
  border: 1.5px solid #fecaca;
  border-radius: 8px;
  font-size: 18px;
  line-height: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  cursor: pointer;
  transition: all 0.15s ease;
}

.kpi-row .btn-remove:hover:not(:disabled) {
  background-color: #fecaca;
  transform: scale(1.05);
}

/* Responsive */
@media (max-width: 640px) {
  .container {
    padding: 24px 18px;
    border-radius: 10px;
  }
  .app-header h1 {
    font-size: 22px;
  }
  .form-row {
    flex-direction: column;
    gap: 0;
  }
  .kpi-row {
    flex-direction: column;
    align-items: stretch;
  }
  .kpi-row .btn-remove {
    width: 100%;
    height: 38px;
  }
  .sub-checks {
    flex-direction: column;
    gap: 10px;
  }
  fieldset {
    padding: 18px;
  }
}


/* ============================================================
   BOTONES Y GRUPO
   ============================================================ */
.btn-group {
  display: flex;
  gap: 12px;
}

.btn-group button {
  flex: 1;
}

.btn-preview {
  width: 100%;
  background: linear-gradient(135deg, #f1f5f9 0%, #e2e8f0 100%);
  color: #334155;
  padding: 16px;
  border: 2px solid #cbd5e1;
  border-radius: 10px;
  font-size: 17px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-preview:hover:not(:disabled) {
  background: linear-gradient(135deg, #e2e8f0 0%, #cbd5e1 100%);
  border-color: #94a3b8;
  transform: translateY(-1px);
}

.btn-secondary-small {
  background-color: #0284c7;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 6px;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.btn-secondary-small:hover {
  background-color: #0369a1;
}

/* ============================================================
   MODAL VISTA PREVIA
   ============================================================ */
.modal {
  position: fixed;
  inset: 0;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal.hidden {
  display: none;
}

.modal-overlay {
  position: absolute;
  inset: 0;
  background: rgba(15, 23, 42, 0.6);
  backdrop-filter: blur(4px);
}

.modal-content {
  position: relative;
  background: #ffffff;
  border-radius: 14px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.25);
  width: 92vw;
  max-width: 900px;
  max-height: 90vh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  animation: modalIn 0.25s ease;
}

@keyframes modalIn {
  from { opacity: 0; transform: scale(0.96) translateY(10px); }
  to { opacity: 1; transform: scale(1) translateY(0); }
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 18px 24px;
  border-bottom: 1px solid #e2e8f0;
  background: #f8fafc;
}

.modal-header h3 {
  font-size: 17px;
  font-weight: 700;
  color: #0f172a;
  margin: 0;
}

.modal-close {
  width: 36px;
  height: 36px;
  background: #fee2e2;
  color: #991b1b;
  border: none;
  border-radius: 8px;
  font-size: 22px;
  line-height: 1;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.15s ease;
}

.modal-close:hover {
  background: #fecaca;
}

.modal-body {
  flex: 1;
  overflow-y: auto;
  padding: 0;
}

.preview-config-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 24px;
  background: #f1f5f9;
  border-bottom: 1px solid #e2e8f0;
  position: sticky;
  top: 0;
  z-index: 10;
}

.preview-config-bar span {
  font-size: 13px;
  font-weight: 600;
  color: #475569;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

/* ============================================================
   DOCUMENTO DE VISTA PREVIA (simula hoja A4)
   ============================================================ */
.preview-document {
  padding: 48px 56px;
  min-height: 600px;
  background: #ffffff;
}

/* --- Banner --- */
.preview-banner {
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 24px 28px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.preview-banner-text {
  flex: 1;
}

.preview-banner-subtitle {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 6px;
}

.preview-banner-title {
  font-size: 22px;
  font-weight: 800;
  line-height: 1.2;
  margin: 0;
}

.preview-banner-img {
  width: 100px;
  height: 60px;
  object-fit: contain;
  border-radius: 4px;
}

/* --- Metadatos --- */
.preview-meta {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1px;
  background: #e2e8f0;
  border-radius: 6px;
  overflow: hidden;
  margin-bottom: 24px;
}

.preview-meta-cell {
  background: #f8fafc;
  padding: 14px 18px;
  font-size: 13px;
}

.preview-meta-cell strong {
  color: #334155;
  font-weight: 600;
}

/* --- Barra de sección --- */
.preview-section-bar {
  padding: 12px 18px;
  border-radius: 6px;
  margin: 28px 0 16px 0;
  font-size: 13px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.8px;
}

/* --- Cita en bloque (ejecutivo) --- */
.preview-quote {
  padding: 18px 24px;
  border-radius: 6px;
  margin: 16px 0 20px 0;
  font-style: italic;
  line-height: 1.7;
}

/* --- Párrafos --- */
.preview-paragraph {
  font-size: 14px;
  line-height: 1.7;
  margin-bottom: 14px;
  color: #1e293b;
}

/* --- Tabla KPIs --- */
.preview-table {
  width: 100%;
  border-collapse: collapse;
  margin: 16px 0 24px 0;
  font-size: 13px;
  border-radius: 6px;
  overflow: hidden;
}

.preview-table thead th {
  padding: 12px 16px;
  text-align: center;
  font-weight: 700;
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.preview-table tbody td {
  padding: 12px 16px;
  border-bottom: 1px solid #e2e8f0;
}

.preview-table tbody td:last-child {
  text-align: center;
  font-weight: 700;
}

/* --- Tabla Estado --- */
.preview-estado {
  display: grid;
  grid-template-columns: 2fr 3fr;
  gap: 1px;
  background: #e2e8f0;
  border-radius: 6px;
  overflow: hidden;
  margin: 20px 0;
}

.preview-estado-label {
  padding: 14px 18px;
  text-align: center;
  font-weight: 700;
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.preview-estado-value {
  padding: 14px 18px;
  text-align: center;
  font-weight: 700;
  font-size: 14px;
}

/* --- Firmas --- */
.preview-firmas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 32px;
  margin-top: 48px;
  padding-top: 24px;
}

.preview-firma {
  text-align: center;
  padding-top: 16px;
  border-top: 2px solid #000000;
}

.preview-firma-nombre {
  font-weight: 700;
  font-size: 14px;
  margin-bottom: 4px;
}

.preview-firma-cargo {
  font-size: 12px;
  color: #64748b;
}

/* --- Marca de agua --- */
.preview-watermark {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(-45deg);
  font-size: 72px;
  font-weight: 900;
  color: rgba(160, 160, 160, 0.12);
  text-transform: uppercase;
  letter-spacing: 8px;
  pointer-events: none;
  white-space: nowrap;
  z-index: 0;
}

/* Responsive modal */
@media (max-width: 640px) {
  .modal-content {
    width: 96vw;
    max-height: 92vh;
    border-radius: 10px;
  }
  .preview-document {
    padding: 28px 20px;
  }
  .preview-meta {
    grid-template-columns: 1fr;
  }
  .preview-estado {
    grid-template-columns: 1fr;
  }
  .preview-firmas {
    grid-template-columns: 1fr;
  }
  .btn-group {
    flex-direction: column;
  }
}

```

