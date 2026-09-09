# Lesson 5 — Collections

## Introduction

Collections are fundamental data structures used to store and organize multiple values.

Julia provides several important collection types for different purposes:

* **Tuples** — ordered, immutable collections
* **Named tuples** — tuples whose elements have names
* **Dictionaries** — collections of key-value pairs
* **Sets** — unordered collections of unique elements

These structures are heavily used in scientific computing, data processing, machine learning, and numerical programming.

---

# 1. Tuples

A tuple is an ordered collection of values.

Unlike arrays, tuples are immutable.

## Creating tuples

A tuple can be created by separating values with commas.

```python
%%bash
julia <<'JULIA'
x = (10, 20, 30)

println(x)
println(typeof(x))
JULIA
```

A tuple can contain values of different types.

```python
%%bash
julia <<'JULIA'
x = (10, 3.14, true, "Julia")

println(x)
println(typeof(x))
JULIA
```

Tuples can also contain expressions.

```python
%%bash
julia <<'JULIA'
x = (2 + 3, 4 * 5, 10 / 2)

println(x)
JULIA
```

---

# 2. Tuple Indexing

Tuples use one-based indexing, just like arrays.

```python
%%bash
julia <<'JULIA'
x = (10, 20, 30, 40, 50)

println(x[1])
println(x[3])
println(x[end])
JULIA
```

Tuple indexing returns the corresponding element.

```python
%%bash
julia <<'JULIA'
x = ("Julia", "Python", "C++")

println(x[1])
println(x[2])
println(x[3])
JULIA
```

Because tuples are immutable, their elements cannot be changed.

---

# 3. Tuple Unpacking

Tuple unpacking allows multiple variables to receive values from a tuple.

```python
%%bash
julia <<'JULIA'
x = (10, 20, 30)

a, b, c = x

println(a)
println(b)
println(c)
JULIA
```

This is useful when a function returns multiple values.

```python
%%bash
julia <<'JULIA'
function coordinates()
    return 10, 20
end

x, y = coordinates()

println(x)
println(y)
JULIA
```

---

# 4. Tuple Iteration

Tuples can be iterated over using a `for` loop.

```python
%%bash
julia <<'JULIA'
x = (10, 20, 30, 40)

for value in x
    println(value)
end
JULIA
```

The tuple can also be processed together with its index.

```python
%%bash
julia <<'JULIA'
x = (10, 20, 30)

for (i, value) in enumerate(x)
    println("Index: ", i, ", Value: ", value)
end
JULIA
```

---

# 5. Named Tuples

A named tuple is a tuple whose elements have names.

## Creating named tuples

```python
%%bash
julia <<'JULIA'
person = (name = "Alice", age = 25, height = 1.70)

println(person)
println(typeof(person))
JULIA
```

The names make the data easier to understand.

---

# 6. Accessing Named Tuple Values

Named tuple elements can be accessed using their names.

```python
%%bash
julia <<'JULIA'
person = (name = "Alice", age = 25, height = 1.70)

println(person.name)
println(person.age)
println(person.height)
JULIA
```

They can also be accessed using indexing.

```python
%%bash
julia <<'JULIA'
person = (name = "Alice", age = 25, height = 1.70)

println(person[1])
println(person[2])
println(person[3])
JULIA
```

Named tuples are useful when a small fixed collection of related values should have meaningful names.

---

# 7. Dictionaries

A dictionary stores data as **key-value pairs**.

The general structure is:

```text
key => value
```

Dictionaries are useful when values need to be associated with identifiers rather than numerical positions.

---

# 8. Creating Dictionaries

A dictionary can be created using `Dict`.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22,
    "department" => "Mathematics"
)

println(student)
println(typeof(student))
JULIA
```

The keys and values can have different types.

```python
%%bash
julia <<'JULIA'
data = Dict(
    "temperature" => 25.5,
    "pressure" => 101.3,
    "valid" => true
)

println(data)
JULIA
```

---

# 9. Keys and Values

A dictionary consists of keys and their corresponding values.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22,
    "department" => "Mathematics"
)

println(keys(student))
println(values(student))
JULIA
```

`keys` returns the dictionary's keys.

`values` returns the dictionary's values.

---

# 10. Accessing Values

Values can be accessed using their keys.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22
)

println(student["name"])
println(student["age"])
JULIA
```

The key is used instead of a numerical index.

---

# 11. Adding Entries

A new key-value pair can be added using indexing.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22
)

student["department"] = "Mathematics"

println(student)
JULIA
```

Another entry can be added in the same way.

