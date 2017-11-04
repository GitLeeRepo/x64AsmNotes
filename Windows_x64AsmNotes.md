# Overview

x64 Assembly language notes using MASM (Microsoft Assembler)

# References

## YouTube Videos

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
