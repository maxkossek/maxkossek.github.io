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
: String data type. A string is not a list of `char`s.

`'a list`
: List type. Defined as: `datatype 'a list = nil | :: of 'a * 'a list`.

`'a option`
: Option type. Defined as `datatype 'a option = NONE | SOME of 'a`.


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

Equality operators: `=`, `<>` (not equal).

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

The "infix" keyword declares a function as infix.
```
- fun floatAdd (x, y) : real = x + y;
val floatAdd = fn : real * real -> real
- infix floatAdd;
infix floatAdd
- 3.0 floatAdd 5.0;
val it = 8.0 : real
- floatAdd 3.0 5.0;
stdIn:54.1-54.9 Error: expression or pattern begins with infix identifier "floatAdd"
```

The "op" keyword converts an infix operator to prefix.
```
- op + (3, 5);
val it = 8 : int
- foldl op+ 0 [1, 2, 3];
val it = 6 : int
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
- (1,2);
val it = (1,2) : int * int
- (1,"a",false);
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

A record assigns a name/tag to the fields in a tuple. The field tags can be uppercase of lowercase.
```
- val bob = { name = "Bob", age = 50 };
val bob = {age=50,name="Bob"} : {age:int, name:string}
- val white = { R = 255, G = 255, B = 255 };
val white = {B=255,G=255,R=255} : {B:int, G:int, R:int}
```

The hash operator ("#") retrieves the matching field from a record.
```
- val point = { x = 3.0, y = 5.0 };
val point = {x=3.0,y=5.0} : {x:real, y:real}
- val x = #x point;
val x = 3.0 : real
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
  | add5 (x::xs) = (x + 5) :: add5 xs

- fun fst (a,_) = a;
val fst = fn : 'a * 'b -> 'a
- fun snd (_,b) = b;
val snd = fn : 'a * 'b -> 'b
- fun shift (a,b,c) = (c,a,b);
val out = fn : 'a * 'b * 'c -> 'c * 'a * 'b
```

The "case" keyword is another way to perform pattern matching. A `case` statement has the form: `case e of p1 => e | ... | pn => e`.
```
fun is_even x =
  case x mod 2 of
    0 => true
  | _ => false
```


Patterns:

`c`
: Constant.

`[]`, `(x::[]`), `(x::xs)`
: List.

`(c,c)`, `(c,c,c)`, ...
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

While loops have the form `while e do e`.
```
fun dec r = while !r > 0 do r := !r - 1;
```



EXCEPTIONS
----------

The "raise" keyword raises an exception. Built-in exception types: `Domain`, `Empty`, `Fail of string`, `SysErr of string * syserror option`, `UnequalLengths`, `Option`, `Overflow`.
```
- fun safeDiv (x, y) = if y = 0 then raise Domain else x div y;
val safeDiv = fn : int * int -> int
- safeDiv (5, 0);

uncaught exception Domain [domain error]
  raised at: stdIn:58.21-59.4
```

The "handle" keyword is used to catch / handle an exception.
```
- fun hdExn [] = raise Empty 
=   | hdExn (x::xs) = x;
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
