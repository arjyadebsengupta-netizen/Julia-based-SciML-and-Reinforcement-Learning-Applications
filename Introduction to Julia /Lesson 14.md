# Lesson 14 — Packages and Project Environments

Introduction to Julia's package-management and environment system.

## Topics

- `Pkg`
- Installing packages
- Removing packages
- Updating packages
- `Project.toml`
- `Manifest.toml`
- Julia environments
- `Pkg.activate`
- `Pkg.status`
- `Pkg.instantiate`
- Package dependencies
- Direct versus transitive dependencies
- Package compatibility
- Environment isolation
- Reproducible scientific computing
- Project-specific environments
- Using Julia projects with GitHub

---

# 1. Introduction

Julia has a built-in package-management system called `Pkg`.

`Pkg` handles:

- Installing packages
- Removing packages
- Updating packages
- Managing dependencies
- Creating isolated project environments
- Recording package compatibility
- Reproducing a project's package setup

This is especially important in scientific computing because different projects may require different versions of the same package.

For example, one project might require:

    DifferentialEquations
    Flux
    CUDA

while another project might require:

    DifferentialEquations
    JuMP
    Plots

Each project can have its own environment.

---

# 2. Pkg

`Pkg` is Julia's standard package manager.

It is included with Julia.

The package manager can be accessed through the Julia package-management mode by pressing:

    ]

For example:

    julia> ]

The prompt changes to something similar to:

    (@v1.12) pkg>

The package prompt allows package-management commands.

Pressing `Backspace` or `Ctrl+C` returns to the normal Julia prompt.

You can also use `Pkg` programmatically:

    julia> import Pkg

    julia> Pkg.status()

---

# 3. Installing Packages

Packages are installed using `Pkg.add`.

From the package prompt:

    (@v1.12) pkg> add DataFrames

Or programmatically:

    julia> import Pkg
    julia> Pkg.add("DataFrames")

After installation, the package can be loaded:

    julia> using DataFrames

For example:

    julia> df = DataFrame(x = 1:3, y = [10, 20, 30])

The important distinction is:

    Pkg.add("DataFrames")

installs the package into the active environment.

Whereas:

    using DataFrames

loads the package into the current Julia session.

---

# 4. Removing Packages

A package can be removed using `Pkg.rm`.

Package prompt:

    (@v1.12) pkg> rm DataFrames

Programmatically:

    julia> import Pkg
    julia> Pkg.rm("DataFrames")

Removing a package removes it from the project's direct dependencies.

Julia may still retain package files in its global package storage because packages can be shared between environments.

---

# 5. Updating Packages

Packages can be updated using `Pkg.update`.

For example:

    julia> import Pkg
    julia> Pkg.update()

Or from package mode:

    (@v1.12) pkg> update

You can also update a particular package:

    (@v1.12) pkg> update DataFrames

Updating does not mean that every package will necessarily jump to the newest available version.

Julia's compatibility rules and the project's dependency constraints are considered when resolving versions.

---

# 6. Julia Environments

A Julia environment is a collection of package-management information associated with a project.

An environment determines:

- Which packages are direct dependencies
- Which package versions are allowed
- Which exact dependency versions are resolved
- Which package versions are used by the project

The most important files are:

    Project.toml
    Manifest.toml

A project environment might look like:

    scientific-ml/
    ├── Project.toml
    ├── Manifest.toml
    ├── src/
    ├── scripts/
    ├── notebooks/
    └── data/

The environment belongs to the project rather than to a particular Julia session.

---

# 7. Project.toml

`Project.toml` describes the project's declared dependencies and compatibility requirements.

A simplified example:

    name = "ScientificML"
    uuid = "..."
    version = "0.1.0"

    [deps]
    DataFrames = "..."
    DifferentialEquations = "..."
    Plots = "..."

    [compat]
    DataFrames = "1"
    DifferentialEquations = "7"
    Plots = "1"

The exact UUIDs are generated and managed by Julia.

The `[deps]` section records direct dependencies.

The `[compat]` section specifies versions that the project is compatible with.

---

