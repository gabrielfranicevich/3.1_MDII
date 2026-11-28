---
fontsize: 11pt
geometry: margin=2cm
linestretch: 1.1
header-includes:
  - \usepackage{graphicx}
---

# Matchings y Matchings Pesados

---

## Matchings en Grafos Bipartitos

### 1. Definición y Problema

Dado un grafo $G$, un **matching** en $G$ es un subgrafo $M$ de $G$ tal que el grado (en $M$) de cada vértice de $M$ es uno. En otras palabras, un matching es un conjunto de lados disjuntos.

* **El problema a resolver:** Dado un grafo $G$ bipartito, encontrar un matching en $G$ con la mayor cantidad de lados posible.
* Un matching que tenga la mayor cantidad posible de lados se dice **matching maximal**.

Si bien hay problemas de matchings en grafos generales, nos concentraremos en matchings en grafos bipartitos porque:

1. Muchos problemas de aplicación sólo requieren un grafo bipartito.
2. El algoritmo para grafos no bipartitos se "apoya" en el algoritmo para bipartitos, así que hay que conocer este de todos modos. *(Nota: La persona que generalizó el algoritmo a grafos no bipartitos fue **Edmonds**, pero es mucho más complicado y no lo veremos).*

#### Ejemplo de aplicación

\mbox{}\newline

Supongamos que $G$ es un grafo cuyos vértices son tareas que hay que hacer y trabajadores que pueden hacer esas tareas. Se une un trabajador con una tarea si el trabajador es capaz de hacerla. Queremos encontrar la mayor cantidad posible de emparejamientos tarea/trabajador, es decir, un **matching maximal**.

*(Más adelante veremos una generalización: asociar un costo a cada par tarea/trabajador y buscar un matching maximal de costo mínimo).*

---

### 2. Reducción del problema a flujos en networks

Para resolver el problema aprovecharemos lo aprendido sobre flujos:

1. Transformamos el grafo bipartito no dirigido en un *network* (con todas sus capacidades enteras).
2. Encontramos un flujo entero maximal en ese *network* (sabemos por el Teorema de la Integralidad que existe, y algoritmos como Edmonds-Karp o Dinitz lo encuentran).
3. A partir de ese flujo maximal obtenemos un matching maximal.

\newpage

#### Transformación del grafo en network

\mbox{}\newline

Sea $G$ bipartito, con partes $X$ e $Y$. Es decir, $V(G) = X \cup Y$, $X \cap Y = \emptyset$, y no hay lados entre vértices de $X$ ni entre vértices de $Y$. Los únicos lados son entre algún vértice de $X$ y algún vértice de $Y$.

Para construir el *network* $N$, agregamos dos vértices nuevos: una fuente $s$ y un resumidero $t$. Los lados dirigidos del *network* serán:

* $\vec{xy}$ para todo $x \in X, y \in Y$ tal que $xy \in E(G)$.
* $\vec{sx}$ para todo $x \in X$.
* $\vec{yt}$ para todo $y \in Y$.
* **Capacidad $1$ en todos los lados.**

#### Relación entre matchings maximales en $G$ y flujos maximales enteros en $N$

\mbox{}\newline

Supongamos que tenemos un flujo entero $f$. Como la capacidad de todos los lados es $1$:

* Si $x \in X$, como sólo entra un lado a $x$ ($\vec{sx}$), entonces $\text{in}_f(x) = f(\vec{sx})$. 
  * Como $f$ es entero y la capacidad es $1$, $f(\vec{sx})$ es $1$ o $0$.
  * Por conservación de flujo:
    * $\text{out}_f(x) = \text{in}_f(x)$ también debe ser $1$ o $0$.
    * $\text{out}_f(x) > 0 \implies \text{out}_f(x) = 1$ y $\exists! y \in Y$ : $f(\vec{xy}) = 1$.
* Un razonamiento similar con $t$ aplica para los $y \in Y$:
  * o bien $\text{out}_f(y) = 0$,
  * o es $1$ (en cuyo caso existe un único $x \in X$ con $f(\vec{xy}) = 1$).

Concluimos que dado un flujo entero $f$, si definimos:

```{=latex}
\noindent\resizebox{\linewidth}{!}{\ensuremath{\mathbf{W} = \{x \in X : \text{in}_f(x) = 1\} \qquad \mathbf{U} = \{y \in Y : \text{out}_f(y) = 1\} \qquad \mathbf{F} = \{xy \in E(G) : x \in W, y \in U, f(\vec{xy}) = 1\}}}
```

Entonces $(W \cup U, F)$ es un subgrafo de $G$ que es un **matching**. Además, la cantidad de lados del matching es igual a la cantidad de $x \in X$ con $\text{in}_f(x) = 1$, lo cual es igual a $\text{out}_f(s) = v(f)$ (el valor del flujo).

**Viceversa.** Dado un matching $M$, definimos:

```{=latex}
\noindent\resizebox{\linewidth}{!}{\ensuremath{f(\vec{xy}) = 1 \text{ si } xy \in E(M) \qquad f(\vec{sx}) = 1 \text{ si } x \in V(M) \qquad f(\vec{yt}) = 1 \text{ si } y \in V(M) \qquad f(\vec{xy}) = 0 \text{ otro caso }}}
```

Esto forma un flujo válido (construido camino a camino de la forma $sxyt$) cuyo valor es la cantidad de lados del matching. Por lo tanto, **flujos maximales enteros se corresponden con matchings maximales**.

#### Algoritmo

\mbox{}\newline

1. Construir el *network* asociado.
2. Encontrar flujo maximal entero $f$ en el *network* (ej. usando Edmonds-Karp o Dinitz).
3. Tomar como matching aquellos lados $xy$ con $f(\vec{xy}) = 1$.

*Observación:* Si usáramos Dinitz, en el primer *network* auxiliar (nivel 0: $\{s\}$, nivel 1: $X$, nivel 2: $Y$, nivel 3: $\{t\}$), todo camino aumentante es de la forma $sxyt$, lo que equivale a agregar lados $xy$ al matching de forma directa sin romper la condición de matching. Luego se trata de ir extendiendo ese matching inicial.

---

### 3. Representación Matricial y Ejemplo

En muchos problemas de grafos bipartitos con partes $X$ e $Y$, hay una gran densidad de conexiones, por lo que conviene usar una matriz de adyacencia en lugar de listas de vecinos. La matriz total luce como:
$$\begin{bmatrix} 0 & A \\ A^t & 0 \end{bmatrix}$$
Por ello, se suele dar únicamente la submatriz $A$, cuyas filas representan elementos de $X$ y cuyas columnas representan elementos de $Y$. En grafos sin pesos, las entradas son $1$ o $0$. En grafos con pesos, se coloca el peso del lado $xy$ o $\infty$ si no existe el lado.

