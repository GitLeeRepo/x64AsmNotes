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
* **x/20i $pc** - - display instructions starting at the program counter (i.e. current location in code)
* **x/20w &varname** - dispaly 20 word size values beginning at the address of varname

## Examine Registers

* **i r** - (**info registers**)

## Display Source and Location

* **l** - (**list**) display 10 lines of source from 5 lines before to 5 lines from current position
* **l 15** - (**list**) display 10 lines of source, starting at line 10 through 19 (5 before 15 and 5 including and after 15)
* **l 1,20** - (**list**) display from line 1 through line 20
* **bt** - (**back trace**) show the current line of exection and what function you are in, with the calling stack

## Setting break points

* **b 18** - (**break**) set break point on line 18
* **cl 18** - (**clear**) break point on line 18

## Running and Stepping through code

* **r** - (**run**) code to next breakpoint (if any) or to completion if not.
* **s** - (**step**) **into** function, or **over** if not a function on the current line
* **n** - (**next**) step **over** a function or any other line of code
* **c** - (**continue**) - continue running code from the current line
* **en** - (**enbable**) break points
* **dis** - (**disable**) break points



