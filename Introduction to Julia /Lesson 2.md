# Lesson 2 — Control Flow

## Introduction

Control flow determines **which statements Julia executes, when they are executed, and how many times they are executed**.

In this lesson, we introduce the fundamental mechanisms Julia provides for controlling program execution:

* `if`
* `elseif`
* `else`
* Conditional expressions
* `while`
* `for`
* Nested loops
* `break`
* `continue`
* Boolean conditions
* Iteration over collections
* Loop variables
* Practical control-flow patterns

The examples use Julia's standard syntax and can be executed in Google Colab using the same `%%bash` and Julia here-document workflow introduced in Lecture 0.5.

---

# 1. `if`

The `if` statement executes a block of code when a condition is `true`.

### Syntax

```julia
if condition
    statements
end
```

### Example

```python
%%bash
julia <<'JULIA'
x = 10

if x > 5
    println("x is greater than 5")
end
JULIA
```

The condition must evaluate to a Boolean value.

---

# 2. `else`

`else` provides an alternative block when the `if` condition is `false`.

```python
%%bash
julia <<'JULIA'
x = 3

if x > 5
    println("x is greater than 5")
else
    println("x is not greater than 5")
end
JULIA
```

Only one of the two branches is executed.

---

# 3. `elseif`

`elseif` allows multiple conditions to be tested sequentially.

```python
%%bash
julia <<'JULIA'
x = 75

if x >= 90
    println("Grade A")
elseif x >= 75
    println("Grade B")
elseif x >= 60
    println("Grade C")
else
    println("Grade D")
end
JULIA
```

Julia checks the conditions from top to bottom.

The first condition that evaluates to `true` determines the executed branch.

---

# 4. Boolean Conditions

Control-flow statements commonly use Boolean expressions.

A Boolean value is either:

```julia
true
```

or

```julia
false
```

### Comparisons

```python
%%bash
julia <<'JULIA'
x = 10

println(x > 5)
println(x < 5)
println(x == 10)
println(x != 10)
println(x >= 10)
println(x <= 10)
JULIA
```

These expressions can be used directly in conditional statements.

---

# 5. Combining Boolean Conditions

Multiple conditions can be combined using logical operators.

### AND — `&&`

Both conditions must be true.

```python
%%bash
julia <<'JULIA'
x = 10

if x > 5 && x < 20
    println("x is between 5 and 20")
end
JULIA
```

### OR — `||`

At least one condition must be true.

```python
%%bash
julia <<'JULIA'
x = 25

if x < 10 || x > 20
    println("Condition satisfied")
end
JULIA
```

### NOT — `!`

`!` reverses a Boolean value.

```python
%%bash
julia <<'JULIA'
x = 10

if !(x < 5)
    println("x is not less than 5")
end
JULIA
```

---

# 6. Conditional Expressions

Julia also provides a compact conditional expression using the ternary operator:

```julia
condition ? value_if_true : value_if_false
```

### Example

```python
%%bash
julia <<'JULIA'
x = 10

result = x > 5 ? "large" : "small"

println(result)
JULIA
```

The expression evaluates to one of two values depending on the condition.

Conditional expressions are useful when the decision is simple.

For more complicated logic, an `if` block is generally clearer.

---

# 7. `while`

A `while` loop repeatedly executes a block of code **while a condition remains true**.

### Syntax

```julia
while condition
    statements
end
```

### Example

```python
%%bash
julia <<'JULIA'
x = 1

while x <= 5
    println(x)
    x += 1
end
JULIA
```

Output:

```text
1
2
3
4
5
```

The condition is checked before every iteration.

---

# 8. Loop Variables

A loop often requires a variable whose value changes during execution.

In the previous example:

```julia
x += 1
```

updates the loop variable.

The sequence is:

```text
x = 1
↓
check condition
↓
execute loop body
↓
increase x
↓
check condition again
```

A `while` loop therefore requires careful management of the condition and variables controlling the loop.

---

# 9. `for`

A `for` loop iterates over an iterable object.

### Basic syntax

```julia
for variable in iterable
    statements
end
```

### Example

```python
%%bash
julia <<'JULIA'
for i in 1:5
    println(i)
end
JULIA
```

Here:

* `i` is the loop variable.
* `1:5` is the iterable.
* The loop executes once for each value.

---

# 10. Iteration Over Collections

`for` loops can iterate over collections.

For example, an array:

```python
%%bash
julia <<'JULIA'
numbers = [10, 20, 30, 40, 50]

for x in numbers
    println(x)
end
JULIA
```

The loop variable `x` takes each element of the collection in sequence.

---

# 11. Iterating Over Strings

Strings can also be iterated over.

```python
%%bash
julia <<'JULIA'
word = "Julia"

for character in word
    println(character)
end
JULIA
```

The loop variable receives each character encountered during iteration.

---

# 12. Looping Over a Range

Ranges are particularly common in Julia loops.

```python
%%bash
julia <<'JULIA'
for i in 1:10
    println(i)
end
JULIA
```

A range can also have a step:

```python
%%bash
julia <<'JULIA'
for i in 2:2:10
    println(i)
end
JULIA
```

This produces:

```text
2
4
6
8
10
```

---

# 13. Nested Loops

A loop can contain another loop.

This is called a **nested loop**.

### Example

```python
%%bash
julia <<'JULIA'
for i in 1:3
    for j in 1:3
        println("i = ", i, ", j = ", j)
    end
end
JULIA
```

The inner loop completes its iterations for each iteration of the outer loop.

Conceptually:

```text
i = 1
    j = 1
    j = 2
    j = 3

i = 2
    j = 1
    j = 2
    j = 3

i = 3
    j = 1
    j = 2
    j = 3
```

Nested loops are important for operations involving multiple indices, grids, matrices, and pairwise computations.

---

# 14. `break`

`break` immediately terminates the loop in which it occurs.

### Example

```python
%%bash
julia <<'JULIA'
for i in 1:10
    if i == 5
        break
    end

    println(i)
end
JULIA
```

Output:

```text
1
2
3
4
```

When `i == 5`, `break` terminates the loop.

---

# 15. `continue`

`continue` skips the remainder of the current iteration and proceeds to the next iteration.

### Example

```python
%%bash
julia <<'JULIA'
for i in 1:5
    if i == 3
        continue
    end

    println(i)
end
JULIA
```

Output:

```text
1
2
4
5
```

The iteration where `i == 3` is skipped.

---

# 16. `break` vs `continue`

The two statements have different effects.

| Statement  | Effect                      |
| ---------- | --------------------------- |
| `break`    | Terminates the loop         |
| `continue` | Skips the current iteration |

For example:

```julia
break
```

means:

> Stop looping.

Whereas:

```julia
continue
```

means:

> Skip this iteration and continue looping.

---

# 17. Conditional Logic Inside Loops

Conditions and loops are commonly combined.

```python
%%bash
julia <<'JULIA'
for i in 1:10
    if i % 2 == 0
        println(i, " is even")
    else
        println(i, " is odd")
    end
end
JULIA
```

Here the `for` loop controls repetition while the `if` statement controls what happens during each iteration.

This combination is one of the most fundamental control-flow patterns in programming.

---

# 18. Searching With a Loop

A common control-flow pattern is searching for a particular value.

```python
%%bash
julia <<'JULIA'
numbers = [4, 8, 15, 16, 23, 42]

for x in numbers
    if x == 23
        println("Found 23")
        break
    end
end
JULIA
```

The loop stops once the required value is found.

---

# 19. Counting With a Loop

A loop can maintain a counter.

```python
%%bash
julia <<'JULIA'
numbers = [1, 4, 7, 2, 9, 6]

count = 0

for x in numbers
    if x > 5
        count += 1
    end
end

println("Number of values greater than 5: ", count)
JULIA
```

The variable `count` records how many elements satisfy the condition.

---

# 20. Accumulation With a Loop

Another important pattern is accumulating a result.

```python
%%bash
julia <<'JULIA'
total = 0

for i in 1:10
    total += i
end

println(total)
JULIA
```

Here `total` is updated during every iteration.

The sequence is:

```text
total = 0
total = 0 + 1
total = 1 + 2
total = 3 + 3
...
```

This pattern is useful for sums, products, statistics, and many numerical algorithms.

---

# 21. Nested Conditional Logic

Conditional statements can also be nested.

