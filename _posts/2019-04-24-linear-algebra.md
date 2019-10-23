---
layout: post
title: Linear Algebra with Applications (Leon)
date: 2019-04-24
categories: [book, class]
tags: [mathematics]
author: "Max Kossek"
description: Summary of concepts in Linear Algebra and Steven Leon's Book.
sitemap:
    lastmod: 2019-10-22
---

<img style="float: right; width: 25%; margin: 0 1rem;" src="/assets/images/book-covers/linearalgebra-cover.jpg" alt="Linear Algebra with Applications Book Cover">
Linear Algebra with Applications is a linear algebra textbook focused on engineering applications. The book starts with an introduction to the concept of matrices. Matrix operations, determinants and similar transformation techniques are discussed. The middle of the book focuses on vector spaces and how they can be transformed. Eigenvalues and eigenvectors appear towards the end of the book.

<div class="toc">
<strong>Table of Contents:</strong>
<ol>
<li><a href="#1-matrices-and-systems-of-equations">Matrices and Systems of Equations</a></li>
<li><a href="#2-determinants">Determinants</a></li>
<li><a href="#3-vector-spaces">Vector Spaces</a></li>
<li><a href="#4-linear-transformations">Linear Transformations</a></li>
<li><a href="#5-orthogonality">Orthogonality</a></li>
<li><a href="#6-eigenvalues">Eigenvalues</a></li>
</ol>
</div>

## 1 Matrices and Systems of Equations

### 1.1 Systems of Linear Equations
A linear equation in n unknowns: a<sub>1</sub>x<sub>1</sub> + a<sub>2</sub>x<sub>2</sub> + ... a<sub>n</sub>x<sub>n</sub> = b.[^book] Linear systems are either consistent (1 or infinite solutions) or inconsistent (0 solutions). An augmented matrix includes the solution set on the right hand side of the matrix (A | B). There are three main matrix row operations:

1. Interchange two rows (swap)
2. Multiply by a non zero number α
3. Add or subtract a multiple of one row with another row.  


### 1.2 Row Echelon Form
Lead variables are nonzero elements in each row, free variables are variables skipped in the reduction process. Conditions for Row Echelon Form:

1. First nonzero entry in each nonzero row is 1.
2. If row k is not all zero, then the number of leading zeros in row k+1 is greater than the leading number of zeros in row k.
3. All zero rows are beneath other rows.

```
{ {1, 4, 2}, {0, 1, 3}, {0, 0, 1} }
```

Gaussian elimination is the name of the process to reduce to row echelon form. Overdetermined systems have more equations than unknowns. Underdetermined systems have more unknowns than equations. The conditions for a matrix to be in reduced row echelon form are:
1. The matrix is in row echelon form.
2. The first nonzero entry is the only nonzero number in it's column.


### 1.3 Matrix Arithmetic 
Matrix notation { {a<sub>11</sub>, a<sub>12</sub>, ..., a<sub>1n</sub>}, ..., { a<sub>m1</sub>, a<sub>m2</sub>, ..., a<sub>mn</sub> } }. Scalar multiplication multiplies each number in a matrix by some number α. Matrix Addition adds each entry of two matrices A + B = C. A (mxn)\*(nxr) matrix multiplication has resulting matrix (mxr). When multiplying two matrices, the inner dimensions must always match. Matrix multiplication is not commutative: AB != BA.
```
# Multiplying two matrices with dimensions 2x2 * 2x1 yields a 2x1 matrix
{ {1, 2}, {7, 1} } * { {3}, {4} } = { {11}, {25} }

1(3) + 2(4) = 11
7(3) + 1(4) = 25
```

A linear system `Ax = b` is consistent if and only if can be written as a linear combination (c<sub>1</sub>a<sub>1</sub> + ... c<sub>n</sub>a<sub>n</sub>). The transpose of a matrix is the results of interchanging the row vectors with the column vectors, so that a<sub>ij</sub> = a<sub>ji</sub><sup>T</sup>.


### 1.4 Matrix Algebra 
The identity matrix is the matrix that when multiplied with another matrix A, has a result that is the same as matrix A (IA = AI = A). Identity matrices have 1's on the diagonal, and 0's everywhere else `{ {1, 0, 0}, {0, 1, 0}, {0, 0, 1} }`. A matrix is nonsingular if there exists another matrix such that AB = BA = I. A matrix is singular if it doesn't have an inverse matrix B. 


### 1.5 Elementary Matrices
Elementary matrices are the result of applying exactly one elementary row operation to the identity matrix. An elementary matrix is always nonsingular. An upper triangular matrix has only non-zero entries at or above the diagonal. A lower triangular matrix has only non-zero entries at or below the diagonal. We can use the LU factorization of A, such that LU = A. A diagonal matrix has all zero entries except on the diagonal.


