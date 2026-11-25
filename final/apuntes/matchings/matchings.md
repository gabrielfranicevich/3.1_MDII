# Apuntes de Teoría de Grafos: Matchings y Matchings Pesados
**Autor original:** Daniel Penazzi  
**Fechas de las clases:** 14, 16 y 26 de mayo de 2021

---

# Parte 1: Matchings en Grafos Bipartitos (Clase 1)

## 1. Definición y Problema
Dado un grafo $G$, un **matching** en $G$ es un subgrafo $M$ de $G$ tal que el grado (en $M$) de cada vértice de $M$ es uno. En otras palabras, un matching es un conjunto de lados disjuntos.

* **El problema a resolver:** Dado un grafo $G$ bipartito, encontrar un matching en $G$ con la mayor cantidad de lados posible.
* Un matching que tenga la mayor cantidad posible de lados se dice **matching maximal**.

Si bien hay problemas de matchings en grafos generales, nos concentraremos en matchings en grafos bipartitos porque:
1. Muchos problemas de aplicación sólo requieren un grafo bipartito.
2. El algoritmo para grafos no bipartitos se "apoya" en el algoritmo para bipartitos, así que hay que conocer este de todos modos. *(Nota: La persona que generalizó el algoritmo a grafos no bipartitos fue **Edmonds**, pero es mucho más complicado y no lo veremos).*

### Ejemplo de aplicación
Supongamos que $G$ es un grafo cuyos vértices son tareas que hay que hacer y trabajadores que pueden hacer esas tareas. Se une un trabajador con una tarea si el trabajador es capaz de hacerla. Queremos encontrar la mayor cantidad posible de emparejamientos tarea/trabajador, es decir, un **matching maximal**.

*(Más adelante veremos una generalización: asociar un costo a cada par tarea/trabajador y buscar un matching maximal de costo mínimo).*

---

## 2. Reducción del problema a flujos en networks
Para resolver el problema aprovecharemos lo aprendido sobre flujos:
1. Transformamos el grafo bipartito no dirigido en un *network* (con todas sus capacidades enteras).
2. Encontramos un flujo entero maximal en ese *network* (sabemos por el Teorema de la Integralidad que existe, y algoritmos como Edmonds-Karp o Dinitz lo encuentran).
3. A partir de ese flujo maximal obtenemos un matching maximal.

### Transformación del grafo en network
Sea $G$ bipartito, con partes $X$ e $Y$. Es decir, $V(G) = X \cup Y$, $X \cap Y = \emptyset$, y no hay lados entre vértices de $X$ ni entre vértices de $Y$. Los únicos lados son entre algún vértice de $X$ y algún vértice de $Y$.

Para construir el *network* $N$, agregamos dos vértices nuevos: una fuente $s$ y un resumidero $t$. Los lados dirigidos del *network* serán:
* $\vec{xy}$ para todo $x \in X, y \in Y$ tal que $xy \in E(G)$.
* $\vec{sx}$ para todo $x \in X$.
* $\vec{yt}$ para todo $y \in Y$.
* **Capacidad $1$ en todos los lados.**

### Relación entre matchings maximales en $G$ y flujos maximales enteros en $N$
Supongamos que tenemos un flujo entero $f$. Como la capacidad de todos los lados es $1$:
* Si $x \in X$, como sólo entra un lado a $x$ ($\vec{sx}$), entonces $\text{in}_f(x) = f(\vec{sx})$. Como $f$ es entero y la capacidad es $1$, $f(\vec{sx})$ es $1$ o $0$.
* Por conservación de flujo, $\text{out}_f(x) = \text{in}_f(x)$ también debe ser $1$ o $0$. Si $\text{out}_f(x) > 0$, debe ser $1$, y debe haber un único $y \in Y$ con $f(\vec{xy}) = 1$.
* Un razonamiento similar con $t$ aplica para los $y \in Y$: o bien $\text{out}_f(y) = 0$, o es $1$ (en cuyo caso existe un único $x \in X$ con $f(\vec{xy}) = 1$).

Concluimos que dado un flujo entero $f$, si definimos:
* $W = \{x \in X : \text{in}_f(x) = 1\}$
* $U = \{y \in Y : \text{out}_f(y) = 1\}$
* $F = \{xy \in E(G) : x \in W, y \in U, f(\vec{xy}) = 1\}$

Entonces $(W \cup U, F)$ es un subgrafo de $G$ que es un **matching**. Además, la cantidad de lados del matching es igual a la cantidad de $x \in X$ con $\text{in}_f(x) = 1$, lo cual es igual a $\text{out}_f(s) = v(f)$ (el valor del flujo).

