# Bundle del Proyecto para IA: trivia

## Estructura de Archivos
- README.md
- css/style.css
- documentacion/LEEME.md
- img/fondo/fondo_1.webp
- img/tarjetas/biologia-pregunta.webp
- img/tarjetas/biologia-respuesta.webp
- img/tarjetas/espacio-pregunta.webp
- img/tarjetas/espacio-respuesta.webp
- img/tarjetas/geografia-pregunta.webp
- img/tarjetas/geografia-respuesta.webp
- img/tarjetas/geologia-pregunta.webp
- img/tarjetas/geologia-respuesta.webp
- img/tarjetas/historia-pregunta.webp
- img/tarjetas/historia-respuesta.webp
- img/tarjetas/nutricion-pregunta.webp
- img/tarjetas/nutricion-respuesta.webp
- img/tarjetas/tecnologia-pregunta.webp
- img/tarjetas/tecnologia-respuesta.webp
- index.html
- js/config.js
- js/grabar.js
- js/inicio.js
- js/script.js
- js/trivias/trivia_parte_1.js
- js/trivias/trivia_parte_10.js
- js/trivias/trivia_parte_11.js
- js/trivias/trivia_parte_12.js
- js/trivias/trivia_parte_2.js
- js/trivias/trivia_parte_3.js
- js/trivias/trivia_parte_4.js
- js/trivias/trivia_parte_5.js
- js/trivias/trivia_parte_6.js
- js/trivias/trivia_parte_7.js
- js/trivias/trivia_parte_8.js
- js/trivias/trivia_parte_9.js
- js/trivias_old.js

---

## Código Fuente

### Archivo: `README.md`
```md
# Quiz de Tarjetas (Trivia)

Trivia de preguntas y respuestas en formato de tarjetas, que avanza sola. Cada pregunta se muestra unos segundos y después aparece la respuesta. Está pensada para pasar en una pantalla o para grabarla como video.

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/trivia/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/trivia/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## Funcionalidades

- **Áreas temáticas**: Historia, Geografía, Biología, Geología, Espacio, Tecnología y Nutrición. Cada área tiene su propia imagen de pregunta y de respuesta.
- **Varias partes**: las preguntas están divididas en archivos `js/trivias/trivia_parte_N.js`. Al empezar, la app detecta cuáles existen y deja elegir cuál reproducir.
- **Cuenta regresiva** de bienvenida y avance automático: 10 segundos para la pregunta y 3 para la respuesta.
- **Grabación opcional**: si se activa al empezar, la app graba la pestaña con las funciones del propio navegador, sin instalar nada, y descarga el video `.webm` al terminar.
- Precarga de imágenes para que no parpadeen.

## Cómo se usa

1. Abrí `index.html`.
2. En la ventana inicial elegí qué parte de la trivia reproducir y si querés grabarla.
3. Si elegiste grabar, el navegador te va a pedir qué pestaña compartir: elegí la de la trivia.
4. Al terminar, el video se descarga solo.

## Agregar preguntas

1. Copiá uno de los archivos de `js/trivias/` con el número siguiente, por ejemplo `trivia_parte_13.js`.
2. Cada pregunta tiene esta forma:

```js
{
    pregunta: "¿...?",
    area: "Historia",
    imgPregunta: "",   // vacío = usa la imagen del área
    respuesta: "...",
    imgRespuesta: ""
}
```

La parte nueva aparece sola en la ventana de inicio. No hace falta registrarla en ningún lado.

## Configuración

En `js/config.js` se definen:

- si la trivia vuelve a empezar al terminar (`REINICIAR_AL_TERMINAR`);
- las imágenes por defecto y las de cada área;
- los tiempos de pregunta y respuesta.

## Ejecutar localmente

Usa módulos de JavaScript, así que hay que abrirla con un servidor (por ejemplo `npx serve`). Abriendo el archivo directamente no funciona.

## Estructura

```
index.html            Página de inicio
css/style.css         Estilos
js/inicio.js          Ventana inicial: elegir parte y grabación
js/script.js          Ciclo de preguntas y respuestas
js/grabar.js          Grabación de la pestaña
js/config.js          Configuración
js/trivias/           Preguntas, divididas en partes
img/                  Fondo e imágenes por área
```

```

### Archivo: `css/style.css`
```css
/* --- CONFIGURACIÓN GENERAL Y ANIMACIÓN DE FONDO --- */
body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    overflow: hidden;
    
    /* Fondo oscuro de respaldo */
    background-color: #0f172a; 
    
    /* Configuración de la imagen de fondo */
    background-image: url('../img/fondo/fondo_1.webp');
    background-repeat: no-repeat;
    background-attachment: fixed;
    
    /* Escalamos la imagen para permitir los desplazamientos en todos los extremos */
    background-size: 150% 150%;
    
    /* Nueva animación extendida y ralentizada a 40s para que sea suave */
    animation: secuenciaFondoExtendida 40s ease-in-out infinite;
}

/* --- SECUENCIA MAXIMA EXTENDIDA --- */
@keyframes secuenciaFondoExtendida {
    /* --- EJE HORIZONTAL --- */
    0% { background-position: 50% 50%; }     /* Centro */
    8% { background-position: 100% 50%; }    /* Todo a la derecha */
    16% { background-position: 50% 50%; }    /* Regresa al centro */
    24% { background-position: 0% 50%; }     /* Todo a la izquierda */
    32% { background-position: 50% 50%; }    /* Regresa al centro */

    /* --- EJE VERTICAL --- */
    40% { background-position: 50% 100%; }   /* Todo hacia abajo */
    48% { background-position: 50% 50%; }    /* Regresa al centro */
    56% { background-position: 50% 0%; }     /* Todo hacia arriba */
    64% { background-position: 50% 50%; }    /* Regresa al centro */

    /* --- DIAGONALES CRUZADAS --- */
    72% { background-position: 100% 100%; }  /* Esquina inferior derecha */
    80% { background-position: 50% 50%; }    /* Regresa al centro */
    90% { background-position: 0% 0%; }      /* Esquina superior izquierda */
    100% { background-position: 50% 50%; }   /* Cierre perfecto en el centro */
}

/* --- CONTENEDOR ENMARCADO (Efecto Cine / Redes Sociales) --- */
.contenedor-trivia {
    perspective: 2000px;
    padding: 40px;
    background: rgba(255, 255, 255, 0.03);
    border-radius: 24px;
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    border: 1px solid rgba(255, 255, 255, 0.1);
    box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
}

/* --- ESTRUCTURA E INTERFACES 3D DE LA TARJETA --- */
.tarjeta {
    width: 680px;         
    height: 460px;        
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    cursor: pointer;
}

/* Clase añadida por JS para girar la tarjeta */
.tarjeta.volteada {
    transform: rotateY(180deg);
}

/* --- PROPIEDADES COMPARTIDAS DE LAS CARAS --- */
.cara {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    -webkit-backface-visibility: hidden;
    display: flex;
    flex-direction: column;    
    justify-content: space-between;
    align-items: center;
    padding: 35px;             
    box-sizing: border-box;
    border-radius: 20px;       
    text-align: center;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
}

/* --- CARA FRENTE (Pregunta) --- */
.frente {
    background: linear-gradient(135deg, #ffffff, #f8fafc);
    color: #1e293b;
    border: 4px solid #3b82f6;
}

/* --- CARA DORSO (Respuesta) --- */
.dorso {
    background: linear-gradient(135deg, #10b981, #059669);
    color: #ffffff;
    transform: rotateY(180deg);
    border: 4px solid #34d399;
}

/* --- ESTILIZACIÓN DE LAS IMÁGENES --- */
.img-trivia {
    width: 100%;
    height: 240px;         
    object-fit: cover;     
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
    border: 1px solid rgba(0, 0, 0, 0.05);
    transition: transform 0.5s ease;
}

/* Efecto de luces sutiles sobre las imágenes del dorso */
.dorso .img-trivia {
    border: 1px solid rgba(255, 255, 255, 0.2);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
}

/* --- TRATAMIENTO DE LOS TEXTOS --- */
#texto-pregunta, #texto-respuesta {
    font-size: 1.7rem;     
    font-weight: 700;      
    line-height: 1.4;
    margin-top: auto;      
    margin-bottom: auto;
    padding: 10px 0 0 0;
}

#texto-pregunta {
    letter-spacing: -0.02em;
    text-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
}

#texto-respuesta {
    letter-spacing: 0.01em;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}




/* --- ESTILO DEL PIE DE PÁGINA --- */
.pie-pagina {
    position: absolute;
    bottom: 20px;
    width: 100%;
    text-align: center;
   /* color: rgba(255, 255, 255, 0.6); /* Color blanco suave para que no distraiga de la trivia */
   color: #333333; 
   font-size: 0.9rem;
    letter-spacing: 0.05em;
    font-family: inherit;
}
```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/trivia/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `img/fondo/fondo_1.webp`
```webp
[Archivo omitido: pesa mas de 500 KB]
```

