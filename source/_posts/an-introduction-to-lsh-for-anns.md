---
title: An Introduction to LSH for ANNS
date: "2024-11-18 12:00:00+08:00"
categories:
    - Locality-Sensitive Hashing
tags:
    - ANNS
---

This article introduces Locality-Sensitive Hashing (LSH) as an effective technique for Approximate Nearest Neighbor Search (ANNS) in high-dimensional spaces, addressing the "curse of dimensionality." By using hash functions that map similar data points into the same buckets, LSH reduces the general $c$-NNS problem into manageable $(R, c)$-NNS subproblems. This approach enables fast approximate searches with acceptable accuracy, effectively tackling the challenges of high-dimensional data search.

<!-- more -->

## Nearest Neighbor Search and the Curse of Dimensionality

Nearest Neighbor Search (NNS) is a fundamental problem in computer science and data analysis, with applications in areas such as machine learning, computer vision, and information retrieval.

Mathematically, given a dataset $\mathcal{O}=\{\boldsymbol{o_1},\boldsymbol{o_2},...,\boldsymbol{o_n}\}$, where $\boldsymbol{o_i}$ represents a point in $d$-dimensional space, the goal of NNS is to find a point $\boldsymbol{o^{\ast}} \in\mathcal{O}$ such that:

$$
\text{dist}(\boldsymbol{o^{\ast}},\boldsymbol{q})=\min_{\boldsymbol{o_i}\in\mathcal{O}}\text{dist}(\boldsymbol{o_i},\boldsymbol{q})
$$

where $\boldsymbol{q}\in\mathbb{R}^{d}$ is the query point, and the $\mathrm{dist}(\cdot,\cdot)$ is a distance metric.

In low-dimensional spaces, exact nearest neighbor search is generally well understood and can be performed efficiently. Traditional methods, such as KD-trees, can quickly find the nearest neighbors with good performance when the data is low-dimensional. These techniques work by organizing the data in such a way that the search process can be narrowed down to a small subset of the dataset, reducing the number of comparisons needed.

However, as the number of dimensions increases, the performance of these exact search methods starts to degrade significantly. This phenomenon is known as the "curse of dimensionality". In fact, for sufficiently high dimensions, in theory or in practice, they often provide little improvement over linear search which compares a query to each point from the database.

## The Need for Approximate Nearest Neighbor Search

To overcome the limitations imposed by the curse of dimensionality, researchers have proposed the use of approximate nearest neighbor search (ANNS) methods. Instead of finding the exact closest neighbors, ANNS algorithms aim to find a solution that is close enough to the query point. The trade-off between accuracy and speed is acceptable in many applications.

In the context of ANNS, the goal is to find a point $\boldsymbol{\hat{o}}$ from the dataset $\mathcal{O}$ that is approximately the nearest neighbor to a query point $\boldsymbol{q}$. Instead of minimizing the exact distance, the objective is to find $\boldsymbol{\hat{o}}$ such that the distance between $\boldsymbol{q}$ and $\boldsymbol{\hat{o}}$ is within some approximation factor $c$ of the true nearest neighbor distance.

$$
\mathrm{dist}(\boldsymbol{\hat{o}},\boldsymbol{q})\le c\cdot\mathrm{dist}(\boldsymbol{o^\ast}, \boldsymbol{q})=c\cdot\min_{\boldsymbol{o_i}\in\mathcal{O}}\mathrm{dist}(\boldsymbol{o_i},\boldsymbol{q})
$$

where $c\ge 1$ is the approximation ratio that controls the trade-off between accuracy and computational efficiency. Since the ANNS problem is highly related to the parameter $c$, it can also be referred to the $c$-NNS problem, and $\boldsymbol{\hat{o}}$ can be referred to as the $c$-NN of $\boldsymbol{q}$.

## Introduction to Locality-Sensitive Hashing (LSH)

Locality-Sensitive Hashing (LSH) is one of the most popular and effective techniques for solving $c$-NNS, especially in high-dimensional space. Essentially, Locality-Sensitive Hashing (LSH) solves the $c$-NNS by addressing a related problem known as the approximate **near** neighbor search problem. The approximate near neighbor search problem is the decision version of $c$-NNS, focusing on whether a point exists within a specified distance from the query.

Given a dataset $\mathcal{O}\subset\mathbb{R}^d$, a query point $\boldsymbol{q}\in\mathbb{R}^d$, a distance threshold $R>0$, and a approximation ratio $c\ge 1$, the approximate **near** neighbor search problem aims to design a data structure that satisfies the following conditions:

- If there exists $\boldsymbol{o_{i}}\in\mathcal{O}$ satisfying $\text{dist}(\boldsymbol{o_i},\boldsymbol{q})\le r$, the data structure should return YES and a point $\boldsymbol{\tilde{o}}\in\mathcal{O}$ satisfying $\text{dist}(\boldsymbol{\tilde{o}},\boldsymbol{q})\le cr$,
- If for all $\boldsymbol{o_i}\in\mathcal{O}$, $\text{dist}(\boldsymbol{o_i},\boldsymbol{q})\gt cr$ , the data structure should return NO.
- Otherwise, the result is undefined.

The approximate near neighbor search problem is strongly influenced by the parameters $c$ and $R$, and it is often referred to as the $(R,c)$-NNS problem. In this context, the point $\boldsymbol{\tilde{o}}$ is called the $(R,c)$-NN of the query.

We will discuss how to reduce the $c$-NNS problem to the $(R,c)$-NNS problem. For now, let's focus on how LSH can be used to solve $(R,c)$-NNS problem.

### Locality-Sensitive Hashing Function Family

A key idea behind LSH is to use a family of locality-sensitive hash functions to hash points in a way that nearby points in the original space is likely to be mapped to the same hash bucket. The goal is to efficiently find approximate nearest neighbors by ensuring that nearby points are grouped together with high probability, while distant points are less likely to be hashed to the same bucket.

Given a dataset $\mathcal{O}\in\mathbb{R}^d$ and a distance metric $\text{dist}(\cdot,\cdot)$ associated with a distance measure $D$, a family of hash family $H$ is called $(r_1,r_2,p_1,p_2)$-sensitive for $D$ if, for any pair of point $\boldsymbol{o_i},\boldsymbol{o_j}\in\mathcal{P}$ and any $h\in\mathcal{H}$, the following conditions hold:

-   if $\text{dist}(\boldsymbol{o_i},\boldsymbol{o_j})\le r_1$, then $\Pr[h(\boldsymbol{o_i})=h(\boldsymbol{o_j})]\ge p_1$
-   if $\text{dist}(\boldsymbol{o_i},\boldsymbol{o_j})\gt r_2$, then $\Pr[h(\boldsymbol{o_i})=h(\boldsymbol{o_j})]\le p_2$

This family $\mathcal{H}$ is referred to as the LSH family for the distance measure $D$. To improve performance, it is desirable to amplify the gap between $p_1$ and $p_2$, which can be achieved by concatenating multiple LSH functions. Specifically, for a given $k$, define a new family of function $\mathcal{G}$ such that for each $x$, the function $g(\boldsymbol{o})$ is defined as:

$$
g(\boldsymbol{o})=(h_1(\boldsymbol{o}), h_2(\boldsymbol{o}),\cdots,h_k(\boldsymbol{o}))
$$

### Use LSH Family to Solve ANNS Problem

We will briefly describe how a LSH family can be used to solve the $(R,c)$-NNS problem. First, we set $r_1=R$ and $r_2=c\cdot R$. For an integer $L$ we independently and uniformly choose $L$ function $g_1,g_2,\cdots,g_L$ from the LSH family $\mathcal{G}$.

To process a query, we search through the buckets correspondent to $g_1(\boldsymbol{q}),g_2(\boldsymbol{q}),\cdots,g_L(\boldsymbol{q})$, retrieve the points (including duplicates) from the each bucket until one of the following conditions is met:

- All points from the $L$ buckets have been retrieved, or

- The total number of points retrieved exceeds $3L$.

Let $\boldsymbol{o_1}, \boldsymbol{o_2}, ..., \boldsymbol{o_t}$ be the points encountered during this process. If there exists a point $\boldsymbol{o_i}$ such that $\text{dist}(\boldsymbol{o_i},\boldsymbol{q})\le r_2=c\cdot R$, then we return YES along with the point $\boldsymbol{o_i}$. Otherwise, we return NO.

Observer that if the following two events hold, the algorithm is correct:

- $E_1$: If there exists $\boldsymbol{o_i}\in\mathcal{O}$ such that $\mathrm{dist}(\boldsymbol{o_i},\boldsymbol{q})\le R$, then $g_j(\boldsymbol{o_i})=g_j(\boldsymbol{q})$ for some $j\in\{1,\cdots,L\}$.
- $E_2$: The total number of elements $\boldsymbol{o}\in\mathcal{O}$ such that $g_j(\boldsymbol{o})=g_j(\boldsymbol{q})$ and $\mathrm{dist}(\boldsymbol{o},\boldsymbol{q})>cR$ is at most $3L$.