**Viceversa:** Dado un matching, definimos $f = 1$ en los lados del matching, $0$ en los otros lados $\vec{xy}$, $f(\vec{sx}) = 1$ si $x$ está en el matching, y $f(\vec{yt}) = 1$ si $y$ está en el matching. Esto forma un flujo válido (construido camino a camino de la forma $sxyt$) cuyo valor es la cantidad de lados del matching. Por lo tanto, **flujos maximales enteros se corresponden con matchings maximales**.

### Algoritmo
1. Construir el *network* asociado.
2. Encontrar flujo maximal entero $f$ en el *network* (ej. usando Edmonds-Karp o Dinitz).
3. Tomar como matching aquellos lados $xy$ con $f(\vec{xy}) = 1$.

*Observación:* Si usáramos Dinitz, en el primer *network* auxiliar (nivel 0: $\{s\}$, nivel 1: $X$, nivel 2: $Y$, nivel 3: $\{t\}$), todo camino aumentante es de la forma $sxyt$, lo que equivale a agregar lados $xy$ al matching de forma directa sin romper la condición de matching. Luego se trata de ir extendiendo ese matching inicial.

---

## 3. Representación Matricial y Ejemplo
En muchos problemas de grafos bipartitos con partes $X$ e $Y$, hay una gran densidad de conexiones, por lo que conviene usar una matriz de adyacencia en lugar de listas de vecinos. La matriz total luce como:
$$\begin{bmatrix} 0 & A \\ A^t & 0 \end{bmatrix}$$
Por ello, se suele dar únicamente la submatriz $A$, cuyas filas representan elementos de $X$ y cuyas columnas representan elementos de $Y$. En grafos sin pesos, las entradas son $1$ o $0$. En grafos con pesos, se coloca el peso del lado $xy$ o $\infty$ si no existe el lado.

### Ejemplo paso a paso (Grafos y Colas)
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

### Estructura general de los caminos aumentantes
Por la estructura bipartita, todo camino aumentante es un **camino alternado** de la forma:
$$s x_{i_1} y_{i_1} x_{i_2} y_{i_2} \dots y_{i_{r-1}} x_{i_r} y_{i_r} t$$
Al actualizar el matching:
1. Se agregan los $r$ lados $x_{i_j}y_{i_j}$ ($j = 1, \dots, r$).
2. Se borran los $r-1$ lados $x_{i_j}y_{i_{j-1}}$ ($j = 2, \dots, r$).
Así, el tamaño del matching aumenta exactamente en $1$ en cada iteración.

### Rehaciendo el ejemplo directamente sobre la matriz
Para hacerlo a mano sobre la matriz $A$, marcamos los elementos del matching (por ejemplo, con corchetes `[1]`) y anotamos las etiquetas de la cola al costado de las filas y debajo de las columnas:
1. **Inicio:** Marcamos con `s` las filas no matcheadas (fila $C$).
2. **Revisar filas etiquetadas:** En la fila $C$, buscamos los `1`s no marcados (columnas $I$ y $II$) y las etiquetamos con `C`.
3. **Revisar columnas etiquetadas:** Miramos si en las columnas $I$ y $II$ hay algún `[1]` del matching.
   * Si **no hay**, la columna está libre y podemos extender el matching.
   * Si **hay**, etiquetamos la fila correspondiente con el nombre de la columna. Aquí $I$ etiqueta la fila $A$ con `I`, y $II$ etiqueta la fila $B$ con `II`.
4. **Continuar:** Revisamos las nuevas filas etiquetadas ($A$ y $B$). La fila $A$ tiene `1`s en $III$ y $IV$; etiquetamos ambas columnas con `A`.
5. **Extender:** Al revisar la columna $IV$, vemos que no tiene ningún `[1]` marcado (está libre). Miramos su etiqueta (`A`), marcamos `[1]` en la posición $(A, IV)$. Como la fila $A$ ahora tiene dos `[1]`, miramos la etiqueta de la fila $A$ (`I`) y desmarcamos $(A, I)$. Esto libera la columna $I$; miramos su etiqueta (`C`) y marcamos $(C, I)$. Al llegar a una fila con etiqueta `s`, el proceso termina.

---

## 4. Teoremas de Hall y Kőnig

### Matchings perfectos y completos
* **Matching perfecto:** Cubre todos los vértices de $G$ (requiere necesariamente que $|X| = |Y|$).
* **Matching completo sobre $X$ (o de $X$ a $Y$):** Cubre todos los vértices de la parte $X$ (requiere necesariamente que $|X| \le |Y|$). Si $|X| = |Y|$, todo matching completo es perfecto.

