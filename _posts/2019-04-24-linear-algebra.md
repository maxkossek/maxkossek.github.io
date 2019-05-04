---
layout: post
title: Linear Algebra with Applications (Leon)
date: 2019-04-24
categories: [book, class]
tags: [mathematics]
author: "Max Kossek"
description: Summary of concepts in Linear Algebra and Steven Leon's Book.
---

## 1 Matrices and Systems of Equations

### 1.1 Systems of Linear Equations
Linear equation in n unknowns a<sub>1</sub>x<sub>1</sub> + a<sub>2</sub>x<sub>2</sub> + ... a<sub>n</sub>x<sub>n</sub> = b.[^book] Linear system is either *consistent* (1 or infinite solutions) or *inconsistent* (0 solutions). Augmented matrix includes solution set on right hand side of matrix (A | B). Row operations:

1. Interchange two rows (swap)
2. Multiply by non zero number α
3. Add or subtract mutiple of one row with another row.  

### 1.2 Row Echelon Form
Lead variables are nonzero elements in each row, free variables are variables skipped in reduction process. Conditions for Row Echelon Form:

1. First nonzero entry in each nonzero row is 1
2. If row k is not all zero, then number of leading zero's in row k+1 > leading zeros in row k.
3. All-zero rows are beneath other rows.
```
{ {1, 4, 2}, {0, 1, 3}, {0, 0, 1} }
```

Gaussian elimination is name of process to reduce to row echelon form. Overdetermined system has more equations than unknowns. Underdetermined system has more unknowns than equations. 

Conditions for Reduced Row Echelon Form:

1. Row Echelon Form
2. First nonzero entry is the only nonzero number in it's column.


### 1.3 Matrix Arithmetic 
Matrix notation { {a<sub>11</sub>, a<sub>12</sub>, ..., a<sub>1n</sub>}, ..., { a<sub>m1</sub>, a<sub>m2</sub>, ..., a<sub>mn</sub> } }. Scalar multiplication multiplies each number in a matrix by some number α. Matrix Addition adds each entry of two matrices A + B = C. A (mxn)\*(nxr) matrix multiplication has resulting matrix (mxr). Inner dimension of the two matrices must always match. Matrix multiplication is not commutative AB != BA. For example, here we multiply matrices 2x**2** \* **2**x1 = 2x1.

```
{ {1, 2}, {7, 1} } \* { {3}, {4} } = { {11}, {25} }

1(3) + 2(4) = 11
7(3) + 1(4) = 25
```

Linear system `Ax = b` is consistent if and only if can be written as *linear combination* (c<sub>1</sub>a<sub>1</sub> + ... c<sub>n</sub>a<sub>n</sub>). The *transpose* of a matrix is when we make the row vectors the column vectors, so that a<sub>ij</sub> = a<sub>ji</sub><sup>T</sup>.


### 1.4 Matrix Algebra 
*Identity matrix* is the matrix that when multiplied with another matrix A, the result is the same matrix A (IA = AI = A). Identity matrix has 1's on the diagonal, and 0's everywhere else `{ {1, 0, 0}, {0, 1, 0}, {0, 0, 1} }`. A matrix is *nonsingular* if there exists another matrix such that AB = BA = I. A matrix is *singular* if it doesn't have inverse matrix B. 


### 1.5 Elementary Matrices
*Elementary matrix* is the result of applying exactly one elementary row operation to the identity matrix. An elementary matrix is always nonsingular. A *upper triangular* matrix has only non-zero entries in the at or above the diagonal. A *lower triangular* matrix only has non-zero entries at or below the diagonal. We can use a LU factorization of A, so that LU = A. A *diagonal* matrix has all zero entries except on the diagonal.


## 2 Determinants

### 2.1 Determinant of a Matrix
For a 2x2 matrix the determinant is calculated by a<sub>11</sub>a<sub>22</sub> - a<sub>21</sub>a<sub>12</sub>.
```
{ {5, 1}, {3, 4} }
det = 5(4) - 1(3) = 17

{ {2, 5, 4}, {3, 1, 2}, {5, 4, 6} }
det = 2(6-8) - 5(18-10) + 4(12-5) = -16
```

