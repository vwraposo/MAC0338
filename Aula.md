# MAC 0338 - Análise de Algoritmos
#### Carlinhos E. Ferreira:
* E-mail: cef@ime.usp.br
* Sala: 108C
* Tel: 3091-6079
* Monitoria, segunda-feira 12 hrs sala B-129.

#### Bibliografia
* CLRS - Introduction to Algorithms 2a edição
* Material extra: slides da Chris, páginas do PF ...

#### Programa
1. **Notação assintótica** ($\mathcal{O}$)
1. **Notação assintótica** ($\mathcal{O}$)
2. **Análise:**
    * Pior caso
    * Probabilistíca
    * Amortizada
3. **Paradigmas de projetos de alforitmos:**
    * Divisão e conquista
    * PD
    * Guloso
4. **Introdução a teoria de complexidade computacional:**
    * Redução de problemas
    * classes de complexidade

#### Provas
* **P1** 7/4 - 10:00
* **P2** 26/5 - 10:00
* **P3** 5/7 - 14:00
*  MP = média aritmética das Provas
* Várias Listas

---

# Aula 8/3

Nesta aula mostraremos um exemplo de análise de algoritmos. Mostraremos porque as constantes podem ser ignoradas introduzindo a notação $\mathcal{O} ()$
    * CLRs 1.1, 1.2, 2.1, 2.2

### Probelma: Ordenar um vetor

Rearranjar os elementos de um vetor $A[1 \dots n]$ de forma que $A[1] \leq A[2] \leq \dots \leq A[n]$

**Exemplo:**

Dado:

42 | 12 | 10 | 7 | 15 | 21 | 20 | 32 | 8 | 14
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---|

Saída:

7 | 8 | 10 | 12 | 14 | 15 | 20 | 21 | 32 | 42
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---|

