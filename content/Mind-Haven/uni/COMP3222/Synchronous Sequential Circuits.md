
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

## General form of a sequential circuit 
![[Pasted image 20241009154658.png]]
**primary input**: W
**primary output**: Z

Output of the FFs are the *state*, Q, of the circuit.
	To simplify design and analysis, the state should only change once per per clock cycle, FFs should therefore be edge-triggered
	change in state depend upon both the current input and the present (current) outputs of the FFs, Q
outputs of the circuit depend upon the current state, and may also depend upon the current inputs, (this is not required)

when the output `Z` only depend upon the current state `Q`, the circuit is said to be *Moore* type
alternatively, when the outputs Z depend upon the current state, `Q` and the inputs `W`,  the circuit is said to be *Mealy* type
	mealy circuits may require less states than Moore circuits for similar behaviour and are responsive to changes in the inputs
because the functional behaviour of the circuit can be represented using a finite number of states, sequential circuits are also called **Finite state machines**

## Basic design steps
consider the design of a simple circuit meeting the the following specifications:
	1. The circuits has one input immediately preceding clock cycles/periods the input `w` was equal to 1. otherwise, `z = 0`
	2. `z = 1` if during two immediately preceding clock cycles/periods the input `w` was equal to 1. otherwise, `z = 0`
	3. all changes in the circuit occur on the positive edge of a clock signal

the circuit detects two or more consecutive 1s. circuits that detect the occurrence of a particular input pattern are referred to as *sequence detectors*

### Input/Output behaviour of the circuit
output doesn't only depend on the present value of `w`, seen if we consider the desired input/output behaviour of the circuit
![[Pasted image 20241102113936.png]]

different outputs during $t_4$ and $t_8$ or $t_2$ and $t_5$ illustrate that the output must be determined by some state of the circuit other than by the current input value
the first step in designing a finite state machine is to determine how many states are needed and which 'transitions' are possible from one state to another

## Behaviour of state machine
there is no set procedure for determining number of states
for the example:
	- select a starting state that the circuit should enter when first powered on or when a *reset* signal is applied; *State A*
	- while w = 0, the circuit need not do anything, and so each active clock edge results in the circuit remaining in *State A*
	-  while w = 1, the machine should recognise this and move to a new state *B*. the transition takes place upon the occurrence of the next active clock edge after w = 1
	- in both states A and B, the output z = 0 as the machine has not yet seen w = 1 for 2 consecutive clock cycles
	- when in state B, when w = 0 when the next active clock edge occurs, the circuit should return to state A. however, if w = 1 is seen in state B, the circuit should change to a third state called C and generate as output z = 1
	- the circuit should remain in state C and output z = 1 as long as w = 1, then w  becomes 0, the machine should return state A
as all possible values for *w* have been considered in all possible states, we can conclude that 3 states are enough in this case

## State diagram
the behaviour of a sequential circuit can be described in several ways
the conceptually easiest is to use a pictorial representation in the form of *state diagram*, which is directed graph that depicts states of the circuit as nodes and transitions between states as directed edges.
the state diagram corresponding to our specification to our specification is an shown to the right
it should be noted that any labels instead of letters could be used for the states, and that the transition that is taken is the one associated with the input present when the active clock edge arrives
![[Pasted image 20241102131348.png]]
while a state diagram is easy to understand, for implementation, it is more convenient to translate the diagram into tabular form
a *state table* indicates all transitions from each *present* state to the *next state* for different input signal values
	note that for our (moore machine) design, the output is listed with respect to the present state only
![[Pasted image 20241102131554.png]]

## State assignment
the state table of the previous slide defines 3 states in terms of letters *A, B and C*
when implemented in a logic circuit, each state is represented by a particular valuation of *state variables*
each state variable is implemented by a ff
	the example with three states to represent, at least two state variables are required


## More sequential with two state flip-flops
![[Pasted image 20241102131743.png]]
upper case *Y_1* and *Y_2* - *next-state variables*
lower case *y_1* and *y_2* - *present-state variables*
next, we need to determine what type of flip-flop to use and design the combinational circuit blocks

## State-assigned table
producing a truth table that defines the function of the combinational circuits
this requires us to assign a specific valuation of the state variables to each state, resulting in so-called *state-assigned table* for the circuit
![[Pasted image 20241102132031.png]]

