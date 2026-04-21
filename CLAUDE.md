# CLAUDE.md — ANTop Satelital Presentation Project

## Qué es este proyecto

Presentación de ~45 minutos (~45 slides) sobre el trabajo final de carrera **"ANTop Satelital"**, en formato **HTML con animaciones**. El informe ya está escrito y aprobado. El objetivo es armar una presentación visual, minimalista y técnicamente rigurosa para una defensa formal ante jurado.

## Repositorios relevantes

- `/home/gstfrenkel/Desktop/tpp/satellite-antop` — biblioteca C++ de ANTop (algoritmo principal)
- `/home/gstfrenkel/Desktop/tpp/dtnsim` — simulador de redes DTN sobre OMNeT++
- `/home/gstfrenkel/Desktop/tpp/presentation/` — carpeta de trabajo de la presentación

## Archivos de referencia del informe (en /presentation)

| Archivo | Contenido |
|---|---|
| `intro.tex` | Introducción y motivación |
| `estado-del-arte.tex` | DTN, CGR, ANTop original |
| `h3.tex` | Sistema H3 y discretización |
| `antop.tex` | ANTop satelital: loop epoch, hipercubo, algoritmos |
| `dtnsim.tex` | dtnsim: arquitectura, limitaciones, integración |
| `simulaciones.tex` | Escenarios y resultados comparativos |
| `objectives.md` | Documento maestro de objetivos y estructura (leer siempre) |

## Flujo de trabajo acordado

1. Armar un `.md` de información técnica por cada sección (leyendo los `.tex` correspondientes)
2. Una vez que todos los `.md` estén listos, construir el HTML de la presentación sección por sección
3. Las animaciones se evalúan concepto por concepto al llegar a cada sección

**No saltar pasos.** No escribir HTML antes de tener el `.md` de la sección correspondiente aprobado.

## Estructura de la presentación

| # | Sección | Tiempo |
|---|---|---|
| 1 | Introducción | 3 min |
| 2 | Estado del arte (DTN, CGR, ANTop original) | 7 min |
| 3 | ANTop Satelital (hipercubo, loop epoch) | 7 min |
| 4 | H3 (hexágonos, coordenadas IJK, jerarquía) | 8 min |
| 5 | dtnsim (arquitectura, limitaciones, integración) | 8 min |
| 6 | Simulaciones y resultados | 7 min |
| 7 | Conclusiones y trabajo futuro | 4 min |

## Audiencia

- Jurado/docentes con conocimientos generales de redes.
- Sin conocimiento previo de DTN, CGR, ANTop ni H3.
- Todos los conceptos deben introducirse desde cero.
- Nivel técnico equivalente al del informe escrito.

## Lineamientos de diseño (no negociables)

- **Temática espacial/satelital**. Fondo oscuro.
- **Minimalista**: diagramas e imágenes dominan sobre el texto.
- **Animaciones** solo para ilustrar conceptos, no decorativas.
- **Proyección** en pantalla grande.
- **Sin preguntas retóricas** en la apertura.
- **Apertura**: imagen impactante de constelación satelital.
- Logos FIUBA/UBA: pendiente de incorporar.

## Conceptos que requieren explicación visual

| Concepto | Formato |
|---|---|
| Hipercubo de ANTop | Diagrama animado |
| H3: hexágonos vs cuadrados | Comparativa visual |
| H3: coordenadas IJK | Diagrama |
| H3: creación de direcciones ANTop | Diagrama animado |
| Loop epoch | Ejemplo animado breve |
| Constelación LEO | Imagen/animación de apertura |
| Resultados ANTop vs CGR | Gráficas de simulación |

## Lo que NO hacer

- No mencionar OMNeT++ en profundidad (sí mencionar dtnsim).
- No usar preguntas retóricas que asuman conocimiento previo del público.
- No hacer slides con mucho texto.
- No agregar animaciones decorativas.
- No construir el HTML antes de tener el .md de la sección aprobado.

## Autores

- **Gastón Frenkel**
- **Valentina Adelsflügel**

## Estado del HTML (`presentation/index.html`)

