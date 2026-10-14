# Bundle del Proyecto para IA: descargaVideo

## Estructura de Archivos
- README.md
- documentacion/LEEME.md
- gestor_imagenes (4).html
- index.html
- index2.html
- index3.html

---

## Código Fuente

### Archivo: `README.md`
```md
# 📥 Generador Amigable yt-dlp

Una interfaz web ligera, intuitiva y 100% *front-end* para construir comandos complejos de `yt-dlp` en Windows sin complicaciones.

Permite configurar descargas de video, audio y fraccionamiento por tramos en cuestión de segundos, sin necesidad de recordar parámetros de consola ni enviar datos a servidores externos.

---

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/descargaVideo/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/descargaVideo/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## 🚀 Características

* **Interfaz Limpia y Oscura:** Diseño moderno, responsivo y adaptado para un trabajo rápido.
* **Pegado Directo:** Botón para pegar la URL del portapapeles con un solo clic.
* **Descarga Completa o por Tramos:** Opción para fraccionar videos largos en partes personalizadas (ideal para conferencias, cursos o podcasts).
* **Formatos y Calidades Ajustables:** Selección simple entre MP4, MKV, WEBM, MP3 y M4A, con ajuste de resolución (4K, 1080p, 720p, etc.).
* **Ruta Predeterminada Práctica:** Salida configurada directamente hacia la carpeta `%USERPROFILE%\Downloads` usando el título original del video.
* **Privacidad Total:** Funciona de forma totalmente local en el navegador (sin backend ni rastreo).

---

## 📌 Requisitos Previos (Windows 11)

Para ejecutar los comandos generados en tu consola (CMD o PowerShell), necesitas tener instalados **yt-dlp** y **FFmpeg**.

### Instalación Rápida con `winget` (Recomendado)

Abre PowerShell o CMD y ejecuta:

```powershell
winget install yt-dlp.yt-dlp
winget install Gyan.FFmpeg# descargaVideo

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/descargaVideo/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `gestor_imagenes (4).html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestor de Imágenes</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <style>
        .preview-canvas { max-width: 100%; height: auto; }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen p-6 font-sans">
    <div class="max-w-6xl mx-auto">
        <header class="mb-8 border-b border-slate-800 pb-4 flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
            <div>
                <h1 class="text-3xl font-bold tracking-tight text-amber-400">Gestor de Imágenes</h1>
                <p class="text-slate-400 text-sm mt-1">Optimizador con control de compresión, metadatos y subida a Cloudinary</p>
            </div>
            <div class="flex flex-wrap items-center gap-4 bg-slate-800 p-3 rounded-lg border border-slate-700">
                <div class="flex items-center gap-2">
                    <label for="globalCategory" class="text-sm font-medium text-slate-300">Tipo:</label>
                    <select id="globalCategory" class="bg-slate-900 border border-slate-600 rounded px-2 py-1 text-sm text-amber-300 focus:outline-none focus:border-amber-500">
                        <option value="instalaciones">Instalaciones (Personalizada)</option>
                        <option value="galeria">Galería (Genérica)</option>
                    </select>
                </div>
                <div class="flex items-center gap-2 border-l border-slate-700 pl-3">
                    <input type="checkbox" id="globalKeepOriginal" checked class="w-4 h-4 accent-amber-500 cursor-pointer">
                    <label for="globalKeepOriginal" class="text-sm text-slate-300 cursor-pointer">Respetar nombre original</label>
                </div>
                <div class="flex items-center gap-2 border-l border-slate-700 pl-3">
                    <label for="globalQuality" class="text-sm text-slate-300">Calidad (<span id="qualityVal">80</span>%):</label>
                    <input type="range" id="globalQuality" min="40" max="100" value="80" class="accent-amber-500 cursor-pointer w-24">
                </div>
            </div>
        </header>

        <!-- Configuración de Proyecto (Cloudinary) -->
        <div class="mb-8 bg-slate-800 border border-slate-700 rounded-xl p-5">
            <h2 class="text-sm font-bold text-sky-400 uppercase tracking-wide mb-3">Proyecto / destino en Cloudinary</h2>
            <div class="flex flex-col md:flex-row md:items-end gap-3 flex-wrap">
                <div class="flex-1 min-w-[170px]">
                    <label class="text-xs text-slate-400 block mb-1">Proyecto guardado</label>
                    <select id="projectSelect" class="w-full bg-slate-900 border border-slate-600 rounded px-2 py-1.5 text-sm text-slate-200 focus:outline-none focus:border-sky-500">
                        <option value="">— Nuevo / sin guardar —</option>
                    </select>
                </div>
                <div class="flex-1 min-w-[140px]">
                    <label class="text-xs text-slate-400 block mb-1">Nombre del proyecto</label>
                    <input type="text" id="projName" placeholder="Ej: Monarca" class="w-full bg-slate-900 border border-slate-600 rounded px-2 py-1.5 text-sm text-slate-200 focus:outline-none focus:border-sky-500">
                </div>
                <div class="flex-1 min-w-[140px]">
                    <label class="text-xs text-slate-400 block mb-1">Cloud name</label>
                    <input type="text" id="projCloudName" placeholder="Ej: p0qlmlor" class="w-full bg-slate-900 border border-slate-600 rounded px-2 py-1.5 text-sm text-slate-200 focus:outline-none focus:border-sky-500">
                </div>
                <div class="flex-1 min-w-[140px]">
                    <label class="text-xs text-slate-400 block mb-1">Upload preset (unsigned)</label>
                    <input type="text" id="projUploadPreset" placeholder="Ej: monarca_unsigned" class="w-full bg-slate-900 border border-slate-600 rounded px-2 py-1.5 text-sm text-slate-200 focus:outline-none focus:border-sky-500">
                </div>
                <div class="flex-1 min-w-[140px]">
                    <label class="text-xs text-slate-400 block mb-1">Carpeta base en Cloudinary</label>
                    <input type="text" id="projFolder" placeholder="Ej: monarca" class="w-full bg-slate-900 border border-slate-600 rounded px-2 py-1.5 text-sm text-slate-200 focus:outline-none focus:border-sky-500">
                </div>
                <div class="flex-1 min-w-[140px]">
                    <label class="text-xs text-slate-400 block mb-1">Tag(s) (separados por coma)</label>
                    <input type="text" id="projTags" placeholder="Ej: monarca_galeria" class="w-full bg-slate-900 border border-slate-600 rounded px-2 py-1.5 text-sm text-slate-200 focus:outline-none focus:border-sky-500">
                </div>
                <div class="flex gap-2">
                    <button id="saveProjectBtn" class="bg-emerald-600 hover:bg-emerald-500 text-white text-sm font-semibold px-3 py-2 rounded cursor-pointer">Guardar</button>
                    <button id="deleteProjectBtn" class="bg-rose-600/80 hover:bg-rose-500 text-white text-sm font-semibold px-3 py-2 rounded cursor-pointer">Eliminar</button>
                </div>
            </div>
            <p class="text-xs text-slate-500 mt-3">
                El <strong class="text-slate-400">upload preset</strong> tiene que existir y estar marcado como <strong class="text-slate-400">Unsigned</strong> en el panel de Cloudinary de ese proyecto
                (Settings → Upload → Upload presets). La <strong class="text-slate-400">carpeta base</strong> es opcional: si la completás, cada imagen se sube dentro de
                <code class="text-slate-400">carpeta-base/tipo</code> (por ejemplo <code class="text-slate-400">monarca/galeria</code> o <code class="text-slate-400">monarca/instalaciones</code>), usando el mismo selector "Tipo" de cada tarjeta.
                Esto se configura una sola vez por proyecto; después este mismo panel queda guardado en tu navegador para reusar.
            </p>
        </div>

        <!-- Zona de Carga -->
        <div class="mb-8">
            <label for="fileInput" class="border-2 border-dashed border-slate-700 hover:border-amber-500 bg-slate-800/50 hover:bg-slate-800 rounded-xl p-8 flex flex-col items-center justify-center cursor-pointer transition-all duration-200 group">
                <svg class="w-12 h-12 text-slate-500 group-hover:text-amber-400 mb-3 transition-colors" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
                </svg>
                <span class="text-sm font-medium text-slate-300 group-hover:text-amber-300">Arrastra tus imágenes aquí o <span class="text-amber-400 underline">haz clic para buscar</span></span>
                <span class="text-xs text-slate-500 mt-1">Soporta múltiples archivos PNG, JPG, WEBP</span>
                <input type="file" id="fileInput" multiple accept="image/*" class="hidden">
            </label>
        </div>

        <!-- Acciones Globales -->
        <div id="batchActions" class="hidden mb-6 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3 bg-slate-800 p-4 rounded-lg border border-slate-700">
            <span id="counterText" class="text-sm text-slate-300 font-medium">0 imágenes cargadas</span>
            <div class="flex gap-2">
                <button id="uploadAllBtn" class="bg-sky-600 hover:bg-sky-500 text-white font-bold px-5 py-2 rounded-lg text-sm transition-colors shadow-lg cursor-pointer">
                    Subir todas a Cloudinary
                </button>
                <button id="downloadAllBtn" class="bg-amber-500 hover:bg-amber-600 text-slate-950 font-bold px-5 py-2 rounded-lg text-sm transition-colors shadow-lg cursor-pointer">
                    Descargar todas procesadas (.WebP)
                </button>
            </div>
        </div>

        <!-- Contenedor de Tarjetas -->
        <div id="cardsContainer" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"></div>
    </div>

    <script>
        const fileInput = document.getElementById('fileInput');
        const cardsContainer = document.getElementById('cardsContainer');
        const globalCategory = document.getElementById('globalCategory');
        const globalKeepOriginal = document.getElementById('globalKeepOriginal');
        const globalQuality = document.getElementById('globalQuality');
        const qualityVal = document.getElementById('qualityVal');
        const batchActions = document.getElementById('batchActions');
        const counterText = document.getElementById('counterText');
        const downloadAllBtn = document.getElementById('downloadAllBtn');
        const uploadAllBtn = document.getElementById('uploadAllBtn');

        // --- Proyectos (Cloudinary) ---
        const PROJECTS_KEY = 'gestorImagenes_proyectos_cloudinary_v1';
        const projectSelect = document.getElementById('projectSelect');
        const projName = document.getElementById('projName');
        const projCloudName = document.getElementById('projCloudName');
        const projUploadPreset = document.getElementById('projUploadPreset');
        const projFolder = document.getElementById('projFolder');
        const projTags = document.getElementById('projTags');
        const saveProjectBtn = document.getElementById('saveProjectBtn');
        const deleteProjectBtn = document.getElementById('deleteProjectBtn');

        function loadProjects() {
            try {
                return JSON.parse(localStorage.getItem(PROJECTS_KEY)) || [];
            } catch {
                return [];
            }
        }

        function saveProjects(list) {
            try {
                localStorage.setItem(PROJECTS_KEY, JSON.stringify(list));
            } catch (e) {
                console.error('No se pudo guardar en localStorage:', e);
            }
        }

        function refreshProjectSelect(selectId) {
            const list = loadProjects();
            projectSelect.innerHTML = '<option value="">— Nuevo / sin guardar —</option>' +
                list.map(p => `<option value="${p.id}">${p.nombre}</option>`).join('');
            if (selectId) projectSelect.value = selectId;
        }

        // Proyecto precargado por defecto (hasta definir una forma mejor de gestionarlo)
        const DEFAULT_PROJECT = {
            id: 'default-monarca-galeria',
            nombre: 'Monarca galeria',
            cloudName: 'p0qlmlor',
            uploadPreset: 'subir_gestor',
            folder: 'monarca',
            tags: 'monarca_galeria'
        };

        function ensureDefaultProject() {
            const list = loadProjects();
            if (!list.find(p => p.id === DEFAULT_PROJECT.id)) {
                list.push(DEFAULT_PROJECT);
                saveProjects(list);
            }
        }

        ensureDefaultProject();
        refreshProjectSelect(DEFAULT_PROJECT.id);
        projName.value = DEFAULT_PROJECT.nombre;
        projCloudName.value = DEFAULT_PROJECT.cloudName;
        projUploadPreset.value = DEFAULT_PROJECT.uploadPreset;
        projFolder.value = DEFAULT_PROJECT.folder;
        projTags.value = DEFAULT_PROJECT.tags;

        projectSelect.addEventListener('change', () => {
            const list = loadProjects();
            const p = list.find(x => x.id === projectSelect.value);
            if (p) {
                projName.value = p.nombre;
                projCloudName.value = p.cloudName;
                projUploadPreset.value = p.uploadPreset;
                projFolder.value = p.folder || '';
                projTags.value = p.tags || '';
            } else {
                projName.value = '';
                projCloudName.value = '';
                projUploadPreset.value = '';
                projFolder.value = '';
                projTags.value = '';
            }
        });

        saveProjectBtn.addEventListener('click', () => {
            if (!projName.value.trim() || !projCloudName.value.trim() || !projUploadPreset.value.trim()) {
                alert('Completá al menos Nombre, Cloud name y Upload preset.');
                return;
            }
            const list = loadProjects();
            const existingId = projectSelect.value;
            const id = existingId || Date.now().toString(36);
            const data = {
                id,
                nombre: projName.value.trim(),
                cloudName: projCloudName.value.trim(),
                uploadPreset: projUploadPreset.value.trim(),
                folder: projFolder.value.trim(),
                tags: projTags.value.trim()
            };
            const idx = list.findIndex(x => x.id === id);
            if (idx >= 0) list[idx] = data; else list.push(data);
            saveProjects(list);
            refreshProjectSelect(id);
        });

        deleteProjectBtn.addEventListener('click', () => {
            const id = projectSelect.value;
            if (!id) return;
            if (!confirm('¿Eliminar este proyecto guardado? (no borra nada en Cloudinary, solo la configuración local)')) return;
            const list = loadProjects().filter(x => x.id !== id);
            saveProjects(list);
            projName.value = '';
            projCloudName.value = '';
            projUploadPreset.value = '';
            projFolder.value = '';
            projTags.value = '';
            refreshProjectSelect();
        });

        function getCurrentProjectConfig() {
            const cloudName = projCloudName.value.trim();
            const uploadPreset = projUploadPreset.value.trim();
            const folder = projFolder.value.trim();
            const tags = projTags.value.trim();
            if (!cloudName || !uploadPreset) return null;
            return { cloudName, uploadPreset, folder, tags };
        }

        // --- Imágenes ---
        let imageFiles = [];

        globalQuality.addEventListener('input', (e) => {
            qualityVal.textContent = e.target.value;
        });

        fileInput.addEventListener('change', (e) => {
            const files = Array.from(e.target.files);
            let processedCount = 0;

            files.forEach(file => {
                if (!file.type.startsWith('image/')) return;

                const reader = new FileReader();
                reader.onload = (event) => {
                    const img = new Image();
                    img.onload = () => {
                        const randomStr = Math.random().toString(36);
                        const fileDate = file.lastModified ? new Date(file.lastModified) : new Date();
                        const rawName = file.name.substring(0, file.name.lastIndexOf('.')) || file.name;
                        const keepOrig = globalKeepOriginal.checked;

                        const imgData = {
                            id: randomStr.substring(2, 9),
                            fileObj: file,
                            imgElement: img,
                            originalSize: file.size,
                            timestamp: fileDate.getTime(),
                            keepOriginal: keepOrig,
                            originalName: rawName,
                            customName: 'imagen',
                            category: globalCategory.value,
                            customLegend: '',
                            dateStr: getFormattedDate(fileDate),
                            quality: parseInt(globalQuality.value) / 100
                        };
                        imageFiles.push(imgData);
                        processedCount++;

                        if (processedCount === files.filter(f => f.type.startsWith('image/')).length) {
                            sortImages();
                            renderCards();
                        }
                    };
                    img.src = event.target.result;
                };
                reader.readAsDataURL(file);
            });
            fileInput.value = '';
        });

        globalCategory.addEventListener('change', () => {
            const val = globalCategory.value;
            imageFiles.forEach(item => {
                item.category = val;
                item.customLegend = '';
            });
            renderCards();
        });

        globalKeepOriginal.addEventListener('change', () => {
            const val = globalKeepOriginal.checked;
            imageFiles.forEach(item => {
                item.keepOriginal = val;
            });
            renderCards();
        });

        globalQuality.addEventListener('change', () => {
            const q = parseInt(globalQuality.value) / 100;
            imageFiles.forEach(item => {
                item.quality = q;
            });
            renderCards();
        });

        function getFormattedDate(dateObj) {
            const yyyy = dateObj.getFullYear();
            const mm = String(dateObj.getMonth() + 1).padStart(2, '0');
            const dd = String(dateObj.getDate()).padStart(2, '0');
            const hh = String(dateObj.getHours()).padStart(2, '0');
            const min = String(dateObj.getMinutes()).padStart(2, '0');
            const ss = String(dateObj.getSeconds()).padStart(2, '0');
            return `${yyyy}${mm}${dd}${hh}${min}${ss}`;
        }

        function sortImages() {
            imageFiles.sort((a, b) => b.timestamp - a.timestamp);
        }

        function formatBytes(bytes) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024, dm = 2;
            const sizes = ['Bytes', 'KB', 'MB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + ' ' + sizes[i];
        }

        function getFinalName(item) {
            return item.keepOriginal
                ? item.originalName
                : `${item.dateStr}_${item.customName || 'imagen'}`;
        }

        function renderCards() {
            cardsContainer.innerHTML = '';
            if (imageFiles.length > 0) {
                batchActions.classList.remove('hidden');
                counterText.textContent = `${imageFiles.length} ${imageFiles.length === 1 ? 'imagen cargada' : 'imágenes cargadas'}`;
            } else {
                batchActions.classList.add('hidden');
            }

            imageFiles.forEach((item, index) => {
                const card = document.createElement('div');
                card.className = "bg-slate-800 rounded-xl border border-slate-700 p-4 flex flex-col gap-3 shadow-md";

                const finalGeneratedName = `${getFinalName(item)}.webp`;

                card.innerHTML = `
                    <div class="relative rounded-lg overflow-hidden bg-slate-950 aspect-video flex items-center justify-center border border-slate-700">
                        <canvas id="canvas-${index}" class="preview-canvas object-contain w-full h-full"></canvas>
                    </div>
                    <div class="flex flex-col gap-2 text-sm">
                        <div class="flex justify-between items-center text-xs text-slate-400">
                            <span>Original: <strong class="text-slate-200">${formatBytes(item.originalSize)}</strong></span>
                            <div class="flex items-center gap-1.5">
                                <input type="checkbox" data-index="${index}" class="card-keep-original w-3.5 h-3.5 accent-amber-500 cursor-pointer" ${item.keepOriginal ? 'checked' : ''}>
                                <span class="cursor-pointer">Respetar original</span>
                            </div>
                        </div>

                        ${item.keepOriginal ? `
                            <div class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1.5 text-slate-400 text-xs truncate">
                                ${item.originalName}
                            </div>
                        ` : `
                            <input type="text" value="${item.customName}" data-index="${index}" placeholder="Nombre corto..." class="custom-name-input w-full bg-slate-900 border border-slate-600 rounded px-2 py-1 text-slate-200 text-xs focus:outline-none focus:border-amber-500">
                        `}

                        <div class="grid grid-cols-2 gap-2 mt-1">
                            <div>
                                <label class="text-xs text-slate-400">Tipo:</label>
                                <select data-index="${index}" class="category-select w-full bg-slate-900 border border-slate-600 rounded px-2 py-1 text-slate-200 text-xs focus:outline-none focus:border-amber-500">
                                    <option value="instalaciones" ${item.category === 'instalaciones' ? 'selected' : ''}>Instalaciones</option>
                                    <option value="galeria" ${item.category === 'galeria' ? 'selected' : ''}>Galería</option>
                                </select>
                            </div>
                            <div>
                                <label class="text-xs text-slate-400">Leyenda:</label>
                                <input type="text" value="${item.customLegend}" data-index="${index}" placeholder="Sin leyenda (opcional)" class="legend-input w-full bg-slate-900 border border-slate-600 rounded px-2 py-1 text-slate-200 text-xs focus:outline-none focus:border-amber-500">
                            </div>
                        </div>
                        <div class="text-xs text-amber-400/90 truncate mt-1 bg-slate-900/60 p-1.5 rounded border border-slate-800 flex justify-between items-center">
                            <span class="truncate"><span class="text-slate-500">Salida:</span> ${finalGeneratedName}</span>
                            <span id="size-badge-${index}" class="text-[10px] bg-slate-800 px-1.5 py-0.5 rounded text-slate-300 shrink-0">Calculando...</span>
                        </div>
                    </div>
                    <div class="flex justify-between items-center pt-2 border-t border-slate-700/60 mt-auto gap-2 flex-wrap">
                        <button data-index="${index}" class="delete-btn text-rose-400 hover:text-rose-300 text-xs font-medium cursor-pointer">Eliminar</button>
                        <div class="flex items-center gap-2 flex-wrap justify-end">
                            <span id="upload-status-${index}" class="text-[10px] bg-slate-800 px-1.5 py-0.5 rounded text-slate-400 shrink-0">Sin subir</span>
                            <button data-index="${index}" class="upload-btn bg-sky-600 hover:bg-sky-500 text-white font-bold px-3 py-1.5 rounded text-xs transition-colors cursor-pointer">Subir a Cloudinary</button>
                            <button data-index="${index}" class="download-single-btn bg-amber-500 hover:bg-amber-600 text-slate-950 font-bold px-3 py-1.5 rounded text-xs transition-colors cursor-pointer">Descargar WebP</button>
                        </div>
                    </div>
                `;

                cardsContainer.appendChild(card);
                drawPreviewAndMeasure(index);
            });

            attachEvents();
        }

        // Dibuja el canvas de previsualización (con leyenda) y mide el tamaño resultante
        function drawPreviewAndMeasure(index) {
            const item = imageFiles[index];
            const canvas = document.getElementById(`canvas-${index}`);
            if (!canvas) return;
            const ctx = canvas.getContext('2d');

            const img = item.imgElement;
            canvas.width = img.naturalWidth;
            canvas.height = img.naturalHeight;

            ctx.drawImage(img, 0, 0);
            drawLegend(ctx, canvas, item.customLegend);

            canvas.toBlob((blob) => {
                const badge = document.getElementById(`size-badge-${index}`);
                if (badge) {
                    const newSize = blob.size;
                    const diff = newSize - item.originalSize;
                    const diffPercent = Math.round((diff / item.originalSize) * 100);
                    badge.textContent = `${formatBytes(newSize)} (${diffPercent > 0 ? '+' : ''}${diffPercent}%)`;
                    badge.className = `text-[10px] px-1.5 py-0.5 rounded shrink-0 font-medium ${newSize < item.originalSize ? 'bg-emerald-950 text-emerald-400 border border-emerald-800' : 'bg-amber-950 text-amber-400 border border-amber-800'}`;
                }
            }, 'image/webp', item.quality);
        }

        // Dibuja la caja + texto de la leyenda sobre un canvas ya con la imagen pintada
        function drawLegend(ctx, canvas, legend) {
            if (!legend || legend.trim() === '') return;
            const fontSize = Math.max(16, Math.floor(canvas.height * 0.04));
            ctx.font = `bold ${fontSize}px sans-serif`;
            ctx.fillStyle = "rgba(0, 0, 0, 0.6)";

            const textMetrics = ctx.measureText(legend);
            const padding = fontSize * 0.8;
            const boxWidth = textMetrics.width + (padding * 2);
            const boxHeight = fontSize * 1.6;
            const x = (canvas.width - boxWidth) / 2;
            const y = canvas.height - boxHeight - (fontSize * 0.8);

            ctx.beginPath();
            ctx.roundRect(x, y, boxWidth, boxHeight, 8);
            ctx.fill();

            ctx.fillStyle = "#ffffff";
            ctx.textAlign = "center";
            ctx.textBaseline = "middle";
            ctx.fillText(legend, canvas.width / 2, y + (boxHeight / 2));
        }

        // Genera el blob final (imagen + leyenda quemada) en WebP, para descarga o subida
        function buildProcessedBlob(item) {
            return new Promise((resolve) => {
                const canvas = document.createElement('canvas');
                const ctx = canvas.getContext('2d');
                canvas.width = item.imgElement.naturalWidth;
                canvas.height = item.imgElement.naturalHeight;
                ctx.drawImage(item.imgElement, 0, 0);
                drawLegend(ctx, canvas, item.customLegend);
                canvas.toBlob((blob) => resolve(blob), 'image/webp', item.quality);
            });
        }

        function attachEvents() {
            document.querySelectorAll('.custom-name-input').forEach(input => {
                input.addEventListener('input', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles[idx].customName = e.target.value;
                });
            });

            document.querySelectorAll('.card-keep-original').forEach(chk => {
                chk.addEventListener('change', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles[idx].keepOriginal = e.target.checked;
                    renderCards();
                });
            });

            document.querySelectorAll('.legend-input').forEach(input => {
                input.addEventListener('input', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles[idx].customLegend = e.target.value;
                    drawPreviewAndMeasure(idx);
                });
            });

            document.querySelectorAll('.category-select').forEach(select => {
                select.addEventListener('change', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles[idx].category = e.target.value;
                    renderCards();
                });
            });

            document.querySelectorAll('.delete-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles.splice(idx, 1);
                    renderCards();
                });
            });

            document.querySelectorAll('.download-single-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const idx = e.target.dataset.index;
                    downloadItem(idx);
                });
            });

            document.querySelectorAll('.upload-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const idx = e.target.dataset.index;
                    uploadToCloudinary(idx);
                });
            });

            downloadAllBtn.onclick = () => {
                imageFiles.forEach((_, idx) => {
                    setTimeout(() => downloadItem(idx), idx * 250);
                });
            };

            uploadAllBtn.onclick = () => {
                if (!getCurrentProjectConfig()) {
                    alert('Configurá y guardá un proyecto (Cloud name + Upload preset) antes de subir.');
                    return;
                }
                imageFiles.forEach((_, idx) => {
                    setTimeout(() => uploadToCloudinary(idx), idx * 400);
                });
            };
        }

        async function downloadItem(index) {
            const item = imageFiles[index];
            const blob = await buildProcessedBlob(item);
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `${getFinalName(item)}.webp`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }

        async function uploadToCloudinary(index) {
            const item = imageFiles[index];
            const config = getCurrentProjectConfig();
            const statusEl = document.getElementById(`upload-status-${index}`);

            if (!config) {
                alert('Configurá y guardá un proyecto (Cloud name + Upload preset) arriba antes de subir.');
                return;
            }

            if (statusEl) {
                statusEl.textContent = 'Subiendo…';
                statusEl.className = 'text-[10px] px-1.5 py-0.5 rounded shrink-0 font-medium bg-slate-700 text-slate-300';
            }

            try {
                const blob = await buildProcessedBlob(item);
                const finalName = getFinalName(item);

                const subfolder = item.category; // "instalaciones" o "galeria"
                const folderPath = config.folder ? `${config.folder}/${subfolder}` : subfolder;

                const formData = new FormData();
                formData.append('file', blob, `${finalName}.webp`);
                formData.append('upload_preset', config.uploadPreset);
                formData.append('public_id', finalName);
                formData.append('folder', folderPath);
                if (config.tags) formData.append('tags', config.tags);

                const res = await fetch(`https://api.cloudinary.com/v1_1/${config.cloudName}/image/upload`, {
                    method: 'POST',
                    body: formData
                });
                const data = await res.json();

                if (!res.ok) throw new Error((data.error && data.error.message) || 'Error desconocido de Cloudinary');

                if (statusEl) {
                    statusEl.textContent = `✓ ${folderPath}/`;
                    statusEl.className = 'text-[10px] px-1.5 py-0.5 rounded shrink-0 font-medium bg-emerald-950 text-emerald-400 border border-emerald-800';
                    statusEl.title = data.secure_url;
                }
            } catch (err) {
                console.error('Error subiendo a Cloudinary:', err);
                if (statusEl) {
                    statusEl.textContent = 'Error ✕';
                    statusEl.className = 'text-[10px] px-1.5 py-0.5 rounded shrink-0 font-medium bg-rose-950 text-rose-400 border border-rose-800';
                    statusEl.title = err.message;
                }
            }
        }
    </script>
