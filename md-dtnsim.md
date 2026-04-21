# Sección 5: Herramientas de Simulación
**Tiempo estimado:** 8 minutos | **Slides estimadas:** ~3

---

## Narrativa

El hilo conductor es una cadena de necesidades y soluciones:

> DTNSim no soporta conectividad dinámica → se agrega ContactlessDtn. Atlas no tiene phase offset → se extiende. Para comparar con CGR en igualdad de condiciones → se genera el contact plan desde el mismo movimiento orbital.

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

## Slide 2 — Limitación: DTNSim no soporta conectividad dinámica

**Contenido:** Por qué DTNSim en su forma original no es compatible con ANTop.

Puntos clave:
- El módulo **Dtn** original está diseñado en torno a contact plans **predefinidos**: la conectividad entre nodos se conoce de antemano.
- **ContactlessDtn** reemplaza el contact plan por un flujo en tres pasos: (1) simulación orbital → posiciones actuales, (2) derivación de conectividad a partir de esas posiciones, (3) algoritmo de ruteo que decide cómo usar esa conectividad.
- La conectividad **emerge de la simulación**: no se predetermina. Cada paso de simulación recalcula qué nodos pueden comunicarse según sus posiciones actuales.
- El algoritmo de ruteo es **intercambiable**: ContactlessDtn no asume ANTop; puede soportar CGR u otros. Es ANTop el que interpreta las posiciones en términos de celdas H3 y adyacencia.

**Elemento visual:** Diagrama comparativo de dos paneles.
- Izquierdo (DTN original): tabla de contact plan con ventanas fijas → Router.
- Derecho (ContactlessDtn): flujo vertical — simulación orbital con satélites en órbita → grafo de conectividad dinámica → Router (ANTop, CGR, ...).

---

## Slide 3 — Atlas: constelaciones paramétricas

**Contenido:** Cómo Atlas permite definir constelaciones experimentales y qué extensión se realizó.

Puntos clave:
- **Atlas** permite definir constelaciones satelitales de forma completamente **paramétrica**.
- Genera órbitas sintéticas a partir de parámetros configurables: cantidad de satélites, planos orbitales, altitud, inclinación, phase offset.
- Esto permite explorar sistemáticamente distintas topologías y densidades sin depender de datos orbitales reales.
- **Extensión realizada**: Atlas no incluía soporte para **phase offset** → se extendió para modelar correctamente constelaciones **Walker** completas.
- El phase offset distribuye los satélites de forma más uniforme entre planos → mayor cobertura de celdas H3 → mejor conectividad para ANTop.

**Elemento visual sugerido:** Las dos imágenes del informe lado a lado: constelación Walker sin phase offset (satélites alineados) vs. con phase offset (distribución uniforme). Mostrar también la cobertura H3 correspondiente (celdas ocupadas vs. vacías).

---

## Conceptos introducidos en esta sección

| Concepto | Definición |
|---|---|
| ContactlessDtn | Módulo nuevo que reemplaza contact plans por conectividad dinámica basada en posición H3. |
| Atlas | Simulador con constelaciones completamente paramétricas. |
| Phase offset | Desfase entre satélites de planos consecutivos. Mejora uniformidad de cobertura H3. |
| Walker constellation | Tipo de constelación con satélites distribuidos uniformemente en planos igualmente espaciados. |

---

## Lo que NO va en esta sección

- Resultados de las simulaciones → van en la sección siguiente.
- Detalles de H3 (resolución, IJK) → ya cubiertos.
- Detalles del loop epoch → ya cubiertos.
- OS3, SGP4, TLE, csm/ContactSatelliteMobility → removidos de la presentación.
