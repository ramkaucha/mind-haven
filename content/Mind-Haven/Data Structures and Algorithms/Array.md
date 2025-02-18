Stores $n$ items with consecutive indicies.

#COMP3121 use the notation convention that indicies are numbers $1,...,n$ (rather than $0,..., n-1$)

The entire array by $A : [3,1,2,1,9,1,0,1]$
A subarray by $A[3..6] : [2,1,9,1]$ and 
an array element by $A[5] = 9$

#COMP3121 only talks about static arrays, i.e. arrays of fixed size.

Array operations:
	Random access takes $O(1)$; given an index $i$, we can directly access and modify the associated element $A[i]$
	insertion or deletion takes $O(n)$ as we need to recreate the array
	without special structure, search takes $O(n)$ time in the worst case as we may need to check every element of the array.
