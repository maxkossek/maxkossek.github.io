---
layout: post
title: Computer Organization and Design (Patterson & Hennessy)
date: 2019-05-07
categories: [book]
tags: [computer science]
author: "Max Kossek"
description: Book Notes for the 5th edition of the book Computer Organization and Design by Patterson & Hennessy
sitemap:
    lastmod: 2019-10-22
---


## 1 Computer Abstractions and Technology

### 1.1 Introduction
Personal computers deliver good performance to single users at low cost.[^book] Servers carry larger workloads and have greater storage, input, and output capacity. Embedded computers are the largest segment of computers used in a variety of applications (car, tv, airplane etc.). Computers are designed towards their applications, some focus on dependability, while other focus on speed or other factors.


### 1.2 Eight Great Ideas in Computer Architecture
1. Moore's Law (Integrated circuit resources double every 18-24 months)
2. Abstraction
3. Making the common case fast
4. Parallelism
5. Pipelining
6. Prediction
7. Hierarchy of Memories
8. Dependability via Redundancy


### 1.3 Below Your Program
Operating systems create the interface between the program and the hardware. Compilers translate programs written in a higher-level language into hardware instructions. Assemblers translate symbolic versions of instructions into binary code.


### 1.4 Under the Covers
Five classic components of a computer: input, output, memory, datapath and control. Datapath and control combine to form the processor. Processors execute the instructions of the program, do arithmetic and logic operations and signal states.


### 1.5 Technologies for Building Processors and Memory
A transistor is a simple electrical on/off switch. Integrated circuits combine multiple transistors. Silicon, a semiconductor (conducts electricity moderately well) is the basis of most chips.


### 1.6 Performance
Individual users are usually more focused on the execution time of a task. Datacenter managers focus on total work done in a given period of time (throughput or bandwidth). CPU execution time only includes the time the CPU spent computing. System CPU time also includes the time spent on input / output and other operating system tasks. A clock cycle is one clock period of the processor clock. `CPU time = Instruction count * Clock Cycles per Instruction * Clock Cycle Time`. 


### 1.7 The Power Wall
For most integrated circuits the primary source of energy consumption is switching transistors states. Computer designers are facing a power wall.


### 1.8 The Sea Change: The Switch from Uniprocessors to Multiprocessors
To move past the power wall, designers now implement multiprocessors more frequently. A "quadcore" microprocessor has four processors. It's harder to write parallel programs because applications have to be divided into roughly equal parts, and communication between parts has to be limited.


### 1.9 Real Stuff: Benchmarking the Intel Core i7
Benchmarks are programs written to specifically test performance.


### 1.10 Fallacies and Pitfalls
Improving performance of one part by a certain rate, doesn't increase the performance of the whole system by the same rate. 



## 2 Instructions: Language of the Computer

### 2.1 Introduction
Instruction sets are the vocabularies of commands for an architecture. Instruction sets of computers are generally quite similar; they all have the goal of maximizing speed and clarity.


### 2.2 Operations of the Computer Hardware
MIPS assembly language notation for addition is `add a, b, c`, which adds b+c and stores the sum in a. Instructions require a set amount of operands (addition has 3). This hard set rule increases simplicity. Design Principle 1: Simplicity favors regularity.
```
# In C: 
f = (g + h) - (i + j);

# In MIPS: 
add t0, g, h
add t1, i, j
sub f, t0, t1
```

 
### 2.3 Operands of the Computer Hardware
Registers are primitives built directly into hardware. The size of a register is 32 bits in MIPS. Design Principle 2: Smaller is faster. MIPS convention is to use a dollar sign followed by two characters for register names `$s0, $s1, ...`;  temporary registers are `$t0, $t1, ...`.

Complex data structures like arrays and structures are stored in memory. A memory address must be supplied to access a register in memory. Load is used to copy a value from memory into a register. `lw $t0,8($s3)` loads A[8] into $t0 if $s3 is the base address of array a.

