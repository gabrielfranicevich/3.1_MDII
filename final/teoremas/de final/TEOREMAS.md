# TEOREMAS PARA EL FINAL JULIO 2024

¿CUÁL ES LA COMPLEJIDAD DEL ALGORITMO DE EDMONDS-KARP?

RTA: LA COMPLEJIDAD DE E-K ES $O(n m^2)$ DONDE $n=|V|$ Y $m=|E|$ SOBRE UNA NETWORK $N=(V, E, c)$.

DEM:
EDMONDS-KARP CONSTRUYE UNA SUCESIÓN DE CAMINOS AUMENTANTES UTILIZANDO BFS.

LA COMPLEJIDAD DE E-K ENTONCES ESTÁ DADA POR
$COMPL(E-K) = O(COMPL(BFS)) \cdot \#CAMINOS\_AUMENTANTES$
$= O(m \cdot \#CAMINOS\_AUMENTANTES)$

DEF: UN LADO "SE VUELVE" CRÍTICO EN UN PASO DADO SI ES USADO PARA CONSTRUIR EL SIGUIENTE FLUJO EN MODO FORWARDS Y SE SATURA O EN MODO BACKWARDS Y SE VACÍA.

¿CUÁNTAS VECES SE PUEDE VOLVER CRÍTICO UN LADO? PRIMERO NECESITAREMOS ALGUNAS DEFINICIONES.

DEF: DADO UN FLUJO $f$ EN $N$ Y VÉRTICES $x, z$,
$$ d_f(x, z) = \begin{cases} 0 & x = z \\ \infty & \text{NO EXISTE } f\text{-CAM-AUMENTANTE DE } x \text{ a } z \\ \text{MIN. LONGITUD DE } f\text{-CAM-AUMENTANTE DE } x \text{ a } z & \text{SI EXISTE} \end{cases} $$

Si $f_0, f_1, \dots$ SON LOS FLUJOS QUE SE VAN OBTENIENDO EN EK, DEFINIMOS:
$d_k(x) = d_{f_k}(s, x)$ , $b_k(x) = d_{f_k}(x, t)$

PROPOSICIÓN: $d_k(x) \le d_{k+1}(x)$ Y $b_k(x) \le b_{k+1}(x)$ $\forall x$ (NO HACE FALTA DEMOSTRARLA)

OBS: $d_k(t) = d_k(x) + b_k(x)$ SI AMBAS SON FINITAS.

¿CUÁNTAS VECES SE PUEDE VOLVER CRÍTICO UN LADO?
SEA $\vec{xy}$ UN LADO QUE SE VUELVE CRÍTICO EN EL PASO $k$. ESTO PUEDE DARSE DE DOS FORMAS:

**a)** $\vec{xy}$ SE SATURA EN EL PASO $k$
$\implies f_k(\vec{xy}) < c(\vec{xy}), f_{k+1}(\vec{xy}) = c(\vec{xy})$
$\implies$ SE UTILIZÓ UN CAMINO AUMENTANTE $s \dots x y \dots t$ DE LONGITUD MÍNIMA (EK)
$\therefore d_k(y) = d_k(x) + 1$

Si $\vec{xy}$ SE VUELVE CRÍTICO DE NUEVO EN ALGÚN PASO $j > k$, ENTONCES
- SE VACIÓ $\implies$ SE USÓ BACKWARDS EN EL PASO $i$
- SE SATURÓ $\implies$ SE USÓ BACKWARDS EN ALGÚN PASO $i$ CON $k \le i < j$

$\implies \exists i \text{ tq } k \le i \le j$ DONDE $\vec{xy}$ SE USA EN MODO BACKWARDS
$\implies$ SE UTILIZÓ UN CAMINO AUMENTANTE DE LONGITUD MÍNIMA DE LA FORMA
$s \dots y x \dots t \text{ tq } d_i(x) = d_i(y) + 1$

LUEGO
$d_j(t) \ge d_i(t) = d_i(x) + b_i(x)$
$= d_i(y) + 1 + b_i(x)$
$\ge d_k(y) + 1 + b_k(x)$
$= d_k(x) + 1 + 1 + b_k(x)$
$= d_k(t) + 2$

**b)** $\vec{xy}$ SE VACÍA EN EL PASO $k$
$\implies f_k(\vec{xy}) > 0 \text{ y } f_{k+1}(\vec{xy}) = 0$
$\implies$ SE UTILIZÓ UN CAMINO AUMENTANTE DE LONGITUD MÍNIMA (EK) DE LA FORMA $s \dots y \overleftarrow{x} \dots t$
$\therefore d_k(x) = d_k(y) + 1$

Si $\vec{xy}$ SE VUELVE CRÍTICO DE NUEVO EN UN PASO $j > k$, ENTONCES
- SE VACIÓ $\implies$ SE USÓ FORWARDS EN ALGÚN PASO $i$ CON $k \le i \le j$
- SE SATURÓ $\implies$ SE USÓ FORWARDS EN EL PASO $j$.

$\therefore$ SE USÓ FORWARDS EN ALGÚN PASO $i$ CON $k \le i \le j$ CON UN CAMINO AUMENTANTE DE LONGITUD MÍNIMA DE LA FORMA
$s \dots \vec{xy} \dots t \text{ DE MANERA TAL QUE }$
$d_i(y) = d_i(x) + 1$

$d_j(t) \ge d_i(t) = d_i(y) + b_i(y)$
$= d_i(x) + 1 + b_i(y)$
$\ge d_k(x) + 1 + b_k(y)$
$= d_k(y) + 1 + 1 + b_k(y)$
$= d_k(t) + 2$

EN AMBOS CASOS (a y b), LA DISTANCIA ENTRE $s$ Y $t$ DEBE AUMENTAR EN AL MENOS 2 PARA QUE UN LADO PUEDA VOLVER A SER CRÍTICO. COMO ESTA DISTANCIA VA ENTRE $0$ Y $n-1$, UN LADO PUEDE VOLVERSE CRÍTICO $O(n)$ VECES A LO SUMO.

FINALMENTE,
- CADA CAMINO AUMENTANTE DE EK VUELVE CRÍTICO AL MENOS UN LADO
- CADA LADO PUEDE VOLVERSE CRÍTICO $O(n)$ VECES.
- HAY $m$ LADOS $\implies$ HAY $O(m \cdot n)$ CAMINOS AUMENTANTES POSIBLES.

$\therefore COMPL(EK) = O(m \cdot \#CAM\_AUM) = O(m \cdot m \cdot n) = O(m^2 n)$

---

## LAS DISTANCIAS DE E-K NO DISMINUYEN

DEF: DADOS VÉRTICES $x, z$ EN UN NETWORK $N$ Y UN FLUJO $f$,
$$ d_f(x, z) = \begin{cases} 0 & \text{si } x = z \\ \infty & \text{si NO EXISTE } f\text{-CAM-AUM DE } x \text{ A } z \\ \text{MIN LONGITUD } f\text{-CAM-AUM DE } x \text{ A } z & \text{si EXISTE} \end{cases} $$

DEF $d_k(x) = d_{f_k}(s, x)$ PARA $f_k$ EL k-ÉSIMO FLUJO EN UNA CORRIDA DE EK

Proposición: $d_k(x) \le d_{k+1}(x) \quad \forall x$
DEM:
SEA $A = \{x : d_{k+1}(x) < d_k(x)\}$, Y SUPONGAMOS QUE $A \neq \emptyset$
SEA $x \in A$ TAL QUE $d_{k+1}(x) = \min \{d_{k+1}(y) : y \in A\}$ [1]

[1] $\implies d_{k+1}(x) < d_k(x) \le \infty \implies d_{k+1}(x) < \infty \implies \exists f\text{-CAM-AUM ENTRE } s \text{ y } x$.
OBS: $x \neq s$ PUES $s \notin A$ YA QUE $d_k(s) = d_{k+1}(s)$
SEA ENTONCES $P_{k+1} : s \dots x$ UN $f_{k+1}$-CAM-AUM ENTRE $s$ y $x$ DE LONGITUD MÍNIMA, ES DECIR DE LONGITUD $d_{k+1}(x)$.
COMO $x \neq s$, EXISTE UN VÉRTICE $z$ EN $P_{k+1}$ INMEDIATAMENTE ANTERIOR A $x$.
LUEGO $P_{k+1} : s \dots z x$ Y POR SER DE LONG. MÍNIMA, $d_{k+1}(z) = d_{k+1}(x) - 1$ [2]
AHORA HAY DOS OPCIONES
a) $\vec{zx}$ ES UN LADO
b) $\vec{xz}$ ES UN LADO

VEAMOS AMBOS CASOS

**a)** $\vec{zx}$ ES UN LADO
[2] $\implies d_{k+1}(z) < d_{k+1}(x) \stackrel{[1]}{\implies} z \notin A \implies d_k(z) \le d_{k+1}(z) < \infty$
$\implies \exists f_k\text{-CAM-AUM ENTRE } s \text{ y } z$.
SEA $P_k : s \dots z$ UN $f_k$-CAM. AUM DE LONG. MÍNIMA, COMO $\vec{zx} \in E$, PODRÍAMOS AGREGARLO Y OBTENER UN $f_k$-CAM-AUM ENTRE $s$ y $x$ TAL QUE
$d_k(x) \le d_k(z) + 1 \le d_{k+1}(z) + 1 \le d_{k+1}(x)$ ABSURDO PUES $x \in A$.
$\therefore \vec{zx}$ DEBE ESTAR SATURADO $f_k(\vec{zx}) = c(\vec{zx})$
PERO $P_{k+1} : s \dots z x$ ES $f_{k+1}$-CAM-AUM $\implies f_{k+1}(\vec{zx}) < c(\vec{zx})$ $\implies \vec{zx}$ SE UTILIZÓ DE MODO BACKWARDS AL PASAR DE $f_k$ A $f_{k+1}$
ES DECIR SE UTILIZÓ UN CAMINO DE LONGITUD MÍNIMA DE LA FORMA
$\tilde{P}_k : s \dots x \overleftarrow{z} \dots t$
LUEGO $d_k(z) = d_k(x) + 1 > d_{k+1}(x) + 1 = d_{k+1}(z) + 1 + 1$
$\ge d_k(z) + 2 \implies 0 > 2$ ABSURDO DE ASUMIR $\vec{zx}$ LADO.

