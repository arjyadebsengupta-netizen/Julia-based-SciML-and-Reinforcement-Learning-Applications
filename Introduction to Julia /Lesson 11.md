# Lesson 11 — Ranges, Iterators and Generators

Introduction to Julia's iteration system.

## Topics

- Ranges
- `start:stop`
- `start:step:stop`
- Decreasing ranges
- Floating-point ranges
- `range`
- `collect`
- Range indexing
- Iteration
- `eachindex`
- `enumerate`
- `zip`
- Tuple iteration
- Dictionary iteration
- Generators
- Generator expressions
- Conditional generators
- Nested generators
- Generators versus comprehensions
- Lazy computation
- `sum`
- `maximum`
- `any`
- `all`
- `eachrow`
- `eachcol`
- Numerical grids
- Memory-efficient iteration

---

## 1. Ranges

Ranges represent sequences of values without necessarily storing every value as a full array.

### `start:stop`

The simplest range syntax is:

    1:5

This represents:

    1, 2, 3, 4, 5

You can assign a range to a variable:

    r = 1:5

    first(r)
    last(r)
    length(r)

---

### `start:step:stop`

Use this form when you want to specify the step size.

    1:2:10

This represents:

    1, 3, 5, 7, 9

Another example:

    0:5:20

This represents:

    0, 5, 10, 15, 20

The general syntax is:

    start:step:stop

---

## 2. Decreasing Ranges

A range can move downward by using a negative step.

    5:-1:1

This represents:

    5, 4, 3, 2, 1

Another example:

    10:-2:0

This represents:

    10, 8, 6, 4, 2, 0

The syntax remains:

    start:step:stop

The step is simply negative.

---

## 3. Floating-Point Ranges

Ranges can contain floating-point values.

    0.0:0.5:2.0

Conceptually gives:

    0.0, 0.5, 1.0, 1.5, 2.0

When you want to control the number of points explicitly, use `range`.

---

## 4. `range`

The `range` function is useful when you want to specify the number of points or the step explicitly.

### Specify the number of points

    range(0, 1, length=5)

Produces:

    0.0, 0.25, 0.5, 0.75, 1.0

### Specify a step and length

    range(0, step=0.2, length=6)

Produces:

    0.0, 0.2, 0.4, 0.6, 0.8, 1.0

### Important distinction

    1:2:10

means:

    Start at 1
    Step by 2
    Stop at or before 10

Whereas:

    range(0, 1, length=6)

means:

    Create 6 evenly spaced values from 0 to 1

---

## 5. `collect`

Ranges are lightweight iterable objects.

If you want to explicitly create an array containing all the values, use `collect`.

    r = 1:5
    collect(r)

Result:

    [1, 2, 3, 4, 5]

Compare:

    r = 1:1_000_000

with:

    a = collect(1:1_000_000)

The first represents the sequence as a range.

The second creates an actual array containing one million elements.

This distinction matters when working with large numerical problems.

---

## 6. Range Indexing

Ranges can be indexed.

    r = 10:2:20

The values are:

    10, 12, 14, 16, 18, 20

Therefore:

    r[1]

returns:

    10

And:

    r[3]

returns:

    14

You can inspect the valid indices:

    firstindex(r)
    lastindex(r)

---

## 7. Iteration

Iteration means processing elements one at a time.

A basic `for` loop:

    for x in 1:5
        println(x)
    end

Output:

    1
    2
    3
    4
    5

The important idea is:

> An iterable object provides values that can be processed one at a time.

Common iterable objects include:

- Arrays
- Ranges
- Tuples
- Dictionaries
- Generators

---

## 8. `eachindex`

For arrays, `eachindex` provides their valid indices.

    A = [10, 20, 30]

    for i in eachindex(A)
        println(A[i])
    end

Output:

    10
    20
    30

`eachindex` is generally preferable to manually assuming that valid indices are:

    1:length(A)

because it is designed to work correctly with different array indexing schemes.

---

## 9. `enumerate`

Sometimes you need both the index and the value.

    A = ["a", "b", "c"]

    for (i, x) in enumerate(A)
        println(i, " => ", x)
    end

