# Bundle del Proyecto para IA: juegomaletines

## Estructura de Archivos
- README.md
- dinamicas.css
- dinamicas.html
- dinamicas.js
- documentacion/LEEME.md
- index.html
- juego.html
- script.js
- styles.css

---

## Código Fuente

### Archivo: `README.md`
```md
# Simulador de Maletines

Simulador del juego de los maletines ("Trato o No Trato"), basado en el concepto de valor esperado. Tiene dos usos: jugar la partida clásica, o usarlo como base para dinámicas con personas o grupos (por ejemplo, en talleres o evaluaciones).

Sitio publicado: https://juegomaletines.netlify.app

## Funcionalidades

### Jugar (`juego.html`)

- Partida completa: elegís un maletín, lo abrís en rondas y negociás con la banca.
- Panel con el **valor esperado (VE)** y la **oferta estimada de la banca** en cada ronda.
- Modos de visualización: **Básico**, **TV** y **Seguimiento**. Seguimiento sirve para anotar en vivo un programa de televisión: se marca el maletín del participante, se registran las ofertas reales y se puede deshacer.
- **Perfil de la banca**: Normal, Tacaña o Generosa.
- Modo educativo y perfil de riesgo.
- Decisión final: quedarse con el maletín propio o cambiarlo.

### Dinámicas (`dinamicas.html`)

Ejercicios individuales y grupales basados en el mismo modelo:

- **Perfil de decisión (estandarizada)**: prueba individual con las mismas condiciones para todos. Al terminar genera un informe técnico de evaluación conductual, que se puede imprimir.
- **Consenso cronometrado**: el grupo decide en conjunto con tiempo límite. Se puede registrar quién impulsó cada decisión y al final sale un informe grupal.
- **Equipos enfrentados**.
- **Consistencia (test-retest)**.

## Cómo se usa

1. Abrí `index.html` y elegí **Jugar** o **Dinámicas**.
2. En las dinámicas, iniciá la prueba y seguí las decisiones de la persona o del grupo.
3. Al final, abrí el informe con **Ver Informe Profesional** e imprimilo si hace falta.

En [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/juegomaletines/documentacion) están el manual de usuario ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/juegomaletines/documentacion/MANUAL_USUARIO.pdf)) y una [presentación del proyecto](https://github.com/FABIOR1981/documentacion-central/blob/main/juegomaletines/documentacion/presentacion.pdf). También se pueden consultar desde la bitácora de proyectos.

## Ejecutar localmente

No necesita instalación. Abrí `index.html` en el navegador.

## Estructura

```
index.html                    Menú de inicio
juego.html / script.js        Partida clásica
dinamicas.html / dinamicas.js Dinámicas e informes
styles.css / dinamicas.css    Estilos
documentacion/                Aviso: la documentación está en documentacion-central
```

```

### Archivo: `dinamicas.css`
```css
/* ==========================================================================
   Dinámicas — Estilos independientes con la estética original del juego principal
   ========================================================================== */

:root {
  --bg-dark: #0b0f19;
  --card-bg: #151c2d;
  --accent-blue: #00f0ff;
  --accent-purple: #7000ff;
  --gold: #ffd700;
  --green: #00ff88;
  --red-val: #ff2a6d;
  --text-main: #f8fafc;
  --text-muted: #94a3b8;
  --border-color: #1e293b;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* Regla crítica para garantizar el ocultamiento de elementos */
.overlay[hidden],
[hidden] {
  display: none !important;
}

body {
  background-color: var(--bg-dark);
  color: var(--text-main);
  font-family: 'Rajdhani', sans-serif;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

/* ---------------- Navegación superior ---------------- */

.top-nav {
  width: 100%;
  max-width: 1100px;
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.top-nav a {
  color: var(--text-muted);
  text-decoration: none;
  font-weight: 600;
  font-size: 0.9rem;
  transition: color 0.2s;
}

.top-nav a:hover {
  color: var(--accent-blue);
}

h1 {
  font-family: 'Orbitron', sans-serif;
  font-size: 2rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  background: linear-gradient(135deg, var(--accent-blue), var(--accent-purple));
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  margin-bottom: 5px;
  text-align: center;
}

.subtitle {
  color: var(--text-muted);
  font-size: 1rem;
  text-align: center;
  max-width: 600px;
  margin-bottom: 30px;
}

/* ---------------- Control de Pantallas ---------------- */

.screen {
  display: none;
  width: 100%;
  max-width: 1100px;
  animation: fadeIn 0.3s ease-out;
}

.screen.active {
  display: block;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* ---------------- Menú de Dinámicas (Grid) ---------------- */

.dyn-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

.dyn-card {
  background: var(--card-bg);
  border: 1px solid var(--border-color);
  border-radius: 12px;
  padding: 24px;
  cursor: pointer;
  transition: transform 0.2s, border-color 0.2s, box-shadow 0.2s;
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.dyn-card:not(.disabled):hover {
  transform: translateY(-4px);
  border-color: var(--accent-blue);
  box-shadow: 0 8px 24px rgba(0, 240, 255, 0.15);
}

.dyn-card.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.dyn-tag {
  align-self: flex-start;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.dyn-tag.individual {
  background: rgba(0, 240, 255, 0.1);
  color: var(--accent-blue);
  border: 1px solid rgba(0, 240, 255, 0.3);
}

.dyn-tag.grupal {
  background: rgba(112, 0, 255, 0.15);
  color: #b87dff;
  border: 1px solid rgba(112, 0, 255, 0.3);
}

.dyn-card h3 {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.1rem;
}

.dyn-card p {
  color: var(--text-muted);
  font-size: 0.9rem;
  line-height: 1.4;
}

.soon {
  font-size: 0.75rem;
  color: var(--gold);
  font-weight: 700;
  text-transform: uppercase;
}

/* ---------------- Cajas de Intro e Inputs ---------------- */

.intro-box {
  background: var(--card-bg);
  border: 1px solid var(--border-color);
  border-radius: 12px;
  padding: 30px;
  max-width: 600px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.intro-box h2 {
  font-family: 'Orbitron', sans-serif;
  color: var(--accent-blue);
  font-size: 1.3rem;
}

.intro-box .note {
  color: var(--text-muted);
  font-size: 0.95rem;
  line-height: 1.5;
}

.field-row {
  display: flex;
  gap: 10px;
}

input[type="text"] {
  flex: 1;
  background: #0f172a;
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 10px 14px;
  color: var(--text-main);
  font-family: inherit;
  font-size: 1rem;
}

input[type="text"]:focus {
  outline: none;
  border-color: var(--accent-blue);
}

/* ---------------- Dashboard y Métricas ---------------- */

.dashboard {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 12px;
  margin-bottom: 16px;
}

.metric-card {
  background: var(--card-bg);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 12px;
  text-align: center;
}

.metric-card h3 {
  font-size: 0.75rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 4px;
}

.metric-card .value {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.2rem;
  font-weight: 700;
}

.metric-card .value.offer {
  color: var(--gold);
}

.status-bar {
  background: rgba(15, 23, 42, 0.8);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 10px 16px;
  text-align: center;
  font-size: 1rem;
  font-weight: 600;
  margin-bottom: 16px;
  color: var(--accent-blue);
}

.player-case-bar {
  background: rgba(255, 215, 0, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 8px;
  padding: 8px 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-bottom: 16px;
  font-size: 0.9rem;
}

.player-case-bar .num {
  font-family: 'Orbitron', sans-serif;
  font-weight: 700;
  color: var(--gold);
}

.player-case-bar .hint {
  color: var(--text-muted);
  font-size: 0.8rem;
}

/* ---------------- Tablero Principal (Estilo Clásico del Juego) ---------------- */

.main-container {
  display: grid;
  grid-template-columns: 140px 1fr 140px;
  gap: 16px;
  margin-bottom: 20px;
}

.values-column {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.value-badge {
  background: var(--card-bg);
  border: 1px solid var(--border-color);
  border-radius: 4px;
  padding: 4px 8px;
  font-size: 0.8rem;
  font-weight: 700;
  text-align: center;
  transition: opacity 0.3s, filter 0.3s;
}

.value-badge.low { color: #38bdf8; }
.value-badge.high { color: #f43f5e; }

.value-badge.eliminated {
  opacity: 0.2;
  filter: grayscale(1);
  text-decoration: line-through;
}

.cases-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(70px, 1fr));
  gap: 10px;
  align-content: start;
}

.case {
  background: var(--card-bg);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  aspect-ratio: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  cursor: default;
  position: relative;
  transition: transform 0.15s, border-color 0.15s, background-color 0.15s;
  user-select: none;
}

.case.selectable {
  cursor: pointer;
  border-color: rgba(0, 240, 255, 0.4);
}

.case.selectable:hover {
  transform: scale(1.05);
  border-color: var(--accent-blue);
  box-shadow: 0 0 12px rgba(0, 240, 255, 0.2);
}

.case .num {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.1rem;
  font-weight: 700;
}

.case .val {
  font-size: 0.75rem;
  font-weight: 700;
  color: var(--gold);
  margin-top: 2px;
}

.case.opened {
  background: rgba(15, 23, 42, 0.6);
  border-color: transparent;
  opacity: 0.5;
}

.case.player {
  border-color: var(--gold);
  background: rgba(255, 215, 0, 0.1);
}

.case .mine-tag {
  position: absolute;
  top: 2px;
  font-size: 0.55rem;
  font-weight: 900;
  color: var(--gold);
  letter-spacing: 0.5px;
}

/* ---------------- Botones y Controles ---------------- */

.controls {
  display: flex;
  justify-content: center;
  margin-top: 10px;
}

.btn {
  background: var(--card-bg);
  border: 1px solid var(--border-color);
  color: var(--text-main);
  padding: 10px 20px;
  border-radius: 8px;
  font-family: 'Orbitron', sans-serif;
  font-size: 0.85rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.btn:hover {
  border-color: var(--accent-blue);
  color: var(--accent-blue);
}

.btn-deal {
  background: var(--green);
  color: #000;
  border: none;
}
.btn-deal:hover {
  background: #00cc6a;
  color: #000;
  box-shadow: 0 0 16px rgba(0, 255, 136, 0.4);
}

.btn-nodeal {
  background: var(--red-val);
  color: #fff;
  border: none;
}
.btn-nodeal:hover {
  background: #d61f55;
  color: #fff;
  box-shadow: 0 0 16px rgba(255, 42, 109, 0.4);
}

.btn-keep {
  background: var(--card-bg);
  border: 1px solid var(--accent-blue);
  color: var(--accent-blue);
}

.btn-swap {
  background: var(--card-bg);
  border: 1px solid var(--gold);
  color: var(--gold);
}

.btn-ghost {
  background: transparent;
  border: 1px solid var(--border-color);
  color: var(--text-muted);
}

.reset-btn {
  background: transparent;
  border: none;
  color: var(--text-muted);
  font-size: 0.8rem;
  cursor: pointer;
  text-decoration: underline;
}

/* ---------------- Overlays y Modales ---------------- */

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(11, 15, 25, 0.85);
  backdrop-filter: blur(6px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
  padding: 20px;
}

.panel {
  background: var(--card-bg);
  border: 1px solid var(--border-color);
  border-radius: 16px;
  padding: 30px;
  width: 100%;
  max-width: 480px;
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 16px;
  box-shadow: 0 20px 50px rgba(0,0,0,0.5);
  animation: popIn 0.25s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.panel.wide {
  max-width: 680px;
}

@keyframes popIn {
  from { transform: scale(0.9); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

.panel h2 {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.3rem;
}

.big-amount {
  font-family: 'Orbitron', sans-serif;
  font-size: 2.2rem;
  font-weight: 900;
  color: var(--gold);
}

.panel-sub {
  color: var(--text-muted);
  font-size: 0.9rem;
  line-height: 1.4;
}

.panel-actions {
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
}

/* ---------------- Componentes de Resultados e Historial ---------------- */

.risk-profile {
  background: rgba(0, 0, 0, 0.25);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  padding: 12px;
  text-align: left;
}

.risk-tag {
  display: inline-block;
  font-family: 'Orbitron', sans-serif;
  font-size: 0.75rem;
  color: var(--gold);
  text-transform: uppercase;
  margin-bottom: 4px;
}

.result-history {
  display: flex;
  flex-direction: column;
  gap: 6px;
  max-height: 160px;
  overflow-y: auto;
  text-align: left;
  background: rgba(0,0,0,0.2);
  padding: 10px;
  border-radius: 8px;
}

.result-history h3 {
  font-size: 0.75rem;
  color: var(--text-muted);
  text-transform: uppercase;
  margin-bottom: 4px;
}

.hist-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.8rem;
  border-bottom: 1px solid rgba(255,255,255,0.05);
  padding-bottom: 4px;
}

.hist-row .hist-offer { color: var(--gold); }
.hist-row .hist-dec.deal { color: var(--green); font-weight: 700; }
.hist-row .hist-dec.nodeal { color: var(--red-val); }

/* Cronómetro grupal */
.countdown-ring {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.8rem;
  font-weight: 700;
  color: var(--accent-blue);
}

.countdown-ring.low {
  color: var(--red-val);
  animation: pulse 0.5s infinite alternate;
}

@keyframes pulse {
  from { transform: scale(1); }
  to { transform: scale(1.1); }
}

/* Edición de participantes grupales */
.participant-list {
  display: flex;
  flex-direction: column;
  gap: 6px;
  max-height: 120px;
  overflow-y: auto;
}

.participant-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(0,0,0,0.2);
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 0.9rem;
}

.participant-row button {
  background: transparent;
  border: none;
  color: var(--red-val);
  cursor: pointer;
  font-size: 0.75rem;
}

.driver-select {
  display: flex;
  flex-direction: column;
  gap: 4px;
  text-align: left;
}

.driver-select label {
  font-size: 0.75rem;
  color: var(--text-muted);
}

.driver-select select {
  background: #0f172a;
  border: 1px solid var(--border-color);
  color: var(--text-main);
  padding: 8px;
  border-radius: 6px;
  font-family: inherit;
}

/* ---------------- Contenedores de Informe Profesional ---------------- */

.pro-report-container {
  margin-top: 16px;
  background: rgba(15, 23, 42, 0.95);
  border: 1px solid rgba(255, 215, 0, 0.35);
  border-radius: 12px;
  padding: 16px;
  text-align: left;
}

.pro-report-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid rgba(255, 215, 0, 0.2);
  padding-bottom: 8px;
  margin-bottom: 12px;
}

.pro-report-header h3 {
  font-family: 'Orbitron', sans-serif;
  font-size: 0.82rem;
  color: var(--gold);
  text-transform: uppercase;
  letter-spacing: 1px;
  margin: 0;
}

.pro-report-body {
  max-height: 280px;
  overflow-y: auto;
  padding-right: 6px;
}

.pro-report-body::-webkit-scrollbar {
  width: 6px;
}
.pro-report-body::-webkit-scrollbar-track {
  background: rgba(15, 23, 42, 0.5);
}
.pro-report-body::-webkit-scrollbar-thumb {
  background: rgba(255, 215, 0, 0.3);
  border-radius: 3px;
}

/* ---------------- Reglas de Impresión Exclusiva ---------------- */

@media print {
  body * {
    visibility: hidden;
  }

  body.printing-pro-report #ind-pro-report,
  body.printing-pro-report #ind-pro-report *,
  body.printing-grp-report #grp-pro-report,
  body.printing-grp-report #grp-pro-report * {
    visibility: visible;
  }

  body.printing-pro-report #ind-pro-report,
  body.printing-grp-report #grp-pro-report {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    margin: 0;
    padding: 20px;
    background: #fff !important;
    color: #111 !important;
    border: none !important;
    box-shadow: none !important;
  }

  body.printing-pro-report .pro-report-body,
  body.printing-grp-report .pro-report-body {
    max-height: none !important;
    overflow: visible !important;
    color: #111 !important;
  }

  body.printing-pro-report .pro-report-header h3,
  body.printing-grp-report .pro-report-header h3,
  body.printing-pro-report strong,
  body.printing-grp-report strong,
  body.printing-pro-report td,
  body.printing-grp-report td,
  body.printing-pro-report th,
  body.printing-grp-report th {
    color: #111 !important;
  }

  body.printing-pro-report table,
  body.printing-grp-report table {
    background: #fff !important;
    border: 1px solid #ddd !important;
  }

  body.printing-pro-report th,
  body.printing-grp-report th {
    background: #f1f5f9 !important;
  }

  body.printing-pro-report tr,
  body.printing-grp-report tr {
    border-bottom: 1px solid #ddd !important;
  }

  body.printing-pro-report .btn,
  body.printing-grp-report .btn {
    display: none !important;
  }
}
```

