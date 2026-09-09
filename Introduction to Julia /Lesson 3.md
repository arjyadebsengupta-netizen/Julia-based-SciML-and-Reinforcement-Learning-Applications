# Lesson 3 — Functions

## Introduction

Functions are reusable blocks of code that perform a specific operation.

Instead of repeatedly writing the same instructions, we can define a function once and call it whenever we need it.

Functions are fundamental to Julia programming and are especially important in scientific computing, where mathematical operations and numerical procedures are naturally expressed as functions.

In this lesson, we introduce:

* Defining functions
* Function syntax
* Calling functions
* Arguments
* Parameters
* Return values
* `return`
* Multiple arguments
* Default arguments
* Keyword arguments
* Anonymous functions
* Short-form function syntax
* Functions with conditional logic
* Functions with loops
* Multiple return values
* Practical function patterns

---

# 1. Defining a Function

A function can be defined using the `function` keyword.

### Syntax

```julia
function function_name(parameters)
    statements
end
```

### Example

```python
%%bash
julia <<'JULIA'
function greet()
    println("Hello from Julia")
end
JULIA
```

The function has now been defined.

Defining a function does not execute its body.

The function must be called for its code to run.

---

# 2. Calling a Function

A function is called using its name followed by parentheses.

```python
%%bash
julia <<'JULIA'
function greet()
    println("Hello from Julia")
end

greet()
JULIA
```

Output:

```text
Hello from Julia
```

The function body executes when `greet()` is called.

---

# 3. Functions With Arguments

Functions can receive information through arguments.

```python
%%bash
julia <<'JULIA'
function greet(name)
    println("Hello, ", name)
end

greet("Julia")
JULIA
```

Output:

```text
Hello, Julia
```

Here:

* `name` is the parameter of the function.
* `"Julia"` is the argument supplied when calling the function.

---

# 4. Parameters and Arguments

The terms **parameter** and **argument** refer to different things.

In:

```julia
function square(x)
    return x^2
end
```

`x` is a **parameter**.

When we write:

```julia
square(5)
```

`5` is an **argument**.

The parameter receives the value of the argument when the function is called.

---

# 5. Returning a Value

Functions can produce a result.

```python
%%bash
julia <<'JULIA'
function square(x)
    return x^2
end

result = square(5)

println(result)
JULIA
```

Output:

```text
25
```

The value returned by the function can be assigned to a variable.

---

# 6. The `return` Statement

The `return` keyword explicitly specifies the value produced by a function.

```python
%%bash
julia <<'JULIA'
function add(a, b)
    return a + b
end

result = add(10, 20)

println(result)
JULIA
```

Output:

```text
30
```

When Julia reaches `return`, the function returns that value and stops executing the function body.

---

# 7. Functions Without an Explicit `return`

Julia functions do not always require the `return` keyword.

The value of the final expression is returned automatically.

```python
%%bash
julia <<'JULIA'
function square(x)
    x^2
end

println(square(6))
JULIA
```

Output:

```text
36
```

The final expression:

```julia
x^2
```

becomes the return value.

This is very common in Julia.

---

# 8. Short-Form Function Syntax

Simple functions can be written using the short-form syntax:

```julia
function_name(arguments) = expression
```

For example:

```python
%%bash
julia <<'JULIA'
square(x) = x^2

println(square(7))
JULIA
```

Output:

```text
49
```

This is equivalent to:

```julia
function square(x)
    x^2
end
```

Short-form syntax is particularly convenient for simple mathematical functions.

---

# 9. Multiple Arguments

A function can accept multiple arguments.

```python
%%bash
julia <<'JULIA'
function add(a, b)
    return a + b
end

println(add(3, 4))
JULIA
```

Another example:

```python
%%bash
julia <<'JULIA'
function multiply(a, b)
    return a * b
end

println(multiply(6, 5))
JULIA
```

The arguments are passed according to their position.

---

# 10. Functions With Three or More Arguments

Functions are not restricted to two arguments.

```python
%%bash
julia <<'JULIA'
function average(a, b, c)
    return (a + b + c) / 3
end

println(average(10, 20, 30))
JULIA
```

The three supplied arguments correspond to the three parameters.

---

# 11. Default Arguments

Julia allows functions to define default values for arguments.

### Example

```python
%%bash
julia <<'JULIA'
function power(x, n=2)
    return x^n
end

println(power(5))
println(power(5, 3))
JULIA
```

Output:

```text
25
125
```

When `n` is not supplied, Julia uses the default value `2`.

