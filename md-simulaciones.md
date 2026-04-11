# Sección 6: Simulaciones y Resultados
**Tiempo estimado:** 7 minutos | **Slides estimadas:** ~8-9

---

## Narrativa

El hilo conductor es: definir el marco experimental → mostrar los resultados → dejar que los números hablen. Los resultados son el punto central de toda la presentación — acá se valida o refuta la hipótesis del trabajo.

Resultado principal: **ANTop es mejor o comparable a CGR en todas las métricas, con la ventaja de no requerir contact plan global y con menor costo computacional.**

---

## Slide 1 — Marco experimental: métricas

**Contenido:** Qué se mide y por qué.

Las cuatro métricas de desempeño evaluadas:

| Métrica | Descripción |
|---|---|
| **Arrival time** | Tiempo total desde que se crea un paquete hasta que llega a destino. |
| **Elapsed time** | Tiempo de cómputo total acumulado en todos los nodos por los que pasó el paquete. Mide el costo computacional de ruteo. |
| **Number of hops** | Cantidad de nodos intermedios recorridos. |
| **Delivery ratio** | Porcentaje de paquetes que llegan a destino. |

**Elemento visual sugerido:** Tabla visual con las cuatro métricas, íconos representativos para cada una (reloj, CPU, saltos, checkmark).

---

## Slide 2 — Marco experimental: escenarios y constelaciones

**Contenido:** Cómo están configuradas las simulaciones.

Puntos clave:
- **Algoritmos comparados**: ANTop vs. CGR Rev17 en dos variantes:
  - *oneBestPath*: un único mejor camino por destino.
  - *perNeighborBestPath*: un mejor camino por vecino para cada destino.
- **Escenarios de fallas**: sin fallas, 5%, 10% y 20%.
- **Constelaciones Walker** (inclinación 53°), con 4 tamaños: **120, 180, 240 y 360 satélites**.
- Total: **16 simulaciones**. Carga de tráfico constante: **100 paquetes por nodo**.
- ANTop usa resolución H3 **0** (122 celdas): apropiada para el rango de satélites simulados.

**Nota sobre CGR Rev17**: la implementación estándar de CGR resultó computacionalmente prohibitiva para estos escenarios → se usa la versión optimizada. Incluso así, CGR *perNeighborBestPath* no pudo completarse para 360 satélites.

**Elemento visual sugerido:** Tabla o matriz de escenarios: filas = tamaños de constelación, columnas = niveles de falla. Marcar la celda faltante (CGR per-neighbor, 360 sats). Destacar el total de 16 simulaciones.

---

## Slide 3 — Delivery Ratio: ANTop entrega el 100%

**Contenido:** Tasa de entrega de paquetes para todos los escenarios.

Resultados:
- **ANTop**: 100% de entrega en **todos** los escenarios y niveles de falla (0%, 5%, 10%, 20%).
- **CGR per-neighbor**: ~100% en casi todos los casos. Leve fluctuación en 240 satélites sin fallas.
- **CGR one-best**: no alcanza el 100% en ningún escenario. En los casos más desfavorables desciende hasta ~93% (180 satélites con fallas).

Conclusión: ANTop es el único algoritmo que garantiza entrega completa incluso bajo 20% de fallas.

**Elemento visual sugerido:** Gráficas de delivery ratio del informe (una por nivel de falla). Dado el espacio, mostrar la comparativa más representativa (ej: 0% y 20% de fallas) o las cuatro en tamaño reducido.

---

## Slide 4 — Arrival Time: ANTop es más rápido y escala mejor

**Contenido:** Tiempo de llegada de paquetes.

Resultados:
- **ANTop** presenta las menores medianas en **todos** los escenarios, seguido por CGR *per-neighbor*.
- La diferencia es significativa: en varios escenarios el **percentil 75 de ANTop** está por debajo del **percentil 25 de CGR**.
- Comportamiento con el tamaño: CGR tiende a empeorar con más satélites. **ANTop mejora** → aprovecha la mayor conectividad de constelaciones más densas.
- Con fallas: ANTop muestra mayor dispersión (algunos paquetes tardan más), pero sus medianas siguen siendo significativamente menores que las de CGR.

**Elemento visual sugerido:** Boxplots de arrival time. Resaltar el cruce de percentiles (P75 ANTop vs. P25 CGR). Destacar visualmente la tendencia inversa de ANTop vs. CGR al aumentar la constelación.

---

## Slide 5 — Elapsed Time: ANTop es predecible y constante

