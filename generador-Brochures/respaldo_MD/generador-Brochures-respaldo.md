# Bundle del Proyecto para IA: generador-Brochures

## Estructura de Archivos
- PRODUCT.md
- README.md
- css/styles.css
- documentacion/LEEME.md
- index.html
- js/main.js

---

## Código Fuente

### Archivo: `PRODUCT.md`
```md
# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Uso interno, confirmado por el usuario. No se ha especificado quién lo opera ni en qué situación concreta.

## Product Purpose

La aplicación permite preparar brochures profesionales editando datos de perfil, servicios, público, clientes, contacto y estilo, con vista previa y salida para guardar como PDF o imprimir en A4. El propósito y los criterios de éxito más allá de ese flujo todavía no se han confirmado.

## Positioning

Pendiente de definir; no se ha confirmado qué diferencia al producto de editar una plantilla manualmente.

## Operating Context

La interfaz está en español. Incluye perfiles preconfigurados para psicología clínica, psicología organizacional, OEC y calidad, e ingeniería de software. Permite añadir logotipos de clientes y personalizar la plantilla y el color de acento.

## Capabilities and Constraints

- Aplicación web implementada con HTML, CSS y JavaScript.
- Ofrece cuatro espacios para servicios y admite hasta ocho logotipos de clientes.
- La foto de perfil puede indicarse por URL o cargarse desde el equipo.
- El nombre, teléfono y correo de Rita Morales están precompletados y bloqueados en el formulario actual; que deban permanecer así es una decisión pendiente de confirmar.
- Los nombres de clientes y demás datos de los perfiles preconfigurados aparecen en el código; su vigencia y autorización para uso real no están confirmadas.

## Evidence on Hand

El propio código contiene los perfiles preconfigurados y una URL de imagen de Cloudinary. No se encontraron pruebas verificadas, testimonios ni métricas de resultados.

```

### Archivo: `README.md`
```md
# Generador de Brochures

Herramienta web para armar brochures profesionales de una página. Se completan los datos en un panel lateral, se ve el resultado al instante y se guarda como PDF o se imprime en A4.

Sitio publicado: https://generador-brochures.netlify.app

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/generador-Brochures/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/generador-Brochures/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## Funcionalidades

- **Perfiles predeterminados** para empezar rápido: psicología clínica, psicología organizacional, OEC y calidad, e ingeniería de software.
- Panel con 5 pestañas:
  1. **Perfil**: nombre, título profesional, registro profesional, frase principal, presentación y foto (por URL o subida desde el equipo).
  2. **Servicios**: enfoque, a quién está dirigido y hasta 4 servicios principales.
  3. **Clientes**: nombres de empresas y hasta 8 logos (subidos desde el equipo o por URL).
  4. **Contacto**: datos de contacto y modalidad de atención.
  5. **Estilo**: plantilla visual y color de acento.
- **Vista previa** en tiempo real, escalada a la pantalla.
- **Guardado automático** en el navegador: al volver a abrir la página, sigue el trabajo donde quedó.
- Botón **Guardar como PDF / Imprimir** con formato A4.

## Cómo se usa

1. Abrí la página y elegí un perfil predeterminado, o completá los datos desde cero.
2. Recorré las pestañas y ajustá textos, foto, servicios, clientes y estilo.
3. Revisá la vista previa.
4. Tocá **Guardar como PDF / Imprimir** y en el diálogo de impresión elegí "Guardar como PDF" o tu impresora.

## Ejecutar localmente

No necesita instalación ni build. Abrí `index.html` en el navegador o serví la carpeta con cualquier servidor estático (por ejemplo `npx serve`).

## Estructura

```
index.html        Panel de edición y vista previa
css/styles.css    Estilos del panel y de las plantillas del brochure
js/main.js        Pestañas, perfiles, render de la vista previa, logos y guardado
PRODUCT.md        Notas de producto
```

```

