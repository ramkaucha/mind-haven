
System of linear equations play a central part of linear algebra. Many problems can be formulated as systems of linear equations, and linear algebra gives us the tools for solving them

Example:
A company produces products $N_1,...,N_n$ for which resources $R_1,...,R_m$ are required. To produce a unit of product $N_j,a_{ij}$ units of resources $R_i$ is needed, which $i = 1,..., m$ and $j = 1,...,n$.
The objective is to find an optimal production plan, i.e. a plan of how many units $x_j$ of products $N_j$ should be produced if a total of $b_i$ units of resources $R_i$ are available and (ideally) no resources are left over.

If we produce $x_i, ..., x_n$ units of the corresponding products, we need a total of $$a_{i1}x_1 + ... + a_{in}x_n$$
many units of resources $R_i$. An optimal product plan $(x_1, ...x_n) \in R^n$, therefore, has to satisfy the following system of requirements:
$$a_{11}x_1 + ... +a_{1n}x_n = b_1$$
$$a_{m1}x_1 + ... + a_{mn}x_n = b_m$$
Equation is the general form of a *system of linear equations*, and $x_1,...,x_n$ are the unknowns of the system. Every n-tuple $(x_1, ...,x_n) \in R^n$ that satisfies is a solution of this linear equation system.
