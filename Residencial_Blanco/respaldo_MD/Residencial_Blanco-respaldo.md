# Bundle del Proyecto para IA: Residencial_Blanco

## Estructura de Archivos
- css/formatos.css
- css/formatos/_comun.css
- css/formatos/f1-editorial.css
- css/formatos/f2-minimal.css
- css/formatos/f3-cinematico.css
- css/formatos/f4-revista.css
- css/formatos/f5-jardin.css
- css/formatos/f6-brutalista.css
- css/formatos/f7-app.css
- css/styles.css
- documentacion/LEEME.md
- index.html
- js/config.js
- js/main.js
- mantenimiento-config.html
- netlify.toml
- readme.md

---

## Código Fuente

### Archivo: `css/formatos.css`
```css
/*
 * FORMATOS ALTERNATIVOS DE INTERFAZ (1 al 5)
 * El formato 0 (Original) vive solo en styles.css y no se redefine aquí.
 * Cada formato usa body[data-ui-style="N"]. Mantener este archivo cargado DESPUÉS de styles.css.
 */

/* ---------- Base común de los formatos 1 al 5 ---------- */
body[data-ui-style="1"],
body[data-ui-style="2"],
body[data-ui-style="3"],
body[data-ui-style="4"],
body[data-ui-style="5"] {
    overflow-x: hidden;
}

body[data-ui-style="1"] section,
body[data-ui-style="2"] section,
body[data-ui-style="3"] section,
body[data-ui-style="4"] section,
body[data-ui-style="5"] section {
    scroll-margin-top: 84px;
}

/* Los textos largos se parten por palabra completa, no letra a letra. */
body[data-ui-style="1"] h1,
body[data-ui-style="2"] h1,
body[data-ui-style="3"] h1,
body[data-ui-style="4"] h1,
body[data-ui-style="5"] h1,
body[data-ui-style="1"] h2,
body[data-ui-style="2"] h2,
body[data-ui-style="3"] h2,
body[data-ui-style="4"] h2,
body[data-ui-style="5"] h2,
body[data-ui-style="1"] h3,
body[data-ui-style="2"] h3,
body[data-ui-style="3"] h3,
body[data-ui-style="4"] h3,
body[data-ui-style="5"] h3,
body[data-ui-style="1"] h4,
body[data-ui-style="2"] h4,
body[data-ui-style="3"] h4,
body[data-ui-style="4"] h4,
body[data-ui-style="5"] h4,
body[data-ui-style="1"] p,
body[data-ui-style="2"] p,
body[data-ui-style="3"] p,
body[data-ui-style="4"] p,
body[data-ui-style="5"] p,
body[data-ui-style="1"] li,
body[data-ui-style="2"] li,
body[data-ui-style="3"] li,
body[data-ui-style="4"] li,
body[data-ui-style="5"] li,
body[data-ui-style="1"] a,
body[data-ui-style="2"] a,
body[data-ui-style="3"] a,
body[data-ui-style="4"] a,
body[data-ui-style="5"] a,
body[data-ui-style="1"] button,
body[data-ui-style="2"] button,
body[data-ui-style="3"] button,
body[data-ui-style="4"] button,
body[data-ui-style="5"] button,
body[data-ui-style="1"] span,
body[data-ui-style="2"] span,
body[data-ui-style="3"] span,
body[data-ui-style="4"] span,
body[data-ui-style="5"] span {
    overflow-wrap: break-word;
}

body[data-ui-style="1"] .logo img,
body[data-ui-style="2"] .logo img,
body[data-ui-style="3"] .logo img,
body[data-ui-style="4"] .logo img,
body[data-ui-style="5"] .logo img {
    width: auto;
    height: auto;
    max-width: 190px;
    max-height: 56px;
    object-fit: contain;
}

body[data-ui-style="1"] .ui-style-control,
body[data-ui-style="2"] .ui-style-control,
body[data-ui-style="3"] .ui-style-control,
body[data-ui-style="4"] .ui-style-control,
body[data-ui-style="5"] .ui-style-control {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    margin-left: auto;
    margin-right: 1.5rem;
    color: var(--text-muted);
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
}

body[data-ui-style="1"] .ui-style-control select,
body[data-ui-style="2"] .ui-style-control select,
body[data-ui-style="3"] .ui-style-control select,
body[data-ui-style="4"] .ui-style-control select,
body[data-ui-style="5"] .ui-style-control select {
    padding: 0.38rem 0.8rem;
    border: 1px solid var(--accent);
    border-radius: 999px;
    background: var(--white);
    color: var(--text-main);
    font: inherit;
    font-size: 0.8rem;
    font-weight: 600;
    letter-spacing: 0;
    text-transform: none;
    cursor: pointer;
}

body[data-ui-style="1"] .expandable-content p,
body[data-ui-style="2"] .expandable-content p,
body[data-ui-style="3"] .expandable-content p,
body[data-ui-style="4"] .expandable-content p,
body[data-ui-style="5"] .expandable-content p,
body[data-ui-style="1"] .service-block p,
body[data-ui-style="2"] .service-block p,
body[data-ui-style="3"] .service-block p,
body[data-ui-style="4"] .service-block p,
body[data-ui-style="5"] .service-block p,
body[data-ui-style="1"] .service-list li,
body[data-ui-style="2"] .service-list li,
body[data-ui-style="3"] .service-list li,
body[data-ui-style="4"] .service-list li,
body[data-ui-style="5"] .service-list li,
body[data-ui-style="1"] .director-text p,
body[data-ui-style="2"] .director-text p,
body[data-ui-style="3"] .director-text p,
body[data-ui-style="4"] .director-text p,
body[data-ui-style="5"] .director-text p,
body[data-ui-style="1"] .mv-block p,
body[data-ui-style="2"] .mv-block p,
body[data-ui-style="3"] .mv-block p,
body[data-ui-style="4"] .mv-block p,
body[data-ui-style="5"] .mv-block p {
    text-align: left;
}

body[data-ui-style="1"] .expandable-container::after,
body[data-ui-style="2"] .expandable-container::after,
body[data-ui-style="3"] .expandable-container::after,
body[data-ui-style="4"] .expandable-container::after,
body[data-ui-style="5"] .expandable-container::after {
    background: linear-gradient(transparent, var(--card-bg, var(--white)));
}

body[data-ui-style="1"] .about-grid > *,
body[data-ui-style="2"] .about-grid > *,
body[data-ui-style="3"] .about-grid > *,
body[data-ui-style="4"] .about-grid > *,
body[data-ui-style="5"] .about-grid > *,
body[data-ui-style="1"] .services-container > *,
body[data-ui-style="2"] .services-container > *,
body[data-ui-style="3"] .services-container > *,
body[data-ui-style="4"] .services-container > *,
body[data-ui-style="5"] .services-container > *,
body[data-ui-style="1"] .values-grid > *,
body[data-ui-style="2"] .values-grid > *,
body[data-ui-style="3"] .values-grid > *,
body[data-ui-style="4"] .values-grid > *,
body[data-ui-style="5"] .values-grid > *,
body[data-ui-style="1"] .gallery-grid > *,
body[data-ui-style="2"] .gallery-grid > *,
body[data-ui-style="3"] .gallery-grid > *,
body[data-ui-style="4"] .gallery-grid > *,
body[data-ui-style="5"] .gallery-grid > * {
    min-width: 0;
}

body[data-ui-style="1"] .gallery-item,
body[data-ui-style="2"] .gallery-item,
body[data-ui-style="3"] .gallery-item,
body[data-ui-style="4"] .gallery-item,
body[data-ui-style="5"] .gallery-item {
    height: auto;
    aspect-ratio: 4 / 3;
}

body[data-ui-style="1"] .gallery-grid,
body[data-ui-style="2"] .gallery-grid,
body[data-ui-style="3"] .gallery-grid,
body[data-ui-style="4"] .gallery-grid,
body[data-ui-style="5"] .gallery-grid {
    grid-auto-flow: dense;
}

/* =====================================================
   1. EDITORIAL LATERAL: menú en columna lateral, lectura de revista
   ===================================================== */
body[data-ui-style="1"] {
    --primary: #713f57;
    --primary-dark: #44263a;
    --accent: #d58a91;
    --bg-light: #fbf1ef;
    --text-main: #382d35;
    --text-muted: #756572;
    --white: #fffaf8;
    --card-radius: 6px;
    --shadow: none;
    --border-card: 1px solid rgba(113, 63, 87, 0.18);
    --side: 220px;
}

body[data-ui-style="1"] section {
    padding: 6rem 8% 5rem;
}

body[data-ui-style="1"] .section-title {
    text-align: left;
    font-size: 2.6rem;
}

body[data-ui-style="1"] .section-title::after {
    margin-left: 0;
}

body[data-ui-style="1"] .hero {
    min-height: 640px;
    align-items: flex-end;
    justify-content: flex-start;
    padding: 0 8% 9%;
    text-align: left;
    background: linear-gradient(120deg, rgba(68, 38, 58, 0.9), rgba(68, 38, 58, 0.2)), var(--hero-image) center/cover no-repeat;
}

body[data-ui-style="1"] .hero-content h1 {
    font-size: clamp(2.8rem, 6vw, 5rem);
    letter-spacing: 0;
    line-height: 1.05;
}

body[data-ui-style="1"] .hero-content p {
    margin-left: 0;
}

body[data-ui-style="1"] .btn {
    background: var(--accent);
    color: var(--primary-dark);
}

body[data-ui-style="1"] .about-grid {
    grid-template-columns: 1.1fr 0.9fr;
}

body[data-ui-style="1"] .card-director,
body[data-ui-style="1"] .combined-mv {
    background: var(--white);
    box-shadow: none;
}

body[data-ui-style="1"] .values-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
}

body[data-ui-style="1"] .value-item {
    background: transparent;
    border: none;
    border-top: 2px solid var(--accent);
    border-radius: 0;
    padding: 1.5rem 0.25rem 0;
    box-shadow: none;
}

body[data-ui-style="1"] .services-container {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1.75rem;
    padding: 0;
}

body[data-ui-style="1"] .service-block {
    padding: 2.25rem 2rem;
    border-left: 4px solid var(--accent);
    box-shadow: none;
}

body[data-ui-style="1"] .gallery-filters {
    justify-content: flex-start;
}

body[data-ui-style="1"] .gallery-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 1rem;
}

body[data-ui-style="1"] .gallery-item {
    border-radius: var(--card-radius);
    box-shadow: none;
}

body[data-ui-style="1"] .contact-content {
    margin: 0;
    text-align: left;
}

body[data-ui-style="1"] .social-icons {
    justify-content: flex-start;
}

@media (min-width: 981px) {
    body[data-ui-style="1"] {
        padding-left: var(--side);
    }

    body[data-ui-style="1"] header {
        inset: 0 auto 0 0;
        width: var(--side);
        height: 100vh;
        flex-direction: column;
        align-items: stretch;
        justify-content: flex-start;
        gap: 2rem;
        padding: 2rem 1.4rem;
        background: #f2e3e5;
        border-right: 1px solid rgba(113, 63, 87, 0.18);
        box-shadow: none;
        backdrop-filter: none;
    }

    body[data-ui-style="1"] .menu-toggle {
        display: none;
    }

    body[data-ui-style="1"] nav {
        order: 2;
    }

    body[data-ui-style="1"] nav ul {
        flex-direction: column;
        gap: 0;
    }

    body[data-ui-style="1"] nav a {
        display: block;
        padding: 0.6rem 0;
        border-bottom: 1px solid rgba(113, 63, 87, 0.12);
        color: var(--primary-dark);
        font-family: var(--font-heading);
        font-size: 1.05rem;
    }

    body[data-ui-style="1"] .ui-style-control {
        order: 3;
        flex-direction: column;
        align-items: flex-start;
        margin: auto 0 0;
    }

    body[data-ui-style="1"] .hero {
        height: 100vh;
    }

    body[data-ui-style="1"] section {
        scroll-margin-top: 0;
    }
}

/* =====================================================
   2. FICHA MINIMAL: líneas finas, sin sombras, lectura rápida
   ===================================================== */
body[data-ui-style="2"] {
    --primary: #287c82;
    --primary-dark: #18555b;
    --accent: #e78b55;
    --bg-light: #f5f8f6;
    --text-main: #263b3e;
    --text-muted: #647578;
    --white: #ffffff;
    --card-radius: 4px;
    --shadow: none;
    --border-card: 1px solid rgba(24, 85, 91, 0.16);
}

body[data-ui-style="2"] header {
    box-shadow: none;
    border-bottom: 1px solid rgba(24, 85, 91, 0.14);
}

body[data-ui-style="2"] section {
    padding: 5rem 8% 4rem;
}

body[data-ui-style="2"] .section-title {
    margin-bottom: 2.5rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid rgba(24, 85, 91, 0.2);
    text-align: left;
    font-family: var(--font-body);
    font-size: 0.8rem;
    font-weight: 700;
    letter-spacing: 0.18em;
    text-transform: uppercase;
}

body[data-ui-style="2"] .section-title::after {
    display: none;
}

body[data-ui-style="2"] .hero {
    height: auto;
    min-height: min(88vh, 720px);
    justify-content: flex-start;
    padding: 8rem 8% 5rem;
    text-align: left;
    color: var(--text-main);
    background: linear-gradient(90deg, var(--bg-light) 0 52%, rgba(245, 248, 246, 0) 52%), var(--hero-image) center/cover no-repeat;
}

body[data-ui-style="2"] .hero-content {
    max-width: min(540px, 46%);
}

body[data-ui-style="2"] .hero-content h1 {
    color: var(--primary-dark);
    font-size: clamp(2.6rem, 5vw, 4.2rem);
    letter-spacing: 0;
    line-height: 1.1;
}

body[data-ui-style="2"] .hero-content h2 {
    color: var(--text-muted);
}

body[data-ui-style="2"] .hero-content p {
    margin-left: 0;
}

body[data-ui-style="2"] .about-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 2rem;
}

body[data-ui-style="2"] .card-director,
body[data-ui-style="2"] .combined-mv {
    box-shadow: none;
    padding: 2rem;
}

body[data-ui-style="2"] .values-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 1rem;
}

body[data-ui-style="2"] .value-item {
    background: var(--white);
    box-shadow: none;
    padding: 1.5rem;
}

body[data-ui-style="2"] .services-container {
    grid-template-columns: 1fr;
    gap: 0;
    padding: 0;
}

body[data-ui-style="2"] .service-block {
    display: grid;
    grid-template-columns: minmax(200px, 0.3fr) minmax(0, 1fr);
    column-gap: 3rem;
    align-content: start;
    padding: 2rem 0;
    background: transparent;
    border: none;
    border-top: 1px solid rgba(24, 85, 91, 0.2);
    border-radius: 0;
    box-shadow: none;
}

body[data-ui-style="2"] .service-block {
    --card-bg: var(--bg-light);
}

body[data-ui-style="2"] .service-block:hover {
    transform: none;
}

body[data-ui-style="2"] .service-block > div:first-child {
    display: contents;
}

body[data-ui-style="2"] .service-block h3 {
    grid-column: 1;
    grid-row: 1 / span 3;
    align-self: start;
    margin: 0;
    padding: 0;
    border: none;
    font-size: 1.3rem;
}

body[data-ui-style="2"] .service-block h3 i {
    width: 42px;
    height: 42px;
    font-size: 1.1rem;
}

body[data-ui-style="2"] .service-block p,
body[data-ui-style="2"] .service-block .expandable-container,
body[data-ui-style="2"] .service-block .expandable-btn-container {
    grid-column: 2;
}

body[data-ui-style="2"] .service-block .expandable-btn-container {
    margin-top: 1rem;
    padding-top: 0;
    border: none;
    text-align: left;
}

body[data-ui-style="2"] #instalaciones,
body[data-ui-style="2"] #galeria {
    background: #e8f2ef;
}

body[data-ui-style="2"] .gallery-filters {
    justify-content: flex-start;
}

body[data-ui-style="2"] .gallery-grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 0.75rem;
}

body[data-ui-style="2"] .gallery-item {
    aspect-ratio: 1 / 1;
    box-shadow: none;
}

/* =====================================================
   3. CINEMÁTICO: oscuro, portada dominante, acentos dorados
   ===================================================== */
body[data-ui-style="3"] {
    --primary: #9bd9cf;
    --primary-dark: #d2f1e9;
    --accent: #e5b96d;
    --bg-light: #101522;
    --text-main: #f4f0e8;
    --text-muted: #abb6c2;
    --white: #192235;
    --card-radius: 12px;
    --shadow: 0 20px 50px rgba(0, 0, 0, 0.35);
    --border-card: 1px solid rgba(229, 185, 109, 0.2);
}

body[data-ui-style="3"] header {
    background: linear-gradient(rgba(8, 11, 20, 0.92), rgba(8, 11, 20, 0.55));
    box-shadow: none;
    backdrop-filter: blur(8px);
}

body[data-ui-style="3"] nav a {
    color: var(--text-main);
}

body[data-ui-style="3"] nav a:hover {
    color: var(--accent);
}

body[data-ui-style="3"] .hero {
    min-height: 640px;
    align-items: flex-end;
    justify-content: flex-start;
    padding: 0 8% 9%;
    text-align: left;
    background:
        linear-gradient(180deg, rgba(8, 11, 20, 0.3), rgba(8, 11, 20, 0.88)),
        linear-gradient(90deg, rgba(8, 11, 20, 0.8), rgba(8, 11, 20, 0) 70%),
        var(--hero-image) center/cover no-repeat;
}

body[data-ui-style="3"] .hero-content h1 {
    font-size: clamp(2.8rem, 7vw, 5.5rem);
    letter-spacing: 0;
    line-height: 1.05;
}

body[data-ui-style="3"] .hero-content h2 {
    color: var(--accent);
}

body[data-ui-style="3"] .hero-content p {
    margin-left: 0;
}

body[data-ui-style="3"] .btn {
    background: var(--accent);
    color: #101522;
}

body[data-ui-style="3"] section {
    padding: 6rem 8% 5rem;
}

body[data-ui-style="3"] .section-title {
    text-align: left;
}

body[data-ui-style="3"] .section-title::after {
    margin-left: 0;
}

body[data-ui-style="3"] #mision-vision,
body[data-ui-style="3"] #servicios {
    background: #151c2d !important;
}

body[data-ui-style="3"] #valores,
body[data-ui-style="3"] #instalaciones {
    background: #0b101c;
}

body[data-ui-style="3"] #galeria {
    background: #101522;
}

body[data-ui-style="3"] .about-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
}

body[data-ui-style="3"] .values-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
}

body[data-ui-style="3"] .services-container {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1.75rem;
    padding: 0;
}

body[data-ui-style="3"] .service-block {
    padding: 2.25rem 2rem;
}

body[data-ui-style="3"] .gallery-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 1rem;
}

body[data-ui-style="3"] .gallery-item {
    aspect-ratio: 16 / 10;
}

body[data-ui-style="3"] .gallery-filters {
    justify-content: flex-start;
}

body[data-ui-style="3"] .social-icons a {
    background: var(--accent);
    color: #101522;
}

body[data-ui-style="3"] footer {
    background: #0b101c;
    border-top: 1px solid rgba(229, 185, 109, 0.2);
    color: var(--text-muted);
}

/* =====================================================
   4. REVISTA BENTO: piezas de distinto tamaño, portada dividida
   ===================================================== */
body[data-ui-style="4"] {
    --primary: #31506b;
    --primary-dark: #1d3043;
    --accent: #ef755f;
    --bg-light: #fff4e8;
    --text-main: #293746;
    --text-muted: #687582;
    --white: #fffdf9;
    --card-radius: 20px;
    --shadow: 0 12px 30px rgba(41, 55, 70, 0.08);
    --border-card: 1px solid rgba(49, 80, 107, 0.12);
}

body[data-ui-style="4"] header {
    background: var(--white);
}

body[data-ui-style="4"] section {
    padding: 5.5rem 8% 4.5rem;
}

body[data-ui-style="4"] .hero {
    height: auto;
    min-height: min(90vh, 740px);
    justify-content: flex-start;
    padding: 8rem 8% 5rem;
    text-align: left;
    color: var(--text-main);
    background: linear-gradient(90deg, var(--bg-light) 0 50%, rgba(255, 244, 232, 0) 50%), var(--hero-image) center/cover no-repeat;
}

body[data-ui-style="4"] .hero-content {
    max-width: min(520px, 45%);
}

body[data-ui-style="4"] .hero-content h1 {
    color: var(--primary-dark);
    font-size: clamp(2.6rem, 5vw, 4.4rem);
    letter-spacing: 0;
    line-height: 1.08;
}

body[data-ui-style="4"] .hero-content h2 {
    color: var(--primary);
}

body[data-ui-style="4"] .hero-content p {
    margin-left: 0;
}

body[data-ui-style="4"] .btn {
    background: var(--accent);
    color: var(--primary-dark);
}

body[data-ui-style="4"] .about-grid {
    grid-template-columns: 1.15fr 0.85fr;
}

body[data-ui-style="4"] .values-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
}

body[data-ui-style="4"] .services-container {
    grid-template-columns: repeat(6, minmax(0, 1fr));
    gap: 1.25rem;
    padding: 0;
}

body[data-ui-style="4"] .service-block {
    grid-column: span 3;
    padding: 2.25rem 2rem;
    border-top: 5px solid var(--accent);
}

body[data-ui-style="4"] .service-block:nth-child(4n + 1),
body[data-ui-style="4"] .service-block:nth-child(4n + 4) {
    grid-column: span 4;
}

body[data-ui-style="4"] .service-block:nth-child(4n + 2),
body[data-ui-style="4"] .service-block:nth-child(4n + 3) {
    grid-column: span 2;
}

body[data-ui-style="4"] .service-block:nth-child(4n + 2) {
    --card-bg: #ffe3d6;
    background: var(--card-bg);
}

body[data-ui-style="4"] .service-block:nth-child(4n + 3) {
    --card-bg: #e3ecf4;
    background: var(--card-bg);
}

body[data-ui-style="4"] .gallery-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    grid-auto-rows: 220px;
    gap: 1rem;
}

body[data-ui-style="4"] .gallery-item {
    aspect-ratio: auto;
}

body[data-ui-style="4"] .gallery-item[data-pos="1"] {
    grid-column: span 2;
    grid-row: span 2;
}

/* =====================================================
   5. JARDÍN VIVO: formas suaves, pasteles, mosaico orgánico
   ===================================================== */
body[data-ui-style="5"] {
    --primary: #765985;
    --primary-dark: #4c385b;
    --accent: #ed927d;
    --bg-light: #fff3eb;
    --text-main: #3e3445;
    --text-muted: #766b79;
    --white: #fffdfb;
    --card-radius: 24px;
    --shadow: 0 14px 34px rgba(76, 56, 91, 0.1);
    --border-card: 1px solid rgba(118, 89, 133, 0.12);
}

body[data-ui-style="5"] header {
    top: 14px;
    left: 5%;
    width: 90%;
    padding: 0.7rem 1.6rem;
    border-radius: 999px;
    box-shadow: 0 10px 30px rgba(76, 56, 91, 0.14);
}

body[data-ui-style="5"] section {
    padding: 6rem 8% 4.5rem;
}

body[data-ui-style="5"] .hero {
    min-height: 640px;
    padding-top: 7rem;
    border-radius: 0 0 56px 56px;
    color: var(--primary-dark);
    background: linear-gradient(180deg, rgba(255, 243, 235, 0.6), rgba(255, 243, 235, 0.88)), var(--hero-image) center/cover no-repeat;
}

body[data-ui-style="5"] .hero-content h1 {
    color: var(--primary-dark);
    letter-spacing: 0;
}

body[data-ui-style="5"] .hero-content h2 {
    color: var(--primary);
}

body[data-ui-style="5"] .btn {
    background: var(--accent);
    color: var(--primary-dark);
}

body[data-ui-style="5"] #mision-vision {
    background: var(--white);
}

body[data-ui-style="5"] #valores {
    background: #f1e8f6;
}

body[data-ui-style="5"] .value-item:nth-child(odd) {
    background: var(--white);
}

body[data-ui-style="5"] .value-item:nth-child(even) {
    background: #fbd9cf;
}

body[data-ui-style="5"] .services-container {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1.5rem;
    padding: 0;
}

body[data-ui-style="5"] .service-block {
    --card-bg: #eadcf3;
    padding: 2.25rem 2rem;
    background: var(--card-bg);
    border: none;
}

body[data-ui-style="5"] .service-block:nth-child(4n + 2),
body[data-ui-style="5"] .service-block:nth-child(4n + 3) {
    --card-bg: #fbd9cf;
}

body[data-ui-style="5"] .gallery-grid {
    display: block;
    column-count: 3;
    column-gap: 1rem;
}

body[data-ui-style="5"] .gallery-item {
    display: block;
    margin-bottom: 1rem;
    break-inside: avoid;
    aspect-ratio: auto;
    height: 260px;
    border: none;
    border-radius: 28px;
}

body[data-ui-style="5"] .gallery-item[data-pos="1"],
body[data-ui-style="5"] .gallery-item[data-pos="5"] {
    height: 320px;
}

body[data-ui-style="5"] .gallery-item[data-pos="2"],
body[data-ui-style="5"] .gallery-item[data-pos="6"] {
    height: 220px;
}

body[data-ui-style="5"] .social-icons a {
    background: var(--primary);
}

/* =====================================================
   Adaptación a pantallas medianas y móviles (formatos 1 al 5)
   ===================================================== */
@media (max-width: 980px) {
    body[data-ui-style="1"] .about-grid,
body[data-ui-style="2"] .about-grid,
body[data-ui-style="3"] .about-grid,
body[data-ui-style="4"] .about-grid {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="1"] .services-container,
body[data-ui-style="3"] .services-container,
body[data-ui-style="5"] .services-container {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="4"] .services-container {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    body[data-ui-style="4"] .service-block,
body[data-ui-style="4"] .service-block:nth-child(n) {
        grid-column: auto;
    }

    body[data-ui-style="1"] .values-grid,
body[data-ui-style="2"] .values-grid,
body[data-ui-style="3"] .values-grid,
body[data-ui-style="4"] .values-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    body[data-ui-style="1"] .gallery-grid,
body[data-ui-style="2"] .gallery-grid,
body[data-ui-style="3"] .gallery-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    body[data-ui-style="5"] .gallery-grid {
        column-count: 2;
    }

    body[data-ui-style="2"] .hero,
body[data-ui-style="4"] .hero {
        background: linear-gradient(rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.9)), var(--hero-image) center/cover no-repeat;
    }

    body[data-ui-style="2"] .hero-content,
body[data-ui-style="4"] .hero-content {
        max-width: 100%;
    }
}

@media (max-width: 768px) {
    body[data-ui-style="1"] .ui-style-control,
body[data-ui-style="2"] .ui-style-control,
body[data-ui-style="3"] .ui-style-control,
body[data-ui-style="4"] .ui-style-control,
body[data-ui-style="5"] .ui-style-control {
        margin-right: 1rem;
    }

    body[data-ui-style="1"] .ui-style-control span,
body[data-ui-style="2"] .ui-style-control span,
body[data-ui-style="3"] .ui-style-control span,
body[data-ui-style="4"] .ui-style-control span,
body[data-ui-style="5"] .ui-style-control span {
        display: none;
    }

    body[data-ui-style="1"] header,
body[data-ui-style="2"] header,
body[data-ui-style="3"] header,
body[data-ui-style="4"] header,
body[data-ui-style="5"] header {
        padding: 0.85rem 5%;
    }

    body[data-ui-style="1"] section,
body[data-ui-style="2"] section,
body[data-ui-style="3"] section,
body[data-ui-style="4"] section,
body[data-ui-style="5"] section {
        padding: 4.5rem 6% 3rem;
    }

    body[data-ui-style="5"] header {
        top: 8px;
        left: 3%;
        width: 94%;
        padding: 0.6rem 1.1rem;
        border-radius: 28px;
    }

    body[data-ui-style="1"] .hero-content h1,
body[data-ui-style="2"] .hero-content h1,
body[data-ui-style="3"] .hero-content h1,
body[data-ui-style="4"] .hero-content h1,
body[data-ui-style="5"] .hero-content h1 {
        font-size: clamp(2.4rem, 12vw, 3.6rem);
    }

    body[data-ui-style="1"] .values-grid,
body[data-ui-style="2"] .values-grid,
body[data-ui-style="3"] .values-grid,
body[data-ui-style="4"] .values-grid,
body[data-ui-style="4"] .services-container {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="2"] .service-block {
        display: flex;
        padding: 1.5rem 0;
    }

    body[data-ui-style="2"] .service-block > div:first-child {
        display: block;
    }

    body[data-ui-style="2"] .service-block h3 {
        margin-bottom: 1rem;
    }

    body[data-ui-style="2"] .gallery-grid,
body[data-ui-style="4"] .gallery-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    body[data-ui-style="4"] .gallery-grid {
        grid-auto-rows: 160px;
    }

    body[data-ui-style="4"] .gallery-item[data-pos="1"] {
        grid-column: span 2;
        grid-row: span 1;
    }

    body[data-ui-style="3"] .gallery-grid {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="5"] .gallery-grid {
        column-count: 1;
    }

    body[data-ui-style="5"] .gallery-item,
body[data-ui-style="5"] .gallery-item[data-pos] {
        height: 230px;
    }

    body[data-ui-style="5"] .hero {
        border-radius: 0 0 32px 32px;
    }

    body[data-ui-style="3"] nav ul {
        background: var(--white);
    }
}

```

