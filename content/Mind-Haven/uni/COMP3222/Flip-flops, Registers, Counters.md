
### Why we need circuits with memory
Consider a alarm system that is required to remain activated when triggered, even when the cause for triggering has ceased
![[Pasted image 20241001224858.png]]
Here, the *Reset* signal is intended to provide a means of switching off the alarm

## How to create a memory element
Using feedback to 'trap' a value
Consider a simple cyclic circuit comprising two inverters
![[Pasted image 20241001224952.png]]
The circuit has two stable states
But there is no way of changing from one state to the other

### Memory element using NOR gates
![[Pasted image 20241001225032.png]]

| A   | B   | A NOR B |
| --- | --- | ------- |
| 0   | 0   | 1       |
| 0   | 1   | 0       |
| 1   | 0   | 0       |
| 1   | 1   | 0       |
When both Set and Reset are 0, the state, Q, is preserved; Set = 1, Reset = 0 => Q = 1, Set = D, Reset 1 => Q  = 0, known as a *latch*

### Basic latch using cross-coupled NOR gates
![[Pasted image 20241002102446.png]]


### Gated SR latch using NAND gates
more usual configuration as it uses less transistors
	has exactly the same characteristic table
	note that S and R inputs are flipped about wrt the outputs
![[Pasted image 20241002102548.png]]

### Gated D latch
eliminates the illegal input combination S = R = 1
useful for storing a data bit
![[Pasted image 20241002102619.png]]

### Negative edge-trimmed (Master-slave)
Latches are triggered by the level of the control signal, flip-flops are triggered on control signal transitions
![[Pasted image 20241002102723.png]]

### Positive-edge-triggered D flip-flop
![[Pasted image 20241002102744.png]]

### Comparison of level-sensitive and edge-triggered D-type storage elements
![[Pasted image 20241002102821.png]]

### Code for a gated D latch
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;

ENTITY latch IS
	PORT ( D, CLK: IN         STD_LOGIC;         
			Q       :OUT    STD_LOGIC);
END latch;

ARCHITECTURE  Behaviour  OF latch IS
BEGIN
	PROCESS(D, CLK)
	BEGIN
		IF CLK = '1' THEN
			Q <= D;
		END IF;
	END PROCESS;
END Behaviour;
```

Note - the `PROCESS` describing a latch, while exploiting implicit memory. complies with the *COMBINATIONAL* design rule that all signals that can affect the output are listed in the sensitivity list
![[Pasted image 20241002141308.png]]

### Code for positive edge-triggered D flip-flop
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;

ENTITY flipflop IS
	PORT ( D, CLK: IN         STD_LOGIC;         
			Q       :OUT    STD_LOGIC);
END flipflop;

ARCHITECTURE  Behaviour  OF flopflop IS
BEGIN
	PROCESS(CLK)
	BEGIN
		IF CLK`event AND CLK = '1' THEN
			Q <= D;
		END IF;
	END PROCESS;
END Behaviour;
```
*Note*: Synchronous processes only list the clock signal in the sensitivity list:
ii) All assignment statements within a synchronous process should be guarded by a (`CLK''event AND CLK = ' '`) condition;
iii) Each signal on the LHS of an assignment statement guarded by a (`CLK'event AND CLK = ' '`) condition is the output of a flip-flop
![[Pasted image 20241002143027.png]]

### Equivalent code using a WAIT UNTIL statement
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;

ENTITY flipflop IS
	PORT ( D, Clock: IN         STD_LOGIC;         
			Q       :OUT    STD_LOGIC);
END flipflop;

ARCHITECTURE  Behaviour  OF flopflop IS
BEGIN
	PROCESS(CLK)
	BEGIN
		WAIT UNTIL Clock'EVENT AND Clock = '1';
		Q <= D;
	END PROCESS;
