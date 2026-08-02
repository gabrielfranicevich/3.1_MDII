# Teoremas Hasta Agosto


<div style="page-break-before: always;"></div>

## Complejidad del Algoritmo de Edmonds-Karp

### Pregunta 1

> ¿Cuál es la complejidad del algoritmo de Edmonds-Karp? Probarlo.
> (Nota: en la prueba se definen unas distancias,
> y se prueba que esas distancias no disminuyen en pasos sucesivos de EK.
> Ud. puede usar esto sin necesidad de probarlo)
> (casi toda la prueba de este teorema valdría para por ejemplo Ford Fulkerson,
> excepto por unas partes claves de la prueba donde se usa una
> propiedad especifica de Edmonds-Karp. Si usted escribe toda la prueba
> sin destacar que propiedad de Edmonds-Karp se usa y donde, el ejercicio no
> está aprobado.)

### Respuesta 1

La complejidad del algoritmo de Edmonds-Karp es **$O(nm^2)$** (o $O(VE^2)$),
donde $n$ es el número de vértices y $m$ el número de aristas de la red.

A continuación se presenta la prueba, destacando los puntos donde se utiliza la
propiedad específica de Edmonds-Karp que lo diferencia de un Ford-Fulkerson genérico.

#### 1. Costo por iteración

El algoritmo de Edmonds-Karp es una variante de Ford-Fulkerson que elige siempre
el camino aumentante más corto utilizando una búsqueda en anchura (**BFS**).

* Realizar una búsqueda **BFS** tiene una complejidad de **$O(m)$**.
* Actualizar el flujo a lo largo del camino encontrado es proporcional
a la longitud del camino, la cual está acotada por **$O(n)$**.
* Dado que en redes conectadas $n \leq m + 1$, el costo total de cada iteración
(encontrar y aumentar flujo) es **$O(m)$**.

#### 2. Definición de Lados Críticos

Un lado $\vec{xy}$ se denomina **crítico** en una iteración $k$ si es utilizado
para aumentar el flujo y resulta que:

1. Se usa en modo **forward** y se **satura** ($f_{k+1}(x,y) = c(x,y)$).
2. Se usa en modo **backward** y se **vacía** ($f_{k+1}(x,y) = 0$).

En cada iteración del algoritmo, **al menos un lado del camino aumentante debe
volverse crítico**.

#### 3. Acotación de la cantidad de aumentos (La Prueba)

Para probar la complejidad, debemos contar cuántas veces puede un mismo lado
$\vec{xy}$ volverse crítico. Para ello usamos las distancias $d_k(x)$
(desde la fuente $s$) y $b_k(x)$ (hacia el resumidero $t$), las cuales,
como se indica en la nota, son monótonas crecientes:
$d_k(x) \leq d_{k+1}(x)$ y $b_k(x) \leq b_{k+1}(x)$.

##### El uso de la propiedad específica de Edmonds-Karp

Aquí es donde la elección del camino más corto es fundamental:

* Supongamos que el lado $\vec{xy}$ se vuelve crítico en el paso $k$
siendo usado en modo forward. **PROPIEDAD EK:** Al ser un camino de
longitud mínima encontrado por BFS, se cumple necesariamente que
**$d_k(y) = d_k(x) + 1$**.
* Para que este mismo lado pueda volver a ser crítico en un paso posterior
$j > k$, su flujo debe haber sido modificado en la dirección opuesta en algún
paso intermedio $i$ ($k < i < j$).
* Para que el flujo disminuya en la dirección $x \to y$ en el paso $i$,
el algoritmo debe haber usado el lado en modo backward ($y \to x$).
**PROPIEDAD EK:** Nuevamente, como se usa el camino más corto,
en ese paso $i$ se debe cumplir que **$d_i(x) = d_i(y) + 1$**.

##### Análisis del incremento de distancia

Combinando estas igualdades con la propiedad de que las distancias no disminuyen
($d_k \leq d_i$ y $b_k \leq b_i$):

1. En el paso $i$: $d_i(t) = d_i(x) + b_i(x)$.
2. Sustituyendo la propiedad de EK del paso $i$: $d_i(t) = (d_i(y) + 1) + b_i(x)$.
3. Aplicando la monotonía de las distancias desde el paso $k$:
$d_i(t) \geq (d_k(y) + 1) + b_k(x)$.
4. Sustituyendo la propiedad de EK del paso $k$:
$d_i(t) \geq (d_k(x) + 1) + 1 + b_k(x) = d_k(x) + b_k(x) + 2$.
5. Como $d_k(t) = d_k(x) + b_k(x)$, concluimos que
**$d_j(t) \geq d_i(t) \geq d_k(t) + 2$**.

#### 4. Conclusión

* Cada vez que un lado vuelve a ser crítico, la distancia total entre $s$ y $t$
en la red residual debe aumentar en al menos **2 unidades**.
* La distancia entre $s$ y $t$ puede variar solamente entre $1$ y **$n-1$**.
* Por lo tanto, un lado puede ser crítico a lo sumo **$O(n)$** veces
(específicamente $\frac{n-1}{2}$ veces).
* Como hay $m$ lados y cada uno puede ser crítico $O(n)$ veces,
y cada camino aumenta la cantidad de lados críticos,
el número total de caminos aumentantes es **$O(nm)$**.
* Multiplicando los **$O(nm)$** caminos por el costo de **$O(m)$** de cada BFS,
obtenemos la complejidad final: **$O(nm^2)$**.


<div style="page-break-before: always;"></div>

## Lema de las distancias

### Pregunta 2

> Probar que si, dados vértices x, z y flujo f
> definimos a la distancia entre x y z relativa a f como
> la longitud del menor f-camino aumentante entre x y z,
> si es que existe tal camino, o infinito si no existe o 0 si x = z,
> denotandola por $d_f(x, z)$, y definimos $b_k(x) = b_{f_k}(x, t)$,
> donde fk es el k-esimo flujo en una corrida de Edmonds-Karp, entonces $b_k(x) \le b_{k+1}(x)$.
> (casi toda la prueba de este teorema valdría para por ejemplo Ford Fulkerson,
> excepto por unas partes claves de la prueba donde se usa una
> propiedad especifica de Edmonds-Karp. Si usted escribe toda la prueba
> sin destacar que propiedad de Edmonds-Karp se usa y donde, el ejercicio no
> está aprobado.)

### Respuesta 2

Para probar que las distancias al resumidero no disminuyen en pasos sucesivos
del algoritmo de Edmonds-Karp, es decir, que **$b_k(x) \leq b_{k+1}(x)$**
para todo vértice $x$ (donde $b_k(x)$ es la longitud del menor $f_k$-camino
aumentante desde $x$ hasta $t$), procederemos mediante el método del **absurdo**.

#### 1. Suposición inicial

Supongamos que el teorema es falso y que existen vértices para los cuales la
distancia al resumidero disminuye en el paso $k+1$.
Definimos el conjunto de estos vértices como:
**$A = \{y \in V : b_{k+1}(y) < b_k(y)\}$**
Asumimos que **$A \neq \emptyset$**.

#### 2. Elección del vértice mínimo

De entre todos los elementos de $A$, elegimos un vértice **$x$** cuya
distancia al resumidero en el flujo $f_{k+1}$ sea la **mínima** posible:
**$b_{k+1}(x) = \min \{b_{k+1}(y) : y \in A\}$**.

* Notamos que **$x \neq t$**, dado que $b_k(t) = b_{k+1}(t) = 0$,
por lo que $t$ no puede pertenecer a $A$.
* Como $x \in A$, sabemos que $b_{k+1}(x) < b_k(x) \leq \infty$,
lo que implica que existe un camino aumentante relativo a
$f_{k+1}$ entre $x$ y $t$.

