---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2024-01-10
featured: true
title: "Theorems related to Geometry"
description: 'Collection of Theorems related to Geometry'
_styles: >
    .table {
        padding-top:200px;
        margin-bottom: 2.5rem;
        border-bottom: 2px;
    }
    .p {
        font-size:20px;
    }
---
<style>
blockquote {
    width: 100%; 
}
</style>

## AM–GM inequality

The arithmetic mean 

$$
AM = \frac{x_1+\cdots x_n}{n}
$$

The geometric mean 

$$
GM = \sqrt[^n]{x_1+\cdots x_n} 
$$

We have 

$$
AM \ge GM \\ 
\frac{x_1+\cdots x_n}{n} \ge  \sqrt[^n]{x_1+\cdots x_n} 
$$


## Aitchison Geometry 

From [Wikipedia](https://en.wikipedia.org/wiki/Compositional_data)

Consider a compositional data $x=(x_1, x_2, \cdots, x_D) \in \mathcal{S}^D$ where 

$$
\mathcal{S}^D = \{ 
    \mathbf{x} = [x_1, x_2, \cdots, x_D] \in \mathbb{R}^D \vert x_i >0, i=1,2,\cdots, D; \sum_{i=1}^D x_i = \kappa \}
$$

<blockquote>
Note that the end point of the x_i excludes zero, hence it is opened. <br> Therefore, reaching to the point, such as $(1,0)$ is equivalent to reaching $\infty$ for a given isomorphic transformation. 
</blockquote>

The information of a compositional data is preserved under multiplication by any positive constant. The normalization to the standard simplex is called closure and denoted by $C[\cdot]$:

$$
C[x_1, x_2, \ldots, x_D] = \left[ \frac{x_1}{\sum_{i=1}^{D} x_i}, \frac{x_2}{\sum_{i=1}^{D} x_i}, \ldots, \frac{x_D}{\sum_{i=1}^{D} x_i} \right]
$$


The simplex can be given the structure of a vector space in several different ways. The following vector space structure is called Aitchison geometry or the Aitchison simplex and has the following operations:


1.Perturbation (vector addition)

$$
\mathbf{x} \oplus \mathbf{y} = \left[\frac{x_1 y_1}{\sum_{i=1}^{D} x_i y_i}, \frac{x_2 y_2}{\sum_{i=1}^{D} x_i y_i}, \ldots, \frac{x_D y_D}{\sum_{i=1}^{D} x_i y_i}\right] = C[x_1 y_1, \ldots, x_D y_D] \quad \forall \mathbf{x}, \mathbf{y} \in S^D
$$

2.Powering (scalar multiplication)

$$
\alpha \odot \mathbf{x} = \left[ \frac{x_1^\alpha}{\sum_{i=1}^{D} x_i^\alpha}, \frac{x_2^\alpha}{\sum_{i=1}^{D} x_i^\alpha}, \ldots, \frac{x_D^\alpha}{\sum_{i=1}^{D} x_i^\alpha} \right] = C[x_1^\alpha, \ldots, x_D^\alpha] \quad \forall \mathbf{x} \in S^D, \ \alpha \in \mathbb{R}
$$

3.Inner Product

$$
\langle \mathbf{x}, \mathbf{y} \rangle = \frac{1}{2D} \sum_{i=1}^{D} \sum_{j=1}^{D} \log \frac{x_i}{x_j} \log \frac{y_i}{y_j} \quad \forall \mathbf{x}, \mathbf{y} \in S^D
$$

<blockquote>
Note that the inner product in Euclidean space is defined by  

$$
\langle \mathbf{x}, \mathbf{y} \rangle = \sum_{i=1}^D \textcolor{blue}{x_i} \cdot \textcolor{green}{y_i}
$$. 

Similarly the inner product in Aitchison geometry is defined in the form of 

$$
\langle \mathbf{x}, \mathbf{y} \rangle = \sum_{i=1}^D  \textcolor{blue}{(\cdot)_x} \cdot \textcolor{green}{(\cdot)_y }
$$ 
and the framework is equal to the inner product of Euclidean space. 

On consideration in Aitchison geometry is that the ratio between the elements is important. Hence, 
we have the following definition of the inner product. 

$$
\langle \mathbf{x}, \mathbf{y} \rangle = \frac{1}{2D} \sum_{i=1}^{D} \sum_{j=1}^{D}\textcolor{blue}{\Big( \log \frac{x_i}{x_j} \Big)} \textcolor{green}{\Big(  \log \frac{y_i}{y_j} \Big)} \quad \forall \mathbf{x}, \mathbf{y}   \in S^D
$$

</blockquote>




The uniform composition  $[1/D, 1/D, \cdots, 1/D]$ is the zero vector.


### Additive log ratio transform 
(isomorphism) $S^D \rightarrow \mathbb{R}^{D-1}$

$$
\operatorname{alr}(\mathbf{x}) = \Big[\log \frac{x_1}{x_D} \cdots \frac{x_{D-1}}{x_D}  \Big]
$$

### Center log ratio transform 
(isomorphism and an isometry) $S^D \rightarrow U$, $U \subset \mathbb{R}^{D-1}$

$$
\operatorname{clr}(\mathbf{x}) = \Big[\log \frac{x_1}{g(x)} \cdots \frac{x_{D-1}}{g(x)}  \Big]
$$

where $g(x)$ is the geometric mean of $x$.  That is, $\Big( \prod_{i=1}^n x_i \Big)^{(1/n)}$


### Isometric log ratio transform 
(isomorphism and an isometry) $S^D \rightarrow \mathbb{R}^{D-1}$

$$
\operatorname{ilr}(\mathbf{x}) = \Big[\langle \mathbf{x}, \mathbf{e}_1 \rangle, \cdots, \langle \mathbf{x}, \mathbf{e}_{D-1} \rangle \Big]
$$

where $e_1, e_2, \cdots, e_{D-1}$ are orthonormal bases. Once the basis $\Psi$ is built, the ilr transform can be calculated as follows

$$
\operatorname{ilr}(\mathbf{x}) = \operatorname{clr}(\mathbf{x}) \Psi^T
$$

Every composition $x$ can be decomposed as follows

$$
\mathbf{x} = \bigoplus_{i=1}^{D} x_i^* \odot \mathbf{e}_i
$$

The values $x_i^*$ are the coordinates of $x$ with respect to the given basis. 


---


## Notes on the Isomorphism 

For two vector spaces $U$ and $V$, two spaces can be **considered equally** for defined metric $d_V$ and d_U$ when 
1. **There exists an isomorphism** $f:U \rightarrow V$. That is, $f( \mathbf{x} \oplus_U \mathbf{y}) = f( \mathbf{x}) \oplus_V f(\mathbf{y})$ for bijective function $f$ where $\oplus_U$ and $\oplus_V$ are vector additions (operations) in spaces $U$ and $V$ respectively.    
2. The isomorphism $f$ is also **isometric** (note that it requires additional assumption of metric spaces for $U$ and $V$)


$$
d_V(f(\mathbf{x}), f(\mathbf{y})) = d_U(\mathbf{x}, \mathbf{y}) \quad \text{for all} ~~ \mathbf{x}, \mathbf{y} \in U
$$


---


## Riesz Isomorphism 


Let $H$ be a Hilbert space whose inner product $\langle x,y \rangle$ is linear. 
For every continuous linear functional $\phi \in H^*$, there exists a unique vector $f_\phi \in H$, called the Riesz representation of $\phi$, such that 

$$ 
\phi(x) = \langle x, f_\phi \rangle  \quad \text{for all } x \in H
$$

<blockquote markdown="1">
Used in the paper: *The Linear Representation Hypothesis and the Geometry of Large Language Models*.

---

Each **concept variable** $W$ defines a set of counterfactual outputs

$$\{Y (W = w)\}$$ such as $$\{ king, queen, man, woman, \cdots \}$$


The **Embedding Representation** 

* Embedding representation $\bar{\gamma}_W$ of a concept $W$ if we have $\lambda_1 - \lambda_0 \in  \text{Cone}(\bar{\lambda}_W)$ for any context embeddings $\lambda_0, \lambda_1 \in \Lambda$ that satisfy 

$$
\frac{P(W=1 \vert \lambda_1)}{P(W=1 \vert \lambda_0)} > 1 \quad \text{and} \quad 
\frac{P(W, Z \vert \lambda_1)}{P(W, Z \vert \lambda_0)} = 
\frac{P(W \vert \lambda_1)}{P(W \vert \lambda_0)}
$$

for each concept $Z$ that is causally separable with $W$. 

* $\lambda_0 = \lambda(\text{He is the monarch of England})$ 
* $\lambda_1 = \lambda((\text{She is the monarch of England})$


$$P(W) \quad \text{for }  W \in \{0, 1\}$$ 

* $W=1$ : concept output  (king)
* $W=0$ : counterfactual output (queen)

The paper restrict to binary concepts. 


---

The **Unembedding Representation** is defined by the difference between two representations. 

* Unembedding representation $\bar{\gamma}_W$ of a concept $W$ if
    * $\gamma(Y(1)) - \gamma(Y(0)) \in \text{Cone}(\bar{\gamma}_W)$
    * $\gamma(king) - \gamma(queen)$
    * $Y(1) = king$,  $Y(0) = queen$ 

---


Consider Riesz isomorphism $\bar{\gamma} \mapsto \langle \bar{\gamma}, \cdot \rangle_C $

We can map $\bar{\Gamma}$ to $\Lambda$ by mapping each $\bar{\gamma}_W$ to a linear function according to 

$$\bar{\gamma}_W \mapsto \langle \bar{\gamma}_W, \cdot \rangle_C$$


This map sends each unembedding representation of a concept to the embedding representation of the same concept. 


</blockquote>