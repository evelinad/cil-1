Sparse Coding
===

Signal Compression
---

Given signal $f$, use a dictionary: orthogonal matrix of basis functions $U=[u_1,\cdots,u_D]\in\mathbb{R}^{D\times D}$ to compute

$$z=\sum_{i=1}^D\left&lt;u_i,f\right&gt;=Uf$$

$z$ has the same energy as $f$. Let $\hat{z}$ be the sparsified $z$, i.e. where small values are truncated, which is equivalent to not using certain basis functions. Let $\sigma$ be the set of indeces of bases used.

Reconstruct signal:

$$\hat{f}=U^T\hat{z}$$

Reconstruction error depends only on the bases not used:

$$||f-\hat{f}||^2=\sum_{k\notin\sigma}|\left&lt;f,u\_k\right&gt;|^2$$

In general, different dictionaries appropriate for different types of signal.

### Fast Fourier Transform (FFT)

Good for sine-like signals. Poor for localized signals.

### Fast Wavelet Transform (FWT)

Wavelets are good for localized signals. Poor for non-vanishing signals.

Uses [Haar wavelets](media/Haar_wavelet.svg) , in 4 dimensions:

$$
\begin{bmatrix}
	1 \\\ 1 \\\ -1 \\\ -1
\end{bmatrix}
\begin{bmatrix}
	1 \\\ -1 \\\ 0 \\\ 0
\end{bmatrix}
\begin{bmatrix}
	0 \\\ 0 \\\ 1 \\\ -1
\end{bmatrix}
$$

### Karhunen–Loève Transform

It's PCA. Assume many signals $X$ to be compared to compute correlation matrix $\Sigma=U\Lambda U^T$, then $\hat{z}=U_kx$ (is the result sparse??).

### Discrete Cosine Transform

Cosine wavelets. JPEG. Related to DFT but only using real numbers. Most common is DCT-II.

$$X\_k = \sum\_{n=0}^{N-1} x\_n \cos \left[\frac{\pi}{N} \left(n+\frac{1}{2}\right)\right] $$

### Compressive Sensing

### Recovery

Overcomplete Dictionaries
---

Overcompleteness: more dictionary elements than dimensions, i.e. $U\in\mathbb{R}^{D\times L}$ and $L>D$. Not linearly independent anymore.

Example: Directional Gabor Wavelets.

Increasing overcompleteness factor $\frac{L}{D}$ increases sparsity of coding and linear dependency (coherence).

Assume noisy observation (why here and not for general signal compression?):

$$x=Uz+n\text{, where }n_d\sim\mathcal{N}(0,\sigma^2)$$

Approaches:

* Maximize sparsity with bounded error

$$z^*\in\arg\min_z||z||_0\text{ subject to }||x-Uz||_2<\sigma$$

* Minimize residual with bounded sparsity

$$z^*\in\arg\min_z||x-Uz||_2\text{ subject to}||z||_0\leq K$$

### Matching Pursuit

### Inpainting

Dictionary Learning
---

Adapt the dictionary to the signal. $U\in\mathbb{R}^{D\times L}$ where $L$ doesn't need to be as big.

Again, minimize the squared error:

$$||X-UZ||_F^2$$

subject to

$$||Z\_n||\_0\leq K,||U\_l||\_2=1$$

Algorithm:

* Coding: $Z\leftarrow\arg\min_Z||X-UZ||_F^2$ subject to $Z$ sparse.
	* Doable as $s$ independent coding steps $Z_n\leftarrow\arg\min_z||z||_0$ subject to $||x_n-Uz||_2\leq\sigma||x_n||_2$.
* Dict update: $U\leftarrow\arg\min_U||X-UZ||_F^2$ subject to $||u_l||_2=1$.
	* For each $l$ fix all other columns in $U$ and change $U\_l$ to minimize $||R\_l-U\_lZ\_{l:}||$, subject to $||U\_l||\_2=1$.
		* Approximately achieved using SVD of $R\_l=X-\sum\_{e\neq l}U\_eZ\_{e:}$. The wanted $U\_l$ is first left-singular vector.

Initialization possibility of $U$:

* Random on unit sphere
* Sample from X
* Some fixed overcomplete dictionary


### Examples

Image approximation with sparse coding and dictionary.

Model based speech enhancement. Enhancement pipeline:

1. learning step
2. enhancement step

Need to know in detail??
