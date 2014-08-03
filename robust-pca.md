Robust PCA
===

Convex Optimization
---

$f$ is convex if

TODO

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

### Dual Problem

Maximize dual function:

$$d(\lambda,\nu)=\inf_xL(x,\lambda,\nu)$$

subject to $\lambda\geq0$. It's a lower bound on solution.

Gradient ascent:

* $x\leftarrow\arg\min_xL(x,\lambda,\nu)$
* $(\lambda,\nu)\leftarrow(\lambda,\nu)+\alpha\nabla d(\lambda,\nu)$

where $\nabla$ is gradient operator, $\alpha$ is step length.

### Matrix Form

1. Minimize $f(x)$ subject to $Ax=b$.
2. Lagrangian: $L(x,\nu)=f(x)+\nu^T(Ax-b)$.
3. Find solution $\nu^\*$ to dual problem through gradient ascent.
	* Note that $\nabla d(\nu)=Ax-b$
4. Recover optimal: $\arg\min\_xL(x,\nu^\*)$.

### Dual Decomposition

If function is separable:

$$f(x)=\sum_{i=1}^nf_i(x_i)$$

Lagrangian is separable as well:

$$L(x,\nu)=\sum_i L_i(x_i\nu)-\nu^Tb=\sum_if_i(x_i)+\nu^TA_ix_i$$

Gradient Ascent is parallelizable:

* $ax\_i\leftarrow\arg\min\_{x\_i}L\_i(x\_i\nu)$
* $\nu\leftarrow\nu+\alpha(\sum_i A_ix_i-b)$

Converges only under strict convexity and finiteness of $f$.

### Method of Multipliers

Augmented Lagrangian penalizes violating constraints more:

$$L\_\rho(x,\nu)=L(x,\nu)+\frac{\rho}{2}||Ax-b||\_2^2$$

Method of Multipliers:

* $x\leftarrow\arg\min\_x L\_\rho(x,\nu)$
* $\nu\leftarrow\nu+\rho(Ax-b)$

Converges under more general assumptions. Not parallelizable.

Optimality conditions:

* Optimal feasibility: $Ax-b=0$
* Dual feasibility: $\nabla f(x)+A^T\nu=0$

### Alternating Detection of Method of Multipliers (ADMM)

Minimize $f(x)+p(z)$ subject to $Ax+Bz=c$.

Augmented Lagrangian:

$$L_\rho(x,z,\nu)=f(x)+p(z)+\nu^T(Ax+Bz-c)+\frac{\rho}{2}||Ax+Bz-c||_2^2$$

ADMM:

* $x\leftarrow\arg\min\_x L\_\rho(x,z,\nu)$
* $z\leftarrow\arg\min\_z L\_\rho(x,z,\nu)$
* $\nu=\nu+\rho(Ax+Bz-c)$

Optimality conditions

* Optimal feasibility: $Ax+Bz-c=0$
* Dual feasibility: $\nabla f(x)+A^T\nu=0,\nabla p(z)+B\nu=0$

Robust PCA
---

Robust PCA: works with corrupted observations (i.e. where bayesian assumptions for noise are inadequate).

Applications:

* Image processing (corrupted images, removing face illumination, reconstructing partly hidden images)
* Web data analysis
* Bioinformatiocs
* Computer vision
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

$$X=L+S$$

### ADMM for RPCA

Set $f(x)=||L||_*$, $p(z)=||S||_1$.

Lagrangian:

$$L\_\mu(L,S,N)=||L||\_*+\lambda||X||\_1+(N:(X-L-S))+\frac{\mu}{2}||X-L-S||\_F^2$$

Gradient ascent:

* $S\leftarrow\arg\min\_S L\_\mu(S,L,N)=\mathcal{S}\_{\lambda\mu^{-1}}(X-L+\mu^{-1}N)$
* $L\leftarrow\arg\min\_L L\_\mu(S,L,N)=\mathcal{D}\_{\lambda\mu^{-1}}(X-L+\mu^{-1}N)$
* $N\leftarrow N+\mu(X-L-S)$

Where $\lambda$ is the iteration step and

* $\mathcal{S}\_\tau(X)=\operatorname{sgn}(x)\max(|X|-\tau,0)$
* $\mathcal{D}\_\tau(X)=U\mathcal{S}\_\tau(\Sigma)M^T$ where $X=U\Sigma M^T$

Why not compute the minimum directly??

### Conditions for exact recovery:

$X$ cannot be low rank *and* sparse.

$L$ cannot be sparse.

If $\operatorname{rank}(L)\leq\rho_rn\mu^{-1}(\ln n)^{-2}$, $\operatorname{card}(S)\leq\rho_sh^2$, $\rho_r,\rho_s>0$, then PCP with $\lambda=\frac{1}{n}$ is exact with probability $1-\mathcal{O}(n^{-10})$.

### RPCA for Collaborative filtering

When used for matrix completion - consider only observed values.

If additionally data is corrupt, then

TODO

