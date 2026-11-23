# Códigos de corrección de Errores

- $C$ código  
  - $δ = δ(C) = min(diferencias \; entre \; palabras \; ∈ C)$

Entonces $C$:

- Detecta $\delta - 1$ errores.
- Corrige $\left\lfloor\frac{\delta-1}{2}\right\rfloor$ errores.

## Códigos lineales

### Matriz generadora

para $C$ código lineal, su matriz generadora es una matriz cuyas filas son base de $C$  

- $K = dim(C)$, $n = long(C)$ y $G matriz generadora de C$
  - $G$ tiene $K$ filas y $n$ columnas
  - $G = K×n$

- $K = dim(C)$
  - $|C| = 2^K$

$∀ v ∈ C \quad ∃! \; u = u_1, ..., u_k ∈ \{0,1\} : v = u_1G_1 + ... + u_kG_k = uG$

### Matriz de Chequeo

$C$ código lineal y $H$ su matriz de chequeo

- $δ = min(columnas \, LD \, de \, H)$
  - Si $H$ no tiene columna $0$ o repetidas $=>$ $δ ≥ 3$
- Encontrar errores
  - $Hw^t = 0 => v=w$
  - $Hw^t = H^i => v = w + e_i$
- $G = [A|I] => H = [I|A^t]$

## Códigos cíclicos

$v⊙w = v(x)w(w) \, mod(1+x^n)$

Generador de $C = g(x)$

- es el polinomio de menor grado en $C$ (no nulo)  

- $K = dim(C)$, $n = long(C)$ y $g(x) generador de C$
  - $gr(g) = n-K \quad K = n - gr(g)$
  - $g | (1+x^n)$
  - $g(0) = 1$

### Codificación y decodificación

$q : gr(q) < K$

#### Método 1

- Se genera la palabra
  - $p = qg$

#### Método 2

- Se genera la palabra
  - $q(x)x^{n-k} + (q(x)x^{n-k} \; mod g)$

- Se decodifica la palabra $w$,
con $t = \left\lfloor\frac{\delta-1}{2}\right\rfloor$
y $e$ el error:
  - Se calcula el síndrome:
    - $s_0 = w \; mod g$
    - $|s_0| ≤ t => e = s_0$
  - Se itera como:
    - $s_i = s_{i-1}x \; mod g$
  - Hasta obtener un $j : |s_j| ≤ t$
    - $e = x^{n-j}s_j \; mod(1+x^n)$
