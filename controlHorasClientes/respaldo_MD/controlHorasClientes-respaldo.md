# Bundle del Proyecto para IA: controlHorasClientes

## Estructura de Archivos
- README.md
- css/clientes.css
- css/dashboard.css
- css/informes.css
- css/login.css
- css/main.css
- css/old/clientes.css
- css/old/dashboard.css
- css/old/informes.css
- css/old/login.css
- css/old/main.css
- css/old/registro_horas.css
- css/registro_horas.css
- data/horasClientes copy.json
- data/horasClientes.json
- data/listaClientes copy.json
- data/listaClientes.json
- data/usuarios.json
- documentacion/LEEME.md
- hash_sha256.html
- html/clientes.html
- html/dashboard.html
- html/informes.html
- html/informes2.html
- html/login.html
- html/registro_horas.html
- imagenes/logo_Shalon.png
- index.html
- js/clientes.js
- js/config.js
- js/informes.js
- js/login.js
- js/registro_horas.js
- netlify/functions/package.json
- netlify/functions/update-clientes.js
- netlify/functions/update-horas.js
- package.json

---

## Código Fuente

### Archivo: `README.md`
```md
# Control Horas Clientes

Aplicación web para registrar las horas trabajadas para cada cliente y sacar informes. Tiene login de usuarios, alta de clientes, carga de horas e informes de resumen.

Sitio publicado: https://controlhorasclientes.netlify.app

## Funcionalidades

- **Login** con usuario y contraseña. Las contraseñas se guardan como hash SHA-256 en `data/usuarios.json`.
- **Panel principal** con acceso a las distintas secciones.
- **Clientes**: alta y modificación de la lista de clientes.
- **Registrar horas**: carga de horas trabajadas por cliente y fecha.
- **Informes**: resumen de horas por cliente.

## Cómo se usa

1. Entrá al sitio e iniciá sesión.
2. En **Clientes** cargá los clientes con los que trabajás.
3. En **Registrar Horas** anotá las horas de cada trabajo.
4. En **Informes** consultá el resumen de horas por cliente.

## Cómo funciona

- El frontend es HTML, CSS y JavaScript, sin build.
- Los datos están en archivos JSON dentro de `data/`:
  - `usuarios.json`: usuarios y hash de contraseña.
  - `listaClientes.json`: clientes.
  - `horasClientes.json`: horas registradas.
- Para guardar cambios, la app llama a dos Netlify Functions que actualizan esos archivos en el repositorio a través de la API de GitHub:
  - `update-clientes.js`
  - `update-horas.js`
- `hash_sha256.html` es una utilidad para calcular el hash de una contraseña nueva antes de agregarla a `usuarios.json`.

## Publicación en Netlify

Configurar estas variables de entorno:

| Variable | Para qué sirve |
|---|---|
| `GITHUB_TOKEN` | Token con permiso de escritura sobre este repositorio. |
| `GITHUB_REPO` | Repositorio donde están los JSON (por ejemplo `FABIOR1981/controlHorasClientes`). |
| `GITHUB_BRANCH` | Rama donde se guardan los cambios (por ejemplo `main`). |

## Estructura

```
index.html                 Entrada de la aplicación
html/                      Pantallas: login, dashboard, clientes, registro de horas, informes
js/                        Lógica de cada pantalla y configuración (config.js)
css/                       Estilos de cada pantalla
data/                      Datos en JSON (usuarios, clientes, horas)
netlify/functions/         Funciones que guardan los JSON en GitHub
documentacion/LEEME.md     Aviso: la documentación está en documentacion-central
```

Más información en [DETALLE.md](https://github.com/FABIOR1981/documentacion-central/blob/main/controlHorasClientes/documentacion/DETALLE.md), en documentacion-central.

```

### Archivo: `css/clientes.css`
```css
/* clientes.css */
button:hover {
    background: var(--hover-green) !important;
}

button {
    color: var(--text-white) !important;
}

.clientes-container {
    width: 100%;
    max-width: none;
    margin: 0;
    background: #fff;
    border-radius: 8px;
    box-shadow: none;
    padding: 24px;
    box-sizing: border-box;
}
.clientes-container h2 { margin-bottom: 24px; }
.tabla-clientes { width: 100%; border-collapse: collapse; margin-top: 24px; }
.tabla-clientes th, .tabla-clientes td { border: 1px solid #ccc; padding: 6px; text-align: left; }
.tabla-clientes th { background: #eee; }
.inactivo { color: #c00; }

.clientes-container input, .clientes-container select {
    width: 100%;
    margin-bottom: 12px;
    padding: 8px;
    border-radius: 4px;
    border: 1px solid #ccc;
    box-sizing: border-box;
}
.clientes-container button {
    padding: 10px 24px;
    background: var(--primary-green);
    color: var(--text-white);
    border: none;
    border-radius: 4px;
    font-size: 16px;
    margin-bottom: 16px;
    transition: background 0.2s;
}
.clientes-container button#print-btn {
    background: var(--print-blue);
    color: var(--text-white);
}
```

### Archivo: `css/dashboard.css`
```css
/* dashboard.css */
button {
    color: var(--text-white) !important;
}

body {
    min-height: 100vh;
    overflow: auto;
    background: linear-gradient(135deg, #f4f4f4 0%, #e8f5e9 100%);
}

.dashboard-shell {
    display: flex;
    min-height: 100vh;
    width: 100vw;
    max-width: 100%;
}

.dashboard-sidebar {
    width: 240px;
    flex-shrink: 0;
    background: #2e7d32;
    color: #fff;
    padding: 20px 16px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    position: sticky;
    top: 0;
    height: 100vh;
    box-shadow: 2px 0 12px rgba(0,0,0,0.15);
}

.sidebar-brand {
    margin-bottom: 16px;
}

.sidebar-brand img {
    display: block;
    width: 100%;
    max-width: 180px;
    margin: 0 auto;
}

.sidebar-nav {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.dashboard-btn {
    display: block;
    width: 100%;
    margin: 0;
    padding: 14px 16px;
    font-size: 16px;
    border-radius: 8px;
    border: none;
    background: rgba(255,255,255,0.16);
    color: var(--text-white);
    cursor: pointer;
    transition: background 0.2s ease, transform 0.2s ease;
    font-weight: 600;
    text-decoration: none;
    text-align: center;
}

.dashboard-btn:hover {
    background: rgba(255,255,255,0.24);
    transform: translateX(2px);
}

.dashboard-btn.active {
    background: rgba(255,255,255,0.32);
    box-shadow: inset 0 0 0 1px rgba(255,255,255,0.2);
}

.logout-btn {
    margin-top: auto;
    background: #1b5e20;
}

.dashboard-content {
    flex: 1;
    min-width: 0;
    padding: 24px 24px 16px;
    overflow: auto;
}

.dashboard-header {
    margin-bottom: 32px;
}

.dashboard-header h1 {
    margin: 0 0 8px;
    color: #1b5e20;
}

.dashboard-header p {
    margin: 0;
    color: #5f6b68;
    font-size: 1rem;
}

.dashboard-home {
    display: block;
}

.home-summary-card {
    background: #fff;
    border-radius: 16px;
    padding: 24px;
    box-shadow: 0 6px 20px rgba(0,0,0,0.08);
}

.home-summary-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 16px;
    margin-bottom: 20px;
}

.home-summary-header h2 {
    margin: 0 0 6px;
    color: #2e7d32;
}

.home-summary-header p {
    margin: 0;
    color: #606060;
}

.range-select {
    display: flex;
    flex-direction: column;
    gap: 6px;
    font-size: 0.9rem;
    color: #2e7d32;
    font-weight: 600;
}

.range-select select {
    border: 1px solid #c8e6c9;
    border-radius: 8px;
    padding: 8px 10px;
    background: #f8fff8;
    color: #1b5e20;
}

.home-summary-content {
    display: flex;
    gap: 24px;
    align-items: center;
    flex-wrap: wrap;
}

.chart-wrapper {
    position: relative;
    width: 280px;
    height: 280px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.hours-chart {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background: linear-gradient(135deg, #e8f5e9 0%, #f4f4f4 100%);
    box-shadow: inset 0 0 0 12px rgba(255,255,255,0.8);
}

.chart-center {
    position: absolute;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 120px;
    height: 120px;
    border-radius: 50%;
    background: #fff;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    color: #2e7d32;
    text-align: center;
}

.chart-center strong {
    font-size: 1.2rem;
}

.legend-list {
    list-style: none;
    padding: 0;
    margin: 0;
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 10px;
    min-width: 220px;
    width: 100%;
    max-width: 360px;
}

.legend-item {
    display: flex;
    align-items: center;
    gap: 10px;
    background: #f8fff8;
    padding: 8px 10px;
    border-radius: 10px;
    min-height: 44px;
}

.legend-item strong {
    display: block;
    color: #1b5e20;
}

.legend-item small {
    color: #5f6b68;
}

.legend-dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    display: inline-block;
    flex-shrink: 0;
}

.legend-empty {
    color: #666;
    padding: 8px 0;
}

.dashboard-frame {
    display: none;
    width: 100%;
    min-height: calc(100vh - 140px);
    height: calc(100vh - 140px);
    border: none;
    border-radius: 12px;
    box-shadow: 0 4px 16px rgba(0,0,0,0.08);
    background: #fff;
    overflow: auto;
}

.dashboard-footer {
    text-align: center;
    color: #888;
    font-size: 13px;
    padding: 20px 16px 28px;
    background: #f4f4f4;
}

@media (max-width: 768px) {
    .dashboard-shell {
        flex-direction: column;
    }

    .dashboard-sidebar {
        width: 100%;
        height: auto;
        position: relative;
    }

    .dashboard-content {
        padding: 24px 16px 16px;
    }

    .home-summary-header {
        flex-direction: column;
    }

    .chart-wrapper {
        width: 240px;
        height: 240px;
    }

    .legend-list {
        grid-template-columns: 1fr;
        max-width: none;
    }
}
```

