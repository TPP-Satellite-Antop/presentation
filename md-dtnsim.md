# Sección 5: Herramientas de Simulación
**Tiempo estimado:** 8 minutos | **Slides estimadas:** ~6

---

## Narrativa

El hilo conductor es una cadena de necesidades y soluciones:

> DTNSim no soporta conectividad dinámica → se agrega ContactlessDtn. DTNSim no tiene mecánica orbital → se integra OS3. OS3 no permite constelaciones paramétricas → se usa Atlas. Atlas no tiene phase offset → se extiende. Para comparar con CGR en igualdad de condiciones → se genera el contact plan desde el mismo movimiento orbital.

---

## Slide 1 — DTNSim: el simulador base

**Contenido:** Qué es DTNSim y qué rol cumple en el trabajo.

Puntos clave:
- DTNSim es un simulador de redes DTN orientado al estudio de protocolos de enrutamiento, reenvío y planificación.
- Modela la dinámica de la red mediante **contact plans**: descripciones explícitas de cuándo dos nodos pueden comunicarse.
- Soporta múltiples algoritmos de ruteo, incluyendo CGR.
- La movilidad de los nodos no se representa como desplazamiento continuo, sino como **disponibilidad temporal de enlaces** → adecuado para redes satelitales e interplanetarias.
- Es el entorno base sobre el que se implementa y evalúa ANTop.

**Elemento visual sugerido:** Diagrama simple de DTNSim: nodos con ventanas de contacto representadas como intervalos en una línea de tiempo. Mostrar que los bundles "esperan" hasta que hay un contacto disponible.

---

## Slide 2 — Arquitectura modular de DTNSim

**Contenido:** Estructura interna del simulador y sus componentes principales.

Puntos clave:
- Arquitectura orientada a eventos, organizada en módulos NED.
- Nivel superior: red **Dtnsim** con un módulo **Central** (recolección de métricas globales, gestión de fallos) y un conjunto de módulos **Node**.
- Cada nodo tiene cuatro submódulos:
  - **App**: genera y consume bundles (origen/destino del tráfico).
  - **Dtn**: núcleo del nodo. Gestiona almacenamiento, ruteo y reenvío de bundles.
  - **Com**: abstrae la transmisión. Modela disponibilidad y capacidad de enlaces.
  - **Fault**: módulo opcional para inyección de fallos.
- El módulo Central es un observador externo: no participa en el flujo de bundles, solo supervisa.

**Elemento visual sugerido:** Diagrama de la arquitectura de módulos (similar a la figura del informe): nodo con sus 4 submódulos y flechas de flujo de bundles: App → Dtn → Com → (red).

---

## Slide 3 — Limitación: DTNSim no soporta conectividad dinámica

**Contenido:** Por qué DTNSim en su forma original no es compatible con ANTop.

Puntos clave:
- El módulo **Dtn** original está diseñado en torno a contact plans **predefinidos**: la conectividad entre nodos se conoce de antemano.
- ANTop, en cambio, determina la conectividad **dinámicamente**: dos satélites están conectados si están en celdas H3 adyacentes → depende de sus posiciones geográficas en tiempo real.
- Solución: se desarrolló **ContactlessDtn**, un nuevo módulo que desacopla el Dtn de los contact plans y permite que la conectividad emerja de las posiciones geográficas actuales de los nodos.

**Elemento visual sugerido:** Diagrama comparativo: Dtn original (contact plan fijo, conectividad predefinida) vs. ContactlessDtn (posición geográfica → celda H3 → vecinos dinámicos).

---

## Slide 4 — OS3: movilidad orbital para DTNSim

**Contenido:** Por qué se necesita OS3 y cuál es su limitación.

