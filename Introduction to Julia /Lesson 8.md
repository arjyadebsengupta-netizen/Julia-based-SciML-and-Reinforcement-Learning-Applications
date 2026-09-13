# Lesson 8 — Structs, Mutable Structs and Composite Types

## Introduction

Julia allows you to create your own types for representing structured data.

The main tool for this is `struct`.

Structs are particularly useful in scientific computing because they allow physical systems, simulation states, model parameters, and other related quantities to be represented as a single object.

## Topics

* `struct`
* Fields
* Field access
* `fieldnames`
* Immutable structures
* `mutable struct`
* Constructors
* Inner constructors
* Outer constructors
* Methods for user-defined types
* Multiple dispatch with custom types
* Parametric structs
* Structs containing arrays
* Scientific state representation
* Modeling physical systems with structs

---

## 1. `struct`

A `struct` defines a new composite type.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    position
    velocity
end

p = Particle(2.0, 3.0, 5.0)

println(p)
println(typeof(p))
JULIA
```

A `struct` groups several related values into one object.

Here:

```julia
struct Particle
    mass
    position
    velocity
end
```

defines a new type called `Particle`.

An object of this type can then be created with:

```julia
Particle(2.0, 3.0, 5.0)
```

---

## 2. Fields

The values stored inside a struct are called **fields**.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    position
    velocity
end

p = Particle(2.0, 3.0, 5.0)

println(p.mass)
println(p.position)
println(p.velocity)
JULIA
```

The fields are:

```text
mass
position
velocity
```

Each object stores a value for every field.

---

## 3. Field Access

Fields are accessed using dot notation.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    position
    velocity
end

p = Particle(2.0, 3.0, 5.0)

println(p.mass)
println(p.position)
println(p.velocity)
JULIA
```

The general syntax is:

```julia
object.field
```

For example:

```julia
p.mass
```

accesses the `mass` field of `p`.

---

## 4. `fieldnames`

Julia provides `fieldnames` for inspecting the fields of a type.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    position
    velocity
end

println(fieldnames(Particle))
JULIA
```

The result is a tuple containing the field names.

You can also inspect the number of fields:

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    position
    velocity
end

println(length(fieldnames(Particle)))
JULIA
```

---

## 5. Immutable Structures

Structures created with `struct` are immutable.

This means that after an object is created, its fields cannot be changed.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    position
end

p = Particle(2.0, 3.0)

println(p)

p.position = 5.0
JULIA
```

The final assignment produces an error because `p.position` cannot be modified.

---

## 6. Why Immutability Matters

Immutable objects are useful when the values defining an object should not change after construction.

For example, physical constants or fixed model parameters can naturally be represented using immutable structs.

```python
%%bash
julia <<'JULIA'
struct PhysicalConstants
    speed_of_light
    gravitational_constant
end

constants = PhysicalConstants(299792458.0, 6.67430e-11)

println(constants.speed_of_light)
println(constants.gravitational_constant)
JULIA
```

---

## 7. `mutable struct`

Julia also provides `mutable struct`.

A mutable struct allows its fields to be changed after construction.

```python
%%bash
julia <<'JULIA'
mutable struct Particle
    mass
    position
    velocity
end

p = Particle(2.0, 3.0, 5.0)

println(p)

p.position = 10.0
p.velocity = 7.0

println(p)
JULIA
```

The fields can now be modified.

---

## 8. `struct` vs `mutable struct`

The basic difference is:

```julia
struct Particle
    mass
    position
end
```

creates an immutable object.

Whereas:

```julia
mutable struct Particle
    mass
    position
end
```

creates a mutable object.

For example:

```python
%%bash
julia <<'JULIA'
struct ImmutableParticle
    position
end

mutable struct MutableParticle
    position
end

a = ImmutableParticle(1.0)
b = MutableParticle(1.0)

println(a)
println(b)

b.position = 5.0

println(b)
JULIA
```

---

## 9. Constructors

A constructor creates an instance of a type.

For a basic struct, Julia automatically provides a constructor.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    position
end

p = Particle(2.0, 4.0)

println(p)
JULIA
```

The automatically generated constructor has the same name as the type:

```julia
Particle(...)
```

---

## 10. Constructors with Explicit Field Types

Fields can have declared types.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass::Float64
    position::Float64
    velocity::Float64
end

p = Particle(2.0, 3.0, 5.0)

println(p)
println(typeof(p.mass))
println(typeof(p.position))
JULIA
```

The field types become part of the structure definition.

---

## 11. Inner Constructors

A struct can define its own constructor inside the struct definition.