#### Ejemplo paso a paso (Grafos y Colas)

\mbox{}\newline

Sea $G$ bipartito con $X = \{A, B, C, D\}$ e $Y = \{I, II, III, IV\}$, representado por la matriz:
$$\begin{bmatrix} 1 & 0 & 1 & 1 \\ 0 & 1 & 0 & 1 \\ 1 & 1 & 0 & 0 \\ 0 & 1 & 1 & 0 \end{bmatrix}$$

1. **Matching inicial:** A simple vista construimos $\{AI, BII, DIII\}$. No podemos asignar $C$ porque sus únicos vecinos ($I$ y $II$) ya están ocupados.
2. **Búsqueda de camino aumentante (Edmonds-Karp):** Como todas las capacidades son $1$, no necesitamos guardar $\epsilon(x)$, sólo el predecesor en la cola.
    * Iniciamos la cola con $s$, que agrega a $C$ (único vértice de $X$ no matcheado, por lo que $f(\vec{sC}) = 0$).
    * $C$ tiene vecinos $I$ y $II$. Se agregan a la cola con predecesor $C$.
    * Tanto $I$ como $II$ están en el matching ($f(\vec{It}) = f(\vec{IIt}) = 1$), así que no llegan a $t$, pero devuelven flujo (*backward*) hacia sus parejas: $I$ agrega a $A$, y $II$ agrega a $B$.
    * Revisamos $A$ (vecinos $I, III, IV$): $I$ ya está en la cola; agregamos $III$ y $IV$ con predecesor $A$.
    * Revisamos $B$ (vecinos $II, IV$): ambos ya están en la cola.
    * Revisamos $III$: está matcheado con $D$, agrega a $D$ (*backward*).
    * Revisamos $IV$: **no forma parte del matching** ($f(\vec{IVt}) = 0$), por lo que puede agregar a $t$ a la cola. ¡Terminamos!

3. **Reconstrucción del camino aumentante:**
   $$s \to C \to I \leftarrow A \to IV \to t$$
   En términos de flujo, sumamos $1$ a $\vec{CI}$ y $\vec{AIV}$, y restamos $1$ a $\vec{AI}$. En el matching, esto significa **agregar $CI$ y $AIV$, y eliminar $AI$**. El nuevo matching es $\{AIV, BII, CI, DIII\}$, que cubre todos los vértices y es maximal.

\newpage

#### Estructura general de los caminos aumentantes

\mbox{}\newline

Por la estructura bipartita, todo camino aumentante es un **camino alternado** de la forma:
$$s x_{i_1} y_{i_1} x_{i_2} y_{i_2} \dots y_{i_{r-1}} x_{i_r} y_{i_r} t$$
Al actualizar el matching:

1. Se agregan los $r$ lados $x_{i_j}y_{i_j}$ ($j = 1, \dots, r$).
2. Se borran los $r-1$ lados $x_{i_j}y_{i_{j-1}}$ ($j = 2, \dots, r$).
Así, el tamaño del matching aumenta exactamente en $1$ en cada iteración.

#### Rehaciendo el ejemplo directamente sobre la matriz

\mbox{}\newline

Para hacerlo a mano sobre la matriz $A$, marcamos los elementos del matching (por ejemplo, con corchetes `[1]`) y anotamos las etiquetas de la cola al costado de las filas y debajo de las columnas:

1. **Inicio:** Marcamos con `s` las filas no matcheadas (fila $C$).
2. **Revisar filas etiquetadas:** En la fila $C$, buscamos los `1`s no marcados (columnas $I$ y $II$) y las etiquetamos con `C`.
3. **Revisar columnas etiquetadas:** Miramos si en las columnas $I$ y $II$ hay algún `[1]` del matching.
   * Si **no hay**, la columna está libre y podemos extender el matching.
   * Si **hay**, etiquetamos la fila correspondiente con el nombre de la columna. Aquí $I$ etiqueta la fila $A$ con `I`, y $II$ etiqueta la fila $B$ con `II`.
4. **Continuar:** Revisamos las nuevas filas etiquetadas ($A$ y $B$). La fila $A$ tiene `1`s en $III$ y $IV$; etiquetamos ambas columnas con `A`.
5. **Extender:** Al revisar la columna $IV$, vemos que no tiene ningún `[1]` marcado (está libre). Miramos su etiqueta (`A`), marcamos `[1]` en la posición $(A, IV)$. Como la fila $A$ ahora tiene dos `[1]`, miramos la etiqueta de la fila $A$ (`I`) y desmarcamos $(A, I)$. Esto libera la columna $I$; miramos su etiqueta (`C`) y marcamos $(C, I)$. Al llegar a una fila con etiqueta `s`, el proceso termina.

---

\newpage

### 4. Teoremas de Hall y Kőnig

#### Matchings perfectos y completos

\mbox{}\newline

* **Matching perfecto:** Cubre todos los vértices de $G$ (necesita $|X| = |Y|$).
* **Matching completo sobre $X$ (o de $X$ a $Y$):** Cubre todos los vértices de $X$ (necesita $|X| \le |Y|$).

Todo matching completo con $|X| = |Y|$ es perfecto.

#### Condición y Teorema de Hall

\mbox{}\newline

Dado $S \subseteq X$, definimos el conjunto de vecinos de $S$ como:
$$\Gamma(S) = \bigcup_{x \in S} \Gamma(x) = \{y \in Y : \exists x \in S \text{ con } xy \in E\}$$

**Condición de Hall:** Para que exista un matching completo sobre $X$, es necesario que $|S| \le |\Gamma(S)|$ para todo $S \subseteq X$.

**Teorema (Hall):** Sea $G$ grafo bipartito con partes $X$ e $Y$  
  $\forall S \subseteq X, |S| \le |\Gamma(S)| \implies \exists \text{matching completo sobre } X$.

*Idea de la demostración (por el absurdo usando Edmonds-Karp):*  
Si no existe matching completo sobre $X$, al correr el algoritmo obtenemos un matching maximal que deja un subconjunto no vacío $S_0 \subseteq X$ sin cubrir. En el último paso de Edmonds-Karp se halla un corte minimal $C = \{s\} \cup S \cup T$ (con $S \subseteq X, T \subseteq Y$). Se demuestra que:

1. $S_0 \subseteq S$ (son los vértices agregados directamente por $s$).
2. Cada vértice de $S - S_0$ fue agregado *backward* por un único vértice de $T$, y todo vértice de $T$ agrega exactamente a un vértice de $S - S_0$, por lo que $|S - S_0| = |T|$.
3. Todos los vecinos de $S$ están en $T$, es decir, $T = \Gamma(S)$.
4. Luego, $|\Gamma(S)| = |T| = |S - S_0| = |S| - |S_0| < |S|$ (pues $S_0 \neq \emptyset$), contradiciendo la hipótesis.

#### Teorema del Matrimonio de Kőnig

\mbox{}\newline

**Teorema:** Todo grafo bipartito regular (con grado $\Delta > 0$) tiene un matching perfecto.

*Contexto:* Si tenemos conjuntos de hombres y mujeres donde cada persona es compatible mutuamente con exactamente $k$ personas del otro grupo ($k > 0$), es posible casar a todos de forma compatible.

*Demostración:* Como $G$ es $\Delta$-regular y bipartito, la cantidad total de lados es $|E| = \Delta |X|$ y también $|E| = \Delta |Y|$. Como $\Delta \neq 0$, resulta $|X| = |Y|$. Para cualquier $S \subseteq X$, la cantidad de lados incidentes a $S$ es $|E_S| = \Delta |S|$. Todos estos lados inciden en $\Gamma(S) \subseteq Y$, por lo que $E_S \subseteq E_{\Gamma(S)}$, lo que implica $\Delta |S| \le \Delta |\Gamma(S)| \implies |S| \le |\Gamma(S)|$. Por el Teorema de Hall, existe un matching completo sobre $X$, que al ser $|X|=|Y|$, es perfecto.

*(Nota histórica: Dénes Kőnig publicó el primer libro de texto de teoría de grafos y probó también que en grafos bipartitos el tamaño del matching maximal es igual al tamaño de la cobertura minimal de vértices).*

\newpage

#### Teorema de Kőnig sobre coloreo de lados

\mbox{}\newline

Un **coloreo propio de lados** asigna colores a las aristas tal que dos aristas con un extremo en común tengan colores distintos. El **índice cromático** $\chi'(G)$ es la menor cantidad de colores necesarios. Por el Teorema de Vizing (1964), para todo grafo se cumple $\Delta \le \chi'(G) \le \Delta + 1$ (determinar cuál de los dos valores toma es NP-completo en general).

**Teorema (Kőnig):** Si $G$ es bipartito, entonces $\chi'(G) = \Delta$.

*Demostración:*

1. **Caso regular:** Por inducción en $\Delta$. Si $\Delta = 1$, son lados disjuntos y basta $1$ color. Si $\Delta > 1$, por el Teorema del Matrimonio existe un matching perfecto $E_1$. Al borrar $E_1$ de $G$, obtenemos un grafo bipartito $(\Delta-1)$-regular $G^*$. Por hipótesis inductiva, $G^*$ se colorea con $\Delta - 1$ colores, y asignando un color nuevo a $E_1$ completamos el coloreo con $\Delta$ colores.
2. **Caso general:** Todo grafo bipartito $G$ puede sumergirse como subgrafo en un grafo bipartito $\Delta$-regular $H$ con el mismo grado máximo $\Delta(H) = \Delta(G)$ (duplicando las partes $X^*, Y^*$ y uniendo cada vértice $z$ de grado menor a $\Delta$ con su copia $z^*$ iterativamente hasta subir el grado mínimo $\delta$ hasta $\Delta$). Entonces $\chi'(G) \le \chi'(H) = \Delta(H) = \Delta(G)$.

---

\newpage

---

## Matchings Pesados — Minimizar el Máximo

### 1. Planteamiento del problema

En aplicaciones reales, asignar una tarea a un trabajador tiene un costo (tiempo, dinero, transporte). Supondremos un grafo bipartito con partes de igual tamaño $n$ (nota: aquí $n = |X| = |Y|$) y pesos en los lados, donde existe al menos un matching perfecto.

Existen dos criterios principales para minimizar costos:

1. **Minimizar la suma de los costos** (ej. tareas secuenciales, o costos pagados por una empresa).
2. **Minimizar el mayor costo** (ej. tareas ejecutadas en paralelo donde importa el tiempo total de finalización, o costos pagados individualmente por cada trabajador).

Evaluar todas las $n!$ biyecciones posibles tiene complejidad $O(n!)$, por lo que necesitamos algoritmos polinomiales.

---

### 2. Algoritmo de Minimizar el Máximo (Algoritmo de Gross)

Queremos hallar un matching perfecto que minimice el peso máximo entre sus aristas.

#### Idea del algoritmo y Búsqueda Binaria

\mbox{}\newline

Si fijamos un umbral $\mu$ y eliminamos del grafo todos los lados con peso estrictamente mayor que $\mu$, podemos correr el algoritmo de matching maximal en el grafo resultante:

* Si existe un **matching perfecto**, sabemos que el óptimo es $\le \mu$ (y de hecho $\le$ al mayor peso usado en ese matching). Bajamos el umbral.
* Si **no existe** un matching perfecto, el umbral es muy restrictivo. Subimos el umbral.

Para evitar probar todos los pesos uno por uno ($O(n^2)$ búsquedas), ordenamos los pesos distintos de la matriz y aplicamos **búsqueda binaria** sobre ellos hasta encontrar un valor $\mu$ para el cual existe matching perfecto, pero no existe para el peso inmediatamente anterior.

*Optimización práctica:* Cuando encontramos un matching perfecto bajo un umbral $\mu$, si el peso máximo efectivo de ese matching es $\mu' < \mu$, podemos actualizar nuestra cota superior directamente a $\mu'$ antes de calcular el siguiente punto medio.

---

\newpage

---

### 3. Análisis de Complejidades

#### Complejidad de encontrar matching maximal en grafos bipartitos

\mbox{}\newline

* **Edmonds-Karp:** En redes generales es $O(V E^2)$. Aquí, buscar un camino aumentante cuesta $O(m)$ (donde $m$ es la cantidad de lados). Como todas las capacidades son $1$, cada camino aumentante incrementa el flujo en $1$ unidad, y el flujo máximo está acotado por $n$. Por lo tanto, hay a lo sumo $n$ iteraciones, dando una complejidad de **$O(mn)$**.
* **Dinic (Dinitz):** En redes con capacidades unitarias es $O(m n^{2/3})$. Sin embargo, en redes bipartitas todo vértice distinto de $s, t$ tiene grado de entrada $1$ o grado de salida $1$. Bajo esta propiedad adicional, Dinic corre en tiempo **$O(m\sqrt{n})$**.

