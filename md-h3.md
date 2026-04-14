# Sección 4: H3
**Tiempo estimado:** 8 minutos | **Slides estimadas:** ~10-11

---

## Narrativa

Esta sección responde la pregunta planteada al final de ANTop Satelital: ¿cómo se mapean posiciones geográficas a direcciones del hipercubo de forma que se preserven las relaciones de vecindad?

El hilo conductor es:
> Necesitamos discretizar la Tierra → elegimos hexágonos → usamos H3 → así construimos las direcciones ANTop → luego refinamos el sistema para cubrir toda la Tierra.

La sección tiene dos partes:
1. **Qué es H3** y por qué hexágonos (slides 1-4).
2. **Cómo se construyen las direcciones ANTop** a partir de H3, y las limitaciones que motivan la dimensión prima y los múltiples hipercubos (slides 5-11).

---

## Slide 1 — Por qué hexágonos y no cuadrados

**Contenido:** Motivación de la elección de hexágonos como unidad de discretización.

Puntos clave:
- Una grilla cuadrada divide la Tierra en celdas de tamaño no uniforme.
- En cuadrados y triángulos, la distancia desde el centro de una celda hasta el centro de sus vecinos **no es constante** (los vecinos comparten vértices están más lejos que los que comparten aristas).
- En hexágonos, todos los vecinos comparten aristas con la celda central → la distancia al centro de cada vecino es **siempre la misma**.
- Esta propiedad es clave para ANTop: la distancia entre direcciones debe aproximar correctamente la distancia topológica entre satélites.

**Elemento visual sugerido:** Comparativa triple — cuadrado, triángulo, hexágono — cada uno con una celda central resaltada y líneas rectas trazadas desde su centro hasta el centro de cada vecino. Las líneas del hexágono son todas iguales; las del cuadrado y el triángulo tienen longitudes distintas (resaltar visualmente la diferencia).

---

## Slide 2 — Qué es H3

**Contenido:** Descripción general de la biblioteca H3.

Puntos clave:
- H3 es un sistema de **indexación geoespacial jerárquico** de Uber que discretiza la Tierra en celdas hexagonales.
- Usa un **icosaedro** (poliedro de 20 caras triangulares) inscrito en la esfera como estructura intermedia. Se proyectan hexágonos sobre cada cara y se mapean de vuelta a la esfera con proyección gnomónica policéntrica.
- La orientación sigue la **proyección Dymaxion** (Fuller): minimiza distorsión de áreas y mantiene las masas terrestres lo más intactas posibles.
- Limitación geométrica: no es posible cubrir una esfera solo con hexágonos → H3 incluye exactamente **12 celdas pentagonales** por resolución, en los vértices del icosaedro.

**Elemento visual sugerido:** Imagen del mapa Dymaxion desplegado / icosaedro. Luego imagen de la teselación H3 sobre la Tierra mostrando hexágonos uniformes y la presencia de pentágonos.

---

## Slide 3 — Jerarquía de resoluciones

**Contenido:** El sistema de resoluciones de H3 y su relevancia para el trabajo.

Puntos clave:
- H3 define 16 niveles de resolución (0 a 15), de menor a mayor granularidad.
- Resolución 0: **122 celdas**. Cada hexágono se subdivide en ~7 celdas en el nivel siguiente.
- Resoluciones usadas en este trabajo: **0, 1 y 2**:
  - Res. 0: 122 celdas | Res. 1: 842 celdas | Res. 2: 5882 celdas.
- La resolución elegida determina la granularidad con la que se mapean posiciones de satélites a celdas.

**Elemento visual sugerido:** Zoom progresivo: resolución 0 (122 celdas grandes) → resolución 1 → resolución 2. Tabla comparativa con cantidades de celdas por resolución.

---

## Slide 4 — Sistema de coordenadas IJK

**Contenido:** El sistema de coordenadas interno de H3 y el problema que introduce.

