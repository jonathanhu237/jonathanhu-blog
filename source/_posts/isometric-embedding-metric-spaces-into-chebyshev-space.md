---
title: Isometric Embedding Metric Spaces into Chebyshev Space
categories:
  - Math
tags:
  - Embedding
  - Norm
date: 2024-11-24 21:18:06
---


An embedding is a mapping that preserves the structure of metric space when transformed into another. By leveraging embeddings, we can reduce complex problem to spaces where more efficient solutions are possible. In this post, we firstl introduce the concepts of metric space and embedding. Then, we demonstract two isometric embeddings: the first maps a finite $\ell_1$ space to an $\ell_\infty$ space, and the second maps any finite metric space to an $\ell_\infty$ space.

<!-- more -->

## Preliminaries

### Metric Space

Let $\mathcal{M}$ be a set of points, and let $d_{\mathcal{M}}$ be a metric which is a function that defines the distance between any two point in $\mathcal{M}$. The pair $(\mathcal{M},d_{\mathcal{M}})$ is called a metric space if $d_{\mathcal{M}}$ satisfies the following properties for all $\boldsymbol{x},\boldsymbol{y},\boldsymbol{z}\in\mathcal{M}$:

- Non-negativity: $d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})\ge 0$, and $d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})=0$ if and only if $\boldsymbol{x}=\boldsymbol{y}$.
- Symmetry: $d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})=d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{x})$.
- Triangle Inequality: $d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{z})\le d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y}) + d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{z})$.

If the set $\mathcal{M}$ contains a finite number of points, the metric space $(\mathcal{M},d_{\mathcal{M}})$ is referred to as a finite metric space.

In most cases, the metric spaces we consider have $\mathcal{M}\subset\mathbb{R}^d$, where $d\in\mathbb{N}$, and a specified distance function $d_\mathcal{M}$. For instance, the $\ell_p$-distance for $p\ge 1$ define a metric space $(\mathcal{M},d_{\mathcal{M}})$ , where $\mathcal{M}=\mathbb{R}^d$ and the distance function is:
$$
d_\mathcal{M}(\boldsymbol{x},\boldsymbol{y})=\left(\sum_{i=1}^d(x_i-y_i)^p\right)^{\frac{1}{p}}
$$

For convenience, we will use the notation $\mathcal{L}_{p}^{n,d}$ to represent the metric space $(\mathcal{M},\ell_p)$, where $\mathcal{M}\subset\mathbb{R}^d$.

### Embedding

