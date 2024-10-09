
| Objectives                                                                                | Check |
| ----------------------------------------------------------------------------------------- | ----- |
| Representation of numbers in computers                                                    | []    |
| Circuits for performing arithmetic operations                                             | []    |
| VHDL for specifying arithmetic circuits                                                   | []    |
| Performance issues in large circuits<br>- How architectural choices influence performance | []    |

## Unsigned integer representation
*unsigned* - positive only numbers
*signed* - can be negative number

## Binary addition - the half-adder circuit
![[Pasted image 20241009163137.png]]

### Binary addition of more than 2 bits
in general, this requires at position *i* the addition of three bits (two inputs and a carry-in from position i-1) and results in a sum bit and a carry-out bit for the next (more significant) position to the left
![[Pasted image 20241009163415.png]]

## The full-adder circuit
Consider the addition of two bits and a carry in at each bit position, resulting in a sum bit and a carry out
![[Pasted image 20241009165719.png]]
**note**: (NOT A AND B) OR (A AND NOT B) = A XOR B

### Decomposed full-adder
![[Pasted image 20241009165808.png]]

## N-bit ripple-carry adder
Just a manual decimal addition, binary addition can be performed by adding pairs of bits together starting at the least-significant bit (l.s.b) position - a carry-out of position i is added to the operands in positions i + 1
**n-bit ripple-carry adder is just n amount of full adders**
![[Pasted image 20241009170125.png]]

When the operands X and Y are applied to the adder, it takes some time before the output S is valid because the carries have to be determined first
	if the time to produce the carry-out from the l.s.b is $\bigtriangleup t$, and this delay is identical for all adder stages, then the delay for the carry-out to be produced from the m.s.b (most significant bit) is $n \bigtriangleup t$ 
	the adder derives its name from the manner in which carries ripple through the adder from right to left (imagine changing Y = 0..00 -> 0..01 after adding Y to X = 01...1)
### VHDL Code for full-adder
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;

ENTITY fulladd IS 
	PORT (Cin, x, y     : IN STD_LOGIC;
		   s, Cout       : IN STD_LOGIC );
END fulladd;

ARCHITECTURE LogicFunc OF fulladd IS
BEGIN
	s <= x XOR y XOR Cin;
	Cout <= (x AND y) OR (Cin AND x) OR (Cin AND y);
END LogicFunc;
```

### VHDL Code for a four-bit adder
![[Pasted image 20241009170805.png]]

## Declaration of a user-defined package
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;
...
-- your component desriptions (entities & architectures)
...
PACKAGE fulladd_package IS
	COMPONENT fulladd
		PORT (Cin, x, y         : IN STD_LOGIC;
			   s, Cout          : OUT STD_LOGIC);
	END COMPONENT;
END fulladd_package;
```
 avoids need to declare the component in the architecture part
 allows component to be reused across multiple projects
 the package declaration can appear at the end of the source file containing the component(s)
## Instantiation of component using package
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;
USE work.fulladd_package.all; -- user-defined package in current, "working" directory eliminates need to explicitly declare fulladd component

ENTITY adder4 IS
	PORT ( Cin                      : IN STD_LOGIC;
		   x3, x2, x1, x0       : IN STD_LOGIC;
		   y3, y2, y1, y0       : IN STD_LOGIC;
		   s3, s2, s1, s0        : OUT STD_LOGIC;
		   Cout                    : OUT STD_LOGIC);
END adder4;

ARCHITECTURE Structure OF adder4 IS
	SIGNAL c1, c2, c3: STD_LOGIC;
BEGIN
	stage0: fulladd PORT MAP (Cin, x0, y0, s0, c1);
	stage1: fulladd PORT MAP (c1, x1, y1, s1, c2);
	stage2: fulladd PORT MAP (c2, x2, y2, s2, c3);
	stage3: fulladd PORT MAP (c3, x3, y3, s3, Cout);
END Structure;
```

### Using multibit signals
```vhdl
ENTITY adder4 IS
	PORT ( Cin                      : IN STD_LOGIC;
		   X, Y                     : IN STD_LOGIC_VECTOR(3 DOWNTO 0) -- declaration of signals representing numeric values 
		   S                         : OUT STD_LOGIC_VECTOR(3 DOWNTO 0);
		   Cout                    : OUT STD_LOGIC);
END adder4;

