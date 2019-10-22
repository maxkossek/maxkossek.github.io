---
layout: post
title: How to Design Programs (Felleisen)
date: 2019-03-29
categories: [book]
tags: [computer science]
author: "Max Kossek"
description: Book Notes for How to Design Programs by Matthias Felleisen
---

<img style="float: right; width: 25%; margin: 0 1rem;" src="/assets/images/book-covers/htdp-cover.jpg" alt="How to Design Programs Book Cover">

The program "design recipe" consists of 6 steps:
1. Start with problem analysis and data definitions
2. Write a signature, purpose statement and header
3. Give functional examples of the program
4. Keep function templates or inventories of what is given and what needs to be computed
5. Code the function definition and evaluate results
6. Test and revise based on failures / successes


<div class="toc">
<strong>Table of Contents:</strong>
<ol>
<li><a href="#i-fixed-size-data">Fixed-Size Data</a></li>
<a href="#intermezzo-1">Intermezzo 1</a>
<li><a href="#ii-arbitrarily-large-data">II Arbitrarily Large Data</a></li>
<a href="#intermezzo-2-quote-unquote">Intermezzo 2: Quote, Unquote</a>
<li><a href="#iii-abstraction">III Abstraction</a></li>
<a href="#intermezzo-3-scope-and-abstraction">Intermezzo 3: Scope and Abstraction</a>
<li><a href="#iv-intertwined-data">IV Intertwined Data</a></li>
<a href="#intermezzo-4-the-nature-of-numbers">Intermezzo 4: The Nature of Numbers</a>
<li><a href="#v-generative-recursion">V Generative Recursion</a></li>
<a href="#intermezzo-5-the-cost-of-computation">Intermezzo 5: The Cost of Computation</a>
<li><a href="#vi-accumulators">VI Accumulators</a></li>
</ol>
</div>



## Prologue: How to Program
A "1String" is a string with length one (char).[^book]



## I Fixed-Size Data

### 1 Arithmetic
BSL operations on numbers include: `+, -, \*, /, abs, add1, ceiling, sqr` etc. A predicate is a function that determines whether an input belongs to some class of data `number? 4` evaluates to true.


### 2 Functions and Programs
Programs are functions, they consume inputs and produce outputs. There are two kinds of definitions:

1. constant definitions `(define x 5)`
2. function definitions that use parameters `(define (Function_Name Variable ... Variable) Expression)`

Best practice is to define one function per task. "Batch" programs consume all inputs at once and compute the result. "Interactive" programs consume some input, then compute, produce some output, then consume more input, and so on.


### 3 How to Design Programs
Programs consume information (domain) and then execute functions to produce output (range). Data definitions are often written in the form of comments `; A Temperature is a number`. The design process:

1. Express how you wish to represent information as data `; We use numbers to represent centimeters`
2. What does the function compute? Write down a signature, a statement of purpose, and a function header `; String -> Number`
3. Illustrate signature and purpose statement with functional examples
4. Take inventory, to understand what are the givens and what we need to compute
5. Write executable expressions and function definitions
6. Test the function on examples


### 4 Intervals, Enumerations, and Itemizations
Intervals are collections of elements that satisfy a specific property. Enumerations list every single piece of data belonging to an element. Itemizations, mixes the first two, specifying ranges in one clause and specific pieces of data in another clause. Finite state machines (FSM), are state transition diagrams with a limited number of states.


### 5 Adding Structure
Structure type definitions are another form of definition. These definitions create many different functions in one line `(define-struct StructureName [FieldNames])`. For example:
```
; movie-title can be used as selector
; movie? is predicate
(define-struct movie [title producer year])
(make-movie "Title" "Producer" 2000)
```

Sets are data collections or data classes. Data definitions play a central and important role in the design process. They delimit the possible ranges of values for functions / variables.


### 6 Itemizations and Structures
Data definitions describe a structure, the appropriate input data types, and what the inputs represent. Predicates determine if a piece of data is a member of some class. Add predicates to a program to detect faulty data types early.


### 7 Summary
Good programmers design, bad programmers tinker.



## Intermezzo 1
Syntax describes grammar and structure; semantics describes meaning. A sentence can be grammatically correct but make no sense. Example of a programming language's syntax description:
```
; function application:
(function argument ... argument)
; function definition:
(define (function-name parameter ... parameter)
  	function-body)
; conditional expression:
(cond
  	cond-clause
  	...
  	cond-clause)
; structure type definition
(define-struct name [name])
```



## II Arbitrarily Large Data

