[Lecture 1: Bits, Bytes and Integers](#Bits,-Bytes-and-Integers)<br>
[Lecture 2: Machine Programming: Basics](#Machine-Programming-Basics)<br>
[Lecture 3: Machine Programming: Control](#Machine-Programming-Control)<br>

## Bits, Bytes and Integers
### Binary Representations
Binary representation leads to a simple binary, i.e. base-2, numbering system<br>
- 0 represents 0<br>
- 1 represents 1<br>
- Each “place” represents a power of two, exactly as each place in our usual “base 10”, 10-ary numbering system represents a power of 10<br>

## Hexadecimal 00<sub>16</sub> to FF<sub>16</sub>
- Base 16 number representation
- Use characters ‘0’ to ‘9’ and ‘A’ to ‘F’

Consider 1A2B in Hexadecimal:
- 1 \* 16<sup>3</sup> + A \* 16<sup>2</sup> + 2 \* 16<sup>1</sup> + B \* 16<sup>0</sup>
- 1 \* 16<sup>3</sup> + 10 \* 16<sup>2</sup> + 2 \* 16<sup>1</sup> + 11 \* 16<sup>0</sup> = 6699 (decimal)

## Bit-level Manipulations
- **AND** A&B = 1 when both A=1 and B=1
- **OR** A|B = 1 when either A=1 or B=1
- **NOT** ~A = 1 when A=0
- **Exclusive-OR (XOR)** A^B = 1 when either A=1 or B=1, but not both

- Boolean algebra operates on bit vectors.
- In regards to operations, the following:
  - & Intersection 01000001 { 0, 6 }
  - | Union 01111101 { 0, 2, 3, 4, 5, 6 }
  - ^ Symmetric difference 00111100 { 2, 3, 4, 5 }
  - ~ Complement 10101010 { 1, 3, 5, 7 }

### Bit Operations in C
Operations &, |, ~, ^ Available in C
- Apply to any “integral” data type
- long, int, short, char, unsigned
- View arguments as bit vectors
- Arguments applied bit-wise

Examples (Char data type)
- ~0x41 → 0xBE
  - ~01000001 → 10111110
- ~0x00 → 0xFF
  - ~00000000 → 11111111
- 0x69 & 0x55 → 0x41
  - 01101001 & 01010101 → 01000001
- 0x69 | 0x55 → 0x7D
  - 01101001 | 01010101 → 01111101

### Logical Operations in C
<ins>**In contrast to Bit-Level Operators**</ins>
- Logic Operations: &&, ||, !
- View 0 as “False”
- Anything nonzero as “True”
- Always return 0 or 1
- Early termination

Examples (Char data type)
- !0x41 → 0x00
- !0x00 → 0x01
- !!0x41→ 0x01
- 0x69 && 0x55 → 0x01
- 0x69 || 0x55 → 0x01
- p && *p (avoids null pointer access)

### Shift Operations
**Left Shift**: x << y
- Shift bit-vector x left y positions
  – Throw away extra bits on the left
- Fill with 0’s on the right

**Right Shift**: x >> y
- Shift bit-vector x right y positions
  - Throw away extra bits on the right
- Logical shift
  - Fill with 0’s on the left
- Arithmetic shift
  - Replicate the most significant bit on the left
 
**Undefined Behavior**
- Shift amount < 0 or ≥ word size
<img width="295" alt="Screen Shot 2024-01-22 at 6 58 21 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/cf3c1e9c-8c86-46c8-b52d-0b11bb9b4934">

## Integers

### Overflow
For a binary digit line, if two binaries add up to have the resulting binary having an extra digit, this will be considered as **overflow**<br>
For example: 111 + 001 = 1 000, the exceeded 1 is considered overflow.<br>

“Ints” means the finite set of integer numbers that we can represent on a number line enumerated by some fixed number of bits, i.e. <ins>**bit width**</ins>. <br>
- An <ins>**“unsigned” int**</ins> is any int on a number line, e.g. of a data type, that doesn’t contain any negative numbers <br>
- A <ins>**non-negative number**</ins> is a number greater than or equal to (>=) 0 on a number line, e.g. of a data type, that does contain negative numbers <br>

### Negative Numbers
We will use the leading bit as the <ins>**sign**</ins> bit <br>
- 0 means non-negative
- 1 means negative
  - This will allow us to represent negative numbers and non-negative numbers
  - and make 0 represent 0
    - The issue here is there is a -0, which is the same as 0, except that it is different

Given a non-negative number in binary, we can find its negative by flipping each bit and adding 1.
For example:
- 0101 is 5
- 1010 is the "one's complement of 5"
- 1011 is the "twos complement of 5"
- 0101 + 1011 = 1 0000 = 0000
- -x = ~x + 1

This works because after flipping all the bits and making addition to the non-negative number, the result would be 1111, adding 1 in the end would result in a 0 due to overflow. <br>

### Numeric Ranges
- unsigned values:
  - Umin = 0
  - Umax = 2<sup>w</sup> - 1
- Two's complement values:
  - Tmin = -2<sup>w - 1</sup>
  - Tmax = 2<sup>w - 1</sup> - 1

## Conversion and Casting
Large negative weight becomes Large positive weight<br>

Basic rules for casting Signed <-> Unsigned:
- Bit pattern is maintained
- But reinterpreted
- Can have unexpected effects: adding or subtracting 2<sup>w</sup>
- Expression containing signed and unsigned int
  - int is cast to unsigned!

### Two's Complement -> Unsigned
- Ordering inversion
- Negative -> Big Positive

## Expanding and Truncating

### Sign Extension
- Given w-bit signed integer x
- Converting it to a w+k-bit integer with the same value
- This can be done by making k copies of the sign bit and extending it to the new bits.

### Truncation
- Given k+w-bit signed or unsigned integer X
- Converting it to w-bit integer 'X' with the same value for "small enough" X
- This can be done by dropping the top k bits.

### Summary
- Expanding (e.g., short int to int)
  - Unsigned: zeros added
  - Signed: sign extension
  - Both yield the expected result
- Truncating (e.g., unsigned to unsigned short)
  - Unsigned/Signed: bits are truncated
  - Result reinterpreted
  - Unsigned: mod operation
  - Signed: similar to mod
  - For small (in magnitude) numbers yield expected behavior

## Addition, Negation, Multiplication and Shifting

### Addition
- Unsigned Addition:
  - Operand: w bits
  - True sum: w+1 bits
  - Discard Carry: w bits
  - This should ignore the carry output

For example:<br>
   1110 1001 + 1101 0101 = 1 1011 1110 <br>
   Discard the carry: the result is 1011 1110 -> 190 in decimal (UAdd) <br>

- Two's Complement Addition:
  - Operand: w bits
  - True sum: w+1 bits
  - Discard Carry: w bits
  - The bit-level behavior is the same with TAdd and UAdd

For example:<br>
   1110 1001 + 1101 0101 = 1 1011 1110 <br>
   Discard the carry: the result is 1011 1110 -> -66 in decimal (TAdd) <br>

### Visualizing Additions
<img width="478" alt="Screen Shot 2024-01-23 at 12 08 15 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/40916407-6e5d-47e3-bbb5-5666a921e961"><br>
<img width="477" alt="Screen Shot 2024-01-23 at 12 08 33 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/51eee06d-1fa8-43ab-8eee-86f3828490de"><br>
<img width="516" alt="Screen Shot 2024-01-23 at 12 08 48 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/0d6e4aa5-92d4-4f0e-95bf-22d5a4bd59c8"><br>

### Multiplication
Power-of-2 Multiply with Shift
- Operation:
  - u << k gives u \* 2<sup>k</sup>
  - Both signed and unsigned
  - Operand: w bits
  - True Product: w+k bits
  - Discard k bits: w bits

For example: <br>
  u << 3 == u \* 8
  (u << 5) - (u << 3) == u \* 24
  Most machines shift and add faster than multiply since the compiler generates this code automatically.

### Division
Unsigned Power-of-2 Divide with Shift<br>
- Operation:
  - u >> k gives floor(u / 2<sup>k</sup>)
  - This would require logical shift.

Examples: <br>
<img width="781" alt="Screen Shot 2024-01-23 at 12 17 45 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/b86b3a95-c324-4384-ad49-b1d03f1abab1"><br>

Signed Power-of-2 Divide with Shift<br>
- Operation
  - x >> k gives floor(x / 2<sup>k</sup>)
  - This would require arithmetic shift.
  - Round to the left, not toward zero (Unlikely to be what is expected, introduces a bias.)

Examples: <br>
<img width="789" alt="Screen Shot 2024-01-23 at 12 20 10 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/ae91bbf5-f8bb-47f2-af05-efdf3348cf60"><br>

## Byte Ordering
Big Engian: Sun (Oracle SPARC), PPC Mac, Internet<br>
- The least significant byte has the highest address<br>

Little Endian: x86, ARM<br>
- The least significant byte has the lowest address<br>

Byte ordering is a concern when we are communicating data over a network, via files, etc.<br>
Note: <br>
- Bits are not reversed, as the low-order bit is the reference point.<br>
- This doesn't affect chars, or strings (array of chars), as chars are only one byte.<br>

Example: Given a variable x has a 4-byte value of 0x01234567, the address given by &x is 0x100:<br>
<img width="686" alt="Screen Shot 2024-01-23 at 12 26 10 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/63531822-9b65-4ed1-99cb-b364bb18ef48"><br>

### Reading Byte-Reversed Listing
Disassembly: <br>
- Text representation of binary machine code<br>
- Generated by the program that reads the machine code<br>

For example:<br>
- Value: 0x12ab<br>
- Pad to 32 bits: 0x000012ab<br>
- Split into bytes: 00 00 12 ab<br>
- Reverse: ab 12 00 00<br>

If we want to add this value to %ebx, then the assembly code would be: <br>
add $0x12ab, %ebx <br>
The instruction code corresponding would be: <br>
81 c3 ab 12 00 00 <br>

## Machine Programming Basics
### Assembly Basics
- <ins>**Architecture**</ins>: (also ISA: instruction set architecture) The parts of a processor design that one needs to understand for writing assembly/machine code.
  - Examples: instruction set specification, registers, memory model
  - The architecture is not the hardware, it is the interface of the hardware
- <ins>*Microarchitecture**</ins>: Implementation of the architecture
  - Examples: cache sizes and core frequency

- Code Forms:
  - <ins>**Machine Code**</ins>: The byte-level programs that a processor executes
  - <ins>**Assembly Code**</ins>: A text representation of machine code, (human-readable version of the assembly language)

Example ISAs:
- Intel: x86, IA32, Itanium, x86-64
- ARM: Used in almost all mobile phones
- RISC V: New open-source ISA

### Assembly Code and Machine Basics
- <ins>**PC: Program counter**</ins>
  - Address of next instruction
  - Called “RIP” (x86-64)
- <ins>**Register file**</ins>
  - Heavily used program data
  - Can be accessed directly
- <ins>**Condition codes (FLAGS)**</ins>
  - Store status information about the most recent arithmetic or logical operation
  - Used for conditional branching
- <ins>**Memory**</ins>
  - Byte addressable array
  - Heap to support code and user data 
  - Stack to support procedures
<img width="710" alt="Screen Shot 2024-01-23 at 2 17 32 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/43881059-d097-4c46-9ccc-e773c903794c">

### Assembly Data Types
- Integer: data of 1, 2, 4, or 8 bytes
  - Data values
  - Addresses (untyped pointers)
- Floating points: 4, 8 or 10 bytes
- SIMD vector data types of 8, 16, 32 or 64 bytes
- Code: Byte sequences encoding a series of instructions
- No aggregate types such as arrays or structures
  - Just contiguously allocated bytes in memory

Example:<br>
<img width="374" alt="Screen Shot 2024-01-23 at 2 31 04 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/db92779a-df9c-4ccf-8882-600e5241b4a8"><br>
These are 64-bit registers, so we know this is a 64-bit add <br>

### X86-64 Integer Registers
<img width="1144" alt="Screen Shot 2024-01-23 at 2 34 59 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/03150432-333f-4b76-a3b1-2c8f332df516"><br>

### Moving Data
- Moving Data:
  - movq Source, Dest
- Operand Types
  - Immediate: Constant integer data
    - Example: $0x400, $-533
    - Like C constant, but prefixed with ‘$’
    - Encoded with 1, 2, or 4 bytes
  - Register: One of 16 integer registers
    - Example: %rax, %r13
    - But %rsp reserved for special use
    - Others have special uses for particular instructions
  - Memory: 8 consecutive bytes of memory at the address given by the register
    - Simplest example: (%rax)
    - Various other “addressing modes”

Movq Operand Combinations:<br>
<img width="911" alt="Screen Shot 2024-01-23 at 2 46 52 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/be735984-ef85-4a22-ab3a-195c6406b5b3">
- The immediate cannot be a dest
- Cannot do memory-memory transfer with a single instruction

### Memory Addressing Nodes
Normal (R) Mem[Reg[R]]
- Register R specifies the memory address
- Aha! Pointer dereferencing in C
- movq (%rcx),%rax
  
Displacement D(R) Mem[Reg[R]+D]
- Register R specifies the start of the memory region
- Constant displacement D specifies the offset
- movq 8(%rbp),%rdx

<ins>**Most General Form**</ins>:
D(Rb,Ri,S) Mem[Reg[Rb]+S*Reg[Ri]+ D]
- D: Constant “displacement” 1, 2, or 4 bytes
- Rb: Base register: Any of 16 integer registers
- Ri: Index register: Any, except for %rsp
- S: Scale: 1, 2, 4, or 8 (why these numbers?)
  
Special Cases
- (Rb,Ri) Mem[Reg[Rb]+Reg[Ri]]
- D(Rb,Ri) Mem[Reg[Rb]+Reg[Ri]+D]
- (Rb,Ri,S) Mem[Reg[Rb]+S*Reg[Ri]]

An example of a swap function:<br>
<img width="847" alt="Screen Shot 2024-01-23 at 2 58 33 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/e3b05ccd-abc7-4f6f-874c-77b463843975"><br>

An example of address computation:<br>
<img width="734" alt="Screen Shot 2024-01-23 at 3 01 13 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/0a939e1c-332f-47a5-a4b7-5f513a41d004"><br>

### Address Computation Instruction
leaq Src, Dst
- Src is address mode expression
- Set Dst to address denoted by the expression
Uses
- Computing addresses without a memory reference
  - E.g., translation of p = &x[i];
- Computing arithmetic expressions of the form x + k \* y
  - k = 1, 2, 4, or 8

### Turning C into Object Code
<img width="877" alt="Screen Shot 2024-01-25 at 2 02 40 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/62264bf5-2dfc-4d43-9b8e-a596e0571b08"><br>

## Machine Programming Control
### Finding pointers
- %rsp and %rip always hold pointers
  - Register values that are “close” to %rsp or %rip are probably also pointers
- If a register is being used as a pointer…
  - mov (%rsi), %rsi
  - …Then its value is expected to be a pointer.
    - There might be a bug that makes its value incorrect.

- Not as obvious with complicated address “modes”
  -  (%rsi, %rbx) – One of these is a pointer, we don’t know which.
  -  (%rsi, %rbx, 2) – %rsi is a pointer, %rbx isn’t (why?)
  -  0x400570(, %rbx, 2) – 0x400570 is a pointer, %rbx isn’t (why?)
  -  lea (anything), %rax – (anything) may or may not be a pointer

### Control Flow
- We would use jmp statements as GOTO statements for changing control flows
<img width="793" alt="Screen Shot 2024-01-25 at 2 22 37 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/a6829724-13e1-4ca9-bc8d-e9038aabe74c"><br>
- We will use condition codes to determine if we need to jump

- Single-bit registers
  - CF Carry Flag (for unsigned)
  - SF Sign Flag (for signed)
  - ZF Zero Flag
  - OF Overflow Flag (for signed)

- Compare Instruction
  - cmp a, b
    - Computes 𝑏𝑏 − 𝑎𝑎 (just like sub)
    - Sets condition codes based on the result, but…
    - Does not change 𝒃𝒃
    - Used for if (a < b) { … } whenever 𝑏𝑏 − 𝑎𝑎 isn’t needed for anything else

- Test Instruction
  - test a, b
    - Computes 𝑏𝑏&𝑎𝑎 (just like and)
    - Sets condition codes (only SF and ZF) based on the result, but…
    - Does not change 𝒃𝒃
    - Most common use: test %rX, %rX to compare %rX to zero
    - Second most common use: test %rX, %rY tests if any of the 1-bits in %rY are also 1 in %rX (or vice versa)

### Conditional Branches
- jX Instructions
  - Jump to different parts of the code depending on condition codes
<img width="646" alt="Screen Shot 2024-01-25 at 2 30 04 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/d34bd338-6ee0-4fd6-9b48-8a418730d82a"><br>
- For example:
  - cmp a, b
  - je 1000
- If a and b are equal, then jump to instruction 1000

- SetX Instructions
  - Set the low-order byte of the destination to 0 or 1 based on combinations of condition codes
  - Does not alter the remaining 7 bytes<br>
<img width="644" alt="Screen Shot 2024-01-25 at 2 32 20 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/83fdd64b-0395-4d3f-ad4c-294e13406628"><br>

- Example<br>
<img width="825" alt="Screen Shot 2024-01-25 at 2 45 26 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/db1098d9-dfd8-443a-b212-d9059937aeae"><br>

### Conditional Move Instruction
- This is much more efficient for processors
  
- Conditional Move Instructions
  - Instruction supports:
    - if (Test) Dest ← Src
  - Supported in post-1995 x86 processors
  - GCC tries to use them
    - But, only when known to be safe
- Why?
- Branches are very disruptive to an instruction flow through pipelines
- Conditional moves do not require control transfer
<img width="851" alt="Screen Shot 2024-01-25 at 2 49 02 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/20ff5d06-fb88-491b-8a0d-122ab5973c2e">

### Loops
- Use conditional branch to either continue looping or to exit the loop<br>
<img width="858" alt="Screen Shot 2024-01-25 at 2 52 54 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/7483aa24-b508-466f-a733-0d84014ae0fc">

