# Lesson 7 — Types and Multiple Dispatch

## Introduction

Julia has a powerful type system that is closely connected to one of the defining features of the language: **multiple dispatch**.

Every value in Julia has a type. Types allow Julia to organize values into a hierarchy and determine which method of a function should be executed.

The central idea of this lesson is:

```text
FUNCTION
   +
ARGUMENT TYPES
   ↓
METHOD SELECTION
   ↓
APPROPRIATE METHOD
```

This mechanism is fundamental to understanding how Julia functions work.

In this lesson, we introduce:

* `typeof`
* `isa`
* Julia's type hierarchy
* Abstract types
* Concrete types
* `supertype`
* `subtypes`
* Type annotations
* Function argument types
* Multiple methods
* `methods`
* Multiple dispatch
* Dispatch with multiple arguments
* General and specific methods
* Parametric types
* `Vector{T}`
* Type parameters
* `where T`
* Generic functions
* User-defined types
* Types and method selection

---

# 1. `typeof`

The `typeof` function tells us the concrete type of a value.

```python id="7f3q2k"
%%bash
julia <<'JULIA'
x = 10
y = 3.14
z = true
c = 'A'

println(typeof(x))
println(typeof(y))
println(typeof(z))
println(typeof(c))
JULIA
```

Typical output is:

```text
Int64
Float64
Bool
Char
```

The type of a value can therefore be inspected directly.

---

# 2. `isa`

The `isa` operator checks whether a value belongs to a particular type.

```python id="5m8q1d"
%%bash
julia <<'JULIA'
x = 10

println(x isa Int)
println(x isa Float64)
println(x isa Number)
JULIA
```

The results are Boolean values.

```text
true
false
true
```

The expression:

```julia
x isa Number
```

is true because `Int` is part of Julia's numerical type hierarchy under `Number`.

---

# 3. Julia's Type Hierarchy

Julia organizes types into a hierarchy.

A simplified part of the hierarchy is:

```text
Any
 │
 ├── Number
 │    │
 │    ├── Real
 │    │    │
 │    │    ├── Integer
 │    │    │    ├── Signed
 │    │    │    └── Unsigned
 │    │    │
 │    │    └── AbstractFloat
 │    │
 │    └── Complex
 │
 └── Other types
```

This hierarchy allows broad categories of values to be described using abstract types.

For example:

```text
Int
   ↓
Integer
   ↓
Real
   ↓
Number
   ↓
Any
```

---

# 4. Abstract Types

An abstract type describes a category of related types.

For example:

```julia id="k9d4fz"
Number
Real
Integer
AbstractFloat
```

are abstract types.

They describe groups of types rather than one particular representation of a value.

For example, both integers and floating-point numbers are numerical types.

```python id="2w6p8n"
%%bash
julia <<'JULIA'
println(Int <: Number)
println(Float64 <: Number)
println(Int <: Real)
println(Float64 <: Real)
JULIA
```

The `<:` operator checks a subtype relationship.

---

# 5. Concrete Types

A concrete type is a type that can have actual instances.

Examples include:

```text
Int64
Float64
Bool
Char
String
```

For example:

```python id="r3n7vx"
%%bash
julia <<'JULIA'
x = 10
y = 3.14

println(typeof(x))
println(typeof(y))
JULIA
```

`Int64` and `Float64` are concrete types.

The distinction is important:

```text
Abstract type
    → describes a category

Concrete type
    → represents an actual data representation
```

---

# 6. `supertype`

The `supertype` function returns the immediate supertype of a type.

```python id="c5v8mz"
%%bash
julia <<'JULIA'
println(supertype(Int))
println(supertype(Float64))
JULIA
```

We can follow the hierarchy upward.

```python id="q8k2ws"
%%bash
julia <<'JULIA'
T = Int

println(T)
println(supertype(T))
println(supertype(supertype(T)))
JULIA
```

This allows us to examine the relationship between types.

---

# 7. `subtypes`

The `subtypes` function shows the direct subtypes of a type.

```python id="m4z7qx"
%%bash
julia <<'JULIA'
println(subtypes(Number))
JULIA
```

We can also examine subtypes of `Real`.

```python id="p9c3vw"
%%bash
julia <<'JULIA'
println(subtypes(Real))
JULIA
```

The exact list can depend on the Julia version and loaded packages.

---

# 8. Type Relationships

Julia provides operators for examining type relationships.

The `<:` operator checks whether one type is a subtype of another.

