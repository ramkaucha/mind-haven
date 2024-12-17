
| Apply design techniques to comprehensive digital design problems<br>- Consider the datapath components needed, the finite state machines required for their control and their description in VHDL  | []  |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| Learn how digital systems comprising datapaths and control circuits can be derived from an ASM chart                                                                                               | []  |
| Look art a number of practical issues to do with real system inputs and outputs                                                                                                                    | []  |
| Examine the description of increasingly complex algorithms in hardware<br>- Binary multiplication<br>- Binary division<br>- Calculating the mean of a set of numbers<br>- Sorting a set of numbers | []  |


## Digital Systems
![[Pasted image 20241108194442.png]]
A digital system comprises a *datapath*, which transforms the data as required by a specification, and a *controller* (control unit, control path), which supervises the operation of the datapath by monitoring its *status* and setting *control* signals
the behaviour of both parts is conveniently modelled in an integrated manner using an **Algorithmic State Machine** (ASM) chart

### Details for connecting register to a bus
consider two 2-bit registers
	3-state buffers are used to avoid 'trying' outputs together
![[Pasted image 20241108194716.png]]
## Control circuit design
Consider the control required to swap the contents of R1 and R2 using R3 for temporary storage
	what are the individual register transfers required to effect the swap?
	which control signals need to be asserted for each transfer?
	when & how should the control signals be sequenced?
![[Pasted image 20241108194852.png]]
In successive steps:
1. `R3 <= R2 : R3in <= '1'; R2out <= '1'`
2. `R2 <= R1 : R2in <= '1'; R1out <= '1'`
3. `R1 <= R3 : R1in <= '1'; R3out <= '1'`

### Shift-register based control circuit
swapping the contents of R1 and R2 using R3 for temporary storage
	- could use one-hot control to enable 3-state buffers and loading of registers
	- suffer delay of 1 cycle after input `w` is asserted, but could be converted to Mealy machine if desired
	- assumes `w` is deasserted for at least two clock cycles after it is asserted
![[Pasted image 20241108195103.png]]

#### Needed components : n-bit register with enable
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity regne is
	generic (n : integer := 8);
	port ( R : in std_logic_vector(n-1 downto 0);
			Rin, Clock : in std_logic;
			Q : out std_logic_vector(n-1 downto 0));
end regne;

architecture Behavior of regne is
begin
	process
	begin
		wait until clock'event and clock = '1;
		if Rin = '1' then
			Q <= R;
		end if;
	end process;
end Behavior;
```

#### Needed components : n-bit 3-state buffer
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity trin is
	generic ( n : integer := 8)
	port ( X : in std_logic_vector(n-1 downto 0);
			E : in std_logic;
			F : out std_logic_vector(n-1 downto 0)))
end trin;

architecture Behavior of trin is
begin
	F <= (others => 'Z') when E = '0' else X;
end Behavior
```

#### Needed component: L-R shift register with reset
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity shiftr is --left-to-rigth shift register with async reset
	generic(K : integer := 4)
	port ( Resetn, Clock, w : in std_logic;
			Q : buffer std_logic_vector(1 to K));
end shiftr;

architecture Behavior of shiftr is
begin
	process (Resetn, Clock)
	begin
		if Resetn = '0' then
			Q <= (others => '0');
		elsif Clock'event and Clock = '1' then
			Genbits: FOR i in K downto 2 loop
				Q(i) <= Q(i-1)
			end loop;
			Q(1) <= w;
		end if;
	end process;
end Behavior;
```

#### Package definition
```vhdl
library ieee;
use ieee.std_logic_1164.all;

package components is
	component regne -- register
		generic ( N : integer := 8);
		port ( R : in std_logic_vector(n-1 downto 0);
				Rin, Clock : in std_logic;
				Q : out std_logic_vector(n-1 downto 0));
	end component;

	component shiftr -- left-to-right shift register wtih async head
		generic (K : integer := 4);
		port (Resetn, Clock, w : in std_logic;
				Q : buffer std_logic_vector(1 to K))
	end component;

	component trin -- 3-state buffers
		generic( N : integer := 8);
		port ( X : in std_logic_vector(n-1 downto 0);
				E : in std_logic;
				F : out std_logic_vector(n-1 downto 0));
	end component;
