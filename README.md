# Python Bootcamp for R Users

A self-study crash course notebook for data scientists transitioning from R to Python, based on Chapter 2 of *Data Science from Scratch*.

## Overview

This repository contains an interactive Jupyter Notebook (`Pythion Bootcamp for R Users.ipynb`) that walks through core Python concepts with explicit comparisons to R, making the transition as smooth as possible.

## Topics Covered

### Formatting & Syntax
- Whitespace rules (Python uses indentation instead of `{}` for code blocks)
- `print()` vs. implicit shell output
- Suppressing output with `;`
- Nested loops and string casting with `str()`

### Modules
- Four import patterns: `import`, aliased import (`import x as y`), selective import (`from x import y`), and wildcard (and why to avoid it)
- Working examples using `re` and `matplotlib`

### Functions
- Defining functions with `def` and documenting with docstrings
- Local vs. global variables
- Nested function calls
- Passing functions as arguments
- Lambda functions (`lambda x: ...`)
- Default argument values

### Strings
- Single vs. double quotes
- Escape characters (`\t`, `\n`, `\\`, `\"`)
- Raw strings (`r"..."`)
- f-strings for interpolation (`f"{var}"`)
- `.format()` method
- Multi-line block quotes with triple quotes
- Exception handling with `try` / `except`

### Lists
- Creation, `len()`, `sum()`
- **0-based indexing** (vs. 1-based in R)
- Slicing with `[start:stop:stride]` — stop is exclusive
- Negative indexing to count from the end
- Slicing strings with the same syntax
- Membership testing with `in`
- `extend()` and `append()` (in-place modification)
- List unpacking and throwaway variables (`_`)

### Tuples
- Immutable lists created with `()` or bare comma syntax
- Returning multiple values from functions
- Multiple assignment and variable swapping

### Dictionaries
- Key/value pairs (analogous to JSON or named lists in R)
- Safe access with `.get()` and default values
- Adding and updating keys on assignment
- Querying `.keys()`, `.values()`, `.items()`
- List comprehensions: `[expr for item in iterable if condition]`
- Pythonic membership testing (`"key" in dict`)
- Default dictionaries (`defaultdict`)

## Getting Started

### Prerequisites

- Python 3.x
- Jupyter Notebook or JupyterLab (or VS Code with the Jupyter extension)

### Running the Notebook

```bash
jupyter notebook "Pythion Bootcamp for R Users.ipynb"
```

Or open the file directly in VS Code.

### Useful Jupyter Keyboard Shortcuts (macOS)

| Shortcut | Action |
|---|---|
| `Ctrl + Enter` | Run current cell |
| `Shift + Enter` | Run cell and move to next |
| `Alt + Enter` | Run cell and insert below |
| `Cmd + Enter` | Insert cell below |
| `Cmd + Shift + Enter` | Insert cell above |
| `Enter` | Edit cell |
| `Esc` | Exit edit mode |
| `D`, `D` | Delete selected cell |
| `A` / `B` | Insert cell above / below |
| `Y` / `M` / `R` | Set cell to code / markdown / raw |

## Source Material

- *Data Science from Scratch* by Joel Grus — Chapter 2 (Python Crash Course)
