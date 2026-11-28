### Análisis de Complejidad del Algoritmo Húngaro

El **Algoritmo Húngaro** (o método de *minimizar la suma*) resuelve el problema de asignación de costo mínimo en un grafo bipartito completo $K_{n,n}$ con matriz de costos $C \in \mathbb{R}_{\ge 0}^{n \times n}$.

Dependiendo de cómo se gestionen las búsquedas de caminos aumentantes y las actualizaciones de la matriz de costos, el algoritmo presenta tres cotas de complejidad temporal:

1. **$O(n^5)$**: Implementación ingenua (*naif*), descartando el matching parcial y recalculando desde cero tras cada cambio de matriz.
2. **$O(n^4)$**: Implementación estándar (de lápiz y papel), preservando el matching parcial y el bosque alternante tras actualizar la matriz.
3. **$O(n^3)$**: Implementación optimizada en computadora mediante potenciales duales y mantenimiento dinámico de mínimos por columna.

---

#### 1. Definiciones Preliminares y Paso Inicial

Dado un matching parcial $M$ compuesto exclusivamente por aristas de costo cero ($C_{x,y} = 0$):

- **$S \subseteq X$**: Conjunto de filas (vértices izquierdos) etiquetadas. En las pruebas de las implementaciones estándar y optimizada, al comienzo de cada fase elegimos una única fila libre $x_0$ como raíz y empezamos con $S = \{x_0\}$. El matching y los potenciales persisten entre fases; las etiquetas de búsqueda se reinician.
- **$\Gamma(S) \subseteq Y$**: Conjunto de columnas (vértices derechos) alcanzables desde $S$ mediante aristas de costo cero; es decir, columnas etiquetadas durante la búsqueda.
- **$\overline{S} = X \setminus S$** y **$\overline{\Gamma(S)} = Y \setminus \Gamma(S)$**: Sus respectivos complementos.

##### Paso 0: Reducción Inicial y Matching de Ceros

1. Restar el mínimo de cada fila a todos los elementos de esa fila, y luego el mínimo de cada columna a todos los elementos de esa columna. Esto toma $O(n^2)$.
2. Construir un matching inicial greedy $M_0$ tomando ceros independientes. Esto también toma $O(n^2)$.

Como $|M| \le n$, necesitaremos a lo sumo **$O(n)$ fases de extensión** para alcanzar un matching perfecto ($|M| = n$). Cada fase usa una raíz libre fija; cuando encuentra un camino aumentante, aumenta $|M|$ en uno y comienza otra fase con otra fila libre.

---

#### 2. Teorema 1: Complejidad $O(n^4)$ (Implementación Estándar)

> **Teorema 1.** *La complejidad temporal del Algoritmo Húngaro, manteniendo el matching parcial y el etiquetado actual tras cada actualización de la matriz, es $O(n^4)$.*

##### Demostración

La complejidad total está dada por:

$$T_{\text{total}} = O(n^2) + O(n) \cdot T_{\text{ext}}$$

donde $T_{\text{ext}}$ es el costo de extender el matching $M$ en $1$ arista (incrementar $\vert{}M\vert{}$ en $1$).

Para extender $M$, ejecutamos una búsqueda de camino aumentante (tipo BFS/Edmonds-Karp) sobre el subgrafo de ceros:

1. **Costo de exploración del grafo de ceros:**
  - Cada vez que una fila $x$ entra en $S$, recorremos sus $n$ columnas buscando ceros ($O(n)$). Como una fila entra en $S$ a lo sumo una vez por fase de extensión, el escaneo total de filas es $O(n^2)$.
  - Cada vez que una columna $y$ entra en $\Gamma(S)$, verificamos en $O(1)$ si está libre (en cuyo caso encontramos un camino aumentante) o si está matcheada con alguna fila $z$ (en cuyo caso etiquetamos $z$ y la agregamos a $S$).