## 2 Determinants

### 2.1 Determinant of a Matrix
For a 2x2 matrix the determinant is calculated by a<sub>11</sub>a<sub>22</sub> - a<sub>21</sub>a<sub>12</sub>. The det(A<sup>T</sup>) is equal to det(A). If A is a nxn triangular matrix, then det(A) is the product of the diagonal elements. If the nxn matrix A has a all zero row or column, or two identical rows or columns then det(A) = 0.
```
{ {5, 1}, {3, 4} }
det = 5(4) - 1(3) = 17

{ {2, 5, 4}, {3, 1, 2}, {5, 4, 6} }
det = 2(6-8) - 5(18-10) + 4(12-5) = -16
```


### 2.2 Properties of Determinants
A nxn matrix is singular if and only if det(A) = 0. det(AB) = det(A)det(B). Row operations and their effect on determinants:

Row Operation | Effect on det(A)
--- | ---
I (interchanging rows) | det(EA) = -det(A)
II (multiplying by scalar α) | det(EA) = αdet(A)
III (add/sub multiple of another row) | det(EA) = det(A)



## 3 Vector Spaces

### 3.1 Definitions & Examples
Standard Euclidean R<sup>n</sup> space properties:
```
αx = { {αx1}, {αx2}, ..., {αxn} }
x+y = { {x1+y1}, {x2+y2}, ..., {xn+yn} }
```

If V is a vector space and x is an element of V then:
1. 0x = 0
2. x + y = 0 implies y = -x
3. (-1)x = -x


### 3.2 Subspaces
S is a subspace of V, if αx∈S and x+y∈S, when x&y∈S. x is in the null space of a matrix N(A) if:
1. A(αx) = αAx = α0 = 0
2. A(x+y) = Ax+Ay = 0+0 = 0

The set of all linear combinations of vectors v<sub>1</sub>,... v<sub>n</sub> in the vector space V is called the span.


### 3.3 Linear Independence
If a span can be written as a linear combination of n-1 elements, then n-1 elements are the span. Vectors are linearly independent if they form a minimal spanning set. For linearly independent sets, scalars must all be equal to 0 to satisfy c<sub>1</sub>v<sub>1</sub> + ... c<sub>n</sub>v<sub>n</sub> = 0. Vectors are linearly dependent if scalars exists such that c<sub>1</sub>v<sub>1</sub> + ... c<sub>n</sub>v<sub>n</sub> = 0.


### 3.4 Basis and Dimension
The basis is a set of vectors v<sub>1</sub>,...v<sub>n</sub> that are linearly independent and span the vector space. If the vector space V has a basis with n vectors, then it is said to be n-dimensional.


### 3.5 Change of Basis
Standard basis in R<sup>2</sup> is [e1,e2], any vector x in R<sup>2</sup> can be written as x = x<sub>1</sub>e<sub>1</sub> + x<sub>2</sub>e<sub>2</sub>. To switch bases from [u1,u2] to [e1,e2], we must express the old basis elements u1 & u2 in terms of e1 & e2. We use the transition matrix U to find the corresponding vector x, x = Uc.


### 3.6 Row Space and Column Space
The column space is the subspace of R<sup>m</sup> spanned by the column vectors of an mxn matrix. The row space is the subspace R<sup>1xn</sup> spanned by the row vectors. The rank of a matrix is the dimension of the row space of the matrix. To find the rank, we reduce to row echelon form; the rank will be all the nonzero rows. To find the basis for a column space, we reduce to row echelon form; the columns of the original matrix that are nonzero in the REF form will be the basis. If we have a mxn matrix, then the rank plus the nullity of the matrix equals n.


## 4 Linear Transformations

### 4.1 Definitions and Examples
Mapping L from the vector space V to vector space W `L: L->W` is a linear transformation if L(αv<sub>1</sub> + βv<sub>1</sub>) = αL(v<sub>1</sub>) + βL(v<sub>2</sub>) for all vectors in the vectors space and all scalars α,β. If L: V-> W:

1. L(Ov) = 0w
2. L(αv<sub>1</sub> + ... αv<sub>n</sub>) = α<sub>1</sub>L(v<sub>1</sub>) + ... α<sub>n</sub>L(v<sub>n</sub>)
3. L(-v) = -L(v) for all v in the vector space

The kernel when L: V-\>W is ker(L) = {v∈V \| L(v) = 0<sub>w</sub>}. The image of a linear transformation is L(s) = {w∈W \| w = L(v) for some v∈S}. The image of the entire vector space L(v) is called the range of L.


