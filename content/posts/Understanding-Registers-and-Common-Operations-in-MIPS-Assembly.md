+++
title = 'Understanding Registers and Common Operations in MIPS Assembly'
date = 2024-09-30T15:05:14-07:00
+++

## 1. Types of Registers in MIPS Assembly

MIPS provides 32 general-purpose registers that are divided into different categories based on their use.

### 1.1 Temporary Registers ($t0 - $t9)

**Purpose**: `$t0` to `$t9` are temporary registers used to store temporary data. Their values do not need to be preserved across function calls, meaning they may be overwritten during the execution of a function.

**Use case**:  
• Short-term computation results: Used to store intermediate results or temporary variables.  
• Local operations: Used within the current code block, with no need to preserve their values after a function call.


**Example**:
```asm
addi $t0, $0, 10  # Store 10 in $t0
```

### 1.2 Saved Registers ($s0 - $s7)

**Purpose**: `$s0` to `$s7` are saved registers, used to store data that must be preserved across function calls. Both the caller and callee are responsible for ensuring that the values in these registers remain unchanged before and after a function call.

**Use case**: Suitable for storing global variables or data that needs to persist across function calls.  
• Long-term variable storage: When a variable needs to be shared or retained across multiple functions, it should be stored in an `$s` register.  
• Data across function calls: Any data that must be preserved before calling other functions should be placed in an `$s` register.

**Example**:
```asm
addi $s0, $zero, 100  # Store 100 in $s0
```

### 1.3 Argument Registers ($a0 - $a3)

**Purpose**: `$a0` to `$a3` are argument registers, used to pass parameters during function calls. In system calls, these registers are also used to pass arguments.

**Use case**:  
• Before function calls: Store the arguments to be passed to the called function in `$a0` to `$a3`.  
• Before system calls: Store the parameters to be passed (such as the address of a string to print or an integer) in `$a0`.

**Example**:
```asm
li $a0, 5  # Pass 5 as an argument
```

### 1.4 Return Value Registers ($v0 - $v1)

**Purpose**: `$v0` and `$v1` are return value registers, used to store function return values. In system calls, `$v0` is also used to specify the system call number.

**Use case**:  
• Before returning from a function: Store the return value in `$v0` (if there is a second return value, use `$v1`).  
• Before a system call: Store the system call number in `$v0`.  
• After a system call: The result of the system call (such as a value read) is stored in `$v0`.

**Example**:
```asm
li $v0, 1      # System call 1: Print integer
move $a0, $s0  # Pass the integer to be printed in $a0
syscall        # Execute the system call
```

### 1.5 Special Purpose Registers

- **$zero ($0)**: Constantly holds the value 0 and is commonly used in operations requiring a value of 0.
- **$at**: Reserved for the assembler and not generally used in direct coding.
- **$k0 and $k1**: Reserved for the operating system, primarily used for exception handling.
- **$gp**: Global pointer register, used to access global data.
- **$sp**: Stack pointer, points to the current top of the stack.
- **$fp**: Frame pointer, points to the current stack frame.
- **$ra**: Return address register, holds the return address for functions.
- **hi and lo**: Used to store the high and low results of multiplication and division operations.

## 2. Common Operations in MIPS Assembly

### 2.1 Immediate Operations

The `addi` instruction is used to add an immediate value to a register, with the result stored in the destination register.

**Example**:
```asm
addi $t0, $zero, 10  # $t0 = $zero + 10
```

### 2.2 Register Addition

The `add` instruction is used to add the values of two registers and store the result in a destination register.

**Example**:
```asm
add $s0, $t0, $t1  # $s0 = $t0 + $t1
```

### 2.3 System Call Operations

The `syscall` instruction is used to make system calls. The system call number is specified in `$v0`, with arguments passed through `$a0` to `$a3`. The return value, if any, is stored in `$v0`.

**Example**: Print an integer
```asm
li $v0, 1      # System call 1: Print integer
li $a0, 42     # Store 42 in $a0
syscall        # Execute the system call, printing 42
```

### 2.4 Printing an Integer

Use system call number `1` to print an integer. The integer value is passed via `$a0`.

**Example**:
```asm
li $v0, 1      # System call 1: Print integer
move $a0, $s0  # Move the value of $s0 into $a0
syscall        # Execute the system call
```

### 2.5 Printing a String

Use system call number `4` to print a string. The address of the string is passed via `$a0`.

**Example**: Print a space and a newline
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

### 2.6 Printing Multiple Integers

**Example**: Print two integers with a space between them
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
