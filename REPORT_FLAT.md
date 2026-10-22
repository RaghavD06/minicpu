# MiniCPU Micro Project Report — Formal Language and Automata Theory

## Project title

**MiniCPU — A Formal-Language-Based Assembly Instruction Validator and CPU Simulator**

## Problem statement

An assembly program must belong to a defined instruction language before it can be executed. This project implements the lexical and syntax-validation portion using regular expressions and a deterministic finite automaton (DFA), then passes only accepted instructions to the CPU simulator.

## Objectives

1. Define a regular instruction language for the MiniCPU instruction set.
2. Tokenize source into meaningful token classes.
3. Validate syntax using instruction-specific regular expressions.
4. Validate token sequences using an explicit deterministic state machine.
5. Visualize the token sequence and DFA transition path.
6. Reject invalid instructions before CPU decoding or execution.

## FLAT implementation

### Regular expressions

`src/flat.ts` contains a pattern for every instruction class:

- `MOV R[0-7], (number|R[0-7])`
- `ADD R[0-7], R[0-7]`
- `SUB R[0-7], R[0-7]`
- `LOAD R[0-7], number`
- `STORE R[0-7], number`
- `JMP number`
- `CMP R[0-7], R[0-7]`
- `HLT`

The validator normalizes input to uppercase, applies the selected pattern, and exposes the result to the dashboard as `PASS` or `REJECTED`.

### Tokenization

The tokenizer converts input into token classes:

- `OPCODE`
- `REGISTER`
- `NUMBER`
- `COMMA`
- `INVALID`

For example:

```text
MOV R1, 10
```

becomes:

```text
[OPCODE, REGISTER, COMMA, NUMBER]
```

### Deterministic finite automaton

The DFA starts at `q0`, chooses a fixed expected token sequence based on the opcode, and consumes exactly one token at each transition. A valid path ends in `ACCEPT`; any unexpected or missing token transitions to `INVALID`.

For `MOV R1, 10`, the visible path is:

```text
q0 → q1 → q2 → q3 → q4 → ACCEPT
```

For an invalid instruction such as `MOV X1, hello`, the register and operand token classes do not match the expected transitions, so the path ends in `INVALID`.

## Validation pipeline

```text
Assembly source
    ↓
Tokenizer
    ↓
Regular-expression validation
    ↓
DFA token-sequence validation
    ↓
Instruction decoder
    ↓
CPU execution
```

The simulator checks both regex and DFA results before calling the decoder. Consequently, invalid instructions are shown in the trace but never execute.

## Test examples

Accepted:

```asm
MOV R1, 10
ADD R1, R2
SUB R1, R2
LOAD R1, 100
STORE R1, 100
JMP 5
HLT
```

Rejected:

```asm
MOV X1, 10
ADD R1
ADD R1, X
STORE R9, 100
INVALID R1, 10
```

The Invalid syntax demo in the application demonstrates this behavior directly.

## Subject mapping

| FLAT concept | Implementation |
|---|---|
| Regular Expressions | Opcode-specific syntax patterns in `src/flat.ts` |
| DFA | Deterministic token transitions and accepting/rejecting states |
| Alphabet | Token classes produced by the tokenizer |
| Acceptance | `ACCEPT` state shown in the validation pipeline |
| Rejection | `INVALID` state and explanatory error |

## How to run

```bash
npm install
npm run dev
```

Production build:

```bash
npm run build
```

## Links

- GitHub: https://github.com/RaghavD06/minicpu
- Deployed application: https://minicpi.vercel.app

## Limitations

The grammar intentionally covers a small teaching language and does not include labels, comments, symbolic expressions, or a full parser generator. This keeps the DFA and validation path visible for demonstration.
