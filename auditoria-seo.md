# Auditoría SEO + AI Visibility — victormartincentro.com
**Fecha:** 2026-06-16 · **Tier de datos:** 0 (WebFetch estático, sin PSI ni Search Console) · **Findings:** 34

---

## Scores Globales

| Score | Valor | Banda | Interpretación |
|---|---|---|---|
| 🔍 **Search SEO** | **34.5 / 100** | **F** | Problemas fundacionales — corregir indexabilidad y estructura primero |
| 🤖 **AI Visibility** | **32.3 / 100** | **F** | Problemas fundacionales — añadir schema, estructura y bloques de respuesta |

> **Diagnóstico conjunto:** El sitio tiene un diseño visual sólido y copy bien escrito, pero ambas dimensiones están en rojo por las mismas causas: ausencia total de schema, contenido placeholder visible en producción, sin robots.txt ni sitemap, y sin señales de autoridad E-E-A-T. Son todos issues directamente accionables.

---

## Breakdown por Categoría

### Search SEO (34.5/100)

| Categoría | Peso | Valor | Estado |
|---|---|---|---|
| Indexability & Crawl | 22 | 25.0 | 🔴 Crítico |
| Core Web Vitals / Performance | 16 | 75.0 | 🟡 Aceptable |
| On-Page & Meta | 12 | 53.3 | 🟠 Mejorable |
| Structured Data | 12 | 0.0 | 🔴 Crítico |
| Rendering | 8 | 100.0 | 🟢 OK |
| Internal Linking & Semantics | 8 | 50.0 | 🟠 Mejorable |
| E-E-A-T | 7 | 0.0 | 🔴 Crítico |
| Images / Media | 5 | 0.0 | 🔴 Crítico |
| Sitemaps & Discovery | 5 | 0.0 | 🔴 Crítico |
| Freshness | 3 | 66.7 | 🟡 Aceptable |
| Social Cards | 2 | 0.0 | 🔴 Crítico |
| **Local** *(activo — negocio local en Vigo)* | 10 | 0.0 | 🔴 Crítico |

### AI Visibility (32.3/100)

| Categoría | Peso | Valor | Estado |
|---|---|---|---|
| Answer Extractability | 20 | 55.6 | 🟠 Mejorable |
| Structured Data | 16 | 0.0 | 🔴 Crítico |
| Fact Density / Original Data | 14 | 0.0 | 🔴 Crítico |
| AI Crawler Access | 12 | 60.0 | 🟡 Aceptable |
| Rendering (non-JS) | 10 | 100.0 | 🟢 OK |
| Entity / Knowledge-Graph | 10 | 0.0 | 🔴 Crítico |
| E-E-A-T / Authority | 9 | 0.0 | 🔴 Crítico |
| Freshness | 6 | 66.7 | 🟡 Aceptable |
| Images / Multimodal | 3 | 0.0 | 🔴 Crítico |
| llms.txt | 0 | 0.0 | (reportado, peso 0) |

---

## Issues Encontrados — Completo

### 🔴 CRÍTICOS (severity 4–5)

---

#### F-01 · Cero JSON-LD Schema en toda la página
**Módulo:** M5 · **Scope:** Site · **Fixable:** proposed

**Evidencia:** `grep 'application/ld+json' index.html` → 0 resultados. No existe ningún bloque de datos estructurados en el HTML publicado.

**Impacto:** Este es el gap más grande de todo el sitio. Sin schema, Google no puede construir Knowledge Panel, no aparecerá en el Local Pack de "masajes Vigo", no hay rich results, y los LLMs no pueden citar el centro con confianza. Afecta tanto al Search SEO (Structured Data: 0/100) como a la AI Visibility (Structured Data: 0/100, Entity: 0/100).

**Fix propuesto:** Añadir en `<head>` un bloque JSON-LD con:
- `HealthAndBeautyBusiness` (subtipo de `LocalBusiness`) — name, address, telephone, geo, openingHours, priceRange, url, sameAs
- `Person` (Víctor Martín) — jobTitle, knowsAbout, worksFor
- `ItemList` de servicios con `Service` por modalidad
- `FAQPage` (ver F-05)

