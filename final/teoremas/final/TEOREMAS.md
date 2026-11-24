---
fontsize: 11pt
geometry: margin=2cm
linestretch: 1.1
toc: true
toc-depth: 2
---

# Teoremas para el final julio 2024

\newpage

## 1. Complejidad E-K

La complejidad de E-K es $O(n m^2)$ donde $n=|V|$ y $m=|E|$ sobre una network $N=(V, E, c)$.

**Demostración.**

Edmonds-Karp construye una sucesión de caminos aumentantes utilizando BFS.

La complejidad de E-K entonces está dada por
$$
\operatorname{Comp}(\text{E-K})
=O(\operatorname{Comp}(\mathrm{BFS}))\cdot\#\text{caminos aumentantes}
=O(m\cdot\#\text{caminos aumentantes}).
$$

**Definición.** Un lado "se vuelve" crítico en un paso dado si es usado para construir el siguiente flujo en modo forwards y se satura o en modo backwards y se vacía.

¿Cuántas veces se puede volver crítico un lado? Primero necesitaremos algunas definiciones.

**Definición.** Dado un flujo $f$ en $N$ y vértices $x, z$,
$$
d_f(x,z)=
\begin{cases}
0 & \text{si } x=z,\\
\infty & \text{si no existe un camino aumentante de } x \text{ a } z,\\
\text{longitud mínima de un camino aumentante de } x \text{ a } z & \text{si existe.}
\end{cases}
$$

Si $f_0, f_1, \dots$ son los flujos que se van obteniendo en EK, definimos:
$d_k(x) = d_{f_k}(s, x)$ , $b_k(x) = d_{f_k}(x, t)$

**Proposición.** $d_k(x) \le d_{k+1}(x)$ y $b_k(x) \le b_{k+1}(x)$ $\forall x$ (no hace falta demostrarla)

**Observación.** $d_k(t) = d_k(x) + b_k(x)$ si ambas son finitas.

¿Cuántas veces se puede volver crítico un lado?
Sea $\vec{xy}$ un lado que se vuelve crítico en el paso $k$. Esto puede darse de dos formas:

**a)** $\vec{xy}$ se satura en el paso $k$
$\implies f_k(\vec{xy}) < c(\vec{xy}), f_{k+1}(\vec{xy}) = c(\vec{xy})$
$\implies$ se utilizó un camino aumentante $s \dots x y \dots t$ de longitud mínima (EK)
$\therefore d_k(y) = d_k(x) + 1$

Si $\vec{xy}$ se vuelve crítico de nuevo en algún paso $j > k$, entonces

- Se vació $\implies$ se usó backwards en el paso $i$
- Se saturó $\implies$ se usó backwards en algún paso $i$ con $k \le i < j$

$\implies \exists i \text{ tq } k \le i \le j$ donde $\vec{xy}$ se usa en modo backwards
$\implies$ se utilizó un camino aumentante de longitud mínima de la forma
$s \dots y x \dots t \text{ tq } d_i(x) = d_i(y) + 1$

Luego
$d_j(t) \ge d_i(t) = d_i(x) + b_i(x)$
$= d_i(y) + 1 + b_i(x)$
$\ge d_k(y) + 1 + b_k(x)$
$= d_k(x) + 1 + 1 + b_k(x)$
$= d_k(t) + 2$

\newpage

**b)** $\vec{xy}$ se vacía en el paso $k$
$\implies f_k(\vec{xy}) > 0 \text{ y } f_{k+1}(\vec{xy}) = 0$
$\implies$ se utilizó un camino aumentante de longitud mínima (EK) de la forma $s \dots y \overleftarrow{x} \dots t$
$\therefore d_k(x) = d_k(y) + 1$

Si $\vec{xy}$ se vuelve crítico de nuevo en un paso $j > k$, entonces
- Se vació $\implies$ se usó forwards en algún paso $i$ con $k \le i \le j$
- Se saturó $\implies$ se usó forwards en el paso $j$.

$\therefore$ se usó forwards en algún paso $i$ con $k \le i \le j$ con un camino aumentante de longitud mínima de la forma
$s \dots \vec{xy} \dots t \text{ de manera tal que }$
$d_i(y) = d_i(x) + 1$

$d_j(t) \ge d_i(t) = d_i(y) + b_i(y)$
$= d_i(x) + 1 + b_i(y)$
$\ge d_k(x) + 1 + b_k(y)$
$= d_k(y) + 1 + 1 + b_k(y)$
$= d_k(t) + 2$

En ambos casos (a y b), la distancia entre $s$ y $t$ debe aumentar en al menos 2 para que un lado pueda volver a ser crítico. Como esta distancia va entre $0$ y $n-1$, un lado puede volverse crítico $O(n)$ veces a lo sumo.

Finalmente,

- Cada camino aumentante de EK vuelve crítico al menos un lado
- Cada lado puede volverse crítico $O(n)$ veces.
- Hay $m$ lados $\implies$ hay $O(m \cdot n)$ caminos aumentantes posibles.

$$
\operatorname{Comp}(\mathrm{EK})
=O(m\cdot\#\text{caminos aumentantes})
=O(m\cdot m\cdot n)
=O(m^2n).
$$

---

\newpage

## 2. Las distancias de E-K no disminuyen

**Definición.** Dados vértices $x, z$ en un network $N$ y un flujo $f$,
$$
d_f(x,z)=
\begin{cases}
0 & \text{si } x=z,\\
\infty & \text{si no existe un camino aumentante de } x \text{ a } z,\\
\text{longitud mínima de un camino aumentante de } x \text{ a } z & \text{si existe.}
\end{cases}
$$

**Definición.** $d_k(x) = d_{f_k}(s, x)$ para $f_k$ el k-ésimo flujo en una corrida de EK

**Proposición.** $d_k(x) \le d_{k+1}(x) \quad \forall x$

**Demostración.**

Sea $A = \{x : d_{k+1}(x) < d_k(x)\}$, y supongamos que $A \neq \emptyset$
Sea $x \in A$ tal que $d_{k+1}(x) = \min \{d_{k+1}(y) : y \in A\}$ [1]

[1] implica que

$$
d_{k+1}(x)<d_k(x)\le\infty
\quad\Longrightarrow\quad
d_{k+1}(x)<\infty,
$$

por lo que existe un camino aumentante de $s$ a $x$.
**Observación.** $x \neq s$ pues $s \notin A$ ya que $d_k(s) = d_{k+1}(s)$
Sea entonces $P_{k+1} : s \dots x$ un $f_{k+1}$-cam-aum entre $s$ y $x$ de longitud mínima, es decir de longitud $d_{k+1}(x)$.
Como $x \neq s$, existe un vértice $z$ en $P_{k+1}$ inmediatamente anterior a $x$.
Luego $P_{k+1} : s \dots z x$ y por ser de long. Mínima, $d_{k+1}(z) = d_{k+1}(x) - 1$ [2]
Ahora hay dos opciones
A) $\vec{zx}$ es un lado
B) $\vec{xz}$ es un lado

Veamos ambos casos

**a)** $\vec{zx}$ es un lado
[2] $\implies d_{k+1}(z) < d_{k+1}(x) \stackrel{[1]}{\implies} z \notin A \implies d_k(z) \le d_{k+1}(z) < \infty$
$\implies \exists f_k\text{-cam-aum entre } s \text{ y } z$.
Sea $P_k : s \dots z$ un $f_k$-cam. Aum de long. Mínima, como $\vec{zx} \in E$, podríamos agregarlo y obtener un $f_k$-cam-aum entre $s$ y $x$ tal que
$d_k(x) \le d_k(z) + 1 \le d_{k+1}(z) + 1 \le d_{k+1}(x)$ absurdo pues $x \in A$.
$\therefore \vec{zx}$ debe estar saturado $f_k(\vec{zx}) = c(\vec{zx})$
Pero $P_{k+1} : s \dots z x$ es $f_{k+1}$-cam-aum $\implies f_{k+1}(\vec{zx}) < c(\vec{zx})$ $\implies \vec{zx}$ se utilizó de modo backwards al pasar de $f_k$ a $f_{k+1}$
Es decir se utilizó un camino de longitud mínima de la forma
$\tilde{P}_k : s \dots x \overleftarrow{z} \dots t$
Luego $d_k(z) = d_k(x) + 1 > d_{k+1}(x) + 1 = d_{k+1}(z) + 1 + 1$
$\ge d_k(z) + 2 \implies 0 > 2$ absurdo de asumir $\vec{zx}$ lado.

\newpage

**b)** $\vec{xz}$ es un lado
Luego $P_{k+1}$ es de la forma $s \dots \overleftarrow{z} x$