2. **Operación de Cambio de Matriz:**
   Si la exploración termina sin encontrar una columna libre, cada columna de $\Gamma(S)$ está emparejada con una fila distinta de $S \setminus \{x_0\}$. Recíprocamente, cada fila de $S \setminus \{x_0\}$ fue alcanzada desde su columna emparejada en $\Gamma(S)$. Por tanto, $|S| = |\Gamma(S)| + 1$, en particular $|S| > |\Gamma(S)|$ y $|\Gamma(S)| \le n-1$. Así, $\overline{\Gamma(S)} \ne \emptyset$. Como la búsqueda examinó los ceros desde todas las filas alcanzables, no hay ceros en $S \times \overline{\Gamma(S)}$. Los costos reducidos son finitos y no negativos; por ello,
   $$0 < m = \min \{ C_{x,y} : x \in S,\ y \in \overline{\Gamma(S)} \} < +\infty.$$
   Procedemos a actualizar $C$:
- **Restamos $m$** a todas las filas $x \in S$.
- **Sumamos $m$** a todas las columnas $y \in \Gamma(S)$.

   *(Nota: Esta operación equivale a restar $m$ en $S \times \overline{\Gamma(S)}$, sumar $m$ en $\overline{S} \times \Gamma(S)$, y dejar intactos los cuadrantes $S \times \Gamma(S)$ y $\overline{S} \times \overline{\Gamma(S)}$).*

   Tanto hallar $m$ como actualizar las entradas de la matriz toma tiempo $\omega = O(n^2)$.

##### Lema de Invarianza y Progreso (Acotación de cambios de matriz)

> **Lema 1.** *Tras un cambio de matriz, las aristas del matching $M$ y del árbol alternante actual siguen teniendo costo cero. Además, al continuar la búsqueda sin reiniciar, o bien se extiende el matching $M$, o bien el conjunto $S$ crece estrictamente ($\vert{}S_{\text{nuevo}}\vert{} > \vert{}S\vert{}$).*

**Prueba del Lema 1:**

- **Preservación de ceros actuales:** Los únicos elementos cuyo valor aumenta están en $\overline{S} \times \Gamma(S)$. Sin embargo, toda columna $y \in \Gamma(S)$ ya fue revisada; si estuviera matcheada con una fila $x$, dicha fila $x$ habría sido agregada a $S$. Por lo tanto, no existe ninguna arista de $M$ ni arista explorada del árbol alternante en $\overline{S} \times \Gamma(S)$. Ningún cero relevante se pierde.
- **No negatividad:** En $S \times \overline{\Gamma(S)}$, cada entrada es al menos $m$, así que al restar $m$ sigue siendo no negativa. En $\overline{S} \times \Gamma(S)$ las entradas aumentan en $m$; en $S \times \Gamma(S)$ quedan iguales porque a la fila se le resta $m$ y a la columna se le suma $m$; y en $\overline{S} \times \overline{\Gamma(S)}$ no cambian. Por lo tanto, $C'_{x,y} \ge 0$ para toda entrada.
- **Progreso estricto:** Por definición de $m$, aparece al menos un nuevo cero en algún par $(x, y) \in S \times \overline{\Gamma(S)}$. Como $x \in S$ y $y \notin \Gamma(S)$, la columna $y$ recibe una etiqueta desde $x$ y entra en $\Gamma(S)$. Al revisar $y$:
  - Si $y$ está libre, se completa un camino aumentante y $|M|$ aumenta en $1$.
  - Si $y$ está matcheada con una fila $z$, entonces $z \notin S$: toda fila no raíz de $S$ entró por su columna emparejada, que ya está en $\Gamma(S)$, y la raíz $x_0$ es libre. Se etiqueta $z$ y se agrega a $S$, logrando $|S_{\text{nuevo}}| \ge |S| + 1$. $\blacksquare$

##### Cálculo Final para $O(n^4)$

Dado que $1 \le \vert{}S\vert{} \le n$, por el Lema 1 pueden ocurrir a lo sumo $\phi \le n = O(n)$ cambios de matriz antes de que el matching se extienda. Así, el costo de una fase de extensión es:

$$T_{\text{ext}} = \underbrace{O(n^2)}_{\text{exploración BFS}} + \underbrace{O(n)}_{\phi\text{ cambios}} \cdot \underbrace{O(n^2)}_{\omega\text{ actualización}} = O(n^3)$$

Sustituyendo en la complejidad total:

$$T_{\text{total}} = O(n^2) + O(n) \cdot O(n^3) = O(n^4)$$