# 8. Manifest.toml

`Manifest.toml` records the resolved package environment in much greater detail.

It can contain:

- Exact package versions
- Dependency relationships
- Package UUIDs
- Source information
- Dependency versions
- Other information needed to reproduce the environment

For example, your project might directly depend on:

    DifferentialEquations

But `DifferentialEquations` itself depends on many other packages.

The manifest records the resolved dependency graph.

Conceptually:

    Your project
        |
        +-- DifferentialEquations
        |       |
        |       +-- Dependency A
        |       +-- Dependency B
        |
        +-- Plots
                |
                +-- Dependency C

`Project.toml` describes what the project asks for.

`Manifest.toml` describes the concrete environment that was resolved.

---

# 9. Project.toml versus Manifest.toml

The distinction is important.

`Project.toml`:

- Declares the project's direct dependencies
- Declares compatibility requirements
- Describes the project itself

`Manifest.toml`:

- Records the resolved dependency graph
- Records concrete package versions
- Helps reproduce the exact environment

A useful mental model is:

    Project.toml
        = What my project depends on

    Manifest.toml
        = What Julia actually resolved for this project

---

# 10. Pkg.activate

`Pkg.activate` changes the active Julia environment.

For example:

    julia> import Pkg
    julia> Pkg.activate(".")

The `"."` means the current directory.

If the current directory contains a `Project.toml`, Julia activates that project.

You can also specify another directory:

    julia> Pkg.activate("~/JuliaProjects/scientific-ml")

After activation, package operations apply to that environment.

For example:

    julia> Pkg.activate("~/JuliaProjects/scientific-ml")
    julia> Pkg.status()

---

# 11. Checking the Active Environment

`Pkg.status()` displays information about the active environment.

For example:

    julia> import Pkg
    julia> Pkg.status()

You may see output similar to:

    Status `~/JuliaProjects/scientific-ml/Project.toml`
      [....] DataFrames v1.x
      [....] Plots v1.x

This tells you:

- Which project is active
- Which packages are installed in that environment
- Their resolved versions

Package mode provides the same functionality:

    (@v1.12) pkg> status

---

# 12. Pkg.instantiate

`Pkg.instantiate()` is particularly important for reproducibility.

Suppose you clone a Julia project from GitHub.

The project contains:

    Project.toml
    Manifest.toml

You can activate the project:

    julia> import Pkg
    julia> Pkg.activate(".")

Then instantiate it:

    julia> Pkg.instantiate()

Julia reads the project's package-management files and installs the required packages.

This makes it possible to set up a project without manually installing every package one by one.

---

# 13. A Typical Project Setup

Suppose you create:

    my-project/

Enter the directory and activate it:

    julia> import Pkg
    julia> Pkg.activate(".")

Then add packages:

    julia> Pkg.add("Plots")
    julia> Pkg.add("DataFrames")

Julia creates or updates:

    Project.toml
    Manifest.toml

The project is now isolated from unrelated Julia projects.

---

# 14. Package Dependencies

Packages can depend on other packages.

For example:

    MyProject
        |
        +-- PackageA
        |
        +-- PackageB
                |
                +-- PackageC

Your project does not necessarily need to declare `PackageC` directly.

If `PackageB` requires `PackageC`, Julia's package resolver handles that dependency.

This creates a dependency graph.

---

# 15. Direct Dependencies

A direct dependency is a package that your project explicitly declares.

For example:

    julia> Pkg.add("DataFrames")

means `DataFrames` becomes a direct dependency of the active project.

Conceptually:

    MyProject
        |
        +-- DataFrames

It appears in the project's `[deps]` section.

---

# 16. Transitive Dependencies

A transitive dependency is a dependency required indirectly through another dependency.

For example:

    MyProject
        |
        +-- PackageA
                |
                +-- PackageB
                        |
                        +-- PackageC

Here:

- `PackageA` is a direct dependency
- `PackageB` is a transitive dependency
- `PackageC` is also a transitive dependency

You normally do not need to manually manage every transitive dependency.

Julia's package resolver handles the dependency graph.

---