1. Ordenação por inserção:

    A ideia do algoritmo é, em cada passo, considerar o vetor dividido em duas partes:

    Ordenado |  Bagunça
    --- | ---

    Pegamos o primeiro elemento da parte bagunçadae procuramos o lugar dele na parte ordenada

    \[
        \underbrace{\text{20 | 25 | 35 | 40 | 44 | 55}}_{ordenado}\text{ | 38 | 99 | 10 | 65 | 50}
    \]

    **Ideia:**

    Comparamos com os valores anteriores a ele, deslocando o número pra direita se for maior,  até encontrar um número menor ou igual ou até verificar todos

    **Código:**

    Ordena por inserção (A, n)
    ```
        0. j <- 2
        1. enquanto j <= n faça
        2.    x <- A[j]
        3.    i <- j - 1
        4.    enquanto i >= e A[i] > x
        5.        A[i+1] <- A[i]
        6.        i <- i - 1
        7.    A[i+1] <- x
        8.    j <- j + 1
    ```
    Invariante do primeiro while, até a posição $j-1$ o vetor está ordenado. Provar o segundo while com indução, um pouco mais complicado.

    **Quantas atribuições o algoritmo faz?**

    A primeira observação é que depende da instância. Para algumas instâncias o número de atribuições é menor que para outras. O que queremos? O número mínimo, máximos ou médio de atribuições? O melhor caso, pior caso ou caso médio?

    **Análise:**

    Linha | Atribuição
    --- | ---
    0 | $1$
    1 | $0$
    2 | $n-1$
    3 | $n-1$
    4 | $0$
    5 | $?\leq j-1 \leq n-1$
    6 | $?\leq j-1 \leq n-1$
    7 | $n-1$
    8 | $n-1$

    Olhando para o trecho 4-6 qual o número máximo de atribuições em função de j?

    $$j-1$$

    Como sei que $j \leq n$, o total de atribuições é:

    \[
        \leq 1 + n-1 + n-1 + (n-1).(n-1+n-1) + n-1 + n-1 \\
        = 1 + 4n - 4 + (n-1)(2n -2) = \\
        2n^2 - 1
    \]

    Essa análise está muito pessimista, pois não é verdade que o loop interno executa n-1 vezes para todo j.

    $j$ | 5 - 6
    --- | ---
    2  | $\leq 1$
    3 | $\leq 2$
    4 | $\leq 3$
    $\vdots$ | $\vdots$

    O número de atribuições:
    \[
        \leq 1 + 4.(n-1) + 2 + 4 + \dots + 2(n-1) \\
        = 1 + 4(n-1) + 2 (1 + \dots + n-1) \\
        = 1 + 4(n-1) + 2 (\frac{n. (n-1)}{2}) \\
        = n^2 + 3n -3
    \]

    Assim obtemos uma estimativa mais precisa.
    Quando comparamos $n^2 + 3n - 3$ com $n^2$

    $n$ | $n^2 -3n - 3$| $n^2$
    --- | --- | ---|
    1 | 1 | 1
    2| 7 | 4
    3 | 15 | 9
    10 | 127 | 100
    100 | 10297 | 10000
    1000 | 1002997| 1000000

    Quando $n$ cresce $n^2$ domina os outros termos da função.

    Se chamarmos de $t_i$ o tempo consumido na execução da linha i:
    \[
    \begin{array}{lr}
        0 \rightarrow t_0 \rightarrow 1 \ \text{vez} \\
        1 \rightarrow t_1 \rightarrow n \\
        2 \rightarrow t_2 \rightarrow n-1 \\
        3 \rightarrow t_3 \rightarrow n-1 \\
        4 \rightarrow t_4 \leq 2 + 3 + 4 + \dots + n \\
        5 \rightarrow t_5 \leq 1 + 2 + 3 + \dots + n-1 \\
        6 \rightarrow t_6 \leq 1 + 2 + 3 + \dots + n-1 \\
        7 \rightarrow t_7 \rightarrow n-1 \\
        8 \rightarrow t_8 \rightarrow n-1 \\
    \end{array}
    \]

    Ou seja, o consumo de tempo é limitado por
    \[
        \leq t_0 + n.t_1 + (n-1).(t_2+ t_3+t_7+t_8) +
        \frac{(n-1).(n+2)}{2}.t_4 + \frac{n.(n-1)}{2}.(t_5 + t_6) \\
        = \frac{t_4+t_5+t_6}{2}.n^2 + (t_1 + t_2+t_3+t_7+t_8 + \frac{t_4}{2}).n + t_0- t_1-t_2-t_3-t_7-t_8-t_4 \\
        = c_1.n^2 + c_2.n + c3
    \]  

    Onde $c_1, c_2, c_3$ são constantes que dependem da máquina.
    Note que o que importa é que o tempo varia proporcionalmente a $n^2$ e é essa a ideia da **notação $\mathcal{O}()$.**

    _Intuitivamente_
    \[
        \mathcal{O}(f(n)) \approx \left.\begin{array}{lr}
        \text{conjunto de funções cujo crescimento} \\ \text{é limitado pelo
         crescimento de f(n)} \\
         \text{multiplicado por uma constante}
         \end{array}
         \right.
    \]

    Assim, $n^2, \frac{3}{2}.n^2, 1000.n^2, \frac{n^2}{1000}$ crescem na mesma velocidade.

    \[
        n^2 + 99.n \quad é \quad \mathcal{O}(n^2) \\
        33.n^2 \quad é \quad \mathcal{O}(n^2) \\
        75.n - 3 \quad é \quad \mathcal{O}(n^2) \\
    \]

    Mas, $\frac{n^3}{10000} + 7.n^2 + 15$ não é $\mathcal{O}(n^2)$.

    _Formalmente_

    Dizemos que uma função $g(n)$ é $\mathcal{O}(f(n))$ se existem constantes positivas $c$ e $n_0$ tais que

    \[
        g(n) \leq c.f(n) \quad \quad n \geq n_0
    \]

    Escrevemos $g(n)$ é $\mathcal{O}(f(n))$ ou $g(n) = \mathcal{O}(f(n))$.

    Exemplos:

    * $10.n^2 e \mathcal{O}(n^2)$
    \[  
        10.n^2 \leq c.n^2 \quad \quad \forall n \geq n_0 \\
        \text{Tome } c = 10, n_0 = 1
    \]

    * $20.n^3 + 10.n.log_n + 5$ é $\mathcal{O}(n^3)$
    \[
        \leq 20.n^3 + 10.n^2 + n^2 \\
        \leq 20.n^3 + 11.n^2 \\
        \leq 31.n^3 \\
        \text{Tome } c = 31, n_0 = 5
    \]

    Podemos dizer:
    \[
        f(n) \in \mathcal{O}(g(n)) \\
        f(n) \text{ é } \mathcal{O}(g(n)) \\
        f(n) = \mathcal{O}(g(n))
    \]