### Archivo: `css/styles.css`
```css
html { font-size: clamp(14px, 0.95vw, 18px); }
* { box-sizing: border-box; }
body { margin: 0; font-family: Figtree, system-ui, sans-serif; color: #1b2430; background: #e6e9ed; }
.app { display: grid; grid-template-columns: 480px 1fr; height: 100vh; overflow: hidden; }

.panel { background: #fff; display: flex; flex-direction: column; border-right: 1px solid #d5dae0; height: 100vh; overflow: hidden; }
.panel-header { padding: 16px 20px 12px; border-bottom: 1px solid #e3e7ec; }
.panel-header h1 { font-size: 1.2rem; margin: 0 0 10px; color: #0f172a; font-weight: 700; }

/* Sistema de Pestañas Modernas */
.tabs-nav { display: flex; background: #f1f5f9; padding: 4px; gap: 4px; border-bottom: 1px solid #e2e8f0; overflow-x: auto; }
.tab-btn { flex: 1; padding: 8px 10px; border: 0; background: transparent; font: 600 0.8rem Figtree, sans-serif; color: #55606e; cursor: pointer; border-radius: 6px; transition: all 0.2s; white-space: nowrap; text-align: center; }
.tab-btn:hover { color: #0f172a; background: #e2e8f0; }
.tab-btn.active { background: #fff; color: #1f4e5f; box-shadow: 0 1px 3px rgba(0,0,0,0.08); }

.panel-body { padding: 20px; overflow-y: auto; flex: 1; }
.tab-pane { display: none; }
.tab-pane.active { display: block; }

fieldset { border: 0; margin: 0 0 16px 0; padding: 0; }
legend { font-weight: 700; font-size: 0.95rem; color: #334155; margin-bottom: 8px; }
label { display: block; font-size: 0.85rem; color: #55606e; margin: 10px 0 3px; font-weight: 500; }
input, select, textarea { width: 100%; font: inherit; font-size: 0.9rem; padding: 8px 10px; border: 1px solid #c9d0d8; border-radius: 6px; background: #fff; }
textarea { resize: vertical; }
.ayuda { font-size: 0.75rem; color: #55606e; margin: 4px 0 0; }
.fila-url { display: flex; gap: 6px; align-items: stretch; }
.fila-url input { flex: 1; min-width: 0; }
.fila-url .btn { width: auto; margin-top: 0; padding: 8px 14px; }
.logos-lista { display: grid; grid-template-columns: repeat(auto-fill, minmax(96px, 1fr)); gap: 8px; margin-top: 10px; }
.logo-item { position: relative; height: 68px; padding: 8px; border: 1px solid #c9d0d8; border-radius: 6px; background: #fff; display: flex; align-items: center; justify-content: center; }
.logo-item img { max-width: 100%; max-height: 100%; object-fit: contain; }
.logo-item button { position: absolute; top: 3px; right: 3px; width: 24px; height: 24px; border: 0; border-radius: 50%; background: #1f4e5f; color: #fff; font-size: 15px; line-height: 1; cursor: pointer; }
.logo-item button:hover { background: #153743; }
.sep-sm { margin-top: 6px; }
.sep-md { margin-top: 12px; }
.input-color { height: 38px; padding: 3px; }
/* Foco visible para teclado */
:focus-visible { outline: 2px solid #1f4e5f; outline-offset: 2px; }
input:focus-visible, select:focus-visible, textarea:focus-visible { border-color: #1f4e5f; }
.tab-btn { min-height: 36px; }
@media (prefers-reduced-motion: reduce) { * { transition: none !important; } }
.chk { display: flex; gap: 8px; align-items: center; margin-top: 10px; font-size: 0.85rem; color: #1b2430; }
.chk input { width: auto; }

.panel-footer { padding: 12px 20px; border-top: 1px solid #e3e7ec; background: #f8fafc; }
.btn { width: 100%; padding: 11px; border: 0; border-radius: 8px; background: #1f4e5f; color: #fff; font: 600 0.95rem Figtree, sans-serif; cursor: pointer; transition: background 0.2s; }
.btn:hover { background: #153743; }
.btn.sec { background: #e3e7ec; color: #1b2430; margin-top: 6px; padding: 8px; font-size: 0.85rem; }
.btn.sec:hover { background: #cbd5e1; }
#aviso { display: none; margin-bottom: 10px; padding: 8px 10px; border-radius: 6px; background: #fff3cd; color: #664d03; font-size: 0.8rem; }

/* Vista Previa */
.vista { overflow: auto; padding: 32px; display: flex; justify-content: center; align-items: flex-start; background: #f1f3f5; }
.stage { flex: none; }
#hoja { width: 210mm; min-height: 297mm; height: auto; background: #fff; overflow: visible; transform-origin: 0 0; box-shadow: 0 10px 30px rgba(0,0,0,.15); font-size: 10.25pt; line-height: 1.55; }
:where(#hoja) :where(p,h1,h2,ul){ margin: 0; }
:where(#hoja) ul { padding: 0; list-style: none; }
:where(#hoja) li { padding: .7mm 0; }
:where(#hoja) .foto { overflow: hidden; display: flex; align-items: center; justify-content: center; flex: none; }
:where(#hoja) .foto img { width: 100%; height: 100%; object-fit: cover; }
:where(#hoja) .lab { display: block; font-size: 8.5pt; color: var(--mut); }
:where(#hoja) .clientes-bloque { margin-top: 4mm; }
:where(#hoja) .clientes-bloque .lab { margin-bottom: 1.5mm; }
:where(#hoja) .clientes-grid { display: flex; flex-wrap: wrap; gap: 3mm; margin-top: 2mm; }
:where(#hoja) .cliente-tag { background: #e2e8f0; padding: 1.5mm 3mm; border-radius: 4px; font-size: 8.5pt; font-weight: 600; color: #334155; }
:where(#hoja) .logos-grid { display: flex; flex-wrap: wrap; align-items: center; gap: 3mm 6mm; margin: 2mm 0; }
:where(#hoja) .logos-grid img { height: 11mm; width: auto; max-width: 34mm; object-fit: contain; }
:where(#hoja) .aparte { margin-top: 2mm; }
:where(#hoja) section { margin-bottom: 4mm; }
:where(#hoja) :where(h2, .serv, .serv-card, .perfil-exec, .foto, .cliente-tag) { break-inside: avoid; }
:where(#hoja) h2 { break-after: avoid; }

/* Plantillas Visuales Originales (t1, t2, t3) */
.t1 { --ink:#1d2b2a; --mut:#4a5a58; --acc:#3f6f68; --soft:#e4eeeb; display: grid; grid-template-columns: 72mm 1fr; color: var(--ink); }
.t1 .lado { background: var(--soft); padding: 18mm 11mm; display: flex; flex-direction: column; gap: 8mm; }
.t1 .foto { width: 50mm; height: 64mm; border-radius: 25mm 25mm 3mm 3mm; background: var(--acc); color: var(--on-acc); font: 500 32pt Newsreader, serif; }
.t1 .cont div { margin-bottom: 4mm; overflow-wrap: anywhere; }
.t1 .main { padding: 17mm 16mm 14mm 14mm; display: flex; flex-direction: column; gap: 5mm; }
.t1 h1 { font: 400 24pt/1.15 Newsreader, serif; } .t1 .quien b { font-size: 12.5pt; }
.t1 h2 { font: 600 10.5pt Figtree, sans-serif; color: var(--ink); margin-bottom: 1.5mm; }
.t1 .serv { border-top: .3mm solid #cfdad7; padding: 2.5mm 0; } .t1 .serv b { display: block; }
.t1 .meta { margin-top: auto; display: grid; grid-template-columns: repeat(3, 1fr); gap: 4mm; font-size: 9pt; }
.t1 .nota { font-size: 8pt; color: var(--mut); }

.t2 { --ink:#14233a; --mut:#4f5d72; --acc:#2f5fa8; --soft:#eef2f7; display: flex; flex-direction: column; font-family: 'Libre Franklin', sans-serif; color: var(--ink); }
.t2 .top { background: var(--ink); color: #fff; padding: 16mm 16mm 12mm; display: flex; justify-content: space-between; gap: 10mm; align-items: flex-end; }
.t2 .top h1 { font: 700 22pt/1.2 'Libre Franklin', sans-serif; margin-bottom: 4mm; } .t2 .top .lab { color: #b9c4d6; }
.t2 .foto { width: 42mm; height: 42mm; border-radius: 3mm; background: var(--acc); color: var(--on-acc); font: 700 24pt 'Libre Franklin', sans-serif; }
.t2 .quien b { font-size: 11.5pt; }
.t2 .cuerpo { flex: 1; display: grid; grid-template-columns: 1fr 1.3fr; gap: 12mm; padding: 12mm 16mm; }
.t2 .cuerpo > div { display: flex; flex-direction: column; }
.t2 .cuerpo section, .t2 .cuerpo > div > p { margin-bottom: 6mm; }
.t2 h2 { font: 700 10.5pt 'Libre Franklin', sans-serif; color: var(--ink); margin-bottom: 2mm; }
.t2 .serv { border-left: 1.2mm solid var(--acc); padding: 1mm 0 1mm 4mm; margin-bottom: 5mm; } .t2 .serv b { display: block; }
.t2 .meta { margin-top: auto; } .t2 .meta div { margin-bottom: 3mm; }
.t2 .pie { background: var(--soft); padding: 8mm 16mm; display: grid; grid-template-columns: repeat(auto-fit, minmax(36mm, 1fr)); gap: 5mm; font-size: 9pt; }
.t2 .nota { grid-column: 1/-1; font-size: 8pt; color: var(--mut); }

.t3 { --ink:#17181a; --mut:#5a5e65; --acc:#7a3e5a; padding: 18mm 20mm 14mm; display: flex; flex-direction: column; gap: 5mm; color: var(--ink); }
.t3 .cab { display: flex; justify-content: space-between; gap: 10mm; align-items: flex-start; }
.t3 h1 { font: 400 36pt/1.05 Newsreader, serif; margin-bottom: 3mm; } .t3 .lab { font-size: 9pt; }
.t3 .foto { width: 34mm; height: 34mm; border-radius: 50%; background: var(--acc); color: var(--on-acc); font: 400 24pt Newsreader, serif; }
.t3 .tit { font: italic 400 16pt/1.3 Newsreader, serif; color: #6b344d; max-width: 140mm; }
.t3 .fila { display: grid; grid-template-columns: 42mm 1fr; gap: 5mm; border-top: .25mm solid #d9dbe0; padding: 2.5mm 0; } .t3 .fila > b { font-weight: 600; }
.t3 .pie { margin-top: auto; border-top: .3mm solid var(--ink); padding-top: 4mm; display: flex; flex-wrap: wrap; gap: 3mm 10mm; font-size: 9pt; }
.t3 .nota { width: 100%; font-size: 8pt; color: var(--mut); }

/* Plantillas Corporativas Actualizadas para respetar --acc */

/* t4: Ejecutivo Corporativo (Usa --acc en bordes y detalles) */
.t4 { --ink:#0f172a; --mut:#475569; --acc:#c59b27; --soft:#f4ebd0; display: flex; flex-direction: column; font-family: 'Libre Franklin', sans-serif; color: var(--ink); background: #ffffff; }
.t4 .hero-exec { background: #0d1b2a; color: #f8fafc; padding: 14mm 16mm 10mm; border-bottom: 3px solid var(--acc); display: flex; justify-content: space-between; align-items: center; }
.t4 .hero-exec .quien, .t5 .top-modern .quien { color: #cbd5e1; }
.t4 .hero-exec .lab, .t5 .top-modern .lab { color: #b9c4d6; }
.t4 .sello-exec { font-size: 8.5pt; border: 1px solid var(--acc); padding: 2mm 4mm; color: #fff; border-radius: 3px; text-transform: uppercase; font-weight: 600; flex: none; }
.t4 .enfoque-exec { margin-top: 3mm; font-size: 9pt; color: var(--mut); }
.t4 .meta-exec { display: grid; grid-template-columns: repeat(3, 1fr); gap: 4mm; font-size: 9pt; }
.t4 .pie-exec .lab, .t5 .pie-modern .lab, .t6 .pie-bio .lab { color: inherit; }
.t4 .pie-exec .nota { grid-column: 1/-1; font-size: 8pt; }
.t5 .pie-modern, .t6 .pie-bio { flex-wrap: wrap; gap: 2mm 8mm; }
.t5 .pie-modern .nota, .t6 .pie-bio .nota { width: 100%; font-size: 8pt; }
.t4 .hero-exec h1 { font: 700 20pt/1.2 'Libre Franklin', sans-serif; color: #fdf8e2; margin-bottom: 2mm; }
.t4 .cuerpo-exec { padding: 12mm 16mm; flex: 1; display: flex; flex-direction: column; gap: 8mm; }
.t4 .perfil-exec { display: grid; grid-template-columns: 1fr 40mm; gap: 10mm; align-items: center; background: #f8fafc; padding: 8mm; border: 1px solid #e2e8f0; border-radius: 4px; }
.t4 .foto { width: 40mm; height: 40mm; border-radius: 4px; background: var(--acc); color: var(--on-acc); font: 700 24pt 'Libre Franklin', sans-serif; }
.t4 h2 { font: 700 11pt 'Libre Franklin', sans-serif; color: #0d1b2a; border-bottom: 1.5px solid var(--acc); padding-bottom: 1mm; margin-bottom: 3mm; }
.t4 .servicios-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 4mm; }
.t4 .serv-card { background: var(--soft); padding: 5mm; border-top: 4px solid var(--acc); border-radius: 3px; font-size: 9pt; }
.t4 .serv-card b { display: block; margin-bottom: 1.5mm; color: #1e293b; }
.t4 .pie-exec { background: #0d1b2a; color: #94a3b8; padding: 8mm 16mm; display: grid; grid-template-columns: repeat(3, 1fr); gap: 6mm; font-size: 8.5pt; }
.t4 .pie-exec b { color: #fff; display: block; margin-bottom: 1mm; }

/* t5: Corporativo Moderno (Usa --acc) */
.t5 { --ink:#1e293b; --mut:#64748b; --acc:#0284c7; --soft:#f1f5f9; display: flex; flex-direction: column; font-family: Figtree, sans-serif; color: var(--ink); }
.t5 .top-modern { background: #1e293b; color: #fff; padding: 14mm 16mm; display: flex; justify-content: space-between; align-items: center; }
.t5 .top-modern h1 { font: 700 20pt/1.2 Figtree, sans-serif; color: #fff; margin-bottom: 2mm; }
.t5 .foto { width: 38mm; height: 38mm; border-radius: 50%; background: var(--acc); color: var(--on-acc); font: 700 22pt Figtree, sans-serif; border: 2px solid var(--acc); }
.t5 .cuerpo-modern { padding: 12mm 16mm; flex: 1; display: grid; grid-template-columns: 1fr 1fr; gap: 10mm; }
.t5 h2 { font: 700 10.5pt Figtree, sans-serif; color: var(--ink); margin-bottom: 2mm; text-transform: uppercase; letter-spacing: 0.5px; }
.t5 .serv { background: #f8fafc; border-left: 3px solid var(--acc); padding: 3mm 4mm; margin-bottom: 3mm; border-radius: 0 4px 4px 0; font-size: 9pt; }
.t5 .serv b { display: block; color: #0f172a; }
.t5 .pie-modern { background: var(--soft); padding: 8mm 16mm; border-top: 1px solid #cbd5e1; display: flex; justify-content: space-between; font-size: 8.5pt; }

/* t6: Bienestar Institucional (Usa --acc) */
.t6 { --ink:#1b3022; --mut:#4a6b57; --acc:#2e6940; --soft:#eaf2ed; display: flex; flex-direction: column; font-family: Figtree, sans-serif; color: var(--ink); }
.t6 .header-bio { background: var(--soft); padding: 14mm 16mm; display: flex; gap: 10mm; align-items: center; border-bottom: 2px solid var(--acc); }
.t6 .header-bio h1 { font: 700 22pt/1.2 Figtree, sans-serif; color: var(--ink); margin-bottom: 2mm; }
.t6 .foto { width: 42mm; height: 42mm; border-radius: 6mm; background: var(--acc); color: var(--on-acc); font: 700 24pt Figtree, sans-serif; }
.t6 .cuerpo-bio { padding: 12mm 16mm; flex: 1; display: grid; grid-template-columns: 1fr 1fr; gap: 10mm; }
.t6 h2 { font: 700 11pt Figtree, sans-serif; color: var(--ink); margin-bottom: 2mm; border-bottom: 1px solid #cbd5e1; padding-bottom: 1mm; }
.t6 .serv { padding: 2.5mm 0; border-bottom: 1px dashed #cbd5e1; font-size: 9pt; }
.t6 .serv b { display: block; color: #112218; }
.t6 .pie-bio { background: #1b3022; color: #cbd5e1; padding: 8mm 16mm; display: flex; justify-content: space-between; font-size: 8.5pt; }
.t6 .pie-bio b { color: #fff; }

@media (max-width: 1000px) { .app { grid-template-columns: 1fr; height: auto; overflow: visible; } .panel { height: auto; overflow: visible; } .vista { padding: 12px; } }
@media print { @page { size: A4; margin: 0; } body { background: #fff; -webkit-print-color-adjust: exact; print-color-adjust: exact; } .app { display: block; height: auto; overflow: visible; } .panel { display: none; } .vista { padding: 0; display: block; overflow: visible; } .stage { width: 210mm !important; height: auto !important; } #hoja { transform: none !important; box-shadow: none !important; } }
```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/generador-Brochures/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Generador Profesional de Brochures</title>
    <!-- Fuentes de Google -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <meta name="description" content="Generador de brochures profesionales en formato A4 con vista previa en tiempo real.">
    <link href="https://fonts.googleapis.com/css2?family=Figtree:wght@400;500;600;700&family=Libre+Franklin:wght@400;600;700&family=Newsreader:ital,opsz,wght@0,6..72,400;0,6..72,500;1,6..72,400&display=swap" rel="stylesheet">
    <!-- Enlace a la hoja de estilos externa -->
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
<main class="app">
    <section class="panel" aria-label="Edición del brochure">
        <div class="panel-header">
            <h1>Generador de Brochures</h1>
            <label for="preset">Cargar perfil predeterminado</label>
            <select id="preset">
                <option value="clinica">Psicóloga Clínica (Pacientes / Adultos)</option>
                <option value="org">Psicóloga Laboral / Organizacional</option>
                <option value="oec">Especialista en OEC y Calidad</option>
                <option value="dev">Ingeniera de Software & Tech</option>
            </select>
        </div>

        <!-- Pestañas de Navegación del Panel Lateral -->
        <div class="tabs-nav" role="tablist" aria-label="Secciones del formulario">
            <button type="button" id="tab-0" role="tab" class="tab-btn active" aria-selected="true" aria-controls="pane-0">1. Perfil</button>
            <button type="button" id="tab-1" role="tab" class="tab-btn" aria-selected="false" tabindex="-1" aria-controls="pane-1">2. Servicios</button>
            <button type="button" id="tab-2" role="tab" class="tab-btn" aria-selected="false" tabindex="-1" aria-controls="pane-2">3. Clientes</button>
            <button type="button" id="tab-3" role="tab" class="tab-btn" aria-selected="false" tabindex="-1" aria-controls="pane-3">4. Contacto</button>
            <button type="button" id="tab-4" role="tab" class="tab-btn" aria-selected="false" tabindex="-1" aria-controls="pane-4">5. Estilo</button>
        </div>

        <div class="panel-body">
            <!-- PESTAÑA 1: PERFIL -->
            <div class="tab-pane active" id="pane-0" role="tabpanel" aria-labelledby="tab-0" tabindex="0">
                <fieldset>
                    <legend>Información Personal</legend>
                    <label>Nombre</label><input id="f-nombre" value="Lic. Rita Morales" readonly>
                    <label>Título profesional</label><input id="f-titulo">
                    <label>Registro profesional (opcional)</label><input id="f-registro">
                    <label>Frase principal (Hero)</label><input id="f-titular">
                    <label>Presentación breve / Bio</label><textarea id="f-bio" rows="4"></textarea>
                    
                    <label>Foto: URL de Cloudinary o Web</label>
                    <input type="text" id="f-fotourl" placeholder="https://res.cloudinary.com/.../tu-foto.jpg">
                    <label class="sep-sm">O subir desde tu equipo:</label>
                    <input type="file" id="f-foto-file" accept="image/*">
                    <button class="btn sec" id="sinfoto" type="button">Quitar foto</button>
                </fieldset>
            </div>

            <!-- PESTAÑA 2: SERVICIOS Y ENFOQUE -->
            <div class="tab-pane" id="pane-1" role="tabpanel" aria-labelledby="tab-1" tabindex="0" hidden>
                <fieldset>
                    <legend>Metodología y Oferta</legend>
                    <label>Enfoque metodológico</label><textarea id="f-enfoque" rows="3"></textarea>
                    <label>A quién está dirigido (un ítem por línea)</label><textarea id="f-publico" rows="3"></textarea>
                    <label>Servicios principales (hasta 4)</label>
                    <div id="svs"></div>
                </fieldset>
            </div>

            <!-- PESTAÑA 3: CLIENTES -->
            <div class="tab-pane" id="pane-2" role="tabpanel" aria-labelledby="tab-2" tabindex="0" hidden>
                <fieldset>
                    <legend>Empresas y Aliados</legend>
                    <label>Nombres de empresas separados por comas</label>
                    <textarea id="f-clientes" rows="4" placeholder="Empresa Alfa, Logística Sur, Corporación Delta"></textarea>
                    <p class="ayuda">Ideal para perfiles corporativos o profesionales que desean reforzar su credibilidad mediante referencias.</p>
                </fieldset>
                <fieldset>
                    <legend>Logos de clientes</legend>
                    <p class="ayuda">Aparecen en el brochure junto a los nombres (hasta 8). Funcionan mejor en PNG con fondo transparente o blanco.</p>
                    <label>Subir logos desde tu equipo (podés elegir varios)</label>
                    <input type="file" id="f-logos-file" accept="image/*" multiple>
                    <label for="f-logo-url">O agregar por URL</label>
                    <div class="fila-url">
                        <input type="url" id="f-logo-url" placeholder="https://res.cloudinary.com/.../logo.png">
                        <button type="button" class="btn sec" id="btn-logo-url">Agregar</button>
                    </div>
                    <p class="ayuda" id="logos-msg" role="status" aria-live="polite"></p>
                    <div class="logos-lista" id="logos-lista"></div>
                </fieldset>
            </div>

            <!-- PESTAÑA 4: CONTACTO Y FISCAL -->
            <div class="tab-pane" id="pane-3" role="tabpanel" aria-labelledby="tab-3" tabindex="0" hidden>
                <fieldset>
                    <legend>Logística y Contacto</legend>
                    <label>Modalidad de atención</label><input id="f-modalidad">
                    <label>Horarios / Disponibilidad</label><input id="f-horarios">
                    <label>Primera consulta / Reunión</label><textarea id="f-consulta" rows="2"></textarea>
                    <label>Teléfono o WhatsApp</label><input id="f-tel" value="+598 99140274" readonly>
                    <label>Correo electrónico</label><input id="f-email" value="licritamorales@gmail.com" readonly>
                    <label>Web o Redes</label><input id="f-web">
                    <label>Ubicación</label><input id="f-lugar">
                </fieldset>
                <fieldset>
                    <legend>Legal y Fiscal</legend>
                    <label class="chk"><input type="checkbox" id="f-conf" checked> Mostrar nota de confidencialidad</label>
                    <label class="chk"><input type="checkbox" id="f-fiscal"> Mostrar datos fiscales</label>
                    <label>Razón social</label><input id="f-razon">
                    <label>RUT</label><input id="f-rut">
                </fieldset>
            </div>

            <!-- PESTAÑA 5: ESTILO Y DISEÑO -->
            <div class="tab-pane" id="pane-4" role="tabpanel" aria-labelledby="tab-4" tabindex="0" hidden>
                <fieldset>
                    <legend>Apariencia Visual y Plantillas</legend>
                    <label>Plantilla de diseño</label>
                    <select id="f-plantilla">
                        <option value="t1">Clínica: Lateral con foto y tono cálido</option>
                        <option value="t2">Organizacional: Portada ejecutiva y área de clientes</option>
                        <option value="t3">Minimal: Filas limpias y aireado</option>
                        <option value="t4">Ejecutivo Corporativo: Azul marino y oro</option>
                        <option value="t5">Corporativo Moderno: Gris pizarra y estructura tech</option>
                        <option value="t6">Bienestar Institucional: Verde bosque y tonos neutros</option>
                    </select>

                    <label class="sep-md">Color de acento principal</label>
                    <input type="color" id="f-acc" value="#3f6f68" class="input-color">
                </fieldset>
            </div>
        </div>

        <div class="panel-footer">
            <div id="aviso" role="status" aria-live="polite">El brochure supera las tres hojas A4 recomendadas. Considerá acortar algunos textos.</div>
            <button type="button" class="btn" id="btn-imprimir">Guardar como PDF / Imprimir</button>
        </div>
    </section>

    <section class="vista" aria-label="Vista previa del brochure">
        <div class="stage" id="stage">
            <div id="hoja"></div>
        </div>
    </section>
