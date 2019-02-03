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

You get enough space to house the largest type. Remember, you can only have **one** at a time.

### Discriminated Union