---

# Aula 10/3

**Continuação**
**Notação $\Omega$:**

Dizemos que uma função $g(n)$ é $\Omega(f(n))$ se existem constantes positivas $c$ e $n_0$ tais que
\[
    c.f(n) \leq g(n) \quad \forall n \geq n_0
\]

Exemplo:
\[
    g(n) \geq \frac{n^2}{1000},  n \geq 0 \\
    g(n) = \Omega (n^2)
\]

**Melhor caso:**

Linha |Execução | Consumo de tempo
--- | --- | ---
0 | $1$ | $t_0$
1 | $n$ | $n.t_1$
2 | $n-1$ | $(n-1).t_2$
3 | $n-1$ | $(n-1)t_3$
4 | $\geq n-1$ | $\geq (n-1).t_4$
5 | $0$ |
6 | $0$ |
7 | $n-1$ | $(n-1).t_7$
8 | $n-1$ | $(n-1).t_8$

\[
    (t_1+t_2+t_3+t_4+t_7+t_8).n + t_0 -t_2-t_3-t_4-t_7-t_8 \\
    \geq c_1'.n +c_2' \Rightarrow \Omega(n)
\]

A função ordena por inserção, consome pelo menos $\Omega(n)$ unidades de tempo e no máximo $\mathcal{O}(n^2)$.

**Notação $\theta()$:**

Sejam $f(n)$ e $g(n)$ funções dos inteiros nos reais. Dizemos que $f(n) = \theta(g(n))$ se
\[
    f(n) \text{ é } \mathcal{O}(g(n)) e f(n) = \Omega(g(n))
\]
ou existem $c1, c2, n0$ tais que
\[
    c_1.g(n)\leq f(n) \leq c_2(g(n)) \quad \forall n\geq n_0
\]

Lembra que quando dizemos que um algoritmo $\mathcal{A}$ é $\theta(n)$ e outro $\mathcal{B}$ é $\theta(n.log \ n)$, estamos dizendo que $\mathcal{A}$ é assintoticamente ("n grandes") mais eficiente que $\mathcal{B}$. Lembre que há constantes escondidas!

Exemplo:

$\mathcal{A}$ consome $100.n$ unidades de tempo $\rightarrow \theta(n)$

$\mathcal{B}$ consome $n.log_{10} \ n$ unidades de tempo $\rightarrow \theta(n.log \ n)$

O algoritmo $\mathcal{A}$ só executa menos unidades de tempo que $\mathcal{B}$ para $n > 10^{100}$ (1 googol)

Desde o big Bang se passaram $4.10^17s$. O número de átomos do universo é estimado entre $4.10^{78}$ e $6.10^{79}$.

Em geral, um algoritmo $\theta(n)$ ou $\theta(n.log \ n)$ com constantes razoáveirs é eficiente.

* eficiente = polinomial
* $\theta(n^2)$ pode ser satisfatório.
* $\theta(2^n)$ dificilmente é aceitável.

\[
    \begin{array}{lr}
        \mathcal{O}(n) : \text{ linear} \\
        \mathcal{O}(n^2) : \text{ quadrático} \\
        \mathcal{O}(n^3) : \text{ cúbico} \\
        \mathcal{O}(2^n) : \text{ eponencial} \\
    \end{array}
\]

**Análise do caso médio:**

SUpomos que a entrada é uma permutação de 1 a n escolhida com probabilidade uniforme.

Qual o número esperado de execuções das linhas 4-6?

Para cada $j$ o número de t de vezes que a linha 4 executa é $1\leq t \leq j$

Para um $x$ na posição $j$ executamos o loop:

* 1 vez se $x > A[j-i]$ (maior de $A[1\dots j]$)
* 2 vezes se $x$ for o 2o maior

$\quad \ \ \vdots$

* j vezes se $x$ for o menor

Como a chance de cada situação é igula, tempos uma distribuição uniforme de 1 até j vezes que executaremos o loop. Portanto a probabilidade de executar t vezes  é $\frac{1}{j}$.

Assim o número esperado de execuções das linhas 4(5, 6) é:

\[
    = 1.\frac{1}{j} + 2$\frac{1}{j}$ + \dots +j.\frac{1}{j} \\
    = \frac{1}{j} (1 + 2 + \dots +j) \\
    = \frac{1}{j} (1 + 2 + \dots +j) \\
    = \frac{1}{j} (j.\frac{(1+j)}{2})
    = \frac{1}{j} (j.\frac{(1+j)}{2}) \\
    = \frac{(1+j)}{2}
\]

Com isso, o número esperado de execuções das linhas 4-6 em todas as iterações $j = 2, 3,4 \dots, n$
\[
    \sum^n_{j=2} \frac{(1+j)}{2} = \frac{1}{2}. (3 + 4 \dots + n+1) \\
    = \frac{(n-1).(n+4)}{4}
\]

Ordenação por inserção tem um consumo de tempo no caso médio dado por
\[
    t_0 + (t_1+t_2+t_3+t_4+t_7+t_8).n - (t_2+t_3+t_4+t_7+t_8) + (t_4+t_5+t_6).\frac{(n-1).(n+4)}{4}
\]

Portanto no caso médio a ordenação por inserção é $\theta(n^2)$.

### Divisão e conquista
    CLRS 2.3, 4.1, 4.2

É um paradgma de projeto de alforitmos que envolve os seguintes passos:

1. **Divide** o problema em subproblemas
2. **Conquista** resolve recursivamente os subproblemas
3. **Combina** junta as soluções dos subproblemas para gerar a solução da instância original.

Vamos exemplificar o uso da técnica com o algoritmo **Mergesort**.

1. **Divide:** Divide a sequência de n elementos em duas susquências de aproximadamente $\frac{n}{2}$.
2. **Conquista:** ordena recursivamente as subsequências
3. **Combina:** intercala as subsequências ordenadas obtendo a sequência original ordenada.

##### _Mergesort (A, p, r)_

Rearranja os elementos de $A[p \dots r]$ para ordenação

```
    1. se p < r
    2.    q <- floor((p+r) / 2)
    3.    Mergesort (A, p , q)
    4.    Mergesort (A, q+1, r)
    5.    Intercala (A, p , q, r)
```

A função Intercala receve o vetor A e três índices $p \leq q \leq r$ tais que $A[p\dots q]$ e $A[q+1 \dots r]$ estão ordenados e rearraja os elementos de A de forma que $A[p \dots r]$ fique ordenado.

Exemplo:

Dado

12 | 25| 33 | 67 | 98 | llll| 20 | 41 | 43 | 95 | 102 | 110
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---| ---| ---|

Saída

12 | 20| 25 | 33 | 41 | 43 | 67| 95 | 98 | 102 | 110
--- | --- | --- | --- | --- | --- | --- | --- | --- | ---| ---| ---|

Para intercalar pegamos um ponteiro pro começo de cada subsequência e analisamos os número de 2 em 2 e ordenamos, precisa se pensar no caso de quando uma das subsequências ter sido percorrida por copleta.

##### Intercala (A, p, q, r)
```
    0. B[p ... r] é um vetor auxiliar
    1. para i <- p até q faça
    2.    B[i] <- A[i]
    3. para j <- q+1 até r faça
    4.    B[j] <- A[r-j+q+1]
    5. i <- p
    6. j <- r
    7. para k <- p até r faça
    8.    se B[i] <= B[j] e i <= q
    9.        A[k] <- B[i]
    10.       i <- i+1
    11.   senão
    12.       A[k] <- B[j]
    10.       j <- j-1
```

---

# Aula 15/03

Análise do consumo de tempo do Intercala. Se $n = r - p + 1$

* Linhas 1-4 consumo $\theta(n)$
* Linhas 5-6 conusmo $\theta(1)$
* Linhas 7-13 consumo $\theta(n)$

Portanto o consumo de tempo do Intercala é: $\theta(n)$.

Análise do consumo de tempo do Mergesort. Se chamamos de $T(n)$ o tempo consumido pelo algoritmo para uma instância de tamanho $n$.

* Linhas 1-2 consumo $\theta(1)$
* Linhas 3 conusmo $T(\lfloor{\frac{n}{2}}\rfloor)$
* Linhas 4 conusmo $T(\lceil{\frac{n}{2}}\rceil)$
* Linhas 5 consumo $\theta(n)$