**b)** $\vec{xz}$ ES UN LADO
LUEGO $P_{k+1}$ ES DE LA FORMA $s \dots \overleftarrow{z} x$
$\implies d_{k+1}(z) < d_{k+1}(x) \stackrel{[1]}{\implies} z \notin A \implies d_k(z) \le d_{k+1}(z) < \infty \implies \exists f_k\text{-CAM-AUM ENTRE } s \text{ y } z$
$P_k : s \dots z$ UNO DE ELLOS, DE LONGITUD MÍNIMA, PODRÍA AGREGARLE $\vec{xz}$ BACKWARDS, PERO ENTONCES
$d_k(x) \le d_k(z) + 1 \le d_{k+1}(z) + 1 \le d_{k+1}(x) \implies$ ABSURDO PUES $x \in A$
COMO NO LO PUEDO AGREGAR BACKWARDS, ENTONCES $f_k(\vec{xz}) = 0$
PERO SI $P_{k+1} : s \dots \overleftarrow{z} x$ ES UN $f_{k+1}$-CAM-AUM, ENTONCES $f_{k+1}(\vec{xz}) > 0$
$\implies$ SE UTILIZÓ $\vec{xz}$ FORWARDS AL PASAR DE $f_k$ A $f_{k+1}$
$\therefore \exists f_k \text{-CAM-AUM DE LONGITUD MÍNIMA DE LA FORMA}$
$\tilde{P}_k : s \dots \vec{xz} \dots t$
$\implies d_k(z) = d_k(x) + 1 > d_{k+1}(x) + 1 = d_{k+1}(z) + 1 + 1 \ge d_k(z) + 2 \implies 0 > 2$ ABSURDO DE ASUMIR QUE $\vec{xz}$ ES UN LADO

CONCLUIMOS ENTONCES QUE $A = \emptyset$ Y $d_k(x) \le d_{k+1}(x) \quad \forall x$ $\blacksquare$

---

## LA COMPLEJIDAD DE DINIC, PARA AMBAS VERSIONES, ES $O(n^2 m)$
DEM
COMO EL NIVEL DE $t$ AUMENTA EN LOS NA SUCESIVOS, SÓLO PUEDE HABER $O(n)$ NA'S, LUEGO
$COMPL(DINIC) = O(n) \cdot (\overbrace{COMPL(CONSTRUIR\_NA)}^{BFS \text{ ES } O(m)} + COMPL(HALLAR\_FLUJO\_BLOQUEANTE))$
$= O(n) \cdot (O(m) + COMPL(HALLAR\_FLUJO\_BLOQUEANTE))$

VEAMOS LA COMPLEJIDAD DE HALLAR UN FLUJO BLOQUEANTE EN AMBAS VERSIONES:

**VERSIÓN ORIGINAL**
EL NA ES TAL QUE NO HAY VÉRTICE SIN LADOS DE SALIDA (*)
$\therefore$ DFS SIEMPRE LLEGA A $t$ SIN BACKTRACKING $\implies$ DFS ES $O(\#NIVELES) = O(n)$
COMO CADA CAMINO BORRA UN LADO DEL NA, HAY $O(m)$ CAMINOS EN CADA NA.
$\therefore$ LA COMPLEJIDAD DE HALLAR LOS CAMINOS Y AUMENTAR EL FLUJO POR ELLOS ES $O(n m)$ [1]

AHORA BIEN, ¿CUÁL ES LA COMPLEJIDAD DE ASEGURAR (*)?
PARA ELLO, SE DEBEN RECORRER LOS VÉRTICES LUEGO DE CADA NUEVO CAMINO Y BORRAR AQUELLOS QUE NO TENGAN LADOS DE SALIDA
HAY $O(m)$ CAMINOS $\therefore$ SE "PODA" $O(m)$ VECES A LOS $O(n)$ VÉRTICES, CHEQUEANDO SUS SALIDAS EN $O(1)$, TOTAL: $O(m n)$ [2]
$COMPL(\text{BORRAR UN VÉRTICE Y SUS LADOS}) = O(d(x))$, PERO SOBRE TODOS LOS "PODAR" SE OBTIENE $\sum_x O(d(x)) = O(m)$ [3]

FINALMENTE, $COMPL(HALLAR\_FLUJO\_BLOQUEANTE) = \overbrace{O(m n)}^{[1]} + \overbrace{O(m n)}^{[2]} + \overbrace{O(m)}^{[3]} = O(m n)$
$\therefore COMPL(DINIC) = O(n)(O(m) + O(m n)) = O(m n^2)$ $\blacksquare$

**VERSIÓN OCCIDENTAL**
PARA ELLO AVANZAMOS EN PSEUDO-CÓDIGO
```
g=0; FLAG=1;
while (FLAG)
  P:=[s];
  x:=s;
  while ((x != t) and FLAG)
    if (\Gamma^+(x) != \emptyset)
      TOMAR y \in \Gamma^+(x)
      P.add(y); x:=y; } A (AVANZAR)
    else
      if (x != s)
        z:= ELEM ANT DE x EN P
        P.delete(x); NA.delete(zx); x:=z; } R (RETROCEDER)
      else
        FLAG=0
  if (x == t)
    AUMENTAR EL FLUJO A LO LARGO DE P Y BORRAR LOS LADOS SATURADOS } I (INCREMENTAR)
return g;
```

UNA CORRIDA DEL ALGORITMO ES DE LA FORMA $\overbrace{A \dots A R A \dots A I}^{PATRÓN \text{ A}\dots\text{AX REPETIDO}} A \dots A R \dots$
$O(A) = O(1)$ OPERACIÓN ADD
$O(R) = O(1)$ OPERACIÓN DELETE
$O(I) = O(n)$ RECORRER P 2 VECES P/AUMENTAR FLUJO Y BORRAR LADOS

¿CUÁNTAS A HAY EN CADA PARTE DEL PATRÓN REPETIDO? COMO CADA A AVANZA UN NIVEL EN EL NA, HAY $O(n)$ A's HASTA TENER QUE RETROCEDER O LLEGAR A $t$.
$COMPL(A \dots A R) = O(n) + O(1) = O(n) = COMPL(A \dots A I) = O(n) \cdot O(1) + 1 \cdot O(n)$
COMO R BORRA UN LADO E I BORRA AL MENOS UN LADO, HAY $O(m)$ REPETICIONES DE A...AX
FINALMENTE $COMPL(HALLAR\_FLUJO\_BLOQUEANTE) = O(m) \cdot O(n) = O(m n)$ $\blacksquare$

---

## TEOREMA: COMPLEJIDAD DE WAVE DE TARJAN ES $O(n^3)$
COMO WAVE ES DE TIPO DINIC Y LA DISTANCIA ENTRE $s$ y $t$ AUMENTA EN LOS NA'S SUCESIVOS, HAY $O(n)$ NA'S, PUES $t$ NO PUEDE ESTAR A MÁS DE $n$ LADOS DE $s$.
LUEGO $COMPL(WAVE) = \underbrace{O(n)}_{\#NA'S} ( \underbrace{O(m)}_{\text{CONSTRUIR EL NA CON BFS}} + \underbrace{COMPL(HALLAR\_FLUJO\_BLOQUEANTE)}_{\text{QUEREMOS VER QUE ES } O(n^2)} )$
NOTAR QUE $O(m) + O(n^2) = O(n^2)$ PUES $m \le \binom{n}{2} = O(n^2)$

EL PROCESO SE DIVIDE EN
**"OLAS" O "BALANCEOS" HACIA ADELANTE:**
CUANDO UN VÉRTICE $x$ LE MANDA FLUJO A $z$, PUEDE QUE $\vec{xz}$ SE SATURE O NO.
$S = \text{TODOS LOS CASOS DONDE SE SATURA UN LADO}$
$P = \text{TODOS LOS CASOS DONDE NO SE SATURA UN LADO}$

**"OLAS" O BALANCEOS HACIA ATRÁS:**
CUANDO $x$ LE DEVUELVE FLUJO A $v$, PUEDE QUE $\overleftarrow{vx}$ SE VACÍE O NO
$V = \text{CASOS DONDE SE VACÍA UN LADO}$
$Q = \text{CASOS DONDE NO SE VACÍA UN LADO}$

$S$: SUP $\vec{xz}$ SATURADO, ¿PUEDE VOLVER A SATURARSE?
PARA ELLO, PRIMERO $z$ LE DEBE DEVOLVER FLUJO A $x$, CON LO CUAL $z$ ESTÁ BLOQUEADO, PERO $x$ NO LE VOLVERÁ A MANDAR PUES SÓLO SE ENVÍA HACIA ADELANTE A VÉRTICES NO BLOQUEADOS.
$\therefore$ C/LADO SE SATURA A LO SUMO UNA VEZ,
[1] $COMPL(S) = O(m)$

$V$: SUP $\overleftarrow{vx}$ SE VACÍA. ¿PUEDE VOLVER A VACIARSE? NO, PUES SI $x$ LE DEVOLVIÓ FLUJO A $v$ PARA VACIAR EL LADO $\overleftarrow{vx}$, ENTONCES $x$ ESTÁ BLOQUEADO Y $v$ NO LE MANDARÁ MÁS FLUJO COMO PARA QUE $x$ PUEDA DEVOLVERLO. $\therefore \overleftarrow{vx}$ NO SE PODRÁ VACIAR DE NUEVO. C/LADO SE VACÍA UNA VEZ A LO SUMO.
[2] $COMPL(V) = O(m)$

$P$ y $Q$
EN UNA OLA HACIA ADELANTE, TODOS LOS LADOS, MENOS QUIZÁS EL ÚLTIMO USADO, SE SATURAN.
$\therefore$ EN CADA VÉRTICE EN CADA OLA HACIA ADELANTE, HAY A LO SUMO 1 LADO QUE NO SE SATURA Y SE INCLUYE EN $P$. ENVIAR FLUJO POR ESTE LADO ES $O(1)$.
[3] $COMPL(P) = O(n) \cdot \#OLAS\_HACIA\_ADELANTE$

LO MISMO OCURRE CON $Q$ PUES EN CADA VÉRTICE DE CADA OLA HACIA ATRÁS, HAY A LO SUMO UN LADO QUE NO SE VACÍA, Y DEVOLVER FLUJO POR ÉL ES $O(1)$.
[4] $COMPL(Q) = O(n) \cdot \#OLAS\_HACIA\_ATRÁS$

$\#OLAS\_HACIA\_ADELANTE = \#OLAS\_HACIA\_ATRÁS$
EN C/OLA HACIA ADELANTE, SALVO QUIZÁS EN LA ÚLTIMA, ALGÚN VÉRTICE QUEDA DESBALANCEADO Y $\therefore$ BLOQUEADO, Y NUNCA SE DESBLOQUEA.
[5] $\#OLAS\_HACIA\_ADELANTE = O(n)$

[3], [4] y [5] $\implies COMPL(P) = COMPL(Q) = O(n) \cdot O(n) = O(n^2)$

$\therefore COMPL(HALLAR\_FLUJO\_BLOQUEANTE) = [1] + [2] + [3] + [4]$
$= O(m) + O(m) + O(n^2) + O(n^2)$
$= O(n^2)$ $\blacksquare$

---

## MAX FLOW MIN CUT
a) EL VALOR DE TODO FLUJO ES MENOR O IGUAL QUE LA CAPACIDAD DE TODO CORTE
b) $f$ FLUJO $\implies f$ MAXIMAL $\iff \exists \text{ CORTE } S \text{ tq } v(f) = CAP(S)$ (y ESTE CORTE ES MINIMAL)

