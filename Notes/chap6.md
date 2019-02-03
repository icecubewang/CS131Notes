# Types 💧🔥🍃⚡️🔮🧱🥊🐉🐛

Most programming languages have types. The only ones that do not are those in which all operations are meaningful on all values (i.e. [BCPL](https://en.wikipedia.org/wiki/BCPL)).

But what exactly *is* a type, mathematically?

## Types Are Sets 🎳

*Type* - A set of values or operations.

Types are merely sets of potential/legal values such that any two variables of the same type are encoded in the physical memory the same way (e.g. `char` is encoded in 8-bit ASCII, `int` in 32-bit binary). They are also the sets of potential/legal *operations* such that you can perform the same operation on any two variables of the same type.

## Primitive vs Constructed Types 🧱🏠 

A **primitive** type is any type that the program can use but CANNOT define by itself.

A **constructed** type is any type that the program CAN define for itself, using primitives.

- This does NOT mean "primitives are pre-defined, constructs are user-defined"
- ex: `bool` in ML is pre-defined but NOT primitive because you can implement it yourself using an `int`

The *implementation* of those primitive types can be tricky though. Some languages, like Java, define exactly the values in the `int` set (-(2^31) to (2^31)-1).. Some, like C, allow the compiler to choose depending on the machine.

### Enums

A constructed type usually implemented using integers behind the scenes (C allows you a lot more freedom than ML in this regard).

### Tuples

An *aggregate type* (arrays, lists, sets, strings, lists are all aggregate types) consisting of the product of two sets (i.e. `(int * int)` consists of all values you get from multiplying the set of integers from `-(2^63)` to `(2^63)-1` on a 64-bit machine)

### Arrays

Most languages index from 0. Some however allow you to index with other values like `char`.

### Unions

Union-types allow you to define all the *possibilities* for a set:

```c
union element {
    int i;
    float f;
}
```

```ocaml
datatype element =
	I of int |
	F of real;
```

You get enough space to house the largest type. Remember, you can only have **one** at a time. C is more flexible than ML, allowing you to do this:

```c
union element {
    int a;
    float b;
};

union element e;
e.a = 100;
int x = e.a;
printf("%d\n", x); // 100
float y = e.b;
printf("%f\n", y); // 0.000000
```

As incorrect as it may be! 🙃 

ML forces you to account for every possible value in the union.

#### Discriminated Union

A discriminated union has no business being called that and should instead be called something like "strongly-typed union" or "type-aware union." Basically, it's a union but it knows what type it is. You can use this to do pattern matching later.

![](https://images.slideplayer.com/35/10316056/slides/slide_8.jpg)

![](https://image.slidesharecdn.com/madridfsharp-141029164653-conversion-gate02/95/madrid-f-meetup-introduction-to-f-10-638.jpg?cb=1414601293)

## Subtypes 👨 ➡️ 👦 

A **subtype** is a subset of *values*, but a *superset* of operations. This is because once you restrict the values to a certain domain, you can perform operations that otherwise could not. For example, if you restrict an int to only those characters that have ASCII representations, you can perform a cast to a `char` for that subtype.

- This is analogous to having a sub<u>class</u>. Once you have a `Flyable` class, its child class `Bird` can implement more specialized child functions.

## Type Inference 🔮 

Although most languages support type *annotations*, many still also have type *inference*. Most languages only infer the types of expressions if the types of the operands are known (i.e. `a*0` has type `double` if `a` is a double). However, ML takes it "to extremes," trying to infer the type of literally every expression. Luckily, it's type inference system is so powerful that further annotations are rarely needed. 💯 

## Type Checking ♟ 

Two main types:

1. **Static** type checking - performed at compile time. Determines a type for everything before running. If anything cannot be determined or is determined to be an error, the program will fail to compile (i.e. if `reverse()` takes in a bool and you pass a string).
2. **Dynamic** type checking - performed at *runtime*. Before applying any operation, the language system checks the operands to make sure the types are right. If the types are not right, then throw an error. 
   1. Note that because of this checking before every operation, this slows down dynamically typed languages. Some however take the hit because of the gain in flexibility.

However, like many things in this book, the line between the two is not as neat as you would like. Using subtypes can force statically typed languages into runtime checks and Lisp - a dynamically typed language - makes use of type annotations on occasion.

Completely separate of the static-dynamic metric though, is *strength* of type guarantees.

1. **Strongly**-typed languages undergo rigorous type-checking to ensure that no type-incorrect operation can be performed.

   1. ex: ML, Java

2. **Weakly**-typed languages offer more flexibility in not ensuring "type-incorrect" operations can be performed, but are considered to have "holes" that can produce bugs. 🕷 

   1. ex: C (refer to the union example above)

Like other things, there is no clear cut line, rather "strong" and "weak" are two opposite ends of the spectrum and most languages fall somewhere in the middle (Java is strongly typed, Pascal a "just a bit" weakly typed, but C is considerably weakly typed)

## Type Equivalence

1. **Name equivalence** - two types are equivalent iff they have the same name (i.e. `int` or are the same struct)
2. **Structural equivalence** - two types are equivalent iff they are constructed from the same primitive types

For ex:

```ocaml
type ipair1 = int * real;
type ipair2 = int * real;
```

These two are not equivalent under named equivlance ("ipair1" !== "ipair2") but are under structural (they are both tuples with the first element being an int and the second being a real). ML actually uses structural in this case.