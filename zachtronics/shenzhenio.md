# Shenzhen I/O

## Microcontroller Pin Interfaces

### Simple I/O

Continuous signal from 0 to 100. Unmarked. Can read/write data at any time with no regard to state of connected devices.

### XBus

Discrete data packets from -999 to 999. Pins marked with yellow dot. Synchronous protocol that is only transferred when a reader is attempting to read, and a writer is attempting to write (block occurs otherwise).

### Sleep

CPUs are faster than the signals they read/write. They can execute multiple instructions in a single **time unit**. Use `slp (x)`, to advance to the next time unit where x is the number of time units.

Ex.

```
mov 100 p1
slp 3
mov 0 p1
slp 3
# outputs 100 to p1 for 3 units, then 0 to p1 for 3 units resulting in a square wave pattern
```

## Language Reference

Microcontroller programming languages have the following syntax structure:

> LABEL CONDITION INSTRUCTION COMMENT

Not all of these components are required, but the specific order is.

### Comments

`# text here` Writes a comment

### Labels

`foo:` Creates a label for jump instructions

### Conditional Execution

`+` and `-` prefixes enable and disable instructions based on test instruction outcomes.

### Registers

Registers are sources and destinations of data manipulation. The microcontroller has the following register types:

`acc` is the primary, general purpose data register, capable of having arithmetic instructions applied to it.

`dat` is a secondary register available in certain microcontroller models.

`p0`, `p1`, `x0`, `x1`, `x2`, and `x3` are pin registers used to read and write values in order to communicate with other connected, compatible devices. Pins are either simple I/O or XBus interfaces.

`null` is a pseudo-register where when read from produces a value of 0, and does nothing when written to.

### Instructions

Instructions can be applied to registers (R), integers (I), registers or integers (R/I), pin registers (P), or labels (L).

#### Basic Instructions

`nop` No effect

`mov R/I R` Moves a source register or integer to a destination register

`jmp L` Jumps to label

`slp R/I` Sleeps for the number of time units specified by integer or register

`slx P` Sleep until data is available to be read on specified XBus pin

#### Arithmetic

`add R/I` Addition

`sub R/I` Subtraction

`mul R/I` Multiplication

`not` If `acc == 0` store `acc = 100` else `acc = 0`

`dgt R/I` Isolate the specified digit of the value in the `acc` register and store in `acc`

Examples:

`acc = 596` and `dgt 0` sets `acc = 6`

`acc = 596` and `dgt 1` sets `acc = 9`

`acc = 596` and `dgt 2` sets `acc = 5`

`dst R/I R/I` Set the digit of the `acc` specified by the first operand to the value of the second operand

Examples:

`acc = 596` and `dst 0 7` sets `acc =597`

`acc = 596` and `dst 1 7` sets `acc = 576`

`acc = 596` and `dst 2 7` sets `acc = 796`

#### Test Instructions

`teq R/I E/I` tests if first and second parameter are equal

`tgt R/I R/I` tests if first parameter is greater than second

`tlt R/I R/I` tests if first parameter is less than second

`tcp R/I R/I` compares the value of parameters. Enables `+` and disables `-` when first argument is greater, disables both if equal, and disables `+` and enables `-` when first argument is less than.
