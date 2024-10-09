
| Objective                                                                             | Check |
| ------------------------------------------------------------------------------------- | ----- |
| Learn design techniques for circuits that use flip-flops                              | []    |
| Understand the concept of states and their implementation with flip-flops             | []    |
| Learn about the synchronous control of circuits using a clock signal                  | []    |
| Learn how to design synchronous sequential circuits                                   | []    |
| Learn how to specify synchronous sequential circuits using VHDL                       | []    |
| Understand the techniques CAD tools use to synthesise synchronous sequential circuits | []    |

Consider a general class of circuits, known as *sequential circuits*, output depending on *past inputs and state*,  as well as *present input values*
Clock signal is commonly used to control the operation of a sequential circuit; known as *synchronous sequential circuits* (SSC)
SSC designed using combinational logic together with one of more flip-flops (ff).

## General of of sequential circuit 
![[Pasted image 20241009154658.png]]
**primary input**: W
**primary output**: Z

Output of the FFs are the *state*, Q, of the circuit.
	To simplify design and analysis, the state should only change once per per clock cycle: 

