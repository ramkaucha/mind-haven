
## Lab 07 Part 2:

List the pseudocode for your binary search algorithm
```
Init:
- target = input value to search for
- left = 0
- right = 31 (array size - 1)

while (left <= right):
	mid = (left + right) / 2
	current = memory[mid]

	if (current = mid):
		return found = true, addres = mid
	else if (current < target):
		left = mid + 1
	else
		left = mid - 1

return found = false
```


Sketch the ASM chart for your design,
![[ASM.excalidraw]]

Sketch the datapath for your design, label all signals including all internal wires and indicate the bit width of signals that are wider than a single bit
![[datapath.excalidraw|2000]]



Refine your ASM chart with the names of the signals that are to be asserted/tested during each state or state transition
![[ASM.excalidraw 1]]
