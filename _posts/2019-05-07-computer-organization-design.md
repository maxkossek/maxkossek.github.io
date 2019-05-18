---
layout: post
title: Computer Organization and Design (Patterson & Hennessy)
date: 2019-05-07
categories: [book]
tags: [computer science]
author: "Max Kossek"
description: Book Notes for the 5th edition of the book Computer Organization and Design by Patterson & Hennessy
---


## 1 Computer Abstractions and Technology

### 1.1 Introduction
Personal computers delivery good performance to single users at low cost.[^book] Servers carry larger workloads and have greater storage, input and output capacity. Embedded computers are largest segment of computers. They are in a variety of applications (car, tv, airplane etc.). Computers are designed towards their applications, some focus on dependability, speed or other factors.


### 1.2 Eight Great Ideas in Computer Architecture
1. Moore's Law (Integrated circuit resources double every 18-24 months)
2. Abstraction
3. Make the common case fast
4. Parallelism
5. Pipelining
6. Prediction
7. Hierarchy of Memories
8. Dependability via Redundancy


### 1.3 Below Your Program
Operating system creates the interface between the program and the hardware. Compilers translate a program written in a higher-level language into hardware instructions. Assembler translates symbolic version of instructions into binary code.


### 1.4 Under the Covers
Five classic components of a computer: input, output, memory, datapath and control. Datapath and control combine to form the processor. Processors execute the instructions of the program and do arithmetic and logic operations as well as signal states.


### 1.5 Technologies for Building Processors and Memory
*Transistor* is a simple electrical on/off switch. Integrated circuits combine multiples of transistors. Silicon, a semiconductor (conducts electricity moderately well) is the basis of most chips.


### 1.6 Performance
Individual users are more focused on execution time of a task. Datacenter managers focus on total work done in a given period of time (throughput or bandwidth). CPU execution time only includes the time the CPU spend computing. System CPU time also includes the time spent on Input/Output and other operating system tasks. A clock cycle is one clock period of the processor clock. `CPU time = Instruction count * Clock Cycles per Instruction * Clock Cycle Time`. 


### 1.7 The Power Wall
For most integrated circuits the primary source of energy consumption is switching transistors states. Computer designers are facing a power wall.


### 1.8 The Sea Change: The Switch from Uniprocessors to Multiprocessors
To move past power wall, designers now implement multiprocessors more heavily. A "quadcore" microprocessor has four processors. It's harder to write parallel programs because applications have to be divided into roughly equal parts, and communications between parts have to be limited.


### 1.9 Real Stuff: Benchmarking the Intel Core i7
*Benchmarks* are programs written to specifically test performance.


### 1.10 Fallacies and Pitfalls
Improving performance of one part by a certain rate, doesn't increase the performance of the whole system by the same rate. 




## 2 Instructions: Language of the Computer


### 2.1 Introduction
*Instruction set* is the vocabulary of commands for an architecture. Instruction sets of computers are generally quite similar, they all have the goal of maximizing speed and clarity.


### 2.2 Operations of the Computer Hardware
MIPS assembly language notation for addition is `add a, b, c`, which adds b+c and stores the sum in a. Instructions require a set amount of operands (addition has 3). This hard set rule increases the simplicity. Design Principle 1: Simplicity favors regularity.
```
# In C: 
f = (g + h) - (i + j);

# In MIPS: 
add t0, g, h
add t1, i, j
sub f, t0, t1
```


### 2.3 Operands of the Computer Hardware
*Registers* are primitives built directly into hardware. Size of a register is 32 bits in MIPS. Design Principle 2: Smaller is faster. MIPS convention is to use dollar sign followed by two characters for register names `$s0, $s1, ...`; for temporary registers `$t0, $t1, ...`.

More complex data structures like arrays and structures are stored in memory. To access a register in memory, a *memory address* must be supplied. Load is used to copy a value from memory into a register. `lw $t0,8($s3)` loads A[8] into $t0 if $s3 is the base address of array a.

In MIPS, registers must start at addresses that are multiples of 4. This is called an *alignment restriction*. In order to access the 8th element in array a, the offset must be 4 x 8, or 32. `store` is the complement of load. `sw $t0,48($s3)` stores $t0 into A[12]. 


### 2.4 Signed and Unsigned Numbers
In computer number kept in high/low signals, hence binary. In any number base value of its digit d is:
```
d * Base^i
1011_two = 2^3 + 0 + 2^1 + 1 = 11_ten
```