ARCHITECTURE Structure OF adder4 IS
	SIGNAL c1, c2, c3: STD_LOGIC(1 TO 3); -- declaration of collected, unrelated or non-numeric signals
BEGIN
	stage0: fulladd PORT MAP (Cin, x0, y0, s0, c1);
	stage1: fulladd PORT MAP (c1, x1, y1, s1, c2);
	stage2: fulladd PORT MAP (c2, x2, y2, s2, c3);
	stage3: fulladd PORT MAP (c3, x3, y3, s3, Cout);
END Structure;
```

## Signed number representations
![[Pasted image 20241009172000.png]]

### Three common signed representations
**Sign and magnitude** - use the same positional number representation as for unsigned numbers with a sign bit in the left-most position
**1's complement** - derive an n-bit negative number $K_1$, by subtracting equivalent positive number P from $2^n - 1$ i.e. $K_1 = (2^n - 1) - P$
	Equivalent to flipping every bit
**2's complement (preferred, as we shall see)** - Derive an n-bit negative number $K_2$ by subtracting equivalent position number P from $2^n$ i.e. $K_2 = 2^n - P = K_1 + 1$ 
	Algorithm for mentally deriving 2's complement:
		Given a signed number $B = b_{n-1}b_{n-2}....b_1b_0$, its 2's complement $K_2 = k_{n-1}k_{n-2}...k_1k_0$ can be obtained by examining the bits of B from the right to left and taking the following action:
		**copy all bits of B that are 0 and the first bit that is 1; then simply complement all the remaining bits on the left**


### Interpreting B=b3b2b1b0 as a signed integer
![[Pasted image 20241009172803.png]]
Note the two representation for zero in the *sign and magnitude* and *1's complement* forms
Note also the larger range that can be represented using *2's complement*

### 2's complement addition
![[Pasted image 20241009173202.png]]

### 2's complement subtraction
Most readily done by negating the subtrahend and adding it to the minuend
	involves finding 2's complement of the subtrahend and then performing addition
As we ignore carry-outs of the m.s.b., the same adder circuit can be used for both addition and subtraction
![[Pasted image 20241009173443.png]]

## Adder/subtractor unit
![[Pasted image 20241009173521.png]]
*XOR gates act as conditional inverters*
**Good design rule:**
	Develop circuits to be as flexible as possible and exploit common portions for as many tasks as possible
		minimises the number of gates needed and the wiring complexity


## Detecting overflow
![[Pasted image 20241009173923.png]]
When the carry-out of the m.s.b != carry-out of the sign-bit position, overflow has occurred:
$Overflow = \overline{c_3}c_4 + c_3 \overline{c_4} = c_3 \oplus c_4$
for n-bit number: 
$Overflow = c_{n-1} \oplus c_n$

## IP-based adder/substracter
using vivado's IP catalog to select an adder/subtracter
	bit width and signed/unsigned representation controllable 
		here parameterised for 16-bit wide unsigned addition
![[Pasted image 20241009174314.png]]

### Timing simulation of IP-based adder
![[Pasted image 20241009174328.png]]
Note the delay observed in updating the sum when an input changes
	a period of glitching the S output occurs as carries ripple through the adder

## Behavioural code for a 16-bit adder
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;
USE ieee.std_logic_signed.all;

ENTITY adder16 IS
	PORT ( X, Y       : IN STD_LOGIC_VECTOR(15 DOWNTO 0);
		   S           : IN STD_LOGIC_VECTOR(15 DOWNTO 0));
END adder16;

ARCHITECTURE Behaviour OF adder16 IS
BEGIN
	S <= X + Y;  -- maths ops +, - and * are synthesied by most tools 
END behaviour
```
Note use of `std_logic_signed` package
	defines arithmetic ops on signed data types
	`std_logic_unsigned` does so for data of type unsigned
	OP components of the required size are automatically inferred

## Accessing the internal signals
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;
USE ieee.std_logic_signed.all;

ENTITY adder16 IS
	PORT (Cin.       : IN STD_LOGIC;
		   X, Y       : IN STD_LOGIC_VECTOR (15 DOWNTO 0);
		   S           : OUT STD_LOGIC_VECTOR (15 DOWNTO 0);
		   Cout, Overflow : OUT STD_LOGIC);
END adder16;

ARCHITECTURE Behaviour OF adder16 IS
	SIGNAL Sum: STD_LOGIC_VECTOR(16 DOWNTO 0);