### Archivo: `css/informes.css`
```css
/* informes.css */
button:hover {
    background: var(--hover-green) !important;
}

button {
    color: var(--text-white) !important;
}

.informes-container { width: 100%; max-width: none; margin: 0; background: #fff; border-radius: 8px; box-shadow: none; padding: 24px; box-sizing: border-box; }
.informes-container h2 { margin-bottom: 24px; }
.informes-container select, .informes-container input { margin-bottom: 12px; padding: 8px; border-radius: 4px; border: 1px solid #ccc; }
.informes-container button { padding: 10px 24px; background: var(--primary-green); color: var(--text-white); border: none; border-radius: 4px; font-size: 16px; margin-bottom: 16px; transition: background 0.2s; }
.informes-container button#print-btn { background: var(--print-blue); color: var(--text-white); }
.report-actions { display: flex; gap: 10px; flex-wrap: wrap; margin-bottom: 12px; }
.report-actions button.active { background: #2e7d32; }
.tabla-informe { width: 100%; border-collapse: collapse; margin-top: 24px; }
.tabla-informe th, .tabla-informe td { border: 1px solid #ccc; padding: 6px; text-align: left; }
.tabla-informe th { background: #eee; }
.total-row { background: #f4f4f4; font-weight: bold; }
.report-sheet { background: #fff; padding: 20px; border-radius: 12px; border: 1px solid #e0e0e0; }
.report-header { display: flex; justify-content: space-between; align-items: center; gap: 16px; margin-bottom: 20px; padding-bottom: 12px; border-bottom: 1px solid #e0e0e0; }
.report-brand { display: flex; align-items: center; gap: 12px; }
.report-brand img { width: 120px; height: auto; object-fit: contain; }
.report-brand h2 { margin: 0 0 4px; color: #1b5e20; }
.report-brand p { margin: 0; color: #5f6b68; font-size: 0.95rem; }
.report-title { font-size: 1.1rem; font-weight: bold; color: #2e7d32; text-align: right; }
.report-summary { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 10px; margin: 16px 0 20px; }
.report-card { background: #f8fff8; border: 1px solid #c8e6c9; border-radius: 10px; padding: 12px; display: flex; justify-content: space-between; align-items: center; gap: 10px; }
.report-card strong { color: #1b5e20; }
.tabla-informe-nueva td:nth-child(3) { white-space: nowrap; }

@media print {
    @page { margin-top: 1cm; margin-bottom: 1cm; }
    body { margin: 0; }
    .report-actions, button { display: none; }
    .informes-container { padding: 0; }
    .report-sheet { border: none; padding: 0; }
    .report-header { break-inside: avoid-page; }
}
```

### Archivo: `css/login.css`
```css
/* login.css */
.login-box { max-width: 350px; margin: 80px auto; background: #fff; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); padding: 32px; }
.login-box h2 { margin-bottom: 24px; }
.login-box input { width: 100%; margin-bottom: 16px; padding: 8px; border-radius: 4px; border: 1px solid #ccc; box-sizing: border-box; }
.login-box button {
    width: 100%;
    padding: 10px;
    background: var(--primary-green);
    color: var(--text-white);
    border: none;
    border-radius: 4px;
    font-size: 16px;
    transition: background 0.2s;
    font-weight: bold;
}
.login-box button:hover {
    background: var(--hover-green);
}
.login-box .error { color: #c00; margin-bottom: 12px; }
```

### Archivo: `css/main.css`
```css
/* main.css */
:root {
    --primary-green: #81C784;
    --hover-green: #4CAF50;
    --print-blue: #64B5F6;
    --text-white: #ffffff;
    --bg-gray: #f4f4f4;
}

button {
    color: var(--text-white) !important; /* Texto blanco para mejor contraste */
    font-family: inherit;
}

body {
    font-family: Arial, sans-serif;
    background: var(--bg-gray);
    margin: 0;
    padding: 0;
    overflow: auto;
}

#app {
    max-width: 900px;
    margin: 40px auto;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    padding: 32px;
}
```

### Archivo: `css/old/clientes.css`
```css
button:hover {
    background: #125ea2 !important;
}
/* ...existing code... */
button {
    color: #000 !important;
}
/* Restaurar estilos de estructura y tabla */
.clientes-container {
    max-width: 600px;
    margin: 60px auto;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    padding: 32px;
}
.clientes-container h2 {
    margin-bottom: 24px;
}
.tabla-clientes {
    width: 100%;
    border-collapse: collapse;
    margin-top: 24px;
}
.tabla-clientes th, .tabla-clientes td {
    border: 1px solid #ccc;
    padding: 6px;
    text-align: left;
}
.tabla-clientes th {
    background: #eee;
}
.inactivo {
    color: #c00;
}
/* clientes.css - Estilos específicos para clientes.html */
.clientes-container input, .clientes-container select {
    width: 100%;
    margin-bottom: 12px;
    padding: 8px;
    border-radius: 4px;
    border: 1px solid #ccc;
}
.clientes-container button {
    padding: 10px 24px;
    background: #A5D6A7;
    color: #1976d2;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    margin-bottom: 16px;
    transition: background 0.2s;
}
.clientes-container button#print-btn {
    background: #90caf9;
    color: #1565c0;
}

```

### Archivo: `css/old/dashboard.css`
```css
/* ...existing code... */
button {
    color: #000 !important;
}
/* dashboard.css - Estilos específicos para dashboard.html */
.dashboard-container { max-width: 600px; margin: 60px auto; background: #fff; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); padding: 32px; text-align: center; }
.dashboard-container h1 { margin-bottom: 32px; }
.dashboard-btn { display: block; width: 100%; margin: 16px 0; padding: 16px; font-size: 18px; border-radius: 6px; border: none; background: #A5D6A7; color: #1976d2; cursor: pointer; transition: background 0.2s; }
.dashboard-btn#print-btn { background: #90caf9; color: #1565c0; }
.dashboard-btn:hover { background: #125ea2; }

```

### Archivo: `css/old/informes.css`
```css
button:hover {
    background: #125ea2 !important;
}
/* ...existing code... */
button {
    color: #000 !important;
}
/* informes.css - Estilos específicos para informes.html */
.informes-container { max-width: 900px; margin: 60px auto; background: #fff; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); padding: 32px; }
.informes-container h2 { margin-bottom: 24px; }
.informes-container select, .informes-container input { margin-bottom: 12px; padding: 8px; border-radius: 4px; border: 1px solid #ccc; }
.informes-container button { padding: 10px 24px; background: #A5D6A7; color: #1976d2; border: none; border-radius: 4px; font-size: 16px; margin-bottom: 16px; transition: background 0.2s; }
.informes-container button#print-btn { background: #90caf9; color: #1565c0; }
.tabla-informe { width: 100%; border-collapse: collapse; margin-top: 24px; }
.tabla-informe th, .tabla-informe td { border: 1px solid #ccc; padding: 6px; text-align: left; }
.tabla-informe th { background: #eee; }
.total-row { background: #f4f4f4; font-weight: bold; }
@media print {
    @page { margin-top: 1cm; margin-bottom: 1cm; }
    body { margin: 0; }
}

```

### Archivo: `css/old/login.css`
```css
/* login.css - Estilos para el formulario de login */
.login-box { max-width: 350px; margin: 80px auto; background: #fff; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); padding: 32px; }
.login-box h2 { margin-bottom: 24px; }
.login-box input { width: 100%; margin-bottom: 16px; padding: 8px; border-radius: 4px; border: 1px solid #ccc; }
.login-box button {
    width: 100%;
    padding: 10px;
    background: #A5D6A7;
    color: #000;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    transition: background 0.2s;
}
.login-box button:hover {
    background: #125ea2;
    color: #000;
}
.login-box .error { color: #c00; margin-bottom: 12px; }

```

### Archivo: `css/old/main.css`
```css
button {
    color: #000;
}
/* Estilos base para controlHorasClientes */
body {
    font-family: Arial, sans-serif;
    background: #f4f4f4;
    margin: 0;
    padding: 0;
}
#app {
    max-width: 900px;
    margin: 40px auto;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    padding: 32px;
}

```

### Archivo: `css/old/registro_horas.css`
```css
button:hover {
    background: #125ea2 !important;
}
/* ...existing code... */
button {
    color: #000 !important;
}
/* registro_horas.css - Estilos específicos para registro_horas.html */
.registro-horas-container { max-width: 600px; margin: 60px auto; background: #fff; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); padding: 32px; }
.registro-horas-container h2 { margin-bottom: 24px; }
.registro-horas-container select, .registro-horas-container input { width: 100%; margin-bottom: 16px; padding: 8px; border-radius: 4px; border: 1px solid #ccc; }
.registro-horas-container button { width: 100%; padding: 10px; background: #A5D6A7; color: #1976d2; border: none; border-radius: 4px; font-size: 16px; transition: background 0.2s; }
.registro-horas-container button#print-btn { background: #90caf9; color: #1565c0; }
.registro-horas-container .msg { margin-bottom: 12px; }
.tramos-list { margin-top: 16px; }
.tramo-item { background: #f4f4f4; border-radius: 4px; padding: 8px; margin-bottom: 8px; }

```

### Archivo: `css/registro_horas.css`
```css
/* registro_horas.css */
button:hover {
    background: var(--hover-green) !important;
}

button {
    color: var(--text-white) !important;
}

.registro-horas-container { width: 100%; max-width: none; margin: 0; background: #fff; border-radius: 8px; box-shadow: none; padding: 24px; box-sizing: border-box; }
.registro-horas-container h2 { margin-bottom: 24px; }
.registro-horas-container select, .registro-horas-container input { width: 100%; margin-bottom: 16px; padding: 8px; border-radius: 4px; border: 1px solid #ccc; box-sizing: border-box; }
.registro-horas-container button { width: 100%; padding: 10px; background: var(--primary-green); color: var(--text-white); border: none; border-radius: 4px; font-size: 16px; transition: background 0.2s; }
.registro-horas-container button#print-btn { background: var(--print-blue); color: var(--text-white); }
.registro-horas-container .msg { margin-bottom: 12px; }
.tramos-list { margin-top: 16px; }
.tramo-item { background: #f4f4f4; border-radius: 4px; padding: 8px; margin-bottom: 8px; border-left: 4px solid var(--primary-green); }
```

