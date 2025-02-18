Logarithm is the inverse function of the exponential.

For $a,b > 0$ and $a \neq 1$, we say $n = log_ab$ if $a^n = b$,. 
Thus, $log_ab$ can be read as 'to what power do we raise $a$ in order to get $b$?'

Some properties of logarithms are:
- $a^{log_ab} =b$, from the definition of logarithm
- $log_a(bc) = log_ab + log_ac$, from the index law $a^m a^n = a^{m+n}$
- $log_a(b^k) = klogb$, from the index law $(a^n)^k = a^{kn}$

Logarithms are ubiquitous in the analysis of DSA and Algos, e.g. height of a perfect binary tree with $n$ nodes is $log_2n + 1$.

#COMP3121 will be using base 2 unless specified, but this often doesn't matter because of the *change of base rule*. For $a,b,n > 0$ and $a,b \neq 1$, we have 
$$log_an = \frac{log_bn}{log_ba}$$
If $a$ and $b$ are constants and $n$ is a variable, then the denominator is a constant, so $log_an$ is just a constant factor times $log_bn$, in complexity analysis, we can ignore constant factors, which makes logs of any base interchangeable. Therefore, we can write $logn$, suppressing the base.

**BE CAREFUL**: this only applies for 'bare' logarithms. Constant factors in *exponents* cannot be ignored, just as $n^2$ and $n^3$ are not equivalent, by virtue of having constant indices, $2^{log_2n}$ and $2^{log_3n}$ are not equivalent either.