Puntos clave:
- DTNSim no incluye mecánica orbital → se integra **OS3** (Open Source Satellite Simulator), que extiende OMNeT++ con modelos de movilidad orbital basados en **SGP4** (Simplified General Perturbations 4).
- OS3 permite que cada satélite actualice su posición en función del tiempo de simulación, de forma transparente para el resto del sistema.
- **Limitación**: OS3 requiere archivos **TLE** (Two-Line Element) para describir las órbitas → restringe la simulación a constelaciones reales predefinidas y dificulta la generación de escenarios ficticios y parametrizables.

**Elemento visual sugerido:** Diagrama mostrando OS3 como capa sobre DTNSim. Imagen o ícono de archivo TLE con una anotación de su limitación (constelaciones fijas, sin flexibilidad experimental).

---

## Slide 5 — Atlas: constelaciones paramétricas

**Contenido:** Cómo Atlas resuelve la limitación de OS3 y qué agrega al trabajo.

Puntos clave:
- **Atlas** es un simulador construido sobre OS3 que permite definir constelaciones satelitales de forma completamente **paramétrica**, sin archivos TLE.
- Introduce el módulo **NORAD**: genera órbitas sintéticas a partir de parámetros configurables: cantidad de satélites, planos orbitales, altitud, inclinación, phase offset.
- Esto permite explorar sistemáticamente distintas topologías y densidades sin depender de datos orbitales reales.
- **Extensión realizada**: Atlas no incluía soporte para **phase offset** → se extendió para modelar correctamente constelaciones **Walker** completas.
- El phase offset distribuye los satélites de forma más uniforme entre planos → mayor cobertura de celdas H3 → mejor conectividad para ANTop.

**Elemento visual sugerido:** Las dos imágenes del informe lado a lado: constelación Walker sin phase offset (satélites alineados) vs. con phase offset (distribución uniforme). Mostrar también la cobertura H3 correspondiente (celdas ocupadas vs. vacías).

---

## Slide 6 — Generación de contact plans para CGR

**Contenido:** Cómo se garantiza que la comparación ANTop vs. CGR sea justa.

Puntos clave:
- CGR usa contact plans; ANTop usa posiciones geográficas → la conectividad se modela de forma diferente.
- Para comparar en condiciones equivalentes, se desarrolló la red **csm**: una simulación independiente que genera contact plans a partir del **mismo movimiento orbital** usado por ANTop.
- El módulo **ContactSatelliteMobility** convierte el movimiento orbital en contactos: en cada actualización, mapea la posición del satélite a una celda H3, obtiene sus vecinos, y registra un contacto con los satélites en celdas adyacentes.
- El resultado es un archivo de contact plan en el formato de CGR, que refleja exactamente las mismas condiciones de conectividad que usa ANTop.
- Esto garantiza que **ambos algoritmos son evaluados sobre la misma topología de red**.

**Elemento visual sugerido:** Diagrama del pipeline de simulación: (1) Atlas genera órbitas → (2) csm convierte movimiento orbital a contact plan → (3) DTNSim ejecuta ANTop y CGR con las mismas condiciones.

---

## Conceptos introducidos en esta sección

| Concepto | Definición |
|---|---|
| ContactlessDtn | Módulo nuevo que reemplaza contact plans por conectividad dinámica basada en posición H3. |
| OS3 | Extensión de OMNeT++ con modelos de movilidad orbital (SGP4). |
| SGP4 | Modelo estándar de propagación de órbitas satelitales LEO. |
| TLE | Formato estándar para describir órbitas satelitales reales. |
| Atlas | Simulador sobre OS3 con constelaciones completamente paramétricas. |
| Phase offset | Desfase entre satélites de planos consecutivos. Mejora uniformidad de cobertura H3. |
| Walker constellation | Tipo de constelación con satélites distribuidos uniformemente en planos igualmente espaciados. |
| csm | Simulación auxiliar que genera contact plans equivalentes a las condiciones de ANTop. |

---

## Lo que NO va en esta sección

- Resultados de las simulaciones → van en la sección siguiente.
- Detalles de H3 (resolución, IJK) → ya cubiertos.
- Detalles del loop epoch → ya cubiertos.