### Archivo: `data/horasClientes copy.json`
```json
[
  {
    "cliente": "Pepe",
    "fecha": "2026-01-02",
    "tramos": [
      {
        "inicio": "",
        "fin": "",
        "horas": "3"
      },
      {
        "inicio": "",
        "fin": "",
        "horas": "4"
      },
      {
        "inicio": "09:00",
        "fin": "14:00",
        "horas": "5.00"
      }
    ]
  },
  {
    "cliente": "Gusano Manzana",
    "fecha": "2026-01-03",
    "tramos": [
      {
        "inicio": "10:00",
        "fin": "14:30",
        "horas": "4.30"
      },
      {
        "inicio": "17:00",
        "fin": "19:00",
        "horas": "2.00"
      }
    ]
  },
  {
    "cliente": "Soluciones Creativas",
    "fecha": "2026-01-05",
    "tramos": [
      {
        "inicio": "08:00",
        "fin": "12:00",
        "horas": "4.00"
      },
      {
        "inicio": "13:00",
        "fin": "17:00",
        "horas": "4.00"
      }
    ]
  },
  {
    "cliente": "Distribuidora El Faro",
    "fecha": "2026-01-10",
    "tramos": [
      {
        "inicio": "09:00",
        "fin": "13:00",
        "horas": "4.00"
      },
      {
        "inicio": "15:00",
        "fin": "18:00",
        "horas": "3.00"
      }
    ]
  },
  {
    "cliente": "Consultora MBTI",
    "fecha": "2026-01-15",
    "tramos": [
      {
        "inicio": "08:30",
        "fin": "12:30",
        "horas": "4.00"
      },
      {
        "inicio": "14:00",
        "fin": "19:00",
        "horas": "5.00"
      }
    ]
  },
  {
    "cliente": "Cliente Prueba",
    "fecha": "2026-01-20",
    "tramos": [
      {
        "inicio": "10:00",
        "fin": "13:00",
        "horas": "3.00"
      },
      {
        "inicio": "18:00",
        "fin": "21:00",
        "horas": "3.00"
      }
    ]
  },
  {
    "cliente": "Soluciones Creativas",
    "fecha": "2026-01-12",
    "tramos": [
      {
        "inicio": "08:00",
        "fin": "10:00",
        "horas": "2.00"
      },
      {
        "inicio": "10:30",
        "fin": "12:00",
        "horas": "1.50"
      },
      {
        "inicio": "13:00",
        "fin": "15:00",
        "horas": "2.00"
      },
      {
        "inicio": "",
        "fin": "",
        "horas": "2.50"
      }
    ]
  },
  {
    "cliente": "Distribuidora El Faro",
    "fecha": "2026-01-18",
    "tramos": [
      {
        "inicio": "09:00",
        "fin": "11:00",
        "horas": "2.00"
      },
      {
        "inicio": "11:30",
        "fin": "13:00",
        "horas": "1.50"
      },
      {
        "inicio": "14:00",
        "fin": "16:00",
        "horas": "2.00"
      }
    ]
  },
  {
    "cliente": "Consultora MBTI",
    "fecha": "2026-01-22",
    "tramos": [
      {
        "inicio": "08:30",
        "fin": "10:30",
        "horas": "2.00"
      },
      {
        "inicio": "11:00",
        "fin": "13:00",
        "horas": "2.00"
      },
      {
        "inicio": "",
        "fin": "",
        "horas": "3.00"
      }
    ]
  },
  {
    "cliente": "Pepe",
    "fecha": "2026-01-25",
    "tramos": [
      {
        "inicio": "08:00",
        "fin": "12:00",
        "horas": "4.00"
      },
      {
        "inicio": "",
        "fin": "",
        "horas": "2.00"
      }
    ]
  },
  {
    "cliente": "Cliente Prueba",
    "fecha": "2026-01-28",
    "tramos": [
      {
        "inicio": "08:00",
        "fin": "10:00",
        "horas": "2.00"
      },
      {
        "inicio": "10:30",
        "fin": "12:00",
        "horas": "1.50"
      },
      {
        "inicio": "13:00",
        "fin": "15:00",
        "horas": "2.00"
      }
    ]
  },
  {
    "cliente": "Gusano Manzana",
    "fecha": "2026-01-30",
    "tramos": [
      {
        "inicio": "08:00",
        "fin": "11:00",
        "horas": "3.00"
      },
      {
        "inicio": "12:00",
        "fin": "14:00",
        "horas": "2.00"
      },
      {
        "inicio": "15:00",
        "fin": "17:00",
        "horas": "2.00"
      }
    ]
  },
  {
    "cliente": "Consultora MBTI",
    "fecha": "2026-01-02",
    "tramos": [
      {
        "inicio": "",
        "fin": "",
        "horas": "2"
      }
    ]
  },
  {
    "cliente": "Montaña",
    "fecha": "2026-01-20",
    "tramos": [
      {
        "inicio": "14:00",
        "fin": "16:00",
        "horas": "2:00"
      }
    ]
  },
  {
    "cliente": "Distribuidora El Faro",
    "fecha": "2026-01-15",
    "tramos": [
      {
        "inicio": "",
        "fin": "",
        "horas": "7"
      }
    ]
  },
  {
    "cliente": "Consultora MBTI",
    "fecha": "2026-01-23",
    "tramos": [
      {
        "inicio": "09:00",
        "fin": "14:00",
        "horas": "5:00"
      }
    ]
  },
  {
    "cliente": "Consultora MBTI",
    "fecha": "2026-01-05",
    "tramos": [
      {
        "inicio": "21:30",
        "fin": "12:30",
        "horas": "15:00"
      }
    ]
  },
  {
    "cliente": "Soluciones Creativas",
    "fecha": "2026-01-07",
    "tramos": [
      {
        "inicio": "19:00",
        "fin": "13:00",
        "horas": "18:00"
      }
    ]
  },
  {
    "cliente": "Montaña",
    "fecha": "2026-01-29",
    "tramos": [
      {
        "inicio": "08:00",
        "fin": "11:00",
        "horas": "3:00"
      }
    ]
  },
  {
    "cliente": "Soluciones Creativas",
    "fecha": "2026-07-01",
    "tramos": [
      {
        "inicio": "",
        "fin": "",
        "horas": "7"
      }
    ]
  },
  {
    "cliente": "Consultora MBTI",
    "fecha": "2026-07-01",
    "tramos": [
      {
        "inicio": "08:00",
        "fin": "12:00",
        "horas": "4:00"
      }
    ]
  }
]
```

### Archivo: `data/horasClientes.json`
```json
[
 
]
```

### Archivo: `data/listaClientes copy.json`
```json
[
  {
    "nombre": "Pepe",
    "rubro": "Despachante de Aduana",
    "activo": false
  },
  {
    "nombre": "Gusano Manzana",
    "rubro": "Agente"
  },
  {
    "nombre": "Montaña",
    "rubro": "Exportador",
    "activo": true
  },
  {
    "nombre": "Soluciones Creativas",
    "rubro": "Comercio",
    "activo": true
  },
  {
    "nombre": "Distribuidora El Faro",
    "rubro": "Importador",
    "activo": true
  },
  {
    "nombre": "Consultora MBTI",
    "rubro": "Línea marítima",
    "activo": true
  },
  {
    "nombre": "Cliente Prueba",
    "rubro": "Aerolínea",
    "activo": false
  },
  {
    "nombre": "nuevo cliente",
    "rubro": "Exportador",
    "activo": true
  },
  {
    "nombre": "TREN",
    "rubro": "Despachante de Aduana",
    "activo": true
  }
]
```

### Archivo: `data/listaClientes.json`
```json
[

]
```

