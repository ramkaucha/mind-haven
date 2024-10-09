
## Karnaugh map

The key to finding a minimum-cost expression for a given logic function is to reduce the number of product (or sum) terms needed in the expression by applying the combining/uniting theorem
$x*y + x*y' = x$ and $(x + y)*(x+y') = x$ as judiciously as possible.

Karnaugh map approach provides a systematic way of performing this optimisation

### Karnaugh map layout
![[Pasted image 20241001221020.png]]

Note that the addresses of horizontally and vertically adjacent cells in the Karnaugh map differ in a single bit which facilitates their combination
![[Pasted image 20241001221108.png]]

four variable Karnaugh map
![[Pasted image 20241001221131.png]]
Note that both row and col addresses are Gray coded
Note that opposite edges of the maps are considered adjacent

five variable Karnaugh map
![[Pasted image 20241001221243.png]]

## Minimisation strategy

**Literal** - each appearance of a variable in a product term
**Implicant**- a product term that indicates a set of valuations for which a given function is equal 1. (Implicants are expanded by square/rectangular powers of 2 to form PIs)
**Prime implicant** -  cannot be combined into another implicant that has fewer literals
**Cover** - a collection of implicants that account for all valuations for which a given function is equal 1, a cover consisting only of prime implicants leads to a minimal cost implementation
**Cost** - Use the number of gates plus the total number of inputs to gates, but we assume that the primary inputs are available in both true and complemented form at zero cost -i.e. don't count input inverters

### Essential prime implicants
If a prime implicant includes a minterm for which f = 1, that is not included in any other prime implicant, then it must be included in any other prime implicant, then it must be included in the cover and is referred to as *essential prime implicants*
If the set of essential prime implicants covers all valuations for which f = 1, then this set is the desired cover of f. Otherwise, determine the nonessential prime implicants that should be included to form a minimum-cost cover

### Heuristic choice of nonessential prime implicants
The choice of nonessential prime implicants to be included in the cover is governed by cost considerations
The choice is often not obvious and, for functions of a large number of variables, may involve assessing many possibilities
A *heuristic* approach, which considers only a subset of possibilities but gives good results most of the time, is used
	Arbitrarily select one nonessential prime implicant and determine the cost of the resulting cover;
	then, determine the cost of a cover that does not include the chosen prime implicant; and
	select for implementation the cover which costs least

### Practical Reality: Incompletely specified functions
Often functions are not completely specified, the outputs under certain input conditions are not given - treat them as 'don't care (d)' values
Don't care values offer additional opportunities for simplification (can be assigned value 0 or 1, as desired)

### Practical Reality: Multiple-output synthesis
![[Pasted image 20241001222915.png]]

### Practical Reality: Factoring for multilevel implementation
![[Pasted image 20241001222932.png]]
Electrical properties limit the number of inputs a logic gate can have before performance is comprimised
Similarly, manufacturing constraints may impose an upper limit on the number of inputs to a logic resouirce
	factoring a circuit or function allows a function to be implemented from a simpler sub-functions
	In the above derivation, 4-in AND and OR gates are needed to implement f naively, but suppose the implementation technology we use is restricted to 2-input gates?
	Then a multilevel (more than two levels of gates) function/circuit implementation with **slower signal propagation** can be used

Factoring can be used to overcome fan-in constraints
![[Pasted image 20241001223518.png]]

## Two-input lookup table (LUT) as an FPGA configurable logic block
![[Pasted image 20241001223347.png]]
### Simplified view of a section of programmed FPGA
![[Pasted image 20241001223410.png]]

### Three-input LUT
![[Pasted image 20241001223423.png]]


## Practical Tradeoff - functional decomposition
Multilevel realisation can reduce the cost of a circuit but incur increased propagation delay penalties
![[Pasted image 20241001223806.png]]

## NAND and NOR gates
NAND & NOR gate circuit realisation of SOP/POS networks are preferred over AND/OR/NOT gate realisation because they require less transistors to implement
![[Pasted image 20241001224240.png]]

### DeMorgan's theorem in terms of logic gates
Allows us to transform AND-OR networks into NAND-only OR NOR-only equivalent networks
![[Pasted image 20241001224319.png]]

### Using NAND gates to implement a SOP network
![[Pasted image 20241001224343.png]]

### Using NOR gates to implement a POS network
![[Pasted image 20241001224402.png]]

## Multilevel NAND-gate circuit
![[Pasted image 20241001224423.png]]

## Equivalent NOR-gate circuit
![[Pasted image 20241001224438.png]]

## Analysis of multilevel circuit
![[Pasted image 20241001224554.png]]

## Analysing multilevel NOR/NAND-only circuits
![[Pasted image 20241009161359.png]]
