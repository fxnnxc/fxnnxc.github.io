---
layout: post
title: 'Kronecker Product'
date: 2023-04-06 11:12:00-0400
description: None
tags: LinearAlgebra 
category: LA
subcategory : Insertion
---


# Kronecker Product 

For matrix 
$$
A = \begin{pmatrix} a_{11} & \cdots & a_{1n} \\ \vdots & \ddots & \vdots \\ a_{m1} & \cdots & a_{mn} \end{pmatrix}
$$
Kronecker Product is defined by 

$$
A \otimes B  = \begin{pmatrix} a_{11} B & \cdots & a_{1n}B \\ \vdots & \ddots & \vdots \\ a_{m1}B & \cdots & a_{mn}B \end{pmatrix}
$$

For example, 

$$
\begin{gather}
A = \begin{pmatrix}
  1 & -1 
\end{pmatrix},
B = \begin{pmatrix}
  1 & 2 \\
  3 & 4  
\end{pmatrix} \\
A \otimes B = \begin{pmatrix}
  1 & 2 & -1 & -2 \\
  3 & 4 & -3 & -4
\end{pmatrix}
\end{gather}

$$



# Properties 

---

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Scalar Multiplication</tag>

$$
\Large{(kA) \otimes B = A \otimes (kB) = k (A\otimes B)}
$$

---

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Distributive</tag>

$$
\begin{gather*}
\Large{A \otimes (B+C)= (A \otimes B ) + (B \otimes C )} \\
\Large{(A+B) \otimes C = (A \otimes C ) + (B \otimes C )}
\end{gather*}
$$

---

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Associative</tag>


$$
\Large{(A \otimes B) \otimes C = A \otimes (B \otimes C )}
$$

---

<tag class="box-demo-link" style="background:#FFAAAA; color:#000000; font-size:18px">Mixed Product Property</tag>

$$
\Large{(A \otimes B ) \cdot (C \otimes D) = (A \cdot B) \otimes (C \cdot D)}
$$

---

<tag class="box-demo-link" style="background:#FFAAAA; color:#000000; font-size:18px">Hadamard Product</tag>

$$
\Large{(A \otimes B ) \odot (C \otimes D) = (A\odot B) \otimes (C\odot D)}
$$

---

<tag class="box-demo-link" style="background:#FF0000; color:#FFFFFF; font-size:18px">Commutative</tag> (Does not have it!)

In general, commutative property does not hold. 

$$
\Large{A \otimes B \ne B \otimes A}
$$

---

# Examples 


---

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Scalar Multiplication</tag>


$$
A = \begin{pmatrix}
  1 & -1 
\end{pmatrix},
B = \begin{pmatrix}
  1 & 2 \\
  3 & 4  
\end{pmatrix}, 
C = \begin{pmatrix}
  4 \\
  5   
\end{pmatrix}
$$


$$
\begin{gather}
(kA) \otimes B = 
\begin{pmatrix}
k & 2k &  -k  & -2k \\
3k & 4k & -3k & -4k 
\end{pmatrix} = k (A\otimes B) = A \otimes (kB)
\end{gather} 
$$

---

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Distributive</tag>



$$
A = \begin{pmatrix}
  1 & -1 
\end{pmatrix},
B = \begin{pmatrix}
  1 & 2 \\
  3 & 4  
\end{pmatrix}, 
C = \begin{pmatrix}
  5 & 6 \\
  7 & 8    
\end{pmatrix}
$$ 

$$
\begin{align*}
A \otimes (B + C) &= \begin{pmatrix} 
1 &  -1 
\end{pmatrix} \otimes \Big(
\begin{pmatrix}
  1 & 2 \\
  3 & 4    
\end{pmatrix} + 
\begin{pmatrix}
  5 & 6 \\
  7 & 8    
\end{pmatrix}
\Big) \\
&= \begin{pmatrix} 
1 &  -1 
\end{pmatrix} \otimes \Big(
\begin{pmatrix}
  6 & 8 \\
  10 & 12  
\end{pmatrix}
\Big) \\
&= \begin{pmatrix}
6 & 8 & -6 & -8 \\
10 & 12 & -10 & 12 \\
\end{pmatrix} \\
&= \begin{pmatrix} 
1 & 2 & -1 & -2 \\
3 & 4 & -3 & 4 \\
\end{pmatrix} + 
\begin{pmatrix}
5 & 6 & -5 & -6 \\
7 & 8 & -7 & 8 \\
\end{pmatrix} \\
&= \Big( \begin{pmatrix} 
1 & -1 
\end{pmatrix} \otimes \begin{pmatrix}
  1 & 2 \\
  3 & 4    
\end{pmatrix}\Big) + \Big( \begin{pmatrix} 
1 & -1 
\end{pmatrix} \otimes \begin{pmatrix}
  5 & 6 \\
  7 & 8    
\end{pmatrix}\Big) \\ 
&= (A\otimes B ) + (A \otimes C)
\end{align*}
$$

