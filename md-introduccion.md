# Sección 1: Introducción
**Tiempo estimado:** 3 minutos | **Slides estimadas:** 3

---

## Narrativa

La introducción debe establecer el problema de forma visual e inmediata, sin asumir conocimiento previo. El hilo conductor es:

> Las redes satelitales LEO son el futuro de las comunicaciones globales — pero rutear datos en ellas es fundamentalmente distinto a lo que conocemos en Internet.

No hay definiciones técnicas en esta sección. El objetivo es generar contexto e interés.

---

## Slide 1 — Apertura visual

**Contenido:** Imagen de alta calidad de una constelación LEO (ej: Starlink) orbitando la Tierra. Título del trabajo sobre la imagen.

- Título: **ANTop Satelital**
- Subtítulo: nombre de los autores, institución, año
- Sin texto adicional

**Nota de diseño:** Esta slide debe impactar visualmente. La imagen ocupa toda la pantalla. El texto va superpuesto con tipografía clara sobre zona oscura.

---

## Slide 2 — El problema: redes LEO y sus desafíos

**Contenido:** Presentar por qué las redes LEO son un desafío para los protocolos tradicionales.

Puntos clave a transmitir:
- Los satélites en órbita baja (LEO) se mueven continuamente → la topología de la red cambia todo el tiempo.
- Los enlaces entre satélites son intermitentes: dos satélites se "ven" solo durante ventanas de tiempo limitadas.
- Las latencias de propagación varían dinámicamente.
- Los protocolos de Internet tradicionales (TCP/IP) asumen conectividad estable y continua → **no funcionan** en este entorno.

**Elemento visual sugerido:** Diagrama animado simple mostrando dos satélites LEO orbitando, con un enlace que aparece y desaparece a medida que se cruzan/alejan. Transmite la idea de conectividad intermitente sin necesidad de texto.

---

## Slide 3 — La solución: DTN y el objetivo del trabajo

**Contenido:** Introducir DTN como la arquitectura adecuada para estos entornos, y plantear el objetivo del trabajo.

Puntos clave:
- Para estos entornos existe una arquitectura de red específica: **DTN (Delay/Disruption Tolerant Networking)** — redes tolerantes a demoras e interrupciones.
- Dentro de DTN, el protocolo de ruteo más usado hoy es **CGR (Contact Graph Routing)**: efectivo, pero con limitaciones de escalabilidad.
- Este trabajo propone y evalúa **ANTop** como alternativa: un enfoque descentralizado donde cada satélite toma decisiones de ruteo usando solo información local.

**Elemento visual sugerido:** Diagrama de bloques simple con dos componentes: `DTN` → `ANTop` → `Comparación vs CGR`. Sirve como mapa conceptual del trabajo.

---

## Conceptos introducidos en esta sección

| Concepto | Definición rápida |
|---|---|
| LEO | Órbita baja terrestre (~200–2000 km). Satélites en movimiento continuo. |
| Topología dinámica | La estructura de la red cambia con el movimiento orbital. |
| Enlace intermitente | La conexión entre dos nodos existe solo en ventanas de tiempo. |
| DTN | Arquitectura de red diseñada para alta latencia e intermitencia. |
| CGR | Protocolo de ruteo estándar en DTN. Usa conocimiento global de contactos futuros. |
| ANTop | Protocolo alternativo. Ruteo local sin conocimiento global. |

---

## Lo que NO va en esta sección

- Explicación técnica de CGR (va en Estado del Arte).
- Explicación de H3 (va en su propia sección).
- Detalles del hipercubo o loop epoch (van en ANTop Satelital).
- Resultados o métricas.
