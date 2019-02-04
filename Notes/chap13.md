# A First Look at Java ☕️

Java is a very object-oriented, imperative language. It operates by segmenting a given program into small bundles of data called *objects* that "know" how to do things.

## Definitions

- Object - An instance of a class
- Class - A declaration/definition of the fields and methods of a certain type of object
- Field - A member property of an object
- Method - A member function of an object

Let's implement Points that appear on our screen in a certain color

## Point 🎨 

```java
public class Point {
	// FIELDS
    private int x, y;
    private Color m_color;
    
    // METHODS
    public int currentX() {
        return x;
    }
    public int currentY() {
        return y;
    }
    public void move(int newX, int newY) {
        x = newX;
        y = newY;
    }
}
```

All the stuff here was pretty basic coming from C.

## Methods

There is a difference between an *instance* method and a *class* method

- an instance method operates on the *instance*, the actually instantiated object from a class
  - ex: if `s` is a `String`, then `s.length()` is an instance method that depends on the reference to `s` specifically
  - if we had another String `r`, `r.length()` would be different
- a class method operates on the *class* "prototype"
  - ex: `String.valueOf(2+2)` returns `"4"` (i.e. the `String` value of 4 (2+2))
  - technically you could also do `s.valueOf()` but it's seen as bad practice since `valueOf` comes from the String class and has nothing to do with `s`

## Objects

Created using `new <class_name>()` (i.e. `new String()`)

Remember there is no `delete`! Java has a garbage collector 🗑 👍 