In MIPS, registers must start at addresses that are multiples of 4. This is called an alignment restriction. In order to access the 8th element in array a, the offset must be 4 x 8, or 32. Store is the complement of load. `sw $t0,48($s3)` stores $t0 into A[12]. 


### 2.4 Signed and Unsigned Numbers
In computers, numbers are stored using high/low signals, hence the use of binary. Overflow occurs when the result of an operation can't be shown by the rightmost hardware bits available. To represent negative numbers, the standard is to use two's complement representation or signed binary numbers. A leading 1 signifies a negative number; a leading 0 a positive number. Overflow occurs on signed numbers when the most significant left bit is different from the infinite number of digits to the left of it. A fast way to invert a 2's complement number is to flip all bits and then add 1. 
```
Inverting a number: 0000 1100 -> 1111 0100    
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
[op-opcode] [rs-first register] [rt-second register] [rd-destination register] [shamt-shift amount] [funct-function]
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

AND is used to force 0's where there is a 0 in a bit pattern. This is also called a mask.


### 2.7 Instructions for Making Decisions
MIPS has two decision making structures similar to `if` and `go to`. Branch if equal `beq register1, register2, L1` goes to the instruction L1 if the value of register1 equals register2. Branch if not equal `bne register1, register2, L1` goes to instruction L1 if the value of register1 does not equal register2. 
```
# f,g,h,i,j in register $s0 to $s4
# if (i == j) f = g + h; else f = g - h;

bne $s3, $s4, Else	# go to else if i != j
add $s0, $s1, $s2	# f = g + h
j Exit			# Go to exit
Else: sub $s0, $s1, $s2	# f = g - h
Exit:
```

A basic block is a sequence of instructions without branches or branch targets. To check if a value is less than another value use `slt $t0, $s3, $s4`. $t0 is set to 1 if $s3 is less than $s4. Jump register instruction `jr` is a unconditional jump to a register.


### 2.8 Supporting Procedures in Computer Hardware
Procedures or functions are ways to abstract in software. Only parameters have to be provided, while the function does the rest. In MIPS software there are registers for functions:
- `$a0-$a3`: argument registers for parameters
- `$v0-$v1`: value registers for return values
- `$ra`: return address register

The jump-and-link `jal` instruction stores the return address in register 31 `$ra`. The return address is the program counter + 4. The program counter holds the address of the current instruction being executed. Stacks act as a last-in-first-out queue. In a function call there are registers inside and outside of scope:

Preserved | Not preserved
--- | ---
Saved registers: `$s0-$s7` | Temporary registers: `$t0-$t9`
Stack pointer register: `$sp` | Argument registers: `$a0-$a3`
Return address register: `$ra` | Return value registers: `$v0-$v1`
Stack above the stack pointer | Stack below the stack pointer

The procedure frame is the segment of the stack containing the procedure's registers and local variables. Layout of memory allocations:
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
lui $s0,61		# load upper 16 bits
ori $s0,$s0,2304	#insert lower 16 bits
```

There are multiple forms of addressing in MIPS:
1. Immediate addressing: operand is a constant
2. Register addressing: operand is a register
3. Base addressing: operand is a memory location where the address = register + constant
4. PC-relative addressing: operand is a memory location with address = program counter + constant
5. Pseudodirect addressing: the jump address is the right 26 bits of the instruction concatenated with the upper bits of the program counter


### 2.11 Parallelism and Instructions: Synchronization
When instructions aren't synchronized, there is danger of a data race. The results of the program could change depending on the order in which the events occur. Use lock to prevent instruction overlap from occurring. Load linked `ll` and store conditional `sc` are used in sequence to perform an atomic exchange on a memory location.