END Behaviour;
```

## Master-slave D flip-flop with asynchronous Clear and Preset
A design may call for a preset value on a FF
Active low *Preset*' and *Clear*' inputs allow the flop-flop to be set to a given value asynchronously (independently of the Clock) - only one of them should be pulled low at a time
![[Pasted image 20241002143428.png]]
When the clock is low, we need to override the master output to ensure the preset/clear state is preserved after the ==finish==
==How long does the FF stay in the Clear or Preset state==

### Positive-edge-triggered D flip-flop with synchronous Clear and Preset
![[Pasted image 20241002144144.png]]
Synchronous clear and preset is best done by gating the D input
![[Pasted image 20241002144206.png]]
 clk'
### D flip-flop with asynchronous reset
```vhdl
library ieee;
use ieee.std_logic_1164all;

entity flipflop is 
	port (D, Resetn, CLK: IN     STD_LOGIC;
		  Q                   : OUT  STD_LOGIC);
end flipflop;

architecture Behaviour of flopflop is
begin
	process(Resetn, CLK)
	begin
		if Resetn = '0' then
			Q <= '0';
		elsif CLK'event and clk = '1' then
			Q <= D;
		end if;
	end process;
end Behaviour;
```

Note: For synchronous process with an asynchronous reset/set, both the CLK and the reset/set, both the CLK and the reset/set signal must be in the sensitivity list
Only assign a constant, e.g. '0'/1, to the FF output within the reset/set condition

### D flip-flop with synchronous reset
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity flipflop is
	port (D, Resetn, Clock : in std_logic;
			Q : out std_logic);
end flipflop;

architecture Behavior of flipflop is
begin
	process
	begin
		wait until Clock'Event and Clock = '1';
		if ResetN = '0' then
			Q <= '0';
		else
			Q <= D;
		end if;
	end process;
end Behavior;
```


### Code for eight-bit register with asynchronous reset
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity reg8 is
	port ( D : in STD_LOGIC_VECTOR(7 downto 0);
			Resetn, Clock : in STD_LOGIC;
			Q : out STD_LOGIC_VECTOR(7 downto 0));
end reg8;

architecture Behavior of reg8 is
begin
	process (Resetn, Clock)
	begin
		if Resetn = '0' then
			Q <= '00000000';
		elsif Clock'event and Clock = '1' then
			Q <= D;
		end if;
	end process
end Behavior;
```

### Code for n-bit register with asynchronous reset
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity regn is
	generic (N : integer := 16); -- parameterised componenet with default value of 16 for the data width parameter N
	port (D : in std_logic_vector(N - 1 downto 0);
			Resetn, Clock: in std_logic;
			Q : out std_logic_vector(N - 1 downto 0));
end regn;

architcture Behavior of regn is
begin
	process
	begin
		if Resetn = '0' then
			Q <= (OTHERS => '0'); -- idion for setting all bits of a signal to 0s
		elsif Clock'event and Clock = '1' then
			Q <= D;
		end if;
	end process;
end Behavior;
```

### 8-bit register based on regn component
```vhdl
library ieee;
user ieee.std_logic_1164.all;

entity reg8 is
	port ( D : in std_logic_vector(7 downto 0);
			Resetn, Clock : in std_logic;
			Q : out std_logic_vector(7 downto 0));
end reg8;

architecture Structure of reg8 is;
	component regn is
		generic(N : integer := 16);
		port (D : in std_logic_vector(n-1 downto 0);
			Resetn, Clock : in std_logic;
			Q : out std_logic_vector(n-1 downto 0));
	end component;
begin
	reg8: regn
		generic map (n >= 8) -- GENERIC MAP is used to overwirte default parameter value when instantiating component
		port map (D, Resetn, Clk, Q);
end structure;
```


## Simple shift register
![[Pasted image 20241025201431.png]]

