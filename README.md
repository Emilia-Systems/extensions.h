# extensions.h

## Why this exists

C99 has served me well for a long time, but it's missing a lot of ergonomics
that C++ standards take for granted, things like `nullptr`,
real compile-time constants, `typeof`, checked arithmetic, and clean
attribute syntax. Rather than reach for C++ wholesale, the goal here is to
pull in the useful parts of **C23** (and the GNU extensions C23 still
doesn't cover) into a single, well-documented header, so the rest of the
codebase can use clear, self-explanatory names (`packed`, `noreturn`,
`force_inline`) instead of scattering raw `__attribute__((...))` and
`__builtin_...` calls everywhere.

In short: this header is the seam between "plain C" and "everything modern
GCC actually offers," kept in one place so it's easy to see exactly what
compiler magic the project depends on.

## What it's for

This header is part of a **custom RV64 based system[*]** built from
scratch, targeting the GNU toolchain exclusively (no Clang/MSVC
portability attempted). It centralizes:

- Struct layout control (`packed`, `align`)
- Linker placement (`section`)
- Function behavior hints (`noreturn`, `hot`, `cold`, `noinline`,
  `force_inline`, `noclone`)
- Trap/interrupt handler generation for RISC-V's privilege modes
  (`interrupt(int_machine | int_supervisor | int_rnmi)`)
- Optimizer hints (`likely`, `unlikely`, `unreachable`)

**[*]** *"System" here refers to the complete Von Neumann architecture
this project targets, not just the OS, but the CPU, GPU, and NIC design
it will eventually run on.*

## Scope and assumptions

- **Toolchain**: GCC only.
- **Architecture**: RISC-V, starting from a base `rv64i` target. Specific
  extensions (M, A, F, D, C, etc.) are still being decided as the project
  grows.
- **Environment**: freestanding (`-ffreestanding`, no hosted libc), every
  macro here needs to work without any standard library support.

## Status

Actively evolving alongside the OS itself. Expect additions as new
architecture-specific needs come up (memory model, per-hart data, atomics,
etc.) once the extension set is finalized.