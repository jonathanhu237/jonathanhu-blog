---
title: Johnson Lindenstrauss Lemma
categories:
    - Math
tags:
    - Embedding
    - Norm
---

The Johnson-Lindenstrauss (JL) Lemma is a important result in mathematics that provides a way to reduce the dimensionality of data points while preserving the pairwise distances between them, up to a certain distortion. In this blog post, we introduce the JL Lemma and present a proof of the theorem. 

The proof is based on the work of Dasgupta et al.'s paper *An elementary proof of a theorem of Johnson and Lindenstrauss.*. For a more detailed explanation, please refer to the original paper.

<!-- more -->

## JL Lemma

For any set $\mathcal{P}$ of $n$ points in $\mathbb{R}^d$, there exists a map $f:\mathbb{R}^{d}\to\mathbb{R}^k$, such that for all $\boldsymbol{p_i},\boldsymbol{p_j}\in\mathcal{P}$:

$$
(1-\epsilon)\Vert\boldsymbol{p_i}-\boldsymbol{p_j}\Vert_2^2\le\Vert\boldsymbol{f(p_i)-f(p_j)}\Vert_2^2\le(1+\epsilon)\Vert\boldsymbol{p_i}-\boldsymbol{p_j}\Vert_2^2
$$

where $0\lt\epsilon\lt 1$ and $k$ is a positive integer satisfying:

$$
k\gt\frac{24}{3\epsilon^2-2\epsilon^3}\log{n}
$$

Let $A$ be a random matrix of size $k\times d$, where each entry is independently drawn from the standard normal distribution $\mathcal{N}(0,1)$. Using this random matrix, we can construct a mapping that satisfies the JL Lemma. For any point $\boldsymbol{p}\in\mathcal{P}$, the mapping is defined as:

$$
\boldsymbol{f(p)}=\frac{A}{\sqrt{k}}\cdot\boldsymbol{p}
$$

## Proof of JL Lemma

To prove the JL Lemma, we first establish some auxiliary lemmas and inequalities. For convenience, we will omit the subscript $2$ in the norm, so $\Vert\cdot\Vert^2$ refers to the $2$-norm by default.

### Lemma 1

For any point $\boldsymbol{p}\in\mathcal{P}$, let $\boldsymbol{Y}=\frac{A}{\sqrt{k}}\cdot\boldsymbol{p}$. Then, we have:

$$
\mathrm{E}[\Vert\boldsymbol{Y}\Vert^2]=\Vert\boldsymbol{p}\Vert^2
$$

#### Proof of Lemma 1

From the definition of $Y_i$, we have:

$$
Y_i=\frac{1}{\sqrt{k}}\sum_{j=1}^dA_{ij}p_j
$$

Now, we compute $\text{E}\left[\Vert\boldsymbol{Y}\Vert^2\right]$:

$$
\begin{align*}
\text{E}\left[\Vert\boldsymbol{Y}\Vert^2\right]&=\text{E}\left[\sum_{i=1}^{k}\frac{1}{k}\left(\sum_{j=1}^dA_{ij}p_j\right)^2\right] \\
&=\text{E}\left[\sum_{i=1}^{k}\frac{1}{k}\sum_{j=1}^{d}\sum_{t=1}^{d}A_{ij}A_{it}p_jp_t\right] \\
&=\sum_{i=1}^k\frac{1}{k}\sum_{j=1}^{d}\sum_{t=1}^{d}p_jp_t\text{E}\left[A_{ij}A_{it}\right]
\end{align*}
$$

- if $i\ne t$, then:
$$
\text{E}\left[A_{ij}A_{it}\right]=\text{E}\left[A_{ij}\right]\text{E}\left[A_{it}\right]=0
$$
- if $i=t$, then:
$$
\text{E}\left[A_{ij}A_{it}\right]=\text{E}\left[A_{ij}^{2}\right]=\text{Var}(A_{ij})+\text{E}^2\left[A_{ij}\right]=1
$$

Thus, we get:

$$
\text{E}\left[\Vert\boldsymbol{Y}\Vert^2\right]=\sum_{i=1}^{k}\frac{1}{k}\sum_{j=1}^{d}p_j^2=\sum_{i=1}^{k}\frac{1}{k}\Vert\boldsymbol{p}\Vert^2=\Vert\boldsymbol{p}\Vert^2
$$

### Lemma 2

Let $X$ be a random variable distributed as $\mathcal{N}(0,1)$. For a constant $\lambda\le\frac{1}{2}$, we have:
$$
\text{E}\left[e^{\lambda X^2}\right]=\frac{1}{\sqrt{1-2\lambda}}
$$

#### Proof of Lemma 2