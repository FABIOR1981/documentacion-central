# Bundle del Proyecto para IA: proyectivasnuevas

## Estructura de Archivos
- README.md
- configuracion.html
- css/estilos.css
- documentacion/LEEME.md
- index.html
- js/acceso.js
- js/almacenamiento.js
- js/comun.js
- js/configuracion.js
- js/evaluacion.js

---

## Código Fuente

### Archivo: `README.md`
```md
# PsiMatrix – Suite Proyectiva Digital

Aplicación web (HTML + CSS + JavaScript, sin framework ni servidor) para administrar cuatro pruebas proyectivas en pantalla y generar un informe con las producciones del evaluado.

Consta de **dos páginas independientes**:

| Página | Para quién | Qué hace |
|---|---|---|
| `configuracion.html` | El evaluador | Define los parámetros de la evaluación, gestiona perfiles y deja una configuración activa |
| `index.html` | El evaluado / la administración de las pruebas | Registra al evaluado, administra las pruebas y genera el informe. No permite cambiar la configuración |

## Documentación

| Documento | Para quién | Formato |
|---|---|---|
| Manual de Usuario | El profesional que configura y administra las pruebas | [Markdown](https://github.com/FABIOR1981/documentacion-central/blob/main/proyectivasnuevas/documentacion/MANUAL_USUARIO.md) · [PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/proyectivasnuevas/documentacion/Manual_Usuario.pdf) |

## Pruebas incluidas

| Prueba | Qué registra |
|---|---|
| **Test de la Casa** | Dibujo libre (lápiz fino, pincel, goma, color) |
| **Constelación Familiar / Vincular** | Elementos arrastrables en un plano (Principal, Vínculo Cercano, Periférica, Obstáculo, Refugio); se pueden renombrar y eliminar |
| **Dinámica Cromática** | Trazo continuo o manchas con el color elegido |
| **Laberinto Estructural** | Trazo desde la entrada a la salida, con tres niveles de dificultad (ver abajo) |

De cada prueba se guarda la imagen final y el tiempo total de ejecución.

## Flujo de uso

1. **El evaluador abre `configuracion.html`**, ajusta los parámetros (o carga un perfil) y pulsa "Aplicar e ir a la evaluación". Eso deja la configuración como **configuración activa** y abre `index.html`.
2. **Datos del evaluado** (`index.html`): nombre (obligatorio), documento y edad. Solo datos de la persona.
3. **Batería de pruebas:** se muestran solo las pruebas elegidas, en el orden configurado. Se pueden reabrir sin perder lo dibujado. Cada prueba se cierra con "Guardar y Continuar".
4. **Informe:** integra las pruebas guardadas, con observaciones del profesional por prueba y generales.
5. **Salida:** exportar a Word (`.docx`), imprimir / guardar como PDF, o iniciar un nuevo evaluado (la configuración activa se conserva).

La configuración activa **se conserva aunque se cierre la página**: al volver a abrir `index.html`, el registro del evaluado muestra un resumen de la configuración vigente (dispositivo, pruebas, laberinto, límites) y un botón **"Configuración (evaluador)"** que lleva a `configuracion.html` para ajustarla. Ese botón solo está en el registro; las pruebas y el menú no tienen acceso a la configuración. Si hay datos de una evaluación en curso, pide confirmar antes de salir.

Si `index.html` se abre sin una configuración activa, avisa y ofrece ir a `configuracion.html`.

### Acceso del evaluador (`js/acceso.js`)

Quién puede abrir la configuración también está detrás de una interfaz (`access.request(motivo)` devuelve si se permite). Hoy el acceso es libre (`OpenAccess`). Para exigir una contraseña más adelante se cambia solo la última línea de `acceso.js`: los dos puntos de control (el botón de `index.html` y la carga de `configuracion.html`) ya están cableados. Si no se autoriza, `configuracion.html` muestra "Acceso restringido".

Ojo: una contraseña verificada en el navegador es una **barrera de uso, no de seguridad** (quien lea el código puede saltársela). Una restricción real requiere autenticación en un servidor.

## Configuración de la evaluación

Los parámetros se definen en `configuracion.html`, aparte de las pruebas y antes de comenzar. Es responsabilidad del evaluador mantenerlos iguales cuando se necesite comparar entre evaluados.

**Generales**
- Dispositivo y forma de ingreso: computadora con mouse, pantalla táctil, tablet con dedo o lápiz, celular con dedo u otro.
- Qué pruebas incluir y en qué orden.
- Cronómetro visible u oculto para el evaluado.
- Al cumplirse el límite de tiempo de una prueba: solo avisar, o guardar y cerrar la prueba.

**Por prueba** (todas tienen consigna editable y límite de tiempo en minutos, 0 = sin límite)

| Prueba | Parámetros propios |
|---|---|
| Test de la Casa | Permitir color o solo grafito; permitir goma |
| Constelación Familiar | Paleta fija (con modelos Genérica, Familiar y Laboral, editables) o rótulos que escribe el evaluado; máximo de elementos |
| Dinámica Cromática | Paleta libre o restringida a los colores marcados; herramientas disponibles (trazo continuo, mancha) |
| Laberinto | Tipo, dificultad y número de laberinto (ver abajo) |

**Perfiles:** la configuración se puede guardar con un nombre (por ejemplo "Adultos laborales") y volver a cargar. Los perfiles guardan solo parámetros, nunca datos del evaluado ni el dispositivo. Dónde se guardan lo decide la capa de almacenamiento (ver más abajo); por defecto, en el navegador de ese equipo.

**Registro:** en el informe, cada prueba muestra la consigna y los parámetros con que se administró, tal como estaban al guardarla.

## Laberinto

- **Tipos** (se eligen en la configuración):
  - **Rectangular:** paredes finas sobre una cuadrícula fina.
  - **Pasillos anchos:** cuadrícula gruesa dibujada como pasillos anchos (fondo oscuro, pasillos blancos), más fácil de recorrer con el dedo.
  - **Senderos:** el mismo camino de "Pasillos anchos" dibujado como una red de curvas entre puntos, sin cuadrícula visible ni fondo oscuro.
- **Niveles:** cada tipo tiene tres niveles que cambian el tamaño de la cuadrícula.

  | Nivel | Rectangular | Pasillos anchos | Senderos |
  |---|---|---|---|
  | Fácil | 16×9 | 9×5 | 9×5 |
  | Media | 20×11 | 12×7 | 12×7 |
  | Difícil | 25×14 | 15×9 | 15×9 |

  El tamaño de «Pasillos anchos» y «Senderos» está calibrado para que el recorrido óptimo, en proporción al ancho de cada camino dibujado, sea comparable al del rectangular en cada nivel. Ambos comparten la misma cuadrícula de nodos; solo cambia cómo se dibuja.

  En la configuración se ve una vista previa del recorrido óptimo.
- **Numeración:** cada tipo y nivel tiene 9999 laberintos numerados. El mismo tipo, nivel y número dan siempre el mismo laberinto, lo que permite administrar el mismo laberinto a distintos evaluados. Con el campo vacío o con "🎲 Al azar" se elige uno cualquiera.
- **Filtro de calidad:** se descartan automáticamente los laberintos demasiado directos. Cada tipo y nivel exige que el recorrido óptimo esté dentro de una franja de largo y tenga un mínimo de bifurcaciones.
- **Pantalla completa:** botón "⛶ Pantalla completa" para usar toda la pantalla (útil con la tablet en horizontal). Se sale con el botón o con Esc.
- **Medidas del participante** (aparecen en el informe): celdas recorridas frente al óptimo, celdas distintas, si llegó a la salida y:
  - en **Rectangular**, los *cruces de pared* (veces que el trazo atraviesa una pared);
  - en **Pasillos anchos** y **Senderos**, las *salidas del camino* (veces que el trazo pasa de dentro a fuera del camino dibujado; empezar a trazar fuera del camino no cuenta).

  Son medidas **descriptivas**: se calculan muestreando el trazo, por lo que en una esquina puede haber un error de una celda. **No son comparables entre tipos distintos**: para comparar entre evaluados hay que usar el mismo tipo, nivel y número.

## Informe

El informe incluye datos del evaluado, dispositivo, fecha y, por cada prueba: la imagen, el tiempo (con el límite, si se configuró), la consigna, los parámetros usados y las observaciones. Para el laberinto agrega nivel, número, recorrido óptimo y recorrido del participante.

## Privacidad y almacenamiento

Todo ocurre en el navegador y **no se envía nada a ningún servidor**. Los datos del evaluado y sus producciones viven solo en memoria: al cerrar o recargar la pestaña se pierden (la app avisa antes de salir si hay datos). Descargue el informe antes de cerrar.

Lo único que se guarda son **parámetros de configuración** (perfiles y configuración activa), y siempre a través de la capa de almacenamiento.

### Capa de almacenamiento (`js/almacenamiento.js`)

El resto del código no sabe dónde se guardan los parámetros: solo usa el objeto `store`. Cualquier implementación debe cumplir este contrato (todas las operaciones son asíncronas):

| Método | Devuelve |
|---|---|
| `listProfiles()` | nombres de los perfiles |
| `getProfile(name)` | la configuración del perfil, o `null` |
| `saveProfile(name, config)` | crea o reemplaza un perfil |
| `deleteProfile(name)` | elimina un perfil |
| `getActiveConfig()` | la configuración activa, o `null` |
| `setActiveConfig(config)` | fija la configuración activa |
| `describe()` | texto que indica dónde se guarda |

Implementaciones incluidas:
- **`LocalStorageStore`** (por defecto): `localStorage` del navegador. Los perfiles no se comparten entre navegadores ni equipos.
- **`JsonProfilesStore`**: agrega perfiles compartidos desde un archivo JSON del sitio (solo lectura) y delega los perfiles nuevos y la configuración activa en otra implementación. No está activada; para usarla se cambia la última línea de `almacenamiento.js`:

```js
const store = new JsonProfilesStore('data/perfiles.json', new LocalStorageStore());
```

Formato del archivo: `{ "perfiles": { "Adultos laborales": { ...configuración sin dispositivo... } } }`.

Para una **base de datos**, se escribe otra clase con el mismo contrato (en `almacenamiento.js` hay un ejemplo comentado de `ApiStore` sobre una API REST) y se elige en esa misma línea. Ninguna otra parte del código cambia.

## Estructura del repositorio

```
proyectivasnuevas/
├── index.html            Evaluación: registro, pruebas e informe
├── configuracion.html    Configuración de la evaluación (uso del evaluador)
├── css/
│   └── estilos.css       Estilos compartidos (incluye vista de impresión)
├── js/
│   ├── comun.js          Utilidades, modelo de configuración y generación de laberintos
│   ├── almacenamiento.js Capa de almacenamiento (contrato + implementaciones)
│   ├── acceso.js         Control de acceso del evaluador (hoy libre; contraseña a futuro)
│   ├── configuracion.js  Lógica de configuracion.html
│   └── evaluacion.js     Lógica de index.html
└── README.md
```

`index.html` conserva ese nombre porque es el archivo que los servidores (Netlify, GitHub Pages) abren por defecto al entrar a la dirección del sitio.

`evaluacion.js` se organiza en secciones: utilidades y flujo principal, Casa, Constelación, Dinámica Cromática, Laberinto (dibujo, análisis, pantalla completa y registro del recorrido), aplicación de la configuración (cronómetro y límites), guardado e informe.

## Cómo ejecutarla

- **Local:** abrir `configuracion.html` en el navegador, o servir la carpeta con cualquier servidor estático. Como las dos páginas se comunican a través del almacenamiento del navegador, con archivos abiertos desde el disco funciona en Chrome y Edge; en Firefox conviene servirlos (por ejemplo, con `python -m http.server`) o publicarlos.
- **Publicación:** sirve cualquier hosting estático (Netlify, GitHub Pages). Deben subirse juntos `index.html`, `configuracion.html`, `css/` y `js/`.
- **Conexión:** la exportación a Word carga la librería [`docx` 8.5.0](https://www.npmjs.com/package/docx) desde jsDelivr, por lo que necesita internet. Sin conexión queda disponible "Imprimir / Guardar PDF".

## Uso en tablet y celular

El dibujo usa Pointer Events, así que funciona con mouse, dedo y lápiz. Con el dedo el trazo es menos preciso que con lápiz (especialmente en el laberinto), y por eso el dispositivo queda registrado en el informe. Se recomienda usar la tablet en horizontal.

## Alcance y limitaciones

- Las pruebas son **cualitativas**: no incluyen puntuación normativa ni baremos. El laberinto es de diseño propio y no equivale a instrumentos normativos como el de Porteus.
- Los parámetros (dificultad del laberinto, consignas, límites, etc.) los define el profesional; no hay asignación automática por edad.
- Cada parámetro que se cambia afecta la estandarización: para comparar entre evaluados hay que usar la misma configuración (los perfiles ayudan a eso).
- La interpretación de las producciones es responsabilidad del profesional.

```

### Archivo: `configuracion.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PsiMatrix - Configuración de la evaluación</title>
    <link rel="stylesheet" href="css/estilos.css">
</head>
<body>

