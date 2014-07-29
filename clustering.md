Clustering
===

Goal: find a meaningful data partitioning. Can be hard and soft/fuzzy.

A form of unsupervised learning, whereas the corresponding supervised procedure is statistical classification.

More than 100 methods. Success of a method depends on underlying method.  "Clustering is in the eye of the beholder." 

Main types of clustering:

* Centroid-based/k-means
* Distribution-based/mixture models - clusters are objects belonging to same distribution
	* Gaussian Mixture Models: data set modeled as fixed number of gaussian distributions
		* Expectation Maximization algorithm
* Connectivity-based/hierarchical
* Density-based - clusters as areas of higher density
	* DBSCAN
* Subspace clustering - for high dimensional data

K-Means clustering
---

Find partition of data $\mathbf{S}$. Minimize within-cluster sum of squares (WCSS):


$$\underset{\mathbf{S}} {\operatorname{arg\,min}} \sum\_{i=1}^{k} \sum\_{\mathbf x\_j \in S\_i} || \mathbf x\_j - \boldsymbol\mu\_i ||^2$$ 

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

### As matrix factorization

Given X find approximation UZ to minimize Sum of Squared Errors:

$$ ||X-UZ||_F^2 $$

where U is the centroids matrix and Z is the assignment matrix, Z has only one 1 entry per column, the rest are zeros.

Mixture Models
---

Assumption: data is generated i.i.d. by a mixture of K probabability densities:

$$p(x)=\sum_{k=1}^K\pi_kp(x|\theta_k) $$

Goal: find $\pi,\theta$ to maximize likelihood of data:

$$ p(X|\pi,\theta)=\prod\_{n=1}^Np(x\_n) $$

### Latent variables

A latent variable is a random variable z of dimension k with a categorical distribution: exactly one element at a time is 1 i.e.

$$p(z=(0,\ldots,\underbrace{1}\_k,\ldots,0))=p(z\_k=1)=\pi_k$$

and 0 otherwise. A data point depends only on one mixture component at a time, determined by the latent variable:

$$ p(x|z\_{k'}=1)=\prod\_{k=1}^Kp(x|\theta\_k)^{z\_k}=p(x|\theta\_{k'}) $$

Responsibility: the probability that data point n belongs to a cluster k is (by Bayes' rule):

$$ \gamma(k,n):=p(z\_k=1|X\_n)=\frac{\pi\_kp(X\_n|\theta\_k)}{\sum\_{j=1}^K \pi_jp(X\_n|\theta\_j)} $$

### Gaussian Mixture Model (GMM)

Probability densities are all gaussian:

$$ p(x)=\sum_{k=1}^K\pi_k \mathcal{N}(x|\mu_k,\Sigma_k) $$

Goal: find $\pi, \mu, \Sigma$ to maximize likelihood of data:

$$ p(X|\pi,\mu,\Sigma)=\prod\_{n=1}^Np(x\_n)=\prod\_{n=1}^N
\sum_{k=1}^K\pi_k \mathcal{N}(x|\mu_k,\Sigma_k)
$$

For easier computations take use log likelihood:

$$ \ln p(X|\pi,\mu,\Sigma)=\sum\_{n=1}^N\ln\left(\sum\_{k=1}^K\pi_k\mathcal{N}(x_n|\mu_k,\Sigma_k)\right) $$

### Expectation Maximization Algorithm

Compute initial values for $\mu, \pi, \Sigma$:

$$ N\_k\leftarrow\sum\_{n=1}^N\gamma(k,n) $$ 

$$ \mu\_k\leftarrow\frac{1}{N\_k}\sum\_{n=1}^N\gamma(k,n)X\_n $$

$$ \pi_k\leftarrow\frac{N\_k}{N} $$

$$ \Sigma\leftarrow\frac{1}{n}X^TX \text{ (sample covariance matrix)}$$

Iterate. E-step: evaluate responsibilities:

$$ \gamma(k,n)\leftarrow \frac{\pi\_k\mathcal{N}(x\_n|\mu\_k,\Sigma\_k)}{\sum\_{j=1}^K \pi_j\mathcal{N}(x\_n|\mu\_j,\Sigma\_j)} $$

M-step: re-estimate $\mu, \pi, \Sigma$.

Check for log-likelihood or parameter convergence.

### GMM an matrix factorization

Soft clustering: instead of assigning a cluster, assign a probability: 

$$Z\_{kn}\in[0,1], \sum\_{k=1}^KZ\_{kn}=1$$

GMM gives an upper bound for the SSE:

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

Role-Based Access Control (RBAC): given User-permissions $X\in\mathbb{B}^{D\times N}$ (ideally) find roles $U\in\mathbb{R}^{D\times K}$ and assignments $Z\in\mathbb{B}^{K\times N}$ so that

$$X=U\otimes Z :\Leftrightarrow X\_{dn}=\bigvee\_k[U{dk}\wedge Z\_{kn}]$$

Min-noise approximation: find

$$\arg\min\_{U,Z}||X-U\otimes Z||\_F^2$$

The problem is NP-hard, equivalent to set basis problem: given finite set $U$, family $\mathcal{S}\subseteq\mathcal{P}(U)$ and $k$, does there exist a set basis of size at most $k$ for $\mathcal{S}$?

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
* Model-based clustering
	* The best

### Model-based Clustering