DEM:
**a)** VIMOS EN UN LEMA QUE SI $f$ ES UN FLUJO y $S$ ES UN CORTE,
$v(f) = f(S, \bar{S}) - f(\bar{S}, S) \le f(S, \bar{S}) \le c(S, \bar{S}) = CAP(S)$
(ya que $\sum_{x \in S, y \notin S, \vec{xy} \in E} f(\vec{xy}) \ge 0$ pues $f(\vec{xy}) \ge 0 \quad \forall \vec{xy} \in E$)

**b)** $f$ FLUJO
($\implies$) $f$ MAXIMAL, DEFINIMOS $S = \{s\} \cup \{x \in V : \exists f\text{-CAM-AUM ENTRE } s \text{ y } x\}$
VEAMOS QUE $S$ ES UN CORTE, $s \in S \checkmark$
SI NO FUERA CORTE $\implies t \in S \implies \exists f\text{-CAM-AUM ENTRE } s \text{ y } t \implies \text{PUEDO AUMENTAR EL FLUJO EN ALGÚN } "\epsilon"$
$\therefore \text{EXISTIRÁ } f' \text{ CON } v(f') = v(f) + \epsilon > v(f)$
ABSURDO PUES $f$ ES MAXIMAL.
$\therefore t \notin S$ y $S$ ES CORTE.

AHORA PODEMOS USAR EL LEMA MENCIONADO ANTERIORMENTE PARA VER
[0] $v(f) = f(S, \bar{S}) - f(\bar{S}, S)$

[1] $f(S, \bar{S}) = \sum_{x \in S, y \notin S, \vec{xy} \in E} f(\vec{xy})$
PARA CUALQUIER PAR $\vec{xy}$ DE LA SUMATORIA,
$x \in S \implies \exists f\text{-CAM AUM DE } s \text{ A } x$
$y \notin S \implies \nexists f\text{-CAM AUM DE } s \text{ A } y$
$\vec{xy} \in E \implies s \dots \vec{xy}$ PODRÍA SER CAM-AUM PERO NO LO ES
$\therefore f(\vec{xy}) = c(\vec{xy}) \quad \forall \vec{xy}$
$\therefore f(S, \bar{S}) = c(S, \bar{S}) = CAP(S)$

$f(\bar{S}, S) = \sum_{y \notin S, x \in S, \vec{yx} \in E} f(\vec{yx})$
PARA CUALQUIER PAR $\vec{yx}$,
$y \notin S \implies \nexists s \dots y \quad f\text{-CAM AUM}$
$x \in S \implies \exists s \dots x \quad f\text{-CAMAUM}$
$\vec{yx} \in E \implies$ LE PODRÍA AGREGAR $s \dots x \overleftarrow{y}$, PERO ESE CAMINO NO ES AUMENTANTE $\implies f(\vec{yx}) = 0$
[2] $\therefore f(\bar{S}, S) = 0$

De [0], [1] y [2] $\implies v(f) = f(S, \bar{S}) - f(\bar{S}, S) = CAP(S)$. COMO TODO CORTE $T$ CUMPLE
$CAP(T) \ge v(f) = CAP(S) \implies S \text{ ES MINIMAL}$.

($\impliedby$) $\exists \text{ CORTE } S \text{ TAL QUE } v(f) = CAP(S)$
SEA $g$ OTRO FLUJO,
$v(g) \stackrel{(a)}{\le} CAP(S) = v(f) \implies f \text{ MAXIMAL}$
y $S$ ES MINIMAL PUES SI $T$ ES OTRO CORTE
$CAP(T) \ge v(f) = CAP(S)$ $\blacksquare$

---

## BABY BROOKS
$G \text{ CONEXO NO REGULAR} \implies \chi(G) \le \Delta$

DEM
SEA $x \text{ tq } d(x) = \delta$
CORREMOS $BFS(x)$, OBTENIENDO TODOS LOS VÉRTICES Y EL ORDEN EN EL QUE FUERON AGREGADOS.
AHORA CORREMOS GREEDY CON EL ORDEN INVERSO AL OBTENIDO DE MANERA TAL QUE LA RAÍZ $x$ SERÁ EL ÚLTIMO POR COLOREAR.
$c(\text{PRIMER VÉRTICE}) = 1$ (ES POSTERIOR EN EL ORDEN INVERSO)
PARA COLOREAR CUALQUIER VÉRTICE $z \neq x$, SABEMOS QUE $z$ FUE INCLUIDO A BFS POR UN VECINO ANTERIOR QUE AÚN NO FUE COLOREADO, POR LO TANTO SE DESCARTARÁN, EN EL PEOR CASO, $d(z) - 1 \le \Delta - 1$ COLORES, SIENDO POSIBLE UTILIZAR ALGÚN COLOR EN $\{1, \dots, \Delta\}$ PARA $z$.
AHORA BIEN CUANDO LLEGAMOS A $x$, COMO $d(x) = \delta < \Delta$ (POR NO SER REGULAR), HABRÁ SIEMPRE UN COLOR EN $\{1, \dots, \Delta\}$ QUE SUS VECINOS NO UTILICEN, ENTONCES LO UTILIZA.
DE ESTA FORMA SE OBTUVO UN COLOREO PROPIO DE $G$ CON $\Delta$ COLORES
$\therefore \chi(G) \le \Delta$ $\blacksquare$

---

## 2-COLOR ES POLINOMIAL
PARA ELLO DARÉ UN ALGORITMO POLINOMIAL QUE COLOREA UN GRAFO $G$ CON DOS COLORES Y MOSTRARÉ QUE SI EL COLOREO NO ES PROPIO ENTONCES $\chi(G) \ge 3$.
SI $G$ NO ES CONEXO SE CORRE EL ALGORITMO EN TODAS SUS COMPONENTES CONEXAS Y SIGUE SIENDO POLINOMIAL.

ALGORITMO DE COLOREO
SEA $x$ UN VÉRTICE ARBITRARIO DE $G$
CORRO $BFS(x)$ OBTENIENDO UN ORDEN DE VÉRTICES EN DISTINTOS NIVELES. $\implies O(m) \text{ por BFS}$
LUEGO COLOREO TODO VÉRTICE $z$ DE LA FORMA
$c(z) = NIVEL_{BFS}(z) \pmod 2$
LUEGO CHEQUEO SI ES PROPIO, EN $\sum_{v \in V} O(d(v)) = O(m)$
- SI ES PROPIO, ENTONCES EXISTE UN COLOREO PROPIO CON 2 COLORES $\implies \chi(G) \le 2$ $\checkmark$
- SI NO ES PROPIO,
$\exists u, v \text{ VÉRTICES tq } c(u) = c(v) \text{ y } uv \in E$
$\implies NIVEL_{BFS}(u) \pmod 2 = NIVEL_{BFS}(v) \pmod 2$
$\implies NIVEL_{BFS}(u) + NIVEL_{BFS}(v) \text{ ES PAR }$ [1]
SABEMOS QUE EXISTE UN CAMINO, POR BFS, DESDE LA RAÍZ HASTA $u$ Y OTRO HASTA $v$
$x \dots u$
$x \dots v$
$\therefore \exists w \text{ VÉRTICE A PARTIR DEL CUAL LOS CAMINOS DIFIEREN }$ 
$x \dots w \dots u$
$x \dots w \dots v$
COMO $uv \in E$, TENEMOS EL CICLO
$w \dots u v \dots w$
¿CUÁNTOS LADOS TIENE?
$w \dots u \implies NIVEL_{BFS}(u) - NIVEL_{BFS}(w)$
$v \dots w \implies NIVEL_{BFS}(v) - NIVEL_{BFS}(w)$
$uv \implies 1 \text{ LADO}$
EN TOTAL SON $\underbrace{NIVEL_{BFS}(u) + NIVEL_{BFS}(v)}_{\text{PAR POR [1]}} - \underbrace{2 \cdot NIVEL_{BFS}(w)}_{\text{PAR}} + \underbrace{1}_{uv} = \text{IMPAR}$
Y POR LO TANTO HAY UN CICLO CON UNA CANTIDAD IMPAR DE LADOS $\implies \chi(G) \ge 3$.

