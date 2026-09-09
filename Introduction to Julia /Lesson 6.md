# Lesson 6 — Comprehensions, Map, Filter and Reduce

## Introduction

Julia provides several powerful ways to create, transform, select, and aggregate collections.

In the previous lesson, we introduced tuples, named tuples, dictionaries, and sets. In this lesson, we use collection-processing tools to perform operations such as:

* generating new collections
* transforming values
* selecting values based on conditions
* aggregating collections into single values
* processing numerical data

The main tools introduced in this lesson are:

* Array comprehensions
* Conditional comprehensions
* Nested comprehensions
* Generator expressions
* `map`
* `filter`
* `reduce`
* `sum`
* `prod`

These operations are especially useful in numerical and scientific computing.

---

# 1. Array Comprehensions

An array comprehension provides a compact way to construct an array from an iteration.

The basic syntax is:

```julia
[expression for variable in collection]
```

For example:

```python id="x1c4fz"
%%bash
julia <<'JULIA'
squares = [x^2 for x in 1:5]

println(squares)
JULIA
```

The expression:

```julia
x^2
```

is evaluated for every value of `x`.

The result is an array.

---

# 2. Comprehensions with Arrays

Comprehensions can operate on existing arrays.

```python id="5x2v8p"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5]

squares = [value^2 for value in x]

println(squares)
JULIA
```

Another example:

```python id="y6n1ml"
%%bash
julia <<'JULIA'
x = [2, 4, 6, 8]

half = [value / 2 for value in x]

println(half)
JULIA
```

---

# 3. Applying Functions in Comprehensions

A function can be applied inside a comprehension.

```python id="m9v3jz"
%%bash
julia <<'JULIA'
function square(x)
    return x^2
end

x = [1, 2, 3, 4, 5]

result = [square(value) for value in x]

println(result)
JULIA
```

Comprehensions therefore provide a concise way to apply an operation to every element.

---

# 4. Conditional Comprehensions

A comprehension can include a condition using `if`.

The general structure is:

```julia
[expression for variable in collection if condition]
```

For example:

```python id="t2r8kx"
%%bash
julia <<'JULIA'
even_numbers = [x for x in 1:10 if x % 2 == 0]

println(even_numbers)
JULIA
```

Only values satisfying the condition are included.

---

# 5. Conditional Transformation

The expression and condition can perform different tasks.

```python id="q5m2ra"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5, 6]

result = [value^2 for value in x if value > 3]

println(result)
JULIA
```

Only values greater than `3` are squared.

---

# 6. Nested Comprehensions

Comprehensions can contain multiple iteration variables.

```python id="h7q4ks"
%%bash
julia <<'JULIA'
result = [i + j for i in 1:3, j in 1:3]

println(result)
JULIA
```

This produces a two-dimensional array.

Nested comprehensions can be useful for generating structured numerical data.

---

# 7. Nested Comprehensions with Conditions

Conditions can also be used in nested comprehensions.

```python id="c4v8yn"
%%bash
julia <<'JULIA'
result = [i + j for i in 1:5, j in 1:5 if i + j > 5]

println(result)
JULIA
```

The condition controls which combinations contribute to the resulting collection.

---

# 8. Generator Expressions

A generator expression looks similar to a comprehension but does not immediately construct the complete resulting array.

The basic form is:

```julia
(expression for variable in collection)
```

For example:

```python id="e3z7qp"
%%bash
julia <<'JULIA'
g = (x^2 for x in 1:5)

println(g)
JULIA
```

The generator produces values as they are consumed.

---

# 9. Using Generators with `sum`

Generators are particularly useful when combined with reduction operations.

```python id="n2h6cw"
%%bash
julia <<'JULIA'
result = sum(x^2 for x in 1:100)

println(result)
JULIA
```

This allows the values to be generated as needed rather than explicitly constructing an intermediate array.

---

# 10. `map`

The `map` function applies a function to every element of a collection.

The general structure is:

```julia
map(function, collection)
```

For example:

```python id="z8k2wv"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5]

result = map(x -> x^2, x)

println(result)
JULIA
```

The result contains the square of every element.

---

# 11. `map` with a Named Function

A previously defined function can also be passed to `map`.

```python id="r4m9dx"
%%bash
julia <<'JULIA'
function cube(x)
    return x^3
end

x = [1, 2, 3, 4]

result = map(cube, x)

println(result)
JULIA
```

---

# 12. `map` with Multiple Collections

`map` can operate on multiple collections simultaneously.

```python id="w6p3zs"
%%bash
julia <<'JULIA'
x = [1, 2, 3]
y = [10, 20, 30]

result = map(+, x, y)

println(result)
JULIA
```

The corresponding elements are combined:

```text id="a7j4kp"
1 + 10
2 + 20
3 + 30
```

The result is:

```text id="d3k6rt"
[11, 22, 33]
```

---

# 13. `filter`

The `filter` function selects elements satisfying a condition.

The general structure is:

```julia
filter(function, collection)
```

For example:

```python id="b5n7qc"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5, 6]

result = filter(x -> x % 2 == 0, x)

println(result)
JULIA
```

Only even values are retained.

---