Puntos clave:
- H3 usa coordenadas discretas **IJK**: tres ejes sobre la grilla hexagonal, separados 120° entre sí.
- Permite expresar la posición de cada celda como un desplazamiento $(i, j, k)$ respecto de un origen.
- **Problema**: cada hexágono tiene **6 vecinos**, pero el sistema IJK tiene solo **3 ejes** → no todas las direcciones de vecindad coinciden con un eje.
- Consecuencia: algunos vecinos físicamente adyacentes (a 1 salto) requieren **más de 1 desplazamiento IJK** para ser alcanzados. Ejemplo: la celda en $(2,1,0)$ está a 2 saltos físicos pero requiere 3 desplazamientos IJK.
- Esto introduce una discrepancia entre distancia física y distancia IJK.

**Elemento visual sugerido:** Diagrama del sistema IJK con tres ejes a 120°. Resaltar dos celdas: una a 1 salto físico (alcanzable en 1 desplazamiento IJK, verde) y otra a 2 saltos físicos (requiere más desplazamientos IJK de lo esperado, celeste).

---

## Slide 5 — Construcción de direcciones ANTop: el concepto

**Contenido:** Cómo se traduce una posición IJK a una dirección binaria ANTop. Introducción al proceso.

Puntos clave:
- Una dirección ANTop es una **secuencia de movimientos IJK** desde el origen hasta una celda objetivo.
- Hay 6 movimientos posibles sobre la grilla hexagonal (uno por dirección de vecindad), numerados 1 a 6.
- Cada movimiento se codifica en **3 bits**.
- Se empaquetan **2 movimientos por byte** para comprimir la dirección.
- La **distancia de Hamming** entre dos direcciones ANTop aproxima la distancia real entre celdas en H3.

**Elemento visual sugerido:** Imagen base: grilla hexagonal con origen marcado y una celda objetivo. Esta misma imagen se usará de forma evolutiva en las slides siguientes.

---

## Slide 6 — Construcción de direcciones: paso 1 (descomposición en movimientos)

**Contenido:** Primera etapa — identificar los movimientos necesarios para llegar a la celda objetivo.

Usando el ejemplo del informe: celda en $(2,1,0)$.

- Desde el origen $(0,0,0)$, el camino más corto requiere 3 movimientos:
  - $(1,0,0)$ + $(1,0,0)$ + $(0,1,0)$

**Elemento visual sugerido:** La misma imagen base de la slide 5, ahora con los 3 movimientos dibujados como flechas sobre la grilla. Cada flecha etiquetada con su vector IJK.

---

## Slide 7 — Construcción de direcciones: paso 2 (codificación en bits)

**Contenido:** Segunda etapa — codificar cada movimiento en 3 bits.

- $(1,0,0)$ → dirección 4 → `100`
- $(1,0,0)$ → dirección 4 → `100`
- $(0,1,0)$ → dirección 2 → `010`

**Elemento visual sugerido:** La misma imagen con las flechas, ahora con la codificación binaria de cada movimiento resaltada al lado de cada flecha.

---

## Slide 8 — Construcción de direcciones: paso 3 (compresión en bytes)

**Contenido:** Tercera etapa — empaquetar los movimientos en bytes (2 por byte).

- Byte 1: primer `100` + segundo `100` → `00100100`
- Byte 2: tercer `010` → `00000010`
- Dirección final: `00100100 00000010`

**Elemento visual sugerido:** La misma imagen con las flechas y codificaciones. Ahora se agrega la representación de bytes debajo, mostrando visualmente el empaquetado de los movimientos en cada byte.

---

## Slide 9 — La dimensión prima: solución a la discrepancia IJK

**Contenido:** Cómo se reduce la discrepancia introduciendo un segundo sistema de coordenadas.

Puntos clave:
- Se introduce una **dimensión prima**: el sistema IJK rotado **60° en sentido horario**.
- En esta nueva orientación, celdas que requerían más de 1 desplazamiento en IJK original ahora son alcanzables en 1 paso.
- Juntos, IJK + IJK prima definen las **6 direcciones de adyacencia** de la grilla hexagonal → se eliminan los "saltos extra" en ambas dimensiones.
- Cada celda puede tener direcciones en **ambas dimensiones** (dos hipercubos independientes para la misma topología física).
- Al rutear, el nodo puede usar cualquiera de las representaciones para elegir el próximo salto.

