---
title: Johnson Lindenstrauss Lemma
categories:
  - Math
tags:
  - Embedding
  - Norm
date: 2024-11-29 17:34:11
---


The Johnson-Lindenstrauss (JL) Lemma is a important result in mathematics that provides a way to reduce the dimensionality of data points while preserving the pairwise distances between them, up to a certain distortion. In this blog post, we introduce the JL Lemma and present a proof of the theorem. 

The proof is based on the work of Dasgupta et al.'s paper *An elementary proof of a theorem of Johnson and Lindenstrauss.*. For a more detailed explanation, please refer to the original paper.

<!-- more -->

## JL Lemma

For any set $\mathcal{O}$ of $n$ points in $\mathbb{R}^d$, there exists a map $f:\mathbb{R}^{d}\to\mathbb{R}^k$, such that for all $\boldsymbol{o_i},\boldsymbol{o_j}\in\mathcal{P}$:

$$
(1-\epsilon)\Vert\boldsymbol{o_i}-\boldsymbol{o_j}\Vert_2^2\le\Vert\boldsymbol{f(o_i)-f(o_j)}\Vert_2^2\le(1+\epsilon)\Vert\boldsymbol{o_i}-\boldsymbol{o_j}\Vert_2^2
$$

where $0\lt\epsilon\lt 1$ and $k$ is a positive integer satisfying:

$$
k\gt\frac{24}{3\epsilon^2-2\epsilon^3}\log{n}
$$

Let $A$ be a random matrix of size $k\times d$, where each entry is independently drawn from the standard normal distribution $\mathcal{N}(0,1)$. Using this random matrix, we can construct a mapping that satisfies the JL Lemma. For any point $\boldsymbol{p}\in\mathbb{R}^d$, the mapping is defined as:

$$
\boldsymbol{f(p)}=\frac{A}{\sqrt{k}}\cdot\boldsymbol{p}
$$

## Proof of JL Lemma

To prove the JL Lemma, we first establish some auxiliary lemmas and inequalities. For convenience, we will omit the subscript $2$ in the norm, so $\Vert\cdot\Vert^2$ refers to the $2$-norm by default.

### Lemma 1

For any point $\boldsymbol{p}\in\mathbb{R}^d$, let $\boldsymbol{Y}=\frac{A}{\sqrt{k}}\cdot\boldsymbol{p}$. Then, we have:

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

The PDF of the standard normal distribution is:

$$
f(x)=\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}
$$

We now compute $\text{E}\left[e^{\lambda x^2}\right]$:

$$
\text{E}\left[e^{\lambda x^2}\right]=\int_{-\infty}^{+\infty}\frac{1}{\sqrt{2\pi}}e^{\lambda x^2}e^{-\frac{x^2}{2}}\mathrm{d}x=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{+\infty}e^{-(1-2\lambda)\frac{x^2}{2}}\mathrm{d}x
$$

Next, let $y=x\sqrt{1-2\lambda}$, so that $\mathrm{d}y=\sqrt{1-2\lambda}\mathrm{d}x$. Substituting this into the integral, we get:

$$
\text{E}\left[e^{\lambda x^2}\right]=\frac{1}{\sqrt{1-2\lambda}}\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{+\infty}e^{-\frac{y^2}{2}}\mathrm{d}y
$$

Using the well-known result for the standard normal distribution:

$$
\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{+\infty}e^{-\frac{y^2}{2}}\mathrm{d}y=1
$$

Therefore:

$$
\text{E}\left[e^{\lambda x^2}\right]=\frac{1}{\sqrt{1-2\lambda}}
$$

### Lemma 3

For any point $\boldsymbol{p}\in\mathbb{R}^{d}$, let $\boldsymbol{Y}=\frac{A}{\sqrt{k}}\cdot\boldsymbol{p}$. The following inequalities hold:

$$
\begin{align*}
    \Pr\left[\Vert\boldsymbol{Y}\Vert^2\ge(1+\epsilon)\Vert\boldsymbol{p}\Vert^2\right]&\le\frac{1}{n^2}\\
    \Pr\left[\Vert\boldsymbol{Y}\Vert^2\le(1-\epsilon)\Vert\boldsymbol{p}\Vert^2\right]&\le\frac{1}{n^2}
\end{align*}
$$