### Archivo: `data/usuarios.json`
```json
[{
    "email": "fabior1981@gmail.com",
    "nombre": "Fabio Roberto",
    "rol": "admin",
    "contrasena": "8899b499283b53d3377ff57984b0fc503a20747b10daed146825bed10412c18d",
    "tipdocu": "CI",
    "documento": "30856946",
    "telefono": "",
    "activo": true
  },
  {
    "email": "shalonmorales@gmail.com",
    "nombre": "Shalon Morales",
    "rol": "admin",
    "contrasena": "052749e9cbfefbcd309e6a7d43e06a0c59371a6e113a5292b6454ea4594e62e1",
    "tipdocu": "CI",
    "documento": "30340187",
    "telefono": "",
    "activo": true
  }
]

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/controlHorasClientes/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `hash_sha256.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Hash SHA-256 Online</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f4f4f4; padding: 40px; }
    .container { background: #fff; max-width: 400px; margin: auto; padding: 30px 30px 20px 30px; border-radius: 8px; box-shadow: 0 2px 8px #0001; }
    label { font-weight: bold; display: block; margin-bottom: 8px; }
    input, textarea, button { width: 100%; margin-bottom: 15px; padding: 8px; border-radius: 4px; border: 1px solid #ccc; }
    textarea { resize: none; }
    .result { font-family: monospace; background: #f0f0f0; padding: 10px; border-radius: 4px; }
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
  <script>
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
  </script>
</body>
</html>

```

### Archivo: `html/clientes.html`
```html
<!-- clientes.html - Gestión de clientes: alta, listado y baja lógica -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Clientes</title>
    <link rel="stylesheet" href="../css/main.css">
    <link rel="stylesheet" href="../css/clientes.css">
</head>
<body>
    <div class="clientes-container">
        <h2>Clientes</h2>
        <form id="altaClienteForm">
            <input type="text" id="nombreCliente" placeholder="Nombre del cliente" required autofocus>
            <select id="rubroCliente" required>
                <option value="">Seleccione un rubro</option>
            </select>
            <input type="number" id="valorHoraCliente" placeholder="Valor Hora" min="0" step="0.01">
            <div style="display:flex; flex-direction:column; gap:6px; margin-top:8px; font-size:0.95rem; align-items:flex-start;">
                <select id="activoCliente">
                    <option value="true">Activo</option>
                    <option value="false">Inactivo</option>
                </select>
            </div>
            <div class="msg" id="msgAlta"></div>
            <button type="submit">Guardar cliente</button>
        </form>
        <table class="tabla-clientes">
            <thead>
                <tr><th>Nombre</th><th>Rubro</th><th>Valor Hora</th><th>Estado</th><th>Acciones</th></tr>
            </thead>
            <tbody id="tbodyClientes"></tbody>
        </table>
    </div>
    <script src="../js/config.js"></script>
    <script src="../js/clientes.js"></script>
<script>
// Verificar expiración de sesión
const usuarioStr = localStorage.getItem('usuarioLogueado');
let usuario = null;
if(usuarioStr) {
    try {
        usuario = JSON.parse(usuarioStr);
    } catch {}
}
if(!usuario || !usuario.expiracion || Date.now() > usuario.expiracion) {
    localStorage.removeItem('usuarioLogueado');
    window.location.href = 'login.html';
}
</script>
</body>
<footer style="text-align:center; color:#888; font-size:13px; margin-top:32px;">Realizado por TechFaRo © 2026</footer>
</html>

```

### Archivo: `html/dashboard.html`
```html
<!-- dashboard.html - Panel principal para controlHorasClientes -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard - Control Horas Clientes</title>
    <link rel="stylesheet" href="../css/main.css">
    <link rel="stylesheet" href="../css/dashboard.css">
</head>
<body>
    <div class="dashboard-shell">
        <aside class="dashboard-sidebar">
            <div class="sidebar-brand">
                <img src="../imagenes/logo_shalon.png" alt="Logo Shalon Morales">
            </div>
            <nav class="sidebar-nav" aria-label="Menú principal">
                <a class="dashboard-btn sidebar-link active" href="#" data-page="home">Inicio</a>
                <a class="dashboard-btn sidebar-link" href="#" data-page="clientes.html">Clientes</a>
                <a class="dashboard-btn sidebar-link" href="#" data-page="registro_horas.html">Registrar horas</a>
                <a class="dashboard-btn sidebar-link" href="#" data-page="informes2.html">Informes</a>
            </nav>
            <button class="dashboard-btn sidebar-link logout-btn" id="logoutBtn">Cerrar sesión</button>
        </aside>

        <main class="dashboard-content">
            <header class="dashboard-header">
                <h1>Panel Principal</h1>
                <p>Gestiona clientes, horas e informes desde un solo lugar.</p>
            </header>

            <section id="dashboardHome" class="dashboard-home">
                <div class="home-summary-card">
                    <div class="home-summary-header">
                        <div>
                            <h2>Resumen de horas por cliente</h2>
                            <p>Visualiza cómo se distribuyen las horas en los últimos <span id="rangeLabel">6 meses</span>.</p>
                        </div>
                        <label class="range-select" for="rangeSelect">
                            <span>Ver últimos</span>
                            <select id="rangeSelect"></select>
                        </label>
                    </div>

                    <div class="home-summary-content">
                        <div class="chart-wrapper">
                            <div id="hoursChart" class="hours-chart" aria-label="Gráfico de horas por cliente"></div>
                            <div class="chart-center">
                                <strong id="chartTotalHours">0 h</strong>
                                <span>Total</span>
                            </div>
                        </div>
                        <ul id="legendList" class="legend-list"></ul>
                    </div>
                </div>
            </section>

            <iframe id="contentFrame" class="dashboard-frame" title="Contenido del panel" src=""></iframe>
        </main>
    </div>

    <footer class="dashboard-footer">Realizado por TechFaRo © 2026</footer>

    <script src="../js/config.js"></script>
    <script>
    // Mostrar nombre de usuario si está logueado y verificar expiración de sesión
    const usuarioStr = localStorage.getItem('usuarioLogueado');
    let usuario = null;
    if(usuarioStr) {
        try {
            usuario = JSON.parse(usuarioStr);
        } catch {}
    }
    if(!usuario || !usuario.expiracion || Date.now() > usuario.expiracion) {
        localStorage.removeItem('usuarioLogueado');
        window.location.href = 'login.html';
    }

    const homeSection = document.getElementById('dashboardHome');
    const contentFrame = document.getElementById('contentFrame');
    const menuLinks = document.querySelectorAll('.sidebar-link[data-page], .dashboard-card-link[data-page]');
    const rangeSelect = document.getElementById('rangeSelect');
    const rangeLabel = document.getElementById('rangeLabel');
    const hoursChart = document.getElementById('hoursChart');
    const chartTotalHours = document.getElementById('chartTotalHours');
    const legendList = document.getElementById('legendList');
    const dashboardConfig = (window.config && window.config.dashboardHome) || {};
    const maxClients = Number(dashboardConfig.maxClients || 15);
    const monthsOptions = Array.isArray(dashboardConfig.monthsOptions) && dashboardConfig.monthsOptions.length
        ? dashboardConfig.monthsOptions
        : [3, 6, 9, 12];
    const defaultMonths = Number(dashboardConfig.defaultMonths || monthsOptions[0] || 6);

    monthsOptions.forEach(months => {
        const option = document.createElement('option');
        option.value = months;
        option.textContent = `${months} meses`;
        if (months === defaultMonths) {
            option.selected = true;
        }
        rangeSelect.appendChild(option);
    });

    function parseHoursValue(value) {
        if (value === null || value === undefined || value === '') return 0;
        const text = String(value).trim();
        if (!text) return 0;
        if (text.includes(':')) {
            const [hours, minutes] = text.split(':').map(Number);
            return hours + (minutes || 0) / 60;
        }
        const normalized = text.replace(',', '.');
        const parts = normalized.split('.');
        const hours = parseInt(parts[0] || '0', 10);
        const decimals = parts[1] || '0';
        if (decimals && !isNaN(Number(decimals))) {
            const decimalValue = Number(decimals);
            if (decimalValue < 60) {
                return hours + decimalValue / 60;
            }
            return hours + decimalValue / 100;
        }
        return hours;
    }

    function getMonthlyTotals(records, months, clientesDisponibles = []) {
        const dates = records.map(r => r.fecha).filter(Boolean);
        const latestDate = dates.length ? dates.reduce((latest, current) => current > latest ? current : latest, dates[0]) : null;
        if (!latestDate) {
            const nombresClientes = (clientesDisponibles || [])
                .map(item => item && (item.nombre || item))
                .filter(Boolean);
            return nombresClientes.map(nombre => [nombre, 0]);
        }

        const latest = new Date(`${latestDate}T00:00:00`);
        const start = new Date(latest.getFullYear(), latest.getMonth() - months + 1, 1);
        const totals = {};

        records.forEach(record => {
            if (!record.fecha) return;
            const recordDate = new Date(`${record.fecha}T00:00:00`);
            if (recordDate < start || recordDate > latest) return;
            const client = record.cliente || 'Sin cliente';
            const total = (record.tramos || []).reduce((sum, tramo) => sum + parseHoursValue(tramo && tramo.horas), 0);
            totals[client] = (totals[client] || 0) + total;
        });

        const clientes = (clientesDisponibles || [])
            .map(item => item && (item.nombre || item))
            .filter(Boolean);
        const nombresClientes = Array.from(new Set([...clientes, ...Object.keys(totals)]));

        return nombresClientes
            .map(nombre => [nombre, totals[nombre] || 0])
            .sort((a, b) => b[1] - a[1] || a[0].localeCompare(b[0]));
    }

    async function renderHomeSummary(months) {
        try {
            const [responseHours, responseClients] = await Promise.all([
                fetch('../data/horasClientes.json'),
                fetch('../data/listaClientes.json')
            ]);
            const records = await responseHours.json();
            const clientes = await responseClients.json();
            const entries = getMonthlyTotals(records, months, clientes);
            rangeLabel.textContent = `${months} meses`;

            if (!entries.length) {
                hoursChart.style.background = 'linear-gradient(135deg, #e8f5e9 0%, #f4f4f4 100%)';
                chartTotalHours.textContent = '0 h';
                legendList.innerHTML = '<li class="legend-empty">No hay registros para este periodo.</li>';
                return;
            }

            const total = entries.reduce((sum, [, value]) => sum + value, 0);
            chartTotalHours.textContent = `${total.toFixed(1)} h`;

            const colors = ['#2e7d32', '#66bb6a', '#26a69a', '#5c6bc0', '#ef5350', '#ffb300', '#8d6e63', '#ab47bc', '#26c6da', '#ffa726'];
            let currentAngle = 0;
            const topEntries = maxClients > 0 ? entries.slice(0, maxClients) : entries;
            const segments = topEntries.map(([client, value], index) => {
                const percentage = total > 0 ? (value / total) * 100 : 0;
                const start = currentAngle;
                currentAngle += percentage;
                return `${colors[index % colors.length]} ${start.toFixed(2)}% ${currentAngle.toFixed(2)}%`;
            });
            hoursChart.style.background = total > 0 ? `conic-gradient(${segments.join(', ')})` : 'linear-gradient(135deg, #e8f5e9 0%, #f4f4f4 100%)';

            legendList.innerHTML = topEntries.map(([client, value], index) => `
                <li class="legend-item">
                    <span class="legend-dot" style="background:${colors[index % colors.length]};"></span>
                    <div>
                        <strong>${client}</strong>
                        <small>${value.toFixed(1)} h</small>
                    </div>
                </li>
            `).join('');
        } catch (error) {
            hoursChart.style.background = 'linear-gradient(135deg, #e8f5e9 0%, #f4f4f4 100%)';
            chartTotalHours.textContent = '0 h';
            legendList.innerHTML = '<li class="legend-empty">No se pudo cargar el resumen.</li>';
        }
    }

    rangeSelect.addEventListener('change', function() {
        renderHomeSummary(Number(this.value));
    });

    function showPage(page) {
        document.querySelectorAll('.sidebar-link[data-page]').forEach(link => link.classList.remove('active'));
        const activeLink = document.querySelector(`.sidebar-link[data-page="${page}"]`);
        if (activeLink) {
            activeLink.classList.add('active');
        }

        if (page === 'home') {
            homeSection.style.display = 'grid';
            contentFrame.style.display = 'none';
            contentFrame.src = '';
            return;
        }

        homeSection.style.display = 'none';
        contentFrame.style.display = 'block';
        contentFrame.src = page === 'informes2.html' ? 'informes2.html' : page;
    }

    menuLinks.forEach(link => {
        link.addEventListener('click', function(event) {
            event.preventDefault();
            showPage(this.getAttribute('data-page'));
        });
    });

    renderHomeSummary(Number(rangeSelect.value || defaultMonths));

    document.getElementById('logoutBtn').onclick = function() {
        localStorage.removeItem('usuarioLogueado');
        window.location.href = 'login.html';
    };
    </script>
</body>
</html>

```

### Archivo: `html/informes.html`
```html
<!-- informes.html - Informes de horas por cliente y periodo -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Informes de Horas</title>
    <link rel="stylesheet" href="../css/main.css">
    <link rel="stylesheet" href="../css/informes.css">
</head>
<body>
    <div class="informes-container">
        <h2>Informes de Horas</h2>
        <form id="formInforme">
            <label>Cliente:
                <select id="clienteInforme">
                    <option value="todos">Todos</option>
                </select>
            </label>
            <label>Fecha inicio:
                <input type="date" id="fechaInicio" required>
            </label>
            <label>Fecha fin:
                <input type="date" id="fechaFin" required>
            </label>
            <button type="submit">Generar informe</button>
        </form>
        <div class="report-actions">
            <button type="button" id="print-btn" onclick="window.print()" style="margin-bottom:8px;">Imprimir informe</button>
        </div>
        <div id="resultadoInforme"></div>
    </div>
    <script src="../js/informes.js"></script>
<script>
// Verificar expiración de sesión
const usuarioStr = localStorage.getItem('usuarioLogueado');
let usuario = null;
if(usuarioStr) {
    try {
        usuario = JSON.parse(usuarioStr);
    } catch {}
}
if(!usuario || !usuario.expiracion || Date.now() > usuario.expiracion) {
    localStorage.removeItem('usuarioLogueado');
    window.location.href = 'login.html';
}
</script>
</body>
<footer style="text-align:center; color:#888; font-size:13px; margin-top:32px;">Realizado por TechFaRo © 2026</footer>
</html>

```

### Archivo: `html/informes2.html`
```html
<!-- informes2.html - Versión alternativa de informes para impresión -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Informes 2</title>
    <link rel="stylesheet" href="../css/main.css">
    <link rel="stylesheet" href="../css/informes.css">
</head>
<body>
    <div class="informes-container">
        <h2>Informes</h2>
        <form id="formInforme2">
            <label>Cliente:
                <select id="clienteInforme2">
                    <option value="todos">Todos</option>
                </select>
            </label>
            <label>Fecha inicio:
                <input type="date" id="fechaInicio2" required>
            </label>
            <label>Fecha fin:
                <input type="date" id="fechaFin2" required>
            </label>
            <button type="button" id="btnGenerarInforme2">Descargar PDF</button>
        </form>
        <div id="resultadoInforme2"></div>
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <script src="../js/informes.js"></script>
<script>
const usuarioStr = localStorage.getItem('usuarioLogueado');
let usuario = null;
if(usuarioStr) {
    try {
        usuario = JSON.parse(usuarioStr);
    } catch {}
}
if(!usuario || !usuario.expiracion || Date.now() > usuario.expiracion) {
    localStorage.removeItem('usuarioLogueado');
    window.location.href = 'login.html';
}
</script>
</body>
</html>

```

### Archivo: `html/login.html`
```html
<!-- login.html - Formulario de login para controlHorasClientes -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login - Control Horas Clientes</title>
    <link rel="stylesheet" href="../css/main.css">
    <link rel="stylesheet" href="../css/login.css">
</head>
<body>
    <div class="login-box">
        <img src="../imagenes/logo_shalon.png" alt="Logo Shalon Morales" style="display:block; margin:0 auto 24px auto; max-width:180px;">
        <h2>Iniciar Sesión</h2>
        <form id="loginForm">
                    <input type="text" id="documento" placeholder="Documento" required autofocus>
            <input type="password" id="contrasena" placeholder="Contraseña" required>
            <div class="error" id="loginError" style="display:none;"></div>
            <button type="submit">Ingresar</button>
        </form>
    </div>
    <script src="../js/login.js"></script>
    <script>
    document.getElementById('loginForm').onsubmit = async function(e) {
        e.preventDefault();
        const documento = document.getElementById('documento').value;
        const pass = document.getElementById('contrasena').value;
        const usuario = await validarLogin(documento, pass);
        if(usuario) {
            // Guardar usuario y expiración en localStorage para mantener sesión
            const sessionDuration = window.config?.sessionDuration || (8 * 60 * 60 * 1000);
            const expiracion = Date.now() + sessionDuration;
            localStorage.setItem('usuarioLogueado', JSON.stringify({
                documento: usuario.documento,
                nombre: usuario.nombre,
                rol: usuario.rol,
                expiracion: expiracion
            }));
            window.location.href = 'dashboard.html';
        } else {
            document.getElementById('loginError').textContent = 'Usuario o contraseña incorrectos';
            document.getElementById('loginError').style.display = 'block';
        }
    }
    </script>
</body>
<footer style="text-align:center; color:#888; font-size:13px; margin-top:32px;">Realizado por TechFaRo © 2026</footer>
</html>

```

### Archivo: `html/registro_horas.html`
```html
<!-- registro_horas.html - Registro de horas por cliente y día -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registrar Horas</title>
    <link rel="stylesheet" href="../css/main.css">
    <link rel="stylesheet" href="../css/registro_horas.css">
</head>
<body>
    <div class="registro-horas-container">
        <h2>Registrar Horas</h2>
        <form id="registroHorasForm">
            <div style="display:flex; gap:8px; align-items:center; margin-bottom:12px;">
                <select id="clienteSelect" required style="flex:2"></select>
                <input type="date" id="fecha" required style="flex:1">
            </div>
            <div style="display:flex; gap:8px; align-items:center; margin-bottom:12px;">
                <input type="time" id="nuevoInicio" placeholder="Inicio" style="width:100px;">
                <input type="time" id="nuevoFin" placeholder="Fin" style="width:100px;">
                <input type="number" min="0" step="0.1" id="nuevoHoras" placeholder="Horas" style="width:80px;">
                <button type="button" id="addTramoBtn" style="width:100px; color:#000;">Insertar</button>
            </div>
            <div class="msg" id="msgRegistro"></div>
            <table id="tablaTramos" style="width:100%; border-collapse:collapse; margin-bottom:16px;">
                <thead>
                    <tr style="background:#eee;">
                        <th style="padding:6px; border:1px solid #ccc;">Inicio</th>
                        <th style="padding:6px; border:1px solid #ccc;">Fin</th>
                        <th style="padding:6px; border:1px solid #ccc;">Horas</th>
                        <th style="padding:6px; border:1px solid #ccc;">Acciones</th>
                    </tr>
                </thead>
                <tbody id="tbodyTramos"></tbody>
            </table>
            <button type="submit" style="color:#000;">Guardar registro</button>
        </form>
    </div>
    <script src="../js/registro_horas.js"></script>
<script>
// Verificar expiración de sesión
const usuarioStr = localStorage.getItem('usuarioLogueado');
let usuario = null;
if(usuarioStr) {
    try {
        usuario = JSON.parse(usuarioStr);
    } catch {}
}
if(!usuario || !usuario.expiracion || Date.now() > usuario.expiracion) {
    localStorage.removeItem('usuarioLogueado');
    window.location.href = 'login.html';
}
</script>
</body>
<footer style="text-align:center; color:#888; font-size:13px; margin-top:32px;">Realizado por TechFaRo © 2026</footer>
</html>

```

### Archivo: `imagenes/logo_Shalon.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Control Horas Clientes</title>
    <link rel="stylesheet" href="css/main.css">
</head>
<body>
    <div id="app">
        <button id="logoutBtn" style="float:right; margin: 10px 0 0 0; display:none;">Cerrar sesión</button>
        <!-- Aquí irá la aplicación -->
    </div>
    <script src="js/config.js"></script>
    <script>
    // Si no hay sesión, redirigir a login
    if(!localStorage.getItem('usuarioLogueado')) {
        window.location.href = 'html/login.html';
    } else {
        window.location.href = 'html/dashboard.html';
    }
    </script>
    <!-- Agregar aquí los demás scripts principales -->
</body>
</html>

```

### Archivo: `js/clientes.js`
```javascript
// clientes.js - Gestión de clientes: alta, listado y baja lógica

let clientes = [];

// Cargar clientes desde el archivo JSON y renderizar la tabla
async function cargarClientes() {
    try {
        const resp = await fetch('../data/listaClientes.json');
        clientes = await resp.json();
    } catch { clientes = []; }
    renderTablaClientes();
}

function renderTablaClientes() {
    const tbody = document.getElementById('tbodyClientes');
    tbody.innerHTML = '';
    clientes.forEach((c, idx) => {
        const tr = document.createElement('tr');
        const valorHora = c.valorHora == null || c.valorHora === '' ? '-' : Number(c.valorHora).toFixed(2);
        tr.innerHTML = `
            <td>${c.nombre}</td>
            <td>${c.rubro || ''}</td>
            <td>${valorHora}</td>
            <td class='${c.activo === false ? "inactivo" : ""}'>${c.activo === false ? 'Inactivo' : 'Activo'}</td>
            <td>
                <button type='button' onclick='editarCliente(${idx})'>Editar</button>
            </td>
        `;
        tbody.appendChild(tr);
    });
}

async function guardarClientes() {
    await fetch('/.netlify/functions/update-clientes', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ data: clientes })
    });
}
// Editar cliente: carga los datos en el formulario para modificar
window.editarCliente = function(idx) {
    const cliente = clientes[idx];
    document.getElementById('nombreCliente').value = cliente.nombre || '';
    document.getElementById('rubroCliente').value = cliente.rubro || '';
    document.getElementById('valorHoraCliente').value = cliente.valorHora == null || cliente.valorHora === '' ? '' : cliente.valorHora;
    document.getElementById('activoCliente').value = cliente.activo === false ? 'false' : 'true';
    document.getElementById('altaClienteForm').setAttribute('data-edit', idx);
    document.getElementById('msgAlta').textContent = 'Editando cliente. Modifica y guarda para actualizar.';
    document.getElementById('msgAlta').style.color = '#1976d2';
}

