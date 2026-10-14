# Bundle del Proyecto para IA: empaquetador_web

## Estructura de Archivos
- README.md
- estilo/estilo.css
- index.html
- js/app.js

---

## Código Fuente

### Archivo: `README.md`
```md
# empaquetador_web
```

### Archivo: `estilo/estilo.css`
```css
/* Tipografías y Scrollbars */
body { font-family: 'Inter', sans-serif; }
code, pre, textarea { font-family: 'JetBrains Mono', monospace; }
::-webkit-scrollbar { width: 8px; height: 8px; }
::-webkit-scrollbar-track { background: #0f172a; }
::-webkit-scrollbar-thumb { background: #334155; border-radius: 4px; }
::-webkit-scrollbar-thumb:hover { background: #475569; }

/* Selección de texto */
::selection { background-color: #6366f1; color: #ffffff; }

/* Emulación local de Clases de Utilidad de Tailwind CSS */
.min-h-screen { min-height: 100vh; }
.flex { display: flex; }
.flex-col { flex-direction: column; }
.flex-wrap { flex-wrap: wrap; }
.items-center { align-items: center; }
.justify-between { justify-content: space-between; }
.justify-center { justify-content: center; }
.justify-start { justify-content: flex-start; }
.shrink-0 { flex-shrink: 0; }
.flex-1 { flex: 1 1 0%; }

.grid { display: grid; }
.grid-cols-1 { grid-template-columns: repeat(1, minmax(0, 1fr)); }
@media (min-width: 1024px) {
  .lg\:grid-cols-12 { grid-template-columns: repeat(12, minmax(0, 1fr)); }
  .lg\:col-span-4 { grid-column: span 4 / span 4; }
  .lg\:col-span-8 { grid-column: span 8 / span 8; }
}

.max-w-7xl { max-width: 80rem; }
.mx-auto { margin-left: auto; margin-right: auto; }
.max-w-xl { max-width: 36rem; }
.max-w-md { max-width: 28rem; }

.w-full { width: 100%; }
.h-full { height: 100%; }
.h-16 { height: 4rem; }
.w-10 { width: 2.5rem; }
.h-10 { height: 2.5rem; }
.w-12 { width: 3rem; }
.h-12 { height: 3rem; }
.w-16 { width: 4rem; }
.h-16 { height: 4rem; }
.w-4 { width: 1rem; }
.h-4 { height: 1rem; }
.w-64 { width: 16rem; }
.h-64 { height: 16rem; }

.min-h-\[220px\] { min-height: 220px; }
.max-h-\[300px\] { max-height: 300px; }
.max-h-\[240px\] { max-height: 240px; }
.min-h-\[520px\] { min-height: 520px; }

.p-6 { padding: 1.5rem; }
.p-5 { padding: 1.25rem; }
.p-4 { padding: 1rem; }
.p-3 { padding: 0.75rem; }
.p-3\.5 { padding: 0.875rem; }
.p-1 { padding: 0.25rem; }
.p-12 { padding: 3rem; }
.py-6 { padding-top: 1.5rem; padding-bottom: 1.5rem; }
.py-3 { padding-top: 0.75rem; padding-bottom: 0.75rem; }
.py-2 { padding-top: 0.5rem; padding-bottom: 0.5rem; }
.py-1 { padding-top: 0.25rem; padding-bottom: 0.25rem; }
.py-1\.5 { padding-top: 0.375rem; padding-bottom: 0.375rem; }
.px-4 { padding-left: 1rem; padding-right: 1rem; }
.px-5 { padding-left: 1.25rem; padding-right: 1.25rem; }
.px-3 { padding-left: 0.75rem; padding-right: 0.75rem; }
.px-3\.5 { padding-left: 0.875rem; padding-right: 0.875rem; }
.px-2 { padding-left: 0.5rem; padding-right: 0.5rem; }
.pl-3 { padding-left: 0.75rem; }
.pl-8 { padding-left: 2rem; }
.pr-3 { padding-right: 0.75rem; }

.mt-6 { margin-top: 1.5rem; }
.mt-4 { margin-top: 1rem; }
.mb-2 { margin-bottom: 0.5rem; }

.space-x-3 > * + * { margin-left: 0.75rem; }
.space-x-2 > * + * { margin-left: 0.5rem; }
.space-x-1 > * + * { margin-left: 0.25rem; }
.space-x-1\.5 > * + * { margin-left: 0.375rem; }
.space-x-2\.5 > * + * { margin-left: 0.625rem; }
.space-y-1 > * + * { margin-top: 0.25rem; }
.space-y-2 > * + * { margin-top: 0.5rem; }
.gap-6 { gap: 1.5rem; }
.gap-3 { gap: 0.75rem; }
.gap-4 { gap: 1rem; }
.gap-2 { gap: 0.5rem; }

@media (min-width: 640px) {
  .sm\:px-6 { padding-left: 1.5rem; padding-right: 1.5rem; }
  .sm\:flex-row { flex-direction: row; }
  .sm\:items-center { align-items: center; }
}
@media (min-width: 768px) {
  .md\:flex-row { flex-direction: row; }
  .md\:text-left { text-align: left; }
  .md\:justify-start { justify-content: flex-start; }
  .md\:w-auto { width: auto; }
}
@media (min-width: 1024px) {
  .lg\:px-8 { padding-left: 2rem; padding-right: 2rem; }
}

.bg-dark-950 { background-color: #030712; }
.bg-dark-900 { background-color: #0f172a; }
.bg-dark-900\/90 { background-color: rgba(15, 23, 42, 0.9); }
.bg-slate-950 { background-color: #030712; }
.bg-slate-950\/60 { background-color: rgba(3, 7, 18, 0.6); }
.bg-slate-900 { background-color: #0f172a; }
.bg-slate-900\/50 { background-color: rgba(15, 23, 42, 0.5); }
.bg-slate-800 { background-color: #1e293b; }
.hover\:bg-slate-700:hover { background-color: #334155; }
.hover\:bg-slate-900:hover { background-color: #0f172a; }
.bg-indigo-600 { background-color: #4f46e5; }
.hover\:bg-indigo-500:hover { background-color: #6366f1; }
.bg-violet-600 { background-color: #7c3aed; }
.hover\:bg-violet-500:hover { background-color: #8b5cf6; }
.bg-emerald-600 { background-color: #059669; }
.hover\:bg-emerald-500:hover { background-color: #10b981; }

.text-slate-100 { color: #f1f5f9; }
.text-slate-200 { color: #e2e8f0; }
.text-slate-300 { color: #cbd5e1; }
.text-slate-400 { color: #94a3b8; }
.text-slate-500 { color: #64748b; }
.text-slate-700 { color: #334155; }
.text-white { color: #ffffff; }
.text-indigo-400 { color: #818cf8; }
.text-rose-400 { color: #fb7185; }
.hover\:text-rose-300:hover { color: #fda4af; }
.hover\:text-rose-400:hover { color: #fb7185; }
.hover\:text-slate-300:hover { color: #cbd5e1; }
.hover\:text-white:hover { color: #ffffff; }
.hover\:text-indigo-300:hover { color: #a5b4fc; }

.border { border-width: 1px; border-style: solid; }
.border-b { border-bottom-width: 1px; border-bottom-style: solid; }
.border-2 { border-width: 2px; }
.border-dashed { border-style: dashed; }
.border-slate-800 { border-color: #1e293b; }
.border-slate-800\/80 { border-color: rgba(30, 41, 59, 0.8); }
.border-slate-700 { border-color: #334155; }
.border-indigo-500 { border-color: #6366f1; }
.hover\:border-slate-700:hover { border-color: #334155; }
.focus\:border-indigo-500:focus { border-color: #6366f1; }

.rounded-xl { border-radius: 0.75rem; }
.rounded-2xl { border-radius: 1rem; }
.rounded-full { border-radius: 9999px; }
.rounded-lg { border-radius: 0.5rem; }
.rounded { border-radius: 0.25rem; }

.shadow-lg { box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -4px rgba(0, 0, 0, 0.1); }
.shadow-xl { box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1); }
.shadow-sm { box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05); }
.shadow { box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px -1px rgba(0, 0, 0, 0.1); }
.shadow-inner { box-shadow: inset 0 2px 4px 0 rgba(0, 0, 0, 0.06); }
.shadow-indigo-500\/20 { box-shadow: 0 10px 15px -3px rgba(99, 102, 241, 0.2); }
.shadow-indigo-600\/30 { box-shadow: 0 10px 15px -3px rgba(79, 70, 229, 0.3); }
.shadow-violet-600\/30 { box-shadow: 0 10px 15px -3px rgba(124, 58, 237, 0.3); }
.shadow-emerald-600\/20 { box-shadow: 0 10px 15px -3px rgba(5, 150, 105, 0.2); }

.font-bold { font-weight: 700; }
.font-medium { font-weight: 500; }
.font-semibold { font-weight: 600; }
.text-lg { font-size: 1.125rem; line-height: 1.75rem; }
.text-xl { font-size: 1.25rem; line-height: 1.75rem; }
.text-xs { font-size: 0.75rem; line-height: 1rem; }
.text-sm { font-size: 0.875rem; line-height: 1.25rem; }
.text-\[10px\] { font-size: 10px; }
.tracking-tight { letter-spacing: -0.025em; }
.uppercase { text-transform: uppercase; }
.tracking-wider { letter-spacing: 0.05em; }
.font-mono { font-family: 'JetBrains Mono', monospace; }
.truncate { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }

.bg-gradient-to-tr { background-image: linear-gradient(to top right, var(--tw-gradient-stops)); }
.bg-gradient-to-r { background-image: linear-gradient(to right, var(--tw-gradient-stops)); }
.from-indigo-600 { --tw-gradient-from: #4f46e5; --tw-gradient-stops: var(--tw-gradient-from), var(--tw-gradient-to, rgba(79, 70, 229, 0)); }
.via-violet-600 { --tw-gradient-to: rgba(124, 58, 237, 0); --tw-gradient-stops: var(--tw-gradient-from), #7c3aed, var(--tw-gradient-to); }
.to-pink-600 { --tw-gradient-to: #db2777; }
.from-white { --tw-gradient-from: #ffffff; --tw-gradient-stops: var(--tw-gradient-from), var(--tw-gradient-to, rgba(255, 255, 255, 0)); }
.via-slate-200 { --tw-gradient-to: rgba(226, 232, 240, 0); --tw-gradient-stops: var(--tw-gradient-from), #e2e8f0, var(--tw-gradient-to); }
.to-indigo-300 { --tw-gradient-to: #a5b4fc; }
.from-indigo-600 { --tw-gradient-from: #4f46e5; }
.to-violet-600 { --tw-gradient-to: #7c3aed; }
.hover\:from-indigo-500:hover { --tw-gradient-from: #6366f1; }
.hover\:to-violet-500:hover { --tw-gradient-to: #8b5cf6; }

.bg-clip-text { -webkit-background-clip: text; background-clip: text; }
.text-transparent { color: transparent; }

.sticky { position: sticky; }
.top-0 { top: 0px; }
.z-50 { z-index: 50; }
.relative { position: relative; }
.absolute { position: absolute; }
.inset-0 { inset: 0px; }
.inset-y-0 { top: 0px; bottom: 0px; }
.left-0 { left: 0px; }
.-right-16 { right: -4rem; }
.-top-16 { top: -4rem; }

.overflow-hidden { overflow: hidden; }
.overflow-y-auto { overflow-y: auto; }
.blur-3xl { filter: blur(64px); }
.pointer-events-none { pointer-events: none; }
.group:hover .group-hover\:bg-indigo-500\/20 { background-color: rgba(99, 102, 241, 0.2); }
.group:hover .group-hover\:opacity-100 { opacity: 1; }

.transition { transition-property: color, background-color, border-color, text-decoration-color, fill, stroke, opacity, box-shadow, transform; transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1); transition-duration: 150ms; }
.transition-all { transition-property: all; transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1); transition-duration: 150ms; }
.duration-700 { transition-duration: 700ms; }
.backdrop-blur { backdrop-filter: blur(8px); }
.cursor-pointer { cursor: pointer; }
.hidden { display: none; }
.opacity-80 { opacity: 0.8; }
.resize-none { resize: none; }
.focus\:outline-none:focus { outline: 2px solid transparent; outline-offset: 2px; }
.focus\:ring-indigo-500:focus { --tw-ring-color: #6366f1; }
```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Code Bundler — Empaquetador para IA</title>
    <!-- Alpine.js -->
    <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
    <!-- JSZip para descompresión de archivos .zip -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
    <!-- FontAwesome Iconos -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter y JetBrains Mono -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
    <!-- Estilos locales que emulan las clases de utilidad -->
    <link rel="stylesheet" href="estilo/estilo.css">
