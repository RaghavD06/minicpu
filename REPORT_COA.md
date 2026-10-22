# MiniCPU Micro Project Report — Computer Organization and Architecture

## Project title

**MiniCPU — A Formal-Language-Based Assembly Instruction Validator and CPU Simulator**

## Problem statement

Real processors translate assembly instructions into operations on registers and memory through a defined instruction cycle. This project implements a small educational CPU so that instruction formats, addressing modes, and the fetch–decode–execute cycle can be observed interactively in a browser.

## Objectives

1. Define and decode a small assembly instruction set.
2. Demonstrate register, immediate, and direct-memory addressing.
3. Simulate the program counter, instruction register, registers, flags, and memory.
4. Display every instruction's FETCH, DECODE, and EXECUTE phases.
5. Provide a safe, understandable CPU model suitable for classroom demonstration.

## COA implementation

### Instruction formats

The decoder in `src/flat.ts` converts accepted source into a structured instruction:

- `REGISTER_IMMEDIATE`: `MOV R1, 10`
- `REGISTER_REGISTER`: `ADD R1, R2` and `SUB R1, R2`
- `REGISTER_ADDRESS`: `LOAD R1, 100` and `STORE R1, 100`
- `ADDRESS`: `JMP 5`
- `NONE`: `HLT`

Each decoded instruction contains an opcode, format, addressing mode, operands, and original source. The dashboard displays the decoded format after a step or complete run.

### Addressing modes

- **Immediate:** the operand is a literal value, for example `MOV R1, 10`.
- **Register:** the operand identifies another register, for example `ADD R1, R2`.
- **Direct:** the operand is a memory address, for example `LOAD R1, 100`.

The decoder identifies the mode from the instruction format before the executor runs.

### Instruction cycle

`src/simulator.ts` records the three phases for every valid or rejected instruction:

1. **FETCH** — read the instruction at `PC` and place it in `IR`.
2. **DECODE** — identify opcode, format, operands, and addressing mode.
3. **EXECUTE** — update registers, memory, flags, program counter, or halt state.

The UI presents these phases in the Instruction Cycle panel and preserves them in the execution trace.

### CPU state

`src/cpu.ts` models:

- Registers `R0` through `R7`
- Program Counter (`PC`)
- Instruction Register (`IR`)
- Sparse memory
- Zero, Negative, and Carry flags
- Halt state

Supported operations are `MOV`, `ADD`, `SUB`, `LOAD`, `STORE`, `JMP`, `CMP`, and `HLT`.

## Demonstration

The Arithmetic demo:

```asm
MOV R1, 10
MOV R2, 20
ADD R1, R2
HLT
```

finishes with `R1 = 30`. The Memory demo stores `42` at address `100` and loads it into `R2`, demonstrating direct addressing.

## Subject mapping

| COA concept | Implementation |
|---|---|
| Instruction formats | Decoder format field and Instruction Decoder panel |
| Addressing modes | Immediate, Register, and Direct detection |
| Instruction cycle | Explicit FETCH → DECODE → EXECUTE trace |
| Registers and memory | CPU State panel and executor |

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

This is a deliberately small teaching CPU. It uses integer operands, sparse memory, and a simplified flag model rather than implementing a real processor's binary encoding or complete instruction set.