</main>

<!-- Enlace al archivo JavaScript externo -->
<script src="js/main.js"></script>
</body>
</html>
```

### Archivo: `js/main.js`
```javascript
const $ = id => document.getElementById(id);
const esc = s => String(s ?? '').replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const nl = s => esc(s).replace(/\n/g, '<br>');
const F = ['nombre', 'titulo', 'registro', 'titular', 'bio', 'enfoque', 'publico', 'modalidad', 'horarios', 'consulta', 'tel', 'email', 'web', 'lugar', 'razon', 'rut', 'fotourl', 'clientes'];
const CONTACTO = { nombre: 'Lic. Rita Morales', tel: '+598 99140274', email: 'licritamorales@gmail.com' };
const FOTO_PERFIL = 'https://res.cloudinary.com/p0qlmlor/image/upload/v1791151072/brochures/logos/Perfil_Rita.webp';
const FOTOS_EJEMPLO = new Set([
    'https://images.unsplash.com/photo-1573496359142-b8d87734a5a2?auto=format&fit=crop&w=600&q=80',
    'https://images.unsplash.com/photo-1580489944761-15a19d654956?auto=format&fit=crop&w=600&q=80',
    'https://images.unsplash.com/photo-1573497019940-1c28c88b4f3e?auto=format&fit=crop&w=600&q=80',
    'https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=600&q=80'
]);