### Archivo: `img/tarjetas/biologia-pregunta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/biologia-respuesta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/espacio-pregunta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/espacio-respuesta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/geografia-pregunta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/geografia-respuesta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/geologia-pregunta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/geologia-respuesta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/historia-pregunta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/historia-respuesta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/nutricion-pregunta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/nutricion-respuesta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/tecnologia-pregunta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/tarjetas/tecnologia-respuesta.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz de Tarjetas</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>

    <div class="contenedor-trivia">
    <div class="tarjeta" id="tarjeta">
        
        <!-- CARA FRENTE (Pregunta) -->
        <div class="cara frente">
            <img id="img-pregunta" src="" alt="Imagen de la pregunta" class="img-trivia">
            <div id="texto-pregunta">Cargando pregunta...</div>
        </div>
        
        <!-- CARA DORSO (Respuesta) -->
        <div class="cara dorso">
            <img id="img-respuesta" src="" alt="Imagen de la respuesta" class="img-trivia">
            <div id="texto-respuesta">Cargando respuesta...</div>
        </div>

    </div>
</div>

    <footer class="pie-pagina">
        <p>&copy; 2026 Quiz en Tarjetas. Creado por TechFaro : techfaro.code@gmail.com</p>
    </footer>

    <script type="module" src="js/inicio.js"></script>
</body>
</html>
```

### Archivo: `js/config.js`
```javascript
// Configuración del juego
export const REINICIAR_AL_TERMINAR = false; // true = vuelve a empezar en bucle / false = se detiene

// --- CONFIGURACIÓN DE IMÁGENES POR DEFECTO ---
export const IMG_PREGUNTA_DEFECTO = "https://images.unsplash.com/photo-1607604276583-eef5d076aa5f?w=400";
export const IMG_RESPUESTA_DEFECTO = "https://images.unsplash.com/photo-1542435503-956c469947f6?w=400";
export const IMG_BIENVENIDA = "img/fondo/fondo_1.webp"; // Usamos tu fondo optimizado para la bienvenida

// 2. CONFIGURACIÓN DE IMÁGENES FIJAS POR ÁREA TEMÁTICA
export const IMAGENES_POR_AREA = {
    "Historia": {
        pregunta: "img/tarjetas/historia-pregunta.webp",
        respuesta: "img/tarjetas/historia-respuesta.webp"
    },
    "Geografía": {
        pregunta: "img/tarjetas/geografia-pregunta.webp",
        respuesta: "img/tarjetas/geografia-respuesta.webp"
    },
    "Biología": {
        pregunta: "img/tarjetas/biologia-pregunta.webp",
        respuesta: "img/tarjetas/biologia-respuesta.webp"
    },
    "Geología": {
        pregunta: "img/tarjetas/geologia-pregunta.webp",
        respuesta: "img/tarjetas/geologia-respuesta.webp"
    },
    "Espacio": {
        pregunta: "img/tarjetas/espacio-pregunta.webp",
        respuesta: "img/tarjetas/espacio-respuesta.webp"
    },
    "Tecnología": {
        pregunta: "img/tarjetas/tecnologia-pregunta.webp",
        respuesta: "img/tarjetas/tecnologia-respuesta.webp"
    },
    "Nutricion": {
        pregunta: "img/tarjetas/nutricion-pregunta.webp",
        respuesta: "img/tarjetas/nutricion-respuesta.webp"
    }
};

// Tiempos en milisegundos (10s pregunta / 3s respuesta  )
export const TIEMPO_MOSTRAR_PREGUNTA = 15000;
export const TIEMPO_MOSTRAR_RESPUESTA = 6000;
export const TIEMPO_CUENTA_ATRAS = 15; // Duración de la cuenta regresiva en segundos

```

### Archivo: `js/grabar.js`
```javascript
// grabar.js
// Graba esta pestaña del navegador con las APIs nativas (sin instalar nada)
// y descarga el .webm automáticamente cuando la trivia termina.
// Se usa desde js/inicio.js, después de que el usuario confirma el modal.
//
// Nota: el estado de la grabación solo se informa por consola (F12), nunca
// en pantalla, para que ningún aviso quede "quemado" dentro del video grabado.

let mediaRecorder = null;
let chunks = [];
let stream = null;
let descargado = false;

function logEstado(texto) {
    console.log('[grabar.js]', texto);
}