Output:

    1 => a
    2 => b
    3 => c

Conceptually:

    enumerate(A)

produces:

    (1, "a")
    (2, "b")
    (3, "c")

---

## 10. `zip`

`zip` lets you iterate over multiple collections simultaneously.

    names = ["Alice", "Bob", "Charlie"]
    scores = [90, 85, 95]

    for (name, score) in zip(names, scores)
        println(name, ": ", score)
    end

Output:

    Alice: 90
    Bob: 85
    Charlie: 95

Conceptually:

    zip(names, scores)

produces:

    ("Alice", 90)
    ("Bob", 85)
    ("Charlie", 95)

---

## 11. Tuple Iteration

Tuples are iterable.

    t = (10, 20, 30)

    for x in t
        println(x)
    end

You can also destructure tuples during iteration.

    points = ((1, 2), (3, 4), (5, 6))

    for (x, y) in points
        println(x, " ", y)
    end

This is useful when working with coordinate pairs.

---

## 12. Dictionary Iteration

Dictionaries can also be iterated.

    d = Dict(
        "Alice" => 90,
        "Bob" => 85
    )

Iterating directly:

    for item in d
        println(item)
    end

Each item is a key-value pair.

You can destructure the pairs:

    for (name, score) in d
        println(name, ": ", score)
    end

You can also explicitly iterate over:

    keys(d)
    values(d)
    pairs(d)

For example:

    for name in keys(d)
        println(name)
    end

---

## 13. Generators

A generator produces values lazily.

Instead of immediately constructing a complete array, Julia can describe how to produce each value when it is needed.

    g = (x^2 for x in 1:5)

The generator represents:

    1², 2², 3², 4², 5²

without first constructing:

    [1, 4, 9, 16, 25]

---

## 14. Generator Expressions

The general syntax is:

    (expression for variable in collection)

Example:

    (x^2 for x in 1:10)

Another example:

    (sin(x) for x in 0:0.1:1)

The expression is evaluated as values are requested.

---

## 15. Conditional Generators

Generators can contain conditions.

    (x^2 for x in 1:10 if iseven(x))

Only values satisfying the condition participate.

Conceptually, the generated values are:

    4, 16, 36, 64, 100

---

## 16. Nested Generators

Generators can contain multiple iteration variables.

    (x + y for x in 1:3, y in 1:3)

This represents combinations of `x` and `y`.

A condition can also be applied:

    ((x, y) for x in 1:3, y in 1:3 if x != y)

The general idea is similar to:

    for x
        for y
            produce something

but the values are generated lazily.

---

## 17. Generators versus Comprehensions

These two constructs look very similar.

### Generator

    (x^2 for x in 1:5)

### Comprehension

    [x^2 for x in 1:5]

The comprehension creates an array immediately:

    [1, 4, 9, 16, 25]

The generator creates a lazy iterable.

You can turn a generator into an array using `collect`:

    collect(x^2 for x in 1:5)

---

## 18. Lazy Computation

Consider:

    sum(x^2 for x in 1:1_000_000)

This allows `sum` to consume the generated values directly.

You do not need to first create:

    [x^2 for x in 1:1_000_000]

The general pattern is:

    generator → consumer

rather than:

    generator → array → consumer

This can reduce unnecessary memory allocation.

---

## 19. `sum`

`sum` can consume generators directly.

    sum(x^2 for x in 1:10)

This computes:

    sum(x^2 for x in 1:10)

You can also use conditions:

    sum(x^2 for x in 1:10 if iseven(x))

---

## 20. `maximum`

Generators can also be passed to `maximum`.

    maximum(x^2 for x in 1:10)

Result:

    100

No intermediate array is required.

---

## 21. `any`

`any` checks whether at least one element satisfies a condition.

    any(x > 10 for x in 1:20)

Result:

    true

You can also use the function form:

    any(iszero, [1, 2, 0, 4])

Result:

    true

---

## 22. `all`

`all` checks whether every element satisfies a condition.

    all(x > 0 for x in 1:10)

Result:

    true

Whereas:

    all(x > 5 for x in 1:10)

returns:

    false

---

## 23. `eachrow`