### Condición y Teorema de Hall
Dado $S \subseteq X$, definimos el conjunto de vecinos de $S$ como:
$$\Gamma(S) = \bigcup_{x \in S} \Gamma(x) = \{z \in Y : \exists x \in S \text{ con } xz \in E\}$$

**Condición de Hall:** Para que exista un matching completo sobre $X$, es necesario que $|S| \le |\Gamma(S)|$ para todo $S \subseteq X$.

**Teorema (Hall):** Si $G$ es un grafo bipartito con partes $X$ e $Y$, y $|S| \le |\Gamma(S)|$ para todo $S \subseteq X$, entonces existe un matching completo sobre $X$.

*Idea de la demostración (por el absurdo usando Edmonds-Karp):* 
Si no existe matching completo sobre $X$, al correr el algoritmo obtenemos un matching maximal que deja un subconjunto no vacío $S_0 \subseteq X$ sin cubrir. En el último paso de Edmonds-Karp se halla un corte minimal $C = \{s\} \cup S \cup T$ (con $S \subseteq X, T \subseteq Y$). Se demuestra que:
1. $S_0 \subseteq S$ (son los vértices agregados directamente por $s$).
2. Cada vértice de $S - S_0$ fue agregado *backward* por un único vértice de $T$, y todo vértice de $T$ agrega exactamente a un vértice de $S - S_0$, por lo que $|S - S_0| = |T|$.
3. Todos los vecinos de $S$ están en $T$, es decir, $T = \Gamma(S)$.
4. Luego, $|\Gamma(S)| = |T| = |S - S_0| = |S| - |S_0| < |S|$ (pues $S_0 \neq \emptyset$), contradiciendo la hipótesis.

### Teorema del Matrimonio de Kőnig (1914)
**Teorema:** Todo grafo bipartito regular (con grado $\Delta > 0$) tiene un matching perfecto.

*Contexto:* Si tenemos conjuntos de hombres y mujeres donde cada persona es compatible mutuamente con exactamente $k$ personas del otro grupo ($k > 0$), es posible casar a todos de forma compatible.

*Demostración:* Como $G$ es $\Delta$-regular y bipartito, la cantidad total de lados es $|E| = \Delta |X|$ y también $|E| = \Delta |Y|$. Como $\Delta \neq 0$, resulta $|X| = |Y|$. Para cualquier $S \subseteq X$, la cantidad de lados incidentes a $S$ es $|E_S| = \Delta |S|$. Todos estos lados inciden en $\Gamma(S) \subseteq Y$, por lo que $E_S \subseteq E_{\Gamma(S)}$, lo que implica $\Delta |S| \le \Delta |\Gamma(S)| \implies |S| \le |\Gamma(S)|$. Por el Teorema de Hall, existe un matching completo sobre $X$, que al ser $|X|=|Y|$, es perfecto.

*(Nota histórica: Dénes Kőnig publicó el primer libro de texto de teoría de grafos y probó también que en grafos bipartitos el tamaño del matching maximal es igual al tamaño de la cobertura minimal de vértices).*

### Teorema de Kőnig sobre coloreo de lados
Un **coloreo propio de lados** asigna colores a las aristas tal que dos aristas con un extremo en común tengan colores distintos. El **índice cromático** $\chi'(G)$ es la menor cantidad de colores necesarios. Por el Teorema de Vizing (1964), para todo grafo se cumple $\Delta \le \chi'(G) \le \Delta + 1$ (determinar cuál de los dos valores toma es NP-completo en general).

**Teorema (Kőnig):** Si $G$ es bipartito, entonces $\chi'(G) = \Delta$.

*Demostración:* 
1. **Caso regular:** Por inducción en $\Delta$. Si $\Delta = 1$, son lados disjuntos y basta $1$ color. Si $\Delta > 1$, por el Teorema del Matrimonio existe un matching perfecto $E_1$. Al borrar $E_1$ de $G$, obtenemos un grafo bipartito $(\Delta-1)$-regular $G^*$. Por hipótesis inductiva, $G^*$ se colorea con $\Delta - 1$ colores, y asignando un color nuevo a $E_1$ completamos el coloreo con $\Delta$ colores.
2. **Caso general:** Todo grafo bipartito $G$ puede sumergirse como subgrafo en un grafo bipartito $\Delta$-regular $H$ con el mismo grado máximo $\Delta(H) = \Delta(G)$ (duplicando las partes $X^*, Y^*$ y uniendo cada vértice $z$ de grado menor a $\Delta$ con su copia $z^*$ iterativamente hasta subir el grado mínimo $\delta$ hasta $\Delta$). Entonces $\chi'(G) \le \chi'(H) = \Delta(H) = \Delta(G)$.

