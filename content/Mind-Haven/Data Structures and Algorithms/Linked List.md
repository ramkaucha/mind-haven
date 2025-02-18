Linked list stores each item with a link to the next to the next item. In a doubly linked list, each item also includes a link to the previous item. 

#COMP3121 only discusses doubly linked lists unless specified

Linked list operations:
	accessing the next or previous item only takes $O(1)$ by following the relevant link
	random access takes $O(n)$ in the worst case, as we need to follow links one at a time until we reach the desired index
	Insertion and deletion from a designated position both take $O(1)$ as we only need to modify up to four links
	Search takes $O(n)$ in the worst case.