```python
%%bash
julia <<'JULIA'
x = 15

if x > 0
    if x > 10
        println("Positive and greater than 10")
    else
        println("Positive but not greater than 10")
    end
else
    println("Not positive")
end
JULIA
```

The inner `if` is evaluated only when the outer condition is satisfied.

---

# 22. Combining Nested Loops and Conditions

A practical numerical pattern is to inspect every pair of values and apply a condition.

```python
%%bash
julia <<'JULIA'
for i in 1:4
    for j in 1:4
        if i == j
            println("Diagonal: ", i, ", ", j)
        end
    end
end
JULIA
```

This structure is useful when working with two-dimensional numerical data.

---

# 23. Practical Control-Flow Pattern: Classification

A value can be classified using multiple conditions.

```python
%%bash
julia <<'JULIA'
x = 72

if x >= 90
    println("Excellent")
elseif x >= 75
    println("Good")
elseif x >= 50
    println("Pass")
else
    println("Fail")
end
JULIA
```

The conditions are evaluated sequentially.

---

# 24. Practical Control-Flow Pattern: Filtering

A loop can be used to process only elements satisfying a condition.

```python
%%bash
julia <<'JULIA'
numbers = 1:10

for x in numbers
    if x % 2 == 0
        println(x)
    end
end
JULIA
```

Only even numbers are processed.

---

# 25. Practical Control-Flow Pattern: Early Termination

`break` is useful when continuing the loop is unnecessary.

```python
%%bash
julia <<'JULIA'
for x in 1:100
    if x^2 > 50
        println("First value whose square exceeds 50: ", x)
        break
    end
end
JULIA
```

Once the required condition is reached, the loop terminates.

---

# 26. Practical Control-Flow Pattern: Skipping Values

`continue` is useful when particular values should be ignored.

```python
%%bash
julia <<'JULIA'
for x in 1:10
    if x % 2 != 0
        continue
    end

    println("Processing: ", x)
end
JULIA
```

Odd values are skipped.

---

# 27. `while` vs `for`

Both are looping constructs, but they are generally used for different situations.

### `for`

Use `for` when iterating over a known iterable:

```julia
for x in collection
    ...
end
```

or:

```julia
for i in 1:10
    ...
end
```

### `while`

Use `while` when repetition depends primarily on a condition:

```julia
while condition
    ...
end
```

The distinction is not an absolute rule, but it is a useful way to choose between the two constructs.

---

# 28. Complete Control-Flow Example

The following example combines several concepts from this lesson.

```python
%%bash
julia <<'JULIA'
numbers = [3, 8, 12, 5, 17, 20]

count = 0
total = 0

for x in numbers

    if x < 5
        continue
    elseif x > 15
        println("Large value: ", x)
    else
        println("Value: ", x)
    end

    total += x
    count += 1
end

println("Count: ", count)
println("Total: ", total)
JULIA
```

This example demonstrates how loops, Boolean conditions, `continue`, `elseif`, and accumulation can work together.

---

# 29. Key Syntax

## Conditional execution

```julia
if condition
    ...
elseif condition
    ...
else
    ...
end
```

## Conditional expression

```julia
condition ? value1 : value2
```

## `while` loop

```julia
while condition
    ...
end
```

## `for` loop

```julia
for variable in iterable
    ...
end
```

## Loop termination

```julia
break
```

## Skip current iteration

```julia
continue
```

---

# 30. Summary

Control flow allows Julia programs to make decisions and repeat operations.

The fundamental constructs introduced in this lesson are:

* `if` — executes code conditionally.
* `elseif` — tests additional conditions.
* `else` — provides an alternative branch.
* Conditional expressions — compact two-way decisions.
* `while` — repeats while a condition is true.
* `for` — iterates over an iterable.
* Boolean conditions — determine control-flow decisions.
* Loop variables — represent the current iteration value.
* Iteration over collections — processes elements sequentially.
* Nested loops — place one loop inside another.
* `break` — terminates a loop.
* `continue` — skips the current iteration.

The central idea is:

```text
condition → decision
iteration → repetition
break → termination
continue → skip
nested control flow → combine these mechanisms
```

These constructs form the foundation for implementing algorithms, numerical methods, simulations, data processing, and scientific-computing workflows in Julia.