| Sección | Estado |
|---|---|
| Introducción (slides 1–3) | ✅ Construida y aprobada |
| Estado del arte (slides 4–13) | ✅ Construida y aprobada |
| ANTop Satelital (slides 14–19) | ✅ Construida y aprobada |
| H3 (slides H3-1 a H3-10) | ✅ Construida y aprobada |
| dtnsim | ⬜ Pendiente |
| Simulaciones y resultados | ⬜ Pendiente |
| Conclusiones | ⬜ Pendiente |

## Estructura HTML de `index.html`

### Convenciones de navegación
- Secciones: `<!-- ═══════════════════ SECCIÓN N: NOMBRE ═══════════════════ -->`
- Slides numerados: `<!-- ─────────────────────── SLIDE N — NOMBRE ─────────────────────────── -->`
- Slides con ID alfanumérico: `<!-- ─── SLIDE H3-1 — NOMBRE ────────────────────── -->`
- Slides con sufijo: `<!-- ─── SLIDE 17b — NOMBRE ──── -->`
- Grupos de slides: `<!-- ─── SLIDES LP1–LP5 — NOMBRE ──────────── -->`

### Slides actuales por sección

**Sección 1 — Introducción**
- SLIDE 1 — Apertura (starfield + constelación orbital)
- SLIDE 2 — El problema (ISL intermitentes)
- SLIDE 3 — La solución

**Sección 2 — Estado del arte**
- SLIDE 4 — DTN (store-carry-forward)
- SLIDE 5 — CGR
- SLIDE 6 — Limitaciones CGR
- SLIDE 7 — ANTop (hipercubo estático)
- SLIDES 8–11 — Ruteo greedy (paso a paso)
- SLIDE 12 — Reactivo vs proactivo
- SLIDE 13 — Tablas ANTop (hipercubo de referencia)

**Sección 3 — ANTop Satelital**
- SLIDE 14 — El desafío (hipercubo con conectividad intermitente)
- SLIDE 15 — Detección de bucles
- SLIDES LP1–LP5 — Bucle en ANTop original (demo animada, 5 pasos)
- SLIDES PP0–PP5 — Propuesta: ruteo alternativo (demo animada, 6 pasos)
- SLIDE 16 — Paquetes del mismo flujo
- SLIDES 17 / 17b / 17c — Loop epoch: mecanismo (3 estados)
- SLIDE 18 — Falsos positivos
- SLIDE 19 — Transición a H3

**Sección 4 — H3**
- SLIDE H3-1 — Por qué hexágonos
- SLIDE H3-3 — Sistema IJK
- SLIDE H3-4 — Construcción ANTop: concepto
- SLIDE H3-5 — Paso 1: descomposición
- SLIDE H3-6 — Paso 2: codificación en bits
- SLIDE H3-7 — Paso 3: compresión en bytes
- SLIDE H3-8 — Dimensión prima
- SLIDE H3-9 — Origen del hipercubo
- SLIDE H3-10 — Medición del error

### Estructura CSS/JS
- Estilos globales y por sección: bloque `<style>` al inicio
- Cada sección tiene su propio bloque CSS marcado con `/* ═══ SECCIÓN N ═══ */`
- Animaciones JS: bloque `<script>` al final, organizado por slide (`// ── SLIDE N ──`)

## Sistema visual establecido

**Tipografía**
- Títulos: `Space Grotesk` (Google Fonts), weight 600–700
- Cuerpo: `Inter` (Google Fonts), weight 300–500

**Paleta de colores**
- Fondo: `#060b18`
- Azul acento: `#38bdf8`
- Azul medio: `#0ea5e9`
- Púrpura: `#818cf8`
- Texto: `#f1f5f9`
- Texto muted: `#94a3b8`
- Dim: `#3f4f67`

**Animaciones orbitales**
- Geometría 3D real: órbitas circulares definidas por `rx` (radio), `inc` (inclinación), `lan` (longitud del nodo ascendente)
- Orden de dibujo: back arc → satélites z<0 → Tierra → front arc → satélites z≥0
- Brillo dinámico: `alpha = 0.15 + 0.85 × (z/rx + 1) / 2` (continuo, sin saltos)
- Condición para que back arc pase por detrás del disco terrestre: `rxF × cos(inc) < ER_frac`

## Pendientes

- División de slides entre co-autores (a definir más adelante).
- Logos institucionales FIUBA/UBA (a incorporar más adelante).
- Detalle de escenarios de simulación (al armar sección 6).
- Conclusiones y trabajo futuro (al armar sección 7).