---

# Parte 2: Matchings Pesados — Minimizar el Máximo (Clase 2)

## 1. Planteamiento del problema
En aplicaciones reales, asignar una tarea a un trabajador tiene un costo (tiempo, dinero, transporte). Supondremos un grafo bipartito con partes de igual tamaño $n$ (nota: aquí $n = |X| = |Y|$) y pesos en los lados, donde existe al menos un matching perfecto.

Existen dos criterios principales para minimizar costos:
1. **Minimizar la suma de los costos** (ej. tareas secuenciales, o costos pagados por una empresa).
2. **Minimizar el mayor costo** (ej. tareas ejecutadas en paralelo donde importa el tiempo total de finalización, o costos pagados individualmente por cada trabajador).

Evaluar todas las $n!$ biyecciones posibles tiene complejidad $O(n!)$, por lo que necesitamos algoritmos polinomiales.

---

## 2. Algoritmo de Minimizar el Máximo (Algoritmo de Gross)
Queremos hallar un matching perfecto que minimice el peso máximo entre sus aristas.

### Idea del algoritmo y Búsqueda Binaria
Si fijamos un umbral $\mu$ y eliminamos del grafo todos los lados con peso estrictamente mayor que $\mu$, podemos correr el algoritmo de matching maximal en el grafo resultante:
* Si existe un **matching perfecto**, sabemos que el óptimo es $\le \mu$ (y de hecho $\le$ al mayor peso usado en ese matching). Bajamos el umbral.
* Si **no existe** un matching perfecto, el umbral es muy restrictivo. Subimos el umbral.

Para evitar probar todos los pesos uno por uno ($O(n^2)$ búsquedas), ordenamos los pesos distintos de la matriz y aplicamos **búsqueda binaria** sobre ellos hasta encontrar un valor $\mu$ para el cual existe matching perfecto, pero no existe para el peso inmediatamente anterior.

*Optimización práctica:* Cuando encontramos un matching perfecto bajo un umbral $\mu$, si el peso máximo efectivo de ese matching es $\mu' < \mu$, podemos actualizar nuestra cota superior directamente a $\mu'$ antes de calcular el siguiente punto medio.

---

## 3. Análisis de Complejidades

### Complejidad de encontrar matching maximal en grafos bipartitos
* **Edmonds-Karp:** En redes generales es $O(V E^2)$. Aquí, buscar un camino aumentante cuesta $O(m)$ (donde $m$ es la cantidad de lados). Como todas las capacidades son $1$, cada camino aumentante incrementa el flujo en $1$ unidad, y el flujo máximo está acotado por $n$. Por lo tanto, hay a lo sumo $n$ iteraciones, dando una complejidad de **$O(mn)$**.
* **Dinic (Dinitz):** En redes con capacidades unitarias es $O(m n^{2/3})$. Sin embargo, en redes bipartitas todo vértice distinto de $s, t$ tiene grado de entrada $1$ o grado de salida $1$. Bajo esta propiedad adicional, Dinic corre en tiempo **$O(m\sqrt{n})$**.

### Complejidad de Minimizar el Máximo
En el peor caso, el grafo es completo ($m = n^2$) y todos los pesos son distintos:
1. **Ordenar los pesos:** Usando un algoritmo de ordenamiento óptimo toma $O(n^2 \lg(n^2)) = O(n^2 \lg n)$.
2. **Búsqueda binaria:** Realiza $O(\lg(n^2)) = O(\lg n)$ llamadas al algoritmo de matching maximal.
   * Con Dinic: $O(n^{5/2} \lg n)$.
   * Con Edmonds-Karp: $O(n^3 \lg n)$.

El costo de ordenamiento queda absorbido, resultando en una complejidad total de **$O(n^{5/2} \lg n)$** usando Dinic.

---

