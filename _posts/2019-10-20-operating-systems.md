---
layout: post
title: "Operating Systems: Three easy pieces (Arpaci-Dusseau)"
date: 2019-10-20
categories: [book]
tags: [computer science]
author: "Max Kossek"
description: "Book Notes for the book Operating Systems: Three easy pieces by Arpaci-Dusseau"
sitemap:
    lastmod: 2019-11-06
---

### 1 A Dialogue on the Book


### 2 Introduction to Operating Systems
During a program, the processor fetches instructions from memory, and then decodes and executes them.[^book] After each instruction the program counter is incremented. Operating systems are "resource managers", allowing programs to share memory, interact with devices and more. The OS takes a physical resource, and virtualizes it by transforming it into a more general virtual form of itself. Virtualization gives concurrent programs access to devices and their own instructions and data. OS Policies determine the priorities and access rights given to concurrent programs. 

Memory is just an array of bytes occupying an address. Both data and instructions are stored in memory. The OS virtualizes memory by giving each process access to it's own private address space. Long lived information is held by hard and solid-state drives. The file system is software that manages persistent data stored in the disk. Files are often shared across different processes. System calls route to the file system, which handles the request and possibly returns an error code. Most file systems use write protocols, such as journaling or copy-on-write, to be able to restore the system to a reasonable state after a failure occurs. The goals of an operating system are:
1. High performance, by reducing instruction time and memory space requirements.
2. Protection, by isolating processes from each other.
3. Reliability, by maximizing up-time and reducing failure cost.
4. Other goals: energy-efficiency, security, mobility.



## Part I: Virtualization

### 3 A Dialogue on Virtualization


### 4 The Abstraction: The Process
A process is a running program. With time sharing, the user can run many concurrent processes on a single CPU, because the operating system stops and runs processes as needed. Both instructions and data are stored in memory. The program counter keeps track of which instruction is currently being executed.

To start a process, the OS moves the executable format of a program from disk to main memory. In C programs, the stack holds local variables, function parameters and return addresses. The heap is stores explicitly requested dynamically-allocated data such as by calling `malloc()`. Heap memory space has to be explicitly freed by calling `free()`. In Unix, each process has three open file descriptors: standard input, output and error. The `main()` function is the entry point to start a program.

Processes can be in one of three states: running, ready or blocked. Each process can be described by its machine state: contents in memory, register values and information about I/O. Process lists track the state of each process. The register context holds the register contents of a stopped process in memory.


### 5 Interlude: Process API
Processes have a process identifier or PID (e.g. 27048). In Unix systems, the `fork()` system call creates a new child process from the parent process. Fork creates a copy of the same program. `exec()` transforms the currently running program into a different program. The `wait()` system call halts the parent process until the child process finishes. 

The shell is a user program, that displays a prompt, reads in input, calls `fork()` to create a new child process, calls `exec()` to run the command, and then calls `wait()` until the command completes. Command `pipe()` connects the output of one process to an in-kernel pipe (queue). The output of one process is used as input for another process. System call `kill()` sends signals to a process, including directives to pause, die or abort. Keystroke ctrl-c sends a `SIGINT` (interrupt) signal to a process; ctrl-z sends a `SIGTSTP` (stop) signal. Normal users can only control their own processes. The superuser (or root) can control all processes.


### 6 Mechanism: Limited Direct Execution
In the direct execution protocol, the OS allocates memory, loads a program into memory, sets up the stack, clears registers and then hands-off control to the program's main function. After the program returns, the OS frees up memory and removes the program from the process list.

It is important to restrict the operations that a program can perform. Processor modes (such as user mode), are used to restrict what code can do. Kernel mode has no restrictions on operations. With system calls, the kernel can grant programs access to certain functionalities. During a system call, a trap instruction jumps into the kernel and raises the privilege level to kernel mode. After privileged operations are completed (if allowed), the OS calls a return-from-trap instruction. 

At boot time, the kernel sets up a trap table which tells the hardware what code to run during exceptions and what addresses a trap can jump to. The hardware remembers these instructions (also called trap handlers) until the machine is next rebooted. User code cannot directly jump to a specific address; instead, a system-call number is assigned to each system call to introduce a level of indirection.

In a cooperative scheduling system, the OS waits for the program to issue a system call to regain back control. This is less than ideal, since programs can get stuck in infinite loops or behave maliciously. Hardware timer devices periodically call an interrupt handler to halt a process. If the OS decides to switch programs, a context switch is executed to save the register values of the current process and restore the register of the upcoming process from the kernel stack. Reboots are useful because they reset the system to a more tested state, reclaiming stale or leaked memory resources.


### 7 Scheduling: Introduction
The turnaround time of a job is the time of completion minus the time of arrival (Tturn = Tcomplete - Tarrive). Most modern schedulers are preemptive, performing context switches to stop one process in order to run another. The response time of a job is the time of arrival subtracted from the time the job was first scheduled (Tresponse = Tfirstrun - Tarrival).

First In, First Out (FIFO) is the most basic algorithm for scheduling. FIFO has a problem with the "convoy effect", wherein a big process slows down the turnaround time of all subsequent smaller processes. Shortest Job First (SJF) runs the shortest job first, then the next shortest, and so on. SJF still has the convoy effect problem if jobs arrive at different time points. Shortest Time-to-Completion First (STCF) schedules the job with the least time left first.