function descargarVideo(parte) {
    if (descargado || chunks.length === 0) return;
    descargado = true;
    const blob = new Blob(chunks, { type: 'video/webm' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `trivia_parte_${parte}.webm`;
    document.body.appendChild(a);
    a.click();
    a.remove();
    logEstado('✅ Video descargado: trivia_parte_' + parte + '.webm');
    setTimeout(() => URL.revokeObjectURL(url), 10000);
}

/**
 * Pide permiso para grabar la pestaña actual y arranca el MediaRecorder.
 * Se detiene y descarga solo cuando la trivia dispara 'trivia-finalizada'.
 * @param {string|number} parte - número de trivia_parte_N.js, para el nombre del archivo
 * @returns {Promise<boolean>} true si el usuario aceptó grabar
 */
export async function iniciarGrabacion(parte) {
    try {
        stream = await navigator.mediaDevices.getDisplayMedia({
            video: { frameRate: 30 },
            audio: false,
            preferCurrentTab: true, // Chrome: evita elegir manualmente la pestaña
        });
    } catch (err) {
        logEstado('❌ No se pudo grabar (permiso denegado o cancelado).');
        return false;
    }

    chunks = [];
    descargado = false;

    const tiposPosibles = ['video/webm;codecs=vp9', 'video/webm;codecs=vp8', 'video/webm'];
    const mimeType = tiposPosibles.find((t) => MediaRecorder.isTypeSupported(t)) || '';

    mediaRecorder = new MediaRecorder(stream, mimeType ? { mimeType } : undefined);
    mediaRecorder.ondataavailable = (e) => {
        if (e.data && e.data.size > 0) chunks.push(e.data);
    };
    mediaRecorder.onstop = () => {
        descargarVideo(parte);
        stream.getTracks().forEach((t) => t.stop());
    };

    // Si el usuario corta el "compartir pantalla" manualmente, igual se descarga lo grabado
    stream.getVideoTracks()[0].addEventListener('ended', () => {
        if (mediaRecorder && mediaRecorder.state !== 'inactive') mediaRecorder.stop();
    });

    // Auto-stop cuando la trivia dispara su evento de fin (ver script.js)
    document.addEventListener('trivia-finalizada', () => {
        if (mediaRecorder && mediaRecorder.state !== 'inactive') {
            logEstado('🏁 Trivia terminada. Descargando video...');
            setTimeout(() => mediaRecorder.stop(), 800); // pequeño colchón final
        }
    });

    mediaRecorder.start(1000); // un chunk por segundo (más seguro ante cortes)
    logEstado('🔴 Grabando trivia_parte_' + parte + '... se descarga sola al terminar.');
    return true;
}

```

### Archivo: `js/inicio.js`
```javascript
// inicio.js
// Punto de entrada de la app. Antes de arrancar la trivia:
//   1. Detecta qué archivos trivia_parte_N.js existen en js/trivias/
//   2. Muestra un modal para elegir cuál reproducir y si se quiere
//      grabar + descargar el video al terminar.
//   3. Recién ahí importa script.js y arranca el juego.

const MAX_PARTES_A_BUSCAR = 20; // tope de búsqueda; ajustar si algún día hay más de 20 archivos

async function detectarPartesDisponibles() {
    const disponibles = [];
    for (let n = 1; n <= MAX_PARTES_A_BUSCAR; n++) {
        try {
            // Un 404 en un import() dinámico rechaza la promesa: así detectamos
            // qué archivos existen realmente en js/trivias/ sin necesitar un
            // listado manual ni un endpoint de servidor.
            await import(`./trivias/trivia_parte_${n}.js`);
            disponibles.push(n);
        } catch (err) {
            // Si falla la 1 o 2 seguidas asumimos que no hay más archivos después
            if (disponibles.length > 0) break;
        }
    }
    return disponibles;
}

function crearModal(partes) {
    return new Promise((resolve) => {
        const overlay = document.createElement('div');
        Object.assign(overlay.style, {
            position: 'fixed',
            inset: '0',
            background: 'rgba(0,0,0,0.75)',
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'center',
            zIndex: 10000,
            fontFamily: 'sans-serif',
        });

        const caja = document.createElement('div');
        Object.assign(caja.style, {
            background: '#fff',
            borderRadius: '12px',
            padding: '28px 32px',
            maxWidth: '360px',
            width: '90%',
            boxShadow: '0 8px 30px rgba(0,0,0,0.3)',
            textAlign: 'center',
        });

        const titulo = document.createElement('h2');
        titulo.textContent = 'Quiz de Tarjetas';
        titulo.style.margin = '0 0 4px';

        const subtitulo = document.createElement('p');
        subtitulo.textContent = 'Elegí qué trivia querés reproducir.';
        subtitulo.style.margin = '0 0 18px';
        subtitulo.style.color = '#555';
        subtitulo.style.fontSize = '14px';

        const selectLabel = document.createElement('label');
        selectLabel.textContent = 'Trivia:';
        selectLabel.style.display = 'block';
        selectLabel.style.textAlign = 'left';
        selectLabel.style.fontSize = '13px';
        selectLabel.style.marginBottom = '4px';
        selectLabel.style.fontWeight = 'bold';

        const select = document.createElement('select');
        Object.assign(select.style, {
            width: '100%',
            padding: '8px',
            fontSize: '14px',
            marginBottom: '18px',
            borderRadius: '6px',
            border: '1px solid #ccc',
        });
        partes.forEach((n) => {
            const opt = document.createElement('option');
            opt.value = String(n);
            opt.textContent = `Parte ${n} (trivia_parte_${n}.js)`;
            select.appendChild(opt);
        });

        const checkboxWrap = document.createElement('label');
        Object.assign(checkboxWrap.style, {
            display: 'flex',
            alignItems: 'center',
            gap: '8px',
            fontSize: '14px',
            textAlign: 'left',
            marginBottom: '22px',
            cursor: 'pointer',
        });
        const checkbox = document.createElement('input');
        checkbox.type = 'checkbox';
        const checkboxTexto = document.createElement('span');
        checkboxTexto.textContent = 'Grabar y descargar el video al terminar';
        checkboxWrap.appendChild(checkbox);
        checkboxWrap.appendChild(checkboxTexto);

        const nota = document.createElement('p');
        nota.textContent = 'Si activás la grabación, Chrome te va a pedir permiso para compartir esta pestaña.';
        Object.assign(nota.style, {
            fontSize: '11px',
            color: '#888',
            margin: '-14px 0 18px',
            display: 'none',
        });
        checkbox.addEventListener('change', () => {
            nota.style.display = checkbox.checked ? 'block' : 'none';
        });

        const boton = document.createElement('button');
        boton.textContent = 'Comenzar';
        Object.assign(boton.style, {
            width: '100%',
            padding: '10px',
            fontSize: '15px',
            fontWeight: 'bold',
            border: 'none',
            borderRadius: '8px',
            background: '#d32f2f',
            color: '#fff',
            cursor: 'pointer',
        });

        boton.addEventListener('click', () => {
            overlay.remove();
            resolve({ parte: select.value, grabar: checkbox.checked });
        });

        caja.appendChild(titulo);
        caja.appendChild(subtitulo);
        caja.appendChild(selectLabel);
        caja.appendChild(select);
        caja.appendChild(checkboxWrap);
        caja.appendChild(nota);
        caja.appendChild(boton);
        overlay.appendChild(caja);
        document.body.appendChild(overlay);
    });
}

async function main() {
    const partes = await detectarPartesDisponibles();

    if (partes.length === 0) {
        document.body.innerHTML =
            '<p style="font-family:sans-serif;text-align:center;margin-top:40px;">No se encontró ningún archivo trivia_parte_N.js en js/trivias/</p>';
        return;
    }

    const { parte, grabar } = await crearModal(partes);

    const moduloTrivia = await import(`./trivias/trivia_parte_${parte}.js`);
    const { iniciarApp } = await import('./script.js');

    if (grabar) {
        const { iniciarGrabacion } = await import('./grabar.js');
        await iniciarGrabacion(parte);
    }

    iniciarApp(moduloTrivia.trivia);
}

main();

```

### Archivo: `js/script.js`
```javascript
// Este módulo ya no importa las preguntas directamente: las recibe vía
// iniciarApp(trivia) desde js/inicio.js, que es quien pregunta al usuario
// (por el modal) qué archivo de js/trivias/ usar.
import {
    REINICIAR_AL_TERMINAR,
    IMG_PREGUNTA_DEFECTO,
    IMG_RESPUESTA_DEFECTO,
    IMG_BIENVENIDA,
    IMAGENES_POR_AREA,
    TIEMPO_MOSTRAR_PREGUNTA,
    TIEMPO_MOSTRAR_RESPUESTA,
    TIEMPO_CUENTA_ATRAS
} from './config.js';

let trivia = [];
let indiceActual = -1;
const tarjeta = document.getElementById('tarjeta');
const textoPregunta = document.getElementById('texto-pregunta');
const textoRespuesta = document.getElementById('texto-respuesta');
const imgPregunta = document.getElementById('img-pregunta');
const imgRespuesta = document.getElementById('img-respuesta');

// Precarga automática de imágenes en segundo plano para que no parpadeen
function precargarImagenes() {
    Object.values(IMAGENES_POR_AREA).forEach(tema => {
        const imgP = new Image(); imgP.src = tema.pregunta;
        const imgR = new Image(); imgR.src = tema.respuesta;
    });
}

function iniciarTrivia() {
    // --- ESTADO DE BIENVENIDA Y CUENTA ATRÁS ---
    if (indiceActual === -1) {
        precargarImagenes(); // Empieza a cachear las imágenes de los temas
        textoPregunta.textContent = "¡Acepta el desafío!";
        imgPregunta.src = IMG_BIENVENIDA;
        tarjeta.classList.remove('volteada');

        let segundosRestantes = TIEMPO_CUENTA_ATRAS;
        textoRespuesta.textContent = `Comenzamos en... ${segundosRestantes}`;

        // Iniciamos la cuenta regresiva numérica
        const intervaloBienvenida = setInterval(() => {
            segundosRestantes--;
            if (segundosRestantes > 0) {
                textoRespuesta.textContent = `Comenzamos en... ${segundosRestantes}`;
            } else {
                clearInterval(intervaloBienvenida);
                indiceActual = 0; // Pasamos a la primera pregunta real
                iniciarTrivia();
            }
        }, 1000);

        return; // Detiene la ejecución aquí para que no avance al ciclo de preguntas aún
    }

    // --- VERIFICACIÓN DE FIN DE JUEGO ---
    if (indiceActual >= trivia.length) {
        if (REINICIAR_AL_TERMINAR) {
            indiceActual = 0;
            trivia.sort(() => Math.random() - 0.5);
            console.log("El bucle está activo. Reiniciando trivia...");
        } else {
            console.log("Fin de las preguntas. El juego se ha detenido.");
            mostrarPantallaFin();
            return;
        }
    }

    // --- CICLO NORMAL DE PREGUNTAS ---
    const tarjetaActual = trivia[indiceActual];
    const areaTema = tarjetaActual.area;

    // 3. CARGAR DATOS DEL FRENTE (Pregunta)
    textoPregunta.textContent = tarjetaActual.pregunta;

    if (tarjetaActual.imgPregunta) {
        imgPregunta.src = tarjetaActual.imgPregunta;
    } else if (areaTema && IMAGENES_POR_AREA[areaTema]) {
        imgPregunta.src = IMAGENES_POR_AREA[areaTema].pregunta;
    } else {
        imgPregunta.src = IMG_PREGUNTA_DEFECTO;
    }

    // 4. CARGAR DATOS DEL DORSO (Respuesta)
    textoRespuesta.textContent = `Respuesta: ${tarjetaActual.respuesta}`;

    if (tarjetaActual.imgRespuesta) {
        imgRespuesta.src = tarjetaActual.imgRespuesta;
    } else if (areaTema && IMAGENES_POR_AREA[areaTema]) {
        imgRespuesta.src = IMAGENES_POR_AREA[areaTema].respuesta;
    } else {
        imgRespuesta.src = IMG_RESPUESTA_DEFECTO;
    }

    // Asegurar que la tarjeta inicie de frente
    tarjeta.classList.remove('volteada');

    // Esperar unos segundos y girar la tarjeta
    setTimeout(() => {
        tarjeta.classList.add('volteada');

        // Esperar el tiempo de la respuesta y pasar a la siguiente tarjeta
        setTimeout(() => {
            indiceActual++;
            iniciarTrivia();
        }, TIEMPO_MOSTRAR_RESPUESTA);

    }, TIEMPO_MOSTRAR_PREGUNTA);
}

function mostrarPantallaFin() {
    textoPregunta.textContent = "¡Has completado todas las preguntas!";
    imgPregunta.src = IMG_BIENVENIDA;
    tarjeta.classList.remove('volteada');
    // Aviso para quien esté grabando (ver grabar.js): la trivia terminó.
    document.dispatchEvent(new Event('trivia-finalizada'));
}

/**
 * Arranca la app con el set de preguntas elegido por el usuario en el modal.
 * @param {Array} triviaSeleccionada - array de preguntas (trivia_parte_N.js)
 */
export function iniciarApp(triviaSeleccionada) {
    trivia = triviaSeleccionada.slice(); // copia para no mutar el original al mezclar
    trivia.sort(() => Math.random() - 0.5);
    indiceActual = -1;
    iniciarTrivia();
}

// Se mantiene por compatibilidad, por si algo externo necesita reiniciar
// el ciclo sin recargar la página (por ejemplo, si en el futuro se agrega
// una opción de "repetir").
window.__trivia = {
    reiniciar() {
        indiceActual = -1;
        iniciarTrivia();
    }
};

```

### Archivo: `js/trivias/trivia_parte_1.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué civilización antigua construyó el Coliseo y expandió su imperio por todo el Mediterráneo?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La civilización Romana",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué período histórico comenzó tras la caída del Imperio Romano de Occidente en el año 476?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Edad Media",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué importante invento de Johannes Gutenberg en el siglo XV permitió duplicar libros de forma masiva?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La imprenta",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué hito histórico de 1789 marcó el fin de la monarquía absoluta en Francia bajo el lema 'Libertad, Igualdad y Fraternidad'?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Revolución Francesa",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Cuál es la línea imaginaria que divide a la Tierra en los hemisferios Norte y Sur?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Ecuador",
        imgRespuesta: ""
    },
    {
        pregunta: "En términos de población, ¿cuál es el continente más poblado del mundo?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Asia",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué gran cordillera montañosa se extiende a lo largo de la costa occidental de América del Sur?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "La Cordillera de los Andes",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el proceso por el cual el suelo fértil pierde sus propiedades debido a la sequía o la deforestación?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Desertificación",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Cómo se llaman las células que no tienen un núcleo definido y cuyo material genético está disperso?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Células procariotas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué proceso realizan las plantas para transformar la luz solar en energía química?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "La fotosíntesis",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué molécula con estructura de doble hélice contiene las instrucciones genéticas de los seres vivos?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "El ADN",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denomina a los organismos que producen su propio alimento, como las plantas y las algas?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Organismos autótrofos",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Cuál es la capa más interna y caliente de la Tierra, compuesta principalmente de hierro y níquel?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El núcleo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el supercontinente que existió hace millones de años antes de dividirse en los actuales?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Pangea",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de rocas se forman a partir del enfriamiento y solidificación del magma o la lava?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Rocas ígneas (o magmáticas)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman los fragmentos de la litosfera que se mueven sobre el manto terrestre y causan terremotos?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Placas tectónicas",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Cuál es el planeta más grande de nuestro sistema solar?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Júpiter",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué fuerza invisible mantiene a los planetas orbitando alrededor del Sol?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "La fuerza de gravedad",
        imgRespuesta: ""
    },
    {
        pregunta: "¿A qué planeta del sistema solar se le conoce popularmente como el 'lucero del alba' debido a su gran brillo?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Venus",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la gigantesca agrupación de estrellas, gas y polvo a la que pertenece nuestro sistema solar?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "La Vía Láctea",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué componente de la computadora es considerado el 'cerebro' encargado de procesar todas las instrucciones?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "La CPU (Unidad Central de Procesamiento)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el sistema de redes interconectadas a nivel mundial que permite compartir información?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Internet",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué componente de hardware se utiliza para almacenar los datos de manera temporal mientras los programas se ejecutan?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Memoria RAM",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de periférico es una impresora: de entrada, de salida o de almacenamiento?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Periférico de salida",
        imgRespuesta: ""
    },
    {
        pregunta: "Según la OMS, ¿qué tipo de grasas presentes en alimentos ultraprocesados y fritos se deben evitar por completo para proteger la salud del corazón?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Las grasas trans",
        imgRespuesta: ""
    },
    {
        pregunta: "Según las recomendaciones de la OMS, ¿cuántos gramos de sal al día (equivalente a una cucharadita) como máximo debería consumir un adulto?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "5 gramos",
        imgRespuesta: ""
    },
    {
        pregunta: "Para combatir la obesidad, la OPS promueve el uso de octógonos negros en los envases. ¿Cómo se le llama a este sistema de advertencia?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Etiquetado frontal nutricional",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué grupo de alimentos de origen vegetal recomienda la OMS consumir diariamente por su alto contenido de fibra, vitaminas y minerales?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Frutas y verduras",
        imgRespuesta: ""
    }
];
```

### Archivo: `js/trivias/trivia_parte_10.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué gran fortificación china, de miles de kilómetros de extensión, fue construida para proteger el territorio de invasiones?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Gran Muralla China",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se conoce al período de tensión política y militar entre Estados Unidos y la Unión Soviética tras la Segunda Guerra Mundial?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Guerra Fría",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué pastor y activista estadounidense lideró el movimiento por los derechos civiles y pronunció el discurso 'Tengo un sueño'?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Martin Luther King Jr.",
        imgRespuesta: ""
    },
    {
        pregunta: "¿En qué famosa fecha del calendario romano fue asesinado Julio César por un grupo de senadores?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Los Idus de Marzo",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Cuál es la capital de Egipto, la ciudad más poblada del mundo árabe, situada a orillas del río Nilo?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Cairo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué mar, ubicado entre África y la península arábiga, debe su nombre a las floraciones de algas que tiñen sus aguas?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Mar Rojo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el bioma de selva tropical más extenso del planeta, compartido por nueve países sudamericanos?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "La selva amazónica",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la zona del océano Atlántico donde, según leyendas populares, desaparecen embarcaciones y aeronaves de forma misteriosa?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Triángulo de las Bermudas",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Qué grupo de animales, caracterizado por tener plumas, pico y poner huevos, incluye a las águilas y los pingüinos?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Las aves",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué orgánulo exclusivo de las células vegetales contiene clorofila y realiza la fotosíntesis?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "El cloroplasto",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la desaparición total y definitiva de una especie de la faz de la Tierra?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Extinción",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman las sustancias químicas producidas por glándulas que regulan funciones del organismo, como la insulina o la adrenalina?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Hormonas",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Qué roca ígnea de grano grueso, muy utilizada en construcción y mesadas, está compuesta principalmente de cuarzo y feldespato?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El granito",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué combustible fósil gaseoso, compuesto principalmente por metano, se utiliza ampliamente para calefacción y cocina?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El gas natural",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la capa superficial y fértil del suelo, rica en materia orgánica descompuesta, ideal para el cultivo?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Humus",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la capa de la atmósfera terrestre más cercana a la superficie, donde ocurren los fenómenos climáticos?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Troposfera",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Qué empresa aeroespacial privada, fundada por Elon Musk, desarrolló los cohetes reutilizables Falcon 9?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "SpaceX",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la estrella pequeña y densa que queda como remanente después de que una estrella similar al Sol agota su combustible?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Enana blanca",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué fenómeno astronómico ocurre cuando la Luna se interpone entre el Sol y la Tierra, ocultando la luz solar?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Eclipse solar",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denomina a la materia invisible que no emite luz pero cuya gravedad influye en el movimiento de las galaxias?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Materia oscura",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué tipo de plataforma web, como Google, permite encontrar información en Internet a partir de palabras clave?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Motor de búsqueda",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denomina al servicio que permite reproducir contenido de audio o video en tiempo real a través de Internet, sin descargarlo?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Streaming",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué periférico de entrada se utiliza para desplazar el cursor y hacer clic en elementos de la pantalla?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "El mouse (ratón)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué proceso consiste en instalar nuevas versiones de un programa para corregir errores o agregar funciones?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Actualización de software",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué proteína presente en el trigo, la cebada y el centeno provoca reacciones adversas en las personas celíacas?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "El gluten",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el indicador que mide qué tan rápido un alimento eleva el nivel de azúcar en la sangre?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Índice glucémico",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué alimento de origen animal es considerado una fuente de proteína completa por contener todos los aminoácidos esenciales?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "El huevo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de fibra, presente en la avena y las manzanas, se disuelve en agua y ayuda a controlar el colesterol?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Fibra soluble",
        imgRespuesta: ""
    }
];

