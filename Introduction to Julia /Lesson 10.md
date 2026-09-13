# Lesson 10 — Strings and String Manipulation

Introduction to text processing in Julia.

## Topics

- Strings
- Characters
- String indexing
- String slicing
- `length`
- `first`
- `last`
- String concatenation
- String interpolation
- `string`
- `parse`
- `lowercase`
- `uppercase`
- `strip`
- `replace`
- `occursin`
- `startswith`
- `endswith`
- `findfirst`
- `split`
- `join`
- Iterating through characters
- `collect`
- `enumerate`
- Multiline strings
- Raw strings
- String comparison
- Parsing numerical data from strings

---

## 1. Strings

Strings represent sequences of text in Julia.

```python
%%bash
julia <<'JULIA'
name = "Julia"
message = "Scientific Computing"

println(name)
println(message)
println(typeof(name))
JULIA
```

Strings are created using double quotation marks.

```python
%%bash
julia <<'JULIA'
text = "Hello, World!"

println(text)
JULIA
```

Strings can contain spaces, numbers, and special characters.

```python
%%bash
julia <<'JULIA'
text = "Julia 1.12 is fast!"

println(text)
JULIA
```

---

## 2. Characters

A character represents a single character.

Characters are created using single quotation marks.

```python
%%bash
julia <<'JULIA'
c = 'J'

println(c)
println(typeof(c))
JULIA
```

Characters can represent letters, numbers, and symbols.

```python
%%bash
julia <<'JULIA'
a = '7'
b = '+'
c = 'x'

println(a)
println(b)
println(c)
JULIA
```

A character and a one-character string are different types.

```python
%%bash
julia <<'JULIA'
c = 'A'
s = "A"

println(typeof(c))
println(typeof(s))
JULIA
```

---

## 3. String Indexing

Julia uses 1-based indexing for strings.

```python
%%bash
julia <<'JULIA'
text = "Julia"

println(text[1])
println(text[2])
println(text[5])
JULIA
```

The first character can be accessed using `begin`.

```python
%%bash
julia <<'JULIA'
text = "Julia"

println(text[begin])
JULIA
```

The last character can be accessed using `end`.

```python
%%bash
julia <<'JULIA'
text = "Julia"

println(text[end])
JULIA
```

---

## 4. String Slicing

A range can be used to access part of a string.

```python
%%bash
julia <<'JULIA'
text = "Julia"

println(text[1:3])
println(text[2:5])
JULIA
```

`begin` and `end` can also be used with ranges.

```python
%%bash
julia <<'JULIA'
text = "Scientific"

println(text[begin:5])
println(text[4:end])
JULIA
```

---

## 5. `length`

`length` returns the number of characters in a string.

```python
%%bash
julia <<'JULIA'
text = "Julia"

println(length(text))
JULIA
```

```python
%%bash
julia <<'JULIA'
message = "Scientific Computing"

println(length(message))
JULIA
```

---

## 6. `first`

`first` returns the first character of a string.

```python
%%bash
julia <<'JULIA'
text = "Julia"

println(first(text))
JULIA
```

---

## 7. `last`

`last` returns the last character of a string.

```python
%%bash
julia <<'JULIA'
text = "Julia"

println(last(text))
JULIA
```

---

## 8. String Concatenation

Strings can be combined using `*`.

```python
%%bash
julia <<'JULIA'
first_name = "Arjyadeb"
last_name = "Sengupta"

full_name = first_name * " " * last_name

println(full_name)
JULIA
```

Multiple strings can be concatenated.

```python
%%bash
julia <<'JULIA'
a = "Physics"
b = "Informed"
c = "Machine"
d = "Learning"

result = a * " " * b * " " * c * " " * d

println(result)
JULIA
```

---

## 9. String Interpolation

Variables can be inserted directly into strings using `$`.

```python
%%bash
julia <<'JULIA'
name = "Julia"

println("Hello, $name")
JULIA
```

Expressions can be inserted using `$()`.

```python
%%bash
julia <<'JULIA'
x = 10
y = 20

println("The sum is $(x + y)")
JULIA
```

Multiple variables can be interpolated.

```python
%%bash
julia <<'JULIA'
name = "Julia"
version = 1.12

println("$name version $version")
JULIA
```

---

## 10. `string`

The `string` function converts values into strings.

```python
%%bash
julia <<'JULIA'
x = 42

text = string(x)

println(text)
println(typeof(text))
JULIA
```

Multiple values can be combined using `string`.

```python
%%bash
julia <<'JULIA'
name = "Julia"
version = 1.12

result = string(name, " version ", version)

println(result)
JULIA
```

---

## 11. `parse`

`parse` converts a string into a specified type.