</body>
</html>

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Multimedia</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <style>
        .preview-canvas { max-width: 100%; height: auto; }
        code {
            background: #11111b;
            color: #a6e3a1;
            padding: 3px 6px;
            border-radius: 4px;
            font-family: monospace;
            font-size: 12px;
            display: inline-block;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen p-4 md:p-6 font-sans">
    <div class="max-w-7xl mx-auto">
        <!-- Encabezado Principal y Pestañas -->
        <header class="mb-6 border-b border-slate-800 pb-4 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
            <div>
                <h1 class="text-3xl font-bold tracking-tight text-amber-400">Dashboard Multimedia</h1>
                <p class="text-slate-400 text-sm mt-1">Gestión integral de imágenes, videos y comandos multimedia</p>
            </div>
            
            <!-- Selector de Secciones -->
            <div class="flex bg-slate-800 p-1.5 rounded-xl border border-slate-700 w-full sm:w-auto">
                <button id="tabImagesBtn" onclick="switchTab('images')" class="flex-1 sm:flex-initial px-5 py-2 rounded-lg text-sm font-semibold transition-all duration-200 bg-amber-500 text-slate-950 shadow-md">
                    📷 Gestor de Imágenes
                </button>
                <button id="tabVideosBtn" onclick="switchTab('videos')" class="flex-1 sm:flex-initial px-5 py-2 rounded-lg text-sm font-semibold transition-all duration-200 text-slate-400 hover:text-slate-200">
                    🎬 yt-dlp & FFmpeg
                </button>
            </div>
        </header>

        <!-- SECCIÓN 1: GESTOR DE IMÁGENES -->
        <section id="sectionImages" class="block">
            <div class="mb-6 flex flex-wrap items-center justify-between gap-4 bg-slate-800/80 p-4 rounded-xl border border-slate-700">
                <div class="flex flex-wrap items-center gap-4">
                    <div class="flex items-center gap-2">
                        <label for="globalCategory" class="text-sm font-medium text-slate-300">Tipo:</label>
                        <select id="globalCategory" class="bg-slate-900 border border-slate-600 rounded px-2.5 py-1.5 text-sm text-amber-300 focus:outline-none focus:border-amber-500">
                            <option value="instalaciones">Instalaciones (Personalizada)</option>
                            <option value="galeria">Galería (Genérica)</option>
                        </select>
                    </div>
                    <div class="flex items-center gap-2 border-l border-slate-700 pl-4">
                        <input type="checkbox" id="globalKeepOriginal" checked class="w-4 h-4 accent-amber-500 cursor-pointer">
                        <label for="globalKeepOriginal" class="text-sm text-slate-300 cursor-pointer">Respetar nombre original</label>
                    </div>
                    <div class="flex items-center gap-2 border-l border-slate-700 pl-4">
                        <label for="globalQuality" class="text-sm text-slate-300">Calidad (<span id="qualityVal">80</span>%):</label>
                        <input type="range" id="globalQuality" min="40" max="100" value="80" class="accent-amber-500 cursor-pointer w-28">
                    </div>
                </div>
            </div>

            <!-- Zona de Carga -->
            <div class="mb-6">
                <label for="fileInput" class="border-2 border-dashed border-slate-700 hover:border-amber-500 bg-slate-800/40 hover:bg-slate-800/80 rounded-xl p-8 flex flex-col items-center justify-center cursor-pointer transition-all duration-200 group">
                    <svg class="w-10 h-10 text-slate-500 group-hover:text-amber-400 mb-2 transition-colors" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
                    </svg>
                    <span class="text-sm font-medium text-slate-300 group-hover:text-amber-300">Arrastra tus imágenes aquí o <span class="text-amber-400 underline">haz clic para buscar</span></span>
                    <span class="text-xs text-slate-500 mt-1">Soporta múltiples archivos PNG, JPG, WEBP</span>
                    <input type="file" id="fileInput" multiple accept="image/*" class="hidden">
                </label>
            </div>

            <!-- Acciones Globales -->
            <div id="batchActions" class="hidden mb-6 flex justify-between items-center bg-slate-800/80 p-4 rounded-xl border border-slate-700">
                <span id="counterText" class="text-sm text-slate-300 font-medium">0 imágenes cargadas</span>
                <button id="downloadAllBtn" class="bg-amber-500 hover:bg-amber-600 text-slate-950 font-bold px-5 py-2 rounded-lg text-sm transition-colors shadow-lg cursor-pointer">
                    Descargar todas procesadas (.WebP)
                </button>
            </div>

            <!-- Contenedor de Tarjetas de Imágenes -->
            <div id="cardsContainer" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"></div>
        </section>

        <!-- SECCIÓN 2: YT-DLP & FFMPEG -->
        <section id="sectionVideos" class="hidden">
            <div class="bg-slate-800/80 rounded-xl border border-slate-700 p-6 shadow-xl max-w-4xl mx-auto flex flex-col gap-5">
                
                <!-- Guía rápida desplegable -->
                <details class="bg-slate-900/80 border border-amber-500/30 rounded-lg p-4 text-xs text-slate-300">
                    <summary class="font-bold text-amber-400 cursor-pointer text-sm mb-1">📌 Requisitos e Instalación Rápida en Windows (Clic para desplegar)</summary>
                    <p class="mt-2 text-slate-300">Para ejecutar los comandos generados, necesitas tener instalados <code>yt-dlp</code> y <code>ffmpeg</code> en tu consola:</p>
                    <ul class="list-disc ml-5 mt-2 space-y-1">
                        <li><b>Opción winget (Recomendado):</b> <code>winget install yt-dlp.yt-dlp</code> | <code>winget install Gyan.FFmpeg</code></li>
                        <li><b>Descarga PowerShell:</b> <code>Invoke-WebRequest -Uri "https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp.exe" -OutFile "yt-dlp.exe"</code></li>
                    </ul>
                </details>

                <!-- URL e Inserción -->
                <div id="downloadSection">
                    <label for="url" class="block text-xs font-semibold text-slate-400 mb-1">1. URL del Video (YouTube, Instagram, TikTok, Vimeo, etc.):</label>
                    <div class="flex gap-2">
                        <input type="text" id="url" placeholder="https://www.youtube.com/watch?v=..." oninput="generateCommand()" class="flex-grow bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-slate-200 focus:outline-none focus:border-amber-500">
                        <button type="button" onclick="pasteClipboard()" class="bg-sky-600 hover:bg-sky-500 text-white font-bold px-4 py-2 rounded-lg text-xs transition-colors whitespace-nowrap cursor-pointer">📋 Pegar URL</button>
                    </div>
                </div>

                <!-- Formato y Calidad -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label for="ext" class="block text-xs font-semibold text-slate-400 mb-1">2. Acción / Formato Final de Salida:</label>
                        <select id="ext" onchange="toggleFormatOptions(); generateCommand();" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-slate-200 focus:outline-none focus:border-amber-500">
                            <option value="mp4" selected>Video MP4 (Compatible con todo)</option>
                            <option value="mkv">Video MKV (Máxima fidelidad)</option>
                            <option value="webm">Video WEBM (Nativo de YouTube)</option>
                            <option value="mp3">Solo Audio MP3 (Música / Podcast)</option>
                            <option value="m4a">Solo Audio M4A (Sin pérdida / Original)</option>
                            <option value="merge_audio">🎵 Insertar/Mezclar Audio local con Video</option>
                        </select>
                    </div>
                    <div id="qualityBox">
                        <label for="quality" class="block text-xs font-semibold text-slate-400 mb-1">3. Calidad de Video Deseada:</label>
                        <select id="quality" onchange="generateCommand()" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-slate-200 focus:outline-none focus:border-amber-500">
                            <option value="best">Máxima Calidad Disponible (4K/2K/1080p)</option>
                            <option value="1080" selected>Full HD (1080p) - Recomendado</option>
                            <option value="720">HD (720p) - Descarga Rápida</option>
                            <option value="480">Estándar (480p) - Poco peso</option>
                        </select>
                    </div>
                </div>

                <!-- Panel FFmpeg Audio Local -->
                <div id="audioMergeBox" class="hidden border border-dashed border-sky-500/50 p-4 rounded-xl bg-slate-900/60 flex flex-col gap-3">
                    <h3 class="text-xs font-bold text-amber-400 uppercase tracking-wider">Configuración de Ensamblado de Audio (FFmpeg)</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                        <div>
                            <label for="localVideo" class="block text-xs text-slate-400 mb-1">Video Base:</label>
                            <input type="text" id="localVideo" value="trivia_01.mp4" oninput="generateCommand()" class="w-full bg-slate-900 border border-slate-700 rounded px-2.5 py-1.5 text-xs text-slate-200">
                        </div>
                        <div>
                            <label for="localAudio" class="block text-xs text-slate-400 mb-1">Audio (MP3/WAV):</label>
                            <input type="text" id="localAudio" value="musica.mp3" oninput="generateCommand()" class="w-full bg-slate-900 border border-slate-700 rounded px-2.5 py-1.5 text-xs text-slate-200">
                        </div>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                        <div>
                            <label for="audioMode" class="block text-xs text-slate-400 mb-1">Modo de Audio:</label>
                            <select id="audioMode" onchange="generateCommand()" class="w-full bg-slate-900 border border-slate-700 rounded px-2.5 py-1.5 text-xs text-slate-200">
                                <option value="replace" selected>Reemplazar audio original</option>
                                <option value="mix">Mezclar ambos audios</option>
                            </select>
                        </div>
                        <div>
                            <label for="outputVideo" class="block text-xs text-slate-400 mb-1">Archivo Resultante:</label>
                            <input type="text" id="outputVideo" value="trivia_01_final.mp4" oninput="generateCommand()" class="w-full bg-slate-900 border border-slate-700 rounded px-2.5 py-1.5 text-xs text-slate-200">
                        </div>
                    </div>
                </div>

                <!-- Opciones de Fragmentación -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-3" id="segmentsSection">
                    <div>
                        <label for="mode" class="block text-xs font-semibold text-slate-400 mb-1">4. Modo de Descarga:</label>
                        <select id="mode" onchange="toggleSegments(); generateCommand();" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-2.5 py-1.5 text-xs text-slate-200">
                            <option value="single" selected>Archivo Completo</option>
                            <option value="segments">Por Tramos (Fraccionado)</option>
                        </select>
                    </div>
                    <div>
                        <label for="duration" class="block text-xs font-semibold text-slate-400 mb-1">Minutos por tramo:</label>
                        <input type="number" id="duration" value="30" step="5" disabled oninput="generateCommand()" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-2.5 py-1.5 text-xs text-slate-200 disabled:opacity-40">
                    </div>
                    <div>
                        <label for="totalDuration" class="block text-xs font-semibold text-slate-400 mb-1">Duración Total (HH:MM:SS):</label>
                        <input type="text" id="totalDuration" value="01:00:00" disabled oninput="generateCommand()" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-2.5 py-1.5 text-xs text-slate-200 disabled:opacity-40">
                    </div>
                </div>

                <!-- Salida del Comando -->
                <div>
                    <label for="finalOutput" class="block text-xs font-semibold text-slate-400 mb-1">Comando Listo para la Consola (CMD/PowerShell):</label>
                    <textarea id="finalOutput" readonly class="w-full h-28 bg-slate-950 border border-slate-700 rounded-lg p-3 text-amber-300 font-mono text-xs focus:outline-none resize-none"></textarea>
                </div>

                <button id="copyBtn" onclick="copyToClipboard()" class="w-full bg-amber-500 hover:bg-amber-600 text-slate-950 font-bold py-2.5 rounded-lg text-sm transition-all cursor-pointer shadow-md">
                    📋 Copiar Comando al Portapapeles
                </button>
            </div>
        </section>
    </div>

    <!-- SCRIPT UNIFICADO -->
    <script>
        /* ----------------------------------------------------
           LÓGICA GENERAL Y NAVEGACIÓN ENTRE PESTAÑAS
        ---------------------------------------------------- */
        function switchTab(tab) {
            const secImg = document.getElementById('sectionImages');
            const secVid = document.getElementById('sectionVideos');
            const btnImg = document.getElementById('tabImagesBtn');
            const btnVid = document.getElementById('tabVideosBtn');

            if (tab === 'images') {
                secImg.classList.remove('hidden');
                secVid.classList.add('hidden');
                btnImg.className = "flex-1 sm:flex-initial px-5 py-2 rounded-lg text-sm font-semibold transition-all duration-200 bg-amber-500 text-slate-950 shadow-md";
                btnVid.className = "flex-1 sm:flex-initial px-5 py-2 rounded-lg text-sm font-semibold transition-all duration-200 text-slate-400 hover:text-slate-200";
            } else {
                secImg.classList.add('hidden');
                secVid.classList.remove('hidden');
                btnVid.className = "flex-1 sm:flex-initial px-5 py-2 rounded-lg text-sm font-semibold transition-all duration-200 bg-amber-500 text-slate-950 shadow-md";
                btnImg.className = "flex-1 sm:flex-initial px-5 py-2 rounded-lg text-sm font-semibold transition-all duration-200 text-slate-400 hover:text-slate-200";
            }
        }

        /* ----------------------------------------------------
           LÓGICA DEL GESTOR DE IMÁGENES
        ---------------------------------------------------- */
        const fileInput = document.getElementById('fileInput');
        const cardsContainer = document.getElementById('cardsContainer');
        const globalCategory = document.getElementById('globalCategory');
        const globalKeepOriginal = document.getElementById('globalKeepOriginal');
        const globalQuality = document.getElementById('globalQuality');
        const qualityVal = document.getElementById('qualityVal');
        const batchActions = document.getElementById('batchActions');
        const counterText = document.getElementById('counterText');
        const downloadAllBtn = document.getElementById('downloadAllBtn');

        let imageFiles = [];

        globalQuality.addEventListener('input', (e) => {
            qualityVal.textContent = e.target.value;
        });

        fileInput.addEventListener('change', (e) => {
            const files = Array.from(e.target.files);
            let processedCount = 0;

            files.forEach(file => {
                if (!file.type.startsWith('image/')) return;
                
                const reader = new FileReader();
                reader.onload = (event) => {
                    const img = new Image();
                    img.onload = () => {
                        const randomStr = Math.random().toString(36);
                        const fileDate = file.lastModified ? new Date(file.lastModified) : new Date();
                        const rawName = file.name.substring(0, file.name.lastIndexOf('.')) || file.name;
                        const keepOrig = globalKeepOriginal.checked;
                        
                        const imgData = {
                            id: randomStr.substring(2, 9),
                            fileObj: file,
                            imgElement: img,
                            originalSize: file.size,
                            timestamp: fileDate.getTime(),
                            keepOriginal: keepOrig,
                            originalName: rawName,
                            customName: 'imagen',
                            category: globalCategory.value,
                            customLegend: globalCategory.value === 'instalaciones' ? 'Instalaciones' : 'Galería',
                            dateStr: getFormattedDate(fileDate),
                            quality: parseInt(globalQuality.value) / 100
                        };
                        imageFiles.push(imgData);
                        processedCount++;

                        if (processedCount === files.filter(f => f.type.startsWith('image/')).length) {
                            sortImages();
                            renderCards();
                        }
                    };
                    img.src = event.target.result;
                };
                reader.readAsDataURL(file);
            });
            fileInput.value = '';
        });

        globalCategory.addEventListener('change', () => {
            const val = globalCategory.value;
            imageFiles.forEach(item => {
                item.category = val;
                item.customLegend = val === 'instalaciones' ? 'Instalaciones' : 'Galería';
            });
            renderCards();
        });

        globalKeepOriginal.addEventListener('change', () => {
            const val = globalKeepOriginal.checked;
            imageFiles.forEach(item => {
                item.keepOriginal = val;
            });
            renderCards();
        });

        globalQuality.addEventListener('change', () => {
            const q = parseInt(globalQuality.value) / 100;
            imageFiles.forEach(item => {
                item.quality = q;
            });
            renderCards();
        });

        function getFormattedDate(dateObj) {
            const yyyy = dateObj.getFullYear();
            const mm = String(dateObj.getMonth() + 1).padStart(2, '0');
            const dd = String(dateObj.getDate()).padStart(2, '0');
            const hh = String(dateObj.getHours()).padStart(2, '0');
            const min = String(dateObj.getMinutes()).padStart(2, '0');
            const ss = String(dateObj.getSeconds()).padStart(2, '0');
            return `${yyyy}${mm}${dd}${hh}${min}${ss}`;
        }

        function sortImages() {
            imageFiles.sort((a, b) => b.timestamp - a.timestamp);
        }

        function formatBytes(bytes) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024, dm = 2;
            const sizes = ['Bytes', 'KB', 'MB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + ' ' + sizes[i];
        }

        function renderCards() {
            cardsContainer.innerHTML = '';
            if (imageFiles.length > 0) {
                batchActions.classList.remove('hidden');
                counterText.textContent = `${imageFiles.length} ${imageFiles.length === 1 ? 'imagen cargada' : 'imágenes cargadas'}`;
            } else {
                batchActions.classList.add('hidden');
            }

            imageFiles.forEach((item, index) => {
                const card = document.createElement('div');
                card.className = "bg-slate-800 rounded-xl border border-slate-700 p-4 flex flex-col gap-3 shadow-md";
                
                const finalGeneratedName = item.keepOriginal 
                    ? `${item.originalName}.webp` 
                    : `${item.dateStr}_${item.customName || 'imagen'}.webp`;

                card.innerHTML = `
                    <div class="relative rounded-lg overflow-hidden bg-slate-950 aspect-video flex items-center justify-center border border-slate-700">
                        <canvas id="canvas-${index}" class="preview-canvas object-contain w-full h-full"></canvas>
                    </div>
                    <div class="flex flex-col gap-2 text-sm">
                        <div class="flex justify-between items-center text-xs text-slate-400">
                            <span>Original: <strong class="text-slate-200">${formatBytes(item.originalSize)}</strong></span>
                            <div class="flex items-center gap-1.5">
                                <input type="checkbox" data-index="${index}" class="card-keep-original w-3.5 h-3.5 accent-amber-500 cursor-pointer" ${item.keepOriginal ? 'checked' : ''}>
                                <span class="cursor-pointer">Respetar original</span>
                            </div>
                        </div>
                        
                        ${item.keepOriginal ? `
                            <div class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1.5 text-slate-400 text-xs truncate">
                                ${item.originalName}
                            </div>
                        ` : `
                            <input type="text" value="${item.customName}" data-index="${index}" placeholder="Nombre corto..." class="custom-name-input w-full bg-slate-900 border border-slate-600 rounded px-2 py-1 text-slate-200 text-xs focus:outline-none focus:border-amber-500">
                        `}
                        
                        <div class="grid grid-cols-2 gap-2 mt-1">
                            <div>
                                <label class="text-xs text-slate-400">Tipo:</label>
                                <select data-index="${index}" class="category-select w-full bg-slate-900 border border-slate-600 rounded px-2 py-1 text-slate-200 text-xs focus:outline-none focus:border-amber-500">
                                    <option value="instalaciones" ${item.category === 'instalaciones' ? 'selected' : ''}>Instalaciones</option>
                                    <option value="galeria" ${item.category === 'galeria' ? 'selected' : ''}>Galería</option>
                                </select>
                            </div>
                            <div>
                                <label class="text-xs text-slate-400">Leyenda:</label>
                                <input type="text" value="${item.customLegend}" data-index="${index}" class="legend-input w-full bg-slate-900 border border-slate-600 rounded px-2 py-1 text-slate-200 text-xs focus:outline-none focus:border-amber-500">
                            </div>
                        </div>
                        <div class="text-xs text-amber-400/90 truncate mt-1 bg-slate-900/60 p-1.5 rounded border border-slate-800 flex justify-between items-center">
                            <span class="truncate"><span class="text-slate-500">Salida:</span> ${finalGeneratedName}</span>
                            <span id="size-badge-${index}" class="text-[10px] bg-slate-800 px-1.5 py-0.5 rounded text-slate-300 shrink-0">Calculando...</span>
                        </div>
                    </div>
                    <div class="flex justify-between items-center pt-2 border-t border-slate-700/60 mt-auto">
                        <button data-index="${index}" class="delete-btn text-rose-400 hover:text-rose-300 text-xs font-medium cursor-pointer">Eliminar</button>
                        <button data-index="${index}" class="download-single-btn bg-amber-500 hover:bg-amber-600 text-slate-950 font-bold px-3 py-1.5 rounded text-xs transition-colors cursor-pointer">Descargar WebP</button>
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

            const img = item.imgElement;
            canvas.width = img.naturalWidth;
            canvas.height = img.naturalHeight;

            ctx.drawImage(img, 0, 0);

            if (item.customLegend.trim() !== '') {
                const fontSize = Math.max(16, Math.floor(canvas.height * 0.04));
                ctx.font = `bold ${fontSize}px sans-serif`;
                ctx.fillStyle = "rgba(0, 0, 0, 0.6)";
                
                const textMetrics = ctx.measureText(item.customLegend);
                const padding = fontSize * 0.8;
                const boxWidth = textMetrics.width + (padding * 2);
                const boxHeight = fontSize * 1.6;
                const x = (canvas.width - boxWidth) / 2;
                const y = canvas.height - boxHeight - (fontSize * 0.8);

                ctx.beginPath();
                ctx.roundRect(x, y, boxWidth, boxHeight, 8);
                ctx.fill();

                ctx.fillStyle = "#ffffff";
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                ctx.fillText(item.customLegend, canvas.width / 2, y + (boxHeight / 2));
            }

            canvas.toBlob((blob) => {
                const badge = document.getElementById(`size-badge-${index}`);
                if (badge) {
                    const newSize = blob.size;
                    const diff = newSize - item.originalSize;
                    const diffPercent = Math.round((diff / item.originalSize) * 100);
                    badge.textContent = `${formatBytes(newSize)} (${diffPercent > 0 ? '+' : ''}${diffPercent}%)`;
                    badge.className = `text-[10px] px-1.5 py-0.5 rounded shrink-0 font-medium ${newSize < item.originalSize ? 'bg-emerald-950 text-emerald-400 border border-emerald-800' : 'bg-amber-950 text-amber-400 border border-amber-800'}`;
                }
            }, 'image/webp', item.quality);
        }

        function attachEvents() {
            document.querySelectorAll('.custom-name-input').forEach(input => {
                input.addEventListener('input', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles[idx].customName = e.target.value;
                });
            });

            document.querySelectorAll('.card-keep-original').forEach(chk => {
                chk.addEventListener('change', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles[idx].keepOriginal = e.target.checked;
                    renderCards();
                });
            });

            document.querySelectorAll('.legend-input').forEach(input => {
                input.addEventListener('input', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles[idx].customLegend = e.target.value;
                    drawPreviewAndMeasure(idx);
                });
            });

            document.querySelectorAll('.category-select').forEach(select => {
                select.addEventListener('change', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles[idx].category = e.target.value;
                    if(e.target.value === 'instalaciones') {
                        imageFiles[idx].customLegend = 'Instalaciones';
                    } else {
                        imageFiles[idx].customLegend = 'Galería';
                    }
                    renderCards();
                });
            });

            document.querySelectorAll('.delete-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const idx = e.target.dataset.index;
                    imageFiles.splice(idx, 1);
                    renderCards();
                });
            });

            document.querySelectorAll('.download-single-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const idx = e.target.dataset.index;
                    downloadItem(idx);
                });
            });

            downloadAllBtn.onclick = () => {
                imageFiles.forEach((_, idx) => {
                    setTimeout(() => downloadItem(idx), idx * 250);
                });
            };
        }

        function downloadItem(index) {
            const item = imageFiles[index];
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            canvas.width = item.imgElement.naturalWidth;
            canvas.height = item.imgElement.naturalHeight;
            ctx.drawImage(item.imgElement, 0, 0);

            if (item.customLegend.trim() !== '') {
                const fontSize = Math.max(16, Math.floor(canvas.height * 0.04));
                ctx.font = `bold ${fontSize}px sans-serif`;
                ctx.fillStyle = "rgba(0, 0, 0, 0.6)";
                const textMetrics = ctx.measureText(item.customLegend);
                const padding = fontSize * 0.8;
                const boxWidth = textMetrics.width + (padding * 2);
                const boxHeight = fontSize * 1.6;
                const x = (canvas.width - boxWidth) / 2;
                const y = canvas.height - boxHeight - (fontSize * 0.8);

                ctx.beginPath();
                ctx.roundRect(x, y, boxWidth, boxHeight, 8);
                ctx.fill();

                ctx.fillStyle = "#ffffff";
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                ctx.fillText(item.customLegend, canvas.width / 2, y + (boxHeight / 2));
            }

            canvas.toBlob((blob) => {
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = item.keepOriginal ? `${item.originalName}.webp` : `${item.dateStr}_${item.customName || 'imagen'}.webp`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
            }, 'image/webp', item.quality);
        }

        /* ----------------------------------------------------
           LÓGICA DEL GENERADOR DE COMANDOS YT-DLP & FFMPEG
        ---------------------------------------------------- */
        async function pasteClipboard() {
            try {
                const text = await navigator.clipboard.readText();
                if (text) {
                    document.getElementById('url').value = text.trim();
                    generateCommand();
                }
            } catch (err) {
                alert('Por favor concede permiso al navegador para acceder al portapapeles.');
            }
        }

        function toggleFormatOptions() {
            const ext = document.getElementById('ext').value;
            const isAudio = (ext === 'mp3' || ext === 'm4a');
            const isMerge = (ext === 'merge_audio');

            document.getElementById('qualityBox').style.display = (isAudio || isMerge) ? 'none' : 'block';
            document.getElementById('downloadSection').style.display = isMerge ? 'none' : 'block';
            document.getElementById('segmentsSection').style.display = isMerge ? 'none' : 'grid';
            document.getElementById('audioMergeBox').style.display = isMerge ? 'flex' : 'none';
        }

        function toggleSegments() {
            const isSeg = (document.getElementById('mode').value === 'segments');
            document.getElementById('duration').disabled = !isSeg;
            document.getElementById('totalDuration').disabled = !isSeg;
        }

        function timeToSeconds(timeStr) {
            if (!timeStr) return 0;
            const parts = timeStr.split(':').map(Number);
            if (parts.length === 3) return parts[0] * 3600 + parts[1] * 60 + parts[2];
            if (parts.length === 2) return parts[0] * 60 + parts[1];
            return 0;
        }

        function secondsToTime(secs) {
            const h = Math.floor(secs / 3600).toString().padStart(2, '0');
            const m = Math.floor((secs % 3600) / 60).toString().padStart(2, '0');
            const s = Math.floor(secs % 60).toString().padStart(2, '0');
            return `${h}:${m}:${s}`;
        }

        function getFormatFilter(quality, ext) {
            if (ext === 'mp4') {
                if (quality === 'best') return '"bv*[ext=mp4]+ba[ext=m4a]/b[ext=mp4]/best"';
                return `"bv*[height<=${quality}][ext=mp4]+ba[ext=m4a]/b[height<=${quality}][ext=mp4]/best"`;
            } else {
                if (quality === 'best') return '"bestvideo+bestaudio"';
                return `"bestvideo[height<=${quality}]+bestaudio/best[height<=${quality}]"`;
            }
        }

        function generateCommand() {
            const ext = document.getElementById('ext').value;
            const output = document.getElementById('finalOutput');

            if (ext === 'merge_audio') {
                const vIn = document.getElementById('localVideo').value.trim() || 'video.mp4';
                const aIn = document.getElementById('localAudio').value.trim() || 'audio.mp3';
                const vOut = document.getElementById('outputVideo').value.trim() || 'resultado.mp4';
                const aMode = document.getElementById('audioMode').value;

                if (aMode === 'replace') {
                    output.value = `ffmpeg -i "${vIn}" -i "${aIn}" -c:v copy -c:a aac -map 0:v:0 -map 1:a:0 -shortest "${vOut}"`;
                } else {
                    output.value = `ffmpeg -i "${vIn}" -i "${aIn}" -filter_complex "[0:a][1:a]amix=inputs=2:duration=first[a]" -map 0:v -map "[a]" -c:v copy -c:a aac "${vOut}"`;
                }
                return;
            }

            const url = document.getElementById('url').value.trim() || 'URL_DEL_VIDEO';
            const defaultPath = '%USERPROFILE%\\Downloads\\';
            const defaultPrefix = '%(title)s';
            
            const quality = document.getElementById('quality').value;
            const mode = document.getElementById('mode').value;
            const isAudio = (ext === 'mp3' || ext === 'm4a');

            let baseCmd = '';
            if (isAudio) {
                if (ext === 'mp3') {
                    baseCmd = `yt-dlp -x --audio-format mp3 --audio-quality 0`;
                } else {
                    baseCmd = `yt-dlp -f "ba[ext=m4a]/ba" -x --audio-format m4a`;
                }
            } else {
                const formatFilter = getFormatFilter(quality, ext);
                baseCmd = `yt-dlp -f ${formatFilter} --merge-output-format ${ext}`;
            }

            if (mode === 'single') {
                output.value = `${baseCmd} -o "${defaultPath}${defaultPrefix}.%(ext)s" "${url}"`;
            } else {
                const segMinutes = parseFloat(document.getElementById('duration').value) || 30;
                const totalSecs = timeToSeconds(document.getElementById('totalDuration').value);
                const segSecs = segMinutes * 60;
                
                if (totalSecs <= 0) {
                    output.value = 'Ingresa la duración total del video para calcular los tramos.';
                    return;
                }

                let currentSec = 0;
                let commands = [];
                let index = 1;

                while (currentSec < totalSecs) {
                    let nextSec = Math.min(currentSec + segSecs, totalSecs);
                    let startTime = secondsToTime(currentSec);
                    let endTime = secondsToTime(nextSec);
                    let numStr = index.toString().padStart(2, '0');

                    let cmd = `${baseCmd} --download-sections "*${startTime}-${endTime}" -o "${defaultPath}${defaultPrefix}_parte_${numStr}.%(ext)s" "${url}"`;
                    commands.push(cmd);

                    currentSec = nextSec;
                    index++;
                }

                output.value = commands.join(' && ');
            }
        }

        function copyToClipboard() {
            const outputText = document.getElementById('finalOutput').value;
            const copyBtn = document.getElementById('copyBtn');

            if (!outputText || outputText.startsWith('Ingresa')) {
                alert('Primero genera un comando válido.');
                return;
            }

            navigator.clipboard.writeText(outputText).then(() => {
                const originalText = copyBtn.innerText;
                copyBtn.innerText = '¡Copiado con éxito! ✅';
                copyBtn.className = "w-full bg-emerald-500 text-slate-950 font-bold py-2.5 rounded-lg text-sm transition-all cursor-pointer shadow-md";

                setTimeout(() => {
                    copyBtn.innerText = originalText;
                    copyBtn.className = "w-full bg-amber-500 hover:bg-amber-600 text-slate-950 font-bold py-2.5 rounded-lg text-sm transition-all cursor-pointer shadow-md";
                }, 2000);
            });
        }

        // Inicializar comando por defecto
        generateCommand();
    </script>
</body>
</html>
```

### Archivo: `index2.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Generador Amigable yt-dlp & FFmpeg</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 20px;
      background: #1e1e2e;
      color: #cdd6f4;
    }
    .card {
      background: #181825;
      padding: 25px;
      border-radius: 12px;
      max-width: 800px;
      margin: 0 auto 20px auto;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
    }
    .info-box {
      background: #313244;
      border-left: 4px solid #f9e2af;
      padding: 15px;
      border-radius: 6px;
      margin-bottom: 20px;
      font-size: 13px;
    }
    .info-box summary {
      font-weight: bold;
      color: #f9e2af;
      cursor: pointer;
      font-size: 14px;
    }
    .info-box ul {
      margin: 10px 0 0 20px;
      padding: 0;
    }
    .info-box li {
      margin-bottom: 8px;
    }
    code {
      background: #11111b;
      color: #a6e3a1;
      padding: 3px 6px;
      border-radius: 4px;
      font-family: monospace;
      font-size: 12px;
      display: inline-block;
      margin-top: 2px;
    }
    h2 { color: #89b4fa; margin-top: 0; }
    label { font-weight: 600; display: block; margin-top: 15px; color: #a6adc8; }
    input, select, textarea {
      width: 100%;
      padding: 10px;
      margin-top: 6px;
      box-sizing: border-box;
      border: 1px solid #45475a;
      border-radius: 6px;
      background: #313244;
      color: #cdd6f4;
      font-size: 14px;
    }
    input:disabled, select:disabled {
      background: #181825;
      border-color: #313244;
      color: #585b70;
      cursor: not-allowed;
    }
    textarea.code {
      font-family: monospace;
      height: 120px;
      background: #11111b;
      color: #f9e2af;
      font-size: 13px;
    }
    button {
      padding: 10px 18px;
      background: #a6e3a1;
      color: #11111b;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
      font-weight: bold;
      width: 100%;
      transition: background 0.2s;
    }
    button:hover { background: #94e2d5; }
    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
    .grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; }
    .hint { font-size: 11px; color: #a6adc8; margin-top: 4px; }
    .input-group {
      display: flex;
      gap: 10px;
      align-items: flex-end;
    }
    .input-group div { flex-grow: 1; }
    .btn-paste {
      width: auto;
      margin-top: 0;
      padding: 10px 18px;
      background: #89b4fa;
      color: #11111b;
      white-space: nowrap;
    }
    .btn-paste:hover { background: #b4befe; }
    .audio-box {
      border: 1px dashed #89b4fa;
      padding: 15px;
      border-radius: 8px;
      margin-top: 15px;
      background: #1e1e2e;
    }
  </style>
</head>
<body>

<div class="card">
  <h2>Generador Amigable yt-dlp & FFmpeg</h2>

  <!-- Requisitos e Instalación Rápida -->
  <details class="info-box">
    <summary>📌 Requisitos e Instalación Rápida en Windows 11 (Haz clic para desplegar)</summary>
    <p style="margin: 8px 0 5px 0;">Para ejecutar los comandos generados, necesitas tener instalados <code>yt-dlp</code> y <code>ffmpeg</code>. Puedes instalarlos ejecutando estos comandos en tu consola (PowerShell o CMD):</p>
    
    <ul>
      <li>
        <b>Opción 1: Vía winget (Gestor oficial de Windows 11 - Recomendado):</b><br>
        <code>winget install yt-dlp.yt-dlp</code><br>
        <code>winget install Gyan.FFmpeg</code>
      </li>
      <li>
        <b>Opción 2: Descarga directa a tu carpeta activa (PowerShell):</b><br>
        <code>Invoke-WebRequest -Uri "https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp.exe" -OutFile "yt-dlp.exe"</code>
      </li>
      <li>
        <b>Nota sobre FFmpeg:</b> Es imprescindible para convertir audio a MP3, normalizar volumen, reparar metadatos o insertar pistas de audio.
      </li>
    </ul>
  </details>

  <div id="downloadSection">
    <div class="input-group">
      <div>
        <label for="url">1. URL del Video (YouTube, Instagram, TikTok, Vimeo, etc.):</label>
        <input type="text" id="url" placeholder="https://www.youtube.com/watch?v=..." oninput="generateCommand()">
      </div>
      <button type="button" class="btn-paste" onclick="pasteClipboard()">📋 Pegar URL</button>
    </div>
    <div class="hint">Compatible con YouTube, Instagram, TikTok, Twitter/X, Facebook, Vimeo y más.</div>
  </div>

  <div class="grid-2">
    <div>
      <label for="ext">2. Acción / Formato Final de Salida:</label>
      <select id="ext" onchange="toggleFormatOptions(); generateCommand();">
        <option value="mp4" selected>Video MP4 (Compatible con todo)</option>
        <option value="mkv">Video MKV (Máxima fidelidad)</option>
        <option value="webm">Video WEBM (Nativo de YouTube)</option>
        <option value="mp3">Solo Audio MP3 (Música / Podcast)</option>
        <option value="m4a">Solo Audio M4A (Sin pérdida / Original)</option>
        <option value="merge_audio">🎵 Insertar/Mezclar Audio local con Video</option>
        <option value="fix_meta">🛠️ Reparar Metadatos MP4 (faststart / Bucle)</option>
        <option value="normalize_lufs">🔊 Normalizar Audio (-26 LUFS / Sala de Espera)</option>
      </select>
    </div>
    <div id="qualityBox">
      <label for="quality">3. Calidad de Video Deseada:</label>
      <select id="quality" onchange="generateCommand()">
        <option value="best">Máxima Calidad Disponible (4K/2K/1080p)</option>
        <option value="1080" selected>Full HD (1080p) - Recomendado</option>
        <option value="720">HD (720p) - Descarga Rápida</option>
        <option value="480">Estándar (480p) - Poco peso</option>
      </select>
    </div>
  </div>

  <!-- Sección Especial para Normalización de Audio (LUFS) -->
  <div id="normalizeBox" class="audio-box" style="display: none;">
    <h3 style="margin-top:0; color:#f9e2af; font-size:15px;">Normalización de Audio EBU R128 (FFmpeg)</h3>
    <div class="grid-3">
      <div>
        <label for="normMode">Modo de Procesamiento:</label>
        <select id="normMode" onchange="toggleNormModeOptions(); generateCommand();">
          <option value="single" selected>Un solo archivo</option>
          <option value="batch_num">Lote numerado (FOR /L)</option>
          <option value="batch_all">Todos los *.mp4 de la carpeta (CMD)</option>
        </select>
      </div>
      <div>
        <label for="normTarget">Objetivo LUFS:</label>
        <input type="number" id="normTarget" value="-26" step="1" oninput="generateCommand()">
      </div>
      <div>
        <label for="normOverwrite">Manejo de Nombre:</label>
        <select id="normOverwrite" onchange="generateCommand()">
          <option value="replace" selected>Reemplazar original (Mantiene nombre)</option>
          <option value="prefix">Crear copia (norm_archivo.mp4)</option>
        </select>
      </div>
    </div>

    <!-- Opción Un Solo Archivo -->
    <div id="normSingleBox" style="margin-top: 10px;">
      <label for="normSingleInput">Nombre / Ruta del Archivo:</label>
      <input type="text" id="normSingleInput" value="trivia_01.mp4" placeholder="trivia_01.mp4" oninput="generateCommand()">
    </div>

    <!-- Opción Lote Numerado -->
    <div id="normBatchBox" class="grid-3" style="display: none; margin-top: 10px;">
      <div>
        <label for="normPrefix">Prefijo:</label>
        <input type="text" id="normPrefix" value="trivia_0" placeholder="Ej: trivia_0" oninput="generateCommand()">
      </div>
      <div>
        <label for="normStart">Inicio:</label>
        <input type="number" id="normStart" value="1" min="0" oninput="generateCommand()">
      </div>
      <div>
        <label for="normEnd">Fin:</label>
        <input type="number" id="normEnd" value="6" min="1" oninput="generateCommand()">
      </div>
    </div>
  </div>

  <!-- Sección Especial para Insertar Audio Local -->
  <div id="audioMergeBox" class="audio-box" style="display: none;">
    <h3 style="margin-top:0; color:#f9e2af; font-size:15px;">Configuración de Ensamblado de Audio (FFmpeg)</h3>
    <div class="grid-2">
      <div>
        <label for="localVideo">Ruta / Nombre del Video Base:</label>
        <input type="text" id="localVideo" value="trivia_01.mp4" placeholder="Ej: D:\Trivias\trivia_01.mp4" oninput="generateCommand()">
      </div>
      <div>
        <label for="localAudio">Ruta / Nombre del Audio (MP3/WAV):</label>
        <input type="text" id="localAudio" value="musica.mp3" placeholder="Ej: D:\Trivias\musica.mp3" oninput="generateCommand()">
      </div>
    </div>
    <div class="grid-2">
      <div>
        <label for="audioMode">Modo de Audio:</label>
        <select id="audioMode" onchange="generateCommand()">
          <option value="replace" selected>Reemplazar audio original por completo</option>
          <option value="mix">Mezclar nuevo audio con el audio existente</option>
        </select>
      </div>
      <div>
        <label for="outputVideo">Nombre del Archivo Resultante:</label>
        <input type="text" id="outputVideo" value="trivia_01_final.mp4" placeholder="trivia_01_final.mp4" oninput="generateCommand()">
      </div>
    </div>
  </div>

  <!-- Sección Especial para Reparar Metadatos (Faststart) -->
  <div id="metaBox" class="audio-box" style="display: none;">
    <h3 style="margin-top:0; color:#f9e2af; font-size:15px;">Reparación e Indexación de Contenedor MP4</h3>
    <div class="grid-2">
      <div>
        <label for="fixMode">Tipo de Procesamiento:</label>
        <select id="fixMode" onchange="toggleFixModeOptions(); generateCommand();">
          <option value="single" selected>Un solo archivo</option>
          <option value="batch">Lote numerado (Bucle FOR)</option>
        </select>
      </div>
      <div id="singleFixBox">
        <label for="singleInput">Nombre/Ruta del Archivo:</label>
        <input type="text" id="singleInput" value="trivia_01.mp4" placeholder="trivia_01.mp4" oninput="generateCommand()">
      </div>
    </div>

    <div id="batchFixBox" class="grid-3" style="display: none; margin-top: 10px;">
      <div>
        <label for="batchPrefix">Prefijo:</label>
        <input type="text" id="batchPrefix" value="trivia_0" placeholder="Ej: trivia_0" oninput="generateCommand()">
      </div>
      <div>
        <label for="batchStart">Inicio:</label>
        <input type="number" id="batchStart" value="2" min="0" oninput="generateCommand()">
      </div>
      <div>
        <label for="batchEnd">Fin:</label>
        <input type="number" id="batchEnd" value="6" min="1" oninput="generateCommand()">
      </div>
    </div>
  </div>

  <!-- Fila dividida en 3 columnas -->
  <div class="grid-3" id="segmentsSection">
    <div>
      <label for="mode">4. Modo de Descarga:</label>
      <select id="mode" onchange="toggleSegments(); generateCommand();">
        <option value="single" selected>Archivo Completo</option>
        <option value="segments">Por Tramos (Fraccionado)</option>
      </select>
    </div>
    <div>
      <label for="duration">Minutos por tramo:</label>
      <input type="number" id="duration" value="30" step="5" disabled oninput="generateCommand()">
    </div>
    <div>
      <label for="totalDuration">Duración Total Video:</label>
      <input type="text" id="totalDuration" value="01:00:00" placeholder="HH:MM:SS" disabled oninput="generateCommand()">
    </div>
  </div>

  <label for="finalOutput">Comando Listo para la Consola (CMD):</label>
  <textarea id="finalOutput" class="code" readonly></textarea>

  <button id="copyBtn" onclick="copyToClipboard()" style="background: #f9e2af; color: #11111b; margin-top: 15px;">
    📋 Copiar Comando al Portapapeles
  </button>
</div>

<script>
  async function pasteClipboard() {
    try {
      const text = await navigator.clipboard.readText();
      if (text) {
        document.getElementById('url').value = text.trim();
        generateCommand();
      }
    } catch (err) {
      alert('Por favor concede permiso al navegador para acceder al portapapeles.');
    }
  }

  function toggleFormatOptions() {
    const ext = document.getElementById('ext').value;
    const isAudio = (ext === 'mp3' || ext === 'm4a');
    const isMerge = (ext === 'merge_audio');
    const isMeta = (ext === 'fix_meta');
    const isNorm = (ext === 'normalize_lufs');

    document.getElementById('qualityBox').style.display = (isAudio || isMerge || isMeta || isNorm) ? 'none' : 'block';
    document.getElementById('downloadSection').style.display = (isMerge || isMeta || isNorm) ? 'none' : 'flex';
    document.getElementById('segmentsSection').style.display = (isMerge || isMeta || isNorm) ? 'none' : 'grid';
    document.getElementById('audioMergeBox').style.display = isMerge ? 'block' : 'none';
    document.getElementById('metaBox').style.display = isMeta ? 'block' : 'none';
    document.getElementById('normalizeBox').style.display = isNorm ? 'block' : 'none';
  }

  function toggleFixModeOptions() {
    const isBatch = (document.getElementById('fixMode').value === 'batch');
    document.getElementById('singleFixBox').style.display = isBatch ? 'none' : 'block';
    document.getElementById('batchFixBox').style.display = isBatch ? 'grid' : 'none';
  }

  function toggleNormModeOptions() {
    const normMode = document.getElementById('normMode').value;
    document.getElementById('normSingleBox').style.display = (normMode === 'single') ? 'block' : 'none';
    document.getElementById('normBatchBox').style.display = (normMode === 'batch_num') ? 'grid' : 'none';
  }

  function toggleSegments() {
    const isSeg = (document.getElementById('mode').value === 'segments');
    document.getElementById('duration').disabled = !isSeg;
    document.getElementById('totalDuration').disabled = !isSeg;
  }

  function timeToSeconds(timeStr) {
    if (!timeStr) return 0;
    const parts = timeStr.split(':').map(Number);
    if (parts.length === 3) return parts[0] * 3600 + parts[1] * 60 + parts[2];
    if (parts.length === 2) return parts[0] * 60 + parts[1];
    return 0;
  }

  function secondsToTime(secs) {
    const h = Math.floor(secs / 3600).toString().padStart(2, '0');
    const m = Math.floor((secs % 3600) / 60).toString().padStart(2, '0');
    const s = Math.floor(secs % 60).toString().padStart(2, '0');
    return `${h}:${m}:${s}`;
  }

  function getFormatFilter(quality, ext) {
    if (ext === 'mp4') {
      if (quality === 'best') return '"bv*[ext=mp4]+ba[ext=m4a]/b[ext=mp4]/best"';
      return `"bv*[height<=${quality}][ext=mp4]+ba[ext=m4a]/b[height<=${quality}][ext=mp4]/best"`;
    } else {
      if (quality === 'best') return '"bestvideo+bestaudio"';
      return `"bestvideo[height<=${quality}]+bestaudio/best[height<=${quality}]"`;
    }
  }

  function generateCommand() {
    const ext = document.getElementById('ext').value;
    const output = document.getElementById('finalOutput');

    // Módulo especial para Normalización LUFS
    if (ext === 'normalize_lufs') {
      const normMode = document.getElementById('normMode').value;
      const targetLUFS = document.getElementById('normTarget').value || '-26';
      const overwrite = document.getElementById('normOverwrite').value === 'replace';

      if (normMode === 'single') {
        const fileIn = document.getElementById('normSingleInput').value.trim() || 'video.mp4';
        if (overwrite) {
          output.value = `ffmpeg -i "${fileIn}" -af loudnorm=I=${targetLUFS}:LRA=7:TP=-2 -c:v copy "temp_${fileIn}" && move /y "temp_${fileIn}" "${fileIn}"`;
        } else {
          output.value = `ffmpeg -i "${fileIn}" -af loudnorm=I=${targetLUFS}:LRA=7:TP=-2 -c:v copy "norm_${fileIn}"`;
        }
      } else if (normMode === 'batch_num') {
        const prefix = document.getElementById('normPrefix').value.trim();
        const start = document.getElementById('normStart').value;
        const end = document.getElementById('normEnd').value;
        if (overwrite) {
          output.value = `for /L %i in (${start},1,${end}) do (ffmpeg -i ${prefix}%i.mp4 -af loudnorm=I=${targetLUFS}:LRA=7:TP=-2 -c:v copy temp_${prefix}%i.mp4 && move /y temp_${prefix}%i.mp4 ${prefix}%i.mp4)`;
        } else {
          output.value = `for /L %i in (${start},1,${end}) do ffmpeg -i ${prefix}%i.mp4 -af loudnorm=I=${targetLUFS}:LRA=7:TP=-2 -c:v copy norm_${prefix}%i.mp4`;
        }
      } else { // batch_all (*.mp4 en CMD)
        if (overwrite) {
          output.value = `for %f in (*.mp4) do (ffmpeg -i "%f" -af loudnorm=I=${targetLUFS}:LRA=7:TP=-2 -c:v copy "temp_%f" && move /y "temp_%f" "%f")`;
        } else {
          output.value = `for %f in (*.mp4) do ffmpeg -i "%f" -af loudnorm=I=${targetLUFS}:LRA=7:TP=-2 -c:v copy "norm_%f"`;
        }
      }
      return;
    }

    // Módulo especial para reparación de metadatos (faststart)
    if (ext === 'fix_meta') {
      const fixMode = document.getElementById('fixMode').value;
      if (fixMode === 'single') {
        const fileIn = document.getElementById('singleInput').value.trim() || 'archivo.mp4';
        const nameParts = fileIn.split('.');
        const ext = nameParts.pop();
        const baseName = nameParts.join('.');
        output.value = `ffmpeg -i "${fileIn}" -c copy -movflags +faststart "${baseName}_corregido.${ext}"`;
      } else {
        const prefix = document.getElementById('batchPrefix').value.trim();
        const start = document.getElementById('batchStart').value;
        const end = document.getElementById('batchEnd').value;
        output.value = `for /L %i in (${start},1,${end}) do ffmpeg -i ${prefix}%i.mp4 -c copy -movflags +faststart ${prefix}%i_corregido.mp4`;
      }
      return;
    }

    // Módulo especial para unión de audio con FFmpeg
    if (ext === 'merge_audio') {
      const vIn = document.getElementById('localVideo').value.trim() || 'video.mp4';
      const aIn = document.getElementById('localAudio').value.trim() || 'audio.mp3';
      const vOut = document.getElementById('outputVideo').value.trim() || 'resultado.mp4';
      const aMode = document.getElementById('audioMode').value;

      if (aMode === 'replace') {
        output.value = `ffmpeg -i "${vIn}" -i "${aIn}" -c:v copy -c:a aac -map 0:v:0 -map 1:a:0 -shortest "${vOut}"`;
      } else {
        output.value = `ffmpeg -i "${vIn}" -i "${aIn}" -filter_complex "[0:a][1:a]amix=inputs=2:duration=first[a]" -map 0:v -map "[a]" -c:v copy -c:a aac "${vOut}"`;
      }
      return;
    }

    const url = document.getElementById('url').value.trim() || 'URL_DEL_VIDEO';
    const defaultPath = '%USERPROFILE%\\Downloads\\';
    const defaultPrefix = '%(title)s';
    
    const quality = document.getElementById('quality').value;
    const mode = document.getElementById('mode').value;
    const isAudio = (ext === 'mp3' || ext === 'm4a');

    let baseCmd = '';
    if (isAudio) {
      if (ext === 'mp3') {
        baseCmd = `yt-dlp -x --audio-format mp3 --audio-quality 0`;
      } else {
        baseCmd = `yt-dlp -f "ba[ext=m4a]/ba" -x --audio-format m4a`;
      }
    } else {
      const formatFilter = getFormatFilter(quality, ext);
      baseCmd = `yt-dlp -f ${formatFilter} --merge-output-format ${ext}`;
    }

    if (mode === 'single') {
      output.value = `${baseCmd} -o "${defaultPath}${defaultPrefix}.%(ext)s" "${url}"`;
    } else {
      const segMinutes = parseFloat(document.getElementById('duration').value) || 30;
      const totalSecs = timeToSeconds(document.getElementById('totalDuration').value);
      const segSecs = segMinutes * 60;
      
      if (totalSecs <= 0) {
        output.value = 'Ingresa la duración total del video para calcular los tramos.';
        return;
      }

      let currentSec = 0;
      let commands = [];
      let index = 1;

      while (currentSec < totalSecs) {
        let nextSec = Math.min(currentSec + segSecs, totalSecs);
        let startTime = secondsToTime(currentSec);
        let endTime = secondsToTime(nextSec);
        let numStr = index.toString().padStart(2, '0');

        let cmd = `${baseCmd} --download-sections "*${startTime}-${endTime}" -o "${defaultPath}${defaultPrefix}_parte_${numStr}.%(ext)s" "${url}"`;
        commands.push(cmd);

        currentSec = nextSec;
        index++;
      }

      output.value = commands.join(' && ');
    }
  }

  function copyToClipboard() {
    const outputText = document.getElementById('finalOutput').value;
    const copyBtn = document.getElementById('copyBtn');

    if (!outputText || outputText.startsWith('Ingresa')) {
      alert('Primero genera un comando válido.');
      return;
    }

    navigator.clipboard.writeText(outputText).then(() => {
      const originalText = copyBtn.innerText;
      copyBtn.innerText = '¡Copiado con éxito! ✅';
      copyBtn.style.background = '#a6e3a1';

      setTimeout(() => {
        copyBtn.innerText = originalText;
        copyBtn.style.background = '#f9e2af';
      }, 2000);
    });
  }

  generateCommand();
</script>

</body>
</html>
```

### Archivo: `index3.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Centro de Comandos Multimedia (para CMD)</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 20px;
      background: #1e1e2e;
      color: #cdd6f4;
    }
    .card {
      background: #181825;
      padding: 25px;
      border-radius: 12px;
      max-width: 800px;
      margin: 0 auto 20px auto;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
    }
    .info-box {
      background: #313244;
      border-left: 4px solid #f9e2af;
      padding: 15px;
      border-radius: 6px;
      margin-bottom: 20px;
      font-size: 13px;
    }
    .info-box summary {
      font-weight: bold;
      color: #f9e2af;
      cursor: pointer;
      font-size: 14px;
    }
    .info-box ul {
      margin: 10px 0 0 20px;
      padding: 0;
    }
    .info-box li { margin-bottom: 8px; }
    code {
      background: #11111b;
      color: #a6e3a1;
      padding: 3px 6px;
      border-radius: 4px;
      font-family: monospace;
      font-size: 12px;
    }
    h2 { color: #89b4fa; margin-top: 0; }
    label { font-weight: 600; display: block; margin-top: 15px; color: #a6adc8; }
    input, select, textarea {
      width: 100%;
      padding: 10px;
      margin-top: 6px;
      box-sizing: border-box;
      border: 1px solid #45475a;
      border-radius: 6px;
      background: #313244;
      color: #cdd6f4;
      font-size: 14px;
    }
    textarea.code {
      font-family: monospace;
      height: 120px;
      background: #11111b;
      color: #f9e2af;
      font-size: 13px;
    }
    button {
      padding: 10px 18px;
      background: #a6e3a1;
      color: #11111b;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 14px;
      font-weight: bold;
      width: 100%;
      transition: background 0.2s;
    }
    button:hover { background: #94e2d5; }
    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
    .grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; }
    .hint { font-size: 11px; color: #a6adc8; margin-top: 4px; }
    .input-group { display: flex; gap: 10px; align-items: flex-end; }
    .input-group div { flex-grow: 1; }
    .btn-paste {
      width: auto;
      margin-top: 0;
      padding: 10px 18px;
      background: #89b4fa;
      color: #11111b;
      white-space: nowrap;
    }
    .btn-paste:hover { background: #b4befe; }
    .action-box {
      border: 1px dashed #89b4fa;
      padding: 15px;
      border-radius: 8px;
      margin-top: 15px;
      background: #1e1e2e;
    }
  </style>
</head>
<body>

<div class="card">
  <h2>Centro de Comandos Multimedia (para CMD)</h2>

  <details class="info-box">
    <summary>📌 Requisitos e Instalación Rápida en Windows (CMD / PowerShell)</summary>
    <p style="margin: 8px 0 5px 0;">Ejecuta estos comandos en tu consola si aún no tienes las herramientas:</p>
    <ul>
      <li><code>winget install yt-dlp.yt-dlp</code></li>
      <li><code>winget install Gyan.FFmpeg</code></li>
    </ul>
  </details>

  <!-- PASO 1: SELECCIONAR LA ACCIÓN PRIMERO -->
  <div>
    <label for="actionType" style="font-size: 15px; color: #f9e2af;">1. ¿Qué deseas hacer?</label>
    <select id="actionType" onchange="updateUI(); generateCommand();">
      <optgroup label="📥 Descargas desde la Web (yt-dlp)">
        <option value="dl_video" selected>Descargar Video (MP4 / MKV / WEBM)</option>
        <option value="dl_audio">Descargar solo Audio (MP3 / M4A)</option>
      </optgroup>
      <optgroup label="⚙️ Procesamiento de Archivos Locales (FFmpeg)">
        <option value="concat_local">🔗 Unir / Concatenar Videos de una carpeta</option>
        <option value="merge_audio">🎵 Insertar / Reemplazar / Mezclar Audio</option>
        <option value="fix_meta">🛠️ Reparar Metadatos (faststart / Bucle MP4)</option>
        <option value="normalize">🔊 Normalizar Volumen (-26 LUFS Sala de Espera)</option>
      </optgroup>
    </select>
  </div>

  <!-- PASO 2A: URL DE DESCARGA -->
  <div id="urlSection" style="margin-top: 15px;">
    <div class="input-group">
      <div>
        <label for="url">2. URL del Video o Playlist Web:</label>
        <input type="text" id="url" placeholder="https://www.youtube.com/watch?v=... o playlist" oninput="generateCommand()">
      </div>
      <button type="button" class="btn-paste" onclick="pasteClipboard()">📋 Pegar URL</button>
    </div>
  </div>

  <!-- OPCIONES DE PLAYLIST / LISTAS -->
  <div id="playlistBox" class="action-box" style="margin-top: 15px;">
    <h3 style="margin-top:0; color:#f9e2af; font-size:14px;">Opciones de Lista / Playlist (Varias partes)</h3>
    <div class="grid-2">
      <div>
        <label for="isPlaylist">¿Es un enlace de Playlist / Partes?:</label>
        <select id="isPlaylist" onchange="generateCommand()">
          <option value="no" selected>No (Video individual)</option>
          <option value="yes">Sí (Es una playlist / Lista de varias partes)</option>
        </select>
      </div>
      <div>
        <label for="playlistMode">Modo de procesamiento de la lista:</label>
        <select id="playlistMode" onchange="generateCommand()">
          <option value="separate" selected>Descargar en partes separadas (01, 02...)</option>
          <option value="concat">Unir todas las partes al descargar</option>
        </select>
      </div>
    </div>
  </div>

  <!-- OPCIONES DE DESCARGA VIDEO -->
  <div id="dlVideoBox" class="grid-2" style="margin-top: 10px;">
    <div>
      <label for="videoFormat">Formato de Video:</label>
      <select id="videoFormat" onchange="generateCommand()">
        <option value="mp4" selected>MP4 (Compatible universal)</option>
        <option value="mkv">MKV (Máxima calidad)</option>
        <option value="webm">WEBM (Nativo Web)</option>
      </select>
    </div>
    <div>
      <label for="quality">Calidad:</label>
      <select id="quality" onchange="generateCommand()">
        <option value="best">Máxima Disponible (4K/2K/1080p)</option>
        <option value="1080" selected>Full HD (1080p)</option>
        <option value="720">HD (720p)</option>
        <option value="480">480p</option>
      </select>
    </div>
  </div>

  <!-- OPCIONES DE DESCARGA AUDIO -->
  <div id="dlAudioBox" class="grid-2" style="display: none; margin-top: 10px;">
    <div>
      <label for="audioFormat">Formato de Audio:</label>
      <select id="audioFormat" onchange="generateCommand()">
        <option value="mp3" selected>MP3 (Universal)</option>
        <option value="m4a">M4A (Calidad original sin pérdida)</option>
      </select>
    </div>
  </div>

  <!-- SECCIÓN: UNIR / CONCATENAR LOCAL -->
  <div id="concatBox" class="action-box" style="display: none;">
    <h3 style="margin-top:0; color:#f9e2af; font-size:15px;">Unión Local de Archivos (Sin re-codificar)</h3>
    <div class="grid-2">
      <div>
        <label for="concatExt">Extensión de los archivos:</label>
        <select id="concatExt" onchange="generateCommand()">
          <option value="*.mp4" selected>Todos los .MP4 de la carpeta</option>
          <option value="*.mkv">Todos los .MKV de la carpeta</option>
          <option value="*.ts">Todos los .TS de la carpeta</option>
        </select>
      </div>
      <div>
        <label for="concatOutput">Nombre del archivo final:</label>
        <input type="text" id="concatOutput" value="Pelicula_Completa.mp4" oninput="generateCommand()">
      </div>
    </div>
    <div class="hint">Ejecuta el comando abriendo CMD directamente dentro de la carpeta donde están los videos.</div>
  </div>

  <!-- SECCIÓN: NORMALIZACIÓN -->
  <div id="normalizeBox" class="action-box" style="display: none;">
    <h3 style="margin-top:0; color:#f9e2af; font-size:15px;">Normalización de Audio (Reemplazo Automático)</h3>
    <div class="grid-3">
      <div>
        <label for="normScope">Alcance:</label>
        <select id="normScope" onchange="toggleNormInputs(); generateCommand();">
          <option value="all_mp4" selected>Todos los *.mp4 de la carpeta</option>
          <option value="single">Un solo archivo específico</option>
          <option value="batch_num">Lote numerado (Ej: trivia_01 a 06)</option>
        </select>
      </div>
      <div>
        <label for="normTarget">Objetivo LUFS:</label>
        <input type="number" id="normTarget" value="-26" step="1" oninput="generateCommand()">
      </div>
      <div>
        <label for="normOverwrite">Resultado:</label>
        <select id="normOverwrite" onchange="generateCommand()">
          <option value="replace" selected>Reemplazar original (Mismo nombre)</option>
          <option value="prefix">Guardar con prefijo norm_</option>
        </select>
      </div>
    </div>

    <div id="normSingleFile" style="display: none; margin-top: 10px;">
      <label for="normFileName">Nombre del archivo:</label>
      <input type="text" id="normFileName" value="trivia_01.mp4" oninput="generateCommand()">
    </div>

    <div id="normBatchInputs" class="grid-3" style="display: none; margin-top: 10px;">
      <div>
        <label for="normPrefix">Prefijo:</label>
        <input type="text" id="normPrefix" value="trivia_0" oninput="generateCommand()">
      </div>
      <div>
        <label for="normStart">Inicio:</label>
        <input type="number" id="normStart" value="1" oninput="generateCommand()">
      </div>
      <div>
        <label for="normEnd">Fin:</label>
        <input type="number" id="normEnd" value="6" oninput="generateCommand()">
      </div>
    </div>
  </div>

  <!-- SECCIÓN: INSERTAR / MEZCLAR AUDIO -->
  <div id="mergeBox" class="action-box" style="display: none;">
    <h3 style="margin-top:0; color:#f9e2af; font-size:15px;">Ensamblado de Audio</h3>
    <div class="grid-2">
      <div>
        <label for="localVideo">Video Base:</label>
        <input type="text" id="localVideo" value="trivia_01.mp4" oninput="generateCommand()">
      </div>
      <div>
        <label for="localAudio">Pista de Audio Local:</label>
        <input type="text" id="localAudio" value="musica.mp3" oninput="generateCommand()">
      </div>
    </div>
    <div class="grid-2">
      <div>
        <label for="audioMode">Modo:</label>
        <select id="audioMode" onchange="generateCommand()">
          <option value="replace" selected>Reemplazar audio del video</option>
          <option value="mix">Mezclar ambos audios</option>
        </select>
      </div>
      <div>
        <label for="mergeOverwrite">Resultado:</label>
        <select id="mergeOverwrite" onchange="generateCommand()">
          <option value="replace" selected>Sobrescribir Video Base (Mismo nombre)</option>
          <option value="new_file">Guardar en un archivo nuevo</option>
        </select>
      </div>
    </div>
  </div>

  <!-- SECCIÓN: REPARAR METADATOS -->
  <div id="fixMetaBox" class="action-box" style="display: none;">
    <h3 style="margin-top:0; color:#f9e2af; font-size:15px;">Reparación MP4 (faststart)</h3>
    <div class="grid-2">
      <div>
        <label for="fixScope">Alcance:</label>
        <select id="fixScope" onchange="toggleFixInputs(); generateCommand();">
          <option value="all_mp4" selected>Todos los *.mp4 de la carpeta</option>
          <option value="single">Un solo archivo</option>
        </select>
      </div>
      <div>
        <label for="fixFileName" id="fixFileLabel" style="display:none;">Nombre del archivo:</label>
        <input type="text" id="fixFileName" value="trivia_01.mp4" style="display:none;" oninput="generateCommand()">
      </div>
    </div>
  </div>

  <!-- OPCIONES DE TRAMOS PARA DESCARGAS -->
  <div class="grid-3" id="segmentsSection" style="margin-top: 15px;">
    <div>
      <label for="mode">Modo Descarga:</label>
      <select id="mode" onchange="toggleSegments(); generateCommand();">
        <option value="single" selected>Archivo Completo</option>
        <option value="segments">Por Tramos (Fraccionado)</option>
      </select>
    </div>
    <div>
      <label for="duration">Minutos por tramo:</label>
      <input type="number" id="duration" value="30" disabled oninput="generateCommand()">
    </div>
    <div>
      <label for="totalDuration">Duración Total Video:</label>
      <input type="text" id="totalDuration" value="01:00:00" placeholder="HH:MM:SS" disabled oninput="generateCommand()">
    </div>
  </div>

  <label for="finalOutput" style="margin-top: 20px;">Comando listo para CMD:</label>
  <textarea id="finalOutput" class="code" readonly></textarea>

  <button id="copyBtn" onclick="copyToClipboard()" style="background: #f9e2af; color: #11111b; margin-top: 15px;">
    📋 Copiar Comando al Portapapeles
  </button>
</div>

<script>
  async function pasteClipboard() {
    try {
      const text = await navigator.clipboard.readText();
      if (text) {
        document.getElementById('url').value = text.trim();
        generateCommand();
      }
    } catch (err) { alert('Concede permiso al navegador para acceder al portapapeles.'); }
  }

  function updateUI() {
    const action = document.getElementById('actionType').value;
    const isDlVideo = (action === 'dl_video');
    const isDlAudio = (action === 'dl_audio');
    const isDownload = isDlVideo || isDlAudio;

    document.getElementById('urlSection').style.display = isDownload ? 'block' : 'none';
    document.getElementById('playlistBox').style.display = isDownload ? 'block' : 'none';
    document.getElementById('dlVideoBox').style.display = isDlVideo ? 'grid' : 'none';
    document.getElementById('dlAudioBox').style.display = isDlAudio ? 'grid' : 'none';
    document.getElementById('segmentsSection').style.display = isDownload ? 'grid' : 'none';

    document.getElementById('concatBox').style.display = (action === 'concat_local') ? 'block' : 'none';
    document.getElementById('normalizeBox').style.display = (action === 'normalize') ? 'block' : 'none';
    document.getElementById('mergeBox').style.display = (action === 'merge_audio') ? 'block' : 'none';
    document.getElementById('fixMetaBox').style.display = (action === 'fix_meta') ? 'block' : 'none';
  }

  function toggleNormInputs() {
    const scope = document.getElementById('normScope').value;
    document.getElementById('normSingleFile').style.display = (scope === 'single') ? 'block' : 'none';
    document.getElementById('normBatchInputs').style.display = (scope === 'batch_num') ? 'grid' : 'none';
  }

  function toggleFixInputs() {
    const scope = document.getElementById('fixScope').value;
    const isSingle = (scope === 'single');
    document.getElementById('fixFileLabel').style.display = isSingle ? 'block' : 'none';
    document.getElementById('fixFileName').style.display = isSingle ? 'block' : 'none';
  }

  function toggleSegments() {
    const isSeg = (document.getElementById('mode').value === 'segments');
    document.getElementById('duration').disabled = !isSeg;
    document.getElementById('totalDuration').disabled = !isSeg;
  }

  function generateCommand() {
    const action = document.getElementById('actionType').value;
    const output = document.getElementById('finalOutput');

    // 1. UNIR / CONCATENAR ARCHIVOS LOCALES
    if (action === 'concat_local') {
      const ext = document.getElementById('concatExt').value;
      const outFile = document.getElementById('concatOutput').value.trim() || 'Pelicula_Completa.mp4';
      output.value = `(for %i in (${ext}) do @echo file '%i') > lista.txt && ffmpeg -f concat -safe 0 -i lista.txt -c copy "${outFile}"`;
      return;
    }

    // 2. NORMALIZACIÓN DE AUDIO
    if (action === 'normalize') {
      const scope = document.getElementById('normScope').value;
      const target = document.getElementById('normTarget').value || '-26';
      const replace = document.getElementById('normOverwrite').value === 'replace';

      if (scope === 'all_mp4') {
        output.value = replace 
          ? `for %f in (*.mp4) do (ffmpeg -i "%f" -af loudnorm=I=${target}:LRA=7:TP=-2 -c:v copy "temp_%f" && move /y "temp_%f" "%f")`
          : `for %f in (*.mp4) do ffmpeg -i "%f" -af loudnorm=I=${target}:LRA=7:TP=-2 -c:v copy "norm_%f"`;
      } else if (scope === 'single') {
        const file = document.getElementById('normFileName').value.trim() || 'video.mp4';
        output.value = replace
          ? `ffmpeg -i "${file}" -af loudnorm=I=${target}:LRA=7:TP=-2 -c:v copy "temp_${file}" && move /y "temp_${file}" "${file}"`
          : `ffmpeg -i "${file}" -af loudnorm=I=${target}:LRA=7:TP=-2 -c:v copy "norm_${file}"`;
      } else {
        const p = document.getElementById('normPrefix').value;
        const s = document.getElementById('normStart').value;
        const e = document.getElementById('normEnd').value;
        output.value = replace
          ? `for /L %i in (${s},1,${e}) do (ffmpeg -i ${p}%i.mp4 -af loudnorm=I=${target}:LRA=7:TP=-2 -c:v copy temp_${p}%i.mp4 && move /y temp_${p}%i.mp4 ${p}%i.mp4)`
          : `for /L %i in (${s},1,${e}) do ffmpeg -i ${p}%i.mp4 -af loudnorm=I=${target}:LRA=7:TP=-2 -c:v copy norm_${p}%i.mp4`;
      }
      return;
    }

    // 3. INSERTAR / MEZCLAR AUDIO
    if (action === 'merge_audio') {
      const v = document.getElementById('localVideo').value.trim() || 'video.mp4';
      const a = document.getElementById('localAudio').value.trim() || 'audio.mp3';
      const mode = document.getElementById('audioMode').value;
      const replace = document.getElementById('mergeOverwrite').value === 'replace';
      const targetOut = replace ? `temp_${v}` : `nuevo_${v}`;

      let cmd = (mode === 'replace')
        ? `ffmpeg -i "${v}" -i "${a}" -c:v copy -c:a aac -map 0:v:0 -map 1:a:0 -shortest "${targetOut}"`
        : `ffmpeg -i "${v}" -i "${a}" -filter_complex "[0:a][1:a]amix=inputs=2:duration=first[a]" -map 0:v -map "[a]" -c:v copy -c:a aac "${targetOut}"`;

      output.value = replace ? `${cmd} && move /y "${targetOut}" "${v}"` : cmd;
      return;
    }

    // 4. REPARAR METADATOS
    if (action === 'fix_meta') {
      const scope = document.getElementById('fixScope').value;
      if (scope === 'all_mp4') {
        output.value = `for %f in (*.mp4) do (ffmpeg -i "%f" -c copy -movflags +faststart "temp_%f" && move /y "temp_%f" "%f")`;
      } else {
        const f = document.getElementById('fixFileName').value.trim() || 'video.mp4';
        output.value = `ffmpeg -i "${f}" -c copy -movflags +faststart "temp_${f}" && move /y "temp_${f}" "${f}"`;
      }
      return;
    }

    // 5. DESCARGAS WEB (yt-dlp)
    const url = document.getElementById('url').value.trim() || 'URL_DEL_VIDEO';
    const isPlaylist = document.getElementById('isPlaylist').value === 'yes';
    const playlistMode = document.getElementById('playlistMode').value;

    let path = '%USERPROFILE%\\Downloads\\%(title)s.%(ext)s';
    let extraFlags = ' --retries 10 --fragment-retries 10';

    if (isPlaylist) {
      if (playlistMode === 'concat') {
        extraFlags += ' --concat-playlist always';
        path = '%USERPROFILE%\\Downloads\\%(playlist_title)s.%(ext)s';
      } else {
        path = '%USERPROFILE%\\Downloads\\%(playlist_index)s - %(title)s.%(ext)s';
      }
    }

    if (action === 'dl_video') {
      const fmt = document.getElementById('videoFormat').value;
      const q = document.getElementById('quality').value;
      const filter = (q === 'best') ? `"bv*[ext=${fmt}]+ba/b[ext=${fmt}]/best"` : `"bv*[height<=${q}][ext=${fmt}]+ba/b[height<=${q}]/best"`;
      output.value = `yt-dlp -f ${filter} --merge-output-format ${fmt}${extraFlags} -o "${path}" "${url}"`;
    } else {
      const afmt = document.getElementById('audioFormat').value;
      output.value = (afmt === 'mp3')
        ? `yt-dlp -x --audio-format mp3 --audio-quality 0${extraFlags} -o "${path}" "${url}"`
        : `yt-dlp -f "ba[ext=m4a]/ba" -x --audio-format m4a${extraFlags} -o "${path}" "${url}"`;
    }
  }

  function copyToClipboard() {
    const text = document.getElementById('finalOutput').value;
    if (!text) return;
    navigator.clipboard.writeText(text).then(() => {
      const btn = document.getElementById('copyBtn');
      btn.innerText = '¡Copiado con éxito! ✅';
      setTimeout(() => { btn.innerText = '📋 Copiar Comando al Portapapeles'; }, 2000);
    });
  }

  updateUI();
  generateCommand();
</script>
</body>
</html>
```

