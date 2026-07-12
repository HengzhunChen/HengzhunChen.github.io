---
layout: splash
author_profile: false
---

<h1> Eigenvalue Problem </h1>

<h2>Content</h2>

- [Preliminary](#preliminary)

    - [Definition](#definition)

    - [Perturbation theory](#perturbation-theory)

- [Direct Eigensolver](#direct-eigensolver)

    - [Power method](#power-method)

    - [Inverse power method](#inverse-power-method)

    - [Orthogonal iteration](#orthogonal-iteration)

    - [QR iteration](#qr-iteration)

- [Direct Symmetric Eigensolver](#direct-eigensolver)

    - [Divide-and-Conquer](#divide-and-conquer)

    - [Bisection and inverse iteration](#bisection-and-inverse-iteration)

    - [Jacobi's method](#jacobis-method)

- [Iterative Symmetric Eigensolver](#iterative-symmetric-eigensolver)

    - [Lanczos algorithm](#lanczos-algorithm)

------------------------------------------------------------------------

# Preliminary

## Definition

The polynomial $p(\lambda) = \det(A-\lambda I)$  is called the characteristic polynomial of $A$. The roots of $p(\lambda)=0$ are the eigenvalues of $A$.

A nonzero vector $x$ satisfying $Ax=\lambda x$ is a (right) eigenvector for the eigenvalue $\lambda$. 

A nonzero vector $y$ such that $y^\ast A = \lambda y^\ast$ is a left eigenvector.

A Jordan block has one right eigenvector $e_1=[1, 0, \ldots, 0]^T$, and one left eigenvector $e_n=[0, \ldots, 0, 1]^T$. Therefore, a matrix is diagonalizable if all the Jordan blocks are scalar, otherwise it is called defective. An $n$-by-$n$ defective matrix does not have $n$ eigenvectors.

For Jordan block $J_m(\lambda)$, if $m=1$, then $\lambda$ is called a simple eigenvalue. 

## Perturbation theory

Eigenvalues pf $A$ are continuous functions of $A$, even if they are not differentiable.

### Condition number

Let $\lambda$ be a simple eigenvalue of $A$ with normalized right eigenvector $x$ and normalized left eigenvector $y$. Let $\lambda + \delta \lambda$ be the corresponding eigenvalue of $A+\delta A$. Then

$$ |\delta \lambda| \leq \frac{1}{|y^*x|} \|\delta A\| + O(\|\delta A\|^2) = \sec \Theta(y, x) \|\delta A\| + O(\|\delta A\|^2),  $$

where $\Theta(y, x)$ is the acute angle between $y$ and $x$. And $\sec \Theta(y, x)=\frac{1}{\vert y^*x \vert}$ is the condition number of the eigenvalue $\lambda$.

NOTE: The condition number of Jordan block is $1 / \vert e_n^\ast e_1 \vert = \infty$.

### Pertubation theory for symmetric matrix

**Eigenvalue**:

Weyl's Theorem: Let $A$ and $E$ be $n$-by-$n$ symmetric matrices. Let $\alpha_1 \geq \cdots \geq \alpha_n$ be the eigenvalues of $A$ and $\hat{\alpha}_1 \geq \cdots \geq \hat{\alpha}_n$ be the eigenvalues of $A + E$. Then $\vert \alpha_i - \hat{\alpha}_i \vert \leq \vert\vert E \vert\vert_2$.

This theorem could be proved using Courant-Fischer minimax theorem.

**Eigenvector**:

Let $A=Q\Lambda Q^T = Q\text{diag}(\alpha_i)Q^T$ be an eigendecomposition of $A$. 
Let $A+E=\hat{A}=\hat{Q} \hat{\Lambda} \hat{Q}^T$ be the pertured eigendecomposition. 
Let $\theta$ denote the acute between $q_i$ and $\hat{q}_i$. 

Define eigen-gap as $\text{gap}(i,A)=\min_{j\neq i} \vert \alpha_j - \alpha_i \vert$. Then

$$ \frac{1}{2} \sin(2\theta) \leq \frac{\|E\|_2}{\text{gap}(i, A)}, \quad \text{provided gap}(i, A) >0.  $$

$$ \frac{1}{2} \sin(2\theta) \leq \frac{\|E\|_2}{\text{gap}(i, A+E)}, \quad \text{provided gap}(i, A+E) >0. $$

Therefore, we can not hope to get the individual eigenvectors accurately if we have a group of $k$ tightly clustered eigenvalues. However, it is possible to compute the $k$-dimensional invariant subspace spanned by these vectors quite accurately.

------------------------------------------------------------------------

# Direct Eigensolver

We call a method direct if experience shows that it (nearly) never fails to converge in a fixed number of iterations.

## Power method

$$ y_{i+1} = A x_i, \quad x_{i+1} = y_{i+1} / \| y_{i+1} \|_2, \quad \lambda_{i+1} = x_{i+1}^T A x_{i+1}.  $$

Note that multiplying a scalar will not change the direction of a vector.

Thus the space of $x_i$ is the same as $A^i x_0$, further, assume $\lambda_1$ is the largest eigenvalue with eigen-gap strictly greater than $0$,

$$ A^i x_0 = (S\Lambda^i S^{-1}) S S^{-1} x_0 
= (S\Lambda^i S^{-1}) S \begin{bmatrix} \xi_1 \\ \xi_2 \\ \vdots \\ \xi_n \end{bmatrix} 
= \xi_1 \lambda_1^i S \begin{bmatrix} 1 \\ \frac{\xi_2}{\xi_1} (\frac{\lambda_2}{\lambda_1})^i \\ \vdots \\ \frac{\xi_n}{\xi_1} (\frac{\lambda_n}{\lambda_1})^i \end{bmatrix}. $$

Thus, the direction of normalized vector will converge to $S e_1$, i.e., eigenvector corresponding to the largest eigenvalue.

## Inverse power method

With a shift $\sigma$, the method will converge to the eigenvalue closest to $\sigma$,

$$ y_{i+1} = (A - \sigma I)^{-1} x_i, \quad x_{i+1} = y_{i+1} / \| y_{i+1} \|_2, \quad \lambda_{i+1} = x_{i+1}^T A x_{i+1}.  $$


## Orthogonal Iteration

$$ Y_{i+1} = A Z_i, \quad \text{QR decomposition } Y_{i+1} \Rightarrow Z_{i+1} R_{i+1}.  $$

Note that if $col(X)=col(Y)$, then $col(AX)=col(AY)$. 

Thus, we have 

$$ span(Z_{i+1}) \xrightarrow{QR} span(Y_{i+1}) \xrightarrow{def} span(AZ_{i}) \xrightarrow{QR + col} span(AY_{i}) \xrightarrow{def} span(AAZ_{i-1}) \xrightarrow{\cdots} span(A^{i+1} Z_0). $$

Furthermore,

$$ A^k Z_0 = S\Lambda^k S^{-1} SS^{-1} Z_0 = S \Lambda^k C_0. $$

Assume $\Lambda_1^k$ and $C_{01}$ are invertible, then we have

$$ \Lambda^k C_0 = 
\begin{bmatrix} \Lambda_1^k & 0 \\ 0 & \Lambda_2^k \end{bmatrix}
\begin{bmatrix} C_{01} \\ C_{02} \end{bmatrix}
= \begin{bmatrix} \Lambda_1^k C_{01} \\ \Lambda_2^k C_{02} \end{bmatrix}
= \begin{bmatrix} I_p \\ \Lambda_2^k C_{02} C_{01}^{-1} \Lambda_1^{-k} \end{bmatrix} \Lambda_1^k C_{01}
:= \begin{bmatrix} I_p \\ E_k \end{bmatrix} D_k. $$

Note that right-multiplying an invertible matrix will not change the column space.

$$ span(A^k Z_0) = span(S \begin{bmatrix} I_p \\ E_k \end{bmatrix} D_k) = span(S \begin{bmatrix} I_p \\ E_k \end{bmatrix}). $$

Finally, Let $\mu_i = \lambda_{p+i}$ (the eigenvalues in $\Lambda_2$), and $\nu_j = \lambda_j$ (the eigenvalues in $\Lambda_1$), then

$$ (E_k)_{ij} = (C_{02} C_{01}^{-1})_{ij} \cdot \mu_i^k \nu_j^{-k} = (C_{02} C_{01}^{-1})_{ij} \cdot \left( \frac{\mu_i}{\nu_j} \right)^k. $$

Thus, $E_k \rightarrow 0$ when $k \rightarrow \infty$. This is the invariant subspace corresponding to the $p$ largest eigenvalues.

Q: what is the target of the algorithm ? 

A: eigen-subspace comes from $Z_i$, however, the eigenvalue should be computed from another quantity $Z_i ^T A Z_i$. Note that if $A$ is not symmetric, this quantity will only converge to the Schur form of $A$ under some conditions.

Q: how to we obtain eigenvectors ?

A: The algorithm converges to the Schur form $Q^\ast A Q = T$. Then if $Tx=\lambda x$, we have $AQ=QTx=\lambda Qx$, $Qx$ is an eigenvector of $A$. Thus we only need to find eigenvectors of a block upper triangle matrix, i.e., solve some triangluar linear systems.

## QR Iteration

To incorporate shifting and inverting, and simplify the computation of eigenvalues, reorganize the orthogonal iteration as

$$ \text{QR decomposition } A_{i} \Rightarrow Q_{i} R_{i}, \quad A_{i+1} = R_i Q_i.  $$

A simplified expression of iteration is

$$ A_{i+1} = R_i Q_i = Q_i^T (Q_i R_i) Q_i = Q_i^T A_i Q_i. $$

One could prove the under some condition $A_i$ is identical to the matrix $Z_i^T A Z_i$ implicitly computed by orthogonal iteration and thus converges to Schur form.

To make QR iteration practical, we need to solve the following issues:

- QR decomposition is too expensive. Can we seek some cheaper structure throughout iteration?

- How do we recognize convergence?

- How shall we choose shift $\sigma_i$ to accelerate convergence?

### Hessenberg Reduction

Hessenber form: matrix $A$ is zero below the first subdiagonal. An upper Hessenberg matrix is unreduced if all subdiagonals are nonzero.

NOTE: Hessenberg form is the maximal zero structure preserved by QR iteration.

**Fact 1:** QR decomposition of a Hessenberg matrix is cheap ($O(n^2)$ instead of $O(n^3)$)

**Fact 2:** If you start with Hessenberg, QR iteration keeps it Hessenberg! This means the expensive step becomes cheap forever.

**Fact 3 (Implicit Q Theorem):** The orthogonal matrix $Q$ such that $Q^T A Q = H$ is unreduced upper Hessenberg is uniquely determined by its first column (up to signs).

If we initially reduce the matrix to uppper Hessenberg form, the QR iteration preserved the Hessenberg form. 

Furthermore, since our target is only $A_{i+1}= Q_i^T A_i Q_i$, we don't need to explicitly implement the QR decomposition and matrix multiplication, instead, the uniqueness of Hessenberg transformation tells us that we only need to design the first column and then find Givens rotations to make the resulted matrix upper Hessenberg. 

The first column of $Q_i$ is proportional to be the first column of $A_i-\sigma_i I$ since this is the first column of Q in the QR decomposition of $A_i -\sigma_i I$.

### Convergence condition

Since $A_i$ is related to $A$ by a similarity transformation by an orthogonal matrix, we expect $A_i$ to have  roundoff errors of size $O(\varepsilon \vert\vert A \vert\vert)$.

Thus, nay subdiagonal entry of $A_i$ smaller than $O(\varepsilon \vert\vert A \vert\vert)$ in magnitude could be set to zero.

### Implicit shift

AS QR iteration converges, $A_i$ becomes upper triangular, and diagonal entries approach eigenvalues. The last row will be zero except for the eigenvalue in $(n,n)$ position. 

**Types of shift**

- bottom-right entry shift: $\sigma_i = A_i(n, n)$

- Wilkinson shift: choose the eigenvalue of the bottom-right 2-by-2 block that is closer to $A_i(n, n)$

- double shift: choose $\sigma_i$ as one eigenvalue of the bottom-right 2-by-2 block, perform QR step with $\sigma_i$ and its conjugate $\bar{\sigma}_i$, which could be done in an implicit manner

**Deflation**

If $\sigma_i$ is an exact eigenvalue of $A_i$, then the QR iteration converges in one step and reduce to smaller eigenproblem. 

Once an eigenvalue converges at the bottom, we remove it and work on a smaller matrix.

NOTE: although the shift accelarate the convergence, there are tiny sets of matrices where the algorithm failed.

---------------------------------------------------------------------------

# Direct Symmetric Eigensolver

For symmetric matrix, there are always some simplification for general eigensolver and some new algorithms utilizing this property to give better efficiency and accuracy.

## Divide-and-Conquer

Given a tridiagonal symmetric matrix $T$, we try to reduce the problem into matrices in half of the size with a recursive manner.

$$ T = \begin{bmatrix} \hat{T}_1 & \\ & \hat{T}_2 \end{bmatrix} 
+ \begin{bmatrix} 0 &  &  &  \\  & 0 & b_m &  \\  & b_m & 0 &  \\  &  &  & 0 \end{bmatrix} 
= \begin{bmatrix} T_1 & \\ & T_2 \end{bmatrix}  
+ \begin{bmatrix} 0 &  &  &  \\  & b_m & b_m &  \\  & b_m & b_m &  \\  &  &  & 0 \end{bmatrix}
= \begin{bmatrix} T_1 & \\ & T_2 \end{bmatrix} + b_m v v^T, 
\quad v = \begin{bmatrix} e_m \\ e_1 \end{bmatrix}
% \quad v = \begin{bmatrix} 0 \\ \vdots \\ 0 \\ 1 \\ 1 \\ 0 \\ \vdots \\ 0 \end{bmatrix}. $$

Suppose we have obtained eigen-decomposition of $T_1$ and $T_2$ as $T_i = Q_i \Lambda_i Q_i^T$ for $i=1,2$. Then we have

$$ 
\begin{align*}
T &= \begin{bmatrix} T_1 & \\ & T_2 \end{bmatrix} + b_m v v^T 
= \begin{bmatrix} Q_1 & \\ & Q_2 \end{bmatrix} 
\left( \begin{bmatrix} \Lambda_1 & \\ & \Lambda_2 \end{bmatrix} + b_m u u^T \right)
\begin{bmatrix} Q_1^T & \\ & Q_2^T \end{bmatrix} \\
&= \begin{bmatrix} Q_1 & \\ & Q_2 \end{bmatrix} Q \Lambda Q^T
\begin{bmatrix} Q_1^T & \\ & Q_2^T \end{bmatrix} 
\end{align*}
$$

where

$$ u = \begin{bmatrix} Q_1^T & \\ & Q_2^T \end{bmatrix} v. $$

Hence, we only need to consider eigendecomposition of $D+\rho uu^T$.

### Eigenvalues

For eigenvalues, using $\det(I+xy^T)=1+y^Tx$, we have the secular equation

$$ \det(D + \rho uu^T - \lambda I)= \det(D - \lambda I) \det(I + \rho(D - \lambda I)^{-1}uu^T) = 1+ \rho\sum_{i=1}^n \frac{u_i^2}{d_i - \lambda} := f(\lambda). $$

Here we assume that $D-\lambda I$ is nonsingular. This is because for $(D + \rho uu^T)q=\lambda q$ with $Dq = \lambda q$,

- if $u^T q=0$, then $u_j=0$,
- if $u^T q \neq 0$, then we have $\lambda=d_j$ and further $u_j=0$,

both cases gives deflation for the secular equation, i.e., the number of roots need to solved become less. If $u_i=0$, then the corresponding eigenvector is $e_i$.

For solving the secular equation, we use the Newton's method. Since $f(\lambda)$ is not well approximated by a straight line $l(x)$, we approximate it by another simple function $h(x)$. 

Given the approximated zero $\lambda_j$ (note that this is not the notation for exact eigenvalue), we construct 

$$ \frac{c_1}{d_i-\lambda} + \frac{c_2}{d_{i+1} - \lambda} + c_3 = h(\lambda) \approx f(\lambda) = 1+ \rho\sum_{k=1}^n \frac{u_k^2}{d_k - \lambda}. $$

Let

$$ f(\lambda) = 1+ \rho\sum_{k=1}^i \frac{u_k^2}{d_k - \lambda} + \rho\sum_{k=i+1}^n \frac{u_k^2}{d_k - \lambda} := 1 + \psi_1(\lambda) + \psi_2(\lambda), $$

and

$$ h(\lambda) = 1 + \hat{c}_1 + \frac{c_1}{d_i-\lambda} + \hat{c}_2  +\frac{c_2}{d_{i+1} - \lambda} := 1 + h_1(\lambda) + h_2(\lambda), $$

then we ask

$$ h_1(\lambda_j) = \psi_1(\lambda_j), \, h_1'(\lambda_j) = \psi_1'(\lambda_j), \quad h_2(\lambda_j) = \psi_2(\lambda_j), \, h_2'(\lambda_j) = \psi_2'(\lambda_j). $$

### Eigenvectors

If $\alpha$ is an eigenvalue of $D+\rho uu^T$, then $(D - \alpha I)^{-1} u$ is its eigenvectors.

However, this simple formula is not numerically stable. If $\alpha_i$ and $\alpha_{i+1}$ are close, then $(D - \alpha_i I)^{-1} u$ and $(D - \alpha_{i+1} I)^{-1} u$ will lose orthogonality.

To maintain the orthogonality between vectors is a difficult issue (recall the Gram-Schmidt process), some alternative methods have been developed for stable computation of eigenvectors.

## Bisection and Inverse Iteration

The Sylvester's inertia theorem asserts that for nonsingular $X$,

$$\text{Inertia}(A)=\text{Inertia}(X^T A X). $$

By Gaussian elimination, 

$$ \text{Inertia}(A - zI) = \text{Inertia}(LDL^T) = \text{Inertia}(D). $$

Thus, we can count the number of eigenvalues in different signs from the sign of diagonal elements in $D$.

Define 

$$ \text{Negcount}(A, z) = \# \text{eigenvalues of } A < z, $$

which is simple and turns out to be stale to compute for tridiagonal $A$.

Suppose $z_1 < z_2$ , then the number of eigenvalues in the interval $[z_1, z_2)$ could be computed by 

$$\text{Negcount}(A, z_2) - \text{Negcount}(A, z_1).$$

Combined with bisection algorithm, we can find all eigenvalues of $A$ insides $[a,b)$ to a given tolerance.

Once we have computed the eigenvalues, we use inverse iteration with shifts to compute eigenvectors. For orthogonality, one may perform a QR decomposition to reorthogonalize the computed eigenvectors.

## Jacobi's method

Jabobi's method works on the original dense matrix and is usually much slower than the previous method. But it can sometimes compute tiny eigenvalues and their eigenvectors with much higher accuracy than the previous methods and can be easily parallelized.

We define the Jacobi rotation $J_i=R(j,k,\theta)$ as Givens rotation embedding into identity matrix aligning with the dimension of the matrix $A$.

We will make $J^T A J$ nearly diagonal by iteratively choosing $J_i$ to make one pair of offdiagonal entries of $A_{i+1}=J_i^T A_i J_i$ zero at a time, i.e.,

$$ \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}^T
\begin{bmatrix} a^{(i)}_{jj} & a^{(i)}_{jk} \\ a^{(i)}_{kj} & a^{(i)}_{kk} \end{bmatrix}
\begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}
= \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix}.
$$

For each rotation step fixing index pair $(j,k)$, we have

$$ \text{off}(A) := \sqrt{\sum_{1\leq j' < k' \leq n} a_{j'k'}^2}, \quad \text{off}^2(A) = S^2 + a_{jk}^2. $$

Note that we have zero out $a_{jk}$, thus

$$ \text{off}^2(\hat{A}) = \hat{S}^2 + \hat{a}_{jk}^2 = \hat{S}^2. $$

By the design of $J_i=R(j,k,\theta)$, we know that 

$$ (a^{(i)}_{jj})^2 + (a^{(i)}_{jk})^2 + (a^{(i)}_{kj})^2 + (a^{(i)}_{kk})^2 = (a^{(i+1)}_{jj})^2 + (a^{(i+1)}_{kk})^2. $$

Since orthogonal rotation will not change the Frobenius norm and the sum-of-square of the four cross-term are the same, we have

$$ \hat{S}^2 = S^2 \Rightarrow \text{off}^2(\hat{A}) = \text{off}^2(A) - a_{jk}^2 $$

The Jacobi method will eventually converge to the diagonal form and gives the desired eigenpairs.

---------------------------------------------------------------------------

# Iterative Symmetric Eigensolver

## Lanczos Algorithm

Suppose $A=A^T$ is a symmetric matrix.

Starting with a given $x_1$, the $k-1$ iteration of either power method or inverse iteration produce a sequence of vectors $x_1, \ldots, x_k$. These vectors span a Krylov subspace, defined as

$$ \mathcal{K}_k(x_1, A) =\text{span}[x_1, Ax_1, A^2x_1, \ldots, A^{k-1}x_1]. $$

Rather than taking $x_k$ as our approximate eigenvector, it is natural to ask for the "best" approximate eigenvector in $\mathcal{K}_k$. These best approximations are then called Ritz values and Ritz vectors.

### Rayleigh-Ritz method

Given a partial unitary matrix $Q_k \in \mathbb{R}^{n\times k}$ such that $Q_k^T Q_k = I_k$, we approximate the eigenvalues of matrix $A$ by the eigenvalues of 

$$ T_k = Q_k^T A Q_k. $$

The approximations are called Ritz values. 

Let $T_k = V \Lambda V^T$ be the eigendecomposition of $T_k$, the eigenvector approximations are given by $Q_k V$ and are called Ritz vectors.

The Ritz values and Ritz vectors are "optimal" approximation in the sense that it minimizes the residual 

$$ \|AQ_k - Q_k R\|_2, \quad \text{for all } R=R^T, $$

or equivalent, let $P_k$ ranges all partial orthogonal matrix such that $\text{span}(P_k) = \text{span}(Q_k)$, it minimizes

$$ \|AP_k - P_k D\|_2, \quad \text{for all diagonal } D, P_k^T P_k = I_k,  \text{span}(P_k) = \text{span}(Q_k). $$

### Lanczos Eigensolver

When applying the Rayleigh-Ritz method with $Q_k$ computed by the Lanczos algorithm, the resulted 

$$ T = \begin{bmatrix} Q_k & Q_u \end{bmatrix}^T A \begin{bmatrix} Q_k & Q_u \end{bmatrix} $$

is a tridiagonal matrix and hence easy to compute the approximated Ritz values, Ritz vectors from $T_k = Q_k^T A Q_k$ and residuals.

One of the possible issues is that the lose of orthogonality between eigenvectors. A careful implementation with more cost is called Lanczos with full reorthogonalization. 