### Derivation of logic expressions
depends on ff type used for the implementation
![[derivation-of-logic-expression-l06.excalidraw|1000]]

### Final implementation
![[Pasted image 20241102132445.png]]

### Timing diagram of the circuit
![[Pasted image 20241102132504.png]]

## SUMMARY: FSM design steps
1. Obtain the specification of the desired circuit
2. Derive the states for the machine and create *state diagram*
	given a starting state, consider the behaviour in response to all possible inputs and identify new states as required. repeat for all added states until all possible inputs have been considered for all states. When finished, the state diagram shows all states and the conditions under which the circuit moves from one state to another
3. *Create state table* from  the state diagram
4. Determine the number of state variables required to represent all the states and *perform a state assignment*
5. given the type of flip-flops to be used, derive the next-state logic expressions to control the FF inputs and to produce the desired output
6. implement the circuit

## State-assignment problem
### Improved state assignment for example 1
![[Pasted image 20241102132858.png]]
choosing  C = 11 rather than C = 10, as we previously did, and choosing to implement the circuit using D-type ffs results in the next-state and output expressions
$Y_1 = D_1 = w, Y_2 = D_2 = wy_1, z = y_2$
### Circuit for improve state assignment
![[Pasted image 20241102133022.png]]

## Mealy state machines
Moore state machines - output is purely a function of the present state of the circuit
Mealy state machines - output is also a function of the circuit's current inputs
Hence, provides additional flexibility and responsiveness in the design of sequential circuits


For example 1, output was required to become 1 in the cycle after two consecutive 1s on the input had been detected
instead, suppose that the output should become 1 in the clock cycle during which a second or further consecutive 1 is detected
the input/output sequence should then look as follows:
![[Pasted image 20241102134348.png]]

**State diagram for revised example 1**
![[Pasted image 20241102134457.png]]
note that now only two states are needed because we allow the output value to depend upon the present value of the input as well as the present state of the machine

**State table for the revised example 1**
![[Pasted image 20241102134713.png]]
note that for a Mealy machine, the output value depends upon the present state as well as the input value

**State-assigned table for revised example 1**
![[Pasted image 20241102134804.png]]
assuming D-type ffs are selected to be used in the implementation of the machine, the next-state and output expression are:
$Y = D = w, z = wy$

**Implementation of revised example 1**
![[Pasted image 20241102134947.png]]

## Using CAD tools to design FSMs
one could use structural VHDL to input a manually derived design before simulation and implementation
but CAD tools offer a better alternative, namely, to enter the state diagram and to derive the design automatically
	graphical tools exist for this purpose
	behavioural HDL is used to capture the diagram

### VHDL code for Moore-type FSM of example 1
there is no standard way of describing FSMs
using VHDL syntax, there are a few different ways of describing FSMs
	note user-defined signal type (an emumerated type) (line 8)
	the compiler chooses the number of state ffs and the state assignment
	changes in state occur on positive clock edges
```VHDL
library ieee;
use ieee.std_logic_1164.all;

entity simple is
	port (Clock, Resetn, w : in std_logic;
			z : out std_logic);
end simple;

architecture behavior of simple is
	type State_type is (A,B,C)
	signal y : State_type
	begin
		process(Resetn, Clock)
		begin
			if Resetn = '0' then
				y <= A;
			elsif (Clock'event and clock = '1') then
				case y is
					when A => -- each state needs a WHEN
						if w = '0' then -- input determines
							y <= A;  -- next state
						else
							y <= B;
						end if;
					when B => -- when in state B
						if w = '0' then
							y <= A;
						else
							y <= C;
						end if;
					when C =>
						if w = '0' then
							y <= A;
						else
							y <= C;
						end if;
				end case;
			end if;
		end process;
		z <= '1' when y = C else '0'; -- output depends on state
	end behavior;
```

### Simulation results for the implemented circuit
![[Pasted image 20241102135706.png]]
for this simple FSM it is easy to check its correctness
for more complex FSMs, there may be a large number of possible states and inputs, so the designed needs to plan sequences of input patterns and corresponding acceptance tests carefully

