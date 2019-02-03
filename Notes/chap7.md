# A Second Look at ML 🐫 

Patterns are basically how ML operates, doing everything from parameter matching to control flow.

```ocaml
fun f n -> n * n;;
(* This function takes one parameter n and squares it. n matches any parameter. *)
fun f (a, b) -> a * b;;
(* This function takes one parameter which is a tuple of a b. (a,b) matches any tuple with two elements  *)
fun a -> fun b -> a * b);;
(* This function technically takes one parameter at each step, with the first function taking in a and the second function taking in b, but really can be called similar to passing two parameters in (ex below) *)
(fun a -> fun b -> a * b) 2 3;;
(* : int = 6 *)
```

- underscore `_` matches anything
- using a constant matches only that constant

```ocaml
let g _ = "yes";;
(* always returns the string "yes" *)
g 9;;
(* : string = "no" *)
g [];;
(* : string = "no" *)
let f 0 = "yes";;
(* only returns "yes" if passed with 0 (below)*)
f 0;;
(* : string = "yes" *)
f 1;;
(* Exception: Match_failure *)
```

