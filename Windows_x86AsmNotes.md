# Overview

x86-32 Assembly language notes using MASM (Microsoft Assembler) under Windows.  Refer to the separate x64 notes for both Windows and Linux.

# References

* [Modern X86 Assembly Language Programming: 32-bit, 64-bit, SSE, and AVX](https://www.amazon.com/gp/product/1484200659/ref=oh_aui_detailpage_o00_s00?ie=UTF8&psc=1) by Daniel Kusswurm, published by Apress.  Many of the notes here come from following this book.

## YouTube Videos

* [Intro to x86 Assembly & Architecture](https://www.youtube.com/playlist?list=PL038BE01D3BAEFDB0) - Classroom based course from Open Security Training by Xeno Kovah.  Several of the notes came from this video.

# Data Types

## Fundemental Data Types

Data Type       | Bit Length | Byte Length | Typical Use                        | C/C++ Types
----------------|------------|-------------|------------------------------------|--------------------
Byte            | 8          | 1           | Char, int, BCD                     | char
Word            | 16         | 2           | Char, int                          | short int
Double Word     | 32         | 4           | Int, single prec float             | int, long, float
Quadword        | 64         | 8           | Int, double prec float, packed int | long long, double
Quintword       | 80         | 10          | Double ext-prec float, packed BCD  | long double
Double Quadword | 128        | 16          | Packed int, packed float           | n/a
Quad Quadword   | 256        | 32          | Packed int, packed float           | n/a

## Additional Data Types

* **X86 strings** - typically stored in bytes and words.  Several instructions are provided compare, move, load, store, scan strings.  X86 strings are also used for arrays.
* **Bit fields** - used for masks by certain instructions.  Up to 32 bits in length.
* **Bit strings** - can be up to **2^32 - 1** bits in length.  Several instructions provided to operate on them
* **BCD** - Binary Coded Decimal - for representing decimal values.  When **packed** there are two digits per byte, while unmpacked are 1 digit per byte.

## Packed Data Types

Packed data types are used in SIMD (Single Intruction Multiple Data) instructions.  For example a 64 bit packed field can pack 8 8-bit integers, or 4 16-bit integers, or 2 32-bit integers

## Example Data Definition Statement

```asm
    myByte      db  'A'                   ; Byte
    myWord      dw  ffffH                 ; Word 2-bytes - unsigned: 65,535; signed -1
    myDouble    dd  ffffffffH             ; Double word 4-bytes - unsigned: 4,294,967,295; signed -1
    myQuad      dq  ffffffffffffffffH     ; Quadword 8-bytes - signed -1
    myTen       dt  ffffffffffffffffffffH ; Ten bytes 
    myString    db "Hello, World!", 0dh, 0ah, 0 ; terminated with a CR/LF and zero to mark the end of the string
```

# Registers                       

## General Purpose Registers

There are eight 32-bit general purpose registers in the x86-32 core.  They are primarily used for arithmetic, address calculations, and logical operations.

32-bit Register | 16-bit register | 8-bit registers | 32-bit Specialized Uses
----------------|-----------------|-----------------|------------------------------------
EAX             | AX              | AH and AL       | Accumulator.  Stores return values.
EBX             | BX              | BH and BL       | Memory pointer, base register
ECX             | CX              | CH and CL       | String repeat counts, loop counter 
EDX             | DX              | DH and DL       | imul and idiv operations
ESI             | SI              | N/A             | Source String Addr
EDI             | DI              | N/A             | Destination String Address
EBP             | BP              | N/A             | Base pointer for items on stack
ESP             | SP              | N/A             | Stack related operations

Some of the Specialized Uses are by convention only. Several can be used for general purposes (calculations, temp storage, etc), although the **ESP** should not be used for anything other than its stack related functionality.

## Segment Registers

The segment registers **(CS, DS, SS, ES, FS, GS)** are used to specify memory for **code, data, and stack segements**.  Normally the programmer doesn't need to deal with these registers, the operating system manages them.

## Other Registers

* **EFLAG Register** - contains status bits that track logical and arithmetic operations, among other things
* **EIP Register** - the instruction pointer that contains the offset to the next instruction to be executed.  This register is not directly accessed.
* **X87 Registers (MMX)** - Supports **SIMD Single Instruction Multiple Data** operations
* **AVE/SSE Reigisters** - Supports **SIMD Single Instruction Multiple Data** operations

# Instruction Set

## Operands

There are three basic types of operands:
* Immediate - constant value that is part of the instruction
* Register - operate on the general purpose registers
* Memory - operate on memory and data stored in memory

The following are all valid operand operatons

```asm
    mov eax,10        ; move immediate to register
    mov eax,ebx       ; move register to register
    mov eax,[myVar]   ; move memory value to register
    lea eax,&myvar    ; move memory address to register
    mov [myVar],eax   ; move register value to memory location
    mov [myVar],10    ; move immediate to memory location
```
Note that moving memory to memory is NOT a valid operation


## Memory Addressing

Memory is addresses through several components (Displacements, Base Registers, Index Registers, and a scale factor for the index).  An **Effective Address** is caluclated from **BaseReg + IndexReg  \* ScaleFactor + Displ** 

Address Mode                    | Example
--------------------------------|-------------------------------
Disp                            | mov eax,\[ValInMem\]
BaseReg                         | mov eax,\[ebx\]
BaseReg + Disp                  | mov eax,\[ebx+8\]
Disp + IndexReg * SF            | mov eax,\[MyArray+esi\*4\]
BaseReg + IndexReg              | mov eax,\[ebx+esi\]
BaseReg + IndexReg  * SF + Disp | mov eax,\[ebx+esi\*4+MyVar\]

### Big Endian vs Little Endian

* **Big Endian** - Typically found on **RISC** based architectures.  Data is stored in memory for its most signficant byte to the least significant byte.  When looking at a memory dump of bytes in hex, the numbers read as we normally read numbers, from left to right.
* **Little Endian** - Typically found on **CISC** based architectures (such as the x86).  Data is stored from the least significant byte to the most significant byte.  Therefore, when looking at a memory dump of bytes in hex, the number reads backwards to what we are accustommed to, you read the number from right to left.  This is only true of memory locations, within **registers** the bytes are layout in **Big Endian** format.

### No Memory to Memory Operations

Unlike some assembly languages, such as IBM's Basic Assembly Language on the mainframe, there are no memory to memory operations in x86 assembly.  Everything has to move through a register, except in the case of moving immediate (in line constant) values to memory which is allowed.

## Data Transfer Instructions


Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**mov**      | x
**cmovcc**   | x
**push**     | x
**pop**      | x
**pushad**   | x
**popad**    | x
**xchg**     | x
**xadd**     | x
**movsx**    | x
**movzx**    | x

## Binary Arithmetic Instructions

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**add**      | x
**addc**     | x
**sub**      | x
**sbb**      | x
**imul**     | x
**mul**      | x
**idiv**     | x
**div**      | x
**inc**      | x
**dec**      | x
**neg**      | x
**daa**      | x
**das**      | x
**aaa**      | x
**aas**      | x
**aam**      | x
**aad**      | x

## Data Comparison Instructions

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**cmp**      | x
**cmpxchg**  | x
**cmpxchg8b** | x

## Data Conversion Instructions

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**cbw**      | x
**cwde**     | x
**cwd**      | x
**cdq**      | x
**bswap**    | x
**movbe**    | x
**xlatb**    | x

## Logical Instructions

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**and**      | x
**or**       | x
**xor**      | x
**not**      | x
**test**     | x

## Rotate and Shift

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**rcl**      | x
**rcr**      | x
**rol**      | x
**ror**      | x
**sal/shl**  | x
**sar**      | x
**shr**      | x
**shld**     | x
**shrd**     | x

## Byte Set and Bit String Instructions

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**setcc**    | x
**bt**       | x
**bts**      | x
**btr**      | x
**btc**      | x
**bsf**      | x
**bsr**      | x

## String Instructions

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**cmpsb**    | x
**cmpsw**    | x
**cmpsd**    | x
**lodsd**    | x
**lodsw**    | x
**movsb**    | x
**movsw**    | x
**movsd**    | x
**scasb**    | x
**scasw**    | x
**scasd**    | x
**stosb**    | x
**stosw**    | x
**stosd**    | x
**rep**      | x
**repe**     | x
**repz**     | x
**repne**    | x
**repnz**    | x

## Flag Manipulation Instructions

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**clc**      | x
**stc**      | x
**cmc**      | x
**std**      | x
**lahf**     | x
**sahf**     | x
**pushfd**   | x
**popfd**    | x

## Control Transfer Instructions

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**jmp**      | x
**jcc**      | x
**call**     | x
**ret**      | x
**enter**    | x
**leave**    | x
**jecxz**    | x
**loop**     | x
**loope**    | x
**loopz**    | x
**loopne**   | x
**loopnz**   | x

## Miscellaneous Instructions

Mnemonic     | Description
-------------|------------------------------------------------------------------------------------
**bound**    | x
**lea**      | x
**nop**      | x
**cpuid**    | x

# Calling Conventions

## C Declaration

* The most common caling convention
* Function parameters pushed onto the stack from right to left
* Saves the old stack pointer and sets up a new stack frame
* eax or edx:eax returns the results of primitive data types
* **Caller** is responsible for cleaning up the stack

## Standard Declaration

* Not as frequently used, although t is used by Microsoft C++ and the Win32 API
* Parameters are also pushed on to the stack from right to left
* Saves the old stack frame pointer and sets up a new one.
* eax or edx:eax returns the results of primitive types
* The **Called** routine is responsible for cleaning up the stack