## User-defined state assignment
it is possible for the user to manually specify a desired state assignment, but there is no standardised approach for doing so
in Vivado, this is done as following
```vhdl
architecture behavior of simple is
	type State_TYPE is (A, B, C);
	signal y_present, y_next : State_type;
	attribute enum_encoding : string
	attribute enum_encoding of State_type : type is "00 01 11";
	attribute fsm_encoding : string
	attribute fsm_encoding of y_present : signal is "user_encoding";
	attribute fsm_encoding of y_next : signal is "user_encoding";
begin
```
but node that this should not normally be necessary
	allow the tool to determine the best assignment

### Using constants for manual state assignments - works with all VHDL compilers
note the need for a `WHEN OTHERS` clause in the `next_state` logic as there is no enumerated `state_type`; `y_present` is simply a `STD_LOGIC_VECTOR`
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity simple is
	port (Clock, Resetn, w : in std_logic;
			z : out std_logic);
end simple;

architecture behavior of simple is
	signal y_present, y_next : STD_LOGIC_VECTOR(1 DOWNTO 0)
	constant a : std_logic_vector(1 downto 0) == "00";
	constant b : std_logic_vector(1 downto 0) == "01";
	constant c : std_logic_vector(1 downto 0) == "11"
	begin
		process(w, y_present)
		begin
			case y_present is
				when A =>
					if w = '0' then y_next <= A;
					else y_next <= B;
					end if;
				when B =>
					if w = '0' then y_next <= A;
					else y_next <= C;
					end if;
				when C =>
					if w = '0' then y_next <= A;
					else y_next <= C;
					end if;
				when others =>
					y_next <= A;
			end case;
		end process;
		process(Clock, Resetn)
		begin
			if Resetn = '0' then
				y_present <= A;
			elsif (Clock'event and Clock = '1') then
				y_present <= y_next;
			end if;
		end process;
		z <= '1' when y_present = C else '0';
	end behavior;
```


## Mealy-type FSM for Example 
### VHDL code
![[Pasted image 20241102142225.png]]
![[Pasted image 20241102142231.png]]
note use of second process to determine output independently determine output independently of the state tranisition logic
it is also common to separate the next-state logic from the state transition logic
![[Pasted image 20241102142323.png]]
**Potential problem with asynchronous inputs to the Mealy Machine**
here change in `w` occur after negative clock edges
`z` should not be asserted until after `w` is asserted for 1 clock period
	if `z` input to another circuit that is not controlled by the same clock, we could observe downstream errors
	on the other hand, a downstream circuit controlled by the same clock should ignore the erroneous outputs

## Example 2: designing control circuit for a bus-based register swap
consider the control required to swap the contents of R1 and R2 via a bus using R3 for temporary storage
![[Pasted image 20241102143014.png]]

### Details for connecting register to a bus
consider two 2-bit registers
	3-state buffers used to avoid 'typing' outputs together
![[Pasted image 20241102143438.png]]
### Control circuit design
consider the control required to swap the contents of R1 and R2 using R3 for temporary storage
	**What register transfers** are required to effect the swap
	**What control signals** need to be asserted for each transfer
	**When & how** should the control signals be *sequenced*
![[Pasted image 20241102143702.png]]
### Signals needed by control circuit
![[Pasted image 20241102143722.png]]

### Moore state diagram for ex. 2:
![[Pasted image 20241102143738.png]]
**State table**
![[Pasted image 20241102143756.png]]

**State-assigned table, next-state and output expressions for example 2 using D-type ff and sequential state assignment**
![[Pasted image 20241102143829.png]]

### Final implementation of Ex 2
![[Pasted image 20241102143942.png]]

#### Improved state assignment for Ex 2 using Gray state assignment
swapping the assignment for states C and D
![[Pasted image 20241102144005.png]]

**One-hot state assignment for Ex 2**
![[Pasted image 20241102144020.png]]
treating the remaining 12 valuations of the state variables as don't cares results in $Y_1 = \overline{w}y_1 + y_4, Y_2 = wy_2, Y_3 = y_2$ and $Y_4 = y_3$
the output expressions are just the outputs of the ffs: $R2_{out} = R3_{in} = y_2, R1_{out} = R2_{in} = y_3, R3_{out} = Done = y_4$
these expressions are simpler than previously seen, but 4 ffs are needed
simpler expressions, as often result from one-hot encodings, may lead to faster circuits

### Mealy-type FSM for swapping two registers
![[Pasted image 20241102144302.png]]
while the Mealy implementation only requires 3 states, this does not necessarily imply a simpler circuit since we still need at least 2 FFs
the most important difference with the Moore version is the timing of the outputs signals, which are generated one clock cycle sooner
note also that the entire swap only takes 3 clock cycles for the mealy-type FSM, whereas it takes 4 clock cycles to complete for the Moore machine

### Complete design example - serial addition
we have looked at several addition schemas that added two n-bit numbers in parallel (e.g. ripple-carry, carry-lookahead)
in these schemes, the speed of the adder is important, but fast adders are more complex and thus more expensive
if speed is not important, then a more cost-effective option is to use a serial adder in which bits are added in pair at a time
![[Pasted image 20241102144613.png]]

## Serial addition
let $A = a_{n-1}a_{n-2}...a_0$ and $B = b_{n-1}b_{n-2}...b_0$ be two unsigned numbers that are to be added to produce $S = s_{n-1}s_{n-2}...s_0$
our task is to design a circuit that will perform the serial addition, dealing with the pair of bits each clock cycle
having loaded a pair of numbers in parallel, the process starts by adding $a_0$ and $b_0$ and shifting the result, $s_0$, into the sum register. in the next clock cycle, bits $a_1$ and $b_1$ are added, *including possible carry from bit-position 0*
assume we are to use positive edge-triggered D-type ffs in the design

### State diagram for the serial adder FSM
an FSM is needed since the sum bit produced differs depending upon the carry produced in the previous cycle
we therefore need two states depending upon the value of the carry-in bit
![[Pasted image 20241102145410.png]]
### State table for the serial adder FSM
the state table is readily obtained from the state diagram
![[Pasted image 20241102145441.png]]

### State-assigned table for the serial adder FSM
![[Pasted image 20241102145458.png]]
a simple state assignment leads to the following next-state and output equations $Y = ab + ay + by, s = a \oplus b \oplus y$
these are the same as for a full-adder with carry-in y, carry-out Y, and sum s

### Circuit for the serial adder FSM
![[Pasted image 20241102145724.png]]

### State diagram for a Moore-type serial adder FSM
need separate states, i.e. two states, for each output we found in the state diagram of the Mealy machine
![[Pasted image 20241102145825.png]]
![[Pasted image 20241102145835.png]]
![[Pasted image 20241102145848.png]]
the next-state and output equations are: $Y_1 = a \oplus b \oplus y_2, Y_2 = ab + ay_2 + by_2, s = y_1$
the expressions for Y1 and Y2 correspond to the sum and carry-out expressions in the full-adder circuit
![[Pasted image 20241102145946.png]]
referring back to the Mealy circuit, the output s is now passed through the ff and thus delayed by one clock cycle

**How do we build the serial adder ? How do we control its operation ?**
![[Pasted image 20241102173047.png]]

### Code for n-bit left-to-right shift register with an enable input
```vhdl
library ieee;
use ieee.std_logic_1164.all;
-- left-to-right shift register with parallel load and enable
entity shiftme is
	generic(N : integer := 4)
	port (R : in std_logic_vector(n - 1 downto 0);
			L, E, w: in std_logic; -- (L)oad, (E)nable & serial input (w)
			Clock : in std_logic;
			Q : BUFFER std_logic_vector(n-1 downto 0));
end shiftme;

architecture behavior of shiftme is
begin
	process
	begin
		wait until Clock'event and Clock = '1';
		if E = '1' then
			if L = '1' then
				Q <= R;
			else
				Genbits: for i in 0 to n-2 loop
					Q(i) <= Q(i + 1)
				end loop;
				Q(n-1) <= w;
			end if;
		end if;
	end process;
end behavior;
```

### Code for serial adder
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity serial is
	generic (length : integer := 8);
	port ( Clock : in std_logic;
			Reset : in std_logic;
			A, B : in std_logic_vector(length-1 downto 0);
			Sum : buffer std_logic_vector(lengt-1 downto 0));
end serial;

architecture behavior of serial is
	component shiftme -- include the parallel load shift register as a component
	port ( R : in std_logic_vector(n-1 downto 0);
			L, E, w : in std_logic; -- load, enable, shift-in
			Clock : in std_logic;
			Q : buffer std_logic_vector(n-1 downto 0);
	end component;

signal QA, QB, Null in : std_logic_vector(lenght-1 downto 0);
signal s, Low, High, Run : std_logic;
signal Count : integer range 0 to length;
type State_type is (G, H); -- our Mealy machine
signal y : State_type

begin
	Low <= '0'; High <= '1';
	ShiftA : shiftme generic map (n => length)
		port map(A, Reset, High, Low, Clock, QA);
	ShiftB : shiftme generic map(n => length)
		port map(B, Reset, High, Low, Clock, QB);
	AdderFSM: process (reset Clock)
	begin
		if Reset = '1' then
			y <= G;
		elsif Clock'event and Clock = '1' then
			case y is
				when G =>
					if QA(0) = '1' and QB(0) = '1' then
						y <= H;
					else y <= G;
					end if;
				when H =>
					if QA(0) = '0' and QB(0) = '0' then
						y <= G;
					else y <= H;
					end if;
			end case;
		end if;
	end process AdderFSM

	with y select
		s <= QA(0) XOR QB(0) WHEN G, NOT (QA(0) XOR QB(0)) WHEN H;
	Null_in <= (OTHERS => '0');
	ShiftSum shiftme generic map (n => length)
					port map (Null_in, Reset, Run, s, Clock, Sum)

	Stop: process
	begin
		wait until (Clock'event and Clock = '1');
		if Reset = '1' then
			Count <= length;
		elsif Run = '1' then
			Count <= Count - 1;
		end if;
	end process;
	Run <= '0' when Count = 0 else '1'; -- stop counter and ShiftSum
end behavior;
```

### Synthesised serial adder
![[Pasted image 20241102174427.png]]

## State minimisation
minimising the number of states:
	possibly fewer ff needed to represent states
	complexity of the FSM's combinational logic may be reduced
to reduce the number of states in a state diagram, some states must be equivalent to others in terms of their contribution to the overall behaviour of the FSM
**Definition:** two states $S_i$ and $S_j$ are said to be *equivalent* if and only if for every possible input sequence the same output sequence will be produced regardless of whether $S_i$ or $S_j$ is the initial state

### State minimisation procedure
we exploit the idea that is easy to show that some states are definitely not equivalent and partition the set of states into equivalent sets of states on that basis:
	first, partition the states into different sets on the basis of the different output values they produce
	next, consider the members of each set and determine whether or not they have next states that belong on the same sets, i.e. refine the partitioning until all states within each set have the same next state set for each possible input value
	when the partitioning cannot be further refined, replace each set with a single state - a minimal number of states has been found

### State minimisation example
![[Pasted image 20241102174957.png]]
initial partitioning $P_1 = (ABCDEFG)$
the different output values lead to a partitioning into two sets $P_2 = (ABD)(CEFG)$
the first set has a next state in the first set when $w = 0$ and in the second set when $w = 1$
however, state F differs from the other members of the second set in that it has a next state in the first set when $w = 1$
we therefore have $P_3 = (ABD)(CEG)(F)
checking all successor states for each set under each input we note no further partitioning is necessary, thus 4 states suffice for this example and we can label them $A = (AD), B = (B), C = (CEG) and F = (F)$
thus the following minimised state table can be derived
this functionality equivalent FSM only requires two state ff
![[Pasted image 20241102175556.png]]

#### Minimised state table for example
thus the following minimised state table can be derived, this functionality equivalent FSM only requires two state ff
![[Pasted image 20241108182534.png]]

### Analysis of synchronous sequential circuits
designers must be able to analyse the behaviour of existing circuits --  easier than synthesising
to analyse circuit - simply reverse the steps of the synthesis
	1. ff output represent the present state variables
	2. their inputs determine the next state the circuit will enter
	3. from this information, we can construct the state-assigned table
	4. which leads to the state table, and ultimately, the state diagram

#### Analysis example
What is the function of this circuit
![[Pasted image 20241108182952.png]]![[Pasted image 20241108183024.png]]
#### Example using [JK]() ff
![[Pasted image 20241108193619.png]] ![[Pasted image 20241108193626.png]] 

![[Pasted image 20241108193651.png]] ![[Pasted image 20241108193657.png]] ![[Pasted image 20241108193703.png]] ![[Pasted image 20241108193720.png]]
![[Pasted image 20241108193730.png]]