end components;
```

#### the digital system from the example above used to swap R1 and R2 via R3
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity swap is
	port ( Data : in std_logic_vector(7 downto 0);
			Resetn, w : in std_logic;
			Clock, Extem : in std_logic;
			RinExt : in std_logic_vector(1 to 3); -- allows regs to be externally loaded
			BusWires : buffer std_logic_vector(7 downto 0));
end swap;

architecture Structure of swap is
	signal Rin, Rout, Q : std_logic_vector(1 to 3)
	signal R1, R2, R3 : std_logic_vector(7 downto 0);
begin
	control : shiftr generic map (K => 3)
		port map (Resetn, Clock, w, Q);
	Rin(1) <= RinExt(1) OR Q(3);
	Rin(2) <= RinExt(2) OR Q(2);
	Rin(3) <= RinExt(3) OR Q(1);
	Rout(1) <= Q(2); Rout(2) <= Q(1); Rout(3) <= Q(3);

	tri_ext : trin port map (Data, Extern, BusWires);
	reg1 : regn port map (BusWires, Rin(1), Clock, R1);
	reg2 : regn port map (BusWires, Rin(2), Clock, R2);
	reg3 : regn port map (BusWires, Rin(3), Clock, R3);
	tri1 : trin port map (R1, Rout(1), BusWires);
	tri2 : trin port map (R1, Rout(1), BusWires);
	tri3 : trin port map (R1, Rout(1), BusWires);
```
![[Pasted image 20241108201926.png]]
![[Pasted image 20241108201934.png]]


### Using multiplexers to implement a bus
more typical use of MUXes instead of 3-state buffer since programmable devices don't usually have many 3-state resources

![[Pasted image 20241108202107.png]]
#### Using MUXes for register swap
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity swapmux is
	port (Data : in std_logic_vector(7 downto 0);
			Resetn, w : in std_logic;
			Clock : in std_logic;
			RinExt : in std_logic_vector(1 to 3);
			BusWires : in std_logic_vector(7 downto 0));
end swapmux;

architecture Mixed of swapmux is
	signal Rin, Q : std_logic_vector(1 to 3);
	signal R1, R2, R3 : std_logic_vector (7 downto 0);
begin
	control : shiftr generic map (K => 3)
		port map (Resetn, Clock, w, Q);
	Rin(1) <= RinExt(1) OR Q(3);
	Rin(2) <= RinExt(2) OR Q(2);
	Rin(3) <= RinExt(3) OR Q(1);
	reg1 : regn port map (BusWires, Rin(1), Clock, R1);
	reg2 : regn port map (BusWires, Rin(2), Clock, R2);
	reg3 : regn port map (BusWires, Rin(3), Clock, R3);\
	muxes : with Q select
		BusWires <= Data when "000",
					R2 when "100",
					R1 when "010",
					R3 when others;
end Mixed;
```

## Algorithmic state machines
ASMs are a type of flowchart
	- they are used to represent more complex (larger) FSMs that are impractical to represent using state machines and state tables
	- they can be used to represent the state transitions and generated outputs for an FSM
	- can be used to capture datapath activity
3 type of elements in ASM charts
![[Pasted image 20241110060339.png]]
### Example
![[Pasted image 20241110060352.png]] ![[Pasted image 20241110060357.png]]

#### Pseudo-code for a bit counter (popcount)
```
input: A -- the word whose ON bits are to be counted
output: B -- the count of the number of ON bits in A

B = 0
while A != 0 do
	if a_0 = 1 then
		B = B + 1
	end if
	right shift A
