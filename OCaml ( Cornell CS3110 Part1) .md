# OCaml Programming ( Cornell CS3110 Part1)



# Ch2. The Basics of OCaml

## 2.1 Expressions

Every expression has : 

**Semantics :** 

- Type-checking rules (***static semantics***) : produce a type or fail with an error message
- Evaluation rules (***dynamic semantics***) : produce a value, or exception or infinite loop

> ***Static*** refers to the state of a program before it is executed. For example, any operations performed by the compiler on a program that has not yet run are considered static.

In OCaml, all values are expressions, but the reverse is not true. (by now)

Note it must be `+.` in float and `+` in int.

## 2.2 If Expressions

> `==>` 	pronounced evaluate to
>
> `:` 	      pronounced  has type

You can use expressions like (32 : int) to explicitly prompt the compiler to perform type checking and ensure that a variable matches the programmer's intended type. Note that this operation is merely an annotation, not a type conversion.

***if expression*** : 

`if e1 then e2 else e3`

`e1` must be bool, `e2` and `e3` must be the same type.

## 2.3 Let Definitions

`let x = 32 (* binds 32 to x *)`. Unlike in imperative programming, the value of `x` cannot be changed, which is an important characteristic of functional programming.

You can also write let `(y : int) = 31`, where `(y : int)` is merely a type annotation provided by the programmer, not a type conversion. The parentheses can be omitted, allowing a shorter form: `let y : int = 32`.

**A definitions gives a name to a value**

**Definitons are not expressions, or vice-versa**

**But definitions syntactically contain expressions**

**Syntax :** 

`let x = e`

where x is an ***identifier (NOT variable)***

identifier must start with a lowercase letter.

**Evaluation:**

Evaluate `e` to value `v`, then

Bind `v` to `x`: henceforth, `x` will evaluate to `v`

let definition IS NOT expression, Vice versa.

## 2.4 Let Expressions

An interesting usage make ***let*** convert to ***expressions*** : let a = 0 in a;;  bind 0 to a and evaluate a. another example : `let b = 2 in 2 * b`

Out of ***let*** Expression, ***let*** defintion become invalid. That’s saying after `let a = 0 in a`,  `let a = 0` become invalid.

## 2.5 Variable Expressions and Scope

### 2.5.1 let definitions toplevel

`let x = e` is implicitly, "in rest of what you type",  e.g.

```ocaml
let a = "big";;
let b = "red";;
let c = a ^ b;;
(* toplevel understands as: *)
let a = "big" in
let b = "red" in
let c = a ^ b in ..
```

## 2.6 Scope and the Toplevel

"An interesting phenomenon."

```ocaml
let x = 1
let x = 2
(* val x : int = 2 *)
```

It seems like the value of x has been changed (which contradicts the idea of functional programming?), but in reality, the above code is equivalent to

```ocaml
let x = 1 in (*allocate memory that always 1*)
	let x = 2 in (*allocate memory that always 1*)
		x
```

In reality, when the outer let x = 1 is executed, the inner computation has already finished, and the inner `let x = 2` disappears. The above code effectively becomes `let x = 1 in 2`.

## 2.7 Anonymous Functions

`fun x -> x + 1` define an anonymous function, invoke this : `(fun x -> x + 1) 30`

## 2.8 Lambdas

### 2.8.1 Anonymous function expression

**Syntax**: `fun x1 ... xn -> e (* fun is keyword in OCaml* )`

### 2.8.2 Functions are values

***Can use them anywhere we use values:***

- functions can take functions as arguments
- functions can return functions as results

## 2.9 Function application

Syntax: `e0 e1 ... en`

No parentheses are needed unless you need to explicitly change the order of evaluation.

Evaluation of `e0 e1 … en`:

1. Evaluate subexpressions:
   
    `e0 ==> v0, ..., en ==> vn`
    
    `v0` must be a function:
    
    `fun x1 ... xn -> e`
    
2. substitute `vi` for `xi` in `e` yielding new expression `e'`. evaluate it : `e’ ==> v`
3. result is `v`

e.g.

from

`(fun x y -> x + y) (2 + 3) 8`

to
`(fun x y -> x + y) 5 8`

to
`5 + 8`

to
`13`

## 2.10 Named Functions

`let inc = fun x -> x + 1`

*is equivalent to*

`let inc x = x + 1;;`

The above two ways are equivalent. Later is syntactic sugar

> not necessary, but make language “sweeter”
> 

another syntactic sugar : 

`(fun x -> x + 1) 2;;`

is equivalent to

`let x = 2 in x + 1;;`

The above two ways are equivalent. Later is syntactic sugar

or do this : 

`let f x y = x - y in f 3 2;;`

## 2.11 Recursive Functions

OCaml explicitly declare recursive function : 

let rec f …

## 2.12 Function Types

Type `t -> u` is the type of a function that takes input of type `t` and returns output of type `u`

Type `t1 -> t2 -> u` is the type of a fucntion that takes input of type `t1` and another input of type `t2` and returns output of type `u`

Note dual purpose for  `->` syntax : 

- function types
- functions values (e.g.  `fun x -> x + 1`, arrow points to value )

## 2.13 Partial Application

type in utop :

`let add x y = x + y;; (*  val add : int -> int -> int = <fun> *)`

`add 3;; (* - : int -> int = <fun> *)`

`let add3 y = add 3;; (* bind fun to add3 *)`

`add3 4;;  (* - : int = 7 *) `

### 2.14.1 More syntactic sugar

***Multi-argument functions do not exist !!!***

`fun x y -> e`

*is syntactic sugar for* 

`fun x -> (fun y -> e)`

*e.g.*

`fun x y -> x + y` 

*is syntactic sugar for*

`fun x -> fun y -> x + y`

***SO, FUNCTIONS ARE VALUES !***

## 2.13 Polymorphic Functions

### 2.13.1 Type variables

**Variable** : name standing for unknown value

**Type variable** : name standing for unknown type

Cpp example : `std::vector<T>`

OCaml Syntax : single quote followed by identifier

e.g. `'foo`, `'key`, `'value`

But most often simply just : `'a` (pronounced alpha), `'b` (pronounced beta)

### 2.13.2 Polymorphism

- *poly =* many*, morph =* form
- write function that works for many arguments regardless of their type
- colsely related to C++ template instantiation

