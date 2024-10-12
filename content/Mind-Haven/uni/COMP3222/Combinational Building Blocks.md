
| Objective                                                                                                                                                                              | Check |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----- |
| Learn about commonly used combinational sub-circuits<br>- multiplexers, sued for signal selection and implementing general logic functions<br>- encoders, decoders and code converters | []    |
| Learn about the key VHDL constructs used to specify combinational circuits<br>- non-simple, concurrent assignment statements<br>- sequential statements                                | []    |

## Multiplexers
MUX has a number of data inputs, one or more select inputs, and one output that
	passes the signal value on one of the data inputs to the output
	the data input is select by the values of the the select inputs
a 2-to-1 MUX, which has 2 data inputs and therefore 1 select input, is shown below
	this 2-to-1 MUX implements the function $f = \overline{s}*w_0 + s*w_1$
![[Pasted image 20241009183247.png]]

### A 4-to-1 multiplexer
![[Pasted image 20241009183317.png]]
A 4-to-1 MUX, which has 4 data and 2 select inputs, realises the function
$f = \overline{S_1} \overline{S_0}W_0 + \overline{S_1}S_0W_1 + S_1 \overline{S_0}W_2 + S_1S_0W_3$
larger MUXes can be built using the same approach, but can also be built from smaller MUXes

### Using 2-to-1 MUXes to build a 4-to-1 MUX
![[Pasted image 20241009183703.png]]


### 16-to-1 multiplexer built from 4-to-1 MUXes
![[Pasted image 20241009214124.png]]

## Practical applications of multiplexers
a circuit that has n inputs and k outputs, whose function it is to provide the capability to connect any input to any output is called *n x k crossbar switch*
a 2 x 2 crossbar switch can easily be built using two 2-to-1 multiplexers
![[Pasted image 20241009214241.png]]

### Synthesis of logic functions using multiplexers
Using MUXes to implement functions
Example: consider the XOR function below, 
	each row of the truth table can be connected to a MUX input as a constant
	the select inputs are driven by the variables of the function
but we can be more efficient if we rewrite the truth table
![[Pasted image 20241009214507.png]]
### 3-input majority fn using a 4-to-1 MUX
![[Pasted image 20241009215050.png]]
*note* - for the majority function, any variable pair could be used as the selector inputs without changing the circuit structure


## Decoders
Used to decode encoded information
*binary decoder* (DEC), is a logic circuit with $n$ inputs and $2^n$ outputs
	only one output is asserted at a time, and each output corresponds to one valuation of the inputs
	has an enable input *En* that is used to disable the outputs so that no outputs is asserted when *En* = 0\
![[Pasted image 20241009215302.png]]

### 2-to-4 binary decoder
![[Pasted image 20241009215319.png]]
a n-bit binary code in which exactly one of the bits is set to 1 at a time is said to be *one-hot encoded*
	the single bit that is set to 1 is said to be 'hot'
the outputs of a binary decoder are one-hot encoded
larger decoders can be built using the same structure of (c), or they can be built from smaller decoders

### 3-to-8 decoder using two 2-to-4 decoders
![[Pasted image 20241009221133.png]]

### 4-to-16 decoder built using a decoder tree
![[Pasted image 20241009221149.png]]

### 4-to-1 MUX built using a decoder
![[Pasted image 20241009221211.png]]

## Demultiplexing
note that a decoder on its own can also be used as a $1-to-2^n$ demultiplexer
	the $En$ input plays the role of data-in and on of the $2^n$ outputs is selected using the $n$ select bits
![[Pasted image 20241009221307.png]]


## Encoders
performs the opposite function of a decoder
	encodes given information into a more compact form
*binary encoder* encodes information from $2^n$ inputs into a n-bit code as shown below
	exactly one of the input signal should have a value of 1, and the outputs present the binary number that identifies which input is equal to 1
![[Pasted image 20241009221451.png]]

### 4-to-2 binary encoder
![[Pasted image 20241009221518.png]]

### Priority encoders
In a priority encoder more than one input may be asserted, and each input has a priority level associated with it
the encoder outputs indicate the active input that has the highest priority
truth table for a 4-to-2 priority encoder
![[Pasted image 20241009221609.png]]
the previous techniques can be used to synthesise the output functions
however, a more convenient approach is to define intermediate signals
$i_0 = \overline{w_3} \overline{w_2} \overline{w_1} w_0$
$i_1 = \overline{w_3w_2}w_1$
$i_2 = \overline{w_3}w_2$
$i_3 = w_3$
the outputs can then be written as:
$y_0 = i_1 + i_3$
$y_1 = i_2 + i_3$
$z = i_0 + i_1 + i_2 + i_3$