## 4. Ejemplo de Minimizar el Máximo
Dada una matriz $8 \times 8$ de tiempos con valores distintos $\{1, 4, 5, 6, 7, 8, 9, 10, 15\}$:
1. **Umbral $\mu = 7$** (mediana de los 9 valores): Convertimos en `1` las entradas $\le 7$ y en `0` las $> 7$. Corriendo el algoritmo matricial, tras dos extensiones de caminos aumentantes, encontramos un matching perfecto cuyo peso máximo es $7$ ($Ai:5, Bvii:1, Cvi:7, Dviii:4, Eiii:4, Fv:6, Giv:4, Hii:1$).
2. **Umbral $\mu = 5$** (mediana de $\{1, 4, 5, 6, 7\}$): Filtramos entradas $\le 5$. Encontramos un matching inicial y, tras una extensión a partir de la fila libre $F$ (camino $F \to ii \to D \to iii \to E \to vi$), obtenemos un matching perfecto con peso máximo $5$ ($Ai:5, Bvii:1, Cviii:1, Diii:1, Evi:5, Fii:5, Giv:4, Hv:4$).
3. **Umbral $\mu = 4$** (valores restantes $\{1, 4\}$): Al filtrar por $\le 4$, la fila $F$ queda compuesta únicamente por ceros, lo que garantiza que no existe ningún matching perfecto (en general, si no hay filas/columnas de ceros, el algoritmo igualmente arrojará un conjunto $S$ que viola la condición de Hall).

**Conclusión:** El valor óptimo mínimo para el tiempo máximo es **$5$**.

---

# Parte 3: Matchings Pesados — Algoritmo Húngaro (Clase 3)

## 1. Minimizar la Suma de Costos
Dada una matriz de costos $C$ de tamaño $n \times n$, buscamos una permutación $\sigma: \{1, \dots, n\} \to \{1, \dots, n\}$ que minimice:
$$\sum_{i=1}^{n} C_{i, \sigma(i)}$$

El **Algoritmo Húngaro** (creado por Harold Kuhn en 1955 en honor a los matemáticos húngaros Dénes Kőnig y Jenő Egerváry) resuelve este problema en tiempo polinomial.

### Propiedades Fundamentales
1. **Propiedad Obvia:** Si $C_{i,j} \ge 0$ para todo $i, j$, y existe un matching perfecto $\sigma$ tal que $C_{i, \sigma(i)} = 0$ para todo $i$, entonces $\sigma$ minimiza la suma de los costos (ya que ninguna asignación puede sumar menos que $0$).
2. **Propiedad Casi Obvia:** Si obtenemos una matriz $\tilde{C}$ restándole una constante $r$ (positiva o negativa) a toda una fila o a toda una columna de $C$, entonces una permutación $\sigma$ minimiza la suma respecto de $C$ **si y sólo si** minimiza la suma respecto de $\tilde{C}$. Esto se debe a que el costo de *todas* las asignaciones posibles se ve desplazado exactamente en la misma constante $-r$.

---

## 2. Desarrollo del Algoritmo Húngaro
La estrategia consiste en transformar la matriz mediante sumas y restas a filas y columnas (manteniendo siempre todas las entradas $\ge 0$) hasta que aparezca un matching perfecto compuesto exclusivamente por ceros.

### Fase Inicial
1. Restar el mínimo de cada fila a todos los elementos de esa fila.
2. Restar el mínimo de cada columna a todos los elementos de esa columna.
3. Buscar un matching maximal de ceros en la matriz resultante. Si es perfecto, terminamos.

### ¿Qué hacer si no hay matching perfecto de ceros?
Si el matching maximal de ceros no es perfecto, el algoritmo de búsqueda matricial se detendrá dejando algunas filas etiquetadas. Por la demostración del Teorema de Hall, el conjunto $S$ formado por **todas las filas etiquetadas** cumple que $|S| > |\Gamma(S)|$, donde $\Gamma(S)$ son **todas las columnas etiquetadas**.

Esto nos indica que las filas de $S$ están "compitiendo" por muy pocos ceros en $\Gamma(S)$, por lo que debemos generar nuevos ceros en la intersección de las filas de $S$ con las columnas **no etiquetadas** (el complemento de $\Gamma(S)$):
1. Calculamos $m = \min \{ C_{i,j} : i \in S, j \notin \Gamma(S) \}$.
2. Restamos $m$ a todas las filas de $S$. Esto crea al menos un nuevo cero en la zona $S \times \overline{\Gamma(S)}$ sin generar negativos allí, pero puede generar valores negativos en la intersección $S \times \Gamma(S)$.
3. Para eliminar esos negativos, **sumamos $m$ a todas las columnas de $\Gamma(S)$**.

*Observación clave:* Al sumar $m$ a las columnas de $\Gamma(S)$, pueden desaparecer algunos ceros que estaban en filas fuera de $S$ y columnas dentro de $\Gamma(S)$. Sin embargo, **ninguno de esos ceros perdidos pertenecía al matching actual**, por lo que no debemos empezar de cero: conservamos el matching y las etiquetas actuales, revisamos las filas de $S$ para etiquetar las columnas de los nuevos ceros creados, y continuamos buscando caminos aumentantes.

---

## 3. Resumen del Algoritmo Húngaro
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

## 4. Ejemplo Completo del Algoritmo Húngaro
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