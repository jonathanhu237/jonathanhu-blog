---
title: A Distortion Embedding From Any Metric Space to Chebyshev Space
categories:
    - Math
tags:
    - Embedding
    - Norm
---

In the previous post, we introduced the concepts of metric space and embedding, and we discussed two isometric embeddings into the Chebyshev $(\ell_\infty)$ space. However, in many practical scenarios, distortion embeddings are more effective, as they trade precision for computational efficiency.

In this post, we introduce a new embedding that maps points from any metric space into $\ell_\infty$-space with appropriate dimensions. Furthermore, we demonstrate that the contraction of this embedding does not exceed a given parameter $D$.

This embedding was proposed by Matousek in his paper *On the Distortion Required For Embedding Finite Metric Spaces into Normed Spaces*. For a deeper understanding, we encourage you to refer to the original paper.

<!-- more -->

## Theorem

Given a finite metric space $(\mathcal{M},d_{\mathcal{M}})$, an odd integer parameter $D$, and $n=|\mathcal{M}|$, there exists an embedding $f$ from $(\mathcal{M},d_{\mathcal{M}})$ into $\mathcal{L}_{\infty}^{n,O(qn^{1/q}\log{n})}$ such that, for any pair of points $\boldsymbol{x},\boldsymbol{y}\in\mathcal{M}$, the embedding satisfies:
$$
\frac{d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})}{D}\le\Vert \boldsymbol{f(x)}-\boldsymbol{f(y)}\Vert_{\infty}\le d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})
$$
where $q=\left\lceil\frac{D}{2}\right\rceil$. 

This result can be expressed concisely as:
$$
(\mathcal{M},d_{\mathcal{M}})\overset{D}{\hookrightarrow}\mathcal{L}_{\infty}^{n,O(qn^{1/q}\log{n})}
$$
Next, we introduce the construction of this embedding.

## Construction of the Embedding

The core of the embedding involves constructing several subsets of $\mathcal{M}$. Given an auxiliary parameter $p=n^{-1/q}$, for $j=1,2,\cdots,q$, each element in $\mathcal{M}$ is chosen to be part of a subset with probability $p_j=\min\left(\frac{1}{2},p^j\right)$. Repeating the process $m=\left\lceil 24n^{1/q}\log{n}\right\rceil$ for each $j$, we can construct $mq$ subsets:
$$
\mathcal{M}_{11},\cdots,\mathcal{M}_{m1},\mathcal{M}_{12},\cdots,\mathcal{M}_{m2},\cdots,\mathcal{M}_{1q},\cdots,\mathcal{M}_{mq}
$$
For a point $\boldsymbol{x}$ and a subset $\mathcal{M}_{ij}$ of $\mathcal{M}$, the distance between $\boldsymbol{x}$ and $\mathcal{M}_{ij}$ is defined as:
$$
d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{ij})=\min_{\boldsymbol{x'}\in\mathcal{M}_{ij}}d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x'})
$$
The mapping $\boldsymbol{f(x)}$ is then given by:
$$
\left(d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{11}),\cdots,d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{m1}),d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{12}),\cdots,d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{m2}),\cdots,d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{1q}),\cdots,d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{mq})\right)
$$

## Proof

To prove that the contraction of the embedding is at most $D$, we first need to establish an important lemma.

Let $\boldsymbol{x}$ and $\boldsymbol{y}$ be two distinct points of $\mathcal{M}$. Then there exists an index $j\in\{1,2,\cdots,q\}$ such that:
$$
\Pr\left[\left|d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{ij})-d_{\mathcal{M}}(\boldsymbol{y},\mathcal{M}_{ij})\right|\ge\frac{d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})}{D}\right]\ge\frac{p}{12}
$$