Let $(\mathcal{M},d_{\mathcal{M}})$ and $(\mathcal{M'},d_{\mathcal{M'}})$ be two metric spaces. A map $\boldsymbol{f}:\mathcal{M}\to\mathcal{M'}$ is called an embedding. The embedding is said to be isometric if, for all $\boldsymbol{x},\boldsymbol{y}\in\mathcal{M}$, the following hods:
$$
d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})=d_{\mathcal{M'}}(\boldsymbol{f}(\boldsymbol{x}),\boldsymbol{f}(\boldsymbol{y}))
$$
The contraction of $\boldsymbol{f}$ is defined as the maximum factor by which the distances shrink under the mapping, defined as:
$$
\max_{x,y\in\mathcal{M}}\frac{d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})}{d_{\mathcal{M'}}(\boldsymbol{f}(\boldsymbol{x}),\boldsymbol{f}(\boldsymbol{y}))}
$$
Similarly, the expansion of $\boldsymbol{f}$ is the maximum factor by which distances are stretched, defined as:
$$
\max_{x,y\in\mathcal{M}}\frac{d_{\mathcal{M'}}(\boldsymbol{f}(\boldsymbol{x}),\boldsymbol{f}(\boldsymbol{y}))}{d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})}
$$

## Isometric Embedding from $\mathcal{L}_{1}^{n,d}$ to $\mathcal{L}_{\infty}^{n,2^d}$

$\ell_1$-distance between two points $\boldsymbol{x}=(x_1,x_2,\cdots,x_d)$ and $\boldsymbol{y}=(y_1,y_2,\cdots,y_d)$ can be computed in $O(d)$ time. Given a finite set $\mathcal{M}$ with cardinality $n$, there are $\binom{n}{2}$ pairs of points. Computing the distance for all pairs in $\mathcal{M}$ has a time complexity of $O(dn^2)$.

If we need to find the further $\ell_1$-distance between points in $\mathcal{M}$, a straightforward method is to compute the distance for all pairs, which results in a complexity of $O(dn^2)$. To reduce this computational cost, we introduce an isometric embedding from $\mathcal{L}_{p}^{n,d}$ to $\mathcal{L}_{p}^{n,2^d}$. Using this embedding, the furthest distances can be calculated with a time complexity $O(2^dn)$, which is significantly more efficient when $n\gg 2^d$.

### Construction of the Embedding

Given a $d$-dimensional vector $\boldsymbol{a}$ where each entry is either $1$ or $-1$, the dot product $\boldsymbol{a}\cdot\boldsymbol{x}$ yields the projection of $\boldsymbol{x}$ onto the direction defined by $\boldsymbol{a}$.Since there are $2^d$ possible vector $\boldsymbol{a}$, we obtain $2^d$ projections for any vector $\boldsymbol{x}$.

Define $\boldsymbol{f}:\mathbb{R}^d\to\mathbb{R}^{2^d}$ as the mapping that send $\boldsymbol{x}$ to be the concatenation of these $2^d$ projections of $\boldsymbol{x}$ (the order of concatenation does not matter). We will show that for all $\boldsymbol{x},\boldsymbol{y}\in\mathcal{X}$, the following holds:
$$
\Vert \boldsymbol{f}(\boldsymbol{x})-\boldsymbol{f}(\boldsymbol{y})\Vert_\infty=\Vert\boldsymbol{x}-\boldsymbol{y}\Vert_1
$$

### Proof

We start by computing $\Vert \boldsymbol{f}(\boldsymbol{x})-\boldsymbol{f}(\boldsymbol{y})\Vert_\infty$:
$$
\begin{align}
\Vert \boldsymbol{f}(\boldsymbol{x})-\boldsymbol{f}(\boldsymbol{y})\Vert_\infty&=\max_{\boldsymbol{a}}\vert\boldsymbol{a}\cdot\boldsymbol{x}-\boldsymbol{a}\cdot\boldsymbol{y}\vert \\
&=\max_{\boldsymbol{a}}\vert\boldsymbol{a}\cdot(\boldsymbol{x}-\boldsymbol{y})\vert \\
&=\max_{\boldsymbol{a}}\left|\sum_{i=1}^d a_i\cdot(x_i-y_i)\right|
\end{align}
$$
To maximize this expression, we choose $a_i=\text{sgn}(x_i-y_i)$, which ensures that:
$$
\max_{\boldsymbol{a}}\left|\sum_{i=1}^d a_i\cdot(x_i-y_i)\right|=\sum_{i=1}^d\vert x_i-y_i\vert
$$
Therefore:
$$
\Vert \boldsymbol{f}(\boldsymbol{x})-\boldsymbol{f}(\boldsymbol{y})\Vert_\infty=\sum_{i=1}^d\vert x_i-y_i\vert=\Vert\boldsymbol{x}-\boldsymbol{y}\Vert_1
$$

### Time Complexity

Observe that:
$$
\max_{\boldsymbol{x},\boldsymbol{y}\in\mathcal{M}}\Vert \boldsymbol{f}(\boldsymbol{x})-\boldsymbol{f}(\boldsymbol{y})\Vert_\infty=\max_{\boldsymbol{x},\boldsymbol{y}\in\mathcal{M}}\max_{i=1}^{2^d}\left|f_i(\boldsymbol{x})-f_i(\boldsymbol{y})\right|=\max_{i=1}^{2^d}\max_{\boldsymbol{x},\boldsymbol{y}\in\mathcal{M}}\left|f_i(\boldsymbol{x})-f_i(\boldsymbol{y})\right|
$$
The term $\max_{\boldsymbol{x},\boldsymbol{y}\in\mathcal{M}}\left|f_i(\boldsymbol{x})-f_i(\boldsymbol{y})\right|$ can be computed in $O(n)$ by first finding $\max_{\boldsymbol{x}\in\mathcal{X}}f_i(\boldsymbol{x})$ and $\min_{\boldsymbol{x}\in\mathcal{X}}f_i(\boldsymbol{x})$, then taking the absolute value of their difference. The target value is obtained by repeating this process for each $i$ from $1$ to $2^d$. Thus, the total time complexity  is $O(n2^d)$.

## Isometric Embedding from Any Finite Metric Space to $\mathcal{L}_{\infty}^{n,n}$

We will demonstrate that any finite metric space $(\mathcal{M},d_{\mathcal{M}})$ can be isometrically embedded into $\mathcal{L}_{\infty}^{n,n}$. This result establishes that the $\ell_\infty$-metric space is universal, meaning any metric space can be embedded isometrically in an $\ell_\infty$-metric space of suitable dimension.

### Construction of the Embedding

Let $\mathcal{M}=\{\boldsymbol{x_1},\boldsymbol{x_2},\cdots,\boldsymbol{x_n}\}$, For each point $\boldsymbol{x}\in\mathcal{M}$, define the mapping:
$$
\boldsymbol{f}(\boldsymbol{x})=(d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x_1}),d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x_2}),\cdots,d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x_n}))
$$
We will show that the mapping is an isometric embedding, i.e., for any pair of points $\boldsymbol{x_i},\boldsymbol{x_j}\in\mathcal{X}$, the followings holds:
$$
d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_j})=\Vert \boldsymbol{f}(\boldsymbol{x_i})-\boldsymbol{f}(\boldsymbol{x_j})\Vert_\infty
$$