```python
%%bash
julia <<'JULIA'
data = Dict("x" => 10)

data["y"] = 20
data["z"] = 30

println(data)
JULIA
```

---

# 12. Updating Entries

Assigning a new value to an existing key updates that entry.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22
)

student["age"] = 23

println(student)
JULIA
```

The key remains the same while its associated value changes.

---

# 13. Deleting Entries

The `delete!` function removes a key-value pair.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22,
    "department" => "Mathematics"
)

delete!(student, "age")

println(student)
JULIA
```

The specified key and its value are removed from the dictionary.

---

# 14. `haskey`

The `haskey` function checks whether a dictionary contains a particular key.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22
)

println(haskey(student, "name"))
println(haskey(student, "department"))
JULIA
```

The result is a Boolean:

```text
true
false
```

This is useful before attempting to access a value.

---

# 15. `get`

The `get` function allows a default value to be returned when a key is not present.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22
)

println(get(student, "name", "Unknown"))
println(get(student, "department", "Unknown"))
JULIA
```

Here:

```text
"name"
```

exists, so its value is returned.

The `"department"` key does not exist, so `"Unknown"` is returned.

---

# 16. Iterating Through Dictionaries

Dictionaries can be iterated over using key-value pairs.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22,
    "department" => "Mathematics"
)

for (key, value) in student
    println(key, " => ", value)
end
JULIA
```

The iteration order of a dictionary should not be relied upon.

---

# 17. Iterating Through Keys

The keys can be iterated over separately.

```python
%%bash
julia <<'JULIA'
student = Dict(
    "name" => "Alice",
    "age" => 22,
    "department" => "Mathematics"
)

for key in keys(student)
    println(key)
end
JULIA
```

---

# 18. Iterating Through Values

Values can also be iterated over separately.

```python
%%bash
julia <<'JULIA'
scores = Dict(
    "Alice" => 90,
    "Bob" => 85,
    "Charlie" => 95
)

for value in values(scores)
    println(value)
end
JULIA
```

---

# 19. Sets

A set is a collection of unique elements.

Unlike arrays and tuples, sets are not ordered collections.

Sets are useful when:

* duplicate values should be removed
* membership needs to be tested
* mathematical set operations are required

---

# 20. Creating Sets

A set can be created using `Set`.

```python
%%bash
julia <<'JULIA'
x = Set([1, 2, 3, 4])

println(x)
println(typeof(x))
JULIA
```

Duplicate values are automatically removed.

```python
%%bash
julia <<'JULIA'
x = Set([1, 2, 2, 3, 3, 3])

println(x)
JULIA
```

The resulting set contains only unique elements.

---

# 21. Adding Elements

The `push!` function adds an element to a set.

```python
%%bash
julia <<'JULIA'
x = Set([1, 2, 3])

push!(x, 4)

println(x)
JULIA
```

Adding an element that already exists does not create a duplicate.

```python
%%bash
julia <<'JULIA'
x = Set([1, 2, 3])

push!(x, 2)

println(x)
JULIA
```

---

# 22. Removing Elements

The `pop!` function can remove an element from a set.

```python
%%bash
julia <<'JULIA'
x = Set([1, 2, 3, 4])

pop!(x, 3)

println(x)
JULIA
```

The specified element is removed if it exists.

Another useful function is `delete!`.

```python
%%bash
julia <<'JULIA'
x = Set([1, 2, 3, 4])

delete!(x, 2)

println(x)
JULIA
```

---

# 23. Membership Testing

The `in` operator can be used to test whether an element belongs to a set.

```python
%%bash
julia <<'JULIA'
x = Set([1, 2, 3, 4])

println(2 in x)
println(10 in x)
JULIA
```

The result is a Boolean.

The `∈` symbol can also be used.

```python
%%bash
julia <<'JULIA'
x = Set([1, 2, 3, 4])

println(2 ∈ x)
JULIA
```

---

# 24. Set Union

The union of two sets contains all unique elements appearing in either set.

```python
%%bash
julia <<'JULIA'
A = Set([1, 2, 3])
B = Set([3, 4, 5])

C = union(A, B)

println(C)
JULIA
```

The result contains:

```text
1, 2, 3, 4, 5
```

---

# 25. Set Intersection

The intersection contains elements common to both sets.

```python
%%bash
julia <<'JULIA'
A = Set([1, 2, 3])
B = Set([3, 4, 5])

C = intersect(A, B)

