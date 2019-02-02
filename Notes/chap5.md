# A First Look at ML 🐪

Standard Meta-Language (SML (AKA ML)) is one of the most popular functional languages. It is secure, has a garbage collector, and avoids crashes. However, it is not employer-bait. You will rarely see ML asked for on a job posting.

Thus, we learn ML not to impress recruiters or pad our resumes, but to be better, more intelligent, and more sophisticated programmers. Or something like that... 🤓 

## Type Inference

ML has an "unusually powerful" type inference system. This allows it to better prevent runtime errors and to guarantee reliability and consistency. ✅

In the interpreter, ML always keep a variable `it` that stores the value and type of the most recently typed in expression.

Some common ML types:

- `int`
- `real`
  - analogous to float
  - are not an "equality type" (i.e. you cannot test if a real number is equal to anything because ML knows that floating points are stored in imprecise ways and thus will not ✋🛑 guarantee anything)
- `bool`
- `string`
  - declared with double-quotes
  - `"Taylor"` is a string
- `char`
  - declared with double quotes and hashtag
  - `"T"` is a string
  - `#"T"` is a char

ML also uses short-circuiting operators like most other languages (i.e. if you have an `or` you stop after the first `true` and vice-versa for `and`)

## Conditionals 🔮 

Make sure you know the difference between an `if-else` **statement**:

```
if (1 < 2) {
    // do this
} eles {
    // do that
}
```

and an `if-else` **expression**:

```ocaml
if 1 < 2 then #"x" else #"y";
- val it = #"x" : char
```

The difference being that an if-statement provides *control flow*, an if-expression is a *value*

- Basically the ternary operator in other languages

## Type Conversion/Coercion

There is none. Absolutely none. Not even `1.0 * 2`. No operators are overloaded. Nothing. Don't even try.

(It does have casting functions though)

## Variables 📦

Use `val` keyword. 

You can reassign but it does not actually overwrite the previous value, so any code that used it before will keep its old value. 😱 

## Collections 👥

ML supports **tuples** and **lists**.

### Tuples

```ocaml
val barney = (1+2, 3.0*4.0, "brown");
- val barney = (3, 12.0, "brown") : int * real * string
```

This is a tuple of type `int * real * string`. Note that ML and its derivatives use the `*` symbol as a *type constructor*, not for multiplication.

Note that parentheses matter:

- `string * (int * int)` - This is a tuple where the first element is a string and the second element is itself a tuple of two ints
- `(string * int) * int` - This is a tuple where the first element is a tuple of one string and one int and the second element an int.
- `string * int * int` is a tuple/triple where the first element is a string, and the second and third elements are ints.

Tuples are heterogenous.

Fixed length.

### Lists

Homogenous.

Variable length.

Formed with `[]`

```ocaml
[1, 2, 3];
- val it = [1,2,3] : int list
```

Similar to tuples `*`, lists use `list` as a type constructor.

## Functions

Finally! What we're here for.

- Declared using the `fun` keyword
- `->` means "returns"
- Always take **one parameter** only
  - That parameter can be a tuple or a list, but that is still only one parameter
- Iterative loops to imperative languages are what recursive functions are to functional

## Types 💧🔥🍃⚡️ 

You can have **polymorphic types** defined as `a'`. For example:

```ocaml
fun length x =
	if null x then 0
	else 1 + length (tl x);
- val length = fn : a' list -> int
```

In particular, the type of the function that the interpreter gives us (`a' list -> int`) specifies that we have a polymorphic function. It will accept a list of anything and return an integer (the length).

You may need to explicitly set the return type sometimes to overcome ML's natural type inference.