Round-Robin (RR) scheduling switches between jobs, running each for a time slice instead of to completion. The shorter the time slice, the better RR performs under the response time metric. If the time slice is too short, then the costs of context switching outweigh the benefits of reduced response time. RR is one of the worst policies for the turnaround time metric. There's a trade-off between turnaround and response time.


### 8 Scheduling: The Multi-Level Feedback Queue
Multi-level Feedback Queues (MLFQ) learn from the past to predict future behavior. MLFQ's might have a number of distinct queues, each assigned a different priority level. This algorithm assumes a job might be short, if it is then the job will run to completion. If not, then the algorithm slowly moves the job down the priority queue. John Outsterhout's Law: Default parameter values are often left unmodified, hence we must hope that they work well in the field (avoid voodoo constants). Rules for MLFQ:
1. If Priority(A) > Priority(B), then run A.
2. If Priority(A) = Priority(B), then run A&B in Round Robin.
3. When a job enters the system it is placed at the highest priority.
4. Once a job uses up its time allotment at a given level, its priority is reduced.
5. After some time period S, move all the jobs in the system to the topmost queue (to avoid starvation).


### 9 Scheduling: Proportional Share
Proportional-share or fair-share schedulers try to guarantee that each job obtains a certain percentage of CPU time. In lottery scheduling, tickets are used to represent the share of a resource that a process should receive. The next process is determined probabilistically by picking a winning ticket. Processes and their ticket amounts can be stored in a sorted linked list. Random approaches have several advantages: (1) avoidance of worst-cases; (2) lightweight and little memory use due to lack of state; (3) fast computation; (4) converge to expected outcomes over time.

Stride scheduling also makes use of tickets, but in a deterministic manner. Each job's stride is inverse to the proportion of it's number of tickets. Every time a process runs we increment it's pass value by it's stride. The scheduler runs the program with the lowest pass value at each cycle.

Completely fair schedulers (CFS) try to evenly divide CPU time among all processes. Each time a process runs, it accumulates virtual run time (vruntime). To decide the next process, the scheduler picks the process with the lowest vruntime. CFS uses Red-Black Trees rather than lists to reduce access time.


### 10 Multiprocessor Scheduling (Advanced)
Multiprocessors allow applications to run in parallel threads. Multi-threaded applications spread work across multiple CPUs and hence run faster. Processors use caches to store frequently accessed data more quickly. Caches are based on the notion of temporal locality (recently used data is likely to be accessed again) and spatial locality (surrounding memory addresses are likely to be accessed). Synchronization or "cache coherence" makes caching with multiple processors much more difficult. Bus snooping is used to monitor the value of a data item in main memory. If the item in main memory changes, then the cache copy is invalidated or updated.

Locking data structures use a mutex to ensure that an item can't be simultaneously be modified by multiple processes. Simple scheduling mechanisms don't make efficient use of cache affinity (the state of caches accumulated over time). Multi-queue multiprocessor scheduling (MQMS) uses multiple queues, such as one for each processor. Multi-queues make use of cache affinity and reduce lock contention, but have a risk of load imbalance. 

Migration techniques are used to solve load imbalances. With a work stealing approach, a process occasionally looks at another targets queue to see how full it is. If the target queue is more full than the source queue, then it will migrate one or more jobs. There is trade-off between high overhead and the intended goal of fixing load imbalance.


### 11 Summary Dialogue on CPU Virtualization


### 12 A Dialogue on Memory Virtualization
Every address generated by a user program is a virtual address. Virtualization provides ease of use, isolation and protection of memory addresses.


### 13 The Abstraction: Address Spaces
In time sharing, the OS keeps processes in memory while it switches between them. The address space of a process consists of three main parts: program code, the heap and the stack. Program code memory holds program instructions, static data and global variables. The heap contains dynamic data structures such as assigned by `malloc()`. Stack memory contains local variables, arguments to routines and return values of functions. The stack and the heap grow in opposite directions into a space of free memory addresses between them.

By virtualizing memory, the OS can assign certain memory addresses (such as 0) and give the illusion of a very large address space to every process. Memory isolation ensures that a process can't affect the OS or another process.


### 14 Interlude: Memory API
In C, stack memory is allocated and deallocated by the compiler, heap memory is allocated and deallocated by the programmer. `malloc()` takes the amount of bytes to be allocated as input and returns a void pointer. `sizeof()` determines the amount of bytes needed for a given data type or structure. The function `free()` is used to free heap memory; it takes in a pointer returned by `malloc()` as an argument. C does not automatically garbage collect heap memory. Heap memory has to be explicitly freed to avoid memory leaks.


### 15 Mechanism: Address Translation
Address translation means that the hardware transforms each memory address from it's virtual address to a physical address: `physical address = virtual address + base`. Base registers are used to transform virtual addresses into physical registers, and a bounds (or limit) register ensures that memory addresses are confined to the address space. The OS gives an illusion of independent memory, when in reality memory is shared between many programs as the CPU switches between running programs. In terms of program memory, the OS must: find space for the program in address memory, reclaim memory once the program stops, preserve state if a context switch occurs and provide exception handlers.


### 16 Segmentation




[^book]: Arpaci-Dusseau, R. H., & Arpaci-Dusseau, A. C. (2018). Operating systems: Three easy pieces. Arpaci-Dusseau Books LLC.