# Bundle del Proyecto para IA: gestor_pruebas_llamados

## Estructura de Archivos
- admin.html
- datos/datos.js
- documentacion/LEEME.md
- documentacion/MANUAL_TECNICO.md
- documentacion/MANUAL_USUARIO.md
- documentacion/PROPUESTA_PSICOLOGOS.md
- documentacion/img/01_inicio.png
- documentacion/img/02_seleccion.png
- documentacion/img/03_informe_dinamicas.png
- documentacion/img/04_tabla_puntaje.png
- documentacion/img/05_hoja_postulante.png
- documentacion/img/06_bases_ia.png
- documentacion/img/06b_bases_ia_paso2.png
- documentacion/img/06c_bases_ia_prompt.png
- documentacion/img/07_admin.png
- documentacion/img/08_modal_area.png
- documentacion/img/09_modal_cargo.png
- documentacion/img/10_modal_dinamica.png
- estilos/administracion.css
- estilos/aplicacion.css
- estilos/informe.css
- index.html
- js/administracion.js
- js/informe.js
- js/interfaz.js
- js/utilidades.js
- netlify.toml
- netlify/functions/pruebas.js
- readme.md

---

## Código Fuente

### Archivo: `admin.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Administrar datos · Evaluaciones psicotécnicas</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,600&family=Instrument+Sans:wght@400;500;600&display=swap">
<link rel="stylesheet" href="estilos/aplicacion.css">
<link rel="stylesheet" href="estilos/administracion.css">
</head>
<body>
<header class="top"><div class="marca"><span class="logo">⚙</span>Administrar datos</div><div class="esp"></div>
 <button class="ghost" id="bAsist">✨ Asistente de bases</button><button class="ghost" id="tema">☾ Tema</button><a class="ghost" href="index.html">← Generador</a></header>
<main class="app">
 <section class="col-izq">
  <div class="seg" role="group" aria-label="Mostrar"><button data-v="act" aria-pressed="true">Activos</button><button data-v="ina" aria-pressed="false">Inactivos</button><button data-v="todos" aria-pressed="false">Todos</button></div>
  <div class="cab-s"><p class="paso"><b>1</b> Áreas</p><button class="link" id="nA">+ Nueva</button></div><div class="lst" id="ar"></div>
  <div class="cab-s" style="margin-top:22px"><p class="paso off" id="p2"><b>2</b> Cargos</p><button class="link" id="nC">+ Nuevo</button></div><div class="lst" id="ca"></div>
 </section>
 <section>
  <p class="paso off" id="p3"><b>3</b> Dinámicas</p>
  <div class="cab" id="cab" hidden><div><h2 id="tC"></h2><p id="sC"></p></div><div style="display:flex;gap:8px"><button class="ghost" id="eC">✏️ Cargo y competencias</button><button class="ghost" id="nD">+ Nueva dinámica</button></div></div>
  <p class="sub" id="avC" hidden></p>
  <div class="lista" id="lista"></div>
 </section>
 <aside class="col-der"><div class="panel"><h2>Cambios pendientes</h2><p class="sub" id="pn"></p>
  <div class="extra"><ul class="sel log" id="log"></ul><div class="fila" style="display:block"><b style="font-size:13px">Catálogo</b><p class="sub" id="res" style="margin:2px 0 0"></p></div></div>
  <button class="btn" id="gu" disabled>Guardar</button><button class="btn sec" style="width:100%;margin-top:8px" id="de" disabled>Descartar cambios</button></div></aside>
</main>
<dialog id="dm"><form id="fm"><div class="mh"><h2 id="mt"></h2><button type="button" class="x" data-c aria-label="Cerrar">✕</button></div><div class="mb" id="mc"></div><div class="mf"><button type="button" class="btn sec" data-c>Cancelar</button><button class="btn" id="ok">Guardar</button></div></form></dialog>
<script src="js/utilidades.js"></script>
<script src="datos/datos.js"></script>
<script src="js/administracion.js"></script>
</body>
</html>

```

### Archivo: `datos/datos.js`
```javascript
// Única capa de acceso a datos: todas las lecturas y escrituras pasan por Netlify.
// La contraseña se pide al abrir la app, se guarda solo en esta pestaña (sessionStorage) y viaja en el encabezado X-Clave.
const Datos = (() => {
    const API = '/.netlify/functions/pruebas';
    let origen = 'bd';

    // ---- Contraseña ----
    let clave = '';
    try { clave = sessionStorage.getItem('pruebas_clave') || ''; } catch (e) { /* sin sessionStorage */ }
    const recordar = c => {
        clave = c || '';
        try { clave ? sessionStorage.setItem('pruebas_clave', clave) : sessionStorage.removeItem('pruebas_clave'); } catch (e) { /* sin sessionStorage */ }
    };

    // Diálogo con el mismo estilo de los demás modales de la app (clases mh / mb / campo / mf).
    function pedirClave(aviso) {
        return new Promise(resolver => {
            const d = document.createElement('dialog');
            d.style.width = 'min(420px, calc(100vw - 24px))';
            d.innerHTML = '<form>'
                + '<div class="mh"><h2>Contraseña</h2></div>'
                + '<div class="mb"><p class="sub" data-a></p>'
                + '<label class="campo">Contraseña<input type="password" autocomplete="current-password" required></label></div>'
                + '<div class="mf"><button type="button" class="btn sec" data-c>Cancelar</button><button class="btn">Entrar</button></div>'
                + '</form>';
            d.querySelector('[data-a]').textContent = aviso || 'Para usar la aplicación hace falta la contraseña.';
            const inp = d.querySelector('input');
            let valor = null;
            d.querySelector('form').addEventListener('submit', e => { e.preventDefault(); valor = inp.value; d.close(); });
            d.querySelector('[data-c]').onclick = () => d.close();
            d.addEventListener('close', () => { d.remove(); resolver(valor); });
            document.body.appendChild(d);
            d.showModal();
            inp.focus();
        });
    }
    let pendiente = null; // evita abrir dos diálogos si hay pedidos simultáneos
    const obtenerClave = aviso => pendiente || (pendiente = pedirClave(aviso).finally(() => { pendiente = null; }));

    // ---- Pedido genérico con reintento de contraseña ----
    async function pedir(url, { metodo = 'GET', cuerpo = null } = {}) {
        let reintento = false;
        // Sin contraseña guardada: se pide antes de llamar, así no aparece un error 401 en la consola.
        if (!clave && !(cuerpo && cuerpo.clave)) {
            const primera = await obtenerClave('');
            if (primera === null) {
                const e = new Error('Se necesita la contraseña.');
                e.status = 401;
                throw e;
            }
            recordar(primera);
        }
        for (;;) {
            const usada = cuerpo && cuerpo.clave && !reintento ? cuerpo.clave : clave;
            const opciones = { method: metodo, cache: 'no-store', headers: { 'X-Clave': usada } };
            if (cuerpo) {
                opciones.headers['Content-Type'] = 'application/json';
                opciones.body = JSON.stringify({ ...cuerpo, clave: usada });
            }
            const r = await fetch(url, opciones);
            const j = await r.json().catch(() => ({}));
            if (r.status === 401) {
                recordar('');
                const nueva = await obtenerClave(reintento || usada ? 'La contraseña no es correcta.' : '');
                if (nueva === null) {
                    const e = new Error('Se necesita la contraseña.');
                    e.status = 401;
                    e.detalle = j;
                    throw e;
                }
                recordar(nueva);
                reintento = true;
                continue;
            }
            if (!r.ok) {
                const e = new Error(j.mensaje || j.error || 'Error ' + r.status);
                e.status = r.status;
                e.detalle = j;
                throw e;
            }
            if (usada) recordar(usada);
            return j;
        }
    }

    async function cargar() {
        const base = await pedir(API);
        if (!base.areas || !base.cargos || !Array.isArray(base.dinamicas)) {
            throw new Error('La respuesta del servidor tiene un formato inválido.');
        }
        origen = 'bd';
        return base;
    }

    async function guardar(base, claveIngresada) {
        const j = await pedir(API, { metodo: 'POST', cuerpo: { clave: claveIngresada, base } });
        origen = 'bd';
        return j;
    }

    // ---- Informes guardados (informes.json + informes/<código>.json) ----
    const listarInformes = () => pedir(API + '?informes=1');
    const leerInforme = codigo => pedir(API + '?informe=' + encodeURIComponent(codigo));
    const guardarInforme = (informe, { clave: claveIngresada = '', reemplazar = false } = {}) => pedir(API, {
        metodo: 'POST',
        cuerpo: { accion: 'guardar_informe', clave: claveIngresada, reemplazar, informe }
    });

    return { cargar, guardar, listarInformes, leerInforme, guardarInforme, get origen() { return origen; } };
})();

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/gestor_pruebas_llamados/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `documentacion/MANUAL_TECNICO.md`
```md
# Manual Técnico — Gestor de Pruebas para Llamados

**Funcionamiento, datos, instalación y mantenimiento**

Versión 1.0 — Octubre 2026 · Versión para imprimir: [Manual_Tecnico.pdf](Manual_Tecnico.pdf)

---

## Índice

- [1. Descripción general](#1-descripción-general)
- [2. Estructura del proyecto](#2-estructura-del-proyecto)
- [3. Instalación y ejecución](#3-instalación-y-ejecución)
  - [3.1 Requisito: servidor HTTP](#31-requisito-servidor-http)
  - [3.2 Ejecución local](#32-ejecución-local)
  - [3.3 Publicación (Netlify u otro hosting estático)](#33-publicación-netlify-u-otro-hosting-estático)
- [4. Modelo de datos](#4-modelo-de-datos)
  - [4.1 Relaciones](#41-relaciones)
  - [4.2 Códigos](#42-códigos)
  - [4.3 areas.json](#43-areasjson)
  - [4.4 cargos.json](#44-cargosjson)
  - [4.5 dinamicas.json](#45-dinamicasjson)
- [5. Funcionamiento del generador (script.js)](#5-funcionamiento-del-generador-scriptjs)
  - [5.1 Cálculo de tiempos](#51-cálculo-de-tiempos)
  - [5.2 Cálculo de puntajes](#52-cálculo-de-puntajes)
  - [5.3 Impresión](#53-impresión)
- [6. Panel ABM (admin.js)](#6-panel-abm-adminjs)
  - [6.1 Procedimiento para publicar cambios](#61-procedimiento-para-publicar-cambios)
  - [6.2 Cambios que requieren editar los JSON a mano](#62-cambios-que-requieren-editar-los-json-a-mano)
- [7. Asistente de IA (bases_ia.js)](#7-asistente-de-ia-bases_iajs)
  - [7.1 Flujo](#71-flujo)
  - [7.2 Incorporar la respuesta de la IA](#72-incorporar-la-respuesta-de-la-ia)
  - [7.3 Limitaciones conocidas](#73-limitaciones-conocidas)
- [8. Solución de problemas técnicos](#8-solución-de-problemas-técnicos)
- [9. Catálogo incluido](#9-catálogo-incluido)

---

## 1. Descripción general

Aplicación web **estática** (HTML5, CSS3 y JavaScript sin frameworks) que genera paquetes de evaluación psicotécnica: informe para el evaluador, matriz de puntaje por postulante y hojas de trabajo imprimibles en A4. Incluye un panel ABM para mantener el catálogo y un asistente que arma un *prompt* para IA a partir de las bases del llamado.

- No hay backend, base de datos ni autenticación.
- Los datos se leen de archivos JSON en la carpeta `datos/`.
- Todo el procesamiento ocurre en el navegador del usuario.
- Única dependencia externa: **Mammoth.js 1.6.0** (lectura de .docx), que se carga desde cdnjs solo al usar el asistente de IA.

Para el uso de las pantallas, ver el **Manual de Usuario** (`docs/MANUAL_USUARIO.md`).

---

## 2. Estructura del proyecto

| Archivo | Función |
|---|---|
| `index.html` | Pantalla principal (Generador de Evaluaciones) y área imprimible. |
| `script.js` | Lógica del generador: carga de datos, filtrado de dinámicas, cálculo de tiempos y puntajes, armado del informe y de las hojas del postulante. Estado global en `baseDatosGlobal`. |
| `style.css` | Estilos de la pantalla principal y reglas de impresión (`@page A4`, saltos de página). |
| `admin.html` | Panel de Administración (ABM) con los modales de área, cargo y dinámica. |
| `admin.js` | Lógica del ABM: altas, edición, bajas, generación de códigos y exportación. |
| `admin.css` | Estilos del panel ABM. |
| `bases_ia.js` | Módulo autónomo del asistente "Directrices desde bases (IA)". Inyecta su propio botón, modal y estilos. |
| `datos/datos.js` | Única capa de acceso a datos (objeto `Datos` con `cargar()` y `exportar()`). |
| `datos/areas.json` | Catálogo de áreas. |
| `datos/cargos.json` | Catálogo de cargos y sus competencias. |
| `datos/dinamicas.json` | Catálogo de dinámicas. |
| `utilidades.js` | Función `esc()` para escapar HTML (actualmente no se incluye en las páginas). |
| `docs/` | Manuales e imágenes. |

---

## 3. Instalación y ejecución

### 3.1 Requisito: servidor HTTP

`datos/datos.js` obtiene los JSON con `fetch()`. Los navegadores bloquean `fetch` sobre `file://`, por lo que la aplicación **debe servirse por HTTP**. Si se abre `index.html` con doble clic aparece la alerta *"Error de conexión. Asegúrate de correr la app en Netlify o un servidor local."*

### 3.2 Ejecución local

Desde la raíz del proyecto, cualquiera de estas opciones:

```
python -m http.server 8080
npx serve .
```

Luego abrir `http://localhost:8080` (generador) o `http://localhost:8080/admin.html` (ABM).

### 3.3 Publicación (Netlify u otro hosting estático)

No requiere *build*: se publica la carpeta raíz tal cual (publish directory = `/`, sin comando de build). Cualquier hosting de archivos estáticos sirve (Netlify, GitHub Pages, IIS, Apache, Nginx).

---

## 4. Modelo de datos

### 4.1 Relaciones

```
areas.json        cargos.json                      dinamicas.json
 "FIN" ─────────▶  "CAR-FIN-CON-01"  ◀───────────  "cargo_id": "CAR-FIN-CON-01"
 (sigla)            area = "Finanzas" (nombre)       id = "DIN-FIN-CON-01"
```

- Un **cargo** pertenece a un área, pero guarda el **nombre** del área (campo `area`), no la sigla. La sigla queda implícita en el código del cargo.
- Una **dinámica** se vincula al cargo por `cargo_id`, que debe coincidir **exactamente** con la clave del cargo.

### 4.2 Códigos

| Elemento | Formato | Generación |
|---|---|---|
| Área | `AAA` | Sigla de 3 letras ingresada por el usuario (se pasa a mayúsculas, no se permiten duplicados). |
| Cargo | `CAR-<área>-<esp>-<NN>` | `<esp>` = hasta 3 letras ingresadas, completadas con `X`. `NN` = mayor número existente con ese prefijo + 1. |
| Dinámica | `DIN-<área>-<esp>-<NN>` | Prefijo tomado del código del cargo; `NN` = mayor existente + 1. |

### 4.3 areas.json

Objeto cuya clave es la sigla:

```json
{
    "FIN": { "nombre": "Finanzas" }
}
```

### 4.4 cargos.json

Objeto cuya clave es el código del cargo:

```json
{
    "CAR-FIN-CON-01": {
        "nombre": "Contador",
        "area": "Finanzas",
        "competencias": [
            { "comp": "Pensamiento Analítico", "desc": "Analiza con lógica y detecta incoherencias." }
        ]
    }
}
```

Las `competencias` definen las columnas de la tabla de puntaje. Los cargos creados desde el ABM reciben dos competencias de ejemplo que deben reemplazarse editando este archivo.

### 4.5 dinamicas.json

Arreglo de objetos:

| Campo | Uso |
|---|---|
| `id` | Código único de la dinámica. |
| `cargo_id` | Código del cargo al que pertenece. |
| `titulo` | Título. |
| `desc` | Descripción breve (informe del evaluador). |
| `tiempo_limite` | Texto libre; los números se usan para el tiempo total. |
| `hoja_postulante` | Consigna para el candidato. Admite HTML; se muestra con `white-space: pre-line` (respeta `\n`). |
| `caso_o_consigna` | Objetivo de la prueba (informe del evaluador). |
| `guia_evaluacion` | Guía de observación; los `\n` se convierten en `<br>`. |
| `respuesta_esperada` | Corrector; los `\n` se convierten en `<br>`. |

Renglones de respuesta: al final de `hoja_postulante` se agregan elementos `<div class='renglon-respuesta'></div>`, uno por renglón. El ABM los separa al editar (campo *Renglones para escribir*, 0 a 30) y los vuelve a componer al guardar.

```json
{
    "id": "DIN-FIN-CON-01",
    "cargo_id": "CAR-FIN-CON-01",
    "titulo": "…",
    "desc": "…",
    "tiempo_limite": "8 Minutos",
    "hoja_postulante": "Consigna…\n\n<div class='renglon-respuesta'></div><div class='renglon-respuesta'></div>",
    "caso_o_consigna": "…",
    "guia_evaluacion": "• Éxito: …\n• Alerta: …",
    "respuesta_esperada": "…"
}
```

---

## 5. Funcionamiento del generador (`script.js`)

1. Al cargar la página, `cargarDatos()` llama a `Datos.cargar()`, que lee los tres JSON en paralelo y los guarda en `baseDatosGlobal = { areas, cargos, dinamicas }`.
2. `inicializarSelectCargos()` llena el selector de cargos; `actualizarDinamicas()` muestra como casillas las dinámicas con `cargo_id` igual al cargo elegido.
3. Cualquier cambio de cargo o de selección llama a `limpiarResultados()`, que borra el informe para evitar mezclar datos.
4. `generarEvaluacion()` arma el informe, la leyenda de competencias, una tabla por postulante y una hoja por dinámica y por postulante.

### 5.1 Cálculo de tiempos

`minutosDe(din)` extrae todos los números de `tiempo_limite` y toma el mayor (p. ej. "Aprox. 5 a 8 minutos" → 8; "2 Minutos de Simulación" → 2). El total es la suma de las dinámicas seleccionadas. Textos sin números cuentan 0.

### 5.2 Cálculo de puntajes

Con *c* competencias y *d* dinámicas, cada celda se puntúa de 0 a 5:

| Total | Máximo |
|---|---|
| Por dinámica (fila) | *c* × 5 |
| Por competencia (columna) | *d* × 5 |
| General | *c* × *d* × 5 |

### 5.3 Impresión

- `@page { size: A4; margin: 15mm }` y `print-color-adjust: exact`.
- `.no-print` (panel de control) se oculta al imprimir.
- Cada `.candidate-sheet` (hoja del postulante) fuerza un salto de página antes.
- El interruptor *Guía y corrector* agrega la clase `ocultar-guia` al área imprimible, que oculta los bloques `.bloque-guia` (guía de observación y corrector) en pantalla y en papel.

---

## 6. Panel ABM (`admin.js`)

- Trabaja sobre la copia en memoria `baseDatosGlobal`. **No escribe en el servidor**: los cambios se pierden al recargar si no se exportan.
- Altas: áreas (`guardarArea`), cargos (`guardarCargo`) y dinámicas (`guardarDinamica`), con códigos autogenerados (sección 4.2).
- Edición y baja: solo para dinámicas (`editarDinamica`, `eliminarDinamica`). Al editar, el código no cambia aunque se cambie el cargo.
- Exportación: `Datos.exportar()` descarga `areas.json`, `cargos.json` y `dinamicas.json` (indentados a 4 espacios).

### 6.1 Procedimiento para publicar cambios

1. El usuario hace los cambios en el ABM y presiona **Descargar JSON Actualizado**.
2. Reemplazar los tres archivos en `datos/`.
3. Validar que los JSON sean correctos (abrir la app localmente o usar un validador JSON).
4. Hacer *commit* y publicar (por ejemplo, Netlify toma los cambios del repositorio).

### 6.2 Cambios que requieren editar los JSON a mano

- Definir o modificar las **competencias** de un cargo.
- Renombrar o eliminar **áreas** o **cargos**.
- Corregir el `cargo_id` de una dinámica.

---

## 7. Asistente de IA (`bases_ia.js`)

Módulo autónomo (función autoejecutable) que en `DOMContentLoaded` inyecta su botón, su modal y sus estilos en `index.html`. Reutiliza `baseDatosGlobal` para conocer áreas, cargos y dinámicas existentes.

### 7.1 Flujo

1. Carga Mammoth.js desde `cdnjs.cloudflare.com` (solo la primera vez) y extrae el texto del .docx con `mammoth.extractRawText`. El archivo no sale del navegador.
2. **Competencias:** marca las casillas cuyo patrón (expresión regular del objeto `COMPETENCIAS`) aparece en el texto (12 competencias predefinidas).
3. **Tareas:** toma hasta 12 líneas que estén dentro de secciones con títulos *Funciones / Tareas / Cometidos / Responsabilidades*.
4. **Cargo y área:** busca "cargo de …" / "área de …" o nombres ya existentes en el catálogo. Compara con el catálogo (ignorando mayúsculas y tildes) y ofrece coincidencias similares.
5. **Anonimización** (`anonimizar`):