*(Nota sobre la versión $O(n^5)$: En esta variante se inicia cada búsqueda desde todas las filas libres para obtener el conjunto canónico de alcanzables que determina el cubrimiento mínimo de König. Si después de cada cambio de matriz se descartan tanto las etiquetas como $M$, se recalcula desde cero un matching máximo del grafo de ceros mediante caminos aumentantes. El costo por recálculo es $O(n^3)$: hay a lo sumo $n$ aumentos, cada uno con una búsqueda de $O(n^2)$. Mientras el tamaño del matching máximo no aumente, el conjunto de filas alcanzables desde todas las filas libres es independiente del matching máximo elegido (propiedad de la descomposición de Dulmage–Mendelsohn). Cada cambio agrega al menos una fila alcanzable nueva: el nuevo cero alcanza una columna antes no alcanzable y, si está emparejada, su fila pareja no pertenecía al conjunto previo; si está libre, el matching máximo aumenta y comienza una nueva fase. Hay a lo sumo $O(n)$ cambios por fase y $O(n)$ fases, es decir, $O(n^2)$ recálculos. Así, esta implementación tiene cota $O(n^2)\cdot O(n^3)=O(n^5)$. Esta cota usa el algoritmo de caminos aumentantes BFS/DFS; Hopcroft–Karp daría $O(n^{2.5})$ por recálculo y una cota $O(n^{4.5})$ para esta variante.)*

---

#### 3. Teorema 2: Complejidad $O(n^3)$ (Implementación Optimizada)

> **Teorema 2.** *Mediante el uso de potenciales duales (variables de ajuste por fila y columna) y un arreglo auxiliar de mínimos por columna, el Algoritmo Húngaro puede implementarse en tiempo $O(n^3)$.*

##### Demostración

Para reducir la complejidad total de $O(n^4)$ a $O(n^3)$, debemos reducir el costo amortizado de cada cambio de matriz de $\omega = O(n^2)$ a **$O(n)$** dentro de una misma fase de extensión. Esto se logra mediante dos optimizaciones estructurales, bajo el modelo de costo uniforme donde las operaciones aritméticas y comparaciones toman $O(1)$.

**Persistencia entre fases.** Los potenciales $RF$ y $SC$ se inicializan en cero una sola vez, después de las reducciones iniciales de filas y columnas, y se conservan hasta terminar el algoritmo. Al comenzar cada fase nueva sí se reinician la raíz, $S$, $\Gamma(S)$, la cola, `pred` y $mS$. Al aumentar el matching, las aristas nuevas vienen del camino aumentante y tienen costo reducido cero; las aristas emparejadas que se quitan dejan de pertenecer al matching. Por tanto, el matching actualizado sigue formado por ceros efectivos de $\tilde C$.

###### Truco 1: Actualización implícita mediante potenciales ($O(n)$)

En lugar de modificar las $n^2$ entradas de $C$, mantenemos dos arreglos de dimensión $n$:

- $RF[x]$: valor acumulado restado a la fila $x$ (inicializado en $0$).
- $SC[y]$: valor acumulado sumado a la columna $y$ (inicializado en $0$).

El costo efectivo (reducido) de cualquier entrada $(x,y)$ se calcula en tiempo $O(1)$ al vuelo como:

$$\tilde{C}_{x,y} = C_{x,y} - RF[x] + SC[y]$$

Cuando ocurre un cambio de matriz con mínimo $m$:

- Para cada $x \in S$: $RF[x] \leftarrow RF[x] + m$
- Para cada $y \in \Gamma(S)$: $SC[y] \leftarrow SC[y] + m$ *(Nota: como la fórmula efectiva es $C_{x,y} - RF[x] + SC[y]$, sumar $m$ a la columna equivale a incrementar $SC[y]$ en $m$).*

Actualizar ambos arreglos toma exactamente **$O(n)$** operaciones en lugar de $O(n^2)$.

###### Truco 2: Cálculo de $m$ en $O(n)$ mediante mínimos incrementales

Observemos que:

$$m = \min_{y \in \overline{\Gamma(S)}} \left( \min_{x \in S} \tilde{C}_{x,y} \right)$$