Y DE ESTA FORMA SE DETERMINA EN TIEMPO POLINOMIAL SI UN GRAFO ES O NO 2-COLOREABLE (SOBRE EL TAMAÑO DE G).

---

## TEOREMA DE HALL
SEA $G$ UN GRAFO BIPARTITO CON PARTES $X$ E $Y$.
EXISTE UN MATCHING COMPLETO DE $X$ A $Y$ SI TODO SUBCONJUNTO DE $X$ TIENE MÁS (O IGUAL) VECINOS QUE ELLOS. FORMALMENTE:
$\exists M \text{ MATCHING CON } |E(M)| = |X| \iff |\Gamma(S)| \ge |S| \quad \forall S \subseteq X$

($\implies$) $\exists M \text{ MATCHING CON } |E(M)| = |X| \implies \exists f \text{ INYECTIVA DE } X \text{ EN } Y \text{, CON } f(x) \in \Gamma(x)$
$\therefore |f(S)| = |S| \quad \forall S \subseteq X \text{ y } f(S) \subseteq \Gamma(S)$
$\implies |\Gamma(S)| \ge |f(S)| = |S| \quad \forall S \subseteq X$ $\blacksquare$

($\impliedby$) SUPONEMOS LA CONDICIÓN DE HALL $|\Gamma(S)| \ge |S| \quad \forall S \subseteq X$ VERDADERA.
SI AL CORRER EL ALGORITMO P/HALLAR MATCHING MAXIMAL NOS QUEDAMOS CON UN $M_{max}$ TAL QUE $|E(M_{max})| < |X|$
UTILIZARÉ LA FORMA MATRICIAL DEL ALGORITMO P/CONSTRUIR UN $S \subseteq X$ QUE VIOLE LA CONDICIÓN DE HALL.
CORREMOS EL ALGORITMO SOBRE EL ÚLTIMO MATCHING $M_{max}$ EL CUAL NO CUBRE A $X$.
$\implies \exists \text{ FILAS SIN "MATCHEAR" Y OTRAS FILAS ETIQUETADAS. DEFINIMOS:}$
$S = \{\text{FILAS ETIQUETADAS}\}$
$T = \{\text{COLUMNAS ETIQUETADAS}\}$
$S_0 = \{\text{FILAS ETIQUETADAS CON } * \} \neq \emptyset \text{ PUES } |E(M_{max})| < |X|$
$T_1 = \{\text{COLS. ETIQUETADAS POR FILAS DE } S_0\}$
$S_1 = \{\text{FILAS ETIQUETADAS POR COLUMNAS DE } T_1\}$
$\vdots$
$T_{j+1} = \{\text{COLUMNAS ETIQUETADAS POR } S_j\}$
$S_j = \{\text{FILAS ETIQUETADAS POR } T_j\}$

ES CLARO QUE
$S = S_0 \cup S_1 \cup \dots \cup S_k$
$T = T_1 \cup T_2 \cup \dots \cup T_k$
Y EN AMBAS LA UNIÓN ES DISJUNTA [1]
PUES EL ALGORITMO SE DETIENE AL PASAR DE UN $S_k$ A UN $T_{k+1} = \emptyset$ Y NUNCA AL PASAR DE UN $T_j$ A UN $S_j$ PUES AL REVISAR COLUMNAS DE $T_j$, O ESTÁN LIBRES Y SE EXTIENDE EL MATCHING (LO CUAL NO OCURRE POR HIP) O ENCUENTRAN UNA FILA Y LA ETIQUETAN $\therefore S_j \neq \emptyset$, MÁS AÚN, CADA COLUMNA DE $T_j$ ETIQUETA ÚNICAMENTE A LA FILA CON LA QUE ESTÁ MATCHEADA,
POR LO TANTO $|S_j| = |T_j| \quad \forall j=1 \dots k$ [2]

AHORA,
$|S| \stackrel{[1]}{=} |S_0| + |S_1| + \dots + |S_k|$
$\stackrel{[2]}{=} |S_0| + |T_1| + \dots + |T_k|$
$\stackrel{[1]}{=} |S_0| + |T| > |T| \text{ PUES } S_0 \neq \emptyset$

VEAMOS QUE $T = \Gamma(S)$
$T \subseteq \Gamma(S)$ PUES LAS FILAS DE $S$ SÓLO ETIQUETAN COLUMNAS VECINAS.
SEA $y \in \Gamma(S)$, SUP $y \notin T$, ENTONCES $y$ NO ESTÁ ETIQUETADO
$\implies \exists x \in S \text{ tq } y \in \Gamma(x)$, PERO AL ESCANEAR $x$ DEBERÍA HABER ETIQUETADO A $y$. ABSURDO
$\therefore y \in T$
Y ASÍ $T = \Gamma(S) \implies |S| > |\Gamma(S)|$ VIOLA LA CONDICIÓN DE HALL LA CUAL ES UN ABSURDO QUE VIENE DE SUPONER QUE $|E(M_{max})| < |X|$ $\blacksquare$

---

## TEOREMA DEL MATRIMONIO DE KÖNIG
TODO GRAFO BIPARTITO REGULAR TIENE UN MATCHING PERFECTO

DEM
SEA $G = (V, E)$ UN GRAFO BIPARTITO REGULAR CON PARTES $X$ E $Y$.
PARA $W \subseteq V$ DEFINIMOS $E_W = \{\hat{w}v \in E : \hat{w} \in W\}$

SEA $S \subseteq X$ UN SUBCONJUNTO, SEA $\hat{x} \in E_S$ UN LADO
$\implies \exists x \in S, y \in Y \text{ tq } \hat{x} = xy \implies \hat{x} \in E_{\Gamma(S)}$
$\therefore y \in \Gamma(x) \implies y \in \Gamma(S)$
ESTO SIGNIFICA QUE $E_S \subseteq E_{\Gamma(S)}$ Y EN PARTICULAR, $|E_S| \le |E_{\Gamma(S)}|$ [1]

VEAMOS $|E_W|$ PARA $W \subseteq X$ ó $W \subseteq Y$
SI $\hat{w}v \in E_W, v \notin W$ PUES $W \subseteq X \implies v \in Y$ y $W \subseteq Y \implies v \in X$
$\implies E_W = \bigcup_{\hat{w} \in W} \{\hat{w}v, v \in \Gamma(\hat{w})\}$ Y LA UNIÓN ES DISJUNTA.
$\implies |E_W| = \sum_{\hat{w} \in W} d(\hat{w}) \stackrel{\text{REGULAR}}{=} \Delta \cdot |W|$ [2]

LUEGO POR [1] Y [2]
$\Delta \cdot |S| \le \Delta \cdot |\Gamma(S)| \implies |S| \le |\Gamma(S)| \quad \forall S \subseteq X$
$\therefore$ POR EL TEOREMA DE HALL EXISTE UN MATCHING COMPLETO DE $X$ A $Y$.
$\implies |X| \le |Y|$, PERO LA ELECCIÓN DE UTILIZAR $X$ SOBRE $Y$ FUE ARBITRARIA, POR LO TANTO TAMBIÉN VALE PARA $Y$, $|Y| \le |X|$
$\therefore |X| = |Y|$ Y EL MATCHING COMPLETO ES PERFECTO $\blacksquare$

---

## TEOREMA DE LA COTA DE HAMMING
SEA $C \subseteq \{0,1\}^n$ UN CÓDIGO BINARIO DE LONGITUD $n$ CON $\delta = \delta(C)$ y $t = \lfloor \frac{\delta - 1}{2} \rfloor$
ENTONCES $|C| \le \frac{2^n}{\sum_{i=0}^t \binom{n}{i}}$

DEM
SEA $A = \bigcup_{v \in C} D_t(v)$ DONDE $D_t(v)$ ES EL DISCO DE RADIO $t$ ALREDEDOR DE $v$
COMO $t = \lfloor \frac{\delta - 1}{2} \rfloor$, $C$ CORRIGE $t$ ERRORES $\implies D_t(v) \cap D_t(w) = \emptyset \quad \forall v, w, v \neq w \in C$
$\therefore A \text{ ES UNA UNIÓN DISJUNTA } \implies |A| = \sum_{v \in C} |D_t(v)|$ [1]

SEA $S_r(v) = \{w \in \{0,1\}^n : d_H(v, w) = r\} \implies D_t(v) = \bigcup_{r=0}^t S_r(v)$ Y LA UNIÓN ES DISJUNTA PUES $d_H(v, w) = r \implies d_H(v, w) \neq r' \quad \forall r' \neq r$
LUEGO $|D_t(v)| = \sum_{r=0}^t |S_r(v)|$ [2]
NOTAR QUE $w \in S_r(v) \iff w \text{ DIFIERE EN } r \text{ BITS DE } v$
$\therefore$ HAY UNA BIYECCIÓN ENTRE $S_r(v)$ Y EL CONJUNTO DE SUBCONJUNTOS DE $r$ BITS EN LOS QUE LAS PALABRAS DE $S_r(v)$ DIFIEREN DE $v$.
$\implies |S_r(v)| = |\{ \text{CONJUNTO DE SUBCONJUNTOS DE } r \text{ BITS DE LOS } n \text{ BITS POSIBLES} \}| = \binom{n}{r}$
$\stackrel{[2]}{\implies} |D_t(v)| = \sum_{r=0}^t \binom{n}{r}$

AHORA, POR [1]
$|A| = \sum_{v \in C} |D_t(v)| = \sum_{v \in C} (\sum_{r=0}^t \binom{n}{r}) \stackrel{\text{INDEPENDIENTE DE } v}{=} |C| \cdot \sum_{r=0}^t \binom{n}{r}$
AHORA BIEN, COMO $A \subseteq \{0,1\}^n \implies |A| \le 2^n$
$\therefore |C| \cdot \sum_{r=0}^t \binom{n}{r} \le 2^n \implies |C| \le \frac{2^n}{\sum_{r=0}^t \binom{n}{r}} \quad (\text{NOTAR QUE } \sum_{r=0}^t \binom{n}{r} > 0 \quad \forall n \ge 0, t \ge 0)$ $\blacksquare$