# 17. Why Direct versus Transitive Dependencies Matter

Suppose your code contains:

    using DataFrames

Then `DataFrames` should normally be a direct dependency of your project.

You should not rely on some other package happening to install `DataFrames` as a transitive dependency.

For example, this is fragile:

    MyProject
        |
        +-- PackageA
                |
                +-- DataFrames

while your code directly uses:

    using DataFrames

Instead, declare `DataFrames` directly:

    MyProject
        |
        +-- PackageA
        |
        +-- DataFrames

This makes the project dependency structure explicit.

---

# 18. Package Compatibility

Julia packages specify compatibility requirements.

For example, a project may declare:

    [compat]
    DataFrames = "1"
    Plots = "1"

This does not necessarily mean one exact version.

It specifies an allowed compatibility range according to Julia's package-version compatibility rules.

Compatibility information helps the resolver determine which versions can coexist.

---

# 19. Dependency Resolution

Suppose a project requires:

    PackageA
    PackageB

and:

    PackageA → DependencyX
    PackageB → DependencyX

Julia attempts to find a version of `DependencyX` compatible with both packages.

Conceptually:

    PackageA ──┐
               ├──> DependencyX
    PackageB ──┘

The package resolver handles this automatically.

If no compatible combination exists, package resolution can fail rather than silently constructing an incompatible environment.

---

# 20. Environment Isolation

Different projects can use different package versions.

For example:

    Project A
        DataFrames 1.x
        Plots 1.x

    Project B
        DataFrames 2.x
        Plots 1.x

The projects can maintain separate environments.

This prevents one project's package requirements from automatically changing another project's environment.

---

# 21. Why Environment Isolation Matters

Scientific projects often depend on many packages.

For example:

    Project A
        Flux
        CUDA
        DifferentialEquations
        Plots

while another project might use:

    Project B
        JuMP
        Optim
        DataFrames
        Plots

Trying to maintain one giant global environment can make dependency management unnecessarily difficult.

Project-specific environments keep dependencies organized.

---

# 22. Reproducible Scientific Computing

Reproducibility means that another researcher should be able to recreate the computational environment used for a study.

A Julia project can record:

    Project.toml
    Manifest.toml

along with:

    source code
    configuration files
    data-processing scripts
    notebooks

This gives other researchers the information needed to reconstruct the software environment.

A reproducible scientific project might look like:

    research-project/
    ├── Project.toml
    ├── Manifest.toml
    ├── src/
    ├── scripts/
    ├── notebooks/
    ├── data/
    └── README.md

---

# 23. Project-Specific Environments

A good scientific-computing workflow is to create one environment for each substantial project.

For example:

    ~/JuliaProjects/
    ├── scientific-ml/
    │   ├── Project.toml
    │   └── Manifest.toml
    │
    ├── qkd-ml/
    │   ├── Project.toml
    │   └── Manifest.toml
    │
    └── equation-discovery/
        ├── Project.toml
        └── Manifest.toml

Each project can then have its own package configuration.

---

# 24. Creating a New Environment

You can create a new project environment by activating a directory.

For example:

    julia> import Pkg
    julia> Pkg.activate("my-project")

If the directory does not yet contain a project environment, Julia can create the required project structure when packages are added.

Then:

    julia> Pkg.add("Plots")

The environment now records `Plots` as a dependency.

---

# 25. Working Inside an Activated Project

A typical workflow is:

    julia> import Pkg

    julia> Pkg.activate(".")

    julia> Pkg.status()

    julia> Pkg.add("DifferentialEquations")

    julia> Pkg.status()

Then your Julia code can use the package:

    using DifferentialEquations

The package is available because it belongs to the active project environment.

---

# 26. Checking the Active Project

A useful command is:

    julia> Base.active_project()

This returns the path of the currently active project file.

For example:

    "/home/user/project/Project.toml"

This is useful when you are unsure which environment Julia is currently using.

---

# 27. Package Status from Package Mode

You can enter package mode with:

    julia> ]

Then:

    (@v1.12) pkg> status