```

### Archivo: `js/trivias/trivia_parte_11.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué losa de piedra, hallada en Egipto en 1799, permitió descifrar los jeroglíficos gracias a su texto trilingüe?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Piedra de Rosetta",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué conflicto armado de 1910 derrocó al régimen de Porfirio Díaz e impulsó reformas sociales y agrarias en México?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Revolución Mexicana",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué general argentino cruzó los Andes y lideró la independencia de Argentina, Chile y Perú?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "José de San Martín",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué reina francesa, esposa de Luis XVI, fue guillotinada durante la Revolución Francesa en 1793?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "María Antonieta",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Cuál es la capital de Portugal, ubicada a orillas del río Tajo?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Lisboa",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué remota isla del Pacífico, perteneciente a Chile, es famosa por sus enormes estatuas de piedra llamadas moáis?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Isla de Pascua",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el continente más frío y con menor densidad de población del planeta, ubicado en el polo sur?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "La Antártida",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la zona donde el río Nilo se ramifica en varios brazos antes de desembocar en el mar Mediterráneo?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Delta del Nilo",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Qué naturalista sueco creó el sistema de clasificación de los seres vivos mediante género y especie?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Carlos Linneo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la relación ecológica en la que un organismo se beneficia a costa de perjudicar a otro?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Parasitismo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué componentes de la sangre son los responsables de detener las hemorragias formando coágulos?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Plaquetas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué estrategia de supervivencia utilizan animales como el camaleón para confundirse con su entorno y evitar depredadores?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Camuflaje",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Cómo se llama la depresión circular en la superficie terrestre producida por el impacto de un cuerpo celeste?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Cráter de impacto",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de roca o sedimento de grano muy fino, moldeable al mezclarse con agua, se utiliza para fabricar cerámica y ladrillos?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Arcilla",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denomina al conjunto de procesos geológicos relacionados con la actividad volcánica y la salida de magma a la superficie?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Vulcanismo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué mineral blando de color gris oscuro, compuesto de carbono puro, se utiliza en la mina de los lápices?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Grafito",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Qué empresa privada se convirtió en la primera en enviar astronautas a la Estación Espacial Internacional en una nave propia?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "SpaceX",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la energía de alta velocidad proveniente del espacio exterior que bombardea constantemente la atmósfera terrestre?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Radiación cósmica",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman los objetos construidos por el ser humano que orbitan la Tierra para comunicaciones, GPS o clima?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Satélites artificiales",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama a un grupo de miles de estrellas unidas gravitacionalmente que se formaron aproximadamente al mismo tiempo?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Cúmulo estelar",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué moneda digital descentralizada, creada en 2009, fue la primera criptomoneda basada en tecnología blockchain?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Bitcoin",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman los pequeños archivos que los sitios web guardan en el navegador para recordar preferencias del usuario?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Cookies",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denomina al engaño digital que busca robar datos personales o bancarios haciéndose pasar por una entidad confiable?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Phishing",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué pequeño dispositivo de almacenamiento portátil se conecta por USB para transportar archivos entre computadoras?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Pendrive (memoria USB)",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Cómo se llama la alimentación que excluye la carne pero puede incluir huevo y lácteos según el tipo de dieta?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Vegetarianismo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué patrón alimentario consiste en alternar períodos de ingesta con períodos prolongados sin consumir alimentos?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Ayuno intermitente",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué método visual, dividido en secciones de vegetales, proteínas y carbohidratos, ayuda a equilibrar las comidas diarias?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "El plato saludable (método del plato)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de sustitutos del azúcar, como la sacarina o el aspartamo, aportan sabor dulce con pocas o ninguna caloría?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Edulcorantes artificiales",
        imgRespuesta: ""
    }
];