# 14. Filtering with a Named Function

A named function can also be used.

```python id="u3v8mn"
%%bash
julia <<'JULIA'
function is_positive(x)
    return x > 0
end

x = [-3, -2, -1, 0, 1, 2, 3]

result = filter(is_positive, x)

println(result)
JULIA
```

---

# 15. `reduce`

The `reduce` function combines the elements of a collection into a single result.

The basic form is:

```julia
reduce(function, collection)
```

For example:

```python id="p9c4zs"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

result = reduce(+, x)

println(result)
JULIA
```

The operation is conceptually:

```text
(((1 + 2) + 3) + 4)
```

producing:

```text
10
```

---

# 16. `reduce` with Multiplication

A multiplication operation can also be used.

```python id="q7m2hx"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

result = reduce(*, x)

println(result)
JULIA
```

The operation combines the elements using multiplication.

---

# 17. `sum`

The `sum` function calculates the sum of elements.

```python id="v8n3kc"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5]

println(sum(x))
JULIA
```

It can also be applied to a generator.

```python id="e5q9rs"
%%bash
julia <<'JULIA'
result = sum(x^2 for x in 1:10)

println(result)
JULIA
```

---

# 18. `prod`

The `prod` function calculates the product of elements.

```python id="j4r7mz"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5]

println(prod(x))
JULIA
```

For example:

```text id="y2x8qa"
1 × 2 × 3 × 4 × 5 = 120
```

`prod` can also operate on generators.

```python id="c6v5nt"
%%bash
julia <<'JULIA'
result = prod(x for x in 1:5)

println(result)
JULIA
```

---

# 19. Applying Functions to Collections

Comprehensions, `map`, and `filter` all provide ways of processing collections.

Consider:

```python id="s8w2pd"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5]

squares_comprehension = [value^2 for value in x]
squares_map = map(value -> value^2, x)

println(squares_comprehension)
println(squares_map)
JULIA
```

Both approaches produce the transformed values.

The choice depends on readability and the operation being expressed.

---

# 20. Transformation with `map`

Suppose a numerical dataset contains measurements that need to be converted.

```python id="f5k9vx"
%%bash
julia <<'JULIA'
temperature_celsius = [0, 10, 20, 30, 40]

temperature_kelvin = map(
    x -> x + 273.15,
    temperature_celsius
)

println(temperature_kelvin)
JULIA
```

Each measurement is transformed independently.

---

# 21. Selection with `filter`

Suppose only measurements above a threshold are required.

```python id="h2m7qs"
%%bash
julia <<'JULIA'
temperature = [15, 22, 28, 31, 18, 35]

high_temperature = filter(
    x -> x > 25,
    temperature
)

println(high_temperature)
JULIA
```

The resulting collection contains only values satisfying the condition.

---

# 22. Combining `map` and `filter`

Transformation and selection can be combined.

```python id="k8p4yd"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5, 6]

result = map(
    x -> x^2,
    filter(x -> x % 2 == 0, x)
)

println(result)
JULIA
```

The process is:

```text
Original data
      ↓
Filter even values
      ↓
Square the remaining values
      ↓
Result
```

---

# 23. Combining `map`, `filter`, and `reduce`

The three operations can be combined into a processing pipeline.

```python id="m6q3vz"
%%bash
julia <<'JULIA'
x = 1:10

result = reduce(
    +,
    map(
        x -> x^2,
        filter(x -> x % 2 == 0, x)
    )
)

println(result)
JULIA
```

The operations occur conceptually as:

```text
1. Generate values from 1 to 10
2. Keep the even values
3. Square them
4. Add the resulting values
```

The even values are:

```text
2, 4, 6, 8, 10
```

After squaring:

```text
4, 16, 36, 64, 100
```

The final result is their sum.

---

# 24. Functional Programming Patterns

Functional programming emphasizes applying functions to data rather than explicitly managing every iteration step.

Julia supports these patterns through tools such as:

```text
map
filter
reduce
```

A common conceptual pattern is:

```text
Collection
    ↓
filter
    ↓
map
    ↓
reduce
    ↓
Single result
```

This pattern is useful for expressing data-processing pipelines.

---

# 25. Anonymous Functions

Anonymous functions are especially useful with `map`, `filter`, and `reduce`.

For example:

```python id="n4v8cx"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

result = map(x -> 2x + 1, x)

println(result)
JULIA
```

Here:

```julia
x -> 2x + 1
```

is an anonymous function.

It is applied independently to every element.

---

# 26. Numerical Data Processing

These tools are particularly useful for numerical datasets.

Consider a collection of measurements:

```python id="q3m7ks"
%%bash
julia <<'JULIA'
measurements = [2.1, 3.4, 5.2, 1.8, 4.7, 6.3]

scaled = map(x -> 2x, measurements)

println(scaled)
JULIA
```

The measurements have been transformed by a scaling operation.

---

# 27. Numerical Filtering

We can select measurements above a threshold.

```python id="w9c5pz"
%%bash
julia <<'JULIA'
measurements = [2.1, 3.4, 5.2, 1.8, 4.7, 6.3]

large_values = filter(x -> x > 4.0, measurements)

println(large_values)
JULIA
```