let fotoUrlCustom = '', accU = 0, logosCli = [];
const MAX_LOGOS = 8;
const LS = 'brochure_pro_v4';

// Sistema de pestañas del panel lateral
function switchTab(index) {
    document.querySelectorAll('.tab-btn').forEach((btn, i) => {
        btn.classList.toggle('active', i === index);
        btn.setAttribute('aria-selected', i === index ? 'true' : 'false');
        btn.tabIndex = i === index ? 0 : -1;
    });
    document.querySelectorAll('.tab-pane').forEach((pane, i) => {
        pane.classList.toggle('active', i === index);
        pane.hidden = i !== index;
    });
}

document.querySelectorAll('.tab-btn').forEach((btn, i) => {
    btn.addEventListener('click', () => switchTab(i));
    btn.addEventListener('keydown', e => {
        const n = document.querySelectorAll('.tab-btn').length;
        const dir = e.key === 'ArrowRight' ? 1 : e.key === 'ArrowLeft' ? -1 : 0;
        if (!dir) return;
        e.preventDefault();
        const j = (i + dir + n) % n;
        switchTab(j);
        document.querySelectorAll('.tab-btn')[j].focus();
    });
});

document.querySelectorAll('label:not(.chk)').forEach(l => {
    const n = l.nextElementSibling;
    if (n && /^(INPUT|SELECT|TEXTAREA)$/.test(n.tagName)) l.htmlFor = n.id;
});

