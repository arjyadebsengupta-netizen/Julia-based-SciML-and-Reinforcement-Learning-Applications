# Lesson 13 — Modules and Namespaces

Introduction to organizing Julia code into reusable namespaces.

## Topics

- Modules
- Defining modules
- `export`
- `using`
- `import`
- Qualified names
- Namespaces
- Module constants
- Nested modules
- Standard-library modules
- Namespace conflicts
- Extending existing functions
- `Base`
- `include`
- `names`
- `isdefined`
- `parentmodule`
- Modules containing structs
- Modules containing functions
- Modules and multiple dispatch
- Modules versus packages
- Scientific project organization

---

## 1. Modules

A module is a namespace that groups related names together.

A module can contain:

- Variables
- Constants
- Functions
- Structs
- Types
- Other modules

Modules help prevent unrelated code from interfering with each other.

For example:

    module PhysicsTools

    # code here

    end

The names defined inside `PhysicsTools` belong to that module's namespace.

---

## 2. Defining Modules

The basic syntax is:

    module MyModule

    # definitions

    end

Example:

    module PhysicsTools

    const c = 299792458

    function energy(m)
        return m * c^2
    end

    end

The module can then be referenced as:

    PhysicsTools.c

and:

    PhysicsTools.energy(1.0)

---

## 3. `export`

Names inside a module are not automatically made available as unqualified names when the module is brought into another scope.

You can explicitly export names.

Example:

    module PhysicsTools

    export energy

    function energy(m)
        return m
    end

    end

If another module uses:

    using .PhysicsTools

then `energy` can be accessed directly:

    energy(10)

Without `export`, the qualified form can still be used:

    PhysicsTools.energy(10)

A useful principle is:

> `export` controls which names are intended to be part of a module's public interface.

---

## 4. `using`

`using` makes exported names from another module available.

For example:

    using LinearAlgebra

You can then use exported functionality such as:

    norm([3, 4])

You can also use a relative module:

    using .PhysicsTools

The dot means that Julia should look for `PhysicsTools` relative to the current module.

---

## 5. `import`

`import` brings a module or particular names into the current namespace.

For example:

    import LinearAlgebra

You can then refer to:

    LinearAlgebra.norm([3, 4])

You can also import a specific function:

    import Base: show

This is particularly important when you want to extend an existing function with a new method.

---

## 6. `using` versus `import`

A useful distinction is:

    using ModuleName

makes exported names available for use.

For example:

    using LinearAlgebra

Then:

    norm([3, 4])

can be used directly.

With:

    import LinearAlgebra

you generally use the qualified name:

    LinearAlgebra.norm([3, 4])

`import` is also used when extending functions from another module.

For example:

    import Base: show

---

## 7. Qualified Names

A qualified name explicitly specifies the module containing a name.

The syntax is:

    ModuleName.name

Example:

    LinearAlgebra.norm

Another example:

    PhysicsTools.energy

Qualified names make the source of a name explicit.

This can help avoid ambiguity when multiple modules contain similarly named functions.

---

## 8. Namespaces

A namespace is the collection of names associated with a particular scope or module.

For example:

    module A

    x = 10

    end

    module B

    x = 20

    end

There are two different `x` variables:

    A.x

and:

    B.x

They do not conflict because they belong to different namespaces.

---

## 9. Module Constants

A module can define constants.

Example:

    module PhysicalConstants

    const c = 299792458.0
    const h = 6.62607015e-34

    end

You can access them using qualified names:

    PhysicalConstants.c

    PhysicalConstants.h

Constants are useful for values that conceptually remain fixed throughout a program.

---

## 10. Nested Modules

Modules can contain other modules.

Example:

    module ScientificTools

        module Physics

            const c = 299792458.0

        end

    end

The nested module can be accessed as:

    ScientificTools.Physics.c

Nested modules can organize larger projects hierarchically.

---

## 11. Standard-Library Modules

Julia provides standard-library modules for common functionality.

Examples include:

    LinearAlgebra
    Statistics
    Random
    Dates
    SparseArrays

For example:

    using LinearAlgebra

Then:

    A = [1.0 2.0; 3.0 4.0]

    det(A)

Another example:

    using Statistics

    data = [1, 2, 3, 4, 5]

    mean(data)

The exact functionality available depends on the standard-library module being used.

---

## 12. Namespace Conflicts

Different modules can export names with the same name.

For example, suppose two modules both define:

    calculate

If both are brought into the same namespace, Julia may encounter a name conflict.

A simple way to avoid ambiguity is to use qualified names:

    ModuleA.calculate(...)
    ModuleB.calculate(...)

You can also control what you import or use.

The general principle is:

> Qualified names make ownership of a function or variable explicit.

---

## 13. Extending Existing Functions

One of Julia's most important features is multiple dispatch.

Suppose Julia already provides a function:

    f(x)

You can add a new method for your own type.

For example:

    struct Particle
        mass::Float64
    end

Suppose you want a custom display method.

You can extend `Base.show`:

    import Base: show

    function show(io::IO, p::Particle)
        print(io, "Particle(mass=$(p.mass))")
    end

