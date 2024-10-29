+++
title = 'Shell Basics Pipelines and Built in Commands'
date = 2024-10-28T23:55:01-07:00
+++

This blog post is a summary and reflection based on my [practice](https://github.com/TriangleMesh/rust-shell) implementing a shell in Rust following a [tutorial](https://www.joshmcguigan.com/blog/build-your-own-shell-rust/?continueFlag=ec50636fc072c968484f031a257acd3d). 

## Introduction

A shell is essentially an interface that facilitates interaction between the user and the operating system. When you input a command in the terminal, the shell parses it, translates it into commands or system calls that the operating system can understand, and finally interacts with the OS kernel through system call interfaces.

When implementing a shell, the core task is to process the user’s input and handle each part accordingly.

When a user inputs a command in the terminal, for example:

```bash
rm -rf /path/to/directory
```

We need to parse it into:

* Command: `rm`
* Option: `-rf`
* Argument: `/path/to/directory`

Each of these parts needs to be handled separately to execute the user’s intent correctly.

Another example:

```bash
cat access.log | grep "404"
```

This command involves a pipeline operation and should be parsed into two subcommands:

1. Subcommand 1: `cat access.log`
   - Command: `cat`
   - Argument: `access.log`

2. Subcommand 2: `grep "404"`
   - Command: `grep`
   - Argument: `"404"`

Here, the output of the first subcommand serves as the input to the second subcommand.

### Implementation in Rust

In Rust, you can split the user’s input command string by the pipe symbol `|` into multiple subcommands. Then, split each subcommand by spaces into the command and its arguments. Here’s a sample code:

```rust
let mut commands = input.trim().split('|').peekable();
while let Some(subcommand) = commands.next() {
    let mut parts = subcommand.trim().split_whitespace();
    let action = parts.next().unwrap();
    let args = parts;
    match action {
        // Handle different commands here
        _ => { /* ... */ }
    }
}
```
In this structure:

* `commands` is an iterator of `subcommands` split by the pipe symbol.
* `subcommand` is the current `subcommand` string being processed.
* `action` is the command part of the `subcommand`.
* `args` are the arguments of the `subcommand`.

## Pipelines

### Concept of Pipelines

In a shell, you can use the pipe symbol | to pass the output of one command as the input to the next command. For example:

```bash
ls | grep "Cargo"
```

The execution process of this command is:

1. `ls` lists all files in the current directory.
2. The pipe `|` passes the output of `ls` to the `grep` command.
3. `grep "Cargo"` filters lines containing "Cargo" from the input and outputs the result.

### How Pipelines Work

To implement pipeline functionality in a shell, you need to control the standard input (`stdin`) and standard output (`stdout`) of child processes. The specific steps are:

1. Create the first child process and set its `stdout` to a pipe so that its output can be passed to the next process.
2. For each subsequent child process:
   - Set its `stdin` to the `stdout` of the previous process.
   - If there is a next process, set its `stdout` to a pipe.

### Implementation in Rust

Using Rust’s standard library `Command` and `Stdio`, we can conveniently implement pipeline operations. Below is an example demonstrating how to connect two commands in Rust:

```rust
use std::process::{Command, Stdio};

fn main() {
    // Create the first command, setting stdout to a pipe
    let ls = Command::new("ls")
        .stdout(Stdio::piped())
        .spawn()
        .expect("Failed to execute ls");

    // Get the stdout of the first command
    let ls_stdout = ls.stdout.expect("Failed to capture ls stdout");

    // Create the second command, using the stdout of the first command as stdin
    let grep = Command::new("grep")
        .arg("Cargo")
        .stdin(Stdio::from(ls_stdout))
        .stdout(Stdio::piped())
        .spawn()
        .expect("Failed to execute grep");

    // Get the output of the second command
    let output = grep
        .wait_with_output()
        .expect("Failed to wait on grep");

    // Output the result
    println!("{}", String::from_utf8_lossy(&output.stdout));
}
```

### Key Implementation Points

1. **Configure the input and output of child processes:**
   - Use `stdout(Stdio::piped())` to redirect the standard output of a child process to a pipe.
   - For child processes that need to read input from the previous command, use `stdin(Stdio::from(previous_stdout))`.

2. **Manage the lifecycle of child processes:**
   - Use `.spawn()` to start a child process.
   - For child processes that need to capture output, use `.wait_with_output()` to wait for the process to finish and get the output.

3. **Error Handling:**
   - Use `expect` or `if let Err(e) = ...` at each step to handle potential errors and ensure program robustness.

## Handling Built-in Commands

### Why Built-in Commands Are Needed

Certain commands (like `cd`) need to modify the state of the shell process itself. For example, the `cd` command changes the current working directory. If `cd` is implemented as an external command (child process), it can only change the working directory of the child process and will not affect the parent process (the shell).

Therefore, these built-in commands need to be implemented directly within the shell to modify the shell process’s state.

### Code Implementation Example

When processing user input commands, you can use a `match` statement to handle built-in commands specially. For example:

```rust
use std::env;
use std::path::Path;

match action {
    "cd" => {
        // Get the target directory, defaulting to the home directory
        let new_dir = args.peekable().peek().map_or("~", |x| *x);
        let root = Path::new(new_dir);
        if let Err(e) = env::set_current_dir(&root) {
            eprintln!("cd: {}", e);
        }
        // After executing a built-in command, there's no need to handle subsequent pipelines
        previous_command = None;
    },
    // Handle other commands
    _ => { /* ... */ }
}
```
In this example:

- `env::set_current_dir` is used to change the current working directory.
- If the change fails, an error message is output.
- After executing the built-in command, `previous_command` is set to `None`, indicating that there’s no need to handle pipelines.