## Combinational VHDL
non-simple assignment statements
	selected assignment
	conditional assignment
sequential statements
	if-then-else
	case
concurrent statements
	process vs assignment statements

### VHDL code for 2-to-1 multiplexer using a select signal assignment
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;

entity mux2to1 is
	port (w0, w1, s: IN STD_LOGIC;
			f: OUT STD_LOGIC);
end mux2to1;

architecture Behavior of mux2to1 is
begin
	-- then 'with x select' idiom always infers a multiplexer. so use it when design calls for it
	with s select
		f <= w0 when '0'
			w1 when others; -- all possible valuations of the condition 's' need to be considered
end Behavior
```

### VHDL code for a 4-to-1 multiplexer
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity mux4to1 is
	port (w0, w1, w2, w3 : in std_logic;
			s : in std_logic_vector(1 downto 0);
			f : out std_logic);
end mux4to1;

architecture Behavior of mux4to1 is
begin
	with s select
		f <= w0 when "00", -- note "" used to multi constant; '' used for single bit constants
			w1 when "01",
			w2 when "10",
			w3 when others;
end Behavior;

package mux4to1_package is
	component mux4to1
		port (w0, w1, w2, w3: in std_logic;
				s : in std_logic_vector(1 downto 0);
				f : out std_logic);
	end component;
end mux4to1_package;
```
declaring a package containing the mux allows us to use the component form the `WORK` library
allows the component to be instantiated within the body of another design's architecture without declaring it in the header of the architecture

### Hierarchical code for a 16-to-1 MUX
![[Pasted image 20241009222646.png]]
```vhdl
library ieee;
use ieee.std_logic_1164.all;

use work.mux4to1_package.all;

entity mux16to1 is
	port (w : in std_logic_vector(0 to 15);
			s : in std_logic_vector (3 downto 0);
			f : out std_logic);
end mux16to1;

architecture Structure of mux16to1 is
	signal m: std_logic_vector (0 to 3);
begin
	Mux1: mux4to1 port map (w(0), w(1), w(2), w(3), s (1 downto 0), m(0));
	Mux2: mux4to1 port map (w(4), w(5), w(6), w(7), s (1 downto 0), m(1));
	Mux3: mux4to1 port map (w(8), w(9), w(10), w(11), s(1 downto 0), m(2));
	Mux4: mux4to1 port map (w(12), w(13), w(14), w(15), s(1 downto 0), m(3));
	Mux5: mux4to1 port map (m(0), m(1), m(2), m(3), s(1 downto 0), f);
end Structure;
```
#### 16-to-1 MUX using a generate statement
```vhdl
library ieee;
use ieee.std_logic_1164.all;
use work.mux4to1_package.all;

entity mux16to1 is
	port (w : std_logic_vector(0 to 15);
			s : std_logic_vector(3 downto 0);
			f : out std_logic);
end mux16to1;

architecture Structure of mux16to1 is
	signal m: std_logic_vector(0 to 3);
begin
	G1: for i in 0 to 3 generate
		Muxes: mux4to1 port map (
			w(4*i), w(4*i + 1), w(4*i + 2), w(4*i + 3), s(1 downto 0), m(i));
	end generate;
	Mux5: mux4to1 port map (m(0), m(1), m(2), m(3), s(3 downto 2), f);
end Structure;
```

### VHDL code for 2-to-4 binary encoder
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity dec2to4 is
	port (w : in std_logic_vector(1 downto 0);
			En : in std_logic;
			y : out std_logic_vector(0 to 3));
end dec2to4;

architecture Behavior of dec2to4 is
	signal Enw: std_logic_vector(2 downto 0);
begin
	Enw <= En & w; -- concatentation
	with Enw select
		y <= "1000" when "100",
			"0100" when "101",
			"0010" when "110",
			"0001" when "111",
			"0000" when others;