Definimos un arreglo $mS[y]$ de tamaño $n$ (*slack* o holgura de la columna $y$) que almacena, para cada columna $y \notin \Gamma(S)$, el menor costo efectivo observado desde el subconjunto $S_{\mathrm{esc}} \subseteq S$ de filas ya procesadas por BFS, junto con un arreglo auxiliar `pred[y]` que registra qué fila $x \in S_{\mathrm{esc}}$ alcanza dicho mínimo (para reconstruir el camino aumentante en $O(1)$ por arista):

$$mS[y] = \min_{x \in S_{\mathrm{esc}}} \{ C_{x,y} - RF[x] + SC[y] \}$$

**Invariante de holgura y ciclo de vida de $mS[y]$ durante una fase de extensión:**

En todo momento de la fase y para cada columna $y \notin \Gamma(S)$, se mantiene la igualdad anterior y `pred[y]` es una fila de $S_{\mathrm{esc}}$ que alcanza ese mínimo. Si $S_{\mathrm{esc}}$ está vacío, $mS[y]=+\infty$. Solo cuando la cola queda vacía se cumple $S_{\mathrm{esc}}=S$; en ese momento el mínimo global para el próximo cambio de matriz es $m = \min_{y \notin \Gamma(S)} mS[y]$.

1. **Inicialización (al inicio de cada extensión, $O(n)$):**
   Se inicializa $mS[y] = +\infty$ y `pred[y] = -1` para todo $y \in \{1, \dots, n\}$.

2. **Al agregar una fila $x$ a $S$ ($O(n)$ por fila):**
   Cuando escaneamos la fila $x$ recorriendo todas las columnas no etiquetadas $y \notin \Gamma(S)$ *(ignorando las columnas que ya pertenecen a $\Gamma(S)$ para no sobrescribir sus predecesores ni crear ciclos)*:
- Calculamos `temp = C[x][y] - RF[x] + SC[y]`.
- Si `temp == 0`, etiquetamos la columna $y$ con predecesor $x$, la agregamos a $\Gamma(S)$ y verificamos si $y$ está libre (completando el camino aumentante) o matcheada con alguna fila $z$ (en cuyo caso agregamos $z$ a $S$ y la encolamos en el BFS).
- Si `temp > 0`, actualizamos el mínimo de la columna y su predecesor en $O(1)$:
     ```text
     if temp < mS[y]:
         mS[y] = temp
         pred[y] = x
     ```
   Como cada fila entra en $S$ a lo sumo una vez por extensión, todas estas actualizaciones cuestan $O(n^2)$ acumulado en toda la fase.

   La guarda $y \notin \Gamma(S)$ se aplica también cuando `temp == 0`: cada columna se procesa una sola vez y su predecesor no se sobrescribe. En particular, la columna emparejada que condujo a una fila no puede volver a apuntar a esa misma fila, así que la reconstrucción no forma ciclos.

3. **Al necesitar un cambio de matriz ($O(n)$):**
- Hallamos $m$ recorriendo únicamente las columnas no etiquetadas ($y \notin \Gamma(S)$):
     $$
     m = \min \{ mS[y] : y \notin \Gamma(S) \}
     $$
     Como la cola está vacía, todos los ceros alcanzables ya se procesaron; por eso las holguras pendientes son estrictamente positivas. Hay columnas pendientes porque $|S|>|\Gamma(S)|$, y todas las holguras son finitas al ser completa la matriz. Así, $0<m<+\infty$. Hallar el mínimo toma **$O(n)$**.
- Actualizamos los potenciales:
  - $RF[x] \leftarrow RF[x] + m$ para todo $x \in S$ ($O(n)$).
  - $SC[y] \leftarrow SC[y] + m$ para todo $y \in \Gamma(S)$ ($O(n)$).
- **Actualización crítica de $mS[y]$:** Como a todas las filas $x \in S$ se les restó $m$ (es decir, $RF[x]$ aumentó en $m$), el mínimo efectivo $\tilde{C}_{x,y}$ para toda columna $y \notin \Gamma(S)$ disminuyó exactamente en $m$. Por lo tanto, actualizamos $mS$ en **$O(n)$** sin mirar la matriz:
     $$
     \forall y \notin \Gamma(S): \quad mS[y] \leftarrow mS[y] - m
     $$