```

### Archivo: `js/trivias/trivia_parte_12.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿En qué año el arqueólogo Howard Carter descubrió la tumba intacta del joven faraón Tutankamón?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "1922",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué proceso bélico, iniciado en 1810, permitió a las Provincias Unidas del Río de la Plata independizarse de España?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Guerra de Independencia Argentina",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué físico y matemático inglés formuló las leyes del movimiento y la ley de gravitación universal en el siglo XVII?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Isaac Newton",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué rey persa, conocido como 'el Grande', fundó el Imperio Aqueménida y fue famoso por su tolerancia hacia los pueblos conquistados?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Ciro el Grande",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Qué país de Oceanía, formado por dos islas principales, es conocido por sus paisajes montañosos y por ser locación de 'El Señor de los Anillos'?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Nueva Zelanda",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el sistema de arrecifes de coral más grande del mundo, ubicado frente a la costa de Australia?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "La Gran Barrera de Coral",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el río más largo de Europa, que nace en Rusia y desemboca en el Mar Caspio?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El río Volga",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué gran cuerpo de agua, ubicado entre Estados Unidos y México, recibe las aguas del río Mississippi?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Golfo de México",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Cuántos huesos tiene aproximadamente el esqueleto de un ser humano adulto?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "206 huesos",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué monje agustino austríaco es considerado el padre de la genética moderna por sus experimentos con plantas de arveja?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Gregor Mendel",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denomina a la variedad de especies, genes y ecosistemas presentes en un lugar determinado del planeta?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Biodiversidad",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué órganos del cuerpo humano se encargan de filtrar la sangre y eliminar desechos a través de la orina?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Los riñones",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Cómo se llama la gran masa de hielo compactado que se desplaza lentamente por efecto de su propio peso y la gravedad?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Glaciar",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué roca sedimentaria, compuesta principalmente de carbonato de calcio, se utiliza para fabricar cemento y cal?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Roca caliza",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el proceso por el cual partículas transportadas por el agua o el viento se depositan y acumulan en capas?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Sedimentación",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la capa atmosférica que contiene el gas encargado de filtrar la mayor parte de la radiación ultravioleta del Sol?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "La capa de ozono (estratosfera)",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Qué telescopio, lanzado en 1990, orbita la Tierra y tomó algunas de las imágenes más famosas del universo profundo?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "El Telescopio Espacial Hubble",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el estado en el que los astronautas flotan libremente porque la gravedad efectiva casi no se percibe?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Microgravedad (gravedad cero)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el vehículo robótico de la NASA que explora la superficie de Marte buscando signos de vida pasada?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Perseverance (o Curiosity)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuántos planetas conforman actualmente nuestro sistema solar según la clasificación oficial vigente?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Ocho planetas",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Cómo se llama el pequeño circuito integrado que procesa información dentro de dispositivos electrónicos?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Chip (microchip)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué lenguaje de programación, conocido por su sintaxis simple, es muy utilizado en ciencia de datos e inteligencia artificial?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Python",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de plataformas digitales, como Instagram o Facebook, permiten crear perfiles y conectar personas a través de Internet?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Redes sociales",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué periférico de salida muestra visualmente la información procesada por la computadora?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Monitor (pantalla)",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué bebida de origen animal es reconocida como una de las principales fuentes de calcio en la alimentación diaria?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "La leche",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de alimentos, como las nueces y las almendras, aportan grasas saludables además de proteínas y fibra?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Frutos secos",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Por qué se considera al desayuno una comida clave, ya que rompe el ayuno nocturno y aporta energía para iniciar el día?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Es la primera comida del día tras el ayuno nocturno",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué indicador, basado en el peso y la altura, se utiliza comúnmente para estimar si una persona tiene sobrepeso u obesidad?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "El Índice de Masa Corporal (IMC)",
        imgRespuesta: ""
    }
];