**Ejemplo base:**
```json
{
  "@context": "https://schema.org",
  "@type": "HealthAndBeautyBusiness",
  "name": "Víctor Martín Centro",
  "url": "https://victormartincentro.com",
  "telephone": "+34685106677",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Vigo",
    "addressRegion": "Galicia",
    "addressCountry": "ES"
  },
  "priceRange": "30€",
  "currenciesAccepted": "EUR",
  "employee": {
    "@type": "Person",
    "name": "Víctor Martín",
    "jobTitle": "Terapeuta de Bienestar Integral",
    "knowsAbout": ["masajes terapéuticos", "acupuntura", "psicoterapia", "kinesiología", "reflexología podal"]
  }
}
```
*(Completar con dirección real, horarios y sameAs cuando estén disponibles)*

---

#### F-02 · Credenciales de Víctor son placeholders visibles en producción
**Módulo:** M16 · **Scope:** Page · **Fixable:** advisory

**Evidencia:** El HTML publicado contiene literalmente:
```
"[Escuela/Universidad]"
"[Formación y colegiación]"
"[Formación]"
"+X años de práctica clínica"
```

**Impacto:** Daño severo al E-E-A-T (0/100). Google evalúa explícitamente la verificabilidad de las credenciales en contenido de "Your Money or Your Life" (YMYL — salud/terapias es YMYL). Un usuario que vea estos placeholders perderá la confianza inmediatamente. Los LLMs no citarán un terapeuta sin credenciales verificables.

**Fix requerido (contenido de Víctor):**
- Nombre real de la escuela/universidad de acupuntura
- Número de colegiación de psicoterapia (obligatorio legalmente en España)
- Años concretos de experiencia
- Foto profesional real

---

#### F-03 · Sin sección FAQ — sin oportunidad de featured snippets ni AI answers
**Módulo:** M11 · **Scope:** Page · **Fixable:** proposed

**Evidencia:** No existe ninguna sección de preguntas frecuentes ni schema FAQPage. Queries directamente perdibles: "¿qué es la kinesiología?", "¿cuánto cuesta un masaje en Vigo?", "diferencia acupuntura y acupuntura zonal", "¿la primera consulta es gratuita?".

**Impacto:** La FAQ es la táctica de mayor ROI para AI Visibility (Answer Extractability: 55.6/100 → podría llegar a 90+). Los LLMs responden preguntas directas con fragmentos de FAQ. También genera featured snippets en Google.

**Fix propuesto:** Añadir sección FAQ antes del CTA final con 6-8 preguntas + schema FAQPage.

**Preguntas sugeridas:**
1. ¿Qué es la kinesiología y para qué sirve?
2. ¿Cuánto cuesta una sesión?
3. ¿La primera consulta es realmente gratuita?
4. ¿Qué diferencia hay entre acupuntura y acupuntura zonal?
5. ¿Puedo combinar varios tratamientos en una misma sesión?
6. ¿Dónde estáis ubicados en Vigo?
7. ¿Cómo reservo una cita?
8. ¿Qué problemas trata la psicoterapia en el centro?

---

#### F-04 · Sin schema LocalBusiness — pérdida total de Local SEO
**Módulo:** M19 · **Scope:** Site · **Fixable:** proposed

**Evidencia:** Centro de bienestar físico en Vigo sin ningún schema `HealthAndBeautyBusiness`. Google no puede leer nombre, dirección, teléfono, precios ni horarios de forma estructurada.

**Impacto:** El Local Pack de Google ("masajes Vigo", "acupuntura Vigo en Google Maps") requiere concordancia entre schema web, GBP y NAP. Sin schema, el centro no puede aparecer en resultados locales aunque alguien busque exactamente sus servicios.

