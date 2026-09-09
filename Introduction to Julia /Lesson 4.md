# Lesson 4 — Arrays

## Introduction

Arrays are fundamental data structures in Julia for storing and working with numerical data.

An array can contain multiple values organized along one or more dimensions. One-dimensional arrays are commonly used as vectors, two-dimensional arrays as matrices, and higher-dimensional arrays are useful for representing structured scientific and numerical data.

In this lesson, we introduce:

* Creating arrays
* Vectors
* Matrices
* Multidimensional arrays
* Array indexing
* Slicing
* Array assignment
* `push!`
* `pop!`
* `append!`
* `size`
* `length`
* `ndims`
* `eltype`
* `zeros`
* `ones`
* `fill`
* `reshape`
* `copy`
* `similar`
* Iterating over arrays
* Row and column access
* Mathematical operations on arrays
* Arrays in scientific computing

---

# 1. Creating Arrays

Arrays can be created using square brackets.

```python id="z8m4qa"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5]

println(x)
JULIA
```

Output:

```text
[1, 2, 3, 4, 5]
```

The values are separated by commas.

---

# 2. Vectors

A one-dimensional array is commonly used as a vector.

```python id="f7v1pc"
%%bash
julia <<'JULIA'
v = [10, 20, 30, 40, 50]

println(v)
println(typeof(v))
JULIA
```

A vector can represent a sequence of numerical values.

For example:

```text
[10, 20, 30, 40, 50]
```

can represent measurements, coordinates, or other numerical data.

---

# 3. Matrices

A matrix is a two-dimensional array.

Julia allows matrices to be written using spaces between elements in a row and semicolons between rows.

```python id="c9q5wr"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6;
     7 8 9]

println(A)
JULIA
```

The matrix is:

```text
1  2  3
4  5  6
7  8  9
```

A matrix therefore has rows and columns.

---

# 4. Multidimensional Arrays

Julia arrays can have more than two dimensions.

For example, a three-dimensional array can be created using `zeros`.

```python id="r3h7kn"
%%bash
julia <<'JULIA'
A = zeros(2, 3, 4)

println(A)
JULIA
```

This creates an array with:

* 2 elements along the first dimension,
* 3 along the second dimension,
* 4 along the third dimension.

Multidimensional arrays are useful for representing structured numerical data such as grids, time-dependent data, and simulation fields.

---

# 5. Array Indexing

Julia uses **1-based indexing**.

The first element of an array has index `1`.

```python id="w2s6ap"
%%bash
julia <<'JULIA'
x = [10, 20, 30, 40, 50]

println(x[1])
println(x[3])
println(x[5])
JULIA
```

Output:

```text
10
30
50
```

---

# 6. The `end` Index

The keyword `end` refers to the final index of an array.

```python id="t5n8qy"
%%bash
julia <<'JULIA'
x = [10, 20, 30, 40, 50]

println(x[end])
JULIA
```

Output:

```text
50
```

This is useful when the length of an array is not known explicitly.

---

# 7. Matrix Indexing

Matrices require two indices:

```julia
A[row, column]
```

For example:

```python id="u4c9xz"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6;
     7 8 9]

println(A[1, 1])
println(A[2, 3])
println(A[3, 2])
JULIA
```

Output:

```text
1
6
8
```

The first index specifies the row and the second specifies the column.

---

# 8. Multidimensional Indexing

Higher-dimensional arrays use one index for each dimension.

```python id="n7x4pb"
%%bash
julia <<'JULIA'
A = zeros(2, 3, 4)

A[1, 2, 3] = 10

println(A[1, 2, 3])
JULIA
```

Here:

```julia
A[1, 2, 3]
```

specifies a location along three dimensions.

---

# 9. Slicing

A range of indices can be used to select multiple elements.

```python id="e4k6sz"
%%bash
julia <<'JULIA'
x = [10, 20, 30, 40, 50]

println(x[2:4])
JULIA
```

Output:

```text
[20, 30, 40]
```

The expression:

```julia
x[2:4]
```

selects elements from index `2` through index `4`.

---

# 10. Matrix Slicing

Rows and columns can be selected using ranges.

```python id="m8p3vx"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6;
     7 8 9]

println(A[1:2, 2:3])
JULIA
```

This selects:

```text
2  3
5  6
```

---

# 11. Row Access

The colon `:` can be used to select an entire row.

```python id="j2s7ka"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6;
     7 8 9]

println(A[2, :])
JULIA
```

This accesses the second row.

---

# 12. Column Access

Similarly, the colon can select an entire column.

```python id="p6r9wd"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6;
     7 8 9]

println(A[:, 2])
JULIA
```

This accesses the second column.

---

# 13. Array Assignment

Individual array elements can be changed using assignment.

