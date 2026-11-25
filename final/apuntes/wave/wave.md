# Algoritmo Wave de Tarjan

## Tabla de Contenidos

1. [Introducción](#introduccion)
2. [Algoritmo MKM](#algoritmo-mkm)
3. [Descripción de Wave](#descripcion-wave)
4. [Pseudocódigo de Wave](#pseudocodigo-wave)
5. [Complejidad de Wave](#complejidad-wave)
6. [Push-relabel](#push-relabel)

## 1. Introducción {#introduccion}

Vimos que el algoritmo de Dinic encuentra flujos maximales por medio de los *networks* auxiliares: construir un *network* auxiliar, encontrar un flujo bloqueante en él, cambiar el flujo mediante ese flujo bloqueante y repetir hasta que el flujo quede maximal.

Muchos algoritmos posteriores a Dinic usaron esa misma estrategia, cambiando la forma de encontrar flujos bloqueantes. El primero que hizo esto fue Karzanov, pero su algoritmo era complicado y otros autores fueron simplificando sus ideas. Para ejemplificar esta estrategia de una forma distinta, estudiaremos el algoritmo **Wave de Tarjan** y mencionaremos brevemente el algoritmo **MKM**.

## 2. Algoritmo MKM {#algoritmo-mkm}

En Dinitz, Edmonds-Karp y Ford-Fulkerson construíamos nuestro flujo bloqueante o maximal, según el caso, de a un camino aumentante por vez. Pero, ¿qué tal si pudiéramos construir varios caminos aumentantes en paralelo?

Un algoritmo anterior a Wave, llamado **MKM** por las iniciales de sus autores (Malhotra, Kumar y Maheshwari), logra esto mediante dos operaciones: *push* y *pull*.

* **Operación *push*:** En un vértice $x$, consiste en empujar cierta cantidad de flujo desde $x$ hacia **todos** sus vecinos de $\Gamma^{+}(x)$.

* **Operación *pull*:** Envía flujo **desde** todos los vecinos de $\Gamma^{-}(x)$ hacia $x$.

La cantidad de flujo enviada desde $x$ hacia $\Gamma^{+}(x)$ y desde $\Gamma^{-}(x)$ hacia $x$ son iguales, para que $x$ conserve la propiedad $\text{in}(x) = \text{out}(x)$. Sin embargo, esta operación doble *push-pull* en $x$ va a "desbalancear" a los vértices de $\Gamma^{+}(x)$ (pues tendrán $\text{in} > \text{out}$) y a los de $\Gamma^{-}(x)$ (pues tendrán $\text{in} < \text{out}$).

Para resolver esto:

1. A partir de los vértices de $\Gamma^{+}(x)$, se los va balanceando haciendo solo *push* en ellos. Esto desbalanceará a sus propios vecinos en $\Gamma^{+}$, a los cuales se les aplicará *push* sucesivamente hasta llegar a $t$.

2. Dualmente, a los vértices de $\Gamma^{-}(x)$ se les aplica solamente la operación de *pull* para balancearlos, lo cual desbalanceará a sus predecesores (con $\text{out} > \text{in}$), aplicándoles *pull* sucesivamente hasta llegar a $s$.

Lo que resulta sigue siendo un flujo, y es equivalente a haber construido muchos posibles caminos aumentantes pasando todos por $x$. Se repite este proceso hasta obtener un flujo bloqueante.

### Elección del vértice y cantidad de flujo en MKM

* **¿Cuánto flujo mandar?** Simplemente lo máximo posible, que será el mínimo entre lo máximo que podamos mandar hacia $\Gamma^{+}(x)$ con *push* y lo máximo que podamos traer desde $\Gamma^{-}(x)$ con *pull*, asegurando que $x$ quede balanceado desde el principio.

* **¿Qué vértice** $x$ **elegir?** Queremos que la serie de *pushes* desde $x$ llegue a $t$ y la serie de *pulls* llegue a $s$ sin "trabarse" en el medio. Por ejemplo, si $x$ puede mandar $5$ unidades a cada uno de sus vecinos $y, z$, pero $y$ solo puede mandar $2$ unidades a sus propios vecinos $u, v$, entonces no se podría balancear $y$ y habría un problema. Por ello, MKM revisa todos los vértices para elegir aquel que **menos capacidad** tiene de enviar flujo tanto hacia adelante como hacia atrás. De esta forma, se garantiza que todos los demás vértices tendrán al menos esa misma capacidad para absorber y transmitir el flujo.

Si bien se puede probar que MKM tiene la misma complejidad teórica que el algoritmo Wave, en la práctica demora más porque, por construcción, elige el vértice $x$ por el cual puede mandar la menor cantidad de flujo en cada paso. Tarjan lo modificó para tratar de mandar más flujo; esta modificación no utiliza *pulls* (aunque sí una variación) y trabaja con **preflujos** en lugar de flujos.

## 3. Descripción de Wave {#descripcion-wave}

En Dinitz, el invariante de las funciones parciales $g$ obtenidas durante la construcción del flujo bloqueante en un *network* auxiliar era "ser flujo", y el algoritmo se detenía cuando $g$ era bloqueante. En MKM, aunque durante un ciclo *push-pull* no se tiene un flujo, al término de cada ciclo sí se cumple, manteniendo la misma estrategia: conservar la "flujosidad" y parar cuando sea bloqueante.

En **Wave**, el invariante se invierte: el invariante es que $g$ **sea bloqueante**, y nos detenemos cuando $g$ **sea un flujo**.

### Funcionamiento mediante "olas"

1. **Inicialización:** El primer paso consiste en incrementar el valor de $g$ bloqueándolo inmediatamente. Esto se hace mandando desde $s$ hacia cada uno de sus vecinos todo el flujo que permitan las capacidades de los lados correspondientes. Es equivalente a elegir $x = s$ en MKM sin importar si se puede llegar a $t$. Esto bloquea a $g$, pero lo que queda no es un flujo: todos los vecinos de $s$ (los vértices del nivel $1$) quedan con flujo entrante y nada saliente (están desbalanceados).

2. **Balanceo hacia adelante (*Forward Balance* -** $FB$**):** Intentamos balancear los vértices desbalanceados haciendo *push* en cada uno de ellos y en sus vecinos sucesivamente hacia $t$. A esta operación Tarjan la llama *forward balance* ($FB$). Como empezamos desde $s$ y no desde el vértice de menor capacidad, puede ocurrir que no podamos balancear un vértice porque sus lados salientes se saturen. A estos vértices que no pueden enviar más flujo hacia adelante los marcamos como **"bloqueados"** ($B(x) = 1$).

3. **Balanceo hacia atrás (*Backward Balance* -** $BB$**):** ¿Qué hacemos con los vértices bloqueados y desbalanceados? Lo único posible es devolver flujo a uno o más de los vértices que les enviaron flujo originalmente. A esta operación Tarjan la denomina *backward balance* ($BB$). Esta operación siempre tiene éxito para balancear el vértice, ya que en el peor de los casos devuelve todo el flujo recibido.

### El problema de devolver flujo y la solución en olas

Si el vértice bloqueado está en el nivel $1$, le devuelve flujo a $s$ sin problemas. Pero si está en un nivel superior, al devolver flujo a sus predecesores, estos (que ya estaban balanceados) volverán a quedar desbalanceados.

Si esos predecesores devolvieran inmediatamente el flujo hacia atrás hasta $s$, se cometería un **error**, pues se rompería la propiedad de que $g$ sea bloqueante. Lo correcto es que esos vértices intenten primero redirigir las unidades devueltas hacia otros vértices disponibles hacia adelante. Solo si un vértice no puede enviar más flujo hacia adelante —es decir, si está **bloqueado**— tiene permitido ejecutar $BB(x)$.

Para coordinar esto sin saber de antemano si un vértice que recibe flujo devuelto podrá redirigirlo o no, Tarjan diseña el algoritmo para operar en **olas** (*waves*):

* **Ola hacia adelante:** Recorre los vértices en orden BFS desde $s$ hacia $t$, mandando flujo hacia los niveles siguientes e intentando balancear los vértices mediante $FB(x)$. Si no logra balancear un vértice, lo marca como bloqueado ($B(x) = 1$).

* **Ola hacia atrás:** Recorre los vértices en orden BFS inverso desde $t$ hacia $s$, devolviendo flujo mediante $BB(x)$ **únicamente** desde los vértices que están bloqueados y desbalanceados.

Los vértices que quedan desbalanceados tras recibir flujo devuelto, pero que **no están bloqueados**, simplemente esperan a la **siguiente ola hacia adelante** para intentar redirigir ese exceso de flujo. Wave alterna ciclos de *ola hacia adelante - ola hacia atrás* hasta que todos los vértices intermedios queden balanceados.

## 4. Pseudocódigo de Wave {#pseudocodigo-wave}

Al igual que en Dinic, $\Gamma^{+}(x)$ y las capacidades $c$ hacen referencia al *network* auxiliar. Además, necesitaremos:

* $D(x) = \text{in}(x) - \text{out}(x)$: el desbalanceo del vértice $x$.

* $B(x)$: booleano que vale $1$ si el vértice $x$ está bloqueado, y $0$ en caso contrario.

* $M(x)$: conjunto que registra los vértices que efectivamente le han mandado flujo a $x$.

### Inicialización

```python
g = 0
FORALL x:
  D(x) = 0
  B(x) = 0
ENDFOR

FORALL x ∈ Γ+(s):
  g(sx) = c(sx)
  D(x) += c(sx)
  D(s) -= c(sx)
  M(x) = {s}
ENDFOR
```

### Operación Forward Balance: $FB(x)$

```python
FB(x):
  WHILE (D(x) > 0) AND (Γ+(x) ≠ ∅):
    Tomar y ∈ Γ+(x)
    IF (B(y) == 1) THEN:
      Γ+(x) = Γ+(x) - {y}  # Si y está bloqueado, no tiene sentido mandarle flujo
    ELSE:
      A = Min{ D(x), c(xy) - g(xy) }
      g(xy) += A
      D(x) -= A
      D(y) += A
      M(y) = M(y) ∪ {x}
      IF (g(xy) == c(xy)) THEN:
        Γ+(x) = Γ+(x) - {y}
  ENDWHILE
  IF (D(x) > 0) THEN:
    B(x) = 1
```

### Operación Backward Balance: $BB(x)$

```python
BB(x):
  WHILE (D(x) > 0):  # Siempre tiene éxito
    Tomar y ∈ M(x)
    A = Min{ D(x), g(yx) }
    g(yx) -= A
    D(x) -= A
    D(y) += A
    IF (g(yx) == 0) THEN:
      M(x) = M(x) - {y}
  ENDWHILE
```

### Ciclo principal de las olas (Paso bloqueante)

Para evitar recorrer todos los vértices verificando si $D(x) = 0$ en cada ciclo, aprovechamos que la suma de todos los $D(x)$ en el grafo siempre es cero (cada envío de $A$ unidades resta $A$ a un vértice y suma $A$ a otro). Como $s$ es el único vértice con desbalanceo negativo, todos los vértices intermedios estarán balanceados ($D(x) = 0 \ \forall x \neq s, t$) si y solo si $D(s) + D(t) = 0$.

Notamos como "$s+1$" al primer vértice en el orden BFS posterior a $s$, y como "$t-1$" al último vértice en el orden BFS anterior a $t$.

```python
WHILE (D(s) + D(t) ≠ 0):
  # Ola hacia adelante (orden BFS)
  FOR x = "s + 1" TO "t - 1":
    IF (D(x) > 0) THEN:
      FB(x)
  ENDFOR

  # Ola hacia atrás (orden BFS inverso)
  FOR x = "t - 1" TO "s + 1":
    IF (B(x) == 1) AND (D(x) > 0) THEN:
      BB(x)
  ENDFOR
ENDWHILE

RETURN(g)
```

## 5. Complejidad de Wave {#complejidad-wave}

**Teorema:** La complejidad de Wave es $O(n^3)$.

**Demostración:**

Como Wave utiliza la técnica de los *networks* auxiliares y sabemos que puede haber a lo sumo $n$ *networks* auxiliares antes de hallar un flujo maximal, basta con demostrar que la complejidad de encontrar un flujo bloqueante en un *network* auxiliar es $O(n^2)$.

En cada llamada a $FB(x)$ o $BB(x)$, se revisan vecinos $y$ en $\Gamma^{+}(x)$ o $M(x)$, y cada revisión toma tiempo $O(1)$. Por lo tanto, la complejidad del paso bloqueante equivale a la cantidad total de veces que se examina un lado durante todas las olas. Dividimos estos procesamientos en cuatro categorías:

1. **Saturaciones (**$S$**):** Procesamientos en $FB(x)$ donde el lado $\vec{xy}$ queda saturado.

   Cuando un lado $\vec{xy}$ se satura, se elimina de $\Gamma^{+}(x)$. Para volver a usarse, primero debería des-saturarse (que $y$ le devuelva flujo a $x$), lo cual solo ocurre si $y$ se bloquea. Pero si $y$ está bloqueado, $x$ nunca volverá a mandarle flujo. Por ende, cada lado se satura a lo sumo una vez:

   $$
   S \le m
   $$

2. **Vaciados (**$V$**):** Procesamientos en $BB(x)$ donde el lado $\vec{yx}$ queda vacío ($g(\vec{yx}) = 0$).

   Un lado $\vec{yx}$ solo puede vaciarse si $x$ está bloqueado. Pero una vez que $x$ está bloqueado, $y$ nunca más podrá enviarle flujo, por lo que $\vec{yx}$ no volverá a llenarse ni a vaciarse. Así, cada lado se vacía a lo sumo una vez:

   $$
   V \le m
   $$

3. **Envíos parciales hacia adelante (**$P$**):** Procesamientos en $FB(x)$ donde el lado $\vec{xy}$ **no** queda saturado.

   En una ejecución de $FB(x)$, todos los lados revisados se saturan excepto, a lo sumo, el último (cuando $D(x)$ llega a $0$). Así, hay a lo sumo $1$ procesamiento de tipo $P$ por cada llamada a $FB(x)$.

   Por otro lado, cada ola hacia adelante ejecuta a lo sumo $n - 2$ operaciones $FB$. Además, en cada ola hacia adelante (salvo la última, donde todos los vértices quedan balanceados y el algoritmo termina), **al menos un vértice queda bloqueado**. Como los vértices nunca se desbloquean, hay a lo sumo $n$ olas hacia adelante. Por lo tanto:

   $$
   P \le (n - 2) \cdot n = O(n^2)
   $$

4. **Devoluciones parciales hacia atrás (**$Q$**):** Procesamientos en $BB(x)$ donde el lado $\vec{yx}$ **no** queda vacío.

   Análogamente a $P$, en cada llamada a $BB(x)$ hay a lo sumo un lado que no se vacía por completo. Como hay a lo sumo $n - 2$ operaciones $BB$ por ola hacia atrás, y la cantidad de olas hacia atrás es igual a la de olas hacia adelante ($O(n)$), tenemos:

   $$
   Q \le (n - 2) \cdot n = O(n^2)
   $$

Sumando todas las operaciones del paso bloqueante:

$$
\text{Complejidad paso bloqueante} = S + P + V + Q = O(m) + O(n^2) + O(m) + O(n^2) = O(n^2)
$$

Multiplicando por los $O(n)$ *networks* auxiliares, la complejidad total del algoritmo Wave es:

$$
O(n) \cdot O(n^2) = O(n^3)
$$

## 6. Push-relabel {#push-relabel}

Una variación de estas ideas, en donde en lugar de construir *networks* auxiliares se utilizan etiquetas de niveles directamente sobre el grafo (como se explicó en Dinitz), se conoce como el algoritmo **Push-relabel** (Goldberg-Tarjan).

Este enfoque es incluso más general dependiendo de la regla que se utilice para seleccionar los vértices activos y reetiquetarlos. Aunque la complejidad general del algoritmo *Push-relabel* es $O(n^2 m)$, ciertas especificaciones en el orden de procesamiento (que resultan equivalentes a la estrategia de olas de Wave) permiten alcanzar la misma cota de $O(n^3)$.
