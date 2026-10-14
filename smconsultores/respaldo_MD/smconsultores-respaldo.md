# Bundle del Proyecto para IA: smconsultores

## Estructura de Archivos
- README.md
- img/auditoria-soporte.jpg
- img/logistica-comex.jpg
- img/logos_clientes/1_logo-aramaycia.png
- img/logos_clientes/2_logo-ibarra.png
- img/logos_clientes/3_logo-santarosa.png
- img/logos_clientes/4_logo-liguori.png
- img/logos_clientes/5_logo-ace.png
- img/perfil-shalon.jpg
- img/servicio-capacitacion.jpg
- img/servicio-sistemas.jpg
- img/servicio-talento.jpg
- img/servicio-talento_2.jpg
- index.html
- js/app.js
- js/logos.js
- js/main.js
- style.css

---

## Código Fuente

### Archivo: `README.md`
```md
# SM Consultores

Sitio institucional de **SM Consultores**, consultora en sistemas de gestión, desarrollo organizacional, OEC (Operador Económico Calificado), comercio exterior y cadena de suministro.

Sitio publicado: https://smconsultores.netlify.app

Para probar cambios antes de publicarlos está la versión de pruebas: [smconsultores-test](https://github.com/FABIOR1981/smconsultores-test).

## Contenido del sitio

- **Portada**: estrategia, sistemas de gestión y desarrollo organizacional.
- **Servicios especializados**:
  - Gestión del talento y cultura organizacional.
  - Sistemas de gestión, seguridad y certificaciones.
  - Desarrollo organizacional y comunicación.
- **Principales clientes de comercio exterior**, con sus logos.
- **Contacto**: para coordinar una reunión, con los canales de comunicación.
- Menú adaptable a celular (hamburguesa) que resalta la sección que se está viendo.

## Cómo actualizar

### Logos de clientes

1. Subí el logo a `img/logos_clientes/` con un número adelante, que define el orden. Por ejemplo: `6_logo-nuevo.png`.
2. Agregalo a la lista en `js/logos.js`, con el nombre del archivo y la web del cliente.

`js/main.js` lee esa lista, ordena los logos por el número del archivo y arma las tarjetas.

### Textos e imágenes

Los textos están en `index.html` y las imágenes de los servicios en `img/`.

## Ejecutar localmente

No necesita instalación. Abrí `index.html` en el navegador.

## Estructura

```
index.html            Página completa
style.css             Estilos
js/app.js             Menú en celular y resaltado de la sección activa
js/logos.js           Lista de logos de clientes (lo único a editar al sumar uno)
js/main.js            Carga y ordena los logos
img/                  Imágenes del sitio y logos de clientes
```

```

### Archivo: `img/auditoria-soporte.jpg`
```jpg
[Archivo Binario o No Leible]
```

### Archivo: `img/logistica-comex.jpg`
```jpg
[Archivo Binario o No Leible]
```

### Archivo: `img/logos_clientes/1_logo-aramaycia.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `img/logos_clientes/2_logo-ibarra.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `img/logos_clientes/3_logo-santarosa.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `img/logos_clientes/4_logo-liguori.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `img/logos_clientes/5_logo-ace.png`
```png
[Archivo Binario o No Leible]
```

### Archivo: `img/perfil-shalon.jpg`
```jpg
[Archivo Binario o No Leible]
```

### Archivo: `img/servicio-capacitacion.jpg`
```jpg
[Archivo Binario o No Leible]
```

### Archivo: `img/servicio-sistemas.jpg`
```jpg
[Archivo Binario o No Leible]
```

### Archivo: `img/servicio-talento.jpg`
```jpg
[Archivo Binario o No Leible]
```

