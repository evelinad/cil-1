Dimensionality Reduction
===

Reduce random variables under consideration to avoid effects of high dimensionality.

Feature extraction creates new features from functions of the original features.
Feature selection returns a subset of the features. 

Curse of dimensionality
---

High dimensionality has a multitude of phenomena. One is that distance functions lose their usefulness.

Assume data in d dimensions with a gaussian distribution:

$$x_d\sim\mathcal{N}(0,1)$$

Then distances are too (with bigger variance):

$$x_d-y_d\sim\mathcal{N}(0,2)$$

If we normalize by dimension, the data is Gamma distributed:

$$\frac{1}{D}\sum_{d=1}^D(x_d-y_d)^2\sim\Gamma(\frac{D}{2},\frac{4}{D})$$

The Gamma distribution tends to a normal with no variance with high dimensionality:

$$\Gamma(\frac{D}{2},\frac{4}{D})\mathop{\rightarrow}_{D\rightarrow\infty}\mathcal{N}(2,0)$$

i.e. in high dimension the normalized distance between points tends to have constant value.

In other words, if the data doesn't behave that way, we can assume that:

* the intrinsic dimensionality of the data isn't that high, or
* it isn't normally distributed at all

Principal Component Analysis
---

PCA: Orthogonal linear transformation that transforms the data to a new coordinate system such that the greatest variance by some projection of the data comes to lie on the first coordinate (called the first principal component), the second greatest variance on the second coordinate, and so on.

Components ordered by variance - by how much they account for data variability.

Minimize covariance between principal components.

### With eigenvalue decomposition

Given the eigenvalue decomposition of X^TX:

$$X^TX=W\Lambda W^T$$

The score matrix is:

$$T=XW$$

Note that X^TX is proportional to the covariance matrix

$$ \frac{1}{n}X^TX $$

### With singular value decomposition

Given the svd

$$ X=U\Sigma W^T $$

The score matrix can be computed with:

$$ T=XW=U\Sigma $$

### Dimensionality reduction

Choose L by analyzing singular value spectrum and choosing a knee.

$$\tilde{X}=T_LW_L^T=U_L\Sigma_LW_L^T$$

Minimizes error:

$$ ||X-X_L||_2 $$

Where the euclidean matrix norm is defined as

$$ ||A||\_p=\max_{x\neq0}\frac{||Ax||_p}{||x||_p} $$

When p=2 (spectral norm) it's the largest singular value.