Now Julia knows how to display your `Particle`.

You are not replacing `Base.show`.

You are adding a new method to the existing generic function.

---

## 14. `Base`

`Base` is Julia's core module containing fundamental language functionality.

Many familiar functions belong to `Base`.

Examples include:

    println
    show
    length
    getindex
    iterate
    + 
    *

You can inspect the module:

    Base

Functions can be extended by importing them.

For example:

    import Base: length

Then define:

    length(x::MyType) = ...

This adds a new method to `Base.length`.

---

## 15. `include`

`include` evaluates the contents of another Julia source file in the current module.

Suppose a project contains:

    src/
        Physics.jl
        Utilities.jl
        Models.jl

A main module could contain:

    module ScientificProject

    include("Physics.jl")
    include("Utilities.jl")
    include("Models.jl")

    end

The code in those files becomes part of the module's source.

`include` is useful for splitting a large module across multiple files.

---

## 16. `names`

The `names` function can be used to inspect names associated with a module.

For example:

    names(Base)

You can also request imported names:

    names(Base, all=true)

This can be useful when exploring a module or debugging namespace issues.

---

## 17. `isdefined`

`isdefined` checks whether a name is defined in a module.

Example:

    isdefined(Base, :println)

This returns:

    true

You can use it to test whether a particular binding exists.

For example:

    isdefined(Main, :x)

checks whether `x` is defined in `Main`.

---

## 18. `parentmodule`

`parentmodule` returns the module in which a particular function, type, or other binding was originally defined.

For example:

    parentmodule(sin)

The result indicates the module associated with the definition of `sin`.

This is useful when investigating where functionality comes from.

---

## 19. Modules Containing Structs

Modules can organize custom types.

Example:

    module Particles

    struct Particle
        mass::Float64
        charge::Float64
    end

    end

The type is accessed as:

    Particles.Particle

You can create an instance:

    p = Particles.Particle(1.0, -1.0)

This keeps the type associated with the module that defines it.

---

## 20. Modules Containing Functions

Modules can also group related functions.

Example:

    module NumericalTools

    function square(x)
        return x^2
    end

    function cube(x)
        return x^3
    end

    end

These functions can be called using:

    NumericalTools.square(5)

    NumericalTools.cube(5)

This is useful for organizing functionality by purpose.

---

## 21. Modules and Multiple Dispatch

Modules work naturally with Julia's multiple-dispatch system.

Suppose:

    module Particles

    struct Particle
        mass::Float64
    end

    end

You can define a function for that type:

    module PhysicsTools

    function energy(p::Particles.Particle, v)
        return 0.5 * p.mass * v^2
    end

    end

The method dispatches according to the types of its arguments.

The important point is:

> Modules organize names; multiple dispatch determines which method is selected.

These two mechanisms work together but serve different purposes.

---

## 22. Extending a Function from Another Module

Suppose a module defines a type:

    module Particles

    struct Particle
        mass::Float64
    end

    end

You want to extend an existing function such as `show`.

Use:

    import Base: show

Then define:

    function show(io::IO, p::Particles.Particle)
        print(io, "Particle(mass=$(p.mass))")
    end

The important sequence is:

    import Base: show

followed by:

    function show(...)

This tells Julia that you are intentionally extending the existing generic function.

---

## 23. Public and Internal Names

A module may contain both public and internal functionality.

For example:

    module NumericalTools

    export solve

    function solve(x)
        return _internal_algorithm(x)
    end

    function _internal_algorithm(x)
        return x^2
    end

    end

The exported function is:

    solve

The helper function:

    _internal_algorithm

can remain an internal implementation detail.

The underscore itself does not enforce privacy. It is simply a naming convention.

---

## 24. Modules versus Packages

A module is a namespace and a unit of code organization.

A package is a distributable Julia project with a defined project structure and package metadata.

A package can contain one or more modules.

Conceptually:

    Package
        ↓
    Project structure
        ↓
    Module(s)
        ↓
    Types and functions

A module can exist without being a complete package.

Packages provide additional infrastructure for:

- Dependency management
- Versioning
- Reproducible environments
- Testing
- Distribution

---

## 25. Scientific Project Organization

Modules become especially useful as a scientific project grows.

A small project might begin as:

    main.jl

containing everything.

As it grows, it can be organized into:

    ScientificProject/
        Project.toml
        src/
            ScientificProject.jl
            Physics.jl
            Models.jl
            Numerics.jl
            Utilities.jl
        test/
            runtests.jl

The main module might contain:

    module ScientificProject

    include("Physics.jl")
    include("Models.jl")
    include("Numerics.jl")
    include("Utilities.jl")

    end

This separates different responsibilities.

---

## 26. Example Scientific Module

A simple physics module:

    module Physics

    export kinetic_energy
    export potential_energy

    function kinetic_energy(m, v)
        return 0.5 * m * v^2
    end

    function potential_energy(m, g, h)
        return m * g * h
    end

    end

Using it:

    using .Physics

    kinetic_energy(2.0, 3.0)

    potential_energy(2.0, 9.81, 5.0)

