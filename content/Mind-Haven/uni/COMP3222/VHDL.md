
## Typical CAD flow for logic design
![[Pasted image 20240909220732.png]]
CAD = Computer Aided Design

HDL is similar to a computer programming language except that it is used to describe hardware instead of instructions to be executed on a processor (similar to markup languages)
VHDL used in this course (another commonly used language is verilog)

## VDHL
very high speed integrated circuit HDL
Schematic capture - use of a graphical tool to provide a logic circuit diagram, instead of this, the use of hardware description language (VHDL), to capture design's intents offers number of advantages
- source code is plain text, it can be easily edited, copied and search, designers can also include documentation explaining how the design works
- VHDL (standard language) provides design portability - design specified in VHDL can be implemented in different types of chips and with CAD tools from different vendors
- Portability is useful because digital circuit technology changes rapidly - by using standard language, designer can focus on the functionality without being concerned about the details of the implementation technology

### Simple logic `fn` and its VHDL entity declaration
VHDL entity declaration expresses what the design unit 'looks like' to the outside world, i.e. describes its interface
![[Pasted image 20240909223632.png]]
```vhdl
ENTITY example1 IS
	PORT(x1, x2, x3  : IN  BIT;
			f        : OUT BIT);
END example1;
```
`IN` - the 'mode' of a signal indicates its direction
Signals of type `BIT` can assume values 0 and 1 
`x1, x2, x3` and `example1` - valid identifiers start with a letter, and comprise alpha-numeric and underscore characters

The architecture of a VDHL design unit describes what the design 'looks like' on the inside i.e. what its behaviour and/or structure is
example1,
here, the functionality of the circuit is expressed using a *simple signal assignment statement*
in VHDL, all statements need to be terminated by a semi-colon (;)
Note that in VHDL, the Boolean operators all have the same precedence, hence we must use parentheses to indicate the intended meaning
```vhdl
ARCHITECTURE LogicFunc OF example1 IS
BEGIN
	f  <= (x1 AND x2) OR (NOT x2 AND x3);
END LogicFunc;
```

In our code, we'll use capital letters to represent KEYWORDS in the code; 
Note that VHDL is not case sensitive

### Four-input example
![[Pasted image 20240909224041.png]]

```vhdl
ENTITY example2 IS
	PORT(x1, x2, x3, x3   : IN BIT;
			f             : OUT BIT );
END example2;

ARCHITECTURE LogicFunc OF example2 IS
BEGIN
	f  <= (x1 AND x3) OR (x2 AND x4);
	g <= (x1 OR NOT x3) AND (NOT x2 OR x4);
END LogicFunc;
```
Note: simple signal assignment statements are examples of *concurrent assignment statements* - they are all 'evaluated' at the same time when a signal on the RHS changes value; therefore the order doesn't matter
