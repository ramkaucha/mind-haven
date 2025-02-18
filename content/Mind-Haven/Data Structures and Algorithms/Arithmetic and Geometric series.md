To analyse the time complexity of an algo, often have to sum a large number of terms.

In arithmetic series, the terms have *common difference*, gathering like term gives
$$a + (a+d) + (a+2d) + ...+ (a+(n-1)d) = na + \frac{n(n-1)}{2}d$$
In geometric series, the terms have *common ratio*, factorising gives
$$a + ar + ar^2 + ...+ ar^{n -1} = a \frac{1-r^n}{1-r}$$
In particular, if $|r| < 1$, then as the number of terms increase to infinity, the term $r^n$ approaches zero, so the sum converges
$$a + ar + ar^2 + ...  = \frac{a}{1-r}$$