```python
%%bash
julia <<'JULIA'
x = parse(Int, "42")

println(x)
println(typeof(x))
JULIA
```

Floating-point numbers can also be parsed.

```python
%%bash
julia <<'JULIA'
x = parse(Float64, "3.14159")

println(x)
println(typeof(x))
JULIA
```

---

## 12. `lowercase`

`lowercase` converts letters to lowercase.

```python
%%bash
julia <<'JULIA'
text = "Julia Programming"

println(lowercase(text))
JULIA
```

---

## 13. `uppercase`

`uppercase` converts letters to uppercase.

```python
%%bash
julia <<'JULIA'
text = "Julia Programming"

println(uppercase(text))
JULIA
```

---

## 14. `strip`

`strip` removes whitespace from the beginning and end of a string.

```python
%%bash
julia <<'JULIA'
text = "   Julia   "

println(text)
println(strip(text))
JULIA
```

This is useful when cleaning text data.

```python
%%bash
julia <<'JULIA'
data = "   42   "

clean_data = strip(data)

println(clean_data)
JULIA
```

---

## 15. `replace`

`replace` replaces part of a string.

```python
%%bash
julia <<'JULIA'
text = "I like Python"

result = replace(text, "Python" => "Julia")

println(result)
JULIA
```

Another example:

```python
%%bash
julia <<'JULIA'
text = "Physics and Physics"

result = replace(text, "Physics" => "Mathematics")

println(result)
JULIA
```

---

## 16. `occursin`

`occursin` checks whether a substring occurs inside a string.

```python
%%bash
julia <<'JULIA'
text = "Julia is fast"

println(occursin("Julia", text))
println(occursin("Python", text))
JULIA
```

The result is a Boolean value.

```python
%%bash
julia <<'JULIA'
text = "Scientific Machine Learning"

if occursin("Machine", text)
    println("Machine was found")
end
JULIA
```

---

## 17. `startswith`

`startswith` checks whether a string begins with a specified sequence.

```python
%%bash
julia <<'JULIA'
text = "Julia Programming"

println(startswith(text, "Julia"))
println(startswith(text, "Python"))
JULIA
```

---

## 18. `endswith`

`endswith` checks whether a string ends with a specified sequence.

```python
%%bash
julia <<'JULIA'
filename = "simulation.jl"

println(endswith(filename, ".jl"))
println(endswith(filename, ".py"))
JULIA
```

---

## 19. `findfirst`

`findfirst` finds the first occurrence of a character or substring.

```python
%%bash
julia <<'JULIA'
text = "Julia Programming"

println(findfirst("a", text))
JULIA
```

It can also search for a substring.

```python
%%bash
julia <<'JULIA'
text = "Scientific Machine Learning"

println(findfirst("Machine", text))
JULIA
```

---

## 20. `split`

`split` divides a string into smaller pieces.

```python
%%bash
julia <<'JULIA'
text = "Julia is fast"

words = split(text)

println(words)
JULIA
```

A delimiter can be specified.

```python
%%bash
julia <<'JULIA'
text = "Julia,Python,Fortran"

languages = split(text, ",")

println(languages)
JULIA
```

The resulting values are strings.

```python
%%bash
julia <<'JULIA'
data = "10 20 30 40 50"

values = split(data)

println(values)
println(typeof(values[1]))
JULIA
```

---

## 21. `join`

`join` combines elements into a string.

```python
%%bash
julia <<'JULIA'
words = ["Julia", "is", "fast"]

sentence = join(words, " ")

println(sentence)
JULIA
```

A delimiter can be specified.

```python
%%bash
julia <<'JULIA'
values = ["10", "20", "30", "40"]

result = join(values, ",")

println(result)
JULIA
```

---

## 22. Iterating Through Characters

A string can be iterated over directly.

```python
%%bash
julia <<'JULIA'
text = "Julia"

for character in text
    println(character)
end
JULIA
```

Each iteration produces one character.

```python
%%bash
julia <<'JULIA'
text = "Physics"

for character in text
    println(character)
end
JULIA
```

---

## 23. `collect`

`collect` converts a string into a collection of characters.

```python
%%bash
julia <<'JULIA'
text = "Julia"

characters = collect(text)

println(characters)
JULIA
```

The resulting collection can be indexed.

```python
%%bash
julia <<'JULIA'
text = "Julia"

characters = collect(text)

println(characters[1])
println(characters[3])
JULIA
```

---

## 24. `enumerate`

`enumerate` provides both the index and the corresponding character.

```python
%%bash
julia <<'JULIA'
text = "Julia"

for (index, character) in enumerate(text)
    println(index, " => ", character)
end
JULIA
```

This is useful when both the position and character are required.

```python
%%bash
julia <<'JULIA'
text = "Physics"

for (index, character) in enumerate(text)
    println("Position ", index, ": ", character)
end
JULIA
```