---

# 28. Numerical Aggregation

The selected measurements can then be aggregated.

```python id="r6n2vx"
%%bash
julia <<'JULIA'
measurements = [2.1, 3.4, 5.2, 1.8, 4.7, 6.3]

large_values = filter(x -> x > 4.0, measurements)

total = sum(large_values)

println(total)
JULIA
```

This represents a simple numerical data-processing workflow:

```text
Measurements
      ↓
Filter
      ↓
Selected measurements
      ↓
Sum
      ↓
Total
```

---

# 29. Generator-Based Numerical Processing

Generators can be useful when only the final aggregate is required.

```python id="b7q4md"
%%bash
julia <<'JULIA'
result = sum(x^2 for x in 1:1000 if x % 2 == 0)

println(result)
JULIA
```

The expression generates the required values and `sum` aggregates them.

This can avoid explicitly constructing an intermediate array.

---

# 30. Comprehension vs `map` vs `filter`

These tools solve related but different problems.

### Comprehension

Useful for constructing a new collection:

```julia
[x^2 for x in 1:5]
```

### `map`

Useful for applying a function to every element:

```julia
map(x -> x^2, x)
```

### `filter`

Useful for selecting elements:

```julia
filter(x -> x > 3, x)
```

### `reduce`

Useful for combining elements into one result:

```julia
reduce(+, x)
```

---

# 31. `sum` and `reduce`

`sum` is a specialized aggregation operation.

For example:

```python id="t5w8ny"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

println(sum(x))
println(reduce(+, x))
JULIA
```

Both calculate the sum in this example.

However, `reduce` is more general because the combining operation can be changed.

---

# 32. `prod` and Multiplicative Reduction

Similarly, `prod` performs multiplicative aggregation.

```python id="p4m9vz"
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4]

println(prod(x))
println(reduce(*, x))
JULIA
```

Both calculate the product in this example.

---

# 33. Complete Numerical Processing Example

The following example combines several concepts from this lesson.

```python id="y7c3qx"
%%bash
julia <<'JULIA'
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

filtered = filter(x -> x % 2 == 0, data)

squared = map(x -> x^2, filtered)

total = sum(squared)

println("Original data: ", data)
println("Filtered data: ", filtered)
println("Squared data: ", squared)
println("Total: ", total)
JULIA
```

The processing pipeline is:

```text
Original data
     ↓
filter
     ↓
Even values
     ↓
map
     ↓
Squared values
     ↓
sum
     ↓
Final numerical result
```

---

# 34. Functional Data-Processing Example

The same calculation can be expressed more compactly.

```python id="c8m5zr"
%%bash
julia <<'JULIA'
data = 1:10

result = sum(
    x^2 for x in data if x % 2 == 0
)

println(result)
JULIA
```

This demonstrates how generators can express a transformation, condition, and reduction in a compact form.

---

# 35. Practical Example — Scientific Calculation

Suppose a collection contains measurements and we want to calculate the total squared deviation from zero.

```python id="v4q9ks"
%%bash
julia <<'JULIA'
measurements = [-2.0, 1.5, -3.0, 4.0, -1.0]

squared = map(x -> x^2, measurements)

total = sum(squared)

println("Squared measurements: ", squared)
println("Total squared value: ", total)
JULIA
```

This type of processing appears frequently in numerical analysis and scientific computing.

---

# 36. Summary

This lesson introduced Julia's main collection transformation and aggregation tools.

## Array comprehensions

```julia
[x^2 for x in 1:5]
```

Used to construct new arrays from iterations.

## Conditional comprehensions

```julia
[x for x in 1:10 if x % 2 == 0]
```

Used to construct collections while applying a condition.

## Nested comprehensions

```julia
[i + j for i in 1:3, j in 1:3]
```

Used to construct multidimensional collections.

## Generator expressions

```julia
(x^2 for x in 1:100)
```

Generate values lazily for consumption by another operation.

## `map`

```julia
map(x -> x^2, x)
```

Transforms every element.

## `filter`

```julia
filter(x -> x > 0, x)
```

Selects elements satisfying a condition.

## `reduce`

```julia
reduce(+, x)
```

Combines collection elements into a single result.

## `sum`

```julia
sum(x)
```

Calculates the sum.

## `prod`

```julia
prod(x)
```

Calculates the product.

---

# Key Functional Programming Pattern

A very important pattern is:

```text
Collection
    ↓
filter
    ↓
map
    ↓
reduce
    ↓
Result
```

For example:

```julia
reduce(+, map(x -> x^2, filter(x -> x > 0, data)))
```

This expresses:

```text
select values
      ↓
transform values
      ↓
combine values
```

---

# Key Ideas to Remember

```text
Comprehension
    → construct a collection

Conditional comprehension
    → construct + select

Nested comprehension
    → construct multidimensional data

Generator
    → produce values without immediately constructing an array

map
    → transform every element

filter
    → select elements

reduce
    → combine elements

sum
    → numerical addition

prod
    → numerical multiplication
```

These operations provide the foundation for functional-style collection processing in Julia and are particularly useful for numerical data processing and scientific computing.