|     | In  | Q1  | Q2  | Q3  | Q4  | = Out |
| --- | --- | --- | --- | --- | --- | ----- |
| t0  | 1   | 0   | 0   | 0   | 0   |       |
| t1  | 0   | 1   | 0   | 0   | 0   |       |
| t2  | 1   | 0   | 1   | 0   | 0   |       |
| t3  | 1   | 1   | 0   | 1   | 0   |       |
| t4  | 1   | 1   | 1   | 0   | 1   |       |
| t5  | 0   | 1   | 1   | 1   | 0   |       |
| t6  | 0   | 0   | 1   | 1   | 1   |       |
| t7  | 0   | 0   | 0   | 1   | 1   |       |
## Parallel-access shift register
![[Pasted image 20241025201827.png]]

### Behavioural code for D flip-flop with a 2-to-1 multiplexer on the D input
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity muxdff is
	port (D0, D1, Sel, Clock : in std_logic;
			Q : out std_logic);
end muxdff;

architecture Behavior of muxdff is
begin
	process
	begin
		wait until Clock'event and clock = '1';
		if Sel = '0' then
			Q <= D0;
		else
			Q <= D1;
		end if;
	end process;
end behavior;

-- or
process(Clock)
begin
	if Clock' then
		if Sel ..
			etc.
		end if;
	end if;
```

### Hierarchical code for a four-bit shift register
*design hierarchies* are recursive structures comprised of components, or sub-circuits, whose architectures, at the leaf level are expressed in terms of their behaviours
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity shift4 is
	port ( P : in std_logic_vector(3 downto 0);
			ser, Id, Clock : in std_logic;
-- BUFFGER: data flows out of the entity, but the entity can also read the signal. the signal cannot be driven from outside the entity, so it cannot be used for data input
			Q : BUFFER std_logic_vector (3 downto 0)); -- allows for internal feedback
end shift4;

architecture Structure of shift4 is
	component muxxdf
		port (D0, D1, Sel, Clock : in std_logic;
			Q : out std_logic);
	end component;
begin
	Stage3: muxdff port map (ser, P(3), Id, Clock, Q(3));
	Stage2: muxdff port map (Q(3), P(2), Id, Clock, Q(2));
	Stage1: muxdff port map (Q(2), P(1), Id, Clock, Q(1));
	Stage0: muxdff port map (Q(1), P(0), Id, Clock, Q(0));
end Structure;
```

#### Alternative (behavioural) code for shift register
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity shift4 is
	port (  P : in std_logic_vector(3 downto 0);
			ser, Id, Clock : in std_logic;
			Q : buffger std_logic_vector(3 downto 0)); -- BUFFER mode allows Q to appear on both the left and righrt sides of signal assignments within the design
end shift4;

architecture Behavior of shift4 is
begin
	process
	begin
		wait until Clock'event and Clock = '1'; -- a wait until statement implies all signals assigned a value inside the process are implemented as the output of a flip-flop
		if Id = '1' then
			Q <= P;
		else
			Q(0) <= Q(1);
			Q(1) <= Q(2);
			Q(2) <= Q(3);
			Q(3) <= ser;
		end if;
	end process;