The det(A<sup>T</sup>) is equal to det(A). If A is nxn triangular matrix, then det(A) is the product of the diagonal elements. If nxn matrix A has all zero row or column, or two identical rows or columns then det(A) = 0.


### 2.2 Properties of Determinants
Row Operations effect on the determinant:

Row Operation | Effect on det(A)
--- | ---
I (interchanging rows) | det(EA) = -det(A)
II (multiply by scalar α) | det(EA) = αdet(A)
III (add/sub multiple of another row) | det(EA) = det(A)

A nxn matrix is singular if and only if det(A) = 0. det(AB) = det(A)det(B).


## 3 Vector Spaces

### 3.1 Definitions & Examples
Standard Euclidean R<sup>n</sup> space properties:
```
αx = { {αx1}, {αx2}, ..., {αxn} }
x+y = { {x1+y1}, {x2+y2}, ..., {xn+yn} }
```

If V is a vector space and x is an element of V:
1. 0x = 0
2. x + y = 0 implies y = -x
3. (-1)x = -x


### 3.2 Subspaces
S is a subspace of V, if αx∈S and x+y∈S, when x&y∈S. x is in the *null space* of a matrix N(A) if:
1. A(αx) = αAx = α0 = 0
2. A(x+y) = Ax+Ay = 0+0 = 0

The set of all linear combinations of vectors v<sub>1</sub>,... v<sub>n</sub> in vector space V is called the *span*.


### 3.3 Linear Independence
If a span can be written as a linear combination of n-1 elements, then n-1 elements are the span. Vectors are *linearly independent* if they form a minimal spanning set. For linearly independent set, scalars must all be equal to 0 to satisfy c<sub>1</sub>v<sub>1</sub> + ... c<sub>n</sub>v<sub>n</sub> = 0. Vectors are *linearly dependent* if scalars exists so that c<sub>1</sub>v<sub>1</sub> + ... c<sub>n</sub>v<sub>n</sub> = 0.


### 3.4 Basis and Dimension
*Basis* is a set of vectors v<sub>1</sub>,...v<sub>n</sub> that are linearly independent and span the vector space. If vector space V has basis with n vectors, then it is said to be n-dimensional.


### 3.5 Change of Basis
Standard basis in R<sup>2</sup> is [e1,e2], any vector x in R<sup>2</sup> can be written as x = x<sub>1</sub>e<sub>1</sub> + x<sub>2</sub>e<sub>2</sub>. To switch bases from [u1,u2] to [e1,e2], we must express the old basis elements u1 & u2 in terms of e1 & e2. Use *transition matrix* U to find corresponding vector x, x = Uc.


### 3.6 Row Space and Column Space
*Column space* is the subspace of R<sup>m</sup> spanned by the column vectors of an mxn matrix. *Row space* is the subspace R<sup>1xn</sup> spanned by the row vectors. The rank of a matrix is the dimension of the row space of the matrix. To find the rank reduce to Row Echelon Form, and it will be the nonzero rows. To find the basis for column space, reduce to Row Echelon Form, the columns of the original matrix that are nonzero in the REF form will be the basis. If mxn matrix, then rank plus the nullity of the matrix equals n.


## 4 Linear Transformations

### 4.1 Definitions and Examples
Mapping L from vector space V to vector space W `L: L->W` is a linear transformation if L(αv<sub>1</sub> + βv<sub>1</sub>) = αL(v<sub>1</sub>) + βL(v<sub>2</sub>) for all vectors in the vectors space and all scalars α,β. If L: V-> W:

1. L(Ov) = 0w
2. L(αv<sub>1</sub> + ... αv<sub>n</sub>) = α<sub>1</sub>L(v<sub>1</sub>) + ... α<sub>n</sub>L(v<sub>n</sub>)
3. L(-v) = -L(v) for all v in the vector space

The *kernel* when L: V-\>W is ker(L) = {v∈V \| L(v) = 0<sub>w</sub>}. *Image* of linear transformation is L(s) = {w∈W \| w = L(v) for some v∈S}. Image of the entire vector space L(v) is called the range of L.


### 4.2 Matrix Representations of Linear Transformations
If L is linear transformation from R<sup>n</sup>-\>R<sup>m</sup> then there is an mxn matrix such that L(x) = Ax.


