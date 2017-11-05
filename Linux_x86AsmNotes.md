# Overview

Notes on x64 assebly code under Linux.

# Reference

* [Introduction to 64 Bit Intel Assembly Language Programming for Linux: Second Edition](https://www.amazon.com/gp/product/B008H7HL3M/ref=oh_aui_d_detailpage_o00_?ie=UTF8&psc=1) by Ray Seyfarth.  Many of the notes taken come from this book
* My notes on x64 assembly for Windows which contains more indepth descriptions of the instruction set

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

## For Progams that DO use a main proc

Asseble with **yasm** as above, but use **gcc* to do the linking instead

```bash
gcc -o progname progname.o
