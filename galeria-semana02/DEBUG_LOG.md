# Bitácora de Depuración y Validación - Práctica Semana 02

**Asignatura:** Desarrollo de Aplicaciones Web (IS093A)  
**Facultad:** Facultad de Ingeniería de Sistemas — UNCP  
**Integrantes:**

- ANDRADE-EGOAVIL (HTML · A11y · SEO)
- VEGA-SANABRIA (CSS · Layout · Debug)

---

## 1. Registro de Errores Encontrados y Soluciones Manuales

### Error 1: Legibilidad y Contraste de Colores en Botones y Enlaces

- **Herramienta:** WAVE Accessibility Tool / Chrome DevTools A11y.
- **Tipo de Error:** _Very Low Contrast Ratio_.
- **Causa:** En la propuesta visual inicial tipo _blueprint_, el azul primario (`#1d4ed8`) no alcanzaba el ratio WCAG AA sobre fondos oscuros en el modo `prefers-color-scheme: dark`.
- **Corrección Manual:** Se declararon variables independientes en `:root` para modo claro y modo oscuro. En el bloque `@media (prefers-color-scheme: dark)`, se ajustó `--primary-color` al tono `#7aa2ff` sobre la superficie `#171f30`, asegurando un ratio de contraste mínimo superior a 4.5:1.

### Error 2: Falta de Enlace de Salto Directo (Skip-Link)

- **Herramienta:** WAVE Accessibility Tool / Pruebas de Navegación por Teclado (`Tab`).
- **Tipo de Error:** _Missing Keyboard Navigation Element_.
- **Causa:** Al navegar mediante el teclado, el foco debía recorrer los enlaces del encabezado antes de llegar al contenido del portafolio.
- **Corrección Manual:** Se incluyó `<a href="#main-content" class="skip-link">Saltar al contenido principal</a>` al inicio del `<body>` en `index.html`. En `styles.css` se configuró fuera de pantalla (`top: -100px`) y se renderiza visiblemente (`top: 1rem`) únicamente cuando recibe el foco mediante la pseudoclase `:focus`.

### Error 3: Estructura y Jerarquía de Encabezados Sanitizada

- **Herramienta:** Google Lighthouse (Auditoría de Accesibilidad / SEO).
- **Tipo de Error:** _Heading elements are not in a sequentially-descending order_.
- **Causa:** La sección principal carecía de un título descriptivo de nivel `<h2>` previo a la estructura de las tarjetas en `<article>`.
- **Corrección Manual:** Se ordenó jerárquicamente la estructura HTML:
  - `<h1>` para el título general ("Galería de Proyectos Técnicos").
  - `<h2>` para las secciones principales (`#section-title` e información de integrantes).
  - `<h3>` para el título interno de cada tarjeta (`.card-header h3`).

---

## 2. Decisiones de Arquitectura y Maquetado CSS

- **Layout Híbrido:**
  - **Estructura Global:** Maquetada con **CSS Grid** (`.main-layout`), definiendo un sistema de dos columnas asimétricas (`minmax(0, 3fr)` y `minmax(0, 1fr)`) activadas mediante `@media (min-width: 56.25rem)`.
  - **Galería Masonry pura (Sin JS):** Implementada mediante CSS Multi-column (`columns: 1` a `columns: 2` con `column-gap` y `break-inside: avoid` en cada `.card`), permitiendo la alineación fluida de tarjetas de diferente altura.
- **Tipografía Fluida y Unidades Lógicas:**
  - Declaración de `clamp()` en variables para títulos (`--fs-title`, `--fs-subtitle`, `--fs-body`) integrando tipografía serif técnica (_Fraunces_) e sans-serif (_Inter_).
  - Uso de propiedades lógicas (`padding-block`, `padding-inline`, `margin-block-end`, `border-block-end`) para garantizar compatibilidad con internacionalización y modos de escritura.
- **Accesibilidad e Accesibilidad Cognitiva:**
  - Implementación de `@media (prefers-reduced-motion: reduce)` para desactivar transiciones suaves y animaciones en usuarios con sensibilidad al movimiento.

---

## 3. Reporte de Compatibilidad Cruzada (Can I Use)

- **Propiedades Utilizadas:** CSS Grid Areas, Variables CSS (`var()`), `clamp()`, `columns` (Multi-column layout).
- **Compatibilidad:** Soportado en el 98% + de navegadores globales (Chrome 105+, Edge 105+, Safari 16+, Firefox 110+).
- **Estrategia Fallback:** Para pantallas de menor resolución, CSS Columns degrada automáticamente a 1 sola columna vertical sin romper el flujo del DOM.

---

## 4. Resultados de Validaciones (Antes y Después)

| Herramienta                    | Resultado Inicial                               | Resultado Final                       |
| :----------------------------- | :---------------------------------------------- | :------------------------------------ |
| **W3C HTML Validator**         | 2 Advertencias de estructura                    | **0 Errores / 0 Advertencias**        |
| **WAVE Tool**                  | 2 Errores de contraste / 1 Alerta de navegación | **0 Errores / 0 Fallos de contraste** |
| **Lighthouse (SEO)**           | 85 / 100                                        | **100 / 100**                         |
| **Lighthouse (Accesibilidad)** | 82 / 100                                        | **100 / 100**                         |