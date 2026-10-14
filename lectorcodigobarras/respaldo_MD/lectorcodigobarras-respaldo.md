# Bundle del Proyecto para IA: lectorcodigobarras

## Estructura de Archivos
- README.md
- documentacion/LEEME.md
- icon-192.png
- icon-512.png
- index.html
- manifest.json
- sw.js
- test.html

---

## Código Fuente

### Archivo: `README.md`
```md
# 📱 Lector de Códigos PWA

Una aplicación web progresiva (PWA) minimalista y rápida diseñada para escanear códigos QR y de barras utilizando la cámara del dispositivo móvil, con soporte para instalación en pantalla completa (modo *standalone*) y funcionamiento optimizado.

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/lectorcodigobarras/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/lectorcodigobarras/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## 🚀 Características Principales

* **Modo PWA Nativa:** Se puede instalar directamente en la pantalla de inicio de tu smartphone (como Android/Samsung) abriéndose en una ventana limpia y sin barras de navegador.
* **Escaneo Eficiente:** Utiliza la robusta librería `html5-qrcode` (versión fijada vía CDN) para garantizar una alta compatibilidad de lectura en diferentes lentes y dispositivos móviles.
* **Gestión de Portapapeles:** Copia el contenido del código detectado al portapapeles de forma automática o mediante un botón dedicado.
* **Soporte Offline:** Incorpora un Service Worker (`sw.js`) que cachea la app, los íconos y la librería de escaneo para funcionar sin conexión.
* **Versión visible en pantalla:** El pie de la app muestra la versión leída directamente desde `sw.js` (`CACHE_NAME`), sin duplicar el dato.

## 📂 Estructura del Proyecto

1. **`index.html`**: Interfaz de usuario, estilos y lógica del escáner.
2. **`manifest.json`**: Configuración de la PWA (nombre, íconos y `display: standalone`).
3. **`sw.js`**: Service Worker para caché y funcionamiento offline. Contiene `CACHE_NAME`, que define la versión mostrada en la app.
4. **`icons/icon-192.png` y `icons/icon-512.png`**: Íconos de la app alojados en el propio repo (no dependen de un CDN externo).
5. **`test.html`**: Panel auxiliar para generar códigos QR/de barras de prueba (estáticos y dinámicos con fecha/hora), útil para probar el lector sin códigos físicos a mano.

## 🛠️ Tecnologías Utilizadas

* **HTML5 & CSS3** (Diseño adaptable y moderno).
* **JavaScript (Vanilla)** para el control de la interfaz y eventos.
* **[html5-qrcode](https://github.com/mebjas/html5-qrcode)** vía CDN (versión fijada) como motor de decodificación de cámara.
* **Service Workers & Web App Manifest** para las capacidades de Progressive Web App.

## 🔢 Versionado

Para publicar una nueva versión, alcanza con cambiar `CACHE_NAME` en `sw.js` (por ejemplo, de `lector-cache-v1.1.0` a `lector-cache-v1.1.1`). El número se refleja automáticamente en el pie de `index.html` y fuerza la invalidación del caché anterior.

## 🌐 Despliegue

El proyecto está optimizado para ser alojado de manera estática en plataformas como **Netlify**. Subí todos los archivos y carpetas (`index.html`, `manifest.json`, `sw.js`, `icons/`) directamente en la carpeta raíz del proyecto para evitar errores de ruta (`404`).

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/lectorcodigobarras/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `icon-192.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `icon-512.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Lector de Códigos</title>
    <!-- Configuración para PWA (Instalación como App) -->
    <link rel="manifest" href="manifest.json">
    <meta name="theme-color" content="#121212">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <link rel="icon" href="icons/icon-192.png" type="image/png">
    <link rel="apple-touch-icon" href="icons/icon-192.png">

    <script src="https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js" type="text/javascript"></script>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: #121212;
            color: #e0e0e0;
            margin: 0;
            padding: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            min-height: 100vh;
            box-sizing: border-box;
        }
        h2 {
            margin: 5px 0 8px 0;
            font-size: 1.1rem;
            color: #ffffff;
            text-align: center;
        }
        .container {
            width: 100%;
            max-width: 420px;
            background: #1e1e1e;
            padding: 10px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            box-sizing: border-box;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        #reader {
            width: 100% !important;
            border-radius: 8px;
            overflow: hidden;
            border: none !important;
            max-height: 260px;
        }
        #reader video {
            border-radius: 8px;
            object-fit: cover;
            max-height: 260px;
        }
        .controls {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }
        button {
            width: 100%;
            padding: 10px;
            border-radius: 6px;
            border: none;
            font-size: 0.95rem;
            font-weight: bold;
            box-sizing: border-box;
            cursor: pointer;
        }
        .btn-success { background-color: #4CAF50; color: #fff; }
        .btn-danger { background-color: #f44336; color: #fff; }
        .btn-warning { background-color: #ff9800; color: #000; }
        .btn-copy { background-color: #00bcd4; color: #000; margin-top: 6px; }
        .btn-exit { background-color: #333; color: #fff; margin-top: 2px; }
        .btn-exit:hover { background-color: #444; }
        
        #result-box {
            padding: 8px;
            background: #2a2a2a;
            border-left: 4px solid #4CAF50;
            border-radius: 4px;
            word-break: break-all;
            display: none;
        }
        .result-title {
            font-size: 0.75rem;
            color: #888;
            margin-bottom: 2px;
        }
        .result-content {
            font-size: 1rem;
            font-family: monospace;
            color: #fff;
        }
        #error-msg {
            color: #ff5252;
            font-size: 0.8rem;
            text-align: center;
            min-height: 15px;
        }
        #app-version {
            font-size: 0.7rem;
            color: #555;
            text-align: center;
            margin-top: 4px;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Lector de Códigos</h2>
    
    <div class="controls">
        <button class="btn-success" id="btn-action" onclick="toggleEscanner()">Iniciar Lector</button>
        <button class="btn-exit" onclick="salirApp()">Salir</button>
    </div>

    <select id="camera-select" style="display:none;"></select>
    <div id="error-msg"></div>

    <div id="reader"></div>

    <div id="result-box">
        <div class="result-title" id="format-type">Código detectado:</div>
        <div class="result-content" id="result-text">-</div>
        <button class="btn-copy" onclick="copiarAlPortapapeles()">Copiar Código</button>
        <button class="btn-warning" onclick="reiniciarEscaneo()" style="margin-top: 6px;">Escanear Siguiente</button>
    </div>

    <div id="app-version">v...</div>
</div>

<script>
    let html5QrCode = null;
    let cameraInitialized = false;

    // Lee la versión dinámicamente desde sw.js
    (async function mostrarVersion() {
        const el = document.getElementById('app-version');
        try {
            const res = await fetch('./sw.js', { cache: 'no-store' });
            const texto = await res.text();
            const match = texto.match(/CACHE_NAME\s*=\s*['"]([^'"]+)['"]/);
            if (match) {
                const version = match[1].replace(/^lector-cache-/, '');
                el.innerText = `v${version}`;
            } else {
                el.innerText = '';
            }
        } catch (err) {
            el.innerText = '';
        }
    })();

    async function inicializarYCategorizarCamara() {
        if (cameraInitialized) return true;
        try {
            html5QrCode = new Html5Qrcode("reader");
            let cameras = await Html5Qrcode.getCameras();
            
            if (cameras && cameras.length > 0) {
                const select = document.getElementById('camera-select');
                select.innerHTML = "";
                let selectedCamId = cameras[0].id;

                cameras.forEach((cam, index) => {
                    let option = document.createElement('option');
                    option.value = cam.id;
                    option.text = cam.label || `Lente ${index + 1}`;
                    select.appendChild(option);

                    if (cam.label.toLowerCase().includes('back') || index === 0) {
                        selectedCamId = cam.id;
                    }
                });

                select.value = selectedCamId;
                cameraInitialized = true;
                return true;
            }
            document.getElementById('error-msg').innerText = "No se encontró ninguna cámara en este dispositivo.";
        } catch (err) {
            console.error(err);
            const nombre = err && err.name;
            let mensaje = "Error de permisos de cámara. Usa HTTPS.";
            if (nombre === "NotAllowedError") {
                mensaje = "Permiso de cámara denegado. Habilitalo en la configuración del navegador.";
            } else if (nombre === "NotFoundError" || nombre === "OverconstrainedError") {
                mensaje = "No se encontró ninguna cámara en este dispositivo.";
            } else if (nombre === "NotReadableError") {
                mensaje = "La cámara está siendo usada por otra aplicación.";
            }
            document.getElementById('error-msg').innerText = mensaje;
        }
        return false;
    }

    async function toggleEscanner() {
        const btnAction = document.getElementById('btn-action');

        if (html5QrCode && html5QrCode.isScanning) {
            await detenerEscanner();
            return;
        }

        document.getElementById('error-msg').innerText = "Iniciando...";
        let listo = await inicializarYCategorizarCamara();
        if (!listo) return;

        const cameraId = document.getElementById('camera-select').value;
        const config = {
            fps: 15,
            qrbox: { width: 250, height: 180 }, 
            aspectRatio: 4/3
        };

        try {
            await html5QrCode.start(
                cameraId, 
                config,
                async (decodedText, decodedResult) => {
                    if (html5QrCode && html5QrCode.isScanning) {
                        await html5QrCode.stop();
                    }

                    document.getElementById('result-box').style.display = 'block';
                    document.getElementById('format-type').innerText = `Formato: ${decodedResult.result.format.formatName || 'Detectado'}`;
                    document.getElementById('result-text').innerText = decodedText;
                    
                    const btnAction = document.getElementById('btn-action');
                    btnAction.innerText = "Iniciar Lector";
                    btnAction.className = "btn-success";
                    document.getElementById('reader').style.display = 'none';

                    if (navigator.vibrate) {
                        navigator.vibrate(100);
                    }
                },
                (errorMessage) => {}
            );

            document.getElementById('error-msg').innerText = "";
            btnAction.innerText = "Detener Lector";
            btnAction.className = "btn-danger";
            document.getElementById('reader').style.display = 'block';
        } catch (err) {
            console.error(err);
            document.getElementById('error-msg').innerText = "No se pudo iniciar la cámara.";
        }
    }

    async function detenerEscanner() {
        if (html5QrCode && html5QrCode.isScanning) {
            try {
                await html5QrCode.stop();
                document.getElementById('error-msg').innerText = "Lector detenido.";
                const btnAction = document.getElementById('btn-action');
                btnAction.innerText = "Iniciar Lector";
                btnAction.className = "btn-success";
            } catch (err) {
                console.error("Error al detener", err);
            }
        }
    }

    async function reiniciarEscaneo() {
        document.getElementById('result-box').style.display = 'none';
        document.getElementById('reader').style.display = 'block';
        document.querySelector('.btn-copy').innerText = "Copiar Código";
        await toggleEscanner();
    }

    function copiarAlPortapapeles() {
        const texto = document.getElementById('result-text').innerText;
        if (texto && texto !== "-") {
            navigator.clipboard.writeText(texto).then(() => {
                const btnCopy = document.querySelector('.btn-copy');
                btnCopy.innerText = "¡Copiado!";
                setTimeout(() => {
                    btnCopy.innerText = "Copiar Código";
                }, 2000);
            });
        }
    }

    async function salirApp() {
        await detenerEscanner();
        window.close();
        setTimeout(() => {
            window.location.href = "about:blank";
        }, 200);
    }
</script>

</body>
</html>
```