```python id="n6x4br"
%%bash
julia <<'JULIA'
println(Int <: Integer)
println(Integer <: Real)
println(Real <: Number)
println(Number <: Any)
JULIA
```

This gives a way to reason about Julia's type hierarchy.

---

# 9. Type Annotations

A variable can be annotated with a type.

```python id="w2k7fc"
%%bash
julia <<'JULIA'
x::Int = 10

println(x)
println(typeof(x))
JULIA
```

A type annotation specifies a required type for the value assigned in that context.

Another example:

```python id="j8p4zs"
%%bash
julia <<'JULIA'
x::Float64 = 3.14

println(x)
JULIA
```

Type annotations can also appear in function definitions.

---

# 10. Function Argument Types

A function argument can be restricted to a particular type.

```python id="d7m3qy"
%%bash
julia <<'JULIA'
function square(x::Int)
    return x^2
end

println(square(5))
JULIA
```

This method accepts an `Int` argument.

A different type may require another method.

---

# 11. Multiple Methods

A single Julia function can have multiple methods.

For example:

```python id="f9k5vx"
%%bash
julia <<'JULIA'
function describe(x::Int)
    println("Integer")
end

function describe(x::Float64)
    println("Floating-point number")
end

describe(10)
describe(3.14)
JULIA
```

Both definitions belong to the same function:

```text
describe
```

but they are different methods.

---

# 12. `methods`

The `methods` function displays the methods associated with a function.

```python id="z4q8mc"
%%bash
julia <<'JULIA'
function describe(x::Int)
    println("Integer")
end

function describe(x::Float64)
    println("Floating-point number")
end

println(methods(describe))
JULIA
```

This shows that `describe` has multiple methods.

---

# 13. Multiple Dispatch

Multiple dispatch means that Julia selects a method based on the types of the arguments supplied to a function.

Consider:

```python id="v6p2kr"
%%bash
julia <<'JULIA'
function combine(x::Int, y::Int)
    println("Two integers")
end

function combine(x::Float64, y::Float64)
    println("Two floating-point values")
end

combine(2, 3)
combine(2.0, 3.0)
JULIA
```

Julia examines the types of **both arguments**.

The selected method therefore depends on:

```text
function
   +
type of x
   +
type of y
```

---

# 14. Dispatch with Multiple Arguments

The power of multiple dispatch becomes clearer when different combinations of argument types are defined.

```python id="s7m3nx"
%%bash
julia <<'JULIA'
function operation(x::Int, y::Int)
    println("Integer + Integer")
end

function operation(x::Int, y::Float64)
    println("Integer + Float64")
end

function operation(x::Float64, y::Int)
    println("Float64 + Integer")
end

function operation(x::Float64, y::Float64)
    println("Float64 + Float64")
end

operation(1, 2)
operation(1, 2.0)
operation(1.0, 2)
operation(1.0, 2.0)
JULIA
```

Each call selects the appropriate method.

---

# 15. General and Specific Methods

Julia allows methods to be defined at different levels of generality.

For example:

```python id="b3v8mq"
%%bash
julia <<'JULIA'
function identify(x::Number)
    println("Number")
end

function identify(x::Int)
    println("Integer")
end

identify(10)
identify(3.14)
JULIA
```

For `10`, both methods could conceptually apply:

```text
Int
Number
```

Julia selects the more specific applicable method:

```text
Int
```

For `3.14`, the `Number` method applies.

---

# 16. Specificity

A more specific method is preferred when several methods match the same call.

Consider:

```text
Number
  ↑
Real
  ↑
Integer
  ↑
Int
```

A method accepting `Int` is more specific than one accepting `Number`.

This allows generic behavior to coexist with specialized behavior.

---

# 17. Generic Functions

A generic function is a function whose behavior can be defined through multiple methods for different argument types.

For example:

```python id="q6w9zs"
%%bash
julia <<'JULIA'
function process(x::Int)
    return x + 1
end

function process(x::Float64)
    return x / 2
end

println(process(10))
println(process(10.0))
JULIA
```

The function name is the same:

```text
process
```

but its behavior depends on the argument type.

---

# 18. Parametric Types

Julia supports types with parameters.

A type parameter allows the same general type structure to represent different element types.

A common example is:

```julia id="r8c4my"
Vector{T}
```

where `T` represents the element type.

For example:

```text
Vector{Int}
Vector{Float64}
Vector{String}
```

are all vectors, but with different element types.

---

# 19. `Vector{T}`

Consider several vectors:

```python id="m5v9cx"
%%bash
julia <<'JULIA'
x = [1, 2, 3]
y = [1.0, 2.0, 3.0]
z = ["Julia", "Python"]

println(typeof(x))
println(typeof(y))
println(typeof(z))
JULIA
```

Their types are typically:

```text
Vector{Int64}
Vector{Float64}
Vector{String}
```

The element type is part of the vector's type.

---

# 20. Inspecting Element Types

The `eltype` function returns the element type of a collection.

```python id="c8q2nv"
%%bash
julia <<'JULIA'
x = [1, 2, 3]
y = [1.0, 2.0, 3.0]

println(eltype(x))
println(eltype(y))
JULIA
```

This is useful when working with parameterized collections.

---

# 21. Type Parameters

The `T` in:

```julia id="p5w7kd"
Vector{T}
```

represents a type parameter.

For example:

```text
Vector{Int}
```

means a vector whose elements are `Int`.

While:

```text
Vector{Float64}
```

means a vector whose elements are `Float64`.

The structure is therefore:

```text
Vector
  +
element type
  ↓
Vector{T}
```

---

# 22. `where T`

Julia uses `where` to define methods that work generically with type parameters.

For example:

```python id="x7m3qp"
%%bash
julia <<'JULIA'
function first_element(x::Vector{T}) where T
    return x[1]
end

println(first_element([1, 2, 3]))
println(first_element([1.5, 2.5, 3.5]))
JULIA
```

Here `T` represents the element type of the vector.

The method is therefore generic over `T`.

---

# 23. Generic Methods with `where T`

The type parameter can be used in the method signature.

```python id="k4z8ws"
%%bash
julia <<'JULIA'
function element_type(x::Vector{T}) where T
    return T
end

println(element_type([1, 2, 3]))
println(element_type([1.0, 2.0, 3.0]))
JULIA
```

The method works for different element types without defining a separate method for every type.

---

# 24. Generic Functions and Type Parameters

Consider a function that returns the first element of a vector.

```python id="v9c5nr"
%%bash
julia <<'JULIA'
function first_value(x::Vector{T}) where T
    return x[1]
end

integers = [10, 20, 30]
floats = [1.5, 2.5, 3.5]
strings = ["A", "B", "C"]

println(first_value(integers))
println(first_value(floats))
println(first_value(strings))
JULIA
```

One generic method handles all three cases.

---

# 25. User-Defined Types

Julia allows programmers to define their own types.

A simple example is:

```python id="q3n8xm"
%%bash
julia <<'JULIA'
struct Point
    x
    y
end

p = Point(3, 4)

println(p)
println(typeof(p))
JULIA
```

`Point` is now a user-defined type.

User-defined types become especially important when combined with multiple dispatch.

Detailed treatment of structs and composite types will be developed in **Lesson 8**.

---

# 26. Types and Method Selection

Consider the following function:

```python id="t6m2vz"
%%bash
julia <<'JULIA'
function magnitude(x::Number)
    return abs(x)
end

function magnitude(x::Int)
    return abs(x)
end

println(magnitude(-5))
println(magnitude(-3.5))
JULIA
```

When Julia receives:

```julia id="p4x7cw"
magnitude(-5)
```

it examines the argument type.

```text
-5
 ↓
Int
 ↓
Applicable methods
 ↓
Most specific method
 ↓
Selected method
```

The type therefore participates directly in function execution.

---

# 27. Multiple Dispatch with User-Defined Types

User-defined types can have specialized methods.

```python id="g8q5mz"
%%bash
julia <<'JULIA'
struct Point
    x
    y
end

function describe(p::Point)
    println("This is a Point")
end

p = Point(2, 3)

describe(p)
JULIA
```

A function can therefore be extended to understand new types.

---

# 28. Dispatch Based on Multiple User-Defined Types

Multiple dispatch can involve more than one user-defined type.

```python id="w4n9kp"
%%bash
julia <<'JULIA'
struct Point
    x
    y
end

struct Vector2D
    x
    y
end

function combine(p::Point, v::Vector2D)
    return Point(p.x + v.x, p.y + v.y)
end

p = Point(1, 2)
v = Vector2D(3, 4)

result = combine(p, v)

println(result)
JULIA
```

The selected method depends on both argument types.

---

# 29. The Central Idea

The most important concept in this lesson is:

```text
FUNCTION
   +
ARGUMENT TYPES
   ↓
METHOD SELECTION
   ↓
APPROPRIATE METHOD
```

For example:

```text
combine(Int, Int)
       ↓
Integer method

combine(Int, Float64)
       ↓
Integer–Float method

combine(Float64, Float64)
       ↓
Floating-point method
```

Julia does not require different function names for these cases.

The same generic function can have multiple methods.

---

# 30. Why Multiple Dispatch Matters

Multiple dispatch is particularly powerful in scientific computing because mathematical operations often depend on the types of several interacting objects.

For example, an operation may need different behavior for:

```text
Number + Number
Matrix + Matrix
Point + Vector
DifferentialEquation + Solver
Model + Data
```

The function name can remain conceptually the same while specialized methods handle different combinations of types.

This makes Julia's type system closely connected to its scientific programming model.

---

# 31. Complete Example

The following example combines types, methods, multiple dispatch, and generic programming.

```python id="e5r8mq"
%%bash
julia <<'JULIA'
struct Point
    x
    y
end

function describe(x::Number)
    println("This is a number")
end

function describe(p::Point)
    println("This is a point")
end

function combine(x::Int, y::Int)
    return x + y
end

function combine(x::Float64, y::Float64)
    return x * y
end

function first_value(x::Vector{T}) where T
    return x[1]
end

describe(10)
describe(Point(2, 3))

println(combine(2, 3))
println(combine(2.0, 3.0))

println(first_value([10, 20, 30]))
println(first_value([1.5, 2.5, 3.5]))
JULIA
```

This example demonstrates the relationship between:

```text
Types
  ↓
Methods
  ↓
Multiple Dispatch
  ↓
Generic Functions
```

---

# 32. Summary

In this lesson, we introduced Julia's type system and multiple dispatch.

## `typeof`

```julia id="u4n8xc"
typeof(x)
```

Returns the concrete type of a value.

## `isa`

```julia id="m5q7zr"
x isa Number
```

Checks whether a value belongs to a type or type category.

## Type hierarchy

Julia organizes types into relationships such as:

```text
Any
 ↓
Number
 ↓
Real
 ↓
Integer
 ↓
Int
```

## Abstract types

Describe categories of related types.

Examples:

```text
Number
Real
Integer
AbstractFloat
```

## Concrete types

Represent actual values.

Examples:

```text
Int64
Float64
Bool
String
```

## `supertype`

```julia id="k7x3vp"
supertype(Int)
```

Examines the immediate supertype.

## `subtypes`

```julia id="n2c9wd"
subtypes(Number)
```

Examines direct subtypes.

## Type annotations

```julia id="r8m4zq"
x::Int
```

Specify a type in a variable or function context.

## Multiple methods

A single function can have multiple implementations:

```julia id="w6p3ky"
function f(x::Int)
    ...
end

function f(x::Float64)
    ...
end
```

## Multiple dispatch

Julia selects methods based on the types of the arguments.

```text
FUNCTION
   +
ARGUMENT TYPES
   ↓
METHOD SELECTION
   ↓
APPROPRIATE METHOD
```

## Parametric types

Types can contain parameters:

```julia id="c9v5nx"
Vector{Int}
Vector{Float64}
Vector{String}
```

## `where T`

Generic methods can introduce type parameters:

```julia id="h3q8mz"
function f(x::Vector{T}) where T
    ...
end
```

## Generic functions

One function can operate on many types through multiple methods and type parameters.

---

# Key Ideas to Remember

```text
typeof
    → What is the concrete type?

isa
    → Does this value belong to this type/category?

Type hierarchy
    → How are types related?

Abstract type
    → General category

Concrete type
    → Actual representable type

supertype
    → Move upward in the hierarchy

subtypes
    → Examine direct child types

Type annotation
    → Specify a type

Multiple methods
    → Several implementations of one function

Multiple dispatch
    → Select a method from argument types

Parametric type
    → Type containing parameters

Vector{T}
    → Vector parameterized by element type

where T
    → Introduce a type parameter in a method

Generic function
    → Function that works across types

User-defined type
    → Programmer-defined type

Method selection
    → Determined by applicable argument types
```

The essential Julia programming model introduced in this lesson is:

```text
                FUNCTION
                    │
                    │
             ARGUMENT TYPES
                    │
                    ↓
             TYPE DISPATCH
                    │
                    ↓
            METHOD SELECTION
                    │
                    ↓
          APPROPRIATE METHOD
```

Understanding this mechanism is essential before moving to **Lesson 8 — Structs, Mutable Structs and Composite Types**, where user-defined types are studied in greater depth.