You might see:

    Status `~/JuliaProjects/scientific-ml/Project.toml`
      [....] Flux v0.x
      [....] Plots v1.x

The environment shown in the status output is important.

Always check it when debugging package-related problems.

---

# 28. Using a Project Environment from the Julia Command Line

Julia can start directly in a particular project environment.

For example:

    julia --project=.

This activates the project in the current directory.

You can also specify a project directory:

    julia --project=/path/to/project

This is useful when running scripts.

For example:

    julia --project=. simulation.jl

The script runs using the project's environment.

---

# 29. Running Scientific Scripts Reproducibly

Suppose a project contains:

    Project.toml
    Manifest.toml
    simulation.jl

A user can run:

    julia --project=. -e 'using Pkg; Pkg.instantiate(); include("simulation.jl")'

The important sequence is:

    activate project
          ↓
    instantiate dependencies
          ↓
    run scientific code

This reduces dependency-related differences between machines.

---

# 30. Using Julia Projects with GitHub

A Julia scientific project can be stored in a Git repository.

For example:

    scientific-project/
    ├── Project.toml
    ├── Manifest.toml
    ├── src/
    ├── scripts/
    ├── notebooks/
    └── README.md

The repository records the project environment alongside the source code.

Another researcher can clone the repository and activate the project.

Conceptually:

    GitHub repository
          ↓
    git clone
          ↓
    cd project
          ↓
    Pkg.activate(".")
          ↓
    Pkg.instantiate()
          ↓
    run code

---

# 31. Cloning a Julia Project

After cloning a repository:

    cd scientific-project

Start Julia:

    julia

Activate the project:

    julia> import Pkg
    julia> Pkg.activate(".")

Instantiate its dependencies:

    julia> Pkg.instantiate()

Check the environment:

    julia> Pkg.status()

Now the project environment is ready.

---

# 32. GitHub and Reproducibility

A scientific GitHub repository can contain:

    Project.toml
    Manifest.toml
    src/
    scripts/
    notebooks/
    README.md

The source code and environment specification are therefore distributed together.

This is particularly useful for:

- Computational physics
- Scientific machine learning
- Numerical PDEs
- Reinforcement learning
- Optimization
- Data analysis
- Reproducible research

---

# 33. Project Environment Example

Consider a scientific machine-learning project:

    scientific-ml/
    ├── Project.toml
    ├── Manifest.toml
    ├── src/
    │   ├── models.jl
    │   └── operators.jl
    ├── scripts/
    │   ├── train.jl
    │   └── evaluate.jl
    ├── notebooks/
    └── README.md

The project environment might contain:

    Flux
    Lux
    DifferentialEquations
    Plots

The source code can then use:

    using Flux
    using DifferentialEquations
    using Plots

without requiring those packages to be installed into every other Julia project.

---

# 34. Environment Isolation and Scientific Experiments

Suppose experiment A uses:

    Flux
    DifferentialEquations

while experiment B uses:

    Lux
    SciMLSensitivity

They can have separate environments.

This is useful when experiments require different package versions.

For example:

    experiment-A/
    ├── Project.toml
    └── Manifest.toml

    experiment-B/
    ├── Project.toml
    └── Manifest.toml

Each environment can be activated independently.

---

# 35. Pkg Workflow

A basic project workflow is:

    1. Create or enter project directory
    2. Activate the project
    3. Add required packages
    4. Develop the scientific code
    5. Test the code
    6. Commit Project.toml
    7. Commit Manifest.toml when appropriate
    8. Share the repository

The essential commands are:

    Pkg.activate(".")
    Pkg.add("PackageName")
    Pkg.status()
    Pkg.instantiate()

---

# 36. Important Mental Model

Think of Julia package management as having several layers:

    Julia installation
          ↓
    Julia package storage
          ↓
    Project environment
          ↓
    Project.toml
          ↓
    Manifest.toml
          ↓
    Your scientific code

The project environment determines which package versions are used by that project.

---

# 37. Common Mistake: Installing Globally and Forgetting the Project

A common beginner workflow is:

    Pkg.add("PackageA")
    Pkg.add("PackageB")
    Pkg.add("PackageC")