### Archivo: `img/servicio-talento_2.jpg`
```jpg
[Archivo Binario o No Leible]
```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SM Consultores - Sistemas de Gestión y Mejora Organizacional</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="blob-decorator blob-1"></div>
    <div class="blob-decorator blob-2"></div>

    <header class="navbar">
        <div class="logo">SM <span>Consultores</span></div>
        <button class="menu-toggle" id="menuToggle" aria-label="Abrir menú">&#9776;</button>
        <nav class="nav-links" id="navLinks">
            <a href="#inicio" class="nav-item">Inicio</a>
            <a href="#experta" class="nav-item">Especialidad</a>
            <a href="#servicios" class="nav-item">Servicios</a>
            <a href="#clientes" class="nav-item">Clientes</a>
            <a href="#contacto" class="nav-item">Contacto</a>
        </nav>
    </header>

    <section id="inicio" class="hero-section">
        <div class="hero-container">
            <div class="hero-text-block">
                <span class="badge">Sistemas de Gestión y Mejora Organizacional</span>
                <h2 class="section-main-title">Estrategia, Sistema de Gestión y Desarrollo Organizacional</h2>
                <p>Acompañamos a las Organizaciones en el fortalecimiento de su gestión, el cumplimiento normativo y la mejora continua, mediante la implementación de sistemas de gestión, auditorías en la cadena de suministro y procesos de certificación alineados a estándares internacionales. Integramos además la comunicación organizacional y la gestión de personas como factores clave para impulsar el cambio, fortalecer la cultura corporativa, desarrollar competencias y asegurar la sostenibilidad de los resultados. Complementamos este enfoque con herramientas de comunicación organizacional, gestión del cambio y desarrollo de personas, acompañando a la Dirección en la definición de estrategias, la toma de decisiones y el fortalecimiento de la cultura organizacional para alcanzar resultados sostenibles.</p>
                <!--<p>Complemento este enfoque con herramientas de comunicación organizacional, gestión del cambio y desarrollo de personas, acompañando a la Dirección en la definición de estrategias, la toma de decisiones y el fortalecimiento de la cultura organizacional para alcanzar resultados sostenibles.</p>-->
                <div class="hero-actions">
                    <a href="#contacto" class="btn-primary">Contacto Directo</a>
                    <a href="#servicios" class="btn-secondary">Nuestros Servicios</a>
                </div>
            </div>
            <div class="hero-image-block">
                <div class="image-wrapper main-img">
                    <img src="img/logistica-comex.jpg" alt="Logística y Comercio Exterior">
                </div>
                <div class="image-wrapper sub-img">
                    <img src="img/auditoria-soporte.jpg" alt="Auditoría de Sistemas">
                </div>
            </div>
        </div>
    </section>

    <section id="experta" class="about-section">
        <div class="about-grid">
            <div class="about-images">
                <div class="accent-box"></div>
                <div class="image-wrapper-about">
                    <img src="img/perfil-shalon.jpg" alt="Consultora Experta Shalon Morales">
                </div>
            </div>
            <div class="about-text">
                <span class="section-title-badge">PERFIL PROFESIONAL</span>
                <h2 class="section-main-title">Consultora Experta en OEC, Comercio Exterior y Cadena de Suministro</h2>
                <p class="highlight-text">"Experiencia real en Comercio Exterior aplicada a Seguridad y Gestión"</p>
                <p>Soy Licenciada en Ciencias de la Comunicación, con Posgrado en Comunicación Organizacional. Auditora de Sistemas certificada por AENOR y Especialista en Sistemas de Gestión Integrados, Normas ISO y Programa Operador Económico Calificado (OEC).</p>
                <p>Cuento con amplia experiencia en Comercio Exterior, Logística y Cadena de Suministro, acompañando a Agentes de Carga, Importadores, Despachantes de Aduana, Operadores Logísticos y empresas de diversos sectores en la gestión de riesgos, el cumplimiento normativo y la mejora continua.</p>
                <p>Mi trayectoria en diferentes industrias me ha permitido desarrollar soluciones prácticas, adaptadas a cada organización, integrando una visión estratégica orientada a la eficiencia, la seguridad y el desarrollo sostenible.</p>
                
            </div>
        </div>
    </section>

    <section id="servicios" class="services-section">
        <span class="section-title-badge center">IMPULSAMOS LA EXCELENCIA</span>
        <h2 class="section-main-title">Nuestros Servicios Especializados</h2>
        
        <div class="services-grid">
            <div class="card-canvas">
               <div class="card-image">
                    <img src="img/servicio-capacitacion.jpg" alt="Desarrollo de Equipos">
                </div>
                <div class="card-body">
                    <h3>Gestión del Talento y Cultura Organizacional</h3>
                    <ul>
                        <li>Selección y evaluación de talento por competencias.</li>
                        <li>Diagnóstico de clima laboral, cultura y desempeño organizacional.</li>
                        <li>Desarrollo de líderes y equipos de alto desempeño.</li>
                        <li>Gestión del cambio y acompañamiento en procesos de transformación.</li>
                        <li>Intervención en situaciones de crisis, mediación y resolución de conflictos.</li>
                        <li>Diseño e implementación de talleres de sensibilización en salud mental, bienestar laboral, liderazgo y fortalecimiento cultural.</li>
                    </ul>
                </div>
            </div>

            <div class="card-canvas highlight-card">
                <div class="card-image">
                    <img src="img/servicio-sistemas.jpg" alt="Implementación de Sistemas">
                </div>
                <div class="card-body">
                    <h3>Sistemas de Gestión,Seguridad y Certificaciones</h3>
                    <ul>
                        <li>Diagnóstico organizacional y análisis de brechas conforme a Normas ISO, Programa OEC y otros estándares aplicables.</li>
                        <li>Implementación y fortalecimiento de sistemas de gestión.</li>
                        <li>Gestión de riesgos y seguridad en la cadena de suministro.</li>
                        <li>Diseño e implementación de procedimientos y controles</li>
                        <li>Auditorías internas, externas y de segunda parte.</li>
                        <li>Acompañamiento integral en procesos de certificación y mejora continua</li>
                    </ul>
                </div>
                
            </div>

            <div class="card-canvas">
                <div class="card-image">
                    <img src="img/servicio-talento.jpg" alt="Gestión de Personas">
                </div>
                <div class="card-body">
                    <h3>Desarrollo Organizacional y Comunicación</h3>
                    <ul>
                        <li>Capacitación y formación a medida.</li>
                        <li>Desarrollo de competencias y cultura preventiva.</li>
                        <li>Fortalecimiento de la comunicación y el trabajo en equipo</li>
                        <li>Comunicación organizacional y fortalecimiento de la cultura corporativa.</li>
                        <li>Gestión del cambio y transformación organizacional.</li>
                        <li>Diseño de estrategias de comunicación interna y alineación de equipos con los objetivos de la organización.</li>
                    </ul>
                </div>
            </div>
        </div>

        <p class="services-footer-quote">“Contamos con un equipo multidisciplinario de profesionales especializados, que aportan una visión integral y estratégica de las operaciones de Comercio Exterior”.</p>
    </section>

    <section id="clientes" class="clients-section">
        <span class="section-title-badge center">RESPALDO Y CONFIANZA</span>
        <h2 class="section-main-title">Principales Clientes de Comercio Exterior</h2>
        <p class="clients-subtitle">“La confianza de nuestros clientes respalda nuestros resultados”</p>
        
        <div id="clients-grid" class="clients-grid"></div>
    </section>

    <section id="contacto" class="contact-section">
    <div class="contact-wrapper">
        <div class="contact-info-panel">
            <h2>Coordinemos una reunion</h2>
            <p><strong>Transformemos desafíos en oportunidades de mejora</strong></p>
            <p>Si desea coordinar una reunión de diagnóstico, solicitar una propuesta o conocer más sobre nuestros servicios, ponemos a tu disposición los siguientes canales de comunicación.</p>
            
            <div class="contact-details">
                <p><strong>Razón Social:</strong> Shalon Elizabeth Morales Caraballo</p>
                <p><strong>RUT:</strong> 219364190012</p>
                <p><strong>Modalidad de atención:</strong> Presencial y remota</p>
                <p><strong>Ubicación:</strong> Montevideo, Uruguay</p>
            </div>
        </div>

        <div class="contact-channels-panel">
            <span class="badge-channels">Canales de Comunicación</span>
            <!--<h3>Canales de Comunicación</h3>-->
            
            <div class="channel-box">
                <div class="channel-icon">💼</div>
                <div class="channel-data">
                    <span class="channel-label">LinkedIn</span>
                    <a href="https://www.linkedin.com/in/shalonmorales/" target="_blank" rel="noopener noreferrer" class="channel-link">Shalon Morales</a>
                </div>
            </div>

            

            <div class="channel-box">
                <div class="channel-icon">📱</div>
                <div class="channel-data">
                    <span class="channel-label">Teléfono / WhatsApp</span>
                    <a href="https://wa.me/59899987300" target="_blank" rel="noopener noreferrer" class="channel-link">(+598) 99 987 300</a>
                </div>
            </div>

            <div class="channel-box">
                <div class="channel-icon">✉️</div>
                <div class="channel-data">
                    <span class="channel-label">Correo electrónico</span>
                    <a href="mailto:shalonmorales@gmail.com" class="channel-link">shalonmorales@gmail.com</a>
                </div>
            </div>
            
        </div>
    </div>