| Patrón | Reemplazo |
|---|---|
| Correos electrónicos | `[EMAIL]` |
| Documentos tipo `1.234.567-8` | `[DOC]` |
| Fechas `dd/mm/aaaa` y variantes | `[FECHA]` |
| Montos con `$`, `U$S`, `USD`, `UYU` | `[MONTO]` |
| "llamado / concurso / licitación" + número | `[LLAMADO]` |
| Términos indicados por el usuario | `[X]` |

6. **Códigos** (`generarIds`): si el área o el cargo no existen calcula códigos nuevos sin colisiones, y reserva 4 códigos de dinámica consecutivos.
7. Arma el *prompt*: pide 4 dinámicas en bloques JSON listos para pegar (área y cargo solo si son nuevos) e incluye las dinámicas ya existentes del cargo para evitar repeticiones.

### 7.2 Incorporar la respuesta de la IA

1. Pegar cada bloque en el archivo que indica (`areas.json` y `cargos.json` son objetos; `dinamicas.json` es un arreglo). Cuidar las comas entre elementos.
2. Revisar el contenido (consignas, respuestas esperadas, competencias del cargo).
3. Validar los JSON, probar localmente y publicar.

### 7.3 Limitaciones conocidas

- La detección de cargo puede capturar texto de más (p. ej. "Tesorero del área de Finanzas").
- Solo acepta `.docx`; requiere acceso a cdnjs.

---

## 8. Solución de problemas técnicos

| Síntoma | Causa | Solución |
|---|---|---|
| Alerta "Error de conexión…" / Estado "Error al leer los datos". | Página abierta como `file://`, JSON faltante o con error de sintaxis. | Servir por HTTP; revisar la consola del navegador (F12) y validar los JSON. |
| Selector de cargos vacío. | Error de sintaxis en `cargos.json`. | Validar el JSON (comas, comillas, llaves). |
| "No hay dinámicas para este cargo". | Ninguna dinámica tiene `cargo_id` igual a la clave del cargo. | Corregir `cargo_id` o crear dinámicas. |
| Cambios del ABM perdidos. | Se recargó sin exportar. | Exportar siempre antes de salir. |
| Solo se descarga un JSON. | El navegador bloquea descargas múltiples. | Permitir descargas múltiples para el sitio. |
| "No se pudo cargar el lector de .docx". | Sin acceso a cdnjs. | Verificar conexión o proxy; opcionalmente alojar Mammoth localmente y cambiar `MAMMOTH_URL` en `bases_ia.js`. |
| Impresión sin colores. | *Gráficos de fondo* desactivado. | Activarlo en el diálogo de impresión. |

---

## 9. Catálogo incluido

| Cargo | Código | Competencias | Dinámicas |
|---|---|---|---|
| Carga / Descarga | CAR-OPE-CAR-01 | Actitud hacia la Normativa · Resistencia / Dinamismo · Trabajo en Equipo | 4 |
| Administrativo | CAR-ADM-GEN-01 | Atención al Detalle y Orden · Gestión del Tiempo · Comunicación Efectiva | 4 |
| Encargado | CAR-SUP-ENC-01 | Liderazgo Práctico · Toma de Decisiones · Orientación a Resultados | 4 |
| Jefe | CAR-SUP-JEF-01 | Visión Estratégica · Negociación e Influencia · Gestión de Crisis | 4 |
| Contador | CAR-FIN-CON-01 | Método y Criterio Profesional · Pensamiento Analítico · Integridad y Confidencialidad | 4 |

```

### Archivo: `documentacion/MANUAL_USUARIO.md`
```md
# Manual de Usuario — Gestor de Pruebas para Llamados

**Generador de Evaluaciones Psicotécnicas**

Versión 1.0 — Octubre 2026 · Versión para imprimir: [Manual_de_Usuario.pdf](Manual_de_Usuario.pdf)

---

## Índice

