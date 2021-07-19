![](https://golang.org/lib/godoc/images/go-logo-blue.svg)

- [1. Hello World](#1-hello-world)
  - [1.1. Running a program](#11-running-a-program)
  - [1.2. Imports](#12-imports)
- [2. Variables](#2-variables)
  - [2.1. Declaration](#21-declaration)
  - [2.2. Basic Types](#22-basic-types)
    - [2.2.1. Type Conversions](#221-type-conversions)
    - [2.2.2. Type Inference](#222-type-inference)
  - [2.3. Constants](#23-constants)
- [3. Functions](#3-functions)
  - [3.1. Named return values](#31-named-return-values)
  - [3.2. Function Values](#32-function-values)
  - [3.3. Function Closures](#33-function-closures)
- [4. Flow Control](#4-flow-control)
  - [4.1. For Loop](#41-for-loop)
    - [4.1.1. For-ever Loop](#411-for-ever-loop)
  - [4.2. While Loop](#42-while-loop)
  - [4.3. If/Else Statements](#43-ifelse-statements)
    - [4.3.1. If with short (init) statements](#431-if-with-short-init-statements)
  - [4.4. Switch](#44-switch)
    - [4.4.1. Switch without condition](#441-switch-without-condition)
  - [4.5. Defer](#45-defer)
- [5. Advanced Types](#5-advanced-types)
  - [5.1. Pointers](#51-pointers)
  - [5.2. Struct](#52-struct)
  - [5.3. Arrays and Slices](#53-arrays-and-slices)
    - [5.3.1. Slice properties](#531-slice-properties)
    - [5.3.2. Make function](#532-make-function)
    - [5.3.3. Appending to a slice](#533-appending-to-a-slice)
  - [5.4. Maps](#54-maps)
    - [5.4.1. Testing if a value is in a map](#541-testing-if-a-value-is-in-a-map)
  - [5.5. Range](#55-range)
- [6. Methods](#6-methods)
  - [6.1. Pointer Receivers](#61-pointer-receivers)
  - [6.2. Methods as Functions](#62-methods-as-functions)

# 1. Hello World

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, World!")
}
```

## 1.1. Running a program

Go is a compiled language. Once you have Go installed, you can compile and run your file as:

```sh
$ go tool compile hello.go
```
However, for testing purposes you can run this command directly:

```sh
$ go run hello.go
```

This will also compile the program and store it temporarily, and clears it after executing it.

## 1.2. Imports

Multiple imports

```go
import (    // Multiple import statements
	"fmt"       // import "fmt";
	"math"      // import "math";
)           // ^ Not good practice
```

# 2. Variables

## 2.1. Declaration

Data type comes after variable name

```go
var foo bool
var bar, baz int

var x, y = 1, 2
    // no need to write type when initialising with a constant value

func main() {
    z := 1
    a, b, c := 1, 2, 3
    ...
}   // ^ short declaration
    // cannot be used outside functions
```

Default values (zero values) are `0, false, ""` for  `int, bool, string` respectively.

## 2.2. Basic Types

```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

More types can be found in [Section 5](#5-advanced-types).

### 2.2.1. Type Conversions

Unlike in C, in Go assignment between items of different type requires an explicit conversion.

```go
var i int = 42              // i := 42
var f float64 = float64(i)  // f := float64(i)
var u uint = uint(f)        // u := uint(f)
```

### 2.2.2. Type Inference

When right side is typed:

```go
var i int
j := i          // int
```

When right side is untyped numerical:
```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```
## 2.3. Constants

```go
const Pi = 3.14 // cannot be declared with `:=` syntax
```

# 3. Functions

```go
func add(x, y int) int {
	return x + y
}
```

A function can also return multiple values

```go
func swap(x, y string) (string, string) {
	return y, x
}
```

## 3.1. Named return values

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return      // <- Naked Return
}
```

Numeric constants are high-precision values. \
An untyped constant takes the type needed by its context.

## 3.2. Function Values

Functions can be passed around like other values. They can be used as function arguments, return values, etc.

## 3.3. Function Closures

A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables.

```go
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}
```
Here, the `adder` function returns a closure. Each instance of the closure is "bound" to its own sum variable.

```go
func main() {
	pos, neg := adder(), adder()
    pos(0)      // 0
    neg(0)      // 0
    pos(1)      // 1
    neg(-1)     // -1
    pos(2)      // 3
    neg(-2)     // -3
}
```

# 4. Flow Control

## 4.1. For Loop

The expression need not be surrounded by `()` but the braces `{}` are required.

```go
sum := 0
// init, condition, post
for i := 0; i < 10; i++ {
	sum += i
}

// init and post are optional
for ; sum < 1000; {
    sum += sum
}
```

### 4.1.1. For-ever Loop
```go
for {
    ...
}
```

## 4.2. While Loop

`for` is Go's `while`

```go
sum := 1
for sum < 1000 {
    sum += sum
}
```

## 4.3. If/Else Statements

Just like `for` loop syntax, in if `if` statements the expression need not be surrounded by `()` but the braces `{}` are required.

```go
if x < 0 {
    ...
} else if {
    ...
} else {
    ...
}
```

### 4.3.1. If with short (init) statements

```go
if x := foo(0); x > 0 {
    ...
}
fmt.Println(x) // error
// ^ scope of the variable is limited to if/else statement
```

## 4.4. Switch

Switch in GO doesn't need a `break` statement, only the first matching result is executed. Cases are evaluated from top to bottom. \
Similar to if and for, `()` are not required, `{}` are.

```go
switch z {
    case 1:
        ...
    case 0:
        ...
    default:
        ...
}
```

The switch variable need not be pre-declared, we can write short statements in switch as well. Also, unlike C, switch cases need not be constants, and can be strings as well.

```go
switch x := foo(0); x {
    case 1:
        ...
    case bar(0):
        ...
    default:
        ...
}
// cannot have multiple (conflicting) types in same switch

switch os := runtime.GOOS; os {
    case "darwin":
        fmt.Println("OS X.")
    case "linux":
        fmt.Println("Linux.")
    default:
        // freebsd, openbsd,
        // plan9, windows...
        fmt.Printf("%s.\n", os)
}
```

### 4.4.1. Switch without condition

It is the same as writing `switch true`.

```go
switch {
    case age <= 12:
        fmt.Println("Child")
    case age < 18:
        fmt.Println("Teenager")
    default:
        fmt.Println("Adult")
}
```

Alternative way of writing `if ... else if ... else`.

## 4.5. Defer

Delays the execution of a function until all surrounding functions are executed. In case of multiple defer statements, LIFO order is followed.

```go
func main() {
 
    fmt.Println("Start")
    // Multiple defer statements (LIFO)
    defer fmt.Println("World")
    defer fmt.Println("Hello")
    fmt.Println("End")
    // ^ prints
    //  Start ⏎ End ⏎ Hello ⏎ World
}
```
# 5. Advanced Types

## 5.1. Pointers

Similarities with C:

```go
var p *int      // declaration

i := 42
p = &i          // assignment

fmt.Println(*p) // dereferencing
*p = 21 // (i = 21)
```

Difference from C: **GO doesn't have pointer arithmetic.**

## 5.2. Struct

Declaration:
```go
type Vertex struct {
	X int
	Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)
```
Just like other variables, `&` returns the pointer to the struct value.

Accessing a field:
```go
func main() {
	v := Vertex{1, 2}
    p := &v
    p.X = 1e6
    v.Y = 4
	fmt.Println(v)  // {1000000 4}
}
```
Go syntax allows accessing a field through a pointer directly. \
(`p.X` is shorthand for `(*p).X`, which is allowed in Go without dereferencing explicitly.)

## 5.3. Arrays and Slices

```go
var a [10]int
```

Array slicing is allowed in go. For example,

```go
a[1:4]
```

Just like python,
- Lower limit is included, upper limit is excluded.
- Slices are references to the original arrays.
- We can use `a[:10], a[0:], a[:]`, etc.

Note: **A slice can be seen as a dynamically-sized array.** This makes slices very commonly used.

### 5.3.1. Slice properties

A slice has both a length and a capacity. \
The *length* of a slice is the number of elements it contains.
The *capacity* of a slice is the number of elements in the underlying array, counting from the first element in the slice.

```go
func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	// t=0

	// Slice the slice to give it zero length.
	s = s[:0] // t=1

	// Extend its length.
	s = s[:4] // t=2

	// Drop its first two values.
	s = s[2:] // t=3
}
```
| t     | `s`               | `len(s)`  | `cap(s)`  |
| ----- | ----------------- | --------- | --------- |
| t = 0 | [2 3 5 7 11 13]   | 6         | 6         |
| t = 1 | [ ]               | 0         | 6         |
| t = 2 | [2 3 5 7]         | 4         | 6         |
| t = 3 | [5 7]             | 2         | 4         |

Note:
- The "zero-value" for a slice is `nil`. It has 0 len and 0 cap.
- Slices can contain any type, including other slices. Declaration syntax:
```go
board := [][]string{
    []string{"_", "_", "_"},
    []string{"_", "_", "_"},
    []string{"_", "_", "_"},
}
```


### 5.3.2. Make function

The `make` function can be used to create dynamic-sized arrays.

```go
a := make([]int, 5)     // len(a)=5
b := make([]int, 0, 5)  // len(b)=0, cap(b)=5
```

### 5.3.3. Appending to a slice

```go
var s []int             // [ ]
s = append(s, 2, 3, 4)  // [2, 3, 4]
```

## 5.4. Maps

Basically, dictionaries. Zero value of a map is `nil`, with no keys or values. Maps can also be initialised using `make` function.

```go
m := make(map[string]Vertex)
// keys: string, values: vertex
m["IIIT-H"] = Vertex{
	17.4449, 78.3498
}
```

Alternative ways:

```go
var m = map[string]Vertex{
	"IIIT-H": Vertex{
		17.4449, 78.3498
	},
}
// Top level type can be omitted
var m = map[string]Vertex{
	"IIIT-H":    {17.4449, 78.3498},
}
```

### 5.4.1. Testing if a value is in a map

```go
elem, ok := m[key]
```
If `key` is in `m`, `ok` is `true`. If not, `ok` is `false`.

If `key` is not in the map, then `elem` is the zero value for the map's element type.

## 5.5. Range

(`enumerate` in python) Can be used for either slices or maps.

```go
var s = []int{1, 2, 4, 8, 16, 32, 64, 128}

for i, v := range s {
    ...
}
```
Alternative ways of using `range`:
``` go
for i, _ := range s {       ...
for _, value := range s {   ...
for i := range s {          ...
```

# 6. Methods

Methods can be defined on types. This type is passed to the method as a *receiver* argument. \
Methods are functions with a special argument list before the method name.

```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Dist() float64 {
    x := v.X
    y := v.Y
	return math.Sqrt(x*x + y*y)
}

func main() {
    v := Vertex{3, 4}
    v.Dist()     // 5
}
```

Methods can only be defined on receiver whose types are defined in the same package, hence they cannot be defined on built-in methods either. \
If you wish to define methods on non-structs, you can do it as well, for example:

```go
type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}
```

## 6.1. Pointer Receivers

Methods can have pointers as receivers. (another way of saying "pass by reference", meanwhile value receivers are "pass by value"). In case of methods, pointer receivers are more common.

```go
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	v.Scale(10)     // {30 40}
	v.Dist()        // 50
    p := &v
    p.Scale(5)      // {150, 200}
    // ^ interpreted as (*p).Scale(5)
}
```
Note that there is no difference when calling the method. Just like struct fields, struct methods can also work with or without explicit dereferencing., which is not true for functions, as we will see.

## 6.2. Methods as Functions

Note that writing these methods is the same as writing these functions:

```go
func Dist(v Vertex) float64 {
    x := v.X
    y := v.Y
	return math.Sqrt(x*x + y*y)
}

func Scale(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
    v := Vertex{3, 4}
    Scale(&v, 10)   // {30 40}
    Dist(v)         // 5
}
```
Notice the differences in calling the functions.
