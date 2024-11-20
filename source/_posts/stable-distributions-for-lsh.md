---
title: Stable Distributions for LSH
date: "2024-11-20 11:00:00+08:00"
categories:
    - Algorithm
tags:
    - LSH
    - ANNS
    - Stable Distribution
cover: /img/covers/serene_twilight_lake.jpg
thumbnail: /img/covers/serene_twilight_lake.jpg
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

Given two vector $\boldsymbol{o_1}=(o_{11},o_{12},\cdots,o_{1d})$ and vector $\boldsymbol{o_2}=(o_{21},o_{22},\cdots,o_{2d})$, we can easily prove that the absolute value of the different between