```python id="x5c8mt"
%%bash
julia <<'JULIA'
x = [10, 20, 30, 40, 50]

x[2] = 100

println(x)
JULIA
```

Output:

```text
[10, 100, 30, 40, 50]
```

Array assignment can also modify several elements.

```python id="a7k3qp"
%%bash
julia <<'JULIA'
x = [10, 20, 30, 40, 50]

x[2:4] = [200, 300, 400]

println(x)
JULIA
```

The selected portion of the array is replaced.

---

# 14. `push!`

`push!` adds an element to the end of a one-dimensional array.

```python id="v9r2ks"
%%bash
julia <<'JULIA'
x = [1, 2, 3]

push!(x, 4)

println(x)
JULIA
```

Output:

```text
[1, 2, 3, 4]
```

`push!` modifies the existing array.

---

# 15. `pop!`

`pop!` removes and returns the last element.

```python id="q4m8zx"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

value = pop!(x)

println("Removed: ", value)
println("Array: ", x)
JULIA
```

Output:

```text
Removed: 4
Array: [1, 2, 3]
```

---

# 16. `append!`

`append!` adds the elements of another collection to the end of an array.

```python id="h6w3np"
%%bash
julia <<'JULIA'
x = [1, 2, 3]

append!(x, [4, 5, 6])

println(x)
JULIA
```

Output:

```text
[1, 2, 3, 4, 5, 6]
```

The elements from the second collection are added individually.

---

# 17. `size`

The `size` function gives the size of an array along each dimension.

```python id="k3p7sd"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6]

println(size(A))
JULIA
```

Output:

```text
(2, 3)
```

This means the matrix has:

* 2 rows
* 3 columns

For a three-dimensional array:

```python id="n4q8vt"
%%bash
julia <<'JULIA'
A = zeros(2, 3, 4)

println(size(A))
JULIA
```

Output:

```text
(2, 3, 4)
```

---

# 18. `length`

The `length` function gives the total number of elements.

```python id="s6w2mx"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6]

println(length(A))
JULIA
```

Output:

```text
6
```

For a multidimensional array, `length` gives the total number of elements across all dimensions.

---

# 19. `ndims`

The `ndims` function returns the number of dimensions.

```python id="r8k5yc"
%%bash
julia <<'JULIA'
x = [1, 2, 3]

A = [1 2;
     3 4]

B = zeros(2, 3, 4)

println(ndims(x))
println(ndims(A))
println(ndims(B))
JULIA
```

Output:

```text
1
2
3
```

---

# 20. `eltype`

The `eltype` function tells us the type of elements stored in an array.

```python id="q2m7vf"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

println(eltype(x))
JULIA
```

Output:

```text
Int64
```

For floating-point data:

```python id="b5r9kx"
%%bash
julia <<'JULIA'
x = [1.0, 2.0, 3.0]

println(eltype(x))
JULIA
```

Output:

```text
Float64
```

---

# 21. `zeros`

`zeros` creates an array filled with zeros.

```python id="m3v8qs"
%%bash
julia <<'JULIA'
x = zeros(5)

println(x)
JULIA
```

Output:

```text
[0.0, 0.0, 0.0, 0.0, 0.0]
```

A matrix can also be created:

```python id="x7n2pf"
%%bash
julia <<'JULIA'
A = zeros(3, 4)

println(A)
JULIA
```

This creates a `3 × 4` array.

---

# 22. `ones`

`ones` creates an array filled with ones.

```python id="w4k9zb"
%%bash
julia <<'JULIA'
x = ones(5)

println(x)
JULIA
```

Output:

```text
[1.0, 1.0, 1.0, 1.0, 1.0]
```

A matrix can also be created:

```python id="d8m3qr"
%%bash
julia <<'JULIA'
A = ones(2, 3)

println(A)
JULIA
```

---

# 23. `fill`

`fill` creates an array whose elements all contain a specified value.

```python id="f2v7mk"
%%bash
julia <<'JULIA'
x = fill(5, 4)

println(x)
JULIA
```

Output:

```text
[5, 5, 5, 5]
```

A multidimensional array can also be created:

```python id="q9t4ns"
%%bash
julia <<'JULIA'
A = fill(7, 2, 3)

println(A)
JULIA
```

This creates a `2 × 3` array filled with `7`.

---

# 24. `reshape`

`reshape` changes the dimensions of an array while preserving its elements.

```python id="k7p3yc"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5, 6]

A = reshape(x, 2, 3)

println(A)
JULIA
```

The six elements are arranged into a `2 × 3` array.

A key point is that `reshape` changes the shape of the data rather than changing the number of elements.

---

# 25. Reshaping Into Higher Dimensions

`reshape` can also create multidimensional arrays.

