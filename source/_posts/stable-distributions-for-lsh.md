---
title: Stable Distributions for LSH
date: "2024-11-23 01:39:00+08:00"
categories:
    - Locality-Sensitive Hashing
tags:
    - ANNS
    - Norm
    - Stable Distribution
---
In the previous post, we discussed that LSH addresses the $c$-NNS problem by solving multiple $(R,c)$-NNS problems using different radii. In this article, we will explore how to construct LSH families specifically designed to solve the $(R,c)$-NNS problems under the $\ell_p$ norm, leveraging the properties of $p$-stable distribution.

<!-- more -->

## $\ell_p$ Norm

The $\ell_p$ norm is a mathematical concept used to measure the length or distance of a vector in a space. It generalizes the idea of distance in different ways, depending on the value of $p$. The $\ell_p$ norm of a vector $\boldsymbol{o}=(o_1,o_2,\cdots,o_d)$ in $\mathbb{R}^d$ is defined as:

$$
\Vert \boldsymbol{o}\Vert_p=\left(\sum_{i=1}^d\left|o_i\right|^p\right)^{1/p}
$$

where $p\ge 1$. 

### Special Cases of the $\ell_p$ Norm

1. $\ell_1$ Norm (Manhattan Distance)：
The $\ell_1$ norm, also known as the Manhattan distance, measures the sum of the absolute values of the vector components. Its mathematical expression is:
$$
\Vert \boldsymbol{o}\Vert_1=\sum_{i=1}^d\left|o_i\right|
$$

2. $\ell_2$ Norm (Euclidean Distance):
The $\ell_2$ norm, also known as the Euclidean distance, measures the straight-line distance between the origin and the point represented by the vector. Its mathematical expression is:
$$
\Vert \boldsymbol{o}\Vert_2=\sqrt{\sum_{i=1}^do_i^2}
$$

3. $\ell_\infty$ Norm (Chebyshev Distance):
The $\ell_\infty$ norm, also known as the Chebyshev distance, measures the largest absolute value among the vector components. Its mathematical expression is:
$$
\Vert \boldsymbol{o}\Vert_\infty=\max_{i=1,\cdots,d}\left| o_i\right|
$$

## LSH for $(R,c)$-NNS Under $\ell_p$ Norm

To solve the  $(R,c)$-NNS problem using LSH, the key idea is to design a family of LSH functions $\mathcal{H}$, such that, for any $h\in\mathcal{H}$, the following conditions holds:

- If $\Vert \boldsymbol{o_1}-\boldsymbol{o_2}\Vert_p\le r_1$, then $\Pr\left[h(\boldsymbol{o_1})=h(\boldsymbol{o_2})\right]\ge p_1$
- If $\Vert \boldsymbol{o_1}-\boldsymbol{o_2}\Vert_p\gt r_2$, then $\Pr[h(\boldsymbol{o_1})=h(\boldsymbol{o_2})]\le p_2$

In other words, we aim to construct hash functions where the probability of two points colliding (hashing to the same bucket) is directly related to their $\ell_p$-norm distance. Points that are close in $\ell_p$-norm have a higher collision probability, which points that are far apart have a lower collision probability.

In the paper Locality-sensitive hashing scheme based on p-stable distributions, Datar et al. proposed LSH families that leverage the properties of stable distributions, we will discuss the stable distributions and their connection to LSH families in detail shortly.

### Stable Distributions

Stable distributions are a family of probability distributions that have a special property: any linear combination of independent random variables that follow a stable distribution also follows the same stable distribution (up to scaling and shifting).

Formally, a distribution $\mathcal{D}$ is stable if, for independent random variables $X_1,X_2,\cdots,X_n\sim\mathcal{D}$, and constants $a_1,a_2,\cdots,a_n$, the linear combination:
$$
S=a_1X_1+a_2X_1+\cdots+a_nX_n
$$
is distributed as:
$$
S\sim bX+c
$$
where $X\sim\mathcal{D}$, and $b>0$ and $c$ are some constants. 

