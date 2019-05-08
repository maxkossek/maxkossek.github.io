---
layout: post
title: Computer Organization and Design (Patterson & Hennessy)
date: 2019-05-07
categories: [book]
tags: [computer science]
author: "Max Kossek"
description: Book Notes for the 5th edition of the book Computer Organization and Design by Patterson & Hennessy
---


## 2 Instructions: Language of the Computer


### 2.1 Introduction
*Instruction set* is the vocabulary of commands for an architecture.[^book] Languages of computers are generally quite similar, they all have the goal of maximizing speed and clarity.


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






[^book]: Patterson, D. A., & Hennessy, J. L. (2013). Computer Organization and Design MIPS Edition: The Hardware/Software Interface. Newnes.