#### Complejidad de Minimizar el Máximo

\mbox{}\newline

En el peor caso, el grafo es completo ($m = n^2$) y todos los pesos son distintos:

1. **Ordenar los pesos:** Usando un algoritmo de ordenamiento óptimo toma $O(n^2 \lg(n^2)) = O(n^2 \lg n)$.
2. **Búsqueda binaria:** Realiza $O(\lg(n^2)) = O(\lg n)$ llamadas al algoritmo de matching maximal.
   * Con Dinic: $O(n^{5/2} \lg n)$.
   * Con Edmonds-Karp: $O(n^3 \lg n)$.

El costo de ordenamiento queda absorbido, resultando en una complejidad total de **$O(n^{5/2} \lg n)$** usando Dinic.

---

### 4. Ejemplo de Minimizar el Máximo

Dada una matriz $8 \times 8$ de tiempos con valores distintos $\{1, 4, 5, 6, 7, 8, 9, 10, 15\}$:

1. **Umbral $\mu = 7$** (mediana de los 9 valores): Convertimos en `1` las entradas $\le 7$ y en `0` las $> 7$. Corriendo el algoritmo matricial, tras dos extensiones de caminos aumentantes, encontramos un matching perfecto cuyo peso máximo es $7$ ($Ai:5, Bvii:1, Cvi:7, Dviii:4, Eiii:4, Fv:6, Giv:4, Hii:1$).
2. **Umbral $\mu = 5$** (mediana de $\{1, 4, 5, 6, 7\}$): Filtramos entradas $\le 5$. Encontramos un matching inicial y, tras una extensión a partir de la fila libre $F$ (camino $F \to ii \to D \to iii \to E \to vi$), obtenemos un matching perfecto con peso máximo $5$ ($Ai:5, Bvii:1, Cviii:1, Diii:1, Evi:5, Fii:5, Giv:4, Hv:4$).
3. **Umbral $\mu = 4$** (valores restantes $\{1, 4\}$): Al filtrar por $\le 4$, la fila $F$ queda compuesta únicamente por ceros, lo que garantiza que no existe ningún matching perfecto (en general, si no hay filas/columnas de ceros, el algoritmo igualmente arrojará un conjunto $S$ que viola la condición de Hall).

**Conclusión:** El valor óptimo mínimo para el tiempo máximo es **$5$**.

---

\newpage

---

## Matchings Pesados — Algoritmo Húngaro

### 1. Minimizar la Suma de Costos

Dada una matriz de costos $C$ de tamaño $n \times n$, buscamos una permutación $\sigma: \{1, \dots, n\} \to \{1, \dots, n\}$ que minimice:
$$\sum_{i=1}^{n} C_{i, \sigma(i)}$$

El **Algoritmo Húngaro** (creado por Harold Kuhn en 1955 en honor a los matemáticos húngaros Dénes Kőnig y Jenő Egerváry) resuelve este problema en tiempo polinomial.

#### Propiedades Fundamentales

\mbox{}\newline

1. **Propiedad Obvia:** Si $C_{i,j} \ge 0$ para todo $i, j$, y existe un matching perfecto $\sigma$ tal que $C_{i, \sigma(i)} = 0$ para todo $i$, entonces $\sigma$ minimiza la suma de los costos (ya que ninguna asignación puede sumar menos que $0$).
2. **Propiedad Casi Obvia:** Si obtenemos una matriz $\tilde{C}$ restándole una constante $r$ (positiva o negativa) a toda una fila o a toda una columna de $C$, entonces una permutación $\sigma$ minimiza la suma respecto de $C$ **si y sólo si** minimiza la suma respecto de $\tilde{C}$. Esto se debe a que el costo de *todas* las asignaciones posibles se ve desplazado exactamente en la misma constante $-r$.

---

### 2. Desarrollo del Algoritmo Húngaro

La estrategia consiste en transformar la matriz mediante sumas y restas a filas y columnas (manteniendo siempre todas las entradas $\ge 0$) hasta que aparezca un matching perfecto compuesto exclusivamente por ceros.

#### Fase Inicial

\mbox{}\newline

1. Restar el mínimo de cada fila a todos los elementos de esa fila.
2. Restar el mínimo de cada columna a todos los elementos de esa columna.
3. Buscar un matching maximal de ceros en la matriz resultante. Si es perfecto, terminamos.

#### ¿Qué hacer si no hay matching perfecto de ceros?

\mbox{}\newline

Si el matching maximal de ceros no es perfecto, el algoritmo de búsqueda matricial se detendrá dejando algunas filas etiquetadas. Por la demostración del Teorema de Hall, el conjunto $S$ formado por **todas las filas etiquetadas** cumple que $|S| > |\Gamma(S)|$, donde $\Gamma(S)$ son **todas las columnas etiquetadas**.

Esto nos indica que las filas de $S$ están "compitiendo" por muy pocos ceros en $\Gamma(S)$, por lo que debemos generar nuevos ceros en la intersección de las filas de $S$ con las columnas **no etiquetadas** (el complemento de $\Gamma(S)$):

1. Calculamos $m = \min \{ C_{i,j} : i \in S, j \notin \Gamma(S) \}$.
2. Restamos $m$ a todas las filas de $S$. Esto crea al menos un nuevo cero en la zona $S \times \overline{\Gamma(S)}$ sin generar negativos allí, pero puede generar valores negativos en la intersección $S \times \Gamma(S)$.
3. Para eliminar esos negativos, **sumamos $m$ a todas las columnas de $\Gamma(S)$**.

*Observación clave:* Al sumar $m$ a las columnas de $\Gamma(S)$, pueden desaparecer algunos ceros que estaban en filas fuera de $S$ y columnas dentro de $\Gamma(S)$. Sin embargo, **ninguno de esos ceros perdidos pertenecía al matching actual**, por lo que no debemos empezar de cero: conservamos el matching y las etiquetas actuales, revisamos las filas de $S$ para etiquetar las columnas de los nuevos ceros creados, y continuamos buscando caminos aumentantes.

---

### 3. Resumen del Algoritmo Húngaro