println(C)
JULIA
```

The only common element is:

```text
3
```

---

# 26. Set Difference

The difference between two sets contains elements present in the first set but not in the second.

```python
%%bash
julia <<'JULIA'
A = Set([1, 2, 3])
B = Set([3, 4, 5])

C = setdiff(A, B)

println(C)
JULIA
```

Here, the result contains:

```text
1, 2
```

because those elements are in `A` but not in `B`.

---

# 27. Set Operations

Several mathematical set operations can be combined.

```python
%%bash
julia <<'JULIA'
A = Set([1, 2, 3, 4])
B = Set([3, 4, 5, 6])

println("Union: ", union(A, B))
println("Intersection: ", intersect(A, B))
println("A - B: ", setdiff(A, B))
println("B - A: ", setdiff(B, A))
JULIA
```

These operations are useful for comparing collections of values.

---

# 28. Comparing the Main Collection Types

The four collection types introduced in this lesson serve different purposes.

| Collection  | Ordered | Mutable | Main purpose                       |
| ----------- | ------- | ------- | ---------------------------------- |
| Tuple       | Yes     | No      | Fixed collection of values         |
| Named Tuple | Yes     | No      | Fixed collection with named fields |
| Dictionary  | No      | Yes     | Key-value relationships            |
| Set         | No      | Yes     | Unique elements and set operations |

A useful way to think about them is:

```text
Tuple
    ↓
ordered + immutable

Named Tuple
    ↓
ordered + named + immutable

Dictionary
    ↓
key → value

Set
    ↓
unique elements
```

---

# 29. Practical Example — Student Data

Different collection types can be used together.

```python
%%bash
julia <<'JULIA'
student = (
    name = "Alice",
    age = 22,
    courses = Set(["Mathematics", "Physics", "Machine Learning"])
)

println("Name: ", student.name)
println("Age: ", student.age)
println("Courses: ", student.courses)

println("Machine Learning enrolled: ",
        "Machine Learning" in student.courses)
JULIA
```

This demonstrates how named tuples and sets can work together.

---

# 30. Practical Example — Scientific Data

Dictionaries can associate quantities with their values.

```python
%%bash
julia <<'JULIA'
measurements = Dict(
    "temperature" => 298.15,
    "pressure" => 101325.0,
    "density" => 1.225
)

for (quantity, value) in measurements
    println(quantity, " = ", value)
end
JULIA
```

This type of structure is useful when numerical values need descriptive labels.

---

# 31. Practical Example — Removing Duplicate Data

Sets are useful for identifying unique values.

```python
%%bash
julia <<'JULIA'
data = [1, 2, 2, 3, 4, 4, 5, 5, 5]

unique_data = Set(data)

println("Original data: ", data)
println("Unique values: ", unique_data)
JULIA
```

---

# 32. Practical Example — Comparing Two Datasets

Sets can be used to compare collections of values.

```python
%%bash
julia <<'JULIA'
dataset_A = Set([1, 2, 3, 4, 5])
dataset_B = Set([4, 5, 6, 7, 8])

println("Common values: ", intersect(dataset_A, dataset_B))
println("Only in A: ", setdiff(dataset_A, dataset_B))
println("Only in B: ", setdiff(dataset_B, dataset_A))
println("All values: ", union(dataset_A, dataset_B))
JULIA
```

This is useful when comparing identifiers, categories, or other discrete data.

---

# 33. Summary

In this lesson, we introduced Julia's fundamental collection types.

## Tuples

```julia
x = (10, 20, 30)
x[1]
a, b, c = x
```

Tuples are ordered and immutable.

## Named tuples

```julia
person = (name = "Alice", age = 25)

person.name
person.age
```

Named tuples give meaningful names to tuple elements.

## Dictionaries

```julia
data = Dict("x" => 10, "y" => 20)

data["x"]
data["z"] = 30

haskey(data, "x")
get(data, "z", 0)

delete!(data, "x")
```

Dictionaries store key-value relationships.

## Sets

```julia
A = Set([1, 2, 3])
B = Set([3, 4, 5])

push!(A, 6)
delete!(A, 2)

2 in A

union(A, B)
intersect(A, B)
setdiff(A, B)
```

Sets store unique elements and support mathematical set operations.

---

# Key Ideas to Remember

```text
Tuple
    → ordered
    → immutable

Named Tuple
    → ordered
    → immutable
    → named elements

Dictionary
    → key-value pairs
    → lookup by key

Set
    → unique elements
    → membership testing
    → union
    → intersection
    → difference
```

These collection types form an important foundation for the data structures and higher-level operations introduced in the following lessons.
