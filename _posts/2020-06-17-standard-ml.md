---
layout: post
title: Standard ML
date: 2020-06-17
categories: [man]
tags: [language, functional]
author: "Max Kossek"
description: Reference for the Standard ML functional programming language.
sitemap:
    lastmod: 2020-06-20
---

NAME
----

Standard ML -- Modular, functional programming language



DESCRIPTION
-----------

The `$ sml` command starts the interpreter for Standard ML implementations that support it. The `use` command reads in a file into the interpreter. To directly load in a file from the command line use the command: `$ sml file.sml`.
```
$ sml
Standard ML of New Jersey (64-bit) v110.97 [built: Tue Apr 21 17:39:48 2020]
- use "hello.sml"
[opening hello.sml]
val hello = fn : unit -> unit
val it = () : unit
- hello ();
Hello World
val it = () : unit
```

Expressions in the interpreter have to separated by a semicolon (";"). In a source code file, this operator should not be used. In the toplevel, the result of an expression is bound to the value `it` if it is not bound to another value.
```
- "hello world";
val it = "hello world" : string
- 3 + 5;
val it = 8 : int
- it + 1;
val it = 9 : int
```

Standard ML implementation files have a ".sml" files. Signature files carry a ".sig" extension.

Comments begin with `(*` and end with `*)`.



INPUT / OUTPUT
--------------

The `print` function prints a string to the standard output.
```
- print;
val it = fn : string -> unit
- print "Hello World";
Hello Worldval it = () : unit
```

The `_` or `()` patterns can be used to discard a `unit` return value.
```
- print "abc\n";
abc
val it = () : unit
- val _ = print "abc\n";
abc
- val () = print "abc\n";
abc
```

The `TextIO.openIn` function opens a file for reading. The `TextIO.inputAll` function gets all the input from the input stream. The `TextIO.closeIn` close an input stream.
```
fun readFile f =
  let val file = TextIO.openIn f
      val content = TextIO.inputAll file
      val _ = TextIO.closeIn file
  in String.tokens (fn c => c = #"\n") content
  end
```

The `TextIO.openOut` function opens a file for writing. The `TextIO.output` function writes a string to the output stream. The `TextIO.closeOut` function closes and output stream.
```
fun writeFile f =
  let val file = TextIO.openOut f
      val _ = TextIO.output (file, "Writing to file")
  in TextIO.closeOut file
  end
```



DATA TYPES
----------

`bool`
: Boolean value, either `true` or `false`.

`char`
: Character.

`int`
: Integers.

`real`
: Real numbers.

`string`
: String data type. Strings are immutable and not a list of `char`s.

`unit`
: Unit type.

`word`
: Unsigned integer that supports modular arithmetic, and logical operations.

`order`
: Ordered type. Defined as `datatype order = LESS | EQUAL | GREATER`.

`'a array`
: Array.

`'a list`
: List type. Defined as: `datatype 'a list = nil | :: of 'a * 'a list`.

`'a option`
: Option type. Defined as `datatype 'a option = NONE | SOME of 'a`.

`'a ref`
: Reference type.

`'a vector`
: Vector.


The "type" keyword creates a synonym to a built-in data type.
```
type id = string
```

The "datatype" keyword creates a new user-defined data type. The data constructors in a `datatype` declaration are usually capitalized.
```
datatype binop = Plus | Minus | Times | Div

datatype stmt = Num of int | Str of string | Cstmt of stmt * stmt

- datatype 'a tree = Leaf | Node of 'a tree * 'a * 'a tree;
datatype 'a tree = Leaf | Node of 'a tree * 'a * 'a tree
- Leaf;
val it = Leaf : 'a tree
- Node (Leaf, 5, Leaf);
val it = Node (Leaf,5,Leaf) : int tree
```

Type declarations can have one or more type parameters if they are written as a tuple.
```
- datatype ('a, 'b) either = Left of 'a | Right of 'b;
datatype ('a,'b) either = Left of 'a | Right of 'b
- Left 5;
val it = Left 5 : (int,'a) either
```

