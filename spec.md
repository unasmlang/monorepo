# Unasm spec
as implemented in libunasm; extends [Unasm](https://esolangs.org/wiki/Unasm)

Instructions are seperated by `;`s or newlines. Interpereters/Compilers **must** support `\r\n` (CRLF) aswell as `\n` (LF)

## Baseline

### Per-Register

Note, you can swap r1 & r2 for the instruction to apply the operation to the other register.<br/>
As an example, `r2 0` is equivalent to `r1 0` but for register 2

Note: Registers can be non-numbers, although this is discouraged.<br/>
Why is this? idk, normalcat decided that.

| Instruction | Description                                                                                                        |
|-------------|--------------------------------------------------------------------------------------------------------------------|
| r1          | Sets Register 1 to the inputted argument                                                                           |
| r1+         | Increments Register 1 by 1                                                                                         |
| r1-         | Decrements Register 1 by 1                                                                                         |
| r1*         | Multiplies register 1 by register 2                                                                                |
| r1/         | Divides register 1 by register 2                                                                                   |
| r1=         | Sets Register 1 to 0; equivalent to `r1 0`                                                                         |
| r1#         | Sets Register 1 to a random integer between 0 and 255                                                              |
| r1r2        | Sets Register 1 to the value of Register 2                                                                         |
| swap        | Swaps both registers                                                                                               |
| cmp(1)      | Runs the next line only if register 1 is larger than register 2, otherwise it will skip the next line and move on. |

*(1) For non-ints, this is treatead as a nop*

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
| nl          | Outputs a newline, equivalent to `r1 10; outc`           |

### Unsafe

The below are unsafe and should NOT be enabled when running untrusted code.

| Instruction | Description                               | How to enable in libunasm                               |
|-------------|-------------------------------------------|---------------------------------------------------------|
| jseval(1)   | Evaluates the arguments passed as JS      | Lang-Specific; [See Here](/ts/libunasm/#js-eval) for TS |
| get         | Reads the file at args[0] & puts it in r1 | Lang-Specific; [See Here](/ts/libunasm/#fs-read) for TS |

*(1) Some implementations may use `eval` for this, although this should refer to evaluating further unasm code in the same environment.*

### Variables

| Instruction                        | Description                                                  | Note                             |
|------------------------------------|--------------------------------------------------------------|----------------------------------|
| saveVars &lt;name&gt;              | Saves r1 & r2 to an in-memory slot `<name>`                  |                                  |
| loadVars &lt;name&gt;              | Loads r1 & r2 from an in-memory slot `<name>`                |                                  |
| s &lt;name&gt; &lt;value&gt; *(1)* | Sets variable `<name>` to `value` (value can include spaces) | `libunasm.variables` in libunasm |
| g &lt;name&gt;                     | Gets `<name>` & puts it in r1                                |                                  |

*Note: Variables set using `s/g` can also be accessed in other instructions using `%variable%`, as per spec. Some implementations may ignore this.*

*(1) If no value is provided, sets it to r1 - Attempts to convert to number, if conversion fails, uses a string*

## libunasm
Below, you can find instructions specifically found in libunasm

Note that these apply to all first-party release implementations of libunasm

### Quine

| Instruction | Description                                 |
|-------------|---------------------------------------------|
| q/quine     | Prints the current running program's source |

### Jmp

| Instruction | Description                             |
|-------------|-----------------------------------------|
| ljmp        | Jumps to the specified instruction/line |

## Notes

Any unknown or disabled instructions should be treated as a nop by the interpereter.