### Archivo: `css/formatos/_comun.css`
```css
/* Base común de los formatos 1 al 5. Se carga solo si el formato activo no es el 0. */

body[data-ui-style] {
    overflow-x: hidden;
}

body[data-ui-style] section {
    scroll-margin-top: 84px;
}

body[data-ui-style] h1,
body[data-ui-style] h2,
body[data-ui-style] h3,
body[data-ui-style] h4,
body[data-ui-style] p,
body[data-ui-style] li,
body[data-ui-style] a,
body[data-ui-style] button,
body[data-ui-style] span {
    overflow-wrap: break-word;
}

body[data-ui-style] .logo img {
    width: auto;
    height: auto;
    max-width: 190px;
    max-height: 56px;
    object-fit: contain;
}

body[data-ui-style] .expandable-content p,
body[data-ui-style] .service-block p,
body[data-ui-style] .service-list li,
body[data-ui-style] .director-text p,
body[data-ui-style] .mv-block p {
    text-align: left;
}

body[data-ui-style] .expandable-container::after {
    background: linear-gradient(transparent, var(--card-bg, var(--white)));
}

body[data-ui-style] .about-grid > *,
body[data-ui-style] .services-container > *,
body[data-ui-style] .values-grid > *,
body[data-ui-style] .gallery-grid > * {
    min-width: 0;
}

body[data-ui-style] .gallery-item {
    height: auto;
    aspect-ratio: 4 / 3;
}

body[data-ui-style] .gallery-grid {
    grid-auto-flow: dense;
}

@media (max-width: 768px) {
    body[data-ui-style] header {
        padding: 0.85rem 5%;
    }

    body[data-ui-style] section {
        padding: 4.5rem 6% 3rem;
    }

    body[data-ui-style] .hero-content h1 {
        font-size: clamp(2.4rem, 12vw, 3.6rem);
    }
}

```

### Archivo: `css/formatos/f1-editorial.css`
```css
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,600;0,700;1,600&display=swap');

/* 1. EDITORIAL LATERAL: menú en columna lateral, marfil, tinta y terracota */
body[data-ui-style="1"] {
    --primary: #2f3a4a;
    --primary-dark: #1c2430;
    --accent: #b85a36;
    --bg-light: #f6f0e6;
    --text-main: #2b2622;
    --text-muted: #6f665d;
    --white: #fffdf8;
    --card-radius: 6px;
    --shadow: none;
    --border-card: 1px solid rgba(47, 58, 74, 0.18);
    --font-heading: 'Cormorant Garamond', Georgia, serif;
    --side: 220px;
}

body[data-ui-style="1"] h2,
body[data-ui-style="1"] h3 {
    font-weight: 700;
}

body[data-ui-style="1"] section {
    padding: 6rem 8% 5rem;
}

body[data-ui-style="1"] .section-title {
    text-align: left;
    font-size: 3.2rem;
}

body[data-ui-style="1"] .section-title::after {
    margin-left: 0;
}

/* Portada centrada y enmarcada, con margen alrededor de la foto */
body[data-ui-style="1"] .hero {
    min-height: 70vh;
    height: auto;
    margin: 1.5rem;
    padding: 0 6%;
    border-radius: var(--card-radius);
    text-align: center;
    background: linear-gradient(rgba(28, 36, 48, 0.35), rgba(28, 36, 48, 0.72)), var(--hero-image) center/cover no-repeat;
}

body[data-ui-style="1"] .hero-content h1 {
    font-size: clamp(3rem, 7vw, 5.8rem);
    font-style: italic;
    font-weight: 600;
    letter-spacing: 0;
    line-height: 1.02;
}

body[data-ui-style="1"] .hero-content h2 {
    color: #f3e3d2;
}

body[data-ui-style="1"] .btn {
    background: var(--accent);
    color: #ffffff;
    border-radius: 2px;
}

body[data-ui-style="1"] .about-grid {
    grid-template-columns: 1.1fr 0.9fr;
}

body[data-ui-style="1"] .card-director,
body[data-ui-style="1"] .combined-mv {
    background: var(--white);
    box-shadow: none;
}

body[data-ui-style="1"] .values-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
}

body[data-ui-style="1"] .value-item {
    background: transparent;
    border: none;
    border-top: 2px solid var(--accent);
    border-radius: 0;
    padding: 1.5rem 0.25rem 0;
    box-shadow: none;
}

body[data-ui-style="1"] .services-container {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1.75rem;
    padding: 0;
}

body[data-ui-style="1"] .service-block {
    padding: 2.25rem 2rem;
    border-left: 4px solid var(--accent);
    box-shadow: none;
}

body[data-ui-style="1"] .gallery-filters {
    justify-content: flex-start;
}

body[data-ui-style="1"] .gallery-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 1rem;
}

body[data-ui-style="1"] .gallery-item {
    border-radius: var(--card-radius);
    box-shadow: none;
}

body[data-ui-style="1"] .contact-content {
    margin: 0;
    text-align: left;
}

body[data-ui-style="1"] .social-icons {
    justify-content: flex-start;
}

@media (min-width: 981px) {
    body[data-ui-style="1"] {
        padding-left: var(--side);
    }

    body[data-ui-style="1"] header {
        inset: 0 auto 0 0;
        width: var(--side);
        height: 100vh;
        flex-direction: column;
        align-items: stretch;
        justify-content: flex-start;
        gap: 2rem;
        padding: 2rem 1.4rem;
        background: #efe6d6;
        border-right: 1px solid rgba(47, 58, 74, 0.18);
        box-shadow: none;
        backdrop-filter: none;
    }

    body[data-ui-style="1"] .menu-toggle {
        display: none;
    }

    body[data-ui-style="1"] nav {
        order: 2;
    }

    body[data-ui-style="1"] nav ul {
        flex-direction: column;
        gap: 0;
    }

    body[data-ui-style="1"] nav a {
        display: block;
        padding: 0.6rem 0;
        border-bottom: 1px solid rgba(47, 58, 74, 0.14);
        color: var(--primary-dark);
        font-family: var(--font-heading);
        font-size: 1.25rem;
        font-weight: 700;
    }

    body[data-ui-style="1"] .hero {
        height: calc(100vh - 3rem);
    }

    body[data-ui-style="1"] section {
        scroll-margin-top: 0;
    }
}

@media (max-width: 980px) {
    body[data-ui-style="1"] .hero {
        margin: 5.5rem 1rem 1rem;
    }

    body[data-ui-style="1"] .about-grid,
    body[data-ui-style="1"] .services-container {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="1"] .values-grid,
    body[data-ui-style="1"] .gallery-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
}

@media (max-width: 768px) {
    body[data-ui-style="1"] .values-grid {
        grid-template-columns: 1fr;
    }
}

```

### Archivo: `css/formatos/f2-minimal.css`
```css
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap');

/* 2. FICHA MINIMAL: líneas finas, sin sombras, lectura rápida */
body[data-ui-style="2"] {
    --font-heading: 'DM Sans', sans-serif;
    --font-body: 'DM Sans', sans-serif;
    --primary: #287c82;
    --primary-dark: #18555b;
    --accent: #e78b55;
    --bg-light: #f5f8f6;
    --text-main: #263b3e;
    --text-muted: #647578;
    --white: #ffffff;
    --card-radius: 4px;
    --shadow: none;
    --border-card: 1px solid rgba(24, 85, 91, 0.16);
}

body[data-ui-style="2"] header {
    box-shadow: none;
    border-bottom: 1px solid rgba(24, 85, 91, 0.14);
}

body[data-ui-style="2"] section {
    padding: 5rem 8% 4rem;
}

body[data-ui-style="2"] .section-title {
    margin-bottom: 2.5rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid rgba(24, 85, 91, 0.2);
    text-align: left;
    font-family: var(--font-body);
    font-size: 0.8rem;
    font-weight: 700;
    letter-spacing: 0.18em;
    text-transform: uppercase;
}

body[data-ui-style="2"] .section-title::after {
    display: none;
}

body[data-ui-style="2"] .hero {
    height: auto;
    min-height: min(88vh, 720px);
    justify-content: flex-start;
    padding: 8rem 8% 5rem;
    text-align: left;
    color: var(--text-main);
    background: linear-gradient(90deg, var(--bg-light) 0 52%, rgba(245, 248, 246, 0) 52%), var(--hero-image) center/cover no-repeat;
}

body[data-ui-style="2"] .hero-content {
    max-width: min(540px, 46%);
}

body[data-ui-style="2"] .hero-content h1 {
    color: var(--primary-dark);
    font-size: clamp(2.6rem, 5vw, 4.2rem);
    font-weight: 700;
    letter-spacing: -0.03em;
    line-height: 1.1;
}

body[data-ui-style="2"] .hero-content h2 {
    color: var(--text-muted);
}

body[data-ui-style="2"] .hero-content p {
    margin-left: 0;
}

body[data-ui-style="2"] .about-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 2rem;
}

body[data-ui-style="2"] .card-director,
body[data-ui-style="2"] .combined-mv {
    box-shadow: none;
    padding: 2rem;
}

body[data-ui-style="2"] .values-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 1rem;
}

body[data-ui-style="2"] .value-item {
    background: var(--white);
    box-shadow: none;
    padding: 1.5rem;
}

body[data-ui-style="2"] .services-container {
    grid-template-columns: 1fr;
    gap: 0;
    padding: 0;
}

body[data-ui-style="2"] .service-block {
    --card-bg: var(--bg-light);
    display: grid;
    grid-template-columns: minmax(200px, 0.3fr) minmax(0, 1fr);
    column-gap: 3rem;
    align-content: start;
    padding: 2rem 0;
    background: transparent;
    border: none;
    border-top: 1px solid rgba(24, 85, 91, 0.2);
    border-radius: 0;
    box-shadow: none;
}

body[data-ui-style="2"] .service-block:hover {
    transform: none;
}

body[data-ui-style="2"] .service-block > div:first-child {
    display: contents;
}

body[data-ui-style="2"] .service-block h3 {
    grid-column: 1;
    grid-row: 1 / span 3;
    align-self: start;
    margin: 0;
    padding: 0;
    border: none;
    font-size: 1.3rem;
}

body[data-ui-style="2"] .service-block h3 i {
    width: 42px;
    height: 42px;
    font-size: 1.1rem;
}

body[data-ui-style="2"] .service-block p,
body[data-ui-style="2"] .service-block .expandable-container,
body[data-ui-style="2"] .service-block .expandable-btn-container {
    grid-column: 2;
}

body[data-ui-style="2"] .service-block .expandable-btn-container {
    margin-top: 1rem;
    padding-top: 0;
    border: none;
    text-align: left;
}

body[data-ui-style="2"] #instalaciones,
body[data-ui-style="2"] #galeria {
    background: #e8f2ef;
}

body[data-ui-style="2"] .gallery-filters {
    justify-content: flex-start;
}

body[data-ui-style="2"] .gallery-grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 0.75rem;
}

body[data-ui-style="2"] .gallery-item {
    aspect-ratio: 1 / 1;
    box-shadow: none;
}

@media (max-width: 980px) {
    body[data-ui-style="2"] .about-grid {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="2"] .values-grid,
    body[data-ui-style="2"] .gallery-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    body[data-ui-style="2"] .hero {
        background: linear-gradient(rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.9)), var(--hero-image) center/cover no-repeat;
    }

    body[data-ui-style="2"] .hero-content {
        max-width: 100%;
    }
}

@media (max-width: 768px) {
    body[data-ui-style="2"] .values-grid {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="2"] .service-block {
        display: flex;
        padding: 1.5rem 0;
    }

    body[data-ui-style="2"] .service-block > div:first-child {
        display: block;
    }

    body[data-ui-style="2"] .service-block h3 {
        margin-bottom: 1rem;
    }
}

```

