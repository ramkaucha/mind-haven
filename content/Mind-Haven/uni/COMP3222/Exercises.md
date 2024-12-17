## Boolean Functions

1. 2.2 - done
2. * 2.3 - done
3. 2.7 (b) - done
4. * 2.8; Also, derive the truth table for the function _f_  - done
5. 2.12 - keep getting the wrong answer
6. 2.15 - done
7. * 2.29 - done
8. 2.32  - done 
9. 2.33 - done 
10. * 2.50 - done

## Simplifying Combinational Functions
1. 4.8 - done
2. * 4.9 - done
3. 4.12- done
4. 4.16
5. 4.17
6. * 4.20 - What is the cost of your solution?
7. * 4.37
8. 4.39
9. 4.45
10. * Draw the circuit corresponding to the following VHDL code:

```
ENTITY q4 IS
	PORT ( x1, x2 : IN BIT;
			f, g : OUT BIT);
END q4;

ARCHITECTURE logicfunc of q4 IS
	SIGNAL x3: BIT;
BEGIN
	g <= x1 OR x3;   -- line 1
	f <= x3;         -- line 2
	x3 <= x1 AND x2; -- line 3
END logicfunc;
```
Explain the effect of swapping lines 1 and 3 ?

## Arithmetic Circuits
1. Review Example 5.10
2. * Discuss Example 5.11
3. 5.10
4. * 5.18
5. 5.19
6. * 5.21
7. * 5.22
8. 5.25
9. 5.27
10. * 5.28

## Combinational Logic Blocks
1. 6.1 - done
2. 6.5 - done
3. *6.7 -  done
4. * 6.11 - done
5. 6.12 - done
6. * 6.18 - done
7. * 6.19 - just write the selected signal assignment statement - done
8. Redo 6.19 using a conditional assignment statement -- done
9. * Describe the situations when you would use a selected signal assignment statement in VHDL. When would you use a conditional assignment statement instead?
10. * 6.26 - you may use the code from 6.18 - done
11. 6.31
12. On lecture slide L04/S22 the claim is made that it is more convenient to design a 4-to-2 priority encoder by defining intermediate signals i0 - i3 and expressing y0, y1 and z in terms of these signals. Implement the 4-to-2 priority encoder using the usual design methods. Which approach leads to a simpler implementation?
13. Write the architecture body of the code on slide L04/S31 more clearly. Move the instantiation of Dec_left outside G1 to avoid the nested G2 block

## Flip-flops
1. Discuss Example 7.13
2. 7.1 - done
3. 7.3 - done
4. 7.5 - done - using 2 d ff -> XOR gates, -> using enable ,  first ff has 1
5. 7.6 - done 
6. 7.8 - done
7. Consider the following VHDL code:
```VHDL
PROCESS(Resetn, Clk)
BEGIN
	IF Resetn = '0' THEN
		Q <= '0';
	ELSIF Clk'EVENT AND Clk = '1' THEN
		Q <= D;
	END IF;
END PROCESS;
```
Assume `Resetn` transitions from '0' -> '1' (low to high) while the `Clk` signal is '1' (high)
a. What is the value of 'Q' at this instant?
At that instance 'Q' = 0
b. What would the value of 'Q' become if, before the `Clk` signal goes low, the `D` signal transitions from '0' -> '1'?
Q  = '0'
c. If the `ELSIF` clause were written as `ELSIF Clk = '1' THEN` instead of `ELSIF Clk'EVENT AND Clk = '1' THEN`, what would the value of `Q` be after each of the scenarios described in a. and b. above?
a. Q = D
b. Q  = 1

8. Determine the expected skew of a clock signal that is generated in the bottom-left corner and arrives at the top-right corner of a silicon device that measures 2cm x 2cm assuming that signals travel at 0.3X the speed of light (3x10^9 m/s) in device wires
1. First, let's understand what we need:
    - Find clock skew between bottom-left and top-right corners
    - Distance is diagonal across 2cm × 2cm chip
    - Signal speed = 0.3 × speed of light
2. Calculate the diagonal distance using Pythagorean theorem:
    - Distance = √(2cm² + 2cm²)
    - Distance = √8 cm
    - Distance = 2.83 cm = 0.0283 meters
3. Calculate signal speed:
    - Speed of light = 3 × 10⁹ m/s
    - Signal speed = 0.3 × (3 × 10⁹)
    - Signal speed = 9 × 10⁸ m/s
4. Calculate time (skew) = distance/speed:
    - Time = 0.0283 / (9 × 10⁸)
    - Time = 3.14 × 10⁻¹¹ seconds
    - Time = 31.4 picoseconds

Therefore, the expected clock skew is approximately 31.4 picoseconds.
1. Slide L05/S56 suggests that the user of the 4-bit counter cannot change the `Enable` signal at any time. What are the hazards in asserting/deasserting this signal, and when is it safe to change the signal level
Summary:

## Finite state machines
1. * 8.1 - Sketch the state diagram and draw the derived circuit. - done
2. 8.2 - Hint: determine the JK settings needed to achieve 0->0, 0->1, 1->0 and 1->1 transitions in the flip-flops.
3. * 8.3 - Assume you are designing a Mealy machine
4. * Derive the state table for problem 8.3 and derive the circuit using T-type flip-flops.
5. * 8.4
6. * Repeat 3., 4. and 5. assuming you are designing a Moore machine
7. 8.9
8. 8.10
9. 8.15
10. * 8.11; Derive a Mealy-type FSM from the description. Minimize the number of states required and redraw the diagram.
11. * 8.26; Describe the FSM using behavioural VHDL code.
12. * 8.29; Assuming _tsu_ = 0.6 ns, _tcQ_ = 0.8 - 1.0 ns, _th_ = 0.4 ns and the delay of a _k_-input gate is _tk-gate_ = 1.0 + 0.1k ns, what is the maximum frequency of this cicruit? All other things being equal, how large would _th_ need to be to have hold time being violated?
13. 8.8
14. 8.19
15.  8.42

Design exercise (L06/S90)
parity generator for serial communication design an even parity generator to produce parity bit *p* to replace $b_7 = 0$ of each ASCII byte *B* that is to be serially transmitted by the system below
![[Pasted image 20241128210025.png]]

## Digital system design
1. * 10.5
2. * 10.22
3. * 10.12
4. * Blackjack player ASM chart


Notes:
How does FF even work (the block)
Go through all the lec codes and understand it
check if its hand written or typed
