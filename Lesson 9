# Lesson 9 — Broadcasting and Dot Syntax

## Introduction

Julia provides a powerful broadcasting system for applying operations and functions element-by-element across arrays and other collections.

Broadcasting allows the same operation to be applied to many elements without explicitly writing a loop.

This is especially important in numerical and scientific computing, where operations are frequently performed element-by-element on vectors, matrices, and higher-dimensional arrays.

## Topics

* Scalar operations
* Array operations
* Broadcasting
* Function broadcasting
* `f.(x)`
* `.+`
* `.-`
* `.*`
* `./`
* `.^`
* Broadcasting mathematical functions
* Broadcasting functions with multiple arguments
* Compatible shapes
* `.*` versus `*`
* Broadcasted assignment with `.=`
* `ifelse.`
* Fused dotted expressions
* Broadcasting in scientific computing

---

## 1. Scalar Operations

A scalar operation works with individual values.

```python
%%bash
julia <<'JULIA'
x = 5
y = 2

println(x + y)
println(x - y)
println(x * y)
println(x / y)
println(x^y)
JULIA
```

These operations involve individual scalar values.

For example:

```julia
x + y
```

adds two scalar values.

---

## 2. Array Operations

Arrays can also participate in arithmetic operations.

For example, adding two vectors:

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3]
y = [4, 5, 6]

println(x + y)
JULIA
```

Vector addition is defined element-by-element.

The result is:

```text
[5, 7, 9]
```

However, not every operator has the same meaning for arrays.

---

## 3. Broadcasting

Broadcasting applies an operation element-by-element.

The dot `.` before an operator tells Julia to broadcast that operation.

For example:

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3]

println(x .+ 10)
println(x .* 2)
println(x .^ 2)
JULIA
```

The operations are applied to every element.

For:

```julia
x .+ 10
```

Julia computes:

```text
1 + 10
2 + 10
3 + 10
```

---

## 4. Function Broadcasting

The dot syntax can also be used with functions.

```python
%%bash
julia <<'JULIA'
x = [1.0, 4.0, 9.0]

println(sqrt.(x))
JULIA
```

Instead of applying `sqrt` to the entire array, broadcasting applies it to every element.

Conceptually:

```text
sqrt.([1.0, 4.0, 9.0])

        ↓

[sqrt(1.0), sqrt(4.0), sqrt(9.0)]
```

---

## 5. `f.(x)`

The general syntax for broadcasting a function is:

```julia
f.(x)
```

For example:

```python
%%bash
julia <<'JULIA'
f(x) = x^2 + 1

x = [1, 2, 3, 4]

println(f.(x))
JULIA
```

The function is applied independently to each element.

Conceptually:

```text
f.(x)
 ↓
[f(x₁), f(x₂), f(x₃), f(x₄)]
```

---

## 6. `.+`

The `.+` operator performs element-wise addition.

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3]
y = [4, 5, 6]

println(x .+ y)
JULIA
```

The result is:

```text
[5, 7, 9]
```

It performs:

```text
1 + 4
2 + 5
3 + 6
```

---

## 7. `.-`

The `.-` operator performs element-wise subtraction.

```python
%%bash
julia <<'JULIA'
x = [10, 20, 30]
y = [1, 2, 3]

println(x .- y)
JULIA
```

The result is:

```text
[9, 18, 27]
```

---

## 8. `.*`

The `.*` operator performs element-wise multiplication.

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3]
y = [4, 5, 6]

println(x .* y)
JULIA
```

The result is:

```text
[4, 10, 18]
```

Each corresponding pair of elements is multiplied.

---

## 9. `./`

The `./` operator performs element-wise division.

```python
%%bash
julia <<'JULIA'
x = [10.0, 20.0, 30.0]
y = [2.0, 4.0, 5.0]

println(x ./ y)
JULIA
```

The result is:

```text
[5.0, 5.0, 6.0]
```

---

## 10. `.^`

The `.^` operator performs element-wise exponentiation.

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

println(x .^ 2)
JULIA
```

The result is:

```text
[1, 4, 9, 16]
```

---

## 11. Broadcasting Mathematical Functions

Many mathematical functions can be broadcast over arrays.

```python
%%bash
julia <<'JULIA'
using Base.Math

x = [0.0, π/2, π]

println(sin.(x))
println(cos.(x))
println(exp.(x))
println(log.(x .+ 1))
JULIA
```

The dot applies each function to every element.

For example:

```julia
sin.(x)
```

means:

```text
[sin(x₁), sin(x₂), ..., sin(xₙ)]
```

---

## 12. Broadcasting User-Defined Functions

Broadcasting is not limited to built-in mathematical functions.

```python
%%bash
julia <<'JULIA'
function square_plus_one(x)
    x^2 + 1
