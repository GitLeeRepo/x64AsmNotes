# Overview

Notes on x64 assembly code under Linux.

# Reference

* [Introduction to 64 Bit Intel Assembly Language Programming for Linux: Second Edition](https://www.amazon.com/gp/product/B008H7HL3M/ref=oh_aui_d_detailpage_o00_?ie=UTF8&psc=1) by Ray Seyfarth.  Many of the notes taken come from this book
* [My notes on x86 assembly for Windows](https://github.com/GitLeeRepo/x86Andx64AsmNotes/blob/master/Windows_x86AsmNotes.md) which contains more in depth descriptions of the instruction set
* [64-bit Examples](https://www.csee.umbc.edu/portal/help/nasm/sample_64.shtml) - includes printf examples for 64-bit (using registers not stack for params)

# Assembling and Linking

## For Programs that do NOT use a main proc 

Using a _Start label instead of a main proc for example

**Assemble with:**

```bash
yasm -f elf64 -gdwarf2 -l progname.lst progname.asm
```

* -f elf64 - the Linux executable format
* -gdwarf2 - debugger ino
* -l progname.lst - the assembly listing

**Link with:**

```bash
ld -o progname progname.o
```

## For Programs that DO use a main proc

Assemble with **yasm** as above, but use **gcc** to do the linking instead

```bash
gcc -static -o progname progname.o
```
Note that the examples in [Introduction to 64 Bit Intel Assembly Language Programming for Linux: Second Edition](https://www.amazon.com/gp/product/B008H7HL3M/ref=oh_aui_d_detailpage_o00_?ie=UTF8&psc=1) by Ray Seyfarth, do NOT include the **-static** arguement for **gcc**, but without it I get an **"/usr/bin/ld: pythag.o: relocation R_X86_64_32 against `.data' can not be used when making a shared object; recompile with -fPIC /usr/bin/ld: final link failed: Nonrepresentable section on output"** error.  The error doesn't occur when using the **ld** linker (requires **_start** label instead of **main**), so apparently and issue with using **gcc** as the linker, where it wants to link to shared libararies that must be relocateable, unless the **-static** arguement is used.  **yasm** does not have a **-fPIC** option.