$$
\begin{aligned}
\implies d_{k+1}(z) < d_{k+1}(x) \stackrel{[1]}{\implies} z \notin A\\
&\implies d_k(z) \le d_{k+1}(z) < \infty\\
&\implies \exists f_k\text{-cam-aum entre } s \text{ y } z
\end{aligned}
$$

$P_k : s \dots z$ uno de ellos, de longitud mínima, podría agregarle $\vec{xz}$ backwards, pero entonces
$d_k(x) \le d_k(z) + 1 \le d_{k+1}(z) + 1 \le d_{k+1}(x) \implies$ absurdo pues $x \in A$
Como no lo puedo agregar backwards, entonces $f_k(\vec{xz}) = 0$
Pero si $P_{k+1} : s \dots \overleftarrow{z} x$ es un $f_{k+1}$-cam-aum, entonces $f_{k+1}(\vec{xz}) > 0$
$\implies$ se utilizó $\vec{xz}$ forwards al pasar de $f_k$ a $f_{k+1}$
$\therefore \exists f_k \text{-cam-aum de longitud mínima de la forma}$
$\tilde{P}_k : s \dots \vec{xz} \dots t$
$\implies d_k(z) = d_k(x) + 1 > d_{k+1}(x) + 1 = d_{k+1}(z) + 1 + 1 \ge d_k(z) + 2 \implies 0 > 2$ absurdo de asumir que $\vec{xz}$ es un lado

Concluimos entonces que $A = \emptyset$ y $d_k(x) \le d_{k+1}(x) \quad \forall x$ $\blacksquare$

---

\newpage

## 3. La complejidad de Dinic, ambas versiones

es $O(n^2 m)$

**Demostración.**

Como el nivel de $t$ aumenta en los NA sucesivos, sólo puede haber $O(n)$ NA's, luego
$$
\begin{aligned}
\operatorname{Comp}(\mathrm{Dinic})
&=O(n)\Bigl(\underbrace{O(m)}_{\text{construcción del nivel con BFS}}+\operatorname{Comp}(\text{flujo bloqueante})\Bigr)\\
&=O(n)\bigl(O(m)+\operatorname{Comp}(\text{flujo bloqueante})\bigr).
\end{aligned}
$$

Veamos la complejidad de hallar un flujo bloqueante en ambas versiones:

**Versión original.**
El NA es tal que no hay vértice sin lados de salida (*)
$\therefore$ DFS siempre llega a $t$ sin backtracking $\implies$ DFS es $O(\#\text{niveles}) = O(n)$
Como cada camino borra un lado del NA, hay $O(m)$ caminos en cada NA.
$\therefore$ la complejidad de hallar los caminos y aumentar el flujo por ellos es $O(n m)$ [1]

Ahora bien, ¿cuál es la complejidad de asegurar (*)?
Para ello, se deben recorrer los vértices luego de cada nuevo camino y borrar aquellos que no tengan lados de salida
Hay $O(m)$ caminos $\therefore$ se "poda" $O(m)$ veces a los $O(n)$ vértices, chequeando sus salidas en $O(1)$, total: $O(m n)$ [2]
$\operatorname{Comp}(\text{Borrar un vértice y sus lados}) = O(d(x))$, pero sobre todos los "podar" se obtiene $\sum_x O(d(x)) = O(m)$ [3]

Finalmente,

$$
\begin{aligned}
\operatorname{Comp}(\text{flujo bloqueante})
&=\underbrace{O(mn)}_{[1]}+\underbrace{O(mn)}_{[2]}+\underbrace{O(m)}_{[3]}\\
&=O(mn).
\end{aligned}
$$
$\therefore \operatorname{Comp}(\mathrm{Dinic}) = O(n)(O(m) + O(m n)) = O(m n^2)$ $\blacksquare$

**Versión occidental.**
Para ello avanzamos en pseudo-código

```js
g=0; FLAG=1;
while (FLAG)
  P:=[s];
  x:=s;
  while ((x != t) and FLAG)
    if (Γ⁺(x) ≠ ∅)
      TOMAR y ∈ Γ⁺(x)
      P.add(y); x:=y;                                              // A (AVANZAR)
    else
      if (x != s)
        z:= ELEM ANT DE x EN P
        P.delete(x); NA.delete(zx); x:=z;                          // R (RETROCEDER)
      else
        FLAG=0
  if (x == t)
    AUMENTAR EL FLUJO A LO LARGO DE P Y BORRAR LOS LADOS SATURADOS // I (INCREMENTAR)
return g;
```

Una corrida del algoritmo es de la forma $\overbrace{A \dots A R A \dots A I}^{PATRÓN \text{ a}\dots\text{Ax repetido}} A \dots A R \dots$
$O(A) = O(1)$ operación add
$O(R) = O(1)$ operación delete
$O(I) = O(n)$ recorrer p 2 veces p/aumentar flujo y borrar lados

¿Cuántas a hay en cada parte del patrón repetido? Como cada a avanza un nivel en el NA, hay $O(n)$ a's hasta tener que retroceder o llegar a $t$.
$$
\begin{aligned}
\operatorname{Comp}(A\dots AR)&=O(n)+O(1)=O(n),\\
\operatorname{Comp}(A\dots AI)&=O(n)\cdot O(1)+1\cdot O(n)=O(n).
\end{aligned}
$$
Como r borra un lado e i borra al menos un lado, hay $O(m)$ repeticiones de a...ax
Finalmente $\operatorname{Comp}(\text{hallar flujo bloqueante}) = O(m) \cdot O(n) = O(m n)$ $\blacksquare$

---

\newpage

## 4. Complejidad de wave de Tarjan

es $O(n^3)$

Como wave es de tipo Dinic y la distancia entre $s$ y $t$ aumenta en los NA's sucesivos, hay $O(n)$ NA's, pues $t$ no puede estar a más de $n$ lados de $s$.
Luego

$$
\begin{aligned}
\operatorname{Comp}(\mathrm{Wave})
={}&\underbrace{O(n)}_{\text{cantidad de niveles}}\Bigl(
\underbrace{O(m)}_{\text{construcción del nivel con BFS}}\\
&+\underbrace{\operatorname{Comp}(\text{flujo bloqueante})}_{\text{queremos acotar por }O(n^2)}\Bigr).
\end{aligned}
$$
Notar que $O(m) + O(n^2) = O(n^2)$ pues $m \le \binom{n}{2} = O(n^2)$

El proceso se divide en
**"olas" o "balanceos" hacia adelante:**
Cuando un vértice $x$ le manda flujo a $z$, puede que $\vec{xz}$ se sature o no.
$S = \text{Todos los casos donde se satura un lado}$
$P = \text{Todos los casos donde no se satura un lado}$

**"olas" o balanceos hacia atrás:**
Cuando $x$ le devuelve flujo a $v$, puede que $\overleftarrow{vx}$ se vacíe o no
$V = \text{Casos donde se vacía un lado}$
$Q = \text{Casos donde no se vacía un lado}$

$S$: sup $\vec{xz}$ saturado, ¿puede volver a saturarse?
Para ello, primero $z$ le debe devolver flujo a $x$, con lo cual $z$ está bloqueado, pero $x$ no le volverá a mandar pues sólo se envía hacia adelante a vértices no bloqueados.
$\therefore$ c/lado se satura a lo sumo una vez,
[1] $\operatorname{Comp}(S) = O(m)$

$V$: sup $\overleftarrow{vx}$ se vacía. ¿Puede volver a vaciarse? No, pues si $x$ le devolvió flujo a $v$ para vaciar el lado $\overleftarrow{vx}$, entonces $x$ está bloqueado y $v$ no le mandará más flujo como para que $x$ pueda devolverlo. $\therefore \overleftarrow{vx}$ no se podrá vaciar de nuevo. C/lado se vacía una vez a lo sumo.
[2] $\operatorname{Comp}(V) = O(m)$

$P$ y $Q$
En una ola hacia adelante, todos los lados, menos quizás el último usado, se saturan.
$\therefore$ en cada vértice en cada ola hacia adelante, hay a lo sumo 1 lado que no se satura y se incluye en $P$. Enviar flujo por este lado es $O(1)$.
[3] $\operatorname{Comp}(P) = O(n) \cdot \#\text{olas hacia adelante}$

Lo mismo ocurre con $Q$ pues en cada vértice de cada ola hacia atrás, hay a lo sumo un lado que no se vacía, y devolver flujo por él es $O(1)$.
[4] $\operatorname{Comp}(Q) = O(n) \cdot \#\text{olas hacia atrás}$

$\#\text{olas hacia adelante} = \#\text{olas hacia atrás}$
En c/ola hacia adelante, salvo quizás en la última, algún vértice queda desbalanceado y $\therefore$ bloqueado, y nunca se desbloquea.
[5] $\#\text{olas hacia adelante} = O(n)$

[3], [4] y [5] $\implies \operatorname{Comp}(P) = \operatorname{Comp}(Q) = O(n) \cdot O(n) = O(n^2)$

$\therefore \operatorname{Comp}(\text{hallar flujo bloqueante}) = [1] + [2] + [3] + [4]$
$= O(m) + O(m) + O(n^2) + O(n^2)$
$= O(n^2)$ $\blacksquare$

---

\newpage

## 5. Max flow min cut

A) el valor de todo flujo es menor o igual que la capacidad de todo corte
B) $f$ flujo $\implies f$ maximal $\iff \exists \text{ corte } S \text{ tq } v(f) = CAP(S)$ (y este corte es minimal)