1. Restar el mínimo de cada fila.
2. Restar el mínimo de cada columna.
3. Buscar un matching perfecto de ceros (usando el algoritmo de etiquetado matricial).
4. Si el matching es perfecto, **fin** (evaluar la suma en la matriz original).
5. Si no es perfecto, identificar $S$ (filas etiquetadas) y $\Gamma(S)$ (columnas etiquetadas):
   * Calcular $m =$ mínimo de las entradas en las filas de $S$ y columnas fuera de $\Gamma(S)$.
   * Restar $m$ a las filas de $S$.
   * Sumar $m$ a las columnas de $\Gamma(S)$.
   * Volver al paso 3 conservando el matching actual.

---

### 4. Ejemplo Completo del Algoritmo Húngaro

Consideremos la siguiente matriz de costos $4 \times 4$ (donde $\infty$ indica que no existe arista):
$$\begin{bmatrix}
\infty & 5 & 9 & 7 \\
9 & 5 & 5 & 8 \\
7 & 5 & 9 & 5 \\
\infty & 5 & 4 & 7
\end{bmatrix}$$

1. **Restar mínimos de fila** (mínimos: $5, 5, 5, 4$):
   $$\begin{bmatrix}
   \infty & 0 & 4 & 2 \\
   4 & 0 & 0 & 3 \\
   2 & 0 & 4 & 0 \\
   \infty & 1 & 0 & 3
   \end{bmatrix}$$

2. **Restar mínimos de columna** (mínimos: $2, 0, 0, 0$):
   $$\begin{bmatrix}
   \infty & 0 & 4 & 2 \\
   2 & 0 & 0 & 3 \\
   0 & 0 & 4 & 0 \\
   \infty & 1 & 0 & 3
   \end{bmatrix}$$

3. **Búsqueda de matching de ceros:**
   * Matching inicial: $(A, ii), (B, iii), (C, i)$. La fila $D$ queda sin matchear (etiqueta `s`).
   * Etiquetado: $D$ etiqueta a la columna $iii$ (`D`); $iii$ está matcheada con $B$, así que etiqueta a la fila $B$ (`iii`); $B$ tiene un cero en $ii$, así que etiqueta a la columna $ii$ (`B`); $ii$ está matcheada con $A$, así que etiqueta a la fila $A$ (`ii`).
   * No hay más ceros disponibles desde las filas etiquetadas $S = \{A, B, D\}$, con columnas etiquetadas $\Gamma(S) = \{ii, iii\}$.

4. **Modificación de la matriz:**
   * Las filas en $S$ ($A, B, D$) intersectadas con las columnas fuera de $\Gamma(S)$ ($i, iv$) contienen los valores $\{\infty, 2, 2, 3, \infty, 3\}$. El mínimo es $m = 2$.
   * Restamos $2$ a las filas $A, B, D$ y sumamos $2$ a las columnas $ii, iii$:
   $$\begin{bmatrix}
   \infty & [0] & 4 & 0 \\
   0 & 0 & [0] & 1 \\
   [0] & 2 & 6 & 0 \\
   \infty & 1 & 0 & 1
   \end{bmatrix}$$
   *(Nótese que los ceros marcados con `[0]` del matching siguen intactos).*

5. **Extensión del matching:**
   * Revisando las filas de $S$, vemos que la fila $A$ ahora tiene un nuevo cero en la columna $iv$. Etiquetamos la columna $iv$ con `A`.
   * La columna $iv$ está libre. Reconstruimos el camino aumentante hacia atrás:
     * Marcamos $(A, iv)$, desmarcamos $(A, ii)$.
     * Marcamos $(B, ii)$, desmarcamos $(B, iii)$.
     * Marcamos $(D, iii)$ (llegando a la fila $D$ marcada con `s`).

6. **Resultado final:**
   El matching perfecto obtenido es **$Aiv, Bii, Ci, Diii$**. Sumando sus costos en la matriz original obtenemos:
   $$7 + 5 + 7 + 4 = 23$$
   Este es el costo mínimo posible.

---

\newpage

---

### Análisis de Complejidad del Algoritmo Húngaro

El **Algoritmo Húngaro** (o método de *minimizar la suma*) resuelve el problema de asignación de costo mínimo en un grafo bipartito completo $K_{n,n}$ con matriz de costos $C \in \mathbb{R}_{\ge 0}^{n \times n}$.

Dependiendo de cómo se gestionen las búsquedas de caminos aumentantes y las actualizaciones de la matriz de costos, el algoritmo presenta tres cotas de complejidad temporal:

1. **$O(n^5)$**:
    - Implementación ingenua (*naif*)
    - descartando el matching parcial y recalculando desde cero tras cada cambio de matriz.
2. **$O(n^4)$**:
    - Implementación estándar (de lápiz y papel)
    - preservando el matching parcial y el árbol alternante tras actualizar la matriz.
3. **$O(n^3)$**:
    - Implementación optimizada en computadora mediante potenciales duales y mantenimiento dinámico de mínimos por columna.

---

#### 1. Definiciones Preliminares y Paso Inicial

\mbox{}\newline

Dado un matching parcial $M$ compuesto exclusivamente por aristas de costo cero ($C_{x,y} = 0$):

- **$S \subseteq X$**: Conjunto de filas (vértices izquierdos) etiquetadas. En las pruebas de las implementaciones estándar y optimizada, al comienzo de cada fase elegimos una única fila libre $x_0$ como raíz y empezamos con $S = \{x_0\}$. El matching y los potenciales persisten entre fases; las etiquetas de búsqueda se reinician.
- **$\Gamma(S) \subseteq Y$**: Conjunto de columnas (vértices derechos) alcanzables desde $S$ mediante aristas de costo cero; es decir, columnas etiquetadas durante la búsqueda.
- **$\overline{S} = X \setminus S$** y **$\overline{\Gamma(S)} = Y \setminus \Gamma(S)$**: Sus respectivos complementos.

\newpage

##### Paso 0: Reducción Inicial y Matching de Ceros

\mbox{}\newline

1. Restar el mínimo de cada fila a todos los elementos de esa fila, y luego el mínimo de cada columna a todos los elementos de esa columna. Esto toma $O(n^2)$.
    - Para cada una de las $n$ filas/columnas, se recorren las $n$ columnas/filas para hallar el mínimo ($2 \cdot n \cdot n = 2n^2$ comparaciones) 
    - Se vuelven a recorrer para restar ($2n^2$ restas). Total: $4n^2 = O(n^2)$.
2. Construir un matching inicial greedy $M_0$ tomando ceros independientes. Esto también toma $O(n^2)$.
    - Se recorren las $n \times n$ celdas de la matriz en $O(n^2)$
    - Si hay un 0 en (x, y) y $x$ e $y$ no están usadas, se agrega $xy$ a $M_0$ y se marcan $x$ e $y$ en $O(1)$.
    - En total: $O(n^2)$.