</head>
<body class="bg-dark-950 text-slate-100 min-h-screen flex flex-col selection:bg-indigo-500 selection:text-white" x-data="empaquetadorApp()" x-init="inicializar()">

    <!-- Barra de Navegación -->
    <header class="border-b border-slate-800 bg-dark-900/90 backdrop-blur sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-indigo-600 via-violet-600 to-pink-600 flex items-center justify-center shadow-lg shadow-indigo-500/20">
                    <i class="fa-solid fa-file-code text-white text-lg"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg tracking-tight bg-gradient-to-r from-white via-slate-200 to-indigo-300 bg-clip-text text-transparent">AI Code Bundler</h1>
                    <p class="text-xs text-slate-400">Unifica tu repositorio en un único Markdown optimizado para IA</p>
                </div>
            </div>
            <div class="flex items-center space-x-3">
                <button @click="cargarProyectoDemo()" class="text-xs px-3.5 py-2 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 transition flex items-center space-x-2 shadow-sm cursor-pointer">
                    <i class="fa-solid fa-wand-magic-sparkles text-indigo-400"></i>
                    <span>Cargar Demo</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Zona Principal -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 flex flex-col gap-6">

        <!-- Barra de Importación -->
        <section class="bg-dark-900 border border-slate-800 rounded-2xl p-6 shadow-xl relative overflow-hidden group">
            <div class="absolute -right-16 -top-16 w-64 h-64 bg-indigo-500/10 rounded-full blur-3xl pointer-events-none group-hover:bg-indigo-500/20 transition-all duration-700"></div>
            
            <div class="flex flex-col md:flex-row items-center justify-between gap-6 relative z-10">
                <div class="space-y-1 text-center md:text-left">
                    <h2 class="text-xl font-bold text-white flex items-center justify-center md:justify-start gap-2">
                        <i class="fa-solid fa-cloud-arrow-up text-indigo-400"></i>
                        <span>Importar Proyecto (Carpeta, ZIP o Archivos)</span>
                    </h2>
                    <p class="text-sm text-slate-400 max-w-xl">
                        Sube la carpeta de tu código fuente o un .zip para unificarlo ordenadamente.
                    </p>
                </div>

                <div class="flex flex-wrap items-center justify-center gap-3 w-full md:w-auto">
                    <label class="cursor-pointer bg-indigo-600 hover:bg-indigo-500 text-white font-medium px-5 py-3 rounded-xl shadow-lg shadow-indigo-600/30 transition flex items-center space-x-2 text-sm">
                        <i class="fa-solid fa-folder-tree"></i>
                        <span>Subir Carpeta</span>
                        <input type="file" webkitdirectory directory multiple class="hidden" @change="manejarSeleccionCarpeta($event)">
                    </label>
                    <label class="cursor-pointer bg-violet-600 hover:bg-violet-500 text-white font-medium px-5 py-3 rounded-xl shadow-lg shadow-violet-600/30 transition flex items-center space-x-2 text-sm">
                        <i class="fa-solid fa-file-zipper"></i>
                        <span>Subir .ZIP</span>
                        <input type="file" accept=".zip" class="hidden" @change="manejarSeleccionZip($event)">
                    </label>
                </div>
            </div>

            <!-- Zona Drag & Drop -->
            <div 
                @dragover.prevent="estaArrastrando = true" 
                @dragleave.prevent="estaArrastrando = false" 
                @drop.prevent="manejarSoltado($event)"
                :class="estaArrastrando ? 'border-indigo-500 bg-indigo-500/10' : 'border-slate-800 hover:border-slate-700'"
                class="mt-6 border-2 border-dashed rounded-xl p-6 text-center transition flex flex-col items-center justify-center cursor-pointer">
                <div class="w-12 h-12 rounded-full bg-slate-800 flex items-center justify-center text-slate-400 mb-2">
                    <i class="fa-solid fa-arrow-up-from-bracket text-lg text-indigo-400"></i>
                </div>
                <p class="text-sm font-medium text-slate-300">Arrastra tu carpeta o archivo .zip aquí</p>
            </div>
        </section>

        <!-- Panel de Control y Vista de Archivos -->
        <template x-if="obtenerCantidadArchivos() > 0">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                
                <!-- Columna Izquierda: Controles y Árbol (4 cols) -->
                <div class="bg-dark-900 border border-slate-800 rounded-2xl p-5 shadow-xl flex flex-col gap-4 lg:col-span-4">
                    <div class="flex items-center justify-between border-b border-slate-800 pb-3">
                        <div class="flex items-center space-x-2">
                            <i class="fa-solid fa-folder-open text-indigo-400"></i>
                            <h3 class="font-semibold text-white text-sm">Archivos Detectados (<span x-text="obtenerCantidadArchivos()"></span>)</h3>
                        </div>
                        <button @click="limpiarTodo()" class="text-xs text-rose-400 hover:text-rose-300 transition flex items-center space-x-1 cursor-pointer">
                            <i class="fa-solid fa-trash-can"></i>
                            <span>Limpiar</span>
                        </button>
                    </div>

                    <!-- Opciones de Generación para IA -->
                    <div class="space-y-2 bg-slate-950/60 p-3.5 rounded-xl border border-slate-800/80">
                        <h4 class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Opciones del Bundle</h4>
                        <label class="flex items-center space-x-2.5 text-xs text-slate-300 cursor-pointer">
                            <input type="checkbox" x-model="opciones.excluirBinarios" class="w-4 h-4 rounded bg-slate-900 border-slate-700 text-indigo-600 focus:ring-indigo-500">
                            <span>Excluir imágenes/binarios (Ahorra tokens)</span>
                        </label>
                    </div>

                    <!-- Botón Generar Bundle -->
                    <button @click="generarBundleParaIa()" class="w-full bg-gradient-to-r from-indigo-600 to-violet-600 hover:from-indigo-500 hover:to-violet-500 text-white font-medium py-3 rounded-xl shadow-lg shadow-indigo-600/30 transition flex items-center justify-center space-x-2 text-sm cursor-pointer">
                        <i class="fa-solid fa-file-lines"></i>
                        <span>Generar Bundle Markdown para IA</span>
                    </button>

                    <!-- Árbol Interactivo con Buscador -->
                    <div class="flex-1 flex flex-col gap-2 min-h-[220px]">
                        <div class="flex items-center justify-between text-xs text-slate-400 font-semibold uppercase tracking-wider">
                            <span>Estructura</span>
                        </div>
                        <div class="relative">
                            <span class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none text-slate-500 text-xs">
                                <i class="fa-solid fa-search"></i>
                            </span>
                            <input type="text" x-model="filtroArbol" placeholder="Buscar archivo..." class="w-full bg-slate-950 border border-slate-800 rounded-xl pl-8 pr-3 py-1.5 text-xs text-slate-200 focus:outline-none focus:border-indigo-500 font-mono">
                        </div>

                        <div class="flex-1 bg-slate-950 border border-slate-800 rounded-xl p-3 overflow-y-auto max-h-[240px] space-y-1 font-mono text-xs">
                            <template x-for="(nodo, ruta) in filtrarArbol()" :key="ruta">
                                <div class="flex items-center justify-between py-1 px-2 rounded hover:bg-slate-900 group transition">
                                    <div class="flex items-center space-x-2 overflow-hidden cursor-pointer" @click="seleccionarArchivo(ruta)">
                                        <i :class="obtenerIconoNodo(nodo)" class="shrink-0"></i>
                                        <span class="truncate text-slate-300" :class="archivoSeleccionado === ruta ? 'text-indigo-400 font-bold' : ''" x-text="ruta"></span>
                                    </div>
                                    <div class="flex items-center space-x-2 opacity-80 group-hover:opacity-100">
                                        <span class="text-slate-500 text-[10px]" x-text="formatearBytes(nodo.tamano)"></span>
                                        <button @click="eliminarArchivo(ruta)" class="text-slate-500 hover:text-rose-400 p-1 cursor-pointer"><i class="fa-solid fa-xmark"></i></button>
                                    </div>
                                </div>
                            </template>
                        </div>
                    </div>
                </div>

                <!-- Columna Derecha: Editor / Resultado Markdown / Logs (8 cols) -->
                <div class="bg-dark-900 border border-slate-800 rounded-2xl p-5 shadow-xl flex flex-col lg:col-span-8">
                    
                    <!-- Pestañas de Navegación -->
                    <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between border-b border-slate-800 pb-4 gap-4">
                        <div class="flex items-center space-x-1 bg-slate-950 p-1 rounded-xl border border-slate-800">
                            <button @click="pestanaActiva = 'bundle'" :class="pestanaActiva === 'bundle' ? 'bg-indigo-600 text-white shadow' : 'text-slate-400 hover:text-white'" class="px-4 py-2 rounded-lg text-xs font-medium transition flex items-center space-x-2 cursor-pointer">
                                <i class="fa-solid fa-file-arrow-down"></i>
                                <span>Bundle para IA (.md)</span>
                            </button>
                            <button @click="pestanaActiva = 'editor'" :class="pestanaActiva === 'editor' ? 'bg-indigo-600 text-white shadow' : 'text-slate-400 hover:text-white'" class="px-4 py-2 rounded-lg text-xs font-medium transition flex items-center space-x-2 cursor-pointer">
                                <i class="fa-solid fa-code"></i>
                                <span>Editor (<span x-text="archivoSeleccionado || 'Ninguno'"></span>)</span>
                            </button>
                            <button @click="pestanaActiva = 'registros'" :class="pestanaActiva === 'registros' ? 'bg-indigo-600 text-white shadow' : 'text-slate-400 hover:text-white'" class="px-4 py-2 rounded-lg text-xs font-medium transition flex items-center space-x-2 cursor-pointer">
                                <i class="fa-solid fa-terminal"></i>
                                <span>Registros</span>
                            </button>
                        </div>

                        <!-- Botones de Exportación -->
                        <template x-if="bundleAi">
                            <div class="flex items-center space-x-2">
                                <button @click="copiarCodigo()" class="bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 px-3 py-2 rounded-xl text-xs font-medium transition flex items-center space-x-1.5 cursor-pointer">
                                    <i class="fa-solid fa-copy text-indigo-400"></i>
                                    <span x-text="copiado ? '¡Copiado!' : 'Copiar Markdown'"></span>
                                </button>
                                <button @click="descargarBundle()" class="bg-emerald-600 hover:bg-emerald-500 text-white px-4 py-2 rounded-xl text-xs font-medium shadow-lg shadow-emerald-600/20 transition flex items-center space-x-1.5 cursor-pointer">
                                    <i class="fa-solid fa-download"></i>
                                    <span>Descargar .md</span>
                                </button>
                            </div>
                        </template>
                    </div>

                    <!-- Contenido de las Pestañas -->
                    <div class="flex-1 mt-4 flex flex-col min-h-[520px]">
                        
                        <!-- Pestaña 1: Bundle Markdown para IA -->
                        <div x-show="pestanaActiva === 'bundle'" class="flex-1 flex flex-col gap-3">
                            <div class="flex items-center justify-between text-xs text-slate-400 px-1">
                                <span x-text="bundleAi ? 'Tamaño del bundle: ' + formatearBytes(bundleAi.length) : 'Sin bundle generado'"></span>
                                <span class="text-slate-500">Formato listo para arrastrar y soltar en tu IA</span>
                            </div>
                            <div class="flex-1 bg-slate-950 rounded-xl border border-slate-800 overflow-hidden relative">
                                <template x-if="!bundleAi">
                                    <div class="absolute inset-0 flex flex-col items-center justify-center p-8 text-center space-y-2">
                                        <i class="fa-solid fa-file-lines text-4xl text-slate-700"></i>
                                        <p class="text-sm text-slate-400">Haz clic en <strong class="text-indigo-400">"Generar Bundle Markdown para IA"</strong> para unificar tu código.</p>
                                    </div>
                                </template>
                                <textarea readonly x-model="bundleAi" class="w-full h-full bg-slate-950 text-slate-300 p-4 text-xs resize-none focus:outline-none font-mono" placeholder="El código unificado aparecerá aquí..."></textarea>
                            </div>
                        </div>

                        <!-- Pestaña 2: Editor de Código -->
                        <div x-show="pestanaActiva === 'editor'" class="flex-1 flex flex-col gap-3">
                            <template x-if="!archivoSeleccionado">
                                <div class="flex-1 flex flex-col items-center justify-center text-center p-8 space-y-2">
                                    <i class="fa-solid fa-file-code text-4xl text-slate-700"></i>
                                    <p class="text-sm text-slate-400">Selecciona un archivo del árbol de la izquierda para editarlo.</p>
                                </div>
                            </template>
                            <template x-if="archivoSeleccionado">
                                <div class="flex-1 flex flex-col gap-2">
                                    <div class="flex items-center justify-between text-xs text-slate-400 px-1">
                                        <span class="font-mono text-indigo-400" x-text="archivoSeleccionado"></span>
                                        <button @click="guardarArchivoActual()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-3 py-1 rounded-lg text-xs transition cursor-pointer">Guardar Cambios</button>
                                    </div>
                                    <textarea x-model="contenidoEditor" class="flex-1 w-full bg-slate-950 text-slate-200 p-4 text-xs rounded-xl border border-slate-800 focus:outline-none focus:border-indigo-500 resize-none font-mono"></textarea>
                                </div>
                            </template>
                        </div>

                        <!-- Pestaña 3: Registros -->
                        <div x-show="pestanaActiva === 'registros'" class="flex-1 flex flex-col gap-3">
                            <div class="flex items-center justify-between text-xs text-slate-400 px-1">
                                <span>Historial de Procesamiento</span>
                                <button @click="registros = []" class="text-slate-500 hover:text-slate-300 cursor-pointer">Limpiar</button>
                            </div>
                            <div class="flex-1 bg-slate-950 rounded-xl border border-slate-800 p-4 overflow-y-auto space-y-2 font-mono text-xs">
                                <template x-for="reg in registros" :key="reg.id">
                                    <div class="flex items-start space-x-2 border-b border-slate-900 pb-1.5">
                                        <span class="text-slate-500 text-[10px]" x-text="reg.tiempo"></span>
                                        <span class="font-bold uppercase text-[10px] px-1.5 py-0.5 rounded bg-slate-900 text-indigo-400" x-text="reg.tipo"></span>
                                        <span class="text-slate-300 flex-1" x-text="reg.mensaje"></span>
                                    </div>
                                </template>
                            </div>
                        </div>

                    </div>
                </div>

            </div>
        </template>

        <!-- Estado Vacío Inicial -->
        <template x-if="obtenerCantidadArchivos() === 0">
            <div class="bg-dark-900/50 border border-slate-800/80 rounded-2xl p-12 text-center flex flex-col items-center justify-center space-y-4">
                <div class="w-16 h-16 rounded-2xl bg-slate-800/80 flex items-center justify-center text-indigo-400 shadow-inner">
                    <i class="fa-solid fa-laptop-code text-2xl"></i>
                </div>
                <div class="max-w-md space-y-1">
                    <h3 class="text-lg font-semibold text-white">Ningún proyecto cargado</h3>
                    <p class="text-sm text-slate-400">
                        Sube tu carpeta de código o haz clic en <button @click="cargarProyectoDemo()" class="text-indigo-400 underline hover:text-indigo-300 cursor-pointer">Cargar Demo</button> para probar.
                    </p>
                </div>
            </div>
        </template>

    </main>

    <!-- Script JS externo -->
    <script src="js/app.js"></script>