**Demostración.**

**a)** Vimos en un lema que si $f$ es un flujo y $S$ es un corte,
$v(f) = f(S, \bar{S}) - f(\bar{S}, S) \le f(S, \bar{S}) \le c(S, \bar{S}) = CAP(S)$
(ya que $\sum_{x \in S, y \notin S, \vec{xy} \in E} f(\vec{xy}) \ge 0$ pues $f(\vec{xy}) \ge 0 \quad \forall \vec{xy} \in E$)

**b)** $f$ flujo
($\implies$) $f$ maximal, definimos $S = \{s\} \cup \{x \in V : \exists f\text{-cam-aum entre } s \text{ y } x\}$
Veamos que $S$ es un corte, $s \in S \checkmark$
Si no fuera corte

$$
\begin{aligned}
\implies t \in S\\
&\implies \exists f\text{-cam-aum entre } s \text{ y } t\\
&\implies \text{Puedo aumentar el flujo en algún } "\epsilon"
\end{aligned}
$$


$\therefore \text{Existirá } f' \text{ con } v(f') = v(f) + \epsilon > v(f)$
Absurdo pues $f$ es maximal.
$\therefore t \notin S$ y $S$ es corte.

Ahora podemos usar el lema mencionado anteriormente para ver
[0] $v(f) = f(S, \bar{S}) - f(\bar{S}, S)$

[1] $f(S, \bar{S}) = \sum_{x \in S, y \notin S, \vec{xy} \in E} f(\vec{xy})$
Para cualquier par $\vec{xy}$ de la sumatoria,
$x \in S \implies \exists f\text{-cam aum de } s \text{ a } x$
$y \notin S \implies \nexists f\text{-cam aum de } s \text{ a } y$
$\vec{xy} \in E \implies s \dots \vec{xy}$ podría ser cam-aum pero no lo es
$\therefore f(\vec{xy}) = c(\vec{xy}) \quad \forall \vec{xy}$
$\therefore f(S, \bar{S}) = c(S, \bar{S}) = CAP(S)$

$f(\bar{S}, S) = \sum_{y \notin S, x \in S, \vec{yx} \in E} f(\vec{yx})$
Para cualquier par $\vec{yx}$,
$y \notin S \implies \nexists s \dots y \quad f\text{-cam aum}$
$x \in S \implies \exists s \dots x \quad f\text{-camaum}$
$\vec{yx} \in E \implies$ le podría agregar $s \dots x \overleftarrow{y}$, pero ese camino no es aumentante $\implies f(\vec{yx}) = 0$
[2] $\therefore f(\bar{S}, S) = 0$

De [0], [1] y [2] $\implies v(f) = f(S, \bar{S}) - f(\bar{S}, S) = CAP(S)$. Como todo corte $T$ cumple
$CAP(T) \ge v(f) = CAP(S) \implies S \text{ es minimal}$.

($\impliedby$) $\exists \text{ corte } S \text{ tal que } v(f) = CAP(S)$
Sea $g$ otro flujo,
$v(g) \stackrel{(a)}{\le} CAP(S) = v(f) \implies f \text{ maximal}$
Y $S$ es minimal pues si $T$ es otro corte
$CAP(T) \ge v(f) = CAP(S)$ $\blacksquare$

---

## 6. Baby Brooks

$G \text{ conexo no regular} \implies \chi(G) \le \Delta$

**Demostración.**

Sea $x \text{ tq } d(x) = \delta$
Corremos $BFS(x)$, obteniendo todos los vértices y el orden en el que fueron agregados.
Ahora corremos greedy con el orden inverso al obtenido de manera tal que la raíz $x$ será el último por colorear.
$c(\text{Primer vértice}) = 1$ (es posterior en el orden inverso)
Para colorear cualquier vértice $z \neq x$, sabemos que $z$ fue incluido a BFS por un vecino anterior que aún no fue coloreado, por lo tanto se descartarán, en el peor caso, $d(z) - 1 \le \Delta - 1$ colores, siendo posible utilizar algún color en $\{1, \dots, \Delta\}$ para $z$.
Ahora bien cuando llegamos a $x$, como $d(x) = \delta < \Delta$ (por no ser regular), habrá siempre un color en $\{1, \dots, \Delta\}$ que sus vecinos no utilicen, entonces lo utiliza.
De esta forma se obtuvo un coloreo propio de $G$ con $\Delta$ colores
$\therefore \chi(G) \le \Delta$ $\blacksquare$

## 7. 2-color es polinomial

Para ello daré un algoritmo polinomial que colorea un grafo $G$ con dos colores y mostraré que si el coloreo no es propio entonces $\chi(G) \ge 3$.
Si $G$ no es conexo se corre el algoritmo en todas sus componentes conexas y sigue siendo polinomial.

**Algoritmo de coloreo.**
Sea $x$ un vértice arbitrario de $G$
Corro $BFS(x)$ obteniendo un orden de vértices en distintos niveles. $\implies O(m) \text{ por BFS}$
Luego coloreo todo vértice $z$ de la forma
$c(z) = \operatorname{nivel}_{\mathrm{BFS}}(z) \pmod 2$
Luego chequeo si es propio, en $\sum_{v \in V} O(d(v)) = O(m)$

- Si es propio, entonces existe un coloreo propio con 2 colores $\implies \chi(G) \le 2$ $\checkmark$
- Si no es propio,
$\exists u, v \text{ vértices tq } c(u) = c(v) \text{ y } uv \in E$
$\implies \operatorname{nivel}_{\mathrm{BFS}}(u) \pmod 2 = \operatorname{nivel}_{\mathrm{BFS}}(v) \pmod 2$
$\implies \operatorname{nivel}_{\mathrm{BFS}}(u) + \operatorname{nivel}_{\mathrm{BFS}}(v) \text{ es par }$ [1]
Sabemos que existe un camino, por BFS, desde la raíz hasta $u$ y otro hasta $v$
$x \dots u$
$x \dots v$
$\therefore \exists w \text{ vértice a partir del cual los caminos difieren }$ 
$x \dots w \dots u$
$x \dots w \dots v$
Como $uv \in E$, tenemos el ciclo
$w \dots u v \dots w$
¿Cuántos lados tiene?
$w \dots u \implies \operatorname{nivel}_{\mathrm{BFS}}(u) - \operatorname{nivel}_{\mathrm{BFS}}(w)$
$v \dots w \implies \operatorname{nivel}_{\mathrm{BFS}}(v) - \operatorname{nivel}_{\mathrm{BFS}}(w)$
$uv \implies 1 \text{ lado}$
En total, el ciclo tiene longitud

$$
\begin{aligned}
&\underbrace{\operatorname{nivel}_{\mathrm{BFS}}(u)+\operatorname{nivel}_{\mathrm{BFS}}(v)}_{\text{par por [1]}}
-\underbrace{2\operatorname{nivel}_{\mathrm{BFS}}(w)}_{\text{par}}+\underbrace{1}_{uv}\\
&=\text{impar}.
\end{aligned}
$$
Y por lo tanto hay un ciclo con una cantidad impar de lados $\implies \chi(G) \ge 3$.

Y de esta forma se determina en tiempo polinomial si un grafo es o no 2-coloreable (sobre el tamaño de g).

---

\newpage

## 8. Teorema de Hall

Sea $G$ un grafo bipartito con partes $X$ e $Y$.
Existe un matching completo de $X$ a $Y$ si todo subconjunto de $X$ tiene más (o igual) vecinos que ellos. Formalmente:
$\exists M \text{ matching con } |E(M)| = |X| \iff |\Gamma(S)| \ge |S| \quad \forall S \subseteq X$

($\implies$) Sea $M$ un matching con $|E(M)|=|X|$. Entonces existe una función inyectiva