end while
```

1. what datapath components do we need to perform the computation?
2. how do we control the computation
3. how do we transfer inputs/outputs

#### ASM for popcount
![[Pasted image 20241123121413.png]]
not that use of the "start" signal, s, to indicate when input is available, and a *Done*  signal to indicate when computation has finished
	handshake protocol used to communicate with the environment or "user" circuit

note that the ASM chart describes control and datapath aspects of the system in an integrated way

**Note**: of the state actions - particularly for S2
	since the *Shift right*  action is a Moore-like state output, it won't occur until the next active clock edge after state *S2* is entered, even if that edge causes transition to *S3*
	similarly, the conditional (Mealy-like) control output to increment *B* will not take effect until the active clock edge occurs after it is asserted

#### Datapath for the ASM chart
![[Pasted image 20241123121655.png]]
##### ASM chart for the bit counter contro circuit
![[Pasted image 20241123121828.png]]

### VHDL code for the bit-counting circuit
```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_unsigned.all;
use work.components.shiftrne;

entity bitcount is
	port (
		Clock, Resetn : in std_logic;
		LA, s : in std_logic;
		Data : in std_logic_vector(7 downto 0);
		B : buffer integer range 0 to 8;
		Done : out std_logic
	);
end bitcount;

architecture Behavior of bitcount is
	type State_type is (s1, s2, s3);
	signal y : State_type;
	signal A : std_logic_vector(7 downto 0);
	signal z, EA, LB, EB, low : std_logic;
BEGIN
	FSM_transitions: process(Resetn, Clock)
	BEGIN
		if Resetn = '0' then
			y <= S1;
		elsif Clock'event and Clock = '1' then
			case y is
				when s1 =>
					if s = '0' then y <= S1;
					else y <= s2; end if;
				when s2 =>
					if z = '0' then y <= s2;
					else y <= s3; end if;
				when s3 =>
					if s = '1' then y <= s3;
					else y <= s1; end if;
			end case;
		end if;
	END PROCESS;

	FSM_outputs : PROCESS(y, A(0))
	begin
		EA <= '0'; LB <= '0'; EB <= '0'; Done <= '0';
		Case y is
			when s1 =>
				LB <= '1';
			when s2 =>
				EA <= '1';
				if A(0) = '1' then EB <= '1';
			when s3 =>
				Done <= '1';
		end case;
	end process;

	-- the datapath circuit 
	upcount : process(Resetn, Clock)
	begin
		if Resetn = '0' then
			B <= '0';
		elsif Clock'event and Clock = '1' then
			IF LB = '1' THEN
				B <= '0';
			ELSIF EB = '1' THEN
				B <= B + 1;
			END IF;
		END IF;
	end process;

	low <= '0';
	ShiftA: shiftrne generic map (N => 8) PORT MAP (Data, LA, EA, low, Clock, A);
	z <= '1' when A = "00000000" ELSE '0';
END Behavior;
```
![[Pasted image 20241123122639.png]]

## Practical Issues
### Issue 1: Input switch debouncing
![[Pasted image 20241123122704.png]]
When an input switch is thrown, it can bounce for up to 10ms and thus give rise to an undesirable sequence of pulses on `Data` when wired as depicted
	one approach to avoiding misreads is to use a latch to trap the switch value
	another is to wait for 10ms bounce period before sampling Data

**Debounce code**
```vhdl
synchronise: process(clk)
begin
	wait until clk'event and clk = '1' -- rising clock edge
	input_prev <= input_switch; -- save the current switch setting

-- the following counter counts time that the inputs have been steady
-- the input signal must be steeady for approx. 10 milliseconds

	if input_switch != input_prev then -- if the switch has bounced
		sync_count <= (others => '0'); -- reset a counter
	elsif sync_count != x"80000" then -- otherwise, count ~10ms worth of clock
		sync_count <= sync_count + 1; -- pulses at 50MHz (assumed clock freq.)
	end if;

-- if the full time is reached, update the input signal
	if sync_count = x"80000" then
		input_value <= input_switch; -- switch has stopped bouncing
	end if;