---

## MATRIZ DE CHEQUEO
SI $H$ ES MATRIZ DE CHEQUEO DE $C$, ENTONCES
$\delta(C) = \min \{j : \exists \text{ UN CONJUNTO DE } j \text{ COLUMNAS LD EN } H\}$

DEM:
SEA $\delta = \delta(C)$ y $M = \min \{j : \exists \text{ UN CONJUNTO DE } j \text{ COLUMNAS LD EN } H\}$
SEA $H^{(k)}$ LA k-ÉSIMA COLUMNA DE $H$.
$m \le \delta$
COMO $C$ ES LINEAL, $\delta = \min \{|x| : x \in C, x \neq 0\}$
SEA $x \in C, x \neq 0 \text{ CON } |x| = \delta$ (EXISTE PORQUE ES UN MÍNIMO)
$|x| = \delta \implies x \text{ TIENE } \delta \text{ UNOS ("1"s) POR LO TANTO }$
$x = e_{i_1} + \dots + e_{i_\delta}$ DONDE $i_1, \dots, i_\delta$ SON LAS POSICIONES DE LOS "1"s EN $x$.

LUEGO COMO $x \in C$ y $C \in Nu(H)$, $Hx^t = 0$
$0 = Hx^t = H(e_{i_1} + \dots + e_{i_\delta})^t = He_{i_1}^t + \dots + He_{i_\delta}^t = H^{(i_1)} + \dots + H^{(i_\delta)}$
$\therefore \{H^{(i_1)}, \dots, H^{(i_\delta)}\} \text{ SON } \delta \text{ COLUMNAS LD DE } H$
$\implies M \le \delta$

$\delta \le M$
SEAN $H^{(j_1)}, \dots, H^{(j_M)}$ $M$ COLUMNAS LD EN $H$, COMO $M$ ES EL MÍNIMO,
$0 = H^{(j_1)} + \dots + H^{(j_M)} = H(e_{j_1})^t + \dots + H(e_{j_M})^t = H(e_{j_1} + \dots + e_{j_M})^t$
$\implies \underbrace{(e_{j_1} + \dots + e_{j_M})}_{\neq 0} \in Nu(H) = C$ y $|e_{j_1} + \dots + e_{j_M}| = M \ge \delta$ (PUES $\delta = \min \{|x| : x \in C, x \neq 0\}$)

FINALMENTE, $\delta = M$ $\blacksquare$

---

## CÓDIGO CÍCLICO
SEA $C$ UN CÓDIGO CÍCLICO DE DIMENSIÓN $k$ Y LONGITUD $n$ Y SEA $g(x)$ SU POLINOMIO GENERADOR. PROBAR QUE:
i) $C = \{p(x) : gr(p) < n \text{ y } g(x) | p(x)\}$
ii) $C = \{v(x) \odot g(x) : v \text{ ES UN POLINOMIO CUALQUIERA}\}$
iii) $gr(g) = n - k$
iv) $g(x)$ DIVIDE A $1 + x^n$

DEM:
SEAN $C_1 = \{p(x) : gr(p) < n \text{ y } g(x) | p(x)\}$
$C_2 = \{v(x) \odot g(x) : v \text{ ES UN POLINOMIO CUALQUIERA}\}$
PARA PROBAR i) Y ii), VEREMOS QUE $C_1 \subseteq C_2 \subseteq C \subseteq C_1$ Y $\therefore C_1 = C = C_2$

$C_1 \subseteq C_2$
$p(x) \in C_1 \implies gr(p) < n \text{ y } g(x) | p(x) \implies \exists q(x) \text{ tq } p(x) = q(x)g(x)$
$\implies p(x) \bmod (1+x^n) = p(x) \quad p(x) \bmod (1+x^n) = q(x)g(x) \bmod (1+x^n)$
$p(x) = q(x) \odot g(x) \in C_2$

$C_2 \subseteq C$
SEA $p(x) = g(x) \odot v(x)$ PARA ALGÚN POLINOMIO $v(x)$ ($p(x) \in C_2$)
LUEGO $v(x) = v_0 + v_1 x + \dots + v_d x^d$ PARA ALGÚN $d$.
$\implies p(x) = g(x) \odot v(x) = v(x) \odot g(x) = (v_0 + v_1 x + \dots + v_d x^d) \odot g(x)$
$= v_0 \odot g(x) + v_1 x \odot g(x) + \dots + v_d x^d \odot g(x)$
$= v_0 g(x) + v_1 Rot(g(x)) + \dots + v_d Rot^d(g(x)) \in C$ PUES ES UNA COMBINACIÓN LINEAL DE PALABRAS EN C.

$C \subseteq C_1$
$p(x) \in C \implies gr(p) < n$
SABEMOS QUE $\exists q(x) \text{ y } r(x) \text{ tq } p(x) = g(x) \cdot q(x) + r(x) \text{, CON } gr(r) < gr(g)$
AHORA, $p(x) \bmod (1+x^n) = (g(x) \cdot q(x) + r(x)) \bmod (1+x^n)$
$p(x) = g(x) \odot q(x) + (r(x) \bmod (1+x^n)) \text{, } gr(r) < gr(g) < n \implies r \bmod 1+x^n = r$
$p(x) = g(x) \odot q(x) + r(x)$
$C \ni p(x), C \ni g(x) \odot q(x) \implies C \ni r(x)$
PERO $r(x) \in C \text{ y } gr(r) < gr(g) \implies r(x) = 0 \therefore g(x) | p(x) \text{ y } gr(p) < n \implies p(x) \in C_1$

iii) VIMOS YA QUE $C = C_1$
$\therefore p(x) \in C \iff gr(p) < n \text{ y } \exists q(x) \text{ tq } p(x) = q(x)g(x)$
$gr(p) < n \implies gr(q \cdot g) < n \implies gr(q) + gr(g) < n$
$\implies gr(q) < n - gr(g)$
SI TOMO CUALQUIER $q(x)$ CON $gr(q) < n - gr(g) \implies gr(q \cdot g) < n \text{ Y } q \cdot g \in C$
$\therefore \exists \text{ UNA BIYECCIÓN ENTRE } C \text{ Y LOS POLINOMIOS DE GRADO MENOR A } n - gr(g)$
COMO $C$ ES LINEAL,
$|C| = 2^k$
POR LA BIYECCIÓN
$|C| = |\{\text{CONJUNTO DE POLINOMIOS DE GRADO MENOR A } n - gr(g)\}| = 2^{n - gr(g)}$
$\therefore k = n - gr(g) \text{ ó } gr(g) = n - k$

iv) $(1+x^n) = g(x)q(x) + r(x) \text{ CON } gr(r) < gr(g)\text{, DONDE } q \text{ Y } r \text{ EXISTEN POR LA DIVISIÓN } \frac{1+x^n}{g(x)}$
$(1+x^n) \bmod (1+x^n) = 0 = (g(x)q(x) + r(x)) \bmod (1+x^n)$
$0 = g(x) \odot q(x) + r(x) \bmod (1+x^n)$
$r(x) = g(x) \odot q(x) \in C$ y como $gr(r) < gr(g) \implies r(x) = 0 \text{ y } \therefore g(x) | (1+x^n)$

---

## 3-SAT ES NP-COMPLETO
DEM: VEREMOS QUE CNF-SAT $\le_P$ 3-SAT
SEA $B$ UNA INSTANCIA DE CNF-SAT CON VARIABLES $x_1, \dots, x_n$
$B = D_1 \land \dots \land D_M \quad \text{CON} \quad D_j = \ell_{j_1} \lor \dots \lor \ell_{j_{\Gamma_j}}$ DONDE $\ell_{j_k}$ SON LITERALES (VARIABLES O SUS NEGACIONES)
QUEREMOS UN ALGORITMO POLINOMIAL $A$ tq $A(B)$ ESTÉ EN EL DOMINIO DE 3-SAT, Y QUE
$B \text{ SEA SATISFACIBLE } \iff A(B) \text{ LO ES, NOTAR QUE } B(\vec{b}) = 1 \iff D_j(\vec{b}) = 1 \quad \forall j \quad (\vec{b} \text{ UNA ASIGNACIÓN DE LOS } x_i)$
PARA ELLO, $A(B) = E_1 \land \dots \land E_M$ DONDE $E_j$ SERÁN CONJUNCIONES DE DISJUNCIONES DE 3 LITERALES.
POR LO TANTO $A$ TRANSFORMARÁ $D_j \to E_j$ DE LA SIGUIENTE MANERA:

- $\Gamma_j = 3 \implies E_j = D_j$
- $\Gamma_j \ge 4 \implies E_j = (\ell_{j_1} \lor \ell_{j_2} \lor y_{j_1}) \land (\bar{y}_{j_1} \lor \ell_{j_3} \lor y_{j_2}) \land (\bar{y}_{j_2} \lor \ell_{j_4} \lor y_{j_3}) \land \dots \land (\bar{y}_{j_{\Gamma_j-4}} \lor \ell_{j_{\Gamma_j-2}} \lor y_{j_{\Gamma_j-3}}) \land (\bar{y}_{j_{\Gamma_j-3}} \lor \ell_{j_{\Gamma_j-1}} \lor \ell_{j_{\Gamma_j}})$
- $\Gamma_j = 1 \implies E_j = (\ell_{j_1} \lor y_{j_1} \lor y_{j_2}) \land (\ell_{j_1} \lor y_{j_1} \lor \bar{y}_{j_2}) \land (\ell_{j_1} \lor \bar{y}_{j_1} \lor y_{j_2}) \land (\ell_{j_1} \lor \bar{y}_{j_1} \lor \bar{y}_{j_2})$
- $\Gamma_j = 2 \implies E_j = (\ell_{j_1} \lor \ell_{j_2} \lor y_j) \land (\ell_{j_1} \lor \ell_{j_2} \lor \bar{y}_j)$