Specifically, if the linear combination is distributed as:
$$
S\sim\left(\sum_{i=1}^n\left|a_i\right|^p\right)^{1/p}X
$$
then the distribution $\mathcal{D}$ is said to be $p$-stable. It is known that stable distributions exist for any $p\in(0,2]$. In particular:

- Cauchy Distribution $(\mathcal{D}_C)$:
The Cauchy distribution is 1-stable and is defined by the density function:
$$
c(x)=\frac{1}{\pi}\frac{1}{1+x^2}
$$

- Gaussian Distribution $(\mathcal{D}_G)$
The Gaussian (normal) distribution is 2-stable and is defined by the density function:

$$
g(x)=\frac{1}{\sqrt{2\pi}}\exp\left(-\frac{x^2}{2}\right)
$$

### $p$-stable Distributions and LSH families

Given a vector $\boldsymbol{o}=(o_1,o_2,\cdots,o_d)$ and $d$ independent random variables $X_1,X_2,\cdots,X_d$ that follow a $p$-stable distribution $\mathcal{D}$, the property of the $p$-stable distribution ensures that the linear combination of these random variables:
$$
S=o_1X_1+o_2X_2+\cdots+o_dX_d
$$
is distributed as:
$$
S\sim\left(\sum_{i=1}^d\left|o_i\right|^p\right)^{1/p}X=\Vert \boldsymbol{o}\Vert_pX
$$
where $X\sim\mathcal{D}$. 

Leveraging this property, Datar et al. design an LSH family $\mathcal{H}$ for solving the $(R,c)$-NNS problem. The hash function $h\in\mathcal{H}$ is defined as:
$$
h(\boldsymbol{o})=\left\lfloor\frac{\boldsymbol{a}\cdot\boldsymbol{o}+b}{w}\right\rfloor
$$
Where:

- $\boldsymbol{a}$ is a $d$ dimensional vector with entries chosen independently from a $p$-stable distribution, 
- $w$ is a fixed constant, and
- $b$ is a real number chosen uniformly from $[0,w]$. 

We will prove that $\mathcal{H}$ is indeed an LSH family shortly.

The hash function can be intuitively understood as a series of steps applied to a given point:

1. The point $\boldsymbol{o}$ is projected onto a random one-dimensional line defined by $\boldsymbol{a}$.
2. A random offset $b$ is added to the projection to introduce randomness.
3. The random line is divided into segments of equal width $w$, The segment that the projection falls into is determined using division and the floor operation.
4. The index of the segment where the projection falls into is assigned as the hash value of $\boldsymbol{o}$.

The geometric interpretation of this process is as follows:

![geometric interpretation of LSH](stable-distributions-for-lsh/geometric-interpretation-of-LSH.svg)

Given two points $\boldsymbol{o_1}$ and $\boldsymbol{o_2}$, the projection of $\boldsymbol{o_1}$ and $\boldsymbol{o_2}$ fall into the same hash bucket if and only if the following two events occur simultaneously:

1. event $E_1$: the distance between the projection of $\boldsymbol{o_1}$ and $\boldsymbol{o_2}$ smaller than the segment width $w$. Mathematically:

$$
\left|\left(\boldsymbol{a}\cdot\boldsymbol{o_1}+b\right)-\left(\boldsymbol{a}\cdot\boldsymbol{o_2}+b\right)\right|=\left|\boldsymbol{a}\cdot\boldsymbol{o_1}-\boldsymbol{a}\cdot\boldsymbol{o_2}\right|\lt w
$$

2. event $E_2$: there is not segment boundary between the projection of $\boldsymbol{o_1}$ and $\boldsymbol{o_2}$. This ensures both projections fall into the same segment.

Thus, the probability of a collision is the probability that both events $E_1$ and $E_2$ hold simultaneously. 

For convenience, let $U=\left|\boldsymbol{a}\cdot(\boldsymbol{o_1}-\boldsymbol{o_2})\right|$. The probability $P(E_1)$ is:

$$
P(E_1)=\Pr\left[U<w\right]=\int_{0}^wf_U(u)\mathrm{d}u
$$

where $f_U(u)$ is the probability density function (PDF) of the random variable $U$. Given a realization $u$ of $U$, the conditional probability $P(E_2\mid u)$ is:

$$
P(E_2\mid u)=1-\frac{u}{w}
$$
Therefore, the overall collision probability can be calculated as:
$$
\Pr[h(\boldsymbol{o_1})=h(\boldsymbol{o_2})]=\int_0^wf_U(u)\left(1-\frac{u}{w}\right)\mathrm{d}u
$$
Let $s=\Vert\boldsymbol{o_1}-\boldsymbol{o_2}\Vert_p$. By the properties of $p$-stable distributions, we have $U=s|X|$ , where $|X|$ is the absolute value of a random variable following a $p$-stable distribution. Let $F_U(\cdot)$  and $F_{|X|}(\cdot)$ denote the cumulative distribution function (CDF) of $U$ and $|X|$, respectively. Then:
$$
F_U(u)=\Pr[U\le u]=\Pr[s|X|\le u]=\Pr\left[|X|\le\frac{u}{s}\right]=F_{|X|}\left(\frac{u}{s}\right)
$$
Differentiating $F_U(u)$, we obtain the PDF of $U$:

$$
f_{U}(u)=F^{\prime}_{U}(u)=\frac{\mathrm{d}}{\mathrm{d}u}F_{\lvert X\rvert}\left(\frac{u}{s}\right)=\frac{1}{s}f_{\lvert X\rvert}\left(\frac{u}{s}\right)
$$

where $f_{|X|}(\cdot)$ is the PDF of $|X|$. Substituting this back, the collision probability becomes:

$$
\Pr[h(\boldsymbol{o_1})=h(\boldsymbol{o_2})]=\int_0^w\frac{1}{s}f_{|X|}\left(\frac{u}{s}\right)\left(1-\frac{u}{w}\right)\mathrm{d}u
$$

For convenience, we omit the subscript $|X|$. Since the collision probability depends on $s$, we denote it as $p(s)$, where:

$$
p(s)=\int_0^w\frac{1}{s}f\left(\frac{u}{s}\right)\left(1-\frac{u}{w}\right)\mathrm{d}u
$$
Notice that the collision probability $p(s)$ decreases monotonically as the distance $s$ between two points increases. For given $R$ and $c$, the hash function $h$ is $(R,cR,p(R),p(cR))$-sensitive.

### LSH Families for Special Cases of the $\ell_p$ norm

As previously mentioned, $p$-stable distributions exist for any $p\in(0,2]$. The primary difference in designing LSH families for different values of $p$ lies in the choice of $p$-stable distribution from which the entries of $\boldsymbol{a}$ are drawn. Here, we introduce two special distributions for the cases $p=1$ and $p=2$. Using these distributions, we can construct LSH families for $(R,c)$-ANNS under $\ell_1$ and $\ell_2$ norms.

#### LSH Family for $\ell_1$ Norm

The standard Cauchy distribution is a 1-stable distribution. Its PDF is given by:
$$
f(x)=\frac{1}{\pi(1+x^2)}
$$
By utilizing the standard Cauchy distribution to design the LSH family for $(R,c)$-ANNS, the resulting LSH family is $\left(R,cR,\frac{1}{\pi(1+R^2)},\frac{1}{\pi(1+c^2R^2)}\right)$-sensitive.

#### LSH Family for $\ell_2$ Norm

The standard Gaussian distribution is a 2-stable distribution. Its PDF is given by:
$$
f(x)=\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}
$$
By leveraging the standard Gaussian distribution to design the LSH family for $(R,c)$-ANNS, the resulting LSH family is $\left(R,cR,\frac{1}{\sqrt{2\pi}}e^{-\frac{R^2}{2}},\frac{1}{\sqrt{2\pi}}e^{-\frac{c^2R^2}{2}}\right)$-sensitive.
