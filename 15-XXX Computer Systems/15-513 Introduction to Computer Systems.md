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
- Fill with 0’s on right

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