### Archivo: `css/formatos/f3-cinematico.css`
```css
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;700&display=swap');

/* 3. CINEMÁTICO: oscuro, portada dominante, acentos dorados */
body[data-ui-style="3"] {
    --font-heading: 'Cinzel', serif;
    --primary: #9bd9cf;
    --primary-dark: #d2f1e9;
    --accent: #e5b96d;
    --bg-light: #101522;
    --text-main: #f4f0e8;
    --text-muted: #abb6c2;
    --white: #192235;
    --card-radius: 12px;
    --shadow: 0 20px 50px rgba(0, 0, 0, 0.35);
    --border-card: 1px solid rgba(229, 185, 109, 0.2);
}

body[data-ui-style="3"] header {
    background: linear-gradient(rgba(8, 11, 20, 0.92), rgba(8, 11, 20, 0.55));
    box-shadow: none;
    backdrop-filter: blur(8px);
}

body[data-ui-style="3"] nav a {
    color: var(--text-main);
    font-size: 0.8rem;
    letter-spacing: 0.14em;
    text-transform: uppercase;
}

body[data-ui-style="3"] nav a:hover {
    color: var(--accent);
}

body[data-ui-style="3"] .hero {
    min-height: 640px;
    align-items: flex-end;
    justify-content: flex-start;
    padding: 0 8% 9%;
    text-align: left;
    background:
        linear-gradient(180deg, rgba(8, 11, 20, 0.3), rgba(8, 11, 20, 0.88)),
        linear-gradient(90deg, rgba(8, 11, 20, 0.8), rgba(8, 11, 20, 0) 70%),
        var(--hero-image) center/cover no-repeat;
}

body[data-ui-style="3"] .hero-content h1 {
    font-size: clamp(2.4rem, 6vw, 4.6rem);
    letter-spacing: 0.04em;
    line-height: 1.1;
}

body[data-ui-style="3"] .hero-content h2 {
    color: var(--accent);
}

body[data-ui-style="3"] .hero-content p {
    margin-left: 0;
}

body[data-ui-style="3"] .btn {
    background: var(--accent);
    color: #101522;
}

body[data-ui-style="3"] section {
    padding: 6rem 8% 5rem;
}

body[data-ui-style="3"] .section-title {
    text-align: left;
}

body[data-ui-style="3"] .section-title::after {
    margin-left: 0;
}

body[data-ui-style="3"] #mision-vision,
body[data-ui-style="3"] #servicios {
    background: #151c2d !important;
}

body[data-ui-style="3"] #valores,
body[data-ui-style="3"] #instalaciones {
    background: #0b101c;
}

body[data-ui-style="3"] #galeria {
    background: #101522;
}

body[data-ui-style="3"] .about-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
}

body[data-ui-style="3"] .values-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
}

body[data-ui-style="3"] .services-container {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1.75rem;
    padding: 0;
}

body[data-ui-style="3"] .service-block {
    padding: 2.25rem 2rem;
}

body[data-ui-style="3"] .gallery-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 1rem;
}

body[data-ui-style="3"] .gallery-item {
    aspect-ratio: 16 / 10;
}

body[data-ui-style="3"] .gallery-filters {
    justify-content: flex-start;
}

body[data-ui-style="3"] .social-icons a {
    background: var(--accent);
    color: #101522;
}

body[data-ui-style="3"] footer {
    background: #0b101c;
    border-top: 1px solid rgba(229, 185, 109, 0.2);
    color: var(--text-muted);
}

@media (max-width: 980px) {
    body[data-ui-style="3"] .about-grid,
    body[data-ui-style="3"] .services-container {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="3"] .values-grid,
    body[data-ui-style="3"] .gallery-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
}

@media (max-width: 768px) {
    body[data-ui-style="3"] .values-grid,
    body[data-ui-style="3"] .gallery-grid {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="3"] nav ul {
        background: var(--white);
    }
}

```

### Archivo: `css/formatos/f4-revista.css`
```css
@import url('https://fonts.googleapis.com/css2?family=Archivo:wght@400;500;600;800&display=swap');

/* 4. REVISTA: tipográfica, blanco y negro con un solo acento, portada a ancho completo */
body[data-ui-style="4"] {
    --primary: #111111;
    --primary-dark: #000000;
    --accent: #e4572e;
    --bg-light: #f3f3f0;
    --text-main: #1a1a1a;
    --text-muted: #5c5c5c;
    --white: #ffffff;
    --card-radius: 0;
    --shadow: none;
    --border-card: 1px solid #111111;
    --font-heading: 'Archivo', 'Arial Narrow', sans-serif;
    --font-body: 'Archivo', sans-serif;
    --hero-h: 62vh;
}

body[data-ui-style="4"] header {
    background: var(--white);
    border-bottom: 2px solid #111111;
    box-shadow: none;
}

body[data-ui-style="4"] nav a {
    font-size: 0.78rem;
    font-weight: 600;
    letter-spacing: 0.12em;
    text-transform: uppercase;
}

body[data-ui-style="4"] nav a:hover {
    color: var(--accent);
}

body[data-ui-style="4"] h1,
body[data-ui-style="4"] h2,
body[data-ui-style="4"] h3 {
    font-weight: 800;
}

body[data-ui-style="4"] section {
    padding: 5.5rem 8% 4.5rem;
}

body[data-ui-style="4"] .section-title {
    margin-bottom: 3rem;
    padding-top: 1rem;
    border-top: 4px solid #111111;
    text-align: left;
    font-size: clamp(2.2rem, 5vw, 3.6rem);
    letter-spacing: -0.02em;
    line-height: 1;
    text-transform: uppercase;
}

body[data-ui-style="4"] .section-title::after {
    display: none;
}

/* Foto a ancho completo arriba y el título debajo de la imagen */
body[data-ui-style="4"] .hero {
    display: block;
    height: auto;
    min-height: 0;
    padding: calc(76px + var(--hero-h) + 2.5rem) 8% 4rem;
    text-align: left;
    color: var(--text-main);
    background: var(--hero-image) center 76px / 100% var(--hero-h) no-repeat, var(--white);
}

body[data-ui-style="4"] .hero-content h1 {
    color: #111111;
    font-size: clamp(3rem, 9vw, 7rem);
    letter-spacing: -0.03em;
    line-height: 0.95;
    text-transform: uppercase;
}

body[data-ui-style="4"] .hero-content h2 {
    margin-top: 1rem;
    color: var(--accent);
    font-style: normal;
    font-weight: 600;
}

body[data-ui-style="4"] .hero-content p {
    margin-left: 0;
    color: var(--text-muted);
}

body[data-ui-style="4"] .btn {
    background: #111111;
    color: #ffffff;
    border-radius: 0;
    text-transform: uppercase;
    letter-spacing: 0.1em;
}

body[data-ui-style="4"] .btn:hover {
    background: var(--accent);
}

body[data-ui-style="4"] .about-grid {
    grid-template-columns: 1.15fr 0.85fr;
}

body[data-ui-style="4"] .card-director,
body[data-ui-style="4"] .combined-mv {
    border-radius: 0;
    box-shadow: none;
    border: 1px solid #111111;
}

body[data-ui-style="4"] .values-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 2.5rem 2rem;
    counter-reset: valor;
}

/* Valores numerados 01-06 en lugar de tarjetas con icono */
body[data-ui-style="4"] .value-item {
    counter-increment: valor;
    padding: 1.25rem 0 0;
    background: transparent;
    border: none;
    border-top: 1px solid #111111;
    border-radius: 0;
    box-shadow: none;
}

body[data-ui-style="4"] .value-item:hover {
    transform: none;
}

body[data-ui-style="4"] .value-item::before {
    content: counter(valor, decimal-leading-zero);
    display: block;
    margin-bottom: 0.75rem;
    color: var(--accent);
    font-family: var(--font-heading);
    font-size: 3.2rem;
    font-weight: 800;
    line-height: 1;
}

body[data-ui-style="4"] .value-item i {
    display: none;
}

body[data-ui-style="4"] .services-container {
    grid-template-columns: repeat(6, minmax(0, 1fr));
    gap: 1.25rem;
    padding: 0;
}

body[data-ui-style="4"] .service-block {
    grid-column: span 3;
    padding: 2.25rem 2rem;
    border: 1px solid #111111;
    border-top: 5px solid var(--accent);
    border-radius: 0;
    box-shadow: none;
}

body[data-ui-style="4"] .service-block:nth-child(4n + 1),
body[data-ui-style="4"] .service-block:nth-child(4n + 4) {
    grid-column: span 4;
}

body[data-ui-style="4"] .service-block:nth-child(4n + 2),
body[data-ui-style="4"] .service-block:nth-child(4n + 3) {
    grid-column: span 2;
}

body[data-ui-style="4"] .gallery-grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 0.75rem;
}

/* Fotos en blanco y negro que recuperan el color al pasar el cursor */
body[data-ui-style="4"] .gallery-item {
    aspect-ratio: 3 / 4;
    border-radius: 0;
    box-shadow: none;
}

body[data-ui-style="4"] .gallery-item img {
    filter: grayscale(1) contrast(1.05);
    transition: filter 0.4s ease, transform 0.4s ease;
}

body[data-ui-style="4"] .gallery-item:hover img {
    filter: none;
}

@media (max-width: 980px) {
    body[data-ui-style="4"] .about-grid {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="4"] .values-grid,
    body[data-ui-style="4"] .services-container {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    body[data-ui-style="4"] .service-block,
    body[data-ui-style="4"] .service-block:nth-child(n) {
        grid-column: auto;
    }

    body[data-ui-style="4"] .gallery-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
}

@media (max-width: 768px) {
    body[data-ui-style="4"] {
        --hero-h: 42vh;
    }

    body[data-ui-style="4"] .values-grid,
    body[data-ui-style="4"] .services-container {
        grid-template-columns: 1fr;
    }
}

```

### Archivo: `css/formatos/f5-jardin.css`
```css
@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;800&display=swap');

/* 5. JARDÍN VIVO: formas suaves, pasteles, mosaico orgánico */
body[data-ui-style="5"] {
    --font-heading: 'Nunito', sans-serif;
    --font-body: 'Nunito', sans-serif;
    --primary: #765985;
    --primary-dark: #4c385b;
    --accent: #ed927d;
    --bg-light: #fff3eb;
    --text-main: #3e3445;
    --text-muted: #766b79;
    --white: #fffdfb;
    --card-radius: 24px;
    --shadow: 0 14px 34px rgba(76, 56, 91, 0.1);
    --border-card: 1px solid rgba(118, 89, 133, 0.12);
}

body[data-ui-style="5"] header {
    top: 14px;
    left: 5%;
    width: 90%;
    padding: 0.7rem 1.6rem;
    border-radius: 999px;
    box-shadow: 0 10px 30px rgba(76, 56, 91, 0.14);
}

body[data-ui-style="5"] section {
    padding: 6rem 8% 4.5rem;
}

body[data-ui-style="5"] .hero {
    min-height: 640px;
    padding-top: 7rem;
    border-radius: 0 0 56px 56px;
    color: var(--primary-dark);
    background: linear-gradient(180deg, rgba(255, 243, 235, 0.6), rgba(255, 243, 235, 0.88)), var(--hero-image) center/cover no-repeat;
}

body[data-ui-style="5"] .hero-content h1 {
    color: var(--primary-dark);
    letter-spacing: 0;
}

body[data-ui-style="5"] .hero-content h2 {
    color: var(--primary);
}

body[data-ui-style="5"] .btn {
    background: var(--accent);
    color: var(--primary-dark);
}

body[data-ui-style="5"] #mision-vision {
    background: var(--white);
}

body[data-ui-style="5"] #valores {
    background: #f1e8f6;
}

body[data-ui-style="5"] .value-item:nth-child(odd) {
    background: var(--white);
}

body[data-ui-style="5"] .value-item:nth-child(even) {
    background: #fbd9cf;
}

body[data-ui-style="5"] .services-container {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1.5rem;
    padding: 0;
}

body[data-ui-style="5"] .service-block {
    --card-bg: #eadcf3;
    padding: 2.25rem 2rem;
    background: var(--card-bg);
    border: none;
}

body[data-ui-style="5"] .service-block:nth-child(4n + 2),
body[data-ui-style="5"] .service-block:nth-child(4n + 3) {
    --card-bg: #fbd9cf;
}

body[data-ui-style="5"] .gallery-grid {
    display: block;
    column-count: 3;
    column-gap: 1rem;
}

body[data-ui-style="5"] .gallery-item {
    display: block;
    margin-bottom: 1rem;
    break-inside: avoid;
    aspect-ratio: auto;
    height: 260px;
    border: none;
    border-radius: 28px;
}

body[data-ui-style="5"] .gallery-item[data-pos="1"],
body[data-ui-style="5"] .gallery-item[data-pos="5"] {
    height: 320px;
}

body[data-ui-style="5"] .gallery-item[data-pos="2"],
body[data-ui-style="5"] .gallery-item[data-pos="6"] {
    height: 220px;
}

body[data-ui-style="5"] .social-icons a {
    background: var(--primary);
}

@media (max-width: 980px) {
    body[data-ui-style="5"] .services-container {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="5"] .gallery-grid {
        column-count: 2;
    }
}

@media (max-width: 768px) {
    body[data-ui-style="5"] header {
        top: 8px;
        left: 3%;
        width: 94%;
        padding: 0.6rem 1.1rem;
        border-radius: 28px;
    }

    body[data-ui-style="5"] .hero {
        border-radius: 0 0 32px 32px;
    }

    body[data-ui-style="5"] .gallery-grid {
        column-count: 1;
    }

    body[data-ui-style="5"] .gallery-item,
    body[data-ui-style="5"] .gallery-item[data-pos] {
        height: 230px;
    }
}

```

### Archivo: `css/formatos/f6-brutalista.css`
```css
@import url('https://fonts.googleapis.com/css2?family=Archivo+Black&family=Space+Mono:wght@400;700&display=swap');

/* 6. BRUTALISTA POP: colores planos, bordes negros gruesos y sombras duras */
body[data-ui-style="6"] {
    --primary: #1f1fff;
    --primary-dark: #000000;
    --accent: #ff4f9a;
    --bg-light: #ffe14d;
    --text-main: #000000;
    --text-muted: #2b2b2b;
    --white: #ffffff;
    --card-radius: 0;
    --shadow: 6px 6px 0 #000000;
    --border-card: 3px solid #000000;
    --font-heading: 'Archivo Black', 'Arial Black', sans-serif;
    --font-body: 'Space Mono', monospace;
}

body[data-ui-style="6"] header {
    background: #ffffff;
    border-bottom: 4px solid #000000;
    box-shadow: none;
    backdrop-filter: none;
}

body[data-ui-style="6"] nav a {
    padding: 0.3rem 0.7rem;
    border: 2px solid #000000;
    background: #ffffff;
    font-size: 0.8rem;
    font-weight: 700;
    text-transform: uppercase;
}

body[data-ui-style="6"] nav a:hover {
    background: var(--accent);
    color: #000000;
    box-shadow: 3px 3px 0 #000000;
}

body[data-ui-style="6"] section {
    padding: 5.5rem 8% 4.5rem;
}

body[data-ui-style="6"] .section-title {
    display: inline-block;
    margin-bottom: 3rem;
    padding: 0.4rem 1.2rem;
    background: var(--accent);
    border: 3px solid #000000;
    box-shadow: var(--shadow);
    color: #000000;
    font-size: clamp(1.8rem, 4vw, 2.8rem);
    text-transform: uppercase;
    transform: rotate(-1deg);
}

body[data-ui-style="6"] .section-title::after {
    display: none;
}

body[data-ui-style="6"] .hero {
    height: auto;
    min-height: 100vh;
    justify-content: flex-start;
    padding: 8rem 8% 4rem;
    border-bottom: 4px solid #000000;
    text-align: left;
    background: var(--hero-image) center/cover no-repeat, var(--primary);
}

body[data-ui-style="6"] .hero-content {
    max-width: 640px;
    padding: 2rem;
    background: #ffffff;
    border: 4px solid #000000;
    box-shadow: 10px 10px 0 #000000;
    color: #000000;
}

body[data-ui-style="6"] .hero-content h1 {
    color: #000000;
    font-size: clamp(2.4rem, 6vw, 4.4rem);
    letter-spacing: -0.02em;
    line-height: 1;
    text-transform: uppercase;
}

body[data-ui-style="6"] .hero-content h2 {
    color: var(--primary);
    font-style: normal;
    font-weight: 700;
}

body[data-ui-style="6"] .hero-content p {
    margin-left: 0;
}

body[data-ui-style="6"] .btn {
    padding: 0.8rem 1.6rem;
    background: var(--primary);
    border: 3px solid #000000;
    border-radius: 0;
    box-shadow: 5px 5px 0 #000000;
    color: #ffffff;
    font-weight: 700;
    text-transform: uppercase;
}

body[data-ui-style="6"] .btn:hover {
    background: var(--accent);
    color: #000000;
    box-shadow: 2px 2px 0 #000000;
    transform: translate(3px, 3px);
}

body[data-ui-style="6"] #mision-vision {
    background: #ffffff;
}

body[data-ui-style="6"] #valores {
    background: #ffd1ec;
}

body[data-ui-style="6"] #servicios {
    background: #a8f5c8 !important;
}

body[data-ui-style="6"] #instalaciones {
    background: #c9d6ff;
}

body[data-ui-style="6"] #galeria {
    background: var(--bg-light);
}

body[data-ui-style="6"] #contacto {
    background: #ffffff;
}

body[data-ui-style="6"] .about-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 2rem;
}

body[data-ui-style="6"] .card-director,
body[data-ui-style="6"] .combined-mv,
body[data-ui-style="6"] .service-block,
body[data-ui-style="6"] .value-item {
    background: #ffffff;
    border: 3px solid #000000;
    border-radius: 0;
    box-shadow: var(--shadow);
}

body[data-ui-style="6"] .director-img {
    border-radius: 0 !important;
    border: 3px solid #000000;
}

body[data-ui-style="6"] .values-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 2rem;
}

body[data-ui-style="6"] .value-item:nth-child(odd) {
    transform: rotate(-1deg);
}

body[data-ui-style="6"] .value-item:nth-child(even) {
    transform: rotate(1deg);
}

body[data-ui-style="6"] .value-item:hover {
    transform: translate(-3px, -3px);
    box-shadow: 9px 9px 0 #000000;
}

body[data-ui-style="6"] .value-item i {
    color: var(--primary);
}

body[data-ui-style="6"] .services-container {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 2rem;
    padding: 0;
}

body[data-ui-style="6"] .service-block {
    padding: 2rem;
}

body[data-ui-style="6"] .gallery-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 1.25rem;
}

body[data-ui-style="6"] .gallery-item {
    border: 3px solid #000000;
    border-radius: 0;
    box-shadow: var(--shadow);
}

body[data-ui-style="6"] .gallery-filters {
    justify-content: flex-start;
}

body[data-ui-style="6"] .filter-btn {
    border: 3px solid #000000;
    border-radius: 0;
    background: #ffffff;
    color: #000000;
    font-weight: 700;
    text-transform: uppercase;
}

body[data-ui-style="6"] .filter-btn.active {
    background: var(--primary);
    color: #ffffff;
    box-shadow: 3px 3px 0 #000000;
}

body[data-ui-style="6"] .contact-content {
    margin: 0;
    text-align: left;
}

body[data-ui-style="6"] .social-icons {
    justify-content: flex-start;
}

body[data-ui-style="6"] .social-icons a {
    border: 3px solid #000000;
    border-radius: 0;
    background: var(--accent);
    box-shadow: 3px 3px 0 #000000;
    color: #000000;
}

body[data-ui-style="6"] footer {
    background: #000000;
    border-top: 4px solid #000000;
    color: var(--bg-light);
}

@media (max-width: 980px) {
    body[data-ui-style="6"] .about-grid,
    body[data-ui-style="6"] .services-container {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="6"] .values-grid,
    body[data-ui-style="6"] .gallery-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
}

@media (max-width: 768px) {
    body[data-ui-style="6"] .values-grid,
    body[data-ui-style="6"] .gallery-grid {
        grid-template-columns: 1fr;
    }

    body[data-ui-style="6"] nav ul {
        background: #ffffff;
        border-bottom: 4px solid #000000;
    }
}

```

