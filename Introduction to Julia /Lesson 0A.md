# Lecture 0.5 — How to Use Julia in Google Colab

A practical lecture focused specifically on running and using Julia in Google Colab.

This lecture assumes that the learner already understands what Julia is and instead answers:

> **How do I actually work with Julia in Google Colab?**

---

# 0.5.1 What Is Google Colab?

Google Colab is a cloud-based interactive notebook environment.

A Colab notebook consists of cells that can contain code, text, mathematical expressions, and other computational content. Code is executed on a remote runtime rather than directly on the user's computer.

For scientific computing, this provides a convenient environment for:

* numerical experiments
* machine learning
* scientific simulations
* data analysis
* experimentation with computational libraries
* GPU-accelerated computation when supported hardware is available

For this course, Colab provides a convenient environment in which we can execute Julia without requiring Julia to be permanently installed on the learner's personal computer.

---

# 0.5.2 Colab as a Cloud Computing Environment

When a Colab notebook is connected to a runtime, the notebook is associated with a temporary computational environment.

Conceptually:

```text
Your computer
     ↓
Web browser
     ↓
Google Colab notebook
     ↓
Remote runtime
     ↓
CPU / GPU / other available hardware
```

The code is executed by the remote runtime.

This makes Colab particularly useful when experimenting with computationally demanding scientific software.

However, the runtime should not be treated as a permanent computer. Files and software installed inside the runtime can disappear when the runtime is reset or terminated.

---

# 0.5.3 Running Julia in Google Colab

Julia can be used in Google Colab by configuring the runtime so that Julia is available for execution.

There are different ways of working with Julia in Colab. The exact interface available for selecting or configuring runtimes can change over time.

In this course, we use a direct and reproducible approach:

1. Start a normal Colab notebook.
2. Install Julia inside the current runtime when necessary.
3. Invoke Julia from Bash cells.
4. Execute Julia programs inside those cells.
5. Install and use Julia packages from the Julia package manager.

Our practical workflow is therefore:

```text
Colab notebook
      ↓
%%bash
      ↓
Julia executable
      ↓
Julia program
      ↓
Output
```

---

# 0.5.4 Configuring Julia in the Colab Runtime

When Julia is not already available in the current runtime, we can install it manually.

The following cell installs Julia 1.12.7.

```python
#Installing Julia
%%bash
wget -q https://julialang-s3.julialang.org/bin/linux/x64/1.12/julia-1.12.7-linux-x86_64.tar.gz
tar -xzf julia-1.12.7-linux-x86_64.tar.gz
mv julia-1.12.7 /usr/local/julia
ln -sf /usr/local/julia/bin/julia /usr/local/bin/julia
```

The important commands are:

* `wget` — downloads the Julia archive.
* `tar` — extracts the archive.
* `mv` — places the Julia installation in `/usr/local/julia`.
* `ln -sf` — creates a convenient `julia` command.

After this cell, Julia can be invoked using:

```text
julia
```

---

# 0.5.5 Checking the Julia Installation

We can verify that Julia is available:

```python
#Checking the Julia version
%%bash
julia --version
```

This confirms that the Julia executable can be found by the runtime.

For the setup used in this course, the expected version is Julia 1.12.7.

---

# 0.5.6 Executing Julia Code in Colab

A Colab cell is not automatically a Julia cell in the workflow used in this course.

We explicitly invoke Julia from a Bash cell.

For a short expression:

```python
#Executing a Julia expression
%%bash
julia -e 'println("Hello from Julia")'
```

The option:

```text
-e
```

tells Julia to execute the supplied expression.

For larger programs, a here-document is much more convenient.

```python
#Executing a multi-line Julia program
%%bash
julia <<'JULIA'
x = 10
y = 20

println(x + y)
JULIA
```

Everything between the two `JULIA` markers is Julia code.

---

# 0.5.7 Understanding the `%%bash` Workflow

The following structure is fundamental to this course:

```python
%%bash
julia <<'JULIA'

# Julia code

JULIA
```

There are several layers involved.

### Layer 1 — Colab

```text
%%bash
```

tells Colab to execute the cell through Bash.

### Layer 2 — Julia

```text
julia
```

starts the Julia executable.

### Layer 3 — Here-document

```text
<<'JULIA'
```

begins a block of text that is passed to Julia.

### Layer 4 — Julia program

Everything between the two markers is interpreted as Julia code.

This allows us to write ordinary multi-line Julia programs while remaining inside a Colab notebook.

---

# 0.5.8 Julia Cells and Notebook Workflow

A practical Julia notebook can contain a sequence of cells.

For example:

```text
Cell 1
Install/configure Julia

       ↓

Cell 2
Check Julia version

       ↓

Cell 3
Install required packages

       ↓

Cell 4
Run Julia code

       ↓

Cell 5
Perform numerical experiment

       ↓

Cell 6
Save results
```