</section>

    <footer>
        <div class="footer-content">
            <p class="footer-logo">SM Consultores</p>
            <p>&copy; 2026 SM Consultores. Estrategia, Gestión y Cumplimiento. Todos los derechos reservados.</p>
        </div>
    </footer>

    <script src="js/app.js"></script>
    <script src="js/logos.js"></script>
    <script src="js/main.js"></script>
</body>
</html>
```

### Archivo: `js/app.js`
```javascript
document.addEventListener('DOMContentLoaded', () => {
    const menuToggle = document.getElementById('menuToggle');
    const navLinks = document.getElementById('navLinks');
    const navItems = document.querySelectorAll('.nav-item');
    const sections = document.querySelectorAll('section');

    // Control del Menú Hamburguesa para móviles
    menuToggle.addEventListener('click', () => {
        navLinks.classList.toggle('active');
    });

    // Cerrar menú al hacer click en un enlace (ideal para móviles)
    navItems.forEach(item => {
        item.addEventListener('click', () => {
            navLinks.classList.remove('active');
        });
    });

    // Iluminación activa del menú según la sección en pantalla (ScrollSpy)
    const observerOptions = {
        root: null,
        rootMargin: '-50% 0px -50% 0px', // Detecta cuando la sección ocupa el centro de la pantalla
        threshold: 0
    };

    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                const id = entry.target.getAttribute('id');
                navItems.forEach(item => {
                    item.classList.remove('active');
                    if (item.getAttribute('href') === `#${id}`) {
                        item.classList.add('active');
                    }
                });
            }
        });
    }, observerOptions);

    sections.forEach(section => {
        observer.observe(section);
    });
});
```

### Archivo: `js/logos.js`
```javascript
// Solo editas esta lista cuando agregas un archivo nuevo
const clientesComex = [
    {
        nombre: "Aramaycia",
        logo: "1_logo-aramaycia.png",
        url: "https://www.aramaycia.com/"
    },
    {
        nombre: "Ibarra Comex",
        logo: "2_logo-ibarra.png",
        url: "https://www.ibarra.com.uy/"
    },
    {
        nombre: "Santa Rosa",
        logo: "3_logo-santarosa.png",
        //url: "https://www.nissan.com.uy/"
    },
    {
        nombre: "Liguori Dendi",
        logo: "4_logo-liguori.png",
        url: "https://ld.com.uy/"
    },
    {
        nombre: "ACE",
        logo: "5_logo-ace.png",
        url: "https://coopelectoral.com.uy/"
    }
];
```

### Archivo: `js/main.js`
```javascript
// ==========================================================================
// LÓGICA DE CARGA Y ORDENACIÓN DINÁMICA DE LOGOS DE CLIENTES
// Ahora toma la variable 'clientesComex' directamente de logos.js
// ==========================================================================

