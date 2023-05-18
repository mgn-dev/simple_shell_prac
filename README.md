# Simple Shell Practice

A practice workspace for building toward the ALX Simple Shell project — a custom Unix command interpreter written in C.

## Overview

This project serves as a preparation and practice area for the Simple Shell capstone project in the ALX Software Engineering curriculum. Before implementing a full-featured shell with process forking, PATH resolution, built-in commands, and signal handling, learners use this space to experiment with shell concepts in isolation: reading input lines, tokenizing commands, executing programs with `execve`, managing child processes with `wait`, handling exit status codes, and implementing basic built-ins like `env` and `cd`.

The practice workspace is intentionally minimal — a blank canvas for iterative practice. The skills consolidated here directly map to the Simple Shell project specification and checker requirements.

## Skills covered


- Read user input in a loop with a custom prompt display
- Tokenize input strings into an argument vector (`argv`) for `execve`
- Fork child processes and execute external commands with proper error handling
- Wait for child processes and capture exit status with `wait`/`waitpid`
- Resolve commands using the `PATH` environment variable
- Build built-in commands that do not fork (`exit`, `env`)
- Handle the `EOF` condition (Ctrl+D) gracefully
- Manage memory for dynamically allocated argument arrays across iterations
- Understand signal behavior in interactive shells (SIGINT handling in the full project)

## Tech Stack

| Category | Technologies |
|----------|-------------|
| Language | C (C99, `gnu89` standard) |
| Compiler | GCC |
| System Calls | `fork`, `execve`, `wait`, `waitpid`, `access`, `stat` |
| Headers | `unistd.h`, `sys/wait.h`, `stdlib.h`, `string.h` |
| Runtime | Ubuntu 20.04+ |

## Project Structure

| Component | Description |
|-----------|-------------|
| `README.md` | Practice task overview and curriculum placement |
| Practice scripts | Learner-created C files for incremental shell features |
| Test commands | Manual verification against `/bin/ls`, `/bin/echo`, etc. |

As practice progresses, a typical layout emerges:

| File | Purpose |
|------|---------|
| `shell.h` | Struct definitions, function prototypes, includes |
| `main.c` | Entry point, input loop, prompt display |
| `read_line.c` | Read and return a line of input (with or without `getline`) |
| `split_line.c` | Tokenize input into `argv` array |
| `execute.c` | Fork and exec external commands |
| `path_resolver.c` | Search PATH directories for executable |
| `builtins.c` | Handle `exit` and `env` without forking |

## Key Implementations

Practice tasks build toward these core Simple Shell features:

- **Input loop** — Display `($)` or custom prompt, read a line, handle empty input and whitespace-only lines
- **Command parsing** — Split on spaces/tabs, handle multiple arguments, build a NULL-terminated `argv`
- **Process execution** — `fork()` in the parent, `execve()` in the child with the resolved program path and argument array; parent calls `wait()` to prevent zombies
- **PATH resolution** — Tokenize `$PATH` on `:`, append command name to each directory, check existence with `access` or `stat`, execute on first match
- **Built-in commands** — `exit [status]` terminates the shell with optional status code; `env` prints the environment without forking
- **Error handling** — Print meaningful errors for command not found, permission denied, and fork failures; free all allocated memory before exit
- **Memory management** — Free token arrays and input buffers after each command iteration; avoid leaks across the REPL loop

## Getting Started

### Prerequisites

- Ubuntu 20.04+
- GCC
- Git
- Completion of `alx-low_level_programming` through process-adjacent modules (pointers, strings, malloc, file I/O)

### Setup

```bash
git clone <repository-url>
cd simple_shell_prac
```

### Suggested Practice Progression

**Step 1 — Prompt and read:**

```c
/* Print prompt, read a line, echo it back */
write(1, "#cisfun$ ", 9);
/* use read() or getline() */
```

**Step 2 — Execute a hardcoded command:**

```bash
gcc -Wall -Werror -Wextra -pedantic -std=gnu89 \
  main.c execute.c -o shell
echo "/bin/ls" | ./shell
```

**Step 3 — Tokenize and exec with argv:**

```bash
echo "ls -la /tmp" | ./shell
```

**Step 4 — Add PATH resolution:**

```bash
echo "ls" | ./shell    # resolves via PATH
echo "nonexistent" | ./shell  # prints error
```

**Step 5 — Built-ins:**

```bash
echo "env" | ./shell
echo "exit 3" | ./shell; echo $?   # should print 3
```

### Verification

Test against known system behavior:

```bash
echo "/bin/ls -1" | ./shell
echo "/bin/echo hello" | ./shell
echo "exit" | ./shell
```

Compare output and exit codes with the equivalent Bash commands.

## Curriculum Context

Simple Shell Practice prepares learners for the **Simple Shell** capstone project — a mandatory checkpoint in the ALX low-level project that validates process management, system call usage, and C project architecture skills.

| Previous | Next |
|----------|------|
| `binary_trees` and `alx-low_level_programming` (`0x1E-search_algorithms`) | Simple Shell (full capstone project) |

Related work:

- `alx-low_level_programming` — C foundation: pointers, strings, malloc, and function pointers used throughout shell implementation
- `monty` — interpreter loop pattern (read → parse → dispatch) analogous to the shell REPL
- `printf` — formatted output for error messages and prompt display
- `alx-higher_level_programming` — follows after Simple Shell; shifts to Python and web development

The Simple Shell project is a prerequisite for continuing in the ALX Software Engineering program. Mastery of `fork`/`execve`/`wait` here transfers directly to understanding how servers spawn worker processes and how containers wrap process isolation.
