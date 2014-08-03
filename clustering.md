Clustering
===

Goal: find a meaningful data partitioning. Can be hard and soft/fuzzy.

A form of unsupervised learning, whereas the corresponding supervised procedure is statistical classification.

More than 100 methods. Success of a method depends on underlying method.  "Clustering is in the eye of the beholder." 

Main types of clustering:

* Centroid-based/k-means
* Distribution-based/mixture models - clusters are objects belonging to same distribution
	* Gaussian Mixture Models: data set modeled as fixed number of gaussian distributions
* Connectivity-based/hierarchical
* Density-based - clusters as areas of higher density
	* DBSCAN
* Subspace clustering - for high dimensional data

K-Means clustering
---

Find partition of data $S$. Minimize within-cluster sum of squares (WCSS):

$$\underset{S} {\operatorname{arg\,min}} \sum\_{i=1}^{k} \sum\_{ x\_j \in S\_i} ||  x\_j - \mu\_i ||^2$$ 

Equivalently, find approximation $UZ$ to minimize Sum of Squared Errors:

$$ ||X-UZ||_F^2 $$

where $U$ is the centroids matrix and $Z$ is the assignment matrix, $Z$ has only one 1 entry per column, the rest are zeros, i.e.

$$Z\_{ln}\in\\{0,1\\}, \sum\_{l=1}^LZ\_{ln}=1$$

NP-hard -> approximate solution. K-means partitions data space into Voronoi diagram.

Before clustering perform [dimensionality reduction](dimensionality-reduction.md).

Drawbacks:

* Assumes equal-sized clusters
* Can't represent density-based clusters
* Can't find non-convex clusters
* Can theoretically converge in exponential time

### K-means/Lloyd/standard algorithm

* Assignment step: partition data according to Voronoi - assign to nearest centroid
* Update step: new means -> new centroids

Converges to local optimum.

Initialization: random centroid picked among data

Usually used in euclidean space.

Instability: amount of assignments that change when clustering data altered with random noise.

Choose cluster number (k) that minimizes instability.

Mixture Models
---

Assumption: data is generated i.i.d. by a mixture of K probabability densities:

$$p(x)=\sum_{k=1}^K\pi_kp(x|\theta_k) $$

Goal: find $\pi,\theta$ to maximize likelihood of data:

$$ L(\pi,\theta;X)=p(X|\pi,\theta)=\prod\_{n=1}^Np(x\_n) $$

### Expectation Maximization

In mixture models, the latent variables are random variables $z$ of dimension $k$ with a categorical distribution: exactly one element at a time is 1, i.e. 

$$p(z=(0,\ldots,\underbrace{1}\_k,\ldots,0))=p(z\_k=1)=\pi_k$$

and 0 otherwise. A data point depends only on one mixture component at a time, determined by the latent variable:

$$ p(x|z\_{k'}=1)=\prod\_{k=1}^Kp(x|\theta\_k)^{z\_k}=p(x|\theta\_{k'}) $$

1. E step: compute membership probabilities - the probability that data point n belongs to a cluster k is (by Bayes' rule) (we don't need to compute the expected latent values $E[z|X,\theta]$):

$$ \gamma(k,n)\leftarrow \operatorname{E}[z\_k|X\_n,\theta]=p(z\_k=1|X\_n,\theta)=\frac{\pi\_kp(X\_n|\theta\_k)}{\sum\_{j=1}^K \pi_jp(X\_n|\theta\_j)} $$


2. M step: estimate $\pi,\theta$.

Gaussian Mixture Model (GMM)
---

A specific mixture model where probability densities are all gaussian:

$$ p(x)=\sum_{k=1}^K\pi_k \mathcal{N}(x|\mu_k,\Sigma_k) $$

### Expectation Maximization

Goal: find $\theta=(\pi, \mu, \Sigma)$ that maximizes the likelihood of the data:

$$ L(\pi,\mu,\Sigma|X)=p(X|\pi,\mu,\Sigma)=\prod\_{n=1}^Np(x\_n)=\prod\_{n=1}^N
\sum_{k=1}^K\pi_k \mathcal{N}(x|\mu_k,\Sigma_k)
$$

1. E step: evaluate responsibilities:

$$ \gamma(k,n)\leftarrow \frac{\pi\_k\mathcal{N}(x\_n|\mu\_k,\Sigma\_k)}{\sum\_{j=1}^K \pi_j\mathcal{N}(x\_n|\mu\_j,\Sigma\_j)} $$

2. M step: estimate $\pi, \mu, \Sigma$.

$$ N\_k\leftarrow\sum\_{n=1}^N\gamma(k,n) $$ 

$$ \pi_k\leftarrow\frac{N\_k}{N} $$

$$ \mu\_k\leftarrow\frac{1}{N\_k}\sum\_{n=1}^N\gamma(k,n)X\_n $$

$$ \Sigma_k\leftarrow\frac{1}{N_k}(X-\mu_k)\operatorname{diag}(\gamma(k,1),\ldots,\gamma(k,n))(X-\mu_k)^T$$ 

### GMM as matrix factorization

Soft clustering: instead of assigning a cluster, assign a probability: 