### 2.12 Translating and Starting a Program
There are four steps in transforming a C program in such a way that the computer can run it:
1. Compiler - Transforms the C program into assembly language program.
2. Assembler - Turns the assembly language program into an object file (machine language instructions, data and information on where to store in memory).
3. Linker - Combines independent machine language programs into a single executable file (places in memory, determines address of data and instructions & patches internal and external references). Creates an executable file.
4. Loader - Reads the executable file to memory and starts the program.

Dynamically linked libraries try to save storage by not loading linked routines until the program executes.



## 3 - Arithmetic for Computers

### 3.5 Floating Point
Scientific notation writes numbers with a single number left of the decimal point. Binary numbers can also be written in scientific notation. Numbers such as this are called floating point, because the binary point is not fixed.

When writing numbers in floating-point, there is a trade-off between the size of the fraction (mantissa) and the size of the exponent. The number of bits are limited, allocating more bits to the fraction increases accuracy, while allocating more to the exponent increases the capacity or size that can be represented. The general form of floating-point numbers is:

(-1)<sup>S</sup> * F * 2<sup>E</sup>   
F = value of fraction field; E = value in exponent field; S = sign bit

Overflow occurs, when the number is too large to be represented by the number of bits allocated to the exponent. Underflow occurs when the number is too small to be represented by the exponent. Programming languages such as C also offer a double data type which increases the amount of accuracy by representing the floating-point value with two 32-bit words. Some standards use biasing to make sorting easier. By adding a bias, negative exponents are made smaller than positive exponents. The IEEE 754 standard floating-point representation is:

(-1)<sup>S</sup> * (1 + Fraction) * 2<sup>(Exponent - Bias)</sup>

Adding numbers in scientific notation: 9.999<sub>ten</sub> * 10<sup>1</sup> + 1.610<sub>ten</sub> * 10<sup>\-1</sup>:

Operation | Example
--- | ---
Align decimal point to match the bigger decimal point |1.610<sub>ten</sub> * 10<sup>-1</sup> -> 0.016<sub>ten</sub> * 10<sup>1</sup>
Add significands | 9.999 + 0.016 = 10.015 * 10<sup>1</sup>
Normalize to scientific notation | 1.0015 * 10<sup>2</sup>
Round to initial precision | 1.002 * 10<sup>2</sup>

Floating-point multiplication, for example 1.110<sub>ten</sub> * 10<sup>10</sup> * 9.200<sub>ten</sub> * 10<sup>-5</sup>:

Operation | Example
--- | ---
Calculate exponents by addition | 10 + (-5) = 5
Multiply the significands | 1.110<sub>ten</sub> * 9.200<sub>ten</sub> = 10.212000<sub>ten</sub>
Normalize the product and round | 1.021<sub>ten</sub> * 10<sup>6</sup>
Sign depends on original operands. If both have the same sign, then the result is positive, else negative | +1.021<sub>ten</sub> * 10<sup>6</sup>

MIPS uses separate floating-point registers `$f0,$f1,...` for single precision and double precision. To load and add two single precision values:
```
lwcl 	$f4,c($sp)
lwcl 	$f6,a($sp)
add.s	$f2,$f4,$f6
swcl	$f2,b($sp)
```

IEEE 754 standard for floating-point values always keeps two extra bits on the right during intermediate additions. These bits are called the guard and are used for rounding.
 


## 4 - The Processor

### 4.1 Introduction
For every instruction the first two steps are the same:
1. Fetch an instruction from memory based on the program counter.
2. Read one or two registers depending on the instruction.


### 4.2 Logic Design Conventions
Combinational elements always output the same values given the same inputs-- there is no storage. State elements store information about what state the circuit is in. Edge-triggered clocking means that the next states are determined by sampling the inputs and states right as the clock shifts from 0->1 or 1->0. 


### 4.3 Building a Datapath
A register file is a collection of registers that can be read or written to. Branching datapaths have to compute the location of the branch, and decide whether to jump to the branch, or simply follow the normal program counter interval. All datapaths must execute within one clock-cycle. 