\[
    T(n) = T(\lfloor{\frac{n}{2}}\rfloor) + T(\lceil{\frac{n}{2}}\rceil) + \theta (n)
\]

_Como calculat $T(n)$?_

Um método para calcular esse tipo de recorrência é o método da expansão:

1. Restrinja-se a uma potência de 2
2. Troque a notação $\theta(n)$ por $c_1.n$
3. Use uma constante $c_2$ para $T(1)$
1. Use expansão para encontrar "um chute"
1. Verifique se sem chute está certo.

Quando n é potencia de 2, $n = 2^k$ e $k = log n$
\[
    T(1) = c_2 \\
    T(n) = 2.T(\frac{n}{2})+ c_1.n, \quad n > 1 \\
    \begin{array}{lr}
    \quad \quad \quad \quad = 2.\left[2.T(\frac{n}{4}) + c_1.\frac{n}{2}\frac{n}{2}\right] + c_1.n \\
    \quad \quad \quad \quad = 4.T(\frac{n}{4}) + 2.c_1.n \\
    \quad \quad \quad \quad \vdots \\
    \quad \quad \quad \quad = 2^k.T(\frac{n}{2^k}) + k.c_1.n \\
    \quad \quad \quad \quad = c_2.n + c1.n.log \ n \\
    \quad \quad \quad \quad = \theta (n.log \ n)
    \end{array}
\]

Vamos conferir:

Para $n = 2^k \begin{cases} T(1) = c_2 \\ T(n) = 2.T(\frac{n}{2}) + c_1.n, & n > 1\end{cases}$

Vale $T(n) = c_2.n + c1.n.log \ n$.

**Prova por indução em k**

_Base_: $k = 0, n= 1, T(1) = c_2 = c_2.1 + c_1.1.0$

_Passo_: Tome $k \geq 1$

\[
    HI: T(2^{k-1}) = c_2.2^{k-1} + c_1.2^{k-1}.(k-1)
\]
Sabemos que

\[
    T(2^k) = 2.T(\frac{2^k}{2}) + c_1.2^k \\
    = 2.T(2^{k-1}) + c_1.2^k \\
    \overset{HI}{=} 2.\left(c_2.2^{k-1} + c_1.2^{k-1}.(k-1)\right) + c_1.2^k \\
    = 2.c_2.2^{k-1} + 2.c_1.2^{k-1}.(k-1) + c_1.2^k \\
    = c_2.2^k + c_1.(k-1).2^k + c_1.2^k \\
    = c_2.2^k + c1.k.2^k \\
    = c_2.n + c_1.n.log \ n_\blacksquare
\]

**Exercícios:**

1.
$T (n) =  \begin{cases}
        c_1 &  n = 1 \\
            T(\frac{n}{2}) + c_2  & n > 1\end{cases}$
\[
    T(1) = c_1 \\
    n = 2^k
\]

Expandindo
\[
    T(n) = T(\frac{n}{2}) + c_2 \\
    = \left( T(\frac{n}{4}) + c_2 \right) + c_2 \\
    = T(\frac{n}{4}) + 2.c_2 \\
    \vdots \\
    = T(\frac{n}{2^k}) + k.c_2 = T(1) + k.c_2 \\
    = c_1 + k.c_2 \Rightarrow \theta(1)
\]

Prova por indução

Base: $n= 1, T(1)=c_1= c_1 + 0.c_2$
Passo:
\[
    HI: T(2^{k-1}) = c_1 + (k-1).c_2 \\
    \\
    T(2^k) = T(\frac{2^k}{2}) + c_2 = T(2^{k-1}) + c_2 \\
    (c_1 + (k-1).c_2) + c_2 = c_1 + k.c_2
\]

2. $T(n) = T(n-1) + \theta (n)$

\[
    T(1) = c_1 \\
    \theta(n) = c_2.n
\]

Então, expandindo

\[
    T(n) = T(n-1) + c_2.n \\
    = \left(T(n-2) + c_2.(n-1)\right) + c_2.n \\
    = T(n-2) + c_2.n + c_2.(n-1)\\
    = (T(n-3) + c_2.(n-2)) + + c_2.n + c_2.(n-1)
    \vdots \\
    = T(1) + 2.c_2 + 3.c_2 + \dots + (n-1).c_2 + n.c_2 \\
    = c_1 + c_2 (2 + 3 + \dots + n) = c_1 + c_2.\left(\frac{(n-1).(n+1)}{2}\right)
\]\

