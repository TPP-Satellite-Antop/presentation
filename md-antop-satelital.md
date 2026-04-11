# Sección 3: ANTop Satelital
**Tiempo estimado:** 7 minutos | **Slides estimadas:** 7

---

## Narrativa

El hilo conductor es: ANTop original es una buena base, pero aplicarlo a LEO plantea dos problemas concretos que hay que resolver:

1. **Los bucles se vuelven más frecuentes** — y el mecanismo original no los maneja bien cuando hay múltiples paquetes del mismo flujo.
2. **Las direcciones del hipercubo deben representar posiciones geográficas** — lo que requiere una estrategia de mapeo espacial. (Este punto actúa como transición hacia la sección de H3.)

---

## Slide 1 — El desafío: ANTop en entornos LEO

**Contenido:** Por qué ANTop original necesita ser adaptado.

Puntos clave:
- ANTop fue diseñado para redes ad-hoc genéricas con topología dinámica pero sin los niveles de intermitencia y cambio continuo de LEO.
- En constelaciones LEO, la conectividad entre satélites cambia de forma predecible pero frecuente → el hipercubo nunca está completo y los vecinos disponibles varían constantemente.
- Esto genera dos consecuencias directas que deben abordarse:
  - **Bucles temporales**: más frecuentes e inevitables en redes con baja conectividad.
  - **Representación geográfica**: las direcciones del hipercubo deben reflejar posiciones físicas sobre la Tierra.

**Elemento visual sugerido:** Diagrama de una constelación LEO con un grafo de conectividad que va cambiando (aristas apareciendo y desapareciendo). Contrastar con un hipercubo completo vs. uno incompleto.

---

## Slide 2 — El problema con los bucles en ANTop original

**Contenido:** La limitación del mecanismo de detección de bucles original y por qué no alcanza.

Puntos clave:
- En ANTop original, cuando un nodo detecta un bucle, devuelve el paquete al nodo emisor inmediato (*sender*). Esto rompe el ciclo pero **no fuerza una reexploración local de alternativas**: delega esa decisión en el nodo anterior.
- Problema: el nodo que detecta el bucle no reconsidera sus propios vecinos → puede perderse caminos alternativos válidos.
- La implementación propuesta mejora esto: al detectar un bucle, en lugar de devolver el paquete, el nodo **busca activamente un nuevo vecino candidato**:
  1. Obtener vecinos ordenados por distancia al destino.
  2. Excluir vecinos ya explorados (visited bitmap).
  3. Evitar en primera instancia volver al sender.
  4. Seleccionar el vecino no explorado más cercano al destino.

**Elemento visual sugerido:** Diagrama comparativo en dos columnas: ANTop original (bucle → devolver al sender) vs. propuesta (bucle → buscar nuevo vecino). Animación simple mostrando un paquete girando en ciclo y el mecanismo de escape.

---

## Slide 3 — El problema con múltiples paquetes del mismo flujo

**Contenido:** El fenómeno que surge al mejorar la detección de bucles y cómo lo introduce el loop epoch.

Puntos clave:
- Problema nuevo: un mensaje puede fragmentarse en varios paquetes con el **mismo par origen-destino**, enviados en un intervalo de tiempo muy corto.
- Si todos entran en bucle en el mismo nodo → cada uno dispara una reexploración independiente → cada paquete es ruteado por un vecino distinto hasta agotar vecinos disponibles. Esto es incorrecto.
- La solución es el **loop epoch**: un contador de versión por par origen-destino que asegura que solo se ejecute **una reexploración por versión**, y que todos los paquetes posteriores del mismo bucle converjan a la misma decisión.

**Elemento visual sugerido:** Diagrama de línea de tiempo con 3 paquetes (P1, P2, P3) llegando al mismo nodo X casi simultáneamente, todos en bucle. Sin loop epoch: 3 reexploraciones → 3 vecinos distintos. Con loop epoch: 1 reexploración → todos van al mismo vecino.

---

