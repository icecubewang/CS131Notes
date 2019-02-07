# Memory 🧠 Locations 📍 for Variables

## Dynamic 🌊

**Dynamic/activation specific** variables are initialized only upon entry inside the block they are defined in

```ocaml
let days2ms days =
	let hours = days * 24 in
	let minutes = hours * 60 in
	let seconds = minutes * 60 in
	seconds * 1000
;;
```

Above `hours`,`minutes`, and `days` are all specific to the call of the `days2ms` function. If the function is never called, they are never created (i.e. allocated space and given an address).

```ocaml
let fact n =
	if (n = 0)
	then 1
	else let b = fact (n - 1) in
	n * b
;;
```

Here `b` is only initialized not when `fact` is called, but when the `else` block is entered.

## Static ⚡️

**Static** variables, on the other hand, are created once and occupy the same memory location the entire run of the program.

```c
int count = 0;
int nextcount() {
    count = count + 1;
    return count;
}
```

Here `count` is both static AND global (they are NOT the same thing). Static just means the variable occupies the same address; global means any part of the program can access it (i.e. functions besides `nextcount`).

- If you call it twice in a row you get: `0 1`

```c
int nextcount() {
    static int count = 0;
    count = count + 1;
    return count;
}
```

Here, `count` is static but LOCAL (not global) and can thus only be accessed inside the scope of `nextcount`.

- If you call it twice in a row you get `0 1`

```c
int nextcount() {
    int count = 0;
    count = count + 1;
    return count;
}
```

Here `count` is dynamic and local. It is reallocated space and a new address every function call.

- If you call it twice in a row you get `1 1`

## Activation Records 🗄 

**Activation records/stack frames** are the collections of all activation-specific variables and data packaged into one block of memory. 🧠 

### Static Allocation

Use the same block of memory for every call of a specific function.

#### How It Works 🛠 

You have the following function in Fortran:

```fortran
FUNCTION AVG (ARR, N)
DIMENSION ARR(N)
SUM = 0.0
DO 100 I = 1, N
	SUM = SUM + ARR(I)
100 CONTINUE
AVG = SUM / FLOAT(N)
RETURN
END
```

Assuming Fortran uses the two words for each float (SUM and AVG), one word for each int (`I`), and one word for each address (ARR, N, return address), we need (2\*2) + 1 + (1\*3) = 8 words.

This 8 word space is allocated and assigned at compile time and every time `AVG` is invoked, it reuses this block. Simple! 😎 

#### Pros 👍 

- Efficiency 🏃- No allocation at runtime
- Simplicity - "Easiest" implementation of stack frames
- Oldest - Well-established

#### Cons 👎 

- *Cannot multiple simultaneous function calls* (of the same function)
  - i.e. **NO RECURSION** (and likely no multi-threading?)

### Dynamic Allocation

"The stack" in C/++.

During runtime, at function invocation a new activation record is created. Upon function return, the block is deallocated.

- Records/frames are pushed onto the stack on call and popped off on return

Since the addresses are not predetermined at compile time, we use a machine register to store the return address (rip).