Overflow occurs when result of operations can't be shown by the rightmost hardware bits available. To represent negative numbers, standard is to use two's complement representation or signed binary numbers. Leading 1 is negative number, leading 0 is a positive number. Overflow occurs on signed numbers when the most signifcant left bit is different from the infinite number of digits to the left of this bit. A fast way to invert a 2's complement number is to flip all bits and then add 1. 
```
Inverting number: 0000 1100 -> 1111 0100    
Adding more Bits: -12_ten = 1111 0100 -> 1111 1111 1111 0011   
```


### 2.5 Representing Instructions in the Computer
In MIPS, registers `$s0` to `$s7` map onto register 16-23 and registers `$t0` to `$t7` map onto registers 8-15. 
```
add $t0,$s1,$s2
# Decimal Representation:
0 17 18 8 0 32
# Binary:
000000 10001 10010 01000 00000 100000
# MIPS Fields:
op-opcode rs-firstregister rt-secondregister rd-destinationregister shamt-shiftamount funct-function
```

Hexadecimal numbers are a more compact way of displaying machine instructions. Design Principle 3: Good design demands good compromises.


### 2.6 Logical Operations
Logical operators on fields of bits:

Logical operation | C operators | Java operator | MIPS
--- | --- | --- | ---
Shift left | \<\< | \<\< |sll
Shift right | \>\> | \>\>\> | srl
Bit AND | & | & | and, andi
Bit OR | \| | \| | or, ori
Bit NOT | ~ | ~ | nor

AND is used to force 0's where there is a 0 in a bit pattern. This is also called a *mask*.


### 2.7 Instructions for Making Decisions
MIPS has two decision making structures similar to if and go to. *Branch if equal* `beq register1, register2, L1` goes to the instruction L1 if the value of register1 equals register2. *Branch if not equal* `bne register1, register2, L1` goes to instruction L1 if value of register1 does not equal register2. 
```
# f,g,h,i,j in register $s0 to $s4
# if (i == j) f = g + h; else f = g - h;

bne $s3, $s4, Else    # go to else if i != j
add $s0, $s1, $s2    # f = g + h
j Exit            # Go to exit
Else: sub $s0, $s1, $s2    # f = g - h
Exit:
```

A *basic block* is a sequence of instructions without branches or branch targets. To check if a value is less than another value use `slt $t0, $s3, $s4`. $t0 is set to 1 if $s3 is less than $s4. Jump register instruction `jr` is a unconditional jump to a register.


### 2.8 Supporting Procedures in Computer Hardware
*Procedures* or *functions* are ways to abstract in software. Only parameters have to be provided, while the function does the rest. In MIPS software there is registers for functions:
- `$a0-$a3`: argument registers for parameters
- `$v0-$v1`: value register to return values
- `$ra`: return address register to return back to origin

Jump-and-links `jal` instruction stores the return address in register 31 `$ra`. The return address is the program counter + 4. *Program counter* is the register that holds the address of the current instruction being executed. *Stack* acts as a last-in-first-out queue. In a function call there are register inside and oustide of scope: 
Preserved | Not preserved
=== | ===
Saved registers: `$s0-$s7` | Temporary registers: `$t0-$t9`
Stack pointer register: `$sp` | Argument registers: `$a0-$a3`
Return address register: `$ra` | Return value registers: `$v0-$v1`
Stack above the stack pointer | Stack below the stack pointer

The *procedure frame* is the segment of the stack containing the procedure's registers and local variables. Layout of memory allocations:
- Stack (Functions, recursive calls...)
- Dynamic Data / Heap (For example `malloc()` in C)
- Static Data (Static variables and constants)
- Text (Machine language code for source file)
- Reserved


### 2.9 Communicating with People
Most computers use 8-bit bytes following the ASCII standard to represent characters. Load byte `lb` loads a byte from memory `lb $t0,0($sp)`. Store byte `sb` takes a byte and writes it to memory `sb $t0,0($gp)`.


### 2.10 MIPS Addressing for 32-bit Immediates and Addresses
Load upper immediate `lui` allows constants that are larger than 16 bits to be loaded.
```
# Store 0000 0000 0011 1101 0000 1001 0000 0000 in $s0:
lui $s0,61        # load upper 16 bits
ori $s0,$s0,2304    #insert lower 16 bits
```

Program Counter relative addressing uses the program counter as the register to be added to the address. There are multiple forms of addressing modes in MIPS:

1. Immediate addressing: operand is constant
2. Register addressing: operand is register
3. Base addressing: operand is memory location with address is (register + constant)
4. PC-relative addressing: operand is memory location with address (program counter + constant)
5. Pseudodirect addressing: jump address is 26 bits of instructions concatenated with upper bits of program counter.


