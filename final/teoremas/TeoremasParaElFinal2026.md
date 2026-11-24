# Teóricos para el final - MDII - 2026

2 preguntas tomadas de esta lista de 16. A lo sumo se toma 1 de cada sub-lista

**OBSERVACIÓN PARA LOS PRIMEROS 3 ejercicios:** como explicamos en el teórico, casi toda la prueba de estos teoremas valdrían para p. ej. Ford-Fulkerson, excepto por unas partes claves de la prueba donde se usa una propiedad específica de Edmonds-Karp. Si ustedes escriben toda la prueba sin destacar qué propiedad de Edmonds-Karp se usa y dónde, el ejercicio no está aprobado.

## Lista de Teoremas

### A

1. ¿Cuál es la complejidad del algoritmo de Edmonds-Karp? Probarlo. *(Nota: en la prueba se definen unas distancias, y se prueba que esas distancias no disminuyen en pasos sucesivos de EK. Ud. puede usar esto sin necesidad de probarlo).*

2. Probar que si, dados vértices $x, z$ y flujo $f$, definimos a la distancia entre $x$ y $z$ relativa a $f$ como la longitud del menor $f$-camino aumentante entre $x$ y $z$, si es que existe tal camino, o infinito si no existe o $0$ si $x=z$, denotándola por $d_{f}(x,z)$, y definimos $b_{k}(x)=d_{f_{k}}(x,t)$, donde $f_{k}$ es el $k$-ésimo flujo en una corrida de Edmonds-Karp, entonces $b_{k}(x) \le b_{k+1}(x)$. *(Este teorema lo dimos en clase, pero no está en los pdf).*

3. Probar que si, dados vértices $x, z$ y flujo $f$, definimos a la distancia entre $x$ y $z$ relativa a $f$ como la longitud del menor $f$-camino aumentante entre $x$ y $z$, si es que existe tal camino, o infinito si no existe o $0$ si $x=z$, denotándola por $d_{f}(x,z)$, y definimos $d_{k}(x)=d_{f_{k}}(s,x)$ donde $f_{k}$ es el $k$-ésimo flujo en una corrida de Edmonds-Karp, entonces $d_{k}(x) \le d_{k+1}(x)$. *(Este teorema está en los pdf, pero no lo dimos en clase).*

4. ¿Cuál es la complejidad del algoritmo de Dinic? Probarla en ambas versiones: Dinitz original y Dinic-Even *(no hace falta probar que la distancia en networks auxiliares sucesivos aumenta)*. *(Nota: en algunos exámenes se puede pedir una sola de las dos versiones, en otros las dos).*

5. ¿Cuál es la complejidad del algoritmo de Wave? Probarla *(no hace falta probar que la distancia en networks auxiliares sucesivos aumenta)*. *(Nota: en algunos exámenes se puede pedir que prueben la complejidad de una subparte de Wave, p. ej., la que denotamos* $S$ *en el teórico).*

6. Probar que la distancia en networks auxiliares sucesivos aumenta. *(Este teorema sólo se tomará a partir de noviembre 2026).*

### C

1. Probar que si $f$ es un flujo maximal entonces existe un corte $S$ tal que $v(f) = \text{cap}(S)$. *(Puede usar sin necesidad de probarlo que si* $f$ *es flujo y* $S$ *es corte entonces* $v(f) = f(S, \overline{S}) - f(\overline{S}, S)$*).*

2. Probar que si $G$ es conexo no regular, entonces $\chi(G) \le \Delta(G)$.

3. Enunciar y probar el Teorema de Hall.

4. Enunciar y probar el teorema del matrimonio de Kőnig.

5. Demostrar la complejidad $O(n^{4})$ del algoritmo Húngaro y dar una idea de cómo se puede obtener una complejidad $O(n^{3})$.

6. Enunciar el teorema de la cota de Hamming y probarlo.

7. Probar que si $H$ es matriz de chequeo de $C$, entonces:
   

   $$
   \delta(C) = \min\{j : \exists \text{ un conjunto de } j \text{ columnas LD de } H\}
   $$

   
   *(LD es "linealmente dependiente").*

8. Sea $C$ un código cíclico de dimensión $k$ y longitud $n$ y sea $g(x)$ su polinomio generador. Probar que:

   * i) $C$ está formado por los múltiplos de $g(x)$ de grado menor que $n$:
     

     $$
     C = \{p(x) : \text{gr}(p) < n \ \& \ g(x) \mid p(x)\}
     $$

   * ii) $C = \{v(x) \odot g(x) : v(x) \text{ es un polinomio cualquiera}\}$

   * iii) $\text{gr}(g(x)) = n - k$.

   * iv) $g(x)$ divide a $1 + x^{n}$.

### B

1. Probar que 3-COLOR es NP-completo.

2. Probar que Matrimonio3D ("matrimonio trisexual") es NP-completo.