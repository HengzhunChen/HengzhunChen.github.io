---
layout: splash
author_profile: false
---

<h1> Basic Numerical Optimization </h1>

<h2>Content</h2>

- [Fundamentals of Unconstrained Optimization](#fundamentals-of-unconstrained-optimization)

    - [Basic Tool: Taylor's Theorem](#basic-tool-taylors-theorem)

    - [Overview of Algorithms](#overview-of-algorithms)

- [Line Search Methods](#line-search-methods)

    - [Step Length Selection](#step-length-selection)

    - [Convergence of Line Search Methods](#convergence-of-line-search-methods)

    - [Rate of convergence of Line Search Methods](#rate-of-convergence-of-line-search-methods)

- [Trust-Region Methods](#trust-region-methods)

    - [model function](#model-function)

    - [algorithms for subproblem](#algorithms-for-subproblem)

    - [Convergence of Trust-Region Methods](#convergence-of-trust-region-methods)

- [Conjugate Gradient Methods](#conjugate-gradient-methods)

    - [Linear CG method](#linear-cg-method)

    - [Nonlinear CG method](#nonlinear-cg-method)

- [Quasi-Newton Methods](#quasi-newton-methods)

    - [BFGS method (SR2)](#bfgs-method-sr2)

    - [SR1 Method](#sr1-method)

    - [The Broyden class](#the-broyden-class)

- [Large-Scale Newton-Type Methods](#large-scale-newton-type-methods)

    - [Inexact Newton Methods](#inexact-newton-methods)

    - [Limited-Memory Quasi-Newton Methods](#limited-memory-quasi-newton-methods)

- [Derivative-Free Optimization](#derivative-free-optimization)

    - [Model-based methods](#model-based-methods)

    - [Coordinate-search and pattern-search method](#coordinate-search-and-pattern-search-method)

    - [A conjugate-direction method](#a-conjugate-direction-method)

    - [Nelder-Mead method](#nelder-mead-method)

- [Least-Squares Problem](#least-squares-problem)

    - [Linear least-squares problems](#linear-least-squares-problems)

    - [Nonlinear least-squares problems](#nonlinear-least-squares-problems)

- [Nonlinear Equations](#nonlinear-equations)

    - [Local algorithms](#local-algorithms)

    - [Merit function algorithms](#merit-function-algorithms)

    - [Continuation/Homotopy methods](#continuationhomotopy-methods)    


- [Theory of Constrained Optimization](#theory-of-constrained-optimization)

    - [Motivation of first-order condition](#motivation-of-first-order-condition)

    - [First-order optimality conditions](#first-order-optimality-conditions)

    - [Second-order conditions](#second-order-conditions)

    - [Duality](#duality)

- [Linear Programming](#linear-programming)

    - [Optimality and duality](#optimality-and-duality)

    - [Geometry structure](#geometry-structure)

    - [The simplex method](#the-simplex-method)

    - [Interior-point method](#interior-point-methods)

    - [Comparison of simplex method and interior-point method](#comparison-of-simplex-method-and-interior-point-method)

- [Quadratic Programming](#quadratic-programming)

    - [Equality-constrained QP](#equality-constrained-qp)

    - [Inequality-constrained QP](#inequality-constrained-qp)

- [Penalty Methods](#penalty-methods)

    - [Quadratic penalty method](#quadratic-penalty-method)

    - [Nonsmooth penalty method](#nonsmooth-penalty-method)

    - [Augmented Lagrangian method](#augmented-lagrangian-method)

- [Active-set Method for Nonlinear Programming (SQP)](#active-set-method-for-nonlinear-programming-sqp)

    - [Motivation of SQP](#motivation-of-sqp)

    - [Algorithm development of SQP](#algorithm-development-of-sqp)

- [Interior-point Method for Nonlinear Programming](#interior-point-method-for-nonlinear-programming)

    - [Derivation of interior-point method](#derivation-of-interior-point-method)

    - [Algorihtm development of interior-point method](#algorihtm-development-of-interior-point-method)



-------------------------------------------------------------------

# Fundamentals of Unconstrained Optimization

## Basic Tool: Taylor's Theorem

The mathematical tool used to study minimizers of smooth functions is Taylor's theorem.

**Taylor's Theorem**

Suppose $f:\mathbb{R}^n \rightarrow \mathbb{R}$ is continuously differentiable and that $p\in \mathbb{R}^n$. Then we have that

$$ f(x+p) = f(x) + \nabla f(x+tp)^T p, $$

for some $t\in(0, 1)$. Moverover, if $f$ is twice continuously differentiable, we have that

$$ \nabla f(x+p) = \nabla f(x) + \int_0^1 \nabla^2 f(x+tp)p ~ dt, $$

and that 

$$ f(x+p) = f(x) + \nabla f(x)^T p + \frac{1}{2} p^T \nabla f(x+tp)p, $$

for some $t\in (0, 1)$.

## Overview of Algorithms

**Optimization strategies**

- line search method: choose direction and search for step size

- trust region method: construct a model function within a trusted region


**Search directions**

- steepest descent direction: cheap but slow convergence

- Newton direction: fast local convergence (quadratic) but expensive

- quasi-Newton direction: super-linear convergence, not that expensive

- conjugate gradient direction: effective but not super-linear 

**NOTE**: All search directions except conjugate gradients have an analogue in the trust-region framework.

-------------------------------------------------------------------

# Line Search Methods

The iteration is given by 

$$ x_{k+1} = x_k + \alpha_k p_k, $$

where $\alpha_k$ is step length and $p_k$ is descent direction.

## Step Length Selection

### Exact line search: 

Find global minimizer of $\phi(\alpha)=f(x_k + \alpha p_k)$ with $\alpha > 0$.

### Inexact line search:

Typical line search algorithms try out a sequence of canditate values of $\alpha$, stopping to accept one of these values when certain conditions are satisfied.

- Wolfe conditions:

    - Armijo condition: $\alpha_k$ should give sufficient decrease in the objective function $f$, i.e., the decrease should greater than a threshold

    $$ f(x_k+\alpha p_k) \leq f(x_k) + c_1 \alpha \nabla f_k^T p_k $$

    - Curvature condition: for those sufficient decrease steps, we should run out of all the possible decrease, i.e., $\phi^{\prime}(\alpha)$ is going to be zero or positive from negative.

    $$ \nabla f(x_k + \alpha p_k)^T p_k \geq c_2 \nabla f_k^T p_k, \quad \nabla f_k^T p_k < 0 $$

- Goldstein conditions: the step length should achieve sufficient decrease but not be too short

    $$ f(x_k) + (1-c) \alpha \nabla f_k^T p_k \leq  f(x_k+\alpha p_k) \leq f(x_k) + c \alpha \nabla f_k^T p_k $$


## Convergence of Line Search Methods

Using the angle $\theta_k$ between direction $p_k$ and the steepest descent direction $-\nabla f_k$ defined as

$$ \cos \theta_k = \frac{-\nabla f_k^T p_k}{\|\nabla f_k\| \|p_k\|}, $$

and the recursive relation between $f_k$ and $f_{k+1}$ with some conditions, we have the Zoutendijk condition for ``global convergence'':

$$ \sum_{k=0}^{\infty} \cos^2 \theta_k \|\nabla f_k \|^2 < \infty. $$

## Rate of convergence of Line Search Methods

The challenge is to design algorithms that incorporate both properties: good global convergence guarantees and a rapid rate of convergence.

- steepest descent: linear rate

- Newton method: quadratic local convergence rate

    methods to modify indefinite Hessian
    
    - eigenvalue modification
    - adding a multiply of the identity
    - modified Cholesky Factorization
    - modified symmetric indefinite factorization

- quasi-Newton method: superlinear local convergence rate

    NOTE: superlinear convergence rate can be attained when $B_k$ become increasingly accurate approximation to $\nabla^2 f(x^\ast)$ along the search direction, i.e.,

    $$ \lim_{k\rightarrow \infty} \frac{\|(B_k - \nabla^2 f(x^*))p_k\|} {\|p_k\|} = 0. $$

-----------------------------------------------------------------------------


# Trust-Region Methods

Trust-region methods define a region around the current iterative within which they trust the model to be an adequate representation of the objective function. And they choose the direction and length of the step simultaneously.

## model function

Given Taylor-series of $f$ around $x_k$

$$ f(x_k + p) = f_k + g_k^T p + \frac{1}{2} p^T \nabla^2 f(x_k + tp) p, $$

using an approximation $B_k$ to the Hessian, the subproblem is

$$ \min_{p\in\mathbb{R}^n} m_k(p) = f_k + g_k^T p + \frac{1}{2} p^T B_k p \quad \text{s.t. } \|p\| \leq \Delta_k. $$

To choose the trust-region radius $\Delta_k$, define the reduction ratio

$$ \rho_k = \frac{f(x_k) - f(x_k + p_k)}{m_k(0) - m_k(p_k)}. $$

If $\rho_k$ is negative, the step must be rejected, if $\rho_k$ is close to 1, the radius is expanded, if $\rho_k$ is positive but significantly smaller thatn 1, we do not alter the trust region.

## algorithms for subproblem

### Cauchy Point Calculation

The Cauchy point does not depend very strongly on the matrix $B_k$, which is used only in the calculation of step length to guarantee global convergence. It is just the minimizer of $m_k$ along the steepest descent direction $g_k$, subject to the trust-region bound, i.e., it finds the solution of the linear version of the trust-region subproblem.

Rapid convergence can be expected only if $B_k$ plays a role in determining the direction of the step as well as its length.

### Dogleg Method

This method can be used when $B_k$ is positive definite.

If the unconstrained minimizer of $m$, denoted as $p^B = -B^{-1}g$ is outside the trust-region, the dogleg method uses a path consisting of two line segment to approach $p^B$, i.e.,

$$ \tilde{p}(\tau) = \begin{cases} \tau p^U, & 0\leq \tau \leq 1 \\ p^U + (\tau - 1)(p^B - p^U), & 1 \leq \tau \leq 2 \end{cases} $$

where $p^U = -\frac{g^T g}{g^T B g} g$. Then it choose $p$ to minimize the model $m$ along this path, subject to the trust-region bound. The selection of $p^U$ ensures that $m$ is decreasing along the path.

### Two-dimensional Subspace Minimization

When $B$ is positive definite, the dogleg method stragy can be made slightly more sophistivated by widening the search for $p$ to the entire two-dimensional subspace spaned by $p^U$ and $p^B$, i.e.,

$$ \min_{p\in\mathbb{R}^n} m(p) = f + g^T p + \frac{1}{2} p^T B p \quad \text{s.t. } \|p\| \leq \Delta, p\in \text{span}[g, g^{-1}B]. $$

When $B$ has negative eigenvalues, the two-dimesional subspace is changed to 

$$ \text{span} [g, (B+\alpha I)^{-1}g], \quad \alpha \in (-\lambda_{\min}(B), -2\lambda_{\min}(B)). $$

### Iterative Solution

When the problem is relatively small, we can find a good approximation at the cost of a few factorization of matrix $B$. Using Lagrange multiplier method, the exact solution is

$$ p^*=-B^{-1}g, \|p^*\| \leq \Delta, \quad \text{or} \quad p(\lambda)=-(B+\lambda I)^{-1} g, \|p(\lambda)\| = \Delta. $$

For the second case, one could apply Newton method with some "approximation" to find the value of $\lambda$. This process is terminated after two or three iterations with a fairly looose approximation to the true solution.

## Convergence of Trust-Region Methods

- Global convergence

    With some conditions, one can prove the global convergence to stationary point with $\liminf_{k\rightarrow \infty} \vert\vert g_k \vert\vert = 0$ by contradiction.

- Local convergence: superlinear

    The (approximate) solution of the trust-region subproblem is well inside the trust region and becomes closer and closer to the true Newton step. Once could show that the sequence $\lbrace\Delta_k\rbrace$ is bounded away from zero and $\vert\vert p_k \vert\vert \rightarrow 0$. Hence, the trust-region constraint eventually becomes inactive in algorithms and the superlinear convergnce can be attained.

---------------------------------------------------------------------------------------------


# Conjugate Gradient Methods

CG methods are among the most unseful techniques for solving large linear systems of equations. And they can be adapted to solve nonlinear optimization problems. The key features of these algorithms are that they require no matrix storage and are faster than the steepest descent methods.

## Linear CG method

Linear CG method can be derived from the FOM (full orthogonlization method) for symmetric positive definite matrix. Here we derive in another approach starting from the conjugate directions.

### Conjugate direction methods

A set of nonzero vectors $\lbrace p_0, p_1, \cdots, p_l \rbrace$ is said to be conjugate with respect to the symmetric positive definite matrix $A$ if 

$$ p_i^T A p_j = 0, \quad \text{for all } i \neq j. $$

The conjugate direction method solves 

$$ \min \phi(x) = \frac{1}{2} x^T A x - b^T x, \quad x \in \mathbb{R}^n, $$

in $n$ steps by successively minimizing it along the individual directions in the conjugate set with exact line search.

The conjugate directions is a linearly independent set. When the Hessian matrix is diagonal, it actually minimizes the function by performing one-dimensional minimizations along the coordinate directions and each coordinate minimization correctly determines one of the components of the solution $x^\ast$.

**NOTE: There are many ways to choose the set of conjugate directions, each with different convergence rates and computational cost.**

### Linear conjuagte gradient method

The CG methjod is a conjugate direction method with a very special property: 

In generating its set of conjugate vectors, it can compute a new vector $p_k$ by using only the previous vector $p_{k-1}$.

This property relies on the fact that the first direction $p_0$ is the steepest direction $-r_0$. The result may not hold for other choices and $p_0$.

### Rate of convergence

When the distribution of the eigenvalues of $A$ has certain favorable features, the CG algorithm will identify the solution in many fewer than $n$ iterations. 

We can accelarate the CG method by transforming the linear system to improve the eigenvalue distribution of $A$, which is known as **preconditioning**.

A quantity for convergence rate of CD could be

$$ \min_{P_k} \max_{1\leq i \leq n} (1 + \lambda_i P_k(\lambda_i))^2. $$

For example, if $A$ has only $r$ distinct eigenvalues, then the CG method will terminate at the solution in at most $r$ iterations.

## Nonlinear CG method

### Algorithm

Nonlinear CG could be derived from linear CG by

- replace the step length $\alpha_k$ with a line search method

- replace the residual $r_k$ with the gradient of the nonlinear objective $f$

However, the gradient $\nabla f_k$ are not mutually orthogonal, which leads to different expressions of $\beta_k=\frac{r_{k+1}^T r_{k+1}}{r_k^T r_k}$, for examples,

- Fletcher-Reeves (FR): $\beta_{k+1} = \frac{\nabla f_{k+1}^T \nabla f_{k+1}}{\nabla f_{k}^T \nabla f_{k}}$

- Polak-Ribiere (PR): $\beta_{k+1} = \frac{\nabla f_{k+1}^T (\nabla f_{k+1} - \nabla f_k)}{\nabla f_{k}^T \nabla f_{k}}$

For FR method, we have to require the step length $\alpha_k$ satisfy the strong Wolfe conditions to ensure all directions $p_k$ are descent directions of objective $f$. However, this is no true for PR method.

Numerical experience indicates that PR tends to the more robust and efficient one.
 
### Global convergence

Nonlinear CG methods posses surprising, sometimes bizarre, convergence properties.

A weakness of FR methos is that if it generates a bad direction and a tiny step, then the next direction and next step are also likely to be poor.

A strategy is to use **restart** method.

PR method essntially performs a restart after it encounters a bad direction.

The periodically restarted CG has a subsequence performs like steepest desent method. Using Zoutendijk's theorem gives the global convergence.

For unrestarted CG, we can not show the limit of $\lbrace \nabla f_k \rbrace$ is zero by only the lower limit is zero by contradiction.

------------------------------------------------------------------------------------------


# Quasi-Newton Methods

The superlinear convergence rate has been discussed under some conditions over the approximation $B_k$ of the Hessian matrix. There are many different choices of such approximations, which leads to different numerical behaviours.

One of the key ingradients in quasi-Newton method is the **least-change** property: the Hessian of the model change as little as possible form one iteration to the next.

## BFGS method (SR2)

Instead of computing $B_k$ afresh at every iteration, we update it in a simple manner to account for the curvature measured during the most recent step. For the new iterate $x_{k+1}$ with the new quadratic model as 

$$ m_{k+1}(p) = f_{k+1} + \nabla f_{k+1}^T p + \frac{1}{2} p^T B_{k+1} p, $$

one reasonable requirement is that 

**the gradient of $m_{k+1}$ should match the gradient of the objective function $f$ at the latest two iterates $x_k$ and $x_{k+1}$.** 

The second conditions is automatically satisfied while the first condition leads to 

$$ \nabla m_{k+1}(-\alpha_k p_k) = \nabla f_{k+1} - \alpha_k B_{k+1} p_k = \nabla f_k, $$

which could be simplified as the **secant equation**:

$$ B_{k+1} s_k = y_k, \quad s_{k+1}:=x_{k+1} - x_k = \alpha_k p_k, \quad y_k := \nabla f_{k+1} - \nabla f_k. $$

The secant equation with the symmetric positive definite property of $B_{k+1}$ leads to the curvature condition:

$$ s_k^T y_k > 0. $$

This condition will be guaranteed to hold if we impose the Wolfe or strong Wolfe conditions on the line search.

To determine $B_{k+1}$ uniquely, we impose the additional condition that

$$ \min_B \| B - B_k\| \quad \text{s.t.} \quad B=B^T, Bs_k=y_k. $$

With the weighted Frobenius norm $\vert\vert A \vert\vert_W = \vert\vert W^{1/2}AW^{1/2} \vert\vert_F$, the unique solution is 

$$ (\text{DFP}) \quad B_{k+1} = (I - \rho_k y_k s_k^T) B_k (I - \rho_k s_k y_k^T) + \rho_k y_ky_k^T, \quad \rho_k = \frac{1}{y_k^T s_k}. $$

The inverse of $B_k$ defined as $H_k=B_k^{-1}$ is useful in the optimization algorithm. Hence the Sherman-Morrison-Woodbury formula we have 

$$ (\text{DFP}) \quad H_{k+1} = H_k - \frac{H_k y_k y_k^T H_k}{y_k^T H_k y_k} + \frac{s_ks_k^T}{y_k^Ts_k}. $$

If we directly consider $H_k$ instead of $B_k$ with

$$ \min_H \| H - H_k\| \quad \text{s.t.} \quad H=H^T, Hy_k=s_k. $$

The we have 

$$ (\text{BFGS}) \quad H_{k+1} = (I - \rho_k s_k y_k^T) H_k (I - \rho_k y_k s_k^T) + \rho_k s_ks_k^T, \quad \rho_k = \frac{1}{y_k^T s_k}. $$

Also, by Sherman-Morrison-Woodbury formula

$$ (\text{BFGS}) \quad B_{k+1} = B_k - \frac{B_k s_k s_k^T B_k}{s_k^T B_k s_k} + \frac{y_ky_k^T}{y_k^Ts_k}. $$

NOTE: It is reasonable to ask whether there are situations in which the updating formula can produce bad result and is there any hope of correcting it? 

These questions have been studied analytically and experimentally, and it is now known that the BFGS formula has very effective self-correcting properties, i.e., the Hessian approximation will tend to correct itself within a few steps.

## SR1 Method

Unlike the rank-two update formula, this symmetric-rank-1 (SR1) update does not guarantee that the updated matrix maintains positive definiteness. However, with the advent of trust-region methods, the SR1 updating formula has proved to be quite useful, and its ability to generate indefinite Hessian approximations can actually regarded as one of its chief advantages.

### Motivation of SR1 Method

Consider the general form of rank-1 update as

$$ B_{k+1} = B_k + \sigma v v^T, $$

where $\sigma$ is either $+1$ or $-1$, and $\sigma$ and $v$ are chosen such that $B_{k+1}$ satisfies the secant equation $y_k = B_{k+1}s_k$. 

Thus we have

$$ (\text{SR1}) \quad B_{k+1} = B_k + \frac{(y_k - B_k s_k)(y_k - B_k s_k)^T}{(y_k - B_k s_k)^T s_k}. $$

Also, by Sherman-Morrison-Woodbury formula

$$ (\text{SR1}) \quad H_{k+1} = H_k + \frac{(s_k - H_k y_k)(s_k - H_k y_k)^T}{(s_k - H_k y_k)^T s_k}. $$

In practice, SR1 performs well simply with skipping the update if the denominator is small, i.e., the update is applied only if 

$$ |s_k^T(y_k - B_k s_k)| \geq r \|s_k\| \|y_k - B_k s_k\|. $$

The condition $s_k^T(y_k - B_ks_k) \approx 0$ occurs infrequently in SR1, which is different with the BFGS case.

### Properties

When $f$ is quadratic, the secant equation is satisfied along all previous search directions, regardless of how the line search is performed. And the iterates will converge to the minimizer in at most $n$ steps, provided that $(s_k - H_k y_k)^T y_k \neq 0$ for all $k$, which is similar to the conjugate direction method.

## The Broyden class

The Broyden class is a family of updates takes the form

$$ B_{k+1} = (1- \phi_k) B_{k+1}^{\text{BFGS}} + \phi_k B_{k+1}^{\text{DFP}}. $$

This relation indicates that all members of the Broyden class satisfy the secant equation.

The restricted Broyden class is defined by restricting $\phi_k$ to the interval $[0,1]$.

NOTE: For strongly convex quadratic function, under some conditions $\phi_k$, with exact line search and $B_0=I$, the iterates are identical to those generated by the CG method. 

--------------------------------------------------------------------------

# Large-Scale Newton-Type Methods

For unconstrained optimization problems with thousands or millions of variables, the cost of factoring the Hessian is prohibitive, and it is preferable to compute approximation to the Newton step using iterative linear algebra techiques.

## Inexact Newton Methods

The basic Newton step $p_k$ is obtained by solving linear system

$$ \nabla^2 f_k p_k = - \nabla f_k. $$

We will try to solve it using the CG method or Lanczos method, with modifications to handle negative curvature in the Hessian $\nabla^2 f_k$.

### Local convergence of inexact Newton methods

We define the residual as 

$$ r_k = \nabla^2 f_k p_k + \nabla f_k. $$

Usually, we terminate the CG iterations when 

$$ \|r_k\| \leq \eta_k \|\nabla f_k \|, $$

where the "forcing sequence" $\eta_k = \min(0.5, \sqrt{\vert\vert\nabla f_k\vert\vert})$ gives superlinear convergence, and $\eta_k = \min(0.5, \vert\vert\nabla f_k\vert\vert)$ yields quadratic convergence.

### Inexact Newton step solvers

- Line search Newton-CG method

    We terminate the CG iterations as soon as a direction of negative curvature is generated and the stop at negative curvature with $d_j^T B_k d_j \leq 0$ ensures that $p_k$ is a descent direction for $f$ at $x_k$.

- Trust-Region Newton-CG method (CG-Steihaug)

    The trust-region Newton-CG method deals more effectively when the Hessian is nearly singular comparing with line search Newton-CG method.

- Trust-Region Newton-Lanczos method

    A limitation of Newton-CG method is that it accepts any direction of negative curvature, even when this direction gives an insingificant reduction in the model. A more praactical alternative is to use the Lanczos method.


## Limited-Memory Quasi-Newton Methods

### recursive vector multiplication

Take L-BFGS for example, the updating formula is

$$ H_{k+1} = V_k^T H_k V_k + \rho_k s_k s_k^T, \quad \rho_k=\frac{1}{y_k^T s_k}, \quad V_k = I - \rho_k y_k s_k^T.  $$

We store a modified version of $H_k$ implicity, by storing a certain number (say, $m$) of the vector pairs $\lbrace s_i, y_i \rbrace$. After the new iterate is computed, the oldest vector pair in the set of pairs $\lbrace s_i, y_i \rbrace$ is replaced by the new pair $\lbrace s_k, y_k \rbrace$ obtained from the current step.

Since the product $H_k \nabla f_k$ could be computed by a sequence of inner product and vector summation, an efficient two-loop recursive procedure can be derived for the product.

### compact representation

One can represent the quasi-Newton matrices in a compact form like

$$ B_k = B_0 - [B_0 S_k \quad Y_k] \begin{bmatrix} S_k^T B_0 S_k & L_k \\ L_k^T & -D_k \end{bmatrix}^{-1} \begin{bmatrix} S_k^T B_0 \\ Y_k^T \end{bmatrix}. $$

For limited-memory version, one could truncate the matrix $S_k$ and $Y_k$ to obtain a "SVD" shape structure of $B_k$ for computation. 

------------------------------------------------------------------------------

# Derivative-Free Optimization

Various algorithm have been developed that do not attempt to approximate the gradient. Rather, they use the function values at a set of sample points to determine a new iterate by some other means.

## Model-based methods

Some of the most effective algorithms for unconstrained optimization compute iterated steps by minimizing a quadratic model of the objective function $f$. The quadratic model is formed using function and derivative information at the current iterate. When derivatives are not available, we may define the model $m_k$ as the quadratic function that interpolates $f$ at a set of approprately chosen sample points, i.e, we construct

$$ m_k(x_k + p)  = c + g^T p + \frac{1}{2}p^T G p, $$

by imposing the interpolation conditions

$$ m_k(y^l) = f(y^l), \quad l=1, 2, \ldots, q. $$

Once $m_k$ has been formed, we compute a step $p$ by approximately solving the trust region subproblem for some trust-region radius.

NOTE: we can replace the quadratic model by a linear model to reduce the computation. A mixture of quadratic and linear models is also used in practice.


NOTE: To acheive economy for the quadratic model, a method is to retain only $O(n)$ points for the interpolation conditions and absorb the remaining degrees of freedom in the model $m_k(x+p)$ by requiring that the Hessian of the model change at little as possible from one iteration to the next. 

$$ \min \|G - G_k\|_F^2 \quad  \text{s.t. } G = G^T, m(y^l) = f(y^l). $$

The least-change property is one of the key ingredient in quasi-Newton mothods.


## Coordinate-search and pattern-search method

Coordinate search may be useful because it does not require the gradient $\nabla f_k$. 

The coordinate search method cycles through the $n$ coordinate directions $e_1, \ldots, e_n$, obtaining new iterates by performing a line search along each direction in turn.

However, a cyclic search along any set of linearly independent direcions does not guarantee global convergence.

The pattern search methods choose a certrain set of search directions $\mathcal{D}_k$ at each iterate and evaluate $f$ at a given step length along each of these directions. If a point with a significantly lower function value is found, it is adopted as the new iterate, and the center of the candidate points is shifted to this new point.


## A conjugate-direction method

A derivative-free optimization method should construct conjugate directionos using only function values.

This construction uses the parallel subspace property.

The resulting conjugate-gradient method has been found to be useful for solving small dimensional problems.


## Nelder-Mead method

In a single iteration of the Nelder-Mead algorithm, we seek to remove the vertex with the worst function value and replace it with another point with a better value. The new point is obtained by reflecting, expanding, or contracting the simplex along the line joining the worst vertex with the centroid of the remaining vectices. if we cannot find a better point in this manner, we retain only the vertex with the best function value, and we shrink the simplex by moving all other vertices toward this value.

NOTE: unless the final shrinkage step is performed, the average function value (the centroid of the vertices) 

$$ \frac{1}{n+1} \sum_{i=1}^{n+1} f(x_i) $$

will decrease at each step.

----------------------------------------------------------------------------------


# Least-Squares Problem

In least-squares problems, the objective function $f$ has the form:

$$ f(x) = \frac{1}{2} \sum_{j=1}^m r_j^2(x), $$

where each residual $r_j(x)$ is a smooth function from $\mathbb{R}^n$ to $\mathbb{R}$. And we always assume that $m \geq n$.

This special form of $f$ makes it easier to solve than general unconstraint minimization problem.

Denote residual vector $r(x)= \left( r_1, \cdots, r_m \right)^T$ and Jacobian matrix $J(x) = \begin{pmatrix} \nabla r_1(x)^T \\ \vdots \\ \nabla r_m(x)^T \end{pmatrix}$, then we have

$$ \nabla f(x) = J(x)^T r(x), \quad \nabla^2 f(x) = J(x)^T J(x) + \sum_{j=1}^m  r_j(x) \nabla^2 r_j(x). $$

NOTE: the term $J(x)^T J(x)$ is often more important than the second summation term in $\nabla^2 f(x)$, either because the $\nabla^2 r_j(x)$ are relative small near the solution or because of small residuals.


## Linear least-squares problems

The objective is now 

$$ f(x)=\frac{1}{2} \|Jx - y\|, $$

which gives the normal equations $J^T J x^{\ast} = y$.

For $m\geq n$ and $J$ has full column rank, we have three major algorithms

- Cholesky factorization

- QR factorization

- Singular value decomposition


## Nonlinear least-squares problems

### Gauss-Newton method

Instead of solving the standard Newton equations $\nabla^2 f(x_k) p = - \nabla f(x_k)$, we solve the modified system to obtain the search diection $p_k^{\text{GN}}$, 

$$ J_k^T J_k p_k^{\text{GN}} = - J_k^T r_k. $$

The modification can be viewed as taking the approximation

$$ \nabla^2 f_k \approx J_k^T J_k, $$

or finding the search direction by solving the linear least-squares problem 

$$ \min_p \frac{1}{2} \|J_k p + r_k \|^2, $$

due to the approximation

$$ f(x_k + p) = \frac{1}{2} \|r(x_k + p)\|^2 \approx \frac{1}{2} \|J_k p + r_k\|^2. $$

One can prove that when $J_k$ has full rank and $\nabla f_k$ is non-zero, the direction $p_k^{\text{GN}}$ is a desent direction. Further, under some assumption the Gauss-Newton method possess global convergence.

### Levenberg-Marquardt method

This method solves the same subproblem for search direction but replaces the line search with a trust-region strategy.

$$ \min_p \frac{1}{2} \|J_k p + r_k \|^2, \quad \|p\| \leq \Delta_k. $$


### Methods for large-residual problem

In large-residual problems, the second-part of the Hessian $\nabla^2 f(x)$ could not be ignored.

We often do not know beforehand whether a problem will turn out to have small or large residuals at the solution. A hybrid algorithm will be used, which would behave like Gauss-Newton if the residuals are small and switch to Newton or quasi-Newton if the residuals are large. This can be achieved by maintaining a sequence of positive definite Hessian approximations $B_k$.

---------------------------------------------------------------------------------------------------


# Nonlinear Equations

The problem of nonlinear equations 

$$ r(x)=0, \quad r:\mathbb{R}^n \rightarrow \mathbb{R}^n, $$

can be connected with optimization algorithms in the views of:

- stationary point of an optimization problem, $r(x)=\nabla f(x) = 0$, which gives the local algorihtms

- solution of the nonlinear least-square problem, $\min_x \frac{1}{2} \sum_{i=1}^n r_i^2(x)$, which gives the merit function algorithms

The nonlinear equations could also connect to ODEs, which corresponding to the homotopy method.

## Local algorithms

By treating $r(x)=\nabla f(x) = 0$， the algorithms for unconstrained optimization could be derived similarly.

**Newton's method**

From 

$$ r(x+p) = r(x) + \int_{0}^1 J(x+tp) p ~ dt, $$

we can define a linear model of $r(x+p)$ as 

$$ M_k(p) = r(x_k) + J(x_k) p, $$

which gives the Newton step $J(x_k) p _k = - r(x_k)$.

When $x_k$ is close to a nondegenerate root $x^\ast$ ($J(x^\ast)$ is nonsingular), Newton's method converges superlinearly.

**Inexact Newton methods**

Inexact Newton methods use search direction $p_k$ that satisfy the condition

$$ \|r_k + J_k p_k\| \leq \eta_k \|r_k\|, \quad \eta_k \in [0, \eta]. $$

**Broyden's method**

Quasi-Newton method construct the approximation to the Jacobian $J(x)$, updating it at each iteration so that it mimics the behavior of the true Jacobian $J$ over the step just taken, i.e.,

$$ M_k(p) = r(x_k) + B_k p, \quad B_{k+1} = B_k + \frac{(y_k - B_k s_k) s_k^T}{s_k^T s_k}. $$


## Merit function algorithms

The most widely used merit function is the sum of squares, 

$$ f(x) = \frac{1}{2} \sum_{i=1}^n r_i^2(x). $$

Each root of $r(x)$ is a minimizer of $f$. Since $\nabla f(x)= J(x)^T r(x)$, for those local minima that are not roots of $r(x)$, they satisfy an interesting property that $J(x)$ is singular.

Optimization algorithm could be applied to this merit function:

- Line search methods

- Trust-region methods


## Continuation/Homotopy methods

Newton-based methods all suffer from one shortcoming that they require $J(x)$ to be nonsingular. This condition often can not be guaranteed in practice.

The motivation of continuation method is that: rather than dealing with the original problem $r(x)=0$ directly, we set up an ``easy'' system of equations and gradually transform the easy system into the original system $r(x)$.

One simple way is to define the homotopy map $H(x, \lambda)$ as 

$$ H(x, \lambda) = \lambda r(x) + (1 - \lambda) (x-a). $$

The trajectory of points $(x, \lambda)$ for which $H(x, \lambda)=0$ is called the zero path.

**Practical continuation methods**

Treat the homotopy map as 

$$ H(x(s), \lambda(s)) = 0, \quad s\geq 0. $$

Taking derivative with respect to $s$ gives

$$ \frac{\partial}{\partial x} H(x, \lambda) \frac{d x}{d s} + \frac{\partial}{\partial \lambda} H(x, \lambda) \frac{d \lambda}{d s} = 0. $$

The vector $(\dot{x}, \dot{\lambda})$ lies in the null space of the $n \times (n+1)$ matrix $\left[ \frac{\partial}{\partial x} H(x, \lambda), \frac{\partial}{\partial \lambda} H(x, \lambda) \right]$, which could be computed by a QR factorization. After solving the ODE system for $(\dot{x}, \dot{\lambda})$ with initial condition $(a, 0)$ we can trace the zero path. 


--------------------------------------------------------------------------------------

# Theory of Constrained Optimization

A general formulation is 

$$ \min_{x\in \mathbb{R}^n} f(x)  \quad \text{subject to } \quad c_i(x) = 0, i\in \mathcal{E}, \quad c_i(x) \geq 0, i \in \mathcal{I}. \quad \Rightarrow \quad \min_{x\in \Omega} f(x) $$


## Motivation of first-order condition

We want to derive mathematical characterization of the solution of the constrained minimization problem.

Firstly introduce the concept of active set.

**Definition: Active set**

The active set $\mathcal{A}(x)$ at any feasible $x$ consists of the equality constraint indices from $\mathcal{E}$ together with the indices of the inequality constraints $i$ for which $c_i(x)=0$, i.e.,

$$ \mathcal{A}(x) = \mathcal{E} \cup \{i \in \mathcal{I} ~|~ c_i(x) = 0\}. $$

The active set comes from the consideration that for $x$ lies strictly inside the feasible set so that the strict inequality $c_i(x) > 0$ hold, then any step $s$ sufficiently small will retains feasibility of $c_i(x+s)$ in the sense that $c_1(x) + \nabla c_i(x)^T s \geq 0$. 


Since we can not produce a decrease in $f$ at the local minimizer, we try to determine whether it is possible to take a feasible descent step away from a given feasible point $x$ by examing the first derivatives of $f$ and constraint $c_i$.

This approach makes sense only when the linearized approximation captures the essential geometric features of the feasible set near the point $x$ in question.

**Definition: Tangent cone**

The vector $d$ is said to be a tangent vector to feasible set $\Omega$ at a point $x$ if there exists $t_k$ and $z_k$ such that

$$ \lim_{k \rightarrow \infty} t_k = 0, \lim_{k \rightarrow \infty} z_k = x, \quad \Rightarrow \quad \lim_{k\rightarrow \infty} \frac{z_k - x}{t_k} = d. $$

The set of all tangents to $\Omega$ at $x^\ast$ is call the tangent cone and is denoted by $T_{\Omega}(x^{\ast})$.

NOTE: The scalar sequence $t_k$ here is useful since it could be pre-choosed and used to determine $z_k$, which gives $z_k = x^\ast + t_k d + o(t_k)$.

If $x^\ast$ is a local minimizer, then we have

$$ \nabla f(x^\ast)^T d \geq 0, \quad \text{for all } d \in T_{\Omega}(x^\ast). $$


**Definition: linearized feasible directions**

Given feasible point $x$ and active constraint set $\mathcal{A}(x)$, the set of linearized feasible directions $\mathcal{F}(x)$ is 

$$ \mathcal{F}(x) = \left\{ d ~|~ d^T \nabla c_i(x) = 0, i\in\mathcal{E}, d^T\nabla c_i(x) \geq 0, i \in \mathcal{A}(x) \cap \mathcal{I}. \right\}. $$

Constraint qualification are assumptions that ensure similarity of the constraint set $\Omega$ and its linearized approximation, in a neighborhood of $x^{\ast}$., i.e., conditions that the linearized feasible set $\mathcal{F}(x)$ is similar to the tangent cone $T_{\Omega}(x)$.

**Definition: LICQ**

Given the point $x$ and the active set $\mathcal{A}(x)$, we say that the linear independence constraint quanlification holds if the set of active constraint gradients $\lbrace \nabla c_i(x), i \in \mathcal{A}(x) \rbrace$ is linearly independent.

Then we have 

$$ T_{\Omega}(x^{\ast}) \rightarrow \mathcal{F}(x^\ast), \quad \mathcal{F}(x^\ast) \xrightarrow{\text{LICQ}} T_{\Omega}(x^\ast). $$

NOTE: given a sequence of positive scalars $t_k$, we apply the implicit function theorem to 

$$ R(z, t) = \begin{bmatrix} c(z) - tA(x^\ast)d \\ Z^T (z - x^\ast - td) \end{bmatrix} = 0 $$

to generate the feasible path $z_k$ approching $x^\ast$ with desired tangent direction.

**First-order necessary condtion**

By LICQ, we know that if $x^\ast$ is *not* a local minimizer, then 

$$ \text{there exists } d \in \mathcal{F}(x^\ast) \text{ such that } \nabla f(x^\ast)^T d < 0. $$

Denote 

$$ B = [\nabla c_i(x)]_{i \in \mathcal{I} \cap \mathcal{A} }, \quad C = [\nabla c_i(x)]_{i \in \mathcal{E}}, $$

then it is equivalent that 

$$ \nabla f(x^\ast)^T d < 0, \quad B^T d \geq 0, \quad C^T d = 0 \quad \text{is solvable}. $$

A classical result gives the solution to this linear inequalities system

**Farkas Lemma**

Let the cone $K=\lbrace By + Cw \mid y \geq 0 \rbrace$, given any vector $g\in \mathbb{R}^n$, we have either that $g \in K$ or that there exists $d \in \mathbb{R}^n$ satisfying 

$$ g^T d < 0, \quad B^T d \geq 0, \quad C^T d=0,  $$

but not both.

NOTE: $d$ in Farkas lemma could be constructed by projection, but one should be careful that $K$ is only a cone but not a vector space.

## First-order optimality conditions

Define the **Lagrangian function** as

$$ \mathcal{L}(x, \lambda) = f(x) - \sum_{i \in \mathcal{E}\cup \mathcal{I}} \lambda_i c_i(x). $$

Suppose $x^\ast$ is a local minimizer, with continuous differentiable $f$ and $c_i$, and the LICQ holds at $x^\ast$. Then there is a Lagrange multiplier vector $\lambda^\ast$ such that

$$
\begin{align}
    \nabla_x\mathcal{L}(x^\ast, \lambda^\ast) &= 0, \\
    c_i(x^\ast) &= 0, i \in \mathcal{E} \\
    c_i(x^\ast) &\geq 0, i \in \mathcal{I} \\
    \lambda_i^\ast &\geq 0, i \in \mathcal{I} \\
    \lambda_i^\ast c_i(x^\ast) &= 0, i \in \mathcal{E} \cup \mathcal{I}.    
\end{align}
$$

The last condition is called the complementarity conditions, which removes those inactive indices from the Lagrangian function condition

$$ 0 = \nabla_x \mathcal{L}(x^\ast, \lambda^\ast) = \nabla f(x) - \sum_{i \in \mathcal{A}(x^\ast)} \lambda_i^\ast \nabla c_i(x^\ast). $$

### A Geometric Viewpoint

**Definition: Normal cone**

The normal cone to the set $\Omega$ at the point $x \in \Omega$ is defined as

$$ N_{\Omega}(x) = \{ v \mid v^T w \leq 0, \text{ for all } w \in T_{\Omega}(x) \}. $$

Geometrically, each normal vector $v$ makes an angle of at least $\pi/2$ with every tangent vector.

Then the first-order condition is simplified as: suppose $x^\ast$ is a local minimizer, then

$$ -\nabla f(x^\ast) \in N_{\Omega}(x). $$

With LICQ and Farkas Lemma, we further have

$$ N_{\Omega}(x^\ast) = - \{\sum_{i\in \mathcal{A}(x^\ast)}\lambda_i\nabla c_i(x^\ast), \lambda_i \geq 0, \quad i\in \mathcal{A}(x^\ast) \cap \mathcal{I} \} $$


## Second-order conditions

Essentially, the second-order conditions concern the curvature of the Lagrangian function in the "undecided" directions, i.e., the directions $w\in \mathcal{F}(x^\ast)$ for which $w^T \nabla f(x^\ast)=0$.

To figure out such "undecided" directions, note that the KKT conditions gives 

$$ \nabla f(x^\ast) = \sum_{i \in \mathcal{E} \cup \mathcal{I}} \lambda_i^\ast \nabla c_i(x^\ast). $$

Then from $w^T \nabla f(x^\ast)=0$ we define the critical cone $\mathcal{C}(x^\ast, \lambda^\ast)$. 

That is, the critical cone is almost the "Null space" of the gradient space of constraint. For example, the null space of matrix $A$ if we consider linear constraint $Ax=b$.

**Definition: Critical cone**

$$ \mathcal{C}(x^\ast, \lambda^\ast) = \{ w\in \mathcal{F}(x^\ast) \mid \nabla c_i(x^\ast)^T w = 0, \quad i \in \mathcal{A}(x^\ast) \cap \mathcal{I} \text{ with } \lambda_i^\ast > 0 \}. $$

Then we have 

- Second-order necessary conditions: LICQ + KKT +

$$ w^T \nabla_{xx}^2 \mathcal{L}(x^\ast, \lambda^\ast) w \geq 0, \quad \forall w \in \mathcal{C}(x^\ast, \lambda^\ast). $$

- Second-order sufficient conditions: KKT + 

$$ w^T \nabla_{xx}^2 \mathcal{L}(x^*, \lambda^*) w > 0, \quad \forall w \in \mathcal{C}(x^\ast, \lambda^\ast), w \neq 0. $$

NOTE: The sufficient condition over the critical cone acutally gives a local strong convexity.


## Duality

Consider the problem

$$ \min_{x\in \mathbb{R}^n} f(x), \quad s.t. \quad  c_i(x) \geq 0, i=1, 2, \ldots, m. $$

where $f$ and $-c_i$ are all convex function.

Recall the Lagrangian function defined as

$$ \mathcal{L}(x, \lambda) = f(x) - \lambda^T c(x). $$

Define the dual objective function as

$$ q(\lambda) := \inf_{x} \mathcal{L}(x, \lambda). $$

Then the dual problem is defined as

$$ \max_{\lambda \in \mathbb{R}^n} q(\lambda), \quad s.t. \quad \lambda \geq 0. $$

Some of the important relation for dual problem are as follows.

- The dual function $q$ is concave and its domain $\lbrace \lambda \mid q(\lambda) > - \infty \rbrace$ is convex.

- **Weak duality**: For any feasible $\bar{x}$ and any $\bar{\lambda} >0$, we have $q(\bar{\lambda}) \leq f(\bar{x})$.

- If $f$ and $-c_i$ are differentiable at the minimizer $\bar{x}$, then any $\bar{\lambda}$ such that $(\bar{x}, \bar{\lambda})$ satisfying KKT condition is a solution of the dual problem.

- Conversely, with strict conditions the solution to the dual problem can be used to derive the solution to the original problem.

-------------------------------------------------------------------------------------------

# Linear programming

After introducing slack variable and splitting to nonnegative and negative part, linear programs take the standard form as

$$ \min \, c^T x, \quad s.t. \quad Ax=b, x \geq 0, $$

where $x\in \mathbb{R}^n$, $A \in \mathbb{R}^{m \times n}$ with $m < n$.

NOTE: We always assume that matrix $A$ has full row rank.

## Optimality and duality

The Lagrangian function 

$$ \mathcal{L}(x, \lambda, s) = c^T x - \lambda^T (Ax - b) - s^T x, $$

gives the KKT condition of linear programming as

$$
\begin{align*}
    A^T \lambda + s &= c, \\ 
    Ax &= b, \\
    x &\geq 0, \\
    s &\geq 0, \\
    x^T s &= 0.
\end{align*}
$$

It is easy to verify that the KKT condition is also the sufficient condition for a solution of the linear programming.

The dual problem takes the form

$$ \max \, b^T \lambda, \quad s.t. \quad A^T \lambda + s = c, s \geq 0. $$

And we have the strong duality for the linear programming problem.

## Geometry structure

The feasible set defined by the linear constraints is a polytope. And the vertices of this polytope are the points that do not lie on a straight line between two other points in the set.

We introduce the definition of basic feasible point.

**Definition: basic feasible point**

A vector $x$ is a basic feasible point if it is feasible and if there exists a subset $\mathcal{B}$ of the index set $\lbrace 1, 2, \ldots, n\rbrace$ such that

- $\mathcal{B}$ contains exactly $m$ indices and the $m\times m$ matrix $B=[A_i]_{i \in \mathcal{B}}$ is non-singular, where $A_i$ is the $i$-th column of $A$.

- $i \notin \mathcal{B} \Rightarrow x_i=0$ (**$\mathcal{B}$ contains all the inactive non-negative constraints**)

One can verify that 

- If the linear programming problem has a nonempty feasible region, there is at least one basic feasible point

- If the linear programming problem has solutions, then at least one such solution is a basic optimal point.

- All basic feasible point are vertices of the feasible polytope $\lbrace x \mid Ax=b, x \geq 0 \rbrace$ and vice versa.

Thus, we know then the solution of programming problem must be one of the vertices of the feasible region.


## The simplex method

Starting from a basic feasible point (vertex), all iterates of the simplex method are basic feasible points (vertices). Most steps consist of a move from one vertex to an adjacent one for which the basis $\mathcal{B}$ differs in exactly one component. We will use the KKT condition to guide the movement between the vertices.

Given a basis $\mathcal{B}$ and corresponding basice feasible point $x$, denote the nonbasis index set as $\mathcal{N}=\lbrace 1, \ldots, n \rbrace \setminus \mathcal{B}$. We then split a vector $x$ as $x_B$ and $x_N$. If we want the pair $(x, s, c)$ to satisfy the KKT condition, one could choose

$$ 
\begin{align*}
    Ax = b &\Rightarrow x_B= B^{-1} b, \quad x_N = 0 \\
    x^T s = 0 & \Rightarrow s_B = 0 \\
    A^T \lambda + s = c &\Rightarrow \lambda = B^{-T} c_B, \quad s_N = c_N - N^T \lambda
\end{align*}
$$

The other constraints $x \geq 0$ is already satisfied and only the non-negative condition $s \geq 0$ has not be enforced explicitly.  

If there is an index $q \in \mathcal{N}$ such that $s_q < 0$, we choose index $q$ to enter $\mathcal{B}$ and update $x$ and $s$ acoordingly, which yields a leaving index from $\mathcal{B}$.

Denote the update of $x$ as $x^+$, then we have

$$ Ax^+ = Bx_B^+ + A_q x_q^+ = b = Ax = Bx_B \Rightarrow x_B^+ = x_B - B^{-1} A_q x_q^{+}. $$

Further, for the update of the objective function,

$$ c^T x^+ = c^T x + s_q x_q^+. $$

We continue to increase $x_q^+$ to $\infty$, if one of the components $x_p$, $p \in \mathcal{B}$ decreases to zero, then we finish an iterate of the simplex method, else, the linear program is unbouned.

### Implementation issues

First, if there are many negative components of $s_N$ at each step, different strategies resolves the tradeoff between the effort spent on finding a good entering index and the amount of decrease in $c^Tx$ resulting from this choice.

Second, to start the simplex method, we have to find a basic feasible point, which is equivalent to solve a linear program. A two-phase approach is commonly used to deal with this difficulty.

In phase I we redefine the original problem as a linear program problem with an explicit basic feasible point, 

$$ \min \, e^T z \quad s.t. \quad Ax + Ez = b, (x, z) \geq 0, $$

where $z \in \mathbb{R}^m, e=(1, \cdots, 1)^T$, and $E$ is a diagonal matrix whose diagonal entries are

$$ E_{jj} = +1，\text{if } b_j \geq 0, \quad E_{jj} = -1，\text{if } b_j < 0. $$

Then the point defined as

$$ x=0, \quad z_j =|b_j|, \quad j=1, \ldots, m, $$

is a basic feasible point. Further, the Phase-I problem has an optimal ojective value of zero if and only if the original problem is feasible. Then the Phase-II problem will be

$$ \min \, c^T x \quad s.t. \quad Ax + z = b, \quad x \geq 0, \quad 0 \geq z \geq 0. $$

We need to retain the artifical variable $z$ in Phase-II since some components of $z$ may still be present in the optimal basis from Phase-I.

Third, a phenomenon known as cycling can occur. After a number of successive "degenerate" steps, we may return to an earlier basis $\mathcal{B}$.

Finally, a variert of techniques are used to reduce the size of the user-defined linear programming problem before passing it to the solver.

**NOTE**: The complexity of the simplex method is exponential.


## Interior-Point methods

Many large linear programs could be solved efficiently by using methods from nonlinear programming and nonlinear equations. Interior-point method comes from the fact that all iterates satisfy the inequality constraints strictly.

Next we introduce a subclass of interior-point method named primal-dual methods.

Primal-dual method finds the solution $(x^\ast, \lambda^\ast, s^\ast)$ of the KKT systems by applying Newton's method to the equalities and modifying the search directions and step lengths so that the inequality $x > 0, s >0$ are satisfied strictly at every iteration.

The Newton equation is given by

$$
\begin{bmatrix} 
    0 & A^T & I \\
    A & 0 & 0 \\
    S & 0 & X
\end{bmatrix} \begin{bmatrix}
    \Delta x \\
    \Delta \lambda \\
    \Delta s
\end{bmatrix} = \begin{bmatrix}
    -(A^T \lambda + s - c) \\
    -(Ax - b) \\
    - XS e
\end{bmatrix}, \quad e = (1, 1, \ldots, 1)^T
$$

where $X=\text{diag}(x_1, \ldots, x_n)$ and $S=\text{diag}(s_1, \ldots, s_n)$.

For the new iterate $(x, \lambda, s) + \alpha (\Delta x, \Delta \lambda, \Delta s)$, we often can take only a small step along this direction before violating the condition $x, s > 0$. This positive property is important for solving the linear system.

In order to accelerate the convergence, we consider to use a less aggressive Newton direction to obtain larger step length, i.e., we relax the Newton equation as  

$$
\begin{bmatrix} 
    0 & A^T & I \\
    A & 0 & 0 \\
    S & 0 & X
\end{bmatrix} \begin{bmatrix}
    \Delta x \\
    \Delta \lambda \\
    \Delta s
\end{bmatrix} = \begin{bmatrix}
    -(A^T \lambda + s - c) \\
    -(Ax - b) \\
    - XS e + \sigma \mu e
\end{bmatrix}, \quad \mu = \frac{x^T s}{n}, \quad \sigma \in [0, 1].
$$

That is, we relax the KKT condition as

$$
\begin{align*}
    A^T \lambda + s &= c, \\ 
    Ax &= b, \\
    x &\geq 0, \\
    s &\geq 0, \\
    x_i s_i &= \tau, \quad i=1, \ldots, n,
\end{align*}
$$

which is equivalent to solve a logarithmic-barrier formulation of the linear program problem

$$ \min \, c^T x - \tau \sum_{i=1}^n \ln x_i, \quad s.t., \quad Ax=b, \text{ for } \tau > 0. $$

We highlight that for $- XS e + \sigma \mu e$, with $\sigma=1$, the so called centering direction will prevent the iterates from coming too close to the boundary of the nonnegative orthant, they ensure that it is possible to take a non-trivial step along each search direction (recall that the iterates of simplex method are all vertices point). 

One of the remained problem is to make sure that $\mu_k$ will approach to zero as $k\rightarrow \infty$. 

For theoretical analysis, we introduce the concept of central path and it neighborhoods, which gives a non-trivial path lies in the feasible set toward the optimal solution.

We define the central path as 

$$ \mathcal{C} = \{(x_\tau, \lambda_\tau, s_\tau) \mid  Ax_\tau =b, A^T\lambda_\tau + s_\tau =c, x_\tau,s_\tau >0, \quad (x_\tau)_i (s_\tau)_i = \tau, \, i=1, \ldots, n\tau > 0 \} $$

where $(x_\tau, \lambda_\tau, s_\tau)$ is the solution of the KKT condition of the relaxed lograrithmic-barrier linear program problem.

Since we don't know the solution of the relaxed KKT condition, which need to be solved through a Newton process, we introduce some neighborhoods of the central path $\mathcal{C}$. For example, for $\gamma \in [0, 1)$，

$$ \mathcal{N}_{-\infty} (\gamma) = \{(x, \lambda, s) \mid Ax =b, A^T\lambda + s =c, x,s >0, \quad x_i s_i \geq \gamma \mu, \, i=1, \ldots, n \}. $$

Now the update constraint changes from $x^{k+1}, s^{k+1} > 0$ to $(x^{k+1}, \lambda^{k+1}, s^{k+1}) \in \mathcal{N}_{-\infty}(\gamma)$, which gives a long step update formulation.

Then we could prove that under some conditions, there is a constant $\delta$ independent of $n$ such that 

$$ \mu_{k+1} \leq (1 - \frac{\delta}{n}) \mu_k. $$

Thus, suppose the starting point satisfying $(x^{0}, \lambda^{0}, s^{0}) \in \mathcal{N}_{-\infty}(\gamma)$, then after $O(n\log(1/\epsilon))$ steps we have $\mu_k \leq \epsilon \mu_0$.


## Comparison of simplex method and interior-point method

Each interior-point iteration is expensive while the simplex method requires a larger number of inexpensive iterations.

For worst case, simplex is exponential and interior-point is polynomial.

In general, simplex codes are faster on probelms of small-medium dimensions, while interiot-point codes are competitive and often faster on large problems.

Interior-point methods are less useful than simplex approaches in situations in which warm-start information is readily available.


---------------------------------------------------------------------------

# Quadratic programming

The general quadratic program (QP) can be stated as

$$
\begin{align*}
    \min_x & \quad q(x) = \frac{1}{2} x^T G x + x^T c \\
    \text{s.t.} & \quad a_i^T x = b_i, \quad i \in \mathcal{E} \\
     & \quad a_i^T x \geq b_i, \quad i \in \mathcal{I}
\end{align*}
$$

$G$ is a symmetric $n \times n$ matrix. Here we consider convex QP that $G$ is positive semidefinite.

## Equality-constrained QP

The equality-constrained QP takes the form

$$
\begin{align*}
    \min_x & \quad q(x) = \frac{1}{2} x^T G x + x^T c \\
    \text{s.t.} & \quad Ax = b \\
\end{align*}
$$

We always assume that $A$ has full row rank (rank $m$).

The first-order necessary condition gives

$$
\begin{bmatrix}
    G & -A^T \\ A & 0
\end{bmatrix} 
\begin{bmatrix} x^\ast \\ \lambda^\ast \end{bmatrix}
= \begin{bmatrix} -c \\ b \end{bmatrix}
$$

Or equivalently, let $x^\ast=x+p$ we have
$$
\begin{bmatrix}
    G & -A^T \\ A & 0
\end{bmatrix} 
\begin{bmatrix} -p \\ \lambda^\ast \end{bmatrix}
= \begin{bmatrix} g \\ h \end{bmatrix},
\quad h=Ax-b, g=c + Gx.
$$

Such KKT matrix is non-singular if we add conditions that $A$ has full row rank and $Z^T G Z$ is positive definite, where $Z$ is the null space of matrix $A$, i.e., $AZ=0$.

**Methods for solving the KKT system**

- Direction methods

    - Symmetric indefinite factorization 
    
    - Schur-complement

    - Null-space method: assume that we have obtained $Y$ and $Z$ such that $[Y \mid Z]$ is non-singular, let $p=Y p_Y + Z p_z$.

- Iterative methods

    - GMRES for full linear system

    - PCG for reduced system derived from null-space method

    - Projected CG avoids operating with the null-space basis $Z$


## Inequality-constrained QP

Two types of methods: active-set methods and interior-point methods for QP are similar for the situation of LP, i.e., simplex method and interior-point method.

Consider the QP problem

$$ \min_x \quad q(x)=\frac{1}{2} x^T G x + x^T c \quad \text{subject to } a_i^T x = b_i, \quad i \in \mathcal{A}(x^*). $$


### Active-set methods for QP

Assume that we have a feasible start point $x_k$, with a working set $\mathcal{W}_k$, which should converged to the active set of the optimal point $x^\ast$. 

We also assume that the gradients $a_i$ of the constraints in the working set are linearly independent.

The first step is to solve the optimal solution under current working set, i.e. let $p=x-x_k$, $g_k = Gx_k + c$, then solve

$$ \min_xp\quad \frac{1}{2} p^T G p + g_k^T p \quad \text{subject to } a_i^T p = b_i, \quad i \in \mathcal{W}_k, $$

the constraints guarantee that $a_i^T(x_k + \alpha p_k) = a_i^T x_k = b_i$ for all step size $\alpha$.

We then consider using step size

$$ \alpha_k = \min \left( 1, \min_{i\notin \mathcal{W}_k, a_i^T p_k < 0} \frac{b_i-a_i^T x_i}{a_i^T p_k} \right). $$

If such $p$ gives $x_{k+1}=x_k + p$ satisfying all the constraints, then we find the local minimum for current working set.

If such $p$ gives $x_{k+1}=x_k + p$ violating some of the constraints with $\alpha_k < 1$, then we add one of the blocking constraints (those $i$ such that the minimum of $\alpha$ is achieved) to $\mathcal{W}_k$ to get $\mathcal{W}_{k+1}$ and continue the iteration.

After we obtain the local minimum $\hat{x}$ of the current working set, we solve for the Lagrange multiplier $\hat{\lambda}_i$ from

$$ \sum_{i \in \hat{\mathcal{W}}} a_i \hat{\lambda}_i = g = G \hat{x} + c, $$

and check the complementarity condition. If one of the multiplier does not satisfy the complementarity condition, the objective function may be decreased by dropping one of the constraints, thus, we remove one of the indices to get $\mathcal{W}_{k+1}$ and repeat the iteration.

**NOTE**: One could prove that the dropping strategy gives a feasible and descent direction under some condition.


### Interior-point methods for QP

The procedure of interior-point method for QP is similar for those in LP, except with a different KKT matrix.


--------------------------------------------------------------

# Penalty Methods

Most approach define a sequence of penalty function, in which the penalty terms for the constrain violations are multiplied by a positive coefficient. By making the coefficient larger, we penalize constraint violations more severely, thereby forcing the minimizer of the penalty function closer to the feasible region for the constrained problem.

## Quadratic penalty method

Given the constrained optimization problem

$$ \min_{x} f(x) \quad s.t. \quad c_i(x)=0, i \in \mathcal{E}, \quad c_i(x) \geq 0, i \in \mathcal{I}.  $$

The quadratic penalty function is given by

$$ Q(x; \mu) = f(x) + \frac{\mu}{2} \sum_{i \in \mathcal{E}} c_i^2(x) + \frac{\mu}{2}\sum_{i \in \mathcal{I}} ([c_i(x)]^-)^2, \quad [y]^- := \max(-y, 0). $$

We note that the minimization of $Q(x; \mu)$ becomes more difficult to perform as $\mu_k$ becomes large, unless we use special techniques to calculate the search direction.

The relation between original constraint optimization and quadratic penalty problem is 

- If $x_k$ is exact global minimizer of $Q(x;\mu_k)$, then the global solution $x^\ast$ of original problem is only the limit of $x_k$, i.e., $\lim_{k\in\mathcal{K}} x_k = x^\ast$ for an infinite subsequence $\mathcal{K}$ such that $\mu_k \rightarrow \infty$.

- If $x_k$ is an inexact solution such that $\vert\vert \nabla_x Q(x; \mu_k) \vert\vert \leq \tau_k$ with $\tau_k \rightarrow 0$, then a feasible limit point $x^\ast$ of $x_k$ such that $\mu_k \rightarrow \infty$ satisfying LICQ condition is the KKT point of original problem and we have

$$ \lim_{k\in \mathcal{K}} -\mu_k c_i(x_k) = \lambda_i^\ast, \quad i \in \mathcal{E}. $$


## Nonsmooth penalty method

Some nonsmooth penalty functions, like $l_1$ penalty function, is exact, which means that for all penalty parameter $\mu_k > \mu^\ast$, the solution of the penalty problems are the same, $x_k = x^\ast$ for $k > K$, i.e., the solution of the original problem. Such exact problem is different with quadratic penalty and now a single optimization for the $l_1$ penalty could yield the exact solution of original problem.

NOTE: one can prove that penalty function must be nonsmooth to be exact.

The $l_1$ penalty function takes the form

$$ \phi_1(x; \mu) = f(x) + \mu \sum_{i \in \mathcal{E}} |c_i(x)| + \mu\sum_{i \in \mathcal{I}} [c_i(x)]^-, \quad [y]^- := \max(-y, 0). $$

The relation between original constraint problem and $l_1$ penalty problem is 

- The strict local minima of original problem is the local minima of $\phi_1(x;\mu)$ for $\mu > \mu^\ast$.

- The feasible stationary point of $\phi_1(x; \mu)$ for $\mu > \hat{\mu}$ is the KKT point of original problem.

The stationary point of penalty function $\phi(x; \mu)$ is defined as $\hat{x}\in\mathbb{R}^n$ such that all the directional derivatives are greater or equal to zero, i.e., 

$$ D(\phi_1(\hat{x}; \mu); p) \geq 0, \quad p \in \mathbb{R}^n. $$


## Augmented Lagrangian method

Motivation: Whether we can alter the function $Q(x; \mu_k)$ to make the approximated minimizers more nearly satisfy the equality constraint $c_i(x)=0$, even for moderate values of $\mu_k$.

By using some techniques one could convert the inequality constraints into equality constraints, thus, we focus on equality constraints onlyl.

Note that the quadratic penalty method gives relation

$$ c_i(x_k) \approx - \frac{\lambda_i^*}{\mu_k}, $$

thus $c_i(x_k)\rightarrow 0$ as $\mu_k \rightarrow \infty$. However, we can add another term such that

$$ c_i(x_k) \approx - \frac{1}{\mu_k}(\lambda_i^* - \lambda_i^k), $$

which gives the augmented Lagrangian function

$$ \mathcal{L}_A(x, \lambda_k; \mu_k) = f(x) - \sum_{i\in\mathcal{E}}\lambda_i^k c_i(x) + \frac{\mu_k}{2} \sum_{i\in\mathcal{E}}c_i^2(x). $$

From the proof of quadratic penatly method we have 

$$ \nabla f(x^\ast) - A(x^*)^T \lambda^* = 0, \quad A(x)^T = [\nabla c_i(x)]_{i\in\mathcal{E}}. $$

From the optimal condition for augmented Lagrangian we see that

$$ 0 \approx \nabla f(x_k) - \sum_{i\in\mathcal{E}}[\lambda_i^k - \mu_k c_i(x_k)] \nabla c_i(x_k), $$

which implies $\lambda_i^* \approx \lambda_i^k - \mu_k c_i(x_k)$. This satisfies our goal set up before.

The relation between solution of original problem and augmented Lagrangian method is

local solution of original problem + LICQ + second order sufficient condition $\Rightarrow$ strict local minimizer of augmented Lagrangian for $\mu > \hat{\mu}$.


------------------------------------------------------------------------------------

# Active-set Method for Nonlinear Programming (SQP)

One of the most effective methods for nonlinearly constrained optimization generates steps by solving quadratic subproblem with updated active set. This approach is also called Sequential Quadratic Programming (SQP).

SQP methods are most efficient if the number of active constraints is nearly as large as the number of variables, i.e., the number of free variables is relatively small.

## Motivation of SQP

The idea behind the SQP is to model the constraint problem at the current iterate $x_k$ by a quadratic programming subproblem, then use the minimizer of this subproblem to define a new iterate $x_{k+1}$.

Take the equality-constrained problem for example, consider

$$ \min \, f(x) \quad s.t. \quad c(x) = 0, $$

the first-order (KKT) condition is

$$ F(x,\lambda) = \begin{bmatrix} \nabla f(x) - A(x)^T \lambda \\ c(x) \end{bmatrix} = 0, $$

where $A(x)$ is the Jacobian matrix of the constraints.

The Newton step to solve this nonlinear equation gives

$$ \begin{bmatrix} \nabla^2_{xx} \mathcal{L}_k & -A_k^T \\ A_k & 0 \end{bmatrix} \begin{bmatrix} p_k \\ p_{\lambda} \end{bmatrix} = \begin{bmatrix} -\nabla f_k + A_k^T \lambda_k \\ c_k \end{bmatrix}, $$

where $x_{k+1}=x_k + p_k$ and $\lambda_{k+1} = \lambda_k + p_{\lambda}$.

There is an alternative way to view the iteration as the solution of a quadratic program

$$ \min_p \, f_k + \nabla f_k^T p + \frac{1}{2} p^T \nabla_{xx}^2 \mathcal{L} p \quad s.t. \quad A_k p + c_k = 0. $$

The Newton point view facilites the analysis, while the SQP framework enables us to derive practical algorithms and extend the technique to the inequality-constrained case.

For inequalty-constrained problem

$$ \min \, f(x) \quad s.t. \quad c_i(x) = 0, i \in \mathcal{E}, \quad c_i(x) \geq 0, i \in \mathcal{I}, $$

we linearize both the inequality and equality constraints to obtain

$$ \min_p \, f_k + \nabla f_k^T p + \frac{1}{2} p^T \nabla_{xx}^2 \mathcal{L} p \quad s.t. \quad \nabla c_i(x_k)^T p + c_i(x_k) = 0, i \in \mathcal{E}, \quad \nabla c_i(x_k)^T p + c_i(x_k) \geq 0, i \in \mathcal{I}. $$

There are two types of active-set SQP methods:

- IQP (Inequality-cosntrained QP)

    A general IQP is solved at each iteration, with the twin goals to computing a step and generating an estimate of the optimal active set.

- EQP (Inequality-cosntrained QP)

    EQP first compute an estimate of the optimal active set, then solve an EQP to find the step. The working set is updated by rules based on Lagrange multiplier estimates, or by solving an auxiliary subproblem.

## Algorithm development of SQP

**Merit functions**

SQP methods often use a merit function to decide whether a trial step should be accepted. For example, the $l_1$ merit function

$$ \phi_1(x;\mu) = f(x) + \mu\|c(x)\|_1. $$

In a line search method, a step $\alpha_k p_k$ will be accepted if the following sufficient decrease condition holds:

$$ \phi_1(x_k+\alpha_k; \mu_k) \leq \phi_1(x_k; \mu_k) + \eta \alpha_k D(\phi_1(x;\mu); p_k), \quad \eta \in (0, 1). $$

However, many merit function may impede progress of an optimization algorithms, a phenomenon called Maratos effect. A possible cause is that the linear approximations to the constraints are not sufficiently accurate and we could overcome this via a second-order correction.

**Line search VS Trust region**

When $\nabla_{xx}^2 \mathcal{L}$ does not being positive definite on the tangent space of the active constraints, line search methods either replace it by a positive definite approximation of modify $\nabla_{xx}^2 \mathcal{L}$ during the process of matrix factorization. There modification may introduce unwanted distortions in the model. 

For the trust region method, the inclusion of trust region may case the subproblem to be infeasible.

A appropriate viewpoint is that there is no reason to satisfy the linear constraints exactly at every step. There are three classes for the method:

- relaxation method: relax $A_k p + c_k = 0$ into $A_k p + c_k = r_k$

- penalty method: move the linear constraints into the objective function in the form of an $l_1$ penalty term (S $l_1$ QP)

- filter method: first, a linear program ($f_k + \nabla f_k^T p$) is solved to identify a working set $\mathcal{W}$, then a EQP is solved with the working set $\mathcal{W}$. (SLQP)

-----------------------------------------------------------------------------------


# Interior-point Method for Nonlinear Programming

Numerical experience indicates that interior-point methods are often faster than active-set SQP methods on large problems, particularly when the number of free variable is large.

The term ""interior point" derives from the fact that early barrier methods did not use slacks and assumed that the initial point $x_0$ is feasible with respect to the inequality constraints. However, modern interior-point method are infeasible and remain interior only with repsect to the constraints $x,z \geq 0$.

## Derivation of interior-point method

The first derivation comes from the KKT condition of the constrained optimization. Similar to the discussion for linear (quadratic) programming, we consider the perturbed KKT conditions for a sequence of positive parameters $\lbrace \mu_k \rbrace$ that converge to zero, while maintaining $s, z>0$, which gives the primal-dual central path $(x(\mu), s(\mu), y(\mu), z(\mu))$,

$$
\begin{align*}
    \nabla f(x) - A_E^T(x) y - A_I^T(x) z &= 0, \\
    Sz - \mu e & = 0, \\
    c_E(x) &= 0, \\
    c_I(x) - s &= 0, \\
    s, z & \geq 0.
\end{align*}
$$

The second derivation associates with the barrier problem

$$
\begin{align*}
    \min_{x,s} & \quad f(x) - \mu \sum_{i=1}^m \log s_i, \\
    s.t., & \quad c_E(x) = 0, \\
    & \quad c_I(x) - s = 0.
\end{align*}
$$

One could verify that the KKT condition of this barrier problem coincide with the perturbed KKT system.

## Algorihtm development of interior-point method

After obtaining the perturbed KKT conditions, we need to solve this nonlinear system. Applying Newton's method we have a primal-dual system as

$$
\begin{bmatrix}
    \nabla^2_{xx} \mathcal{L} & 0 & -A_E^T(x) & -A_I^T(x) \\
    0 & Z & 0 & S \\
    A_E(x) & 0 & 0 & 0 \\
    A_I(x) & -I & 0 & 0
\end{bmatrix}
\begin{bmatrix}
    p_x \\ p_s \\ p_y \\ p_z
\end{bmatrix} = 
- \begin{bmatrix}
    \nabla f(x) - A_E^T(x) y - A_I^T(x)z \\
    Sz - \mu e \\
    c_E(x) \\
    c_I(x) - s
\end{bmatrix}
$$

with the Lagrange function $\mathcal{L}(x,s,y, z)=f(x) - y^T c_E(x) - z^T (c_I(x) - s)$.

The primal-dual system often be written in the symmetric form as

$$
\begin{bmatrix}
    \nabla^2_{xx} \mathcal{L} & 0 & A_E^T(x) & A_I^T(x) \\
    0 & \Sigma & 0 & -I \\
    A_E(x) & 0 & 0 & 0 \\
    A_I(x) & -I & 0 & 0
\end{bmatrix}
\begin{bmatrix}
    p_x \\ p_s \\ -p_y \\ -p_z
\end{bmatrix} = 
- \begin{bmatrix}
    \nabla f(x) - A_E^T(x) y - A_I^T(x)z \\
    z - \mu S^{-1} e \\
    c_E(x) \\
    c_I(x) - s
\end{bmatrix}
$$

where $\Sigma = S^{-1} Z$.

**Error function**

The error function based on the perturbed KKT system is defined as

$$ E(x,s,y,z; \mu) = \max\{\|\nabla f(x) - A_E(x)^T y - A_I(x)^T z\|, \|Sz - \mu e\|, \|c_E(x)\|, \|c_I(x) - s\| \}, $$

which is used to control the accuracy of the subproblem.

**Merit function**

We may use, for example, an exact merit function of the form

$$ \phi_{\nu}(x,s) = f(x) - \mu \sum_{i=1}^m \log s_i + \nu \|c_E(x)\| + \nu \|c_I(x) - s\|, $$

to determine whether a step is productive and should be accepted.

### Line search VS Trust-region

For line seach category, we add a line search to control the rate of decrease in slack $s$ and multipliers $z$, and introduce modifications in the primal-dul system when negative curvature is encountered.

For trust-region category, we start with the barrier interpretation, and then design an SQP method tailored for the structure of the barrier problem.