### 8 Lists
The empty list is `'()`, a one item list `(cons "Hello" '())`, two item list `(cons "Hello" (cons "World" '()))`. To check if a list is empty we can use the predicate `empty?`. `first` selects the first item of a cons; `rest` selects all items but the first of a cons.


### 9 Designing with Self-Referential Data Definitions
Recursion is the self-use of a function. The design process for recursive functions:

1. Problem Analysis: Develop a data representation for the information
2. Header: Write down a signature using defined names
3. Examples: Work through examples
4. Template: Translate the data definition into a template
5. Definition: Find a function that combines the values of the expressions in the cond clauses into the expected answer
6. Tests: Write check-expect testing suite.


### More on Lists
Lists and sets both are collections of values, but they differ:

property | lists | sets
--- | --- | ---
membership | one among many | critical
ordering | critical | irrelevant
no. of occurences | sensible | irrelevant
size | finite but arbitrary | finite or infinite


### 11 Design by Composition
`list` is shorthand for cons `(list "Hello" "World" ".")`. While programming, keep a wishlist of functions that must be designed to complete a program.


### 12 Projects: Lists
Recursive functions deal with self-references; Auxiliary functions deal with cross-references. Complex problems should be decomposed into smaller ones.



## Intermezzo 2: Quote, Unquote
"Quote" is another shorthand way to define a list `'("Hello" "World" ".")`. Use "quasiquote" to evaluate an expression during the construction of a list. Use `,@` to unquote-splice a nested list from it's surrounding list.
```
(define x 42)
`(40 41 ,x 43)
> (list 40 41 42 43)
```



## III Abstraction
Copying code means copying mistakes. When code is copied, one fix has to be applied to many different locations. This process is expensive and error-prone.

### 14 Similarities Everywhere
"Thou shall not steal code, not even your own." Instead, abstract over similar pieces of code. A parametric data definition abstracts from a reference to a particular collection of data in the same manner as a function abstracts from a particular value `; A [list-of-item] is one of: '(), (cons item [list-of-item])`. 


### 15 Designing Abstractions
To "abstract" is to turn something concrete into a parameter. Putting the definition for some functionality in one place makes it easy to maintain a program. When you discover a mistake, you have to only modify one place in the code. The design process for abstraction:

1. Compare two items for similarities.
2. Abstract - replace the contents of corresponding code highlights with new names and add these names to the parameter list.
3. Validate - test by defining the two original functions in terms of the abstraction.
4. Write new signature: `; [X Y] [List-of X] [X -> Y] -> [List-of Y]`.


### 16 Using Abstractions
Local definitions, or private definitions are used for functions that play a subordinate role and have no use outside of a narrow context. Local is used to reformulate deeply nested expressions.
```
(local (def ...)
  ; IN
  body-expression)
```

The design recipe for local definitions:
1. Follow the design recipe for functions (Distill the problem statement into a signature, a purpose statement, an example, and a stub definition.)
2. Find a matching abstraction. To match means to pick an abstraction whose purpose is more general than the one for the function to be designed; it also means that the signatures are related.
3. Write down a template. For the reuse of abstractions, a template uses local for two different purposes.
4. Define the auxiliary function inside local.
5. Test the definition.


### 17 Nameless Functions
We can view lambda as an abbreviation for a local expression `(lambda (variable1 variable2 ...) expression)`. A function definition abbreviates a plain constant definition with a lambda expression on the right-hand side. Lambda makes short functions more readable than local definitions.
```
; standard expression
(define (f x)
	(* 10 x))

; lambda expression
(define f
	(lambda (x) (* 10 x)))
```


### 18 Summary
Repeated code patterns call for abstraction. Most languages come with a large collection of abstractions.



## Intermezzo 3: Scope and Abstraction
There are two kinds of variable occurrences. A variable in the function header is a "binding occurrence" and a variable in the function’s body is the "bound occurrence". Regions of binding occurence are called the lexical scope. Free occurrence applies to a variable without a binding occurrence (not defined). The scope of local definitions is the local expression.
```
; f and g are called top-level scopes
(define (f x) 
	(+ (* x x) 25))
(define (g x) 
	(+ (f (+ x 1)) 
	(f (- x 1))))
```



## IV Intertwined Data

### 19 The Poetry of S-Expressions
Design recipe for self-referential data definitions:

1. Draw arrows to connect references to definitions. Construct some examples for every individual definition. 
2. Design as many functions in parallel as there are data definitions.
3. Work through functional examples that use all mutual references in the nest of data definitions.
4. For each function, design the template according to its primary data definition.
5. For the design of the body, start with cond lines that do not contain natural recursions or calls to other functions. They are called base cases. The corresponding answers are typically easy to formulate. After that, deal with the self-referential cases and the cases of cross-function calls.
6. Run the tests when all definitions are completed.


