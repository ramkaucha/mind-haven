
# Summary:
Logic design is concerned with finding the cheapest implementation of combinational (combinatorial) logic functions
- circuit costs = sum of the number of gates and the total number of gate inputs
Laws of Boolean algebra can be used to minimise the cost of a combinational logic function
- these are applied one at a time with the objective of eliminating one or more variables from a term or whole terms from the function
There are two canonical representations of combinational logic functions:
- Canonical sum-of-products represents a function as a sum (OR) of minterms
- Canonical product-of-sums represents a function as a product (AND) of maxterms



## Binary Switch
The simplest binary element is a switch that has 2 states 
![[Pasted image 20240908101349.png]]
the switch is controlled by input variable x, and that the switch is open if x = 0 and closed if x = 1

#### Example: Light controlled by a switch
the state of the light L is dependent on the state of the switch
the light is on, L = 1, if the switch is closed, x = 1, and vise versa
$L(x) = x$
![[Pasted image 20240908101745.png]]
![[Pasted image 20240908101802.png]]
An equivalent circuit using a ground connection as the return path

### AND function
The logical AND function (series connection)
![[Pasted image 20240908102358.png]]
In a series connection, both switches need to be closed for the light to be on.
$L(x_1, x_2) = x_1 * x_2$

### OR function
The logical OR function (parallel connection)
![[Pasted image 20240908102454.png]]
In a parallel connection, either one of the switches needs to be closed for the light to be on
$L(x_1, x_2) = x_1 + x_2$

### Series-parallel connection
![[Pasted image 20240908102541.png]]
$L(x_1, x_2, x_3) = (x_1 + x_2) * x_3$

*Which function determines the output of the light* 
Ans: The AND function here determines the output of the light. Either $x_1$ or $x_2$ needs to be closed, AND $x_3$ needs to be closed for the light to be on.

*How many possible states are there for this circuit*
EITHER - check lec ans
2 states - on and off
6 states

*How many of these have the light on*
2 states

### Inversion
![[Pasted image 20240908103347.png]]4
When the switch is closed in this circuit, it short-circuits the potential difference across the light
$L(x) = \overline{x}$, where L = 1 if x = 0 and L = 0 if x = 1
There are many ways to represent his complement operation: $\overline{x} = x' = !x = ~x =$ NOT x

Truth tables for the AND, OR and NOT operations
![[Pasted image 20240909205628.png]]

## Basic gate symbols
Each logic operation can be implemented electronically, with transistors, resulting in a circuit element called a **logic gate**
each logic gate has one or more inputs and one output that is a function of its inputs
a logical circuit is often described by drawing a circuit diagram, or schematic, consisting of graphical symbols representing the logic gates
![[Pasted image 20240909205916.png]]

The function from L01/S15
![[Pasted image 20240909205932.png]]
- larger circuit is implemented by a network of gates
- these are called logic networks or logic circuits
- complexity of a given network (in terms of the gate count & number of gate inputs) has a direct impact on its cost
- in order to reduce manufactured cost, we seek ways to implement logic circuits as inexpensively as possible
	- smaller (simpler) circuits are faster, require less energy to operate, and are less susceptible to failure
![[Pasted image 20240909210704.png]]
![[Pasted image 20240909210716.png]]

## Axioms and theorems of Boolean algebra
The theory underlying the simplification of logic expressions and their circuit realisation is founded on the axioms and the theorems of *Boolean algebra*
![[Pasted image 20240909211017.png]]
![[Pasted image 20240909211523.png]]
![[Pasted image 20240909211036.png]]

Example:
$f = x_2 + x_1\overline{x}_3 => \overline{f} = \overline{x_2 + x_1 \overline{x_3}}$
				$=\overline{x}_2 * \overline{x_1 \overline{x_3}}$
				$=\overline{x_2} * (\overline{x_1} + \overline{\overline{x_3}})$
				$=\overline{x_2} * (\overline{x_1} + x_3)$

**Duality** - dual of boolean expression is derived by replacing * by +, + by , 0 by 1, and 1 by 0. and leaving variables unchanged. Any theorem can be proven is thus also proven for its dual, meta-theorem (a theorem about theorem)
![[Pasted image 20240909211507.png]]
Differs from deMorgan's Law - duality is a statement about theorems, duality is not a way of manipulating (re-writing) expressions

proofs of the axioms and theorems of Boolean algebra can be established in various ways, for example the absorption theorem (X + X * Y = X) may be proven deductively, exhaustively or using set theory

