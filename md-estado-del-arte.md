# Sección 2: Estado del Arte
**Tiempo estimado:** 7 minutos | **Slides estimadas:** 7

---

## Narrativa

El hilo conductor de esta sección es el **contraste entre dos paradigmas de ruteo**:

- **CGR**: proactivo, centralizado, basado en conocimiento global de la red → efectivo pero costoso y poco escalable.
- **ANTop original**: reactivo, descentralizado, basado en información local → más adaptable, pero diseñado para redes ad-hoc genéricas.

Esta sección sienta las bases conceptuales que justifican las decisiones de diseño del trabajo.

---

## Slide 1 — DTN: arquitectura para redes intermitentes

**Contenido:** Qué es DTN y por qué es necesario en entornos satelitales.

Puntos clave:
- DTN (Delay/Disruption Tolerant Networking) es una arquitectura diseñada para redes donde la conectividad extremo a extremo no está garantizada.
- Introduce el concepto de **Bundle Protocol**: los datos se encapsulan en "bundles" que se almacenan en nodos intermedios y se reenvían cuando hay conectividad disponible (**store-carry-and-forward**).
- Permite comunicación en entornos con largas latencias, interrupciones frecuentes y ausencia de ruta end-to-end simultánea.
- Es el estándar en comunicaciones espaciales (NASA, ESA, redes interplanetarias).

**Elemento visual sugerido:** Diagrama animado de store-carry-and-forward: un bundle "espera" en un nodo intermedio hasta que aparece un contacto con el siguiente nodo y es reenviado.

---

## Slide 2 — CGR: Contact Graph Routing

**Contenido:** Qué es CGR y cómo funciona conceptualmente.

Puntos clave:
- CGR es el protocolo de ruteo más extendido para DTN satelital.
- Requiere un **Contact Plan**: una tabla con todas las oportunidades de comunicación futuras entre nodos (inicio, fin, tasa de datos, latencia).
- A partir del Contact Plan construye un **Contact Graph**: un grafo acíclico dirigido donde los vértices son episodios de contacto y las aristas representan períodos de almacenamiento entre contactos.
- Usa una variante del **algoritmo de Dijkstra** para encontrar la ruta que minimiza el tiempo de entrega (Best Delivery Time).
- En el momento de reenviar, evalúa condiciones locales: prioridad del bundle, tamaño, estado de las colas.

**Elemento visual sugerido:** Diagrama del Contact Graph: nodos representando satélites, aristas con ventanas de tiempo etiquetadas. Una ruta óptima resaltada sobre el grafo.

---

## Slide 3 — Limitaciones de CGR

**Contenido:** Por qué CGR no escala bien en constelaciones grandes o con fallas.

Puntos clave:
- El Contact Plan debe ser **global, preciso y actualizado**: todos los nodos necesitan conocer todos los contactos futuros de toda la red.
- En constelaciones grandes (cientos o miles de satélites), mantener este plan actualizado es costoso en términos de señalización y almacenamiento.
- Ante **fallas inesperadas** de nodos o cambios no previstos en la topología, el Contact Plan queda desactualizado → decisiones de ruteo incorrectas.
- El costo computacional del algoritmo de Dijkstra crece exponencialmente con el tamaño de la red.
- Es un enfoque **centralizado por diseño**: poco adaptable a escenarios dinámicos o distribuidos.

**Elemento visual sugerido:** Diagrama comparativo: constelación pequeña (Contact Plan manejable) vs. constelación grande (Contact Plan imposible de mantener). O un timeline donde un nodo falla y el Contact Plan queda "desactualizado".

---

## Slide 4 — ANTop: topología codificada en las direcciones

**Contenido:** La idea central del protocolo ANTop original.

Puntos clave:
- ANTop (Adjacent Network Topologies) propone un enfoque radicalmente distinto: **codificar la topología de la red en las direcciones de los nodos**.
- Cada nodo tiene una dirección representada como una **secuencia de bits** que refleja su posición relativa dentro de una estructura lógica llamada **hipercubo**.
- Esta codificación permite que cada nodo tome decisiones de ruteo **usando solo información de sus vecinos inmediatos**, sin conocimiento global de la red.
- Diseñado para redes **ad-hoc descentralizadas** donde todos los nodos tienen la misma jerarquía.
- Además de la dirección dinámica, cada nodo tiene un **identificador universal** persistente para ser identificado independientemente de su posición en el hipercubo.

**Elemento visual sugerido:** Imagen de un hipercubo de 4 dimensiones (16 vértices) con nodos etiquetados con direcciones binarias. Resaltar que la distancia entre dos nodos = distancia Hamming (cantidad de bits que difieren).

---

## Slide 5 — El hipercubo y el ruteo greedy

**Contenido:** Cómo ANTop usa el hipercubo para rutear paquetes.

