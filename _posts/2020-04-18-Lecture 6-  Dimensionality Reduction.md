# Lecture 5-  Dimensionality Reduction

THe first latent dimension is the direction in which the points have the highest variance. THe second is the dimension orthogonal to the first, in which the points show the 2nd greatest variance.

# Linear Algebra
The Rank of a matrix measures dimensionality.

##$X^TX$
For a matrix $X$, $X^TX$ (the [Gram matrix](https://en.wikipedia.org/wiki/Gramian_matrix), also known as the covariance matrix) is a square matrix whose diagonal elements are the sums of squares of of elements in $x$, and  is positive semi-definite.
- If the vectors are centered random variables, the Gramian is proportional to the covariance matrix, with the scaling determined by the number of elements in the vector.
- Since the Gram matrix over the reals is a symmetric matrix, it is diagonalizable and its eigenvalues are non-negative. The diagonalization of the Gram matrix is the singular value decomposition.

# PCA
PCA involves converting a set of observations into a set of values of linearly uncorrelated **principal components**. The transformation is defined in such a way that the principal components are ordered in terms of variance, and all principal components are orthogonal to each other. 

For a data matrix $M$, PCA is normally computed by computing the matrx of eigenvectors $W$ of $M^TM$ (the Gram matrix or correlation matrix). The full principal components decomposition of $M$ is then $T=MW$ (you can drop some columns if you don't want the whole decomposition). The matrix $W$ of eigenvectors of the correlation matrix $M^TM$ can be thought of as a rigid rotation in high dimensional space.  When you apply this transformation to the original data, the axis corresponding to the principal eigenvector is the one along which the points are most spread out.

# SVD:
- SVD is essentially a way to do PCA
- Goal is to compress/reduce dimensionality
- We can often take data and represent by an M by N matrix
  - $A = U \Sigma V^T = \sum_i\sigma_i u_i \cdot v_i$
-  SVD is a **unique** decomposition which also **optimally** minimizes the reconstruction error of the decomposition, defined by the Frobenius Norm = Euclidean distance between two matrices.
    - $||A-B||_F = \sqrt{\sum_{ij}{(A_{ij}-B_{ij})^2}}$
-How to pick singular values r
  - Pick r so the retained singular values have at least 90% of the total energy, which is defined as the sum of their squares.





![Screenshot 2019-10-23 at 9.55.04 PM.png](/assets/blog_resources/5F756B561D6F7945C1E6B80D6DBF30CA.png)

U is the left singular vectors
$\Sigma$ is the right singular vectors
V is the right singular vectors

## Properties:
It is always possible to decompose a real matrix A into $A = U \Sigma V^T$ where
- U, $\Sigma$, V are unique given A
- U, V are column orthonormal ($U^TU=I, V^TV=I$ and U, V are normal)
- $\Sigma$ is diagonal

![Screenshot 2019-10-23 at 10.02.24 PM.png](/assets/blog_resources/3DBDA7091595270609867989A67F2A68.png)

SVD Decomposes into:
- U: user-to-concept matrix
- V: movie-to-concept matrix
- $\Sigma$: its diagonal elements: ‘strength’ of each concept 

If you multiply the AVD decomposition for particular axes, that is the projection of the data onto that axis.

# Computing SVD
First we need a method for finding th principal eigenvalue and eigenvector of a symmetric matrix.
  Method (Power Iteration - We never use this in practice as there are much more advanced methods, but this is a good basic method):
  1. Start with any 'guess eigenvector' $x_0$
  2. Construct $x_{k+1} = \frac{M_{x_k}}{||M_{X_k}||_F}$ for k=0,1,... and stop when converge
  3. Once you have the princple eigenvector $x$, get its eigenvalue $\lambda$ by $\lambda = x^TMx$.  
  4. Find more eigenvalues and vectors by continuing on
      $$M* = M-\lambda x x^T$$, where $M*$ is the matrix after removing the factor of x.

This algorithm is very distributable. You only need a couple of iterations.

Suppose $A=U\Sigma V^T$
$$A^T = V\Sigma U^T$$
$A^TA = V \Sigma^2V^T$ since $U^TU$ is an identity matrix by orthonormality.
$(A^TA)V=V\Sigma^2$. 

Thus, we can find $V$ and $\Sigma$ by finding the eigenpairs for $A^TA$.
We can find $U$ and $\Sigma$ by finding the eigenpairs for $AA^T$.

**Time Complexity** when computing the full SVD using specialized methods: $O(nm^2)$ or $O(n^2m)$ whichever is less. But less work if we just wnat singular values, or if we want the first k singular vectors, or if the matrix is sparse.

## Using SVD
Q: Find users that like movie 'Matrix' but havent watched it.
A: Map query into the '**concept space**' for Matrix.
  - That is, project each user matrix $d$ into the dimension defined by 'Matrix' by dot product
  - $d_{concept} = dV$
  - Project each 


Advantages of SVD: Optimal low-rank approximation (in Frobenius norm)
Drawbacks of SVD:
- Interprability problem
- Lack of Sparsity (Singular vectors are DENSE, even if original matrices are sparse!) (1)


# CUR Decomposition
CUR is a provably good approximation to SVD (3). It performs much better than SVD space and time-wise on sparse data (4). The tradeoff is introducing some small error. 
Motivation: It is common for the matrix A we wish to decompose to be very sparse. But U and V from an SVD decomposition will not be sparse. **CUR** decomposition solves this problem by using only (randomly chosen) rows and columns of A.

Goal: Express A as a product of matrices C, U, R.
  Make $||A - C\cdot U\cdot R||_F$ small
  Matrix U is not diagonal, it is the dense pseudo-inverse of the intersection of C and R.
  Constraints on C and R:

![Screenshot 2019-10-23 at 10.54.49 PM.png](/assets/blog_resources/595F105B0E276CC236CD10A9A1B325E5.png)![Screenshot 2019-10-23 at 10.56.31 PM.png](/assets/blog_resources/2029991877F0B89CB1E7395E91AF6677.png)

Let $W$ be the 'intersection of sampled columns C and rows R.
Def: $W+$ is the pseudoinverse
  Let SVD of $W = XZY^T$
  Then: $W^+ = YZ+X^T$
  where $Z+$ is the reciprocals of non-zero singular values Z^+_{ii} = 1/Z_{ii}. The pseudinvest is as lcose to the real inverse as you can get (2)

How to sample the rows and columns? We must decrease the exected error between A and its decomposition, so we much pick rows and columns in a non-uniform nammer. We pick rows and coluymns wiht probability proportional to the square of its Frobenium norm (its 'importance')

# CUR Ago:

![Screenshot 2019-10-23 at 11.07.32 PM.png](/assets/blog_resources/0DA8A325D519274B1ABE3594C57B5B4E.png)

What CUR Computes: ![Screenshot 2019-10-23 at 11.13.57 PM.png](/assets/blog_resources/356ED26ABC360C1EC38509F3C169CEC1.png)

Rought untuition: CUR is more likely to pick poiunts away from the origin. Assuming smooth data with no outliers these are the directions of maximum variation.

### CUR Pros:

- Easy interoperation
 - Since the basis vectors are actual columns and rows
- Sparse Basis
 - Since the basis vectors are actual columns and rows

### CUR Cons
 - Duplicate rows and cols - columns fo large norms will be sampled many times.



**Summary**

![Screenshot 2019-10-23 at 11.14.26 PM.png](/assets/blog_resources/D760D338DD2BE0B617D85022AE6AA6F7.png)

(1)

![Screenshot 2019-10-23 at 10.52.18 PM.png](/assets/blog_resources/12CFADDAEEA5698B506A7E21F914D4AA.png)

(2) Note about the pseudoinverse:

 - $W=XZY^T$ then due to normality $W^{-1} = (Y^T)^^{-1}Z^{-1}X^{-1} = YZ^{-1}X^T$. Since Z is diagonal, $Z^{-1}=1/Z\_{ii}$. Thus if W is invertible, pseudoinverse is the true inverse. If W is non-invertible, the pseudoinvese is as close to the real inverse as you can get.

(3) CUR is a O(k log k) approximation to SVD, where k is the rank of A

![Screenshot 2019-10-23 at 11.10.17 PM.png](/assets/blog_resources/9D52DF8E3CF5924CE965E737B50543DF.png)

(4)CUR Much more space and time efficient than SVD for very sparse matrices.

![Screenshot 2019-10-23 at 11.16.01 PM.png](/assets/blog_resources/F3DB11C45851169C14C622668983EAA1.png)