$('svs').innerHTML = [0, 1, 2, 3].map(i => `
    <div style="margin-top:8px; display:flex; flex-direction:column; gap:4px;">
        <input id="st${i}" placeholder="Servicio ${i+1}" aria-label="Servicio ${i+1}: nombre">
        <textarea id="sd${i}" rows="1" placeholder="Descripción breve" aria-label="Servicio ${i+1}: descripción"></textarea>
    </div>
`).join('');

const PRE = {
    clinica: {
        pl: 't1',
        nombre: CONTACTO.nombre,
        titulo: 'Psicóloga clínica',
        registro: 'Registro profesional N.º 0000',
        titular: 'Un espacio seguro para entender lo que sentís y empezar a cambiar',
        bio: 'Acompaño a adultos y jóvenes en procesos de ansiedad, estrés, duelo y cambios vitales con escucha atenta y objetivos claros.',
        enfoque: 'Terapia cognitivo-conductual con herramientas de regulación emocional.',
        publico: 'Adultos y jóvenes desde 18 años\nPersonas que atraviesan duelos o cambios vitales',
        sv: [
            ['Psicoterapia individual', 'Sesiones semanales de 50 minutos, presenciales u online.'],
            ['Terapia de pareja', 'Espacio para ordenar la comunicación y acordar objetivos.'],
            ['Orientación vocacional', 'Acompañamiento para elegir camino de estudio o trabajo.']
        ],
        clientes: '',
        modalidad: 'Presencial y online',
        horarios: 'Lunes a viernes de 9 a 19 h',
        consulta: 'Primera entrevista de 50 minutos para conocer necesidades y definir objetivos.',
        tel: CONTACTO.tel,
        email: CONTACTO.email,
        web: '',
        lugar: 'Montevideo, Uruguay',
        fotourl: FOTO_PERFIL
    },
    org: {
        pl: 't4', // Por defecto ahora sugiere la plantilla ejecutiva t4 para perfiles organizacionales
        nombre: CONTACTO.nombre,
        titulo: 'Psicóloga laboral y organizacional',
        registro: '',
        titular: 'Psicología organizacional para equipos que quieren trabajar mejor',
        bio: 'Acompaño a organizaciones en selección de personas, clima laboral y prevención de riesgos psicosociales con informes claros.',
        enfoque: 'Diagnóstico con instrumentos validados y planes de acción con indicadores.',
        publico: 'Pymes y empresas medianas\nÁreas de recursos humanos\nLíderes de equipos',
        sv: [
            ['Selección y reclutamiento', 'Perfiles por competencias y evaluaciones psicolaborales.'],
            ['Clima y cultura laboral', 'Diagnóstico, devolución a dirección y planes de mejora.'],
            ['Riesgos psicosociales', 'Relevamiento, prevención y talleres para equipos.'],
            ['Capacitación a medida', 'Formación para mandos medios y liderazgo efectivo.']
        ],
        clientes: 'Empresa Alfa S.A., Logística del Sur, Corporación Delta, TechSolutions',
        modalidad: 'In-company y remoto',
        horarios: 'Coordinación previa',
        consulta: 'Reunión inicial de relevamiento sin cargo para definir alcance.',
        tel: CONTACTO.tel,
        email: CONTACTO.email,
        web: '',
        lugar: 'Montevideo, Uruguay',
        fotourl: FOTO_PERFIL
    },
    oec: {
        pl: 't4',
        nombre: CONTACTO.nombre,
        titulo: 'Especialista en OEC y Calidad',
        registro: '',
        titular: 'Estrategia, gestión y cumplimiento para organizaciones de alto desempeño',
        bio: 'Especialista en implementación de Programas de Operador Económico Calificado (OEC), comercio exterior y sistemas integrados.',
        enfoque: 'Auditoría de procesos y mejora continua bajo normas internacionales.',
        publico: 'Empresas exportadoras e importadoras\nOperadores logísticos\nCadenas de suministro',
        sv: [
            ['Implementación OEC', 'Diagnóstico, manuales de procesos y gestión de riesgos aduaneros.'],
            ['Auditorías Previas', 'Simulacros de auditoría y levantamiento de no conformidades.'],
            ['Gestión de Procesos', 'Mapeo de flujos, indicadores y optimización operativa.']
        ],
        clientes: 'Comercio Exterior S.A., Aduanas y Carga Ltda., Zona Franca Global',
        modalidad: 'Presencial en planta y auditoría remota',
        horarios: 'Lunes a viernes',
        consulta: 'Reunión técnica preliminar para evaluar estado de cumplimiento.',
        tel: CONTACTO.tel,
        email: CONTACTO.email,
        web: '',
        lugar: 'Montevideo, Uruguay',
        fotourl: FOTO_PERFIL
    },
    dev: {
        pl: 't5',
        nombre: CONTACTO.nombre,
        titulo: 'Ingeniera de Software & Cloud Architecture',
        registro: '',
        titular: 'Arquitectura de software escalable y desarrollo backend de alto rendimiento',
        bio: 'Diseño e implementación de aplicaciones robustas, APIs seguras y soluciones en la nube para proyectos tecnológicos complejos.',
        enfoque: 'Clean Code, arquitectura hexagonal, microservicios y despliegue continuo.',
        publico: 'Startups en crecimiento\nEmpresas de tecnología\nEquipos de desarrollo',
        sv: [
            ['Arquitectura de Software', 'Diseño de sistemas distribuidos y migración a la nube.'],
            ['Desarrollo Backend', 'APIs RESTful, optimización de bases de datos y seguridad.'],
            ['Code Review y Testing', 'Auditoría de código, refactorización y buenas prácticas.']
        ],
        clientes: 'Fintech Nova, Software House Uy, CloudNet Inc.',
        modalidad: 'Remoto internacional / Híbrido',
        horarios: 'Horario flexible',
        consulta: 'Evaluación técnica inicial de arquitectura y requerimientos.',
        tel: CONTACTO.tel,
        email: CONTACTO.email,
        web: '',
        lugar: 'Montevideo, Uruguay',
        fotourl: FOTO_PERFIL
    }
};