Standard ML can determine most types using type inference. However, some operators are overloaded. To explicitly state a value's type use the syntax: `e : t`.
```
- fun add (x, y) = x + y;
val add = fn : int -> int -> int

- fun add (x, y) : real = x + y;
val add = fn : real -> real -> real

- fun add (x, y) = (x + y) : real;
val add = fn : real -> real -> real

- fun add (x : real, y : real) = x + y;
val add = fn : real -> real -> real

- fun add (x : real, y) = x + y;
val add = fn : real -> real -> real
```



OPERATORS
---------

Arithmetic operators: `+`, `-`, `*`, `/` (real division), `div` (integer division), `mod` (integer modulo).

Unary operators: `~` (negation).

Boolean operators: `not`, `andalso`, `orelse`.

Equality operators: `=`, `<>` (not equal), `<`, `>`, `<=`, `>=`.

Concatenation operator: `^` (string concatenation), `@` (list concatenation).

Function composition operator: `o`.



VARIABLES
---------

Variables are assigned using the "val" keyword. Variable names can begin with a lowercase or uppercase letter and are usually written in camelCase. Pattern matching allows you to assign values from a list or tuple.
```
- val i = 5;
val i = 5 : int
- val f = 3.14;
val f = 3.14 : real
- val n = ~125 - ~25;
val n = ~100 : int
- val c = #"a";
val c = #"a" : char
- val s = "Hello World\n";
val s = "Hello World\n" : string

- val (a, b) = (1, 2);
val a = 1 : int
val b = 2 : int

- val [a, b, c] = [1, 2, 3];
stdIn:21.5-21.26 Warning: binding not exhaustive
          a :: b :: c :: nil = ...
val a = 1 : int
val b = 2 : int
val c = 3 : int
```

The "let" keyword binds an expression to a value. Let bindings have the form: let val n = e ... in e end`. Let bindings are the main mechanism to nest functions and expressions.
```
fun circleArea r =
  let val pi = Math.pi
      val rsqr = Math.pow (r, 2.0)
  in pi * rsqr
  end
```



REFERENCES
----------

A reference is a pointer to a location in memory.

The "ref" keyword creates a new reference. The ":=" assignment operator assigns a new value to a reference: `re := e`. The "!" operator dereferences the value of a reference: `! re`.
```
- ref;
val it = fn : 'a -> 'a ref
- op :=;
val it = fn : 'a ref * 'a -> unit
- !;
val it = fn : 'a ref -> 'a

- val r = ref 5;
val r = ref 5 : int ref
- !r;
val it = 5 : int
- r := 10;
val it = () : unit
- !r;
val it = 10 : int
```



FUNCTIONS
---------

Functions are defined using the "fun" keyword. Functions definitions have the form: `fun f p = e`. Function calls have the form: `f a`. Function names can begin with a lowercase or uppercase letter and are usually written in camelCase.
```
- fun add x y = x + y ;;
val add = fn : int -> int -> int
- add 5 3 ;;
val it = 8 : int

- fun pointAdd (x1, y1) (x2, y2) = (x1 + x2, y1 + y2);
val pointAdd = fn : int * int -> int * int -> int * int
- pointAdd (5, 5) (3, 7);
val it = (8,12) : int * int

- fun sqrtList l = List.map Math.sqrt l;
val sqrtList = fn : real list -> real list
- sqrtList [25.0, 256.0];
val it = [5.0,16.0] : real list
```

Functions with multiple arguments can be written with tuples, as curried functions, or a combination of the two. Curried functions can be partially applied. Conventionally, higher-order functions (functions that take other functions as arguments) are curried, while first-order functions are written as tuples. For example, the list concatenation operator `@` takes in a list tuple, while the higher-order functions `List.map` and `List.filter` are curried.
```
- fun add (x, y) = x + y;
val add = fn : int * int -> int