end process synchronise;
```

### Issue 2: Asynchronous inputs
![[Pasted image 20241123123217.png]]
When an asynchronous input fails to satisfy setup or hold times, the flip-flop can enter a metastable value state (intermediate/indeterminate value) and not recover for an indefinite period of time

using a pair of flip-flops in series *significantly reduces* the likelihood that any synchronous system reading Data will observe such a metastable value
	COTS (Commercial, Off-the-Shelf) devices typically specify a maximum period of metastability
	as long as the clock period in the circuit above exceeds this value + the setup time, the FF on the right will not also enter a metastable state when the FF on the left does
	cost is one clock period of delay or latency in the arrival of the data input

### Issue 3
![[Pasted image 20241128210204.png]]
While seemingly attractive, clock gating to enable a FF is to be avoided as it contributes to clock skew

#### Better solution: A FF with an enable input
it is often desirable to control when a FF or register is loaded with a new value
![[Pasted image 20241128210258.png]]
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.all;

ENTITY rege IS
	PORT (R, Resetn, E, CLock : IN STD_LOGIC;
				Q : BUFFER STD_LOGIC);
END rege;

ARCHITECTURE Behavior OF rege IS
BEGIN
	PROCESS (Resetn, Clock)
	BEGIN
		IF Resetn = '0' THEN
			Q <= '0';
		ELSIF Clock'EVENT and Clock = '1' THEN
			IF E = '1' THEN
				Q <= R;
			ELSE
				Q <= Q;
			END IF;
		END IF;
	END PROCESS;
END Behavior;
```

#### VHDL code for n-bit register with an enable input
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity regne is
	generic ( N : integer := 4);
	port 
	(
		R : in std_logic_vector(N-1 downto 0);
		Resetn : in std_logic;
		E, Clock : in std_logic;
		Q : out std_logic_vector(N-1 downto 0)
	);
end regne;

architecture Behavior of regne is
begin
	process (Resetn, Clock)
	begin
		if Resetn = '1' then
			Q <= (OTHERS => '0');
		elsif Clock'event and Clock = '1' then
			if E = '1' then
				Q <= R;
			end if;
	end process;
end Behavior;
```

#### A shift register with parallel-load and enable control inputs
![[Pasted image 20241128211133.png]]
#### Code for right-to-left shift with an enable input
```vhdl
library ieee;
use ieee.std_logic_1164.all;

-- right-to-left shift register with parallel load and enable
entity shiftlne is
	generic ( N : integer := 4);
	port (
		R : in std_logic_vector(n-1 downto 0);
		L,E,w : in std_logic;
		Clock : in std_logic;
		Q : Buffer std_logic_vector(n-1 downto 0)
	);
end shiftlne;

architecture Behavior of shiftlne is
begin
	process
	begin
		wait until Clock'event and Clock = '1' then
		if L = '1' then
			Q <= R;
		elsif E = '1' then
			Q(0) <= w;
			Genbits: for i in 1 to N-1 loop
				Q(i) <= Q(i-1);
			end loop;
		end if;
	end process;
end Behavior;
```

#### Component declaration statements assumed for remaining design problems
```vhdl
library ieee;
use ieee.std_logic_1164.all;