$$
f:X\to Y,\qquad f(x)\in\Gamma(x)\quad\text{para todo }x\in X.
$$
$\therefore |f(S)| = |S| \quad \forall S \subseteq X \text{ y } f(S) \subseteq \Gamma(S)$
$\implies |\Gamma(S)| \ge |f(S)| = |S| \quad \forall S \subseteq X$ $\blacksquare$

($\impliedby$) suponemos la condición de hall $|\Gamma(S)| \ge |S| \quad \forall S \subseteq X$ verdadera.
Si al correr el algoritmo p/hallar matching maximal nos quedamos con un $M_{max}$ tal que $|E(M_{max})| < |X|$
Utilizaré la forma matricial del algoritmo p/construir un $S \subseteq X$ que viole la condición de hall.
Corremos el algoritmo sobre el último matching $M_{max}$ el cual no cubre a $X$.
$\implies \exists \text{ filas sin "matchear" y otras filas etiquetadas. definimos:}$
$S = \{\text{Filas etiquetadas}\}$
$T = \{\text{Columnas etiquetadas}\}$
$S_0 = \{\text{Filas etiquetadas con } * \} \neq \emptyset \text{ pues } |E(M_{max})| < |X|$
$T_1 = \{\text{Cols. etiquetadas por filas de } S_0\}$
$S_1 = \{\text{Filas etiquetadas por columnas de } T_1\}$
$\vdots$
$T_{j+1} = \{\text{Columnas etiquetadas por } S_j\}$
$S_j = \{\text{Filas etiquetadas por } T_j\}$

Es claro que
$S = S_0 \cup S_1 \cup \dots \cup S_k$
$T = T_1 \cup T_2 \cup \dots \cup T_k$
Y en ambas la unión es disjunta [1]
Pues el algoritmo se detiene al pasar de un $S_k$ a un $T_{k+1} = \emptyset$ y nunca al pasar de un $T_j$ a un $S_j$ pues al revisar columnas de $T_j$, o están libres y se extiende el matching (lo cual no ocurre por hip) o encuentran una fila y la etiquetan $\therefore S_j \neq \emptyset$, más aún, cada columna de $T_j$ etiqueta únicamente a la fila con la que está matcheada,
Por lo tanto $|S_j| = |T_j| \quad \forall j=1 \dots k$ [2]

Ahora,
$|S| \stackrel{[1]}{=} |S_0| + |S_1| + \dots + |S_k|$
$\stackrel{[2]}{=} |S_0| + |T_1| + \dots + |T_k|$
$\stackrel{[1]}{=} |S_0| + |T| > |T| \text{ pues } S_0 \neq \emptyset$

Veamos que $T = \Gamma(S)$
$T \subseteq \Gamma(S)$ pues las filas de $S$ sólo etiquetan columnas vecinas.
Sea $y \in \Gamma(S)$, sup $y \notin T$, entonces $y$ no está etiquetado
$\implies \exists x \in S \text{ tq } y \in \Gamma(x)$, pero al escanear $x$ debería haber etiquetado a $y$. Absurdo
$\therefore y \in T$
Y así $T = \Gamma(S) \implies |S| > |\Gamma(S)|$ viola la condición de hall la cual es un absurdo que viene de suponer que $|E(M_{max})| < |X|$ $\blacksquare$

---

\newpage

## 9. Teorema del matrimonio de König

Todo grafo bipartito regular tiene un matching perfecto

**Demostración.**

Sea $G = (V, E)$ un grafo bipartito regular con partes $X$ e $Y$.
Para $W \subseteq V$ definimos $E_W = \{\hat{w}v \in E : \hat{w} \in W\}$

Sea $S \subseteq X$ un subconjunto, sea $\hat{x} \in E_S$ un lado
$\implies \exists x \in S, y \in Y \text{ tq } \hat{x} = xy \implies \hat{x} \in E_{\Gamma(S)}$
$\therefore y \in \Gamma(x) \implies y \in \Gamma(S)$
Esto significa que $E_S \subseteq E_{\Gamma(S)}$ y en particular, $|E_S| \le |E_{\Gamma(S)}|$ [1]

Veamos $|E_W|$ para $W \subseteq X$ ó $W \subseteq Y$
Si $\hat{w}v \in E_W, v \notin W$ pues $W \subseteq X \implies v \in Y$ y $W \subseteq Y \implies v \in X$
$\implies E_W = \bigcup_{\hat{w} \in W} \{\hat{w}v, v \in \Gamma(\hat{w})\}$ y la unión es disjunta.
$\implies |E_W| = \sum_{\hat{w} \in W} d(\hat{w}) \stackrel{\text{Regular}}{=} \Delta \cdot |W|$ [2]

Luego por [1] y [2]
$\Delta \cdot |S| \le \Delta \cdot |\Gamma(S)| \implies |S| \le |\Gamma(S)| \quad \forall S \subseteq X$
$\therefore$ por el teorema de hall existe un matching completo de $X$ a $Y$.
$\implies |X| \le |Y|$, pero la elección de utilizar $X$ sobre $Y$ fue arbitraria, por lo tanto también vale para $Y$, $|Y| \le |X|$
$\therefore |X| = |Y|$ y el matching completo es perfecto $\blacksquare$

---

\newpage

## 10. Teorema de la cota de Hamming

Sea $C \subseteq \{0,1\}^n$ un código binario de longitud $n$ con $\delta = \delta(C)$ y $t = \lfloor \frac{\delta - 1}{2} \rfloor$
Entonces $|C| \le \frac{2^n}{\sum_{i=0}^t \binom{n}{i}}$

**Demostración.**

Sea $A = \bigcup_{v \in C} D_t(v)$ donde $D_t(v)$ es el disco de radio $t$ alrededor de $v$
Como $t = \lfloor \frac{\delta - 1}{2} \rfloor$, $C$ corrige $t$ errores $\implies D_t(v) \cap D_t(w) = \emptyset \quad \forall v, w, v \neq w \in C$
$\therefore A \text{ es una unión disjunta } \implies |A| = \sum_{v \in C} |D_t(v)|$ [1]

Sea $S_r(v) = \{w \in \{0,1\}^n : d_H(v, w) = r\} \implies D_t(v) = \bigcup_{r=0}^t S_r(v)$ y la unión es disjunta pues $d_H(v, w) = r \implies d_H(v, w) \neq r' \quad \forall r' \neq r$
Luego $|D_t(v)| = \sum_{r=0}^t |S_r(v)|$ [2]
Notar que $w \in S_r(v) \iff w \text{ difiere en } r \text{ bits de } v$
$\therefore$ hay una biyección entre $S_r(v)$ y el conjunto de subconjuntos de $r$ bits en los que las palabras de $S_r(v)$ difieren de $v$.
Cada palabra a distancia $r$ se determina eligiendo las $r$ posiciones en las que difiere de $v$. Por lo tanto,

$$
|S_r(v)|=\binom nr.
$$
$\stackrel{[2]}{\implies} |D_t(v)| = \sum_{r=0}^t \binom{n}{r}$

Ahora, por [1]
$$
\begin{aligned}
|A|&=\sum_{v\in C}|D_t(v)|\\
&=\sum_{v\in C}\left(\sum_{r=0}^t\binom nr\right)\\
&=|C|\sum_{r=0}^t\binom nr.
\end{aligned}
$$
Ahora bien, como $A \subseteq \{0,1\}^n \implies |A| \le 2^n$


$$
|C|\sum_{r=0}^t\binom nr\le 2^n
\quad\Longrightarrow\quad
|C|\le\frac{2^n}{\displaystyle\sum_{r=0}^t\binom nr}.
$$

El denominador es positivo para $n,t\ge0$.

$\blacksquare$

---

\newpage

## 11. Matriz de chequeo

Si $H$ es matriz de chequeo de $C$, entonces
$\delta(C) = \min \{j : \exists \text{ un conjunto de } j \text{ columnas ld en } H\}$

**Demostración.**

Sea $\delta = \delta(C)$ y $M = \min \{j : \exists \text{ un conjunto de } j \text{ columnas ld en } H\}$
Sea $H^{(k)}$ la k-ésima columna de $H$.
$M \le \delta$
Como $C$ es lineal, $\delta = \min \{|x| : x \in C, x \neq 0\}$
Sea $x \in C, x \neq 0 \text{ con } |x| = \delta$ (existe porque es un mínimo)
$|x| = \delta \implies x \text{ tiene } \delta \text{ unos ("1"s) por lo tanto }$
$x = e_{i_1} + \dots + e_{i_\delta}$ donde $i_1, \dots, i_\delta$ son las posiciones de los "1"s en $x$.

