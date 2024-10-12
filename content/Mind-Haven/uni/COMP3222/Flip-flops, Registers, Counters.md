
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





## Timing analysis of a 4-bit counter

==Q: When is it best to (de)assert Enable?==


## Clock skew
Clock skew is the spread in time (relative delay) in clock edges arriving at the various synchronous components of a digital circuits
caused by wire delays (typically)