window.toggleActivo = async function(idx) {
    clientes[idx].activo = clientes[idx].activo === false ? true : false;
    await guardarClientes();
    renderTablaClientes();
}

document.getElementById('altaClienteForm').onsubmit = async function(e) {
    e.preventDefault();
    const nombre = document.getElementById('nombreCliente').value.trim();
    const rubro = document.getElementById('rubroCliente').value;
    const valorHoraRaw = document.getElementById('valorHoraCliente').value.trim();
    const activo = document.getElementById('activoCliente').value === 'true';
    const msg = document.getElementById('msgAlta');
    const editIdx = this.getAttribute('data-edit');
    const valorHora = valorHoraRaw === '' ? 0 : Number(valorHoraRaw);
    if(!nombre) {
        msg.textContent = 'El nombre es obligatorio.';
        msg.style.color = '#c00';
        return;
    }
    if(!rubro) {
        msg.textContent = 'Debe seleccionar un rubro.';
        msg.style.color = '#c00';
        return;
    }
    if (Number.isNaN(valorHora)) {
        msg.textContent = 'El valor hora debe ser numérico.';
        msg.style.color = '#c00';
        return;
    }
    if(editIdx !== null && editIdx !== '') {
        // Editar cliente existente
        const editIndex = Number(editIdx);
        if(clientes.some((c, i) => i !== editIndex && c.nombre.toLowerCase() === nombre.toLowerCase())) {
            msg.textContent = 'Ya existe un cliente con ese nombre.';
            msg.style.color = '#c00';
            return;
        }
        clientes[editIndex].nombre = nombre;
        clientes[editIndex].rubro = rubro;
        clientes[editIndex].valorHora = valorHora;
        clientes[editIndex].activo = activo;
        await guardarClientes();
        msg.textContent = 'Cliente actualizado correctamente.';
        msg.style.color = '#080';
        this.removeAttribute('data-edit');
    } else {
        // Alta nuevo cliente
        if(clientes.some(c => c.nombre.toLowerCase() === nombre.toLowerCase())) {
            msg.textContent = 'Ya existe un cliente con ese nombre.';
            msg.style.color = '#c00';
            return;
        }
        clientes.push({ nombre, rubro, valorHora, activo });
        await guardarClientes();
        msg.textContent = 'Cliente guardado correctamente.';
        msg.style.color = '#080';
    }
    document.getElementById('altaClienteForm').reset();
    document.getElementById('activoCliente').value = 'true';
    renderTablaClientes();

}

