# Unasm spec
as implemented in libunasm; extends [Unasm](https://esolangs.org/wiki/Unasm)

Instructions are seperated by `;`s or newlines. Interpereters/Compilers **must** support `\r\n` (CRLF) aswell as `\n` (LF)

## Baseline

### Per-Register

Note, you can swap r1 & r2 for the instruction to apply the operation to the other register.<br/>
As an example, `r2 0` is equivalent to `r1 0` but for register 2

| Instruction | Description                                           |
|-------------|-------------------------------------------------------|
| r1          | Sets Register 1 to the inputted argument              |
| r1+         | Increments Register 1 by 1                            |
| r1-         | Decrements Register 1 by 1                            |
| r1*         | Multiplies register 1 by register 2                   |
| r1/         | Divides register 1 by register 2                      |
| r1=         | Sets Register 1 to 0; equivalent to `r1 0`            |
| r1#         | Sets Register 1 to a random integer between 0 and 255 |
| r1r2        | Sets Register 1 to the value of Register 2            |
| swap        | Swaps both registers                                  |

### Jmp

| Instruction | Description                                              |
|-------------|----------------------------------------------------------|
| lbl         | Sets the label (arg 1)'s name to the current instruction |
| jmp         | Jumps to the specified label                             |
| rjmp        | Jumps to a random instruction                            |

### Quine

| Instruction | Description                                                 |
|-------------|-------------------------------------------------------------|
| src         | Prints the interpereter's source, not available on libunasm |

### Output

| Instruction | Description                                              |
|-------------|----------------------------------------------------------|
| out         | Outputs Register 1's value                               |
| outc        | Outputs Register 1's value as an ascii or utf8 character |

## libunasm
### Quine

| Instruction | Description                                 |
|-------------|---------------------------------------------|
| q/quine     | Prints the current running program's source |

### Jmp

| Instruction | Description                             |
|-------------|-----------------------------------------|
| ljmp        | Jumps to the specified instruction/line |