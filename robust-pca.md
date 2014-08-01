Robust PCA
===

Robust PCA
---

Robust PCA: work with corrupted observations

### Classical PCA

Mimimize

$$||X-L||_2$$

subject to

$$\operatorname{rank}(L)\leq k$$

### Idealized Robust PCA

Additive decomposition: Decompose into sum of low-rank and sparse matrix, i.e. minimize

$$\operatorname{rank}(L)+\lambda \operatorname{card}(S)$$

subject to

$$X = L + S$$

Which is hard.

### Principal Component Pursuit (PCP)

A relaxation: minimize

$$||L||_*+\lambda||S||_1$$

subject to 

$$L+S=X$$

Convex Optimization
---

### Canonical form

Minimize $f(x)$ subject to

* $g_i(x)\leq0$
* $h_i(x)=0$

where $f,g_i$ are convex, $h_i$ are affine.

### Unconstrained Form

Minimize

$$f(x)+\sum\_{i=1}^mI\_-(g\_i(x))+\sum\_{i=1}^pI\_0(h\_i(x))$$

Where $I\_-(\cdot),I\_0(\cdot)$ are defined to be $\infty$ outside of constraints.

Lagrangian:

$$L(x,\lambda,\nu)=f(x)+\sum\_{i=1}^m\lambda\_ig\_i(x)+\sum\_{i=1}^n\nu\_ih\_i(x)$$

## Dual Problem

Maximize dual function:

$$d(\lamba,\nu)=\inf_xL(x,\lambda,\nu)$$

subject to $\lambda\geq0$. It's a lower bound on solution.

Gradient method:

$$\nu\leftarrow\nu+\alpha\nabla d(\nu)$$

where $\nabla$ is gradient operator, $\alpha$ is step length.


### Dual Ascent

Minimize $f(x)$ subject to $Ax=b$.
Lagrangian: $L(x,\nu)=f(x)+\nu^T(Ax-b)$.
Find solution to dual problem $\nu^*$.
Recover optimal: $\arg\min_xL(x,\nu^*)$.

### Method of Multipliers

### Alternating Direction Method of Multipliers (ADMM)

Minimize $f(x)+p(x)$ subject to $Ax+Bz=c$.
