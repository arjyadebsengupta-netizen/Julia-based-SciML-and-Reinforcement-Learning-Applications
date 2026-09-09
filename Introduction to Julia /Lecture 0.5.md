# Lecture 0.5 — How to Use Julia in Google Colab

## 0.5.1 Introduction

Google Colab provides a cloud-based notebook environment where code can be executed without setting up a local development environment.

Julia can be used in a Colab notebook by installing Julia inside the Colab environment and then invoking Julia from Colab cells.

In this course, we will use the following workflow:

```text
Google Colab notebook
        ↓
%%bash
        ↓
Julia executable
        ↓
Julia code
```

The important point is that the Colab notebook cell itself is initially interpreted by the notebook environment. We therefore explicitly invoke Julia from a Bash cell.

---

## 0.5.2 Installing Julia

The following cell downloads Julia 1.12.7, extracts it, places it in `/usr/local/julia`, and creates a symbolic link so that the `julia` command can be used directly.

```python
#Installing Julia
%%bash
wget -q https://julialang-s3.julialang.org/bin/linux/x64/1.12/julia-1.12.7-linux-x86_64.tar.gz
tar -xzf julia-1.12.7-linux-x86_64.tar.gz
mv julia-1.12.7 /usr/local/julia
ln -sf /usr/local/julia/bin/julia /usr/local/bin/julia
```

### What is happening?

* `wget` downloads the Julia archive.
* `tar -xzf` extracts the compressed archive.
* `mv` moves the Julia installation to `/usr/local/julia`.
* `ln -sf` creates a symbolic link called `julia`.
* After this, the `julia` command can be executed from the Colab environment.

---

## 0.5.3 Checking the Julia Version

After installation, we can verify that Julia is available.

```python
#Checking the version
%%bash
julia --version
```

This should display the installed Julia version.

For this course, the installation used here is Julia 1.12.7.

---

## 0.5.4 Executing Julia Code from Colab

A normal Colab cell is not automatically a Julia cell in the workflow used in this course.

Instead, we use a Bash cell and explicitly launch Julia.

For a short Julia command, we can use:

```python
%%bash
julia -e 'println("Hello from Julia")'
```

The `-e` option tells Julia to execute the expression supplied after it.

For larger pieces of Julia code, however, it is much more convenient to use a **here-document**.

The general structure is:

```python
%%bash
julia <<'JULIA'
# Julia code goes here
JULIA
```

Here:

* `%%bash` tells Colab to execute the cell as Bash.
* `julia` launches Julia.
* `<<'JULIA'` begins a here-document.
* Everything between the two `JULIA` markers is passed to Julia.
* The final `JULIA` marks the end of the Julia code.

This allows us to write ordinary Julia code in a readable multi-line form.

---

## 0.5.5 Running a Julia Program in a Colab Cell

For example:

```python
#Running Julia code
%%bash
julia <<'JULIA'
x = [1, 2, 3, 4, 5]

println(x)
println(typeof(x))
println(length(x))
JULIA
```

The Julia code is contained between the two `JULIA` markers.

This is the format we will use throughout the course when a lesson contains multiple Julia statements.

---

## 0.5.6 Installing Julia Packages

Julia's package manager is accessed through the `Pkg` module.

For example, to install `IJulia`:

```python
#Installing IJulia
%%bash
julia -e 'using Pkg; Pkg.add("IJulia")'
```

Similarly, `ForwardDiff` can be installed with:

```python
#Installing ForwardDiff
%%bash
julia -e 'using Pkg; Pkg.add("ForwardDiff")'
```

The important point is that `Pkg.add()` is Julia code, so it must be executed by Julia.

---

## 0.5.7 Checking Installed Packages

We can inspect the Julia packages installed in the current environment using `Pkg.status()`.

```python
#Checking the installed Julia packages
%%bash
julia -e 'using Pkg; Pkg.status()'
```

This displays the packages available in the current Julia environment.

---

## 0.5.8 Using a Julia Package in Colab

Once a package has been installed, it can be loaded inside Julia.

For example, `ForwardDiff` provides automatic differentiation.

The following cell computes the derivative of

$$
f(x)=x^3+2x^2+x
$$

at \(x=2\).

```python
#Computing a derivative with ForwardDiff
%%bash
julia <<'JULIA'
using ForwardDiff

f(x) = x^3 + 2x^2 + x

println(ForwardDiff.derivative(f, 2.0))
JULIA
```

The important distinction is that the code above is **not being executed as Python**.

The Colab cell is a Bash cell, Bash launches Julia, and Julia executes the code between the `JULIA` markers.

---

## 0.5.9 A Complete Example

The following example combines several ideas introduced in this lecture.

```python
#Basic Julia workflow in Google Colab
%%bash
julia <<'JULIA'
using ForwardDiff

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

This demonstrates the complete workflow:

```text
Colab cell
    ↓
%%bash
    ↓
julia
    ↓
Julia program
    ↓
Julia package
    ↓
Output
```

---

## 0.5.10 Why Use This Workflow?

Using a here-document is particularly useful for this course because it allows a complete Julia lesson to remain inside a single Colab cell.

For example, we can write:

```python
%%bash
julia <<'JULIA'
x = [10, 20, 30, 40, 50]

println(x[1])
println(x[3])
println(x[end])

x[2] = 100

println(x)
JULIA
```

This is much easier to read than placing a long Julia program inside one `julia -e '...'` expression.

Therefore, throughout the practical lessons, multi-line Julia examples will generally use:

```text
%%bash
julia <<'JULIA'
...
JULIA
```

---

## 0.5.11 Colab's Temporary Environment

The Julia installation and packages installed during a Colab session belong to the current Colab environment.

A Colab runtime is not the same thing as a permanent local computer environment. If the runtime is reset or terminated, software installed during that runtime may need to be installed again in a new session.

Therefore, installation cells are normally placed near the beginning of a notebook so that the environment can be recreated when necessary.

---

## 0.5.12 Learning Objectives

After this lecture, you should be able to:

1. Install Julia in a Google Colab environment.
2. Verify the Julia installation.
3. Understand how `%%bash` is used to invoke Julia.
4. Execute Julia using `julia -e`.
5. Execute multi-line Julia programs using a Bash here-document.
6. Install Julia packages using `Pkg.add`.
7. Inspect installed packages using `Pkg.status`.
8. Load and use Julia packages inside Colab.
9. Run Julia-based numerical and scientific-computing examples in a Colab notebook.

---

## 0.5.13 Key Takeaway

For this course, the fundamental Colab pattern is:

```python
%%bash
julia <<'JULIA'
# Julia code
JULIA
```

Colab provides the notebook environment, Bash launches Julia, and Julia executes the actual Julia program.

This workflow will be used for the practical Julia lessons that follow.