### Archivo: `css/formatos/f7-app.css`
```css
/* 7. APP MÓVIL: el Original en el celular, una pantalla a la vez con barra de pestañas (ver main.js).
   Hereda colores y tipografías del Original; en escritorio se ve igual que él. */
#app-tabs {
    display: none;
}

@media (max-width: 768px) {
    body[data-ui-style="7"] {
        padding-bottom: 76px;
    }

    body[data-ui-style="7"] nav:not(#app-tabs),
    body[data-ui-style="7"] .menu-toggle {
        display: none;
    }

    body[data-ui-style="7"] header {
        justify-content: center;
        padding: 0.7rem 5%;
    }

    body[data-ui-style="7"] section {
        display: none;
        min-height: calc(100vh - 140px);
        padding: 5.5rem 6% 2rem;
        scroll-margin-top: 0;
    }

    body[data-ui-style="7"] section.tab-visible {
        display: block;
    }

    body[data-ui-style="7"] #inicio.tab-visible {
        display: flex;
    }

    body[data-ui-style="7"] .section-title {
        margin-bottom: 2rem;
        font-size: 2rem;
    }

    body[data-ui-style="7"] .hero {
        height: auto;
        min-height: calc(100vh - 76px);
    }

    body[data-ui-style="7"] .hero-content h1 {
        font-size: clamp(2.2rem, 10vw, 3.2rem);
    }

    body[data-ui-style="7"] .gallery-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 0.75rem;
    }

    body[data-ui-style="7"] .gallery-filters {
        justify-content: flex-start;
        flex-wrap: nowrap;
        overflow-x: auto;
    }

    /* Barra de pestañas inferior con los colores del Original */
    #app-tabs {
        position: fixed;
        inset: auto 0 0 0;
        z-index: 1400;
        display: flex;
        justify-content: center;
        gap: 0.25rem;
        padding: 0.4rem 0.5rem calc(0.4rem + env(safe-area-inset-bottom));
        background: var(--white);
        border-top: 1px solid rgba(69, 107, 140, 0.18);
        box-shadow: 0 -4px 16px rgba(0, 0, 0, 0.06);
    }

    #app-tabs button {
        flex: 1 1 0;
        max-width: 120px;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 0.2rem;
        padding: 0.4rem 0.2rem;
        border: none;
        border-radius: 14px;
        background: transparent;
        color: var(--text-muted);
        font: inherit;
        font-size: 0.68rem;
        font-weight: 600;
        cursor: pointer;
    }

    #app-tabs button i {
        font-size: 1.25rem;
    }

    #app-tabs button.active {
        background: var(--bg-light);
        color: var(--primary-dark);
    }

    /* Los botones flotantes suben para no tapar la barra */
    body[data-ui-style="7"] .whatsapp-float,
    body[data-ui-style="7"] .ui-style-control {
        bottom: 92px;
    }

    body[data-ui-style="7"] footer {
        display: none;
    }
}

```

### Archivo: `css/styles.css`
```css
/* Fuentes */
/* Fuentes: se cargan una sola vez desde index.html (antes también se importaban acá, duplicado). */

/* ============================================================
   VARIABLES DE ESTÉTICA (Verde Oliva y Tono Madera)
   ============================================================ */
:root {
    --hero-image: none;
    --primary: #78A6C8;
    --primary-dark: #456B8C;
    --accent: #D99B78;
    --bg-light: #F3F8FC;
    --text-main: #263B4D;
    --text-muted: #6B7280;
    --white: #FFFFFF;
    --font-heading: 'Playfair Display', serif;
    --font-body: 'Plus Jakarta Sans', sans-serif;
    --card-radius: 16px;
    --shadow: 0 10px 30px rgba(0,0,0,0.04);
    --border-card: 1px solid rgba(69, 107, 140, 0.14);
}

:root {
    --transition: all 0.3s ease;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html,
body {
    overflow-x: hidden;
}

img {
    max-width: 100%;
}

h1,
h2,
h3,
p,
li,
a,
button,
select {
    overflow-wrap: anywhere;
}

body {
    font-family: var(--font-body);
    background-color: var(--bg-light);
    color: var(--text-main);
    line-height: 1.6;
    transition: background-color 0.4s ease, color 0.4s ease;
}

h1, h2, h3 {
    font-family: var(--font-heading);
}

/* Header & Nav */
header {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    box-shadow: 0 2px 10px rgba(0,0,0,0.05);
    z-index: 1000;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1.2rem 5%;
}

.logo {
    font-family: var(--font-heading);
    font-size: 1.8rem;
    font-weight: 700;
    color: var(--primary);
    letter-spacing: 1px;
}

nav ul {
    display: flex;
    list-style: none;
    gap: 2rem;
}

nav a {
    text-decoration: none;
    color: var(--text-main);
    font-weight: 500;
    transition: var(--transition);
}

nav a:hover {
    color: var(--primary);
}

.menu-toggle {
    display: none;
    font-size: 1.5rem;
    cursor: pointer;
    color: var(--primary);
}

/* Hero Section */
.hero {
    height: 100vh;
    background: linear-gradient(rgba(38, 59, 77, 0.5), rgba(38, 59, 77, 0.64)), var(--hero-image) center/cover no-repeat;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    color: #FFFFFF;
    padding: 0 1rem;
    margin-top: 0;
}

.hero-content h1 {
    font-size: 4rem;
    margin-bottom: 0.5rem;
    letter-spacing: 2px;
}

.hero-content h2 {
    font-size: 1.5rem;
    font-weight: 400;
    font-style: italic;
    margin-bottom: 1.5rem;
    color: #F3EFEA;
}

.hero-content p {
    font-size: 1.1rem;
    max-width: 600px;
    margin: 0 auto 2rem auto;
}

.btn {
    display: inline-block;
    background-color: var(--primary);
    color: #FFFFFF;
    padding: 0.8rem 2rem;
    border-radius: 30px;
    text-decoration: none;
    font-weight: 600;
    transition: var(--transition);
    border: none;
    cursor: pointer;
}

.btn:hover {
    background-color: var(--primary-dark);
    transform: translateY(-2px);
}

/* Sections General Styling */
section {
    padding: 6rem 10% 4rem 10%;
}

.section-title {
    text-align: center;
    font-size: 2.5rem;
    color: var(--primary);
    margin-bottom: 3rem;
    position: relative;
}

.section-title::after {
    content: '';
    display: block;
    width: 60px;
    height: 3px;
    background-color: var(--accent);
    margin: 0.5rem auto 0 auto;
}

/* =========================================
   SECCIÓN SOBRE NOSOTROS (Misión/Visión + Director)
   ========================================= */
.about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2.5rem;
    align-items: stretch;
}

.card-director, .combined-mv {
    background: var(--white);
    padding: 2.5rem;
    border-radius: var(--card-radius);
    box-shadow: var(--shadow);
    border: var(--border-card);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: background-color 0.4s ease, border-radius 0.4s ease;
}

.card-director {
    border-top: 4px solid var(--accent);
}

.combined-mv {
    border-top: 4px solid var(--primary);
}

.director-header {
    display: flex;
    align-items: center;
    gap: 1.5rem;
    margin-bottom: 1.5rem;
    border-bottom: 2px solid var(--bg-light);
    padding-bottom: 1.5rem;
}

.director-img {
    width: 90px !important;
    height: 110px !important;
    min-width: 90px !important;
    min-height: 110px !important;
    border-radius: 12px !important;
    object-fit: cover !important;
    object-position: top center !important;
    border: 3px solid var(--accent);
    flex-shrink: 0;
}

.director-header h3 {
    color: var(--primary);
    font-size: 1.4rem;
    margin-bottom: 0.2rem;
}

.director-name {
    color: var(--text-main);
    font-weight: 700;
    font-size: 1.05rem;
    margin-bottom: 0.15rem;
}

.director-subtitle {
    color: var(--text-muted);
    font-size: 0.95rem;
    font-weight: 500;
}

/* Contenedores expandibles */
.expandable-container {
    max-height: 180px;
    overflow: hidden;
    position: relative;
    transition: max-height 0.5s ease-in-out;
}

.expandable-container::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 50px;
    background: linear-gradient(transparent, var(--white));
    pointer-events: none;
    transition: opacity 0.3s ease;
}

.expandable-container.expanded {
    max-height: 2000px;
}

.expandable-container.expanded::after {
    opacity: 0;
}

.director-text p {
    text-align: justify;
    color: var(--text-main);
    font-size: 0.95rem;
    line-height: 1.7;
    margin-bottom: 1rem;
}

.expandable-btn-container {
    margin-top: 1.5rem;
    text-align: center;
    border-top: 1px solid var(--bg-light);
    padding-top: 1rem;
}

.btn-leer-mas {
    background: transparent;
    border: 2px solid var(--accent);
    color: var(--primary-dark);
    padding: 0.5rem 1.5rem;
    border-radius: 20px;
    cursor: pointer;
    font-weight: 600;
    font-size: 0.95rem;
    transition: var(--transition);
}

.btn-leer-mas:hover {
    background: var(--accent);
    color: #FFFFFF;
    transform: translateY(-2px);
}

.mv-block h3 {
    color: var(--primary);
    margin-bottom: 1rem;
    font-size: 1.4rem;
    display: flex;
    align-items: center;
    gap: 0.8rem;
}

.mv-block p {
    text-align: justify;
    color: var(--text-main);
    line-height: 1.7;
}

.mv-divider {
    height: 1px;
    background-color: var(--bg-light);
    margin: 2rem 0;
}

/* Valores */
#valores {
    background-color: var(--white);
}

.values-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
}

.values-grid > #valores-dinamicos {
    display: contents;
}

.value-item {
    background: var(--bg-light);
    padding: 2rem;
    border-radius: var(--card-radius);
    box-shadow: var(--shadow);
    border: var(--border-card);
    transition: var(--transition);
}

.value-item:hover {
    transform: translateY(-3px);
}

.value-item i {
    font-size: 2rem;
    color: var(--accent);
    margin-bottom: 1rem;
}

.value-item h4 {
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
    color: var(--primary-dark);
}

/* =========================================
   SECCIÓN SERVICIOS
   ========================================= */
.services-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(100%, 280px), 1fr));
    gap: 2.5rem;
    padding: 0 5%;
    align-items: stretch;
}

.service-block {
    background: var(--white);
    padding: 3rem 2.5rem;
    border-radius: var(--card-radius);
    box-shadow: var(--shadow);
    border: var(--border-card);
    transition: var(--transition);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.service-block:hover {
    transform: translateY(-5px);
}

.service-block h3 {
    color: var(--primary);
    font-size: 1.5rem;
    margin-bottom: 1.5rem;
    display: flex;
    align-items: center;
    gap: 1rem;
    border-bottom: 2px solid var(--bg-light);
    padding-bottom: 1.5rem;
}

.service-block h3 i {
    background: var(--bg-light);
    color: var(--accent);
    width: 55px;
    height: 55px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    font-size: 1.5rem;
    flex-shrink: 0;
}

.service-block p {
    margin-bottom: 1.5rem;
    text-align: justify;
    color: var(--text-main);
    line-height: 1.7;
}

.service-list {
    list-style: none;
    padding: 0;
}

.service-list li {
    margin-bottom: 1.5rem;
    position: relative;
    padding-left: 2.5rem;
    text-align: justify;
    color: var(--text-muted);
    line-height: 1.6;
}

.service-list li::before {
    content: '\f105';
    font-family: 'Font Awesome 6 Free';
    font-weight: 900;
    position: absolute;
    left: 0;
    top: 2px;
    color: var(--accent);
    font-size: 1.2rem;
}

.service-list li strong {
    color: var(--primary-dark);
    display: block;
    margin-bottom: 0.3rem;
    font-size: 1.05rem;
}

/* Galería de Fotos */
.gallery-container {
    background-color: var(--bg-light);
    padding: 5rem 10%;
}

.gallery-filters {
    display: flex;
    justify-content: center;
    gap: 1rem;
    margin-bottom: 2rem;
    flex-wrap: wrap;
}

.filter-btn {
    background: transparent;
    border: 2px solid var(--primary);
    color: var(--primary);
    padding: 0.5rem 1.2rem;
    border-radius: 20px;
    cursor: pointer;
    font-weight: 600;
    transition: var(--transition);
}

.filter-btn.active, .filter-btn:hover {
    background: var(--primary);
    color: var(--white);
}

.gallery-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(100%, 280px), 1fr));
    gap: 1.5rem;
}

.gallery-item {
    position: relative;
    border-radius: var(--card-radius);
    overflow: hidden;
    height: 250px;
    box-shadow: var(--shadow);
    border: var(--border-card);
    cursor: pointer;
}

.gallery-item img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.5s ease;
}

.gallery-item:hover img {
    transform: scale(1.08);
}

.gallery-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    background: linear-gradient(transparent, rgba(0,0,0,0.8));
    color: #FFFFFF;
    padding: 1.5rem 1rem 1rem 1rem;
    opacity: 0;
    transition: opacity 0.3s ease;
}

.gallery-item:hover .gallery-overlay {
    opacity: 1;
}

/* Galería completa en modal */
.gallery-more {
    margin-top: 2.5rem;
    text-align: center;
}

.gallery-modal {
    position: fixed;
    inset: 0;
    z-index: 1800;
    overflow-y: auto;
    padding: 4rem 5% 3rem;
    background: var(--bg-light);
}

.gallery-modal[hidden] {
    display: none;
}

.gallery-modal h3 {
    margin-bottom: 2rem;
    color: var(--primary);
    font-size: 2rem;
    text-align: center;
}

.gallery-modal-close {
    position: fixed;
    top: 14px;
    right: 22px;
    z-index: 1;
    width: 44px;
    height: 44px;
    border: none;
    border-radius: 50%;
    background: var(--primary);
    color: #FFFFFF;
    font-size: 1.8rem;
    line-height: 1;
    cursor: pointer;
}

.gallery-modal-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(min(100%, 260px), 1fr));
    gap: 1.25rem;
    max-width: 1200px;
    margin: 0 auto;
}

/* Lightbox Modal */
.lightbox {
    display: none;
    position: fixed;
    z-index: 2000;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.85);
    justify-content: center;
    align-items: center;
    padding: 2rem;
}

.lightbox img {
    max-width: 90%;
    max-height: 85vh;
    border-radius: 5px;
}

.lightbox-close {
    position: absolute;
    top: 20px;
    right: 30px;
    color: white;
    font-size: 2.5rem;
    cursor: pointer;
}

/* Contacto */
.contact-content {
    text-align: center;
    max-width: 600px;
    margin: 0 auto;
}

.social-icons {
    display: flex;
    justify-content: center;
    gap: 1.5rem;
    margin-top: 2rem;
}

.social-icons a {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 50px;
    height: 50px;
    background: var(--primary);
    color: var(--white);
    border-radius: 50%;
    font-size: 1.3rem;
    text-decoration: none;
    transition: var(--transition);
}

.social-icons a:hover {
    background: var(--accent);
    transform: translateY(-3px);
}

/* WhatsApp Flotante */
.whatsapp-float {
    position: fixed;
    bottom: 30px;
    right: 30px;
    background: #25D366;
    color: white;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    box-shadow: 0 4px 10px rgba(0,0,0,0.2);
    z-index: 1500;
    transition: var(--transition);
    text-decoration: none;
}

.whatsapp-float:hover {
    transform: scale(1.1);
}

/* Selector de formato flotante (opuesto a WhatsApp) */
.ui-style-control {
    position: fixed;
    bottom: 30px;
    left: 30px;
    z-index: 1500;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.5rem 0.9rem;
    background: #FFFFFF;
    color: #263B4D;
    border-radius: 999px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.2);
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
}

.ui-style-control[hidden] {
    display: none;
}

.ui-style-control select {
    padding: 0.3rem 0.6rem;
    border: 1px solid var(--accent);
    border-radius: 999px;
    background: #FFFFFF;
    color: #263B4D;
    font: inherit;
    font-size: 0.8rem;
    font-weight: 600;
    letter-spacing: 0;
    text-transform: none;
    cursor: pointer;
}

@media (max-width: 768px) {
    .ui-style-control {
        bottom: 20px;
        left: 20px;
    }

    .ui-style-control span {
        display: none;
    }
}

/* Footer */
footer {
    background: var(--primary-dark);
    color: #FFFFFF;
    text-align: center;
    padding: 2rem;
    font-size: 0.9rem;
}

/* Responsive */
@media (max-width: 900px) {
    .about-grid {
        grid-template-columns: 1fr;
    }
}

@media (max-width: 768px) {
    nav ul {
        display: none;
        flex-direction: column;
        position: absolute;
        top: 100%;
        left: 0;
        width: 100%;
        background: var(--white);
        padding: 1.5rem;
        box-shadow: 0 5px 10px rgba(0,0,0,0.05);
        text-align: center;
    }
    nav ul.active {
        display: flex;
    }
    .menu-toggle {
        display: block;
    }
    .hero-content h1 {
        font-size: 2.8rem;
    }
    .services-container {
        grid-template-columns: 1fr;
        padding: 0;
    }
    .service-block {
        padding: 2rem 1.5rem;
    }
}


```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/Residencial_Blanco/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="" data-config-brand-attr="content:DESCRIPCION">
    <title data-config-brand="TITULO">Residencial Geriátrico</title>
    
    <!-- Preconexiones para acelerar DNS -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link rel="preconnect" href="https://cdnjs.cloudflare.com" crossorigin>
    <link rel="preconnect" href="https://res.cloudinary.com" crossorigin>

    <!-- Fuentes y CSS externos cargados sin bloquear el renderizado -->
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Plus+Jakarta+Sans:wght@400;500;600&display=swap" rel="stylesheet" media="print" onload="this.media='all'">
    <noscript><link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=Plus+Jakarta+Sans:wght@400;500;600&display=swap" rel="stylesheet"></noscript>

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" media="print" onload="this.media='all'">
    <noscript><link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"></noscript>

    <!-- Tu CSS principal -->
    <link rel="stylesheet" href="css/styles.css">