// Ya no declaramos 'clientesComex' aquí, la toma del archivo logos.js

// Función para leer el prefijo numérico del archivo (ej: "1_logo-aramaycia.png" -> 1)
function obtenerOrden(nombreArchivo) {
    const partes = nombreArchivo.split('_');
    const numero = parseInt(partes[0], 10);
    return isNaN(numero) ? 99 : numero; // Si un archivo nuevo no tiene prefijo, va al final
}

// Renderizado dinámico de las tarjetas de clientes
function cargarLogosClientes() {
    const grid = document.getElementById('clients-grid');
    if (!grid) return; // Seguridad en caso de que no encuentre el contenedor

    // Ordenar los clientes por el número del prefijo del archivo
    // Usamos la variable 'clientesComex' que viene desde logos.js
    const clientesOrdenados = [...clientesComex].sort((a, b) => {
        return obtenerOrden(a.logo) - obtenerOrden(b.logo);
    });

    // Inyectar la estructura HTML correspondiente a cada logo en la subruta indicada
    grid.innerHTML = clientesOrdenados.map(cliente => {
        const logoHtml = `
            <div class="client-logo-container">
                <img src="img/logos_clientes/${cliente.logo}" alt="Logo ${cliente.nombre}" class="client-logo">
            </div>`;

        if (cliente.url) {
            return `
                <div class="client-card">
                    <a class="client-card-link" href="${cliente.url}" target="_blank" rel="noopener noreferrer" title="${cliente.nombre}">
                        ${logoHtml}
                    </a>
                </div>`;
        }

        return `
            <div class="client-card">
                ${logoHtml}
            </div>`;
    }).join('');
}

// Ejecutar la carga cuando el documento esté completamente listo
document.addEventListener('DOMContentLoaded', () => {
    if (typeof clientesComex !== 'undefined') {
        cargarLogosClientes();
    } else {
        console.warn('clientesComex no está definido. Verifica que js/logos.js se cargue antes de js/main.js.');
    }
});
```

### Archivo: `style.css`
```css
/* ==========================================================================
   Variables Globales y Paleta de Colores (Estilo Canva de Alta Gama)
   ========================================================================== */
