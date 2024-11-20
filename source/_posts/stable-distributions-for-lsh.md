---
title: Stable Distributions for LSH
date: "2024-11-20 11:00"
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

The $\ell_p$ norm is a mathematical concept used to measure the "length" or "distance" of a vector in a space. It generalizes the idea of distance in different ways, depending on the value of $p$. The $\ell_p$ norm of a vector $o=(o_1,o_2,\cdots,o_d)$ in $\mathbb{R}^d$ is defined as:

$$
\Vert o\Vert_p=\left(\sum_{i=1}^d\left|o_i\right|^p\right)^{1/p}
$$

where $p\ge 1$. 

### Special Cases of the $\ell_p$ Norm:

1. $\ell_1$ norm (Manhattan Distance)：
The $\ell_1$ norm, also known as the "Manhattan Distance", measures the sum of the absolute values of the vector components. Its mathematical expression is:
$$
\Vert o\Vert_1=\sum_{i=1}^d\left|o_i\right|
$$

2. $\ell_2$ norm (Euclidean Distance):
The $\ell_2$ norm, also known as the "Euclidean Distance", measures the straight-line distance between the origin and the point represented by the vector. Its mathematical expression is:
$$
\Vert o\Vert_2=\sqrt{\sum_{i=1}^do_i^2}
$$

3. $\ell_\infty$ norm (Chebyshev Distance):
The $\ell_\infty$ norm, also known as the "Chebyshev Distance", measures the largest absolute value among the vector components. Its mathematical expression is:
$$
\Vert o\Vert_\infty=\max_{i=1,\cdots,d}\left| o_i\right|
$$

## LSH for $(R,c)$-NNS Under $\ell_p$ Norm

To solve the  $(R,c)$-NNS problem using LSH, the key idea is to  desing a family of LSH functions $\mathcal{H}$, such that, for any $h\in\mathcal{H}$, the following conditions holds:

- If $\Vert o_1-o_2\Vert_p\le r_1$, then $\Pr\left[h(o_1)=h(o_2)\right]\ge p_1$
- If $\Vert o_1-o_2\Vert_p\gt r_2$, then $\Pr[h(o_1)=h(o_2)]\le p_2$

In other words, we need to find out a method to construct the hash function whose collision probability is associated with the $\ell_p$ norm between two points.