### 4.4 A Simple Implementation Scheme
MIPS ALU can perform AND, OR , add, subtract, sle and NOR based on the ALU control lines. For l-type instruction, the ALU calculates the address by addition. For r-type instructions, the ALU performs the desired operation based on the funct field. The ALU opcode decides whether to take the funct field into account. All the control lines in the complete datapath are controlled by the opcode fields of the instruction (bits 31-26).


### 4.5 An Overview of Pipelining
Pipelining is the term for overlapping multiple instructions. In pipelining, any given step isn't faster. Time is only saved because the steps occur in parallel. The total throughput is improved, while the execution time or latency is unchanged. Due to start-up and run-down times, the more cycles a pipelined programs goes through, the closer the time savings are to the number of steps pipelined. 

There are multiple errors that can occur when pipelining. Structural hazards occur when the hardware cannot execute the instructions given within the same clock cycle. Data hazards occur when the pipeline has to stop in order for a different step to finish (if we have to reuse a value from a directly previous cycle, then we have to wait a whole clock cycle to reuse that value). To avoid this issue, forwarding can be used to retrieve the data early. Control hazards occur when the instruction fetched is not the one that is needed.


### 4.6 Pipelined Datapath and Control
An instruction in MIPS has at most five stages:
1. IF, instruction fetch
2. ID, instruction decode and register file read
3. EX, execution or address calulcation
4. MEM, data memory access
5. WB, write back


### 4.7 Data Hazards: Forwarding versus Stalling
Forwarding hardware has to be designed specific to the circuit. Similarly, hazard detection units can be used to check if an instruction could cause a hazard. If such a case is detected, then the other pipelined processes are stalled for some amount of time. Nops are instructions that have no effects. 


### 4.8 Control Hazards
A control hazard occurs when there is a problem determining the proper instruction to fetch. Branch stalling can be used, but this slows down the pipeline. Another option is to assume that the branch will not be taken, and continue executing instructions. If the branch is taken, then the operations executed are simply discarded or flushed. Moving the branch execution to an ealier stage can reduce the cost of taking the wrong branch. Dynamic branch prediction is used when pipelines are more complex. With this technique, the history of the program and other factors are taken into account to try to predict the likelihood that a branch is taken.


### 4.9 Exceptions
Exceptions are events other than branches or jumps that disrupt normal instruction execution. Arithmetic overflows, using undefined instructions and hardware malfunctions all are exceptions. When an exception occurs, the address of the exception is saved in an exception program counter. Then control of the program is transferred to a different address. Exceptions can cause the program to terminate, or to continue running with some adjustments.


### 4.10 Parallelism via Instructions
Parallelism can be used to further increase the efficiency gains from pipelining. The depth of the pipeline can be increased to overlap more instructions. Multiple-issue is a technique that adds additional components to the computer so that instructions can launch from any pipeline stage. Parallelism uses speculation to decide which instructions can be reordered or combined. There has to be a method to roll back changes, if a speculation causes unwanted behavior. 

Loop unrolling makes multiple copies of a loop to improve the performance of a program. Register renaming uses different register names for some data in order to decrease hazards. This technique is usually used when some ordering reuses a name of a register, but is not data dependendent (antidependence). Dynamic pipelining reorders how the instructions are executed through hardware. Pipelining potentially decreases the energy efficiency, since speculation results in more false predictions and hazards. 




## 5 Large and Fast: Exploiting Memory Hierarchy


### 5.1 Introduction
There are two types of locality:
1. Temporal locality - if an item is referenced, then it will probably be referenced again soon (loops, reusing variables etc.).
2. Spatial locality - if an item is referenced, then other items close to the target's address are more likely to be referenced (program counters, incrementors, pointers etc.).

The bigger the memory, the slower it will be to access any item in that memory. Due to physical constraints, only a small number of locations can be accessed in close proximity.

