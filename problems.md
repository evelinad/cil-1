Problems
===

Summary of problems we solve

Matrix Factorization
---

$$ U^\*,Z^\*=\arg\min\_{U,Z}||X-UZ||\_F^2 $$

### Clustering 

$$Z\_{ln}\in\\{0,1\\}, \sum\_{l=1}^LZ\_{ln}=1$$

Equivalent to Z-orthogonal Semi-NMRF.

### Multi-Assignment Clustering

$$\sum\_{l=1}^LZ\_{ln}\geq1,U\_{ld}\in\\{0,1\\}$$

### Dictionary learning for sparse coding

$$||Z\_n||\_0\leq K,||U\_l||\_2=1$$

###  Non-negative Matrix Factorization (NMF)

$$U\geq0,Z\geq0$$

### Latent Semantic Indexing (LSI)?

### Semi-NMF

$$Z\geq0$$

Equivalent to soft k-means.

### Z-orthogonal Semi-NMF

$$ZZ^T=I,Z\geq0$$

Equivalent to clustering.

Likelihood Maximization
---

### Probabilistic Latent Semantic Indexing (PLSI)