#### 3. Análisis del camino en el paso $k+1$

Sea $P_{k+1}$ un camino aumentante de longitud mínima desde $x$ hasta $t$
relativo al flujo $f_{k+1}$. Sea **$z$** el vértice inmediatamente posterior a
$x$ en dicho camino ($P_{k+1}: x \to z \to \dots \to t$).
Al ser $P_{k+1}$ un camino de longitud mínima, se cumple que la
distancia de $z$ a $t$ es exactamente una unidad menor que la de $x$:
**$b_{k+1}(z) = b_{k+1}(x) - 1$**.

#### 4. Relación de distancias entre $x$ y $z$

Debido a que $b_{k+1}(z) < b_{k+1}(x)$ y habiendo elegido a $x$ como el mínimo
en $A$, deducimos que **$z$ no pertenece a $A$**. Por lo tanto,
su distancia no disminuyó entre los pasos $k$ y $k+1$:
**$b_k(z) \leq b_{k+1}(z)$**.

Ahora relacionamos las distancias en el paso $k$:

1. Por suposición inicial: $b_k(x) > b_{k+1}(x)$.
2. Sustituyendo el valor de $b_{k+1}(x)$ obtenido en el punto 3:
$b_k(x) > b_{k+1}(z) + 1$.
3. Usando la propiedad de $z$: **$b_k(x) > b_k(z) + 1$**.

Esta desigualdad implica que el arco (virtual) $x \to z$
**no estaba disponible** en el paso $k$; de lo contrario, la distancia mínima
$b_k(x)$ sería a lo sumo $b_k(z) + 1$. Sin embargo, el arco $x \to z$ **sí está
disponible** en el paso $k+1$ (pues $z$ es el sucesor de $x$ en $P_{k+1}$).

La única forma de que un arco no disponible en el paso $k$ se vuelva disponible
en el paso $k+1$ es que el flujo haya sido modificado en la **dirección opuesta
($z \to x$)** durante el aumento de flujo de $f_k$ a $f_{k+1}$.

#### 5. Propiedad específica de Edmonds-Karp

> **PUNTO CLAVE:** El camino aumentante utilizado para pasar de
$f_k$ a $f_{k+1}$ debió pasar por el arco en dirección **$z \to x$**.
Como **Edmonds-Karp utiliza siempre el camino aumentante de longitud mínima
(BFS)**, el camino que se usó en el paso $k$ para ir de $s$ a $t$ y que contenía
el segmento $z \to x$ era un camino de distancia mínima.
>
> En consecuencia, en dicho camino, el vértice $z$ estaba exactamente a una
unidad más de distancia del resumidero que el vértice $x$:
> **$b_k(z) = b_k(x) + 1$**.

#### 6. Contradicción final

Si sustituimos este resultado en nuestra desigualdad del final del punto 4:
**$b_k(x) > b_k(z) + 1 = (b_k(x) + 1) + 1 = b_k(x) + 2$**
Esto nos lleva a la contradicción **$0 > 2$**.

Por lo tanto, la suposición de que el conjunto $A$ no es vacío es falsa.
Concluimos que **$b_k(x) \leq b_{k+1}(x)$** para todo vértice $x$
en cada iteración del algoritmo.


<div style="page-break-before: always;"></div>

## Probar que $d_k(x) \le d_{k+1}(x)$

### Pregunta 3

> Probar que si, dados vértices x, z y flujo f, definimos a la distancia entre
> x y z relativa a f como la longitud del menor f-camino aumentante entre x y z,
> si es que existe tal camino, o infinito si no existe o 0 si x = z,
> denotandola por $d_f(x, z)$, y definimos $d_k(x) = d_{f_k}(s, x)$, donde fk es el k-ésimo
> flujo en una corrida de Edmonds-Karp, entonces $d_k(x) \le d_{k+1}(x)$.
> (casi toda la prueba de este teorema valdría para por ejemplo Ford Fulkerson,
> excepto por unas partes claves de la prueba donde se usa una
> propiedad especifica de Edmonds-Karp. Si usted escribe toda la prueba
> sin destacar que propiedad de Edmonds-Karp se usa y donde, el ejercicio no
> está aprobado.)

### Respuesta 3

#### Prueba de $d_k(x) \leq d_{k+1}(x)$

**1. Suposición inicial (Reducción al absurdo):**
Supongamos que existen vértices para los cuales la distancia desde la fuente
disminuye en el paso $k+1$.
Sea $A = \{y : d_{k+1}(y) < d_k(y)\}$ el conjunto de estos vértices.
Asumimos que $A \neq \emptyset$.

**2. Elección del vértice mínimo:**
De entre todos los elementos de $A$,
elegimos un vértice **$x$** tal que su distancia desde la fuente
en el flujo $f_{k+1}$ sea la mínima posible:
$$d_{k+1}(x) = \min \{d_{k+1}(y) : y \in A\}$$.

* Como $d_k(s) = d_{k+1}(s) = 0$, sabemos que $s \notin A$.
* Como $x \in A$, entonces $d_{k+1}(x) < d_k(x) \leq \infty$,
lo que implica que existe un $f_{k+1}$-camino aumentante entre $s$ y $x$.

**3. Análisis del camino en el flujo $f_{k+1}$:**
Sea $P_{k+1}$ un camino aumentante de **longitud mínima** desde $s$ hasta $x$
relativo al flujo $f_{k+1}$. Sea **$z$** el vértice inmediatamente anterior a
$x$ en dicho camino ($s \dots z \to x$). Al ser un camino de longitud mínima,
se cumple que:
$$d_{k+1}(z) = d_{k+1}(x) - 1$$.

**4. Relación de distancias entre $x$ y $z$:**
Como $d_{k+1}(z) < d_{k+1}(x)$ y elegimos a $x$ como el mínimo en $A$,
deducimos que **$z \notin A$**. Por lo tanto, su distancia no disminuyó:
$$d_k(z) \leq d_{k+1}(z)$$.

Relacionando las distancias de $x$ y $z$ en el paso $k$:

1. Por suposición: $d_k(x) > d_{k+1}(x)$.
2. Sustituyendo el valor de $d_{k+1}(x)$: $d_k(x) > d_{k+1}(z) + 1$.
3. Usando la propiedad de $z$: **$d_k(x) > d_k(z) + 1$**.

**5. Uso de la PROPIEDAD ESPECÍFICA de Edmonds-Karp:**
La desigualdad anterior ($d_k(x) > d_k(z) + 1$) implica que el arco virtual
$z \to x$ **no estaba disponible** en el paso $k$ (si lo hubiera estado,
por definición de distancia mínima, $d_k(x)$ sería a lo sumo $d_k(z) + 1$).

Sin embargo, el arco $z \to x$ **sí está disponible** en el paso $k+1$.
La única forma en que un arco no disponible se vuelva disponible es que
el flujo haya sido modificado en la **dirección opuesta ($x \to z$)**
durante el aumento de flujo anterior (de $f_k$ a $f_{k+1}$).

> **PUNTO CLAVE:** En un algoritmo de Ford-Fulkerson genérico, solo sabríamos
que el camino usado en el paso $k$ contenía el arco $x \to z$.
Pero en **Edmonds-Karp**, sabemos que ese camino era de **longitud mínima**
(encontrado por BFS). Por lo tanto, en el paso $k$, el vértice $x$ era el
predecesor de $z$ en un camino de distancia mínima,
lo que implica necesariamente que:
> $$d_k(z) = d_k(x) + 1$$.