## 2.14 Operators As Functions

`1 + 2` 

Equivalent to

`( + ) 1 2`

we can define our own Operator, e.g.

`let ( <^> ) x y = max x y`

usage : `1 <^> 3`

## 2.15 Application Operator

Application:

`let ( @@ ) f x = f x`

Reverse application:

`let ( |> ) x f = f x`

aka pipline

OCaml defined that :

`f @@ x` is equivalent to `f x` 

and

`x |> f` is equivalent to `f x`

# Ch3. Data and Type

## 3.1 Lists

`[] (* is 'a list *)`

`[1] (* is int list *)`

`[1;2;3;] (* is int list *)`

`[[2;3];[3;4]] (* is int list list *)`

type pronounced from right to left : `[[2;3];[3;4]]` is a list of list of int

operator `::` is used to concat `'a` and `'a list`

### 3.1.1 OCaml lists

- Immutable : cannot change elements. **All you can do is construct new lists out of old lists !**
- Singly-linked:
    - Good for sequential access of short-to-medium length lists
    - Data structures are tools: none is perfect
    - we’ll study the implementation later

## 3.2 List Syntax and Semantics

**Syntax:**

- `[]` is the empty list which is pronounced "nil"

- `e1 :: e2` prepends element `e1` to `list e2`. `::` is pronounced "cons"
- `[e1; e2]` is sugar for `e1 :: e2 :: []`
- And similarly for longer lists `[e1; e2; e3]` etc.

**Evaluation:**

- `[]` is a value
- To evaluate `e1 :: e2`,
    - evaluate `e1` to value `v1`,
    - evaluate `e2` to a (list) value `v2`, and
    - return `v1 :: v2`

### 3.2.1 List types

for any type t, the type t list describes lists where all elements have type t

- `[1;2;3] (* int list *)`
- `[true] (* bool list *)`
- `[[1 + 1; 2 - 3];[3 * 7]] (* int list list *)`

## 3.3 Records and Tuples

**A record is an aggregation of named elements, while a tuple is an aggregation of unnamed elements.**

**define a record type** : 

```ocaml
type student = {
    name : string;
    year : int;
}
```

use record type : 

```ocaml
let ljh = {
    name = “liujunhui”;
    year = 2001;
}
```

Or, if you want to explicitly perform type checking : 

```ocaml
let ljh : student = {
    name = “liujunhui”;
    year = 2001;
}
```

> execute xxx.ml : `type #use "xxx.m";;` in utop
> 

by `ljh.name` to access the content of `name` field of record `student`

**define a tuple type** : 

```ocaml
type time = int * int * string
```

use tuple type : 

```ocaml
let t : time = (10, 10, "am") (* utop shows type time *)
let s = (10, 10, "am") (* utop shows type int * int * string *)
```

## 3.4 Comparsion of Data Type

- length
    - unbounded : List
    - bounded :
        - access by posisiton : Tuple
        - access by name : Record

## 3.5 Record Syntax and Semantics

**Records and tuples**

- **new kind of definition : type definition**
- **new kinds of types : record types, tuple types**

---

**Record syntax**

- `type t = {f1 : t1; f2 : t2}` define a record type `t`, `f1`, `f2` are fields and `t1`, `t2` are types

- `{f1 = e1; f2 = e2}` is a record with fields named f1 and f2
    - Order of fields is irrelevant
    - any number of fields permitted from 1 upto about 4 million
- e.g. accesses field `f` of a record expression `e` : `e.f`
- ***Field names are identifiers not expressions***

---

**Record semantics**

**Evaluation**

- If ei ==> vi then {f1 = e1; ...; fn = en} ==> {f1 = v1; ...; fn = vn}
- If e ==> {…; f = v; …} then e.f ==> v

**Type checking**

- Record types must be defined before use so that OCaml knows the field names
- if ei : ti and t is defined to be {f1 : t1, …, fn : tn}, then {f1 = e1; …; fn = en} : t
- if e : t and if t is defined to be {…; f : tf; …}, then e.f : tf

**Record copy**

{e with f1 = e1} creates a copy of record e with new field value for f1

- this is not mutation : original record unchanged
- can have multiple new field values:
    - {e with f1 = e1; f2 = e2; f3 = e3} etc.
- sugar for {f1 = e1; …; fn = en; g1 = e.g1; …; gm = e.gm} where gi are the fields of e that are not named in the copy
- cannot "add" new fields : type cannot change

## 3.6 Tuples Syntax and Semantics

**Tuple Syntax**

- `(e1, e2, e3)` is a tuple

- `type t = t1 * t2` defines a tuple type, `t1` and `t2` are types (can be different)

- Order of components is relevant

**Evaluation** 

If `ei` ==> `vi`

then `(e1, ..., en)` ==> `(v1, ..., vn)`

**Type checking ** 

If `ei : ti`

then `(e1, ..., en)` : `t1 * ... * tn`

## 3.7 Pattern Matching

```ocaml
match not true with | true -> "nope" | false -> "yep" (* - : string = "yep" *)

match 32 with foo -> foo + 1 (* - : int = 33*)

match "foo" with | "bar" -> 0 | _ ->1 (* - : int = 1 *)

match [] with | [] -> "empty" | _ -> "not empty" (* - : string *)

(* note pattern matching all data type pointed by -> should be the same. e.g. *)

match ["liu"; "junhui"] with | [] -> "empty" | a :: b -> a (* - : string = "liu" *)

match ["liu"; "junhui"] with | [] -> ["empty"] | a :: b -> b (* - : string list = ["junhui"] *)

(* pattern matching can match record. e.g. *)

type student = {name : string; year : int}

let ljh = {name = "liujunhui"; year = 2001}

let name_with_year s = match s with | {name; year} -> name ^ (string_of_int year)
```

## 3.8 Pattern Matching With Lists

A list can only be:

- nil, or
- the cons of an element onto another list

Use **pattern matching** to access list in one of those ways : 

```ocaml
let empty lst = 
    match lst with
    | [] -> true
	| _ -> false (* _::_ or h::t *)
```

---

```ocaml
let rec sum lst = 
    match lst with
    | [] -> 0
    | h :: t -> h + (sum t)
```

> tips : use #trace sum;; can trace function invocation stack. #untrace sum;; to cancel trace
> 

---

