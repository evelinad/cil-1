Non-Negative Matrix Factorization
===

Given a sparse $X\in\mathbb{R}\_+^{D\times N}$ find $U\in\mathbb{R}\_+^{D\times K}$ so that

$$X\approx UZ$$

Application: clustering of text documents. $X_{dn}$ denotes whether document $n$ contains word $d$. $X_n$ is a bag of words.

We have different techniques depending on the cost function (the error measure).

Latent Semantic Indexing (LSI)
---

Not non-negative. Use SVD:

$$ X_k=U_kD_kV_k^T $$

Wikipedia: LSI will return results that are conceptually similar in meaning to the search criteria even if the results don’t share a specific word or words with the search criteria.

Like PCA but without subtracting the means in order not to lose sparseness.

Probabilistic Latent Semantic Indexing (PLSI) - using the Kullback Leibler divergence
---

Generative model: assume $k$ topics, documents are assigned to various topics with a certain probabilities, then the existence of the words depends only on the topics:

$$ p(\text{word},\text{doc})=\sum_\text{topic} p(\text{word}|\text{topic})p(\text{topic},\text{doc})$$

Assume the data is normalized. We would like to minimize $D_{KL}(X||UZ)$, i.e. find

$$\min\_{U,Z}\sum\_{d=1}^D\sum\_{n=1}^Nx\_{d\_n}\ln\frac{x\_{d\_n}}{(UZ)\_{d\_n}}$$

This is equivalent to maximum likelihood estimation and can be done with the EM algorithm.

Using Squared Error
---

We want to minimize Sum of Squared Errors $||X-UZ||_F^2$.

$U,Z$ initialized randomly, then iterate:

* $U\leftarrow U \odot XZ^T \oslash UZZ^T$
* $Z\leftarrow Z \odot U^TX \oslash U^TUZ$

Leads to a parts-based representation.

### Semi-NMF

Relaxing non-negative assumption on $U$ leads to algorithm similar to k-means:

* $U\leftarrow XZ^T(ZZ^T)^{-1}$
* $Z\leftarrow Z\sqrt{\left[(U^TX)^++(U^TU)^-Z\right] \oslash \left[(U^TX)^-+(U^TU)^+Z\right]}$

where $A\_{ij}^+=\max\\{0,A\_{ij}\\}$ and $A\_{ij}^-=\min\\{0,A\_{ij}\\}$.

It's equivalent to soft k-means clustering. If $Z$ is required to be orthogonal, it's equivalent to k-means clustering.

Why then such a complicated algorithm?

Convex NMF
---

$U$ should be a convex combination of $X$, makes $Z$ more sparse and orthogonal.
