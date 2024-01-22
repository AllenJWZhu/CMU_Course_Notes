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
-- & Intersection 01000001 { 0, 6 }
-- | Union 01111101 { 0, 2, 3, 4, 5, 6 }
-- ^ Symmetric difference 00111100 { 2, 3, 4, 5 }
-- ~ Complement 10101010 { 1, 3, 5, 7 }