```ocaml
let rec length lst = 
    match lst with
    | [] -> 0
    | h :: t -> 1 + (length t)
```

---

```ocaml
(* example usage : append [1;2;3] [4;5;6] is [1;2;3;4;5;6]
   accually is what (@) operator do *)
let rec append lst1 lst2 = 
    match lst1 with
        | [] -> lst2
        | h :: t -> h :: (append t lst2)
```

## 3.9 The `function` Keyword

*Immediately matching against implicit final argument is so useful there’s sugar for it*

```ocaml
let f x y z =
    match z with
    | p1 -> e1
    | p2 -> e2
```

*Can be written*

```ocaml
let f x y = function
    | p1 -> e1
    | p2 -> e2
```

e.g.

```ocaml
let rec length lst = 
    match lst with
    | [] -> 0
    | h :: t -> 1 + (length t)
```

*Can be written*

```ocaml
let rec length = function
    | [] -> 0
    | h :: t -> 1 + (length t)
```

## 3.10 `::` vs `@`

- `::`
    - "cons"
    - Add an element onto the head of a list
    - `'a -> 'a list -> 'a list`
    - Constant time : O(1)
- `@`
    - "append"
    - Combine two lists
    - `'a list -> 'a list -> 'a list`
    - Linear time in first list : for lst1 @ lst2, it’s O(length lst1)

## 3.11 Pattern Matching Syntax and Semantics

**Syntax :** 

```ocaml
match e with
| p1 -> e1 (* branch1 *)
| p2 -> e2 (* branch2 *)
| ...
| p2 -> en (* branch n *)
```

Can have more branches, or even just one

p is a new syntactic class : ***pattern expressions***

### 3.11.1 Match expressions

**Evaluations** :

- if `e` ==> `v`
    - and `pi` is the first pattern, top-to-bottom, that matches `v`
    - and `ei` ==> `vi`
    - then `(match e ... )` ==> `vi`
- but if no pattern match, raise an `exception Match_failure`

**Type-checking** : 

if `e` and `pi` have type `ta` and `ei` have type `tb` then entire match expression has type `tb`

### 3.11.2 Basic pattern syntax

- `x` i.e. any identifier
- `_` i.e. underscore
- any constant

### 3.11.3 Semantics of pattern matching

- A constant matches itself
- An identifier aka pattern variable matches anything and binds it in the scope of the branch
- The underscore aka wilcard matches anything but doesn’t bind it

### 3.11.4 Data type patterns

- `[]`
- `p1 :: p2` etc.
- `[p1; p2]` etc.
- `(p1, p2)` etc.
- `{f1 = p1; f2 = p2}` etc.

All of which match the corresponding data type values, and can bind parts of them

## 3.12 Static Checking of Pattern Matching

OCaml’s static checking is not only type checking

e.g.

```ocaml
let bad_empty lst = 
    match lst with
    | [] -> true
```

This coverage of space of possible shapes that this data could take is not exhaustive; simply say we’ve left out a branch which maybe cause Match_failure exception at runtime. Luckily OCaml’s static checking will detect it.

---

```ocaml
let rec bad_sum lst = 
    match lst with
    | h :: t -> h + bad_sum t
    | [x] -> x (* branch2 *)
    | [] -> 0
```

OCaml compiler will detect branch 2 will never been enter

---

```ocaml
let rec bud_sum’ lst = 
    List.hd lst + bad_sum’ (List.tl lst)
```

List.hd and List.tl are from list standard library. List.hd [1;2;3];; returns 1 : int and List.tl [1;2;3];; returns [2;3] : int list. OCaml complier will **not** detect that something wrong because the risk of error depends on standard library fun

### 3.12.1 Compiler check for your errors

- Static semantics is more than just type checking
- OCaml compiler checks for
    - Exhaustiveness of patterns
    - Unused branches
- Do not ignore those warnings — the compiler is finding your errors and giving you a chance to fix them !!

## 3.13 Variants

```ocaml
type point = float * float (* **tuple** *)
type primary_color = Red | Green | Blue (* **varient** like **enumerate** *)
type shape = Circle of {center  : point; radius : float} | Rectangle of {lower_left : point; upper_right : point}
let r (* : primary_color *) = Red
let c1 = Circle {center = (0., 0.); radius = 1.}
let r1 = Rectangle {lower_left = (-1., -1.); upper_right = (1., 1.)}
```

## 3.14 Pattern Matching with Variants Part 1

> In OCaml, ***constructor*** is used in variant types and record types to create concrete data type instance. For example, in type color = Red | Green | Blue, **Red, Green and Blue are constructor**. Or in type shape = Circle of float | Rectangle of float * float, **Cirle and Rectangle are also constructors but they need parameters** (let s = Circle 10.  ;;  let s2 = Rectangle (5., 8.);; )
> 

3.13 cont’d

```ocaml
let avg a b = (a +. b)  /. 2.
let center s = match s with Circle {center; radius} -> center | Rectangle {lower_left; upper_right} -> let (x_ll, y_ll) = lower_left in let (x_ur, y_ur) = upper_right in (avg x_ll x_ur, avg y_ll y_ur)
```

## 3.15 Pattern Matching with Variants Part 2

we can modify 

```ocaml
let center s = match s with Circle {center; radius} -> center | Rectangle {lower_left; upper_right} -> let (x_ll, y_ll) = lower_left in let (x_ur, y_ur) = upper_right in (avg x_ll x_ur, avg y_ll y_ur)
```

to

```ocaml
let center s = match s with Circle {center; radius} -> center | Rectangle {lower_left = (x_ll, y_ll); upper_right = (x_ur, y_ur} -> (avg x_ll x_ur, avg y_ll y_ur)
```

## 3.16 Variant Syntax and Semantics

**Type definiation syntax** : 

```ocaml
type t = 
| C1 of t1
| ...
| Cn of tn
```

where `Ci` is **Constructors aka tags**, ti is Optional data carried by constructor

> OCaml requires Constructors names must to be capitalized
> 

### 3.16.1 Non-constant variant expressions

**Syntax :** `C e`

**Evaluation :** 

if `e` ==> `v` then `C e` ==> `C v`

**Type checking :** 

`C e : t`

if `t = ...| C of t' | ...` and `e : t'`

**Pattern matching :** 

`C p` is a pattern

**for example :** 

