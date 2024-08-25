Matrices can be used to compactly represent systems of linear equations, but they also represent linear functions (linear mappings).

**Definition** Matrix - with $m,n \in N$ a real-valued (m,n) matrix A is a $m*n$ tuple of elements $a_{ij}, i = 1,...,m, j = 1,..., n$ which is ordered according to a rectangular scheme consisting of $m$ rows and $n$ columns:
![[Pasted image 20240825182600.png]]

By convention (1, n)-matrices are called rows and (m,1)-matrices are called columns. These special matrices are also called row/column vectors.

$R^{m \times n}$ is the set of all real-valued (m,n)-matrices. $A \in R^{m \times n}$ can be equivalently represented as $a \in R^{mn}$ by stacking by all $n$ columns of the matrix into a long vector.

The sum of two matrices $A \in R^{m \times n}, B \in R^{m \times n}$ is defined as the element-wise sum, i.e.
![[Pasted image 20240825182824.png]]

For matrices, $A \in R^{m \times n}, B \in R^{n \times k}$, the elements $c_{ij}$ of the project $C = AB \in R^{m \times k}$ are computed as:
![[Pasted image 20240825182926.png]]

This means, to compute elements $c_{ij}$ ,we multiply the elements of the $ith$ row of A with the $jth$ column of B and sum them up. 

Matrices can only be multiplied if their neighbouring dimensions matches. $n \times k$ -matrix A can be multiplied with a $k \times m$-matrix B, but only from the left side.

**Definition** Identity matrix - as the $n \times n$-matrix containing 1 on the diagonal and 0 everywhere else.
![[Pasted image 20240825183540.png]]

**Definition** Inverse - consider a square matrix $A \in R^{n \times n}$. Let matrix $B \in R^{n \times n}$ have the property that $AB = I_n = BA$. B is called the inverse of $A$ and denoted by $A^{-1}$

not every matrix has an inverse, if inverse does exist, it is called regular/invertible/nonsingular, otherwise singular / noninvertible.
![[Pasted image 20240825183939.png]]