$$Z\_{kn}\in[0,1], \sum\_{k=1}^KZ\_{kn}=1$$

GMM gives an upper bound for the squared error:

$$||X-UZ||\_F^2$$

Don't understand - lecture05, slide 28.

### Comparison to k-means

* EM soft assignments
* Stronger in that they estimate ovariances and not only means
* EM where covariance matrices are assumed to be fixed to $\epsilon I$, when $\epsilon\rightarrow0$, the algorithm tends to hard assignment to clusters, i.e. it reduces to k-means.
* Slower
* k-means can be used o initialize EM

### Order selection

What should be $K$? Tradeoff: goodness of fit vs model complexity and risk of overfitting. The likelihood increases with k apart fom local minima.

Note: The following criteria are meant to be minimized.

The **Akaike Information Criterion (AIC)**, rewards goodness of fit but penalizes the number of estimated parameter to avoid overfitting:

$$AIC = 2K-1\ln L$$

Where $L$ is the maximum likelihood associated with given $K$. Valid only asymptotically, for few data use corrected AIC (AICc).

The **Bayesian Information Criterion (BIC)** penalizes the complexity more. For large $N$ it's:

$$BIC = \frac{1}{2}k\ln N-\ln L$$

Warning: in wikipedia BIC is 2x as defined here.

Multi-Assignment Clustering
---

Based on [1]

A probabilistic model for clustering Boolean data. Multi-assignment: an object can belong to multiple clusters simultaneously.

Application: role mining for Role-Based Access Control (RBAC): given User-permissions $X\in\mathbb{B}^{D\times N}$ (ideally) find roles $U\in\mathbb{R}^{D\times K}$ and assignments $Z\in\mathbb{B}^{K\times N}$ so that

$$X=U\otimes Z :\Leftrightarrow X\_{dn}=\bigvee\_k[U_{dk}\wedge Z\_{kn}]$$

Since a user can have multiple roles, a multi-assignment clustering is needed.

The exact boolean matrix decomposition problem is equivalent to set basis problem: given finite set $U$, family $\mathcal{S}\subseteq\mathcal{P}(U)$ and $k$, find a set basis of size at most $k$ for $\mathcal{S}$. NP-hard. Corresponding decision problem is NP-complete.

That's why we look for an approximate decomposition. There are different approaches to what to fix, what to approximate, and how.

### Approaches

* SVD
	* Compute SVD, pick $U_k, V_k$ and round up/down according to threshold to get bools
	* Not suitable for role mining: poor Boolean decomposition
* K-Means
	* Use Hamming distance instead of Frobenius norm
	* Boolean centroids, use median during update step
	* Disjoint clustering -> no multi-assignments
* Role Miner
	* Find sets of common permissions
	* Sensitive to noise - overfitting
* Discrete Basis Problem Solver
	* Put things in roles that are often 1 together
* Lossy Compression Problem 1 (LCP1): minimal deviation $||X-U\otimes Z||$ for given K
* LCP2: minimal K for given deviation
* Model-based clustering - Structure Inference Problem (SIP): assume $X$ generated by hidden structure and perturbed by noise. Infer the structure.
	* Inferring a structure is better than just trying to approximate the generated matrix

### Model-based Clustering

Data is generated by a probabilistic model:

Mixture Noise Model: data is a mixture of independent emission from the structure part and a random noise.

$$ X\_{dn}=(1-\xi\_{dn})(U\otimes Z)\_{dn}+\xi\_{dn}R\_{dn} $$

The likelihood of a model is then:

$$L(Z,\beta,\xi,r;X)=p(X|Z,\beta,\xi,r)=\prod\_{n,d}(r^{X\_{dn}}(1-r)^{1-X\_{dn}})^{\xi\_{dn}}((1-\prod\_k\beta\_{d\_k}^{Z\_{kn}})^{X\_{dn}}(\prod\_k\beta\_{d\_k}^{Z\_{kn}})^{1-X\_{dn}})^{1-\xi\_{d\_n}} $$

Which can be rewritten to:

$$L(Z,\beta,\epsilon,r;X)=p(X|Z,\beta,\epsilon,r)=\prod\_{n,d}(\epsilon r+(1-\epsilon)(1-\beta\_{d,\mathcal{L}\_n}))^{x\_{dn}}(\epsilon(1-r)+(1-\epsilon)\beta\_{d,\mathcal{L}\_n})^{1-x\_{dn}}$$

Where 

* $X_{dn}$ denotes whether user $n$ has permission $d$,
* $Z_{kn}$ denotes whether user $n$ has role $k$,
* $\mathcal{L}\_n=\\{k|Z\_{kn}=1\\}$ the set of roles of user $n$.
* $\beta_{dk}$ is the probability that role $k$ does not contain permission $d$,
* $\epsilon\_{dn}$ is the probability that $X_{dn}$ was generated by noise model,
* $r$ is the probability that noisy bit is 1.

Maximize likelihood using expectation maximization algorithm.

Bibliography
---

1. PSTREICH, Andreas P., et al. Multi-assignment clustering for Boolean data. In: Proceedings of the 26th Annual International Conference on Machine Learning. ACM, 2009. p. 969-976.robabilistic model. Maximum-likelihood estimation with multiple assignments.