```ocaml
type point = float * float (* **tuple** *)
type shape = Circle of {center  : point; radius : float} | Rectangle of {lower_left : point; upper_right : point}
let c1 : shape = Circle {center = (0., 0.); radius = 1.}
```

### 3.16.2 Constant variant expressions

**Syntax :** `C e`

**Evaluation :** already a value

**Type checking :** 

`C e : t`

if `t = ... | C | ...` 

**Pattern matching :** 

`C` is a pattern

## 3.17 Algebraic Data Type

when we model the real world, we will use variant when we use OR and record when AND. 

- Records and tuples : each-of types
    - aka **product** types
- Variants : one-of types
    - aka **sum** types
    - **aka tagged union**

### 3.17.1 Algebraic Data Type (Records and Vriants)

- Sums and product : algebra
- Hence variants known as algebraic data types (ADT)
- ADT also stands for abstract data type

## 3.18 An ADT for Pokemon

**A feature in OCaml :** 

```ocaml
type ptype = Normal | Fire | Water
type peff = Normal | NotVery | Super
let x = Normal (* this refers to the last Normal constructor because ***definition shadow*** in OCaml *)
```

---

**An idiomatic way to solve that (we can do better after leaning *module* in OCaml) :**

```ocaml
type ptype = TNormal | TFire | TWater
type peff = ENormal | ENotVery | ESuper
let mult_of_eff = function ENormal -> 1.0 | ENotVery -> 0.5 | ESuper -> 2.0
(* let mult_of_eff p = match p with ENormal -> 1.0 | ENotVery -> 0.5 | ESuper -> 2.0 *)
let eff = function
| (TFire, TFire) ->ENotVery
| (TWater, TWater) ->ENotVery
| (TFire, TWater) -> ENotVery
| (TWater, TFire) -> ESuper
| _ -> ENormal
```

## 3.19 Recursive Parameterized Variants

---

```ocaml
type intlist = Nil | Cons of int * intlist
let rec length = function Nil -> 0 | Cons (_, t) -> 1 + length t
```

---

*but when we need stringlist, must we paste code and rename fun length ? NO ! We can do like this :* 

```ocaml
type 'a mylist = Nil | Cons of 'a * 'a mylist
let rec length = function Nil -> 0 | Cons (_, t) -> 1 + length t
```

---

*By the way, we actually could get some nicer syntax for our lists :* 

```ocaml
type 'a mylist = [] | (::) of 'a * 'a mylist
let rec length = function [] -> 0 | _ :: t -> 1 + length t
```

**This is OCaml list implementation**

- **'a list** is a **type constructor** parameterized on **type vaiable 'a**
- `[]` and `::` are constructors

## 3.20 Option : A built-in variant

**options are build-in variant in the OCaml standard library :** 

`type 'a option = None | Some of 'a`

*type in utop :* 

`None (* - : a option = None *)`

`Some 1 (* - : int option = Some 1 *)`

`Some [1;2;3] (* - : int list option = Some [1; 2; 3] *)`

---

*we can define  a fun to extract 'a option -> 'a :* 

```ocaml
let get_val o = match o with None -> failwith "??" | Some x -> x
```

---

*because is None we don’t know what 'a to return, how to solve it ? Add a parameter default :* 

```ocaml
let get_val default o = match o with None -> default | Some x -> x (* val get_val : 'a -> 'a option -> 'a = <fun> *)
```

---

***something practical :*** 

```ocaml
let rec list_max (lst : 'a list) : 'a option = 
    match lst with
    | h :: t -> begin
        match list_max t with 
        | None -> Some h
        | Some m -> Some (max h m)
    end
    | [] -> None
```

> `begin ... end` is same as `(...)` but is more **striking** for programmers

## 3.21 Exceptions : extensible variants

`#show exn;; (* type exn = .. *)`

**Provide constructors (`exception` is a keyword to define exceptions. exception inherit from exn)**

```ocaml
exception ABadThing
exception OhNo of string
```

**Use this**

```ocaml
OhNo "oops"
```

---

**Type *exn* is a build-in *extensible* variant**

***Normally when we define variants, we have to list all of the constructors right there as part of the definition**. exn allows us to **provide the constructors later on** with that keyword :* 

```ocaml
exception OhNo of string (* we should provide the constructor OhNo like this *)
OhNo "oops" (* use like this after provide exception OhNo *)
```

---

**Relation between `exn` and `exception`**

- Type `exn` is a build-in **extensible** variant
- Add new constructors to it with `exception`
- Build-in function `raise` : exn -> 'a

---

**Some predefined exceptions :** 

- `exception Failure of string`
    - Exception raised by libraary functions to signal that they are undefined on the given argrments
    - Build-in function `failwith : string -> 'a`
- `exception InvalidArgument of string`
    - Exception raised by library functions to signal that the given argements do not make sense
    - Build-in function `invalid_arg : string -> ’a`

## 3.22 Handing Exceptions

```ocaml
let safe_div x y = 
    try x / y with
        | Division_by_zero -> 0
```

**Syntax** 

```ocaml
try e with
    | p1 -> e1
    | p2 -> e2
```

**Evaluation** 

if `e ==> v` then `(try e ...)` ==> `v`

if e raises an exception x then match x against the patterns if none match reraises x

**Type checking** 

`(try e …) : t`

if `e : t` and `ei : t` and `pi : exn`

## 3.23 Binary Trees

```ocaml
type 'a tree = Leaf | Node of 'a * 'a tree * 'a tree
let t = Node (1, Node (2, Leaf, Leaf), Node(3, Leaf, Leaf))
let rec size = function Leaf -> 0 | Node (_, l, r) ->1 + size l + size r
let rec sum = function Leaf -> 0 | Node (v, l, r) -> v + sum l + sum r
```

***OCaml do this so BRILLIANTLY !***

# Ch4. Higher-Order Programming

## 4.1 Higher-Order Functions

***Recall that FUNCTIONS ARE VALUES !***

- Functions can be used anywhere we use values
- Functions can take functions as arguments
- Functions can return functions as reusults

***So functions are higher-order***

e.g.

```ocaml
let double x = 2 * x
let square x = x * x
let quad x = 4 * x
let quad' x = double (double x)
let quad'' x = x |> double |> double
let fourth x = x * x * x * x
let fourth' x = square (square x)
let fourth'' x = x |> square |> square
```

---