package component is
	--2-to-1 multiplexer
	component mux2to1
		port (w0, w1, : in std_logic;
				s : in std_logic;
				f : out std_logic);
	end component;

	-- D FF with 2-to-1 multiplexer connect to D component muxdff
	component muxdff
		port (D0, D1, Sel, Clock : in std_logic;
				Q : out std_logic);
	end component;

	-- n-bit register with enable
	component regne
		generic (N : integer := 4);
		port (
			R : in std_logic_vector(N-1 downto 0);
			Resetn : in std_logic;
			E, Clock : in std_logic;
			Q : out std_logic_vector(N-1 downto 0)
		);
	end component;

	-- n-bit right-to-left shift register with parallel load and enable
	component shiftlne -- shift left (towards msb)-mult by 2
		generic(N : integer := 4);
		port (
			R : in std_logic_vector(N-1 downto 0);\
			L,E,w : in std_logic;
			Clock : in std_logic;
			Q : buffer std_logic_vector(N-1 downto 0)
		);
	end component;

	-- n-bit left-to-right shift register with parallel and enable
	component shiftrne -- shift right (towards lsb)-div by 2
		generic(N : integer := 4);
		port (
			R : in std_logic_vector(N-1 downto 0);
			L,E,w : in std_logic;
			Clock : in std_logic;
			Q : buffer std_logic_vector(N-1 downto 0)
		);
	end component;

	-- up-counter that counts up from initial value R to modulus-1
	component upcount
		generic (modulus: integer := 8);
		port (
			Resetn : in std_logic;
			Clock, E, L : in std_logic;
			R : in integer range 0 to modulus-1;
			Q : buffer integer range 0 modulus-1
		);
	end component;

	-- down-counter that counts from modulus-1 down to 0
	component downcnt
		generic (modulus : integer := 8);
		port (
			Clock, E, L : in std_logic;
			Q : buffer integer range 0 to modulus-1
		);
	end component;
end components;
```


### Design exercise 2
implement a binary multiplexer circuit
![[Pasted image 20241128214426.png]]
Question the designer needs to answer
1. how is the computation performed? What algorithm is to be used?
2. What datapath components are required?
3. How are they controlled ? How is their timing to to be controlled?

#### ASM chart for the multiplexer
![[Pasted image 20241128214530.png]]
A: multiplicand
B: multiplier
P: product
```
P = 0
for i = 0 to n-1 do
	if b_i = 1 then
		P = P + A
	end if
	left shift A
end for
```

#### Datapath circuit for the multiplier
![[Pasted image 20241128214638.png]]
![[Pasted image 20241128214647.png]]

#### ASM chart for the multiplier control circuit
![[Pasted image 20241128214714.png]]
#### VHDL code for the multiplier circuit
![[Pasted image 20241128215225.png]]
```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_unsigned.all;
use work.components.all;

entity multiply is
	generic (N : integer := 8; NN : integer := 16);
	port (
		Clock : in std_logic;
		Resetn : in std_logic;
		LA, LB, s : in std_logic;
		DataA : in std_logic_vector(N-1 downto 0);
		DataB : in std_logic_vector(N-1 downto 0);
		P : buffer std_logic_vector(NN-1 downto 0);
		Done : out std_logic
	);
end multiply;

architecture Behavior of multiply is
	type State_type IS (S1, S2, S3);
	signal y : State_type
	signal Psel, z, EA, EB, EP, Zero : std_logic;
	signal B, N_Zeros : std_logic_vector(N-1 downto 0);
	signal A, AIn, DataP, Sum : std_logic_vector(NN-1 downto 0);
begin
	FSM_transitions: process(Resetn, Clock)
	begin
		if Resetn = '0' then
			y <= S1;
		elsif Clock'event and Clock = '1'
		then case y is
			when S1 =>
				if s = '0' then y <= S1;
				else y <= Y2; end if;
			when S2 =>
				if z = '0' then y <= S2;
				else y <= S3; end if;
			when S3 =>
				if s = '1' then y <= S3;
				ELSE y <= S1; end if;
			end case;
		end if;
	end process;

	FSM_outputs : process(y, B(0))
	begin
		EP <= '0'; EA <= '0'; EB <= '0';
		Done <= '0'; Psel <= '0';
		Case y is
			when S1 =>
				EP <= '1'
			when S2 =>
				EA <= '1'; EB <= '1'; Psel <= '1';
				if B(0) = '1' then EP <= '1';
				END IF;
			when S3 =>
				Done <= '1';
		end case;
	end process;

	-- define the datapath circuit
	Zero <= '0';
	N_Zeros <= (others => '0');
	Ain <= N_Zeros & DataA;
	ShiftA: shiftlne generic map (N => NN)
		port map (Ain, LA, EA, Zero, Clock, A);
	ShiftB : shiftrne generic map (N => N)
		port map (DataB, LB, EB, Zero, Clock, B);
	z <= '1' when B = N_Zeros else '0';
	Sum <= A + P;
	-- define the 2n 2-to-1 multiplexers for DataP
	GenMUX: for i in 0 to NN-1 generate
		Muxi: mux2to1 port map (Zero, Sum(i), Psel, DataP(i));
	end generate;
	RegP: regne generic map(N => NN);
		port map (DataP, Resetn, EP, Clock, P);