```

### Archivo: `js/trivias/trivia_parte_2.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué reina gobernó Egipto y se alió con Julio César y Marco Antonio?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Cleopatra",
        imgRespuesta: ""
    },
    {
        pregunta: "¿En qué año comenzó la Primera Guerra Mundial?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "1914",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué monumento de piedra caliza de más de 130 metros de altura fue construido como tumba para el faraón Keops?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Gran Pirámide de Giza",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Quién fue el líder del movimiento de independencia de la India basado en la resistencia pacífica?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Mahatma Gandhi",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Cuál es el río más largo y caudaloso del mundo, ubicado en América del Sur?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El río Amazonas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué océano es el más grande de la Tierra por superficie?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Océano Pacífico",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué país tiene la mayor extensión territorial del mundo?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Rusia",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es la capital de Italia, conocida históricamente como 'La Ciudad Eterna'?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Roma",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Qué tipo de células tienen un núcleo definido que contiene su material genético?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Células eucariotas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué pigmento verde en las plantas es responsable de absorber la luz solar durante la fotosíntesis?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "La clorofila",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué gas vital para los seres humanos producen las plantas como desecho durante la fotosíntesis?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Oxígeno",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de seres vivos se alimentan exclusivamente de otros animales?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Carnívoros",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Qué escala sismológica se utiliza comúnmente para medir la magnitud de la energía liberada por un terremoto?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "La escala de Richter",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué material rocoso fundido se encuentra bajo la superficie terrestre antes de ser expulsado por un volcán?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Magma",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de rocas se forman por la acumulación y compactación de sedimentos a lo largo del tiempo?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Rocas sedimentarias",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es la capa sólida y más superficial de la Tierra, sobre la cual habitamos?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "La corteza terrestre",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Cuál es la estrella más cercana a la Tierra, que provee luz y calor a nuestro planeta?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "El Sol",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué planeta de nuestro sistema solar es conocido por tener un brillante y extenso sistema de anillos visibles?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Saturno",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el único satélite natural que orbita alrededor de la Tierra?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "La Luna",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué planeta es conocido popularmente como el 'Planeta Rojo' debido al óxido de hierro en su superficie?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Marte",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué sistema operativo móvil de código abierto, desarrollado por Google, es el más utilizado en el mundo?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Android",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué lenguaje de programación es ampliamente utilizado tanto para desarrollo web dinámico como para aplicaciones con Vanilla JS?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "JavaScript",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué sigla en inglés define a la red inalámbrica de corto alcance que permite conectar dispositivos domésticos y periféricos?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Bluetooth",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué elemento del hardware de almacenamiento de datos no tiene partes móviles, siendo mucho más rápido que un disco duro tradicional?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "SSD (Unidad de Estado Sólido)",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué macronutriente esencial, compuesto por aminoácidos, es fundamental para la construcción y reparación de tejidos y músculos?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Proteínas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué nutriente líquido vital representa aproximadamente el 60% del peso corporal de un adulto y debe consumirse a diario?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Agua",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de vitaminas, como la C y el complejo B, se disuelven en agua y no se almacenan en el cuerpo por mucho tiempo?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Vitaminas hidrosolubles",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de carbohidratos no digeribles, abundantes en la avena y legumbres, mejoran la digestión y previenen el estreñimiento?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "La fibra alimentaria",
        imgRespuesta: ""
    }
];
```

### Archivo: `js/trivias/trivia_parte_3.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué navegante genovés llegó a América en 1492 creyendo haber alcanzado las Indias?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Cristóbal Colón",
        imgRespuesta: ""
    },
    {
        pregunta: "¿En qué país se originó la Revolución Industrial a finales del siglo XVIII?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Gran Bretaña (o Inglaterra)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué imperio construyó la magnífica ciudad de Machu Picchu en los Andes peruanos?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "El Imperio Inca",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué caída de un famoso muro en 1989 simbolizó el fin de la Guerra Fría y la unificación de Alemania?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "El Muro de Berlín",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Cuál es el desierto cálido más grande del mundo, ubicado en el norte de África?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El desierto del Sahara",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué país de América del Norte está compuesto por 50 estados y limita al sur con México?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Estados Unidos",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es la montaña más alta del planeta sobre el nivel del mar, ubicada en el Himalaya?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Monte Everest",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué canal artificial conecta el océano Atlántico con el océano Pacífico a través de América Central?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Canal de Panamá",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Cómo se llaman los animales que no poseen una columna vertebral?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Invertebrados",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué órgano del cuerpo humano se encarga de bombear la sangre a través del sistema circulatorio?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "El corazón",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué nombre recibe el conjunto de poblaciones de diferentes especies que interactúan en un área determinada?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Comunidad (o Biocenosis)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denominan los microorganismos unicelulares que pueden ser beneficiosos o causar infecciones, tratadas con antibióticos?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Bacterias",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Qué tipo de rocas se crean cuando otras rocas se transforman debido a altas presiones y temperaturas sin llegar a fundirse?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Rocas metamórficas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el mineral más duro que existe en la Tierra según la escala de Mohs?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El diamante",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué proceso natural desgasta y transporta los suelos y rocas debido al viento o al agua?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "La erosión",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué nombre reciben los restos o huellas preservadas de organismos antiguos que vivieron en épocas geológicas pasadas?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Fósiles",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Qué nombre reciben los cuerpos rocosos o metálicos de menor tamaño que orbitan alrededor del Sol, principalmente entre Marte y Júpiter?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Asteroides",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denominan las regiones del espacio con una gravedad tan intensa que ni siquiera la luz puede escapar de ellas?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Agujeros negros",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué planeta del sistema solar es el más cercano al Sol y carece de atmósfera significativa?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Mercurio",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de cuerpo celeste helado desarrolla una cola brillante de gas y polvo al acercarse al Sol?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Cometa",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué lenguajes estandarizados se utilizan para estructurar el contenido de una web y aplicarle estilos visuales?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "HTML y CSS",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué término técnico describe al software malicioso diseñado para dañar, espiar o alterar el funcionamiento de un dispositivo?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Malware (o virus informático)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué protocolo seguro de transferencia de hipertexto cifra los datos de navegación web, representado por un candado en el navegador?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "HTTPS",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de aplicación web combina lo mejor de las aplicaciones web y las nativas, pudiendo instalarse y funcionar offline (sigla PWA)?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "PWA (Aplicación Web Progresiva)",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué mineral esencial es vital para la formación y mantenimiento de huesos y dientes fuertes?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "El calcio",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué macronutriente es la fuente de energía más rápida y preferida por el cerebro y el cuerpo humano durante el ejercicio?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Carbohidratos (o hidratos de carbono)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de grasas saludables, presentes en el aceite de oliva, aguacates y frutos secos, ayudan a proteger el sistema cardiovascular?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Grasas insaturadas (monoinsaturadas / poliinsaturadas)",
        imgRespuesta: ""
    },
    {
        pregunta: "Según las pautas de alimentación saludable, ¿qué tipo de productos alimenticios industriales con listas largas de ingredientes químicos se deben evitar?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Alimentos ultraprocesados",
        imgRespuesta: ""
    }
];
```

### Archivo: `js/trivias/trivia_parte_4.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué antigua ciudad-estado griega era famosa por su estricto entrenamiento militar y sus guerreros espartanos?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Esparta",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Quién fue el primer presidente de los Estados Unidos, cuyo rostro aparece en el billete de un dólar?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "George Washington",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué ruta comercial terrestre conectaba a China con el Mediterráneo para el intercambio de bienes de lujo?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Ruta de la Seda",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué conflicto bélico global finalizó tras el lanzamiento de dos bombas atómicas en agosto de 1945?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Segunda Guerra Mundial",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Qué país europeo tiene forma de bota y se adentra en el mar Mediterráneo?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Italia",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es la capital de Francia, famosa por la Torre Eiffel y el Museo del Louvre?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "París",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué isla es la más grande del mundo, perteneciente políticamente a Dinamarca?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Groenlandia",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el único país de América del Sur que tiene costas tanto en el océano Pacífico como en el mar Caribe?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Colombia",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Qué reino biológico agrupa a las setas, hongos y levaduras?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "El reino Fungi",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué orgánulo celular es conocido como la 'central de energía' porque genera la mayor parte de ATP de la célula?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "La mitocondria",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman los animales que se alimentan tanto de plantas como de otros animales?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Omnívoros",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el órgano más grande del cuerpo humano que nos protege de agentes externos?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "La piel",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Qué capa intermedia de la Tierra se sitúa entre la corteza y el núcleo, representando la mayor parte de su volumen?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El manto",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué escala mineralógica de diez niveles se utiliza para medir la dureza de los minerales?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "La escala de Mohs",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el proceso de solidificación de minerales que genera estructuras con formas geométricas ordenadas?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Cristalización",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué fenómeno natural ocurre cuando el magma del interior de la Tierra asciende y sale al exterior de forma violenta o efusiva?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Erupción volcánica",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Qué teoría científica describe el origen y la gran expansión inicial de nuestro universo hace unos 13.800 millones de años?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "La teoría del Big Bang",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el planeta más frío de nuestro sistema solar, clasificado como un gigante helado junto a Neptuno?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Urano",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué instrumento óptico revolucionó la astronomía al permitir observar objetos lejanos en el espacio con gran detalle?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "El telescopio",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la violenta explosión con la que finaliza la vida de una estrella masiva?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Supernova",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué popular plataforma de control de versiones permite a los desarrolladores alojar código y colaborar en proyectos de software?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "GitHub",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tecnología descentralizada de bloques sirve de base para la seguridad y transparencia de las criptomonedas?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Blockchain",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué dispositivo de red dirige el tráfico de datos y conecta múltiples dispositivos locales a la red de Internet?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Router",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué término describe el almacenamiento y acceso a servicios, programas y datos a través de servidores remotos en Internet?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "La nube (o Cloud Computing)",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué mineral es crucial para la producción de hemoglobina, cuya deficiencia puede provocar anemia?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Hierro",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué vitamina, conocida como la 'vitamina del sol', es sintetizada por la piel mediante la exposición solar y fija el calcio?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Vitamina D",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué término describe a la unidad de medida que cuantifica la cantidad de energía que los alimentos aportan a nuestro cuerpo?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Caloría",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué componente de las frutas y verduras actúa protegiendo a las células contra el daño de los radicales libres y el envejecimiento prematuro?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Antioxidantes",
        imgRespuesta: ""
    }
];
```

### Archivo: `js/trivias/trivia_parte_5.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Quién fue el célebre conquistador macedonio que creó uno de los imperios más grandes de la antigüedad antes de cumplir los 30 años?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Alejandro Magno",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué imperio tenía su capital en Constantinopla tras la división del Imperio Romano?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "El Imperio Bizantino (o Imperio Romano de Oriente)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué declaración de derechos fundamentales fue redactada en Francia en 1789 tras la toma de la Bastilla?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Declaración de los Derechos del Hombre y del Ciudadano",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué famoso líder militar autoproclamado emperador de los franceses dominó Europa a principios del siglo XIX?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Napoleón Bonaparte",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Qué país insular de Asia es conocido popularmente como 'La Tierra del Sol Naciente'?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Japón",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es la capital de España, situada geográficamente en el centro de la península ibérica?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Madrid",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué río atraviesa varios países de Europa Central, incluyendo Viena y Budapest, hasta desembocar en el mar Negro?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El río Danubio",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué país sudamericano es el más largo y estrecho del mundo, ubicado entre la cordillera de los Andes y el océano Pacífico?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Chile",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Qué naturalista inglés propuso la teoría de la evolución por selección natural en su obra 'El origen de las especies'?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Charles Darwin",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el proceso por el cual una célula se divide para formar dos células hijas idénticas?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Mitosis",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de vasos sanguíneos llevan la sangre oxigenada desde el corazón hacia el resto del cuerpo?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Las arterias",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la relación ecológica en la que ambos organismos se benefician mutuamente, como las abejas y las flores?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Mutualismo",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Qué mineral blando, el número uno en la escala de Mohs, se usa comúnmente para fabricar polvos corporales?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El talco",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué teoría científica explica la deriva continental y los movimientos de la litosfera terrestre?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "La tectónica de placas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué fosa oceánica, situada en el Pacífico occidental, es el punto más profundo conocido de la corteza terrestre?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "La fosa de las Marianas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué recurso natural no renovable es un líquido oleoso de origen orgánico formado por la descomposición de materia acumulada hace millones de años?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El petróleo",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Qué galaxia espiral gigante es la vecina grande más cercana a nuestra Vía Láctea?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Andrómeda",
        imgRespuesta: ""
    },
    {
        pregunta: "¿En qué año el ser humano pisó la Luna por primera vez durante la misión Apolo 11?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "1969",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué planeta enano fue considerado el noveno planeta del sistema solar hasta su reclasificación en 2006?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Plutón",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el telescopio espacial lanzado en 2021 que es el sucesor científico del telescopio Hubble?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Telescopio Espacial James Webb",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué sigla identifica a los sistemas capaces de simular procesos de inteligencia humana, como el aprendizaje o la resolución de problemas?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "IA (Inteligencia Artificial)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué periférico de entrada se utiliza para introducir comandos de texto e instrucciones escritas en una computadora?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Teclado",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué sistema de archivos y control de versiones distribuido fue creado originalmente por Linus Torvalds en 2005?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Git",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué término describe al conjunto de componentes físicos y tangibles de un sistema informático?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Hardware",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué tipo de azúcares, añadidos a refrescos y golosinas, aumentan el riesgo de caries y diabetes y deben limitarse al mínimo según la OMS?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Azúcares libres (o refinados)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué órgano produce la bilis para ayudar en la digestión y absorción de las grasas que consumimos?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "El hígado",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de vitaminas, como las A, D, E y K, se disuelven en grasas y aceites y pueden almacenarse en el organismo?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Vitaminas liposolubles",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué legumbres son una excelente fuente vegetal de proteínas, hierro y fibra, consumidas ampliamente en guisados y ensaladas?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Lentejas (o porotos/garbanzos)",
        imgRespuesta: ""
    }
];
```

### Archivo: `js/trivias/trivia_parte_6.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué civilización mesoamericana diseñó un preciso calendario astronómico y construyó la pirámide de Chichén Itzá?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La civilización Maya",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué famosa plaga devastadora asoló Europa a mediados del siglo XIV, acabando con un tercio de su población?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Peste Negra",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tratado firmado en 1919 puso fin oficialmente a las hostilidades de la Primera Guerra Mundial?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "El Tratado de Versalles",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Quién fue el rey francés conocido como el 'Rey Sol' que mandó a construir el Palacio de Versalles?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Luis XIV",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Cuál es el país más poblado de América del Sur, cuya capital es Brasilia?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Brasil",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué estrecho natural de agua separa el extremo sur de España del norte de África?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El estrecho de Gibraltar",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el lago navegable más alto del mundo, compartido por Perú y Bolivia?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El lago Titicaca",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué país europeo destaca por sus icónicos molinos de viento, canales de agua y campos de tulipanes?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Países Bajos (u Holanda)",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Qué glóbulos de la sangre son responsables de combatir infecciones y defender al organismo?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Glóbulos blancos (o leucocitos)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué sustancia producida por el páncreas regula los niveles de glucosa en la sangre?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "La insulina",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué animales son capaces de regular su propia temperatura corporal de manera interna, conocidos como 'de sangre caliente'?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Homeotermos (o endotermos)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué estructura rígida de celulosa protege y da soporte a las células vegetales pero está ausente en las células animales?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "La pared celular",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Qué nombre recibe el agua que se filtra y se acumula en depósitos subterráneos bajo la superficie terrestre?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Agua subterránea (o acuífero)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el mineral más común y abundante en la corteza terrestre, compuesto de dióxido de silicio?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El cuarzo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué elemento gaseoso, indispensable para la vida, es también el principal componente de la atmósfera terrestre en un 78%?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El nitrógeno",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denomina al punto de la superficie terrestre situado directamente encima del foco o hipocentro de un terremoto?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Epicentro",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Qué planeta del sistema solar tarda aproximadamente 365 días en completar una órbita alrededor del Sol?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "La Tierra",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué nombre recibe el destello luminoso que se produce cuando un meteoroide entra en la atmósfera terrestre y se quema?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Estrella fugaz (o meteoro)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la frontera imaginaria alrededor de un agujero negro a partir de la cual nada puede escapar?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Horizonte de sucesos",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué planeta del sistema solar destaca por su color azul intenso y por albergar los vientos más veloces registrados?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Neptuno",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué navegador web gratuito y de código abierto fue desarrollado por la fundación Mozilla?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Firefox",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué término inglés define al conjunto de programas, instrucciones y reglas informáticas para ejecutar tareas en una computadora?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Software",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué puerto de conexión estándar y reversible se ha convertido en el estándar universal para cargar y transferir datos en dispositivos?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "USB-C",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué unidad de medida de almacenamiento equivale aproximadamente a 1024 Megabytes?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "1 Gigabyte (GB)",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué glándula en el cuello necesita yodo para producir hormonas que regulan el metabolismo corporal?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "La glándula tiroides",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué vitamina esencial para la visión y el sistema inmune abunda en alimentos de color naranja como la zanahoria y la calabaza?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Vitamina A",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de grasas, sólidas a temperatura ambiente y presentes en carnes rojas y mantequilla, deben consumirse con moderación?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Grasas saturadas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué término define al estado físico y metabólico que resulta de una ingesta insuficiente o desequilibrada de nutrientes esenciales?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Malnutrición (o desnutrición)",
        imgRespuesta: ""
    }
];
```

### Archivo: `js/trivias/trivia_parte_7.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué imperio conquistó Constantinopla en 1453, marcando el fin del Imperio Bizantino?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "El Imperio Otomano",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué pueblo navegante escandinavo realizó incursiones y exploraciones por Europa entre los siglos VIII y XI?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Los Vikingos",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué revolución de 1917 derrocó al zar e instauró un gobierno bolchevique liderado por Lenin?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Revolución Rusa",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué mercader y explorador veneciano viajó a China en el siglo XIII y describió sus experiencias en 'Los viajes de Marco Polo'?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Marco Polo",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Cuál es el río considerado el más largo de África, que atraviesa Egipto antes de desembocar en el Mediterráneo?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El río Nilo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es la capital de Alemania, ciudad que estuvo dividida por un muro durante la Guerra Fría?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Berlín",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué imponente conjunto de cataratas se encuentra en la frontera entre Argentina y Brasil?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Las Cataratas del Iguazú",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuál es el desierto no polar más árido del mundo, ubicado en el norte de Chile?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El desierto de Atacama",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Qué ácido nucleico, similar al ADN pero de cadena simple, interviene en la síntesis de proteínas?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "El ARN",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la célula especializada que transmite impulsos eléctricos en el sistema nervioso?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "La neurona",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denominan los animales que se alimentan exclusivamente de plantas?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Herbívoros",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman los animales que poseen columna vertebral, como los mamíferos y las aves?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Vertebrados",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Cómo se denomina al punto exacto en el interior de la Tierra donde se origina un terremoto?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Hipocentro (o foco sísmico)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman las formaciones rocosas que cuelgan del techo de las cuevas, formadas por el goteo de agua con minerales?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Estalactitas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué combustible fósil sólido, de color negro, se forma a partir de restos vegetales acumulados durante millones de años?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El carbón mineral",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el proceso continuo de evaporación, condensación y precipitación del agua en la naturaleza?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El ciclo del agua",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Qué unidad de distancia se utiliza en astronomía para medir el espacio que recorre la luz en un año?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "El año luz",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué constelación, fácilmente reconocible por su 'cinturón' de tres estrellas alineadas, es una de las más famosas del cielo nocturno?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Orión",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el laboratorio orbital habitado de forma permanente por astronautas desde el año 2000?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "La Estación Espacial Internacional (ISS)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué astrónomo polaco propuso el modelo heliocéntrico, según el cual la Tierra gira alrededor del Sol?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Nicolás Copérnico",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Cómo se denomina a la secuencia ordenada de pasos que resuelve un problema o realiza una tarea en programación?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Algoritmo",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué término describe a un conjunto organizado de datos almacenados y estructurados para su consulta y gestión?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Base de datos",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tecnología permite conectar dispositivos a Internet de forma inalámbrica dentro de un rango determinado?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Wi-Fi",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denomina al software cuyo código fuente puede ser visto, modificado y distribuido libremente por cualquier persona?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Código abierto (Open Source)",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué tipo de grasas, abundantes en pescados como el salmón, son beneficiosas para la salud cardiovascular y cerebral?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Ácidos grasos Omega-3",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué mineral, presente en la banana, es fundamental para el funcionamiento muscular y nervioso?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Potasio",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denominan los microorganismos vivos presentes en yogures y fermentados que benefician la flora intestinal?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Probióticos",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué patrón de alimentación, basado en aceite de oliva, pescado, frutas y verduras, es reconocido por sus beneficios cardiovasculares?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "La dieta mediterránea",
        imgRespuesta: ""
    }
];