*Note that quad’’ and fourth’’ have something similar, can we **factor it out** ?*

e.g.

```ocaml
let twice f x = f (f x)
let quad''' x = twice double x
let quad'''' = twice double (* val quad'''' : int -> int = <fun> *)
```

## 4.2 Map

```ocaml
let inc x = x + 1
List.map (fun x -> (inc x) ) [1;2;3] (* bad style *)
(* is equivalent to*)
List.map inc [1;2;3] (* good style *)
```

## 4.3 Implementing Map

```ocaml
let rec add1 = function             [] -> [] | h :: t -> (h + 1) :: add1 t
let rec concat3110 = function [] -> [] | h :: t -> (h ^ "3110") :: concat3110 t
```

---

***factor the similarity out !***

```ocaml
let rec transform f = function [] -> [] | h :: t -> f h :: transform f t (* val transform : ('a -> 'b) -> 'a list -> 'b list = <fun> *)
let add1' y = transform (fun x -> x + 1) y (* val add1' : int list -> int list = <fun> *)
let concat3310' y = transform (fun x -> x ^ "3310") y (* val concat3310' : string list -> string list = <fun> *)
let stringlist_of_intlist lst = transform string_of_int lst (* val stringlist_of_intlist : int list -> string list = <fun> *)
```

---

***map is transform above !***

```ocaml
let rec map f = function [] -> [] | h :: t -> f h :: map f t (* val map : ('a -> 'b) -> 'a list -> 'b list = <fun> *)
```

---

***Abstraction Principle : Factor out recurring code patterns. Don’t duplicate them.***

## 4.4 Combine

```ocaml
let rec sum = function [] -> 0 | h :: t -> h + sum t (* val sum : int list -> int = <fun> *)
let rec concat = function [] -> "" | h :: t -> h ^ concat t (* val concat : string list -> string = <fun> *)
```

---

***factor the similarity out !***

```ocaml
let rec combine init op = function [] -> init | h :: t -> op h (combine init op t) (* val combine : 'a -> ('b -> 'a -> 'a) -> 'b list -> 'a = <fun> *)
let rec sum' lst = combine 0 (+) lst (* val sum' : int list -> int = <fun> *)
let concat' lst = combine "" (^) lst (* val concat' : string list -> string = <fun> *)
```

---

***fold is combine above ! And fold is a cousin of reduce***

```ocaml
let rec fold init op = function [] -> init | h :: t -> op h (fold init op t) (* val fold : 'a -> ('b -> 'a -> 'a) -> 'b list -> 'a = <fun> *)
```

## 4.5 Fold

**`List.fold_right` (implemented in standard library)**

`List.fold_right f [a;b;c] init` 

computes

`f a (f b (f c init))`

*we can implement by ourselves*

```ocaml
let rec fold_right f lst acc = match lst with [] -> acc | h :: t -> f h (fold_right f t acc) (* val fold_right : ('a -> 'b -> 'b) -> 'a list -> 'b -> 'b = <fun> *)
let _ = fold_right (+) [1;2;3] 0 (* - : int = 6 *)
```

---

**`List.fold_left`**

`List.fold_left f init [a;b;c]`

computes

`f (f (f init a) b) c`

*we can implement by ourselves*

```ocaml
let rec fold_left f acc lst = match lst with [] -> acc | h :: t -> let acc' = f acc h in fold_left f acc' t (* val fold_left : ('a -> 'b -> 'a) -> 'a -> 'b list -> 'a = <fun> *)
let _ = fold_left (+) 1 [1;2;3] (* - : int = 7 *)
```

---

different between fold_left and fold_right : 

- fold_left is TR : No work remaining to do after recursive call so that it will require only constant stack space.

- fold_right is not TR : Some work remaining to do after recursive call so that it is going to take stack space that is linear in the size of list size.

## 4.6 Filter

```ocaml
let rec evens = function 
    | [] -> [] 
    | h :: t -> begin
        if h mod 2 = 0
        then h :: evens t
        else evens t
    end (*  val evens : int list -> int list = <fun> *)
let rec odds = function 
    | [] -> [] 
    | h :: t -> begin
        if h mod 2 = 1
        then h :: odds t
        else odds t
    end (*  val odds : int list -> int list = <fun> *)
```

---

***factor the similarity out !***

```ocaml
let rec filter p = function
    | [] -> []
    | h :: t -> if p h then h :: filter p t else filter p t (* val filter : ('a -> bool) -> 'a list -> 'a list = <fun> *)
let even x = x mod 2 = 0
let odd x = x mod 2 = 1
let evens' lst = filter even lst
let odds' lst = filter odd lst
```

---

***use TR.   (critical idea is use an acc container to hold element h.)***

```ocaml
let rec filter_aux p acc = function
    | [] -> List.rev acc (* after TR, we reverse the result list *)
    | h :: t ->filter_aux p (if p h then h :: acc else acc) t
let rec filter' p lst = filter_aux p [] lst
```

## 4.7 Trees with Map and Fold

```ocaml
type 'a tree = Leaf | Node of 'a * 'a tree * 'a tree (* type 'a tree = Leaf | Node of 'a * 'a tree * 'a tree *)
let t = Node (1, Node (2, Leaf, Leaf), Node (3, Leaf, Leaf)) (* val t : int tree = Node (1, Node (2, Leaf, Leaf), Node (3, Leaf, Leaf)) *)
let rec map f = function Leaf -> Leaf | Node (v, l, r) -> Node (f v, map f l, map f r) (* val map : ('a -> 'b) -> 'a tree -> 'b tree = <fun> *)
let rec fold acc f = function Leaf -> acc | Node (v, l, r) -> f v (fold acc f l) (fold acc f r) (* val fold : 'a -> ('b -> 'a -> 'a -> 'a) -> 'b tree -> 'a = <fun> *)
let sum t = fold 0 (fun x y z -> x + y + z) t (* val sum : int tree -> int = <fun> *)
let _ = sum t (* - : int = 6 *)
let _ = map (fun c -> c * 2) t (* - : int tree = Node (2, Node (4, Leaf, Leaf), Node (6, Leaf, Leaf)) *)
```

***How nice to code in this higher order way ~***

# Ch 5. Modular Programming

## 5.1 Modular Programming

### 5.1.1 Scale

