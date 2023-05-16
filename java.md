<img src="https://upload.wikimedia.org/wikipedia/en/thumb/3/30/Java_programming_language_logo.svg/1024px-Java_programming_language_logo.svg.png" width="100">


# Hello World

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("This will be printed");
    }
}
```

- When we declare a public class, we must declare it inside a file with the same name (`Main.java`), otherwise we'll get an error when compiling.
- `static` means that you can run this method (`main`, not to be confused with `Main`, the class) without creating an instance of Main (much like `@staticmethod` in python).
- `out` (much like `stdout` in C) is a static variable, with `println` as one of its methods.

# Variables

## Primitive types

```java
byte // 1
short // 2
int // 4
long // 8
float // 4
double // 8
char // 2
boolean // 1
```

## Strings

`String` is not primitive, but Java has special treatment for String.

`str + str2` is an exception in Java (operator overloading, `+` used for non-primitive type). Strings can also be concatenated to primitives.

## Declaration

For most primitive types, declaration is common with C/C++. Some things to keep in mind
- Floats have to be cast explicitly
```java
float f = (float) 4.5; // by default, double
float f = 4.5f; // shorthand to cast floats
```
- Characters are, unlike C, different from numbers
```java
char c = 'j'; // it is not common to put
        // ascii values in char declaration
```
- Booleans only accept `true` or `false` (no implicit conversion). Booleans cannot be added, etc, and non booleans cannot be used in `if` statements.
- Strings can be declared in two ways, and get stored in two different ways – in the Java Heap, or in the String intern pool (within the heap).

**Note: == vs .equals()**: `a.equals(b)` checks logical equality while `a == b` returns true only if they are the same object.