Luego como $x \in C$ y $C = Nu(H)$, $Hx^t = 0$
$$
\begin{aligned}
0=Hx^t
&=H(e_{i_1}+\dots+e_{i_\delta})^t\\
&=He_{i_1}^t+\dots+He_{i_\delta}^t\\
&=H^{(i_1)}+\dots+H^{(i_\delta)}.
\end{aligned}
$$
$\therefore \{H^{(i_1)}, \dots, H^{(i_\delta)}\} \text{ son } \delta \text{ columnas ld de } H$
$\implies M \le \delta$

$\delta \le M$
Sean $H^{(j_1)}, \dots, H^{(j_M)}$ $M$ columnas ld en $H$, como $M$ es el mínimo,
$0 = H^{(j_1)} + \dots + H^{(j_M)} = H(e_{j_1})^t + \dots + H(e_{j_M})^t = H(e_{j_1} + \dots + e_{j_M})^t$
$\implies \underbrace{(e_{j_1} + \dots + e_{j_M})}_{\neq 0} \in Nu(H) = C$ y $|e_{j_1} + \dots + e_{j_M}| = M \ge \delta$ (pues $\delta = \min \{|x| : x \in C, x \neq 0\}$)

Finalmente, $\delta = M$ $\blacksquare$

---

\newpage

## 12. Código cíclico

Sea $C$ un código cíclico de dimensión $k$ y longitud $n$ y sea $g(x)$ su polinomio generador. Probar que:
I) $C = \{p(x) : gr(p) < n \text{ y } g(x) | p(x)\}$
Ii) $C = \{v(x) \odot g(x) : v \text{ es un polinomio cualquiera}\}$
Iii) $gr(g) = n - k$
Iv) $g(x)$ divide a $1 + x^n$

**Demostración.**

Sean $C_1 = \{p(x) : gr(p) < n \text{ y } g(x) | p(x)\}$
$C_2 = \{v(x) \odot g(x) : v \text{ es un polinomio cualquiera}\}$
Para probar i) y ii), veremos que $C_1 \subseteq C_2 \subseteq C \subseteq C_1$ y $\therefore C_1 = C = C_2$

$C_1 \subseteq C_2$
$p(x) \in C_1 \implies gr(p) < n \text{ y } g(x) | p(x) \implies \exists q(x) \text{ tq } p(x) = q(x)g(x)$
$\implies p(x) \bmod (1+x^n) = p(x) \quad p(x) \bmod (1+x^n) = q(x)g(x) \bmod (1+x^n)$
$p(x) = q(x) \odot g(x) \in C_2$

$C_2 \subseteq C$
Sea $p(x) = g(x) \odot v(x)$ para algún polinomio $v(x)$ ($p(x) \in C_2$)
Luego $v(x) = v_0 + v_1 x + \dots + v_d x^d$ para algún $d$.
$\implies p(x) = g(x) \odot v(x) = v(x) \odot g(x) = (v_0 + v_1 x + \dots + v_d x^d) \odot g(x)$
$= v_0 \odot g(x) + v_1 x \odot g(x) + \dots + v_d x^d \odot g(x)$
$= v_0 g(x) + v_1 Rot(g(x)) + \dots + v_d Rot^d(g(x)) \in C$ pues es una combinación lineal de palabras en c.

$C \subseteq C_1$
$p(x) \in C \implies gr(p) < n$
Sabemos que $\exists q(x) \text{ y } r(x) \text{ tq } p(x) = g(x) \cdot q(x) + r(x) \text{, con } gr(r) < gr(g)$
Ahora, $p(x) \bmod (1+x^n) = (g(x) \cdot q(x) + r(x)) \bmod (1+x^n)$
$p(x) = g(x) \odot q(x) + (r(x) \bmod (1+x^n)) \text{, } gr(r) < gr(g) < n \implies r \bmod 1+x^n = r$
$p(x) = g(x) \odot q(x) + r(x)$
$C \ni p(x), C \ni g(x) \odot q(x) \implies C \ni r(x)$
Como $r(x)\in C$ y $gr(r)<gr(g)$, se sigue que $r(x)=0$. Por lo tanto,

$$
g(x)\mid p(x),\qquad gr(p)<n,\qquad p(x)\in C_1.
$$

Iii) vimos ya que $C = C_1$
$\therefore p(x) \in C \iff gr(p) < n \text{ y } \exists q(x) \text{ tq } p(x) = q(x)g(x)$
$gr(p) < n \implies gr(q \cdot g) < n \implies gr(q) + gr(g) < n$
$\implies gr(q) < n - gr(g)$
Si tomo cualquier $q(x)$ con $gr(q) < n - gr(g) \implies gr(q \cdot g) < n \text{ y } q \cdot g \in C$
$\therefore \exists \text{ una biyección entre } C \text{ y los polinomios de grado menor a } n - gr(g)$
Como $C$ es lineal,
$|C| = 2^k$
Por la biyección
$|C| = |\{\text{Conjunto de polinomios de grado menor a } n - gr(g)\}| = 2^{n - gr(g)}$
$\therefore k = n - gr(g) \text{ ó } gr(g) = n - k$

Iv) Por el algoritmo de la división, existen $q(x)$ y $r(x)$ tales que

$$
1+x^n=g(x)q(x)+r(x),\qquad gr(r)<gr(g).
$$
$(1+x^n) \bmod (1+x^n) = 0 = (g(x)q(x) + r(x)) \bmod (1+x^n)$
$0 = g(x) \odot q(x) + r(x) \bmod (1+x^n)$
$r(x) = g(x) \odot q(x) \in C$ y como $gr(r) < gr(g) \implies r(x) = 0 \text{ y } \therefore g(x) | (1+x^n)$

---

\newpage

## 13. 3-SAT es NP-completo

**Demostración.** Veremos que CNF-SAT $\le_P$ 3-SAT
Sea $B$ una instancia de CNF-SAT con variables $x_1, \dots, x_n$
$B = D_1 \land \dots \land D_M \quad \text{Con} \quad D_j = \ell_{j_1} \lor \dots \lor \ell_{j_{\Gamma_j}}$ donde $\ell_{j_k}$ son literales (variables o sus negaciones)
Queremos un algoritmo polinomial $A$ tq $A(B)$ esté en el dominio de 3-SAT, y que
La transformación debe preservar la satisfacibilidad. Para toda asignación $\vec{b}$,

$$
B(\vec{b})=1
\quad\Longleftrightarrow\quad
D_j(\vec{b})=1\quad\text{para todo }j.
$$
Para ello, $A(B) = E_1 \land \dots \land E_M$ donde $E_j$ serán conjunciones de disjunciones de 3 literales.
Por lo tanto $A$ transformará $D_j \to E_j$ de la siguiente manera:

- Si $\Gamma_j=3$, entonces $E_j=D_j$.
- Si $\Gamma_j\ge 4$:

  $$
  \begin{aligned}
  E_j={}&(\ell_{j_1}\lor\ell_{j_2}\lor y_{j_1})
  \land(\bar y_{j_1}\lor\ell_{j_3}\lor y_{j_2})\\
  &\land(\bar y_{j_2}\lor\ell_{j_4}\lor y_{j_3})\land\dots\\
  &\land(\bar y_{j_{\Gamma_j-4}}\lor\ell_{j_{\Gamma_j-2}}\lor y_{j_{\Gamma_j-3}})\\
  &\land(\bar y_{j_{\Gamma_j-3}}\lor\ell_{j_{\Gamma_j-1}}\lor\ell_{j_{\Gamma_j}}).
  \end{aligned}
  $$

- Si $\Gamma_j=1$:

  $$
  \begin{aligned}
  E_j={}&(\ell_{j_1}\lor y_{j_1}\lor y_{j_2})
  \land(\ell_{j_1}\lor y_{j_1}\lor\bar y_{j_2})\\
  &\land(\ell_{j_1}\lor\bar y_{j_1}\lor y_{j_2})
  \land(\ell_{j_1}\lor\bar y_{j_1}\lor\bar y_{j_2}).
  \end{aligned}
  $$

- Si $\Gamma_j=2$:

  $$
  E_j=(\ell_{j_1}\lor\ell_{j_2}\lor y_j)
  \land(\ell_{j_1}\lor\ell_{j_2}\lor\bar y_j).
  $$