**6. Contradicción final:**
Si sustituimos este resultado en nuestra desigualdad del final del punto 4:
$$d_k(x) > d_k(z) + 1 = (d_k(x) + 1) + 1 = d_k(x) + 2$$
Esto nos lleva a la contradicción **$0 > 2$**.

**Conclusión:** Nuestra suposición de que $A$ no es vacío es falsa.
Por lo tanto, **$d_k(x) \leq d_{k+1}(x)$** para todo vértice $x$ y
para toda iteración $k$.


<div style="page-break-before: always;"></div>

## Complejidad de Dinitz

### Pregunta 4

> ¿Cuál es la complejidad del algoritmo de Dinic? Probarla en ambas
> versiones: Dinitz original y Dinic-Even. (no hace falta probar que la
> distancia en networks auxiliares sucesivos aumenta). (Nota: en algunos
> examenes se puede pedir una sola de las dos versiones, en otros las dos)

### Respuesta 4

#### 1. Estructura General de la Complejidad

El algoritmo opera por fases, construyendo en cada una un
**network auxiliar (NA)** y hallando un **flujo bloqueante** en él.

* **Número de fases:**  
  Se asume que la cantidad de networks auxiliares es a lo sumo **$O(n)$**,
  dado que la distancia entre la fuente ($s$) y el resumidero ($t$)
  aumenta estrictamente en cada fase.
