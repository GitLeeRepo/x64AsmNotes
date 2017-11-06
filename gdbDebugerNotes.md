# Overview

Notes on using the gdb debugger under Linux

# Reference

# Command Line Args

# Commands in the debugger

## Display Commands

* **p varname** - display the contents of the memory variable in decimal
* **p/a varname** - display the address of the variable
* **p/c varname** - display the value as a character
* **p/d varname** - display the value as a decimal number (the default)
* **p/f varname** - display the value as a floating point numbers
* **p/i**         - display the instruction
* **p/s varname** - display a string value
* **p/t varname** - display the value as a binary number
* **p/u varname** - display the value as an unsigned integer
* **p/x varname** - display the value in hex format

## Examine Memory Commands

Note gdb labels data type commands differently than the assembler.  To designate a memory size to examine, use the following single letters with the **x** command:

Letter | Type     | Size in Bytes
-------|----------|--------------
b      | byte     | 1
h      | halfword | 2
w      | word     | 4
g      | giant    | 8

The single letter qualifiers used for the display commands above can also be used, **x** for hex for example.

* **x/w &varname** - word size contents at the specified address (&varname) in decimal
* **x/xw &varname** - word size content at the specified address (&varname) in hexadecimal
* **x/s &strname** - the null terminated string at the specified address (&strname)



