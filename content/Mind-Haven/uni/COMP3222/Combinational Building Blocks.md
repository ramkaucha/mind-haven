
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

**note: slide 29**




