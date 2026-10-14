# Bundle del Proyecto para IA: techfaro-webutilidades

## Estructura de Archivos
- README.md
- archivospruebas/cedulas.txt
- archivospruebas/postulantes.txt
- css/bolillero.css
- css/bolillerodatos.css
- css/comunes.css
- css/login.css
- css/mbti.css
- css/menu.css
- css/ovo.css
- diagrama.txt
- documentacion/LEEME.md
- estructura.txt
- html/COMO_AGREGAR_UNA_HERRAMIENTA.md
- html/_plantilla.html
- html/bolillero.html
- html/bolillerocompleto.html
- html/data_entry_mbti.html
- html/hash_sha256.html
- html/mbti.html
- html/mbti_para_imprimir_a4.html
- html/mbti_para_imprimir_a4_3X9.html
- html/menu.html
- html/ovo.html
- imagenes/fondo1.jpg
- index.html
- js/_plantilla.js
- js/bolillero.js
- js/bolillerodatos.js
- js/data_entry_mbti.js
- js/funcgral.js
- js/login.js
- js/mbti.js
- js/mbti_nuevo.js
- js/menu-manifest.js
- js/menu.js
- js/ovo.js

---

## Código Fuente

### Archivo: `README.md`
```md
# WebUtilidades (TechFaRo)

Conjunto de herramientas web de apoyo para el trabajo de evaluación y selección: cuestionarios, correctores, sorteos y utilidades. Se entra con usuario y contraseña, y las herramientas se eligen desde un menú lateral.

Sitio publicado: https://techfaro-webutilidades.netlify.app

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/techfaro-webutilidades/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/techfaro-webutilidades/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## Herramientas

| Herramienta | Qué hace |
|---|---|
| **OVO (Orientación Vocacional)** | Cuestionario vocacional paso a paso, con gráfico de resultados y detección del tipo de perfil (dominante, mixto o multipotencial). Se puede imprimir el cuestionario solo o junto con los resultados. Para ver los resultados se pide usuario y contraseña. |
| **MBTI Formulario** | Cuestionario MBTI en pantalla, más versiones para imprimir en A4. |
| **MBTI Corrector** | Carga de las respuestas de un cuestionario en papel y cálculo del resultado con su interpretación. |
| **Bolillero por CI** | Arma grupos al azar a partir de un archivo de cédulas, sin repetir y validando el dígito verificador uruguayo. |
| **Bolillero Datos Completo** | Igual que el anterior, pero a partir de un archivo con todos los datos de cada postulante. |
| **Hash SHA-256** | Calcula el hash de un texto, por ejemplo para preparar contraseñas. |

En `archivospruebas/` hay archivos de ejemplo para probar los bolilleros.

## Cómo se usa

1. Entrá al sitio e iniciá sesión.
2. Elegí la herramienta en el menú. Se abre dentro de la misma página.
3. Con **Cerrar sesión** salís.

## Agregar una herramienta nueva

Está explicado paso a paso en [html/COMO_AGREGAR_UNA_HERRAMIENTA.md](html/COMO_AGREGAR_UNA_HERRAMIENTA.md). En resumen:

1. Copiá `html/_plantilla.html` y `js/_plantilla.js`.
2. Escribí la herramienta.
3. Sumala al menú en `js/menu-manifest.js`.

## Ejecutar localmente

No necesita instalación ni build. Serví la carpeta con cualquier servidor estático (por ejemplo `npx serve`) y abrí `index.html`.

## Estructura

```
index.html               Login
html/                    Menú y herramientas (OVO, MBTI, bolilleros, hash)
js/                      Lógica de cada herramienta, login y menú (menu-manifest.js)
css/                     Estilos
imagenes/                Fondos
archivospruebas/         Archivos de ejemplo para los bolilleros
```

```

### Archivo: `archivospruebas/cedulas.txt`
```txt
12345678
30856946
22222222
33333333
30340507
44444444
55555555
66666666
77777777
99999999
01234567
10235478
78946512
98765421
74185232
14725896
32165478
21457859
35795156
35795148
12345678
30856946
22222222
33333333
30340507
44444444
55555555
66666666
77777777
99999999
01234567
10235478
78946512
98765421
74185232
14725896
32165478
21457859
35795156
35795148
35795148
35795148
11111111
88888888

```

### Archivo: `archivospruebas/postulantes.txt`
```txt
11111111;Juan;Pérez;098765432;juanperez@example.com;Calle Principal 123
11111111;Juan;Pérez;098765432;juanperez@example.com;Calle Principal 123
11111111;Juan;Pérez;098765432;juanperez@example.com;Calle Principal 123
11111111;Juan;Pérez;098765432;juanperez@example.com;Calle Principal 123
11111111;Juan;Pérez;098765432;juanperez@example.com;Calle Principal 123
22222222;María;López;987654321;marialopez@example.com;Avenida Secundaria 456
33333333;Pedro;García;876543210;pedrog@example.com;Calle Lateral 789
44444444;Ana;González;765432109;anagonzalez@example.com;Avenida Central 123
55555555;Carlos;Rodríguez;654321098;carlosrodriguez@example.com;Calle Lateral 456
66666666;Sofía;Martínez;543210987;sofiamartinez@example.com;Avenida Principal 789
77777777;Diego;Hernández;432109876;diegohernandez@example.com;Calle Secundaria 123
88888888;Laura;Jiménez;321098765;laurajimenez@example.com;Avenida Central 456
99999999;Pablo;Romero;210987654;pabloromero@example.com;Calle Lateral 789
111222334;Valentina;Santos;109876543;valentinasantos@example.com;Avenida Principal 123
444555667;Lucas;Vázquez;987654321;lucasvazquez@example.com;Calle Secundaria 456
777888991;Camila;Gómez;876543210;camilagomez@example.com;Avenida Central 789
30856946;Mateo;Díaz;765432109;mateodiaz@example.com;Calle Lateral 123
555666778;Victoria;Fernández;654321098;victoriafernandez@example.com;Avenida Principal 456
888999001;Benjamín;Moreno;543210987;benjaminmoreno@example.com;Calle Secundaria 789
111222335;Emma;Castillo;432109876;emmacastillo@example.com;Avenida Central 123
444555668;Lucas;Pérez;321098765;lucasperez@example.com;Calle Lateral 456
777888992;Sofía;García;210987654;sofiagarcia@example.com;Avenida Principal 789
222333446;Mateo;López;109876543;mateolopez@example.com;Calle Secundaria 123
```

### Archivo: `css/bolillero.css`
```css

body {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    background: linear-gradient(135deg, #f8fafc 60%, #00bfae 100%);
}
.container-flex {
  display: flex;
  flex-grow: 1;
  gap: 32px;
  flex-wrap: wrap;
}
.container {
  background: #fff;
  padding: 32px;
  border-radius: 16px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.12);
}



h1 {
  font-size: 2.2rem;
  margin-bottom: 32px;
  color: #43a047;
}

#archivoCedulas {
  padding: 16px;
  border: 1px solid #43a047;
  border-radius: 16px;
  font-size: 1rem;
}

button {
  background: #43a047;
  color: #fff;
  padding: 16px 32px;
  border: none;
  border-radius: 16px;
  cursor: pointer;
  font-size: 1.1rem;
  font-weight: 600;
  box-shadow: 0 4px 16px rgba(0,0,0,0.12);
  margin: 8px;
  transition: background 0.3s, transform 0.2s;
  outline: none;
  border: 2px solid transparent;
}

button:hover {
  background: #00bfae;
  transform: scale(1.04);
  border-color: #43a047;
}

#resultados {
  margin-top: 32px;
}

#listaCedulas {
  list-style: none;
  padding: 0;
  margin: 0;
}

#listaCedulas li {
  background-color: white;
  padding: 10px;
  margin: 5px 0;
  border-radius: var(--border-radius);
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.2);
}

#mensaje {
  color: red;
  font-weight: bold;
}

/* Estilos para el footer */
footer {
    /*background-color: #f5f5f5;*/
	background-color: rgba(0, 0, 0, 0.1); /* Fondo negro con 50% de opacidad */
    padding: 10px;
    text-align: right;
}



```

### Archivo: `css/bolillerodatos.css`
```css


.container {
    background-color: #fff;
}



h1 {
    font-size: 2rem;
}

#archivoCedulas {
    padding: 10px;
    border: 1px solid var(--secondary-color);
    border-radius: var(--border-radius);
}

button:hover {
    filter: brightness(0.9);
}

#resultados {
    margin-top: 20px;
}

#listaCedulas {
    list-style: none;
    padding: 0;
    margin: 0;
}

#listaCedulas li {
    background-color: white;
    padding: 10px;
    margin: 5px 0;
    border-radius: var(--border-radius);
    box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.2);
}

#mensaje {
    color: red;
    font-weight: bold;
}

footer {
    background-color: rgba(0, 0, 0, 0.1);
}

.grupo {
    writing-mode: vertical-lr;
    -webkit-writing-mode: vertical-lr;
    text-orientation: upright;
    width: 50px;
    text-align: center;
}
```

### Archivo: `css/comunes.css`
```css
:root {
    --primary-color: #43a047;
    --primary-contrast: #ffffff;
    --secondary-color: #f8fafc;
    --accent-color: #00bfae;
    --text-color: #1f2937;
    --muted-color: #6b7280;
    --border-radius: 12px;
    --font-family: 'Inter', 'Roboto', Arial, sans-serif;
    --shadow: 0 10px 28px rgba(15, 23, 42, 0.14);
    --shadow-soft: 0 4px 14px rgba(15, 23, 42, 0.09);
    --max-width: 2200px;
}

* {
    box-sizing: border-box;
}

body {
    font-family: var(--font-family);
    min-height: 100vh;
    margin: 0;
    padding: 0;
    color: var(--text-color);
    background: linear-gradient(140deg, #e9f2f8 0%, #f7fbfd 40%, #e8f5f5 100%);
}

main {
    /* Antes: width: min(100%, 1160px); margin: 0 auto;
       Eso dejaba franjas enormes vacías a los costados en pantallas anchas
       y hacía que el menú pareciera centrado en vez de pegado al borde. */
    width: 100%;
    max-width: var(--max-width);
    margin: 0;
    padding: 24px 20px 32px;
}

[data-theme='dark'] {
    --primary-color: #22c55e;
    --primary-contrast: #0f172a;
    --secondary-color: #0f172a;
    --accent-color: #14b8a6;
    --text-color: #e2e8f0;
    --muted-color: #94a3b8;
    --background: #020617;
    --surface: #0a1120;
    --border-color: #1e293b;
    --shadow: 0 10px 28px rgba(0, 0, 0, 0.42);
    --shadow-soft: 0 4px 14px rgba(0, 0, 0, 0.28);
}

[data-theme='dark'] body {
    background: #030b1a;
    color: var(--text-color);
}

[data-theme='dark'] .container {
    background-color: var(--surface);
    border: 1px solid var(--border-color);
    box-shadow: var(--shadow-soft);
}

[data-theme='dark'] #contIzq,
[data-theme='dark'] #contDer {
    background-color: rgba(15, 23, 42, 0.9);
}

[data-theme='dark'] input,
[data-theme='dark'] select,
[data-theme='dark'] textarea {
    background: #0b162f;
    color: #e2e8f0;
    border-color: #334155;
}

[data-theme='dark'] button,
[data-theme='dark'] input[type='submit'] {
    background-color: #22c55e;
    color: #0f172a;
}

[data-theme='dark'] tr:nth-child(odd) {
    background: #0b172f;
}

[data-theme='dark'] tr:nth-child(even) {
    background: #0e1f39;
}

[data-theme='dark'] th {
    background: #10213a;
    color: #e2e8f0;
}

/* Refuerza contraste en modo oscuro para cuestionarios / tarjetas que usan colores claros por defecto */
[data-theme='dark'] .main-content,
[data-theme='dark'] .pregunta,
[data-theme='dark'] .container,
[data-theme='dark'] #contIzq,
[data-theme='dark'] #contDer {
    background-color: #0a1225;
    border-color: #24304f;
    color: #e2e8f0;
}

[data-theme='dark'] .pregunta {
    background: #0f1a33;
    border: 1px solid #2e4269;
    box-shadow: 0 2px 8px rgba(0,0,0,0.45);
}

[data-theme='dark'] .pregunta h2 {
    color: #7ee4a0;
}

[data-theme='dark'] .opciones-radio label {
    color: #cbd5e1;
}

[data-theme='dark'] #resultado {
    background-color: rgba(255, 255, 255, 0.08);
    border-color: #1f2f48;
}

[data-theme='dark'] button,
[data-theme='dark'] #btnAutoCompletar,
[data-theme='dark'] input[type='submit'] {
    background-color: #22c55e;
    color: #0f172a;
    border-color: #1e293b;
}

.container-flex {
    display: flex;
    gap: 20px;
    flex-wrap: nowrap;
    justify-content: flex-start;
    align-items: flex-start;
    overflow-x: hidden;
}

.container {
    background-color: #ffffff;
    border: 1px solid #e5edf5;
    box-shadow: var(--shadow-soft);
    border-radius: var(--border-radius);
    padding: 20px;
    flex: 1 1 320px;
    min-width: 280px;
    margin-bottom: 20px;
}

#contIzq {
    flex: 0 0 280px;
    max-width: 280px;
    background-color: rgba(255, 255, 255, 0.92);
}

#contDer {
    flex: 1 1 auto;
    min-width: 0;
    max-width: calc(100vw - 320px);
    background-color: rgba(255, 255, 255, 0.92);
    overflow-x: hidden;
    overflow-y: auto;
    word-wrap: break-word;
    margin-left: 0;
}

#contDer iframe {
    width: 100%;
    min-width: 0;
    max-width: 100%;
    min-height: 640px;
    border: none;
    overflow-x: hidden;
    overflow-y: auto;
}

#contIzq {
    flex: 0 0 280px;
    width: 280px;
    max-width: 280px;
}

.container-flex {
    display: flex;
    gap: 20px;
    flex-wrap: nowrap;
    justify-content: flex-start;
    align-items: flex-start;
    width: 100%;
}

.container-flex {
    overflow-x: hidden;
}


body,
html {
    overflow-x: hidden;
    overflow-y: auto;
}


form {
    display: flex;
    flex-direction: column;
    gap: 16px;
}

label {
    font-weight: 600;
    color: var(--text-color);
}

input,
select,
textarea {
    width: 100%;
    padding: 11px 12px;
    border: 1px solid #d3dce6;
    border-radius: 10px;
    background: #fcfeff;
    color: var(--text-color);
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

input:focus,
select:focus,
textarea:focus,
button:focus,
a:focus {
    outline: none;
    border-color: var(--primary-color);
    box-shadow: 0 0 0 4px rgba(67, 160, 71, 0.15);
}

button,
input[type='submit'] {
    background-color: var(--primary-color);
    color: var(--primary-contrast);
    border: none;
    border-radius: 10px;
    padding: 11px 16px;
    font-weight: 700;
    cursor: pointer;
    transition: transform 0.2s ease, background-color 0.25s ease, box-shadow 0.2s ease;
}

button:hover,
input[type='submit']:hover {
    background-color: #388e3c;
    transform: translateY(-1px);
    box-shadow: 0 10px 20px rgba(24, 123, 70, 0.18);
}

h1,
h2,
h3,
h4 {
    margin: 0 0 12px;
    font-family: var(--font-family);
}

h1 {
    text-align: center;
    color: #111827;
}

p,
small {
    color: var(--muted-color);
}

footer {
    padding: 14px 18px;
    text-align: center;
    color: #4b5563;
    font-size: 0.9rem;
}

table {
    border-collapse: collapse;
    width: 100%;
    margin-top: 12px;
    border: 1px solid #e2e8f0;
    border-radius: 12px;
    overflow: hidden;
}

th,
td {
    padding: 12px 10px;
    text-align: left;
    border-bottom: 1px solid #e5edf5;
}

th {
    background: #edf7fb;
    color: #0f172a;
    font-weight: 700;
}

tr:nth-child(even) {
    background: #fcfeff;
}

tr:nth-child(odd) {
    background: #f8fbff;
}

tr:hover {
    background: #eaf4fa;
}

@media (max-width: 1024px) {
    .container-flex {
        flex-direction: column;
        /* En modo columna el "ancho" pasa a ser el eje transversal: sin esto,
           align-items:flex-start (pensado para el modo fila de escritorio)
           hace que los paneles se encojan a su tamaño de contenido en vez de
           ocupar el ancho disponible -- y un iframe sin ancho resuelto cae
           en su tamaño por defecto del navegador (300px). */
        align-items: stretch;
    }

    #contIzq,
    #contDer,
    .container {
        max-width: 100%;
        width: 100%;
        flex: 1 1 auto;
    }
}

```

### Archivo: `css/login.css`
```css


.container {
    background-color: #fff;
    width: 300px;
}



footer {
    background-color: rgba(0, 0, 0, 0.1);
}
```

### Archivo: `css/mbti.css`
```css
/* Las variables de color, tipografía y sombra vienen de comunes.css
   (ya cargado antes que este archivo), para que todas las herramientas
   compartan la misma paleta en un solo lugar. */

body {
    font-family: var(--font-family);
    min-height: 100vh;
    margin: 0;
    padding: 0;
    background: linear-gradient(135deg, var(--secondary-color) 60%, var(--accent-color) 100%);
    color: var(--text-color);
    display: flex;
    flex-direction: column;
    align-items: stretch;
}

.main-content {
    width: 100%;
    max-width: 900px;
    margin: 32px auto;
    padding: 32px;
    background: #fff;
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
}

h1 {
    text-align: center;
    color: var(--primary-color);
    margin-bottom: 32px;
    font-size: 2.2rem;
}

/* Estilos de la rejilla de preguntas */
#preguntas {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 24px;
    padding: 0;
}

.pregunta {
    border: 1px solid #C8E6C9;
    padding: 24px;
    background: #F1F8E9;
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
    font-size: 14px;
    display: flex;
    flex-direction: column;
}

.pregunta h2 {
    margin: 0 0 10px 0;
    font-size: 1em;
    font-weight: bold;
    color: #388E3C; 
}

.opciones-radio {
    display: flex;
    justify-content: space-between; /* Distribuye las 5 opciones */
    margin-top: 10px;
    user-select: none; /* Evita que se seleccione el texto al hacer clic */
}

.opciones-radio label {
    display: flex;
    flex-direction: column; 
    align-items: center;
    cursor: pointer;
    font-size: 0.9em;
    text-align: center;
    color: #666;
    padding: 5px;
}

.opciones-radio input[type="radio"] {
    margin-top: 5px;
    transform: scale(1.2); 
    cursor: pointer;
}

/* Botones y Resultado */
.botones-container {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-top: 30px;
    flex-wrap: wrap; /* Para que se adapten mejor en móviles */
}

button {
    background: var(--primary-color);
    color: #fff;
    padding: 16px 32px;
    border: none;
    border-radius: var(--border-radius);
    cursor: pointer;
    font-size: 1.1rem;
    font-weight: 600;
    box-shadow: var(--shadow);
    margin: 8px;
    transition: background 0.3s, transform 0.2s;
    outline: none;
    border: 2px solid transparent;
    animation: fadeIn 0.4s;
    font-size: 1.1em;
    font-weight: bold;
}

button:hover {
    background: var(--accent-color);
    transform: scale(1.04);
    border-color: var(--primary-color);
}

/* Estilo especial para el botón de test */
#btnAutoCompletar {
    background-color: #f39c12; /* Naranja/Amarillo para test */
}
#btnAutoCompletar:hover {
    background-color: #e67e22; 
}

#resultado {
    margin-top: 20px;
    padding: 20px;
    border: 2px solid #2E7D32;
    background-color: #DCEDC8; /* Fondo de resultado */
    border-radius: 5px;
    font-weight: bold;
    text-align: center;
}
```

### Archivo: `css/menu.css`
```css
/* ======================================= */
/* Archivo: menu.css (mejoras para submenú + responsive) */
/* ======================================= */

.container {
    padding: 0; /* ya controla comunes.css */
}

nav {
    width: 100%;
}

.menu,
.submenu {
    list-style: none;
    margin: 0;
    padding: 0;
}

.menu > .menu-item {
    border-bottom: 1px solid rgba(67, 160, 71, 0.24);
}

.menu-item > a {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px 14px;
    text-decoration: none;
    color: #0f172a;
    background: #f2fcf8;
    border-radius: 10px;
    margin: 8px 8px;
    font-size: 1.01rem;
    font-weight: 700;
    transition: background-color 0.2s ease, transform 0.2s ease;
}

.menu-item > a:hover,
.menu-item > a:focus {
    background: #d8f7e3;
    transform: translateY(-1px);
}

.menu-item > a::after {
    content: '\25BC';
    font-size: 0.72rem;
    transition: transform 0.2s ease;
    opacity: 0.7;
}

.menu-item:not(:has(.submenu)) > a::after {
    content: '';
}

.menu-item:hover > a::after,
.menu-item > a:focus::after {
    transform: rotate(-180deg);
}

.submenu {
    display: none;
    padding-left: 12px;
    margin-left: 8px;
}

.menu-item:hover > .submenu,
.menu-item:focus-within > .submenu,
.menu-item.open > .submenu {
    display: block;
}

.submenu li {
    margin: 4px 0;
}

.submenu li a {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 8px 16px;
    text-decoration: none;
    color: #1f2937;
    border-radius: 8px;
    transition: background-color 0.15s ease;
    font-weight: 500;
}

/* Las hojas del menú (abren una herramienta real, no despliegan nada más)
   llevan una flecha hacia la DERECHA, para indicar "esto navega" y no
   confundirlas con las categorías, que despliegan más contenido (flecha
   hacia abajo, arriba). */
.submenu > li:not(.menu-item) > a::after {
    content: '\25B6';
    font-size: 0.72rem;
    opacity: 0.5;
}

.submenu li a:hover,
.submenu li a:focus {
    background-color: #e6fdf5;
    color: #0f5132;
}

.menu-item .submenu .menu-item > a {
    background: #e6f9f1;
}

@media (max-width: 900px) {
    .container-flex {
        flex-direction: column;
    }

    #contIzq,
    #contDer {
        width: 100%;
        flex: 1 1 auto;
        max-width: none;
    }

    .menu-item > a {
        margin: 5px 0;
    }
}

/* ============ Header de marca (menu.html) ============ */
.site-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 16px;
    padding: 14px 24px;
    background: #fff;
    border-bottom: 1px solid #e5edf5;
    box-shadow: var(--shadow-soft);
    position: sticky;
    top: 0;
    z-index: 20;
}

.site-header__brand {
    display: flex;
    align-items: center;
    gap: 12px;
}

.site-header__logo {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    border-radius: 10px;
    background: var(--primary-color);
    color: var(--primary-contrast);
    font-weight: 800;
    font-size: 0.95rem;
    letter-spacing: 0.02em;
    flex-shrink: 0;
}

.site-header__titles {
    display: flex;
    flex-direction: column;
    line-height: 1.2;
}

.site-header__title {
    font-weight: 800;
    font-size: 1.05rem;
    color: var(--text-color);
}

.site-header__subtitle {
    font-size: 0.78rem;
    color: var(--muted-color);
}

.site-header__user {
    display: flex;
    align-items: center;
    gap: 14px;
}

.site-header__username {
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--text-color);
    white-space: nowrap;
}

.site-header__logout {
    background: transparent;
    color: var(--muted-color);
    border: 1px solid #e5edf5;
    padding: 7px 14px;
    font-size: 0.85rem;
    font-weight: 600;
    border-radius: 8px;
    transition: background-color 0.15s ease, color 0.15s ease, border-color 0.15s ease;
}

.site-header__logout:hover {
    background: #fdecec;
    color: #b91c1c;
    border-color: #f3c8c8;
    transform: none;
    box-shadow: none;
}

[data-theme='dark'] .site-header {
    background: #0a1120;
    border-bottom-color: #1e293b;
}

[data-theme='dark'] .site-header__title,
[data-theme='dark'] .site-header__username {
    color: #e2e8f0;
}

[data-theme='dark'] .site-header__logout {
    border-color: #334155;
    color: #94a3b8;
}

@media (max-width: 600px) {
    .site-header {
        flex-wrap: wrap;
        padding: 12px 16px;
    }
    .site-header__subtitle {
        display: none;
    }
}

/* ============ Estado activo: qué herramienta está abierta ============ */
.submenu li:not(.menu-item) > a.active {
    background: var(--primary-color);
    color: var(--primary-contrast);
    font-weight: 700;
}

.submenu li:not(.menu-item) > a.active::after {
    color: var(--primary-contrast);
    opacity: 0.9;
}

[data-theme='dark'] .submenu li:not(.menu-item) > a.active {
    color: #0f172a;
}

/* ============ Íconos del menú ============ */
.menu-link-content {
    display: flex;
    align-items: center;
    gap: 10px;
    min-width: 0;
}

.menu-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 18px;
    height: 18px;
    flex-shrink: 0;
    opacity: 0.75;
}

.menu-icon svg {
    width: 100%;
    height: 100%;
}

.menu-label {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.submenu li:not(.menu-item) > a.active .menu-icon {
    opacity: 1;
}

/* ============ Correcciones de contraste en modo oscuro ============ */
.theme-label {
    color: var(--text-color);
}
[data-theme='dark'] .theme-label {
    color: #e2e8f0;
}

/* El botón de cerrar sesión debe seguir siendo "fantasma" (borde sutil) en
   modo oscuro, no el verde sólido que se le da a los botones normales -
   si no, queda texto verde sobre fondo verde, casi ilegible. */
[data-theme='dark'] .site-header__logout {
    background: transparent;
    color: #94a3b8;
    border-color: #334155;
}
[data-theme='dark'] .site-header__logout:hover {
    background: #2a1414;
    color: #f87171;
    border-color: #7f1d1d;
}

/* ============ Modo oscuro: renglones del menú ============ */
/* Las reglas de más arriba (colores de .menu-item > a, .submenu li a, etc.)
   solo contemplaban el tema claro. Sin esto, los renglones del menú se
   quedan con fondo/texto claros aunque el resto de la página ya pasó a
   oscuro -- y algunos textos (las hojas del submenú) quedan casi ilegibles,
   texto oscuro sobre fondo oscuro. */

[data-theme='dark'] .menu > .menu-item {
    border-bottom-color: rgba(34, 197, 94, 0.24);
}

[data-theme='dark'] .menu-item > a {
    background: var(--surface);
    color: var(--text-color);
    border: 1px solid var(--border-color);
}

[data-theme='dark'] .menu-item > a:hover,
[data-theme='dark'] .menu-item > a:focus {
    background: #16203a;
}

[data-theme='dark'] .submenu li a {
    color: var(--muted-color);
}

[data-theme='dark'] .submenu li a:hover,
[data-theme='dark'] .submenu li a:focus {
    background-color: #16203a;
    color: var(--text-color);
}

[data-theme='dark'] .menu-item .submenu .menu-item > a {
    background: #0d1526;
    border-color: var(--border-color);
}

```