function cargarPreset(k) {
    const p = PRE[k];
    F.forEach(f => { if($('f-'+f))$('f-'+f).value = p[f] || ''; });
    for(let i=0; i<4; i++) {
        $('st'+i).value = p.sv[i] ? p.sv[i][0] : '';
        $('sd'+i).value = p.sv[i] ? p.sv[i][1] : '';
    }
    accU = 0;
    $('f-plantilla').value = p.pl;
    $('f-conf').checked = true;
    $('f-fiscal').checked = false;
    fotoUrlCustom = '';
    logosCli = [];
    panelLogos();
    render();
}

function leerDatos() {
    const d = {};
    F.forEach(k => { if($('f-'+k)) d[k] =$('f-'+k).value.trim(); });
    Object.entries(CONTACTO).forEach(([key, value]) => {
        $('f-' + key).value = value;
        d[key] = value;
    });
    d.conf = $('f-conf').checked;
    d.fiscal = $('f-fiscal').checked;
    d.pub = d.publico ? d.publico.split('\n').map(s => s.trim()).filter(Boolean) : [];
    d.clientesArr = d.clientes ? d.clientes.split(',').map(s => s.trim()).filter(Boolean) : [];
    d.sv = [0,1,2,3].map(i => ({t: $('st'+i).value.trim(), d:$('sd'+i).value.trim()})).filter(s => s.t);
    return d;
}

const ini = n => n.split(/\s+/).filter(w => w && !/\.$/.test(w)).slice(0,2).map(w => w[0]).join('').toUpperCase();
const S = (t, h) => h ? `<section><h2>${t}</h2>${h}</section>` : '';

function prepararRender(d) {
    const imgSrc = fotoUrlCustom || d.fotourl;
    const fotoHtml = imgSrc ? `<img src="${esc(imgSrc)}" alt="Foto de ${esc(d.nombre)}">` : esc(ini(d.nombre));
    
    const cont = [
        ['Teléfono', d.tel],
        ['Correo', d.email],
        ['Web / Redes', d.web],
        ['Ubicación', d.lugar]
    ].filter(x => x[1]).map(([l, v]) => `<div><span class="lab">${l}</span>${esc(v)}</div>`).join('');

    const tags = d.clientesArr.map(c => `<span class="cliente-tag">${esc(c)}</span>`).join('');
    const logosHtml = logosCli.length
        ? `<div class="logos-grid">${logosCli.map((s, i) => `<img src="${esc(s)}" alt="Logo de cliente ${i + 1}">`).join('')}</div>`
        : '';
    const hayCli = d.clientesArr.length > 0 || logosCli.length > 0;
    const cliInner = logosHtml + (d.clientesArr.length ? `<div class="clientes-grid">${tags}</div>` : '');
    let clientesHtml = '';
    if (hayCli) {
        clientesHtml = `<div class="clientes-bloque"><span class="lab">Clientes / Aliados Destacados</span>${cliInner}</div>`;
    }

    let fiscalTxt = '';
    if (d.fiscal && (d.razon || d.rut)) {
        fiscalTxt = `<div><span class="lab">Datos Fiscales</span>${esc(d.razon)}${d.rut ? '<br>RUT: '+esc(d.rut) : ''}</div>`;
    }

    return {
        foto: fotoHtml,
        cont: cont,
        fiscalTxt: fiscalTxt,
        serv: d.sv.map(s => `<div class="serv"><b>${esc(s.t)}</b>${esc(s.d)}</div>`).join(''),
        servCards: d.sv.map(s => `<div class="serv-card"><b>${esc(s.t)}</b>${esc(s.d)}</div>`).join(''),
        pub: d.pub.length ? `<ul>${d.pub.map(x => `<li>${esc(x)}</li>`).join('')}</ul>` : '',
        meta: [
            ['Modalidad', d.modalidad],
            ['Horarios', d.horarios],
            ['Primera consulta', d.consulta]
        ].filter(m => m[1]).map(([l, v]) => `<div><span class="lab">${l}</span>${esc(v)}</div>`).join(''),
        quien: `<b>${esc(d.nombre)}</b><br>${esc(d.titulo)}${d.registro ? `<span class="lab">${esc(d.registro)}</span>` : ''}`,
        clientesSec: clientesHtml,
        tags: tags,
        hayCli: hayCli,
        cliInner: cliInner,
        nota: d.conf ? '<p class="nota">Todo lo conversado en consulta es estrictamente confidencial y está amparado por el secreto profesional.</p>' : ''
    };
}