This is called an **inner constructor**.

```python
%%bash
julia <<'JULIA'
struct PositiveNumber
    value

    function PositiveNumber(x)
        if x <= 0
            error("Value must be positive")
        end

        new(x)
    end
end

x = PositiveNumber(5)

println(x)
JULIA
```

The important operation inside an inner constructor is:

```julia
new(...)
```

`new` creates the object being constructed.

---

## 12. Inner Constructor Validation

Inner constructors are useful for enforcing conditions on valid objects.

```python
%%bash
julia <<'JULIA'
struct PhysicalParameter
    value

    function PhysicalParameter(x)
        if x < 0
            error("Parameter must be non-negative")
        end

        new(x)
    end
end

p = PhysicalParameter(10.0)

println(p.value)
JULIA
```

An invalid value produces an error.

```julia
PhysicalParameter(-1.0)
```

would fail because the constructor rejects it.

---

## 13. Outer Constructors

An outer constructor is defined outside the struct definition.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    position
end

Particle(mass) = Particle(mass, 0.0)

p = Particle(2.0)

println(p)
JULIA
```

The additional constructor provides a convenient way to create a `Particle` with a default position.

---

## 14. Multiple Constructors

Several constructors can be defined for the same type.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    position
    velocity
end

Particle(mass) = Particle(mass, 0.0, 0.0)
Particle(mass, position) = Particle(mass, position, 0.0)

p1 = Particle(2.0)
p2 = Particle(2.0, 5.0)
p3 = Particle(2.0, 5.0, 3.0)

println(p1)
println(p2)
println(p3)
JULIA
```

The different constructors provide different ways of creating the same type.

---

## 15. Methods for User-Defined Types

Functions can be defined specifically for user-defined types.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    velocity
end

kinetic_energy(p::Particle) = 0.5 * p.mass * p.velocity^2

p = Particle(2.0, 3.0)

println(kinetic_energy(p))
JULIA
```

The function accepts a `Particle` as its argument.

---

## 16. Methods Can Access Fields

A method can use the fields of a struct to perform calculations.

```python
%%bash
julia <<'JULIA'
struct Rectangle
    width
    height
end

area(r::Rectangle) = r.width * r.height
perimeter(r::Rectangle) = 2 * (r.width + r.height)

r = Rectangle(4.0, 3.0)

println(area(r))
println(perimeter(r))
JULIA
```

The struct stores the data.

The methods operate on that data.

---

## 17. Multiple Dispatch with Custom Types

User-defined types participate in Julia's multiple dispatch system.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
end

struct Field
    strength
end

interact(p::Particle, f::Field) = p.mass * f.strength

p = Particle(2.0)
f = Field(5.0)

println(interact(p, f))
JULIA
```

The selected method depends on the types of the arguments.

Here:

```julia
interact(p::Particle, f::Field)
```

is selected because the arguments are:

```text
Particle
Field
```

---

## 18. Multiple Methods with Custom Types

Different types can have different methods.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
end

struct Photon
    energy
end

describe(p::Particle) = "This is a particle."
describe(p::Photon) = "This is a photon."

p = Particle(2.0)
ph = Photon(10.0)

println(describe(p))
println(describe(ph))
JULIA
```

The same function name:

```julia
describe
```

has multiple methods.

Julia chooses the appropriate method based on the argument type.

---

## 19. Parametric Structs

A parametric struct contains one or more type parameters.

```python
%%bash
julia <<'JULIA'
struct Box{T}
    value::T
end

a = Box(10)
b = Box(3.14)
c = Box("Julia")

println(a)
println(b)
println(c)

println(typeof(a))
println(typeof(b))
println(typeof(c))
JULIA
```

The type parameter `T` represents the type stored inside the object.

For example:

```text
Box{Int64}
Box{Float64}
Box{String}
```

---

## 20. Type Parameters

The type parameter can be used to describe the fields of a struct.

```python
%%bash
julia <<'JULIA'
struct PairValue{T}
    first::T
    second::T
end

x = PairValue(1.0, 2.0)

println(typeof(x))
println(x.first)
println(x.second)
JULIA
```

Here both fields use the same type parameter `T`.

---

## 21. Multiple Type Parameters

A struct can have more than one type parameter.

```python
%%bash
julia <<'JULIA'
struct PairValue{T, S}
    first::T
    second::S
end

x = PairValue(10, 3.14)

println(x)
println(typeof(x))
JULIA
```

The two fields can now have different types.

---

## 22. Structs Containing Arrays

Structs can contain arrays.

This is extremely useful in scientific computing.

```python
%%bash
julia <<'JULIA'
struct ParticleState
    position
    velocity