end

values = [1, 2, 3, 4]

println(square_plus_one.(values))
JULIA
```

The function is applied independently to every element.

---

## 13. Broadcasting Functions with Multiple Arguments

A function can be broadcast over multiple arrays.

```python
%%bash
julia <<'JULIA'
f(x, y) = x^2 + y

x = [1, 2, 3]
y = [10, 20, 30]

println(f.(x, y))
JULIA
```

Conceptually:

```text
[f(x₁,y₁), f(x₂,y₂), f(x₃,y₃)]
```

The corresponding elements are supplied to the function.

---

## 14. Broadcasting with a Scalar and an Array

Broadcasting can combine arrays with scalar values.

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

println(x .+ 5)
println(x .* 3)
println(x ./ 2)
println(2 .^ x)
JULIA
```

The scalar is broadcast across the array.

For example:

```julia
x .+ 5
```

performs:

```text
[1 + 5, 2 + 5, 3 + 5, 4 + 5]
```

---

## 15. Compatible Shapes

Broadcasting works when the dimensions of the participating objects are compatible.

For example:

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3]
y = [10, 20, 30]

println(x .+ y)
JULIA
```

Both arrays have three elements, so their elements can be paired.

A scalar is also compatible with an array:

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3]

println(x .+ 10)
JULIA
```

The scalar is effectively used for every element.

---

## 16. Broadcasting with Matrices

Broadcasting also works with matrices.

```python
%%bash
julia <<'JULIA'
A = [1 2; 3 4]

println(A .+ 10)
println(A .* 2)
println(A .^ 2)
JULIA
```

Every matrix element is operated on independently.

For example:

```julia
A .^ 2
```

squares every element of `A`.

---

## 17. Broadcasting Across Compatible Dimensions

Broadcasting can operate across dimensions when the dimensions are compatible.

```python
%%bash
julia <<'JULIA'
A = [1 2 3; 4 5 6]
b = [10, 20, 30]

println(A .+ b)
JULIA
```

The vector is broadcast across the rows.

Conceptually:

```text
[1 2 3]     [10 20 30]
[4 5 6]  +  [10 20 30]
```

giving:

```text
[11 22 33]
[14 25 36]
```

---

## 18. `.*` versus `*`

This distinction is important in Julia.

For arrays:

```julia
*
```

represents matrix multiplication.

Whereas:

```julia
.*
```

represents element-wise multiplication.

For example:

```python
%%bash
julia <<'JULIA'
A = [1 2; 3 4]
B = [5 6; 7 8]

println("Matrix multiplication:")
println(A * B)

println("Element-wise multiplication:")
println(A .* B)
JULIA
```

The two operations are mathematically different.

### Matrix multiplication

```julia
A * B
```

uses the rules of matrix multiplication.

### Element-wise multiplication

```julia
A .* B
```

multiplies corresponding elements.

---

## 19. Other Dotted Operators

The same distinction applies to other arithmetic operators.

```julia
+
```

versus:

```julia
.+
```

```julia
-
```

versus:

```julia
.-
```

```julia
*
```

versus:

```julia
.*
```

```julia
/
```

versus:

```julia
./
```

```julia
^
```

versus:

```julia
.^
```

The dotted version requests element-wise broadcasting.

---

## 20. Broadcasted Assignment with `.=`

The `.=` operator performs broadcasted assignment.

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

x .= x .* 2

println(x)
JULIA
```

The existing array is updated element-by-element.

The result is:

```text
[2, 4, 6, 8]
```

---

## 21. `.=` with a Function

Broadcasted assignment can also be used with functions.

```python
%%bash
julia <<'JULIA'
x = [1.0, 4.0, 9.0]

x .= sqrt.(x)

println(x)
JULIA
```

The elements of `x` are replaced with their square roots.

---

## 22. `ifelse.`

`ifelse` can also be broadcast.

The scalar form is:

```julia
ifelse(condition, value_if_true, value_if_false)
```

For example:

```python
%%bash
julia <<'JULIA'
x = 5

println(ifelse(x > 0, 1, 0))
JULIA
```

The function can be broadcast over an array:

```python
%%bash
julia <<'JULIA'
x = [-2, -1, 0, 1, 2]

result = ifelse.(x .> 0, 1, 0)

println(result)
JULIA
```

The condition itself is broadcast:

```julia
x .> 0
```

and then `ifelse.` is applied element-by-element.

---

## 23. Fused Dotted Expressions

One of Julia's powerful broadcasting features is the fusion of dotted operations.

Consider:

```python
%%bash
julia <<'JULIA'
x = [1.0, 2.0, 3.0, 4.0]