The hit rate is the fraction of memory accesses found in a higher level of memory. The miss rate is the fraction of items not found in the upper level (miss rate = 1 - hit rate). Hit time is the time it takes to access an element in a higher level in the memory hierarchy. Miss penalty is the time it takes to fetch the block from a lower level in the hierarchy if the block is not found.


### 5.2 Memory Technologies
Types of memory:
1. Dynamic random access memory (DRAM): Moderately fast access, medium cost. Dynamic charges on capacitors that have to be periodically refreshed.
2. Random access memory (SRAM): Fast access, high cost. Integrated circuits.
3. Flash Memory: Slow access, low cost. Electrically erasable, programmable read-only memory.
4. Magnetic Disk: Slowest access, lowest cost. Movable rotating arm with electromagnetic coil to read and write.


### 5.3 The Basics of Caches
Direct mapping of a memory location means that it can only be in one location in the cache (no index is used). A tag gives the address information of a block in the cache. First the index is used to determine which set the word could be in. Then the tag is used to determine if a word in the set matches the word in memory. Increasing the block size decreases the miss rate, by reducing the ratio of tag storage to data storage. When the program starts up, the cache won't have valid information. Valid bits are used to label entries that contain a useful address.

The control unit must fetch the address from memory if there is a miss in the cache:
1. Send original program counter value (PC - 4) to memory.
2. Read from main memory.
3. Write a cache entry with data from the memory read.
4. Restart instruction execution, this time accessing a block from cache.

A write-through scheme simultaneously writes to the cache and main memory. This can be ineffective because it takes a long time to write to main memory. There are two main alternatives to the write-through scheme:
1. Write buffer - Store data in a buffer while it is being written to memory, allowing execution to proceed.
2. Write-back scheme - Only write data to the cache until the block is replaced.
  

### 5.4 Measuring and Improving Cache Performance
Memory-stall clock cycles = (Memory accesses / Program) * Miss Rate * Miss Penalty     
Average Memory Access Time (AMAT) = Time for hit + Miss Rate * Miss Penalty

There are three types of block placements:
- Direct Mapped: all sets with 1 element 
- Set associative: multiple sets with n elements
- Fully Associative: 1 set with all elements

In a set-associative cache, the memory block is in the set: (block number) modulo (number of sets in cache). Increasing the associativity decreases the number of sets and increases the amount of elements in each set. Increasing associativity usually decreases the miss rate (due to more flexibility in allocation to a set), but potentially increases the hit time. Each factor of 2 increase in associativity decreases the index by 1 bit and increases the tag by 1 bit.

In a direct-mapped cache there is only one possible block that can be replaced. With set-associative caches there are different strategies for replacements. Least recently used replacement (LRU), replaces the block that has been unused for the longest time. Many computers use multiple levels of caches. This way the first cache focuses on access time, while the secondary caches focuses on minimizing the miss rate.

There are several algorithms that have been designed to decrease miss rate. Blocked algorithms make use of temporal locality by loading in rows and columns near a block into the cache. It is important to match the block size to the dimensions being accessed.


### 5.5 Dependable Memory Hierarchy
Reliability is a measure of continuous service accomplishment. Mean time to failure (MTTF) or annual failure rate (AFR) are used to measure reliability. A fault is the failure of a component of the system. There are three ways to reduce mean time to failure: fault avoidance, fault tolerance, fault forecasting. Parity bits are used to signal if an error has occurred. When everything is ok, parity bits are set to high to create an even number of high bits in a group.



### 5.8 A Common Framework for Memory Hierarchy

Scheme Name | Number of sets | Block per set
--- | --- | ---
Direct mapped | Number of blocks in cache | 1
Set associative | Number of blocks in cache / Associativity | Associativity (usually 2-16)
Fully associative | 1 | Number of blocks in cache

Increases in associativity decrease the miss rate, but the benefits quickly drop off. Most of the benefit is gained going from fully associative to 2-way associative. With associate cache strategies, there are many potential strategies for block replacement:
- Random: Blocks are randomly replaced
- Last recently used (LRU): Block that has been unused for the longest time is replaced