end

position = [1.0, 2.0, 3.0]
velocity = [0.5, 1.0, 1.5]

state = ParticleState(position, velocity)

println(state)
println(state.position)
println(state.velocity)
JULIA
```

The struct provides a single object representing the state.

---

## 23. Typed Arrays Inside Structs

The array field can also be given an explicit type.

```python
%%bash
julia <<'JULIA'
struct ParticleState
    position::Vector{Float64}
    velocity::Vector{Float64}
end

state = ParticleState(
    [1.0, 2.0, 3.0],
    [0.5, 1.0, 1.5]
)

println(state)
println(typeof(state.position))
println(typeof(state.velocity))
JULIA
```

---

## 24. Parametric Structs with Arrays

A parametric struct can make array-based scientific states generic.

```python
%%bash
julia <<'JULIA'
struct State{T}
    position::Vector{T}
    velocity::Vector{T}
end

state1 = State(
    [1.0, 2.0, 3.0],
    [0.5, 1.0, 1.5]
)

state2 = State(
    [1, 2, 3],
    [4, 5, 6]
)

println(typeof(state1))
println(typeof(state2))
JULIA
```

The element type is represented by `T`.

---

## 25. Scientific State Representation

A common scientific-computing pattern is to represent the complete state of a system using a struct.

For example:

```python
%%bash
julia <<'JULIA'
struct State
    position
    velocity
    time
end

state = State(
    [1.0, 2.0],
    [0.5, 1.0],
    0.0
)

println(state)
println(state.position)
println(state.velocity)
println(state.time)
JULIA
```

The state contains:

```text
position
velocity
time
```

Instead of passing these quantities separately, they can be passed together as one object.

---

## 26. Modeling a Physical System

Structs can represent the parameters of a physical system.

```python
%%bash
julia <<'JULIA'
struct Pendulum
    mass
    length
    gravity
end

pendulum = Pendulum(
    1.0,
    2.0,
    9.81
)

println(pendulum)
println(pendulum.mass)
println(pendulum.length)
println(pendulum.gravity)
JULIA
```

The object represents the physical system.

---

## 27. Methods for a Physical System

Methods can then operate on the physical system.

```python
%%bash
julia <<'JULIA'
struct Pendulum
    mass
    length
    gravity
end

angular_frequency(p::Pendulum) =
    sqrt(p.gravity / p.length)

p = Pendulum(1.0, 2.0, 9.81)

println(angular_frequency(p))
JULIA
```

The physical parameters are stored in the struct.

The mathematical operation is implemented as a method.

---

## 28. Physical State and Physical Parameters

It is often useful to distinguish between a physical system's parameters and its current state.

```python
%%bash
julia <<'JULIA'
struct Pendulum
    mass
    length
    gravity
end

struct PendulumState
    angle
    angular_velocity
end

system = Pendulum(1.0, 2.0, 9.81)

state = PendulumState(
    0.5,
    0.0
)

println(system)
println(state)
JULIA
```

Here:

```text
Pendulum
    ↓
physical parameters

PendulumState
    ↓
current dynamical state
```

This separation is useful when building simulations.

---

## 29. Mutable Scientific State

A simulation state may naturally need to change over time.

A `mutable struct` can represent such a state.

```python
%%bash
julia <<'JULIA'
mutable struct ParticleState
    position
    velocity
    time
end

state = ParticleState(
    0.0,
    1.0,
    0.0
)

println(state)

state.position = 1.0
state.time = 1.0

println(state)
JULIA
```

The state can now be updated during a simulation.

---

## 30. Structs and Multiple Dispatch in Scientific Computing

Structs and multiple dispatch work naturally together.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
    velocity
end

struct Force
    value
end

kinetic_energy(p::Particle) =
    0.5 * p.mass * p.velocity^2

acceleration(p::Particle, f::Force) =
    f.value / p.mass

p = Particle(2.0, 3.0)
f = Force(10.0)

println(kinetic_energy(p))
println(acceleration(p, f))
JULIA
```

The physical entities are represented by types.

The operations are represented by methods.

This is one of the important patterns behind scientific Julia code.

---

## 31. Structs as Scientific Data Models

A struct can represent:

* a particle
* a physical system
* simulation parameters
* a numerical state
* model parameters
* experimental data
* solver configuration
* machine-learning model state

For example:

```python
%%bash
julia <<'JULIA'
struct SimulationParameters
    dt
    steps
    tolerance
end

parameters = SimulationParameters(
    0.01,
    1000,
    1e-8
)

println(parameters)
JULIA
```