**Fix:** Incluido en F-01 (JSON-LD general). Añadir dirección física completa en cuanto esté disponible.

---

### 🟠 ALTOS (severity 3)

---

#### F-05 · robots.txt ausente (404)
**Módulo:** M1 · **Scope:** Site · **Fixable:** auto

**Evidencia:** `curl -I https://victormartincentro.com/robots.txt` → 404

**Fix automático — crear `/public/robots.txt`:**
```
User-agent: *
Allow: /

Sitemap: https://victormartincentro.com/sitemap.xml
```

---

#### F-06 · sitemap.xml ausente (404)
**Módulo:** M17 · **Scope:** Site · **Fixable:** auto

**Evidencia:** `curl -I https://victormartincentro.com/sitemap.xml` → 404

**Fix automático — crear `/public/sitemap.xml`:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://victormartincentro.com/</loc>
    <lastmod>2026-06-16</lastmod>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

---

#### F-07 · Falta etiqueta canonical
**Módulo:** M2 · **Scope:** Page · **Fixable:** auto

**Evidencia:** `grep -i canonical index.html` → 0 resultados

**Fix automático:** Añadir en `<head>`:
```html
<link rel="canonical" href="https://victormartincentro.com" />
```

---

#### F-08 · og:image ausente — sin preview al compartir por WhatsApp
**Módulo:** M7/M8 · **Scope:** Page · **Fixable:** proposed

**Evidencia:** OG tags presentes (title, description, type, url) pero `og:image` ausente. El principal canal de reservas es WhatsApp — cuando alguien reenvía el enlace, no aparece ninguna imagen de previsualización.