* **Costo por fase:**  
  Cada fase implica construir el NA mediante una búsqueda en anchura (**BFS**
  en $O(m)$ y hallar un flujo bloqueante (**CFB**).
* **Complejidad Total:**  
  $O(n) \cdot (O(m) + CFB)$. La prueba se reduce a demostrar que $CFB = O(nm)$
  para ambas versiones.

---

#### 2. Prueba para la Versión de Dinitz Original

En la versión original, el flujo bloqueante se halla mediante
búsquedas en profundidad (**DFS**) sucesivas.

* **Propiedad del NA:**  
   Dinitz mantiene el network auxiliar de forma proactiva para asegurar que
   **DFS nunca realice backtracking**; es decir,
   todo vértice con un lado entrante debe tener al menos un lado saliente
   (excepto $t$).
* **Operación PODAR:**
  Para mantener esta propiedad,
  tras encontrar cada camino aumentante,
  se realiza una operación de "poda" que elimina vértices y
  lados que ya no pueden llegar a $t$.
* **Análisis del Flujo Bloqueante ($O(nm)$):**
  1. **Búsqueda de caminos:** Como no hay backtracking,
  cada DFS toma **$O(n)$** pasos. Al haber como máximo $m$ caminos
  (cada uno satura al menos un lado), el costo total es **$O(nm)$**.
  2. **Costos de PODAR:**  
    Recorrer vértices buscando salidas:  
    $O(n)$ por cada uno de los $O(m)$ caminos, resultando en **$O(nm)$**.
    Borrar vértices y sus lados:  
    Cada vértice se borra una sola vez. La suma de los grados eliminados es
    $O(m)$ por el lema del apretón de manos.
* **Conclusión:** La suma de estos costos da $O(nm)$,
  lo que multiplicado por las $O(n)$ fases resulta en **$O(n^2m)$**.

---

#### 3. Prueba para la Versión de Dinic-Even

La versión de Even simplifica el proceso al no realizar la poda proactiva,
permitiendo que el DFS realice **backtracking**,
pero eliminando el lado (o vértice) inmediatamente después de retroceder por él
para "guardar" esa información.

* **Análisis por Palabras (A, R, I):**
  La ejecución se modela como una sucesión de operaciones:
  **A**vanzar, **R**etroceder e **I**ncrementar flujo.
  Se analizan subpalabras de la forma $A \dots AX$, donde $X$ es **R** o **I**.
* **Análisis del Flujo Bloqueante ($O(nm)$):**
  1. **Cantidad de palabras:**
     Cada palabra termina en una operación ($R$ o $I$) que borra permanentemente
     al menos un lado del NA.
     Por lo tanto, hay a lo sumo **$O(m)$** palabras.
  2. **Complejidad de cada palabra:**
     * Cada operación **A** (avanzar) y **R** (retroceder) es **$O(1)$**.
     * La operación **I** (incrementar) recorre el camino para aumentar flujo y
       borrar lados, lo cual es **$O(n)$**.
     * Como cada palabra puede tener a lo sumo $n$ avances (niveles del NA)
       antes de un $R$ o $I$, la complejidad de cada palabra es **$O(n)$**.
* **Conclusión:** El costo de hallar el flujo bloqueante es
  $O(m \text{ palabras}) \cdot O(n \text{ por palabra}) = O(nm)$.
  Al multiplicar por las $O(n)$ fases, se obtiene la complejidad final de **$O(n^2m)$**.


<div style="page-break-before: always;"></div>

## Complejidad de Wave

### Pregunta 5

> ¿Cual es la complejidad del algoritmo de Wave? Probarla. (no hace
> falta probar que la distancia en networks auxiliares sucesivos aumenta).
> (Nota: en algunos examenes se puede pedir que prueben la complejidad
> de una subparte de Wave, pej, la que denotamos S en el teorico)

### Respuesta 5

La complejidad total del algoritmo de **Wave (Tarjan)** es **$O(n^3)$**.

#### 1. Estructura general de la prueba

* **Cantidad de Networks Auxiliares:**
  Se asume que la distancia entre la fuente ($s$) y el resumidero ($t$) aumenta
  en cada fase, por lo que hay a lo sumo **$O(n)$** networks auxiliares.
* **Costo por Network Auxiliar:**
  La complejidad total es $O(n)$ multiplicado por la complejidad de hallar un
  **flujo bloqueante** en cada NA.
* **Costo del flujo bloqueante:**
  Se demuestra que hallar un flujo bloqueante mediante el método de "olas" de
  Wave tiene una complejidad de **$O(n^2)$**.
* **Conclusión:**
  $O(n \text{ fases}) \cdot O(n^2 \text{ por fase}) = \mathbf{O(n^3)}$.

#### 2. Prueba del paso bloqueante ($O(n^2)$)

Para hallar el flujo bloqueante, Wave realiza ciclos de "olas hacia adelante"
(procesando vértices desbalanceados con **FB**) y "olas hacia atrás"
(procesando vértices bloqueados con **BB**).
La complejidad se analiza contando cuántas veces se procesan los lados,
dividiéndolos en cuatro categorías: **$S, V, P$** y **$Q$**.

##### A. Análisis de $S$ (Saturaciones hacia adelante)

Se denota por **$S$** a los pasos donde un lado $\vec{xz}$ se **satura**
durante una ola hacia adelante.

* Para que un lado saturado vuelva a procesarse,
  primero debe desaturarse (devolver flujo).
* En Wave, un vértice $z$ solo devuelve flujo si está **bloqueado**.
* Si $z$ está bloqueado, el algoritmo garantiza que $x$ **nunca más** le enviará
  flujo hacia adelante en olas posteriores.
* Por lo tanto, cada lado se satura a lo sumo una vez. Como hay $m$ lados,
  **$S = O(m)$**.

##### B. Análisis de $V$ (Vaciados hacia atrás)

Se denota por **$V$** a los pasos donde un lado se **vacía** completamente
durante una ola hacia atrás.

* Bajo un razonamiento análogo al de $S$, un lado solo se vacía si el vértice
  está bloqueado, y un vértice bloqueado nunca vuelve a recibir flujo.
* Por ende, cada lado se vacía a lo sumo una vez: **$V = O(m)$**.

##### C. Análisis de $P$ y $Q$ (Flujos parciales)

* **$P$ (Ola hacia adelante):** En cada operación **FB(x)**,
  se intenta balancear el vértice enviando flujo a sus vecinos.
  Todos los lados que se miran se saturan (entran en $S$),
  **excepto a lo sumo el último**,
  que recibe el remanente del desbalanceo y queda parcialmente lleno (entra en $P$).
* **$Q$ (Ola hacia atrás):** En cada **BB(x)**,
  todos los lados se vacían (entran en $V$)
  **excepto a lo sumo uno** que queda con flujo parcial (entra en $Q$).
* La cantidad de estas operaciones depende del número de olas.
  En cada ciclo de olas (excepto el último),
  **al menos un vértice se bloquea** y nunca se desbloquea,
  por lo que hay **$O(n)$** olas.
* En cada ola hay $O(n)$ vértices.
  Así, **$P = O(n^2)$** y **$Q = O(n^2)$**.

#### 3. Conclusión de la Complejidad

Sumando las categorías, el costo de hallar el flujo bloqueante es
$S + V + P + Q = O(m) + O(m) + O(n^2) + O(n^2)$.
Dado que en cualquier grafo $m = O(n^2)$, el costo es **$O(n^2)$**.

Finalmente, al multiplicar por las $O(n)$ fases de networks auxiliares,
la complejidad total de Wave es **$O(n \cdot n^2) = O(n^3)$**.


<div style="page-break-before: always;"></div>

## Max Flow Min Cut

### Pregunta 7

> Probar que si f es un flujo maximal entonces existe un corte S tal que
> v(f) = cap(S).
> (puede usar sin necesidad de probarlo que si f es flujo y S es corte
> entonces $v(f) = f(S, \bar{S}) - f(\bar{S}, S)$)

### Respuesta 7

Para probar que si **$f$ es un flujo maximal**,
entonces existe un **corte $S$ tal que $v(f) = cap(S)$**,
se procede construyendo dicho corte a partir de
las propiedades de los caminos aumentantes en la red residual.

#### 1. Construcción del corte $S$

Definimos el conjunto **$S$** como el conjunto de todos los vértices alcanzables
desde la fuente mediante caminos aumentantes:  
**$S = \{s\} \cup \{x \in V : \text{exista un } f\text{-camino aumentante desde } s \text{ a } x\}$**.

Para que $S$ sea un corte válido,
la fuente $s$ debe pertenecer a él y el resumidero $t$ no:

* Por definición, **$s \in S$**.
* Si supusiéramos que **$t \in S$**, entonces existiría un $f$-camino aumentante
desde $s$ hasta $t$.
* Si existiera tal camino, el flujo $f$ podría aumentarse en una cantidad
$\epsilon > 0$, lo que contradice la hipótesis de que **$f$ es maximal**.
* Por lo tanto, **$t \notin S$**, y $S$ es efectivamente un corte.

#### 2. Cálculo del valor del flujo $v(f)$

Utilizando la propiedad establecida de que para cualquier flujo $f$ y corte $S$
se cumple:  
**$v(f) = f(S, \bar{S}) - f(\bar{S}, S)$**.

Analizaremos cada término considerando la pertenencia de los vértices a $S$ o a
su complemento $\bar{S}$:

##### Análisis de $f(S, \bar{S})$

Este término suma el flujo en los lados que salen de $S$ hacia $\bar{S}$. Sea un
lado $\vec{xz}$ tal que $x \in S$ y $z \notin S$:

* Como $x \in S$, hay un camino aumentante de $s$ a $x$.
* Si el flujo en el lado fuera menor que su capacidad
(**$f(\vec{xz}) < c(\vec{xz})$**), se podría extender el camino de $s$ a $x$
agregando $z$ como un lado **forward**, lo que significaría que $z \in S$.
* Dado que $z \notin S$, se concluye que todos estos lados están saturados:
**$f(\vec{xz}) = c(\vec{xz})$**.
* Por lo tanto, **$f(S, \bar{S}) = \sum c(\vec{xz}) = cap(S)$**.

##### Análisis de $f(\bar{S}, S)$

Este término suma el flujo en los lados que entran a $S$ desde $\bar{S}$.
Sea un lado $\vec{wx}$ tal que $w \notin S$ y $x \in S$:

* Como $x \in S$, hay un camino aumentante de $s$ a $x$.
* Si el flujo en el lado fuera mayor a cero (**$f(\vec{wx}) > 0$**),
se podría extender el camino de $s$ a $x$ agregando $w$ como un lado
**backward**, lo que implicaría que $w \in S$.
* Nuevamente, como $w \notin S$, el flujo debe ser nulo: **$f(\vec{wx}) = 0$**.
* En consecuencia, **$f(\bar{S}, S) = 0$**.

#### 3. Conclusión

Sustituyendo los resultados obtenidos:  
**$v(f) = cap(S) - 0 = cap(S)$**.

Esto prueba que para un flujo maximal existe un corte con capacidad
idéntica a su valor. Debido a que el valor de cualquier flujo está
acotado superiormente por la capacidad de cualquier corte,
se confirma además que este **$S$ es un corte minimal**.


<div style="page-break-before: always;"></div>

## Baby Brooks

### Pregunta 8

> Probar que si G es conexo no regular, entonces $\chi(G) \le \Delta(G)$.

### Respuesta 8

Para probar que si un grafo $G$ es **conexo y no regular**,
entonces su número cromático está acotado por su grado máximo
(**$\chi(G) \leq \Delta(G)$**), se utiliza un caso particular del
Teorema de Brooks, a veces llamado "Brooks para bebés".

La demostración se basa en encontrar un ordenamiento específico de los vértices
para que el algoritmo **Greedy** pueda colorear el grafo usando
a lo sumo $\Delta(G)$ colores.

#### 1. Selección del vértice raíz

Como $G$ no es regular, sabemos que el grado mínimo es estrictamente menor que
el grado máximo (**$\delta(G) < \Delta(G)$**).
Elegimos un vértice **$x$** tal que su grado sea mínimo: **$d(x) = \delta(G)$**.

#### 2. Construcción del orden de coloreo

1. Dado que $G$ es conexo, realizamos una búsqueda en anchura (**BFS**)
o en profundidad (**DFS**) partiendo desde $x$.
Esto permite alcanzar todos los vértices del grafo.
2. Definimos el orden de coloreo para el algoritmo Greedy como el
**orden inverso** al orden en que los vértices fueron incorporados al árbol de búsqueda.
3. En este orden inverso, el vértice raíz $x$ será el **último**
en ser coloreado ($v_n = x$).

#### 3. Coloreo de los vértices intermedios ($v_i$ con $i < n$)

Para cualquier vértice $v_i$ que no sea la raíz ($i < n$):

* En el orden de inserción de la búsqueda (BFS/DFS),
todo vértice distinto de la raíz es agregado al árbol por un vecino
que ya pertenecía a él.
* Esto implica que, en el **orden inverso**, cada vértice $v_i$ tiene al menos
un vecino que aparece **después** en el orden
(el vértice que lo "descubrió" en la búsqueda).
* Por lo tanto, cuando el algoritmo Greedy llega a $v_i$,
ese vecino posterior aún no ha sido coloreado.
Esto significa que $v_i$ tiene, como máximo, **$d(v_i) - 1$** vecinos ya coloreados.
* Como $d(v_i) \leq \Delta(G)$, el algoritmo descarta a lo sumo
**$\Delta(G) - 1$** colores.
* Siempre habrá al menos un color disponible en el conjunto
$\{1, 2, \dots, \Delta(G)\}$ para asignar a $v_i$.

#### 4. Coloreo del vértice raíz ($v_n = x$)

Cuando el algoritmo llega finalmente al último vértice, $x$:

* Todos sus vecinos ya han sido coloreados.
* El algoritmo Greedy descarta los colores de sus vecinos,
que son **$d(x)$** en total.
* Debido a la **no regularidad** del grafo, sabemos que **$d(x) = \delta(G) < \Delta(G)$**.
* Como el número de colores descartados es menor que $\Delta(G)$,
existe al menos un color disponible en el conjunto $\{1, 2, \dots, \Delta(G)\}$
para colorear a $x$.

#### Conclusión

Se ha obtenido un coloreo propio de $G$ utilizando, a lo sumo,
$\Delta(G)$ colores. Por lo tanto, queda probado que **$\chi(G) \leq \Delta(G)$**.


<div style="page-break-before: always;"></div>

## Hall

### Pregunta 9

> Enunciar y probar el Teorema de Hall.

### Respuesta 9

El **Teorema de Hall**, también conocido como el "teorema del matrimonio",
establece las condiciones necesarias y suficientes para la existencia de
un matching completo en un grafo bipartito.

#### Enunciado del Teorema

Sea $G = (V, E)$ un grafo bipartito con partes $X$ e $Y$.
Existe un **matching completo** de $X$ a $Y$
(es decir, un conjunto de lados disjuntos que cubren todos los vértices de $X$)
si y solo si para todo subconjunto $S \subseteq X$ se cumple que:
**$|S| \leq |\Gamma(S)|$**
Donde $\Gamma(S)$ denota el conjunto de todos los vecinos de los vértices de $S$
en la otra parte del grafo.

---

#### Demostración

La prueba se divide en dos partes: la necesidad (directa) y la suficiencia (recíproca).

##### 1. Necesidad ($\implies$)

Supongamos que existe un matching completo $M$ de $X$ a $Y$.
Este matching induce una **función inyectiva** $f: X \to Y$ tal que
para cada $x \in X$, $f(x)$ es el vértice con el que está apareado.

* Como $f$ es inyectiva, para cualquier subconjunto $S \subseteq X$,
la imagen $f(S)$ tiene el mismo tamaño que el conjunto original: $|f(S)| = |S|$.
* Por definición de vecindad,
todos los vértices apareados con elementos de $S$ pertenecen
a su conjunto de vecinos: $f(S) \subseteq \Gamma(S)$.
* Por lo tanto, la cardinalidad de los vecinos debe ser al menos tan grande como
la del subconjunto: **$|S| = |f(S)| \leq |\Gamma(S)|$**.

##### 2. Suficiencia ($\impliedby$)

Procedemos por el **método del absurdo** utilizando las propiedades de los
algoritmos de búsqueda de matching (como Edmonds-Karp o Dinic).

* **Suposición:**
Asumimos que la condición de Hall $|S| \leq |\Gamma(S)|$ se cumple
para todo $S \subseteq X$, pero que **no existe** un matching completo.
* Al correr el algoritmo para hallar el matching maximal,
nos quedaremos con un matching $M_{max}$
que deja al menos un vértice de $X$ sin cubrir.
* Sea **$S_0 \subseteq X$** el conjunto de vértices que quedaron sin matchear
($S_0 \neq \emptyset$).
* El algoritmo intenta encontrar caminos aumentantes etiquetando vértices.
Definimos:
  * **$S$** como el conjunto de filas (vértices de $X$)
  etiquetadas al finalizar la búsqueda.
  * **$T$** como el conjunto de columnas (vértices de $Y$)
  etiquetadas al finalizar la búsqueda.
* Debido a que el algoritmo se detuvo sin encontrar un camino aumentante:
  1. Cada vértice en $T$
  (excepto si fuera el fin de un camino, lo cual no ocurre aquí)
  debe estar matcheado con un vértice en $S \setminus S_0$.
  2. Esto implica que existe una correspondencia uno a uno entre $T$ y
  $S \setminus S_0$, por lo que **$|T| = |S| - |S_0|$**.
  3. Como $S_0$ no es vacío, se deduce que **$|S| > |T|$**.
* Finalmente, se demuestra que **$T = \Gamma(S)$**. Esto es porque:
  * $T \subseteq \Gamma(S)$ ya que las columnas solo se etiquetan a
  través de sus vecinos en $S$.
  * Si existiera un vecino $y \in \Gamma(S)$ que no estuviera en $T$,
  el algoritmo lo habría etiquetado al escanear su vecino en $S$,
  contradiciendo que $y \notin T$.
* **Conclusión:** Obtenemos que **$|\Gamma(S)| = |T| < |S|$**.
  Esto contradice la hipótesis inicial (la condición de Hall),
  lo cual es un absurdo. Por lo tanto, debe existir un matching completo.


<div style="page-break-before: always;"></div>

## König

### Pregunta 10

> Enunciar y probar el teorema del matrimonio de König.

### Respuesta 10

El **Teorema del matrimonio de Kőnig** (o König),
formulado originalmente en 1914,
establece una propiedad fundamental de los grafos bipartitos regulares.

#### Enunciado del Teorema de König

Todo **grafo bipartito regular** (es decir, un grafo donde $\chi(G) = 2$ y
todos sus vértices tienen el mismo grado $\Delta$) tiene un **matching perfecto**.

---

#### Demostración del Teorema de König

La prueba se basa en demostrar que todo grafo bipartito regular satisface la
**condición de Hall**, lo que garantiza la existencia de un matching completo
que, debido a la estructura del grafo, resulta ser perfecto.

**1. Igualdad de las partes:**
Sean $X$ e $Y$ las partes del grafo bipartito $G$ y sea $\Delta$
el grado de cada vértice.

* Si calculamos el número total de lados $|E|$ a partir de la parte $X$,
tenemos que $|E| = \Delta \cdot |X|$.
* Si hacemos lo mismo desde la parte $Y$, obtenemos $|E| = \Delta \cdot |Y|$.
* Igualando ambas expresiones ($\Delta \cdot |X| = \Delta \cdot |Y|$),
y asumiendo que el grafo tiene al menos un lado ($\Delta \neq 0$),
concluimos que **$|X| = |Y|$**. Dado que las partes son de igual tamaño,
cualquier matching completo sobre $X$ será automáticamente un matching perfecto.

**2. Verificación de la condición de Hall:**
Para probar que existe un matching completo sobre $X$,
debemos demostrar que para todo subconjunto $S \subseteq X$ se cumple que
$|S| \le |\Gamma(S)|$.

* Sea $S$ un subconjunto arbitrario de $X$.
* Definimos $E_S$ como el conjunto de lados que tienen un extremo en $S$.
Como el grafo es $\Delta$-regular,
el número de estos lados es exactamente **$|E_S| = \Delta \cdot |S|$**.
* Por definición, todos los vecinos de los vértices de $S$ se encuentran en el
conjunto $\Gamma(S) \subseteq Y$.
* Todos los lados de $E_S$ deben tener su otro extremo dentro de $\Gamma(S)$,
por lo que $E_S \subseteq E_{\Gamma(S)}$
(donde $E_{\Gamma(S)}$ son los lados incidentes a los vecinos de $S$).
* Esto implica que la cardinalidad de los lados incidentes a $S$
no puede superar a la de los incidentes a sus vecinos: **$|E_S| \le |E_{\Gamma(S)}|$**.
* Sabiendo que $|E_{\Gamma(S)}| = \Delta \cdot |\Gamma(S)|$ por la
regularidad del grafo, la desigualdad queda como:  
**$\Delta \cdot |S| \le \Delta \cdot |\Gamma(S)|$**.
* Al simplificar por $\Delta$, obtenemos **$|S| \le |\Gamma(S)|$**.

**Conclusión:**
Como se cumple la condición de Hall para cualquier subconjunto $S$,
el teorema de Hall asegura la existencia de un matching que cubre todo $X$.
Al haber probado previamente que $|X| = |Y|$,
dicho matching cubre todos los vértices del grafo,
siendo por lo tanto un **matching perfecto**.


<div style="page-break-before: always;"></div>

## Complejidad del algoritmo Hungaro

### Pregunta 11

> Demostrar la complejidad $O(n^4)$ del algoritmo Hungaro y dar una idea
> de como se puede obtener una complejidad $O(n^3)$

### Respuesta 11

La complejidad del algoritmo húngaro puede analizarse en dos niveles:  
una implementación estándar que resulta en **$O(n^4)$** y una optimizada
mediante técnicas de programación que alcanza **$O(n^3)$**.

#### 1. Demostración de la complejidad $O(n^4)$

El algoritmo busca un matching perfecto de costo mínimo en un grafo bipartito
con $n$ vértices en cada parte.
El análisis de su complejidad se desglosa de la siguiente manera:

* **Paso Inicial:**
  Restar el mínimo de cada fila y columna para asegurar la presencia de ceros
  en la matriz, junto con el cálculo del matching inicial de ceros,
  tiene un costo de **$O(n^2)$**.
* **Cantidad de Extensiones:** Para obtener un matching perfecto,
  se debe extender el matching parcial a lo sumo **$n$ veces**
  (una por cada lado del matching).
* **Costo de cada Extensión:**
  En cada intento de extender el matching se realizan dos tareas:
  1. **Revisión de filas y columnas:**
    Se buscan caminos aumentantes de ceros. Usando una implementación matricial,
    esto requiere revisar $n$ filas y $n$ columnas, con un costo de **$O(n^2)$**.
  2. **Cambios de matriz:**
    Si no se encuentra un camino aumentante,
    se debe modificar la matriz para agregar ceros.
      * **Número de cambios por extensión:**
        Debido a que cada cambio de matriz aumenta el tamaño del conjunto de
        filas etiquetadas ($S$) o extiende el matching, y $|S| \leq n$,
        puede haber a lo sumo **$O(n)$ cambios de matriz** antes de lograr una extensión.
      * **Costo de un cambio de matriz:**
        Se debe calcular $m = \min(S \times \Gamma(S)^c)$,
        lo cual es **$O(n^2)$**,
        y luego restar/sumar ese valor a las filas de $S$ y columnas
        de $\Gamma(S)$, lo cual es también **$O(n^2)$**.
* **Cálculo Final:**
    $$O(n^2) + O(n) \cdot [O(n^2) + O(n) \cdot O(n^2)] = O(n^2) + O(n) \cdot O(n^3) = \mathbf{O(n^4)}$$

---

#### 2. Idea para obtener una complejidad $O(n^3)$

Para reducir la complejidad a **$O(n^3)$**,
el objetivo es lograr que el cambio de matriz pase de $O(n^2)$ a **$O(n)$**.
Esto se consigue mediante dos "trucos" de implementación:

##### A. Actualización "indicada" (Lazy Update)

En lugar de recorrer toda la matriz para restar o sumar el valor $m$
(lo cual consume $O(n^2)$), se mantienen dos arreglos auxiliares: **$RF[x]$**
para las filas y **$SC[y]$** para las columnas.

* Cuando se debe restar $m$ a una fila o sumarlo a una columna,
  solo se actualiza la entrada correspondiente en el arreglo
  ($O(1)$ por fila/columna, $O(n)$ en total).
* Para verificar si una entrada es un "cero" durante la búsqueda,
  se calcula: `0 == C[x][y] - RF[x] + SC[y]`.
  Se asume que esta operación aritmética es $O(1)$.

##### B. Optimización del cálculo del mínimo ($m$)

Calcular el mínimo global de la región $S \times \Gamma(S)^c$ en cada paso
es costoso. La idea es mantener un arreglo **$MS[y]$** que guarde
el valor mínimo de la columna $y$ considerando únicamente las filas que ya
pertenecen al conjunto $S$.

* Cada vez que una nueva fila $x$ se agrega a $S$, se actualiza el arreglo $MS$
  recorriendo la fila una sola vez ($O(n)$).
* Al momento de necesitar el valor $m$, este será simplemente el
  **mínimo del arreglo $MS$** (excluyendo las columnas ya etiquetadas),
  lo cual se resuelve en **$O(n)$**.

Al hacer que el cambio de matriz sea $O(n)$,
la extensión de un lado del matching pasa a ser $O(n^2)$,
y el algoritmo total resulta en $O(n \cdot n^2) = **$O(n^3)$**$.


<div style="page-break-before: always;"></div>

## Teorema de la cota de Hamming

### Pregunta 12

> Enunciar el teorema de la cota de Hamming y probarlo

### Respuesta 12

El **Teorema de la Cota de Hamming** establece un límite superior
para la cantidad de palabras que puede tener un código binario de bloque,
basándose en su capacidad de corrección de errores.

#### Enunciado del Teorema de la Cota de Hamming

Sea **$C$** un código binario de longitud **$n$**,
con distancia mínima **$\delta = \delta(C)$** y sea
**$t = \lfloor \frac{\delta - 1}{2} \rfloor$** la cantidad de errores
que el código puede corregir.
Entonces, la cardinalidad del código satisface la siguiente desigualdad:
$$|C| \le \frac{2^n}{\sum_{i=0}^t \binom{n}{i}}$$

---

#### Demostración del Teorema de la Cota de Hamming

La prueba se basa en el concepto de empaquetamiento de esferas (discos)
en el espacio de palabras binarias de longitud $n$.

1. **Definición del conjunto de estudio:**
   Sea **$A = \bigcup_{v \in C} D_t(v)$**,
   donde **$D_t(v)$** representa el disco (o bola) de radio $t$ centrado en la
   palabra del código $v$.
2. **Propiedad de los discos:**
   Como **$t = \lfloor \frac{\delta - 1}{2} \rfloor$**,
   el código corrige hasta $t$ errores,
   lo que implica que los discos de radio $t$ centrados en palabras distintas
   del código son **disjuntos** entre sí. Por lo tanto,
   la unión que forma el conjunto $A$ es una unión disjunta.
3. **Cálculo de la cardinalidad de $A$:**
   Debido a la propiedad anterior,
   la cantidad de elementos en $A$ es la suma de las cardinalidades
   de cada disco: **$|A| = \sum_{v \in C} |D_t(v)|$**.
4. **Estructura del disco:**
   Cada disco $D_t(v)$ se puede descomponer en "capas" o superficies esféricas
   según la distancia de Hamming: **$D_t(v) = \bigcup_{r=0}^t S_r(v)$**,
   donde $S_r(v) = \{w \in \{0,1\}^n : d_H(v, w) = r\}$.
   Al ser estas superficies disjuntas, se tiene que **$|D_t(v)| = \sum_{r=0}^t |S_r(v)|$**.
5. **Cardinalidad de una superficie esférica:**
   Una palabra $w$ pertenece a $S_r(v)$ si difiere de $v$
   en exactamente $r$ bits de los $n$ posibles.
   Por lo tanto, existe una biyección entre el conjunto $S_r(v)$ y el conjunto
   de subconjuntos de $r$ índices elegidos de un total de $n$.
   De esto se deduce que la cardinalidad de cada superficie
   es el número combinatorio: **$|S_r(v)| = \binom{n}{r}$**.
6. **Sustitución en el tamaño del disco:**
   Al no depender el tamaño de la superficie de la palabra $v$ elegida,
   la cardinalidad de cualquier disco de radio $t$ es
   **$|D_t(v)| = \sum_{r=0}^t \binom{n}{r}$**.
7. **Conclusión final:**
   Al sustituir esto en la sumatoria total:
    * **$|A| = \sum_{v \in C} \left( \sum_{r=0}^t \binom{n}{r} \right) = |C| \cdot \sum_{r=0}^t \binom{n}{r}$**.
    * Dado que $A$ es un subconjunto del espacio total de palabras binarias de
    longitud $n$ ({0,1}$^n$), su cardinalidad no puede superar a la del espacio
    total: **$|A| \le 2^n$**.
    * Finalmente, despejando $|C|$, obtenemos:  
    **$|C| \le \frac{2^n}{\sum_{r=0}^t \binom{n}{r}}$**.


<div style="page-break-before: always;"></div>

## Matriz de Chequeo

### Pregunta 13

> Probar que si H es matriz de chequeo de C, entonces;
> $\delta(C) = \min\{j : \exists$ un conjunto de $j$ columnas LD de $H\}$
> (LD es "linealmente dependiente")

### Respuesta 13

Para probar que si $H$ es la matriz de chequeo de un código lineal $C$,
entonces la **distancia mínima $\delta(C)$ es igual al mínimo número de columnas
linealmente dependientes (LD) de $H$**,
se procede demostrando dos desigualdades fundamentales.

Definamos **$m$** como el mínimo número de columnas LD de $H$:
**$m = \min\{j : \exists \text{ un conjunto de } j \text{ columnas LD de } H\}$**.

#### 1. Prueba de que $m \leq \delta(C)$

Partimos de la existencia de una palabra en el código con el peso mínimo.

* Sea **$v \in C$** una palabra no nula tal que su peso de Hamming es la
  distancia mínima: **$|v| = \delta(C)$**.
* Esto significa que $v$ tiene exactamente $\delta(C)$ unos en las posiciones
$i_1, i_2, \dots, i_{\delta(C)}$.
* Podemos expresar a $v$ como la suma de vectores de la base canónica:
**$v = e_{i_1} + e_{i_2} + \dots + e_{i_{\delta(C)}}$**.
* Como $v$ pertenece al código $C$, y $C$ es el núcleo de $H$ ($C = Nu(H)$),
  se debe cumplir que **$Hv^t = 0$**.
* Desglosando la operación:
  $H(e_{i_1} + e_{i_2} + \dots + e_{i_{\delta(C)}})^t = H(e_{i_1})^t + H(e_{i_2})^t + \dots + H(e_{i_{\delta(C)}})^t = 0$.
* Dado que $H(e_j)^t$ es simplemente la **columna $j$-ésima de $H$**
  (denotada como $H^{(j)}$), tenemos que:
  **$H^{(i_1)} + H^{(i_2)} + \dots + H^{(i_{\delta(C)})} = 0$**.
* Esta igualdad muestra que existe un conjunto de $\delta(C)$ columnas de $H$
  que es linealmente dependiente.
* Por definición, $m$ es el tamaño del conjunto LD más pequeño, por lo tanto:
  **$m \leq \delta(C)$**.

#### 2. Prueba de que $\delta(C) \leq m$

Ahora partimos de un conjunto de columnas dependientes
para encontrar una palabra del código.

* Sea **$\{H^{(j_1)}, H^{(j_2)}, \dots, H^{(j_m)}\}$**
  un conjunto de $m$ columnas de $H$ que es linealmente dependiente.
* Por definición de dependencia lineal, existen escalares
  $c_1, c_2, \dots, c_m$ (no todos nulos) tales que
  **$c_1 H^{(j_1)} + c_2 H^{(j_2)} + \dots + c_m H^{(j_m)} = 0$**.
* En un código binario, los únicos escalares no nulos son $1$,
  por lo que esto equivale a decir que la suma de estas $m$ columnas es cero.
* Definimos el vector **$w = c_1 e_{j_1} + c_2 e_{j_2} + \dots + c_m e_{j_m}$**.
* Al calcular **$Hw^t$**, obtenemos la suma de las columnas:
  $Hw^t = c_1 H^{(j_1)} + \dots + c_m H^{(j_m)} = 0$.
* Como $Hw^t = 0$ y $w \neq 0$,
  entonces **$w$ es una palabra del código $C$**.
* El peso de Hamming de $w$ es a lo sumo $m$ ($|w| \leq m$).
* Como $\delta(C)$ es el mínimo peso de todas las palabras no nulas de $C$,
  concluimos que: **$\delta(C) \leq |w| \leq m$**.

#### Conclusión 13

Al haber probado que **$m \leq \delta(C)$** y que **$\delta(C) \leq m$**,
queda demostrado que:
$$
\delta(C) = \min\{j : \exists \text{ un conjunto de } j \text{ columnas LD de } H\}
$$


<div style="page-break-before: always;"></div>

## Código Ciclico

### Pregunta 14

> Sea C un código cíclico de dimensión k y longitud n  
> Sea g(x) su polinomio generador.  
> Probar que:
> >
> 1. C esta formado por los multiplos de g(x) de grado menor que n:  
> $C = \{p(x) : gr(p) < n \land g(x)|p(x)\}$
> 2. $C = \{v(x) \odot g(x) : v \text{ es un polinomio cualquiera}\}$
> 3. $gr(g(x)) = n - k$
> 4. $g(x)$ divide a $1 + x^n$

### Respuesta 14

Sea $C$ un código cíclico de dimensión $k$ y longitud $n$,
y sea $g(x)$ su polinomio generador. Por definición,
$g(x)$ es el único polinomio no nulo de grado mínimo en $C$.

A continuación se presentan las demostraciones para las cuatro propiedades solicitadas:

#### 1. $C$ está formado por los múltiplos de $g(x)$ de grado menor que $n$

Para probar que $C = \{p(x) : gr(p) < n \land g(x)|p(x)\}$,
demostraremos ambas inclusiones:

* **($\supseteq$):** Si $p(x) = q(x)g(x)$ y $gr(p) < n$,
  entonces $p(x) = p(x) \pmod{1+x^n}$. Por la definición del producto modular
  $\odot$, esto equivale a $p(x) = q(x) \odot g(x)$.
  Como veremos en el siguiente punto,
  cualquier producto modular de un polinomio por $g(x)$ pertenece a $C$.
* **($\subseteq$):** Sea $p(x) \in C$. Dividimos $p(x)$ por $g(x)$ obteniendo
  $p(x) = q(x)g(x) + r(x)$ con $gr(r) < gr(g)$.
  Despejando el resto: $r(x) = p(x) + q(x)g(x)$. Como $gr(r) < gr(g) < n$,
  entonces $r(x) = r(x) \pmod{1+x^n}$. Operando bajo el módulo:
    $$r(x) = (p(x) + q(x)g(x)) \pmod{1+x^n} = p(x) \pmod{1+x^n} + (q(x)g(x)) \pmod{1+x^n}$$
    $$r(x) = p(x) + q(x) \odot g(x)$$
    Como $p(x) \in C$ y $q(x) \odot g(x) \in C$ (por ser $C$ un ideal),
    su suma $r(x)$ también pertenece a $C$. Sin embargo, se supuso que
    $gr(r) < gr(g)$, y $g(x)$ es el polinomio no nulo de menor grado en $C$.
    La única posibilidad es que **$r(x) = 0$**,
    lo que implica que $g(x)$ divide a $p(x)$.

#### 2. $C = \{v(x) \odot g(x) : v \text{ es un polinomio cualquiera}\}$

* **($\subseteq$):** Como probamos arriba, si $p(x) \in C$,
  entonces $p(x) = q(x)g(x)$ con $gr(p) < n$,
  lo que implica $p(x) = q(x) \odot g(x)$.
* **($\supseteq$):** Sea $p(x) = v(x) \odot g(x)$.
  Desarrollando $v(x) = v_0 + v_1x + \dots + v_dx^d$:
    $$p(x) = v_0 \odot g(x) + v_1(x \odot g(x)) + \dots + v_d(x^d \odot g(x))$$
    Sabemos que $x \odot g(x) = rot(g(x))$.
    Dado que $g(x) \in C$ y $C$ es cíclico,
    todas las rotaciones de $g(x)$ pertenecen a $C$. Al ser $C$ también lineal,
    la combinación lineal de estas rotaciones pertenece al código.
    Por lo tanto, $p(x) \in C$.

#### 3. $gr(g(x)) = n - k$

A partir de la propiedad 1,
sabemos que $p(x) \in C \iff p(x) = q(x)g(x)$ con $gr(p) < n$.

* Para que $gr(q(x)g(x)) < n$, se debe cumplir que $gr(q) + gr(g) < n$,
  es decir, **$gr(q) < n - gr(g)$**.
* Existe entonces una biyección entre las palabras del código $C$ y el conjunto
  de polinomios $q(x)$ de grado menor a $n - gr(g)$.
* La cantidad de tales polinomios $q(x)$ es $2^{n - gr(g)}$
  (ya que cada uno de los $n - gr(g)$ coeficientes tiene 2 opciones: 0 o 1).
* Sabiendo que para un código lineal de dimensión $k$ la cardinalidad es
  $|C| = 2^k$, igualamos: $2^k = 2^{n - gr(g)}$.
* Concluimos que $k = n - gr(g)$, o equivalentemente, **$gr(g(x)) = n - k$**.

#### 4. $g(x)$ divide a $1 + x^n$

Dividimos el polinomio del módulo $1+x^n$ por $g(x)$:
$$1 + x^n = q(x)g(x) + r(x) \quad \text{con } gr(r) < gr(g)$$.
Despejamos el resto: $r(x) = 1 + x^n + q(x)g(x)$.
Tomamos módulo $1+x^n$ en ambos miembros:

* Como $gr(r) < gr(g) < n$, entonces $r(x) \pmod{1+x^n} = r(x)$.
* En el miembro derecho:
  $(1 + x^n + q(x)g(x)) \pmod{1+x^n} = (q(x)g(x)) \pmod{1+x^n} = q(x) \odot g(x)$.
  Resulta que $r(x) = q(x) \odot g(x)$, lo cual, por la propiedad 2,
  implica que **$r(x) \in C$**. Dado que $gr(r) < gr(g)$,
  y $g(x)$ es el elemento no nulo de grado mínimo,
  concluimos que **$r(x) = 0$**. Por lo tanto, $1 + x^n = q(x)g(x)$,
  lo que prueba que **$g(x) | 1 + x^n$**.


<div style="page-break-before: always;"></div>

## 3-COLOR es NP-completo

### Pregunta 15

> Probar que 3-COLOR es NP-completo.

### Respuesta 15

Para probar que el problema **3-COLOR**
(determinar si un grafo $G$ admite un coloreo propio con 3 colores)
es **NP-completo**, se deben demostrar dos condiciones fundamentales:
su pertenencia a la clase **NP** y su **NP-hard**.

#### 1. Pertenencia a la clase NP

Un problema pertenece a **NP** si una solución propuesta (un certificado)
puede verificarse en tiempo polinomial.

* **Certificado:**
  Una asignación de un color de $\{0, 1, 2\}$
  a cada uno de los $n$ vértices del grafo.
* **Verificación:**
  Consiste en recorrer todos los lados del grafo
  y verificar que para cada lado $uv$, el color de $u$ sea distinto al de $v$.
* **Complejidad:**
  Esta revisión toma tiempo **$O(m)$**,
  donde $m$ es el número de lados,
  lo cual es claramente polinomial respecto al tamaño de la entrada.

---

#### 2. Prueba de NP-hard (Reducción de 3SAT)

Para demostrar que es **NP-hard**,
reduciremos polinomialmente el problema **3SAT**
(que ya se sabe que es NP-completo) a **3-COLOR**.
Dada una instancia de 3SAT con variables $x_1, \dots, x_n$ y
disjunciones $D_1, \dots, D_m$ donde cada
$D_j = \ell_{j1} \lor \ell_{j2} \lor \ell_{j3}$,
construiremos un grafo $G$ tal que la expresión es satisfacible si y solo si
$\chi(G) = 3$.

##### Construcción del grafo $G$

La construcción debe ser polinomial en el tamaño de la expresión booleana.
El grafo se compone de:

1. **Vértices de literales:**
   Para cada variable $x_i$, creamos dos vértices, $v_{x_i}$ y $v_{\bar{x}_i}$.
2. **Vértices de cláusulas (Garras):**
   Para cada disyunción $D_j$, se agregan seis vértices
   $\{a_{j1}, a_{j2}, a_{j3}, q_{j1}, q_{j2}, q_{j3}\}$.
3. **Vértices especiales:**
   Se agregan dos vértices fundamentales llamados **$s$** (fuente/base) y
   **$t$** (resumidero/referencia).

##### Lados del grafo

1. **Estructura base:** Se une $s$ con $t$ ($st \in E$).
2. **Estructura de literales:**
   Para cada variable $x_i$, se forma un triángulo entre
   $v_{x_i}$, $v_{\bar{x}_i}$ y $t$. Esto obliga a que en un 3-coloreo,
   los literales solo puedan tomar los colores de $s$ o del "tercer color".
3. **Gadget de cláusula:**
   * Los vértices $\{a_{j1}, a_{j2}, a_{j3}\}$ forman un triángulo.
   * Cada $q_{jr}$ se conecta con $s$, con $a_{jr}$ y con el vértice del literal
   correspondiente $v(\ell_{jr})$.

##### Demostración de la equivalencia

**A. Si la expresión es satisfacible $\implies \chi(G) = 3$:**
Si existe un vector de bits que satisface la expresión, coloreamos:

* $c(s) = 1$, $c(t) = 2$ y el "tercer color" = 0.
* Si $x_i$ es verdadero, $c(v_{x_i}) = 1$ y $c(v_{\bar{x}_i}) = 0$. Si es falso,
  se invierten. Como están unidos a $t$ (color 2), esto es válido.
* En cada cláusula $D_j$, hay al menos un literal verdadero $\ell_{j,k_j}$.
  Coloreamos su correspondiente $a_{j,k_j}$ con 2.
  Los otros dos $a_{jr}$ reciben colores 1 y 0.
* Los conectores $q_{jr}$ se pueden colorear de forma que no haya conflictos:
  el que corresponde al literal verdadero tendrá color 0
  (distinto de $s=1$, $a=2$ y literal=1).

**B. Si $\chi(G) = 3 \implies$ la expresión es satisfacible:**
Sea $c$ un 3-coloreo propio. Definimos la asignación de verdad como:
$x_i$ es verdadero si $c(v_{x_i}) = c(s)$.

* Debido al triángulo con $t$, cada literal $v(\ell)$ tiene color $c(s)$ o el
  "tercer color".
* En el triángulo de la cláusula $D_j$, al menos un vértice $a_{jk}$ debe tener
  el color $c(t)$.
* Como $q_{jk}$ está unido a $s$ y a $a_{jk}$, su color debe ser el "tercer color".
* Finalmente, como $v(\ell_{jk})$ está unido a $q_{jk}$ (tercer color) y a $t$
  (color $c(t)$), su color debe ser obligatoriamente **$c(s)$**.
* Por definición de nuestra asignación, esto significa que el literal
  $\ell_{jk}$ es verdadero, por lo que la cláusula $D_j$ se satisface.

**Conclusión:** Al ser 3-COLOR un problema en NP al cual se reduce un problema
NP-completo (3SAT) en tiempo polinomial, queda probado que **3-COLOR es NP-completo**.