end Behavior;
```


## Flip-flop timing parameters
three important parameters that need to be considered in the design of sequential circuits
*propagation delay* $t_{cQ}$ , the time needed for the output of a FF to change after the triggering clock edge has occurred
*setup time* $t_{su}$ , the time interval the input needs to be stable for prior to the triggering clock edge, for it to be reliably read
*hold time* $t_h$ , time interval the input needs to be stable for after the triggering clock edge, for it to be reliably read

the magnitude of these parameters depend upon the design of the flip-flip, the process technology used to implement them, and the source voltage level

## Propagation delay
propagation delay is the time it takes for the new value to emerge from a flip-flop after the triggering edge
![[Pasted image 20241025203113.png]]

### $t_{cQ}$ for gated D latch
![[Pasted image 20241025203143.png]]
assume gates have a propagation delay of $\triangle ns$, and that signals need to be applied for at least $\triangle ns$ time to affect the output of a gate

here, $t_{cQ}$ = $2 \triangle$ for 0 -> 1 transitions, but $3 \triangle$ for 1 -> 0 transitions
![[Pasted image 20241025203639.png]]

## Setup and hold times
the designer of the circuit that generates the D signal must ensure setup and hold times are satisfied
together, they define a window of time around the triggering clock edge during which D must be stable

![[Pasted image 20241025203939.png]]
$t_{su}$ => a change in D has to have had time to be seen by both outputs before the negative `Clk` edge affects the output gates; here $t_{su} > 3 \triangle$ to reliably transition Q from 1 -> 0

typically values for 28nm CMOS are $t_su = 0.03 ns$ and $t_h = 0.02 ns$
![[Pasted image 20241123123750.png]]

#### $t_h$ for a positive edge triggered D  flip-flop
![[Pasted image 20241123123819.png]]
$t_h > \triangle_2$
if gate N2 is slow relative to gates N4 and N1, then the risk of hold time violations rises
![[Pasted image 20241123123900.png]]
The larger $\epsilon$ is, the less likely it is for N2 to be held high

#### Other types of flip-flops T flip-flop
![[Pasted image 20241123124050.png]]![[Pasted image 20241123124054.png]]
![[Pasted image 20241123124101.png]]

## JK flip-flop
combines the features of an SR flip-flop and a T flip-flop
![[Pasted image 20241123124128.png]]

### three-bit up-counter (ripple counter)
![[Pasted image 20241123124146.png]]

#### Derivation of a synchronous up-counter
in which all output bits change at the same time
based on T FF triggered by the one clock signal
![[Pasted image 20241123134623.png]]

#### Four-bit synchronous up-counter
![[Pasted image 20241123134641.png]]

##### Inclusion of an Enable and asynchronous Clear capability
![[Pasted image 20241123134704.png]]
![[Pasted image 20241123135217.png]]

#### Behavioural code for four-bit up-counter with asynchronous clear
```vhdl
library ieee;
use ieee.std_logic_1161.all;
use ieee.std_logic_unsigned.all; -- needed to be able to increment Count

entity upcount is
	port (
		Clock, Resetn, E : in std_logic;
		Q : out std_logic_vector(3 downto 0)
	);
end upcount;

architecture Behavior of upcount is
	signal Count : std_logic_vector(3 downto 0);
begin
	process(Clock, Resetn)
	begin
		if Resetn = '0' then
			Count <= "0000";
		elsif Clock'event and Clock = '1' then
			if E = '1' then
				Count <= Count + 1;
			else
				Count <= Count; -- not required because of implied memory semantics
			end if;
		end if;
	end process;
	Q <= Count;
end Behavior;
```

### Starting the count from any value
![[Pasted image 20241123135535.png]]

#### four-bit counter with parallel load, using INTEGER signals
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity upcount is
	port (
		R : in integer range 0 to 15;
		Clock, Resetn, L : in std_logic;
		Q : buffer integer range 0 to 15
	);
end upcount;

architecture Behavior of upcount is
begin
	process (Clock, Resetn)
	begin
		if Resetn = '0' then
			Q <= "0000";
		elsif Clock'event and Clock = '1' then
			if L = '1' then
				Q <= R;
			else
				Q <= Q + 1;
		end if;
	end process;
end Behavior;
```

#### Counting the count range
a modulo-6 counter with synchronous reset
![[Pasted image 20241123140353.png]]

##### module-6 counter with asynchronous reset
![[Pasted image 20241123140417.png]]

#### Code for a down-counter
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity downcount is
	generic ( modulus : integer := 8);
	port (
		Clock, L, E : in std_logic;
		Q : out integer range 0 to modulus - 1
	);
end downcount;

architecture Behavior of downcount is
	signal Count : integer range 0 to modulus - 1;
begin
	process
	begin
		wait until Clock'event and Clock = '1';
		if L = '1' then
			Count <= modulus - 1;
		else
			if E = '1' then
				Count <= Count - 1;
			end if;
		end if;
	end process;
	Q <= Count;