### 2.11 Parallelism and Instructions: Synchronization
When instructions aren't synchronized, there is a danger of a data race when occuring. The results of the program could change depending on the order in which the events occur. Use lock to prevent instruction overlapping to occur. Load linked `ll` and store conditional `sc` are used in sequence to perform an atomic exchange on a memory locations.


### 2.12 Translating and Starting a Program
There are four steps in transforming a C program in such a way that the computer can run it:
1. Compiler - Transform C program into assembly language program.
2. Assembler - Turn assembly language program into an object file (machine language instructions, data and information on where to store in memory).
3. Linker - Combines independent machine language programs into a signle executable file (places in memory, determines address of data and instructions & patches internal and external references). Creates an executable file.
4. Loader - Read executable file to memory and start.

*Dynamically linked libraries* try to save storage by not loadding routines that are linked until the program is executing.




## 4 - The Processor

### 4.1 Introduction
For every instruction the first two steps are the same:
1. Fetch instruction from memory based on program counter
2. Read one or two registers depending on the instruction


### 4.2 Logic Design Conventions
Combinational elements always output the same values given the same inputs-- there is no storage. State elements store information about what state the circuit is in. Edge-triggered clocking means that the next states are determined by sampling the inputs and states right as the clock shifts from 0->1 or 1->0. 


### 4.3 Building a Datapath
Register file is a collection of registers that can be read or written to. The one used for our purposes has two read ports and one write port. Branching datapaths have to compute the location of the branch, and decide whether to jump to the branch, or simply follow the normal program counter increment interval. All datapaths must execute within one clockcycle. 


### 4.4 A Simple Implementation Scheme
MIPS ALU can perform AND, OR , add, subtract, sle and NOR based on the ALU control lines. For l-type instruction, the ALU calculates the address by addition. For r-type instructions, the ALU performs the desired operation based on the funct field. The ALU opcode decides whether to take the funct field into account. All the control lines in the complete datapath are controlled by the opcode fields of the instruction (bits 31-26).


### 4.5 An Overview of Pipelining
Pipelining is the term for overlapping multiple instructions. In pipelining, any given step isn't faster. Time is only saved because the steps occur in parallel. The total throughput is improved, the execution time or latency is unchanged. Due to start-up and run-down times, the more cycles a pipelined programs goes through, the closer the time savings are to the number of steps pipelined. 

There are multiple errors that can occur when pipelining. Structural hazards occur when the hardware cannot execute the instructions given within the same clock cycle. Data hazards occur when the pipeline has to stop in order for a different step to finish (if we have to reuse a value from a directly previous cycle, then we have to wait a whole clock cycle to reuse that value). To avoid this issue, forwarding can be used to retrieve the data early. Control hazards occur when the instruction fetched is not the one that is needed.


### 4.6 Pipelined Datapath and Control
An instruction in MIPS has at most five stages: 1. IF, instruction fetch; 2. ID, instruction decode and register file read; 3. EX, execution or address calulcation; 4. MEM, data memory access; 5. WB, write back.


### 4.7 Data Hazards: Forwarding versus Stalling
Forwarding hardware has to be designed specific to the circuit. Similarly hazard detection unit's can be used to check if an instruction could cause a hazard. If such a case is detected, then the other pipelined processes are stalled for some amount of time. Nops are instructions that have no effects. 


### 4.8 Control Hazards



## Appendix A: Assemblers, Linkers and the SPIM Simulator

### A.1 Introduction
Assembly language makes machine language more readable because it uses symbols instead of bits. The programmer can define a *macro* to extend the assembly language. Advancements in hardware have made assembly programming less common. It is still useful for applications where size and speed have to be maximized. Disadvantages of assembly programming are: 1. Specific to machine, must be rewritten for different computer architecture; 2. Programs are longer; 3. Common programming idioms have to be built with combinations of more basic instructions.


### A.2 Assemblers
Assembler translates assembly language file into a binary machine instruction file. The assembler knows only the address of local labels. Linker combines a collection of object files. Labels can be used before they are invoked. Hence the assembler finds all labels first, and then produces instructions. Assembler parses each line for *lexemes*. Lexemes are individual words, numbers or punctuation. `ble $t0, 100, loop` contains six lexemes: `ble`, `$t0`, `,`, `100`, `,`, `loop`. Labels and their addresses are recorded in a symbol table.

*Macro* can be used to name a frequently used sequence of instructions. Any name call of the macro will search for a pattern and replace it with the macro's definition. *Pseudoinstructions* are instructions that are only in the assembler, not in the hardware. Pseudoinstructions make it easier to write code, as the hardware can expand the single instruction into multiple hardware instructions. 