<div class="container">
    <header>
        <h1>PsiMatrix Suite</h1>
        <p class="subtitle">Configuración de la evaluación · uso del evaluador</p>
    </header>

    <!-- Acceso denegado -->
    <div id="cfg-denied" class="screen">
        <h2>Acceso restringido</h2>
        <p class="subtitle" style="margin-bottom: 25px;">Solo el evaluador puede modificar la configuración.</p>
        <a class="btn-link" href="index.html">Volver a la evaluación</a>
    </div>

    <!-- Configuración de la evaluación (uso del evaluador) -->
    <div id="screen-config" class="screen active">
        <h2>Configuración de la Evaluación</h2>
        <p class="subtitle" style="margin-bottom: 20px;">Ajuste las condiciones de administración antes de comenzar. Todo viene con valores por defecto y lo que se elija queda registrado en el informe. Si va a comparar entre evaluados, use siempre la misma configuración.</p>
        <div id="cfg-status" class="cfg-status" style="display: none;"></div>

        <div class="cfg-block">
            <h3>Perfiles de configuración</h3>
            <div class="cfg-row">
                <select id="cfg-profile" aria-label="Perfil"></select>
                <button class="secondary" type="button" onclick="loadProfile()">Cargar</button>
                <button class="secondary" type="button" onclick="saveProfile()">Guardar como perfil…</button>
                <button class="secondary" type="button" onclick="deleteProfile()">Eliminar</button>
            </div>
            <p class="hint">Los perfiles guardan solo parámetros (nunca datos del evaluado) y no incluyen el dispositivo. La configuración activa y los perfiles se guardan en: <strong id="cfg-storage-info"></strong>.</p>
        </div>

        <div class="cfg-block">
            <h3>Condiciones generales</h3>
            <div class="cfg-field">
                <label for="cfg-device">Dispositivo y forma de ingreso:</label>
                <select id="cfg-device">
                    <option value="">Seleccione...</option>
                    <option>Computadora con mouse</option>
                    <option>Computadora con pantalla táctil (dedo)</option>
                    <option>Tablet con dedo</option>
                    <option>Tablet con lápiz</option>
                    <option>Celular con dedo</option>
                    <option>Otro</option>
                </select>
            </div>
            <div class="cfg-field">
                <label class="cfg-check"><input type="checkbox" id="cfg-timer-visible"> Mostrar el cronómetro al evaluado durante la prueba</label>
            </div>
            <div class="cfg-field">
                <label for="cfg-limit-action">Al cumplirse el límite de tiempo de una prueba:</label>
                <select id="cfg-limit-action">
                    <option value="avisar">Solo avisar (el evaluado puede seguir)</option>
                    <option value="cerrar">Guardar y cerrar la prueba</option>
                </select>
            </div>
        </div>

        <h3 class="cfg-section-title">Pruebas de la batería</h3>
        <p class="hint" style="margin: 0 0 12px 0;">Marque las pruebas que va a administrar y ordénelas con las flechas.</p>
        <div id="cfg-tests">

            <div class="cfg-card" id="cfg-card-house" data-test="house">
                <div class="cfg-card-head">
                    <label class="cfg-check"><input type="checkbox" id="cfg-house-enabled" onchange="updateCfgVisibility()"> <strong>Test de la Casa</strong></label>
                    <span class="cfg-move">
                        <button type="button" aria-label="Subir" onclick="moveCfgCard('house', -1)">↑</button>
                        <button type="button" aria-label="Bajar" onclick="moveCfgCard('house', 1)">↓</button>
                    </span>
                </div>
                <div class="cfg-body">
                    <div class="cfg-field">
                        <label for="cfg-house-consigna">Consigna:</label>
                        <textarea id="cfg-house-consigna" rows="2"></textarea>
                    </div>
                    <div class="cfg-grid">
                        <label class="cfg-check"><input type="checkbox" id="cfg-house-color"> Permitir color (si no, solo grafito)</label>
                        <label class="cfg-check"><input type="checkbox" id="cfg-house-eraser"> Permitir goma</label>
                        <label class="cfg-inline">Límite de tiempo (min, 0 = sin límite): <input type="number" class="cfg-num" id="cfg-house-limit" min="0" max="240"></label>
                    </div>
                </div>
            </div>

            <div class="cfg-card" id="cfg-card-constellation" data-test="constellation">
                <div class="cfg-card-head">
                    <label class="cfg-check"><input type="checkbox" id="cfg-constellation-enabled" onchange="updateCfgVisibility()"> <strong>Constelación Familiar / Vincular</strong></label>
                    <span class="cfg-move">
                        <button type="button" aria-label="Subir" onclick="moveCfgCard('constellation', -1)">↑</button>
                        <button type="button" aria-label="Bajar" onclick="moveCfgCard('constellation', 1)">↓</button>
                    </span>
                </div>
                <div class="cfg-body">
                    <div class="cfg-field">
                        <label for="cfg-constellation-consigna">Consigna:</label>
                        <textarea id="cfg-constellation-consigna" rows="2"></textarea>
                    </div>
                    <div class="cfg-field">
                        <label for="cfg-const-mode">Elementos disponibles:</label>
                        <select id="cfg-const-mode" onchange="updateCfgVisibility()">
                            <option value="fija">Paleta fija (definida por el evaluador)</option>
                            <option value="libre">Rótulos que escribe el evaluado</option>
                        </select>
                    </div>
                    <div id="cfg-const-fixed">
                        <div class="cfg-field">
                            <label for="cfg-const-preset">Cargar un modelo de paleta:</label>
                            <select id="cfg-const-preset" onchange="applyConstPreset()">
                                <option value="">— Elegir modelo —</option>
                                <option value="generica">Genérica</option>
                                <option value="familiar">Familiar</option>
                                <option value="laboral">Laboral</option>
                            </select>
                        </div>
                        <div class="cfg-field">
                            <label for="cfg-const-circles">Círculos (personas / vínculos), un rótulo por línea:</label>
                            <textarea id="cfg-const-circles" rows="4"></textarea>
                        </div>
                        <div class="cfg-field">
                            <label for="cfg-const-rects">Rectángulos (obstáculos / refugios), un rótulo por línea:</label>
                            <textarea id="cfg-const-rects" rows="3"></textarea>
                        </div>
                    </div>
                    <div class="cfg-grid">
                        <label class="cfg-inline">Máximo de elementos (0 = sin límite): <input type="number" class="cfg-num" id="cfg-const-max" min="0" max="50"></label>
                        <label class="cfg-inline">Límite de tiempo (min, 0 = sin límite): <input type="number" class="cfg-num" id="cfg-constellation-limit" min="0" max="240"></label>
                    </div>
                </div>
            </div>

            <div class="cfg-card" id="cfg-card-color" data-test="color">
                <div class="cfg-card-head">
                    <label class="cfg-check"><input type="checkbox" id="cfg-color-enabled" onchange="updateCfgVisibility()"> <strong>Dinámica Cromática</strong></label>
                    <span class="cfg-move">
                        <button type="button" aria-label="Subir" onclick="moveCfgCard('color', -1)">↑</button>
                        <button type="button" aria-label="Bajar" onclick="moveCfgCard('color', 1)">↓</button>
                    </span>
                </div>
                <div class="cfg-body">
                    <div class="cfg-field">
                        <label for="cfg-color-consigna">Consigna:</label>
                        <textarea id="cfg-color-consigna" rows="2"></textarea>
                    </div>
                    <div class="cfg-field">
                        <label for="cfg-color-mode">Paleta de colores:</label>
                        <select id="cfg-color-mode" onchange="updateCfgVisibility()">
                            <option value="libre">Libre (cualquier color)</option>
                            <option value="fija">Restringida a los colores marcados</option>
                        </select>
                    </div>
                    <div id="cfg-color-swatches-wrap" class="cfg-field">
                        <label>Colores disponibles:</label>
                        <div id="cfg-color-swatches" class="cfg-swatches"></div>
                    </div>
                    <div class="cfg-grid">
                        <label class="cfg-check"><input type="checkbox" id="cfg-color-fluid"> Trazo continuo</label>
                        <label class="cfg-check"><input type="checkbox" id="cfg-color-splash"> Mancha / textura</label>
                        <label class="cfg-inline">Límite de tiempo (min, 0 = sin límite): <input type="number" class="cfg-num" id="cfg-color-limit" min="0" max="240"></label>
                    </div>
                </div>
            </div>

            <div class="cfg-card" id="cfg-card-maze" data-test="maze">
                <div class="cfg-card-head">
                    <label class="cfg-check"><input type="checkbox" id="cfg-maze-enabled" onchange="updateCfgVisibility()"> <strong>Laberinto Estructural</strong></label>
                    <span class="cfg-move">
                        <button type="button" aria-label="Subir" onclick="moveCfgCard('maze', -1)">↑</button>
                        <button type="button" aria-label="Bajar" onclick="moveCfgCard('maze', 1)">↓</button>
                    </span>
                </div>
                <div class="cfg-body">
                    <div class="cfg-field">
                        <label for="cfg-maze-consigna">Consigna:</label>
                        <textarea id="cfg-maze-consigna" rows="2"></textarea>
                    </div>
                    <div class="cfg-grid">
                        <label class="cfg-inline">Tipo:
                            <select id="cfg-maze-type" onchange="updateMazePreview()">
                                <option value="rectangular">Rectangular</option>
                                <option value="pasillos">Pasillos anchos</option>
                                <option value="senderos">Senderos</option>
                            </select>
                        </label>
                        <label class="cfg-inline">Dificultad:
                            <select id="cfg-maze-level" onchange="updateMazePreview()">
                                <option value="facil">Fácil</option>
                                <option value="media">Media</option>
                                <option value="dificil">Difícil</option>
                            </select>
                        </label>
                        <label class="cfg-inline">Nº de laberinto (1 a 9999): <input type="number" class="cfg-num" id="cfg-maze-seed" min="1" max="9999" placeholder="al azar" oninput="updateMazePreview()"></label>
                        <button class="secondary" type="button" onclick="cfgRandomMazeSeed()">🎲 Al azar</button>
                        <label class="cfg-inline">Límite de tiempo (min, 0 = sin límite): <input type="number" class="cfg-num" id="cfg-maze-limit" min="0" max="240"></label>
                    </div>
                    <p class="hint" id="cfg-maze-preview" style="margin-top: 10px;"></p>
                    <p class="hint">Cada tipo y nivel tiene 9999 laberintos numerados; el mismo tipo, nivel y número dan siempre el mismo laberinto (útil para comparar entre evaluados). Se descartan automáticamente los demasiado directos. «Pasillos anchos» usa una cuadrícula gruesa con pasillos más fáciles de recorrer con el dedo. «Senderos» dibuja el mismo tipo de camino como una red de curvas entre puntos, sin cuadrícula visible. Las medidas del recorrido no son comparables entre tipos distintos.</p>
                </div>
            </div>

        </div>

        <div class="actions-bar">
            <a class="btn-link secondary" href="index.html">Ir a la evaluación sin cambios</a>
            <div class="actions-group">
                <button class="secondary" onclick="applyConfigAndGo(false)">Solo aplicar</button>
                <button onclick="applyConfigAndGo(true)">Aplicar e ir a la evaluación</button>
            </div>
        </div>
    </div>

</div>

<script src="js/comun.js"></script>
<script src="js/almacenamiento.js"></script>
<script src="js/acceso.js"></script>
<script src="js/configuracion.js"></script>

</body>
</html>

```

### Archivo: `css/estilos.css`
```css
:root {
    --bg-body: #0f172a;
    --bg-container: #1e293b;
    --surface: #334155;
    --brand-primary: #38bdf8;
    --brand-primary-hover: #0ea5e9;
    --brand-accent: #14b8a6;
    --text-main: #f1f5f9;
    --text-muted: #94a3b8;
    --border-color: #475569;
    --radius: 12px;
    --shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3), 0 8px 10px -6px rgba(0, 0, 0, 0.3);
}

* { box-sizing: border-box; }

body {
    font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    background-color: var(--bg-body);
    color: var(--text-main);
    margin: 0;
    padding: 30px 20px;
}

.container {
    max-width: 1000px;
    margin: 0 auto;
    background: var(--bg-container);
    padding: 40px;
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    border: 1px solid var(--border-color);
}

header {
    text-align: center;
    margin-bottom: 35px;
    border-bottom: 2px solid var(--border-color);
    padding-bottom: 20px;
}

h1 {
    color: #ffffff;
    font-size: 1.8rem;
    margin: 0 0 8px 0;
    font-weight: 700;
    letter-spacing: -0.025em;
}

p.subtitle {
    color: var(--text-muted);
    font-size: 1rem;
    margin: 0;
}

.screen { display: none; }
.screen.active { display: block; animation: fadeIn 0.3s ease-in-out; }

@keyframes fadeIn {
    from { opacity: 0; transform: translateY(4px); }
    to { opacity: 1; transform: translateY(0); }
}

/* Formularios */
.form-group { margin-bottom: 20px; }
label {
    display: block;
    font-weight: 600;
    margin-bottom: 8px;
    color: var(--text-main);
    font-size: 0.95rem;
}
input[type="text"], input[type="number"], select {
    padding: 12px 16px;
    border: 1px solid var(--border-color);
    border-radius: 8px;
    font-size: 1rem;
    background-color: #0f172a;
    color: #ffffff;
    transition: all 0.2s;
}
.form-group input, .form-group select { width: 100%; }
input:focus-visible, button:focus-visible, [role="button"]:focus-visible, textarea:focus-visible {
    outline: none;
    border-color: var(--brand-primary);
    box-shadow: 0 0 0 3px rgba(56, 189, 248, 0.35);
}

/* Botones */
button {
    background-color: var(--brand-primary);
    color: #0f172a;
    border: none;
    padding: 12px 24px;
    font-size: 0.95rem;
    font-weight: 700;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s;
    box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}
button:hover { background-color: var(--brand-primary-hover); transform: translateY(-1px); }
button.secondary { background-color: var(--surface); color: var(--text-main); }
button.secondary:hover { background-color: #475569; }

/* Menú de pruebas */
.test-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 20px;
    margin: 25px 0;
}
.test-card {
    border: 1px solid var(--border-color);
    padding: 24px;
    border-radius: var(--radius);
    text-align: left;
    cursor: pointer;
    transition: all 0.2s;
    background: var(--surface);
    position: relative;
    overflow: hidden;
}
.test-card:hover {
    border-color: var(--brand-primary);
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.4);
    transform: translateY(-2px);
}
.test-card h3 { margin: 0 0 8px 0; color: #ffffff; font-size: 1.1rem; }
.test-card p { margin: 0; color: var(--text-muted); font-size: 0.85rem; line-height: 1.4; }
.test-card.completed::after {
    content: '✓ Completado';
    position: absolute;
    top: 12px;
    right: 12px;
    background: rgba(20, 184, 166, 0.2);
    color: #2dd4bf;
    font-size: 0.75rem;
    padding: 2px 8px;
    border-radius: 20px;
    font-weight: 600;
}

/* Lienzos */
.canvas-container {
    border: 2px solid var(--border-color);
    background: #ffffff;
    border-radius: var(--radius);
    position: relative;
    display: block;
    width: 100%;
    overflow: hidden;
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.5);
}
canvas { display: block; width: 100%; height: auto; cursor: crosshair; touch-action: none; }

.toolbar {
    display: flex;
    gap: 12px;
    margin-bottom: 20px;
    align-items: center;
    flex-wrap: wrap;
    background: var(--surface);
    padding: 12px 16px;
    border-radius: 8px;
    border: 1px solid var(--border-color);
}
.toolbar label { margin: 0; font-size: 0.85rem; color: var(--text-main); display: flex; align-items: center; gap: 8px; }
.toolbar button { padding: 8px 16px; font-size: 0.85rem; background: var(--bg-body); color: var(--text-main); }
.toolbar button:hover { background: var(--border-color); }
.toolbar button.active { background: var(--brand-primary); color: #0f172a; }
.toolbar button.accent { background: var(--brand-accent); color: #0f172a; }
.toolbar button.accent:hover { background: #2dd4bf; }
.toolbar input[type="color"] { width: 38px; height: 34px; padding: 0; border: none; background: none; cursor: pointer; }
.toolbar select { padding: 6px 10px; font-size: 0.85rem; }
.toolbar input[type="range"] { width: 120px; padding: 0; }
.toolbar input[type="number"] { width: 110px; padding: 6px 10px; font-size: 0.85rem; }

/* Laberinto a pantalla completa */
#maze-fs-bar { display: none; }
#maze-stage.fs {
    position: fixed;
    inset: 0;
    z-index: 1000;
    background: var(--bg-body);
    display: flex;
    flex-direction: column;
    align-items: center;
}
#maze-stage.fs #maze-fs-bar {
    display: flex;
    justify-content: flex-end;
    gap: 10px;
    width: 100%;
    padding: 6px 10px;
    flex-shrink: 0;
}
#maze-stage.fs #maze-fs-bar button { padding: 8px 16px; font-size: 0.85rem; }
#maze-stage.fs .canvas-container {
    width: auto;
    border: none;
    border-radius: 0;
    box-shadow: none;
    margin: auto 0;
}

/* Constelación */
.constellation-layout { display: flex; gap: 20px; align-items: flex-start; }
.constellation-layout .canvas-container { flex: 1; min-width: 0; }
.constellation-palette {
    width: 220px;
    flex-shrink: 0;
    border: 1px solid var(--border-color);
    padding: 16px;
    border-radius: var(--radius);
    background: var(--surface);
}
.palette-title { font-size: 0.8rem; font-weight: 700; text-transform: uppercase; color: var(--text-muted); margin: 0 0 10px 0; }
.palette-item {
    background: var(--brand-accent);
    color: #0f172a;
    padding: 10px 12px;
    margin-bottom: 10px;
    border-radius: 6px;
    text-align: center;
    cursor: pointer;
    font-size: 0.85rem;
    font-weight: 700;
    transition: background 0.2s;
}
.palette-item:hover { background: #2dd4bf; }
.palette-actions { display: flex; flex-direction: column; gap: 8px; margin-top: 14px; }
.palette-actions button { width: 100%; font-size: 0.8rem; padding: 9px 12px; }
.hint { font-size: 0.75rem; color: var(--text-muted); margin: 12px 0 0 0; line-height: 1.4; }

#constellation-workspace {
    width: 100%;
    aspect-ratio: 1080 / 630;
    position: relative;
    background: #ffffff;
    overflow: hidden;
    touch-action: none;
}
.placed-item {
    position: absolute;
    cursor: move;
    user-select: none;
    -webkit-user-select: none;
    touch-action: none;
    font-size: 0.8rem;
    font-weight: bold;
    box-shadow: 0 4px 6px -1px rgba(0,0,0,0.3);
    z-index: 10;
}
.placed-item.rect {
    padding: 8px 14px;
    background: var(--brand-accent);
    color: #0f172a;
    border-radius: 6px;
    white-space: nowrap;
}
.placed-item.circle {
    width: 68px;
    height: 68px;
    border-radius: 50%;
    background: #0f172a;
    color: #ffffff;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 4px;
    font-size: 0.7rem;
    line-height: 1.1;
    overflow: hidden;
    word-break: break-word;
}
.placed-item.selected { outline: 3px solid var(--brand-primary); outline-offset: 2px; }

