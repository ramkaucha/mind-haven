
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

