+++
title = '理解 MIPS 汇编中的寄存器与常见操作'
date = 2024-09-30T15:05:14-07:00
+++

## 1. MIPS 汇编中的寄存器类型

MIPS 提供 32 个通用寄存器，根据用途划分为不同类别。

### 1.1 临时寄存器（$t0 - $t9）

**用途**：`$t0` 到 `$t9` 是临时寄存器，用于保存临时数据。它们的值不需要跨函数调用保留，因此在函数执行过程中可能被覆盖。

**使用场景**：

- 短期计算结果：保存中间结果或临时变量。
- 局部操作：在当前代码块中使用，函数调用后无需保留。

**示例**：

```asm
addi $t0, $0, 10  # Store 10 in $t0
```

### 1.2 保存寄存器（$s0 - $s7）

**用途**：`$s0` 到 `$s7` 是保存寄存器，用于存储需要跨函数调用保留的数据。调用方与被调用方共同负责确保这些寄存器在函数调用前后的值保持不变。

**使用场景**：适合保存全局变量或需要跨函数调用持续存在的数据。

- 长期变量存储：当变量需要在多个函数之间共享或保留时，应将其存入 `$s` 寄存器。
- 跨函数调用的数据：调用其他函数之前需要保留的数据，应放在 `$s` 寄存器中。

**示例**：

```asm
addi $s0, $zero, 100  # Store 100 in $s0
```

### 1.3 参数寄存器（$a0 - $a3）

**用途**：`$a0` 到 `$a3` 是参数寄存器，用于在函数调用时传递参数。在系统调用中，它们也用于传递参数。

**使用场景**：

- 函数调用前：将要传递给被调用函数的参数放入 `$a0` 到 `$a3`。
- 系统调用前：将所需参数，例如待打印字符串的地址或一个整数，放入 `$a0`。

**示例**：

```asm
li $a0, 5  # Pass 5 as an argument
```

### 1.4 返回值寄存器（$v0 - $v1）

**用途**：`$v0` 和 `$v1` 是返回值寄存器，用于保存函数返回值。在系统调用中，`$v0` 还用于指定系统调用编号。

**使用场景**：

- 函数返回前：将返回值放入 `$v0`；如有第二个返回值，则使用 `$v1`。
- 系统调用前：将系统调用编号放入 `$v0`。
- 系统调用后：系统调用的结果，例如读取到的值，保存在 `$v0` 中。

**示例**：

```asm
li $v0, 1      # System call 1: Print integer
move $a0, $s0  # Pass the integer to be printed in $a0
syscall        # Execute the system call
```

### 1.5 特殊用途寄存器

- **$zero（$0）**：始终保存数值 0，常用于需要零值的操作。
- **$at**：保留给汇编器使用，通常不在代码中直接使用。
- **$k0 和 $k1**：保留给操作系统，主要用于异常处理。
- **$gp**：全局指针寄存器，用于访问全局数据。
- **$sp**：栈指针，指向当前栈顶。
- **$fp**：帧指针，指向当前栈帧。
- **$ra**：返回地址寄存器，保存函数的返回地址。
- **hi 和 lo**：用于保存乘除运算结果的高位与低位。

## 2. MIPS 汇编中的常见操作

### 2.1 立即数操作

`addi` 指令将立即数与寄存器中的值相加，并将结果存入目标寄存器。

**示例**：

```asm
addi $t0, $zero, 10  # $t0 = $zero + 10
```

### 2.2 寄存器加法

`add` 指令将两个寄存器的值相加，并将结果存入目标寄存器。

**示例**：

```asm
add $s0, $t0, $t1  # $s0 = $t0 + $t1
```

### 2.3 系统调用

`syscall` 指令用于发起系统调用。系统调用编号放在 `$v0` 中，参数通过 `$a0` 到 `$a3` 传递；如果有返回值，则保存在 `$v0` 中。

**示例**：打印整数。

```asm
li $v0, 1      # System call 1: Print integer
li $a0, 42     # Store 42 in $a0
syscall        # Execute the system call, printing 42
```

### 2.4 打印整数

使用系统调用编号 `1` 打印整数，整数值通过 `$a0` 传入。

**示例**：

```asm
li $v0, 1      # System call 1: Print integer
move $a0, $s0  # Move the value of $s0 into $a0
syscall        # Execute the system call
```

### 2.5 打印字符串

使用系统调用编号 `4` 打印字符串，字符串地址通过 `$a0` 传入。

**示例**：打印空格和换行。

```asm
.data
space:    .asciiz " "    # Define a space character
newline:  .asciiz "\n"   # Define a newline character

.text
li $v0, 4          # System call 4: Print string
la $a0, space      # Load the address of the space into $a0
syscall            # Print space

li $v0, 4          # System call to print newline
la $a0, newline
syscall
```

### 2.6 打印多个整数

**示例**：打印两个整数，中间用空格分隔。

```asm
.data
space:    .asciiz " "

.text
# Assume $s0 and $s1 contain the integers to be printed
li $v0, 1          # Print the first integer
move $a0, $s0
syscall

li $v0, 4          # Print space
la $a0, space
syscall

li $v0, 1          # Print the second integer
move $a0, $s1
syscall
```