### Archivo: `css/ovo.css`
```css
/* O.V.O. Mejorado - TechFaRo */
:root {
    --bg: #f0f2f5;
    --card: #ffffff;
    --text: #1a1a2e;
    --text-light: #555;
    --accent: #43a047;
    --accent-hover: #388e3c;
    --yes: #10b981;
    --no: #ef4444;
    --border: #e5e7eb;
    --shadow: 0 4px 6px -1px rgba(0,0,0,0.1), 0 2px 4px -2px rgba(0,0,0,0.1);
    --shadow-lg: 0 20px 25px -5px rgba(0,0,0,0.1), 0 8px 10px -6px rgba(0,0,0,0.1);
}
[data-theme="dark"] {
    --bg: #0f172a;
    --card: #1e293b;
    --text: #f1f5f9;
    --text-light: #94a3b8;
    --accent: #22c55e;
    --accent-hover: #16a34a;
    --border: #334155;
    --shadow: 0 4px 6px -1px rgba(0,0,0,0.3), 0 2px 4px -2px rgba(0,0,0,0.3);
    --shadow-lg: 0 20px 25px -5px rgba(0,0,0,0.4), 0 8px 10px -6px rgba(0,0,0,0.4);
}

* { margin: 0; padding: 0; box-sizing: border-box; }
body {
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    transition: background 0.3s, color 0.3s;
}

.app-header {
    background: var(--card);
    border-bottom: 1px solid var(--border);
    padding: 16px 24px;
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: var(--shadow);
}
.app-header-inner {
    max-width: 800px;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.logo {
    font-size: 22px;
    font-weight: 800;
    letter-spacing: 2px;
    color: var(--accent);
}
.logo span {
    font-size: 11px;
    font-weight: 500;
    color: var(--text-light);
    letter-spacing: 1px;
    display: block;
    margin-top: -2px;
}
.theme-btn {
    background: none;
    border: 1.5px solid var(--border);
    border-radius: 20px;
    width: 44px;
    height: 24px;
    position: relative;
    cursor: pointer;
    transition: all 0.3s;
}
.theme-btn::after {
    content: '';
    position: absolute;
    top: 2px;
    left: 2px;
    width: 18px;
    height: 18px;
    background: var(--accent);
    border-radius: 50%;
    transition: transform 0.3s;
}
[data-theme="dark"] .theme-btn::after { transform: translateX(20px); }

.container { max-width: 800px; margin: 0 auto; padding: 24px; }

.progress-wrap { margin-bottom: 24px; }
.progress-info {
    display: flex;
    justify-content: space-between;
    font-size: 13px;
    color: var(--text-light);
    margin-bottom: 6px;
}
.progress-bar {
    height: 8px;
    background: var(--border);
    border-radius: 99px;
    overflow: hidden;
}
.progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--accent), var(--accent-color, #00bfae));
    border-radius: 99px;
    transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
    width: 0%;
}

.steps-dots {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin-bottom: 20px;
}
.step-dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: var(--border);
    transition: all 0.3s;
    cursor: pointer;
}
.step-dot.active { background: var(--accent); transform: scale(1.3); }
.step-dot.completed { background: var(--yes); }

.question-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 20px 24px;
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    gap: 16px;
    box-shadow: var(--shadow);
    transition: all 0.3s ease;
    animation: slideIn 0.4s ease both;
    position: relative;
    overflow: hidden;
}
.question-card:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
    border-color: var(--accent);
}
.question-card.answered-yes {
    border-left: 4px solid var(--yes);
    background: linear-gradient(90deg, rgba(16,185,129,0.05) 0%, var(--card) 100%);
}
.question-card.answered-no {
    border-left: 4px solid var(--no);
    opacity: 0.7;
}
@keyframes slideIn {
    from { opacity: 0; transform: translateY(12px); }
    to { opacity: 1; transform: translateY(0); }
}
.q-number {
    font-size: 20px;
    font-weight: 800;
    color: var(--accent);
    min-width: 36px;
    text-align: center;
    flex-shrink: 0;
}
.q-text {
    flex: 1;
    font-size: 15px;
    line-height: 1.4;
    color: var(--text);
}
.q-actions {
    display: flex;
    gap: 8px;
    flex-shrink: 0;
}
.btn-answer {
    padding: 10px 20px;
    border: 2px solid var(--border);
    border-radius: 10px;
    background: var(--card);
    color: var(--text);
    font-size: 13px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.2s;
    min-width: 60px;
}
.btn-answer:hover { transform: scale(1.05); }
.btn-answer.yes-btn:hover, .btn-answer.yes-btn.selected {
    background: var(--yes); border-color: var(--yes); color: white;
}
.btn-answer.no-btn:hover, .btn-answer.no-btn.selected {
    background: var(--no); border-color: var(--no); color: white;
}

.nav-buttons {
    display: flex;
    justify-content: space-between;
    margin-top: 24px;
    gap: 12px;
    flex-wrap: wrap;
}
.btn-nav {
    padding: 12px 28px;
    border-radius: 10px;
    border: none;
    font-size: 14px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.2s;
}
.btn-nav:disabled { opacity: 0.4; cursor: not-allowed; }
.btn-prev { background: var(--border); color: var(--text); }
.btn-prev:hover:not(:disabled) { background: #d1d5db; }
.btn-next {
    background: var(--accent);
    color: white;
    box-shadow: 0 4px 12px rgba(79,70,229,0.3);
}
.btn-next:hover:not(:disabled) {
    background: var(--accent-hover);
    transform: translateY(-1px);
}
.btn-print {
    background: var(--yes);
    color: white;
}
.btn-print:hover:not(:disabled) {
    background: #059669;
    transform: translateY(-1px);
}

.welcome-screen {
    text-align: center;
    padding: 40px 20px;
    animation: fadeIn 0.6s ease;
}
@keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
}
.welcome-screen h2 {
    font-size: 28px;
    margin-bottom: 12px;
    color: var(--accent);
}
.welcome-screen p {
    color: var(--text-light);
    font-size: 15px;
    max-width: 500px;
    margin: 0 auto 24px;
    line-height: 1.6;
}
.welcome-card {
    background: var(--card);
    border-radius: 16px;
    padding: 24px;
    max-width: 480px;
    margin: 0 auto 24px;
    text-align: left;
    border: 1px solid var(--border);
    box-shadow: var(--shadow);
}
.welcome-card h3 {
    font-size: 14px;
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 12px;
    color: var(--accent);
}
.welcome-card ul { list-style: none; padding: 0; }
.welcome-card li {
    padding: 6px 0;
    font-size: 14px;
    color: var(--text);
    display: flex;
    align-items: center;
    gap: 8px;
}
.welcome-card li::before {
    content: '✓';
    color: var(--yes);
    font-weight: 700;
}
.btn-start {
    padding: 14px 48px;
    background: linear-gradient(135deg, var(--accent), var(--accent-color, #00bfae));
    color: white;
    border: none;
    border-radius: 12px;
    font-size: 16px;
    font-weight: 700;
    cursor: pointer;
    box-shadow: 0 8px 24px rgba(79,70,229,0.35);
    transition: all 0.3s;
}
.btn-start:hover {
    transform: translateY(-2px);
    box-shadow: 0 12px 32px rgba(79,70,229,0.45);
}

.results-screen { animation: fadeIn 0.6s ease; }
.results-header { text-align: center; margin-bottom: 24px; }
.results-header h2 { font-size: 24px; color: var(--accent); margin-bottom: 6px; }
.results-header p { color: var(--text-light); font-size: 14px; }
.results-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 24px;
}
@media (max-width: 600px) { .results-grid { grid-template-columns: 1fr; } }
.result-card {
    background: var(--card);
    border-radius: 14px;
    padding: 18px;
    border: 1px solid var(--border);
    box-shadow: var(--shadow);
    transition: all 0.3s;
}
.result-card:hover {
    transform: translateY(-3px);
    box-shadow: var(--shadow-lg);
}
.result-card .area-name {
    font-size: 13px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    margin-bottom: 4px;
}
.result-card .area-desc {
    font-size: 12px;
    color: var(--text-light);
    margin-bottom: 10px;
}
.result-bar-bg {
    height: 10px;
    background: var(--border);
    border-radius: 99px;
    overflow: hidden;
    margin-bottom: 6px;
}
.result-bar-fill {
    height: 100%;
    border-radius: 99px;
    transition: width 1s cubic-bezier(0.4, 0, 0.2, 1);
}
.result-stats {
    display: flex;
    justify-content: space-between;
    font-size: 13px;
    font-weight: 700;
}
.chart-container {
    background: var(--card);
    border-radius: 16px;
    padding: 20px;
    border: 1px solid var(--border);
    box-shadow: var(--shadow);
    margin-bottom: 20px;
    position: relative;
    height: 320px;
}
.btn-restart {
    display: block;
    margin: 0 auto;
    padding: 12px 32px;
    background: var(--accent);
    color: white;
    border: none;
    border-radius: 10px;
    font-size: 14px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.2s;
}
.btn-restart:hover { background: var(--accent-hover); transform: translateY(-1px); }

.keyboard-hint {
    text-align: center;
    font-size: 11px;
    color: var(--text-light);
    margin-top: 12px;
    opacity: 0.7;
}

/* ===== MODALS ===== */
.modal-overlay {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.6);
    backdrop-filter: blur(4px);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
    animation: fadeIn 0.3s ease;
}
.modal-box {
    background: var(--card);
    border-radius: 16px;
    padding: 32px;
    width: 100%;
    max-width: 420px;
    box-shadow: var(--shadow-lg);
    border: 1px solid var(--border);
    animation: slideUp 0.3s ease;
}
@keyframes slideUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
}
.modal-box h3 {
    font-size: 18px;
    margin-bottom: 4px;
    color: var(--accent);
    text-align: center;
}
.modal-box .modal-sub {
    font-size: 12px;
    color: var(--text-light);
    text-align: center;
    margin-bottom: 20px;
}
.modal-box .lock-icon {
    font-size: 40px;
    text-align: center;
    margin-bottom: 12px;
}
.modal-field {
    margin-bottom: 14px;
}
.modal-field label {
    display: block;
    font-size: 11px;
    text-transform: uppercase;
    font-weight: 700;
    color: var(--text-light);
    margin-bottom: 4px;
}
.modal-field input {
    width: 100%;
    padding: 10px 12px;
    border: 2px solid var(--border);
    border-radius: 8px;
    background: var(--bg);
    color: var(--text);
    font-size: 14px;
    transition: border-color 0.2s;
}
.modal-field input:focus {
    outline: none;
    border-color: var(--accent);
}
.modal-error {
    color: var(--no);
    font-size: 12px;
    text-align: center;
    margin-bottom: 12px;
    min-height: 18px;
    font-weight: 600;
}
.modal-actions {
    display: flex;
    gap: 10px;
}
.modal-actions button {
    flex: 1;
    padding: 10px;
    border-radius: 8px;
    border: none;
    font-size: 13px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.2s;
}
.modal-btn-cancel { background: var(--border); color: var(--text); }
.modal-btn-cancel:hover { background: #d1d5db; }
.modal-btn-login { background: var(--accent); color: white; }
.modal-btn-login:hover { background: var(--accent-hover); }
.modal-btn-confirm { background: var(--yes); color: white; }
.modal-btn-confirm:hover { background: #059669; }
.modal-btn-skip {
    background: transparent;
    color: var(--text-light);
    border: 1.5px solid var(--border) !important;
}
.modal-btn-skip:hover { background: var(--border); }

.area-1 { color: #e11d48; }
.area-2 { color: #2563eb; }
.area-3 { color: #d97706; }
.area-4 { color: #059669; }
.area-5 { color: #7c3aed; }
.bg-area-1 { background: #e11d48; }
.bg-area-2 { background: #2563eb; }
.bg-area-3 { background: #d97706; }
.bg-area-4 { background: #059669; }
.bg-area-5 { background: #7c3aed; }

.hidden { display: none !important; }


/* ===== BOTONES DE RESULTADOS (nuevos) ===== */
.results-actions {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    justify-content: center;
    margin-top: 24px;
    margin-bottom: 16px;
}
.btn-action {
    padding: 12px 24px;
    border: 2px solid var(--border);
    border-radius: 12px;
    background: var(--card);
    color: var(--text);
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.25s ease;
    display: flex;
    align-items: center;
    gap: 8px;
    box-shadow: var(--shadow);
}
.btn-action:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
    border-color: var(--accent);
}
.btn-back-quiz:hover {
    border-color: var(--accent);
    background: linear-gradient(135deg, rgba(79,70,229,0.08), var(--card));
}
.btn-print-quiz:hover {
    border-color: #059669;
    background: linear-gradient(135deg, rgba(5,150,105,0.08), var(--card));
}
.btn-print-full:hover {
    border-color: #d97706;
    background: linear-gradient(135deg, rgba(217,119,6,0.08), var(--card));
}

```

### Archivo: `diagrama.txt`
```txt
webUtilidades
├── index.html
├──html/
│	├──bolillero.html
│	├──bolillerocompleto.html
│	├──mbti.html
│	├──menu.html
│	└──ovo.html
├──css/
│	├──login.css
│	├──menu.css
│	├──bolillero.css
│	├──mbti.css
│	├──bolillerocompleto.css
│	└──ovo.css
├──	js/
│	├──funcGral.js
│	├──bolillero.js
│	├──bolilleroDatos.js
│	├──login.js
│	├──menu.js
│	└──ovo.js
└──ArchivosPruebas/
	├──cedulas.txt
	└──postulantes.txt
```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/techfaro-webutilidades/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `estructura.txt`
```txt
[Archivo Binario o No Leible]
```

### Archivo: `html/COMO_AGREGAR_UNA_HERRAMIENTA.md`
```md
# Cómo agregar una herramienta nueva al menú

Esto es lo único que hay que saber para sumar "material de trabajo" nuevo
al proyecto, sin romper nada de lo que ya funciona.

## Pasos

1. **Copiá las plantillas:**
   - `html/_plantilla.html` → `html/tu-herramienta.html`
   - `js/_plantilla.js` → `js/tu-herramienta.js`

2. **Editá el HTML:** cambiá el `<title>` y el `<h1>`, y armá el contenido
   dentro del `<div class="container">`. Si necesitás estilos propios,
   creá `css/tu-herramienta.css` y enlazalo en el `<head>`.

3. **Editá el JS:** escribí la lógica de tu herramienta dentro del bloque
   `if (sessionStorage.getItem("verificado")) { ... }` que ya trae la
   plantilla. No hace falta armar el chequeo de sesión: ya viene puesto.

4. **Sumala al menú:** abrí `js/menu-manifest.js` y agregá un renglón
   dentro de la categoría que corresponda (o creá una categoría nueva):

   ```js
   { nombre: 'Tu Herramienta', archivo: 'tu-herramienta.html' },
   ```

   Ese es el **único** archivo que hay que tocar para que aparezca en el
   menú — no hace falta editar `menu.html`.

5. **Probala:** abrí `menu.html`, buscá tu herramienta en el menú y
   verificá que cargue bien.

## Errores típicos a evitar

Estos son bugs reales que aparecieron en este proyecto por herramientas
armadas "a mano" sin plantilla — por eso existe esta plantilla:

- **Olvidarse de `funcgral.js`**: si tu script usa `darUrl(...)`, tiene
  que estar cargado `<script src="../js/funcgral.js">` ANTES que tu
  propio `<script>`.
- **Armar tu propio menú lateral (`id="contIzq"`)**: no hace falta, la
  página ya se carga dentro del menú principal, que tiene el suyo.
- **Olvidarse de agregarla al manifest**: la herramienta queda "invisible",
  solo accesible escribiendo la URL a mano.
- **Nombres de archivo con mayúsculas inconsistentes** entre el `href`/
  `src` y el archivo real (ej. `FuncGral.js` vs `funcgral.js`): funciona
  en Windows pero se rompe en Netlify, que sí distingue mayúsculas.

```

### Archivo: `html/_plantilla.html`
```html
<!DOCTYPE html>
<!--
  PLANTILLA PARA UNA HERRAMIENTA NUEVA
  -------------------------------------
  Cómo usarla (ver también html/COMO_AGREGAR_UNA_HERRAMIENTA.md):

    1) Copiá este archivo a html/nombre-de-tu-herramienta.html
       Copiá también js/_plantilla.js a js/nombre-de-tu-herramienta.js
    2) Cambiá el <title> y el <h1> de acá abajo.
    3) Si tu herramienta necesita estilos propios, creá css/nombre.css
       y descomentá el <link> de más abajo.
    4) Escribí el contenido de tu herramienta dentro del <div class="container">.
    5) Escribí la lógica en tu archivo .js (ver la plantilla de JS: ya viene
       protegida con el chequeo de sesión, no hace falta agregarlo).
    6) Agregá un renglón en js/menu-manifest.js para que aparezca en el menú.

  Qué NO hay que hacer (errores ya vistos en este proyecto):
    - No armes tu propio "menú lateral" ni un <div id="contIzq"> acá:
      esta página se carga DENTRO del menú principal, que ya tiene el suyo.
    - No te olvides de <script src="../js/funcgral.js"> ANTES que tu propio
      script: si tu JS usa darUrl(), esa función vive en funcgral.js.
-->
<html>
<head>
    <title>Nombre de la Herramienta</title>
    <link rel="stylesheet" href="../css/comunes.css">
    <!-- <link rel="stylesheet" href="../css/nombre-de-tu-herramienta.css"> -->
</head>
<body>
    <div class="container">
        <h1>Nombre de la Herramienta</h1>

        <!-- Contenido de la herramienta acá -->

    </div>

    <script src="../js/funcgral.js"></script>
    <script src="../js/_plantilla.js"></script>

    <footer>
        <p>Copyright © 2024 TechFaRo. Todos los derechos reservados.</p>
    </footer>
</body>
</html>

```

### Archivo: `html/bolillero.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bolillero</title>
	<link rel="stylesheet" href="../css/comunes.css">
    <link rel="stylesheet" href="../css/bolillero.css">
</head>
<body>
	<div class="container">
	<form>
        <h1>Bolillero - Selector de Cédulas Aleatorias</h1>
        <input type="file" id="archivoCedulas" accept=".txt">
        <label for="cantidadCedulasPorGrupo">Cantidad por grupo:</label>
        <input type="number" id="cantidadCedulasPorGrupo" min="1" required>
	</form>
        <button onclick="generarListas()">Generar</button>
		
        <div id="resultados">
            <ul id="listaCedulas"></ul>
            <p id="mensaje"></p>
        </div>
    </div>
	<script src="../js/funcgral.js"></script>
    <script src="../js/bolillero.js"></script>
	
	<footer>
    <p>Copyright © 2024 TechFaRo. Todos los derechos reservados.</p>
</footer>
</body>
</html>
```

### Archivo: `html/bolillerocompleto.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bolillero</title>
	<link rel="stylesheet" href="../css/comunes.css">
    <link rel="stylesheet" href="../css/bolillerodatos.css">
</head>
<body>
	<div class="container">
	<h1>Bolillero - Selector de Cédulas Aleatorias</h1>
        <input type="file" id="archivoCedulas" accept=".txt">
        <label for="cantidadCedulasPorGrupo">Cantidad por grupo:</label>
        <input type="number" id="cantidadCedulasPorGrupo" min="1" required>
        <button onclick="generarListas()">Generar</button>
		<button id="btnImprimir" onclick="Imprimir()">Imprimir</button>

        <div id="resultados">
            <ul id="listaCedulas"></ul>
            <p id="mensaje"></p>
        </div>
	</div>
    <script src="../js/funcgral.js"></script>
	<script src="../js/bolillerodatos.js"></script>
    <footer>
        <p>Copyright © 2024 TechFaRo. Todos los derechos reservados.</p>
    </footer>

