# HTML REDESIGN SPECIFICATION — JEFFREY LEVINSON V3
## Especificación Arquitectónica, Sistema de Diseño e Implementación UI/UX

**Fecha:** 28 de octubre de 2026  
**Autor:** Director Creativo Senior & Director de Arte UI/UX (Paguro Creative)  
**Destinatarios:** Danna Olarte (Líder Técnico Antigravity), Joseph (Creative Lead / Partner)  
**Clasificación:** **ESPECIFICACIÓN TÉCNICA DE DESARROLLO FRONT-END (HTML5 / CSS3 / ES6)**  

---

## 1. Objetivos del Rediseño y Resolución de Fallas Previas

El rediseño del archivo maestro `website/v3/index.html` erradica de raíz las tres fallas críticas diagnosticadas en `CREATIVE_AUDIT_V3.md`:

1. **Resolución del Problema A (Colapso a 228 px en `.strip`):**
   - Se elimina la cuadrícula estática de 5 columnas comprimidas.
   - Se implementa el **Editorial Showcase Interactivo**: un escenario principal (*Main Stage*) a proporción estricta 4:5 de gran escala (mínimo 540 px de ancho en pantallas de 1440 px), complementado por una **Banda de Navegación de 5 Miniaturas (*Filmstrip Navigator*)** con estados activos y un **Panel Dinámico de Metadatos y Narrativa** sincronizado en tiempo real.
2. **Resolución del Problema B (Aislamiento de Alternativas en `.alts-grid`):**
   - Se transforman las imágenes `-07` y `-08` en un **Díptico Editorial Curado ("Frames from the Night")**, con diseño asimétrico tipo revista de arte, marcos finos metálicos y tarjetas contextuales de canal (*VIP Lookbook* y *Digital Press*).
3. **Resolución del Problema C (Ausencia de Narrativa Comercial):**
   - Se incorpora a cada campaña una **Tarjeta Ejecutiva de Intención y Valor Comercial**, detallando el perfil de compradora, el disparador psicológico de compra, la justificación de precio (US$ 1.395 a US$ 3.200) y la ecuación de ingeniería que avala la inversión.

---

## 2. Sistema de Diseño (Design System)

### 2.1. Paleta Cromática de Lujo Nocturno
```css
:root {
  /* Fondos y Superficies */
  --bg-deep: #08080a;           /* Fondo negro carbón absoluto */
  --bg-surface: #101014;        /* Superficie de tarjetas y paneles */
  --bg-elevated: #16161c;       /* Módulos flotantes y controles */
  --bg-glass: rgba(16, 16, 20, 0.75); /* Vidrio esmerilado con backdrop-filter */

  /* Límites y Bordes */
  --border-subtle: rgba(255, 255, 255, 0.08); /* Líneas de división sutiles */
  --border-medium: rgba(255, 255, 255, 0.16); /* Bordes activos de miniaturas */
  --border-focus: rgba(212, 175, 55, 0.45);   /* Anillos de foco y acentos oro */

  /* Tipografía y Textos */
  --text-primary: #f5f5f7;      /* Blanco perla de máxima legibilidad */
  --text-secondary: #a2a2b0;    /* Gris medio para metadatos y fichas técnicas */
  --text-muted: #6e6e7c;        /* Gris oscuro para numeración y microcopy */
  
  /* Acentos de Campaña */
  --accent-gold: #d4af37;       /* Oro 18K para Campaña 01 Vintage */
  --accent-chrome: #9ec5fe;     /* Azul hielo / Gunmetal para Campaña 02 Intermission */
  --accent-touring: #e63946;    /* Rojo Touring para Campaña 03 Between Places */
  
  /* Sombras y Luces */
  --shadow-card: 0 12px 36px rgba(0, 0, 0, 0.55);
  --shadow-active: 0 0 24px rgba(212, 175, 55, 0.25);
  --radius-sm: 2px;
  --radius-md: 4px;
}
```

### 2.2. Tipografía y Jerarquía
- **Display / Titulares de Campaña:** Fuentes con serifa de corte editorial de alto contraste, inspiradas en la alta costura italiana y revistas de arquitectura:
  `font-family: 'Playfair Display', 'Cormorant Garamond', 'Didot', 'Bodoni MT', serif;`
- **Cuerpo, UI y Metadatos Técnicos:** Tipografía geométrica sans-serif neutra y nítida:
  `font-family: 'Plus Jakarta Sans', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;`