This structure makes experiments reproducible and easy to modify.

Installation and configuration cells are normally placed near the beginning of the notebook.

---

# 0.5.9 Installing Julia Packages

Julia uses a package manager called `Pkg`.

Packages can be installed from within Julia.

For example:

```python
#Installing IJulia
%%bash
julia -e 'using Pkg; Pkg.add("IJulia")'
```

Another example is `ForwardDiff`:

```python
#Installing ForwardDiff
%%bash
julia -e 'using Pkg; Pkg.add("ForwardDiff")'
```

Once installed, a package can be loaded using `using`.

---

# 0.5.10 Using `Pkg`

`Pkg` is Julia's package-management system.

It can be used to:

* install packages
* remove packages
* update packages
* inspect package status
* activate environments
* manage project dependencies

For example:

```python
#Checking installed Julia packages
%%bash
julia -e 'using Pkg; Pkg.status()'
```

This displays information about the packages available in the current Julia environment.

---

# 0.5.11 Julia Environments in Colab

Julia environments allow a project to specify the packages and versions it depends on.

A project environment is associated with files such as:

```text
Project.toml
Manifest.toml
```

Conceptually:

```text
Julia project
     │
     ├── Project.toml
     │       ↓
     │   Direct dependencies
     │
     └── Manifest.toml
             ↓
        Resolved dependency versions
```

This becomes increasingly important when scientific projects contain many dependencies.

---

# 0.5.12 Activating an Environment

An environment can be activated using `Pkg.activate()`.

For example:

```python
#Activating a Julia environment
%%bash
julia <<'JULIA'
using Pkg

Pkg.activate("/content/my_project")

println("Active environment:")
Pkg.status()
JULIA
```

The path should point to the directory containing the project's Julia environment.

Once an environment is activated, package operations are associated with that environment.

---

# 0.5.13 Working with `Project.toml`

A Julia project can contain a `Project.toml` file describing its direct dependencies.

A simplified project might contain information corresponding to:

```text
Project.toml
      ↓
Project dependencies
      ↓
DataFrames
ForwardDiff
DifferentialEquations
...
```

The environment can then be activated before running the project.

For reproducible scientific work, project environments are preferable to installing packages randomly into an unspecified environment.

---

# 0.5.14 Using Julia Packages Inside Colab

After a package has been installed in the appropriate environment, it can be loaded and used from Julia.

For example, `ForwardDiff` can calculate derivatives automatically.

```python
#Using ForwardDiff
%%bash
julia <<'JULIA'
using ForwardDiff

f(x) = x^3 + 2x^2 + x

println(ForwardDiff.derivative(f, 2.0))
JULIA
```

The important workflow is:

```text
Install package
      ↓
Activate appropriate environment
      ↓
using PackageName
      ↓
Use package functionality
```

---

# 0.5.15 Running Julia Scripts from Colab

Julia programs can also be stored in `.jl` files.

For example, suppose a file called `experiment.jl` exists in the Colab filesystem.

It can be executed with:

```python
#Running a Julia script
%%bash
julia experiment.jl
```

This is useful when a program becomes too large to keep inside a single notebook cell.

A practical project can therefore contain:

```text
project/
│
├── Project.toml
├── Manifest.toml
├── experiment.jl
└── data/
```

The notebook can be used for experimentation while larger programs can be maintained as Julia source files.

---

# 0.5.16 Working with Files in the Colab Filesystem

The Colab runtime provides a filesystem that can be accessed by programs running in the environment.

Julia can read and write files in this filesystem.

For example:

```python
#Writing a text file from Julia
%%bash
julia <<'JULIA'
open("example.txt", "w") do file
    write(file, "Hello from Julia\n")
end

println("File created.")
JULIA
```

The file is created inside the current working directory of the runtime.

We can also inspect the directory from Bash:

```python
#Listing files
%%bash
ls
```

---

# 0.5.17 Colab's Temporary Runtime Filesystem

The filesystem associated with a Colab runtime should generally be regarded as temporary.

For example:

```text
Colab runtime starts
       ↓
Install Julia
       ↓
Install packages
       ↓
Create files
       ↓
Run experiments
       ↓
Runtime terminates
       ↓
Temporary environment may disappear
```

Therefore, important research data should not exist only inside the temporary runtime.

This is especially important for:

* datasets
* trained models
* experiment results
* figures
* source code
* project environments
* important notebooks

---

# 0.5.18 Saving Important Work Outside the Runtime

Important work should be copied or saved to persistent storage.

Depending on the project, this can include:

* downloading files to your computer
* storing notebooks externally
* using persistent cloud storage
* using Git repositories
* storing research datasets in appropriate persistent storage

The key principle is:

> **The Colab runtime is a computational workspace, not necessarily permanent storage.**