</head>
<body>

    <!-- Header -->
    <header>
        <div class="logo">
            <img src="" alt="" data-config-brand-attr="src:LOGO" width="238" height="118">
        </div>

        <div class="menu-toggle" id="mobile-menu">
            <i class="fas fa-bars"></i>
        </div>
        <nav>
            <ul id="nav-list">
                <li><a href="#inicio" data-config-nav="0">Inicio</a></li>
                <li><a href="#mision-vision" data-config-nav="1">Nosotros</a></li>
                <li><a href="#valores" data-config-nav="2">Valores</a></li>
                <li><a href="#servicios" data-config-nav="3">Servicios</a></li>
                <li><a href="#instalaciones" data-config-nav="4">Instalaciones</a></li>
                <li><a href="#galeria" data-config-nav="5">Galería</a></li>
                <li><a href="#contacto" data-config-nav="6">Contacto</a></li>
            </ul>
        </nav>
    </header>

    <!-- Hero -->
    <section id="inicio" class="hero">
        <div class="hero-content">
            <h1 data-config-brand="NOMBRE"></h1>
            <h2 data-config-text="LEMA"></h2>
            <p data-config-text="HERO_DESCRIPCION"></p>
            <a href="#contacto" class="btn" data-config-text="CTA_CONTACTO"></a>
        </div>
    </section>

    <!-- Perfil del Director (Izquierda) y Misión/Visión (Derecha) -->
    <section id="mision-vision">
        <h2 class="section-title" data-config-text="TITULO_NOSOTROS"></h2>
        <div class="about-grid">
            
            <!-- Columna Izquierda: Perfil del Director con botón expandible -->
            <div class="card-director">
                <div>
                    <div class="director-header">
                        <img src="" alt="" data-config-brand-attr="src:IMAGEN_DIRECTOR" data-config-attr="alt:TEXTOS:DIRECTOR_ALT" class="director-img" width="240" height="323">
                        <div>
                            <h3 data-config-text="TITULO_DIRECTOR"></h3>
                            <p class="director-name" data-config-text="NOMBRE_DIRECTOR"></p>
                            <span class="director-subtitle" data-config-text="CARGO_DIRECTOR"></span>
                        </div>
                    </div>

                    <div class="expandable-container" id="director-text-container">
                        <div class="expandable-content">
                            <p class="content-subtitle" data-config-text="COMPROMISO_DIRECTOR"></p>
                            <div data-config-director-body></div>
                        </div>
                    </div>
                </div>

                <div class="expandable-btn-container">
                    <button class="btn-leer-mas" data-target="director-text-container">Ver más</button>
                </div>
            </div>

            <!-- Columna Derecha: Misión y Visión unificadas -->
            <div class="card-mv combined-mv">
                <div>
                    <div class="mv-block">
                        <h3><i class="fas fa-bullseye"></i> <span data-config-text="TITULO_MISION">Nuestra Misión</span></h3>
                        <p data-config-text="MISION"></p>
                    </div>
                    
                    <div class="mv-divider"></div>

                    <div class="mv-block">
                        <h3><i class="fas fa-eye"></i> <span data-config-text="TITULO_VISION"></span></h3>
                        <p data-config-text="VISION"></p>
                    </div>
                </div>
            </div>

        </div>
    </section>

    <!-- Valores -->
    <section id="valores">
        <h2 class="section-title" data-config-text="TITULO_VALORES"></h2>
        <div class="values-grid">
            <div id="valores-dinamicos"></div>
        </div>
    </section>

    <!-- Servicios -->
    <section id="servicios" style="background-color: var(--bg-light);">
        <h2 class="section-title" data-config-text="TITULO_SERVICIOS"></h2>
        <div class="services-container">
            
            <!-- Instalaciones y Comodidades -->
            <div class="service-block" data-config-service="INSTALACIONES">
                <div>
                    <h3><i class="fas fa-home"></i> <span data-config-service-title>Instalaciones</span></h3>
                    <p data-config-service-intro></p>
                    
                    <div class="expandable-container" id="service-instalaciones">
                        <div class="expandable-content">
                            <ul class="service-list" data-config-service-details style="margin-top: 1rem;">
                                <li><strong>Habitaciones confortables:</strong> Habitaciones equipadas y calefaccionadas, preparadas para brindar comodidad, descanso y tranquilidad en un ambiente acogedor.</li>
                                <li><strong>Accesibilidad:</strong> Contamos con espacios pensados para facilitar la movilidad, el desplazamiento y la autonomía de nuestros residentes.</li>
                                <li><strong>Baños adaptados:</strong> Baños adaptados para brindar mayor seguridad, comodidad y facilidad de uso, contemplando las necesidades de cada persona.</li>
                                <li><strong>Seguridad y tranquilidad:</strong> Disponemos de sistema de videovigilancia, como parte de las medidas destinadas a favorecer un entorno cuidado y seguro.</li>
                                <li><strong>Conectividad:</strong> Contamos con Wi-Fi, facilitando la comunicación y el contacto con familiares y seres queridos.</li>
                                <li><strong>Áreas comunes:</strong> Contamos con espacios amplios, confortables y luminosos, pensados para compartir, conversar, realizar actividades y disfrutar momentos de encuentro. Además, disponemos de áreas exteriores adaptadas y cuidadas, ideales para disfrutar al aire libre, compartir con familiares y crear momentos especiales en un entorno agradable y tranquilo.</li>
                            </ul>
                        </div>
                    </div>
                </div>

                <div class="expandable-btn-container">
                    <button class="btn-leer-mas" data-target="service-instalaciones">Ver más</button>
                </div>
            </div>
            
            <!-- Alimentación -->
            <div class="service-block service-fixed-height" data-config-service="ALIMENTACION">
                <div>
                    <h3><i class="fas fa-utensils"></i> <span data-config-service-title>Alimentación</span></h3>
                    <p data-config-service-intro></p>
                    <ul class="service-list" data-config-service-details>
                        <li><strong>Servicio completo:</strong> Contamos con desayuno, almuerzo, merienda, cena y colaciones, además de dietas especiales de acuerdo con las necesidades y requerimientos de cada persona.</li>
                        <li><strong>Sentido de hogar:</strong> Buscamos que cada comida sea también un momento agradable para compartir, disfrutar y sentirse como en casa.</li>
                    </ul>
                </div>
            </div>

            <!-- Talleres y Actividades -->
            <div class="service-block" data-config-service="TALLERES">
                <div>
                    <h3><i class="fas fa-puzzle-piece"></i> <span data-config-service-title>Talleres y Actividades</span></h3>
                    <p data-config-service-intro></p>
                    
                    <div class="expandable-container" id="service-talleres">
                        <div class="expandable-content">
                            <ul class="service-list" data-config-service-details style="margin-top: 1rem;">
                                <li><strong>Talleres recreativos:</strong> Actividades pensadas para estimular la participación, el entretenimiento y el vínculo con los demás.</li>
                                <li><strong>Gimnasia funcional:</strong> Ejercicios adaptados que favorecen el movimiento, la movilidad y el mantenimiento de las capacidades físicas.</li>
                                <li><strong>Manualidades y expresión:</strong> Espacios para desarrollar la creatividad, expresarse y disfrutar de actividades que estimulan las habilidades y la imaginación.</li>
                                <li><strong>Momentos de encuentro:</strong> Propuestas para compartir, conversar y fortalecer los vínculos en un ambiente cálido y agradable.</li>
                            </ul>
                        </div>
                    </div>
                </div>

                <div class="expandable-btn-container">
                    <button class="btn-leer-mas" data-target="service-talleres">Ver más</button>
                </div>
            </div>

            <!-- Equipo y Enfermería -->
            <div class="service-block" data-config-service="ENFERMERIA">
                <div>
                    <h3><i class="fas fa-user-nurse"></i> <span data-config-service-title>Equipo, Enfermería</span></h3>
                    <p data-config-service-intro></p>
                    
                    <div class="expandable-container" id="service-enfermeria">
                        <div class="expandable-content">
                            <p style="margin-bottom: 1rem;">Nuestro trabajo incluye:</p>
                            <ul class="service-list" data-config-service-details>
                                <li><strong>Administración:</strong> Control de stock y administración de la medicación, de acuerdo con las indicaciones correspondientes.</li>
                                <li><strong>Control continuo:</strong> Seguimiento diario del estado y las necesidades de cada residente.</li>
                                <li><strong>Comunicación:</strong> Coordinación con profesionales de la salud y referentes familiares cuando corresponde.</li>
                                <li><strong>Atención segura:</strong> Aplicación de protocolos y pautas de cuidado, orientados a brindar una atención segura y responsable.</li>
                                <li><strong>Continuidad del cuidado:</strong> Registro y seguimiento de la información relevante, favoreciendo la continuidad del cuidado.</li>
                            </ul>
                        </div>
                    </div>
                </div>

                <div class="expandable-btn-container">
                    <button class="btn-leer-mas" data-target="service-enfermeria">Ver más</button>
                </div>
            </div>

        </div>
    </section>

    <!-- Galería de Instalaciones Dinámica -->
    <section id="instalaciones" class="gallery-container">
        <h2 class="section-title" data-config-text="TITULO_INSTALACIONES"></h2>
        
        <div class="gallery-filters" id="instalaciones-filtros">
            <button class="filter-btn active" data-filter="all" data-config-filter="0">Todas</button>
            <button class="filter-btn" data-filter="exterior" data-config-filter="1">Exterior</button>
            <button class="filter-btn" data-filter="interior" data-config-filter="2">Áreas Comunes</button>
            <button class="filter-btn" data-filter="habitacion" data-config-filter="3">Habitaciones</button>
        </div>

        <div class="gallery-grid" id="instalaciones-dinamicas">
            <!-- Se carga dinámicamente desde Cloudinary -->
        </div>
        <div class="gallery-more" hidden>
            <button type="button" class="btn" data-galeria-completa="instalaciones-dinamicas" data-config-text="BOTON_VER_GALERIA">Ver galería completa</button>
        </div>
    </section>
    <!-- Galería de Actividades Dinámica -->
    <section id="galeria" class="gallery-container">
        <h2 class="section-title" data-config-text="TITULO_GALERIA"></h2>
        
        <div class="gallery-filters">
            <p data-config-text="MENSAJE_GALERIA" style="text-align: center; color: var(--text-muted); width: 100%;"></p>
        </div>

        <div class="gallery-grid" id="galeria-dinamica">
        </div>
        <div class="gallery-more" hidden>
            <button type="button" class="btn" data-galeria-completa="galeria-dinamica" data-config-text="BOTON_VER_GALERIA">Ver galería completa</button>
        </div>
    </section>

    <!-- Modal de galería completa -->
    <div class="gallery-modal" id="gallery-modal" hidden>
        <button type="button" class="gallery-modal-close" id="gallery-modal-close" aria-label="Cerrar galería">&times;</button>
        <h3 id="gallery-modal-title"></h3>
        <div class="gallery-modal-grid" id="gallery-modal-grid"></div>
    </div>

    <!-- Lightbox Modal -->
    <div class="lightbox" id="lightbox">
        <span class="lightbox-close" id="lightbox-close">&times;</span>
        <img id="lightbox-img" src="" alt="Imagen ampliada">
    </div>

    <!-- Contacto -->
    <section id="contacto">
        <h2 class="section-title" data-config-text="TITULO_CONTACTO"></h2>
        <div class="contact-content">
            <p data-config-text="DESCRIPCION_CONTACTO"></p>
            <p style="margin-top: 1.5rem; font-weight: 500;">
                <i class="fas fa-map-marker-alt" style="color: var(--primary); margin-right: 8px;"></i>
                <strong>DIRECCIÓN:</strong> <span data-config="direccion">Rivera 5734 esquina Vicente Rocafuerte</span>
            </p>
            <div class="social-icons">
                <a href="https://whatsapp.com" data-config="whatsapp" target="_blank" title="WhatsApp" aria-label="WhatsApp"><i class="fab fa-whatsapp"></i></a>
                <a href="https://instagram.com" data-config="instagram" target="_blank" title="Instagram" aria-label="Instagram"><i class="fab fa-instagram"></i></a>
                <a href="https://facebook.com" data-config="facebook" target="_blank" title="Facebook" aria-label="Facebook"><i class="fab fa-facebook-f"></i></a>
                <a href="https://maps.google.com/?q=Rivera+5734+esquina+Vicente+Rocafuerte" data-config="mapa" target="_blank" title="Ubicación" aria-label="Ver ubicación en Google Maps"><i class="fas fa-map-marker-alt"></i></a>
            </div>
        </div>
    </section>

    <!-- Botón flotante de WhatsApp -->
<a href="https://whatsapp.com" data-config="whatsapp-flotante" class="whatsapp-float" target="_blank" aria-label="Chat de WhatsApp">
  <i class="fab fa-whatsapp"></i>
</a>

    <!-- Selector de formato flotante -->
    <label class="ui-style-control" for="ui-style-selector">
        <span>Formato</span>
        <select id="ui-style-selector" aria-label="Formato de interfaz"></select>
    </label>

    <footer>
        <p>&copy; <span data-config-year></span> <span data-config-brand="NOMBRE_COMPLETO"></span>. Todos los derechos reservados.</p>
    </footer>

    <!-- Scripts con defer para no bloquear el renderizado -->
    <script src="js/config.js" defer></script>
    <script src="js/main.js" defer></script>
