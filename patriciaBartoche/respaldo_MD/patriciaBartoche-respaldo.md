# Bundle del Proyecto para IA: patriciaBartoche

## Estructura de Archivos
- README.md
- atelier.html
- aurora.html
- bosque.html
- css/atelier.css
- css/aurora.css
- css/bosque.css
- css/estilos.css
- css/instalaciones.css
- css/luz.css
- css/mineral.css
- index.html
- js/configuracion.js
- js/principal.js
- js/selector.js
- luz.html
- mineral.html

---

## Código Fuente

### Archivo: `README.md`
```md
# Patricia Bertoche · Centro de masajes y relajación

Sitio estático (HTML, CSS y JS, sin dependencias) con 6 diseños intercambiables. Todo el contenido se edita en un solo archivo y las fotos se leen desde Cloudinary.

## Estructura

```
centro-relax/
├── index.html      Diseño 1 · Serenidad
├── atelier.html    Diseño 2 · Atelier
├── luz.html        Diseño 3 · Luz
├── bosque.html     Diseño 4 · Bosque
├── aurora.html     Diseño 5 · Aurora
├── mineral.html    Diseño 6 · Mineral
├── css/            Un archivo de estilos por diseño (estilos.css es el de Serenidad)
│                   + instalaciones.css (compartido por los 6)
└── js/
    ├── configuracion.js   Todos los datos editables
    ├── principal.js       Arma la página y conecta Cloudinary y WhatsApp
    └── selector.js        Barra para cambiar de diseño
```

Los 6 diseños usan el mismo contenido y los mismos scripts. Solo cambian el HTML (orden de secciones) y el CSS.

## Cómo editar el contenido

Todo se cambia en `js/configuracion.js`:

- **Contacto:** `contacto.whatsapp` (código de país + número, sin `+` ni espacios), `contacto.instagram` y `contacto.usuarioInstagram`.
- **Textos:** nombre del negocio, inicio, "Sobre mí", pasos de la visita, opiniones y formulario.
- **Servicios:** nombre, duración, precio y detalle. Al agregar o quitar uno, se actualiza también el formulario de reserva.
- **Avisos de ejemplo:** `mostrarNotas: false` oculta las notas "de ejemplo".

Los precios, testimonios y nombres actuales son de ejemplo: hay que reemplazarlos por los reales.

## Fotos (Cloudinary)

El sitio solo lee: no necesita `UPLOAD_PRESET`. Usa la cuenta `p0qlmlor` y busca las fotos **por etiqueta**, con el formato `proyecto_carpeta`:

| Carpeta en Cloudinary | Etiqueta | Se muestra en |
|---|---|---|
| `patriciabertoche/galeria` | `patriciabertoche_galeria` | Sección Galería |
| `patriciabertoche/imagenes` | `patriciabertoche_imagenes` | Foto de "Sobre mí" |
| `patriciabertoche/instalaciones` | `patriciabertoche_instalaciones` | Sección Instalaciones |

- La foto de "Sobre mí" es la que tenga `patricia` en el nombre (`cloudinary.palabraRetrato`); si no hay, se usa la primera.
- Alternativa directa: completar `cloudinary.fotoRetrato` con el ID público de la imagen.
- El título de cada foto de la galería sale del campo `caption` de Cloudinary.
- Mientras una carpeta no tenga fotos, la vista previa muestra en qué carpeta y con qué etiqueta subirlas (`vistasPreviasConCarpeta: false` lo desactiva).

Requisitos en Cloudinary:

1. Cada foto debe tener su etiqueta, escrita exactamente igual (ojo: `bertoche`, no `bartoche`).
2. Tiene que estar activada la opción **Resource list** en Settings > Security.
3. Cloudinary guarda la lista en caché unos minutos: si acabás de subir o etiquetar, esperá y recargá con Ctrl+F5.

Para comprobar una etiqueta, abrir en el navegador `https://res.cloudinary.com/p0qlmlor/image/list/<etiqueta>.json`. Si da 404, esa etiqueta no tiene fotos. El error rojo en la consola es normal mientras eso pase.

## Mostrar varios diseños a la clienta

Con `mostrarSelectorDisenos: true` aparece una barra flotante abajo para cambiar entre los diseños. La lista está en `disenos` (nombre y archivo).

Cuando la clienta elija:

1. Poner `mostrarSelectorDisenos: false`.
2. Renombrar el archivo elegido como `index.html` (y borrar los diseños que no se usen, con sus CSS).

## Publicar

Es un sitio estático: subir la carpeta completa a Netlify, GitHub Pages o cualquier hosting. Para probarlo en local, abrir `index.html` en el navegador.
```

### Archivo: `atelier.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Masajes y relajación</title>
<meta name="description" content="Centro estético de masajes y relajación. Reservá tu momento de calma.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Italiana&family=Jost:wght@300;400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="css/atelier.css">
<link rel="stylesheet" href="css/instalaciones.css">
</head>
<body>

<header class="cabecera">
  <a href="#inicio" class="marca" data-texto="negocio.nombre"></a>
  <button class="menu-boton" aria-label="Abrir menú" aria-expanded="false">Menú</button>
  <nav class="menu" id="menu">
    <a href="#servicios">Servicios</a>
    <a href="#galeria">Galería</a>
    <a href="#instalaciones">Instalaciones</a>
    <a href="#sobre-mi">Sobre mí</a>
    <a href="#cuidados">Tu visita</a>
    <a href="#contacto" class="boton-mini">Reservar</a>
  </nav>
</header>

<main>
  <section class="hero" id="inicio">
    <div class="respiro" aria-hidden="true"><span id="respiro-texto"></span></div>
    <div class="hero-texto">
      <h1 data-texto="inicio.titulo"></h1>
      <p data-texto="inicio.texto"></p>
      <div class="acciones">
        <a href="#contacto" class="boton" data-texto="inicio.botonReserva"></a>
        <a href="#servicios" class="enlace" data-texto="inicio.botonServicios"></a>
      </div>
    </div>
  </section>

  <section class="seccion oscura" id="sobre-mi">
    <div class="dos-col">
      <div class="retrato" role="img"><span data-texto="sobreMi.textoFoto"></span></div>
      <div>
        <h2 data-texto="sobreMi.titulo"></h2>
        <div id="parrafos-sobre-mi"></div>
        <a class="boton claro" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="sobreMi.botonInstagram"></a>
      </div>
    </div>
  </section>

  <section class="seccion" id="servicios">
    <h2 data-texto="servicios.titulo"></h2>
    <p class="intro" data-texto="servicios.intro"></p>
    <div class="servicios" id="lista-servicios"></div>
    <p class="nota" data-texto="servicios.nota"></p>
  </section>

  <section class="seccion" id="galeria">
    <h2 data-texto="galeria.titulo"></h2>
    <p class="intro" data-texto="galeria.intro"></p>
    <div class="galeria" id="lista-galeria"></div>
    <a class="boton" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="galeria.boton"></a>
    <p class="nota" data-texto="galeria.nota"></p>
  </section>

  <section class="seccion" id="cuidados">
    <h2 data-texto="visita.titulo"></h2>
    <ol class="pasos" id="lista-pasos"></ol>
  </section>

  <section class="seccion" id="instalaciones">
    <h2 data-texto="instalaciones.titulo"></h2>
    <p class="intro" data-texto="instalaciones.intro"></p>
    <div class="instalaciones mosaico" id="lista-instalaciones"></div>
    <p class="nota" data-texto="instalaciones.nota"></p>
  </section>

  <section class="seccion" id="opiniones">
    <h2 data-texto="opiniones.titulo"></h2>
    <div class="opiniones" id="lista-opiniones"></div>
    <p class="nota" data-texto="opiniones.nota"></p>
  </section>

  <section class="seccion" id="contacto">
    <h2 data-texto="reserva.titulo"></h2>
    <p class="intro" data-texto="reserva.intro"></p>
    <form id="formulario" class="formulario" novalidate>
      <label><span data-texto="reserva.etiquetaNombre"></span>
        <input type="text" name="nombre" autocomplete="name" required>
      </label>
      <label><span data-texto="reserva.etiquetaServicio"></span>
        <select name="servicio" id="campo-servicio" required></select>
      </label>
      <label><span data-texto="reserva.etiquetaHorario"></span>
        <input type="text" name="horario" id="campo-horario">
      </label>
      <p class="error" id="error" role="alert" hidden></p>
      <button type="submit" class="boton" data-texto="reserva.boton"></button>
    </form>
  </section>

</main>

<footer class="pie">
  <p data-texto="negocio.pie"></p>
  <a target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="contacto.usuarioInstagram"></a>
</footer>

<script src="js/configuracion.js"></script>
<script src="js/principal.js"></script>
<script src="js/selector.js"></script>
</body>
</html>

```

### Archivo: `aurora.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Masajes y relajación</title>
<meta name="description" content="Centro estético de masajes y relajación. Reservá tu momento de calma.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="css/aurora.css">
<link rel="stylesheet" href="css/instalaciones.css">
</head>
<body>

<a class="franja" target="_blank" rel="noopener" data-whatsapp data-texto="inicio.botonReserva"></a>
<header class="cabecera">
  <a href="#inicio" class="marca" data-texto="negocio.nombre"></a>
  <button class="menu-boton" aria-label="Abrir menú" aria-expanded="false">Menú</button>
  <nav class="menu" id="menu">
    <a href="#servicios">Servicios</a>
    <a href="#galeria">Galería</a>
    <a href="#instalaciones">Instalaciones</a>
    <a href="#sobre-mi">Sobre mí</a>
    <a href="#cuidados">Tu visita</a>
    <a href="#contacto" class="boton-mini">Reservar</a>
  </nav>
</header>

<main>
  <section class="hero" id="inicio">
    <div class="respiro" aria-hidden="true"><span id="respiro-texto"></span></div>
    <div class="hero-texto">
      <h1 data-texto="inicio.titulo"></h1>
      <p data-texto="inicio.texto"></p>
      <div class="acciones">
        <a href="#contacto" class="boton" data-texto="inicio.botonReserva"></a>
        <a href="#servicios" class="enlace" data-texto="inicio.botonServicios"></a>
      </div>
    </div>
  </section>

  <section class="seccion" id="servicios">
    <h2 data-texto="servicios.titulo"></h2>
    <p class="intro" data-texto="servicios.intro"></p>
    <div class="servicios" id="lista-servicios"></div>
    <p class="nota" data-texto="servicios.nota"></p>
  </section>

  <section class="seccion" id="opiniones">
    <h2 data-texto="opiniones.titulo"></h2>
    <div class="opiniones" id="lista-opiniones"></div>
    <p class="nota" data-texto="opiniones.nota"></p>
  </section>

  <section class="seccion" id="galeria">
    <h2 data-texto="galeria.titulo"></h2>
    <p class="intro" data-texto="galeria.intro"></p>
    <div class="galeria" id="lista-galeria"></div>
    <a class="boton" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="galeria.boton"></a>
    <p class="nota" data-texto="galeria.nota"></p>
  </section>

  <section class="seccion oscura" id="sobre-mi">
    <div class="dos-col">
      <div class="retrato" role="img"><span data-texto="sobreMi.textoFoto"></span></div>
      <div>
        <h2 data-texto="sobreMi.titulo"></h2>
        <div id="parrafos-sobre-mi"></div>
        <a class="boton claro" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="sobreMi.botonInstagram"></a>
      </div>
    </div>
  </section>

  <section class="seccion" id="cuidados">
    <h2 data-texto="visita.titulo"></h2>
    <ol class="pasos" id="lista-pasos"></ol>
  </section>

  <section class="seccion" id="instalaciones">
    <h2 data-texto="instalaciones.titulo"></h2>
    <p class="intro" data-texto="instalaciones.intro"></p>
    <div class="instalaciones mosaico" id="lista-instalaciones"></div>
    <p class="nota" data-texto="instalaciones.nota"></p>
  </section>

  <section class="seccion" id="contacto">
    <h2 data-texto="reserva.titulo"></h2>
    <p class="intro" data-texto="reserva.intro"></p>
    <form id="formulario" class="formulario" novalidate>
      <label><span data-texto="reserva.etiquetaNombre"></span>
        <input type="text" name="nombre" autocomplete="name" required>
      </label>
      <label><span data-texto="reserva.etiquetaServicio"></span>
        <select name="servicio" id="campo-servicio" required></select>
      </label>
      <label><span data-texto="reserva.etiquetaHorario"></span>
        <input type="text" name="horario" id="campo-horario">
      </label>
      <p class="error" id="error" role="alert" hidden></p>
      <button type="submit" class="boton" data-texto="reserva.boton"></button>
    </form>
  </section>

