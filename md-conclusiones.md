# Sección 7: Conclusiones y Trabajo Futuro
**Tiempo estimado:** 4 minutos | **Slides estimadas:** ~4

---

## Narrativa

El cierre debe ser conciso y contundente. No es un resumen de lo que se mostró — es la respuesta directa a la pregunta que motivó el trabajo: ¿puede ANTop ser una alternativa viable a CGR en redes satelitales LEO?

La respuesta es sí, y hay evidencia concreta para respaldarlo.

---

## Slide 1 — Conclusiones: la respuesta a la hipótesis

**Contenido:** Respuesta directa y fundamentada a la pregunta central del trabajo.

Puntos clave:
- ANTop demuestra ser una **alternativa viable y escalable** a CGR para el enrutamiento en constelaciones LEO.
- Sus ventajas más significativas:
  - **100% de tasa de entrega** en todos los escenarios, incluso con 20% de fallas.
  - **Menor tiempo de llegada** en todos los casos, con tendencia a mejorar en redes más grandes.
  - **Costo computacional predecible y constante** (0.01–0.1 ms), sin los picos de CGR.
  - **Rutas más cortas** — medianas de 1 a 10 saltos frente a casos extremos de >10.000 en CGR.
- La naturaleza **reactiva y descentralizada** de ANTop lo hace inherentemente más adaptable a fallas y cambios de topología que CGR, cuya efectividad depende de un contact plan global preciso y actualizado.

**Elemento visual sugerido:** La tabla resumen de la sección de simulaciones (ANTop vs. CGR per-neighbor vs. CGR one-best), ahora con el foco en las conclusiones que se desprenden de ella.

---

## Slide 2 — Trabajo futuro

**Contenido:** Qué direcciones quedan abiertas a partir de este trabajo.

Puntos clave:
- **Escalabilidad a resoluciones mayores**: evaluar ANTop con resoluciones H3 1 y 2 en constelaciones más grandes (tipo Starlink, con miles de satélites).
- **Otras geometrías orbitales**: constelaciones con distintas inclinaciones, altitudes o distribuciones no-Walker.
- **Tráfico no uniforme**: evaluar el comportamiento bajo patrones de tráfico heterogéneos o con hot spots geográficos.
- **Validación sobre modelos orbitales reales**: integrar TLE de constelaciones existentes para contrastar con los resultados paramétricos.
- **Optimizaciones del algoritmo**: explorar variantes del mecanismo de loop epoch o estrategias de selección de vecinos bajo alta carga.

**Elemento visual sugerido:** Lista de 4-5 ítems. Puede terminar con una imagen final de constelación satelital como cierre visual de la presentación.

---

## Conceptos recordados en esta sección

No se introducen conceptos nuevos. Esta sección consolida todo lo presentado.

---

## Tono general del cierre

- Directo y seguro — los resultados respaldan la hipótesis claramente.
- No sobreprometer: reconocer las limitaciones sin disculparse por ellas.
- Cerrar con trabajo futuro como señal de que el trabajo abre una línea de investigación, no la cierra.
