# Polymorphism 👯‍♀️

*Polymorphism* - The ability to operate on multiple types.

For example, the below C function is **not** polymorphic, only accepting `char`s

```c
int f (char a, char b) {
    return a == b;
}
```

However, the following ML function **is**, working on any pair of values of the same type (so long as they are equatable)

```
fun f(a,b) = (a = b);;
```

Polymorphism can take many different forms. Let's look at the first:

## Overloading 🏋️‍♂️

An *overloaded* operator is one that has more than one definition depending on the types of the operands.

- Function names can also be overloaded
- ex: Pascal greatly overloads the `+` operator: integer addition on integers, float point addition on real numbers, concatentation on strings, union on sets/lists
- C++ allows you to overload operators for a class

## Type Coercion 🦎🌈

*Type coercion* - The **implicit** casting of one value of one type to its logical representation in another type

```c
double x;
x = 2;
// here 2 is coerced into 2.0
// this is the same as:
x = (double) 2;
```

Although many coercions are fairly logical to the experienced programmer (i.e. coercing `bool` to `0` or `1` or `char` to its ASCII number), they must be defined in painstakingly detailed ways.

Even worse, the interactions between type coercion and function overloading can produce a complicated set of questions:

```c
int square (int x) { return x * x; }
double square (double x) { return x * x; }
// which should be called if we do this:
square('a');
// C calls the int version bc it's "closer" to a char
// even worse what about:
void f (int x, char y) { }
void f (char x, int y) { }
f ('a', 'b');
// C generates a compiler error
```

## Parametric Polymorphism 🍎🍏 

*Parametric polymorphism* - A function that has a type containing one or more **type variables**

- ex:

```ocaml
fun f(a,b) -> (a = b);;
(* ''a * ''a -> bool *)
```

- This function has parametric polymorphism because its type is `''a * ''a -> bool` where `''a` is the type variable, a **polytype**. If you substitute `int` for `''a` you get a function that takes in a tuple of ints and returns a bool; if you substitute `char`, a function that takes in a tuple of chars.

C++ uses parametric polymorphism with templates

```c++
template<class X> X max (X first, X second) {
    return first > second ? first : second;
}

int max1 = max(3, 5);
// type: int * int -> int
char max2 = max('c', 'd');
// type: char * char -> char

// Here "max" returns whatever type it was given, and both of its parameters must be this type (if u give it two ints, it returns two ints)
```

- Note that behind the scenes, what the compiler really does with templates is create one overloaded version of the template function for each type needed. This results in more code than perhaps was expected after compilation.

### Subtype Polymorphism 🎸 🐍

Remember that a subtype is a *subset* of values, but a *superset* of operations. It follows, then, that any function that expects one type can also work on any subtypes of that type (i.e. any function that expects just an `AmazingMusician` object can also work on a `TaylorSwift` object).

Also note that there is a distinction within polymorphism:

- **Ad-hoc** polymorphism is a function/operator that supports more than one, but a finite number of types
- **Universal** polymorphism supports infinitely many types