#### Proof of Lemma 3

We begin by simplifying the inequalities. From the definition of $\boldsymbol{Y}$, we have:

$$
Y_i=\frac{1}{\sqrt{k}}\sum_{j=1}^{d}A_{ij}p_j
$$

We now compute $\Vert\boldsymbol{Y}\Vert^2$:

$$
\Vert\boldsymbol{Y}\Vert^2=\sum_{i=1}^{k}\left(\frac{1}{\sqrt{k}}\sum_{j=1}^{d}A_{ij}p_j\right)^2=\sum_{i=1}^{k}\frac{1}{k}\left(\sum_{j=1}^{d}A_{ij}p_j\right)^2
$$

Since the standard normal distribution is a $2$-stable distribution, using the will-known property, we have:

$$
\Vert\boldsymbol{Y}\Vert^2=\sum_{i=1}^{k}\frac{1}{k}\Vert p\Vert^2X_{i}^{2}=\frac{\Vert p\Vert^2}{k}\sum_{i=1}^{k}X_{i}^{2}
$$

where $X_i$ is a random variable distributed as the standard normal distribution. Substituting this back, the inequalities we need to prove reduce to:

$$
\begin{align*}
    \Pr\left[\sum_{i=1}^{k}X_{i}^{2}\ge k(1+\epsilon)\right]&\le\frac{1}{n^2}\\
    \Pr\left[\sum_{i=1}^{k}X_{i}^{2}\le k(1-\epsilon)\right]&\le\frac{1}{n^2}
\end{align*}
$$

For the first inequality, let $\lambda>0$, then:

$$
\Pr\left[\sum_{i=1}^{k}X_{i}^{2}\ge k(1+\epsilon)\right]=\Pr\left[e^{\lambda\sum_{i=1}^{k}X_i^2}\ge e^{\lambda k(1+\epsilon)}\right]
$$

Using the Markov's inequality, we have:

$$
\Pr\left[e^{\lambda\sum_{i=1}^{k}X_i^2}\ge e^{\lambda k(1+\epsilon)}\right]\le\frac{\text{E}\left[e^{\lambda\sum_{i=1}^{k}X_i^2}\right]}{e^{\lambda k(1+\epsilon)}}=\frac{\text{E}^{k}\left[e^{\lambda X_1^2}\right]}{e^{\lambda k(1+\epsilon)}}
$$

Let $\lambda=\frac{\epsilon}{2(1+\epsilon)}\le\frac{1}{2}$. By Lemma 2, we have:

$$
\frac{\text{E}^{k}\left[e^{\lambda X_1^2}\right]}{e^{\lambda k(1+\epsilon)}}=\left(\frac{1}{\sqrt{1-2\lambda}}\right)^{k}e^{-\lambda k(1+\epsilon)}=\left[(1+\epsilon)e^{-\epsilon}\right]^{\frac{k}{2}}
$$

Using the Taylor expansion for $\ln(1+\epsilon)$:

$$
\ln(1+\epsilon)=\epsilon-\frac{\epsilon^2}{2}+\frac{\epsilon^3}{3}-\frac{\epsilon^4}{4}+\cdots
$$

We have:

$$
1+\epsilon\le e^{\epsilon-\frac{\epsilon^2}{2}+\frac{\epsilon^3}{3}}
$$

Thus:

$$
\left[(1+\epsilon)e^{-\epsilon}\right]^{\frac{k}{2}}\le \left(e^{\epsilon-\frac{\epsilon^2}{2}+\frac{\epsilon^3}{3}}e^{-\epsilon}\right)^{\frac{k}{2}}=e^{-\frac{k(3\epsilon^2-2\epsilon^3)}{12}}
$$

If $k$ satisfies:

$$
k\gt\frac{24}{3\epsilon^2-2\epsilon^3}\log{n}
$$

then:

$$
e^{-\frac{k(3\epsilon^2-2\epsilon^3)}{12}}\le e^{-2\log{n}}=\frac{1}{n^2}
$$

Therefore:

$$
\Pr\left[\sum_{i=1}^{k}X_{i}^{2}\ge k(1+\epsilon)\right]\le\frac{1}{n^2}
$$

Similarly, for the second inequality, let $\lambda>0$, then:

$$
\begin{align*}
\Pr\left[\sum_{i=1}^{k}X_{i}^{2}\le k(1-\epsilon)\right]&=\Pr\left[e^{-\lambda\sum_{i=1}^{k}X_i^2}\ge e^{-\lambda k(1-\epsilon)}\right] \\
&\le\frac{\text{E}\left[e^{-\lambda\sum_{i=1}^{k}X_i^2}\right]}{e^{-\lambda k(1-\epsilon)}} \\
&=\frac{\text{E}^{k}\left[e^{-\lambda X_1^2}\right]}{e^{-\lambda k(1-\epsilon)}}
\end{align*}
$$

Let $\lambda=\frac{\epsilon}{2(1-\epsilon)}$, so that $-\lambda=\frac{\epsilon}{2(\epsilon-1)}\lt 0\lt \frac{1}{2}$. We have:

$$
\frac{\text{E}^{k}\left[e^{-\lambda X_1^2}\right]}{e^{-\lambda k(1-\epsilon)}}=\left(\frac{1}{\sqrt{1+2\lambda}}\right)^ke^{\lambda k(1-\epsilon)}=\left[(1-\epsilon)e^{\epsilon}\right]^{\frac{k}{2}}
$$

Similarly, using the Taylor expansion for $\ln(1-\epsilon)$:

$$
\ln(1-\epsilon)=-\epsilon-\frac{\epsilon^2}{2}-\frac{\epsilon^3}{3}\cdots
$$

we have:

$$
1-\epsilon\le e^{-\epsilon-\frac{\epsilon^2}{2}}
$$

Thus:

$$
\left[(1-\epsilon)e^{\epsilon}\right]^{\frac{k}{2}}\le e^{-\frac{k\epsilon^2}{4}}\lt e^{-\frac{6\epsilon^2}{3\epsilon^2-2\epsilon^3}\log{n}}\lt e^{-2\log{n}}=\frac{1}{n^2}
$$

Therefore:

$$
\Pr\left[\sum_{i=1}^{k}X_{i}^{2}\le k(1-\epsilon)\right]\le\frac{1}{n^2}
$$

### Proof of the JL Lemma

Given any point $\boldsymbol{p}\in\mathbb{R}^d$, we will use Lemma 3 and Boole's inequality to show that the JL Lemma holds.

From Lemma 3, for any point $\boldsymbol{p}\in\mathbb{R}^{d}$, the following holds:

$$
\Pr\left[\Vert\boldsymbol{f(p)}\Vert^2\notin\left[(1-\epsilon)\Vert\boldsymbol{p}\Vert^{2},(1+\epsilon)\Vert\boldsymbol{p}\Vert^{2}\right]\right]\le\frac{2}{n^2}
$$

Since this probability holds for any point $\boldsymbol{p}\in\mathbb{R}^d$, it also holds for any pair of points $\boldsymbol{u}=\boldsymbol{o_i}-\boldsymbol{o_j}$, where $\boldsymbol{o_i},\boldsymbol{o_j}\in\mathcal{O}$.

Furthermore, using the linearity of the projection, we have:

$$
\boldsymbol{f(o_1-o_2)}=\boldsymbol{f(o_1)}-\boldsymbol{f(o_2)}
$$

Therefore, applying Lemma 3 to the difference between two points $\boldsymbol{o_i}$ and $\boldsymbol{o_j}$, we obtain:

$$
\Pr\left[\Vert\boldsymbol{f(o_i)}-\boldsymbol{f(o_j)}\Vert^2\notin\left[(1-\epsilon)\Vert\boldsymbol{o_i}-\boldsymbol{o_j}\Vert^{2},(1+\epsilon)\Vert\boldsymbol{o_i}-\boldsymbol{o_j}\Vert^{2}\right]\right]\le\frac{2}{n^2}
$$

The number of pairs of points $\boldsymbol{o_i},\boldsymbol{o_j}\in\mathcal{O}$ is $\binom{n}{2}$. Thus, using Boole's inequality, the probability that at least one pair of points falls outside the desired error bound is at most:

$$
\frac{n(n-1)}{2}\frac{2}{n^2}=1-\frac{1}{n}
$$

Thus, with probability at least $\frac{1}{n}\gt 0$, all pairs of points in $\mathcal{O}$ will fall within the desired error bounds, completing the proof of the JL Lemma.