**Elemento visual sugerido:** Dos diagramas lado a lado: IJK original y IJK prima (rotado 60°). Las mismas dos celdas de referencia (verde y celeste) en ambos sistemas: mostrar que la verde es alcanzable en 1 paso en IJK prima pero no en el original.

---

## Slide 10 — Origen del hipercubo y partición del espacio

**Contenido:** Por qué el sistema IJK de H3 es local y por qué los orígenes de los hipercubos son las celdas pentagonales.

Puntos clave:
- El sistema IJK de H3 es **local**: no existe un único sistema de coordenadas global porque los hexágonos no comparten orientación entre sí. La causa es la presencia de los **pentágonos**, que rompen la uniformidad de la grilla e impiden definir ejes consistentes sobre toda la superficie.
- Llevar ese sistema local a escala global introduce **ruido**: las distorsiones se acumulan y se vuelven peores cuanto más lejos están las celdas del origen establecido.
- Solución: **múltiples hipercubos independientes**, cada uno con su sistema de coordenadas local.
- Origen de cada hipercubo: una **celda pentagonal**. ¿Por qué?
  - Exactamente **12 pentágonos** por resolución → cantidad fija.
  - Distribuidos **aproximadamente de forma uniforme** → partición equilibrada del espacio.
  - Los pentágonos coinciden con los vértices del icosaedro → su coordenada IJK respecto de sí mismos es $(0,0,0)$ → correspondencia directa con el origen del sistema local.

**Elemento visual sugerido:** Imagen de los 12 pentágonos distribuidos sobre la Tierra, cada uno marcado como origen de su hipercubo local.

---

## Slide 11 — Medición del error de distancias

**Contenido:** Validación de la solución: ¿qué tan bien aproxima ANTop las distancias reales de H3?

Puntos clave:
- Se mide la diferencia entre la **distancia ANTop** (Hamming en el hipercubo) y la **distancia real H3** (mínimo de saltos en la grilla).
- Resultados principales:
  - Para **distancias cortas**: error prácticamente nulo.
  - **Error absoluto**: crece con la resolución, pero de forma sublineal con la distancia.
  - **Error relativo**: más estable entre resoluciones. Peor en distancias intermedias (el error absoluto no es despreciable, pero la distancia tampoco es suficientemente grande para diluirlo).
- Conclusión: la aproximación es suficientemente precisa para guiar decisiones de ruteo — el objetivo no es exactitud de distancias sino orientar correctamente el tráfico.

**Elemento visual sugerido:** Gráfica de error relativo vs. distancia por resolución (la más explicativa del informe). Resaltar el pico en distancias intermedias y la convergencia a bajos errores en distancias largas.

---

## Conceptos introducidos en esta sección

| Concepto | Definición |
|---|---|
| H3 | Sistema de indexación geoespacial jerárquico de Uber. Celdas hexagonales sobre la Tierra. |
| Icosaedro | Poliedro de 20 caras usado como estructura intermedia. |
| Proyección Dymaxion | Minimiza distorsión de áreas al desplegar la esfera sobre el icosaedro. |
| Pentágono H3 | Celda con 5 lados. 12 por resolución. Usados como orígenes de hipercubo. |
| Resolución H3 | Nivel de granularidad. Res. 0: 122 celdas. Res. 1: 842. Res. 2: 5882. |
| IJK | Sistema de coordenadas discretas con 3 ejes a 120°. |
| Dimensión prima | IJK rotado 60°. Complementa IJK para cubrir las 6 direcciones de vecindad hexagonal. |
| Error de distancia | Diferencia entre distancia ANTop (Hamming) y distancia real H3 (saltos en la grilla). |

---

## Lo que NO va en esta sección

- Implementación de `RoutingAntop` en dtnsim → va en la sección de dtnsim.
- Comparativas de rendimiento con CGR → van en Simulaciones.
- Detalles del loop epoch → ya cubiertos en ANTop Satelital.