Prova por indução

Base: $n= 1, T(1) = c_1 = c_1 + c_2.(\frac{0.3}{2})$

Passo :
\[
    HI: T(n-1) = c_1 + c_2.\left(\frac{(n-2).(n+1)}{2}\right)
    \\
    T (n) = T(n-1) + c_2.n \\
    = c_1 + c2.\left(\frac{(n-1).(n-2)}{2} + n\right) \\
    = c_1 + c_2.\left(\frac{n^2 -n -2 + 2n}{2}\right)\\
    = c_1 + c_2.\left(\frac{n^2 + n -2}{2}\right)
    = c_1 + c_2.\left(\frac{(n-1)(n+2)}{2}\right)_ \blacksquare
\]

Portanto essa recorrência descreve um algorimo $\theta(n^2)$

3. $T(n) = 3.T (\lfloor\frac{n}{2}\rfloor) + \theta (n)$

\[
    n = 2^k \\
    T(1) = c_1 \\
    \theta(n) = c_2.n
\]
Então, expandindo
\[
    T(n) = 3.T(\frac{n}{2}) + c_2.n \\
    = 3.(3.T(\frac{n}{4}) + c_2.\frac{n}{2})) + c_2.n \\
    = 3^2.T(\frac{n}{4}) + 2.c_2.\frac{n}{2}) + c_2.n \\
    \vdots \\
    = 3^k.T(\frac{n}{2^k}) + \frac{3^{k-1}}{2^{k-1}}.c_2.n + \dots + \frac{3}{2}.c_2.n + c_2.n \\
    3^k.c_1 +  c_2n.\left(\frac{3^{k-1}}{2^{k-1}}\ + \frac{3^{k-2}}{2^{k-2}} +  \dots + \frac{3}{2} + 1\right) \\
    = 3^k.c_1  +  c_2.n.\left(\frac{\left(\frac{3^{k-1}}{2^{k-1}}\right) - 1}{\frac{3}{2} - 1}\right) \\
    = 3^{log \ n}.c_1 + 2.c_2.(3^{log \ n} - n) \\
\]
Como $3 = 2^{log \ 3}$
\[
    = c_1.\left(2^{log \ 3 }\right)^{log \ n} + 2.c_2.\left(\left(2^{log \ 3 }\right)^{log \ n} - n\right) \\
    = c_1.\left(2^{log \ n }\right)^{log \ 3} + 2.c_2.\left(\left(2^{log \ n }\right)^{log \ 3} - n\right) \\
    = c_1.n^{log \ 3} + 2.c_2(n^{log \ 3} - n)
\]
Portanto, $T(n) = \theta (n^{log \ 3})$

Prova por indução:

Base: $n =1, T(n) = c1.1 + 2.c_2 (1 - 1) = c_1$

Passo:

\[
    HI: T(2^{k-1}) = c_1.(2^{k-1})^{log \ 3} + 2.c_2. ((2^{k-1})^{log \ 3} - 2^{k-1}) \\
    T(2^k) = 3.T(2^{k-1}) + c_2.2^k \\
    = 3.c_1.(2^{k-1})^{log \ 3} + 3.2.c_2. ((2^{k-1})^{log \ 3} - 2^{k-1}) + c_2.2^k \\
    = 3.c_1.(2^{k-1})^{log \ 3} + 3.2.c_2. (2^{k-1})^{log \ 3} -  3.2.c_2.2^{k-1} + c_2.2^k \\
    = 3.c_1.(2^{k-1})^{log \ 3} + 3.2.c_2. (2^{k-1})^{log \ 3} -  3.c_2.2^{k} +  c_2.2^k  \\
    = 2^{log \ 3}.c_1.(2^{k-1})^{log \ 3} + 2^{log \ 3}.2.c_2.(2^{k-1})^{log \ 3} - 2.c_2.2^k \\
    = c_1.(2^{k})^{log \ 3} + 2.c_2.((2^{k})^{log \ 3} - 2^k)_ \blacksquare
\]
