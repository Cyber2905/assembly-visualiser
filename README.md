# x86-64 Stack Visualiser

A small browser tool that lets you step through x86-64 assembly one instruction at a time and watch the registers and the stack change, colour-coded so it's easy to follow even if you're new to assembly.

It's a single `index.html` file with no dependencies and no build step. Everything runs in your browser using a small, deterministic simulator written in plain JavaScript, so what you see is exactly what the instruction does.

**Live demo:** https://cyber2905.github.io/asm-visualiser/ (after you enable GitHub Pages, see below)

## What it does

- Enter your own program (or pick an example), then step forward and backward through it.
- Each step gets a plain-English explanation, for example *"push: rsp moves down 8 bytes, then the value of rbp is stored in that new slot."*
- Registers and stack slots that just changed are highlighted.
- Colour coding shows what each thing is:
  - **Instructions:** stack (teal), copy data (blue), maths (purple), jump / call (orange)
  - **Stack slots:** return address, saved `rbp`, pushed value, local variable, and reserved-but-empty space
- Helpful error messages with line numbers when the program can't run, such as unknown instructions with "did you mean...?" suggestions, wrong operand counts, memory-to-memory moves, missing labels, and a `ret` that doesn't find a return address.

## Supported syntax

Intel syntax, one instruction per line.

| Category | Instructions |
|---|---|
| Stack | `push`, `pop`, `leave` |
| Data | `mov`, `lea` |
| Maths | `add`, `sub`, `xor` |
| Control | `call`, `ret`, `jmp`, `nop` |

- **Registers:** `rax rbx rcx rdx rsi rdi rbp rsp`, plus 32-bit names such as `eax` and `edi`
- **Operands:** registers, decimal or `0x` hex numbers, and memory like `[rbp-8]` or `DWORD PTR [rbp-4]`
- **Comments** start with `;`. Lines like `.globl main` are ignored, so you can paste output from [Compiler Explorer](https://godbolt.org).

The program starts as if a function had just been called: `rdi = 5`, `rsi = 7`, and a return address is already on the stack.

## Run it locally

Open `index.html` in any modern browser. That's it.

## Deploy with GitHub Pages

1. Create a new repository on GitHub named `asm-visualiser`.
2. Put `index.html` and `README.md` in the **root** of the repository (not inside a subfolder).
3. Commit and push:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Add x86-64 stack visualiser"
   git branch -M main
   git remote add origin https://github.com/Cyber2905/asm-visualiser.git
   git push -u origin main
   ```
4. On GitHub, open the repository's **Settings → Pages**.
5. Under **Build and deployment**, set **Source** to **Deploy from a branch**, choose branch **main** and folder **/ (root)**, then click **Save**.
6. Wait a minute or two. The page will be live at `https://cyber2905.github.io/asm-visualiser/`.
7. Paste that link into the "Live demo" line above and into the repository's **About** box.

## Known limitations

- Each 8-byte stack slot holds one value, so a 4-byte write fills a whole slot rather than half of one.
- No flags or conditional jumps yet (`cmp`, `je`, `jne`, ...).
- Only 8 registers are modelled.

## Ideas for next steps

- Conditional jumps with a flags panel, and a loop example
- Byte-accurate memory so 4-byte and 8-byte values can share a slot
- More registers (`r8`-`r15`) and instructions (`imul`, `and`, `or`, `shl`, ...)
- Unit tests for the instruction handlers

## Licence

Add a licence file (MIT is a common choice for small projects) before sharing the repository publicly.