:root {
    --primary-deep: #0a2540;     /* Azul marino Canva clásico */
    --secondary-gold: #cfa864;   /* Dorado / Bronce corporativo para acentos */
    --teal-accent: #20c997;      /* Toque moderno */
    --bg-warm: #fdfbf7;          /* Fondo hueso/cálido en vez de blanco puro */
    --card-bg: #ffffff;
    --text-main: #334155;
    --text-light: #64748b;
    --white: #ffffff;
    --radius-large: 16px;
    --radius-medium: 8px;
    --shadow-heavy: 0 20px 40px rgba(0,0,0,0.08);
    --shadow-soft: 0 10px 20px rgba(0,0,0,0.04);
}

/* ==========================================================================
   Reset y Estilos Base
   ========================================================================== */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html {
    scroll-behavior: smooth;
    font-family: 'Inter', system-ui, -apple-system, sans-serif;
    color: var(--text-main);
    background-color: var(--bg-warm);
}

/* Decoraciones Abstractas de Fondo */
.blob-decorator {
    position: absolute;
    width: 400px;
    height: 400px;
    border-radius: 50%;
    filter: blur(120px);
    z-index: -1;
    opacity: 0.4;
}
.blob-1 {
    background-color: #e2d4bc;
    top: 10%;
    right: -5%;
}
.blob-2 {
    background-color: #d0e1fd;
    top: 60%;
    left: -10%;
}

/* ==========================================================================
   Navbar (Navegación Superior)
   ========================================================================== */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1.5rem 4rem;
    background-color: rgba(253, 251, 247, 0.95);
    backdrop-filter: blur(10px);
    position: sticky;
    top: 0;
    z-index: 1000;
    border-bottom: 1px solid rgba(0,0,0,0.05);
}

.logo {
    font-size: 1.6rem;
    font-weight: 800;
    color: var(--primary-deep);
    letter-spacing: -0.5px;
}

.logo span {
    color: var(--secondary-gold);
    font-weight: 300;
}

.nav-links {
    display: flex;
    gap: 2.5rem;
}

.nav-item {
    text-decoration: none;
    color: var(--text-main);
    font-weight: 600;
    font-size: 0.95rem;
    transition: all 0.3s ease;
}

.nav-item:hover, .nav-item.active {
    color: var(--secondary-gold);
}

.menu-toggle {
    display: none;
    font-size: 1.8rem;
    background: none;
    border: none;
    cursor: pointer;
    color: var(--primary-deep);
}

/* ==========================================================================
   Insignias de Título (Badges)
   ========================================================================== */
.badge, .section-title-badge {
    display: inline-block;
    padding: 0.4rem 1rem;
    background-color: rgba(207, 168, 100, 0.15);
    color: #96712b;
    border-radius: 50px;
    font-size: 0.8rem;
    font-weight: 700;
    letter-spacing: 1.5px;
    margin-bottom: 1.2rem;
    text-transform: uppercase;
}
.section-title-badge.center {
    display: table;
    margin: 0 auto 1rem auto;
}

/* ==========================================================================
   Sección Héroe (Inicio)
   ========================================================================== */
.hero-section {
    min-height: 85vh;
    display: flex;
    align-items: center;
    padding: 4rem;
}

.hero-container {
    display: grid;
    grid-template-columns: 1.1fr 0.9fr;
    gap: 4rem;
    max-width: 1300px;
    margin: 0 auto;
    align-items: center;
}

.hero-text-block h1 {
    font-size: 3.5rem;
    color: var(--primary-deep);
    line-height: 1.15;
    margin-bottom: 1.5rem;
    font-weight: 800;
}

.hero-text-block p {
    font-size: 1.15rem;
    color: var(--text-light);
    margin-bottom: 2.5rem;
    line-height: 1.7;
}

.hero-actions {
    display: flex;
    gap: 1rem;
}

.btn-primary {
    padding: 1rem 2.2rem;
    background-color: var(--primary-deep);
    color: var(--white);
    text-decoration: none;
    border-radius: var(--radius-medium);
    font-weight: 700;
    box-shadow: 0 10px 20px rgba(10, 37, 64, 0.15);
    transition: all 0.3s ease;
}

.btn-primary:hover {
    transform: translateY(-2px);
    background-color: #12355a;
}

.btn-secondary {
    padding: 1rem 2.2rem;
    background-color: transparent;
    color: var(--primary-deep);
    border: 2px solid var(--primary-deep);
    text-decoration: none;
    border-radius: var(--radius-medium);
    font-weight: 700;
    transition: all 0.3s ease;
}

.btn-secondary:hover {
    background-color: var(--primary-deep);
    color: var(--white);
}