- **Monospace para Datos de Ingeniería y Códigos de Pieza:**
  `font-family: 'JetBrains Mono', 'SF Mono', Consolas, monospace;`

### 2.3. Proporciones y Escala
- **Aspect Ratio Inmutable:** Todas las fotografías de campaña se renderizan estrictamente en `aspect-ratio: 4 / 5`. Queda terminantemente prohibido recortar las imágenes con relaciones arbitrarias tipo `16:9` o columnas delgadas de `1:2`.

---

## 3. Arquitectura de Componentes HTML/CSS

### 3.1. Navegación Global (`header.nav-header`)
- Logotipo tipográfico `JEFFREY LEVINSON` en espaciado amplio (`letter-spacing: 0.28em`).
- Subtítulo de manufactura: `AEROSPACE MONOCOQUE · PHILADELPHIA`.
- Enlaces de salto directo:
  - `01 VINTAGE`
  - `02 INTERMISSION`
  - `03 BETWEEN PLACES`
  - `AUDIT & STRATEGY`
- Barra flotante fija con fondo de cristal ahumado (`backdrop-filter: blur(16px)`).

---

### 3.2. Hero Section: Tríptico Editorial (`section.hero-triptych`)
- **Estructura:** 3 columnas verticales equivalentes en desktop (cada una contiene una de las piezas estáticas maestras: `c1_static.jpg`, `c2_static.jpg`, `c3_static.jpg`).
- **Comportamiento Interactivo:**
  - En reposo, las 3 columnas conviven en un equilibrio visual sereno.
  - Al hacer hover sobre una columna, se aplica un zoom sutil (`transform: scale(1.03)`) y un velo de sombra tenue sobre las otras dos columnas, dirigiendo el foco óptico.
  - Al hacer clic en cualquier columna del tríptico, la página realiza un desplazamiento suave (*smooth scroll*) directamente al capítulo correspondiente.
- **Overlays Informativos:** Cada columna incorpora:
  - Micro-etiqueta superior: `COLLECTION 01`, `COLLECTION 02`, `COLLECTION 03`.
  - Título y acabado: `ELINA PLUS · MALBEC`, `ELINA PLUS · BLACK CHROME`, `FINLEY · TOURING RED`.
  - Indicador de anclaje: `EXPLORE CHAPTER ↓`.
- **Cero Elementos 3D:** Erradicación total de cualquier modelo 3D WebGL o canvas sintético.

---

### 3.3. Manifiesto Estratégico y Ancla de Marca (`section.manifesto-bar`)
- Bloque editorial de alta jerarquía tipográfica entre el Hero y las Campañas.
- Define las 4 credenciales de ingeniería de Jeffrey Levinson:
  1. *Aluminio Aeroespacial 6061-T6 Monocasco* tallado por CNC.
  2. *Laca Cerámica Patentada* resistente a rayaduras e impermeable.
  3. *Cierre Acústico de Tolerancia Micrométrica* con clic de seguridad.
  4. *Tecnología RF-Transparente Patentada* que permite recibir señal móvil y 5G con el bolso cerrado.

---

### 3.4. Capítulos de Campaña (`section.campaign-chapter`)
Cada una de las 3 campañas se articula a través de una sección completa estructurada en 4 módulos consecutivos:

#### Módulo 1: Cabecera de Campaña (`.campaign-header`)
- Número y título en romana de gran formato (ej. `01 / VINTAGE`).
- Badge de especificación técnica de producto (ej. `ELINA PLUS · 6061-T6 ALUMINUM · 18K GOLD HARDWARE · CHAINLESS`).
- Extracto de manifiesto narrativo y contextualización de la escena.

#### Módulo 2: The Interactive Editorial Showcase (`.editorial-showcase`)
Este componente resuelve definitivamente el **Problema A**. Consta de un layout bifocal sincronizado por JavaScript:

```
+---------------------------------------------------------------------------------------+
|  THE INTERACTIVE EDITORIAL SHOWCASE                                                  |
|                                                                                       |
|  +------------------------------------+  +-----------------------------------------+  |
|  |                                    |  |  SLIDE 01 / 05                          |  |
|  |                                    |  |  PIECE CODE: C1-02                      |  |
|  |                                    |  |  TITLE: The Niche                       |  |
|  |                                    |  |                                         |  |
|  |        MAIN DISPLAY STAGE          |  |  SCENE RATIONALE:                       |  |
|  |        Proporción Estricta 4:5     |  |  Bodegón arquitectónico donde el clutch |  |
|  |        Ancho: 540px - 620px        |  |  Elina PLUS Malbec reposa verticalmente |  |
|  |        Resolución Óptima           |  |  en un nicho de piedra caliza...        |  |
|  |                                    |  |                                         |  |
|  |                                    |  |  TECHNICAL SPECIFICATIONS:              |  |
|  |                                    |  |  • Optic: 100mm Macro · f/4.0           |  |
|  |                                    |  |  • Finish: Ceramic Malbec Lacquer       |  |
|  |                                    |  |  • Hardware: 18K Gold Plated Brass      |  |
|  |                                    |  |  • Absence of Chain: Confirmed          |  |
|  |                                    |  +-----------------------------------------+  |
|  |                                    |                                               |
|  +------------------------------------+  +-----------------------------------------+  |
|                                          |  FILMSTRIP CONTROLS:                    |  |
|                                          |  [ ← PREV ]               [ NEXT → ]    |  |
|                                          +-----------------------------------------+  |
|                                                                                       |
|  +---------------------------------------------------------------------------------+  |
|  |  FILMSTRIP NAVIGATOR (5 Miniaturas Sincronizadas con Indicador Activo)          |  |
|  |  [ C1-02 (Act) ]  [ C1-03 ]  [ C1-04 ]  [ C1-05 ]  [ C1-06 ]                    |  |
|  +---------------------------------------------------------------------------------+  |
+---------------------------------------------------------------------------------------+
```

##### Especificaciones Técnicas del Showcase:
1. **Contenedor `.stage-viewport`:**
   - Proporción `4 / 5`, altura máxima 720 px en desktop, ancho escalable entre 520 px y 640 px.
   - Transición de imagen: desvanecimiento cruzado suave (`opacity: 0` a `1` en 320 ms con curva `ease-in-out`).
   - La imagen nunca se deforma ni se corta con proporciones arbitrarias.
2. **Panel `.slide-metadata`:**
   - Renderiza dinámicamente los datos de la pieza activa: Código de Pieza, Título Oficial, Escena & Rol de Producto, Especificaciones Técnicas de Óptica y Acabado, y Función Comercial.
3. **Banda `.filmstrip-thumbnails`:**
   - 5 botones de miniatura (`aspect-ratio: 4/5`, ancho 100 px - 120 px).
   - La miniatura activa exhibe un borde luminoso dorado/cromo/rojo según la campaña, escala 1.05 y opacidad 1.0; las demás permanecen en opacidad 0.55 con transición de hover.
   - Controles de avance y retroceso (`← Anterior` / `Siguiente →`) accesibles por ratón, teclado (flechas izquierda/derecha al hacer foco) y gestos táctiles.

---

#### Módulo 3: The Editorial Diptych Spread ("Frames from the Night") (`.editorial-diptych`)
Este componente resuelve definitivamente el **Problema B**. Reemplaza el antiguo `.alts-grid` por una composición de doble página de revista de arte:
- **Estructura:** 2 columnas asimétricas o equilibradas para las piezas `-07` y `-08`.
- **Badges de Canal:**
  - Pieza 07: Badge superior `VIP LOOKBOOK ARCHIVE · PRIVATE COMMISSION`.
  - Pieza 08: Badge superior `EDITORIAL PRESS SPREAD · CULTURE & SOCIETY`.
- **Textos Contextuales:** Breve justificación curatorial al pie de cada imagen que explica por qué esa toma enriquece la narrativa de la campaña.
- **Bordes:** Enmarcadas con un borde perimetral metálico sutil (`border: 1px solid rgba(255, 255, 255, 0.12)`) que realza su carácter de pieza de archivo.

---

#### Módulo 4: Tarjeta de Intención y Valor Comercial (`.commercial-card`)
Este componente resuelve definitivamente el **Problema C**:
- **Grid de 3 Columnas Internas:**
  1. *Target Buyer & Psychographics:* Perfil sociodemográfico de la compradora, edad, hábitos culturales y motivaciones de estatus.
  2. *Commercial Trigger & Occasion:* Disparador de compra (gala, apertura de galería, recompensa personal, boda de destino).
  3. *Price Architecture & Engineering Rationale:* Rango de precio (ej. US$ 2.450 / US$ 2.850 / US$ 1.950), justificación de manufactura CNC frente a la marroquinería convencional y retorno de inversión estilística.

---