- fun add x y = x + y;
val add = fn : int -> int -> int
- add 5;
val it = fn : int -> int
```

By default, the Standard ML will try to infer the types of a function. To explicitly specify the types of the function parameters / return value use the ":" operator.
```
- fun add (x, y) = x + y;
val add = fn : int * int -> int
- fun floatAdd (x, y) : real = x + y;
val floatAdd = fn : real * real -> real
- fun intAdd (x, y) : int = x + y;
val intAdd = fn : int * int -> int

- fun floatAdd (x : real, y : real) = x + y;
val floatAdd = fn : real * real -> real
- floatAdd (3.0, 5.0);
val it = 8.0 : real
```

Functions can be recursive. The "and" keyword allows you to create mutually recursive functions.
```
fun fact n =
  if n <= 1 then 1
  else n * fact (n - 1);

fun even n = n = 0 orelse odd (n - 1)
and odd n = n <> 0 andalso even (n - 1)
```

Lambda / anonymous functions have the form: `(fn a => e)`.
```
- (fn x => x + 5) 3;
val it = 8 : int

- List.foldl (fn (x, y) => x :: x :: y) [] [1, 2, 3];
val it = [3,3,2,2,1,1] : int list

- val getThird = fn (a, b, c) => c;
val getThird = fn : 'a * 'b * 'c -> 'c
- getThird (1, 2, 3);
val it = 3 : int
```

The "infix" keyword declares a function as infix. The infix keyword can be declared before or after a function is being defined. Using an infix operator as prefix raises an error. Optionally, you can specify the precedence of the infix function name using the syntax: `infix [0-9] f`; where `[0-9]` is the precedence of the `f`. The built-in infix identifiers have the following precedences: `infix 7 * / div mod`; `infix 6 + - ^`; `infix 5 :: @`; `infix 4 = <> > >= < <=`; `infix 3 := o`; `infix 0 before`.
```
- fun floatAdd (x, y) : real = x + y;
val floatAdd = fn : real * real -> real
- infix floatAdd;
infix floatAdd
- 3.0 floatAdd 5.0;
val it = 8.0 : real

- infix 7 %;
infix 7 %
- fun x % y = x mod y;
val % = fn : int * int -> int
- 10 % 8;
val it = 2 : int
- 18 % 10;
val it = 8 : int
```

The "op" keyword converts an infix operator to prefix. This is useful if you want to determine the signature of an infix operator, or want to pass an infix operator to a higher-order function.
```
- op + (3, 5);
val it = 8 : int

- op +;
val it = fn : int * int -> int
- op <;
val it = fn : int * int -> bool

- List.foldl (op +) 0 [1, 2, 3];
val it = 6 : int

- List.foldl (op ::) [1, 2] [3, 4];
val it = [4,3,1,2] : int list

- List.reduce (op @) [] [[1, 2], [3, 4]];
val it = [3,4,1,2] : int list
```

The "o" operator is used for function composition: `f o g`. The expression `(f o g) a` is equivalent to `f (g a)`. The "before" keyword evaluates two expressions and returns the first of them: `a before b`. The "ignore" keyword discards the result of a computation, and returns the unit value `()`.
```
- op o;
val it = fn : ('a -> 'b) * ('c -> 'a) -> 'c -> 'b
- (Math.sqrt o Math.sqrt o Math.sqrt) 256.0;
val it = 2.0 : real

- ignore;
val it = fn : 'a -> unit
- ignore (3 + 5);
val it = () : unit

- op before;
val it = fn : 'a * unit -> 'a
- 3 + 5 before print "computed";
computedval it = 8 : int
```




LISTS
-----

Lists contain a comma-separated list of elements that must all have the same data type. The empty list is represented as `[]` or `nil`.
```
- [];
val it = [] : 'a list
- nil;
val it = [] : 'a list
- [1, 2, 3];
val it = [1,2,3] : int list
- ["hello", "world"];
val it = ["hello","world"] : string list
- [[1, 2], [3, 4]];
val it = [[1,2],[3,4]] : int list list
```

The "::" cons operator conses an element to the front of a list. The "@" operator concatenates two lists.
```
- 1 :: [];
val it = [1] : int list
- 2 :: [1];
val it = [2,1] : int list
- 3 :: 2 :: [1];
val it = [3,2,1] : int list

