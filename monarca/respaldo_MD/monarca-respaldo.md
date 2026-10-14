# Bundle del Proyecto para IA: monarca

## Estructura de Archivos
- README.md
- css/styles.css
- documentacion/LEEME.md
- img/Logo_verdeOliva_transparente.webp
- img/director.webp
- index.html
- js/config.js
- js/main.js
- netlify.toml

---

## Código Fuente

### Archivo: `README.md`
```md
# Residencial Monarca

Sitio web estático de Residencial Monarca, un residencial geriátrico que presenta su propuesta de cuidado, servicios, instalaciones y formas de contacto para residentes y sus familias.

## Funcionalidades

- Página de inicio responsive con navegación por secciones.
- Presentación institucional, misión, visión y valores.
- Perfil de dirección con contenido expandible ("Ver más" / "Ver menos").
- Sección de servicios: instalaciones, alimentación, talleres y equipo de enfermería.
- Instalaciones dinámicas cargadas desde Cloudinary, ordenadas y con filtros por área generados automáticamente.
- Galería dinámica de actividades cargada desde Cloudinary.
- Visor ampliado de imágenes (lightbox).
- Enlaces de contacto, ubicación y redes sociales, configurables desde un único archivo.

## Estructura del proyecto

```text
.
├── index.html            # Página principal
├── css/styles.css        # Estilos del sitio
├── js/config.js          # Datos configurables: Cloudinary, contacto y redes
├── js/main.js            # Menú, galería dinámica, filtros y lightbox
├── img/                  # Logo e imagen del perfil de dirección
├── documentacion/        # Documentación interna (no se publica en Netlify)
├── netlify.toml          # Configuración de publicación en Netlify
├── .netlifyignore        # Archivos excluidos de la publicación
└── README.md
```

## Configuración (`js/config.js`)

Es el único lugar donde se cambian los datos del sitio. Se carga antes de `main.js`.

- `CLOUDINARY.CLOUD_NAME`: cuenta de Cloudinary desde donde se leen las imágenes.
- `CLOUDINARY.TAG_GALERIA`: etiqueta de las fotos de la galería de actividades.
- `CLOUDINARY.TAG_INSTALACIONES`: etiqueta de las fotos de instalaciones.
- `CONTACTO`: dirección, número de WhatsApp, Instagram y Facebook.

Este archivo es público (lo descarga el navegador). No guardes en él claves secretas ni presets de subida.

## Actualizar las imágenes

Las imágenes se leen desde Cloudinary por etiqueta, sin tocar el código del sitio:

1. Sube la imagen a Cloudinary con la etiqueta correspondiente (`TAG_GALERIA` o `TAG_INSTALACIONES`).
2. Para las instalaciones, opcionalmente completa estos metadatos de contexto: `caption` (título), `alt` (descripción), `area` (categoría del filtro) y `orden` (número de posición).
3. Recarga el sitio: la imagen aparece automáticamente.

La lectura por etiqueta requiere que Cloudinary tenga habilitada la lista de recursos por etiqueta (opción de seguridad "Resource list").

## Ejecución local

El sitio no necesita proceso de compilación. Conviene usar un servidor HTTP local en lugar de abrir `index.html` directamente.

Con Node.js:

```bash
npx serve .
```

O con Python:

```bash
python -m http.server 8000
```

Después, abre la URL indicada por el servidor, normalmente `http://localhost:8000`.

## Tecnologías

- HTML5, CSS3 y JavaScript vanilla.
- Cloudinary para el alojamiento y la entrega de imágenes.
- Google Fonts: Playfair Display y Plus Jakarta Sans.
- Font Awesome 6.4.0 mediante CDN.
- Netlify para el despliegue estático.

## Publicación

El proyecto se publica en Netlify sin paso de compilación (`publish = "."`). No requiere backend ni base de datos. Antes de publicar, conviene verificar que los enlaces de WhatsApp, redes sociales y ubicación correspondan a los datos definitivos de la residencia.

```

### Archivo: `css/styles.css`
```css
/* Fuentes */
/* Fuentes: se cargan una sola vez desde index.html (antes también se importaban acá, duplicado). */

/* ============================================================
   VARIABLES DE ESTÉTICA (Verde Oliva y Tono Madera)
   ============================================================ */