```

### Archivo: `js/trivias/trivia_parte_8.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué guerrero y gobernante mongol unificó las tribus nómadas y fundó uno de los imperios contiguos más grandes de la historia?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Gengis Kan",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué monje alemán inició en 1517 el movimiento religioso que dividió al cristianismo occidental?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Martín Lutero (la Reforma Protestante)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué líder venezolano es conocido como 'El Libertador' por su papel en la independencia de varios países sudamericanos?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Simón Bolívar",
        imgRespuesta: ""
    },
    {
        pregunta: "¿En qué legendario conflicto de la mitología griega los griegos usaron un caballo de madera para infiltrarse en una ciudad amurallada?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "La Guerra de Troya",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Cuál es la capital del Reino Unido, atravesada por el río Támesis?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Londres",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué lago, ubicado entre Israel y Jordania, es tan salado que resulta casi imposible hundirse en sus aguas?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Mar Muerto",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué gigantesca formación geológica de Estados Unidos fue esculpida durante millones de años por el río Colorado?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Gran Cañón",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué país nórdico es famoso por sus géiseres, volcanes activos y paisajes de hielo y fuego?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Islandia",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Qué orgánulos celulares son los encargados de fabricar proteínas a partir de las instrucciones del ARN?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Ribosomas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué proceso biológico atraviesa una rana al pasar de renacuajo acuático a adulto terrestre?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Metamorfosis",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la secuencia de organismos donde cada uno se alimenta del anterior, transfiriendo energía?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Cadena alimenticia (o trófica)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué sistema del cuerpo humano, compuesto por glóbulos blancos y anticuerpos, nos defiende de agentes patógenos?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "El sistema inmunológico",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Cómo se llama el fenómeno geotérmico que expulsa periódicamente agua caliente y vapor hacia el cielo?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Géiser",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué roca ígnea oscura, formada por el rápido enfriamiento de la lava, es una de las más comunes de la corteza terrestre?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "El basalto",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama al suelo que permanece congelado de manera permanente durante al menos dos años consecutivos?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Permafrost",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué mineral, formado por la evaporación de agua salada, es la fuente natural de la sal de mesa?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Halita (sal gema)",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Cómo se denomina a los planetas que orbitan alrededor de estrellas distintas al Sol?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Exoplanetas",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué científico italiano perfeccionó el telescopio y confirmó observacionalmente el modelo heliocéntrico en el siglo XVII?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Galileo Galilei",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman los distintos aspectos visibles de la Luna a lo largo de su órbita, como la luna llena o nueva?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Fases lunares",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la región helada más allá de Neptuno donde se originan muchos cometas de corto período?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "El Cinturón de Kuiper",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué término técnico define a la interfaz que permite que dos programas o aplicaciones se comuniquen entre sí?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "API",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué práctica recomienda usar claves largas, únicas y con doble factor para proteger las cuentas digitales?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Ciberseguridad (gestión de contraseñas)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué sistema operativo de código abierto, basado en Unix, es ampliamente utilizado en servidores de todo el mundo?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Linux",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de código de barras bidimensional puede ser escaneado con la cámara de un celular para acceder a un enlace o dato?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Código QR",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué vitamina, presente en cítricos como la naranja, previene el escorbuto y fortalece el sistema inmune?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Vitamina C",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la condición en la que el cuerpo no puede digerir correctamente el azúcar presente en la leche?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Intolerancia a la lactosa",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tipo de colesterol, conocido como 'el bueno', ayuda a eliminar el exceso de colesterol del torrente sanguíneo?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Colesterol HDL",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman los aminoácidos que el cuerpo humano no puede producir por sí mismo y deben obtenerse de la dieta?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Aminoácidos esenciales",
        imgRespuesta: ""
    }
];

