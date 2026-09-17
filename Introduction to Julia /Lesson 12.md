# Lesson 12 — Errors and Exception Handling

Introduction to handling errors and exceptional situations in Julia.

## Topics

- Exceptions
- `error`
- `throw`
- `ArgumentError`
- `try`
- `catch`
- Exception objects
- `showerror`
- Built-in exception types
- `BoundsError`
- `KeyError`
- Checking exception types
- `tryparse`
- `finally`
- `rethrow`
- Custom exception types
- `@assert`
- Input validation
- Exception propagation
- `nothing`
- Expected versus unexpected errors
- Scientific validation

---

## 1. Exceptions

An exception is an object representing an error or exceptional situation during program execution.

For example:

    x = [10, 20, 30]
    x[10]

This produces a `BoundsError` because index `10` is outside the valid range of the array.

Exceptions interrupt the normal flow of execution unless they are handled.

---

## 2. `error`

The `error` function immediately stops execution and throws an exception.

    error("Something went wrong")

Example:

    function divide_positive(a, b)
        if b == 0
            error("Division by zero")
        end

        return a / b
    end

Calling:

    divide_positive(10, 0)

produces an error.

Use `error` when the program reaches a situation that should not continue normally.

---

## 3. `throw`

`throw` explicitly throws an exception object.

For example:

    throw(ArgumentError("Invalid argument"))

This differs from:

    error("Invalid argument")

because `throw` allows you to specify the particular exception type.

General pattern:

    throw(exception_object)

---

## 4. `ArgumentError`

`ArgumentError` is appropriate when a function receives an invalid argument.

Example:

    function square_root_positive(x)
        if x < 0
            throw(ArgumentError("x must be non-negative"))
        end

        return sqrt(x)
    end

Now:

    square_root_positive(-4)

throws an `ArgumentError`.

This communicates that the problem is with the argument supplied to the function.

---

## 5. `try`

A `try` block allows code to be executed while providing a mechanism for handling exceptions.

Basic structure:

    try
        # code that may fail
    catch
        # code executed if an exception occurs
    end

Example:

    try
        x = [1, 2, 3]
        println(x[10])
    catch
        println("An error occurred")
    end

The program does not terminate at the exception because it is caught.

---

## 6. `catch`

`catch` handles an exception produced inside the corresponding `try` block.

Example:

    try
        error("Something failed")
    catch
        println("The error was caught")
    end

You can also capture the exception object:

    try
        error("Something failed")
    catch e
        println("Exception: ", e)
    end

The variable `e` contains the exception object.

---

## 7. Exception Objects

Exceptions are objects.

For example:

    e = ArgumentError("Invalid input")

You can inspect the object:

    typeof(e)

which gives:

    ArgumentError

You can then throw it:

    throw(e)

Capturing exception objects is useful when you need to inspect or report details about a failure.

---

## 8. `showerror`

`showerror` displays an exception in Julia's standard error representation.

Example:

    e = ArgumentError("Invalid input")

    showerror(stdout, e)

You can use this when you need more controlled exception output.

Inside a `catch` block:

    try
        throw(ArgumentError("Invalid input"))
    catch e
        showerror(stdout, e)
    end

---

## 9. Built-in Exception Types

Julia provides many built-in exception types.

Examples include:

- `ArgumentError`
- `BoundsError`
- `KeyError`
- `DimensionMismatch`
- `DivideError`
- `DomainError`
- `MethodError`
- `OverflowError`
- `TypeError`
- `UndefVarError`

Different exception types communicate different kinds of failures.

---

## 10. `BoundsError`

A `BoundsError` occurs when an index is outside the valid bounds of a collection.

Example:

    A = [10, 20, 30]

    A[5]

The valid indices are:

    1, 2, 3

Therefore index `5` produces a `BoundsError`.

You can catch it specifically:

    try
        A[5]
    catch e
        println(typeof(e))
    end

---

## 11. `KeyError`

A `KeyError` occurs when you try to access a dictionary using a key that does not exist.

Example:

    d = Dict(
        "Alice" => 90,
        "Bob" => 85
    )

    d["Charlie"]

This produces a `KeyError`.

You can catch it:

    try
        println(d["Charlie"])
    catch e
        println(typeof(e))
    end

---

## 12. Checking Exception Types

You can check the type of an exception using `isa`.

Example:

    try
        A = [1, 2, 3]
        A[10]
    catch e
        if e isa BoundsError
            println("Invalid array index")
        end
    end

You can handle different exception types separately:

    try
        # code that may fail
    catch e
        if e isa BoundsError
            println("Index problem")
        elseif e isa KeyError
            println("Dictionary key problem")
        elseif e isa ArgumentError
            println("Invalid argument")
        else
            println("Some other error occurred")
        end
    end

This is useful when different errors require different responses.