// Llenar el combo de rubros desde config.js (funciona si el DOM ya está cargado o no)
function populateRubros() {
    if (window.config && Array.isArray(window.config.rubros) && window.config.rubros.length > 0) {
        const rubroSelect = document.getElementById('rubroCliente');
        if (!rubroSelect) {
            return;
        }
        // Evitar duplicados si se recarga el DOM
        rubroSelect.innerHTML = '<option value="">Seleccione un rubro</option>';
        window.config.rubros.forEach(rubro => {
            const opt = document.createElement('option');
            opt.value = rubro;
            opt.textContent = rubro;
            rubroSelect.appendChild(opt);
        });
    } else {
        // No se encontró window.config.rubros o está vacío
    }
}

// Exponer para depuración manual
window.populateRubros = populateRubros;

if (document.readyState === 'loading') {
    window.addEventListener('DOMContentLoaded', populateRubros);
} else {
    // Si el DOM ya está listo, intentarlo de inmediato
    populateRubros();
}

// Reintentar un segundo después si sigue vacío
setTimeout(() => {
    const select = document.getElementById('rubroCliente');
    if (select && select.options.length <= 1) {
        populateRubros();
    }
}, 1000);


async function guardarClientes() {
    await fetch('/.netlify/functions/update-clientes', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ data: clientes })
    });
}

cargarClientes();

```

### Archivo: `js/config.js`
```javascript
// Archivo de configuración para controlHorasClientes
// Modificar los valores según corresponda

window.config = {
    rubros: [
        "Importador",
        "Exportador",
        "Despachante de Aduana",
        "Comercio",
        "Agente",
        "Línea marítima",
        "Aerolínea"
    ],
    googleCalendarAccount: "", // Cuenta de Gmail para futura integración
    dashboardHome: {
        maxClients: 0,
        monthsOptions: [1,3, 6, 9, 12],
        defaultMonths: 6
    },
    // Otros parámetros de configuración pueden agregarse aquí
    sessionDuration: 8 * 60 * 60 * 1000 // Duración de sesión en milisegundos (8 horas)
};

// Debug: confirmar carga de config
//console.log('config.js cargado, window.config:', window.config);

```

### Archivo: `js/informes.js`
```javascript
// informes.js - Lógica para mostrar informes de horas por cliente y periodo

let vistaActual = 'classic';

function getReportConfig() {
    if (document.getElementById('formInforme2')) {
        return {
            formId: 'formInforme2',
            selectId: 'clienteInforme2',
            startId: 'fechaInicio2',
            endId: 'fechaFin2',
            resultId: 'resultadoInforme2',
            buttonId: 'btnGenerarInforme2',
            mode: 'nuevo'
        };
    }

    return {
        formId: 'formInforme',
        selectId: 'clienteInforme',
        startId: 'fechaInicio',
        endId: 'fechaFin',
        resultId: 'resultadoInforme',
        buttonId: null,
        mode: 'classic'
    };
}

function sanitizeFileName(value) {
    return String(value || '')
        .normalize('NFD')
        .replace(/[\u0300-\u036f]/g, '')
        .toLowerCase()
        .replace(/[^a-z0-9]+/g, '_')
        .replace(/^_+|_+$/g, '') || 'informe';
}

function buildReportFileName(cliente, fechaInicio, fechaFin) {
    const today = new Date().toISOString().slice(0, 10).replace(/-/g, '');
    if (cliente === 'todos') {
        return `Informe_${today}.pdf`;
    }
    const safeCliente = sanitizeFileName(cliente);
    const inicio = (fechaInicio || today).replace(/-/g, '');
    const fin = (fechaFin || today).replace(/-/g, '');
    return `${safeCliente}_${inicio}_${fin}.pdf`;
}

function triggerPdfDownload(fileName, element) {
    if (typeof window.html2pdf !== 'function') {
        alert('No se pudo preparar el PDF porque la librería de exportación no está disponible.');
        return;
    }

    const options = {
        margin: [0.3, 0.3, 0.3, 0.3],
        filename: fileName,
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { scale: 2 },
        jsPDF: { unit: 'in', format: 'a4', orientation: 'portrait' }
    };

    window.html2pdf().set(options).from(element).save();
}

function getHorasTotalesEnMinutos(tramos) {
    return (tramos || []).reduce((sum, t) => {
        let minutosTotales = 0;
        if (t.inicio && t.fin) {
            const [h1, m1] = t.inicio.split(':').map(Number);
            const [h2, m2] = t.fin.split(':').map(Number);
            minutosTotales = (h2*60 + m2) - (h1*60 + m1);
            if(minutosTotales < 0) minutosTotales += 24*60;
        } else if (t.horas && t.horas.includes(':')) {
            const [h, m] = t.horas.split(':').map(Number);
            minutosTotales = h*60 + m;
        } else if (t.horas) {
            let partes = t.horas.split('.');
            let h = parseInt(partes[0] || '0');
            let m = parseInt(partes[1] || '0');
            if (m > 0) {
                if (m < 60) {
                    minutosTotales = h*60 + m;
                } else {
                    minutosTotales = h*60 + Math.round(60 * (parseFloat('0.'+m)));
                }
            } else {
                minutosTotales = h*60;
            }
        }
        return sum + minutosTotales;
    }, 0);
}

function formatearHoras(minutos) {
    if (!Number.isFinite(minutos)) return '-';
    const horas = Math.floor(minutos / 60).toString().padStart(2, '0');
    const mins = (minutos % 60).toString().padStart(2, '0');
    return `${horas}:${mins}`;
}

function formatearFecha(fecha) {
    if (!fecha) return '-';
    const texto = String(fecha);
    const match = texto.match(/^(\d{4})-(\d{2})-(\d{2})$/);
    if (!match) return texto;
    const [, anio, mes, dia] = match;
    return `${dia}-${mes}-${anio}`;
}

function renderInformeClassic(registros, cliente) {
    let html = '';
    const clientesAgrupados = {};
    registros.forEach(r => {
        if(!clientesAgrupados[r.cliente]) clientesAgrupados[r.cliente] = [];
        clientesAgrupados[r.cliente].push(r);
    });
    const clientesOrdenados = Object.keys(clientesAgrupados).sort((a, b) => a.localeCompare(b));
    clientesOrdenados.forEach(nombreCliente => {
        html += `<h3>Cliente: ${nombreCliente}</h3>`;
        html += `<table class='tabla-informe'><thead><tr><th>Fecha</th><th>Carga Horaria</th><th>Total horas</th></tr></thead><tbody>`;
        let totalCliente = 0;
        clientesAgrupados[nombreCliente].sort((a, b) => a.fecha.localeCompare(b.fecha)).forEach(r => {
            let tramosHtml = '';
            let totalDia = 0;
            r.tramos.forEach(t => {
                const minutosTotales = getHorasTotalesEnMinutos([t]);
                const horasFinal = formatearHoras(minutosTotales);
                tramosHtml += `${t.inicio || '-'} - ${t.fin || '-'} (${horasFinal})<br>`;
                totalDia += minutosTotales;
            });
            const totalDiaStr = formatearHoras(totalDia);
            html += `<tr><td>${formatearFecha(r.fecha)}</td><td>${tramosHtml}</td><td>${totalDiaStr}</td></tr>`;
            totalCliente += totalDia;
        });
        const totalClienteStr = formatearHoras(totalCliente);
        html += `<tr class='total-row'><td colspan='2'>Total ${nombreCliente}</td><td>${totalClienteStr}</td></tr>`;
        html += '</tbody></table>';
    });
    return html;
}

