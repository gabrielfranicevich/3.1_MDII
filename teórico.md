# teórico

## Contents

1. [Grafos y coloración](<#grafos-y-coloración>)
    * [Baby Brooks (08)](<#baby-brooks>)
2. [Network flows](<#network-flows>)
    * [Ford-Fulkerson](<#ford-fulkerson>)
    * [Max flow Min cut (07)](<#max-flow-min-cut>)
      * [ƒ flujo maximal => ∃corte minimal 𝑆 : 𝓋(ƒ) = Cap(𝑆) (07)](<#ƒ-flujo-maximal--corte-minimal-𝑆--𝓋ƒ--cap𝑆-de-final>)
    * [Edmonds-Karp (01, 02, 03)](<#edmonds-karp>)
      * [Lema de las Distancias (02, 03)](<#lema-de-las-distancias>)
        * [𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥) (03)](<#𝑑𝑘𝑥--𝑑𝑘1𝑥>)
        * [𝑏𝑘(𝑥) ≤ 𝑏𝑘+1(𝑥) (02)](<#𝑏𝑘𝑥--𝑏𝑘1𝑥>)
      * [Complejidad de Edmonds-Karp (01)](<#complejidad-de-edmonds-karp>)
    * [Dinitz (04)](<#dinitz>)
      * [Complejidad de Dinitz (04)](<#complejidad-de-dinitz>)
    * [Wave (Tarjan) (05)](#wave-tarjan)
      * [Complejidad de Wave (05)](<#complejidad-de-wave>)
3. [Bipartitos y matchings](<#bipartitos-y-matchings>)
    * [Teorema de Hall (09)](<#teorema-de-hall>)
    * [Matrimonio de Kőnig (10)](<#matrimonio-de-kőnig>)
    * [Algoritmo Húngaro (11)](<#algoritmo-húngaro>)
      * [Complejidad del algoritmo Húngaro (11)](<#complejidad-del-algoritmo-húngaro>)
4. [Complejidad Computacional](<#complejidad-computacional>)
    * [3-color (15)](#3-color-es-np-completo)
5. [Códigos](<#códigos>)
    * [Cota de Hamming (12)](<#cota-de-hamming>)
    * [Distancia Mínima y Matriz de Chequeo (13)](<#teorema-de-la-distancia-mínima-y-la-matriz-de-chequeo>)
    * [Cíclicos (14)](<#códigos-cíclicos>)
      * [Polinomio Generador (14)](<#polinomio-generador>)
6. listas de temas
    * A:
      * [01. Complejidad de Edmonds-Karp](<#complejidad-de-edmonds-karp>)
      * [02. 𝑏𝑘(𝑥) ≤ 𝑏𝑘+1(𝑥)](<#𝑏𝑘𝑥--𝑏𝑘1𝑥>)
      * [03. 𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥)](<#𝑑𝑘𝑥--𝑑𝑘1𝑥>)
      * [04. Complejidad de Dinitz](<#complejidad-de-dinitz>)
      * [05. Complejidad de Wave](<#complejidad-de-wave>)
    * B:
      * [15. 3-color (15)](#3-color-es-np-completo)
    * C:
      * [07. Max flow Min cut](<#max-flow-min-cut>)
      * [08. Baby Brooks](<#baby-brooks>)
      * [09. Teorema de Hall](<#teorema-de-hall>)
      * [10. Matrimonio de König](<#matrimonio-de-kőnig>)
      * [11. Complejidad del Húngaro](<#complejidad-del-algoritmo-húngaro>)
      * [12. Cota de Hamming](<#cota-de-hamming>)
      * [13. Distancia Mínima y Matriz de Chequeo](<#teorema-de-la-distancia-mínima-y-la-matriz-de-chequeo>)
      * [14. Polinomio Generador](<#polinomio-generador>)

---

## Grafos y Coloración

### Grafos

Def. 1 Una grafo es un par 𝐺 = (𝑉, 𝐸) con
  𝑉 (vértices) un conjunto cualquiera
  𝐸 (aristas) ⊆ {𝐴 ⊆ 𝑉 : |𝐴| = 2} = {{u,v} : u,v ∈ 𝑉}

Def. 2 El vecindario de un vértice 𝑣 en un grafo 𝐺 = (𝑉, 𝐸) es el conjunto
  Γ(𝑣) ≝ {𝑢 ∈ 𝑉 : {𝑣, 𝑢} ∈ 𝐸}.
  -> si 𝑢 ∈ Γ(𝑣), entonces 𝑢 es vecino de 𝑣.

Def. 3 El grado de un vértice (𝑑(𝑣)) es su cantidad de vecinos.
  𝑑(𝑣) ≝ |{𝑢 ∈ 𝑉 : {𝑣,𝑢} ∈ 𝐸}| = |{𝑒 ∈ 𝐸 : 𝑣 ∈ 𝑒}|

Def. 4 Sea 𝐺 = (𝑉, 𝐸) un grafo, su grado mínimo/máximo es el grado del
vértice con menor/mayor grado.
  Δ(𝐺) = max{𝑑(𝑣) : 𝑣 ∈ 𝑉}
  δ(𝐺) = min{𝑑(𝑣) : 𝑣 ∈ 𝑉}

Def. 5 Un grafo 𝐺 es regular sii δ(𝐺) = Δ(𝐺).

Def. 6 Un camino entre 𝑢 y 𝑣 es una secuencia de vértices
𝑧₀ … 𝑧ₜ : 𝑧₀ = 𝑢, 𝑧ₜ = 𝑣, 𝑧ᵢ 𝑧₊₁ ∈ 𝑉 ∀i ∈ {i ∈ ℕ₀ : i < 𝓉}

𝑢 ~ 𝑣 ≡ ∃ camino entre 𝑢 y 𝑣
  -> relación de equivalencia (cumple reflexividad, transitividad, simetría)

Def. 7 Una componente conexa (𝐶) de un grafo 𝐺 = (𝑉, 𝐸)
es el subgrafo inducido por una clase de equivalencia de 𝑉 bajo la relación ~.
([𝑣] = {𝑢 ∈ 𝑉 : 𝑢 ~ 𝑣})
  -> 𝐶 = [𝑣] (∃𝑣 ∈ 𝑉) ⊆ 𝑉

Def. 8 Un grafo 𝐺 = (𝑉, 𝐸) es conexo si tiene una sola componente conexa.
  -> (𝐶 = 𝑉 = [𝑣] ∀𝑣 ∈ 𝑉)

### Coloración

Def. 9 Un coloreo propio de 𝐺 = (𝑉, 𝐸) con 𝑘 colores es una función
  𝐶 : 𝑉 ↦→ 𝐴 (|𝐴| = 𝑘)
  𝑢𝑣 ∈ 𝐸 ⇒ 𝐶(𝑢) ≠ 𝐶(𝑣).

Def. 10 El número cromático de un grafo 𝐺 = (𝑉, 𝐸) es
𝛸(𝐺) = min{𝑘 : 𝐺 tiene un coloreo propio con 𝑘 colores}

Alg. 1 Greedy para Colorear un Grafo

* Funcionamiento:
  * Dado un orden de los vértices de 𝐺 (𝑣_1, …, 𝑣_𝑛)
    1. 𝐶(𝑣_1) = 1.
    2. Para 𝑖 : (2 ≤ 𝑖 ≤ 𝑛)
      𝐶(𝑣_𝑖) = min{𝑘 ∈ ℕ : 𝐶(𝑣_𝑗) ≠ 𝑘 (∀𝑗 < 𝑖 : 𝑣_𝑗 ∈ Γ(𝑣_𝑖))}
* Complejidad
  * Σ(𝑑(𝑣)) -> para cada vértice 𝑣, se miran sus vecinos
  * Σ(𝑑(𝑣)) = 2𝑚 -> por apreton de manos, las aristas se cuentan dos veces
  * 𝑂(𝑚) = 𝑂(𝐸)
  * En no conexos: 𝑂(𝑛 + 𝑚)
    porque se cuentan vertices aunque no haya aristas, domina el mayor

#### Baby Brooks

Teo. 1 Si G = (𝑉, 𝐸) es un grafo conexo no regular, entonces χ(G) ≤ ∆(G).

Prueba:  
Como 𝐺 no es regular, sabemos que el grado mínimo es estrictamente menor que el
grado máximo (𝛿(𝐺) < Δ(𝐺)).  
Elegimos un vértice 𝑣 tal que su grado sea mínimo: 𝑑(𝑣) = 𝛿(𝐺).

1. Construcción del orden de coloreo
    * Hacemos BFS partiendo desde 𝑣
    * como 𝐺 es conexo, alcanzamos todos los vértices del grafo
    * En un árbol BFS, cada vértice es "descubierto" por un padre.
2. Hacemos Greedy
    * Usamos el orden inverso al orden en que
    los vértices fueron incorporados al árbol de búsqueda
    * el vértice raíz 𝑣 será el último en ser coloreado (𝑣_𝑛 = 𝑣)
    * cuando vayamos a colorear un vértice cualquiera (excepto 𝑣),
    su "padre" en el árbol BFS todavía no estará coloreado
3. Tomemos un vértice 𝑢 distinto de 𝑣
    * Cuando el algoritmo llega a 𝑢, su "padre" todavía no fue coloreado
    * De sus vecinos, a lo sumo 𝑑(𝑢) - 1 tienen color
    * Como su grado máximo es Δ(𝐺), a lo sumo le roban Δ(𝐺) - 1 colores
    * Hay al menos un color disponible en el conjunto de Δ(𝐺) colores
4. LLegamos a 𝑣
    * Es el último, todos sus vecinos están coloreados
    * le podrían robar hasta 𝑑(𝑣) colores
    * Como 𝑑(𝑣) = 𝛿(𝐺) y sabemos que 𝛿(𝐺) < Δ(𝐺)
    * Sus vecinos le roban estrictamente menos de Δ(𝐺) colores
    * Sobra al menos un color para colorear a 𝑣
5. Conclusión
    * Encontramos un coloreo propio del grafo usando como máximo Δ(𝐺) colores
    * lo que demuestra que χ(G) ≤ ∆(G) ■

---

## Network flows

Def. 11 Un grafo 𝐺 = (𝑉, 𝐸) con 𝑉 un conjunto y 𝐸 ⊆ 𝑉 × 𝑉 se llama dirigido.
Usaremos →𝑣𝑢 para denotar (𝑣, 𝑢) donde (𝑣, 𝑢) ∈ 𝐸.
Observe que →𝑣𝑢 ≠ →𝑢𝑣.

Def. 12 Un network es una 3-upla (𝑉, 𝐸, 𝑐) donde:
(𝑉, 𝐸) es un grafo dirigido
𝑐 : 𝐸 → ℝ⁺ es una función que denota la capacidad de cada uno de los lados.

Def. 13 (Vecinos)
  Γ⁺(𝑣) ≝ {𝑢 : →𝑣𝑢 ∈ 𝐸}
  Γ⁻(𝑣) ≝ {𝑢 : →𝑢𝑣 ∈ 𝐸}

Def. 14 Sea ƒ : 𝐸 → ℝ⁺, 𝑣 ∈ 𝑉
outƒ(𝑣) ≝ ∑_{𝑢∈Γ⁺(𝑣)} ƒ(→𝑣𝑢)
inƒ(𝑣) ≝ ∑_{𝑢∈Γ⁻(𝑣)} ƒ(→𝑢𝑣)

Def. 15 Dada 𝑁 = (𝑉, 𝐸, 𝑐) una network y 𝓈, 𝓉 ∈ 𝑉,
una función ƒ : 𝐸 → ℝ es un flujo sii

1. 0 ≤ ƒ(→𝑣𝑢) ≤ 𝑐(→𝑣𝑢) ∀→𝑣𝑢 ∈ 𝐸 (feasability)
2. inƒ(𝑣) = outƒ(𝑣) ∀𝑣 ∈ 𝑉 − {𝓈, 𝓉} (conservación)
3. outƒ(𝓈) ≥ inƒ(𝓈) (𝓈 de source) (inƒ(𝓈) = 0)
4. inƒ(𝓉) ≥ outƒ(𝓉) (𝓉 de target) (outƒ(𝓉) = 0)

Prop. 1 Sean 𝑁 = (𝑉, 𝐸, 𝑐) una network y ƒ un flujo en 𝑁 de 𝓈 a 𝓉.
  outƒ(𝓈) = inƒ(𝓉)

Def. 16 Sean 𝑁 = (𝑉, 𝐸, 𝑐) una network y ƒ un flujo en 𝑁 de 𝓈 a 𝓉.
el valor de ƒ es:
  𝓋(ƒ) = outƒ(𝓈) - inƒ(𝓈) = inƒ(𝓉) - outƒ(𝓉)

Teo. 2 Sean 𝑁 = (𝑉, 𝐸, 𝑐) una network y ƒ un flujo en 𝑁.
Luego 𝓋(ƒ) = inƒ(𝓉) - outƒ(𝓉)

Def. 17 Sea 𝑁 = (𝑉, 𝐸, 𝑐) una network y ƒ un flujo en N de 𝓈 a 𝓉.
ƒ se dice maximal sii 𝓋(ƒ) ≥ 𝓋(𝑔) ∀𝑔 flujo en 𝑁 de 𝓈 a 𝓉.

Def. cmp. (Corchete de Iverson) Sea 𝜁 una expresión booleana.
[𝜁] devuelve 1 si la condición es verdadera y 0 si es falsa.

Def. 18 Sean ƒ : 𝐸 → ℝ y 𝐴, 𝐵 ⊆ 𝑉. Definimos
ƒ(𝐴, 𝐵) = ∑︁_{→𝑣𝑢∈𝐸}[𝑣 ∈ 𝐴] [𝑢 ∈ 𝐵]𝑔(→𝑣𝑢)

Lema 1 Sean ƒ, 𝑔 funciones sobre los lados
con ƒ(→𝑣𝑢) ≤ 𝑔(→𝑣𝑢) ∀→𝑣𝑢 ∈ 𝐸.
Entonces ƒ(𝐴, 𝐵) ≤ 𝑔(𝐴, 𝐵) ∀𝐴, 𝐵 ⊆ 𝑉.

Teo. 3 (Probar que flujo es maximal)
Sea ƒ un flujo sobre 𝑁 = (𝐺, 𝐸, 𝑐) de 𝓈 a 𝓉.
Entonces 𝓋(ƒ) = 𝑐({𝓈} , 𝑉) ⇒ ƒ es maximal.

Alg. 2 Greedy para flujos

* Funcionamiento
  1. Inicializa el flujo en ƒ(→𝑥𝑦) = 0 ∀→𝑥𝑦 ∈ 𝐸.
  2. Encuentra un camino no saturado de 𝓈 a 𝓉
      * es decir, un camino con flujo menor a la capacidad en cada lado.
      * Si el camino no existe, termina.
  3. Suma al flujo de cada lado en el camino el valor máximo que puede
    sumarse sin sobrepasar la capacidad de ningún lado.
  4. Regresa a 2.
* Propiedades:
  * Siempre devuelve un flujo
  * Su complejidad 𝑂(𝑚^2), hace 𝑂(𝑚) iteraciones y cada iteración es 𝑂(𝑚).
  * No siempre devuelve flujo maximal.

### Ford-Fulkerson

Def. 19 Un ƒ-camino aumentante es una sucesión de vertices 𝑣_0, …, 𝑣_𝑟
tq 𝑣_0 = 𝓈, 𝑣_𝑟 = 𝓉, ∀𝑖 ∈ [0, 𝑟 − 1] y se cumple una de 2:

* →𝑣_𝑖𝑣_{𝑖+1} ∈ 𝐸 y ƒ(→𝑣_𝑖𝑣_{𝑖+1}) < 𝑐(→𝑣_𝑖𝑣_{𝑖+1})
* ←𝑣_𝑖𝑣_{𝑖+1} ∈ 𝐸 y ƒ(←𝑣_𝑖𝑣_{𝑖+1}) > 0

Teo. 4 Si ƒ es un flujo, 𝓋(ƒ) = 𝚟 y aumentamos ƒ con un ƒ-camino aumentante
con 𝜀, entonces lo que queda sigue siendo un flujo y su valor es 𝚟 + 𝜀.

Alg. 3 Ford-Fulkerson

* Funcionamiento
  1. Inicializa ƒ(→𝑣𝑢) = 0 ∀→𝑣𝑢 ∈ 𝐸.
  2. Mientras exista un ƒ-camino aumentante desde 𝓈 hasta 𝓉:
      1. Encuentra ese camino 𝑣_0…𝑣_𝑟 (con 𝑣_0 = 𝓈, 𝑣_𝑟 = 𝓉).
      2. Calcula el incremento para cada arista en el camino (𝜀_𝑖)
          * si se usa un lado forward, el margen es lo que falta para saturarlo
            * 𝑐(→𝑣_𝑖𝑣_{𝑖+1}) - ƒ(→𝑣_𝑖𝑣_{𝑖+1})
          * si se usa un lado backward, el margen es lo que se ha usado
            * ƒ(←𝑣_𝑖𝑣_{𝑖+1})
      3. Determina el cuello de botella 𝜀 = min{𝜀_𝑖 ∀𝑖 : 0 < 𝑖 < 𝑟}.
      4. Actualiza ƒ sumando 𝜀 a cada arista en el camino.
  3. Si no existe un ƒ-camino aumentante, el flujo actual es el máximo posible.
* Propiedades
  * no siempre termina
  * si termina devuelve un flujo maximal

Def 20. Sea 𝑁 = (𝑉, 𝐸, 𝑐) una network. Un corte es un conjunto 𝑆 ⊆ 𝑉
tal que 𝓈 ∈ 𝑆 sii 𝓉 ∉ 𝑆.

Lema 3 Sea ƒ un flujo sobre una network 𝑁 con un corte 𝑆
entonces 𝓋(ƒ) = ƒ(𝑆, 𝑆ᶜ) - ƒ(𝑆ᶜ, 𝑆)
𝑆ᶜ = 𝑉 - 𝑆

Def 21. La capacidad de un corte 𝑆 se define como Cap(𝑆) = 𝑐(𝑆, 𝑆ᶜ)
la suma de las capacidades de los vértices que van desde el corte hacia fuera.

Def 22. Un corte 𝑆 es minimal si Cap(𝑆) ≤ Cap(𝑆') ∀𝑆' ∈ 𝑉.

### Max Flow, Min Cut

Teo. 5 Sea 𝑆 un corte sobre una network 𝑁 = (𝑉, 𝐸, 𝑐) con flujo ƒ.
Entonces:

* 𝓋(ƒ) ≤ Cap(𝑆).
* ƒ es maximal sii existe un corte minimal 𝑆 tal que 𝓋(ƒ) = Cap(𝑆)

#### Prueba Max Flow Min Cut

Lo vamos a hacer en 3 partes:

##### 𝓋(ƒ) ≤ Cap(𝑆)

Sabemos que 𝓋(ƒ) = ƒ(𝑆, 𝑆ᶜ) - ƒ(𝑆ᶜ, 𝑆)

ƒ(𝑆ᶜ, 𝑆) = ∑︁_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∈ 𝑆ᶜ] [𝑢 ∈ 𝑆] ƒ(→𝑣𝑢)

∵ ƒ(→𝑣𝑢) ≥ 0 ∀→𝑣𝑢 ∈ 𝐸
  => ƒ(𝑆ᶜ, 𝑆) ≥ 0

=> 𝓋(ƒ) = ƒ(𝑆, 𝑆ᶜ) - ƒ(𝑆ᶜ, 𝑆) ≤ ƒ(𝑆, 𝑆ᶜ) ≤ 𝑐(𝑆, 𝑆ᶜ) = Cap(𝑆) ■

##### ∃ corte minimal 𝑆 : 𝓋(ƒ) = Cap(𝑆) => ƒ es maximal

Sean ƒ un flujo y 𝑆 un corte con 𝓋(ƒ) = Cap(𝑆)
Sea 𝑔 un flujo cualquiera

∵ 𝓋(𝑔) ≤ Cap(𝑆) y 𝓋(ƒ) = Cap(𝑆)
=> 𝓋(𝑔) ≤ 𝓋(ƒ)

como 𝑔 un flujo cualquiera => 𝓋(𝑔) ≤ 𝓋(ƒ) ∀flujo 𝑔
=> ƒ es maximal ■

De regalo: 𝑆 es minimal
Sea 𝑇 un corte cualquiera,
∵ Cap(𝑇) ≥ 𝓋(ƒ) = Cap(𝑆)
=> Cap(𝑇) ≥ Cap(𝑆) ∀corte 𝑇
=> 𝑆 es minimal ■

##### ƒ flujo maximal => ∃corte minimal 𝑆 : 𝓋(ƒ) = Cap(𝑆) (de final)

Sean ƒ un flujo maximal

1. construyamos un corte 𝑆 en base a ƒ
    * 𝑆 = 𝓈 ∪ {𝑣 ∈ 𝑉 : ∃ƒ-camino aumentante entre 𝓈 y 𝑣}
      * 𝓉 ∉ 𝑆
        * ∵ ∃ƒ-camino aumentante entre 𝓈 y 𝑣
        * => (𝓉 ∈ 𝑆 => ∃ƒ-camino aumentante entre 𝓈 y 𝓉)
        * => (𝓉 ∈ 𝑆 => se puede aumentar el flujo ƒ)
        * => 𝓉 ∈ 𝑆 contradice ƒ maximal
        * => 𝓉 ∉ 𝑆
    * ∵ 𝓉 ∉ 𝑆 y 𝓈 ∈ 𝑆 => 𝑆 es un corte

2. ∵ 𝑆 es un corte => 𝓋(ƒ) = ƒ(𝑆, 𝑆ᶜ) - ƒ(𝑆ᶜ, 𝑆)
    1. Considerando el primer termino (Aristas que salen de 𝑆):
        * ƒ(𝑆, 𝑆ᶜ) = ∑︁_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∈ 𝑆] [𝑢 ∉ 𝑆] ƒ(→𝑣𝑢)
          * sea →𝑣𝑢 ∈ 𝐸 con 𝑣 ∈ 𝑆 y 𝑢 ∉ 𝑆
            * ∵ 𝑣 ∈ 𝑆 => ∃ƒ-camino aumentante entre 𝓈 y 𝑣
            * ∵ 𝑢 ∉ 𝑆 => ¬∃ƒ-camino aumentante entre 𝓈 y 𝑢
            * ∵ ∃→𝑣𝑢
              * ƒ(→𝑣𝑢) < 𝑐(→𝑣𝑢) => ∃ƒ-camino aumentante de 𝓈 a 𝑢 con 𝓈…𝑣𝑢
              * ∵ sabemos que ¬∃ƒ-camino aumentante de 𝓈 a 𝑢 con 𝓈…𝑣𝑢
                * => ƒ(→𝑣𝑢) ≮ 𝑐(→𝑣𝑢) => ƒ(→𝑣𝑢) = 𝑐(→𝑣𝑢)
                * => ƒ(→𝑣𝑢) = 𝑐(→𝑣𝑢) (∀→𝑣𝑢 ∈ 𝐸, 𝑣 ∈ 𝑆 𝑢 ∉ 𝑆)
        * ƒ(𝑆, 𝑆ᶜ) = ∑_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∈ 𝑆] [𝑢 ∉ 𝑆] ƒ(→𝑣𝑢)
          = ∑_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∈ 𝑆] [𝑢 ∉ 𝑆] 𝑐(→𝑣𝑢) = Cap(𝑆)
          * ƒ(𝑆, 𝑆ᶜ) = Cap(𝑆)
    2. Considerando el segundo termino (Aristas que entran a 𝑆):
        * ƒ(𝑆ᶜ, 𝑆) = ∑_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∉ 𝑆] [𝑢 ∈ 𝑆] ƒ(→𝑣𝑢)
          * Sea →𝑢𝑣 ∈ 𝐸 con 𝑣 ∉ 𝑆 y 𝑢 ∈ 𝑆
            * ∵ 𝑢 ∉ 𝑆 => ¬∃ƒ-camino aumentante entre 𝓈 y 𝑣
            * ∵ 𝑣 ∈ 𝑆 => ∃ƒ-camino aumentante entre 𝓈 y 𝑢
            * ∵ ∃←𝑣𝑢
              * ƒ(→𝑢𝑣) > 0 => ∃ƒ-camino aumentante de 𝓈 a 𝑢 con 𝓈…(←𝑣𝑢)
                * ∵ sabemos que ¬∃ƒ-camino aumentante de 𝓈 a 𝑢
                  * => ƒ(→𝑢𝑣) ≯ 0 => ƒ(→𝑢𝑣) = 0
                  * => ƒ(→𝑢𝑣) = 0 (∀→𝑢𝑣 ∈ 𝐸, 𝑢 ∈ 𝑆 𝑣 ∉ 𝑆)
        * ƒ(𝑆ᶜ, 𝑆) = ∑_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∉ 𝑆] [𝑢 ∈ 𝑆] ƒ(→𝑣𝑢) = 0
          * ƒ(𝑆ᶜ, 𝑆) = 0
    3. ∵ ƒ(𝑆, 𝑆ᶜ) = Cap(𝑆), ƒ(𝑆ᶜ, 𝑆) = 0 y 𝓋(ƒ) = ƒ(𝑆, 𝑆ᶜ) - ƒ(𝑆ᶜ, 𝑆)
    => 𝓋(ƒ) = Cap(𝑆) - 0 = Cap(𝑆) ■

### Edmonds-Karp

Def.  Un lado es (o se vuelve) crítico en un paso dado de Edmonds-Karp si
es usado para actualizar el flujo y satisface una de estas:

1. se usa en modo forward y se satura
2. se usa en modo backward y se vacía.

Cuando un lado se vuelve crítico, no puede volver a usarse (en esa dirección)
en el próximo paso;
si se satura, no puede enviarse más flujo forward;
si se vacía, no puede enviarse más flujo backward.

Def.  Dados 𝑣, 𝑢 ∈ 𝑉 en una network con flujo ƒ,
definimos 𝑑_ƒ(𝑣, 𝑢) la distancia entre esos vértices relativa al flujo ƒ
como sigue:

𝑑_ƒ(𝑣, 𝑢) = 0 si 𝑢 = 𝑣
𝑑_ƒ(𝑣, 𝑢) = ∞ si no existe ƒ-c.a. de 𝑣 a 𝑢
𝑑_ƒ(𝑣, 𝑢) = min {longitudes de los ƒ-c.a. de 𝑣 a 𝑢}

#### Lema de las Distancias

Def. Sea ƒ𝑘 el flujo obtenido en la iteración 𝑘 de Edmonds-Karp.

Def. 𝑑𝑘(𝑥) = 𝑑ƒ𝑘(𝓈, 𝑥), 𝑏𝑘(𝑥) = 𝑑ƒ𝑘(𝑥, 𝓉)

Teo. Para todo 𝑥 ∈ 𝑉,

1) 𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥)
2) 𝑏𝑘(𝑥) ≤ 𝑏𝑘+1(𝑥)

Es decir, la longitud de los ƒ-c.a. que se obtienen
iterativamente con Edmonds-Karp nunca decrece.

Prueba:

##### 𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥)

lo voy a hacer por el absurdo,

Sea 𝐴 = {𝑥 ∈ 𝑉 : 𝑑𝑘+1(𝑥) < 𝑑𝑘(𝑥)},
Sup. 𝐴 ≠ ∅.

Tomo 𝑥 ∈ 𝐴 : 𝑑𝑘+1(𝑥) = min{𝑑𝑘+1(𝑦) : 𝑦 ∈ 𝐴}
(𝑥 es el elemento de 𝐴 con menor 𝑑𝑘+1)

∵ 𝑑𝑘+1(𝑥) < 𝑑𝑘(𝑥) ≤ ∞
=> 𝑑𝑘+1(𝑥) < ∞ (≠∞)
=> ∃ƒ𝑘+1-c.a. entre 𝓈 y 𝑥
  (en el paso 𝑘+1 hay un ƒ-c.a. entre 𝓈 y 𝑥)

∵ 𝑑𝑘+1(𝓈) = 𝑑𝑘(𝓈) = 0 => 𝓈 ∉ 𝐴 => 𝑥 ≠ 𝓈
=> tiene un elemento inmediatamente anterior en cualquier camino

Sea 𝑝𝑘+1 el menor ƒ𝑘+1-c.a. entre 𝓈 y 𝑥, de longitud 𝑑𝑘+1(𝑥)
formando un camino 𝑝𝑘+1, de la forma 𝓈…𝑧𝑥,
con 𝑧 el elemento inmediatamente anterior de 𝑥

∵ 𝑝𝑘+1 es el camino de longitud mínima entre 𝓈 y 𝑥
=> también es el camino de longitud mínima entre 𝓈 y todos los vertices intermedios
=> 𝑑𝑘+1(𝑥) = 𝑑𝑘+1(𝑧) + 1 => 𝑑𝑘+1(𝑧) < 𝑑𝑘+1(𝑥)

∵ 𝑑𝑘+1(𝑧) < 𝑑𝑘+1(𝑥) y 𝑥 es el elemento de 𝐴 con menor 𝑑𝑘+1
=> 𝑧 ∉ 𝐴 => 𝑑𝑘(𝑧) ≤ 𝑑𝑘+1(𝑧)

∵ 𝑑𝑘+1(𝑧) < 𝑑𝑘+1(𝑥), 𝑑𝑘+1(𝑥) < ∞ y 𝑑𝑘(𝑧) ≤ 𝑑𝑘+1(𝑧)
=> 𝑑𝑘(𝑧) < ∞
=> ∃ƒ𝑘+1-c.a. 𝑝𝑘 de longitud mínima entre 𝓈 y 𝑧 de la forma 𝓈…𝑧

* si →𝑧𝑥 es un lado:
  * En principio, podríamos poner 𝑥 al final de 𝑝𝑘, pero si hago eso queda:
    * 𝑑𝑘(𝑥) ≤ 𝑑𝑘(𝑧) + 1 ≤ 𝑑𝑘+1(𝑧) = 𝑑𝑘+1(𝑥) => 𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥)
    * lo que es absurdo porque 𝑥 ∈ 𝐴
    * => no se puede poner 𝑥 al final de 𝑝𝑘 => ƒ𝑘(→𝑧𝑥) = 𝑐(→𝑧𝑥)
  * Pero en 𝑝𝑘+1 está el lado →𝑧𝑥
    * => para pasar de ƒ𝑘 a ƒ𝑘+1 lo usé backwards
    * => ∃c.a. de longitud mínima (por EK) 𝑝'𝑘 de forma 𝓈…←𝑥𝑧…𝓉
    * => 𝑑𝑘(𝑧) = 𝑑𝑘(𝑥) + 1
    * ∵ 𝑑𝑘(𝑧) = 𝑑𝑘(𝑥) + 1 > 𝑑𝑘+1(𝑥) + 1 = 𝑑𝑘+1(𝑧) + 2 ≥ 𝑑𝑘(𝑧) + 2
    * => 𝑑𝑘(𝑧) > 𝑑𝑘(𝑧) + 2 (ABSURDO)
* si →𝑥𝑧 es un lado:
  * En principio, podríamos poner 𝑥 al final de 𝑝𝑘, pero si hago eso queda:
    * 𝑑𝑘(𝑥) ≤ 𝑑𝑘(𝑧) + 1 ≤ 𝑑𝑘+1(𝑧) + 1 = 𝑑𝑘+1(𝑥) => 𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥)
    * lo que es absurdo porque 𝑥 ∈ 𝐴
    * => no se puede poner 𝑥 al final de 𝑝𝑘 => ƒ𝑘(→𝑥𝑧) = 0
  * Pero en 𝑝𝑘+1 está el lado ←𝑧𝑥
    * => para pasar de ƒ𝑘 a ƒ𝑘+1 lo usé forwards
    * => ∃c.a. de longitud mínima (por EK) 𝑝'𝑘 de forma 𝓈…𝑥𝑧…𝓉
    * => 𝑑𝑘(𝑧) = 𝑑𝑘(𝑥) + 1
    * ∵ 𝑑𝑘(𝑧) = 𝑑𝑘(𝑥) + 1 > 𝑑𝑘+1(𝑥) + 1 = 𝑑𝑘+1(𝑧) + 2 ≥ 𝑑𝑘(𝑧) + 2
    * => 𝑑𝑘(𝑧) > 𝑑𝑘(𝑧) + 2 (ABSURDO)

En ambos casos llego a un absurdo,
y este absurdo viene de suponer que 𝐴 ≠ ∅ => 𝐴 = ∅ y
Finalmente 𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥)

##### 𝑏𝑘(𝑥) ≤ 𝑏𝑘+1(𝑥)

Sea 𝐴 = {𝑦 ∈ 𝑉 ∶ 𝑏𝑘+1(𝑦) < 𝑏𝑘(𝑦)} },

Por el absurdo, supongamos que 𝐴 ≠ ∅.

Entonces, tomo 𝑥 ∈ 𝐴 : 𝑏𝑘+1(𝑥) = min{𝑏𝑘+1(𝑦) : 𝑦 ∈ 𝐴}
(𝑥 es el elemento de 𝐴 con menor 𝑏𝑘+1)

∵ 𝑏𝑘(𝓉) = 𝑏𝑘+1(𝓉) = 0
  => 𝓉 ∉ 𝐴
  => 𝑥 ≠ 𝓉
∵ 𝑥 ∈ 𝐴
  => 𝑏𝑘+1(𝑥) < 𝑏𝑘(𝑥) ≤ ∞
  => ∃ƒ𝑘+1-c.a. entre 𝑥 y 𝓉

Sea 𝑃𝑘+1 un ƒ𝑘+1-c.a. de longitud mínima desde 𝑥 hasta 𝓉
Sea 𝑧 vértice inmediatamente posterior a 𝑥 en dicho camino (𝑃𝑘+1 ∶ 𝑥𝑧…𝓉).

∵ 𝑃𝑘+1 es un ƒ𝑘+1-c.a. de longitud mínima
  => 𝑏𝑘+1(𝑧) = 𝑏𝑘+1(𝑥) − 1.

∵ 𝑏𝑘+1(𝑧) < 𝑏𝑘+1(𝑥) y 𝑥 es el mínimo en 𝐴
  => 𝑧 ∉ 𝐴.
  => su distancia no disminuyó entre los pasos 𝑘 y 𝑘 + 1:
    𝑏𝑘(𝑧) ≤ 𝑏𝑘+1(𝑧).

Por hipótesis: (𝑥 ∈ 𝐴) 𝑏𝑘(𝑥) > 𝑏𝑘+1(𝑥)
∵ 𝑏𝑘+1(𝑧) = 𝑏𝑘+1(𝑥) − 1
  => 𝑏𝑘(𝑥) > 𝑏𝑘+1(𝑧) + 1
∵ (𝑧 ∉ 𝐴) 𝑏𝑘(𝑧) ≤ 𝑏𝑘+1(𝑧)
  => 𝑏𝑘(𝑥) > 𝑏𝑘+1(𝑧) + 1 ≥ 𝑏𝑘(𝑧) + 1
  => 𝑏𝑘(𝑥) > 𝑏𝑘(𝑧) + 1

∵ 𝑏𝑘(𝑥) > 𝑏𝑘(𝑧) + 1
  => →𝑥𝑧 no estaba disponible en el paso 𝑘, de lo contrario,
    la distancia mínima 𝑏𝑘(𝑥) sería a lo sumo 𝑏𝑘(𝑧) + 1.

Pero, →𝑥𝑧 sí está disponible en paso 𝑘+1 (pues 𝑧 es el sucesor de 𝑥 en 𝑃𝑘+1).
La única forma de que un arco no disponible en el paso 𝑘 se vuelva disponible
en el paso 𝑘 + 1 es que el flujo haya sido modificado en la dirección opuesta
(→𝑧𝑥) durante el aumento de flujo de ƒ𝑘 a ƒ𝑘+1.

PUNTO CLAVE:
El camino aumentante usado para pasar de ƒ𝑘 a ƒ𝑘+1 debió pasar en dirección →𝑧𝑥.
Como EK usa siempre el camino aumentante de longitud mínima (BFS),
el camino que se usó en el paso 𝑘 para ir de 𝓈 a 𝓉 y que contenía a →𝑧𝑥
era un camino de distancia mínima.
=> en es camino, 𝑧 estaba a una unidad más de distancia de 𝓉 que 𝑥:
𝑏𝑘(𝑧) = 𝑏𝑘(𝑥) + 1.

Contradicción final Si sustituimos este resultado en nuestra desigualdad:
𝑏𝑘(𝑥) > 𝑏𝑘(𝑧)+1 = (𝑏𝑘(𝑥)+1)+1 = 𝑏𝑘(𝑥)+2
Esto nos lleva a la contradicción 0 > 2.
Por lo tanto, la suposición de que el conjunto 𝐴 no es vacío es falsa.
Concluimos que 𝑏𝑘(𝑥) ≤ 𝑏𝑘+1(𝑥) para todo vértice 𝑥 en cada iteración. ■

#### Complejidad de Edmonds-Karp

Teo.  Edmonds-Karp siempre termina y opera con complejidad 𝑂(𝑛𝑚^2).

𝑛 = |𝑉|, 𝑚 = |𝐸|

Prueba:

EK encuentra un camino aumentante de 𝓈 a 𝓉 y
aumenta el flujo a lo largo de este,
esto lo hace hasta que no encuentra más caminos aumentantes.

Por lo tanto, su complejidad viene dada por:
  (cmp(encontrar c.a.) + cmp(aumentar flujo)) * cantidad de c.a.

∵ BFS solo añade a la cola vértices no descubiertos
=> el camino tiene a lo sumo 𝑛 vértices
∵ hay que recorrerlo 1 o 2 veces dependiendo de implementación
=> cmp(aumentar flujo) = 𝑂(𝑛)

cmp(encontrar c.a.) = cmp(BFS) = 𝑂(𝑚)

=> cmp(EK) = (𝑂(𝑛) + 𝑂(𝑚)) * cantidad de c.a.

∵ solo en una componente conexa de 𝓈 ~ 𝓉 se puede transportar flujo
  => 𝑛 - 1 ≤ 𝑚 (con 𝑛 - 1 = 𝑚 solo en una componente conexa 𝑁 con Δ(𝑁))
  => 𝑂(𝑛) ≤ 𝑂(𝑚)

=> cmp(EK) = 𝑂(𝑚) * cantidad de c.a.

Cantidad de caminos aumentantes:
  ∵ 𝜀 es la capacidad mínima residual de todos las aristas en un camino aumentante
    => en cada camino aumentante hay al menos una arista que se vuelve crítica
  => cantidad de c.a. ≈ 𝑚 * veces que una arista puede volverse crítica (≤)

  vamos a ver como se puede volver crítica una arista
    El lado 𝑥𝑧 se satura en el paso 𝑘
      => para pasar de ƒ𝑘 a ƒ𝑘+1 usé un ƒ𝑘-c.a. de la forma 𝓈…𝑥𝑧…𝓉
      ∵ EK
        => es el camino de menor longitud
        => también es el camino de menor longitud entre 𝓈, 𝑥 y 𝑧
        => 𝑑𝑘(𝑧) = 𝑑𝑘(𝑥) + 1
      Veamos como puede volver 𝑥𝑧 a ser crítica en un paso 𝑗 > 𝑘
        se usa backwards en 𝑗 y se vacía
        se usa backwards en 𝑖 entre 𝑘 y 𝑗 para volver a saturarse en 𝑗
          => ∃𝑖 : 𝑘 < 𝑖 ≤ 𝑗 tq 𝑥𝑧 se usa backwards en 𝑖
            para pasar de ƒ𝑖 a ƒ𝑖+1 uso un ƒ𝑖-c.a. de la forma 𝓈…←𝑧𝑥…𝓉
              => 𝑑𝑖(𝑥) = 𝑑𝑖(𝑧) + 1
                ∵ 𝑑𝑘(𝑥) ≤ 𝑑𝑖(𝑥), 𝑑𝑘(𝑧) ≤ 𝑑𝑖(𝑧), (por Lema de Distancias)
                  𝑑𝑘(𝑥) + 1 = 𝑑𝑘(𝑧), 𝑑𝑖(𝑧) + 1 = 𝑑𝑖(𝑥)
                  => 𝑑𝑘(𝑥) + 1 = 𝑑𝑘(𝑧) ≤ 𝑑𝑖(𝑧) = 𝑑𝑖(𝑥) - 1
                  => 𝑑𝑘(𝑥) + 1 ≤ 𝑑𝑖(𝑥) - 1
                  => 𝑑𝑘(𝑥) + 2 ≤ 𝑑𝑖(𝑥)
                ∵ 𝑑𝑗(𝓉) ≥ 𝑑𝑖(𝓉), 𝑏𝑖(𝑥) ≥ 𝑏𝑘(𝑥) (por lema de distancias)
                𝑑𝑗(𝓉) ≥ 𝑑𝑖(𝓉) = 𝑑𝑖(𝑥) + 𝑏𝑖(𝑥) ≥ 𝑑𝑘(𝑥) + 2 + 𝑏𝑖(𝑥)
                  ≥ 𝑑𝑘(𝑥) + 2 + 𝑏𝑘(𝑥) = 𝑑𝑘(𝓉) + 2
                => 𝑑𝑗(𝓉) ≥ 𝑑𝑘(𝓉) + 2
    El lado 𝑥𝑧 se vacía en el paso 𝑘
      => para pasar de ƒ𝑘 a ƒ𝑘+1 usé un ƒ𝑘-c.a. de la forma 𝓈…←𝑧𝑥…𝓉
      ∵ EK
        => es el camino de menor longitud
        => también es el camino de menor longitud entre 𝓈, 𝑥 y 𝑧
        => 𝑑𝑘(𝑥) = 𝑑𝑘(𝑧) + 1
      Veamos como puede volver 𝑥𝑧 a ser crítica en un paso 𝑗 > 𝑘
        se usa forwards en 𝑗 y se vacía
        se usa forwards en 𝑖 entre 𝑘 y 𝑗 para volver a vaciarse en 𝑗
          => ∃𝑖 : 𝑘 < 𝑖 ≤ 𝑗 tq 𝑥𝑧 se usa forwards en 𝑖
            para pasar de ƒ𝑖 a ƒ𝑖+1 uso un ƒ𝑖-c.a. de la forma 𝓈…𝑥𝑧…𝓉
            => 𝑑𝑖(𝑧) = 𝑑𝑖(𝑥) + 1
              ∵ 𝑑𝑘(𝑥) ≤ 𝑑𝑖(𝑥), 𝑑𝑘(𝑧) ≤ 𝑑𝑖(𝑧), (por Lema de Distancias)
              𝑑𝑘(𝑧) + 1 = 𝑑𝑘(𝑥), 𝑑𝑖(𝑥) = 𝑑𝑖(𝑧) -1
              => 𝑑𝑘(𝑥) ≤ 𝑑𝑖(𝑥)
              => 𝑑𝑘(𝑧) + 1 ≤ 𝑑𝑖(𝑧) -1
              => 𝑑𝑘(𝑧) + 2 ≤ 𝑑𝑖(𝑧)
              ∵ 𝑑𝑗(𝓉) ≥ 𝑑𝑖(𝓉), 𝑏𝑖(𝑧) ≥ 𝑏𝑘(𝑧) (por lema de distancias)
                𝑑𝑗(𝓉) ≥ 𝑑𝑖(𝓉) = 𝑑𝑖(𝑧) + 𝑏𝑖(𝑧) ≥ 𝑑𝑘(𝑧) + 2 + 𝑏𝑖(𝑧)
                ≥ 𝑑𝑘(𝑧) + 2 + 𝑏𝑘(𝑧) = 𝑑𝑘(𝓉) + 2
  En ambos casos llego a que para que una arista pueda volver a hacerse crítica,
  la distancia entre 𝓈 y 𝓉 debe aumentar al menos en 2
  ∵ esta distancia está siempre entre 1 y 𝑛
  => esto puede pasar a lo sumo 𝑛/2 veces 𝑂(𝑛)
  ∵ cantidad de c.a. ≈ 𝑚 * veces que una arista puede volverse crítica (≤)
  => cantidad de c.a. ≈ 𝑚𝑛/2 = 𝑂(𝑛𝑚)

∵ cmp(EK) = 𝑂(𝑚) * cantidad de c.a.
=> cmp(EK) = 𝑂(𝑚)𝑂(𝑛𝑚) = 𝑂(𝑛𝑚^2) ■

### Dinitz

Def. Un network por niveles es un network con 𝑉𝑖 ⊆ 𝑉, 0 ≤ 𝑖 ≤ 𝑟, tales que:

* 𝑉𝑖 ∩ 𝑉𝑗 = ∅
* 𝑉 = ∪^𝑟_{𝑖=0} 𝑉𝑖
* 𝐸 ⊆ ∪^{𝑟−1}_{𝑖=0} (𝑉𝑖 × 𝑉𝑖+1)

Def. Dado un flujo ƒ sobre un network 𝑁 = (𝑉, 𝐸, 𝑐),
definimos el network auxiliar (relativo a ƒ) como el network por niveles dado por:

* 𝑟 = 𝑑ƒ(𝓈, 𝓉)
* 𝑉0 = {𝓈}
* 𝑉𝑟 = {𝓉}
* 𝑉𝑖 = {𝑥 : 𝑑ƒ(𝓈, 𝑥) = 𝑖} donde 0 < 𝑖 < 𝑟.
* 𝐸aux = {(𝑥, 𝑦) : ∃𝑖 : 𝑥∈𝑉𝑖, 𝑦∈𝑉𝑖+1 ∧
  ((→𝑥𝑦 ∈ 𝐸 ∧ ƒ(→𝑥𝑦) < 𝑐(→𝑥𝑦)) ∨ (→𝑦𝑥 ∈ 𝐸 ∧ ƒ(→𝑦𝑥) > 0))}

Def. Un flujo en un network 𝑁 = (𝑉, 𝐸, 𝑐) se dice bloqueante si,
al querer correr greedy a partir de ese flujo, el flujo no puede aumentarse.

#### Complejidad de Dinitz

cmp(Dinitz) = cmp(Even) =
(cmp(hallar blocking flow) + cmp(construir 𝑁𝐴)) * cantidad de 𝑁𝐴s

∵ el nivel de 𝓉 aumenta en 𝑁𝐴s sucesivos
=> tendré a lo sumo 𝑂(𝑛) 𝑁𝐴s

∵ uso BFS para construir 𝑁𝐴
=> cmp(construir 𝑁𝐴) = 𝑂(𝑚)

cmp(Dinitz) = cmp(Even) = (cmp(hallar blocking flow) + 𝑂(𝑚)) * 𝑂(𝑛)

cmp(Dinitz) = cmp(Even) = max(𝑂(𝑚), cmp(hallar blocking flow)) * 𝑂(𝑛)

Complejidad de hallar blocking flow:

Dinitz:

En esta versión, se realiza un proceso llamado PODAR.
Este asegura que la 𝑁𝐴 tenga la propiedad de que todo vértice tiene
al menos un lado saliente hacia el siguiente nivel.
=> el DFS encuentra un camino de 𝓈 a 𝓉 sin hacer backtracking (retrocesos).

* PODAR:
  * funcionamiento de PODAR:
    * va revisando los vertices desde los niveles superiores para abajo:
      * si el vertice tiene aristas salientes no hace nada
      * si no tiene aristas salientes, borra el vértice y sus aristas entrantes
  * Complejidad de PODAR:
    * Borrar un vértice y sus lados
      * Se hace a lo sumo una vez por vértice
      * Su complejidad es 𝑂(𝑑(𝑥)).
      * Para todos los vertices = Σ_{𝑥∈𝑉} 𝑑(𝑥) = 𝑂(𝑚)
* Caminos:
  * Encontrar y aumentar caminos:
    * ∵ hay a lo sumo 𝑛 niveles
    * ∵ para aumentar el flujo se recorre el camino 2 veces.
      (encontrar camino y aumentar flujo)
      * => aumentar el flujo en un camino es 𝑂(𝑛)
  * Caminos aumentantes por fase:
    * ∵ cada camino aumentante satura (al menos) una arista
    * ∵ cada 𝑁𝐴 tiene a lo sumo 𝑚 aristas
      * => se puede saturar como máximo 𝑚 aristas antes de quedarse sin red.
    * => tendré 𝑂(𝑚) caminos aumentantes por fase.
  * Complejidad de aumentar todos los flujos: 𝑂(𝑛 × 𝑚).
* Complejidad de encontrar blocking flow:
  * cmp(PODAR) + cmp(caminos) = 𝑂(𝑚) + 𝑂(𝑛𝑚) = 𝑂(𝑛𝑚)

∵ cmp(Dinitz) = max(𝑂(𝑚), cmp(hallar blocking flow)) * 𝑂(𝑛)
=> cmp(Dinitz) = max(𝑂(𝑚), 𝑂(𝑛𝑚)) × 𝑂(𝑛) = 𝑂(𝑛𝑚) × 𝑂(𝑛) = 𝑂(𝑛^2𝑚)

Dinitz-Even:

La versión de Even simplifica el proceso al no realizar la poda proactiva,
permitiendo que el DFS realice backtracking, pero eliminando el lado (o vértice)
inmediatamente después de retroceder por él para "guardar" esa información.

```psudocode
// s = source, t = target
g := 0
while (1) {
  p := [s]
  x := s
  while (x != t) {
    if (VecinosAdelanteDe(x) != []) {       // Avanzar (𝐴)
      y := tomar de VecinosAdelanteDe(x)
      agregar y al final de p
      x := y
    } else {
      if (x != s) {                         // Retroceder (𝑅)
        z := vértice anterior a x en p
        borrar zx
        x := z
      } else {
        return g
      }
    }
  }
  aumentar g en p y borrar lados saturados  // Incrementar (𝐼)
}
```

* Análisis por Palabras (𝐴, 𝑅, 𝐼):
  * La ejecución se modela como una sucesión de operaciones:
    * Avanzar (𝐴)
    * Retroceder (𝑅):
      borra un lado porque si retrocediste => llegaste a un vértice sin vecinos
    * Incrementar flujo (𝐼): borra un lado porque se satura (cuello de botella).
    * Se analizan subpalabras de la forma 𝐴…𝐴𝑋, donde 𝑋 es 𝑅 o 𝐼.
* Análisis del Flujo Bloqueante (𝑂(𝑛𝑚)):
  1. Cantidad de palabras:
      Cada palabra termina en una operación (𝑅 o 𝐼)
      que borra permanentemente al menos un lado del 𝑁𝐴.
      Por lo tanto, hay a lo sumo 𝑂(𝑚) palabras.
  2. Complejidad de cada palabra:
      * Cada operación 𝐴 y 𝑅 es 𝑂(1).
      * La operación 𝐼 recorre el camino para aumentar flujo y borrar lados,
        lo cual es 𝑂(𝑛).
      * Como cada palabra puede tener a lo sumo 𝑛 avances (niveles del 𝑁𝐴)
        antes de un 𝑅 o 𝐼, la complejidad de cada palabra es 𝑂(𝑛).
* Conclusión:
  cmp (hallar blocking flow) = 𝑂(𝑚 palabras) * 𝑂(𝑛 por palabra) = 𝑂(𝑛𝑚)

∵ cmp(Even) = max(𝑂(𝑚), cmp(hallar blocking flow)) * 𝑂(𝑛)
=> cmp(Even) = max(𝑂(𝑚), 𝑂(𝑛𝑚)) × 𝑂(𝑛) = 𝑂(𝑛𝑚) × 𝑂(𝑛) = 𝑂(𝑛^2𝑚) ■

### Wave (Tarjan)

#### Complejidad de Wave

salteado por ahora

---

## Bipartitos y Matchings

Def.  Un grafo 𝐺 = (𝑉, 𝐸) es bipartito si 𝑉 existe una partición de 𝑉
en dos conjuntos (𝑉_1, 𝑉_2) tales que (𝑉_1 ∪ 𝑉_2 = 𝑉) y (𝑉_1 ∩ 𝑉_2 = ∅),
y para toda arista {𝑢, 𝑣} ∈ 𝐸, se cumple que
𝑢 está en 𝑉_1 sii 𝑣 está en 𝑉_2.
(∀𝑣1 ∈ 𝑉_1, : Γ(𝑣1) ⊆ 𝑉_2, ∧ ∀𝑣2 ∈ 𝑉_2, : Γ(𝑣2) ⊆ 𝑉_1)

Def.  Sea 𝐺 = (𝑉, 𝐸) un grafo no dirigido.
Un matching en 𝐺 es un subgrafo 𝑀 ⊆ 𝐺 tal que 𝑑_𝑀(𝑣) = 1 para todo 𝑣 ∈ 𝑀.
𝛿(𝑀) = Δ(𝑀) = 1

Def.  Sean 𝐺 = (𝑉, 𝐸) un grafo y 𝑀 ⊆ 𝐺 un matching.
Decimos que 𝑀 es maximal si |𝐸(𝑀)| ≥ |𝐸(𝑀′)| para todo matching 𝑀′ ⊆ 𝐺.

### Teorema de Hall

Teo. Sea 𝐺 = (𝑉, 𝐸) bipartito con partes 𝑋 e 𝑌.
Existe un matching completo de 𝑋 a 𝑌 sii
todo subconjunto de 𝑋 tiene al menos la misma cantidad de vecino que de vértices.
∃matching 𝑀 ⊆ 𝐺 : |𝐸(𝑀)| = |𝑋| <=> |Γ(𝑆)| ≥ |𝑆| ∀𝑆 ⊆ 𝑋

Prueba:

* ⇒
  * Sup existe un matching completo 𝑀 de 𝑋 a 𝑌
  * => 𝑀 induce una función inyectiva ƒ : 𝑋 → 𝑌 | ƒ(𝑥) ∈ Γ(𝑥) ∀𝑥 ∈ 𝑋
  * ∵ ƒ es inyectiva
  * => |ƒ(𝑆)| = |𝑆| ∀𝑆 ⊆ 𝑋
  * => ƒ(𝑆) ⊆ Γ(𝑆) ⇒ |𝑆| ≤ |Γ(𝑆)|
* ⇐
  * Sup que:
    1. se cumple que |Γ(𝑆)| ≥ |𝑆| ∀𝑆 ⊆ 𝑋 (la condición de Hall)
    2. corriendo algoritmo para hallar un matching maximal llego a
    un matching maximal 𝑀 : |𝐸(𝑀)| < |𝑋|
  * Construiremos un 𝑆 ⊆ 𝑋 : |Γ(𝑆)| < |𝑆| (violando Hall y llegando a un absurdo)
    * Corramos el algoritmo de extensión de matching sobre el matching 𝑀
    * ∵ 𝑀 no cubre a 𝑋
      * => existen filas sin matchear
    * Sea 𝑆 el conjunto de filas etiquetadas
    * Sea 𝑆0 ⊆ 𝑆 el conjunto de filas estiquetadas con "∗"
      (las filas que no forman parte del matching)
      * ∵ 𝑀 no es completo => 𝑆0 ≠ ∅.
    * Sea 𝑇 el conjunto de columnas etiquetadas
    * Sea 𝑇1 ⊆ 𝑇 el conjunto de columnas estiquetadas por 𝑆0
    * En gral.:
      * 𝑇𝑖+1 serán las columnas etiquetadas por 𝑆𝑖
      * 𝑆𝑗 son las filas etiquetadas por 𝑇𝑗
    * Las sucesiones son finitas
    * 𝑆 = 𝑆0∪𝑆1∪… con uniones disjuntas
    * 𝑇 = 𝑇1∪𝑇2∪… con uniones disjuntas
    * Revisemos una columna de 𝑇
      * o está libre y el matching se extiende
        * no puede pasar porque 𝑀 es maximal
      * o encuentra una fila matcheada de esa columna y la etiqueta
    * => el algoritmo nunca se detiene cuando se pasa de un 𝑇𝑗 a un 𝑆𝑗
    * => el algoritmo solo se detiene al pasar de un 𝑆𝑘 a un 𝑇𝑘+1 que es vacío
      (cuando al revisar un conjunto de vecinos para una fila, no hay vecinos libres)
    * ∵ cada columna etiqueta solo a la fila con la que está matcheada
      , 𝑇𝑗 crea a 𝑆𝑗 y el algoritmo nunca se detiene en 𝑇
      * => |𝑇𝑗| = |𝑆𝑗|
    * |𝑆| = |𝑆0| + |𝑆1| + … + |𝑆𝑘| = |𝑆0| + |𝑇1| + |𝑇2| + … + |𝑇𝑘|
    = |𝑆0| + |𝑇|
    * ∵ 𝑆0 ≠ ∅
    * => |𝑆| = |𝑆0| + |𝑇| > |𝑇|
    * ∴ |𝑆| > |𝑇| Si demostráramos que 𝑇 = Γ(𝑆), la prueba concluiría.
      * 𝑇 ⊆ Γ(𝑆)
        * 𝑇 son las columnas etiquetadas
        * las columnas son etiquetadas por filas de 𝑆.
        * Y una fila solo etiqueta columnas vecinas.
        * => 𝑇 ⊆ Γ(𝑆).
      * Γ(𝑆) ⊆ 𝑇
        * Probemos que 𝑦 ∈ Γ(𝑆) => 𝑦 ∈ 𝑇
        * ∴ 𝑦 ∈ Γ(𝑆)
          * => ∃𝑥 ∈ 𝑆 : 𝑦 ∈ Γ(𝑥)
        * ∴ 𝑥 ∈ 𝑆
          * => 𝑥 es encontrado por el algoritmo
          * cuando 𝑥 es encontrado 𝑥 etiqueta a 𝑦
        * ∴ 𝑦 está etiquetado
          * => 𝑦 ∈ 𝑇 ■

### Matrimonio de Kőnig

Teo.
  Sea 𝐺 = (𝑉, 𝐸) grafo regular bipartito (𝜒(𝐺) = 2 y δ(𝐺) = Δ(𝐺) > 0)
  => 𝐺 tiene matching perfecto.

Prueba:

Dado 𝑊 ⊆ 𝑉, definimos
  𝐸_𝑊 = {𝑤𝑢 : 𝑤𝑢 ∈ 𝐸 y 𝑤 ∈ 𝑊}

Sean 𝑋 e 𝑌 las partes de 𝐺

Sean 𝑆 ⊆ 𝑋, 𝑒 ∈ 𝐸_𝑆

=> ∃𝑥 ∈ 𝑆, 𝑦 ∈ 𝑌 : 𝑒 = 𝑥𝑦 = 𝑦𝑥
  => 𝑦 ∈ Γ(𝑥)
∴ 𝑥 ∈ 𝑆
  => 𝑦 ∈ Γ(𝑆)
=> 𝑒 ∈ 𝐸_Γ(𝑆)

∴ (𝑒 ∈ 𝐸_𝑆 => 𝑒 ∈ 𝐸_Γ(𝑆))
  => 𝐸_𝑆 ⊆ 𝐸_Γ(𝑆)
  => |𝐸_𝑆| ≤ |𝐸_Γ(𝑆)|

Calculemos |𝐸_𝑊| para 𝑊 ⊆ 𝑋 o 𝑊 ⊆ 𝑌

sea 𝑤𝑢 ∈ 𝐸_𝑊, 𝑤 ∈ 𝑊 => u ∉ 𝑊

=> 𝐸_𝑊 = ∪_{𝑤∈𝑊} {𝑤𝑣 : 𝑣 ∈ Γ(𝑤)}

=> |𝐸_𝑊| = ∑_{𝑤∈𝑊} |{𝑤𝑣 : 𝑣 ∈ Γ(𝑤)}| = ∑_{𝑤∈𝑊} 𝑑(𝑤)

∴ 𝐺 es regular y 𝑊 ⊆ 𝑉
  => (∀𝑤∈𝑊) 𝑑(𝑤) = Δ(𝐺)

=> ∑_{𝑤∈𝑊} 𝑑(𝑤) = ∑_{𝑤∈𝑊} Δ(𝐺) = |𝑊| × Δ(𝐺)

=> |𝐸_𝑆| ≤ |𝐸_Γ(𝑆)| <=> |𝑆| × Δ(𝐺) ≤ |Γ(𝑆)| × Δ(𝐺)
<=> |𝑆| ≤ |Γ(𝑆)|

∴ |𝑆| ≤ |Γ(𝑆)| y 𝑆 ⊆ 𝑋
=> por teorema de Hall hay matching completo de 𝑋 a 𝑌

∴ No se asumió ninguna particularidad de 𝑋 o 𝑌
=> se puede hacer analogamente para 𝑆 ⊆ 𝑌 por simetría
=> hay matching completo de 𝑌 a 𝑋

∴ ∃matching completo de 𝑋 a 𝑌
=> |𝑋| ≤ |𝑌|.

∴ la elección de 𝑋 sobre 𝑌 en la prueba fue arbitraria.
=> Por simetría también vale para 𝑌.
=> |𝑋| = |𝑌|.
=> hay matching perfecto

otra forma de verlo:
∴ 𝑋 e 𝑌 son las partes de 𝐺 => 𝐸_𝑋 = 𝐸_𝑌
=> |𝐸_𝑋| = |𝐸_𝑌|
=> |𝑋| × Δ(𝐺) = |𝑌| × Δ(𝐺) (por lo visto antes)
=> |𝑋| = |𝑌|
∴ hay matching completo de 𝑋 a 𝑌 y |𝑋| = |𝑌|
=> hay matching perfecto ■

### Algoritmo Húngaro

#### Complejidad del algoritmo Húngaro

Teo. La complejidad del algoritmo "minimizar la suma",
también llamado "algoritmo húngaro",
tal como fue dado en clase haciéndolo a mano, es 𝑂(𝑛^4)

Teo. El algoritmo húngaro se puede codificar con complejidad 𝑂(𝑛^3).

Prueba:

salteado por ahora

---

## Complejidad Computacional

### 3-color es NP-completo

El problema de determinar si un grafo 𝐺 admite un coloreo propio con 3 colores
es NP-completo.

Prueba:

se deben demostrar dos condiciones fundamentales:

1. su pertenencia a la clase NP.
    * un problema pertenece a la clase NP si
    dada una solución propuesta se puede verificar en tiempo polinomial.
2. si es NP-hard
    * un problema es NP-hard si:
    TODOS los problemas NP pueden reducirse a él en tiempo polinomial.
    <=> es difícil como el problema más difícil de NP.
    para esto lo vamos a comparar con un problema NP-completo conocido (3sat)

entonces:

1. 3-color es NP:
    * una solución es una coloración en {0, 1, 2} de los 𝑛 vértices del grafo.
    * Se verifica recorriendo todas las aristas 𝑢𝑣 y viendo que 𝐶(𝑢) ≠ 𝐶(𝑣).
    * es de complejidad 𝑂(𝑚), con 𝑚 = número de aristas, claramente polinomial.
2. 3-color es NP-hard:
    * probaré que 3SAT ≤ 3COLOR.

3SAT ≤ 3COLOR
Para ello,
debemos transformar en tiempo polinomial un elemento B
del dominio (conjunto de instancias) de 3SAT a un elemento
A(B) = G del dominio de 3COLOR, mostrando que B es satisfacible sii G es 3-colorable.

Sabemos que B es de la forma D1∧…∧Dm con variables x1, …, xi,
donde Dj = lj1 ∨ lj2 ∨ lj3, con ljk = x_algo o x^c_algo.

A transforma a B en G de la siguiente forma:

* V(G) =
  {u1, …, ui, w1, …, wi} ∪
  {∪^m_{j=1} {a_{j1}, a_{j2}, a_{j3}, e_{j1}, e_{j2}, e_{j3}}} ∪
  {CAPI, t}

* E(G) =
  * Triángulos = {(u_it), (tw_i), (w_iu_i)} con i=1, …, n
  * Garras =
    {a_{j1}a_{j2}, a_{j2}a_{j3}, a_{j3}a_{j1}} con j=1, …, m ∪
    {a_{j1}e_{j1}, a_{j2}e_{j2}, a_{j3}e_{j3}} con j=1, …, m
  * Lados = {(CAPI)e_{jr}} con j=1, …, m y r = 1,2,3
  * Lados = {(CAPI)t}
  * Lados = {e_{jr}(v(l_{jr}))} con j=1, …, m y r = 1,2,3
    * v(l_{jr}) = u_q si ∃q : l_{jr} = x_q
    * v(l_{jr}) = w_q si ∃q : l_{jr} = x^c_q

B satisfacible sii χ(G) ≤ 3

* ⇐
* ⇒  

---

## Códigos

### Cota de Hamming

Sean 𝐶 ⊆ {0, 1}^𝑛; 𝛿 = 𝛿(𝑐) y 𝑡 = floor((𝛿−1)2)
Entonces:
|𝐶| ≤ 2^𝑛/ 1 + 𝑛 + 𝑛𝐶2 + … + 𝑛𝐶𝑡 (𝑛𝐶𝑘 = 𝑛!/(𝑘!(𝑛−𝑘)!))

### Teorema de la Distancia Mínima y la Matriz de Chequeo

Teo. Sea 𝐶 un código lineal con matriz de chequeo 𝐻.
  Entonces 𝛿(𝐶) es el mínimo número de columnas LD de 𝐻

### Códigos cíclicos

#### Polinomio Generador

Sea 𝐶 cíclico y 𝑔(𝑥) su polinomio generador. Entonces

1. 𝐶 = {𝑝(𝑥) : 𝑔𝑟(𝑝(𝑥)) < 𝑛 ∧ 𝑔(𝑥) | 𝑝(𝑥)};
  es decir, los códigos son los múltiplos de 𝑔(𝑥) con grado < 𝑛.
2. 𝐶 = {𝑔(𝑥) ◦ 𝑣(𝑥) : 𝑣 es un polinomio arbitrario}
    * $C = {v(x) ⊙ g(x) : v es un polinomio cualquiera}$
3. 𝑔𝑟(𝑔(𝑥)) = 𝑛 − 𝑘 con 𝑘 = dim𝐶
4. 𝑔(𝑥) | 1 + 𝑥^𝑛
5. Si 𝑔(𝑥) = 𝑔0 + 𝑔1𝑥 + … , entonces 𝑔0 = 1