ES CLARO QUE $D_j(\vec{b}) = 1 \iff \exists \vec{y} \text{ tq } E_j(\vec{b}, \vec{y}) = 1$ PARA LOS CASOS DE $\Gamma_j = 1, 2, 3$ (ASIGNACIONES DE LOS $y_j$)
($\impliedby \text{ PARA } \Gamma_j \ge 4$) SUPONGAMOS QUE NO SE CUMPLE, i.e $\exists \vec{b}, \vec{y} \text{ tq } E_j(\vec{b}, \vec{y}) = 1 \text{ y } D_j(\vec{b}) = 0$
$0 = D_j(\vec{b}) = (\ell_{j_1} \lor \dots \lor \ell_{j_{\Gamma_j}})(\vec{b}) \implies \ell_{j_k}(\vec{b}) = 0 \quad \forall k$
$\implies E_j(\vec{b}, \vec{y}) = (0 \lor 0 \lor y_{j_1}) \land (\bar{y}_{j_1} \lor 0 \lor y_{j_2}) \land \dots \land (\bar{y}_{j_{\Gamma_j-4}} \lor 0 \lor y_{j_{\Gamma_j-3}}) \land (\bar{y}_{j_{\Gamma_j-3}} \lor 0 \lor 0)$
$= y_{j_1} \land (\bar{y}_{j_1} \implies y_{j_2}) \land \dots \land (\bar{y}_{j_{\Gamma_j-4}} \implies y_{j_{\Gamma_j-3}}) \land \bar{y}_{j_{\Gamma_j-3}}$
$= 0 \quad \forall \vec{y}$ ABSURDO PUES $E_j(\vec{b}, \vec{y}) = 1$

($\implies \text{ PARA } \Gamma_j \ge 4$)
SUPONGAMOS $D_j(\vec{b}) = 1$, ENTONCES $\exists k \text{ tq } \ell_{j_k}(\vec{b}) = 1$
DEFINIMOS $\vec{y} = (d_1, \dots, d_{\Gamma_j-3})$ TAL QUE $d_1 = \dots = d_{k-2} = 1$  y $d_{k-1} = d_k = \dots = d_{\Gamma_j-3} = 0$
$y_{j_t}(\vec{y}) = d_t$
LUEGO
$E_j(\vec{b}, \vec{y}) = \underbrace{(\ell_{j_1} \lor \ell_{j_2} \lor y_{j_1})(\vec{b}, \vec{y})}_{\text{TODOS ESTOS TÉRMINOS SON 1 PUES CONTIENEN UN } d_p \text{ CON } 1 \le p \le k-2}$
$\land \underbrace{(\bar{y}_{j_1} \lor \ell_{j_3} \lor y_{j_2})(\vec{b}, \vec{y})}_{=1}$
$\land \dots \lor 1$
$\land \underbrace{(\bar{y}_{j_{k-3}} \lor \dots \lor y_{j_{k-2}} \lor \ell_{j_{k-1}})(\vec{b}, \vec{y})}_{=1} \land \underbrace{(\bar{y}_{j_{k-2}}^0 \lor \ell_{j_k}^1 \lor y_{j_{k-1}}^0)(\vec{b}, \vec{y})}_{=1}$
$\land \underbrace{(\bar{y}_{j_{k-1}}^1 \lor \dots)(\vec{b}, \vec{y})}_{=1} \land \dots \land \underbrace{(\bar{y}_{j_{\Gamma_j-3}}^1 \lor \dots)(\vec{b}, \vec{y})}_{=1} = 1$
$\text{TODOS ESTOS TÉRMINOS SON 1 PUES CONTIENEN UN } \bar{d}_q \text{ CON } k-2 \le q \le \Gamma_j-3$

ASÍ QUEDA DEMOSTRADO QUE $D_j(\vec{b}) = 1 \iff \exists \vec{y} \text{ tq } E_j(\vec{b}, \vec{y}) = 1 \quad \forall j$
$\therefore B \text{ ES SATISFACIBLE } \iff A(B) \text{ ES SATISFACIBLE}$ $\blacksquare$

---

## 3-COLOR ES NP-COMPLETO
DEM: VEREMOS QUE 3-SAT $\le_P$ 3-COLOR EN TIEMPO POLINOMIAL
DOMINIO = "CONJUNTO DE INSTANCIAS"
PARA ELLO, DEBEMOS TRANSFORMAR UN ELEMENTO $B$ DEL DOMINIO DE 3-SAT A UN ELEMENTO $A(B) = G$ DEL DOMINIO DE 3-COLOR, Y MOSTRAR QUE $B \text{ ES SATISFACIBLE } \iff \chi(G) \le 3$.
SABEMOS QUE $B$ ES DE LA FORMA $D_1 \land \dots \land D_M$ DONDE $D_j = (\ell_{j_1} \lor \ell_{j_2} \lor \ell_{j_3})$, CON $\ell_{j_k} = x_{\text{ALGO}} \text{ ó } \bar{x}_{\text{ALGO}}$
LUEGO $A$ TRANSFORMA $B$ EN $G$, EN TIEMPO POLINOMIAL, DE LA SIGUIENTE FORMA:
VÉRTICES DE $G$
$\{v_1, \dots, v_n, w_1, \dots, w_n\} \cup \left(\bigcup_{j=1}^M \{a_{j_1}, a_{j_2}, a_{j_3}, e_{j_1}, e_{j_2}, e_{j_3}\}\right) \cup \{t, CAPITÁN\}$
PARA LAS VARIABLES DE B $x_1, \dots, x_n$

LADOS DE $G$
TRIÁNGULOS $\{(t, v_i), (t, w_i), (v_i, w_i)\}$ PARA $i=1 \dots n$
TRIÁNGULOS $\{(a_{j_1}, a_{j_2}), (a_{j_2}, a_{j_3}), (a_{j_3}, a_{j_1})\}$ PARA $j=1 \dots M$ (GARRAS)
LADOS $\{(a_{j_r}, e_{j_r})\}$ PARA $j=1 \dots M$ y $r=1, 2, 3$
LADOS $\{(CAPITÁN, e_{j_k})\}$ PARA $j=1 \dots M$ y $k=1, 2, 3$
LADO $(CAPITÁN, t)$

PARA UN LITERAL $\ell$, DEFINIMOS EL VÉRTICE CORRESPONDIENTE A ÉL COMO
$v(\ell) = \begin{cases} v_k & \text{si } \exists k \text{ tq } \ell = x_k \\ w_k & \text{si } \exists k \text{ tq } \ell = \bar{x}_k \end{cases}$

FINALMENTE, AGREGAMOS LOS LADOS
$\{(e_{j_r}, v(\ell_{j_r}))\}$ PARA $j=1 \dots M$ y $r=1, 2, 3$

ES CLARO QUE LA CONSTRUCCIÓN DE $G$ ES EN TIEMPO POLINOMIAL EN EL TAMAÑO DE $B$.