---

## 25. Multiline Strings

Triple quotation marks create multiline strings.

```python
%%bash
julia <<'JULIA'
text = """
Julia is a programming language
for technical computing
and scientific computing.
"""

println(text)
JULIA
```

Multiline strings are useful for larger blocks of text.

```python
%%bash
julia <<'JULIA'
description = """
Physics-Informed Machine Learning
combines physical models
with machine learning.
"""

println(description)
JULIA
```

---

## 26. Raw Strings

Raw strings treat backslashes more literally.

Use the `raw` string macro.

```python
%%bash
julia <<'JULIA'
path = raw"C:\Users\Julia\data"

println(path)
JULIA
```

Raw strings are useful when working with paths or text containing backslashes.

```python
%%bash
julia <<'JULIA'
text = raw"\alpha + \beta = \gamma"

println(text)
JULIA
```

---

## 27. String Comparison

Strings can be compared using comparison operators.

```python
%%bash
julia <<'JULIA'
a = "Julia"
b = "Julia"

println(a == b)
JULIA
```

Different strings are not equal.

```python
%%bash
julia <<'JULIA'
a = "Julia"
b = "Python"

println(a == b)
JULIA
```

Strings can also be ordered lexicographically.

```python
%%bash
julia <<'JULIA'
a = "Apple"
b = "Banana"

println(a < b)
println(a > b)
JULIA
```

---

## 28. Parsing Numerical Data from Strings

Numerical data frequently arrives as text.

```python
%%bash
julia <<'JULIA'
x = "25"
y = "3.14"

x_number = parse(Int, x)
y_number = parse(Float64, y)

println(x_number)
println(y_number)
JULIA
```

The parsed values can then be used in numerical calculations.

```python
%%bash
julia <<'JULIA'
x = "10"
y = "20"

x_number = parse(Int, x)
y_number = parse(Int, y)

result = x_number + y_number

println(result)
JULIA
```

Multiple numerical values can be parsed after splitting a string.

```python
%%bash
julia <<'JULIA'
data = "10,20,30,40,50"

strings = split(data, ",")

numbers = parse.(Int, strings)

println(numbers)
println(sum(numbers))
JULIA
```

Floating-point data can be handled in the same way.

```python
%%bash
julia <<'JULIA'
data = "1.5,2.5,3.5,4.5"

strings = split(data, ",")

numbers = parse.(Float64, strings)

println(numbers)
println(sum(numbers))
println(length(numbers))
JULIA
```

---

## 29. Practical Text Processing Pattern

A common text-processing workflow is:

```python
%%bash
julia <<'JULIA'
text = "   Julia, Python, Julia, Python   "

text = strip(text)
text = lowercase(text)
items = split(text, ",")

println(items)
JULIA
```

Individual elements can then be cleaned.

```python
%%bash
julia <<'JULIA'
text = "   Julia, Python, Julia, Python   "

text = strip(text)
text = lowercase(text)

items = split(text, ",")

for item in items
    println(strip(item))
end
JULIA
```

---

## 30. Strings in Scientific Computing

Strings are commonly used for:

- filenames
- dataset labels
- experiment names
- parameter descriptions
- simulation metadata
- units
- configuration values
- logging

Example:

```python
%%bash
julia <<'JULIA'
experiment = "transmon_simulation"
temperature = "0.02"
shots = "10000"

temperature_value = parse(Float64, temperature)
shots_value = parse(Int, shots)

println("Experiment: $experiment")
println("Temperature: $temperature_value K")
println("Shots: $shots_value")
JULIA
```

---

## 31. Complete Example

```python
%%bash
julia <<'JULIA'
data = "  Julia, Python, Julia, Fortran  "

data = strip(data)
languages = split(data, ",")

clean_languages = String[]

for language in languages
    language = strip(language)
    language = lowercase(language)
    push!(clean_languages, language)
end

println(clean_languages)

for (index, language) in enumerate(clean_languages)
    println(index, " => ", language)
end
JULIA
```

---

## 32. Summary

The main string operations introduced in this lesson are:

```julia
length(text)
first(text)
last(text)

text[1]
text[1:3]

a * b
"$variable"
string(value)

parse(Int, text)
parse(Float64, text)

lowercase(text)
uppercase(text)
strip(text)
replace(text, "old" => "new")

occursin("text", text)
startswith(text, "prefix")
endswith(text, "suffix")
findfirst("text", text)

split(text)
join(values, delimiter)

collect(text)
enumerate(text)
```

The central workflow is:

```text
TEXT
  ↓
INSPECT
  ↓
MODIFY
  ↓
SPLIT / JOIN
  ↓
PARSE
  ↓
NUMERICAL DATA
```

---