```

### Archivo: `js/trivias/trivia_parte_9.js`
```javascript
export const trivia = [
    // --- HISTORIA ---
    {
        pregunta: "¿Qué movimiento cultural y artístico surgido en Italia en el siglo XIV impulsó el retorno a los ideales clásicos grecorromanos?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "El Renacimiento",
        imgRespuesta: ""
    },
    {
        pregunta: "¿En qué año las Trece Colonias declararon su independencia del Reino de Gran Bretaña?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "1776",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué líder sudafricano pasó 27 años preso por luchar contra el sistema de segregación racial conocido como apartheid?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "Nelson Mandela",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el antiguo conjunto de leyes babilónico, uno de los primeros códigos legales escritos de la historia?",
        area: "Historia",
        imgPregunta: "",
        respuesta: "El Código de Hammurabi",
        imgRespuesta: ""
    },

    // --- GEOGRAFÍA ---
    {
        pregunta: "¿Cuál es la capital de Argentina, ubicada a orillas del Río de la Plata?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Buenos Aires",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué golfo, rico en yacimientos de petróleo, se ubica entre la península arábiga e Irán?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "El Golfo Pérsico",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llaman los estrechos entrantes de mar rodeados de montañas escarpadas, típicos del paisaje de Noruega?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "Fiordos",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué vasta región elevada de Asia, conocida como 'el techo del mundo', alberga las cabeceras de varios de los grandes ríos asiáticos?",
        area: "Geografía",
        imgPregunta: "",
        respuesta: "La Meseta del Tíbet",
        imgRespuesta: ""
    },

    // --- BIOLOGÍA ---
    {
        pregunta: "¿Cómo se llama el proceso de división celular que produce células sexuales con la mitad de la carga genética?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Meiosis",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se denominan los animales que dependen de la temperatura ambiente para regular su temperatura corporal, como las serpientes?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Reptiles (animales ectotermos)",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama el proceso mediante el cual el polen se transfiere de una flor a otra para permitir la reproducción de las plantas?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Polinización",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué proteína presente en los glóbulos rojos es la encargada de transportar el oxígeno por todo el cuerpo?",
        area: "Biología",
        imgPregunta: "",
        respuesta: "Hemoglobina",
        imgRespuesta: ""
    },

    // --- GEOLOGÍA ---
    {
        pregunta: "¿Qué roca volcánica de aspecto vítreo y brillante se forma por el enfriamiento extremadamente rápido de la lava?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Obsidiana",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué teoría, propuesta por Alfred Wegener a principios del siglo XX, planteó que los continentes se desplazan lentamente?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "La teoría de la deriva continental",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué fenómeno de olas gigantescas suele generarse tras un terremoto submarino de gran magnitud?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Un tsunami",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la resina fósil de origen vegetal, de tono anaranjado, que a veces conserva insectos atrapados en su interior?",
        area: "Geología",
        imgPregunta: "",
        respuesta: "Ámbar",
        imgRespuesta: ""
    },

    // --- ESPACIO ---
    {
        pregunta: "¿Quién fue el astronauta que se convirtió en la primera persona en pisar la superficie lunar en 1969?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Neil Armstrong",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué cuerpo celeste es el principal responsable de generar las mareas oceánicas de la Tierra?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "La Luna",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cómo se llama la nube de gas y polvo interestelar donde nacen y a veces terminan su vida las estrellas?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "Nebulosa",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Cuánto tiempo, aproximadamente, tarda la Tierra en completar una vuelta sobre su propio eje?",
        area: "Espacio",
        imgPregunta: "",
        respuesta: "24 horas",
        imgRespuesta: ""
    },

    // --- TECNOLOGÍA ---
    {
        pregunta: "¿Qué término define a la computadora remota que almacena datos y ofrece servicios a otros dispositivos conectados a una red?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Servidor",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tecnología inmersiva, mediante un visor especial, sumerge al usuario en un entorno digital simulado en 3D?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Realidad Virtual",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué tecnología permite fabricar objetos físicos tridimensionales a partir de un diseño digital, capa por capa?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Impresión 3D",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué sistema de seguridad informática filtra el tráfico de red para bloquear accesos no autorizados?",
        area: "Tecnología",
        imgPregunta: "",
        respuesta: "Firewall (cortafuegos)",
        imgRespuesta: ""
    },

    // --- NUTRICION ---
    {
        pregunta: "¿Qué mineral, presente en semillas y frutos secos, es clave para la función muscular y la salud ósea?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Magnesio",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué mineral es fundamental para fortalecer el sistema inmunológico y la cicatrización de heridas?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Zinc",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué vitamina, obtenida principalmente de productos de origen animal, es esencial para la formación de glóbulos rojos y el sistema nervioso?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Vitamina B12",
        imgRespuesta: ""
    },
    {
        pregunta: "¿Qué mineral, cuyo exceso se asocia a la hipertensión arterial, está presente principalmente en la sal común?",
        area: "Nutricion",
        imgPregunta: "",
        respuesta: "Sodio",
        imgRespuesta: ""
    }
];

```

### Archivo: `js/trivias_old.js`
```javascript
import { trivia as setTriv } from './trivias/trivia_parte_4.js'; 

export const trivia = setTriv;
```

