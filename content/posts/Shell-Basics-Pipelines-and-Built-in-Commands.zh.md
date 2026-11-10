+++
title = 'Shell 基础：管道与内建命令'
date = 2024-10-28T23:55:01-07:00
+++

这篇文章是我参考[教程](https://www.joshmcguigan.com/blog/build-your-own-shell-rust/?continueFlag=ec50636fc072c968484f031a257acd3d)，用 Rust [实现 Shell](https://github.com/TriangleMesh/rust-shell) 过程中的总结与思考。

## 简介

Shell 本质上是用户与操作系统之间的交互界面。在终端输入命令后，Shell 会进行解析，将其转换为操作系统能够理解的命令或系统调用，最终通过系统调用接口与操作系统内核交互。

实现 Shell 时，核心任务是处理用户输入，并对各个部分进行相应处理。

例如，用户在终端输入：

```bash
rm -rf /path/to/directory
```

我们需要将其解析为：

- 命令：`rm`
- 选项：`-rf`
- 参数：`/path/to/directory`

这些部分都需要分别处理，才能正确执行用户的意图。

再看一个例子：

```bash
cat access.log | grep "404"
```

这个命令包含管道操作，应当被解析为两个子命令：

1. 子命令一：`cat access.log`
   - 命令：`cat`
   - 参数：`access.log`
2. 子命令二：`grep "404"`
   - 命令：`grep`
   - 参数：`"404"`

第一个子命令的输出会成为第二个子命令的输入。

### Rust 实现

在 Rust 中，可以用管道符 `|` 将用户输入的命令字符串拆成多个子命令，再按空格将每个子命令拆为命令及其参数。示例代码如下：

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

在这个结构中：

- `commands` 是按管道符拆分得到的子命令迭代器。
- `subcommand` 是当前处理的子命令字符串。
- `action` 是子命令中的命令部分。
- `args` 是子命令的参数。

## 管道

### 管道的概念

在 Shell 中，可以使用管道符 `|`，将一个命令的输出传给下一个命令作为输入。例如：

```bash
ls | grep "Cargo"
```

执行过程如下：

1. `ls` 列出当前目录中的所有文件。
2. 管道符 `|` 将 `ls` 的输出传给 `grep`。
3. `grep "Cargo"` 从输入中筛选出包含“Cargo”的行，并输出结果。

### 管道的工作原理

要在 Shell 中实现管道，需要控制子进程的标准输入（`stdin`）和标准输出（`stdout`）。具体步骤为：

1. 创建第一个子进程，将其 `stdout` 设置为管道，以便把输出传给下一个进程。
2. 对于后续的每个子进程：
   - 将其 `stdin` 设置为前一个进程的 `stdout`。
   - 如果后面还有进程，则将其 `stdout` 也设置为管道。

### Rust 实现

使用 Rust 标准库中的 `Command` 和 `Stdio`，可以方便地实现管道操作。下面演示如何连接两个命令：

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

### 实现要点

1. **配置子进程的输入输出**：
   - 使用 `stdout(Stdio::piped())`，将子进程的标准输出重定向到管道。
   - 对于需要读取上一个命令输出的子进程，使用 `stdin(Stdio::from(previous_stdout))`。
2. **管理子进程生命周期**：
   - 使用 `.spawn()` 启动子进程。
   - 对于需要获取输出的子进程，使用 `.wait_with_output()` 等待其结束并取得输出。
3. **错误处理**：
   - 在各个步骤中使用 `expect` 或 `if let Err(e) = ...` 处理可能出现的错误，提高程序的健壮性。

## 处理内建命令

### 为什么需要内建命令

某些命令（例如 `cd`）需要修改 Shell 进程自身的状态。`cd` 会改变当前工作目录；如果把它实现为外部命令，也就是子进程，它只能改变子进程的工作目录，无法影响父进程，也就是 Shell。

因此，这类内建命令需要直接在 Shell 中实现，才能修改 Shell 进程的状态。

### 代码示例

处理用户输入时，可以用 `match` 语句对内建命令进行特殊处理。例如：

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

在这个例子中：

- `env::set_current_dir` 用于改变当前工作目录。
- 如果修改失败，会输出错误信息。
- 执行内建命令后，将 `previous_command` 设置为 `None`，表示不需要继续处理管道。