The parameters $k$ and $L$ are chosen so as to ensure that with a constant probability, both of these events hold. For convenience, we assume the event $E_1$ holds with probability $P(E_1)$ and event $E_2$ holds with probability $P(E_2)$.

Set $k=\lceil\log_{1/p_2}n\rceil$, where $n=\left|\mathcal{O}\right|$. For fix $j$, the probability that $g_j(\boldsymbol{o_i})=g_j(\boldsymbol{q})$ is at most:

$$
p_2^k=p_2^{\lceil\log_{1/p_2}n\rceil}\le p_2^{\log_{1/p_2}n}=\frac{1}{n}
$$

Therefore, the expect number of the points satisfying $\text{dist}(\boldsymbol{o_i},\boldsymbol{q})$ that collide with $\boldsymbol{q}$ via $g_j$ is at most $1$. Since there are $L$ hash functions, by Markov's inequality, the probability that the number of collisions exceeds $3L$ is at most 

$$
L\cdot\frac{1}{3L}=\frac{1}{3}
$$

Thus, $P(E_2)\ge\frac{2}{3}$.

If there exists an $\boldsymbol{o_i}\in\mathcal{O}$ such that  $\mathrm{dist}(\boldsymbol{o_i},\boldsymbol{q})\le R$, for a fixed $j$, we have:

$$
\Pr[g_j(\boldsymbol{o_i})=g_j(\boldsymbol{q})]=p_1^k
$$

We then get:

$$
p_1^k=p_1^{\lceil\log_{1/p_2}n\rceil}\gt p_1^{\log_{1/p_2}n+1}=n^{\log_{1/p_2}p_1}p_1=n^{-\frac{\log(1/p_1)}{\log(1/p_2)}}p_1=n^{-\rho}p_1
$$

where $\rho=\frac{\log(1/p_1)}{\log(1/p_2)}$. The probability that a collision occurs in at least one of the hash functions is at least:

$$
1-(1-p_1^{k})^{L}
$$

We set $L=n^{\rho}/p_1$, so we have:

$$
P(E_1) \ge 1-(1-p_1^{k})^{L} = 1-(1-n^{-\rho}p_1)^L=1-(1-n^{-\rho}p_1)^{n^{\rho}/p_1}>1-\frac{1}{e}
$$

The second-to-last inequality is from the fact that for $x\ge 1$,

$$
\left(1-\frac{1}{x}\right)^{x}<\frac{1}{e}
$$

Thus, the probability that both event $E_1$ and event $E_2$ hold simultaneously is:

$$
P(E_1\cap E_2)=P(E_1)+P(E_2)-P(E_1\cup E_2)\ge P(E_1)+P(E_2)-1>\frac{2}{3}-\frac{1}{e}
$$

Therefore, the failure probability is at most $1-(\frac{2}{3}-\frac{1}{e})=\frac{1}{3}+\frac{1}{e}<1$.

By demonstrating that the failure probability is less than 1, the proof establishes that the algorithm has a chance of success. Additionally, the failure probability can be further reduced by building and querying several instances of the data structure. For example, if two independent data structures are built, each with a failure probability $\delta$, the combined failure probability for the system is $\delta^2$.

## Reduce ANNS Problem

We have discussed how to use LSH to solve $(R,c)$-NNS problem, but the ultimate goal is to solve $c$-NNS problem. Therefore, the remain task is to reduce the $c$-NNS problem to $(R,c)$-NNS problem. It is important to note that the parameter $c$ in $c$-NNS is different from the parameter $c$ in $(R,c)$-NNS. Specifically, we can reduce the $c^2$-NNS problem to multiple $(R,c)$-NNS problem as follows.

let $R_0$ be the smallest **possible** distance between $\boldsymbol{q}$ and a point in $\mathcal{O}$. For example, in the case of Hamming distance, the smallest possible distance is 1. Then define the set of radii:

$$
R=\{R_0, cR_0, c^2R_0, c^3R_0,\cdots\}
$$

there must exist an $i$ such that:

$$
c^iR_0\lt\text{dist}(o^\ast,q)\le c^{i+1}R_0
$$

where $\boldsymbol{o^\ast}$ is the exact nearest neighbor of $\boldsymbol{q}$. By the definition of the $(R,c)$-NNS, the algorithm must return a point $\boldsymbol{\tilde{o}}$ such that:

$$
\text{dist}(\boldsymbol{\tilde{o}},\boldsymbol{q})\le c^{i+2}R_0=c^2\cdot c^iR_0\lt c^2\text{dist}(\boldsymbol{o^\ast},\boldsymbol{q})
$$

Thus, $\boldsymbol{\tilde{o}}$ is the $c^2$-NN of the $\boldsymbol{q}$.