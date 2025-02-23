---
layout: splash
author_profile: false
---

<h1> First-Order Methods in Optimization (Amir Beck) </h1>

<h2>Content</h2>

- [Preliminary](#preliminary)

- [Subgradient](#subgradient)

- [Conjugate Functions](#conjugate-functions)

- [Smoothness and Strong Convexity](#smoothness-and-strong-convexity)

- [Proximal Operator](#proximal-operator)

- [Subgradient Method](#subgradient-method)

- [Mirror Descent](#mirror-descent)

- [The Proximal Gradient Method](#the-proximal-gradient-method)

- [Block Proximal Gradient Method](#block-proximal-gradient-method)

- [Dual-Based Proximal Gradient Methods](#dual-based-proximal-gradient-methods)

- [The Generalized Conditional Gradient Method](#the-generalized-conditional-gradient-method)

- [Alternating Minimization](#alternating-minimization)

- [ADMM](#admm)

------------------------------------------------------------------

# Preliminary

## Fact : Linear transformations
All linear transformation from $\mathbb{R}^{m\times n}$ to $\mathbb{R}^k$ have
the form
 
$$
\mathcal{A}(X) = \begin{pmatrix} \text{Tr}(A_1^TX) \\ \text{Tr}(A_2^TX) \\ \vdots \\ \text{Tr}(A_k^TX) \end{pmatrix}, \quad A_i \in \mathbb{R}^{m \times n}.
$$

Note: this maybe useful for matrix calculus.

Adjoint transformations of $\mathcal{A}(X)$ 

$$
\langle y, \mathcal{A}(X) \rangle = \langle \mathcal{A}^T(y), X \rangle, \quad \mathcal{A}^T(y)=\sum_{i=1}^k y_i A_i.
$$

## Fact : identity

$$
\max\{a_1, a_2, \cdots, a_k\}= \max_{\lambda\in \Delta_k} \sum_{i=1}^k \lambda_i a_i, \quad \Delta_k = \{x\in\mathbb{R}^n : x \geq 0, e^T x=1\}.
$$

Double maximization

$$
\max_{x\in \mathbb{E}} \left\{ \langle y, x \rangle - \frac{1}{2}\|x\|^2 \right\} = \max_{\alpha \geq 0} \max_{\|x\| = \alpha} \left\{ \langle y, x \rangle - \frac{1}{2}\alpha^2 \right\} 
$$

Double minimization

$$
\min_{u\in \mathbb{E}} \left\{ g(\|u\|) + \frac{1}{2}\|u -x\|^2 \right\} = \min_{\alpha \geq 0} \min_{u\in\mathbb{E},\|u\| = \alpha} \left\{ g(\alpha) + \frac{1}{2}\alpha^2 - \langle u, x\rangle - \frac{1}{2}\|x\|^2 \right\} 
$$

## Fact: duality
Strong duality: 

- Step 0: introduce another variable to convert non-constraint problem into constraint problem
- Step 1: remove constraint through Lagrange function
- Step 2: convert non-convex problem into convex problem

(some examples can be found in Chapter 6)

## Support function
Def: Let $C\subseteq \mathbb{E}$ be nonempty set, then the support function of $C$ is the function $\sigma_C: \mathbb{E}^* \rightarrow (-\infty, \infty]$ given by

$$
\sigma_C(y) = \max_{x\in C} \langle y, x \rangle.
$$

Lemma:
Let $A, B\subseteq \mathbb{E}$ be nonempty closed and convex sets. Then $A=B$ if and only if $\sigma_A=\sigma_B$.
Let $A\subseteq \mathbb{E}$ be nonempty, then $\sigma_A=\sigma_{closed(A)}=\sigma_{conv(A)}$.

-----------------------------------

# Subgradient

## Subdifferential
- Theorem 1: $\text{int}(\text{dom}(f)) \subseteq \text{dom}(\partial f)$  (convex function must be suddifferentiable at any point in the interior of their domain)

- Theorem 2: $\text{ri}(\text{dom}(f)) \subseteq \text{dom}(\partial f)$. 


## Subgradient: max formula
Let $f: \mathbb{E} \rightarrow (-\infty, \infty]$ be a proper convex function.
Then for any $x\in \text{int}(\text{dom}(f))$ and $d\in\mathbb{E}$,

$$
f'(x; d) = \max\{\langle g, d \rangle: g\in \partial f(x)\} = \sigma_{\partial f(x)}(d)
$$

If $f$ is differentiable at $x$, then $f'(x;d)=\langle \nabla f(x), d \rangle$.


## Subgradient: Lipschitz Continuity and Boundedness
Let $f: \mathbb{E} \rightarrow (-\infty, \infty]$ be a proper and convex function. Suppose that $X\subseteq \text{int}(\text{dom}f)$. 

Consider the following two claims:

- (a) $\|f(x) - f(y)\| \leq L \|x-y\|$ for any $x, y \in X$.

- (b) $\|g\|_* \leq L$ for any $g\in \partial f(x), x\in X$

Then (b) $\Rightarrow$ (a), if $X$ is open, (b) $\Leftrightarrow$ (a).


## Subgradient: Optimality Condition

**Fermat's optimality condition:** 

Let $f: \mathbb{E}\rightarrow (-\infty, \infty]$ be a proper convex function. Then

$$
x^* \in \text{argmin} \{f(x):x\in \mathbb{E}\} \Longleftrightarrow 0 \in \partial f(x^*)
$$ 

**Composite Model:** 

Let $f: \mathbb{E}\rightarrow (-\infty, \infty]$ be a proper function, and let $g: \mathbb{E}\rightarrow (-\infty, \infty]$ be a proper convex function such that $\text{dom}(g) \subseteq \text{int}(\text{dom}(f))$. Consider the problem

$$
(P) \quad \min_{x\in \mathbb{E}} f(x) + g(x)
$$

(a) (necessary condition) If $x^* \in \text{dom}(g)$ is a local optimal solution of $(P)$ and $f$ is differentiable at $x^*$, then

$$
-\nabla f(x^*) \in \partial g(x^*)
$$ 

(b) (IFF condition for convex problems) Suppose that $f$ is convex. If $f$ is differentiable at $x^* \in \text{dom}(g)$, then $x^* $ is a global optimal solution of $(P)$ if and only if $-\nabla f(x^* ) \in \partial g(x^* )$.

**KKT conditions:**

Consider the minimization problem

$$ \min f(x) \quad s.t. \quad g_i(x) \leq 0, \quad i=1, 2, \ldots, m, $$

where $f, g_1, \ldots, g_m: \mathbb{E} \rightarrow \mathbb{R}$ are real-valued convex functions.

(a) Let $x^*$ be an optimal solution, and assume the Slater's condition is satisfied. Then there exist $\lambda_1, \cdots, \lambda_m \geq 0$ for which

$$
\begin{align*}
& 0 \in \partial f(x^*) + \sum_{i=1}^m \lambda_i \partial g_i(x^*) \\
& \lambda_i g_i(x^*) = 0, \quad i=1, 2, \cdots, m.
\end{align*}
$$

(b) If $x^*\in \mathbb{E}$ satisfies conditions above for some $\lambda_1, \cdots, \lambda_m \geq 0$, then it is an optimal solution of the minimization problem.

------------------------------------------

# Conjugate Functions
Def: Let $f:\mathbb{E}\rightarrow [-\infty, \infty]$ be an extended real-valued function. The function $f^* :\mathbb{E}^*\rightarrow [-\infty, \infty]$, defined by

$$
f^*(y) = \max_{x\in \mathbb{E}} \{\langle y, x \rangle - f(x) \}, y \in \mathbb{E}^*
$$

is called the conjugate function of $f$.

Let $f:\mathbb{E}\rightarrow (-\infty, \infty]$ be an extended real-valued function, the conjugate function $f^*$ is closed and convex.

**Fenchel's inequality:**

Let $f:\mathbb{E}\rightarrow (-\infty, \infty]$ be an extended real-valued proper function. Then for any $x\in \mathbb{E}$ and $y\in \mathbb{E}^*$, 

$$
f(x) + f^*(y) \geq \langle y, x \rangle.
$$

**Fenchel's Duality theorem：**

Let $f, g:\mathbb{E}\rightarrow (-\infty, \infty]$ be proper convex functions. If $\text{ri}(\text{dom}f) \cap \text{ri}(\text{dom}g) \neq \emptyset$, then 

$$
\min_{x\in\mathbb{E}} \{f(x)+g(x)\} = \max_{y\in \mathbb{E}^*} \{-f^*(y) - g^*(-y)\},
$$

and the maximum in the right-hand side problem is attained whenever it is finite.


## Conjugate subgradient theorem

Let $f:\mathbb{E}\rightarrow (-\infty, \infty]$ be proper and convex. The following two claims are equivalent for any $x\in\mathbb{E}, y\in\mathbb{E}^* $:

- (a) $\langle x,y \rangle = f(x) + f^*(y)$.
- (b) $y\in \partial f(x)$.

If in addition $f$ is closed, then (a) and (b) are equivalent to

- (c) $x\in \partial f^*(y)$.

**Second formulation**

Let $f:\mathbb{E}\rightarrow (-\infty, \infty]$ be a proper closed convex function. Then for any $x\in \mathbb{E}, y\in \mathbb{E}^*$, 

$$
\partial f(x) = \text{argmax}_{\tilde{y}\in\mathbb{E}^*}  \{ \langle x, \tilde{y} \rangle - f^*(\tilde{y})\}
$$

and

$$
\partial f^*(y) = \text{argmax}_{\tilde{x}\in\mathbb{E}}  \{ \langle y, \tilde{x} \rangle - f(\tilde{x})\}
$$

------------------------------------------------------

# Smoothness and Strong Convexity

DEF: Let $L\geq 0$. A function $f:\mathbb{E}\rightarrow (-\infty, \infty]$ is said to be $L$-smooth over a set $D\subseteq \mathbb{E}$ if it is differentiable over $D$ and satisfies 

$$
\|\nabla f(x) - \nabla f(y) \|_{*} \leq L\|x - y\| \quad \text{for all } x, y\in D.  
$$

**Descent Lemma**

Let $f:\mathbb{E}\rightarrow (-\infty, \infty]$ be an $L$-smooth function ($L \geq 0$) over a given convex set $D$. Then for any $x,y\in D$,

$$
f(y) \leq f(x) + \langle \nabla f(x), y-x \rangle + \frac{L}{2}\|x-y\|^2.
$$ 

DEF: A function $f:\mathbb{E}\rightarrow (-\infty, \infty]$ is called $\sigma$-strongly convex for a given $\sigma > 0$ if dom($f$) is convex and the following inequality holds for any $x,y\in \text{dom}(f)$ and $\lambda\in [0, 1]$:

$$
f(\lambda x + (1-\lambda)y) \leq \lambda f(x) +  (1-\lambda)f(y) - \frac{\sigma}{2}\lambda(1-\lambda)\|x-y\|^2.
$$

**Conjugate Correspondence Theorem**

Let $\sigma > 0$. Then

- (a) If $f:\mathbb{E}\rightarrow \mathbb{R}$ is a $\frac{1}{\sigma}$-smooth convex function, then $f^* $ is $\sigma$-strongly convex w.r.t the dual norm $\|\cdot\|_{*}$

- (b) If $f:\mathbb{E}\rightarrow (-\infty, \infty]$ is a proper closed $\sigma$-strongly convex function, then $f^* :\mathbb{E}^ *\rightarrow \mathbb{R}$ is $\frac{1}{\sigma}$-smooth.

--------------------------------------


# Proximal Operator

DEF: Given a function $f:\mathbb{E}\rightarrow (-\infty, \infty]$, the proximal mapping of $f$ is the operator given by

$$
\text{prox}_f(x) = \text{argmin}_{u\in \mathbb{E}} \left\{ f(u) + \frac{1}{2}\|u-x\|^2 \right\} \quad \text{for any } x \in \mathbb{E}.
$$

**Nonexpansivity of the prox operator**

$$
\langle x - y, \text{prox}_f(x) - \text{prox}_f(y) \rangle \geq \| \text{prox}_f(x) - \text{prox}_f(y)  \|^2,
$$

$$
\|\text{prox}_f(x) - \text{prox}_f(y) \|\leq \|x-y\|.
$$

**Thm: proximal and projection**

Let $C\subseteq \mathbb{E}$ be nonempty. Then 

$$ \text{prox}_{\delta_{C}}(x)=P_C(x)=\text{argmin}_{u\in C} \|u-x\|^2. $$


**Thm: First prox theorem**

Let $f:\mathbb{E}\rightarrow (-\infty, \infty]$ be a proper closed and convex function. Then $\text{prox}_f(x)$ is a singleton for any $x\in \mathbb{E}$.

**Thm: Second prox theorem**

Let $f:\mathbb{E}\rightarrow (-\infty, \infty]$ be a proper closed and convex function. Then for any $x,u\in\mathbb{E}$, the following three claims are equivalent:
- (a) $u=\text{prox}_f(x)$.
- (b) $x - u \in \partial f(u)$.
- (c) $\langle  x-u, y-u \leq f(y) - f(u) \rangle$ for any $x\in \mathbb{E}$.

**Moreau decomposition**

Let $f:\mathbb{E}\rightarrow (-\infty, \infty]$ be a proper closed and convex, then for any $x\in\mathbb{E}$,

$$
\text{prox}_f(x) + \text{prox}_{f^*}(x) = x.
$$

## The Moreau Envelope
Given a proper closed convex function $f:\mathbb{E}\rightarrow (-\infty, \infty]$ and $\mu>0$, then Moreau envelope of $f$ is defined as 

$$
M_f^\mu(x) = \min_{u\in\mathbb{E}} \left\{ f(u)+\frac{1}{2}\|x-u\|^2\right\} 
$$


## Proximal of Spectral function
Spectral prox formula over $\mathbb{S}^n$

Let $F:\mathbb{S}^n \rightarrow (-\infty, \infty]$ be given by $F=f\circ\lambda$, where $f:\mathbb{R}^n \rightarrow (-\infty, \infty]$ is a permutation symmetric proper closed and convex function. Let $X\in \mathbb{S}^n$, and suppose that $X=U\text{diag}(\lambda(X))U^T$ where $U\in \mathbb{O}^n$. Then 

$$
\text{prox}_F (X) = U \text{diag} (\text{prox}_f (\lambda(X)))U^T.
$$

------------------------------

# Subgradient Method

One substantial difference between the gradient and subgradient methods is that the direction of minus the subgradient is not necessarily a descent direction.

## Projected Subgradient Method

For convex optimization problem

$$
\min \{f(x) : x\in C \}
$$

### Algorithm
Initialization: pick $x^0\in C$ arbitrarily

General step: for $k=0, 1, \cdots$
- pick a step size $t_k > 0$ and a subgradient $f'(x^k)\in \partial f(x^k)$
- set $x^{k+1}=P_C(x^k - t_k f'(x^k))$


### Fundamental inequality

$$
\|x^{k+1} - x^*\|^2 \leq \|x^k - x^*\|^2 - 2t_k (f(x^k) - f_{\text{opt}}) + t_k^2 \|f'(x^k)\|^2
$$

Step size: minimize the right hand side of the inequality above

$$
\sum_{n=0}^k t_n (f(x^n) - f_{\text{opt}}) \leq \frac{1}{2} \|x^0 - x^*\|^2 + \frac{1}{2} \sum_{n=0}^k t_n^2 \|f'(x^n)\|^2
$$

Dynamic Step size: 

move $\sum_{n=0}^k t_n$ into right hand side through $f_{\text{best}}^k:= \min_{n=0, 1, \cdots, k} f(x^n)$ to make it decay to 0

## Variants

- Strongly Convex version :  $O(1/\sqrt{k}) \rightarrow O(1/k)$. 
- Stochastic version : pick $g^k\in \mathbb{E}$ such that $E(g^k \| x^k) \in \partial f(x^k)$ 
- Dual version

-------------------------------------------

# Mirror Descent

The method is essentially a generalization of the projected subgradient method to the non-Euclidean setting. 

## Motivation
Recall the general update step of the projected subgradient method

$$
x^{k+1} = P_C(x^k - t_k f'(x^k)), \quad f'(x^k)\in \partial f(x^k).
$$

Reformulate the update step as the minimization of a linearization term plus a quadratic proximity term

$$
x^{k+1} = \text{argmin}_{x\in C} \left\{ f(x^k) + \langle f'(x^k), x-x^k \rangle + \frac{1}{2t_k}\|x-x^k\|^2 \right\}
$$

Replace the the Euclidean "distance" $\frac{1}{2}\|x-y\|^2$ by a different distance we get the mirror descent method.

## Bregman distance
Let $\omega:\mathbb{E}\rightarrow (-\infty, \infty]$ be a proper closed and convex function that is differentiable over $\text{dom}(\partial \omega)$. The Bregman distance associated with $\omega$ is the function $B_{\omega}: \text{dom}(\omega) \times \text{dom}(\partial \omega) \rightarrow \mathbb{R}$ given by

$$
B_{\omega}(x,y) = \omega(x) - \omega(y) - \langle \nabla \omega(y), x-y \rangle.
$$

Replacing the term $\frac{1}{2}\|x-x^k\|^2$ by a Bregman distance one has

$$
\begin{align*}
x^{k+1} &= \text{argmin}_{x\in C} \left\{ f(x^k) + \langle f'(x^k), x-x^k \rangle + \frac{1}{t_k}B(x-x^k) \right\} \\
&\text{argmin}_{x\in C} \left\{ \langle t_kf'(x^k) - \nabla \omega(x^k) , x \rangle  + \omega(x) \right\}
\end{align*}
$$

## Composite Method
Consider problem

$$
\min_{x\in\mathbb{E}} \{F(x):=f(x) + g(x)\}
$$

Instead of linearizing both $f$ and $g$, we will linearize $f$ and keep $g$ as it is. Then we pick $f'(x^k)\in \partial f(x^k)$ and set

$$
\begin{align*}
x^{k+1} &= \text{argmin}_{x} \left\{ \langle f'(x^k), x \rangle + g(x^k) + \frac{1}{t_k}B(x-x^k) \right\} \\
&=\text{argmin}_{x} \left\{ \langle t_kf'(x^k) - \nabla \omega(x^k) , x \rangle + t_kg(x)  + \omega(x) \right\}
\end{align*}
$$

------------------------------------------

# The Proximal Gradient Method

Consider the composite model

$$
\min_{x\in\mathbb{E}} \{F(x):=f(x) + g(x)\}
$$

where $g:\mathbb{E}\rightarrow (-\infty, \infty]$ is proper closed and convex, $f:\mathbb{E}\rightarrow (-\infty, \infty]$ is proper closed and $L_f$-smooth over $\text{int}(\text{dom}(f))$. $\text{dom}(f)$ is convex, $\text{dom}(g) \subseteq \text{int}(\text{dom}(f))$.

## Motivation

Recall the general update step of the projected gradient method for $\min\{f(x):x\in C\}$

$$
x^{k+1} = P_C(x^k - t_k  \nabla f(x^k))
$$

Reformulate the update step as the minimization of a linearization term plus a quadratic proximity term

$$
x^{k+1} = \text{argmin}_{x\in C} \left\{ f(x^k) + \langle \nabla f(x^k), x-x^k \rangle + \frac{1}{2t_k}\|x-x^k\|^2 \right\}
$$

It is natural to define the next iterate as the minimizer of the sum of the linearization of $f$ around $x^k$, the nonsmooth function $g$ and a quadratic prox term:

$$
\begin{align*}
x^{k+1} &= \text{argmin}_{x\in \mathbb{E}} \left\{ f(x^k) + \langle \nabla f(x^k), x-x^k \rangle + g(x) + \frac{1}{2t_k}\|x-x^k\|^2 \right\}  \\
&= \text{argmin}_{x\in\mathbb{E}} \left\{ t_k g(x) + \frac{1}{2}\|x - (x^k - t_k \nabla f(x^k))\|^2 \right\} \\
&= \text{prox}_{t_kg}(x^k - t_k \nabla f(x^k))
\end{align*}
$$ 


## Toolbox

Define prox-grad operator 

$$
T^{f,g}_{L}(x):= \text{prox}_{\frac{1}{L}g} \left( x - \frac{1}{L} \nabla f(x) \right)
$$

Define gradient mapping

$$
G^{f,g}_L(x) := L(x - T^{f,g}_L(x))
$$

The update step can be rewritten as 

$$
x^{k+1} = x^k - \frac{1}{L_k} G_{L_k}(x^k)
$$

**Remark**
The gradient mapping is a generalization of the "usual" gradient operator $x\mapsto \nabla f(x)$ in the sense that they coincide when $g\equiv 0$ and that, for a general $g$, the points in which the gradient mapping vanishes are the stationary points of the problem of minimizing $f+g$. We can think of the quantity $\|G_L(x)\|$ as an "optimality measure" in the sense that it is always nonnegative, and equal to zero if and only if $x$ is a stationary point.

**Sufficient Decrease Lemma**

Suppose $g:\mathbb{E}\rightarrow (-\infty, \infty]$ is proper closed and convex, $f:\mathbb{E}\rightarrow (-\infty, \infty]$ is proper closed and $L_f$-smooth over $\text{int}(\text{dom}(f))$. $\text{dom}(f)$ is convex, $\text{dom}(g) \subseteq \text{int}(\text{dom}(f))$. Let $F=f+g$ and $T_L\equiv T_L^{f,g}$. Then for any $x\in \text{int}(\text{dom}(f))$ and $L\in (\frac{L_f}{2}, \infty)$, the following inequality holds:

$$
F(x) - F(T_L(x)) \geq \frac{L-\frac{L_f}{2}}{L^2} \left\| G_L^{f,g}(x) \right\|^2
$$

## Step size strategy
- Constant $L_k = \bar{L} \in (\frac{L_f}{2}, \infty)$ for all $k$.
- Backtracking procedure: while $F(x^k) - F(T_{L_k}(x^k)) \leq \frac{\gamma}{L_k} \|G_{L_k}(x^k)\|^2$, set $L_k=\eta L_k$.

## Convergence analysis
- The non-convex case: $O(1/\sqrt{k})$
- The convex case: $O(1/k)$
- The strongly convex case: $O(q^k)$ for $q\in (0, 1)$.

## Improvement

### FISTA (fast iterative shrinkage-thresholding algorithm)

How to accelerate the method in order to obtain a rate of $O(1/k)$ in function values?

Assume that $f:\mathbb{E}\rightarrow \mathbb{R}$ is convex and it is $L_f$-smooth, $g:\mathbb{E}\rightarrow (-\infty, \infty]$ is proper and convex. 
By add the history information like conjugate gradient type method, one has
- set $x^{k+1}=\text{prox}_{\frac{1}{L_k}g} \left( y^k - \frac{1}{L_k}\nabla f(y^k)\right) $ 
- set $t_{k+1}=\frac{1+\sqrt{1+4t_k^2}}{2}$
- compute $y^{k+1}=x^{k+1}+ (\frac{t_k - 1}{t_{k+1}})(x^{k+1}-x^k)$.

Note: no easy motivation for the design of step size here.

#### variant
- MFISTA: monotone version of FISTA, make sure that the function value is always descent.
- Weight FISTA: replace the endowed inner product to $Q$-inner product, $\langle x, y \rangle=x^TQ y$
- Restarted FISTA
- V-FISTA: strongly convex version

### S-FISTA (smoothing)

Consider optimization
$$
\min_{x\in\mathbb{E}} \{H(x):= f(x)+h(x)+g(x)\}
$$

$h$ is real-valued and convex but not smooth, the target is to find a smooth approximation of $h$, say $\tilde{h}$ and solve the problem via FISTA with smooth and nonsmooth parts taken as $(f+\tilde{h}, g)$.

#### Toolbox

Def: smooth-approximation
A convex function $h:\mathbb{E}\rightarrow \mathbb{R}$ is called $(\alpha, \beta)$-smoothable $(\alpha, \beta > 0)$ if for any $\mu>0$ there exists a convex differentiable function $h_{\mu}:\mathbb{E}\rightarrow \mathbb{R}$ such that the following holds:
- $h_{\mu}(x) \leq h(x)\leq h_{\mu}(x) + \beta \mu$ for all $x\in \mathbb{E}$.
- $h_{\mu}$ is $\frac{\alpha}{\mu}$-smooth.

The function $h_{\mu}$ is called a $\frac{1}{\mu}$-smooth approximation for $h$ with parameters $(\alpha, \beta)$.

A natural $\frac{1}{\mu}$-smooth approximation of a given real-valued convex function is its Moreau envelope $M_h^\mu$.

Thm: Let $h:\mathbb{E}\rightarrow \mathbb{R}$ be a convex function satisfying 
$$
|h(x)-h(y)| \leq l_h \|x-y\| 
$$

for all $x,y\in\mathbb{E}$. Then for any $\mu>0$, $M^\mu_h$ is a $\frac{1}{\mu}$-smooth approximation of $h$ with parameters $(1, \frac{l_h^2}{2})$.


### Non-Euclidean Proximal Gradient Methods

#### The Non-Eculidean Gradient Method
Recall the general update step of the gradient method for $\min\{f(x):x\in \mathbb{E}\}$

$$
x^{k+1} = x^k - t_k  \nabla f(x^k)
$$

Actually $\nabla f(x_k) \in \mathbb{E}^* $, we can replace it with a "primal counterpart" in $\mathbb{E}$.
For any vector $a\in \mathbb{E}^*$, we define the set of primal counterpart of $a$ as 

$$
\Lambda_a = \text{argmax}_{v\in\mathbb{E}} \{\langle a, v \rangle : \|v\|\leq 1\}
$$  

Note that by conjugate subgradient theorem, 

$$
\Lambda_a = \partial h(a), \quad h(\cdot)=\|\cdot\|_*
$$

Algorithm
- pick $\nabla f(x^k)^\dagger \in \Lambda_{\nabla f(x^k)}$ and $L_k > 0$
- set $x^{k+1}=x^k - \frac{\|\nabla f(x^k)\|_*}{L_k} \nabla f(x^k)^\dagger$

#### Non-Eculidean Proximal Gradient Method
Consider the composite model

$$
\min_{x\in\mathbb{E}} \{F(x):=f(x) + g(x)\}
$$

where $g:\mathbb{E}\rightarrow (-\infty, \infty]$ is proper closed and convex, $f:\mathbb{E}\rightarrow (-\infty, \infty]$ is proper closed and $L_f$-smooth over $\text{int}(\text{dom}(f))$. $\text{dom}(f)$ is convex, $\text{dom}(g) \subseteq \text{int}(\text{dom}(f))$.

Recall the general update step of the proximal gradient method

$$
x^{k+1} = \text{argmin}_{x\in \mathbb{E}} \left\{ f(x^k) + \langle \nabla f(x^k), x-x^k \rangle + g(x) + \frac{L_k}{2}\|x-x^k\|^2 \right\}
$$

Replace the half-squared Euclidean distance with a Bregman distance $B_{\omega}$

$$
x^{k+1} = \text{argmin}_{x\in \mathbb{E}} \left\{ f(x^k) + \langle \nabla f(x^k), x-x^k \rangle + g(x) + L_k B_{\omega}\|x-x^k\|^2 \right\}
$$

------------------------------------------------------

# Block Proximal Gradient Method

Consider model

$$
\min_{x_1\in\mathbb{E}_1, \cdots, x_p\in\mathbb{E}_p} \left\{F(x_1, \cdots, x_p) = f(x_1, \cdots, x_p) + \sum_{j=1}^p g_j(x_j) \right\}
$$

Foy any $i\in\{1, 2, \cdots, p\}$,  we define $\mathcal{U}_i:\mathbb{E}_i \rightarrow \mathbb{E}$ to be the linear transformation given by

$$
\mathcal{U}_i(d) = (0, \cdots, 0, d, 0, \cdots, 0), \quad d\in \mathbb{E}_i
$$

where $d$ lies in the $i$-th block.


## Cyclic Block Proximal Gradient Method
We successively pick a block in a cyclic manner and perform a prox-grad step w.r.t the chosen block. Each iteration of the CBPG method involves $p$ subiterations, denote

$$
x^{k, i} = \sum_{j=1}^i \mathcal{U}_j(x_j^{k+1}) + \sum_{j=i+1}^p \mathcal{U}_j(x_j^k),
$$

and define partial prox-grad mapping as

$$
T^i_L(x) = \text{prox}_{\frac{1}{L}g_i} \left( x_i - \frac{1}{L}\nabla_i f(x)  \right),
$$

then we have algorithm
- set $x^{k, 0} = x^k$
- for $i=1, \cdots, p$, compute

    $$ x^{k,i} = x^{k, i-1} + \mathcal{U}_i(T^i_{L_i}(x^{k, i-1}) - x_i^{k, i-1}). $$

- set $x^{k+1}=x^{k,p}$

The convergence analysis comes in the same manner of Proximal Gradient method.

### Randomized block proximal gradient method

Pick $i_k\in\{1, 2, \cdots, p\}$ randomly via a uniform distribution and set

$$
x^{k+1} = x^{k} + \mathcal{U}_{i_k}(T^{i_k}_{L_{i_k}}(x^{k}) - x_{i_k}^{k}).
$$

-----------------------------------------------------

# Dual-Based Proximal Gradient Methods

Consider model 

$$
\min_{x\in\mathbb{E}} \quad \{f(x) + g(\mathcal{A}(x)) \}
$$

where $f:\mathbb{E}\rightarrow (-\infty, \infty]$ is proper closed and $\sigma$-strongly convex ($\sigma>0$), $g:\mathbb{V}\rightarrow (-\infty, \infty]$ is proper closed and convex, $\mathcal{A}:\mathbb{E} \rightarrow \mathbb{V}$ is a linear transformation.

## Motivation
To construct the dual problem, rewrite it into

$$
\min_{x, z} f(x)+g(z) \quad s.t. \mathcal{A}(x) - z = 0.
$$

Then the Lagrange is 

$$
L(x, z; y) = f(x) + g(z) - \langle y, \mathcal{A}(x) - z \rangle = f(x) + g(z) - \langle\mathcal{A}^T(y), x \rangle + \langle y, z \rangle
$$

Dual problem

$$
\max_{y\in\mathbb{V}} \quad \{g(y)=-f^*(\mathcal{A}^T(y)) - g^*(-y)\}
$$

## Algorithms
- Dual Proximal Gradient (dual representation)

- Dual Proximal Gradient (primal representation)

  Remark. The primal sequence $x^k=\text{argmax}_x \{ \langle x, \mathcal{A}^T(y^k) \rangle - f(x)\}$ are actually not necessarily feasible w.r.t. the primal problem  

- Fast Dual Proximal Gradient (dual/primal representation)

- Dual Block proximal Gradient Method

  Apply the dual method to problem $\min_{x\in \mathbb{E}} ( f(x) + \sum_{i=1}^p g_i(x) )$

------------------------------------------


# The Generalized Conditional Gradient Method

## Motivation

### Conditional Gradient
Consider the problem

$$
\min \{ f(x) : x\in C\}, \quad C \subseteq \mathbb{E},
$$

whose update step is 

$$
x^{k+1}=P_C(x^k - t_k \nabla f(x^k) ).
$$

We now consider an alternative approach that does not require the evaluation of the orthogonal projection, instead we compute the next step as a convex combination of the current iterate and a minimizer of a linearized version of the objective function over $C$.
- compute $p^k \in \text{argmax}_{p\in C} \langle p, \nabla f(x^k) \rangle$
- choose $t_k\in [0, 1]$ and set $x^{k+1}= x^k + t_k (p^k - x^k) = (1 - t_k) x^k + t_k p^k$.

### Generalized Conditional Gradient
Consider composite problem

$$
\min\quad \{ F(x) \equiv f(x) + g(x)\}
$$

follow the same manner we have algorithm

- compute $p^k \in \text{argmax}_{p\in \mathbb{E}} \langle p, \nabla f(x^k) \rangle + g(p) $
- choose $t_k\in [0, 1]$ and set $x^{k+1}= x^k + t_k (p^k - x^k) = (1 - t_k) x^k + t_k p^k$.

#### Toolbox
The analysis of the conditional gradient method relies on a different optimality measure, i.e., the conditional gradient norm.

$$
\begin{aligned}
S(x) &= \langle \nabla f(x), x-p(x) \rangle + g(x) - g(p(x))  \\
&= \max_{p\in\mathbb{E}} \{ \langle \nabla f(x), x-p \rangle + g(x) - g(p) \}  \\
&= \langle \nabla f(x), x \rangle + g(x) + g^*(-\nabla f(x))
\end{aligned}
$$

$S(\cdot)$ is an optimality measure in the sense that it is always nonnegative and is equal to zero only at stationary points.

--------------------------------------------

# Alternating Minimization

## Motivation 
Consider the problem with proper closed function $F$

$$
\min_{x_1\in\mathbb{E}_1, \cdots, x_p\in\mathbb{E}_p} F(x_1, x_2, \cdots, x_p),
$$

we successively pick a block in a cyclic manner and set the new value of the chosen block to be a minimizer of the objective w.r.t. the chosen block, i.e., 
- for $i=1, 2, \cdots, p$, compute

  $$
  x_i^{k+1} \in \text{argmin}_{x_i\in\mathbb{E}_i} F(x_1^{k+1}, \cdots, x_{i-1}^{k+1}, x_i, x_{i+1}^k, \cdots, x_p^k)
  $$


## Properties

### Well-defined
Define

$$
\mathcal{U}_i(d) = (0, \cdots, 0, d, 0, \cdots, 0), \quad d\in \mathbb{E}_i
$$

Lemma:
If $F$ is proper and closed and has bounded level sets, then the alternating minimization problem

$$
\min_{y\in\mathbb{E}_i} F(x + \mathcal{U}_i(y-x_i))
$$

possess minimization.

### Coordinate-wise minima and stationary point

A vector $x^* \in \mathbb{E}$ is a coordinate-wise minimum of a function $F:\mathbb{E}_1 \times \cdots \times \mathbb{E}_p \rightarrow (-\infty, \infty]$ if $x^* \in \text{dom}(F)$ and 

$$
F(x^*) \leq F(x^* + \mathcal{U}_i(y)) \text{ for all } i=1, \cdots,  p, y\in \mathbb{E}_i.
$$ 

Thm:
The limit points of the sequence generated by the alternating minimization method are coordinate-wise minima if the level set of $F$ are bounded and all the subproblem has unique minimizer.

**Remark:**
Even if convexity is assumed, coordinate-wise minima points are not necessarily stationary points of the objective function. One possible reason for this phenomena is that the stationary condition $0\in \partial F(x)$ does not decompose into separate conditions for each block. Hence, we consider a specific composite model for which we will be able to prove that coordinate-wise minima points are necessarily stationary points.

## Composite Model
Consider

$$
\min_{x_1\in\mathbb{E}_1, \cdots, x_p\in\mathbb{E}_p} \left\{ F(x_1, x_2, \cdots, x_p) = f(x_1, \cdots, x_p) + \sum_{j=1}^p g_j(x_j) \right\},
$$

For coordinate-wise minima $x^*$, for all $i=1, \cdots, p$,

$$
x^*_i \in \text{argmin}_{y\in\mathbb{E}_i} {f(x^* + \mathcal{U}_i (y-x^*_i)) + g_i(y)},
$$

then we have $-\nabla_i f(x^* ) \in \partial g_i(x^* )$, furthermore, $-\nabla f(x) \in \partial g(x^*)$. Thus, under some conditions coordinate-wise minimality $\Rightarrow$ stationarity. 
Besides, the uniqueness of the optimal solution to the subproblem can be removed if we assume convexity of the objective function.

--------------------------------------

# ADMM

ADMM can be treat as an application of dual proximal method for special optimization problem, which could be solved efficiently using alternating minimization.

## Motivation

Consider problem

$$
\min \{ H(x,z)\equiv h_1(x)+h_2(z) : Ax + Bz = c  \}
$$

The dual problem is

$$
\min_{y\in \mathbb{R}^m}  \{ h_1^*(-A^Ty) + h_2^*(-B^Ty) + \langle c, y \rangle \}
$$

Apply the proximal point method to the dual problem (also known as augumented Lagrangian method)

$$
y^{k+1} = \text{argmin}_{y\in\mathbb{R}^m} \left\{ h_1^*(-A^Ty) + h_2^*(-B^Ty) + \langle c, y \rangle + \frac{1}{2\rho} \|y-y^k\|^2 \right\}
$$

By fermat's optimality condition and conjugate subgradient theorem,

$$
\begin{align*}
y^{k+1} &= y^k + \rho(Ax^{k+1} + Bz^{k+1} - c) \\
 0 &\in A^T(y^k + \rho(Ax^{k+1} + Bz^{k+1} - c)) + \partial h_1(x^{k+1}) \\
 0 &\in B^T(y^k + \rho(Ax^{k+1} + Bz^{k+1} - c)) + \partial h_2(z^{k+1}) \\
\end{align*}
$$

which is satisfied if and only of $(x^{k+1}, z^{k+1})$ is a coordinate-wise minimum of the function

$$
\tilde{H}(x, z) := h_1(x) + h_2(z) + \frac{\rho}{2} \|Ax + Bz - c + \frac{1}{\rho}y^k \|^2 
$$

If we apply the alternating minimization method to above problem, then we get the ADMM method (alternating direction method of multipliers), i.e., 

$$
\begin{align*}
x^{k+1} &\in \text{argmin}_{x} \left\{ h_1(x) + \frac{\rho}{2} \|Ax+Bz^k - c + \frac{1}{\rho} y^k \|^2 \right\} \\
z^{k+1} &\in \text{argmin}_{z} \left\{ h_2(z) + \frac{\rho}{2} \|Ax^{k+1} + Bz - c + \frac{1}{\rho} y^k \|^2 \right\} \\
y^{k+1} &= y^k + \rho(Ax^{k+1} + Bz^{k+1} - c) \\
\end{align*}
$$

## Variant

### AD-PMM (alternating direction proximal method of multipliers)

Add proximal term to each subproblem in ADMM with different inner product we get

$$
\begin{align*}
x^{k+1} &\in \text{argmin}_{x} \left\{ h_1(x) + \frac{\rho}{2} \|Ax+Bz^k - c + \frac{1}{\rho} y^k \|^2 \right\} + \frac{1}{2} \|x-x^k\|_G^2 \\
z^{k+1} &\in \text{argmin}_{z} \left\{ h_2(z) + \frac{\rho}{2} \|Ax^{k+1} + Bz - c + \frac{1}{\rho} y^k \|^2 + \frac{1}{2} \|z-z^k\|^2_Q \right\} \\
y^{k+1} &= y^k + \rho(Ax^{k+1} + Bz^{k+1} - c) \\
\end{align*}
$$

By using the proximality term, the subproblem of ADMM can be simplified considerably by choosing $G=\alpha I - \rho A^TA$ with $\alpha \geq \rho\lambda_{\max}(A^TA)$ and $Q=\beta I - \rho B^T B$ with $\beta \geq \rho \lambda_{\max}(B^TB)$. For some particular examples, ADMM need to solve a linear systems while AD-PMM will only utilize matrix/vector multiplication.

--------------------------------------