### Archivo: `manifest.json`
```json
{
  "name": "Lector de Códigos",
  "short_name": "Lector",
  "start_url": "./index.html",
  "display": "standalone",
  "background_color": "#121212",
  "theme_color": "#121212",
  "icons": [
    {
      "src": "icons/icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icons/icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}

```

### Archivo: `sw.js`
```javascript
const CACHE_NAME = 'lector-cache-v1.1.1';
const urlsToCache = [
  './',
  './index.html',
  './manifest.json',
  './icons/icon-192.png',
  './icons/icon-512.png',
  'https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js'
];

// Instalación del Service Worker
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => {
        return cache.addAll(urlsToCache);
      })
      .then(() => self.skipWaiting())
  );
});

// Activación y limpieza de cachés antiguas
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames.map((cacheName) => {
          if (cacheName !== CACHE_NAME) {
            return caches.delete(cacheName);
          }
        })
      );
    }).then(() => self.clients.claim())
  );
});

// Interceptación de peticiones para funcionamiento offline
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((response) => response || fetch(event.request))
  );
});
```

### Archivo: `test.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Panel Dinámico - Códigos de Barras y QR</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            color: #333;
            text-align: center;
            padding: 20px;
        }
        h1 {
            font-size: 1.5rem;
            margin-bottom: 5px;
        }
        p {
            color: #666;
            margin-bottom: 25px;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 20px;
            max-width: 900px;
            margin: 0 auto;
        }
        .card {
            background: #fff;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        .card img {
            max-width: 100%;
            height: auto;
            margin-bottom: 10px;
        }
        .card span {
            font-family: monospace;
            font-size: 0.85rem;
            background: #eef2f5;
            padding: 4px 8px;
            border-radius: 4px;
            word-break: break-all;
            color: #007bff;
        }
        .dynamic-badge {
            font-size: 0.75rem;
            background: #28a745;
            color: white;
            padding: 2px 6px;
            border-radius: 3px;
            margin-bottom: 6px;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <h1>Panel de Códigos Dinámicos</h1>
    <p>Cada vez que recargues esta página, los códigos con etiqueta verde cambiarán con la hora exacta.</p>

    <div class="grid">
        <!-- DINÁMICO 1: QR con Fecha y Hora Actual -->
        <div class="card">
            <span class="dynamic-badge">DINÁMICO (Fecha/Hora)</span>
            <img id="qr-dynamic" src="" alt="QR Dinámico">
            <span id="text-qr">-</span>
        </div>

        <!-- DINÁMICO 2: Código de Barras con Timestamp numérico -->
        <div class="card">
            <span class="dynamic-badge">DINÁMICO (Timestamp)</span>
            <img id="barcode-dynamic" src="" alt="Barcode Dinámico">
            <span id="text-bar">-</span>
        </div>

        <!-- ESTÁTICO 1 -->
        <div class="card">
            <span>ESTÁTICO</span>
            <img src="https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=TEST-S22-12345" alt="QR Estático">
            <span>TEST-S22-12345</span>
        </div>

        <!-- ESTÁTICO 2 -->
        <div class="card">
            <span>ESTÁTICO</span>
            <img src="https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=https://www.google.com" alt="QR Google">
            <span>https://www.google.com</span>
        </div>

        <!-- ESTÁTICO 3 -->
        <div class="card">
            <span>ESTÁTICO (Barras)</span>
            <img src="https://barcode.tec-it.com/barcode.ashx?data=7791234567890&code=Code128&dpi=96" alt="Barcode 1">
            <span>7791234567890</span>
        </div>

        <!-- ESTÁTICO 4 -->
        <div class="card">
            <span>ESTÁTICO (Barras)</span>
            <img src="https://barcode.tec-it.com/barcode.ashx?data=CONSULTORIO-01&code=Code128&dpi=96" alt="Barcode 3">
            <span>CONSULTORIO-01</span>
        </div>
    </div>

    <script>
        // Generar datos basados en el momento exacto de apertura/recarga
        const ahora = new Date();
        
        // Formato de fecha y hora legible para el QR: "2026-09-15 14:05:30"
        const anio = ahora.getFullYear();
        const mes = String(ahora.getMonth() + 1).padStart(2, '0');
        const dia = String(ahora.getDate()).padStart(2, '0');
        const hora = String(ahora.getHours()).padStart(2, '0');
        const min = String(ahora.getMinutes()).padStart(2, '0');
        const seg = String(ahora.getSeconds()).padStart(2, '0');
        
        const textoDinamicoQR = `LOG-${anio}${mes}${dia}-${hora}:${min}:${seg}`;
        
        // Código numérico único basado en milisegundos exactos para el código de barras
        const textoDinamicoBarra = String(Date.now()).slice(-10);

        // Inyectar QR dinámico
        document.getElementById('qr-dynamic').src = `https://api.qrserver.com/v1/create-qr-code/?size=180x180&data=${encodeURIComponent(textoDinamicoQR)}`;
        document.getElementById('text-qr').innerText = textoDinamicoQR;

        // Inyectar Código de barras dinámico
        document.getElementById('barcode-dynamic').src = `https://barcode.tec-it.com/barcode.ashx?data=${textoDinamicoBarra}&code=Code128&dpi=96`;
        document.getElementById('text-bar').innerText = textoDinamicoBarra;
    </script>

</body>
</html>
```

