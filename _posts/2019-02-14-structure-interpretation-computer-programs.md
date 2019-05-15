---
layout: post
title: Structure and Interpretation of Computer Programs (Abelson)
date: 2019-02-14
categories: [book]
tags: [computer science]
author: "Max Kossek"
description: Book Notes for the book Structure and Interpretation of Computer Programs by Harold Abelson
---

<img style="float: right; width: 25%; margin: 0 1rem;" src="/assets/images/book-covers/sicp-cover.jpg" alt="Structure and Interpretation of Computer Programs Book Cover">
Structure and Interpretation of Computer Programs is a introductory computer science textbook. Programs in the book are written in Scheme. However, the focus of the book is on programming concepts, not on the syntax of Scheme. Mathematics and logic play a large part in the examples presented.

<div class="toc">
<strong>Table of Contents:</strong><br><br>
<ul>
<li><a href="#1-building-abstraction-with-procedures">1 Building Abstraction with Procedures</a></li>
<li><a href="#2-building-abstractions-with-data">2 Building Abstractions with Data</a></li>
<li><a href="#3-modularity-objects-and-state">3 Modularity, Objects, and State</a></li>
<li><a href="#4-metalinguistic-abstraction">4 Metalinguistic Abstraction</a></li>
</ul>
</div>

## 1 Building Abstraction with Procedures

### 1.1 The Elements of Programming
Languages have: primitive expressions (simplest entities), means of combination (compound elements built from expression) and means of abstraction (naming and manipulation of compound elements).[^1]  `(+ (* 3 5) (- 10 6))` is an example of prefix notation used in scheme. Interpreter runs in a read-eval-print loop (REPL). A name is a *variable*; a value is *object* `(define size 2)`. Evaluation rule is recursive in nature. Program is in “environment”, a place in memory.

Procedure definitions are abstraction techniques; compound operations are given names and then referred to as units `(define (square x) (* x x))`. Normal-order evaluation completely expands and then reduces. Case analysis is used for conditionals to determine true/false of predicates `(cond ((> x 0) x)`. Scheme has normal primitive predicates (<, =, >) and compound predicates (and, or, not). *Functions* describe things (declarative knowledge); *Procedures* describe how to do things (imperative knowledge).

A user should not need to know how a black box procedure is implemented in order to use it. *Bound variables* bind their formal parameters and are independent of the names chosen for them. Variables that aren’t bound are *free variables*. Nesting of definitions is called block structure, and allows name-packing (reusing names).


### 1.2 Procedures and the Processes They Generate
Recursive process is a chain of deferred operations where the interpreter is required to keep track of operations to be performed later on; the procedure definition refers to the procedure itself. Only memory provides a complete description of the state of a recursive program. Iterative process is a state that can be summarized by a fixed number of state variables together with a rule for how variables should be updates from start to end state). Memory & variables provide a complete description of state of program. 

Order of growth for the resources needed for n elements is denoted by R(n) = Θ(f(n)). Greatest common divisor is the largest integer that divides two integers a and b with no remainder.


### 1.3 Formulating Abstractions with Higher-Order Procedures
Higher-order procedures are procedures that manipulate other procedures. `let` expression is shorthand for an underlying lambda (λ) application, which allows writing procedures without having to name them.


## 2 Building Abstractions with Data

### 2.1 Introduction to Data Abstraction
Data abstraction allows us to structure programs in such a way, that no assumptions about the data are made unless they are strictly necessary for the task. *List-structured* data are data objects constructed from pairs (i.e. numerator and denominator pair for a rational number). Abstraction barriers separate the programs that use data abstraction from programs that implement the data abstraction.


### 2.2 Hierarchical Data and the Closure Property
Closure property occurs when a combination of things results in an operation that itself is combined using the same operations. Closure allows the creation of hierarchical structures. A sequence is an ordered collection of data objects. A sequence of pairs is called a list. Numbering of lists usually begins with 0. `(cons 1 (cons 2 nil)` will print `(1 2)`. `cdr` yields the list without the first element; `car` yields the first element of a list. Map takes a procedure and returns a list of results after applying the procedure to all elements in the list.


### 2.3 Symbolic Data
To identify lists and symbols only the name is written `a`; expressions have to be placed in quotation marks `“a”`. `memq` takes two arguments, a symbol and a list. If the symbol is not contained in the list, then memq returns false. Otherwise, it returns the sublist of the list beginning with the first occurrence of the symbol `(memq 'apple '(x (apple sauce) y apple pear))` returns `(apple pear)`. 

`Element-of-set?` returns predicate of whether element is member of set Θ(n). `Adjoin-set` returns the set that contains elements of the original set and the adjoined element Θ(n). `Union-set` is the set containing the elements that appear in each argument Θ(n2). `Intersection-set` is the set containing only elements that appear in both arguments Θ(n2). 

*Huffman trees* are an algorithm that proposes to encode messages with the fewest bits. Huffman trees are arranged so that the symbols with the lowest frequency appear farthest away from the root.


### 2.4 Multiple Representation of Abstract Data
Data type tags identify which data type is used `imag-part-rectangular` vs `imag-part-polar`. Data-directed programming handles generic operations by explictly dealing with operation-and-type tables.


### 2.5 Systems with Generic Operations
Hierarchy of types state a nesting structure of some form of data (Integers are a subtype of rational numbers; rational numbers are a supertype of integers). In this "tower" the type inherits all operations defined on a supertype. Controlling coercion is a serious problem with hierarchies. Much of the complexity results from relationships among diverse types.


