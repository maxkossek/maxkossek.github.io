---
layout: post
title: Standard ML
date: 2020-06-17
categories: [man]
tags: [language, functional]
author: "Max Kossek"
description: Reference for the Standard ML functional programming language.
sitemap:
    lastmod: 2020-06-17
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

Expressions in the interpreter have to separated by a semicolon (";"). In a source code file, this operator should not be used.

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

`word`
: Unsigned integer that supports modular arithmetic, and logical operations.

`'a array`
: Array.

`'a list`
: List type. Defined as: `datatype 'a list = nil | :: of 'a * 'a list`.

`'a option`
: Option type. Defined as `datatype 'a option = NONE | SOME of 'a`.

`'a vector`
: Vector.


The "type" keyword creates a synonym to a built-in data type.
```
type id = string
```

The "datatype" keyword creates a new user-defined data type. The data constructors in a `datatype` declaration must be capitalized.
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

Standard ML can determine most types using type inference. To explicitly state a value's type use the syntax: `e : t`.
```
- val r = 5.0 : real;
val r = 5.0 : real

- fun add x y = x + y;
val add = fn : int -> int -> int
- fun add (x : real) (y : real) = x + y;
val add = fn : real -> real -> real
```



OPERATORS
---------

Arithmetic operators: `+`, `-`, `*`, `/` (real division), `div` (integer division), `mod` (integer modulo).

Unary operators: `~` (negation).

Boolean operators: `not`, `andalso`, `orelse`.

Equality operators: `=`, `<>` (not equal), `<`, `>`, `<=`, `>=`.

Concatenation operator: `^` (string concatenation), `@` (list concatenation).



VARIABLES
---------

Variables are assigned using the "val" keyword. Variable names can begin with a lowercase or uppercase letter and are usually written in camelCase.
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
```

The "let" keyword binds an expression to a value. Let bindings have the form: let val n = e ... in e end`.
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

The "ref" keyword creates a new reference. The ":=" assignment operator assigns a new value to a reference: `r := e`. The "!" operator dereferences the value of a reference.
```
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

Functions are defined using the "fun" keyword. Function names can begin with a lowercase or uppercase letter and are usually written in camelCase.
```
- fun adder x y = x + y ;;
val adder = fn : int -> int -> int
- adder 5 3 ;;
val it = 8 : int
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

Functions can be recursive.
```
fun fact n =
  if n <= 1 then 1
  else n * fact (n - 1);
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

The "infix" keyword declares a function as infix. The infix keyword can be declared before or after a function is being defined. An optional number before the function name specifies the precedence of the infix operator. Using an infix operator as prefix raises an error.
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

The "op" keyword converts an infix operator to prefix.
```
- op + (3, 5);
val it = 8 : int
- foldl (op +) 0 [1, 2, 3];
val it = 6 : int
- foldl (op ::) [1, 2] [3, 4];
val it = [4,3,1,2] : int list
```




LISTS
-----

Lists contain a comma-separated list of elements that must all have the same data type. The empty list is represented as `[]`.
```
- [];
val it = [] : 'a list
- [1, 2, 3];
val it = [1,2,3] : int list
- ["hello", "world"];
val it = ["hello","world"] : string list
```

Lists can be nested.
```
- [[1, 2], [3, 4]];
val it = [[1,2],[3,4]] : int list list
```

The "::" cons operator conses an element to the front of a list.
```
- 1 :: [];
val it = [1] : int list
- 2 :: [1];
val it = [2,1] : int list
- 3 :: 2 :: [1];
val it = [3,2,1] : int list
```

The "@" operator concatenates two lists.
```
- [1, 2] @ [3, 4];
val it = [1,2,3,4] : int list
```

List functions:

```
- foldl op+ 0 [1, 2, 3];
val it = 6 : int
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
  | x :: xs => if n = 0 then SOME x else nth (n - 1) xs
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

open Example
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




EXCEPTIONS
----------

The "raise" keyword raises an exception. Built-in exception types: `Domain`, `Empty`, `Fail of string`, `Subscript`, `SysErr of string * syserror option`, `UnequalLengths`, `Option`, `Overflow`.
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


Src: [^chilipala] [^reppy]

[^chilipala]: Chlipala, A. (2006). Comparing Objective Caml and Standard ML. Retrieved from http://adam.chlipala.net/mlcomp/

[^reppy]: Reppy, J. (2004). The Standard ML Basis Library. Retrieved from https://smlfamily.github.io/Basis/manpages.html
