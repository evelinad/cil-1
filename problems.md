Problems
===

Summary of problems we solve

Sum of Squares Error
---

$$ U^\*,Z^\*=\arg\min\_{U,Z}||X-UZ||\_F^2 $$

### PCA

$$\operatorname{rank}(TW)\leq K$$

This also minimizes the spectral norm!

$$ U^\*,Z^\*=\arg\min\_{U,Z}||X-UZ||\_2 $$

### K-Means Clustering 

$$Z\_{ln}\in\\{0,1\\}, \sum\_{l=1}^LZ\_{ln}=1$$

Equivalent to Z-orthogonal Semi-NMRF.

### Multi-Assignment Clustering

$$\sum\_{l=1}^LZ\_{ln}\geq1,U\_{ld}\in\\{0,1\\}$$

NP-hard, approximate problem with model-based clustering.

### Dictionary learning for sparse coding

$$||Z\_n||\_0\leq K,||U\_l||\_2=1$$

###  Non-negative Matrix Factorization (NMF)

$$U\geq0,Z\geq0$$

### Semi-NMF

$$Z\geq0$$

Equivalent to soft k-means.

### Z-orthogonal Semi-NMF

$$ZZ^T=I,Z\geq0$$

Equivalent to clustering.

### Convex-NMF

$$U=XW,W\in\mathbb{R}^{N\times K}$$

Likelihood Maximization
---

Given observed data $X$ and unknown latent variables $Z$, and likelihood function

$$L(\theta; X, Z) = p(X, Z|\theta) $$ 

find parameters $\theta$ to maximize the marginal likelihood:

$$ \theta^*=\arg\max_\theta L(\theta; X) $$

### Mixture Model

$$p(x)=\sum_{k=1}^K\pi_kp(x|\theta_k) $$

### Gaussian Mixture Model

$$ p(x)=\sum_{k=1}^K\pi_k \mathcal{N}(x|\mu_k,\Sigma_k) $$

### Model-Based Clustering

$$ X\_{dn}=(1-\xi\_{dn})(U\otimes Z)\_{dn}+\xi\_{dn}R\_{dn} $$

### Probabilistic Latent Semantic Indexing (PLSI)

$$ p(\text{word},\text{doc})=\sum_\text{topic} p(\text{word}|\text{topic})p(\text{topic},\text{doc})$$

Convex Optimization
---

Minimize $f(x)$ subject to $Ax=b$.

### Principal Component Pursuit

Minimize

$$||L||_*+\lambda||S||_1$$

subject to 

$$X=L+S$$

Which is a relaxation of the hard problem to minimize

$$\operatorname{rank}(L)+\lambda \operatorname{card}(S)$$

TODO
### Convex NMF
### Matching Pursuit
### Inpainting
