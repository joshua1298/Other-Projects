# 8-Bit ALU (Logisim Implementation)

This project implements an 8-bit Arithmetic Logic Unit (ALU) using Logisim. The ALU performs arithmetic, logic, shift, and comparison operations and produces condition flags as side effects.

---

## Overview
### Inputs
- X (8-bits)
- Y (8-bits)
- Opcode (3-bits)
### Outputs
- Result (8-bits)
- Flags (5-bits)

All operations are selected using a multiplexer controlled by the opcode.

---

## Supported Operations

| Opcode | Operation |
|--------|----------|
| 000 | X OR Y |
| 001 | X AND Y |
| 010 | X + Y |
| 011 | X - Y (X + ~Y + 1) |
| 100 | Shift Left |
| 101 | Logical Shift Right |
| 110 | Arithmetic Shift Right |
| 111 | X < Y |

For shift operations, only the lowest 3 bits of Y are used as the shift amount.

---

## Implementation Details

### Addition
Implemented using the built-in Logisim Adder and the carry in is always set to zero

### Subtraction
Implemented using two’s complement so X - Y is actually calculated using adders as: X + (-Y + 1). The new equation inverts Y and sets the carry in to 1 

### Shifts
Implemented using Logisim’s built-in shifters. The 3 lowest bits of Y are used as the shift amount and X is the number that gets shifted.

### Comparison (X < Y)
Implemented using a comparator where output is 00000001 if X < Y and 00000000 otherwise

---

## Flags (Condition Codes)

The ALU outputs 5 flags:

| Bit | Flag | Meaning |
|-----|------|--------|
| 0 | EQ | X == Y |
| 1 | OF | Signed overflow |
| 2 | CF | Unsigned overflow / borrow |
| 3 | ZF | Result == 0 |
| 4 | SF | Sign bit of result |

---

## Flag Rules

### SF (Sign Flag)
SF is always the most significant bit of the final result:
- SF = Result[7]

---

### ZF (Zero Flag)
ZF is set when:
- Result == 00000000

---

### CF (Carry Flag)
CF is only valid for addition and subtraction:

- Addition: CF = CarryOut
- Subtraction: CF = NOT(CarryOut)

For all other operations:
- CF = 0

---

### OF (Overflow Flag)
OF is only valid for addition and subtraction:

- Addition: OF is set when signed overflow occurs
- Subtraction: OF is set when signed overflow occurs

For all other operations:
- OF = 0

Signed overflow occurs when the result cannot be represented in 8-bit two’s complement form.

---

### EQ (Equal Flag)
EQ is set when:
- X == Y

This flag is independent of the opcode.

---

## Design Notes

- Built using modular Logisim components (Adder, Shifter, Comparator, Multiplexer)
- Final result is selected using an 8-input multiplexer controlled by opcode
- Flags are computed from the final selected result, not intermediate computations
- Designed to simulate a simplified CPU ALU datapath