### 4.3 Similarity
If
1. B is matrix representing L with respect to {u1, u2}
2. A is matrix representing L with respect to {e1, e2}
3. U is transition matrix for change of basis from {u1, u2} to {e1, e2}
then B = U<sup>-1</sup>AU. Two matrices A & B are similar if they have a nonsingular matrix S such that B = S<sup>-1</sup>AS.


## 5 Orthogonality

### 5.1 The Scalar Product in R<sup>n</sup>
Product of x<sup>T</sup>y is a 1x1 matrix (vector), also called scalar product.
```
{ {3, -2, 1} } * { {4}, {3}, {2} } = 3(4) - 2(3) + 1(2) = 8
```

<p>The euclidean length ||x|| = (x<sup>T</sup>x)<sup>1/2</sup>. If x and y vectors in R<sup>2</sup> or R<sup>3</sup> then the distance is ||x - y||. Unit vector is equal to the vector divided by it's norm u = x / ||x||. Two vectors are orthogonal if x<sup>T</sup>y = 0.</p>

### 5.2 Orthogonal Subspaces
Two subspaces X and Y are *orthogonal* if x<sup>T</sup>y = 0 for every x,y. Fundamental subspaces theorem: If A mxn matrix, then N(A) = R(A<sup>T</sup>)<sup>⟂</sup> and N(A<sup>T</sup>) = R(A)<sup>⟂</sup>.

### 5.3 Least Squares Problem
The least squares of overdetermined system is the minimation of the residual ||r(x)|| = ||b - Ax||. 


### 5.4 Inner Product Spaces
Inner product for pair of vectors x & y is a real number satisfying:
1. \<x, x> >= 0
2. \<x, y> = \<y, x> for all x and y.
3. \<ax, by, z> = a\<x, z> + b\<y, z> for all x,y,z and all scalars a,b.

<p>The standard inner product is \<x,y> = x<sup>T</sup>y. The length or norm of an inner product is \||v|| = sqrt(\<v, v>). If u and v vectors in inner product space of V, then:</p>
1. Scalar Projection u-\>v: α = \<u,v> / \|\|v\|\|
2. Vector Projection u-\>v: p = α(v/\|\|v\|\|) = [v \* \<u,v>] / \<v, v>


### 5.5 Orthonormal Sets
A *orthogonal set* is when nonzero vectors v<sub>1</sub>,... v<sub>n</sub> in the inner product spaces satisfy \<v<sub>i</sub>, v<sub>j</sub>> = 0 when i != j. The *orthonormal set* is the orthogonal set of unit vectors. A permutation matrix is the identity matrix with the columns reordered. If column vectors of A form a orthonormal set in the R<sup>m</sup>, then A<sup>T</sup>A = I and least squares solution is x = A<sup>T</sup>b.

### 5.6 QR Factorization
Gram Schmidt QR Factorization can be completed on mxn matrix A such that QR = A. Q is a mxn orthonormal matrix, and R is the upper triangular nxn matrix with diagonal entries all positive.


## 6 Eigenvalues

### 6.1 Eigenvalues & Eigenvectors
If A nxn matrix, the scalar λ is eigenvalue if there exists nonzero vector X such that Ax = λx, where x is the eigenvector. λ is a eigenvalue of A if and only if equation det(A - λI) = 0 has a nontrivial solution. Eigenspace is the subspace N(A - λI). 


### 6.2 System of Linear Differential Equations
If λ eigenvalue of A and x eigenvector belonging to λ, then e<sup>λt</sup>x is a solution to the system Y' = AY.


### 6.3 Diagonalization
If λ<sub>1</sub>,... λ<sub>n</sub> are distinct eigenvalues of a nxn matrix, then eigenvectors x<sub>1</sub>,... x<sub>n</sub> are linearly indepedent. If nxn has nonsingular matrix X and diagonal matrix D such that X<sup>-1</sup>AX = D, then X diagonalizes A. A nxn matrix is *defective* if it has fewer than n linearly indepedent eigenvectors. *Stochastic process* is a sequence of experiments where the outcome depends on change. A *Markov process* has properties:
1. Set of possible outcomes if finite
2. Probability of next outcome depends on the last outcome
3. Probabilities are constant over time


[^book]: Leon, S. J., Bica, I., & Hohn, T. (1980). Linear algebra with applications (pp. 283-313). New York: Macmillan.