/* Collage de Imágenes del Héroe */
.hero-image-block {
    position: relative;
    height: 480px;
}

.image-wrapper {
    position: absolute;
    border-radius: var(--radius-large);
    overflow: hidden;
    box-shadow: var(--shadow-heavy);
}

.image-wrapper img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.main-img {
    width: 80%;
    height: 400px;
    top: 0;
    left: 0;
    z-index: 2;
    border: 8px solid var(--white);
}

.sub-img {
    width: 55%;
    height: 280px;
    bottom: 0;
    right: 0;
    z-index: 3;
    border: 8px solid var(--white);
}

/* ==========================================================================
   Sección Especialidad / Perfil Profesional (FOTO RECTANGULAR CORREGIDA)
   ========================================================================== */
.about-section {
    padding: 8rem 4rem;
}

.about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 6rem;
    max-width: 1200px;
    margin: 0 auto;
    align-items: center;
}

.about-images {
    position: relative;
    padding-left: 2rem;
}

.accent-box {
    position: absolute;
    width: 100%;
    max-width: 420px;       /* Vinculado al ancho de la imagen */
    height: 100%;           /* Toma el alto total de la imagen rectangular */
    background-color: var(--secondary-gold);
    top: -20px;
    left: -20px;
    border-radius: var(--radius-large);
    z-index: 1;
    opacity: 0.15;
}

.image-wrapper-about {
    position: relative;
    border-radius: var(--radius-large);
    overflow: hidden;
    box-shadow: var(--shadow-heavy);
    z-index: 2;
    
    /* CORRECCIÓN: Estructura vertical rectangular para plano más entero */
    width: 100%;
    max-width: 420px;       
    height: 580px;          
    margin: 0 auto;         
}

.image-wrapper-about img {
    width: 100%;
    height: 100%;
    object-fit: cover;      /* Escala sin deformar la silueta */
    object-position: top;   /* Prioriza encuadrar desde la cabeza hacia abajo */
}

.about-text h2 {
    font-size: 2.5rem;
    /*color: var(--primary-deep);*/
    margin-bottom: 1.5rem;
    font-weight: 800;
}

.highlight-text {
    font-size: 1.25rem;
    color: #96712b;
    font-weight: 600;
    margin-bottom: 1.5rem;
    line-height: 1.5;
    font-style: italic;
}

.about-text p {
    color: var(--text-light);
    margin-bottom: 1.5rem;
    font-size: 1.05rem;
}

/* ==========================================================================
   Sección Servicios (Multi-capa Contraste)
   ========================================================================== */
.services-section {
    padding: 8rem 4rem;
    background-color: #0a2540; 
    color: var(--white);
    text-align: center;
}

.section-main-title {
    font-size: 2.5rem; /* Ajusta este valor al tamaño deseado */
    font-weight: 700;   /* Negrita */
    line-height: 1.2;
    margin-bottom: 20px;
    color: var(--primary-deep);       /* Ajusta según tu paleta de colores */
}

.services-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 2.5rem;
    max-width: 1200px;
    margin: 0 auto;
    text-align: left;
}

.card-canvas {
    background-color: rgba(255, 255, 255, 0.03);
    border: 1px solid rgba(255, 255, 255, 0.08);
    border-radius: var(--radius-large);
    overflow: hidden;
    transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.card-canvas:hover {
    transform: translateY(-10px);
    background-color: rgba(255, 255, 255, 0.06);
    box-shadow: 0 30px 60px rgba(0,0,0,0.3);
}

.card-image {
    height: 200px;
    overflow: hidden;
}

.card-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.card-body {
    padding: 2.5rem 2rem;
}

.card-body h3 {
    font-size: 1.35rem;
    margin-bottom: 1.2rem;
    color: var(--secondary-gold);
    font-weight: 700;
}

.card-body ul {
    list-style: none;
}

.card-body ul li {
    position: relative;
    padding-left: 1.5rem;
    margin-bottom: 0.8rem;
    color: #cbd5e1;
    font-size: 0.95rem;
    line-height: 1.5;
}

.card-body ul li::before {
    content: "✓";
    position: absolute;
    left: 0;
    color: var(--secondary-gold);
    font-weight: bold;
}

/* Variación de Tarjeta Destacada Blanca */
.highlight-card {
    background-color: var(--white);
    color: var(--text-main);
    box-shadow: var(--shadow-heavy);
    border: none;
}

.highlight-card .card-body h3 {
    color: var(--primary-deep);
    font-weight: 800;
}

.highlight-card .card-body ul li {
    color: var(--text-main);
}

.highlight-card .card-body ul li::before {
    color: var(--primary-deep);
}

.services-footer-quote {
    max-width: 800px;
    margin: 4rem auto 0 auto;
    font-style: italic;
    color: #94a3b8;
    font-size: 1.1rem;
    line-height: 1.6;
    border-top: 1px solid rgba(255,255,255,0.1);
    padding-top: 2rem;
}

/* ==========================================================================
   Sección Clientes (GRILLA Y LOGOS ALINEADOS AL 100%)
   ========================================================================== */
.clients-section {
    padding: 8rem 4rem;
    text-align: center;
}

.clients-subtitle {
    font-size: 1.2rem;
    color: var(--text-light);
    font-style: italic;
    margin-top: 0rem;
    margin-bottom: 4rem;
}

.clients-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 2rem;
    max-width: 1200px;
    margin: 0 auto;
    align-items: stretch; /* Fuerza idéntica altura en todas las tarjetas de la fila */
}