### 4.2 Matrix Representations of Linear Transformations
If L is a linear transformation from R<sup>n</sup>-\>R<sup>m</sup> then there is an mxn matrix such that L(x) = Ax.


### 4.3 Similarity
Two matrices A & B are similar if they have a nonsingular matrix S such that B = S<sup>-1</sup>AS. Matrices A and B are similar if:

1. B is a matrix representing L with respect to {u1, u2}
2. A is a matrix representing L with respect to {e1, e2}
3. If U is the transition matrix for change of basis from {u1, u2} to {e1, e2} then B = U<sup>-1</sup>AU


## 5 Orthogonality

### 5.1 The Scalar Product in R<sup>n</sup>
The product of x<sup>T</sup>y is a 1x1 matrix (vector), also called a scalar product.
```
{ {3, -2, 1} } * { {4}, {3}, {2} } = 3(4) - 2(3) + 1(2) = 8
```

The euclidean length is: \|\|x\|\| = (x<sup>T</sup>x)<sup>1/2</sup>. If x and y are vectors in R<sup>2</sup> or R<sup>3</sup> then the distance is \|\|x - y\|\|. Unit vectors are equal to the vector divided by it's norm u = x / \|\|x\|\|. Two vectors are orthogonal if x<sup>T</sup>y = 0.

### 5.2 Orthogonal Subspaces
Two subspaces X and Y are orthogonal if x<sup>T</sup>y = 0 for every x, y. Fundamental subspaces theorem: If A mxn matrix, then N(A) = R(A<sup>T</sup>)<sup>⟂</sup> and N(A<sup>T</sup>) = R(A)<sup>⟂</sup>.


### 5.3 Least Squares Problem
The least squares of an overdetermined system is the minimization of the residual ||r(x)|| = ||b - Ax||. 


### 5.4 Inner Product Spaces
The inner product for a pair of vectors x & y is a real number satisfying:

1. \<x, x> >= 0
2. \<x, y> = \<y, x> for all x and y.
3. \<ax, by, z> = a\<x, z> + b\<y, z> for all x,y,z and all scalars a,b.

The standard inner product is <x,y> = x<sup>T</sup>y. The length or norm of an inner product is \|\|v\|\| = sqrt(<v, v>). If u and v are vectors in the inner product space of V, then:

1. Scalar Projection u-\>v: α = \<u,v> / \|\|v\|\|
2. Vector Projection u-\>v: p = α(v/\|\|v\|\|) = [v \* \<u,v>] / \<v, v>


### 5.5 Orthonormal Sets
An orthogonal set is when the nonzero vectors v<sub>1</sub>,... v<sub>n</sub> in the inner product space satisfy \<v<sub>i</sub>, v<sub>j</sub>> = 0 when i != j. The orthonormal set is the orthogonal set of unit vectors. A permutation matrix is the identity matrix with the columns reordered. If the column vectors of A form a orthonormal set in the R<sup>m</sup>, then A<sup>T</sup>A = I and the least squares solution is x = A<sup>T</sup>b.

### 5.6 QR Factorization
Gram Schmidt QR Factorization can be completed on a mxn matrix A such that QR = A. Q is a mxn orthonormal matrix, and R is the upper triangular nxn matrix with diagonal entries all positive.


## 6 Eigenvalues

### 6.1 Eigenvalues & Eigenvectors
If A is a nxn matrix, then the scalar λ is a eigenvalue if there exists a nonzero vector X such that Ax = λx, where x is the eigenvector. λ is a eigenvalue of A if and only if the equation det(A - λI) = 0 has a nontrivial solution. The eigenspace is the subspace N(A - λI). 


### 6.2 System of Linear Differential Equations
If λ is a eigenvalue of A and x is a eigenvector belonging to λ, then e<sup>λt</sup>x is a solution to the system Y' = AY.


### 6.3 Diagonalization
If λ<sub>1</sub>,... λ<sub>n</sub> are distinct eigenvalues of a nxn matrix, then eigenvectors x<sub>1</sub>,... x<sub>n</sub> are linearly independent. If nxn has a nonsingular matrix X and a diagonal matrix D such that X<sup>-1</sup>AX = D, then X diagonalizes A. A nxn matrix is defective if it has fewer than n linearly independent eigenvectors. Stochastic processes are a sequence of experiments where the outcome depends on change. A Markov process has properties:
1. The set of possible outcomes is finite.
2. The probability of the next outcome depends on the last outcome.
3. Probabilities are constant over time.


[^book]: Leon, S. J., Bica, I., & Hohn, T. (1980). Linear algebra with applications (pp. 283-313). New York: Macmillan.