</main>

<footer class="pie">
  <p data-texto="negocio.pie"></p>
  <a target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="contacto.usuarioInstagram"></a>
</footer>

<script src="js/configuracion.js"></script>
<script src="js/principal.js"></script>
<script src="js/selector.js"></script>
</body>
</html>

```

### Archivo: `bosque.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Masajes y relajación</title>
<meta name="description" content="Centro estético de masajes y relajación. Reservá tu momento de calma.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Young+Serif&family=Work+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="css/bosque.css">
<link rel="stylesheet" href="css/instalaciones.css">
</head>
<body>

<header class="cabecera">
  <a href="#inicio" class="marca" data-texto="negocio.nombre"></a>
  <button class="menu-boton" aria-label="Abrir menú" aria-expanded="false">Menú</button>
  <nav class="menu" id="menu">
    <a href="#servicios">Servicios</a>
    <a href="#galeria">Galería</a>
    <a href="#instalaciones">Instalaciones</a>
    <a href="#sobre-mi">Sobre mí</a>
    <a href="#cuidados">Tu visita</a>
    <a href="#contacto" class="boton-mini">Reservar</a>
  </nav>
</header>

<main>
  <section class="hero" id="inicio">
    <div class="respiro" aria-hidden="true"><span id="respiro-texto"></span></div>
    <div class="hero-texto">
      <h1 data-texto="inicio.titulo"></h1>
      <p data-texto="inicio.texto"></p>
      <div class="acciones">
        <a href="#contacto" class="boton" data-texto="inicio.botonReserva"></a>
        <a href="#servicios" class="enlace" data-texto="inicio.botonServicios"></a>
      </div>
    </div>
  </section>

  <section class="seccion oscura" id="sobre-mi">
    <div class="dos-col">
      <div class="retrato" role="img"><span data-texto="sobreMi.textoFoto"></span></div>
      <div>
        <h2 data-texto="sobreMi.titulo"></h2>
        <div id="parrafos-sobre-mi"></div>
        <a class="boton claro" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="sobreMi.botonInstagram"></a>
      </div>
    </div>
  </section>

  <section class="seccion" id="galeria">
    <h2 data-texto="galeria.titulo"></h2>
    <p class="intro" data-texto="galeria.intro"></p>
    <div class="galeria" id="lista-galeria"></div>
    <a class="boton" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="galeria.boton"></a>
    <p class="nota" data-texto="galeria.nota"></p>
  </section>

  <section class="seccion" id="servicios">
    <h2 data-texto="servicios.titulo"></h2>
    <p class="intro" data-texto="servicios.intro"></p>
    <div class="servicios" id="lista-servicios"></div>
    <p class="nota" data-texto="servicios.nota"></p>
  </section>

  <section class="seccion" id="cuidados">
    <h2 data-texto="visita.titulo"></h2>
    <ol class="pasos" id="lista-pasos"></ol>
  </section>

  <section class="seccion" id="instalaciones">
    <h2 data-texto="instalaciones.titulo"></h2>
    <p class="intro" data-texto="instalaciones.intro"></p>
    <div class="instalaciones mosaico" id="lista-instalaciones"></div>
    <p class="nota" data-texto="instalaciones.nota"></p>
  </section>

  <section class="seccion" id="opiniones">
    <h2 data-texto="opiniones.titulo"></h2>
    <div class="opiniones" id="lista-opiniones"></div>
    <p class="nota" data-texto="opiniones.nota"></p>
  </section>

  <section class="seccion" id="contacto">
    <h2 data-texto="reserva.titulo"></h2>
    <p class="intro" data-texto="reserva.intro"></p>
    <form id="formulario" class="formulario" novalidate>
      <label><span data-texto="reserva.etiquetaNombre"></span>
        <input type="text" name="nombre" autocomplete="name" required>
      </label>
      <label><span data-texto="reserva.etiquetaServicio"></span>
        <select name="servicio" id="campo-servicio" required></select>
      </label>
      <label><span data-texto="reserva.etiquetaHorario"></span>
        <input type="text" name="horario" id="campo-horario">
      </label>
      <p class="error" id="error" role="alert" hidden></p>
      <button type="submit" class="boton" data-texto="reserva.boton"></button>
    </form>
  </section>

</main>

<footer class="pie">
  <p data-texto="negocio.pie"></p>
  <a target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="contacto.usuarioInstagram"></a>
</footer>

<script src="js/configuracion.js"></script>
<script src="js/principal.js"></script>
<script src="js/selector.js"></script>
</body>
</html>