- OCaml : 200 000 LoC
- Unreal engine 4 : 2 000 000 LoC
- Windows Vista : 50 000 000 LoC

https://informationisbeautiful.net/visualizations/million-lines-of-code/ shows how many codes in read world software

... can’t be done by one person

... no individual programmer can understand all the details

... too complex to build with OCaml we’ve seen so far

### 5.1.2 Modularity

Modular programming : 

- develop separately
- understand in isolation
- reason locally, not globally

### 5.1.3 Features for modularity

|  | Java | OCaml |
| --- | --- | --- |
| Namespaces | Classes, packages | Structures |
| interfaces | Interfaces | Signatures |
| Encapsulation | Public, private | Abstract types |
| Code reuse | Subtyping, inheritance | Functors, includes |

## 5.2 Modules and Structures

```ocaml
module Mylist = struct
    type 'a mylist =
    | Nil
    | Cons of 'a * 'a mylist

    let rec map f = function
    | Nil -> Nil
    | Cons (h, t) -> Cons (f h, map f t)
end

module Tree = struct
    type 'a tree =
    | Leaf
    | Node of 'a * 'a tree * 'a tree

    let rec map f = function
    | Leaf -> Leaf
    | Node (v, l, r) -> Node (f v, map f l, map f r)
end

let lst = Mylist.map succ (Cons (1, Nil)) (* val lst : int Mylist.mylist = Mylist.Cons (2, Mylist.Nil) *)
let t = Tree.Node (1, Leaf, Leaf) (* t : int Tree.tree = Tree.Node (1, Tree.Leaf, Tree.Leaf) *)
let t' : int Tree.tree = Node (1, Leaf, Leaf) (* another way to tell OCaml which structrue value belongs to) *)
```

*Node that types of Cons, Nil and Leaf belongs to which structure will be infered by OCaml*

## 5.3 Functional Stacks

```ocaml
module Mystack = struct
    type 'a stack =
    | Empty
    | Entry of 'a * 'a stack

    let empty = Empty

    let push x s = Entry (x, s)

    let peek = function
    | Empty -> failwith "Emtpy"
    | Entry (x, _) -> x

    let pop = function
    | Empty -> failwith "Emtpy"
    | Entry (x, stk) -> stk
end
```

**Syntax comparison**

JAVA : 

```java
s = new Stack();
s.push(1);
```

OCaml : 

```ocaml
let s = MyStack.empty
let s' = MyStack.push 1 s
```

## 5.3 Functional Data Structures

- No mutable updates : operations take an “old” value and return “new” value
- Functional data structres are **persistent** rather than **ephemral**
- Efficiency ? Complexity ?

The persistence here is conceptual; at the lower level of OCaml's implementation, different values are not stored in different places, so there's no need to worry about resource wastage.

## 5.4 Module and Structure Syntax and Semantics

**Module definition Syntax** 

```ocaml
module ModuleName = struct
    definitions
end
```

- ModuleName must be capitalized, idiomatically in CamelCase
- `Definitions` inside of a module can be anything we’ve seen :
    - `let`, `type`, `exception`
    - `module`
    - Can be terminated by`;;` but not idiomatic

---

**Structure semantics** 

To evaluate a `structure`

```ocaml
struct
    definition1
    definition2
    ...
    definitionN
end
```

Evaluate each `definition` in order top-to-bottom

Result is a `module` value aka module, which binds names to values

---

```ocaml
module M = struct
	...
end
```

Evaluate the `structure` to a `module`

*or say*

Bind the module to M

---

**Modules are not like other values**

- Cannot bind module to name with let
- Cannot pass module to function as parameter
- Cannot return module from function as output

## 5.5 Scope and Opening

5.3 Cont’d

```ocaml
let x = ListStack.peek (ListStack.push 42 List.Stack.empty)
```

*is equivalent to*

```ocaml
let x = ListStack.(peek (push 42 empty)
```

*is equivalent to*

```ocaml
let x = ListStack.(empty |> push 42 |> peek)
```

*is equivalent to*

```ocaml
let x = let open ListStack in empty |> push 42 |> peek
```

*is equivalent to*

```ocaml
open ListStack (* all the rest of code are in ListStack scope *)
let x = empty |> push 42 |> peek
```

## 5.6 Function Queues

```ocaml
module TwoListQueue = struct
(* [{front = [a;b]; back = [e; d; c]}]
represents the queue a, b, c, d, e
if [front] is empty then [back] must alse emtpy,
to guarantee that the next the first element of
the queue is always the head of [front] *)
(* constant time except when front gets exhausted *)
    type 'a queue = {
        front : 'a list;
        back : 'a list
    }

    let empty = {
        front = [];
        back = []
    }

    let peek = function
        | {front = []} -> None
        | {front = x :: _} -> Some x

    let enqueue x = function
        | {front = []} -> {front = [x]; back = []}
        | q -> {q with back = x :: q.back}

    let dequeue = function
        | {front = []} -> None
        | {front = _ :: []; back} -> Some {front = List.rev back; back = []}
        | {front = _ :: t; back} -> Some {front = t; back}
end
```



## 5.7 Exceptions vs Options and More Application Operators

*Notice the differences of*

`let peek = function [] -> failwith "Empty" | x :: _ -> x`

*and*

`let peek = function [] -> None | x :: _ -> Some x`

---

```ocaml
module ListQueue = struct
    type 'a queue = 'a list

    let empty = []

    let enqueue x q =
        q @ [x]

    let peek = function
        | [] -> None
        | x :: _ -> Some x

    let dequeue = function
        | [] -> None
        | _ :: q -> Some q
end
```

*if we implement queue like this, when*

```ocaml
let q : int list = ListQueue.(empty |> enqueue 42 |> dequeue |> enqueue 43)
```

*there will be an error saying This expression has type int queue option but an expression was expected of type int queue.*

***Solution***

```ocaml
(* Option.map *)
let ( >>| ) opt f = match opt with None -> None | Some x -> Some (f x) (* val ( >>| ) : 'a option -> ('a -> 'b) -> 'b option = <fun> *)
let q : int list option = ListQueue.(empty |> enqueue 42 |> dequeue >>| enqueue 43)
```

*but there will be an error when*

```ocaml
let q : int list option = ListQueue.(empty |> enqueue 42 |> dequeue >>| enqueue 43 >>| dequeue)
```

