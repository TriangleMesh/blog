+++
title = 'MIPS Instruction Set and Machine Code Generation'
date = 2024-09-29T16:07:04-07:00
+++

### Overview of the MIPS Instruction Set

The MIPS instruction set consists of **32-bit fixed-length instructions**, divided into several types, each with a specific field format.

### Main Instruction Types:
- **R-type Instructions**: Used for operations between registers.
  - `opcode | rs | rt | rd | shamt | funct`
  
- **I-type Instructions**: Used for operations between registers and immediate values or for memory access.
  - `opcode | rs | rt | immediate`

- **J-type Instructions**: Used for jump operations.
  - `opcode | address`

- **Branch Instructions**: A subtype of I-type instructions, used for conditional jumps.
  - `opcode | rs | rt | offset`

### R-type Instructions

**R-type instructions** are used for operations between **registers**, where both operands and the result are stored in registers. They are commonly used for arithmetic and logical operations.

#### Format:
```
| opcode (6 bits) | rs (5 bits) | rt (5 bits) | rd (5 bits) | shamt (5 bits) | funct (6 bits) |
```
- **opcode**: The operation code, fixed as `000000`.
- **rs**: Source register 1.
- **rt**: Source register 2.
- **rd**: Destination register.
- **shamt**: Shift amount, set to `00000` for non-shift operations.
- **funct**: Function code, specifying the exact operation.

#### Example: `add $s0, $t0, $t1`
This instruction adds `$t0` and `$t1`, storing the result in `$s0`.

- **opcode**: `000000`
- **rs**: `$t0` (`01000`)
- **rt**: `$t1` (`01001`)
- **rd**: `$s0` (`10000`)
- **shamt**: `00000`
- **funct**: `100000` (Addition)

**Machine code:**
```
000000 01000 01001 10000 00000 100000
```
**Hexadecimal representation:** `0x01098020`

### I-type Instructions

**I-type instructions** are used for operations between **registers** and **immediate values**, or for **loading and storing** memory operations. These instructions contain a 16-bit immediate value or address offset.

#### Format:
```
| opcode (6 bits) | rs (5 bits) | rt (5 bits) | immediate (16 bits) |
```
- **opcode**: The operation code, specifying the instruction type.
- **rs**: Source register or base register.
- **rt**: Destination register.
- **immediate**: Immediate value or address offset.

#### Example 1: `addi $t0, $0, 10`
This instruction adds the immediate value `10` to `$0` (which is always 0), storing the result in `$t0`.

- **opcode**: `001000` (`addi`)
- **rs**: `$0` (`00000`)
- **rt**: `$t0` (`01000`)
- **immediate**: `0000000000001010` (binary for 10)

**Machine code:**
```
001000 00000 01000 0000000000001010
```
**Hexadecimal representation:** `0x2008000A`

**Note:** While the assembly format for `addi $t0, $0, 10` places the destination register `$t0` before the source register `$0`, the machine code is generated in the order `opcode | rs | rt | immediate`, where the source register (`rs`) comes before the destination register (`rt`). This design difference is based on architectural considerations.

#### Example 2: `lw $t0, 4($t1)`
This instruction loads a word from the address `$t1 + 4` into `$t0`.

- **opcode**: `100011` (`lw`)
- **rs**: `$t1` (`01001`)
- **rt**: `$t0` (`01000`)
- **immediate**: `0000000000000100` (binary for 4)

**Machine code:**
```
100011 01001 01000 0000000000000100
```
**Hexadecimal representation:** `0x8D280004`

### J-type Instructions

**J-type instructions** are used for unconditional jumps to a specified address.

#### Format:
```
| opcode (6 bits) | address (26 bits) |
```
- **opcode**: The operation code, indicating a jump instruction.
- **address**: The target address (excluding the upper 4 bits and lower 2 bits).

#### Example: `j 0x00400000`
This instruction jumps unconditionally to the address `0x00400000`.

- **opcode**: `000010` (`j`)
- **address**: `00000000010000000000000000`

**Machine code:**
```
000010 00000000010000000000000000
```
**Hexadecimal representation:** `0x08000000`

### Branch Instructions

**Branch instructions** are used for conditional jumps and are commonly found in control flow for conditions and loops.

#### Format:
```
| opcode (6 bits) | rs (5 bits) | rt (5 bits) | offset (16 bits) |
```
- **opcode**: The operation code, indicating the branch type (e.g., `beq`, `bne`).
- **rs**: Source register 1.
- **rt**: Source register 2.
- **offset**: The offset, relative to the next instruction's address.

#### Example: `beq $t0, $t1, label`
If `$t0` equals `$t1`, this instruction branches to `label`.

- **opcode**: `000100` (`beq`)
- **rs**: `$t0` (`01000`)
- **rt**: `$t1` (`01001`)
- **offset**: `0000000000000010` (assuming an offset of 2)

**Machine code:**
```
000100 01000 01001 0000000000000010
```
**Hexadecimal representation:** `0x11090002`