end Behavior;
```
![[Pasted image 20241009223702.png]]

### Hierarchical code for 4-to-16 binary decoder
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity dec4to16 is
	port ( w : in std_logic_vecotr(3 downto 0);
			En : in std_logic;
			y : out std_logic_vector(0 to 15));
end dec4to16;

architecture Structure of dec4to16 is
	component dec2to4
		port ( w : in std_logic_vector(1 downto 0);
				En : in std_logic;
				y : out std_logic_vector(0 to 3));
	end component;
	signal m : std_logic_vector(0 to 3);
begin
	g1 : for i in 0 to 3 generate
		Dec_right: dec2to4 port map ( w(1 downto 0), m(i), y(4 * i to 4 * i + 3));
		g2: if i=3 generate
			Dec_left: dec2to4 port map (w (3 downto 0), En, m);
		end generate;
	end generate;
end structure;
```

### Specification of 2-to-1 MUX using a conditional signal assignment
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity mux2to1 is
	port (w0, w1 ,s : in std_logic;
			f : out std_logic);
end mux2to1;

architecture Behavior of mux2to1 is
begin
	f <= w0 when s = '0' else w1;
end Behavior;
```

### VHDL code for priority encoder
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity priority is
	port (w : in std_logic_vector(3 downto 0);
			y : out std_logic_vector(1 downto 0);
			z : out std_logic);
end priority;

architecture Behavior of priority is
begin
	y <= '11' when w(3) = '1' else -- conditional signal assignments evaluate conditions in listed order
		'10' when w(2) = '1' else
		'01' when w(1) = '1' else -- arbitrary, unreleated conditions are possible
		'00';
	z <= '0' when w = "0000" else '1';
```
![[Pasted image 20241010205704.png]]

### priority encoder specified using if-then-else
```vhdl
library ieee;
use ieee.std_logic_1164.all;
entity priority is
	port (
	w : in std_logic_vector(3 downto 0);
	y : out std_logic_vector(1 downto 0);
	z : out std_logic);
end priority;

architecture Behavior of priority is
begin
	process (w)
	begin
		if w(3) = '1' then
			y <= "11";
		elsif w(2) = '1' then
			y <= "10";
		elsif w(1) = '1' then
			y <= "01";
		else
			y <= "00";
		end if;
	end process;
	z <= '0' when w = "0000" else '1';
end Behavior;
```

### 2-to-1 MUX specified using an if-then-else statement
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity mux2to1 is
	port (w0, w1, s: in std_logic; 
			f : out std_logic);
end mux2to1;

architecture Behavior of mux2to1 is
begin
	process(w0, w1, s) -- sensitivity list
	begin
		if s = '0' then
			f <= w0;
		else
			f <= w1;
		end if;
	end process; -- the assignment to f is not commited until the current invocation of the process ends
end Behavior;
```
Processes are typically used to describe complex behaviours
processes contains sequential statements i.e. statements within the process are evaluated one after another
	when there are consecutive assignments to the one signal, only the last assignment made is the one that is committed when the process exits
process is triggered when a signal in its sensitivity list have change in value

### Alternate code to 2-to-1 MUX
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity mux2to1 is
	port (w0, w1, s : in std_logic;
			f : out std_logic);
end mux2to1;

architecture Behavior of mux2to1 is
begin
	process (w0, w1, s)
	begin
		f <= w0;
		if s = '1' then -- works because of sequentual evaluation
			f <= w1;
		end if;
	end process;
end Behavior;
```

### One-bit equality comparator
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity compare1 is
	port (A, B : in std_logic;
			AeqB : out std_logic);
end compare1;

architecture Behavior of compare1 is
begin
	process(A, B)
	begin
		AeqB <= '0';
		if A = B then
			AeqB <= '1';
		end if;
	end process;
end Behavior;
```

### 2-to-4 binary decoder (CASE)
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity dec2to4 is
	port (w : in std_logic_vector(1 downto 0);
			En : in std_logic;
			y : out std_logic_vector(3 downto 0));
end dec2to4;

architecture Behavior of dec2to4 is
begin
	process (w, En)
	begin
		if En = '1' then
			case w is
				when "00" => y <= "1000";
				when "01" => y <= "0100";
				when "10" => y <= "0010";
				when others => y <= "0001";
			end case;
		else
			y <= "0000";
		end if;
	end process;
end Behavior;
```

### BCD-to-7 -seg decoder (code)
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity seg7 is
	port (bcd : in std_logic_vector(3 downto 0);
			leds : out std_logic_vector(1 to 7));