### 20 Iterative Refinement
Iterative refinement is a well-known scientific process. If the discrepancies between the predictions and the measurements are too large, a model is refined with the goal of improving the predictions. This iterative process continues until the predictions are sufficiently accurate.


### 21 Refining Interpreters
Many people refer to the interactions area as the read-eval-print loop (REPL). Eval is short for evaluator, a function that is also called an interpreter. Programmers invented parsers to bridge the gap between convenience of use and implementation. A parser simultaneously checks whether some piece of data conforms to a data definition and if it does, builds a matching element from the chosen class of data.


### 22 Project: The Commerce of XML
Configuration refers to the data that the main function needs when the program is launched.


### 23 Simultaneous Processing
If one of the parameters plays a dominant role, then think of the other parameters as atomic, and design around the dominant parameter. If multiple parameters are of the same length, process them simultaneously by applying recursion to all parameters at the same time. If there's no obvious connection between the parameters, then all possible examples have to examined (e.g by creating a table of possible combinations).



## Intermezzo 4: The Nature of Numbers
Overflow occurs when arithmetic produces numbers that are too large to be represented. Arithmetic operations preserve exactness when possible; they produce an inexact result when necessary. Tolerance for inexactness of numbers varies depending on the field (finance vs. science).



## V Generative Recursion
Programmers is another form of recursion that can be more powerful than structural recursion. An algorithm tends to rearrange a problem into a set of several problems, solving each, and then combining their solutions into one overall solution. Often some of these newly generated problems are the same kind of problem as the given one, in which case the algorithm can be reused to solve them. In these cases, the algorithm is recursive, but it's recursion uses newly generated data, not the input data.


### 25 Non-standard Recursion
There is no standard way to find algorithms. In many fields there is plenty of room for exploration.


### 26 Designing Algorithms
Design recipe for generative recursion:

1. Analyze the problem information and define data collections.
2. Write the signature, a function header, and a purpose statement.
3. Explain the “how” with function examples.
4. Define the algorithm as functions and expressions in terms of the chosen data representation.
5. Test.
6. Describe termination behavior, with (1) a size argument for each recursive call or (2) examples of exceptions to termination.

Generative recursion may generate new problems without any limitations, never terminating. Structural recursion relies on systematic data analysis and not much more; generative recursion often requires a deep, often mathematical, insight into the problem-solving process itself. Observable equivalence is when two functions produce the same output given the same input, but use different processes to process the input.


### 28 Mathematical Examples
Many solutions to mathematical problems employ generative recursion. Adaptive algorithms allocate time to those parts of the graph that need it and spend little time on the others.


### 30 Summary
Unlike structurally designed functions, algorithms may not terminate for some inputs. This problem might be inherent to the algorithm or related to how the algorithm was translated into code.


## Intermezzo 5: The Cost of Computation
Evaluating `(how-many some-list)` takes roughly n times some fixed amount of time, where n is the length of the list. Average-case analysis starts with the assumption that inputs are usually distributed between the best and worst-case. A best-case analysis focuses on the class of inputs for which the program can easily find the answer. A worst-case analysis determines how badly a program performs for those inputs that stress it most. Given a function g on the natural numbers, O(g) (pronounced: “big-O of g”) is a class of functions on natural numbers. A function f is a member of O(g) if there exist numbers c and bigEnough such that for all n >= bigEnough it is true that f(n) <= c\*g(n).



## VI Accumulators
Accumulators are arguments that represent the context of the function call. During recursive calls, the accumulator changes in relation to the context.


### 32 Designing Accumulator-Style Functions
Design recipe for functions that use accumulators:

1. Sketch a manual evaluation of the function to understand the nature of the accumulator.
2. Determine the kind of data that the accumulator tracks. 
3. Use the invariant (constant relation) to determine the initial value a0 for a.
4. Exploit the invariant to determine how to compute the accumulator for the recursive function calls.


### 34 Summary
Traversals "forget" past arguments when they progress throughout the program. Introduce an accumulator if knowledge of this lost data helps. To create an accumulator, determine it's initial state, how to update it and how to use it's knowledge.



## Epilogue: Moving On
Computing generalizes simple arithmetic (0’s and 1’s) to more general processes, so we can compute almost any type of data in a variety of ways.


[^book]: Felleisen, M., Findler, R. B., Flatt, M., & Krishnamurthi, S. (2018). How to design programs: an introduction to programming and computing. MIT Press.