Misses can be categorized into the "three Cs":
1. Compulsory: Miss because the block has never been in the cache (start-up of program).
2. Capacity: Miss because the cache isn't big enough, and the block had to be replaced.
3. Conflict: Miss because multiple blocks compete for the same set (fully associative caches never have conflict misses).


### 5.10 Parallelism and Memory Hierarchy: Cache Coherence
Cache coherence requires that processors must not read and write values at the same time so as to override changes. Write serialization ensures that all writes to the same location occur in the same order. Many multiprocessors employ migration and replication to increase cache coherence. Migration moves a data item to a local cache for easier access. Replication duplicates a data item to the local cache. Write invalidate protocols invalidate all other copies of a data item when writing to the cache. This gives exclusive access to a single processor as long as it takes to write to the cache.


### 5.16 Concluding Remarks
The principle of locality helps us overcome the long latency of memory access. Cache hierarchies are used to make use of multiple cache optimization techniques. Prefetching brings data into the cache before it is referenced.




## 6 Parallel Processors from Client to Cloud

### 6.1 Introduction
Multiprocessors use multiple smaller processors to create more powerful computers. Task-level parallelism occurs when multiprocessors run independent tasks simultaneously. Parallel processing programs run a single task on multiple processors. Shared Memory Processors (SMPs) are separate processors that share the same memory. 


### 6.2 The Difficulty of Creating Parallel Processing Programs  
The challenges in parallel programming include: scheduling, even work partitioning, synchronization and communication overhead. Strong scaling is the speed-up measured without increasing the size of the problem. Weak scaling measures the speed-up while proportionally increasing the size of the problem. Increasing the load of just a single processor can dramatically reduce the speed-up acquired from using multiple processors. Amdahl's Law: S<sub>latency</sub> = 1 / [(1 - p) + p/s].


### 6.3 SISD, MIMD, SIMD, SPMD, and Vector
SISD processors have a single instruction and data stream. MIMD processors have multiple instruction and data streams. SIMD processors have a single instruction stream, but use multiple data streams. The SIMD design is useful for reducing the cost of storing and reading instructions. Vector instructions have properties that make them more efficient for multi-processing than traditional scalar architectures.


### 6.5 Multicore and Other Shared Memory Multiprocessors
Shared memory multiprocessors use a single physical address space. Uniform memory access (UMA) multiprocessors have the same memory access latency for all processors, while nonuniform memory access (NUMA) multiprocessors have different access times depending on which processor is accessing memory. A lock can be used to synchronize the multiprocessors, by limiting access to a variable for the duration of a single processors access time. OpenMP is an API that allows programmers to model multiprocessors. 


### 6.7 Clusters, Warehouse Scale Computers, and Other Message-Passing
Message passing is when multiple processors explicitly send and receive information to each other. Clusters are computers running in parallel, each running a different copy of the operating system with separate memory.



## Appendix A: Assemblers, Linkers and the SPIM Simulator

### A.1 Introduction
Assembly language makes machine language more readable because it uses symbols instead of bits. The programmer can define a macro to extend the assembly language. Advancements in hardware have made assembly programming less common. It is still useful for applications where size and speed have to be maximized. Disadvantages of assembly programming are:
1. Specific to machine, it must be rewritten for different computer architectures.
2. Programs are longer.
3. Common programming idioms have to be built with combinations of more basic instructions.


### A.2 Assemblers
Assemblers translate assembly language files into binary machine instruction files. The assembler knows only the address of local labels. Linkers combine a collection of object files. Labels can be used before they are invoked. The assembler finds all labels first, and then produces instructions. Assemblers parse each line for lexemes. Lexemes are individual words, numbers or punctuation. `ble $t0, 100, loop` contains six lexemes: `ble`, `$t0`, `,`, `100`, `,`, `loop`. Labels and their addresses are recorded in a symbol table.

