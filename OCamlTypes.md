### Primitives
```ocaml
(* int *)
# 117;;
- : int = 117

(* floats are distinguished by adding a decimal point. *)
# 3.14;;
- : float = 3.14

# 3.;;
- : float = 3.

(* char type primitives are delimited by single quotes. *)
# 'c';;
- : char = 'c'

(* string type primitives are delimitied by double quotes. *)
# "foobarbaz";;
- : string = "foobarbaz"

(* bool type primitives are lower-case. *)
# true;;
- : bool = true

(* The unit type consists of only a single value. *)
# ();;
- : unit = ()
```

### Declaring types
```ocaml
(* Type declarations start with the keyword `type`, followed by a name, 
 * followed by an `=` and the type definition. Type declarations are also terminated by a `;;`. *)

# type flag = bool;;              
type flag = bool

(* Once a type is declared, you can assert that a value has that type by labelling it. *)
# let running: flag = true;;
val running : flag = true

# type dollars = float;;
type dollars = float

# let my_bank_balance: dollars = 1100.56;;
val my_bank_balance : dollars = 1100.56
```

### Tuples
```ocaml
(* A tupled type is represented by its sub-types, each separated from one another by asterisks. *)

# type my_tuple = int * float * char * string * bool * unit;;
type my_tuple = int * float * char * string * bool * unit

# let my_tuple_value: my_tuple = (1, 2., 'd', "x", true, ());;
val my_tuple_value : my_tuple = (1, 2., 'd', "x", true, ())

(* You can use destructuring assignment to pull values out of a tuple. *)
# let (my_int, my_float, my_char, my_string, my_bool, my_unit) = my_tuple_value;;
val my_int : int = 1
val my_float : float = 2.
val my_char : char = 'd'
val my_string : string = "x"
val my_bool : bool = true
val my_unit : unit = ()

# type rational_number = int * int;;
type rational_number = int * int

# let my_pi: rational_number = (22, 7);;
val my_pi : rational_number = (22, 7)

# let ((numerator, denominator): rational_number) = my_pi;;
val numerator : int = 22
val denominator : int = 7
```

### Records
```ocaml
(* A record type is like a tuple, in that it aggregates a number of sub-types under one name.
 * However, unlike a tuple, each sub-type has a corresponding label.*)
 
(* Records are delimited by curly brackets, inside of which a set of (label, type) pairs
 * are recorded. labels and types are separated by a colons, and the pairs themselves are
 * separated by semicolons. *)
 
# type bank_statement = {
#   savings: dollars;
#   checking: dollars
# }
type bank_statement = { savings : dollars; checking : dollars; }

(* The type declaration and value declarations for records are nearly identical. In
 * value declarations, the (label, value) pairs are separated by `=`. *)
 
# let my_bank_statement = {
#   savings = 100.56;
#   checking = 1000.;
# }
val my_bank_statement : bank_statement = {savings = 100.56; checking = 1000.}

(* If you have variables `savings` and `checking` in context, you can use a shorthand... *)

# let savings = 100.56;;
val savings: float = 100.56;;
# let checking = 1000.;;
val checking: float = 1000.
# let my_bank_statement = { savings; checking };;
val my_bank_statement : bank_statement = {savings = 100.56; checking = 1000.}

# let checking = 1000.;;
val checking : float = 1000.

# let my_bank_statement = { savings; checking };;
val my_bank_statement : bank_statement = {savings = 100.56; checking = 1000.}

(* You can use a label to retrieve a value from a record. *)

# let my_checking = my_bank_statement.checking;;
val my_checking : dollars = 1000.
```

### Lists: A Parameterized Type
```ocaml
(* Lists are collections of values, separated by semicolons, and delimited by square brackets. 
 * Unlike lists in other languages, OCaml lists must contain values all having the same type. *)

# let googl_closing_prices = [943.46; 941.48; 951.99; 955.24; 943.63];;
val googl_closing_prices : float list = [943.46; 941.48; 951.99; 955.24; 943.63]

(* The above is a list of floats. float is the parameterized type of the list. *)

# type dollar_returns = dollars list;;
type dollar_returns = dollars list

(* Any type parameter can be filled by a valid type expression. *)
# type accounts_role = bank_statement list;;
type accounts_role = bank_statement list

# let my_wells_fargo_accounts = [{
#   savings = 100.56;
#   checking = 1000.;
# }; {
#   savings = 10000.;
#   checking = 0.;
# }]
val my_wells_fargo_accounts : bank_statement list =
  [{savings = 100.56; checking = 1000.}; {savings = 10000.; checking = 0.}]
```

### Function Types
```ocaml
(* Function types are represented by two or more sub-types, separated by arrows. *)
# type statement_constructor = float -> float -> bank_statement

(* A function of type `statement_constructor` takes a float and another float, and returns a bank record. *)

# let construct_statement: statement_constructor = 
#   fun checking savings -> 
#     { checking; savings }
let construct_statement: statement_constructor = fun checking savings -> { checking; savings }

# type statement_serializer = bank_statement -> string;;
type statement_serializer = bank_statement -> string

# let my_serializer: statement_serializer = 
# fun a -> 
#   Printf.sprintf "savings: %f, checking: %f" a.savings a.checking;;
val my_serializer : bank_statement -> string = <fun>

# type statement_deserializer = string -> bank_statement;;
type statement_deserializer = string -> bank_statement

# let my_deserializer: statement_deserializer =
#   fun str ->  
#     Scanf.sscanf str "savings: %f, checking: %f" construct_statement
val my_deserializer : string -> bank_statement = <fun>
```

### Inductive Types
```ocaml
(* Inductive types consist of subtypes (possibly the same type), labelled by constructors. 
 * Constructors are capitalized. *)

# type peano_number = 
#     Zero 
#   | Successor of peano_number;;
type peano_number = Zero | Successor of peano_number

# type trie =
#     Leaf
#   | Branch of char * trie list;;
type trie = Leaf | Branch of char * trie list

# type json =
#     Integer of int
#   | Float of float
#   | String of string
#   | List of json list
#   | Object of (string * json) list;;
type json =
    Integer of int
  | Float of float
  | String of string
  | List of json list
  | Object of (string * json) list
  
(* We haven't covered how to parameterize our own types, but it is useful. *)

# type 'a binary_tree =
#     Leaf of 'a
#   | Branch of 'a binary_tree * 'a binary_tree;;
type 'a binary_tree = Leaf of 'a | Branch of 'a binary_tree * 'a binary_tree

# type 'a stream =
#     EndOfStream
#   | Continuation of 'a * (unit -> 'a stream)
type 'a stream = EndOfStream | Continuation of 'a * (unit -> 'a stream)
```
