end Behavior;
```

#### Simulation result fort the multiplier circuit
![[Pasted image 20241128220116.png]]

### Design 3: Division
![[Pasted image 20241128220202.png]]
```
R = 0
for i = 0 to n - 1 do
	Left-shift R || A;
	if R >= B then
		Q_n-1-i = 1
		R = R - B;
	else
		q_n-1-i = 0;
	end if;
end for;
```

1. How is the computation performed?
2. What datapath components are requried
3. How are they to be controlled?

##### ASM chart for the divider
![[Pasted image 20241128220322.png]]

##### Datapath circuit for the divider
![[Pasted image 20241128220343.png]]![[Pasted image 20241128220351.png]]
![[Pasted image 20241128220410.png]]

#### An example of division using n = 8 clk cycles
one drawback of the divider we've designed is that it takes two cycles per iteration to (i) shift R || A, and (ii) update R <- R - B when required
	if possible enhancement is to perform a shift and a subtraction in a single clock cycle (write the results of the subtraction to $r_{n-1}..r_0$|| write the next bit of A to a temp FF, $rr_0$)
	a second enhancement is to reuse the redundant bits of shift register A to store Q 
![[Pasted image 20241128220614.png]]
![[Pasted image 20241128220623.png]]

#### VHDL code for the enhanced divider circuit
![[Pasted image 20241128221221.png]]
```vhdl
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_unsigned.all;
use work.components.all;

entity divider is
	generic (N : integer := 8);
	port (
		Clock : in std_logic;
		Resetn : in std_logic;
		s, LA, EB : in std_logic;
		DataA : in std_logic_vector(N-1 downto 0);
		DataB : in std_logic_vector(N-1 downto 0);
		R, Q : buffer std_logic_vector(N-1 downto 0);
		Done : out std_logic
	);
end divider;

architecture Behavior of divider is
	type State_type is (S1, S2, S3);
	signal y : Sate_type;
	signal Zero, Cout, z : std_logic;
	signal EA, Rsel, LR, ER, ER0, LC, EC, R0 : std_logic;
	signal A, B, DataR : std_logic_vector(N-1 downto 0);
	signal Sum : std_logic_vector(N downto 0);
	signal Cout : integer range 0 to N-1;