BEGIN
	Sum <= ('0' & X) + ('0' & Y) + Cin; -- note concatenation operator used
	S <= Sum (15 DOWNTO 0);
	Cout <= Sum(16);
	Overflow <= Sum(16) XOR X(15) XOR Y(15) XOR Sum(15);
END Behavior;
```

### Using inbuilt INTEGER signals
```vhdl
ENTITY adder16 IS
	PORT ( X, Y     : IN  INTEGER RANGE -32768 TO 32767;
		   S          : OUT  INTEGER RANGE -32768 TO 32767);
END adder16;

ARCHITECTURE Behavior OF adder16 IS
BEGIN
	S <= X + Y;
END Behavior;
```
Does not require library since `INTEGER` is an inbuilt type
but, accessing internal signals, so as to determine overflow, is impossible


## Performance issues
the speed of a circuit is limited by the longest delay alogn the paths through the circuit
in digital circuits, the longest delay is referred to as the *critical-path delay*, and the path that causes this delay is called the *critical path*
better performance can be achieved using faster circuits by either:
	1. Using superior gate technology (process/technology innovation) or 
	2. Changing the overall structure of a functional unit (architectural innovation)

## Fast addition - carry-lookahead addition
Carry propagation delay can be reduced by quickly evaluating the carry-in for each stage
recall, the carry-out for stage *i* can be realised as:
$c_{i+1} = x_iy_i + x_ic_i + y_ic_i$ which can be factored as $c_{i+1} = x_iy_i + (x_i + y_i)c_i$ and rewritten as $c_{i + 1} = g_i + p_ic_i$ where $g_i = x_iy_i$ and $p_i = x_i + y_i$ 

### Carry-lookahead addition
Expanding previous expression in terms of stage $i - 1$
$c_{i+ 1} = g_i + p_ig_{i -1} + p_ip_{i -1}g_{i - 2} + ... p_2p_1g_0 + p_i+p_i-1...p_1p_0c_0$
which represents a *two-level AND-OR circuit* (after the $g_i$'s and $p_i$'s are calculated) in which $c_{i + 1} which evaluated more quickly
an adder based on this expression is called a *carry-lookahead adder*


#### Ripple-carry adder based on $c_{i + 1} = g_i + p_ic_i$ 
![[Pasted image 20241009181312.png]]
Replaces one AND gate in FA circuit with an OR gate
critical path length (shown in red) 2n + 1 gates:
	all $g_i$ and $p_i$ signals available after 1 gate delay
	each stage adds 2 more gate delays to compute $c_{i + 1}$

For $c_{i+ 1} = g_i + p_ig_{i -1} + p_ip_{i -1}g_{i - 2} + ... p_2p_1g_0 + p_i+p_i-1...p_1p_0c_0$
![[Pasted image 20241009181603.png]]
All carries produced at the same time after 3 gate delays
all sum bits computed in 4 gate delays i.e. 1 gate delay after carries determined BUT, the complexity grows at every stage
	2 more wires enter/leave per stage
	1 more AND gate with one more input per stage
	one more input to OR gate per stage

### Carry-lookahead adder (CLA) with ripple-carry between blocks
the complexity of an n-bit CLA grows rapidly with n
thus, use a hierarchical approach to implement large adders
for example, split a 32-bit adder into four 8-bit blocks
	each block can be a CLA
	interconnect blocks *either* in ripple-carry fashion:
![[Pasted image 20241009181835.png]]
or using a second level of carry-lookahead:
![[Pasted image 20241009181856.png]]

### Hierarchical carry-lookahead addition
uses a second (or more) levels of carry-lookahead to reduce the time to produce carry signals between blocks
each block produces its own "block" generate and propagate signals instead of carrys-outs - denote these as $G_j$ & $P_j$ or block $j$
we can then have for block 0
![[Pasted image 20241009182048.png]]
The schema takes 3 gate delays to produce the block $G_j$ and $P_j$ functions
it takes two more gate delays to produce the carry signals $c_8, c_16$ & $c_24$ (so, 5 gate delays in total)
two more gate delays are needed to produce the carry signals for each bit within each block, and one more to produce the sum bits
	after inputs change, a total of 8 gate delays are required to compute the result (critical path computes $s_{32}$)
	in contrast, a 32-bit ripple-carry adder uses 64 gates delays to compute a result (critical path computes $c_{32}$)