For a matrix:

    A = [
        1 2 3
        4 5 6
        7 8 9
    ]

`eachrow(A)` lets you iterate over rows.

    for row in eachrow(A)
        println(row)
    end

Conceptually, the rows are:

    [1, 2, 3]
    [4, 5, 6]
    [7, 8, 9]

---

## 24. `eachcol`

Similarly, `eachcol(A)` lets you iterate over columns.

    for col in eachcol(A)
        println(col)
    end

Conceptually, the columns are:

    [1, 4, 7]
    [2, 5, 8]
    [3, 6, 9]

This is useful for matrix-based scientific computations.

---

## 25. Numerical Grids

Ranges and iteration are extremely useful for constructing numerical grids.

For example:

    x = range(0, 1, length=101)

This represents a one-dimensional grid:

    x₀, x₁, ..., x₁₀₀

You can construct a two-dimensional grid using nested iteration:

    grid = ((x, y) for x in range(0, 1, length=101),
                       y in range(0, 1, length=101))

This represents coordinate pairs:

    (x₁, y₁)
    (x₁, y₂)
    ...
    (x₂, y₁)
    ...

without necessarily materializing all coordinate pairs.

For scientific ML and PDE work, this idea is important because numerical grids can become extremely large as dimensionality increases.

---

## 26. Memory-Efficient Iteration

Compare:

    sum(x^2 for x in 1:10_000_000)

with:

    sum([x^2 for x in 1:10_000_000])

The first allows `sum` to consume generated values directly.

The second explicitly constructs a large intermediate array first.

The general pattern is:

    range
       ↓
    generator
       ↓
    consumer

For example:

    sum(f(x) for x in range)

    maximum(f(x) for x in range)

    any(condition(x) for x in range)

    all(condition(x) for x in range)

This is an important Julia pattern for writing memory-efficient numerical code.

---

# 27. Core Mental Model

Keep these distinctions clear:

    Range
        ↓
    describes a numerical sequence

    Iterator
        ↓
    provides values one at a time

    Generator
        ↓
    computes values lazily

    Comprehension
        ↓
    materializes a collection

    collect
        ↓
    materializes an iterable into an array

A useful progression is:

    1:10

→ range

    (x^2 for x in 1:10)

→ generator

    [x^2 for x in 1:10]

→ array comprehension

    collect(x^2 for x in 1:10)

→ explicitly materialized generator

And finally:

    sum(x^2 for x in 1:10)

→ consume the generator directly without first creating the array.

---

# 28. Practical Scientific Computing Pattern

Ranges, iteration, and generators are particularly useful in numerical computing.

For example:

    x = range(0, 2π, length=1000)

    maximum(sin(xi) for xi in x)

Here:

- `x` is a range.
- `xi` is produced one value at a time.
- `sin(xi)` is computed when needed.
- `maximum` consumes the generated values.
- No explicit array of `sin(x)` values is required.

Another example:

    total = sum(
        x^2 + y^2
        for x in 1:1000
        for y in 1:1000
    )

This expresses a large nested computation without explicitly creating a million-element intermediate array.

---

# 29. Key Takeaways

1. `start:stop` creates a range.

2. `start:step:stop` creates a range with an explicit step.

3. Negative steps create decreasing ranges.

4. `range` is useful when controlling spacing or the number of points.

5. `collect` materializes an iterable into an array.

6. Iteration processes values one at a time.

7. `eachindex` provides valid indices for an array.

8. `enumerate` provides index-value pairs.

9. `zip` combines multiple iterables for simultaneous iteration.

10. Tuples and dictionaries are iterable.

11. Generators compute values lazily.

12. Generator expressions use the form:

        (expression for variable in collection)

13. Generator expressions can include conditions.

14. Generators can contain nested iteration.

15. Comprehensions create collections; generators remain lazy.

16. `sum`, `maximum`, `any`, and `all` can consume generators directly.

17. `eachrow` iterates over matrix rows.

18. `eachcol` iterates over matrix columns.

19. Ranges and generators are useful for numerical grids.

20. Lazy iteration can avoid unnecessary intermediate arrays and reduce memory usage.