### A.3 Linkers
Separate compilation allows the program to be split into multiple files. The *linker* merges these additional files by: searching the program for library routines, determining memory location and resolving references among files. 


### A.4 Loading
If linked without error, the operating system can start the program by:
1. Reading file header: determine size of text and data
2. Creating address space
3. Copying instruction & data from executable file into address space
4. Copying arguments of program onto stack
5. Initializing machine registers
6. Jumping to a start-up routine



## Appendix B: The Basics of Logic Design

### B.1 Introduction


### B.2 Gates, Truth Tables, and Logic Equations
Logic blocks without memory are called *combinational*; those with memory are called *sequential*. In Boolean Algebra a OR operator is written as `+`; AND as `*` and NOT as `'`. Not A or B and C would be `A' + B*C`. NOR and NAND gates are universal, because any logic function can be built from these gates.


### B.3 Combinational Logic
Decoder has n inputs, and 2<sup>n</sup> outputs:

x | y | Out3 | Out2 | Out1 | Out0
--- | --- | --- | --- | --- | ---
0 | 0 | 0 | 0 | 0 | 1
0 | 1 | 0 | 0 | 1 | 0
1 | 0 | 0 | 1 | 0 | 0
1 | 1 | 1 | 0 | 0 | 0

A multiplexer uses a selector value to select one of the inputs to output. For n data inputs, there has to be log<sub>2</sub>n selector inputs.

Sel | x | y | Out
--- | --- | --- | ---
0 | 0 | 0 | 0
0 | 0 | 1 | 0
0 | 1 | 0 | 1
0 | 1 | 1 | 1
1 | 0 | 0 | 0
1 | 0 | 1 | 1
1 | 1 | 0 | 0
1 | 1 | 1 | 1

Sum of products, and products of sums are two-level representations of combinational logic. Read-only memory (ROM) can be used to hardwire a set of logic functions. Inputs and Outputs that don't necessarily have to be true or false, are labelled as *don't cares*. Don't cares can be used to simplify the logic function. A *bus* is a collection of data inputs that is treated as one logical signal. 


### B.4 Using a Hardware Description Language
Hardware description language is a tool used for simulating and generating hardware designs. Verilog and VHDL are the most common languages. Two primary datatypes in Verilog: *wire* specifies a combinational signal, *reg* holds a value. For a wire that is 32-bits wide write `wire [31:0]` or generally `[starting bit:ending bit]`. To access a single value in an array write `wire [NUMBER]`. Registers and wires can be either 0, 1, or X (unset). Constant values are prefixed to state whether they are decimal or binary: `4'b0100` and `4'd4` both are 4-bit binary constant with value 4. `- 8 'h4` is a 8-bit constant with value -4. Values can be concatenated with `{ }`. `{16{2'b01}}` creates 32-bit value with pattern 01..01. `{A[31:16],B[15:0]}` creates 32-bit value with upper from A and lower from B. All unary, binary, arithmetic, logic, comparison and shift operators are the same as in C. 

Verilog program is split into modules consisting of:
- initial constructs: initialize reg variables
- continous assignments: define combinational logic
- always constructs: define sequential or combinational logic
- other modules

Key word `assign` continously assigns a value to it's output. An `always` block is constantly reevaluated:
```
always @(list of signals causing reevaluation) begin
Verilog statements
control statements end
```

Blocking assignment uses the assignment operator `=`. Nonblocking assignment uses symbol `<=`. Nonblocking assignments evaluate all right-hand sides of assignments in an always block are evaluated and assigned simultaneously. 


### B.5 Constructing a Basic Arithmetic Logic Unit
The Arithmetic Logic Unit performs all arithmetic and logic operations in a computer. Addition function is built by connecting multiple single bit full adders that have 3 inputs (a, b and carryin) and produce 2 outputs (sum, carryout). DeMorgan's Theorem: `(a + b)'` = `a' * b'`. 


### B.6 Faster Addition: Carry Lookahead
Carryin of higher bits can be calculated ahead of time using generate and propagate:
```
gi = ai * bi
pi = ai + bi

c1 = g0 + (p0 * c0)
c2 = g1 + (p1 * g0) + (p1 * p0 * c0)
c3 = g2 + (p2 * g1) + (p2 * p1 * g0) + (p2 * p1 * p0 * c0)
```



[^book]: Patterson, D. A., & Hennessy, J. L. (2013). Computer Organization and Design MIPS Edition: The Hardware/Software Interface. Newnes.