Es claro que $D_j(\vec{b}) = 1 \iff \exists \vec{y} \text{ tq } E_j(\vec{b}, \vec{y}) = 1$ para los casos de $\Gamma_j = 1, 2, 3$ (asignaciones de los $y_j$)
($\impliedby \text{ para } \Gamma_j \ge 4$) supongamos que no se cumple, i.e $\exists \vec{b}, \vec{y} \text{ tq } E_j(\vec{b}, \vec{y}) = 1 \text{ y } D_j(\vec{b}) = 0$
$$
D_j(\vec{b})=0
\quad\Longrightarrow\quad
\ell_{j_k}(\vec{b})=0\quad\text{para todo }k.
$$
$$
\begin{aligned}
E_j(\vec{b},\vec{y})={}&(0\lor0\lor y_{j_1})
\land(\bar y_{j_1}\lor0\lor y_{j_2})\land\dots\\
&\land(\bar y_{j_{\Gamma_j-4}}\lor0\lor y_{j_{\Gamma_j-3}})
\land(\bar y_{j_{\Gamma_j-3}}\lor0\lor0)\\
={}&y_{j_1}\land(\bar y_{j_1}\implies y_{j_2})\land\dots\\
&\land(\bar y_{j_{\Gamma_j-4}}\implies y_{j_{\Gamma_j-3}})
\land\bar y_{j_{\Gamma_j-3}}.
\end{aligned}
$$
$= 0 \quad \forall \vec{y}$ absurdo pues $E_j(\vec{b}, \vec{y}) = 1$

($\implies \text{ para } \Gamma_j \ge 4$)
Supongamos $D_j(\vec{b}) = 1$, entonces $\exists k \text{ tq } \ell_{j_k}(\vec{b}) = 1$
Definimos $\vec{y} = (d_1, \dots, d_{\Gamma_j-3})$ tal que $d_1 = \dots = d_{k-2} = 1$  y $d_{k-1} = d_k = \dots = d_{\Gamma_j-3} = 0$
$y_{j_t}(\vec{y}) = d_t$
Luego
$$
\begin{aligned}
E_j(\vec{b},\vec{y})={}&\underbrace{(\ell_{j_1}\lor\ell_{j_2}\lor y_{j_1})(\vec{b},\vec{y})}_{=1}\\
&\land\underbrace{(\bar y_{j_1}\lor\ell_{j_3}\lor y_{j_2})(\vec{b},\vec{y})}_{=1}\land\dots\\
&\land\underbrace{(\bar y_{j_{k-3}}\lor\dots\lor y_{j_{k-2}}\lor\ell_{j_{k-1}})(\vec{b},\vec{y})}_{=1}\\
&\land\underbrace{(\bar y_{j_{k-2}}^{0}\lor\ell_{j_k}^{1}\lor y_{j_{k-1}}^{0})(\vec{b},\vec{y})}_{=1}\land\dots\\
&\land\underbrace{(\bar y_{j_{k-1}}^{1}\lor\dots)(\vec{b},\vec{y})}_{=1}\land\dots
\land\underbrace{(\bar y_{j_{\Gamma_j-3}}^{1}\lor\dots)(\vec{b},\vec{y})}_{=1}=1.
\end{aligned}
$$

Los términos restantes valen 1 porque contienen algún $\bar d_q$, con $k-2\le q\le\Gamma_j-3$.

Así queda demostrado que $D_j(\vec{b}) = 1 \iff \exists \vec{y} \text{ tq } E_j(\vec{b}, \vec{y}) = 1 \quad \forall j$
$\therefore B \text{ es satisfacible } \iff A(B) \text{ es satisfacible}$ $\blacksquare$

---

\newpage

## 14. 3-COLOR es NP-completo

**Demostración.** Veremos que 3-SAT $\le_P$ 3-COLOR en tiempo polinomial
Dominio = "conjunto de instancias"
Para ello, debemos transformar un elemento $B$ del dominio de 3-SAT a un elemento $A(B) = G$ del dominio de 3-COLOR, y mostrar que $B \text{ es satisfacible } \iff \chi(G) \le 3$.
Sabemos que $B$ es de la forma $D_1 \land \dots \land D_M$ donde $D_j = (\ell_{j_1} \lor \ell_{j_2} \lor \ell_{j_3})$, con $\ell_{j_k} = x_{\text{Algo}} \text{ ó } \bar{x}_{\text{Algo}}$
Luego $A$ transforma $B$ en $G$, en tiempo polinomial, de la siguiente forma:
Vértices de $G$
$$
\begin{aligned}
V(G)={}&\{v_1,\dots,v_n,w_1,\dots,w_n\}\\
&\cup\bigcup_{j=1}^M\{a_{j_1},a_{j_2},a_{j_3},e_{j_1},e_{j_2},e_{j_3}\}\\
&\cup\{t,\text{Capitán}\}.
\end{aligned}
$$
Para las variables de b $x_1, \dots, x_n$

Lados de $G$
Triángulos $\{(t, v_i), (t, w_i), (v_i, w_i)\}$ para $i=1 \dots n$
Triángulos $\{(a_{j_1}, a_{j_2}), (a_{j_2}, a_{j_3}), (a_{j_3}, a_{j_1})\}$ para $j=1 \dots M$ (garras)
Lados $\{(a_{j_r}, e_{j_r})\}$ para $j=1 \dots M$ y $r=1, 2, 3$
Lados $\{(\text{Capitán}, e_{j_k})\}$ para $j=1 \dots M$ y $k=1, 2, 3$
Lado $(\text{Capitán}, t)$

Para un literal $\ell$, definimos el vértice correspondiente a él como
$$
v(\ell)=
\begin{cases}
v_k & \text{si }\ell=x_k,\\
w_k & \text{si }\ell=\bar{x}_k.
\end{cases}
$$

Finalmente, agregamos los lados
$\{(e_{j_r}, v(\ell_{j_r}))\}$ para $j=1 \dots M$ y $r=1, 2, 3$

Es claro que la construcción de $G$ es en tiempo polinomial en el tamaño de $B$.

Ahora debemos probar
$B \text{ satisfacible } \iff \chi(G) \le 3 \quad (*)$
La fórmula $B$ es satisfacible si existe una asignación $\vec{b}$ tal que

$$
B(\vec{b})=1
\quad\Longleftrightarrow\quad
D_j(\vec{b})=1\quad\text{para todo }j\in\{1,\dots,M\}.
$$

($\impliedby$) $\chi(G) \le 3$ donde $G=A(B)$. $V = \text{"vértices de } G\text{", } E = \text{"lados de } G\text{"}$
Como $G$ tiene triángulos ($K_3$) de subgrafos, $\chi(G) \ge 3 \therefore \chi(G) = 3$
Por lo tanto, existe un coloreo propio $c$ de $G$ con 3 colores; $c(v)$ denota el color de $v$.
Como $(t, \text{Capitán}) \in E \implies c(t) \neq c(\text{Capitán})$
$\therefore C \text{ utiliza los colores } \{c(t), c(\text{Capitán}), c_3\}$
Definimos $\vec{b}=(b_1,\dots,b_n)$ mediante

$$
b_i=
\begin{cases}
1 & \text{si }c(v_i)=c(\text{Capitán}),\\
0 & \text{si no.}
\end{cases}
$$

Probaremos que $B(\vec{b}) = 1$ i.e $B$ es satisfacible
Para ello, hay que mostrar que $D_j(\vec{b}) = 1 \quad \forall j=1 \dots M$, y para ello, hay que mostrar que
$\forall j=1 \dots M, \exists r \text{ tq } \ell_{j_r}(\vec{b}) = 1$
Sea $j \in \{1 \dots M\}$, como los "$a_{j_r}$"'s forman triángulos que utilizan los 3 colores,
$\exists r \text{ tq } c(a_{j_r}) = c(t)$ [1]
$(e_{j_r}, a_{j_r}) \in E \implies c(e_{j_r}) \neq c(a_{j_r}) = c(t)$
$(e_{j_r}, \text{Capitán}) \in E \implies c(e_{j_r}) \neq c(\text{Capitán})$ $\implies c(e_{j_r}) = c_3$ [2]
$(e_{j_r}, v(\ell_{j_r})) \in E \text{ y } [2] \implies c(v(\ell_{j_r})) \neq c_3$
Como $v(\ell_{j_r})$ es $v_k$ o $w_k$, existe un lado que lo une con $t$. Por lo tanto,

$$
c(v(\ell_{j_r}))\neq c(t).
$$
Por ende $c(v(\ell_{j_r})) = c(\text{Capitán})$

Veamos que ocurre con $\ell_{j_r}$ para los casos $\ell_{j_r} = \text{Variable y } \ell_{j_r} = \text{Variable nega.}$
Si $\ell_{j_r}$ es una variable, $\exists k \text{ tq } \ell_{j_r} = x_k \implies \ell_{j_r}(\vec{b}) = x_k(\vec{b}) = b_k$