Entonces, el costo del paso 0 es: $O(n^2) + O(n^2) = O(n^2)$

##### Mecánica de Búsqueda y Cambio de Matriz

\mbox{}\newline

Antes de acotar el costo temporal, definimos cómo opera una **fase de extensión** partiendo de un matching parcial $M$ y una fila libre fija $x_0$:

1. **Búsqueda en el subgrafo de ceros:**
    - Inicializamos $S = \{x_0\}$ y $\Gamma(S) = \emptyset$.
    - Mientras existan filas en $S$ sin escanear:
      - Tomamos una fila $x \in S$ y revisamos sus columnas buscando ceros ($C_{x,y} = 0$) con $y \notin \Gamma(S)$.
      - Cada vez que alcanzamos una columna $y \notin \Gamma(S)$, la etiquetamos y la agregamos a $\Gamma(S)$:
        - Si $y$ está **libre**, hallamos un camino aumentante desde $x_0$ hasta $y$, invertimos sus aristas para incrementar $|M|$ en $1$ y cerramos la fase.
        - Si $y$ está **emparejada** en $M$ con una fila $z$, etiquetamos $z$ y la agregamos a $S$ para explorarla.

2. **Cambio de matriz (cuando la búsqueda se bloquea):** Si todas las filas de $S$ fueron escaneadas sin hallar una columna libre en $\Gamma(S)$, entonces:
    - Cada columna de $\Gamma(S)$ está emparejada con una fila distinta de $S \setminus \{x_0\}$, y recíprocamente toda fila de $S \setminus \{x_0\}$ ingresó desde su columna emparejada en $\Gamma(S)$. Por tanto, $|S| = |\Gamma(S)| + 1$, de donde $|S| > |\Gamma(S)|$ y $|\Gamma(S)| \le n - 1$, garantizando que $\overline{\Gamma(S)} \ne \emptyset$.
    - Como ya se examinaron todos los ceros desde $S$ y no se alcanzó ninguna columna fuera de $\Gamma(S)$, no existen ceros en la submatriz $S \times \overline{\Gamma(S)}$. Dado que los costos son finitos y no negativos:
      $$0 < m = \min \{ C_{x,y} : x \in S,\ y \in \overline{\Gamma(S)} \} < +\infty$$
    - Actualizamos la matriz de costos $C$ a $C'$ realizando:
      - **Restar $m$** a todas las filas $x \in S$.
      - **Sumar $m$** a todas las columnas $y \in \Gamma(S)$.

  *(Nota: Esta operación equivale a restar $m$ en $S \times \overline{\Gamma(S)}$, sumar $m$ en $\overline{S} \times \Gamma(S)$, y dejar intactos los cuadrantes $S \times \Gamma(S)$ y $\overline{S} \times \overline{\Gamma(S)}$).*

---

##### Lema de Invarianza y Progreso (Acotación de cambios de matriz)

> **Lema 1.** *Tras un cambio de matriz, todas las entradas de $C'$ siguen siendo no negativas, las aristas del matching $M$ y del árbol alternante actual conservan costo cero, y al continuar la búsqueda sin reiniciar, o bien se extiende el matching $M$, o bien el conjunto $S$ crece estrictamente ($|S_{\text{nuevo}}| > |S|$).*

**Prueba del Lema 1:**

- **No negatividad:** En $S \times \overline{\Gamma(S)}$, toda entrada es al menos $m$, por lo que tras restar $m$ resulta $C'_{x,y} \ge 0$. En $\overline{S} \times \Gamma(S)$ las entradas aumentan en $m$; en $S \times \Gamma(S)$ se resta $m$ por fila y se suma $m$ por columna (quedando invariantes); y en $\overline{S} \times \overline{\Gamma(S)}$ no se modifican. Así, $C'_{x,y} \ge 0$ para todo $(x,y)$.
- **Preservación de ceros actuales:** Los únicos elementos cuyo valor aumenta están en $\overline{S} \times \Gamma(S)$. Sin embargo, toda columna $y \in \Gamma(S)$ ya fue revisada; si estuviera emparejada en $M$ con alguna fila $x$, dicha fila $x$ habría sido agregada a $S$. Por lo tanto, no existe ninguna arista de $M$ ni arista explorada del árbol alternante en $\overline{S} \times \Gamma(S)$. Ningún cero relevante se pierde.
- **Progreso estricto:** Por definición de $m$, aparece al menos un nuevo cero en algún par $(x, y) \in S \times \overline{\Gamma(S)}$. Como $x \in S$ y $y \notin \Gamma(S)$, la columna $y$ recibe una etiqueta desde $x$ y entra en $\Gamma(S)$. Al revisar $y$:
  - Si $y$ está libre, se completa un camino aumentante y $|M|$ aumenta en $1$.
  - Si $y$ está emparejada con una fila $z$, entonces $z \notin S$ (ya que toda fila no raíz de $S$ entró por su columna emparejada, que ya pertenece a $\Gamma(S)$, y la raíz $x_0$ es libre). Se etiqueta $z$ y se agrega a $S$, logrando $|S_{\text{nuevo}}| \ge |S| + 1$. $\blacksquare$

Como $1 \le |S| \le n$, el Lema 1 garantiza que en cada fase ocurren a lo sumo $n$ cambios de matriz antes de que la búsqueda encuentre una columna libre y extienda $|M|$ en $1$. Dado que $|M| \le n$, necesitaremos a lo sumo **$O(n)$ fases de extensión** para alcanzar un matching perfecto ($|M| = n$).

$$
T_{\text{total}} = \underbrace{O(n^2)}_{\text{paso 0}} + \underbrace{O(n)}_{\text{fases de extensión}} \cdot \underbrace{T_{\text{ext}}}_{\text{incrementar }\vert{}M\vert{}\text{ en }1}
$$

---

\newpage

---

#### 2. Teorema 1: Complejidad $O(n^4)$ (Implementación Estándar)

> **Teorema 1.** *La complejidad temporal del Algoritmo Húngaro, manteniendo el matching parcial y el etiquetado actual tras cada actualización de la matriz, es $O(n^4)$.*

##### Demostración

\mbox{}\newline

Para extender $M$, ejecutamos una búsqueda de camino aumentante (tipo BFS/Edmonds-Karp) sobre el subgrafo de ceros:

1. **Costo de exploración del grafo de ceros:**
    - Cada vez que una fila $x$ entra en $S$, recorremos sus $n$ columnas buscando ceros ($O(n)$). Como una fila entra en $S$ a lo sumo una vez por fase de extensión, el escaneo total de filas es $O(n^2)$.
    - Cada vez que una columna $y$ entra en $\Gamma(S)$, verificamos en $O(1)$ si está libre (en cuyo caso encontramos un camino aumentante) o si está matcheada con alguna fila $z$ (en cuyo caso etiquetamos $z$ y la agregamos a $S$).
2. **Costo de cada cambio de matriz ($\omega$):**
    - Calcular $m = \min \{ C_{x,y} : x \in S,\ y \in \overline{\Gamma(S)} \}$ recorre a lo sumo $n^2$ entradas ($O(n^2)$).
    - Restar $m$ a las filas de $S$ y sumarlo a las columnas de $\Gamma(S)$ modifica a lo sumo $n$ filas y $n$ columnas de longitud $n$ ($O(n^2)$). En ese mismo recorrido sobre $S \times \overline{\Gamma(S)}$ se identifican los ceros nuevos y se etiquetan las columnas recién alcanzadas sin necesidad de reescanear las filas ya procesadas de $S$.
    - Por tanto, cada cambio de matriz toma tiempo $\omega = O(n^2)$.

##### Cálculo Final para $O(n^4)$

\mbox{}\newline

Dado que $1 \le \vert{}S\vert{} \le n$, por el Lema 1 pueden ocurrir a lo sumo $\phi \le n = O(n)$ cambios de matriz antes de que el matching se extienda. Así, el costo de una fase de extensión es:

$$T_{\text{ext}} = \underbrace{O(n^2)}_{\text{exploración BFS}} + \underbrace{O(n)}_{\phi\text{ cambios}} \cdot \underbrace{O(n^2)}_{\omega\text{ actualización}} = O(n^3)$$

Sustituyendo en la complejidad total:

$$T_{\text{total}} = O(n^2) + O(n) \cdot O(n^3) = O(n^4)$$

---

\newpage

---

#### 3. Teorema 2: Complejidad $O(n^3)$ (Implementación Optimizada)

> **Teorema 2.** *Mediante el uso de potenciales duales (variables de ajuste por fila y columna) y un arreglo auxiliar de mínimos por columna, el Algoritmo Húngaro puede implementarse en tiempo $O(n^3)$.*

##### Demostración

\mbox{}\newline

Para reducir la complejidad de $O(n^4)$ a $O(n^3)$, debemos reducir el costo de cada cambio de matriz de $\omega = O(n^2)$ a **$O(n)$** dentro de una misma fase de extensión. Esto se logra mediante dos optimizaciones estructurales, asumiendo que las operaciones aritméticas y comparaciones toman $O(1)$.

**Persistencia entre fases:**

- **Estructuras globales:** Los potenciales $RF$ y $SC$ se inicializan en cero una sola vez (tras las reducciones iniciales de filas y columnas) y se conservan hasta el final del algoritmo.
- **Estructuras por fase:** Al comenzar cada nueva fase se reinician *la raíz*, $S$, $\Gamma(S)$, *la cola*, *pred* y $mS$.
- **Invariante del matching:** Al aumentar el matching, las aristas nuevas provienen del camino aumentante (costo reducido cero) y las descartadas salen del conjunto. Así, el matching actualizado siempre está compuesto por ceros efectivos de $\tilde{C}$.

**Truco 1: Actualización implícita mediante potenciales ($O(n)$)**

En lugar de modificar las $n^2$ entradas de $C$, mantenemos dos arreglos de dimensión $n$:

- $RF[x]$: valor acumulado restado a la fila $x$ (inicializado en $0$).
- $SC[y]$: valor acumulado sumado a la columna $y$ (inicializado en $0$).

El costo efectivo (reducido) de cualquier entrada $(x,y)$ se calcula en tiempo $O(1)$ al vuelo como:

$$\tilde{C}_{x,y} = C_{x,y} - RF[x] + SC[y]$$

Cuando ocurre un cambio de matriz con mínimo $m$:

- Para cada $x \in S$: $RF[x] \leftarrow RF[x] + m$
- Para cada $y \in \Gamma(S)$: $SC[y] \leftarrow SC[y] + m$

> *(Como la fórmula es $C_{x,y} - RF[x] + SC[y]$, sumar $m$ a la columna equivale a $SC[y] + m$).*

Actualizar ambos arreglos toma **$O(n)$** en lugar de $O(n^2)$.

\newpage

**Truco 2: Cálculo de $m$ en $O(n)$ mediante mínimos incrementales**

Observemos que:

$$m = \min_{y \in \overline{\Gamma(S)}} \left( \min_{x \in S} \tilde{C}_{x,y} \right)$$

Definimos dos arreglos auxiliares de tamaño $n$:

1. **$mS[y]$ (slack o holgura)**: Almacena, para cada columna $y \notin \Gamma(S)$, el menor costo efectivo observado desde el subconjunto $S_{\mathrm{esc}} \subseteq S$ de filas ya procesadas por el BFS:
  $$mS[y] = \min_{x \in S_{\mathrm{esc}}} \{ C_{x,y} - RF[x] + SC[y] \}$$
2. **$pred[y]$**: Registra qué fila $x \in S_{\mathrm{esc}}$ alcanza dicho mínimo, permitiendo reconstruir el camino aumentante en $O(1)$ por arista.  
> **Invariante de holgura:** En todo momento de la fase y para cada columna $y \notin \Gamma(S)$, se mantiene la igualdad de $mS[y]$ y `pred[y]` apunta a la fila de $S_{\mathrm{esc}}$ que logra ese mínimo. Si $S_{\mathrm{esc}} = \emptyset$, entonces $mS[y] = +\infty$. Solo cuando la cola queda vacía se cumple $S_{\mathrm{esc}} = S$, momento en el cual $m = \min_{y \notin \Gamma(S)} mS[y]$.

###### **Ciclo de vida de $mS[y]$ durante una fase de extensión**

1. **Inicialización — $O(n)$:**
   Al iniciar cada extensión, se asigna $mS[y] = +\infty$ y `pred[y] = -1` para todo $y \in \{1, \dots, n\}$.