---

## 13. `tryparse`

`tryparse` attempts to parse a value without throwing an exception when parsing fails.

For example:

    tryparse(Int, "123")

returns:

    123

But:

    tryparse(Int, "hello")

returns:

    nothing

This is useful when invalid input is an expected possibility.

Compare:

    parse(Int, "hello")

which throws an exception.

With:

    tryparse(Int, "hello")

which returns `nothing`.

---

## 14. `finally`

A `finally` block contains code that should execute whether or not an exception occurs.

General structure:

    try
        # code
    catch
        # error handling
    finally
        # cleanup
    end

Example:

    try
        println("Doing some work")
    catch e
        println("An error occurred")
    finally
        println("Cleanup")
    end

`finally` is useful for cleanup operations such as closing resources or restoring program state.

---

## 15. `rethrow`

Sometimes you catch an exception only to perform some additional action and then want the exception to continue propagating.

Use `rethrow`.

Example:

    try
        error("Something failed")
    catch e
        println("Logging the error")
        rethrow()
    end

The exception is caught temporarily and then thrown again.

This is useful when you want to log or inspect an error without silently hiding it.

---

## 16. Custom Exception Types

You can define your own exception type using a `struct` that is a subtype of `Exception`.

Example:

    struct InvalidMeasurement <: Exception
        message::String
    end

You can throw it:

    throw(InvalidMeasurement("Measurement is physically invalid"))

You can then catch it specifically:

    try
        throw(InvalidMeasurement("Negative measurement"))
    catch e
        if e isa InvalidMeasurement
            println("Invalid measurement detected")
        end
    end

Custom exception types are useful when a scientific application has domain-specific failure conditions.

---

## 17. `@assert`

`@assert` checks that a condition is true.

Example:

    x = 10

    @assert x > 0

If the condition is true, execution continues.

If it is false, an `AssertionError` is thrown.

Example:

    x = -10

    @assert x > 0

You can provide a message:

    @assert x > 0 "x must be positive"

Assertions are useful for checking assumptions that should hold inside a program.

---

## 18. Input Validation

Functions should validate inputs when invalid values could produce meaningless or incorrect results.

Example:

    function reciprocal(x)
        @assert x != 0 "x must not be zero"
        return 1 / x
    end

Another approach is explicit exception handling:

    function reciprocal(x)
        if x == 0
            throw(ArgumentError("x must not be zero"))
        end

        return 1 / x
    end

Use validation to establish the assumptions under which a function is valid.

---

## 19. Exception Propagation

An exception does not necessarily have to be handled where it occurs.

For example:

    function inner()
        error("Failure in inner")
    end

    function middle()
        inner()
    end

    function outer()
        middle()
    end

If `outer()` is called:

    outer()

the exception propagates through:

    inner()
        ↓
    middle()
        ↓
    outer()

until some code catches it.

If nothing catches it, Julia reports the exception and terminates the current execution path.

---

## 20. `nothing`

`nothing` is a special Julia value representing the absence of a meaningful value.

Example:

    result = nothing

A function can return `nothing` when there is no useful result.

For example:

    function print_message()
        println("Hello")
        return nothing
    end

`tryparse` also commonly uses `nothing` to indicate that parsing was unsuccessful:

    x = tryparse(Int, "hello")

Then:

    x === nothing

returns:

    true

---

## 21. Expected versus Unexpected Errors

An important distinction is between errors that are expected as part of normal input handling and errors that indicate a genuine programming or system failure.

### Expected situation

A user enters:

    "hello"

when an integer is expected.

Using:

    tryparse(Int, "hello")

is appropriate because invalid input is a normal possibility.

### Unexpected situation

Suppose a numerical algorithm unexpectedly accesses:

    A[1000]

when `A` should contain only 100 elements.

A `BoundsError` may indicate a bug in the algorithm rather than normal user input.

Do not blindly catch every exception and continue.

---

## 22. Avoid Catching Everything Blindly

This pattern is usually poor error handling:

    try
        # complex computation
    catch
        println("Something went wrong")
    end

It hides the actual cause of the failure.

A better approach is to catch errors you know how to handle:

    try
        value = tryparse(Int, input)

        if value === nothing
            println("Please enter an integer")
        end
    catch e
        rethrow()
    end

For unexpected errors, allowing the exception to propagate can be preferable.

---

## 23. Scientific Validation

Scientific programs require careful validation because mathematically valid-looking inputs can still be physically or numerically invalid.

For example, suppose a function requires a probability.

    function probability_check(p)
        @assert 0 <= p <= 1 "Probability must be between 0 and 1"
        return p
    end

Then:

    probability_check(0.5)

is valid.

But:

    probability_check(1.5)

fails validation.

---

## 24. Validating Numerical Parameters