When `n` is supplied, that value is used instead.

---

# 12. Keyword Arguments

Keyword arguments are specified using a semicolon in the function definition.

### Example

```python
%%bash
julia <<'JULIA'
function greet(name; message="Hello")
    println(message, ", ", name)
end

greet("Julia")
greet("Julia", message="Welcome")
JULIA
```

Output:

```text
Hello, Julia
Welcome, Julia
```

Keyword arguments are supplied using their parameter names.

---

# 13. Positional Arguments vs Keyword Arguments

Consider:

```julia
function example(a, b; c=10)
    ...
end
```

Here:

* `a` and `b` are positional arguments.
* `c` is a keyword argument.

The function can be called as:

```julia
example(1, 2)
```

or:

```julia
example(1, 2, c=20)
```

---

# 14. Anonymous Functions

Julia allows functions to be created without giving them a name.

These are called **anonymous functions**.

### Syntax

```julia
x -> expression
```

### Example

```python
%%bash
julia <<'JULIA'
f = x -> x^2

println(f(5))
JULIA
```

Output:

```text
25
```

Here:

```julia
x -> x^2
```

creates an anonymous function.

The function is then assigned to `f`.

---

# 15. Anonymous Functions With Multiple Arguments

Anonymous functions can also accept multiple arguments.

```python
%%bash
julia <<'JULIA'
f = (x, y) -> x + y

println(f(10, 20))
JULIA
```

Output:

```text
30
```

---

# 16. Functions With Conditional Logic

Functions can contain `if` statements.

```python
%%bash
julia <<'JULIA'
function classify(x)

    if x > 0
        return "positive"
    elseif x < 0
        return "negative"
    else
        return "zero"
    end

end

println(classify(10))
println(classify(-5))
println(classify(0))
JULIA
```

Output:

```text
positive
negative
zero
```

This combines the concepts from **Lesson 2 — Control Flow** with functions.

---

# 17. Functions With Boolean Conditions

Functions can make decisions based on Boolean expressions.

```python
%%bash
julia <<'JULIA'
function is_even(x)

    if x % 2 == 0
        return true
    else
        return false
    end

end

println(is_even(10))
println(is_even(7))
JULIA
```

Output:

```text
true
false
```

A shorter version is:

```python
%%bash
julia <<'JULIA'
is_even(x) = x % 2 == 0

println(is_even(10))
println(is_even(7))
JULIA
```

---

# 18. Functions With Loops

Functions can contain loops.

```python
%%bash
julia <<'JULIA'
function sum_numbers(n)

    total = 0

    for i in 1:n
        total += i
    end

    return total
end

println(sum_numbers(10))
JULIA
```

Output:

```text
55
```

The function combines:

* a parameter,
* a loop,
* a local variable,
* accumulation,
* and a return value.

---

# 19. Functions With `while`

A function can also contain a `while` loop.

```python
%%bash
julia <<'JULIA'
function count_down(n)

    while n > 0
        println(n)
        n -= 1
    end

end

count_down(5)
JULIA
```

Output:

```text
5
4
3
2
1
```

The function controls the entire looping operation.

---

# 20. Multiple Return Values

Julia functions can return multiple values.

```python
%%bash
julia <<'JULIA'
function operations(a, b)

    sum_value = a + b
    product_value = a * b

    return sum_value, product_value
end

result = operations(5, 3)

println(result)
JULIA
```

A function can therefore produce several results from a single call.

---

# 21. Unpacking Multiple Return Values

Multiple returned values can be assigned to multiple variables.

```python
%%bash
julia <<'JULIA'
function operations(a, b)

    sum_value = a + b
    product_value = a * b

    return sum_value, product_value
end

sum_value, product_value = operations(5, 3)

println("Sum: ", sum_value)
println("Product: ", product_value)
JULIA
```

Output:

```text
Sum: 8
Product: 15
```

This is useful when a calculation naturally produces several related results.

---

# 22. Local Variables Inside Functions

Variables created inside a function are normally local to that function.

```python
%%bash
julia <<'JULIA'
function calculate(x)

    result = x^2

    return result
end

println(calculate(5))
JULIA
```

The variable `result` belongs to the function's local scope.

This helps prevent internal variables from interfering with unrelated parts of a program.

---

# 23. Functions Calling Other Functions

A function can call another function.

```python
%%bash
julia <<'JULIA'
function square(x)
    return x^2
end

function double_square(x)
    return 2 * square(x)
end

println(double_square(5))
JULIA
```

Output:

```text
50
```

Here:

```text
double_square
      ↓
   square
      ↓
     x²
```

Breaking a larger calculation into smaller functions can make programs easier to understand and reuse.

---

# 24. Practical Pattern: Mathematical Functions

Functions provide a natural way to represent mathematical expressions.

```python
%%bash
julia <<'JULIA'
f(x) = x^3 - 2x + 1

println(f(2))
println(f(5))
JULIA
```

This style is particularly important in scientific computing.

A mathematical function can be defined once and evaluated at different values.

---

# 25. Practical Pattern: Reusable Calculations

Suppose a calculation is needed several times.

Instead of repeating the calculation:

```python
%%bash
julia <<'JULIA'
function kinetic_energy(mass, velocity)
    return 0.5 * mass * velocity^2
end

println(kinetic_energy(2.0, 3.0))
println(kinetic_energy(5.0, 10.0))
JULIA
```

The calculation is defined once and reused.

---

# 26. Practical Pattern: Validation

A function can perform a check before producing a result.

```python
%%bash
julia <<'JULIA'
function reciprocal(x)

    if x == 0
        return "Undefined"
    end

    return 1 / x
end

println(reciprocal(5))
println(reciprocal(0))
JULIA
```

Control flow can therefore be used inside functions to handle different situations.

---

# 27. Practical Pattern: Searching

A function can perform a search using a loop.

```python
%%bash
julia <<'JULIA'
function find_value(target)

    for x in 1:100
        if x == target
            return x
        end
    end

    return nothing
end

println(find_value(25))
println(find_value(150))
JULIA
```

The `return` statement can terminate the function immediately when the desired value is found.

---

# 28. Practical Pattern: Accumulation

A function can encapsulate an accumulation algorithm.

```python
%%bash
julia <<'JULIA'
function factorial(n)

    result = 1

    for i in 1:n
        result *= i
    end

    return result
end

println(factorial(5))
JULIA
```

Output:

```text
120
```

The function combines a loop with an accumulating variable.

---

# 29. Practical Pattern: Functions With Default Parameters

Default arguments can make a function flexible.

```python
%%bash
julia <<'JULIA'
function power(x, exponent=2)
    return x^exponent
end

println(power(4))
println(power(4, 3))
println(power(4, 4))
JULIA
```

The same function can therefore be used for several related calculations.

---

# 30. Complete Example

The following example combines several concepts from this lesson.

```python
%%bash
julia <<'JULIA'
function classify_number(x; threshold=10)

    if x > threshold
        return "large"
    elseif x < 0
        return "negative"
    else
        return "small or equal"
    end

end

println(classify_number(15))
println(classify_number(5))
println(classify_number(-2))
println(classify_number(25, threshold=20))
JULIA
```

This example combines:

* function definition,
* parameters,
* keyword arguments,
* default values,
* Boolean conditions,
* `if`,
* `elseif`,
* `else`,
* and return values.

---

# 31. Function Syntax Summary

## Standard function

```julia
function name(parameters)
    statements
end
```

## Short-form function

```julia
name(parameters) = expression
```

## Function call

```julia
name(arguments)
```

## Default argument

```julia
function name(x, y=default)
    ...
end
```

## Keyword argument

```julia
function name(x; y=default)
    ...
end
```

## Anonymous function

```julia
x -> expression
```

## Multiple return values

```julia
return value1, value2
```

---

# 32. Key Concepts

A function has several important components:

```text
function definition
       ↓
parameters
       ↓
function body
       ↓
computation
       ↓
return value
```

When the function is called:

```text
arguments
    ↓
parameters receive values
    ↓
function executes
    ↓
result is returned
```

Functions allow code to become:

* reusable,
* modular,
* easier to test,
* easier to understand,
* and easier to combine into larger programs.

---

# 33. Summary

In this lesson, we learned how to create and use functions in Julia.

The main concepts were:

* Defining functions with `function`.
* Calling functions.
* Passing arguments.
* Using parameters.
* Returning values with `return`.
* Using implicit returns.
* Writing short-form functions.
* Using multiple arguments.
* Defining default arguments.
* Using keyword arguments.
* Creating anonymous functions.
* Combining functions with conditional logic.
* Combining functions with loops.
* Returning multiple values.
* Using functions to organize reusable calculations.
* Having functions call other functions.

The central idea is:

```text
Function
   ↓
Input
   ↓
Computation
   ↓
Output
```

Functions are one of the most important building blocks of Julia programming. They become especially powerful when combined with the control-flow mechanisms introduced in Lesson 2.