2. **Al agregar una fila $x$ a $S$ — $O(n)$ por fila:**
   Se escanea la fila $x$ recorriendo únicamente las columnas no etiquetadas $y \notin \Gamma(S)$ *(esta guarda evita sobrescribir predecesores y previene ciclos en la reconstrucción)*:

   * Se calcula `temp = C[x][y] - RF[x] + SC[y]`.
   * **Si `temp == 0`:** Se etiqueta $y$ con predecesor $x$, se agrega a $\Gamma(S)$ y se verifica su estado:
     * Si está *libre*, se completa el camino aumentante.
     * Si está *emparejada* con una fila $z$, se agrega $z$ a $S$ y se encola en el BFS.
   * **Si `temp > 0`:** Se actualiza la holgura de la columna en $O(1)$:

     ```text
     if temp < mS[y]:
         mS[y] = temp
         pred[y] = x
     ```
   * *Costo acumulado:* Como cada fila entra en $S$ a lo sumo una vez por extensión, estas actualizaciones toman **$O(n^2)$** en toda la fase.

\newpage

3. **Al necesitar un cambio de matriz — $O(n)$:**

   * **Hallar $m$:** Se busca el mínimo entre las columnas no etiquetadas:
     $$m = \min \{ mS[y] : y \notin \Gamma(S) \}$$
     Como la cola está vacía, todos los ceros alcanzables ya fueron procesados y las holguras pendientes son estrictamente positivas. Además, existen columnas pendientes porque $|S| > |\Gamma(S)|$ y la matriz es completa, por lo que $0 < m < +\infty$. Este paso toma **$O(n)$**.
   * **Actualizar potenciales:**
     * $RF[x] \leftarrow RF[x] + m$ para todo $x \in S$ ($O(n)$).
     * $SC[y] \leftarrow SC[y] + m$ para todo $y \in \Gamma(S)$ ($O(n)$).
   * **Actualizar holguras:** Dado que $RF[x]$ aumentó en $m$ para todo $x \in S$, el mínimo efectivo para toda columna $y \notin \Gamma(S)$ disminuye exactamente en $m$. Actualizamos $mS$ en **$O(n)$** sin consultar la matriz:
     $$\forall y \notin \Gamma(S): \quad mS[y] \leftarrow mS[y] - m$$
   * **Procesar nuevos ceros:** Toda columna pendiente cuyo $mS[y]$ pase a valer $0$ tiene ahora un cero efectivo con `pred[y]`. Se etiqueta con dicho predecesor, entra en $\Gamma(S)$ y, si está emparejada con una fila $z \notin S$, se incorpora $z$ a $S$ y se encola para continuar el BFS antes de calcular otro mínimo global.

###### **Conservación del invariante y cota de cambios**

* **Por qué se conserva el invariante:** Al restar $m$ a todas las holguras pendientes, cada candidata calculada desde $S_{\mathrm{esc}}$ disminuye en la misma magnitud; por ello, el mínimo baja en $m$ y `pred[y]` no cambia. Al procesar una fila nueva $z$, $\tilde{C}_{z,y}$ se evalúa con los potenciales vigentes y se compara con el mínimo ya actualizado. Por inducción sobre filas procesadas y cambios de matriz, el invariante se sostiene hasta hallar una columna libre o vaciar la cola.
* **Cota de cambios por fase:** En cada cambio de matriz al menos una columna pendiente llega a holgura cero. Si está libre, aumenta el matching; si está emparejada con una fila $z$, necesariamente $z \notin S$ (de haber sido escaneada antes, su arista de matching ya habría etiquetado a esa columna). Como en cada cambio se incorpora al menos una fila nueva a $S$, hay a lo sumo **$n$ cambios por fase**.

###### **Cálculo Final de Complejidad**

En cada una de las $O(n)$ fases de extensión se realizan las siguientes operaciones:

| Operación dentro de la fase | Detalle | Costo |
| --- | --- | --- |
| **Exploración de filas** | Escanear las filas agregadas a $S$ y actualizar $mS$ | $O(n^2)$ |
| **Cambios de matriz** | Hasta $O(n)$ cambios, cada uno con costo $O(n)$ (hallar $m$, actualizar $RF$, $SC$ y $mS$) | $O(n) \cdot O(n) = O(n^2)$ |
| **Total por fase ($T_{\text{ext}}$)** | Suma de exploración y actualizaciones | **$O(n^2)$** |

\newpage

Multiplicando el costo de una fase por las $O(n)$ extensiones necesarias para alcanzar un matching perfecto (incluyendo las reducciones iniciales de $O(n^2)$):

$$T_{\text{total}} = O(n^2) + O(n) \cdot O(n^2) = O(n^3) \quad \blacksquare$$

###### **Alcance del modelo aritmético**

* **Costos reales (modelo RAM real):** Se asume que la suma, resta y comparación exactas toman $O(1)$.
* **Costos enteros exactos:** El número de operaciones es $O(n^3)$ e independiente de la magnitud. Si $U$ es el costo máximo tras la reducción inicial, el costo óptimo es a lo sumo $nU$. En cada cambio de matriz el objetivo dual $\sum_x RF[x] - \sum_y SC[y]$ aumenta en $m$ (ya que $|S| - |\Gamma(S)| = 1$) sin superar el costo óptimo. Así, cada potencial queda acotado por $nU$ y su longitud binaria es $O(\log n + \log U)$.
* **Costos racionales exactos:** Multiplicando la matriz por el producto de sus denominadores se aplica el mismo argumento, logrando una cota fuertemente polinomial.
* **Punto flotante:** Las comparaciones exactas (`temp == 0` y $mS[y] = 0$) no son confiables debido a errores de redondeo, los cuales pueden ocultar ceros o generar falsos ceros; por ende, se requiere una política de tolerancia numérica ($\epsilon$) para garantizar terminación y corrección.

---

> *Nota sobre la variante $O(n^5)$*
> *Si después de cada actualización de la matriz se descartan tanto las etiquetas como el matching $M$ y se inician las búsquedas desde todas las filas libres, el algoritmo debe recalcular desde cero un matching máximo en el grafo de ceros:*
>
> - *Costo por recálculo — $O(n^3)$: Se requieren a lo sumo $n$ aumentos y cada búsqueda de camino aumentante toma $O(n^2)$ mediante BFS o DFS.*
> - *Cantidad de recálculos — $O(n^2)$: Por la descomposición de Dulmage–Mendelsohn, mientras el tamaño del matching máximo no cambie, el conjunto de filas alcanzables es independiente del matching elegido. Cada nuevo cero habilita una columna antes inalcanzable: si está emparejada, añade su fila asociada al conjunto de alcanzables; si está libre, el matching crece y arranca una nueva fase. Esto acota el proceso a $O(n)$ actualizaciones por fase y $O(n)$ fases en total.*
> - *Complejidad total: $O(n^2) \cdot O(n^3) = O(n^5)$*

---
