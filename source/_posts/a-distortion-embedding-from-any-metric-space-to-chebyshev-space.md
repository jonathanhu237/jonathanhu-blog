---
title: A Distortion Embedding From Any Metric Space to Chebyshev Space
categories:
  - Math
tags:
  - Embedding
  - Norm
date: 2024-11-28 14:36:52
---


In the previous post, we introduced the concepts of metric space and embedding, and we discussed two isometric embeddings into the Chebyshev $(\ell_\infty)$ space. However, in many practical scenarios, distortion embeddings are more effective, as they trade precision for computational efficiency.

In this post, we introduce a new embedding that maps points from any metric space into $\ell_\infty$-space with appropriate dimensions. Furthermore, we demonstrate that the contraction of this embedding does not exceed a given parameter $D$.

This embedding was proposed by Matousek in his paper *On the Distortion Required For Embedding Finite Metric Spaces into Normed Spaces*. For a deeper understanding, we encourage you to refer to the original paper.

<!-- more -->

## Theorem

Given a finite metric space $(\mathcal{M},d_{\mathcal{M}})$, a parameter $D$, and $n=|\mathcal{M}|$, there exists an embedding $f$ from $(\mathcal{M},d_{\mathcal{M}})$ into $\mathcal{L}_{\infty}^{n,O\left(Dn^{\frac{2}{D}}\log{n}\right)}$ such that, for any pair of points $\boldsymbol{x},\boldsymbol{y}\in\mathcal{M}$, the embedding satisfies:
$$
\frac{d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})}{D}\le\Vert \boldsymbol{f(x)}-\boldsymbol{f(y)}\Vert_{\infty}\le d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})
$$

This result can be expressed concisely as:
$$
(\mathcal{M},d_{\mathcal{M}})\overset{D}{\hookrightarrow}\mathcal{L}_{\infty}^{n,O\left(Dn^{\frac{2}{D}}\log{n}\right)}
$$
Next, we introduce the construction of this embedding.

## Construction of the Embedding

The core of the embedding involves constructing several subsets of $\mathcal{M}$. Given an auxiliary parameter $p=\min\left(\frac{1}{2},n^{-\frac{2}{D}}\right)$, for $j=1,2,\cdots,\left\lceil\frac{D}{2}\right\rceil$, each element in $\mathcal{M}$ is chosen to be part of a subset with probability $p^j$. Repeating the process $m=\left\lceil 24n^{\frac{2}{D}}\log{n}\right\rceil$ for each $j$, we can construct $mq$ subsets:
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

### Important Lemma

Let $\boldsymbol{x}$ and $\boldsymbol{y}$ be two distinct points of $\mathcal{M}$. Then there exists an index $j\in\{1,2,\cdots,q\}$ such that:

$$
\Pr\left[\left|d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{ij})-d_{\mathcal{M}}(\boldsymbol{y},\mathcal{M}_{ij})\right|\ge\frac{d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})}{D}\right]\ge\frac{p}{12}
$$

Assuming the lemma holds, we proceed to prove the theorem.

### Proof of the Theorem Using the Lemma

Fix $j$, as guaranteed by the lemma. For a given pair of points $\boldsymbol{x}$ and $\boldsymbol{y}$, the probability that the distance is larger than $D$ for all $m$ trials is at most:

$$
\left(1-\frac{p}{12}\right)^{m}=\left(\left(1-\frac{p}{12}\right)^{\frac{12}{p}}\right)^{\frac{pm}{12}}\lt e^{-\frac{pm}{12}}
$$

Since $p=\min\left(\frac{1}{2},n^{-\frac{2}{D}}\right)$, $m=\left\lceil 24n^{\frac{2}{D}}\log{n}\right\rceil$:

- If $\frac{1}{2}\ge n^{-\frac{2}{D}}$, then:
$$
e^{-\frac{pm}{12}}\le e^{-\frac{n^{-\frac{2}{D}}\cdot 24n^{\frac{2}{D}}\log{n}}{12}}=e^{-2\log{n}}=\frac{1}{n^2}
$$

- If $\frac{1}{2}\lt n^{-\frac{2}{D}}$, then:
$$
e^{-\frac{pm}{12}}\le e^{-\frac{\frac{1}{2}\cdot 24n^{\frac{2}{D}}\log{n}}{12}}=e^{-n^{\frac{2}{D}}\log{n}}=\left(\frac{1}{n}\right)^{n^{\frac{2}{D}}} \lt\left(\frac{1}{n}\right)^{2}=\frac{1}{n^2}
$$

Therefore:

$$
\left(1-\frac{p}{12}\right)^{m}\lt e^{-\frac{pm}{12}}\le\frac{1}{n^2}
$$

Since there are $\binom{n}{2} \le n^2$ pairs of points in $\mathcal{M}$, by the Boole's inequality, the probability that the contraction of the distance exceeds $D$ in all trials for all points is at most:

$$
n^2\cdot \frac{1}{n^2}=1
$$

This implies that the following inequality holds for all pairs of point in $\mathcal{M}$ with nonzero probability:

$$
\frac{d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})}{D}\le\max_{i=1}^{m}\left|d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{ij})-d_{\mathcal{M}}(\boldsymbol{y},\mathcal{M}_{ij})\right|\le\Vert \boldsymbol{f(x)}-\boldsymbol{f(y)}\Vert_{\infty}
$$

To complete the proof, we need to show the reverse inequality:

$$
\Vert \boldsymbol{f(x)}-\boldsymbol{f(y)}\Vert_{\infty}\le d_\mathcal{M}(\boldsymbol{x},\boldsymbol{y})
$$

Given $\boldsymbol{x}$, $\boldsymbol{y}$ and subset $\mathcal{M}_{ij}$, let $\boldsymbol{x'}$ and $\boldsymbol{y'}$ be the closest points to $\boldsymbol{x}$ and $\boldsymbol{y}$ in $\mathcal{M}_{ij}$, respectively. Then:

$$
\left|d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{ij})-d_{\mathcal{M}}(\boldsymbol{y},\mathcal{M}_{ij})\right|=\left|d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x'})-d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{y'})\right|
$$

Assume that $d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x'})\gt d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{y'})$, then:

$$
\left|d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x'})-d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{y'})\right|=d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x'})-d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{y'})\le d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y'})-d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{y'})\le d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})
$$

Similarly, assume that $d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{y'})\gt d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x'})$, then:

$$
\left|d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x'})-d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{y'})\right|=d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{y'})-d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x'})\le d_{\mathcal{M}}(\boldsymbol{y},\boldsymbol{x'})-d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{x'})\le d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})
$$

In both cases, we have:

$$
\left|d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{ij})-d_{\mathcal{M}}(\boldsymbol{y},\mathcal{M}_{ij})\right|\le d_{\mathcal{M}}(\boldsymbol{x},\boldsymbol{y})
$$

Thus:

$$
\Vert \boldsymbol{f(x)}-\boldsymbol{f(y)}\Vert_{\infty}\le d_\mathcal{M}(\boldsymbol{x},\boldsymbol{y})
$$

which completes the proof.

### Proof of the Lemma

Define $B_{\mathcal{M}}(\boldsymbol{p},r)=\{\boldsymbol{p'}\mid d_{\mathcal{M}}(\boldsymbol{p},\boldsymbol{p'})\le r\}$. Let $\Delta=\frac{d_{\mathcal{M}(\boldsymbol{x},\boldsymbol{y})}}{D}$, and construct the following sequence:

- $B_0=\{\boldsymbol{x}\}$
- $B_1=B_\mathcal{M}(\boldsymbol{y},\Delta)$
- $B_2=B_\mathcal{M}(\boldsymbol{x},2\Delta)$
- $B_3=B_\mathcal{M}(\boldsymbol{y},3\Delta)$
- $\cdots$
- Continue this alternation, finishing with $B_{\left\lceil\frac{D}{2}\right\rceil}$.

#### Key Claim

There exists an index $t$, such that:

- $\left| B_t\right|\ge n^{\frac{2t}{D}}$
- $\left| B_{t+1}\right|\le n^{\frac{2(t+1)}{D}}$

This claim can be established by contradiction:

- Initially, $|B_0|=1$. If $|B_1|\le n^{\frac{2}{D}}$, the claim holds. Therefore, $|B_1|\gt n^{\frac{2}{D}}$.
- If $|B_1|\gt n^{\frac{2}{D}}$, then $|B_2|\gt n^{\frac{4}{D}}$, otherwise, the claim holds.
- Similarly, if $|B_2|\gt n^{\frac{4}{D}}$, then $|B_3|\gt n^{\frac{6}{D}}$, otherwise, the claim holds.

Continuing the process, for the final $B_{\left\lceil\frac{D}{2}\right\rceil}$, we find:

$$
\left|B_{\left\lceil\frac{D}{2}\right\rceil}\right|\gt n^{\frac{2\left\lceil\frac{D}{2}\right\rceil}{D}}\ge n
$$

which contradicts the fact that $\left|B_{\left\lceil\frac{D}{2}\right\rceil}\right|\le n$.

#### Establishing the Key Inequality