Puntos clave:
- La **distancia topológica** entre dos nodos es la **distancia Hamming** entre sus direcciones: la cantidad de bits en que difieren → cuántos saltos separan a los nodos en el hipercubo.
- El ruteo es **greedy**: en cada salto, el nodo selecciona el vecino cuya dirección minimice la distancia Hamming con el destino.
- En un hipercubo **completo**, esto garantiza un camino de longitud mínima.
- En la práctica se trabaja con hipercubos **incompletos** (no todos los vértices tienen nodo asignado): no siempre existe un vecino que reduzca la distancia en cada paso → pueden aparecer bucles o caminos subóptimos.
- Incorporación de nuevos nodos: el nodo emite un broadcast, un nodo vecino le cede una dirección disponible de su espacio de direcciones administrado.

**Elemento visual sugerido:** Animación de ruteo greedy sobre un hipercubo: origen resaltado, destino resaltado, el camino se construye eligiendo en cada paso el vecino con menor distancia Hamming al destino. Luego mostrar un hipercubo incompleto donde el camino greedy falla y aparece un bucle.

---

## Slide 6 — Ruteo reactivo vs. proactivo

**Contenido:** Contextualizar ANTop dentro del paradigma de ruteo reactivo.

Puntos clave:
- **Proactivo** (CGR): las rutas se calculan y mantienen actualizadas continuamente. Siempre hay un camino óptimo disponible, pero requiere señalización constante ante cambios de topología.
- **Reactivo** (ANTop): las rutas se calculan solo cuando son necesarias, en base al estado local disponible al momento de rutear. Sin conocimiento global, sin precomputación.
- En redes LEO con topología en cambio continuo, el mantenimiento proactivo de rutas es costoso e ineficiente.
- El enfoque reactivo de ANTop resulta más natural para estos entornos.

**Elemento visual sugerido:** Tabla comparativa simple: CGR vs ANTop en las dimensiones de conocimiento requerido, costo de mantenimiento, adaptabilidad a fallas, escalabilidad.

---

## Slide 7 — Tabla de ruteo de ANTop

**Contenido:** La estructura de estado local que mantiene cada nodo en ANTop.

Puntos clave: cada nodo mantiene dos estructuras:

1. **Tabla de ruteo** (indexada por destino):
   - *Next hop*: vecino actualmente seleccionado para ese destino.
   - *Distance*: distancia estimada al destino.
   - *Neighbors*: lista de vecinos ordenada ascendentemente por distancia al destino.
   - *Visited bitmap*: bits que registran qué vecinos ya fueron explorados.

2. **Tabla de pares origen-destino** (indexada por par src-dst):
   - *Distance*: mínimo número de hops con el que el nodo recibió un paquete de ese par. Permite detectar bucles sin guardar información de cada paquete individual.

Estas estructuras se construyen dinámicamente durante la operación — no son precomputadas.

**Elemento visual sugerido:** Las dos tablas del informe (Tabla de ruteo y Tabla de pares) renderizadas visualmente con datos de ejemplo. Simple, limpia, tipografía monoespaciada.

---

## Conceptos introducidos en esta sección

| Concepto | Definición |
|---|---|
| Bundle Protocol | Protocolo de encapsulación para DTN. Store-carry-and-forward. |
| Store-carry-and-forward | El nodo almacena el bundle hasta tener conectividad con el siguiente. |
| Contact Plan | Tabla global de oportunidades de comunicación futuras entre todos los nodos. |
| Contact Graph | Grafo acíclico derivado del Contact Plan. Base de cálculo de rutas en CGR. |
| Best Delivery Time (BDT) | Métrica que CGR minimiza: tiempo de entrega proyectado. |
| Hipercubo | Estructura lógica de $n$ dimensiones donde cada vértice tiene dirección binaria única. |
| Distancia Hamming | Cantidad de bits que difieren entre dos direcciones ANTop. Determina la distancia topológica entre nodos en el hipercubo. |
| Ruteo greedy | En cada salto, elegir el vecino que más reduce la distancia al destino. |
| Visited bitmap | Registro de vecinos ya explorados para un destino dado. |

---

## Nota de posicionamiento de H3

H3 **no pertenece al marco teórico** de esta sección. Su uso es una consecuencia de implementación: al adaptar ANTop al dominio satelital, surge la necesidad de codificar posiciones geográficas en las direcciones del hipercubo. H3 es la solución elegida para ese problema concreto. Se introduce en la sección de ANTop Satelital, donde surge naturalmente de ese razonamiento.

---

## Lo que NO va en esta sección

- Modificaciones de ANTop para el dominio satelital (loop epoch, etc.) → van en ANTop Satelital.
- H3 → va en su propia sección.
- Resultados comparativos → van en Simulaciones.