:root {
    --primary: #4A5D4E;
    --primary-dark: #374439;
    --accent: #D4A373;
    --bg-light: #F9F8F6;
    --text-main: #2C352D;
    --text-muted: #6B7280;
    --white: #FFFFFF;
    --font-heading: 'Playfair Display', serif;
    --font-body: 'Plus Jakarta Sans', sans-serif;
    --card-radius: 16px;
    --shadow: 0 10px 30px rgba(0,0,0,0.04);
    --border-card: 1px solid rgba(74, 93, 78, 0.08);
}

:root {
    --transition: all 0.3s ease;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
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
    background: linear-gradient(rgba(44, 53, 45, 0.5), rgba(44, 53, 45, 0.6)), url('https://res.cloudinary.com/p0qlmlor/image/upload/q_auto,f_auto,w_1200/v1/monarca/instalaciones/fachada.webp') center/cover no-repeat;
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
    grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
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
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
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

.logo {
    display: flex;
    align-items: center; /* Esto asegura que el logo quede centrado verticalmente */
}

.logo img {
    max-height: 70px; /* Ajusta este valor (ej. 50px o 70px) hasta que lo veas perfecto */
    width: auto;      /* Mantiene las proporciones originales para que no se deforme */
    display: block;
}


.whatsapp-float {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: #25d366;
  color: white;
  border-radius: 50px;
  text-align: center;
  font-size: 30px;
  box-shadow: 2px 2px 10px rgba(0,0,0,0.3);
  z-index: 1000;
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  transition: transform 0.3s ease;
}

.whatsapp-float:hover {
  transform: scale(1.1);
  color: white;
}


.content-subtitle {
    color: var(--primary);
    font-size: 1.1rem;
    font-weight: bold;
    margin-bottom: 0.8rem;
    font-family: var(--font-heading);
}

.content-subtitle-spaced {
    color: var(--primary);
    font-size: 1.1rem;
    font-weight: bold;
    margin: 1.5rem 0 0.8rem 0;
    font-family: var(--font-heading);
}

```

### Archivo: `documentacion/LEEME.md`
```md
# Documentación

La documentación de este proyecto está en el repositorio **documentacion-central**:

https://github.com/FABIOR1981/documentacion-central/tree/main/monarca/documentacion

También se puede consultar desde la bitácora de proyectos.

```

### Archivo: `img/Logo_verdeOliva_transparente.webp`
```webp
[Archivo Binario o No Leible]
```

### Archivo: `img/director.webp`
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
    <meta name="description" content="Residencial Monarca: un hogar donde cuidar es acompañar. Residencial geriátrico con atención profesional, instalaciones cálidas y equipo de enfermería.">
    <title>Monarca | Residencial Geriátrico</title>
    
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

    
    <link rel="preload" as="image" href="https://res.cloudinary.com/p0qlmlor/image/upload/q_auto,f_auto,w_1200/v1/monarca/instalaciones/fachada.webp" fetchpriority="high">
</head>
<body>

    <!-- Header -->
    <header>
        <div class="logo">
            <img src="img/Logo_verdeOliva_transparente.webp" alt="Residencial Monarca" width="238" height="118">
        </div>

        <div class="menu-toggle" id="mobile-menu">
            <i class="fas fa-bars"></i>
        </div>
        <nav>
            <ul id="nav-list">
                <li><a href="#inicio">Inicio</a></li>
                <li><a href="#mision-vision">Nosotros</a></li>
                <li><a href="#valores">Valores</a></li>
                <li><a href="#servicios">Servicios</a></li>
                <li><a href="#instalaciones">Instalaciones</a></li>
                <li><a href="#galeria">Galería</a></li>
                <li><a href="#contacto">Contacto</a></li>
            </ul>
        </nav>
    </header>

    <!-- Hero -->
    <section id="inicio" class="hero">
        <div class="hero-content">
            <h1>MONARCA</h1>
            <h2>“Un hogar donde cuidar es acompañar”</h2>
            <p>Brindamos un espacio seguro, cálido y profesional pensado para el bienestar integral.</p>
            <a href="#contacto" class="btn">Contactate con nosotros</a>
        </div>
    </section>

    <!-- Perfil del Director (Izquierda) y Misión/Visión (Derecha) -->
    <section id="mision-vision">
        <h2 class="section-title">Sobre Nosotros</h2>
        <div class="about-grid">
            
            <!-- Columna Izquierda: Perfil del Director con botón expandible -->
            <div class="card-director">
                <div>
                    <div class="director-header">
                        <img src="img/director.webp" alt="Director de Residencial Monarca" class="director-img" width="240" height="323">
                        <div>
                            <h3>Dirección y Compromiso</h3>
                            <p class="director-name">Javier Bia</p>
                            <span class="director-subtitle">Lic. en Enfermería</span>
                        </div>
                    </div>

                    <div class="expandable-container" id="director-text-container">
                        <div class="expandable-content">
                            <p class="content-subtitle">Mi compromiso con Monarca</p>
                            <p>Soy Licenciado en Enfermería y cuento con más de 20 años de experiencia en el ámbito de la salud, con una trayectoria especialmente vinculada a la salud mental y al cuidado de las personas.</p>
                            
                            <p>A lo largo de mi carrera he buscado ampliar permanentemente mi formación para brindar una atención responsable, humana y de calidad. Cuento con capacitación en geriatría, manejo y administración de medicamentos, RCP, lengua de señas y atención ante situaciones de crisis, entre otras áreas vinculadas al cuidado.</p>
                            
                            <p>También tuve la oportunidad de participar en misiones de paz de la Organización de las Naciones Unidas (ONU), una experiencia que enriqueció mi formación profesional y personal, fortaleciendo mis capacidades para trabajar en diferentes contextos y afrontar situaciones complejas.</p>
                            
                            <p>Mi formación incluye además gestión de riesgos, responsabilidad social empresarial y estrategias para la optimización del rol del acompañante y su integración en la vida cotidiana de las personas. He participado en jornadas y capacitaciones relacionadas con el adulto mayor, medicina defensiva y medicina asertiva, aspectos médico-toxicológicos vinculados al uso de sustancias psicoactivas y estrategias de intervención ante situaciones de crisis.</p>

                            <p class="content-subtitle-spaced">¿Por qué Monarca?</p>
                            <p>Después de tantos años trabajando en el ámbito de la salud, entiendo que cuidar no significa solamente atender una necesidad. Cuidar también es escuchar, respetar, acompañar y estar presente.</p>
                            
                            <p>Por eso nace Residencial Monarca: con la intención de crear un espacio donde cada persona pueda sentirse segura, valorada y acompañada, y donde las familias encuentren la tranquilidad de saber que sus seres queridos están en un entorno cálido y cuidado.</p>
                            
                            <p>Mi compromiso es trabajar junto a nuestro equipo para que Monarca sea mucho más que un residencial: sea un hogar.<br>
                            <strong>Bienvenidos a Monarca. Un hogar donde cuidar es acompañar.</strong></p>
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
                        <h3><i class="fas fa-bullseye"></i> Nuestra Misión</h3>
                        <p>En Residencial Monarca brindamos un hogar cálido, seguro y acogedor, donde cada persona sea cuidada con respeto, empatía y dedicación. Contamos con un equipo capacitado y una atención cercana, orientada a promover la autonomía, el bienestar y la calidad de vida de cada residente. Queremos que quienes viven en Monarca se sientan acompañados, valorados y, sobre todo, como en casa.</p>
                    </div>
                    
                    <div class="mv-divider"></div>

                    <div class="mv-block">
                        <h3><i class="fas fa-eye"></i> Nuestra Visión</h3>
                        <p>Ser un residencial reconocido por la calidad humana de su atención, la confianza de las familias y el compromiso con el bienestar integral de sus residentes. Aspiramos a construir un lugar donde envejecer sea una etapa vivida con dignidad, tranquilidad, afecto y respeto.</p>
                    </div>
                </div>
            </div>

        </div>
    </section>

    <!-- Valores -->
    <section id="valores">
        <h2 class="section-title">Nuestros Valores</h2>
        <div class="values-grid">
            <div class="value-item">
                <i class="fas fa-heart"></i>
                <h3>Calidez Humana</h3>
                <p>Cuidamos desde la cercanía, el afecto y la escucha. Buscamos que cada residente se sienta acompañado, querido y contenido.</p>
            </div>
            <div class="value-item">
                <i class="fas fa-hand-holding-heart"></i>
                <h3>Respeto y Dignidad</h3>
                <p>Reconocemos la historia, las preferencias, las costumbres y la individualidad de cada residente, promoviendo un trato digno.</p>
            </div>
            <div class="value-item">
                <i class="fas fa-shield-alt"></i>
                <h3>Seguridad y Confianza</h3>
                <p>Trabajamos para ofrecer un entorno seguro, tranquilo y cuidado, generando confianza tanto en residentes como en sus familias.</p>
            </div>
            <div class="value-item">
                <i class="fas fa-user-md"></i>
                <h3>Profesionalismo</h3>
                <p>Contamos con un equipo de profesionales capacitados y comprometidos para brindar una atención responsable, cercana y de calidad.</p>
            </div>
            <div class="value-item">
                <i class="fas fa-smile"></i>
                <h3>Bienestar y Calidad de Vida</h3>
                <p>Promovemos una vida activa y confortable, respetando las posibilidades, necesidades y preferencias de cada residente.</p>
            </div>
            <div class="value-item">
                <i class="fas fa-home"></i>
                <h3>Sentido de Hogar</h3>
                <p>Monarca busca mucho más que un lugar donde vivir: queremos construir un hogar donde compartir y pertenecer.</p>
            </div>
        </div>
    </section>

    <!-- Servicios -->
    <section id="servicios" style="background-color: var(--bg-light);">
        <h2 class="section-title">Nuestros Servicios</h2>
        <div class="services-container">
            
            <!-- Instalaciones y Comodidades -->
            <div class="service-block">
                <div>
                    <h3><i class="fas fa-home"></i> Instalaciones</h3>
                    <p>Un espacio pensado para sentirse como en casa. En Residencial Monarca cuidamos cada detalle para ofrecer un entorno cálido, confortable y seguro.</p>
                    
                    <div class="expandable-container" id="service-instalaciones">
                        <div class="expandable-content">
                            <ul class="service-list" style="margin-top: 1rem;">
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
            <div class="service-block service-fixed-height">
                <div>
                    <h3><i class="fas fa-utensils"></i> Alimentación</h3>
                    <p>En Residencial Monarca ofrecemos una alimentación variada y equilibrada, contemplando las necesidades de cada residente.</p>
                    <ul class="service-list">
                        <li><strong>Servicio completo:</strong> Contamos con desayuno, almuerzo, merienda, cena y colaciones, además de dietas especiales de acuerdo con las necesidades y requerimientos de cada persona.</li>
                        <li><strong>Sentido de hogar:</strong> Buscamos que cada comida sea también un momento agradable para compartir, disfrutar y sentirse como en casa.</li>
                    </ul>
                </div>
            </div>

            <!-- Talleres y Actividades -->
            <div class="service-block">
                <div>
                    <h3><i class="fas fa-puzzle-piece"></i> Talleres y Actividades</h3>
                    <p>En Residencial Monarca promovemos espacios de encuentro, movimiento y creatividad, adaptados a las posibilidades e intereses de cada residente.</p>
                    
                    <div class="expandable-container" id="service-talleres">
                        <div class="expandable-content">
                            <ul class="service-list" style="margin-top: 1rem;">
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
            <div class="service-block">
                <div>
                    <h3><i class="fas fa-user-nurse"></i> Equipo, Enfermería</h3>
                    <p>En Residencial Monarca contamos con un equipo de profesionales de la salud comprometido con el cuidado y bienestar de cada residente.</p>
                    
                    <div class="expandable-container" id="service-enfermeria">
                        <div class="expandable-content">
                            <p style="margin-bottom: 1rem;">Nuestro trabajo incluye:</p>
                            <ul class="service-list">
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
        <h2 class="section-title">Nuestras Instalaciones</h2>
        
        <div class="gallery-filters" id="instalaciones-filtros">
            <button class="filter-btn active" data-filter="all">Todas</button>
            <button class="filter-btn" data-filter="exterior">Exterior</button>
            <button class="filter-btn" data-filter="interior">Áreas Comunes</button>
            <button class="filter-btn" data-filter="habitacion">Habitaciones</button>
        </div>

        <div class="gallery-grid" id="instalaciones-dinamicas">
            <!-- Se carga dinámicamente desde Cloudinary -->
        </div>
    </section>
    <!-- Galería de Actividades Dinámica -->
    <section id="galeria" class="gallery-container">
        <h2 class="section-title">Galería</h2>
        
        <div class="gallery-filters">
            <p style="text-align: center; color: var(--text-muted); width: 100%;">Momentos y actividades en Residencial Monarca</p>
        </div>

        <div class="gallery-grid" id="galeria-dinamica">
        </div>
    </section>

    <!-- Lightbox Modal -->
    <div class="lightbox" id="lightbox">
        <span class="lightbox-close" id="lightbox-close">&times;</span>
        <img id="lightbox-img" src="" alt="Imagen ampliada">
    </div>

    <!-- Contacto -->
    <section id="contacto">
        <h2 class="section-title">Contacto</h2>
        <div class="contact-content">
            <p>¿Querés coordinar una visita o tenés alguna consulta? Estamos para escucharte y asesorarte de forma personalizada.</p>
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

    <footer>
        <p>&copy; 2026 Residencial Monarca. Todos los derechos reservados.</p>
    </footer>

    <!-- Scripts con defer para no bloquear el renderizado -->
    <script src="js/config.js" defer></script>
    <script src="js/main.js" defer></script>
</body>
</html>
```

### Archivo: `js/config.js`
```javascript
// Configuración general de Residencial Monarca
// Único lugar donde se cambian los datos del sitio: Cloudinary, contacto y redes.
// Debe cargarse ANTES de main.js (ver el final de index.html).
const CONFIG = {

  // --- Galería y Dinámicos (Cloudinary) ---
  CLOUDINARY: {
    CLOUD_NAME: 'p0qlmlor',        // Cloud name de la cuenta
    TAG_GALERIA: 'monarca_galeria',  // Etiqueta que llevan las fotos de la galería
    TAG_INSTALACIONES: 'monarca_instalaciones' // Etiqueta que llevan las fotos de instalaciones en Cloudinary
  },

  // --- Contacto y redes ---
  CONTACTO: {
    DIRECCION: 'Rivera 5734 esquina Vicente Rocafuerte',
    WHATSAPP_NUMERO: '59899081886',  
    INSTAGRAM_URL: 'https://www.instagram.com/recidencialmonarca?utm_source=qr',    
    FACEBOOK_URL: 'https://www.facebook.com/share/1JEh8uoqEZ/?mibextid=wwXIfr'      
  }
};
```

### Archivo: `js/main.js`
```javascript
document.addEventListener('DOMContentLoaded', () => {
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
        return nombre ? nombre.charAt(0).toUpperCase() + nombre.slice(1) : 'Monarca';
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
            document.querySelectorAll('#instalaciones-dinamicas .gallery-item').forEach(item => {
                const visible = filtro === 'all' || item.getAttribute('data-category') === filtro;
                item.style.display = visible ? '' : 'none';
            });
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
                    button.textContent = 'Ver menos';
                } else {
                    button.textContent = 'Ver más';
                    container.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
                }
            }
        });
    });

    // 5. Lógica del Lightbox
    function inicializarLightbox() {
        const lightbox = document.getElementById('lightbox');
        const lightboxImg = document.getElementById('lightbox-img');
        const lightboxClose = document.getElementById('lightbox-close');
        const allGalleryItems = document.querySelectorAll('.gallery-item');

        if (!lightbox || !lightboxImg || !lightboxClose) return;

        allGalleryItems.forEach(item => {
            const nuevoItem = item.cloneNode(true);
            item.parentNode.replaceChild(nuevoItem, item);
            
            nuevoItem.addEventListener('click', () => {
                const imgElement = nuevoItem.querySelector('img');
                if (imgElement) {
                    lightboxImg.setAttribute('src', imgElement.getAttribute('src'));
                    lightbox.style.display = 'flex';
                }
            });
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

    // 6. Datos de contacto y redes desde config.js
    const cfgContacto = (typeof CONFIG !== 'undefined' && CONFIG.CONTACTO) ? CONFIG.CONTACTO : {};

    function asignarEnlace(clave, url) {
        if (!url) return;
        document.querySelectorAll(`[data-config="${clave}"]`).forEach(el => { el.href = url; });
    }

    // Enlace normal de WhatsApp (para botones de texto o secciones) — con el mismo mensaje predeterminado
    const mensajeWhatsapp = "Hola, quiero consultar sobre los servicios de Residencial Monarca";
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

### Archivo: `netlify.toml`
```toml
[build]
  publish = "."

  [build.environment]
  NETLIFY_BUILD_DISABLE_BADGE = "true"
```