Macros can be used to name a frequently used sequence of instructions. Any name call of the macro will search for a pattern and replace it with the macros definition. Pseudoinstructions are instructions that are only in the assembler, not in the hardware. Pseudoinstructions make it easier to write code, as the hardware can expand the single instruction into multiple hardware instructions. 


### A.3 Linkers
Separate compilation allows the program to be split into multiple files. The linker merges these additional files by: searching the program for library routines, determining memory location and resolving references among files. 


### A.4 Loading
If linked without error, the operating system can start the program by:
1. Reading the file header, determining the size of text and data.
2. Creating address space.
3. Copying instructions & data from the executable file into the address space.
4. Copying arguments of the program onto the stack.
5. Initializing machine registers.
6. Jumping to a start-up routine.



## Appendix B: The Basics of Logic Design

### B.2 Gates, Truth Tables, and Logic Equations
Logic blocks without memory are called combinational; those with memory are called sequential. In Boolean Algebra a OR operator is written as "+", AND as "\*" and NOT as "'". Not A or B and C would be `A' + B*C`. NOR and NAND gates are universal, because any logic function can be built from these gates.


### B.3 Combinational Logic
Decoders have n inputs, and 2<sup>n</sup> outputs:

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

Sum of products, and products of sums are two-level representations of combinational logic. Read-only memory (ROM) can be used to hardwire a set of logic functions. Inputs and Outputs that don't necessarily have to be true or false, are labelled as don't cares. Don't cares can be used to simplify the logic function. A bus is a collection of data inputs that is treated as one logical signal. 


### B.4 Using a Hardware Description Language
Hardware description language is a tool used for simulating and generating hardware designs. Verilog and VHDL are the most common languages. Two primary datatypes in Verilog: wire specifies a combinational signal, reg holds a value. For a wire that is 32-bits wide write `wire [31:0]` or generally `[starting bit:ending bit]`. To access a single value in an array write `wire [NUMBER]`. Registers and wires can be either 0, 1, or X (unset). Constant values are prefixed to state whether they are decimal or binary: `4'b0100` and `4'd4` both are 4-bit binary constant with value 4. `- 8 'h4` is a 8-bit constant with value -4. Values can be concatenated with `{ }`. `{16{2'b01}}` creates 32-bit value with pattern 01..01. `{A[31:16],B[15:0]}` creates 32-bit value with upper from A and lower from B. All unary, binary, arithmetic, logic, comparison and shift operators are the same as in C. 

Verilog programs are split into modules consisting of:
- Initial constructs: initialize reg variables
- Continous assignments: define combinational logic
- Always constructs: define sequential or combinational logic
- Other modules

Key word `assign` continuously assigns a value to it's output. An `always` block is constantly reevaluated:
```
always @(list of signals causing reevaluation) begin
	Verilog statements
control statements end
```

Blocking assignment uses the assignment operator `=`. Nonblocking assignment uses the symbol `<=`.


### B.5 Constructing a Basic Arithmetic Logic Unit
The Arithmetic Logic Unit performs all arithmetic and logic operations in a computer. The addition function is built by connecting multiple single bit full adders that have 3 inputs (a, b and carryin) and produce 2 outputs (sum, carryout). DeMorgan's Theorem: `(a + b)'` = `a' * b'`. 


### B.6 Faster Addition: Carry Lookahead
The carryin of higher bits can be calculated ahead of time using generate and propagate:
```
gi = ai * bi
pi = ai + bi

c1 = g0 + (p0 * c0)
c2 = g1 + (p1 * g0) + (p1 * p0 * c0)
c3 = g2 + (p2 * g1) + (p2 * p1 * g0) + (p2 * p1 * p0 * c0)
```


[^book]: Patterson, D. A., & Hennessy, J. L. (2013). Computer Organization and Design MIPS Edition: The Hardware/Software Interface. Newnes.