## Slide 4 — Loop epoch: mecanismo de versionado distribuido

**Contenido:** Cómo funciona el loop epoch en detalle.

Puntos clave:
- Cada par origen-destino mantiene un **contador entero** (loop epoch) en la tabla de pares del nodo.
- Cada paquete transporta su propio valor de epoch.
- Al detectar un bucle, el nodo compara el epoch del paquete con el local:
  - Si el epoch **local es mayor**: el bucle ya fue resuelto antes → usar el next hop vigente, actualizar el epoch del paquete.
  - Si el epoch **local es igual o menor al del paquete**: nuevo bucle → reexplorar vecino, incrementar ambos epochs.
- El epoch se propaga implícitamente a través de los paquetes: cualquier nodo que reciba un paquete con epoch mayor al suyo adopta ese valor → **convergencia eventual sin sincronización explícita**.

**Elemento visual sugerido:** Animación paso a paso con Nodo X, Paquete 1 y Paquete 2 (como en las tablas del informe). Mostrar los estados de distancia y loop epoch en cada paso. Mantenerlo breve: mostrar solo el caso clave (P1 dispara reexploración, P2 usa decisión vigente).

---

## Slide 5 — Falsos positivos y decisión de diseño

**Contenido:** Una decisión de diseño descartada y la razón.

Puntos clave:
- La detección de bucles por comparación de distancias al origen puede generar **falsos positivos**: si una falla o cambio de topología desvió el paquete, su hop count puede superar el valor registrado sin que haya un bucle real.
- Se evaluó introducir un **margen de tolerancia** para reducir falsos positivos.
- Conclusión experimental: el costo de mantener paquetes más tiempo en bucle antes de detectarlo superó el beneficio de reducir falsos positivos → **se mantuvo la detección sin umbral**.

**Elemento visual sugerido:** Diagrama mostrando el escenario de falso positivo (cambio de topología → desvío legítimo → parece bucle). Una anotación clara de por qué se descartó el umbral.

---

## Slide 7 — De la topología al espacio geográfico

**Contenido:** Transición hacia H3. Plantear el problema de representación que da lugar a la siguiente sección.

Puntos clave:
- Para aplicar ANTop en satélites, las **direcciones del hipercubo deben reflejar posiciones geográficas**: dos satélites cercanos deben tener direcciones cercanas (poca distancia de Hamming).
- Trabajos anteriores usaban una **grilla de celdas cuadradas** para discretizar la Tierra, pero esto introduce distorsiones geométricas significativas sobre una superficie esférica.
- La solución adoptada: **celdas hexagonales** con H3. Esto preserva mejor las relaciones de vecindad y reduce la distorsión a escala global.
- (Transición) → La siguiente sección explica qué es H3 y cómo se construyen las direcciones ANTop a partir de él.

**Elemento visual sugerido:** Imagen comparativa: grilla cuadrada proyectada sobre la Tierra (con distorsión visible en los polos) vs. teselación hexagonal H3 (uniforme). No entrar en detalles de H3 aún — solo mostrar la diferencia visual.

---

## Conceptos introducidos en esta sección

| Concepto | Definición |
|---|---|
| Bucle temporal | Paquete atrapado en un ciclo de nodos del que no puede salir sin cambio de topología. |
| Reexploración de vecinos | Mecanismo mejorado: al detectar bucle, buscar un nuevo vecino en lugar de devolver al sender. |
| Loop epoch | Contador de versión por par origen-destino. Asegura una sola reexploración por bucle detectado. |
| Convergencia eventual | Los nodos adoptan el epoch máximo observado sin sincronización explícita. |
| Representación geográfica | Necesidad de mapear posiciones físicas de satélites a direcciones del hipercubo. |

---

## Lo que NO va en esta sección

- Detalles de H3 (resolución, IJK, pentágonos, algoritmo BFS) → van en la sección siguiente.
- Resultados de simulación → van en Simulaciones.
- Detalles de implementación en dtnsim → van en la sección de dtnsim.