/* Informe */
.report-preview {
    border: 1px solid var(--border-color);
    padding: 30px;
    border-radius: var(--radius);
    background: #ffffff;
    color: #334155;
    margin-bottom: 25px;
}
.report-header { border-bottom: 2px solid #e2e8f0; padding-bottom: 15px; margin-bottom: 20px; }
.report-header h3 { margin: 0 0 5px 0; color: #0f172a; }
.report-section { margin-bottom: 30px; }
.report-section h4 { color: #0f172a; margin: 0 0 5px 0; }
.report-meta { margin: 0 0 10px 0; font-size: 0.85rem; color: #64748b; }
.img-result {
    max-width: 100%;
    border: 1px solid #cbd5e1;
    border-radius: 8px;
    margin-top: 10px;
    background: #fff;
    box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
}
.obs-label { display: block; margin: 14px 0 6px 0; font-size: 0.85rem; color: #0f172a; }
.obs {
    width: 100%;
    min-height: 72px;
    padding: 10px;
    border: 1px solid #cbd5e1;
    border-radius: 8px;
    background: #f8fafc;
    color: #0f172a;
    font-family: inherit;
    font-size: 0.9rem;
    resize: vertical;
}
.obs-print { display: none; white-space: pre-wrap; font-size: 0.9rem; color: #0f172a; }
.actions-bar {
    display: flex;
    justify-content: space-between;
    gap: 10px;
    flex-wrap: wrap;
    margin-top: 25px;
    padding-top: 20px;
    border-top: 1px solid var(--border-color);
}
.actions-group { display: flex; gap: 10px; flex-wrap: wrap; }

@media (max-width: 720px) {
    body { padding: 12px 8px; }
    .container { padding: 20px 14px; }
    .constellation-layout { flex-direction: column; }
    .constellation-palette { width: 100%; }
    .report-preview { padding: 16px; }
}

@media print {
    body { background: #fff; padding: 0; }
    .container { background: #fff; border: none; box-shadow: none; padding: 0; max-width: none; }
    header, .actions-bar, .screen > h2, .screen > p.subtitle { display: none !important; }
    .report-preview { border: none; padding: 0; }
    .obs { display: none; }
    .obs-print { display: block; }
    .report-section { break-inside: avoid; }
}

/* ---------- Configuración de la evaluación ---------- */
.cfg-block {
    border: 1px solid var(--border-color);
    background: var(--surface);
    border-radius: var(--radius);
    padding: 16px 18px;
    margin-bottom: 18px;
}
.cfg-block h3, .cfg-section-title { margin: 0 0 10px 0; font-size: 1rem; color: #ffffff; }
.cfg-section-title { margin-top: 24px; }
.cfg-row { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }
.cfg-row select { flex: 1; min-width: 180px; }
.cfg-row button { padding: 9px 14px; font-size: 0.85rem; }

.cfg-card {
    border: 1px solid var(--border-color);
    background: var(--surface);
    border-radius: var(--radius);
    margin-bottom: 14px;
}
.cfg-card-head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
    padding: 12px 16px;
    border-bottom: 1px solid var(--border-color);
}
.cfg-card-head strong { color: #ffffff; }
.cfg-move { display: flex; gap: 6px; }
.cfg-move button { padding: 4px 12px; font-size: 0.95rem; background: var(--bg-body); color: var(--text-main); }
.cfg-move button:hover { background: var(--border-color); }
.cfg-body { padding: 14px 16px; }
.cfg-card.off .cfg-body { opacity: 0.4; pointer-events: none; }

.cfg-field { display: flex; flex-direction: column; gap: 6px; margin-bottom: 12px; }
.cfg-field > label { margin: 0; font-size: 0.85rem; }
.cfg-field select, .cfg-field textarea { width: 100%; }
.cfg-field textarea, .cfg-block textarea {
    padding: 10px 12px;
    border: 1px solid var(--border-color);
    border-radius: 8px;
    background: #0f172a;
    color: #ffffff;
    font-family: inherit;
    font-size: 0.95rem;
    resize: vertical;
}
.cfg-grid { display: flex; flex-wrap: wrap; gap: 10px 24px; align-items: center; }
.cfg-check { display: flex !important; align-items: center; gap: 8px; margin: 0 !important; font-weight: 600; cursor: pointer; }
.cfg-check input { width: auto !important; margin: 0; }
.cfg-inline { display: flex !important; align-items: center; gap: 8px; margin: 0 !important; font-size: 0.85rem; flex-wrap: wrap; }
.cfg-inline select { padding: 8px 10px; font-size: 0.85rem; }
.cfg-num { width: 90px; padding: 8px 10px; font-size: 0.9rem; }
.cfg-swatches { display: flex; flex-wrap: wrap; gap: 8px 16px; }
.cfg-swatch { display: flex !important; align-items: center; gap: 6px; margin: 0 !important; font-weight: 500; font-size: 0.85rem; cursor: pointer; }
.cfg-swatch input { width: auto !important; margin: 0; }
.cfg-swatch .dot { width: 18px; height: 18px; border-radius: 50%; border: 2px solid rgba(255, 255, 255, 0.5); display: inline-block; }

/* Cronómetro y avisos dentro de cada prueba */
.subtitle.consigna { margin: 0 0 14px 0; }
.test-status { display: flex; gap: 16px; align-items: center; flex-wrap: wrap; margin: -6px 0 14px 0; }
.test-status:empty { display: none; }
.test-timer { font-weight: 700; color: var(--brand-primary); font-variant-numeric: tabular-nums; }
.limit-note { color: #fbbf24; font-weight: 600; }

/* Paleta de colores restringida (Dinámica Cromática) */
#paint-swatches { display: none; gap: 8px; align-items: center; flex-wrap: wrap; }
.toolbar button.swatch {
    width: 34px;
    height: 34px;
    padding: 0;
    border-radius: 50%;
    border: 2px solid rgba(255, 255, 255, 0.5);
}
.toolbar button.swatch.active { outline: 3px solid var(--brand-primary); outline-offset: 2px; }

/* ---------- Enlaces con aspecto de botón y avisos de la página de configuración ---------- */
.btn-link {
    display: inline-block;
    background-color: var(--brand-primary);
    color: #0f172a;
    padding: 12px 24px;
    font-size: 0.95rem;
    font-weight: 700;
    border-radius: 8px;
    text-decoration: none;
}
.btn-link:hover { background-color: var(--brand-primary-hover); }
.btn-link.secondary { background-color: var(--surface); color: var(--text-main); }
.btn-link.secondary:hover { background-color: #475569; }

.cfg-status { padding: 10px 14px; border-radius: 8px; margin-bottom: 16px; font-size: 0.9rem; font-weight: 600; }
.cfg-status.ok { background: rgba(20, 184, 166, 0.15); color: #2dd4bf; border: 1px solid #14b8a6; }
.cfg-status.error { background: rgba(244, 63, 94, 0.15); color: #fda4af; border: 1px solid #f43f5e; }

/* ---------- Configuración activa en el registro del evaluado ---------- */
.active-cfg {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 12px;
    flex-wrap: wrap;
    border: 1px solid var(--border-color);
    background: var(--surface);
    border-radius: var(--radius);
    padding: 12px 16px;
    margin-bottom: 25px;
    font-size: 0.85rem;
    color: var(--text-muted);
}
.active-cfg strong { color: var(--text-main); }
.active-cfg button { padding: 8px 14px; font-size: 0.8rem; }

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/proyectivasnuevas/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PsiMatrix - Suite Proyectiva Digital</title>
    <link rel="stylesheet" href="css/estilos.css">
</head>
<body>

<div class="container">
    <header>
        <h1>PsiMatrix Suite</h1>
        <p class="subtitle">Plataforma Digital de Evaluación y Producción Proyectiva Clínica</p>
    </header>

    <!-- Pantalla 1: Datos del evaluado -->
    <div id="screen-patient" class="screen active">
        <h2>Registro del Evaluado</h2>
        <p class="subtitle" style="margin-bottom: 20px;">Ingrese los datos del evaluado para iniciar el protocolo de pruebas.</p>
        <div class="active-cfg">
            <div><strong>Configuración activa:</strong> <span id="active-cfg-summary"></span></div>
            <button class="secondary" type="button" onclick="openEvaluatorConfig()">Configuración (evaluador)</button>
        </div>
        <div class="form-group">
            <label for="p-name">Nombre y Apellido:</label>
            <input type="text" id="p-name" placeholder="Ej. Roberto Sánchez" autocomplete="off">
        </div>
        <div class="form-group">
            <label for="p-id">Documento / ID:</label>
            <input type="text" id="p-id" placeholder="Ej. 3.456.789-2" autocomplete="off">
        </div>
        <div class="form-group">
            <label for="p-age">Edad:</label>
            <input type="number" id="p-age" placeholder="Ej. 38" min="0" max="120">
        </div>
        <div style="margin-top: 30px;">
            <button onclick="startEvaluation()">Continuar</button>
        </div>
    </div>


    <!-- Sin configuración activa -->
    <div id="screen-noconfig" class="screen">
        <h2>No hay una configuración activa</h2>
        <p class="subtitle" style="margin-bottom: 25px;">El evaluador debe definir la configuración de la evaluación antes de comenzar.</p>
        <a class="btn-link" href="configuracion.html">Ir a la configuración</a>
    </div>

    <!-- Pantalla 2: Menú de pruebas -->
    <div id="screen-menu" class="screen">
        <div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 8px; margin-bottom: 15px;">
            <h2 style="margin:0;">Batería de Pruebas</h2>
            <span style="font-size: 0.9rem; color: var(--text-muted);">Evaluado: <strong id="lbl-patient-name" style="color: var(--brand-primary);"></strong></span>
        </div>
        <p class="subtitle">Seleccione una técnica para administrar. Se muestran en el orden configurado.</p>

        <div class="test-grid" id="test-grid">
            <div class="test-card" id="card-house" role="button" tabindex="0" onclick="loadTest('house')">
                <h3>Test de la Casa</h3>
                <p>Espacio habitable y estructura defensiva.</p>
            </div>
            <div class="test-card" id="card-constellation" role="button" tabindex="0" onclick="loadTest('constellation')">
                <h3>Constelación Familiar</h3>
                <p>Dinámica vincular y distribución espacial.</p>
            </div>
            <div class="test-card" id="card-color" role="button" tabindex="0" onclick="loadTest('color')">
                <h3>Dinámica Cromática</h3>
                <p>Expresión afectiva y flujo pulsional.</p>
            </div>
            <div class="test-card" id="card-maze" role="button" tabindex="0" onclick="loadTest('maze')">
                <h3>Laberinto Estructural</h3>
                <p>Planificación y control de impulsos (Adultos).</p>
            </div>
        </div>

        <div class="actions-bar">
            <button class="secondary" onclick="switchScreen('screen-patient')">Modificar Datos</button>
            <button onclick="finishAll()">Finalizar y Generar Informe</button>
        </div>
    </div>

    <!-- Pantalla 3A: Test de la Casa -->
    <div id="screen-house" class="screen">
        <h2>Test de la Casa (Espacio Habitable)</h2>
        <p class="subtitle consigna" id="consigna-house"></p>
        <div class="test-status"><span class="test-timer" id="timer-house"></span><span class="limit-note" id="limit-note-house"></span></div>
        <div class="toolbar">
            <button id="btn-house-pen" data-tool="pen" class="active" onclick="setHouseTool('pen', this)">Lápiz fino</button>
            <button id="btn-house-brush" data-tool="brush" onclick="setHouseTool('brush', this)">Pincel</button>
            <button id="btn-house-eraser" data-tool="eraser" onclick="setHouseTool('eraser', this)">Goma</button>
            <label id="lbl-house-color">Color: <input type="color" id="house-color" value="#0f172a"></label>
            <button class="secondary" onclick="confirmClear('houseCanvas', 'house')">Limpiar Lienzo</button>
        </div>
        <div class="canvas-container">
            <canvas id="houseCanvas" width="1080" height="630"></canvas>
        </div>
        <div class="actions-bar">
            <button class="secondary" onclick="backToMenu()">Regresar al Menú</button>
            <button onclick="saveTestResult('house')">Guardar y Continuar</button>
        </div>
    </div>

    <!-- Pantalla 3B: Constelación Familiar -->
    <div id="screen-constellation" class="screen">
        <h2>Constelación Familiar / Vincular</h2>
        <p class="subtitle consigna" id="consigna-constellation"></p>
        <div class="test-status"><span class="test-timer" id="timer-constellation"></span><span class="limit-note" id="limit-note-constellation"></span></div>
        <div class="constellation-layout">
            <div class="constellation-palette">
                <p class="palette-title">Elementos:</p>
                <div id="palette-items"></div>
                <div class="palette-actions">
                    <button class="secondary" onclick="renameSelected()">Renombrar seleccionado</button>
                    <button class="secondary" onclick="deleteSelected()">Eliminar seleccionado</button>
                    <button class="secondary" onclick="clearConstellation()">Limpiar Plano</button>
                </div>
                <p class="hint">Arrastre para mover. Toque un elemento para seleccionarlo; doble clic para renombrarlo.</p>
            </div>
            <div class="canvas-container">
                <div id="constellation-workspace"></div>
            </div>
        </div>
        <div class="actions-bar">
            <button class="secondary" onclick="backToMenu()">Regresar al Menú</button>
            <button onclick="saveTestResult('constellation')">Guardar y Continuar</button>
        </div>
    </div>

    <!-- Pantalla 3C: Dinámica Cromática -->
    <div id="screen-color" class="screen">
        <h2>Dinámica Cromática y Expresión Emocional</h2>
        <p class="subtitle consigna" id="consigna-color"></p>
        <div class="test-status"><span class="test-timer" id="timer-color"></span><span class="limit-note" id="limit-note-color"></span></div>
        <div class="toolbar">
            <button id="btn-color-fluid" data-tool="fluid" class="active" onclick="setColorTool('fluid', this)">Trazo Continuo</button>
            <button id="btn-color-splash" data-tool="splash" onclick="setColorTool('splash', this)">Mancha / Textura</button>
            <label id="lbl-paint-color">Color: <input type="color" id="paint-color" value="#0ea5e9"></label>
            <span id="paint-swatches"></span>
            <button class="secondary" onclick="confirmClear('colorCanvas', 'color')">Limpiar Lienzo</button>
        </div>
        <div class="canvas-container">
            <canvas id="colorCanvas" width="1080" height="630"></canvas>
        </div>
        <div class="actions-bar">
            <button class="secondary" onclick="backToMenu()">Regresar al Menú</button>
            <button onclick="saveTestResult('color')">Guardar y Continuar</button>
        </div>
    </div>

    <!-- Pantalla 3D: Laberinto -->
    <div id="screen-maze" class="screen">
        <h2>Laberinto Estructural Complejo (Adultos)</h2>
        <p class="subtitle consigna" id="consigna-maze"></p>
        <div class="test-status"><span class="test-timer" id="timer-maze"></span><span class="limit-note" id="limit-note-maze"></span></div>
        <div class="toolbar">
            <label>Grosor de trazo: <input type="range" id="maze-size" min="2" max="6" value="3"></label>
            <button class="secondary" onclick="clearMazePath()">Borrar trazo</button>
            <button class="accent" onclick="toggleMazeFullscreen()">⛶ Pantalla completa</button>
        </div>
        <div id="maze-stage">
            <div id="maze-fs-bar">
                <button class="secondary" onclick="clearMazePath()">Borrar trazo</button>
                <button onclick="toggleMazeFullscreen()">Salir de pantalla completa</button>
            </div>
            <div class="canvas-container">
                <canvas id="mazeCanvas" width="1080" height="630"></canvas>
            </div>
        </div>
        <p class="hint" id="maze-info" style="margin-top: 10px;"></p>
        <div class="actions-bar">
            <button class="secondary" onclick="backToMenu()">Regresar al Menú</button>
            <button onclick="saveTestResult('maze')">Guardar y Continuar</button>
        </div>
    </div>

    <!-- Pantalla 4: Informe final -->
    <div id="screen-report" class="screen">
        <h2>Protocolo e Informe Clínico Final</h2>
        <p class="subtitle" style="margin-bottom: 20px;">Resumen integrado de las producciones proyectivas del evaluado.</p>

        <div class="report-preview" id="report-content">
            <div class="report-header">
                <h3>PsiMatrix - Reporte Proyectivo Digital</h3>
                <p><strong>Evaluado:</strong> <span id="rep-name"></span> | <strong>ID:</strong> <span id="rep-id"></span> | <strong>Edad:</strong> <span id="rep-age"></span></p>
                <p style="margin: 4px 0 0 0; color: #64748b; font-size: 0.85rem;">Dispositivo de administración: <span id="rep-device"></span></p>
                <p style="margin: 4px 0 0 0; color: #64748b; font-size: 0.85rem;">Fecha de Evaluación: <span id="rep-date"></span></p>
            </div>
            <div id="rep-results-container"></div>
        </div>

        <div class="actions-bar">
            <button class="secondary" onclick="backToMenu()">Regresar al Menú</button>
            <div class="actions-group">
                <button class="secondary" onclick="resetAll()">Nuevo evaluado</button>
                <button class="secondary" onclick="window.print()">Imprimir / Guardar PDF</button>
                <button onclick="exportToWord()">Exportar Informe a Word (.docx)</button>
            </div>
        </div>
    </div>
</div>

<!-- Librería para generar .docx real (con imágenes incrustadas) -->
<script src="https://cdn.jsdelivr.net/npm/docx@8.5.0/build/index.umd.js"></script>
<script src="js/comun.js"></script>
<script src="js/almacenamiento.js"></script>
<script src="js/acceso.js"></script>
<script src="js/evaluacion.js"></script>

</body>
</html>

```

### Archivo: `js/acceso.js`
```javascript
/* ============================================================================
   Acceso del evaluador

   Decide quién puede abrir la configuración. Igual que el almacenamiento, está detrás de una
   interfaz: hoy el acceso es libre, y mañana se puede exigir una contraseña cambiando SOLO la
   última línea de este archivo (los dos puntos de control ya están cableados en la aplicación:
   el botón «Configuración (evaluador)» de index.html y la carga de configuracion.html).

   Contrato:
     request(motivo)  → Promise<boolean>   true si se permite el acceso
     describe()       → string             cómo se controla el acceso (texto para mostrar)

   Importante: una contraseña verificada en el navegador es una barrera de uso, no de seguridad
   (quien lea el código puede saltársela). Una restricción real necesita autenticación en un servidor.
   ============================================================================ */

// --- Implementación actual: acceso libre ---
class OpenAccess {
    async request() { return true; }
    describe() { return 'acceso libre (sin contraseña)'; }
}

/* Ejemplo de lo que sería con contraseña (mismo contrato, otra clase):

   class PasswordAccess {
       constructor(verify) { this.verify = verify; }   // verify(clave) → Promise<boolean>, por ejemplo contra un servidor
       async request(motivo) {
           const clave = prompt('Contraseña del evaluador:');
           return clave !== null && await this.verify(clave);
       }
       describe() { return 'protegido con contraseña'; }
   }
*/

// >>> Único lugar donde se decide cómo se controla el acceso <<<
const access = new OpenAccess();
// const access = new PasswordAccess(async (clave) => (await fetch('/api/verificar', { method: 'POST', body: clave })).ok);

```

### Archivo: `js/almacenamiento.js`
```javascript
/* ============================================================================
   Capa de almacenamiento (patrón repositorio)

   El resto de la aplicación NO sabe dónde se guardan los datos: solo usa el objeto `store`
   que se crea al final de este archivo. Para cambiar el destino (archivo JSON, base de datos)
   basta con escribir otra clase con el mismo contrato y elegirla en la última línea.

   Contrato (todas las operaciones devuelven Promesas, para que una implementación remota
   pueda ser asíncrona sin tocar el resto del código):

     listProfiles()             → Promise<string[]>        nombres de los perfiles
     getProfile(name)           → Promise<object|null>     configuración del perfil
     saveProfile(name, config)  → Promise<void>            crea o reemplaza un perfil
     deleteProfile(name)        → Promise<void>
     getActiveConfig()          → Promise<object|null>     configuración vigente para las evaluaciones
     setActiveConfig(config)    → Promise<void>
     describe()                 → string                   dónde se guarda (para mostrarlo al evaluador)

   Si una operación no es posible (por ejemplo, modificar un perfil de solo lectura) la
   implementación debe lanzar un Error con un mensaje entendible: la interfaz lo muestra tal cual.

   Solo se guardan parámetros de configuración. Los datos del evaluado y sus producciones
   nunca pasan por esta capa.
   ============================================================================ */

// --- Implementación por defecto: localStorage del navegador ---
class LocalStorageStore {
    constructor(prefix = 'psimatrix_') {
        this.profilesKey = prefix + 'perfiles_v1';
        this.activeKey = prefix + 'config_activa_v1';
    }
    _read(key, fallback) {
        try {
            const raw = localStorage.getItem(key);
            return raw ? JSON.parse(raw) : fallback;
        } catch (e) { return fallback; }
    }
    _write(key, value) {
        try { localStorage.setItem(key, JSON.stringify(value)); }
        catch (e) { throw new Error('No se pudo escribir en el almacenamiento del navegador.'); }
    }
    async listProfiles() { return Object.keys(this._read(this.profilesKey, {})).sort(); }
    async getProfile(name) { return this._read(this.profilesKey, {})[name] || null; }
    async saveProfile(name, config) {
        const all = this._read(this.profilesKey, {});
        all[name] = config;
        this._write(this.profilesKey, all);
    }
    async deleteProfile(name) {
        const all = this._read(this.profilesKey, {});
        delete all[name];
        this._write(this.profilesKey, all);
    }
    async getActiveConfig() { return this._read(this.activeKey, null); }
    async setActiveConfig(config) { this._write(this.activeKey, config); }
    describe() { return 'este navegador (localStorage)'; }
}

// --- Perfiles compartidos desde un archivo JSON externo (solo lectura) ---
// Formato del archivo:  { "perfiles": { "Adultos laborales": { ...configuración sin dispositivo... } } }
// Los perfiles nuevos y la configuración activa se delegan en otra implementación (`base`).
class JsonProfilesStore {
    constructor(url, base) {
        this.url = url;
        this.base = base;
        this._cache = null;
    }
    async _remote() {
        if (this._cache) return this._cache;
        try {
            const res = await fetch(this.url, { cache: 'no-store' });
            if (!res.ok) throw new Error('HTTP ' + res.status);
            const data = await res.json();
            this._cache = data && typeof data.perfiles === 'object' && data.perfiles ? data.perfiles : {};
        } catch (e) { this._cache = {}; }   // sin archivo o sin conexión: solo se ven los perfiles locales
        return this._cache;
    }
    async listProfiles() {
        const remote = Object.keys(await this._remote());
        const local = await this.base.listProfiles();
        return [...new Set([...remote, ...local])].sort();
    }
    async getProfile(name) {
        const remote = await this._remote();
        return remote[name] || this.base.getProfile(name);
    }
    async _assertEditable(name) {
        if ((await this._remote())[name]) {
            throw new Error(`«${name}» es un perfil del archivo compartido y no se puede modificar desde la aplicación.`);
        }
    }
    async saveProfile(name, config) { await this._assertEditable(name); return this.base.saveProfile(name, config); }
    async deleteProfile(name) { await this._assertEditable(name); return this.base.deleteProfile(name); }
    getActiveConfig() { return this.base.getActiveConfig(); }
    setActiveConfig(config) { return this.base.setActiveConfig(config); }
    describe() { return `archivo ${this.url} (solo lectura) y ${this.base.describe()}`; }
}

/* Ejemplo de cómo sería una base de datos detrás de una API (mismo contrato, otra clase):

   class ApiStore {
       constructor(baseUrl) { this.baseUrl = baseUrl; }
       async _json(path, options) {
           const res = await fetch(this.baseUrl + path, { headers: { 'Content-Type': 'application/json' }, ...options });
           if (!res.ok) throw new Error('El servidor respondió ' + res.status);
           return res.status === 204 ? null : res.json();
       }
       listProfiles()            { return this._json('/perfiles'); }
       getProfile(name)          { return this._json('/perfiles/' + encodeURIComponent(name)); }
       saveProfile(name, config) { return this._json('/perfiles/' + encodeURIComponent(name), { method: 'PUT', body: JSON.stringify(config) }); }
       deleteProfile(name)       { return this._json('/perfiles/' + encodeURIComponent(name), { method: 'DELETE' }); }
       getActiveConfig()         { return this._json('/configuracion-activa'); }
       setActiveConfig(config)   { return this._json('/configuracion-activa', { method: 'PUT', body: JSON.stringify(config) }); }
       describe()                { return 'el servidor ' + this.baseUrl; }
   }
*/

// >>> Único lugar donde se decide dónde se guardan los datos <<<
const store = new LocalStorageStore();
// Perfiles compartidos desde un JSON del sitio (solo lectura) además de los locales:
// const store = new JsonProfilesStore('data/perfiles.json', new LocalStorageStore());

```

### Archivo: `js/comun.js`
```javascript
// Utilidades, constantes, modelo de configuración y generación de laberintos.
// Se comparte entre la página de la evaluación (index.html) y la de configuración (configuracion.html).

const CW = 1080, CH = 630;           // resolución interna de todos los lienzos (dibujo y geometría)
const $ = (id) => document.getElementById(id);

// Crea un elemento DOM: el('div', { className: 'x' }, [hijos])
function el(tag, props, children) {
    const e = document.createElement(tag);
    Object.assign(e, props || {});
    (children || []).forEach(c => e.appendChild(typeof c === 'string' ? document.createTextNode(c) : c));
    return e;
}

const TEST_KEYS = ['house', 'constellation', 'color', 'maze'];
const TESTS = {
    house:         { title: 'Test de la Casa (Espacio Habitable)',        short: 'Test de la Casa' },
    constellation: { title: 'Constelación Familiar / Vincular',           short: 'Constelación Familiar' },
    color:         { title: 'Dinámica Cromática y Expresión Emocional',   short: 'Dinámica Cromática' },
    maze:          { title: 'Laberinto Estructural Complejo (Adultos)',   short: 'Laberinto Estructural' }
};

// ---------- LABERINTO ----------
// Cada nivel tiene su propia cuadrícula y una franja de dificultad aceptable, medida sobre el
// recorrido óptimo (celdas de la entrada a la salida vs. el mínimo posible) y las bifurcaciones
// que hay en ese recorrido. Los laberintos demasiado directos se descartan de forma
// determinista: el mismo tipo + nivel + número dan siempre el mismo laberinto.
const MAZE_MAX = 9999;   // por tipo y nivel: laberintos numerados del 1 al MAZE_MAX
// Tipos de laberinto. Comparten el mismo modelo (celdas y pasajes); cambian la cuadrícula y el dibujo.
//   rectangular: paredes finas sobre una cuadrícula fina.
//   pasillos:    cuadrícula gruesa dibujada como pasillos anchos (más fácil de trazar con el dedo).
const MAZE_TYPES = {
    rectangular: {
        label: 'Rectangular',
        levels: {
            facil:   { label: 'Fácil',   cols: 16, rows: 9,  loops: 3, ratio: [2.0, 3.0], minJunctions: 6 },
            media:   { label: 'Media',   cols: 20, rows: 11, loops: 3, ratio: [2.8, 3.8], minJunctions: 9 },
            dificil: { label: 'Difícil', cols: 25, rows: 14, loops: 2, ratio: [3.6, 5.0], minJunctions: 13 }
        }
    },
    pasillos: {
        label: 'Pasillos anchos',
        exitLabel: 'Salidas del pasillo',   // nombre de la medida de "cruces" para este tipo (ver mazeTraceMeta)
        levels: {
            facil:   { label: 'Fácil',   cols: 9,  rows: 5, loops: 1, ratio: [2.0, 3.0], minJunctions: 3 },
            media:   { label: 'Media',   cols: 12, rows: 7, loops: 1, ratio: [2.8, 3.8], minJunctions: 6 },
            dificil: { label: 'Difícil', cols: 15, rows: 9, loops: 2, ratio: [3.6, 5.0], minJunctions: 9 }
        }
    },
    senderos: {
        label: 'Senderos',
        exitLabel: 'Salidas del sendero',
        levels: {
            facil:   { label: 'Fácil',   cols: 9,  rows: 5, loops: 2, ratio: [2.0, 3.0], minJunctions: 3 },
            media:   { label: 'Media',   cols: 12, rows: 7, loops: 2, ratio: [2.8, 3.8], minJunctions: 6 },
            dificil: { label: 'Difícil', cols: 15, rows: 9, loops: 3, ratio: [3.6, 5.0], minJunctions: 9 }
        }
    }
};
// Tipos cuyo trazo es un camino con ancho (tubo), no una cuadrícula de paredes: pasillos y senderos.
const TUBE_TYPES = new Set(['pasillos', 'senderos']);
const mazeCfg = (type, level) => MAZE_TYPES[type].levels[level];
const randomMazeSeed = () => 1 + Math.floor(Math.random() * MAZE_MAX);

function mulberry32(a) {
    return function () {
        a |= 0; a = (a + 0x6D2B79F5) | 0;
        let t = Math.imul(a ^ (a >>> 15), 1 | a);
        t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
        return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
    };
}

function buildMaze(rnd, cols, rows, loops) {
    const cells = [];
    for (let r = 0; r < rows; r++)
        for (let c = 0; c < cols; c++)
            cells.push({ r, c, w: { t: true, r: true, b: true, l: true }, v: false });
    const at = (r, c) => cells[r * cols + c];

    // Recorrido en profundidad (backtracker): laberinto perfecto
    const stack = [at(0, 0)];
    at(0, 0).v = true;
    while (stack.length) {
        const cur = stack[stack.length - 1];
        const nb = [];
        if (cur.r > 0 && !at(cur.r - 1, cur.c).v) nb.push([at(cur.r - 1, cur.c), 't', 'b']);
        if (cur.c < cols - 1 && !at(cur.r, cur.c + 1).v) nb.push([at(cur.r, cur.c + 1), 'r', 'l']);
        if (cur.r < rows - 1 && !at(cur.r + 1, cur.c).v) nb.push([at(cur.r + 1, cur.c), 'b', 't']);
        if (cur.c > 0 && !at(cur.r, cur.c - 1).v) nb.push([at(cur.r, cur.c - 1), 'l', 'r']);
        if (!nb.length) { stack.pop(); continue; }
        const [n, a, b] = nb[Math.floor(rnd() * nb.length)];
        cur.w[a] = false; n.w[b] = false; n.v = true;
        stack.push(n);
    }

    // Pasillos alternativos: solo paredes interiores, abriendo ambos lados
    let opened = 0, tries = 0;
    while (opened < loops && tries < 1000) {
        tries++;
        const cell = cells[Math.floor(rnd() * cells.length)];
        if (rnd() < 0.5) {
            if (cell.c < cols - 1 && cell.w.r) { cell.w.r = false; at(cell.r, cell.c + 1).w.l = false; opened++; }
        } else {
            if (cell.r < rows - 1 && cell.w.b) { cell.w.b = false; at(cell.r + 1, cell.c).w.t = false; opened++; }
        }
    }
    return cells;
}

// ---------- Senderos: geometría de nodos y curvas (pura, sin canvas) ----------
// Posición de cada nodo en el lienzo (CW × CH), con un desplazamiento aleatorio pero determinista:
// depende solo del número de laberinto y la cuadrícula, no del intento que generó el laberinto.
function trailPositions(seed, cols, rows) {
    const cw = CW / cols, ch = CH / rows;
    const rnd = mulberry32((Math.imul(seed, 2654435761) ^ 0x9E3779B9) >>> 0);
    const pos = [];
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            const jx = (rnd() - 0.5) * cw * 0.5;
            const jy = (rnd() - 0.5) * ch * 0.5;
            pos.push([(c + 0.5) * cw + jx, (r + 0.5) * ch + jy]);
        }
    }
    return pos;
}

// Puntos de una curva (Bézier cuadrática) entre dos nodos, con una curvatura determinista según el
// número de laberinto y el borde. Se usa tanto para dibujar como para medir el trazo del participante.
function curvePoints(a, b, seed, edgeId, dir, n = 10) {
    const hrnd = mulberry32((seed * 97 + edgeId * 131 + (dir === 'r' ? 17 : 53)) >>> 0);
    const off = hrnd() - 0.5;
    const mx = (a[0] + b[0]) / 2, my = (a[1] + b[1]) / 2;
    const dx = b[0] - a[0], dy = b[1] - a[1];
    const len = Math.hypot(dx, dy) || 1;
    const nx = -dy / len, ny = dx / len;
    const o = off * len * 0.2;
    const cx = mx + nx * o, cy = my + ny * o;
    const pts = [];
    for (let k = 0; k <= n; k++) {
        const t = k / n, u = 1 - t;
        pts.push([u * u * a[0] + 2 * u * t * cx + t * t * b[0], u * u * a[1] + 2 * u * t * cy + t * t * b[1]]);
    }
    return pts;
}

// Recorrido óptimo (BFS) y bifurcaciones a lo largo de ese recorrido
function analyzeMaze(cells, cols, rows) {
    const prev = new Array(cells.length).fill(-1);
    const queue = [0];
    prev[0] = 0;
    for (let q = 0; q < queue.length; q++) {
        const i = queue[q], c = cells[i];
        const ns = [];
        if (!c.w.t) ns.push(i - cols);
        if (!c.w.b) ns.push(i + cols);
        if (!c.w.l) ns.push(i - 1);
        if (!c.w.r) ns.push(i + 1);
        ns.forEach(n => { if (prev[n] < 0) { prev[n] = i; queue.push(n); } });
    }
    const path = [];
    for (let i = cells.length - 1; i !== 0; i = prev[i]) path.push(i);
    path.push(0);
    let junctions = 0;
    path.slice(1, -1).forEach(i => {
        const open = Object.values(cells[i].w).filter(x => !x).length;
        if (open >= 3) junctions++;
    });
    return { length: path.length, junctions, ratio: path.length / (cols + rows - 1) };
}

function generateMaze(seed, levelKey, typeKey = 'rectangular') {
    const cfg = mazeCfg(typeKey, levelKey);
    let best = null, bestDev = Infinity;
    for (let attempt = 0; attempt < 600; attempt++) {
        const rnd = mulberry32((Math.imul(seed, 2654435761) + Math.imul(attempt, 40503)) >>> 0);
        const cells = buildMaze(rnd, cfg.cols, cfg.rows, cfg.loops);
        const info = analyzeMaze(cells, cfg.cols, cfg.rows);
        const dev = Math.max(0, cfg.ratio[0] - info.ratio, info.ratio - cfg.ratio[1]) +
                    Math.max(0, cfg.minJunctions - info.junctions) / 10;
        if (dev === 0) return { cells, info };
        if (dev < bestDev) { bestDev = dev; best = { cells, info }; }
    }
    return best;   // no debería ocurrir: se devuelve el más cercano a la franja
}

// ---------- CONFIGURACIÓN DE LA EVALUACIÓN ----------
// Los parámetros se definen antes de comenzar (pantalla de configuración), vienen con valores por
// defecto y lo que efectivamente se usó en cada prueba queda registrado en el informe.
const COLOR_PALETTE = [
    { id: 'rojo',     name: 'Rojo',     hex: '#dc2626' },
    { id: 'naranja',  name: 'Naranja',  hex: '#ea580c' },
    { id: 'amarillo', name: 'Amarillo', hex: '#facc15' },
    { id: 'verde',    name: 'Verde',    hex: '#16a34a' },
    { id: 'celeste',  name: 'Celeste',  hex: '#0ea5e9' },
    { id: 'azul',     name: 'Azul',     hex: '#1d4ed8' },
    { id: 'violeta',  name: 'Violeta',  hex: '#7c3aed' },
    { id: 'marron',   name: 'Marrón',   hex: '#78350f' },
    { id: 'negro',    name: 'Negro',    hex: '#0f172a' }
];
const CONST_PRESETS = {
    generica: { circles: ['Principal', 'Vínculo Cercano', 'F. Periférica'], rects: ['Obstáculo', 'Refugio'] },
    familiar: { circles: ['Yo', 'Madre', 'Padre', 'Hermano/a', 'Pareja', 'Hijo/a', 'Otro'], rects: ['Obstáculo', 'Refugio'] },
    laboral:  { circles: ['Yo', 'Jefe/a', 'Compañero/a', 'Equipo', 'Cliente'], rects: ['Obstáculo', 'Refugio'] }
};
function defaultConfig() {
    return {
        device: '',
        timerVisible: false,
        limitAction: 'avisar',
        order: TEST_KEYS.slice(),
        tests: {
            house: {
                enabled: true, limitMin: 0, color: true, eraser: true,
                consigna: 'Dibuje una casa o espacio habitable utilizando las herramientas provistas.'
            },
            constellation: {
                enabled: true, limitMin: 0, mode: 'fija', maxItems: 0,
                circles: CONST_PRESETS.generica.circles.slice(), rects: CONST_PRESETS.generica.rects.slice(),
                consigna: 'Seleccione elementos de la paleta y ubíquelos libremente en el plano.'
            },
            color: {
                enabled: true, limitMin: 0, paletteMode: 'libre', colors: COLOR_PALETTE.map(c => c.id),
                tools: { fluid: true, splash: true },
                consigna: 'Exprese libremente un estado afectivo o vivencia mediante el color y el trazo fluido.'
            },
            maze: {
                enabled: true, limitMin: 0, type: 'rectangular', level: 'media', seed: randomMazeSeed(),
                consigna: 'Trace un recorrido continuo desde la Entrada (Verde) hasta la Salida (Roja).'
            }
        }
    };
}

function mergeDeep(def, src) {
    if (!src || typeof src !== 'object') return def;
    Object.keys(def).forEach(k => {
        if (!(k in src)) return;
        if (def[k] && typeof def[k] === 'object' && !Array.isArray(def[k])) def[k] = mergeDeep(def[k], src[k]);
        else if (typeof src[k] === typeof def[k] && Array.isArray(src[k]) === Array.isArray(def[k])) def[k] = src[k];
    });
    return def;
}


// Deja una configuración (venga de donde venga: perfil, JSON, base de datos) completa y con valores válidos
function normalizeConfig(src) {
    const cfg = mergeDeep(defaultConfig(), src);
    if (!Array.isArray(cfg.order) || cfg.order.length !== TEST_KEYS.length || !TEST_KEYS.every(k => cfg.order.includes(k))) cfg.order = TEST_KEYS.slice();
    if (!['avisar', 'cerrar'].includes(cfg.limitAction)) cfg.limitAction = 'avisar';
    TEST_KEYS.forEach(k => {
        const l = cfg.tests[k].limitMin;
        if (!Number.isInteger(l) || l < 0 || l > 240) cfg.tests[k].limitMin = 0;
    });
    if (!TEST_KEYS.some(k => cfg.tests[k].enabled)) TEST_KEYS.forEach(k => { cfg.tests[k].enabled = true; });

    const c = cfg.tests.constellation;
    if (!['fija', 'libre'].includes(c.mode)) c.mode = 'fija';
    if (!Number.isInteger(c.maxItems) || c.maxItems < 0 || c.maxItems > 50) c.maxItems = 0;
    const labels = (a) => a.filter(x => typeof x === 'string' && x.trim()).map(x => x.trim().slice(0, 24));
    c.circles = labels(c.circles);
    c.rects = labels(c.rects);
    if (c.mode === 'fija' && !c.circles.length && !c.rects.length) { c.mode = 'libre'; }

    const cc = cfg.tests.color;
    if (!['libre', 'fija'].includes(cc.paletteMode)) cc.paletteMode = 'libre';
    cc.colors = cc.colors.filter(id => COLOR_PALETTE.some(p => p.id === id));
    if (cc.paletteMode === 'fija' && !cc.colors.length) cc.paletteMode = 'libre';
    if (!cc.tools.fluid && !cc.tools.splash) cc.tools.fluid = true;

    const m = cfg.tests.maze;
    if (!MAZE_TYPES[m.type]) m.type = 'rectangular';
    if (!mazeCfg(m.type, m.level)) m.level = 'media';
    if (!Number.isInteger(m.seed) || m.seed < 1 || m.seed > MAZE_MAX) m.seed = randomMazeSeed();
    return cfg;
}

```

### Archivo: `js/configuracion.js`
```javascript
// Página de configuración de la evaluación (uso del evaluador).
// Depende de comun.js (modelo y utilidades) y almacenamiento.js (dónde se guardan perfiles y configuración activa).

// --- Formulario de configuración ---
function buildConfigUI() {
    const box = $('cfg-color-swatches');
    COLOR_PALETTE.forEach(c => {
        const cb = el('input', { type: 'checkbox', value: c.id });
        const dot = el('span', { className: 'dot' });
        dot.style.background = c.hex;
        box.appendChild(el('label', { className: 'cfg-swatch' }, [cb, dot, c.name]));
    });
}

function fillConfigForm(cfg) {
    $('cfg-device').value = cfg.device;
    $('cfg-timer-visible').checked = cfg.timerVisible;
    $('cfg-limit-action').value = cfg.limitAction;
    cfg.order.forEach(k => $('cfg-tests').appendChild($('cfg-card-' + k)));
    TEST_KEYS.forEach(k => {
        $('cfg-' + k + '-enabled').checked = cfg.tests[k].enabled;
        $('cfg-' + k + '-consigna').value = cfg.tests[k].consigna;
        $('cfg-' + k + '-limit').value = cfg.tests[k].limitMin;
    });
    const h = cfg.tests.house;
    $('cfg-house-color').checked = h.color;
    $('cfg-house-eraser').checked = h.eraser;
    const c = cfg.tests.constellation;
    $('cfg-const-mode').value = c.mode;
    $('cfg-const-circles').value = c.circles.join('\n');
    $('cfg-const-rects').value = c.rects.join('\n');
    $('cfg-const-max').value = c.maxItems;
    const cc = cfg.tests.color;
    $('cfg-color-mode').value = cc.paletteMode;
    $('cfg-color-swatches').querySelectorAll('input').forEach(i => { i.checked = cc.colors.includes(i.value); });
    $('cfg-color-fluid').checked = cc.tools.fluid;
    $('cfg-color-splash').checked = cc.tools.splash;
    $('cfg-maze-type').value = cfg.tests.maze.type;
    $('cfg-maze-level').value = cfg.tests.maze.level;
    $('cfg-maze-seed').value = cfg.tests.maze.seed;
    updateCfgVisibility();
    updateMazePreview();
}

function updateCfgVisibility() {
    TEST_KEYS.forEach(k => $('cfg-card-' + k).classList.toggle('off', !$('cfg-' + k + '-enabled').checked));
    $('cfg-const-fixed').style.display = $('cfg-const-mode').value === 'fija' ? '' : 'none';
    $('cfg-color-swatches-wrap').style.display = $('cfg-color-mode').value === 'fija' ? '' : 'none';
}

function moveCfgCard(k, dir) {
    const card = $('cfg-card-' + k), box = card.parentElement;
    if (dir < 0 && card.previousElementSibling) box.insertBefore(card, card.previousElementSibling);
    if (dir > 0 && card.nextElementSibling) box.insertBefore(card.nextElementSibling, card);
}

function applyConstPreset() {
    const sel = $('cfg-const-preset'), p = CONST_PRESETS[sel.value];
    if (p) {
        $('cfg-const-circles').value = p.circles.join('\n');
        $('cfg-const-rects').value = p.rects.join('\n');
    }
    sel.value = '';
}

function cfgRandomMazeSeed() {
    $('cfg-maze-seed').value = randomMazeSeed();
    updateMazePreview();
}

function updateMazePreview() {
    const type = $('cfg-maze-type').value, level = $('cfg-maze-level').value, raw = $('cfg-maze-seed').value.trim(), box = $('cfg-maze-preview');
    if (raw === '') { box.textContent = 'Campo vacío: se elegirá un laberinto al azar al confirmar.'; return; }
    const seed = Number(raw);
    if (!Number.isInteger(seed) || seed < 1 || seed > MAZE_MAX) {
        box.textContent = `Ingrese un número entero entre 1 y ${MAZE_MAX}.`;
        return;
    }
    const cfg = mazeCfg(type, level), g = generateMaze(seed, level, type);
    box.textContent = `Cuadrícula ${cfg.cols}×${cfg.rows} · Recorrido óptimo: ${g.info.length} celdas · Bifurcaciones en ese recorrido: ${g.info.junctions}`;
}

// Lee y valida el formulario; devuelve la configuración o null (mostrando el motivo)
function readConfigForm() {
    const fail = (msg, id) => { alert(msg); if (id && $(id)) $(id).focus(); return null; };
    const cfg = {
        device: $('cfg-device').value,
        timerVisible: $('cfg-timer-visible').checked,
        limitAction: $('cfg-limit-action').value,
        order: [...$('cfg-tests').children].map(c => c.dataset.test),
        tests: {}
    };
    if (!cfg.device) return fail('Seleccione el dispositivo y la forma de ingreso con que se realizarán las pruebas.', 'cfg-device');

    TEST_KEYS.forEach(k => {
        cfg.tests[k] = {
            enabled: $('cfg-' + k + '-enabled').checked,
            consigna: $('cfg-' + k + '-consigna').value.trim(),
            limitMin: Number($('cfg-' + k + '-limit').value || 0)
        };
    });
    if (!TEST_KEYS.some(k => cfg.tests[k].enabled)) return fail('Seleccione al menos una prueba para la batería.');

    for (const k of TEST_KEYS) {
        const lim = cfg.tests[k].limitMin;
        if (cfg.tests[k].enabled && (!Number.isInteger(lim) || lim < 0 || lim > 240)) {
            return fail(`Límite de tiempo de «${TESTS[k].short}»: ingrese un número entero de minutos entre 0 y 240 (0 = sin límite).`, 'cfg-' + k + '-limit');
        }
    }

    Object.assign(cfg.tests.house, { color: $('cfg-house-color').checked, eraser: $('cfg-house-eraser').checked });

    const lines = (id) => $(id).value.split('\n').map(s => s.trim().slice(0, 24)).filter(Boolean);
    const maxItems = Number($('cfg-const-max').value || 0);
    Object.assign(cfg.tests.constellation, {
        mode: $('cfg-const-mode').value, circles: lines('cfg-const-circles'), rects: lines('cfg-const-rects'), maxItems
    });
    if (cfg.tests.constellation.enabled) {
        if (!Number.isInteger(maxItems) || maxItems < 0 || maxItems > 50) return fail('Máximo de elementos de la Constelación: ingrese un entero entre 0 y 50 (0 = sin límite).', 'cfg-const-max');
        if (cfg.tests.constellation.mode === 'fija' && !cfg.tests.constellation.circles.length && !cfg.tests.constellation.rects.length) {
            return fail('La paleta fija de la Constelación necesita al menos un elemento.', 'cfg-const-circles');
        }
    }

    Object.assign(cfg.tests.color, {
        paletteMode: $('cfg-color-mode').value,
        colors: [...$('cfg-color-swatches').querySelectorAll('input')].filter(i => i.checked).map(i => i.value),
        tools: { fluid: $('cfg-color-fluid').checked, splash: $('cfg-color-splash').checked }
    });
    if (cfg.tests.color.enabled) {
        if (!cfg.tests.color.tools.fluid && !cfg.tests.color.tools.splash) return fail('Dinámica Cromática: habilite al menos una herramienta.');
        if (cfg.tests.color.paletteMode === 'fija' && !cfg.tests.color.colors.length) return fail('Dinámica Cromática: marque al menos un color disponible.');
    }

    const raw = $('cfg-maze-seed').value.trim();
    let seed = randomMazeSeed();
    if (raw !== '') {
        seed = Number(raw);
        if (cfg.tests.maze.enabled && (!Number.isInteger(seed) || seed < 1 || seed > MAZE_MAX)) {
            return fail(`Número de laberinto: ingrese un entero entre 1 y ${MAZE_MAX}, o deje el campo vacío para elegir uno al azar.`, 'cfg-maze-seed');
        }
        if (!Number.isInteger(seed) || seed < 1 || seed > MAZE_MAX) seed = randomMazeSeed();
    }
    Object.assign(cfg.tests.maze, { type: $('cfg-maze-type').value, level: $('cfg-maze-level').value, seed });
    return cfg;
}


// --- Mensajes en pantalla ---
function showStatus(msg, isError) {
    const box = $('cfg-status');
    box.textContent = msg || '';
    box.className = 'cfg-status ' + (isError ? 'error' : 'ok');
    box.style.display = msg ? '' : 'none';
}

// --- Perfiles (a través de la capa de almacenamiento: no importa dónde se guarden) ---
async function refreshProfileSelect(selected) {
    const sel = $('cfg-profile');
    sel.innerHTML = '';
    sel.appendChild(el('option', { value: '', textContent: 'Valores por defecto' }));
    let names = [];
    try { names = await store.listProfiles(); }
    catch (e) { showStatus('No se pudieron leer los perfiles: ' + e.message, true); }
    names.forEach(n => sel.appendChild(el('option', { value: n, textContent: n })));
    sel.value = names.includes(selected) ? selected : '';
}

async function saveProfile() {
    const cfg = readConfigForm();
    if (!cfg) return;
    const name = (prompt('Nombre del perfil (por ejemplo «Adultos laborales»):') || '').trim();
    if (!name) return;
    try {
        if ((await store.listProfiles()).includes(name) && !confirm('Ya existe un perfil con ese nombre. ¿Reemplazarlo?')) return;
        const copy = JSON.parse(JSON.stringify(cfg));
        delete copy.device;                       // el dispositivo no forma parte del perfil
        await store.saveProfile(name, copy);
    } catch (e) { showStatus(e.message, true); return; }
    await refreshProfileSelect(name);
    showStatus(`Perfil «${name}» guardado.`);
}

async function loadProfile() {
    const name = $('cfg-profile').value;
    const device = $('cfg-device').value;
    let cfg = defaultConfig();
    if (name) {
        let saved = null;
        try { saved = await store.getProfile(name); }
        catch (e) { showStatus(e.message, true); return; }
        if (!saved) { showStatus('No se encontró el perfil.', true); return; }
        cfg = normalizeConfig(saved);
    }
    cfg.device = device;
    fillConfigForm(cfg);
    showStatus(name ? `Perfil «${name}» cargado en el formulario. Falta aplicarlo.` : 'Valores por defecto cargados en el formulario. Falta aplicarlos.');
}

async function deleteProfile() {
    const name = $('cfg-profile').value;
    if (!name) { alert('Elija primero un perfil para eliminarlo.'); return; }
    if (!confirm(`¿Eliminar el perfil «${name}»?`)) return;
    try { await store.deleteProfile(name); }
    catch (e) { showStatus(e.message, true); return; }
    await refreshProfileSelect();
    showStatus(`Perfil «${name}» eliminado.`);
}

// --- Aplicar la configuración: queda como configuración activa para las evaluaciones ---
async function applyConfigAndGo(go) {
    const cfg = readConfigForm();
    if (!cfg) return;
    try { await store.setActiveConfig(cfg); }
    catch (e) { showStatus('No se pudo aplicar la configuración: ' + e.message, true); return; }
    if (go) location.href = 'index.html';
    else showStatus('Configuración aplicada. Las evaluaciones que se abran desde ahora usarán estos parámetros.');
}

// --- Inicio de la página ---
async function initConfigPage() {
    let allowed = false;
    try { allowed = await access.request('configuracion'); } catch (e) { allowed = false; }
    if (!allowed) {
        $('screen-config').classList.remove('active');
        $('cfg-denied').classList.add('active');
        return;
    }
    buildConfigUI();
    $('cfg-storage-info').textContent = store.describe();
    let active = null;
    try { active = await store.getActiveConfig(); }
    catch (e) { showStatus('No se pudo leer la configuración activa: ' + e.message, true); }
    fillConfigForm(active ? normalizeConfig(active) : defaultConfig());
    await refreshProfileSelect();
}
initConfigPage();

```

### Archivo: `js/evaluacion.js`
```javascript
// ---------- Constantes y estado ----------
let patient = { name: '', id: '', age: '' };
let testData = { house: null, constellation: null, color: null, maze: null };
let elapsedMs = { house: 0, constellation: 0, color: 0, maze: 0 };
let dirty = { house: false, constellation: false, color: false, maze: false };
let obs = { house: '', constellation: '', color: '', maze: '', general: '' };
let activeTest = null, startTime = 0;

// ---------- Utilidades ----------
function switchScreen(screenId) {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    $(screenId).classList.add('active');
    window.scrollTo(0, 0);
}

function clearCanvas(canvas) {
    const ctx = canvas.getContext('2d');
    ctx.save();
    ctx.globalAlpha = 1;
    ctx.fillStyle = '#FFFFFF';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.restore();
}

// Coordenadas del puntero en el espacio interno del lienzo (funciona aunque el lienzo se escale por CSS)
function canvasPoint(canvas, e) {
    const r = canvas.getBoundingClientRect();
    return {
        x: (e.clientX - r.left) * canvas.width / r.width,
        y: (e.clientY - r.top) * canvas.height / r.height
    };
}

// Dibujo con Pointer Events: mouse, táctil y lápiz
function bindDrawing(canvas, h) {
    let drawing = false, last = null;
    canvas.addEventListener('pointerdown', (e) => {
        if (e.pointerType === 'mouse' && e.button !== 0) return;
        e.preventDefault();
        canvas.setPointerCapture(e.pointerId);
        drawing = true;
        last = canvasPoint(canvas, e);
        h.draw(last, last);
    });
    canvas.addEventListener('pointermove', (e) => {
        if (!drawing) return;
        const p = canvasPoint(canvas, e);
        h.draw(last, p);
        last = p;
    });
    const stop = () => { drawing = false; last = null; };
    canvas.addEventListener('pointerup', stop);
    canvas.addEventListener('pointercancel', stop);
}

function setActiveTool(btn) {
    btn.parentElement.querySelectorAll('button[data-tool]').forEach(b => b.classList.toggle('active', b === btn));
}

function fmtTime(sec) {
    const m = Math.floor(sec / 60), s = sec % 60;
    return m > 0 ? `${m} min ${String(s).padStart(2, '0')} s` : `${s} s`;
}

// ---------- Flujo principal ----------
function startEvaluation() {
    const name = $('p-name').value.trim();
    const age = $('p-age').value.trim();
    if (!name) {
        alert('Por favor ingrese al menos el nombre del evaluado.');
        $('p-name').focus();
        return;
    }
    if (age !== '' && (isNaN(age) || Number(age) < 0 || Number(age) > 120)) {
        alert('La edad debe ser un número entre 0 y 120.');
        $('p-age').focus();
        return;
    }
    patient.name = name;
    patient.id = $('p-id').value.trim() || 'S/D';
    patient.age = age || 'S/D';
    $('lbl-patient-name').textContent = patient.name;
    refreshMenu();
    switchScreen('screen-menu');
}

function refreshMenu() {
    TEST_KEYS.forEach(k => $('card-' + k).classList.toggle('completed', !!testData[k]));
}

function enterTest(type) { activeTest = type; startTime = Date.now(); }
function leaveTest() {
    if (activeTest) elapsedMs[activeTest] += Date.now() - startTime;
    activeTest = null;
}

function loadTest(type) {
    enterTest(type);
    switchScreen('screen-' + type);
}

function backToMenu(force) {
    if (!force && activeTest && dirty[activeTest] &&
        !confirm('Hay cambios sin guardar en esta prueba. ¿Regresar al menú de todos modos?\n(El informe usará la última versión guardada.)')) {
        return;
    }
    leaveTest();
    refreshMenu();
    switchScreen('screen-menu');
}

function confirmClear(canvasId, type) {
    if (!confirm('¿Limpiar el lienzo? Se perderá lo dibujado.')) return;
    clearCanvas($(canvasId));
    dirty[type] = true;
}

// ---------- CASA ----------
let houseTool = 'pen';
function setHouseTool(t, btn) { houseTool = t; setActiveTool(btn); }

function initHouseCanvas() {
    const c = $('houseCanvas');
    c.width = CW; c.height = CH;
    clearCanvas(c);
    const ctx = c.getContext('2d');
    const widths = { pen: 4, brush: 15, eraser: 30 };
    bindDrawing(c, {
        draw: (a, b) => {
            ctx.save();
            ctx.lineCap = 'round';
            ctx.lineJoin = 'round';
            ctx.lineWidth = widths[houseTool];
            ctx.strokeStyle = houseTool === 'eraser' ? '#FFFFFF' : $('house-color').value;
            ctx.beginPath();
            ctx.moveTo(a.x, a.y);
            ctx.lineTo(b.x, b.y);
            ctx.stroke();
            ctx.restore();
            dirty.house = true;
        }
    });
}

// ---------- CONSTELACIÓN FAMILIAR ----------
let selectedItem = null;

function selectItem(div) {
    if (selectedItem) selectedItem.classList.remove('selected');
    selectedItem = div;
    if (div) div.classList.add('selected');
}

function placeItem(div, fx, fy) {
    div.dataset.fx = fx;
    div.dataset.fy = fy;
    div.style.left = (fx * 100) + '%';
    div.style.top = (fy * 100) + '%';
}

function addConstellationItem(label, isCircle) {
    const ws = $('constellation-workspace');
    const max = config.tests.constellation.maxItems;
    if (max && ws.children.length >= max) {
        alert(`Se alcanzó el máximo de ${max} elementos.`);
        return;
    }
    const div = document.createElement('div');
    div.className = 'placed-item ' + (isCircle ? 'circle' : 'rect');
    div.dataset.circle = isCircle ? '1' : '0';
    div.textContent = label;

    const n = ws.children.length;                       // posición inicial escalonada
    placeItem(div, 0.04 + ((n * 0.07) % 0.55), 0.06 + ((n * 0.09) % 0.6));
    ws.appendChild(div);

    div.addEventListener('pointerdown', (e) => {
        e.preventDefault();
        e.stopPropagation();
        selectItem(div);
        div.setPointerCapture(e.pointerId);
        const er = div.getBoundingClientRect();
        const ox = e.clientX - er.left, oy = e.clientY - er.top;
        const move = (ev) => {
            const wr = ws.getBoundingClientRect();
            const x = Math.max(0, Math.min(wr.width - div.offsetWidth, ev.clientX - wr.left - ox));
            const y = Math.max(0, Math.min(wr.height - div.offsetHeight, ev.clientY - wr.top - oy));
            placeItem(div, x / wr.width, y / wr.height);
            dirty.constellation = true;
        };
        const up = () => {
            div.removeEventListener('pointermove', move);
            div.removeEventListener('pointerup', up);
            div.removeEventListener('pointercancel', up);
        };
        div.addEventListener('pointermove', move);
        div.addEventListener('pointerup', up);
        div.addEventListener('pointercancel', up);
    });
    div.addEventListener('dblclick', () => { selectItem(div); renameSelected(); });

    selectItem(div);
    dirty.constellation = true;
}

function renameSelected() {
    if (!selectedItem) { alert('Seleccione primero un elemento del plano.'); return; }
    const t = prompt('Rótulo del elemento:', selectedItem.textContent);
    if (t !== null && t.trim()) {
        selectedItem.textContent = t.trim().slice(0, 24);
        dirty.constellation = true;
    }
}

function deleteSelected() {
    if (!selectedItem) { alert('Seleccione primero un elemento del plano.'); return; }
    selectedItem.remove();
    selectedItem = null;
    dirty.constellation = true;
}

function wipeConstellation() {
    $('constellation-workspace').innerHTML = '';
    selectedItem = null;
}

function clearConstellation() {
    if ($('constellation-workspace').children.length && !confirm('¿Limpiar el plano? Se quitarán todos los elementos.')) return;
    wipeConstellation();
    dirty.constellation = true;
}

function wrapText(ctx, text, cx, cy, maxWidth, lineHeight) {
    const words = text.split(/\s+/);
    const lines = [];
    let line = '';
    words.forEach(w => {
        const test = line ? line + ' ' + w : w;
        if (line && ctx.measureText(test).width > maxWidth) { lines.push(line); line = w; }
        else line = test;
    });
    if (line) lines.push(line);
    const y0 = cy - ((lines.length - 1) * lineHeight) / 2;
    lines.forEach((l, i) => ctx.fillText(l, cx, y0 + i * lineHeight));
}

// Genera la imagen de la constelación reproduciendo lo que se ve en pantalla
function renderConstellationImage() {
    const ws = $('constellation-workspace');
    const wr = ws.getBoundingClientRect();
    const c = document.createElement('canvas');
    c.width = CW; c.height = CH;
    const ctx = c.getContext('2d');
    ctx.fillStyle = '#FFFFFF';
    ctx.fillRect(0, 0, CW, CH);
    const k = CW / wr.width;
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';

    ws.querySelectorAll('.placed-item').forEach(el => {
        const er = el.getBoundingClientRect();
        const x = (er.left - wr.left) * k, y = (er.top - wr.top) * k;
        const w = er.width * k, h = er.height * k;
        const fs = parseFloat(getComputedStyle(el).fontSize) * k;
        ctx.font = `bold ${fs}px system-ui, sans-serif`;
        if (el.dataset.circle === '1') {
            ctx.fillStyle = '#0f172a';
            ctx.beginPath();
            ctx.ellipse(x + w / 2, y + h / 2, w / 2, h / 2, 0, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = '#FFFFFF';
            wrapText(ctx, el.textContent, x + w / 2, y + h / 2, w * 0.85, fs * 1.1);
        } else {
            ctx.fillStyle = '#14b8a6';
            ctx.fillRect(x, y, w, h);
            ctx.fillStyle = '#0f172a';
            ctx.fillText(el.textContent, x + w / 2, y + h / 2);
        }
    });
    return c;
}

// ---------- DINÁMICA CROMÁTICA ----------
let colorTool = 'fluid';
function setColorTool(t, btn) { colorTool = t; setActiveTool(btn); }

function initColorCanvas() {
    const c = $('colorCanvas');
    c.width = CW; c.height = CH;
    clearCanvas(c);
    const ctx = c.getContext('2d');
    bindDrawing(c, {
        draw: (a, b) => {
            const col = $('paint-color').value;   // se lee siempre al dibujar
            ctx.save();
            if (colorTool === 'fluid') {
                ctx.lineCap = 'round';
                ctx.lineJoin = 'round';
                ctx.lineWidth = 14;
                ctx.strokeStyle = col;
                ctx.beginPath();
                ctx.moveTo(a.x, a.y);
                ctx.lineTo(b.x, b.y);
                ctx.stroke();
            } else {
                ctx.fillStyle = col;
                ctx.globalAlpha = 0.35;
                for (let i = 0; i < 5; i++) {
                    const ang = Math.random() * Math.PI * 2, d = Math.random() * 26;
                    ctx.beginPath();
                    ctx.arc(b.x + Math.cos(ang) * d, b.y + Math.sin(ang) * d, 10 + Math.random() * 22, 0, Math.PI * 2);
                    ctx.fill();
                }
            }
            ctx.restore();
            dirty.color = true;
        }
    });
}

// ---------- LABERINTO (dibujo, recorrido y pantalla completa) ----------
let COLS = 20, ROWS = 11;
let mazeCells = null, mazeSeed = 1, mazeLevel = 'media', mazeType = 'rectangular', mazeInfo = null, mazeHasPath = false;
let mazeSegs = [], mazeTube = 0;   // pasillos y senderos: polilíneas del trazo (cada una, una lista de puntos) y ancho
let mazeTrailPos = null;           // solo «senderos»: posición de cada nodo en el lienzo
// Registro del recorrido del participante, medido en celdas del laberinto
let mazeTrace = null;

function resetMazeTrace() {
    mazeTrace = { cells: 0, visited: new Set(), crossings: 0, reachedExit: false, prev: null, lastCell: null, inside: null };
}

// ¿Pasar de la celda a a la celda b atraviesa una pared?
function wallBetween(a, b) {
    const dr = b.r - a.r, dc = b.c - a.c;
    const cellAt = (r, c) => mazeCells[r * COLS + c];
    if (Math.abs(dr) + Math.abs(dc) === 1) {
        const w = cellAt(a.r, a.c).w;
        return dr === -1 ? w.t : dr === 1 ? w.b : dc === -1 ? w.l : w.r;
    }
    if (Math.abs(dr) === 1 && Math.abs(dc) === 1) {   // paso diagonal por una esquina
        const v1 = { r: a.r, c: b.c }, v2 = { r: b.r, c: a.c };
        return (wallBetween(a, v1) || wallBetween(v1, b)) && (wallBetween(a, v2) || wallBetween(v2, b));
    }
    return true;
}

// Recorre el segmento a→b en pasos cortos para no saltarse ninguna celda
function traceMazeSegment(a, b, newStroke) {
    const t = mazeTrace, cw = CW / COLS, ch = CH / ROWS;
    if (newStroke) { t.prev = null; t.inside = null; }
    const dist = Math.hypot(b.x - a.x, b.y - a.y);
    const usesTube = TUBE_TYPES.has(mazeType);
    const stepLen = usesTube ? Math.min(cw, ch) / 6 : Math.min(cw, ch) / 3;
    const steps = Math.max(1, Math.ceil(dist / stepLen));
    for (let i = 0; i <= steps; i++) {
        const x = a.x + (b.x - a.x) * i / steps;
        const y = a.y + (b.y - a.y) * i / steps;
        if (usesTube) {           // salidas del camino: pasar de dentro a fuera del trazo
            const inside = insideTube(x, y);
            if (t.inside === true && !inside) t.crossings++;
            t.inside = inside;
        }
        const c = Math.floor(x / cw), r = Math.floor(y / ch);
        if (c < 0 || r < 0 || c >= COLS || r >= ROWS) { t.prev = null; continue; }
        const cell = { r, c };
        if (t.prev && t.prev.r === r && t.prev.c === c) continue;
        if (!t.prev && t.lastCell && t.lastCell.r === r && t.lastCell.c === c) { t.prev = cell; continue; }
        if (mazeType === 'rectangular' && t.prev && wallBetween(t.prev, cell)) t.crossings++;
        t.cells++;
        t.visited.add(r * COLS + c);
        if (r === ROWS - 1 && c === COLS - 1) t.reachedExit = true;
        t.prev = cell;
        t.lastCell = cell;
    }
}

// --- Pasillos anchos y Senderos: geometría y dibujo ---
// mazeSegs es una lista de polilíneas (cada una, una lista de puntos [x,y]): un tramo recto por
// pasillo, o los puntos muestreados de una curva por sendero. Sirve por igual para dibujar y medir.
function buildTubeGeometry() {
    const cw = CW / COLS, ch = CH / ROWS;
    mazeTube = Math.min(cw, ch) * 0.56;
    mazeSegs = [];
    mazeCells.forEach(cell => {
        const cx = (cell.c + 0.5) * cw, cy = (cell.r + 0.5) * ch;
        if (!cell.w.r) mazeSegs.push([[cx, cy], [cx + cw, cy]]);
        if (!cell.w.b) mazeSegs.push([[cx, cy], [cx, cy + ch]]);
    });
}

function buildTrailGeometry() {
    mazeTrailPos = trailPositions(mazeSeed, COLS, ROWS);
    mazeTube = Math.min(CW / COLS, CH / ROWS) * 0.22;
    mazeSegs = [];
    mazeCells.forEach(cell => {
        const i = cell.r * COLS + cell.c;
        if (!cell.w.r) mazeSegs.push(curvePoints(mazeTrailPos[i], mazeTrailPos[i + 1], mazeSeed, i, 'r'));
        if (!cell.w.b) mazeSegs.push(curvePoints(mazeTrailPos[i], mazeTrailPos[i + COLS], mazeSeed, i, 'b'));
    });
}

// ¿El punto está dentro de algún tramo del trazo? (distancia a la polilínea más cercana ≤ mitad del ancho)
function insideTube(x, y) {
    const h = mazeTube / 2;
    for (const poly of mazeSegs) {
        for (let k = 0; k < poly.length - 1; k++) {
            const [ax, ay] = poly[k], [bx, by] = poly[k + 1];
            const dx = bx - ax, dy = by - ay;
            const t = Math.max(0, Math.min(1, ((x - ax) * dx + (y - ay) * dy) / (dx * dx + dy * dy || 1)));
            if (Math.hypot(x - (ax + t * dx), y - (ay + t * dy)) <= h) return true;
        }
    }
    return false;
}

function strokePolylines(ctx) {
    ctx.beginPath();
    mazeSegs.forEach(poly => {
        ctx.moveTo(poly[0][0], poly[0][1]);
        for (let k = 1; k < poly.length; k++) ctx.lineTo(poly[k][0], poly[k][1]);
    });
    ctx.stroke();
}

function drawTubes(ctx, cw, ch) {
    ctx.save();
    ctx.fillStyle = '#334155';
    ctx.fillRect(0, 0, CW, CH);
    ctx.strokeStyle = '#ffffff';
    ctx.lineWidth = mazeTube;
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    strokePolylines(ctx);
    ctx.fillStyle = '#10b981';
    ctx.beginPath();
    ctx.arc(cw * 0.5, ch * 0.5, mazeTube * 0.34, 0, Math.PI * 2);
    ctx.fill();
    ctx.fillStyle = '#f43f5e';
    ctx.beginPath();
    ctx.arc(cw * (COLS - 0.5), ch * (ROWS - 0.5), mazeTube * 0.34, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
}

function drawTrails(ctx) {
    ctx.save();
    ctx.fillStyle = '#ffffff';
    ctx.fillRect(0, 0, CW, CH);
    ctx.fillStyle = '#94a3b8';
    mazeTrailPos.forEach(([x, y], i) => {
        if (i === 0 || i === mazeTrailPos.length - 1) return;
        ctx.beginPath();
        ctx.arc(x, y, 5, 0, Math.PI * 2);
        ctx.fill();
    });
    ctx.strokeStyle = '#0f172a';
    ctx.lineWidth = mazeTube;
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    strokePolylines(ctx);
    const a = mazeTrailPos[0], b = mazeTrailPos[mazeTrailPos.length - 1];
    ctx.fillStyle = '#10b981';
    ctx.beginPath();
    ctx.arc(a[0], a[1], 9, 0, Math.PI * 2);
    ctx.fill();
    ctx.fillStyle = '#f43f5e';
    ctx.beginPath();
    ctx.arc(b[0], b[1], 9, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
}

function drawMaze() {
    const c = $('mazeCanvas');
    const ctx = c.getContext('2d');
    clearCanvas(c);
    const cw = c.width / COLS, ch = c.height / ROWS;
    if (mazeType === 'pasillos') {
        drawTubes(ctx, cw, ch);
        mazeHasPath = false;
        resetMazeTrace();
        return;
    }
    if (mazeType === 'senderos') {
        drawTrails(ctx);
        mazeHasPath = false;
        resetMazeTrace();
        return;
    }

    ctx.save();
    ctx.strokeStyle = '#0f172a';
    ctx.lineWidth = 3.5;
    ctx.lineCap = 'round';
    ctx.beginPath();
    mazeCells.forEach(cell => {
        const x = cell.c * cw, y = cell.r * ch;
        if (cell.w.t) { ctx.moveTo(x, y); ctx.lineTo(x + cw, y); }
        if (cell.w.l) { ctx.moveTo(x, y); ctx.lineTo(x, y + ch); }
        if (cell.c === COLS - 1 && cell.w.r) { ctx.moveTo(x + cw, y); ctx.lineTo(x + cw, y + ch); }
        if (cell.r === ROWS - 1 && cell.w.b) { ctx.moveTo(x, y + ch); ctx.lineTo(x + cw, y + ch); }
    });
    ctx.stroke();
    ctx.fillStyle = '#10b981';
    ctx.fillRect(cw * 0.15, ch * 0.15, cw * 0.7, ch * 0.7);
    ctx.fillStyle = '#f43f5e';
    ctx.fillRect(cw * (COLS - 1) + cw * 0.15, ch * (ROWS - 1) + ch * 0.15, cw * 0.7, ch * 0.7);
    ctx.restore();
    mazeHasPath = false;
    resetMazeTrace();
}

function applyMaze(seed, levelKey, typeKey) {
    typeKey = typeKey || 'rectangular';
    const cfg = mazeCfg(typeKey, levelKey);
    mazeType = typeKey;
    mazeLevel = levelKey;
    mazeSeed = seed;
    COLS = cfg.cols;
    ROWS = cfg.rows;
    const g = generateMaze(seed, levelKey, typeKey);
    mazeCells = g.cells;
    mazeInfo = g.info;
    if (typeKey === 'pasillos') buildTubeGeometry();
    else if (typeKey === 'senderos') buildTrailGeometry();
    else { mazeSegs = []; mazeTube = 0; mazeTrailPos = null; }
    $('maze-info').textContent = `Tipo ${MAZE_TYPES[typeKey].label} · Dificultad ${cfg.label} · Laberinto Nº ${seed} · Recorrido óptimo: ${g.info.length} celdas · Bifurcaciones en ese recorrido: ${g.info.junctions}`;
    drawMaze();
}

function clearMazePath() {
    if (mazeHasPath && !confirm('¿Borrar el trazo del laberinto?')) return;
    drawMaze();
    dirty.maze = true;
}

function initMazeCanvas() {
    const c = $('mazeCanvas');
    c.width = CW; c.height = CH;
    applyMaze(config.tests.maze.seed, config.tests.maze.level, config.tests.maze.type);
    const ctx = c.getContext('2d');
    bindDrawing(c, {
        draw: (a, b) => {
            traceMazeSegment(a, b, a === b);   // a === b solo al apoyar (inicio de trazo)
            ctx.save();
            ctx.lineCap = 'round';
            ctx.lineJoin = 'round';
            ctx.lineWidth = parseFloat($('maze-size').value) * 1.5;
            ctx.strokeStyle = mazeType === 'pasillos' ? '#7c3aed' : mazeType === 'senderos' ? '#ea580c' : '#2563eb';
            ctx.beginPath();
            ctx.moveTo(a.x, a.y);
            ctx.lineTo(b.x, b.y);
            ctx.stroke();
            ctx.restore();
            mazeHasPath = true;
            dirty.maze = true;
        }
    });
}

function mazeTraceMeta(d) {
    const t = d.trace;
    if (!t || !t.cells) return 'Recorrido del participante: sin trazo registrado';
    const diff = t.cells - d.optimal;
    return `Recorrido del participante: ${t.cells} celdas (${diff >= 0 ? '+' : ''}${diff} respecto al óptimo de ${d.optimal}) | Celdas distintas: ${t.distinct} | ${d.exitLabel || 'Cruces de pared'}: ${t.crossings} | Llegó a la salida: ${t.reachedExit ? 'sí' : 'no'}`;
}

function mazeMeta(d) {
    return ` | Tipo ${d.type} | Dificultad ${d.level} | Laberinto Nº ${d.seed} | Recorrido óptimo: ${d.optimal} celdas`;
}

// Pantalla completa del laberinto: modo fijo por CSS (funciona en cualquier navegador)
// + Fullscreen API cuando existe, para ocultar las barras del navegador
let mazeNativeFs = false;

function fitMazeCanvas() {
    const stage = $('maze-stage'), c = $('mazeCanvas');
    if (!stage.classList.contains('fs')) { c.style.width = ''; c.style.height = ''; return; }
    const aw = stage.clientWidth - 8;
    const ah = stage.clientHeight - $('maze-fs-bar').offsetHeight - 8;
    if (aw <= 0 || ah <= 0) return;
    const k = Math.min(aw / CW, ah / CH);
    c.style.width = Math.floor(CW * k) + 'px';
    c.style.height = Math.floor(CH * k) + 'px';
}

function setMazeFullscreen(on) {
    const stage = $('maze-stage');
    stage.classList.toggle('fs', on);
    document.body.style.overflow = on ? 'hidden' : '';
    const fsEl = document.fullscreenElement || document.webkitFullscreenElement;
    if (on) {
        const req = stage.requestFullscreen || stage.webkitRequestFullscreen;
        if (req && !fsEl) {
            try { const r = req.call(stage); if (r && r.catch) r.catch(() => {}); } catch (err) { /* sin API: queda el modo CSS */ }
        }
    } else {
        mazeNativeFs = false;
        if (fsEl) {
            const exit = document.exitFullscreen || document.webkitExitFullscreen;
            try { const r = exit.call(document); if (r && r.catch) r.catch(() => {}); } catch (err) {}
        }
    }
    fitMazeCanvas();
    setTimeout(fitMazeCanvas, 150);
}

function toggleMazeFullscreen() {
    setMazeFullscreen(!$('maze-stage').classList.contains('fs'));
}

function onFullscreenChange() {
    const fsEl = document.fullscreenElement || document.webkitFullscreenElement;
    if (fsEl === $('maze-stage')) mazeNativeFs = true;
    else if (mazeNativeFs) setMazeFullscreen(false);   // salió con gesto o Esc del sistema
    setTimeout(fitMazeCanvas, 150);
}

// ---------- APLICACIÓN DE LA CONFIGURACIÓN ----------
// La configuración la define el evaluador en configuracion.html y llega por la capa de almacenamiento
// (almacenamiento.js). Durante la evaluación no se puede modificar desde esta página.
let config = null;                 // configuración vigente
const limitFired = { house: false, constellation: false, color: false, maze: false };

const activeKeys = () => config.order.filter(k => config.tests[k].enabled);

function setConsigna(k) {
    const p = $('consigna-' + k), t = config.tests[k].consigna;
    p.textContent = '';
    if (!t) { p.style.display = 'none'; return; }
    p.style.display = '';
    p.appendChild(document.createTextNode('Consigna: '));
    p.appendChild(el('em', { textContent: `"${t}"` }));
}

function buildPalette() {
    const box = $('palette-items'), c = config.tests.constellation;
    box.innerHTML = '';
    const addBtn = (text, fn) => {
        const d = el('div', { className: 'palette-item', textContent: text });
        d.setAttribute('role', 'button');
        d.tabIndex = 0;
        d.addEventListener('click', fn);
        box.appendChild(d);
    };
    if (c.mode === 'fija') {
        c.circles.forEach(l => addBtn('+ ' + l, () => addConstellationItem(l, true)));
        c.rects.forEach(l => addBtn('+ ' + l, () => addConstellationItem(l, false)));
    } else {
        addBtn('+ Círculo (persona / vínculo)', () => addFreeItem(true));
        addBtn('+ Rectángulo (obstáculo / refugio)', () => addFreeItem(false));
    }
}

function addFreeItem(isCircle) {
    const t = prompt('Rótulo del elemento:');
    if (t && t.trim()) addConstellationItem(t.trim().slice(0, 24), isCircle);
}

function buildPaintSwatches() {
    const cc = config.tests.color, box = $('paint-swatches'), input = $('paint-color'), lbl = $('lbl-paint-color');
    box.innerHTML = '';
    if (cc.paletteMode === 'fija') {
        lbl.style.display = 'none';
        box.style.display = 'flex';
        COLOR_PALETTE.filter(c => cc.colors.includes(c.id)).forEach(c => {
            const b = el('button', { type: 'button', className: 'swatch', title: c.name });
            b.style.background = c.hex;
            b.addEventListener('click', () => {
                input.value = c.hex;
                box.querySelectorAll('.swatch').forEach(x => x.classList.toggle('active', x === b));
            });
            box.appendChild(b);
        });
        if (box.firstChild) box.firstChild.click();
    } else {
        lbl.style.display = '';
        box.style.display = 'none';
    }
}

function applyConfigToUI() {
    const grid = $('test-grid');
    config.order.forEach(k => {
        const card = $('card-' + k);
        grid.appendChild(card);
        card.style.display = config.tests[k].enabled ? '' : 'none';
        setConsigna(k);
    });

    const h = config.tests.house;
    $('btn-house-eraser').style.display = h.eraser ? '' : 'none';
    $('lbl-house-color').style.display = h.color ? '' : 'none';
    if (!h.color) $('house-color').value = '#0f172a';
    if (!h.eraser && houseTool === 'eraser') { houseTool = 'pen'; setActiveTool($('btn-house-pen')); }

    buildPalette();

    const cc = config.tests.color;
    $('btn-color-fluid').style.display = cc.tools.fluid ? '' : 'none';
    $('btn-color-splash').style.display = cc.tools.splash ? '' : 'none';
    if (!cc.tools[colorTool]) colorTool = cc.tools.fluid ? 'fluid' : 'splash';
    setActiveTool($('btn-color-' + colorTool));
    buildPaintSwatches();

    if (!config.timerVisible) TEST_KEYS.forEach(k => { $('timer-' + k).style.display = 'none'; });
}

// --- Cronómetro y límite de tiempo ---
const mmss = (s) => `${String(Math.floor(s / 60)).padStart(2, '0')}:${String(s % 60).padStart(2, '0')}`;

function tickTimer() {
    if (!activeTest || !config) return;
    const k = activeTest, tc = config.tests[k];
    const total = elapsedMs[k] + (Date.now() - startTime);
    const limitMs = tc.limitMin * 60000;
    const timerEl = $('timer-' + k);
    if (config.timerVisible) {
        const secs = limitMs ? Math.max(0, Math.ceil((limitMs - total) / 1000)) : Math.floor(total / 1000);
        timerEl.textContent = (limitMs ? '⏱ Tiempo restante ' : '⏱ Tiempo ') + mmss(secs);
        timerEl.style.display = '';
    } else {
        timerEl.style.display = 'none';
    }
    if (limitMs && total >= limitMs && !limitFired[k]) {
        limitFired[k] = true;
        onLimitReached(k);
    }
}

function onLimitReached(k) {
    const min = config.tests[k].limitMin;
    if (config.limitAction === 'cerrar') {
        saveTestResult(k);   // guarda lo realizado y vuelve al menú
        alert(`Se cumplió el tiempo límite (${min} min) de «${TESTS[k].short}». Se guardó lo realizado.`);
    } else {
        $('limit-note-' + k).textContent = `⏰ Se cumplió el tiempo límite (${min} min).`;
    }
}

// --- Parámetros usados, para el informe ---
function paramLines(k) {
    const t = config.tests[k], lines = [];
    if (k === 'house') {
        lines.push(`Color: ${t.color ? 'permitido' : 'solo grafito'}`, `Goma: ${t.eraser ? 'permitida' : 'no permitida'}`);
    } else if (k === 'constellation') {
        lines.push(t.mode === 'fija'
            ? `Elementos: paleta fija (círculos: ${t.circles.join(', ') || '—'}; rectángulos: ${t.rects.join(', ') || '—'})`
            : 'Elementos: rótulos escritos por el evaluado');
        lines.push(`Máximo de elementos: ${t.maxItems || 'sin límite'}`);
    } else if (k === 'color') {
        lines.push(t.paletteMode === 'fija'
            ? `Paleta: restringida (${COLOR_PALETTE.filter(c => t.colors.includes(c.id)).map(c => c.name).join(', ')})`
            : 'Paleta: libre');
        lines.push('Herramientas: ' + [t.tools.fluid && 'trazo continuo', t.tools.splash && 'mancha / textura'].filter(Boolean).join(', '));
    }
    lines.push(`Cronómetro visible para el evaluado: ${config.timerVisible ? 'sí' : 'no'}`);
    return lines;
}

function timeMeta(d) {
    let s = `Tiempo de ejecución: ${fmtTime(d.time)}`;
    if (d.limitMin) s += ` (límite ${d.limitMin} min: ${d.limitReached ? 'alcanzado' : 'no alcanzado'})`;
    return s;
}

// ---------- GUARDADO ----------
function saveTestResult(type) {
    leaveTest();
    let img;
    if (type === 'house') img = $('houseCanvas').toDataURL('image/png');
    else if (type === 'constellation') img = renderConstellationImage().toDataURL('image/png');
    else if (type === 'color') img = $('colorCanvas').toDataURL('image/png');
    else img = $('mazeCanvas').toDataURL('image/png');

    testData[type] = {
        img,
        time: Math.round(elapsedMs[type] / 1000),
        limitMin: config.tests[type].limitMin,
        limitReached: limitFired[type],
        consigna: config.tests[type].consigna,
        params: paramLines(type)
    };
    if (type === 'maze') {
        testData.maze.seed = mazeSeed;
        testData.maze.type = MAZE_TYPES[mazeType].label;
        testData.maze.typeKey = mazeType;
        testData.maze.exitLabel = MAZE_TYPES[mazeType].exitLabel;
        testData.maze.level = mazeCfg(mazeType, mazeLevel).label;
        testData.maze.optimal = mazeInfo.length;
        testData.maze.trace = {
            cells: mazeTrace.cells,
            distinct: mazeTrace.visited.size,
            crossings: mazeTrace.crossings,
            reachedExit: mazeTrace.reachedExit
        };
    }
    dirty[type] = false;
    backToMenu(true);
}

// ---------- INFORME ----------
function obsBlock(key) {
    const wrap = el('div');
    const ta = el('textarea', { className: 'obs', value: obs[key], placeholder: 'Escriba aquí observaciones clínicas (conducta durante la prueba, indicadores relevantes, etc.)' });
    const pr = el('div', { className: 'obs-print', textContent: obs[key] });
    ta.addEventListener('input', () => { obs[key] = ta.value; pr.textContent = ta.value; });
    wrap.appendChild(el('label', { className: 'obs-label', textContent: 'Observaciones del profesional:' }));
    wrap.appendChild(ta);
    wrap.appendChild(pr);
    return wrap;
}

function finishAll() {
    const keys = activeKeys();
    const done = keys.filter(k => testData[k]);
    if (!done.length) {
        alert('Guarde al menos una prueba antes de generar el informe.');
        return;
    }
    const missing = keys.filter(k => !testData[k]).map(k => TESTS[k].short);
    const unsaved = keys.filter(k => dirty[k]).map(k => TESTS[k].short);
    let msg = '';
    if (missing.length) msg += 'Pruebas sin realizar: ' + missing.join(', ') + '.\n';
    if (unsaved.length) msg += 'Cambios sin guardar en: ' + unsaved.join(', ') + ' (el informe usará la última versión guardada).\n';
    if (msg && !confirm(msg + '¿Generar el informe igualmente?')) return;

    $('rep-name').textContent = patient.name;
    $('rep-id').textContent = patient.id;
    $('rep-age').textContent = patient.age;
    $('rep-device').textContent = config.device;
    $('rep-date').textContent = new Date().toLocaleDateString('es-UY');

    const container = $('rep-results-container');
    container.innerHTML = '';
    let num = 0;
    keys.forEach(k => {
        const d = testData[k];
        if (!d) return;
        num++;
        const sec = el('div', { className: 'report-section' });
        sec.appendChild(el('h4', { textContent: `${num}. ${TESTS[k].title}` }));
        let meta = timeMeta(d);
        if (k === 'maze') meta += mazeMeta(d);
        sec.appendChild(el('p', { className: 'report-meta', textContent: meta }));
        if (k === 'maze') sec.appendChild(el('p', { className: 'report-meta', textContent: mazeTraceMeta(d) }));
        sec.appendChild(el('p', { className: 'report-meta', textContent: 'Consigna: ' + (d.consigna ? `"${d.consigna}"` : '(sin consigna en pantalla)') }));
        if (d.params.length) sec.appendChild(el('p', { className: 'report-meta', textContent: 'Parámetros: ' + d.params.join(' | ') }));
        sec.appendChild(el('img', { className: 'img-result', src: d.img, alt: TESTS[k].short }));
        sec.appendChild(obsBlock(k));
        container.appendChild(sec);
    });
    const gen = el('div', { className: 'report-section' });
    gen.appendChild(el('h4', { textContent: 'Observaciones generales' }));
    gen.appendChild(obsBlock('general'));
    container.appendChild(gen);

    switchScreen('screen-report');
}

// Bytes de una imagen (data: URL) leídos por el propio navegador, sin decodificar base64 a mano
async function dataUrlToBytes(dataUrl) {
    const res = await fetch(dataUrl);
    return new Uint8Array(await res.arrayBuffer());
}

function downloadBlob(blob, filename) {
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = filename;
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    setTimeout(() => URL.revokeObjectURL(url), 2000);
}

async function exportToWord() {
    if (!window.docx) {
        alert('No se pudo cargar la librería de Word (¿sin conexión?). Use "Imprimir / Guardar PDF" o vuelva a intentarlo con conexión.');
        return;
    }
    const { Document, Packer, Paragraph, TextRun, ImageRun, HeadingLevel } = window.docx;

    const multiline = (text) => {
        const lines = (text || '').split('\n');
        return new Paragraph({
            spacing: { after: 200 },
            children: lines.map((l, i) => new TextRun({ text: l, break: i ? 1 : 0 }))
        });
    };
    const children = [
        new Paragraph({ heading: HeadingLevel.HEADING_1, children: [new TextRun('PsiMatrix - Reporte Proyectivo Digital')] }),
        new Paragraph({ children: [
            new TextRun({ text: 'Evaluado: ', bold: true }), new TextRun(patient.name + '   '),
            new TextRun({ text: 'ID: ', bold: true }), new TextRun(patient.id + '   '),
            new TextRun({ text: 'Edad: ', bold: true }), new TextRun(String(patient.age))
        ] }),
        new Paragraph({ children: [
            new TextRun({ text: 'Dispositivo de administración: ', bold: true }),
            new TextRun(config.device)
        ] }),
        new Paragraph({ spacing: { after: 300 }, children: [
            new TextRun({ text: 'Fecha de Evaluación: ', bold: true }),
            new TextRun(new Date().toLocaleDateString('es-UY'))
        ] })
    ];

    const note = (text) => new Paragraph({ children: [new TextRun({ text, italics: true, color: '64748B' })] });
    const imgBytes = {};
    for (const k of activeKeys()) if (testData[k]) imgBytes[k] = await dataUrlToBytes(testData[k].img);
    let num = 0;
    activeKeys().forEach(k => {
        const d = testData[k];
        if (!d) return;
        num++;
        let meta = timeMeta(d);
        if (k === 'maze') meta += mazeMeta(d);
        children.push(
            new Paragraph({ heading: HeadingLevel.HEADING_2, children: [new TextRun(`${num}. ${TESTS[k].title}`)] }),
            note(meta),
            ...(k === 'maze' ? [note(mazeTraceMeta(d))] : []),
            note('Consigna: ' + (d.consigna ? `"${d.consigna}"` : '(sin consigna en pantalla)')),
            ...(d.params.length ? [note('Parámetros: ' + d.params.join(' | '))] : []),
            new Paragraph({
                spacing: { before: 120, after: 160 },
                children: [new ImageRun({ data: imgBytes[k], transformation: { width: 580, height: 338 } })]
            })
        );
        if (obs[k].trim()) {
            children.push(new Paragraph({ children: [new TextRun({ text: 'Observaciones del profesional:', bold: true })] }), multiline(obs[k]));
        }
    });
    if (obs.general.trim()) {
        children.push(
            new Paragraph({ heading: HeadingLevel.HEADING_2, children: [new TextRun('Observaciones generales')] }),
            multiline(obs.general)
        );
    }

    const doc = new Document({ sections: [{ children }] });
    const blob = await Packer.toBlob(doc);
    const slug = patient.name.normalize('NFD').replace(/[\u0300-\u036f]/g, '').replace(/[^A-Za-z0-9]+/g, '_').replace(/^_+|_+$/g, '') || 'Evaluado';
    downloadBlob(blob, `PsiMatrix_Informe_${slug}.docx`);
}

// ---------- Acceso a la configuración (evaluador) ----------
let leavingForConfig = false;

const hasEvaluationData = () => TEST_KEYS.some(k => testData[k] || dirty[k]);

function configSummary(cfg) {
    const names = cfg.order.filter(k => cfg.tests[k].enabled).map(k => TESTS[k].short);
    const parts = [cfg.device || 'sin dispositivo', names.join(' → ')];
    if (cfg.tests.maze.enabled) {
        const m = cfg.tests.maze;
        parts.push(`Laberinto ${MAZE_TYPES[m.type].label} · ${mazeCfg(m.type, m.level).label} Nº ${m.seed}`);
    }
    if (TEST_KEYS.some(k => cfg.tests[k].enabled && cfg.tests[k].limitMin)) parts.push('con límites de tiempo');
    return parts.join(' · ');
}

async function openEvaluatorConfig() {
    let allowed = false;
    try { allowed = await access.request('configuracion'); } catch (e) { allowed = false; }
    if (!allowed) return;
    if (hasEvaluationData() && !confirm('Hay datos de la evaluación en curso que se perderán si sale a la configuración. ¿Continuar?')) return;
    leavingForConfig = true;      // evita el aviso de "salir de la página" del navegador
    location.href = 'configuracion.html';
}

// ---------- Nuevo evaluado ----------
function resetAll() {
    if (!confirm('Se borrarán todos los datos y producciones del evaluado actual. La configuración se conserva. ¿Iniciar un nuevo evaluado?')) return;
    patient = { name: '', id: '', age: '' };
    TEST_KEYS.forEach(k => {
        testData[k] = null; elapsedMs[k] = 0; dirty[k] = false; obs[k] = '';
        limitFired[k] = false;
        $('limit-note-' + k).textContent = '';
    });
    obs.general = '';
    activeTest = null;
    clearCanvas($('houseCanvas'));
    clearCanvas($('colorCanvas'));
    wipeConstellation();
    applyMaze(mazeSeed, mazeLevel, mazeType);   // mismo laberinto configurado, sin trazo
    dirty.maze = false;
    ['p-name', 'p-id', 'p-age'].forEach(id => { $(id).value = ''; });
    refreshMenu();
    switchScreen('screen-patient');
}

$('constellation-workspace').addEventListener('pointerdown', (e) => {
    if (e.target === e.currentTarget) selectItem(null);
});

document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape' && $('maze-stage').classList.contains('fs')) setMazeFullscreen(false);
    if (e.key === 'Enter' && e.target.matches && e.target.matches('[role="button"]')) {
        e.preventDefault();
        e.target.click();
    }
    if ((e.key === 'Delete' || e.key === 'Backspace') && selectedItem &&
        $('screen-constellation').classList.contains('active') &&
        !/^(INPUT|TEXTAREA)$/.test(e.target.tagName)) {
        e.preventDefault();
        deleteSelected();
    }
});

document.addEventListener('fullscreenchange', onFullscreenChange);
document.addEventListener('webkitfullscreenchange', onFullscreenChange);
window.addEventListener('resize', fitMazeCanvas);
window.addEventListener('orientationchange', () => setTimeout(fitMazeCanvas, 200));

window.addEventListener('beforeunload', (e) => {
    if (leavingForConfig) return;
    if (patient.name || TEST_KEYS.some(k => testData[k] || dirty[k])) {
        e.preventDefault();
        e.returnValue = '';
    }
});

// ---------- Inicialización ----------
async function init() {
    let saved = null;
    try { saved = await store.getActiveConfig(); } catch (e) { saved = null; }
    if (!saved) { switchScreen('screen-noconfig'); return; }
    config = normalizeConfig(saved);
    $('active-cfg-summary').textContent = configSummary(config);
    initHouseCanvas();
    initColorCanvas();
    initMazeCanvas();
    applyConfigToUI();
}
init();
setInterval(tickTimer, 500);

```