end seg7;

architecture Behavior of seg7 is
begin
	process(bcd
	begin
		case bcd is
			when "0000" => leds <= "0000001";
			when "0001" => leds <= "1001111";
			when "0010" => leds <= "0010010";
			when "0011" => leds <= "0000110";
			when "0100" => leds <= "1001100";
			when "0101" => leds <= "0100100";
			when "0110" => leds <= "0100000";
			when "0111" => leds <= "0001111";
			when "1000" => leds <= "0000000";
			when "1001" => leds <= "0001100";
			when others => leds <= "_______";
		end case;
	end process;
end Behavior;
```

## VHDL operators used in synthesis
![[Pasted image 20241010211413.png]]
precedence in the table to the left is from top to bottom between categories
operators within the same category have the same precedence and are therefore evaluated from left o right
often good to parenthesise expressions to explicitly confirm evaluation order
note also that for good synthesis tools `s <= a + b + c + d;` results in 3 3 sequential additions, whereas `s <= (a + b) + (c + d);` performs two sub-additions in parallel and then one final addition for 2/3 the delay

## Multiplexer-based logic synthesis using Shannon's expansion theorem
Besides using simple inputs, it is possible to connect more complex circuits as inputs to a MUX, allowing *fns to be synthesised* using a combination of MUXes and other logic gates

### 3-input majority fn implemented using 2-to-1 MUX
![[Pasted image 20241010212320.png]]

### Shannon's expansion theorem
MUX implementations of logic fns require that a given fn be decomposed with respect to the variables that are used as the select inputs
any boolean function $f(w_1,...w_n)$ can be written in the form $f(w_1,w_2,...,w_n)$  = $\overline{w_1}*f(0, w_2, ...,w_n) + w_1*f(1,w_2,...,w_n)$ (the expansion can be done w.r.t any of the n variables)
![[Pasted image 20241010212531.png]]

### Cofactors of f
in Shannon's expansion, the term $f(0, w_2, ...,w_n)$ is called the *cofactor of f*  with respect to $\overline{w_1}$, denoted by $f_\overline{w_1}$
similarly, the term $f(1, w_2, ...,w_n)$ is called the cofactor of f with respect to $w_1$ , written $f_{w_1}$, hence we can write $f = \overline{w_1}f_{\overline{w_1}} + w_1f_{w_1}$
in general, if the expansion is done with respect to variable $w_i$, then $f_{w_i}$ denotes $f(w_1,...,w_{i-1}, 1, w_{i+1},...w_n)$ and $f(w_1,...,w_n)$ = $\overline{w_i}f_{\overline{w_i}} + w_if_{w_i}$, whereby the complexity of the expression may vary, depending on which variable $w_i$ is used

### Complexity of cofactors
for the function $f = \overline{w_1}w_3 + w_2 \overline{w_3}$ , with canonical SOP form: $f = \overline{w_1w_2}w_3 + \overline{w_1}w_2w_3 + \overline{w_1}w_2w_3 + w_1w_2\overline{w_3}$, decomposition using $w_1$ gives
$f = \overline{w_1}f_{\overline{w_1}} + w_1f_{w_1}$
$= \overline{w_1}(\overline{w_2}w_3 + w_2w_3 + w_2 \overline{w_3}) + w_1(w_2\overline{w_3})$
$= \overline{w_1}(w_3 + w_2) + w_1(w_2\overline{w_3})$
using $w_2$ instead of $w_1$ produces
$f = \overline{w_2}f_{\overline{w_2}} + w_2f_{w_2}$
$= \overline{w_2}(\overline{w_1}w_3) + w_2(\overline{w_1} + \overline{w_3})$
finally, using $w_3$ gives
$f = \overline{w_3}f_{\overline{w_3}} + w_3f_{w_3}$
$= \overline{w_3}(w_2) + w_3(\overline{w_1})$

### Shannon's expansion with more than one variable
Shannon's expansion can also be carried out with respected to more than one variable
for example, expanding a function with respect to variables $w_1$ and $w_2$ gives
![[Pasted image 20241010213526.png]]
which is in a form that can be implemented with to 4-to-1 MUX using $w_1$ and $w_2$ as the select inputs
**When an expansion is carried out with respected to all $n$ variables, a canonical SOP form results**
	Hence, an n-variable function is implemented by an n-input LUT by programming it with the function's truth table


