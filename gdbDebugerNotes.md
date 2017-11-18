# Overview

Notes on using the gdb debugger under Linux.  It also contains information on the **objdump** command

# Reference

## YouTube Videos

* [Into to gdb](https://www.youtube.com/watch?v=xQ0ONbt-qPs)

# Command Line Args

* **gdb progname** - launch with the executable name to debug.  Make sure the executable was complile/assembled with the **-g** flag to include debugging information (symbolic names, etc)
* **gdb --args progname arg1 arg2 ...** - launch with command line arguements for progname

# Commands in the debugger

## Display Commands

* **p varname** - (**print**) display the contents of the memory variable in decimal
* **p/a &varname** - (**print**) display the address of the variable
* **p/c varname** - (**print**) display the value as a character
* **p/d varname** - (**print**) display the value as a decimal number (the default)
* **p/f varname** - (**print**) display the value as a floating point numbers
* **p/i $pc**     - (**print**) display the instruction at the current execution line
* **p/s varname** - (**print**) display a string value
* **p/t varname** - (**print**) display the value as a binary number
* **p/u varname** - (**print**) display the value as an unsigned integer
* **p/x varname** - (**print**) display the value in hex format
* **

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

* **i r** - (**info registers**) - display all registers
* **i r regname** - (**info registers**) display the specified register
* **i r reg1 reg2** - (**info registers**) display the two registers listed
* **p $regname** - (**print**) display the contents of the $regname register

## Watch points

When a watch is added, it will display the value of the watched variable/register when it is changed while stepping through the code.  The code will break at the watch point

* **wa varname** - (**watch**) - add a watch on varname
* **wa $regname** - (**watch**) - add a watch on the specified register
* **i wat** - (**info watch**) - display all watches
* **awatch** - set a watchpoint for an expression.  The program will break when the expression is true.
* **rwatch varname** - shows watch when varname is read.  The program will break when the variable is read
* **dis #** - (**disable**) - disable watch by number (use **i wat**) to get watch numbers.  It is still in the list, but is disabled
* **en #** - (**enable**) - enable disabled watch by number
* **del #** - (**delete**) - delete a watch by number both disabling and removing it from the list


## Display Source and Location

* **l** - (**list**) display 10 lines of source from 5 lines before to 5 lines from current position
* **l 15** - (**list**) display 10 lines of source, starting at line 10 through 19 (5 before 15 and 5 including and after 15)
* **l 1,20** - (**list**) display from line 1 through line 20
* **whe** - (**where**) display the next line to be executed
* **bt** - (**back trace**) show the current line of exection and what function you are in (current call stack frame).  If there are multiple nested calls it will show the call stacks of the calling procs that proceeded it.  To switch the context to one of the call frames higher in the call hierarch in order to examine its local varibles enter **frame #** with the number being the frame number listed when running **bt**.

Note: pressing **Enter** repeats the last command, so entering **list** followed by **Enter** on a blank line will list the next 10 lines of code.  This can be repeated until you reach the end of the program listing.

## Break points

* **b 18** - (**break**) set break point on line 18
* **cl 18** - (**clear**) break point on line 18
* **i b** - (**info break**) dieplays list of break points and watches

## Running and Stepping through code

* **start** - start execution of program, pausing at first instruction
* **r args** - (**run**) run the program with optional args.
* **s optcount** - (**step**) run one line of code.  Step **into** function, or **over** the code if not a function on the current line
* **n optcount** - (**next**) run one line of code.  Step **over** a function or any other line of code
* **c** - (**continue**) - continue running code from the current line
* **en** - (**enbable**) break points
* **dis** - (**disable**) break points

Note: since **Enter** repeats the last command you can continue to **step** through the program by just pressing **Enter** on a blank line after the original **step** has been executed.

## Setting Variables/Registers in the Debugger

* **set var varname = 5** - set the variable varname = 5
* **set var $regname = 8** - set the register $regname = 8


## objdump Command

Provides a dump for binary executables and object files.  The **-d** arguement tells it to disasseble the output, and the **-M intel** commnd tells it to display the assembly code in the **intel format** rather than the default **AT&T format**

```bash
objdump -d -M intel ObjOrExeFile
```

## Doing a Hex Dump

Use the **xxd** utility to do a hex dump from a file, or from the console when no parameters are provided (end input wiht Ctrl-D)

```bash
xxd test.o

...
00000070: 0200 0000 0000 0000 0000 0000 4865 6c6c  ............Hell
00000080: 6f2c 2077 6f72 6c64 210a 0000 002e 7465  o, world!.....te
...
```