.client-card {
    background-color: var(--white);
    padding: 2.5rem 1.5rem;
    border-radius: var(--radius-large);
    box-shadow: var(--shadow-soft);
    border-top: 4px solid var(--primary-deep);
    transition: all 0.3s ease;
    
    /* Configuración flex para nivelar verticalmente los elementos internos */
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    text-align: center;
    height: 100%;
}

.client-card:hover {
    transform: translateY(-5px);
    box-shadow: var(--shadow-heavy);
    border-top-color: var(--secondary-gold);
}

.client-card-link {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    color: inherit;
    text-decoration: none;
}

.client-card-link:focus-visible {
    outline: 3px solid var(--secondary-gold);
    outline-offset: 4px;
}

/* MODIFICACIÓN: Contenedor adaptado para ocupar el área total */
.client-logo-container {
    width: 100%;
    height: 120px;              /* Bloque simétrico con más altura de visualización */
    display: flex;
    align-items: center;        /* Centrado vertical completo del logo */
    justify-content: center;    /* Centrado horizontal completo del logo */
    margin-bottom: 1.5rem;
    padding: 0;                 /* Eliminado el padding para expandirse al área total */
    background-color: transparent;
    overflow: hidden;
}

/* MODIFICACIÓN: Imagen estirada al 100% respetando sus proporciones */
.client-logo {
    width: 100%;
    height: 100%;
    object-fit: contain;        /* Escala al área total disponible sin deformar */
    filter: grayscale(15%);     /* Estética pulida integrada */
    transition: all 0.3s ease;
}

.client-card:hover .client-logo {
    filter: grayscale(0%);
    transform: scale(1.05);
}

.client-card h4 {
    font-size: 1.3rem;
    color: var(--primary-deep);
    margin-top: 0;
    margin-bottom: 0.75rem;
    font-weight: 800;
}

.client-contact {
    font-size: 0.95rem;
    color: var(--text-main);
    line-height: 1.4;
    margin-bottom: 0.75rem;
}

.client-tag {
    display: inline-block;
    font-size: 0.75rem;
    background-color: #f1f5f9;
    color: var(--text-light);
    padding: 0.25rem 0.6rem;
    border-radius: 4px;
    margin: 0.25rem auto 1rem auto;
    font-weight: 600;
    width: max-content;
}

.client-mail {
    font-size: 0.85rem;
    color: #2b6cb0;
    font-weight: 500;
    word-break: break-all;
    margin-top: auto;          /* Empuja el correo al fondo de la tarjeta nivelando la base */
    padding-top: 0.5rem;
}

/* ==========================================================================
   Sección Contacto (Canales Directos Informacionales)
   ========================================================================== */
.contact-section {
    padding: 4rem 4rem 8rem 4rem;
}

.contact-wrapper {
    display: grid;
    grid-template-columns: 0.9fr 1.1fr;
    max-width: 1200px;
    margin: 0 auto;
    background-color: var(--white);
    border-radius: var(--radius-large);
    overflow: hidden;
    box-shadow: var(--shadow-heavy);
}