begin
	FSM_transitions: process(Resetn, Clock)
	begin
		if Resetn = '0' then y <= S1;
		elsif Clock'event and Clock = '1' then
			Case y is
				when S1 =>
					if s = '0' then y <= S1; ELSE y <= S2; end if;
				when S2 =>
					if z = '0' then y <= S2; else y <= S3; end if;
				when S3 =>
					if s = '1' then y <= S3; else y <= S1; end if;
			end case;
	end process;

	FSM_outputs: process(s, y, Cout, z)
	begin
		LR <= '0'; ER <= '0'; ER0 <= '0';
		LC <= '0'; EC <= '0'; EA <= '0'; Done <= '0';
		Rsel <= '0';
		case y is
			WHEN S1 =>
				LC <= '1';
				if s = '0' then
					LR <= '1'; EA <= '0'; ER0 <='0';
				else
					LR <= '0'; EA <= '1'; ER0 <= '1';
				end if;
			WHEN S2 =>
				Rsel <= '1'; ER <= '1'; ER0 <= '1'; EA <= '1';
				if Cout <= '1' THEN LR <= '1'; ELSE ER <= '0'; END IF;
				IF z = '0' THEN EC <= '1'; ELSE EC <= '0'; END IF;
			WHEN S3 =>
				Done <= '1';
		end case;
	end process;

	-- define the datapath circuit
	Zero <= '0';
	RegB: regne generic map (N => N)
		port map (DataB, Resetn, EB, Clock, B);
	ShiftR: shiftlne generic map (N => N)
		port map (DataR, LR, ER, R0, Clock, R);
	FF_R0: muxdff port map (Zero, A(N-1), ER0, Clock, R0);
	ShiftA: shiftlne generic map (N => N)
		port map (DataA, LA, EA, Cout, Clock, A);
	Q <= A;
	Counter : downcnt generic map (modulus => N)
		port map (Clock, EC, LC, Count);
	z <= '1' when Count = 0 else '0';

	Sum <= R & R0 + (NOT  B + 1);
	Cout <= Sum(N);
	DataR <= (others => '0') when Rsel = '0' else Sum(N-1 downto 0);
end Behavior;
```

##### Simulation results for enhanced divider circuit
![[Pasted image 20241128221825.png]]

#### Design ex 4: Finding the mean of k numbers
![[Pasted image 20241128221851.png]]
```
Sum = 0;
for i = k - 1 downto 0 do
	Sum = Sum + R_i;
end for;
M = Sum / k;
```
(a) pseudo-code

##### Finding the mean of k numbers - datapath
![[Pasted image 20241128221958.png]]

##### Datapath & controlpath ASM for the mean operation
![[Pasted image 20241128222017.png]]
##### Schematic of the mean circuit using a RAM block
![[Pasted image 20241128222038.png]]

##### Simulation of the mean circuit using block RAM
![[Pasted image 20241128222052.png]]

#### Design Ex 5: Sort k words
![[Pasted image 20241128222110.png]]
```
// selection sort 1d array of data
for i = 0 to k-2 do
	A = R_i;
	for j = i+1 to k-1 do
		B = R_j;
		if B < A then // swap elements
			R_j = A;
			R_i = B; // place smallest of [i + 1, k - 1] in R_i
			A = R_i;
		end if;
	end for;
end for;
```

##### ASM chart for the sort operation
![[Pasted image 20241128222229.png]]
```
for i = 0 to k-2 do
	A = R_i;
	for j = i+1 to k-1 do
		B = R_j;
		if B < A then
			R_j = A;
			R_i = B;
			A = R_i;
		end if;
	end for;
end for;
```

![[Pasted image 20241128222334.png]]
![[Pasted image 20241128222343.png]]
![[Pasted image 20241128222356.png]]

##### VHDL code for the sort operation
![[Pasted image 20241128223235.png]]
![[Pasted image 20241128223242.png]]
![[Pasted image 20241128224349.png]]
```vhdl
library ieee;
use ieee.std_logic_1164.all;
use work.components.all;

entity sort is
	generic (N : integer := 4);
	port (
		Clock, Resetn : in std_logic;
		s, WrInit, Rd : in std_logic;
		DataIn : in std_logic_vector(N-1 downto 0);
		RAdd : in integer range 0 to 3;
		DataOut : buffer std_logic_vector(N-1 downto 0);
		Done : buffer std_logic
	);
end sort;

architecture Behavior of sort is
	type state_type is (S1, S2, S3, S4, S5, S6, S7, S8, S9);
	signal y : state_type;
	signal Ci, Cj : integer range 0 to 3;
	signal Rin : std_logic_vector(3 downto 0);
	type RegArray is
		array(3 downto 0) of std_logic_vector(N-1 downto 0);
	signal R: RegArray;
	signal RData, ABMux : std_logic_vector(N-1 downto 0);
	signal Int, Csel, Wr, BltA : std_logic;
	signal CMux, IMux : integer range 0 to 3;
	signal Ain, Bin, Bout : std_logic;
	signal LI, LJ, EI, EJ, zi, zj : std_logic;
	signal Zero : integer range 3 downto 0; -- parallel data for Ci = 0
	signal A, B, ABData : std_logic_vector(N-1 downto 0);