</body>
</html>
```

### Archivo: `js/config.js`
```javascript
// Configuración de marca blanca.
// Este es el único archivo que debería modificarse para reutilizar la plantilla.
// Debe cargarse ANTES de main.js (ver el final de index.html).
const CONFIG = {

  // --- Identidad y recursos de la marca ---
  MARCA: {
    NOMBRE: 'Nombre de la Marca',
    NOMBRE_COMPLETO: 'Nombre Completo de la Institución',
    TITULO: 'Título del Sitio | Eslogan o Actividad',
    DESCRIPCION: 'Descripción general de la institución o servicio, enfocada en la calidad, el profesionalismo y el bienestar.',
    LOGO: 'https://res.cloudinary.com/p0qlmlor/image/upload/q_auto,f_auto,w_1200/v1/enBlanco/Residencial/personal/logo_transparente.webp',
    IMAGEN_DIRECTOR: 'https://res.cloudinary.com/p0qlmlor/image/upload/q_auto,f_auto,w_1200/v1/enBlanco/Residencial/personal/director.webp',
    IMAGEN_HERO: 'https://res.cloudinary.com/p0qlmlor/image/upload/q_auto,f_auto,w_1200/v1/enBlanco/Residencial/instalaciones/fachada.webp',
    ANO: '2026'
  },

  // --- Apariencia editable de la marca ---
  TEMA: {
    PRIMARIO: '#78A6C8',
    PRIMARIO_OSCURO: '#456B8C',
    ACENTO: '#D99B78',
    FONDO: '#F3F8FC',
    TEXTO: '#263B4D',
    FUENTE_TITULOS: 'Playfair Display',
    FUENTE_TEXTO: 'Plus Jakarta Sans'
  },

  // --- Formato de interfaz ---
  UI: {
    ESTILO: 0,
    MOSTRAR_SELECTOR: true,
    OPCIONES: [
      { ID: 0, NOMBRE: 'Original' },
      { ID: 1, NOMBRE: 'Editorial Lateral', ARCHIVO: 'f1-editorial.css' },
      { ID: 2, NOMBRE: 'Ficha Minimal', ARCHIVO: 'f2-minimal.css' },
      { ID: 3, NOMBRE: 'Cinemático', ARCHIVO: 'f3-cinematico.css' },
      { ID: 4, NOMBRE: 'Revista Bento', ARCHIVO: 'f4-revista.css' },
      { ID: 5, NOMBRE: 'Jardín Vivo', ARCHIVO: 'f5-jardin.css' },
      { ID: 6, NOMBRE: 'Brutalista Pop', ARCHIVO: 'f6-brutalista.css' },
      { ID: 7, NOMBRE: 'Original móvil (pestañas)', ARCHIVO: 'f7-app.css' }
    ]
  },

// --- Textos editables del sitio ---
  TEXTOS: {
    LEMA: '“Innovación y compromiso en cada solución”',
    HERO_DESCRIPCION: 'Brindamos un espacio seguro, eficiente y profesional pensado para el desarrollo y el éxito integral.',
    CTA_CONTACTO: 'Contactate con nosotros',
    TITULO_NOSOTROS: 'Sobre Nosotros',
    TITULO_VALORES: 'Nuestros Valores',
    TITULO_SERVICIOS: 'Nuestros Servicios',
    TITULO_INSTALACIONES: 'Nuestras Instalaciones',
    TITULO_GALERIA: 'Galería',
    MENSAJE_GALERIA: 'Momentos y actividades en nuestras instalaciones',
    TITULO_CONTACTO: 'Contacto',
    DESCRIPCION_CONTACTO: '¿Querés coordinar una visita o tenés alguna consulta? Estamos para escucharte y asesorarte de forma personalizada.',
    BOTON_MAS: 'Ver más',
    BOTON_MENOS: 'Ver menos',
    BOTON_VER_GALERIA: 'Ver galería completa',
    DIRECTOR_ALT: 'Responsable de la institución',
    VALOR_HOGAR: 'Buscamos construir un espacio donde cada persona se sienta valorada, contenida y parte de una comunidad.',
    FALLBACK_GALERIA: 'Galería',
    NAVEGACION: ['Inicio', 'Nosotros', 'Valores', 'Servicios', 'Instalaciones', 'Galería', 'Contacto'],
    FILTROS_INSTALACIONES: ['Todas', 'Exterior', 'Áreas Comunes', 'Espacios'],
    TITULO_DIRECTOR: 'Dirección y Compromiso',
    NOMBRE_DIRECTOR: 'Nombre del Responsable',
    CARGO_DIRECTOR: 'Cargo o Profesión',
    COMPROMISO_DIRECTOR: 'Mi compromiso con {NOMBRE}',
    POR_QUE: '¿Por qué elegirnos?',
    BIENVENIDA: 'Bienvenidos a {NOMBRE}. {LEMA}',
    TITULO_MISION: 'Nuestra Misión',
    TITULO_VISION: 'Nuestra Visión',
    TEXTO_DIRECTOR: [
      'Contamos con una sólida trayectoria en la gestión y desarrollo de proyectos orientados a la excelencia operativa.',
      'Nuestro enfoque se basa en la mejora continua, la innovación y la adaptación a las demandas cambiantes del mercado.',
      'Entendemos que el éxito de cualquier organización radica en la calidad humana y en el compromiso con quienes confían en nosotros.',
      'Por ello, impulsamos una cultura de trabajo basada en la transparencia, la responsabilidad y la atención personalizada.',
      'Nuestro compromiso es seguir evolucionando para ofrecer soluciones que marquen la diferencia.'
    ],
    MISION: 'Ofrecer soluciones integrales y de alta calidad, enfocadas en satisfacer las necesidades de nuestros clientes mediante un servicio profesional, transparente y comprometido con la excelencia.',
    VISION: 'Consolidarnos como una organización referente en nuestro sector, reconocida por la innovación, la calidad humana y la eficacia en cada uno de nuestros servicios.',
    INTRO_INSTALACIONES: 'Espacios diseñados para ofrecer comodidad, funcionalidad y un entorno óptimo.',
    INTRO_ALIMENTACION: 'Soluciones integrales adaptadas a las necesidades específicas de cada cliente o proyecto.',
    INTRO_TALLERES: 'Iniciativas y dinámicas orientadas al crecimiento, la participación y el dinamismo.',
    INTRO_ENFERMERIA: 'Profesionales altamente capacitados para asegurar la excelencia en cada proceso.',
    VALORES: [
      ['Calidez Humana', 'Atendemos desde la cercanía, el afecto y la escucha activa, logrando que cada persona se sienta contenida.'],
      ['Respeto y Dignidad', 'Reconocemos la historia, las preferencias y la individualidad de cada persona, promoviendo un trato digno.'],
      ['Seguridad y Confianza', 'Trabajamos para ofrecer un entorno seguro y tranquilo, generando absoluta confianza en las familias.'],
      ['Profesionalismo', 'Contamos con un equipo capacitado y comprometido para brindar una respuesta responsable y de calidad.'],
      ['Bienestar y Calidad de Vida', 'Promovemos un desarrollo confortable, respetando las necesidades particulares de cada residente o usuario.'],
      ['Sentido de Pertenencia', 'Buscamos construir un espacio donde compartir, integrarse y sentirse verdaderamente como en casa.']
    ],
    SERVICIOS: {
      INSTALACIONES: {
        TITULO: 'Infraestructura',
        INTRO: 'Espacios diseñados para ofrecer comodidad, funcionalidad y un entorno óptimo.',
        DETALLE: '<li><strong>Áreas equipadas:</strong> Zonas adaptadas para optimizar cada tarea y brindar la mejor experiencia.</li><li><strong>Accesibilidad:</strong> Diseño pensado para garantizar la comodidad y el libre desplazamiento.</li><li><strong>Tecnología y confort:</strong> Recursos modernos orientados a facilitar la operativa diaria.</li><li><strong>Seguridad:</strong> Medidas de control y supervisión para un entorno protegido.</li><li><strong>Conectividad:</strong> Conexión permanente para agilizar la comunicación.</li><li><strong>Zonas comunes:</strong> Espacios amplios y versátiles para el encuentro y la gestión.</li>'
      },
      ALIMENTACION: {
        TITULO: 'Prestaciones',
        INTRO: 'Soluciones integrales adaptadas a las necesidades específicas de cada cliente o proyecto.',
        DETALLE: '<li><strong>Servicio a medida:</strong> Propuestas personalizadas según los requerimientos particulares.</li><li><strong>Atención dedicada:</strong> Enfoque orientado a garantizar la satisfacción y el cumplimiento de objetivos.</li>'
      },
      TALLERES: {
        TITULO: 'Programas y Desarrollo',
        INTRO: 'Iniciativas y dinámicas orientadas al crecimiento, la participación y el dinamismo.',
        DETALLE: '<li><strong>Capacitaciones:</strong> Espacios de formación y actualización continua.</li><li><strong>Dinámicas grupales:</strong> Actividades para fomentar la colaboración y la sinergia.</li><li><strong>Proyectos especiales:</strong> Propuestas creativas adaptadas a diferentes objetivos.</li><li><strong>Evaluación y seguimiento:</strong> Instancias para medir resultados y optimizar procesos.</li>'
      },
      ENFERMERIA: {
        TITULO: 'Equipo y Gestión',
        INTRO: 'Profesionales altamente capacitados para asegurar la excelencia en cada proceso.',
        DETALLE: '<p style="margin-bottom: 1rem;">Nuestra gestión incluye:</p><li><strong>Planificación estratégica:</strong> Organización y control sistemático de las operaciones.</li><li><strong>Supervisión continua:</strong> Monitoreo constante para garantizar altos estándares.</li><li><strong>Atención directa:</strong> Canales de comunicación ágiles y efectivos.</li><li><strong>Mejora continua:</strong> Actualización constante de protocolos y metodologías.</li>'
      }
    }
  },

  // --- Galería y Dinámicos (Cloudinary) ---
  CLOUDINARY: {
    CLOUD_NAME: 'p0qlmlor',        // Cloud name de la cuenta
    UPLOAD_PRESET: 'subir_gestor',    // Preset unsigned de subida de Cloudinary
    CARPETA_BASE: 'en_Blanco/Residencial',        // Carpeta raíz para las imágenes subidas
    CARPETA_DEFAULT: 'galeria',     // Subcarpeta seleccionada por defecto
    TAG_GALERIA: 'enBlanco_residencial_galeria',  // Etiqueta que llevan las fotos de la galería
    LIMITE_VISIBLES: 6,            // Fotos mostradas en la página; el resto se ve en "Ver galería completa"
    TAG_INSTALACIONES: 'enBlanco_residencial_instalaciones' // Etiqueta que llevan las fotos de instalaciones en Cloudinary
  },

  // --- Contacto y redes ---
  CONTACTO: {
    DIRECCION: 'Dirección de la institución',
    WHATSAPP_NUMERO: '59890000000',  
    INSTAGRAM_URL: 'https://www.instagram.com/',    
    FACEBOOK_URL: 'https://www.facebook.com/'      
  }
};
```

### Archivo: `js/main.js`
```javascript
document.addEventListener('DOMContentLoaded', () => {
    const cfgMarca = (typeof CONFIG !== 'undefined' && CONFIG.MARCA) ? CONFIG.MARCA : {};
    const cfgTextos = (typeof CONFIG !== 'undefined' && CONFIG.TEXTOS) ? CONFIG.TEXTOS : {};
    const cfgTema = (typeof CONFIG !== 'undefined' && CONFIG.TEMA) ? CONFIG.TEMA : {};
    const cfgUI = (typeof CONFIG !== 'undefined' && CONFIG.UI) ? CONFIG.UI : {};

    function resolverTexto(texto) {
        return String(texto).replace(/\{([^}]+)\}/g, (_, clave) => {
            if (clave === 'NOMBRE') return cfgMarca.NOMBRE || '';
            if (clave === 'LEMA') return cfgTextos.LEMA || '';
            return `{${clave}}`;
        });
    }

    function aplicarConfiguracion() {
        document.querySelectorAll('[data-config-text]').forEach(elemento => {
            const clave = elemento.dataset.configText;
            if (cfgTextos[clave] !== undefined) elemento.textContent = resolverTexto(cfgTextos[clave]);
        });

        document.querySelectorAll('[data-config-brand]').forEach(elemento => {
            const clave = elemento.dataset.configBrand;
            if (cfgMarca[clave] !== undefined) elemento.textContent = resolverTexto(cfgMarca[clave]);
        });

        document.querySelectorAll('[data-config-html]').forEach(elemento => {
            const clave = elemento.dataset.configHtml;
            if (cfgTextos[clave] !== undefined) elemento.innerHTML = resolverTexto(cfgTextos[clave]);
        });

        document.querySelectorAll('[data-config-attr]').forEach(elemento => {
            const [atributo, seccion, clave] = elemento.dataset.configAttr.split(':');
            const fuente = seccion === 'MARCA' ? cfgMarca : cfgTextos;
            if (fuente[clave] !== undefined) elemento.setAttribute(atributo, resolverTexto(fuente[clave]));
        });

        document.querySelectorAll('[data-config-brand-attr]').forEach(elemento => {
            const [atributo, clave] = elemento.dataset.configBrandAttr.split(':');
            if (cfgMarca[clave] !== undefined) {
                elemento.setAttribute(atributo, resolverTexto(cfgMarca[clave]));
            }
        });

        Object.entries(cfgTema).forEach(([clave, valor]) => {
            const nombreVariable = {
                PRIMARIO: '--primary',
                PRIMARIO_OSCURO: '--primary-dark',
                ACENTO: '--accent',
                FONDO: '--bg-light',
                TEXTO: '--text-main',
                FUENTE_TITULOS: '--font-heading',
                FUENTE_TEXTO: '--font-body'
            }[clave];
            if (nombreVariable) document.documentElement.style.setProperty(nombreVariable, valor);
        });

        if (cfgMarca.IMAGEN_HERO) {
            document.documentElement.style.setProperty('--hero-image', `url("${cfgMarca.IMAGEN_HERO}")`);
        }

        if (cfgMarca.TITULO) document.title = cfgMarca.TITULO;
        const descripcion = document.querySelector('meta[name="description"]');
        if (descripcion && cfgMarca.DESCRIPCION) descripcion.setAttribute('content', cfgMarca.DESCRIPCION);

        document.querySelectorAll('[data-config-year]').forEach(elemento => {
            elemento.textContent = cfgMarca.ANO || new Date().getFullYear();
        });
    }

    // Formato 7: una sola pantalla visible a la vez, con barra de pestañas inferior.
    const GRUPOS_APP = [
        { nav: 0, icono: 'fa-house', secciones: ['inicio'] },
        { nav: 1, icono: 'fa-users', secciones: ['mision-vision', 'valores'] },
        { nav: 3, icono: 'fa-hand-holding-heart', secciones: ['servicios'] },
        { nav: 5, icono: 'fa-images', secciones: ['instalaciones', 'galeria'] },
        { nav: 6, icono: 'fa-phone', secciones: ['contacto'] }
    ];

    function mostrarPestana(indice) {
        const grupo = GRUPOS_APP[indice];
        document.querySelectorAll('body > section').forEach(seccion => {
            seccion.classList.toggle('tab-visible', grupo.secciones.includes(seccion.id));
        });
        document.querySelectorAll('#app-tabs button').forEach((boton, i) => {
            boton.classList.toggle('active', i === indice);
        });
        window.scrollTo(0, 0);
    }

    function configurarModoApp(activo) {
        document.getElementById('app-tabs')?.remove();
        document.querySelectorAll('body > section.tab-visible').forEach(seccion => seccion.classList.remove('tab-visible'));
        if (!activo) return;

        const barra = document.createElement('nav');
        barra.id = 'app-tabs';
        barra.setAttribute('aria-label', 'Secciones');
        barra.innerHTML = GRUPOS_APP.map((grupo, i) =>
            `<button type="button" data-tab="${i}"><i class="fas ${grupo.icono}"></i><span>${escaparHtml(cfgTextos.NAVEGACION?.[grupo.nav] ?? '')}</span></button>`
        ).join('');
        barra.addEventListener('click', e => {
            const boton = e.target.closest('button');
            if (boton) mostrarPestana(Number(boton.dataset.tab));
        });
        document.body.appendChild(barra);
        mostrarPestana(0);
    }

    // En el formato 7 los enlaces internos (#contacto, etc.) cambian de pestaña.
    document.addEventListener('click', e => {
        if (document.body.dataset.uiStyle !== '7' || !window.matchMedia('(max-width: 768px)').matches) return;
        const enlace = e.target.closest('a[href^="#"]');
        if (!enlace) return;
        const indice = GRUPOS_APP.findIndex(grupo => grupo.secciones.includes(enlace.getAttribute('href').slice(1)));
        if (indice >= 0) {
            e.preventDefault();
            mostrarPestana(indice);
        }
    });

    // Cada formato tiene su hoja en css/formatos/; se carga solo la del formato activo.
    function cargarCssFormato(estilo) {
        document.querySelectorAll('link[data-formato-css]').forEach(enlace => enlace.remove());
        if (estilo === 0) return;
        const archivo = (cfgUI.OPCIONES.find(opcion => Number(opcion.ID) === estilo) || {}).ARCHIVO;
        ['_comun.css', archivo].filter(Boolean).forEach(nombre => {
            const enlace = document.createElement('link');
            enlace.rel = 'stylesheet';
            enlace.href = `css/formatos/${nombre}`;
            enlace.dataset.formatoCss = '';
            document.head.appendChild(enlace);
        });
    }

    function aplicarFormatoInterfaz(estilo) {
        const estilosDisponibles = Array.isArray(cfgUI.OPCIONES) ? cfgUI.OPCIONES : [];
        const estiloValido = estilosDisponibles.some(opcion => Number(opcion.ID) === Number(estilo)) ? Number(estilo) : 0;
        document.body.dataset.uiStyle = estiloValido;
        cargarCssFormato(estiloValido);
        configurarModoApp(estiloValido === 7);

        const selector = document.getElementById('ui-style-selector');
        if (!selector) return;

        selector.innerHTML = estilosDisponibles.map(opcion =>
            `<option value="${opcion.ID}">${escaparHtml(opcion.NOMBRE)}</option>`
        ).join('');
        selector.value = String(estiloValido);
        selector.closest('.ui-style-control').hidden = cfgUI.MOSTRAR_SELECTOR === false;
        selector.onchange = () => {
            aplicarFormatoInterfaz(selector.value);
            window.localStorage.setItem('residencial-ui-style', selector.value);
        };
    }

    function aplicarContenidosMarca() {
        const valores = document.getElementById('valores-dinamicos');
        if (valores && Array.isArray(cfgTextos.VALORES)) {
            const iconosValores = ['fa-heart', 'fa-hand-holding-heart', 'fa-shield-alt', 'fa-user-md', 'fa-smile', 'fa-home'];
            valores.innerHTML = cfgTextos.VALORES.map((valor, indice) => `
                <div class="value-item">
                    <i class="fas ${iconosValores[indice] || 'fa-star'}"></i>
                    <h3>${escaparHtml(valor[0])}</h3>
                    <p>${escaparHtml(valor[1])}</p>
                </div>
            `).join('');
        }

        document.querySelectorAll('[data-config-nav]').forEach(enlace => {
            const indice = Number(enlace.dataset.configNav);
            if (cfgTextos.NAVEGACION?.[indice] !== undefined) enlace.textContent = cfgTextos.NAVEGACION[indice];
        });

        const director = document.querySelector('[data-config-director-body]');
        if (director && Array.isArray(cfgTextos.TEXTO_DIRECTOR)) {
            const textosDirector = cfgTextos.TEXTO_DIRECTOR.map(texto => escaparHtml(resolverTexto(texto)));
            const cuerpoDirector = textosDirector.map((texto, indice) => {
                const subtitulo = indice === 4
                    ? `<p class="content-subtitle-spaced">${escaparHtml(resolverTexto(cfgTextos.POR_QUE || ''))}</p>`
                    : '';
                const cierre = indice === textosDirector.length - 1
                    ? `<br><strong>${escaparHtml(resolverTexto(cfgTextos.BIENVENIDA || ''))}</strong>`
                    : '';
                return `${subtitulo}<p>${texto}${cierre}</p>`;
            }).join('');
            director.innerHTML = cuerpoDirector;
        }

        document.querySelectorAll('[data-config-service]').forEach(bloque => {
            const clave = bloque.dataset.configService;
            const servicio = cfgTextos.SERVICIOS?.[clave];
            if (!servicio) return;

            const titulo = bloque.querySelector('[data-config-service-title]');
            const introduccion = bloque.querySelector('[data-config-service-intro]');
            const detalles = bloque.querySelector('[data-config-service-details]');
            if (titulo) titulo.textContent = servicio.TITULO;
            if (introduccion) introduccion.textContent = servicio.INTRO;
            if (detalles) detalles.innerHTML = servicio.DETALLE;
        });

        document.querySelectorAll('[data-config-filter]').forEach(boton => {
            const indice = Number(boton.dataset.configFilter);
            if (cfgTextos.FILTROS_INSTALACIONES?.[indice] !== undefined) boton.textContent = cfgTextos.FILTROS_INSTALACIONES[indice];
        });
    }

    aplicarConfiguracion();
    aplicarContenidosMarca();
    aplicarFormatoInterfaz(window.localStorage.getItem('residencial-ui-style') ?? cfgUI.ESTILO ?? 0);

    // 1. Menú Responsive Móvil
    const mobileMenu = document.getElementById('mobile-menu');
    const navList = document.getElementById('nav-list');

    if (mobileMenu && navList) {
        mobileMenu.addEventListener('click', () => {
            navList.classList.toggle('active');
        });

        navList.querySelectorAll('a').forEach(link => {
            link.addEventListener('click', () => {
                navList.classList.remove('active');
            });
        });
    }

    // Configuración general de Cloudinary
    const cfgCloudinary = (typeof CONFIG !== 'undefined' && CONFIG.CLOUDINARY) ? CONFIG.CLOUDINARY : {};
    const cloudName = cfgCloudinary.CLOUD_NAME;

    function escaparHtml(texto) {
        return String(texto).replace(/[&<>'"]/g, caracter => ({
            '&': '&amp;',
            '<': '&lt;',
            '>': '&gt;',
            "'": '&#39;',
            '"': '&quot;'
        }[caracter]));
    }

    function obtenerTitulo(imagen) {
        const titulo = imagen.context?.custom?.caption;
        if (titulo && titulo.trim()) return titulo.trim();

        const nombre = imagen.public_id.split('/').pop().replace(/[-_]+/g, ' ').trim();
        return nombre ? nombre.charAt(0).toUpperCase() + nombre.slice(1) : (cfgTextos.FALLBACK_GALERIA || 'Galería');
    }

    function obtenerDescripcion(imagen) {
        const descripcion = imagen.context?.custom?.alt;
        return descripcion && descripcion.trim() ? descripcion.trim() : '';
    }

    function obtenerArea(imagen) {
        const area = imagen.context?.custom?.area;
        if (area && area.trim()) {
            return area.trim().toLowerCase();
        }
        return 'all'; 
    }

    function obtenerOrden(imagen) {
        const orden = imagen.context?.custom?.orden;
        return orden && !isNaN(orden) ? parseInt(orden, 10) : 99;
    }

    // Numera (1 a 6, en ciclo) las imágenes visibles de una grilla; los formatos tipo mosaico usan data-pos
    function marcarPosiciones(grilla) {
        if (!grilla) return;
        let n = 0;
        grilla.querySelectorAll('.gallery-item').forEach(item => {
            if (item.style.display === 'none') {
                item.removeAttribute('data-pos');
            } else {
                item.setAttribute('data-pos', String((n % 6) + 1));
                n++;
            }
        });
    }

    // Muestra solo las primeras N imágenes (que coincidan con el filtro) y activa "Ver galería completa" si sobran
    const limiteVisibles = Number(cfgCloudinary.LIMITE_VISIBLES) || 6;

    function actualizarGrilla(grilla, filtro = 'all') {
        if (!grilla) return;
        grilla.dataset.filtro = filtro;
        let coincidentes = 0;
        grilla.querySelectorAll('.gallery-item').forEach(item => {
            const coincide = filtro === 'all' || item.dataset.category === filtro;
            if (coincide) coincidentes++;
            item.style.display = coincide && coincidentes <= limiteVisibles ? '' : 'none';
        });
        marcarPosiciones(grilla);
        const contenedorMas = document.querySelector(`[data-galeria-completa="${grilla.id}"]`)?.closest('.gallery-more');
        if (contenedorMas) contenedorMas.hidden = coincidentes <= limiteVisibles;
    }

    // 2. Cargar Instalaciones Dinámicas desde Cloudinary (con orden y filtros automáticos)
    const instalacionesDinamicas = document.getElementById('instalaciones-dinamicas');
    const contenedorFiltros = document.getElementById('instalaciones-filtros');
    const tagInstalaciones = cfgCloudinary.TAG_INSTALACIONES;

    const nombresAmigables = {
        'exterior': 'Exterior',
        'interior': 'Áreas Comunes',
        'habitacion': 'Habitaciones'
    };

    if (instalacionesDinamicas && cloudName && tagInstalaciones) {
        fetch(`https://res.cloudinary.com/${cloudName}/image/list/${tagInstalaciones}.json`)
            .then(response => {
                if (!response.ok) throw new Error("No se pudo obtener la lista de instalaciones de Cloudinary.");
                return response.json();
            })
            .then(data => {
                const imagenes = data.resources || [];

                if (imagenes.length === 0) {
                    instalacionesDinamicas.innerHTML = '<p style="grid-column: 1 / -1; text-align: center; color: var(--text-muted);">Próximamente más información sobre instalaciones.</p>';
                    return;
                }

                // Ordenar las imágenes según el metadato "orden"
                imagenes.sort((a, b) => obtenerOrden(a) - obtenerOrden(b));

                // Extraer áreas únicas de los metadatos para los filtros dinámicos
                const areasUnicas = [...new Set(imagenes.map(img => obtenerArea(img)).filter(area => area !== 'all'))];

                if (contenedorFiltros) {
                    let htmlBotones = `<button class="filter-btn active" data-filter="all">Todas</button>`;
                    
                    areasUnicas.forEach(area => {
                        const nombreVisible = nombresAmigables[area] || (area.charAt(0).toUpperCase() + area.slice(1));
                        htmlBotones += `<button class="filter-btn" data-filter="${area}">${nombreVisible}</button>`;
                    });
                    
                    contenedorFiltros.innerHTML = htmlBotones;
                }

                const htmlInstalaciones = imagenes.map(img => {
                    const urlImagen = `https://res.cloudinary.com/${cloudName}/image/upload/q_auto,f_auto,w_1200,c_limit/v${img.version}/${img.public_id}.${img.format}`;
                    const titulo = escaparHtml(obtenerTitulo(img));
                    const descripcion = escaparHtml(obtenerDescripcion(img));
                    const categoria = escaparHtml(obtenerArea(img));
                    
                    return `
                        <div class="gallery-item" data-category="${categoria}">
                            <img src="${urlImagen}" alt="${titulo}${descripcion ? `: ${descripcion}` : ''}" loading="lazy" width="1170" height="821">
                            <div class="gallery-overlay">
                                <h3>${titulo}</h3>
                                ${descripcion ? `<p>${descripcion}</p>` : ''}
                            </div>
                        </div>
                    `;
                }).join('');
                
                instalacionesDinamicas.innerHTML = htmlInstalaciones;
                actualizarGrilla(instalacionesDinamicas);
                
                inicializarFiltrosInstalaciones();
                inicializarLightbox();
            })
            .catch(error => {
                console.error('Error cargando las instalaciones desde Cloudinary:', error);
                instalacionesDinamicas.innerHTML = '<p style="grid-column: 1 / -1; text-align: center; color: var(--text-muted);">No se pudieron cargar las instalaciones.</p>';
                inicializarLightbox();
            });
    }

    // Lógica de Filtrado de Instalaciones (delegación de eventos: funciona con botones fijos o generados)
    function inicializarFiltrosInstalaciones() {
        const contenedor = document.getElementById('instalaciones-filtros');
        if (!contenedor || contenedor.dataset.filtrosListos === '1') return;
        contenedor.dataset.filtrosListos = '1';

        contenedor.addEventListener('click', (e) => {
            const btn = e.target.closest('.filter-btn');
            if (!btn || !contenedor.contains(btn)) return;

            contenedor.querySelectorAll('.filter-btn.active').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');

            const filtro = btn.getAttribute('data-filter');
            actualizarGrilla(document.getElementById('instalaciones-dinamicas'), filtro);
        });
    }

    // 3. Cargar Galería Dinámica desde Cloudinary
    const galeriaDinamica = document.getElementById('galeria-dinamica');
    const tagGaleria = cfgCloudinary.TAG_GALERIA;
    
    if (galeriaDinamica && cloudName && tagGaleria) {
        fetch(`https://res.cloudinary.com/${cloudName}/image/list/${tagGaleria}.json`)
            .then(response => {
                if (!response.ok) throw new Error("No se pudo obtener la lista de Cloudinary.");
                return response.json();
            })
            .then(data => {
                const imagenes = data.resources || [];

                if (imagenes.length === 0) {
                    galeriaDinamica.innerHTML = '<p style="grid-column: 1 / -1; text-align: center; color: var(--text-muted);">Próximamente compartiremos más momentos.</p>';
                    return;
                }

                // Las más recientes primero
                imagenes.sort((a, b) => new Date(b.created_at || 0) - new Date(a.created_at || 0));

                const htmlImagenes = imagenes.map(img => {
                    const urlImagen = `https://res.cloudinary.com/${cloudName}/image/upload/q_auto,f_auto,w_1200,c_limit/v${img.version}/${img.public_id}.${img.format}`;
                    const titulo = escaparHtml(obtenerTitulo(img));
                    const descripcion = escaparHtml(obtenerDescripcion(img));
                    
                    return `
                        <div class="gallery-item">
                            <img src="${urlImagen}" alt="${titulo}${descripcion ? `: ${descripcion}` : ''}" loading="lazy" width="1170" height="821">
                            <div class="gallery-overlay">
                                <h3>${titulo}</h3>
                                ${descripcion ? `<p>${descripcion}</p>` : ''}
                            </div>
                        </div>
                    `;
                }).join('');
                
                galeriaDinamica.innerHTML = htmlImagenes;
                actualizarGrilla(galeriaDinamica);
                inicializarLightbox();
            })
            .catch(error => {
                console.error('Error cargando la galería desde Cloudinary:', error);
                galeriaDinamica.innerHTML = '<p style="grid-column: 1 / -1; text-align: center; color: var(--text-muted);">Próximamente compartiremos más momentos.</p>';
                inicializarLightbox();
            });
    } else {
        inicializarLightbox();
    }

    // 4. Lógica general para botones expandibles ("Ver más" / "Ver menos")
    const expandableButtons = document.querySelectorAll('.btn-leer-mas');

    expandableButtons.forEach(button => {
        button.addEventListener('click', () => {
            const targetId = button.getAttribute('data-target');
            const container = document.getElementById(targetId);

            if (container) {
                container.classList.toggle('expanded');

                if (container.classList.contains('expanded')) {
                    button.textContent = cfgTextos.BOTON_MENOS || 'Ver menos';
                } else {
                    button.textContent = cfgTextos.BOTON_MAS || 'Ver más';
                    container.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
                }
            }
        });
    });

    // 5. Lógica del Lightbox (un solo listener delegado: sirve también para las fotos del modal)
    function inicializarLightbox() {
        const lightbox = document.getElementById('lightbox');
        const lightboxImg = document.getElementById('lightbox-img');
        const lightboxClose = document.getElementById('lightbox-close');

        if (!lightbox || !lightboxImg || !lightboxClose || lightbox.dataset.listo === '1') return;
        lightbox.dataset.listo = '1';

        document.addEventListener('click', (e) => {
            const imgElement = e.target.closest('.gallery-item')?.querySelector('img');
            if (imgElement) {
                lightboxImg.setAttribute('src', imgElement.getAttribute('src'));
                lightbox.style.display = 'flex';
            }
        });

        lightboxClose.addEventListener('click', () => {
            lightbox.style.display = 'none';
        });

        lightbox.addEventListener('click', (e) => {
            if (e.target !== lightboxImg) {
                lightbox.style.display = 'none';
            }
        });
    }

    // 5b. Modal con la galería completa
    const modalGaleria = document.getElementById('gallery-modal');
    const modalGrilla = document.getElementById('gallery-modal-grid');

    function cerrarModalGaleria() {
        modalGaleria.hidden = true;
        document.body.style.overflow = '';
    }

    function abrirModalGaleria(idGrilla) {
        const grilla = document.getElementById(idGrilla);
        if (!grilla || !modalGaleria || !modalGrilla) return;

        const filtro = grilla.dataset.filtro || 'all';
        document.getElementById('gallery-modal-title').textContent =
            grilla.closest('section')?.querySelector('.section-title')?.textContent || '';

        modalGrilla.innerHTML = '';
        grilla.querySelectorAll('.gallery-item').forEach(item => {
            if (filtro !== 'all' && item.dataset.category !== filtro) return;
            const copia = item.cloneNode(true);
            copia.style.display = '';
            copia.removeAttribute('data-pos');
            modalGrilla.appendChild(copia);
        });

        modalGaleria.hidden = false;
        modalGaleria.scrollTop = 0;
        document.body.style.overflow = 'hidden';
    }

    document.addEventListener('click', (e) => {
        const boton = e.target.closest('[data-galeria-completa]');
        if (boton) {
            abrirModalGaleria(boton.dataset.galeriaCompleta);
        } else if (modalGaleria && e.target.closest('#gallery-modal-close')) {
            cerrarModalGaleria();
        }
    });

    document.addEventListener('keydown', (e) => {
        if (e.key === 'Escape' && modalGaleria && !modalGaleria.hidden) cerrarModalGaleria();
    });

    // 6. Datos de contacto y redes desde config.js
    const cfgContacto = (typeof CONFIG !== 'undefined' && CONFIG.CONTACTO) ? CONFIG.CONTACTO : {};

    function asignarEnlace(clave, url) {
        if (!url) return;
        document.querySelectorAll(`[data-config="${clave}"]`).forEach(el => { el.href = url; });
    }

    // Enlace normal de WhatsApp (para botones de texto o secciones) — con el mismo mensaje predeterminado
    const mensajeWhatsapp = `Hola, quiero consultar sobre los servicios de ${cfgMarca.NOMBRE_COMPLETO || 'el residencial'}`;
    asignarEnlace('whatsapp', cfgContacto.WHATSAPP_NUMERO && `https://wa.me/${cfgContacto.WHATSAPP_NUMERO}?text=${encodeURIComponent(mensajeWhatsapp)}`);

    // Enlace específico para el Botón Flotante con el mismo mensaje predeterminado
    if (cfgContacto.WHATSAPP_NUMERO) {
        const urlWhatsappFlotante = `https://wa.me/${cfgContacto.WHATSAPP_NUMERO}?text=${encodeURIComponent(mensajeWhatsapp)}`;
        asignarEnlace('whatsapp-flotante', urlWhatsappFlotante);
    }

    asignarEnlace('instagram', cfgContacto.INSTAGRAM_URL);
    asignarEnlace('facebook', cfgContacto.FACEBOOK_URL);

    if (cfgContacto.DIRECCION) {
        document.querySelectorAll('[data-config="direccion"]').forEach(el => { el.textContent = cfgContacto.DIRECCION; });
        asignarEnlace('mapa', `https://maps.google.com/?q=${encodeURIComponent(cfgContacto.DIRECCION)}`);
    }
});
```

### Archivo: `mantenimiento-config.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#173c35">
  <title>Mantenimiento de configuración</title>
  <style>
    :root {
      color-scheme: light;
      --ink: #202a27;
      --muted: #64716d;
      --line: #d9e1dc;
      --paper: #f4f7f4;
      --surface: #fff;
      --green: #173c35;
      --green-soft: #e7f0eb;
      --lime: #c9df80;
      --coral: #bd543d;
      --focus: #267c69;
      font-family: "Segoe UI", Tahoma, sans-serif;
      color: var(--ink);
      background: var(--paper);
    }

    * { box-sizing: border-box; }

    body {
      min-width: 320px;
      min-height: 100vh;
      margin: 0;
      background:
        linear-gradient(115deg, transparent 0 74%, rgb(201 223 128 / 12%) 74% 100%),
        var(--paper);
    }

    button, input, textarea { font: inherit; }
    button { cursor: pointer; }

    .masthead {
      color: #f7faf7;
      background: var(--green);
      border-bottom: 5px solid var(--lime);
    }

    .masthead-inner, main {
      width: min(1120px, calc(100% - 40px));
      margin: 0 auto;
    }

    .masthead-inner {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 24px;
      min-height: 116px;
      padding: 22px 0;
    }

    .eyebrow {
      margin: 0 0 7px;
      color: var(--lime);
      font-size: 11px;
      font-weight: 700;
      text-transform: uppercase;
    }

    h1 {
      margin: 0;
      font-family: Georgia, "Times New Roman", serif;
      font-size: 34px;
      font-weight: 400;
      line-height: 1.1;
    }

    .file-state {
      max-width: 320px;
      color: #d1ddd7;
      font-size: 13px;
      line-height: 1.5;
      text-align: right;
      overflow-wrap: anywhere;
    }

    main { padding: 24px 0 64px; }

    .toolbar {
      position: sticky;
      z-index: 2;
      top: 0;
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 9px;
      margin: 0 0 18px;
      padding: 12px;
      border: 1px solid var(--line);
      border-radius: 6px;
      background: rgb(255 255 255 / 96%);
      box-shadow: 0 5px 18px rgb(32 42 39 / 6%);
      backdrop-filter: blur(10px);
    }

    .toolbar-spacer { flex: 1; }

    .button {
      min-height: 40px;
      padding: 0 15px;
      border: 1px solid var(--line);
      border-radius: 4px;
      color: var(--ink);
      background: var(--surface);
      font-size: 13px;
      font-weight: 650;
      transition: background-color .16s ease, border-color .16s ease, transform .16s ease;
    }

    .button:hover { border-color: #8ba79b; background: #f4f8f5; }
    .button:active { transform: translateY(1px); }
    .button:focus-visible, input:focus-visible, textarea:focus-visible, summary:focus-visible {
      outline: 3px solid rgb(38 124 105 / 28%);
      outline-offset: 2px;
    }

    .button-primary {
      border-color: var(--green);
      color: #fff;
      background: var(--green);
    }

    .button-primary:hover { border-color: #28594f; color: #fff; background: #28594f; }
    .button:disabled { cursor: not-allowed; opacity: .45; }

    .empty-state {
      display: grid;
      min-height: 260px;
      place-items: center;
      padding: 32px;
      border: 1px dashed #aabbb1;
      border-radius: 6px;
      color: var(--muted);
      background: rgb(255 255 255 / 60%);
      text-align: center;
    }

    .empty-state strong {
      display: block;
      margin-bottom: 7px;
      color: var(--green);
      font-family: Georgia, "Times New Roman", serif;
      font-size: 23px;
      font-weight: 400;
    }

    .sections { display: grid; gap: 10px; }

    details.section {
      border: 1px solid var(--line);
      border-radius: 5px;
      background: var(--surface);
      animation: arrive .3s ease both;
    }

    details.section:nth-child(2) { animation-delay: 25ms; }
    details.section:nth-child(3) { animation-delay: 50ms; }
    details.section:nth-child(4) { animation-delay: 75ms; }
    details.section:nth-child(5) { animation-delay: 100ms; }
    details.section:nth-child(6) { animation-delay: 125ms; }

    details.section > summary {
      display: flex;
      align-items: center;
      gap: 12px;
      min-height: 58px;
      padding: 13px 18px;
      color: var(--green);
      font-size: 16px;
      font-weight: 700;
      cursor: pointer;
      list-style: none;
    }

    details.section > summary::-webkit-details-marker, .nested > summary::-webkit-details-marker { display: none; }
    details.section > summary::before, .nested > summary::before {
      width: 8px;
      height: 8px;
      border-right: 2px solid currentColor;
      border-bottom: 2px solid currentColor;
      content: "";
      transform: rotate(-45deg);
      transition: transform .15s ease;
    }

    details[open] > summary::before { transform: rotate(45deg); }

    .section-count {
      margin-left: auto;
      color: var(--muted);
      font-size: 11px;
      font-weight: 500;
    }

    .section-body { padding: 0 18px 20px; }
    .fields { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 16px 20px; }

    .field { min-width: 0; }
    .field-wide { grid-column: 1 / -1; }

    .field-label, .nested-label {
      display: block;
      margin: 0 0 6px;
      color: #394943;
      font-size: 12px;
      font-weight: 700;
    }

    .field-key {
      margin-left: 5px;
      color: #84918b;
      font-size: 10px;
      font-weight: 500;
    }

    .field input[type="text"], .field input[type="url"], .field input[type="number"], .field textarea {
      display: block;
      width: 100%;
      min-height: 41px;
      padding: 9px 11px;
      border: 1px solid #cbd6cf;
      border-radius: 4px;
      color: var(--ink);
      background: #fff;
      line-height: 1.5;
    }

    .field textarea { min-height: 92px; resize: vertical; }
    .field textarea.code-value { min-height: 150px; font-family: Consolas, "Courier New", monospace; font-size: 12px; }
    .field input[type="color"] { width: 46px; height: 41px; padding: 4px; border: 1px solid #cbd6cf; border-radius: 4px; background: #fff; }
    .color-controls { display: flex; gap: 8px; }
    .color-controls input[type="text"] { flex: 1; min-width: 0; }

    .boolean-control { display: inline-flex; align-items: center; gap: 9px; min-height: 41px; color: var(--ink); font-size: 13px; }
    .boolean-control input { width: 18px; height: 18px; accent-color: var(--green); }

    .nested-list { display: grid; gap: 10px; }

    details.nested {
      min-width: 0;
      border: 1px solid #e1e8e3;
      border-radius: 4px;
      background: #fbfcfb;
    }

    .nested > summary {
      display: flex;
      align-items: center;
      gap: 9px;
      min-height: 42px;
      padding: 9px 12px;
      color: #30483f;
      font-size: 13px;
      font-weight: 650;
      cursor: pointer;
      list-style: none;
    }

    .nested > summary::before { width: 6px; height: 6px; }
    .nested-content { padding: 0 12px 14px; }
    .array-tools { display: flex; justify-content: flex-end; margin-top: 10px; }
    .button-small { min-height: 32px; padding: 0 11px; font-size: 12px; }

    .remove-button {
      flex: 0 0 auto;
      width: 30px;
      height: 30px;
      border: 1px solid #e7c7bf;
      border-radius: 4px;
      color: var(--coral);
      background: #fff;
      font-size: 17px;
      line-height: 1;
    }

    .remove-button:hover { background: #fff2ef; }

    .array-item {
      display: flex;
      align-items: flex-start;
      gap: 9px;
      padding: 10px;
      border: 1px solid #e1e8e3;
      border-radius: 4px;
      background: #fbfcfb;
    }

    .array-item-content { flex: 1; min-width: 0; }
    .array-item-content > .field { margin: 0; }
    .muted-note { margin: 8px 0 0; color: var(--muted); font-size: 11px; line-height: 1.5; }

    @keyframes arrive {
      from { opacity: 0; transform: translateY(5px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @media (max-width: 680px) {
      .masthead-inner, main { width: min(100% - 28px, 1120px); }
      .masthead-inner { align-items: flex-start; flex-direction: column; gap: 10px; padding: 22px 0 18px; }
      .file-state { max-width: 100%; text-align: left; }
      h1 { font-size: 28px; }
      .toolbar { top: 0; gap: 7px; padding: 9px; }
      .button { min-height: 38px; padding: 0 11px; font-size: 12px; }
      .toolbar-spacer { display: none; }
      .fields { grid-template-columns: minmax(0, 1fr); }
      .field-wide { grid-column: auto; }
      details.section > summary { padding-inline: 13px; }
      .section-body { padding-inline: 13px; }
      .array-item { gap: 6px; padding: 8px; }
    }

    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after { scroll-behavior: auto !important; animation-duration: .01ms !important; transition-duration: .01ms !important; }
    }
  </style>
</head>
<body>
  <header class="masthead">
    <div class="masthead-inner">
      <div>
        <p class="eyebrow">Residencial Blanco / herramienta local</p>
        <h1>Mantenimiento de configuración</h1>
      </div>
      <div class="file-state" id="fileState" role="status" aria-live="polite">Abre `js/config.js` para comenzar.</div>
    </div>
  </header>

  <main>
    <div class="toolbar" aria-label="Acciones de archivo">
      <button class="button" id="openButton" type="button">Abrir config.js</button>
      <button class="button button-primary" id="downloadButton" type="button" disabled>Descargar copia</button>
      <span class="toolbar-spacer"></span>
      <input id="fileInput" type="file" accept=".js,.txt,text/javascript" hidden>
    </div>

    <div id="emptyState" class="empty-state">
      <div><strong>Configuración sin cargar</strong>Abre el archivo de configuración para editarlo.</div>
    </div>
    <div id="sections" class="sections" hidden></div>
  </main>

  <script>
    "use strict";

    const SECTION_NAMES = {
      MARCA: "Identidad y recursos",
      TEMA: "Apariencia",
      UI: "Formato de interfaz",
      TEXTOS: "Textos del sitio",
      CLOUDINARY: "Galería y Cloudinary",
      CONTACTO: "Contacto y redes"
    };

    const FIELD_NAMES = {
      NOMBRE: "Nombre de la marca", NOMBRE_COMPLETO: "Nombre completo", TITULO: "Título del sitio",
      DESCRIPCION: "Descripción", LOGO: "Logo (URL)", IMAGEN_DIRECTOR: "Imagen del director (URL)",
      IMAGEN_HERO: "Imagen principal (URL)", ANO: "Año", PRIMARIO: "Color primario",
      PRIMARIO_OSCURO: "Color primario oscuro", ACENTO: "Color de acento", FONDO: "Fondo",
      TEXTO: "Texto", FUENTE_TITULOS: "Fuente de títulos", FUENTE_TEXTO: "Fuente de texto",
      ESTILO: "Formato activo", MOSTRAR_SELECTOR: "Mostrar selector de formatos", OPCIONES: "Formatos disponibles",
      LEMA: "Lema", HERO_DESCRIPCION: "Descripción principal", CTA_CONTACTO: "Botón de contacto",
      TITULO_NOSOTROS: "Título: nosotros", TITULO_VALORES: "Título: valores", TITULO_SERVICIOS: "Título: servicios",
      TITULO_INSTALACIONES: "Título: instalaciones", TITULO_GALERIA: "Título: galería", MENSAJE_GALERIA: "Mensaje de galería",
      TITULO_CONTACTO: "Título: contacto", DESCRIPCION_CONTACTO: "Descripción de contacto", BOTON_MAS: "Botón: ver más",
      BOTON_MENOS: "Botón: ver menos", BOTON_VER_GALERIA: "Botón: galería completa", DIRECTOR_ALT: "Texto alternativo del director",
      VALOR_HOGAR: "Valor: hogar", NAVEGACION: "Navegación", FILTROS_INSTALACIONES: "Filtros de instalaciones",
      TITULO_DIRECTOR: "Título del director", NOMBRE_DIRECTOR: "Nombre del director", CARGO_DIRECTOR: "Cargo del director",
      COMPROMISO_DIRECTOR: "Compromiso del director", POR_QUE: "¿Por qué elegirnos?", BIENVENIDA: "Bienvenida",
      TITULO_MISION: "Título: misión", TITULO_VISION: "Título: visión", TEXTO_DIRECTOR: "Texto del director",
      MISION: "Misión", VISION: "Visión", INTRO_INSTALACIONES: "Introducción: instalaciones",
      INTRO_ALIMENTACION: "Introducción: alimentación", INTRO_TALLERES: "Introducción: talleres",
      INTRO_ENFERMERIA: "Introducción: equipo", VALORES: "Valores", SERVICIOS: "Servicios",
      INSTALACIONES: "Instalaciones", ALIMENTACION: "Alimentación", TALLERES: "Talleres",
      ENFERMERIA: "Equipo y gestión", DETALLE: "Detalle (HTML)", INTRO: "Introducción", ARCHIVO: "Archivo CSS",
      ID: "Identificador", CLOUD_NAME: "Cloud name", UPLOAD_PRESET: "Preset de subida", CARPETA_BASE: "Carpeta base",
      CARPETA_DEFAULT: "Carpeta predeterminada", TAG_GALERIA: "Etiqueta de galería", LIMITE_VISIBLES: "Imágenes visibles",
      TAG_INSTALACIONES: "Etiqueta de instalaciones", DIRECCION: "Dirección", WHATSAPP_NUMERO: "Número de WhatsApp",
      INSTAGRAM_URL: "Instagram (URL)", FACEBOOK_URL: "Facebook (URL)"
    };

    const fileState = document.getElementById("fileState");
    const fileInput = document.getElementById("fileInput");
    const openButton = document.getElementById("openButton");
    const downloadButton = document.getElementById("downloadButton");
    const emptyState = document.getElementById("emptyState");
    const sections = document.getElementById("sections");
    let config = null;
    let dirty = false;
    let sourceName = "config.js";

    function tokenize(source) {
      const tokens = [];
      let position = 0;
      while (position < source.length) {
        const character = source[position];
        if (/\s/.test(character)) { position += 1; continue; }
        if (source.startsWith("//", position)) {
          position = source.indexOf("\n", position);
          if (position === -1) break;
          continue;
        }
        if (source.startsWith("/*", position)) {
          const end = source.indexOf("*/", position + 2);
          if (end === -1) throw new Error("Hay un comentario sin cerrar.");
          position = end + 2;
          continue;
        }
        if (character === "'" || character === '"') {
          const quote = character;
          let value = "";
          position += 1;
          let closed = false;
          while (position < source.length) {
            const next = source[position++];
            if (next === quote) { closed = true; break; }
            if (next !== "\\") { value += next; continue; }
            if (position >= source.length) throw new Error("Hay una cadena incompleta.");
            const escaped = source[position++];
            const escapes = { n: "\n", r: "\r", t: "\t", b: "\b", f: "\f", v: "\v", "0": "\0" };
            if (Object.hasOwn(escapes, escaped)) value += escapes[escaped];
            else if (escaped === "\n") continue;
            else if (escaped === "\r") { if (source[position] === "\n") position += 1; }
            else if (escaped === "x" || escaped === "u") {
              const digits = escaped === "x" ? 2 : 4;
              const code = source.slice(position, position + digits);
              if (!new RegExp(`^[0-9a-fA-F]{${digits}}$`).test(code)) throw new Error("Hay una secuencia de escape inválida.");
              value += String.fromCharCode(parseInt(code, 16));
              position += digits;
            } else value += escaped;
          }
          if (!closed) throw new Error("Hay una cadena sin cerrar.");
          tokens.push({ type: "string", value });
          continue;
        }
        const numberMatch = source.slice(position).match(/^-?(?:0|[1-9]\d*)(?:\.\d+)?(?:[eE][+-]?\d+)?/);
        if (numberMatch) {
          tokens.push({ type: "number", value: Number(numberMatch[0]) });
          position += numberMatch[0].length;
          continue;
        }
        const identifierMatch = source.slice(position).match(/^[A-Za-z_$][\w$]*/);
        if (identifierMatch) {
          tokens.push({ type: "identifier", value: identifierMatch[0] });
          position += identifierMatch[0].length;
          continue;
        }
        if ("{}[]:,;=".includes(character)) {
          tokens.push({ type: "punctuation", value: character });
          position += 1;
          continue;
        }
        throw new Error(`No se reconoce el carácter “${character}” cerca de la posición ${position + 1}.`);
      }
      return tokens;
    }

    function parseConfig(source) {
      const tokens = tokenize(source);
      let cursor = -1;
      for (let index = 0; index < tokens.length - 1; index += 1) {
        if (tokens[index].type === "identifier" && tokens[index].value === "CONFIG" && tokens[index + 1].value === "=") {
          cursor = index + 2;
          break;
        }
      }
      if (cursor === -1) throw new Error("No se encontró una asignación `CONFIG = { ... }`.");

      function peek(value) { return tokens[cursor]?.value === value; }
      function take(value) {
        const token = tokens[cursor];
        if (!token || (value && token.value !== value)) throw new Error(`Se esperaba “${value || "un valor"}” en la configuración.`);
        cursor += 1;
        return token;
      }

      function parseValue() {
        const token = tokens[cursor];
        if (!token) throw new Error("La configuración está incompleta.");
        if (token.value === "{") {
          take("{");
          const object = {};
          while (!peek("}")) {
            const key = take().value;
            if (typeof key !== "string" && typeof key !== "number") throw new Error("Hay una clave de objeto inválida.");
            take(":");
            object[key] = parseValue();
            if (peek(",")) take(",");
            else if (!peek("}")) throw new Error("Falta una coma entre campos.");
          }
          take("}");
          return object;
        }
        if (token.value === "[") {
          take("[");
          const array = [];
          while (!peek("]")) {
            array.push(parseValue());
            if (peek(",")) take(",");
            else if (!peek("]")) throw new Error("Falta una coma entre elementos.");
          }
          take("]");
          return array;
        }
        take();
        if (token.type === "string" || token.type === "number") return token.value;
        if (token.type === "identifier" && token.value === "true") return true;
        if (token.type === "identifier" && token.value === "false") return false;
        if (token.type === "identifier" && token.value === "null") return null;
        throw new Error(`El valor “${token.value}” no es compatible con el editor.`);
      }

      const parsed = parseValue();
      if (!parsed || Array.isArray(parsed) || typeof parsed !== "object") throw new Error("CONFIG debe contener un objeto.");
      return parsed;
    }

    function labelFor(key) {
      return FIELD_NAMES[key] || key.replaceAll("_", " ").toLowerCase().replace(/(^|\s)\S/g, letter => letter.toUpperCase());
    }

    function valueAt(path) {
      return path.reduce((value, key) => value[key], config);
    }

    function setValue(path, value) {
      const parent = path.slice(0, -1).reduce((node, key) => node[key], config);
      parent[path[path.length - 1]] = value;
      markDirty();
    }

    function markDirty() {
      dirty = true;
      updateStatus(`${sourceName} · cambios sin guardar`);
    }

    function updateStatus(message) {
      fileState.textContent = message;
    }

    function makeField(key, value, path) {
      const wrapper = document.createElement("div");
      const longText = typeof value === "string" && (value.length > 115 || /DESCRIP|DETALLE|MISION|VISION|INTRO|COMPROMISO|TEXTO|LEMA|MENSAJE|VALOR_/i.test(key));
      const isHtml = key === "DETALLE";
      wrapper.className = `field${longText ? " field-wide" : ""}`;

      if (typeof value === "boolean") {
        const label = document.createElement("label");
        label.className = "boolean-control";
        const input = document.createElement("input");
        input.type = "checkbox";
        input.checked = value;
        input.addEventListener("change", () => setValue(path, input.checked));
        label.append(input, document.createTextNode(labelFor(key)));
        wrapper.append(label);
        return wrapper;
      }

      const label = document.createElement("label");
      label.className = "field-label";
      label.append(document.createTextNode(labelFor(key)));
      const keyHint = document.createElement("span");
      keyHint.className = "field-key";
      keyHint.textContent = key;
      label.append(keyHint);

      if (typeof value === "string" && /^(?:#(?:[0-9a-f]{3}){1,2})$/i.test(value)) {
        const controls = document.createElement("div");
        controls.className = "color-controls";
        const color = document.createElement("input");
        color.type = "color";
        color.value = value;
        color.setAttribute("aria-label", `${labelFor(key)} selector de color`);
        const text = document.createElement("input");
        text.type = "text";
        text.value = value;
        text.spellcheck = false;
        text.setAttribute("aria-label", `${labelFor(key)} valor hexadecimal`);
        color.addEventListener("input", () => { text.value = color.value; setValue(path, color.value); });
        text.addEventListener("input", () => {
          if (/^(?:#(?:[0-9a-f]{3}){1,2})$/i.test(text.value)) color.value = text.value;
          setValue(path, text.value);
        });
        controls.append(color, text);
        wrapper.append(label, controls);
        return wrapper;
      }

      let input;
      if (longText) {
        input = document.createElement("textarea");
        if (isHtml) input.classList.add("code-value");
      } else {
        input = document.createElement("input");
        input.type = typeof value === "number" ? "number" : /URL|LOGO|IMAGEN/i.test(key) ? "url" : "text";
        if (typeof value === "number") input.step = "any";
      }
      input.value = value ?? "";
      input.addEventListener("input", () => setValue(path, input.type === "number" ? Number(input.value) : input.value));
      wrapper.append(label, input);
      if (isHtml) {
        const note = document.createElement("p");
        note.className = "muted-note";
        note.textContent = "Admite etiquetas HTML, por ejemplo <li> y <strong>.";
        wrapper.append(note);
      }
      return wrapper;
    }

    function emptyItemFor(key, array) {
      if (key === "VALORES") return ["", ""];
      if (key === "OPCIONES") {
        const ids = array.filter(item => item && typeof item.ID === "number").map(item => item.ID);
        return { ID: ids.length ? Math.max(...ids) + 1 : 0, NOMBRE: "", ARCHIVO: "" };
      }
      const example = array[0];
      if (typeof example === "string") return "";
      if (typeof example === "number") return 0;
      if (typeof example === "boolean") return false;
      if (Array.isArray(example)) return example.map(() => "");
      if (example && typeof example === "object") {
        return Object.fromEntries(Object.entries(example).map(([field, fieldValue]) => [field, field === "ID" && typeof fieldValue === "number" ? Math.max(-1, ...array.map(item => Number(item?.ID) || 0)) + 1 : typeof fieldValue === "boolean" ? false : typeof fieldValue === "number" ? 0 : ""]));
      }
      return "";
    }

    function itemHeading(item, index) {
      if (Array.isArray(item) && item[0]) return String(item[0]);
      if (item && typeof item === "object") return item.NOMBRE || item.TITULO || item.ID || `Elemento ${index + 1}`;
      return typeof item === "string" && item ? item : `Elemento ${index + 1}`;
    }

    function makeArray(key, array, path) {
      const block = document.createElement("div");
      block.className = "field-wide";
      const heading = document.createElement("div");
      heading.className = "nested-label";
      heading.textContent = labelFor(key);
      const list = document.createElement("div");
      list.className = "nested-list";

      array.forEach((item, index) => {
        const itemPath = [...path, index];
        const row = document.createElement("div");
        row.className = "array-item";
        const content = document.createElement("div");
        content.className = "array-item-content";
        if (item && typeof item === "object" && !Array.isArray(item)) {
          const nested = document.createElement("details");
          nested.className = "nested";
          nested.open = true;
          const summary = document.createElement("summary");
          summary.textContent = itemHeading(item, index);
          nested.append(summary, renderObject(item, itemPath));
          content.append(nested);
        } else if (Array.isArray(item)) {
          const innerFields = document.createElement("div");
          innerFields.className = "fields";
          item.forEach((inner, innerIndex) => innerFields.append(makeField(innerIndex === 0 ? "Nombre" : "Descripción", inner, [...itemPath, innerIndex])));
          content.append(innerFields);
        } else {
          const field = makeField(`${key} ${index + 1}`, item, itemPath);
          const label = field.querySelector(".field-label");
          if (label) label.firstChild.textContent = `${labelFor(key)} ${index + 1}`;
          content.append(field);
        }
        const remove = document.createElement("button");
        remove.type = "button";
        remove.className = "remove-button";
        remove.textContent = "×";
        remove.title = `Eliminar ${itemHeading(item, index)}`;
        remove.setAttribute("aria-label", remove.title);
        remove.addEventListener("click", () => {
          const target = valueAt(path);
          target.splice(index, 1);
          markDirty();
          render();
        });
        row.append(content, remove);
        list.append(row);
      });

      const tools = document.createElement("div");
      tools.className = "array-tools";
      const add = document.createElement("button");
      add.type = "button";
      add.className = "button button-small";
      add.textContent = "+ Agregar elemento";
      add.addEventListener("click", () => {
        valueAt(path).push(emptyItemFor(key, valueAt(path)));
        markDirty();
        render();
        const openSections = [...sections.querySelectorAll("details")].filter(details => details.open);
        openSections.forEach(details => { details.open = true; });
      });
      tools.append(add);
      block.append(heading, list, tools);
      return block;
    }

    function renderObject(object, path) {
      const body = document.createElement("div");
      body.className = "section-body";
      const fields = document.createElement("div");
      fields.className = "fields";
      for (const [key, value] of Object.entries(object)) {
        const currentPath = [...path, key];
        if (Array.isArray(value)) {
          fields.append(makeArray(key, value, currentPath));
        } else if (value && typeof value === "object") {
          const nested = document.createElement("details");
          nested.className = "nested field-wide";
          const summary = document.createElement("summary");
          summary.textContent = labelFor(key);
          nested.append(summary, renderObject(value, currentPath));
          fields.append(nested);
        } else {
          fields.append(makeField(key, value, currentPath));
        }
      }
      body.append(fields);
      return body;
    }

    function render() {
      sections.replaceChildren();
      for (const [key, value] of Object.entries(config)) {
        if (!value || typeof value !== "object" || Array.isArray(value)) continue;
        const section = document.createElement("details");
        section.className = "section";
        section.open = true;
        const summary = document.createElement("summary");
        summary.append(document.createTextNode(SECTION_NAMES[key] || labelFor(key)));
        const count = document.createElement("span");
        count.className = "section-count";
        count.textContent = `${Object.keys(value).length} campos`;
        summary.append(count);
        section.append(summary, renderObject(value, [key]));
        sections.append(section);
      }
      emptyState.hidden = true;
      sections.hidden = false;
      downloadButton.disabled = false;
    }

    function loadSource(source, name) {
      const parsed = parseConfig(source);
      if (!parsed.MARCA || !parsed.TEXTOS) throw new Error("El archivo no parece ser el config.js de este sitio.");
      config = parsed;
      sourceName = name || "config.js";
      dirty = false;
      render();
      updateStatus(`${sourceName} · listo para editar`);
    }

    function serializeConfig() {
      const output = ["// Configuración de marca blanca.", "// Editar este archivo para personalizar la plantilla.", "const CONFIG = {"];
      const entries = Object.entries(config);
      entries.forEach(([key, value], index) => {
        output.push(`  ${JSON.stringify(key)}: ${JSON.stringify(value, null, 2).split("\n").map((line, lineIndex) => lineIndex === 0 ? line : `  ${line}`).join("\n")}${index < entries.length - 1 ? "," : ""}`);
      });
      output.push("};", "");
      return output.join("\n");
    }

    function downloadConfig() {
      const blob = new Blob([serializeConfig()], { type: "text/javascript;charset=utf-8" });
      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");
      link.href = url;
      link.download = "config.js";
      link.click();
      URL.revokeObjectURL(url);
      dirty = false;
      updateStatus("config.js · copia descargada");
    }

    openButton.addEventListener("click", () => fileInput.click());
    fileInput.addEventListener("change", async () => {
      const file = fileInput.files?.[0];
      if (!file) return;
      if (dirty && !window.confirm("Hay cambios sin guardar. ¿Descartar y abrir otro archivo?")) {
        fileInput.value = "";
        return;
      }
      try {
        loadSource(await file.text(), file.name);
      } catch (error) {
        updateStatus(`No se pudo abrir el archivo: ${error.message}`);
      }
      fileInput.value = "";
    });
    downloadButton.addEventListener("click", downloadConfig);
    window.addEventListener("beforeunload", event => {
      if (dirty) { event.preventDefault(); event.returnValue = ""; }
    });

    if (location.protocol !== "file:") {
      fetch("./js/config.js").then(response => {
        if (!response.ok) throw new Error("No se encontró js/config.js");
        return response.text();
      }).then(source => loadSource(source, "js/config.js")).catch(() => {});
    }
  </script>
</body>
</html>
```

### Archivo: `netlify.toml`
```toml
[build]
  publish = "."

  [build.environment]
  NETLIFY_BUILD_DISABLE_BADGE = "true"
```

### Archivo: `readme.md`
```md
# Plantilla de marca blanca

Sitio web estático adaptable para una institución o residencial. La identidad, los textos, los recursos visuales, el contacto y Cloudinary se administran desde un único archivo de configuración.

## Documentación

El manual de usuario está en [documentacion-central](https://github.com/FABIOR1981/documentacion-central/tree/main/Residencial_Blanco/documentacion) ([PDF](https://github.com/FABIOR1981/documentacion-central/blob/main/Residencial_Blanco/documentacion/MANUAL_USUARIO.pdf)). También se puede consultar desde la bitácora de proyectos.

## Personalización

Editar [`js/config.js`](js/config.js). No es necesario modificar `index.html`, `js/main.js` ni `css/styles.css` para cambiar la marca.

### Identidad y apariencia

En `MARCA` se configuran el nombre, título, descripción, logo, imagen del director y portada principal. `IMAGEN_HERO` usa una URL directa de Cloudinary porque es una imagen única de fondo.

En `TEMA` se configuran colores y familias tipográficas.

### Contenido

En `TEXTOS` se configuran:

- Navegación y filtros.
- Lema, textos del hero y títulos de secciones.
- Biografía y datos del director.
- Misión, visión y valores.
- Títulos, introducciones y detalles de servicios.
- Textos de galería, contacto y botones.

### Contacto

En `CONTACTO` se configuran dirección, teléfono de WhatsApp, Instagram y Facebook.

## Cloudinary

La configuración se encuentra en `CONFIG.CLOUDINARY`:

```javascript
CLOUD_NAME: 'p0qlmlor',
UPLOAD_PRESET: 'subir_gestor',
CARPETA_BASE: 'en_Blanco/Residencial',
CARPETA_DEFAULT: 'galeria',
TAG_GALERIA: 'enBlanco_residencial_galeria',
TAG_INSTALACIONES: 'enBlanco_residencial_instalaciones'
```

El sitio utiliza estas etiquetas para consultar las listas públicas de Cloudinary:

- `TAG_GALERIA`: imágenes de la galería de actividades.
- `TAG_INSTALACIONES`: imágenes de instalaciones y filtros por área.
Las imágenes deben estar etiquetadas exactamente con esos valores. Cloudinary debe tener disponible el endpoint público `image/list` para que las consultas funcionen.

La fachada del hero no se busca por tag: se carga directamente mediante `MARCA.IMAGEN_HERO`.
El logo y la imagen del director también se cargan mediante URLs directas configuradas en `MARCA.LOGO` y `MARCA.IMAGEN_DIRECTOR`.

## Estructura

```text
.
├── index.html       # Estructura de la página
├── js/config.js     # Único archivo de personalización
├── js/main.js       # Configuración dinámica, Cloudinary e interacciones
├── css/styles.css   # Base visual y estilo 0 - Original
├── css/formatos/ # _comun.css + un archivo por formato (f1 a f5), cargados según el formato activo
├── img/             # Recursos locales auxiliares
├── netlify.toml     # Configuración opcional de Netlify
└── readme.md
```

## Ejecución local

No requiere compilación. Conviene utilizar un servidor HTTP local para que funcionen correctamente las consultas a Cloudinary.

Con Node.js:

```bash
npx serve .
```

O con Python:

```bash
python -m http.server 8000
```

Luego abrir `http://localhost:8000` o la dirección indicada por el servidor.

## Tecnologías

- HTML5, CSS3 y JavaScript vanilla.
- Cloudinary para imágenes y galerías dinámicas.
- Google Fonts mediante CDN.
- Font Awesome mediante CDN.
- Netlify como opción de publicación.

## Publicación

El proyecto puede publicarse en Netlify o en cualquier servidor de archivos estáticos. Antes de publicar, verificar los valores de `MARCA`, `CONTACTO` y `CLOUDINARY` en `js/config.js`, además de que las etiquetas de Cloudinary sean públicas y coincidan exactamente.
```