Or use qualified names:

    Physics.kinetic_energy(2.0, 3.0)

    Physics.potential_energy(2.0, 9.81, 5.0)

---

## 27. Example Scientific Module with a Struct

A module can contain both data structures and operations.

    module Particles

    export Particle, momentum

    struct Particle
        mass::Float64
        velocity::Float64
    end

    function momentum(p::Particle)
        return p.mass * p.velocity
    end

    end

Then:

    using .Particles

    p = Particle(2.0, 3.0)

    momentum(p)

This keeps the type and the operations associated with the same conceptual domain.

---

## 28. Module Organization and Multiple Dispatch

Suppose we have different physical systems.

    module Physics

    abstract type System end

    struct ClassicalParticle <: System
        mass::Float64
    end

    struct HarmonicOscillator <: System
        mass::Float64
        omega::Float64
    end

    function energy(s::ClassicalParticle, v)
        return 0.5 * s.mass * v^2
    end

    function energy(s::HarmonicOscillator, x, v)
        return 0.5 * s.mass * v^2 +
               0.5 * s.mass * s.omega^2 * x^2
    end

    end

The module organizes the physics-related names.

Multiple dispatch chooses the appropriate `energy` method based on the argument types.

---

## 29. Relative Modules

Inside a module, a leading `.` can refer to a module relative to the current module.

For example:

    module Project

        module Physics
            function energy(x)
                x^2
            end
        end

        using .Physics

    end

The `.Physics` form indicates that `Physics` is a module in the current module hierarchy.

---

## 30. Qualified Access versus Exported Access

Suppose:

    module Physics

    export energy

    function energy(x)
        return x^2
    end

    end

You can use:

    using .Physics

Then:

    energy(5)

Or explicitly:

    Physics.energy(5)

Qualified access is often useful when you want to make the origin of a name clear.

---

## 31. Avoiding Namespace Pollution

Exporting too many names can make a module difficult to use.

For example, a module may contain many helper functions:

    helper1
    helper2
    helper3
    helper4
    solve
    simulate
    analyze

It may be better to export only the intended public interface:

    export solve, simulate, analyze

Internal helpers can remain accessible through qualified names if necessary.

A good module should have a clear public interface.

---

## 32. Scientific Example: Numerical Solver Module

Consider:

    module Solvers

    export euler_step

    function euler_step(f, x, dt)
        return x + dt * f(x)
    end

    end

Using it:

    using .Solvers

    f(x) = -x

    x = 1.0
    dt = 0.01

    x_next = euler_step(f, x, dt)

This keeps the numerical integration routine separate from the physical model.

---

## 33. Scientific Example: Model Module

The physical model can live in another module:

    module Models

    export decay

    function decay(x, λ)
        return -λ * x
    end

    end

Then a solver module can operate on the model:

    using .Models
    using .Solvers

    x = 1.0
    λ = 0.5
    dt = 0.01

    x_next = euler_step(x -> decay(x, λ), x, dt)

This separation is useful in computational physics because the model and numerical method can be developed independently.

---

# 34. Key Takeaways

1. A module creates a namespace for related code.

2. Modules can contain:
   - Functions
   - Structs
   - Constants
   - Types
   - Other modules

3. Define a module with:

       module MyModule
           ...
       end

4. `export` declares names intended for convenient external use.

5. `using` makes exported names available.

6. `import` brings modules or specific names into scope and is important when extending existing functions.

7. Qualified names use:

       ModuleName.name

8. Qualified names help avoid namespace conflicts.

9. Modules can contain constants.

10. Modules can be nested.

11. Julia's standard library is organized into modules such as `LinearAlgebra`, `Statistics`, `Random`, and `SparseArrays`.

12. Namespace conflicts can often be resolved by using qualified names.

13. Existing functions can be extended through multiple dispatch.

14. `Base` contains fundamental Julia functionality.

15. `include` allows a module's implementation to be split across multiple source files.

16. `names` can be used to inspect names associated with a module.

17. `isdefined` checks whether a name is defined.

18. `parentmodule` helps identify the module associated with a definition.

19. Modules can contain both structs and functions.

20. Modules and multiple dispatch complement each other:
    modules organize names, while multiple dispatch selects methods.

21. A package is a distributable Julia project; a module is primarily a namespace and code organization mechanism.

22. Scientific projects benefit from separating:
    - Physical models
    - Numerical methods
    - Data structures
    - Utilities
    - Analysis

23. A well-organized scientific project should expose a small, clear public interface while keeping implementation details internal.

---

# 35. Core Mental Model

Think of a scientific Julia project as:

    Package
        ↓
    Module
        ↓
    Namespace
        ↓
    Types + Functions + Constants
        ↓
    Multiple Dispatch

And think of the main module mechanisms as:

    module
        ↓
    creates namespace

    export
        ↓
    defines convenient public names

    using
        ↓
    brings exported names into scope

    import
        ↓
    explicitly brings in names/modules
    and enables intentional method extension

    Module.name
        ↓
    qualified access

    include
        ↓
    splits module implementation across files

This gives Julia projects a clean structure as they grow from small scripts into reusable scientific software.