---

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Associative</tag>


$$
A = \begin{pmatrix}
  1 & -1 
\end{pmatrix},
B = \begin{pmatrix}
  1 & 2 \\
  3 & 4  
\end{pmatrix}, 
C = \begin{pmatrix}
  4 \\
  5   
\end{pmatrix}
$$


$$
\begin{align*}
A \otimes (B+C) &= 
\begin{pmatrix}
  1 & -1 
\end{pmatrix} \otimes \Big( \begin{pmatrix}
  1 & 2  \\
  3 & 4 
\end{pmatrix} 
\otimes
\begin{pmatrix}
4 \\
5
\end{pmatrix}\Big) \\
&= \begin{pmatrix}
  1 & -1 
\end{pmatrix} \otimes \begin{pmatrix}
  4 & 8  \\
  5 & 10 \\
  12 & 16 \\
  15 & 20
\end{pmatrix}\\ 
&= \begin{pmatrix}
  4 & 8 & -4 & -8  \\
  5 & 10 & -5 & -10 \\
  12 & 16 & -12 & -16 \\
  15 & 20 & -15 & -20
\end{pmatrix} \\
&=  \begin{pmatrix}
  1 & 2 & -1 & -2  \\
  3 & 4 & -3 & -4 
\end{pmatrix} \otimes \begin{pmatrix}
  4 \\
  5   
\end{pmatrix} \\
&=  \Big(\begin{pmatrix}
  1 & -1  \\
\end{pmatrix}  \otimes
\begin{pmatrix}
1 & 2 \\
3 & 4 
\end{pmatrix}\Big)
\otimes \begin{pmatrix}
  4 \\
  5   
\end{pmatrix} \\
&= (A \otimes B) \otimes C
\end{align*}
$$


---

<tag class="box-demo-link" style="background:#FFAAAA; color:#000000; font-size:18px">Mixed Product Property</tag>




$$
A = \begin{pmatrix} 1 & -1 \end{pmatrix},
B = \begin{pmatrix} 1 & 2 \end{pmatrix}, 
C = \begin{pmatrix} 3 \\ 4  \end{pmatrix},
D = \begin{pmatrix} 5 \\ 6 \end{pmatrix}
$$

* Warning $AC,BD$ matrix multiplications must be defined. 

$$
\begin{align*}
(A \otimes B ) \cdot (C \otimes D)  
&= \Big(\begin{pmatrix} 1 & -1 \end{pmatrix}  \otimes  \begin{pmatrix} 1 & 2 \end{pmatrix} \Big) \cdot
   \Big(\begin{pmatrix} 3 \\ 4 \end{pmatrix} \otimes  \begin{pmatrix} 5 \\ 6 \end{pmatrix}\Big) \\
&= \begin{pmatrix} 1 & 2 & -1 & -2  \end{pmatrix} \cdot \begin{pmatrix} 15 \\ 18 \\ 20 \\ 24  \end{pmatrix} \\
&= \begin{pmatrix} -17  \end{pmatrix} 
\end{align*}
$$

$$
\begin{align*}
(A \cdot C ) \otimes (B \cdot D)  
&= \Big(\begin{pmatrix} 1 & -1 \end{pmatrix}  \cdot  \begin{pmatrix} 3 \\ 4 \end{pmatrix} \Big) \otimes
   \Big(\begin{pmatrix} 1 & 2 \end{pmatrix} \cdot  \begin{pmatrix} 5 \\ 6 \end{pmatrix}\Big) \\