```python id="p4w8qx"
%%bash
julia <<'JULIA'
x = collect(1:24)

A = reshape(x, 2, 3, 4)

println(size(A))
println(ndims(A))
JULIA
```

Output:

```text
(2, 3, 4)
3
```

The total number of elements remains `24`.

---

# 26. `copy`

`copy` creates a copy of an array.

```python id="s3n7kv"
%%bash
julia <<'JULIA'
x = [1, 2, 3]

y = copy(x)

y[1] = 100

println("x = ", x)
println("y = ", y)
JULIA
```

Output:

```text
x = [1, 2, 3]
y = [100, 2, 3]
```

The copied array can therefore be modified independently.

---

# 27. `similar`

`similar` creates an array with the same general structure and element type as another array, but without requiring the same values.

```python id="m6q2xr"
%%bash
julia <<'JULIA'
x = [1.0, 2.0, 3.0, 4.0]

y = similar(x)

println(y)
println(eltype(y))
println(size(y))
JULIA
```

The values of `y` are not initialized to the values of `x`.

`similar` is useful when a new array is needed with compatible storage characteristics.

---

# 28. Iterating Over Arrays

Arrays can be traversed using a `for` loop.

```python id="v5k8nd"
%%bash
julia <<'JULIA'
x = [10, 20, 30, 40]

for value in x
    println(value)
end
JULIA
```

The loop variable receives each element in sequence.

---

# 29. Iterating Using Indices

Sometimes both the index and value are needed.

```python id="c8m4py"
%%bash
julia <<'JULIA'
x = [10, 20, 30, 40]

for i in eachindex(x)
    println("Index = ", i, ", Value = ", x[i])
end
JULIA
```

`eachindex(x)` provides valid indices for the array.

---

# 30. Iterating Over a Matrix

A matrix can also be iterated over.

```python id="q7x3mv"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6]

for value in A
    println(value)
end
JULIA
```

Julia's array iteration follows its internal array indexing order.

For scientific computing, understanding array layout becomes important when performance matters.

---

# 31. Iterating Through Rows and Columns

Rows and columns can be accessed explicitly.

```python id="n5w8rq"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6;
     7 8 9]

for i in 1:size(A, 1)
    println("Row ", i, ": ", A[i, :])
end
JULIA
```

Similarly, columns can be accessed:

```python id="b9c2xt"
%%bash
julia <<'JULIA'
A = [1 2 3;
     4 5 6;
     7 8 9]

for j in 1:size(A, 2)
    println("Column ", j, ": ", A[:, j])
end
JULIA
```

---

# 32. Mathematical Operations on Arrays

Arrays are commonly used for numerical calculations.

For example, two vectors can be added element by element.

```python id="h4m7zs"
%%bash
julia <<'JULIA'
x = [1, 2, 3]
y = [4, 5, 6]

println(x + y)
JULIA
```

Output:

```text
[5, 7, 9]
```

This performs vector addition.

---

# 33. Scalar Operations

An array can be multiplied by a scalar.

```python id="r6q9vk"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

println(2 * x)
JULIA
```

Output:

```text
[2, 4, 6, 8]
```

This is useful for scaling numerical data.

---

# 34. Matrix Operations

Matrices can participate in mathematical operations.

```python id="t8p3mn"
%%bash
julia <<'JULIA'
A = [1 2;
     3 4]

B = [5 6;
     7 8]

println("A + B =")
println(A + B)

println("A * B =")
println(A * B)
JULIA
```

Here:

* `A + B` performs matrix addition.
* `A * B` performs matrix multiplication.

---

# 35. Mathematical Functions on Arrays

Many mathematical functions can operate on array data.

For example, `sum`, `minimum`, and `maximum`.

```python id="y5k8rp"
%%bash
julia <<'JULIA'
x = [4, 8, 2, 10, 6]

println("Sum: ", sum(x))
println("Minimum: ", minimum(x))
println("Maximum: ", maximum(x))
JULIA
```

These operations are useful when processing numerical datasets.

---

# 36. Element-Wise Mathematical Operations

Julia provides dotted operators for element-wise operations.

```python id="k9w4vs"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

println(x .^ 2)
JULIA
```

Output:

```text
[1, 4, 9, 16]
```

Each element is squared individually.

Similarly:

```python id="c6p2xm"
%%bash
julia <<'JULIA'
x = [1, 2, 3]
y = [4, 5, 6]

println(x .* y)
JULIA
```

Output:

```text
[4, 10, 18]
```

The distinction between ordinary array operations and element-wise operations becomes important when performing numerical computations.

---

# 37. Arrays in Scientific Computing

Arrays are central to scientific computing because many physical and mathematical quantities are represented as numerical collections.

For example, measurements taken at different times can be represented using a vector.

