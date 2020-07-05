## MOVEM Move multiple registers

Operation 1 : REPEAT
[destination_register] ← [source]
UNTIL all registers in list moved

Operation 2: REPEAT
[destination] ← [source_register]
UNTIL all registers in list moved

Syntax 1: MOVEM <ea>,<register list>
Syntax 2: MOVEM <register list>,<ea>

## Sample syntax
```assembly
MOVEM.L D0-D7/A0-A6,$1234
MOVEM.L (A5),D0-D2/D5-D7/A0-A3/A6
MOVEM.W (A7)+,D0-D5/D7/A0-A6
MOVEM.W D0-D5/D7/A0-A6,-(A7)
```

## Attributes
Size = word, longword

## Description
The group of registers specified by <register list> is copied to
or from consecutive memory locations. The starting location is
provided by the effective address. Any combination of the 68000ís
sixteen address and data registers can be copied by a single MOVEM
instruction. Note that either a word or a longword can be moved,
and that a word is sign-extended to a longword when it is moved
(even if the destination is a data register).
When a group of registers is transferred to or from memory
(using an addressing mode other than pre-decrementing or post-
incrementing), the registers are transferred starting at the specified
address and up through higher addresses. The order of transfer
of registers is data register D0 to D7, followed by address register
A0 to A7.
For example, MOVEM.L D0-D2/D4/A5/A6,$1234 moves registers
D0,D1,D2,D4,A5,A6 to memory, starting at location $1234 (in
which D0 is stored) and moving to locations $1238, $123C,... Note
that the address counter is incremented by 2 or 4 after each move
according to whether the operation is moving words or
longwords, respectively.
If the effective address is in the pre-decrement mode (i.e.,
-(An)), only a register to memory operation is permitted. The
registers are stored starting at the specified address minus two
(or four for longword operands) and down through lower
addresses. The order of storing is from address register A7 to
address register A0, then from data register D7 to data register


```
D0. The decremented address register is updated to contain the
address of the last word stored.
If the effective address is in the post-increment mode (i.e.,
(An)+), only a memory to register transfer is permitted. The
registers are loaded starting at the specified address and up
through higher addresses. The order of loading is the inverse of
that used by the pre-decrement mode and is D0 to D7 followed
by A0 to A7. The incremented address register is updated to
contain the address of the last word plus two (or four for longword
operands).
Note that the MOVEM instruction has a side effect. An extra
bus cycle occurs for memory operands, and an operand at one
address higher than the last register in the list is accessed. This
extra access is an ëovershootí and has no effect as far as the
programmer is concerned. However, it could cause a problem if
the overshoot extended beyond the bounds of physical memory.
Once again, remember that MOVEM.W sign-extends words when
they are moved to data registers.
```
## Application
This instruction is invariably used to save working registers on
entry to a subroutine and to restore them at the end of a
subroutine.

BSR Example
.
.
Example MOVEM.L D0-D5/A0-A3,-(SP) Save registers
.
.
Body of subroutine
.
.
MOVEM.L (SP)+,D0-D5/A0-A3 Restore registers
RTS Return

## Condition codes
|X|N|Z|V|C|
|--|--|--|--|--|
|-|-|-|-|-|

## Source operand addressing modes
|Dn|An|(An)|(An)+|-(An)|(d,An)|(d,An,Xi)|ABS.W|ABS.L|(d,PC)|(d,PC,Xn)|imm|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|✓||✓|✓|✓|✓|✓|✓|✓|||| (memory to register)


## Destination operand addressing modes
|Dn|An|(An)|(An)+|-(An)|(d,An)|(d,An,Xi)|ABS.W|ABS.L|(d,PC)|(d,PC,Xn)|imm|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|✓||✓|✓|✓|✓|✓|✓|✓|||| (register to memory)