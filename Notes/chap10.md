# Scope 🕳 

Only the oldest programming languages like Fortran and Basic force you to come up with unique variable names. Every other uses the concept of *scope* to allow programs to be easier to read and write.

Whenever there is more than one variable with the same name, you are dealing with scope. To allow scope, the system must correctly **bind** every occurence of a *name* to the right *variable*. We have a notion of "possible bindings" for every name; that is, the set of all possible references that a name can be bound to.

For example:

```
fun square square = square * square;
```

Here `square` has two possible bindings: to the function named `square` or to the parameter `square`. Which one to use depends on the scope of the definitions.

However, there are many different ways to implement such a concept.

## Block Scoping 📦 

*Block* - Any language construct that contains definitions and the region of the program in which those definitions apply/are scoped/are used.

A block associates **definitions** with a particular **region** of the program text.

```ocaml
let
	val x = 1;
	val y = 2;
in x + y
```

This is one **block**: we defined all our variables and this is the region in which they are used.

Another, more complicated example:

```ocaml
fun f (a :: b :: _) = a + b
| f [a] = a
| f [] = 0;
```

Here we have two definitions of `a` in two different scopes.  The above could as well have been written as:

```
fun f (a :: b :: _) = a + b
| f [a'] = a'
| f [] = 0
```



In C, the region for blocks is more-defined, using `{ } ` to demarcate.

### "Classic block scope rule" 📜 

> The scope of a definition is the block containing that definition, from the point of definition to the end of the block, excluding any re-definitions in nested blocks.

## Labeled Namespaces 🏠 

A labeled namespace is like a block but it has a name that can be used to access the definitions outside the block.

- ex: modules, packages, static variables, literally `namespace` in C

```java
public class Month {
    public static int min = 1;
    public static int max = 12;
}
```

Here you can call `Month.min` from anywhere in the program and get `1`, even though you are not in the scope of the `Month` class.

### Pros 👍 

- Reduce "namespace pollution" 🏭 - the problem you have finding unique variable names after introducing too many variables in too broad a scope (i.e. usually any variable name you have to append 2 to)
- Allow you to use the simple name within the block (i.e. you can just say `min` in you're in the scope of `Month`)
- Usually allow private implementations within that namespace/block/class 🔒 

## Primitive Namespaces 🧱 

To keep things working even in cases like this:

```ocaml
fun f int = int * int;
(* val f = fun : int -> int *)
```

```java
class Reuse {
    Reuse Reuse(Reuse Reuse) {
        Reuse:
        	for (;;) {
                if (Reuse.Reuse(Reuse) == Reuse) {
                    break Reuse;
                }
        	}
        	return Reuse;
    }
}
```

Most languages have something called a **primitive namespace**, a second namespace for types that cannot be explicitly created using the language itself but rather exists in the very definition of the language.

- This also brings up the fact that, unlike blocks which are continuous, namespaces can be *fragmented*, two adjacent names can belong to different scopes.

## Dynamic Scoping

Determined at runtime. 🏃 

If a definition is not found within the current function, search it's caller and so on until you find one.

### "Classic dynamic scope rule" 📜 

> The scope of a definition is the function containing that definition from the point of definition to the end of the function, excluding any re-definitions of the same name in other functions called by the function.

### Static vs Dynamic

- Block scoping refers only to the literal program text inside the block and is thus easy to determine at compile time.
- Dynamic scoping refers to the events that happen at runtime (i.e. a function is called) and is thus impossible to determine until runtime.

### A Question 👀 

What does `g 5` return in the following implementation of `g` assuming block-scoping?

```
fun g x =
	let
		val inc = 1;
		fun f y = y + inc;
		fun h z =
			let
				val inc = 2;
			in
				f z
			end;
	in
		h x
	end;
```

...

...

...

If you answered `6` you are correct! The value of `inc` being `2` only applies in the scope of the `let...in...` block for the `h` function. It does NOT extend outwards into the scope of the `let...in...` block for the `g` function.

However, if the above was dynamic scoped, the answer would be `7`. This is because `inc` is not declared inside `f` and thus you search in the caller (`h`) for a definition of `inc` and find one equal to 2.

This example I hope demonstrates why dynamic scoping is **rarely used**.

### Cons 👎 

- Inefficient due to binding lookup at runtime
- Leads to large, often complicated scopes since the entire current scope extends into any function calls.