### Proof

We start by computing $\Vert \boldsymbol{f}(\boldsymbol{x_i})-\boldsymbol{f}(\boldsymbol{x_j})\Vert_\infty$:
$$
\Vert \boldsymbol{f}(\boldsymbol{x_i})-\boldsymbol{f}(\boldsymbol{x_j})\Vert_\infty=\max_{t=1}^n\left|f_t(\boldsymbol{x_i})-f_t(\boldsymbol{x_j})\right|
$$
By the definition of the mapping, we have:
$$
\max_{t=1}^n\left|f_t(\boldsymbol{x_i})-f_t(\boldsymbol{x_j})\right|=\max_{\boldsymbol{x_k}\in\mathcal{M}}\left|d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_k})-d_{\mathcal{M}}(\boldsymbol{x_j},\boldsymbol{x_k})\right|
$$
Using the triangle inequality, we know:
$$
\max_{\boldsymbol{x_k}\in\mathcal{M}}\left|d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_k})-d_{\mathcal{M}}(\boldsymbol{x_j},\boldsymbol{x_k})\right|\le d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_j})
$$
On the other hand, we have:
$$
\max_{\boldsymbol{x_k}\in\mathcal{M}}\left|d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_k})-d_{\mathcal{M}}(\boldsymbol{x_j},\boldsymbol{x_k})\right|\ge\left| d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_i})-d_{\mathcal{M}}(\boldsymbol{x_j},\boldsymbol{x_i})\right|=d_{\mathcal{M}}(\boldsymbol{x_j},\boldsymbol{x_i})=d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_j})
$$
Thus, we conclude:
$$
\max_{\boldsymbol{x_k}\in\mathcal{M}}\left|d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_k})-d_{\mathcal{M}}(\boldsymbol{x_j},\boldsymbol{x_k})\right|=d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_j})
$$
Therefore:
$$
\Vert \boldsymbol{f}(\boldsymbol{x_i})-\boldsymbol{f}(\boldsymbol{x_j})\Vert_\infty=d_{\mathcal{M}}(\boldsymbol{x_i},\boldsymbol{x_j})
$$