without checking which environment is active.

The packages may be added to an unintended environment.

A safer workflow is:

    Pkg.activate(".")
    Pkg.status()
    Pkg.add("PackageA")

Always know which project is active before changing its dependencies.

---

# 38. Common Mistake: Relying on Transitive Dependencies

Suppose `PackageA` happens to depend on `PackageB`.

Your code uses:

    using PackageB

It may work temporarily even if `PackageB` was not declared as a direct dependency.

This is poor project organization.

If your project directly uses a package, declare it as a direct dependency.

For example:

    Pkg.add("PackageB")

Then your project's dependency structure explicitly records the requirement.

---

# 39. Common Mistake: Confusing Installing with Loading

These operations are different:

    Pkg.add("Plots")

means:

    Install and register Plots as a project dependency.

Whereas:

    using Plots

means:

    Load Plots into the current Julia session.

You generally install a package once per environment and load it whenever your code needs it.

---

# 40. Common Mistake: Forgetting to Instantiate a Cloned Project

After cloning a project from GitHub, do not immediately assume every dependency is installed.

Use:

    julia> import Pkg
    julia> Pkg.activate(".")
    julia> Pkg.instantiate()

Then:

    julia> Pkg.status()

This prepares the environment described by the project's package-management files.

---

# 41. Scientific Computing Workflow

For a scientific project, a useful pattern is:

    project/
    ├── Project.toml
    ├── Manifest.toml
    ├── src/
    ├── scripts/
    ├── notebooks/
    ├── test/
    └── README.md

Then:

    julia> import Pkg
    julia> Pkg.activate(".")
    julia> Pkg.instantiate()

And scientific code can be run inside the project environment.

---

# 42. Example: Numerical Simulation Project

Suppose you create:

    numerical-pde/

Activate it:

    julia> import Pkg
    julia> Pkg.activate(".")

Add packages:

    julia> Pkg.add("DifferentialEquations")
    julia> Pkg.add("Plots")

Check:

    julia> Pkg.status()

The project now knows that these packages are direct dependencies.

A simulation file might contain:

    using DifferentialEquations
    using Plots

The code runs using the versions resolved for this project.

---

# 43. Example: Scientific ML Project

Suppose:

    scientific-ml/

contains:

    Project.toml
    Manifest.toml
    train.jl

You can set up the environment:

    julia> import Pkg
    julia> Pkg.activate(".")
    julia> Pkg.add("Flux")
    julia> Pkg.add("Plots")

Then:

    julia> include("train.jl")

Or from the shell:

    julia --project=. train.jl

The script uses the project's package environment.

---

# 44. Package Environment as Part of the Experiment

For computational research, the environment is part of the computational setup.

A complete experiment is not just:

    source code

It is closer to:

    source code
    +
    package environment
    +
    configuration
    +
    data
    +
    computational procedure

`Project.toml` and `Manifest.toml` help capture the package-environment component.

---

# 45. Key Commands

The most important commands from this lesson are:

    import Pkg

    Pkg.activate(".")

    Pkg.status()

    Pkg.add("PackageName")

    Pkg.rm("PackageName")

    Pkg.update()

    Pkg.instantiate()

Remember:

    activate
        → choose the environment

    add
        → add a dependency

    rm
        → remove a dependency

    update
        → update compatible dependencies

    status
        → inspect the environment

    instantiate
        → recreate/install the environment

---

# 46. Core Mental Model

Julia's package system can be understood as:

    Project
       |
       +-- Project.toml
       |       |
       |       +-- Direct dependencies
       |       +-- Compatibility requirements
       |
       +-- Manifest.toml
               |
               +-- Resolved dependency graph
               +-- Concrete package versions

The project environment isolates this dependency configuration from other projects.

Therefore:

    Project.toml
        describes the project requirements

    Manifest.toml
        records the resolved environment

    Pkg.activate
        selects the environment

    Pkg.add
        adds dependencies

    Pkg.instantiate
        recreates the environment

This system is one of the foundations of reproducible Julia-based scientific computing.