y = 2 .* x .^ 2 .+ 3 .* x .+ 1

println(y)
JULIA
```

The entire expression is broadcast element-by-element.

Conceptually, Julia evaluates:

```text
2x² + 3x + 1
```

for each element of `x`.

---

## 24. Dot Fusion with Functions

Broadcasting can also fuse function calls and operators.

```python
%%bash
julia <<'JULIA'
x = [1.0, 2.0, 3.0]

y = sin.(x) .+ cos.(x)

println(y)
JULIA
```

A more compact fused expression is:

```python
%%bash
julia <<'JULIA'
x = [1.0, 2.0, 3.0]

y = sin.(x) .+ 2 .* x .^ 2

println(y)
JULIA
```

The operations are performed element-by-element.

---

## 25. Broadcasting and Anonymous Functions

Anonymous functions can also be broadcast.

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

result = (x -> x^2 + 1).(x)

println(result)
JULIA
```

The anonymous function is applied independently to every element.

---

## 26. Broadcasting in Numerical Data Processing

Broadcasting is useful for transforming numerical datasets.

```python
%%bash
julia <<'JULIA'
data = [10.0, 20.0, 30.0, 40.0]

normalized = data ./ maximum(data)

println(normalized)
JULIA
```

Each value is divided by the maximum value.

---

## 27. Broadcasting in Scientific Computing

Scientific computations frequently involve applying the same mathematical transformation to many values.

For example:

```python
%%bash
julia <<'JULIA'
x = collect(0.0:0.5:5.0)

y = sin.(x)

println(x)
println(y)
JULIA
```

Here:

```julia
sin.(x)
```

computes the sine of every value in `x`.

---

## 28. Broadcasting a Physical Model

Suppose a physical model is defined by:

```text
y = A sin(ωt)
```

The model can be evaluated at many time points using broadcasting.

```python
%%bash
julia <<'JULIA'
A = 2.0
ω = 3.0

t = collect(0.0:0.1:1.0)

y = A .* sin.(ω .* t)

println(t)
println(y)
JULIA
```

The same mathematical expression is evaluated over the entire time array.

---

## 29. Broadcasting with Multiple Arrays

Scientific models often depend on multiple numerical arrays.

```python
%%bash
julia <<'JULIA'
x = [1.0, 2.0, 3.0]
a = [2.0, 3.0, 4.0]
b = [1.0, 1.0, 1.0]

y = a .* x .+ b

println(y)
JULIA
```

Each corresponding set of values is used in the calculation.

---

## 30. Broadcasting for Numerical Transformation

Broadcasting can be used for transformations such as:

```python
%%bash
julia <<'JULIA'
data = [1.0, 4.0, 9.0, 16.0]

sqrt_data = sqrt.(data)
log_data = log.(data)
scaled_data = 2 .* data

println(sqrt_data)
println(log_data)
println(scaled_data)
JULIA
```

Each operation produces a transformed array.

---

## 31. Broadcasting vs Explicit Loops

The following loop:

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]
y = similar(x)

for i in eachindex(x)
    y[i] = x[i]^2
end

println(y)
JULIA
```

can be expressed using broadcasting:

```python
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

y = x .^ 2

println(y)
JULIA
```

Broadcasting provides a concise way to express element-wise operations.

---

## 32. Complete Example — Numerical Transformation

```python
%%bash
julia <<'JULIA'
x = collect(0.0:0.5:5.0)

y = 2 .* sin.(x) .+ x .^ 2

println("x:")
println(x)

println("y:")
println(y)
JULIA
```

The expression:

```julia
2 .* sin.(x) .+ x .^ 2
```

applies the complete mathematical transformation to every element of `x`.

---

## 33. Complete Example — Scientific Data Processing

```python
%%bash
julia <<'JULIA'
data = [2.0, 4.0, 6.0, 8.0, 10.0]

scaled = 3 .* data
squared = data .^ 2
normalized = data ./ maximum(data)

println("Original:")
println(data)

println("Scaled:")
println(scaled)

println("Squared:")
println(squared)

println("Normalized:")
println(normalized)
JULIA
```

Broadcasting allows all of these transformations to be written directly as array expressions.

---

## 34. Key Ideas

### Scalar operation

```julia
x + y
```

### Element-wise array operation

```julia
x .+ y
```

### Broadcast a function

```julia
f.(x)
```

### Element-wise multiplication

```julia
x .* y
```

### Element-wise division

```julia
x ./ y
```

### Element-wise exponentiatio