### 3.5. Sección de Auditoría y Verificación de Producción (`section.audit-master`)
- Cuadro de mando ejecutivo que detalla:
  - 24 imágenes generadas y aprobadas en 2K (8 por campaña).
  - Cero créditos pendientes, presupuesto ejecutado con precisión.
  - Registro de los 4 documentos estratégicos maestros (`CREATIVE_AUDIT_V3.md`, `CAMPAIGN_STRATEGY_AND_PUBLICATION_RATIONALE.md`, `HTML_REDESIGN_SPEC.md`, `ANTIGRAVITY_FINAL_EXECUTION.md`).
  - Botón de descarga/visualización del paquete de entrega `JEFFREY_LEVINSON_V3_DELIVERY`.

---

### 3.6. Pie de Página de Lujo (`footer.site-footer`)
- Sello de manufactura: `JEFFREY LEVINSON · CRAFTED IN PHILADELPHIA, USA`.
- Declaración de derechos de autor y enlace al repositorio oficial: `github.com/Paguro-Corp/JL_V1`.
- Créditos de dirección creativa y ejecución técnica: `Creative Direction: Paguro Creative · Technical Execution: Antigravity`.

---

## 4. Especificaciones Responsive por Breakpoint

### Breakpoint 1: Desktop Ultra-Wide & Standard (1440 px a 1920 px)
- Ancho máximo de contenedor: `1380 px` centrado con padding lateral de `48 px`.
- Tríptico Hero: 3 columnas de `400 px` cada una con altura fija de `580 px`.
- Showcase: Layout de 2 columnas lado a lado. Main Stage ocupa `560 px` de ancho; Panel de Metadatos ocupa `480 px` de ancho. Filmstrip con 5 miniaturas de `100 px` cada una.
- Díptico Editorial: 2 columnas de `580 px` con separación de `40 px`.

### Breakpoint 2: Laptop & Small Desktop (1024 px a 1280 px)
- Ancho máximo de contenedor: `980 px` centrado con padding lateral de `32 px`.
- Tríptico Hero: 3 columnas de `290 px` cada una con altura de `480 px`.
- Showcase: Main Stage de `460 px` de ancho; Panel de Metadatos lateral compacto. Filmstrip con 5 miniaturas de `84 px`.
- Díptico Editorial: 2 columnas de `450 px`.

### Breakpoint 3: Tablet Portrait (768 px a 1023 px)
- Padding lateral: `24 px`.
- Tríptico Hero: Pasa a un carrusel táctil horizontal o 3 filas verticales de altura reducida (`340 px`).
- Showcase: Pasa a flujo vertical apilado. Main Stage centrado con ancho de `80%` del viewport (`aspect-ratio: 4/5` preservado). Panel de Metadatos se ubica debajo del escenario. Filmstrip con scroll horizontal táctil y miniaturas de `90 px`.
- Díptico Editorial: Las 2 imágenes se apilan verticalmente con espaciado de `32 px`.

### Breakpoint 4: Mobile Standard (390 px a 430 px — iPhone 13/14/15/16)
- Padding lateral: `16 px`.
- Tríptico Hero: 1 columna de ancho completo con desplazamiento vertical suave o pestañas selectoras. Cada imagen mantiene su proporción `4:5`.
- Showcase:
  - Main Stage ocupa el `100%` del ancho disponible menos padding (aprox. `358 px` de ancho x `447 px` de alto).
  - Metadatos condensados en acordeón o tarjeta minimalista.
  - Filmstrip horizontal con `overflow-x: auto` y `scroll-snap-type: x mandatory` para navegación táctil fluida.
- Díptico Editorial: 1 columna vertical con espaciado de `24 px`.
- Tarjeta Comercial: Pasa de 3 columnas a 3 bloques verticales con micro-iconos.

---

## 5. Requerimientos de Performance y Accesibilidad (a11y)
1. **Optimización de Carga:**
   - Las 3 imágenes del Tríptico Hero (`c1_static.jpg`, `c2_static.jpg`, `c3_static.jpg`) llevan atributo `fetchpriority="high"`.
   - Las 21 imágenes restantes llevan atributo `loading="lazy"` y `decoding="async"`.
2. **Atributos ARIA y Semántica:**
   - Contenedor del Showcase con `role="region"` y `aria-label="Editorial Showcase"`.
   - Botones de miniatura con `aria-selected="true/false"` y textos descriptivos legibles para lectores de pantalla.
   - Navegación por teclado completa: flechas izquierda/derecha para alternar slides.

---
*Fin de la especificación técnica. Aprobado para implementación en `website/v3/index.html`.*