function renderInformeNuevo(registros, cliente, fechaInicio, fechaFin) {
    const clientesAgrupados = {};
    registros.forEach(r => {
        if(!clientesAgrupados[r.cliente]) clientesAgrupados[r.cliente] = [];
        clientesAgrupados[r.cliente].push(r);
    });
    const clientesOrdenados = Object.keys(clientesAgrupados).sort((a, b) => a.localeCompare(b));

    let html = '<div class="report-sheet">';
    html += '<div class="report-header">';
    html += '<div class="report-brand">';
    html += '<img src="../imagenes/logo_shalon.png" alt="Logo Shalon Morales">';
    html += '<div><h2>Informe de horas</h2><p>Periodo: ' + formatearFecha(fechaInicio || '-') + ' al ' + formatearFecha(fechaFin || '-') + '</p></div>';
    html += '</div>';
    html += `<div class="report-title">${cliente !== 'todos' ? cliente : 'Todos los clientes'}</div>`;
    html += '</div>';
    html += '<div class="report-summary">';
    clientesOrdenados.forEach(nombreCliente => {
        const totalCliente = clientesAgrupados[nombreCliente].reduce((sum, r) => sum + getHorasTotalesEnMinutos(r.tramos), 0);
        html += `<div class="report-card"><strong>${nombreCliente}</strong><span>${formatearHoras(totalCliente)} hs</span></div>`;
    });
    html += '</div>';
    html += '<table class="tabla-informe tabla-informe-nueva"><thead><tr><th>Cliente</th><th>Fecha</th><th>Detalle</th><th>Horas</th></tr></thead><tbody>';
    clientesOrdenados.forEach(nombreCliente => {
        clientesAgrupados[nombreCliente].sort((a, b) => a.fecha.localeCompare(b.fecha)).forEach(r => {
            const detalle = (r.tramos || []).map(t => `${t.inicio || '-'} - ${t.fin || '-'} (${formatearHoras(getHorasTotalesEnMinutos([t]))})`).join('<br>');
            const totalDia = formatearHoras(getHorasTotalesEnMinutos(r.tramos));
            html += `<tr><td>${nombreCliente}</td><td>${formatearFecha(r.fecha)}</td><td>${detalle}</td><td>${totalDia}</td></tr>`;
        });
    });
    html += '</tbody></table></div>';
    return html;
}

async function cargarClientesInforme(config = getReportConfig()) {
    const select = document.getElementById(config.selectId);
    if (!select) return;
    try {
        const resp = await fetch('../data/listaClientes.json');
        const clientes = await resp.json();
        clientes.sort((a, b) => a.nombre.localeCompare(b.nombre));
        clientes.forEach(c => {
            const opt = document.createElement('option');
            opt.value = c.nombre;
            opt.textContent = c.nombre;
            select.appendChild(opt);
        });
    } catch {}
}

function filtrarPorFecha(registros, inicio, fin) {
    return registros.filter(r => r.fecha >= inicio && r.fecha <= fin);
}

async function generarInforme(config, { shouldDownload = false } = {}) {
    const cliente = document.getElementById(config.selectId).value;
    const fechaInicio = document.getElementById(config.startId).value;
    const fechaFin = document.getElementById(config.endId).value;
    const resultado = document.getElementById(config.resultId);
    if (!resultado) return;

    resultado.innerHTML = '';
    let registros = [];
    try {
        const resp = await fetch('../data/horasClientes.json');
        registros = await resp.json();
    } catch {}
    registros = filtrarPorFecha(registros, fechaInicio, fechaFin);
    if(cliente !== 'todos') {
        registros = registros.filter(r => r.cliente === cliente);
    }
    if(registros.length === 0) {
        resultado.innerHTML = '<p>No hay registros para el periodo seleccionado.</p>';
        return;
    }

    const html = config.mode === 'nuevo' ? renderInformeNuevo(registros, cliente, fechaInicio, fechaFin) : renderInformeClassic(registros, cliente);
    resultado.innerHTML = html;

    if (shouldDownload) {
        const fileName = buildReportFileName(cliente, fechaInicio, fechaFin);
        window.setTimeout(() => triggerPdfDownload(fileName, resultado), 150);
    }
}

function setVistaInforme(vista) {
    vistaActual = vista;
    const config = getReportConfig();
    if (config.formId === 'formInforme') {
        const form = document.getElementById(config.formId);
        if (form) {
            form.dispatchEvent(new Event('submit'));
        }
    }
}

window.addEventListener('DOMContentLoaded', () => {
    const config = getReportConfig();
    cargarClientesInforme(config);
    const form = document.getElementById(config.formId);
    if (form) {
        form.addEventListener('submit', function(e) {
            e.preventDefault();
            generarInforme(config, { shouldDownload: false });
        });
    }
    const button = config.buttonId ? document.getElementById(config.buttonId) : null;
    if (button) {
        button.addEventListener('click', function(e) {
            e.preventDefault();
            generarInforme(config, { shouldDownload: true });
        });
    }
    if (config.mode === 'classic') {
        setVistaInforme('classic');
    }
});

```

### Archivo: `js/login.js`
```javascript
// login.js - Lógica de login para controlHorasClientes
// Utiliza SHA-256 para validar contraseñas

async function sha256(texto) {
    const encoder = new TextEncoder();
    const data = encoder.encode(texto);
    const hashBuffer = await crypto.subtle.digest('SHA-256', data);
    const hashArray = Array.from(new Uint8Array(hashBuffer));
    return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
}

async function validarLogin(documento, contrasena) {
    const resp = await fetch('../data/usuarios.json');
    const usuarios = await resp.json();
    const hash = await sha256(contrasena);
    const usuario = usuarios.find(u => u.documento === documento && u.contrasena === hash && u.activo);
    return usuario || null;
}

// Ejemplo de uso:
// document.getElementById('loginForm').onsubmit = async function(e) {
//     e.preventDefault();
//     const email = document.getElementById('email').value;
//     const pass = document.getElementById('contrasena').value;
//     const usuario = await validarLogin(email, pass);
//     if(usuario) {
//         // Login exitoso
//     } else {
//         // Mostrar error
//     }
// }

```

### Archivo: `js/registro_horas.js`
```javascript
// registro_horas.js - Lógica para registrar horas por cliente y día

// Cargar tramos existentes al cambiar cliente o fecha
document.getElementById('clienteSelect').addEventListener('change', cargarTramosExistentes);
document.getElementById('fecha').addEventListener('change', cargarTramosExistentes);

function cargarTramosExistentes() {
    const cliente = document.getElementById('clienteSelect').value;
    const fecha = document.getElementById('fecha').value;
    if (!cliente || !fecha) {
        tramos = [];
        renderTramos();
        return;
    }
    fetch('../data/horasClientes.json')
        .then(resp => resp.json())
        .then(registros => {
            const registro = registros.find(r => r.cliente === cliente && r.fecha === fecha);
            tramos = registro ? [...registro.tramos] : [];
            renderTramos();
        })
        .catch(() => {
            tramos = [];
            renderTramos();
        });
}

let tramos = [];

// Cargar clientes en el select
async function cargarClientes() {
    const select = document.getElementById('clienteSelect');
    select.innerHTML = '';
    try {
        const resp = await fetch('../data/listaClientes.json');
        const clientes = await resp.json();
        clientes.sort((a, b) => a.nombre.localeCompare(b.nombre));
        clientes.filter(c => c.activo !== false).forEach(c => {
            const opt = document.createElement('option');
            opt.value = c.nombre;
            opt.textContent = c.nombre;
            select.appendChild(opt);
        });
    } catch {}
}

// Renderizar tramos en la tabla tipo Excel
function renderTramos() {
    const tbody = document.getElementById('tbodyTramos');
    if (!tbody) return;
    tbody.innerHTML = '';
    tramos.forEach((tramo, idx) => {
        const tr = document.createElement('tr');
        tr.innerHTML = `
            <td style='padding:6px; border:1px solid #ccc;'>${tramo.inicio || '-'}</td>
            <td style='padding:6px; border:1px solid #ccc;'>${tramo.fin || '-'}</td>
            <td style='padding:6px; border:1px solid #ccc;'>${tramo.horas || '-'}</td>
            <td style='padding:6px; border:1px solid #ccc;'>
                <div style="display:flex; gap:8px; justify-content:center;">
                    <button type='button' onclick='editarTramo(${idx})'>Editar</button>
                    <button type='button' onclick='eliminarTramo(${idx})'>Eliminar</button>
                </div>
            </td>
        `;
        tbody.appendChild(tr);
    });
}

window.editarTramo = function(idx) {
    const tramo = tramos[idx];
    document.getElementById('nuevoInicio').value = tramo.inicio;
    document.getElementById('nuevoFin').value = tramo.fin;
    document.getElementById('nuevoHoras').value = tramo.horas;
    // Al editar, eliminar el tramo actual para que se re-agregue
    tramos.splice(idx, 1);
    renderTramos();
}

window.actualizarTramo = function(idx, campo, valor) {
    tramos[idx][campo] = valor;
    renderTramos();
}

window.eliminarTramo = function(idx) {
    tramos.splice(idx, 1);
    renderTramos();
}


document.addEventListener('click', function(e) {
    if(e.target && e.target.id === 'addTramoBtn') {
        const inicio = document.getElementById('nuevoInicio').value;
        const fin = document.getElementById('nuevoFin').value;
        let horas = document.getElementById('nuevoHoras').value;
        const msg = document.getElementById('msgRegistro');
        // Validar que la hora final sea estrictamente mayor que la inicial (no permite cruzar medianoche)
        if(inicio && fin) {
            const [h1, m1] = inicio.split(':').map(Number);
            const [h2, m2] = fin.split(':').map(Number);
            const minutosInicio = h1*60 + m1;
            const minutosFin = h2*60 + m2;
            if(minutosFin <= minutosInicio) {
                msg.textContent = 'La hora final debe ser mayor que la hora inicial (no se permite cruzar medianoche).';
                msg.style.color = '#c00';
                return;
            }
            let totalMin = minutosFin - minutosInicio;
            const horasEnteras = Math.floor(totalMin / 60);
            const minutos = totalMin % 60;
            horas = `${horasEnteras}:${minutos.toString().padStart(2,'0')}`;
        }
        if(!inicio && !fin && !horas) return;
        tramos.push({ inicio, fin, horas });
        document.getElementById('nuevoInicio').value = '';
        document.getElementById('nuevoFin').value = '';
        document.getElementById('nuevoHoras').value = '';
        msg.textContent = '';
        renderTramos();
    }
});

