# Lecture 1 — Basic Syntax and Variables

## 1.1 Introduction

In this lecture, we begin writing Julia programs.

We will learn the basic building blocks of Julia:

* comments
* expressions
* statements
* variables
* assignment
* basic data types
* arithmetic operations
* comparison operators
* logical operators
* type inspection
* type conversion
* constants
* string basics
* printing output

All examples are written so that they can be executed directly in Google Colab using the Julia workflow introduced in Lecture 0.5.

---

# 1.2 Comments

Comments are text written in a program that Julia does not execute.

A single-line comment begins with `#`.

```python
#Comments
%%bash
julia <<'JULIA'
# This is a comment

x = 10

# Julia executes the assignment above
println(x)
JULIA
```

Comments are useful for explaining what a section of code does.

---

# 1.3 Expressions and Statements

An **expression** is something Julia can evaluate to produce a value.

For example:

```text
2 + 3
```

produces:

```text
5
```

An assignment is another example of an expression:

```text
x = 10
```

It assigns the value `10` to `x`.

We can execute several expressions sequentially:

```python
#Expressions and statements
%%bash
julia <<'JULIA'
2 + 3

x = 10

y = 20

println(x)
println(y)
println(x + y)
JULIA
```

Julia does not require semicolons at the end of statements.

---

# 1.4 Variables

A variable is a name associated with a value.

For example:

```python
#Variables
%%bash
julia <<'JULIA'
x = 10
y = 20

println(x)
println(y)

z = x + y

println(z)
JULIA
```

Here:

```text
x = 10
y = 20
z = x + y
```

creates three variables.

The value of a variable can be used in subsequent expressions.

---

# 1.5 Variable Reassignment

A variable can generally be assigned a new value.

```python
#Variable reassignment
%%bash
julia <<'JULIA'
x = 10

println(x)

x = 20

println(x)

x = 50

println(x)
JULIA
```

The variable `x` first refers to `10`, then `20`, and finally `50`.

---

# 1.6 Variable Names

Julia variable names can contain letters, digits, underscores, and many Unicode characters.

A variable name cannot begin with a digit.

For example:

```python
#Variable names
%%bash
julia <<'JULIA'
number = 10
student_name = "Julia"
x1 = 25

println(number)
println(student_name)
println(x1)
JULIA
```

Julia is case-sensitive.

Therefore:

```text
x
```

and

```text
X
```

are different variables.

```python
#Case sensitivity
%%bash
julia <<'JULIA'
x = 10
X = 20

println(x)
println(X)
JULIA
```

---

# 1.7 Basic Data Types

Julia has a rich type system.

Some basic types that we will encounter frequently are:

* `Int`
* `Float64`
* `Bool`
* `String`
* `Char`

We can inspect the type of a value using `typeof()`.

```python
#Basic data types
%%bash
julia <<'JULIA'
x = 10
y = 3.14
z = true
name = "Julia"
letter = 'J'

println(typeof(x))
println(typeof(y))
println(typeof(z))
println(typeof(name))
println(typeof(letter))
JULIA
```

Typical results are:

```text
Int64
Float64
Bool
String
Char
```

The exact integer type can depend on the platform.

---

# 1.8 Integers

Integers represent whole numbers.

```python
#Integers
%%bash
julia <<'JULIA'
x = 10
y = -25
z = 0

println(x)
println(y)
println(z)

println(typeof(x))
JULIA
```

---

# 1.9 Floating-Point Numbers

Floating-point numbers represent numbers containing a fractional part.

```python
#Floating-point numbers
%%bash
julia <<'JULIA'
x = 3.14
y = -2.5
z = 1.0

println(x)
println(y)
println(z)

println(typeof(x))
JULIA
```

The commonly used double-precision floating-point type in Julia is `Float64`.

---

# 1.10 Boolean Values

A Boolean has one of two values:

```text
true
false
```

```python
#Boolean values
%%bash
julia <<'JULIA'
x = true
y = false

println(x)
println(y)

println(typeof(x))
JULIA
```

Booleans are particularly important for conditions and control flow, which we will study in Lecture 2.

---

# 1.11 Strings

A string is a sequence of characters enclosed in double quotation marks.

```python
#Strings
%%bash
julia <<'JULIA'
name = "Julia"
message = "Hello, Julia!"

println(name)
println(message)

println(typeof(name))
JULIA
```

We will study strings and string manipulation in much greater detail later in the course.

---

# 1.12 Characters

A single character is represented using single quotation marks.

```python
#Characters
%%bash
julia <<'JULIA'
letter = 'A'

println(letter)
println(typeof(letter))
JULIA
```

A `Char` represents a single character, whereas a `String` represents a sequence of characters.

---

# 1.13 Arithmetic Operators

Julia supports the standard arithmetic operators.

| Operator | Meaning        |
| -------- | -------------- |
| `+`      | Addition       |
| `-`      | Subtraction    |
| `*`      | Multiplication |
| `/`      | Division       |
| `^`      | Exponentiation |
| `%`      | Remainder      |

Example:

```python
#Arithmetic operations
%%bash
julia <<'JULIA'
a = 10
b = 3

println(a + b)
println(a - b)
println(a * b)
println(a / b)
println(a ^ b)
println(a % b)
JULIA
```

---

# 1.14 Integer Division

The operator `÷` performs integer division.

```python
#Integer division
%%bash
julia <<'JULIA'
a = 10
b = 3

println(a ÷ b)
JULIA
```

The result is the integer quotient.

---

# 1.15 Operator Precedence

Julia follows mathematical precedence rules.

For example:

```python
#Operator precedence
%%bash
julia <<'JULIA'
x = 2 + 3 * 4
y = (2 + 3) * 4

println(x)
println(y)
JULIA
```

The multiplication is performed before addition in the first expression.

Parentheses can be used to explicitly control the order of evaluation.

---

# 1.16 Comparison Operators

Comparison operators compare values.

| Operator | Meaning               |
| -------- | --------------------- |
| `==`     | Equal                 |
| `!=`     | Not equal             |
| `<`      | Less than             |
| `>`      | Greater than          |
| `<=`     | Less than or equal    |
| `>=`     | Greater than or equal |

Example:

```python
#Comparison operators
%%bash
julia <<'JULIA'
x = 10
y = 20

println(x == y)
println(x != y)
println(x < y)
println(x > y)
println(x <= y)
println(x >= y)
JULIA
```

The results are Boolean values.

---

# 1.17 Logical Operators

Julia provides logical operators for working with Boolean expressions.

The main operators are:

* `&&` — logical AND
* `||` — logical OR
* `!` — logical NOT

```python
#Logical operators
%%bash
julia <<'JULIA'
a = true
b = false

println(a && b)
println(a || b)
println(!a)
println(!b)
JULIA
```

These operators become especially important when constructing conditions.

---

# 1.18 Checking Types with `typeof`

We can inspect the type of any Julia expression using `typeof()`.

```python
#Checking types
%%bash
julia <<'JULIA'
x = 10
y = 10.0
z = true

println(typeof(x))
println(typeof(y))
println(typeof(z))

println(typeof(x + y))
JULIA
```

Type inspection will become increasingly important as we study Julia's type system.

---

# 1.19 Type Conversion

Julia provides functions for converting values between compatible types.

For example:

```python
#Type conversion
%%bash
julia <<'JULIA'
x = 10

y = Float64(x)

println(y)
println(typeof(y))
JULIA
```

We can also convert a floating-point value to an integer when appropriate:

```python
#Integer conversion
%%bash
julia <<'JULIA'
x = 10.0

y = Int(x)

println(y)
println(typeof(y))
JULIA
```

Conversion is not the same thing as simply inspecting a type.

`typeof()` tells us what type a value already has.

A conversion function attempts to produce a value of another type.

---

# 1.20 Explicit Type Annotations

Julia allows us to explicitly specify a variable's type.

For example:

```python
#Type annotations
%%bash
julia <<'JULIA'
x::Int = 10

println(x)
println(typeof(x))
JULIA
```

Type annotations become much more important when we study Julia's type system and multiple dispatch in later lectures.

---

# 1.21 Constants

A variable can be declared as a constant using `const`.

```python
#Constants
%%bash
julia <<'JULIA'
const SPEED_OF_LIGHT = 299792458

println(SPEED_OF_LIGHT)
JULIA
```

A constant indicates that the binding is intended not to be reassigned.

Constants are particularly useful for mathematical and physical parameters that should remain fixed.

For example:

```python
#Physical constant
%%bash
julia <<'JULIA'
const c = 299792458

println("Speed of light = ", c, " m/s")
JULIA
```

---

# 1.22 Printing Output

The `println()` function prints a value and then moves to a new line.

```python
#Printing output
%%bash
julia <<'JULIA'
x = 10

println(x)
println("Hello Julia")
println("x = ", x)
JULIA
```

Multiple values can be passed to `println()`.

This is useful when displaying results from calculations.

---

# 1.23 Basic Mathematical Expressions

Julia can be used directly as a mathematical computing language.

For example:

```python
#Mathematical expressions
%%bash
julia <<'JULIA'
x = 2
y = 3

a = x^2 + y^2
b = x * y + 5

println(a)
println(b)
JULIA
```

This simple expression-based syntax is one of the reasons Julia is convenient for scientific computing.

---

# 1.24 A Complete Example

The following example combines the concepts introduced in this lecture.

```python
#Basic Julia program
%%bash
julia <<'JULIA'
name = "Julia"
x = 10
y = 5.0
active = true

println("Name: ", name)
println("x = ", x)
println("y = ", y)
println("Active: ", active)

println("x + y = ", x + y)
println("x - y = ", x - y)
println("x * y = ", x * y)
println("x / y = ", x / y)

println("Type of x: ", typeof(x))
println("Type of y: ", typeof(y))
println("Type of active: ", typeof(active))
JULIA
```

---

# 1.25 Practice Exercises

Try modifying the examples above to solve the following.

### Exercise 1

Create variables for:

* your name
* your age
* your height
* whether you are a student

Print all four values and their types.

### Exercise 2

Create two variables `a` and `b` and calculate:

$$
a+b,\qquad
a-b,\qquad
ab,\qquad
\frac{a}{b},\qquad
a^b.
$$

### Exercise 3

Create two numerical variables and determine whether:

* they are equal
* the first is greater than the second
* the first is less than the second.

### Exercise 4

Create an integer and convert it to `Float64`.

Print both the value and its type.

### Exercise 5

Create a mathematical expression involving several variables and evaluate it.

---

# 1.26 Summary

In this lecture, we introduced the basic building blocks of Julia programming.

We learned:

* comments
* expressions
* statements
* variables
* variable reassignment
* variable naming
* integers
* floating-point numbers
* Booleans
* strings
* characters
* arithmetic operators
* integer division
* operator precedence
* comparison operators
* logical operators
* `typeof()`
* type conversion
* type annotations
* constants
* `println()`

The central idea is simple:

```text
Values
  ↓
Variables
  ↓
Expressions
  ↓
Operations
  ↓
Output
```

These concepts form the foundation for everything that follows in Julia.

**Next: Lesson 2 — Control Flow**