Fix $t$ as identified above. Suppose $\mathcal{M}_{ij}\cap B_t\ne\varnothing$ and $\mathcal{M}_{ij}\cap B_{t+1}=\varnothing$, then we have:

$$
\left|d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{ij})-d_{\mathcal{M}}(\boldsymbol{y},\mathcal{M}_{ij})\right|\ge\Delta
$$

To prove if, without loss of generality, assume:

- $B_t=B_{\mathcal{M}}(\boldsymbol{x},t\Delta)$
- $B_{t+1}=B_{\mathcal{M}}(\boldsymbol{y},(t+1)\Delta)$

Since $\mathcal{M}_{ij}\cap B_t\ne\varnothing$, it follows that:

$$
d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{ij}) \le t\Delta
$$

Moreover, because $\mathcal{M}_{ij}\cap B_{t+1}=\varnothing$, we have:

$$
d_{\mathcal{M}}(\boldsymbol{y},\mathcal{M}_{ij}) \gt (t+1)\Delta
$$

Thus:

$$
\left|d_{\mathcal{M}}(\boldsymbol{x},\mathcal{M}_{ij})-d_{\mathcal{M}}(\boldsymbol{y},\mathcal{M}_{ij})\right|\ge(t+1)\Delta-t\Delta=\Delta
$$

#### Calculating the Probability

For the probability:

$$
\Pr\left[\mathcal{M}_{ij}\cap B_t\ne\varnothing\wedge\mathcal{M}_{ij}\cap B_{t+1}=\varnothing\right]
$$

fix $t$ as identified earlier and let $j=t+1$.

The probability $\Pr\left[\mathcal{M}_{ij}\cap B_t\ne\varnothing\right]$ can be analyzed as follows:

$$
\begin{align*}
    \Pr\left[\mathcal{M}_{ij}\cap B_t\ne\varnothing\right]&=1-\Pr\left[\mathcal{M}_{ij}\cap B_t=\varnothing\right] \\
    &=1-(1-p^j)^{|B_t|} \\
    &\ge 1-(1-p^j)^{n^{\frac{2(j-1)}{D}}} \\
    &\gt 1-e^{-p^jn^{\frac{2(j-1)}{D}}}
\end{align*}
$$

Since $p\le n^{-\frac{2}{D}}$, then $p^{-1}\ge n^{\frac{2}{D}}$. Substituting this:

$$
\Pr\left[\mathcal{M}_{ij}\cap B_t\ne\varnothing\right]\gt 1-e^{-p^jn^{\frac{2(j-1)}{D}}}\ge 1-e^{-p^jp^{1-j}}=1-e^{-p}
$$

For $0\lt p\le\frac{1}{2}$, using simple calculus, it can be shown that:

$$
1-e^{-p}\ge\frac{p}{3}
$$

The probability $\Pr\left[\mathcal{M}_{ij}\cap B_{t+1}=\varnothing\right]$ can be analyzed as follows:

$$
\Pr\left[\mathcal{M}_{ij}\cap B_{t+1}=\varnothing\right]=(1-p^j)^{\left| B_{t+1}\right|}\ge (1-p^j)^{n^\frac{2j}{D}}\ge (1-p^j)^{\frac{1}{p^j}}
$$

For $p^j\le\frac{1}{2^j}\le\frac{1}{2}$, using calculus, it can be shown that:

$$
(1-p^j)^{\frac{1}{p^j}}\ge\frac{1}{4}
$$

Thus:

$$
\Pr\left[\mathcal{M}_{ij}\cap B_t\ne\varnothing\wedge\mathcal{M}_{ij}\cap B_{t+1}=\varnothing\right]\ge\frac{p}{12}
$$

## Corollary

Starting with the embedding:

$$
(\mathcal{M},d_{\mathcal{M}})\overset{D}{\hookrightarrow}\mathcal{L}_{\infty}^{n,O\left(Dn^{\frac{2}{D}}\log{n}\right)}
$$

Substituting $D=\Theta(\log{n})$:

$$
n^{\frac{2}{D}}=n^{\frac{2}{\Theta(\log{n})}}
$$

Simplify the logarithm:

$$
\log{n^{\frac{2}{\Theta(\log{n})}}}=\frac{2}{\Theta(\log{n})}\log{n}=\Theta(1)
$$

Exponentiate back to simplify:

$$
n^{\frac{2}{D}}=\Theta(1)
$$

Thus, the embedding becomes:

$$
(\mathcal{M},d_{\mathcal{M}})\overset{\Theta(\log{n})}{\hookrightarrow}\mathcal{L}_{\infty}^{n,O\left(\log^2{n}\right)}
$$