```python id="u3m8qk"
%%bash
julia <<'JULIA'
temperature = [20.5, 21.1, 22.4, 23.0, 24.2]

println(temperature)
println("Minimum: ", minimum(temperature))
println("Maximum: ", maximum(temperature))
println("Average: ", sum(temperature) / length(temperature))
JULIA
```

A spatial field can be represented using a matrix.

```python id="a8v4yn"
%%bash
julia <<'JULIA'
temperature_field = [
    20.0 21.0 22.0;
    21.0 22.0 23.0;
    22.0 23.0 24.0
]

println(temperature_field)
JULIA
```

Higher-dimensional arrays can represent more complex scientific data.

For example:

```text
spatial dimensions + time
```

can naturally lead to three-dimensional numerical arrays.

---

# 38. Practical Scientific Pattern

Consider a simple collection of measurements.

```python id="g7n2xp"
%%bash
julia <<'JULIA'
measurements = [2.1, 2.4, 2.8, 3.0, 3.3]

println("Number of measurements: ", length(measurements))
println("Data type: ", eltype(measurements))
println("Dimensions: ", ndims(measurements))
println("Size: ", size(measurements))

println("Minimum: ", minimum(measurements))
println("Maximum: ", maximum(measurements))
println("Mean: ", sum(measurements) / length(measurements))
JULIA
```

This demonstrates how basic array operations can be combined to inspect numerical data.

---

# 39. Complete Example

The following example combines many of the concepts introduced in this lesson.

```python id="p6v3kx"
%%bash
julia <<'JULIA'
x = collect(1:12)

println("Original array:")
println(x)

println("Length: ", length(x))
println("Dimensions: ", ndims(x))
println("Element type: ", eltype(x))

A = reshape(x, 3, 4)

println("Reshaped array:")
println(A)

println("Second row:")
println(A[2, :])

println("Third column:")
println(A[:, 3])

B = copy(A)

B[1, 1] = 100

println("Original A:")
println(A)

println("Copied B:")
println(B)

println("Sum of A: ", sum(A))
println("Maximum of A: ", maximum(A))
println("Minimum of A: ", minimum(A))
JULIA
```

This example combines:

* array creation,
* `collect`,
* `length`,
* `ndims`,
* `eltype`,
* `reshape`,
* indexing,
* row access,
* column access,
* `copy`,
* assignment,
* and mathematical operations.

---

# 40. Key Array Syntax

## Create an array

```julia
x = [1, 2, 3]
```

## Create a matrix

```julia
A = [1 2;
     3 4]
```

## Index an element

```julia
x[1]
```

## Matrix indexing

```julia
A[2, 3]
```

## Slice

```julia
x[2:4]
```

## Row access

```julia
A[2, :]
```

## Column access

```julia
A[:, 2]
```

## Add an element

```julia
push!(x, 4)
```

## Remove the last element

```julia
pop!(x)
```

## Append elements

```julia
append!(x, [5, 6])
```

## Array size

```julia
size(x)
```

## Number of elements

```julia
length(x)
```

## Number of dimensions

```julia
ndims(x)
```

## Element type

```julia
eltype(x)
```

## Array of zeros

```julia
zeros(3, 4)
```

## Array of ones

```julia
ones(3, 4)
```

## Filled array

```julia
fill(5, 3, 4)
```

## Reshape

```julia
reshape(x, 2, 3)
```

## Copy

```julia
copy(x)
```

## Similar array

```julia
similar(x)
```

---

# 41. Summary

Arrays provide Julia with a flexible way to represent numerical data across one or more dimensions.

In this lesson, we learned:

* How to create arrays.
* How vectors represent one-dimensional data.
* How matrices represent two-dimensional data.
* How multidimensional arrays represent higher-dimensional data.
* How Julia uses 1-based indexing.
* How to access individual elements.
* How to slice arrays.
* How to assign new values to array elements.
* How to use `push!`.
* How to use `pop!`.
* How to use `append!`.
* How to inspect arrays with `size`.
* How to determine the number of elements with `length`.
* How to determine the number of dimensions with `ndims`.
* How to determine the element type with `eltype`.
* How to create arrays using `zeros`.
* How to create arrays using `ones`.
* How to create arrays using `fill`.
* How to reshape arrays using `reshape`.
* How to create independent copies using `copy`.
* How to create compatible arrays using `similar`.
* How to iterate over arrays.
* How to access rows and columns.
* How to perform mathematical operations on arrays.
* How arrays represent numerical data in scientific computing.

The central idea is:

```text
Array
   ↓
one or more dimensions
   ↓
indexing and slicing
   ↓
assignment and modification
   ↓
iteration and mathematical operations
   ↓
scientific and numerical data
```

Arrays form one of the foundations of scientific Julia programming. They will be used extensively in later lessons involving collections, broadcasting, automatic differentiation, and scientific computing workflows.