Suppose a simulation requires a positive time step.

    function simulate(dt, T)
        if dt <= 0
            throw(ArgumentError("dt must be positive"))
        end

        if T <= 0
            throw(ArgumentError("T must be positive"))
        end

        # simulation code
    end

This prevents invalid parameters from entering the numerical computation.

---

## 25. Domain Errors in Scientific Computing

Some mathematical operations are only defined for certain inputs.

For example:

    sqrt(-1.0)

does not produce a real-valued result.

Similarly, taking the logarithm of a non-positive real number is outside the real domain.

For scientific code, it is often useful to validate the mathematical domain explicitly.

Example:

    function real_log(x)
        if x <= 0
            throw(DomainError(x, "x must be positive"))
        end

        return log(x)
    end

Now:

    real_log(2.0)

is valid, while:

    real_log(-2.0)

produces a `DomainError`.

---

## 26. Checking Dimensions

Scientific computing often involves arrays and matrices.

Suppose two vectors must have the same length:

    function dot_product_checked(a, b)
        if length(a) != length(b)
            throw(DimensionMismatch("Vectors must have the same length"))
        end

        return sum(a .* b)
    end

This gives a more informative failure than allowing an unrelated error to occur later.

---

## 27. Combining Validation with Exceptions

A scientific function can validate its assumptions at the beginning:

    function normalize_probability(p)
        if any(x < 0 for x in p)
            throw(ArgumentError("Probabilities cannot be negative"))
        end

        total = sum(p)

        if total == 0
            throw(DomainError(total, "Probability sum cannot be zero"))
        end

        return p ./ total
    end

This separates input validation from the main numerical operation.

---

## 28. Practical Error-Handling Pattern

A useful pattern is:

    function process_input(input)
        value = tryparse(Float64, input)

        if value === nothing
            return nothing
        end

        if !isfinite(value)
            throw(ArgumentError("Input must be finite"))
        end

        return value
    end

Here:

1. Parsing failure is expected.
2. `tryparse` handles the expected failure.
3. `nothing` represents unsuccessful parsing.
4. A non-finite value is treated as invalid input.
5. An `ArgumentError` communicates the invalid argument.

---

## 29. `try` and `finally` for Resource Cleanup

When code acquires a resource, cleanup should happen even if an exception occurs.

General structure:

    try
        # use resource
    finally
        # release resource
    end

The key principle is:

> Cleanup belongs in `finally` when it must happen regardless of success or failure.

---

## 30. Exception Handling Strategy

A useful strategy is:

    Expected invalid input
        ↓
    validate / tryparse
        ↓
    return a meaningful result or `nothing`

    Unexpected failure
        ↓
    allow exception to propagate
        ↓
    inspect/debug the actual problem

    Recoverable known exception
        ↓
    try
        ↓
    catch specific exception type
        ↓
    handle it

    Exception that must continue upward
        ↓
    catch
        ↓
    log/inspect
        ↓
    rethrow

---

# 31. Core Mental Model

Keep these concepts separate:

    error(...)
        ↓
    immediately creates and throws an error

    throw(exception)
        ↓
    explicitly throws a particular exception object

    try
        ...
    catch
        ...
    end
        ↓
    handles exceptions

    finally
        ↓
    performs cleanup regardless of failure

    rethrow()
        ↓
    lets a caught exception continue propagating

    tryparse(...)
        ↓
    handles expected parsing failure without throwing

    nothing
        ↓
    represents absence of a meaningful result

    @assert
        ↓
    checks that an expected condition is true

---

# 32. Key Takeaways

1. Exceptions represent errors or exceptional situations.

2. `error("message")` stops execution by throwing an error.

3. `throw(exception)` explicitly throws an exception object.

4. `ArgumentError` is appropriate for invalid function arguments.

5. `try` and `catch` provide exception handling.

6. The caught exception can be stored in a variable:

       catch e

7. Exception objects can be inspected with `typeof` and `isa`.

8. `showerror` displays an exception's error representation.

9. `BoundsError` commonly indicates invalid collection indexing.

10. `KeyError` indicates that a dictionary key was not found.

11. Different exception types can be handled differently.

12. `tryparse` is useful when parsing failure is an expected possibility.

13. `finally` is used for cleanup that must happen regardless of success or failure.

14. `rethrow()` allows a caught exception to propagate upward.

15. Custom exception types can represent domain-specific failures.

16. `@assert` checks assumptions that should hold during execution.

17. Input validation prevents invalid data from entering numerical computations.

18. Exceptions can propagate through multiple function calls.

19. `nothing` can represent the absence of a meaningful result.

20. Expected invalid input and unexpected programming errors should not be treated the same way.

21. Scientific computing requires validation of domains, dimensions, ranges, finiteness, and physical constraints.

22. Good exception handling should make failures informative without silently hiding genuine bugs.