</body>
</html>
```

### Archivo: `js/app.js`
```javascript
function empaquetadorApp() {
    return {
        archivos: {},
        arbolArchivos: {},
        archivoSeleccionado: null,
        contenidoEditor: '',
        filtroArbol: '',
        opciones: {
            excluirBinarios: true
        },
        pestanaActiva: 'bundle',
        bundleAi: '',
        registros: [],
        estaArrastrando: false,
        copiado: false,

        inicializar() {
            this.agregarRegistro('AI Code Bundler inicializado correctamente.', 'info');
        },

        obtenerCantidadArchivos() {
            return Object.keys(this.archivos).length;
        },

        agregarRegistro(mensaje, tipo = 'info') {
            const ahora = new Date();
            const tiempo = ahora.toTimeString().split(' ')[0] + '.' + String(ahora.getMilliseconds()).padStart(3, '0');
            this.registros.push({ id: Math.random(), tiempo, mensaje, tipo });
        },

        formatearBytes(bytes, decimales = 2) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024;
            const dm = decimales < 0 ? 0 : decimales;
            const tamanos = ['Bytes', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + ' ' + tamanos[i];
        },

        obtenerIconoNodo(nodo) {
            const ruta = nodo.ruta || '';
            if (ruta.endsWith('.html')) return 'fa-solid fa-file-code text-orange-400';
            if (ruta.endsWith('.css')) return 'fa-solid fa-file-css text-cyan-400';
            if (ruta.endsWith('.js')) return 'fa-solid fa-file-lines text-yellow-400';
            if (/\.(png|jpg|jpeg|gif|webp|svg|ico)$/i.test(ruta)) return 'fa-solid fa-file-image text-emerald-400';
            return 'fa-solid fa-file text-slate-400';
        },

        // Filtro para excluir carpetas y archivos basura del sistema (node_modules, .git, etc.)
        esArchivoIgnorable(ruta) {
            const patronesIgnorados = /(^|\/)(node_modules|\.git|\.next|dist|build|\.DS_Store|__MACOSX)(\/|$)/i;
            return patronesIgnorados.test(ruta);
        },

        filtrarArbol() {
            if (!this.filtroArbol) return this.arbolArchivos;
            const filtrado = {};
            for (const [ruta, nodo] of Object.entries(this.arbolArchivos)) {
                if (ruta.toLowerCase().includes(this.filtroArbol.toLowerCase())) {
                    filtrado[ruta] = nodo;
                }
            }
            return filtrado;
        },

        async manejarSeleccionCarpeta(evento) {
            const listaArchivos = evento.target.files;
            if (!listaArchivos || listaArchivos.length === 0) return;
            await this.procesarListaArchivos(listaArchivos);
            evento.target.value = '';
        },

        async manejarSeleccionZip(evento) {
            const archivo = evento.target.files[0];
            if (!archivo) return;
            this.agregarRegistro(`Descomprimiendo archivo ZIP: ${archivo.name}...`, 'info');
            try {
                const zip = new JSZip();
                const contenidoZip = await zip.loadAsync(archivo);
                const tempArchivos = { ...this.archivos };

                for (const [rutaRelativa, entradaZip] of Object.entries(contenidoZip.files)) {
                    if (entradaZip.dir) continue;
                    
                    const rutaLimpia = rutaRelativa.startsWith('/') ? rutaRelativa.substring(1) : rutaRelativa;
                    if (this.esArchivoIgnorable(rutaLimpia)) continue;

                    const esBinario = /\.(png|jpg|jpeg|gif|webp|ico|woff|woff2|ttf|eot|pdf|zip)$/i.test(rutaLimpia);
                    let contenido = esBinario ? '[Archivo Binario]' : await entradaZip.async('text');

                    tempArchivos[rutaLimpia] = {
                        contenido: contenido,
                        tamano: contenido.length,
                        esBinario: esBinario
                    };
                    this.agregarRegistro(`Extraído del ZIP: ${rutaLimpia}`, 'exito');
                }

                this.archivos = tempArchivos;
                this.finalizarProcesamiento();
            } catch (err) {
                this.agregarRegistro(`Error al procesar el archivo ZIP: ${err.message}`, 'error');
            }
            evento.target.value = '';
        },

        async manejarSoltado(evento) {
            this.estaArrastrando = false;
            const archivos = evento.dataTransfer.files;
            if (archivos.length === 1 && archivos[0].name.endsWith('.zip')) {
                await this.manejarSeleccionZip({ target: { files: archivos } });
                return;
            }
            const listaArchivos = [];
            const items = evento.dataTransfer.items;
            if (items) {
                for (let i = 0; i < items.length; i++) {
                    const item = items[i].webkitGetAsEntry ? items[i].webkitGetAsEntry() : null;
                    if (item) {
                        await this.recorrerArbolArchivos(item, '', listaArchivos);
                    } else {
                        const f = items[i].getAsFile();
                        if (f) listaArchivos.push(f);
                    }
                }
            }
            if (listaArchivos.length > 0) {
                await this.procesarListaArchivos(listaArchivos);
            }
        },

        async recorrerArbolArchivos(item, ruta, listaArchivos) {
            if (item.isFile) {
                await new Promise((resolver) => {
                    item.file(archivo => {
                        archivo.fullPath = ruta + archivo.name;
                        listaArchivos.push(archivo);
                        resolver();
                    });
                });
            } else if (item.isDirectory) {
                const lectorDir = item.createReader();
                await new Promise((resolver) => {
                    lectorDir.readEntries(async entradas => {
                        for (let i = 0; i < entradas.length; i++) {
                            await this.recorrerArbolArchivos(entradas[i], ruta + item.name + '/', listaArchivos);
                        }
                        resolver();
                    });
                });
            }
        },

        async procesarListaArchivos(listaArchivos) {
            this.agregarRegistro(`Procesando ${listaArchivos.length} archivo(s)...`, 'info');
            const tempArchivos = { ...this.archivos };
            for (let i = 0; i < listaArchivos.length; i++) {
                const archivo = listaArchivos[i];
                let ruta = archivo.webkitRelativePath || archivo.fullPath || archivo.name;
                ruta = ruta.startsWith('/') ? ruta.substring(1) : ruta;

                if (this.esArchivoIgnorable(ruta)) continue;

                const esBinario = /\.(png|jpg|jpeg|gif|webp|ico|woff|woff2|ttf|eot|pdf|zip)$/i.test(ruta);
                let contenido = '';
                if (esBinario) {
                    contenido = '[Archivo Binario Omitido]';
                } else {
                    contenido = await this.leerComoTexto(archivo);
                }

                tempArchivos[ruta] = {
                    contenido: contenido,
                    tamano: archivo.size,
                    esBinario: esBinario
                };
                this.agregarRegistro(`Cargado: ${ruta}`, 'exito');
            }
            this.archivos = tempArchivos;
            this.finalizarProcesamiento();
        },

        finalizarProcesamiento() {
            const rutas = Object.keys(this.archivos).sort();
            const tempArbol = {};
            for (const ruta of rutas) {
                tempArbol[ruta] = { ruta: ruta, tamano: this.archivos[ruta].tamano };
            }
            this.arbolArchivos = tempArbol;

            if (!this.archivoSeleccionado && rutas.length > 0) {
                this.seleccionarArchivo(rutas[0]);
            }
            this.archivos = { ...this.archivos };
            this.arbolArchivos = { ...this.arbolArchivos };
            this.agregarRegistro('Estructura de archivos lista.', 'exito');
            this.generarBundleParaIa();
        },

        leerComoTexto(archivo) {
            return new Promise((resolver, rechazar) => {
                const lector = new FileReader();
                lector.onload = e => resolver(e.target.result);
                lector.onerror = e => rechazar(e);
                lector.readAsText(archivo);
            });
        },

        seleccionarArchivo(ruta) {
            this.archivoSeleccionado = ruta;
            const datosArchivo = this.archivos[ruta];
            if (datosArchivo && !datosArchivo.esBinario) {
                this.contenidoEditor = datosArchivo.contenido;
            } else {
                this.contenidoEditor = '[Archivo binario o no editable]';
            }
            this.pestanaActiva = 'editor';
        },

        guardarArchivoActual() {
            if (!this.archivoSeleccionado) return;
            const datosArchivo = this.archivos[this.archivoSeleccionado];
            if (datosArchivo && !datosArchivo.esBinario) {
                datosArchivo.contenido = this.contenidoEditor;
                datosArchivo.tamano = this.contenidoEditor.length;
                this.agregarRegistro(`Guardados cambios en: ${this.archivoSeleccionado}`, 'exito');
                this.generarBundleParaIa();
            }
        },

        eliminarArchivo(ruta) {
            const tempArchivos = { ...this.archivos };
            delete tempArchivos[ruta];
            this.archivos = tempArchivos;
            this.finalizarProcesamiento();
            if (this.archivoSeleccionado === ruta) {
                this.archivoSeleccionado = null;
                this.contenidoEditor = '';
            }
            this.agregarRegistro(`Archivo eliminado: ${ruta}`, 'info');
        },

        limpiarTodo() {
            this.archivos = {};
            this.arbolArchivos = {};
            this.archivoSeleccionado = null;
            this.contenidoEditor = '';
            this.bundleAi = '';
            this.registros = [];
            this.agregarRegistro('Proyecto limpiado.', 'info');
        },

        async cargarProyectoDemo() {
            this.limpiarTodo();
            this.agregarRegistro('Cargando proyecto demo...', 'info');

            const archivosDemo = {
                'index.html': '<!DOCTYPE html>\n<html>\n<head>\n<link rel="stylesheet" href="css/estilos.css">\n</head>\n<body>\n<h1>Hola</h1>\n<script src="js/app.js"></script>\n</body>\n</html>',
                'css/estilos.css': 'body { background: #111; color: #fff; font-family: sans-serif; }',
                'js/app.js': 'console.log("Hola desde app.js modular");'
            };

            const tempArchivos = {};
            for (const [ruta, contenido] of Object.entries(archivosDemo)) {
                tempArchivos[ruta] = {
                    contenido: contenido,
                    tamano: contenido.length,
                    esBinario: false
                };
            }

            this.archivos = tempArchivos;
            this.finalizarProcesamiento();
        },

        generarBundleParaIa() {
            this.agregarRegistro('Generando Bundle Markdown para IA...', 'info');
            let output = `# Bundle del Proyecto para IA\n\n`;
            output += `## Estructura de Archivos\n`;

            const rutas = Object.keys(this.archivos).sort();
            rutas.forEach(ruta => {
                output += `- ${ruta}\n`;
            });
            output += `\n---\n\n## Código Fuente\n\n`;

            for (const ruta of rutas) {
                const archivo = this.archivos[ruta];
                if (this.opciones.excluirBinarios && archivo.esBinario) continue;

                const extension = ruta.split('.').pop().toLowerCase();
                let lang = extension;
                if (extension === 'js') lang = 'javascript';
                if (extension === 'html') lang = 'html';
                if (extension === 'css') lang = 'css';
                if (extension === 'json') lang = 'json';

                output += `### Archivo: \`${ruta}\`\n`;
                output += `\`\`\`${lang}\n`;
                output += archivo.contenido + '\n';
                output += `\`\`\`\n\n`;
            }

            this.bundleAi = output;
            this.agregarRegistro(`Bundle generado con éxito (${this.formatearBytes(output.length)})`, 'exito');
        },

        copiarCodigo() {
            const textarea = document.createElement('textarea');
            textarea.value = this.bundleAi;
            document.body.appendChild(textarea);
            textarea.select();
            try {
                document.execCommand('copy');
                this.copiado = true;
                setTimeout(() => this.copiado = false, 2000);
                this.agregarRegistro('Markdown copiado al portapapeles.', 'exito');
            } catch (err) {
                this.agregarRegistro('Error al copiar.', 'error');
            }
            document.body.removeChild(textarea);
        },

        descargarBundle() {
            const blob = new Blob([this.bundleAi], { type: 'text/markdown;charset=utf-8' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'proyecto-para-ia.md';
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
            this.agregarRegistro('Archivo proyecto-para-ia.md descargado.', 'exito');
        }
    }
}
```

