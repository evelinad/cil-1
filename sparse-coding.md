Sparse Coding
===

Classical Methods
---

### Signal Compression

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

It's PCA. Assume many signals $X$ to be compared to compute correlation matrix $\Sigma=U\Lambda U^T$, then $\hat{z}=U_Kx$.

### Discrete Cosine Transform

Cosine wavelets. JPEG. Related to DFT but only using real numbers. Most common is DCT-II.

### Compressive Sensing

Compress data while gathering it. Application: MRI.

Given signal $x$, assume it is sparse in some orthonormal basis $U$, i.e. for some sparse $z$:

$$x=Uz$$

How do you find $U$ and $z$?? How do you know $U$ later on? It's an input to MP!

Then it is possible to represent the signal with fewer dimensions:

$$y=Wx=WUz=\Theta z\text{ where }\Theta\in\mathbb{R}^{M\times D}$$

Recovery is possible for any $W$ for which

* $W\_{ij}\in\_{\text{u.a.r}}\mathcal{N}(0,\frac{1}{D})$
* $M\geq cK\ln(\frac{D}{K})$ for some $c$

Recovery: Find sparsest applicable $z$

$$z^*\leftarrow\arg\min_z||z||_0\text{ subject to }y=\Theta z$$

It's NP hard, approximate with Matching Pursuit.

Reconstruct signal:

$x\leftarrow Uz$

How do you find $U$??

Overcomplete Dictionaries
---

Overcompleteness: more dictionary elements than dimensions, i.e. $U\in\mathbb{R}^{D\times L}$ and $L>D$. Not linearly independent anymore.

Example: Directional Gabor Wavelets.

Increasing overcompleteness factor $\frac{L}{D}$ increases sparsity of coding and linear dependency (coherence).

Assume noisy observation:

$$x=Uz+n\text{, where }n_d\sim\mathcal{N}(0,\sigma^2)$$

### Sparse Coding

Approaches:

* Maximize sparsity with no error (NP hard):

$$z^*\in\arg\min_z||z||_0\text{ subject to }x=Uz$$

* Maximize sparsity with bounded error (how??):

$$z^*\in\arg\min_z||z||_0\text{ subject to }||x-Uz||_2<\sigma$$

* Minimize residual with bounded sparsity (Matching Pursuit):

$$z^*\in\arg\min_z||x-Uz||_2\text{ subject to }||z||_0\leq K$$

### Matching Pursuit

Objective: find sparsest representation

$$z^*\leftarrow\arg\min_z||x-Uz||_2$$

subject to

$$||z||_0\leq K$$

Greedy

* Init: $z\leftarrow 0$, $r\leftarrow x$
* While $||z||_0<k$:
	* $d\leftarrow \arg\max_d|U_d^TR|$
		* Minimizing the residual is equivalent to maximizing the absolute scalar product.
	* $z_d\leftarrow z_d^+u_d^TR$
	* $r\leftarrow r-(U_d^Tr)U_d$

Exact recovery condition:

$$K<\frac{1}{2}\left(1+\frac{1}{m(U)}\right)$$

i.e. sparse coding (small $K$) recovers support.

Tradeoff: increasing $L$ leads to sparser coding but increases coherence.

### Inpainting

Sparse coding of known parts of picture.
Reconstructing image predicts missing parts.

Given diagonal masking matrix $M$, $M_{dd}=1$ if pixel $d$ is known.


Iteratively find sparse coding in $U$: $z\leftarrow\arg\min_z||z||_0$ so that $||M(x-Uz)||_2<\sigma$, doable with EM:

* Reconstruct image: $\hat{x}\leftarrow Mx+(I-M)Uz$
* Find sparse coding of $\hat{x}$: $z\leftarrow\arg\min_z||z||_0$ so that $||\hat{x}-Uz||_2<\sigma$

Reconstruct image: $\hat{x}=Mx+(I-M)Uz$

Initialization??

Dictionary Learning
---

Adapt the dictionary to the signal. $U\in\mathbb{R}^{D\times L}$ where $L$ doesn't need to be as big.

Minimize the squared error:

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

* Apply DFT to all signals before processing them.
1. Learning step:
	1. Learn dictionary for speech and hardcode
	2. Live: on input just noise learn dictionary for noise and combine dictionaries
2. Enhancement step:
	1. Live: Express current input with combined dictionaries
	2. Throw away the noise part
	3. Reconstruct speech only