&= \begin{pmatrix} -1  \end{pmatrix} \otimes \begin{pmatrix} 17 \end{pmatrix} \\
&= \begin{pmatrix} -17  \end{pmatrix} 
\end{align*} 
$$

$$
\begin{gather*}
\Rightarrow (A \otimes B ) \cdot (C \otimes D) = (A \cdot B) \otimes (C \cdot D)
\end{gather*}
$$


---

<tag class="box-demo-link" style="background:#FFAAAA; color:#000000; font-size:18px">Hadamard Product</tag>

The size of ($A$ and $C$) and  ($B$ and $D$) must be equal respectively.  

$$
A = \begin{pmatrix}
  1 & -1 
\end{pmatrix},
B = \begin{pmatrix}
  1 \\ 2 
\end{pmatrix}, 
C = \begin{pmatrix}
  3 &
  4   
\end{pmatrix},
D = \begin{pmatrix}
  4 \\
  5   
\end{pmatrix}
$$


$$
\begin{align*}
(A \otimes B ) \odot (C \otimes D) 
&= 
\Big(  
 \begin{pmatrix} 1 & -1 \end{pmatrix} \otimes \begin{pmatrix} 1 \\ 2  \end{pmatrix}
 \odot 
 \begin{pmatrix} 3  & 4 \end{pmatrix} \otimes \begin{pmatrix} 4 \\ 5  \end{pmatrix}
 \Big) \\
&= 
\begin{pmatrix} 1 & -1 \\ 2 & -2 \end{pmatrix} 
\odot
\begin{pmatrix} 12 & 16 \\ 15 & 20 \end{pmatrix} \\
&=
\begin{pmatrix} 12 & -16 \\ 30 & -40 \end{pmatrix} 
\end{align*}
$$

$$
\begin{align*}
(A\odot C) \otimes (B\odot D)  
&= 
\Big(
\begin{pmatrix} 1 & -1 \end{pmatrix} \odot \begin{pmatrix} 3 & 4 \end{pmatrix}
\Big)\otimes
\Big(
\begin{pmatrix} 1 \\ 2 \end{pmatrix} \odot \begin{pmatrix} 4 \\ 5 \end{pmatrix}
\Big)\\
&= \begin{pmatrix} 3 & -4 \end{pmatrix} \otimes \begin{pmatrix} 4 \\ -10 \end{pmatrix} \\
&= \begin{pmatrix} 12 & -16 \\ -30 & -40 \end{pmatrix} 
\end{align*}

$$

$$
\Rightarrow (A \otimes B ) \odot (C \otimes D) = (A\odot B) \otimes (C\odot D)
$$



---

<tag class="box-demo-link" style="background:#FF0000; color:#FFFFFF; font-size:18px">Commutative</tag> (Does not have it!)

In general, commutative property does not hold. 


$$
A = \begin{pmatrix}
  1 & -1 
\end{pmatrix},
B = \begin{pmatrix}
  1 & 2 \\
  3 & 4  
\end{pmatrix}
$$


$$
\begin{gather*}
A \otimes B = \begin{pmatrix}
1 & -1 
\end{pmatrix}
\begin{pmatrix}
1 & 2 \\
3 & 4
\end{pmatrix} = \begin{pmatrix}
1 & 2 & -1 & -2 \\
3 & 4 & -3 & -4
\end{pmatrix}
 \\ 
B \otimes A = \begin{pmatrix}
1 & 2 \\
3 & 4
\end{pmatrix} \begin{pmatrix}
1 & -1 
\end{pmatrix} = \begin{pmatrix}
1 & -1 & 2 & -2 \\
3 & -3 & 4 & -4
\end{pmatrix} \\ 
A \otimes B \ne B \otimes A
\end{gather*}
$$

---

## Transpose 

$$
\Large{
(A \otimes B)^{\top} = A^{\top} \otimes B^{\top}
}
$$

$$
\Large{
(A \otimes B)^{*} = A^{*} \otimes B^{*}
}
$$

---

## Inverse 

Assume $A$ and $B$ are both invertible. 

$$
\Large{
(A \otimes B)^{-1} = A^{-1} \otimes B^{-1}
}
$$

## Moore–Penrose pseudoinverse 

$$
\Large{
(A \otimes B)^{+} = A^{+} \otimes B^{+}
}
$$




---
# References 

[1] https://en.wikipedia.org/wiki/Kronecker_product