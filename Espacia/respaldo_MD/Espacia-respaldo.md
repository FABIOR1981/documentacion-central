# Bundle del Proyecto para IA: Espacia

## Estructura de Archivos
- css/1_comunes.css
- css/2_abmusu.css
- css/2_agenda_v2.css
- css/2_dashboard.css
- css/2_informe.css
- css/2_login.css
- css/2_pdf_styles.css
- css/2_reservas.css
- dashboard.html
- documentacion/Manual_Administrador.pdf
- documentacion/Manual_Gestor.pdf
- documentacion/Manual_Usuario.pdf
- documentacion/img/a01_agenda.png
- documentacion/img/a02_agendando_para.png
- documentacion/img/a03_detalle.png
- documentacion/img/a04_agenda_bloqueo.png
- documentacion/img/a05_reservas.png
- documentacion/img/a06_informe.png
- documentacion/img/a07_bloqueo.png
- documentacion/img/a08_bloqueo_confirmar.png
- documentacion/img/a09_usuarios.png
- documentacion/img/a10_nuevo_usuario.png
- documentacion/img/a11_editar_usuario.png
- documentacion/img/a12_inactivar.png
- documentacion/img/g01_agenda.png
- documentacion/img/g02_agendando_para.png
- documentacion/img/g03_detalle.png
- documentacion/img/g04_agenda_bloqueo.png
- documentacion/img/g05_reservas.png
- documentacion/img/g06_informe.png
- documentacion/img/g07_bloqueo.png
- documentacion/img/g08_bloqueo_confirmar.png
- documentacion/img/g09_usuarios.png
- documentacion/img/g10_nuevo_usuario.png
- documentacion/img/g11_editar_usuario.png
- documentacion/img/g12_inactivar.png
- documentacion/img/u01_login.png
- documentacion/img/u02_cambio_pass.png
- documentacion/img/u03_agenda.png
- documentacion/img/u04_duracion.png
- documentacion/img/u05_confirmada.png
- documentacion/img/u06_detalle.png
- documentacion/img/u07_mis_reservas.png
- documentacion/img/u08_cancelar.png
- documentacion/img/u09_movil.png
- documentacion/img/u10_movil_menu.png
- documentacion/manual_admin.md
- documentacion/manual_gestor.md
- documentacion/manual_usuario.md
- documentacion/md-to-pdf.html
- documentacion/readme_extendido.md
- documentacion/requerimientos_detallados.md
- hash_sha256.html
- imagenes/icon-192.png
- imagenes/icon-512.png
- imagenes/logo_demaria.png
- index.html
- js/agenda/agenda_cancelar.js
- js/agenda/agenda_detalle.js
- js/agenda/agenda_reservar.js
- js/agenda/readme_contenido _agenda.md
- js/agenda_v2.js
- js/bloqueos.js
- js/components/timeindicator.js
- js/config.js
- js/config_particular.js
- js/config_respaldo.js
- js/dashboard.js
- js/informe_modular.js
- js/login_local.js
- js/modelo/reserva.js
- js/pdf/pdf_core.js
- js/pdf/pdf_reporte.js
- js/pdf/pdf_ticket_cancela.js
- js/pdf/pdf_ticket_reserva.js
- js/persistencia/abm_usuario.js
- js/reservas_futuras.js
- js/utils.js
- manifest.json
- netlify.toml
- netlify/functions/backup-reservas.js
- netlify/functions/change-password.js
- netlify/functions/get-usuarios.js
- netlify/functions/informe_canceladas.js
- netlify/functions/informe_reservas.js
- netlify/functions/listar_usuarios.js
- netlify/functions/login.js
- netlify/functions/package.json
- netlify/functions/reservar.js
- netlify/functions/update-usuarios.js
- netlify/functions/utils/backup/config.js
- netlify/functions/utils/config_netlify.js
- netlify/functions/utils/jwthelper.js
- netlify/functions/utils/validateauth0token.js
- netlify/functions/utils/validateidentitytoken.js
- package.json
- readme.md
- sw.js
- vscode-vfs-info.md

---

## Código Fuente

### Archivo: `css/1_comunes.css`
```css
/* =========================================
   VARIABLES Y CONFIGURACIÓN BASE
   ========================================= */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

:root {
    /* Tema Espartano (Actual) */
    --primary: #2c3e50;
    --accent: #3498db;
    --radius: 8px;
    --bg-zebra: #f2f4f6;
    --white: #ffffff;
    --border: #e9ecef;
    --text-main: #333333;
    --text-muted: #666666;
    --bg-body: #ffffff;
    --shadow-card: none;
    --font-family: sans-serif;
     /* Tema Espartano (Nuevo) */
     --primary: #2c3e50;
     --accent: #3498db;
     --radius: 8px;
     --bg-zebra: #f2f4f6;
     --white: #ffffff;
     --border: #e9ecef;
     --text-main: #333333;
     --text-muted: #666666;
     --bg-body: #ffffff;
     --shadow-card: none;
     --font-family: sans-serif;
}

/* Tema Moderno (Verde Bosque - Soft UI) */
body.theme-modern {
    --primary: #2E7D32;
    --primary-hover: #1B5E20;
    --accent: #27AE60;
    --radius: 12px;
    --radius-btn: 8px;
    --bg-zebra: #F9FAFB;
    --white: #ffffff;
    --border: #E5E7EB;
    --text-main: #1F2937;
    --text-muted: #6B7280;
    --bg-body: #E8F5E9; /* Verde pastel muy suave para el fondo general */
    --shadow-card: 0 10px 15px -3px rgba(0, 0, 0, 0.05), 0 4px 6px -2px rgba(0, 0, 0, 0.025);
    --shadow-input: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
    --font-family: 'Inter', sans-serif;
}

/* Tema Azul Acero (Profesional y Calmo) */
body.theme-blue {
    --primary: #4A90E2;
    --primary-hover: #357ABD;
    --accent: #2C3E50;
    --radius: 12px;
    --radius-btn: 8px;
    --bg-zebra: #F8FAFC;
    --white: #ffffff;
    --border: #E2E8F0;
    --text-main: #1E293B;
    --text-muted: #64748B;
    --bg-body: #F0F4F8; /* Azul pastel muy suave */
    --shadow-card: 0 10px 15px -3px rgba(0, 0, 0, 0.05), 0 4px 6px -2px rgba(0, 0, 0, 0.025);
    --shadow-input: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
    --font-family: 'Inter', sans-serif;
}

/* Tema Oscuro (Elegante) */
body.theme-dark {
    --primary: #8B5CF6; /* Púrpura suave */
    --primary-hover: #7C3AED;
    --accent: #10B981;
    --radius: 12px;
    --radius-btn: 8px;
    --bg-zebra: #1E293B;
    --white: #0F172A; /* Fondo de tarjetas oscuro */
    --border: #334155;
    --text-main: #F8FAFC;
    --text-muted: #94A3B8;
    --bg-body: #020617; /* Fondo general muy oscuro */
    --shadow-card: 0 10px 15px -3px rgba(0, 0, 0, 0.5), 0 4px 6px -2px rgba(0, 0, 0, 0.3);
    --shadow-input: 0 1px 2px 0 rgba(0, 0, 0, 0.3);
    --font-family: 'Inter', sans-serif;
}

/* Tema Oscuro Accesible (Especial Astigmatismo) */
body.theme-astigmatismo {
    --primary: #5C8374; /* Verde azulado de bajo brillo */
    --primary-hover: #4A6D60;
    --accent: #93B1A6;
    --radius: 12px;
    --radius-btn: 8px;
    --bg-zebra: #2C2C35; /* Contraste sutil para filas */
    --white: #22222A; /* Tarjetas en gris oscuro profundo, no negro */
    --border: #383846; /* Borde de bajo contraste */
    --text-main: #D9D9E3; /* Gris claro/blanco roto (evita el efecto halo) */
    --text-muted: #8F8F9D;
    --bg-body: #18181E; /* Fondo principal en gris muy oscuro, elimina reflejo */
    --shadow-card: 0 4px 15px rgba(0, 0, 0, 0.3);
    --shadow-input: 0 1px 2px 0 rgba(0, 0, 0, 0.2);
    --font-family: 'Inter', sans-serif;
}

/* Tema Cálido y Acogedor (Spa/Bienestar) */
body.theme-warm {
    --primary: #D97757; /* Terracota suave / Arcilla */
    --primary-hover: #C06040;
    --accent: #E6A57A;
    --radius: 16px; /* Bordes más redondeados para mayor suavidad */
    --radius-btn: 12px;
    --bg-zebra: #FAF6F0; /* Crema muy claro */
    --white: #FFFFFF;
    --border: #E8DFD5;
    --text-main: #4A3F35; /* Marrón oscuro en lugar de gris/negro */
    --text-muted: #8C7A6B;
    --bg-body: #F4EFEA; /* Arena/Beige muy suave */
    --shadow-card: 0 8px 20px -4px rgba(139, 115, 85, 0.15), 0 4px 8px -2px rgba(139, 115, 85, 0.1); /* Sombra con tinte cálido */
    --shadow-input: 0 2px 4px 0 rgba(139, 115, 85, 0.05);
    --font-family: 'Inter', sans-serif;
}

/* Tema Océano (Soft UI - Azul/Celeste) */
body.theme-ocean {
    --primary: #0284C7; /* Azul vibrante pero suave */
    --primary-hover: #0369A1;
    --accent: #38BDF8;
    --radius: 12px;
    --radius-btn: 8px;
    --bg-zebra: #F0F9FF; /* Celeste muy claro */
    --white: #ffffff;
    --border: #E0F2FE;
    --text-main: #0F172A;
    --text-muted: #475569;
    --bg-body: #E0F2FE; /* Celeste pastel para el fondo general */
    --shadow-card: 0 10px 15px -3px rgba(2, 132, 199, 0.05), 0 4px 6px -2px rgba(2, 132, 199, 0.025);
    --shadow-input: 0 1px 2px 0 rgba(2, 132, 199, 0.05);
    --font-family: 'Inter', sans-serif;
}

/* Tema Lavanda (Soft UI - Púrpura/Lila) */
body.theme-lavender {
    --primary: #7C3AED; /* Púrpura vibrante */
    --primary-hover: #6D28D9;
    --accent: #A78BFA;
    --radius: 12px;
    --radius-btn: 8px;
    --bg-zebra: #F5F3FF; /* Lila muy claro */
    --white: #ffffff;
    --border: #EDE9FE;
    --text-main: #1E1B4B;
    --text-muted: #5B21B6;
    --bg-body: #EDE9FE; /* Lila pastel para el fondo general */
    --shadow-card: 0 10px 15px -3px rgba(124, 58, 237, 0.05), 0 4px 6px -2px rgba(124, 58, 237, 0.025);
    --shadow-input: 0 1px 2px 0 rgba(124, 58, 237, 0.05);
    --font-family: 'Inter', sans-serif;
}

/* Tema Atardecer (Cálido - Naranja/Amarillo) */
body.theme-sunset {
    --primary: #EA580C; /* Naranja cálido */
    --primary-hover: #C2410C;
    --accent: #F97316;
    --radius: 16px;
    --radius-btn: 12px;
    --bg-zebra: #FFF7ED; /* Naranja muy claro */
    --white: #FFFFFF;
    --border: #FFEDD5;
    --text-main: #431407;
    --text-muted: #9A3412;
    --bg-body: #FFEDD5; /* Naranja pastel para el fondo general */
    --shadow-card: 0 8px 20px -4px rgba(234, 88, 12, 0.15), 0 4px 8px -2px rgba(234, 88, 12, 0.1);
    --shadow-input: 0 2px 4px 0 rgba(234, 88, 12, 0.05);
    --font-family: 'Inter', sans-serif;
}

/* Tema Minimalista (Gris/Beige - Elegante y Neutro) */
body.theme-minimalist {
    --primary: #57534E; /* Gris cálido oscuro */
    --primary-hover: #44403C;
    --accent: #A8A29E;
    --radius: 12px;
    --radius-btn: 8px;
    --bg-zebra: #F5F5F4; /* Gris cálido muy claro */
    --white: #FFFFFF;
    --border: #E7E5E4;
    --text-main: #292524;
    --text-muted: #78716C;
    --bg-body: #FAFAF9; /* Fondo casi blanco, muy neutro */
    --shadow-card: 0 10px 15px -3px rgba(87, 83, 78, 0.05), 0 4px 6px -2px rgba(87, 83, 78, 0.025);
    --shadow-input: 0 1px 2px 0 rgba(87, 83, 78, 0.05);
    --font-family: 'Inter', sans-serif;
}

/* Tema Mostaza (Luminoso - Amarillo Pastel/Mostaza) */
body.theme-mustard {
    --primary: #CA8A04; /* Mostaza vibrante */
    --primary-hover: #A16207;
    --accent: #EAB308;
    --radius: 16px;
    --radius-btn: 12px;
    --bg-zebra: #FEFCE8; /* Amarillo muy claro */
    --white: #FFFFFF;
    --border: #FEF08A;
    --text-main: #422006;
    --text-muted: #854D0E;
    --bg-body: #FEF9C3; /* Amarillo pastel para el fondo general */
    --shadow-card: 0 8px 20px -4px rgba(202, 138, 4, 0.15), 0 4px 8px -2px rgba(202, 138, 4, 0.1);
    --shadow-input: 0 2px 4px 0 rgba(202, 138, 4, 0.05);
    --font-family: 'Inter', sans-serif;
}

/* Tema Estilo Pisama (Clínico, Limpio, Salud Mental) */
body.theme-pisama {
    --primary: #1A4D42; /* Verde oscuro/azulado profundo (típico de salud/bienestar) */
    --primary-hover: #133A31;
    --accent: #D4A373; /* Un toque cálido/madera para contrastar la frialdad clínica */
    --radius: 12px;
    --radius-btn: 8px;
    --bg-zebra: #F8F9FA; /* Gris extremadamente claro */
    --white: #FFFFFF;
    --border: #E9ECEF;
    --text-main: #212529; /* Gris muy oscuro, casi negro, alta legibilidad */
    --text-muted: #6C757D;
    --bg-body: #FAFAFA; /* Blanco roto/Gris perla muy sutil para no saturar la vista */
    --shadow-card: 0 10px 30px -5px rgba(0, 0, 0, 0.08); /* Sombra muy amplia y difuminada (elevación moderna) */
    --shadow-input: 0 2px 5px rgba(0, 0, 0, 0.04);
    --font-family: 'Inter', sans-serif;
}

body {
    font-family: var(--font-family);
    color: var(--text-main);
    background-color: var(--bg-body);
    transition: background-color 0.3s ease, color 0.3s ease;
    letter-spacing: -0.01em;
    margin: 0;
    padding: 0;
    /* overflow: hidden; -> Comentado para permitir scroll vertical natural en móviles */
}

/* =========================================
   TÍTULOS DE MÓDULOS (ESTILO UNIFICADO)
   ========================================= */
.module-title {
    color: var(--primary) !important;
    font-size: 1.8rem !important;
    margin: 0 !important;
    border-left: 5px solid var(--accent) !important;
    padding-left: 15px !important;
    font-weight: 600 !important;
}

/* =========================================
   ESTRUCTURA DE CONTENEDORES (NUEVO LAYOUT SIDEBAR)
   ========================================= */
.dashboard-container {
    display: flex;
    min-height: 100vh;
    max-width: 100%;
    margin: 0 auto;
    padding: 0; /* Removido para que content maneje su propio respiro */
    gap: 0; /* Removido gap global */
    align-items: flex-start;
}

.container, 
.informe-container {
    max-width: 100%; 
    margin: 0 auto; 
    padding: 0 20px;
}

/* =========================================
   SIDEBAR IZQUIERDO
   ========================================= */
.sidebar {
    width: 200px;
    display: flex;
    flex-direction: column;
    gap: 15px;
    position: sticky;
    top: 20px;
    padding-top: 10px;
}

.sidebar-user-header {
    margin-bottom: 5px;
}

#user-email {
    font-weight: 600;
    color: var(--primary);
    font-size: 1rem;
    word-break: break-word;
}

.sidebar-logout {
    padding: 8px 15px;
    background-color: transparent;
    border: 1px solid var(--border);
    color: var(--text-main);
    border-radius: var(--radius-btn);
    cursor: pointer;
    font-size: 0.85rem;
    transition: all 0.2s ease;
    align-self: flex-start;
}

.sidebar-logout:hover {
    background-color: var(--bg-zebra);
}

/* =========================================
   CONTENIDO PRINCIPAL Y HEADER
   ========================================= */
.content {
    flex: 1;
    min-width: 0;
}

.main-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 25px;
    padding-bottom: 15px;
}

#welcome-msg {
    margin: 0;
    font-size: 2rem;
    color: var(--text-main);
}

.logo-top-right {
    display: flex; 
    align-items: center; 
    justify-content: center; 
    height: 50px; 
}

/* =========================================
   BOTONERA TIPO TABS (NUEVO ESTILO)
   ========================================= */
.tabs-nav {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    border-bottom: 1px solid var(--border);
    padding-bottom: 15px;
    margin-bottom: 20px;
}

.btn-tab {
    padding: 8px 16px;
    background-color: var(--primary);
    color: white;
    border: none;
    border-radius: 20px; /* Estilo píldora */
    cursor: pointer;
    font-size: 0.9rem;
    font-weight: 500;
    transition: all 0.2s ease;
    opacity: 0.7; /* Inactivos un poco apagados */
}

.btn-tab:hover {
    opacity: 0.9;
    transform: translateY(-1px);
}

/* Para resaltar el botón activo (se manejará en JS, pero dejamos la clase) */
.btn-tab.active {
    opacity: 1;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

.btn, .btn-menu {
    padding: 10px 18px;
    border: none;
    background-color: var(--primary);
    color: white;
    border-radius: var(--radius-btn, var(--radius));
    cursor: pointer;
    font-weight: 500;
    transition: all 0.2s ease;
    box-shadow: var(--shadow-input, none);
}

.btn:hover, .btn-menu:hover {
    background-color: var(--primary-hover, var(--primary));
    opacity: 0.9;
    transform: translateY(-1px);
}

/* =========================================
   TARJETAS (CARDS)
   ========================================= */
.card {
    background: var(--white);
    border-radius: var(--radius);
    box-shadow: var(--shadow-card);
    padding: 20px;
    margin-bottom: 20px;
    border: 1px solid var(--border);
    transition: all 0.3s ease;
}

/* =========================================
   TABLAS Y EFECTO ZEBRA
   ========================================= */
.table-main-container {
    width: 100%;
    overflow-x: auto;
    margin-top: 20px;
    border-radius: var(--radius);
    box-shadow: var(--shadow-card);
    border: 1px solid var(--border);
    /* Ocultar scrollbar en navegadores webkit (Chrome, Safari) pero mantener funcionalidad */
    scrollbar-width: none; /* Firefox */
    -ms-overflow-style: none;  /* IE and Edge */
}

.table-main-container::-webkit-scrollbar {
    display: none; /* Chrome, Safari and Opera */
}

.custom-table, .table {
    width: 100%;
    border-collapse: collapse !important;
    background: var(--white);
    font-family: var(--font-family);
    margin-bottom: 0; /* Removido para que el contenedor maneje el margen */
    table-layout: fixed; /* Fuerza a respetar los anchos de columna */
}

.custom-table th, .table thead th {
    background-color: var(--bg-zebra);
    color: var(--text-muted);
    padding: 12px 8px; /* Reducido para ahorrar espacio */
    text-align: left;
    font-size: 0.8rem; /* Ligeramente más pequeño */
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.02em; /* Reducido */
    border-bottom: 1px solid var(--border);
    overflow: hidden;
    text-overflow: ellipsis;
}

.custom-table td, .table tbody td {
    padding: 12px 8px; /* Reducido para ahorrar espacio */
    border-bottom: 1px solid var(--border);
    font-size: 0.9rem; /* Ligeramente más pequeño */
    color: var(--text-main);
    white-space: normal; /* Permite que el texto largo (como nombres) se ajuste si es necesario */
    word-wrap: break-word; /* Asegura que el texto largo se rompa */
}

.custom-table tbody tr:hover, .table tbody tr:hover {
    background-color: var(--bg-zebra);
}

/* EFECTO ZEBRA CRÍTICO */
.custom-table tbody tr:nth-child(even) td,
.table tbody tr:nth-child(even) {
    background-color: var(--bg-zebra) !important;
}

.custom-table tbody tr:nth-child(odd) td,
.table tbody tr:nth-child(odd) {
    background-color: var(--white) !important;
}

/* =========================================
   FORMULARIOS (INFORME)
   ========================================= */
.informe-form {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    background-color: var(--white);
    padding: 25px;
    border-radius: var(--radius);
    align-items: flex-end;
    margin-bottom: 2rem;
    border: 1px solid var(--border);
}

.informe-form label {
    display: block;
    font-weight: 600;
    color: var(--primary);
    font-size: 0.9rem;
    margin-bottom: 5px;
}

.informe-form input, 
.informe-form select {
    padding: 10px;
    border: 1px solid var(--border);
    border-radius: 4px;
    min-width: 150px;
    background-color: var(--white);
    color: var(--text-main);
}

.informe-form input:focus, 
.informe-form select:focus {
    outline: none;
    border-color: var(--primary);
    box-shadow: 0 0 0 2px rgba(var(--primary), 0.2);
}

/* =========================================
   ADAPTACIÓN MÓVIL (Media Queries)
   ========================================= */
@media (max-width: 768px) {
    .dashboard-container {
        flex-direction: column;
        padding: 10px;
        gap: 15px;
    }
    
    .sidebar {
        width: 100%;
        position: relative;
        top: 0;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        border-bottom: 1px solid var(--border);
        padding-bottom: 10px;
    }
    
    .sidebar-user-header {
        margin-bottom: 0;
    }

    .main-header {
        flex-direction: column-reverse; /* Logo arriba, titulo abajo */
        gap: 15px;
        align-items: center;
        margin-bottom: 15px;
    }
    
    .logo-top-right {
        align-self: center;
    }
    
    #dashboard-btns {
        justify-content: center;
    }
    .container, .informe-container {
        max-width: 100vw;
        padding: 0 5px;
    }
    .custom-table, .table {
        font-size: 0.92rem;
        min-width: 500px;
    }
    .table-main-container {
        overflow-x: auto;
        -webkit-overflow-scrolling: touch;
    }
    .reserva-card, .informe-form, .login-container {
        padding: 1rem !important;
        min-width: unset;
        max-width: 100vw;
    }
    .informe-form {
        flex-direction: column;
        gap: 10px;
    }
    .btn, .btn-menu {
        width: auto;
    }
    .btn-tab {
        font-size: 0.85rem;
        padding: 6px 12px;
    }
    input, select {
        font-size: 1rem;
        min-width: 100px;
        width: 100%;
        box-sizing: border-box;
    }
}
```

### Archivo: `css/2_abmusu.css`
```css
/* =========================================
   MODAL DE USUARIOS
   ========================================= */
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
    backdrop-filter: blur(3px);
}

.modal-content {
    background: var(--white);
    padding: 30px;
    border-radius: var(--radius);
    width: 90%;
    max-width: 600px;
    box-shadow: var(--shadow-card, 0 15px 30px rgba(0,0,0,0.2));
    max-height: 90vh;
    overflow-y: auto;
}

.modal-content h3 {
    margin-top: 0;
    color: var(--primary);
    font-size: 1.5rem;
    margin-bottom: 20px;
    border-bottom: 2px solid var(--accent);
    padding-bottom: 10px;
}

/* =========================================
   ABM USUARIOS - DISEÑO PROFESIONAL
   ========================================= */

/* 1. CONTENEDOR PRINCIPAL (La "Tarjeta" blanca) */
.abmusu-section {
    background: var(--white);
    padding: 30px;
    border-radius: var(--radius);
    box-shadow: var(--shadow-card);
    max-width: 100%; /* Totalmente fluido */
    width: 100%; /* Ocupa todo el ancho disponible */
    margin: 20px auto;
    border: 1px solid var(--border);
    overflow: hidden; /* Evita que el contenido desborde el borde redondeado */
}

/* Contenedor responsivo para la tabla */
.table-responsive {
    overflow-x: auto;
    width: 100%;
    margin-top: 20px;
}

/* Títulos base si hicieran falta */
.abmusu-section h2 {
    margin-bottom: 25px;
}

/* 2. EL FORMULARIO (GRID DINÁMICO) */
/* Usamos !important en algunos casos para ganar a los estilos inline del JS */
.abmusu-form {
    display: grid !important; 
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)) !important;
    gap: 20px !important;
    max-width: 100% !important; /* Ignora el max-width: 600px del JS */
    background: var(--bg-zebra);
    padding: 25px;
    border-radius: var(--radius);
    margin-bottom: 40px;
}

/* Estilo de los grupos (Labels + Inputs) */
.abmusu-form div {
    display: flex;
    flex-direction: column;
}

.abmusu-form label {
    font-size: 0.85rem;
    font-weight: 700;
    color: var(--text-muted);
    margin-bottom: 8px;
    text-transform: uppercase;
}

/* Estilo de Inputs y Selects */
.abmusu-form input, 
.abmusu-form select {
    width: 100% !important; /* Ignora el max-width manual del JS */
    max-width: 100% !important;
    padding: 10px 12px;
    border: 1px solid var(--border);
    border-radius: var(--radius-btn, 6px);
    font-size: 0.95rem;
    background-color: var(--white);
    color: var(--text-main);
    transition: all 0.2s;
    box-sizing: border-box;
}

.abmusu-form input:focus {
    border-color: var(--accent);
    outline: none;
    box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
}

/* 3. BOTONES (Guardar y Cancelar) */
/* El contenedor de botones al final */
.abmusu-buttons-container {
    grid-column: 1 / -1; /* Ocupa todo el ancho */
    display: flex !important;
    flex-direction: row !important;
    justify-content: center;
    gap: 15px;
    margin-top: 10px;
    padding-top: 20px;
    border-top: 1px solid var(--border);
}

#guardar-btn {
    background: var(--primary);
    color: white;
    padding: 12px 30px;
    border: none;
    border-radius: var(--radius-btn, 6px);
    font-weight: 700;
    cursor: pointer;
    transition: background 0.3s;
}

#guardar-btn:hover { background: var(--primary-hover, var(--primary)); opacity: 0.9; }

#cancelar-btn {
    background: var(--bg-zebra);
    color: var(--text-muted);
    padding: 12px 30px;
    border: 1px solid var(--border);
    border-radius: var(--radius-btn, 6px);
    font-weight: 700;
    cursor: pointer;
}

#cancelar-btn:hover { background: var(--border); }

/* 4. LA TABLA (Basada en tu captura de F12) */
#usuarios-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
    border-radius: var(--radius);
    overflow: hidden;
    border: 1px solid var(--border);
}

#usuarios-table thead th {
    background-color: var(--bg-zebra);
    color: var(--text-muted);
    padding: 14px 16px;
    text-align: left;
    font-size: 0.85rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    border-bottom: 1px solid var(--border);
}

#usuarios-table tbody td {
    padding: 14px 16px;
    border-bottom: 1px solid var(--border);
    font-size: 0.95rem;
    color: var(--text-main);
    max-width: 200px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

/* Efecto cebra para la tabla */
#usuarios-table tbody tr:nth-child(even) {
    background-color: var(--bg-zebra);
}

/* Botones dentro de la tabla más compactos */
#usuarios-table button {
    padding: 6px 10px;
    font-size: 0.8rem;
    border-radius: var(--radius-btn, 4px);
    cursor: pointer;
    margin-right: 4px;
    border: none;
    background-color: var(--primary);
    color: white;
    font-weight: 500;
    transition: opacity 0.2s;
}

#usuarios-table button[data-edit]:hover {
    opacity: 0.9;
}

#usuarios-table button[data-baja]:hover {
    opacity: 0.9;
}
```

### Archivo: `css/2_agenda_v2.css`
```css
.booking-block.past {
    background-color: color-mix(in srgb, var(--booking-bg, #ccc) 45%, #fff 55%) !important;
    /* Solo el fondo se atenúa, el texto no se altera */
    opacity: 1;
    filter: none;
    transition: background-color 0.2s;
}
/* Time Indicator — Línea de tiempo actual */
#time-indicator {
    position: absolute;
    left: 0;
    width: 100%;
    height: 1.5px;
    background: red;
    z-index: 10;
    pointer-events: none;
}
#time-indicator-label {
    position: absolute;
    left: 0;
    top: -8px;
    width: 48px;
    height: 18px;
    background: white;
    border-radius: 9px;
    border: 1px solid red;
    color: red;
    font-size: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 1px 4px rgba(0,0,0,0.08);
}
/* css/2_agenda_v2.css */
.agenda-v2-container {
    padding: 20px;
    background: var(--white, #fff);
    border-radius: var(--radius, 12px);
    box-shadow: var(--shadow-card, 0 4px 6px rgba(0,0,0,0.1));
    font-family: var(--font-family, 'Inter', sans-serif);
    color: var(--text-main, #333);
    margin-top: 20px;
}

.agenda-v2-header {
    /* display: flex; flex-direction: column; align-items: center; margin-bottom: 20px; */
    /* Ahora se maneja inline en el JS para ser más compacto */
}

.agenda-v2-title {
    /* font-size: 2rem; font-weight: bold; margin-bottom: 15px; color: var(--primary, #2c3e50); */
    /* Ahora se maneja inline en el JS para ser más compacto */
    font-weight: bold;
    color: var(--primary, #2c3e50);
}

.agenda-v2-controls {
    display: flex;
    align-items: center;
    gap: 10px;
    flex-wrap: wrap;
    justify-content: center;
}

.agenda-v2-nav-btn {
    background: var(--white, #fff);
    border: 1px solid var(--border, #ccc);
    padding: 8px 15px;
    border-radius: var(--radius-btn, 8px);
    cursor: pointer;
    font-weight: bold;
    transition: all 0.2s;
    color: var(--text-main, #333);
}

.agenda-v2-nav-btn:hover {
    background: var(--bg-zebra, #f5f5f5);
}

.agenda-v2-date-picker {
    padding: 8px 12px;
    border: 1px solid var(--border, #ccc);
    border-radius: var(--radius-btn, 8px);
    font-family: inherit;
    font-size: 1rem;
    color: var(--text-main, #333);
    background: var(--white, #fff);
}

.agenda-v2-legend {
    display: flex;
    gap: 15px;
    margin-bottom: 10px;
    padding: 8px 15px;
    background: var(--bg-zebra, #f8f9fa);
    border-radius: var(--radius, 8px);
    border: 1px solid var(--border, #eee);
    font-size: 0.85rem;
    flex-wrap: wrap;
    justify-content: center;
}

.legend-item {
    display: flex;
    align-items: center;
    gap: 8px;
}

.legend-color {
    width: 16px;
    height: 16px;
    border-radius: 4px;
}

.legend-color.fija { background: var(--accent, #7ED321); }
.legend-color.eventual { background: var(--primary, #4A90E2); }
.legend-color.ajena { background: #D1D5DB; }
.legend-color.bloqueo { background: #E74C3C; }

/* Grid Layout */
.agenda-v2-grid-wrapper {
    overflow-x: auto;
    border: 1px solid var(--border, #e0e0e0);
    border-radius: var(--radius, 8px);
    background: var(--white, #fff);
    min-height: calc(100vh - 160px); /* Forzar a que ocupe todo el espacio libre de la tarjeta */
    position: relative; /* Importante para que los bloques absolutos se posicionen respecto a este contenedor */
}

.agenda-v2-grid {
    display: grid;
    /* Columns will be set dynamically via JS based on number of consultorios */
    min-width: 100%;
    position: relative; /* Necesario para los bloques de reserva */
}

.grid-header-cell {
    background: var(--bg-zebra, #f8f9fa);
    padding: 15px 10px;
    text-align: center;
    font-weight: bold;
    font-size: 1.15rem; /* Aumentado el tamaño del nombre de los consultorios */
    border-bottom: 2px solid #9ca3af; /* Gris más oscuro para separar cabecera */
    border-right: 1px solid #cbd5e1; /* Gris medio */
    position: sticky;
    top: 0;
    z-index: 20; /* Aumentado para que quede por encima de las reservas (z-index: 3 y 4) */
    color: var(--text-main);
}

.grid-time-cell {
    background: var(--bg-zebra, #f8f9fa);
    padding: 10px;
    text-align: center;
    font-weight: bold;
    font-size: 1.05rem; /* Aumentado sutilmente el tamaño de la columna de las horas para mantener la proporción */
    border-right: 2px solid #9ca3af; /* Gris más oscuro para la columna lateral */
    border-bottom: 1px dashed #cbd5e1;
    position: sticky;
    left: 0;
    z-index: 15; /* Aumentado para que quede por encima de las reservas al hacer scroll horizontal */
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--text-main);
}

.grid-time-cell.half-hour {
    border-bottom: 1px solid #6b7280; /* Línea compañera para cerrar la hora en punto en la columna izquierda */
}

.grid-slot {
    border-right: 1px solid #9ca3af; /* Línea vertical más oscura (Gris medio-oscuro) */
    border-bottom: 1px dashed #cbd5e1; /* Por defecto (los minutos 00 a 30) es punteada */
    min-height: 40px;
    position: relative;
    background: var(--white, #fff);
    transition: background 0.2s;
    z-index: 1;
}

.grid-slot.half-hour {
    border-bottom: 1px solid #6b7280; /* Línea separadora de "hora en punto" AÚN MÁS OSCURA */
}

.grid-slot:hover {
    background: var(--bg-zebra, #f1f5f9);
}

.grid-slot.has-booking {
    background: transparent; /* The booking block will cover it */
}

.grid-slot.is-past {
    background: var(--bg-zebra);
    opacity: 0.7;
}

/* Booking Blocks */
.booking-block {
    border-radius: 6px;
    padding: 4px;
    font-size: 0.85rem;
    color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
    overflow: hidden;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    z-index: 3;
    cursor: pointer;
    transition: transform 0.1s;
}

.booking-block:hover {
    transform: scale(1.02);
    z-index: 4;
}

.booking-block.fija { background: var(--accent, #7ED321); }
.booking-block.eventual { background: var(--primary, #4A90E2); }
.booking-block.ajena { background: #9CA3AF; color: #333; }
.booking-block.bloqueo { background: #E74C3C; }

.booking-title {
    font-weight: bold;
    font-size: 1rem;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.booking-time {
    font-size: 0.85rem;
    opacity: 0.9;
}

/* Action Button on Hover */
.slot-action-btn {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: var(--primary, #2c3e50);
    color: white;
    border: none;
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 0.8rem;
    font-weight: bold;
    cursor: pointer;
    opacity: 0;
    transition: opacity 0.2s;
    z-index: 5;
    pointer-events: none;
    box-shadow: 0 2px 5px rgba(0,0,0,0.2);
}

.grid-slot:hover .slot-action-btn {
    opacity: 1;
    pointer-events: auto;
}

/* Modal Styles */
.modal-overlay {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background: rgba(0,0,0,0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10000;
}

.modal-content {
    background: var(--white, #fff);
    padding: 25px;
    border-radius: var(--radius, 12px);
    width: 90%;
    max-width: 400px;
    box-shadow: var(--shadow-card, 0 10px 25px rgba(0,0,0,0.2));
}

.modal-title {
    margin-top: 0;
    color: var(--primary, #333);
    margin-bottom: 15px;
}

.modal-close {
    background: #ccc;
    border: none;
    padding: 8px 15px;
    border-radius: 4px;
    cursor: pointer;
    margin-top: 15px;
}

.modal-confirm {
    background: var(--primary, #2E7D32);
    color: white;
    border: none;
    padding: 8px 15px;
    border-radius: 4px;
    cursor: pointer;
    margin-top: 15px;
    margin-left: 10px;
}

/* =========================================
   VISTA MÓVIL (PESTAÑAS DE CONSULTORIO)
   ========================================= */
.hide-on-desktop {
    display: none !important;
}

@media (max-width: 768px) {
    .hide-on-desktop {
        display: flex !important;
    }

    /* Contenedor de pestañas */
    .agenda-mobile-tabs {
        margin-bottom: 15px;
        gap: 8px;
        overflow-x: auto;
        padding-bottom: 5px;
        scrollbar-width: none; /* Oculta la barra en Firefox para mantenerlo limpio */
        -ms-overflow-style: none; /* IE/Edge */
        /* Importante: para que no expanda el ancho de la tarjeta real empujando todo */
        max-width: 100%;
        display: flex !important;
        flex-wrap: nowrap; /* Fuerza a que queden en una línea para que el scroll horizontal funcione solo en esa barra */
    }
    
    .agenda-mobile-tabs::-webkit-scrollbar {
        display: none; /* Oculta barra en Webkit (Chrome, Safari, móvil nativo) */
    }

    .mobile-tab-btn {
        padding: 10px 18px;
        background: var(--bg-zebra, #f0f0f0);
        border: 1px solid var(--border, #ccc);
        border-radius: 20px; /* Estilo píldora */
        font-weight: 600;
        white-space: nowrap; /* Evita que el texto de los botones se rompa en dos líneas */
        flex-shrink: 0; /* EVITA QUE LOS BOTONES SE EXPRIMAN, manteniendo su tamaño original para poder deslizarlos */
        cursor: pointer;
        color: var(--text-muted, #666);
        transition: all 0.2s ease;
    }

    .mobile-tab-btn.active {
        background: var(--primary);
        color: white;
        border-color: var(--primary);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.15); /* Un poco de realce al botón seleccionado */
        transform: scale(1.05); /* Ligeramente más grande */
    }

    .mobile-tab-btn:not(.active):hover {
        background: var(--border, #e9ecef);
        color: var(--text-main, #333);
    }

    /* Forzar a la grilla a ser solo de 1 columna de horas y 1 columna de datos */
    .agenda-v2-grid {
        grid-template-columns: 60px 1fr !important;
    }

    /* Ocultar ABSOLUTAMENTE TODOS los consultorios por defecto */
    .agenda-v2-grid .grid-header-cell[data-cons],
    .agenda-v2-grid .grid-slot[data-cons],
    .agenda-v2-grid .booking-block[data-cons] {
        display: none !important;
    }

    /* GENERADOR DINÁMICO DE VISIBILIDAD */
    /* Muestra la cabecera y el casillero vacío del consorio activo */
    .agenda-v2-grid[data-active-mobile-cons="1"] .grid-header-cell[data-cons="1"],
    .agenda-v2-grid[data-active-mobile-cons="1"] .grid-slot[data-cons="1"],
    .agenda-v2-grid[data-active-mobile-cons="2"] .grid-header-cell[data-cons="2"],
    .agenda-v2-grid[data-active-mobile-cons="2"] .grid-slot[data-cons="2"],
    .agenda-v2-grid[data-active-mobile-cons="3"] .grid-header-cell[data-cons="3"],
    .agenda-v2-grid[data-active-mobile-cons="3"] .grid-slot[data-cons="3"],
    .agenda-v2-grid[data-active-mobile-cons="4"] .grid-header-cell[data-cons="4"],
    .agenda-v2-grid[data-active-mobile-cons="4"] .grid-slot[data-cons="4"],
    .agenda-v2-grid[data-active-mobile-cons="5"] .grid-header-cell[data-cons="5"],
    .agenda-v2-grid[data-active-mobile-cons="5"] .grid-slot[data-cons="5"],
    .agenda-v2-grid[data-active-mobile-cons="6"] .grid-header-cell[data-cons="6"],
    .agenda-v2-grid[data-active-mobile-cons="6"] .grid-slot[data-cons="6"],
    .agenda-v2-grid[data-active-mobile-cons="7"] .grid-header-cell[data-cons="7"],
    .agenda-v2-grid[data-active-mobile-cons="7"] .grid-slot[data-cons="7"],
    .agenda-v2-grid[data-active-mobile-cons="8"] .grid-header-cell[data-cons="8"],
    .agenda-v2-grid[data-active-mobile-cons="8"] .grid-slot[data-cons="8"],
    .agenda-v2-grid[data-active-mobile-cons="9"] .grid-header-cell[data-cons="9"],
    .agenda-v2-grid[data-active-mobile-cons="9"] .grid-slot[data-cons="9"],
    .agenda-v2-grid[data-active-mobile-cons="10"] .grid-header-cell[data-cons="10"],
    .agenda-v2-grid[data-active-mobile-cons="10"] .grid-slot[data-cons="10"] {
        display: block !important;
        grid-column: 2 !important; /* Fuerza a pintar todo en la columna 2 del móvil */
    }

    /* Tratamiento especial para las reservas (booking block) porque son Flex, no Block */
    .agenda-v2-grid[data-active-mobile-cons="1"] .booking-block[data-cons="1"],
    .agenda-v2-grid[data-active-mobile-cons="2"] .booking-block[data-cons="2"],
    .agenda-v2-grid[data-active-mobile-cons="3"] .booking-block[data-cons="3"],
    .agenda-v2-grid[data-active-mobile-cons="4"] .booking-block[data-cons="4"],
    .agenda-v2-grid[data-active-mobile-cons="5"] .booking-block[data-cons="5"],
    .agenda-v2-grid[data-active-mobile-cons="6"] .booking-block[data-cons="6"],
    .agenda-v2-grid[data-active-mobile-cons="7"] .booking-block[data-cons="7"],
    .agenda-v2-grid[data-active-mobile-cons="8"] .booking-block[data-cons="8"],
    .agenda-v2-grid[data-active-mobile-cons="9"] .booking-block[data-cons="9"],
    .agenda-v2-grid[data-active-mobile-cons="10"] .booking-block[data-cons="10"] {
        display: flex !important;
        grid-column: 2 !important;
    }
}

```

### Archivo: `css/2_dashboard.css`
```css
/* CSS Específico para el Layout Lateral Total (Dashboard2) */
.dashboard-container {
    display: flex;
    min-height: 100vh;
    max-width: 100%; /* Ocupa todo el ancho */
    padding: 0;
    margin: 0;
    gap: 0;
    background-color: var(--bg-body); /* Fondo global detrás del sidebar y content */
}

.sidebar {
    width: 250px;
    background-color: var(--white);
    border-right: 1px solid var(--border);
    padding: 25px 15px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    position: fixed; /* Fijo a la izquierda */
    height: calc(100vh - 25px); /* Restamos el top para el alto total */
    top: 25px; /* Empieza un poco más abajo para formar el margen superior global */
    border-top-right-radius: 15px; /* Redondeamos solo la esquina superior derecha del sidebar para que concuerde visualmente */
    overflow-y: auto;
    z-index: 100;
}

/* Estructura del contenido desplazado por el sidebar fijo */
.content {
    flex: 1;
    margin-left: 250px; /* Igual al ancho del sidebar */
    padding: 25px 25px; /* Margin superior coincidente con el del sidebar */
    max-width: calc(100% - 250px);
    background-color: transparent; /* El color de fondo global ya está en .dashboard-container */
    height: 100vh;
    overflow-y: auto; /* El scroll ahora vive en el contenido principal exclusivamente */
    box-sizing: border-box; /* Para que el padding no rompa la altura total */
}

/* En el nuevo diseño de sidebar fijo, no aplicamos margen extra superior para no romper el alineamiento */
.content > .card {
    margin-top: 0 !important;
    min-height: calc(100vh - 50px); /* Hace que la tarjeta ocupe el alto completo menos el padding */
    display: flex;
    flex-direction: column;
}

.main-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 30px;
    padding-bottom: 20px;
    border-bottom: 1px solid var(--border);
}

/* Botones de navegación en forma de lista vertical */
.tabs-nav {
    display: flex;
    flex-direction: column;
    gap: 8px;
    border-bottom: none;
    padding-bottom: 0;
    margin-bottom: 0;
    margin-top: 10px;
    flex-grow: 1;
}

.btn-tab {
    padding: 12px 15px;
    background-color: transparent;
    color: var(--text-main);
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-size: 0.95rem;
    font-weight: 500;
    text-align: left;
    transition: all 0.2s ease;
    opacity: 0.8;
    display: flex;
    align-items: center;
}

.btn-tab:hover {
    background-color: var(--bg-zebra);
    opacity: 1;
    transform: none; /* Quitamos salto vertical */
}

.btn-tab.active {
    opacity: 1;
    background-color: var(--primary);
    color: white;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

.sidebar-logout {
    width: 100%;
    text-align: center;
    opacity: 1;
    display: block;
}

.container, .informe-container {
    max-width: 100%;
    margin: 0;
    padding: 0;
}

/* --- MENÚ MÓVIL Y HAMBURGUESA --- */
.mobile-top-bar {
    display: none; /* Oculto en PC */
    background-color: var(--white);
    padding: 10px 15px;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid var(--border);
    position: sticky;
    top: 0;
    z-index: 1000;
    box-shadow: 0 2px 5px rgba(0,0,0,0.05);
}

.hamburger-btn {
    background: none;
    border: none;
    font-size: 1.8rem;
    color: var(--text-main);
    cursor: pointer;
    padding: 5px;
}

.mobile-logo {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 40px;
}

.sidebar-overlay {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.4);
    z-index: 99;
}

@media (max-width: 768px) {
    .mobile-top-bar {
        display: flex;
    }
    .dashboard-container {
        flex-direction: column;
    }
    .sidebar {
        /* Transformar a Menú Desplegable (Lateral) */
        position: fixed;
        left: -300px; /* Escondido por defecto */
        top: 61px; /* Para que empiece justo debajo de la barra superior */
        width: 250px;
        height: calc(100vh - 61px); /* Ajustado para no sobrepasar la pantalla */
        z-index: 101; /* Arriba del overlay, pero bajo la barra (z-index 1000) */
        background-color: var(--white);
        border-top-right-radius: 0;
        transition: left 0.3s ease;
        padding: 10px 10px; /* Eliminar excesos de padding */
        padding-bottom: 80px; /* Darle respiro abajo más grande por el botón flotante */
        box-sizing: border-box; /* Previene que padding infle la altura */
        overflow-y: auto; /* Permite scroll si la pantalla es enana */
        box-shadow: 2px 0 10px rgba(0,0,0,0.1);
        gap: 0 !important; /* Quitar gap global para controlar el espaciado manual */
    }
    .sidebar.open {
        left: 0; /* Aparece */
    }
    .sidebar-overlay.active {
        display: block; /* Muestra fondo semitransparente */
    }
    .tabs-nav {
        width: 100%;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: flex-start;
        flex-grow: 0; /* Evitar que los botones se separen exageradamente */
        gap: 0; /* Sin hueco entre botones */
        margin-top: 5px;
        margin-bottom: 5px;
    }
    
    .btn-tab {
        text-align: left;
        justify-content: flex-start;
        padding: 10px 15px; /* Botones apenas más finos */
        font-size: 0.95rem;
    }
    .content {
        margin-left: 0;
        padding: 10px 10px; /* Menos padding en móvil para aprovechar al máximo */
        max-width: 100%;
        height: auto; /* Restablecer en movil */
        overflow-y: visible;
    }

    #desktop-logo {
        display: none !important; /* Ocultamos el logo del sidebar porque ya está en la barra superior */
    }
    .sidebar-user-header {
        padding-top: 5px !important;
        padding-bottom: 10px !important;
        margin-bottom: 5px !important;
    }
}
```

### Archivo: `css/2_informe.css`
```css
/* ==========================================================================
   SISTEMA DE DISEÑO ESPACIA - MÓDULO DE INFORMES
   ========================================================================== */

/* No redefinir :root para evitar collisions con el tema global; reutilizar variables de 1_comunes.css */

.informe-module {
    /* Vars específicas de este módulo si se necesita anular localmente */
}

/* Variables del módulo Informe (no redefinir :root) */
.informe-container {
    --score-blue: linear-gradient(135deg, #1e88e5 0%, #64b5f6 100%);
    --score-amber: linear-gradient(135deg, #f59e0b 0%, #fbbf24 100%);
    --score-green: linear-gradient(135deg, #388e3c 0%, #81c784 100%);
    --score-red: linear-gradient(135deg, #d32f2f 0%, #e57373 100%);
    --score-shadow: 0 4px 12px rgba(0,0,0,0.08);
    --radius-lg: 12px;

    /* Colores para badges (fáciles de ajustar) */
    --badge-blue: #e0f2fe; --badge-blue-text: #0369a1;
    --badge-amber: #fff7ed; --badge-amber-text: #92400e;
    --badge-green: #dcfce7; --badge-green-text: #15803d;
    --badge-red: #fee2e2; --badge-red-text: #b91c1c;
}

/* --- Layout del Grid --- */
.scorecard-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    margin-bottom: 18px;
}

/* --- Tarjetas de Estadísticas (Scorecards) --- */
.card-stat {
    background: var(--score-blue);
    color: #fff;
    border-radius: var(--radius-lg);
    box-shadow: var(--score-shadow);
    padding: 10px 14px;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    justify-content: center;
}
    .card-stat.green {
        background: var(--score-green);
    }
    .card-stat.red {
        background: var(--score-red);
    }
    .card-stat.amber {
        background: var(--score-amber);
    }
    /* Paleta de colores para Scorecards (variables definidas en .informe-container) */
    .scorecard-grid {
        /* variables tomadas desde .informe-container */
    }
/* Contenido Principal de la Tarjeta */
.card-stat-main {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 0;
}

.card-stat-title {
    font-size: 1.15rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.8px;
    opacity: 0.9;
    margin-bottom: -2px;
}

.card-stat-time {
    font-size: 1.6rem;
    font-weight: 800;
    line-height: 1.05;
    letter-spacing: -0.5px;
}

.card-stat-count {
    font-size: 0.85rem;
    font-weight: 500;
    opacity: 0.85;
    margin-top: 2px;
}

/* Desglose Lateral (60m / 90m) */
.card-stat-desglose {
    flex: 0 0 auto;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    justify-content: center;
    border-left: 1px solid rgba(255, 255, 255, 0.22);
    padding-left: 8px;
    margin-left: 6px;
    font-family: 'SFMono-Regular', Consolas, monospace;
    font-size: 0.9rem;
    line-height: 1.2;
}

/* Compact layout: put time and desglose in same row */
.card-stat .card-stat-time, .card-stat .card-stat-desglose {
    display: inline-block;
    vertical-align: middle;
}
.card-stat .card-stat-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 8px;
}

/* --- Formulario de Filtros --- */
.informe-form {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    background: var(--white);
    padding: 20px;
    border-radius: var(--radius-lg);
    box-shadow: 0 1px 3px rgba(0,0,0,0.05);
    border: 1px solid var(--border);
    margin-bottom: 24px;
    align-items: center;
}

.informe-form label {
    color: var(--primary);
    font-weight: 600;
    font-size: 0.9rem;
}

.informe-form input, 
.informe-form select {
    padding: 8px 12px;
    border: 1px solid #cbd5e1;
    border-radius: 6px;
    font-size: 0.9rem;
    transition: border-color 0.2s;
}

.informe-form input:focus {
    border-color: var(--primary);
    outline: none;
    box-shadow: 0 0 0 3px rgba(211, 84, 0, 0.1);
}

/* --- Tabla de Resultados --- */
.table-main-container {
    overflow-x: auto;
    background: var(--white);
    border-radius: var(--radius-lg);
    border: 1px solid var(--border);
}

.custom-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.95rem;
}

.custom-table th {
    background: #f8fafc;
    color: #64748b;
    font-weight: 600;
    text-align: left;
    padding: 12px 16px;
    border-bottom: 2px solid var(--border);
}

.custom-table td {
    padding: 12px 16px;
    border-bottom: 1px solid var(--border);
    color: var(--text-main);
}

/* Badges de Estado */
.status-badge {
    padding: 4px 10px;
    border-radius: 20px;
    font-size: 0.75rem;
    font-weight: 700;
    text-transform: uppercase;
}
.status-badge.pendiente { background: var(--badge-blue); color: var(--badge-blue-text); }
.status-badge.consumida { background: var(--badge-green); color: var(--badge-green-text); }
/* backward compatibility: keep `.reservada` and `.usada` as aliases for older code */
.status-badge.reservada { background: var(--badge-blue); color: var(--badge-blue-text); }
.status-badge.usada { background: var(--badge-green); color: var(--badge-green-text); }
.status-badge.cancelada { background: var(--badge-red); color: var(--badge-red-text); }

/* --- Paginación --- */
.pagination {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
}

.pagination button {
    padding: 8px 16px;
    border-radius: 6px;
    border: 1px solid var(--border);
    background: var(--white);
    cursor: pointer;
    font-weight: 500;
}

.pagination button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

/* --- Responsividad --- */
@media (max-width: 900px) {
    .scorecard-grid { grid-template-columns: 1fr; gap: 10px; }
    .informe-form { flex-direction: column; align-items: stretch; }
    .card-stat-time { font-size: 1.2rem; }
    .card-stat-desglose { font-size: 0.85rem; }
}

@media (max-width: 480px) {
    .card-stat { padding: 8px 10px; }
    .card-stat-time { font-size: 1rem; }
}
```

### Archivo: `css/2_login.css`
```css
/* 2_login.css - Estilos para pantalla de login */
body {
    background: var(--bg-body, linear-gradient(120deg, #e0eafc 0%, #cfdef3 100%));
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: var(--font-family, 'Segoe UI', Arial, sans-serif);
}
.login-container {
    background: var(--white, #fff);
    padding: 2.5rem 2.5rem;
    border-radius: var(--radius, 12px);
    box-shadow: var(--shadow-card, 0 10px 25px rgba(0,0,0,0.08)); /* Sombra más pronunciada para destacar sobre el fondo pastel */
    min-width: 320px;
    max-width: 380px;
    width: 100%;
    text-align: center;
    border: 1px solid var(--border, transparent);
    transition: all 0.3s ease;
}
.login-container h2 {
    margin-bottom: 2rem;
    color: var(--primary, #2a5298);
    font-weight: 700;
    font-size: 1.5rem;
    letter-spacing: -0.02em;
}
.login-container input {
    display: block;
    width: 100%;
    margin-bottom: 1.2rem;
    padding: 0.85rem 1rem;
    border: 1px solid var(--border, #b6c6e0);
    border-radius: var(--radius-btn, 6px);
    font-size: 1rem;
    background: var(--bg-zebra, #f7fafd);
    color: var(--text-main, #333);
    transition: all 0.2s ease;
    box-shadow: var(--shadow-input, none);
    box-sizing: border-box;
}
.login-container input::placeholder {
    color: var(--text-muted, #999);
}
.login-container input:focus {
    border-color: var(--primary, #2a5298);
    background: var(--white, #fff);
    outline: none;
    box-shadow: 0 0 0 3px rgba(46, 125, 50, 0.1); /* Anillo de focus suave */
}
.login-container button {
    width: 100%;
    padding: 0.85rem;
    background: var(--primary, #2a5298);
    color: #fff;
    border: none;
    border-radius: var(--radius-btn, 6px);
    font-size: 1.1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s ease;
    margin-top: 0.5rem;
    box-shadow: var(--shadow-input, none);
}
.login-container button:hover {
    background: var(--primary-hover, #1e3c72);
    transform: translateY(-1px);
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}
.error {
    color: #d32f2f;
    margin-bottom: 1rem;
    font-size: 0.9rem;
    font-weight: 500;
    background: #fdecea;
    padding: 0.5rem;
    border-radius: 4px;
    display: none; /* Ocultar por defecto, mostrar solo si hay error */
}
.error:not(:empty) {
    display: block;
}

/* Estilos para el selector de temas flotante */
.theme-selector-container {
    position: fixed;
    bottom: 20px;
    left: 20px;
    z-index: 9999;
    font-family: var(--font-family, sans-serif);
}
.theme-toggle-btn {
    background: var(--primary);
    color: white;
    border: none;
    border-radius: 50px;
    padding: 10px 20px;
    font-weight: 600;
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    gap: 8px;
}
.theme-toggle-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 16px rgba(0,0,0,0.2);
}
.theme-menu {
    position: absolute;
    bottom: 100%;
    left: 0;
    margin-bottom: 10px;
    background: var(--white);
    color: var(--text-main);
    border-radius: 12px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.15);
    overflow: hidden;
    display: none;
    flex-direction: column;
    min-width: 200px;
    border: 1px solid var(--border, #eee);
}
.theme-menu.show {
    display: flex;
    animation: slideUp 0.2s ease;
}
.theme-option {
    padding: 12px 20px;
    border: none;
    background: none;
    text-align: left;
    cursor: pointer;
    font-size: 0.95rem;
    color: var(--text-main, #333);
    transition: background 0.2s;
    display: flex;
    align-items: center;
    gap: 10px;
}
.theme-option:hover {
    background: var(--bg-zebra, #f5f5f5);
}
.theme-option.active {
    font-weight: bold;
    background: var(--bg-zebra, #f0f7ff);
    color: var(--primary, #0056b3);
}
.color-dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    display: inline-block;
}
@keyframes slideUp {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
}

```

### Archivo: `css/2_pdf_styles.css`
```css
/* ==========================================================================
   ESPACIA - SISTEMA DE IMPRESIÓN PROFESIONAL (2_pdf_styles.css)
   ========================================================================== */

/* Este archivo optimiza la visualización para generar PDFs 
   o imprimir directamente desde el navegador. 
*/

@media print {
    /* 1. Reset General para Papel */
    @page {
        size: A4 portrait;
        margin: 15mm;
    }

    body {
        background: #fff !important;
        color: #000 !important;
        font-size: 11pt;
    }

    /* 2. Ocultar Elementos de Interfaz (No necesarios en un PDF) */
    header, 
    nav, 
    .btn-menu, 
    .informe-form, 
    .pagination, 
    .no-print,
    #sidebar,
    footer {
        display: none !important;
    }

    /* 3. Ajuste de Contenedores */
    .informe-container {
        width: 100% !important;
        margin: 0 !important;
        padding: 0 !important;
        box-shadow: none !important;
    }

    /* 4. Scorecards en Impresión */
    .scorecard-grid {
        display: flex !important;
        flex-direction: row !important;
        justify-content: space-between;
        gap: 10px;
        margin-bottom: 30px !important;
        grid-template-columns: none !important; /* Forzamos flex para evitar bugs de grid en PDF */
    }

    .card-stat {
        flex: 1;
        border: 1px solid #eee !important;
        background: #f9f9f9 !important; /* Fondo gris muy tenue para ahorrar tinta */
        color: #333 !important;
        box-shadow: none !important;
        border-radius: 8px !important;
        -webkit-print-color-adjust: exact;
        print-color-adjust: exact;
        min-height: auto !important;
        padding: 10px !important;
    }

    .card-stat-time {
        color: #000 !important;
        font-size: 1.8rem !important;
    }

    .card-stat-title {
        color: #555 !important;
        font-size: 0.9rem !important;
    }

    .card-stat-desglose {
        border-left: 1px solid #ccc !important;
        color: #666 !important;
    }

    /* 5. Tabla de Datos Profesional */
    .table-main-container {
        border: none !important;
        overflow: visible !important;
    }

    .custom-table {
        width: 100% !important;
        border-collapse: collapse !important;
        margin-top: 20px;
    }

    .custom-table th {
        background-color: #f2f2f2 !important;
        color: #000 !important;
        border-bottom: 2px solid #333 !important;
        /* Corrección de compatibilidad sugerida por VSC */
        -webkit-print-color-adjust: exact; 
        print-color-adjust: exact;
    }

    .custom-table td {
        border-bottom: 1px solid #eee !important;
        padding: 8px !important;
    }

    /* 6. Badges de Estado (Simplificados para BN) */
    .status-badge {
        border: 1px solid #ccc !important;
        background: transparent !important;
        color: #000 !important;
        text-transform: uppercase;
        font-size: 8pt !important;
    }

    /* 7. Pie de página del informe */
    .informe-container::after {
        content: "Informe generado por ESPACIA Management System - Propiedad de TechFaro";
        display: block;
        text-align: center;
        font-size: 9pt;
        color: #aaa;
        margin-top: 50px;
        border-top: 1px solid #eee;
        padding-top: 10px;
    }
}
```

### Archivo: `css/2_reservas.css`
```css
/* reservas.css */
.reserva-card {
    border: none;
    border-radius: var(--radius);
    background: var(--white);
    box-shadow: var(--shadow-md);
    padding: 1.5rem;
    transition: var(--transition);
}

.reserva-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}

.reserva-card button.btn-cancelar {
    background: var(--white);
    color: var(--text-main);
    color: var(--error-color);
    border: 1px solid var(--error-color);
    border-radius: var(--radius-sm);
    padding: 8px 16px;
    font-size: 0.9rem;
}

.reserva-card button.btn-cancelar:hover {
    background: var(--error-color);
    color: #fff;
}
```

### Archivo: `dashboard.html`
```html
<script type="module">
import { SHOW_UPDATE_MODAL } from './js/config.js';
if ('serviceWorker' in navigator && SHOW_UPDATE_MODAL) {
    navigator.serviceWorker.addEventListener('controllerchange', function() {
        document.getElementById('updateModal').style.display = 'block';
    });
}
</script>
<script>
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.addEventListener('controllerchange', function() {
        document.getElementById('updateModal').style.display = 'block';
    });
}
</script>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
    <meta name="theme-color" content="#2E7D32">
    <link rel="manifest" href="manifest.json">
    <link rel="apple-touch-icon" href="imagenes/icon-192.png">
    <title>Espacia</title>
    <link rel="stylesheet" href="css/1_comunes.css">
    <link rel="stylesheet" href="css/2_dashboard.css">
</head>
<body class="theme-classic">
    <!-- Barra Superior Móvil -->
    <div class="mobile-top-bar" id="mobile-top-bar">
        <button class="hamburger-btn" id="hamburger-btn">☰</button>
        <div class="mobile-logo" style="display: flex; align-items: center;">
            <img src="imagenes/logo_demaria.png" alt="Logo de Maria Consultorios" style="height: 40px; object-fit: contain;">
        </div>
    </div>
    <!-- Fondo oscuro que aparece al abrir menú móvil -->
    <div class="sidebar-overlay" id="sidebar-overlay"></div>

        <!-- Modal actualización de versión -->
        <div id="updateModal" style="display:none; position:fixed; top:0; left:0; width:100vw; height:100vh; background:rgba(0,0,0,0.5); z-index:9999;">
            <div style="background:#fff; padding:2em; margin:10% auto; width:300px; border-radius:8px; text-align:center;">
                <h2>¡Nueva versión disponible!</h2>
                <p>Recarga la página para actualizar Espacia.</p>
                <button onclick="location.reload()">Recargar</button>
            </div>
        </div>
        <div class="dashboard-container">
        
        <!-- Sidebar Izquierdo Completo -->
        <aside class="sidebar" id="sidebar-menu">
            <div class="sidebar-user-header" style="text-align: center; padding-top: 15px; padding-bottom: 15px; border-bottom: 1px solid var(--border);">
                <!-- Logo del Consultorio -->
                <div id="desktop-logo" style="margin-bottom: 15px; display: flex; justify-content: center; width: 100%;">
                    <img src="imagenes/logo_demaria.png" alt="Logo de Maria Consultorios" style="height: 70px; object-fit: contain;">
                </div>
                <!-- Nombre del Usuario Logueado (Ej. Fabio Roberto) -->
                <div id="user-email" style="display:block; font-size: 1.1rem; color: var(--primary); font-weight: bold;">Cargando usuario...</div>
            </div>

            <!-- Botonera tipo Lista Vertical -->


                <div id="dashboard-btns" style="display: flex; flex-direction: column;">
                <div class="menu-nav tabs-nav">
                    <button class="btn-tab active" id="btn-agenda-v2">📅 Agenda</button>
                    <button class="btn-tab" id="btn-reservas">🕰️ Reservas</button>
                    <button class="btn-tab" id="btn-informe">📊 Informe</button>
                    <button class="btn-tab" id="btn-abmusu2">👥 ABM Usuarios</button>
                    <button class="btn-tab" id="btn-bloqueo-horarios" style="display:none;">🔒 Bloqueo</button>
                          <button class="btn-tab" id="btn-cuponera" style="display: none;">🎟️ Cuponera</button>
                </div>
            </div>

            <!-- Boton Cerrar Sesion Abajo de Todo -->
            <div class="bottom-actions" style="margin-top: 10px; padding-top: 15px; border-top: 1px solid var(--border); margin-bottom: 5px;">
                <button id="logout-btn" class="btn-secondary sidebar-logout" style="width: 100%; border: 1px solid var(--primary); color: var(--primary); padding: 10px; font-weight: bold; background: transparent; cursor: pointer; border-radius: 8px; margin-bottom: 8px;">🚪 Cerrar Sesión</button>
                <div style="text-align: center; margin-bottom: 8px; font-size: 0.75rem; color: #888;">
                    <span class="app-version-display">v-.-.-</span>
                </div>
                <button id="instalar-pwa-btn" style="width: 100%; border: none; background-color: #28a745; color: white; padding: 10px; font-weight: bold; cursor: pointer; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.2);">📲 Instalar App</button>
            </div>
        </aside>

        <!-- Contenido Principal Expandido -->
        <main class="content">
            <!-- Header Superior Minimizado / Oculto -->
            <div class="main-header" style="display: none;">
                <h1 id="welcome-msg" style="margin: 0; font-size: 2rem;">Cargando...</h1>
                <div class="logo-top-right" style="display: flex; align-items: center; justify-content: center; height: 50px; width: 140px;">
                    <img src="imagenes/logo_demaria.png" alt="Logo de Maria Consultorios" style="height: 100%; max-width: 100%; object-fit: contain;">
                </div>
            </div>

            <section id="admin-section" style="display: none;" class="card admin-card">
                <h2>📊 Reportes Generales</h2>
                <div class="stats-grid">
                    <div class="stat-item">Agendadas: <span id="count-agendadas">0</span></div>
                    <div class="stat-item">Canceladas: <span id="count-canceladas">0</span></div>
                    <div class="stat-item">Anuladas: <span id="count-anuladas">0</span></div>
                </div>
            </section>

            <section id="agenda-v2-section" class="card" style="display:none;">
                <div id="agenda-v2-container"></div>
            </section>
            <section id="reservas-section" class="card" style="display:none;">
                <div id="reservas-container"></div>
            </section>
            <section id="informe-section" class="card" style="display:none;">
                <div id="informe-container"></div>
            </section>
            <section id="abmusu2-section" class="card" style="display:none;">
                <div id="abmusu2-container"></div>
            </section>
            <section id="bloqueo-section" class="card" style="display:none;">
                <div id="bloqueo-container"></div>
            </section>
                <section id="cuponera-section" class="card" style="display:none;">
                    <div id="cuponera-container"></div>
                </section>
        </main>
    </div>

    <!-- Selector de temas flotante (Solo Desarrollo) -->
    <div class="theme-selector-container" style="position: fixed; bottom: 20px; left: 20px; z-index: 9999; font-family: var(--font-family, sans-serif);">
        <div id="themeMenu" class="theme-menu" style="position: absolute; bottom: 100%; left: 0; margin-bottom: 10px; background: var(--white, white); border-radius: 12px; box-shadow: 0 10px 25px rgba(0,0,0,0.15); overflow: hidden; display: none; flex-direction: column; min-width: 200px; border: 1px solid var(--border, #eee);">
            <button class="theme-option" data-theme="modern" style="padding: 12px 20px; border: none; background: none; text-align: left; cursor: pointer; font-size: 0.95rem; color: var(--text-main, #333); display: flex; align-items: center; gap: 10px;">
                <span style="width: 12px; height: 12px; border-radius: 50%; background: #2E7D32;"></span> Verde Bosque
            </button>
            <button class="theme-option" data-theme="warm" style="padding: 12px 20px; border: none; background: none; text-align: left; cursor: pointer; font-size: 0.95rem; color: var(--text-main, #333); display: flex; align-items: center; gap: 10px;">
                <span style="width: 12px; height: 12px; border-radius: 50%; background: #D97757;"></span> Cálido y Acogedor
            </button>
            <button class="theme-option" data-theme="ocean" style="padding: 12px 20px; border: none; background: none; text-align: left; cursor: pointer; font-size: 0.95rem; color: var(--text-main, #333); display: flex; align-items: center; gap: 10px;">
                <span style="width: 12px; height: 12px; border-radius: 50%; background: #0284C7;"></span> Océano
            </button>
            <button class="theme-option" data-theme="lavender" style="padding: 12px 20px; border: none; background: none; text-align: left; cursor: pointer; font-size: 0.95rem; color: var(--text-main, #333); display: flex; align-items: center; gap: 10px;">
                <span style="width: 12px; height: 12px; border-radius: 50%; background: #7C3AED;"></span> Lavanda
            </button>
            <button class="theme-option" data-theme="sunset" style="padding: 12px 20px; border: none; background: none; text-align: left; cursor: pointer; font-size: 0.95rem; color: var(--text-main, #333); display: flex; align-items: center; gap: 10px;">
                <span style="width: 12px; height: 12px; border-radius: 50%; background: #EA580C;"></span> Atardecer (Cálido)
            </button>
            <button class="theme-option" data-theme="minimalist" style="padding: 12px 20px; border: none; background: none; text-align: left; cursor: pointer; font-size: 0.95rem; color: var(--text-main, #333); display: flex; align-items: center; gap: 10px;">
                <span style="width: 12px; height: 12px; border-radius: 50%; background: #57534E;"></span> Minimalista
            </button>
            <button class="theme-option" data-theme="mustard" style="padding: 12px 20px; border: none; background: none; text-align: left; cursor: pointer; font-size: 0.95rem; color: var(--text-main, #333); display: flex; align-items: center; gap: 10px;">
                <span style="width: 12px; height: 12px; border-radius: 50%; background: #CA8A04;"></span> Mostaza
            </button>
           <!-- <button class="theme-option" data-theme="dark" style="padding: 12px 20px; border: none; background: none; text-align: left; cursor: pointer; font-size: 0.95rem; color: var(--text-main, #333); display: flex; align-items: center; gap: 10px;">
                <span style="width: 12px; height: 12px; border-radius: 50%; background: #0F172A;"></span> Modo Oscuro
            </button>
            <button class="theme-option" data-theme="astigmatismo" style="padding: 12px 20px; border: none; background: none; text-align: left; cursor: pointer; font-size: 0.95rem; color: var(--text-main, #333); display: flex; align-items: center; gap: 10px;">
                <span style="width: 12px; height: 12px; border-radius: 50%; background: #2A2A35;"></span> Oscuro (Astigmatismo)
            </button>-->
        </div>
        <button id="themeToggle" style="background: var(--primary); color: white; border: none; border-radius: 50px; padding: 10px 20px; font-weight: 600; cursor: pointer; box-shadow: 0 4px 12px rgba(0,0,0,0.15); display: flex; align-items: center; gap: 8px;">
            🎨 Probar Diseños
        </button>
    </div>

    <!-- Time Indicator: línea de tiempo actual -->
    <script src="js/components/TimeIndicator.js"></script>
    <!-- Script idéntico al dashboard normal (que cargará todos tus JS) -->
    <script type="module" src="js/dashboard.js"></script>
    <script>
        // Registrar el Service Worker (NECESARIO PARA QUE EL NAVEGADOR PERMITA INSTALAR)
        let refreshing = false;

        function promptForUpdate(registration) {
            if (window.confirm('Hay una nueva versión disponible. ¿Deseas recargar ahora para actualizar?')) {
                if (registration.waiting) {
                    registration.waiting.postMessage({ type: 'SKIP_WAITING' });
                }
            }
        }

        function trackInstalling(worker, registration) {
            worker.addEventListener('statechange', () => {
                if (worker.state === 'installed' && navigator.serviceWorker.controller) {
                    promptForUpdate(registration);
                }
            });
        }

        if ('serviceWorker' in navigator) {
            window.addEventListener('load', function() {
                navigator.serviceWorker.register('/sw.js').then(function(registration) {
                    console.log('ServiceWorker registrado con éxito en el scope: ', registration.scope);
                    if (registration.waiting) {
                        promptForUpdate(registration);
                    }
                    if (registration.installing) {
                        trackInstalling(registration.installing, registration);
                    }
                    registration.addEventListener('updatefound', () => {
                        trackInstalling(registration.installing, registration);
                    });
                }, function(err) {
                    console.log('ServiceWorker falló al registrarse: ', err);
                });

                navigator.serviceWorker.addEventListener('controllerchange', function() {
                    if (refreshing) return;
                    refreshing = true;
                    window.location.reload();
                });
            });
        }

        // Lógica para el selector de temas
        document.addEventListener('DOMContentLoaded', () => {
            const toggleBtn = document.getElementById('themeToggle');
            const themeMenu = document.getElementById('themeMenu');
            const themeOptions = document.querySelectorAll('.theme-option');
            const body = document.body;
            
            function applyTheme(themeName) {
                body.classList.remove('theme-modern', 'theme-blue', 'theme-dark', 'theme-warm', 'theme-pisama', 'theme-ocean', 'theme-lavender', 'theme-sunset', 'theme-minimalist', 'theme-mustard', 'theme-astigmatismo');
                if (themeName !== 'classic') body.classList.add(`theme-${themeName}`);
                localStorage.setItem('espacia_theme', themeName);
                
                themeOptions.forEach(opt => {
                    opt.style.fontWeight = opt.dataset.theme === themeName ? 'bold' : 'normal';
                    opt.style.background = opt.dataset.theme === themeName ? '#f0f7ff' : 'none';
                });
                
                document.dispatchEvent(new CustomEvent('themeChanged', { detail: { theme: themeName } }));

                // Actualizar el color de la barra de la PWA
                setTimeout(() => {
                    const metaThemeColor = document.querySelector('meta[name="theme-color"]');
                    if (metaThemeColor) {
                        const computedColor = getComputedStyle(document.body).getPropertyValue('--primary').trim();
                        metaThemeColor.setAttribute('content', computedColor || '#2E7D32');
                    }
                }, 50);
            }

            const savedTheme = localStorage.getItem('espacia_theme') || 'classic';
            applyTheme(savedTheme);

            toggleBtn.addEventListener('click', (e) => {
                e.stopPropagation();
                themeMenu.style.display = themeMenu.style.display === 'flex' ? 'none' : 'flex';
            });

            document.addEventListener('click', (e) => {
                if (!themeMenu.contains(e.target) && e.target !== toggleBtn) {
                    themeMenu.style.display = 'none';
                }
            });

            themeOptions.forEach(option => {
                option.addEventListener('click', () => {
                    applyTheme(option.dataset.theme);
                    themeMenu.style.display = 'none';
                });
            });
            
            // Si ya está funcionando como App independiente, no mostrar el botón
            window.addEventListener('appinstalled', () => {
                const btn = document.getElementById('instalar-pwa-btn');
                if(btn) btn.style.display = 'none';
                window.deferredPrompt = null;
            });
        });

        // ==========================================
        // Lógica PWA a nivel global (Evita perder el evento)
        // ==========================================
        window.deferredPrompt = null;

        window.addEventListener('beforeinstallprompt', (e) => {
            // Previene el infobar por defecto de Chrome
            e.preventDefault();
            // Guarda el evento para poder dispararlo luego 
            window.deferredPrompt = e;
            console.log('✅ Evento beforeinstallprompt capturado. Listo para instalar.');
            
            // Intenta actualizar el botón si ya cargó en el DOM
            const btn = document.getElementById('instalar-pwa-btn');
            if (btn) btn.innerHTML = '📲 Instalar App (Listo)';
        });

        document.addEventListener('DOMContentLoaded', () => {
            const btnInstalar = document.getElementById('instalar-pwa-btn');

            // --- Lógica del Menú Hamburguesa en Móvil ---
            const hamburgerBtn = document.getElementById('hamburger-btn');
            const sidebarMenu = document.getElementById('sidebar-menu');
            const sidebarOverlay = document.getElementById('sidebar-overlay');
            const btnTabs = document.querySelectorAll('.btn-tab');

            function toggleMenu() {
                sidebarMenu.classList.toggle('open');
                sidebarOverlay.classList.toggle('active');
            }

            if (hamburgerBtn && sidebarMenu && sidebarOverlay) {
                hamburgerBtn.addEventListener('click', toggleMenu);
                sidebarOverlay.addEventListener('click', toggleMenu);
                
                // Cerrar menú al hacer clic en cualquier opción (en móvil)
                btnTabs.forEach(btn => {
                    btn.addEventListener('click', () => {
                        if (window.innerWidth <= 768 && sidebarMenu.classList.contains('open')) {
                            toggleMenu();
                        }
                    });
                });
            }
            // ---------------------------------------------
            
            // 🛑 Ocultar el botón OBLIGATORIAMENTE si la app ya está instalada y ejecutándose conectada a la PWA o pantalla completa
            const isStandalone = window.matchMedia('(display-mode: standalone)').matches || 
                                 window.navigator.standalone || 
                                 document.referrer.includes('android-app://') ||
                                 (window.matchMedia('(display-mode: window-controls-overlay)').matches);

            if (isStandalone && btnInstalar) {
                btnInstalar.style.display = 'none';
                return; // Corta la ejecución aquí si es standalone
            }

            // Otra validación: detectar si estamos en PC como App instalada revisando el window.matchMedia y navigator.standalone no siempre es suficiente
            // Algunos navegadores de escritorio inyectan su propio display-mode. Y outerHeight en PC no siempre es fiable si uno reescala la ventana de la PWA.
            
            // Detectar iOS (Safari)
            const isIOS = /iPad|iPhone|iPod/.test(navigator.userAgent) && !window.MSStream;
            
            if (isIOS && btnInstalar) {
                btnInstalar.innerHTML = '🍎 Instalar en iOS';
                btnInstalar.style.backgroundColor = '#007AFF'; // Azul tipo Apple
            } else if (window.deferredPrompt && btnInstalar) {
                btnInstalar.innerHTML = '📲 Instalar App (Listo)';
            }
            
            if (btnInstalar) {
                btnInstalar.addEventListener('click', async () => {
                    if (isIOS) {
                        alert("📲 Para instalar en tu iPhone/iPad:\n\n1. Toca el botón 'Compartir' (el cuadrado con la flecha hacia arriba en la barra inferior).\n2. Selecciona 'Agregar a Inicio' ➕.\n3. Confirma arriba a la derecha.");
                        return;
                    }
                    
                    if (window.deferredPrompt) {
                        try {
                            window.deferredPrompt.prompt();
                            const { outcome } = await window.deferredPrompt.userChoice;
                            if (outcome === 'accepted') {
                                console.log('✅ El usuario aceptó instalar la app');
                                btnInstalar.style.display = 'none';
                            }
                            window.deferredPrompt = null;
                        } catch (err) {
                            console.error('Error lanzando el prompt:', err);
                        }
                    } else {
                        alert("⚠️ No recibimos el aviso de instalación. Requisitos: 1. No estar en Modo Incógnito. 2. URL con HTTPS. 3. Haber interactuado con la página. Si estás en iPhone usa Compartir > Agregar al Inico.");
                    }
                });
            }
        });
    </script>
</body>
<script type="module">
import { SHOW_UPDATE_MODAL } from './js/config.js';
if ('serviceWorker' in navigator && SHOW_UPDATE_MODAL) {
    navigator.serviceWorker.addEventListener('controllerchange', function() {
        document.getElementById('updateModal').style.display = 'block';
    });
}
</script>
</body>
</html>

```

### Archivo: `documentacion/Manual_Administrador.pdf`
```pdf
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/Manual_Gestor.pdf`
```pdf
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/Manual_Usuario.pdf`
```pdf
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a01_agenda.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a02_agendando_para.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a03_detalle.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a04_agenda_bloqueo.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a05_reservas.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a06_informe.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a07_bloqueo.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a08_bloqueo_confirmar.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a09_usuarios.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a10_nuevo_usuario.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a11_editar_usuario.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/a12_inactivar.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g01_agenda.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g02_agendando_para.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g03_detalle.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g04_agenda_bloqueo.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g05_reservas.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g06_informe.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g07_bloqueo.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g08_bloqueo_confirmar.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g09_usuarios.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g10_nuevo_usuario.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g11_editar_usuario.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/g12_inactivar.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u01_login.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u02_cambio_pass.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u03_agenda.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u04_duracion.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u05_confirmada.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u06_detalle.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u07_mis_reservas.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u08_cancelar.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u09_movil.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/img/u10_movil_menu.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `documentacion/manual_admin.md`
```md
# Manual del Administrador

Espacia: control de usuarios, permisos, reservas e informes11

---

## 1. Tu rol como Administrador

Este manual es para quienes tienen perfil **Administrador** en Espacia, el sistema de reservas de consultorios. El Administrador es el responsable final del sistema. Puede hacer todas las tareas del día a día (reservar, cancelar, bloquear horarios, sacar informes) y además decide **quién entra al sistema y con qué permisos**.

Con tu perfil podés:

- Ver la agenda completa, con el nombre de quién reservó cada horario.
- Reservar a nombre de cualquier profesional y cancelar cualquier reserva que todavía no haya empezado.
- **Cargar reservas en fechas pasadas**, para corregir o completar registros.
- **Reservar sin el límite de 12 semanas** que tienen los demás perfiles.
- Consultar todas las reservas próximas, sacar informes y descargarlos en PDF.
- Bloquear horarios en uno, varios o todos los consultorios.
- Dar de alta, modificar, inactivar y reactivar usuarios, y **asignar los roles**.

### Los tres perfiles de Espacia

| Perfil | Quién lo usa | Qué puede hacer |
|---|---|---|
| **Usuario** | Profesionales que alquilan consultorios. | Reservar a su nombre, ver sus reservas y cancelarlas hasta 24 horas antes. Solo ve los consultorios que tiene permitidos. |
| **Gestor** | Secretaría o recepción. | Todo lo operativo: reservar para otros, cancelar, bloquear, informes y usuarios. Puede cargar reservas del día anterior solo hasta las 02:00. |
| **Administrador** | Dirección o responsable del sistema. | Lo mismo que el Gestor, y además cargar reservas en cualquier fecha pasada o a más de 12 semanas. Es quien define los roles. |

> Como Administrador, todo lo que hagas queda registrado: cada reserva guarda **para quién** es y **quién la hizo**, y cada cancelación guarda **quién la canceló**.

### Reglas de reserva para tu perfil

| Regla | Para el Administrador |
|---|---|
| Días y horarios | De lunes a viernes de 08:00 a 22:00 y los sábados de 08:00 a 16:00. Los domingos el local está cerrado. |
| Duración | 60 o 90 minutos por reserva. |
| Fechas pasadas | Podés cargar reservas en días anteriores (por ejemplo, una consulta que se hizo sin reserva). |
| Anticipación | Sin límite de semanas. Los Gestores y Usuarios tienen un límite de 12 semanas. |
| Cancelaciones | Podés cancelar desde la Agenda cualquier reserva que todavía no empezó, aunque falten menos de 24 horas. |
| Duración de la sesión | Unas 12 horas. Después tenés que volver a ingresar. |

## 2. Ingresar y cuidar tu cuenta

1. Abrí la dirección de Espacia.
2. Escribí tu **Usuario** y tu **Contraseña** y hacé clic en **Ingresar**.
3. Si es tu primer ingreso, creá una contraseña nueva cuando el sistema te lo pida y hacé clic en **Guardar y Entrar**.

Para cambiar tu contraseña, escribí tu usuario y tu contraseña actual en la pantalla de ingreso y hacé clic en **Cambiar mi contraseña**.

Tu cuenta es la que tiene más permisos. Para cuidarla:

- Usá una contraseña larga y que no uses en otros sitios.
- No compartas tu usuario. Si otra persona necesita hacer tareas administrativas, creale su propia cuenta de Gestor.
- Cerrá la sesión con **Cerrar Sesión** al terminar.
- Que haya **al menos dos Administradores activos**, para no quedarse sin acceso si uno olvida su contraseña.

## 3. El menú del Administrador

| Opción | Para qué sirve |
|---|---|
| **Agenda** | Ver la disponibilidad, reservar para un profesional y cancelar. |
| **Reservas** | Ver la lista de reservas próximas de todos los profesionales. |
| **Informe** | Ver estadísticas de uso por período, consultorio, profesional y estado. Exportar a PDF. |
| **ABM Usuarios** | Dar de alta, modificar, inactivar y reactivar usuarios. Asignar roles y consultorios. |
| **Bloqueo** | Bloquear horarios para que no se puedan reservar. |
| **Cerrar Sesión** | Salir del sistema. |

## 4. Usuarios, roles y permisos

Esta es la tarea más importante del Administrador. Entrá a **ABM Usuarios**. Vas a ver a todas las personas con acceso: nombre, usuario (ID), rol y estado. Para buscar a alguien, usá **Filtrar por nombre, ID o rol…**.

![Lista de usuarios](img/a09_usuarios.png)

### Dar de alta a una persona

1. Hacé clic en **+ Nuevo Usuario**.
2. Escribí el **Nombre completo**. El sistema arma solo el **Nombre de usuario (ID)** y el **email**. Por ejemplo, *Lucía Torres Silva* queda como `LuciaTS`.
3. Abrí **Datos adicionales y Rol** para revisar el email, cargar el documento y elegir el **Rol del sistema**.
4. En **Espacios permitidos**, marcá los consultorios que va a ver esa persona en su agenda.
5. Hacé clic en **Guardar**.

![Alta de un usuario](img/a10_nuevo_usuario.png)

**Contraseña inicial:** es el ID seguido de `1234567` (en el ejemplo, `LuciaTS1234567`). Pasásela a la persona por un medio privado. En su primer ingreso, el sistema le va a pedir que cree una contraseña propia.

### Qué rol asignar

- **Usuario**: a todos los profesionales que reservan consultorios. Es el perfil más seguro y el que corresponde casi siempre.
- **Gestor**: solo a quienes trabajan en la secretaría o la recepción y necesitan reservar por otros, cancelar, bloquear y administrar usuarios.
- **Administrador**: solo a los responsables del sistema.

> El Gestor también puede cambiar roles desde **ABM Usuarios**. Acordá con los Gestores que los perfiles de Gestor y Administrador los asignás vos, y revisá la lista de usuarios cada tanto.

### Espacios permitidos

Sirven para que cada profesional vea en la agenda solo los consultorios que le corresponden:

- Si marcás algunos consultorios, el profesional solo ve y reserva esos.
- Si no marcás ninguno, ve todos.
- Los Gestores y Administradores siempre ven todos los consultorios.

### Modificar a una persona

1. Hacé clic en el lápiz (**Editar**).
2. Cambiá el nombre, email, documento, rol o espacios permitidos. El **ID** no se puede cambiar.
3. Para asignar una contraseña nueva (por ejemplo, si la olvidó), escribila en **Nueva Contraseña**. Si dejás el campo vacío, la contraseña no cambia.
4. Hacé clic en **Guardar**.

![Edición de un usuario](img/a11_editar_usuario.png)

Si asignaste una contraseña, pasásela por un medio privado y pedile que la cambie desde **Cambiar mi contraseña**.

### Inactivar y reactivar

- **Inactivar** (ícono de la papelera): la persona ya no puede entrar al sistema. Usalo cuando alguien deja de trabajar en el lugar o cuando una cuenta pudo quedar expuesta.
- **Reactivar** (ícono de flechas): vuelve a habilitar el acceso.

![Confirmación de inactivación](img/a12_inactivar.png)

> Los usuarios no se borran, se inactivan. Así se conserva el historial de reservas para los informes. Antes de inactivar a alguien, revisá en **Reservas** si tiene reservas próximas y cancelalas si corresponde.

### Revisión periódica recomendada

Una vez por mes, revisá en **ABM Usuarios**:

1. Que solo tengan rol **Gestor** o **Administrador** las personas que corresponde.
2. Que estén **inactivas** las personas que ya no trabajan en el lugar.
3. Que los **espacios permitidos** de cada profesional estén bien.

## 5. La agenda

1. Hacé clic en **Agenda** y elegí el día con el selector de fecha.
2. Vas a ver todos los consultorios con el nombre del profesional en cada reserva.

![Agenda vista por el Administrador](img/a01_agenda.png)

| Lo que ves | Qué significa |
|---|---|
| Espacio en blanco | Libre. |
| Bloque con un nombre | Reserva de ese profesional. Hacé clic para ver el detalle. |
| **BLOQUEADO** en todas las columnas | Bloqueo de todos los consultorios. |
| Bloque con el texto de un bloqueo en una sola columna | Bloqueo de ese consultorio. |
| Bloque en tono más claro | La reserva ya empezó o ya terminó. |

![Día con un bloqueo de todos los consultorios](img/a04_agenda_bloqueo.png)

## 6. Reservar para un profesional

1. En la **Agenda**, elegí la fecha.
2. En **Agendando para:** (arriba a la derecha), elegí al profesional.

![Selector Agendando para](img/a02_agendando_para.png)

3. Pasá el mouse sobre el horario libre y hacé clic en **+**.
4. Elegí **60 minutos** o **90 minutos** y hacé clic en **Aceptar**.
5. Aparece **Turno agendado correctamente** y se descarga el comprobante en PDF.

Si no elegiste a nadie en **Agendando para:**, el sistema te lo avisa y no reserva. Revisá el nombre antes de confirmar: la reserva queda a nombre de ese profesional y en el detalle figura que la hiciste vos.

### Corregir registros de días anteriores

Como Administrador podés reservar en fechas pasadas. Usalo para:

- registrar una consulta que se hizo sin reserva;
- completar el registro de un día cuando el Gestor ya no puede (los Gestores solo pueden cargar el día anterior hasta las 02:00).

Elegí la fecha pasada en la agenda y reservá igual que siempre. Esas reservas aparecen en el informe como *Consumida*.

## 7. Ver el detalle y cancelar

Hacé clic sobre una reserva para ver su detalle.

![Detalle de una reserva](img/a03_detalle.png)

Fijate en dos datos:

- **Reserva para**: el profesional titular (al que se le cuenta en los informes).
- **Reserva realizada por**: quién la cargó. Si dice el nombre de un Gestor o Administrador, la reserva se hizo desde la administración.

Desde el detalle podés **Descargar PDF** o **Cancelar Reserva** (confirmando con **Sí, Cancelar**). Al cancelar se descarga un comprobante de cancelación y el horario queda libre. Las reservas que ya empezaron no se pueden cancelar.

> Desde la agenda podés cancelar aunque falten menos de 24 horas. Usá esa posibilidad con criterio: la regla de 24 horas existe para que los profesionales avisen con tiempo.

## 8. Lista de reservas

En **Reservas** están las reservas próximas (de los próximos 90 días) de todos los profesionales.

![Lista de reservas](img/a05_reservas.png)

- Filtrá por profesional escribiendo su nombre o eligiéndolo en la lista.
- El botón verde descarga el comprobante.
- El botón **Cancelar** de esta lista solo funciona si faltan más de 24 horas. Para cancelar una reserva más cercana, hacelo desde la **Agenda**.

## 9. Informes

1. Hacé clic en **Informe**.
2. Completá **Inicio** y **Fin**.
3. Si querés, filtrá por **Consultorio**, **Usuario** o **Estado** (*Pendiente*, *Consumida* o *Cancelada*).
4. Hacé clic en **Buscar**.

![Informe de reservas](img/a06_informe.png)

Las tarjetas muestran la cantidad de reservas y las horas de cada grupo (y cuántas fueron de 60 y de 90 minutos):

| Tarjeta | Qué incluye |
|---|---|
| **Reservadas** | Todas las reservas que no se cancelaron. |
| **Pendientes** | Las que todavía no ocurrieron. |
| **Consumidas** | Las que ya pasaron. |
| **Canceladas** | Las canceladas. No se borran: siguen registradas para el control. |

Con **Descargar PDF** obtenés el informe para imprimir o archivar, con los filtros que elegiste.

Usos habituales:

- **Facturación del mes**: elegí el mes, filtrá por profesional y estado *Consumida*.
- **Control de cancelaciones**: filtrá por estado *Cancelada*.
- **Ocupación de un consultorio**: filtrá por consultorio.

## 10. Bloquear horarios

1. Hacé clic en **Bloqueo**.
2. Elegí el **Día**. Marcá **Día completo** o completá **Hora inicio** y **Hora fin** (en horas en punto).
3. Marcá los **Consultorios**. Con **Todos** se bloquean todos.
4. Escribí una **Observación** con el motivo (por ejemplo, *Mantenimiento* o *Feriado*).
5. Hacé clic en **Bloquear Horarios** y confirmá.

![Formulario de bloqueo](img/a07_bloqueo.png)

![Confirmación del bloqueo](img/a08_bloqueo_confirmar.png)

Tené en cuenta:

- El bloqueo no cancela las reservas que ya existen. Si en ese horario hay una reserva, el sistema puede rechazar el bloqueo con el mensaje *El horario ya está ocupado*. Avisale al profesional, cancelá la reserva y volvé a bloquear.
- Para quitar el bloqueo de **un consultorio**, buscalo en la **Agenda**, hacé clic sobre él y elegí **Cancelar Reserva**. Los bloqueos de **todos los consultorios** (los que dicen **BLOQUEADO**) no se abren desde la agenda: si hay que quitar uno, pedíselo al responsable técnico. Por eso, revisá bien la fecha y la hora antes de confirmar un bloqueo de todos los consultorios.
- Para feriados o cierres conocidos de antemano, bloqueá con tiempo, antes de que los profesionales reserven.

## 11. Buenas prácticas

- **Una cuenta por persona.** Así siempre se sabe quién hizo cada reserva o cancelación.
- **No cambies el nombre de un usuario para "reutilizarlo"** con otra persona: el historial quedaría mezclado. Inactivá la cuenta vieja y creá una nueva.
- **Contraseñas**: pasalas siempre por un medio privado y pedí que se cambien en el primer ingreso.
- **Revisá los informes** de cancelaciones y de reservas cargadas por la administración cada tanto.
- **Ante un problema técnico** (el sistema no carga, no guarda usuarios o no muestra la agenda), avisale al responsable técnico. No hace falta tocar nada más.

## 12. Preguntas frecuentes

**Un Gestor no puede cargar una consulta de un día anterior.**
Los Gestores solo pueden cargar el día anterior hasta las 02:00. Cargala vos desde la Agenda.

**Un profesional quiere reservar a más de 12 semanas.**
Él no puede, pero vos sí: reservá a su nombre con **Agendando para:**.

**Alguien olvidó su contraseña.**
En **ABM Usuarios**, editalo y asignale una nueva en **Nueva Contraseña**.

**Una persona dejó de trabajar en el lugar.**
Cancelá sus reservas próximas y después inactivala en **ABM Usuarios**.

**Un profesional no ve un consultorio.**
Revisá sus **Espacios permitidos** en **ABM Usuarios**.

**Quiero saber quién canceló una reserva.**
Buscala en el **Informe** con estado *Cancelada*, o mirá el detalle de la reserva en la agenda.

```

### Archivo: `documentacion/manual_gestor.md`
```md
# Manual del Gestor

Espacia: gestión diaria de reservas, usuarios y bloqueos

---

## 1. Tu rol como Gestor

Este manual es para quienes tienen perfil **Gestor** en Espacia, el sistema de reservas de consultorios. El gestor se encarga del día a día: arma y cancela reservas para los profesionales, bloquea horarios, revisa los informes y mantiene actualizada la lista de usuarios.

Con tu perfil podés:

- Ver la agenda completa, con el nombre de quién reservó cada horario.
- **Reservar a nombre de cualquier profesional** con perfil Usuario.
- **Cancelar cualquier reserva** que todavía no haya empezado.
- Consultar todas las reservas próximas y filtrarlas por profesional.
- Sacar **informes** de uso y descargarlos en PDF.
- **Bloquear horarios** en uno, varios o todos los consultorios.
- Dar de alta, modificar, inactivar y reactivar **usuarios**.

> Lo que hacés queda registrado. Cada reserva guarda **para quién** es y **quién la hizo**, y cada cancelación guarda **quién la canceló**.

### Reglas que aplican a tu perfil

| Regla | Para el Gestor |
|---|---|
| Días y horarios | De lunes a viernes de 08:00 a 22:00 y los sábados de 08:00 a 16:00. Los domingos el local está cerrado. |
| Duración | 60 o 90 minutos por reserva. |
| Horarios de hoy que ya pasaron | Los podés reservar (por ejemplo, para registrar una consulta que se hizo sin reserva). |
| Días anteriores | Solo el **día de ayer** y **hasta las 02:00** de hoy, para cerrar la planilla del día. Después de esa hora, pedíselo a un Administrador. |
| Anticipación máxima | Hasta 12 semanas hacia adelante. |
| Cancelaciones | Podés cancelar cualquier reserva que todavía no empezó, aunque falten menos de 24 horas, desde la Agenda (ver sección 6). |
| Duración de la sesión | Unas 12 horas. Después tenés que volver a ingresar. |

## 2. Ingresar al sistema

1. Abrí la dirección de Espacia.
2. Escribí tu **Usuario** y tu **Contraseña** y hacé clic en **Ingresar**.
3. Si es tu primer ingreso, el sistema te va a pedir que crees una contraseña nueva. Escribila dos veces y hacé clic en **Guardar y Entrar**. Tiene que tener al menos 6 caracteres.

Para cambiar tu contraseña en cualquier momento, escribí tu usuario y tu contraseña actual en la pantalla de ingreso y hacé clic en **Cambiar mi contraseña**.

> Tu cuenta tiene permisos sobre las reservas y los datos de todos los profesionales. No la compartas y usá **Cerrar Sesión** al terminar, sobre todo en computadoras compartidas.

## 3. El menú del Gestor

| Opción | Para qué sirve |
|---|---|
| **Agenda** | Ver la disponibilidad, reservar para un profesional y cancelar. |
| **Reservas** | Ver la lista de reservas próximas de todos los profesionales. |
| **Informe** | Ver estadísticas de uso por período, consultorio, profesional y estado. Exportar a PDF. |
| **ABM Usuarios** | Dar de alta, modificar, inactivar y reactivar usuarios. |
| **Bloqueo** | Bloquear horarios para que no se puedan reservar. |
| **Cerrar Sesión** | Salir del sistema. |

## 4. La agenda

1. Hacé clic en **Agenda**.
2. Elegí el día con el selector de fecha (arriba al centro).
3. Vas a ver **todos los consultorios** y el nombre del profesional en cada reserva.

![Agenda vista por el Gestor](img/g01_agenda.png)

| Lo que ves | Qué significa |
|---|---|
| Espacio en blanco | Libre. |
| Bloque con un nombre | Reserva de ese profesional. Hacé clic para ver el detalle. |
| **BLOQUEADO** en todas las columnas | Bloqueo de todos los consultorios. |
| Bloque con el texto de un bloqueo en una sola columna | Bloqueo de ese consultorio (por ejemplo, *Bloqueada-Mantenimiento*). |
| Bloque en tono más claro | La reserva ya empezó o ya terminó. |

![Día con un bloqueo de todos los consultorios](img/g04_agenda_bloqueo.png)

## 5. Reservar para un profesional

Como Gestor, siempre reservás **a nombre de alguien**. Por eso, antes de reservar hay que elegir a la persona.

1. En la **Agenda**, elegí la fecha.
2. Arriba a la derecha, en **Agendando para:**, elegí al profesional. En la lista aparecen las personas con perfil Usuario.

![Selector Agendando para](img/g02_agendando_para.png)

3. Pasá el mouse sobre el horario libre, en la columna del consultorio, y hacé clic en **+**.
4. Elegí **60 minutos** o **90 minutos** y hacé clic en **Aceptar**.
5. Aparece **Turno agendado correctamente** y se descarga el comprobante en PDF. Si hace falta, se lo podés enviar al profesional.

Tené en cuenta:

- Si no elegiste a nadie en **Agendando para:**, el sistema te avisa: *Seleccione un usuario del combo "Agendando para:" antes de reservar.*
- Revisá el nombre antes de confirmar: el comprobante y la agenda del profesional van a mostrar la reserva a su nombre.
- El selector queda con la última persona elegida. Si vas a reservar para otro profesional, cambialo antes.
- Podés reservar en cualquier consultorio, aunque el profesional tenga asignados solo algunos.

## 6. Ver el detalle y cancelar una reserva

Hacé clic sobre cualquier reserva de la agenda para ver su detalle:

- **Reserva para**: el profesional titular.
- Email, consultorio, fecha, hora y duración.
- **Reserva realizada por**: quién la cargó (el mismo profesional o alguien de la administración).

![Detalle de una reserva](img/g03_detalle.png)

Desde el detalle:

- **Descargar PDF**: baja el comprobante.
- **Cancelar Reserva**: la cancela. Confirmá con **Sí, Cancelar**. Se descarga un comprobante de cancelación y el horario queda libre.

> Desde la agenda podés cancelar cualquier reserva que todavía no empezó, aunque falten menos de 24 horas. Las reservas que ya empezaron o pasaron no se pueden cancelar. La reserva cancelada no se borra: sigue en el **Informe** con estado *Cancelada*.

Para **quitar el bloqueo de un consultorio**, hacé clic sobre el bloque en la agenda y usá el mismo botón **Cancelar Reserva** (ver sección 9).

## 7. Lista de reservas

Hacé clic en **Reservas** en el menú. Aparecen las reservas próximas (de los próximos 90 días) de todos los profesionales, ordenadas por fecha.

![Lista de reservas de todos los profesionales](img/g05_reservas.png)

- **Filtrar por Usuario**: escribí parte del nombre o elegí a la persona en la lista desplegable. Con **Todos** se ven todas otra vez.
- El botón verde descarga el comprobante de cada reserva.
- El botón **Cancelar** de esta lista solo está activo si faltan más de 24 horas. Para cancelar una reserva más cercana, hacelo desde la **Agenda** (sección 6).
- Con **< Anterior** y **Siguiente >** pasás de página.

Los bloqueos y las reservas canceladas no aparecen en esta lista.

## 8. Informes

El informe muestra cuánto se usaron los consultorios en un período.

1. Hacé clic en **Informe**.
2. Completá **Inicio** y **Fin** (las dos fechas son obligatorias).
3. Si querés, filtrá por **Consultorio**, **Usuario** o **Estado**.
4. Hacé clic en **Buscar**.

![Informe de reservas](img/g06_informe.png)

Arriba aparecen cuatro tarjetas con la cantidad de reservas y las horas totales de cada grupo. Debajo, la cantidad de reservas de 60 y de 90 minutos:

| Tarjeta / Estado | Qué incluye |
|---|---|
| **Reservadas** | Todas las reservas del período que no se cancelaron. |
| **Pendiente** | Reservas que todavía no ocurrieron. |
| **Consumida** | Reservas cuyo horario ya pasó. |
| **Cancelada** | Reservas canceladas. No se borran: siguen registradas para el control. |

La tabla muestra el detalle: fecha, hora de inicio, duración, consultorio, profesional y estado.

Para guardarlo o imprimirlo, hacé clic en **Descargar PDF**. El PDF respeta los filtros que elegiste.

> Los bloqueos no se cuentan en el informe.

## 9. Bloquear horarios

Bloqueá horarios cuando un consultorio no se puede usar: mantenimiento, feriado, limpieza, evento, etc.

1. Hacé clic en **Bloqueo**.
2. Elegí el **Día**.
3. Marcá **Día completo** para bloquear toda la jornada, o completá **Hora inicio** y **Hora fin**. Usá horas en punto (por ejemplo, 13:00 a 15:00).
4. Marcá los **Espacios** a bloquear. Con **Todos** se bloquean todos los consultorios.
5. Escribí una **Observación** que explique el motivo (por ejemplo, *Mantenimiento*). Se va a ver en la agenda.
6. Hacé clic en **Bloquear Horarios** y confirmá.

![Formulario de bloqueo](img/g07_bloqueo.png)

![Confirmación del bloqueo](img/g08_bloqueo_confirmar.png)

Al terminar aparece **Bloqueo realizado con éxito** y el horario se ve bloqueado en la agenda.

Tené en cuenta:

- **Antes de bloquear, revisá la agenda.** El bloqueo no cancela las reservas que ya existen. Si en ese horario hay una reserva, el sistema puede rechazar el bloqueo con el mensaje *El horario ya está ocupado*. En ese caso, avisale al profesional, cancelá la reserva y volvé a bloquear.
- No se pueden bloquear fechas pasadas ni más allá de 12 semanas.
- Para quitar el bloqueo de **un consultorio**, buscalo en la **Agenda**, hacé clic sobre él y elegí **Cancelar Reserva**. Los bloqueos de **todos los consultorios** (los que dicen **BLOQUEADO**) no se abren desde la agenda: si hay que quitar uno, pedíselo al responsable técnico. Por eso, revisá bien la fecha y la hora antes de confirmar un bloqueo de todos los consultorios.

## 10. Administrar usuarios

En **ABM Usuarios** está la lista de todas las personas que pueden entrar al sistema, con su nombre, usuario, rol y estado (activo o inactivo). Para buscar a alguien, escribí en **Filtrar por nombre, ID o rol…**.

![Lista de usuarios](img/g09_usuarios.png)

### Dar de alta un usuario

1. Hacé clic en **+ Nuevo Usuario**.
2. Escribí el **Nombre completo**. El sistema arma solo el **Nombre de usuario (ID)** y el **email**. Por ejemplo, *Lucía Torres Silva* queda como `LuciaTS`.
3. En **Datos adicionales y Rol** podés corregir el email, cargar el documento y elegir el **Rol del sistema**. Para los profesionales, dejá **Usuario**.
4. En **Espacios permitidos**, marcá los consultorios que va a ver esa persona en su agenda. Si no marcás ninguno, va a ver todos.
5. Hacé clic en **Guardar**.

![Alta de un usuario](img/g10_nuevo_usuario.png)

**Contraseña inicial:** es el ID seguido de `1234567` (en el ejemplo, `LuciaTS1234567`). Pasásela al profesional por un medio privado. En su primer ingreso, el sistema le va a pedir que cree una contraseña propia.

### Modificar un usuario

1. En la lista, hacé clic en el lápiz (**Editar**) de la persona.
2. Cambiá lo que necesites: nombre, email, documento, rol o espacios permitidos. El **ID** no se puede cambiar.
3. Para **asignar otra contraseña** (por ejemplo, si la olvidó), escribila en **Nueva Contraseña**. Si dejás el campo vacío, la contraseña no cambia.
4. Hacé clic en **Guardar**.

![Edición de un usuario](img/g11_editar_usuario.png)

Si le asignás otra contraseña, pasásela por un medio privado y recomendale que la cambie desde **Cambiar mi contraseña**.

### Inactivar y reactivar

- **Inactivar** (ícono de la papelera): la persona ya no puede entrar al sistema. Sus reservas anteriores siguen en los informes. Confirmá en la ventana que aparece.
- **Reactivar** (ícono de flechas, en un usuario inactivo): vuelve a habilitar el acceso.

![Confirmación de inactivación](img/g12_inactivar.png)

> Los usuarios no se borran: se inactivan. Así se conserva el historial de reservas.

### Sobre los roles

En la pantalla podés asignar cualquier rol, pero dar perfil de **Gestor** o **Administrador** le da a esa persona acceso a todos los datos. Antes de asignar esos perfiles, consultalo con un Administrador. A los profesionales asignales siempre el rol **Usuario**.

## 11. Preguntas frecuentes

**Un profesional me pide cancelar con menos de 24 horas.**
Hacelo desde la **Agenda**: clic en la reserva → **Cancelar Reserva**. En la lista de Reservas el botón aparece gris.

**No aparece un profesional en Agendando para.**
Revisá en **ABM Usuarios** que esté **activo** y que su rol sea **Usuario**.

**Tengo que cargar una consulta de ayer.**
Podés hacerlo hasta las 02:00 de hoy. Después, pedíselo a un Administrador.

**Un profesional no ve todos los consultorios.**
Tiene **Espacios permitidos** asignados. Editalo en **ABM Usuarios**: marcá los consultorios que necesite, o desmarcá todos para que vea todos.

**Un profesional olvidó su contraseña.**
En **ABM Usuarios**, editalo y asignale una nueva en **Nueva Contraseña**.

**Bloqueé un horario por error.**
Si es el bloqueo de un consultorio, buscalo en la Agenda, hacé clic sobre el bloque y elegí **Cancelar Reserva**. Si bloqueaste todos los consultorios, pedíselo al responsable técnico.

```

### Archivo: `documentacion/manual_usuario.md`
```md
# Manual del Usuario

Espacia: reservá tus consultorios

---

## 1. Antes de empezar

Este manual es para profesionales con perfil **Usuario** en Espacia, el sistema de reservas de consultorios. Explica cómo ver la agenda, reservar un consultorio para vos, consultar tus próximas reservas y cancelarlas.

Con tu perfil podés:

- Ver la disponibilidad de los consultorios día por día.
- Reservar un consultorio **a tu nombre**.
- Ver y descargar el comprobante de tus reservas.
- Cancelar tus reservas, respetando el plazo de cancelación.

Con tu perfil **no** podés reservar a nombre de otra persona, ver los datos de las reservas ajenas, sacar informes, bloquear horarios ni administrar usuarios. Para eso, hablá con la administración.

### Reglas generales

| Regla | Detalle |
|---|---|
| Días de atención | De lunes a sábado. Los domingos la agenda aparece como **Día cerrado**. |
| Horario | De lunes a viernes de 08:00 a 22:00. Los sábados de 08:00 a 16:00. |
| Duración de cada reserva | 60 o 90 minutos. Los turnos empiezan en la hora en punto o a la media hora. |
| Anticipación máxima | Hasta 12 semanas. Para reservar con más anticipación, comunicate con la secretaría. |
| Horarios pasados | No se puede reservar un horario que ya pasó. |
| Cancelación | Hasta 24 horas antes del inicio de la reserva. Después de ese plazo, comunicate con la administración. |

## 2. Ingresar al sistema

### Iniciar sesión

1. Abrí la dirección de Espacia que te dio la administración.
2. En **Usuario** escribí tu nombre de usuario (por ejemplo, `AnaR`).
3. En **Contraseña** escribí tu contraseña. Con el ícono del ojo podés ver lo que escribiste.
4. Hacé clic en **Ingresar** o presioná Enter.

![Pantalla de ingreso](img/u01_login.png)

> Por seguridad, la sesión se cierra sola unas horas después de entrar (unas 4). Si pasa eso, volvé a ingresar.

### Primer ingreso: crear tu contraseña

Cuando la administración te da de alta, recibís una contraseña inicial. La primera vez que entres, el sistema te va a pedir que crees una propia:

1. Escribí la nueva contraseña en **Nueva contraseña**.
2. Repetila en **Confirmar contraseña**.
3. Hacé clic en **Guardar y Entrar**.

La contraseña tiene que tener **al menos 6 caracteres** e incluir **una mayúscula, un número y un carácter especial** (por ejemplo `Consulta#7`).

![Cambio de contraseña obligatorio en el primer ingreso](img/u02_cambio_pass.png)

### Cambiar tu contraseña cuando quieras

1. En la pantalla de ingreso, escribí tu usuario y tu contraseña **actual**.
2. Hacé clic en el enlace **Cambiar mi contraseña**, debajo del botón Ingresar.
3. Escribí la nueva contraseña dos veces y hacé clic en **Guardar y Entrar**.

Si olvidaste tu contraseña, pedile a la administración que te asigne una nueva.

## 3. La pantalla principal

Cuando ingresás, a la izquierda aparece el menú con tu nombre y estas opciones:

| Opción | Para qué sirve |
|---|---|
| **Agenda** | Ver la disponibilidad y reservar. |
| **Mis Reservas** | Ver tus próximas reservas, descargar comprobantes y cancelar. |
| **Cerrar Sesión** | Salir del sistema. Usala siempre si la computadora es compartida. |
| **Instalar App** | Agregar Espacia como aplicación en el celular o la computadora (es opcional). |

El botón **Probar Diseños** (abajo a la izquierda) solo cambia los colores de la pantalla. No modifica tus datos.

## 4. Ver la agenda

1. Hacé clic en **Agenda**.
2. Arriba se ve la fecha que estás mirando. Para cambiarla, usá el selector de fecha que está a la derecha.
3. Cada columna es un consultorio y cada fila es un tramo de media hora.

![Agenda del día](img/u03_agenda.png)

Cómo leer la agenda:

| Lo que ves | Qué significa |
|---|---|
| Espacio en blanco | Libre: lo podés reservar. |
| Bloque con **tu nombre** | Una reserva tuya. Hacé clic para ver el detalle. |
| Bloque **Ocupado** | Lo reservó otra persona. Por privacidad no se muestra quién. |
| Bloque **BLOQUEADO** | La administración bloqueó ese horario (por ejemplo, por mantenimiento o feriado). |
| Bloque en tono más claro | La reserva ya empezó o ya pasó. |

> La agenda se actualiza sola cada minuto. Puede que solo veas algunos consultorios: si la administración te asignó espacios determinados, la agenda muestra únicamente esos.

## 5. Reservar un consultorio

1. En la **Agenda**, elegí la fecha.
2. Pasá el mouse sobre el horario libre que querés, en la columna del consultorio. Aparece un botón **+**.
3. Hacé clic en **+**.
4. Elegí la duración: **60 minutos** o **90 minutos**. Solo aparecen las opciones que entran antes del cierre.
5. Hacé clic en **Aceptar**.

![Elegir la duración de la reserva](img/u04_duracion.png)

Si salió todo bien:

- Aparece el mensaje **Turno agendado correctamente**.
- Se descarga automáticamente un **comprobante en PDF** de la reserva.
- La reserva aparece en la agenda con tu nombre.

![Reserva confirmada en la agenda](img/u05_confirmada.png)

Mensajes que te pueden aparecer:

| Mensaje | Qué hacer |
|---|---|
| *No puedes agendar un turno en un horario que ya pasó.* | Elegí un horario posterior. |
| *Este horario acaba de ser tomado por otro usuario.* | Otra persona lo reservó al mismo tiempo que vos. La agenda se actualiza: elegí otro horario. |
| *Para agendar con mayor antelación, comuníquese con nuestra secretaría…* | La fecha está a más de 12 semanas. Comunicate con la secretaría. |
| *No hay tiempo suficiente antes del cierre.* | Elegí un horario más temprano. |
| *No se pueden agendar turnos en días no laborales.* | Ese día el local está cerrado. |

## 6. Ver el detalle de una reserva

En la agenda, hacé clic sobre un bloque con tu nombre. Se abre una ventana con los datos de la reserva: consultorio, fecha, hora, duración y quién la hizo.

![Detalle de una reserva propia](img/u06_detalle.png)

Desde esa ventana podés:

- **Descargar PDF**: bajar el comprobante otra vez.
- **Cancelar Reserva**: cancelarla (ver sección 8).
- **Aceptar**: cerrar la ventana sin cambiar nada.

## 7. Mis Reservas

Hacé clic en **Mis Reservas** en el menú. Vas a ver una lista de tus próximas reservas (de los próximos 90 días), ordenadas por fecha, con la hora de inicio y fin, la duración y el consultorio.

![Listado de Mis Reservas](img/u07_mis_reservas.png)

En la columna **Acciones** de cada reserva:

- El botón verde con la flecha descarga el **comprobante en PDF**.
- El botón **Cancelar** está en rojo si todavía podés cancelar. Si está gris, faltan menos de 24 horas y ya no se puede cancelar desde el sistema.

Si tenés muchas reservas, usá **Siguiente >** y **< Anterior** para pasar de página.

## 8. Cancelar una reserva

Podés cancelar desde **Mis Reservas** (botón **Cancelar**) o desde la **Agenda** (clic en la reserva → **Cancelar Reserva**).

1. Hacé clic en **Cancelar**.
2. Confirmá con **Sí, Cancelar**. Si te arrepentiste, hacé clic en **Volver**.

![Confirmación de cancelación](img/u08_cancelar.png)

Después de cancelar:

- Aparece el mensaje **Reserva cancelada**.
- Se descarga un **comprobante de cancelación** en PDF.
- El horario vuelve a quedar libre en la agenda.

> **Plazo de cancelación:** solo podés cancelar hasta **24 horas antes** del inicio. Si necesitás cancelar con menos tiempo, comunicate con la administración. Las reservas que ya pasaron no se pueden cancelar.

## 9. Usar Espacia en el celular

Espacia también funciona en el celular:

- El menú se abre con el botón **☰** (arriba a la izquierda).
- En la agenda se ve **un consultorio por vez**. Para cambiar de consultorio, usá las pestañas que están arriba de la grilla.
- Con **Instalar App** podés agregar Espacia a la pantalla de inicio del celular para entrar más rápido.

| Agenda en el celular | Menú en el celular |
|---|---|
| ![Agenda en el celular](img/u09_movil.png) | ![Menú en el celular](img/u10_movil_menu.png) |

## 10. Preguntas frecuentes

**No veo el botón + en un horario.**
Ese horario está ocupado, bloqueado o ya pasó. En la computadora, el botón aparece recién cuando pasás el mouse por encima.

**No veo todos los consultorios.**
La administración puede asignarte solo algunos espacios. Si necesitás otros, pedíselo.

**El botón Cancelar está gris.**
Faltan menos de 24 horas para la reserva. Comunicate con la administración.

**No se descargó el comprobante.**
Revisá que el navegador permita descargas. Siempre podés volver a bajarlo desde **Mis Reservas** o desde el detalle de la reserva.

**Me sacó del sistema.**
La sesión vence después de unas horas por seguridad. Volvé a ingresar con tu usuario y contraseña.

**Quiero reservar para otra persona.**
Con tu perfil no se puede. Cada profesional reserva a su nombre. Si hace falta, la administración puede reservar por otra persona.

```

### Archivo: `documentacion/md-to-pdf.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MD → PDF</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/marked/9.1.6/marked.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Lato:wght@300;400;700&display=swap');

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg:      #0f0f0f;
  --surface: #1a1a1a;
  --border:  #2e2e2e;
  --gold:    #c9a84c;
  --gold2:   #e8c97a;
  --text:    #e8e4dc;
  --muted:   #888;
  --white:   #f5f2eb;
}

body {
  font-family: 'Lato', sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  display: grid;
  place-items: center;
  padding: 48px 20px 80px;
}

.wrapper {
  width: 100%;
  max-width: 560px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

header {
  text-align: center;
  margin-bottom: 48px;
  animation: riseIn 0.7s cubic-bezier(0.16,1,0.3,1) both;
}

.eyebrow {
  font-size: 0.68rem;
  font-weight: 700;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--gold);
  margin-bottom: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}
.eyebrow::before, .eyebrow::after {
  content: '';
  width: 28px; height: 1px;
  background: var(--gold);
  opacity: 0.45;
}

h1 {
  font-family: 'Playfair Display', serif;
  font-size: clamp(2.2rem, 6vw, 3.4rem);
  font-weight: 400;
  color: var(--white);
  line-height: 1.08;
}
h1 em { font-style: italic; color: var(--gold); }

.tagline {
  margin-top: 12px;
  font-size: 0.87rem;
  color: var(--muted);
  font-weight: 300;
  letter-spacing: 0.03em;
}

.drop-zone {
  width: 100%;
  border: 1px solid var(--border);
  border-radius: 4px;
  padding: 52px 36px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
  background: var(--surface);
  position: relative;
  animation: riseIn 0.7s 0.08s cubic-bezier(0.16,1,0.3,1) both;
}

.drop-zone::before, .drop-zone::after {
  content: '';
  position: absolute;
  width: 18px; height: 18px;
  border-color: var(--gold);
  border-style: solid;
  opacity: 0.35;
  transition: opacity 0.3s;
}
.drop-zone::before { top: 10px; left: 10px; border-width: 1px 0 0 1px; }
.drop-zone::after  { bottom: 10px; right: 10px; border-width: 0 1px 1px 0; }
.drop-zone:hover, .drop-zone.drag-over {
  border-color: var(--gold);
  background: #1d1b16;
}

.drop-icon {
  width: 50px; height: 50px;
  margin: 0 auto 18px;
}
.drop-icon svg { width: 100%; height: 100%; color: var(--gold); opacity: 0.8; }

.drop-title {
  font-family: 'Playfair Display', serif;
  font-size: 1.2rem;
  color: var(--white);
  margin-bottom: 7px;
}
.drop-sub {
  font-size: 0.81rem;
  color: var(--muted);
  font-weight: 300;
  line-height: 1.7;
}
.drop-sub strong {
  color: var(--gold);
  font-weight: 600;
  cursor: pointer;
  border-bottom: 1px solid rgba(201,168,76,0.3);
}

#file-input { display: none; }

.preview-card {
  display: none;
  width: 100%;
  margin-top: 2px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 4px;
  overflow: hidden;
  animation: riseIn 0.35s cubic-bezier(0.16,1,0.3,1) both;
}

.preview-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 13px 18px;
  border-bottom: 1px solid var(--border);
}
.file-info { display: flex; align-items: center; gap: 11px; }
.file-icon {
  width: 30px; height: 30px;
  background: #201e18;
  border: 1px solid rgba(201,168,76,0.25);
  border-radius: 5px;
  display: grid; place-items: center;
  font-size: 14px;
}
.file-name { font-size: 0.86rem; font-weight: 700; color: var(--white); }
.file-meta { font-size: 0.73rem; color: var(--muted); margin-top: 1px; }

.btn-remove {
  background: none; border: none; cursor: pointer;
  color: var(--muted); padding: 5px; border-radius: 3px;
  transition: all 0.2s; line-height: 0;
}
.btn-remove:hover { color: #e74c3c; }

.preview-body {
  padding: 18px;
  max-height: 200px;
  overflow-y: auto;
  font-size: 0.77rem;
  line-height: 1.6;
  color: #555;
  font-family: 'Courier New', monospace;
  white-space: pre-wrap;
}

.format-bar {
  display: flex;
  align-items: center;
  padding: 12px 18px;
  border-top: 1px solid var(--border);
  gap: 14px;
}
.fmt-label {
  font-size: 0.72rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--muted);
  font-weight: 700;
}
.chip {
  padding: 4px 14px;
  border: 1px solid var(--border);
  border-radius: 2px;
  font-size: 0.78rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s;
  color: var(--muted);
  background: none;
  letter-spacing: 0.06em;
  user-select: none;
}
.chip.active { border-color: var(--gold); color: var(--gold); background: rgba(201,168,76,0.05); }
.chip input { display: none; }

.btn-gen {
  display: none;
  width: 100%;
  margin-top: 14px;
  padding: 17px;
  background: var(--gold);
  color: #0f0f0f;
  border: none;
  border-radius: 4px;
  font-family: 'Lato', sans-serif;
  font-size: 0.82rem;
  font-weight: 700;
  letter-spacing: 0.13em;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.25s;
  animation: riseIn 0.35s 0.08s cubic-bezier(0.16,1,0.3,1) both;
  position: relative; overflow: hidden;
}

.prog-wrap {
  display: none;
  width: 100%;
  margin-top: 14px;
  animation: riseIn 0.3s ease both;
}
.prog-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 9px;
}
.prog-label { font-size: 0.8rem; color: var(--muted); }
.prog-pct   { font-size: 0.8rem; color: var(--gold); font-weight: 700; }
.prog-track { height: 2px; background: var(--border); border-radius: 1px; overflow: hidden; }
.prog-fill  {
  height: 100%;
  background: linear-gradient(90deg, var(--gold), var(--gold2));
  width: 0%;
  transition: width 0.45s cubic-bezier(0.4,0,0.2,1);
}

#render-area {
  position: fixed; left: -9999px; top: 0;
  width: 794px;
  background: white;
  font-family: 'Lato', sans-serif;
  padding: 70px 76px;
  color: #1a1a1a;
  font-size: 13px;
  line-height: 1.78;
}
#render-area h1 { font-family: 'Playfair Display', serif; font-size: 30px; font-weight: 700; margin-bottom: 22px; color: #111; border-bottom: 2px solid #c9a84c; padding-bottom: 14px; line-height: 1.15; }
#render-area h2 { font-family: 'Playfair Display', serif; font-size: 21px; font-weight: 400; margin: 34px 0 11px; color: #111; border-bottom: 1px solid #eae6df; padding-bottom: 5px; }

@keyframes riseIn {
  from { opacity: 0; transform: translateY(18px); }
  to   { opacity: 1; transform: translateY(0); }
}
</style>
</head>
<body>
<div class="wrapper">
  <header>
    <div class="eyebrow">Herramienta de conversión</div>
    <h1>Markdown <em>al papel</em></h1>
    <p class="tagline">De .md a PDF elegante, en segundos</p>
  </header>

  <div class="drop-zone" id="drop-zone">
    <input type="file" id="file-input" accept=".md,.markdown,.txt">
    <div class="drop-icon">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.25" stroke-linecap="round" stroke-linejoin="round">
        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
        <polyline points="14 2 14 8 20 8"/>
        <line x1="12" y1="18" x2="12" y2="12"/>
        <polyline points="9 15 12 12 15 15"/>
      </svg>
    </div>
    <p class="drop-title">Arrastrá tu archivo aquí</p>
    <p class="drop-sub">o <strong id="browse-link">seleccioná desde tu equipo</strong></p>
  </div>

  <div class="preview-card" id="preview-card">
    <div class="preview-top">
      <div class="file-info">
        <div class="file-icon">📄</div>
        <div>
          <div class="file-name" id="fname">—</div>
          <div class="file-meta" id="fmeta">—</div>
        </div>
      </div>
      <button class="btn-remove" id="btn-remove">X</button>
    </div>
    <div class="preview-body" id="preview-body"></div>
    <div class="format-bar">
      <span class="fmt-label">Formato</span>
      <label class="chip active"><input type="radio" name="fmt" value="a4" checked> A4</label>
      <label class="chip"><input type="radio" name="fmt" value="letter"> Letter</label>
    </div>
  </div>

  <button class="btn-gen" id="btn-gen">Generar y descargar PDF</button>

  <div class="prog-wrap" id="prog-wrap">
    <div class="prog-row">
      <span class="prog-label" id="prog-label">Iniciando...</span>
      <span class="prog-pct"  id="prog-pct">0%</span>
    </div>
    <div class="prog-track"><div class="prog-fill" id="prog-fill"></div></div>
  </div>
</div>

<div id="render-area"></div>

<script>
const dropZone = document.getElementById('drop-zone');
const fileInput = document.getElementById('file-input');
const previewCard = document.getElementById('preview-card');
const btnGen = document.getElementById('btn-gen');
const progWrap = document.getElementById('prog-wrap');
const progFill = document.getElementById('prog-fill');
const progLabel = document.getElementById('prog-label');
const progPct = document.getElementById('prog-pct');
const renderArea = document.getElementById('render-area');

let mdContent = '', fileName = '';
const tick = (ms = 30) => new Promise(r => setTimeout(r, ms));

function setProgress(pct, label) {
  progFill.style.width = pct + '%';
  progLabel.textContent = label;
  progPct.textContent = pct + '%';
}

document.getElementById('browse-link').onclick = e => { e.stopPropagation(); fileInput.click(); };
dropZone.onclick = () => fileInput.click();
fileInput.addEventListener('change', () => { if (fileInput.files[0]) loadFile(fileInput.files[0]); });

function loadFile(file) {
  const r = new FileReader();
  r.onload = e => {
    mdContent = e.target.result;
    fileName = file.name.replace(/\.(md|markdown|txt)$/i, '');
    document.getElementById('fname').textContent = file.name;
    document.getElementById('fmeta').textContent = (file.size/1024).toFixed(1) + ' KB';
    document.getElementById('preview-body').textContent = mdContent.split('\n').slice(0, 50).join('\n');
    previewCard.style.display = 'block';
    btnGen.style.display = 'block';
  };
  r.readAsText(file, 'UTF-8');
}

btnGen.addEventListener('click', async () => {
  if (!mdContent) return;
  btnGen.style.display = 'none';
  progWrap.style.display = 'block';

  setProgress(10, 'Parseando Markdown…');
  renderArea.innerHTML = marked.parse(mdContent);
  await tick(100);

  const fmt = document.querySelector('input[name="fmt"]:checked').value;
  const pageW = fmt === 'a4' ? 210 : 215.9;
  const pageH = fmt === 'a4' ? 297 : 279.4;
  const { jsPDF } = window.jspdf;

  setProgress(40, 'Capturando contenido…');
  const canvas = await html2canvas(renderArea, { scale: 2, useCORS: true });

  const doc = new jsPDF({ orientation: 'p', unit: 'mm', format: fmt });
  const imgData = canvas.toDataURL('image/jpeg', 0.95);
  const imgW = pageW;
  const imgH = (canvas.height * imgW) / canvas.width;

  // --- CORRECCIÓN: Evitar página en blanco al final ---
  let yPos = 0;
  let page = 0;
  while (yPos < (imgH - 2)) {
    if (page > 0) doc.addPage();
    doc.addImage(imgData, 'JPEG', 0, -yPos, imgW, imgH);
    yPos += pageH;
    page++;
  }

  setProgress(90, 'Guardando…');
  doc.save((fileName || 'documento') + '.pdf');
  setProgress(100, '✓ ¡Listo!');
  setTimeout(() => { progWrap.style.display = 'none'; btnGen.style.display = 'block'; }, 2000);
});
</script>
</body>
</html>
```

### Archivo: `documentacion/readme_extendido.md`
```md
# README Extendido - Proyecto Espacia

Este documento complementa al `README.md` raíz con más detalles sobre la arquitectura, componentes y reglas de negocio.

## 1. Estructura general
- `index.html`, `dashboard.html`: vistas principales.
- `js/`: lógica cliente, con subcomponentes:
  - `agenda_v2.js`, `agenda/` (Módulos de Agenda v2)
  - `persistencia/abm_usuario.js` (CRUD de usuarios en frontend)
  - `pdf` (generación de PDF de tickets e informes)
  - `utils.js`, `config.js`.
- `css/`: temas separados (comunes, ABM, agenda, dashboard, login, etc.).
- `sw.js`: service worker PWA + cache.
- `netlify/functions`: backend de Netlify (Google Calendar + auth jwt + lógica de informes/usuarios/reservas).

## 2. Reglas de negocio clave
- Rol `admin`/`gestor` puede crear reservas en nombre de usuarios y cancelar cualquier evento.
- Rol `usuario` solo puede gestionar sus reservas propias.
- Reserva cancelada bajo rol admin: en Google Calendar aparezca `Reserva Cancelada por: <nombre_del_admin>`.
- `getCanceladoPor` define como titular si son mismos nombres; si no, `Administración`.

## 3. Detalle de Netlify Functions
- `login.js`: devuelde JWT + datos de usuario.
- `reservar.js`: POST para crear, DELETE para cancelar, GET para consultar; forzar `userToken.nombre`.
- `informe_reservas.js` / `informe_canceladas.js`: extraen eventos usando `googleapis`, parsean descripción.
- `gestionar_usuarios.js`, `listar_usuarios.js`, `update-usuarios.js`: admin user management.
- `backup-reservas.js` (programa backups vía scheduler).

## 4. PWA y cache-busting
- `sw.js` usa `CACHE_NAME` derivado de `APP_VERSION`.
- Cambios de versión en: `package.json`, `js/config.js`, `sw.js`.
- Renovar version automáticamente on release para forzar nueva caché.

## 5. Comportamiento de UI
- Agenda cargada cada 60 segundos.
- Mensajes de error de red integrados.
- Modal de confirmación para acciones críticas (cancelar, borrar usuario).

## 6. Mantenimiento y calidad
- Usa regla ESLint `no-unused-vars` para detectar código husmeado (como fragmentos no usados en `reservar.js`).
- Usa tests integrados con mock de `googleapis` en CI.
- Documenta cada nueva función en `js/agenda/readme_contenido_agenda.md`.

```

### Archivo: `documentacion/requerimientos_detallados.md`
```md
# Requerimientos Detallados - Proyecto Espacia

Este documento describe los requerimientos funcionales y no funcionales de manera exhaustiva para que un lector humano o una IA pueda recrear el proyecto desde cero.

## 1. Objetivo del sistema
- Gestionar reservas de espacios (consultorios, camas, salas) con un flujo de agenda.
- Integrar con Google Calendar para persistencia de eventos.
- Fomentar roles: usuario, gestor y admin.
- Proveer informes (reservas, canceladas) y reportes PDF.

## 2. Ámbitos funcionales
### 2.1 Autenticación y seguridad
- Login con documento/nomUsu + contraseña hash (SHA-256).
- JWT para la sesión (temas: `tipdocu`, `documento`, `email`, `rol`, `nombre`, `nomUsu`).
- Control de acceso a funciones con roles:
  - usuario: crear/agendar, cancelar propias reservas, ver sus reservas.
  - gestor/admin: ver todas, cancelar cualquiera, generar informes, gestionar usuarios.

### 2.2 Agenda de reservas
- Agenda con vista por día + listado hora/intervalo con slots.
- Selección de consultorio (C1..Cn) y rango horario.
- Reservas con descripción en Google Calendar:
  - `Reserva para: <nombre>`
  - `Email: <email>`
  - `Consultorio: <x>`
  - `Hora: ...` etc.
  - `Reserva realizada por: <nombre>` si es admin/gestor.

### 2.3 Cancelación
- Al cancelar un evento se marca `Cancelada - <summary>`.
- Agrega en `description`: `Reserva Cancelada por: <nombre>`.
- Registro debe usar `userToken.nombre`; no ámbito de cliente inseguro.
- Para informes, `informe_canceladas` extrae quien canceló.

### 2.4 Informes y PDF
- Funciones Netlify: `informe_reservas`, `informe_canceladas`.
- PDF incluya resumen y detalle con columna `Cancelado por`.

### 2.5 Gestión de usuarios
- ABM mediante Netlify Functions: `gestionar_usuarios`, `listar_usuarios`, `update-usuarios`.

## 3. Requisitos no funcionales
- PWA (service worker cache con `sw.js/CACHE_NAME` y `config.APP_VERSION`).
- Frontend Vanilla JS + responsive.
- Código modular en `js/agenda`, `js/persistencia`, `js/pdf`.
- Uso de Netlify Functions para backend.

## 4. Datos y estructura
- `js/config.js`: valores del negocio, roles, horarios, etc.
- `package.json`: dependencias `googleapis`, `jsonwebtoken`, `@netlify/functions`.

## 5. Flujo de despliegue
- Mantener versiones:
  - package.json `version`
  - js/config.js `APP_VERSION`
  - sw.js `CACHE_NAME`
- Se aumenta patch por defecto.

## 6. Criterios de aceptación
- Usuario crea/cancela reserva y se refleja en Google Calendar.
- Informe canceladas muestra nombre correcto: titular o admin.
- Compraración `getCanceladoPor` lógica de nombres: si cancela titular, retorna titular, sino `Administración`.
- Workflow de actualizacion de versión y cache PWA funcional.
- Documentación (README, manual_usuario, estructura) completa.

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

### Archivo: `imagenes/icon-192.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `imagenes/icon-512.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `imagenes/logo_demaria.png`
```png
[Archivo Binario o No Leíble]
```

### Archivo: `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
	<link rel="manifest" href="/manifest.json">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
    <meta name="theme-color" content="#2E7D32">
    <title>Espacia</title>
    <link rel="stylesheet" href="css/1_comunes.css">
    <link rel="stylesheet" href="css/2_login.css">
    <!-- FontAwesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
	<!--<link rel="icon" type="image/x-icon" href="/img/favicon.ico">-->
	<link rel="icon" href="data:,">
</head>
<body>
        <div class="login-container">
        <!-- Modal actualización de versión -->
        <div id="updateModal" style="display:none; position:fixed; top:0; left:0; width:100vw; height:100vh; background:rgba(0,0,0,0.5); z-index:9999;">
            <div style="background:#fff; padding:2em; margin:10% auto; width:300px; border-radius:8px; text-align:center;">
                <h2>¡Nueva versión disponible!</h2>
                <p>Recarga la página para actualizar Espacia.</p>
                <button onclick="location.reload()">Recargar</button>
            </div>
        </div>
        <!-- ZONA DE LOGO 1 -->
        <img src="imagenes/logo_demaria.png" alt="Logo De Maria Consultorios" style="height: 100px; max-width: 100%; object-fit: contain; margin-bottom: 20px; display: block; margin-left: auto; margin-right: auto;">
        <h2>Iniciar Sesión</h2>
        <div id="error" class="error"></div>
        <input type="text" id="usuario" placeholder="Usuario" autocomplete="username">
        <div style="position: relative; margin-bottom: 1.2rem;">
            <input type="password" id="contrasena" placeholder="Contraseña" autocomplete="current-password" style="margin-bottom: 0; padding-right: 40px;">
            <button type="button" id="toggle-contrasena" tabindex="-1" style="position: absolute; right: 12px; top: 50%; transform: translateY(-50%); background: transparent; border: none; padding: 0; margin: 0; width: auto; box-shadow: none; font-size: 1.3rem; cursor: pointer; color: #666;">
                <i class="fa fa-eye" id="icon-contrasena"></i>
            </button>
        </div>
        <button id="btnLogin">Ingresar</button>
        <div style="text-align: center; margin-top: 15px;">
            <a href="#" id="btnCambioPassVoluntario" style="color: #666; font-size: 0.85rem; text-decoration: underline;">Cambiar mi contraseña</a>
        </div>
        <div style="text-align: center; margin-top: 15px; font-size: 0.8rem; color: #888;">
            <span id="app-version-display">v-.-.-</span>
        </div>
        </div>
        <script>
            // Alternar visibilidad y icono FontAwesome para contraseña
            document.addEventListener('DOMContentLoaded', function() {
                const input = document.getElementById('contrasena');
                const btn = document.getElementById('toggle-contrasena');
                const icon = document.getElementById('icon-contrasena');
                if (btn && input && icon) {
                    btn.addEventListener('click', function() {
                        input.type = input.type === 'password' ? 'text' : 'password';
                        icon.className = input.type === 'password' ? 'fa fa-eye' : 'fa fa-eye-slash';
                    });
                }
            });
        </script>

    <!-- Modal sugerencia instalar PWA (Solo Móvil) -->
    <div id="pwa-install-modal" class="modal-overlay" style="display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); z-index: 10000; justify-content: center; align-items: flex-end; padding-bottom: 20px;">
        <div style="background: var(--white, white); color: var(--text-main, #333); width: 90%; max-width: 400px; padding: 25px; border-radius: 15px; text-align: center; box-shadow: 0 10px 30px rgba(0,0,0,0.3); animation: slideUp 0.3s ease;">
            <img src="imagenes/icon-192.png" alt="Icono" style="width: 72px; height: 72px; margin-bottom: 15px; border-radius: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.1);">
            <h3 style="margin: 0 0 10px 0; color: var(--primary, #2E7D32); font-size: 1.3rem;">¡Instala nuestra App!</h3>
            <p style="margin: 0 0 20px 0; color: #555; font-size: 0.95rem;">Para una experiencia más rápida, acceso directo desde tu pantalla de inicio y navegación en pantalla completa.</p>
            <div style="display: flex; gap: 10px;">
                <button id="pwa-cancel-btn" style="flex: 1; padding: 12px; background: #f0f0f0; color: #555; border: none; border-radius: 8px; font-weight: bold; cursor: pointer;">Quizás luego</button>
                <button id="pwa-install-btn" style="flex: 1; padding: 12px; background: var(--primary, #2E7D32); color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; box-shadow: 0 4px 10px rgba(0,0,0,0.2);">Instalar PWA</button>
            </div>
        </div>
    </div>
    
    <!-- Selector de temas flotante (Solo Desarrollo) -->
    <div class="theme-selector-container">
        <div id="themeMenu" class="theme-menu">
            <button class="theme-option" data-theme="modern">
                <span class="color-dot" style="background: #2E7D32;"></span> Verde Bosque (Soft UI)
            </button>
            <button class="theme-option" data-theme="warm">
                <span class="color-dot" style="background: #D97757;"></span> Cálido y Acogedor (Spa)
            </button>
            <button class="theme-option" data-theme="ocean">
                <span class="color-dot" style="background: #0284C7;"></span> Océano (Soft UI)
            </button>
            <button class="theme-option" data-theme="lavender">
                <span class="color-dot" style="background: #7C3AED;"></span> Lavanda (Soft UI)
            </button>
            <button class="theme-option" data-theme="sunset">
                <span class="color-dot" style="background: #EA580C;"></span> Atardecer (Cálido)
            </button>
            <button class="theme-option" data-theme="minimalist">
                <span class="color-dot" style="background: #57534E;"></span> Minimalista (Gris/Beige)
            </button>
            <button class="theme-option" data-theme="mustard">
                <span class="color-dot" style="background: #CA8A04;"></span> Mostaza (Luminoso)
            </button>
           <!-- <button class="theme-option" data-theme="dark">
                <span class="color-dot" style="background: #0F172A;"></span> Modo Oscuro (Elegante)
            </button>
            <button class="theme-option" data-theme="astigmatismo">
                <span class="color-dot" style="background: #2A2A35;"></span> Oscuro (Astigmatismo)
            </button>-->
        </div>
        <button id="themeToggle" class="theme-toggle-btn">
            🎨 Probar Diseños
        </button>
    </div>

    <script type="module" src="js/login_local.js"></script>
    <script>
        // Lógica para el selector de temas
        document.addEventListener('DOMContentLoaded', () => {
            const toggleBtn = document.getElementById('themeToggle');
            const themeMenu = document.getElementById('themeMenu');
            const themeOptions = document.querySelectorAll('.theme-option');
            const body = document.body;
            
            // Función para aplicar un tema
            function applyTheme(themeName) {
                // Limpiar clases de tema existentes
                body.classList.remove('theme-modern', 'theme-blue', 'theme-dark', 'theme-warm', 'theme-pisama', 'theme-ocean', 'theme-lavender', 'theme-sunset', 'theme-minimalist', 'theme-mustard', 'theme-astigmatismo');
                
                // Aplicar nueva clase si no es el clásico
                if (themeName !== 'classic') {
                    body.classList.add(`theme-${themeName}`);
                }
                
                // Guardar preferencia
                localStorage.setItem('espacia_theme', themeName);
                
                // Actualizar UI del menú
                themeOptions.forEach(opt => {
                    if (opt.dataset.theme === themeName) {
                        opt.classList.add('active');
                    } else {
                        opt.classList.remove('active');
                    }
                });
                
                // Disparar evento personalizado para que login_local.js/dashboard.js actualicen el fondo si es necesario
                document.dispatchEvent(new CustomEvent('themeChanged', { detail: { theme: themeName } }));

                // Actualizar el color de la barra de la PWA
                setTimeout(() => {
                    const metaThemeColor = document.querySelector('meta[name="theme-color"]');
                    if (metaThemeColor) {
                        const computedColor = getComputedStyle(document.body).getPropertyValue('--primary').trim();
                        // Fallback a verde por defecto si no lo encuentra rápido
                        metaThemeColor.setAttribute('content', computedColor || '#2E7D32');
                    }
                }, 50);
            }

            // Cargar tema guardado
            const savedTheme = localStorage.getItem('espacia_theme') || 'classic';
            applyTheme(savedTheme);

            // Toggle menú
            toggleBtn.addEventListener('click', (e) => {
                e.stopPropagation();
                themeMenu.classList.toggle('show');
            });

            // Cerrar menú al hacer clic fuera
            document.addEventListener('click', (e) => {
                if (!themeMenu.contains(e.target) && e.target !== toggleBtn) {
                    themeMenu.classList.remove('show');
                }
            });

            // Seleccionar tema
            themeOptions.forEach(option => {
                option.addEventListener('click', () => {
                    applyTheme(option.dataset.theme);
                    themeMenu.classList.remove('show');
                });
            });

            // ==========================================
            // Lógica PWA Modal Móvil
            // ==========================================
            // Registrar el Service Worker si no estaba registrado aquí (importante para que dispare beforeinstallprompt en el primer login)
            let refreshing = false;

            const pwaModal = document.getElementById('pwa-install-modal');
            const pwaInstallBtn = document.getElementById('pwa-install-btn');
            const pwaCancelBtn = document.getElementById('pwa-cancel-btn');

            function promptForUpdate(registration) {
                if (window.confirm('Hay una nueva versión disponible. ¿Deseas recargar ahora para actualizar?')) {
                    if (registration.waiting) {
                        registration.waiting.postMessage({ type: 'SKIP_WAITING' });
                    }
                }
            }

            function trackInstalling(worker, registration) {
                worker.addEventListener('statechange', () => {
                    if (worker.state === 'installed' && navigator.serviceWorker.controller) {
                        promptForUpdate(registration);
                    }
                });
            }

            if ('serviceWorker' in navigator) {
                window.addEventListener('load', function() {
                    navigator.serviceWorker.register('/sw.js').then(function(registration) {
                        if (registration.waiting) {
                            promptForUpdate(registration);
                        }
                        if (registration.installing) {
                            trackInstalling(registration.installing, registration);
                        }
                        registration.addEventListener('updatefound', () => {
                            trackInstalling(registration.installing, registration);
                        });
                    });

                    navigator.serviceWorker.addEventListener('controllerchange', function() {
                        if (refreshing) return;
                        refreshing = true;
                        window.location.reload();
                    });
                });
            }

            let deferredPrompt;

            window.addEventListener('beforeinstallprompt', (e) => {
                // Previene el infobar nativo
                e.preventDefault();
                deferredPrompt = e;
                
                // PWA no está instalada, revisar si es dispositivo móvil
                const isMobile = window.innerWidth <= 768 || /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
                
                // Si es móvil y no estamos ya en la app, mostramos la tarjeta
                if (isMobile) {
                    pwaModal.style.display = 'flex';
                }
            });

            pwaCancelBtn.addEventListener('click', () => {
                pwaModal.style.display = 'none';
            });

            pwaInstallBtn.addEventListener('click', async () => {
                pwaModal.style.display = 'none';
                if (deferredPrompt) {
                    deferredPrompt.prompt();
                    const { outcome } = await deferredPrompt.userChoice;
                    console.log(`User response to the install prompt: ${outcome}`);
                    deferredPrompt = null;
                }
            });

            window.addEventListener('appinstalled', () => {
                localStorage.setItem('pwa_installed', 'true');
                pwaModal.style.display = 'none';
                deferredPrompt = null;
            });

            // Si entra por navegador móvil pero ya está instalada previamente
            const isInstalled = localStorage.getItem('pwa_installed') === 'true';
            const isStandalone = window.matchMedia('(display-mode: standalone)').matches || navigator.standalone;
            const isMobileView = window.innerWidth <= 768 || /Android|webOS|iPhone|iPad|iPod|BlackBerry/i.test(navigator.userAgent);
            
            // Si sabemos que la descargó, está en navegador (no standalone) y es móvil:
            if (isInstalled && !isStandalone && isMobileView) {
                // El navegador nativo (Chrome/Safari) suele mostrar su propia barra superior "Abrir en aplicación". 
                // Hacer un botón custom que direccione a la PWA es complejo por seguridad del OS, 
                // dejaremos que el OS maneje el "Abrir App" nativo mediante el TWA/WebAPK banner.
            }
        });
    </script>
</body>
<script type="module">
import { SHOW_UPDATE_MODAL } from './js/config.js';
if ('serviceWorker' in navigator && SHOW_UPDATE_MODAL) {
    navigator.serviceWorker.addEventListener('controllerchange', function() {
        document.getElementById('updateModal').style.display = 'block';
    });
}
</script>
<script>
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.addEventListener('controllerchange', function() {
        document.getElementById('updateModal').style.display = 'block';
    });
}
</script>
</body>
</html>


```

### Archivo: `js/agenda/agenda_cancelar.js`
```javascript
// js/agenda/agenda_cancelar.js
// Lógica de cancelación de reserva: extrae datos del description,
// construye el objeto Reserva completo y genera el ticket PDF.

import { APP_CONFIG } from '../config.js';
import { mostrarToast } from '../utils.js';

// Extrae todos los campos del description de Google Calendar
// Formato: "Reserva para: X\nEmail: Y\nConsultorio: C4\nFecha: 17/03/2026\nHora: 09:00 a 10:30 hs\nIntervalo: 90 min\n\nReserva realizada por: Z"
export function extraerDatosDescription(summary, description) {
    const prefijo  = APP_CONFIG.espacios?.prefijoGeneral || 'C';
    const etiqueta = APP_CONFIG.espacios?.etiquetaGeneral || 'Consultorio';

    const matchSummary   = summary.match(new RegExp(`^${prefijo}\\d+:\\s*(.+)`, 'i'));
    const matchEmail     = description.match(/Email:\s*([^\n]+)/);
    const matchFecha     = description.match(/Fecha:\s*([^\n]+)/);
    const matchHorario   = description.match(/Hora:\s*([\d:]+)\s*a\s*([\d:]+)\s*hs/);
    const matchHoraSimp  = description.match(/Hora:\s*([\d:]+)/);
    const matchIntervalo = description.match(/Intervalo:\s*(\d+)/);
    const matchEspacio   = description.match(new RegExp(`(?:Consultorio|${etiqueta}):\\s*([^\\n]+)`, 'i'));
    const matchCreadoPor = description.match(/Reserva realizada por:\s*([^\n]+)/);

    // Hora fin: desde el rango o calculada con el intervalo
    let horaInicio = matchHorario ? matchHorario[1] : (matchHoraSimp ? matchHoraSimp[1] : '');
    let horaFin    = matchHorario ? matchHorario[2] : '';
    if (!horaFin && horaInicio && matchIntervalo) {
        const [h, m] = horaInicio.split(':').map(Number);
        const fObj = new Date(2000, 0, 1, h, m);
        fObj.setMinutes(fObj.getMinutes() + parseInt(matchIntervalo[1]));
        horaFin = `${fObj.getHours().toString().padStart(2,'0')}:${fObj.getMinutes().toString().padStart(2,'0')}`;
    }

    // Normalizar espacio: "C4" → "Consultorio 4"
    let espacioDisplay = matchEspacio ? matchEspacio[1].trim() : '';
    if (espacioDisplay && espacioDisplay[0].toUpperCase() === prefijo.toUpperCase()) {
        espacioDisplay = etiqueta + ' ' + espacioDisplay.slice(1);
    }

    return {
        usuario:    matchSummary  ? matchSummary[1].trim()   : '',
        email:      matchEmail    ? matchEmail[1].trim()     : '',
        fecha:      matchFecha    ? matchFecha[1].trim()     : '',
        horaInicio,
        horaFin,
        intervalo:  matchIntervalo ? matchIntervalo[1]       : '',
        espacio:    espacioDisplay,
        creadoPor:  matchCreadoPor ? matchCreadoPor[1].trim(): ''
    };
}

export async function cancelarReserva({ id, summary, description, nombreUsuario, esAdmin, created, recargarGrilla, modal, btnConfirmar }) {
    try {
        const datos = extraerDatosDescription(summary, description);
        const { Reserva } = await import('../modelo/reserva.js');

        const reserva      = new Reserva();
        reserva.id         = id;
        reserva.usuario    = datos.usuario    || nombreUsuario;
        reserva.email      = datos.email;
        reserva.fecha      = datos.fecha;
        reserva.h_inicio   = datos.horaInicio;
        reserva.h_fin      = datos.horaFin;
        reserva.intervalo  = datos.intervalo;
        reserva.lugar      = datos.espacio;
        // No sobrescribir `creadoPor` cuando un admin cancela: conservar el valor original
        reserva.creadoPor  = datos.creadoPor || null;
        // Registrar quién ejecuta la cancelación
        reserva.canceladoPor  = nombreUsuario;
        reserva.fechaCreacion = created || null;

        const resp = await reserva.cancelar();
        if (resp.ok) {
            mostrarToast('✅ Reserva cancelada.', 'success');
            try {
                console.log("fechaCreacion:", reserva.fechaCreacion);
                await reserva.imprimirTicketCancelacion();
            } catch (e) { console.error('Error generando ticket cancelación:', e); }
            modal.remove();
            await recargarGrilla();
        } else {
            const err = await resp.json();
            mostrarToast('❌ Error: ' + (err.error || 'No se pudo cancelar'), 'error');
            btnConfirmar.disabled = false;
            btnConfirmar.innerText = 'Cancelar Reserva';
        }
    } catch (error) {
        console.error('Error en cancelarReserva:', error);
        mostrarToast('Error de conexión', 'error');
        btnConfirmar.disabled = false;
        btnConfirmar.innerText = 'Cancelar Reserva';
    }
}
```

### Archivo: `js/agenda/agenda_detalle.js`
```javascript
// js/agenda/agenda_detalle.js
// Modal de detalle de una reserva existente: muestra datos, descarga PDF
// y delega la cancelación a agenda_cancelar.js

import { APP_CONFIG } from '../config.js';
import { mostrarToast, mostrarModalConfirmacion, getRealizadoPor } from '../utils.js';
import { cancelarReserva, extraerDatosDescription } from './agenda_cancelar.js';

// Formatea el description de Google Calendar para mostrarlo en HTML
function formatearDescription(description) {
    if (!description) return '';

    // Reemplazar creador por Administracion cuando sea un admin/gestor reservando para otro titular
    let descriptionFinal = description;
    const reservaParaMatch = descriptionFinal.match(/Reserva para:\s*([^\n<]+)/i);
    const reservaParaNombre = reservaParaMatch ? reservaParaMatch[1].trim() : '';
    const realizadoPor = getRealizadoPor({ usuario: reservaParaNombre, description: descriptionFinal });
    if (realizadoPor) {
        descriptionFinal = descriptionFinal.replace(/Reserva realizada por:\s*[^\n]*/i, `Reserva realizada por: ${realizadoPor}`);
    }

    if (!descriptionFinal.includes('\n')) {
        return descriptionFinal
            .replace(/Email:/g,                '<br><strong>Email:</strong>')
            .replace(/Consultorio:/g,           '<br><strong>Consultorio:</strong>')
            .replace(/Fecha:/g,                 '<br><strong>Fecha:</strong>')
            .replace(/Hora:/g,                  '<br><strong>Hora:</strong>')
            .replace(/Intervalo:/g,             '<br><strong>Intervalo:</strong>')
            .replace(/Reserva realizada por:/g, '<br><br><strong>Reserva realizada por:</strong>')
            .replace(/Reserva Cancelada por:/g, '<br><br><strong style="color:red;">Reserva Cancelada por:</strong>');
    }
    return descriptionFinal.replace(/\n/g, '<br>');
}

// Obtiene el nombre y rol del usuario que está operando en este momento
function obtenerUsuarioActual() {
    try {
        const userActual = JSON.parse(localStorage.getItem('usuarioActual'))
                        || JSON.parse(sessionStorage.getItem('usuarioActual'));
        if (!userActual) return { nombreUsuario: '', esAdminCancelador: false };

        const esAdminCancelador = (userActual.rol === 'admin' || userActual.rol === 'gestor');
        // nombreUsuario = quien ejecuta la cancelación (el usuario logueado, sea admin o profesional)
        // NUNCA el valor del combo — ese es el profesional de la reserva, no quien cancela
        const nombreUsuario = userActual.nombre || userActual.email.split('@')[0].replace(/\./g, ' ');
        return { nombreUsuario, esAdminCancelador };
    } catch {
        return { nombreUsuario: '', esAdminCancelador: false };
    }
}

window.mostrarDetalleReserva = function(id, summary, description, created, recargarGrilla) {
    const formattedDescription = formatearDescription(description);

    const contenido = `
        <div style="margin-bottom:20px;color:var(--text-main,#333);font-size:1rem;line-height:1.5;text-align:left;">
            <strong style="font-size:1.1rem;color:var(--primary,#2c3e50);">${summary}</strong><br><br>
            ${formattedDescription}
        </div>
        <button id="btn-descargar-pdf-modal" style="width:100%;padding:10px;background:#27ae60;color:white;border:none;border-radius:8px;cursor:pointer;font-weight:bold;margin-bottom:15px;display:flex;align-items:center;justify-content:center;gap:8px;">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path><polyline points="7 10 12 15 17 10"></polyline><line x1="12" y1="15" x2="12" y2="3"></line></svg>
            Descargar PDF
        </button>
    `;

    mostrarModalConfirmacion(
        "Detalles de la Reserva",
        contenido,
        "Cancelar Reserva",
        async (btnConfirmar, btnCancelar, modal) => {
            btnConfirmar.disabled = true;
            btnConfirmar.innerText = 'Cancelando...';
            const { nombreUsuario, esAdminCancelador } = obtenerUsuarioActual();
            await cancelarReserva({
                id, summary, description,
                nombreUsuario,
                esAdmin: esAdminCancelador,
                created,
                modal, btnConfirmar,
                recargarGrilla
            });
        },
        "Aceptar",
        "#e74c3c"
    );

    // Descargar PDF de comprobante de reserva
    document.getElementById('btn-descargar-pdf-modal').onclick = async () => {
        try {
            const datos = extraerDatosDescription(summary, description);
            const { generarTicketReserva } = await import('../pdf/pdf_ticket_reserva.js');
            await generarTicketReserva({
                usuario:           datos.usuario,
                email:             datos.email,
                fecha:             datos.fecha,
                inicio:            datos.horaInicio,
                fin:               datos.horaFin,
                intervalo:         datos.intervalo,
                espacio:           datos.espacio,
                creadoPor:         datos.creadoPor,
                realizadaPorAdmin: !!datos.creadoPor,
                fechaCreacion:     created || null
            });
        } catch (e) {
            console.error("Error al generar PDF:", e);
            mostrarToast("Error al generar el PDF", "error");
        }
    };
};
```

### Archivo: `js/agenda/agenda_reservar.js`
```javascript
// js/agenda/agenda_reservar.js
// Flujo completo de nueva reserva: modal de duración, validaciones,
// construcción del objeto Reserva, guardar y ticket de confirmación.

import { APP_CONFIG } from '../config.js';
import { mostrarToast } from '../utils.js';

// Estado compartido — se inyecta desde agenda_v2.js al inicializar
let _state = null;
export function initReservar(state) { _state = state; }

window.iniciarReservaV2 = async function(consultorio, fecha, hora) {
    const { esAdmin, user, usuariosLista, usuarioSeleccionadoAdmin, recargarGrilla } = _state;

    // Validar día laboral
    const fechaObjVal = new Date(fecha + 'T00:00:00');
    if (APP_CONFIG.diasLaborales && !APP_CONFIG.diasLaborales.includes(fechaObjVal.getDay())) {
        mostrarToast("No se pueden agendar turnos en días no laborales.", "warning");
        return;
    }

    // Validar que no sea en el pasado (solo usuarios comunes)
    if (!esAdmin) {
        const [hStr, mStr] = hora.split(':');
        if (new Date(`${fecha}T${hStr}:${mStr}:00`) < new Date()) {
            mostrarToast("No puedes agendar un turno en un horario que ya pasó.", "warning");
            return;
        }
    }

    // Calcular opciones de duración válidas según horario de cierre
    const opcionesIntervalos = APP_CONFIG.horarios.intervalos || [60, 90];
    const dayOfWeek = new Date(fecha + 'T00:00:00').getDay();
    let horaCierre = APP_CONFIG.horarios.fin;
    if (APP_CONFIG.horariosEspeciales?.[dayOfWeek]) {
        horaCierre = APP_CONFIG.horariosEspeciales[dayOfWeek].fin;
    }
    const [hStr, mStr] = hora.split(':');
    const minutosDisponibles = (horaCierre * 60) - (parseInt(hStr, 10) * 60 + parseInt(mStr, 10));
    let opcionesValidas = opcionesIntervalos.filter(min => min <= minutosDisponibles);
    if (APP_CONFIG.horarios.restriccionCierreSoloUsuarios && esAdmin) {
        opcionesValidas = opcionesIntervalos;
    }
    if (opcionesValidas.length === 0) {
        mostrarToast(`No hay tiempo suficiente antes del cierre (${horaCierre}:00 hs).`, "warning");
        return;
    }

    // Modal de selección de duración
    const intervalo = await seleccionarDuracion(consultorio, hora, opcionesValidas);
    if (!intervalo) return;

    // Determinar usuario a reservar
    let emailReserva = user.email;
    let nombreReserva = user.nombre || user.email;
    let creadoPor = null;

    if (esAdmin) {
        if (!usuarioSeleccionadoAdmin) {
            mostrarToast("⚠️ Seleccione un usuario del combo 'Agendando para:' antes de reservar.", "warning");
            return;
        }
        // Asignar siempre creadoPor cuando la acción la realiza un admin (incluso si selecciona su propio usuario)
        const sel = usuariosLista.find(u => u.email === usuarioSeleccionadoAdmin);
        if (sel) {
            emailReserva = sel.email;
            nombreReserva = sel.nombre || sel.email;
        }
        creadoPor = `${user.nombre} <${user.email}>`;
    }

    const [hh, mm] = hora.split(':').map(Number);
    const gridContainer = document.querySelector('#grid-container');
    const originalHTML = gridContainer.innerHTML;
    gridContainer.innerHTML = '<div style="padding:40px;text-align:center;color:#666;">⌛ Procesando reserva...</div>';

    try {
        // Determinar color del espacio
        let colorAUsar = "1";
        if (APP_CONFIG.espacios?.items) {
            const esp = APP_CONFIG.espacios.items.find(i => parseInt(i.id) === parseInt(consultorio));
            if (esp) colorAUsar = esp.colorGoogle;
        } else if (APP_CONFIG.coloresConsultorios) {
            colorAUsar = APP_CONFIG.coloresConsultorios[consultorio] || "1";
        }

        const { Reserva } = await import('../modelo/reserva.js');
        const reserva = new Reserva(
            nombreReserva,
            fecha,
            `${hh.toString().padStart(2,'0')}:${mm.toString().padStart(2,'0')}`,
            consultorio,
            creadoPor,
            null,
            emailReserva,
            null,
            intervalo,
            colorAUsar,
            APP_CONFIG.espacios?.prefijoGeneral || 'C',
            APP_CONFIG.espacios?.etiquetaGeneral || 'Consultorio'
        );

        const resp = await reserva.guardar();
        // Leer el body UNA sola vez — resp.text() solo se puede consumir una vez
        let respJson = null;
        let respText = null;
        try {
            respText = await resp.text();
            respJson = JSON.parse(respText);
        } catch {}

        if (resp.ok) {
            mostrarToast("✅ Turno agendado correctamente.", "success");
            try {
                reserva.fechaCreacion = respJson?.created || null;
                await reserva.imprimirTicketReserva();
            } catch(e) {
                console.error("Error al generar comprobante:", e);
            }
            await recargarGrilla();
        } else if (resp.status === 409) {
            // Conflicto de concurrencia — otro usuario reservó el mismo slot al mismo tiempo
            mostrarToast("⚠️ Este horario acaba de ser tomado por otro usuario. La agenda se actualizó.", "warning");
            await recargarGrilla(); // Recargar para mostrar el slot ya ocupado
        } else {
            mostrarToast("❌ Error: " + (respJson?.error || "No se pudo completar la reserva."), "error");
            await recargarGrilla();
        }
    } catch (e) {
        console.error("Error de conexión:", e);
        mostrarToast("Error de conexión.", "error");
        gridContainer.innerHTML = originalHTML;
    }
};

// Modal de selección de duración — devuelve una Promise con los minutos elegidos o null
function seleccionarDuracion(consultorio, hora, opciones) {
    return new Promise((resolve) => {
        const modalHtml = `
            <div id="modal-duracion" style="position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.5);display:flex;justify-content:center;align-items:center;z-index:9999;">
                <div style="background:var(--white,white);color:var(--text-main,#333);padding:25px;border-radius:12px;box-shadow:0 4px 15px rgba(0,0,0,0.2);max-width:400px;width:90%;text-align:center;">
                    <h3 style="margin-top:0;color:var(--primary,#2c3e50);">Seleccionar Duración</h3>
                    <p style="margin-bottom:20px;color:#666;">¿Cuánto tiempo desea reservar el espacio ${consultorio} a las ${hora}?</p>
                    <div style="display:flex;flex-direction:column;gap:10px;margin-bottom:20px;">
                        ${opciones.map(min => `
                            <button class="btn-duracion-opcion" data-minutos="${min}" style="padding:12px;font-size:1.1rem;background:var(--bg-zebra,#f8f9fa);border:1px solid var(--border,#ccc);border-radius:8px;cursor:pointer;transition:all 0.2s;">
                                ${min} minutos
                            </button>
                        `).join('')}
                    </div>
                    <div style="display:flex;justify-content:space-between;gap:10px;">
                        <button id="btn-aceptar-duracion" style="flex:1;padding:10px 20px;background:var(--primary,#007bff);color:white;border:none;border-radius:8px;cursor:pointer;font-weight:bold;" disabled>Aceptar</button>
                        <button id="btn-cancelar-duracion" style="flex:1;padding:10px 20px;background:#e74c3c;color:white;border:none;border-radius:8px;cursor:pointer;font-weight:bold;">Cancelar</button>
                    </div>
                </div>
            </div>`;

        document.body.insertAdjacentHTML('beforeend', modalHtml);
        const modal = document.getElementById('modal-duracion');
        const btnAceptar = document.getElementById('btn-aceptar-duracion');
        let seleccion = null;

        modal.querySelectorAll('.btn-duracion-opcion').forEach(btn => {
            btn.addEventListener('click', () => {
                modal.querySelectorAll('.btn-duracion-opcion').forEach(b => {
                    b.style.background = 'var(--bg-zebra,#f8f9fa)';
                    b.style.color = 'var(--text-main,black)';
                    b.classList.remove('seleccionado');
                });
                btn.style.background = 'var(--primary,#007bff)';
                btn.style.color = 'white';
                btn.classList.add('seleccionado');
                seleccion = parseInt(btn.getAttribute('data-minutos'), 10);
                btnAceptar.disabled = false;
            });
            btn.addEventListener('mouseover', () => { if (!btn.classList.contains('seleccionado')) btn.style.opacity = '0.8'; });
            btn.addEventListener('mouseout',  () => { if (!btn.classList.contains('seleccionado')) { btn.style.opacity = '1'; btn.style.background = 'var(--bg-zebra,#f8f9fa)'; } });
        });

        btnAceptar.addEventListener('click', () => { if (seleccion) { resolve(seleccion); modal.remove(); } });
        document.getElementById('btn-cancelar-duracion').addEventListener('click', () => { resolve(null); modal.remove(); });
    });
}
```

### Archivo: `js/agenda/readme_contenido _agenda.md`
```md
# Estructura de `js/agenda`

Este directorio aloja la lógica modular de la Agenda V2 con enfoque MVC ligero y funciones de reserva/cancelación. Está pensado como un conjunto de módulos reutilizables que manejan estado, API, vista y acciones de usuario.

- `agenda_estado.js`
  - Estado global de agenda (fecha actual, usuario logueado, rol, selección admin, lista de usuarios y filtros de espacio).
  - Funciones auxiliares: formateo de fecha, validación de día laboral, cálculo de intervalo de horas y estado de disponibilidad.

- `agenda_api.js`
  - Llamadas a Netlify Functions (`get-usuarios`, `informe_reservas`, `informe_canceladas`, `reservar`, etc.).
  - Centraliza `fetch` con headers de autenticación y manejo de errores (reintentos ligeros, timeout configurable).

- `agenda_vista.js`
  - Render de interfaz inicial: encabezado, fecha, mensajes de no laboral/errores y botonera de interés.
  - Capa de presentación independiente de la lógica, facilita adaptación a temas y a futuros frameworks.

- `agenda_controlador.js`
  - Orquesta flujo principal.
  - Carga datos, inicializa componente, controla recarga periódica (60s) y lógica de cuadrícula.
  - Calcula consultorios visibles y monta reservas en pantalla, con soporte de paginación y filtros.

- `agenda_reservar.js` (existente)
  - Lógica de reserva/cancelación para UI de booking.
  - Integración con `window.iniciarReservaV2`, validaciones de intervalo, y guardado de `creadoPor` cuando el operador es admin/gestor.

- `agenda_cancelar.js` (existente)
  - Interacción para cancelar reservas.
  - Genera la marca `Cancelada - ...` y llama a `reservar` DELETE.

- `agenda_detalle.js` (existente)
  - Muestra detalle de una reserva, con campos de resumen, estado y botón de impresión/descarga.
  - Formatea los metadatos del evento (`Reserva realizada por`, `Reserva cancelada por`, `consultorio`, etc.).

## Notas de mantenimiento

- Las funciones clave se llaman desde `agenda_v2.js` (entry point principal de la agenda). No omitir su carga.
- Si se renombra CSS o scripts, actualizar también `sw.js` y `js/config.js` para forzar cache-bust.
- Observa que el módulo `agenda_reservar.js` dejó el código de propiedad `userEmail` en `extendedProperties` y puede necesitar refactor según privacidad.

## Recomendaciones de refactor

- Consolidar selectores y DOM-manipulación en `agenda_vista.js`.
- Convertir las llamadas a Netlify a un wrapper reutilizable en `agenda_api.js`.
- Añadir tests unitarios (mock de `fetch` y `googleapis` para los endpoints).

```

### Archivo: `js/agenda_v2.js`
```javascript
import { APP_CONFIG } from './config.js';
import { getAuthHeaders, mostrarToast, mostrarModalConfirmacion, createComboUsuarios } from './utils.js';
import { initReservar } from './agenda/agenda_reservar.js';
import './agenda/agenda_detalle.js';


let currentDate = new Date();
let usuarioSeleccionadoAdmin = null;
let usuariosLista = [];
let esAdmin = false;
let user = null;

export async function renderAgendaV2(container) {
    try {
        user = JSON.parse(localStorage.getItem('usuarioActual')) || JSON.parse(sessionStorage.getItem('usuarioActual'));
        esAdmin = (user && (user.rol === 'admin' || user.rol === 'gestor'));

        if (esAdmin) {
            try {
                const resp = await fetch('/.netlify/functions/get-usuarios', {
                    headers: getAuthHeaders()
                });
                const js = await resp.json();
                if (Array.isArray(js)) {
                    let filtrados = js.filter(u => u.rol === 'usuario');
                    usuariosLista = filtrados.slice().sort((a, b) => (a.nombre || '').localeCompare(b.nombre || ''));
                }
            } catch (e) {
                console.error("Error cargando usuarios:", e);
            }
        }

        renderUI(container);
        await loadDataAndRenderGrid(container);

        // Inyectar estado compartido a los módulos de agenda
        initReservar({
            esAdmin,
            user,
            usuariosLista,
            get usuarioSeleccionadoAdmin() { return usuarioSeleccionadoAdmin; },
            recargarGrilla: () => loadDataAndRenderGrid(container)
        });

        // Auto-refresh cada 60 segundos para mantener la grilla actualizada
        // con reservas de otros usuarios — se cancela si el contenedor se desmonta
        if (window.__agendaRefreshInterval) clearInterval(window.__agendaRefreshInterval);
        window.__agendaRefreshInterval = setInterval(() => {
            if (document.contains(container)) {
                loadDataAndRenderGrid(container);
            } else {
                clearInterval(window.__agendaRefreshInterval);
            }
        }, 60000);

    } catch (error) {
        container.innerHTML = `<p style="color:red;">Error al cargar la interfaz: ${error.message}</p>`;
    }
}

function renderUI(container) {
    const year = currentDate.getFullYear();
    const month = String(currentDate.getMonth() + 1).padStart(2, '0');
    const day = String(currentDate.getDate()).padStart(2, '0');
    const dateStr = `${year}-${month}-${day}`;
    
    let adminControls = '';
    let combo;
    if (esAdmin && usuariosLista.length > 0) {
        combo = createComboUsuarios(usuariosLista, {
            id: 'combo-usuario-reserva',
            name: 'usuario-reserva',
            clase: '',
            includeTodos: false,
            valueKey: 'email',
            textKey: 'nombre'
        });
        // Agregar opción 'Mí mismo' si corresponde
        if (!APP_CONFIG.comboAgendaSoloUsuarios) {
            const optMiMismo = document.createElement('option');
            optMiMismo.value = user.email;
            optMiMismo.textContent = `Mí mismo (${user.nombre || user.email})`;
            combo.insertBefore(optMiMismo, combo.firstChild);
        }
        // Agregar opción de placeholder
        const optPlaceholder = document.createElement('option');
        optPlaceholder.value = '';
        optPlaceholder.textContent = '-- Seleccione un usuario --';
        optPlaceholder.disabled = true;
        optPlaceholder.selected = true;
        combo.insertBefore(optPlaceholder, combo.firstChild);
        combo.style.padding = '6px';
        combo.style.borderRadius = '4px';
        combo.style.border = '1px solid var(--border)';
        combo.style.background = 'var(--white)';
        combo.style.color = 'var(--text-main)';
        combo.style.fontSize = '0.9rem';
        adminControls = `<div style="display: flex; align-items: center; gap: 10px; color: var(--text-main);">
            <label for='combo-usuario-reserva' style='font-weight:600; font-size: 0.9rem;'>Agendando para:</label>
        </div>`;
    }

    container.innerHTML = `
        <div class="agenda-v2-container">
            <div class="agenda-v2-header" style="display: flex; flex-wrap: wrap; justify-content: space-between; align-items: center; margin-bottom: 20px; gap: 10px;">
                <h2 class="module-title">Agenda</h2>
                <div style="display: flex; align-items: center; gap: 10px; flex-wrap: wrap;">
                    <div style="display: flex; align-items: center; gap: 10px;">
                        <span style="font-weight: bold; font-size: 1.1rem; color: var(--text-main);" id="display-date"></span>
                        <input type="date" id="date-picker" class="agenda-v2-date-picker" value="${dateStr}" style="padding: 6px 10px;">
                    </div>
                </div>
                <div id="combo-usuario-reserva-container"></div>
            </div>
            <div id="grid-container" class="agenda-v2-grid-wrapper" style="margin-top: 15px;">
                <div style="padding: 40px; text-align: center; color: #666;">Cargando disponibilidad...</div>
            </div>
        </div>
    `;
    // Insertar el combo generado
    if (esAdmin && usuariosLista.length > 0) {
        const comboContainer = container.querySelector('#combo-usuario-reserva-container');
        comboContainer.innerHTML = adminControls;
        comboContainer.appendChild(combo);
    }

    updateDateDisplay(container);
    attachEventListeners(container);
}

function updateDateDisplay(container) {
    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
    const displayStr = currentDate.toLocaleDateString('es-UY', options);
    container.querySelector('#display-date').textContent = displayStr.charAt(0).toUpperCase() + displayStr.slice(1);
    
    // Ajustar la fecha para el input type="date" considerando la zona horaria local
    const year = currentDate.getFullYear();
    const month = String(currentDate.getMonth() + 1).padStart(2, '0');
    const day = String(currentDate.getDate()).padStart(2, '0');
    container.querySelector('#date-picker').value = `${year}-${month}-${day}`;
}

function attachEventListeners(container) {

    container.querySelector('#date-picker').addEventListener('change', (e) => {
        if (e.target.value) {
            // Fix timezone offset issue when parsing date string
            const [year, month, day] = e.target.value.split('-');
            currentDate = new Date(year, month - 1, day);
            updateDateDisplay(container);
            loadDataAndRenderGrid(container);
        }
    });

    const combo = container.querySelector('#combo-usuario-reserva');
    if (combo) {
        combo.addEventListener('change', (e) => {
            usuarioSeleccionadoAdmin = e.target.value;
        });
        usuarioSeleccionadoAdmin = combo.value;
    }
}

async function loadDataAndRenderGrid(container) {
    const gridContainer = container.querySelector('#grid-container');
    gridContainer.innerHTML = '<div style="padding: 40px; text-align: center; color: #666;">⌛ Consultando disponibilidad en tiempo real...</div>';

    const year = currentDate.getFullYear();
    const month = String(currentDate.getMonth() + 1).padStart(2, '0');
    const day = String(currentDate.getDate()).padStart(2, '0');
    const dateStr = `${year}-${month}-${day}`;
    
    // Determinar espacios a mostrar (Marca Blanca)
    let consultoriosAMostrar = [];
    if (APP_CONFIG.espacios && APP_CONFIG.espacios.items) {
        consultoriosAMostrar = APP_CONFIG.espacios.items
            .filter(item => item.id !== "0") // Excluir "Todos" p.ej. si existiera
            .map(item => parseInt(item.id))
            .filter(num => {
                if (num === 1) return esAdmin;
                return true;
            });
    } else {
        // Fallback temporal legacy
        consultoriosAMostrar = (APP_CONFIG.consultorios || []).filter(num => {
            if (num === 0) return false;
            if (num === 1) return esAdmin;
            return true;
        });
    }

    // Filtrar consultorios por consPref para roles usuarios
    if (user.rol === 'usuario' && Array.isArray(user.consPref) && user.consPref.length > 0) {
        consultoriosAMostrar = consultoriosAMostrar.filter(num => user.consPref.includes(num));
    }

    try {
        // Fetch data for ALL espacios para esta fecha
        // Enviamos el prefijo Gcal para que el backend lo sepa
        const prefix = APP_CONFIG.espacios?.prefijoGeneral || 'C';
        const url = `/.netlify/functions/informe_reservas?fechaInicio=${dateStr}&fechaFin=${dateStr}&prefijoGcal=${prefix}`;
        const resp = await fetch(url, {
            headers: getAuthHeaders()
        });
        if (!resp.ok) throw new Error(`Error de red: ${resp.status}`);
        const data = await resp.json();
        const reservas = data.reservas || [];

        renderGrid(gridContainer, consultoriosAMostrar, reservas, dateStr);

    } catch (error) {
        console.error("Error loading grid data:", error);
        gridContainer.innerHTML = `<div style="padding: 20px; color: red; text-align: center;">Error al cargar datos: ${error.message}</div>`;
    }
}

function renderGrid(gridContainer, consultorios, reservas, dateStr) {
    const dayOfWeek = currentDate.getDay(); // 0 = Sunday, 1 = Monday...
    
    // Validar que sea un día laboral según config.js
    if (APP_CONFIG.diasLaborales && !APP_CONFIG.diasLaborales.includes(dayOfWeek)) {
        // En lugar de renderizar la grilla, mostramos un mensaje
        gridContainer.innerHTML = `
            <div style="padding: 50px 20px; text-align: center; color: var(--text-muted, #666); background: var(--bg-zebra, #f8f9fa); border-radius: var(--radius, 8px); border: 1px solid var(--border, #eee);">
                <span style="font-size: 3rem; display: block; margin-bottom: 10px;">🌴</span>
                <h3 style="color: var(--primary, #2c3e50); margin-bottom: 10px;">Día cerrado</h3>
                <p style="font-size: 1.1rem;">La agenda no permite reservas los <strong>${currentDate.toLocaleDateString('es-UY', {weekday: 'long'})}s</strong> porque no es un día laboral.</p>
            </div>
        `;
        return;
    }

    // Get hours config
    let startHour = APP_CONFIG.horarios.inicio;
    let endHour = APP_CONFIG.horarios.fin;
    
    if (APP_CONFIG.horariosEspeciales && APP_CONFIG.horariosEspeciales[dayOfWeek]) {
        startHour = APP_CONFIG.horariosEspeciales[dayOfWeek].inicio;
        endHour = APP_CONFIG.horariosEspeciales[dayOfWeek].fin;
    }

    // Create grid layout
    // 1 column for time + 1 column per consultorio
    // Usamos 1fr para que los consultorios ocupen todo el ancho disponible equitativamente
    const gridTemplateColumns = `60px repeat(${consultorios.length}, minmax(110px, 1fr))`;
    
    let html = '';

    // MENU DE PESTAÑAS (SOLO VISIBLE EN MOVILES)
    html += `<div class="agenda-mobile-tabs hide-on-desktop">`;
    const labelGeneral = APP_CONFIG.espacios?.etiquetaGeneral || 'Cons';
    
    consultorios.forEach((cons, idx) => {
        // Encontrar el nombre corto si existe
        let nombreOpcion = `${labelGeneral} ${cons}`;
        if (APP_CONFIG.espacios && APP_CONFIG.espacios.items) {
            const espacioItem = APP_CONFIG.espacios.items.find(i => parseInt(i.id) === cons);
            if (espacioItem && espacioItem.nombreCorto) nombreOpcion = espacioItem.nombreCorto;
        }
        
        html += `<button class="mobile-tab-btn ${idx === 0 ? 'active' : ''}" data-target-cons="${cons}">${nombreOpcion}</button>`;
    });
    html += `</div>`;
    
    // Contenedor de grilla con el estado inicial del primer consultorio para la vista móvil
    html += `<div class="agenda-v2-grid" data-active-mobile-cons="${consultorios[0]}" style="grid-template-columns: ${gridTemplateColumns};">`;

    // Header Row
    html += `<div class="grid-header-cell" style="grid-column: 1; grid-row: 1; font-size: 0.8rem; padding: 10px 2px;">Hora</div>`;
    consultorios.forEach((cons, index) => {
        let nombreColumna = `${labelGeneral} ${cons}`;
        if (APP_CONFIG.espacios && APP_CONFIG.espacios.items) {
            const espacioItem = APP_CONFIG.espacios.items.find(i => parseInt(i.id) === cons);
            if (espacioItem && espacioItem.nombreCorto) nombreColumna = espacioItem.nombreCorto;
        }
        html += `<div class="grid-header-cell" data-cons="${cons}" style="grid-column: ${index + 2}; grid-row: 1; font-size: 0.85rem; padding: 10px 2px;">${nombreColumna}</div>`;
    });

    // Time Rows (30-minute intervals)
    let row = 2;
    for (let h = startHour; h < endHour; h++) {
        for (let m = 0; m < 60; m += 30) {
            const timeLabel = `${h.toString().padStart(2, '0')}:${m.toString().padStart(2, '0')}`;
            const isHalfHour = m === 30;
            const textStyle = isHalfHour ? 'color: #888;' : '';
            
            html += `<div class="grid-time-cell ${isHalfHour ? 'half-hour' : ''}" style="grid-column: 1; grid-row: ${row}; font-size: 0.75rem; padding: 2px; ${textStyle}">${timeLabel}</div>`;
            
            consultorios.forEach((cons, index) => {
                const col = index + 2;
                html += `
                    <div class="grid-slot ${isHalfHour ? 'half-hour' : ''}" 
                         data-cons="${cons}" 
                         data-time="${timeLabel}" 
                         data-date="${dateStr}"
                         style="grid-column: ${col}; grid-row: ${row};">
                        <button class="slot-action-btn" onclick="window.iniciarReservaV2(${cons}, '${dateStr}', '${timeLabel}')">+</button>
                    </div>
                `;
            });
            row++;
        }
    }

    html += `</div>`;
    gridContainer.innerHTML = html;

    // Place bookings
    placeBookings(gridContainer, reservas, consultorios, startHour, endHour);

    // Time Indicator — Línea de tiempo actual SOLO si es el día de hoy
    const gridEl = gridContainer.querySelector('.agenda-v2-grid');
    if (gridEl) {
        // Comparar dateStr (agenda) con hoy
        const hoy = new Date();
        const hoyStr = `${hoy.getFullYear()}-${String(hoy.getMonth() + 1).padStart(2, '0')}-${String(hoy.getDate()).padStart(2, '0')}`;
        if (dateStr === hoyStr) {
            window.initTimeIndicator(gridEl, 50); // 50px por slot de 30min
        }
    }

    // Eventos para Pestañas Móviles
    const mobileTabs = gridContainer.querySelectorAll('.mobile-tab-btn');
    mobileTabs.forEach(btn => {
        btn.addEventListener('click', (e) => {
            mobileTabs.forEach(b => b.classList.remove('active'));
            e.target.classList.add('active');
            const target = e.target.getAttribute('data-target-cons');
            gridEl.setAttribute('data-active-mobile-cons', target);
        });
    });
}

function placeBookings(gridContainer, reservas, consultorios, startHour, endHour) {
    const grid = gridContainer.querySelector('.agenda-v2-grid');
    if (!grid) return;

    reservas.forEach(res => {
        if (res.summary && res.summary.toLowerCase().includes('cancelada')) return;
        
        const prefix = APP_CONFIG.espacios?.prefijoGeneral || 'C';

        // Parse consultorio from summary (e.g., "C3: Reserva..." o "C5: Bloqueada-Limpieza")
        let consMatch = res.summary ? res.summary.match(new RegExp(`${prefix}(\\d+):`, 'i')) : null;
        let consNum = consMatch ? parseInt(consMatch[1]) : null;

        // fallback: a veces el texto viene de otro origen, e.g. "Consultorio: 4"
        if (!consNum && res.description) {
            const descMatch = res.description.match(/consultorio\s*[:\-]?\s*(\\d+)/i);
            if (descMatch) consNum = parseInt(descMatch[1]);
        }
        if (!consNum && res.location) {
            const locMatch = res.location.match(/(\\d+)/);
            if (locMatch) consNum = parseInt(locMatch[1]);
        }

        // Handle global blocks (C0)
        const isGlobalBlock = res.summary && res.summary.toUpperCase().startsWith(`${prefix.toUpperCase()}0: BLOQUEADA`);

        // Parse start time
        // We will use the exact same logic as the original agenda.js to ensure consistency
        // En agenda.js se usa new Date(res.start) y new Date(res.end)
        // Pero la API devuelve res.start.dateTime o res.start.date
        const startDateStr = res.start;
        const endDateStr = res.end;
        
        const startDate = new Date(startDateStr);
        const endDate = new Date(endDateStr);
        
        const isFullDay = !startDateStr || startDateStr.length <= 10; // Si es solo fecha (YYYY-MM-DD)
        
        let startH = startDate.getHours();
        let startM = startDate.getMinutes();
        let endH = endDate.getHours();
        let endM = endDate.getMinutes();

        if (isFullDay) {
            startH = startHour;
            startM = 0;
            endH = endHour;
            endM = 0;
        }

        // Adjust if event starts before visible hours but ends within or after
        if (startH < startHour) {
            if (endH < startHour || (endH === startHour && endM === 0)) return; // Completely before visible hours
            startH = startHour;
            startM = 0;
        }

        // Adjust if event ends after visible hours
        if (endH > endHour || (endH === endHour && endM > 0)) {
            if (startH >= endHour) return; // Completely after visible hours
            endH = endHour;
            endM = 0;
        }

        // Calculate duration in hours (e.g., 1.5 for 90 mins)
        const durationHours = (endH + endM/60) - (startH + startM/60);

        // Only process if within visible hours (should be handled by above checks, but just in case)
        if (startH >= endHour || endH <= startHour || durationHours <= 0) return;

        // Determine booking type and visibility
        let typeClass = 'ajena';
        let title = 'Ocupado';
        let isOwn = false;

        if (isGlobalBlock) {
            typeClass = 'bloqueo';
            title = 'BLOQUEADO';
        }

        // Check if it's own booking
        const userEmail = user ? user.email.toLowerCase() : '';
        const desc = (res.description || '').toLowerCase();
        
        if (!isGlobalBlock && (esAdmin || desc.includes(userEmail))) {
            isOwn = true;
            // Extract name from summary (e.g. "C5: Juan Perez" -> "Juan Perez")
            let nameMatch = res.summary ? res.summary.match(new RegExp(`^${prefix}\\d+:\\s*(.+)$`, 'i')) : null;
            title = nameMatch ? nameMatch[1].trim() : 'Reserva';
            
            if (res.summary.toLowerCase().includes('fija')) {
                typeClass = 'fija';
            } else {
                typeClass = 'eventual';
            }
        }

        if (isGlobalBlock) {
            typeClass = 'bloqueo';
            title = 'BLOQUEADO';
            isOwn = esAdmin; // Only admins see details of blocks
        }

        // Function to place block in a specific column
        const placeBlock = (cNum) => {
            const colIndex = consultorios.indexOf(cNum);
            if (colIndex === -1) return; // Consultorio not visible

            const col = colIndex + 2;
            
            // Calculate exact row position (each hour has 2 rows now)
            const startRowOffset = (startH - startHour) * 2 + (startM >= 30 ? 1 : 0);
            const rowStart = startRowOffset + 2;
            
            // Calculate row span (each row is 30 mins = 0.5 hours)
            // durationHours = 1.5 -> spans 3 rows
            // We use Math.round to avoid floating point precision issues (e.g. 1.5 * 2 = 3.0000000000000004)
            const rowSpan = Math.max(1, Math.round(durationHours * 2));
            
            // Find the slots to mark them as having a booking
            for (let i = 0; i < rowSpan; i++) {
                const slotH = startHour + Math.floor((startRowOffset + i) / 2);
                const slotM = ((startRowOffset + i) % 2) === 0 ? 0 : 30;
                if (slotH >= endHour) break;
                
                const timeLabel = `${slotH.toString().padStart(2, '0')}:${slotM.toString().padStart(2, '0')}`;
                const slot = grid.querySelector(`.grid-slot[data-cons="${cNum}"][data-time="${timeLabel}"]`);
                if (slot) {
                    slot.classList.add('has-booking');
                    // Remove the agendar button
                    const btn = slot.querySelector('.slot-action-btn');
                    if (btn) btn.remove();
                }
            }

            // Create the block element
            const block = document.createElement('div');
            block.className = `booking-block ${typeClass}`;
            block.setAttribute('data-cons', cNum); // Agregamos el identificador para la vista móvil
            block.style.gridColumn = col;
            block.style.gridRow = `${rowStart} / span ${rowSpan}`;
            block.style.position = 'relative';
            block.style.marginTop = '1px';
            block.style.height = 'calc(100% - 2px)';
            block.style.width = 'calc(100% - 4px)';
            block.style.marginLeft = '2px';
            block.style.zIndex = '10';

            // Si la flag está activada, aplicamos el color correspondiente al espacio actual
            if (APP_CONFIG.espacios?.usarColoresEnAgenda !== false) {
                let colorIdGoogle = null;
                if (APP_CONFIG.espacios && APP_CONFIG.espacios.items) {
                    const item = APP_CONFIG.espacios.items.find(i => parseInt(i.id) === cNum);
                    if (item) colorIdGoogle = item.colorGoogle;
                } else if (APP_CONFIG.coloresConsultorios) {
                    colorIdGoogle = APP_CONFIG.coloresConsultorios[cNum];
                }
                if (colorIdGoogle && APP_CONFIG.paletaColoresGoogle[colorIdGoogle]) {
                    const esModoAstigmatismo = document.body.classList.contains('theme-astigmatismo');
                    if (esModoAstigmatismo) {
                        block.style.backgroundColor = APP_CONFIG.paletaColoresGoogle[colorIdGoogle];
                        block.style.backgroundColor = `color-mix(in srgb, ${APP_CONFIG.paletaColoresGoogle[colorIdGoogle]} 20%, #2A2A35 80%)`;
                        block.style.color = '#D9D9E3';
                        block.style.border = `1px solid ${APP_CONFIG.paletaColoresGoogle[colorIdGoogle]}`;
                        block.style.borderLeft = `4px solid ${APP_CONFIG.paletaColoresGoogle[colorIdGoogle]}`;
                    } else {
                        block.style.backgroundColor = APP_CONFIG.paletaColoresGoogle[colorIdGoogle];
                        block.style.color = '#333';
                        block.style.border = '1px solid rgba(0,0,0,0.1)';
                    }
                }
            }

            block.style.boxSizing = 'border-box';
            block.style.minHeight = '20px';

            // Visual pastel si la reserva ya inició
            const ahora = new Date();
            const inicioReserva = new Date(res.start);
            if (ahora > inicioReserva) {
                block.classList.add('past');
                // Guardar el color original en CSS variable para el mix
                block.style.setProperty('--booking-bg', block.style.backgroundColor);
            }

            const timeDisplay = isFullDay ? 'Todo el día' : `${startH.toString().padStart(2, '0')}:${startM.toString().padStart(2, '0')} - ${endH.toString().padStart(2, '0')}:${endM.toString().padStart(2, '0')}`;
            block.innerHTML = `
                <div class="booking-title">${isOwn ? title : 'Ocupado'}</div>
                <div class="booking-time">${timeDisplay}</div>
            `;
            if (isOwn) {
                block.title = res.description || res.summary;
                block.onclick = () => window.mostrarDetalleReserva(res.id, res.summary, res.description || '', res.created || null, () => loadDataAndRenderGrid(gridContainer.closest('.agenda-v2-container')?.parentElement || document.querySelector('.agenda-v2-container').parentElement));
            }
            grid.appendChild(block);
        };

        if (isGlobalBlock) {
            // Place in all visible consultorios
            consultorios.forEach(c => placeBlock(c));
        } else if (consNum !== null) {
            placeBlock(consNum);
        }
    });
}
```

### Archivo: `js/bloqueos.js`
```javascript
// bloqueos.js
// Lógica para bloqueo de horarios por admin/gestor generando reservas "Bloqueada" en los consultorios seleccionados

import { APP_CONFIG } from './config.js';
import { getAuthHeaders, mostrarToast, mostrarModalConfirmacion } from './utils.js';

/**
 * Abre un modal/formulario para seleccionar día, hora inicio/fin, consultorios y bloquea los horarios
 * Solo para admin/gestor
 */
export function renderBloqueoHorarios(container, usuario) {
        container.innerHTML = `
            <div class="bloqueos-header" style="margin-bottom: 20px;">
                <h2 class="module-title">Bloqueo de Horarios</h2>
            </div>
            <div class="informe-container" style="max-width: 900px; margin: 0 auto; padding: 20px; background: var(--white); border-radius: var(--radius-card, 8px); box-shadow: 0 2px 10px rgba(0,0,0,0.05);">
                <form id="form-bloqueo-horarios" class="informe-form" style="display: flex; flex-direction: column; gap: 20px; align-items: stretch; width: 100%; box-sizing: border-box;">
                    
                    <div style="display: flex; flex-wrap: wrap; gap: 20px; align-items: flex-start; width: 100%;">
                        <div style="display: flex; gap: 15px; flex: 1; min-width: 320px; align-items: flex-end;">
                            <div class="form-group" style="display: flex; flex-direction: column; flex: 1;">
                                <label style="font-weight: 600; color: var(--text-muted); font-size: 0.85rem; text-transform: uppercase; margin-bottom: 8px;">DÍA</label>
                                <input type="date" id="bloqueo-dia" required style="padding: 10px; border: 1px solid var(--border); border-radius: var(--radius-btn, 6px); width: 100%; box-sizing: border-box; font-family: inherit; height: 42px; background: var(--white); color: var(--text-main); font-size: 0.95rem; outline: none; transition: border-color 0.2s;" onfocus="this.style.borderColor='var(--primary)'" onblur="this.style.borderColor='var(--border)'">
                            </div>

                            <div class="form-group" style="display: flex; align-items: center; background: var(--bg-zebra); padding: 0 15px; border-radius: var(--radius-btn, 6px); border: 1px solid var(--border); box-sizing: border-box; height: 42px; white-space: nowrap;">
                                <label style="display: flex; align-items: center; gap: 10px; cursor: pointer; font-weight: 500; color: var(--text-main); font-size: 0.95rem; margin: 0;">
                                    <input type="checkbox" id="bloqueo-dia-completo" style="width: 18px; height: 18px; min-width: 0 !important; cursor: pointer; accent-color: var(--primary);">
                                    Día completo
                                </label>
                            </div>
                        </div>

                        <div style="display: flex; gap: 15px; flex: 1; min-width: 300px;">
                            <div class="form-group" style="display: flex; flex-direction: column; flex: 1;">
                                <label style="font-weight: 600; color: var(--text-muted); font-size: 0.85rem; text-transform: uppercase; margin-bottom: 8px;">HORA INICIO</label>
                                <input type="time" id="bloqueo-hora-inicio" value="08:00" required style="padding: 10px; border: 1px solid var(--border); border-radius: var(--radius-btn, 6px); width: 100%; box-sizing: border-box; font-family: inherit; background: var(--white); height: 42px; color: var(--text-main); font-size: 0.95rem; outline: none; transition: border-color 0.2s;" onfocus="this.style.borderColor='var(--primary)'" onblur="this.style.borderColor='var(--border)'">
                            </div>
                            
                            <div class="form-group" style="display: flex; flex-direction: column; flex: 1;">
                                <label style="font-weight: 600; color: var(--text-muted); font-size: 0.85rem; text-transform: uppercase; margin-bottom: 8px;">HORA FIN</label>
                                <input type="time" id="bloqueo-hora-fin" value="21:00" required style="padding: 10px; border: 1px solid var(--border); border-radius: var(--radius-btn, 6px); width: 100%; box-sizing: border-box; font-family: inherit; background: var(--white); height: 42px; color: var(--text-main); font-size: 0.95rem; outline: none; transition: border-color 0.2s;" onfocus="this.style.borderColor='var(--primary)'" onblur="this.style.borderColor='var(--border)'">
                            </div>
                        </div>
                    </div>

                    <div class="form-group" style="display: flex; flex-direction: column; width: 100%;">
                        <label style="font-weight: 600; color: var(--text-muted); font-size: 0.85rem; text-transform: uppercase; margin-bottom: 12px;">${APP_CONFIG.espacios?.etiquetaGeneralPlural ? APP_CONFIG.espacios.etiquetaGeneralPlural.toUpperCase() : 'ESPACIOS'}</label>
                        <div id="bloqueo-consultorios-list" style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; background: var(--white); padding: 25px; border: 1px solid var(--border); border-radius: var(--radius-btn, 6px);"></div>
                    </div>

                    <div class="form-group" style="display: flex; flex-direction: column; width: 100%;">
                        <label style="font-weight: 600; color: var(--text-muted); font-size: 0.85rem; text-transform: uppercase; margin-bottom: 8px;">OBSERVACIÓN (OPCIONAL)</label>
                        <input type="text" id="bloqueo-observacion" maxlength="100" placeholder="Ej: Mantenimiento, Feriado..." style="padding: 10px; border: 1px solid var(--border); border-radius: var(--radius-btn, 6px); width: 100%; box-sizing: border-box; font-family: inherit; background: var(--white); height: 42px; color: var(--text-main); font-size: 0.95rem; outline: none; transition: border-color 0.2s;" onfocus="this.style.borderColor='var(--primary)'" onblur="this.style.borderColor='var(--border)'">
                    </div>

                    <div style="display: flex; justify-content: flex-end; margin-top: 10px; width: 100%;">
                        <button type="submit" class="btn btn-primary" style="padding: 10px 24px; height: 42px; font-size: 0.95rem; font-weight: bold; box-shadow: 0 2px 4px rgba(0,0,0,0.1); cursor: pointer;">Bloquear Horarios</button>
                    </div>
                </form>
            </div>
        `;
        // Consultorios/Espacios checkboxes
        let consultorios = [];
        if (APP_CONFIG.espacios && APP_CONFIG.espacios.items) {
            consultorios = APP_CONFIG.espacios.items.map(item => parseInt(item.id));
        } else {
            consultorios = APP_CONFIG.consultorios || [1,2,3,4,5];
        }

        const list = container.querySelector('#bloqueo-consultorios-list');
        list.innerHTML = consultorios.map(c => {
            const prefix = APP_CONFIG.espacios?.prefijoGeneral || 'C';
            let labelText = c === 0 ? `${prefix}0 (Todos)` : `${prefix}${c}`;
            
            // Usar nombre corto si está disponible
            if (APP_CONFIG.espacios && APP_CONFIG.espacios.items) {
                const item = APP_CONFIG.espacios.items.find(i => parseInt(i.id) === c);
                if (item) labelText = item.nombreCorto;
            }

            return `<label style="display: flex; align-items: center; gap: 15px; cursor: pointer; font-weight: 500; color: var(--text-main); justify-content: flex-start; font-size: 0.95rem;"><input type='checkbox' value='${c}' class='chk-consultorio' checked style="width: 18px; height: 18px; min-width: 0 !important; cursor: pointer; accent-color: var(--primary);"> ${labelText}</label>`;
        }).join('');
        
        // Lógica de "Seleccionar todos" con C0
        const chkConsultorios = Array.from(list.querySelectorAll('.chk-consultorio'));
        const chkC0 = chkConsultorios.find(cb => cb.value == '0');
        const chkResto = chkConsultorios.filter(cb => cb.value != '0');

        if (chkC0) {
            // Si se marca/desmarca C0, se marcan/desmarcan todos los demás
            chkC0.addEventListener('change', (e) => {
                chkResto.forEach(cb => cb.checked = e.target.checked);
            });

            // Si se desmarca cualquier otro, se desmarca C0. Si se marcan todos, se marca C0.
            chkResto.forEach(cb => {
                cb.addEventListener('change', () => {
                    if (!cb.checked) {
                        chkC0.checked = false;
                    } else {
                        const todosMarcados = chkResto.every(c => c.checked);
                        chkC0.checked = todosMarcados;
                    }
                });
            });
        }

        // Día completo: deshabilita horas y fija valores
        const diaCompletoChk = container.querySelector('#bloqueo-dia-completo');
        const horaInicio = container.querySelector('#bloqueo-hora-inicio');
        const horaFin = container.querySelector('#bloqueo-hora-fin');
        diaCompletoChk.onchange = () => {
            horaInicio.disabled = diaCompletoChk.checked;
            horaFin.disabled = diaCompletoChk.checked;
            if (diaCompletoChk.checked) {
                horaInicio.value = '08:00';
                horaFin.value = '21:00';
            }
        };
        // Submit
        container.querySelector('#form-bloqueo-horarios').onsubmit = async (e) => {
            e.preventDefault();
            //const fecha = new Date(container.querySelector('#bloqueo-dia').value);
            const [fy, fm, fd] = container.querySelector('#bloqueo-dia').value.split('-').map(Number);
            const fecha = new Date(fy, fm - 1, fd);
            
            let horaIni = 8, horaFinVal = 21;
            if (!diaCompletoChk.checked) {
                const [h1,m1] = container.querySelector('#bloqueo-hora-inicio').value.split(':').map(Number);
                const [h2,m2] = container.querySelector('#bloqueo-hora-fin').value.split(':').map(Number);
                horaIni = h1;
                horaFinVal = h2;
            }
            const consultoriosSel = Array.from(list.querySelectorAll('input[type=checkbox]:checked')).map(cb => cb.value);
            if (!consultoriosSel.length) { 
                mostrarToast('Seleccione al menos un consultorio', 'warning');
                return; 
            }
            const observacion = container.querySelector('#bloqueo-observacion').value || '';
            const fechaString = container.querySelector('#bloqueo-dia').value;
            const [anio, mes, dia] = fechaString.split('-');
            const fechaFormateada = `${dia}/${mes}/${anio}`;

            const contenido = `
                <p style="margin-bottom: 20px; color: #666;">
                    ¿Está seguro que desea bloquear ${diaCompletoChk.checked ? 'todo el día' : `de ${horaIni.toString().padStart(2, '0')}:00 a ${horaFinVal.toString().padStart(2, '0')}:00`} 
                    el <strong>${fechaFormateada}</strong> 
                    en <strong>${consultoriosSel.length === 1 && consultoriosSel[0] === '0' ? 'todos los espacios' : consultoriosSel.map(c => c=='0'?'Todos':(APP_CONFIG.espacios?.prefijoGeneral||'C')+c).join(', ')}</strong>?
                </p>`;

            mostrarModalConfirmacion(
                "Confirmar Bloqueo",
                contenido,
                "Bloquear",
                async (btnConfirmar, btnCancelar, modal) => {
                    btnConfirmar.disabled = true;
                    btnConfirmar.innerText = 'Bloqueando...';

                    await bloquearHorarios({ fecha, horaInicio: horaIni, horaFin: horaFinVal, consultorios: consultoriosSel, diaCompleto: diaCompletoChk.checked, usuario, observacion });
                    
                    modal.remove();

                    // Usar el sistema de notificaciones toast
                    mostrarToast('Bloqueo realizado con éxito.', 'success');
                    
                    // Limpiar formulario
                    container.querySelector('#form-bloqueo-horarios').reset();
                }
            );
        };
}

/**
 * Genera reservas "Bloqueada" para los consultorios y horarios seleccionados
 * @param {Date} fecha - Día a bloquear
 * @param {number} horaInicio - Hora de inicio
 * @param {number} horaFin - Hora de fin
 * @param {Array<string>} consultorios - IDs de consultorios a bloquear
 * @param {boolean} diaCompleto - Si se bloquea todo el día
 * @param {string} usuario - Email del admin/gestor
 */
/**
 * Automatiza la creación de reservas bloqueadas
 * @param {Object} params
 * @param {Date} params.fecha
 * @param {number} params.horaInicio
 * @param {number} params.horaFin
 * @param {Array<string>} params.consultorios
 * @param {boolean} params.diaCompleto
 * @param {Object} params.usuario - Objeto usuario {nombre, email, rol}
 */
export async function bloquearHorarios({ fecha, horaInicio, horaFin, consultorios, diaCompleto, usuario, observacion }) {
    // Color rojo solo si se bloquean todos los consultorios, si no, color por consultorio
    let todosConsultorios = [];
    if (APP_CONFIG.espacios && APP_CONFIG.espacios.items) {
        todosConsultorios = APP_CONFIG.espacios.items.map(item => item.id);
    } else {
        todosConsultorios = (APP_CONFIG.consultorios || [1,2,3,4,5]).map(String);
    }
    
    const bloquearTodos = consultorios.length === todosConsultorios.length && consultorios.every(c => todosConsultorios.includes(c.toString()));
    const nombreUsuario = usuario && usuario.nombre ? usuario.nombre : 'Admin';
    const prefix = APP_CONFIG.espacios?.prefijoGeneral || 'C';

    if (bloquearTodos) {
        // Generar una única reserva C0 (no crear para c1-c5)
        const colorId = '11';
        const tieneObs = observacion && observacion.trim().length > 0;
        const body = {
            email: usuario.email,
            nombre: 'Bloqueada',
            consultorio: '0',
            fecha: fecha.toISOString().slice(0,10),
            hora: horaInicio,
            minutos: 0,
            intervalo: (horaFin - horaInicio) * 60,
            colorId,
            creadoPor: nombreUsuario,
            observacion: observacion || '',
            summary: `${prefix}0: Bloqueada${tieneObs ? '-' + observacion.trim() : ''}`,
            prefijoGcal: prefix,
            etiquetaGeneral: APP_CONFIG.espacios?.etiquetaGeneral || 'Consultorio'
        };
        const resp = await fetch('/.netlify/functions/reservar', {
            method: 'POST',
            headers: getAuthHeaders(),
            body: JSON.stringify(body)
        });
        if (!resp.ok) {
            let msg = 'Error desconocido';
            try {
                const data = await resp.json();
                msg = data.error || JSON.stringify(data);
            } catch {}
            
            mostrarToast(`Error al bloquear C0 ${horaInicio}:00 - ${msg}`, 'error');
        }
        // Refuerzo: limpiar consultorios para evitar bucle accidental
        consultorios.length = 0;
        return; // No crear reservas para c1-c5
    }
    // Si no es bloqueo total, crear reservas individuales
    for (const consultorio of consultorios) {
        let colorId = '1';
        if (APP_CONFIG.espacios && APP_CONFIG.espacios.items) {
            const item = APP_CONFIG.espacios.items.find(i => parseInt(i.id) === parseInt(consultorio));
            if (item) colorId = item.colorGoogle;
        } else if (APP_CONFIG.coloresConsultorios) {
            colorId = APP_CONFIG.coloresConsultorios[consultorio] || '1';
        }
        
        const tieneObs = observacion && observacion.trim().length > 0;
        const body = {
            email: usuario.email,
            nombre: 'Bloqueada',
            consultorio,
            fecha: fecha.toISOString().slice(0,10),
            hora: horaInicio,
            minutos: 0,
            intervalo: (horaFin - horaInicio) * 60,
            colorId,
            creadoPor: nombreUsuario,
            observacion: observacion || '',
            summary: `${prefix}${consultorio}: Bloqueada${tieneObs ? '-' + observacion.trim() : ''}`,
            prefijoGcal: prefix,
            etiquetaGeneral: APP_CONFIG.espacios?.etiquetaGeneral || 'Consultorio'
        };
        // Llamar a la API de reservas (Netlify Function)
        const resp = await fetch('/.netlify/functions/reservar', {
            method: 'POST',
            headers: getAuthHeaders(),
            body: JSON.stringify(body)
        });
        if (!resp.ok) {
            let msg = 'Error desconocido';
            try {
                const data = await resp.json();
                msg = data.error || JSON.stringify(data);
            } catch {}
            
            mostrarToast(`Error al bloquear C${consultorio} ${horaInicio}:00 - ${msg}`, 'error');
        }
    }
}

```

### Archivo: `js/components/timeindicator.js`
```javascript
// TimeIndicator.js — Línea de tiempo actual para agenda
// Uso: importar y llamar initTimeIndicator(agendaContainer, slotHeight)

function initTimeIndicator(agendaContainer, slotHeight = 50) {
  const startHour = 8;
  const endHour = 21;
  const intervalMinutes = 30;
  const indicatorId = 'time-indicator';

  // Crear el elemento si no existe
  let indicator = document.getElementById(indicatorId);
  if (!indicator) {
    indicator = document.createElement('div');
    indicator.id = indicatorId;
    indicator.style.position = 'absolute';
    indicator.style.left = '0';
    indicator.style.width = '100%';
    indicator.style.height = '1.5px';
    indicator.style.background = 'red';
    indicator.style.zIndex = '10';
    indicator.style.pointerEvents = 'none';
    // Círculo/etiqueta
    const label = document.createElement('div');
    label.id = 'time-indicator-label';
    label.style.position = 'absolute';
    label.style.left = '0';
    label.style.top = '-8px';
    label.style.width = '48px';
    label.style.height = '18px';
    label.style.background = 'white';
    label.style.borderRadius = '9px';
    label.style.border = '1px solid red';
    label.style.color = 'red';
    label.style.fontSize = '12px';
    label.style.display = 'flex';
    label.style.alignItems = 'center';
    label.style.justifyContent = 'center';
    label.style.boxShadow = '0 1px 4px rgba(0,0,0,0.08)';
    indicator.appendChild(label);
    agendaContainer.appendChild(indicator);
  }

  function updateIndicator() {
      // Hora real de Montevideo
      const now = new Date();
      const dtf = new Intl.DateTimeFormat('en-GB', {
        timeZone: 'America/Montevideo',
        hour: '2-digit',
        minute: '2-digit',
        hour12: false
      });
      const parts = dtf.formatToParts(now);
      const hour = parseInt(parts.find(p => p.type === 'hour').value, 10);
      const minutes = parseInt(parts.find(p => p.type === 'minute').value, 10);
      // Validación: fuera de rango
      if (hour < startHour || hour >= endHour) {
        indicator.style.display = 'none';
        return;
      }
      indicator.style.display = 'block';
      // Buscar slot base
      const baseLabel = `${hour.toString().padStart(2, '0')}:00`;
      const slotElem = agendaContainer.querySelector(`.grid-slot[data-time="${baseLabel}"]`);
      if (!slotElem) {
        indicator.style.display = 'none';
        return;
      }
      // Altura de una hora (dos slots de 30min)
      const alturaHora = slotElem.offsetHeight * 2;
      // Cálculo de precisión
      const top = slotElem.offsetTop + (minutes * (alturaHora / 60));
      indicator.style.top = `${top}px`;
      // Actualizar etiqueta
      const label = indicator.querySelector('#time-indicator-label');
      label.textContent = `Ahora ${hour.toString().padStart(2,'0')}:${minutes.toString().padStart(2,'0')}`;
    }

  updateIndicator();
  setInterval(updateIndicator, 60000);
}

// Exportar para uso
window.initTimeIndicator = initTimeIndicator;

```

### Archivo: `js/config.js`
```javascript
// js/config.js

import {
    APP_VERSION,
    SHOW_UPDATE_MODAL,
    EMPRESA_NOMBRE,
    EMPRESA_TELEFONO,
    EMPRESA_EMAIL,
    EMPRESA_LOGO,
    ESPACIOS_CANTIDAD,
    ESPACIOS_ETIQUETA,
    ESPACIOS_PREFIJO,
    USUARIO_DOMINIO_EMAIL,
    ESTILO_FONDO_CSS,
    REGLA_DIAS_LABORALES,
    REGLA_HORA_INICIO,
    REGLA_HORA_FIN,
    REGLA_INTERVALOS_MINUTOS,
    REGLA_HORAS_CANCELACION,
    REGLA_FIN_DE_SEMANA,
    REGLA_MAX_SEMANAS_FUTURO
} from './config_particular.js';

export const TIPOS_CUPONERA = ['10', '20'];

// ==============================================================================
// 1. CONFIGURACIÓN DEL CLIENTE (MARCA BLANCA)
// ==============================================================================
// Estas son las únicas variables que debes modificar al clonar el sistema para un cliente nuevo.

// ATENCIÓN: Al cambiar esta versión, hazlo también en sw.js (CACHE_NAME) para forzar la actualización en los móviles de los clientes.
export { APP_VERSION, SHOW_UPDATE_MODAL };

// A. Identidad de la Empresa
export { EMPRESA_NOMBRE, EMPRESA_TELEFONO, EMPRESA_EMAIL, EMPRESA_LOGO };

// B. Entidad a Alquilar/Reservar (Ej: "Consultorio - C", "Box - B", "Mesa - M")
export const ESPACIOS_ETIQUETA_PLURAL = ESPACIOS_ETIQUETA + "s"; // Para textos como "No hay Consultorios disponibles"

// C. Configuración de Usuarios por defecto
export { USUARIO_DOMINIO_EMAIL };

// D. Reglas de Negocio y Horarios
// Estos valores están definidos en config_particular.js para facilitar su ajuste cliente a cliente.

// E. Estética
// ESTILO_FONDO_CSS se importa desde config_particular.js

// ==============================================================================
// 2. NÚCLEO DEL SISTEMA (NO MODIFICAR - SE AUTOGENERA)
// ==============================================================================
const coloresGoogle = ["6", "5", "3", "7", "10", "2", "11", "9", "4", "1"];
const itemsGenerados = [
    { id: "0", nombreCorto: "Todos", prefijoGcal: `${ESPACIOS_PREFIJO}0`, colorGoogle: "11" }
];
for (let i = 1; i <= ESPACIOS_CANTIDAD; i++) {
    itemsGenerados.push({
        id: String(i),
        nombreCorto: `${ESPACIOS_ETIQUETA} ${i}`,
        prefijoGcal: `${ESPACIOS_PREFIJO}${i}`,
        colorGoogle: coloresGoogle[(i - 1) % coloresGoogle.length]
    });
}

export const APP_CONFIG = {
    // === IDENTIDAD DE EMPRESA (MARCA BLANCA) ===
    EMPRESA_NOMBRE: EMPRESA_NOMBRE,
    EMPRESA_TELEFONO: EMPRESA_TELEFONO,
    EMPRESA_EMAIL: EMPRESA_EMAIL,
    EMPRESA_LOGO: EMPRESA_LOGO,

    // === INFORMACIÓN GENERAL ===
    nombreProyecto: EMPRESA_NOMBRE,
    zonaHoraria: 'America/Montevideo',
    telefonoConsultorio: EMPRESA_TELEFONO,
    roles: ["admin", "gestor", "usuario"],
    tiposDocumento: ["PROF","SIS"],

    // === CONFIGURACIÓN DE UI ===
    estilos: { fondo: ESTILO_FONDO_CSS },
    pageSize: 10,
    toastDuracion: 6000, 
    comboAgendaSoloUsuarios: true,
    
    // === GESTIÓN DE ESPACIOS ===
    espacios: {
        etiquetaGeneral: ESPACIOS_ETIQUETA,
        etiquetaGeneralPlural: ESPACIOS_ETIQUETA_PLURAL,
        prefijoGeneral: ESPACIOS_PREFIJO,
        usarColoresEnAgenda: true,
        items: itemsGenerados 
    },
    
    paletaColoresGoogle: {
        "1": "#a4bdfc", "2": "#7ae7bf", "3": "#dbadff", "4": "#ff887c", 
        "5": "#fbd75b", "6": "#ffb878", "7": "#46d6db", "8": "#e1e1e1", 
        "9": "#5484ed", "10": "#51b749", "11": "#dc2127"
    },
    usuarioEmailDominio: USUARIO_DOMINIO_EMAIL,

    // === CALENDARIO Y HORARIOS ===
    diasLaborales: REGLA_DIAS_LABORALES,
    horarios: {
        inicio: REGLA_HORA_INICIO,
        fin: REGLA_HORA_FIN,
        intervalos: REGLA_INTERVALOS_MINUTOS,
        restriccionCierreSoloUsuarios: true 
    },
    horariosEspeciales: {
        6: { inicio: 8, fin: 16 } // Configuración específica de Sábados
    },

    // === RESERVAS Y CANCELACIONES ===
    maxSemanasReservaUsuario: REGLA_MAX_SEMANAS_FUTURO,
    // estadosReserva: { RESERVADA: 'Reservada',PENDIENTE: 'Pendiente' ,USADA: 'Usada', CANCELADA: 'Cancelada' },
    // Modelo de estados simplificado: solo estados mutables de una reserva
    estadosReserva: { PENDIENTE: 'Pendiente', CONSUMIDA: 'Consumida', CANCELADA: 'Cancelada' },
    cancelacion: {
        horasAntelacion: REGLA_HORAS_CANCELACION,
        reglaFinDeSemana: REGLA_FIN_DE_SEMANA 
    }
};
```

### Archivo: `js/config_particular.js`
```javascript
// js/config_particular.js

// Valores particulares del cliente / despliegue.
// Este archivo solo debe contener variables específicas de marca, empresa y despliegue.

export const APP_VERSION = '4.9.17';
export const SHOW_UPDATE_MODAL = true;

export const EMPRESA_NOMBRE = 'De Maria Consultorios';
export const EMPRESA_TELEFONO = '091001334';
export const EMPRESA_EMAIL = 'demariaconsultorios1334@gmail.com';
export const EMPRESA_LOGO = 'imagenes/logo_demaria.png';

export const ESPACIOS_CANTIDAD = 5;
export const ESPACIOS_ETIQUETA = 'Consultorio';
export const ESPACIOS_PREFIJO = 'C';

export const USUARIO_DOMINIO_EMAIL = '@espacia.com.uy';

export const REGLA_DIAS_LABORALES = [1, 2, 3, 4, 5, 6]; // 0=Dom, 1=Lun... 6=Sáb
export const REGLA_HORA_INICIO = 8; // 08:00
export const REGLA_HORA_FIN = 22;   // 22:00
export const REGLA_INTERVALOS_MINUTOS = [60, 90]; // Duraciones permitidas en el combo

export const REGLA_HORAS_CANCELACION = 24; // Cuántas horas antes puede cancelar un usuario normal
export const REGLA_FIN_DE_SEMANA = false;  // Si es true, el Lunes cuenta como 72hs de anticipación
export const REGLA_MAX_SEMANAS_FUTURO = 12; // Cuántas semanas hacia adelante puede agendar un usuario

export const ESTILO_FONDO_CSS = 'linear-gradient(135deg, #e8f5e9 0%, #c8e6c9 100%)';

```

### Archivo: `js/config_respaldo.js`
```javascript
// js/config.js

export const TIPOS_CUPONERA = ['10', '20'];

// ==============================================================================
// 1. CONFIGURACIÓN DEL CLIENTE (MARCA BLANCA)
// ==============================================================================
//por deployar alguna cosa
// Estas son las únicas variables que debes modificar al clonar el sistema para un cliente nuevo.

// ATENCIÓN: Al cambiar esta versión, hazlo también en sw.js (CACHE_NAME) para forzar la actualización en los móviles de los clientes.
export const APP_VERSION = '4.9.15';
// Controla si se muestra el modal de aviso de cambio de versión
export const SHOW_UPDATE_MODAL = true;

// A. Identidad de la Empresa
export const EMPRESA_NOMBRE = "De Maria Consultorios";
export const EMPRESA_TELEFONO = "091001334"; // Aparece en los PDFs y contactos
export const EMPRESA_EMAIL ="demariaconsultorios1334@gmail.com"; // Para contacto en PDFs y formularios
export const EMPRESA_LOGO = "imagenes/logo_demaria.png"; // Aparece en los PDFs y contactos

// B. Entidad a Alquilar/Reservar (Ej: "Consultorio - C", "Box - B", "Mesa - M")
const ESPACIOS_CANTIDAD = 5; 
export const ESPACIOS_ETIQUETA = "Consultorio"; 
export const ESPACIOS_ETIQUETA_PLURAL = ESPACIOS_ETIQUETA + "s"; // Para textos como "No hay Consultorios disponibles"
export const ESPACIOS_PREFIJO = "C";           

// C. Configuración de Usuarios por defecto
export const USUARIO_DOMINIO_EMAIL = "@espacia.com.uy";

// D. Reglas de Negocio y Horarios
const REGLA_DIAS_LABORALES = [1, 2, 3, 4, 5, 6]; // 0=Dom, 1=Lun... 6=Sáb
const REGLA_HORA_INICIO = 8; // 08:00
const REGLA_HORA_FIN = 22;   // 22:00
const REGLA_INTERVALOS_MINUTOS = [60, 90]; // Duraciones permitidas en el combo

// D. Restricciones de Cancelación
const REGLA_HORAS_CANCELACION = 24; // Cuántas horas antes puede cancelar un usuario normal
const REGLA_FIN_DE_SEMANA = false;  // Si es true, el Lunes cuenta como 72hs de anticipación
const REGLA_MAX_SEMANAS_FUTURO = 12; // Cuántas semanas hacia adelante puede agendar un usuario

// E. Estética
const ESTILO_FONDO_CSS = 'linear-gradient(135deg, #e8f5e9 0%, #c8e6c9 100%)'; // Degradado verde suave


// ==============================================================================
// 2. NÚCLEO DEL SISTEMA (NO MODIFICAR - SE AUTOGENERA)
// ==============================================================================
const coloresGoogle = ["6", "5", "3", "7", "10", "2", "11", "9", "4", "1"];
const itemsGenerados = [
    { id: "0", nombreCorto: "Todos", prefijoGcal: `${ESPACIOS_PREFIJO}0`, colorGoogle: "11" }
];
for (let i = 1; i <= ESPACIOS_CANTIDAD; i++) {
    itemsGenerados.push({
        id: String(i),
        nombreCorto: `${ESPACIOS_ETIQUETA} ${i}`,
        prefijoGcal: `${ESPACIOS_PREFIJO}${i}`,
        colorGoogle: coloresGoogle[(i - 1) % coloresGoogle.length]
    });
}

export const APP_CONFIG = {
    // === IDENTIDAD DE EMPRESA (MARCA BLANCA) ===
    EMPRESA_NOMBRE: EMPRESA_NOMBRE,
    EMPRESA_TELEFONO: EMPRESA_TELEFONO,
    EMPRESA_EMAIL: EMPRESA_EMAIL,
    EMPRESA_LOGO: EMPRESA_LOGO,

    // === INFORMACIÓN GENERAL ===
    nombreProyecto: EMPRESA_NOMBRE,
    zonaHoraria: 'America/Montevideo',
    telefonoConsultorio: EMPRESA_TELEFONO,
    roles: ["admin", "gestor", "usuario"],
    tiposDocumento: ["CI", "DNI", "Otros"],

    // === CONFIGURACIÓN DE UI ===
    estilos: { fondo: ESTILO_FONDO_CSS },
    pageSize: 10,
    toastDuracion: 6000, 
    comboAgendaSoloUsuarios: true,
    
    // === GESTIÓN DE ESPACIOS ===
    espacios: {
        etiquetaGeneral: ESPACIOS_ETIQUETA,
        etiquetaGeneralPlural: ESPACIOS_ETIQUETA_PLURAL,
        prefijoGeneral: ESPACIOS_PREFIJO,
        usarColoresEnAgenda: true,
        items: itemsGenerados 
    },
    
    paletaColoresGoogle: {
        "1": "#a4bdfc", "2": "#7ae7bf", "3": "#dbadff", "4": "#ff887c", 
        "5": "#fbd75b", "6": "#ffb878", "7": "#46d6db", "8": "#e1e1e1", 
        "9": "#5484ed", "10": "#51b749", "11": "#dc2127"
    },
    usuarioEmailDominio: USUARIO_DOMINIO_EMAIL,

    // === CALENDARIO Y HORARIOS ===
    diasLaborales: REGLA_DIAS_LABORALES,
    horarios: {
        inicio: REGLA_HORA_INICIO,
        fin: REGLA_HORA_FIN,
        intervalos: REGLA_INTERVALOS_MINUTOS,
        restriccionCierreSoloUsuarios: true 
    },
    horariosEspeciales: {
        6: { inicio: 8, fin: 15 } // Configuración específica de Sábados
    },

    // === RESERVAS Y CANCELACIONES ===
    maxSemanasReservaUsuario: REGLA_MAX_SEMANAS_FUTURO,
    // estadosReserva: { RESERVADA: 'Reservada',PENDIENTE: 'Pendiente' ,USADA: 'Usada', CANCELADA: 'Cancelada' },
    // Modelo de estados simplificado: solo estados mutables de una reserva
    estadosReserva: { PENDIENTE: 'Pendiente', CONSUMIDA: 'Consumida', CANCELADA: 'Cancelada' },
    cancelacion: {
        horasAntelacion: REGLA_HORAS_CANCELACION,
        reglaFinDeSemana: REGLA_FIN_DE_SEMANA 
    }
};
```

### Archivo: `js/dashboard.js`
```javascript
import { APP_CONFIG, APP_VERSION } from './config.js';
import { getAuthHeaders } from './utils.js';

// Establecer el título del proyecto basado en config.js
if (APP_CONFIG && APP_CONFIG.nombreProyecto) {
    document.title = APP_CONFIG.nombreProyecto;
    
    // Si hay elementos con la clase "logo-text", actualizarlos
    const logoElements = document.querySelectorAll('.logo-text');
    logoElements.forEach(el => {
        el.textContent = APP_CONFIG.nombreProyecto;
    });
}

// Inyectar versión en UI
document.addEventListener('DOMContentLoaded', () => {
    const versionDisplays = document.querySelectorAll('.app-version-display');
    versionDisplays.forEach(el => {
        el.textContent = APP_VERSION || "v-.-.-";
    });
});

// Función para aplicar el fondo según el tema
function aplicarFondo() {
    const currentTheme = localStorage.getItem('espacia_theme') || 'classic';
    if (currentTheme === 'classic' && APP_CONFIG.estilos && APP_CONFIG.estilos.fondo) {
        document.body.style.background = APP_CONFIG.estilos.fondo;
    } else {
        document.body.style.background = ''; // Deja que el CSS del tema actúe
    }
    document.body.style.minHeight = '100vh';
}

// Aplicar fondo inicial
aplicarFondo();

// Escuchar cambios en el tema desde el selector
document.addEventListener('themeChanged', aplicarFondo);


// ================= ZONA DE SECCIONES =================

// --- Utilidad para limpiar CSS de sección ---
function limpiarCssSeccion() {
    const ids = ['agenda-v2-css', 'reservas-css', 'informe-css', 'abmusu2-css'];
    ids.forEach(id => {
        const el = document.getElementById(id);
        if (el) el.remove();
    });
}

// --- Sección Agenda V2 (Grilla) ---
function cargarEstiloAgendaV2() {
    limpiarCssSeccion();
    if (!document.getElementById('agenda-v2-css')) {
        const link = document.createElement('link');
        link.rel = 'stylesheet';
        link.href = 'css/2_agenda_v2.css';
        link.id = 'agenda-v2-css';
        document.head.appendChild(link);
    }
}

async function mostrarAgendaV2() {
    cargarEstiloAgendaV2();
    const sec = document.getElementById('agenda-v2-section');
    const cont = document.getElementById('agenda-v2-container');
    if (sec && cont) {
        sec.style.display = 'block';
        const { renderAgendaV2 } = await import('./agenda_v2.js');
        renderAgendaV2(cont);
    }
}

// --- Sección Reservas Futuras ---
function cargarEstiloReservas() {
    limpiarCssSeccion();
    if (!document.getElementById('reservas-css')) {
        const link = document.createElement('link');
        link.rel = 'stylesheet';
        link.href = 'css/2_reservas.css';
        link.id = 'reservas-css';
        document.head.appendChild(link);
    }
}

async function mostrarReservas() {
    cargarEstiloReservas();
    const sec = document.getElementById('reservas-section');
    const cont = document.getElementById('reservas-container');
    if (sec && cont) {
       sec.style.display = 'block';
        // Solo descarga reservas_futuras.js cuando se necesita
        const { renderMisReservasFuturas } = await import('./reservas_futuras.js');
        renderMisReservasFuturas(cont);    
    }
}

// --- Sección Informe ---
function cargarEstiloInforme() {
    limpiarCssSeccion();
    if (!document.getElementById('informe-css')) {
        const link = document.createElement('link');
        link.rel = 'stylesheet';
        link.href = 'css/2_informe.css';
        link.id = 'informe-css';
        document.head.appendChild(link);
    }
}

async function mostrarInforme() {
    cargarEstiloInforme();
    const sec = document.getElementById('informe-section');
    const cont = document.getElementById('informe-container');
    if (sec && cont) {
		sec.style.display = 'block';
        const { renderInforme } = await import('./informe_modular.js');
        renderInforme(cont);
    }
}

// --- Sección Informe Canceladas ---
async function mostrarInformeCanceladas() {
    cargarEstiloInforme(); // Reutilizamos el estilo del informe normal
    const sec = document.getElementById('informe-canceladas-section');
    const cont = document.getElementById('informe-canceladas-container');
    if (sec && cont) {
		sec.style.display = 'block';
        const { renderInformeCanceladas } = await import('./informe_canceladas.js');
        renderInformeCanceladas(cont);
    }
}

// --- Sección ABM USU ---



function cargarEstiloAbmUsu2() {
    limpiarCssSeccion();
    if (!document.getElementById('abmusu2-css')) {
        const link = document.createElement('link');
        link.rel = 'stylesheet';
        link.href = 'css/2_abmusu.css';
        link.id = 'abmusu2-css';
        document.head.appendChild(link);
    }
}

async function mostrarAbmUsu2() {
    cargarEstiloAbmUsu2();
    const sec = document.getElementById('abmusu2-section');
    const cont = document.getElementById('abmusu2-container');
    if (sec && cont) {
        sec.style.display = 'block';
        const { renderAbmUsu2 } = await import('./persistencia/abm_usuario.js');
        renderAbmUsu2(cont);
    }
}

// --- Controlador de Secciones ---
function mostrarSeccion(seccion) {
    // 1. IDs de las secciones definidos en dashboard.html
    const seccionesIds = [
        'agenda-v2-section',
        'reservas-section', 
        'informe-section',
        'admin-section',
        'abmusu2-section',
        'bloqueo-section',
        'cuponera-section'
    ];

    // 2. Ocultar todas las secciones para limpiar la pantalla
    seccionesIds.forEach(id => {
        const el = document.getElementById(id);
        if (el) el.style.display = 'none';
    });

    // 3. Actualizar el título principal (quitar el "Cargando...")
    const welcome = document.getElementById('welcome-msg');
    if (welcome) {
        welcome.innerText = "Panel de Gestión";
    }

    // 4. Mostrar la sección correspondiente
    if (seccion === 'agenda-v2') mostrarAgendaV2();
    else if (seccion === 'mis-reservas-futuras') mostrarReservas();
    else if (seccion === 'informe') mostrarInforme();
    else if (seccion === 'abmusu2') mostrarAbmUsu2();
    else if (seccion === 'bloqueo') {
        const sec = document.getElementById('bloqueo-section');
        const cont = document.getElementById('bloqueo-container');
        if (sec && cont) {
            sec.style.display = 'block';
            const user = JSON.parse(localStorage.getItem('usuarioActual')) || JSON.parse(sessionStorage.getItem('usuarioActual'));
            import('./bloqueos.js').then(({ renderBloqueoHorarios }) => {
                renderBloqueoHorarios(cont, user);
            });
        }
    }
    else if (seccion === 'cuponera') {
        limpiarCssSeccion();
        const sec = document.getElementById('cuponera-section');
        const cont = document.getElementById('cuponera-container');
        if (sec && cont) {
            sec.style.display = 'block';
            import('./cuponeras.js').then(({ renderCuponera }) => {
                renderCuponera(cont);
            });
        }
    }
}

/**
 * Configuración inicial del Dashboard al cargar la página
 */
const initDashboard = async () => {
    // Verificar si el usuario está autenticado mediante localStorage/sessionStorage
    let user = null;
    try {
        const local = localStorage.getItem('usuarioActual');
        const session = sessionStorage.getItem('usuarioActual');

        user = local ? JSON.parse(local) : (session ? JSON.parse(session) : null);
    } catch (e) { }
    if (!user) {
        window.location.href = "index.html";
        return;
    }
    // Mostrar el nombre del usuario en la barra de navegación
    const emailEl = document.getElementById('user-email');
    if (emailEl) emailEl.innerText = user.nombre || user.email;

    // Configurar los eventos de los botones del menú
    const btnAgendaV2 = document.getElementById('btn-agenda-v2');
    const btnReservas = document.getElementById('btn-reservas');
    const btnInforme = document.getElementById('btn-informe');
    const btnInformeCanceladas = document.getElementById('btn-informe-canceladas');
    const btnAbmUsu2 = document.getElementById('btn-abmusu2');
    const btnBloqueoHorarios = document.getElementById('btn-bloqueo-horarios');
    const btnCuponera = document.getElementById('btn-cuponera');
    const btnLogout = document.getElementById('logout-btn');

    if (btnReservas) {
        btnReservas.textContent = user?.rol === 'usuario' ? '🕰️ Mis Reservas' : '🕰️ Reservas';
        btnReservas.onclick = (e) => { activarBotonTab(e); mostrarSeccion('mis-reservas-futuras'); };
    }
    if (btnAgendaV2) btnAgendaV2.onclick = (e) => { activarBotonTab(e); mostrarSeccion('agenda-v2'); };
    if (btnInforme) btnInforme.onclick = (e) => { activarBotonTab(e); mostrarSeccion('informe'); };
    if (btnInformeCanceladas) btnInformeCanceladas.onclick = (e) => { activarBotonTab(e); mostrarSeccion('informe-canceladas'); };
    if (btnAbmUsu2) btnAbmUsu2.onclick = (e) => { activarBotonTab(e); mostrarSeccion('abmusu2'); };
    if (btnBloqueoHorarios) btnBloqueoHorarios.onclick = (e) => { activarBotonTab(e); mostrarSeccion('bloqueo'); };
    if (btnCuponera) btnCuponera.onclick = (e) => { activarBotonTab(e); mostrarSeccion('cuponera'); };

    // Helper para resaltar visualmente la pestaña activa
    function activarBotonTab(e) {
        document.querySelectorAll('.btn-tab').forEach(btn => btn.classList.remove('active'));
        if(e && e.target) e.target.classList.add('active');
    }

    if (btnLogout) {
        btnLogout.onclick = () => {
            localStorage.removeItem('usuarioActual');
            sessionStorage.removeItem('usuarioActual');
                localStorage.removeItem('consAge_token');
                sessionStorage.removeItem('consAge_token');
            window.location.href = 'index.html';
        };
    }

    // Por defecto, al entrar, mostramos la sección de Agenda V2
    mostrarSeccion('agenda-v2');

    // Mostrar u ocultar el botón ABM USU, Informe y Bloqueo Horarios solo para admin/gestor
    try {
        if (user && (user.rol === 'admin' || user.rol === 'gestor')) {
            if (btnInforme) btnInforme.style.display = '';
            if (btnInformeCanceladas) btnInformeCanceladas.style.display = '';
            if (btnAbmUsu2) btnAbmUsu2.style.display = '';
            if (btnBloqueoHorarios) btnBloqueoHorarios.style.display = '';
            if (btnBloqueoHorarios) {
                btnBloqueoHorarios.onclick = (e) => { activarBotonTab(e); mostrarSeccion('bloqueo'); };
            }
        } else {
            if (btnInforme) btnInforme.style.display = 'none';
            if (btnInformeCanceladas) btnInformeCanceladas.style.display = 'none';
            if (btnAbmUsu2) btnAbmUsu2.style.display = 'none';
            if (btnBloqueoHorarios) btnBloqueoHorarios.style.display = 'none';
        }
    } catch (e) {
        if (btnInforme) btnInforme.style.display = 'none';
        if (btnInformeCanceladas) btnInformeCanceladas.style.display = 'none';
        if (btnBloqueoHorarios) btnBloqueoHorarios.style.display = 'none';
    }
};


// Inicializar dashboard directamente con login local
initDashboard();

```

### Archivo: `js/informe_modular.js`
```javascript
// js/informe_modular.js
// Versión Final Unificada - ESPACIA con Filtro de Estado Dinámico y Estadísticas Completas

import { APP_CONFIG } from './config.js';
import { getAuthHeaders, mostrarToast, createComboUsuarios, getCanceladoPor } from './utils.js';

let __datosActualesInforme = { reservas: [], totales: {} };

export async function renderInforme(container) {
    const etiquetaGlobal = APP_CONFIG.espacios?.etiquetaGeneral || 'Consultorio';
    const nombreEmpresa = APP_CONFIG.nombreProyecto || '';
    
    container.innerHTML = `
    <div class="informe-container">
        <div style="margin-bottom: 20px; border-left: 5px solid var(--primary, #d35400); padding-left: 15px; display: flex; justify-content: space-between; align-items: center;">
            <div>
                <h2 class="module-title" style="margin:0;">ESPACIA <span style="font-weight:300; color:#666;">| ${nombreEmpresa}</span></h2>
                <small style="color:#888;">Informe detallado de reservas y consumos</small>
            </div>
            <button id="btn-exportar-pdf" class="btn-menu" style="display:none; background: #2c3e50; color: white; border: none; cursor: pointer; padding: 10px 15px; border-radius: 6px; font-weight: bold;">
                <i class="fas fa-file-pdf"></i> Descargar PDF
            </button>
        </div>
        
        <form id="form-informe" class="informe-form" style="display: flex; flex-wrap: wrap; gap: 8px; align-items: center;">
            <div style="display: flex; align-items: center; gap: 5px;">
                <label style="color: #d35400; font-weight: 600; margin: 0;">Inicio:</label>
                <input type="date" name="fechaInicio" required style="padding: 6px; border: 1px solid #ccc; border-radius: 4px; max-width: 115px;">
            </div>
            <div style="display: flex; align-items: center; gap: 5px;">
                <label style="color: #d35400; font-weight: 600; margin: 0;">Fin:</label>
                <input type="date" name="fechaFin" required style="padding: 6px; border: 1px solid #ccc; border-radius: 4px; max-width: 115px;">
            </div>
            <div style="display: flex; align-items: center; gap: 5px;">
                <label style="color: #d35400; font-weight: 600; margin: 0;">${etiquetaGlobal}:</label>
                <select name="consultorio" id="combo-consultorio" style="padding: 6px; border: 1px solid #ccc; border-radius: 4px; max-width: 110px;">
                    <option value="">Todos</option>
                </select>
            </div>
            <div id="search-row-informe" style="display: flex; align-items: center; gap: 5px;"></div>
            
            <div style="display: flex; align-items: center; gap: 5px;">
                <label style="color: #d35400; font-weight: 600; margin: 0;">Estado:</label>
                <select name="estado" id="combo-estado-informe" style="padding: 6px; border: 1px solid #ccc; border-radius: 4px; max-width: 120px;">
                    <option value="">Todos</option>
                    <option value="PENDIENTE">Pendiente</option>
                    <option value="CONSUMIDA">Consumida</option>
                    <option value="CANCELADA">Cancelada</option>
                </select>
            </div>

            <div style="display: flex; align-items: center; gap: 5px; margin-left: 10px;">
                <input type="checkbox" id="chk-agrupar-espacio" name="agruparEspacio" style="accent-color: #d35400; width: 18px; height: 18px;">
                <label for="chk-agrupar-espacio" style="color: #d35400; font-weight: 600; margin: 0; cursor:pointer;">Agrupar</label>
            </div>
            <button type="submit" style="background: var(--primary, #d35400); color: white; border: none; padding: 8px 16px; border-radius: 6px; cursor: pointer; font-weight: bold;">Buscar</button>
        </form>
        <div id="total-horas-informe"></div>
        <div id="contenedor-resultados"><p>Complete los filtros y presione Buscar</p></div>
    </div>`;

    await renderComboUsuariosInforme(container);
    initInforme(container);
}

async function renderComboUsuariosInforme(container) {
    let usuariosLista = [];
    try {
        const resp = await fetch('/.netlify/functions/get-usuarios', { headers: getAuthHeaders() });
        const js = await resp.json();
        if (Array.isArray(js)) {
            usuariosLista = js.filter(u => u.rol === 'usuario').sort((a, b) => (a.nombre || '').localeCompare(b.nombre || ''));
        }
    } catch (e) { console.error(e); }
    const searchRow = container.querySelector('#search-row-informe');
    searchRow.innerHTML = `<label style="color: #d35400; font-weight: 600; margin: 0;">Usuario:</label>`;
    const combo = createComboUsuarios(usuariosLista, {
        id: 'combo-usuario-informe',
        name: 'busqueda',
        clase: '',
        includeTodos: true,
        valueKey: 'email',
        textKey: 'nombre'
    });
    combo.style.padding = '6px';
    combo.style.border = '1px solid #ccc';
    combo.style.borderRadius = '4px';
    combo.style.maxWidth = '160px';
    searchRow.appendChild(combo);
}

export async function initInforme(container) {
    const form = container.querySelector('#form-informe');
    const resultContainer = container.querySelector('#contenedor-resultados');
    const btnExportar = container.querySelector('#btn-exportar-pdf');
    const totalHorasDiv = container.querySelector('#total-horas-informe');
    const comboConsultorio = container.querySelector('#combo-consultorio');

    if (APP_CONFIG.espacios?.items) {
        APP_CONFIG.espacios.items.forEach(c => {
            if (c.id === "0") return;
            const opt = document.createElement('option');
            opt.value = c.id;
            opt.textContent = c.nombreCorto || `Espacio ${c.id}`;
            comboConsultorio.appendChild(opt);
        });
    }

  btnExportar.onclick = async () => {
    if (!__datosActualesInforme.reservas || __datosActualesInforme.reservas.length === 0) return;
    
    mostrarToast("Generando reporte...", "info");
    
    try {
        const form = container.querySelector('#form-informe');
        
        // Preparamos el objeto de filtros exactamente como lo espera el nuevo pdf_reporte.js
        const filtros = {
            reservas: __datosActualesInforme.reservas,
            totales: __datosActualesInforme.totales,
            fechaInicio: form.elements['fechaInicio'].value,
            fechaFin: form.elements['fechaFin'].value,
            estado: form.elements['estado'].value,
            usuario: form.elements['busqueda']?.value || "",
            espacio: form.elements['consultorio'].value
        };

        // Importación dinámica del nuevo archivo unificado
        const moduloPDF = await import('./pdf/pdf_reporte.js');
        
        // Llamada a la función exportada
        await moduloPDF.generarPDFInforme(filtros);

    } catch (err) {
        console.error("Error al exportar PDF:", err);
        mostrarToast("Error al crear el PDF", "error");
    }
};

    form.addEventListener('submit', async (e) => {
        e.preventDefault();
        resultContainer.innerHTML = '<p>Cargando informe...</p>';
        btnExportar.style.display = 'none';

        const fIni = form.elements['fechaInicio'].value;
        const fFin = form.elements['fechaFin'].value;
        const cons = form.elements['consultorio'].value;
        const user = form.elements['busqueda']?.value || '';
        const estadoFiltro = form.elements['estado'].value;
        const prefix = APP_CONFIG.espacios?.prefijoGeneral || 'C';

        let url = `/.netlify/functions/informe_reservas?fechaInicio=${fIni}&fechaFin=${fFin}&prefijoGcal=${prefix}`;
        if (cons) url += `&consultorio=${cons}`;
        if (user) url += `&usuario=${user}`;

        try {
            const resp = await fetch(url, { headers: getAuthHeaders() });
            const data = await resp.json();
            let lista = Array.isArray(data) ? data : (data?.reservas || []);

            if (estadoFiltro) {
                lista = lista.filter(r => {
                    const isC = r.summary?.toLowerCase().includes('cancelada');
                    const isPast = new Date(r.start) < new Date();
                    const est = isC ? 'CANCELADA' : (isPast ? 'CONSUMIDA' : 'PENDIENTE');
                    return est === estadoFiltro;
                });
            }

            if (lista.length === 0) {
                resultContainer.innerHTML = '<p>No se encontraron reservas.</p>';
                totalHorasDiv.innerText = '';
                return;
            }

            btnExportar.style.display = 'block';
            renderReservasTable(lista, resultContainer, totalHorasDiv, false);
        } catch (err) {
            resultContainer.innerHTML = `<p style="color:red">Error: ${err.message}</p>`;
        }
    });
}

function renderReservasTable(reservas, containerDestino, totalHorasDiv, esVistaFuturas) {
    const pageSize = APP_CONFIG.pageSize || 20;
    let currentPage = 1;
    const totalPages = Math.ceil(reservas.length / pageSize) || 1;
    
    const horasMinutos = (val) => {
        const h = Math.floor(val);
        const m = Math.round((val - h) * 60);
        return `${h.toString().padStart(2, '0')}:${m.toString().padStart(2, '0')}`;
    };

    function calcularYGuardarTotales() {
        const intervalos = APP_CONFIG.horarios?.intervalos || [60, 90];
        const initS = () => ({ horas: 0, count: 0, desglose: intervalos.reduce((a, m) => ({...a, [m]: 0}), {}) });
        const stats = { pendientes: initS(), consumidas: initS(), canceladas: initS(), total: initS() };

        function resolveEspacioLabelFromRaw(r) {
            const pref = APP_CONFIG.espacios?.prefijoGeneral || '';
            const items = APP_CONFIG.espacios?.items || [];
            const summaryRaw = (r.summary || '').toString();
            // Buscar patrón tipo C5 o C-5 o C 5
            const re = new RegExp(pref + '\\s*-?\\s*(\\d+)', 'i');
            const mPref = summaryRaw.match(re);
            if (mPref) {
                const num = mPref[1];
                const found = items.find(it => it.id === String(num) || (it.prefijoGcal || '').toLowerCase() === (pref + num).toLowerCase());
                if (found) return found.nombreCorto || `${APP_CONFIG.espacios.etiquetaGeneral} ${num}`;
                return `${APP_CONFIG.espacios.etiquetaGeneral} ${num}`;
            }
            // Si no está el prefijo, intentar tomar la primera parte antes de ':'
            const first = summaryRaw.split(':')[0]?.trim();
            if (first) return first;
            return 'N/A';
        }

        const mapped = reservas.map(r => {
            const dI = new Date(r.start);
            const dF = new Date(r.end);
            const dH = (dF - dI) / 3600000;
            const dM = Math.round((dF - dI) / 60000);
            const isC = r.summary?.toLowerCase().includes('cancelada');
            const isP = dI < new Date();

            const duenioReserva = r.summary?.split(':')[1]?.trim() || '';
            const accionRealizada = getCanceladoPor(r) || "---";
            const espacioLabel = resolveEspacioLabelFromRaw(r);

            // Categorizar por estado real: cancelada, consumida (pasada no cancelada), pendiente (futura)
            if (isC) {
                stats.canceladas.horas += dH;
                stats.canceladas.count++;
                if (intervalos.includes(dM)) stats.canceladas.desglose[dM]++;
            } else if (dI < new Date()) {
                stats.consumidas.horas += dH;
                stats.consumidas.count++;
                if (intervalos.includes(dM)) stats.consumidas.desglose[dM]++;
            } else {
                stats.pendientes.horas += dH;
                stats.pendientes.count++;
                if (intervalos.includes(dM)) stats.pendientes.desglose[dM]++;
            }

            // Acumular en total crudo de reservas (como viene de Google Calendar)
            stats.total.horas += dH;
            stats.total.count++;
            if (intervalos.includes(dM)) stats.total.desglose[dM]++;

                return {
                fecha: dI.toLocaleDateString('es-ES'),
                inicio: dI.toLocaleTimeString('es-ES', {hour:'2-digit', minute:'2-digit'}),
                duracion: `${dM} min`,
                usuario: duenioReserva,
                espacio: espacioLabel,
                    estado: isC ? 'CANCELADA' : (dI < new Date() ? 'CONSUMIDA' : 'PENDIENTE'),
                canceladoPor: accionRealizada 
            };
        });

        __datosActualesInforme = {
            reservas: mapped,
            totales: {
                reservadas: { ...stats.total, horas: horasMinutos(stats.total.horas) },
                pendientes: { ...stats.pendientes, horas: horasMinutos(stats.pendientes.horas) },
                consumidas: { ...stats.consumidas, horas: horasMinutos(stats.consumidas.horas) },
                canceladas: { ...stats.canceladas, horas: horasMinutos(stats.canceladas.horas) }
            }
        };
        return stats;
    }

    function renderPage(page) {
        let html = `<div class="table-main-container"><table class="custom-table"><thead><tr><th>#</th><th>Fecha</th><th>Inic.</th><th>Durac.</th><th>Espacio</th><th>Usuario</th><th>Estado</th></tr></thead><tbody>`;
        const start = (page - 1) * pageSize;
        const end = Math.min(start + pageSize, reservas.length);

            for (let idx = start; idx < end; idx++) {
            const r = reservas[idx];
            const dI = new Date(r.start);
            const dM = Math.round((new Date(r.end) - dI) / 60000);
            const isC = r.summary?.toLowerCase().includes('cancelada');
                const isPast = dI < new Date();
            html += `<tr><td>${idx+1}</td><td>${dI.toLocaleDateString('es-ES')}</td><td>${dI.toLocaleTimeString('es-ES', {hour:'2-digit', minute:'2-digit'})}</td><td>${dM} min</td><td>${r.summary?.split(':')[0] || ''}</td><td>${r.summary?.split(':')[1] || ''}</td><td><span class="status-badge ${isC?'cancelada':isPast?'consumida':'pendiente'}">${isC?'CANCELADA':isPast?'CONSUMIDA':'PENDIENTE'}</span></td></tr>`;
        }
        html += `</tbody></table></div><div class="pagination" style="margin-top:10px; display:flex; gap:10px; align-items:center;"><button id="btn-prev" ${page===1?'disabled':''}>Ant.</button><span>${page} / ${totalPages}</span><button id="btn-next" ${page===totalPages?'disabled':''}>Sig.</button></div>`;
        containerDestino.innerHTML = html;

        containerDestino.querySelector('#btn-prev').onclick = () => { if(currentPage > 1) { currentPage--; renderPage(currentPage); } };
        containerDestino.querySelector('#btn-next').onclick = () => { if(currentPage < totalPages) { currentPage++; renderPage(currentPage); } };

        if (!esVistaFuturas && totalHorasDiv) {
            const st = calcularYGuardarTotales();
            const dg = (o) => Object.entries(o).map(([m, c]) => `${m}m: ${c}`).join(' | ');
            totalHorasDiv.innerHTML = `
            <div class="scorecard-grid">
                <div class="card-stat amber">
                    <div style="width:100%;display:flex;justify-content:center;align-items:center;margin-bottom:8px;">
                        <div class="card-stat-title">Reservadas</div>
                    </div>
                    <div style="width:100%;display:flex;justify-content:center;align-items:center;gap:16px;">
                        <div class="card-stat-time">${horasMinutos(st.total.horas)}</div>
                        <div class="card-stat-desglose">${dg(st.total.desglose)}</div>
                    </div>
                </div>
                <div class="card-stat">
                    <div style="width:100%;display:flex;justify-content:center;align-items:center;margin-bottom:8px;">
                        <div class="card-stat-title">Pendientes</div>
                    </div>
                    <div style="width:100%;display:flex;justify-content:center;align-items:center;gap:16px;">
                        <div class="card-stat-time">${horasMinutos(st.pendientes.horas)}</div>
                        <div class="card-stat-desglose">${dg(st.pendientes.desglose)}</div>
                    </div>
                </div>
                <div class="card-stat green">
                    <div style="width:100%;display:flex;justify-content:center;align-items:center;margin-bottom:8px;">
                        <div class="card-stat-title">Consumidas</div>
                    </div>
                    <div style="width:100%;display:flex;justify-content:center;align-items:center;gap:16px;">
                        <div class="card-stat-time">${horasMinutos(st.consumidas.horas)}</div>
                        <div class="card-stat-desglose">${dg(st.consumidas.desglose)}</div>
                    </div>
                </div>
                <div class="card-stat red">
                    <div style="width:100%;display:flex;justify-content:center;align-items:center;margin-bottom:8px;">
                        <div class="card-stat-title">Canceladas</div>
                    </div>
                    <div style="width:100%;display:flex;justify-content:center;align-items:center;gap:16px;">
                        <div class="card-stat-time">${horasMinutos(st.canceladas.horas)}</div>
                        <div class="card-stat-desglose">${dg(st.canceladas.desglose)}</div>
                    </div>
                </div>
            </div>`;
        }
    }
    renderPage(currentPage);
}
```

### Archivo: `js/login_local.js`
```javascript
// login_local.js
// Valida usuario y contraseña contra usuarios.json (local)
import { APP_CONFIG, APP_VERSION } from './config.js';
import { hashPassword } from './utils.js';

// Establecer el título del proyecto basado en config.js
if (APP_CONFIG && APP_CONFIG.nombreProyecto) {
    document.title = APP_CONFIG.nombreProyecto;
}

// Inyectar versión en UI
document.addEventListener('DOMContentLoaded', () => {
    const versionDisplays = document.querySelectorAll('#app-version-display, .app-version-display');
    versionDisplays.forEach(el => {
        el.textContent = APP_VERSION || "v-.-.-";
    });
});

// Función para aplicar el fondo según el tema
function aplicarFondo() {
    const currentTheme = localStorage.getItem('espacia_theme') || 'classic';
    if (currentTheme === 'classic' && APP_CONFIG.estilos && APP_CONFIG.estilos.fondo) {
        document.body.style.background = APP_CONFIG.estilos.fondo;
    } else {
        document.body.style.background = ''; // Deja que el CSS del tema actúe
    }
    document.body.style.minHeight = '100vh';
}

// Aplicar fondo inicial
aplicarFondo();

// Escuchar cambios en el tema desde el selector
document.addEventListener('themeChanged', aplicarFondo);

async function login(isVoluntary = false) {
    const usuarioInput = document.getElementById('usuario').value.trim();
    const contrasena = document.getElementById('contrasena').value;
    const errorDiv = document.getElementById('error');
    errorDiv.textContent = '';

    if (!usuarioInput || !contrasena) {
        if (isVoluntary) {
            errorDiv.textContent = 'Para cambiar tu contraseña, ingresa tu nombre de usuario y contraseña actual arriba.';
        } else {
            errorDiv.textContent = 'Por favor, complete ambos campos.';
        }
        return;
    }

    const bodyLogin = {
        nomUsu: usuarioInput,
        contrasenaHash: await hashPassword(contrasena)
    };

    try {
        const response = await fetch('/.netlify/functions/login', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(bodyLogin)
        });
        const result = await response.json();
        if (response.ok && result.success) {
            const user = result.user;
            if (user.requiereCambioPass || isVoluntary) {
                // Forzar u ofrecer cambio de contraseña
                // Crear el CSS del modal si no existe
                if (!document.getElementById('modal-styles')) {
                    const style = document.createElement('style');
                    style.id = 'modal-styles';
                    style.innerHTML = `
                        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); display: flex; justify-content: center; align-items: center; z-index: 9999; backdrop-filter: blur(3px); }
                        .modal-box { background: var(--white, white); color: var(--text-main, #333); padding: 25px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.2); width: 90%; max-width: 400px; text-align: center; }
                        .modal-title { margin-top: 0; color: #2c3e50; font-size: 1.3rem; margin-bottom: 5px; }
                        .modal-text { color: #666; margin-bottom: 20px; font-size: 0.95rem; }
                        .modal-input { width: 100%; padding: 10px; margin-bottom: 15px; border: 1px solid #ccc; border-radius: 6px; box-sizing: border-box; font-size: 1rem; text-align: center; }
                        .modal-btn { background: #27ae60; color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; font-size: 1rem; }
                        .modal-btn:hover { background: #219653; }
                        .modal-error { color: #e74c3c; font-size: 0.85rem; margin-top: -10px; margin-bottom: 15px; display: none; }
                    `;
                    document.head.appendChild(style);
                }

                // Incluir FontAwesome en el head si no existe
                if (!document.getElementById('fa-styles')) {
                    const faStyle = document.createElement('link');
                    faStyle.id = 'fa-styles';
                    faStyle.rel = 'stylesheet';
                    faStyle.href = 'https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css';
                    document.head.appendChild(faStyle);
                }

                // Crear modal HTML con FontAwesome
                const modalHtml = `
                    <div class="modal-overlay" id="modal-cambio-pass">
                        <div class="modal-box">
                            <h3 class="modal-title">Cambio de Contraseña</h3>
                            <p class="modal-text" id="modal-desc-pass">Por seguridad, crea una contraseña de al menos 6 caracteres.</p>
                            <div style="position: relative; margin-bottom: 10px;">
                                <input type="password" id="nueva-pass-input" class="modal-input" placeholder="Nueva contraseña" autofocus style="margin-bottom: 0; padding-right: 40px;">
                                <button type="button" id="toggle-nueva-pass" tabindex="-1" style="position: absolute; right: 12px; top: 50%; transform: translateY(-50%); background: transparent; border: none; padding: 0; margin: 0; cursor: pointer; color: #666; font-size: 1.3rem;">
                                    <i class="fa fa-eye" id="icon-nueva-pass"></i>
                                </button>
                            </div>
                            <div style="position: relative; margin-bottom: 15px;">
                                <input type="password" id="confirmar-pass-input" class="modal-input" placeholder="Confirmar contraseña" style="margin-bottom: 0; padding-right: 40px;">
                                <button type="button" id="toggle-confirmar-pass" tabindex="-1" style="position: absolute; right: 12px; top: 50%; transform: translateY(-50%); background: transparent; border: none; padding: 0; margin: 0; cursor: pointer; color: #666; font-size: 1.3rem;">
                                    <i class="fa fa-eye" id="icon-confirmar-pass"></i>
                                </button>
                            </div>
                            <div id="modal-error-msg" class="modal-error">La contraseña no cumple los requisitos</div>
                            <button id="btn-guardar-pass" class="modal-btn">Guardar y Entrar</button>
                            ${isVoluntary ? '<button id="btn-cancelar-pass" class="modal-btn" style="background: transparent; color: #777; margin-top: 10px; font-weight: normal; border: 1px solid #ccc;">Cancelar</button>' : ''}
                        </div>
                    </div>
                `;

                document.body.insertAdjacentHTML('beforeend', modalHtml);

                const modalObj = document.getElementById('modal-cambio-pass');
                const btnGuardarPass = document.getElementById('btn-guardar-pass');
                const btnCancelarPass = document.getElementById('btn-cancelar-pass');
                const inputPass = document.getElementById('nueva-pass-input');
                const confirmPass = document.getElementById('confirmar-pass-input');
                const errorMsg = document.getElementById('modal-error-msg');
                const descText = document.getElementById('modal-desc-pass');
                // Botones FontAwesome
                const btnToggleNuevaPass = document.getElementById('toggle-nueva-pass');
                const iconNuevaPass = document.getElementById('icon-nueva-pass');
                const btnToggleConfirmarPass = document.getElementById('toggle-confirmar-pass');
                const iconConfirmarPass = document.getElementById('icon-confirmar-pass');

                // Función para alternar visibilidad y icono
                btnToggleNuevaPass.addEventListener('click', () => {
                    inputPass.type = inputPass.type === 'password' ? 'text' : 'password';
                    iconNuevaPass.className = inputPass.type === 'password' ? 'fa fa-eye' : 'fa fa-eye-slash';
                });
                btnToggleConfirmarPass.addEventListener('click', () => {
                    confirmPass.type = confirmPass.type === 'password' ? 'text' : 'password';
                    iconConfirmarPass.className = confirmPass.type === 'password' ? 'fa fa-eye' : 'fa fa-eye-slash';
                });

                // Si no es admin ni gestor, le exigimos reglas fuertes
                const isNormalUser = (user.rol !== 'admin' && user.rol !== 'gestor');
                if (isNormalUser) {
                    descText.textContent = "Por seguridad, crea una contraseña de al menos 6 caracteres, incluyendo una mayúscula, un número y un carácter especial.";
                }

                // Permitir enter en el segundo input
                confirmPass.addEventListener('keyup', (e) => {
                    if (e.key === 'Enter') procesarCambioPass();
                });

                // Asegurar foco en el input
                setTimeout(() => inputPass.focus(), 100);

                // Función que maneja el guardado desde el Modal
                const procesarCambioPass = async () => {
                    const nuevaPass = inputPass.value;
                    const passConfirmada = confirmPass.value;
                    
                    // Validar longitud
                    if (!nuevaPass || nuevaPass.length < 6) {
                        errorMsg.textContent = 'La contraseña debe tener al menos 6 caracteres.';
                        errorMsg.style.display = 'block';
                        return;
                    }

                    // Validar mayúscula, número y especial con regex SOLO si es usuario normal
                    if (isNormalUser) {
                        const tieneMayuscula = /[A-Z]/.test(nuevaPass);
                        const tieneNumero = /[0-9]/.test(nuevaPass);
                        const tieneEspecial = /[^A-Za-z0-9]/.test(nuevaPass);
                        
                        if (!tieneMayuscula || !tieneNumero || !tieneEspecial) {
                            errorMsg.textContent = 'Debe contener al menos una letra mayúscula, un número y un carácter especial.';
                            errorMsg.style.display = 'block';
                            return;
                        }
                    }

                    if (nuevaPass !== passConfirmada) {
                        errorMsg.textContent = 'Las contraseñas no coinciden.';
                        errorMsg.style.display = 'block';
                        return;
                    }

                    errorMsg.style.display = 'none';

                    btnGuardarPass.disabled = true;
                    btnGuardarPass.textContent = 'Guardando...';
                    
                    const nuevaHash = await hashPassword(nuevaPass);
                    user.requiereCambioPass = false;
                    
                    try {
                        const resp = await fetch('/.netlify/functions/change-password', {
                            method: 'POST',
                            headers: { 
                                'Content-Type': 'application/json',
                                'Authorization': `Bearer ${result.token}`
                            },
                            body: JSON.stringify({ nuevaContrasenaHash: nuevaHash })
                        });
                        const updateResult = await resp.json();
                        
                        if (!resp.ok || !updateResult.success) {
                            modalObj.remove();
                            errorDiv.textContent = 'No se pudo guardar la contraseña en el servidor.';
                            return;
                        }
                        
                        // Si todo fue bien, cerramos el modal y logueamos
                        modalObj.remove();
                        const { contrasena: _, ...userSinPass } = user;
                        localStorage.setItem('usuarioActual', JSON.stringify(userSinPass));
                        sessionStorage.setItem('usuarioActual', JSON.stringify(userSinPass));
                            localStorage.setItem('consAge_token', result.token);
                            sessionStorage.setItem('consAge_token', result.token);
                        window.location.href = "dashboard.html";

                    } catch (e) {
                        modalObj.remove();
                        errorDiv.textContent = 'Error de conexión al guardar la nueva contraseña.';
                    }
                };

                // Asignar eventos
                btnGuardarPass.addEventListener('click', procesarCambioPass);
                inputPass.addEventListener('keypress', (e) => {
                    if (e.key === 'Enter') procesarCambioPass();
                });
                if (btnCancelarPass) {
                    btnCancelarPass.addEventListener('click', () => {
                        modalObj.remove();
                    });
                }
                
                // Detener la ejecución del resto del script aquí porque el modal es asíncrono puro
                return;
            }
            
            // Guardar usuario autenticado en localStorage (asegurándonos de no guardar la contraseña)
            const { contrasena: _, ...userSinPass } = user;
            try {
                localStorage.setItem('usuarioActual', JSON.stringify(userSinPass));
                sessionStorage.setItem('usuarioActual', JSON.stringify(userSinPass)); // Fallback
                if (result.token) {
                    localStorage.setItem('consAge_token', result.token);
                    sessionStorage.setItem('consAge_token', result.token);
                }
                // Pequeño delay para asegurar persistencia antes de redirigir
                setTimeout(() => {
                    window.location.href = 'dashboard.html';
                }, 100);
            } catch (e) {
                errorDiv.textContent = 'Error guardando usuario en localStorage.';
            }
        } else {
            errorDiv.textContent = result.error || 'Usuario o contraseña incorrectos.';
        }
    } catch (e) {
        errorDiv.textContent = 'Error al validar usuario.';
        console.error(e);
    }
}

// Event Listeners
document.addEventListener('DOMContentLoaded', () => {
    const btnLogin = document.getElementById('btnLogin');
    if (btnLogin) {
        btnLogin.addEventListener('click', () => { login(false); });
    }

    const btnCambioPassVoluntario = document.getElementById('btnCambioPassVoluntario');
    if (btnCambioPassVoluntario) {
        btnCambioPassVoluntario.addEventListener('click', (e) => {
            e.preventDefault();
            login(true);
        });
    }

    // Permitir login con Enter
    const inputs = ['usuario', 'contrasena'];
    inputs.forEach(id => {
        const el = document.getElementById(id);
        if (el) {
            el.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') login(false);
            });
        }
    });
});

```

### Archivo: `js/modelo/reserva.js`
```javascript
// Clase Reserva para agenda_v2.js
import { getAuthHeaders } from '../utils.js';
import { APP_CONFIG } from '../config.js';

export class Reserva {
    constructor(usuario = null, fecha = null, h_inicio = null, lugar = null, creadoPor = null, estado = null, email = null, id = null, intervalo = null, colorId = null, prefijoGcal = null, etiquetaGeneral = null){
        this.usuario = usuario;
        this.fecha = fecha;
        this.h_inicio = h_inicio; // string HH:MM
        this.intervalo = intervalo;
        this.h_fin = this.calcularHoraFin(this.h_inicio, this.intervalo);
        this.lugar = lugar;
        this.creadoPor = creadoPor;
        this.canceladoPor = null;
        this.fechaCreacion = null; // ISO string desde Google Calendar (ev.created)
        this.estado = estado;
        this.email = email;
        this.id = id;
        this.colorId = colorId;
        this.prefijoGcal = prefijoGcal !== null ? prefijoGcal : (APP_CONFIG.espacios?.prefijoGeneral || 'C');
        this.etiquetaGeneral = etiquetaGeneral !== null ? etiquetaGeneral : (APP_CONFIG.espacios?.etiquetaGeneral || 'Consultorio');
        // Variables para backend
        this.hora = null;
        this.minutos = null;
        if (this.h_inicio) {
            const [h, m] = this.h_inicio.split(':').map(Number);
            this.hora = h;
            this.minutos = m;
        }
    }

    calcularHoraFin(h_inicio, intervalo) {
        if (!h_inicio || !intervalo) return '';
        const [h, m] = h_inicio.split(':').map(Number);
        const fObj = new Date(2000, 0, 1, h, m);
        fObj.setMinutes(fObj.getMinutes() + parseInt(intervalo));
        return `${fObj.getHours().toString().padStart(2, '0')}:${fObj.getMinutes().toString().padStart(2, '0')}`;
    }

    async guardar() {
        const body = {
            email: this.email,
            nombre: this.usuario,
            consultorio: this.lugar,
            fecha: this.fecha,
            hora: this.hora,
            minutos: this.minutos,
            intervalo: this.intervalo,
            colorId: this.colorId,
            creadoPor: this.creadoPor,
            prefijoGcal: this.prefijoGcal,
            etiquetaGeneral: this.etiquetaGeneral
        };
        const resp = await fetch('/.netlify/functions/reservar', {
            method: 'POST',
            headers: getAuthHeaders(),
            body: JSON.stringify(body)
        });
        return resp;
    }

    async cancelar() {
        if (!this.id) throw new Error('Falta id de reserva para cancelar');
        const body = {
            eventId: this.id,
            nombreUsuario: this.usuario,
            esAdmin: !!this.creadoPor
        };
        const resp = await fetch('/.netlify/functions/reservar', {
            method: 'DELETE',
            headers: getAuthHeaders(),
            body: JSON.stringify(body)
        });
        return resp;
    }

    async imprimirTicketReserva() {
        const modT = await import('../pdf/pdf_ticket_reserva.js');
        const prefijo = this.etiquetaGeneral?.charAt(0) || 'E';
        await modT.generarTicketReserva({
            nombre: this.usuario,
            fecha: this.fecha,
            hora: this.h_inicio,
            consultorio: this.lugar,
            email: this.email,
            intervalo: this.intervalo,
            inicio: this.h_inicio,
            fin: this.h_fin,
            espacio: prefijo + String(this.lugar),
            usuario: this.usuario,
            realizadaPorAdmin: !!this.creadoPor,
            fechaCreacion: this.fechaCreacion || null
        });
    }

    async imprimirTicketCancelacion() {
         const modCancel = await import('../pdf/pdf_ticket_cancela.js');
         const reservaCancelada = {
            usuario:      this.usuario      || '',
            email:        this.email        || '',
            fecha:        this.fecha        || '',
            inicio:       this.h_inicio     || '',
            fin:          this.h_fin        || '',
            intervalo:    this.intervalo    || '',
            espacio:      this.lugar        || '',
            creadoPor:    this.creadoPor    || '',
            canceladoPor: this.canceladoPor || this.usuario || '',
            fechaCreacion: this.fechaCreacion || null
        };
        await modCancel.generarTicketCancelacion(reservaCancelada);
    }
}
```

### Archivo: `js/pdf/pdf_core.js`
```javascript
import { APP_CONFIG } from '../config.js';

export async function prepararDocumento(formato = 'a4') {
    if (!window.jspdf) {
        try {
            await import('https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js');
        } catch (e) {
            throw new Error('No se pudo cargar jsPDF. Verificá tu conexión a internet.');
        }
    }
    // Carga autotable solo si es un informe A4
    if (formato === 'a4' && !window.jspdf.jsPDF.API.autoTable) {
        try {
            await import('https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js');
        } catch (e) {
            throw new Error('No se pudo cargar jspdf-autotable. Verificá tu conexión a internet.');
        }
    }

    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ orientation: 'p', unit: 'mm', format: formato });
    
    const rawTel = APP_CONFIG.EMPRESA_TELEFONO || '';
    const config = {
        nombre: APP_CONFIG.EMPRESA_NOMBRE || '',
        logo: APP_CONFIG.EMPRESA_LOGO || 'imagenes/logo_default.png',
        color: [211, 84, 0], // Naranja ESPACIA
        width: doc.internal.pageSize.getWidth(),
        height: doc.internal.pageSize.getHeight(),
        telefono: rawTel.replace(/\D/g, '').length === 9 
            ? `${rawTel.slice(0, 3)} ${rawTel.slice(3, 5)} ${rawTel.slice(5)}`
            : rawTel
    };

    return { doc, config };
}

export function dibujarEncabezado(doc, config, subtitulo) {
    const xMargin = 14;
    // Marca
    doc.setFontSize(16).setTextColor(config.color[0], config.color[1], config.color[2]).text("ESPACIA", xMargin, 15);
    doc.setFontSize(9).setTextColor(100).text(subtitulo, xMargin, 20);

    // Logo
    try {
        doc.addImage(config.logo, 'PNG', config.width - 35, 8, 25, 12);
    } catch (e) { console.warn("Logo no disponible"); }

    // Línea de marca
    doc.setDrawColor(config.color[0], config.color[1], config.color[2]).setLineWidth(0.5).line(xMargin, 25, config.width - xMargin, 25);
}

export function dibujarPiePagina(doc, config) {
    const pageCount = doc.internal.getNumberOfPages();
    for (let i = 1; i <= pageCount; i++) {
        doc.setPage(i);
        doc.setFontSize(8).setTextColor(150).setFont(undefined, 'italic');
        doc.text(`Gracias por elegir ${config.nombre}`, config.width / 2, config.height - 10, { align: 'center' });
    }
}
```

### Archivo: `js/pdf/pdf_reporte.js`
```javascript
// pdf_reporte.js - Generador Unificado de Informes para ESPACIA
import { prepararDocumento, dibujarEncabezado, dibujarPiePagina } from './pdf_core.js';
import { APP_CONFIG } from '../config.js';
import { getCanceladoPor } from '../utils.js';

/**
 * Función principal que decide qué tipo de reporte generar basándose en los filtros.
 */
export async function generarPDFInforme(filtros) {
    try {
        const { reservas = [], totales = {}, fechaInicio = '', fechaFin = '', estado = '', usuario = '', espacio = '' } = filtros;
        
        // 1. Preparar documento base (A4)
        const { doc, config } = await prepararDocumento('a4');
        
        // 2. Determinar subtítulo y lógica de nombre de archivo
        let subtituloHeader = "Informe de Movimientos";
        let nombreArchivoPrefijo = "Informe_General";
        let resumenTitle = 'RESUMEN DE GESTIÓN';

        if (usuario) {
            const nombreUser = document.getElementById('combo-usuario-informe')?.options[document.getElementById('combo-usuario-informe').selectedIndex]?.text || usuario;
            // Mantener el encabezado general inalterado y mover el nombre al título del resumen
            nombreArchivoPrefijo = `Informe_Particular_${nombreUser.replace(/\s+/g, '_')}`;
            resumenTitle = `RESUMEN DE GESTIÓN - ${nombreUser}`;
        } else if (estado) {
            subtituloHeader = `Informe de Reservas: ${estado}`;
            nombreArchivoPrefijo = `Reporte_${estado}`;
        }

        // 3. Dibujar Encabezado
        dibujarEncabezado(doc, config, subtituloHeader);

        // 4. Información de Período
        const rangoFechas = `${fechaInicio.split('-').reverse().join('/')} al ${fechaFin.split('-').reverse().join('/')}`;
        doc.setFontSize(10).setTextColor(40).setFont(undefined, 'bold');
        doc.text(`Período: ${rangoFechas}`, 14, 30);

        // --- SECCIÓN 1: RESUMEN DE GESTIÓN ---
        const intervalos = APP_CONFIG.horarios?.intervalos || [60, 90];
        const cabeceraDesglose = `Desglose (${intervalos.map(m => m + 'm').join('/')})`;
        
        doc.setFontSize(13).setTextColor(40).text(resumenTitle, 14, 42);

        let filasResumen = [];
        if (estado) {
            // Si hay filtro de estado, mapear a la clave correcta en `totales`
            const e = estado.toLowerCase();
            let clave = 'reservadas';
            if (e === 'cancelada') clave = 'canceladas';
            else if (e === 'consumida' || e === 'usada') clave = 'consumidas';
            else if (e === 'pendiente') clave = 'pendientes';
            filasResumen.push(generarFilaResumen(estado, totales[clave], intervalos));
        } else {
            // Si es general o por usuario, mostramos todo el desglose incluyendo pendientes
            filasResumen = [
                generarFilaResumen('Reservas Totales', totales.reservadas, intervalos),
                generarFilaResumen('Pendientes', totales.pendientes, intervalos),
                generarFilaResumen('Consumidas', totales.consumidas, intervalos),
                generarFilaResumen('Canceladas', totales.canceladas, intervalos)
            ];
        }

        doc.autoTable({
            startY: 45,
            head: [['Estado', 'Tiempo Total', 'Cantidad', cabeceraDesglose]],
            body: filasResumen,
            theme: 'grid',
            headStyles: { fillColor: config.color, fontSize: 11, halign: 'center' },
            styles: { fontSize: 11, cellPadding: 4, valign: 'middle' },
            columnStyles: { 1: { halign: 'center' }, 2: { halign: 'center' }, 3: { halign: 'center' } }
        });

        // --- SECCIÓN 2: DETALLE DE MOVIMIENTOS ---
        doc.setFontSize(11).setTextColor(40).text('DETALLE DE MOVIMIENTOS', 14, doc.lastAutoTable.finalY + 12);
        
        const etiquetaEspacio = APP_CONFIG.espacios?.etiquetaGeneral || 'Espacio';
        const headersDetalle = ['Fecha', 'Inic.', 'Durac.', 'Usuario', etiquetaEspacio, 'Estado', 'Cancelado por'];

        // Resolver cancelador con fallback: usar getCanceladoPor si existe, sino r.canceladoPor o 'Administración'
        function resolveCancelador(r) {
            try {
                if (typeof getCanceladoPor === 'function') {
                    const val = getCanceladoPor(r);
                    if (val) return val;
                }
            } catch (e) {
                // ignore and fallback
            }
            return r.canceladoPor || 'Administración';
        }

        doc.autoTable({
            startY: doc.lastAutoTable.finalY + 15,
            head: [headersDetalle],
            body: reservas.map(r => {
                const isCancelada = (r.estado && r.estado.toUpperCase() === 'CANCELADA');
                const cancelador = isCancelada ? resolveCancelador(r) : '';

                return [
                    r.fecha,
                    r.inicio,
                    r.duracion,
                    r.usuario,
                    r.espacio,
                    r.estado.toUpperCase(),
                    cancelador
                ];
            }),
            theme: 'striped',
            headStyles: { fillColor: [80, 80, 80], fontSize: 9 },
            styles: { fontSize: 8.5 },
            columnStyles: {
                5: { fontStyle: 'bold' },
                6: { textColor: [180, 0, 0], fontStyle: 'bold', halign: 'center' }
            }
        });

        // 5. Pie de página y Guardar
        dibujarPiePagina(doc, config);
        
        const nombreCons = espacio ? (document.getElementById('combo-consultorio')?.options[document.getElementById('combo-consultorio').selectedIndex]?.text || espacio) : "Todos";
        const nombreFinal = `${nombreArchivoPrefijo}_${nombreCons}_${fechaInicio}_a_${fechaFin}.pdf`.replace(/\s+/g, '_');
        
        doc.save(nombreFinal);

    } catch (e) {
        console.error("Error al generar el PDF unificado:", e);
    }
}

// Helpers internos
function generarFilaResumen(label, stat, intervalos) {
    return [
        label, 
        stat?.horas || '00:00', 
        stat?.count || 0, 
        intervalos.map(m => stat?.desglose?.[m] || 0).join(' / ')
    ];
}
```

### Archivo: `js/pdf/pdf_ticket_cancela.js`
```javascript
export async function generarTicketCancelacion(reserva) {
    // Fix: imports fuera del try/catch para que errores de carga no queden silenciados
    const { prepararDocumento, dibujarEncabezado, dibujarPiePagina } = await import('./pdf_core.js');
    const { APP_CONFIG } = await import('../config.js');
    try {
        let profesional = reserva.usuario;
        let fecha       = reserva.fecha;
        let inicio      = reserva.inicio;
        let fin         = reserva.fin;
        let espacio     = reserva.espacio;

        // Si vienen summary/description, extraer datos desde ahí
        if (reserva.summary || reserva.description) {
            if (reserva.summary) {
                const match = reserva.summary.match(/Reserva: (.+)/);
                if (match) profesional = match[1];
            }
            if (reserva.description) {
                const fechaMatch = reserva.description.match(/Fecha:\s*([\d\/-]+)/);
                if (fechaMatch && (!reserva.fecha || reserva.fecha === '' || reserva.fecha === 'undefined')) {
                    fecha = fechaMatch[1];
                }

                const horaRango = reserva.description.match(/Hora:\s*([\d:]+)\s*a\s*([\d:]+)\s*hs/);
                if (horaRango) {
                    inicio = horaRango[1];
                    fin    = horaRango[2];
                } else {
                    const horaSimple = reserva.description.match(/Hora:\s*([\d:]+)\s*hs/);
                    const intervalo  = reserva.description.match(/Intervalo:\s*(\d+)/);
                    if (horaSimple) {
                        inicio = horaSimple[1];
                        if (intervalo) {
                            const [h, m] = inicio.split(':').map(Number);
                            const fObj = new Date(2000, 0, 1, h, m);
                            fObj.setMinutes(fObj.getMinutes() + parseInt(intervalo[1]));
                            fin = `${fObj.getHours().toString().padStart(2, '0')}:${fObj.getMinutes().toString().padStart(2, '0')}`;
                        }
                    }
                }

                const espacioMatch = reserva.description.match(/Consultorio:\s*([\w\d]+)/);
                if (espacioMatch) {
                    const prefijo  = APP_CONFIG.espacios?.prefijoGeneral || 'C';
                    const etiqueta = APP_CONFIG.espacios?.etiquetaGeneral || 'Lugar';
                    if (espacioMatch[1].length > 1 && espacioMatch[1][0].toUpperCase() === prefijo.toUpperCase()) {
                        espacio = etiqueta + ' ' + espacioMatch[1].slice(1);
                    } else {
                        espacio = etiqueta + ' ' + espacioMatch[1];
                    }
                }
            }
        }

        // Normalizar espacio siempre — puede llegar como "4", "C4" o "Consultorio 4"
        const prefijo2  = APP_CONFIG.espacios?.prefijoGeneral || 'C';
        const etiqueta2 = APP_CONFIG.espacios?.etiquetaGeneral || 'Consultorio';
        espacio = String(espacio || '');
        if (espacio && !espacio.toLowerCase().startsWith(etiqueta2.toLowerCase())) {
            // Quitar prefijo si viene como "C4" → "4"
            if (espacio[0].toUpperCase() === prefijo2.toUpperCase()) {
                espacio = etiqueta2 + ' ' + espacio.slice(1);
            } else if (!isNaN(espacio)) {
                // Es solo número "4" → "Consultorio 4"
                espacio = etiqueta2 + ' ' + espacio;
            }
        }

        const { doc, config } = await prepararDocumento('a5');
        dibujarEncabezado(doc, config, "Comprobante de Cancelación");

        const xMargin = 14;
        doc.setFontSize(11).setTextColor(40).setFont(undefined, 'bold').text("DETALLE DE CANCELACIÓN", xMargin, 35);

        // Mostrar claramente creador y cancelador:
        // - reserva.creadoPor = quien originalmente hizo la reserva (puede ser gestor/admin o el mismo usuario)
        // - reserva.canceladoPor = quien ejecutó la cancelación
        // Usar utilidades centralizadas para consistencia
        const { getRealizadoPor, getCanceladoPor } = await import('../utils.js');
        const displayCreadoPor = getRealizadoPor(reserva) || (reserva.usuario || '');
        const displayCanceladoPor = getCanceladoPor(reserva);

        // Formatear fecha de creación desde ISO a formato local
        const fechaCreacionDisplay = reserva.fechaCreacion
            ? new Date(reserva.fechaCreacion).toLocaleString('es-UY', { dateStyle: 'short', timeStyle: 'short' })
            : '';

        // Importar función utilitaria
        const { formatFechaCorta, parseFecha } = await import('../utils.js');
        const fechaFinal = parseFecha(reserva.fecha) || parseFecha(reserva.description?.match(/Fecha:\s*([\d\/\-]+)/)?.[1]);
        const lineas = [
            ["Profesional:",           profesional],
            ["Fecha:",                 formatFechaCorta(fechaFinal)],
            ["Horario:",               inicio && fin ? `${inicio} a ${fin} hs` : ""],
            ["Lugar:",                 espacio],
            ["Reserva realizada por:", displayCreadoPor],
            ...(fechaCreacionDisplay ? [["Fecha de reserva:", fechaCreacionDisplay]] : []),
            ["Cancelada por:",         displayCanceladoPor],
            ["Fecha de cancelación:",  reserva.fechaCancelacion || new Date().toLocaleString('es-UY')]
        ];

        doc.setFontSize(10).setTextColor(40);
        let y = 45;
        lineas.forEach((linea, i) => {
            doc.setFont(undefined, 'normal').text(linea[0], xMargin, y);
            doc.setFont(undefined, 'bold').text(String(linea[1] || ''), 60, y);
            y += (i === 3 ? 16 : 12); // Más espacio después de Consultorio
        });

        dibujarPiePagina(doc, config);

        const nombreDoc = (profesional || "Cancelacion").replace(/\s+/g, '_');
        const fechaDoc  = (fecha || "sin_fecha").replace(/\//g, '-');
        doc.save(`Ticket_Cancelacion_${nombreDoc}_${fechaDoc}.pdf`);

    } catch (e) {
        console.error("Error en Ticket Cancelación:", e);
    }
}
```

### Archivo: `js/pdf/pdf_ticket_reserva.js`
```javascript
import { prepararDocumento, dibujarEncabezado, dibujarPiePagina } from './pdf_core.js';
import { APP_CONFIG } from '../config.js';
import { parseFecha, formatFechaCorta, getRealizadoPor, getCanceladoPor } from '../utils.js';

export async function generarTicketReserva(reserva) {
    try {
        const { doc, config } = await prepararDocumento('a5');
        dibujarEncabezado(doc, config, "Comprobante de Reserva");
        const xMargin = 14;
        doc.setFontSize(11).setTextColor(40).setFont(undefined, 'bold').text("DETALLES DEL TURNO", xMargin, 35);
        // Mostrar 'Lugar' usando ESPACIOS_ETIQUETA si el prefijo coincide
        let lugar = String(reserva.espacio || '');
        const prefijo = APP_CONFIG.espacios?.prefijoGeneral || 'C';
        const etiqueta = APP_CONFIG.espacios?.etiquetaGeneral || 'Lugar';
        if (lugar.length > 1 && lugar[0].toUpperCase() === prefijo.toUpperCase()) {
            lugar = etiqueta + ' ' + lugar.slice(1);
        } else if (lugar && !isNaN(lugar)) {
            // Si es solo un número (ej: "1"), agregar la etiqueta directamente
            lugar = etiqueta + ' ' + lugar;
        }
       const displayCreadoPor = getRealizadoPor(reserva) || (reserva.usuario || '');

       const fechaRes = parseFecha(reserva.fecha) || parseFecha(reserva.description?.match(/Fecha:\s*([\d\/\-]+)/)?.[1]);
       const fechaMostrar = fechaRes ? formatFechaCorta(fechaRes) : '';
       const displayCanceladoPor =  '';

       const lineas = [
           ["Profesional:", reserva.usuario],
           ["Fecha:", fechaMostrar],
           ["Horario:", `${reserva.inicio} a ${reserva.fin} hs`],
           ["Lugar:", lugar],
           ["Reserva realizada por:", displayCreadoPor],
           ["Fecha de reserva:", reserva.fechaCreacion
               ? new Date(reserva.fechaCreacion).toLocaleString('es-UY', { dateStyle: 'short', timeStyle: 'short' })
               : ''],
           ["Cancelada por:", displayCanceladoPor],
           ["Fecha de cancelación:", reserva.fechaCancelacion || '']
       ];
    doc.setFontSize(10).setTextColor(40);
    let y = 45;
    lineas.forEach((linea, i) => {
        doc.setFont(undefined, 'normal').text(linea[0], xMargin, y);
        doc.setFont(undefined, 'bold').text(String(linea[1] || ''), 60, y);
        y += (i === 3 ? 16 : 12); // Más espacio después de Lugar
    });
        dibujarPiePagina(doc, config);
        const nombreDoc = (reserva.usuario || "Reserva").replace(/\s+/g, '_');
        doc.save(`Ticket_${nombreDoc}_${reserva.fecha}.pdf`);
    } catch (e) { console.error("Error en Ticket:", e); }
}
```

### Archivo: `js/persistencia/abm_usuario.js`
```javascript
// js/persistencia/abm_usuario.js
// ABM de Usuarios — Orden Alfabético y Lógica FabioRE

import { APP_CONFIG } from '../config.js';
import { getAuthHeaders, hashPassword, mostrarToast, mostrarModalConfirmacion } from '../utils.js';

let usuarios = [];

export async function renderAbmUsu2(container) {
    container.innerHTML = `
        <div class="abmusu-section">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:24px;">
                <h2 class="module-title" style="margin:0;">👥 Gestión de Usuarios</h2>
                <button id="btn-nuevo-usuario" style="background:var(--primary); color:white; border:none; padding:10px 20px; border-radius:8px; cursor:pointer; font-weight:bold; font-size:0.95rem;">
                    + Nuevo Usuario
                </button>
            </div>
            <div style="margin-bottom:16px; display:flex; gap:8px; align-items:center;">
                <input id="filtro-usuarios" type="text" placeholder="Filtrar por nombre, ID o rol..." style="width:320px; padding:8px; border:1px solid #ccc; border-radius:6px; font-size:1rem;">
                <button id="filtro-clear-btn" title="Limpiar filtro" style="background:#f1f1f1; border:none; border-radius:6px; padding:8px 12px; cursor:pointer; font-size:1.1em; color:#888;">✖</button>
            </div>
            <div class="table-responsive">
                <table id="usuarios-table">
                    <thead>
                        <tr>
                            <th>Nombre</th>
                            <th>ID Usuario</th>
                            <th>Rol</th>
                            <th>Estado</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody id="usuarios-tbody">
                        <tr><td colspan="5" style="text-align:center; color:#888; padding:30px;">Cargando usuarios...</td></tr>
                    </tbody>
                </table>
            </div>
        </div>
    `;

    container.querySelector('#btn-nuevo-usuario').onclick = () => abrirModal(null);
    await cargarUsuarios();

    // Filtro local
    const filtroInput = container.querySelector('#filtro-usuarios');
    filtroInput.addEventListener('input', function() {
        const texto = this.value.toLowerCase();
        if (!texto) {
            renderTabla(usuarios);
            return;
        }
        const filtrados = usuarios.filter(u =>
            (u.nombre || '').toLowerCase().includes(texto) ||
            (u.nomUsu || '').toLowerCase().includes(texto) ||
            (u.rol || '').toLowerCase().includes(texto)
        );
        renderTabla(filtrados);
    });

    // Botón para limpiar filtro-input
    const filtroClearBtn = container.querySelector('#filtro-clear-btn');
    if (filtroClearBtn) {
        filtroClearBtn.addEventListener('click', function() {
            filtroInput.value = '';
            renderTabla(usuarios);
        });
    }
    }


async function cargarUsuarios() {
    try {
        const resp = await fetch('/.netlify/functions/get-usuarios', { headers: getAuthHeaders() });
        if (!resp.ok) throw new Error();
        let data = await resp.json();

        // --- ORDEN ALFABÉTICO POR NOMBRE ---
        usuarios = data.sort((a, b) => {
            const nomA = (a.nombre || "").toLowerCase();
            const nomB = (b.nombre || "").toLowerCase();
            return nomA.localeCompare(nomB);
        });

        renderTabla();
    } catch (e) {
        mostrarToast('Error al cargar la lista de usuarios', 'error');
    }
}

function renderTabla() {
    const tbody = document.getElementById('usuarios-tbody');
    if (!tbody) return;

    // Permitir recibir lista filtrada
    let lista = Array.isArray(arguments[0]) ? arguments[0] : usuarios;

    if (!lista.length) {
        tbody.innerHTML = '<tr><td colspan="5" style="text-align:center; color:#888; padding:30px;">No hay usuarios registrados.</td></tr>';
        return;
    }

    tbody.innerHTML = lista.map((u) => {
        const activo = u.activo !== false;
        const idUsuario = u.nomUsu || '';
        return `
            <tr style="${activo ? '' : 'opacity:0.55;'}">
                <td>${u.nombre || '-'} </td>
                <td style="font-family:monospace; font-weight:bold; color:var(--primary);">${idUsuario || '-'}</td>
                <td style="text-transform:capitalize; font-size:0.9rem;">${u.rol || '-'}</td>
                <td>${activo ? '✅ <span style="color:#27ae60;">Activo</span>' : '🚫 <span style="color:#e74c3c;">Inactivo</span>'}</td>
                <td>
                    <button data-edit="${idUsuario}" title="Editar" style="background:var(--primary);color:white;margin-right:4px;border:none;padding:5px 8px;border-radius:4px;cursor:pointer;">✏️</button>
                    ${activo 
                        ? `<button data-baja="${idUsuario}" title="Inactivar" style="background:#e74c3c;color:white;border:none;padding:5px 8px;border-radius:4px;cursor:pointer;">🗑️</button>`
                        : `<button data-alta="${idUsuario}" title="Reactivar" style="background:#27ae60;color:white;border:none;padding:5px 8px;border-radius:4px;cursor:pointer;">🔄</button>`
                    }
                </td>
            </tr>
        `;
    }).join('');

    tbody.querySelectorAll('[data-edit]').forEach(btn => btn.onclick = () => abrirModal(btn.dataset.edit));
    tbody.querySelectorAll('[data-baja]').forEach(btn => btn.onclick = () => confirmarBaja(btn.dataset.baja));
    tbody.querySelectorAll('[data-alta]').forEach(btn => btn.onclick = () => reactivarUsuario(btn.dataset.alta));
}

function buscarIndiceUsuario(nomUsu) {
    if (!nomUsu) return -1;
    return usuarios.findIndex(u => (u.nomUsu || '').trim() === nomUsu.trim());
}

function abrirModal(idOrNomUsu) {
    const idx = buscarIndiceUsuario(idOrNomUsu);
    const esEdicion = idx >= 0;
    const u = esEdicion ? usuarios[idx] : {};
    
    const modal = document.createElement('div');
    modal.className = 'modal';
    
    const espaciosCantidad = typeof APP_CONFIG.espacios === 'object' ? APP_CONFIG.espacios.items.length - 1 : 5;
    const espaciosEtiqueta = APP_CONFIG.espacios?.etiquetaGeneral || 'Consultorio';

    modal.innerHTML = `
        <div class="modal-content" style="max-width:480px; width:95%; background:white; padding:24px; border-radius:12px; margin: 40px auto; box-shadow: 0 8px 30px rgba(0,0,0,0.3); font-family: sans-serif;">
            <h3 style="margin-top:0; border-bottom:1px solid #eee; padding-bottom:12px;">${esEdicion ? 'Editar Usuario' : 'Nuevo Usuario'}</h3>

            <form id="form-usuario-modal">
                <div style="margin-bottom:12px;">
                    <label style="display:block; font-weight:bold; font-size:0.85rem; margin-bottom:4px;">Nombre completo *</label>
                    <input type="text" id="abm-nombre" value="${u.nombre || ''}" placeholder="Ej: Juan Pérez" required style="width:100%; padding:10px; border:1px solid #ccc; border-radius:6px; box-sizing: border-box;">
                </div>

                <div style="margin-bottom:15px;">
                    <label style="display:block; font-weight:bold; font-size:0.85rem; margin-bottom:4px;">Nombre de usuario (ID) *</label>
                    <input type="text" id="abm-nomusu" value="${u.nomUsu || ''}" required ${esEdicion ? 'readonly' : ''} style="width:100%; padding:10px; border:1px solid #ccc; border-radius:6px; background:#f8f9fa; box-sizing: border-box; ${esEdicion ? 'opacity:0.65; cursor:not-allowed;' : ''}">
                </div>

                <details style="margin-bottom:15px; border:1px solid #eee; border-radius:8px; background:#fafafa;" ${esEdicion ? 'open' : ''}>
                    <summary style="padding:10px; cursor:pointer; font-size:0.85rem; font-weight:bold; color:var(--primary);">⚙️ Datos adicionales y Rol</summary>
                    <div style="padding:12px; border-top:1px solid #eee; background:white;">
                        <div style="margin-bottom:10px;">
                            <label style="display:block; font-size:0.8rem; margin-bottom:4px;">Email (Opcional)</label>
                            <input type="email" id="abm-email" value="${u.email || ''}" style="width:100%; padding:8px; border:1px solid #ddd; border-radius:4px; box-sizing: border-box;">
                        </div>
                        <div style="display:flex; gap:10px; margin-bottom:10px;">
                            <div style="flex:1;">
                                <label style="display:block; font-size:0.8rem; margin-bottom:4px;">Doc. Tipo</label>
                                <select id="abm-tipdocu" style="width:100%; padding:8px; border:1px solid #ddd; border-radius:4px;">
                                    ${ (Array.isArray(APP_CONFIG.tiposDocumento) ? APP_CONFIG.tiposDocumento : ['PROF','DNI','CI']).map((tipo, index) => `
                                        <option value="${tipo}" ${u.tipdocu===tipo || (!esEdicion && !u.tipdocu && index===0) ? 'selected' : ''}>${tipo}</option>
                                    `).join('') }
                                </select>
                            </div>
                            <div style="flex:2;">
                                <label style="display:block; font-size:0.8rem; margin-bottom:4px;">Número</label>
                                <input type="text" id="abm-documento" value="${u.documento || ''}" style="width:100%; padding:8px; border:1px solid #ddd; border-radius:4px; box-sizing: border-box;">
                            </div>
                        </div>
                        <div>
                            <label style="display:block; font-size:0.8rem; margin-bottom:4px;">Rol del sistema</label>
                            <select id="abm-rol" style="width:100%; padding:8px; border:1px solid #ddd; border-radius:4px;">
                                ${(APP_CONFIG.roles || ['admin','gestor','usuario']).map(r => 
                                    `<option value="${r}" ${u.rol===r || (!esEdicion && r==='usuario') ? 'selected' : ''}>${r}</option>`
                                ).join('')}
                            </select>
                        </div>
                    </div>
                </details>

                <div style="margin-bottom:20px;">
                    <label style="display:block; font-weight:bold; font-size:0.85rem; margin-bottom:8px;">Espacios permitidos</label>
                    <div style="display:flex; flex-wrap:wrap; gap:8px;">
                        ${Array.from({length: espaciosCantidad}, (_, i) => `
                            <label style="font-size:0.75rem; background:#eee; padding:4px 10px; border-radius:15px; cursor:pointer; display:flex; align-items:center; gap:4px;">
                                <input type="checkbox" class="abm-consPref" value="${i+1}" ${(u.consPref||[]).includes(i+1)?'checked':''}>
                                ${espaciosEtiqueta} ${i+1}
                            </label>
                        `).join('')}
                    </div>
                </div>

                ${esEdicion ? `
                <div style="margin-bottom:20px;">
                    <label style="display:block; font-weight:bold; font-size:0.85rem; margin-bottom:4px;">Nueva Contraseña</label>
                    <input type="password" id="abm-password" placeholder="Vacío = No cambiar" style="width:100%; padding:10px; border:1px solid #ccc; border-radius:6px; box-sizing: border-box;">
                </div>` : `
                <div style="background:#f1f3f5; padding:12px; border-radius:8px; font-size:0.75rem; color:#495057; margin-bottom:20px; border: 1px dashed #ced4da;">
                    🔑 Contraseña inicial: <strong>IDUsuario + 1234567</strong>
                </div>`}

                <div style="display:flex; gap:10px; margin-top:10px;">
                    <button type="submit" id="btn-guardar" style="flex:2; background:var(--primary); color:white; border:none; padding:14px; border-radius:8px; cursor:pointer; font-weight:bold;">💾 Guardar</button>
                    <button type="button" id="btn-cancelar" style="flex:1; background:#f8f9fa; border:1px solid #ddd; padding:14px; border-radius:8px; cursor:pointer;">Cancelar</button>
                </div>
            </form>
        </div>
    `;

    document.body.appendChild(modal);

    const inputNombre = modal.querySelector('#abm-nombre');
    const inputNomUsu = modal.querySelector('#abm-nomusu');
    const inputEmail = modal.querySelector('#abm-email');
    const inputDocumento = modal.querySelector('#abm-documento');

    // Guardamos si el usuario modificó manualmente el email para no pisar cambios
    let emailAutoMode = true;

    const generarEmail = (usuarioId) => {
        const dominio = (APP_CONFIG && APP_CONFIG.usuarioEmailDominio) ? APP_CONFIG.usuarioEmailDominio : '@espacia.com.uy';
        return usuarioId ? `${usuarioId.toLowerCase()}${dominio}` : '';
    };

    const actualizarEmailSegunNomUsu = () => {
        const nomUsuVal = inputNomUsu.value.trim();
        if (emailAutoMode) {
            inputEmail.value = generarEmail(nomUsuVal);
        }
    };

    if (!esEdicion) {
        // Sugiere documento no repetido
        // Normaliza los documentos numéricos para que "00000001" y "1" se traten igual.
        const numerosExistentes = new Set(usuarios
            .map(u => parseInt((u.documento || '').toString().trim(), 10))
            .filter(n => Number.isInteger(n) && n >= 0)
        );
        let candidate = 1;
        while (numerosExistentes.has(candidate)) {
            candidate++;
        }
        if (!u.documento) {
            inputDocumento.value = String(candidate);
        }

        // Si existe nomUsu inicial, auto-cargar email
        if (inputNomUsu.value.trim()) {
            inputEmail.value = generarEmail(inputNomUsu.value.trim());
        }

        inputEmail.addEventListener('input', () => {
            const manual = inputEmail.value.trim().toLowerCase();
            emailAutoMode = manual !== generarEmail(inputNomUsu.value.trim()) && manual !== '';
        });

        inputNomUsu.addEventListener('input', actualizarEmailSegunNomUsu);

        inputNombre.addEventListener('input', () => {
            const raw = inputNombre.value.trim()
                .normalize("NFD").replace(/[\u0300-\u036f]/g, "");
            
            const partes = raw.split(/\s+/).filter(p => p.length > 0);
            
            if (partes.length > 0) {
                const primero = partes[0].charAt(0).toUpperCase() + partes[0].slice(1).toLowerCase();
                const iniciales = partes.slice(1).map(p => p.charAt(0).toUpperCase()).join('');
                inputNomUsu.value = (primero + iniciales).replace(/[^a-zA-Z0-9]/g, '');
            } else {
                inputNomUsu.value = '';
            }
            actualizarEmailSegunNomUsu();
        });
    }

    modal.querySelector('#btn-cancelar').onclick = () => modal.remove();
    
    modal.querySelector('#form-usuario-modal').onsubmit = async (e) => {
        e.preventDefault();
        const btn = modal.querySelector('#btn-guardar');
        btn.disabled = true;
        btn.textContent = 'Guardando...';

        const datos = {
            nombre: inputNombre.value.trim(),
            nomUsu: inputNomUsu.value.trim(),
            email: modal.querySelector('#abm-email').value.trim(),
            tipdocu: modal.querySelector('#abm-tipdocu').value,
            documento: modal.querySelector('#abm-documento').value.trim(),
            rol: modal.querySelector('#abm-rol').value,
            consPref: Array.from(modal.querySelectorAll('.abm-consPref:checked')).map(cb => parseInt(cb.value)),
            activo: esEdicion ? usuarios[idx].activo : true
        };

        // Validar nomUsu único (insensible a mayúsculas/minúsculas)
        const nomUsuToCheck = (datos.nomUsu || '').trim().toLowerCase();
        if (!nomUsuToCheck) {
            mostrarToast('El ID de usuario (nomUsu) no puede estar vacío.', 'warning');
            btn.disabled = false;
            btn.textContent = '💾 Guardar';
            return;
        }

        // En edición no validamos contra el propio registro y no permite cambiar el nomUsu.
        if (!esEdicion) {
            const existeNomUsu = usuarios.some((u) => {
                if (!u.nomUsu) return false;
                return u.nomUsu.trim().toLowerCase() === nomUsuToCheck;
            });
            if (existeNomUsu) {
                mostrarToast('Ya existe un usuario con ese ID. Por favor elija otro.', 'warning');
                btn.disabled = false;
                btn.textContent = '💾 Guardar';
                return;
            }
        }

        if (esEdicion) {
            const newPass = modal.querySelector('#abm-password').value;
            if (newPass) datos.contrasena = await hashPassword(newPass);
            else datos.contrasena = usuarios[idx].contrasena;
            usuarios[idx] = { ...usuarios[idx], ...datos };
        } else {
            datos.contrasena = await hashPassword(datos.nomUsu + '1234567');
            datos.requiereCambioPass = true;
            usuarios.push(datos);
        }

        await guardarBackend(modal);
    };
}

async function guardarBackend(modal) {
    try {
        const resp = await fetch('/.netlify/functions/update-usuarios', {
            method: 'POST',
            headers: getAuthHeaders(),
            body: JSON.stringify({ data: usuarios })
        });
        if (resp.ok) {
            mostrarToast('Éxito: Lista de usuarios actualizada', 'success');
            if (modal) modal.remove();
            await cargarUsuarios(); // Esto vuelve a ordenar y renderizar
        } else {
            const errorBody = await resp.json().catch(() => ({}));
            const errorMessage = errorBody.error || 'Error al guardar en el servidor';
            throw new Error(errorMessage);
        }
    } catch (e) {
        mostrarToast(`Error al guardar en el servidor: ${e.message}`, 'error');
        const btn = document.querySelector('#btn-guardar');
        if (btn) { btn.disabled = false; btn.textContent = '💾 Guardar'; }
    }
}

function confirmarBaja(nomUsu) {
    const idx = buscarIndiceUsuario(nomUsu);
    if (idx < 0) {
        mostrarToast('No se encontró el usuario para dar de baja.', 'error');
        return;
    }
    mostrarModalConfirmacion(
        'Inactivar Usuario',
        `<p>¿Deseas dar de baja a <strong>${usuarios[idx].nombre}</strong>?</p><p style="font-size:0.8rem; color:#666;">El usuario no podrá iniciar sesión.</p>`,
        'Inactivar',
        async (b, c, m) => {
            usuarios[idx].activo = false;
            await guardarBackend(m);
        },
        'Cancelar',
        '#e74c3c'
    );
}

async function reactivarUsuario(nomUsu) {
    const idx = buscarIndiceUsuario(nomUsu);
    if (idx < 0) {
        mostrarToast('No se encontró el usuario para reactivar.', 'error');
        return;
    }
    usuarios[idx].activo = true;
    await guardarBackend(null);
}

```

### Archivo: `js/reservas_futuras.js`
```javascript
import { APP_CONFIG } from './config.js';
import { getAuthHeaders, mostrarToast, mostrarModalConfirmacion, createComboUsuarios } from './utils.js';

export async function renderMisReservasFuturas(container) {
    // 1. Cargar el HTML externo
    container.innerHTML = `
        <div class="informe-container">
            <div style="margin-bottom: 20px;">
                <h2 class="module-title" id="vista-titulo">
                </h2>
            </div>

            <div id="admin-filter-container" style="display: none; margin-bottom:1.5em; background: var(--white, white); padding: 15px; border-radius: 8px; box-shadow: var(--shadow-card, 0 1px 3px rgba(0,0,0,0.1));">
                <label style="font-weight:600; color:#2c3e50;">Filtrar por Usuario: </label>
                <input id="input-filtro-usuario" type="text" placeholder="Escriba apellido o nombre..." 
                       style="margin-left:10px; padding:6px; border:1px solid #ced4da; border-radius:4px; width:200px;">
                
                <select id="combo-usuario-futuras" style="padding: 8px; border: 1px solid #ced4da; border-radius: 4px; margin-left: 10px; margin-top:8px;">
                    <option value="">Todos</option>
                </select>
            </div>

            <div id="total-horas-informe"></div>
            
            <div id="contenedor-resultados">
                <p>Cargando interfaz...</p>
            </div>
        </div>
    `;

    let user = JSON.parse(localStorage.getItem('usuarioActual')) || JSON.parse(sessionStorage.getItem('usuarioActual'));
    if (!user) return;

    let esAdmin = false;
    let usuariosLista = [];
    let reservasActivas = []; // Accesible desde manejarCancelacion

    // 2. Obtener lista de usuarios para el combo (Solo si es admin)
    try {
        const resp = await fetch('/.netlify/functions/listar_usuarios', {
            headers: getAuthHeaders()
        });
        const js = await resp.json();
        if (Array.isArray(js.usuarios)) {
            const actual = js.usuarios.find(u => u.email === user.email);
            if (actual && (actual.rol === 'admin' || actual.rol === 'gestor')) esAdmin = true;
            usuariosLista = js.usuarios.filter(u => u.rol === 'usuario').sort((a, b) => (a.nombre || '').localeCompare(b.nombre || ''));
        }
    } catch (e) { }

    // 3. Configurar Título y Filtros
    const tituloEl = document.getElementById('vista-titulo');
    const adminFilter = document.getElementById('admin-filter-container');
    const combo = document.getElementById('combo-usuario-futuras');
    
    tituloEl.innerText = esAdmin ? 'Reservas' : 'Mis Reservas';

    if (esAdmin) {
        adminFilter.style.display = 'block';
        // Usar createComboUsuarios para generar el combo
        const comboNuevo = createComboUsuarios(usuariosLista, {
            id: 'combo-usuario-futuras',
            name: 'usuario-futuras',
            clase: '',
            includeTodos: true,
            valueKey: 'email',
            textKey: 'nombre'
        });
        comboNuevo.style.padding = '8px';
        comboNuevo.style.border = '1px solid #ced4da';
        comboNuevo.style.borderRadius = '4px';
        comboNuevo.style.marginLeft = '10px';
        comboNuevo.style.marginTop = '8px';
        // Reemplazar el combo original
        combo.replaceWith(comboNuevo);
        comboNuevo.addEventListener('change', (e) => cargarReservas(e.target.value));
        // Lógica de búsqueda en el input
        const inputFiltro = document.getElementById('input-filtro-usuario');
        inputFiltro.addEventListener('input', function() {
            const texto = this.value.toLowerCase();
            comboNuevo.innerHTML = '';
            const optTodos = document.createElement('option');
            optTodos.value = '';
            optTodos.textContent = 'Todos';
            comboNuevo.appendChild(optTodos);
            usuariosLista.forEach(u => {
                if ((u.nombre || '').toLowerCase().includes(texto) || (u.email || '').toLowerCase().includes(texto)) {
                    const opt = document.createElement('option');
                    opt.value = u.email;
                    opt.textContent = u.nombre || u.email;
                    comboNuevo.appendChild(opt);
                }
            });
        });
    }

    // --- FUNCIÓN INTERNA: renderReservasTable (Corrigiendo el error de "not defined") ---
    function renderReservasTable(reservas, containerDestino) {
        // Filtrar reservas canceladas y bloqueadas
        reservasActivas = reservas.filter(r => {
            const summary = r.summary ? r.summary.toLowerCase() : '';
            return !summary.includes('cancelada') && !summary.includes('bloqueada');
        });
        const pageSize = APP_CONFIG.pageSize || 20;
        let currentPage = 1;
        const totalPages = Math.ceil(reservasActivas.length / pageSize) || 1;

        function renderPage(page) {
            const mostrarColUsuario = esAdmin;
            let html = `<div class="table-main-container"><table class="custom-table">
                <thead><tr>
                    <th style="width: 40px;">#</th>
                    <th style="width: 90px;">Fecha</th>
                    <th style="width: 130px;">Hora (inicio - fin)</th>
                    <th style="width: 100px;">${APP_CONFIG.espacios?.etiquetaGeneral || 'Consultorio'}</th>
                    ${mostrarColUsuario ? '<th>Usuario</th>' : ''}
                    <th style="width: 90px;">Estado</th>
                    <th style="width: 110px;">Acciones</th>
                </tr></thead>
                <tbody>`;
            
            const start = (page - 1) * pageSize;
            const end = Math.min(start + pageSize, reservasActivas.length);

            for (let idx = start; idx < end; idx++) {
                const r = reservasActivas[idx];
                const d = new Date(r.start);
                const fecha = d.toLocaleDateString('es-ES');
                const startDate = d;
                const endDate = r.end ? new Date(r.end) : null;
                const horaInicio = startDate.toLocaleTimeString('es-ES', { hour: '2-digit', minute: '2-digit' });
                const horaFin = endDate ? endDate.toLocaleTimeString('es-ES', { hour: '2-digit', minute: '2-digit' }) : '';
                let hora = horaFin ? `${horaInicio} - ${horaFin}` : horaInicio;
                if (endDate) {
                    const diffMin = Math.round((endDate - startDate) / (1000 * 60));
                    hora += ` (${diffMin} min)`;
                }
                const match = r.summary ? r.summary.match(/^C(\d+):/) : null;
                const consultorio = match ? `C${match[1]}` : 'N/A';
                let usuario = 'Desconocido';
                if (r.description) {
                    // Preferir el nombre de la persona para quien se reservó (formato: "Reserva para: Nombre <email>")
                    let m = r.description.match(/Reserva para:\s*(.+?)\s*<([^>]+)>/);
                    if (m) {
                        usuario = m[1];
                    } else {
                        // Si no está, fallback a quien realizó la reserva (admin)
                        m = r.description.match(/Reserva realizada por: (.+?) <([^>]+)>/);
                        if (m) usuario = m[1];
                    }
                }
                // Si todavía desconocido, intentar extraer del summary (ej. "C3: Juan Pérez")
                if ((!usuario || usuario === 'Desconocido') && r.summary) {
                    const parts = r.summary.split(':');
                    if (parts.length > 1) {
                        usuario = parts[1].trim();
                    }
                }
                const ahora = new Date();
                const diffHoras = (d - ahora) / (1000 * 60 * 60);
                const puedeCancelar = diffHoras > 24;
                
                // Extraer datos para el PDF
                let emailParaPDF = '';
                let creadoPorParaPDF = '';
                if (r.description) {
                    let mEmail = r.description.match(/<([^>]+)>/);
                    if (mEmail) emailParaPDF = mEmail[1];
                    
                    let mCreado = r.description.match(/Reserva realizada por: (.+?) </);
                    if (mCreado) creadoPorParaPDF = mCreado[1];
                }
                const intervaloParaPDF = endDate ? Math.round((endDate - startDate) / (1000 * 60)) : null;
                const fechaParaPDF = d.toISOString().split('T')[0]; // YYYY-MM-DD
                
                // Guardar datos en atributos data para el botón
                const datosPDF = JSON.stringify({
                    nombre: usuario,
                    fecha: fechaParaPDF,
                    hora: horaInicio,
                    horaFin: horaFin || '',
                    consultorio: match ? match[1] : '',
                    email: emailParaPDF,
                    intervalo: intervaloParaPDF,
                    creadoPor: creadoPorParaPDF,
                    fechaCreacion: r.created || null
                }).replace(/"/g, '&quot;');

                html += `<tr>
                    <td>${idx + 1}</td>
                    <td>${fecha}</td>
                    <td>${hora}</td>
                    <td>${consultorio}</td>
                    ${mostrarColUsuario ? `<td>${usuario}</td>` : ''}
                    <td><span class="status-badge reservada">Reservada</span></td>
                    <td style="display: flex; gap: 5px; justify-content: center;">
                        <button class="btn-descargar-pdf" data-pdf="${datosPDF}" title="Descargar PDF" style="background:#27ae60; color:white; border:none; padding:5px 10px; border-radius:4px; cursor:pointer; display:flex; align-items:center; justify-content:center;">
                            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path><polyline points="7 10 12 15 17 10"></polyline><line x1="12" y1="15" x2="12" y2="3"></line></svg>
                        </button>
                        ${puedeCancelar 
                            ? `<button class="btn-cancelar-reserva" data-id="${r.id}" style="background:#e74c3c; color:white; border:none; padding:5px 10px; border-radius:4px; cursor:pointer;">Cancelar</button>`
                            : `<button disabled style="background:#e0e0e0; color:#888; border:none; padding:5px 10px; border-radius:4px; cursor:not-allowed;">Cancelar</button>`
                        }
                    </td>
                </tr>`;
            }
            html += `</tbody></table></div>`;
            
            // Agregar controles de paginación si hay más de una página
            if (totalPages > 1) {
                html += `
                    <div class="pagination" style="margin:15px 0; text-align:center; display: flex; justify-content: center; align-items: center; gap: 15px;">
                        <button id="pag-prev-futuras" ${page === 1 ? 'disabled' : ''} style="padding: 8px 16px; border: 1px solid var(--border, #ccc); border-radius: 4px; background: ${page === 1 ? 'var(--bg-zebra, #f8f9fa)' : 'var(--white, white)'}; cursor: ${page === 1 ? 'not-allowed' : 'pointer'}; color: ${page === 1 ? 'var(--text-muted, #999)' : 'var(--text-main, #333)'};">&lt; Anterior</button>
                        <span style="font-weight: 500; color: var(--text-main, #555);">Página ${page} de ${totalPages}</span>
                        <button id="pag-next-futuras" ${page === totalPages ? 'disabled' : ''} style="padding: 8px 16px; border: 1px solid var(--border, #ccc); border-radius: 4px; background: ${page === totalPages ? 'var(--bg-zebra, #f8f9fa)' : 'var(--white, white)'}; cursor: ${page === totalPages ? 'not-allowed' : 'pointer'}; color: ${page === totalPages ? 'var(--text-muted, #999)' : 'var(--text-main, #333)'};">Siguiente &gt;</button>
                    </div>
                `;
            }

            containerDestino.innerHTML = html;
            
            // Eventos de botones de la tabla
            containerDestino.querySelectorAll('.btn-cancelar-reserva').forEach(btn => {
                btn.onclick = (e) => manejarCancelacion(e);
            });
            containerDestino.querySelectorAll('.btn-descargar-pdf').forEach(btn => {
                btn.onclick = async (e) => {
                    try {
                        const datosStr = e.currentTarget.getAttribute('data-pdf');
                        const datos = JSON.parse(datosStr);
                        const { generarTicketReserva } = await import('./pdf/pdf_ticket_reserva.js');
                        await generarTicketReserva({
                            usuario:           datos.nombre,
                            email:             datos.email,
                            fecha:             datos.fecha,
                            inicio:            datos.hora,
                            fin:               datos.horaFin || '',
                            intervalo:         datos.intervalo,
                            espacio:           datos.consultorio,
                            creadoPor:         datos.creadoPor || '',
                            realizadaPorAdmin: !!datos.creadoPor,
                            fechaCreacion:     datos.fechaCreacion || null
                        });
                    } catch (err) {
                        console.error("Error al descargar PDF:", err);
                        mostrarToast("Error al generar el PDF", "error");
                    }
                };
            });

            // Eventos de paginación
            const prevBtn = document.getElementById('pag-prev-futuras');
            const nextBtn = document.getElementById('pag-next-futuras');
            
            if (prevBtn) {
                prevBtn.onclick = () => {
                    if (currentPage > 1) {
                        currentPage--;
                        renderPage(currentPage);
                    }
                };
            }
            
            if (nextBtn) {
                nextBtn.onclick = () => {
                    if (currentPage < totalPages) {
                        currentPage++;
                        renderPage(currentPage);
                    }
                };
            }
        }
        renderPage(currentPage);
    }

    // --- FUNCIÓN INTERNA: manejarCancelacion ---
    async function manejarCancelacion(e) {
        const id = e.target.getAttribute('data-id');
        
        const contenido = `<p style="margin-bottom: 20px; color: #666;">¿Estás seguro de que deseas cancelar esta reserva?</p>`;

        mostrarModalConfirmacion(
            "Cancelar Reserva",
            contenido,
            "Sí, Cancelar",
            async (btnConfirmar, btnCancelar, modal) => {
                btnConfirmar.disabled = true;
                btnConfirmar.innerText = 'Cancelando...';

                try {
                    // nombreUsuario = quien ejecuta (el logueado), nunca el del combo
                    const nombreUsuario = user?.nombre || user?.email?.split('@')[0].replace(/\./g, ' ') || '';
                    const esAdminCancelador = (user?.rol === 'admin' || user?.rol === 'gestor');

                    const { Reserva } = await import('./modelo/reserva.js');
                    const reserva = new Reserva();
                    reserva.id           = id;
                    reserva.canceladoPor = nombreUsuario;
                    reserva.creadoPor    = esAdminCancelador ? nombreUsuario : null;

                    // Extraer datos de la reserva para el ticket
                    const r = reservasActivas.find(r => r.id === id);
                    if (r) {
                        const { extraerDatosDescription } = await import('./agenda/agenda_cancelar.js');
                        const datos = extraerDatosDescription(r.summary || '', r.description || '');
                        reserva.usuario      = datos.usuario   || nombreUsuario;
                        reserva.email        = datos.email;
                        reserva.fecha        = datos.fecha;
                        reserva.h_inicio     = datos.horaInicio;
                        reserva.h_fin        = datos.horaFin;
                        reserva.intervalo    = datos.intervalo;
                        reserva.lugar        = datos.espacio;
                        reserva.creadoPor    = datos.creadoPor || reserva.creadoPor;
                        reserva.fechaCreacion = r.created || null;
                    }

                    const resp = await reserva.cancelar();
                    if (resp.ok) {
                        mostrarToast('✅ Reserva cancelada', 'success');
                        try { await reserva.imprimirTicketCancelacion(); } catch(e) { console.error('Error ticket:', e); }
                        modal.remove();
                        cargarReservas(esAdmin ? combo.value : user.email);
                    } else {
                        const err = await resp.json();
                        mostrarToast('❌ Error: ' + (err.error || 'No se pudo cancelar'), 'error');
                        btnConfirmar.disabled = false;
                        btnConfirmar.innerText = 'Sí, Cancelar';
                    }
                } catch (err) {
                    mostrarToast('Error de conexión', 'error');
                    btnConfirmar.disabled = false;
                    btnConfirmar.innerText = 'Sí, Cancelar';
                }
            }
        );
    }

    // 4. Lógica de carga de datos
    const cargarReservas = async (emailFiltro) => {
        const resCont = document.getElementById('contenedor-resultados');
        resCont.innerHTML = '<p>Cargando datos...</p>';
        const hoy = new Date();
        const fechaInicio = hoy.toISOString().split('T')[0];
        const fechaFin = new Date(hoy.getTime() + 90 * 24 * 60 * 60 * 1000).toISOString().split('T')[0];
        let url = `/.netlify/functions/informe_reservas?fechaInicio=${fechaInicio}&fechaFin=${fechaFin}`;
        if (emailFiltro) url += `&usuario=${encodeURIComponent(emailFiltro)}`;
        try {
            const resp = await fetch(url, {
                headers: getAuthHeaders()
            });
            const data = await resp.json();
            let reservas = Array.isArray(data) ? data : (data.reservas || []);
            reservas = reservas.filter(r => r.start && new Date(r.start) > new Date());
            renderReservasTable(reservas, resCont);
        } catch (err) {
            resCont.innerHTML = `<p style=\"color:red\">Error: ${err.message}</p>`;
        }
    };

    // Carga inicial
    cargarReservas(esAdmin ? "" : user.email);
}
```

### Archivo: `js/utils.js`
```javascript
/**
 * Formatea una fecha a DD/MM/AA
 * @param {string|Date} fechaStr
 * @returns {string}
 */
export function parseFecha(fechaInput) {
    if (!fechaInput) return null;

    if (fechaInput instanceof Date) {
        if (isNaN(fechaInput)) return null;
        return fechaInput;
    }

    const value = String(fechaInput).trim();

    // DD/MM/YY o DD/MM/YYYY
    let match = value.match(/^([0-9]{1,2})\/([0-9]{1,2})\/(\d{2}|\d{4})$/);
    if (match) {
        let dia = parseInt(match[1], 10);
        let mes = parseInt(match[2], 10);
        let ano = match[3].length === 2 ? 2000 + parseInt(match[3], 10) : parseInt(match[3], 10);
        if (mes < 1 || mes > 12 || dia < 1 || dia > 31) return null;
        return new Date(ano, mes - 1, dia);
    }

    // YYYY-MM-DD o variantes con hora
    match = value.match(/^(\d{4})-(\d{2})-(\d{2})(?:[T\s](.+))?$/);
    if (match) {
        const year = parseInt(match[1], 10);
        const month = parseInt(match[2], 10);
        const day = parseInt(match[3], 10);
        if (month < 1 || month > 12 || day < 1 || day > 31) return null;
        if (match[4]) {
            // Con hora/timezone, puede ser Z o con offset
            const d = new Date(value);
            return isNaN(d) ? null : d;
        }
        // Sin hora, interpretar como fecha local para evitar desplazamientos UTC inesperados
        const d = new Date(year, month - 1, day);
        return isNaN(d) ? null : d;
    }

    // Intentamos parse genérico con Date (cuidado con locale), fallback de último recurso
    const d = new Date(value);
    return isNaN(d) ? null : d;
}

export function formatFechaCorta(fechaStr) {
    const fecha = parseFecha(fechaStr);
    if (!fecha) return '';
    return `${fecha.getDate().toString().padStart(2, '0')}/${(fecha.getMonth()+1).toString().padStart(2, '0')}/${fecha.getFullYear().toString().slice(-2)}`;
}

export function formatFechaISO(fechaStr) {
    const fecha = parseFecha(fechaStr);
    if (!fecha) return '';
    return `${fecha.getFullYear()}-${(fecha.getMonth()+1).toString().padStart(2,'0')}-${fecha.getDate().toString().padStart(2,'0')}`;
}

export function calcularHoraFin(hInicio, intervaloMinutos) {
    if (!hInicio || !intervaloMinutos) return '';
    const [h, m] = String(hInicio).split(':').map(Number);
    if (isNaN(h) || isNaN(m) || isNaN(intervaloMinutos)) return '';
    const f = new Date(2000, 0, 1, h, m);
    f.setMinutes(f.getMinutes() + Number(intervaloMinutos));
    return `${f.getHours().toString().padStart(2,'0')}:${f.getMinutes().toString().padStart(2,'0')}`;
}
import { APP_CONFIG } from './config.js';

// js/utils.js
/**
 * Crea un combo (select) de usuarios.
 * @param {Array} usuariosLista - Array de usuarios (con nombre y email).
 * @param {Object} opciones - Opciones opcionales: id, name, clase, includeTodos, valueKey, textKey.
 * @returns {HTMLSelectElement} El elemento select generado.
 */
export function createComboUsuarios(usuariosLista, opciones = {}) {
    const {
        id = '',
        name = '',
        clase = '',
        includeTodos = true,
        valueKey = 'email',
        textKey = 'nombre'
    } = opciones;
    const select = document.createElement('select');
    if (id) select.id = id;
    if (name) select.name = name;
    if (clase) select.className = clase;
    if (includeTodos) {
        const optTodos = document.createElement('option');
        optTodos.value = '';
        optTodos.textContent = 'Todos';
        select.appendChild(optTodos);
    }
    usuariosLista.forEach(u => {
        const opt = document.createElement('option');
        opt.value = u[valueKey] || '';
        opt.textContent = u[textKey] || u[valueKey] || '';
        select.appendChild(opt);
    });
    return select;
}
// Funciones de utilidad compartidas en el frontend

/**
 * Obtiene los headers de autorización con el JWT.
 * @returns {Object} Objeto con los headers.
 */
export function getAuthHeaders() {
    const token = localStorage.getItem('consAge_token') || sessionStorage.getItem('consAge_token');
    const headers = {
        'Content-Type': 'application/json'
    };
    if (token) {
        headers['Authorization'] = `Bearer ${token}`;
    }
    return headers;
}

/**
 * Genera un hash SHA-256 de una contraseña.
 * @param {string} pw - La contraseña en texto plano.
 * @returns {Promise<string>} El hash hexadecimal de la contraseña.
 */
export async function hashPassword(pw) {
    if (!pw) return '';
    const enc = new TextEncoder();
    const data = enc.encode(pw);
    const hashBuffer = await window.crypto.subtle.digest('SHA-256', data);
    return Array.from(new Uint8Array(hashBuffer))
        .map(b => b.toString(16).padStart(2, '0'))
        .join('');
}

/**
 * Muestra una notificación tipo toast en la pantalla
 * @param {string} mensaje - El texto a mostrar
 * @param {string} tipo - 'success', 'error', 'warning', o 'info'
 */
export function mostrarToast(mensaje, tipo = 'info') {
    let toastContainer = document.getElementById('toast-container');
    if (!toastContainer) {
        toastContainer = document.createElement('div');
        toastContainer.id = 'toast-container';
        toastContainer.style.cssText = `
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 10000;
            display: flex;
            flex-direction: column;
            gap: 10px;
        `;
        document.body.appendChild(toastContainer);
    }

    const toast = document.createElement('div');
    let bgColor = '#333';
    if (tipo === 'success') bgColor = '#2ecc71';
    if (tipo === 'error') bgColor = '#e74c3c';
    if (tipo === 'warning') bgColor = '#f39c12';

    toast.style.cssText = `
        background-color: ${bgColor};
        color: white;
        padding: 15px 25px;
        border-radius: 8px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        font-family: sans-serif;
        font-size: 14px;
        opacity: 0;
        transform: translateY(20px);
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        justify-content: space-between;
        min-width: 250px;
    `;

    toast.innerHTML = `
        <span>${mensaje}</span>
        <button style="background:none; border:none; color:white; cursor:pointer; font-size:16px; margin-left:15px;">&times;</button>
    `;

    toast.querySelector('button').onclick = () => {
        toast.style.opacity = '0';
        toast.style.transform = 'translateY(20px)';
        setTimeout(() => toast.remove(), 300);
    };

    toastContainer.appendChild(toast);

    setTimeout(() => {
        toast.style.opacity = '1';
        toast.style.transform = 'translateY(0)';
    }, 10);

    setTimeout(() => {
        if (toast.parentElement) {
            toast.style.opacity = '0';
            toast.style.transform = 'translateY(20px)';
            setTimeout(() => toast.remove(), 300);
        }
    }, window.APP_CONFIG?.toastDuracion || APP_CONFIG?.toastDuracion || 3000);
}

/**
 * Muestra un modal de confirmación genérico
 * @param {string} titulo - Título del modal
 * @param {string} HTMLcontenido - Contenido HTML del modal (ej. texto de la pregunta, inputs adicionales)
 * @param {string} textoConfirmar - Texto del botón de confirmación (ej. "Sí, Cancelar", "Aceptar")
 * @param {Function} onConfirm - Callback asíncrono o síncrono al hacer clic en confirmar que recibe (botonConfirmar, btnCancelar, modal)
 * @param {string} [textoCancelar="Volver"] - Texto del botón secundario/cancelar
 * @param {string} [colorConfirmar="#e74c3c"] - Color de fondo del botón principal 
 */
export function mostrarModalConfirmacion(titulo, HTMLcontenido, textoConfirmar, onConfirm, textoCancelar = "Volver", colorConfirmar = "#e74c3c") {
    const minWidth = window.innerWidth < 500 ? "95%" : "400px";

    const modalHtml = `
        <div id="modal-global-confirmacion" style="position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); display: flex; justify-content: center; align-items: center; z-index: 9999;">
            <div style="background: var(--white, white); color: var(--text-main, #333); padding: 25px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.2); max-width: 400px; width: ${minWidth}; text-align: center;">
                <h3 style="margin-top: 0; color: ${colorConfirmar};">${titulo}</h3>
                
                <div style="margin-bottom: 20px; color: #666; font-size: 1rem;">
                    ${HTMLcontenido}
                </div>
                
                <div style="display: flex; justify-content: space-between; gap: 10px;">
                    <button id="btn-global-confirmar" style="flex: 1; padding: 10px 20px; background: ${colorConfirmar}; color: white; border: none; border-radius: 8px; cursor: pointer; font-weight: bold;">${textoConfirmar}</button>
                    <button id="btn-global-cancelar" style="flex: 1; padding: 10px 20px; background: var(--bg-zebra, #f8f9fa); color: #333; border: 1px solid #ccc; border-radius: 8px; cursor: pointer; font-weight: bold;">${textoCancelar}</button>
                </div>
            </div>
        </div>
    `;

    document.body.insertAdjacentHTML('beforeend', modalHtml);
    const modal = document.getElementById('modal-global-confirmacion');
    const btnConfirmar = document.getElementById('btn-global-confirmar');
    const btnCancelar = document.getElementById('btn-global-cancelar');

    btnCancelar.onclick = () => {
        modal.remove();
    };

    btnConfirmar.onclick = async () => {
        await onConfirm(btnConfirmar, btnCancelar, modal);
    };
}

function _normalize(s){
  return (s||'').toString().normalize('NFD').replace(/[\u0300-\u036f]/g,'').toLowerCase().trim();
}

export function getRealizadoPor(reserva){
  if(!reserva) return '';
  const owner = (reserva.usuario || '').toString().trim();

  let creado = (reserva.creadoPor || '').toString().trim();
  if(!creado){
    const m = (reserva.description || '').toString().match(/Reserva realizada por:\s*([^\n<]+)/i);
    if(m) creado = m[1].trim();
  }
  if(creado){
    const m = creado.match(/^(.+?)(?:\s*<|$)/);
    const creadoName = m ? m[1].trim() : creado;
    if(owner && _normalize(owner) === _normalize(creadoName)) return owner;
    return 'Administración';
  }

  if(reserva.realizadaPorAdmin) return 'Administración';
  return owner || '';
}

export function getCanceladoPor(reserva) {
    if (!reserva) return '';

    // 1. Extraer datos básicos
    const estado = (reserva.estado || '').toString().toLowerCase();
    const summary = (reserva.summary || '').toString().toLowerCase();
    const description = (reserva.description || '').toString();

    

    // 3. Obtener el dueño
    let owner = (reserva.usuario || '').toString().trim();
    if (!owner) {
        const m = summary.match(/Reserva para:\s*([^\n<]+)/i) || description.match(/Reserva para:\s*([^\n<]+)/i);
        if (m) owner = m[1].trim();
    }

    // 4. Obtener quién canceló (buscando en ambos campos)
    let cancelRaw = (reserva.canceladoPor || '').toString().trim();
    if (!cancelRaw) {
        const m = description.match(/Reserva Cancelada por:\s*([^\n<]+)/i) || summary.match(/Reserva Cancelada por:\s*([^\n<]+)/i);
        if (m) cancelRaw = m[1].trim();
    }

    // 5. Lógica de comparación mejorada (igual que en getRealizadoPor)
    if (cancelRaw) {
        // Limpiamos posibles sufijos de fecha/hora añadidos al registro
        // Ej: "Mariana Roberto CLI el 29/3/2026, 8:03:59 p. m." -> "Mariana Roberto CLI"
        let cancelClean = cancelRaw.replace(/\s+el\s+\d{1,2}\/\d{1,2}\/\d{2,4}.*$/i, '').trim();
        // Limpiamos emails si existen: "Nombre <email@...>" -> "Nombre"
        const m = cancelClean.match(/^(.+?)(?:\s*<|$)/);
        const cancelName = m ? m[1].trim() : cancelClean;

        if (owner && _normalize(owner) === _normalize(cancelName)) {
            return owner;
        }
        // Mantener comportamiento previo: si quien canceló NO es el titular, mostrar 'Administración'
        return 'Administración';
    }

    // Si no hay datos de cancelación pero el evento está marcado como cancelado, devolver 'Administración'
    if (estado && estado.includes('cancel')) return 'Administración';

    // En cualquier otro caso no hay información de cancelación
    return '';
}
```

### Archivo: `manifest.json`
```json
{
  "short_name": "Espacia",
  "name": "Espacia",
  "icons": [
    {
      "src": "imagenes/icon-192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "imagenes/icon-512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": "/",
  "background_color": "#FFFFFF",
  "display": "standalone",
  "scope": "/",
  "theme_color": "#000000"
}

```

### Archivo: `netlify.toml`
```toml
[build]
  functions = "netlify/functions"

[functions]
  node_bundler = "esbuild"

```

### Archivo: `netlify/functions/backup-reservas.js`
```javascript
const { google } = require('googleapis');
const fetch = require('node-fetch');
const { schedule } = require('@netlify/functions');

const backupHandler = async (event, context) => {
    // Proteger el endpoint (opcional, podrías requerir un token o una cabecera de seguridad si la llamas mediante un Cron)
    // El método suele ser POST si lo llamas de forma segura, o puede ejecutarse mediante Scheduled Functions de Netlify

    try {
        let privateKey = process.env.GOOGLE_PRIVATE_KEY.replace(/\\n/g, '\n');
        if (privateKey.startsWith('"') && privateKey.endsWith('"')) {
            privateKey = privateKey.substring(1, privateKey.length - 1);
        }

        const subject = process.env.GOOGLE_IMPERSONATE_EMAIL || null;
        const auth = new google.auth.JWT(
            process.env.GOOGLE_SERVICE_ACCOUNT_EMAIL,
            null,
            privateKey,
            ['https://www.googleapis.com/auth/calendar'],
            subject
        );

        const calendar = google.calendar({ version: 'v3', auth });
        const calendarId = process.env.CALENDAR_ID;
        if (!calendarId) {
            return { statusCode: 500, body: JSON.stringify({ error: 'Falta configurar CALENDAR_ID en Netlify.' }) };
        }

        // Definir la zona horaria a utilizar (si no está definida en Netlify, forzamos Montevideo)
        const zonaHoraria = process.env.TIMEZONE || 'America/Montevideo';

        // Obtener la fecha y hora de AHORA pero ajustada a Montevideo
        const stringAhoraMVD = new Date().toLocaleString("en-US", { timeZone: zonaHoraria });
        const hoyMVD = new Date(stringAhoraMVD);
        
        // Calcular "ayer" con respecto a esa hora exacta
        const ayerMVD = new Date(hoyMVD);
        ayerMVD.setDate(ayerMVD.getDate() - 1);
        
        // Empieza a las 00:00:00 del día de ayer en Montevideo
        const timeMin = new Date(ayerMVD);
        timeMin.setHours(0, 0, 0, 0);

        // Termina a las 23:59:59.999 del día de ayer en Montevideo
        const timeMax = new Date(ayerMVD);
        timeMax.setHours(23, 59, 59, 999);

        // Construir la cadena en formato ISO con el offset para que Google Calendar entienda el rango
        // En Montevideo el offset es -03:00 todo el año
        const timeMinStr = timeMin.toISOString().replace('Z', '') + '-03:00';
        const timeMaxStr = timeMax.toISOString().replace('Z', '') + '-03:00';

        // Consultar a Google Calendar por las reservas
        const resList = await calendar.events.list({
            calendarId,
            timeMin: timeMinStr,
            timeMax: timeMaxStr,
            maxResults: 2500,
            singleEvents: true,
            orderBy: 'startTime',
            timeZone: zonaHoraria,
        });

        const eventosAyer = resList.data.items || [];

        // Opcionalmente, filtrar solo los útiles
        const reservas = eventosAyer.map(event => {
            let descriptionLimpia = event.description || "";
            // Si la descripción está definida, intentamos quitar escapes literales
            if (descriptionLimpia) {
                // Reemplazamos la etiqueta de Google si viene en HTML extra o solo dejamos el texto limpio
                // Reemplazamos los saltos de línea crudos y repetidos por un formato que facilite la lectura del array
                descriptionLimpia = descriptionLimpia.replace(/\\n/g, '\n').split('\n').filter(l => l.trim() !== "");
            }

            return {
                id: event.id,
                summary: event.summary,
                description: descriptionLimpia,  // Ahora será un array de líneas, mucho más legible en un JSON
                inicio: event.start.dateTime || event.start.date,
                fin: event.end.dateTime || event.end.date,
                estado: (event.summary && event.summary.startsWith('Cancelada')) ? 'cancelada' : 'efectuada'
            };
        });

        // Configuración para guardar el backup en GitHub en el repositorio BD
        const githubToken = process.env.GITHUB_TOKEN;
        const repo = "FABIOR1981/bd"; // Ajusta al nombre real si es diferente
        const branch = "main"; 
        
        // Nombre del archivo de respaldo, por ejemplo: reservas-2026-02-27.json
        // Extraer formato AAAA-MM-DD local a Montevideo, para evitar corrimientos por Z
        const yyyy = ayerMVD.getFullYear();
        const mm = String(ayerMVD.getMonth() + 1).padStart(2, '0');
        const dd = String(ayerMVD.getDate()).padStart(2, '0');
        const fechaStr = `${yyyy}-${mm}-${dd}`;
        
        const filePath = `espacia/backups/reservas-${fechaStr}.json`;

        if (!githubToken) {
            console.error("No GITHUB_TOKEN configured.");
            return { statusCode: 500, body: "Error interno: Faltan credenciales" };
        }

        const fileUrl = `https://api.github.com/repos/${repo}/contents/${filePath}?ref=${branch}`;

        // Preparar el contenido
        const contentJson = JSON.stringify({
            fechaDelBackup: new Date().toISOString(),
            fechaReservas: fechaStr,
            total: reservas.length,
            reservas: reservas
        }, null, 2);

        const encodedContent = Buffer.from(contentJson).toString('base64');

        // Subir a GitHub
        const updateResponse = await fetch(fileUrl, {
            method: 'PUT',
            headers: {
                Authorization: `token ${githubToken}`,
                Accept: 'application/vnd.github.v3+json',
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                message: `Automated backup: Reservas del ${fechaStr}`,
                content: encodedContent,
                branch: branch,
            }),
        });

        if (!updateResponse.ok) {
            const errorText = await updateResponse.text();
            throw new Error(`GitHub Api Error: ${errorText}`);
        }

        return {
            statusCode: 200,
            body: JSON.stringify({ 
                success: true, 
                message: `Backup de las reservas del ${fechaStr} creado exitosamente con ${reservas.length} eventos.` 
            })
        };

    } catch (error) {
        console.error("Error realizando el backup de reservas:", error);
        return {
            statusCode: 500,
            body: JSON.stringify({ error: "No se pudo realizar el backup", details: error.message })
        };
    }
};

// Se ejecuta automáticamente todos los días a las 03:00 AM (Hora configurada por el servidor, expresado en cron pattern)
// "0 3 * * *" = A las 03:00 am todos los días 
exports.handler = schedule("0 3 * * *", backupHandler);

```

### Archivo: `netlify/functions/change-password.js`
```javascript
const fetch = require('node-fetch');
const { verifyToken } = require('./utils/jwthelper');
const config = require('./utils/config_netlify');

exports.handler = async (event, context) => {
  // Solo permitir POST
  if (event.httpMethod !== 'POST') {
    return {
      statusCode: 405,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Método no permitido' }),
    };
  }

  // Manejar CORS preflight
  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization',
        'Access-Control-Allow-Methods': 'POST, OPTIONS',
      },
      body: '',
    };
  }

  const user = verifyToken(event);
  if (!user) {
    return {
      statusCode: 401,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'No autorizado. Token inválido o ausente.' }),
    };
  }

  try {
    const { nuevaContrasenaHash } = JSON.parse(event.body);
    if (!nuevaContrasenaHash) {
      return {
        statusCode: 400,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'La nueva contraseña es requerida' }),
      };
    }

    const githubToken = process.env.GITHUB_TOKEN;
    const repo = config.github.repo;
    const branch = config.github.branch;
    const filePath = config.github.dbPath;

    if (!githubToken) {
      return {
        statusCode: 401,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Token de GitHub no configurado' }),
      };
    }

    // 1. Obtener el archivo actual
    const fileUrl = `https://api.github.com/repos/${repo}/contents/${filePath}?ref=${branch}`;
    const fileResponse = await fetch(fileUrl, {
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3+json',
      },
    });

    if (!fileResponse.ok) {
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al obtener la base de usuarios' }),
      };
    }

    const fileData = await fileResponse.json();
    const currentSha = fileData.sha;
    
    // Decodificar el contenido actual
    let currentUsers = {};
    try {
      const currentContent = Buffer.from(fileData.content, 'base64').toString('utf8');
      currentUsers = JSON.parse(currentContent);
    } catch (e) {
      console.error("Error parseando usuarios.json", e);
      return { statusCode: 500, body: JSON.stringify({ error: 'Error al parsear usuarios.json' }) };
    }

    // 2. Actualizar la contraseña del usuario actual
    const userKey = `${user.tipdocu}-${user.documento}`;
    
    if (!currentUsers[userKey]) {
      return {
        statusCode: 404,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Usuario no encontrado en la base de datos' }),
      };
    }

    // Actualizar la contraseña y quitar el flag de requiereCambioPass
    currentUsers[userKey].contrasena = nuevaContrasenaHash;
    currentUsers[userKey].requiereCambioPass = false;

    // 3. Preparar el nuevo contenido
    const newContent = JSON.stringify(currentUsers, null, 2).replace(
      /"consPref": \[\s+([\s\S]*?)\s+\]/g,
      (match, p1) => `"consPref": [${p1.replace(/\s+/g, '')}]`
    );
    const encodedContent = Buffer.from(newContent).toString('base64');
    
    // 4. Guardar archivo en GitHub
    const updateResponse = await fetch(fileUrl, {
      method: 'PUT',
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3+json',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        message: `Cambio de contraseña para: ${userKey}`,
        content: encodedContent,
        sha: currentSha,
        branch: branch,
      }),
    });

    if (!updateResponse.ok) {
      const errorText = await updateResponse.text();
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al actualizar la contraseña', details: errorText }),
      };
    }

    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ success: true, message: 'Contraseña actualizada' }),
    };

  } catch (error) {
    return {
      statusCode: 500,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Error interno del servidor', details: error.message }),
    };
  }
};

```

### Archivo: `netlify/functions/get-usuarios.js`
```javascript
const fetch = require('node-fetch');
const { verifyToken } = require('./utils/jwthelper');
const config = require('./utils/config_netlify');

exports.handler = async (event, context) => {
  // Manejar CORS preflight
  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization',
        'Access-Control-Allow-Methods': 'GET, OPTIONS',
      },
      body: '',
    };
  }

  if (event.httpMethod !== 'GET') {
    return {
      statusCode: 405,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Método no permitido' }),
    };
  }

  const user = verifyToken(event);
  if (!user) {
    return {
      statusCode: 401,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'No autorizado. Token inválido o ausente.' }),
    };
  }

  // VALIDACIÓN DE SEGURIDAD: Solo admin o gestor pueden descargar el listado completo
  if (user.rol !== 'admin' && user.rol !== 'gestor') {
    return {
      statusCode: 403,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Permisos insuficientes. Solo administradores pueden consultar usuarios.' }),
    };
  }

  try {
    // Configuración del repositorio BD
    const githubToken = process.env.GITHUB_TOKEN;
    const repo = config.github.repo;
    const branch = config.github.branch;
    const filePath = config.github.dbPath;

    if (!githubToken) {
      return {
        statusCode: 401,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Token de GitHub no configurado' }),
      };
    }

    const fileUrl = `https://api.github.com/repos/${repo}/contents/${filePath}?ref=${branch}`;
    
    const fileResponse = await fetch(fileUrl, {
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3.raw', // Importante: .raw para obtener el contenido directamente
      },
    });

    if (!fileResponse.ok) {
      if (fileResponse.status === 404) {
         // Si el archivo no existe aún, devolvemos un array vacío
         return {
            statusCode: 200,
            headers: {
              'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
              'Content-Type': 'application/json',
            },
            body: JSON.stringify([]),
          };
      }
      const error = await fileResponse.text();
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al obtener el archivo de GitHub', details: error }),
      };
    }

    const fileData = await fileResponse.text();
    
    // Parsear el JSON y eliminar las contraseñas antes de enviarlo al cliente
    let usuarios = [];
    try {
      const usuariosDict = JSON.parse(fileData);
      usuarios = Object.values(usuariosDict).map(u => {
        const { contrasena, ...userSinPass } = u;
        return userSinPass;
      });
    } catch (e) {
      console.error("Error parseando usuarios.json", e);
    }

    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(usuarios),
    };

  } catch (error) {
    return {
      statusCode: 500,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Error interno del servidor', details: error.message }),
    };
  }
};

```

### Archivo: `netlify/functions/informe_canceladas.js`
```javascript
const { google } = require('googleapis');
const { verifyToken } = require('./utils/jwthelper');

exports.handler = async (event, context) => {
    // Manejar CORS preflight
    if (event.httpMethod === 'OPTIONS') {
        return {
            statusCode: 200,
            headers: {
                'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
                'Access-Control-Allow-Headers': 'Content-Type, Authorization',
                'Access-Control-Allow-Methods': 'GET, OPTIONS',
            },
            body: '',
        };
    }

    if (event.httpMethod !== 'GET') {
        return { statusCode: 405, body: 'Method Not Allowed' };
    }

    const userToken = verifyToken(event);
    if (!userToken || (userToken.rol !== 'admin' && userToken.rol !== 'gestor')) {
        return { statusCode: 403, body: JSON.stringify({ error: 'No autorizado. Se requieren permisos de administrador.' }) };
    }

    try {
        const { fechaInicio, fechaFin, prefijoGcal } = event.queryStringParameters || {};
        const prefix = prefijoGcal || 'C';

        if (!fechaInicio || !fechaFin) {
            return { statusCode: 400, body: JSON.stringify({ error: 'Faltan parámetros de fecha' }) };
        }

        let privateKey = process.env.GOOGLE_PRIVATE_KEY.replace(/\\n/g, '\n');
        if (privateKey.startsWith('"') && privateKey.endsWith('"')) {
            privateKey = privateKey.substring(1, privateKey.length - 1);
        }

        const subject = process.env.GOOGLE_IMPERSONATE_EMAIL || null;
        const auth = new google.auth.JWT(
            process.env.GOOGLE_SERVICE_ACCOUNT_EMAIL,
            null,
            privateKey,
            ['https://www.googleapis.com/auth/calendar'],
            subject
        );

        const calendar = google.calendar({ version: 'v3', auth });
        const calendarId = process.env.CALENDAR_ID;
        if (!calendarId) {
            return { statusCode: 500, body: JSON.stringify({ error: 'Falta configurar CALENDAR_ID en Netlify.' }) };
        }

        // Ajustar fechas para abarcar todo el día
        const timeMin = `${fechaInicio}T00:00:00-03:00`;
        const timeMax = `${fechaFin}T23:59:59-03:00`;

        const response = await calendar.events.list({
            calendarId,
            timeMin,
            timeMax,
            maxResults: 2500,
            singleEvents: true,
            orderBy: 'startTime',
            timeZone: 'America/Montevideo',
        });

        let eventos = response.data.items || [];

        // Filtrar solo los eventos que están cancelados
        const cancelados = eventos.filter(ev => ev.summary && ev.summary.startsWith('Cancelada'));

        // Mapear a un formato útil para el frontend
        const resultado = cancelados.map(ev => {
            // Normalizador simple para comparar nombres sin acentos ni mayúsculas
            function _normalize(s){ return (s||'').toString().normalize('NFD').replace(/[\u0300-\u036f]/g,'').toLowerCase().trim(); }

            // Extraer titular (Reserva para) desde description o summary
            let titular = '';
            if (ev.description) {
                const mTit = ev.description.match(/Reserva para:\s*([^\n<]+)/i);
                if (mTit) titular = mTit[1].trim();
            }
            if (!titular && ev.summary) {
                const mSum = ev.summary.match(new RegExp(`${prefix}\\d+:\\s*(.+)`, 'i'));
                if (mSum) titular = mSum[1].trim();
            }

            // Extraer quién canceló y cuándo de la descripción
            let cancelRaw = '';
            let fechaCancelacion = 'Desconocida';
            if (ev.description) {
                const match = ev.description.match(/Reserva Cancelada por:\s*(.+?) el (.+)/i);
                if (match) {
                    cancelRaw = match[1].trim();
                    fechaCancelacion = match[2].trim();
                }
            }

            // Determinar valor final para 'canceladoPor'
            let canceladoPor = 'Administración';
            if (cancelRaw) {
                const cancelName = (cancelRaw.match(/^(.+?)(?:\s*<|$)/) || [cancelRaw])[1].trim();
                if (titular && _normalize(cancelName) === _normalize(titular)) {
                    canceladoPor = titular;
                } else {
                    canceladoPor = 'Administración';
                }
            }

            // Extraer el consultorio(espacio) del título original
            let consultorio = 'N/A';
            const regexEspacio = new RegExp(`${prefix}(\\d+):`, 'i');
            const consMatch = ev.summary.match(regexEspacio);
            if (consMatch) {
                consultorio = consMatch[1];
            }

            return {
                id: ev.id,
                summary: ev.summary,
                start: ev.start && ev.start.dateTime,
                end: ev.end && ev.end.dateTime,
                description: ev.description,
                canceladoPor,
                fechaCancelacion,
                consultorio
            };
        });

        return {
            statusCode: 200,
            headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': 'https://espacia.netlify.app'
            },
            body: JSON.stringify({ canceladas: resultado })
        };

    } catch (error) {
        console.error('Error en informe_canceladas:', error);
        return {
            statusCode: 500,
            body: JSON.stringify({ error: 'Error interno del servidor', details: error.message })
        };
    }
};

```

### Archivo: `netlify/functions/informe_reservas.js`
```javascript
const { google } = require('googleapis');
const { verifyToken } = require('./utils/jwthelper');

exports.handler = async (event) => {
    // Manejar CORS preflight
    if (event.httpMethod === 'OPTIONS') {
        return {
            statusCode: 200,
            headers: {
                'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
                'Access-Control-Allow-Headers': 'Content-Type, Authorization',
                'Access-Control-Allow-Methods': 'GET, OPTIONS',
            },
            body: '',
        };
    }

    if (event.httpMethod !== 'GET') return { statusCode: 405, body: 'Método no permitido' };
    
    const userToken = verifyToken(event);
    if (!userToken) {
        return { statusCode: 401, body: JSON.stringify({ error: 'No autorizado. Token inválido o ausente.' }) };
    }

    try {
        const params = event.queryStringParameters || {};
        const { fechaInicio, fechaFin, consultorio, usuario, prefijoGcal } = params;
        const prefix = prefijoGcal || 'C';
        if (!fechaInicio || !fechaFin) {
            return { statusCode: 400, body: JSON.stringify({ error: 'Debe indicar fechaInicio y fechaFin' }) };
        }
        let privateKey = process.env.GOOGLE_PRIVATE_KEY.replace(/\\n/g, '\n');
        if (privateKey.startsWith('"') && privateKey.endsWith('"')) {
            privateKey = privateKey.substring(1, privateKey.length - 1);
        }
        const subject = process.env.GOOGLE_IMPERSONATE_EMAIL || null;
        const auth = new google.auth.JWT(
            process.env.GOOGLE_SERVICE_ACCOUNT_EMAIL,
            null,
            privateKey,
            ['https://www.googleapis.com/auth/calendar'],
            subject
        );
        const calendar = google.calendar({ version: 'v3', auth });
        const calendarId = process.env.CALENDAR_ID;
        if (!calendarId) {
            return { statusCode: 500, body: JSON.stringify({ error: 'Falta configurar CALENDAR_ID en Netlify.' }) };
        }
        const zonaHoraria = process.env.TIMEZONE || 'America/Montevideo';
        const eventsRes = await calendar.events.list({
            calendarId,
            timeMin: `${fechaInicio}T00:00:00-03:00`,
            timeMax: `${fechaFin}T23:59:59-03:00`,
            timeZone: zonaHoraria,
            maxResults: 2500,
            singleEvents: true,
            orderBy: 'startTime',
            fields: 'items(id,summary,description,start,end,colorId,created)'
        });
        let eventos = eventsRes.data.items || [];
        // Filtrar por consultorio(espacio) si se indica, pero siempre incluir bloqueos globales
        if (consultorio) {
            eventos = eventos.filter(ev => {
                if (!ev.summary) return false;
                // Incluir siempre bloqueos globales (C0 o Prefijo+0: Bloqueada)
                if (ev.summary.toUpperCase().startsWith(`${prefix.toUpperCase()}0: BLOQUEADA`)) return true;
                
                // Limpiar el posible prefijo "Cancelada - " para evaluar correctamente
                const summaryLimpio = ev.summary.replace(/^Cancelada\s*-\s*/i, '');

                // Buscar lista de espacios antes de los dos puntos (ej C1,C2,C3: o S1,S2:)
                const regexLista = new RegExp(`^((${prefix}\\d+,)*${prefix}\\d+):`, 'i');
                const m = summaryLimpio.match(regexLista);
                if (m) {
                    // Separar espacios
                    const espaciosArr = m[1].split(',').map(s => s.trim().toUpperCase());
                    return espaciosArr.includes(`${prefix.toUpperCase()}${consultorio}`);
                }
                // Fallback: buscar Prefijo+N: como antes
                return new RegExp(`^${prefix}${consultorio}:\\s`, 'i').test(summaryLimpio);
            });
        }
        // Si solicitan sólo la lista de usuarios, extraer nombre y email desde las descripciones
        if (params.listUsers) {
            const usersMap = new Map();
            eventos.forEach(ev => {
                if (!ev.description) return;
                // 1. Formato: Reserva realizada por: NOMBRE <email>
                let match = ev.description.match(/Reserva realizada por: (.+?) <([^>]+)>/);
                if (match) {
                    const nombre = match[1].trim();
                    const email = match[2].trim();
                    if (!usersMap.has(email)) usersMap.set(email, { nombre, email });
                    return;
                }
                // 1b. Formato: Reserva para: NOMBRE <email> (reservado para...)
                match = ev.description.match(/Reserva para:\s*(.+?)\s*<([^>]+)>/);
                if (match) {
                    const nombre = match[1].trim();
                    const email = match[2].trim();
                    if (!usersMap.has(email)) usersMap.set(email, { nombre, email });
                    return;
                }
                // 2. Formato: Reserva realizada por: email
                match = ev.description.match(/Reserva realizada por: ([^\n@]+@[^\n]+)/);
                if (match) {
                    const email = match[1].trim();
                    if (!usersMap.has(email)) usersMap.set(email, { nombre: email, email });
                    return;
                }
                // 3. Formato: Reserva realizada por: Nombre (sin email)
                match = ev.description.match(/Reserva realizada por: ([^\n]+)/);
                if (match) {
                    const nombre = match[1].trim();
                    if (!usersMap.has(nombre)) usersMap.set(nombre, { nombre, email: nombre });
                }
            });
            const users = Array.from(usersMap.values()).sort((a, b) => a.nombre.localeCompare(b.nombre));
                // (log eliminado)
            return { statusCode: 200, body: JSON.stringify({ users }) };
        }
        // Filtrar por usuario si se indica
        if (usuario) {
            // Buscar por email, nombre completo o usuario (coincidencia exacta o parcial)
            const busqueda = usuario.trim().toLowerCase();
            eventos = eventos.filter(ev => {
                if (!ev.description) return false;
                // Buscar por email en varios formatos
                const match = ev.description.match(/Reserva realizada por: (.+?) <([^>]+)>/);
                if (match) {
                    const nombre = match[1].trim().toLowerCase();
                    const email = match[2].trim().toLowerCase();
                    if (email.includes(busqueda) || nombre.includes(busqueda)) return true;
                } else {
                    // Buscar por "Reserva para: Nombre <email>"
                    const matchPara = ev.description.match(/Reserva para:\s*(.+?)\s*<([^>]+)>/);
                    if (matchPara) {
                        const nombre = matchPara[1].trim().toLowerCase();
                        const email = matchPara[2].trim().toLowerCase();
                        if (email.includes(busqueda) || nombre.includes(busqueda)) return true;
                    }
                    // Buscar por email simple
                    const matchEmail = ev.description.match(/Reserva realizada por: ([^\n@]+@[^\n]+)/);
                    if (matchEmail) {
                        const email = matchEmail[1].trim().toLowerCase();
                        if (email.includes(busqueda)) return true;
                    }
                    // Buscar por email en línea 'Email: ...'
                    const matchEmailLine = ev.description.match(/Email:\s*([^\s]+)/);
                    if (matchEmailLine) {
                        const email = matchEmailLine[1].trim().toLowerCase();
                        if (email.includes(busqueda)) return true;
                    }
                    // Buscar por nombre simple (también buscar en Reserva para)
                    const matchNombre = ev.description.match(/Reserva realizada por: ([^\n]+)/);
                    if (matchNombre) {
                        const nombre = matchNombre[1].trim().toLowerCase();
                        if (nombre.includes(busqueda)) return true;
                    }
                    const matchParaNombre = ev.description.match(/Reserva para:\s*([^\n<]+)/);
                    if (matchParaNombre) {
                        const nombre = matchParaNombre[1].trim().toLowerCase();
                        if (nombre.includes(busqueda)) return true;
                    }
                }
                return false;
            });
        }
        // Mapear a formato simple
        const resultado = eventos.map(ev => ({
            summary: ev.summary,
            start: ev.start && ev.start.dateTime,
            end: ev.end && ev.end.dateTime,
            description: ev.description,
            id: ev.id,
            created: ev.created || null  // Fecha/hora de creación del evento en Google Calendar
        }));

        // Filtrar bloqueos (C0:, C1: Bloqueada, etc) para que no sumen horas ni aparezcan en el informe final
        let eventosFiltrados = resultado.filter(ev => {
            if (!ev.summary) return true;
            return !ev.summary.toUpperCase().includes('BLOQUEADA');
        });

        // Calcular suma total de horas reservadas solo de reservas reales
        let totalHoras = 0;
        eventosFiltrados.forEach(ev => {
            if (ev.start && ev.end) {
                const inicio = new Date(ev.start);
                const fin = new Date(ev.end);
                const diffMs = fin - inicio;
                const diffHoras = diffMs / (1000 * 60 * 60);
                totalHoras += diffHoras;
            }
        });

        return { statusCode: 200, body: JSON.stringify({ reservas: eventosFiltrados, totalHoras }) };
    } catch (error) {
            // (log eliminado)
        return { statusCode: 500, body: JSON.stringify({ error: 'Error', details: error.message }) };
    }
};
```

### Archivo: `netlify/functions/listar_usuarios.js`
```javascript
// Función para obtener la lista de usuarios desde usuarios.json
const { verifyToken } = require('./utils/jwthelper');
const fetch = (...args) => import('node-fetch').then(({default: fetch}) => fetch(...args));

exports.handler = async function(event, context) {
  // Manejar CORS preflight
  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization',
        'Access-Control-Allow-Methods': 'GET, OPTIONS',
      },
      body: '',
    };
  }

  if (event.httpMethod !== 'GET') {
    return { statusCode: 405, body: 'Método no permitido' };
  }

  const userToken = verifyToken(event);
  if (!userToken) {
    return { statusCode: 401, body: JSON.stringify({ error: 'No autorizado. Token inválido o ausente.' }) };
  }

  let usuarios = [];
  try {
    // Usar la nueva función get-usuarios para leer desde el repo BD
    const baseUrl = process.env.URL || 'http://localhost:8888';
    const resp = await fetch(`${baseUrl}/.netlify/functions/get-usuarios`, {
      headers: {
        'Authorization': event.headers.authorization || event.headers.Authorization
      }
    });
    if (resp.ok) {
      usuarios = await resp.json();
    }
  } catch {}
  // Solo usuarios activos
  usuarios = Array.isArray(usuarios) ? usuarios.filter(u => u.activo !== false) : [];
  return {
    statusCode: 200,
    body: JSON.stringify({ usuarios })
  };
};

```

### Archivo: `netlify/functions/login.js`
```javascript
const fetch = require('node-fetch');
const jwt = require('jsonwebtoken');
const config = require('./utils/config_netlify');

exports.handler = async (event, context) => {
  // Manejar CORS preflight
  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization',
        'Access-Control-Allow-Methods': 'POST, OPTIONS',
      },
      body: '',
    };
  }

  if (event.httpMethod !== 'POST') {
    return {
      statusCode: 405,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Método no permitido' }),
    };
  }

  try {
    const { nomUsu, contrasenaHash } = JSON.parse(event.body);

    if (!nomUsu || !contrasenaHash) {
      return {
        statusCode: 400,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Nombre de usuario y contraseña son requeridos' }),
      };
    }

    // Configuración del repositorio BD
    const githubToken = process.env.GITHUB_TOKEN;
    const repo = config.github.repo;
    const branch = config.github.branch;
    const filePath = config.github.dbPath;

    if (!githubToken) {
      return {
        statusCode: 401,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Token de GitHub no configurado' }),
      };
    }

    const fileUrl = `https://api.github.com/repos/${repo}/contents/${filePath}?ref=${branch}`;
    
    const fileResponse = await fetch(fileUrl, {
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3.raw',
      },
    });

    if (!fileResponse.ok) {
      return {
        statusCode: 500,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al obtener la base de usuarios' }),
      };
    }

    const usuarios = await fileResponse.json();
    const user = Object.values(usuarios).find(u =>
      u.nomUsu === nomUsu &&
      u.contrasena === contrasenaHash &&
      u.activo === true
    );

    if (user) {
      // No devolver la contraseña al cliente
      const { contrasena, ...userSinPass } = user;

      // Determinar duración de sesión según rol
      let expiresIn = config.jwt.expiresInUsuario;
      if (user.rol === 'admin' || user.rol === 'gestor') {
        expiresIn = config.jwt.expiresInGestor;
      }

      // Generar JWT
      const token = jwt.sign(
        { 
          tipdocu: user.tipdocu, 
          documento: user.documento, 
          email: user.email, 
          rol: user.rol,
          nombre: user.nombre,
          nomUsu: user.nomUsu
        }, 
        config.jwt.secret, 
        { expiresIn }
      );

      return {
        statusCode: 200,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ success: true, user: userSinPass, token }),
      };
    } else {
      return {
        statusCode: 401,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ success: false, error: 'Usuario o contraseña incorrectos' }),
      };
    }

  } catch (error) {
    return {
      statusCode: 500,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Error interno del servidor', details: error.message }),
    };
  }
};

```

### Archivo: `netlify/functions/package.json`
```json
{
  "name": "espacia-functions",
  "version": "1.0.3",
  "main": "index.js",
  "dependencies": {
    "jsonwebtoken": "^9.0.0",
    "jwks-rsa": "^3.0.1"
  }
}

```

### Archivo: `netlify/functions/reservar.js`
```javascript
const { google } = require('googleapis');
const { verifyToken } = require('./utils/jwthelper');

exports.handler = async (event) => {
    // Manejar CORS preflight
    if (event.httpMethod === 'OPTIONS') {
        return {
            statusCode: 200,
            headers: {
                'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
                'Access-Control-Allow-Headers': 'Content-Type, Authorization',
                'Access-Control-Allow-Methods': 'POST, GET, DELETE, OPTIONS',
            },
            body: '',
        };
    }

    const userToken = verifyToken(event);
    if (!userToken) {
        return { statusCode: 401, body: JSON.stringify({ error: 'No autorizado. Token inválido o ausente.' }) };
    }

    if (event.httpMethod === 'POST') {
        try {
            const { email, nombre, consultorio, fecha, hora, minutos, intervalo, colorId, creadoPor, summary, prefijoGcal, etiquetaGeneral } = JSON.parse(event.body);
            const missing = [];
            if (!email) missing.push('email');
            if (!consultorio) missing.push('consultorio');
            if (!fecha) missing.push('fecha');
            if (!hora && hora !== 0) missing.push('hora');
            if (!colorId) missing.push('colorId');
            if (missing.length) {
                return { statusCode: 400, body: JSON.stringify({ error: 'Datos faltantes o inválidos', missing, received: { email, consultorio, fecha, hora, colorId } }) };
            }

            // --- Restricción de fecha máxima para usuarios comunes ---
            let rol = userToken.rol || 'usuario';
            // Valores por defecto
            let maxSemanas = 12;
            let horasAntelacionConfig = 24;
            let reglaFinDeSemanaConfig = false;
            let adminMismoDiaHoraTope = "02:00"; // Hasta qué hora se considera "ayer" para un gestor

            // Leer configuraciones dinámicas de config.js (si está disponible)
            try {
                const fetch = require('node-fetch');
                // SI RENOMBRAS EL REPOSITORIO DE GITHUB, CAMBIAR AQUI TAMBIÉN
                const resp = await fetch(process.env.URL_CONFIG_JS || 'https://raw.githubusercontent.com/FABIOR1981/Espacia/main/js/config.js');
                if (resp.ok) {
                    const text = await resp.text();
                    
                    const matchSemanas = text.match(/maxSemanasReservaUsuario\s*:\s*(\d+)/);
                    if (matchSemanas) maxSemanas = parseInt(matchSemanas[1], 10);
                    
                    const matchHoras = text.match(/horasAntelacion\s*:\s*(\d+)/);
                    if (matchHoras) horasAntelacionConfig = parseInt(matchHoras[1], 10);
                    
                    const matchFds = text.match(/reglaFinDeSemana\s*:\s*(true|false)/);
                    if (matchFds) reglaFinDeSemanaConfig = matchFds[1] === 'true';
                }
            } catch (err) {
                console.warn('Error leyendo config.js remoto:', err.message);
            }

            // --- RESTRICCIÓN DE EVENTOS PASADOS PENSADA PARA EL BACKUP AUTOMÁTICO ---
            // Asegurar que solo el admin pueda crear eventos en el pasado.
            // Para Gestores o Usuarios, bloquear la creación de eventos si la fecha pertenece al pasado.
            if (rol !== 'admin') {
                const zonaHoraria = process.env.TIMEZONE || 'America/Montevideo';
                const fechaActualLocal = new Date(new Date().toLocaleString("en-US", { timeZone: zonaHoraria }));
                
                // Extraer el inicio de hoy a las 00:00:00 como límite estricto para usuarios comunes
                const hoyMismoBase = new Date(fechaActualLocal);
                hoyMismoBase.setHours(0, 0, 0, 0);
                
                // Fecha de la reserva solicitada
                const fechaReservaReq = new Date(`${fecha}T00:00:00`); 
                
                // Si la fecha solicitada es ANTERIOR a hoy, estamos retrocediendo en el tiempo
                if (fechaReservaReq < hoyMismoBase) {
                    let permitido = false;
                    
                    // Excepción Gestor: Si es GESTOR, y la reserva pedida es "ayer", 
                    // permite agendarla, pero SÓLO si la hora actual de hoy es ANTES de las 02:00 AM.
                    // Esto permite "cerrar" el período de ayer, salvando el backup automático a las 03:00.
                    if (rol === 'gestor') {
                        const ayer = new Date(hoyMismoBase);
                        ayer.setDate(ayer.getDate() - 1);
                        
                        // Si la reserva solicitada coincide exactamente con el día de "ayer"
                        if (fechaReservaReq.getTime() === ayer.getTime()) {
                            // ¿Es antes de las 02:00 AM de hoy?
                            const horaLimite = new Date(hoyMismoBase);
                            horaLimite.setHours(2, 0, 0, 0); // 02:00 AM
                            
                            if (fechaActualLocal < horaLimite) {
                                permitido = true; 
                            }
                        }
                    }

                    if (!permitido) {
                        return { statusCode: 400, body: JSON.stringify({ error: `La fecha solicitada es en el pasado o los plazos administrativos para cerrar la caja/reporte de esa fecha han vencido. Privilegios insuficientes.` }) };
                    }
                } 
                // Si la fecha solicitada es EXACTAMENTE hoy, bloqueamos agendar turnos anteriores a la hora/minuto actual
                else if (fechaReservaReq.getTime() === hoyMismoBase.getTime()) {
                    // Validamos específicamente contra los usuarios 'comunes'
                    if (rol === 'usuario') {
                        // Construimos la hora solicitada
                        const horaInicioStr = hora.toString().padStart(2, '0');
                        const minInicioStr = (minutos !== undefined ? minutos : 0).toString().padStart(2, '0');
                        
                        // Obtenemos qué hora/min es ahora mismo en MVD
                        const horaActual = fechaActualLocal.getHours();
                        const minutoActual = fechaActualLocal.getMinutes();

                        // Convertimos a minutos absolutos para comparar más fácil (ej. 14:30 = 870)
                        const minutosAbsolutosSolicitados = (parseInt(horaInicioStr, 10) * 60) + parseInt(minInicioStr, 10);
                        const minutosAbsolutosActuales = (horaActual * 60) + minutoActual;

                        // Si intenta agendar en un minuto MENOR al de ahora
                        if (minutosAbsolutosSolicitados < minutosAbsolutosActuales) {
                            return { statusCode: 400, body: JSON.stringify({ 
                                error: `El horario de ${horaInicioStr}:${minInicioStr} hs. ya ha pasado. Por favor, seleccione un horario posterior al actual.` 
                            }) };
                        }
                    }
                }
            }

            // Limitar a maxSemanas en el futuro para usuarios comunes
            if (rol !== 'admin') {
                const hoy = new Date();
                const fechaReserva = new Date(fecha);
                const maxFecha = new Date(hoy);
                maxFecha.setDate(maxFecha.getDate() + (maxSemanas * 7));
                if (fechaReserva > maxFecha) {
                    const numero = '091 001 334';
                    return { statusCode: 400, body: JSON.stringify({ error: `Para agendar con mayor antelación, por favor comuníquese con nuestra secretaría vía WhatsApp al ${numero}. Estamos para ayudarle.` }) };
                }
            }

            let privateKey = process.env.GOOGLE_PRIVATE_KEY.replace(/\\n/g, '\n');
            if (privateKey.startsWith('"') && privateKey.endsWith('"')) {
                privateKey = privateKey.substring(1, privateKey.length - 1);
            }

            const subject = process.env.GOOGLE_IMPERSONATE_EMAIL || null;
            const auth = new google.auth.JWT(
                process.env.GOOGLE_SERVICE_ACCOUNT_EMAIL,
                null,
                privateKey,
                ['https://www.googleapis.com/auth/calendar'],
                subject
            );

            const calendar = google.calendar({ version: 'v3', auth });
            // Calcular hora y minutos de inicio
            const horaInicio = hora.toString().padStart(2, '0');
            const minInicio = minutos !== undefined ? minutos.toString().padStart(2, '0') : '00';
            // Calcular fin según intervalo
            let duracion = intervalo ? parseInt(intervalo, 10) : 60;
            let fechaInicio = new Date(`${fecha}T${horaInicio}:${minInicio}:00-03:00`);
            // Sumar minutos manualmente para evitar errores de zona horaria
            let totalMinutos = parseInt(horaInicio, 10) * 60 + parseInt(minInicio, 10) + duracion;
            let horaFinManual = Math.floor(totalMinutos / 60);
            let minFinManual = totalMinutos % 60;
            // Si pasa de día, ajustar la fecha
            let fechaObj = new Date(fecha);
            let diaFin = fechaObj.getDate();
            let mesFin = fechaObj.getMonth() + 1;
            let anioFin = fechaObj.getFullYear();
            if (horaFinManual >= 24) {
                horaFinManual = horaFinManual % 24;
                // Sumar días
                let fechaTemp = new Date(fechaObj);
                fechaTemp.setDate(fechaTemp.getDate() + 1);
                diaFin = fechaTemp.getDate();
                mesFin = fechaTemp.getMonth() + 1;
                anioFin = fechaTemp.getFullYear();
            }
            const fechaFinStr = `${anioFin}-${mesFin.toString().padStart(2,'0')}-${diaFin.toString().padStart(2,'0')}T${horaFinManual.toString().padStart(2,'0')}:${minFinManual.toString().padStart(2,'0')}:00-03:00`;
            const calendarId = process.env.CALENDAR_ID;
            if (!calendarId) return { statusCode: 500, body: JSON.stringify({ error: 'Falta configurar CALENDAR_ID en Netlify.' }) };
            const sendUpdates = process.env.SEND_UPDATES === 'true';

            // Parámetros dinámicos para configuración Marca Blanca
            const prefix = prefijoGcal || 'C';
            const label = etiquetaGeneral || 'Consultorio';

            // Mostrar solo el nombre en la línea visible; el email se guarda en extendedProperties
            let descripcion = `Reserva para: ${nombre || email}\nEmail: ${email}\n${label}: ${consultorio}\nFecha: ${fecha}\nHora: ${horaInicio}:${minInicio} hs. Intervalo: ${duracion} min`;

            // Determinar nombre del creador: preferir el `creadoPor` enviado por el cliente,
            // pero si no viene, usar el nombre del token del usuario que hace la petición.
            let creadorNombre = '';
            if (creadoPor) {
                const m = creadoPor.match(/^(.*?)\s*</);
                creadorNombre = (m && m[1]) ? m[1].trim() : creadoPor;
            } else {
                creadorNombre = userToken.nombre || (email || '').split('@')[0];
            }
            // Añadir siempre la línea 'Reserva realizada por' (se guarda en el description visible)
            if (creadorNombre) {
                descripcion += `\n\nReserva realizada por: ${creadorNombre}`;
            }

            // Validar email para asistentes
            const cleanEmail = email ? email.trim() : '';

            const resourceBody = {
                summary: summary || `${prefix}${consultorio}: ${nombre || email}`,
                colorId: colorId,
                description: descripcion,
                extendedProperties: {
                    private: {
                        userEmail: cleanEmail || email
                    }
                },
                start: {
                    dateTime: `${fecha}T${horaInicio}:${minInicio}:00-03:00`,
                    timeZone: 'America/Montevideo'
                },
                end: {
                    dateTime: fechaFinStr,
                    timeZone: 'America/Montevideo'
                }
            };

            const insertOpts = {
                calendarId,
                resource: resourceBody
            }; 
            if (sendUpdates) insertOpts.sendUpdates = 'all';

            // Verificar disponibilidad antes de crear el evento
            const busySlots = await calendar.events.list({
                calendarId,
                timeMin: `${fecha}T${horaInicio}:${minInicio}:00-03:00`,
                timeMax: fechaFinStr,
                timeZone: 'America/Montevideo',
            });
            // Ignorar eventos que fueron marcados como 'Cancelada' (se mantienen para informes)
            // Filtrar solo eventos del espacio seleccionado (robusto a espacios y mayúsculas)
            const regexConsultorio = new RegExp(`^${prefix}${consultorio}:\\s`, 'i');
            const ocupantes = (busySlots.data.items || []).filter(ev => {
                if (ev.summary && ev.summary.startsWith('Cancelada')) return false;
                return ev.summary && regexConsultorio.test(ev.summary);
            });
            if (ocupantes.length > 0) {
                return { statusCode: 400, body: JSON.stringify({ error: 'El horario ya está ocupado.', ocupados: ocupantes.map(i => ({ id: i.id, summary: i.summary })) }) };
            }

            // Crear el evento
            let eventRes;
            try {
                eventRes = await calendar.events.insert(insertOpts);
            } catch (insertError) {
                // Google Calendar puede rechazar si hay conflicto de concurrencia real
                // En ese caso hacemos una segunda verificación para dar un mensaje claro
                try {
                    const recheck = await calendar.events.list({
                        calendarId,
                        timeMin: `${fecha}T${horaInicio}:${minInicio}:00-03:00`,
                        timeMax: fechaFinStr,
                        timeZone: 'America/Montevideo',
                    });
                    const ocupadoAhora = (recheck.data.items || []).filter(ev => {
                        if (ev.summary && ev.summary.startsWith('Cancelada')) return false;
                        return ev.summary && regexConsultorio.test(ev.summary);
                    });
                    if (ocupadoAhora.length > 0) {
                        return { statusCode: 409, body: JSON.stringify({ error: 'Este horario acaba de ser reservado por otro usuario. Por favor elegí otro.' }) };
                    }
                } catch {}
                return { statusCode: 500, body: JSON.stringify({ error: 'Error al crear la reserva', details: insertError.message }) };
            }
            return { statusCode: 200, body: JSON.stringify({ message: 'Reserva creada correctamente', eventId: eventRes.data.id, created: eventRes.data.created || null }) };
        } catch (error) {
            return { statusCode: 500, body: JSON.stringify({ error: 'Error', details: error.message }) };
        }
    }

    if (event.httpMethod === 'GET') {
        try {
            const { email, fecha, consultorio, all, prefijoGcal } = event.queryStringParameters || {};
            // Permitir all=1 solo para admin (demo: hardcodear email admin)
            const ADMIN_EMAIL = process.env.ADMIN_EMAIL || 'admin@admin.com';
            if (all === '1') {
                // Solo permitir si el usuario es admin (en producción, validar JWT o cabecera auth)
                // Aquí, para demo, permitir si la petición viene de un navegador logueado como admin
                // (en producción, usar auth real)
                // Si quieres más seguridad, puedes validar por IP o cabecera especial
                // Aquí devolvemos todas las reservas activas
                let privateKey = process.env.GOOGLE_PRIVATE_KEY.replace(/\n/g, '\n');
                if (privateKey.startsWith('"') && privateKey.endsWith('"')) {
                    privateKey = privateKey.substring(1, privateKey.length - 1);
                }
                const subject = process.env.GOOGLE_IMPERSONATE_EMAIL || null;
                const auth = new google.auth.JWT(
                    process.env.GOOGLE_SERVICE_ACCOUNT_EMAIL,
                    null,
                    privateKey,
                    ['https://www.googleapis.com/auth/calendar'],
                    subject
                );
                const calendar = google.calendar({ version: 'v3', auth });
                const calendarId = process.env.CALENDAR_ID;
                if (!calendarId) return { statusCode: 500, body: JSON.stringify({ error: 'Falta configurar CALENDAR_ID en Netlify.' }) };
                const now = new Date();
                const busySlots = await calendar.events.list({
                    calendarId,
                    timeMin: now.toISOString(),
                    maxResults: 2500,
                    singleEvents: true,
                    orderBy: 'startTime',
                    timeZone: 'America/Montevideo',
                });
                const zonaHoraria = process.env.TIMEZONE || 'America/Montevideo';
                // Filtrar solo eventos activos (no cancelados)
                const userEvents = busySlots.data.items.filter(event => {
                    return !(event.summary && event.summary.startsWith('Cancelada'));
                }).map(event => {
                    // Extraer consultorio(espacio), fecha, hora, nombre y email
                    let consultorio = '';
                    let fecha = '';
                    let hora = '';
                    let nombre = '';
                    let email = '';
                    let summary = event.summary || '';
                    if (event.summary) {
                        const prefixMatch = prefijoGcal || 'C';
                        const regex = new RegExp(`^${prefixMatch}(\\d+):`);
                        const match = event.summary.match(regex);
                        if (match) consultorio = match[1];
                    }
                    if (event.start && event.start.dateTime) {
                        const dt = new Date(new Date(event.start.dateTime).toLocaleString('en-US', { timeZone: zonaHoraria }));
                        fecha = dt.toISOString().slice(0,10);
                        hora = dt.getHours();
                    }
                    if (event.description) {
                        // Aceptar tanto 'Reserva realizada por: Nombre <email>' como 'Reserva realizada por: Nombre'
                        const m = event.description.match(/Reserva realizada por:\s*(.+?)(?:\s*<([^>]+)>)?/);
                        if (m) {
                            nombre = (m[1] || '').trim();
                            email = (m[2] || '').trim();
                        }
                        // Si no se obtuvo email, intentar con extendedProperties
                        if ((!email || !email.trim()) && event.extendedProperties && event.extendedProperties.private && event.extendedProperties.private.userEmail) {
                            email = event.extendedProperties.private.userEmail;
                        }
                    }
                    return {
                        eventId: event.id,
                        consultorio,
                        fecha,
                        hora,
                        nombre,
                        email,
                        summary
                    };
                });
                return { statusCode: 200, body: JSON.stringify({ userEvents }) };
            }
            if (!email && !fecha && !consultorio) {
                // Si no hay parámetros validos, retornar lista vacía para evitar errores 500/400 en llamadas espurias
                return { statusCode: 200, body: JSON.stringify({ horasDisponibles: [], ocupadasPorUsuario: [], userEvents: [] }) };
            }
            
            // Si el email es nulo (ej. llamada solo con fecha/consultorio), usar string vacía para evitar fallos abajo
            const emailBusqueda = email || '';

            let privateKey = process.env.GOOGLE_PRIVATE_KEY.replace(/\\n/g, '\n');
            if (privateKey.startsWith('"') && privateKey.endsWith('"')) {
                privateKey = privateKey.substring(1, privateKey.length - 1);
            }
            const subject = process.env.GOOGLE_IMPERSONATE_EMAIL || null;
            const auth = new google.auth.JWT(
                process.env.GOOGLE_SERVICE_ACCOUNT_EMAIL,
                null,
                privateKey,
                ['https://www.googleapis.com/auth/calendar'],
                subject
            );
            const calendar = google.calendar({ version: 'v3', auth });
            const calendarId = process.env.CALENDAR_ID;
            if (!calendarId) return { statusCode: 500, body: JSON.stringify({ error: 'Falta configurar CALENDAR_ID en Netlify.' }) };
            // Si se pasa fecha y consultorio, mantener lógica anterior
            if (fecha && consultorio) {
                const busySlots = await calendar.events.list({
                    calendarId,
                    timeMin: `${fecha}T00:00:00-03:00`,
                    timeMax: `${fecha}T23:59:59-03:00`,
                    timeZone: 'America/Montevideo',
                });
                // Log de todos los eventos recuperados para el día
                // Obtener la zona horaria desde config.js o usar por defecto
                const zonaHoraria = process.env.TIMEZONE || 'America/Montevideo';
                // Filtrar solo eventos del consultorio seleccionado (robusto a espacios y mayúsculas)
                const prefixMatch2 = prefijoGcal || 'C';
                const regexConsultorio = new RegExp(`^${prefixMatch2}${consultorio}:\\s`, 'i');
                const eventosConsultorioAll = busySlots.data.items.filter(event => {
                    return event.summary && regexConsultorio.test(event.summary);
                });
                // Separar eventos activos (no cancelados) de los cancelados
                const eventosConsultorioActivos = eventosConsultorioAll.filter(ev => !(ev.summary && ev.summary.startsWith('Cancelada')));
                // Filtrar solo eventos ocupados por el usuario (activos) y mapear hora y eventId
                const userEvents = eventosConsultorioAll.filter(event => {
                    if (!event.description) return false;
                    const desc = event.description;
                    // Buscar por diferentes formatos donde puede aparecer el email del usuario
                    // Primero verificar si el evento tiene la propiedad privada con el email
                    const propEmail = event.extendedProperties && event.extendedProperties.private && event.extendedProperties.private.userEmail;
                    if (propEmail && emailBusqueda && propEmail.toLowerCase() === emailBusqueda.toLowerCase()) return !(event.summary && event.summary.startsWith('Cancelada'));
                    
                    if (!emailBusqueda) return false; // Si no hay email para filtrar, no es un evento "del usuario"
                    
                    // Mantener compatibilidad con formatos antiguos que incluían el email en la descripción
                    if (desc.includes(`Reserva realizada por: ${emailBusqueda}`)) return !(event.summary && event.summary.startsWith('Cancelada'));
                    if (desc.includes(`<${emailBusqueda}>`)) return !(event.summary && event.summary.startsWith('Cancelada'));
                    if (desc.includes(`Reserva para: ${emailBusqueda}`)) return !(event.summary && event.summary.startsWith('Cancelada'));
                    // regex: Reserva para: ... <email>
                    const mPara = desc.match(/Reserva para:\s*.+?<([^>]+)>/);
                    if (mPara && mPara[1] && mPara[1].trim().toLowerCase() === emailBusqueda.toLowerCase()) return !(event.summary && event.summary.startsWith('Cancelada'));
                    return false; 
                }).map(event => ({
                    hora: new Date(new Date(event.start.dateTime).toLocaleString('en-US', { timeZone: zonaHoraria })).getHours(),
                    eventId: event.id,
                    fecha,
                    consultorio
                }));
                // Generar todas las horas posibles del día
                const horas = Array.from({length: 24}, (_, i) => i);
                // Marcar como ocupadas solo las del usuario (activas)
                const ocupadasPorUsuario = userEvents.map(e => e.hora);
                // Horas ocupadas por cualquier persona en ese consultorio (solo activos)
                const ocupadasTodas = eventosConsultorioActivos.map(event => {
                    if (event.start && event.start.dateTime) {
                        return new Date(new Date(event.start.dateTime).toLocaleString('en-US', { timeZone: zonaHoraria })).getHours();
                    }
                    return null;
                }).filter(h => h !== null);
                const libres = horas.filter(h => !ocupadasTodas.includes(h));
                // Respuesta: solo horas libres o tomadas por el usuario
                const resultado = horas.filter(h => libres.includes(h) || ocupadasPorUsuario.includes(h));
                return { statusCode: 200, body: JSON.stringify({ horasDisponibles: resultado, ocupadasPorUsuario, userEvents }) };
            } else {
                // Si solo se pasa email, devolver todas las reservas activas del usuario
                const now = new Date();
                const busySlots = await calendar.events.list({
                    calendarId,
                    timeMin: now.toISOString(),
                    maxResults: 2500,
                    singleEvents: true,
                    orderBy: 'startTime',
                    timeZone: 'America/Montevideo',
                });
                const zonaHoraria = process.env.TIMEZONE || 'America/Montevideo';
                // Filtrar eventos del usuario
                const userEvents = busySlots.data.items.filter(event => {
                    if (!event.description) return false;
                    // Buscar por email exacto
                    // Preferir propiedad privada con el email (si existe)
                    const propEmail2 = event.extendedProperties && event.extendedProperties.private && event.extendedProperties.private.userEmail;
                    if (propEmail2 && propEmail2.toLowerCase() === (email || '').toLowerCase()) return true;
                    if (event.description.includes(`Reserva realizada por: ${email}`)) return true;
                    if (event.description.includes(`<${email}>`)) return true;
                    if (event.description.includes(`Reserva para: ${email}`)) return true;
                    // Buscar por nombre (si está en usuarios.json)
                    let nombreUsuario = ''; 
                    try {
                        // Cargar usuarios.json como archivo estático
                        const usuariosResp = require('node-fetch');
                        // NOTA: Esto es síncrono en Netlify, pero para robustez, podrías cachear el nombre en memoria
                    } catch {}
                    // Buscar por nombre extraído del email (antes de @)
                    const nombreEmail = email.split('@')[0].replace(/\./g, ' ');
                    if (event.description.includes(`Reserva realizada por: ${nombreEmail}`)) return true;
                    if (event.description.includes(`Reserva para: ${nombreEmail}`)) return true;
                    // Buscar por nombre+email (formato antiguo)
                    const regexNombreEmail = new RegExp(`Reserva realizada por: (.+?) ?<${email}>`);
                    if (regexNombreEmail.test(event.description)) return true;
                    const regexParaEmail = new RegExp(`Reserva para: (.+?) ?<${email}>`);
                    if (regexParaEmail.test(event.description)) return true;
                    return false;
                }).map(event => {
                    // Extraer consultorio, fecha, hora, nombre y email
                    let consultorio = '';
                    let fecha = '';
                    let hora = '';
                    let nombre = '';
                    let emailEvento = '';
                    let summary = event.summary || '';
                    if (event.summary) {
                        const match = event.summary.match(/^C(\d+):/);
                        if (match) consultorio = match[1];
                    }
                    if (event.start && event.start.dateTime) {
                        const dt = new Date(new Date(event.start.dateTime).toLocaleString('en-US', { timeZone: zonaHoraria }));
                        fecha = dt.toISOString().slice(0,10);
                        hora = dt.getHours();
                    }
                    if (event.description) {
                        // Extraer nombre y email si están presentes
                        const m = event.description.match(/Reserva realizada por: (.+?) <([^>]+)>/);
                        if (m) {
                            nombre = m[1].trim();
                            emailEvento = m[2].trim();
                        } else {
                            // Si solo hay email
                            const m2 = event.description.match(/Reserva realizada por: ([^@\s]+@[^\s]+)/);
                            if (m2) {
                                nombre = '';
                                emailEvento = m2[1].trim();
                            } else {
                                // Si solo hay nombre
                                const m3 = event.description.match(/Reserva realizada por: (.+)/);
                                if (m3) {
                                    nombre = m3[1].trim();
                                    emailEvento = '';
                                }
                            }
                        }
                    }
                    return {
                        eventId: event.id,
                        consultorio,
                        fecha,
                        hora,
                        nombre,
                        email: emailEvento,
                        summary
                    };
                });
                return { statusCode: 200, body: JSON.stringify({ userEvents }) };
            }
        } catch (error) {
            return { statusCode: 500, body: JSON.stringify({ error: 'Error', details: error.message }) };
        }
    }

   if (event.httpMethod === 'DELETE') {
        try {
            // Extraemos solo el eventId del cuerpo. 
            // Ignoramos 'esAdmin' del cliente por seguridad.
            const { eventId, nombreUsuario: nombreNavegador } = JSON.parse(event.body);
            
            if (!eventId) {
                return { statusCode: 400, body: JSON.stringify({ error: 'Falta el ID del evento' }) };
            }

            // --- VALIDACIÓN DE SEGURIDAD BASADA EN TOKEN ---
            // El servidor decide si es admin basándose en el JWT, no en el JSON recibido
            const esAdmin = userToken.rol === 'admin' || userToken.rol === 'gestor';
            // Usar siempre la identidad provista por el JWT (userToken.nombre). No confiar en el nombre enviado por el cliente.
            const usuarioCancela = userToken.nombre ? userToken.nombre : 'Usuario';

            // debug: para confirmar quién está cancelando
            console.log('[reservar DELETE] userToken.nombre=', userToken.nombre, 'userToken.email=', userToken.email, 'esAdmin=', esAdmin, 'eventId=', eventId);

            let privateKey = process.env.GOOGLE_PRIVATE_KEY.replace(/\\n/g, '\n');
            if (privateKey.startsWith('"') && privateKey.endsWith('"')) {
                privateKey = privateKey.substring(1, privateKey.length - 1);
            }
            
            const subject = process.env.GOOGLE_IMPERSONATE_EMAIL || null;
            const auth = new google.auth.JWT(
                process.env.GOOGLE_SERVICE_ACCOUNT_EMAIL,
                null,
                privateKey,
                ['https://www.googleapis.com/auth/calendar'],
                subject
            );
            
            const calendar = google.calendar({ version: 'v3', auth });
            const calendarId = process.env.CALENDAR_ID;
            if (!calendarId) return { statusCode: 500, body: JSON.stringify({ error: 'Falta configurar CALENDAR_ID.' }) };

            // Obtener el evento para validar política de cancelación y propiedad
            const eventRes = await calendar.events.get({ calendarId, eventId });
            const eventStart = new Date(eventRes.data.start.dateTime);
            const now = new Date();

            // Carga de configuración remota (se mantiene igual)
            let horasAntelacionConfig = 24;
            let reglaFinDeSemanaConfig = false;

            try {
                const fetch = require('node-fetch');
                const resp = await fetch(process.env.URL_CONFIG_JS || 'https://raw.githubusercontent.com/FABIOR1981/Espacia/main/js/config.js');
                if (resp.ok) {
                    const text = await resp.text();
                    const matchHoras = text.match(/horasAntelacion\s*:\s*(\d+)/);
                    if (matchHoras) horasAntelacionConfig = parseInt(matchHoras[1], 10);
                    const matchFds = text.match(/reglaFinDeSemana\s*:\s*(true|false)/);
                    if (matchFds) reglaFinDeSemanaConfig = matchFds[1] === 'true';
                }
            } catch (err) {
                console.warn('Error leyendo config.js remoto:', err.message);
            }
            
            let horasRequeridas = horasAntelacionConfig;
            if (reglaFinDeSemanaConfig && eventStart.getDay() === 1) { // Lunes
                horasRequeridas = horasAntelacionConfig + 48;
            }

            const hoursUntilEvent = (eventStart - now) / (1000 * 60 * 60);
            
            if (hoursUntilEvent < 0) {
                return { statusCode: 400, body: JSON.stringify({ error: 'No puedes cancelar reservas que ya pasaron.' }) };
            }
            
            // VALIDACIÓN DE POLÍTICA: Solo se aplica a usuarios no admin
            if (hoursUntilEvent <= horasRequeridas && !esAdmin) {
                return { statusCode: 400, body: JSON.stringify({ error: `Política de cancelación: No puedes cancelar dentro de las ${horasRequeridas} horas antes.` }) };
            }
            
            // VALIDACIÓN DE PROPIEDAD: Si no es admin, debe ser el dueño
            if (!esAdmin) {
                const eventDesc = (eventRes.data.description || '').toLowerCase();
                const userEmail = (userToken.email || '').toLowerCase();
                
                const propEmail = eventRes.data.extendedProperties?.private?.userEmail;
                const matchProp = propEmail && propEmail.toLowerCase() === userEmail;
                
                const matchDesc = eventDesc.includes(`email: ${userEmail}`) || 
                                  eventDesc.includes(`<${userEmail}>`) || 
                                  eventDesc.includes(userEmail);
                                  
                if (!matchProp && !matchDesc) {
                    return { statusCode: 403, body: JSON.stringify({ error: 'No tienes permiso para cancelar esta reserva.' }) };
                }
            }

            // Actualizar el evento
            const nuevoTitulo = eventRes.data.summary.startsWith('Cancelada') ? eventRes.data.summary : `Cancelada - ${eventRes.data.summary}`;
            const fechaCancelacion = new Date().toLocaleString('es-UY', { timeZone: 'America/Montevideo' });
            
            let nuevaDescripcion = eventRes.data.description || '';
            // Forzar que se use el nombre desde el JWT (userToken.nombre) al registrar la cancelación
            nuevaDescripcion += `\nReserva Cancelada por: ${usuarioCancela} el ${fechaCancelacion}`;

            await calendar.events.patch({
                calendarId,
                eventId,
                resource: {
                    summary: nuevoTitulo,
                    colorId: '8', // Gris
                    description: nuevaDescripcion
                }
            });

            return { statusCode: 200, body: JSON.stringify({ message: 'Reserva marcada como cancelada por el servidor.' }) };

        } catch (error) {
            console.error('Error en DELETE:', error);
            return { statusCode: 500, body: JSON.stringify({ error: 'Error interno', details: error.message }) };
        }
    }

    return { statusCode: 405, body: 'Método no permitido' };
};
```

### Archivo: `netlify/functions/update-usuarios.js`
```javascript
const fetch = require('node-fetch');
const { verifyToken } = require('./utils/jwthelper');
const config = require('./utils/config_netlify');

exports.handler = async (event, context) => {
  // Solo permitir POST
  if (event.httpMethod !== 'POST') {
    return {
      statusCode: 405,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Método no permitido' }),
    };
  }

  // Manejar CORS preflight
  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization',
        'Access-Control-Allow-Methods': 'POST, OPTIONS',
      },
      body: '',
    };
  }

  const user = verifyToken(event);
  if (!user || (user.rol !== 'admin' && user.rol !== 'gestor')) {
    return {
      statusCode: 403,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'No autorizado. Se requieren permisos de administrador.' }),
    };
  }

  try {
    const { data, token } = JSON.parse(event.body);
    if (!data) {
      return {
        statusCode: 400,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Datos requeridos' }),
      };
    }
    if (!Array.isArray(data)) {
      return {
        statusCode: 400,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Los datos deben ser un array' }),
      };
    }

    // --- CORRECCIÓN: Validar nomUsu y nombre (email ahora es opcional) ---
    const nomUsuExistente = new Set();
    for (const item of data) {
      if (!item.nomUsu || !item.nombre) {
        return {
          statusCode: 400,
          headers: {
            'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ error: 'Cada usuario debe tener ID (nomUsu) y nombre' }),
        };
      }

      const nomUsuLower = item.nomUsu.toString().trim().toLowerCase();
      if (nomUsuExistente.has(nomUsuLower)) {
        return {
          statusCode: 400,
          headers: {
            'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ error: `ID de usuario duplicado: ${item.nomUsu}. Por favor, use un ID único y edite el usuario existente en lugar de crear uno nuevo.` }),
        };
      }
      nomUsuExistente.add(nomUsuLower);
    }


    // Token de GitHub (debe estar en variables de entorno de Netlify)
    const githubToken = process.env.GITHUB_TOKEN || token;
    const repo = config.github.repo;
    const branch = config.github.branch;
    const filePath = config.github.dbPath;

    if (!githubToken) {
      return {
        statusCode: 401,
        headers: {
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Token de GitHub no configurado' }),
      };
    }

    // 1. Obtener el SHA actual del archivo
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
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al obtener el archivo de GitHub', details: error }),
      };
    }

    const fileData = await fileResponse.json();
    const currentSha = fileData.sha;
    
    // Decodificar el contenido actual para recuperar las contraseñas
    let currentUsers = {};
    try {
      const currentContent = Buffer.from(fileData.content, 'base64').toString('utf8');
      currentUsers = JSON.parse(currentContent);
    } catch (e) {
      console.error("Error parseando el contenido actual de usuarios.json", e);
    }

    // --- CORRECCIÓN: Lógica de fusión (Merge) basada en nomUsu ---
    const mergedDataDict = {};
    data.forEach(newUser => {
      // Usamos nomUsu como clave principal para evitar problemas si falta el email
      const key = (newUser.tipdocu && newUser.documento) 
                  ? `${newUser.tipdocu}-${newUser.documento}` 
                  : newUser.nomUsu;

      // Buscar si el usuario ya existía para mantener su contraseña
      const existingUser = currentUsers[key] || Object.values(currentUsers).find(u => u.nomUsu === newUser.nomUsu);
      
      if (existingUser && !newUser.contrasena) {
        newUser.contrasena = existingUser.contrasena;
      }
      mergedDataDict[key] = newUser;
    });

    // 2. Preparar el nuevo contenido
    const newContent = JSON.stringify(mergedDataDict, null, 2).replace(
      /"consPref": \[\s+([\s\S]*?)\s+\]/g,
      (match, p1) => `"consPref": [${p1.replace(/\s+/g, '')}]`
    );
    const encodedContent = Buffer.from(newContent).toString('base64');

    // 3. Actualizar el archivo en GitHub
    const updateResponse = await fetch(fileUrl, {
      method: 'PUT',
      headers: {
        Authorization: `token ${githubToken}`,
        Accept: 'application/vnd.github.v3+json',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        message: 'Actualización de usuarios realizada desde ESPACIA.',
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
          'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ error: 'Error al actualizar el archivo en GitHub', details: error }),
      };
    }

    const updateResult = await updateResponse.json();
    return {
      statusCode: 200,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ success: true, message: 'Usuarios sincronizados con GitHub', commit: updateResult.commit.sha }),
    };

  } catch (error) {
    return {
      statusCode: 500,
      headers: {
        'Access-Control-Allow-Origin': 'https://espacia.netlify.app',
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ error: 'Error interno del servidor', details: error.message }),
    };
  }
};
```

### Archivo: `netlify/functions/utils/backup/config.js`
```javascript
// Backup del antiguo config.js
// Este archivo solo es copia de seguridad, no se usa en producción.

module.exports = {
  github: {
    repo: process.env.GITHUB_DB_REPO || 'FABIOR1981/bd',
    branch: process.env.GITHUB_DB_BRANCH || 'main',
    dbPath: process.env.GITHUB_DB_PATH || 'espacia/usuarios.json'
  },
  jwt: {
    secret: process.env.JWT_SECRET || 'secreto_desarrollo_espacia',
    expiresIn: '24h'
  }
};

```

### Archivo: `netlify/functions/utils/config_netlify.js`
```javascript
// config_netlify.js - Configuración centralizada para el backend
// Este archivo NO se envía al frontend, solo se usa en las Netlify Functions

module.exports = {
    github: {
        repo: process.env.GITHUB_DB_REPO || 'FABIOR1981/bd',
        branch: process.env.GITHUB_DB_BRANCH || 'main',
        dbPath: process.env.GITHUB_DB_PATH || 'espacia/usuarios.json'
    },
    jwt: {
        secret: process.env.JWT_SECRET || 'secreto_desarrollo_espacia',
        expiresInGestor: '12h',   // 12 horas para Admins y Gestores
        expiresInUsuario: '4h'    // 4 horas para el resto de los profesionales
    }
};

```

### Archivo: `netlify/functions/utils/jwthelper.js`
```javascript
const jwt = require('jsonwebtoken');
const config = require('./config_netlify');

/**
 * Verifica el token JWT en los headers de la petición.
 * @param {Object} event - El evento de Netlify Function.
 * @returns {Object|null} - El payload del token decodificado si es válido, o null si es inválido/no existe.
 */
const verifyToken = (event) => {
  const authHeader = event.headers.authorization || event.headers.Authorization;
  
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return null;
  }

  const token = authHeader.split(' ')[1];

  try {
    const decoded = jwt.verify(token, config.jwt.secret);
    return decoded;
  } catch (error) {
    console.error('Error verificando JWT:', error.message);
    return null;
  }
};

module.exports = {
  verifyToken
};

```

### Archivo: `netlify/functions/utils/validateauth0token.js`
```javascript
// utils/validateAuth0Token.js
const jwt = require('jsonwebtoken');
const jwksClient = require('jwks-rsa');

const AUTH0_DOMAIN = process.env.AUTH0_DOMAIN;
const AUTH0_AUDIENCE = process.env.AUTH0_AUDIENCE;

const client = jwksClient({
  jwksUri: `https://${AUTH0_DOMAIN}/.well-known/jwks.json`
});

function getKey(header, callback) {
  client.getSigningKey(header.kid, function(err, key) {
    if (err) {
      callback(err);
    } else {
      const signingKey = key.getPublicKey();
      callback(null, signingKey);
    }
  });
}

async function validateAuth0Token(authHeader) {
  if (!authHeader) return null;
  const token = authHeader.replace('Bearer ', '');
  try {
    return await new Promise((resolve, reject) => {
      jwt.verify(token, getKey, {
        audience: AUTH0_AUDIENCE,
        issuer: `https://${AUTH0_DOMAIN}/`,
        algorithms: ['RS256']
      }, (err, decoded) => {
        if (err) return resolve(null);
        resolve(decoded);
      });
    });
  } catch {
    return null;
  }
}

module.exports = { validateAuth0Token };
```

### Archivo: `netlify/functions/utils/validateidentitytoken.js`
```javascript
// Utilidad para validar el token de Netlify Identity (Auth0 como proveedor externo)
const jwt = require('jsonwebtoken');
const jwksClient = require('jwks-rsa');

const IDENTITY_JWKS_URI = process.env.IDENTITY_JWKS_URI; // Debes configurar esta variable en Netlify

const client = jwksClient({
  jwksUri: IDENTITY_JWKS_URI
});

function getKey(header, callback) {
  client.getSigningKey(header.kid, function(err, key) {
    if (err) {
      callback(err);
    } else {
      const signingKey = key.getPublicKey();
      callback(null, signingKey);
    }
  });
}

async function validateIdentityToken(authHeader) {
  if (!authHeader) return null;
  const token = authHeader.replace('Bearer ', '');
  try {
    return await new Promise((resolve, reject) => {
      jwt.verify(token, getKey, {}, (err, decoded) => {
        if (err) return resolve(null);
        resolve(decoded);
      });
    });
  } catch {
    return null;
  }
}

module.exports = { validateIdentityToken };
```

### Archivo: `package.json`
```json
{
  "name": "gestion-espacios",
  "version": "4.9.17",
    "description": "Sistema de turnos con Google Calendar",
    "main": "index.html",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "dependencies": {
      "@netlify/functions": "^2.6.0",
      "googleapis": "^126.0.0",
      "jsonwebtoken": "^9.0.2"
    },
    "author": "",
    "license": "ISC"
  }
```

### Archivo: `readme.md`
```md
# Espacia

Espacia es una aplicación web para la gestión de reservas, usuarios y reportes en entornos colaborativos o institucionales. El proyecto utiliza tecnologías web modernas (HTML, CSS, JavaScript) y arquitectura JAMstack, facilitando su despliegue en Netlify...

## Características principales (ok)
- Gestión de reservas y bloqueos de espacios.
- Administración de usuarios y control de acceso.
- Generación de informes y reportes en PDF.
- Interfaz intuitiva y adaptable.
- Integración con funciones serverless (Netlify Functions).
- Soporte para autenticación y seguridad básica.

## Manuales por rol

Cada manual explica solo lo que puede hacer ese perfil desde la pantalla, en lenguaje simple y con capturas reales (datos de ejemplo):

| Perfil | Para quién | Markdown | PDF |
|---|---|---|---|
| **Usuario** | Profesionales que reservan consultorios | [manual_usuario.md](documentacion/manual_usuario.md) | [Manual_Usuario.pdf](documentacion/Manual_Usuario.pdf) |
| **Gestor** | Secretaría / recepción | [manual_gestor.md](documentacion/manual_gestor.md) | [Manual_Gestor.pdf](documentacion/Manual_Gestor.pdf) |
| **Administrador** | Responsables del sistema | [manual_admin.md](documentacion/manual_admin.md) | [Manual_Administrador.pdf](documentacion/Manual_Administrador.pdf) |

## Estructura del proyecto
- `js/`: Lógica de la aplicación y componentes JavaScript.
- `css/`: Hojas de estilo.
- `imagenes/`: Recursos gráficos.
- `netlify/functions/`: Funciones serverless backend.
- Archivos HTML principales: `index.html`, `dashboard.html`, etc.

## Instalación y despliegue
1. Clona este repositorio.
2. Instala dependencias si aplica.
3. Despliega en Netlify o tu plataforma JAMstack preferida.

## Estado actual
La base de datos se gestiona mediante archivos JSON en GitHub. Se planea migrar a una solución más robusta (Supabase, Firebase o MongoDB).

---

¡Gracias por tu interés en Espacia! Para dudas o contribuciones, revisa la documentación o contacta al equipo de desarrollo.

```

### Archivo: `sw.js`
```javascript
// ATENCIÓN: Al cambiar este número hazlo también en js/config.js para actualizar la interfaz.
const CACHE_NAME = 'espacia-cache-v4.9.17';
const urlsToCache = [
  '/',
  '/index.html',
  '/dashboard.html',
  '/manifest.json',
  
  // Archivos de Estilos (CSS)
  '/css/1_comunes.css',
  '/css/2_abmusu.css',
  '/css/2_agenda_v2.css',
  '/css/2_dashboard.css',
  '/css/2_informe.css',
  '/css/2_login.css',
  '/css/2_pdf_styles.css',
  '/css/2_reservas.css',

  // Archivos de Código (JS)
  '/js/persistencia/abm_usuario.js',
  '/js/agenda_v2.js',
  '/js/bloqueos.js',
  '/js/components/timeindicator.js',
  '/js/config.js',
  '/js/dashboard.js',
  '/js/informe_canceladas.js',
  '/js/informe_modular.js',
  '/js/login_local.js',
  '/js/modelo/reserva.js',
  '/js/pdf/pdf_core.js',
  '/js/pdf/pdf_reporte_general.js',
  '/js/pdf/pdf_reporte_nombre.js',
  '/js/pdf/pdf_ticket_cancela.js',
  '/js/pdf/pdf_ticket_reserva.js',
  '/js/reservas_futuras.js',
  '/js/utils.js',
  // Subcarpetas JS
  '/js/agenda/agenda_cancelar.js',
  '/js/agenda/agenda_detalle.js',
  '/js/agenda/agenda_reservar.js',

  // Imágenes y logos
  '/imagenes/icon-192.png',
  '/imagenes/icon-512.png',
  '/imagenes/logo_demaria.png'
];

// Instalar el Service Worker y almacenar en caché los archivos básicos
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => {
        console.log('Descargando y guardando Espacia en caché local...');
        return cache.addAll(urlsToCache);
      })
  );
  self.skipWaiting();
});

// Interceptar peticiones
self.addEventListener('fetch', event => {
  const url = new URL(event.request.url);

  // 1. ESTRATEGIA "NETWORK FIRST"
  // Para HTML, JSON de datos y funciones de Netlify.
  if (
    event.request.mode === 'navigate' || 
    event.request.headers.get('accept').includes('text/html') ||
    url.pathname.includes('/.netlify/functions/') ||
    (url.pathname.endsWith('.json') && !url.pathname.endsWith('manifest.json'))
  ) {
    event.respondWith(
      fetch(event.request).catch(() => caches.match(event.request))
    );
    return;
  }

  // 2. ESTRATEGIA "STALE-WHILE-REVALIDATE" (EL FIX)
  // Para JS, CSS e Imágenes. Carga rápido desde caché pero busca actualizaciones en red.
  event.respondWith(
    caches.match(event.request).then(cachedResponse => {
      const fetchPromise = fetch(event.request).then(networkResponse => {
        if (networkResponse && networkResponse.status === 200) {
          caches.open(CACHE_NAME).then(cache => {
            cache.put(event.request, networkResponse.clone());
          });
        }
        return networkResponse;
      });
      return cachedResponse || fetchPromise;
    })
  );
});

// Activar y limpiar cachés viejas automáticamente
self.addEventListener('activate', event => {
  const cacheWhitelist = [CACHE_NAME];
  event.waitUntil(
    caches.keys().then(cacheNames => {
      return Promise.all(
        cacheNames.map(cacheName => {
          if (cacheWhitelist.indexOf(cacheName) === -1) {
            console.log('Borrando caché antigua:', cacheName);
            return caches.delete(cacheName);
          }
        })
      );
    })
  );
  self.clients.claim();
});
self.addEventListener('message', event => {
  if (event.data && event.data.type === 'SKIP_WAITING') {
    self.skipWaiting();
  }
});
```

### Archivo: `vscode-vfs-info.md`
```md
# VFS / Repo Path Info for AI

Este archivo describe cómo interactuar con el proyecto cuando está abierto como una carpeta virtual (VFS) en VS Code.

## Rutas VFS
Usar siempre la ruta VFS exacta al leer o editar archivos si el proyecto viene de un repositorio remoto.

Ejemplo de ruta:
- `vscode-vfs://github/FABIOR1981/Espacia`

## Cómo avisar al asistente
Cuando el proyecto está en VFS, menciona explícitamente:
- "El proyecto está en el VFS"
- "La ruta es `vscode-vfs://github/FABIOR1981/Espacia`"
- El archivo actual abierto en el editor, por ejemplo `dashboard.html`.

## Qué evitar
- No usar rutas locales como `C:\Users\fabio\Espacia` cuando el explorador muestra `vscode-vfs://...`.
- No asumir que la copia local es la misma que el repositorio VFS.

## Por qué es importante
- En VS Code, el repositorio remoto se presenta como un sistema de archivos virtual.
- Las herramientas deben leer/escribir en el VFS para que los cambios sean visibles en el explorador.

## Resultado esperado
Si el proyecto viene del repositorio remoto y está abierto como VFS, todas las ediciones deben hacerse en archivos bajo la ruta:
- `vscode-vfs://github/FABIOR1981/Espacia`

Y el asistente debe usar esa ruta en lugar de buscar una copia local.

```