X + X * Y = X * 1 + X * Y by identity
		  = X * (1 + Y) by distributivity
		  = X * 1 by null
		  = X by identity
![[Pasted image 20240909211842.png|200]]
![[Pasted image 20240909211858.png]]
Using set theory, the surrounding box represents the universe, logical AND is given by the intersection of sets and logical OR is given by the union of sets

## Precedence of operations
in the absence of parentheses, operations in a logic expression must be performed in the order:
NOT, AND, and then OR
thus,
$x_1*x_2 + \overline{x_1}*\overline{x_2}$ has the same effect as $(x_1*x_2) = ((\overline{x_1})*(\overline{x_2}))$
To simplify the appearance of logic expressions it is customary to omit the * operator when there is no ambiguity
- the preceding expression can therefore be written as $x_1x_2+\overline{x_1}\overline{x_2}$
![[Pasted image 20240909212602.png|500]]
(b) circuit cost = 17 (6 gates, 11 gate inputs)
(c) circuit cost = 5 (2 gates, 3 gate inputs)
## Evaluating circuit cost
the primary contributor to circuit cost is the number of transistors used
`#` (Transistors used) ~~ circuit (chip) area
circuits with fewer gates and gates with fewer wires (inputs) require fewer transistors => less chip area
measure of circuit cost = sum of the number of gates and the total number of gate inputs

### Synthesis technique for logic circuits
1. Add a product (AND) term for each row in the truth table with the function value f = 1
2. Form the sum (OR) of these terms to produce the function f
3. Simplify the expression using Boolean algebraic manipulation
4. Draw the circuit, complementing inputs where necessary, using AND gates for the product terms and ORing these together

## Minterms
for a function of n variables, a product term in which each of the n variables, $x_j$, appears exactly once is called a minterm, $m_i$, $0 <= i <= 2^n$
- appears in uncomplemented or complemented form
- given row i of the truth table, minterm is formed by including $x_j$ if $x_j = 1$ and by including $\overline{x_j}$ if $x_j = 0$ in minterm $m_i$
- note that $m_i = 1$ in row i of the truth table and $m_i = 0$ in all other rows
Note: minterms are numbered according to which variable appear in uncomplemented form

Good video: https://www.youtube.com/watch?v=TMxMPJeJ_18
## Sum-of-products form
function f can be represented by an expression that is written as sum of minterms, where each minterm is ANDed with the value of f for the corresponding valuation of input variables
![[Pasted image 20240909214315.png]]
a logic expression consisting of product (AND) terms that are summed (ORed) is said to be in the *sum-of-products (SOP)* form
if each product term is a minterm, then the expression is called a canonical sum-of-product for the function f
- every Boolean `fn` has just one *canonical* SOP representation

## Maxterms
principle of duality suggests that if it is possible to synthesize a function f by considering the rows in the truth table for which f = 1, then it should also be possible to synthesize f by considering the rows for which f = 0
maxterms - uses the complements of minterms

### Three-variable minterms and maxterms
![[Pasted image 20240909214957.png]]

the value of the example minterm $m_0$ and maxterm $M_0$ for each valuation of the variables $x_1, x_2$ and $x_3$
**Note** that maxterm $M_i = 0$ in row i of the truth table and $M_i = 1$ for all other rows

## Product-of-Sums form
If a given function f is specified by a truth table, then its complement $\overline{f}$ can be represented by a sum of minterms for which $\overline{f} = 1$, which are the rows of $f$ where $f = 0$
$f = \overline{m_2} = M_2 = \Pi(M_2) = \Pi M(2)$
product-of-sums (POS) form - logic expression consisting of sum (OR) terms that are factors of a logical product (AND)
If each sum term is a maxterm, then the expression is called a *canonical product-of-sums* for the given function

[Example Exercises](onenote:https://unsw-my.sharepoint.com/personal/z5367751_ad_unsw_edu_au/Documents/COMP3222/New%20Section%201.one#Example%20Exercises&section-id={5513AF83-90B5-403E-82EB-AF5BE845484A}&page-id={203344A1-18C7-4D0A-AACF-28E838F51D30}&end)  ([Web view](https://unsw-my.sharepoint.com/personal/z5367751_ad_unsw_edu_au/_layouts/OneNote.aspx?id=%2Fpersonal%2Fz5367751_ad_unsw_edu_au%2FDocuments%2FCOMP3222&wd=target%28New%20Section%201.one%7C5513AF83-90B5-403E-82EB-AF5BE845484A%2FExample%20Exercises%7C203344A1-18C7-4D0A-AACF-28E838F51D30%2F%29))