```

### Archivo: `css/atelier.css`
```css
/* Diseño Atelier: oscuro y elegante, barra lateral, servicios en filas, galería en tira */
:root{--fondo:#221726;--panel:#2E2033;--linea:#4a3850;--texto:#F1E8E4;--suave:#B9A7B4;--oro:#D6B48F;--rosa:#C98B9A;--titulo:"Italiana",Georgia,serif;--cuerpo:"Jost",system-ui,sans-serif;box-sizing:border-box;padding-top:env(safe-area-inset-top,0)}
*,*::before,*::after{box-sizing:inherit}
html{scroll-behavior:smooth}
body{margin:0;background:var(--fondo);color:var(--texto);font:300 1.05rem/1.7 var(--cuerpo)}
h1,h2{font-family:var(--titulo);font-weight:400;line-height:1.1;margin:0 0 .6em}
h1{font-size:clamp(2.6rem,7vw,5rem)}
h2{font-size:clamp(2rem,4vw,3rem);color:var(--oro)}
a{color:inherit}
:focus-visible{outline:2px solid var(--oro);outline-offset:3px}
.cabecera{position:fixed;inset:0 auto 0 0;width:240px;display:flex;flex-direction:column;justify-content:space-between;padding:2.5rem 1.8rem;background:var(--fondo);border-right:1px solid var(--linea);z-index:10}
.marca{font-family:var(--titulo);font-size:1.7rem;line-height:1.2;color:var(--oro);text-decoration:none}
.menu{display:flex;flex-direction:column;gap:1rem}
.menu a{color:var(--suave);text-decoration:none}
.menu a:hover{color:var(--oro)}
.menu-boton{display:none}
main,.pie{margin-left:240px}
.boton,.boton-mini{display:inline-block;background:transparent;color:var(--oro)!important;border:1px solid var(--oro);padding:.8rem 1.8rem;font:400 1rem var(--cuerpo);text-decoration:none;cursor:pointer;transition:.2s}
.boton:hover,.boton-mini:hover,.boton.claro{background:var(--oro);color:var(--fondo)!important}
.boton-mini{padding:.3rem 1rem;text-align:center}
.enlace{margin-left:1.5rem}
.hero{display:grid;grid-template-columns:1.2fr 1fr;align-items:center;gap:3rem;min-height:100vh;padding:4rem 6vw}
.hero-texto{order:1}
.respiro{order:2;justify-self:center;width:min(34vw,360px);aspect-ratio:1;border-radius:50%;border:1px solid var(--oro);background:radial-gradient(circle,rgba(201,139,154,.35),transparent 70%);display:grid;place-items:center;animation:respirar 10s ease-in-out infinite}
.respiro span{font-family:var(--titulo);font-size:1.6rem;color:var(--oro)}
@keyframes respirar{0%,100%{transform:scale(.8)}50%{transform:scale(1)}}
.hero-texto p{max-width:36ch;color:var(--suave);font-size:1.2rem}
.acciones{margin-top:2rem}
.seccion{padding:6rem 6vw}
.seccion>*{max-width:880px}
.intro{color:var(--suave);margin-bottom:2rem}
.nota{font-size:.85rem;color:var(--rosa)}
.servicios{display:block}
details{border-bottom:1px solid var(--linea);padding:1.2rem 0}
summary{display:flex;justify-content:space-between;gap:1rem;font-family:var(--titulo);font-size:1.6rem;cursor:pointer;list-style:none}
summary::-webkit-details-marker{display:none}
summary small{font:300 .95rem var(--cuerpo);color:var(--oro);align-self:center;white-space:nowrap}
details p{margin:.6rem 0 0;color:var(--suave);max-width:60ch}
.galeria{display:flex;gap:1rem;overflow-x:auto;scroll-snap-type:x mandatory;padding-bottom:1rem;margin-bottom:1.5rem;max-width:none}
.foto{position:relative;overflow:hidden;flex:0 0 min(72%,290px);aspect-ratio:3/4;scroll-snap-align:start;border-radius:150px 150px 8px 8px;display:flex;align-items:flex-end;justify-content:center;padding:1.2rem;text-align:center}
.f1{background:linear-gradient(160deg,#5b3a5e,#2E2033)}.f2{background:linear-gradient(160deg,#C98B9A,#7a4a5a)}.f3{background:linear-gradient(160deg,#D6B48F,#8a6a4a)}.f4{background:linear-gradient(160deg,#7a5a80,#3b2a42)}.f5{background:linear-gradient(160deg,#b07a8a,#5b3a5e)}
.oscura{background:var(--panel)}
.oscura>.dos-col{max-width:1000px}
.dos-col{display:grid;grid-template-columns:1fr 1.3fr;gap:4rem;align-items:center}
.retrato{position:relative;overflow:hidden;aspect-ratio:3/4;border-radius:200px 200px 8px 8px;background:linear-gradient(160deg,var(--rosa),#5b3a5e);display:grid;place-items:center}
.pasos{list-style:none;padding:0;margin:0;display:grid;grid-template-columns:repeat(4,1fr);gap:2rem;counter-reset:p;max-width:1000px}
.pasos li{counter-increment:p;border-top:1px solid var(--oro);padding-top:1rem;color:var(--suave)}
.pasos li::before{content:counter(p);display:block;font:italic 1.6rem var(--titulo);color:var(--rosa)}
.pasos strong{display:block;font:400 1.3rem var(--titulo);color:var(--texto)}
.opiniones{display:grid;gap:2rem}
blockquote{margin:0;padding-left:1.5rem;border-left:2px solid var(--rosa)}
blockquote p{margin:0 0 .5rem;font:400 1.6rem/1.35 var(--titulo)}
cite{font-style:normal;color:var(--suave)}
.formulario{display:grid;gap:1.1rem;max-width:520px}
label{display:grid;gap:.35rem}
input,select{font:inherit;padding:.8rem 1rem;background:var(--panel);color:var(--texto);border:1px solid var(--linea)}
.error{color:#f0a0b0;margin:0}
.pie{display:flex;justify-content:space-between;flex-wrap:wrap;gap:.5rem;padding:2rem 6vw calc(2rem + env(safe-area-inset-bottom,0));border-top:1px solid var(--linea);color:var(--suave)}
.pie p{margin:0}
.con-imagen img{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.foto.con-imagen::after{content:"";position:absolute;inset:auto 0 0;height:40%;background:linear-gradient(transparent,rgba(34,23,38,.75))}
.foto.con-imagen span{position:relative;z-index:2}
.vista-previa{display:grid;gap:.15rem;overflow-wrap:anywhere;padding:1rem;text-align:center}
.vista-previa small{font-size:.8rem;opacity:.85}
@media (max-width:900px){
  .cabecera{position:sticky;top:0;inset:auto;width:100%;flex-direction:row;align-items:center;padding:.8rem 5vw;border-right:0;border-bottom:1px solid var(--linea)}
  .menu-boton{display:block;background:none;color:var(--oro);border:1px solid var(--oro);padding:.3rem 1rem;font:inherit}
  .menu{display:none;position:absolute;top:100%;left:0;right:0;background:var(--fondo);padding:1.2rem 5vw}
  .menu.abierto{display:flex}
  main,.pie{margin-left:0}
  .hero,.dos-col{grid-template-columns:1fr}
  .hero{min-height:auto}
  .respiro{width:min(60vw,260px)}
  .pasos{grid-template-columns:1fr 1fr}
  .retrato{max-width:280px}
}
@media (max-width:520px){.pasos{grid-template-columns:1fr}}
@media (prefers-reduced-motion:reduce){.respiro{animation:none}html{scroll-behavior:auto}}

```

### Archivo: `css/aurora.css`
```css
/* Base compartida */
*,*::before,*::after{box-sizing:inherit}
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0)}
html{scroll-behavior:smooth;scroll-padding-top:90px}
body{margin:0}
a{color:inherit}
:focus-visible{outline:3px solid var(--acento);outline-offset:3px}
details{transition:background .25s}
summary{cursor:pointer;list-style:none}
summary::-webkit-details-marker{display:none}
.foto,.retrato{position:relative;overflow:hidden}
.foto{display:flex;align-items:flex-end;padding:1rem}
.retrato{display:grid;place-items:center}
.con-imagen img{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.foto.con-imagen span{position:relative;z-index:2;color:#fff;text-shadow:0 1px 6px rgba(0,0,0,.6)}
.foto.con-imagen::after{content:"";position:absolute;inset:auto 0 0;height:40%;background:linear-gradient(transparent,rgba(0,0,0,.45))}
.vista-previa{display:grid;gap:.15rem;overflow-wrap:anywhere;padding:.5rem;text-align:center}
.foto .vista-previa{text-align:left;padding:0}
.vista-previa small{font-size:.8rem;opacity:.85}
.menu-boton{display:none}
@media (max-width:800px){
  .menu-boton{display:block;background:none;color:inherit;border:1px solid currentColor;border-radius:99px;padding:.3rem 1rem;font:inherit}
  .menu{display:none!important;position:absolute;top:100%;left:0;right:0;flex-direction:column;align-items:flex-start;gap:1rem;padding:1.2rem 5vw;background:var(--menu-fondo,var(--fondo))}
  .menu.abierto{display:flex!important}
}
@media (prefers-reduced-motion:reduce){*{animation:none!important}html{scroll-behavior:auto}}
@keyframes respirar{0%,100%{transform:scale(.8)}50%{transform:scale(1)}}
/* Diseño Aurora: lavanda y durazno, cabecera flotante, paneles redondeados, servicios en carrusel */
:root{--fondo:#F1EDFF;--tinta:#2E2560;--violeta:#5B49C9;--durazno:#FFB8A0;--menta:#BDF0DE;--acento:#5B49C9;--menu-fondo:#fff;--fuente:"Outfit",system-ui,sans-serif}
body{background:var(--fondo);color:var(--tinta);font:400 1.05rem/1.65 var(--fuente)}
h1,h2{font-weight:600;line-height:1.08;margin:0 0 .6em;letter-spacing:-.01em}
h1{font-size:clamp(2.4rem,6vw,4.4rem)}
h2{font-size:clamp(1.8rem,4vw,2.6rem)}
.franja{display:block;text-align:center;background:var(--tinta);color:#fff;padding:.5rem;font-size:.9rem;text-decoration:none}
.cabecera{position:sticky;top:.8rem;z-index:10;margin:.8rem 4vw 0;padding:.55rem 1.2rem;border-radius:99px;background:rgba(255,255,255,.88);backdrop-filter:blur(10px);display:flex;justify-content:space-between;align-items:center;box-shadow:0 6px 24px rgba(91,73,201,.15)}
.marca{font-weight:600;font-size:1.15rem;text-decoration:none}
.menu{display:flex;gap:1.3rem;align-items:center}
.menu a{text-decoration:none;font-size:.95rem}
.menu a:hover{color:var(--violeta)}
.boton,.boton-mini{display:inline-block;background:var(--violeta);color:#fff!important;border:0;border-radius:99px;padding:.85rem 1.8rem;font:500 1rem var(--fuente);text-decoration:none;cursor:pointer;transition:.2s}
.boton:hover,.boton-mini:hover{background:var(--durazno);color:var(--tinta)!important}
.boton-mini{padding:.3rem 1rem}
.boton.claro{background:var(--durazno);color:var(--tinta)!important}
.enlace{margin-left:1.2rem}
.hero{display:grid;grid-template-columns:2fr 1fr;gap:1rem;align-items:center;margin:1rem 4vw;padding:3rem clamp(1.4rem,4vw,3.5rem);border-radius:36px;background:var(--tinta);color:#fff;min-height:70vh}
.hero-texto{order:1}
.hero-texto p{max-width:38ch;font-size:1.2rem;opacity:.9}
.acciones{margin-top:1.8rem}
.respiro{order:2;justify-self:center;width:min(28vw,280px);aspect-ratio:1;border-radius:50%;background:radial-gradient(circle at 35% 30%,#fff,var(--menta) 45%,var(--durazno));display:grid;place-items:center;animation:respirar 10s ease-in-out infinite}
.respiro span{font-weight:600;color:var(--tinta)}
.seccion{margin:1rem 4vw;padding:3rem clamp(1.2rem,4vw,3rem);max-width:none;background:#fff;border-radius:36px}
.intro{max-width:56ch;margin-bottom:1.6rem}
.nota{font-size:.85rem;opacity:.7}
.servicios{display:flex;gap:1rem;overflow-x:auto;scroll-snap-type:x mandatory;padding-bottom:1rem;align-items:flex-start}
details{flex:0 0 min(78%,300px);scroll-snap-align:start;background:var(--fondo);border:2px solid transparent;border-radius:28px;padding:1.4rem}
details[open]{background:var(--menta);border-color:var(--violeta)}
summary{display:grid;gap:.5rem;justify-items:start;font-size:1.25rem;font-weight:600}
summary small{font-size:.85rem;font-weight:500;background:var(--durazno);border-radius:99px;padding:.15rem .8rem}
details p{margin:.8rem 0 0}
.galeria{display:grid;grid-template-columns:repeat(3,1fr);gap:.8rem;margin-bottom:1.6rem}
.foto{aspect-ratio:1;border-radius:28px;color:var(--tinta)}
.f1{background:linear-gradient(150deg,var(--menta),#7fd3b8)}.f2{background:linear-gradient(150deg,var(--durazno),#ff8f6e)}.f3{background:linear-gradient(150deg,#cfc6ff,var(--violeta))}.f4{background:linear-gradient(150deg,#fff,var(--menta))}.f5{background:linear-gradient(150deg,var(--durazno),#cfc6ff)}
.oscura{background:var(--tinta);color:#fff}
.dos-col{display:grid;grid-template-columns:auto 1fr;gap:3rem;align-items:center}
.retrato{width:min(300px,70vw);aspect-ratio:4/5;border-radius:36px;background:linear-gradient(160deg,var(--durazno),var(--violeta));color:#fff}
.pasos{list-style:none;padding:0;margin:0;display:grid;grid-template-columns:repeat(4,1fr);gap:1rem;counter-reset:p}
.pasos li{counter-increment:p;background:var(--fondo);border-radius:24px;padding:1.2rem}
.pasos li::before{content:counter(p);display:grid;place-items:center;width:2.4rem;height:2.4rem;border-radius:50%;background:var(--durazno);font-weight:600;margin-bottom:.6rem}
.pasos strong{display:block;font-size:1.1rem}
.opiniones{display:grid;grid-template-columns:repeat(3,1fr);gap:1rem}
blockquote{margin:0;background:var(--fondo);border-radius:28px;padding:1.5rem}
blockquote p{margin:0 0 1rem;font-size:1.1rem}
cite{font-style:normal;font-weight:600;background:var(--menta);border-radius:99px;padding:.15rem .8rem;font-size:.9rem}
.formulario{display:grid;gap:1.1rem;max-width:520px}
label{display:grid;gap:.35rem;font-weight:500}
input,select{font:inherit;padding:.8rem 1.2rem;border:2px solid #d9d2fb;border-radius:99px;background:#fff;color:var(--tinta)}
.error{color:#b3264f;margin:0}
.pie{display:flex;justify-content:center;flex-wrap:wrap;gap:1.5rem;padding:2rem 4vw calc(5rem + env(safe-area-inset-bottom,0))}
.pie p{margin:0}
@media (max-width:800px){
  .hero,.dos-col,.opiniones{grid-template-columns:1fr}
  .hero{min-height:auto}
  .respiro{width:min(55vw,220px)}
  .pasos{grid-template-columns:1fr 1fr}
  .galeria{grid-template-columns:1fr 1fr}
  .retrato{margin:0 auto}
}
@media (max-width:520px){.pasos{grid-template-columns:1fr}}

```

### Archivo: `css/bosque.css`
```css
/* Base compartida */
*,*::before,*::after{box-sizing:inherit}
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0)}
html{scroll-behavior:smooth;scroll-padding-top:90px}
body{margin:0}
a{color:inherit}
:focus-visible{outline:3px solid var(--acento);outline-offset:3px}
details{transition:background .25s}
summary{cursor:pointer;list-style:none}
summary::-webkit-details-marker{display:none}
.foto,.retrato{position:relative;overflow:hidden}
.foto{display:flex;align-items:flex-end;padding:1rem}
.retrato{display:grid;place-items:center}
.con-imagen img{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.foto.con-imagen span{position:relative;z-index:2;color:#fff;text-shadow:0 1px 6px rgba(0,0,0,.6)}
.foto.con-imagen::after{content:"";position:absolute;inset:auto 0 0;height:40%;background:linear-gradient(transparent,rgba(0,0,0,.45))}
.vista-previa{display:grid;gap:.15rem;overflow-wrap:anywhere;padding:.5rem;text-align:center}
.foto .vista-previa{text-align:left;padding:0}
.vista-previa small{font-size:.8rem;opacity:.85}
.menu-boton{display:none}
@media (max-width:800px){
  .menu-boton{display:block;background:none;color:inherit;border:1px solid currentColor;border-radius:99px;padding:.3rem 1rem;font:inherit}
  .menu{display:none!important;position:absolute;top:100%;left:0;right:0;flex-direction:column;align-items:flex-start;gap:1rem;padding:1.2rem 5vw;background:var(--menu-fondo,var(--fondo))}
  .menu.abierto{display:flex!important}
}
@media (prefers-reduced-motion:reduce){*{animation:none!important}html{scroll-behavior:auto}}
@keyframes respirar{0%,100%{transform:scale(.8)}50%{transform:scale(1)}}
/* Diseño Bosque: verde musgo y ámbar, cabecera sólida, título enorme, galería en bloques y línea de tiempo */
:root{--fondo:#F0F3EA;--tinta:#22372B;--musgo:#2F4A3A;--ambar:#D08C3C;--corteza:#5E4634;--acento:#D08C3C;--menu-fondo:var(--musgo);--titulo:"Young Serif",Georgia,serif;--cuerpo:"Work Sans",system-ui,sans-serif}
body{background:var(--fondo);color:var(--tinta);font:400 1.05rem/1.65 var(--cuerpo)}
h1,h2{font-family:var(--titulo);font-weight:400;line-height:1.05;margin:0 0 .6em}
h2{font-size:clamp(2rem,4vw,2.8rem)}
h2::after{content:"";display:block;width:56px;height:4px;background:var(--ambar);margin-top:.6rem}
.cabecera{position:sticky;top:0;z-index:10;display:flex;justify-content:space-between;align-items:center;padding:.9rem 5vw;background:var(--musgo);color:#F0F3EA}
.marca{font-family:var(--titulo);font-size:1.35rem;text-decoration:none}
.menu{display:flex;gap:1.6rem;align-items:center}
.menu a{text-decoration:none}
.menu a:hover{color:var(--ambar)}
.boton,.boton-mini{display:inline-block;background:var(--ambar);color:#1c2a20!important;border:0;padding:.85rem 1.8rem;font:600 1rem var(--cuerpo);text-decoration:none;cursor:pointer;transition:.2s}
.boton:hover,.boton-mini:hover{background:var(--corteza);color:#fff!important}
.boton-mini{padding:.35rem 1rem}
.boton.claro{background:var(--ambar)}
.enlace{margin-left:1.4rem}
.hero{position:relative;overflow:hidden;min-height:88vh;display:grid;align-items:end;padding:5rem 5vw;background:linear-gradient(180deg,#3d5e49,var(--musgo));color:#F0F3EA}
.hero-texto{position:relative;z-index:1}
.hero h1{font-size:clamp(3rem,10vw,7.5rem);line-height:.95;max-width:11ch}
.hero-texto p{max-width:38ch;font-size:1.25rem}
.acciones{margin-top:2rem}
.respiro{position:absolute;right:-8%;top:6%;width:min(52vw,500px);aspect-ratio:1;border-radius:50%;background:radial-gradient(circle at 35% 30%,#86a894,#3d5e49);display:grid;place-items:center;animation:respirar 10s ease-in-out infinite}
.respiro span{font-family:var(--titulo);font-size:1.6rem;color:#F0F3EA}
.seccion{padding:5rem 5vw;max-width:1200px;margin:0 auto}
.intro{max-width:56ch;margin-bottom:2rem}
.nota{font-size:.85rem;color:var(--corteza)}
.servicios{display:grid;grid-template-columns:1fr 1fr;gap:1rem;align-items:start}
details{background:#fff;border-left:6px solid var(--ambar);padding:1.2rem 1.4rem}
details[open]{background:#e4ead8}
summary{display:flex;justify-content:space-between;gap:1rem;font-family:var(--titulo);font-size:1.25rem}
summary small{font:500 .9rem var(--cuerpo);color:var(--corteza);align-self:center;white-space:nowrap}
details p{margin:.7rem 0 0}
.galeria{display:grid;grid-template-columns:repeat(4,1fr);grid-auto-rows:150px;gap:.8rem;margin-bottom:1.6rem}
.f1{grid-column:span 2;grid-row:span 2;background:linear-gradient(150deg,#5f8570,var(--musgo))}
.f2{background:linear-gradient(150deg,var(--ambar),#a86a22)}
.f3{grid-row:span 2;background:linear-gradient(150deg,#86a894,#3d5e49)}
.f4{background:linear-gradient(150deg,#8a6a52,var(--corteza))}
.f5{grid-column:span 4;background:linear-gradient(90deg,var(--musgo),#5f8570)}
.foto{color:#fff}
.oscura{max-width:none;background:var(--corteza);color:#F0F3EA}
.oscura .dos-col{max-width:1100px;margin:0 auto}
.dos-col{display:grid;grid-template-columns:1fr 1.2fr;gap:4rem;align-items:center}
.retrato{aspect-ratio:4/5;border-radius:0 140px 0 0;background:linear-gradient(160deg,var(--ambar),var(--musgo));color:#fff}
.pasos{list-style:none;margin:0;padding:0 0 0 2rem;border-left:2px solid var(--ambar);display:grid;gap:1.6rem;max-width:640px}
.pasos li{position:relative}
.pasos li::before{content:"";position:absolute;left:calc(-2rem - 8px);top:.5rem;width:14px;height:14px;border-radius:50%;background:var(--ambar)}
.pasos strong{display:block;font:400 1.25rem var(--titulo)}
.opiniones{display:grid;grid-template-columns:repeat(3,1fr);gap:1.5rem}
blockquote{margin:0;border-top:4px solid var(--ambar);padding-top:1rem}
blockquote p{margin:0 0 .8rem;font:400 1.25rem/1.4 var(--titulo)}
cite{font-style:normal;color:var(--corteza);font-weight:500}
.formulario{display:grid;gap:1.1rem;max-width:520px}
label{display:grid;gap:.35rem;font-weight:500}
input,select{font:inherit;padding:.75rem .2rem;border:0;border-bottom:2px solid var(--musgo);background:transparent;color:var(--tinta)}
.error{color:#9b2c2c;margin:0}
.pie{display:flex;justify-content:space-between;flex-wrap:wrap;gap:.5rem;padding:2rem 5vw calc(2rem + env(safe-area-inset-bottom,0));background:var(--musgo);color:#F0F3EA}
.pie p{margin:0}
@media (max-width:800px){
  .cabecera{flex-wrap:wrap}
  .servicios,.dos-col,.opiniones{grid-template-columns:1fr}
  .galeria{grid-template-columns:1fr 1fr;grid-auto-rows:130px}
  .f1{grid-column:span 2}.f3{grid-row:span 1}.f5{grid-column:span 2}
  .retrato{max-width:280px}
  .respiro{width:70vw;right:-20%}
}

```

### Archivo: `css/estilos.css`
```css
:root{
  --niebla:#EDF1EF;
  --pizarra:#1B3431;
  --salvia:#7E948E;
  --malva:#A9788F;
  --blanco:#FAFBFA;
  --radio:14px;
  --titulo:"Marcellus",Georgia,serif;
  --texto:"Figtree",system-ui,sans-serif;
}
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0)}
*,*::before,*::after{box-sizing:inherit}
html{scroll-behavior:smooth;scroll-padding-top:70px}
body{margin:0;font:400 1.05rem/1.65 var(--texto);color:var(--pizarra);background:var(--niebla)}
h1,h2{font-family:var(--titulo);font-weight:400;line-height:1.15;margin:0 0 .6em}
h1{font-size:clamp(2.3rem,6vw,4rem)}
h2{font-size:clamp(1.8rem,4vw,2.6rem)}
a{color:inherit}
:focus-visible{outline:3px solid var(--malva);outline-offset:3px}

/* Cabecera */
.cabecera{position:sticky;top:0;z-index:10;display:flex;justify-content:space-between;align-items:center;padding:.8rem 5vw;background:rgba(237,241,239,.92);backdrop-filter:blur(8px)}
.marca{font-family:var(--titulo);font-size:1.3rem;text-decoration:none}
.menu{display:flex;gap:1.6rem;align-items:center}
.menu a{text-decoration:none}
.menu a:hover{color:var(--malva)}
.menu-boton{display:none;background:none;border:1px solid var(--pizarra);border-radius:99px;padding:.35rem 1rem;font:inherit;color:inherit;cursor:pointer}

/* Botones */
.boton,.boton-mini{display:inline-block;background:var(--pizarra);color:var(--blanco)!important;border:0;border-radius:99px;padding:.85rem 1.8rem;font:600 1rem var(--texto);text-decoration:none;cursor:pointer;transition:background .2s}
.boton:hover,.boton-mini:hover{background:var(--malva)}
.boton-mini{padding:.4rem 1.1rem}
.boton.claro{background:var(--blanco);color:var(--pizarra)!important}
.boton.claro:hover{background:var(--malva);color:var(--blanco)!important}
.enlace{margin-left:1.2rem}

/* Hero: círculo que respira */
.hero{display:grid;grid-template-columns:1fr 1fr;align-items:center;gap:3rem;min-height:82vh;padding:3rem 5vw}
.respiro{position:relative;justify-self:center;width:min(70vw,420px);aspect-ratio:1;border-radius:50%;
  background:radial-gradient(circle at 35% 30%,#c7d6d1,var(--salvia) 70%);
  display:grid;place-items:center;animation:respirar 10s ease-in-out infinite}
.respiro::after{content:"";position:absolute;inset:-9%;border-radius:50%;border:1px solid var(--salvia);opacity:.5}
.respiro span{font-family:var(--titulo);font-size:1.7rem;color:var(--blanco)}
@keyframes respirar{0%,100%{transform:scale(.78)}50%{transform:scale(1)}}
.hero-texto p{max-width:34ch;font-size:1.2rem}
.acciones{margin-top:1.8rem}

/* Secciones */
.seccion{padding:5rem 5vw;max-width:1100px;margin:0 auto}
.intro{max-width:52ch;margin-bottom:2rem}
.nota{font-size:.85rem;color:var(--salvia);margin-top:1.2rem}

/* Servicios */
.servicios{display:grid;grid-template-columns:1fr 1fr;gap:.9rem;align-items:start}
details{background:var(--blanco);border-radius:var(--radio);padding:1.1rem 1.4rem;transition:background .25s}
details[open]{background:#DDE7E3}
summary{display:flex;justify-content:space-between;gap:1rem;font-family:var(--titulo);font-size:1.25rem;cursor:pointer;list-style:none}
summary::-webkit-details-marker{display:none}
summary small{font:400 .9rem var(--texto);color:var(--salvia);align-self:center}
details p{margin:.8rem 0 0}

/* Sobre mí */
.oscura{max-width:none;background:var(--pizarra);color:var(--blanco)}
.dos-col{display:grid;grid-template-columns:1fr 1.2fr;gap:4rem;align-items:center;max-width:1100px;margin:0 auto}
.retrato{aspect-ratio:4/5;border-radius:200px 200px var(--radio) var(--radio);background:linear-gradient(160deg,var(--salvia),#5d716b);display:grid;place-items:center;color:var(--blanco);opacity:.9}

/* Pasos */
.pasos{list-style:none;padding:0;margin:0;display:grid;grid-template-columns:repeat(2,1fr);gap:1.5rem 3rem;counter-reset:paso}
.pasos li{counter-increment:paso;padding-left:3rem;position:relative}
.pasos li::before{content:counter(paso);position:absolute;left:0;top:0;width:2.1rem;height:2.1rem;border-radius:50%;background:var(--malva);color:var(--blanco);display:grid;place-items:center;font-family:var(--titulo)}
.pasos strong{display:block;font-family:var(--titulo);font-weight:400;font-size:1.2rem}

/* Formulario */
.formulario{display:grid;gap:1.1rem;max-width:520px}
label{display:grid;gap:.35rem;font-weight:500}
input,select{font:inherit;padding:.8rem 1rem;border:1px solid var(--salvia);border-radius:10px;background:var(--blanco);color:var(--pizarra)}
.error{color:#9b2c4a;margin:0}

.pie{display:flex;justify-content:space-between;flex-wrap:wrap;gap:.5rem;padding:2rem 5vw calc(2rem + env(safe-area-inset-bottom,0));border-top:1px solid #cfdad6}
.pie p{margin:0}

/* Móvil */
@media (max-width:800px){
  .hero,.dos-col,.servicios,.pasos{grid-template-columns:1fr}
  .hero{min-height:auto;padding-top:2rem}
  .menu-boton{display:block}
  .menu{display:none;position:absolute;top:100%;left:0;right:0;flex-direction:column;align-items:flex-start;gap:1rem;padding:1.2rem 5vw;background:var(--niebla)}
  .menu.abierto{display:flex}
  .retrato{max-width:280px}
}
@media (prefers-reduced-motion:reduce){
  .respiro{animation:none}
  html{scroll-behavior:auto}
}

/* Galería */
.galeria{display:grid;grid-template-columns:repeat(4,1fr);grid-auto-rows:170px;gap:.8rem;margin-bottom:1.8rem}
.foto{border-radius:var(--radio);display:flex;align-items:flex-end;padding:.9rem;color:var(--blanco);font-size:.95rem}
.f1{grid-column:span 2;grid-row:span 2;background:linear-gradient(150deg,var(--salvia),#4d635d)}
.f2{background:linear-gradient(150deg,var(--malva),#7d5569)}
.f3{background:linear-gradient(150deg,#5d716b,var(--pizarra))}
.f4{grid-column:span 2;background:linear-gradient(150deg,#9db1ab,var(--salvia))}
.f5{background:linear-gradient(150deg,#c39db0,var(--malva))}

/* Opiniones */
.opiniones{display:grid;grid-template-columns:repeat(3,1fr);gap:1.2rem}
blockquote{margin:0;background:var(--blanco);border-radius:var(--radio);padding:1.5rem}
blockquote p{margin:0 0 1rem;font-family:var(--titulo);font-size:1.15rem;line-height:1.45}
cite{font-style:normal;color:var(--salvia);font-weight:500}
@media (max-width:800px){
  .galeria{grid-template-columns:1fr 1fr;grid-auto-rows:130px}
  .f1,.f4{grid-column:span 2}
  .f1{grid-row:span 1}
  .opiniones{grid-template-columns:1fr}
}

/* Fotos reales (Cloudinary) */
.foto,.retrato{position:relative;overflow:hidden}
.con-imagen img{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.foto.con-imagen{background:var(--salvia)}
.foto.con-imagen span{position:relative;z-index:1;text-shadow:0 1px 6px rgba(0,0,0,.55)}
.foto.con-imagen::after{content:"";position:absolute;inset:auto 0 0 0;height:45%;background:linear-gradient(transparent,rgba(27,52,49,.55))}
.foto.con-imagen span{z-index:2}

/* Vistas previas con nombre de carpeta */
.vista-previa{display:grid;gap:.15rem;text-align:left;overflow-wrap:anywhere}
.retrato .vista-previa{text-align:center;padding:1rem}
.vista-previa strong{font-weight:600;font-size:1rem}
.vista-previa small{font-size:.8rem;opacity:.85}

```

### Archivo: `css/instalaciones.css`
```css
/* Instalaciones (fotos desde Cloudinary: carpeta "instalaciones")
   Un solo archivo para los 6 diseños. Hereda colores y radio del diseño activo.
   Dos formas: .cinta (tira horizontal) y .mosaico (una foto grande + chicas) */
.instalaciones{margin-bottom:1.5rem;width:100%;min-width:0}
.instalacion{position:relative;overflow:hidden;margin:0;display:flex;align-items:flex-end;padding:1rem;border-radius:var(--radio,18px);background:rgba(127,127,127,.22);color:inherit}
.instalacion img{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.instalacion.con-imagen{color:#fff}
.instalacion.con-imagen::after{content:"";position:absolute;inset:auto 0 0;height:45%;background:linear-gradient(transparent,rgba(0,0,0,.55))}
.instalacion figcaption,.instalacion .vista-previa{position:relative;z-index:2}
.instalacion figcaption{display:grid;gap:.25rem;text-shadow:0 1px 6px rgba(0,0,0,.6)}
.instalacion figcaption strong{font-weight:600}

/* Descripción: aparece al pasar el cursor (o al tocar en el celular) */
.instalacion .descripcion{max-height:0;opacity:0;overflow:hidden;font-size:.92rem;line-height:1.45;transition:max-height .35s ease,opacity .3s ease}
.instalacion:hover .descripcion,.instalacion:focus .descripcion,.instalacion:focus-within .descripcion{max-height:9em;opacity:1}
.instalacion.con-imagen::after{transition:height .3s ease}
.instalacion.con-imagen:hover::after,.instalacion.con-imagen:focus::after{height:75%}
.instalacion:focus-visible{outline:3px solid currentColor;outline-offset:3px}
.instalacion .vista-previa{text-align:left}

/* Tira horizontal */
.cinta{display:flex;gap:1rem;overflow-x:auto;scroll-snap-type:x mandatory;padding-bottom:.8rem}
.cinta .instalacion{flex:0 0 min(86%,520px);aspect-ratio:16/10;scroll-snap-align:start}

/* Mosaico: la primera foto ocupa dos filas */
.mosaico{display:grid;grid-template-columns:2fr 1fr;grid-auto-rows:190px;gap:1rem}
.mosaico .instalacion:first-child{grid-row:span 2}
@media (max-width:800px){
  .mosaico{grid-template-columns:1fr;grid-auto-rows:200px}
  .mosaico .instalacion:first-child{grid-row:span 1}
}

```

### Archivo: `css/luz.css`
```css
/* Diseño Luz: claro y fresco, cabecera centrada, servicios en tarjetas, galería tipo mosaico */
:root{--fondo:#FBFDFC;--aqua:#CDEBE6;--aqua2:#E8F5F2;--teal:#0E4F55;--coral:#F4A58A;--texto:#12363A;--titulo:"DM Serif Display",Georgia,serif;--cuerpo:"DM Sans",system-ui,sans-serif;box-sizing:border-box;padding-top:env(safe-area-inset-top,0)}
*,*::before,*::after{box-sizing:inherit}
html{scroll-behavior:smooth;scroll-padding-top:110px}
body{margin:0;background:var(--fondo);color:var(--texto);font:400 1.05rem/1.65 var(--cuerpo)}
h1,h2{font-family:var(--titulo);font-weight:400;line-height:1.1;margin:0 0 .6em;color:var(--teal)}
h1{font-size:clamp(2.6rem,7vw,5.2rem)}
h2{font-size:clamp(2rem,4vw,2.8rem);text-align:center}
a{color:inherit}
:focus-visible{outline:3px solid var(--coral);outline-offset:3px}
.cabecera{position:sticky;top:0;z-index:10;display:flex;flex-direction:column;align-items:center;gap:.5rem;padding:1rem 5vw;background:rgba(251,253,252,.92);backdrop-filter:blur(8px)}
.marca{font-family:var(--titulo);font-size:1.6rem;color:var(--teal);text-decoration:none}
.menu{display:flex;gap:1.4rem;align-items:center;background:#fff;border-radius:99px;padding:.35rem 1.2rem;box-shadow:0 2px 12px rgba(14,79,85,.1)}
.menu a{text-decoration:none;font-size:.95rem}
.menu a:hover{color:var(--coral)}
.menu-boton{display:none}
.boton,.boton-mini{display:inline-block;background:var(--teal);color:#fff!important;border:0;border-radius:99px;padding:.85rem 1.9rem;font:600 1rem var(--cuerpo);text-decoration:none;cursor:pointer;transition:background .2s}
.boton:hover,.boton-mini:hover{background:var(--coral);color:var(--teal)!important}
.boton-mini{padding:.35rem 1rem;background:var(--coral);color:var(--teal)!important}
.boton.claro{background:var(--teal)}
.enlace{margin-left:1.2rem}
.hero{text-align:center;display:grid;justify-items:center;align-content:center;min-height:78vh;padding:4rem 6vw;background:radial-gradient(circle at 15% 20%,var(--aqua),transparent 55%),radial-gradient(circle at 88% 75%,#FBE3D8,transparent 50%)}
.hero-texto p{max-width:44ch;margin:0 auto;font-size:1.25rem}
.acciones{margin-top:2rem}
.seccion{padding:5rem 6vw;max-width:1120px;margin:0 auto}
.intro,.nota{text-align:center}
.intro{max-width:56ch;margin:0 auto 2.2rem}
.nota{font-size:.85rem;color:#5c8c90;margin-top:1.4rem}
.servicios{display:grid;grid-template-columns:repeat(3,1fr);gap:1rem;align-items:start}
details{background:#fff;border:1px solid var(--aqua);border-radius:24px;padding:1.4rem;transition:background .25s}
details[open]{background:var(--aqua2)}
summary{display:grid;gap:.5rem;justify-items:start;font-family:var(--titulo);font-size:1.4rem;color:var(--teal);cursor:pointer;list-style:none}
summary::-webkit-details-marker{display:none}
summary small{font:600 .85rem var(--cuerpo);background:var(--coral);color:var(--teal);border-radius:99px;padding:.2rem .8rem}
details p{margin:.8rem 0 0}
.galeria{display:block;columns:3;column-gap:1rem;margin-bottom:1.8rem}
.foto{position:relative;overflow:hidden;break-inside:avoid;margin-bottom:1rem;border-radius:24px;display:flex;align-items:flex-end;padding:1rem;color:var(--teal)}
.f1{height:330px;background:linear-gradient(150deg,var(--aqua),#8fcfc7)}.f2{height:200px;background:linear-gradient(150deg,#FBE3D8,var(--coral))}.f3{height:260px;background:linear-gradient(150deg,#8fcfc7,#4fa3a5)}.f4{height:220px;background:linear-gradient(150deg,var(--aqua2),var(--aqua))}.f5{height:300px;background:linear-gradient(150deg,var(--coral),#e88a6b)}
.galeria+.boton{display:block;width:max-content;margin:0 auto}
.oscura{max-width:none;background:var(--aqua)}
.oscura .dos-col{max-width:1000px;margin:0 auto}
.oscura h2{text-align:left}
.dos-col{display:grid;grid-template-columns:auto 1fr;gap:4rem;align-items:center}
.retrato{position:relative;overflow:hidden;width:min(320px,70vw);aspect-ratio:1;border-radius:50%;background:linear-gradient(150deg,#fff,var(--coral));display:grid;place-items:center;color:var(--teal)}
.pasos{list-style:none;padding:0;margin:0;display:grid;grid-template-columns:repeat(4,1fr);gap:1.5rem;counter-reset:p;text-align:center}
.pasos li{counter-increment:p}
.pasos li::before{content:counter(p);display:grid;place-items:center;width:3rem;height:3rem;margin:0 auto .8rem;border-radius:50%;background:var(--teal);color:#fff;font-family:var(--titulo);font-size:1.3rem}
.pasos strong{display:block;font:400 1.2rem var(--titulo);color:var(--teal)}
.opiniones{display:grid;grid-template-columns:repeat(3,1fr);gap:1.2rem}
blockquote{margin:0;background:#fff;border-radius:24px;padding:1.5rem;box-shadow:0 4px 18px rgba(14,79,85,.08)}
blockquote::before{content:"\201C";display:block;font:4rem/1 var(--titulo);color:var(--coral)}
blockquote p{margin:0 0 1rem}
cite{font-style:normal;font-weight:600;color:var(--teal)}
.formulario{display:grid;gap:1.1rem;max-width:520px;margin:0 auto;background:#fff;padding:2rem;border-radius:28px;box-shadow:0 8px 30px rgba(14,79,85,.12)}
label{display:grid;gap:.35rem;font-weight:500}
input,select{font:inherit;padding:.8rem 1rem;border:1px solid #b9dcd7;border-radius:12px;background:var(--fondo);color:var(--texto)}
.error{color:#a3324f;margin:0}
.pie{display:flex;justify-content:space-between;flex-wrap:wrap;gap:.5rem;padding:2rem 6vw calc(2rem + env(safe-area-inset-bottom,0));background:var(--teal);color:#fff}
.pie p{margin:0}
.flotante{position:fixed;right:1rem;bottom:calc(6.5rem + env(safe-area-inset-bottom,0));z-index:20;background:#1FAF5A;color:#fff;border-radius:99px;padding:.7rem 1.3rem;font-weight:600;text-decoration:none;box-shadow:0 6px 18px rgba(0,0,0,.2)}
.con-imagen img{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.foto.con-imagen span{position:relative;z-index:2;color:#fff;text-shadow:0 1px 6px rgba(0,0,0,.6)}
.vista-previa{display:grid;gap:.15rem;overflow-wrap:anywhere;padding:1rem;text-align:center}
.foto .vista-previa{text-align:left;padding:0}
.vista-previa small{font-size:.8rem;opacity:.85}
@media (max-width:800px){
  .menu-boton{display:block;background:none;border:1px solid var(--teal);color:var(--teal);border-radius:99px;padding:.3rem 1rem;font:inherit}
  .cabecera{flex-direction:row;justify-content:space-between}
  .menu{display:none;position:absolute;top:100%;left:5vw;right:5vw;flex-direction:column;align-items:flex-start;border-radius:20px;padding:1rem 1.4rem}
  .menu.abierto{display:flex}
  .servicios,.opiniones,.dos-col{grid-template-columns:1fr}
  .pasos{grid-template-columns:1fr 1fr}
  .galeria{columns:2}
  .oscura h2{text-align:center}
  .retrato{margin:0 auto}
}
@media (max-width:520px){.pasos{grid-template-columns:1fr}.galeria{columns:1}}
@media (prefers-reduced-motion:reduce){html{scroll-behavior:auto}}

```

### Archivo: `css/mineral.css`
```css
/* Base compartida */
*,*::before,*::after{box-sizing:inherit}
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0)}
html{scroll-behavior:smooth;scroll-padding-top:90px}
body{margin:0}
a{color:inherit}
:focus-visible{outline:3px solid var(--acento);outline-offset:3px}
details{transition:background .25s}
summary{cursor:pointer;list-style:none}
summary::-webkit-details-marker{display:none}
.foto,.retrato{position:relative;overflow:hidden}
.foto{display:flex;align-items:flex-end;padding:1rem}
.retrato{display:grid;place-items:center}
.con-imagen img{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.foto.con-imagen span{position:relative;z-index:2;color:#fff;text-shadow:0 1px 6px rgba(0,0,0,.6)}
.foto.con-imagen::after{content:"";position:absolute;inset:auto 0 0;height:40%;background:linear-gradient(transparent,rgba(0,0,0,.45))}
.vista-previa{display:grid;gap:.15rem;overflow-wrap:anywhere;padding:.5rem;text-align:center}
.foto .vista-previa{text-align:left;padding:0}
.vista-previa small{font-size:.8rem;opacity:.85}
.menu-boton{display:none}
@media (max-width:800px){
  .menu-boton{display:block;background:none;color:inherit;border:1px solid currentColor;border-radius:99px;padding:.3rem 1rem;font:inherit}
  .menu{display:none!important;position:absolute;top:100%;left:0;right:0;flex-direction:column;align-items:flex-start;gap:1rem;padding:1.2rem 5vw;background:var(--menu-fondo,var(--fondo))}
  .menu.abierto{display:flex!important}
}
@media (prefers-reduced-motion:reduce){*{animation:none!important}html{scroll-behavior:auto}}
@keyframes respirar{0%,100%{transform:scale(.8)}50%{transform:scale(1)}}
/* Diseño Mineral: gris piedra y azul tinta, minimalista, títulos en columna lateral, servicios en tabla */
:root{--fondo:#EEF0F0;--tinta:#1F2528;--suave:#66707A;--linea:#C9CFD1;--acento:#2F4B7C;--titulo:"Newsreader",Georgia,serif;--cuerpo:"Instrument Sans",system-ui,sans-serif}
body{background:var(--fondo);color:var(--tinta);font:400 1.05rem/1.7 var(--cuerpo)}
h1,h2{font-family:var(--titulo);font-weight:300;line-height:1.05;margin:0 0 .6em}
h1{font-size:clamp(3rem,9vw,7rem);max-width:12ch}
h2{font-size:1.5rem;font-weight:400;color:var(--acento)}
.cabecera{position:sticky;top:0;z-index:10;display:flex;justify-content:space-between;align-items:center;padding:1rem 5vw;background:var(--fondo);border-bottom:1px solid var(--linea)}
.marca{font-family:var(--titulo);font-size:1.3rem;text-decoration:none}
.menu{display:flex;gap:1.6rem;align-items:center;font-size:.95rem}
.menu a{text-decoration:none;color:var(--suave)}
.menu a:hover{color:var(--acento)}
.boton,.boton-mini{display:inline-block;background:var(--acento);color:#fff!important;border:0;padding:.85rem 1.8rem;font:500 1rem var(--cuerpo);text-decoration:none;cursor:pointer;transition:background .2s}
.boton:hover,.boton-mini:hover{background:var(--tinta)}
.boton-mini{padding:.3rem .9rem}
.boton.claro{background:var(--tinta)}
.enlace{margin-left:1.4rem}
.hero{max-width:1200px;margin:0 auto;padding:9rem 5vw 6rem}
.hero-texto p{max-width:40ch;color:var(--suave);font-size:1.25rem}
.acciones{margin-top:2rem}
.seccion{display:grid;grid-template-columns:220px 1fr;gap:0 2rem;max-width:1200px;margin:0 auto;padding:4.5rem 5vw;border-top:1px solid var(--linea)}
.seccion>*{grid-column:2}
.seccion>h2{grid-column:1;grid-row:1/span 6;position:sticky;top:5rem;align-self:start;margin:0}
.seccion.oscura{display:block;background:transparent;color:inherit}
.oscura .dos-col{display:grid;grid-template-columns:220px 1fr;gap:2rem;align-items:start}
.oscura h2{position:static}
.intro,.nota{color:var(--suave)}
.nota{font-size:.85rem}
.servicios{display:block}
details{border-bottom:1px solid var(--linea);padding:1rem 0}
details[open]{background:transparent}
summary{display:grid;grid-template-columns:1fr auto;gap:1rem;font-family:var(--titulo);font-size:1.4rem}
summary small{font:400 .95rem var(--cuerpo);color:var(--suave);align-self:center}
summary:hover span{color:var(--acento)}
details p{margin:.6rem 0 0;color:var(--suave);max-width:58ch}
.galeria{display:grid;grid-template-columns:repeat(3,1fr);gap:.5rem;margin-bottom:1.6rem}
.foto{aspect-ratio:1;color:#fff}
.f1{background:linear-gradient(150deg,#b9c2c6,#8d999e)}.f2{background:linear-gradient(150deg,#2F4B7C,#1f3356)}.f3{background:linear-gradient(150deg,#d9dddf,#aab3b7)}.f4{background:linear-gradient(150deg,#8d999e,#5d6a70)}.f5{background:linear-gradient(150deg,#c9cfd1,#8d999e)}
.retrato{aspect-ratio:3/4;background:linear-gradient(160deg,#c9cfd1,#8d999e);color:#fff}
.pasos{list-style:none;padding:0;margin:0}
.pasos li{display:grid;grid-template-columns:1fr 2fr;gap:1rem;padding:1rem 0;border-bottom:1px solid var(--linea);color:var(--suave)}
.pasos strong{font:400 1.25rem var(--titulo);color:var(--tinta)}
.opiniones{display:grid}
blockquote{margin:0;padding:1.4rem 0;border-bottom:1px solid var(--linea)}
blockquote p{margin:0 0 .5rem;font:italic 300 1.5rem/1.4 var(--titulo)}
cite{font-style:normal;color:var(--suave)}
.formulario{display:grid;gap:1.1rem;max-width:520px}
label{display:grid;gap:.35rem;font-weight:500}
input,select{font:inherit;padding:.75rem 0;border:0;border-bottom:1px solid var(--tinta);background:transparent;color:var(--tinta);border-radius:0}
.error{color:#9b2c3c;margin:0}
.pie{display:flex;justify-content:space-between;flex-wrap:wrap;gap:.5rem;padding:2rem 5vw calc(4.5rem + env(safe-area-inset-bottom,0));border-top:1px solid var(--linea);color:var(--suave)}
.pie p{margin:0}
@media (max-width:800px){
  .seccion,.oscura .dos-col{grid-template-columns:1fr}
  .seccion>*,.seccion>h2{grid-column:1;grid-row:auto;position:static}
  .seccion>h2{margin-bottom:1rem}
  .hero{padding:5rem 5vw 3rem}
  .pasos li{grid-template-columns:1fr}
  .retrato{max-width:240px}
}

```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Masajes y relajación</title>
<meta name="description" content="Centro estético de masajes y relajación. Reservá tu momento de calma.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Marcellus&family=Figtree:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="css/estilos.css">
<link rel="stylesheet" href="css/instalaciones.css">
</head>
<body>

<header class="cabecera">
  <a href="#inicio" class="marca" data-texto="negocio.nombre"></a>
  <button class="menu-boton" aria-label="Abrir menú" aria-expanded="false">Menú</button>
  <nav class="menu" id="menu">
    <a href="#servicios">Servicios</a>
    <a href="#galeria">Galería</a>
    <a href="#instalaciones">Instalaciones</a>
    <a href="#sobre-mi">Sobre mí</a>
    <a href="#cuidados">Tu visita</a>
    <a href="#contacto" class="boton-mini">Reservar</a>
  </nav>
</header>

<main>
  <section class="hero" id="inicio">
    <div class="respiro" aria-hidden="true"><span id="respiro-texto"></span></div>
    <div class="hero-texto">
      <h1 data-texto="inicio.titulo"></h1>
      <p data-texto="inicio.texto"></p>
      <div class="acciones">
        <a href="#contacto" class="boton" data-texto="inicio.botonReserva"></a>
        <a href="#servicios" class="enlace" data-texto="inicio.botonServicios"></a>
      </div>
    </div>
  </section>

  <section class="seccion" id="servicios">
    <h2 data-texto="servicios.titulo"></h2>
    <p class="intro" data-texto="servicios.intro"></p>
    <div class="servicios" id="lista-servicios"></div>
    <p class="nota" data-texto="servicios.nota"></p>
  </section>

  <section class="seccion" id="galeria">
    <h2 data-texto="galeria.titulo"></h2>
    <p class="intro" data-texto="galeria.intro"></p>
    <div class="galeria" id="lista-galeria"></div>
    <a class="boton" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="galeria.boton"></a>
    <p class="nota" data-texto="galeria.nota"></p>
  </section>

  <section class="seccion oscura" id="sobre-mi">
    <div class="dos-col">
      <div class="retrato" role="img"><span data-texto="sobreMi.textoFoto"></span></div>
      <div>
        <h2 data-texto="sobreMi.titulo"></h2>
        <div id="parrafos-sobre-mi"></div>
        <a class="boton claro" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="sobreMi.botonInstagram"></a>
      </div>
    </div>
  </section>

  <section class="seccion" id="cuidados">
    <h2 data-texto="visita.titulo"></h2>
    <ol class="pasos" id="lista-pasos"></ol>
  </section>

  <section class="seccion" id="instalaciones">
    <h2 data-texto="instalaciones.titulo"></h2>
    <p class="intro" data-texto="instalaciones.intro"></p>
    <div class="instalaciones cinta" id="lista-instalaciones"></div>
    <p class="nota" data-texto="instalaciones.nota"></p>
  </section>

  <section class="seccion" id="opiniones">
    <h2 data-texto="opiniones.titulo"></h2>
    <div class="opiniones" id="lista-opiniones"></div>
    <p class="nota" data-texto="opiniones.nota"></p>
  </section>

  <section class="seccion" id="contacto">
    <h2 data-texto="reserva.titulo"></h2>
    <p class="intro" data-texto="reserva.intro"></p>
    <form id="formulario" class="formulario" novalidate>
      <label><span data-texto="reserva.etiquetaNombre"></span>
        <input type="text" name="nombre" autocomplete="name" required>
      </label>
      <label><span data-texto="reserva.etiquetaServicio"></span>
        <select name="servicio" id="campo-servicio" required></select>
      </label>
      <label><span data-texto="reserva.etiquetaHorario"></span>
        <input type="text" name="horario" id="campo-horario">
      </label>
      <p class="error" id="error" role="alert" hidden></p>
      <button type="submit" class="boton" data-texto="reserva.boton"></button>
    </form>
  </section>
</main>

<footer class="pie">
  <p data-texto="negocio.pie"></p>
  <a target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="contacto.usuarioInstagram"></a>
</footer>

<script src="js/configuracion.js"></script>
<script src="js/principal.js"></script>
<script src="js/selector.js"></script>
</body>
</html>

```

### Archivo: `js/configuracion.js`
```javascript
// ============================================================
// CONFIGURACIÓN DEL SITIO
// Editá solo este archivo para cambiar textos, contacto y datos.
// ============================================================
const CONFIGURACION = {

  // Mostrar u ocultar los avisos "de ejemplo" (poné false al publicar)
  mostrarNotas: true,

  // Selector para mostrarle varios diseños al cliente (poné false al entregar el sitio final)
  mostrarSelectorDisenos: true,
  disenos: [
    { nombre: "Serenidad", archivo: "index.html" },
    { nombre: "Atelier", archivo: "atelier.html" },
    { nombre: "Luz", archivo: "luz.html" },
    { nombre: "Bosque", archivo: "bosque.html" },
    { nombre: "Aurora", archivo: "aurora.html" },
    { nombre: "Mineral", archivo: "mineral.html" }
  ],

  // Mientras no haya fotos reales, las vistas previas indican la carpeta y la etiqueta
  // de Cloudinary donde hay que subirlas (poné false para ver los textos de ejemplo)
  vistasPreviasConCarpeta: true,

  // Fotos desde Cloudinary (solo lectura: no se necesita upload preset)
  // El gestor de imágenes etiqueta cada foto como "<proyecto>_<carpeta>"
  cloudinary: {
    nombreNube: "p0qlmlor",
    proyecto: "patriciabertoche",
    carpetaGaleria: "galeria",    // etiqueta: patriciabertoche_galeria
    carpetaImagenes: "imagenes",  // etiqueta: patriciabertoche_imagenes
    carpetaInstalaciones: "instalaciones", // etiqueta: patriciabertoche_instalaciones
    palabraRetrato: "patricia",   // la foto de "Sobre mí" es la que tenga esta palabra en el nombre
    // Opcional y más directo: ID público de la foto de "Sobre mí" tal como figura en Cloudinary
    // (ej: "patriciabertoche/imagenes/patricia.webp" o "patricia.webp"). Si lo completás, no hace falta etiqueta.
    fotoRetrato: ""
  },

  negocio: {
    nombre: "Patricia Bertoche",
    lema: "Masajes y relajación",
    pie: "Patricia Bertoche · Masajes y relajación"
  },

  contacto: {
    whatsapp: "59800000000", // código de país + número, sin + ni espacios
    instagram: "https://www.instagram.com/patricia_bertoche/",
    usuarioInstagram: "@patricia_bertoche",
    saludo: "Hola Patricia"
  },

  inicio: {
    titulo: "Una hora para volver a vos",
    texto: "Masajes, tratamientos corporales y estética en un espacio pensado para bajar el ritmo.",
    botonReserva: "Reservar mi turno",
    botonServicios: "Ver servicios",
    textoInhalar: "Inhalá",
    textoExhalar: "Exhalá"
  },

  servicios: {
    titulo: "Servicios",
    intro: "Tocá un servicio para ver de qué se trata. Todos se adaptan a cómo llegás ese día.",
    nota: "Duraciones de ejemplo: reemplazalas por las reales.",
    lista: [
      { nombre: "Masaje relajante", duracion: "60 min", detalle: "Movimientos lentos y continuos con aceites tibios para soltar la tensión del día a día." },
      { nombre: "Descontracturante", duracion: "50 min", detalle: "Trabajo profundo en cuello, espalda y hombros, para zonas cargadas por postura o estrés." },
      { nombre: "Drenaje linfático", duracion: "60 min", detalle: "Técnica suave que ayuda a reducir la retención de líquidos y la sensación de pesadez." },
      { nombre: "Piedras calientes", duracion: "75 min", detalle: "Calor mineral sobre puntos clave del cuerpo para una relajación muscular más profunda." },
      { nombre: "Limpieza facial", duracion: "60 min", detalle: "Limpieza, exfoliación e hidratación según tu tipo de piel, con masaje facial incluido." },
      { nombre: "Tratamientos corporales", duracion: "Consultar", detalle: "Exfoliaciones e hidratación corporal para renovar la piel. Escribinos y armamos el plan." }
    ]
  },

  galeria: {
    titulo: "Galería",
    intro: "Un vistazo al espacio y a los tratamientos. Más fotos en Instagram.",
    boton: "Ver más en Instagram",
    nota: "Las fotos vienen de Cloudinary. Cada vista previa indica la carpeta y la etiqueta donde subir la foto.",
    fotos: ["Cabina de masajes", "Aceites y aromas", "Piedras calientes", "Sala de té", "Facial"]
  },

  instalaciones: {
    titulo: "Nuestras instalaciones",
    intro: "Un espacio pensado para que te sientas en calma desde que llegás.",
    nota: "Las fotos vienen de Cloudinary: carpeta patriciabertoche/instalaciones.",
    fotos: ["Recepción", "Cabina de masajes", "Sala de té"]
  },

  sobreMi: {
    titulo: "Hola, soy Patricia",
    parrafos: [
      "Trabajo con las manos y con el tiempo: cada sesión empieza escuchando cómo estás y termina con un momento de silencio para que el cuerpo asiente.",
      "Mi centro es un lugar tranquilo, con luz baja, música suave y productos cuidados."
    ],
    botonInstagram: "Seguime en Instagram",
    textoFoto: "Foto de Patricia"
  },

  visita: {
    titulo: "Cómo es tu visita",
    pasos: [
      { titulo: "Llegás y te tomás un té.", detalle: "Unos minutos para dejar el celular y respirar." },
      { titulo: "Charlamos.", detalle: "Me contás dónde sentís tensión o qué querés lograr." },
      { titulo: "Sesión.", detalle: "Camilla tibia, aromas suaves y un ritmo a tu medida." },
      { titulo: "Cierre.", detalle: "Te dejo recomendaciones simples para cuidarte en casa." }
    ]
  },

  opiniones: {
    titulo: "Lo que dicen quienes vinieron",
    nota: "Opiniones de ejemplo: reemplazalas por testimonios reales con permiso de cada persona.",
    lista: [
      { texto: "Salí con el cuerpo liviano y la cabeza en calma. Volví al mes siguiente.", autor: "María L." },
      { texto: "Patricia escucha antes de empezar y se nota. El descontracturante me cambió la semana.", autor: "Andrés P." },
      { texto: "Un lugar tranquilo, prolijo y cálido. Ideal para desconectar.", autor: "Lucía R." }
    ]
  },

  reserva: {
    titulo: "Reservá tu turno",
    intro: "Completá el formulario y se abre WhatsApp con tu mensaje listo para enviar.",
    etiquetaNombre: "Nombre",
    etiquetaServicio: "Servicio",
    opcionVacia: "Elegí uno",
    etiquetaHorario: "Día y horario preferido",
    ejemploHorario: "Ej: jueves por la tarde",
    boton: "Enviar por WhatsApp",
    error: "Completá tu nombre y elegí un servicio para continuar."
  }
};

```

### Archivo: `js/principal.js`
```javascript
// Toda la información editable está en js/configuracion.js
const C = CONFIGURACION;

// Lee un valor de la configuración con ruta tipo "inicio.titulo"
const leer = (ruta) => ruta.split(".").reduce((o, k) => (o ? o[k] : undefined), C);

// Crea un elemento con clase y texto (textContent: seguro ante HTML)
function crear(etiqueta, clase, texto) {
  const el = document.createElement(etiqueta);
  if (clase) el.className = clase;
  if (texto !== undefined) el.textContent = texto;
  return el;
}

// Textos y enlaces simples
document.querySelectorAll("[data-texto]").forEach((el) => {
  el.textContent = leer(el.dataset.texto) ?? "";
});
document.querySelectorAll("[data-enlace]").forEach((el) => {
  el.href = leer(el.dataset.enlace);
});
document.title = `${C.negocio.nombre} | ${C.negocio.lema}`;
document.querySelectorAll(".nota").forEach((n) => (n.hidden = !C.mostrarNotas));

// Servicios (acordeón) y opciones del formulario
const listaServicios = document.getElementById("lista-servicios");
const campoServicio = document.getElementById("campo-servicio");
campoServicio.append(new Option(C.reserva.opcionVacia, ""));
C.servicios.lista.forEach((s, i) => {
  const d = crear("details");
  d.name = "servicio";
  d.open = i === 0;
  const resumen = crear("summary");
  resumen.append(crear("span", "", s.nombre));
  resumen.append(crear("small", "", [s.duracion, s.precio].filter(Boolean).join(" · ")));
  d.append(resumen, crear("p", "", s.detalle));
  listaServicios.append(d);
  campoServicio.append(new Option(s.nombre, s.nombre));
});
document.getElementById("campo-horario").placeholder = C.reserva.ejemploHorario;

// Fotos desde Cloudinary (lista pública por etiqueta, igual que Monarca)
const N = C.cloudinary;
const urlFoto = (img, ancho) =>
  `https://res.cloudinary.com/${N.nombreNube}/image/upload/q_auto,f_auto,w_${ancho},c_limit/v${img.version}/${img.public_id}.${img.format}`;

function traerFotos(carpeta) {
  const etiqueta = `${N.proyecto}_${carpeta}`;
  return fetch(`https://res.cloudinary.com/${N.nombreNube}/image/list/${etiqueta}.json`)
    .then((r) => {
      if (!r.ok) throw new Error(`Cloudinary respondió ${r.status} para la etiqueta "${etiqueta}" (falta la etiqueta o Resource list)`);
      return r.json();
    })
    .then((d) => d.resources || [])
    .catch((e) => { console.warn(e.message); return []; });
}

// Galería
const listaGaleria = document.getElementById("lista-galeria");
// Contenido de una vista previa: dice en qué carpeta de Cloudinary va la foto
function textoVistaPrevia(carpeta, extra) {
  const caja = crear("span", "vista-previa");
  caja.append(crear("strong", "", `${N.proyecto}/${carpeta}`));
  if (extra) caja.append(crear("small", "", extra));
  caja.append(crear("small", "", `Etiqueta: ${N.proyecto}_${carpeta}`));
  return caja;
}
function galeriaDeEjemplo() {
  C.galeria.fotos.forEach((titulo, i) => {
    const foto = crear("div", `foto f${i + 1}`);
    foto.append(C.vistasPreviasConCarpeta ? textoVistaPrevia(N.carpetaGaleria) : crear("span", "", titulo));
    listaGaleria.append(foto);
  });
}
traerFotos(N.carpetaGaleria).then((fotos) => {
  if (!fotos.length) return galeriaDeEjemplo();
  fotos.forEach((img, i) => {
    const foto = crear("div", `foto f${(i % 5) + 1} con-imagen`);
    const titulo = (img.context?.custom?.caption || "").trim();
    const el = document.createElement("img");
    el.src = urlFoto(img, 900);
    el.alt = titulo || "Foto del centro";
    el.loading = "lazy";
    foto.append(el);
    if (titulo) foto.append(crear("span", "", titulo));
    listaGaleria.append(foto);
  });
});

// Instalaciones (carpeta instalaciones)
const listaInstalaciones = document.getElementById("lista-instalaciones");
if (listaInstalaciones) {
  traerFotos(N.carpetaInstalaciones).then((fotos) => {
    if (!fotos.length) {
      C.instalaciones.fotos.forEach((titulo) => {
        const fig = crear("figure", "instalacion");
        fig.append(C.vistasPreviasConCarpeta ? textoVistaPrevia(N.carpetaInstalaciones) : crear("figcaption", "", titulo));
        listaInstalaciones.append(fig);
      });
      return;
    }
    fotos.forEach((img) => {
      const fig = crear("figure", "instalacion con-imagen");
      const titulo = (img.context?.custom?.caption || "").trim();
      // "Description (alt)" de Cloudinary llega como context.custom.alt
      const descripcion = (img.context?.custom?.alt || "").trim();
      const el = document.createElement("img");
      el.src = urlFoto(img, 1100);
      el.alt = descripcion || titulo || "Instalaciones del centro";
      el.loading = "lazy";
      fig.append(el);
      if (titulo || descripcion) {
        const pie = crear("figcaption");
        if (titulo) pie.append(crear("strong", "", titulo));
        if (descripcion) pie.append(crear("span", "descripcion", descripcion));
        fig.append(pie);
      }
      if (descripcion) {
        fig.tabIndex = 0; // en celular se muestra al tocar
        fig.title = descripcion; // texto al dejar el cursor quieto
      }
      listaInstalaciones.append(fig);
    });
  });
}

// Foto de "Sobre mí" (carpeta imagenes)
function mostrarRetrato(url) {
  const contenedor = document.querySelector(".retrato");
  const el = document.createElement("img");
  el.src = url;
  el.alt = C.sobreMi.textoFoto;
  el.onerror = () => console.warn("No se pudo cargar la foto de Patricia:", url);
  contenedor.replaceChildren(el);
  contenedor.classList.add("con-imagen");
}
if (N.fotoRetrato) {
  mostrarRetrato(`https://res.cloudinary.com/${N.nombreNube}/image/upload/q_auto,w_800,c_limit/${N.fotoRetrato}`);
} else {
  traerFotos(N.carpetaImagenes).then((fotos) => {
    if (!fotos.length) {
      if (C.vistasPreviasConCarpeta) {
        document.querySelector(".retrato").replaceChildren(
          textoVistaPrevia(N.carpetaImagenes, `Nombre del archivo: ${N.palabraRetrato}`)
        );
      }
      return;
    }
    const retrato = fotos.find((f) => f.public_id.split("/").pop().toLowerCase().includes(N.palabraRetrato)) || fotos[0];
    mostrarRetrato(urlFoto(retrato, 800));
  });
}

// Sobre mí
const parrafos = document.getElementById("parrafos-sobre-mi");
C.sobreMi.parrafos.forEach((t) => parrafos.append(crear("p", "", t)));
document.querySelector(".retrato").setAttribute("aria-label", C.sobreMi.textoFoto);

// Pasos de la visita
const listaPasos = document.getElementById("lista-pasos");
C.visita.pasos.forEach((p) => {
  const li = crear("li");
  li.append(crear("strong", "", p.titulo), document.createTextNode(p.detalle));
  listaPasos.append(li);
});

// Opiniones
const listaOpiniones = document.getElementById("lista-opiniones");
C.opiniones.lista.forEach((o) => {
  const b = crear("blockquote");
  b.append(crear("p", "", o.texto), crear("cite", "", o.autor));
  listaOpiniones.append(b);
});

// Menú móvil
const botonMenu = document.querySelector(".menu-boton");
const menu = document.getElementById("menu");
botonMenu.addEventListener("click", () => {
  const abierto = menu.classList.toggle("abierto");
  botonMenu.setAttribute("aria-expanded", abierto);
});
menu.addEventListener("click", (e) => {
  if (e.target.tagName === "A") {
    menu.classList.remove("abierto");
    botonMenu.setAttribute("aria-expanded", "false");
  }
});

// Texto del círculo sincronizado con la animación (10 s: 5 inhalar, 5 exhalar)
const textoRespiro = document.getElementById("respiro-texto");
let inhalando = false;
function alternar() {
  if (!textoRespiro) return;
  inhalando = !inhalando;
  textoRespiro.textContent = inhalando ? C.inicio.textoInhalar : C.inicio.textoExhalar;
}
alternar();
setInterval(alternar, 5000);

// Formulario -> WhatsApp
const formulario = document.getElementById("formulario");
const error = document.getElementById("error");
error.textContent = C.reserva.error;
formulario.addEventListener("submit", (e) => {
  e.preventDefault();
  const datos = new FormData(formulario);
  const nombre = datos.get("nombre").trim();
  const servicio = datos.get("servicio");
  const horario = datos.get("horario").trim();

  if (!nombre || !servicio) {
    error.hidden = false;
    return;
  }
  error.hidden = true;

  let mensaje = `${C.contacto.saludo}, soy ${nombre}. Quiero reservar: ${servicio}.`;
  if (horario) mensaje += ` Me viene bien: ${horario}.`;
  window.open(`https://wa.me/${C.contacto.whatsapp}?text=${encodeURIComponent(mensaje)}`, "_blank", "noopener");
});

// Botón flotante de WhatsApp (solo en los diseños que lo incluyen)
document.querySelectorAll("[data-whatsapp]").forEach((a) => {
  a.href = `https://wa.me/${C.contacto.whatsapp}?text=${encodeURIComponent(C.contacto.saludo + ", quiero reservar un turno.")}`;
});

```

### Archivo: `js/selector.js`
```javascript
// Selector de diseños para mostrarle varias opciones al cliente.
// Se configura en js/configuracion.js (disenos y mostrarSelectorDisenos).
(function () {
  const C = CONFIGURACION;
  if (!C.mostrarSelectorDisenos || !Array.isArray(C.disenos) || C.disenos.length < 2) return;

  const actual = location.pathname.split("/").pop() || "index.html";
  const estilo = document.createElement("style");
  estilo.textContent = `
    .selector-disenos{position:fixed;left:50%;bottom:calc(.8rem + env(safe-area-inset-bottom,0px));transform:translateX(-50%);z-index:999;display:flex;flex-wrap:wrap;justify-content:center;align-items:center;gap:.3rem;padding:.35rem;background:rgba(20,20,24,.92);border-radius:22px;width:max-content;box-shadow:0 6px 24px rgba(0,0,0,.3);font:500 .85rem system-ui,sans-serif;color:#fff;max-width:96vw}
    .selector-disenos span{padding:0 .6rem;opacity:.7}
    .selector-disenos a{color:#fff;text-decoration:none;padding:.45rem .9rem;border-radius:99px}
    .selector-disenos a:hover{background:rgba(255,255,255,.15)}
    .selector-disenos a[aria-current="page"]{background:#fff;color:#141418}`;
  document.head.append(estilo);

  const caja = document.createElement("nav");
  caja.className = "selector-disenos";
  caja.setAttribute("aria-label", "Elegir diseño");
  const etiqueta = document.createElement("span");
  etiqueta.textContent = "Diseño";
  caja.append(etiqueta);
  C.disenos.forEach((d, i) => {
    const a = document.createElement("a");
    a.href = d.archivo;
    a.textContent = `${i + 1} · ${d.nombre}`;
    if (d.archivo === actual) a.setAttribute("aria-current", "page");
    caja.append(a);
  });
  document.body.append(caja);
})();

```

### Archivo: `luz.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Masajes y relajación</title>
<meta name="description" content="Centro estético de masajes y relajación. Reservá tu momento de calma.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="css/luz.css">
<link rel="stylesheet" href="css/instalaciones.css">
</head>
<body>

<header class="cabecera">
  <a href="#inicio" class="marca" data-texto="negocio.nombre"></a>
  <button class="menu-boton" aria-label="Abrir menú" aria-expanded="false">Menú</button>
  <nav class="menu" id="menu">
    <a href="#servicios">Servicios</a>
    <a href="#galeria">Galería</a>
    <a href="#instalaciones">Instalaciones</a>
    <a href="#sobre-mi">Sobre mí</a>
    <a href="#cuidados">Tu visita</a>
    <a href="#contacto" class="boton-mini">Reservar</a>
  </nav>
</header>

<main>
  <section class="hero" id="inicio">
    <div class="hero-texto">
      <h1 data-texto="inicio.titulo"></h1>
      <p data-texto="inicio.texto"></p>
      <div class="acciones">
        <a href="#contacto" class="boton" data-texto="inicio.botonReserva"></a>
        <a href="#servicios" class="enlace" data-texto="inicio.botonServicios"></a>
      </div>
    </div>
  </section>

  <section class="seccion" id="galeria">
    <h2 data-texto="galeria.titulo"></h2>
    <p class="intro" data-texto="galeria.intro"></p>
    <div class="galeria" id="lista-galeria"></div>
    <a class="boton" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="galeria.boton"></a>
    <p class="nota" data-texto="galeria.nota"></p>
  </section>

  <section class="seccion" id="servicios">
    <h2 data-texto="servicios.titulo"></h2>
    <p class="intro" data-texto="servicios.intro"></p>
    <div class="servicios" id="lista-servicios"></div>
    <p class="nota" data-texto="servicios.nota"></p>
  </section>

  <section class="seccion" id="opiniones">
    <h2 data-texto="opiniones.titulo"></h2>
    <div class="opiniones" id="lista-opiniones"></div>
    <p class="nota" data-texto="opiniones.nota"></p>
  </section>

  <section class="seccion oscura" id="sobre-mi">
    <div class="dos-col">
      <div class="retrato" role="img"><span data-texto="sobreMi.textoFoto"></span></div>
      <div>
        <h2 data-texto="sobreMi.titulo"></h2>
        <div id="parrafos-sobre-mi"></div>
        <a class="boton claro" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="sobreMi.botonInstagram"></a>
      </div>
    </div>
  </section>

  <section class="seccion" id="cuidados">
    <h2 data-texto="visita.titulo"></h2>
    <ol class="pasos" id="lista-pasos"></ol>
  </section>

  <section class="seccion" id="instalaciones">
    <h2 data-texto="instalaciones.titulo"></h2>
    <p class="intro" data-texto="instalaciones.intro"></p>
    <div class="instalaciones cinta" id="lista-instalaciones"></div>
    <p class="nota" data-texto="instalaciones.nota"></p>
  </section>

  <section class="seccion" id="contacto">
    <h2 data-texto="reserva.titulo"></h2>
    <p class="intro" data-texto="reserva.intro"></p>
    <form id="formulario" class="formulario" novalidate>
      <label><span data-texto="reserva.etiquetaNombre"></span>
        <input type="text" name="nombre" autocomplete="name" required>
      </label>
      <label><span data-texto="reserva.etiquetaServicio"></span>
        <select name="servicio" id="campo-servicio" required></select>
      </label>
      <label><span data-texto="reserva.etiquetaHorario"></span>
        <input type="text" name="horario" id="campo-horario">
      </label>
      <p class="error" id="error" role="alert" hidden></p>
      <button type="submit" class="boton" data-texto="reserva.boton"></button>
    </form>
  </section>

</main>

<footer class="pie">
  <p data-texto="negocio.pie"></p>
  <a target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="contacto.usuarioInstagram"></a>
</footer>

<a class="flotante" target="_blank" rel="noopener" data-whatsapp>WhatsApp</a>

<script src="js/configuracion.js"></script>
<script src="js/principal.js"></script>
<script src="js/selector.js"></script>
</body>
</html>

```

### Archivo: `mineral.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Masajes y relajación</title>
<meta name="description" content="Centro estético de masajes y relajación. Reservá tu momento de calma.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Newsreader:ital,wght@0,300;0,400;1,300&family=Instrument+Sans:wght@400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="css/mineral.css">
<link rel="stylesheet" href="css/instalaciones.css">
</head>
<body>

<header class="cabecera">
  <a href="#inicio" class="marca" data-texto="negocio.nombre"></a>
  <button class="menu-boton" aria-label="Abrir menú" aria-expanded="false">Menú</button>
  <nav class="menu" id="menu">
    <a href="#servicios">Servicios</a>
    <a href="#galeria">Galería</a>
    <a href="#instalaciones">Instalaciones</a>
    <a href="#sobre-mi">Sobre mí</a>
    <a href="#cuidados">Tu visita</a>
    <a href="#contacto" class="boton-mini">Reservar</a>
  </nav>
</header>

<main>
  <section class="hero" id="inicio">
    <div class="hero-texto">
      <h1 data-texto="inicio.titulo"></h1>
      <p data-texto="inicio.texto"></p>
      <div class="acciones">
        <a href="#contacto" class="boton" data-texto="inicio.botonReserva"></a>
        <a href="#servicios" class="enlace" data-texto="inicio.botonServicios"></a>
      </div>
    </div>
  </section>

  <section class="seccion oscura" id="sobre-mi">
    <div class="dos-col">
      <div class="retrato" role="img"><span data-texto="sobreMi.textoFoto"></span></div>
      <div>
        <h2 data-texto="sobreMi.titulo"></h2>
        <div id="parrafos-sobre-mi"></div>
        <a class="boton claro" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="sobreMi.botonInstagram"></a>
      </div>
    </div>
  </section>

  <section class="seccion" id="instalaciones">
    <h2 data-texto="instalaciones.titulo"></h2>
    <p class="intro" data-texto="instalaciones.intro"></p>
    <div class="instalaciones cinta" id="lista-instalaciones"></div>
    <p class="nota" data-texto="instalaciones.nota"></p>
  </section>

  <section class="seccion" id="servicios">
    <h2 data-texto="servicios.titulo"></h2>
    <p class="intro" data-texto="servicios.intro"></p>
    <div class="servicios" id="lista-servicios"></div>
    <p class="nota" data-texto="servicios.nota"></p>
  </section>

  <section class="seccion" id="cuidados">
    <h2 data-texto="visita.titulo"></h2>
    <ol class="pasos" id="lista-pasos"></ol>
  </section>

  <section class="seccion" id="galeria">
    <h2 data-texto="galeria.titulo"></h2>
    <p class="intro" data-texto="galeria.intro"></p>
    <div class="galeria" id="lista-galeria"></div>
    <a class="boton" target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="galeria.boton"></a>
    <p class="nota" data-texto="galeria.nota"></p>
  </section>

  <section class="seccion" id="opiniones">
    <h2 data-texto="opiniones.titulo"></h2>
    <div class="opiniones" id="lista-opiniones"></div>
    <p class="nota" data-texto="opiniones.nota"></p>
  </section>

  <section class="seccion" id="contacto">
    <h2 data-texto="reserva.titulo"></h2>
    <p class="intro" data-texto="reserva.intro"></p>
    <form id="formulario" class="formulario" novalidate>
      <label><span data-texto="reserva.etiquetaNombre"></span>
        <input type="text" name="nombre" autocomplete="name" required>
      </label>
      <label><span data-texto="reserva.etiquetaServicio"></span>
        <select name="servicio" id="campo-servicio" required></select>
      </label>
      <label><span data-texto="reserva.etiquetaHorario"></span>
        <input type="text" name="horario" id="campo-horario">
      </label>
      <p class="error" id="error" role="alert" hidden></p>
      <button type="submit" class="boton" data-texto="reserva.boton"></button>
    </form>
  </section>

</main>

<footer class="pie">
  <p data-texto="negocio.pie"></p>
  <a target="_blank" rel="noopener" data-enlace="contacto.instagram" data-texto="contacto.usuarioInstagram"></a>
</footer>

<script src="js/configuracion.js"></script>
<script src="js/principal.js"></script>
<script src="js/selector.js"></script>
</body>
</html>

```