$$
\begin{aligned}
\implies v(\ell_{j_r}) = v_k \therefore c(v_k) = c(v(\ell_{j_r})) = c(\text{Capitán})\\
&\implies b_k = 1\\
&\implies \ell_{j_r}(\vec{b}) = 1
\end{aligned}
$$


Si $\ell_{j_r}$ es una negación de variable, $\exists k \text{ tq } \ell_{j_r} = \bar{x}_k \implies \ell_{j_r}(\vec{b}) = \bar{x}_k(\vec{b}) = 1 - b_k$
$\implies v(\ell_{j_r}) = w_k \therefore c(w_k) = c(v(\ell_{j_r})) = c(\text{Capitán})$
$v_k w_k \in E \implies c(v_k) \neq c(\text{Capitán}) \implies b_k = 0 \implies \ell_{j_r}(\vec{b}) = 1$
Por lo tanto $\forall j=1 \dots M, \exists r \text{ tq } \ell_{j_r}(\vec{b}) = 1 \therefore B(\vec{b}) = 1 \implies B \text{ es satisfacible}$

($\implies$) $B \text{ satisfacible } \implies \exists \vec{b} \text{ tq } B(\vec{b}) = 1$
Daré un coloreo $c$ propio de $G=A(B)$ con 3 colores y por lo tanto $\chi(G) \le 3$.


$$
\begin{aligned}
 c(\text{Capitán}) = \text{Escarlata}, \quad c(t) = \text{Verde}\\
&\implies \text{El lado } (t, \text{Capitán}) \text{ mantiene lo propio del coloreo.}
\end{aligned}
$$


A partir de algún $\vec{b} \text{ tq } B(\vec{b}) = 1$, definimos (con $\vec{b} = (b_1, \dots, b_n)$)
$$
\begin{aligned}
c(v_i)&=\begin{cases}
\text{Escarlata}&\text{si }b_i=1,\\
\text{Negro}&\text{si }b_i=0,
\end{cases}\\[4pt]
c(w_i)&=\begin{cases}
\text{Negro}&\text{si }b_i=1,\\
\text{Escarlata}&\text{si }b_i=0.
\end{cases}
\end{aligned}
$$
De esta forma, los triángulos $\{t, v_i, w_i\}$ para $i=1 \dots n$ mantienen lo propio del coloreo pues utilizan los 3 colores.

Como $B(\vec{b})=1$, cada cláusula tiene al menos un literal verdadero. Para cada $j$, elijo un índice $k_j$ tal que

$$
\ell_{j_{k_j}}(\vec{b})=1.
$$
Y definimos
$c(a_{j_{k_j}}) = \text{Verde}$
$c(a_{j_r}) = \text{Uno negro y otro escarlata} \quad r \neq k_j$
Los triángulos $\{a_{j_1}, a_{j_2}, a_{j_3}\}$ para $j=1 \dots M$ mantienen lo propio del coloreo. (usan los 3 colores)

Coloreamos
$c(e_{j_{k_j}}) = \text{Negro} \quad \text{Y} \quad c(e_{j_r}) = \text{Verde} \text{ para } r \neq k_j$
Luego los siguientes lados mantienen lo propio del coloreo:
Para todos los índices $j$ y $q\in\{1,2,3\}$, el lado entre $e_{j_q}$ y Capitán conserva colores distintos: sus colores son Negro o Verde, y Escarlata, respectivamente.

Si $r\neq k_j$, $e_{j_r}$ es Verde, mientras que $a_{j_r}$ es Negro o Escarlata; también son distintos.

Para $r=k_j$, $e_{j_{k_j}}$ es Negro y $a_{j_{k_j}}$ es Verde.
Para $r\neq k_j$, $c(e_{j_r})=\text{Verde}$, mientras que $v(\ell_{j_r})$ tiene uno de estos dos colores:

$$
c(v(\ell_{j_r}))\in\{\text{Negro},\text{Escarlata}\}.
$$

Sólo queda por ver el lado $e_{j_{k_j}}, v(\ell_{j_{k_j}})$.
Si $\ell_{j_{k_j}}=x_i$, entonces $v(\ell_{j_{k_j}})=v_i$. Como el literal es verdadero, se obtiene

$$
\ell_{j_{k_j}}(\vec{b})=x_i(\vec{b})=b_i=1
\quad\Longrightarrow\quad
c(v_i)=\text{Escarlata}.
$$
Como $c(v(\ell_{j_{k_j}}))=c(v_i)=\text{Escarlata}$ y $c(e_{j_{k_j}})=\text{Negro}$, los extremos del lado tienen colores distintos.
Si $\ell_{j_{k_j}}=\bar{x}_i$, su valor verdadero implica

$$
\ell_{j_{k_j}}(\vec{b})=\bar{x}_i(\vec{b})=1-b_i=1
\quad\Longrightarrow\quad
b_i=0.
$$
$\therefore v(\ell_{j_{k_j}}) = w_i$ y $c(w_i) = \text{Escarlata}$ y $c(e_{j_{k_j}}) = \text{Negro} \implies c(w_i) = \text{Escarlata}$
Por lo que el lado $e_{j_{k_j}}, v(\ell_{j_{k_j}})$ mantiene lo propio del coloreo.
Finalmente,

$$
\begin{aligned}
 B \text{ satisfacible }\\
&\implies \exists \text{ coloreo } c \text{ propio de } A(B)=G \text{ con 3 colores }\\
&\implies \chi(G) \le 3
\end{aligned}
$$

$\blacksquare$

---

\newpage

## 15. Teorema del matrimonio 3D

Matrimonio 3D es NP-completo

**Demostración.**

Mostraremos que 3-SAT $\le_P$ matrimonio 3D
Esto es, a partir de una instancia $B = D_1 \land \dots \land D_M$ con variables $x_1, \dots, x_n$
Y $D_j = \ell_{j_1} \lor \ell_{j_2} \lor \ell_{j_3}$ con $\ell_{j_k}$ variables o negaciones de variables,
Construir en tiempo polinomial sobre el tamaño de $B$ un 3-hipergrafo $H$ de
Manera tal que $B \text{ es satisfacible } \iff H \text{ tiene un matching perfecto}$
Notar que para que $H$ sea una instancia de matrimonio 3D, sus 3 partes $X, Y, Z$ deben tener la misma cardinalidad. Construcción de $H$ en tiempo polinomial:
Notación: $j \in \{1 \dots M\}, i \in \{1 \dots n\}, r \in \{1, 2, 3\}, k \in \{1 \dots M(n-1)\}$
Vértices de $H$, en 3 partes disjuntas de igual cardinalidad:
$X = \{a_{ij}\}_{i, j} \cup \{s_j\}_j \cup \{g_k\}_k \qquad |X| = nM + M + M(n-1) = 2nM$
$Y = \{b_{ij}\}_{i, j} \cup \{t_j\}_j \cup \{h_k\}_k \qquad |Y| = nM + M + M(n-1) = 2nM = |X|$
$Z = \{u_{ij}\}_{i, j} \cup \{w_{ij}\}_{i, j} \qquad |Z| = nM + nM = 2nM = |X| = |Y|$
Lados de $H : E(H) = E_0 \cup E_1 \cup E_2 \cup E_3$
$$
\begin{aligned}
E_0&=\bigl\{\{a_{ij},b_{ij},u_{ij}\}\bigr\}_{i,j},\\
E_1&=\bigl\{\{a_{i(j+1)},b_{ij},w_{ij}\}\bigr\}_{i,j},\\
&\text{con }j+1=1\text{ cuando }j=M.
\end{aligned}
$$
$E_2 = \{\{g_k, h_k, z\} : z \in Z\}_k$
Se asocia a cada literal el vértice

$$
v_{jr}=
\begin{cases}
u_{ij} & \text{si }\ell_{jr}=x_i,\\
w_{ij} & \text{si }\ell_{jr}=\bar{x}_i.
\end{cases}
$$

Con ellos, se define

$$
E_3=\bigl\{\{s_j,t_j,v_{jr}\}\bigr\}_{j,r}.
$$
Es claro que la construcción de $H$ es en tiempo polinomial dependiente de $n$ y $M$ (tamaño de b)
Ahora debemos mostrar que $B \text{ es satisfacible } \iff \exists \text{ un matching perfecto en } H$