</body>
</html>
```

### Archivo: `html/data_entry_mbti.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Carga de Datos MBTI - Modo Manual</title>
    <link rel="stylesheet" href="../css/comunes.css">
    <link rel="stylesheet" href="../css/mbti.css"> 
    <style>
        /* Estilos esenciales para la interfaz de entrada numérica */
        .input-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr); /* 5 columnas para eficiencia */
            gap: 5px;
        }
        .input-grid input {
            width: 90%; 
            padding: 8px;
            text-align: center;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 1.1em;
        }
        .data-group {
            margin-bottom: 25px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
        }
        .main-content {
            max-width: 1200px; 
            margin: 20px auto;
            padding: 20px;
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .botones-container button {
            padding: 10px 20px;
            font-size: 1em;
            cursor: pointer;
            border: none;
            border-radius: 4px;
            margin-right: 10px;
            transition: background-color 0.3s;
        }
        /* Estilos para la sección de identificación global */
        .identificacion-global-group {
            margin-bottom: 30px;
            padding: 20px;
            border: 2px solid #3498db; 
            border-radius: 4px;
            background-color: #ecf0f1;
        }
        .identificacion-global-group input {
            width: 100%;
            box-sizing: border-box;
            margin-top: 5px;
        }
        .identificacion-global-group label {
            display: block;
            margin-top: 10px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="main-content">
        <h1>Carga de Resultados MBTI (Modos Manual y Directo)</h1>
        
        <div class="identificacion-global-group">
            <h2 style="color: #3498db; margin-top: 0;">Identificación del Candidato</h2>
            <div style="display: flex; gap: 20px;">
                <div style="flex: 1;">
                    <label for="nombreGlobal">Nombre Completo:</label>
                    <input type="text" id="nombreGlobal" placeholder="Nombre y Apellido">
                </div>
                <div style="flex: 1;">
                    <label for="cedulaGlobal">Cédula/DNI:</label>
                    <input type="text" id="cedulaGlobal" placeholder="Sin puntos ni guiones">
                </div>
            </div>
        </div>
        <div style="border: 2px solid #4CAF50; padding: 20px; border-radius: 8px; margin-bottom: 25px;">
            <h2 style="margin-top: 0; color: #4CAF50;">Modo 1: Carga Directa de Puntuación Neta (±50)</h2>
            <p style="text-align: center; font-style: italic; color: #555;">
                Ingrese el resultado de cada dimensión (rango: **-50** a **+50**).
            </p>
            <form id="directEntryForm" onsubmit="event.preventDefault(); calcularResultadoDirecto();">
                <div style="display: flex; justify-content: space-around; gap: 10px; margin-bottom: 15px; flex-wrap: wrap;">
                    <div>
                        <label for="netEI">E/I:</label>
                        <input type="number" id="netEI" min="-50" max="50" placeholder="E/I" style="width: 80px;" required>
                    </div>
                    <div>
                        <label for="netSN">S/N:</label>
                        <input type="number" id="netSN" min="-50" max="50" placeholder="S/N" style="width: 80px;" required>
                    </div>
                    <div>
                        <label for="netTF">T/F:</label>
                        <input type="number" id="netTF" min="-50" max="50" placeholder="T/F" style="width: 80px;" required>
                    </div>
                    <div>
                        <label for="netJP">J/P:</label>
                        <input type="number" id="netJP" min="-50" max="50" placeholder="J/P" style="width: 80px;" required>
                    </div>
                </div>
                <div class="botones-container" style="text-align: center;">
                    <button type="submit" style="background-color: #f39c12; color: white;">Calcular Resultado Directo</button>
                </div>
            </form>
        </div>
        <h2 style="color: #333;">Modo 2: Carga de 100 Respuestas (Raw Score 1-5)</h2>
        <p style="text-align: center; font-style: italic; color: #555;">
            Ingrese los valores de 1 a 5 para cada una de las 100 preguntas.
        </p>

        <form id="dataEntryForm" onsubmit="event.preventDefault(); calcularResultadoManual();">
            
            <div id="dataInputs">
                </div>

            <div class="botones-container" style="text-align: center;">
                <button type="submit" style="background-color: #4CAF50; color: white;">Calcular Resultado Manual</button>
                <button type="button" onclick="imprimirResultado()" style="background-color: #3498db; color: white;">Imprimir Solo Resultado</button>
            </div>
        </form>
        <div id="resultado" style="margin-top: 30px; padding: 20px; border-top: 2px dashed #ccc;">
            El resultado se mostrará aquí.
        </div>
    </div>

	<script src="../js/funcgral.js"></script>
    <script src="../js/data_entry_mbti.js"></script>
</body>
</html>
```

### Archivo: `html/hash_sha256.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Hash SHA-256 Online</title>
  <link rel="stylesheet" href="../css/comunes.css">
  <style>
    /* Ajustes propios de esta herramienta: una tarjeta angosta y centrada
       (no tiene sentido que un formulario tan chico ocupe todo el ancho).
       Colores, tipografía, radio de borde y sombra vienen de comunes.css. */
    body { display: flex; justify-content: center; padding: 40px 20px; }
    .container { max-width: 420px; width: 100%; }
    label { font-weight: 700; display: block; margin-bottom: 8px; }
    textarea { resize: none; font-family: monospace; }
  </style>
</head>
<body>
  <div class="container">
    <h2>Generador SHA-256</h2>
    <label for="input">Texto a codificar:</label>
    <input type="text" id="input" value="1234567">
    <button onclick="generarHash()">Generar SHA-256</button>
    <label for="output">Hash SHA-256:</label>
    <textarea id="output" rows="2" readonly></textarea>
  </div>
  <script src="../js/funcgral.js"></script>
  <script>
    if (sessionStorage.getItem("verificado")) {

      async function generarHash() {
        const texto = document.getElementById('input').value;
        const encoder = new TextEncoder();
        const data = encoder.encode(texto);
        const hashBuffer = await crypto.subtle.digest('SHA-256', data);
        const hashArray = Array.from(new Uint8Array(hashBuffer));
        const hashHex = hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
        document.getElementById('output').value = hashHex;
      }
      // Generar hash inicial
      generarHash();

    } else {
      // No hay sesión iniciada: volver al login
      window.location.href = darUrl('index.html');
    }
  </script>
</body>
</html>

```

### Archivo: `html/mbti.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cuestionario</title>

    <link rel="stylesheet" href="../css/comunes.css">
    <link rel="stylesheet" href="../css/mbti.css">
</head>
<body>
    <div class="main-content">
        <h1>Cuestionario de Personalidad MBTI (100 Items)</h1>
        
        <p style="text-align: center; font-style: italic; color: #555;">
        </p>

        <form id="miFormulario" onsubmit="event.preventDefault(); calcularResultado();">
            
            <div id="preguntas">
                </div>

            <div id="resultado">
                Presiona "Calcular Resultado" una vez que hayas respondido las 100 preguntas.
            </div>
            
            <div class="botones-container">
                <button type="button" id="btnAutoCompletar" onclick="autoCompletar()">Auto-Completar (Test)</button>
                
                <button type="submit">Calcular Resultado</button>
				
                <button id="btnImprimir" type="button" onclick="window.print()">Imprimir Cuestionario</button>
				<button type="button" id="btnImprimirVacio" onclick="window.open('mbti_para_imprimir_a4.html', '_blank')">Imprimir Vacío</button>
            </div>
        </form>
    </div>

    <script src="../js/funcgral.js"></script>
    <script src="../js/mbti.js"></script>
</body>
</html>
```

### Archivo: `html/mbti_para_imprimir_a4.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>MBTI - Imprimir A4 (3 columnas x 6 filas)</title>
    <style>
        /* Página A4 y margen superior amplio para evitar recortes */
        @page { size: A4; margin: 30mm 12mm 12mm 12mm; }
        @media print {
            html, body { height: auto; }
            body { margin: 0; padding: 0; }
            .wrapper { max-width: none; }
            /* Forzar que cada .page ocupe exactamente la hoja imprimible y evitar cortes internos */
            .page {
                box-sizing: border-box;
                page-break-after: always;
                break-after: page;
                page-break-inside: avoid;
                break-inside: avoid;
                /* min-height en vez de height fija: si el contenido no entra, la hoja
                   crece en vez de desbordarse y superponerse con la hoja siguiente.
                   Se resta un pequeño margen de seguridad para evitar que el redondeo
                   del motor de impresión genere una hoja en blanco extra. */
                min-height: calc(297mm - 30mm - 12mm - 4mm);
                padding: 6mm 8mm 6mm 8mm;
                -webkit-print-color-adjust: exact;
            }
            /* Quitar el top-safe en impresión para no sumar espacio extra */
            .page::before { display: none !important; }
            /* Filas de la grilla: dejar que cada fila tome el alto que necesite su contenido
               en vez de forzar una altura total que provoque desbordes */
            .page-grid { grid-template-rows: repeat(6, minmax(min-content, 1fr)); }
            .pregunta { page-break-inside: avoid; break-inside: avoid; overflow: visible; }
            /* Ocultar controles y formulario global en la versión impresa/PDF */
            .global-header, .controls { display: none !important; }
            /* Ocultar el título general al imprimir: cada hoja ya trae su propio
               encabezado (Nombre/Cédula/Fecha), y dejarlo generaba una hoja en blanco inicial */
            header { display: none !important; }
            /* No forzar salto de página después de la última hoja (evita una hoja final en blanco) */
            .page:last-child { page-break-after: auto !important; break-after: auto !important; }
            /* Quitar el margen entre hojas en impresión: el salto de página ya las separa,
               y ese margen sumado a la altura de la hoja provocaba un ligero desborde */
            .page { margin-bottom: 0 !important; }
        }

        body { font-family: Arial,Helvetica,sans-serif; background: #f2f2f2; color:#222; padding: 10px; }
        .wrapper { width: 100%; max-width: 210mm; margin: 0 auto; }
        header { text-align:center; margin-bottom: 8mm; }
        header h1 { margin:0 0 24px 0; font-size:18pt; color:#2a7a2a; }
        .global-header { margin-top:24px; display:flex; gap:8px; justify-content:center; align-items:center; flex-wrap:wrap; }

        /* Cada página imprimible */
        .page { background: white; box-sizing: border-box; padding: 8mm 10mm 10mm 10mm; margin-bottom: 8mm; border: 1px solid transparent; }
        /* Top-safe: espacio extra por si el motor recorta la parte superior */
        .page::before { content: ''; display:block; height: 6mm; }

        /* Encabezado imprimible por hoja (líneas para completar a mano) */
        /* VERSIÓN: Oculto en pantalla y mostrado solo al imprimir (como en la primera versión) */
        .sheet-header-print { display:none !important; gap:8px; align-items:center; margin-bottom:6px; font-size:10pt; flex-wrap:wrap; }
        .sheet-header-print .print-line { display:inline-block; min-width:220px; border-bottom:1px solid #000; padding-bottom:3px; color: #000; }
        .sheet-header-print .print-field { min-width:220px; display:inline-block; }
        .sheet-header-print .print-label { margin-right:6px; color:#222; font-weight:500; }
        .sheet-header-print .print-field-small { min-width:90px; }
        @media screen {
            .sheet-header-print .print-line { border-bottom:1px dashed #bbb; color:#333; }
        }
        @media print {
            /* Mostrar encabezado por hoja solamente al imprimir */
            .sheet-header-print { display:flex !important; }
            /* Numeración de hoja (visible en impresión) */
            .sheet-page-number { font-weight:600; font-size:10pt; margin-left:12px; color:#222; white-space:nowrap; }
        }

        /* Cuadrícula: 3 columnas, 6 filas (18 items por página)*/
        .page-grid { display: grid; grid-template-columns: repeat(3, 1fr); grid-auto-rows: minmax(34px, auto); gap: 5px; grid-auto-flow: row; }

        /* Cada pregunta ocupa una celda. Evitar cortes dentro del elemento */
        .pregunta { border: 1px solid #cfe9cf; background:#f9fff9; padding:4px 6px; border-radius:4px; break-inside: avoid; -webkit-column-break-inside: avoid; }
        .pregunta h2 { margin:0 0 3px 0; font-size:9pt; line-height:1.15; color:#1f5d1f; font-weight:600; }
        /* Opciones: organizar en tres filas para alinear radios / números / leyendas */
        .opciones { display:block; font-size:8pt; }
        .opciones .op-row { display:grid; grid-template-columns: repeat(5, minmax(30px, 1fr)); gap:4px; align-items:center; text-align:center; margin:2px 0; }
        .opciones .op-row.radios input { transform: scale(1); margin:2px 0; }
        .opciones .op-row.numeros { font-size:8pt; color:#111; }
        .opciones .op-row.leyendas { font-size:6.5pt; line-height:1.1; color:#333; }
        /* Mantener compatibilidad con impresión: todo se imprime igual */

        /* Controles */
        .controls { margin:10px 0; text-align:center; }
        .controls button { padding:10px 16px; background:#2a7a2a; color:white; border:none; border-radius:6px; cursor:pointer; }
        .controls button:hover { background:#245f24; }

        /* Visualización en pantalla: limitar páginas para no llenar demasiado */
        @media screen {
            .page { box-shadow:0 6px 24px rgba(0,0,0,0.08); margin-bottom:18px; }
        }

        /* Asegurar que el grid respete el flujo por filas (left->center->right) */
        .page-grid { grid-auto-flow: row; }
    </style>
</head>
<body>
    <div class="wrapper">
        <header>
            <h1>Cuestionario - Versión Imprimible A4</h1>
            
            <div class="global-header" style="margin-top:6px; display:flex; gap:8px; justify-content:center; align-items:center; flex-wrap:wrap;">
                <label>Nombre: <input id="globalNombre" type="text" style="width:320px;"></label>
                <label>Cédula: <input id="globalCedula" type="text" style="width:120px;"></label>
                <label>Fecha: <input id="globalFecha" type="date" style="width:150px;"></label>
                <label style="font-size:0.9em;"><input type="checkbox" id="copyAll" checked> Copiar a todas las hojas</label>
            </div>
        </header>

        <div class="controls">
            <label style="font-size:0.9em; color:#333; margin-right:12px;">Preguntas por página: <strong id="perPageLabel">18</strong></label>
            <button id="btnImprimir3x6">Imprimir / Generar PDF (3x6)</button>
            <button id="btnImprimir3x9" style="margin-left:8px;">Imprimir / Generar PDF (3x9)</button>
        </div>

        <div id="pagesContainer"></div>
    </div>

<script>
    // Cargar las preguntas desde el archivo principal (copiadas aquí para independencia)
    const grupo1_EI = [
        { texto: "Me siento lleno de energía después de socializar.", grupo: 1 },
        { texto: "Prefiero participar activamente en conversaciones de grupo.", grupo: 1 },
        { texto: "Me resulta fácil iniciar una conversación con desconocidos.", grupo: 1 },
        { texto: "A menudo busco la compañía de otros.", grupo: 1 },
        { texto: "Me gustan los ambientes ruidosos y concurridos.", grupo: 1 },
        { texto: "Soy más propenso a hablar que a escuchar.", grupo: 1 },
        { texto: "Disfruto expresando mis ideas abiertamente en público.", grupo: 1 },
        { texto: "Me considero una persona sociable y animada.", grupo: 1 },
        { texto: "Necesito mucha interacción social para sentirme bien.", grupo: 1 },
        { texto: "El aislamiento social me cansa rápidamente.", grupo: 1 },
        { texto: "Me adapto fácilmente a nuevos grupos sociales.", grupo: 1 },
        { texto: "Mis amigos me consideran extrovertido.", grupo: 1 },
        { texto: "Tomo la iniciativa en actividades sociales.", grupo: 1 },
        { texto: "Prefiero hacer cosas con mucha gente.", grupo: 1 },
        { texto: "Me gusta ser el centro de atención.", grupo: 1 },
        { texto: "Soy muy accesible para la mayoría de la gente.", grupo: 1 },
        { texto: "Me gusta debatir temas complejos en voz alta.", grupo: 1 },
        { texto: "Las reuniones grandes me resultan energizantes.", grupo: 1 },
        { texto: "Me aburro si estoy solo por mucho tiempo.", grupo: 1 },
        { texto: "Me expreso con facilidad y entusiasmo.", grupo: 1 },
        { texto: "Suelo actuar antes de pensar demasiado.", grupo: 1 },
        { texto: "Soy impulsivo y me gusta la espontaneidad social.", grupo: 1 },
        { texto: "La rutina me agobia; prefiero la novedad y la acción.", grupo: 1 },
        { texto: "Tengo un círculo amplio de conocidos.", grupo: 1 },
        { texto: "Siempre tengo algo que decir.", grupo: 1 }
    ];

    const grupo2_SN = [
        { texto: "Me concentro en los hechos y detalles tal como son.", grupo: 2 },
        { texto: "Confío en la experiencia y los datos verificables.", grupo: 2 },
        { texto: "Me preocupo más por el presente que por las posibilidades futuras.", grupo: 2 },
        { texto: "Soy práctico y me gusta lo concreto.", grupo: 2 },
        { texto: "Prefiero instrucciones claras y directas, paso a paso.", grupo: 2 },
        { texto: "Mi memoria para los detalles es excelente.", grupo: 2 },
        { texto: "Valoro el sentido común y la realidad inmediata.", grupo: 2 },
        { texto: "Busco soluciones que ya han sido probadas.", grupo: 2 },
        { texto: "Me gusta usar métodos y herramientas establecidos.", grupo: 2 },
        { texto: "Disfruto trabajando con datos tangibles.", grupo: 2 },
        { texto: "Encuentro que los temas abstractos son irrelevantes.", grupo: 2 },
        { texto: "Soy observador de lo que sucede a mi alrededor.", grupo: 2 },
        { texto: "Soy muy minucioso en mis tareas.", grupo: 2 },
        { texto: "Me baso en mis cinco sentidos para tomar decisiones.", grupo: 2 },
        { texto: "Prefiero mantener los pies en la tierra.", grupo: 2 },
        { texto: "Las metáforas y analogías me confunden.", grupo: 2 },
        { texto: "Me gusta ver el panorama completo antes de empezar.", grupo: 2 },
        { texto: "Soy bueno siguiendo un plan de trabajo detallado.", grupo: 2 },
        { texto: "Las teorías no me interesan mucho, a menos que sean aplicables.", grupo: 2 },
        { texto: "Tiendo a ser realista sobre mis expectativas.", grupo: 2 },
        { texto: "Recuerdo mejor los procedimientos que las ideas.", grupo: 2 },
        { texto: "Me molesta cuando la gente se va por las ramas.", grupo: 2 },
        { texto: "Tomo las cosas de forma literal.", grupo: 2 },
        { texto: "Busco la certeza en la información.", grupo: 2 },
        { texto: "Me enfoco en un problema a la vez, con atención.", grupo: 2 }
    ];

    const grupo3_TF = [
        { texto: "La lógica y la razón siempre deben guiar mis decisiones.", grupo: 3 },
        { texto: "Me considero una persona objetiva y crítica.", grupo: 3 },
        { texto: "Busco la justicia y la equidad por encima de la armonía grupal.", grupo: 3 },
        { texto: "Prefiero ser honesto aunque hiera sentimientos.", grupo: 3 },
        { texto: "Mi fortaleza reside en el análisis imparcial de los problemas.", grupo: 3 },
        { texto: "Al decidir, me baso en principios y reglas.", grupo: 3 },
        { texto: "Me resulta fácil ver los errores o fallos en un argumento.", grupo: 3 },
        { texto: "Me concentro en la verdad, sin dejarme llevar por la compasión.", grupo: 3 },
        { texto: "Las críticas no suelen afectarme personalmente.", grupo: 3 },
        { texto: "Suelo ser más reservado con mis emociones.", grupo: 3 },
        { texto: "Busco soluciones que sean técnicamente sólidas y eficientes.", grupo: 3 },
        { texto: "Creo que el respeto se gana con competencia.", grupo: 3 },
        { texto: "La eficiencia es crucial en mi trabajo.", grupo: 3 },
        { texto: "Me siento más cómodo debatiendo que consolando.", grupo: 3 },
        { texto: "Pienso que los sentimientos entorpecen las decisiones.", grupo: 3 },
        { texto: "Mantengo la calma en situaciones de conflicto.", grupo: 3 },
        { texto: "Es más importante la verdad que la diplomacia.", grupo: 3 },
        { texto: "Soy capaz de ser duro si la situación lo requiere.", grupo: 3 },
        { texto: "Mis críticas son directas y al grano.", grupo: 3 },
        { texto: "Me enorgullece mi capacidad de razonamiento frío.", grupo: 3 },
        { texto: "Trato a todos de la misma manera, aplicando reglas.", grupo: 3 },
        { texto: "Me es difícil entender reacciones emocionales exageradas.", grupo: 3 },
        { texto: "Soy mejor para solucionar problemas que para mediar.", grupo: 3 },
        { texto: "Me mantengo distante de los conflictos personales.", grupo: 3 },
        { texto: "La lógica es la herramienta más valiosa.", grupo: 3 }
    ];

    const grupo4_JP = [
        { texto: "Soy una persona organizada y planifico mi trabajo con antelación.", grupo: 4 },
        { texto: "Prefiero tener las cosas resueltas y cerradas cuanto antes.", grupo: 4 },
        { texto: "Me gusta seguir un horario y una lista de tareas.", grupo: 4 },
        { texto: "Trabajo mejor cuando tengo fechas límite claras.", grupo: 4 },
        { texto: "No me gusta dejar las decisiones para última hora.", grupo: 4 },
        { texto: "Disfruto terminando proyectos y pasando al siguiente.", grupo: 4 },
        { texto: "Me siento incómodo con los cambios de último momento.", grupo: 4 },
        { texto: "Tengo un plan para casi todo.", grupo: 4 },
        { texto: "Mis espacios de trabajo suelen estar ordenados y limpios.", grupo: 4 },
        { texto: "Busco la estructura, el orden y la previsibilidad.", grupo: 4 },
        { texto: "Creo en hacer el trabajo antes que el ocio (trabajo primero, juego después).", grupo: 4 },
        { texto: "Soy puntual y valoro la puntualidad de los demás.", grupo: 4 },
        { texto: "Cuando viajo, sigo un itinerario detallado.", grupo: 4 },
        { texto: "Me siento mejor una vez que he tomado una decisión final.", grupo: 4 },
        { texto: "Me gusta tener las cosas bajo control.", grupo: 4 },
        { texto: "La improvisación me estresa.", grupo: 4 },
        { texto: "Considero que la planificación es clave para el éxito.", grupo: 4 },
        { texto: "Soy muy responsable con mis compromisos.", grupo: 4 },
        { texto: "Me molesta la ineficiencia y la desorganización.", grupo: 4 },
        { texto: "Prefiero la anticipación a la sorpresa.", grupo: 4 },
        { texto: "Mis metas están definidas y son claras.", grupo: 4 },
        { texto: "Suelo empezar a trabajar mucho antes de la fecha límite.", grupo: 4 },
        { texto: "La ambigüedad me resulta frustrante.", grupo: 4 },
        { texto: "Me siento satisfecho cuando mi entorno está en orden.", grupo: 4 },
        { texto: "Tengo un sistema para organizar mis cosas.", grupo: 4 }
    ];

    const preguntas = [...grupo1_EI, ...grupo2_SN, ...grupo3_TF, ...grupo4_JP];

    // RENDER: paginar en bloques de 18 (3x6) y respetar orden por filas (left->center->right)
    const perPage = 18;
    const pagesContainer = document.getElementById('pagesContainer');

    function escapeHtmlAttr(s){ return String(s).replace(/&/g,'&amp;').replace(/"/g,'&quot;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

    function buildPages() {
        pagesContainer.innerHTML = '';
        const copyAll = document.getElementById('copyAll')?.checked;
        const globalNombre = document.getElementById('globalNombre')?.value || '';
        const globalCedula = document.getElementById('globalCedula')?.value || '';
        const globalFecha = document.getElementById('globalFecha')?.value || '';
        const totalPages = Math.ceil(preguntas.length / perPage); // Total de hojas (para numeración) 

        for (let i = 0, pageIdx = 0; i < preguntas.length; i += perPage, pageIdx++) {
            const page = document.createElement('div');
            page.className = 'page';

            // Cabecera por hoja (Nombre / Cédula / Fecha)
            const headerDiv = document.createElement('div');
            headerDiv.className = 'sheet-header-print';
            const nombreVal = (copyAll && globalNombre) ? escapeHtml(globalNombre) : '';
            const cedulaVal = (copyAll && globalCedula) ? escapeHtml(globalCedula) : '';
            const fechaVal = (copyAll && globalFecha) ? escapeHtml(globalFecha) : '';
            headerDiv.innerHTML = `
                <div class="sheet-header-print" style="display:flex; justify-content:space-between; align-items:center;">
                    <div style="display:flex; gap:12px; align-items:center;">
                        <div><span class="print-label">Nombre:</span> <span class="print-line print-field">${nombreVal || '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'}</span></div>
                        <div><span class="print-label">Cédula:</span> <span class="print-line print-field-small">${cedulaVal || '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'}</span></div>
                        <div><span class="print-label">Fecha:</span> <span class="print-line print-field-small">${fechaVal || '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'}</span></div>
                    </div>
                    <div class="sheet-page-number">Hoja ${pageIdx + 1}/${totalPages}</div>
                </div>
            `;
            page.appendChild(headerDiv);

            const grid = document.createElement('div');
            grid.className = 'page-grid';

            const slice = preguntas.slice(i, i + perPage);
            // Render en orden natural — con grid-auto-flow: row esto llenará fila por fila
            slice.forEach((p, idx) => {
                const q = document.createElement('div');
                q.className = 'pregunta';
                const numero = i + idx + 1;

                const radios = [`<label><input type="radio" name="pregunta${numero}" value="1"></label>`,
                                `<label><input type="radio" name="pregunta${numero}" value="2"></label>`,
                                `<label><input type="radio" name="pregunta${numero}" value="3"></label>`,
                                `<label><input type="radio" name="pregunta${numero}" value="4"></label>`,
                                `<label><input type="radio" name="pregunta${numero}" value="5"></label>`].join('');

                const numeros = ['1','2','3','4','5'].map(n => `<div>${n}</div>`).join('');

                const leyendas = [`(Muy en desacuerdo)`,``, `(Neutro)`, ``, `(Muy de acuerdo)`].map(t => `<div>${t}</div>`).join('');

                q.innerHTML = `<h2>${numero}. ${escapeHtml(p.texto)}</h2>` +
                              `<div class="opciones">` +
                                `<div class="op-row radios">${radios}</div>` +
                                `<div class="op-row numeros">${numeros}</div>` +
                                `<div class="op-row leyendas">${leyendas}</div>` +
                              `</div>`;
                grid.appendChild(q);
            });
            page.appendChild(grid);
            pagesContainer.appendChild(page);
            }

        // Si seleccionaron copiar a todas, propagar valores
        propagateGlobalToPages();
    }

    function propagateGlobalToPages(){
        const copyAll = document.getElementById('copyAll')?.checked;
        const n = document.getElementById('globalNombre')?.value || '';
        const c = document.getElementById('globalCedula')?.value || '';
        const f = document.getElementById('globalFecha')?.value || '';
        if(!copyAll) return;
        document.querySelectorAll('.page').forEach(page=>{
            const sN = page.querySelector('.print-line.print-field'); if(sN) sN.innerHTML = n ? escapeHtml(n) : '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;';
            const sC = page.querySelector('.print-line.print-field-small'); if(sC) sC.innerHTML = c ? escapeHtml(c) : '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;';
            const sF = page.querySelectorAll('.print-line.print-field-small')[1]; if(sF) sF.innerHTML = f ? escapeHtml(f) : '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;';
        });
    }

    function escapeHtml(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

    // Propagar valores globales cuando cambien
    ['globalNombre','globalCedula','globalFecha','copyAll'].forEach(id => {
        const el = document.getElementById(id);
        if(el) el.addEventListener('input', () => propagateGlobalToPages());
    });

    document.getElementById('btnImprimir3x6').addEventListener('click', () => {
        // actualizar antes de imprimir (3x6)
        buildPages();
        window.print();
    });

    // Abrir la versión 3x9 en una nueva pestaña y, si se solicita, iniciar impresión automática
    document.getElementById('btnImprimir3x9').addEventListener('click', () => {
        const url = 'mbti_para_imprimir_a4_3X9.html?print=true';
        window.open(url, '_blank');
    });

    // Volver a construir y propagar antes de imprimir por si cambia el layout
    window.addEventListener('beforeprint', () => { buildPages(); propagateGlobalToPages(); });

    // Inicializar
    buildPages();
</script>
</body>
</html>
```

### Archivo: `html/mbti_para_imprimir_a4_3X9.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>MBTI - Imprimir A4 (3 columnas x 9 filas)</title>
    <style>
        /* Página A4 y margen superior amplio para evitar recortes */
        @page { size: A4; margin: 30mm 12mm 12mm 12mm; }
        @media print {
            html, body { height: auto; }
            body { margin: 0; padding: 0; }
            .wrapper { max-width: none; }
            /* Forzar que cada .page ocupe exactamente la hoja imprimible y evitar cortes internos */
            .page {
                box-sizing: border-box;
                page-break-after: always;
                break-after: page;
                page-break-inside: avoid;
                break-inside: avoid;
                /* min-height en vez de height fija: si el contenido no entra, la hoja
                   crece en vez de desbordarse y superponerse con la hoja siguiente.
                   Se resta un pequeño margen de seguridad para evitar que el redondeo
                   del motor de impresión genere una hoja en blanco extra. */
                min-height: calc(297mm - 30mm - 12mm - 4mm);
                padding: 6mm 8mm 6mm 8mm;
                -webkit-print-color-adjust: exact;
            }
            /* Quitar el top-safe en impresión para no sumar espacio extra */
            .page::before { display: none !important; }
            /* Filas de la grilla: dejar que cada fila tome el alto que necesite su contenido
               en vez de forzar una altura total que provoque desbordes */
            .page-grid { grid-template-rows: repeat(9, minmax(min-content, 1fr)); }
            .pregunta { page-break-inside: avoid; break-inside: avoid; overflow: visible; }
            /* Ocultar controles y formulario global en la versión impresa/PDF */
            .global-header, .controls { display: none !important; }
            /* Ocultar el título general al imprimir: cada hoja ya trae su propio
               encabezado (Nombre/Cédula/Fecha), y dejarlo generaba una hoja en blanco inicial */
            header { display: none !important; }
            /* No forzar salto de página después de la última hoja (evita una hoja final en blanco) */
            .page:last-child { page-break-after: auto !important; break-after: auto !important; }
            /* Quitar el margen entre hojas en impresión: el salto de página ya las separa,
               y ese margen sumado a la altura de la hoja provocaba un ligero desborde */
            .page { margin-bottom: 0 !important; }
        }

        body { font-family: Arial,Helvetica,sans-serif; background: #f2f2f2; color:#222; padding: 10px; }
        .wrapper { width: 100%; max-width: 210mm; margin: 0 auto; }
        header { text-align:center; margin-bottom: 8mm; }
        header h1 { margin:0 0 24px 0; font-size:18pt; color:#2a7a2a; }
        .global-header { margin-top:24px; display:flex; gap:8px; justify-content:center; align-items:center; flex-wrap:wrap; }

        /* Cada página imprimible */
        .page { background: white; box-sizing: border-box; padding: 8mm 10mm 10mm 10mm; margin-bottom: 8mm; border: 1px solid transparent; }
        /* Top-safe: espacio extra por si el motor recorta la parte superior */
        .page::before { content: ''; display:block; height: 6mm; }

        /* Encabezado imprimible por hoja (líneas para completar a mano) */
        .sheet-header-print { display:none !important; gap:8px; align-items:center; margin-bottom:6px; font-size:10pt; flex-wrap:wrap; }
        .sheet-header-print .print-line { display:inline-block; min-width:220px; border-bottom:1px solid #000; padding-bottom:3px; color: #000; }
        .sheet-header-print .print-field { min-width:220px; display:inline-block; }
        .sheet-header-print .print-label { margin-right:6px; color:#222; font-weight:500; }
        .sheet-header-print .print-field-small { min-width:90px; }
        @media screen {
            .sheet-header-print .print-line { border-bottom:1px dashed #bbb; color:#333; }
        }
        @media print {
            /* Mostrar encabezado por hoja solamente al imprimir */
            .sheet-header-print { display:flex !important; }
            /* Numeración de hoja (visible en impresión) */
            .sheet-page-number { font-weight:600; font-size:10pt; margin-left:12px; color:#222; white-space:nowrap; }
        }

        /* Cuadrícula: 3 columnas, 9 filas (27 items por página)*/
        .page-grid { display: grid; grid-template-columns: repeat(3, 1fr); grid-auto-rows: minmax(30px, auto); gap: 5px; grid-auto-flow: row; }

        /* Cada pregunta ocupa una celda. Evitar cortes dentro del elemento */
        .pregunta { border: 1px solid #cfe9cf; background:#f9fff9; padding:4px 6px; border-radius:4px; break-inside: avoid; -webkit-column-break-inside: avoid; }
        .pregunta h2 { margin:0 0 3px 0; font-size:9pt; line-height:1.15; color:#1f5d1f; font-weight:600; }
        .opciones { display:flex; justify-content:space-between; font-size:8pt; }
        .opciones label { display:flex; flex-direction:column; align-items:center; font-weight:400; }
        .opciones input { margin-top:2px; transform: scale(1); }
        /* Mantener compatibilidad con impresión: todo se imprime igual */

        /* Controles */
        .controls { margin:10px 0; text-align:center; }
        .controls button { padding:10px 16px; background:#2a7a2a; color:white; border:none; border-radius:6px; cursor:pointer; }
        .controls button:hover { background:#245f24; }

        /* Visualización en pantalla: limitar páginas para no llenar demasiado */
        @media screen {
            .page { box-shadow:0 6px 24px rgba(0,0,0,0.08); margin-bottom:18px; }
        }

        /* Asegurar que el grid respete el flujo por filas (left->center->right) */
        .page-grid { grid-auto-flow: row; }
    </style>
</head>
<body>
    <div class="wrapper">
        <header>
            <h1>Cuestionario - Versión Imprimible A4 (3x9)</h1>
            
            <div class="global-header" style="margin-top:6px; display:flex; gap:8px; justify-content:center; align-items:center; flex-wrap:wrap;">
                <label>Nombre: <input id="globalNombre" type="text" style="width:320px;"></label>
                <label>Cédula: <input id="globalCedula" type="text" style="width:120px;"></label>
                <label>Fecha: <input id="globalFecha" type="date" style="width:150px;"></label>
                <label style="font-size:0.9em;"><input type="checkbox" id="copyAll" checked> Copiar a todas las hojas</label>
            </div>
        </header>

        <div class="controls">
            <label style="font-size:0.9em; color:#333; margin-right:12px;">Preguntas por página: <strong id="perPageLabel">27</strong></label>
            <button id="btnImprimir">Imprimir / Generar PDF</button>
        </div>

        <div id="pagesContainer"></div>
    </div>

<script>
    // Cargar las preguntas desde el archivo principal (copiadas aquí para independencia)
    const grupo1_EI = [
        { texto: "Me siento lleno de energía después de socializar.", grupo: 1 },
        { texto: "Prefiero participar activamente en conversaciones de grupo.", grupo: 1 },
        { texto: "Me resulta fácil iniciar una conversación con desconocidos.", grupo: 1 },
        { texto: "A menudo busco la compañía de otros.", grupo: 1 },
        { texto: "Me gustan los ambientes ruidosos y concurridos.", grupo: 1 },
        { texto: "Soy más propenso a hablar que a escuchar.", grupo: 1 },
        { texto: "Disfruto expresando mis ideas abiertamente en público.", grupo: 1 },
        { texto: "Me considero una persona sociable y animada.", grupo: 1 },
        { texto: "Necesito mucha interacción social para sentirme bien.", grupo: 1 },
        { texto: "El aislamiento social me cansa rápidamente.", grupo: 1 },
        { texto: "Me adapto fácilmente a nuevos grupos sociales.", grupo: 1 },
        { texto: "Mis amigos me consideran extrovertido.", grupo: 1 },
        { texto: "Tomo la iniciativa en actividades sociales.", grupo: 1 },
        { texto: "Prefiero hacer cosas con mucha gente.", grupo: 1 },
        { texto: "Me gusta ser el centro de atención.", grupo: 1 },
        { texto: "Soy muy accesible para la mayoría de la gente.", grupo: 1 },
        { texto: "Me gusta debatir temas complejos en voz alta.", grupo: 1 },
        { texto: "Las reuniones grandes me resultan energizantes.", grupo: 1 },
        { texto: "Me aburro si estoy solo por mucho tiempo.", grupo: 1 },
        { texto: "Me expreso con facilidad y entusiasmo.", grupo: 1 },
        { texto: "Suelo actuar antes de pensar demasiado.", grupo: 1 },
        { texto: "Soy impulsivo y me gusta la espontaneidad social.", grupo: 1 },
        { texto: "La rutina me agobia; prefiero la novedad y la acción.", grupo: 1 },
        { texto: "Tengo un círculo amplio de conocidos.", grupo: 1 },
        { texto: "Siempre tengo algo que decir.", grupo: 1 }
    ];

    const grupo2_SN = [
        { texto: "Me concentro en los hechos y detalles tal como son.", grupo: 2 },
        { texto: "Confío en la experiencia y los datos verificables.", grupo: 2 },
        { texto: "Me preocupo más por el presente que por las posibilidades futuras.", grupo: 2 },
        { texto: "Soy práctico y me gusta lo concreto.", grupo: 2 },
        { texto: "Prefiero instrucciones claras y directas, paso a paso.", grupo: 2 },
        { texto: "Mi memoria para los detalles es excelente.", grupo: 2 },
        { texto: "Valoro el sentido común y la realidad inmediata.", grupo: 2 },
        { texto: "Busco soluciones que ya han sido probadas.", grupo: 2 },
        { texto: "Me gusta usar métodos y herramientas establecidos.", grupo: 2 },
        { texto: "Disfruto trabajando con datos tangibles.", grupo: 2 },
        { texto: "Encuentro que los temas abstractos son irrelevantes.", grupo: 2 },
        { texto: "Soy observador de lo que sucede a mi alrededor.", grupo: 2 },
        { texto: "Soy muy minucioso en mis tareas.", grupo: 2 },
        { texto: "Me baso en mis cinco sentidos para tomar decisiones.", grupo: 2 },
        { texto: "Prefiero mantener los pies en la tierra.", grupo: 2 },
        { texto: "Las metáforas y analogías me confunden.", grupo: 2 },
        { texto: "Me gusta ver el panorama completo antes de empezar.", grupo: 2 },
        { texto: "Soy bueno siguiendo un plan de trabajo detallado.", grupo: 2 },
        { texto: "Las teorías no me interesan mucho, a menos que sean aplicables.", grupo: 2 },
        { texto: "Tiendo a ser realista sobre mis expectativas.", grupo: 2 },
        { texto: "Recuerdo mejor los procedimientos que las ideas.", grupo: 2 },
        { texto: "Me molesta cuando la gente se va por las ramas.", grupo: 2 },
        { texto: "Tomo las cosas de forma literal.", grupo: 2 },
        { texto: "Busco la certeza en la información.", grupo: 2 },
        { texto: "Me enfoco en un problema a la vez, con atención.", grupo: 2 }
    ];

    const grupo3_TF = [
        { texto: "La lógica y la razón siempre deben guiar mis decisiones.", grupo: 3 },
        { texto: "Me considero una persona objetiva y crítica.", grupo: 3 },
        { texto: "Busco la justicia y la equidad por encima de la armonía grupal.", grupo: 3 },
        { texto: "Prefiero ser honesto aunque hiera sentimientos.", grupo: 3 },
        { texto: "Mi fortaleza reside en el análisis imparcial de los problemas.", grupo: 3 },
        { texto: "Al decidir, me baso en principios y reglas.", grupo: 3 },
        { texto: "Me resulta fácil ver los errores o fallos en un argumento.", grupo: 3 },
        { texto: "Me concentro en la verdad, sin dejarme llevar por la compasión.", grupo: 3 },
        { texto: "Las críticas no suelen afectarme personalmente.", grupo: 3 },
        { texto: "Suelo ser más reservado con mis emociones.", grupo: 3 },
        { texto: "Busco soluciones que sean técnicamente sólidas y eficientes.", grupo: 3 },
        { texto: "Creo que el respeto se gana con competencia.", grupo: 3 },
        { texto: "La eficiencia es crucial en mi trabajo.", grupo: 3 },
        { texto: "Me siento más cómodo debatiendo que consolando.", grupo: 3 },
        { texto: "Pienso que los sentimientos entorpecen las decisiones.", grupo: 3 },
        { texto: "Mantengo la calma en situaciones de conflicto.", grupo: 3 },
        { texto: "Es más importante la verdad que la diplomacia.", grupo: 3 },
        { texto: "Soy capaz de ser duro si la situación lo requiere.", grupo: 3 },
        { texto: "Mis críticas son directas y al grano.", grupo: 3 },
        { texto: "Me enorgullece mi capacidad de razonamiento frío.", grupo: 3 },
        { texto: "Trato a todos de la misma manera, aplicando reglas.", grupo: 3 },
        { texto: "Me es difícil entender reacciones emocionales exageradas.", grupo: 3 },
        { texto: "Soy mejor para solucionar problemas que para mediar.", grupo: 3 },
        { texto: "Me mantengo distante de los conflictos personales.", grupo: 3 },
        { texto: "La lógica es la herramienta más valiosa.", grupo: 3 }
    ];

    const grupo4_JP = [
        { texto: "Soy una persona organizada y planifico mi trabajo con antelación.", grupo: 4 },
        { texto: "Prefiero tener las cosas resueltas y cerradas cuanto antes.", grupo: 4 },
        { texto: "Me gusta seguir un horario y una lista de tareas.", grupo: 4 },
        { texto: "Trabajo mejor cuando tengo fechas límite claras.", grupo: 4 },
        { texto: "No me gusta dejar las decisiones para última hora.", grupo: 4 },
        { texto: "Disfruto terminando proyectos y pasando al siguiente.", grupo: 4 },
        { texto: "Me siento incómodo con los cambios de último momento.", grupo: 4 },
        { texto: "Tengo un plan para casi todo.", grupo: 4 },
        { texto: "Mis espacios de trabajo suelen estar ordenados y limpios.", grupo: 4 },
        { texto: "Busco la estructura, el orden y la previsibilidad.", grupo: 4 },
        { texto: "Creo en hacer el trabajo antes que el ocio (trabajo primero, juego después).", grupo: 4 },
        { texto: "Soy puntual y valoro la puntualidad de los demás.", grupo: 4 },
        { texto: "Cuando viajo, sigo un itinerario detallado.", grupo: 4 },
        { texto: "Me siento mejor una vez que he tomado una decisión final.", grupo: 4 },
        { texto: "Me gusta tener las cosas bajo control.", grupo: 4 },
        { texto: "La improvisación me estresa.", grupo: 4 },
        { texto: "Considero que la planificación es clave para el éxito.", grupo: 4 },
        { texto: "Soy muy responsable con mis compromisos.", grupo: 4 },
        { texto: "Me molesta la ineficiencia y la desorganización.", grupo: 4 },
        { texto: "Prefiero la anticipación a la sorpresa.", grupo: 4 },
        { texto: "Mis metas están definidas y son claras.", grupo: 4 },
        { texto: "Suelo empezar a trabajar mucho antes de la fecha límite.", grupo: 4 },
        { texto: "La ambigüedad me resulta frustrante.", grupo: 4 },
        { texto: "Me siento satisfecho cuando mi entorno está en orden.", grupo: 4 },
        { texto: "Tengo un sistema para organizar mis cosas.", grupo: 4 }
    ];

    const preguntas = [...grupo1_EI, ...grupo2_SN, ...grupo3_TF, ...grupo4_JP];

    // RENDER: paginar en bloques de 27 (3x9) y respetar orden por filas (left->center->right)
    const perPage = 27;
    const pagesContainer = document.getElementById('pagesContainer');

    function escapeHtmlAttr(s){ return String(s).replace(/&/g,'&amp;').replace(/"/g,'&quot;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

    function buildPages() {
        pagesContainer.innerHTML = '';
        const copyAll = document.getElementById('copyAll')?.checked;
        const globalNombre = document.getElementById('globalNombre')?.value || '';
        const globalCedula = document.getElementById('globalCedula')?.value || '';
        const globalFecha = document.getElementById('globalFecha')?.value || '';
        const totalPages = Math.ceil(preguntas.length / perPage); // Total de hojas (para numeración) 

        for (let i = 0, pageIdx = 0; i < preguntas.length; i += perPage, pageIdx++) {
            const page = document.createElement('div');
            page.className = 'page';

            // Cabecera por hoja (Nombre / Cédula / Fecha)
            const headerDiv = document.createElement('div');
            headerDiv.className = 'sheet-header-print';
            const nombreVal = (copyAll && globalNombre) ? escapeHtml(globalNombre) : '';
            const cedulaVal = (copyAll && globalCedula) ? escapeHtml(globalCedula) : '';
            const fechaVal = (copyAll && globalFecha) ? escapeHtml(globalFecha) : '';
            headerDiv.innerHTML = `
                <div class="sheet-header-print" style="display:flex; justify-content:space-between; align-items:center;">
                    <div style="display:flex; gap:12px; align-items:center;">
                        <div><span class="print-label">Nombre:</span> <span class="print-line print-field">${nombreVal || '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'}</span></div>
                        <div><span class="print-label">Cédula:</span> <span class="print-line print-field-small">${cedulaVal || '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'}</span></div>
                        <div><span class="print-label">Fecha:</span> <span class="print-line print-field-small">${fechaVal || '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'}</span></div>
                    </div>
                    <div class="sheet-page-number">Hoja ${pageIdx + 1}/${totalPages}</div>
                </div>
            `;
            page.appendChild(headerDiv);

            const grid = document.createElement('div');
            grid.className = 'page-grid';

            const slice = preguntas.slice(i, i + perPage);
            // Render en orden natural — con grid-auto-flow: row esto llenará fila por fila
            slice.forEach((p, idx) => {
                const q = document.createElement('div');
                q.className = 'pregunta';
                const numero = i + idx + 1;

                q.innerHTML = `<h2>${numero}. ${escapeHtml(p.texto)}</h2>` +
                    `<div class="opciones">` +
                    `<label>1<br><input type="radio" name="pregunta${numero}" value="1"></label>` +
                    `<label>2<br><input type="radio" name="pregunta${numero}" value="2"></label>` +
                    `<label>3<br><input type="radio" name="pregunta${numero}" value="3"></label>` +
                    `<label>4<br><input type="radio" name="pregunta${numero}" value="4"></label>` +
                    `<label>5<br><input type="radio" name="pregunta${numero}" value="5"></label>` +
                    `</div>`;
                grid.appendChild(q);
            });
            page.appendChild(grid);
            pagesContainer.appendChild(page);
            }

        // Si seleccionaron copiar a todas, propagar valores
        propagateGlobalToPages();
    }

    function propagateGlobalToPages(){
        const copyAll = document.getElementById('copyAll')?.checked;
        const n = document.getElementById('globalNombre')?.value || '';
        const c = document.getElementById('globalCedula')?.value || '';
        const f = document.getElementById('globalFecha')?.value || '';
        if(!copyAll) return;
        document.querySelectorAll('.page').forEach(page=>{
            const sN = page.querySelector('.print-line.print-field'); if(sN) sN.innerHTML = n ? escapeHtml(n) : '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;';
            const sC = page.querySelector('.print-line.print-field-small'); if(sC) sC.innerHTML = c ? escapeHtml(c) : '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;';
            const sF = page.querySelectorAll('.print-line.print-field-small')[1]; if(sF) sF.innerHTML = f ? escapeHtml(f) : '&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;';
        });
    }

    function escapeHtml(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

    // Propagar valores globales cuando cambien
    ['globalNombre','globalCedula','globalFecha','copyAll'].forEach(id => {
        const el = document.getElementById(id);
        if(el) el.addEventListener('input', () => propagateGlobalToPages());
    });

    document.getElementById('btnImprimir').addEventListener('click', () => {
        // actualizar antes de imprimir
        buildPages();
        window.print();
    });

    // Si se abrió con ?print=true, iniciar impresión automática al cargar
    (function(){
        try{
            const q = new URLSearchParams(location.search);
            if(q.get('print') === 'true'){
                // construir y lanzar impresión después de renderizado
                document.addEventListener('DOMContentLoaded', ()=>{ buildPages(); setTimeout(()=>window.print(), 200); });
            }
        }catch(e){}
    })();

    // Volver a construir y propagar antes de imprimir por si cambia el layout
    window.addEventListener('beforeprint', () => { buildPages(); propagateGlobalToPages(); });

    // Inicializar
    buildPages();
</script>
</body>
</html>
```

### Archivo: `html/menu.html`
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Menú Desplegable</title>
  <link rel="stylesheet" href="../css/comunes.css">
  <link rel="stylesheet" href="../css/menu.css">
</head>
<body>
<header class="site-header">
  <div class="site-header__brand">
    <span class="site-header__logo">TF</span>
    <div class="site-header__titles">
      <span class="site-header__title">TechFaRo</span>
      <span class="site-header__subtitle">Herramientas internas</span>
    </div>
  </div>
  <div class="site-header__user">
    <span class="site-header__username" id="nombreUsuarioHeader"></span>
    <button type="button" class="site-header__logout" id="btnCerrarSesion">Cerrar sesión</button>
  </div>
</header>
<main>
<div class="container-flex">
	<div class="container" id="contIzq" >
	  <div class="theme-switcher" style="display:flex; align-items:center; justify-content:space-between; padding: 8px 12px;">
		<span class="theme-label" style="font-weight:700;">Modo oscuro</span>
		<label style="position:relative; display:inline-block; width:48px; height:28px;">
			<input type="checkbox" id="themeToggle" style="opacity:0; width:0; height:0;" />
			<span style="position:absolute; cursor:pointer; inset:0; background:#94a3b8; border-radius:999px; transition: background .2s;"></span>
			<span style="position:absolute; top:3px; left:3px; width:22px; height:22px; background:#fff; border-radius:50%; transition: transform .2s;" id="themeThumb"></span>
		</label>
	</div>
	  <nav>
	  </nav>
	  <script src="../js/menu-manifest.js"></script>
	  <script src="../js/funcgral.js"></script>
	  <script src="../js/menu.js"></script>
	  <script>
	    // Header de marca: mostrar quien inicio sesion y permitir cerrarla
	    (function () {
	      const nombre = sessionStorage.getItem('nombreUsuario');
	      const spanUsuario = document.getElementById('nombreUsuarioHeader');
	      if (spanUsuario) {
	        spanUsuario.textContent = nombre ? ('Hola, ' + nombre) : '';
	      }
	      const btnSalir = document.getElementById('btnCerrarSesion');
	      if (btnSalir) {
	        btnSalir.addEventListener('click', () => {
	          sessionStorage.removeItem('verificado');
	          sessionStorage.removeItem('nombreUsuario');
	          window.location.href = darUrl('index.html');
	        });
	      }
	    })();
	  </script>
	</div>
	<div class="container"id="contDer">
	</div>
</div>
</main>
 <footer>
    <p>Copyright © 2024 TechFaRo. Todos los derechos reservados.</p>
</footer>

</body>
</html>

```

### Archivo: `html/ovo.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>O.V.O. — Orientación Vocacional</title>
    <link rel="stylesheet" href="../css/comunes.css">
    <link rel="stylesheet" href="../css/ovo.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>

<div class="app-header">
    <div class="app-header-inner">
        <div class="logo">O.V.O.<span>Orientación Vocacional y Ocupacional</span></div>
        <button class="theme-btn" onclick="toggleTheme()" title="Cambiar tema"></button>
    </div>
</div>

<div class="container">

    <!-- WELCOME -->
    <div id="welcome" class="welcome-screen">
        <h2>Descubre tu orientación vocacional</h2>
        <p>Responde 80 preguntas sobre actividades que te gustaría realizar. Al finalizar conocerás tu perfil en 5 áreas profesionales.</p>
        <div class="welcome-card">
            <h3>¿Cómo funciona?</h3>
            <ul>
                <li>80 preguntas divididas en 8 bloques de 10</li>
                <li>Responde SI o NO a cada actividad</li>
                <li>Usa los botones o las teclas 1 (SI) y 2 (NO)</li>
                <li>Al finalizar verás tu perfil gráfico</li>
                <li>Duración estimada: 5-8 minutos</li>
            </ul>
        </div>
        <button class="btn-start" onclick="startQuiz()">Comenzar Test</button>
    </div>

    <!-- QUIZ -->
    <div id="quiz" class="hidden">
        <div class="progress-wrap">
            <div class="progress-info">
                <span id="progressText">Pregunta 1 de 80</span>
                <span id="progressPercent">0%</span>
            </div>
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill"></div>
            </div>
        </div>

        <div class="steps-dots" id="stepsDots"></div>

        <div id="questionsContainer"></div>

        <div class="nav-buttons">
            <button class="btn-nav btn-prev" id="btnPrev" onclick="prevStep()" disabled>← Anterior</button>
            <button class="btn-nav btn-print" id="btnPrint" onclick="imprimirCuestionario()">🖨️ Imprimir cuestionario</button>
            <button class="btn-nav btn-next" id="btnNext" onclick="nextStep()">Siguiente →</button>
        </div>

        <div class="keyboard-hint">💡 Atajo de teclado: <strong>1</strong> = SI &nbsp;|&nbsp; <strong>2</strong> = NO &nbsp;|&nbsp; <strong>← →</strong> = Navegar</div>
    </div>

    <!-- RESULTS -->
    <div id="results" class="results-screen hidden">
        <div class="results-header">
            <h2>🎯 Tu Perfil Vocacional</h2>
            <p>Estos son tus resultados basados en tus respuestas</p>
        </div>

        <div class="results-grid" id="resultsGrid"></div>

        <div class="chart-container">
            <canvas id="resultsChart"></canvas>
        </div>

        <div class="results-actions">
            <button class="btn-action btn-back-quiz" onclick="volverAlCuestionario()">📋 Volver al cuestionario</button>
            <button class="btn-action btn-print-quiz" onclick="imprimirCuestionarioDesdeResultados()">🖨️ Imprimir cuestionario</button>
            <button class="btn-action btn-print-full" onclick="imprimirCompleto()">📄 Imprimir cuestionario + resultados</button>
            <button class="btn-restart" onclick="location.reload()">🔄 Realizar de nuevo</button>
        </div>
    </div>

</div>

<!-- DATOS MODAL (antes de empezar) -->
<div id="datosModal" class="modal-overlay hidden">
    <div class="modal-box">
        <div class="lock-icon">📝</div>
        <h3>Datos del evaluado</h3>
        <p class="modal-sub">Ingresa los datos para incluirlos en el cuestionario (opcional)</p>
        <div class="modal-field">
            <label>Nombre completo</label>
            <input type="text" id="datosNombre" placeholder="Ej: Juan Pérez" autocomplete="off">
        </div>
        <div class="modal-field">
            <label>Cédula de identidad</label>
            <input type="text" id="datosCedula" placeholder="Ej: 1.234.567-8" autocomplete="off">
        </div>
        <div class="modal-field">
            <label>Fecha</label>
            <input type="text" id="datosFecha" placeholder="Ej: 08/08/2026" autocomplete="off">
        </div>
        <div class="modal-error" id="datosError"></div>
        <div class="modal-actions">
            <button class="modal-btn-skip" onclick="guardarDatosYOmitir()">Omitir</button>
            <button class="modal-btn-confirm" onclick="guardarDatosYOmitir()">Continuar</button>
        </div>
    </div>
</div>

<!-- LOGIN MODAL (solo para ver resultados) -->
<div id="loginModal" class="modal-overlay hidden">
    <div class="modal-box">
        <div class="lock-icon">🔒</div>
        <h3>Acceso restringido</h3>
        <p class="modal-sub">Solo personal autorizado puede ver los resultados</p>
        <div class="modal-field">
            <label>Usuario</label>
            <input type="text" id="modalUser" placeholder="Ingrese usuario" autocomplete="off">
        </div>
        <div class="modal-field">
            <label>Contraseña</label>
            <input type="password" id="modalPass" placeholder="Ingrese contraseña">
        </div>
        <div class="modal-error" id="modalError"></div>
        <div class="modal-actions">
            <button class="modal-btn-cancel" onclick="closeLoginModal()">Cancelar</button>
            <button class="modal-btn-login" onclick="checkLoginAndShow()">Ver resultados</button>
        </div>
    </div>
</div>

<script src="../js/funcgral.js"></script>
<script src="../js/ovo.js"></script>

</body>
</html>

```

### Archivo: `imagenes/fondo1.jpg`
```jpg
[Archivo Binario o No Leible]
```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
	<title>Bienvenido a WebUtilidades</title>
	<link rel="stylesheet" href="css/comunes.css">
	<link rel="stylesheet" href="css/login.css">
</head>

<body>
<main>
 <div class="container-flex">
    <div class="container" id="contIzq">
       <form id="loginForm" >
			<h1>Login:</h1>
			<label for="username">Usuario:</label>
			<input type="text" id="username" name="username" required>

			<label for="password">Contraseña:</label>
			<input type="password" id="password" name="password" required>

			<input type="submit" value="Login">
		</form>
    </div>
	<div class="container"id="contDer">	
	</div>
 </div>
	<script src="js/funcgral.js"></script> 
    <script src="js/login.js"></script>
</main>
	<footer>
		<p>Copyright © 2024 TechFaRo. Todos los derechos reservados.</p>
	</footer>
</body>

</html>
```

### Archivo: `js/_plantilla.js`
```javascript
/*
 * PLANTILLA DE JS PARA UNA HERRAMIENTA NUEVA
 * --------------------------------------------
 * Todo el código de tu herramienta va DENTRO del bloque if de acá abajo.
 * Eso es lo que hace que la página no se pueda usar sin haber iniciado
 * sesión: si alguien abre el archivo .html directo por URL sin loguearse,
 * el bloque "else" lo manda de vuelta al login.
 *
 * No hace falta tocar el chequeo en sí, solo escribir tu lógica adentro.
 */

if (sessionStorage.getItem("verificado")) {

    // ---------------------------------------------------------------
    // Escribí acá toda la lógica de tu herramienta.
    // ---------------------------------------------------------------



} else {
    // No hay sesión iniciada: volver al login
    window.location.href = darUrl('index.html');
}

```

### Archivo: `js/bolillero.js`
```javascript
let gruposGenerados;
let cantidadCedulasPorGrupo
let listaFilas;

if(sessionStorage.getItem("verificado")){

		function leerArchivoYObtenerCedulas(archivo) {
		  return new Promise((resolve, reject) => {
			const reader = new FileReader();
			reader.onload = (event) => { // Usamos 'event' en lugar de 'e'
			  const cedulas = event.target.result.split('\n').filter(cedula => cedula.trim());
			  resolve(cedulas);
			};
			reader.onerror = error => reject(error);
			reader.readAsText(archivo);
		  });
		}

		function generarGrupos(cedulas, cantidadCedulasPorGrupo) {
		  // Eliminar duplicados y crear un conjunto de cédulas únicas
		  const cedulasUnicas = [...new Set(cedulas)];
		  const cedulasUsadas = new Set();

		  const grupos = [];
		  while (cedulasUnicas.length > 0) {
			const grupo = [];
			while (grupo.length < cantidadCedulasPorGrupo && cedulasUnicas.length > 0) {
			  // Seleccionar una cédula aleatoria no utilizada
			  const indiceAleatorio = Math.floor(Math.random() * cedulasUnicas.length);
			  const cedula = cedulasUnicas[indiceAleatorio];


				if(validarCedulaUruguaya(cedula)){// Verificar si la cédula sea valida
				  
				  if (!cedulasUsadas.has(cedula)) {// Verificar si la cédula ya ha sido utilizada
					grupo.push(cedula);
					
				  }
				}
				cedulasUsadas.add(cedula);
				cedulasUnicas.splice(indiceAleatorio, 1);
			}
			grupos.push(grupo);
		  }

		  return grupos;
		}

		function mostrarResultados(grupos, listaCedulasElement) {
		  listaCedulasElement.innerHTML = '';
		 
		  grupos.forEach((grupo, index) => {
			const listItem = document.createElement('li');
			listItem.textContent = `Grupo ${index + 1}: ${grupo.join(', ')}`;
			listaCedulasElement.appendChild(listItem);
		  });
		}

	
	async function generarListas() {
		  const archivo = document.getElementById('archivoCedulas').files[0];
		   cantidadCedulasPorGrupo = parseInt(document.getElementById('cantidadCedulasPorGrupo').value);
		  const listaCedulasElement = document.getElementById('listaCedulas');
		  const mensajeDiv = document.getElementById('mensaje');

		  if (!archivo) {
			mensajeDiv.textContent = 'Por favor, seleccione un archivo.';
			return;
		  }

		  try {
			const cedulas = await leerArchivoYObtenerCedulas(archivo);
			const grupos = generarGrupos(cedulas, cantidadCedulasPorGrupo);
			mostrarResultados(grupos, listaCedulasElement);
			// Almacenamos los grupos en la variable global
				gruposGenerados = grupos;
		  } catch (error) {
			mensajeDiv.textContent = `Error al leer el archivo: ${error.message}`;
		  }
		}

}else{
	//window.location.href ='https://techfaro-webutilidades.netlify.app';
	//window.location.href ='E:/Users/FABIOR/Desktop/pruebas/techfaro-webutilidades';
	window.location.href =darUrl('index.html');
}

```

### Archivo: `js/bolillerodatos.js`
```javascript
if(sessionStorage.getItem("verificado")){

let gruposGenerados;
let cantidadCedulasPorGrupo;
let listaFilas;




function generarGrupos(personas, cantidadPersonasPorGrupo) {
  // Crear una copia del arreglo de personas para no modificar el original
  const personasCopia = [...personas];
  const grupos = [];
  const cedulasUsadas = new Set();
  const personasNoAsignadas = [];
  

  while (personasCopia.length > 0) {
    const grupo = [];
    while (grupo.length < cantidadPersonasPorGrupo && personasCopia.length > 0) {
      // Seleccionar una persona aleatoria
      const indiceAleatorio = Math.floor(Math.random() * personasCopia.length);
      const persona = personasCopia.splice(indiceAleatorio, 1)[0];

      // Validar cédula y verificar si ya ha sido utilizada
      if (validarCedulaUruguaya(persona.cedula) && !cedulasUsadas.has(persona.cedula)) {
        cedulasUsadas.add(persona.cedula);
        grupo.push(persona);
      } else {
        personasNoAsignadas.push(persona);
      }
    }
    grupos.push(grupo);
  }

  return grupos;
}



function mostrarResultados(grupos, listaCedulasElement) {
    listaCedulasElement.innerHTML = '';

    grupos.forEach((grupo, indexGrupo) => {
        const tabla = document.createElement('table');
        const encabezado = document.createElement('thead');
        const cuerpo = document.createElement('tbody');

        // Crear encabezado de la tabla
        const filaEncabezado = document.createElement('tr');
        const columnasEncabezado = ['Grupo', 'Cédula', 'Nombre', 'Apellido', 'Móvil', 'Email', 'Dirección'];
        columnasEncabezado.forEach(columna => {
            const celdaEncabezado = document.createElement('th');
            celdaEncabezado.textContent = columna;
            filaEncabezado.appendChild(celdaEncabezado);
        });
        encabezado.appendChild(filaEncabezado);

        // Crear filas para cada persona
        grupo.forEach((persona, indexPersona) => {
            const fila = document.createElement('tr');

            // Agregar celda del grupo solo en la primera fila
            if (indexPersona === 0) {
                const celdaGrupo = document.createElement('td');
                celdaGrupo.rowSpan = grupo.length;
                celdaGrupo.textContent = `${indexGrupo + 1}`;
                fila.appendChild(celdaGrupo);
            }
			
			// Agregar la cédula en todas las filas
        const celdaCedula = document.createElement('td');
        celdaCedula.textContent = persona.cedula;
        fila.appendChild(celdaCedula);

            // Agregar las demás celdas de la persona, empezando desde la segunda columna
            const columnas = Object.values(persona);
            for (let i = 1; i < columnas.length; i++) {
                const celda = document.createElement('td');
                celda.textContent = columnas[i];
                fila.appendChild(celda);
            }
            cuerpo.appendChild(fila);
        });

        tabla.appendChild(encabezado);
        tabla.appendChild(cuerpo);
        listaCedulasElement.appendChild(tabla);
    });
}


async function generarListas() {
  const archivo = document.getElementById('archivoCedulas').files[0];
  cantidadCedulasPorGrupo = parseInt(document.getElementById('cantidadCedulasPorGrupo').value);
  const listaCedulasElement = document.getElementById('listaCedulas');
  const mensajeDiv = document.getElementById('mensaje');

  if (!archivo) {
    mensajeDiv.textContent = 'Por favor, seleccione un archivo.';
    return;
  }

  try {
    const personas = await leerArchivoYObtenerObjeto(archivo);
    const grupos = generarGrupos(personas, cantidadCedulasPorGrupo);
    mostrarResultados(grupos, listaCedulasElement);
  } catch (error) {
    mensajeDiv.textContent = `Error al leer el archivo: ${error.message}`;
  }
}

} else {
	// No hay sesión iniciada: volver al login
	window.location.href = darUrl('index.html');
}

```

### Archivo: `js/data_entry_mbti.js`
```javascript
if(sessionStorage.getItem("verificado")){
	// =================================================================
	// CONSTANTES Y FUNCIÓN AUXILIAR
	// =================================================================

	const totalPreguntasPorGrupo = 25; 
	const totalPreguntas = 100;

	// Mapeo interno: 1=-2, 2=-1, 3=0, 4=1, 5=2 (Se calcula como Raw Score - 3)

	/**
	 * Función auxiliar para obtener la característica (letra y nombre)
	 * a partir de la Puntuación Neta (Net Score).
	 */
	function getCaracteristica(grupo, netScore) {
		let letra = "";
		let nombre = "";

		switch (grupo) {
			case 1: // E/I (Positivo = E, Negativo = I)
				if (netScore > 0) { letra = 'E'; nombre = "Extroversión"; } 
				else if (netScore < 0) { letra = 'I'; nombre = "Introversión"; }
				else { letra = '-'; nombre = "Sin preferencia clara"; }
				return { letra: letra, nombre: nombre, id: "E/I" };

			case 2: // S/N (Positivo = S, Negativo = N)
				if (netScore > 0) { letra = 'S'; nombre = "Sensación"; } 
				else if (netScore < 0) { letra = 'N'; nombre = "Intuición"; }
				else { letra = '-'; nombre = "Sin preferencia clara"; }
				return { letra: letra, nombre: nombre, id: "S/N" };

			case 3: // T/F (Positivo = T, Negativo = F)
				if (netScore > 0) { letra = 'T'; nombre = "Pensamiento"; } 
				else if (netScore < 0) { letra = 'F'; nombre = "Sentimiento"; }
				else { letra = '-'; nombre = "Sin preferencia clara"; }
				return { letra: letra, nombre: nombre, id: "T/F" };

			case 4: // J/P (Positivo = J, Negativo = P)
				if (netScore > 0) { letra = 'J'; nombre = "Juicio"; } 
				else if (netScore < 0) { letra = 'P'; nombre = "Percepción"; }
				else { letra = '-'; nombre = "Sin preferencia clara"; }
				return { letra: letra, nombre: nombre, id: "J/P" };
			default:
				return { letra: 'X', nombre: 'Desconocido', id: 'X/X' };
		}
	}

	/**
	 * Genera la interpretación detallada del nivel de preferencia según la magnitud del Net Score.
	 * Rango total: ±50.
	 * Clasificación:
	 * - Net Score = 0: Sin preferencia clara
	 * - |Net Score| > 0 a 9: Ligera preferencia
	 * - |Net Score| 10 a 19: Preferencia moderada
	 * - |Net Score| >= 20: Fuerte preferencia
	 */
	function generateInterpretation(netScore, dim) {
		if (dim.letra === '-') {
			return "Sin preferencia clara";
		}

		const absScore = Math.abs(netScore);
		let intensidad = "";

		if (absScore >= 20) {
			intensidad = "Fuerte preferencia";
		} else if (absScore >= 10) {
			intensidad = "Preferencia moderada";
		} else if (absScore > 0) {
			intensidad = "Ligera preferencia";
		} else {
			return "Sin preferencia clara"; 
		}

		return `${intensidad} por **${dim.nombre}**`;
	}


	// =================================================================
	// FUNCIÓN DE RENDERIZADO DE LOS CAMPOS DE ENTRADA MANUAL (MODO 2)
	// =================================================================
	function renderDataEntryInputs() {
		const contenedor = document.getElementById('dataInputs');
		contenedor.innerHTML = '';
		const dimensionNombres = ["E/I (Extroversión/Introversión)", "S/N (Sensación/Intuición)", "T/F (Pensamiento/Sentimiento)", "J/P (Juicio/Percepción)"];

		for (let g = 1; g <= 4; g++) { // Iterar sobre los 4 grupos
			const divGroup = document.createElement('div');
			divGroup.classList.add('data-group');
			divGroup.innerHTML = `<h3>Dimensión ${g}: ${dimensionNombres[g-1]} (Preguntas ${((g - 1) * totalPreguntasPorGrupo) + 1} - ${g * totalPreguntasPorGrupo})</h3>`;
			
			const divGrid = document.createElement('div');
			divGrid.classList.add('input-grid');

			const inicio = (g - 1) * totalPreguntasPorGrupo + 1;
			const fin = g * totalPreguntasPorGrupo;

			for (let i = inicio; i <= fin; i++) {
				const input = document.createElement('input');
				input.setAttribute('type', 'number');
				input.setAttribute('id', `q${i}`);
				input.setAttribute('min', '1');
				input.setAttribute('max', '5');
				input.setAttribute('placeholder', `P${i}`);
				input.setAttribute('required', 'true');
				input.setAttribute('data-grupo', g); // Asignar el grupo para el cálculo

				divGrid.appendChild(input);
			}
			
			divGroup.appendChild(divGrid);
			contenedor.appendChild(divGroup);
		}
	}


	// =================================================================
	// FUNCIÓN DE IMPRESIÓN DE RESULTADOS
	// =================================================================
	function imprimirResultado() {
		const resultadoDiv = document.getElementById('resultado');
		const contenidoAImprimir = resultadoDiv.innerHTML;
		
		// Validar si ya hay un resultado calculado
		if (!contenidoAImprimir || !contenidoAImprimir.includes('Tipo de Personalidad Sugerido')) {
			alert("Primero, debe calcular un resultado (Modo 1 o Modo 2) para poder imprimirlo.");
			return;
		}

		// 1. Guardar el contenido original del cuerpo
		const originalBody = document.body.innerHTML;
		
		// 2. Reemplazar el cuerpo con el contenido de resultados (optimizado para impresión)
		document.body.innerHTML = `
			<div style="padding: 10px; font-family: Arial, sans-serif; max-width: 800px; margin: 0 auto; line-height: 1.4;">
				${contenidoAImprimir}
				<p style="text-align: center; margin-top: 20px; font-size: 0.7em; color: #666;"></p>
			</div>
		`;
		
		// 3. Imprimir
		window.print();
		
		// 4. Restaurar el contenido original del cuerpo
		document.body.innerHTML = originalBody;
		
		// 5. Es crucial volver a ejecutar la función de renderizado de inputs para re-vincular los eventos
		renderDataEntryInputs();
	}


	// =================================================================
	// LÓGICA DE CÁLCULO DE RESULTADOS PARA ENTRADA DIRECTA (MODO 1)
	// =================================================================
	function calcularResultadoDirecto() {
		const resultadoDiv = document.getElementById('resultado');
		
		// 0. Obtener datos de identificación de los campos GLOBALES
		const nombre = document.getElementById('nombreGlobal').value.trim();
		const cedula = document.getElementById('cedulaGlobal').value.trim();

		const dimensiones = ['netEI', 'netSN', 'netTF', 'netJP'];
		const dimensionesTexto = ["E/I (Extroversión/Introversión)", "S/N (Sensación/Intuición)", "T/F (Pensamiento/Sentimiento)", "J/P (Juicio/Percepción)"];
		let resultadosPorGrupoNet = {};
		let tipoMBTI = "";
		let tipoMBTIArray = [];
		let parejaIndex = -1;
		let tablaDimensionesHTML = '';

		// 1. Recorrer los 4 campos de entrada directa y calcular/validar
		for (let i = 0; i < dimensiones.length; i++) {
			const inputId = dimensiones[i];
			const input = document.getElementById(inputId);
			
			// Validación omitida por brevedad
			if (!input || input.value === "" || isNaN(parseInt(input.value)) || parseInt(input.value) < -50 || parseInt(input.value) > 50) {
				// Manejo de error
				resultadoDiv.innerHTML = `<p style='color: red;'>❌ Por favor, revise el valor de la dimensión ${dimensionesTexto[i]}. Debe ser un número entre -50 y +50.</p>`;
				if(input) { input.focus(); input.style.border = '2px solid red'; }
				return;
			}

			input.style.border = '1px solid #ccc'; // Limpiar error
			resultadosPorGrupoNet[i + 1] = parseInt(input.value); 
		}

		// 2. Generar la tabla de resultados y determinar el MBTI
		let grupoContador = 0;
		
		tablaDimensionesHTML += `<table style="width: 100%; border-collapse: collapse; margin-top: 5px; font-size: 0.9em;">
			<thead style="background-color: #e9e9e9;">
				<tr>
					<th style="padding: 5px; border: 1px solid #ddd;">Dimensión</th>
					<th style="padding: 5px; border: 1px solid #ddd; text-align: center;">Puntaje (±50)</th>
					<th style="padding: 5px; border: 1px solid #ddd; text-align: center;">Letra Resultante</th>
					<th style="padding: 5px; border: 1px solid #ddd;">Interpretación de Preferencia</th>
				</tr>
			</thead>
			<tbody>`;


		for (const grupo in resultadosPorGrupoNet) {
			const netScore = resultadosPorGrupoNet[grupo];
			const dim = getCaracteristica(parseInt(grupo), netScore);
			
			// *** AQUÍ USAMOS LA FUNCIÓN DE INTERPRETACIÓN DETALLADA ***
			const interpretacionCompleta = generateInterpretation(netScore, dim);
			// Si netScore es 0, usar la pareja de letras (E/I, S/N, T/F, J/P)
			let letraFinal = dim.letra;
			if (netScore === 0) {
				letraFinal = dim.id; // Ejemplo: E/I
			}
			if (netScore === 0) {
				tipoMBTIArray.push(dim.id);
				parejaIndex = grupoContador;
			} else {
				tipoMBTIArray.push(dim.letra);
			}

			tablaDimensionesHTML += `
				<tr>
					<td style="padding: 5px; border: 1px solid #ddd;">${dimensionesTexto[grupoContador]}</td>
					<td style="padding: 5px; border: 1px solid #ddd; text-align: center;">${netScore}</td>
					<td style="padding: 5px; border: 1px solid #ddd; font-weight: bold; color: #1c4e7f; text-align: center;">${letraFinal}</td>
					<td style="padding: 5px; border: 1px solid #ddd;">${interpretacionCompleta.replace('**', '<strong>').replace('**', '</strong>')}</td>
				</tr>
			`;
			grupoContador++;
		}
		
		tablaDimensionesHTML += `</tbody></table>`;
				
		// 3. Mostrar el resultado final con formato de informe (Compacto)
		// Armar el perfil resultante según el formato solicitado
		if (parejaIndex === -1) {
			tipoMBTI = tipoMBTIArray.join("");
		} else {
			const left = tipoMBTIArray.slice(0, parejaIndex).join("");
			const right = tipoMBTIArray.slice(parejaIndex + 1).join("");
			tipoMBTI = (left ? left + "-" : "") + tipoMBTIArray[parejaIndex] + (right ? "-" + right : "");
		}
		resultadoDiv.innerHTML = `
			<div class="informe-mbti-resultado" style="padding: 10px; border: 2px solid #1c4e7f; border-radius: 8px; background-color: #f0f8ff; font-family: Arial, sans-serif;">
                
				<h2 style="text-align: center; color: #1c4e7f; margin-top: 0; margin-bottom: 10px; font-size: 1.4em; border-bottom: 1px solid #1c4e7f; padding-bottom: 5px;">INFORME DE PERFIL DE PERSONALIDAD MBTI</h2>
                
				<section style="border: 1px solid #ccc; padding: 8px; margin-bottom: 10px; background-color: white; border-radius: 5px;">
					<h3 style="margin-top: 0; color: #333; border-bottom: 1px solid #eee; padding-bottom: 3px; font-size: 1em;">Datos del Candidato</h3>
					<p style="margin: 3px 0; font-size: 0.85em;"><strong>Nombre Completo:</strong> ${nombre || 'N/A'}</p>
					<p style="margin: 3px 0; font-size: 0.85em;"><strong>Cédula/DNI:</strong> ${cedula || 'N/A'}</p>
					<p style="margin: 3px 0; font-size: 0.85em;"><strong>Documento generado el:</strong> ${new Date().toLocaleDateString('es-ES')}</p>
				</section>

				<section style="margin-bottom: 15px;">
					<h3 style="color: #4CAF50; font-size: 1em;">Resultados por Dimensión (Puntuación Neta)</h3>
					${tablaDimensionesHTML}
				</section>

				<section style="text-align: center; padding: 10px; background-color: #d9edf7; border: 1px solid #bce8f1; border-radius: 5px;">
					<h3 style="color: #31708f; margin-top: 0; font-size: 1.1em;">Tipo de Personalidad Sugerido</h3>
					<p style="font-size: 0.9em; margin: 5px 0;">El perfil resultante es:</p>
					<h2><span style="color: #c0392b; font-size: 2.5em; font-weight: bold; letter-spacing: 4px;">${tipoMBTI}</span></h2>
				</section>
			</div>
		`;
				
		// Desplazar la vista al resultado
		resultadoDiv.scrollIntoView({ behavior: 'smooth', block: 'start' });
	}


	// =================================================================
	// LÓGICA DE CÁLCULO DE RESULTADOS PARA ENTRADA MANUAL (MODO 2)
	// =================================================================
	function calcularResultadoManual() {
		const resultadoDiv = document.getElementById('resultado');
		
		// 0. Obtener datos de identificación de los campos GLOBALES
		const nombre = document.getElementById('nombreGlobal').value.trim();
		const cedula = document.getElementById('cedulaGlobal').value.trim();

		let resultadosPorGrupoNet = { 1: 0, 2: 0, 3: 0, 4: 0 }; 
		let tipoMBTI = "";
		let tipoMBTIArray = [];
		let parejaIndex = -1;
		let tablaDimensionesHTML = '';
		const dimensionesTexto = ["E/I (Extroversión/Introversión)", "S/N (Sensación/Intuición)", "T/F (Pensamiento/Sentimiento)", "J/P (Juicio/Percepción)"];


		// 1. Recorrer los 100 campos de entrada y calcular/validar
		for (let i = 1; i <= totalPreguntas; i++) {
			const input = document.getElementById(`q${i}`);
			
			// Validación básica 
			if (!input.value || parseInt(input.value) < 1 || parseInt(input.value) > 5) {
				resultadoDiv.innerHTML = `<p style='color: red;'>❌ Por favor, revisa la **pregunta ${i}**. El valor debe ser entre 1 y 5.</p>`;
				input.focus();
				input.style.border = '2px solid red';
				return;
			}

			input.style.border = '1px solid #ccc'; // Limpiar error
			
			const valorRawScore = parseInt(input.value); 
			const valorPreguntaNet = valorRawScore - 3; 
			const grupoPregunta = parseInt(input.getAttribute('data-grupo'));
			
			resultadosPorGrupoNet[grupoPregunta] += valorPreguntaNet; 
		}

		// 2. Generar la tabla de resultados y determinar el MBTI
		let grupoContador = 0;
		
		tablaDimensionesHTML += `<table style="width: 100%; border-collapse: collapse; margin-top: 5px; font-size: 0.9em;">
			<thead style="background-color: #e9e9e9;">
				<tr>
					<th style="padding: 5px; border: 1px solid #ddd;">Dimensión</th>
					<th style="padding: 5px; border: 1px solid #ddd; text-align: center;">Net Score (±50)</th>
					<th style="padding: 5px; border: 1px solid #ddd; text-align: center;">Letra Resultante</th>
					<th style="padding: 5px; border: 1px solid #ddd;">Interpretación de Preferencia</th>
				</tr>
			</thead>
			<tbody>`;

		for (const grupo in resultadosPorGrupoNet) {
			const netScore = resultadosPorGrupoNet[grupo];
			const dim = getCaracteristica(parseInt(grupo), netScore);
					
			// *** AQUÍ USAMOS LA FUNCIÓN DE INTERPRETACIÓN DETALLADA ***
			const interpretacionCompleta = generateInterpretation(netScore, dim);
			// Si netScore es 0, usar la pareja de letras (E/I, S/N, T/F, J/P)
			let letraFinal = dim.letra;
			if (netScore === 0) {
				letraFinal = dim.id; // Ejemplo: E/I
				tipoMBTIArray.push(dim.id);
				parejaIndex = grupoContador;
			} else {
				tipoMBTIArray.push(dim.letra);
			}

			tablaDimensionesHTML += `
				<tr>
					<td style="padding: 5px; border: 1px solid #ddd;">${dimensionesTexto[grupoContador]}</td>
					<td style="padding: 5px; border: 1px solid #ddd; text-align: center;">${netScore}</td>
					<td style="padding: 5px; border: 1px solid #ddd; font-weight: bold; color: #1c4e7f; text-align: center;">${letraFinal}</td>
					<td style="padding: 5px; border: 1px solid #ddd;">${interpretacionCompleta.replace('**', '<strong>').replace('**', '</strong>')}</td>
				</tr>
			`;
			grupoContador++;
		}
		
		tablaDimensionesHTML += `</tbody></table>`;
			
			// 3. Mostrar el resultado final con formato de informe (Compacto)
			// Armar el perfil resultante según el formato solicitado
			if (parejaIndex === -1) {
				tipoMBTI = tipoMBTIArray.join("");
			} else {
				const left = tipoMBTIArray.slice(0, parejaIndex).join("");
				const right = tipoMBTIArray.slice(parejaIndex + 1).join("");
				tipoMBTI = (left ? left + "-" : "") + tipoMBTIArray[parejaIndex] + (right ? "-" + right : "");
			}
		resultadoDiv.innerHTML = `
			<div class="informe-mbti-resultado" style="padding: 10px; border: 2px solid #1c4e7f; border-radius: 8px; background-color: #f0f8ff; font-family: Arial, sans-serif;">
				
				<h2 style="text-align: center; color: #1c4e7f; margin-top: 0; margin-bottom: 10px; font-size: 1.4em; border-bottom: 1px solid #1c4e7f; padding-bottom: 5px;">INFORME DE PERFIL DE PERSONALIDAD MBTI</h2>
				
				<section style="border: 1px solid #ccc; padding: 8px; margin-bottom: 10px; background-color: white; border-radius: 5px;">
					<h3 style="margin-top: 0; color: #333; border-bottom: 1px solid #eee; padding-bottom: 3px; font-size: 1em;">Datos del Candidato</h3>
					<p style="margin: 3px 0; font-size: 0.85em;"><strong>Nombre Completo:</strong> ${nombre || 'N/A'}</p>
					<p style="margin: 3px 0; font-size: 0.85em;"><strong>Cédula/DNI:</strong> ${cedula || 'N/A'}</p>
					<p style="margin: 3px 0; font-size: 0.85em;"><strong>Documento generado el:</strong> ${new Date().toLocaleDateString('es-ES')}</p>
				</section>

				<section style="margin-bottom: 15px;">
					<h3 style="color: #4CAF50; font-size: 1em;">Resultados por Dimensión (Puntuación Neta)</h3>
					${tablaDimensionesHTML}
				</section>

				<section style="text-align: center; padding: 10px; background-color: #d9edf7; border: 1px solid #bce8f1; border-radius: 5px;">
					<h3 style="color: #31708f; margin-top: 0; font-size: 1.1em;">Tipo de Personalidad Sugerido</h3>
					<p style="font-size: 0.9em; margin: 5px 0;">El perfil resultante es:</p>
					<h2><span style="color: #c0392b; font-size: 2.5em; font-weight: bold; letter-spacing: 4px;">${tipoMBTI}</span></h2>
				</section>
			</div>
		`;
				
		// Desplazar la vista al resultado
		resultadoDiv.scrollIntoView({ behavior: 'smooth', block: 'start' });
	}

	// Asegura que los campos de entrada se rendericen al cargar la página
	document.addEventListener('DOMContentLoaded', renderDataEntryInputs);
}else{
		//window.location.href ='https://techfaro-webutilidades.netlify.app';
	//window.location.href ='E:/Users/FABIOR/Desktop/pruebas/techfaro-webutilidades';
	window.location.href=darUrl('index.html');
}
```

### Archivo: `js/funcgral.js`
```javascript
//sessionStorage.setItem('Produccion', false);


function darUrl(cadena) {
	const modo=true;


 // URLs base para producción y desarrollo
  const URL_PRODUCCION = 'https://techfaro-webutilidades.netlify.app/';
  //const URL_DESARROLLO = 'E:/Users/FABIOR/Desktop/pruebas/techfaro-webutilidades/';
  const URL_DESARROLLO = 'D:/Cosas/Programacion/Desarrollo/TechFaRo-webUtilidades/';
  
  
 

  try {
    
	if(cadena!='index.html'){
		cadena='html/'+cadena;
	}

    // Construir la URL según el modo	
    if (modo) {
		
      return URL_PRODUCCION + cadena;
    } else {
		
      return URL_DESARROLLO + cadena;
    }
  } catch (error) {
    console.error('Error al construir la URL:', error);
    return 'URL inválida';
  }
}


// Seleccionar todos los enlaces con el atributo data-url
const enlaces = document.querySelectorAll('a[data-url]');

function loadPageInContainer(filePath) {
  const containerDestino = document.getElementById('contDer');
  if (!containerDestino) return;

  // Cargar el HTML completo en un iframe para preservar scripts, estilos y comportamiento original
  containerDestino.innerHTML = '';
  const frame = document.createElement('iframe');
  frame.id = 'moduleFrame';
  frame.src = filePath;
  frame.style.width = '100%';
  frame.style.height = '640px'; // altura provisoria hasta que cargue y se pueda medir
  frame.style.border = 'none';
  frame.style.minHeight = '640px';
  frame.setAttribute('allow', 'clipboard-read; clipboard-write');

  // Ajusta la altura del iframe a la altura real de su contenido, para que
  // páginas cortas no dejen espacio vacío y páginas largas no queden con
  // scroll interno (en vez de eso, la página completa se desplaza).
  function ajustarAltura() {
    try {
      const doc = frame.contentWindow.document;
      const alto = Math.max(
        doc.documentElement.scrollHeight,
        doc.body ? doc.body.scrollHeight : 0
      );
      frame.style.height = Math.max(alto, 640) + 'px';
    } catch (e) {
      // Si por algún motivo no se puede medir, se mantiene la última altura
    }
  }

  frame.onload = () => {
    // Estilo y scroll independiente
    const prime = frame.contentWindow.document;
    prime.body.style.background = 'transparent';
    prime.documentElement.style.overflowX = 'auto';
    prime.body.style.overflowX = 'auto';
    prime.body.style.minWidth = '100%';

    // Arrancar la herramienta con el mismo tema (claro/oscuro) que ya está
    // elegido en el menú, para que no aparezca en blanco en medio de modo oscuro.
    if (document.documentElement.getAttribute('data-theme') === 'dark') {
      prime.documentElement.setAttribute('data-theme', 'dark');
    }

    // Forzar quiebra de contenido muy ancho en el iframe.
    // box-sizing:border-box es clave: sin esto, en páginas que no lo definen
    // por su cuenta, "max-width:100%" + padding propio hace que el elemento
    // termine siendo MÁS ancho que su contenedor (el padding se suma aparte).
    const styleTag = prime.createElement('style');
    styleTag.textContent = `
      * { box-sizing: border-box !important; max-width: 100% !important; overflow-wrap: anywhere !important; }
      input, textarea, select, table { max-width: 100% !important; }
      body { margin: 0 !important; }
    `;
    prime.head.appendChild(styleTag);

    frame.style.width = '100%';
    frame.style.transform = 'none';

    // Medir altura inicial y volver a medir cuando el contenido cambie
    // (nuevas preguntas, resultados, gráficos, etc.)
    ajustarAltura();
    if (window.ResizeObserver) {
      const ro = new ResizeObserver(ajustarAltura);
      ro.observe(prime.documentElement);
    }
    // Red de seguridad por si algo cambia el tamaño sin disparar el observer
    frame.contentWindow.addEventListener('resize', ajustarAltura);
    setTimeout(ajustarAltura, 300);
    setTimeout(ajustarAltura, 1000);
  };

  containerDestino.appendChild(frame);
}

enlaces.forEach(enlace => {
  enlace.addEventListener('click', (event) => {
    event.preventDefault();
    const urlBase = enlace.dataset.url;
    const nuevaUrl = darUrl(urlBase);
    const containerDestino = document.getElementById('contDer');

    // Marcar en el menú cuál herramienta está abierta ahora
    enlaces.forEach(e => e.classList.remove('active'));
    enlace.classList.add('active');

    if (containerDestino) {
      loadPageInContainer(urlBase);
      try {
        window.history.replaceState(null, '', nuevaUrl);
      } catch (e) {
        // nuevaUrl apunta siempre al dominio de producción; si se está probando
        // en otro origen (localhost, preview de Netlify, etc.) el navegador
        // bloquea replaceState entre orígenes distintos. No es crítico: solo
        // afecta la URL mostrada en la barra de direcciones, no la navegación.
        console.warn('No se pudo actualizar la URL visible (origen distinto al de producción):', e.message);
      }
    } else {
      window.location.href = nuevaUrl;
    }
  });
});





function validarCedulaUruguaya(cadena) {
	
	
		  // Limpiamos la cédula para asegurarnos de que solo contenga números
		  const numeroLimpio = cadena.replace(/\D/g, '');

		  // Verificamos si la cédula tiene exactamente 8 dígitos
		  if (numeroLimpio.length !== 8) {
			return false;
		  }

		  // Calculamos el dígito verificador esperado
		  const pesos = [2, 9, 8, 7, 6, 3, 4];
		  let suma = 0;
		  for (let i = 0; i < 7; i++) {
			suma += pesos[i] * parseInt(numeroLimpio[i]);
		  }
		  const digitoVerificadorCalculado = (10 - (suma % 10)) % 10;

		  // Extraemos el dígito verificador de la cédula ingresada
		  const digitoVerificadorIngresado = parseInt(numeroLimpio[7]);

		  // Comparamos ambos dígitos verificadores
		  return digitoVerificadorCalculado === digitoVerificadorIngresado;
	
}


function parsearLinea(linea,caracter) {
  const campos = linea.split(caracter);
  return {
    cedula: campos[0].trim(),
    nombre: campos[1].trim(),
    apellido: campos[2].trim(),
    movil: campos[3].trim(),
    email: campos[4].trim(),
    direccion: campos[5].trim()
  };
}

function codificar(param) {
            const textoCodificado = param;
            const texto = btoa(textoCodificado);
    return  texto;
}



function limpiarTexto(texto) {
	return texto.replace(/[.,\-_/\\!?'"()[\]{}@#$%^&*+=~]/g, '');
}

				




function leerArchivoYObtenerObjeto(archivo) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = (event) => {
      const cadena = event.target.result.split('\n').filter(cedula => cedula.trim());
      const objeto = [];
      cadena.forEach(linea => {
        objeto.push(parsearLinea(linea,';'));
      });
      resolve(objeto);
    };
    reader.onerror = error => reject(error);
    reader.readAsText(archivo);
  });
}


```

### Archivo: `js/login.js`
```javascript
document.querySelector('form').addEventListener('submit', (event) => {
    event.preventDefault();


	let usuarios = [
		{ usuario: "RkFCSU9S", contrasenia: "MzAwNDE1UkZj",Persona:"Fabio" },
		{ usuario: "UklUQU0=", contrasenia: "Q2FuZGVsYXJpYTIwMTU=",Persona:"Rita" },
		{ usuario: "Q0FOREVMQVJJQVJN", contrasenia: "MjAxNUNhbmRl",Persona:"Candelaria" },
					
		// ... más usuarios
	];



    //const username = document.querySelector('#username').value;
	const username = document.getElementById('username').value;
    //const password = document.querySelector('#password').value;
	const password = document.getElementById('password').value;


		const resultadoValidacion = validarUsuario(username, password,usuarios);

		if (resultadoValidacion === true) {
			// Si la validación es correcta, puedes enviar el formulario al servidor o realizar otra acción
			//alert('Login exitoso!');
			sessionStorage.setItem('verificado', true);
			// Guardamos el nombre de quien inició sesión para mostrarlo en el menú
			const usuarioEncontrado = usuarios.find(u => u.usuario === codificar(limpiarTexto(username).toUpperCase()));
			if (usuarioEncontrado) {
				sessionStorage.setItem('nombreUsuario', usuarioEncontrado.Persona);
			}
			 //window.location.href ='https://techfaro-webutilidades.netlify.app';
			   //window.location.href ='E:/Users/FABIOR/Desktop/pruebas/techfaro-webutilidades/html/menu.html';
			   window.location.href =darUrl('menu.html');
			 
			 
		} else {
			alert(resultadoValidacion); // Mostrar el mensaje de error específico
		}
	
});


function validarUsuario(usuario, contrasenia, listaUsuarios) {
    // Limpiar y normalizar
    let usuarioLimpio = limpiarTexto(usuario).toUpperCase();
    let contraseniaLimpia = limpiarTexto(contrasenia);

    // Validaciones básicas
    if (!usuarioLimpio || !contraseniaLimpia) {
        return "Por favor, ingresa tu usuario y contraseña.";
    }

    // Validaciones más específicas (ejemplo: longitud mínima)
    if (usuarioLimpio.length < 5) {
        return "El usuario debe tener al menos 5 caracteres.";
    }
    if (contraseniaLimpia.length < 8) {
        return "La contraseña debe tener al menos 8 caracteres.";
    }

    // Codificar para comparar
    let usuarioCodificado = codificar(usuarioLimpio);
    let contraseniaCodificada = codificar(contraseniaLimpia);

    for (const user of listaUsuarios) {
        if (usuarioCodificado === user.usuario && contraseniaCodificada === user.contrasenia) {
            return true;
        }
    }
    return "Usuario y/o contraseña incorrecto/s";
}


```

### Archivo: `js/mbti.js`
```javascript
if(sessionStorage.getItem("verificado")){

// VALORES BRUTOS: 1, 2, 3, 4, 5 (Visibles en el HTML y la interfaz)
const RAW_SCORE_VALUES = [1, 2, 3, 4, 5];
// Mapeo interno: 1=-2, 2=-1, 3=0, 4=1, 5=2 (Se calcula como Raw Score - 3)

// Función auxiliar para obtener la característica
function getCaracteristica(grupo, netScore) {
    let letra = "";
    let nombre = "";

    switch (grupo) {
        case 1: // E/I (Positivo = E, Negativo = I)
            if (netScore > 0) { letra = 'E'; nombre = "Extroversión"; } 
            else if (netScore < 0) { letra = 'I'; nombre = "Introversión"; }
            else { letra = '-'; nombre = "Sin preferencia clara"; }
            return { letra: letra, nombre: nombre, id: "E/I" };

        case 2: // S/N (Positivo = S, Negativo = N)
            if (netScore > 0) { letra = 'S'; nombre = "Sensación"; } 
            else if (netScore < 0) { letra = 'N'; nombre = "Intuición"; }
            else { letra = '-'; nombre = "Sin preferencia clara"; }
            return { letra: letra, nombre: nombre, id: "S/N" };

        case 3: // T/F (Positivo = T, Negativo = F)
            if (netScore > 0) { letra = 'T'; nombre = "Pensamiento"; } 
            else if (netScore < 0) { letra = 'F'; nombre = "Sentimiento"; }
            else { letra = '-'; nombre = "Sin preferencia clara"; }
            return { letra: letra, nombre: nombre, id: "T/F" };

        case 4: // J/P (Positivo = J, Negativo = P)
            if (netScore > 0) { letra = 'J'; nombre = "Juicio"; } 
            else if (netScore < 0) { letra = 'P'; nombre = "Percepción"; }
            else { letra = '-'; nombre = "Sin preferencia clara"; }
            return { letra: letra, nombre: nombre, id: "J/P" };
        default:
            return { letra: 'X', nombre: 'Desconocido', id: 'X/X' };
    }
}
    
// =================================================================
// FUNCIÓN DE AUTO-COMPLETAR (PARA PRUEBAS)
// =================================================================
function autoCompletar() {
    const totalPreguntas = 100;
            
    for (let i = 1; i <= totalPreguntas; i++) {
        // Generar un índice aleatorio de 0 a 4
        const indiceAleatorio = Math.floor(Math.random() * 5); 
        // Usamos el índice para obtener el valor de RAW_SCORE_VALUES (1 a 5)
        const valorAleatorioRawScore = RAW_SCORE_VALUES[indiceAleatorio]; 
                
        const radios = document.getElementsByName(`pregunta${i}`);
                
        for (let j = 0; j < radios.length; j++) {
            // Comparamos el valor del input (1 a 5)
            if (parseInt(radios[j].value) === valorAleatorioRawScore) {
                radios[j].checked = true;
                        
                // Opcionalmente, quitar el resaltado de error si existe
                const preguntaDiv = radios[j].closest('.pregunta');
                if (preguntaDiv) {
                    preguntaDiv.style.border = '1px solid #C8E6C9';
                }
                        
                break; 
            }
        }
    }
            
    const resultadoDiv = document.getElementById('resultado');
    resultadoDiv.innerHTML = "<p style='color: green;'>✅ ¡Cuestionario **auto-completado** con respuestas aleatorias! Ahora puedes presionar 'Calcular Resultado'.</p>";
    resultadoDiv.scrollIntoView({ behavior: 'smooth', block: 'start' });
}


// =================================================================
// DEFINICIÓN DEL ARRAY DE PREGUNTAS (100 PREGUNTAS: 25 por grupo)
// Los textos y grupos son idénticos a los de tu archivo original.
// =================================================================
const grupo1_EI = [
    { texto: "Me siento lleno de energía después de socializar.", grupo: 1 },
    { texto: "Prefiero participar activamente en conversaciones de grupo.", grupo: 1 },
    { texto: "Me resulta fácil iniciar una conversación con desconocidos.", grupo: 1 },
    { texto: "A menudo busco la compañía de otros.", grupo: 1 },
    { texto: "Me gustan los ambientes ruidosos y concurridos.", grupo: 1 },
    { texto: "Soy más propenso a hablar que a escuchar.", grupo: 1 },
    { texto: "Disfruto expresando mis ideas abiertamente en público.", grupo: 1 },
    { texto: "Me considero una persona sociable y animada.", grupo: 1 },
    { texto: "Necesito mucha interacción social para sentirme bien.", grupo: 1 },
    { texto: "El aislamiento social me cansa rápidamente.", grupo: 1 },
    { texto: "Me adapto fácilmente a nuevos grupos sociales.", grupo: 1 },
    { texto: "Mis amigos me consideran extrovertido.", grupo: 1 },
    { texto: "Tomo la iniciativa en actividades sociales.", grupo: 1 },
    { texto: "Prefiero hacer cosas con mucha gente.", grupo: 1 },
    { texto: "Me gusta ser el centro de atención.", grupo: 1 },
    { texto: "Soy muy accesible para la mayoría de la gente.", grupo: 1 },
    { texto: "Me gusta debatir temas complejos en voz alta.", grupo: 1 },
    { texto: "Las reuniones grandes me resultan energizantes.", grupo: 1 },
    { texto: "Me aburro si estoy solo por mucho tiempo.", grupo: 1 },
    { texto: "Me expreso con facilidad y entusiasmo.", grupo: 1 },
    { texto: "Suelo actuar antes de pensar demasiado.", grupo: 1 },
    { texto: "Soy impulsivo y me gusta la espontaneidad social.", grupo: 1 },
    { texto: "La rutina me agobia; prefiero la novedad y la acción.", grupo: 1 },
    { texto: "Tengo un círculo amplio de conocidos.", grupo: 1 },
    { texto: "Siempre tengo algo que decir.", grupo: 1 }
];

const grupo2_SN = [
    { texto: "Me concentro en los hechos y detalles tal como son.", grupo: 2 },
    { texto: "Confío en la experiencia y los datos verificables.", grupo: 2 },
    { texto: "Me preocupo más por el presente que por las posibilidades futuras.", grupo: 2 },
    { texto: "Soy práctico y me gusta lo concreto.", grupo: 2 },
    { texto: "Prefiero instrucciones claras y directas, paso a paso.", grupo: 2 },
    { texto: "Mi memoria para los detalles es excelente.", grupo: 2 },
    { texto: "Valoro el sentido común y la realidad inmediata.", grupo: 2 },
    { texto: "Busco soluciones que ya han sido probadas.", grupo: 2 },
    { texto: "Me gusta usar métodos y herramientas establecidos.", grupo: 2 },
    { texto: "Disfruto trabajando con datos tangibles.", grupo: 2 },
    { texto: "Encuentro que los temas abstractos son irrelevantes.", grupo: 2 },
    { texto: "Soy observador de lo que sucede a mi alrededor.", grupo: 2 },
    { texto: "Soy muy minucioso en mis tareas.", grupo: 2 },
    { texto: "Me baso en mis cinco sentidos para tomar decisiones.", grupo: 2 },
    { texto: "Prefiero mantener los pies en la tierra.", grupo: 2 },
    { texto: "Las metáforas y analogías me confunden.", grupo: 2 },
    { texto: "Me gusta ver el panorama completo antes de empezar.", grupo: 2 },
    { texto: "Soy bueno siguiendo un plan de trabajo detallado.", grupo: 2 },
    { texto: "Las teorías no me interesan mucho, a menos que sean aplicables.", grupo: 2 },
    { texto: "Tiendo a ser realista sobre mis expectativas.", grupo: 2 },
    { texto: "Recuerdo mejor los procedimientos que las ideas.", grupo: 2 },
    { texto: "Me molesta cuando la gente se va por las ramas.", grupo: 2 },
    { texto: "Tomo las cosas de forma literal.", grupo: 2 },
    { texto: "Busco la certeza en la información.", grupo: 2 },
    { texto: "Me enfoco en un problema a la vez, con atención.", grupo: 2 }
];

const grupo3_TF = [
    { texto: "La lógica y la razón siempre deben guiar mis decisiones.", grupo: 3 },
    { texto: "Me considero una persona objetiva y crítica.", grupo: 3 },
    { texto: "Busco la justicia y la equidad por encima de la armonía grupal.", grupo: 3 },
    { texto: "Prefiero ser honesto aunque hiera sentimientos.", grupo: 3 },
    { texto: "Mi fortaleza reside en el análisis imparcial de los problemas.", grupo: 3 },
    { texto: "Al decidir, me baso en principios y reglas.", grupo: 3 },
    { texto: "Me resulta fácil ver los errores o fallos en un argumento.", grupo: 3 },
    { texto: "Me concentro en la verdad, sin dejarme llevar por la compasión.", grupo: 3 },
    { texto: "Las críticas no suelen afectarme personalmente.", grupo: 3 },
    { texto: "Suelo ser más reservado con mis emociones.", grupo: 3 },
    { texto: "Busco soluciones que sean técnicamente sólidas y eficientes.", grupo: 3 },
    { texto: "Creo que el respeto se gana con competencia.", grupo: 3 },
    { texto: "La eficiencia es crucial en mi trabajo.", grupo: 3 },
    { texto: "Me siento más cómodo debatiendo que consolando.", grupo: 3 },
    { texto: "Pienso que los sentimientos entorpecen las decisiones.", grupo: 3 },
    { texto: "Mantengo la calma en situaciones de conflicto.", grupo: 3 },
    { texto: "Es más importante la verdad que la diplomacia.", grupo: 3 },
    { texto: "Soy capaz de ser duro si la situación lo requiere.", grupo: 3 },
    { texto: "Mis críticas son directas y al grano.", grupo: 3 },
    { texto: "Me enorgullece mi capacidad de razonamiento frío.", grupo: 3 },
    { texto: "Trato a todos de la misma manera, aplicando reglas.", grupo: 3 },
    { texto: "Me es difícil entender reacciones emocionales exageradas.", grupo: 3 },
    { texto: "Soy mejor para solucionar problemas que para mediar.", grupo: 3 },
    { texto: "Me mantengo distante de los conflictos personales.", grupo: 3 },
    { texto: "La lógica es la herramienta más valiosa.", grupo: 3 }
];

const grupo4_JP = [
    { texto: "Soy una persona organizada y planifico mi trabajo con antelación.", grupo: 4 },
    { texto: "Prefiero tener las cosas resueltas y cerradas cuanto antes.", grupo: 4 },
    { texto: "Me gusta seguir un horario y una lista de tareas.", grupo: 4 },
    { texto: "Trabajo mejor cuando tengo fechas límite claras.", grupo: 4 },
    { texto: "No me gusta dejar las decisiones para última hora.", grupo: 4 },
    { texto: "Disfruto terminando proyectos y pasando al siguiente.", grupo: 4 },
    { texto: "Me siento incómodo con los cambios de último momento.", grupo: 4 },
    { texto: "Tengo un plan para casi todo.", grupo: 4 },
    { texto: "Mis espacios de trabajo suelen estar ordenados y limpios.", grupo: 4 },
    { texto: "Busco la estructura, el orden y la previsibilidad.", grupo: 4 },
    { texto: "Creo en hacer el trabajo antes que el ocio (trabajo primero, juego después).", grupo: 4 },
    { texto: "Soy puntual y valoro la puntualidad de los demás.", grupo: 4 },
    { texto: "Cuando viajo, sigo un itinerario detallado.", grupo: 4 },
    { texto: "Me siento mejor una vez que he tomado una decisión final.", grupo: 4 },
    { texto: "Me gusta tener las cosas bajo control.", grupo: 4 },
    { texto: "La improvisación me estresa.", grupo: 4 },
    { texto: "Considero que la planificación es clave para el éxito.", grupo: 4 },
    { texto: "Soy muy responsable con mis compromisos.", grupo: 4 },
    { texto: "Me molesta la ineficiencia y la desorganización.", grupo: 4 },
    { texto: "Prefiero la anticipación a la sorpresa.", grupo: 4 },
    { texto: "Mis metas están definidas y son claras.", grupo: 4 },
    { texto: "Suelo empezar a trabajar mucho antes de la fecha límite.", grupo: 4 },
    { texto: "La ambigüedad me resulta frustrante.", grupo: 4 },
    { texto: "Me siento satisfecho cuando mi entorno está en orden.", grupo: 4 },
    { texto: "Tengo un sistema para organizar mis cosas.", grupo: 4 }
];


// Array principal combinando todos los grupos (100 preguntas)
const preguntas = [...grupo1_EI, ...grupo2_SN, ...grupo3_TF, ...grupo4_JP];


// =================================================================
// FUNCIÓN DE RENDERIZADO (MUESTRA LAS PREGUNTAS EN EL HTML)
// =================================================================

function renderizarPreguntas() {
    const contenedorPreguntas = document.getElementById('preguntas');
    contenedorPreguntas.innerHTML = ''; 

    // Etiquetas completas para las opciones de radio
    const labels = ["1", "2", "3", "4", "5"];

    preguntas.forEach((pregunta, index) => {
        const numeroPregunta = index + 1;
        const divPregunta = document.createElement('div');
        divPregunta.classList.add('pregunta');

        const titulo = document.createElement('h2');
        titulo.textContent = `${numeroPregunta}. ${pregunta.texto}`;
        divPregunta.appendChild(titulo);

        const divOpciones = document.createElement('div');
        divOpciones.classList.add('opciones-radio');

        for (let i = 0; i < 5; i++) { // Iteramos de 0 a 4
            const label = document.createElement('label');
            const valorRawScore = RAW_SCORE_VALUES[i]; // 1, 2, 3, 4, 5
            
            let textoSecundario = '';
            if (valorRawScore === 1) {
                textoSecundario = `<br><span style="font-weight: normal; font-size: 0.75em;">(Muy en desacuerdo)</span>`;
            } else if (valorRawScore === 3) {
                textoSecundario = `<br><span style="font-weight: normal; font-size: 0.75em;">(Neutro)</span>`;
            } else if (valorRawScore === 5) {
                textoSecundario = `<br><span style="font-weight: normal; font-size: 0.75em;">(Muy de acuerdo)</span>`;
            }
            
            label.innerHTML = `
                <input type="radio" name="pregunta${numeroPregunta}" value="${valorRawScore}" required>
                ${labels[i]}
                ${textoSecundario}
            `;
            divOpciones.appendChild(label);
        }
        
        divPregunta.appendChild(divOpciones);
        contenedorPreguntas.appendChild(divPregunta);
    });
}

// =================================================================
// LÓGICA DE CÁLCULO DE RESULTADOS (PUNTUACIÓN NETA: -50 a +50)
// Se aplica la traducción: Net Score = Raw Score - 3
// =================================================================

function calcularResultado() {
    const resultadoDiv = document.getElementById('resultado');
            
    // Máximo de puntuación neta: 25 preguntas * 2 puntos = 50
    let resultadosPorGrupoNet = { 1: 0, 2: 0, 3: 0, 4: 0 }; 
    let preguntaFaltante = -1;
    let tipoMBTI = "";
    const dimensionesTexto = ["Dimensión 1 (E/I)", "Dimensión 2 (S/N)", "Dimensión 3 (T/F)", "Dimensión 4 (J/P)"];
    let letrasFinales = [];


    // 1. Recorrer y validar respuestas y aplicar el Mapeo Interno
    for (let i = 1; i <= preguntas.length; i++) {
        const checkedRadio = document.querySelector(`input[name="pregunta${i}"]:checked`);
                
        // Limpiar estilo de error previo
        if (document.getElementsByName(`pregunta${i}`)[0]) {
            document.getElementsByName(`pregunta${i}`)[0].closest('.pregunta').style.border = '1px solid #C8E6C9';
        }

        if (!checkedRadio) {
            preguntaFaltante = i;
            break; 
        }

        // El valor del input es el RAW SCORE (1 a 5)
        const valorRawScore = parseInt(checkedRadio.value); 
                
        // *** APLICACIÓN DEL MAPEO INTERNO: Net Score = Raw Score - 3 ***
        const valorPreguntaNet = valorRawScore - 3; 
                
        const grupoPregunta = preguntas[i - 1].grupo; 
                
        // Sumamos la puntuación NETA
        resultadosPorGrupoNet[grupoPregunta] += valorPreguntaNet; 
    }

    // 2. Manejar error si falta alguna respuesta
    if (preguntaFaltante !== -1) {
        resultadoDiv.innerHTML = `<p style='color: red;'>❌ Por favor, responde la **pregunta ${preguntaFaltante}** antes de continuar.</p>`;
                
        // Resaltar la pregunta y hacer scroll
        const elementoFaltante = document.getElementsByName(`pregunta${preguntaFaltante}`)[0];
        if (elementoFaltante) {
            elementoFaltante.closest('.pregunta').style.border = '2px solid red';
            elementoFaltante.scrollIntoView({ behavior: 'smooth', block: 'center' });
        }
        return;
    }

    // 3. Determinar el MBTI y generar el resumen estructurado
    let resumenResultados = `<h3>Resultados por Dimensión (Puntuación Neta: -50 a +50):</h3>`;
    let grupoContador = 0;

    for (const grupo in resultadosPorGrupoNet) {
        const netScore = resultadosPorGrupoNet[grupo];
        const dim = getCaracteristica(parseInt(grupo), netScore);
                
        // La letra final es la calculada (E, I, S, N, T, F, J, P) o el primer caracter del ID si es neutro.
        const letraFinal = dim.letra === '-' ? dim.id.charAt(0) : dim.letra;
        tipoMBTI += letraFinal;
        letrasFinales.push(dim.letra === '-' ? dim.id.charAt(0) : dim.letra);


        let interpretacion;
        if (netScore > 0) {
            interpretacion = `Preferencia por: **${dim.nombre}**`;
        } else if (netScore < 0) {
            // Cuando la puntuación neta es negativa, la preferencia es por la SEGUNDA letra del par.
            // Por ejemplo, en E/I, netScore < 0 indica I (Introversión).
            // La función getCaracteristica ya maneja esto.
            interpretacion = `Preferencia por: **${dim.nombre}**`;
        } else {
            interpretacion = `Sin preferencia clara.`;
        }

        // FORMATO: **Dimensión X (A/B):** [Net Score] puntos. (Preferencia por: ...)
        resumenResultados += `**${dimensionesTexto[grupoContador]}:** ${netScore} puntos. (${interpretacion})<br>`;
                
        grupoContador++;
    }
            
    // 4. Mostrar el resultado final
    resultadoDiv.innerHTML = `
        ${resumenResultados}
        <hr>
        <h2>🎉 Su Tipo de Personalidad MBTI Sugerido es: <span style="color: #FF0000; font-size: 1.6em;">${tipoMBTI}</span></h2>
        <p>(${letrasFinales.join(' - ')})</p>
    `;
            
    // Desplazar la vista al resultado
    resultadoDiv.scrollIntoView({ behavior: 'smooth', block: 'start' });
}

// Se asegura que las preguntas se rendericen al cargar la página
document.addEventListener('DOMContentLoaded', renderizarPreguntas);

} else {
	// No hay sesión iniciada: volver al login
	window.location.href = darUrl('index.html');
}

```

### Archivo: `js/mbti_nuevo.js`
```javascript
// Grupos de preguntas (16 por dimensión)
const grupo1 = [
    { texto: "En reuniones sociales, me siento con energía.", grupo: 1 }, // E/I
    { texto: "Prefiero interactuar con un grupo grande de personas.", grupo: 1 }, // E/I
    { texto: "Después de socializar, necesito tiempo a solas para recargarme.", grupo: 1 }, // E/I
    { texto: "Me resulta fácil iniciar conversaciones con desconocidos.", grupo: 1 }, // E/I
    { texto: "Disfruto expresando mis ideas abiertamente en grupos.", grupo: 1 }, // E/I
    { texto: "Me considero una persona sociable y extrovertida.", grupo: 1 }, // E/I
    { texto: "En un grupo, tiendo a hablar más que a escuchar.", grupo: 1 }, // E/I
    { texto: "Los ambientes ruidosos y concurridos me energizan.", grupo: 1 }, // E/I
    { texto: "Prefiero actividades que involucren mucha interacción social.", grupo: 1 }, // E/I
    { texto: "Me siento más cómodo en grupos grandes.", grupo: 1 }, // E/I
    { texto: "En un equipo, prefiero asumir un rol activo y participativo.", grupo: 1 }, // E/I
    { texto: "Me comunico mejor a través del diálogo verbal.", grupo: 1 }, // E/I
    { texto: "Me resulta fácil hacer nuevas amistades.", grupo: 1 }, // E/I
    { texto: "Disfruto siendo el centro de atención.", grupo: 1 }, // E/I
    { texto: "Tiendo a pensar en voz alta para organizar mis ideas.", grupo: 1 }, // E/I
    { texto: "Me siento revitalizado al estar rodeado de gente.", grupo: 1 }, // E/I
];

const grupo2 = [
    { texto: "Presto más atención a los detalles concretos.", grupo: 2 }, // S/N
    { texto: "Prefiero información clara y tangible.", grupo: 2 }, // S/N
    { texto: "Me gusta trabajar con hechos y datos verificables.", grupo: 2 }, // S/N
    { texto: "Me considero una persona práctica y realista.", grupo: 2 }, // S/N
    { texto: "Me atraen más las cosas que puedo experimentar con mis sentidos.", grupo: 2 }, // S/N
    { texto: "Prefiero aprender a través de la experiencia práctica.", grupo: 2 }, // S/N
    { texto: "Me gusta seguir instrucciones paso a paso.", grupo: 2 }, // S/N
    { texto: "Confío más en la información que proviene de la experiencia directa.", grupo: 2 }, // S/N
    { texto: "Abordo los problemas de manera metódica y práctica.", grupo: 2 }, // S/N
    { texto: "Me enfoco principalmente en el presente.", grupo: 2 }, // S/N
    { texto: "Al percibir el mundo, confío más en mis sentidos que en la intuición.", grupo: 2 }, // S/N
    { texto: "Recuerdo mejor los detalles específicos de los eventos.", grupo: 2 }, // S/N
    { texto: "Prefiero explicaciones claras y concisas con ejemplos concretos.", grupo: 2 }, // S/N
    { texto: "Me aburren las teorías abstractas sin aplicación práctica.", grupo: 2 }, // S/N
    { texto: "Me siento más cómodo con lo que es real y tangible.", grupo: 2 }, // S/N
    { texto: "Al planificar, me enfoco en los detalles prácticos y los pasos a seguir.", grupo: 2 }, // S/N
];

const grupo3 = [
    { texto: "Al tomar decisiones, priorizo la lógica y el análisis objetivo.", grupo: 3 }, // T/F
    { texto: "Valoro más la objetividad y la imparcialidad.", grupo: 3 }, // T/F
    { texto: "Me considero una persona analítica y racional.", grupo: 3 }, // T/F
    { texto: "En situaciones de conflicto, busco soluciones lógicas y justas.", grupo: 3 }, // T/F
    { texto: "Considero el impacto emocional de mis decisiones en los demás.", grupo: 3 }, // T/F
    { texto: "Prefiero la comunicación directa y honesta, incluso si es incómoda.", grupo: 3 }, // T/F
    { texto: "Me resulta fácil separar mis emociones al tomar decisiones importantes.", grupo: 3 }, // T/F
    { texto: "En las relaciones, valoro la honestidad y la comunicación clara.", grupo: 3 }, // T/F
    { texto: "Me motiva lograr resultados concretos y objetivos.", grupo: 3 }, // T/F
    { texto: "Al dar retroalimentación, me enfoco en los hechos y el rendimiento.", grupo: 3 }, // T/F
    { texto: "Prefiero recibir críticas directas y constructivas.", grupo: 3 }, // T/F
    { texto: "Me cuesta expresar mis sentimientos y emociones.", grupo: 3 }, // T/F
    { texto: "Me influyen más los argumentos lógicos que las consideraciones emocionales.", grupo: 3 }, // T/F
    { texto: "Valoro la cooperación y la armonía en un equipo.", grupo: 3 }, // T/F
    { texto: "Me preocupo por el bienestar de los demás.", grupo: 3 }, // T/F
    { texto: "Me guio por mis valores y convicciones personales.", grupo: 3 }, // T/F
];

const grupo4 = [
    { texto: "Prefiero tener un plan detallado antes de comenzar un proyecto.", grupo: 4 }, // J/P
    { texto: "Me siento más cómodo con la rutina y la estructura.", grupo: 4 }, // J/P
    { texto: "Me gusta terminar las tareas antes de comenzar otras nuevas.", grupo: 4 }, // J/P
    { texto: "Al planificar, me enfoco en cumplir los plazos establecidos.", grupo: 4 }, // J/P
    { texto: "Me resulta fácil organizar mi tiempo y mis tareas.", grupo: 4 }, // J/P
    { texto: "Me gusta tener control sobre mi entorno y mis actividades.", grupo: 4 }, // J/P
    { texto: "Prefiero los ambientes organizados y ordenados.", grupo: 4 }, // J/P
    { texto: "Me siento más productivo cuando tengo un horario fijo.", grupo: 4 }, // J/P
    { texto: "Prefiero abordar las tareas con anticipación.", grupo: 4 }, // J/P
    { texto: "Me siento más cómodo cuando las decisiones ya están tomadas.", grupo: 4 }, // J/P
    { texto: "Me gusta que las cosas estén resueltas y cerradas.", grupo: 4 }, // J/P
    { texto: "Me adapto fácilmente a los cambios inesperados.", grupo: 4 }, // J/P
    { texto: "Me gusta trabajar con listas de tareas y planes escritos.", grupo: 4 }, // J/P
    { texto: "Prefiero la información clara, concisa y sin ambigüedades.", grupo: 4 }, // J/P
    { texto: "En mi espacio de trabajo, valoro el orden y la organización.", grupo: 4 }, // J/P
    { texto: "Tiendo a dejar las cosas para último momento.", grupo: 4}, // J/P
];

// ----------------------------------------------------------------------------------
// FUNCIÓN PARA INTERCALAR PREGUNTAS SEGÚN UN PATRÓN DEFINIDO POR EL USUARIO
// ----------------------------------------------------------------------------------

function intercalarPreguntasPorPatron(grupos) {
    // ----------------------------------------------------
    // CONSTANTE DE PATRÓN: Define la secuencia de los grupos (1 a 4).
    // Si PATRON_BASE es [0, 0, 0, 0], se devuelve el orden no intercalado (Grupo 1 -> Grupo 2 -> Grupo 3 -> Grupo 4).
    const PATRON_BASE = [0, 0, 0, 0]; // *** MODIFICA ESTE PATRÓN PARA CAMBIAR EL ORDEN ***
    const REPETICIONES = 16; 
    // ----------------------------------------------------
    
    // Verifica si el patrón es el patrón "no intercalado"
    const esPatronNoIntercalado = PATRON_BASE.every(val => val === 0);

    if (esPatronNoIntercalado) {
        // Devuelve los grupos concatenados en orden (Grupo 1, 2, 3, 4)
        return [...grupos[0], ...grupos[1], ...grupos[2], ...grupos[3]];
    }

    // Lógica para intercalar si el patrón no es [0, 0, 0, 0]
    let PATRON_INTERCALADO = [];
    for (let i = 0; i < REPETICIONES; i++) {
        PATRON_INTERCALADO = PATRON_INTERCALADO.concat(PATRON_BASE);
    }

    let preguntasIntercaladas = [];
    let indices = { 1: 0, 2: 0, 3: 0, 4: 0 }; 
    const mapaGrupos = { 1: grupo1, 2: grupo2, 3: grupo3, 4: grupo4 };

    for (const grupoId of PATRON_INTERCALADO) {
        if (mapaGrupos[grupoId] && indices[grupoId] < mapaGrupos[grupoId].length) {
            preguntasIntercaladas.push(mapaGrupos[grupoId][indices[grupoId]]);
            indices[grupoId]++;
        }
    }
    return preguntasIntercaladas;
}

// ----------------------------------------------------------------------------------
// APLICACIÓN DEL PATRÓN
// ----------------------------------------------------------------------------------

const gruposPreguntas = [grupo1, grupo2, grupo3, grupo4];
const preguntas = intercalarPreguntasPorPatron(gruposPreguntas); 
  
const etiquetas = ["NO", "no", "algo", "si", "SI"];
const valores = [-2, -1, 0, 1, 2];

const formulario = document.getElementById('miFormulario');
const preguntasDiv = document.getElementById('preguntas');
const resultadoDiv = document.getElementById('resultado');

preguntas.forEach((pregunta, index) => { // Ahora pregunta es un objeto
    const preguntaDiv = document.createElement('div');
    preguntaDiv.className = 'pregunta';

    const preguntaTitulo = document.createElement('h2');
    preguntaTitulo.textContent = ` ${index + 1}: ${pregunta.texto}`; // Accedemos a pregunta.texto
    preguntaDiv.appendChild(preguntaTitulo);

  for (let j = 0; j < 5; j++) {
        const label = document.createElement('label');
        const radio = document.createElement('input');
        radio.type = 'radio';
        radio.name = `pregunta${index + 1}`;
        radio.value = valores[j];
        label.appendChild(radio);
        label.appendChild(document.createTextNode(etiquetas[j]));
        preguntaDiv.appendChild(label);
    }
	
	 preguntaDiv.style.background = generarGradienteAleatorio(); // Aplica el gradiente

    preguntasDiv.appendChild(preguntaDiv);
});


function generarGradienteAleatorioFuertes() {
    // Genera colores hexadecimales aleatorios
    const color1 = '#' + Math.floor(Math.random()*16777215).toString(16);
    const color2 = '#' + Math.floor(Math.random()*16777215).toString(16);
    return `linear-gradient(to bottom, ${color1}, ${color2})`;
}

function generarGradienteAleatorioPastel() {
    // Genera valores RGB en un rango que produce colores pastel (150-255)
    const r1 = Math.floor(Math.random() * 106) + 150; // Rango: 150-255
    const g1 = Math.floor(Math.random() * 106) + 150; // Rango: 150-255
    const b1 = Math.floor(Math.random() * 106) + 150; // Rango: 150-255

    const r2 = Math.floor(Math.random() * 106) + 150; // Rango: 150-255
    const g2 = Math.floor(Math.random() * 106) + 150; // Rango: 150-255
    const b2 = Math.floor(Math.random() * 106) + 150; // Rango: 150-255

    // Convierte a hexadecimal y crea el gradiente
    const color1 = `#${r1.toString(16)}${g1.toString(16)}${b1.toString(16)}`;
    const color2 = `#${r2.toString(16)}${g2.toString(16)}${b2.toString(16)}`;
    return `linear-gradient(to bottom, ${color1}, ${color2})`;
}

function generarGradienteAleatorio() {
       let r, g, b;
    let baseColor;
    let intentos = 0; // Para evitar bucles infinitos en casos extremos

    do {
        r = Math.floor(Math.random() * 256);
        g = Math.floor(Math.random() * 256);
        b = Math.floor(Math.random() * 256);

        // Evitar grises (R, G y B similares)
        const diferenciaMaxima = 30; // Ajusta este valor para controlar cuan "gris" se considera un color
        if (Math.abs(r - g) < diferenciaMaxima && Math.abs(r - b) < diferenciaMaxima && Math.abs(g - b) < diferenciaMaxima) {
            continue; // Reintenta
        }

        // Evitar colores oscuros (ajustar umbrales según sea necesario)
        const luminosidadMinima = 100; // Ajusta este valor para controlar la luminosidad mínima
        if (r < luminosidadMinima || g < luminosidadMinima || b < luminosidadMinima) {
            continue; // Reintenta
        }

        // Evitar marrones (alto rojo, bajo verde y azul)
        if (r > 150 && g < 100 && b < 100) {
            continue; // Reintenta
        }

        // Evitar violetas oscuros y azules marinos
        if ((r < 100 && g < 100 && b > 50) || (r < 100 && g > 50 && b > 100) ) {
            continue;
        }

        baseColor = `#${r.toString(16).padStart(2, '0')}${g.toString(16).padStart(2, '0')}${b.toString(16).padStart(2, '0')}`;
        intentos++;
    } while (!baseColor && intentos < 100); // Reintenta hasta encontrar un color válido o hasta 100 intentos

    if (!baseColor) {
        // Si no se encuentra un color válido después de varios intentos, se usa un color por defecto
        baseColor = "#a0e8af"; // Un verde claro como valor por defecto
    }

    // Calcula una versión MÁS clara del color base (mismo código que antes)
    const factorClaridad = 0.5;

    const rClaro = Math.min(255, Math.floor(r + (255 - r) * factorClaridad));
    const gClaro = Math.min(255, Math.floor(g + (255 - g) * factorClaridad));
    const bClaro = Math.min(255, Math.floor(b + (255 - b) * factorClaridad));

    const colorClaro = `#${rClaro.toString(16).padStart(2, '0')}${gClaro.toString(16).padStart(2, '0')}${bClaro.toString(16).padStart(2, '0')}`;

    return `linear-gradient(to bottom, ${baseColor}, ${colorClaro})`;
}

// ----------------------------------------------------------------------------------
// FUNCIÓN ACTUALIZADA PARA CALCULAR Y MOSTRAR SOLO LA PUNTUACIÓN NETA (-32 a +32)
// ----------------------------------------------------------------------------------
function calcularResultado() {
    const resultadosPorGrupo = { 1: 0, 2: 0, 3: 0, 4: 0 }; // Objeto para almacenar la Puntuación Neta

    for (let i = 1; i <= preguntas.length; i++) {
        const radios = document.getElementsByName(`pregunta${i}`);
        let preguntaRespondida = false;
        let valorPregunta = 0
        for (let j = 0; j < radios.length; j++) {
            if (radios[j].checked) {
                valorPregunta = parseInt(radios[j].value);
                preguntaRespondida = true;
                break;
            }
        }
        if (!preguntaRespondida) {
            alert("Por favor, responde todas las preguntas.");
            return;
        }

        const grupoPregunta = preguntas[i - 1].grupo;
        resultadosPorGrupo[grupoPregunta] += valorPregunta; // Sumamos la Puntuación Neta
    }

    // Definición de las dimensiones para determinar la letra final
    let tipoMBTI = "";
    const dimensiones = {
        1: { id: "E/I", positivo: "E (Extraversión)", negativo: "I (Introversión)", opuesta: "I" },
        2: { id: "S/N", positivo: "S (Sensación)", negativo: "N (Intuición)", opuesta: "N" },
        3: { id: "T/F", positivo: "T (Pensamiento)", negativo: "F (Sentimiento)", opuesta: "F" },
        4: { id: "J/P", positivo: "J (Juicio)", negativo: "P (Percepción)", opuesta: "P" }
    };

    // Mostrar resultados por grupo
    resultadoDiv.innerHTML = "<h2>Resultados por Dimensión (Puntuación Neta: -32 a +32)</h2>";
    
    for (const grupo in resultadosPorGrupo) {
        const score = resultadosPorGrupo[grupo];
        const dim = dimensiones[grupo];
        let letra = "";
        let interpretacion = "";

        if (score > 0) {
            letra = dim.positivo.charAt(0); // E, S, T, J
            interpretacion = `Preferencia por: ${dim.positivo}`;
        } else if (score < 0) {
            letra = dim.opuesta; // I, N, F, P
            interpretacion = `Preferencia por: ${dim.negativo}`;
        } else {
            letra = "X";
            interpretacion = `Sin preferencia clara.`;
        }

        tipoMBTI += letra;
        
        // Muestra la puntuación neta y la interpretación
        resultadoDiv.innerHTML += `<div style="margin-bottom: 10px;">
            <strong>${dim.id}: ${score} puntos.</strong> (${interpretacion})
        </div>`;
    }

    resultadoDiv.innerHTML += `<hr>
        <h3>Su Tipo de Personalidad MBTI Neto Sugerido es: 
            <span style="color:red; font-size: 1.5em;">${tipoMBTI}</span>
        </h3>`;
}

function imprimirLo(divId){
	const divAImprimir = document.getElementById(divId);
	divAImprimir.focus();
	window.print();
}
```

### Archivo: `js/menu-manifest.js`
```javascript
/*
 * LISTA CENTRAL DE HERRAMIENTAS DEL MENÚ
 * ---------------------------------------
 * Este es el ÚNICO lugar que hay que tocar para que una herramienta nueva
 * aparezca en el menú. No hace falta editar menu.html.
 *
 * Cada renglón es uno de estos dos tipos:
 *
 *   1) Una herramienta (hoja del menú), que carga un archivo de html/:
 *        { nombre: 'Nombre visible', archivo: 'archivo.html', icono: 'nombreIcono' }
 *
 *   2) Una categoría (agrupa varias herramientas o sub-categorías):
 *        { categoria: 'Nombre visible', items: [ ...más renglones... ], icono: 'nombreIcono' }
 *
 * El campo "icono" es opcional: si no se pone, el renglón se muestra sin
 * ícono. Los íconos disponibles están definidos en ICONOS, un poco más
 * abajo — para sumar uno nuevo alcanza con agregar su SVG ahí.
 *
 * Para agregar una herramienta nueva:
 *   1) Copiá html/_plantilla.html y js/_plantilla.js, renombralos.
 *   2) Programá la herramienta.
 *   3) Agregá un renglón acá abajo, dentro de la categoría que corresponda
 *      (o creá una categoría nueva si hace falta).
 * Con esos 3 pasos, la herramienta queda protegida por el login (paso 1,
 * ya viene armado en la plantilla) y visible en el menú (paso 3).
 */
const MENU_MANIFEST = [
  {
    categoria: 'Cuestionarios',
    icono: 'clipboard',
    items: [
     
      {
        categoria: 'Test Vocacional',
        icono: 'compass',
        items: [
          { nombre: 'OVO', archivo: 'ovo.html', icono: 'aptitudes' },
         
        ],
      },
      {
        categoria: 'MBTI',
        icono: 'cabeza',
        items: [
          { nombre: 'MBTI Formulario', archivo: 'mbti.html', icono: 'archivoEditar' },
          { nombre: 'MBTI Corrector', archivo: 'data_entry_mbti.html', icono: 'chequeo' },
        ],
      },
    ],
  },
  {
    categoria: 'Bolilleros',
    icono: 'dado',
    items: [
      { nombre: 'Por CI', archivo: 'bolillero.html', icono: 'credencial' },
      { nombre: 'Datos Completo', archivo: 'bolillerocompleto.html', icono: 'personas' },
    ],
  },
  {
    categoria: 'Utilidades',
    icono: 'controles',
    items: [
      { nombre: 'Hash SHA-256', archivo: 'hash_sha256.html', icono: 'numeral' },
    ],
  },
];

// Íconos en SVG, minimalistas y livianos (sin depender de ninguna librería
// externa). Usan stroke="currentColor" para heredar automáticamente el color
// del texto del link (incluido el estado activo, en blanco).
const ICONOS = {
  clipboard: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="6" y="4" width="12" height="17" rx="2"/><path d="M9 4V3a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v1"/><line x1="9" y1="10" x2="15" y2="10"/><line x1="9" y1="14" x2="15" y2="14"/><line x1="9" y1="18" x2="13" y2="18"/></svg>',
  cabeza: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 21c0-4 3.6-7 8-7s8 3 8 7"/></svg>',
  dado: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="3"/><circle cx="8" cy="8" r="1.1" fill="currentColor" stroke="none"/><circle cx="16" cy="8" r="1.1" fill="currentColor" stroke="none"/><circle cx="8" cy="16" r="1.1" fill="currentColor" stroke="none"/><circle cx="16" cy="16" r="1.1" fill="currentColor" stroke="none"/><circle cx="12" cy="12" r="1.1" fill="currentColor" stroke="none"/></svg>',
  controles: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="4" y1="6" x2="20" y2="6"/><circle cx="9" cy="6" r="2" fill="currentColor" stroke="none"/><line x1="4" y1="12" x2="20" y2="12"/><circle cx="15" cy="12" r="2" fill="currentColor" stroke="none"/><line x1="4" y1="18" x2="20" y2="18"/><circle cx="11" cy="18" r="2" fill="currentColor" stroke="none"/></svg>',
  compass: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="9"/><path d="M15 9l-2 5-5 2 2-5z" fill="currentColor" stroke="none"/></svg>',
  archivoEditar: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14 3H7a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V9z"/><path d="M14 3v6h6"/><path d="M9.5 15.5 13 14l1 1-3.5 1.5H9.5z"/></svg>',
  chequeo: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2"/><path d="m8 12 3 3 5-6"/></svg>',
  credencial: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="5" width="20" height="14" rx="2"/><circle cx="8" cy="12" r="2"/><line x1="14" y1="10" x2="19" y2="10"/><line x1="14" y1="14" x2="19" y2="14"/></svg>',
  personas: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="9" cy="8" r="3"/><path d="M3 20c0-3.3 2.7-6 6-6s6 2.7 6 6"/><circle cx="17" cy="9" r="2.5"/><path d="M15.7 14.3c2.5.5 4.3 2.5 4.3 5.7"/></svg>',
  numeral: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="9" y1="3" x2="7" y2="21"/><line x1="17" y1="3" x2="15" y2="21"/><line x1="4" y1="9" x2="21" y2="9"/><line x1="3" y1="15" x2="20" y2="15"/></svg>',
// --- Nuevos iconos agregados ---
  buscar: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>',
  guardar: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></svg>',
  eliminar: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/><line x1="10" y1="11" x2="10" y2="17"/><line x1="14" y1="11" x2="14" y2="17"/></svg>',
  configuracion: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>',
  home: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>',
  candado: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>',
  ojo: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg>',
  corazon: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l8.72-8.72 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></svg>',
  estrella: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></svg>',
  mail: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>',
  recargar: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="23 4 23 10 17 10"/><path d="M20.49 15a9 9 0 1 1-2.12-9.36L23 10"/></svg>',
  campana: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg>',
  // Opción A: Birrete / Graduación (Enfoque académico)
testVocacional: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 10v6M2 10l10-5 10 5-10 5z"/><path d="M6 12v5c3 3 9 3 12 0v-5"/></svg>',

// Opción B: Encrucijada / Caminos (Enfoque de elección de carrera)
vocacion: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="19" r="2"/><path d="M12 17V7"/><path d="M12 7 7 2"/><path d="M12 7l5-5"/></svg>',

// Opción C: Foco / Descubrimiento (Enfoque de aptitudes y talentos)
aptitudes: '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 18h6"/><path d="M10 22h4"/><path d="M15 14c.8-.7 2-2 2-4a5 5 0 0 0-10 0c0 2 1.2 3.3 2 4l1 1h4z"/></svg>'
};

// Arma el <ul class="menu"> a partir de MENU_MANIFEST y lo inserta en el <nav>.
// Se ejecuta de inmediato (no espera a DOMContentLoaded) porque funcgral.js y
// menu.js, que se cargan justo después en menu.html, necesitan encontrar estos
// elementos ya armados en el DOM apenas se ejecutan.
function renderizarMenu(manifest, nav) {
  if (!nav) return;

  function crearContenidoLink(nombre, icono) {
    const wrapper = document.createElement('span');
    wrapper.className = 'menu-link-content';
    if (icono && ICONOS[icono]) {
      const spanIcono = document.createElement('span');
      spanIcono.className = 'menu-icon';
      spanIcono.innerHTML = ICONOS[icono];
      wrapper.appendChild(spanIcono);
    }
    const spanLabel = document.createElement('span');
    spanLabel.className = 'menu-label';
    spanLabel.textContent = nombre;
    wrapper.appendChild(spanLabel);
    return wrapper;
  }

  function construirLista(items) {
    const ul = document.createElement('ul');
    ul.className = 'submenu';
    items.forEach((item) => {
      const li = document.createElement('li');
      if (item.items) {
        // Es una categoría/sub-categoría: link "vacío" + submenú anidado
        li.className = 'menu-item';
        const a = document.createElement('a');
        a.href = '#';
        a.appendChild(crearContenidoLink(item.categoria, item.icono));
        li.appendChild(a);
        li.appendChild(construirLista(item.items));
      } else {
        // Es una herramienta: link que carga el archivo correspondiente
        const a = document.createElement('a');
        a.href = '#';
        a.dataset.url = item.archivo;
        a.appendChild(crearContenidoLink(item.nombre, item.icono));
        li.appendChild(a);
      }
      ul.appendChild(li);
    });
    return ul;
  }

  const menuRaiz = document.createElement('ul');
  menuRaiz.className = 'menu';
  manifest.forEach((categoria) => {
    const li = document.createElement('li');
    li.className = 'menu-item';
    const a = document.createElement('a');
    a.href = '#';
    a.appendChild(crearContenidoLink(categoria.categoria, categoria.icono));
    li.appendChild(a);
    li.appendChild(construirLista(categoria.items));
    menuRaiz.appendChild(li);
  });

  nav.appendChild(menuRaiz);
}

renderizarMenu(MENU_MANIFEST, document.querySelector('nav'));

```

### Archivo: `js/menu.js`
```javascript
const menuItems = document.querySelectorAll('.menu-item');

const themeToggle = document.getElementById('themeToggle');
const themeThumb = document.getElementById('themeThumb');

function aplicarTemaAlFrame(theme) {
    const frame = document.getElementById('moduleFrame');
    if (!frame) return;
    try {
        const doc = frame.contentDocument;
        if (!doc) return;
        if (theme === 'dark') {
            doc.documentElement.setAttribute('data-theme', 'dark');
        } else {
            doc.documentElement.removeAttribute('data-theme');
        }
    } catch (e) {
        // Si por algún motivo no se puede acceder al documento del iframe, no pasa nada grave
    }
}

function applyTheme(theme) {
    if (theme === 'dark') {
        document.documentElement.setAttribute('data-theme', 'dark');
        if (themeToggle) themeToggle.checked = true;
        if (themeThumb) themeThumb.style.transform = 'translateX(20px)';
        document.body.style.background = '#030b1a';
    } else {
        document.documentElement.removeAttribute('data-theme');
        if (themeToggle) themeToggle.checked = false;
        if (themeThumb) themeThumb.style.transform = 'translateX(0)';
        document.body.style.background = '';
    }
    localStorage.setItem('theme', theme);
    aplicarTemaAlFrame(theme);
}

const savedTheme = localStorage.getItem('theme') || 'light';
applyTheme(savedTheme);

if (themeToggle) {
    themeToggle.addEventListener('change', () => {
        applyTheme(themeToggle.checked ? 'dark' : 'light');
    });
}

if (sessionStorage.getItem('verificado')) {
    menuItems.forEach((item) => {
        const submenu = item.querySelector('.submenu');
        const anchor = item.querySelector(':scope > a');

        if (submenu && anchor) {
            // Permite expandir/contraer submenú en móvil
            anchor.addEventListener('click', (event) => {
                if (window.matchMedia('(max-width: 900px)').matches) {
                    event.preventDefault();
                    item.classList.toggle('open');
                }
            });

            item.addEventListener('blur', () => {
                item.classList.remove('open');
            });
        }
    });
} else {
    window.location.href = darUrl('index.html');
}

```

### Archivo: `js/ovo.js`
```javascript
if(sessionStorage.getItem("verificado")){

// O.V.O. Mejorado - TechFaRo

const preguntas = [
    {n:1, t:"Diseñar programas de computación y explorar nuevas aplicaciones tecnológicas para uso del internet.", a:4},
    {n:2, t:"Criar, cuidar y tratar animales domésticos y de campo.", a:5},
    {n:3, t:"Investigar sobre áreas verdes, medio ambiente y cambios climáticos.", a:5},
    {n:4, t:"Ilustrar, dibujar y animar digitalmente.", a:1},
    {n:5, t:"Seleccionar, capacitar y motivar al personal de una organización/empresa.", a:3},
    {n:6, t:"Realizar excavaciones para descubrir restos del pasado.", a:2},
    {n:7, t:"Resolver problemas de cálculo para construir un puente.", a:4},
    {n:8, t:"Diseñar cursos para enseñar a la gente sobre temas de salud e higiene.", a:5},
    {n:9, t:"Tocar un instrumento y componer música.", a:1},
    {n:10, t:"Planificar cuáles son las metas de una organización pública o privada a mediano y largo plazo.", a:3},
    {n:11, t:"Diseñar y planificar la producción masiva de artículos como muebles, autos, equipos de oficina, empaques y envases para alimentos y otros.", a:4},
    {n:12, t:"Diseñar logotipos y portadas de una revista.", a:1},
    {n:13, t:"Organizar eventos y atender a sus asistentes.", a:2},
    {n:14, t:"Atender la salud de personas enfermas.", a:5},
    {n:15, t:"Controlar ingresos y egresos de fondos y presentar el balance final de una institución.", a:3},
    {n:16, t:"Hacer experimentos con plantas (frutas, árboles, flores).", a:5},
    {n:17, t:"Concebir planos para viviendas, edificios y ciudades.", a:4},
    {n:18, t:"Investigar y probar nuevos productos farmacéuticos.", a:4},
    {n:19, t:"Hacer propuestas y formular estrategias para aprovechar las relaciones económicas entre dos países.", a:3},
    {n:20, t:"Pintar, hacer esculturas, ilustrar libros de arte, etcétera.", a:1},
    {n:21, t:"Elaborar campañas para introducir un nuevo producto al mercado.", a:3},
    {n:22, t:"Examinar y tratar los problemas visuales.", a:5},
    {n:23, t:"Defender a clientes individuales o empresas en juicios de diferente naturaleza.", a:2},
    {n:24, t:"Diseñar máquinas que puedan simular actividades humanas.", a:4},
    {n:25, t:"Investigar las causas y efectos de los trastornos emocionales.", a:2},
    {n:26, t:"Supervisar las ventas de un centro comercial.", a:3},
    {n:27, t:"Atender y realizar ejercicios a personas que tienen limitaciones físicas, problemas de lenguaje, etcétera.", a:5},
    {n:28, t:"Prepararse para ser modelo profesional.", a:1},
    {n:29, t:"Aconsejar a las personas sobre planes de ahorro e inversiones.", a:3},
    {n:30, t:"Elaborar mapas, planos e imágenes para el estudio y análisis de datos geográficos.", a:4},
    {n:31, t:"Diseñar juegos interactivos electrónicos para computadora.", a:1},
    {n:32, t:"Realizar el control de calidad de los alimentos.", a:5},
    {n:33, t:"Tener un negocio propio de tipo comercial.", a:3},
    {n:34, t:"Escribir artículos periodísticos, cuentos, novelas y otros.", a:2},
    {n:35, t:"Redactar guiones y libretos para un programa de televisión.", a:1},
    {n:36, t:"Organizar un plan de distribución y venta de un gran almacén.", a:3},
    {n:37, t:"Estudiar la diversidad cultural en el ámbito rural y urbano.", a:2},
    {n:38, t:"Gestionar y evaluar convenios internacionales de cooperación para el desarrollo social.", a:2},
    {n:39, t:"Crear campañas publicitarias.", a:1},
    {n:40, t:"Trabajar investigando la reproducción de peces, camarones y otros animales marinos.", a:5},
    {n:41, t:"Dedicarse a fabricar productos alimenticios de consumo masivo.", a:4},
    {n:42, t:"Gestionar y evaluar proyectos de desarrollo en una institución educativa y/o fundación.", a:2},
    {n:43, t:"Rediseñar y decorar espacios físicos en viviendas, oficinas y locales comerciales.", a:1},
    {n:44, t:"Administrar una empresa de turismo y/o agencias de viaje.", a:3},
    {n:45, t:"Aplicar métodos alternativos a la medicina tradicional para atender personas con dolencias de diverso índole.", a:5},
    {n:46, t:"Diseñar ropa para niños, jóvenes y adultos.", a:1},
    {n:47, t:"Investigar organismos vivos para elaborar vacunas.", a:5},
    {n:48, t:"Manejar y/o dar mantenimiento a dispositivos/aparatos tecnológicos en aviones, barcos, radares, etcétera.", a:4},
    {n:49, t:"Estudiar idiomas extranjeros –actuales y antiguos- para hacer traducción.", a:2},
    {n:50, t:"Restaurar piezas y obras de arte.", a:1},
    {n:51, t:"Revisar y dar mantenimiento a artefactos eléctricos, electrónicos y computadoras.", a:4},
    {n:52, t:"Enseñar a niños de 0 a 5 años.", a:2},
    {n:53, t:"Investigar y/o sondear nuevos mercados.", a:3},
    {n:54, t:"Atender la salud dental de las personas.", a:5},
    {n:55, t:"Tratar a niños, jóvenes y adultos con problemas psicológicos.", a:2},
    {n:56, t:"Crear estrategias de promoción y venta de nuevos productos ecuatorianos en el mercado internacional.", a:3},
    {n:57, t:"Planificar y recomendar dietas para personas diabéticas y/o con sobrepeso.", a:5},
    {n:58, t:"Trabajar en una empresa petrolera en un cargo técnico como control de la producción.", a:4},
    {n:59, t:"Administrar una empresa (familiar, privada o pública).", a:3},
    {n:60, t:"Tener un taller de reparación y mantenimiento de carros, tractores, etcétera.", a:4},
    {n:61, t:"Ejecutar proyectos de extracción minera y metalúrgica.", a:4},
    {n:62, t:"Asistir a directivos de multinacionales con manejo de varios idiomas.", a:3},
    {n:63, t:"Diseñar programas educativos para niños con discapacidad.", a:2},
    {n:64, t:"Aplicar conocimientos de estadística en investigaciones en diversas áreas (social, administrativa, salud, etcétera.)", a:4},
    {n:65, t:"Fotografiar hechos históricos, lugares significativos, rostros, paisajes para el área publicitaria, artística, periodística y social.", a:1},
    {n:66, t:"Trabajar en museos y bibliotecas nacionales e internacionales.", a:2},
    {n:67, t:"Ser parte de un grupo de teatro.", a:1},
    {n:68, t:"Producir cortometrajes, spots publicitarios, programas educativos, de ficción, etcétera.", a:1},
    {n:69, t:"Estudiar la influencia entre las corrientes marinas y el clima y sus consecuencias ecológicas.", a:5},
    {n:70, t:"Conocer las distintas religiones, su filosofía y transmitirlas a la comunidad en general.", a:2},
    {n:71, t:"Asesorar a inversionistas en la compra de bienes/acciones en mercados nacionales e internacionales.", a:3},
    {n:72, t:"Estudiar grupos étnicos, sus costumbres, tradiciones, cultura y comparar sus vivencias.", a:2},
    {n:73, t:"Explorar el espacio sideral, los planetas, características y componentes.", a:4},
    {n:74, t:"Mejorar la imagen facial y corporal de las personas aplicando diferentes técnicas.", a:5},
    {n:75, t:"Decorar jardines de casas y parques públicos.", a:1},
    {n:76, t:"Administrar y renovar menús de comidas en un hotel o restaurante.", a:5},
    {n:77, t:"Trabajar como presentador de televisión, locutor de radio y televisión, animador de programas culturales y concursos.", a:1},
    {n:78, t:"Diseñar y ejecutar programas de turismo.", a:2},
    {n:79, t:"Administrar y ordenar (planificar) adecuadamente la ocupación del espacio físico de ciudades, países etc., utilizando imágenes de satélite, mapas.", a:4},
    {n:80, t:"Organizar, planificar y administrar centros educativos.", a:3}
];

const areaInfo = {
    1: { name: "Área I", desc: "Artística", color: "#e11d48" },
    2: { name: "Área II", desc: "Humanística", color: "#2563eb" },
    3: { name: "Área III", desc: "Administrativa", color: "#d97706" },
    4: { name: "Área IV", desc: "Científica-Técnica", color: "#059669" },
    5: { name: "Área V", desc: "Salud-Biológica", color: "#7c3aed" }
};

// ===== INTERPRETACIONES VOCACIONALES =====
// EDITABLE POR EL PROFESIONAL — Modificar los textos según criterio experto
//
// El sistema detecta automáticamente 4 tipos de perfil:
//   1. DOMINANTE     : 1 área claramente por encima de las demás
//   2. MIXTO BINARIO : 2 áreas principales muy cercanas
//   3. MIXTO TRIPLE  : 3 áreas principales muy cercanas
//   4. MULTIPOTENCIAL: 4 o 5 áreas con puntuaciones similares (perfil equilibrado)
//
// Umbral de diferencia para considerar "cercanas": 2 puntos (configurable)
const UMBRAL_MIXTO = 2;

const interpretacionesPerfil = {
    // ───────────────────────────────────────────────
    // 1. PERFILES DOMINANTES (1 área clara ganadora)
    // ───────────────────────────────────────────────
    dominante: {
        1: {
            titulo: "Perfil Artístico",
            descripcion: "Tienes una marcada inclinación hacia actividades creativas y expresivas. Disfrutas del diseño, la música, las artes visuales y la comunicación audiovisual. Tu creatividad es tu principal fortaleza.",
            carreras: "Diseño gráfico, Bellas Artes, Música, Comunicación Audiovisual, Publicidad, Fotografía, Arquitectura de Interiores, Animación digital, Diseño de Moda.",
            consejo: "Busca espacios donde puedas expresarte libremente. Tu talento artístico puede combinarse con herramientas tecnológicas para ampliar tus posibilidades profesionales."
        },
        2: {
            titulo: "Perfil Humanístico",
            descripcion: "Te motiva comprender al ser humano, sus culturas, lenguajes y comportamientos. Tienes habilidades para la comunicación, la enseñanza y el análisis social.",
            carreras: "Psicología, Derecho, Pedagogía, Sociología, Antropología, Filosofía, Periodismo, Trabajo Social, Traducción e Interpretación, Ciencias Políticas.",
            consejo: "Tu sensibilidad social y capacidad de escucha son valiosas. Considera orientaciones que te permitan impactar positivamente en comunidades e individuos."
        },
        3: {
            titulo: "Perfil Administrativo",
            descripcion: "Destacas en la planificación, organización y gestión de recursos. Tienes facilidad para los números, la toma de decisiones estratégicas y el liderazgo.",
            carreras: "Administración de Empresas, Contabilidad, Economía, Marketing, Recursos Humanos, Comercio Internacional, Turismo, Finanzas, Gestión Pública.",
            consejo: "Tu visión organizativa te permite liderar equipos y proyectos. Desarrolla habilidades blandas para complementar tu perfil técnico-administrativo."
        },
        4: {
            titulo: "Perfil Científico-Técnico",
            descripcion: "Te atrae la resolución de problemas lógicos, el diseño de sistemas y la investigación científica. Disfrutas entender cómo funcionan las cosas.",
            carreras: "Ingeniería (Civil, Sistemas, Mecánica, Electrónica, Industrial), Arquitectura, Matemáticas, Física, Química, Estadística, Computación, Astronomía.",
            consejo: "Tu pensamiento analítico es una fortaleza clave. Mantente actualizado en tecnologías emergentes y busca proyectos que desafíen tu capacidad de innovación."
        },
        5: {
            titulo: "Perfil Salud-Biológica",
            descripcion: "Te interesa el cuidado de la vida, la salud humana, animal y el medio ambiente. Tienes vocación de servicio y curiosidad por los procesos biológicos.",
            carreras: "Medicina, Enfermería, Odontología, Nutrición, Veterinaria, Biología, Agronomía, Ecología, Farmacia, Fisioterapia, Bioquímica.",
            consejo: "Tu vocación de ayuda es admirable. La constancia y la empatía serán fundamentales en tu desarrollo profesional en el área de la salud."
        }
    },

    // ───────────────────────────────────────────────
    // 2. PERFILES MIXTOS BINARIOS (2 áreas principales)
    // ───────────────────────────────────────────────
    mixto2: {
        "1-2": {
            titulo: "Perfil Artístico-Humanístico",
            descripcion: "Combinas creatividad con sensibilidad social. Eres capaz de expresar ideas complejas de forma accesible y emocional. Tu comunicación tiene un sello personal y artístico.",
            carreras: "Comunicación Social, Diseño Editorial, Arteterapia, Docencia en Artes, Producción Cultural, Periodismo especializado, Guionismo, Community Management creativo."
        },
        "1-3": {
            titulo: "Perfil Artístico-Administrativo",
            descripcion: "Tienes visión creativa y sentido comercial. Puedes transformar ideas artísticas en proyectos viables y rentables. Eres innovador en el mundo de los negocios.",
            carreras: "Dirección de Arte, Branding, Gestión Cultural, Diseño de Producto, Marketing Creativo, Producción Audiovisual, Emprendimiento en industrias creativas."
        },
        "1-4": {
            titulo: "Perfil Artístico-Técnico",
            descripcion: "Fusionas creatividad con precisión técnica. Eres ideal para proyectos que requieren innovación y rigor metodológico. La tecnología es tu lienzo.",
            carreras: "Diseño Industrial, Arquitectura, Animación 3D, Desarrollo de Videojuegos, Ingeniería en Diseño, UX/UI, Realidad Virtual/Aumentada, Diseño de Hardware."
        },
        "1-5": {
            titulo: "Perfil Artístico-Salud",
            descripcion: "Tu creatividad se orienta al bienestar y la sanación. Tienes una visión holística del cuidado humano. El arte es tu herramienta de sanación.",
            carreras: "Musicoterapia, Arteterapia, Diseño de Espacios Terapéuticos, Estética profesional, Decoración de Ambientes de Salud, Diseño de productos médicos."
        },
        "2-3": {
            titulo: "Perfil Humanístico-Administrativo",
            descripcion: "Combinas comprensión humana con habilidades de gestión. Eres un puente entre personas y organizaciones. Liderás con empatía.",
            carreras: "Gestión de Talento Humano, Coaching, Consultoría Organizacional, Relaciones Públicas, Dirección de Fundaciones, Gestión de Proyectos Sociales, Mediación."
        },
        "2-4": {
            titulo: "Perfil Humanístico-Técnico",
            descripcion: "Aplicas el conocimiento técnico con una mirada humanista. Buscas que la tecnología sirva a las personas. Eres un traductor entre humanos y máquinas.",
            carreras: "Ingeniería Social, Ergonomía, Accesibilidad Tecnológica, Docencia en Ciencias Técnicas, Divulgación Científica, Experiencia de Usuario (UX Research)."
        },
        "2-5": {
            titulo: "Perfil Humanístico-Salud",
            descripcion: "Tu vocación de ayuda se complementa con una profunda comprensión del ser humano. Ideal para atención integral que va más allá de lo físico.",
            carreras: "Psicología Clínica, Terapia Ocupacional, Logopedia, Medicina Familiar, Trabajo Social en Salud, Psicopedagogía, Tanatología, Neuropsicología."
        },
        "3-4": {
            titulo: "Perfil Administrativo-Técnico",
            descripcion: "Eres capaz de gestionar proyectos tecnológicos y optimizar procesos con visión estratégica. La eficiencia y la innovación van de la mano.",
            carreras: "Gestión de Proyectos TI, Ingeniería Industrial, Logística, Supply Chain, Análisis de Datos, Consultoría Tecnológica, Transformación Digital, CIO/CTO."
        },
        "3-5": {
            titulo: "Perfil Administrativo-Salud",
            descripcion: "Puedes gestionar instituciones de salud con eficiencia y sensibilidad hacia el bienestar de pacientes. Liderás con cabeza y corazón.",
            carreras: "Administración Hospitalaria, Gestión de Servicios de Salud, Salud Pública, Auditoría Médica, Gestión de Calidad en Salud, Dirección de Clínicas, Epidemiología aplicada."
        },
        "4-5": {
            titulo: "Perfil Técnico-Salud",
            descripcion: "Aplicas el rigor científico al cuidado de la salud. Eres ideal para la investigación biomédica y tecnología sanitaria de vanguardia.",
            carreras: "Ingeniería Biomédica, Bioinformática, Tecnología Médica, Farmacología, Biotecnología, Ingeniería de Alimentos, Genética, Neuroingeniería."
        }
    },

    // ───────────────────────────────────────────────
    // 3. PERFILES MIXTOS TRIPLES (3 áreas principales)
    // ───────────────────────────────────────────────
    mixto3: {
        "1-2-3": {
            titulo: "Perfil Creativo-Comunicador-Gestor",
            descripcion: "Eres un creador con visión estratégica y habilidades de comunicación. Puedes concebir, comunicar y ejecutar proyectos creativos de principio a fin. Tu versatilidad te permite moverte entre equipos multidisciplinarios.",
            carreras: "Dirección de proyectos creativos, Producción cultural, Marketing de contenidos, Branding estratégico, Gestión de festivales, Comunicación institucional."
        },
        "1-2-4": {
            titulo: "Perfil Creativo-Humanista-Tecnológico",
            descripcion: "Unes la sensibilidad artística con el rigor técnico y la comprensión humana. Eres capaz de diseñar soluciones tecnológicas centradas en las personas, con un toque estético único.",
            carreras: "Diseño de interacción (IxD), Arquitectura de información, Docencia en tecnología creativa, Desarrollo de apps educativas, Museografía digital."
        },
        "1-2-5": {
            titulo: "Perfil Creativo-Humanista-Sanador",
            descripcion: "Tu creatividad y comprensión humana se orientan al bienestar. Tienes un enfoque holístico que integra expresión artística, apoyo emocional y cuidado de la salud.",
            carreras: "Arteterapia, Musicoterapia, Psicología del arte, Diseño de espacios de bienestar, Terapias alternativas, Comunicación en salud mental."
        },
        "1-3-4": {
            titulo: "Perfil Creativo-Emprendedor-Técnico",
            descripcion: "Eres un innovador que puede conceptualizar, planificar y construir. Tu mente creativa encuentra soluciones técnicas viables y comercialmente rentables.",
            carreras: "Emprendimiento tecnológico, Desarrollo de productos, Innovación corporativa, Startups, Propiedad intelectual, Consultoría de innovación."
        },
        "1-3-5": {
            titulo: "Perfil Creativo-Emprendedor-Sanador",
            descripcion: "Identificas oportunidades en el sector salud y bienestar desde una perspectiva creativa. Puedes lanzar negocios que mejoren la calidad de vida de las personas.",
            carreras: "Emprendimiento en salud, Diseño de productos wellness, Marketing farmacéutico, Gestión de centros estéticos, Innovación en nutrición, Telemedicina creativa."
        },
        "1-4-5": {
            titulo: "Perfil Creativo-Técnico-Sanador",
            descripcion: "Aplicas la innovación tecnológica al cuidado de la salud con un enfoque estético y humano. Eres ideal para el diseño de dispositivos médicos, apps de salud y ambientes terapéuticos.",
            carreras: "Diseño de dispositivos médicos, UX en salud, Ingeniería biomédica creativa, Apps de bienestar, Diseño de prótesis estéticas, Realidad virtual terapéutica."
        },
        "2-3-4": {
            titulo: "Perfil Humanista-Gestor-Técnico",
            descripcion: "Eres un líder que entiende tanto a las personas como a los sistemas. Puedes gestionar equipos técnicos con empatía y traducir necesidades humanas en soluciones tecnológicas organizadas.",
            carreras: "Gestión ágil de proyectos, Dirección de equipos de desarrollo, Consultoría de transformación digital, Gobierno de TI, Capacitación tecnológica."
        },
        "2-3-5": {
            titulo: "Perfil Humanista-Gestor-Sanador",
            descripcion: "Tienes una vocación de servicio integral que abarca la gestión, la comprensión humana y el cuidado de la salud. Eres un administrador con alma de cuidador.",
            carreras: "Dirección de centros de salud, Gestión de programas sociales en salud, Salud ocupacional, Políticas públicas de salud, Coordinación de equipos multidisciplinarios."
        },
        "2-4-5": {
            titulo: "Perfil Humanista-Técnico-Sanador",
            descripcion: "Unes la comprensión del ser humano con el rigor científico y la vocación de cuidado. Eres ideal para roles que requieren investigación aplicada al bienestar humano.",
            carreras: "Investigación en salud pública, Bioética, Epidemiología social, Ingeniería de tejidos, Neurociencia cognitiva, Psicología de la tecnología."
        },
        "3-4-5": {
            titulo: "Perfil Gestor-Técnico-Sanador",
            descripcion: "Eres un profesional que puede planificar, ejecutar y supervisar proyectos de alta complejidad en el sector salud y tecnología. Tu perfil es altamente demandado en la industria 4.0 de la salud.",
            carreras: "Gestión de hospitales tecnológicos, Dirección de laboratorios, Ingeniería clínica, Salud digital, Gestión de datos biomédicos, Auditoría de sistemas de salud."
        }
    },

    // ───────────────────────────────────────────────
    // 4. PERFIL MULTIPOTENCIAL (4 o 5 áreas cercanas)
    // ───────────────────────────────────────────────
    multipotencial: {
        titulo: "Perfil Multipotencial / Equilibrado",
        descripcion: "Tus intereses están distribuidos de manera equilibrada entre múltiples áreas. Esto indica una gran capacidad de adaptación, curiosidad intelectual versátil y habilidades transferibles. No te sientas presionado a especializarte en una sola cosa: tu fortaleza está justamente en conectar ideas de distintos campos.",
        carreras: "Gestión de proyectos multidisciplinarios, Consultoría generalista, Emprendimiento, Docencia universitaria, Investigación interdisciplinaria, Políticas públicas, Comunicación científica, Diseño de sistemas complejos.",
        consejo: "Tu perfil es un activo valioso en un mundo que valora cada vez más la interdisciplinariedad. Busca roles que te permitan explorar múltiples facetas. Considera carreras con énfasis integrador o diseña tu propia trayectoria profesional combinando varias disciplinas."
    }
};

// ===== FUNCIÓN AUXILIAR: Detectar tipo de perfil =====
function detectarTipoPerfil(counts) {
    // counts: objeto {1: N, 2: N, 3: N, 4: N, 5: N}
    // Devuelve: { tipo: 'dominante'|'mixto2'|'mixto3'|'multipotencial', areas: [area1, area2, ...] }
    //
    // LÓGICA: incluir TODAS las áreas cuya diferencia con el puntaje máximo
    // sea menor o igual a UMBRAL_MIXTO.

    const sorted = Object.entries(counts).sort((a, b) => b[1] - a[1]); // [[area, count], ...]
    const maxPuntaje = sorted[0][1];
    const areas = sorted.map(x => parseInt(x[0]));
    const puntajes = sorted.map(x => x[1]);

    // Incluir todas las áreas dentro del umbral del puntaje máximo
    const areasPrincipales = [];
    for (let i = 0; i < sorted.length; i++) {
        if (maxPuntaje - puntajes[i] <= UMBRAL_MIXTO) {
            areasPrincipales.push(areas[i]);
        } else {
            break; // Como están ordenadas, al encontrar una fuera del umbral, las siguientes también lo estarán
        }
    }

    const cantidad = areasPrincipales.length;

    if (cantidad === 1) {
        return { tipo: 'dominante', areas: areasPrincipales };
    }
    if (cantidad === 2) {
        return { tipo: 'mixto2', areas: areasPrincipales };
    }
    if (cantidad === 3) {
        return { tipo: 'mixto3', areas: areasPrincipales };
    }
    // 4 o 5 áreas
    return { tipo: 'multipotencial', areas: areasPrincipales };
}
function obtenerInterpretacion(tipoPerfil, areas) {
    if (tipoPerfil === 'dominante') {
        return interpretacionesPerfil.dominante[areas[0]];
    }
    if (tipoPerfil === 'mixto2') {
        const key = areas.slice().sort((a, b) => a - b).join('-');
        return interpretacionesPerfil.mixto2[key] || interpretacionesPerfil.dominante[areas[0]];
    }
    if (tipoPerfil === 'mixto3') {
        const key = areas.slice().sort((a, b) => a - b).join('-');
        return interpretacionesPerfil.mixto3[key] || interpretacionesPerfil.multipotencial;
    }
    if (tipoPerfil === 'multipotencial') {
        return interpretacionesPerfil.multipotencial;
    }
    return interpretacionesPerfil.dominante[areas[0]];
}

// ===== NUEVAS FUNCIONES: Volver, Imprimir cuestionario, Imprimir completo =====

function volverAlCuestionario() {
    document.getElementById('results').classList.add('hidden');
    document.getElementById('quiz').classList.remove('hidden');
    renderStep();
    updateProgress();
}

function imprimirCuestionarioDesdeResultados() {
    imprimirCuestionario();
}

function imprimirCompleto() {
    const nombreDisplay = evalNombre || '';
    const cedulaDisplay = evalCedula || '';
    const fechaDisplay = evalFecha || '';

    // ===== CALCULAR RESULTADOS =====
    const counts = {1:0, 2:0, 3:0, 4:0, 5:0};
    preguntas.forEach((p, i) => {
        if (answers[i] === true) counts[p.a]++;
    });

    // ===== DETECTAR TIPO DE PERFIL =====
    const perfilDetectado = detectarTipoPerfil(counts);
    const perfilInterpretado = obtenerInterpretacion(perfilDetectado.tipo, perfilDetectado.areas);

    // Preparar datos para el HTML
    const sorted = Object.entries(counts).sort((a,b) => b[1] - a[1]);
    const areaTop = perfilDetectado.areas[0];
    const infoTop = areaInfo[areaTop];

    // Subtítulo según tipo de perfil
    // Subtítulo dinámico según cantidad de áreas principales
    let subtituloTipo = '';
    const cantAreas = perfilDetectado.areas.length;
    if (cantAreas === 1) {
        subtituloTipo = `Área dominante: <strong>${infoTop.name} — ${infoTop.desc}</strong>`;
    } else {
        const nombresAreas = perfilDetectado.areas.map(a => `<strong>${areaInfo[a].name}</strong>`);
        subtituloTipo = `Áreas combinadas: ${nombresAreas.join(' + ')}`;
    }

    // Badge según tipo
    const badgeTexto = {
        'dominante': 'Perfil Dominante',
        'mixto2': 'Perfil Mixto (2 áreas)',
        'mixto3': 'Perfil Mixto (3 áreas)',
        'multipotencial': `Perfil Multipotencial (${cantAreas} áreas)`
    }[perfilDetectado.tipo];

    // ===== HTML DEL CUESTIONARIO =====
    let preguntasHTML = '';
    preguntas.forEach((p, i) => {
        const siMarcado = answers[i] === true ? '<span style="font-weight:700;font-size:10pt;">✕</span>' : '';
        const noMarcado = answers[i] === false ? '<span style="font-weight:700;font-size:10pt;">✕</span>' : '';
        preguntasHTML += `
        <div style="display:flex;align-items:flex-start;gap:8px;padding:5px 0;border-bottom:1px dotted #bbb;page-break-inside:avoid;">
            <span style="font-weight:700;font-size:10.5pt;min-width:22px;text-align:right;flex-shrink:0;">${p.n}.</span>
            <span style="flex:1;padding-top:1px;font-size:10.5pt;">${p.t}</span>
            <div style="display:flex;gap:10px;align-items:center;flex-shrink:0;">
                <div style="display:flex;flex-direction:column;align-items:center;gap:1px;">
                    <span style="width:14px;height:14px;border:1.5px solid #000;border-radius:50%;display:flex;align-items:center;justify-content:center;">${siMarcado}</span>
                    <span style="font-size:7pt;font-weight:700;">SI</span>
                </div>
                <div style="display:flex;flex-direction:column;align-items:center;gap:1px;">
                    <span style="width:14px;height:14px;border:1.5px solid #000;border-radius:50%;display:flex;align-items:center;justify-content:center;">${noMarcado}</span>
                    <span style="font-size:7pt;font-weight:700;">NO</span>
                </div>
            </div>
        </div>`;
    });

    // ===== HTML DE RESULTADOS (barras) =====
    let resultadosHTML = '';
    sorted.forEach(([area, count]) => {
        const info = areaInfo[area];
        const pct = Math.round((count / 16) * 100);
        const esPrincipal = perfilDetectado.areas.includes(parseInt(area));
        const opacidad = esPrincipal ? '1' : '0.6';
        resultadosHTML += `
        <div style="margin-bottom:12px;page-break-inside:avoid;opacity:${opacidad};">
            <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px;">
                <span style="font-weight:700;font-size:11pt;color:${info.color}">${info.name} — ${info.desc}</span>
                <span style="font-weight:700;font-size:11pt;">${count}/16 (${pct}%)</span>
            </div>
            <div style="width:100%;height:18px;background:#e5e7eb;border-radius:99px;overflow:hidden;">
                <div style="width:${pct}%;height:100%;background:${info.color};border-radius:99px;"></div>
            </div>
        </div>`;
    });

    // ===== HTML DE INTERPRETACIÓN =====
    const interpretacionHTML = `
    <div class="interpretacion-box">
        <div class="interp-header">
            <div class="interp-badge" style="background:${infoTop.color}20;color:${infoTop.color};border:2px solid ${infoTop.color};">
                ${badgeTexto}
            </div>
            <h2 style="font-size:18pt;margin:10px 0 4px 0;color:${infoTop.color};">${perfilInterpretado.titulo}</h2>
            <p style="font-size:9pt;color:#666;margin:0;">${subtituloTipo}</p>
        </div>

        <div class="interp-section">
            <h3>📝 Descripción del Perfil</h3>
            <p>${perfilInterpretado.descripcion}</p>
        </div>

        <div class="interp-section">
            <h3>🎓 Orientaciones Profesionales Sugeridas</h3>
            <p>${perfilInterpretado.carreras}</p>
        </div>

        <div class="interp-section consejo">
            <h3>💡 Recomendación</h3>
            <p>${perfilInterpretado.consejo || 'Explora las áreas que más te motivan y busca orientación profesional personalizada para profundizar en tu perfil.'}</p>
        </div>

        <div class="interp-nota">
            <strong>Nota del profesional:</strong> Esta interpretación es una guía orientativa basada en los resultados del test.
            Se recomienda una entrevista personalizada para una orientación vocacional más precisa.
        </div>
    </div>`;

    // ===== HTML COMPLETO =====
    const htmlContent = `<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>O.V.O. Completo — ${nombreDisplay || 'Resultados'}</title>
<style>
@page { size: A4; margin: 14mm 16mm; }
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    font-size: 10.5pt;
    line-height: 1.4;
    color: #000;
    background: #fff;
    padding: 10mm;
}
.header {
    border-bottom: 2px solid #000;
    padding-bottom: 8px;
    margin-bottom: 10px;
}
.header-top {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 6px;
}
.header-top h1 {
    font-size: 20pt;
    letter-spacing: 3px;
    margin: 0;
}
.header-top .version {
    font-size: 8pt;
    color: #444;
    text-align: right;
}
.header-sub {
    font-size: 9pt;
    color: #333;
    margin-bottom: 8px;
}
.datos {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
    margin-bottom: 8px;
}
.campo {
    flex: 1;
    min-width: 140px;
    display: flex;
    align-items: baseline;
    gap: 4px;
}
.campo label {
    font-size: 7.5pt;
    text-transform: uppercase;
    font-weight: 700;
    white-space: nowrap;
}
.campo .line {
    flex: 1;
    border-bottom: 1px solid #000;
    min-width: 60px;
    height: 16px;
    padding-left: 4px;
    font-size: 10pt;
}
.section-title {
    font-size: 14pt;
    font-weight: 700;
    margin: 20px 0 10px 0;
    padding-bottom: 4px;
    border-bottom: 1px solid #000;
}
.results-section {
    background: #fafafa;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 16px;
    margin-bottom: 16px;
}
.interpretacion-box {
    background: #fff;
    border: 2px solid #e5e7eb;
    border-radius: 12px;
    padding: 20px;
    margin-bottom: 20px;
    page-break-inside: avoid;
}
.interp-header {
    text-align: center;
    margin-bottom: 16px;
    padding-bottom: 12px;
    border-bottom: 1px dashed #ddd;
}
.interp-badge {
    display: inline-block;
    padding: 4px 16px;
    border-radius: 99px;
    font-size: 9pt;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 1px;
}
.interp-section {
    margin-bottom: 14px;
}
.interp-section h3 {
    font-size: 11pt;
    font-weight: 700;
    margin-bottom: 6px;
    color: #1a1a2e;
}
.interp-section p {
    font-size: 10pt;
    color: #333;
    text-align: justify;
}
.interp-section.consejo {
    background: #f0f9ff;
    border-left: 4px solid #0ea5e9;
    padding: 10px 14px;
    border-radius: 0 8px 8px 0;
}
.interp-nota {
    font-size: 8.5pt;
    color: #666;
    font-style: italic;
    text-align: center;
    margin-top: 14px;
    padding-top: 10px;
    border-top: 1px solid #eee;
}
.instrucciones {
    background: #f5f5f5;
    border-left: 3px solid #000;
    padding: 5px 10px;
    margin-bottom: 10px;
    font-size: 9pt;
}
.footer {
    margin-top: 10px;
    text-align: center;
    font-size: 7.5pt;
    color: #555;
    border-top: 1px solid #ccc;
    padding-top: 5px;
}
.page-break {
    page-break-before: always;
}
</style>
</head>
<body>
    <!-- PORTADA / DATOS -->
    <div class="header">
        <div class="header-top">
            <h1>O.V.O.</h1>
            <div class="version">Informe Completo<br>TechFaRo © 2024</div>
        </div>
        <div class="header-sub">Orientación Vocacional y Ocupacional — Cuestionario de Intereses</div>
        <div class="datos">
            <div class="campo"><label>Nombre completo</label><div class="line">${nombreDisplay}</div></div>
            <div class="campo"><label>Cédula de identidad</label><div class="line">${cedulaDisplay}</div></div>
            <div class="campo"><label>Fecha</label><div class="line">${fechaDisplay}</div></div>
        </div>
    </div>

    <!-- RESULTADOS GRÁFICOS -->
    <div class="section-title">📊 Resultados por Área</div>
    <div class="results-section">
        ${resultadosHTML}
    </div>

    <!-- INTERPRETACIÓN PROFESIONAL -->
    <div class="section-title">🎯 Interpretación del Perfil</div>
    ${interpretacionHTML}

    <!-- CUESTIONARIO -->
    <div class="page-break"></div>
    <div class="section-title">📋 Cuestionario de Intereses</div>
    <div class="instrucciones">
        <strong>Instrucciones:</strong> Lee cada actividad y marca con una <strong>X</strong> dentro del círculo si te <strong>gustaría</strong> realizarla (SI), o déjalo en blanco si <strong>no te interesa</strong> (NO). No hay respuestas correctas ni incorrectas.
    </div>
    ${preguntasHTML}

    <div class="footer">
        O.V.O. — Orientación Vocacional y Ocupacional | TechFaRo © 2024
    </div>
    <script>window.onload = function() { setTimeout(function() { window.print(); }, 500); };</script>
</body>
</html>`;

    const blob = new Blob([htmlContent], { type: 'text/html' });
    const url = URL.createObjectURL(blob);
    const w = window.open(url, '_blank');
    if (!w) {
        const a = document.createElement('a');
        a.href = url;
        a.download = `OVO_Completo_${(nombreDisplay || 'resultados').replace(/\s+/g, '_')}.html`;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
    }
}


// ===== INTERPRETACIONES VOCACIONALES =====
// EDITABLE POR EL PROFESIONAL — Modificar los textos según criterio experto
const STEP_SIZE = 10;
const TOTAL_STEPS = Math.ceil(preguntas.length / STEP_SIZE);
let currentStep = 0;
let answers = new Array(preguntas.length).fill(null);
let chartInstance = null;

// Datos del evaluado (pueden quedar vacíos)
let evalNombre = '';
let evalCedula = '';
let evalFecha = '';

// ===== AUTH (usuarios exactos del login.js del repo) =====
const usuariosAuth = [
    { usuario: "RkFCSU9S", contrasenia: "MzAwNDE1UkZj", Persona: "Fabio" },
    { usuario: "UklUQU0=", contrasenia: "Q2FuZGVsYXJpYTIwMTU=", Persona: "Rita" },
    { usuario: "Q0FOREVMQVJJQVJN", contrasenia: "MjAxNUNhbmRl", Persona: "Candelaria" }
];

function limpiarTexto(texto) {
    return texto.replace(/[.,_/\\!?'"()[\]{}@#$%^&+*=~-]/g, '');
}

function codificar(param) {
    return btoa(param);
}

function validarUsuario(usuario, contrasenia, listaUsuarios) {
    let usuarioLimpio = limpiarTexto(usuario).toUpperCase();
    let contraseniaLimpia = limpiarTexto(contrasenia);

    if (!usuarioLimpio || !contraseniaLimpia) {
        return "Por favor, ingresa tu usuario y contraseña.";
    }
    if (usuarioLimpio.length < 5) {
        return "El usuario debe tener al menos 5 caracteres.";
    }
    if (contraseniaLimpia.length < 8) {
        return "La contraseña debe tener al menos 8 caracteres.";
    }

    let usuarioCodificado = codificar(usuarioLimpio);
    let contraseniaCodificada = codificar(contraseniaLimpia);

    for (const user of listaUsuarios) {
        if (usuarioCodificado === user.usuario && contraseniaCodificada === user.contrasenia) {
            return true;
        }
    }
    return "Usuario y/o contraseña incorrecto/s";
}

// ===== THEME =====
function toggleTheme() {
    const html = document.documentElement;
    const current = html.getAttribute('data-theme');
    html.setAttribute('data-theme', current === 'dark' ? 'light' : 'dark');
    localStorage.setItem('ovo-theme', html.getAttribute('data-theme'));
}
(function initTheme() {
    const saved = localStorage.getItem('ovo-theme') || 'light';
    document.documentElement.setAttribute('data-theme', saved);
})();

// ===== DATOS MODAL (antes de empezar) =====
function openDatosModal() {
    document.getElementById('datosModal').classList.remove('hidden');
    document.getElementById('datosNombre').focus();
}

function closeDatosModal() {
    document.getElementById('datosModal').classList.add('hidden');
    document.getElementById('datosError').textContent = '';
}

function guardarDatosYOmitir() {
    evalNombre = document.getElementById('datosNombre').value.trim();
    evalCedula = document.getElementById('datosCedula').value.trim();
    evalFecha = document.getElementById('datosFecha').value.trim();
    closeDatosModal();
    iniciarQuiz();
}

function iniciarQuiz() {
    document.getElementById('welcome').classList.add('hidden');
    document.getElementById('quiz').classList.remove('hidden');
    renderDots();
    renderStep();
    updateProgress();
}

// ===== NAVIGATION =====
function startQuiz() {
    openDatosModal();
}

function renderDots() {
    const container = document.getElementById('stepsDots');
    container.innerHTML = '';
    for (let i = 0; i < TOTAL_STEPS; i++) {
        const dot = document.createElement('div');
        dot.className = 'step-dot';
        dot.onclick = () => { currentStep = i; renderStep(); updateProgress(); };
        container.appendChild(dot);
    }
}

function renderStep() {
    const container = document.getElementById('questionsContainer');
    container.innerHTML = '';
    const start = currentStep * STEP_SIZE;
    const end = Math.min(start + STEP_SIZE, preguntas.length);

    for (let i = start; i < end; i++) {
        const p = preguntas[i];
        const card = document.createElement('div');
        card.className = 'question-card';
        card.dataset.index = i;
        if (answers[i] === true) card.classList.add('answered-yes');
        if (answers[i] === false) card.classList.add('answered-no');
        card.style.animationDelay = ((i - start) * 0.05) + 's';

        card.innerHTML = `
            <span class="q-number">${p.n}</span>
            <span class="q-text">${p.t}</span>
            <div class="q-actions">
                <button class="btn-answer yes-btn ${answers[i] === true ? 'selected' : ''}" onclick="answer(${i}, true)">SI</button>
                <button class="btn-answer no-btn ${answers[i] === false ? 'selected' : ''}" onclick="answer(${i}, false)">NO</button>
            </div>
        `;
        container.appendChild(card);
    }

    document.querySelectorAll('.step-dot').forEach((dot, i) => {
        dot.classList.remove('active', 'completed');
        if (i === currentStep) dot.classList.add('active');
        else if (i < currentStep) dot.classList.add('completed');
    });

    document.getElementById('btnPrev').disabled = currentStep === 0;
    const btnNext = document.getElementById('btnNext');
    if (currentStep === TOTAL_STEPS - 1) {
        btnNext.textContent = 'Ver resultados ✓';
    } else {
        btnNext.textContent = 'Siguiente →';
    }
}

function answer(index, value) {
    answers[index] = value;
    const card = document.querySelector(`.question-card[data-index="${index}"]`);
    card.classList.remove('answered-yes', 'answered-no');
    card.classList.add(value ? 'answered-yes' : 'answered-no');
    card.querySelector('.yes-btn').classList.toggle('selected', value === true);
    card.querySelector('.no-btn').classList.toggle('selected', value === false);
    updateProgress();

    const start = currentStep * STEP_SIZE;
    const end = Math.min(start + STEP_SIZE, preguntas.length);
    let allAnswered = true;
    for (let i = start; i < end; i++) {
        if (answers[i] === null) { allAnswered = false; break; }
    }
    if (allAnswered && currentStep < TOTAL_STEPS - 1) {
        setTimeout(() => { currentStep++; renderStep(); updateProgress(); }, 400);
    }
}

function updateProgress() {
    const answered = answers.filter(a => a !== null).length;
    const pct = Math.round((answered / preguntas.length) * 100);
    document.getElementById('progressText').textContent = `Pregunta ${Math.min(answered + 1, 80)} de 80`;
    document.getElementById('progressPercent').textContent = pct + '%';
    document.getElementById('progressFill').style.width = pct + '%';
}

function nextStep() {
    if (currentStep < TOTAL_STEPS - 1) {
        currentStep++;
        renderStep();
        updateProgress();
    } else {
        showLoginForResults();
    }
}

function prevStep() {
    if (currentStep > 0) {
        currentStep--;
        renderStep();
        updateProgress();
    }
}

// ===== KEYBOARD =====
document.addEventListener('keydown', (e) => {
    if (document.getElementById('quiz').classList.contains('hidden')) return;
    const start = currentStep * STEP_SIZE;
    const end = Math.min(start + STEP_SIZE, preguntas.length);
    const firstUnanswered = answers.findIndex((a, i) => i >= start && i < end && a === null);
    const targetIndex = firstUnanswered !== -1 ? firstUnanswered : start;

    if (e.key === '1') answer(targetIndex, true);
    if (e.key === '2') answer(targetIndex, false);
    if (e.key === 'ArrowRight') nextStep();
    if (e.key === 'ArrowLeft') prevStep();
});

// ===== LOGIN MODAL (solo para ver resultados) =====
function showLoginForResults() {
    const unanswered = answers.filter(a => a === null).length;
    if (unanswered > 0) {
        alert(`Faltan ${unanswered} preguntas por responder. Completa el cuestionario antes de ver los resultados.`);
        return;
    }
    document.getElementById('loginModal').classList.remove('hidden');
    document.getElementById('modalUser').focus();
}

function closeLoginModal() {
    document.getElementById('loginModal').classList.add('hidden');
    document.getElementById('modalError').textContent = '';
    document.getElementById('modalUser').value = '';
    document.getElementById('modalPass').value = '';
}

function checkLoginAndShow() {
    const user = document.getElementById('modalUser').value;
    const pass = document.getElementById('modalPass').value;
    const errorDiv = document.getElementById('modalError');

    const resultado = validarUsuario(user, pass, usuariosAuth);

    if (resultado === true) {
        closeLoginModal();
        mostrarResultadosFinales();
    } else {
        errorDiv.textContent = resultado;
        document.getElementById('modalPass').value = '';
        document.getElementById('modalPass').focus();
    }
}

// ===== IMPRIMIR (sin login) =====
function imprimirCuestionario() {
    generarPDF(evalNombre, evalCedula, evalFecha);
}

function generarPDF(nombre, cedula, fecha) {
    const nombreDisplay = nombre || '';
    const cedulaDisplay = cedula || '';
    const fechaDisplay = fecha || '';

    let preguntasHTML = '';
    preguntas.forEach((p, i) => {
        const siMarcado = answers[i] === true ? '<span style="font-weight:700;font-size:10pt;">✕</span>' : '';
        const noMarcado = answers[i] === false ? '<span style="font-weight:700;font-size:10pt;">✕</span>' : '';
        preguntasHTML += `
        <div style="display:flex;align-items:flex-start;gap:8px;padding:5px 0;border-bottom:1px dotted #bbb;page-break-inside:avoid;">
            <span style="font-weight:700;font-size:10.5pt;min-width:22px;text-align:right;flex-shrink:0;">${p.n}.</span>
            <span style="flex:1;padding-top:1px;font-size:10.5pt;">${p.t}</span>
            <div style="display:flex;gap:10px;align-items:center;flex-shrink:0;">
                <div style="display:flex;flex-direction:column;align-items:center;gap:1px;">
                    <span style="width:14px;height:14px;border:1.5px solid #000;border-radius:50%;display:flex;align-items:center;justify-content:center;">${siMarcado}</span>
                    <span style="font-size:7pt;font-weight:700;">SI</span>
                </div>
                <div style="display:flex;flex-direction:column;align-items:center;gap:1px;">
                    <span style="width:14px;height:14px;border:1.5px solid #000;border-radius:50%;display:flex;align-items:center;justify-content:center;">${noMarcado}</span>
                    <span style="font-size:7pt;font-weight:700;">NO</span>
                </div>
            </div>
        </div>`;
    });

    const htmlContent = `<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>O.V.O. - ${nombreDisplay || 'Cuestionario'}</title>
<style>
@page { size: A4; margin: 14mm 16mm; }
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    font-size: 10.5pt;
    line-height: 1.3;
    color: #000;
    background: #fff;
    padding: 10mm;
}
.header {
    border-bottom: 2px solid #000;
    padding-bottom: 8px;
    margin-bottom: 10px;
}
.header-top {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 6px;
}
.header-top h1 {
    font-size: 20pt;
    letter-spacing: 3px;
    margin: 0;
}
.header-top .version {
    font-size: 8pt;
    color: #444;
    text-align: right;
}
.header-sub {
    font-size: 9pt;
    color: #333;
    margin-bottom: 8px;
}
.datos {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
    margin-bottom: 8px;
}
.campo {
    flex: 1;
    min-width: 140px;
    display: flex;
    align-items: baseline;
    gap: 4px;
}
.campo label {
    font-size: 7.5pt;
    text-transform: uppercase;
    font-weight: 700;
    white-space: nowrap;
}
.campo .line {
    flex: 1;
    border-bottom: 1px solid #000;
    min-width: 60px;
    height: 16px;
    padding-left: 4px;
    font-size: 10pt;
}
.instrucciones {
    background: #f5f5f5;
    border-left: 3px solid #000;
    padding: 5px 10px;
    margin-bottom: 10px;
    font-size: 9pt;
}
.footer {
    margin-top: 10px;
    text-align: center;
    font-size: 7.5pt;
    color: #555;
    border-top: 1px solid #ccc;
    padding-top: 5px;
}
</style>
</head>
<body>
    <div class="header">
        <div class="header-top">
            <h1>O.V.O.</h1>
            <div class="version">Versión Imprimible<br>TechFaRo © 2024</div>
        </div>
        <div class="header-sub">Orientación Vocacional y Ocupacional — Cuestionario de Intereses</div>
        <div class="datos">
            <div class="campo"><label>Nombre completo</label><div class="line">${nombreDisplay}</div></div>
            <div class="campo"><label>Cédula de identidad</label><div class="line">${cedulaDisplay}</div></div>
            <div class="campo"><label>Fecha</label><div class="line">${fechaDisplay}</div></div>
        </div>
    </div>
    <div class="instrucciones">
        <strong>Instrucciones:</strong> Lee cada actividad y marca con una <strong>X</strong> dentro del círculo si te <strong>gustaría</strong> realizarla (SI), o déjalo en blanco si <strong>no te interesa</strong> (NO). No hay respuestas correctas ni incorrectas. Sé honesto/a contigo mismo/a.
    </div>
    ${preguntasHTML}
    <div class="footer">
        O.V.O. — Orientación Vocacional y Ocupacional | TechFaRo © 2024
    </div>
    <script>window.onload = function() { setTimeout(function() { window.print(); }, 500); };</script>
</body>
</html>`;

    const blob = new Blob([htmlContent], { type: 'text/html' });
    const url = URL.createObjectURL(blob);
    const w = window.open(url, '_blank');
    if (!w) {
        const a = document.createElement('a');
        a.href = url;
        a.download = `OVO_${(nombreDisplay || 'cuestionario').replace(/\s+/g, '_')}.html`;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
    }
}

// ===== RESULTS =====
function mostrarResultadosFinales() {
    document.getElementById('quiz').classList.add('hidden');
    document.getElementById('results').classList.remove('hidden');

    const counts = {1:0, 2:0, 3:0, 4:0, 5:0};
    preguntas.forEach((p, i) => {
        if (answers[i] === true) counts[p.a]++;
    });

    const grid = document.getElementById('resultsGrid');
    grid.innerHTML = '';
    const sorted = Object.entries(counts).sort((a,b) => b[1] - a[1]);
    sorted.forEach(([area, count]) => {
        const info = areaInfo[area];
        const pct = Math.round((count / 16) * 100);
        const card = document.createElement('div');
        card.className = 'result-card';
        card.innerHTML = `
            <div class="area-name" style="color:${info.color}">${info.name} — ${info.desc}</div>
            <div class="area-desc">${count} de 16 respuestas positivas</div>
            <div class="result-bar-bg">
                <div class="result-bar-fill" style="background:${info.color};width:0%"></div>
            </div>
            <div class="result-stats">
                <span style="color:${info.color}">${count}/16</span>
                <span style="color:${info.color}">${pct}%</span>
            </div>
        `;
        grid.appendChild(card);
        setTimeout(() => {
            card.querySelector('.result-bar-fill').style.width = pct + '%';
        }, 200);
    });

    const ctx = document.getElementById('resultsChart').getContext('2d');
    if (chartInstance) chartInstance.destroy();
    chartInstance = new Chart(ctx, {
        type: 'bar',
        data: {
            labels: ['Área I\nArtística', 'Área II\nHumanística', 'Área III\nAdministrativa', 'Área IV\nCientífica-Téc.', 'Área V\nSalud-Biológica'],
            datasets: [{
                label: 'Respuestas SI',
                data: [counts[1], counts[2], counts[3], counts[4], counts[5]],
                backgroundColor: ['#e11d48', '#2563eb', '#d97706', '#059669', '#7c3aed'],
                borderRadius: 8,
                borderSkipped: false,
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            animation: { duration: 1200, easing: 'easeOutQuart' },
            plugins: {
                legend: { display: false },
                tooltip: {
                    callbacks: {
                        label: (ctx) => `${ctx.raw} de 16 (${Math.round(ctx.raw/16*100)}%)`
                    }
                }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    max: 16,
                    ticks: { stepSize: 2, color: getComputedStyle(document.body).getPropertyValue('--text-light') },
                    grid: { color: getComputedStyle(document.body).getPropertyValue('--border') }
                },
                x: {
                    ticks: { color: getComputedStyle(document.body).getPropertyValue('--text') },
                    grid: { display: false }
                }
            }
        }
    });
}


// ===== NUEVAS FUNCIONES: Volver, Imprimir cuestionario, Imprimir completo =====



} else {
	// No hay sesión iniciada: volver al login
	window.location.href = darUrl('index.html');
}

```

