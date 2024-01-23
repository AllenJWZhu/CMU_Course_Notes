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
- Least significant byte has the highest address<br>

Little Endian: x86, ARM<br>
- Least significant byte has the lowest address<br>

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

If we want to add this value to ebx, then the assembly code would be: <br>
add $0x12ab, %ebx <br>
The instruction code corresponding would be: <br>
81 c3 ab 12 00 00 <br>