- Se procesan todas las columnas pendientes cuyo $mS[y]$ pasó a valer $0$. Cada una tiene un cero efectivo con la fila `pred[y]`, así que se etiqueta con ese predecesor y entra en $\Gamma(S)$. Se verifica si está libre (para aumentar $M$ y cerrar la fase) o emparejada con una fila $z$; en este último caso se agrega $z$ a $S$ y se encola si todavía no pertenecía a $S$. Se procesan las filas encoladas y los ceros que alcancen antes de calcular otro mínimo global o hacer otro cambio de matriz.

**Por qué se conserva el invariante.** Al restar $m$ a todas las holguras de las columnas aún no etiquetadas, cada candidata calculada desde una fila de $S_{\mathrm{esc}}$ disminuye por la misma cantidad. El mínimo se reduce exactamente en $m$ y su fila minimizadora `pred[y]` no cambia. Al procesar una fila nueva $z$, se calcula $\tilde C_{z,y}$ con los potenciales vigentes y se compara con el mínimo ya actualizado; ambas cantidades usan los mismos $RF$ y $SC$. Si una columna se vuelve cero, se la saca de las columnas pendientes y se procesa de inmediato; toda fila emparejada nueva se encola y se escanea antes de calcular otro mínimo global. Por inducción sobre las filas procesadas y los cambios de matriz, el invariante vale hasta que la búsqueda encuentra una columna libre o se vacía la cola.

En cada cambio de matriz, al menos una columna pendiente alcanza holgura cero. Si está libre se aumenta el matching; si está emparejada con una fila $z$, entonces $z \notin S$: si $z$ ya hubiera sido escaneada, su lado emparejado a esa columna habría hecho que la columna estuviera etiquetada antes. Por tanto, se incorpora una fila nueva. Así hay a lo sumo $n$ cambios por fase.

##### Cálculo Final para $O(n^3)$

En cada una de las $O(n)$ fases de extensión:

- Explorar las filas agregadas a $S$ y actualizar $mS$: $O(n^2)$.
- Realizar hasta $O(n)$ cambios de matriz, cada uno costando $O(n)$ (hallar $m$, actualizar $RF$, $SC$ y restar $m$ a $mS$): $O(n) \cdot O(n) = O(n^2)$.

Por lo tanto, el costo de una fase de extensión es:

$$
T_{\text{ext}} = O(n^2) + O(n^2) = O(n^2)
$$

Multiplicando por las $O(n)$ extensiones necesarias para completar el matching perfecto:

$$
T_{\text{total}} = O(n^2) + O(n) \cdot O(n^2) = O(n^3) \quad \blacksquare
$$

**Alcance del modelo aritmético.** Para costos reales, la prueba es en el modelo RAM real: suma, resta y comparación exactas cuestan $O(1)$. Para costos enteros exactos, el número de operaciones es $O(n^3)$ e independiente de la magnitud. Además, los operandos intermedios tienen tamaño polinomial: si $U$ es el costo máximo tras la reducción inicial, el costo óptimo es a lo sumo $nU$. En cada cambio de matriz el objetivo dual $\sum_x RF[x]-\sum_y SC[y]$ aumenta en $m$, pues $|S|-|\Gamma(S)|=1$, y no supera el costo óptimo. Por tanto, la suma de todos los incrementos $m$ es a lo sumo $nU$; como $RF$ y $SC$ solo acumulan incrementos no negativos, cada uno queda acotado por $nU$. Sus longitudes binarias son entonces $O(\log n+\log U)$. Para costos racionales exactos, se puede multiplicar la matriz por el producto de los denominadores y aplicar el mismo argumento; ese producto tiene longitud binaria a lo sumo la suma de las longitudes de los denominadores de entrada. Así se obtiene la cota fuertemente polinomial para entradas enteras o racionales exactas. En punto flotante, las pruebas de igualdad `temp == 0` y $mS[y]=0$ no son confiables en general: errores de redondeo pueden ocultar ceros o producir falsos ceros, por lo que la prueba no garantiza corrección ni terminación sin una política numérica adicional.