document.getElementById('registroHorasForm').onsubmit = async function(e) {
    e.preventDefault();
    const cliente = document.getElementById('clienteSelect').value;
    const fecha = document.getElementById('fecha').value;
    const msg = document.getElementById('msgRegistro');
    if (!cliente || !fecha || tramos.length === 0) {
        msg.textContent = 'Completa todos los campos y agrega al menos un tramo.';
        msg.style.color = '#c00';
        return;
    }
    // Leer registros existentes
    let registros = [];
    try {
        const resp = await fetch('../data/horasClientes.json');
        registros = await resp.json();
    } catch {}
    // Validar solapamientos con otros clientes en la misma fecha
    let conflictos = [];
    registros.filter(r => r.fecha === fecha && r.cliente !== cliente).forEach(r => {
        r.tramos.forEach((t2, idx2) => {
            tramos.forEach((t1, idx1) => {
                if (t1.inicio && t1.fin && t2.inicio && t2.fin) {
                    // Convertir a minutos
                    const t1ini = t1.inicio.split(':').map(Number);
                    const t1fin = t1.fin.split(':').map(Number);
                    const t2ini = t2.inicio.split(':').map(Number);
                    const t2fin = t2.fin.split(':').map(Number);
                    const t1start = t1ini[0]*60 + t1ini[1];
                    const t1end = t1fin[0]*60 + t1fin[1];
                    const t2start = t2ini[0]*60 + t2ini[1];
                    const t2end = t2fin[0]*60 + t2fin[1];
                    // Verificar solapamiento
                    if (t1start < t2end && t2start < t1end) {
                        conflictos.push(`Conflicto: ${cliente} (${t1.inicio}-${t1.fin}) y ${r.cliente} (${t2.inicio}-${t2.fin}) en ${fecha}`);
                    }
                    // Verificar franja idéntica
                    if (t1.inicio === t2.inicio && t1.fin === t2.fin) {
                        conflictos.push(`Franja idéntica: ${cliente} y ${r.cliente} (${t1.inicio}-${t1.fin}) en ${fecha}`);
                    }
                }
            });
        });
    });
    if (conflictos.length > 0) {
        mostrarModalConflictos(conflictos.join('\n'));
        return;
    }
    // Buscar si ya existe registro para cliente y fecha
    let registro = registros.find(r => r.cliente === cliente && r.fecha === fecha);
    if (registro) {
        registro.tramos = tramos;
    } else {
        registros.push({ cliente, fecha, tramos });
    }
    cargarClientes();
    renderTramos();

    // Modal para mostrar conflictos
    function mostrarModalConflictos(mensaje) {
        let modal = document.getElementById('modalConflictos');
        if (!modal) {
            modal = document.createElement('div');
            modal.id = 'modalConflictos';
            modal.style.position = 'fixed';
            modal.style.top = '0';
            modal.style.left = '0';
            modal.style.width = '100vw';
            modal.style.height = '100vh';
            modal.style.background = 'rgba(0,0,0,0.4)';
            modal.style.display = 'flex';
            modal.style.alignItems = 'center';
            modal.style.justifyContent = 'center';
            modal.innerHTML = `<div style='background:#fff; padding:32px; border-radius:8px; max-width:500px; box-shadow:0 2px 12px #0003;'>
                <h3 style='margin-top:0;'>Conflicto de horarios</h3>
                <textarea style='width:100%;height:120px;'>${mensaje}</textarea>
                <div style='margin-top:18px; text-align:right;'><button id='cerrarModalConflictos'>Cerrar</button></div>
            </div>`;
            document.body.appendChild(modal);
            document.getElementById('cerrarModalConflictos').onclick = function() {
                modal.remove();
            };
        } else {
            modal.querySelector('textarea').value = mensaje;
            modal.style.display = 'flex';
        }
    }
    // Guardar realmente usando función serverless
    try {
        const resp = await fetch('/.netlify/functions/update-horas', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ data: registros })
        });
        const result = await resp.json();
        if (resp.ok && result.success) {
            msg.textContent = 'Registro guardado correctamente.';
            msg.style.color = '#080';
            tramos = [];
            renderTramos();
            document.getElementById('registroHorasForm').reset();
        } else {
            msg.textContent = 'Error al guardar: ' + (result.error || '');
            msg.style.color = '#c00';
        }
    } catch {
        msg.textContent = 'No se pudo guardar el registro (error de red o función).';
        msg.style.color = '#c00';
    }
};

cargarClientes();
renderTramos();

```

### Archivo: `netlify/functions/package.json`
```json
{
  "name": "controlhorasclientes-functions",
  "version": "1.0.0",
  "description": "Funciones serverless para controlHorasClientes",
  "main": "index.js",
  "dependencies": {
    "node-fetch": "^2.6.7"
  }
}

```

### Archivo: `netlify/functions/update-clientes.js`
```javascript
const fetch = require('node-fetch');

exports.handler = async (event, context) => {
  if (event.httpMethod !== 'POST') {
    return {
      statusCode: 405,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Método no permitido' }),
    };
  }

  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization',
        'Access-Control-Allow-Methods': 'POST, OPTIONS',
      },
      body: '',
    };
  }

  try {
    const { data, token } = JSON.parse(event.body);
    if (!data || !Array.isArray(data)) {
      return {
        statusCode: 400,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Datos requeridos y deben ser un array' }),
      };
    }
    for (const item of data) {
      if (!item.nombre) {
        return {
          statusCode: 400,
          headers: {
            'Access-Control-Allow-Origin': '*',
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ error: 'Cada cliente debe tener nombre' }),
        };
      }
    }
    const githubToken = process.env.GITHUB_TOKEN || token;
    const repo = process.env.GITHUB_REPO || 'FABIOR1981/controlHorasClientes';
    const branch = process.env.GITHUB_BRANCH || 'main';
    const filePath = 'data/listaClientes.json';
    if (!githubToken) {
      return {
        statusCode: 401,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Token de GitHub no configurado' }),
      };
    }
    const fileUrl = `https://api.github.com/repos/${repo}/contents/${filePath}?ref=${branch}`;
    const fileResponse = await fetch(fileUrl, {
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3+json',
      },
    });
    if (!fileResponse.ok) {
      const error = await fileResponse.text();
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al obtener el archivo de GitHub', details: error }),
      };
    }
    const fileData = await fileResponse.json();
    const currentSha = fileData.sha;
    const newContent = JSON.stringify(data, null, 2);
    const encodedContent = Buffer.from(newContent).toString('base64');
    const updateResponse = await fetch(fileUrl, {
      method: 'PUT',
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3+json',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        message: 'Actualización de clientes realizada.',
        content: encodedContent,
        sha: currentSha,
        branch: branch,
      }),
    });
    if (!updateResponse.ok) {
      const error = await updateResponse.text();
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al actualizar el archivo en GitHub', details: error }),
      };
    }
    const updateResult = await updateResponse.json();
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ success: true, message: 'Archivo listaClientes.json actualizado en GitHub', commit: updateResult.commit.sha }),
    };
  } catch (error) {
    return {
      statusCode: 500,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Error interno del servidor', details: error.message }),
    };
  }
};

```

### Archivo: `netlify/functions/update-horas.js`
```javascript
const fetch = require('node-fetch');

exports.handler = async (event, context) => {
  if (event.httpMethod !== 'POST') {
    return {
      statusCode: 405,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Método no permitido' }),
    };
  }

  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization',
        'Access-Control-Allow-Methods': 'POST, OPTIONS',
      },
      body: '',
    };
  }

  try {
    const { data, token } = JSON.parse(event.body);
    if (!data || !Array.isArray(data)) {
      return {
        statusCode: 400,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Datos requeridos y deben ser un array' }),
      };
    }
    for (const item of data) {
      if (!item.cliente || !item.fecha || !Array.isArray(item.tramos)) {
        return {
          statusCode: 400,
          headers: {
            'Access-Control-Allow-Origin': '*',
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ error: 'Cada registro debe tener cliente, fecha y tramos' }),
        };
      }
    }
    const githubToken = process.env.GITHUB_TOKEN || token;
    const repo = process.env.GITHUB_REPO || 'FABIOR1981/controlHorasClientes';
    const branch = process.env.GITHUB_BRANCH || 'main';
    const filePath = 'data/horasClientes.json';
    if (!githubToken) {
      return {
        statusCode: 401,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Token de GitHub no configurado' }),
      };
    }
    const fileUrl = `https://api.github.com/repos/${repo}/contents/${filePath}?ref=${branch}`;
    const fileResponse = await fetch(fileUrl, {
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3+json',
      },
    });
    if (!fileResponse.ok) {
      const error = await fileResponse.text();
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al obtener el archivo de GitHub', details: error }),
      };
    }
    const fileData = await fileResponse.json();
    const currentSha = fileData.sha;
    const newContent = JSON.stringify(data, null, 2);
    const encodedContent = Buffer.from(newContent).toString('base64');
    const updateResponse = await fetch(fileUrl, {
      method: 'PUT',
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3+json',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        message: 'Actualización de horas realizada.',
        content: encodedContent,
        sha: currentSha,
        branch: branch,
      }),
    });
    if (!updateResponse.ok) {
      const error = await updateResponse.text();
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al actualizar el archivo en GitHub', details: error }),
      };
    }
    const updateResult = await updateResponse.json();
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ success: true, message: 'Archivo horasClientes.json actualizado en GitHub', commit: updateResult.commit.sha }),
    };
  } catch (error) {
    return {
      statusCode: 500,
      headers: {
        'Access-Control-Allow-Origin': '*',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Error interno del servidor', details: error.message }),
    };
  }
};

```

### Archivo: `package.json`
```json
{
  "name": "controlhorasclientes",
  "version": "1.0.0",
  "description": "Sistema de control de horas por cliente",
  "main": "index.html",
  "scripts": {},
  "dependencies": {
    "node-fetch": "^2.6.7"
  },
  "author": "",
  "license": "ISC"
}

```

