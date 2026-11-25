# teórico

## Contents

1. [Grafos y coloración](<#grafos-y-coloración>)
    - [Baby Brooks (08)](<#baby-brooks>)
2. [Network flows](<#network-flows>)
    - [Ford-Fulkerson](<#ford-fulkerson>)
    - [Max flow Min cut (07)](<#max-flow-min-cut>)
      - [ƒ flujo maximal => ∃corte minimal 𝑆 : 𝓋(ƒ) = Cap(𝑆) (07)](<#ƒ-flujo-maximal--corte-minimal-𝑆--𝓋ƒ--cap𝑆-de-final>)
    - [Edmonds-Karp (01, 02, 03)](<#edmonds-karp>)
      - [Lema de las Distancias (02, 03)](<#lema-de-las-distancias>)
        - [𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥) (03)](<#𝑑𝑘𝑥--𝑑𝑘1𝑥>)
        - [𝑏𝑘(𝑥) ≤ 𝑏𝑘+1(𝑥) (02)](<#𝑏𝑘𝑥--𝑏𝑘1𝑥>)
      - [Complejidad de Edmonds-Karp (01)](<#complejidad-de-edmonds-karp>)
    - [Dinitz (04, 06)](<#dinitz>)
      - [Complejidad de Dinitz (04)](<#complejidad-de-dinitz>)
      - [Distancia en NAs (06)](<#distancia-en-nas>)
    - [Wave (Tarjan) (05)](#wave-tarjan)
      - [Complejidad de Wave (05)](<#complejidad-de-wave>)
3. [Bipartitos y matchings](<#bipartitos-y-matchings>)
    - [Teorema de Hall (09)](<#teorema-de-hall>)
    - [Matrimonio de Kőnig (10)](<#matrimonio-de-kőnig>)
    - [Algoritmo Húngaro (11)](<#algoritmo-húngaro>)
      - [Complejidad del algoritmo Húngaro (11)](<#complejidad-del-algoritmo-húngaro>)
4. [Complejidad Computacional (15, 16)](<#complejidad-computacional>)
    - [3-color (15)](#3-color-es-np-completo)
    - [Matrimonio 3D (16)](<#matrimonio-3d>)
5. [Códigos](<#códigos>)
    - [Cota de Hamming (12)](<#cota-de-hamming>)
    - [Distancia Mínima y Matriz de Chequeo (13)](<#teorema-de-la-distancia-mínima-y-la-matriz-de-chequeo>)
    - [Cíclicos (14)](<#códigos-cíclicos>)
      - [Polinomio Generador (14)](<#polinomio-generador>)
6. listas de temas
    - A:
      - [01. Complejidad de Edmonds-Karp](<#complejidad-de-edmonds-karp>)
      - [02. 𝑏𝑘(𝑥) ≤ 𝑏𝑘+1(𝑥)](<#𝑏𝑘𝑥--𝑏𝑘1𝑥>)
      - [03. 𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥)](<#𝑑𝑘𝑥--𝑑𝑘1𝑥>)
      - [04. Complejidad de Dinitz](<#complejidad-de-dinitz>)
      - [05. Complejidad de Wave](<#complejidad-de-wave>)
      - [06. (Falta) Distancia en NAs](<#distancia-en-nas>)
    - B:
      - [15. 3-color (15)](#3-color-es-np-completo)
      - [16. Matrimonio 3D](<#matrimonio-3d>)
    - C:
      - [07. Max flow Min cut](<#ƒ-flujo-maximal--corte-minimal-𝑆--𝓋ƒ--cap𝑆-de-final>)
      - [08. Baby Brooks](<#baby-brooks>)
      - [09. Teorema de Hall](<#teorema-de-hall>)
      - [10. Matrimonio de König](<#matrimonio-de-kőnig>)
      - [11. (Falta) Complejidad del Húngaro](<#complejidad-del-algoritmo-húngaro>)
      - [12. Cota de Hamming](<#cota-de-hamming>)
      - [13. (Falta) Distancia Mínima y Matriz de Chequeo](<#teorema-de-la-distancia-mínima-y-la-matriz-de-chequeo>)
      - [14. (Falta) Polinomio Generador](<#polinomio-generador>)

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
  Δ(𝐺) ≝ max{𝑑(𝑣) : 𝑣 ∈ 𝑉}
  δ(𝐺) ≝ min{𝑑(𝑣) : 𝑣 ∈ 𝑉}

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
𝛸(𝐺) ≝ min{𝑘 : 𝐺 tiene un coloreo propio con 𝑘 colores}

Alg. 1 Greedy para Colorear un Grafo

- Funcionamiento:
  - Dado un orden de los vértices de 𝐺 (𝑣_1, …, 𝑣_𝑛)
    1. 𝐶(𝑣_1) = 1.
    2. Para 𝑖 : (2 ≤ 𝑖 ≤ 𝑛)
      𝐶(𝑣_𝑖) = min{𝑘 ∈ ℕ : 𝐶(𝑣_𝑗) ≠ 𝑘 (∀𝑗 < 𝑖 : 𝑣_𝑗 ∈ Γ(𝑣_𝑖))}
- Complejidad
  - Σ(𝑑(𝑣)) -> para cada vértice 𝑣, se miran sus vecinos
  - Σ(𝑑(𝑣)) = 2𝑚 -> por apreton de manos, las aristas se cuentan dos veces
  - 𝑂(𝑚) = 𝑂(𝐸)
  - En no conexos: 𝑂(𝑛 + 𝑚)
    porque se cuentan vertices aunque no haya aristas, domina el mayor

#### Baby Brooks

Teo. 1 Si G = (𝑉, 𝐸) es un grafo conexo no regular, entonces χ(G) ≤ ∆(G).

Prueba:  
Como 𝐺 no es regular, sabemos que el grado mínimo es estrictamente menor que el
grado máximo (𝛿(𝐺) < Δ(𝐺)).  
Elegimos un vértice 𝑣 tal que su grado sea mínimo: 𝑑(𝑣) = 𝛿(𝐺).

1. Construcción del orden de coloreo
    - Hacemos BFS partiendo desde 𝑣
    - como 𝐺 es conexo, alcanzamos todos los vértices del grafo
    - En un árbol BFS, cada vértice es "descubierto" por un padre.
2. Hacemos Greedy
    - Usamos el orden inverso al orden en que
    los vértices fueron incorporados al árbol de búsqueda
    - el vértice raíz 𝑣 será el último en ser coloreado (𝑣_𝑛 = 𝑣)
    - cuando vayamos a colorear un vértice cualquiera (excepto 𝑣),
    su "padre" en el árbol BFS todavía no estará coloreado
3. Tomemos un vértice 𝑢 distinto de 𝑣
    - Cuando el algoritmo llega a 𝑢, su "padre" todavía no fue coloreado
    - De sus vecinos, a lo sumo 𝑑(𝑢) - 1 tienen color
    - Como su grado máximo es Δ(𝐺), a lo sumo le roban Δ(𝐺) - 1 colores
    - Hay al menos un color disponible en el conjunto de Δ(𝐺) colores
4. LLegamos a 𝑣
    - Es el último, todos sus vecinos están coloreados
    - le podrían robar hasta 𝑑(𝑣) colores
    - Como 𝑑(𝑣) = 𝛿(𝐺) y sabemos que 𝛿(𝐺) < Δ(𝐺)
    - Sus vecinos le roban estrictamente menos de Δ(𝐺) colores
    - Sobra al menos un color para colorear a 𝑣
5. Conclusión
    - Encontramos un coloreo propio del grafo usando como máximo Δ(𝐺) colores
    - lo que demuestra que χ(G) ≤ ∆(G) ■

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

- Funcionamiento
  1. Inicializa el flujo en ƒ(→𝑥𝑦) = 0 ∀→𝑥𝑦 ∈ 𝐸.
  2. Encuentra un camino no saturado de 𝓈 a 𝓉
      - es decir, un camino con flujo menor a la capacidad en cada lado.
      - Si el camino no existe, termina.
  3. Suma al flujo de cada lado en el camino el valor máximo que puede
    sumarse sin sobrepasar la capacidad de ningún lado.
  4. Regresa a 2.
- Propiedades:
  - Siempre devuelve un flujo
  - Su complejidad 𝑂(𝑚^2), hace 𝑂(𝑚) iteraciones y cada iteración es 𝑂(𝑚).
  - No siempre devuelve flujo maximal.

### Ford-Fulkerson

Def. 19 Un ƒ-camino aumentante es una sucesión de vertices 𝑣_0, …, 𝑣_𝑟
tq 𝑣_0 = 𝓈, 𝑣_𝑟 = 𝓉, ∀𝑖 ∈ [0, 𝑟 − 1] y se cumple una de 2:

- →𝑣_𝑖𝑣_{𝑖+1} ∈ 𝐸 y ƒ(→𝑣_𝑖𝑣_{𝑖+1}) < 𝑐(→𝑣_𝑖𝑣_{𝑖+1})
- ←𝑣_𝑖𝑣_{𝑖+1} ∈ 𝐸 y ƒ(←𝑣_𝑖𝑣_{𝑖+1}) > 0

Teo. 4 Si ƒ es un flujo, 𝓋(ƒ) = 𝚟 y aumentamos ƒ con un ƒ-camino aumentante
con 𝜀, entonces lo que queda sigue siendo un flujo y su valor es 𝚟 + 𝜀.

Alg. 3 Ford-Fulkerson

- Funcionamiento
  1. Inicializa ƒ(→𝑣𝑢) = 0 ∀→𝑣𝑢 ∈ 𝐸.
  2. Mientras exista un ƒ-camino aumentante desde 𝓈 hasta 𝓉:
      1. Encuentra ese camino 𝑣_0…𝑣_𝑟 (con 𝑣_0 = 𝓈, 𝑣_𝑟 = 𝓉).
      2. Calcula el incremento para cada arista en el camino (𝜀_𝑖)
          - si se usa un lado forward, el margen es lo que falta para saturarlo
            - 𝑐(→𝑣_𝑖𝑣_{𝑖+1}) - ƒ(→𝑣_𝑖𝑣_{𝑖+1})
          - si se usa un lado backward, el margen es lo que se ha usado
            - ƒ(←𝑣_𝑖𝑣_{𝑖+1})
      3. Determina el cuello de botella 𝜀 = min{𝜀_𝑖 ∀𝑖 : 0 < 𝑖 < 𝑟}.
      4. Actualiza ƒ sumando 𝜀 a cada arista en el camino.
  3. Si no existe un ƒ-camino aumentante, el flujo actual es el máximo posible.
- Propiedades
  - no siempre termina
  - si termina devuelve un flujo maximal

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

- 𝓋(ƒ) ≤ Cap(𝑆).
- ƒ es maximal sii existe un corte minimal 𝑆 tal que 𝓋(ƒ) = Cap(𝑆)

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
    - 𝑆 = 𝓈 ∪ {𝑣 ∈ 𝑉 : ∃ƒ-camino aumentante entre 𝓈 y 𝑣}
      - 𝓉 ∉ 𝑆
        - ∵ ∃ƒ-camino aumentante entre 𝓈 y 𝑣
        - => (𝓉 ∈ 𝑆 => ∃ƒ-camino aumentante entre 𝓈 y 𝓉)
        - => (𝓉 ∈ 𝑆 => se puede aumentar el flujo ƒ)
        - => 𝓉 ∈ 𝑆 contradice ƒ maximal
        - => 𝓉 ∉ 𝑆
    - ∵ 𝓉 ∉ 𝑆 y 𝓈 ∈ 𝑆 => 𝑆 es un corte

2. ∵ 𝑆 es un corte => 𝓋(ƒ) = ƒ(𝑆, 𝑆ᶜ) - ƒ(𝑆ᶜ, 𝑆)
    1. Considerando el primer termino (Aristas que salen de 𝑆):
        - ƒ(𝑆, 𝑆ᶜ) = ∑︁_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∈ 𝑆] [𝑢 ∉ 𝑆] ƒ(→𝑣𝑢)
          - sea →𝑣𝑢 ∈ 𝐸 con 𝑣 ∈ 𝑆 y 𝑢 ∉ 𝑆
            - ∵ 𝑣 ∈ 𝑆 => ∃ƒ-camino aumentante entre 𝓈 y 𝑣
            - ∵ 𝑢 ∉ 𝑆 => ¬∃ƒ-camino aumentante entre 𝓈 y 𝑢
            - ∵ ∃→𝑣𝑢
              - ƒ(→𝑣𝑢) < 𝑐(→𝑣𝑢) => ∃ƒ-camino aumentante de 𝓈 a 𝑢 con 𝓈…𝑣𝑢
              - ∵ sabemos que ¬∃ƒ-camino aumentante de 𝓈 a 𝑢 con 𝓈…𝑣𝑢
                - => ƒ(→𝑣𝑢) ≮ 𝑐(→𝑣𝑢) => ƒ(→𝑣𝑢) = 𝑐(→𝑣𝑢)
                - => ƒ(→𝑣𝑢) = 𝑐(→𝑣𝑢) (∀→𝑣𝑢 ∈ 𝐸, 𝑣 ∈ 𝑆 𝑢 ∉ 𝑆)
        - ƒ(𝑆, 𝑆ᶜ) = ∑_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∈ 𝑆] [𝑢 ∉ 𝑆] ƒ(→𝑣𝑢)
          = ∑_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∈ 𝑆] [𝑢 ∉ 𝑆] 𝑐(→𝑣𝑢) = Cap(𝑆)
          - ƒ(𝑆, 𝑆ᶜ) = Cap(𝑆)
    2. Considerando el segundo termino (Aristas que entran a 𝑆):
        - ƒ(𝑆ᶜ, 𝑆) = ∑_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∉ 𝑆] [𝑢 ∈ 𝑆] ƒ(→𝑣𝑢)
          - Sea →𝑢𝑣 ∈ 𝐸 con 𝑣 ∉ 𝑆 y 𝑢 ∈ 𝑆
            - ∵ 𝑣 ∉ 𝑆 => ¬∃ƒ-camino aumentante entre 𝓈 y 𝑣
            - ∵ 𝑢 ∈ 𝑆 => ∃ƒ-camino aumentante entre 𝓈 y 𝑢
            - ∵ ∃←𝑣𝑢
              - ƒ(→𝑢𝑣) > 0 => ∃ƒ-camino aumentante de 𝓈 a 𝑢 con 𝓈…(←𝑣𝑢)
                - ∵ sabemos que ¬∃ƒ-camino aumentante de 𝓈 a 𝑢
                  - => ƒ(→𝑢𝑣) ≯ 0 => ƒ(→𝑢𝑣) = 0
                  - => ƒ(→𝑢𝑣) = 0 (∀→𝑢𝑣 ∈ 𝐸, 𝑢 ∈ 𝑆 𝑣 ∉ 𝑆)
        - ƒ(𝑆ᶜ, 𝑆) = ∑_{→𝑣𝑢 ∈ 𝐸}[𝑣 ∉ 𝑆] [𝑢 ∈ 𝑆] ƒ(→𝑣𝑢) = 0
          - ƒ(𝑆ᶜ, 𝑆) = 0
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
=> 𝑥 tiene un elemento inmediatamente anterior

Sea 𝑝𝑘+1 el menor ƒ𝑘+1-c.a. entre 𝓈 y 𝑥, de longitud 𝑑𝑘+1(𝑥)
formando un camino 𝑝𝑘+1, de la forma 𝓈…𝑧𝑥,
con 𝑧 el elemento inmediatamente anterior de 𝑥

∵ 𝑝𝑘+1 es el camino de longitud mínima entre 𝓈 y 𝑥
=> también es el camino de longitud mínima entre 𝓈 y todos los vertices intermedios
=> 𝑑𝑘+1(𝑥) = 𝑑𝑘+1(𝑧) + 1 => 𝑑𝑘+1(𝑧) < 𝑑𝑘+1(𝑥)

∵ 𝑑𝑘+1(𝑧) < 𝑑𝑘+1(𝑥) y 𝑥 es el elemento de 𝐴 con menor 𝑑𝑘+1
=> 𝑧 ∉ 𝐴 => 𝑑𝑘(𝑧) ≤ 𝑑𝑘+1(𝑧)

∵ 𝑑𝑘(𝑧) ≤ 𝑑𝑘+1(𝑧) < 𝑑𝑘+1(𝑥) < ∞
=> 𝑑𝑘(𝑧) < ∞
=> ∃ƒ𝑘-c.a. 𝑝𝑘 de longitud mínima entre 𝓈 y 𝑧 de la forma 𝓈…𝑧

En el paso 𝑘+1 está disponible el arco residual 𝑧𝑥,
veamos si está disponible en el paso 𝑘 hay 2 casos:

- si →𝑧𝑥 es un lado:
  - En principio, podríamos poner 𝑥 al final de 𝑝𝑘, pero si hago eso queda:
    - 𝑑𝑘(𝑥) ≤ 𝑑𝑘(𝑧) + 1 ≤ 𝑑𝑘+1(𝑧) + 1 = 𝑑𝑘+1(𝑥) => 𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥)
    - lo que es absurdo porque 𝑥 ∈ 𝐴
    - => no se puede poner 𝑥 al final de 𝑝𝑘 => ƒ𝑘(→𝑧𝑥) = 𝑐(→𝑧𝑥)
  - Pero en 𝑝𝑘+1 está disponible el lado →𝑧𝑥
    - ∵ no lo puedo usar ahora =>
    - debió haberse desocupado en este paso (𝑘)
    - => para pasar de ƒ𝑘 a ƒ𝑘+1 lo usé backwards
    - ∵ no se puede recorrer el mismo vértice 2 veces, 𝑥 va antes que 𝑧
    - => 𝑝𝑘 es de la forma 𝓈…←𝑥𝑧…𝓉
    - ∵ en EK el uso de un vértice 𝑦 y en un paso 𝑗 siempre es en 𝑑𝑗(𝑦)
    - => 𝑑𝑘(𝑧) = 𝑑𝑘(𝑥) + 1
    - ∵ 𝑑𝑘(𝑧) = 𝑑𝑘(𝑥) + 1 > 𝑑𝑘+1(𝑥) + 1 = 𝑑𝑘+1(𝑧) + 2 ≥ 𝑑𝑘(𝑧) + 2
    - => 𝑑𝑘(𝑧) > 𝑑𝑘(𝑧) + 2 (ABSURDO)
- si →𝑥𝑧 es un lado:
  - En principio, podríamos poner 𝑥 al final de 𝑝𝑘, pero si hago eso queda:
    - 𝑑𝑘(𝑥) ≤ 𝑑𝑘(𝑧) + 1 ≤ 𝑑𝑘+1(𝑧) + 1 = 𝑑𝑘+1(𝑥) => 𝑑𝑘(𝑥) ≤ 𝑑𝑘+1(𝑥)
    - lo que es absurdo porque 𝑥 ∈ 𝐴
    - => no se puede poner 𝑥 al final de 𝑝𝑘 => ƒ𝑘(→𝑥𝑧) = 0
  - Pero en 𝑝𝑘+1 está el lado ←𝑧𝑥
    - ∵ no lo puedo usar ahora
    - => debió haber recibido flujo en este paso (𝑘)
    - => para pasar de ƒ𝑘 a ƒ𝑘+1 lo usé forwards
    - ∵ no se puede recorrer el mismo vértice 2 veces, 𝑥 va antes que 𝑧
    - => 𝑝𝑘 es de la forma 𝓈…𝑥𝑧…𝓉
    - ∵ en EK el uso de un vértice 𝑦 y en un paso 𝑗 siempre es en 𝑑𝑗(𝑦)
    - => 𝑑𝑘(𝑧) = 𝑑𝑘(𝑥) + 1
    - ∵ 𝑑𝑘(𝑧) = 𝑑𝑘(𝑥) + 1 > 𝑑𝑘+1(𝑥) + 1 = 𝑑𝑘+1(𝑧) + 2 ≥ 𝑑𝑘(𝑧) + 2
    - => 𝑑𝑘(𝑧) > 𝑑𝑘(𝑧) + 2 (ABSURDO)

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
  => 𝑛 - 1 ≤ 𝑚 (con 𝑛 - 1 = 𝑚 solo si Δ(𝑁) = 2)
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

- 𝑉𝑖 ∩ 𝑉𝑗 = ∅
- 𝑉 = ∪^𝑟_{𝑖=0} 𝑉𝑖
- 𝐸 ⊆ ∪^{𝑟−1}_{𝑖=0} (𝑉𝑖 × 𝑉𝑖+1)

Def. Dado un flujo ƒ sobre un network 𝑁 = (𝑉, 𝐸, 𝑐),
definimos el network auxiliar (relativo a ƒ) como el network por niveles dado por:

- 𝑟 = 𝑑ƒ(𝓈, 𝓉)
- 𝑉0 = {𝓈}
- 𝑉𝑟 = {𝓉}
- 𝑉𝑖 = {𝑥 : 𝑑ƒ(𝓈, 𝑥) = 𝑖} donde 0 < 𝑖 < 𝑟.
- 𝐸aux = {(𝑥, 𝑦) : ∃𝑖 : 𝑥∈𝑉𝑖, 𝑦∈𝑉𝑖+1 ∧
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

- PODAR:
  - funcionamiento de PODAR:
    - va revisando los vertices desde los niveles superiores para abajo:
      - si el vertice tiene aristas salientes no hace nada
      - si no tiene aristas salientes, borra el vértice y sus aristas entrantes
  - Complejidad de PODAR:
    - Borrar un vértice y sus lados
      - Se hace a lo sumo una vez por vértice
      - Su complejidad es 𝑂(𝑑(𝑥)).
      - Para todos los vertices = Σ_{𝑥∈𝑉} 𝑑(𝑥) = 𝑂(𝑚)
- Caminos:
  - Encontrar y aumentar caminos:
    - ∵ hay a lo sumo 𝑛 niveles
    - ∵ para aumentar el flujo se recorre el camino 2 veces.
      (encontrar camino y aumentar flujo)
      - => aumentar el flujo en un camino es 𝑂(𝑛)
  - Caminos aumentantes por fase:
    - ∵ cada camino aumentante satura (al menos) una arista
    - ∵ cada 𝑁𝐴 tiene a lo sumo 𝑚 aristas
      - => se puede saturar como máximo 𝑚 aristas antes de quedarse sin red.
    - => tendré 𝑂(𝑚) caminos aumentantes por fase.
  - Complejidad de aumentar todos los flujos: 𝑂(𝑛 × 𝑚).
- Complejidad de encontrar blocking flow:
  - cmp(PODAR) + cmp(caminos) = 𝑂(𝑚) + 𝑂(𝑛𝑚) = 𝑂(𝑛𝑚)

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

- Análisis por Palabras (𝐴, 𝑅, 𝐼):
  - La ejecución se modela como una sucesión de operaciones:
    - Avanzar (𝐴)
    - Retroceder (𝑅):
      borra un lado porque si retrocediste => llegaste a un vértice sin vecinos
    - Incrementar flujo (𝐼): borra un lado porque se satura (cuello de botella).
    - Se analizan subpalabras de la forma 𝐴…𝐴𝑋, donde 𝑋 es 𝑅 o 𝐼.
- Análisis del Flujo Bloqueante (𝑂(𝑛𝑚)):
  1. Cantidad de palabras:
      Cada palabra termina en una operación (𝑅 o 𝐼)
      que borra permanentemente al menos un lado del 𝑁𝐴.
      Por lo tanto, hay a lo sumo 𝑂(𝑚) palabras.
  2. Complejidad de cada palabra:
      - Cada operación 𝐴 y 𝑅 es 𝑂(1).
      - La operación 𝐼 recorre el camino para aumentar flujo y borrar lados,
        lo cual es 𝑂(𝑛).
      - Como cada palabra puede tener a lo sumo 𝑛 avances (niveles del 𝑁𝐴)
        antes de un 𝑅 o 𝐼, la complejidad de cada palabra es 𝑂(𝑛).
- Conclusión:
  cmp (hallar blocking flow) = 𝑂(𝑚 palabras) * 𝑂(𝑛 por palabra) = 𝑂(𝑛𝑚)

∵ cmp(Even) = max(𝑂(𝑚), cmp(hallar blocking flow)) * 𝑂(𝑛)
=> cmp(Even) = max(𝑂(𝑚), 𝑂(𝑛𝑚)) × 𝑂(𝑛) = 𝑂(𝑛𝑚) × 𝑂(𝑛) = 𝑂(𝑛^2𝑚) ■

#### Distancia en NAs

### Wave (Tarjan)

En Dinitz, el invariante de las funciones parciales 𝑔 obtenidas
durante la construcción del flujo bloqueante en un 𝑁𝐴 era "ser flujo",
y el algoritmo se detenía cuando 𝑔 era bloqueante.

En Wave, es al revés:  
el invariante es que 𝑔 sea bloqueante y nos detenemos cuando 𝑔 sea un flujo.

Def. (Operación push) hacer push en un vértice 𝑥,
  consiste en empujar cierta cantidad de flujo desde 𝑥
  hacia todos sus vecinos de Γ⁺(𝑥).

Def. (Operación pull) hacer pull en un vértice 𝑥
  consiste en enviar flujo desde todos los vecinos de Γ⁻(𝑥) hacia 𝑥.

### Funcionamiento mediante "olas"

1. **Inicialización:**
  Consiste en incrementar el valor de 𝑔 bloqueándolo inmediatamente.
  Esto se hace mandando desde 𝓈 hacia cada uno de sus vecinos
  todo el flujo que permitan las capacidades de los lados correspondientes.
  Esto bloquea a 𝑔, pero lo que queda no es un flujo:
  todos los vecinos de 𝓈 (los vértices del nivel 1) quedan con
  flujo entrante y nada saliente (están desbalanceados).

2. **Balanceo forward (FB):**
  Intentamos balancear los vértices desbalanceados
  haciendo push en cada uno de ellos y en sus vecinos sucesivamente hacia 𝓉.
  A esta operación Tarjan la llama forward balance (FB).
  Como empezamos desde 𝓈 y no desde el vértice de menor capacidad,
  puede ocurrir que no podamos balancear un vértice porque
  sus lados salientes se saturen. A estos vértices
  que no pueden enviar más flujo hacia adelante los marcamos como
  "bloqueados" (𝑩(𝑥) = 1).

3. **Balanceo backward (BB):**
  ¿Qué hacemos con los vértices bloqueados y desbalanceados?
  Lo único posible es devolver flujo a uno o más de los vértices
  que les enviaron flujo originalmente. A esta operación Tarjan la denomina
  backward balance (BB).
  Esta operación siempre tiene éxito para balancear el vértice,
  ya que en el peor de los casos devuelve todo el flujo recibido.

### El problema de devolver flujo y la solución en olas

Si el vértice bloqueado está en el nivel 1,
le devuelve flujo a 𝓈 sin problemas. Pero si está en un nivel superior,
al devolver flujo a sus predecesores,
estos (que ya estaban balanceados) volverán a quedar desbalanceados.

Si esos predecesores devolvieran inmediatamente el flujo hacia atrás hasta 𝓈,
se cometería un **error**,
pues se rompería la propiedad de que 𝑔 sea bloqueante.
Lo correcto es que esos vértices intenten primero redirigir
las unidades devueltas hacia otros vértices disponibles hacia adelante.
Solo si un vértice no puede enviar más flujo hacia adelante, es decir,
si está **bloqueado**, tiene permitido ejecutar BB(𝑥).

Para coordinar esto sin saber de antemano
si un vértice que recibe flujo devuelto podrá redirigirlo o no,
Tarjan diseña el algoritmo para operar en **olas** (waves):

- **Ola hacia adelante:**
  Recorre los vértices en orden BFS desde 𝓈 hacia 𝓉,
  mandando flujo hacia los niveles siguientes e
  intentando balancear los vértices mediante FB(𝑥).
  Si no logra balancear un vértice, lo marca como bloqueado (𝑩(𝑥) = 1).

- **Ola hacia atrás:**
  Recorre los vértices en orden BFS inverso desde 𝓉 hacia 𝓈,
  devolviendo flujo mediante BB(𝑥) **únicamente** desde
  los vértices que están bloqueados y desbalanceados.

Los vértices que quedan desbalanceados tras recibir flujo devuelto,
pero que **no están bloqueados**, simplemente esperan a la
**siguiente ola hacia adelante** para intentar redirigir ese exceso de flujo.
Wave alterna ciclos de ola hacia adelante y ola hacia atrás
hasta que todos los vértices intermedios queden balanceados.

#### Pseudocódigo de Wave

Al igual que en Dinic, Γ⁺(𝑥) y las capacidades 𝑐
hacen referencia al 𝑁𝐴. Además, necesitaremos:

- 𝐷(𝑥) = in(𝑥) − out(𝑥): el desbalanceo del vértice 𝑥.
- 𝘉(𝑥): vale 1 si el vértice 𝑥 está bloqueado, y 0 en caso contrario.
- 𝑀(𝑥): conjunto que registra los vértices que le han mandado flujo a 𝑥.

##### Inicialización

```python
g = 0
for x in V:
  D(x) = 0
  B(x) = 0

for x in Γ+(s):
  g(sx) = c(sx)   # se envía a todos los vecinos de 𝓈 toda su capacidad
  D(x) += c(sx)   # el desbalance de esa aristo por lado de x queda como la entrada
  D(s) -= c(sx)   #   y de 𝓈 como la -salida
  M(x) = {s}      # se anota que s mandóo flujo a x
```

##### Operación Forward Balance: FB(𝑥)

```python
def FB(x):
  # siempre que x tenga desbalanceo positivo y vecinos disponibles
  while (D(x) > 0) and (Γ+(x) ≠ ∅):
    Tomar y ∈ Γ+(x)
    if (B(y) == 1):
      Γ+(x) = Γ+(x) - {y}  # Si y está bloqueado, no tiene sentido mandarle flujo
    else:
      # a enviar, el minimo entre lo que le queda a xy y el desbalanceo de x
      A = min( D(x), c(xy) - g(xy) ) 
      g(xy) += A  # se envía flujo
      D(x) -= A   # se ajustan desbalanceos
      D(y) += A
      M(y) = M(y) ∪ {x}     # x envió flujo a y
      if (g(xy) == c(xy)):
        Γ+(x) = Γ+(x) - {y} # si se satura xy entonces y ya no está disponible
  
  if (D(x) > 0):
    B(x) = 1  # si todavía queda flujo, se bloquea x
```

##### Operación Backward Balance: BB(𝑥)

```python
def BB(x):
  while (D(x) > 0):           # Siempre tiene éxito
    Tomar y ∈ M(x)
    # a devolver,
    # el minimo entre el desbalanceo de x y lo que se envió por esa arista
    A = min( D(x), g(yx) )
    g(yx) -= A  # se devuelve flujo
    D(x) -= A   # se ajustan desbalanceos
    D(y) += A
    if (g(yx) == 0):
      M(x) = M(x) - {y}
```

##### Ciclo principal de las olas (Paso bloqueante)

Para evitar recorrer todos los vértices verificando si 𝐷(𝑥) = 0 en cada ciclo,
aprovechamos que la suma de todos los 𝐷(𝑥) en el grafo siempre es cero
(cada envío de 𝑎 unidades resta 𝑎 a un vértice y suma 𝑎 a otro).
Como 𝓈 es el único vértice con desbalanceo negativo,
todos los vértices intermedios estarán balanceados
(𝐷(𝑥) = 0 para todo 𝑥 ≠ 𝓈, 𝓉) si y solo si 𝐷(𝓈) + 𝐷(𝓉) = 0.

Notamos como «𝓈+1» al primer vértice en el orden BFS posterior a 𝓈,
y como «𝓉−1» al último vértice en el orden BFS anterior a 𝓉.

```python
while (D(s) + D(t) ≠ 0):
  # Ola hacia adelante (orden BFS)
  for x in range(s + 1, t):
    if (D(x) > 0):
      FB(x)

  # Ola hacia atrás (orden BFS inverso)
  for x in range(t - 1, s + 1):
    if (B(x) == 1) and (D(x) > 0):
      BB(x)

return g
```

#### Complejidad de Wave

Teo. La complejidad de Wave es 𝑂(𝑛³).

Prueba:

Wave es de tipo Dinic. Como la distancia entre 𝓈 y 𝓉
aumenta en los 𝑁𝐴s sucesivos,
hay 𝑂(𝑛) 𝑁𝐴: la distancia no puede superar 𝑛−1.
 => Basta con demostrar que encontrar un flujo bloqueante en un 𝑁𝐴 es 𝑂(𝑛²).

En cada 𝑁𝐴, construir los niveles con BFS cuesta 𝑂(𝑚).  
Veamos que hallar el flujo bloqueante cuesta 𝑂(𝑛²).
Entonces tendremos:

cmp(Wave) = #niveles × (cmp(construir nivel) + cmp(hallar flujo bloqueante))  
cmp(Wave) = 𝑂(𝑛) × (𝑂(𝑚) + cmp(hallar flujo bloqueante))  

El proceso de flujo bloqueante consta de olas hacia adelante y atrás.
En una ola hacia adelante, un vértice 𝑥 envía flujo a 𝑧;
el lado puede saturarse (caso 𝑆) o no (caso 𝑃).
En un balanceo hacia atrás, 𝑥 devuelve flujo a 𝑣;
el lado puede vaciarse (caso 𝑉) o no (caso 𝑄).

- **Caso 𝑆:** 𝑥 envía flujo a 𝑧 y 𝑥𝑧 se satura  

  Cuando un lado 𝑥𝑧 se satura, se elimina de Γ⁺(𝑥).
  Para volver a usarse, primero debería des-saturarse
  (que 𝑧 le devuelva flujo a 𝑥), lo cual solo ocurre si y se bloquea.
  Pero si 𝑧 está bloqueado, 𝑥 nunca volverá a mandarle flujo.
  Por ende, cada lado se satura a lo sumo una vez:
    => 𝑆 ≤ 𝑚
    => el costo total es 𝑂(𝑚)

- **Caso 𝑉:** 𝑧 devuelve flujo a 𝑥 y 𝑧𝑥 se vacía  

  Un lado 𝑧𝑥 solo puede vaciarse si 𝑥 está bloqueado.
  Pero una vez que 𝑥 está bloqueado, 𝑧 nunca más podrá enviarle flujo,
  Por lo que 𝑧𝑥 no vuelve ni a llenarse ni a vaciarse
  Cada lado se vacía a lo sumo una vez:
    => 𝑉 ≤ 𝑚
    => el costo total es 𝑂(𝑚)
  
- **Caso 𝑃:** no se satura  

  En cada ola hacia adelante, en cada vértice puede haber a lo sumo
  un lado por el que se envía flujo sin saturarlo;
  todos los anteriores se saturan. Ese envío cuesta 𝑂(1),
  Por otro lado, cada ola hacia adelante hace a lo sumo 𝑛 − 2 forward balances
    ∵ 𝑃 ≤ (𝑛 − 2)
    => 𝑃 cuesta 𝑂(𝑛) por ola.
  Además, en cada ola hacia adelante (salvo la última), al menos un
  vértice queda bloqueado.
  Como los vértices nunca se desbloquean, hay a lo sumo 𝑛 olas hacia adelante.
    => 𝑂(𝑛) ⋅ 𝑛 = O(𝑛²)

- **Caso 𝑄:**  

  Análogamente, en cada ola atrás hay a lo sumo un lado por vértice que no se vacía;
  cada ola atrás hace a lo sumo 𝑛 − 2 backward balances
  el costo de 𝑄 también es 𝑂(𝑛) por ola.
  La cantidad de olas hacia adelante y atrás es la misma.
  => hay a lo sumo 𝑛 olas atrás,
  => 𝑄 cuesta 𝑂(𝑛) ⋅ 𝑛 = O(𝑛²)
  
=> hallar un flujo bloqueante cuesta 𝑂(𝑚) + 𝑂(𝑚) + 𝑂(𝑛²) + 𝑂(𝑛²) = 𝑂(𝑛²).
Con 𝑂(𝑛) 𝑁𝐴, cada uno con construcción de niveles en 𝑂(𝑚),

=> cmp(Wave) = 𝑂(𝑛) × (𝑂(𝑚) + 𝑂(𝑛²))  
∵ 𝑚 ≤ 𝑛(𝑛−1)/2 = 𝑂(𝑛²)  
  => cmp(Wave) = 𝑂(𝑛³)

la complejidad total es 𝑂(𝑛³). ■

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

- ⇒
  - Sup existe un matching completo 𝑀 de 𝑋 a 𝑌
  - => 𝑀 induce una función inyectiva ƒ : 𝑋 → 𝑌 | ƒ(𝑥) ∈ Γ(𝑥) ∀𝑥 ∈ 𝑋
  - ∵ ƒ es inyectiva
  - => |ƒ(𝑆)| = |𝑆| ∀𝑆 ⊆ 𝑋
  - => ƒ(𝑆) ⊆ Γ(𝑆) ⇒ |𝑆| ≤ |Γ(𝑆)|
- ⇐
  - Sup que:
    1. se cumple que |Γ(𝑆)| ≥ |𝑆| ∀𝑆 ⊆ 𝑋 (la condición de Hall)
    2. corriendo algoritmo para hallar un matching maximal llego a
    un matching maximal 𝑀 : |𝐸(𝑀)| < |𝑋|
  - Construiremos un 𝑆 ⊆ 𝑋 : |Γ(𝑆)| < |𝑆| (violando Hall y llegando a un absurdo)
    - Corramos el algoritmo de extensión de matching sobre el matching 𝑀
    - ∵ 𝑀 no cubre a 𝑋
      - => existen filas sin matchear
    - Sea 𝑆 el conjunto de filas etiquetadas
    - Sea 𝑆0 ⊆ 𝑆 el conjunto de filas estiquetadas con "∗"
      (las filas que no forman parte del matching)
      - ∵ 𝑀 no es completo => 𝑆0 ≠ ∅.
    - Sea 𝑇 el conjunto de columnas etiquetadas
    - Sea 𝑇1 ⊆ 𝑇 el conjunto de columnas estiquetadas por 𝑆0
    - En gral.:
      - 𝑇𝑖+1 serán las columnas etiquetadas por 𝑆𝑖
      - 𝑆𝑗 son las filas etiquetadas por 𝑇𝑗
    - Las sucesiones son finitas
    - 𝑆 = 𝑆0∪𝑆1∪… con uniones disjuntas
    - 𝑇 = 𝑇1∪𝑇2∪… con uniones disjuntas
    - Revisemos una columna de 𝑇
      - o está libre y el matching se extiende
        - no puede pasar porque 𝑀 es maximal
      - o encuentra una fila matcheada de esa columna y la etiqueta
    - => el algoritmo nunca se detiene cuando se pasa de un 𝑇𝑗 a un 𝑆𝑗
    - => el algoritmo solo se detiene al pasar de un 𝑆𝑘 a un 𝑇𝑘+1 que es vacío
      (cuando al revisar un conjunto de vecinos para una fila, no hay vecinos libres)
    - ∵ cada columna etiqueta solo a la fila con la que está matcheada
      , 𝑇𝑗 crea a 𝑆𝑗 y el algoritmo nunca se detiene en 𝑇
      - => |𝑇𝑗| = |𝑆𝑗|
    - |𝑆| = |𝑆0| + |𝑆1| + … + |𝑆𝑘| = |𝑆0| + |𝑇1| + |𝑇2| + … + |𝑇𝑘|
    = |𝑆0| + |𝑇|
    - ∵ 𝑆0 ≠ ∅
    - => |𝑆| = |𝑆0| + |𝑇| > |𝑇|
    - ∴ |𝑆| > |𝑇| Si demostráramos que 𝑇 = Γ(𝑆), la prueba concluiría.
      - 𝑇 ⊆ Γ(𝑆)
        - 𝑇 son las columnas etiquetadas
        - las columnas son etiquetadas por filas de 𝑆.
        - Y una fila solo etiqueta columnas vecinas.
        - => 𝑇 ⊆ Γ(𝑆).
      - Γ(𝑆) ⊆ 𝑇
        - Probemos que 𝑦 ∈ Γ(𝑆) => 𝑦 ∈ 𝑇
        - ∴ 𝑦 ∈ Γ(𝑆)
          - => ∃𝑥 ∈ 𝑆 : 𝑦 ∈ Γ(𝑥)
        - ∴ 𝑥 ∈ 𝑆
          - => 𝑥 es encontrado por el algoritmo
          - cuando 𝑥 es encontrado 𝑥 etiqueta a 𝑦
        - ∴ 𝑦 está etiquetado
          - => 𝑦 ∈ 𝑇 ■

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

Se deben demostrar dos condiciones fundamentales:

1. **Su pertenencia a la clase NP:**
  Dada una solución propuesta, se puede verificar en tiempo polinomial.
2. **Si es NP-Hard:**
  Todos los problemas en NP pueden reducirse a él en tiempo polinomial
  (para esto, probaremos $3SAT \leq_P 3COLOR$).

entonces

1. 3-COLOR es NP
    - Una solución propuesta es una asignación de colores $C: V \to \{0, 1, 2\}$
      para los $n$ vértices del grafo.
    - Se verifica recorriendo todas las aristas $uv \in E$ y
      comprobando que $C(u) \neq C(v)$.
    - Esta verificación tiene una complejidad de $O(m)$,
      con $m = \vert{}E\vert{}$, lo cual es claramente polinomial.

2. 3-COLOR es NP-Hard ($3SAT \leq_P 3COLOR$)

Debemos transformar en tiempo polinomial una instancia $B$ de 3SAT
en un grafo $G$, mostrando que $B$ es satisfacible $\iff$ $G$ es 3-colorable.

Sabemos que $B$ es de la forma $D_1 \land \dots \land D_m$ con variables $x_1,
\dots, x_n$, donde cada cláusula es $D_j = l_{j1} \lor l_{j2} \lor l_{j3}$,
y $l_{jk}$ es una variable o su negación.

**Construcción del Grafo $G$ (Gadgets):**

- **Vértices $V(G)$:**
$\{u_1, \dots, u_n, w_1, \dots, w_n\}$ (para las variables) $\cup$
$\bigcup_{j=1}^m \{a_{j1}, a_{j2}, a_{j3}, e_{j1}, e_{j2}, e_{j3}\}$ (cláusulas)
$\cup$ $\{CAPI, t\}$ (vértices base).
- **Aristas $E(G)$:**
- Triángulos de variables:
  $\{(u_i, t), (t, w_i), (w_i, u_i)\}$ para $i = 1, \dots, n$.
- Garras de cláusulas:
  $\{(a_{j1}, a_{j2}), (a_{j2}, a_{j3}), (a_{j3}, a_{j1})\}$ y
  $\{(a_{jr}, e_{jr})\}$ para $j = 1, \dots, m$ y $r = 1,2,3$.
- Conexiones base:
  $\{(CAPI, e_{jr})\}$ para $j = 1, \dots, m$ y $r = 1,2,3$
  $\{(CAPI, t)\}$.
- Conexiones de literales:
  $\{(e_{jr}, v(l_{jr}))\}$ donde $v(l_{jr}) = u_q$ si el literal es $x_q$,
  o $v(l_{jr}) = w_q$ si el literal es $\neg x_q$.

$\chi(G) \leq 3 \implies B$ satisfacible

- Como en $G$ hay triángulos, necesitamos al menos 3 colores: $\chi(G) = 3$.
- Sea $C$ un coloreo propio de $G$ con colores $\{T, F, Z\}$.
- Como $(CAPI, t)$ es un lado, podemos asumir sin pérdida de generalidad que
  $C(CAPI) = T$ y $C(t) = F$.
- Definimos el vector de asignación $\vec{b}$ para $B$:
- $b_i = 1$ si $C(u_i) = C(CAPI) = T$
- $b_i = 0$ en caso contrario (es decir, $C(u_i) = Z$)

- Para probar que $B(\vec{b}) = 1$, hay que ver que
  $\forall j, \exists r : l_{jr}(\vec{b}) = 1$.
- Sea $j \in \{1, \dots, m\}$. Como los $a_{jr}$ forman un triángulo,
  uno de ellos debe tener el color de $t$.
  Es decir, $\exists r : C(a_{jr}) = C(t) = F$.
- Como $e_{jr}a_{jr} \in E(G) \implies C(e_{jr}) \neq C(a_{jr}) = F$.
- Como $e_{jr}(CAPI) \in E(G) \implies C(e_{jr}) \neq C(CAPI) = T$.
- Por lo tanto, obligatoriamente **$C(e_{jr}) = Z$**.
- Como $e_{jr}v(l_{jr}) \in E(G) \implies C(v(l_{jr})) \neq C(e_{jr}) = Z$.
- Como $tv(l_{jr}) \in E(G) \implies C(v(l_{jr})) \neq C(t) = F$.
- Por lo tanto, obligatoriamente **$C(v(l_{jr})) = T$** (el color de CAPI).
- Al tener el vértice el color de CAPI,
  esto implica por nuestra definición que el literal es verdadero:
  $l_{jr}(\vec{b}) = 1$.
- Como esto ocurre para toda cláusula $j$, $B$ es satisfacible $\blacksquare$

$B$ satisfacible $\implies \chi(G) \leq 3$

Supongamos que $B$ es satisfacible por una asignación de verdad $\vec{b}$.
Construiremos un 3-coloreo válido para $G$ usando los colores $\{T, F, Z\}$.

1. **Vértices Base:** Asignamos $C(CAPI) = T$ y $C(t) = F$.
2. **Vértices de Variables ($u_i, w_i$):**
    - Si $b_i = 1$, asignamos $C(u_i) = T$ y $C(w_i) = Z$.
    - Si $b_i = 0$, asignamos $C(u_i) = Z$ y $C(w_i) = T$.
    - (Esto es válido porque forman un triángulo con $t=F$ y nunca comparten color).

3. **Vértices de Cláusulas ($a_{jr}, e_{jr}$):**
    - Como $\vec{b}$ satisface $B$, en cada cláusula $j$ existe al menos un
      literal verdadero. Elegimos uno y lo llamamos $r$.
    - Dado que el literal $r$ es verdadero,
      sabemos por el paso anterior que su vértice asociado
      tiene color $T$ ($C(v(l_{jr})) = T$).
    - **Para el literal verdadero ($r$):**
    - Coloreamos su brazo: $C(e_{jr}) = Z$.
      (Válido porque se conecta a CAPI ($T$) y a $v(l_{jr})$ ($T$)).
    - Coloreamos su vértice de garra: $C(a_{jr}) = F$.
      (Válido porque se conecta a $e_{jr}$ ($Z$)).

- **Para los otros dos literales falsos (llamémoslos $p$ y $q$):**
- Coloreamos sus brazos: $C(e_{jp}) = C(e_{jq}) = F$.
  (Válido porque se conectan a CAPI ($T$) y a sus respectivos vértices $v$ que,
  como son literales falsos o verdaderos remanentes,
  tienen color $Z$ o $T$. Ninguno es $F$).
- Coloreamos sus vértices de garra: $C(a_{jp}) = T$ y $C(a_{jq}) = Z$.

- (Esto es válido porque el triángulo interno de la garra
  $a_{jr}, a_{jp}, a_{jq}$ queda coloreado perfectamente con
  los tres colores distintos: $F, T, Z$).

Como pudimos asignar colores válidos a todos los vértices del grafo $G$ sin que
ninguna arista conecte dos vértices del mismo color,
concluimos que $G$ es 3-colorable ($\chi(G) \leq 3$) $\blacksquare$

### Matrimonio 3D

Teo. Matrimonio 3D es NP-completo.

Prueba:

Matrimonio 3D pertenece a NP: dado un matching,
se verifica en tiempo polinomial que sus ternas son disjuntas y
cubren todos los vértices.
Para probar que es NP-hard, reducimos 3-SAT a Matrimonio 3D.

Sea 𝐵 = 𝐷₁ ∧ ⋯ ∧ 𝐷ₘ una instancia de 3-SAT, con variables 𝑥₁,…,𝑥ₙ y
cláusulas 𝐷ⱼ = ℓⱼ₁ ∨ ℓⱼ₂ ∨ ℓⱼ₃.
Construimos un 3-hipergrafo 𝐻 con tres partes disjuntas 𝑋, 𝑌 y 𝑍,
cada una de tamaño 2𝑛𝑚:

X = {𝑎ᵢⱼ : 1 ≤ 𝑖 ≤ 𝑛, 1 ≤ 𝑗 ≤ 𝑚} ∪ {𝑠ⱼ : 1 ≤ 𝑗 ≤ 𝑚} ∪ {𝑔ₖ : 1 ≤ 𝑘 ≤ 𝑚(𝑛−1)}

Y = {𝑏ᵢⱼ : 1 ≤ 𝑖 ≤ 𝑛, 1 ≤ 𝑗 ≤ 𝑚} ∪ {𝑡ⱼ : 1 ≤ 𝑗 ≤ 𝑚} ∪ {ℎₖ : 1 ≤ 𝑘 ≤ 𝑚(𝑛−1)}

Z = {𝑢ᵢⱼ : 1 ≤ 𝑖 ≤ 𝑛, 1 ≤ 𝑗 ≤ 𝑚} ∪ {𝑤ᵢⱼ : 1 ≤ 𝑖 ≤ 𝑛, 1 ≤ 𝑗 ≤ 𝑚}

donde 𝑖 ∈ {1,…,𝑛}, 𝑗 ∈ {1,…,𝑚} y 𝑘 ∈ {1,…,𝑚(𝑛−1)}. Definimos las ternas:

E₀ = {{𝑎ᵢⱼ, 𝑏ᵢⱼ, 𝑢ᵢⱼ} : 1 ≤ 𝑖 ≤ 𝑛, 1 ≤ 𝑗 ≤ 𝑚}

E₁ = {{𝑎ᵢ,ⱼ₊₁, 𝑏ᵢⱼ, 𝑤ᵢⱼ} : 1 ≤ 𝑖 ≤ 𝑛, 1 ≤ 𝑗 ≤ 𝑚}

E₂ = {{𝑔ₖ, ℎₖ, 𝑧} : 𝑧 ∈ Z, 1 ≤ 𝑘 ≤ 𝑚(𝑛−1)}

En 𝐸₁ se toma 𝑗+1=1 cuando 𝑗=𝑚. A cada literal ℓⱼᵣ le asociamos

𝑣ⱼᵣ = 𝑢ᵢⱼ si ℓⱼᵣ = 𝑥ᵢ; 𝑣ⱼᵣ = 𝑤ᵢⱼ si ℓⱼᵣ = ¬𝑥ᵢ.

E₃ = {{𝑠ⱼ, 𝑡ⱼ, 𝑣ⱼᵣ} : 1 ≤ 𝑗 ≤ 𝑚, 1 ≤ 𝑟 ≤ 3}.

La construcción tiene tamaño polinomial.
Mostramos que 𝐵 es satisfacible sii 𝐻 tiene un matching perfecto.

**(⇒)** Sea 𝐛 = (𝑏₁,…,𝑏ₙ) una asignación que satisface 𝐵.
Tomamos en 𝐹₀ las ternas {𝑎ᵢⱼ, 𝑏ᵢⱼ, 𝑢ᵢⱼ} para las variables con 𝑏ᵢ = 0,
y en 𝐹₁ las ternas {𝑎ᵢ,ⱼ₊₁, 𝑏ᵢⱼ, 𝑤ᵢⱼ} para las variables con 𝑏ᵢ = 1.
Estas ternas son disjuntas. Para cada cláusula 𝐷ⱼ elegimos un literal verdadero
ℓⱼᵣⱼ y agregamos {𝑠ⱼ, 𝑡ⱼ, 𝑣ⱼᵣⱼ} a 𝐹₃.
Estas ternas tampoco intersectan 𝐹₀ ni 𝐹₁: si 𝑣ⱼᵣⱼ = 𝑢ᵢⱼ,
entonces 𝑏ᵢ = 1, así que 𝑢ᵢⱼ no está en 𝐹₀; si 𝑣ⱼᵣⱼ = 𝑤ᵢⱼ,
entonces 𝑏ᵢ = 0, así que 𝑤ᵢⱼ no está en 𝐹₁.

Sea 𝑁 el conjunto de vértices de 𝑍 que aún no están cubiertos por 𝐹₀∪𝐹₁∪𝐹₃.
Si 𝑝 variables valen 0 y 𝑞 valen 1, 𝐹₀ cubre 𝑝𝑚 vértices de 𝑍,
𝐹₁ cubre 𝑞𝑚 y 𝐹₃ cubre 𝑚. Por tanto,

|𝑁| = |𝑍| − 𝑚(𝑝+𝑞+1) = 2𝑛𝑚 − 𝑚(𝑛+1) = 𝑚(𝑛−1).

Emparejamos biyectivamente los índices 𝑘 con 𝑁 y
agregamos a 𝐹₂ las ternas {𝑔ₖ,ℎₖ,𝑓(𝑘)}.
Son disjuntas de las ya elegidas y entre sí. Así,
𝐹₀∪𝐹₁∪𝐹₂∪𝐹₃ cubre todos los vértices y es un matching perfecto.

**(⇐)** Sea 𝑀 un matching perfecto de 𝐻.
Para cada 𝑖, los vértices 𝑎ᵢⱼ y 𝑏ᵢⱼ solo aparecen en ternas de 𝐸₀ o 𝐸₁.
Si alguna terna de 𝐸₀ los cubre,
la condición de disjunción fuerza sucesivamente las ternas de 𝐸₀ para todo 𝑗;
llamamos a esto el caso 0. Si alguna terna de 𝐸₁ los cubre,
fuerza las de 𝐸₁ para todo 𝑗; es el caso 1.
Definimos 𝑏ᵢ=0 en el caso 0 y 𝑏ᵢ=1 en el caso 1.

Para cada cláusula 𝐷ⱼ, como 𝑠ⱼ y 𝑡ⱼ deben estar cubiertos,
alguna terna {𝑠ⱼ, 𝑡ⱼ, 𝑣ⱼᵣ} pertenece a 𝑀. Si ℓⱼᵣ = 𝑥ᵢ,
entonces 𝑣ⱼᵣ = 𝑢ᵢⱼ; su uso impide la terna de 𝐸₀,
así que se da el caso 1 y 𝑏ᵢ = 1. Luego ℓⱼᵣ(𝐛) = 1. Si ℓⱼᵣ = ¬𝑥ᵢ,
entonces 𝑣ⱼᵣ = 𝑤ᵢⱼ; su uso impide la terna de 𝐸₁,
así que se da el caso 0 y 𝑏ᵢ = 0. También en este caso ℓⱼᵣ(𝐛) = 1. Por tanto,
cada cláusula tiene un literal verdadero y 𝐵(𝐛) = 1. ■

---

## Códigos

### Corrección de errores

Def. La distancia de Hamming entre dos palabras 𝛼, 𝛾 ∈ {0, 1}∗
es 𝑑𝐻 = #{𝑖 : 𝛼𝑖 ≠ 𝛾𝑖 }.

Def. El disco de radios alrededor de 𝛼 ∈ {0, 1}^𝑛
es 𝐷_𝑟(𝛼) = {𝛾 ∈ {0, 1}^𝑛 : 𝑑𝐻 (𝛼, 𝛾) ≤ 𝑟}

Def. Un código 𝐶 ⊆ {0, 1}^𝑛 detecta (hasta) 𝑟 errores si
∀𝛼 ∈ 𝐶 : 𝐷𝑟 (𝛼) ∩ 𝐶 = {𝛼}

Def. Un código 𝐶 ⊆ {0, 1}^𝑛 corrige hasta 𝑡 errores si
∀𝛼, 𝛾 ∈ 𝐶 : 𝛼 ≠ 𝛾 : 𝐷𝑡(𝛼) ∩ 𝐷𝑡(𝛾) = ∅

Def. Dado 𝐶 ⊆ {0, 1}^𝑛, definimos
𝛿(𝐶) = min{𝑑𝐻 (𝛼, 𝛾) : 𝛼, 𝛾 ∈ 𝐶 ∧ 𝛼 ≠ 𝛾}

Teo. Sea 𝐶 ⊆ {0, 1}^𝑛 y 𝛿 = 𝛿(𝐶). Entonces (

1) 𝐶 detecta 𝛿 − 1 errores, pero no detecta 𝛿;
2) Si 𝑡 = floor((𝛿−1)/2), entonces 𝐶 corrige 𝑡 errores pero no 𝑡 + 1.

#### Cota de Hamming

Sean 𝐶 ⊆ {0, 1}^𝑛; 𝛿 = 𝛿(𝑐) y 𝑡 = floor((𝛿−1)/2)
Entonces:
|𝐶| ≤ 2^𝑛/ 1 + 𝑛 + 𝑛𝐶2 + … + 𝑛𝐶𝑡 (𝑛𝐶𝑘 = 𝑛!/(𝑘!(𝑛−𝑘)!))

Prueba:

Sea 𝐴 = ∪_{𝑣 ∈ 𝐶} 𝐷𝑡(𝑣).
∴ 𝑡 = floor((𝛿−1)/2),
  => 𝐶 corrige 𝑡 errores
  ∀𝑣, 𝑤 ∈ 𝐶 : 𝑣 ≠ 𝑤 : 𝐷𝑡(𝑣) ∩ 𝐷𝑡(𝑤) = ∅
    (la unión 𝐴 es disjunta)

  => |𝐴| = ∑_{𝑣 ∈ 𝐶} |𝐷𝑡(𝑣)|

Sean 𝑆𝑟(𝑣) = {𝑤 ∈ {0, 1}^𝑛 : 𝑑𝐻(𝑣, 𝑤) = 𝑟}
=> 𝐷𝑡(𝑣) = ∪_{𝑟 = 0}^{𝑡} 𝑆𝑟(𝑣) (unión obviamente disjunta)

|𝐷𝑡(𝑣)| = ∑_{𝑟 = 0}^{𝑡} |𝑆𝑟(𝑣)|

veamos cuanto vale |𝑆𝑟(𝑣)|:

𝑤 ∈ 𝑆𝑟(𝑣) <=> 𝑑𝐻(𝑣, 𝑤) = 𝑟
<=> 𝑤 difiere en exactamente 𝑟 bits de 𝑣

=>  hay un subconjunto de los 𝑛 bits de las palabras que tiene 𝑟 elementos tq
𝑤 difiere de 𝑣 en esos 𝑟 bits.

- dado 𝑤 ∈ 𝑆𝑟(𝑣) puedo obtener 𝑟 bits de en los que 𝑤 difiere de 𝑣
- dado un conjunto de 𝑟 bits puedo obtener un 𝑤 tal que 𝑑𝐻(𝑣, 𝑤) = 𝑟

=> hay biyección entre 𝑆𝑟(𝑣) y el conjunto de subconjuntos de 𝑟 bits

=> la cardinalidad de estos 2 es la misma
|𝑆𝑟(𝑣)| = 𝑛𝐶𝑟 => |𝐷𝑡(𝑣)| = ∑_{𝑟 = 0}^{𝑡} 𝑛𝐶𝑟

|𝐴| = ∑_{𝑣 ∈ 𝐶} |𝐷𝑡(𝑣)| = ∑_{𝑣 ∈ 𝐶} ∑_{𝑟 = 0}^{𝑡} 𝑛𝐶𝑟

∑_{𝑣 ∈ 𝐶} ∑_{𝑟 = 0}^{𝑡} 𝑛𝐶𝑟 es |𝐶| veces lo mismo (∑_{𝑟 = 0}^{𝑡} 𝑛𝐶𝑟)

=> |𝐴| = |𝐶| ∑_{𝑟 = 0}^{𝑡} 𝑛𝐶𝑟

=> |𝐶| = |𝐴| / ∑_{𝑟 = 0}^{𝑡} 𝑛𝐶𝑟

∴ 𝐴 ⊆ {0, 1}^𝑛 => |𝐴| ≤ 2^𝑛
=> |𝐶| ≤ 2^𝑛 / ∑_{𝑟 = 0}^{𝑡} 𝑛𝐶𝑟

### Teorema de la Distancia Mínima y la Matriz de Chequeo

Teo. Sea 𝐶 un código lineal con matriz de chequeo 𝐻.
  Entonces 𝛿(𝐶) es el mínimo número de columnas LD de 𝐻

### Códigos cíclicos

#### Polinomio Generador

Sea 𝐶 cíclico y 𝑔(𝑥) su polinomio generador. Entonces

1. 𝐶 = {𝑝(𝑥) : 𝑔𝑟(𝑝(𝑥)) < 𝑛 ∧ 𝑔(𝑥) | 𝑝(𝑥)};
  es decir, los códigos son los múltiplos de 𝑔(𝑥) con grado < 𝑛.
2. 𝐶 = {𝑔(𝑥) ◦ 𝑣(𝑥) : 𝑣 es un polinomio arbitrario}
    - $C = {v(x) ⊙ g(x) : v es un polinomio cualquiera}$
3. 𝑔𝑟(𝑔(𝑥)) = 𝑛 − 𝑘 con 𝑘 = dim𝐶
4. 𝑔(𝑥) | 1 + 𝑥^𝑛
5. Si 𝑔(𝑥) = 𝑔0 + 𝑔1𝑥 + … , entonces 𝑔0 = 1