The struct provides a clear representation of the simulation configuration.

---

## 32. Complete Example — Particle Simulation State

```python
%%bash
julia <<'JULIA'
mutable struct ParticleState
    position
    velocity
    time
end

function advance!(state::ParticleState, dt)
    state.position += state.velocity * dt
    state.time += dt
end

state = ParticleState(
    0.0,
    2.0,
    0.0
)

println("Initial state:")
println(state)

advance!(state, 0.5)

println("Updated state:")
println(state)

advance!(state, 0.5)

println("Updated state:")
println(state)
JULIA
```

This example combines:

* `mutable struct`
* fields
* field access
* methods
* mutation
* scientific state representation

The struct represents the state of the simulated particle.

The method updates that state.

---

## 33. Complete Example — Physical System and State

```python
%%bash
julia <<'JULIA'
struct HarmonicOscillator
    mass
    spring_constant
end

mutable struct OscillatorState
    position
    velocity
    time
end

function acceleration(system::HarmonicOscillator, state::OscillatorState)
    -(system.spring_constant / system.mass) * state.position
end

system = HarmonicOscillator(
    1.0,
    4.0
)

state = OscillatorState(
    1.0,
    0.0,
    0.0
)

println("System:")
println(system)

println("State:")
println(state)

println("Acceleration:")
println(acceleration(system, state))
JULIA
```

This illustrates a useful scientific-computing design:

```text
Physical system
      ↓
HarmonicOscillator

Current state
      ↓
OscillatorState

Physical operation
      ↓
acceleration(...)
```

---

## 34. Structs and the Type System

Structs are concrete types.

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
end

p = Particle(2.0)

println(typeof(p))
println(typeof(p) <: Any)
println(isconcretetype(typeof(p)))
JULIA
```

The user-defined type becomes part of Julia's type system.

This means that it can participate in method selection and multiple dispatch.

---

## 35. Structs and Method Selection

Consider:

```python
%%bash
julia <<'JULIA'
struct Particle
    mass
end

struct Photon
    energy
end

describe(x::Particle) = "Particle"
describe(x::Photon) = "Photon"

p = Particle(2.0)
ph = Photon(10.0)

println(describe(p))
println(describe(ph))
JULIA
```

The function is the same:

```julia
describe
```

but the selected method depends on the argument type.

This is the same central idea from Lesson 7:

```text
FUNCTION
   +
ARGUMENT TYPES
   ↓
METHOD SELECTION
   ↓
APPROPRIATE METHOD
```

Now the argument types can be types that you have defined yourself.

---

## 36. Key Syntax

### Immutable struct

```julia
struct Name
    field1
    field2
end
```

### Mutable struct

```julia
mutable struct Name
    field1
    field2
end
```

### Field access

```julia
object.field
```

### Field names

```julia
fieldnames(TypeName)
```

### Constructor

```julia
object = TypeName(value1, value2)
```

### Method for a custom type

```julia
function operation(x::TypeName)
    ...
end
```

### Parametric struct

```julia
struct Name{T}
    value::T
end
```

### Parametric struct with arrays

```julia
struct State{T}
    position::Vector{T}
    velocity::Vector{T}
end
```

---

## 37. Structs in Scientific Computing

Structs provide a natural way to organize scientific programs.

A physical model can contain:

```text
parameters
    ↓
struct
    ↓
physical system
```

A simulation can contain:

```text
position
velocity
time
    ↓
state struct
```

A numerical method can then operate on that state:

```text
state
  ↓
function / method
  ↓
updated state
```

This becomes particularly useful when working with:

* differential equations
* numerical simulations
* optimization
* machine learning
* reinforcement learning
* scientific machine learning

---

# Summary

In this lesson, we learned how to create and use user-defined composite types in Julia.

The central concepts are:

1. `struct` creates a new immutable composite type.
2. Fields store the data associated with an object.
3. Fields are accessed using dot notation.
4. `fieldnames` reveals the fields of a type.
5. `mutable struct` allows fields to be modified.
6. Constructors create instances of a type.
7. Inner constructors allow validation and custom construction logic.
8. Outer constructors provide additional construction methods.
9. Methods can be defined specifically for user-defined types.
10. User-defined types participate in multiple dispatch.
11. Parametric structs allow types to depend on type parameters.
12. Structs can contain arrays and other structured scientific data.
13. Structs provide a natural way to represent scientific systems and simulation states.
14. Combining structs with multiple dispatch provides a powerful pattern for scientific Julia programming.
