Time complexity of an algorithm estimates how much time the algorithm will use for some input, the idea is to represent the efficiency as a function whose parameter is the size of the input. By calculating the time complexity, we can find out whether the algorithm is fast enough without implementing it.[^1] 

## Calculation rules
Time complexity of an algorithm is denoted by $O(...)$, where the dots represent some function, with variable $n$ which denotes the *input* size.

### Loops
The more nested loops the algorithm contains, the slower it is, if there are $k$ nested loops, the time complexity is $O(n^k)$
e.g. time complexity for the following is $O(n)$
```cpp
for (int i = 0; i <= n; i++) {
	// code
}
```

and the time complexity for this is $O(n^2)$
```cpp
for (int i = 0; i <= n; i++) {
	for (int j = 0; j <= n; j++) {
		// code
	}
}
```

### Order of Magnitude
The time complexity only shows the order of magnitude, it does not tell us the exact number of times the code inside a loop is executed.
e.g. the code below is executed `n / 2`, but the time complexity of it is still $O(n)$
```cpp
for (int i = 1; i <= n; i += 2) {
	// code
}
```

### Phases
If algorithm consists of phases, the total time complexity is the largest time complexity of a single phase. IT DOES NOT ADD UP
```cpp
for (int i = 0; i <= n; i++) {
	// code;
}

for (int i = 1; i <= n; i++) {
	for (int j = 1; j <= n; j++) {
		// code
	}
}

for (int i = 1; i <= n; i++) {
	// code
}
```
here, the total time complexity is $O(n^2)$

### Several variables
e.g. time complexity of the following code is $O(nm)$
```cpp
for (int i = 0; i <= n; i++) {
	for (int j = 0; j <= m; j++) {
		// code
	}
}
```

### Recursion
Time complexity of recursive function depends on the number of times the function is called, and the time complexity of a single call.
Total time complexity is the **product** of these values.
e.g.
```cpp
void f(int n) {
	if (n == 1) return;
	f(n-1);
}
```
the call `f(n)` causes $n$ function calls, and the time complexity of each call is $O(1)$, so the total time complexity is $O(n)$

another example.
```cpp
void g(int n) {
	if (n == 1) return;

	g(n-1);
	g(n-1);
}
```
In this cash, each function call generates two other calls, except for `n = 1`.


| function call | number of calls |
| ------------- | --------------- |
| $g(n)$        | 1               |
| $g(n-1)$      | 2               |
| $g(n-2)$      | 4               |
| ...           | ...             |
| $g(1)$        | $2^{n-1}$       |
Hence, the time complexity would be $O(2^{n-1})$

### Complexity classes

Common complexities in algorithms:

$O(1)$ - the running time, of a *constant-time* algorithm does not depend on the input size. A typical constant-time algorithm is a direct formula that calculates the answer

$O(logn)$ - logarithmic algorithm often halves the input size at each step. Running time of such an algorithm is logarithmic, because $log_2n$ equals the number of times $n$ must be divided by 2 to get 1.

$O(\sqrt{n})$ - square root algorithm is slower than $O(logn)$ but faster than $O(n)$, special property of square roots is that $\sqrt{n} = n| \sqrt{n}$, so the square root $\sqrt{n}$ lies, in some cases, in the middle of the input

$O(n)$ - linear algorithm goes through the input a constant number of times. Often best possible time complexity, because it is usually necessary to access each input element once before reporting the answer.

$O(nlogn)$ - time complexity often indicates that the algorithm sorts the input, because the time complexity of efficiency sorting algorithm is $O(nlogn)$. Another possibility is that the algorithm uses a data structure where each operating takes $O(logn)$ time

$O(n^2)$ - quadratic algorithm often contains two nested loops, it is possible to go through all pairs of the input elements in $O(n^2)$ time.

$O(n^3)$ - cubic algorithm often contains three nested loops, it is possible to go through all triplets of the input elements in $O(n^3)$ time

$O(2^n)$ - time complexity often indicates that the algo iterates through all subset of the input elements, e.g. subset of {1,2,3} are {1}, {2}, {1,2} ....

$O(n!)$ - often indicates that the algo iterates through all permutation of the input elements, e.g. perm of {1,2,3} are (1,2,3), (1,3,2) ....

An algo is polynomial if its time complexity is at most $O(n^k)$, where $k$ is a constant. All the above time complexities expect $O(2^n)$ and $O(n!)$ are polynomial.

In practice, $k$ is usually small, and therefore a polynomial time complexity roughly means that the algorithm is efficient.


[^1]: [Competitive Programming Handbook](https://cses.fi/book/book.pdf