end Behavior;
```

#### two-digit bcd counter
![[Pasted image 20241123141133.png]]

#### Ring counter
![[Pasted image 20241123141142.png]]

### Timing analysis of flip-flop circuits
Usually the maximum clock frequency a circuit can be operated at, $F_max$, needs to be determined
whether any hold times are violated also needs to be determined 

### Timing analysis of a simple flip-flop circuit
consider the simpler circuit shown, and let's assume that $t_su = 0.6ns, t_h = 0.4 ns$, and $0.8 ns <= t_{cQ} <= 1.0ns$
assume the delay of a k-input value is $1 + 0.1k$ ns
to calculate $T_{min} = 1 / {F_{max}}$,  we need to determine the **longest timing path** in the circuit (critical path) that starts and ends at a FF
$T_{min} = max(t_{cQ})+t_{NOT} + t_{su}$
i.e. $T_{min} = 1.0 + 1.1 + 0.6 = 2.7 ns$
and $F_{max} = 1 / T_{min} = 370 MHz$

need to check the hold time violations by considering the *shortest possible delay* from any +ve clock edge to any FF input
$min(t_{cQ} ) + t_{NOT} = 0.8 + 1.1 = 1.9 ns > t_h = 0.4ns$
therefore, no violation
![[Pasted image 20241123145949.png]]

## Timing analysis of a 4-bit counter
![[Pasted image 20241123150024.png]]
Assume the same timing parameters as in the previous example; critical path:
$T_{min} = max(t_{cQ(Q0)}) + 3(t_{AND}) + t_{XOR} + t_{su(Q3)}$
$= 1.0 + 3(1.2) + 1.2 + 0.6$
$= 6.4ns$
$F_{max} = 1/6.4 ns =  156 MHz$
this assumes Enable is well behaved; if not, $F_{max}$ may need to be reduced

shortest path from clock to D for each FF is $min(t_{cQ}) + t_{XOR} = 0.8 + 1.2 = 2.0 ns > t_h = 0.4 ns$
therefore, no hold violations (given Enable is well behaved)

==Q: When is it best to (de)assert Enable?==
## Clock skew
Clock skew is the spread in time (relative delay) in clock edges arriving at the various synchronous components of a digital circuits
caused by wire delays (typically)
FPGAs have special clock distribution networks, which use low-resistance (fat) wiring tracks, buffers that amplify the clock signal, and 'balanced' layouts, such as H-trees with the root located at the centre of the chip, to minimise clock skew

### Effect of clock skew of $F_{max}$ for the 4-bit counter
![[Pasted image 20241123150632.png]]
Assumes $t_{skew} = 1.5ns$ delay on clock pulses arriving at Q3
delay on path from Q0 to Q3 is then given by
$t_{cQ} + 3(t_{AND}) + t_{XOR} + t_{su} - t_{skew} = 6.4 - 1.5 = 4.9 ns$
since the skew provides additional time before data is loaded into Q3
however, critical path is now from Q0 to Q2, i.e.
$T_{min} = t_{cQ} + 2(t_{AND}) + t_{XOR} + t_{su}$
$= 1.0 + 2(1.2) + 1.2 + 0.6$
$= 5.2 ns$
$F_max = 192 MHz$

## Negative clock skew
a negative clock skew, (i.e. clock arriving earlier at Q3 than at Q0 - Q2) would have the opposite effect of lengthening the clock period requirement & reducing the maximum clock frequency

### Effect of clock skew on hold times
as *positive clock skew* has the effect of delaying the loading of data into FF Q3, it has the effect of increasing the hold time requirement of this FF to $t_h + t_{skew}$ for all paths that end at Q3
the shortest such path is from FF Q2 to Q3 and has delay $t_{cQ} + t_{AND} + t_{XOR} = 0.8 + 1.2 + 1.2 = 3.2 ns > t_h + t_{skew} = 1.9ns$
therefore, no violation
but, when $t_{skew} >= 3.2 - t_h = 2.8 ns$, then hold time violations will exist and the circuit will not work reliably at any frequency
good circuit design therefore aims to minimise, if not eliminate, clock skew