### Archivo: `dinamicas.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dinámicas - Simulador de Maletines</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@500;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="dinamicas.css">
</head>
<body>

  <div class="top-nav">
    <a href="index.html">‹ Menú principal</a>
    <a href="juego.html">Ir al juego</a>
  </div>

  <h1>Dinámicas</h1>
  <p class="subtitle">Ejercicios basados en el mismo modelo de decisión bajo incertidumbre, pensados para usar con personas o grupos.</p>

  <!-- ============================ Selector ============================ -->
  <div class="screen active" id="screen-menu">
    <div class="dyn-grid">

      <div class="dyn-card" data-target="individual">
        <span class="dyn-tag individual">Individual</span>
        <h3>Perfil de decisión (estandarizada)</h3>
        <p>Mismo tablero para cada persona (no se mezcla al azar), para poder comparar entre candidatos. Mide perfil de riesgo y tiempo de deliberación en cada oferta.</p>
      </div>

      <div class="dyn-card" data-target="group">
        <span class="dyn-tag grupal">Grupal</span>
        <h3>Consenso cronometrado</h3>
        <p>El grupo debate y decide en equipo bajo presión de tiempo. Se registra quién impulsó cada decisión, para observar liderazgo y participación.</p>
      </div>

      <div class="dyn-card disabled">
        <span class="dyn-tag grupal">Grupal</span>
        <h3>Equipos enfrentados</h3>
        <p>Dos o más equipos compiten con tableros propios; gana quien deja menos valor sobre la mesa.</p>
        <span class="soon">Próximamente</span>
      </div>

      <div class="dyn-card disabled">
        <span class="dyn-tag individual">Individual</span>
        <h3>Consistencia (test-retest)</h3>
        <p>La misma persona repite el ejercicio en dos momentos distintos para observar estabilidad en su perfil de riesgo.</p>
        <span class="soon">Próximamente</span>
      </div>

    </div>
  </div>

  <!-- ======================= Individual: intro ======================= -->
  <div class="screen" id="screen-individual-intro">
    <div class="intro-box">
      <h2>Perfil de decisión — versión estandarizada</h2>
      <p class="note">
        El tablero es siempre el mismo (mismos premios en los mismos maletines), para que el resultado
        se pueda comparar entre personas. Se registra qué decisión tomó en cada oferta y cuánto tardó en tomarla.
      </p>
      <div class="field-row">
        <input type="text" id="ind-name-input" placeholder="Nombre del participante (opcional)">
      </div>
      <div class="panel-actions">
        <button class="btn btn-ghost" data-back>‹ Volver</button>
        <button class="btn btn-deal" id="ind-start-btn">Iniciar prueba</button>
      </div>
    </div>
  </div>

  <!-- ======================= Individual: juego ======================= -->
  <div class="screen" id="screen-individual-play">
    <div class="dashboard">
      <div class="metric-card"><h3>Ronda</h3><div class="value" id="ind-round-label">-</div></div>
      <div class="metric-card"><h3>Restantes</h3><div class="value" id="ind-remaining">0</div></div>
      <div class="metric-card"><h3>Valor Esperado</h3><div class="value" id="ind-ve">$0</div></div>
      <div class="metric-card"><h3>Oferta Estimada</h3><div class="value offer" id="ind-offer">$0</div></div>
    </div>

    <div class="status-bar" id="ind-status">Elegí tu maletín</div>

    <div class="player-case-bar" id="ind-player-bar" hidden>
      <span class="label">Tu maletín</span>
      <span class="num" id="ind-player-num">-</span>
      <span class="hint" id="ind-player-hint">queda cerrado hasta el final</span>
    </div>

    <div class="main-container">
      <div class="values-column" id="ind-low-list"></div>
      <div class="cases-grid" id="ind-cases-grid"></div>
      <div class="values-column" id="ind-high-list"></div>
    </div>

    <div class="controls">
      <button class="reset-btn" id="ind-reset-btn">Reiniciar prueba</button>
    </div>
  </div>

  <!-- Overlay oferta individual -->
  <div class="overlay" id="ind-offer-overlay" hidden>
    <div class="panel">
      <h2>La banca te ofrece</h2>
      <div class="big-amount" id="ind-offer-amount">$0</div>
      <p class="panel-sub" id="ind-offer-sub"></p>
      <div class="panel-actions">
        <button class="btn btn-deal" id="ind-deal-btn">Trato</button>
        <button class="btn btn-nodeal" id="ind-nodeal-btn">No Trato</button>
      </div>
    </div>
  </div>

  <!-- Overlay cambio final individual -->
  <div class="overlay" id="ind-swap-overlay" hidden>
    <div class="panel">
      <h2>Última decisión</h2>
      <p class="panel-sub" id="ind-swap-sub"></p>
      <div class="panel-actions">
        <button class="btn btn-keep" id="ind-keep-btn">Me quedo con el mío</button>
        <button class="btn btn-swap" id="ind-swap-btn">Cambiar maletín</button>
      </div>
    </div>
  </div>

  <!-- Overlay resultado individual -->
  <div class="overlay" id="ind-result-overlay" hidden>
    <div class="panel wide">
      
      <!-- Vista limpia para el jugador -->
      <div id="player-view-block">
        <h2 id="ind-result-title">Fin de la prueba</h2>
        <div class="big-amount" id="ind-result-amount">$0</div>
        <p class="panel-sub" id="ind-result-sub"></p>
        
        <div class="risk-profile" id="ind-risk-profile">
          <span class="risk-tag" id="ind-risk-tag">-</span>
          <p id="ind-risk-detail"></p>
        </div>

        <div class="result-history" id="ind-result-history"></div>
      </div>

      <!-- Sección del Informe Técnico Individual -->
      <div id="ind-pro-report" class="pro-report-container" hidden>
        <div class="pro-report-header">
          <h3>📋 Informe Técnico de Evaluación Conductual</h3>
          <button class="btn btn-ghost" id="ind-print-pro-btn" style="padding:4px 12px; font-size:0.75rem;">🖨️ Imprimir Informe</button>
        </div>
        <div id="ind-pro-report-content" class="pro-report-body"></div>
      </div>

      <div class="panel-actions" style="margin-top:20px;">
        <button class="btn btn-ghost" id="ind-toggle-pro-btn">👁️ Ver Informe Profesional</button>
        <button class="btn btn-deal" id="ind-again-btn">Nueva prueba</button>
      </div>
    </div>
  </div>

  <!-- ========================== Grupal: intro ========================== -->
  <div class="screen" id="screen-group-intro">
    <div class="intro-box">
      <h2>Consenso grupal cronometrado</h2>
      <p class="note">
        El grupo mira el mismo tablero (proyectado) y debe decidir en conjunto, dentro de un tiempo límite,
        si acepta cada oferta de la banca. Podés anotar quién impulsó cada decisión para observar
        participación y liderazgo.
      </p>
      <div class="field-row">
        <input type="text" id="grp-name-input" placeholder="Nombre del/de la participante">
        <button class="btn btn-deal" id="grp-add-btn" type="button">Agregar</button>
      </div>
      <div class="participant-list" id="grp-participant-list">
        <p class="empty-note">Todavía no agregaste participantes (es opcional).</p>
      </div>
      <div class="panel-actions">
        <button class="btn btn-ghost" data-back>‹ Volver</button>
        <button class="btn btn-deal" id="grp-start-btn">Iniciar dinámica</button>
      </div>
    </div>
  </div>

  <!-- ========================== Grupal: juego ========================== -->
  <div class="screen" id="screen-group-play">
    <div class="dashboard">
      <div class="metric-card"><h3>Ronda</h3><div class="value" id="grp-round-label">-</div></div>
      <div class="metric-card"><h3>Restantes</h3><div class="value" id="grp-remaining">0</div></div>
      <div class="metric-card"><h3>Valor Esperado</h3><div class="value" id="grp-ve">$0</div></div>
      <div class="metric-card"><h3>Oferta Estimada</h3><div class="value offer" id="grp-offer">$0</div></div>
    </div>

    <div class="status-bar" id="grp-status">Elegí el maletín del grupo</div>

    <div class="player-case-bar" id="grp-player-bar" hidden>
      <span class="label">Maletín del grupo</span>
      <span class="num" id="grp-player-num">-</span>
      <span class="hint" id="grp-player-hint">queda cerrado hasta el final</span>
    </div>

    <div class="main-container">
      <div class="values-column" id="grp-low-list"></div>
      <div class="cases-grid" id="grp-cases-grid"></div>
      <div class="values-column" id="grp-high-list"></div>
    </div>

    <div class="controls">
      <button class="reset-btn" id="grp-reset-btn">Reiniciar dinámica</button>
    </div>
  </div>

  <!-- Overlay oferta grupal -->
  <div class="overlay" id="grp-offer-overlay" hidden>
    <div class="panel">
      <h2>La banca ofrece</h2>
      <div class="countdown-ring" id="grp-countdown">45s</div>
      <div class="big-amount" id="grp-offer-amount">$0</div>
      <p class="panel-sub" id="grp-offer-sub"></p>
      <div class="driver-select" id="grp-driver-wrap">
        <label for="grp-driver-select">¿Quién impulsó esta decisión? (opcional)</label>
        <select id="grp-driver-select">
          <option value="">— Sin especificar —</option>
        </select>
      </div>
      <div class="panel-actions">
        <button class="btn btn-deal" id="grp-deal-btn">Trato</button>
        <button class="btn btn-nodeal" id="grp-nodeal-btn">No Trato</button>
      </div>
    </div>
  </div>

  <!-- Overlay cambio final grupal -->
  <div class="overlay" id="grp-swap-overlay" hidden>
    <div class="panel">
      <h2>Última decisión</h2>
      <p class="panel-sub" id="grp-swap-sub"></p>
      <div class="driver-select">
        <label for="grp-swap-driver-select">¿Quién impulsó esta decisión? (opcional)</label>
        <select id="grp-swap-driver-select">
          <option value="">— Sin especificar —</option>
        </select>
      </div>
      <div class="panel-actions">
        <button class="btn btn-keep" id="grp-keep-btn">Se queda con el suyo</button>
        <button class="btn btn-swap" id="grp-swap-btn">Cambiar maletín</button>
      </div>
    </div>
  </div>

  <!-- Overlay resultado grupal -->
  <div class="overlay" id="grp-result-overlay" hidden>
    <div class="panel wide">
      
      <!-- Vista limpia para el grupo -->
      <div id="grp-player-view-block">
        <h2 id="grp-result-title">Fin de la dinámica</h2>
        <div class="big-amount" id="grp-result-amount">$0</div>
        <p class="panel-sub" id="grp-result-sub"></p>
        <div class="participation-list" id="grp-participation-list"></div>
        <div class="result-history" id="grp-result-history"></div>
      </div>

      <!-- Sección del Informe Técnico Grupal -->
      <div id="grp-pro-report" class="pro-report-container" hidden>
        <div class="pro-report-header">
          <h3>📋 Informe Técnico de Dinámica Grupal</h3>
          <button class="btn btn-ghost" id="grp-print-pro-btn" style="padding:4px 12px; font-size:0.75rem;">🖨️ Imprimir Informe Grupal</button>
        </div>
        <div id="grp-pro-report-content" class="pro-report-body"></div>
      </div>

      <div class="panel-actions" style="margin-top:20px;">
        <button class="btn btn-ghost" id="grp-toggle-pro-btn">👁️ Ver Informe Profesional</button>
        <button class="btn btn-deal" id="grp-again-btn">Nueva dinámica</button>
      </div>
    </div>
  </div>

  <script src="dinamicas.js"></script>
</body>
</html>
```

### Archivo: `dinamicas.js`
```javascript
/* ==========================================================================
   Dinámicas — Archivo independiente para el juego de maletines
   ========================================================================== */

const CONFIG = {
  values: [
    1, 2, 5, 10, 25, 50, 75, 100, 200, 300, 400, 500, 750,
    1000, 2500, 5000, 10000, 20000, 30000, 40000, 50000, 75000, 100000, 250000, 500000, 1000000
  ],
  roundPlan: [6, 5, 4, 3, 2, 1, 1, 1, 1],
  locale: 'es-ES'
};

const BANK = { start: 0.60, end: 0.90 };
const FIXED_SEED = 20260918;
const GROUP_ROUND_SECONDS = 45;

const $ = (id) => document.getElementById(id);

function formatMoney(amount) {
  return '$' + Math.round(amount).toLocaleString(CONFIG.locale);
}

function totalRounds() {
  return CONFIG.roundPlan.length;
}

