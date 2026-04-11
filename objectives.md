# Objetivos de la Presentación — ANTop Satelital

## Contexto General

Este trabajo implementa el protocolo **ANTop (Adjacent Network Topology)** para redes de satélites LEO (Low Earth Orbit), integrando el sistema de indexación geoespacial **H3** de Uber como mecanismo de representación espacial, y evaluando su desempeño frente a **CGR (Contact Graph Routing)** mediante el simulador **dtnsim** sobre OMNeT++.

---

## Objetivo Principal

Demostrar que un esquema de ruteo **reactivo, descentralizado y basado en posición relativa** (ANTop) puede ser una alternativa viable y escalable a CGR en constelaciones satelitales LEO, sin necesidad de conocimiento global de la topología de red.

El resultado más importante a comunicar es el **capítulo de simulaciones**: ANTop presenta excelentes resultados frente a CGR en los escenarios probados, destacándose su escalabilidad, su bajo costo computacional y el reducido número de hops.

---

## Objetivos Específicos

1. **Adaptar ANTop al dominio satelital**
   - Extender el protocolo original (diseñado para redes ad-hoc generales) para operar en entornos LEO con topología altamente dinámica e intermitente.
   - Introducir un mecanismo mejorado de detección y resolución de bucles (*loop epoch*) que evite reexploraciones redundantes en flujos de paquetes con mismo par origen-destino.
   
2. **Integrar H3 como sistema de direccionamiento geoespacial**
   - Reemplazar la representación anterior en grilla cuadrada (con distorsiones geométricas) por celdas hexagonales de H3.
   - Mapear la superficie terrestre en un esquema de hipercubos compatible con el modelo de direccionamiento de ANTop, preservando relaciones topológicas de vecindad.

3. **Evaluar y comparar el desempeño frente a CGR**
   - Comparar métricas de: tasa de entrega de paquetes, latencia extremo a extremo, cantidad de saltos y costo computacional.
   - Analizar el comportamiento bajo diferentes tamaños de constelación y condiciones de fallo.
   - Evaluar la escalabilidad del enfoque descentralizado frente al esquema de contact plan global de CGR.

---

## Estructura Temática de la Presentación (~45 min, ~45 slides)

| # | Sección | Contenido clave | Tiempo est. |
|---|---|---|---|
| 1 | Introducción | Imagen impactante de constelación satelital. Redes LEO y sus desafíos. Motivación del trabajo. | 3 min |
| 2 | Estado del arte | DTN y Bundle Protocol. CGR: cómo funciona y sus limitaciones de escalabilidad. ANTop original. | 7 min |
| 3 | ANTop Satelital | Hipercubo (diagrama/animación). Detección de bucles mejorada. Loop epoch (ejemplo breve). | 7 min |
| 4 | H3 | Qué es. Celdas hexagonales vs cuadradas. Sistema de coordenadas IJK. Jerarquía de resoluciones. Limitaciones. | 8 min |
| 5 | dtnsim | Arquitectura orientada a eventos. Limitaciones originales y acoplamiento con OS3/Atlas. Integración de ANTop. | 8 min |
| 6 | Simulaciones y resultados | Escenarios. Métricas comparativas ANTop vs CGR. Escalabilidad, hops, costo computacional. | 7 min |
| 7 | Conclusiones y trabajo futuro | Según el capítulo de conclusiones del informe. | 4 min |

---

## Audiencia y Tono

- **Público**: Jurado/docentes con conocimientos generales de redes. Sin conocimiento previo de DTN, CGR, ANTop, H3.
- **Nivel técnico**: Equivalente al del informe escrito. Todos los conceptos deben introducirse.
- **Instancia**: Defensa formal, pero el contenido ya fue aprobado — tono de presentación más que de defensa.
- **Expositores**: Dos co-autores (división a definir más adelante).

---

## Lineamientos de Diseño

- **Estilo**: Temática espacial/satelital. Fondo oscuro (no necesita ser imprimible).
- **Densidad**: Minimalista. Diagramas, animaciones e imágenes tienen prioridad sobre el texto.
- **Animaciones**: Para ilustrar conceptos (hipercubo, propagación de paquetes, loop epoch), no decorativas.
- **Formato**: HTML con soporte de animaciones. Proyección en pantalla grande.
- **Apertura**: Imagen impactante de constelación satelital, sin preguntas retóricas.
- **Logos**: FIUBA/UBA (a incorporar más adelante).

---

## Conceptos que Requieren Explicación Visual

| Concepto | Formato sugerido |
|---|---|
| Hipercubo de ANTop y direccionamiento | Diagrama animado |
| H3: hexágonos vs cuadrados | Comparativa visual |
| H3: sistema de coordenadas IJK | Diagrama |
| H3: creación de direcciones | Diagrama animado |
| Detección de bucles / loop epoch | Ejemplo animado breve |
| Constelación LEO en movimiento | Imagen/animación de apertura |
| Resultados ANTop vs CGR | Gráficas de simulación |

---

## Secciones Pendientes de Definir

- División de slides entre co-autores.
- Logos institucionales.
- Detalle de escenarios de simulación (a definir al armar el .md de simulaciones).
- Conclusiones y trabajo futuro (a extraer del capítulo correspondiente del informe).
- Demos visuales dinámicas (a evaluar en cada sección).