*because we get an* int list option value, *then we >>| dequeue on it (option been extracted and match int list then inject option), then we will get an* `int list option option` *rather than* `int list option`*. So we should do this :* 

```ocaml
(* Option.bind *)
let ( >>= ) opt f = match opt with None -> None | Some x -> f x
let q : int list option = ListQueue.(empty |> enqueue 42 |> dequeue >>| enqueue 43 >>= dequeue)
```

## 5.8 Module Types and Signatures

```ocaml
module type Fact = sig
    (* [fact n] is [n] factorial. *)
    val fact : int -> int
end
module RecuriveFact : Fact = struct
    let rec fact n = if n = 0 then 1 else n * fact (n - 1)
end
module TailReceriveFact : Fact  = struct
    (* we cannot use fact_aux because it is not exposed in module type Fact *)
    let rec fact_aux n acc = if n = 0 then acc else fact_aux (n - 1) (n * acc)
    (* but we must provide fact which sig in module type Fact *)
    let fact n= fact_aux n 1
end
```

- Comment does go in the ***module type*** not the ***module***.
- The ***module type*** is where we document all of the specifications about what functions are supposed to do for the rest of the world.

## 5.9 Module Types for Stacks and Queues

```ocaml
module type LIST_STACK = sig
    (** [Empty] is raised when an operation cannot be applied
    to an empty stack. *)
    exception Empty

    (** [empty] is the empty stack. *)
    val empty : 'a list

    (** [is_empty s] is whether [s] is empty. *)
    val is_empty : 'a list -> bool

    (** [push x s] pushes [x] onto the top of [s]. *)
    val push : 'a -> 'a list -> 'a list

    (** [peek s] is the top element of [s].
    Raises [Empty] if [s] is empty. *)
    val peek : 'a list -> 'a

    (** [pop s] is all but the top element of [s].
    Raises [Empty] if [s] is empty. *)
    val pop : 'a list -> 'a list
end

module ListStackImpl = struct
    let empty = []

    let is_empty = function [] -> true | _ -> false

    let push x s = x :: s

    exception Empty

    let peek = function
        | [] -> raise Empty
        | x :: _ -> x

    let pop = function
        | [] -> raise Empty
        | _ :: s -> s
end

module ListStack : LIST_STACK = ListStackImpl 

(* tell OCaml the type of module ListStack and the value of module ListStack. Or we can add type annotation when we define the module ListStackImpl *)
```

## 5.10 Module Type Syntax and Semantics

**Module type syntax :** 

```ocaml
module type ModuleTypeName = sig
    specifications
end
```

- Name does not have to be capitalized but usually is
    - Older idiom : `MODULE_TYPE_NAME`
    - specifications include `val, type, exception, module type`

---

**Module definition syntax revisited**

```ocaml
module ModuleName : Mt = struct
    definitions
end

(* OR *)

module ModuleName = (struct
    definitions
end : Mt)
```

*because we have learned Module type, so we can add type annotation when we define a module to **ceal** the module.* 

---

**Module type semantics :** 

*If you give a module a type* 

```ocaml
module Mod : Sig = struct … end
```

*Then type checker ensures*

1. Signature matching: everthing specified in Sig must be defined in Mod with the right types
2. **Encapsulation: only what is specified in Sig can be accessed outside Mod. The module is sealed**

---

**Singnature matching** : 

- Everything name specified in Sig must be defined in Mod
- The types in Mod must be **the same** as those in Sig **or** **more general**

e.g.

```ocaml
module type IntFun = sig
    val f : int -> int
end

module SuccFun : IntFun = struct
    let f x = x + 1
end

module IdFun : IntFun = struct
    let f x = x (* **more general** *)
end
```

- If types defined in `Mod` is more general than it in `Sig`, it will be constrained by types defined in `Sig`.

## 5.11 Abstract Type

```ocaml
module type Stack = sig
    type 'a stack
end
```

- `'a stack` is abstrct : signature declares only that it **exists**, but does **not define** what it is
- Every `module` of `type Stack` **must define** the abstract `type`

***Abstract types give encapsulation***

- Clients don’t need to know queue is implemented with list
    - Clients will exploit knowledge of representation if you let them
- What if implementers want to upgrade to two-list queues(see 5.7 and 5.6 ) ?
    - If list implementation is hidden, they can freely change

e.g.

```ocaml
module type Stack = sig
    type 'a stack
    exception Empty
    val empty : 'a stack
    val is_empty : 'a stack -> bool
    val push : 'a -> 'a stack -> 'a stack
    val peek : 'a stack -> 'a
    val pop : 'a stack -> 'a stack
    val size : 'a stack -> int
end

module ListStack : Stack = struct
    type 'a stack = 'a list
    exception Empty
    let empty = []
    let is_empty = function [] -> true | _ -> false
    let push x s = x :: s
    let peek = function [] -> raise Empty | x :: _ -> x
    let pop = function [] -> raise Empty | _ :: s -> s
    let size = List.length
end
```

## 5.12 Compilation Units

*for now, we put all the code in one source file : xxx.ml*

*Now we can **separate the module type and module into two source file** : xxx.ml, xxx.mli.*

**Complitation unit = myfile.ml + myfile.mli**

If myfile.ml has contents DM

[and myfile.mli has contents DS]

then OCaml compiler behaves essentially as though : 

```ocaml
module Myfile [: sig DS end] = struct
    DM
end
```

- *[: sig DS end] is just a anonymous signature*

---

e.g.

```ocaml
(* mystack.mli *)

type 'a t
exception Empty
val empty : 'a t
val is_empty : 'a t -> bool
val push : 'a -> 'a t -> 'a t
val peek : 'a t -> 'a
val pop : 'a t -> 'a t
```

```ocaml
(* mystack.ml *)

type 'a t = 'a list
exception Empty
let empty = []
let is_empty = function [] -> true | _ -> false
let push = List.cons
let peek = function [] -> raise Empty | x :: _ -> x
let pop = function [] -> raise Empty | _ :: s -> s
```

---

## 5.13 Utop with Module

Cont’d 5.12 e.g.

**compile way 1**

*type in utop :* 

`#use "mystack.ml";; (* only run code in mystack.ml line by line *)`

---

**compile way 2**

*type in bash :* 

`ocamlbuild mystack.cmo mystack.cmi`

*we get a _build dir, then type*

`utop`