AHORA DEBEMOS PROBAR
$B \text{ SATISFACIBLE } \iff \chi(G) \le 3 \quad (*)$
NOTAR QUE $B \text{ SATISFACIBLE } \iff \exists \vec{b} \text{ VECTOR DE ASIGNACIÓN DE } x_i\text{'s tq } B(\vec{b}) = 1 \iff D_j(\vec{b}) = 1 \quad \forall j=1 \dots M$

($\impliedby$) $\chi(G) \le 3$ DONDE $G=A(B)$. $V = \text{"VÉRTICES DE } G\text{", } E = \text{"LADOS DE } G\text{"}$
COMO $G$ TIENE TRIÁNGULOS ($K_3$) DE SUBGRAFOS, $\chi(G) \ge 3 \therefore \chi(G) = 3$
$\implies \exists \text{ UN COLOREO } c \text{ PROPIO DE } G \text{ CON 3 COLORES. } c(v) = \text{"COLOR DE } v\text{"}$
COMO $(t, CAPITÁN) \in E \implies c(t) \neq c(CAPITÁN)$
$\therefore C \text{ UTILIZA LOS COLORES } \{c(t), c(CAPITÁN), c_3\}$
DEFINIMOS $\vec{b} = (b_1, \dots, b_n)$ TAL QUE $b_i = \begin{cases} 1 & \text{si } c(v_i) = c(CAPITÁN) \\ 0 & \text{si NO.} \end{cases}$

PROBAREMOS QUE $B(\vec{b}) = 1$ i.e $B$ ES SATISFACIBLE
PARA ELLO, HAY QUE MOSTRAR QUE $D_j(\vec{b}) = 1 \quad \forall j=1 \dots M$, Y PARA ELLO, HAY QUE MOSTRAR QUE
$\forall j=1 \dots M, \exists r \text{ tq } \ell_{j_r}(\vec{b}) = 1$
SEA $j \in \{1 \dots M\}$, COMO LOS "$a_{j_r}$"'s FORMAN TRIÁNGULOS QUE UTILIZAN LOS 3 COLORES,
$\exists r \text{ tq } c(a_{j_r}) = c(t)$ [1]
$(e_{j_r}, a_{j_r}) \in E \implies c(e_{j_r}) \neq c(a_{j_r}) = c(t)$
$(e_{j_r}, CAPITÁN) \in E \implies c(e_{j_r}) \neq c(CAPITÁN)$ $\implies c(e_{j_r}) = c_3$ [2]
$(e_{j_r}, v(\ell_{j_r})) \in E \text{ y } [2] \implies c(v(\ell_{j_r})) \neq c_3$
$v(\ell_{j_r}) = v_{ALGO} \text{ ó } w_{ALGO} \implies (v(\ell_{j_r}), t) \in E \therefore c(v(\ell_{j_r})) \neq c(t)$
POR ENDE $c(v(\ell_{j_r})) = c(CAPITÁN)$

VEAMOS QUE OCURRE CON $\ell_{j_r}$ PARA LOS CASOS $\ell_{j_r} = \text{VARIABLE Y } \ell_{j_r} = \text{VARIABLE NEGA.}$
SI $\ell_{j_r}$ ES UNA VARIABLE, $\exists k \text{ tq } \ell_{j_r} = x_k \implies \ell_{j_r}(\vec{b}) = x_k(\vec{b}) = b_k$
$\implies v(\ell_{j_r}) = v_k \therefore c(v_k) = c(v(\ell_{j_r})) = c(CAPITÁN) \implies b_k = 1 \implies \ell_{j_r}(\vec{b}) = 1$
SI $\ell_{j_r}$ ES UNA NEGACIÓN DE VARIABLE, $\exists k \text{ tq } \ell_{j_r} = \bar{x}_k \implies \ell_{j_r}(\vec{b}) = \bar{x}_k(\vec{b}) = 1 - b_k$
$\implies v(\ell_{j_r}) = w_k \therefore c(w_k) = c(v(\ell_{j_r})) = c(CAPITÁN)$
$v_k w_k \in E \implies c(v_k) \neq c(CAPITÁN) \implies b_k = 0 \implies \ell_{j_r}(\vec{b}) = 1$
POR LO TANTO $\forall j=1 \dots M, \exists r \text{ tq } \ell_{j_r}(\vec{b}) = 1 \therefore B(\vec{b}) = 1 \implies B \text{ ES SATISFACIBLE}$

($\implies$) $B \text{ SATISFACIBLE } \implies \exists \vec{b} \text{ tq } B(\vec{b}) = 1$
DARÉ UN COLOREO $c$ PROPIO DE $G=A(B)$ CON 3 COLORES Y POR LO TANTO $\chi(G) \le 3$.
$c(CAPITÁN) = ESCARLATA, \quad c(t) = VERDE \implies \text{EL LADO } (t, CAPITÁN) \text{ MANTIENE LO PROPIO DEL COLOREO.}$
A PARTIR DE ALGÚN $\vec{b} \text{ tq } B(\vec{b}) = 1$, DEFINIMOS (CON $\vec{b} = (b_1, \dots, b_n)$)
$c(v_i) = \begin{cases} ESCARLATA & \text{si } b_i = 1 \\ NEGRO & \text{si } b_i = 0 \end{cases} \quad \text{y} \quad c(w_i) = \begin{cases} NEGRO & \text{si } b_i = 1 \\ ESCARLATA & \text{si } b_i = 0 \end{cases}$
DE ESTA FORMA, LOS TRIÁNGULOS $\{t, v_i, w_i\}$ PARA $i=1 \dots n$ MANTIENEN LO PROPIO DEL COLOREO PUES UTILIZAN LOS 3 COLORES.

COMO $B(\vec{b}) = 1$, $D_j(\vec{b}) = 1 \quad \forall j=1 \dots M \implies \forall j=1 \dots M, \exists k_j \text{ tq } \ell_{j_{k_j}}(\vec{b}) = 1$. TOMO ALGÚN $k_j$ QUE LO CUMPLA.
Y DEFINIMOS
$c(a_{j_{k_j}}) = VERDE$
$c(a_{j_r}) = \text{UNO NEGRO Y OTRO ESCARLATA} \quad r \neq k_j$
LOS TRIÁNGULOS $\{a_{j_1}, a_{j_2}, a_{j_3}\}$ PARA $j=1 \dots M$ MANTIENEN LO PROPIO DEL COLOREO. (USAN LOS 3 COLORES)

COLOREAMOS
$c(e_{j_{k_j}}) = NEGRO \quad \text{y} \quad c(e_{j_r}) = VERDE \text{ PARA } r \neq k_j$
LUEGO LOS SIGUIENTES LADOS MANTIENEN LO PROPIO DEL COLOREO:
$\{(e_{j_q}, CAPITÁN)\}$ PARA $j=1 \dots M$ y $q=1, 2, 3$ PUES $c(e_{j_q}) = NEGRO \text{ ó } VERDE$ Y $c(CAPITÁN) = ESCARLATA$
$\{(e_{j_r}, a_{j_r})\}$ PARA $j=1 \dots M$ Y $r \in \{1, 2, 3\} \text{ CON } r \neq k_j$ PUES $c(e_{j_r}) = VERDE$ Y $c(a_{j_r}) = NEGRO \text{ O ESCARLATA}$
$\{(e_{j_{k_j}}, a_{j_{k_j}})\}$ PARA $j=1 \dots M$ PUES $c(e_{j_{k_j}}) = NEGRO$ Y $c(a_{j_{k_j}}) = VERDE$
$\{(e_{j_r}, v(\ell_{j_r}))\}$ PARA $j=1 \dots M$ Y $r \in \{1, 2, 3\} \text{ CON } r \neq k_j$, PUES $c(e_{j_r}) = VERDE$ Y $v(\ell_{j_r}) = v_{ALGO} \text{ ó } w_{ALGO} \implies c(v(\ell_{j_r})) = \begin{matrix} NEGRO \text{ ó} \\ ESCARLATA \end{matrix}$

SÓLO QUEDA POR VER EL LADO $e_{j_{k_j}}, v(\ell_{j_{k_j}})$.
SI $\ell_{j_{k_j}}$ ES UNA VARIABLE, $\exists i \text{ tq } \ell_{j_{k_j}} = x_i \implies v(\ell_{j_{k_j}}) = v_i$ Y $\ell_{j_{k_j}}(\vec{b}) = 1 \implies x_i(\vec{b}) = 1 \implies b_i = 1 \implies c(v_i) = ESCARLATA$
$\therefore c(v(\ell_{j_{k_j}})) = c(v_i) = ESCARLATA$ Y $c(e_{j_{k_j}}) = NEGRO \therefore \text{ EL LADO } e_{j_{k_j}}, v(\ell_{j_{k_j}}) \text{ MANTIENE LO PROPIO DEL COLOREO}$
SI $\ell_{j_{k_j}}$ ES UNA NEGACIÓN DE VARIABLE, $\exists i \text{ tq } \ell_{j_{k_j}} = \bar{x}_i \implies \ell_{j_{k_j}}(\vec{b}) = \bar{x}_i(\vec{b}) = 1 - b_i = 1 \implies b_i = 0$
$\therefore v(\ell_{j_{k_j}}) = w_i$ Y $c(w_i) = ESCARLATA$ Y $c(e_{j_{k_j}}) = NEGRO \implies c(w_i) = ESCARLATA$
POR LO QUE EL LADO $e_{j_{k_j}}, v(\ell_{j_{k_j}})$ MANTIENE LO PROPIO DEL COLOREO.
FINALMENTE, $B \text{ SATISFACIBLE } \implies \exists \text{ COLOREO } c \text{ PROPIO DE } A(B)=G \text{ CON 3 COLORES } \implies \chi(G) \le 3$ $\blacksquare$

---

## TEOREMA DEL MATRIMONIO 3-D
MATRIMONIO 3D ES NP-COMPLETO

DEM
MOSTRAREMOS QUE 3-SAT $\le_P$ MATRIMONIO 3D
ESTO ES, A PARTIR DE UNA INSTANCIA $B = D_1 \land \dots \land D_M$ CON VARIABLES $x_1, \dots, x_n$
Y $D_j = \ell_{j_1} \lor \ell_{j_2} \lor \ell_{j_3}$ CON $\ell_{j_k}$ VARIABLES O NEGACIONES DE VARIABLES,
CONSTRUIR EN TIEMPO POLINOMIAL SOBRE EL TAMAÑO DE $B$ UN 3-HIPERGRAFO $H$ DE
MANERA TAL QUE $B \text{ ES SATISFACIBLE } \iff H \text{ TIENE UN MATCHING PERFECTO}$
NOTAR QUE PARA QUE $H$ SEA UNA INSTANCIA DE MATRIMONIO 3D, SUS 3 PARTES $X, Y, Z$ DEBEN TENER LA MISMA CARDINALIDAD. CONSTRUCCIÓN DE $H$ EN TIEMPO POLINOMIAL:
NOTACIÓN: $j \in \{1 \dots M\}, i \in \{1 \dots n\}, r \in \{1, 2, 3\}, k \in \{1 \dots m(n-1)\}$
VÉRTICES DE $H$, EN 3 PARTES DISJUNTAS DE IGUAL CARDINALIDAD:
$X = \{a_{ij}\}_{i, j} \cup \{s_j\}_j \cup \{g_k\}_k \qquad |X| = nm + m + m(n-1) = 2mn$
$Y = \{b_{ij}\}_{i, j} \cup \{t_j\}_j \cup \{h_k\}_k \qquad |Y| = nm + m + m(n-1) = 2mn = |X|$
$Z = \{u_{ij}\}_{i, j} \cup \{w_{ij}\}_{i, j} \qquad |Z| = nm + nm = 2mn = |X| = |Y|$
LADOS DE $H : E(H) = E_0 \cup E_1 \cup E_2 \cup E_3$
$E_0 = \{\{a_{ij}, b_{ij}, u_{ij}\}\}_{i, j} \qquad E_1 = \{\{a_{i(j+1)}, b_{ij}, w_{ij}\}\}_{i, j} \qquad j=M \implies j+1=1$
$E_2 = \{\{g_k, h_k, z\} : z \in Z\}_k$
DEFINIMOS $v_{jr} = \begin{cases} u_{ij} & \text{si } \ell_{jr} = x_i \\ w_{ij} & \text{si } \ell_{jr} = \bar{x}_i \end{cases} \quad \text{Y LUEGO} \quad E_3 = \{\{s_j, t_j, v_{jr}\}\}_{j, r}$
ES CLARO QUE LA CONSTRUCCIÓN DE $H$ ES EN TIEMPO POLINOMIAL DEPENDIENTE DE $n$ y $M$ (TAMAÑO DE B)
AHORA DEBEMOS MOSTRAR QUE $B \text{ ES SATISFACIBLE } \iff \exists \text{ UN MATCHING PERFECTO EN } H$

($\implies$) $B \text{ SATISFACIBLE, SEA } \vec{b} \text{ tq } B(\vec{b}) = 1$, Y $\vec{b} = (b_1, \dots, b_n)$ ASIGNACIONES DE VARIABLES.
CONSTRUIMOS A PARTIR DE $B$ UN MATCHING $M$ EN $H$ Y MOSTRAMOS QUE ES PERFECTO.
LADOS DE $M : F_0 \cup F_1 \cup F_2 \cup F_3$
$F_0 = \{\{a_{ij}, b_{ij}, u_{ij}\} : j \in \{1 \dots M\} \text{ y } b_i = 0\} \subseteq E_0$
$F_1 = \{\{a_{i(j+1)}, b_{ij}, w_{ij}\} : j \in \{1 \dots M\} \text{ y } b_i = 1\} \subseteq E_1 \quad (j+1=1 \text{ si } j=M)$
ESTOS LADOS SON DISJUNTOS PUES LOS $b_i$ NO PUEDEN SER $0$ Y $1$ A LA VEZ.
$B(\vec{b}) = 1 \implies D_j(\vec{b}) = 1 \quad \forall j=1 \dots M \implies \forall j=1 \dots M, \exists r_j \text{ tq } \ell_{jr_j}(\vec{b}) = 1 \text{ (SI HAY MÁS DE 1, ELIJO 1)}$
LUEGO DEFINIMOS
$F_3 = \{\{s_j, t_j, v_{jr_j}\}\}_j$, COMO ELEGIMOS UN SOLO $r_j$ P/C $j$, LOS LADOS DE $F_3$ SON DISJUNTOS ENTRE SÍ
SUPONGAMOS QUE LOS LADOS DE $F_3$ NO SON DISJUNTOS CON LOS LADOS DE $F_1$ o $F_0$
$\implies \exists L_3 \in F_3 \text{ tq } \exists L_0 \in F_0 \text{ tq } L_3 \cap L_0 \neq \emptyset \text{ ó } \exists L_1 \in F_1 \text{ tq } L_1 \cap L_0 \neq \emptyset$
NOTAR QUE ESTA INTERSECCIÓN SÓLO PODRÍA CONTENER UN VERT. $v_{jr_j}$, PUES $\forall L \in F_0, F_1, \quad s_j \notin L, t_j \notin L$
CASO $L_0 = \{a_{ij}, b_{ij}, v_{jr_j}\} \in F_0 \implies v_{jr_j} = u_{ij} \implies \ell_{jr_j} = x_i \implies 1 = \ell_{jr_j}(\vec{b}) = x_i(\vec{b}) = b_i = 0$ ABSURDO. (pues $b_i = 0$)
CASO $L_1 = \{a_{i(j+1)}, b_{ij}, v_{jr_j}\} \in F_1 \implies v_{jr_j} = w_{ij} \implies \ell_{jr_j} = \bar{x}_i \implies 1 = \ell_{jr_j}(\vec{b}) = \bar{x}_i(\vec{b}) = 1 - b_i = 0$ ABSURDO (pues $b_i = 1$)
LUEGO LOS LADOS DE $F_3$ SON DISJUNTOS CON LOS DE $F_1$ y $F_0$.
SEA $N = \{z \in Z : z \text{ NO ESTÁ CUBIERTO POR } F_0, F_1 \text{ ó } F_3\}$
VEAMOS $|Z - N| = |\{z \in Z : z \text{ ESTÁ CUBIERTO POR } F_0, F_1 \text{ ó } F_3\}|$
- $F_3$ CUBRE $M$ ELEMENTOS DE $Z$ (UN $v_{jr_j}$ POR CADA $j$)
SI $q = \#\{i : b_i = 1\}$ Y $p = \#\{i : b_i = 0\}$, LUEGO
- $F_0$ CUBRE $p \cdot M$ VÉRTICES $u_{ij} \in Z$ (LADOS $\{a_{ij}, b_{ij}, u_{ij}\} : b_i = 0$)
- $F_1$ CUBRE $q \cdot M$ VÉRTICES $w_{ij} \in Z$ (LADOS $\{a_{i(j+1)}, b_{ij}, w_{ij}\} : b_i = 1\}$)