begin
	FSM_transitions: process(Resetn, Clock)
	begin
		if Resetn = '0' then
			y <= S1;
		elsif Clock'event and Clock = '1' then
			case y is
				WHEN S1 =>
					if S = '0' then y <= S1;
					else y <= S2; end if;
				WHEN S2 => y <= S3;
				WHEN S3 => y <= S4;
				WHEN S4 => y <= S5;
				WHEN S5 =>
					if BltA = '1' then y <= S6;
					else y <= S8; end if;
				WHEN S6 => y <= S7;
				WHEN S7 => y <= S8;
				WHEN S8 =>
					if zj = '0' then y <= S4;
					elsif zi = '0' then y <= S2;
					else y <= S9;
					end if;
				WHEN S9 => if s = '1' then y <= S9; else y <= S1; end if;
			end case;
		end if;
	end process;

	-- define the outputs generated by the fsm
	Int <= '0' when y = S1 else '1';
	Done <= '1' when y = S9 else '0';
	FSM_outputs: process(y, zi, zj)
	begin
		LI <= '0'; LJ <= '0'; EI <= '0'; EJ <= '0'; Csel <= '0';
		Wr <= '0'; Ain <= '0'; Bin <= '0'; Bout <= '0';
		case y is
			WHEN S1 => LI <= '1';
			WHEN S2 => Ain <= '1'; LJ <= '1';
			WHEN S3 => EJ <= '1';
			WHEN S4 => Bin <= '1'; Csel <= '1';
			WHEN S5 => -- no outputs asserted in this state
			WHEN S6 => Csel <= '1'; Wr <= '1';
			WHEN S7 => Wr <= '1'; Bout <= '1';
			WHEN S8 => Ain <= '1';
				if zj = '0' then
					EJ <= '1';
				else
					EJ <= '0';
					if zi = '0' then
						EI <= ''1;
					else
						EI <= '0';
					end if;
				end if;
			WHEN S9 => -- done is assigned 1 by conditional signal assignment
		end case
	end process;

	-- define the datapath circuit
	Zero <= 0;
	GenReg : for i in 0 to 3 generate
		Reg : regne generic map (N => N)
			port map (RData, Resetn, Rin(i), Clock, R(i));
	end generate;
	RegA : regne generic map (N => N)
		port map (ABData, Resetn, Ain, Clock, A);
	RegB : regne generic map (N => N)
		port map (ABData, Resetn, Bin, Clock, A);
	BltA <= '1' when B < A else '0';
	ABMux <= A when Bout = '0' else B;
	RData <= ABMux when WrInit = '0' else DataIn;
	OuterLoop : upcount generic map (modulus => 4)
		port map (Resetn, Clock, EI, LI, Zero, Ci);
	InnerLoop : upcount generic map (modulus => 4)
		port map (Resetn, Clock, EJ, LJ, Ci, Cj);
	CMux <= Ci when Csel = '0' else Cj;
	IMux <= Cmux when Int = '1' else Radd;
	with IMux Select
		ABData <= R(0) when 0,
					R(1) when 1,
					R(2) when 2,
					R(3) when others;

	RinDec : process (WrInit, Wr, IMux)
	begin
		if (WrInit OR Wr) = '1' then
			Case IMux is
				when 0 => Rin <= "0001";
				when 1 => Rin <= "0010";
				when 2 => RIn <= "0100";
				when others => Rin <= "1000";
			end case;
		else Rin <= "0000";
		end if;
	end process;

	Zi <= '1' when Ci = 2 else '0';
	Zj <= '1' when Cj = '3 else '0';
	DataOut <= (others => 'Z') when Rd = '0' else ABData
	
end Behavior;
```