- [1, 2] @ [3, 4];
val it = [1,2,3,4] : int list
```

The `List` structure in the Basis library provides most of the standard list functions.
```
- List.filter (fn x => x mod 2 = 0) [1, 2, 3, 4];
val it = [2,4] : int list
- List.foldl (op +) 0 [1, 2, 3];
val it = 6 : int
- List.map (fn x => x + 5) [1, 2, 3];
val it = [6,7,8] : int list
```



TUPLES
------

Tuples are a combination of some number of elements that can be of different data types.
```
- (1, 2);
val it = (1,2) : int * int
- (1, "a", false);
val it = (1,"a",false) : int * string * bool
- ([1, 2], [3, 4]);
val it = ([1,2],[3,4]) : int list * int list
```

Tuples can be nested inside lists.
```
- [(1, #"a"), (2, #"b")];
val it = [(1,#"a"),(2,#"b")] : (int * char) list
```



RECORDS
-------

A record assigns a name/tag to the fields in a tuple. The field tag name can be uppercase of lowercase.
```
- val bob = {name = "Bob", age = 50};
val bob = {age=50,name="Bob"} : {age:int, name:string}

- val white = {R = 255, G = 255, B = 255};
val white = {B=255,G=255,R=255} : {B:int, G:int, R:int}
```

The hash operator ("#") retrieves the matching field from a record.
```
- val point = {x = 3.0, y = 5.0};
val point = {x=3.0,y=5.0} : {x:real, y:real}
- val x = #x point;
val x = 3.0 : real

- fun addPoint (p : {x : real, y : real}) = #x p + #y p;
val addPoint = fn : {x:real, y:real} -> real
- addPoint { x = 3.0, y = 5.0 };
val it = 8.0 : real
```



ARRAYS
------

An `Array` is a mutable, polymorphic, sequence of values that supports constant-time access and update operations.

The `Array.array` function takes in an integer and element tuple, and initializes an array where all of the elements are equal to the input element.
```
- Array.array (3, #"a");
val it = [|#"a",#"a",#"a"|] : char array
```

The `Array.fromList` function converts a list into an array. The `Array.sub` functions returns the `nth` element in the array. The `Array.update` function updates a value in an array.
```
- val a = Array.fromList [1, 2, 3];
val a = [|1,2,3|] : int array
- Array.sub (a, 1);
val it = 2 : int
- Array.update (a, 0, (~1));
val it = () : unit
- a;
val it = [|~1,2,3|] : int array
```



VECTORS
-------

A `Vector` is an immutable sequence of values that supports a constant-time access operation.

The `Vector.fromList` function constructs a vector from an input list. The `Vector.sub` function returns the value at the specified index. The `Vector.update` function updates an element in the vector.
```
- val v = Vector.fromList [1, 2, 3];
val v = #[1,2,3] : int vector
- Vector.sub (v, 2);
val it = 3 : int
- Vector.update (v, 0, (~1));
val it = #[~1,2,3] : int vector
```



PATTERN MATCHING
----------------

Pattern matching involves matching an expression against one of a number of patterns. The expression of only the first matching pattern is evaluated.

`[]` matches against the empty list. `_` is the wildcard pattern that matches with any value.

Use the vertical bar (`|`) "or" operator to perform pattern matching inside a function. Function pattern matching has the syntax: `fun n p1 = e | n p2 = e | ... | n pn = e`.
```
fun fib 0 = 0
  | fib 1 = 1
  | fib n = fib (n - 1) + fib (n - 2)

fun err "" = "error: unspecified error"
  | err x = "error: " ^ x

fun add5 [] = []
  | add5 (x :: xs) = (x + 5) :: add5 xs

- fun fst (a, _) = a;
val fst = fn : 'a * 'b -> 'a
- fun snd (_, b) = b;
val snd = fn : 'a * 'b -> 'b
- fun shift (a, b, c) = (c, a, b);
val out = fn : 'a * 'b * 'c -> 'c * 'a * 'b
```

The "fn" keyword defines a local function that can be used to match the last parameter of a function with multiple arguments.
```
fun addOpt x = fn
    SOME y => x + y
  | NONE => x

fun nthOpt n = fn
    [] => NONE
  | x :: xs => if n = 0 then SOME x else nthOpt (n - 1) xs
```

The "case" keyword is another way to perform pattern matching. A `case` statement has the form: `case e of p1 => e | ... | pn => e`. Case statements have higher precedence than imperative statements.
```
fun is_even x =
  case x mod 2 of
    0 => true
  | _ => false

fun initList l =
  case l of
    [] => (print "Empty list: initializing\n"; [0])
  | _ => (print "Non-empty list\n"; l)
```

A pattern match should handle all possible input cases. The interpreter will warn you if a pattern match is non-exhaustive. The wildcard pattern `_` matches any value. To avoid future errors, the wildcard pattern should be used conservatively when matching types whose values can change (such as user-defined types).
```
- fun f 1 = true;
stdIn:30.5-30.15 Warning: match nonexhaustive
          1 => ...

val f = fn : int -> bool
```


Patterns:

`c`
: Constant.

`[]`, `x :: []`, `x :: xs`
: List.

`(c, c)`, `(c, c, c)`, ...
: Pair.

`T`
: Data type constructor.

`{t}`, `{t, t}`, ...
: Record, where `t` is the tag of the field.

`()`
: Unit / Empty tuple.

`NONE` or `SOME c`
: Option match.

`_`
: Wildcard pattern (matches anything).



CONDITIONALS
------------

If-expressions have the syntax: `if e1 then e2 else e3`.
```
- if true then 1 else 0 ;;
val it = 1 : int
```


LOOPS
-----

While loops have the form: `while e do e`.
```
fun dec r = while !r > 0 do r := !r - 1;
```



EXCEPTIONS
----------

The built-in exception types: `Bind`, `Chr`, `Div`, `Domain`, `Empty`, `Fail of string`, `Match`, `Size`, `Span`, `Subscript`, `SysErr of string * syserror option`, `UnequalLengths`, `Option`, `Overflow`.

The "raise" keyword raises an exception.
```
- fun safeDiv (x, y) = if y = 0 then raise Domain else x div y;
val safeDiv = fn : int * int -> int
- safeDiv (5, 0);

uncaught exception Domain [domain error]
  raised at: stdIn:58.21-59.4
```

The "handle" keyword is used to catch / handle an exception.
```
- List.nth ([1, 2, 3], 4) handle Subscript => ~1;
val it = ~1 : int

- fun hdExn [] = raise Empty
=   | hdExn (x :: xs) = x;
- val h = hdExn [] handle Empty => 1;
val h = 1 : int
```

The "exception" keyword creates a user-defined exception.
```
- exception DivByZero;
exception DivByZero

- exception BadLog of string;
exception BadLog of string
- BadLog "Invalid input row";
val it = BadLog(-) : exn
```



STRUCTURES
----------

A structure's signature is written in a ".sig" signature file. The signature defines the interface of a structure. Structure signature are conventionally written as all uppercase: `signature NAME`. A function signature has the form: `val f: a [ -> a -> ... -> a ]`.
```
signature EXAMPLE =
  sig
    exception IndexOutOfBounds

    val nthExn: 'a list -> int -> 'a
    val nthOpt: 'a list -> int -> 'a option
  end
```

The structure of a signature is placed in a ".sml" file. The structure defines the implementation of its corresponding interface. The names of structures are usually capitalized: `structure Name :> NAME`.
```
structure Example: EXAMPLE =
  struct
    exception IndexOutOfBounds

    fun nthExn l n =
      case l of
        [] => raise IndexOutOfBounds
      | x :: xs => if n = 0 then x else nthExn xs (n - 1)

    fun nthOpt l n =
      case l of
        [] => NONE
      | x :: xs => if n = 0 then SOME x else nthOpt xs (n - 1)
  end
```

To load the signature and structure in the interpreter use the "use" command.
```
- use "example.sig";
[opening example.sig]
signature EXAMPLE =
  sig
    exception IndexOutOfBounds
    val nthExn : 'a list -> int -> 'a
    val nthOpt : 'a list -> int -> 'a option
  end
val it = () : unit
- use "example.sml";
[opening example.sml]
structure Example : EXAMPLE
opening Example
  exception IndexOutOfBounds
  val nthExn : 'a list -> int -> 'a
  val nthOpt : 'a list -> int -> 'a option
val it = () : unit
- nthExn [1, 2, 3] 4;

uncaught exception IndexOutOfBounds
  raised at: example.sml:7.21-7.37
- nthOpt [1, 2, 3] 4;
val it = NONE : int option
```

The "open" keyword opens the definitions of a structure within the current context. This allows you to view all of a structure's signatures and use a structure's definitions without qualifying them.
```
- Math.sqrt;
val it = fn : real -> real
- sqrt;
stdIn:1.2-1.6 Error: unbound variable or constructor: sqrt
- open Math;
opening Math
  type real = ?.real
...
- sqrt;
val it = fn : real -> real
```

The ":>" operator stands for a signature constraint. Signature constraints allow you to work with abstract data types.
```
signature UNSAFESTR =
  sig
    type unsafestr
    val tounsafe : string -> unsafestr
    val tosafe : unsafestr -> string option
  end

structure UnsafeStr :> UNSAFESTR =
  struct
    type unsafestr = string
    fun tounsafe s = s
    fun tosafe s = if s = "" then NONE else SOME s
  end

- structure U = UnsafeStr;
structure U : UNSAFESTR
- val userInput = U.tounsafe "Some input";
val userInput = - : UnsafeStr.unsafestr
- U.tosafe userInput;
val it = SOME "Some input" : string option
- val userInput = U.tounsafe "";
val userInput = - : UnsafeStr.unsafestr

- U.tosafe "";
stdIn:10.1-10.12 Error: operator and operand do not agree [tycon mismatch]
  operator domain: UnsafeStr.unsafestr
  operand:         string
  in expression:
    U.tosafe ""
- (U.tosafe o U.tounsafe) "";
val it = NONE : string option
```

The Basis library provides standard library functions: `Array`, `Bool`, `Byte`, `Char`, `CommandLine`, `Date`, `General`, `INetSock`, `Int`, `IO`, `List`, `ListPair`, `Math`, `NetHostDB`, `Option`, `OS`, `OS.IO`, `OS.Path`, `OS.Process`, `Posix`, `Real`, `Socket`, `Stream`, `String`, `Text`, `Time`, `Timer`, `Unix`, `UnixSock`, `Vector`, `Word`.



FUNCTORS
--------

A functor is a mapping from one structure to another structure that preserves the properties of the structures. A functor definition has the form: `functor F (F : T) :> T = struct ... end`.
```
signature BASE =
sig
  type t
  val base : t
end

signature TO =
sig
  type t
  val toList : t list
  val toPair : int -> int * t
end

functor ToFunctor (I : BASE) :> TO =
struct
  type t = I.t
  val toList = [I.base]
  fun toPair x = (x, I.base)
end

structure IntBase : BASE =
struct
  type t = int
  val base = 5
end

structure IntTo = ToFunctor(IntBase)

- IntTo.toList;
val it = [-] : IntTo.t list
- IntTo.toPair 3;
val it = (3,-) : int * IntTo.t
```



Src: [^chilipala] [^reppy]

[^chilipala]: Chlipala, A. (2006). Comparing Objective Caml and Standard ML. Retrieved from http://adam.chlipala.net/mlcomp/

[^reppy]: Reppy, J. (2004). The Standard ML Basis Library. Retrieved from https://smlfamily.github.io/Basis/manpages.html