For serious scientific projects, source code and important experimental results should be maintained in a reproducible project structure.

---

# 0.5.19 Using Julia for Numerical Experiments

Colab can be used as a computational laboratory for Julia.

For example:

```python
#Numerical experiment
%%bash
julia <<'JULIA'
x = collect(0:0.1:10)

y = x .^ 2

println("Number of points: ", length(x))
println("First value: ", y[1])
println("Last value: ", y[end])
JULIA
```

This demonstrates a typical numerical workflow:

```text
Define data
    ↓
Perform computation
    ↓
Inspect results
    ↓
Modify experiment
    ↓
Repeat
```

Later in the course, this workflow will be extended to scientific computing, automatic differentiation, differential equations, and machine learning.

---

# 0.5.20 Using Colab Hardware for Scientific Computing

The computational resources available to a Colab runtime depend on the runtime configuration and current availability.

A runtime may provide CPU resources and, where available, accelerator hardware such as GPUs.

For scientific computing, hardware acceleration can be useful for workloads such as:

* large numerical computations
* machine learning
* tensor operations
* scientific simulations
* neural networks

However, not every Julia package automatically uses an accelerator simply because one is available.

The Julia package and computational workflow must support the relevant hardware.

Therefore:

```text
Available hardware
        ≠
Automatically accelerated Julia program
```

Hardware acceleration requires an appropriate Julia ecosystem and program design.

---

# 0.5.21 Practical Considerations When Using Julia in a Cloud Notebook

There are several practical considerations when working with Julia in Colab.

### 1. Runtime lifetime

The runtime is temporary.

Do not rely on it as permanent storage.

### 2. Installation

Julia or required packages may need to be installed again when a new runtime is created.

### 3. Package environments

Use Julia environments for projects that have multiple dependencies.

### 4. Reproducibility

Keep installation, environment configuration, source code, and experiments organized.

### 5. Hardware

Check whether the current runtime provides the computational resources required by the experiment.

### 6. Large experiments

For substantial projects, keep the main source code in `.jl` files or a version-controlled repository rather than placing everything into one notebook.

### 7. Data

Important datasets and results should be stored outside the temporary runtime.

---

# 0.5.22 A Complete Julia + Colab Workflow

The complete workflow introduced in this lecture can be summarized as:

```text
Start Colab notebook
        ↓
Configure/install Julia if necessary
        ↓
Check Julia version
        ↓
Create/activate project environment
        ↓
Install required packages
        ↓
Check package status
        ↓
Run Julia code
        ↓
Run numerical experiments
        ↓
Save important results externally
```

For the practical lessons in this course, the central execution pattern is:

```python
%%bash
julia <<'JULIA'

# Julia code

JULIA
```

---

# 0.5.23 Complete Example

The following example demonstrates the workflow in one notebook cell.

```python
#Complete Julia + Colab example
%%bash
julia <<'JULIA'
using ForwardDiff

println("Julia is running inside Google Colab.")

x = [1, 2, 3, 4, 5]

println("Array:")
println(x)

println("Type:")
println(typeof(x))

println("Length:")
println(length(x))

f(x) = x^3 + 2x^2 + x

println("Derivative at x = 2:")
println(ForwardDiff.derivative(f, 2.0))
JULIA
```

This single cell demonstrates:

* Julia execution
* variables
* arrays
* type inspection
* package usage
* automatic differentiation
* numerical computation

---

# 0.5.24 Learning Objectives

After completing this lecture, you should be able to:

1. Explain what Google Colab is.
2. Understand Colab as a cloud computing environment.
3. Configure Julia inside a Colab runtime.
4. Install Julia when it is not already available.
5. Verify the Julia installation.
6. Execute Julia code from Colab.
7. Understand the `%%bash` workflow.
8. Use Julia here-documents for multi-line programs.
9. Install Julia packages using `Pkg`.
10. Inspect installed packages.
11. Understand Julia project environments.
12. Activate a Julia environment.
13. Understand the role of `Project.toml`.
14. Use Julia packages from Colab.
15. Run `.jl` Julia scripts.
16. Read and write files in the Colab filesystem.
17. Understand the temporary nature of the Colab runtime.
18. Understand the importance of persistent storage.
19. Perform numerical experiments with Julia in Colab.
20. Understand practical considerations for scientific computing in a cloud environment.

---

# 0.5.25 Key Takeaway

Google Colab can serve as a convenient cloud environment for Julia-based scientific computing.

The workflow used in this course is:

```text
Google Colab
      ↓
%%bash
      ↓
Julia
      ↓
Julia environment
      ↓
Julia packages
      ↓
Scientific computation
```

The Colab runtime provides the computational environment, while Julia provides the programming language and scientific-computing ecosystem.

With this setup understood, we can now begin learning Julia itself.

**Next: Lesson 1 — Basic Syntax and Variables**
