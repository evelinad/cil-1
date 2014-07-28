Clustering
===

Goal: find a meaningful data partitioning. Can be hard and soft/fuzzy.

A form of unsupervised learning, whereas the corresponding supervised procedure is statistical classification.

More than 100 methods. Success of a method depends on underlying method.  "Clustering is in the eye of the beholder." 

Main types of clustering:

* Connectivity-based/hierarchical
* Centroid-based/k-means
* Distribution-based - clusters are objects belonging to same distribution
	* Gaussian Mixture Models: data set modeled as fixed number of gaussian distributions
		* Expectation Maximization algorithm
* Density-based - clusters as areas of higher density
	* DBSCAN
* Subspace clustering - for high dimensional data

K-Means clustering
---

Find partition of data $\mathbf{S}$. Minimize within-cluster sum of squares (WCSS):


$$\underset{\mathbf{S}} {\operatorname{arg\,min}} \sum\_{i=1}^{k} \sum\_{\mathbf x\_j \in S\_i} \left\| \mathbf x\_j - \boldsymbol\mu\_i \right\|^2$$ 

NP-hard -> approximate solution. K-means partitions data space into Voronoi diagram.

Before clustering perform [dimensionality reduction](dimensionality-reduction.md).

Drawbacks:

* Assumes equal-sized clusters
* Can't represent density-based clusters
* Can't find non-convex clusters
* Can theoretically converge in exponential time

K-means/Lloyd/standard algorithm
---

* Assignment step: partition data according to Voronoi - assign to nearest centroid
* Update step: new means -> new centroids

Converges to local optimum.

Initialization: random centroid picked among data

Usually used in euclidean space.

Instability: amount of assignments that change when clustering data altered with random noise.

Choose cluster number (k) that minimizes instability.

Clustering as matrix factorization
---

Given X find approximation UZ to minimize error

$$ ||X-UZ||_F^2 $$

where U is the centroids matrix and Z is the assignment matrix, Z has only one 1 entry per column, the rest are zeros.

Where U are the centroids and Z are the assignments