($\implies$) $B \text{ satisfacible, sea } \vec{b} \text{ tq } B(\vec{b}) = 1$, y $\vec{b} = (b_1, \dots, b_n)$ asignaciones de variables.
Construimos a partir de $B$ un matching $M$ en $H$ y mostramos que es perfecto.
Lados de $M : F_0 \cup F_1 \cup F_2 \cup F_3$
$$
\begin{aligned}
F_0&=\bigl\{\{a_{ij},b_{ij},u_{ij}\}:j\in\{1,\dots,M\},\ b_i=0\bigr\}\subseteq E_0,\\
F_1&=\bigl\{\{a_{i(j+1)},b_{ij},w_{ij}\}:j\in\{1,\dots,M\},\ b_i=1\bigr\}\subseteq E_1,
\end{aligned}
$$
con $j+1=1$ cuando $j=M$.
Estos lados son disjuntos pues los $b_i$ no pueden ser $0$ y $1$ a la vez.


$$
\begin{aligned}
 B(\vec{b}) = 1\\
&\implies D_j(\vec{b}) = 1 \quad \forall j=1 \dots M\\
&\implies \forall j=1 \dots M, \exists r_j \text{ tq } \ell_{jr_j}(\vec{b}) = 1 \text{ (si hay más de 1, elijo 1)}
\end{aligned}
$$


Luego definimos
$F_3 = \{\{s_j, t_j, v_{jr_j}\}\}_j$, como elegimos un solo $r_j$ p/c $j$, los lados de $F_3$ son disjuntos entre sí
Supongamos que los lados de $F_3$ no son disjuntos con los lados de $F_1$ o $F_0$


$$
\begin{aligned}
\implies \exists L_3 \in F_3 \text{ tq } \exists L_0 \in F_0 \text{ tq } L_3 \cap L_0 \neq \emptyset \text{ ó } \exists L_1 \in F_1 \text{ tq } L_1 \cap L_3 \neq \emptyset
\end{aligned}
$$


Notar que esta intersección sólo podría contener un vert. $v_{jr_j}$, pues $\forall L \in F_0, F_1, \quad s_j \notin L, t_j \notin L$

**Caso:**


$$
\begin{aligned}
 L_0 = \{a_{ij}, b_{ij}, u_{ij}\} \in F_0\\
&\implies v_{jr_j} = u_{ij}\\
&\implies \ell_{jr_j} = x_i\\
&\implies 1 = \ell_{jr_j}(\vec{b}) = x_i(\vec{b}) = b_i = 0
\end{aligned}
$$

absurdo. (pues $b_i = 0$)

**Caso:**


$$
\begin{aligned}
 L_1 = \{a_{i(j+1)}, b_{ij}, w_{ij}\} \in F_1\\
&\implies v_{jr_j} = w_{ij}\\
&\implies \ell_{jr_j} = \bar{x}_i\\
&\implies 1 = \ell_{jr_j}(\vec{b}) = \bar{x}_i(\vec{b}) = 1 - b_i = 0
\end{aligned}
$$

absurdo (pues $b_i = 1$)
Luego los lados de $F_3$ son disjuntos con los de $F_1$ y $F_0$.
Sea $N = \{z \in Z : z \text{ no está cubierto por } F_0, F_1 \text{ ó } F_3\}$
Veamos $|Z - N| = |\{z \in Z : z \text{ está cubierto por } F_0, F_1 \text{ ó } F_3\}|$
- $F_3$ cubre $M$ elementos de $Z$ (un $v_{jr_j}$ por cada $j$)
Si $q = \#\{i : b_i = 1\}$ y $p = \#\{i : b_i = 0\}$, luego
- $F_0$ cubre $p \cdot M$ vértices $u_{ij} \in Z$ (lados $\{a_{ij}, b_{ij}, u_{ij}\} : b_i = 0$)
- $F_1$ cubre $q \cdot M$ vértices $w_{ij} \in Z$ (lados $\{a_{i(j+1)}, b_{ij}, w_{ij}\} : b_i = 1\}$)

Así, $|Z - N| = M + pM + qM = M(p+q+1) = M(n+1)$ (pues $p+q = n$)
Luego $|N| = |Z| - |Z - N|$
$= 2nM - M(n+1) = M(n-1)$
$\implies \exists \text{ biyección } f : \{1 \dots M(n-1)\} \to N$
Definimos
$F_2 = \{\{g_k, h_k, f(k)\}\}_k$
Disjuntos entre sí y como $f(k) \in N \implies \text{ no estaba cubierto por } F_0, F_1 \text{ ni } F_3$
$\therefore$ los lados de $F_2$ son disjuntos de los de $F_0, F_1 \text{ y } F_3 \text{ y son } M(n-1) \text{ (sobreyectiva)}$

Así, $|E(M)| = \underbrace{|F_0| + |F_1|}_{= nM} + \underbrace{|F_2|}_{= M(n-1)} + \underbrace{|F_3|}_{= M}$
$= nM + M(n-1) + M$
$= 2nM$ y el matching es perfecto.

($\impliedby$) sea $M$ un matching perfecto en $H$
Observar que los vérs. $a_{ij}$ y $b_{ij}$ sólo están en lados de $E_0$ ó $E_1$
Como $M$ es perfecto, deben estar también en algún lado de $M$.
Fijamos $i \in \{1 \dots n\}$
Supongamos que para un $j \in \{1 \dots M\}, \exists L \text{ lado } \in E(M) \cap E_0 \text{ tq } a_{ij} \in L$
$$
\begin{aligned}
\{a_{ij},b_{ij},u_{ij}\}&\in E_0\cap E(M),\\
&\text{así que }b_{ij}\text{ no puede pertenecer a otro lado de }M.
\end{aligned}
$$
$\therefore \{a_{i(j+1)}, b_{ij}, w_{ij}\} \notin E(M) \text{ pero } a_{i(j+1)} \text{ debe estar en } E(M)$
$\implies \{a_{i(j+1)}, b_{i(j+1)}, u_{i(j+1)}\} \in E(M)$
$$
\{a_{ij},b_{ij},u_{ij}\}\in E(M)\quad\text{para todo }j.
$$
Este es el caso 0 para el $i$ fijado.
Ahora si para un

$$
\begin{aligned}
 j \in \{1 \dots M\} \exists L \text{ lado } \in E(M) \cap E_1 \text{ tq } a_{ij} \in L\\
&\implies L = \{a_{ij}, b_{i(j-1)}, w_{i(j-1)}\}
\end{aligned}
$$


Pero como $b_{i(j-1)}$ no puede estar en otro lado de $M$, $\{a_{i(j-1)}, b_{i(j-1)}, u_{i(j-1)}\} \notin E(M) \implies$
$$
\{a_{i(j-1)},b_{i(j-2)},w_{i(j-2)}\}\in E(M)\cap E_1\quad\text{para todo }j.
$$
Este es el caso 1 para el $i$ fijado.

Es claro que siempre se da o el "caso 0" ó el "caso 1" para cada $i$, y a partir de ello,
Definimos
$b_i = \begin{cases} 1 & \text{Si se da "caso 1" para } i \\ 0 & \text{Si se da "caso 0" para } i \end{cases}$
$\vec{b} = (b_1 \dots b_n)$ y queremos ver que $D_j(\vec{b}) = 1 \quad \forall j \quad \text{ y } \therefore B \text{ es satisfacible}.$
Fijamos $j \in \{1 \dots M\}$
Como $M$ es perfecto, los vértices $s_j, t_j$ deben estar en algún lado de $M$
$\implies \exists r \text{ tq } \{s_j, t_j, v_{jr}\} \in E(M)$

Si $\ell_{jr}$ es una variable, $\exists i \text{ tq } \ell_{jr} = x_i \implies v_{jr} = u_{ij}$
Luego $\{s_j, t_j, u_{ij}\} \in E(M) \implies u_{ij} \text{ no está en otro lado de } M$
$\implies \{a_{ij}, b_{ij}, u_{ij}\} \notin E(M) \implies \text{"caso 1" para } i$
$\therefore b_i = 1 \implies \ell_{jr}(\vec{b}) = x_i(\vec{b}) = b_i = 1 \implies D_j(\vec{b}) = 1$

Si $\ell_{jr}$ es una negación de variable, $\exists i \text{ tq } \ell_{jr} = \bar{x}_i \implies v_{jr} = w_{ij}$
Luego $\{s_j, t_j, w_{ij}\} \in E(M) \implies w_{ij} \text{ no puede estar en otro lado de } M$
$\implies \{a_{i(j+1)}, b_{ij}, w_{ij}\} \notin E(M) \implies \text{"caso 0" para } i$
$\therefore b_i = 0 \implies \ell_{jr}(\vec{b}) = \bar{x}_i(\vec{b}) = 1 - b_i = 1 \implies D_j(\vec{b}) = 1$

Así, existe una asignación que satisface todas las cláusulas:

$$
D_j(\vec{b})=1\quad\text{para todo }j
\quad\Longrightarrow\quad
B(\vec{b})=1.
$$

$\blacksquare$
