Intro
===

Sample Covariance Matrix
---

$$ \frac{1}{n}X^TX $$

Eigenvalue decomposition
---

$$ X=Q\Lambda Q^T $$

Q orthogonal, Lambda diagonal.

Defined based on eigenvalues, eigenvectors:

$$\\{(\lambda,u): Au=\lambda u,u\neq0\\}$$
$$Q=[u_1|\cdots|u_n], \Lambda=\operatorname{diag}\\{\lambda_1,\ldots,\lambda_n\\}$$

Singular value decomposition
---

$$ X=U\Sigma V^T $$

U, T orthogonal.

### Relationship with Eigenvalue decomposition:

$$ X^TX=V\Sigma^2V^T $$

$$ XX^T=U\Sigma^2U$$

Singular values of X are square roots of eigenvalues of $X^T*X$ and $X*X^T$.

### Computing the svd:

* Compute eigenvalues of $X^T*X$, square to get singular values.
* Compute eigenvectors of $X^T*X$, the colmns of V.
* Compute $U=AVD^-1$.

RMSE
---

A mean that gives more weight to bigger values: Root mean square (RMS) of a set of values:

$$\sqrt(\frac{1}{n}\sum_{i=1}^nx_i^2)$$

When talking about a matrix:

$$\sqrt(\frac{1}{n}\sum_{i=1}^n\sum_{j=1}A_ij^2)$$

Given a matrix $X$ and an approximation $\tilde{X}$, the Root-Mean-Square-Error is:

$$||X-\tilde{X}||_F^2=(X-\tilde{X})$$

Where the frobenius norm is defined as:

$$||A||_F:=\sqrt{\sum_i=1^m\sum_{j=1}^nA_{ij}^2$$