ASÍ, $|Z - N| = M + pM + qM = M(p+q+1) = M(n+1)$ (pues $p+q = n$)
LUEGO $|N| = |Z| - |Z - N|$
$= 2nM - M(n+1) = M(n-1)$
$\implies \exists \text{ BIYECCIÓN } f : \{1 \dots M(n-1)\} \to N$
DEFINIMOS
$F_2 = \{\{g_k, h_k, f(k)\}\}_k$
DISJUNTOS ENTRE SÍ Y COMO $f(k) \in N \implies \text{ NO ESTABA CUBIERTO POR } F_0, F_1 \text{ ni } F_3$
$\therefore$ LOS LADOS DE $F_2$ SON DISJUNTOS DE LOS DE $F_0, F_1 \text{ y } F_2 \text{ y SON } M(n-1) \text{ (SOBREYECTIVA)}$

ASÍ, $|E(M)| = \underbrace{|F_0| + |F_1|}_{= nM} + \underbrace{|F_2|}_{= M(n-1)} + \underbrace{|F_3|}_{= M}$
$= nM + M(n-1) + M$
$= 2nM$ Y EL MATCHING ES PERFECTO.

($\impliedby$) SEA $M$ UN MATCHING PERFECTO EN $H$
OBSERVAR QUE LOS VÉRS. $a_{ij}$ y $b_{ij}$ SÓLO ESTÁN EN LADOS DE $E_0$ ó $E_1$
COMO $M$ ES PERFECTO, DEBEN ESTAR TAMBIÉN EN ALGÚN LADO DE $M$.
FIJAMOS $i \in \{1 \dots n\}$
SUPONGAMOS QUE PARA UN $j \in \{1 \dots M\}, \exists L \text{ LADO } \in E(M) \cap E_0 \text{ tq } a_{ij} \in L$
$\implies \{a_{ij}, b_{ij}, u_{ij}\} \in E_0 \cap E(M) \implies b_{ij} \text{ NO PUEDE ESTAR EN OTRO LADO DEL MATCHING } M.$
$\therefore \{a_{i(j+1)}, b_{ij}, w_{ij}\} \notin E(M) \text{ PERO } a_{i(j+1)} \text{ DEBE ESTAR EN } E(M)$
$\implies \{a_{i(j+1)}, b_{i(j+1)}, u_{i(j+1)}\} \in E(M)$
$\implies \{a_{ij}, b_{ij}, u_{ij}\} \in E(M) \quad \forall j \quad \text{ PARA ESTE } i \text{ FIJO. } \text{"CASO 0" PARA i}$
AHORA SI PARA UN $j \in \{1 \dots M\} \exists L \text{ LADO } \in E(M) \cap E_1 \text{ tq } a_{ij} \in L \implies L = \{a_{ij}, b_{i(j-1)}, w_{i(j-1)}\}$
PERO COMO $b_{i(j-1)}$ NO PUEDE ESTAR EN OTRO LADO DE $M$, $\{a_{i(j-1)}, b_{i(j-1)}, u_{i(j-1)}\} \notin E(M) \implies$
$\{a_{i(j-1)}, b_{i(j-2)}, w_{i(j-2)}\} \in E(M) \cap E_1 \quad \forall j \quad \text{ PARA ESTE i FIJO} \quad \text{"CASO 1" PARA i}$

ES CLARO QUE SIEMPRE SE DA O EL "CASO 0" Ó EL "CASO 1" PARA CADA $i$, Y A PARTIR DE ELLO,
DEFINIMOS
$b_i = \begin{cases} 1 & \text{si SE DA "CASO 1" PARA } i \\ 0 & \text{si SE DA "CASO 0" PARA } i \end{cases}$
$\vec{b} = (b_1 \dots b_n)$ Y QUEREMOS VER QUE $D_j(\vec{b}) = 1 \quad \forall j \quad \text{ Y } \therefore B \text{ ES SATISFACIBLE}.$
FIJAMOS $j \in \{1 \dots M\}$
COMO $M$ ES PERFECTO, LOS VÉRTICES $s_j, t_j$ DEBEN ESTAR EN ALGÚN LADO DE $M$
$\implies \exists r \text{ tq } \{s_j, t_j, v_{jr}\} \in E(M)$

SI $\ell_{jr}$ ES UNA VARIABLE, $\exists i \text{ tq } \ell_{jr} = x_i \implies v_{jr} = u_{ij}$
LUEGO $\{s_j, t_j, u_{ij}\} \in E(M) \implies u_{ij} \text{ NO ESTÁ EN OTRO LADO DE } M$
$\implies \{a_{ij}, b_{ij}, u_{ij}\} \notin E(M) \implies \text{"CASO 1" PARA } i$
$\therefore b_i = 1 \implies \ell_{jr}(\vec{b}) = x_i(\vec{b}) = b_i = 1 \implies D_j(\vec{b}) = 1$

SI $\ell_{jr}$ ES UNA NEGACIÓN DE VARIABLE, $\exists i \text{ tq } \ell_{jr} = \bar{x}_i \implies v_{jr} = w_{ij}$
LUEGO $\{s_j, t_j, w_{ij}\} \in E(M) \implies w_{ij} \text{ NO PUEDE ESTAR EN OTRO LADO DE } M$
$\implies \{a_{i(j+1)}, b_{ij}, w_{ij}\} \notin E(M) \implies \text{"CASO 0" PARA } i$
$\therefore b_i = 0 \implies \ell_{jr}(\vec{b}) = \bar{x}_i(\vec{b}) = 1 - b_i = 1 \implies D_j(\vec{b}) = 1$

LUEGO $\exists \vec{b} \text{ tq } D_j(\vec{b}) = 1 \quad \forall j \implies B(\vec{b}) = 1 \implies B \text{ ES SATISFACIBLE}$ $\blacksquare$