## 3 Modularity, Objects, and State
Modular describes something that can be divided naturally into coherent parts and can be separately developed and maintained.


### 3.1 Assignment and Local State
*State* can be characterized by one or more state variables, that contain enough information about the history of the current state or behavior of the object. As long as we never modify a data object, we can regard compound data objects to be precisely the totality of its pieces. As soon as we introduce change or “states” the compound data object gets an identity that is different from the pieces it is composed of. *Imperative programming* makes extensive use of assignment.


### 3.2 The Environment Model of Evaluation
In the presence of assignment, a variable can no longer be considered to be merely a name for a value. A assigned variable must somehow designate a place in which values can be stored. This place is called the *environment*. *Procedure objects* are created by binding the formal parameters of the procedure to the arguments of the call. This occurs inside a frame. A procedure is simply a lambda expression evaluated in a specific environment and assigned a name. Local procedures do not interfere with the global state and procedures, because they are bound by the frame. Local procedures can access the variables of the enclosing environment. 


### 3.3 Modeling with Mutable Data
Data objects for which mutators are defined are known as mutable data objects. `(set-car! x y)`, sets the car of x to y. `(define z (cons y (cdr x)))` defines the car of z as y, and the cdr of z as the cdr of x. Including set! in a programming language, introduces all the issues of assignment and mutable data in general. 

A *queue* is a sequence in which items are inserted at one end (called the rear of the queue) and deleted from the other end. Queues can be considered a cons with the front-ptr as the car and the rear-ptr as the cdr. This way it only takes O(1) to insert an element. 


### 3.4 Concurrency: Time Is of the Essence
Building models in terms of computational objects with local state adds time as an essential concept in programming. It now depends when a procedure is executed in comparison to other procedures. *Sequential computers* execute only one operation at a time, so the amount of time it takes to perform a task is proportional to the total number of operations performed. *Concurrent computation* on the other hand can execute many functions at the same time. A possible restriction on concurrency is that no two operations that change shared state variables can occur at the same time. 

A program with two processes, one with three ordered events (a,b,c) and another with three ordered events (x,y,z). If the two processes run concurrently, with no constraints on how execution is interleaved, then there are 20 different possible orderings for the events. *Serialization* implements the following idea: Processes will execute concurrently, but there will be certain collections of procedures that cannot be executed concurrently. Serializers can be used by defining events in terms of a more primitive synchronization mechanism called a mutex. A *mutex* is an object that supports two operations: 1. the mutex can be acquired, 2. the mutex can be released. Once a mutex has been acquired, no other acquisition operations can be done until it is released. *Deadlock* occurs when each procedure is locked forever, until one of them is released. Deadlock is always a danger in systems that provide concurrent access to multiple shared resources.


### 3.5 Streams
*Streams* are a sequence which enables us to represent very large (even infinite) sequences. Stream processing models systems that have state without ever using assignment or mutable data. Additionally, streams use sequence manipulations without incurring the costs of manipulating sequences as lists. The basic idea is to to construct a stream only partially, and to pass the partial construction to the program that consumes the stream. If the consumer attempts to access a part of the stream that has not yet been constructed, the stream will automatically construct just enough more of itself to produce the required part. With ordinary lists, both the car and the cdr are evaluated at construction time. With streams, the cdr is evaluated at selection time. Mutability and delayed evaluation do not mix well in programming languages. 




## 4 Metalinguistic Abstraction
*Evaluators* or *Interpreters* are a procedure that, when applied to an expression of the language, performs the actions required to evaluate that expression. The evaluator, which determines the meaning of expressions in a programming language, is just another program.


### 4.1 The Metacircular Evaluator
A metacircular evaluator is written in the same language that it evaluates. Evaluation process is the interplay between two procedures: eval and apply. Environment is a sequence of frames, where each frame is a table of bindings that associates variables with their corresponding values.

It is possible to split eval into two parts: an expression and an environment. The procedure analyze takes only the expression. It performs the syntactic analysis and returns a new procedure, the execution procedure, that encapsulates the work to be done in executing the analyzed expression. The execution procedure takes an environment as its argument and completes the evaluation. This saves work because analyze will be called only once on an expression, while the execution procedure may be called many times `(define (eval exp env) ((analyze exp) env))`.


### 4.2 Variations on a Scheme — Lazy Evaluation
New languages are often invented by first writing an evaluator that embeds the new language within an existing high-level language. Scheme is an *applicative-order* language; all the arguments to Scheme procedures are evaluated when the procedure is applied. In contrast, *normal-order languages* delay evaluation of procedure arguments until the actual argument values are needed (lazy evaluation).
```
(define (try a b)
(if (= a 0) 1 b))
```

Evaluating `(try 0 (/ 1 0))` generates an error in Scheme. With lazy evaluation, there would be no error. Evaluating the expression would return 1, because the argument (/ 1 0) would never be evaluated.


### 4.3 Variations on a Scheme — Nondeterministic Computing
Stream processing uses lazy evaluation to decouple the time when the stream of possible answers is assembled from the time when the actual stream elements are produced. The evaluator supports the illusion that all the possible answers are laid out before us in a timeless sequence. With nondeterministic evaluation, an expression represents the exploration of a set of possible worlds, each determined by a set of choices. Some of the possible worlds lead to dead ends, while others have useful values.

[^1]: Abelson, H., Sussman, G. J., & Sussman, J. (1996). Structure and interpretation of computer programs. Justin Kelly.