**Contenido:** Costo computacional del proceso de ruteo.

Resultados:
- **ANTop**: medianas estables entre **0.01 y 0.1 ms** en todos los escenarios. Distribución compacta, pocos outliers. El costo prácticamente no crece con el tamaño de la red.
- **CGR one-best**: medianas bajas en escenarios simples, pero con **alta dispersión** y outliers que alcanzan los **100 ms**. El proceso de cálculo de rutas puede ser esporádicamente muy costoso.
- **CGR per-neighbor**: el mayor costo computacional. La diferencia se amplía con el tamaño de la constelación.

Conclusión: ANTop tiene un costo computacional **predecible y escalable**, mientras que CGR puede generar episodios de cómputo significativamente más costosos.

**Elemento visual sugerido:** Boxplots de elapsed time en escala logarítmica. Destacar la franja estable de ANTop (0.01–0.1 ms) vs. la cola larga de CGR one-best (hasta 100 ms).

---

## Slide 6 — Number of Hops: caminos cortos y estables

**Contenido:** Cantidad de saltos recorridos por los paquetes.

Resultados:
- **ANTop**: medianas entre **1 y 10 saltos**. Distribución compacta, estable frente a fallas y tamaño de constelación.
- **CGR per-neighbor**: medianas comparables a ANTop en varios escenarios, pero con mayor dispersión.
- **CGR one-best**: medianas más altas y distribución muy dispersa. Casos extremos superan los **10.000 saltos** — mayor que el número total de satélites — indicando trayectorias de reencaminamiento sucesivo muy largas.

**Elemento visual sugerido:** Boxplots de number of hops en escala logarítmica. Destacar los outliers extremos de CGR one-best (>10.000 saltos) como elemento llamativo.

---

## Slide 7 — Resumen comparativo

**Contenido:** Cuadro integrador de resultados.

| Métrica | ANTop | CGR per-neighbor | CGR one-best |
|---|---|---|---|
| Delivery ratio | **100% siempre** | ~100% | Hasta 93% |
| Arrival time | **Menor en todos los casos** | Intermedio | Mayor |
| Elapsed time | **Estable, 0.01–0.1 ms** | Mayor costo | Bajo pero impredecible |
| Number of hops | **1–10, estable** | Comparable | Hasta >10.000 |
| Escalabilidad | **Mejora con más satélites** | Se degrada | Se degrada |

Conclusión: ANTop iguala o supera a CGR en todas las métricas evaluadas, sin requerir un contact plan global y con un costo computacional predecible y escalable.

**Elemento visual sugerido:** Tabla visual con íconos de color (verde/amarillo/rojo) para cada celda. Simple y de alto impacto visual.

---

## Slide 8 — Observación sobre escalabilidad

**Contenido:** El comportamiento diferencial de ANTop al crecer la red merece destacarse por separado.

Puntos clave:
- A medida que la constelación crece (120 → 360 satélites), CGR se vuelve **más lento y más costoso computacionalmente**.
- ANTop muestra el comportamiento opuesto: **mejora en arrival time y mantiene constante el elapsed time**.
- Interpretación: ANTop aprovecha la mayor conectividad de redes más densas para encontrar rutas más directas. Su naturaleza local y reactiva se beneficia de más opciones de vecindad disponibles.
- Implicación práctica: ANTop es especialmente prometedor para **megaconstelaciones** (miles de satélites), donde CGR enfrenta problemas severos de escalabilidad.

**Elemento visual sugerido:** Gráfica de tendencia: eje X = tamaño de constelación, eje Y = arrival time. Dos líneas: CGR (creciente) vs. ANTop (decreciente o estable). Simple, impactante.

---

## Conceptos introducidos / recordados en esta sección

| Concepto | Definición |
|---|---|
| Walker constellation | Constelación con satélites distribuidos uniformemente en planos igualmente espaciados. |
| CGR Rev17 | Versión optimizada de CGR usada por su costo computacional manejable. |
| oneBestPath | Config. CGR: un único mejor camino por destino. |
| perNeighborBestPath | Config. CGR: mejor camino por vecino para cada destino. Más costoso pero más flexible. |
| Delivery ratio | % de paquetes que llegan a destino. |
| Elapsed time | Tiempo de cómputo acumulado del proceso de ruteo. Mide costo computacional. |

---

## Lo que NO va en esta sección

- Conclusiones finales y trabajo futuro → van en la sección siguiente.
- Detalles de implementación de RoutingAntop → ya cubiertos en dtnsim.
