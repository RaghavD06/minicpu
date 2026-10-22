# MiniCPU — Formal Language + CPU Simulator

MiniCPU is a client-side academic simulator integrating **Formal Language and Automata Theory (FLAT)** with **Computer Organization and Architecture (COA)**. It validates assembly using instruction-specific regular expressions and a deterministic finite automaton, then decodes and executes accepted instructions through a fetch–decode–execute cycle.

## Run locally
```bash
npm install
npm run dev
```
Use `npm run build` for a production bundle. Deploy the generated `dist` directory to Vercel or another static host. `vercel.json` provides an SPA rewrite for refresh/navigation.

## Deployment
**GitHub repository:** https://github.com/RaghavD06/minicpu  
**Deployed URL:** https://minicpi.vercel.app

## Mapping to Academic Concepts
- **FLAT — Regular Expressions:** `src/flat.ts` defines a pattern for every opcode; results are shown in the validation pipeline.
- **FLAT — DFA:** token classes are walked through deterministic `q0` → ... → `ACCEPT` or `INVALID` states and visualized.
- **COA — Instruction Formats:** the decoder produces register/immediate, register/register, register/address, or address formats.
- **COA — Instruction Cycle:** each step records FETCH, DECODE, and EXECUTE.
- **COA — Addressing Modes:** Immediate, Register, and Direct modes are detected from the decoded format.

## Instruction set
`MOV Rn, immediate|Rn`, `ADD Rn, Rn`, `SUB Rn, Rn`, `LOAD Rn, address`, `STORE Rn, address`, `JMP address`, `CMP Rn, Rn`, and `HLT`. Registers are `R0`–`R7`; the CPU exposes PC, IR, Z/N/C flags, and sparse memory.

Use the toolbar to load Arithmetic, Memory, or Invalid syntax demos. Invalid lines are traced but never reach the decoder or executor.

## Submission reports

- `REPORT_COA.md` — Computer Organization and Architecture report
- `REPORT_FLAT.md` — Formal Language and Automata Theory report