const Plantillas = {
    t1: (d, p) => `<aside class="lado"><div class="foto">${p.foto}</div><div class="cont">${p.cont}${p.fiscalTxt}</div></aside><div class="main"><h1>${esc(d.titular)}</h1><div class="quien">${p.quien}</div>${d.bio ? `<p>${nl(d.bio)}</p>` : ''}${S('Enfoque', d.enfoque && `<p>${nl(d.enfoque)}</p>`)}${S('Atiendo a', p.pub)}${S('Servicios', p.serv)}${p.clientesSec}<div class="meta">${p.meta}</div>${p.nota}</div>`,
    
    t2: (d, p) => `<div class="top"><div><h1>${esc(d.titular)}</h1><div class="quien">${p.quien}</div></div><div class="foto">${p.foto}</div></div><div class="cuerpo"><div>${d.bio ? `<p>${nl(d.bio)}</p>` : ''}${S('Enfoque', d.enfoque && `<p>${nl(d.enfoque)}</p>`)}${S('Dirigido a', p.pub)}${p.clientesSec}</div><div>${S('Servicios', p.serv)}${p.meta ? `<div class="meta">${p.meta}</div>` : ''}</div></div><div class="pie">${p.cont}${p.fiscalTxt}${p.nota}</div>`,
    
    t3: (d, p) => {
        const R = (l, h) => h ? `<div class="fila"><b>${l}</b><div>${h}</div></div>` : '';
        return `<div class="cab"><div><h1>${esc(d.nombre)}</h1><div class="quien">${esc(d.titulo)}${d.registro ? `<span class="lab">${esc(d.registro)}</span>` : ''}</div></div><div class="foto">${p.foto}</div></div>${d.titular ? `<p class="tit">${esc(d.titular)}</p>` : ''}${d.bio ? `<p>${nl(d.bio)}</p>` : ''}<div>${R('Enfoque', d.enfoque && nl(d.enfoque))}${R('Atiendo a', p.pub)}${d.sv.map(s => R(esc(s.t), esc(s.d))).join('')}${R('Modalidad', esc(d.modalidad))}${R('Horarios', esc(d.horarios))}${R('Primera consulta', esc(d.consulta))}${p.hayCli ? R('Clientes', p.cliInner) : ''}</div><div class="pie">${p.cont.replace(/<span class="lab">([^<]*)<\/span>/g,'<span class="lab" style="display:inline;margin-right:1.5mm">$1</span>')}${p.fiscalTxt}${p.nota}</div>`;
    },

    // Nueva t4: Ejecutivo Corporativo (Azul Marino y Oro)
    t4: (d, p) => `
        <div class="hero-exec">
            <div>
                <h1>${esc(d.titular)}</h1>
                <div class="quien">${p.quien}</div>
            </div>
            <div class="sello-exec">Perfil Ejecutivo</div>
        </div>
        <div class="cuerpo-exec">
            <div class="perfil-exec">
                <div>
                    <h2>Perfil Profesional</h2>
                    ${d.bio ? `<p>${nl(d.bio)}</p>` : ''}
                    ${d.enfoque ? `<p class="enfoque-exec"><b>Enfoque:</b> ${esc(d.enfoque)}</p>` : ''}
                    ${d.pub.length ? `<p class="enfoque-exec"><b>Dirigido a:</b> ${d.pub.map(esc).join(' · ')}</p>` : ''}
                </div>
                <div class="foto">${p.foto}</div>
            </div>
            ${p.hayCli ? `<div><h2>Clientes y Alianzas</h2>${p.cliInner}</div>` : ''}
            ${d.sv.length ? `<div>
                <h2>Nuestros Servicios</h2>
                <div class="servicios-grid">${p.servCards}</div>
            </div>` : ''}
            ${p.meta ? `<div class="meta-exec">${p.meta}</div>` : ''}
        </div>
        <div class="pie-exec">
            <div><b>Contacto</b>${[d.tel, d.email, d.lugar].filter(Boolean).map(v => `<div>${esc(v)}</div>`).join('')}</div>
            <div><b>Profesional</b>${[d.web, d.modalidad].filter(Boolean).map(v => `<div>${esc(v)}</div>`).join('')}</div>
            ${p.fiscalTxt ? `<div><b>Datos Fiscales</b><div>${esc(d.razon)}</div>${d.rut ? `<div>RUT: ${esc(d.rut)}</div>` : ''}</div>` : ''}
            ${p.nota}
        </div>
    `,

    // Nueva t5: Corporativo Moderno (Gris Pizarra y Tech)
    t5: (d, p) => `
        <div class="top-modern">
            <div>
                <h1>${esc(d.titular)}</h1>
                <div class="quien">${p.quien}</div>
            </div>
            <div class="foto">${p.foto}</div>
        </div>
        <div class="cuerpo-modern">
            <div>
                ${S('Presentación', d.bio && `<p>${nl(d.bio)}</p>`)}
                ${S('Metodología', d.enfoque && `<p>${nl(d.enfoque)}</p>`)}
                ${S('Dirigido a', p.pub)}
                ${p.hayCli ? `<section><h2>Aliados</h2>${p.cliInner}</section>` : ''}
            </div>
            <div>
                ${S('Servicios', p.serv)}
                ${S('Disponibilidad', p.meta)}
            </div>
        </div>
        <div class="pie-modern">
            <div>${[d.tel, d.email, d.lugar].filter(Boolean).map(esc).join(' | ')}</div>
            ${p.fiscalTxt ? `<div>${esc(d.razon)}${d.rut ? ' | RUT: ' + esc(d.rut) : ''}</div>` : ''}
            ${p.nota}
        </div>
    `,

    // Nueva t6: Bienestar Institucional (Verde Bosque)
    t6: (d, p) => `
        <div class="header-bio">
            <div class="foto">${p.foto}</div>
            <div>
                <h1>${esc(d.titular)}</h1>
                <div class="quien">${p.quien}</div>
            </div>
        </div>
        <div class="cuerpo-bio">
            <div>
                ${S('Enfoque y Perfil', (d.bio ? `<p>${nl(d.bio)}</p>` : '') + (d.enfoque ? `<p class="aparte"><b>Metodología:</b> ${esc(d.enfoque)}</p>` : ''))}
                ${S('Dirigido a', p.pub)}
                ${p.hayCli ? `<section><h2>Organizaciones</h2>${p.cliInner}</section>` : ''}
            </div>
            <div>
                ${S('Servicios Ofrecidos', p.serv)}
                ${S('Atención', p.meta)}
            </div>
        </div>
        <div class="pie-bio">
            <div><b>Contacto:</b> ${[d.tel, d.email, d.lugar].filter(Boolean).map(esc).join(' | ')}</div>
            ${p.fiscalTxt ? `<div>${esc(d.razon)}${d.rut ? ' | RUT: ' + esc(d.rut) : ''}</div>` : ''}
            ${p.nota}
        </div>
    `
};

function ajustarEscala() {
    const vista = document.querySelector('.vista');
    const wA4 = 210 * 3.7795;
    const altoHoja = $('hoja').scrollHeight;
    const escala = Math.min(1.5, (vista.clientWidth - (innerWidth < 1000 ? 24 : 64)) / wA4);
    $('hoja').style.transform = `scale(${escala})`;
    $('stage').style.width = (wA4 * escala) + 'px';
    $('stage').style.height = (altoHoja * escala) + 'px';
}