function seededShuffle(array, seed) {
  const arr = [...array];
  let s = seed;
  function rand() {
    s |= 0; s = (s + 0x6D2B79F5) | 0;
    let t = Math.imul(s ^ (s >>> 15), 1 | s);
    t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  }
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(rand() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

function shuffleRandom(array) {
  const arr = [...array];
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

function expectedValueOf(vals) {
  if (!vals.length) return 0;
  return vals.reduce((a, b) => a + b, 0) / vals.length;
}

function bankOfferOf(vals) {
  const total = CONFIG.values.length;
  const opened = total - vals.length;
  const progress = opened / (total - 2 || 1);
  const factor = BANK.start + Math.min(progress, 1) * (BANK.end - BANK.start);
  return expectedValueOf(vals) * factor;
}

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  $(id).classList.add('active');
}

/* ============================================================================
   DINÁMICA INDIVIDUAL
   ============================================================================ */

let ind = null;

function indCreateCases() {
  const values = seededShuffle(CONFIG.values, FIXED_SEED);
  return CONFIG.values.map((_, i) => ({ id: i + 1, value: values[i], opened: false, isPlayer: false }));
}

function indStart(name) {
  ind = {
    name: name || 'Sin nombre',
    cases: indCreateCases(),
    playerCaseId: null,
    phase: 'pick',
    roundIndex: 0,
    opensLeft: 0,
    history: [],
    offerShownAt: null,
    startedAt: performance.now(),
    result: null
  };
  hideIndOverlays();
  showScreen('screen-individual-play');
  indRenderAll();
}

function indUnopened() { return ind.cases.filter(c => !c.opened); }
function indRemainingValues() { return indUnopened().map(c => c.value); }
function indPlayerCase() { return ind.cases.find(c => c.id === ind.playerCaseId); }

function indPick(id) {
  const c = ind.cases.find(item => item.id === id);
  if (!c) return;
  c.isPlayer = true;
  ind.playerCaseId = id;
  ind.phase = 'open';
  ind.roundIndex = 0;
  ind.opensLeft = CONFIG.roundPlan[0];
  indRenderAll();
}

function indOpen(id) {
  const c = ind.cases.find(item => item.id === id);
  if (!c || c.opened || c.isPlayer) return;
  c.opened = true;
  ind.opensLeft--;
  if (ind.opensLeft <= 0) ind.phase = 'offer';
  indRenderAll();
  if (ind.phase === 'offer') setTimeout(indShowOffer, 400);
}

function indShowOffer() {
  const vals = indRemainingValues();
  const ve = expectedValueOf(vals);
  const offer = bankOfferOf(vals);
  const pending = vals.length - 1;

  $('ind-offer-amount').innerText = formatMoney(offer);$('ind-offer-sub').innerText =
    `Valor esperado: ${formatMoney(ve)} · Oferta = ${Math.round((offer / ve) * 100)}% del VE · ` +
    (pending > 0 ? `Si rechazás, seguís con ${pending} maletín${pending === 1 ? '' : 'es'}.` : 'Última oportunidad antes del cambio final.');

  ind.offerShownAt = performance.now();
  $('ind-offer-overlay').hidden = false;
  indRenderStatus();
}

function indDecide(decision) {
  const vals = indRemainingValues();
  const ve = expectedValueOf(vals);
  const offer = bankOfferOf(vals);
  const latencyMs = ind.offerShownAt ? Math.round(performance.now() - ind.offerShownAt) : 0;

  ind.history.push({ round: ind.roundIndex + 1, ve: Math.round(ve), offer: Math.round(offer), decision, latencyMs });

  if (decision === 'Trato') {
    $('ind-offer-overlay').hidden = true;
    indFinish({ type: 'deal', amount: offer, playerValue: indPlayerCase().value });
    return;
  }

  $('ind-offer-overlay').hidden = true;
  ind.roundIndex++;
  if (ind.roundIndex >= totalRounds()) {
    indStartFinalPhase();
  } else {
    ind.phase = 'open';
    ind.opensLeft = CONFIG.roundPlan[ind.roundIndex];
    indRenderAll();
  }
}

function indStartFinalPhase() {
  const other = indUnopened().find(c => !c.isPlayer);
  if (!other) {
    indFinish({ type: 'keep', amount: indPlayerCase().value, playerValue: indPlayerCase().value });
    return;
  }
  ind.phase = 'swap';
  const mine = indPlayerCase();
  $('ind-swap-sub').innerText =
    `Quedan dos maletines: el tuyo (Nº ${mine.id}) y el Nº ${other.id}. ` +
    `Los premios vivos son ${formatMoney(Math.min(mine.value, other.value))} y ${formatMoney(Math.max(mine.value, other.value))}. ¿Cambiás?`;
  $('ind-swap-overlay').hidden = false;
  indRenderAll();
}

function indResolveFinal(didSwap) {
  const mine = indPlayerCase();
  const other = indUnopened().find(c => !c.isPlayer);
  const finalCase = didSwap ? other : mine;
  indFinish({
    type: didSwap ? 'swap' : 'keep',
    amount: finalCase.value,
    playerValue: mine.value,
    otherValue: other ? other.value : null
  });
}

function indComputeRiskProfile(result) {
  if (result.type !== 'deal') {
    return {
      tag: 'Buscador de riesgo',
      detail: 'Llegaste hasta el final sin aceptar ofertas: mantuviste la incertidumbre completa hasta el último instante.'
    };
  }

  const last = ind.history[ind.history.length - 1];
  const ratio = last && last.ve ? last.offer / last.ve : 0;
  const pct = Math.round(ratio * 100);

  if (ratio < 0.68) {
    return {
      tag: 'Conservador',
      detail: `Aceptaste cuando la oferta llegó al ${pct}% del valor esperado: preferiste asegurar una cifra cierta antes que arriesgar.`
    };
  }
  if (ratio < 0.85) {
    return {
      tag: 'Equilibrado',
      detail: `Aceptaste cuando la oferta llegó al ${pct}% del valor esperado: lograste un balance entre prudencia y oportunidad.`
    };
  }
  return {
    tag: 'Buscador de riesgo',
    detail: `Esperaste hasta que la oferta llegó al ${pct}% del valor esperado antes de aceptar: mostraste alta tolerancia a la volatilidad.`
  };
}

function indRenderProReport(ind, profile, totalMs, avgMs) {
  const history = ind.history || [];
  const last = history.length ? history[history.length - 1] : null;
  const ratio = (last && last.ve) ? (last.offer / last.ve) : 0;

  const speedStyle = avgMs < 3000 ? "Procesamiento Intuitivo / Heurístico Rápido" : "Procesamiento Pausado / Analítico Reflexivo";
  const outcomeText = ind.result.type === 'deal' ? `Aceptó oferta de ${formatMoney(ind.result.amount)}` : `Rechazó todas las ofertas (Llegó al final)`;

  let qualitativeObs = '';
  if (ind.result.type !== 'deal') {
    qualitativeObs = 'Demuestra una orientación marcada hacia la maximización del resultado bajo alta incertidumbre, priorizando el beneficio potencial sobre la seguridad del capital inmediato.';
  } else if (ratio < 0.68) {
    qualitativeObs = 'Muestra una aversión al riesgo pronunciada. Estuvo dispuesto a ceder más del 32% del valor esperado ponderado a cambio de eliminar la incertidumbre.';
  } else {
    qualitativeObs = 'Muestra una conducta negociadora equilibrada, evaluando racionalmente el costo de oportunidad y cerrando cuando el retorno marginal justificaba la certeza.';
  }

  const tableRows = history.map(h => {
    const rPct = Math.round((h.offer / h.ve) * 100);
    return `
      <tr style="border-bottom:1px solid rgba(148,163,184,0.1);">
        <td style="padding:6px;">Ronda ${h.round}</td>
        <td style="padding:6px;">${formatMoney(h.ve)}</td>
        <td style="padding:6px;">${formatMoney(h.offer)} (${rPct}%)</td>
        <td style="padding:6px; font-weight:bold;">${h.decision}</td>
        <td style="padding:6px;">${(h.latencyMs / 1000).toFixed(1)}s</td>
      </tr>
    `;
  }).join('');

  const html = `
    <div style="font-size:0.85rem; display:flex; flex-direction:column; gap:10px;">
      <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; background:rgba(0,0,0,0.2); padding:10px; border-radius:8px;">
        <div><strong>Participante:</strong> ${ind.name || 'Sin especificar'}</div>
        <div><strong>Fecha:</strong> ${new Date().toLocaleDateString('es-ES')}</div>
        <div><strong>Perfil de Riesgo:</strong> <span>${profile.tag}</span></div>
        <div><strong>Estilo de Decisión:</strong> ${speedStyle}</div>
      </div>

      <div style="background:rgba(0,0,0,0.2); padding:10px; border-radius:8px;">
        <strong style="display:block; margin-bottom:4px;">Resumen Ejecutivo:</strong>
        <ul style="margin-left:18px; margin-bottom:0; line-height:1.4;">
          <li><strong>Resultado Final:</strong> ${outcomeText}.</li>
          <li><strong>Tiempo Total de Deliberación:</strong> ${(totalMs / 1000).toFixed(1)} segundos (Promedio de ${(avgMs / 1000).toFixed(1)}s por oferta).</li>
          <li><strong>Observación Cualitativa:</strong> ${qualitativeObs}</li>
        </ul>
      </div>

      <div style="margin-top:6px;">
        <strong style="display:block; margin-bottom:6px;">Matriz Cuantitativa de Ofertas y Tiempos:</strong>
        <table style="width:100%; border-collapse:collapse; text-align:left; font-size:0.8rem; background:rgba(0,0,0,0.2); border-radius:8px; overflow:hidden;">
          <thead>
            <tr style="background:rgba(148,163,184,0.1); font-family:Orbitron,sans-serif; font-size:0.68rem; text-transform:uppercase;">
              <th style="padding:6px;">Ronda</th>
              <th style="padding:6px;">VE</th>
              <th style="padding:6px;">Oferta (% VE)</th>
              <th style="padding:6px;">Decisión</th>
              <th style="padding:6px;">Tiempo</th>
            </tr>
          </thead>
          <tbody>
            ${tableRows}
          </tbody>
        </table>
      </div>
    </div>
  `;

  $('ind-pro-report-content').innerHTML = html;
}

function indFinish(result) {
  ind.result = result;
  ind.phase = 'end';
  ind.cases.forEach(c => { c.opened = true; });
  hideIndOverlays();
  indRenderBoard();
  indRenderStatus();
  indRenderMetrics();
  indRenderResult();
  $('ind-result-overlay').hidden = false;
}

function indRenderAll() {
  indRenderBoard();
  indRenderMetrics();
  indRenderStatus();
  indRenderPlayerBar();
}

function indRenderBoard() {
  const lowList = $('ind-low-list');
  const highList = $('ind-high-list');
  lowList.innerHTML = '';
  highList.innerHTML = '';

  const revealed = ind.cases.filter(c => c.opened).map(c => c.value);
  const half = Math.ceil(CONFIG.values.length / 2);

  const makeBadge = (val, cls) => {
    const b = document.createElement('div');
    b.className = `value-badge ${cls} ${revealed.includes(val) ? 'eliminated' : ''}`;
    b.innerText = formatMoney(val);
    return b;
  };

  CONFIG.values.slice(0, half).forEach(v => lowList.appendChild(makeBadge(v, 'low')));
  CONFIG.values.slice(half).forEach(v => highList.appendChild(makeBadge(v, 'high')));

  const grid = $('ind-cases-grid');
  grid.innerHTML = '';
  const clickable = ind.phase === 'pick' || ind.phase === 'open';

  ind.cases.forEach(c => {
    const el = document.createElement('div');
    const classes = ['case'];
    if (c.opened) classes.push('opened');
    if (c.isPlayer) classes.push('player');
    if (ind.phase === 'pick') classes.push('selectable');
    el.className = classes.join(' ');
    el.innerHTML = `
      <div class="num">${c.id}</div>
      ${c.opened ? `<div class="val">${formatMoney(c.value)}</div>` : ''}
      ${c.isPlayer && !c.opened ? '<div class="mine-tag">TUYO</div>' : ''}
    `;
    if (clickable && !c.opened && !c.isPlayer) {
      el.addEventListener('click', () => {
        if (ind.phase === 'pick') indPick(c.id);
        else if (ind.phase === 'open') indOpen(c.id);
      });
    }
    grid.appendChild(el);
  });
}

function indRenderMetrics() {
  const vals = indRemainingValues();
  $('ind-remaining').innerText = vals.length;
  $('ind-round-label').innerText = ind.phase === 'end'
    ? 'Fin'
    : ind.phase === 'pick' ? '-' : `${Math.min(ind.roundIndex + 1, totalRounds())} / ${totalRounds()}`;
  $('ind-ve').innerText = vals.length ? formatMoney(expectedValueOf(vals)) : '$0';$('ind-offer').innerText = (vals.length && ind.phase !== 'pick') ? formatMoney(bankOfferOf(vals)) : '—';
}

function indRenderStatus() {
  let text;
  switch (ind.phase) {
    case 'pick': text = 'Elegí tu maletín: es el que te llevás si no aceptás ninguna oferta.'; break;
    case 'open': text = `Ronda ${ind.roundIndex + 1}: abrí ${ind.opensLeft} maletín${ind.opensLeft === 1 ? '' : 'es'} más.`; break;
    case 'offer': text = 'La banca hizo su oferta. ¿Trato o No Trato?'; break;
    case 'swap': text = 'Dos maletines en juego: quedarse o cambiar.'; break;
    case 'end': text = 'Prueba terminada.'; break;
    default: text = '';
  }
  $('ind-status').innerText = text;
}

function indRenderPlayerBar() {
  const bar = $('ind-player-bar');
  if (ind.playerCaseId == null) { bar.hidden = true; return; }
  bar.hidden = false;
  $('ind-player-num').innerText = 'Nº ' + ind.playerCaseId;
  $('ind-player-hint').innerText = ind.phase === 'end'
    ? `contenía ${formatMoney(indPlayerCase().value)}`
    : 'queda cerrado hasta el final';
}

function indRenderResult() {
  const r = ind.result;
  let title, sub;

  if (r.type === 'deal') {
    title = 'Cerraste trato con la banca';
    const diff = r.playerValue - r.amount;
    sub = `Tu maletín (Nº ${ind.playerCaseId}) tenía ${formatMoney(r.playerValue)}. ` +
      (diff > 0 ? `Dejaste ${formatMoney(diff)} sobre la mesa.` : `Le ganaste ${formatMoney(Math.abs(diff))} a la banca.`);
  } else if (r.type === 'keep') {
    title = 'Te quedaste con tu maletín';
    sub = r.otherValue != null ? `El otro maletín tenía ${formatMoney(r.otherValue)}.` : 'Llegaste al final sin aceptar ninguna oferta.';
  } else {
    title = 'Cambiaste de maletín';
    sub = `Tu maletín original (Nº ${ind.playerCaseId}) tenía ${formatMoney(r.playerValue)}.`;
  }

  $('ind-result-title').innerText = `${title} — ${ind.name}`;
  $('ind-result-amount').innerText = formatMoney(r.amount);$('ind-result-sub').innerText = sub;

  const profile = indComputeRiskProfile(r);
  $('ind-risk-tag').innerText = profile.tag;
  $('ind-risk-detail').innerText = profile.detail;

  const totalMs = ind.history.reduce((acc, h) => acc + h.latencyMs, 0);
  const avgMs = ind.history.length ? Math.round(totalMs / ind.history.length) : 0;

  $('ind-result-history').innerHTML =
    `<h3>Decisiones tomadas</h3>` +
    ind.history.map(h => `
      <div class="hist-row">
        <span>Ronda ${h.round}</span>
        <span>VE ${formatMoney(h.ve)}</span>
        <span class="hist-offer">${formatMoney(h.offer)}</span>
        <span class="hist-dec ${h.decision === 'Trato' ? 'deal' : 'nodeal'}">${h.decision}</span>
      </div>`).join('');

  indRenderProReport(ind, profile, totalMs, avgMs);

  $('ind-pro-report').hidden = true;
  $('ind-toggle-pro-btn').innerText = '👁️ Ver Informe Profesional';
}

function hideIndOverlays() {
  ['ind-offer-overlay', 'ind-swap-overlay', 'ind-result-overlay'].forEach(id => {
    const el = $(id);
    if (el) el.hidden = true;
  });
}

/* ============================================================================
   DINÁMICA GRUPAL
   ============================================================================ */

let grpParticipants = [];
let grp = null;
let grpTimerHandle = null;

function grpAddParticipant(name) {
  const clean = name.trim();
  if (!clean) return;
  grpParticipants.push(clean);
  renderGrpParticipantEditor();
}

function grpRemoveParticipant(index) {
  grpParticipants.splice(index, 1);
  renderGrpParticipantEditor();
}

function renderGrpParticipantEditor() {
  const box = $('grp-participant-list');
  if (!grpParticipants.length) {
    box.innerHTML = '<p class="empty-note">Todavía no agregaste participantes (es opcional).</p>';
    return;
  }
  box.innerHTML = '';
  grpParticipants.forEach((name, i) => {
    const row = document.createElement('div');
    row.className = 'participant-row';
    row.innerHTML = `<span>${name}</span><button type="button" data-i="${i}">Quitar</button>`;
    row.querySelector('button').addEventListener('click', () => grpRemoveParticipant(i));
    box.appendChild(row);
  });
}

function grpFillDriverSelects() {
  ['grp-driver-select', 'grp-swap-driver-select'].forEach(id => {
    const sel = $(id);
    if (sel) {
      sel.innerHTML = '<option value="">— Sin especificar —</option>' +
        grpParticipants.map(n => `<option value="${n}">${n}</option>`).join('');
    }
  });
}

function grpCreateCases() {
  const values = shuffleRandom(CONFIG.values);
  return CONFIG.values.map((_, i) => ({ id: i + 1, value: values[i], opened: false, isPlayer: false }));
}

function grpStart() {
  grp = {
    cases: grpCreateCases(),
    playerCaseId: null,
    phase: 'pick',
    roundIndex: 0,
    opensLeft: 0,
    history: [],
    result: null
  };
  clearInterval(grpTimerHandle);
  grpFillDriverSelects();
  hideGrpOverlays();
  showScreen('screen-group-play');
  grpRenderAll();
}

function grpUnopened() { return grp.cases.filter(c => !c.opened); }
function grpRemainingValues() { return grpUnopened().map(c => c.value); }
function grpPlayerCase() { return grp.cases.find(c => c.id === grp.playerCaseId); }

function grpPick(id) {
  const c = grp.cases.find(item => item.id === id);
  if (!c) return;
  c.isPlayer = true;
  grp.playerCaseId = id;
  grp.phase = 'open';
  grp.roundIndex = 0;
  grp.opensLeft = CONFIG.roundPlan[0];
  grpRenderAll();
}

function grpOpen(id) {
  const c = grp.cases.find(item => item.id === id);
  if (!c || c.opened || c.isPlayer) return;
  c.opened = true;
  grp.opensLeft--;
  if (grp.opensLeft <= 0) grp.phase = 'offer';
  grpRenderAll();
  if (grp.phase === 'offer') setTimeout(grpShowOffer, 400);
}

function grpShowOffer() {
  const vals = grpRemainingValues();
  const ve = expectedValueOf(vals);
  const offer = bankOfferOf(vals);
  const pending = vals.length - 1;

  $('grp-offer-amount').innerText = formatMoney(offer);$('grp-offer-sub').innerText =
    `Valor esperado: ${formatMoney(ve)} · Oferta = ${Math.round((offer / ve) * 100)}% del VE · ` +
    (pending > 0 ? `Si rechazan, siguen con ${pending} maletín${pending === 1 ? '' : 'es'}.` : 'Última oportunidad antes del cambio final.');

  $('grp-driver-select').value = '';$('grp-offer-overlay').hidden = false;
  grpRenderStatus();
  grpStartTimer(offer, ve);
}

function grpStartTimer(offer, ve) {
  clearInterval(grpTimerHandle);
  let secondsLeft = GROUP_ROUND_SECONDS;
  const ring = $('grp-countdown');
  ring.innerText = secondsLeft + 's';
  ring.classList.remove('low');

  grpTimerHandle = setInterval(() => {
    secondsLeft--;
    ring.innerText = Math.max(secondsLeft, 0) + 's';
    if (secondsLeft <= 10) ring.classList.add('low');
    if (secondsLeft <= 0) {
      clearInterval(grpTimerHandle);
      grpDecide('No Trato', true);
    }
  }, 1000);
}

function grpDecide(decision, auto) {
  clearInterval(grpTimerHandle);
  const vals = grpRemainingValues();
  const ve = expectedValueOf(vals);
  const offer = bankOfferOf(vals);
  const driver = auto ? '' : $('grp-driver-select').value;

  grp.history.push({ round: grp.roundIndex + 1, ve: Math.round(ve), offer: Math.round(offer), decision, driver, auto: !!auto });

  if (decision === 'Trato') {
    $('grp-offer-overlay').hidden = true;
    grpFinish({ type: 'deal', amount: offer, playerValue: grpPlayerCase().value });
    return;
  }

  $('grp-offer-overlay').hidden = true;
  grp.roundIndex++;
  if (grp.roundIndex >= totalRounds()) {
    grpStartFinalPhase();
  } else {
    grp.phase = 'open';
    grp.opensLeft = CONFIG.roundPlan[grp.roundIndex];
    grpRenderAll();
  }
}

function grpStartFinalPhase() {
  const other = grpUnopened().find(c => !c.isPlayer);
  if (!other) {
    grpFinish({ type: 'keep', amount: grpPlayerCase().value, playerValue: grpPlayerCase().value });
    return;
  }
  grp.phase = 'swap';
  const mine = grpPlayerCase();
  $('grp-swap-sub').innerText =
    `Quedan dos maletines: el suyo (Nº ${mine.id}) y el Nº ${other.id}. ` +
    `Los premios vivos son ${formatMoney(Math.min(mine.value, other.value))} y ${formatMoney(Math.max(mine.value, other.value))}. ¿Cambian?`;
  $('grp-swap-driver-select').value = '';$('grp-swap-overlay').hidden = false;
  grpRenderAll();
}

function grpResolveFinal(didSwap) {
  const mine = grpPlayerCase();
  const other = grpUnopened().find(c => !c.isPlayer);
  const finalCase = didSwap ? other : mine;
  const driver = $('grp-swap-driver-select').value;

  grp.history.push({
    round: totalRounds() + 1,
    ve: 0,
    offer: 0,
    decision: didSwap ? 'Cambió' : 'Se quedó',
    driver,
    auto: false,
    isFinal: true
  });

  grpFinish({
    type: didSwap ? 'swap' : 'keep',
    amount: finalCase.value,
    playerValue: mine.value,
    otherValue: other ? other.value : null
  });
}

function grpRenderProReport(grp) {
  const history = grp.history || [];
  const tally = grpParticipationTally();
  const totalDecisions = history.length;

  let leadersHtml = '';
  if (tally.length) {
    leadersHtml = tally.map(([name, count]) => {
      const pct = Math.round((count / totalDecisions) * 100);
      return `<li><strong>${name}:</strong> Impulsó ${count} decisión(es) (${pct}% de influencia en el grupo).</li>`;
    }).join('');
  } else {
    leadersHtml = '<li>No se asignaron líderes o impulsores explícitos durante la dinámica.</li>';
  }

  const autoDecisions = history.filter(h => h.auto).length;
  const timeObs = autoDecisions > 0
    ? `El grupo tuvo ${autoDecisions} decisión(es) forzada(s) por agotamiento del tiempo de 45s, indicando dificultad para alcanzar consenso bajo presión.`
    : 'El grupo gestionó eficazmente el tiempo disponible (45s por ronda), logrando consensos dentro del límite estipulado.';

  const tableRows = history.map(h => `
    <tr style="border-bottom:1px solid rgba(148,163,184,0.1);">
      <td style="padding:6px;">${h.isFinal ? 'Final' : 'Ronda ' + h.round}</td>
      <td style="padding:6px;">${h.ve ? formatMoney(h.ve) : '—'}</td>
      <td style="padding:6px;">${h.offer ? formatMoney(h.offer) : '—'}</td>
      <td style="padding:6px; font-weight:bold;">${h.decision}${h.auto ? ' (Tiempo agotado)' : ''}</td>
      <td style="padding:6px;">${h.driver || 'Consenso no registrado'}</td>
    </tr>
  `).join('');

  const html = `
    <div style="font-size:0.85rem; display:flex; flex-direction:column; gap:10px;">
      <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; background:rgba(0,0,0,0.2); padding:10px; border-radius:8px;">
        <div><strong>Modalidad:</strong> Consenso Grupal Cronometrado</div>
        <div><strong>Fecha:</strong> ${new Date().toLocaleDateString('es-ES')}</div>
        <div><strong>Participantes Registrados:</strong> ${grpParticipants.length || 'Sin lista previa'}</div>
        <div><strong>Resultado Final:</strong> ${formatMoney(grp.result.amount)}</div>
      </div>

      <div style="background:rgba(0,0,0,0.2); padding:10px; border-radius:8px;">
        <strong style="display:block; margin-bottom:4px;">Análisis de Liderazgo e Influencia:</strong>
        <ul style="margin-left:18px; margin-bottom:6px; line-height:1.4;">
          ${leadersHtml}
        </ul>
        <strong style="display:block; margin-top:8px; margin-bottom:4px;">Gestión del Tiempo y Presión:</strong>
        <p style="margin:0; line-height:1.4;">${timeObs}</p>
      </div>

      <div style="margin-top:6px;">
        <strong style="display:block; margin-bottom:6px;">Matriz de Decisiones e Impulsores:</strong>
        <table style="width:100%; border-collapse:collapse; text-align:left; font-size:0.8rem; background:rgba(0,0,0,0.2); border-radius:8px; overflow:hidden;">
          <thead>
            <tr style="background:rgba(148,163,184,0.1); font-family:Orbitron,sans-serif; font-size:0.68rem; text-transform:uppercase;">
              <th style="padding:6px;">Ronda</th>
              <th style="padding:6px;">VE</th>
              <th style="padding:6px;">Oferta</th>
              <th style="padding:6px;">Decisión</th>
              <th style="padding:6px;">Impulsor</th>
            </tr>
          </thead>
          <tbody>
            ${tableRows}
          </tbody>
        </table>
      </div>
    </div>
  `;

  $('grp-pro-report-content').innerHTML = html;
}

function grpFinish(result) {
  grp.result = result;
  grp.phase = 'end';
  grp.cases.forEach(c => { c.opened = true; });
  hideGrpOverlays();
  grpRenderBoard();
  grpRenderStatus();
  grpRenderMetrics();
  grpRenderResult();
  $('grp-result-overlay').hidden = false;
}

function grpRenderAll() {
  grpRenderBoard();
  grpRenderMetrics();
  grpRenderStatus();
  grpRenderPlayerBar();
}

function grpRenderBoard() {
  const lowList = $('grp-low-list');
  const highList = $('grp-high-list');
  lowList.innerHTML = '';
  highList.innerHTML = '';

  const revealed = grp.cases.filter(c => c.opened).map(c => c.value);
  const half = Math.ceil(CONFIG.values.length / 2);

  const makeBadge = (val, cls) => {
    const b = document.createElement('div');
    b.className = `value-badge ${cls} ${revealed.includes(val) ? 'eliminated' : ''}`;
    b.innerText = formatMoney(val);
    return b;
  };

  CONFIG.values.slice(0, half).forEach(v => lowList.appendChild(makeBadge(v, 'low')));
  CONFIG.values.slice(half).forEach(v => highList.appendChild(makeBadge(v, 'high')));

  const grid = $('grp-cases-grid');
  grid.innerHTML = '';
  const clickable = grp.phase === 'pick' || grp.phase === 'open';

  grp.cases.forEach(c => {
    const el = document.createElement('div');
    const classes = ['case'];
    if (c.opened) classes.push('opened');
    if (c.isPlayer) classes.push('player');
    if (grp.phase === 'pick') classes.push('selectable');
    el.className = classes.join(' ');
    el.innerHTML = `
      <div class="num">${c.id}</div>
      ${c.opened ? `<div class="val">${formatMoney(c.value)}</div>` : ''}
      ${c.isPlayer && !c.opened ? '<div class="mine-tag">GRUPO</div>' : ''}
    `;
    if (clickable && !c.opened && !c.isPlayer) {
      el.addEventListener('click', () => {
        if (grp.phase === 'pick') grpPick(c.id);
        else if (grp.phase === 'open') grpOpen(c.id);
      });
    }
    grid.appendChild(el);
  });
}

function grpRenderMetrics() {
  const vals = grpRemainingValues();
  $('grp-remaining').innerText = vals.length;
  $('grp-round-label').innerText = grp.phase === 'end'
    ? 'Fin'
    : grp.phase === 'pick' ? '-' : `${Math.min(grp.roundIndex + 1, totalRounds())} / ${totalRounds()}`;
  $('grp-ve').innerText = vals.length ? formatMoney(expectedValueOf(vals)) : '$0';$('grp-offer').innerText = (vals.length && grp.phase !== 'pick') ? formatMoney(bankOfferOf(vals)) : '—';
}

function grpRenderStatus() {
  let text;
  switch (grp.phase) {
    case 'pick': text = 'Elijan el maletín del grupo.'; break;
    case 'open': text = `Ronda ${grp.roundIndex + 1}: abran ${grp.opensLeft} maletín${grp.opensLeft === 1 ? '' : 'es'} más.`; break;
    case 'offer': text = 'La banca ofrece. Discutan y decidan antes de que termine el tiempo.'; break;
    case 'swap': text = 'Dos maletines en juego: quedarse o cambiar.'; break;
    case 'end': text = 'Dinámica terminada.'; break;
    default: text = '';
  }
  $('grp-status').innerText = text;
}

function grpRenderPlayerBar() {
  const bar = $('grp-player-bar');
  if (grp.playerCaseId == null) { bar.hidden = true; return; }
  bar.hidden = false;
  $('grp-player-num').innerText = 'Nº ' + grp.playerCaseId;
  $('grp-player-hint').innerText = grp.phase === 'end'
    ? `contenía ${formatMoney(grpPlayerCase().value)}`
    : 'queda cerrado hasta el final';
}

function grpParticipationTally() {
  const counts = {};
  grp.history.forEach(h => {
    if (h.driver) counts[h.driver] = (counts[h.driver] || 0) + 1;
  });
  return Object.entries(counts).sort((a, b) => b[1] - a[1]);
}

function grpRenderResult() {
  const r = grp.result;
  let title, sub;

  if (r.type === 'deal') {
    title = 'El grupo cerró trato con la banca';
    const diff = r.playerValue - r.amount;
    sub = `El maletín del grupo (Nº ${grp.playerCaseId}) tenía ${formatMoney(r.playerValue)}. ` +
      (diff > 0 ? `Dejaron ${formatMoney(diff)} sobre la mesa.` : `Le ganaron ${formatMoney(Math.abs(diff))} a la banca.`);
  } else if (r.type === 'keep') {
    title = 'El grupo se quedó con su maletín';
    sub = r.otherValue != null ? `El otro maletín tenía ${formatMoney(r.otherValue)}.` : 'Llegaron al final sin aceptar ninguna oferta.';
  } else {
    title = 'El grupo cambió de maletín';
    sub = `El maletín original (Nº ${grp.playerCaseId}) tenía ${formatMoney(r.playerValue)}.`;
  }

  $('grp-result-title').innerText = title;
  $('grp-result-amount').innerText = formatMoney(r.amount);$('grp-result-sub').innerText = sub;

  const tally = grpParticipationTally();
  const partBox = $('grp-participation-list');
  if (!tally.length) {
    partBox.innerHTML = '<p class="empty-note">No se registraron participantes en las decisiones.</p>';
  } else {
    const max = tally[0][1];
    partBox.innerHTML = '<h3 style="font-family:Orbitron,sans-serif;font-size:0.72rem;letter-spacing:1px;text-transform:uppercase;color:#64748b;margin-bottom:10px;">Participación en las decisiones</h3>' +
      tally.map(([name, count]) => `
        <div class="participation-row">
          <span>${name}</span>
          <span class="participation-bar-track"><span class="participation-bar-fill" style="width:${(count / max) * 100}%"></span></span>
          <span>${count}</span>
        </div>`).join('');
  }

  $('grp-result-history').innerHTML =
    '<h3>Historial de decisiones</h3>' +
    grp.history.map(h => `
      <div class="hist-row">
        <span>${h.isFinal ? 'Final' : 'Ronda ' + h.round}</span>
        <span class="hist-offer">${h.offer ? formatMoney(h.offer) : '—'}</span>
        <span class="hist-dec ${h.auto ? 'auto' : (h.decision === 'Trato' ? 'deal' : 'nodeal')}">${h.decision}${h.auto ? ' (auto)' : ''}</span>
        <span>${h.driver || '—'}</span>
      </div>`).join('');

  grpRenderProReport(grp);
  $('grp-pro-report').hidden = true;
  $('grp-toggle-pro-btn').innerText = '👁️ Ver Informe Profesional';
}

function hideGrpOverlays() {
  clearInterval(grpTimerHandle);
  ['grp-offer-overlay', 'grp-swap-overlay', 'grp-result-overlay'].forEach(id => {
    const el = $(id);
    if (el) el.hidden = true;
  });
}

/* ----------------------------- Eventos globales --------------------------- */

document.addEventListener('DOMContentLoaded', () => {
  document.querySelectorAll('.dyn-card[data-target]').forEach(card => {
    card.addEventListener('click', () => {
      const target = card.dataset.target;
      if (target === 'individual') showScreen('screen-individual-intro');
      if (target === 'group') showScreen('screen-group-intro');
    });
  });

  document.querySelectorAll('[data-back]').forEach(btn => {
    btn.addEventListener('click', () => showScreen('screen-menu'));
  });

  // Individual
  $('ind-start-btn').addEventListener('click', () => indStart($('ind-name-input').value));
  $('ind-deal-btn').addEventListener('click', () => indDecide('Trato'));$('ind-nodeal-btn').addEventListener('click', () => indDecide('No Trato'));
  $('ind-keep-btn').addEventListener('click', () => indResolveFinal(false));$('ind-swap-btn').addEventListener('click', () => indResolveFinal(true));

  $('ind-toggle-pro-btn').addEventListener('click', () => {
    const reportBox = $('ind-pro-report');
    const isHidden = reportBox.hidden;
    reportBox.hidden = !isHidden;
    $('ind-toggle-pro-btn').innerText = isHidden ? '🙈 Ocultar Informe Profesional' : '👁️ Ver Informe Profesional';
  });

  $('ind-print-pro-btn').addEventListener('click', () => {
    document.body.classList.add('printing-pro-report');
    window.print();
    document.body.classList.remove('printing-pro-report');
  });

  $('ind-again-btn').addEventListener('click', () => {
    hideIndOverlays();
    $('ind-name-input').value = '';
    showScreen('screen-individual-intro');
  });

  $('ind-reset-btn').addEventListener('click', () => {
    if (ind && ind.history.length && !window.confirm('Esto reinicia la prueba actual. ¿Continuar?')) return;
    indStart(ind ? ind.name : '');
  });

  // Grupal
  $('grp-add-btn').addEventListener('click', () => {
    const input = $('grp-name-input');
    grpAddParticipant(input.value);
    input.value = '';
    input.focus();
  });
  $('grp-name-input').addEventListener('keydown', (e) => {     if (e.key === 'Enter') { e.preventDefault();$('grp-add-btn').click(); }
  });
  $('grp-start-btn').addEventListener('click', grpStart);
  $('grp-deal-btn').addEventListener('click', () => grpDecide('Trato', false));$('grp-nodeal-btn').addEventListener('click', () => grpDecide('No Trato', false));
  $('grp-keep-btn').addEventListener('click', () => grpResolveFinal(false));$('grp-swap-btn').addEventListener('click', () => grpResolveFinal(true));

  $('grp-toggle-pro-btn').addEventListener('click', () => {
    const reportBox = $('grp-pro-report');
    const isHidden = reportBox.hidden;
    reportBox.hidden = !isHidden;
    $('grp-toggle-pro-btn').innerText = isHidden ? '🙈 Ocultar Informe Profesional' : '👁️ Ver Informe Profesional';
  });

  $('grp-print-pro-btn').addEventListener('click', () => {
    document.body.classList.add('printing-grp-report');
    window.print();
    document.body.classList.remove('printing-grp-report');
  });

  $('grp-again-btn').addEventListener('click', () => {
    hideGrpOverlays();
    showScreen('screen-group-intro');
  });

  $('grp-reset-btn').addEventListener('click', () => {
    if (grp && grp.history.length && !window.confirm('Esto reinicia la dinámica actual. ¿Continuar?')) return;
    grpStart();
  });

  renderGrpParticipantEditor();
});
```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/juegomaletines/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simulador de Maletines</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-gradient: radial-gradient(circle at center, #1b2838 0%, #0b101d 100%);
      --card-bg: rgba(20, 30, 48, 0.75);
      --card-border: rgba(255, 215, 0, 0.25);
      --gold: #ffd700;
      --cyan: #00f0ff;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Rajdhani', sans-serif;
      background: var(--bg-gradient);
      color: #f8fafc;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 24px;
      background-attachment: fixed;
    }
    h1 {
      font-family: 'Orbitron', sans-serif;
      font-size: 2.1rem;
      font-weight: 900;
      text-transform: uppercase;
      letter-spacing: 2px;
      background: linear-gradient(180deg, #ffffff 30%, #ffd700 100%);
      -webkit-background-clip: text;
      background-clip: text;
      -webkit-text-fill-color: transparent;
      color: transparent;
      margin-bottom: 10px;
      text-align: center;
    }
    p.subtitle {
      color: #94a3b8;
      max-width: 560px;
      text-align: center;
      margin-bottom: 34px;
      font-size: 1rem;
    }
    .menu-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
      gap: 20px;
      width: 100%;
      max-width: 720px;
    }
    .menu-card {
      background: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: 16px;
      padding: 28px 24px;
      text-decoration: none;
      color: #f8fafc;
      backdrop-filter: blur(10px);
      transition: transform 0.2s ease, box-shadow 0.2s ease;
      display: block;
    }
    .menu-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 14px 34px rgba(255, 215, 0, 0.15);
    }
    .menu-card .icon {
      font-size: 2.2rem;
      margin-bottom: 12px;
      display: block;
    }
    .menu-card h2 {
      font-family: 'Orbitron', sans-serif;
      font-size: 1.05rem;
      text-transform: uppercase;
      letter-spacing: 1px;
      margin-bottom: 10px;
      color: var(--gold);
    }
    .menu-card p {
      color: #94a3b8;
      font-size: 0.92rem;
      line-height: 1.5;
    }
    .menu-card.dynamics h2 { color: var(--cyan); }
  </style>
</head>
<body>

  <h1>Simulador de Maletines</h1>
  <p class="subtitle">Elegí si querés jugar la partida clásica o usar el simulador como base para una dinámica con personas o grupos.</p>

  <div class="menu-grid">
    <a class="menu-card" href="juego.html">
      <span class="icon">🎲</span>
      <h2>Jugar</h2>
      <p>La experiencia completa: elegí maletín, abrí en rondas, negociá con la banca. Incluye modo TV y modo Seguimiento para anotar un programa en vivo.</p>
    </a>

    <a class="menu-card dynamics" href="dinamicas.html">
      <span class="icon">🧩</span>
      <h2>Dinámicas</h2>
      <p>Ejercicios individuales y grupales basados en el mismo modelo, pensados para usar en sesiones con personas reales.</p>
    </a>
  </div>

</body>
</html>

```

### Archivo: `juego.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simulador de Maletines - Valor Esperado</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@500;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="styles.css">
</head>
<body>

  <div class="tv-ticker" id="tv-ticker">
    <span class="tv-dot"></span>
    <span class="tv-ticker-label">EN VIVO</span>
    <span class="tv-sep">·</span>
    <span id="tv-ticker-text">Elegí tu maletín</span>
  </div>

  <div class="tv-toggle-fab">
    <span class="mode-group-label">Modo</span>
    <div class="segmented" id="app-mode-segmented">
      <button type="button" class="seg-btn active" data-value="basico">Básico</button>
      <button type="button" class="seg-btn" data-value="tv">TV</button>
      <button type="button" class="seg-btn" data-value="seguimiento">Seguimiento</button>
    </div>
  </div>

  <div class="page-layout">

    <aside class="side-rail side-left">
      <div class="mode-panel" id="mode-panel">
        <div class="mode-group">
          <span class="mode-group-label">Banca</span>
          <div class="segmented" id="bank-segmented">
            <button type="button" class="seg-btn active" data-value="normal">Normal</button>
            <button type="button" class="seg-btn" data-value="tacana">Tacaña</button>
            <button type="button" class="seg-btn" data-value="generosa">Generosa</button>
          </div>
        </div>

        <label class="switch-row">
          <span class="switch-label">Modo educativo</span>
          <span class="switch">
            <input type="checkbox" id="toggle-educativo">
            <span class="slider"></span>
          </span>
        </label>

        <label class="switch-row">
          <span class="switch-label">Perfil de riesgo</span>
          <span class="switch">
            <input type="checkbox" id="toggle-perfil">
            <span class="slider"></span>
          </span>
        </label>
      </div>
    </aside>

    <div class="center-col">

      <h1>Simulador de Maletines</h1>

      <div class="dashboard">
        <div class="metric-card">
          <h3 id="round-card-label">Ronda</h3>
          <div class="value" id="round-label">-</div>
        </div>
        <div class="metric-card">
          <h3>Maletines Restantes</h3>
          <div class="value" id="remaining-count">0</div>
        </div>
        <div class="metric-card">
          <h3>Valor Esperado (VE)</h3>
          <div class="value" id="ve-val">$0</div>
        </div>
        <div class="metric-card">
          <h3>Oferta Estimada Banca</h3>
          <div class="value offer" id="bank-offer">$0</div>
        </div>
      </div>

      <div class="status-bar" id="status-bar">Elegí tu maletín</div>

      <div class="player-case-bar" id="player-case-bar" hidden>
        <span class="label">Tu maletín</span>
        <span class="num" id="player-case-num">-</span>
        <span class="hint" id="player-case-hint">queda cerrado hasta el final</span>
      </div>

      <div class="seg-toolbar" id="seg-toolbar" hidden>
        <button type="button" class="btn btn-sm btn-keep" id="seg-mark-player-btn">Marcar maletín del participante</button>
        <div class="seg-offer-input">
          <input type="number" inputmode="numeric" id="seg-real-offer-input" placeholder="Oferta real ($)">
          <button type="button" class="btn btn-sm btn-deal" id="seg-register-offer-btn">Registrar oferta</button>
        </div>
        <button type="button" class="btn btn-sm btn-nodeal" id="seg-undo-btn">Deshacer</button>
        <button type="button" class="btn btn-sm btn-swap" id="seg-finish-btn">Finalizar seguimiento</button>
      </div>

      <div class="main-container">
        <div class="values-column" id="low-values-list"></div>
        <div class="cases-grid" id="cases-grid"></div>
        <div class="values-column" id="high-values-list"></div>
      </div>

      <div class="result-history seg-log" id="seg-log" hidden>
        <h3>Registro del seguimiento</h3>
        <div id="seg-log-body"></div>
      </div>

      <div class="controls">
        <button class="reset-btn" id="reset-btn">Reiniciar Juego</button>
      </div>

    </div>

    <aside class="side-rail side-right">
      <div class="edu-row" id="edu-row" hidden>
        <div class="edu-metric"><span>Desvío estándar</span><strong id="edu-std">$0</strong></div>
        <div class="edu-metric"><span>Mediana</span><strong id="edu-median">$0</strong></div>
        <div class="edu-metric"><span>Mínimo vivo</span><strong id="edu-min">$0</strong></div>
        <div class="edu-metric"><span>Máximo vivo</span><strong id="edu-max">$0</strong></div>
      </div>
    </aside>

  </div>

  <!-- Panel de oferta de la banca -->
  <div class="overlay" id="offer-overlay" hidden>
    <div class="panel">
      <h2>La banca te ofrece</h2>
      <div class="thinking" id="offer-thinking" hidden>
        La banca está pensando<span class="dots"><span>.</span><span>.</span><span>.</span></span>
      </div>
      <div class="big-amount" id="offer-amount">$0</div>
      <p class="panel-sub" id="offer-sub"></p>
      <div class="panel-actions">
        <button class="btn btn-deal" id="deal-btn">Trato</button>
        <button class="btn btn-nodeal" id="nodeal-btn">No Trato</button>
      </div>
    </div>
  </div>

  <!-- Panel de cambio final -->
  <div class="overlay" id="swap-overlay" hidden>
    <div class="panel">
      <h2>Última decisión</h2>
      <p class="panel-sub" id="swap-sub"></p>
      <div class="panel-actions">
        <button class="btn btn-keep" id="keep-btn">Me quedo con el mío</button>
        <button class="btn btn-swap" id="swap-btn">Cambiar maletín</button>
      </div>
    </div>
  </div>

  <!-- Resultado final -->
  <div class="overlay" id="result-overlay" hidden>
    <div class="panel wide">
      <h2 id="result-title">Fin de la partida</h2>
      <div class="big-amount" id="result-amount">$0</div>
      <p class="panel-sub" id="result-sub"></p>
      <div class="risk-profile" id="risk-profile" hidden>
        <span class="risk-tag" id="risk-tag">-</span>
        <p id="risk-detail"></p>
      </div>
      <div class="result-history" id="result-history"></div>
      <div class="panel-actions">
        <button class="btn btn-deal" id="play-again-btn">Jugar de nuevo</button>
      </div>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>

```

### Archivo: `script.js`
```javascript
/* ==========================================================================
   Simulador de Maletines — juego clásico completo
   Fases: mezcla -> elegir maletín propio -> rondas de apertura + oferta
          (Trato / No Trato) -> cambio final -> revelación
   Toda la parametrización vive en CONFIG: es el punto de enganche para los
   modos de juego que se agreguen más adelante.
   ========================================================================== */

const CONFIG = {
  values: [
    // Columna izquierda (valores bajos / medios)
    1, 2, 5, 10, 25, 50, 75, 100, 200, 300, 400, 500, 750,
    // Columna derecha (valores altos)
    1000, 2500, 5000, 10000, 20000, 30000, 40000, 50000, 75000, 100000, 250000, 500000, 1000000
  ],

  // Cuántos maletines se abren en cada ronda. La suma debe ser
  // values.length - 2 (queda el propio + uno para el cambio final).
  roundPlan: [6, 5, 4, 3, 2, 1, 1, 1, 1],

  allowFinalSwap: true,
  locale: 'es-ES'
};

// Presets de la banca: cada modo define el rango del factor de oferta
// (arranca tacaño y sube hacia el VE a medida que avanza la partida).
const BANK_PRESETS = {
  normal:   { label: 'Normal',   start: 0.60, end: 0.90 },
  tacana:   { label: 'Tacaña',   start: 0.42, end: 0.72 },
  generosa: { label: 'Generosa', start: 0.72, end: 0.97 }
};

// Estado de los modos elegidos en el panel superior. Se puede tocar mientras
// no arrancó la partida (fase PICK antes de elegir maletín); se bloquea
// apenas se elige el maletín propio.
let modes = {
  bank: 'normal',
  educativo: false,
  perfil: false
};

// Modo de la app: 'basico' | 'tv' | 'seguimiento'.
// 'tv' es puramente estético (no cambia reglas). 'seguimiento' cambia el
// modelo de datos entero: no hay mezcla aleatoria, el usuario va anotando
// a mano qué premio salió de cada maletín mientras mira el programa real.
let appMode = 'basico';

function visualTv() {
  return appMode === 'tv' || appMode === 'seguimiento';
}

function isSeguimiento() {
  return appMode === 'seguimiento';
}

// Guarda el último maletín abierto para dispararle el destello dorado una
// sola vez, en el próximo render, si el modo visual TV está activo.
let pendingFlash = null; // { id, big }

/* ------------------------------------------------------------------------
   MODO SEGUIMIENTO — mismo set de 26 premios que el juego, pero sin mezcla:
   el usuario arma un maletín (lo toca) y después toca el valor que anunció
   el programa para asignárselo. El "VE" y la "oferta estimada" se recalculan
   sobre lo que queda en el pool real, igual que en el juego clásico.
   ------------------------------------------------------------------------ */

function createEmptyTracker() {
  return {
    cases: CONFIG.values.map((_, i) => ({ id: i + 1, value: null, opened: false, isPlayer: false })),
    usedValues: new Set(),
    armedCaseId: null,
    markingPlayer: false,
    playerCaseId: null,
    log: [] // { kind: 'apertura'|'oferta', seq, caseId?, value?, amount?, ve, modelEstimate? }
  };
}

let tracker = createEmptyTracker();

function trackerRemainingValues() {
  return CONFIG.values.filter(v => !tracker.usedValues.has(v));
}

function trackerExpectedValue() {
  const vals = trackerRemainingValues();
  if (!vals.length) return 0;
  return vals.reduce((a, b) => a + b, 0) / vals.length;
}

function trackerBankOffer() {
  const total = CONFIG.values.length;
  const opened = tracker.usedValues.size;
  const progress = opened / (total - 2 || 1);
  const preset = BANK_PRESETS[modes.bank] || BANK_PRESETS.normal;
  const factor = preset.start + Math.min(progress, 1) * (preset.end - preset.start);
  return trackerExpectedValue() * factor;
}

function resetTracker(skipConfirm) {
  const hasProgress = tracker.usedValues.size > 0 || tracker.log.length > 0 || tracker.playerCaseId != null;
  if (hasProgress && !skipConfirm) {
    if (!window.confirm('Esto borra el seguimiento actual. ¿Continuar?')) return;
  }
  tracker = createEmptyTracker();
  setModePanelLocked(false);
  hideAllOverlays();
  renderAll();
}

function armCase(id) {
  const c = tracker.cases.find(item => item.id === id);
  if (!c) return;

  if (tracker.markingPlayer) {
    tracker.cases.forEach(item => { item.isPlayer = false; });
    c.isPlayer = true;
    tracker.playerCaseId = id;
    tracker.markingPlayer = false;
    $('seg-mark-player-btn').classList.remove('active');
    renderAll();
    return;
  }

  if (c.opened) return;
  tracker.armedCaseId = (tracker.armedCaseId === id) ? null : id;
  renderAll();
}

function assignValue(value) {
  if (tracker.armedCaseId == null || tracker.usedValues.has(value)) return;

  const c = tracker.cases.find(item => item.id === tracker.armedCaseId);
  if (!c || c.opened) return;

  c.value = value;
  c.opened = true;
  tracker.usedValues.add(value);
  tracker.log.push({
    kind: 'apertura',
    seq: tracker.log.length + 1,
    caseId: c.id,
    value,
    ve: Math.round(trackerExpectedValue())
  });
  tracker.armedCaseId = null;
  pendingFlash = visualTv() ? { id: c.id, big: value >= 100000 } : null;
  renderAll();
}

function undoLastTrackerAction() {
  const last = tracker.log.pop();
  if (!last) return;

  if (last.kind === 'apertura') {
    const c = tracker.cases.find(item => item.id === last.caseId);
    if (c) { c.value = null; c.opened = false; }
    tracker.usedValues.delete(last.value);
  }
  renderAll();
}

function registerRealOffer(amount) {
  if (!amount || amount <= 0) return;
  tracker.log.push({
    kind: 'oferta',
    seq: tracker.log.length + 1,
    amount: Math.round(amount),
    ve: Math.round(trackerExpectedValue()),
    modelEstimate: Math.round(trackerBankOffer())
  });
  renderAll();
}

function finishTracking() {
  hideAllOverlays();

  const playerC = tracker.cases.find(c => c.isPlayer);
  const revealed = playerC && playerC.opened;

  $('result-title').innerText = 'Resumen del seguimiento';
  $('result-amount').innerText = revealed ? formatMoney(playerC.value) : '—';
  $('result-sub').innerText = revealed
    ? `El maletín del participante (Nº ${playerC.id}) terminó revelado con ${formatMoney(playerC.value)}.`
    : playerC
      ? `El maletín del participante (Nº ${playerC.id}) quedó sin revelar.`
      : 'No se marcó el maletín del participante.';
  $('risk-profile').hidden = true;
  $('play-again-btn').innerText = 'Cerrar resumen';

  const hist = $('result-history');
  const offers = tracker.log.filter(l => l.kind === 'oferta');

  if (!offers.length) {
    hist.innerHTML = '<h3>Ofertas reales registradas</h3>' +
      '<p style="color:#94a3b8;font-size:0.9rem;">No se registró ninguna oferta de la banca.</p>';
  } else {
    hist.innerHTML =
      '<h3>Ofertas reales vs. modelo</h3>' +
      offers.map(o => {
        const diff = o.amount - o.modelEstimate;
        const sign = diff >= 0 ? '+' : '-';
        return `
        <div class="hist-row">
          <span>Real ${formatMoney(o.amount)}</span>
          <span>VE ${formatMoney(o.ve)}</span>
          <span class="hist-offer">Modelo ${formatMoney(o.modelEstimate)}</span>
          <span class="hist-dec ${diff >= 0 ? 'deal' : 'nodeal'}">${sign}${formatMoney(Math.abs(diff))}</span>
        </div>`;
      }).join('');
  }

  $('result-overlay').hidden = false;
}

function setAppMode(newMode) {
  if (newMode === appMode) return;

  const classicProgress = state.playerCaseId != null || state.history.length > 0;
  const trackerProgress = tracker.usedValues.size > 0 || tracker.log.length > 0 || tracker.playerCaseId != null;
  const hasProgress = isSeguimiento() ? trackerProgress : classicProgress;

  if (hasProgress && !window.confirm('Cambiar de modo reinicia lo que llevás hecho hasta ahora. ¿Continuar?')) {
    return;
  }

  appMode = newMode;

  document.querySelectorAll('#app-mode-segmented .seg-btn')
    .forEach(b => b.classList.toggle('active', b.dataset.value === newMode));

  document.body.classList.toggle('theme-tv', visualTv());
  $('seg-toolbar').hidden = !isSeguimiento();

  if (isSeguimiento()) {
    $('round-card-label').innerText = 'Abiertos';
    $('reset-btn').innerText = 'Reiniciar Seguimiento';
    resetTracker(true);
  } else {
    $('round-card-label').innerText = 'Ronda';
    $('reset-btn').innerText = 'Reiniciar Juego';
    $('seg-log').hidden = true;
    initGame();
  }
}

/* ----------------------------- Estado ----------------------------------- */

const PHASE = {
  SHUFFLE: 'shuffle',
  PICK: 'pick',
  OPEN: 'open',
  OFFER: 'offer',
  SWAP: 'swap',
  END: 'end'
};

let state = createEmptyState();
let isShuffling = false;

function createEmptyState() {
  return {
    phase: PHASE.PICK,
    cases: [],          // { id, value, opened, isPlayer }
    playerCaseId: null,
    roundIndex: 0,
    opensLeftInRound: 0,
    history: [],        // { round, offer, ve, decision }
    result: null
  };
}

/* ----------------------------- Utilidades -------------------------------- */

const $ = (id) => document.getElementById(id);

function formatMoney(amount) {
  return '$' + Math.round(amount).toLocaleString(CONFIG.locale);
}

function animateCount(el, from, to, duration) {
  const start = performance.now();
  function step(now) {
    const t = Math.min((now - start) / duration, 1);
    const eased = 1 - Math.pow(1 - t, 3);
    el.innerText = formatMoney(from + (to - from) * eased);
    if (t < 1) requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}

function shuffle(array) {
  const arr = [...array];
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

function unopenedCases() {
  return state.cases.filter(c => !c.opened);
}

function remainingValues() {
  return unopenedCases().map(c => c.value);
}

function revealedValues() {
  return state.cases.filter(c => c.opened).map(c => c.value);
}

function expectedValue() {
  const vals = remainingValues();
  if (!vals.length) return 0;
  return vals.reduce((a, b) => a + b, 0) / vals.length;
}

function bankOffer() {
  const total = CONFIG.values.length;
  const openedCount = total - remainingValues().length;
  const progress = openedCount / (total - 2 || 1);   // 0 al inicio, ~1 al final
  const preset = BANK_PRESETS[modes.bank] || BANK_PRESETS.normal;
  const factor = preset.start + Math.min(progress, 1) * (preset.end - preset.start);
  return expectedValue() * factor;
}

/* --------------------- Estadísticas modo educativo ----------------------- */

function stdDeviationOf(vals) {
  if (!vals.length) return 0;
  const mean = vals.reduce((a, b) => a + b, 0) / vals.length;
  const variance = vals.reduce((acc, v) => acc + (v - mean) ** 2, 0) / vals.length;
  return Math.sqrt(variance);
}

function medianOf(vals) {
  const sorted = [...vals].sort((a, b) => a - b);
  if (!sorted.length) return 0;
  const mid = Math.floor(sorted.length / 2);
  return sorted.length % 2 ? sorted[mid] : (sorted[mid - 1] + sorted[mid]) / 2;
}

function stdDeviation() {
  return stdDeviationOf(remainingValues());
}

function median() {
  return medianOf(remainingValues());
}

/* ------------------------ Perfil de riesgo -------------------------------- */

function computeRiskProfile() {
  const r = state.result;

  if (r.type !== 'deal') {
    return {
      tag: 'Buscador de riesgo',
      detail: 'Jugaste hasta el final sin aceptar ninguna oferta de la banca.'
    };
  }

  const last = state.history[state.history.length - 1];
  const ratio = last && last.ve ? last.offer / last.ve : 0;
  const pct = Math.round(ratio * 100);

  if (ratio < 0.68) {
    return {
      tag: 'Conservador',
      detail: `Aceptaste apenas la oferta llegó al ${pct}% del valor esperado: preferiste la certeza antes que arriesgar.`
    };
  }
  if (ratio < 0.85) {
    return {
      tag: 'Equilibrado',
      detail: `Aceptaste con una oferta del ${pct}% del valor esperado: ni te apuraste ni forzaste el límite.`
    };
  }
  return {
    tag: 'Buscador de riesgo',
    detail: `Esperaste hasta que la oferta llegó al ${pct}% del valor esperado antes de aceptar.`
  };
}

function playerCase() {
  return state.cases.find(c => c.id === state.playerCaseId) || null;
}

function totalRounds() {
  return CONFIG.roundPlan.length;
}

/* ----------------------------- Ciclo de juego ---------------------------- */

function initGame() {
  if (isShuffling) return;

  hideAllOverlays();

  const allCases = document.querySelectorAll('.case');
  const allBadges = document.querySelectorAll('.value-badge');

  if (allCases.length > 0) {
    isShuffling = true;
    state.phase = PHASE.SHUFFLE;
    setStatus('Mezclando maletines...');

    allCases.forEach(el => applyShuffleVars(el, 80, 30));
    allBadges.forEach(el => applyShuffleVars(el, 30, 10));

    setTimeout(() => {
      executeReset();
      isShuffling = false;
    }, 1200);
  } else {
    executeReset();
  }
}

function applyShuffleVars(el, spread, rotSpread) {
  el.style.setProperty('--tx', (Math.random() * spread - spread / 2) + 'px');
  el.style.setProperty('--ty', (Math.random() * spread - spread / 2) + 'px');
  el.style.setProperty('--rot', (Math.random() * rotSpread - rotSpread / 2) + 'deg');
  el.classList.add('shuffling');
}

function executeReset() {
  const shuffledValues = shuffle(CONFIG.values);

  state = createEmptyState();
  state.phase = PHASE.PICK;
  state.cases = CONFIG.values.map((_, i) => ({
    id: i + 1,
    value: shuffledValues[i],
    opened: false,
    isPlayer: false
  }));

  setModePanelLocked(false);
  renderAll();
}

function pickPlayerCase(id) {
  const c = state.cases.find(item => item.id === id);
  if (!c) return;

  c.isPlayer = true;
  state.playerCaseId = id;
  state.phase = PHASE.OPEN;
  state.roundIndex = 0;
  state.opensLeftInRound = CONFIG.roundPlan[0];

  setModePanelLocked(true);
  renderAll();
}

function openCase(id) {
  const c = state.cases.find(item => item.id === id);
  if (!c || c.opened || c.isPlayer) return;

  c.opened = true;
  state.opensLeftInRound--;
  pendingFlash = visualTv() ? { id: c.id, big: c.value >= 100000 } : null;

  // Se corta la interacción antes de mostrar la oferta para que no se
  // abran maletines de más durante la pausa.
  if (state.opensLeftInRound <= 0) state.phase = PHASE.OFFER;

  renderAll();

  if (state.phase === PHASE.OFFER) {
    setTimeout(startOfferPhase, 650);
  }
}

function startOfferPhase() {
  state.phase = PHASE.OFFER;

  const offer = bankOffer();
  const ve = expectedValue();
  const pending = unopenedCases().length - 1; // sin contar el propio

  const subText =
    `Valor esperado del tablero: ${formatMoney(ve)} · ` +
    `La oferta es el ${Math.round((offer / ve) * 100)}% del VE · ` +
    (pending > 0
      ? `Si rechazás, seguís con ${pending} maletín${pending === 1 ? '' : 'es'} en juego.`
      : 'Si rechazás, vas a la decisión final.');

  $('offer-overlay').hidden = false;
  renderStatus();

  if (visualTv()) {
    $('offer-thinking').hidden = false;
    $('offer-amount').style.visibility = 'hidden';
    $('offer-sub').style.visibility = 'hidden';
    $('deal-btn').disabled = true;
    $('nodeal-btn').disabled = true;

    setTimeout(() => {
      $('offer-thinking').hidden = true;
      $('offer-amount').style.visibility = 'visible';
      $('offer-sub').style.visibility = 'visible';
      $('offer-sub').innerText = subText;
      animateCount($('offer-amount'), 0, offer, 700);
      $('deal-btn').disabled = false;
      $('nodeal-btn').disabled = false;
    }, 1100);
  } else {
    $('offer-amount').innerText = formatMoney(offer);
    $('offer-sub').innerText = subText;
  }
}

function acceptOffer() {
  const offer = bankOffer();
  logDecision('Trato', offer);
  finish({
    type: 'deal',
    amount: offer,
    playerValue: playerCase().value
  });
}

function rejectOffer() {
  logDecision('No Trato', bankOffer());
  $('offer-overlay').hidden = true;

  state.roundIndex++;

  if (state.roundIndex >= totalRounds()) {
    startFinalPhase();
  } else {
    state.phase = PHASE.OPEN;
    state.opensLeftInRound = CONFIG.roundPlan[state.roundIndex];
    renderAll();
  }
}

function logDecision(decision, offer) {
  state.history.push({
    round: state.roundIndex + 1,
    offer: Math.round(offer),
    ve: Math.round(expectedValue()),
    decision
  });
}

function startFinalPhase() {
  const other = unopenedCases().find(c => !c.isPlayer);

  if (!CONFIG.allowFinalSwap || !other) {
    finish({ type: 'keep', amount: playerCase().value, playerValue: playerCase().value });
    return;
  }

  state.phase = PHASE.SWAP;
  const mine = playerCase();

  $('swap-sub').innerText =
    `Quedan dos maletines: el tuyo (Nº ${mine.id}) y el Nº ${other.id}. ` +
    `Los premios que siguen vivos son ${formatMoney(Math.min(mine.value, other.value))} y ` +
    `${formatMoney(Math.max(mine.value, other.value))}. ¿Cambiás?`;

  $('swap-overlay').hidden = false;
  renderAll();
}

function resolveFinal(didSwap) {
  const mine = playerCase();
  const other = unopenedCases().find(c => !c.isPlayer);
  const finalCase = didSwap ? other : mine;

  finish({
    type: didSwap ? 'swap' : 'keep',
    amount: finalCase.value,
    playerValue: mine.value,
    otherValue: other ? other.value : null,
    finalCaseId: finalCase.id
  });
}

function finish(result) {
  state.result = result;
  state.phase = PHASE.END;

  // Revelar todo el tablero
  state.cases.forEach(c => { c.opened = true; });

  hideAllOverlays();
  renderBoard();
  renderStatus();
  renderMetrics();
  renderResult();

  $('play-again-btn').innerText = 'Jugar de nuevo';
  $('result-overlay').hidden = false;
}

/* ----------------------------- Render ------------------------------------ */

function renderAll() {
  if (isSeguimiento()) {
    renderTrackerBoard();
    renderTrackerMetrics();
    renderTrackerStatus();
    renderTrackerPlayerBar();
    renderSegLog();
  } else {
    renderBoard();
    renderMetrics();
    renderStatus();
    renderPlayerBar();
  }
}

function renderBoard() {
  const lowList = $('low-values-list');
  const highList = $('high-values-list');
  lowList.innerHTML = '';
  highList.innerHTML = '';

  const gone = revealedValues();
  const half = Math.ceil(CONFIG.values.length / 2);

  const makeBadge = (val, cls) => {
    const badge = document.createElement('div');
    badge.className = `value-badge ${cls} ${gone.includes(val) ? 'eliminated' : ''}`;
    badge.innerText = formatMoney(val);
    return badge;
  };

  CONFIG.values.slice(0, half).forEach(v => lowList.appendChild(makeBadge(v, 'low')));
  CONFIG.values.slice(half).forEach(v => highList.appendChild(makeBadge(v, 'high')));

  const grid = $('cases-grid');
  grid.innerHTML = '';

  const clickable = (state.phase === PHASE.PICK) ||
                    (state.phase === PHASE.OPEN);

  const flash = pendingFlash;

  state.cases.forEach(c => {
    const el = document.createElement('div');
    const classes = ['case'];
    if (c.opened) classes.push('opened');
    if (c.isPlayer) classes.push('player');
    if (state.phase === PHASE.PICK) classes.push('selectable');
    if (flash && flash.id === c.id) classes.push('just-opened-tv');
    el.className = classes.join(' ');

    el.innerHTML = `
      <div class="handle"></div>
      <div class="num">${c.id}</div>
      <div class="val-hidden">${c.opened ? formatMoney(c.value) : ''}</div>
      ${c.isPlayer && !c.opened ? '<div class="mine-tag">TUYO</div>' : ''}
    `;

    if (clickable && !c.opened && !c.isPlayer) {
      el.addEventListener('click', () => {
        if (state.phase === PHASE.PICK) pickPlayerCase(c.id);
        else if (state.phase === PHASE.OPEN) openCase(c.id);
      });
    }

    grid.appendChild(el);
  });

  if (flash) {
    if (flash.big) {
      grid.classList.add('big-hit-tv');
      setTimeout(() => grid.classList.remove('big-hit-tv'), 700);
    }
    pendingFlash = null;
  }
}

function renderMetrics() {
  const count = remainingValues().length;
  $('remaining-count').innerText = count;

  const roundLabel = state.phase === PHASE.PICK
    ? '-'
    : `${Math.min(state.roundIndex + 1, totalRounds())} / ${totalRounds()}`;
  $('round-label').innerText = state.phase === PHASE.END ? 'Fin' : roundLabel;

  if (count === 0) {
    $('ve-val').innerText = '$0';
    $('bank-offer').innerText = '$0';
    return;
  }

  $('ve-val').innerText = formatMoney(expectedValue());
  $('bank-offer').innerText = state.phase === PHASE.PICK ? '—' : formatMoney(bankOffer());

  renderEduRow(remainingValues());
}

function renderEduRow(vals) {
  const row = $('edu-row');
  row.hidden = !modes.educativo;
  if (!modes.educativo) return;

  if (!vals.length) {
    $('edu-std').innerText = '$0';
    $('edu-median').innerText = '$0';
    $('edu-min').innerText = '$0';
    $('edu-max').innerText = '$0';
    return;
  }

  $('edu-std').innerText = formatMoney(stdDeviationOf(vals));
  $('edu-median').innerText = formatMoney(medianOf(vals));
  $('edu-min').innerText = formatMoney(Math.min(...vals));
  $('edu-max').innerText = formatMoney(Math.max(...vals));
}

function renderStatus() {
  switch (state.phase) {
    case PHASE.SHUFFLE:
      setStatus('Mezclando maletines...');
      break;
    case PHASE.PICK:
      setStatus('Elegí tu maletín: es el que te llevás si no aceptás ninguna oferta.');
      break;
    case PHASE.OPEN: {
      const n = state.opensLeftInRound;
      setStatus(`Ronda ${state.roundIndex + 1}: abrí ${n} maletín${n === 1 ? '' : 'es'} más.`);
      break;
    }
    case PHASE.OFFER:
      setStatus('La banca hizo su oferta. ¿Trato o No Trato?');
      break;
    case PHASE.SWAP:
      setStatus('Dos maletines en juego: quedarte o cambiar.');
      break;
    case PHASE.END:
      setStatus('Partida terminada.');
      break;
  }
}

function setStatus(text) {
  $('status-bar').innerText = text;
  const ticker = $('tv-ticker-text');
  if (ticker) ticker.innerText = text;
}

function renderPlayerBar() {
  const bar = $('player-case-bar');
  if (!state.playerCaseId) {
    bar.hidden = true;
    return;
  }
  bar.hidden = false;
  $('player-case-num').innerText = 'Nº ' + state.playerCaseId;
  $('player-case-hint').innerText = state.phase === PHASE.END
    ? `contenía ${formatMoney(playerCase().value)}`
    : 'queda cerrado hasta el final';
}

function renderResult() {
  const r = state.result;
  const mineValue = r.playerValue;

  let title, sub;

  if (r.type === 'deal') {
    title = 'Cerraste trato con la banca';
    const diff = mineValue - r.amount;
    sub = `Tu maletín Nº ${state.playerCaseId} tenía ${formatMoney(mineValue)}. ` +
          (diff > 0
            ? `Dejaste ${formatMoney(diff)} sobre la mesa.`
            : `Le ganaste ${formatMoney(Math.abs(diff))} a la banca.`);
  } else if (r.type === 'keep') {
    title = 'Te quedaste con tu maletín';
    sub = r.otherValue != null
      ? `El otro maletín tenía ${formatMoney(r.otherValue)}.`
      : 'Jugaste hasta el final sin aceptar ninguna oferta.';
  } else {
    title = 'Cambiaste de maletín';
    sub = `Tu maletín original (Nº ${state.playerCaseId}) tenía ${formatMoney(mineValue)}.`;
  }

  const bestOffer = state.history.reduce((max, h) => Math.max(max, h.offer), 0);
  if (bestOffer > 0 && r.type !== 'deal') {
    sub += ` La mejor oferta que rechazaste fue ${formatMoney(bestOffer)}.`;
  }

  $('result-title').innerText = title;
  $('result-amount').innerText = formatMoney(r.amount);
  $('result-sub').innerText = sub;

  const riskBlock = $('risk-profile');
  if (modes.perfil) {
    const profile = computeRiskProfile();
    $('risk-tag').innerText = profile.tag;
    $('risk-detail').innerText = profile.detail;
    riskBlock.hidden = false;
  } else {
    riskBlock.hidden = true;
  }

  const hist = $('result-history');
  if (!state.history.length) {
    hist.innerHTML = '';
    return;
  }

  hist.innerHTML =
    '<h3>Historial de ofertas</h3>' +
    state.history.map(h => `
      <div class="hist-row">
        <span>Ronda ${h.round}</span>
        <span>VE ${formatMoney(h.ve)}</span>
        <span class="hist-offer">${formatMoney(h.offer)}</span>
        <span class="hist-dec ${h.decision === 'Trato' ? 'deal' : 'nodeal'}">${h.decision}</span>
      </div>`).join('');
}

/* --------------------------- Render: seguimiento -------------------------- */

function renderTrackerBoard() {
  const lowList = $('low-values-list');
  const highList = $('high-values-list');
  lowList.innerHTML = '';
  highList.innerHTML = '';

  const half = Math.ceil(CONFIG.values.length / 2);
  const pickMode = tracker.armedCaseId != null;

  const makeBadge = (val, cls) => {
    const badge = document.createElement('div');
    const isUsed = tracker.usedValues.has(val);
    const classes = ['value-badge', cls];
    if (isUsed) classes.push('eliminated');
    if (pickMode && !isUsed) classes.push('pickable');
    badge.className = classes.join(' ');
    badge.innerText = formatMoney(val);

    if (pickMode && !isUsed) {
      badge.addEventListener('click', () => assignValue(val));
    }
    return badge;
  };

  CONFIG.values.slice(0, half).forEach(v => lowList.appendChild(makeBadge(v, 'low')));
  CONFIG.values.slice(half).forEach(v => highList.appendChild(makeBadge(v, 'high')));

  const grid = $('cases-grid');
  grid.innerHTML = '';
  const flash = pendingFlash;

  tracker.cases.forEach(c => {
    const el = document.createElement('div');
    const classes = ['case'];
    if (c.opened) classes.push('opened');
    if (c.isPlayer) classes.push('player');
    if (tracker.armedCaseId === c.id) classes.push('armed-seg');
    if (tracker.markingPlayer && !c.opened) classes.push('selectable');
    if (flash && flash.id === c.id) classes.push('just-opened-tv');
    el.className = classes.join(' ');

    el.innerHTML = `
      <div class="handle"></div>
      <div class="num">${c.id}</div>
      <div class="val-hidden">${c.opened ? formatMoney(c.value) : ''}</div>
      ${c.isPlayer ? '<div class="mine-tag">TUYO</div>' : ''}
    `;

    el.addEventListener('click', () => armCase(c.id));
    grid.appendChild(el);
  });

  if (flash) {
    if (flash.big) {
      grid.classList.add('big-hit-tv');
      setTimeout(() => grid.classList.remove('big-hit-tv'), 700);
    }
    pendingFlash = null;
  }
}

function renderTrackerMetrics() {
  const vals = trackerRemainingValues();
  $('remaining-count').innerText = vals.length;
  $('round-label').innerText = tracker.usedValues.size;

  if (!vals.length) {
    $('ve-val').innerText = '$0';
    $('bank-offer').innerText = '$0';
  } else {
    $('ve-val').innerText = formatMoney(trackerExpectedValue());
    $('bank-offer').innerText = formatMoney(trackerBankOffer());
  }

  renderEduRow(vals);
}

function renderTrackerStatus() {
  let text;
  if (tracker.markingPlayer) {
    text = 'Tocá el maletín del participante en el tablero.';
  } else if (tracker.armedCaseId != null) {
    text = `Maletín Nº ${tracker.armedCaseId} armado: tocá el valor que salió al aire.`;
  } else {
    text = 'Tocá un maletín y después el valor que anunciaron en el programa.';
  }
  setStatus(text);
}

function renderTrackerPlayerBar() {
  const bar = $('player-case-bar');
  if (tracker.playerCaseId == null) {
    bar.hidden = true;
    return;
  }
  const c = tracker.cases.find(item => item.id === tracker.playerCaseId);
  bar.hidden = false;
  $('player-case-num').innerText = 'Nº ' + tracker.playerCaseId;
  $('player-case-hint').innerText = (c && c.opened)
    ? `revelado: ${formatMoney(c.value)}`
    : 'aún sin revelar';
}

function renderSegLog() {
  const panel = $('seg-log');
  const body = $('seg-log-body');

  if (!tracker.log.length) {
    panel.hidden = true;
    body.innerHTML = '';
    return;
  }

  panel.hidden = false;
  body.innerHTML = tracker.log.map(entry => {
    if (entry.kind === 'apertura') {
      return `
        <div class="hist-row">
          <span>#${entry.seq}</span>
          <span>Maletín Nº ${entry.caseId}</span>
          <span class="hist-offer">${formatMoney(entry.value)}</span>
          <span>VE ${formatMoney(entry.ve)}</span>
        </div>`;
    }
    const diff = entry.amount - entry.modelEstimate;
    const sign = diff >= 0 ? '+' : '-';
    return `
      <div class="hist-row">
        <span>#${entry.seq}</span>
        <span>Oferta real ${formatMoney(entry.amount)}</span>
        <span class="hist-offer">Modelo ${formatMoney(entry.modelEstimate)}</span>
        <span class="hist-dec ${diff >= 0 ? 'deal' : 'nodeal'}">${sign}${formatMoney(Math.abs(diff))}</span>
      </div>`;
  }).join('');
}

function setModePanelLocked(locked) {
  const panel = $('mode-panel');
  panel.classList.toggle('locked', locked);

  panel.querySelectorAll('.seg-btn').forEach(btn => { btn.disabled = locked; });
  panel.querySelectorAll('input[type="checkbox"]').forEach(input => { input.disabled = locked; });
}

function hideAllOverlays() {
  ['offer-overlay', 'swap-overlay', 'result-overlay'].forEach(id => { $(id).hidden = true; });
  $('offer-thinking').hidden = true;
  $('offer-amount').style.visibility = 'visible';
  $('offer-sub').style.visibility = 'visible';
  $('deal-btn').disabled = false;
  $('nodeal-btn').disabled = false;
}

/* ----------------------------- Eventos ----------------------------------- */

document.addEventListener('DOMContentLoaded', () => {
  $('reset-btn').addEventListener('click', () => {
    if (isSeguimiento()) resetTracker();
    else initGame();
  });

  $('play-again-btn').addEventListener('click', () => {
    if (isSeguimiento()) {
      $('result-overlay').hidden = true; // cierra el resumen, no borra lo registrado
    } else {
      initGame();
    }
  });

  $('deal-btn').addEventListener('click', acceptOffer);
  $('nodeal-btn').addEventListener('click', rejectOffer);
  $('keep-btn').addEventListener('click', () => resolveFinal(false));
  $('swap-btn').addEventListener('click', () => resolveFinal(true));

  // Selector de banca (3 posiciones)
  document.querySelectorAll('#bank-segmented .seg-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      if (btn.disabled) return;
      modes.bank = btn.dataset.value;
      document.querySelectorAll('#bank-segmented .seg-btn')
        .forEach(b => b.classList.toggle('active', b === btn));
      if (isSeguimiento()) renderTrackerMetrics();
      else renderMetrics();
    });
  });

  // Switches de modo educativo y perfil de riesgo
  $('toggle-educativo').addEventListener('change', (e) => {
    modes.educativo = e.target.checked;
    if (isSeguimiento()) renderEduRow(trackerRemainingValues());
    else renderEduRow(remainingValues());
  });
  $('toggle-perfil').addEventListener('change', (e) => {
    modes.perfil = e.target.checked;
  });

  // Selector de Modo (Básico / TV / Seguimiento)
  document.querySelectorAll('#app-mode-segmented .seg-btn').forEach(btn => {
    btn.addEventListener('click', () => setAppMode(btn.dataset.value));
  });

  // Toolbar de seguimiento
  $('seg-mark-player-btn').addEventListener('click', () => {
    tracker.markingPlayer = !tracker.markingPlayer;
    tracker.armedCaseId = null;
    $('seg-mark-player-btn').classList.toggle('active', tracker.markingPlayer);
    renderAll();
  });

  $('seg-register-offer-btn').addEventListener('click', () => {
    const input = $('seg-real-offer-input');
    const amount = parseFloat(input.value);
    if (!amount || amount <= 0) return;
    registerRealOffer(amount);
    input.value = '';
  });

  $('seg-real-offer-input').addEventListener('keydown', (e) => {
    if (e.key === 'Enter') $('seg-register-offer-btn').click();
  });

  $('seg-undo-btn').addEventListener('click', undoLastTrackerAction);
  $('seg-finish-btn').addEventListener('click', finishTracking);

  initGame();
});

```

### Archivo: `styles.css`
```css
:root {
  --bg-gradient: radial-gradient(circle at center, #1b2838 0%, #0b101d 100%);
  --card-bg: rgba(20, 30, 48, 0.75);
  --card-border: rgba(255, 215, 0, 0.25);
  --gold: #ffd700;
  --gold-glow: rgba(255, 215, 0, 0.4);
  --cyan: #00f0ff;
  --cyan-glow: rgba(0, 240, 255, 0.4);
  --green: #10b981;
  --red-val: #ef4444;
  --text-main: #f8fafc;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Rajdhani', sans-serif;
  background: var(--bg-gradient);
  color: var(--text-main);
  min-height: 100vh;
  padding: 20px;
  background-attachment: fixed;
}

.page-layout {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  gap: 24px;
  width: 100%;
}

.side-rail {
  flex: 0 0 230px;
  width: 230px;
}

.center-col {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex: 0 1 1300px;
  min-width: 0;
}

h1 {
  font-family: 'Orbitron', sans-serif;
  font-size: 2.2rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 2px;
  background: linear-gradient(180deg, #ffffff 30%, #ffd700 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
  text-shadow: 0 0 20px rgba(255, 215, 0, 0.3);
  margin-bottom: 20px;
  text-align: center;
}

/* ------------------------------ Panel de modos ---------------------------- */

.mode-panel {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  gap: 16px;
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  border-radius: 12px;
  padding: 16px 18px;
  backdrop-filter: blur(10px);
  transition: opacity 0.25s ease;
  position: sticky;
  top: 20px;
}

.mode-panel.locked {
  opacity: 0.55;
}

.mode-group {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  gap: 8px;
}

.mode-group-label {
  font-family: 'Orbitron', sans-serif;
  font-size: 0.7rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #94a3b8;
}

.segmented {
  display: flex;
  flex-wrap: wrap;
  background: rgba(15, 23, 42, 0.6);
  border: 1px solid rgba(148, 163, 184, 0.25);
  border-radius: 999px;
  padding: 3px;
  gap: 2px;
}

.seg-btn {
  flex: 1 1 auto;
  font-family: 'Rajdhani', sans-serif;
  font-weight: 700;
  font-size: 0.78rem;
  letter-spacing: 0.5px;
  padding: 6px 8px;
  border-radius: 999px;
  border: none;
  background: transparent;
  color: #94a3b8;
  cursor: pointer;
  transition: background 0.2s ease, color 0.2s ease;
}

.seg-btn:not(:disabled):hover {
  color: #e2e8f0;
}

.seg-btn.active {
  background: linear-gradient(135deg, var(--gold) 0%, #b45309 100%);
  color: #1a1202;
  box-shadow: 0 2px 8px rgba(255, 215, 0, 0.35);
}

.seg-btn:disabled {
  cursor: default;
}

.switch-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  cursor: pointer;
  user-select: none;
}

.switch-label {
  font-family: 'Rajdhani', sans-serif;
  font-weight: 600;
  font-size: 0.9rem;
  color: #cbd5e1;
}

.switch {
  position: relative;
  display: inline-block;
  width: 42px;
  height: 24px;
  flex-shrink: 0;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.switch .slider {
  position: absolute;
  inset: 0;
  background: rgba(148, 163, 184, 0.3);
  border: 1px solid rgba(148, 163, 184, 0.4);
  border-radius: 999px;
  transition: background 0.2s ease, border-color 0.2s ease;
}

.switch .slider::before {
  content: '';
  position: absolute;
  width: 18px;
  height: 18px;
  left: 2px;
  top: 2px;
  background: #f8fafc;
  border-radius: 50%;
  transition: transform 0.2s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
}

.switch input:checked + .slider {
  background: linear-gradient(135deg, var(--green) 0%, #047857 100%);
  border-color: #34d399;
}

.switch input:checked + .slider::before {
  transform: translateX(18px);
}

/* --------------------------- Fila modo educativo -------------------------- */

.edu-row {
  display: flex;
  flex-direction: column;
  gap: 10px;
  position: sticky;
  top: 20px;
}

.edu-metric {
  background: rgba(0, 240, 255, 0.06);
  border: 1px solid rgba(0, 240, 255, 0.2);
  border-radius: 10px;
  padding: 10px 14px;
  text-align: center;
}

.edu-metric span {
  display: block;
  font-family: 'Orbitron', sans-serif;
  font-size: 0.65rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #94a3b8;
  margin-bottom: 3px;
}

.edu-metric strong {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.05rem;
  color: var(--cyan);
  text-shadow: 0 0 8px var(--cyan-glow);
}

/* --------------------------- Perfil de riesgo ------------------------------ */

.risk-profile {
  background: rgba(255, 215, 0, 0.06);
  border: 1px solid rgba(255, 215, 0, 0.25);
  border-radius: 10px;
  padding: 12px 16px;
  margin-bottom: 18px;
  text-align: left;
}

.risk-tag {
  display: inline-block;
  font-family: 'Orbitron', sans-serif;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #1a1202;
  background: var(--gold);
  padding: 4px 10px;
  border-radius: 6px;
  margin-bottom: 6px;
}

.risk-profile p {
  font-size: 0.9rem;
  color: #cbd5e1;
  line-height: 1.4;
}

.dashboard {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  width: 100%;
  max-width: 1300px;
  margin-bottom: 18px;
}

.metric-card {
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  border-radius: 12px;
  padding: 16px 20px;
  text-align: center;
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5);
  position: relative;
  overflow: hidden;
  transition: border-color 0.3s, transform 0.3s;
}

.metric-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
  background: linear-gradient(90deg, transparent, var(--gold), transparent);
}

.metric-card h3 {
  font-family: 'Orbitron', sans-serif;
  font-size: 0.85rem;
  color: #94a3b8;
  letter-spacing: 1px;
  margin-bottom: 6px;
  text-transform: uppercase;
}

.metric-card .value {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.8rem;
  font-weight: 700;
  color: var(--cyan);
  text-shadow: 0 0 10px var(--cyan-glow);
}

.metric-card .value.offer {
  color: var(--gold);
  text-shadow: 0 0 12px var(--gold-glow);
}

/* ---------------------- Barra de estado y maletín propio ---------------- */

.status-bar {
  font-family: 'Rajdhani', sans-serif;
  font-size: 1.05rem;
  font-weight: 600;
  letter-spacing: 0.5px;
  color: #cbd5e1;
  background: rgba(15, 23, 42, 0.6);
  border: 1px solid rgba(148, 163, 184, 0.2);
  border-radius: 999px;
  padding: 8px 22px;
  margin-bottom: 14px;
  text-align: center;
}

.player-case-bar {
  display: flex;
  align-items: center;
  gap: 12px;
  background: rgba(16, 185, 129, 0.1);
  border: 1px solid rgba(16, 185, 129, 0.45);
  border-radius: 10px;
  padding: 8px 18px;
  margin-bottom: 18px;
}

.player-case-bar .label {
  font-family: 'Orbitron', sans-serif;
  font-size: 0.75rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #94a3b8;
}

.player-case-bar .num {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.15rem;
  font-weight: 900;
  color: var(--green);
  text-shadow: 0 0 10px rgba(16, 185, 129, 0.5);
}

.player-case-bar .hint {
  font-size: 0.95rem;
  color: #94a3b8;
}

.main-container {
  display: grid;
  grid-template-columns: 220px 1fr 220px;
  gap: 24px;
  width: 100%;
  max-width: 1300px;
}

.values-column {
  display: flex;
  flex-direction: column;
  gap: 7px;
}

.value-badge {
  font-family: 'Orbitron', sans-serif;
  padding: 9px 12px;
  border-radius: 8px;
  font-weight: 700;
  font-size: 0.95rem;
  text-align: center;
  letter-spacing: 1px;
  transition: all 0.5s ease-out;
  position: relative;
  overflow: hidden;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.15), 0 3px 6px rgba(0,0,0,0.3);
}

.value-badge.low {
  background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
  border: 1px solid #334155;
  color: #93c5fd;
}

.value-badge.high {
  background: linear-gradient(135deg, #b45309 0%, #78350f 100%);
  border: 1px solid #f59e0b;
  color: #fef08a;
  text-shadow: 0 0 8px rgba(245, 158, 11, 0.4);
}

.value-badge.eliminated {
  background: rgba(15, 23, 42, 0.4) !important;
  border-color: rgba(255, 255, 255, 0.05) !important;
  color: #334155 !important;
  text-shadow: none !important;
  opacity: 0.4;
  transform: scale(0.96);
}

.value-badge.eliminated::after {
  content: '';
  position: absolute;
  top: 50%; left: 10%; right: 10%;
  height: 2px;
  background: var(--red-val);
  transform: translateY(-50%);
  box-shadow: 0 0 6px var(--red-val);
}

.cases-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(105px, 1fr));
  gap: 14px;
  align-content: start;
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  border-radius: 16px;
  padding: 20px;
  backdrop-filter: blur(10px);
  box-shadow: inset 0 0 30px rgba(0, 0, 0, 0.5);
}

.case {
  position: relative;
  aspect-ratio: 1.25;
  background: linear-gradient(135deg, #382d18 0%, #8a6a1f 35%, #ffd700 50%, #8a6a1f 65%, #2a200f 100%);
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  user-select: none;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.6), inset 0 1px 1px rgba(255, 255, 255, 0.6);
  border: 1px solid #ffe066;
  transition: transform 0.6s cubic-bezier(0.2, 0.8, 0.2, 1), opacity 0.5s ease;
}

.case::before {
  content: '';
  position: absolute;
  top: 3px; left: 3px; right: 3px; bottom: 3px;
  border: 1px solid rgba(0, 0, 0, 0.3);
  border-radius: 7px;
  pointer-events: none;
}

.case:hover:not(.opened):not(.shuffling):not(.player) {
  transform: translateY(-5px) scale(1.03);
  box-shadow: 0 12px 20px rgba(0, 0, 0, 0.7), 0 0 15px var(--gold-glow);
}

.case.selectable:not(.opened) {
  animation: pickPulse 1.8s ease-in-out infinite;
}

@keyframes pickPulse {
  0%, 100% { box-shadow: 0 6px 12px rgba(0,0,0,0.6), inset 0 1px 1px rgba(255,255,255,0.6); }
  50%      { box-shadow: 0 6px 12px rgba(0,0,0,0.6), 0 0 18px var(--cyan-glow); }
}

.case .handle {
  position: absolute;
  top: -6px;
  width: 36px;
  height: 8px;
  background: linear-gradient(90deg, #444, #ccc, #444);
  border-radius: 3px 3px 0 0;
  border: 1px solid #111;
}

.case .num {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.4rem;
  font-weight: 900;
  color: #1a1202;
  text-shadow: 0 1px 0 rgba(255, 255, 255, 0.4);
  z-index: 2;
}

.case .val-hidden {
  display: none;
}

/* Maletín propio */
.case.player:not(.opened) {
  cursor: default;
  border-color: var(--green);
  box-shadow: 0 0 0 2px rgba(16, 185, 129, 0.6), 0 8px 18px rgba(0, 0, 0, 0.6);
}

.case .mine-tag {
  position: absolute;
  bottom: 6px;
  font-family: 'Orbitron', sans-serif;
  font-size: 0.6rem;
  font-weight: 900;
  letter-spacing: 1px;
  color: #06281d;
  background: var(--green);
  padding: 2px 7px;
  border-radius: 4px;
  z-index: 3;
}

.case.opened {
  background: linear-gradient(135deg, #0d131f 0%, #172033 100%);
  border-color: #1e293b;
  box-shadow: inset 0 3px 10px rgba(0, 0, 0, 0.8);
  cursor: default;
  transform: none;
}

.case.opened.player {
  border-color: var(--green);
  box-shadow: inset 0 3px 10px rgba(0, 0, 0, 0.8), 0 0 0 2px rgba(16, 185, 129, 0.5);
}

.case.opened .handle {
  background: #222;
  border-color: #000;
}

.case.opened .num {
  font-size: 0.85rem;
  color: #475569;
  margin-bottom: 2px;
  text-shadow: none;
}

.case.opened .val-hidden {
  display: block;
  font-family: 'Orbitron', sans-serif;
  font-size: 0.85rem;
  font-weight: 700;
  color: var(--red-val);
  text-shadow: 0 0 8px rgba(239, 68, 68, 0.4);
  animation: popIn 0.3s ease-out;
}

.case.opened.player .val-hidden {
  color: var(--green);
  text-shadow: 0 0 8px rgba(16, 185, 129, 0.5);
}

@keyframes popIn {
  0% { transform: scale(0.5); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}

/* ---------------------------- Overlays / paneles ------------------------- */

.overlay {
  position: fixed;
  inset: 0;
  background: rgba(3, 7, 18, 0.82);
  backdrop-filter: blur(6px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 50;
  padding: 20px;
  animation: fadeIn 0.25s ease-out;
}

.overlay[hidden] { display: none; }

@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.panel {
  background: linear-gradient(160deg, #14213a 0%, #0b101d 100%);
  border: 1px solid var(--card-border);
  border-radius: 16px;
  padding: 32px 36px;
  max-width: 480px;
  width: 100%;
  text-align: center;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.8), 0 0 40px rgba(255, 215, 0, 0.08);
  animation: popIn 0.3s ease-out;
}

.panel.wide { max-width: 620px; }

.panel h2 {
  font-family: 'Orbitron', sans-serif;
  font-size: 1.1rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: #94a3b8;
  margin-bottom: 14px;
}

.big-amount {
  font-family: 'Orbitron', sans-serif;
  font-size: 2.6rem;
  font-weight: 900;
  color: var(--gold);
  text-shadow: 0 0 24px var(--gold-glow);
  margin-bottom: 14px;
  word-break: break-word;
}

.panel-sub {
  font-size: 1rem;
  color: #cbd5e1;
  line-height: 1.5;
  margin-bottom: 22px;
}

.panel-actions {
  display: flex;
  gap: 14px;
  justify-content: center;
  flex-wrap: wrap;
}

.btn {
  font-family: 'Orbitron', sans-serif;
  padding: 13px 26px;
  font-size: 0.9rem;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: uppercase;
  border-radius: 8px;
  cursor: pointer;
  border: 1px solid transparent;
  transition: transform 0.15s ease, box-shadow 0.2s ease, background 0.2s ease;
  color: #fff;
}

.btn:hover { transform: translateY(-2px); }

.btn-deal {
  background: linear-gradient(135deg, #10b981 0%, #047857 100%);
  border-color: #34d399;
  box-shadow: 0 4px 15px rgba(16, 185, 129, 0.4);
}

.btn-nodeal {
  background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%);
  border-color: #f87171;
  box-shadow: 0 4px 15px rgba(220, 38, 38, 0.4);
}

.btn-keep {
  background: linear-gradient(135deg, #334155 0%, #1e293b 100%);
  border-color: #64748b;
}

.btn-swap {
  background: linear-gradient(135deg, #b45309 0%, #78350f 100%);
  border-color: #f59e0b;
  box-shadow: 0 4px 15px rgba(245, 158, 11, 0.35);
}

/* Historial del resultado */
.result-history {
  text-align: left;
  margin: 0 0 22px;
  max-height: 220px;
  overflow-y: auto;
}

.result-history h3 {
  font-family: 'Orbitron', sans-serif;
  font-size: 0.75rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #64748b;
  margin-bottom: 8px;
}

.hist-row {
  display: grid;
  grid-template-columns: 1fr 1.2fr 1.2fr 0.9fr;
  gap: 8px;
  font-size: 0.9rem;
  padding: 6px 8px;
  border-bottom: 1px solid rgba(148, 163, 184, 0.12);
  color: #94a3b8;
}

.hist-offer { color: var(--gold); font-weight: 700; }
.hist-dec.deal { color: var(--green); font-weight: 700; }
.hist-dec.nodeal { color: var(--red-val); font-weight: 700; }

.controls {
  margin-top: 30px;
  display: flex;
  gap: 15px;
}

.reset-btn {
  font-family: 'Orbitron', sans-serif;
  padding: 12px 30px;
  font-size: 0.95rem;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: uppercase;
  background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%);
  color: white;
  border: 1px solid #f87171;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 4px 15px rgba(220, 38, 38, 0.4);
  transition: all 0.2s ease;
}

.reset-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(220, 38, 38, 0.6);
  background: linear-gradient(135deg, #ef4444 0%, #b91c1c 100%);
}

/* Animación de mezcla continua en bucle */
.shuffling {
  animation: shuffleLoop 0.35s ease-in-out infinite alternate;
  z-index: 10;
  pointer-events: none;
}

@keyframes shuffleLoop {
  0% {
    transform: translate(var(--tx, 15px), var(--ty, -20px)) rotate(var(--rot, 12deg)) scale(1.08);
  }
  100% {
    transform: translate(calc(var(--tx, 15px) * -1), calc(var(--ty, -20px) * -1)) rotate(calc(var(--rot, 12deg) * -1)) scale(0.92);
  }
}

@media (max-width: 1300px) {
  .page-layout {
    flex-direction: column;
    align-items: center;
  }
  .side-rail {
    width: 100%;
    max-width: 700px;
    flex: none;
  }
  .mode-panel,
  .edu-row {
    position: static;
  }
  .mode-panel {
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
  }
  .mode-group {
    flex-direction: row;
    align-items: center;
  }
  .edu-row {
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
  }
  .edu-row .edu-metric {
    flex: 1 1 130px;
  }
}

/* ============================================================================
   MODO TV — puramente estético. Todo vive atrás de body.theme-tv, así que el
   modo clásico (por defecto) queda pixel a pixel como estaba antes de esto.
   ============================================================================ */

.tv-ticker,
.tv-toggle-fab {
  position: fixed;
  z-index: 60;
}

.tv-toggle-fab {
  top: 14px;
  right: 14px;
  display: flex;
  align-items: center;
  gap: 10px;
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  border-radius: 999px;
  padding: 6px 14px;
  backdrop-filter: blur(10px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.5);
}

.tv-toggle-fab .mode-group-label {
  white-space: nowrap;
}

.tv-toggle-fab .switch-row {
  gap: 8px;
}

.tv-toggle-fab .switch-label {
  font-family: 'Orbitron', sans-serif;
  font-size: 0.7rem;
  letter-spacing: 1px;
  text-transform: uppercase;
}

/* --------------------------- Toolbar de seguimiento ------------------------ */

.seg-toolbar {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: 10px;
  width: 100%;
  max-width: 1300px;
  margin-bottom: 14px;
}

.btn-sm {
  padding: 8px 16px;
  font-size: 0.75rem;
}

.btn-sm.active {
  outline: 2px solid var(--cyan);
  outline-offset: 2px;
}

.seg-offer-input {
  display: flex;
  gap: 6px;
}

.seg-offer-input input {
  width: 140px;
  font-family: 'Rajdhani', sans-serif;
  font-weight: 600;
  font-size: 0.9rem;
  background: rgba(15, 23, 42, 0.6);
  border: 1px solid rgba(148, 163, 184, 0.3);
  border-radius: 8px;
  padding: 0 10px;
  color: var(--text-main);
}

.seg-offer-input input::placeholder {
  color: #64748b;
}

.seg-log {
  width: 100%;
  max-width: 1300px;
  max-height: none;
  margin: 0 0 16px;
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  border-radius: 12px;
  padding: 14px 18px;
}

.seg-log h3 {
  font-family: 'Orbitron', sans-serif;
  font-size: 0.75rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #64748b;
  margin-bottom: 8px;
}

/* Maletín "armado" esperando que le toques el valor */
.case.armed-seg {
  animation: armedPulse 1.2s ease-in-out infinite;
}

@keyframes armedPulse {
  0%, 100% { box-shadow: 0 6px 12px rgba(0,0,0,0.6), 0 0 0 2px var(--cyan); }
  50%      { box-shadow: 0 6px 12px rgba(0,0,0,0.6), 0 0 22px 4px var(--cyan-glow); }
}

/* Badges de valor clickeables en modo seguimiento (esperando asignación) */
.value-badge.pickable {
  cursor: pointer;
  animation: pickablePulse 1.2s ease-in-out infinite;
}

@keyframes pickablePulse {
  0%, 100% { outline: 2px solid transparent; }
  50%      { outline: 2px solid var(--cyan); outline-offset: 2px; }
}

/* Ticker "EN VIVO": solo visible en modo TV */
.tv-ticker {
  top: 0;
  left: 50%;
  transform: translate(-50%, -100%);
  display: flex;
  align-items: center;
  gap: 8px;
  background: linear-gradient(90deg, #7f1d1d 0%, #991b1b 50%, #7f1d1d 100%);
  border: 1px solid #f87171;
  border-top: none;
  border-radius: 0 0 10px 10px;
  padding: 5px 18px;
  font-family: 'Orbitron', sans-serif;
  font-size: 0.7rem;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: #fff;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.5);
  transition: transform 0.35s ease;
  max-width: 90vw;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.tv-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #fff;
  flex-shrink: 0;
  animation: tvDotBlink 1.2s ease-in-out infinite;
}

@keyframes tvDotBlink {
  0%, 100% { opacity: 1; }
  50%      { opacity: 0.2; }
}

.tv-sep {
  opacity: 0.6;
}

body.theme-tv .tv-ticker {
  transform: translate(-50%, 0);
}

body.theme-tv .page-layout {
  margin-top: 26px;
}

/* Focos de escenario + viñeta, animados detrás de todo */
body.theme-tv {
  position: relative;
  overflow-x: hidden;
}

body.theme-tv::before,
body.theme-tv::after {
  content: '';
  position: fixed;
  inset: -10%;
  z-index: -1;
  pointer-events: none;
}

body.theme-tv::before {
  background:
    radial-gradient(circle at 15% 20%, rgba(255, 215, 0, 0.12), transparent 32%),
    radial-gradient(circle at 85% 15%, rgba(0, 240, 255, 0.1), transparent 32%),
    radial-gradient(circle at 50% 90%, rgba(255, 215, 0, 0.08), transparent 35%);
  animation: tvSpotlights 14s ease-in-out infinite alternate;
}

@keyframes tvSpotlights {
  0%   { transform: translate(0, 0) scale(1); }
  50%  { transform: translate(2%, 3%) scale(1.08); }
  100% { transform: translate(-2%, -2%) scale(1); }
}

body.theme-tv::after {
  background: radial-gradient(ellipse at 50% -10%, transparent 40%, rgba(3, 7, 18, 0.85) 85%);
}

/* Botones de decisión con brillo pulsante */
body.theme-tv .btn-deal,
body.theme-tv .btn-nodeal {
  animation: tvBtnPulse 1.6s ease-in-out infinite;
}

@keyframes tvBtnPulse {
  0%, 100% { box-shadow: 0 4px 15px rgba(255, 215, 0, 0.15); }
  50%      { box-shadow: 0 4px 26px rgba(255, 215, 0, 0.55); }
}

/* Suspenso "la banca está pensando" */
.thinking {
  font-family: 'Orbitron', sans-serif;
  font-size: 1rem;
  color: var(--gold);
  letter-spacing: 1px;
  margin-bottom: 14px;
}

.thinking .dots span {
  animation: thinkingDot 1.2s infinite;
  opacity: 0;
}

.thinking .dots span:nth-child(2) { animation-delay: 0.2s; }
.thinking .dots span:nth-child(3) { animation-delay: 0.4s; }

@keyframes thinkingDot {
  0%, 20%  { opacity: 0; }
  40%      { opacity: 1; }
  100%     { opacity: 0; }
}

/* Destello al abrir un maletín (solo modo TV) */
@keyframes tvCaseFlash {
  0%   { box-shadow: 0 6px 12px rgba(0, 0, 0, 0.6), 0 0 0 rgba(255, 215, 0, 0); }
  35%  { box-shadow: 0 6px 12px rgba(0, 0, 0, 0.6), 0 0 40px 8px rgba(255, 215, 0, 0.8); }
  100% { box-shadow: inset 0 3px 10px rgba(0, 0, 0, 0.8), 0 0 0 rgba(255, 215, 0, 0); }
}

.case.just-opened-tv {
  animation: tvCaseFlash 0.7s ease-out;
}

/* Pulso de pantalla para premios altos ($100.000+) */
@keyframes tvBigHit {
  0%, 100% { box-shadow: inset 0 0 30px rgba(0, 0, 0, 0.5); }
  40%      { box-shadow: inset 0 0 30px rgba(0, 0, 0, 0.5), 0 0 60px 10px rgba(255, 215, 0, 0.5); }
}

.cases-grid.big-hit-tv {
  animation: tvBigHit 0.7s ease-out;
}

@media (max-width: 900px) {
  .tv-toggle-fab {
    top: 8px;
    right: 8px;
    padding: 5px 10px;
    flex-wrap: wrap;
    max-width: 200px;
    justify-content: center;
  }
  .tv-ticker {
    font-size: 0.62rem;
    padding: 4px 12px;
  }
}

@media (max-width: 900px) {
  .main-container {
    grid-template-columns: 1fr;
  }
  .dashboard {
    grid-template-columns: 1fr;
  }
  .values-column {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(110px, 1fr));
  }
}

```