**Fix:** Diseñar imagen 1200×630px (fondo oscuro #1e1e1e, monograma VM, tagline, nombre del centro). Alojar en `/og-image.jpg`. Añadir:
```html
<meta property="og:image" content="https://victormartincentro.com/og-image.jpg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta property="og:image:alt" content="Víctor Martín Centro — Bienestar Integral en Vigo" />
```

---

#### F-09 · Sin Google Business Profile integrado
**Módulo:** M19 · **Scope:** Site · **Fixable:** advisory

**Evidencia:** No hay enlace a perfil de Google Maps, widget de reviews ni mención de GBP.

**Impacto:** El GBP es el principal driver de visibilidad local (aparece antes que la web en búsquedas locales). Sin GBP verificado, el centro es invisible en Google Maps.

**Acción:** Crear/reclamar GBP en `business.google.com` con exactamente la misma NAP (Name, Address, Phone) que el sitio web.

---

#### F-10 · Sin dirección física en la página
**Módulo:** M12 · **Scope:** Page · **Fixable:** advisory

**Evidencia:** La página indica solo "Vigo, Galicia". Sin calle, número, código postal ni barrio.

**Impacto:** Los motores de búsqueda locales y los LLMs no pueden geolocalizar el centro con precisión. Crítico para "masajes cerca de mí" y Local Pack.

---

#### F-11 · Testimonios explícitamente marcados como placeholders
**Módulo:** M16 · **Scope:** Page · **Fixable:** advisory

**Evidencia:** El texto `[Testimonios reales de pacientes pendientes de recopilación · Placeholders de ejemplo]` está visible en el HTML publicado en producción.

**Impacto:** Daño directo a la confianza del usuario. Cualquier bot de rastreo puede leer este texto. Reemplazar con al menos 3 testimonios reales antes de activar campañas de tráfico.

---

#### F-12 · Sin política de privacidad — GA activo sin consentimiento GDPR
**Módulo:** M16 · **Scope:** Site · **Fixable:** advisory

**Evidencia:** Google Analytics (G-FCJE462HH9) cargando en todas las visitas. No hay banner de cookies ni enlace a política de privacidad.

**Impacto:** Riesgo legal RGPD (multas hasta 20M€ / 4% facturación). Google también valora la transparencia como señal de confianza. El RGPD es explícitamente parte del Quality Rater Guidelines de Google.

**Fix:** Crear `/politica-privacidad.html`. Implementar banner de consentimiento (CookieYes, Cookiebot o similar, con tier gratuito). Mover GA a carga condicional post-consentimiento.

---

#### F-13 · Sin imagen real del terapeuta
**Módulo:** M9 · **Scope:** Page · **Fixable:** advisory

**Evidencia:** La sección de Víctor Martín muestra un SVG placeholder con "Foto de Víctor próximamente". No hay ningún `<img>` real en todo el documento.

**Impacto:** Las imágenes de personas reales son el signal de E-E-A-T más directo para Google. También mejoran la tasa de conversión en un 20-30% en servicios de salud/bienestar (sector donde la confianza personal es decisiva).

---

### 🟡 MEDIOS (severity 2)

---

#### F-14 · Twitter/X card meta tags ausentes
**Módulo:** M7 · **Scope:** Page · **Fixable:** auto

**Fix automático:**
```html
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Víctor Martín Centro — Bienestar Integral en Vigo" />
<meta name="twitter:description" content="El único centro en Vigo que integra masajes terapéuticos, acupuntura, psicoterapia y kinesiología. 30€/sesión." />
<meta name="twitter:image" content="https://victormartincentro.com/og-image.jpg" />
```

---

#### F-15 · Section labels usan `<p>` en lugar de headings semánticos
**Módulo:** M7c · **Scope:** Page · **Fixable:** proposed

**Evidencia:** "Por qué Víctor Martín Centro", "Servicios", "El terapeuta", "Testimonios" se renderizan como `<p class="section-label">` sin valor semántico de heading.

**Fix propuesto:** Cambiar a `<h3 class="section-label">` manteniendo estilos CSS. Sin impacto visual, mejora semántica.

---

#### F-16 · Tamaños de fuente por debajo de 12px
**Módulo:** M7b · **Scope:** Page · **Fixable:** proposed

**Evidencia:** CSS declara `font-size: 0.6rem` (≈9.6px), `0.62rem`, `0.65rem`, `0.68rem` en hero-label, nav-cta, section-label, footer-copy.

**Fix propuesto:** Incrementar a mínimo `0.75rem` (12px) todos los elementos de texto visible.

---

#### F-17 · llms.txt ausente
**Módulo:** M21/M14 · **Scope:** Site · **Fixable:** auto

**Evidencia:** `curl -I https://victormartincentro.com/llms.txt` → 404

**Fix automático — crear `/public/llms.txt`:**
```markdown
# Víctor Martín Centro

Centro de bienestar integral en Vigo (Galicia, España).

## Servicios
- Masajes terapéuticos (relajante, descontracturante, deportivo) — 30€/sesión
- Acupuntura — 30€/sesión
- Cráneo puntura — 30€/sesión
- Acupuntura zonal — 30€/sesión
- Psicoterapia (ansiedad, estrés, bloqueos emocionales) — 30€/sesión
- Kinesiología — 30€/sesión
- Reflexología podal — 30€/sesión
- Primera consulta orientativa — Gratuita

## Contacto
- Teléfono/WhatsApp: +34 685 106 677
- Web: https://victormartincentro.com
- Localización: Vigo, Galicia, España

## Propuesta de valor
Único centro en Vigo que integra tratamiento físico (masajes, kinesiología), energético (acupuntura) y mental (psicoterapia) en un mismo espacio con un solo terapeuta.
```

---

#### F-18 · Google Fonts externo — posible render-blocking
**Módulo:** M15 · **Scope:** Page · **Fixable:** proposed

**Evidencia:** `<link href="https://fonts.googleapis.com/css2?family=...&display=swap">` — recurso externo. Preconnect presente, display=swap correcto.

**Impacto estimado (needs PSI para confirmar):** Cormorant Garamond + Raleway pueden añadir 100-300ms de bloqueo en conexiones lentas.

**Fix propuesto:** Self-host las fuentes con `@font-face` local para eliminar dependencia de terceros y mejorar LCP.

---

#### F-19 · Sin fecha de publicación/actualización en meta
**Módulo:** M13 · **Scope:** Page · **Fixable:** proposed

**Fix propuesto:** Añadir en JSON-LD (como parte de F-01):
```json
"datePublished": "2026-06-01",
"dateModified": "2026-06-16"
```

---

#### F-20 · Sin horario de atención en la página
**Módulo:** M12 · **Scope:** Page · **Fixable:** advisory

**Evidencia:** "Horario flexible" mencionado pero sin días/horas concretas. No crawleable.

---

### 🟢 PUNTOS FUERTES (passes notables)

| Finding | Módulo | Detalle |
|---|---|---|
| HTML estático — todo indexable sin JS | M3/M4 | Todo el contenido visible en raw HTML. Excelente para crawlers y AI. |
| Title tag optimizado | M7 | 56 chars, incluye marca y ciudad. |
| Meta description con precio y CTA | M7 | 147 chars, incluye "30€/sesión", diferenciador y acción. |
| H1 único y descriptivo | M7c | Incluye "Vigo" y propuesta de valor diferencial. |
| Precio 30€ claramente extraíble | M11 | Aparece 8+ veces, inequívoco para AI. |
| CSS completamente inline | M15 | Cero requests externos de CSS → FCP rápido. |
| Copyright 2026 actual | M13 | Señal de frescura básica. |
| Contenido accesible para AI crawlers | M14 | Sin JS requerido para leer servicios/precios. |
| Headers de seguridad en vercel.json | — | X-Content-Type-Options, X-Frame-Options: DENY, Referrer-Policy presentes. |
| WhatsApp floating button | — | CTA de conversión persistente, bueno para mobile UX. |

---

## Fixes Automáticos Disponibles

Estos pueden aplicarse con `/claude-seo-ai:fix` sin necesidad de contenido adicional:

| # | Fix | Archivo | Tipo |
|---|---|---|---|
| A1 | `robots.txt` con User-agent: * Allow: / + sitemap ref | `public/robots.txt` (nuevo) | auto |
| A2 | `sitemap.xml` con URL canónica y lastmod | `public/sitemap.xml` (nuevo) | auto |
| A3 | `<link rel="canonical">` en `<head>` | `index.html` | auto |
| A4 | Twitter card meta tags (4 líneas) | `index.html` | auto |
| A5 | `llms.txt` básico | `public/llms.txt` (nuevo) | auto |

**Fixes propuestos** (requieren assets o datos de Víctor pero código generado automáticamente):

| # | Fix | Bloqueante |
|---|---|---|
| P1 | JSON-LD schema completo (LocalBusiness + Person + Service + FAQ) | Dirección real, horarios, credenciales |
| P2 | og:image meta tags | Imagen social 1200×630 (diseño requerido) |
| P3 | Section labels `<p>` → `<h3>` | Ninguno (solo semántico) |
| P4 | Font sizes mínimo 0.75rem | Ninguno |
| P5 | Self-hosting Google Fonts | Archivos de fuente descargados |
| P6 | FAQ section + schema FAQPage | Respuestas de Víctor |
| P7 | datePublished/dateModified en JSON-LD | Fecha real de publicación |

---

## Pendientes que Requieren Contenido Real

Estos issues no pueden resolverse con código — requieren información de Víctor:

| Pendiente | Urgencia | Impacto |
|---|---|---|
| **Dirección física completa** (calle, nº, CP) | Alta | Local Pack, schema, GBP |
| **Horario de atención** (días y horas) | Alta | Local SEO, schema, GBP |
| **Credenciales reales** (escuela, colegiación, años) | Alta | E-E-A-T, YMYL compliance |
| **Foto profesional de Víctor** | Alta | E-E-A-T, conversión |
| **Testimonios reales de pacientes** | Alta | Confianza, E-E-A-T |
| **Texto biográfico completo** | Media | E-E-A-T, entity authority |
| **Google Business Profile creado/reclamado** | Alta | Local Pack, Google Maps |
| **Política de privacidad** | Alta (legal) | RGPD compliance, trust |
| **Páginas individuales por servicio** | Media (futuro) | SEO long-tail |

---

## Top 10 Prioridades por Impacto ÷ Esfuerzo

| # | Prioridad | Impacto | Esfuerzo | Tipo | Score afectado |
|---|---|---|---|---|---|
| **1** | **JSON-LD schema** (LocalBusiness + Person + Service) | 🔴 Muy alto | Bajo (código) | proposed | Search +15 / AI +18 |
| **2** | **Contenido real de Víctor** (credenciales, bio, foto) | 🔴 Muy alto | Medio (Víctor) | advisory | Search +8 / AI +12 |
| **3** | **robots.txt + sitemap.xml** | 🟠 Alto | Muy bajo | auto | Search +5 |
| **4** | **canonical tag** | 🟠 Alto | Muy bajo | auto | Search +3 |
| **5** | **og:image + Twitter card** | 🟠 Alto | Bajo (diseño) | proposed | Search +4 / conversión |
| **6** | **Testimonios reales** (reemplazar placeholders) | 🟠 Alto | Medio (Víctor) | advisory | Search +4 / AI +5 |
| **7** | **FAQ section + schema FAQPage** | 🟠 Alto | Medio | proposed | AI +15 / Search +4 |
| **8** | **Google Business Profile** (crear/verificar) | 🟠 Alto | Bajo (admin) | advisory | Local SEO crítico |
| **9** | **Política de privacidad + banner cookies** | 🟠 Alto (legal) | Medio | advisory | Compliance RGPD |
| **10** | **llms.txt** | 🟡 Medio | Muy bajo | auto | AI visibility (futuro) |

---

## Detalles Técnicos Adicionales

### Seguridad (vercel.json)
✅ `X-Content-Type-Options: nosniff`
✅ `X-Frame-Options: DENY`
✅ `Referrer-Policy: strict-origin-when-cross-origin`
⚠️ Falta: `Content-Security-Policy` (recomendado para bloquear XSS)
⚠️ Falta: `Permissions-Policy` (recomendado)
ℹ️ HSTS: Vercel lo gestiona a nivel de plataforma por defecto

### Analytics
- Google Analytics 4 (G-FCJE462HH9) activo
- Sin consentimiento de cookies → riesgo RGPD
- Sin eventos de conversión configurados visibles (WhatsApp clicks, ver servicios) → oportunidad de tracking

### Vercel Config
- `cleanUrls: true` ✅
- `trailingSlash: false` ✅
- Falta: redirect www → apex (no se puede verificar sin DNS access)

### Accesibilidad (observada, no auditada en detalle)
- SVG monograma sin `aria-label` ni `<title>`
- SVG en sección Víctor sin descripción accesible
- Botón WhatsApp flotante tiene `aria-label="Contactar por WhatsApp"` ✅
- Contraste: texto cream `#e8ddd4` sobre fondo `#1e1e1e` → relación ~11:1 ✅ Excelente
- Fuentes pequeñas (0.6rem) pueden ser problema para usuarios con baja visión

---

## Proyección de Scores Post-Fix

Si se implementan los fixes A1–A5 + P1–P3 + contenido real de Víctor:

| Score | Actual | Post-fix automático | Post-fix completo |
|---|---|---|---|
| Search SEO | 34.5 (F) | ~52 (F→D) | ~74 (C) |
| AI Visibility | 32.3 (F) | ~45 (F→D) | ~71 (C) |

*(Proyección basada en severity × weight de findings resueltos. Métricas PSI pueden modificar Core Web Vitals.)*

---

*Auditoría generada por claude-seo-ai · Tier 0 (sin PSI ni Search Console) · victormartincentro.com · 2026-06-16*
*Para aplicar fixes automáticos: `/claude-seo-ai:fix https://victormartincentro.com --dry-run`*