- [1. Introducción](#1-introducción)
  - [1.1 Palabras que usa la herramienta](#11-palabras-que-usa-la-herramienta)
- [2. Pantallas de la herramienta](#2-pantallas-de-la-herramienta)
- [3. Armar una evaluación](#3-armar-una-evaluación)
  - [3.1 Paso a paso](#31-paso-a-paso)
  - [3.2 Qué contiene el formulario](#32-qué-contiene-el-formulario)
  - [3.3 Botón "Guía y corrector"](#33-botón-guía-y-corrector)
  - [3.4 Imprimir o guardar en PDF](#34-imprimir-o-guardar-en-pdf)
- [4. Asistente "Directrices desde bases (IA)"](#4-asistente-directrices-desde-bases-ia)
  - [4.1 Paso a paso](#41-paso-a-paso)
  - [4.2 Qué hacer con la respuesta de la IA](#42-qué-hacer-con-la-respuesta-de-la-ia)
- [5. Panel de Administración](#5-panel-de-administración)
  - [5.1 Crear un área](#51-crear-un-área)
  - [5.2 Crear un cargo](#52-crear-un-cargo)
  - [5.3 Crear una dinámica](#53-crear-una-dinámica)
  - [5.4 Modificar una dinámica](#54-modificar-una-dinámica)
  - [5.5 Borrar una dinámica](#55-borrar-una-dinámica)
  - [5.6 Guardar los cambios](#56-guardar-los-cambios)
- [6. Problemas frecuentes](#6-problemas-frecuentes)

---

## 1. Introducción

El *Gestor de Pruebas para Llamados* es una herramienta para armar, en pocos minutos, todo el material necesario para evaluar a los postulantes de un llamado:

- el **informe para el evaluador**, con la explicación de cada prueba, su tiempo, qué observar y las respuestas esperadas;
- la **tabla de puntaje** de cada postulante;
- las **hojas de trabajo** que se entregan a los postulantes;
- un **panel de administración** para agregar o modificar áreas, cargos y pruebas;
- un **asistente** que, a partir de las bases del llamado, prepara un pedido para que una inteligencia artificial proponga pruebas nuevas.

No requiere usuario ni contraseña: se abre en el navegador con la dirección que le indique el responsable técnico.

### 1.1 Palabras que usa la herramienta

| Término | Significado |
|---|---|
| **Área** | Sector de la organización (p. ej. Operativa, Administración, Supervisión, Finanzas). |
| **Cargo** | Puesto al que se postula (p. ej. Administrativo, Encargado, Contador). |
| **Competencia** | Capacidad que se observa y se puntúa (p. ej. "Gestión del Tiempo"). |
| **Dinámica** | Prueba o ejercicio práctico que se aplica al postulante. |
| **Guía de observación** | Qué indica un buen desempeño (*Éxito*) y qué indica dificultades (*Alerta*). |
| **Corrector** | Respuestas correctas o esperadas de la prueba. |
| **Hoja del postulante** | Consigna que se le entrega al candidato, con renglones para responder. |
| **Dictamen global** | Conclusión final: *Apto / Apto con reservas / No apto*. |

Cada cargo y cada dinámica tienen un **código** que aparece en pantalla y en las hojas impresas (por ejemplo `CAR-ADM-GEN-01` para un cargo o `DIN-ADM-GEN-03` para una dinámica). Sirve para identificar sin dudas qué prueba se aplicó; la herramienta los crea sola.

---

## 2. Pantallas de la herramienta

| Pantalla | Para qué se usa |
|---|---|
| **Generador de Evaluaciones** | Uso diario: armar e imprimir las evaluaciones. Es la pantalla que se abre al ingresar. |
| **Panel de Administración** | Agregar o modificar áreas, cargos y dinámicas. Se entra con el botón **⚙️ Administrar Datos**. |

---

## 3. Armar una evaluación

![Pantalla principal del generador](img/01_inicio.png)

### 3.1 Paso a paso

1. En **Puesto / Cargo**, elija el cargo del llamado.
2. En **Postulantes**, indique cuántos postulantes va a evaluar (de 1 a 10). Se preparará una tabla de puntaje y un juego de hojas para cada uno.
3. En **Dinámicas a aplicar**, marque las pruebas que va a usar. Aparecen solo las del cargo elegido y puede marcar varias. Cada tarjeta muestra su código y su tiempo. Si marca más de una, arriba a la derecha verá el **tiempo total estimado**.
4. Presione **Generar Formulario**.
5. Revise el resultado que aparece debajo y presione **Imprimir Formulario**.

![Selección de cargo, postulantes y dinámicas](img/02_seleccion.png)

> **Atención:** si después de generar cambia el cargo o marca/desmarca una dinámica, el formulario de abajo se borra y aparece el aviso *"Selección modificada. Presione 'Generar Formulario' para actualizar los datos."* Es para evitar imprimir datos mezclados: vuelva a presionar **Generar Formulario**.

### 3.2 Qué contiene el formulario

Arriba se ve el título **INFORME DE EVALUACIÓN PSICOTÉCNICA**, el cargo y espacios para completar *Fecha* y *Evaluador(a)*.

**1. Dinámicas de evaluación seleccionadas.** Para cada prueba: título, código, descripción, tiempo límite, objetivo, guía de observación y corrector.

![Detalle de dinámicas para el evaluador](img/03_informe_dinamicas.png)

**2. Evaluación y puntaje por postulante.** Primero se listan las competencias a observar. Después, para cada postulante, hay una tabla:

- cada **fila** es una dinámica (D1, D2, …);
- cada **columna** es una competencia, que se puntúa **de 0 a 5**;
- **Total dinámica** es la suma de la fila;
- la fila **TOTAL** suma cada competencia en todas las pruebas, y en la esquina va el total general.

Debajo hay un recuadro para **observaciones y notas de conducta** del postulante.

![Tabla de puntaje por postulante](img/04_tabla_puntaje.png)

*Ejemplo:* con 3 competencias y 2 dinámicas, cada dinámica vale hasta 15 puntos, cada competencia hasta 10 y el total hasta 30.

**3. Conclusión y dictamen general.** Espacio para marcar *Apto / Apto con reservas / No apto* y escribir comentarios finales.

**Hojas de trabajo del postulante.** Al final están las hojas para entregar a los candidatos: una por cada prueba y por cada postulante. Tienen lugar para nombre y fecha, el código de la prueba, el tiempo límite, la consigna y renglones para responder.

![Hoja de trabajo del postulante](img/05_hoja_postulante.png)

### 3.3 Botón "Guía y corrector"

El interruptor **Guía y corrector** (encendido de fábrica) muestra u oculta la guía de observación y las respuestas esperadas. Apáguelo si necesita imprimir el informe **sin las respuestas**, por ejemplo cuando el material puede quedar a la vista de otras personas.

### 3.4 Imprimir o guardar en PDF

- **Imprimir Formulario** abre la ventana de impresión. Los botones y opciones de arriba no se imprimen; solo el informe y las hojas.
- Está preparado para **hoja A4**. Cada hoja del postulante sale en una página aparte.
- Para guardar en PDF, elija *Guardar como PDF* en lugar de una impresora.
- Para conservar los colores de los recuadros, active la opción *Gráficos de fondo* en la ventana de impresión.

---

## 4. Asistente "Directrices desde bases (IA)"

El botón violeta **Directrices desde bases (IA)** ayuda a crear pruebas nuevas para un llamado a partir de sus **bases** en Word (.docx).

> **Privacidad:** el archivo se lee en su propia computadora y **no se envía a ningún lado**. Lo único que usted copiará y llevará a la inteligencia artificial es el texto final, que puede revisar antes.

![Asistente: carga del archivo](img/06_bases_ia.png)

### 4.1 Paso a paso

1. Presione **Directrices desde bases (IA)** y elija el archivo **.docx** con las bases.
2. La herramienta completa sola:
   - **Cargo del llamado** y **Área**. Puede corregirlos. Debajo de cada uno aparece un aviso: en **verde** si ya existe, en **rojo** si hay que crearlo, o una sugerencia si encontró uno **parecido** (con los botones *Usar existente* o *Crear nuevo*).
   - **Competencias detectadas**: casillas ya marcadas según lo que dicen las bases. Puede marcar o desmarcar.
   - **Tareas típicas**: tomadas de las funciones del cargo. Borre o modifique lo que no quiera compartir.
3. En **Términos a enmascarar** escriba, separados por coma, los nombres que no deben salir (organismo, personas, sectores). Se reemplazarán por `[X]`.
4. Presione **Generar prompt**.
5. Lea el texto, presione **Copiar** y péguelo en la inteligencia artificial que use (ChatGPT, Copilot, Gemini, etc.).

![Asistente: datos detectados](img/06b_bases_ia_paso2.png)

> **Consejo:** revise siempre el *Cargo*. A veces toma palabras de más (en el ejemplo puso "Tesorero del área de Finanzas" cuando lo correcto era "Tesorero").

Además de los términos que usted indique, la herramienta oculta sola los **correos electrónicos, números de documento, fechas, montos de dinero y números de llamado**.

![Asistente: prompt generado](img/06c_bases_ia_prompt.png)

### 4.2 Qué hacer con la respuesta de la IA

La IA devolverá **4 pruebas nuevas** (y, si hacía falta, el área y el cargo) en un formato especial listo para cargar. Entregue esa respuesta al **responsable técnico** de la herramienta para que la incorpore. Revise siempre el contenido propuesto antes de usarlo en una evaluación real.

---

## 5. Panel de Administración

Se entra con **⚙️ Administrar Datos** desde la pantalla principal y se sale con **⬅️ Volver al Generador**.

![Panel de administración](img/07_admin.png)

Arriba, **Estado** indica *Datos cargados* (verde) si todo está bien.

> ⚠️ **Muy importante:** lo que agregue o cambie en este panel **no queda guardado automáticamente**. Si recarga o cierra la página antes de guardar, **se pierde**. Al terminar, siga los pasos de la sección 5.6.

### 5.1 Crear un área

1. Presione **+ Crear Nueva Área**.
2. Escriba el **nombre** (p. ej. *Tecnología y Sistemas*).
3. Escriba una **sigla de 3 letras** (p. ej. *TEC*). No puede repetirse.
4. Presione **Guardar Área**.

![Crear área](img/08_modal_area.png)

### 5.2 Crear un cargo

1. Presione **+ Crear Nuevo Cargo**.
2. Escriba el **nombre del cargo** (p. ej. *Tesorero*).
3. Elija el **área**.
4. Escriba una **sigla de especialidad** de hasta 3 letras (p. ej. *TES*).
5. El código se arma solo. Presione **Crear Cargo**.

![Crear cargo](img/09_modal_cargo.png)

> El cargo nuevo se crea con dos competencias de ejemplo. Para definir sus competencias reales, pídaselo al responsable técnico.

### 5.3 Crear una dinámica

1. En **Seleccionar Cargo a Administrar**, elija el cargo.
2. Presione **+ Nueva Dinámica**.
3. Complete el formulario:

| Campo | Qué escribir |
|---|---|
| Cargo Objetivo | Cargo al que pertenece la prueba (ya viene elegido). |
| Código | Se completa solo. |
| Título de la Dinámica | Nombre corto de la prueba. |
| Descripción Breve | En qué consiste, en una línea. |
| Tiempo Límite | P. ej. *3 Minutos Exactos* o *Aprox. 5 a 8 minutos*. |
| Hoja de Trabajo / Consigna para el Postulante | El texto que leerá el candidato. Se respetan los saltos de línea. |
| Renglones para escribir | Cantidad de líneas en blanco para que responda (0 a 30). |
| Objetivo / Consigna para el Evaluador | Qué mide la prueba. |
| Guía de Observación | Sugerencia: una línea "• Éxito: …" y otra "• Alerta: …". |
| Corrector | La respuesta correcta o esperada (opcional). |

4. Presione **Guardar Dinámica**.

![Formulario de dinámica](img/10_modal_dinamica.png)

### 5.4 Modificar una dinámica

Presione **✏️ Editar** en la prueba, haga los cambios y presione **Guardar Dinámica**. El código no cambia. Si la prueba en realidad corresponde a otro cargo, es mejor crearla de nuevo en ese cargo y borrar la anterior.

### 5.5 Borrar una dinámica

Presione **🗑️ Borrar** y confirme.

### 5.6 Guardar los cambios

1. Presione **📥 Descargar JSON Actualizado**. Se descargan **tres archivos** (`areas.json`, `cargos.json` y `dinamicas.json`). Si el navegador pregunta si permite *descargar varios archivos*, acepte.
2. Envíe esos tres archivos al **responsable técnico** para que los publique. Recién entonces los cambios quedarán disponibles para todos.

> Desde el panel no se pueden modificar ni borrar áreas o cargos ya existentes; para eso, consulte al responsable técnico.

---

## 6. Problemas frecuentes

| Qué pasa | Qué hacer |
|---|---|
| Aparece "Error de conexión…" al entrar, o el Estado dice "Error al leer los datos". | Verifique que entró con la dirección correcta. Si sigue, avise al responsable técnico. |
| La lista de cargos está vacía. | Avise al responsable técnico (puede haber un error en los datos). |
| Dice "No hay dinámicas para este cargo". | Ese cargo todavía no tiene pruebas: créelas en el Panel de Administración. |
| Dice "Por favor seleccione al menos una dinámica." | Marque una o más pruebas antes de presionar Generar. |
| El formulario de abajo se borró. | Cambió el cargo o las pruebas marcadas: presione otra vez **Generar Formulario**. |
| Perdí los cambios del Panel de Administración. | Se recargó la página sin guardar. Repita los cambios y use **Descargar JSON Actualizado** antes de salir. |
| Solo se descargó un archivo. | El navegador bloqueó las descargas múltiples: permítalas y vuelva a presionar el botón. |
| El asistente dice "No se pudo cargar el lector de .docx". | Revise su conexión a Internet. El archivo debe ser `.docx` (no `.doc` ni `.pdf`). |
| La impresión sale sin colores. | Active *Gráficos de fondo* en la ventana de impresión. |

```

### Archivo: `documentacion/PROPUESTA_PSICOLOGOS.md`
```md
# Propuesta para profesionales de la Psicología

**Gestor de Pruebas para Llamados**

Versión para imprimir: [Propuesta_para_Psicologos.pdf](Propuesta_para_Psicologos.pdf)

---

## 1. La propuesta en pocas palabras

En un llamado, buena parte del tiempo del psicólogo no se dedica a evaluar. Se va en **preparar el material**: elegir ejercicios adecuados al cargo, redactar consignas, armar planillas de puntaje y fotocopiar hojas para cada postulante. Además, cuando hay muchos candidatos, varios evaluadores o llamados sucesivos, cuesta mantener **los mismos criterios** para todos.

El *Gestor de Pruebas para Llamados* se encarga de esa preparación. En pocos minutos genera, listo para imprimir:

- un **informe para el evaluador**, con cada dinámica explicada, su tiempo, qué observar y la respuesta esperada;
- una **tabla de puntaje por competencias** para cada postulante;
- las **hojas de trabajo** de cada candidato, con consigna y espacio para responder.

> **La idea central:** la herramienta prepara y ordena el material. **La observación, la interpretación y el dictamen siguen siendo del profesional.**

> **Confidencialidad garantizada por diseño:** cuando se usan las bases de un llamado para crear dinámicas nuevas, el texto que se lleva a la inteligencia artificial **no deja rastro de la empresa que redactó las bases, ni de sus clientes, ni de personas concretas.** Se explica en detalle en la sección 6.

![Armado de una evaluación: cargo, cantidad de postulantes y dinámicas](img/02_seleccion.png)

---

## 2. Desafíos habituales en la evaluación de un llamado

| Desafío | Qué suele pasar |
|---|---|
| **Tiempo de preparación** | Cada llamado obliga a buscar o redactar ejercicios, consignas y planillas casi desde cero. |
| **Consistencia entre postulantes** | Si las consignas, los tiempos o los criterios varían, la comparación entre candidatos pierde solidez. |
| **Varios evaluadores** | Sin una guía común, cada evaluador puede valorar distinto la misma conducta. |
| **Registro y fundamentación** | El dictamen tiene que poder justificarse con evidencia ordenada y trazable. |
| **Adecuación al perfil** | Las pruebas tienen que relacionarse con las tareas y competencias reales del cargo que se describen en las bases. |
| **Confidencialidad** | Las bases y los datos del proceso no deberían circular sin cuidado. |

---

## 3. Qué aporta la herramienta

### 3.1 Evaluaciones armadas en minutos

El profesional elige el **cargo**, indica cuántos **postulantes** va a evaluar (hasta 10 por tanda) y marca las **dinámicas** que quiere aplicar. La herramienta muestra solo las dinámicas del cargo elegido y calcula el **tiempo total estimado** de la sesión, lo que ayuda a planificar la jornada.

Con un clic se genera todo el material, ordenado e identificado con un código único por prueba. Así queda claro qué se aplicó a cada candidato.

### 3.2 Dinámicas situacionales ligadas al cargo

Cada cargo tiene sus propias dinámicas: ejercicios prácticos que reproducen situaciones del puesto. La versión actual incluye **20 dinámicas** para **5 cargos** (Carga / Descarga, Administrativo, Encargado, Jefe y Contador), y se pueden agregar las que el profesional necesite.

Cada dinámica trae:

- **descripción y objetivo** (qué se quiere observar);
- **tiempo límite**;
- **consigna para el postulante**;
- **guía de observación**, con indicadores de *Éxito* y de *Alerta*;
- **respuesta esperada**, incluyendo las "trampas" previstas.

*Ejemplo (cargo Carga / Descarga):* en la "Prueba de Atención Sostenida y Verificación de Códigos", el postulante tiene 2 minutos para marcar en un listado los códigos que cumplen una regla. La guía indica que el **éxito** es una alta tasa de aciertos con pocos falsos positivos. La **alerta** es marcar de forma desordenada, omitir más de la mitad o bloquearse ante la presión. El corrector lista los 6 códigos correctos y explica las trampas.

![Informe para el evaluador: guía de observación y respuesta esperada](img/03_informe_dinamicas.png)

### 3.3 Puntaje por competencias, comparable entre candidatos

Cada cargo tiene definidas sus **competencias**. Por ejemplo, para *Jefe*: Visión Estratégica, Negociación e Influencia y Gestión de Crisis. Para cada postulante se genera una matriz:

- cada **fila** es una dinámica y cada **columna** una competencia, puntuada **de 0 a 5**;
- se calculan los totales por dinámica, por competencia y el total general;
- hay un espacio para **observaciones y notas de conducta**;
- el informe cierra con el **dictamen global**: *Apto / Apto con reservas / No apto*.

Así, todos los candidatos se evalúan sobre las mismas competencias, con la misma escala y en el mismo formato. Eso facilita compararlos y fundamentar la decisión.

![Tabla de puntaje por competencias de un postulante](img/04_tabla_puntaje.png)

### 3.4 Material del postulante listo para entregar

Cada postulante recibe su hoja por dinámica, con nombre, fecha, código de la prueba, tiempo límite, consigna y renglones para responder. Al imprimir, cada hoja sale en una página aparte (formato A4) y también se puede guardar en PDF.

El interruptor **Guía y corrector** permite imprimir el informe **sin las respuestas esperadas** cuando el material puede quedar a la vista de otras personas.

![Hoja de trabajo del postulante](img/05_hoja_postulante.png)

---

## 4. El criterio profesional, en el centro

La herramienta **no reemplaza al psicólogo ni emite diagnósticos**. Su función es estructurar el proceso:

- **El profesional decide** qué cargo, qué dinámicas y qué competencias se evalúan.
- **El profesional observa y puntúa.** La guía de observación orienta, pero la valoración es suya.
- **El profesional dictamina.** El dictamen global se completa a mano, con sus fundamentos.
- **El profesional adapta.** Puede crear, modificar o eliminar dinámicas y definir cargos nuevos desde el Panel de Administración.

> **Aclaración:** las dinámicas son **ejercicios situacionales** pensados para observar competencias en relación con el cargo. **No son tests psicométricos estandarizados.** Pueden **complementar** los instrumentos que el profesional ya utiliza y que tienen sus propias normas de aplicación e interpretación.

---

## 5. Adaptar la evaluación a cada llamado

### 5.1 A partir de las bases del llamado

El asistente **Directrices desde bases (IA)** lee el archivo Word de las bases. Detecta el cargo, el área, las competencias y las tareas del puesto, y prepara un texto (*prompt*) para pedirle a una inteligencia artificial que proponga **dinámicas nuevas alineadas a ese perfil**.

Las propuestas de la IA son un **borrador**: el profesional las revisa, ajusta o descarta antes de incorporarlas.

![Asistente: datos detectados en las bases, editables antes de generar el prompt](img/06b_bases_ia_paso2.png)

### 5.2 Banco de dinámicas propio

Desde el **Panel de Administración**, el equipo puede construir y mantener su propio banco de dinámicas por área y cargo, con su guía de observación y su corrector. La experiencia de cada llamado se acumula y queda disponible para los siguientes.

![Panel de Administración: dinámicas de un cargo](img/07_admin.png)

<div style="break-before:page"></div>

## 6. Confidencialidad: las bases no dejan rastro

Las bases de un llamado suelen contener información sensible: el nombre de la organización o de la consultora que las redactó, sus clientes, responsables, números de llamado, fechas, remuneraciones y datos de contacto. El asistente está diseñado para que **nada de eso llegue a la inteligencia artificial**. Lo que se obtiene es un pedido **genérico**, que describe un perfil de puesto pero no permite saber de qué empresa, cliente o llamado proviene.

> **En resumen:** el archivo de bases **nunca sale de la computadora**, el texto completo **nunca se incluye** en el pedido a la IA, los datos identificatorios **se reemplazan automáticamente** y el profesional **revisa todo** antes de copiarlo. Las dinámicas que se crean a partir de ese pedido **no contienen referencias** a la empresa que diseñó las bases ni a sus clientes.

### 6.1 Cinco capas de protección

1. **El archivo no se sube a ningún lado.** El documento Word se lee dentro del propio navegador del profesional. La herramienta no tiene servidor que lo reciba, no lo guarda y, al cerrar la ventana, no queda copia.
2. **Del documento solo se toma lo indispensable.** El pedido a la IA **no contiene el texto de las bases**. Contiene únicamente cuatro datos del perfil: el **nombre del cargo**, el **área**, las **competencias** elegidas de una lista estándar y **hasta 12 tareas típicas** del puesto. Todo lo demás (antecedentes de la organización, condiciones, cronograma, requisitos administrativos, firmas) queda afuera.
3. **Los datos identificatorios se borran solos.** En los datos que sí se usan, la herramienta reemplaza automáticamente:
    - correos electrónicos → `[EMAIL]`;
    - números de documento de identidad → `[DOC]`;
    - fechas → `[FECHA]`;
    - montos en pesos o dólares → `[MONTO]`;
    - números de llamado, concurso o licitación → `[LLAMADO]`.
4. **Nombres de empresa, clientes y sectores, ocultos.** En el campo **Términos a enmascarar** se escriben, separados por coma, los nombres que no deben aparecer: la organización, la consultora, los clientes, marcas, sectores o personas. Cada uno se reemplaza por `[X]` en todo el texto, sin importar mayúsculas o minúsculas.
5. **El profesional tiene la última palabra.** Antes de generar el pedido, el cargo, el área y las tareas se muestran en campos **editables**, para corregir o borrar lo que quiera. El pedido final también se muestra completo y **no se envía automáticamente**: solo llega a la IA si el profesional lo copia y lo pega. Además, el propio pedido le indica a la IA que diseñe las dinámicas **"sin nombres reales ni datos de organismos"**.

### 6.2 Qué llega a la IA y qué no

| Llega a la IA (ya anonimizado) | No llega nunca |
|---|---|
| Nombre genérico del cargo (p. ej. "Tesorero") | El archivo de bases y su texto completo |
| Área (p. ej. "Finanzas") | Nombre de la empresa o consultora que diseñó las bases |
| Competencias elegidas de una lista estándar | Nombres de clientes, marcas o personas |
| Hasta 12 tareas típicas del puesto, redactadas en forma genérica | Correos, documentos, fechas, montos y números de llamado |
| Títulos de las dinámicas que ya existen para ese cargo, para no repetirlas | Datos de los postulantes (la herramienta no los registra) |

*Ejemplo:* si las bases dicen *"Consultora Andina S.A., por cuenta de su cliente Banco Delta, convoca al Llamado N° 12/2026 para Tesorero; consultas a rrhh@andina.com hasta el 15/11/2026"*, y el profesional escribe en Términos a enmascarar *"Consultora Andina, Banco Delta"*, al pedido a la IA solo llega el cargo **"Tesorero"**, el área, las competencias y las tareas genéricas. Si algún dato hubiera quedado dentro de una tarea, se vería como *"[X] … [LLAMADO] … [EMAIL] … [FECHA]"*.

### 6.3 Buena práctica recomendada

Las fechas, montos, correos, documentos y números de llamado se ocultan solos. **Los nombres propios dependen de que se indiquen en "Términos a enmascarar"**, porque cada organización y cada cliente se llaman distinto. Por eso se recomienda:

- escribir siempre en ese campo el nombre de la organización, de la consultora y de los clientes, incluidas siglas y variantes;
- dar una lectura rápida al pedido final antes de copiarlo (lleva menos de un minuto).

Con esos dos hábitos, el pedido que se lleva a la IA describe un puesto de trabajo y **nada más**.

![Pedido final para la IA, visible y revisable antes de copiarlo](img/06c_bases_ia_prompt.png)

<div style="break-before:page"></div>

## 7. Beneficios resumidos

| Para el profesional | Para la organización | Para el postulante |
|---|---|---|
| Menos tiempo de preparación y más tiempo para observar y analizar. | Procesos de selección más ordenados y documentados. | Mismas consignas, mismos tiempos y mismos criterios para todos. |
| Guías de observación y correctores a mano durante la aplicación. | Criterios homogéneos aunque intervengan varios evaluadores. | Evaluación vinculada a tareas reales del cargo. |
| Puntaje por competencias que ayuda a fundamentar el dictamen. | Banco de pruebas reutilizable y adaptable a cada llamado. | Material claro e impreso, con instrucciones precisas. |
| Puede crear dinámicas nuevas con IA sin exponer las bases ni a sus clientes. | La información de la empresa y de sus clientes no sale de la organización. | Confidencialidad: no se registran sus datos en el sistema. |
| Control total: puede crear y ajustar las dinámicas. | No requiere instalación ni licencias: funciona en el navegador. | Trato equitativo y documentado. |

## 8. Cómo empezar

1. **Recorrido de 15 minutos:** abrir la herramienta, elegir un cargo y generar una evaluación de ejemplo. El **Manual de Usuario** explica cada pantalla.
2. **Revisión del banco:** analizar las dinámicas, guías y competencias del cargo de interés y ajustar lo necesario según el criterio profesional.
3. **Prueba piloto:** usarla en el próximo llamado junto con la metodología habitual y comparar tiempos de preparación, claridad del registro y facilidad para fundamentar el dictamen.

```

### Archivo: `documentacion/img/01_inicio.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/02_seleccion.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/03_informe_dinamicas.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/04_tabla_puntaje.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/05_hoja_postulante.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/06_bases_ia.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/06b_bases_ia_paso2.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/06c_bases_ia_prompt.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/07_admin.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/08_modal_area.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/09_modal_cargo.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `documentacion/img/10_modal_dinamica.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `estilos/administracion.css`
```css
/* Panel de administración (ABM): lista de áreas, cargos y dinámicas, formularios y asistente. Usa las variables de aplicacion.css. */
.seg{display:flex;border:1px solid var(--linea);border-radius:10px;padding:3px;gap:2px;background:var(--panel);margin-bottom:18px}
.seg button{flex:1;border:0;background:none;border-radius:8px;padding:6px;font-size:13px;color:var(--suave)}
.seg button[aria-pressed=true]{background:var(--acento);color:var(--acento-t)}
.cab-s{display:flex;justify-content:space-between;align-items:center;margin:0 0 10px}.cab-s .paso{margin:0}
.lst{display:grid;gap:8px}
.it{display:flex;align-items:center;justify-content:space-between;gap:8px;background:var(--panel);border:1px solid var(--linea);border-radius:var(--r);padding:10px 12px;cursor:pointer}
.it:hover{border-color:var(--acento)}.it.on{border-color:var(--acento);box-shadow:0 0 0 2px var(--acento-s)}
.it strong{font-weight:600}.it small{display:block;color:var(--suave)}
.ina{opacity:.62;background-image:repeating-linear-gradient(135deg,transparent 0 8px,var(--acento-s) 8px 9px)}
.ac{display:flex;gap:2px;flex:none}.ac button{border:0;background:none;border-radius:8px;padding:5px 7px;font-size:14px}.ac button:hover{background:var(--acento-s)}
.bad{font-size:11px;font-weight:600;border-radius:6px;padding:1px 7px;background:var(--aviso-s);color:var(--aviso)}
.din.adm{cursor:default}.din.adm:hover{transform:none}
.cp{display:block;margin:0 0 12px;font-size:13px;font-weight:600;color:var(--suave)}
.cp input,.cp select,.cp textarea{display:block;width:100%;margin-top:4px;padding:9px 11px;border:1px solid var(--linea);border-radius:10px;background:var(--bg);color:var(--tinta);font:inherit;font-weight:400}
.cp textarea{min-height:84px;resize:vertical}.dos{display:grid;grid-template-columns:2fr 1fr;gap:10px}
.fc{display:grid;grid-template-columns:84px 1fr 1.4fr 34px;gap:6px;margin-bottom:6px}
.fc input{padding:7px 9px;border:1px solid var(--linea);border-radius:8px;background:var(--bg);color:var(--tinta);font:inherit;font-size:13px;min-width:0}
.fc .x{width:34px;height:34px}
.err{color:var(--aviso);font-size:13px;min-height:1.2em;margin:4px 0 0}
.toast{position:fixed;left:50%;bottom:96px;transform:translateX(-50%);background:var(--tinta);color:var(--bg);padding:10px 16px;border-radius:12px;font-size:14px;z-index:20;box-shadow:var(--sombra)}
.modo{display:block;border:1px solid var(--linea);border-radius:var(--r);padding:11px 14px;margin-bottom:8px;cursor:pointer}
.modo:has(input:checked){border-color:var(--acento);box-shadow:0 0 0 2px var(--acento-s)}.modo input{margin-right:8px}.modo small{display:block;color:var(--suave);margin-left:24px}
.pre{width:100%;min-height:230px;font:12px/1.5 ui-monospace,Menlo,monospace;padding:12px;border:1px solid var(--linea);border-radius:10px;background:var(--bg);color:var(--tinta)}
.log li{display:block;padding:4px 0;border-top:1px solid var(--linea);font-size:13px}
@media(max-width:560px){.fc{grid-template-columns:1fr 34px}.dos{grid-template-columns:1fr}}
.din.adm .meta{flex-wrap:wrap}
.tag.cmp{font:600 11px ui-monospace,Menlo,monospace}
#avC{color:var(--aviso);font-size:13px;margin:0 0 12px;line-height:1.5}
.chk{display:flex;gap:8px;align-items:flex-start;font-size:13px;padding:7px 10px;border:1px solid var(--linea);border-radius:10px;margin-bottom:6px;cursor:pointer}
.chk input{margin-top:2px}.chk .cod{font:600 11px ui-monospace,Menlo,monospace;color:var(--acento);flex:none;margin-top:2px}
.fc input[readonly]{opacity:.7;cursor:not-allowed}

```

### Archivo: `estilos/aplicacion.css`
```css
/* Pantalla de armado: área, cargo, dinámicas y panel de la evaluación. */
:root{--bg:#f6f4ef;--panel:#fffdf9;--tinta:#1d2a2f;--suave:#5c6b70;--linea:#e3ded3;--acento:#1f6b63;--acento-t:#fff;--acento-s:#e2efec;--aviso:#a8481a;--aviso-s:#f8e9df;--sombra:0 1px 2px rgba(29,42,47,.06),0 8px 24px rgba(29,42,47,.06);--r:14px}
@media(prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#11181a;--panel:#182225;--tinta:#e8ece9;--suave:#9aa9ad;--linea:#2a3639;--acento:#4fb5a8;--acento-t:#0b1a18;--acento-s:#1b3330;--aviso:#e59a6d;--aviso-s:#33241b;--sombra:0 1px 2px rgba(0,0,0,.4),0 8px 24px rgba(0,0,0,.3)}}
:root[data-theme="dark"]{--bg:#11181a;--panel:#182225;--tinta:#e8ece9;--suave:#9aa9ad;--linea:#2a3639;--acento:#4fb5a8;--acento-t:#0b1a18;--acento-s:#1b3330;--aviso:#e59a6d;--aviso-s:#33241b;--sombra:0 1px 2px rgba(0,0,0,.4),0 8px 24px rgba(0,0,0,.3)}
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
*,*::before,*::after{box-sizing:inherit}
body{margin:0;background:var(--bg);color:var(--tinta);font:15px/1.5 "Instrument Sans",system-ui,-apple-system,"Segoe UI",sans-serif}
h1,h2,h3{font-family:Fraunces,Georgia,"Times New Roman",serif;font-weight:600;margin:0;letter-spacing:-.01em}
button{font:inherit;color:inherit;cursor:pointer}
:focus-visible{outline:2px solid var(--acento);outline-offset:2px}
.top{position:sticky;top:env(safe-area-inset-top,0px);z-index:5;display:flex;align-items:center;gap:14px;padding:12px clamp(16px,2.5vw,36px);background:color-mix(in srgb,var(--bg) 88%,transparent);backdrop-filter:blur(10px);border-bottom:1px solid var(--linea)}
.marca{display:flex;align-items:center;gap:10px;font-family:Fraunces,Georgia,serif;font-weight:600;font-size:18px}
.logo{width:30px;height:30px;border-radius:9px;background:var(--acento);color:var(--acento-t);display:grid;place-items:center;font-size:15px}
.top .esp{flex:1}
.ghost{background:none;border:1px solid var(--linea);border-radius:10px;padding:7px 12px;color:var(--suave)}
.ghost:hover{border-color:var(--acento);color:var(--tinta)}
.app{display:grid;grid-template-columns:minmax(240px,300px) minmax(0,1fr) minmax(300px,340px);gap:22px;padding:22px clamp(16px,2.5vw,36px) 40px;align-items:start}
.col-izq,.col-der{position:sticky;top:76px;max-height:calc(100vh - 92px);overflow:auto}
.paso{display:flex;align-items:center;gap:8px;font-size:12px;letter-spacing:.06em;text-transform:uppercase;color:var(--suave);font-weight:600;margin:0 0 10px}
.paso b{display:grid;place-items:center;width:20px;height:20px;border-radius:50%;background:var(--acento);color:var(--acento-t);font-size:11px;letter-spacing:0}
.buscar{width:100%;padding:9px 12px;border:1px solid var(--linea);border-radius:10px;background:var(--panel);color:var(--tinta);font:inherit;margin-bottom:10px}
.cargos{display:grid;gap:8px}
.cargo{text-align:left;background:var(--panel);border:1px solid var(--linea);border-radius:var(--r);padding:12px 14px;transition:border-color .15s,box-shadow .15s}
.cargo:hover{border-color:var(--acento)}
.cargo[aria-pressed=true]{border-color:var(--acento);box-shadow:0 0 0 2px var(--acento-s),var(--sombra)}
.cargo strong{display:block;font-weight:600}
.cargo small{color:var(--suave);display:block;margin-top:2px}
.cod{font:11px ui-monospace,Menlo,Consolas,monospace;color:var(--suave)}
.cab{display:flex;align-items:end;justify-content:space-between;gap:12px;margin-bottom:14px;flex-wrap:wrap}
.cab h2{font-size:24px}
.cab p{margin:2px 0 0;color:var(--suave)}
.link{background:none;border:0;color:var(--acento);font-weight:600;padding:4px}
.lista{display:grid;gap:12px;grid-template-columns:repeat(auto-fill,minmax(300px,1fr))}
.din{position:relative;display:flex;flex-direction:column;gap:8px;background:var(--panel);border:1px solid var(--linea);border-radius:var(--r);padding:16px 16px 14px;cursor:pointer;transition:border-color .15s,box-shadow .15s,transform .15s}
.din:hover{transform:translateY(-1px);box-shadow:var(--sombra)}
.din.on{border-color:var(--acento);box-shadow:0 0 0 2px var(--acento-s)}
.din input{position:absolute;opacity:0}
.tilde{position:absolute;top:14px;right:14px;width:22px;height:22px;border-radius:7px;border:1.5px solid var(--linea);display:grid;place-items:center;color:transparent;font-size:13px;font-weight:700}
.din.on .tilde{background:var(--acento);border-color:var(--acento);color:var(--acento-t)}
.din:has(input:focus-visible){outline:2px solid var(--acento);outline-offset:2px}
.din h3{font-size:16px;line-height:1.3;padding-right:30px}
.din p{margin:0;color:var(--suave);font-size:13.5px;display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden}
.meta{display:flex;align-items:center;gap:8px;margin-top:auto;padding-top:4px}
.tag{background:var(--acento-s);color:var(--acento);border-radius:7px;padding:2px 8px;font-size:12px;font-weight:600}
.tag.id{background:none;border:1px solid var(--linea);color:var(--suave);font:11px ui-monospace,Menlo,monospace}
.det{margin-left:auto;background:none;border:0;color:var(--acento);font-weight:600;font-size:13px;padding:4px 2px}
.panel{background:var(--panel);border:1px solid var(--linea);border-radius:18px;box-shadow:var(--sombra);padding:18px}
.panel h2{font-size:20px}
.sub{color:var(--suave);font-size:13px;margin:2px 0 14px}
.fila{display:flex;align-items:center;justify-content:space-between;gap:10px;padding:10px 0;border-top:1px solid var(--linea)}
.stp{display:flex;align-items:center;gap:2px;border:1px solid var(--linea);border-radius:10px}
.stp button{width:32px;height:32px;border:0;background:none;font-size:18px}
.stp output{min-width:28px;text-align:center;font-weight:600}
.sel{margin:0;padding:0;list-style:none;font-size:13.5px}
.sel li{display:flex;justify-content:space-between;gap:10px;padding:5px 0;color:var(--suave)}
.sel li span:first-child{color:var(--tinta)}.sel li span:last-child{white-space:nowrap}
.tmp{display:flex;align-items:center;gap:6px;color:var(--suave)}.tmp input{width:72px;padding:6px 8px;border:1px solid var(--linea);border-radius:8px;background:var(--bg);color:var(--tinta);font:inherit;font-weight:600;text-align:right}.tmp input:disabled{opacity:.5}.rest{border:0;background:none;color:var(--acento);font-size:17px;padding:2px 6px;border-radius:6px}.rest:hover{background:var(--acento-s)}.rest[hidden]{display:none}
.vacio{color:var(--suave);font-size:13.5px;padding:6px 0}
.cob{display:flex;flex-wrap:wrap;gap:5px;margin:8px 0 4px}
.cob i{font:600 11px ui-monospace,Menlo,monospace;font-style:normal;padding:3px 7px;border-radius:6px;border:1px dashed var(--linea);color:var(--suave)}
.cob i.ok{background:var(--acento-s);border:1px solid transparent;color:var(--acento)}
.barra{height:6px;border-radius:9px;background:var(--linea);overflow:hidden}
.barra div{height:100%;background:var(--acento);transition:width .25s}
.sw{display:flex;align-items:center;gap:10px;cursor:pointer}
.sw input{appearance:none;width:38px;height:22px;border-radius:99px;background:var(--linea);position:relative;margin:0;cursor:pointer;transition:background .15s}
.sw input::after{content:"";position:absolute;top:3px;left:3px;width:16px;height:16px;border-radius:50%;background:#fff;transition:transform .15s}
.sw input:checked{background:var(--acento)}
.sw input:checked::after{transform:translateX(16px)}
.btn{width:100%;margin-top:14px;padding:12px 16px;border:0;border-radius:12px;background:var(--acento);color:var(--acento-t);font-weight:600;font-size:15px}
.btn:disabled{opacity:.4;cursor:not-allowed}
.btn.sec{background:none;border:1px solid var(--linea);color:var(--tinta);margin:0;width:auto}
dialog{border:0;padding:0;border-radius:20px;background:var(--panel);color:var(--tinta);width:min(720px,calc(100vw - 24px));max-height:calc(100vh - 32px);box-shadow:0 24px 80px rgba(0,0,0,.35)}
dialog.ancho{width:min(980px,calc(100vw - 24px))}
dialog::backdrop{background:rgba(12,20,22,.58);backdrop-filter:blur(3px)}
.mh{display:flex;justify-content:space-between;align-items:start;gap:12px;padding:20px 22px 12px}
.mh h2{font-size:21px}
.x{width:34px;height:34px;border-radius:10px;border:1px solid var(--linea);background:none;font-size:18px;flex:none}
.mb{padding:4px 22px 18px;overflow:auto;max-height:calc(100vh - 190px)}
.mf{display:flex;gap:10px;justify-content:flex-end;padding:14px 22px;border-top:1px solid var(--linea)}
.mf .btn{margin:0;width:auto}
.bq{border-left:3px solid var(--acento);background:var(--acento-s);border-radius:0 10px 10px 0;padding:10px 14px;margin:10px 0;font-size:14px;white-space:pre-line}
.bq b{display:block;font-size:11px;letter-spacing:.06em;text-transform:uppercase;color:var(--acento);margin-bottom:3px}
.bq.av{border-color:var(--aviso);background:var(--aviso-s)}.bq.av b{color:var(--aviso)}
.cor{filter:blur(6px);cursor:pointer;user-select:none}.cor.ver{filter:none;user-select:auto}
.fondo{background:#8b948f33;padding:18px;overflow:auto;max-height:calc(100vh - 190px)}
.cab[hidden]{display:none}
.areas{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.area{text-align:left;background:var(--panel);border:1px solid var(--linea);border-radius:var(--r);padding:12px 14px}
.area:hover{border-color:var(--acento)}
.area[aria-pressed=true]{background:var(--acento);border-color:var(--acento);color:var(--acento-t)}
.area strong{display:block;font-weight:600}.area small{opacity:.75}
.paso.off{opacity:.45}.paso.off b{background:var(--suave)}
.vacio-box{border:1.5px dashed var(--linea);border-radius:var(--r);padding:26px 18px;text-align:center;color:var(--suave);font-size:14px}
.vacio-box b{color:var(--tinta);font-family:Fraunces,Georgia,serif;font-size:17px}
a.ghost{text-decoration:none;display:inline-block}
@media(max-width:1180px){.app{grid-template-columns:minmax(230px,290px) minmax(0,1fr);padding-bottom:130px}.col-izq{max-height:calc(100vh - 92px - 150px)}.col-der{position:fixed;top:auto;left:0;right:0;bottom:0;z-index:6;max-height:none;overflow:visible;padding:0 10px calc(10px + env(safe-area-inset-bottom,0px))}.col-der .extra{display:none}.col-der .panel{padding:12px 14px;border-radius:16px}.col-der .btn{margin-top:8px}}
@media(max-width:760px){.app{grid-template-columns:minmax(0,1fr)}.col-izq{position:static;max-height:none;overflow:visible}.cargos{grid-template-columns:repeat(auto-fill,minmax(210px,1fr))}.areas{grid-template-columns:repeat(auto-fill,minmax(140px,1fr))}}
.campo{display:block;margin:0 0 12px;font-size:13px;font-weight:600;color:var(--suave)}
.campo input{display:block;width:100%;margin-top:4px;padding:9px 11px;border:1px solid var(--linea);border-radius:10px;background:var(--bg);color:var(--tinta);font:inherit;font-weight:400}
.campo-fila{display:flex;gap:10px;align-items:end}
.err{color:var(--aviso);font-size:13px;min-height:1.2em;margin:4px 0 0}
.aviso-box{border:1px solid var(--aviso);background:var(--aviso-s);border-radius:var(--r);padding:12px 14px;font-size:14px;margin:0 0 10px}
.aviso-box .btn{width:auto;margin:10px 8px 0 0;padding:8px 14px}
.inf-lista{display:grid;gap:8px;margin-top:6px}
.inf-item{display:flex;justify-content:space-between;align-items:center;gap:12px;text-align:left;background:var(--panel);border:1px solid var(--linea);border-radius:var(--r);padding:10px 14px}
.inf-item:hover{border-color:var(--acento)}.inf-item small{display:block;color:var(--suave)}
.idm-s{font:700 12px ui-monospace,Menlo,Consolas,monospace;background:var(--acento-s);color:var(--acento);border-radius:6px;padding:3px 8px;white-space:nowrap}
.av-txt{color:var(--aviso);font-weight:600}.cod-g{font-family:ui-monospace,Menlo,Consolas,monospace}
@media(max-width:560px){.top{gap:8px}.top .txt{display:none}.mf{flex-wrap:wrap}.mf .sw{flex:1 0 100%;margin:0 0 4px!important}}
@media(max-width:480px){.marca .mt{display:none}}
.av-rel{margin-top:8px;font-size:12px;color:var(--aviso);line-height:1.4}.av-rel p{margin:0 0 4px}

```

### Archivo: `estilos/informe.css`
```css
/* Informe imprimible (hoja A4): estilos del informe, las tablas de puntaje y las hojas de trabajo. */
:root{--primary:#2c3e50;--secondary:#34495e;--accent:#2980b9;--light:#ecf0f1;--border:#bdc3c7}
.chip {
    font-size: 0.6875rem;
    padding: 2px 0.5rem;
    border-radius: 62.4375rem;
    background: #eaeded;
    color: #2c3e50;
}
.chip-id {
    font-family: monospace;
}
.chip-tiempo {
    background: #f4ecf7;
    color: #6c3483;
}
/* Estilos de la Hoja de Evaluación para Imprimir */
.print-container {
    max-width: 56.25rem;
    margin: 0 auto;
    background: white;
    padding: 2.5rem;
    border-radius: 0.5rem;
    box-shadow: 0 0.25rem 0.9375rem rgba(0,0,0,0.05);
}

.header-eval {
    display: flex;
    justify-content: space-between;
    border-bottom: 2px solid var(--primary);
    padding-bottom: 0.9375rem;
    margin-bottom: 1.25rem;
}

.header-eval h2 {
    margin: 0;
    color: var(--primary);
    font-size: 1.25rem;
}

.meta-info {
    font-size: 0.8125rem;
    color: #555;
    line-height: 1.5;
}

.section-title {
    background-color: var(--light);
    color: var(--primary);
    padding: 0.5rem 0.75rem;
    font-weight: bold;
    font-size: 0.875rem;
    border-left: 0.25rem solid var(--accent);
    margin: 1.25rem 0 0.625rem 0;
}

.dynamics-list {
    margin: 0.625rem 0;
    padding-left: 1.25rem;
    font-size: 0.875rem;
    line-height: 1.6;
}

.candidate-box {
    border: 1px solid var(--border);
    padding: 0.9375rem;
    margin-bottom: 1.25rem;
    border-radius: 0.375rem;
    background: #fafafa;
}

/* Evita cortar a mitad una dinámica o su tabla al saltar de página */
.candidate-box,
.bloque-dinamica {
    break-inside: avoid;
    page-break-inside: avoid;
}

.candidate-box h4 {
    margin: 0 0 0.625rem 0;
    color: var(--primary);
    font-size: 0.9375rem;
    border-bottom: 1px dashed var(--border);
    padding-bottom: 0.3125rem;
}

.metrics-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.8125rem;
    margin-bottom: 0.625rem;
    background: white;
}

.metrics-table th, .metrics-table td {
    border: 1px solid var(--border);
    padding: 0.375rem 0.5rem;
    text-align: left;
}

.metrics-table th {
    background-color: #eaeded;
}

.observations-box {
    border: 1px solid var(--border);
    min-height: 5.625rem;
    padding: 0.5rem;
    font-size: 0.8125rem;
    background: white;
    border-radius: 0.25rem;
    margin-top: 0.3125rem;
}

.general-notes {
    margin-top: 1.875rem;
    border-top: 1px dashed var(--border);
    padding-top: 0.9375rem;
    page-break-inside: avoid;
}

/* Leyenda y tabla de puntaje por postulante */
.leyenda-dinamicas {
    border: 1px solid var(--border);
    border-left: 0.25rem solid var(--accent);
    border-radius: 0.25rem;
    padding: 0.625rem 0.875rem;
    margin-bottom: 1rem;
    font-size: 0.8125rem;
    background: #fafafa;
}

.leyenda-dinamicas ul {
    list-style: none;
    margin: 0.5rem 0 0 0;
    padding: 0;
}

.leyenda-dinamicas li {
    padding: 0.1875rem 0;
    line-height: 1.6;
}

.badge-d {
    display: inline-block;
    min-width: 1.625rem;
    text-align: center;
    background: var(--primary);
    color: white;
    font-weight: bold;
    font-size: 0.6875rem;
    border-radius: 0.25rem;
    padding: 1px 0.3125rem;
    margin-right: 0.25rem;
}

.tabla-puntaje {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.8125rem;
    background: white;
    table-layout: fixed;
}

.tabla-puntaje th,
.tabla-puntaje td {
    border: 1px solid var(--border);
    padding: 0.5rem 0.625rem;
}

.tabla-puntaje thead th {
    background: var(--primary);
    color: white;
    text-align: center;
    font-size: 0.8125rem;
}

.tabla-puntaje thead th:first-child {
    text-align: left;
    width: 28%;
}

/* Columnas de competencias: compactas y sin salto de línea para que los códigos no se desalineen */
.tabla-puntaje thead th.col-puntaje {
    padding: 0.5rem 0.0625rem;
    font-size: 0.75rem;
    white-space: nowrap;
    overflow: hidden;
}

.tabla-puntaje td.celda-puntaje {
    padding-left: 0.125rem;
    padding-right: 0.25rem;
    white-space: nowrap;
    overflow: hidden;
}

.tabla-puntaje thead th small {
    display: block;
    font-weight: normal;
    font-size: 0.5625rem;
    opacity: 0.85;
    margin-top: 2px;
    word-break: break-all;
}

.celda-comp strong {
    display: block;
    color: var(--primary);
}

.celda-comp > span {
    display: block;
    font-size: 0.6875rem;
    color: #666;
    margin-top: 0.25rem;
    line-height: 1.6;
}

.tabla-puntaje .col-subtotal {
    width: 11%;
}

.tabla-puntaje tbody .col-subtotal {
    background: #eaeded;
    font-weight: 600;
}

.celda-puntaje {
    height: 2.75rem;
    vertical-align: bottom;
    text-align: right;
}

.tabla-puntaje td.celda-formula {
    white-space: normal;
    padding: 0.5rem 0.625rem;
}

.tabla-puntaje td.celda-no-aplica {
    background-color: #e1e4e7 !important;
    color: #7f8c8d;
}

.celda-puntaje .max {
    color: #95a5a6;
    font-size: 0.75rem;
}

.tabla-puntaje tbody tr:nth-child(even):not(.fila-total) {
    background: #f8f9f9;
}

.fila-subtotal td {
    background: #eaeded;
    font-weight: 600;
    color: var(--primary);
}

.fila-total td {
    background: #eaf3fb;
    border-top: 2px solid var(--primary);
    font-weight: bold;
    font-size: 0.875rem;
    color: var(--primary);
}

.fila-total .celda-puntaje {
    height: 3.25rem;
}

.fila-total .max {
    font-size: 0.9375rem;
    color: var(--primary);
}

/* Hoja de Trabajo / Consigna para el Postulante */
.candidate-sheet {
    margin-top: 2.5rem;
    padding-top: 1.25rem;
    display: flex;
    flex-direction: column;
}

.area-respuesta-postulante {
    background: #ffffff;
    border: 1px solid var(--border);
    border-radius: 0.375rem;
    padding: 1.5625rem;
    margin-top: 0.625rem;
    font-size: 0.9375rem;
    line-height: 1.7;
    white-space: pre-line;
    min-height: 21.875rem;
}

.area-respuesta-libre {
    flex: 1;
}

/* Configuración de Impresión */
@page {
    size: A4 portrait;
    margin: 12mm;
}

@media print {
    body {
        -webkit-print-color-adjust: exact;
        print-color-adjust: exact;
    }
    .candidate-box h4 {
        break-after: avoid;
    }
    .section-title {
        break-after: avoid;
    }
}

@media print {
    body {
        background: white;
        padding: 0;
    }
    .no-print {
        display: none;
    }
    .print-container {
        box-shadow: none;
        padding: 0;
        width: 100%;
        max-width: none;
        border-radius: 0;
    }
    .candidate-box {
        padding: 0.5rem;
    }
    .tabla-puntaje {
        font-size: 0.6875rem;
    }
    .tabla-puntaje th,
    .tabla-puntaje td {
        padding: 0.375rem 0.375rem;
    }
    .tabla-puntaje thead th.col-puntaje {
        padding: 0.375rem 0.0625rem;
        font-size: 0.6875rem;
    }
    .tabla-puntaje td.celda-puntaje {
        padding-left: 0.0625rem;
        padding-right: 0.1875rem;
    }
    .tabla-puntaje td.celda-formula {
        padding: 0.375rem;
    }
    .candidate-sheet {
        margin-top: 0;
        padding-top: 0;
        height: 273mm;
        box-sizing: border-box;
        page-break-before: always; /* Una hoja de trabajo por página */
        break-before: page;
        break-inside: avoid;
    }

    .area-respuesta-libre {
        min-height: 0;
    }
}

/* Estilo para los renglones de respuesta del postulante ocupando el 100% del ancho */
.renglon-respuesta {
    width: 100%;
    height: 2rem;
    border-bottom: 1px solid var(--border);
    margin-top: 0.625rem;
    margin-bottom: 0.625rem;
}
.print-container{color:#333;font:14px/1.5 "Segoe UI",Tahoma,Geneva,Verdana,sans-serif}
#hoja{max-width:56.25rem;margin:0 auto;border-radius:4px;box-shadow:0 4px 24px rgba(0,0,0,.2)}
.print-container h2,.print-container h3,.print-container h4{font-family:inherit;letter-spacing:0}
.print-container .idm{display:inline-block;white-space:nowrap;background:#eaeded;padding:2px 9px;border-radius:5px;font:700 18px/1.3 ui-monospace,Menlo,Consolas,monospace;letter-spacing:.02em;color:#2c3e50;vertical-align:middle}
.print-container .candidate-sheet .idm{font-size:22px;padding:3px 10px}
.print-container .tabla-puntaje .chip-id{font-size:12px;font-weight:700}
.print-container .cj{padding:.625rem;margin-top:.5rem;font-size:.8125rem;border-left:3px solid;background:#f9f9f9}
.print-container .cj.t{background:#f4ecf7;border-color:#8e44ad;color:#6c3483}
.print-container .cj.o{border-color:#e67e22}
.print-container .cj.g{background:#fdfefe;border-color:#27ae60;font-size:.75rem}
.print-container .cj.c{background:#eafaf1;border:1px dashed #1e8449;border-left:3px solid #1e8449;font-size:.75rem}
.print-container .bd{margin-bottom:.875rem}
.print-container .ht{color:var(--primary);margin:0;border-bottom:2px solid var(--primary);padding-bottom:.3rem}
.print-container .enc{margin:.9rem 0;font-size:.875rem;border:1px solid var(--border);padding:.75rem;border-radius:.25rem;line-height:1.8}
.print-container .pag{font-size:11px;color:#999;text-align:center;border-top:2px dashed #cfd6d9;padding-top:6px;margin-bottom:10px}
.print-container.ocultar-guia .bloque-guia{display:none}
@media screen,print{.print-container .header-eval{flex-wrap:wrap;gap:.5rem 1.25rem}.print-container .inf-linea{margin-top:.45rem;font-weight:700;display:flex;flex-wrap:wrap;align-items:center;gap:.25rem .5rem}.print-container .inf-n{color:#2c3e50;font-size:16px}.print-container .inf-c{color:var(--accent);font-size:14px}.print-container .inf-sep{color:#8a8a8a}}
/* Impresión: el informe se copia a #zonaImpresion (fuera del modal) y es lo único que sale en papel. */
#zonaImpresion{display:none}
@media print{body>*:not(#zonaImpresion){display:none!important}#zonaImpresion{display:block!important}.pag{display:none}}

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Generador de Evaluaciones Psicotécnicas</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,600&family=Instrument+Sans:wght@400;500;600&display=swap">
<link rel="stylesheet" href="estilos/aplicacion.css">
<link rel="stylesheet" href="estilos/informe.css">
</head>
<body>
<header class="top">
 <div class="marca"><span class="logo">✓</span><span class="mt">Evaluaciones psicotécnicas</span></div><div class="esp"></div>
 <button class="ghost" id="bInformes" title="Informes guardados">📂 Informes</button>
 <button class="ghost" id="tema" aria-label="Cambiar tema" title="Cambiar tema">☾<span class="txt"> Tema</span></button>
 <a class="ghost" href="admin.html" aria-label="Administrar datos" title="Administrar datos">⚙<span class="txt"> Administrar datos</span></a>
</header>
<main class="app">
 <section class="col-izq" aria-label="Área y cargo">
  <p class="paso" id="p1"><b>1</b> Área</p>
  <div class="areas" id="areas"></div>
  <p class="paso off" id="p2" style="margin-top:24px"><b>2</b> Cargo</p>
  <div class="cargos" id="cargos"></div>
 </section>
 <section aria-label="Dinámicas">
  <p class="paso off" id="p3"><b>3</b> Dinámicas a aplicar</p>
  <div class="cab" id="cab" hidden><div><h2 id="tCargo"></h2><p id="sCargo"></p></div><button class="link" id="todas"></button></div>
  <div class="lista" id="lista"></div>
 </section>
 <aside class="col-der" aria-label="Tu evaluación">
  <div class="panel">
   <h2 id="pCargo"></h2>
   <p class="sub" id="pSub"></p>
   <div class="extra">
    <div class="fila"><span>Postulantes</span><div class="stp"><button id="menos" aria-label="Menos">−</button><output id="n">2</output><button id="mas" aria-label="Más">+</button></div></div>
    <div class="fila" style="display:block"><b style="font-size:13px">Seleccionadas</b><ul class="sel" id="selLista"></ul></div>
    <div class="fila" style="display:block"><div style="display:flex;justify-content:space-between;font-size:13px"><b>Cobertura de competencias</b><span id="cobTxt" style="color:var(--suave)"></span></div><div class="cob" id="cob"></div><div class="barra"><div id="cobBar"></div></div><div class="av-rel" id="avRel" hidden></div></div>
    <div class="fila"><label class="sw"><input type="checkbox" id="guia" checked> Guía y corrector en el informe</label></div>
   </div>
   <div class="fila" style="border-top:1px solid var(--linea)"><label for="tiempoIn">Tiempo total</label><span class="tmp"><button type="button" class="rest" id="tReset" title="Volver al valor calculado" aria-label="Volver al valor calculado" hidden>↺</button><input id="tiempoIn" type="number" min="0" max="999" step="1" inputmode="numeric" placeholder="—" disabled><span>min</span></span></div>
   <button class="btn" id="gen" disabled>Generar informe</button>
  </div>
 </aside>
</main>

<dialog id="dDet"><div class="mh"><div><span class="tag id" id="dId"></span><h2 id="dTit" style="margin-top:6px"></h2></div><button class="x" data-cerrar aria-label="Cerrar">✕</button></div>
 <div class="mb" id="dCuerpo"></div>
 <div class="mf"><button class="btn sec" data-cerrar>Cerrar</button><button class="btn" id="dAdd"></button></div></dialog>

<dialog id="dInf" class="ancho"><div class="mh"><div><h2>Vista previa del informe</h2><p class="sub" id="subInf" style="margin:2px 0 0">Así saldrá en hoja A4. Revisa antes de imprimir.</p></div><button class="x" data-cerrar aria-label="Cerrar">✕</button></div>
 <div class="fondo"><article id="hoja" class="print-container"></article></div>
 <div class="mf"><label class="sw" style="margin-right:auto"><input type="checkbox" id="guia2" checked> Guía y corrector</label><button class="btn sec" data-cerrar>Volver a editar</button><button class="btn" onclick="print()">Imprimir A4</button></div></dialog>


<dialog id="dGuardar"><form id="fGuardar"><div class="mh"><h2>Generar y guardar informe</h2><button type="button" class="x" data-cerrar aria-label="Cerrar">✕</button></div>
 <div class="mb"><label class="campo">Nombre del informe <small style="font-weight:400">(opcional)</small><input name="nombre" id="gNombre" maxlength="80" autocomplete="off" placeholder="Ej.: Jefe de Sucursal Mercedes"></label>
 <label class="campo">Fecha de evaluación<input type="date" name="fecha" required></label>
 <div id="gClave" hidden><label class="campo">Contraseña para guardar<input type="password" name="clave" autocomplete="off"></label></div>
 <div id="gRel" class="aviso-box" hidden></div>
 <div id="gDup" class="aviso-box" hidden></div><p class="err" id="gErr"></p></div>
 <div class="mf"><button type="button" class="btn sec" data-cerrar>Cancelar</button><button class="btn" id="gOk">Generar y guardar</button></div></form></dialog>

<dialog id="dInfs"><div class="mh"><h2>Informes guardados</h2><button type="button" class="x" data-cerrar aria-label="Cerrar">✕</button></div>
 <div class="mb"><div class="campo-fila"><label class="campo" style="flex:1">Abrir por código<input id="infCod" autocomplete="off" placeholder="INF-2026-001"></label><button type="button" class="btn" id="infAbrir" style="width:auto;margin:0 0 12px">Abrir</button></div>
 <input type="search" id="infBus" class="buscar" placeholder="Buscar por nombre, cargo o código…" aria-label="Buscar informe"><div class="inf-lista" id="infLista"></div><p class="err" id="infErr"></p></div>
 <div class="mf"><button type="button" class="btn sec" data-cerrar>Cerrar</button></div></dialog>

<div id="zonaImpresion" aria-hidden="true"></div>
<script src="js/utilidades.js"></script>
<script src="datos/datos.js"></script>
<script src="js/informe.js"></script>
<script src="js/interfaz.js"></script>
</body>
</html>

```

### Archivo: `js/administracion.js`
```javascript

// Administración (ABM) con bajas lógicas. Depende de utilidades.js (esc, hojas) y datos/datos.js (cargar y guardar).
const $=s=>document.querySelector(s);
const limpio=t=>String(t||'').replace(/\s*\[cite:[^\]]*\]/g,'').trim();
const clon=o=>JSON.parse(JSON.stringify(o));
let base={areas:{},cargos:{},dinamicas:[]},D=clon(base),LOG=[],V='act';const S={area:null,cargo:null};
const act=o=>o.activo!==false,vis=o=>V==='todos'||(V==='act'?act(o):!act(o));
const areaDe=c=>Object.keys(D.areas).find(k=>k===c.area||D.areas[k].nombre===c.area);
const cargosDe=k=>Object.entries(D.cargos).filter(([,c])=>areaDe(c)===k);
const dinDe=k=>D.dinamicas.filter(d=>d.cargo_id===k);
const getO=(t,k)=>t==='a'?D.areas[k]:t==='c'?D.cargos[k]:D.dinamicas.find(d=>d.id===k);
const nom=(t,k)=>t==='d'?k:(getO(t,k).nombre);
const log=t=>{LOG.push(t);pintaPanel()};
const toast=t=>{const e=document.createElement('div');e.className='toast';e.textContent=t;document.body.append(e);setTimeout(()=>e.remove(),2300)};
// Competencias ↔ dinámicas: una competencia está "en uso" si alguna dinámica del cargo la tiene marcada.
const usosComp=(ck,codigo)=>dinDe(ck).filter(d=>Array.isArray(d.competencias)?d.competencias.includes(codigo):codigosCitados(d.caso_o_consigna).includes(codigo));
// Da código a las competencias que no lo tienen: sigue el prefijo más usado del cargo (1.x si no hay ninguno) y continúa la numeración.
function asignaCodigos(cs){const con=cs.filter(x=>x.codigo),cnt={};con.forEach(x=>{const p=x.codigo.split('.')[0];cnt[p]=(cnt[p]||0)+1});
 const pre=Object.keys(cnt).sort((a,b)=>cnt[b]-cnt[a])[0]||'1';
 let mx=Math.max(0,...con.filter(x=>x.codigo.startsWith(pre+'.')).map(x=>parseInt(x.codigo.slice(pre.length+1),10)||0));
 cs.forEach(x=>{if(!x.codigo)x.codigo=`${pre}.${++mx}`})}
// Datos anteriores a este cambio: da código a las competencias y arma la relación de cada dinámica con los códigos que citaba en su texto.
// Solo modifica en memoria y lo deja como cambio pendiente: se guarda con el botón Guardar.
function migrar(){let nc=0,nd=0;
 Object.values(D.cargos).forEach(c=>{if(c.competencias.some(x=>!x.codigo)){asignaCodigos(c.competencias);nc++}});
 D.dinamicas.forEach(d=>{const car=D.cargos[d.cargo_id];if(!car||Array.isArray(d.competencias))return;
  const cods=car.competencias.map(x=>x.codigo);d.competencias=codigosCitados(d.caso_o_consigna).filter((x,i,a)=>cods.includes(x)&&a.indexOf(x)===i);nd++});
 if(nc)LOG.push(`Código asignado a las competencias de ${nc} cargo${nc>1?'s':''}`);
 if(nd)LOG.push(`Relación con competencias creada en ${nd} dinámica${nd>1?'s':''}`)}
const chipsComp=(d,c)=>{const r=competenciasDeDinamica(d,c),pm=puntajeMaximoDinamica(d);
 return (act(d)&&!r.length?'<span class="bad">Sin competencia</span>':r.map(x=>`<span class="tag cmp">${esc(x)}</span>`).join(''))+(pm!==PUNTAJE_MAX_COMPETENCIA?`<span class="tag id" title="Puntaje máximo por competencia">máx. ${pm} c/u</span>`:'')};
const botones=(t,k,o)=>`<span class="ac"><button data-x="e${t}|${k}" title="Editar" aria-label="Editar">✏️</button>${act(o)?`<button data-x="b${t}|${k}" title="Dar de baja" aria-label="Dar de baja">⏻</button>`:`<button data-x="r${t}|${k}" title="Reactivar" aria-label="Reactivar">↺</button>`}</span>`;

function pintaAreas(){const l=Object.entries(D.areas).filter(([,a])=>vis(a));
 $('#ar').innerHTML=l.map(([k,a])=>`<div class="it ${k===S.area?'on':''} ${act(a)?'':'ina'}" data-s="${k}"><div><strong>${esc(a.nombre)}</strong><small>${k} · ${cargosDe(k).filter(([,c])=>act(c)).length} cargos activos ${act(a)?'':'<span class="bad">Inactiva</span>'}</small></div>${botones('a',k,a)}</div>`).join('')||'<div class="vacio-box">Nada para mostrar en esta vista.</div>'}
function pintaCargos(){$('#p2').classList.toggle('off',!S.area);$('#nC').hidden=!S.area;
 if(!S.area){$('#ca').innerHTML='<div class="vacio-box">Elige un área.</div>';return}
 $('#ca').innerHTML=cargosDe(S.area).filter(([,c])=>vis(c)).map(([k,c])=>`<div class="it ${k===S.cargo?'on':''} ${act(c)?'':'ina'}" data-s="${k}"><div><strong>${esc(bonito(c.nombre))}</strong><small class="cod">${k} · ${dinDe(k).filter(act).length} dinámicas activas ${act(c)?'':'<span class="bad">Inactivo</span>'}</small></div>${botones('c',k,c)}</div>`).join('')||'<div class="vacio-box">Sin cargos en esta vista.</div>'}
function pintaDin(){const c=D.cargos[S.cargo];$('#p3').classList.toggle('off',!c);$('#cab').hidden=!c;$('#avC').hidden=true;
 if(!c){$('#lista').innerHTML=`<div class="vacio-box" style="grid-column:1/-1"><b>${S.area?'Ahora elige un cargo':'Empieza eligiendo un área'}</b><br>Sus dinámicas aparecerán acá.</div>`;return}
 $('#tC').textContent=bonito(c.nombre);$('#sC').textContent=`${c.competencias.length} competencias · ${dinDe(S.cargo).filter(act).length} dinámicas activas`;
 const av=avisosRelaciones(c,dinDe(S.cargo).filter(act));$('#avC').innerHTML=av.map(t=>`<span>⚠ ${t}</span>`).join('<br>');$('#avC').hidden=!av.length;
 $('#lista').innerHTML=dinDe(S.cargo).filter(vis).map(d=>`<div class="din adm ${act(d)?'':'ina'}"><h3>${esc(d.titulo)}</h3><p>${esc(limpio(d.desc))}</p><div class="meta"><span class="tag id">${d.id.replace('DIN-','')}</span>${chipsComp(d,c)}${act(d)?'':'<span class="bad">Inactiva</span>'}<span style="margin-left:auto">${botones('d',d.id,d)}</span></div></div>`).join('')||'<div class="vacio-box" style="grid-column:1/-1">Sin dinámicas en esta vista.</div>'}
function pintaPanel(){const n=LOG.length;$('#pn').textContent=n?`${n} cambio${n>1?'s':''} sin guardar`:'Todo guardado.';
 $('#log').innerHTML=LOG.map(t=>`<li>${esc(t)}</li>`).join('');$('#gu').disabled=$('#de').disabled=!n;
 const c=(o)=>Object.values(o).filter(act).length,i=(o)=>Object.values(o).length-c(o);
 $('#res').textContent=`${c(D.areas)} áreas · ${c(D.cargos)} cargos · ${D.dinamicas.filter(act).length} dinámicas activas (${i(D.areas)+i(D.cargos)+D.dinamicas.filter(d=>!act(d)).length} inactivas)`}
const todo=()=>{pintaAreas();pintaCargos();pintaDin();pintaPanel()};

/* modal genérico */
const dm=$('#dm');let onOk=null;
function abre(titulo,html,ok,fn){$('#mt').textContent=titulo;document.querySelector('#fm .mf .sec').textContent='Cancelar';$('#mc').innerHTML=html+'<p class="err" id="er"></p>';$('#ok').textContent=ok||'';$('#ok').hidden=!ok;onOk=fn;dm.showModal()}
const err=m=>{$('#er').textContent=m;return false};
$('#fm').onsubmit=e=>{e.preventDefault();if(!onOk)return;if(onOk(new FormData($('#fm')))!==false)dm.close()};
dm.addEventListener('click',e=>{if(e.target===dm||e.target.closest('[data-c]'))dm.close()});

function fArea(k){const a=k?D.areas[k]:{nombre:''};
 abre(k?'Editar área':'Nueva área',`<label class="cp">Nombre<input name="nombre" value="${esc(a.nombre)}" required></label><label class="cp">Sigla (3 letras)<input name="sigla" value="${esc(k||'')}" maxlength="3" minlength="3" ${k?'readonly':''} style="text-transform:uppercase" required></label>`,k?'Guardar cambios':'Crear área',f=>{
  const nombre=f.get('nombre').trim(),sg=f.get('sigla').trim().toUpperCase();
  if(!k&&D.areas[sg])return err('Ya existe un área con esa sigla.');
  if(k){const ant=D.areas[k].nombre;Object.values(D.cargos).forEach(c=>{if(c.area===ant)c.area=nombre});D.areas[k].nombre=nombre;log(`Área ${k} modificada`)}
  else{D.areas[sg]={nombre};S.area=sg;S.cargo=null;log(`Área ${sg} creada`)}
  todo()})}
const filaC=(c,k)=>{const uso=k&&c.codigo?usosComp(k,c.codigo):[];
 return `<div class="fc" data-orig="${esc(c.codigo||'')}"><input class="cc" placeholder="Auto" maxlength="12" value="${esc(c.codigo||'')}" ${uso.length?`readonly title="La usan ${uso.length} dinámica(s): el código no se puede cambiar"`:''}><input class="cn" placeholder="Competencia" value="${esc(c.comp||'')}"><input class="cd" placeholder="Qué se observa" value="${esc(c.desc?limpio(c.desc):'')}"><button type="button" class="x" data-delc aria-label="Quitar">✕</button></div>`};
function fCargo(k){const c=k?D.cargos[k]:{nombre:'',competencias:[{},{},{}]};dm.dataset.cargo=k||'';
 const ops=Object.entries(D.areas).filter(([,a])=>act(a)).map(([s,a])=>`<option value="${s}" ${S.area===s?'selected':''}>${esc(a.nombre)} [${s}]</option>`).join('');
 abre(k?'Editar cargo y competencias':'Nuevo cargo',`<label class="cp">Nombre del cargo<input name="nombre" value="${esc(c.nombre)}" required></label>
 ${k?`<p class="sub">Código: <span class="cod">${k}</span></p>`:`<div class="dos"><label class="cp">Área<select name="area" required>${ops}</select></label><label class="cp">Especialidad (3 letras)<input name="esp" maxlength="3" style="text-transform:uppercase" required></label></div>`}
 <p class="paso" style="margin-top:12px">Competencias · columnas de la tabla de puntaje</p><div id="comps">${c.competencias.map(x=>filaC(x,k)).join('')}</div><button type="button" class="link" data-addc>+ Agregar competencia</button>`,k?'Guardar cambios':'Crear cargo',f=>{
  const cs=[...document.querySelectorAll('#comps .fc')].map(r=>({codigo:r.querySelector('.cc').value.trim(),comp:r.querySelector('.cn').value.trim(),desc:r.querySelector('.cd').value.trim()})).filter(x=>x.comp);
  if(!cs.length)return err('Define al menos una competencia.');
  if(new Set(cs.map(x=>x.comp.toLowerCase())).size<cs.length)return err('Hay competencias repetidas.');
  const cods=cs.map(x=>x.codigo).filter(Boolean);if(new Set(cods).size<cods.length)return err('Hay códigos repetidos.');
  asignaCodigos(cs);
  if(k){const nuevos=new Set(cs.map(x=>x.codigo)),perdidas=D.cargos[k].competencias.filter(x=>x.codigo&&!nuevos.has(x.codigo)&&usosComp(k,x.codigo).length);
   if(perdidas.length)return err(`No se puede quitar ni cambiar ${perdidas.map(x=>x.codigo).join(', ')}: ${perdidas.length>1?'las usan':'la usan'} dinámicas de este cargo. Desmárca${perdidas.length>1?'las':'la'} primero en esas dinámicas.`)}
  const nombre=f.get('nombre').trim();
  if(k){D.cargos[k].nombre=nombre;D.cargos[k].competencias=cs;log(`Cargo ${k} modificado`)}
  else{const sg=f.get('area'),esp=(f.get('esp').trim().toUpperCase()+'XXX').slice(0,3),pre=`CAR-${sg}-${esp}-`;
   const mx=Math.max(0,...Object.keys(D.cargos).filter(x=>x.startsWith(pre)).map(x=>parseInt(x.slice(pre.length),10)||0));
   const key=pre+String(mx+1).padStart(2,'0');D.cargos[key]={nombre,area:D.areas[sg].nombre,competencias:cs};S.area=sg;S.cargo=key;log(`Cargo ${key} creado`)}
  todo()})}
function fDin(id){const d=id?D.dinamicas.find(x=>x.id===id):{peso:1},hoja=separarRenglones(d.hoja_postulante||'').texto;
 const T=(n,l,v,r)=>`<label class="cp">${l}<input name="${n}" value="${esc(v??'')}" ${r?'required':''}></label>`,A=(n,l,v,r)=>`<label class="cp">${l}<textarea name="${n}" ${r?'required':''}>${esc(v??'')}</textarea></label>`;
 const car=D.cargos[S.cargo],marcadas=Array.isArray(d.competencias)?d.competencias:codigosCitados(d.caso_o_consigna),
  checks=car.competencias.map((x,i)=>{const k2=codigoCompetencia(x,i);return `<label class="chk"><input type="checkbox" name="comp" value="${esc(k2)}" ${marcadas.includes(k2)?'checked':''}><span class="cod">${esc(k2)}</span><span>${esc(x.comp)}</span></label>`}).join('');
 abre(id?'Editar dinámica':'Nueva dinámica',`${id?`<p class="sub">Código: <span class="cod">${id}</span></p>`:''}
 <p class="paso">Datos</p>${T('titulo','Título',d.titulo,1)}${T('desc','Descripción breve',limpio(d.desc),1)}<div class="dos">${T('tiempo','Tiempo límite (0 si no lleva tiempo)',d.tiempo_limite,1)}<label class="cp">Peso (1 a 5)<input type="number" name="peso" min="1" max="5" value="${d.peso||1}"></label></div>
 <p class="paso" style="margin-top:8px">Competencias que evalúa</p><p class="sub" style="margin:0 0 8px">Marca una o varias: de ahí sale la tabla de puntaje de cada postulante.</p>${checks}
 <label class="cp" style="max-width:280px">Puntaje máximo por competencia (vacío = ${PUNTAJE_MAX_COMPETENCIA})<input type="number" name="pmax" min="1" max="20" value="${d.puntaje_maximo??''}"></label>
 <p class="paso" style="margin-top:8px">Hoja del postulante</p>${A('hoja','Consigna que verá el postulante',hoja,1)}<label class="cp" style="max-width:200px">Renglones (vacío = 5)<input type="number" name="reng" min="0" max="30" value="${d.renglones_hoja??''}"></label>
 <p class="paso" style="margin-top:8px">Para el evaluador</p>${A('caso','Objetivo de la prueba',limpio(d.caso_o_consigna),1)}${A('guia','Guía de observación (Éxito / Alerta)',d.guia_evaluacion,1)}${A('resp','Corrector: respuestas esperadas',d.respuesta_esperada)}`,id?'Guardar cambios':'Crear dinámica',f=>{
  const ck=S.cargo,r=f.get('reng').trim(),rn=r===''?RENGLONES_HOJA_GLOBAL:normalizarCantidadRenglones(r);
  const o={id:id||'',cargo_id:ck,titulo:f.get('titulo').trim(),desc:f.get('desc').trim(),tiempo_limite:f.get('tiempo').trim(),peso:Math.min(5,Math.max(1,parseInt(f.get('peso'),10)||1)),
   hoja_postulante:componerHoja(f.get('hoja'),rn),caso_o_consigna:f.get('caso').trim(),competencias:f.getAll('comp'),guia_evaluacion:f.get('guia').trim(),respuesta_esperada:f.get('resp').trim()};
  if(r!=='')o.renglones_hoja=rn;
  const pm=f.get('pmax').trim();if(pm!=='')o.puntaje_maximo=Math.min(20,Math.max(1,parseInt(pm,10)||PUNTAJE_MAX_COMPETENCIA));
  if(id){const i=D.dinamicas.findIndex(x=>x.id===id);if(d.activo===false)o.activo=false;D.dinamicas[i]=o;log(`Dinámica ${id} modificada`)}
  else{const p=ck.split('-'),pre=p.length>=4?`DIN-${p[1]}-${p[2]}-`:`DIN-${ck}-`;
   const mx=Math.max(0,...D.dinamicas.filter(x=>x.id.startsWith(pre)).map(x=>parseInt(x.id.slice(pre.length),10)||0));o.id=pre+String(mx+1).padStart(2,'0');D.dinamicas.push(o);log(`Dinámica ${o.id} creada`)}
  todo()})}

function baja(t,k){const o=getO(t,k);let msg='';
 if(t==='a'){const cs=cargosDe(k).filter(([,c])=>act(c));msg=`${cs.length} cargo(s) y ${cs.reduce((n,[x])=>n+dinDe(x).filter(act).length,0)} dinámica(s) dejarán de aparecer en el generador.`}
 if(t==='c')msg=`${dinDe(k).filter(act).length} dinámica(s) dejarán de aparecer en el generador.`;
 if(t==='d')msg='Dejará de aparecer en el generador.';
 abre('Dar de baja',`<p><b>${esc(nom(t,k))}</b></p><p>${msg}</p><p class="sub">No se borra nada: queda en Inactivos y puedes reactivarla cuando quieras.</p>`,'Dar de baja',()=>{o.activo=false;log(`${t==='a'?'Área':t==='c'?'Cargo':'Dinámica'} ${k} dada de baja`);todo()})}
function reac(t,k){delete getO(t,k).activo;log(`${t==='a'?'Área':t==='c'?'Cargo':'Dinámica'} ${k} reactivada`);toast('Reactivado');todo()}
const acc=c=>{const[o,k]=c.split('|');({ea:()=>fArea(k),ba:()=>baja('a',k),ra:()=>reac('a',k),ec:()=>fCargo(k),bc:()=>baja('c',k),rc:()=>reac('c',k),ed:()=>fDin(k),bd:()=>baja('d',k),rd:()=>reac('d',k)})[o]()};

/* asistente */
const A={modo:1,area:'',cargo:'',n:4,texto:'',crudo:'',terminos:'',out:''};
const MAMMOTH='https://cdnjs.cloudflare.com/ajax/libs/mammoth/1.6.0/mammoth.browser.min.js';
const cargaMammoth=()=>window.mammoth?Promise.resolve():new Promise((ok,ko)=>{const x=document.createElement('script');x.src=MAMMOTH;x.onload=ok;x.onerror=()=>ko(new Error('No se pudo cargar el lector de .docx'));document.head.appendChild(x)});
function anonimizar(texto,terminos){let t=texto.replace(/[\w.+-]+@[\w-]+\.[\w.-]+/g,'[EMAIL]').replace(/\b\d{1}\.?\d{3}\.?\d{3}-?\d\b/g,'[DOC]').replace(/\b\d{1,2}[\/\-.]\d{1,2}[\/\-.]\d{2,4}\b/g,'[FECHA]').replace(/(\$|U\$S|USD|UYU)\s?[\d.,]+/gi,'[MONTO]').replace(/\b(llamado|concurso|licitaci[oó]n)\s*(n[°ºo.]*\s*)?[\w\/-]*\d[\w\/-]*/gi,'[LLAMADO]');
 terminos.forEach(w=>{if(w.trim())t=t.replace(new RegExp(w.trim().replace(/[.*+?^${}()|[\]\\]/g,'\\$&'),'gi'),'[X]')});return t}
async function leerDocx(f){try{A.terminos=(document.querySelector('[name=terminos]')||{value:A.terminos}).value;await cargaMammoth();const r=await window.mammoth.extractRawText({arrayBuffer:await f.arrayBuffer()});A.crudo=r.value;A.texto=anonimizar(A.crudo,A.terminos.split(','));A.out='';pintaA()}catch(x){$('#er2').textContent=x.message}}
function nuevoTexto(){const m=A.modo,ar=D.areas[A.area],ca=D.cargos[A.cargo];
 const jd='"dinamicas":[{"titulo":"","desc":"","tiempo_limite":"10 Minutos","peso":1,"hoja_postulante":"","caso_o_consigna":"","competencias":["1.1"],"guia_evaluacion":"• Éxito: …\\n• Alerta: …","respuesta_esperada":""}]',jc='"competencias":[{"codigo":"1.1","comp":"","desc":""}]';
 const forma=m===1?`{"area":{"sigla":"3 letras","nombre":""},"cargo":{"especialidad":"3 letras","nombre":"",${jc}},${jd}}`:m===2?`{"cargo":{"especialidad":"3 letras","nombre":"",${jc}},${jd}}`:`{${jd}}`;
 const tarea=m===1?'Crea un área nueva, un cargo dentro de ella y sus dinámicas de evaluación':m===2?`Crea un cargo nuevo dentro del área existente "${ar.nombre}" y sus dinámicas de evaluación`:`Crea únicamente dinámicas de evaluación nuevas para el cargo existente "${ca.nombre}"`;
 const cs=m===3?ca.competencias:[];
 const L=[`Eres un especialista en selección de personal y evaluación psicotécnica. ${tarea}, a partir de las bases del llamado que figuran al final.`,'',
  m===3?`Competencias del cargo (úsalas tal cual):\n${cs.map((c,i)=>`- ${c.codigo||'C'+String(i+1).padStart(2,'0')} ${c.comp}: ${limpio(c.desc)}`).join('\n')}\nDinámicas que ya existen (no las repitas): ${dinDe(A.cargo).map(d=>d.titulo).join('; ')||'ninguna'}.\n`:'',
  `Genera exactamente ${A.n} dinámicas. Reglas:`,
  m===3?'- En "competencias" de cada dinámica lista los códigos de las competencias del cargo que evalúa (por ejemplo ["6.1","6.3"]), usando solo códigos de la lista de arriba: la app los usa para armar la tabla de puntaje.':'- Define entre 3 y 13 competencias del cargo con códigos del tipo 1.1, 1.2…; en "competencias" de cada dinámica lista los códigos que evalúa (por ejemplo ["1.1","1.3"]): la app los usa para armar la tabla de puntaje.',
  '- "tiempo_limite" es texto con minutos (ej. "10 Minutos"). "peso" va de 1 a 5.','- "hoja_postulante" es solo lo que verá el postulante, sin respuestas.','- "guia_evaluacion" usa las viñetas "• Éxito:" y "• Alerta:". "respuesta_esperada" da el criterio de corrección.','- No inventes códigos de identificación: la app los asigna.','- No incluyas datos personales.',
  `Códigos que ya existen, para no duplicar: áreas ${Object.keys(D.areas).join(', ')}; cargos ${Object.keys(D.cargos).join(', ')}.`,'',
  'Responde SOLO con JSON válido, sin texto adicional ni bloques de código, con esta forma:',forma,'','=== BASES DEL LLAMADO (anonimizadas) ===',A.texto.trim()||'[pega aquí el texto de las bases]','=== FIN ==='];
 return L.filter(x=>x!==null).join('\n')}
function pintaA(){const sel=(k,l,o)=>`<label class="cp">${l}<select name="${k}"><option value="">Elegir…</option>${o}</select></label>`;
 const oa=Object.entries(D.areas).filter(([,a])=>act(a)).map(([k,a])=>`<option value="${k}" ${A.area===k?'selected':''}>${esc(a.nombre)}</option>`).join(''),
  oc=Object.entries(D.cargos).filter(([k,c])=>act(c)&&areaDe(c)===A.area).map(([k,c])=>`<option value="${k}" ${A.cargo===k?'selected':''}>${esc(bonito(c.nombre))}</option>`).join('');
 const M=(n,t,s)=>`<label class="modo"><input type="radio" name="modo" value="${n}" ${A.modo===n?'checked':''}><b>${t}</b><small>${s}</small></label>`;
 $('#mc').innerHTML=`<p class="paso"><b>1</b> ¿Qué quieres que genere la IA?</p>${M(1,'Área, cargo y dinámicas','Todo es nuevo.')}${M(2,'Cargo y dinámicas','El área ya existe.')}${M(3,'Solo dinámicas','El área y el cargo ya existen.')}
 ${A.modo>1?`<div class="dos">${sel('area','Área existente',oa)}${A.modo===3?sel('cargo','Cargo existente',oc):'<span></span>'}</div>`:''}
 <p class="paso" style="margin-top:12px"><b>2</b> Bases del llamado</p><p class="sub" style="margin:0 0 8px">El archivo se lee en este navegador y no se envía a ningún servidor. Revisa el texto: es lo único que irá en el prompt.</p>
 <label class="cp">Archivo .docx<input type="file" name="archivo" accept=".docx"></label>
 <label class="cp">Términos a enmascarar <small style="font-weight:400">(organismo, nombres, sectores; separados por coma)</small><input name="terminos" value="${esc(A.terminos)}"></label>
 <label class="cp">Texto de las bases (editable)<textarea name="texto" style="min-height:110px">${esc(A.texto)}</textarea></label>
 <label class="cp" style="max-width:200px">Cantidad de dinámicas<input type="number" name="n" min="1" max="10" value="${A.n}"></label>
 <button type="button" class="btn" data-gen style="width:auto;margin:0 0 10px">Generar prompt</button><p class="err" id="er2"></p>
 ${A.out?`<p class="paso"><b>3</b> Prompt para tu IA</p><textarea class="pre" readonly id="pre">${esc(A.out)}</textarea><button type="button" class="ghost" data-cop style="margin-top:8px">Copiar prompt</button><p class="sub">Revisa el texto antes de usarlo: la anonimización no garantiza quitar todos los datos sensibles.</p>`:''}`}
$('#bAsist').onclick=()=>{abre('Asistente de bases',' ',null,null);document.querySelector('#fm .mf .sec').textContent='Cerrar';dm.dataset.tipo='asist';A.area=A.area||S.area||'';A.cargo=A.cargo||S.cargo||'';pintaA()};
dm.addEventListener('close',()=>{dm.dataset.tipo='';dm.dataset.cargo='';A.out=''});
$('#mc').addEventListener('change',e=>{if(dm.dataset.tipo!=='asist')return;const n=e.target.name,v=e.target.value;
 if(n==='archivo'){if(e.target.files[0])leerDocx(e.target.files[0]);return}
 if(n==='terminos'){A.terminos=v;if(A.crudo){A.texto=anonimizar(A.crudo,v.split(','));A.out='';pintaA()}return}
 if(n==='modo'){A.modo=+v;A.out=''}else if(n==='area'){A.area=v;A.cargo='';A.out=''}else if(n==='cargo'){A.cargo=v;A.out=''}else if(n==='n')A.n=Math.min(10,Math.max(1,+v||1));
 if(n==='texto')A.texto=v;if(n==='modo'||n==='area')pintaA()});
$('#mc').addEventListener('click',e=>{
 if(e.target.closest('[data-addc]'))$('#comps').insertAdjacentHTML('beforeend',filaC({}));
 const q=e.target.closest('[data-delc]');if(q){const fila=q.closest('.fc'),orig=fila.dataset.orig,ck=dm.dataset.cargo,u=orig&&ck?usosComp(ck,orig):[];
  if(u.length)return err(`No se puede quitar ${orig}: la usa${u.length>1?'n':''} ${u.map(d=>d.id.replace('DIN-','')).join(', ')}. Desmárcala primero en ${u.length>1?'esas dinámicas':'esa dinámica'}.`);
  if(document.querySelectorAll('#comps .fc').length>1)fila.remove()}
 if(e.target.closest('[data-gen]')){const t=document.querySelector('[name=texto]');A.texto=t.value;
  if(A.modo>1&&!A.area)return $('#er2').textContent='Elige el área existente.';if(A.modo===3&&!A.cargo)return $('#er2').textContent='Elige el cargo existente.';
  A.out=nuevoTexto();pintaA();document.querySelector('#pre').scrollIntoView({block:'nearest'})}
 if(e.target.closest('[data-cop]')){navigator.clipboard&&navigator.clipboard.writeText(A.out).then(()=>toast('Prompt copiado'),()=>{$('#pre').select()})}});

/* eventos */
document.querySelectorAll('.seg button').forEach(b=>b.onclick=()=>{V=b.dataset.v;document.querySelectorAll('.seg button').forEach(x=>x.setAttribute('aria-pressed',x===b));todo()});
$('#ar').onclick=e=>{const x=e.target.closest('[data-x]');if(x)return acc(x.dataset.x);const s=e.target.closest('[data-s]');if(s){S.area=s.dataset.s;S.cargo=null;todo()}};
$('#ca').onclick=e=>{const x=e.target.closest('[data-x]');if(x)return acc(x.dataset.x);const s=e.target.closest('[data-s]');if(s){S.cargo=s.dataset.s;todo()}};
$('#lista').onclick=e=>{const x=e.target.closest('[data-x]');if(x)acc(x.dataset.x)};
$('#nA').onclick=()=>fArea();$('#nC').onclick=()=>fCargo();$('#nD').onclick=()=>fDin();$('#eC').onclick=()=>fCargo(S.cargo);
$('#gu').onclick=()=>abre('Guardar','<p class="sub">Se guardarán los cambios en la base de datos.</p><label class="cp">Contraseña<input type="password" name="pw" autocomplete="off"></label>','Guardar',f=>{
 const b=$('#ok'),e2=$('#er');b.disabled=true;e2.style.color='var(--suave)';e2.textContent='Guardando…';
 Datos.guardar(D,f.get('pw')).then(r=>{base=clon(D);LOG=[];pintaPanel();dm.close();toast(r.escritos.length?`Guardado: ${r.escritos.join(', ')}`:'No había cambios para guardar')}).catch(x=>{e2.style.color='';err(x.message)}).finally(()=>{b.disabled=false});
 return false});
$('#de').onclick=()=>abre('Descartar cambios',`<p>Se perderán ${LOG.length} cambio(s) sin guardar.</p>`,'Descartar',()=>{D=clon(base);LOG=[];migrar();if(!D.areas[S.area]){S.area=null}if(!D.cargos[S.cargo])S.cargo=null;todo()});
$('#tema').onclick=()=>{const r=document.documentElement,os=matchMedia('(prefers-color-scheme: dark)').matches;r.dataset.theme=(r.dataset.theme||(os?'dark':'light'))==='dark'?'light':'dark'};
window.addEventListener('beforeunload',e=>{if(LOG.length){e.preventDefault();e.returnValue=''}});
async function iniciar(){
 $('#ar').innerHTML='<div class="vacio-box">Cargando catálogo…</div>';
 try{base=await Datos.cargar();D=clon(base);migrar();todo()}
 catch(e){$('#ar').innerHTML=`<div class="vacio-box"><b>No se pudo cargar el catálogo</b><br>${esc(e.message)}<br><button class="btn sec" id="reint" style="margin-top:12px;width:auto">Reintentar</button></div>`;$('#reint').onclick=iniciar}}
iniciar();

```

### Archivo: `js/informe.js`
```javascript
// Construye el HTML del informe imprimible. Depende de utilidades.js (esc, prepararHojaDinamica, cantidadRenglonesDinamica).
const limpio=t=>String(t||'').replace(/\s*\[cite:[^\]]*\]/g,'').trim();
// Sin tiempo = campo vacío o con valor 0 ("0", "0 minutos"). Un texto sin números ("Sin límite") se muestra tal cual.
const sinTiempo=d=>{const t=String(d.tiempo_limite??'').trim();if(!t)return true;const n=t.match(/\d+/g);return !!n&&Math.max(...n.map(Number))===0};
const mins=d=>{const n=(d.tiempo_limite||'').match(/\d+/g);return n?Math.max(...n.map(Number)):0};
const cod=codigoCompetencia,aplica=competenciasDeDinamica; // definidas en utilidades.js (relación competencias ↔ dinámicas)
const nl=(t,v)=>esc(limpio(t)||v).replace(/\n/g,'<br>');

const fmtFecha=f=>/^\d{4}-\d{2}-\d{2}$/.test(f||'')?f.split('-').reverse().join('/'):'';
function construirInforme(key,car,sel,n,tiempoTotal,meta){meta=meta||{};const comps=car.competencias,MAXF=30;
 const calc=sel.reduce((a,d)=>a+mins(d),0),total=tiempoTotal==null?calc:tiempoTotal,multi=sel.length>1,editado=tiempoTotal!=null&&tiempoTotal!==calc;
 const bloques=sel.map((d,i)=>`<div class="bloque-dinamica bd"><p><strong>Dinámica ${i+1}:</strong> ${esc(d.titulo)} <span class="idm">ID: ${esc(d.id)}</span></p>
  <p class="dynamics-list"><strong>Descripción:</strong> ${esc(limpio(d.desc))}</p>
  ${sinTiempo(d)?'':`<div class="cj t"><strong>⏱️ Tiempo límite:</strong> <b>${esc(d.tiempo_limite)}</b></div>`}
  <div class="cj o"><strong>🎯 Objetivo de la prueba:</strong><br>${nl(d.caso_o_consigna,'Sin objetivo específico.')}</div>
  <div class="cj g bloque-guia"><strong>🔍 Guía de observación para el evaluador:</strong><br>${nl(d.guia_evaluacion,'Sin guía específica.')}</div>
  <div class="cj c bloque-guia"><strong>✅ Corrector: respuestas correctas / esperadas:</strong><br>${nl(d.respuesta_esperada,'Sin respuesta esperada definida.')}</div></div>`).join('');
 const pm=sel.map(puntajeMaximoDinamica),cpd=sel.map(d=>new Set(aplica(d,car))),maxD=cpd.map((s,i)=>s.size*pm[i]),maxC=comps.map((c,j)=>cpd.reduce((a,s,i)=>a+(s.has(cod(c,j))?pm[i]:0),0)),maxT=maxD.reduce((a,b)=>a+b,0);
 const pesos=sel.map(d=>Number(d.peso)>0?Number(d.peso):1),pA=pesos.filter((_,i)=>maxD[i]>0),sumP=pA.reduce((a,b)=>a+b,0),iguales=pA.length>0&&pA.every(p=>p===pA[0]);
 const formula=maxT===0?'Sin competencias aplicables':iguales?`Total ÷ ${maxT} × ${MAXF}`:`Σ (Sub total ÷ máximo aplicable × peso) ÷ ${sumP} × ${MAXF}`;
 const leyenda=`<div class="leyenda-dinamicas bloque-dinamica"><strong>Competencias a observar</strong><ul>${comps.map((c,i)=>`<li><strong>${esc(cod(c,i))} - ${esc(c.comp)}:</strong> ${esc(limpio(c.desc))}</li>`).join('')}</ul></div>`;
 const celdaMax=m=>m?`<td class="celda-puntaje"><span class="max">/ ${m}</span></td>`:'<td class="celda-no-aplica"></td>';
 const filas=sel.map((d,i)=>`<tr><td class="celda-comp"><strong><span class="badge-d">D${i+1}</span>${esc(d.titulo)}</strong><span><span class="chip chip-id">${esc(d.id)}</span>${sinTiempo(d)?'':` <span class="chip chip-tiempo">⏱️ ${esc(d.tiempo_limite)}</span>`}${iguales?'':` <span class="chip chip-id">Peso ${pesos[i]}</span>`}${pm[i]!==PUNTAJE_MAX_COMPETENCIA?` <span class="chip chip-id">Máx. ${pm[i]} c/u</span>`:''}</span></td>${comps.map((c,j)=>cpd[i].has(cod(c,j))?`<td class="celda-puntaje"><span class="max">/ ${pm[i]}</span></td>`:'<td class="celda-no-aplica"></td>').join('')}<td class="celda-puntaje col-subtotal"><span class="max">${maxD[i]?`/ ${maxD[i]}`:'No aplica'}</span></td></tr>`).join('');
 const tabla=`<table class="tabla-puntaje"><thead><tr><th>Dinámica</th>${comps.map((c,i)=>`<th class="col-puntaje" title="${esc(c.comp)}">${esc(cod(c,i))}</th>`).join('')}<th class="col-subtotal">Sub total</th></tr></thead><tbody>${filas}
  <tr class="fila-total"><td>TOTAL</td>${maxC.map(celdaMax).join('')}<td class="celda-puntaje"><span class="max">/ ${maxT}</span></td></tr>
  <tr class="fila-total"><td>PUNTAJE FINAL</td><td colspan="${comps.length+1}" class="celda-puntaje celda-formula"><span class="max">${formula}${maxT?` = ______ / ${MAXF}`:''}</span></td></tr></tbody></table>`;
 let post='',hojas='';
 for(let i=1;i<=n;i++){post+=`<div class="candidate-box"><h4>Postulante #${i}: __________________________________________________</h4>${tabla}<div style="font-size:.75rem;font-weight:600;color:var(--secondary);margin-top:.5rem">Observaciones y notas de conducta del postulante:</div><div class="observations-box"></div></div>`;
  sel.forEach(d=>{hojas+=`<div class="candidate-sheet"><div class="pag">Hoja de trabajo · página nueva al imprimir</div><h3 class="ht">HOJA DE TRABAJO / CONSIGNA</h3>
   <div class="enc"><div style="display:flex;justify-content:space-between"><div style="flex:1;margin-right:1.25rem"><strong>Nombre del Postulante:</strong> ______________________________________</div><div><strong>Fecha:</strong> ____/____/20___</div></div>
   <div style="display:flex;justify-content:space-between;margin-top:.5rem;border-top:1px dashed var(--border);padding-top:.5rem"><div><strong>Dinámica ID:</strong> <span class="idm">${esc(d.id)}</span></div><div style="color:#8e44ad;font-weight:bold">${sinTiempo(d)?'':`⏱️ Tiempo Límite: ${esc(d.tiempo_limite)}`}</div></div></div>
   <div class="area-respuesta-postulante ${cantidadRenglonesDinamica(d)===0?'area-respuesta-libre':''}">${prepararHojaDinamica(d)||'Sin material específico definido.'}</div></div>`})}
 return `<div class="header-eval"><div><h2>INFORME DE EVALUACIÓN PSICOTÉCNICA</h2><div class="inf-linea"><span class="inf-n">${esc(meta.nombre||bonito(car.nombre))}</span><span class="inf-sep">-</span><span class="inf-c">Cargo Objetivo:</span><span class="idm">${esc(key)}</span>${meta.nombre?(meta.codigo?`<span class="inf-sep">·</span><span class="inf-c">Informe</span><span class="idm">${esc(meta.codigo)}</span>`:'<span class="inf-sep">·</span><em>sin guardar</em>'):''}</div></div><div class="meta-info"><strong>Fecha de evaluación:</strong> ${fmtFecha(meta.fecha)||'____/____/20___'}<br><strong>Evaluador(a):</strong> ________________________</div></div>
 <div class="section-title">1. DINÁMICAS DE EVALUACIÓN SELECCIONADAS</div>${bloques}${(multi||editado)&&total>0?`<div class="cj t" style="font-weight:bold">⏱️ Tiempo total estimado (${sel.length} dinámica${multi?'s':''}): ${total} minutos</div>`:''}
 <div class="section-title">2. EVALUACIÓN Y PUNTAJE POR POSTULANTE</div>${leyenda}${post}
 <div class="general-notes"><div class="section-title" style="margin-top:0">3. CONCLUSIÓN Y DICTAMEN GENERAL</div><div style="border:1px solid var(--border);height:5rem;border-radius:.25rem;padding:.5rem;font-size:.8125rem"><strong>Dictamen Global (Apto / Apto con reservas / No apto):</strong><br><br><strong>Comentarios finales:</strong></div></div>${hojas}`;
}

```

### Archivo: `js/interfaz.js`
```javascript
// Pantalla de armado. Depende de utilidades.js, datos/datos.js y js/informe.js (se cargan antes).
const $=s=>document.querySelector(s);
let D={areas:{},cargos:{},dinamicas:[]};
const S={area:null,cargo:null,sel:new Set(),n:2,guia:true,tManual:null,firma:'',nombreInf:'',fechaInf:null,clave:''};
const nombreArea=c=>(D.areas[c.area]&&D.areas[c.area].nombre)||c.area;
const deArea=(c,sigla)=>c.area===sigla||c.area===(D.areas[sigla]&&D.areas[sigla].nombre);
// Bajas lógicas: lo que tiene "activo": false no se muestra (un área o cargo inactivo oculta lo que cuelga de él).
const act=o=>o.activo!==false;
const dinDe=id=>D.dinamicas.filter(d=>d.cargo_id===id&&act(d));

function pintaAreas(){const cnt=k=>Object.values(D.cargos).filter(c=>act(c)&&deArea(c,k)).length;
 $('#areas').innerHTML=Object.entries(D.areas).filter(([,a])=>act(a)).map(([k,a])=>`<button class="area" aria-pressed="${k===S.area}" data-a="${esc(k)}"><strong>${esc(a.nombre)}</strong><small>${cnt(k)} cargo${cnt(k)===1?'':'s'}</small></button>`).join('')}
function pintaCargos(){const el=$('#cargos');
 if(!S.area){el.innerHTML='<div class="vacio-box">Primero elige un área.</div>';return}
 el.innerHTML=Object.entries(D.cargos).filter(([k,c])=>act(c)&&deArea(c,S.area)).map(([k,c])=>`<button class="cargo" aria-pressed="${k===S.cargo}" data-c="${k}"><strong>${esc(bonito(c.nombre))}</strong><small>${dinDe(k).length} dinámicas · ${c.competencias.length} competencias</small><span class="cod">${k}</span></button>`).join('')||'<div class="vacio-box">Esta área no tiene cargos activos.</div>'}
function pintaDin(){const car=D.cargos[S.cargo];
 $('#p2').classList.toggle('off',!S.area);$('#p3').classList.toggle('off',!S.cargo);$('#cab').hidden=!car;
 if(!car){$('#lista').innerHTML=`<div class="vacio-box" style="grid-column:1/-1"><b>${S.area?'Ahora elige un cargo':'Empieza eligiendo un área'}</b><br>Las dinámicas disponibles aparecerán acá.</div>`;return}
 const l=dinDe(S.cargo);
 $('#tCargo').textContent=bonito(car.nombre);$('#sCargo').textContent=`${nombreArea(car)} · ${l.length} dinámicas disponibles`;
 $('#todas').textContent=l.every(d=>S.sel.has(d.id))?'Quitar todas':'Seleccionar todas';
 $('#lista').innerHTML=l.map(d=>`<label class="din ${S.sel.has(d.id)?'on':''}"><input type="checkbox" data-d="${d.id}" ${S.sel.has(d.id)?'checked':''}><span class="tilde">✓</span><h3>${esc(d.titulo)}</h3><p>${esc(limpio(d.desc))}</p><div class="meta">${mins(d)?`<span class="tag">⏱ ${mins(d)} min</span>`:''}<span class="tag id">${d.id.replace('DIN-','')}</span><button class="det" data-v="${d.id}">Ver detalle</button></div></label>`).join('')||'<div class="vacio-box" style="grid-column:1/-1">Este cargo todavía no tiene dinámicas activas.</div>'}
function pintaPanel(){const car=D.cargos[S.cargo],sel=car?dinDe(S.cargo).filter(d=>S.sel.has(d.id)):[];
 $('#pCargo').textContent=car?bonito(car.nombre):'Tu evaluación';
 $('#pSub').textContent=car?`${nombreArea(car)} · tu evaluación`:(S.area?'Ahora elige un cargo.':'Empieza eligiendo un área.');$('#n').textContent=S.n;
 $('#selLista').innerHTML=sel.length?sel.map(d=>`<li><span>${esc(d.titulo)}</span><span>${mins(d)?mins(d)+' min':''}</span></li>`).join(''):'<li class="vacio">Aún no elegiste dinámicas.</li>';
 const cubre=new Set(car?sel.flatMap(d=>aplica(d,car)):[]),tot=car?car.competencias.length:0;
 $('#cob').innerHTML=car?car.competencias.map((c,i)=>`<i class="${cubre.has(cod(c,i))?'ok':''}" title="${esc(c.comp)}">${esc(cod(c,i))}</i>`).join(''):'';
 $('#cobTxt').textContent=car?`${cubre.size} de ${tot}`:'';$('#cobBar').style.width=tot?(100*cubre.size/tot)+'%':'0';
 const sinC=car?sel.filter(d=>competenciasDeDinamica(d,car).length===0):[],av=$('#avRel');
 av.innerHTML=sinC.length?`<p>⚠ Sin competencia relacionada: <b>${sinC.map(d=>esc(d.titulo)).join(', ')}</b>.</p>`:'';av.hidden=!sinC.length;
 // Tiempo estimado: se ofrece la suma de las dinámicas, pero se puede editar. Al cambiar la selección vuelve a ofrecerse el valor calculado.
 const firma=[...S.sel].sort().join();if(firma!==S.firma){S.firma=firma;S.tManual=null}
 const t=sel.reduce((a,d)=>a+mins(d),0),inp=$('#tiempoIn');inp.disabled=!sel.length;
 if(!sel.length){inp.value='';S.tManual=null}else if(S.tManual===null)inp.value=t;
 $('#tReset').hidden=!sel.length||S.tManual===null||S.tManual===t;$('#tReset').title=`Volver al valor calculado (${t} min)`;
 $('#gen').disabled=!sel.length}
const tiempoEfectivo=()=>{const sel=dinDe(S.cargo).filter(d=>S.sel.has(d.id));return S.tManual===null?sel.reduce((a,d)=>a+mins(d),0):S.tManual};
const todo=()=>{pintaAreas();pintaCargos();pintaDin();pintaPanel()};

$('#areas').onclick=e=>{const b=e.target.closest('[data-a]');if(!b)return;S.area=b.dataset.a;S.cargo=null;S.sel=new Set();todo()};
$('#cargos').onclick=e=>{const b=e.target.closest('[data-c]');if(!b)return;S.cargo=b.dataset.c;S.sel=new Set();todo()};
$('#lista').onclick=e=>{const v=e.target.closest('[data-v]');if(v){e.preventDefault();abre(v.dataset.v)}};
$('#lista').onchange=e=>{const id=e.target.dataset.d;e.target.checked?S.sel.add(id):S.sel.delete(id);pintaDin();pintaPanel()};
$('#todas').onclick=()=>{const l=dinDe(S.cargo),t=l.every(d=>S.sel.has(d.id));l.forEach(d=>t?S.sel.delete(d.id):S.sel.add(d.id));pintaDin();pintaPanel()};
$('#tiempoIn').oninput=e=>{const v=e.target.value;S.tManual=v===''?null:Math.min(999,Math.max(0,parseInt(v,10)||0));const sel=dinDe(S.cargo).filter(d=>S.sel.has(d.id)),t=sel.reduce((a,d)=>a+mins(d),0);$('#tReset').hidden=S.tManual===null||S.tManual===t};
$('#tiempoIn').onblur=()=>{if($('#tiempoIn').value===''){S.tManual=null;pintaPanel()}};
$('#tReset').onclick=()=>{S.tManual=null;pintaPanel()};
$('#menos').onclick=()=>{S.n=Math.max(1,S.n-1);pintaPanel()};$('#mas').onclick=()=>{S.n=Math.min(10,S.n+1);pintaPanel()};
const sincGuia=v=>{S.guia=v;$('#guia').checked=v;$('#guia2').checked=v;$('#hoja').classList.toggle('ocultar-guia',!v)};
$('#guia').onchange=e=>sincGuia(e.target.checked);$('#guia2').onchange=e=>sincGuia(e.target.checked);
document.querySelectorAll('[data-cerrar]').forEach(b=>b.onclick=()=>b.closest('dialog').close());
document.querySelectorAll('dialog').forEach(d=>d.addEventListener('click',e=>{if(e.target===d)d.close()}));
$('#tema').onclick=()=>{const r=document.documentElement,os=matchMedia('(prefers-color-scheme: dark)').matches;
 r.dataset.theme=(r.dataset.theme||(os?'dark':'light'))==='dark'?'light':'dark'};

function abre(id){const d=D.dinamicas.find(x=>x.id===id),en=S.sel.has(id);
 $('#dId').textContent=d.id;$('#dTit').textContent=d.titulo;
 $('#dCuerpo').innerHTML=`<p style="color:var(--suave);margin:0 0 4px">${esc(limpio(d.desc))} ${sinTiempo(d)?'':`<b>⏱ ${esc(d.tiempo_limite)}</b>`}</p>
 <div class="bq"><b>Objetivo de la prueba</b>${esc(limpio(d.caso_o_consigna))}</div>
 <div class="bq"><b>Guía de observación</b>${esc(limpio(d.guia_evaluacion))}</div>
 <div class="bq av"><b>Corrector · toca para ver</b><span class="cor" onclick="this.classList.toggle('ver')">${esc(limpio(d.respuesta_esperada))}</span></div>`;
 const b=$('#dAdd');b.textContent=en?'Quitar de la evaluación':'Agregar a la evaluación';
 b.onclick=()=>{en?S.sel.delete(id):S.sel.add(id);pintaDin();pintaPanel();$('#dDet').close()};$('#dDet').showModal()}

// Al imprimir (botón o Ctrl+P) el informe se copia fuera del modal: imprimir desde el diálogo dejaba una hoja en blanco y perdía el informe.
const preparaImpresion=()=>{const z=$('#zonaImpresion');z.className='print-container'+(S.guia?'':' ocultar-guia');z.innerHTML=$('#hoja').innerHTML};
window.addEventListener('beforeprint',()=>{if($('#dInf').open)preparaImpresion()});
window.addEventListener('afterprint',()=>{$('#zonaImpresion').innerHTML=''});
// ---------- Guardar el informe (instantánea completa) y reabrirlo por código ----------
const norm=t=>String(t||'').normalize('NFD').replace(/[\u0300-\u036f]/g,'').toLowerCase();
const hoy=()=>new Date(Date.now()-new Date().getTimezoneOffset()*60000).toISOString().slice(0,10);
const fecha=f=>fmtFecha(f)||'';
const snapshot=()=>{const car=D.cargos[S.cargo],ar=D.areas[S.area]||{nombre:car.area},sel=dinDe(S.cargo).filter(d=>S.sel.has(d.id));
 return JSON.parse(JSON.stringify({nombre:S.nombreInf||bonito(car.nombre),fecha_evaluacion:S.fechaInf,postulantes:S.n,tiempo_total:tiempoEfectivo(),guia:S.guia,area:{sigla:S.area,nombre:ar.nombre},cargo:{codigo:S.cargo,...car},dinamicas:sel}))};
function mostrarInforme(snap,est={}){
 $('#hoja').innerHTML=construirInforme(snap.cargo.codigo,snap.cargo,snap.dinamicas,snap.postulantes,snap.tiempo_total,{codigo:est.codigo,nombre:snap.nombre,fecha:snap.fecha_evaluacion});sincGuia(snap.guia);
 $('#subInf').innerHTML=est.codigo?(est.reabierto?`Informe guardado <b class="cod-g">${esc(est.codigo)}</b>, tal como se generó. Así saldrá en hoja A4.`:`Guardado como <b class="cod-g">${esc(est.codigo)}</b>${est.actualizado?' (se actualizó el informe existente)':''}. Así saldrá en hoja A4.`)
  :`<span class="av-txt">⚠ No se pudo guardar: ${esc(est.error||'error desconocido')}.</span> <button type="button" class="link" id="reintGuardar">Reintentar</button>`;
 const b=$('#reintGuardar');if(b)b.onclick=()=>reintentar(snap);
 if(!$('#dInf').open)$('#dInf').showModal()}
async function reintentar(snap){const b=$('#reintGuardar');b.disabled=true;b.textContent='Guardando…';
 try{const r=await Datos.guardarInforme(snap,{clave:S.clave});mostrarInforme(snap,{codigo:r.codigo,actualizado:r.actualizado})}
 catch(x){if(x.status===401||x.status===409){$('#dInf').close();abreGuardar()}else mostrarInforme(snap,{error:x.message})}}
const dg=$('#dGuardar'),fg=$('#fGuardar');
function abreGuardar(){fg.nombre.value=S.nombreInf;fg.nombre.placeholder=`Si lo dejas vacío: ${bonito(D.cargos[S.cargo].nombre)}`;fg.fecha.value=S.fechaInf||hoy();fg.clave.value='';$('#gClave').hidden=true;$('#gDup').hidden=true;$('#gErr').textContent='';
 const av=avisosRelaciones(D.cargos[S.cargo],dinDe(S.cargo).filter(d=>S.sel.has(d.id))),gr=$('#gRel');
 gr.innerHTML=av.length?av.map(t=>`<p style="margin:0 0 6px">⚠ ${t}</p>`).join('')+'<p style="margin:0">Puedes generar el informe igual.</p>':'';gr.hidden=!av.length;
 dg.showModal();fg.nombre.focus()}
async function enviar(reemplazar){
 S.nombreInf=fg.nombre.value.trim();S.fechaInf=fg.fecha.value;if(!S.fechaInf)return;
 if(!$('#gClave').hidden)S.clave=fg.clave.value;
 const snap=snapshot(),ok=$('#gOk');ok.disabled=true;ok.textContent='Guardando…';$('#gErr').textContent='';$('#gDup').hidden=true;
 try{const r=await Datos.guardarInforme(snap,{clave:S.clave,reemplazar});dg.close();mostrarInforme(snap,{codigo:r.codigo,actualizado:r.actualizado})}
 catch(x){
  if(x.status===409&&x.detalle&&x.detalle.error==='nombre_existe'){const g=$('#gDup');
   g.innerHTML=`Ya existe el informe <b>${esc(x.detalle.nombre)}</b> (<span class="cod-g">${esc(x.detalle.codigo)}</span>). Si lo actualizas, conserva su código y se reemplaza su contenido.<br><button type="button" class="btn" id="gAct">Actualizar ${esc(x.detalle.codigo)}</button><button type="button" class="btn sec" id="gCamb">Cambiar el nombre</button>`;g.hidden=false;
   $('#gAct').onclick=()=>enviar(true);$('#gCamb').onclick=()=>{g.hidden=true;fg.nombre.focus();fg.nombre.select()}}
  else if(x.status===401){$('#gClave').hidden=false;$('#gErr').textContent=S.clave?'La contraseña no es correcta.':'Para guardar hace falta la contraseña.';fg.clave.focus()}
  else{dg.close();mostrarInforme(snap,{error:x.message})}}
 finally{ok.disabled=false;ok.textContent='Generar y guardar'}}
fg.onsubmit=e=>{e.preventDefault();enviar(false)};
$('#gen').onclick=()=>{const car=D.cargos[S.cargo],sel=dinDe(S.cargo).filter(d=>S.sel.has(d.id));if(!car||!sel.length)return;abreGuardar()};

let INFS=[];
function pintaInfs(){const q=norm($('#infBus').value),l=INFS.filter(x=>!q||norm([x.nombre,x.codigo,x.cargo_nombre,x.area_nombre].join(' ')).includes(q));
 $('#infLista').innerHTML=l.map(x=>`<button type="button" class="inf-item" data-c="${esc(x.codigo)}"><span><strong>${esc(x.nombre)}</strong><small>${esc(x.cargo_nombre)} · ${esc(x.area_nombre)} · ${esc(fecha(x.fecha_evaluacion))} · ${x.postulantes} postulante${x.postulantes>1?'s':''}</small></span><span class="idm-s">${esc(x.codigo)}</span></button>`).join('')||`<div class="vacio-box">${INFS.length?'Ningún informe coincide con la búsqueda.':'Todavía no hay informes guardados.'}</div>`}
async function abreInformes(){$('#infErr').textContent='';$('#infCod').value='';$('#infBus').value='';$('#infLista').innerHTML='<div class="vacio-box">Cargando…</div>';$('#dInfs').showModal();
 try{INFS=(await Datos.listarInformes()).slice().sort((a,b)=>String(b.actualizado).localeCompare(String(a.actualizado)));pintaInfs()}catch(x){$('#infLista').innerHTML='';$('#infErr').textContent=x.message}}
async function abreGuardado(codigo){const c=String(codigo).trim().toUpperCase();if(!c)return;$('#infErr').textContent='';
 try{const r=await Datos.leerInforme(c);$('#dInfs').close();mostrarInforme(r,{codigo:r.codigo,reabierto:true})}
 catch(x){$('#infErr').textContent=x.status===404?`No existe el informe ${c}.`:x.message}}
$('#bInformes').onclick=abreInformes;$('#infBus').oninput=pintaInfs;
$('#infAbrir').onclick=()=>abreGuardado($('#infCod').value);$('#infCod').onkeydown=e=>{if(e.key==='Enter'){e.preventDefault();abreGuardado(e.target.value)}};
$('#infLista').onclick=e=>{const b=e.target.closest('[data-c]');if(b)abreGuardado(b.dataset.c)};

async function iniciar(){
 $('#areas').innerHTML='<div class="vacio-box" style="grid-column:1/-1">Cargando catálogo…</div>';
 try{D=await Datos.cargar();todo()}
 catch(e){$('#areas').innerHTML=`<div class="vacio-box" style="grid-column:1/-1"><b>No se pudo cargar el catálogo</b><br>${esc(e.message)}<br><button class="btn sec" id="reintentar" style="margin-top:12px">Reintentar</button></div>`;$('#reintentar').onclick=iniciar}}
iniciar();

```

### Archivo: `js/utilidades.js`
```javascript
// Utilidades compartidas por index.html y admin.html
const RENGLONES_HOJA_GLOBAL = 5;
const REGEX_RENGLONES_FINALES = /(?:<div class=['"]renglon-respuesta['"]><\/div>)+\s*$/;

function separarRenglones(html) {
    const final = html.match(REGEX_RENGLONES_FINALES);
    const renglones = final ? (final[0].match(/<div/g) || []).length : 0;
    const texto = html.replace(REGEX_RENGLONES_FINALES, '').replace(/&nbsp;/g, '\u00a0').trim();
    return { texto, renglones };
}

function normalizarCantidadRenglones(valor) {
    const parsed = Number.parseInt(valor, 10);
    return Math.min(Math.max(Number.isNaN(parsed) ? RENGLONES_HOJA_GLOBAL : parsed, 0), 30);
}

function componerHoja(texto, renglones) {
    const cuerpo = texto.replace(/\u00a0/g, '&nbsp;').trim();
    const cantidad = normalizarCantidadRenglones(renglones);
    return cantidad > 0
        ? `${cuerpo}\n\n${"<div class='renglon-respuesta'></div>".repeat(cantidad)}`
        : cuerpo;
}

function cantidadRenglonesDinamica(dinamica) {
    const particular = dinamica.renglones_hoja;
    const cantidad = particular === undefined || particular === null || particular === ''
        ? RENGLONES_HOJA_GLOBAL
        : particular;
    return normalizarCantidadRenglones(cantidad);
}

function prepararHojaDinamica(dinamica) {
    const { texto } = separarRenglones(dinamica.hoja_postulante || '');
    return componerHoja(texto, cantidadRenglonesDinamica(dinamica));
}

// Escapa texto para insertarlo de forma segura dentro de innerHTML o de atributos HTML.
function esc(valor) {
    const mapa = { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' };
    return String(valor ?? '').replace(/[&<>"']/g, (c) => mapa[c]);
}

// Muestra "JEFE DE SUCURSAL" como "Jefe de sucursal"; los nombres que ya tienen minúsculas se dejan como están.
function bonito(nombre) {
    const n = String(nombre ?? '');
    return n === n.toUpperCase() && n !== n.toLowerCase() ? n.charAt(0) + n.slice(1).toLowerCase() : n;
}

// ---- Puntaje máximo por competencia ----
// Valor general: cada competencia evaluada en una dinámica vale hasta 5 puntos.
// Una dinámica puede traer su propio "puntaje_maximo"; si no lo trae, vale el general.
const PUNTAJE_MAX_COMPETENCIA = 5;

function puntajeMaximoDinamica(dinamica) {
    const n = Number(dinamica && dinamica.puntaje_maximo);
    return Number.isFinite(n) && n > 0 ? n : PUNTAJE_MAX_COMPETENCIA;
}

// ---- Relación competencias ↔ dinámicas ----
// Cada competencia lleva un código propio dentro de su cargo (único dentro del cargo).
// Cada dinámica guarda en "competencias" los códigos que evalúa.
const codigoCompetencia = (c, i) => c.codigo || 'C' + String(i + 1).padStart(2, '0');

// Códigos escritos a mano en un texto ("Evalúa 6.1 y 6.3"): así se relacionaban antes de existir el campo "competencias".
const codigosCitados = texto => [...String(texto || '').matchAll(/\b\d+\.\d+\b/g)].map(m => m[0]);

// Códigos (en el orden del cargo) de las competencias que evalúa una dinámica.
function competenciasDeDinamica(dinamica, cargo) {
    const codigos = cargo.competencias.map(codigoCompetencia);
    if (Array.isArray(dinamica.competencias)) return codigos.filter(c => dinamica.competencias.includes(c));
    // Datos e informes guardados antes del campo "competencias": se leían del texto de la dinámica (sin códigos citados = todas).
    const citados = codigosCitados(dinamica.caso_o_consigna);
    return codigos.filter(c => !citados.length || citados.includes(c));
}

// Dinámicas sin ninguna competencia relacionada y competencias que ninguna de esas dinámicas evalúa.
function controlRelaciones(cargo, dinamicas) {
    const sinCompetencia = dinamicas.filter(d => competenciasDeDinamica(d, cargo).length === 0);
    const cubiertas = new Set(dinamicas.flatMap(d => competenciasDeDinamica(d, cargo)));
    const sinCubrir = cargo.competencias.filter((c, i) => !cubiertas.has(codigoCompetencia(c, i)));
    return { sinCompetencia, sinCubrir };
}

// Frases (HTML ya escapado) para avisar del control anterior. Lista vacía = todo relacionado.
function avisosRelaciones(cargo, dinamicas) {
    const { sinCompetencia, sinCubrir } = controlRelaciones(cargo, dinamicas), avisos = [];
    if (sinCompetencia.length) avisos.push(`Dinámicas sin competencia relacionada: <b>${sinCompetencia.map(d => `${esc(d.titulo)} (${esc(d.id.replace('DIN-', ''))})`).join(', ')}</b>.`);
    if (sinCubrir.length) avisos.push(`Competencias que ninguna dinámica evalúa: <b>${sinCubrir.map(c => `${esc(codigoCompetencia(c, cargo.competencias.indexOf(c)))} ${esc(c.comp)}`).join(', ')}</b>.`);
    return avisos;
}

```

### Archivo: `netlify.toml`
```toml
# La carpeta documentacion/ es interna: se responde 404 aunque el archivo exista.
[[redirects]]
  from = "/documentacion/*"
  to = "/404.html"
  status = 404
  force = true

```

### Archivo: `netlify/functions/pruebas.js`
```javascript
// Interfaz con los JSON de bd/pruebas_llamados (areas, cargos y dinamicas).
// Variables de entorno en Netlify: GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD y CLAVE_PRUEBAS.
const crypto = require('crypto');
const REPO = 'FABIOR1981/bd';
const RUTA = 'pruebas_llamados';
const RAMA = 'main';
const ARCHIVOS = ['areas', 'cargos', 'dinamicas'];
const INDICE = 'informes'; // informes.json: índice liviano; cada informe completo vive en informes/<código>.json

const cab = (conToken = false) => ({
  Accept: 'application/vnd.github+json',
  'User-Agent': 'gestor-pruebas',
  ...(conToken ? { Authorization: 'Bearer ' + process.env.GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD } : {})
});
const resp = (c, o) => ({ statusCode: c, headers: { 'Content-Type': 'application/json', 'Cache-Control': 'no-store' }, body: JSON.stringify(o) });
const igual = (a, b) => { const x = Buffer.from(String(a)), y = Buffer.from(String(b)); return x.length === y.length && crypto.timingSafeEqual(x, y); };
// La contraseña (CLAVE_PRUEBAS) se exige para leer y para guardar. Sin la variable configurada en Netlify, todo queda bloqueado.
const claveOk = c => Boolean(process.env.CLAVE_PRUEBAS) && igual(c || '', process.env.CLAVE_PRUEBAS);
const urlArchivo = n => `https://api.github.com/repos/${REPO}/contents/${RUTA}/${n}.json`;
const esObjeto = x => x !== null && typeof x === 'object' && !Array.isArray(x);
const baseValida = b => esObjeto(b) && esObjeto(b.areas) && esObjeto(b.cargos) && Array.isArray(b.dinamicas);

async function leerArchivo(nombre, conToken = false) {
  const r = await fetch(`${urlArchivo(nombre)}?ref=${RAMA}`, { headers: cab(conToken) });
  if (r.status === 404) return { falta: true };
  if (r.status === 401) { console.error('GitHub rechazó el token al leer ' + nombre + '.json: revisar GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD y su acceso al repo'); throw new Error('No se pudo acceder a los datos (error de configuración del servidor).'); }
  if (!r.ok) { console.error('GitHub respondió ' + r.status + ' al leer ' + nombre + '.json'); throw new Error('No se pudo leer ' + nombre + '.json (error ' + r.status + ').'); }
  const m = await r.json();
  return { sha: m.sha, datos: JSON.parse(Buffer.from(m.content, 'base64').toString('utf8')) };
}

async function leerTodo() {
  const partes = await Promise.all(ARCHIVOS.map(nombre => leerArchivo(nombre, Boolean(process.env.GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD))));
  if (partes.some(p => p.falta)) return resp(404, { error: 'Faltan archivos en la base de datos.' });
  return resp(200, Object.fromEntries(ARCHIVOS.map((n, i) => [n, partes[i].datos])));
}

async function guardarTodo(base) {
  const escritos = [];
  for (const nombre of ARCHIVOS) {
    const actual = await leerArchivo(nombre, true);
    const contenido = JSON.stringify(base[nombre], null, 4) + '\n';
    if (!actual.falta && JSON.stringify(actual.datos) === JSON.stringify(base[nombre])) continue;
    const put = await fetch(urlArchivo(nombre), {
      method: 'PUT', headers: { ...cab(true), 'Content-Type': 'application/json' },
      body: JSON.stringify({ message: `pruebas_llamados: actualizar ${nombre}.json`, content: Buffer.from(contenido).toString('base64'), branch: RAMA, ...(actual.falta ? {} : { sha: actual.sha }) })
    });
    if (!put.ok) { console.error(`GitHub ${put.status} al guardar ${nombre}.json`); throw new Error(`No se pudo guardar ${nombre}.json (error ${put.status}).`); }
    escritos.push(nombre);
  }
  return resp(200, { ok: true, escritos });
}

// ---------- Informes guardados ----------
const norm = s => String(s || '').normalize('NFD').replace(/[\u0300-\u036f]/g, '').toLowerCase().replace(/\s+/g, ' ').trim();
const codigoOk = c => /^INF-\d{4}-\d{3,}$/.test(String(c));
const fechaOk = f => /^\d{4}-\d{2}-\d{2}$/.test(f) && !isNaN(Date.parse(f + 'T00:00:00Z'));

async function escribir(nombre, datos, sha, mensaje) {
  return fetch(urlArchivo(nombre), {
    method: 'PUT', headers: { ...cab(true), 'Content-Type': 'application/json' },
    body: JSON.stringify({ message: mensaje, content: Buffer.from(JSON.stringify(datos, null, 4) + '\n').toString('base64'), branch: RAMA, ...(sha ? { sha } : {}) })
  });
}

function validarInforme(i) {
  if (!esObjeto(i)) return 'Informe inválido';
  if (typeof i.nombre !== 'string' || !i.nombre.trim() || i.nombre.trim().length > 80) return 'El nombre del informe es obligatorio (hasta 80 caracteres)';
  if (typeof i.fecha_evaluacion !== 'string' || !fechaOk(i.fecha_evaluacion)) return 'La fecha de evaluación no es válida';
  if (!Number.isInteger(i.postulantes) || i.postulantes < 1 || i.postulantes > 10) return 'La cantidad de postulantes debe ser de 1 a 10';
  if (!esObjeto(i.area) || typeof i.area.sigla !== 'string' || typeof i.area.nombre !== 'string') return 'Falta el área del informe';
  if (!esObjeto(i.cargo) || typeof i.cargo.codigo !== 'string' || typeof i.cargo.nombre !== 'string' || !Array.isArray(i.cargo.competencias)) return 'Falta el cargo del informe';
  if (!Array.isArray(i.dinamicas) || !i.dinamicas.length || i.dinamicas.length > 30 || i.dinamicas.some(d => !esObjeto(d) || typeof d.id !== 'string' || typeof d.titulo !== 'string')) return 'Las dinámicas del informe no son válidas';
  if (i.tiempo_total !== null && !(Number.isFinite(i.tiempo_total) && i.tiempo_total >= 0 && i.tiempo_total <= 999)) return 'El tiempo total no es válido';
  if (typeof i.guia !== 'boolean') return 'Falta indicar si el informe lleva guía y corrector';
  if (JSON.stringify(i).length > 300000) return 'El informe es demasiado grande';
  return null;
}

async function listarInformes() {
  const a = await leerArchivo(INDICE, Boolean(process.env.GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD));
  return resp(200, a.falta || !Array.isArray(a.datos) ? [] : a.datos);
}

async function leerInforme(codigo) {
  if (!codigoOk(codigo)) return resp(400, { error: 'Código de informe inválido' });
  const a = await leerArchivo('informes/' + codigo, Boolean(process.env.GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD));
  if (a.falta) return resp(404, { error: `No existe el informe ${codigo}` });
  return resp(200, a.datos);
}

// El nombre identifica al informe: mismo nombre = mismo informe (conserva el código); otro nombre = informe nuevo.
async function guardarInforme(inf, reemplazar) {
  inf.nombre = inf.nombre.trim();
  for (let intento = 0; intento < 6; intento++) {
    const idx = await leerArchivo(INDICE, true);
    const lista = idx.falta ? [] : idx.datos;
    if (!Array.isArray(lista)) throw new Error('informes.json tiene un formato inválido');
    const existente = lista.find(x => norm(x.nombre) === norm(inf.nombre));
    if (existente && !reemplazar) return resp(409, { error: 'nombre_existe', codigo: existente.codigo, nombre: existente.nombre, mensaje: `Ya existe el informe "${existente.nombre}" (${existente.codigo}).` });
    const ahora = new Date().toISOString();
    const prefijo = `INF-${ahora.slice(0, 4)}-`;
    const mayor = Math.max(0, ...lista.filter(x => String(x.codigo).startsWith(prefijo)).map(x => parseInt(String(x.codigo).slice(prefijo.length), 10) || 0));
    const codigo = existente ? existente.codigo : prefijo + String(mayor + 1).padStart(3, '0');
    const creado = existente ? existente.creado : ahora;
    const registro = { ...inf, codigo, creado, actualizado: ahora };
    const entrada = { codigo, nombre: inf.nombre, fecha_evaluacion: inf.fecha_evaluacion, creado, actualizado: ahora, area: inf.area.sigla, area_nombre: inf.area.nombre, cargo: inf.cargo.codigo, cargo_nombre: inf.cargo.nombre, dinamicas: inf.dinamicas.map(d => d.id), postulantes: inf.postulantes };
    const previo = await leerArchivo('informes/' + codigo, true);
    const p1 = await escribir('informes/' + codigo, registro, previo.falta ? null : previo.sha, `pruebas_llamados: informe ${codigo}`);
    if (!p1.ok) {
      // 409/422: otro guardado simultáneo ocupó ese código. Se espera un instante y se vuelve a calcular.
      if ([409, 422].includes(p1.status) && intento < 5) { await new Promise(r => setTimeout(r, 120 * (intento + 1))); continue; }
      { console.error(`GitHub ${p1.status} al guardar el informe ${codigo}`); throw new Error(`No se pudo guardar el informe ${codigo} (error ${p1.status}).`); }
    }
    const nueva = existente ? lista.map(x => x.codigo === codigo ? entrada : x) : [...lista, entrada];
    const p2 = await escribir(INDICE, nueva, idx.falta ? null : idx.sha, `pruebas_llamados: índice de informes (${codigo})`);
    if (p2.ok) return resp(200, { ok: true, codigo, actualizado: Boolean(existente) });
    if (![409, 422].includes(p2.status)) { console.error(`GitHub ${p2.status} al actualizar informes.json`); throw new Error(`No se pudo actualizar el índice de informes (error ${p2.status}).`); }
    await new Promise(r => setTimeout(r, 120 * (intento + 1)));
  }
  throw new Error('No se pudo guardar: otro guardado simultáneo ocupó el índice. Intente de nuevo.');
}

exports.handler = async (ev) => {
  try {
    if (!process.env.CLAVE_PRUEBAS) return resp(500, { error: 'El servidor no tiene configurada la contraseña.' });
    if (ev.httpMethod === 'GET') {
      if (!claveOk((ev.headers || {})['x-clave'])) return resp(401, { error: 'Clave inválida' });
      const q = ev.queryStringParameters || {};
      if (q.informes) return await listarInformes();
      if (q.informe) return await leerInforme(q.informe);
      return await leerTodo();
    }
    if (ev.httpMethod !== 'POST') return resp(405, { error: 'Método no permitido' });
    if (!process.env.GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD) return resp(500, { error: 'El servidor no tiene configurado el guardado.' });

    const cuerpo = JSON.parse(ev.body || '{}');
    if (!claveOk(cuerpo.clave)) return resp(401, { error: 'Clave inválida' });
    if (cuerpo.accion === 'guardar_informe') {
      const error = validarInforme(cuerpo.informe);
      if (error) return resp(400, { error });
      return await guardarInforme(cuerpo.informe, Boolean(cuerpo.reemplazar));
    }
    if (!baseValida(cuerpo.base)) return resp(400, { error: 'Datos inválidos' });
    return await guardarTodo(cuerpo.base);
  } catch (e) {
    console.error(e);
    return resp(500, { error: e.message || 'Error del servidor' });
  }
};

```

### Archivo: `readme.md`
```md
# Gestor de Pruebas para Llamados

Aplicación web para armar e imprimir **evaluaciones psicotécnicas** en procesos de selección (llamados). Se elige el cargo, la cantidad de postulantes y las dinámicas, y la app genera:

- el **informe para el evaluador**, con la descripción de cada dinámica, el tiempo total, la guía de observación y las respuestas esperadas;
- la **tabla de puntaje** por competencia (escala 0 a 5), con el dictamen de cada postulante;
- las **hojas de trabajo** de cada postulante, que al imprimir empiezan en página nueva.

![Generador de evaluaciones](https://raw.githubusercontent.com/FABIOR1981/documentacion-central/main/gestor_pruebas_llamados/documentacion/img/02_seleccion.png)

## Funcionalidades

| Pantalla | Para qué sirve |
|---|---|
| **Generador** (`index.html`) | Armar la evaluación, mostrar u ocultar la guía y el corrector, e imprimir en A4. |
| **Panel de Administración** (`admin.html`) | Núcleo del sistema: crear áreas y cargos, y **cargar a mano** las dinámicas de cada cargo (el código se genera solo según área y cargo; se puede duplicar una dinámica existente como base o crear varias seguidas). Incluye el **Asistente IA (opcional)**: lee las bases del llamado (`.docx`), anonimiza los datos sensibles y genera un *prompt* para que una IA proponga dinámicas. El archivo se procesa en el navegador y no se sube a ningún servidor. |

Catálogo actual en `FABIOR1981/bd/pruebas_llamados`: 4 áreas, 6 cargos y 25 dinámicas. El repositorio `bd` es la fuente de verdad; esta cifra puede cambiar cuando se actualicen esos JSON.

## Documentación

- 📘 [Manual de Usuario](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor_pruebas_llamados/documentacion/MANUAL_USUARIO.md) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor_pruebas_llamados/documentacion/Manual_de_Usuario.pdf)): uso de las pantallas, para evaluadores y administradores.
- 🧠 [Propuesta para profesionales de la Psicología](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor_pruebas_llamados/documentacion/PROPUESTA_PSICOLOGOS.md) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor_pruebas_llamados/documentacion/Propuesta_para_Psicologos.pdf)): presentación de la herramienta para psicólogos, con énfasis en la confidencialidad de las bases.
- 🛠️ [Manual Técnico](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor_pruebas_llamados/documentacion/MANUAL_TECNICO.md) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/gestor_pruebas_llamados/documentacion/Manual_Tecnico.pdf)): funcionamiento, modelo de datos, instalación y mantenimiento.

## Ejecución local

El frontend es HTML, CSS y JavaScript sin paso de *build*. Para que funcione, también debe estar disponible la función de Netlify `/.netlify/functions/pruebas`: servir los archivos con un servidor HTTP estático no basta. Para desarrollo local, instala Node.js y ejecuta Netlify CLI desde la raíz del proyecto:

```bash
npx netlify dev
```

Abre la URL local que indique Netlify CLI. Para leer desde un repositorio `bd` privado, configura `GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD` en el entorno local; para probar guardados, configura también `CLAVE_PRUEBAS`. No guardes secretos en el repositorio.

## Arquitectura para desarrollo

- `index.html` + `script.js` + `style.css`: generador de formularios e impresión. `script.js` obtiene el catálogo, filtra dinámicas por `cargo_id`, genera informes, hojas por postulante y puntajes.
- `admin.html` + `admin.js` + `admin.css`: gestión en memoria de áreas, cargos, competencias y dinámicas. Los cambios solo persisten al usar **Guardar en GitHub**.
- `bases_ia.js`: añade al panel el asistente opcional. Lee `.docx` en el navegador con Mammoth (cargado desde CDN), permite revisar el resumen y arma un *prompt*; no invoca un modelo ni guarda datos. La anonimización automática no garantiza eliminar todos los datos sensibles: hay que revisar el texto antes de usarlo.
- `utilidades.js`: funciones compartidas para escape HTML y composición de hojas y renglones de respuesta.
- `datos/datos.js`: única capa cliente de acceso a datos; expone `Datos.cargar()` y `Datos.guardar()` para la función Netlify.
- `netlify/functions/pruebas.js`: API del servidor. Lee y escribe los tres JSON en `FABIOR1981/bd`, ruta `pruebas_llamados`, rama `main`, mediante la API de GitHub.

Las páginas cargan los scripts clásicos en el orden indicado en su HTML y comparten estado mediante globales como `Datos` y `baseDatosGlobal`; conserva ese contrato si modificas módulos. No hay datos JSON de respaldo en este proyecto.

### Modelo de datos

La respuesta de la API combina tres archivos:

- `areas.json`: objeto indexado por sigla de área; cada entrada tiene `nombre`.
- `cargos.json`: objeto indexado por código `CAR-...`; cada cargo tiene `nombre`, `area` y `competencias` (cada competencia usa `comp`, `desc` y, opcionalmente, `codigo`).
- `dinamicas.json`: arreglo; cada dinámica relaciona su cargo mediante `cargo_id` y contiene `id`, `titulo`, `desc`, `tiempo_limite`, `hoja_postulante`, `caso_o_consigna`, `guia_evaluacion` y `respuesta_esperada`. `peso` y `renglones_hoja` son opcionales.

Las competencias definen las columnas de puntuación. La referencia opcional de sus códigos dentro de `caso_o_consigna` limita qué competencias aplican a cada dinámica; el cálculo final se normaliza sobre 30 puntos y considera `peso`.

El frontend estático debe desplegarse junto con la función en Netlify, o con otro servidor que implemente exactamente la ruta y el contrato de esa API. Publicar solo los archivos estáticos (por ejemplo, en GitHub Pages) no alcanza. No hay comando de *build*.

## Actualizar los datos

Los datos viven únicamente en el repositorio `FABIOR1981/bd`, carpeta `pruebas_llamados/` (`areas.json`, `cargos.json`, `dinamicas.json`). El navegador los lee y guarda mediante la función de Netlify `netlify/functions/pruebas.js`; no hay respaldo ni lectura/escritura de JSON locales en este proyecto. Si la función no está disponible, la app muestra el error y no carga datos locales.

Variables de entorno en Netlify:

| Variable | Uso |
|---|---|
| `GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD` | Token válido con acceso al repositorio `bd`; se usa para leer repos privados y para guardar. |
| `CLAVE_PRUEBAS` | Contraseña que pide el panel para guardar. |

Si `bd` es público, la lectura puede funcionar sin token. Si es privado, Netlify debe tener configurado un `GITHUB_TOKEN_PRUEBAS_LLAMADOS_BD` válido con permiso de lectura; para guardar se requieren el token con permiso de escritura y `CLAVE_PRUEBAS`.

1. En el **Panel de Administración**, hacer los cambios.
2. Hacer clic en **Guardar en GitHub** e ingresar la contraseña. Si los archivos todavía no existen en `bd/pruebas_llamados/`, se crean en el primer guardado.

> Los cambios del panel quedan en memoria hasta guardar. Si se recarga la página antes, se pierden.

## Estructura

```
├── index.html / script.js / style.css   Generador e impresión
├── admin.html / admin.js / admin.css    Panel de Administración
├── bases_ia.js                          Asistente local que prepara prompts
├── utilidades.js                        Utilidades compartidas
├── datos/
│   └── datos.js                         Cliente de la API de Netlify
├── netlify/functions/pruebas.js         API para los JSON del repositorio bd
└── documentacion/LEEME.md               Enlace a manuales externos
```

```

