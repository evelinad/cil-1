Intro
===

Matrix Operations
---

Mean:

$$\bar{x}=\frac{1}{n}\sum_{i=1}^nX_i,\; \bar{X}=[\bar{x},\cdots,\bar{x}]$$

Sample Covariance Matrix:

$$ \frac{1}{n}(X-\bar{X})^T(X-\bar{X}) $$

Eigenvalue decomposition
---

$$ X=Q\Lambda Q^T $$

$Q$ orthogonal, $\Lambda$ diagonal.

Defined based on eigenvalues, eigenvectors:

$$\\{(\lambda,u): Au=\lambda u,u\neq0\\}$$
$$Q=[u_1|\cdots|u_n], \Lambda=\operatorname{diag}\\{\lambda_1,\ldots,\lambda_n\\}$$

Singular value decomposition
---

$$ X=U\Sigma V^T $$

$U, V$ orthogonal.

### Relationship with Eigenvalue decomposition:

$$ X^TX=V\Sigma^2V^T $$

$$ XX^T=U\Sigma^2U$$

Singular values of X are square roots of eigenvalues of $X^T*X$ and $X*X^T$.

### Computing the svd:

* Compute eigenvalues of $X^T*X$, square to get singular values.
* Compute eigenvectors of $X^T*X$, the colmns of V.
* Compute $U=AVD^{-1}$.

Norms
---

### Vector norms

p-norm for vectors:

$$||\mathbf{x}||\_p := \bigg( \sum\_{i=1}^n |x\_i|^p \bigg)^{1/p}$$

For p=1 taxicab norm, p=2 euclidean norm, p=infinity infinity/maximum norm.

### Matrix norms

#### p-norm for vectors induced to matrices

$$||A||\_p = \sup \limits \_{x \ne 0} \frac{|| A x|| _p}{|| x|| _p}$$

For p=2 it's the spectral norm or the largest singular value:

$$|| A ||\_2=\sqrt{\lambda\_{\text{max}}(A^{^*} A)}=\sigma\_{\text{max}}(A) $$

#### Entrywise norm

Warning: it has the same notation as the induced p-norm!

$$\Vert A \Vert\_{p} = \Vert \mathrm{vec}(A) \Vert\_{p} = \left( \sum\_{i=1}^m \sum\_{j=1}^n |A\_{ij}|^p \right)^{1/p}$$ 

For p=0 it's the Hamming distance. For p=2 you get the Frobenius norm:

$$ ||A||\_F=\sqrt{\sum\_{i=1}^m\sum\_{j=1}^n |A\_{ij}|^2}=\sqrt{\operatorname{trace}(A^{{}^*}A)}=\sqrt{\sum\_{i=1}^{\min\{m,\,n\}} \sigma\_{i}^2} $$

Root Mean Square Error
---

Given a matrix $X$ and an approximation $\tilde{X}$, the error is

$$E=\tilde{X}-X$$

Sum of Squared Errors (SSE):

$$\sum\_{i=1}^m\sum\_{j=1}^nE\_{ij}^2=||E||\_F^2$$

Mean Square Error (MSE):

$$\frac{1}{mn}\sum\_{i=1}^m\sum\_{j=1}^nE\_{ij}^2=\frac{1}{mn}||E||\_F^2$$

Root Mean Square Error (RMSE):

$$\sqrt{\frac{1}{mn}\sum\_{i=1}^m\sum\_{j=1}^nE\_{ij}^2}=\sqrt{\frac{1}{mn}}||E||\_F$$

Probability
---

Expected value:

$$\operatorname{E}[X] = \sum_{i=1}^nx_ip_i$$

Conditional expected value:

$$\operatorname{E}[X|Y] = \sum_{i=1}^nx_ip(x_i|Y)$$

Bayes' Rule:

$$ P(A|B)=\frac{P(B|A)P(A)}{P(B)} $$

