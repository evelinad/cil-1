Background
===

Vector Operations
---

### Scalar/Dot/Inner Product

$$x\cdot y=\left&lt;x,y\right&gt;=x^Ty$$

### Outer Product

$$x\otimes y=xy^T$$

Matrix Operations
---

### Mean:

$$\bar{x}=\frac{1}{n}\sum_{i=1}^nX_i,\; \bar{X}=[\bar{x},\cdots,\bar{x}]$$

### Sample Covariance Matrix

Element $ij$ is covariance between *columns* $i$ and $j$:

$$ \frac{1}{n}(X-\bar{X})(X-\bar{X})^T $$

### Element-wise multiplication (or Hadamard-product):

$$A\odot B=C\Leftrightarrow A\_{ij}=B\_{ij}C\_{ij} $$

### Element-wise division:

$$A\oslash B=C\Leftrightarrow A\_{ij}=B\_{ij}/C\_{ij} $$

### Frobenius product:

$$ A:B=\sum\_{i,j}A\_{ij}B\_{ij} $$

### Coherence

The maximum absolute correlation. It's a measure for linear dependency.

$$m(U)=\max_{i,j:i\neq j}|\left&lt;u_i,u_j\right&gt;|$$

The coherence of an orthogonal matrix is 0.

Matrix Types
---

### Orthogonal:

$$A^TA=AA^T=I$$

equivalently:

$$A^T=A^{-1}$$

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

Singular values of X are square roots of eigenvalues of $X^TX$ and $XX^T$.

### Computing the SVD:

* Compute eigenvalues of $X^TX$, square to get singular values.
* Compute eigenvectors of $X^TX$, the colmns of V.
* Compute $U=AVD^{-1}$.

Norms
---

### Vector norms

p-norm for vectors:

$$||\mathbf{x}||\_p := \bigg( \sum\_{i=1}^n |x\_i|^p \bigg)^{1/p}$$

For $p=1$ taxicab norm (sum of absolute values), $p=2$ euclidean norm, $p=\infty$ infinity/maximum norm.

### Matrix norms

#### p-norm for vectors induced to matrices

$$||A||\_p = \sup \limits \_{x \ne 0} \frac{|| A x|| _p}{|| x|| _p}$$

For p=2 it's the spectral norm or the largest singular value:

$$|| A ||\_2=\sigma\_{\text{max}}(A) $$

#### Entrywise norm

Warning: it has the same notation as the induced p-norm!

$$\Vert A \Vert\_{p} = \Vert \mathrm{vec}(A) \Vert\_{p} = \left( \sum\_{i=1}^m \sum\_{j=1}^n |A\_{ij}|^p \right)^{1/p}$$ 

For p=2 you get the Frobenius norm:

$$ ||A||\_F=\sqrt{\sum\_{i=1}^m\sum\_{j=1}^n |A\_{ij}|^2}=\sqrt{A:A}$$

### Zero Norm

Also called Hamming distance from zero or cardinality, it's the number of nonzero entries.

$$||A||\_0=\operatorname{card}(A)=|\\{A\_{ij}|A\_{ij}\neq0\\}|$$

### Nuclear norm

The sum of singular values

$$||A||\_*=\sum\_{i=1}^{\min\\{m,n\\}}\sigma\_i$$

Error Measures
---

### Error

Given a matrix $X$ and an approximation $\tilde{X}$, the error is

$$E=\tilde{X}-X$$

### Sum of Squared Errors (SSE) - or Squared Error:

$$\sum\_{i=1}^m\sum\_{j=1}^nE\_{ij}^2=||E||\_F^2$$

### Mean Square Error (MSE):

$$\frac{1}{mn}\sum\_{i=1}^m\sum\_{j=1}^nE\_{ij}^2=\frac{1}{mn}||E||\_F^2$$

### Root Mean Square Error (RMSE):

$$\sqrt{\frac{1}{mn}\sum\_{i=1}^m\sum\_{j=1}^nE\_{ij}^2}=\sqrt{\frac{1}{mn}}||E||\_F$$

Probability
---

### Expected value:

$$\operatorname{E}[X] = \sum_{i=1}^nx_ip_i$$

### Conditional expected value:

$$\operatorname{E}[X|Y] = \sum_{i=1}^nx_ip(x_i|Y)$$

### Bayes' Rule:

$$ P(A|B)=\frac{P(B|A)P(A)}{P(B)} $$

Expectation Maximization (EM)
---

Given observed data $X$ and unknown latent variables $Z$, and likelihood function

$$L(\theta; X, Z) = p(X, Z|\theta) $$ 

find parameters $\theta$ to maximize the marginal likelihood:

$$ L(\theta; X) = p(X|\theta) = \sum_Z p(X,Z|\theta) $$

Compute with iterative 2-step algorithm.

1. Expectation step: with fixed parameter estimate $\theta$ calculate the expected latent values:

$$Z\leftarrow E[Z|X,\theta]$$

2. Maximization step: given the the expected latent values, compute a new estimate of the parameters that maximizes the log likelihood:

$$\theta \leftarrow \underset{\theta}{\operatorname{arg\,max}} \log L(\theta;X,Z)$$

Initialization: as in M step.

Terminate on log-likelihood or parameter convergence.

Information Theory
---

### Self-information (surprisal):

$$ I(X) = - \ln(P(X)) $$

Goes from $+\infty$ (for probability 0) to 0 (for probability 1).

### Entropy (expected surprisal):

$$ H(X) = E[I(X)] = E[-\ln(P(X))] $$ 

For finite sample:

$$ H(X) = \sum\_{i} {P(x\_i)\,I(x\_i)} = -\sum\_{i} {P(x\_i) \log\_b P(x\_i)} $$

### Kullback-Leibler Divergence

Given probability distributions $P, Q$.

$$ D\_{KL}(P||Q)=\sum\_{x}P(X)\ln\frac{P(X)}{Q(X)} $$

Non symmetric. Denotes the information lost when $Q$ is used to approximate $P$.

Is a measure of relationship between models and reality ("how much the model has yet to learn"). An approach to minimizing it is expectation maximization.


### Cross entropy

$$ H(P,Q) = H(P)+D_{KL}(P||Q) = -\sum_xP(x)\ln Q(x)$$
