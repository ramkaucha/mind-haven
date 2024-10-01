
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




## Timing analysis of a 4-bit counter

==Q: When is it best to (de)assert Enable?==