*enter utop, and type*

`#directory "_build";;`

`#load "mystack.cmo";;`

`Mystack.empty;; (* - : 'a Mystack.t = <abstr> *)`

---

**some knowledges :** 

*we can code in file* .ocamlinit : 

`#directory "_build";;`

`#load "mystack.cmo";;`

*so that once we enter utop, above two lines of code will be auto executed*

*we can type*

`#require “ounit2”;;`

*in utop to use third part library*

## 5.14 Includes

```ocaml
(* algebra.ml *)

module type Ring = sig
    type t
    val zero : t
    val one : t
    val ( + ) : t -> t -> t
    val ( * ) : t -> t -> t
    val ( ~- ) : t -> t
    val string : t -> string
end

module IntRingRep = struct
    type t = int
    let zero = 0
    let one = 1
    let ( + ) = Stdlib.( + )
    let ( * ) = Stdlib.( * )
    let ( ~- ) = Stdlib.( ~- )
    let string = string_of_int
end

module IntRing : Ring = IntRingRep

module FloatRingRep = struct
    type t = float
    let zero = 0.
    let one = 1.
    let ( + ) = Stdlib.( +. )
    let ( * ) = Stdlib.( *. )
    let ( ~- ) = Stdlib.( ~-. )
    let string = string_of_float
end

module FloatRing : Ring = FloatRingRep

module type Field = sig
    include Ring
    val ( / ) : t -> t -> t
end

module IntFildRep = struct
    include IntRingRep
    let ( / ) = Stdlib.( / )
end

module IntRing : Ring = IntRingRep

module FloatRing : Ring = FloatRingRep

module IntField : Field = IntFildRep
```

---

```ocaml
(* utop *)

#use "algebra.ml"

IntRing.zero (* - : IntRing.t = <abst> *)

IntRingRep.zero (* - : int = 0 *)

let x = FloatRing.zero
let y = FloatRing.one
let z = FloatRing.(x + y + one)
let sz = FloatRing.string z

(* val x : FloatRing.t = <abstr>
val y : FloatRing.t = <abstr>
val z : FloatRing.t = <abstr>
val sz : string = "2." *)
```

## 5.15 Include vs Open

```ocaml
module M = struct
    let x = 0
end

module N = struct
    include M
    let y = x + 1
end

module K = struct
    open M
    let y = x + 1
end

(* module M : sig val x : int end
module N : sig val x : int val y : int end
module K : sig val y : int end *)
```

---

`include` vs. `open`

- `open M` :
    - imports definitions from `M`
    - makes them available for local consumption
    - doesn’t export them to outside world
- `include M` :
    - imports definitions from `M`
    - makes them available for local consumption
    - does export them to the outside world

## 5.16 Functors

***Functions are "functions on modules"***

```ocaml
module type X = sig
    val x : int
end

module A : X = struct
    let x = 0
end

(* functor is just like fun : 
let inc = fun (x : int) -> x + 1;;
*)
module IncX = functor (M : X) -> struct
    let x = M.x + 1
end

(* grammer sugar *)
module IncX' (M : X) = struct
    let x = M.x + 1
end

(* module type X = sig val x : int end
module A : X
module IncX : functor (M : X) -> sig val x : int end
module IncX' : functor (M : X) -> sig val x : int end *)

module B = IncX(A) (* module B : sig val x : int end *)
```

Actually we can ignore the :X in module A : X . as long as A can be X, OCaml will happy to let A as a parameter deliver to IncX functor. But :X in functor(M:X) cannot be ignored.

---

**Syntax**

```ocaml
module F (M1 : S1) … (Mn : Sn) = struct 
	...
end
```

*or*

```
Module F = functor (M1 : S1)
        -> ...
        -> functor (Mn : Sn)
            -> struct
            ...
end
```

## 5.17 Standard Library Map

The **`Map module`** has a `functor` in it called **`Map.Make`** which makes a map data structure based on an ***input***, which is itself a ***structure***.

That ***input structure*** is called **`Map.OrderedType`** : (contents from Library Map document)

```ocaml
module type OrderedType = sig .. end (*Input signature of the functor Map.Make. *)
```

---

```ocaml
type t (* The type of the map keys *)
val compare : t -> t -> int (* A total ordering function over the keys. Because the data structure is based on a binary avTree, we should tell
it how to compare it by keys *)
```

The ***output structure*** is called **`Map.S`** : 

```ocaml
module type S = sig .. end (* Output signature of the functor Map.Make *)
```

---

```ocaml
type key (* The type of the map keys. *)
type +'a t (* The type of maps from type key to type 'a. Ignore the + which is too difficult to understand for new learner *)
val add : key -> 'a -> 'a t -> 'a t (* add x y m returns a map containing
the same bindings as m, plus a binding of x to y *)
val remove : key -> 'a t -> 'a t (* remove x m returns a map containing the same bindings as m, except for x which is unbound in the returned map *)
val find : key -> 'a t -> 'a
```

> To understand `type +'a t`, you can see "Variance and value restriction" in the OCaml manual. It's section 5.1.4 in current manual release 4.11. But in CS3110 we don't discuss it.

Now, we can use it

```ocaml
(* daymap.ml *)

type day = Mon | Tue | Wed | Tur | Fri | Sat | Sun

let int_of_day = function
  | Mon -> 1
  | Tue -> 2
  | Wed -> 3
  | Tur -> 4
  | Fri -> 5
  | Sat -> 6
  | Sun -> 7

module DayKey = struct
  type t = day
  let compare day1 day2 = int_of_day day1 - int_of_day day2
end

module DayMap = Map.Make(DayKey)

let m =
  let open DayMap in
  empty
  |> add Mon "Monday"
  |> add Tue "Tuesday"
```

```ocaml
(* utop *)

#use "daymap.ml"
m (* - : string DayMap.t = <abstr> *)
open DayMap;;
mem Mon m (* - : bool = true *)
mem Dun m (* - : bool = false *)
find Mon m (* - : string = "Monday" *)
let m' = add Sun "Sunday" m (* val m' : string t = <abstr> *)
bindings m (* - : (key * string) list = [(Mon, "Monday"); (Tue, "Tuesday")] *)
bindings m' (* - : (key * string) list = [(Mon, "Monday"); (Tue, "Tuesday"); (Sun, "Sunday")] *)
```