.contact-info-panel {
    background: linear-gradient(135deg, var(--primary-deep), #1e3a5f);
    color: var(--white);
    padding: 4rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.contact-info-panel h2 {
    font-size: 2.3rem;
    margin-bottom: 1.5rem;
    font-weight: 800;
    line-height: 1.2;
}

.contact-info-panel p {
    color: #cbd5e1;
    margin-bottom: 2.5rem;
}

.contact-details p {
    margin-bottom: 0.8rem;
    font-size: 0.95rem;
}

.contact-channels-panel {
    padding: 4rem;
    background-color: var(--white);
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.badge-channels {
    display: inline-block;
    align-self: flex-start;
    padding: 0.3rem 0.8rem;
    background-color: rgba(10, 37, 64, 0.05);
    color: var(--primary-deep);
    border-radius: 50px;
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 1px;
    margin-bottom: 1rem;
}

.contact-channels-panel h3 {
    font-size: 1.8rem;
    color: var(--primary-deep);
    margin-bottom: 0.5rem;
    font-weight: 800;
}

.channels-intro {
    color: var(--text-light);
    font-size: 0.95rem;
    margin-bottom: 2rem;
}

.channel-box {
    display: flex;
    align-items: center;
    gap: 1.2rem;
    padding: 1.2rem;
    background-color: #f8fafc;
    border-left: 4px solid var(--secondary-gold);
    border-radius: 0 var(--radius-medium) var(--radius-medium) 0;
    margin-bottom: 1.2rem;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.channel-box:hover {
    transform: translateX(5px);
    box-shadow: var(--shadow-soft);
}

.channel-icon {
    font-size: 1.8rem;
}

.channel-data {
    display: flex;
    flex-direction: column;
}

.channel-label {
    font-size: 0.8rem;
    font-weight: 700;
    text-transform: uppercase;
    color: var(--text-light);
    letter-spacing: 0.5px;
    margin-bottom: 0.2rem;
}

.channel-link {
    font-size: 1.1rem;
    font-weight: 600;
    color: var(--primary-deep);
    text-decoration: none;
    transition: color 0.2s ease;
}

.channel-link:hover {
    color: var(--secondary-gold);
}

/* ==========================================================================
   Pie de Página (Footer)
   ========================================================================== */
footer {
    background-color: var(--primary-deep);
    color: var(--white);
    padding: 4rem 2rem;
    text-align: center;
    border-top: 1px solid rgba(255,255,255,0.05);
}

.footer-logo {
    font-size: 1.8rem;
    font-weight: 800;
    margin-bottom: 1rem;
    color: var(--secondary-gold);
}

/* ==========================================================================
   Breakpoints Responsivos (Mobile First / Adaptabilidad)
   ========================================================================== */
@media (max-width: 1024px) {
    .hero-container, .about-grid, .contact-wrapper {
        grid-template-columns: 1fr;
        gap: 3rem;
    }
    .hero-section, .about-section, .services-section, .clients-section, .contact-section {
        padding: 4rem 2rem;
    }
    .hero-image-block {
        height: 380px;
    }
    .main-img { height: 320px; }
    .sub-img { height: 200px; }
    .clients-subtitle {
        margin-top: -1rem;
    }
}

@media (max-width: 768px) {
    .navbar {
        padding: 1.5rem 2rem;
    }
    .menu-toggle {
        display: block;
    }
    .nav-links {
        display: none;
        flex-direction: column;
        position: absolute;
        top: 100%;
        left: 0;
        width: 100%;
        background-color: var(--bg-warm);
        padding: 2rem;
        box-shadow: var(--shadow-heavy);
        gap: 1.5rem;
    }
    .nav-links.active {
        display: flex;
    }
    .hero-text-block h1 {
        font-size: 2.4rem;
    }
    .contact-info-panel, .contact-channels-panel {
        padding: 2rem;
    }
    .channel-link {
        font-size: 1rem;
    }
}

/* Lista de detalles técnicos de clientes */
.client-details-list {
    list-style: none;
    padding: 0;
    margin: 1rem 0 1.5rem 0;
    text-align: left; /* Hace que las viñetas se lean de forma cómoda */
}

.client-details-list li {
    position: relative;
    padding-left: 1.2rem;
    font-size: 0.88rem;
    color: var(--text-light);
    line-height: 1.4;
    margin-bottom: 0.6rem;
}

/* El clásico check o viñeta dorada sutil */
.client-details-list li::before {
    content: "•";
    position: absolute;
    left: 0;
    color: var(--secondary-gold);
    font-weight: bold;
    font-size: 1.1rem;
    top: -2px;
}

/* Justificación unificada para secciones Inicio y Experta */
#inicio .hero-text-block p, 
#experta .about-text p {
    text-align: justify;
    line-height: 1.6;
    margin-bottom: 1.2rem;
    
    /* Esta línea desactiva los guiones de corte */
    -webkit-hyphens: none;
    -ms-hyphens: none;
    hyphens: none;
    
    /* Esto ayuda a que el navegador maneje mejor el espacio entre palabras */
    text-justify: inter-word; 
}

```