function guardar() {
    try {
        const est = {};
        F.forEach(f => { if ($('f-' + f)) est[f] = $('f-' + f).value; });
        est.sv = [0,1,2,3].map(i => [$('st'+i).value, $('sd'+i).value]);
        est.pl = $('f-plantilla').value;
        est.conf = $('f-conf').checked;
        est.fiscal = $('f-fiscal').checked;
        est.acc = accU ? $('f-acc').value : '';
        est.foto = fotoUrlCustom;
        est.logos = logosCli;
        localStorage.setItem(LS, JSON.stringify(est));
    } catch (e) { /* almacenamiento lleno o bloqueado: se ignora */ }
}

function restaurar() {
    try {
        const est = JSON.parse(localStorage.getItem(LS) || 'null');
        if (!est) return false;
        F.forEach(f => { if ($('f-' + f)) $('f-' + f).value = est[f] || ''; });
        if (FOTOS_EJEMPLO.has($('f-fotourl').value)) $('f-fotourl').value = FOTO_PERFIL;
        for (let i = 0; i < 4; i++) {
            $('st'+i).value = est.sv?.[i]?.[0] || '';
            $('sd'+i).value = est.sv?.[i]?.[1] || '';
        }
        if (Plantillas[est.pl]) $('f-plantilla').value = est.pl;
        $('f-conf').checked = !!est.conf;
        $('f-fiscal').checked = !!est.fiscal;
        fotoUrlCustom = est.foto || '';
        if (est.acc) { $('f-acc').value = est.acc; accU = 1; }
        logosCli = Array.isArray(est.logos) ? est.logos.filter(s => typeof s === 'string').slice(0, MAX_LOGOS) : [];
        panelLogos();
        render();
        return true;
    } catch (e) { return false; }
}

function render() {
    const d = leerDatos();
    const hoja = $('hoja');
    const plantillaId = $('f-plantilla').value;
    
    hoja.className = plantillaId;
    hoja.innerHTML = Plantillas[plantillaId](d, prepararRender(d));
    document.title = 'Brochure - ' + d.nombre;

    if (accU) {
        hoja.style.setProperty('--acc', $('f-acc').value);
    } else {
        hoja.style.removeProperty('--acc');
        const colorCalculado = getComputedStyle(hoja).getPropertyValue('--acc').trim();
        if (/^#[0-9a-f]{6}$/i.test(colorCalculado)) $('f-acc').value = colorCalculado;
    }
    const accent = getComputedStyle(hoja).getPropertyValue('--acc').trim();
    const rgb = accent.match(/^#([\da-f]{6})$/i)?.[1];
    if (rgb) {
        const channels = [0, 2, 4].map(offset => parseInt(rgb.slice(offset, offset + 2), 16) / 255);
        const luminance = channels.map(channel => channel <= 0.04045 ? channel / 12.92 : ((channel + 0.055) / 1.055) ** 2.4)
            .reduce((sum, channel, index) => sum + channel * [0.2126, 0.7152, 0.0722][index], 0);
        hoja.style.setProperty('--on-acc', luminance > 0.179 ? '#111827' : '#ffffff');
    }

    const paginas = Math.ceil(hoja.scrollHeight / (297 * 3.7795));
    $('aviso').style.display = paginas > 3 ? 'block' : 'none';
    ajustarEscala();
    guardar();
}

// Manejo de imagen por archivo local
$('f-foto-file').onchange = e => {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = ev => {
        const img = new Image();
        img.onload = () => {
            const max = 700;
            const scale = Math.min(1, max / Math.max(img.width, img.height));
            const canvas = document.createElement('canvas');
            canvas.width = img.width * scale;
            canvas.height = img.height * scale;
            canvas.getContext('2d').drawImage(img, 0, 0, canvas.width, canvas.height);
            fotoUrlCustom = canvas.toDataURL('image/jpeg', 0.88);
            $('f-fotourl').value = '';
            render();
        };
        img.src = ev.target.result;
    };
    reader.readAsDataURL(file);
};

$('sinfoto').onclick = () => {
    fotoUrlCustom = '';
    $('f-fotourl').value = '';$('f-foto-file').value = '';
    render();
};

// ---- Logos de clientes ----
function mensajeLogos(t) { $('logos-msg').textContent = t || ''; }

function panelLogos() {
    $('logos-lista').innerHTML = logosCli.map((s, i) => `
        <div class="logo-item"><img src="${esc(s)}" alt="Logo ${i + 1}"><button type="button" data-i="${i}" aria-label="Quitar logo ${i + 1}">×</button></div>`).join('');
}

function agregarLogo(src) {
    if (logosCli.length >= MAX_LOGOS) { mensajeLogos(`Máximo ${MAX_LOGOS} logos. Quitá alguno para agregar otro.`); return false; }
    logosCli.push(src);
    mensajeLogos('');
    panelLogos();
    render();
    return true;
}

function logoDesdeArchivo(file) {
    return new Promise((ok, no) => {
        const r = new FileReader();
        r.onerror = no;
        r.onload = ev => {
            const img = new Image();
            img.onerror = no;
            img.onload = () => {
                const w0 = img.naturalWidth || 300, h0 = img.naturalHeight || 150;
                const k = Math.min(1, 400 / Math.max(w0, h0));
                const c = document.createElement('canvas');
                c.width = Math.max(1, Math.round(w0 * k));
                c.height = Math.max(1, Math.round(h0 * k));
                c.getContext('2d').drawImage(img, 0, 0, c.width, c.height);
                ok(c.toDataURL(file.type === 'image/jpeg' ? 'image/jpeg' : 'image/png', 0.9));
            };
            img.src = ev.target.result;
        };
        r.readAsDataURL(file);
    });
}

$('f-logos-file').onchange = async e => {
    const archivos = [...e.target.files];
    e.target.value = '';
    let fallos = 0;
    for (const f of archivos) {
        if (logosCli.length >= MAX_LOGOS) { mensajeLogos(`Máximo ${MAX_LOGOS} logos. Se omitieron los restantes.`); return; }
        try { agregarLogo(await logoDesdeArchivo(f)); } catch (err) { fallos++; }
    }
    if (fallos) mensajeLogos(`No se pudo leer ${fallos} archivo(s). Probá con PNG, JPG o SVG.`);
};

$('btn-logo-url').onclick = () => {
    const u = $('f-logo-url').value.trim();
    if (!/^https?:\/\/\S+$/i.test(u)) { mensajeLogos('Ingresá una URL que empiece con http:// o https://'); return; }
    if (agregarLogo(u)) $('f-logo-url').value = '';
};
$('f-logo-url').addEventListener('keydown', e => { if (e.key === 'Enter') { e.preventDefault(); $('btn-logo-url').click(); } });

$('logos-lista').addEventListener('click', e => {
    const b = e.target.closest('button[data-i]');
    if (!b) return;
    logosCli.splice(+b.dataset.i, 1);
    mensajeLogos('');
    panelLogos();
    render();
});

$('btn-imprimir').onclick = () => print();
$('preset').onchange = e => cargarPreset(e.target.value);
document.querySelector('.panel').addEventListener('input', render);
addEventListener('resize', ajustarEscala);
$('f-acc').addEventListener('input', () => { accU = 1; });$('f-plantilla').addEventListener('change', () => { accU = 0; render(); });

// Inicializar: retoma el trabajo guardado o carga Psicología Clínica
if (!restaurar()) cargarPreset('clinica');
```

