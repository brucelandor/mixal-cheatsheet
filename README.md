# MIXAL CHEATSHEET

## Structure

```text
+------+------+------+------+-------+--------+
|   0  |  1   |   2  |   3   |   4  |   5    |
+------+------+------+------+-------+--------+
|  +/- | byte | byte | byte  | byte |  byte  |
+------+------+------+-------+------+--------+
|      address       | index |  mod | opcode |
+------+-------------+-------+------+--------+
```

`M = address + [rIi], i = index`  
`V = [M](mod)|(L:R), mod = L*8 + R`

1 byte = 6 bit  
1 word = 1 sign + 5 word

### Registers

* rA, 1 word
* rX, 1 word
* rJ, 2 bytes, positiVe
* rIi, i = [1|2|3|4|5|6], 1 sign + 2 byte

### Memory

* 4000 cell, 0~3999
* 1 cell = 1 word

### Other

1. OV, overflow toggle
2. CM, comparison indicator, L, E, G
3. I/O, deVices, un, n = [0~20]

## Operators

### Loading operators

> On partial loading, the sign is used if it is part of the fields, otherwise  
> \+ is used.

1. `LDA rA <- V`
2. `LDX rX <- V`
3. `LDi rIi <- V`
4. `LDAN rA <- -V`
5. `LDXN rX <- -V`
6. `LDiN rIi <- -V`

### Storing operators

> The number of bytes is taken from the right-hand of the register.

1. `STA rA -> M`
2. `STX rX -> M`
3. `STi rIi -> M` +/- 0 0 0 m n
4. `STJ rJ -> M` the sign is always \+, stores contents(rJ) into the address part of contents(M).
5. `STZ 0 -> M`

### Arithmetic operators

1. `ADD rA <- rA + V`
2. `SUB rA <- rA - V`
3. `MUL rAX <- rA * V`
4. `DIV rA <- rAX / V, rX <- reminder`

### Address transfer operators

> value of M, not [M] to register.

1. `ENTA rA <- M`
2. `ENTX rX <- M`
3. `ENTi rIi <- M`
4. `ENNA rA <- -M`
5. `ENNX rX <- -M`
6. `ENNi rIi <- -M`
7. `INCA rA <- rA + M`
8. `INCX rX <- rX + M`
9. `INCi rIi <- rIi +M`
10. `DECA rA <- rA - M`
11. `DECX rX <- rX - M`
12. `DECi rIi <- rIi - M`

### Comparison operators

> compare register with V, set CM.

1. `CMPA rA with V`
2. `CMPX rX with V`
3. `CMPi rIi with V`

### Jump operators

```code
JMP, JSJ, JOV, JNOV, JL, JE, JG, JGE, JNE, JLE.
JAN, JAZ, JAP, JANN, JANZ, JANP, JAE, JAO.
JZN, JZZ, JZP, JZNN, JZNZ, JZNP, JZE, JZO.
JiN, JiZ, JiP, JiNN, JiNZ, JiNP.
```

### Input-Output operators

> mod to identify io device.

1. `OUT M(mod)`
2. `IN M(mod)`
3. `IOC`
4. `JRED M(mod) jump to M if ready`
5. `JBUS M(mod) jump to M if busy`

### Conversion operators

1. `NUM rA <- rAX`
2. `CHAR rAX <- rA`

### Shift operators

> shift command does not affect the sign.  
> M specifies the number of MIX bytes to be shifted.

```code
SLA, SRA, SLAX, SRAX, SLC, SRC, SLB, SRB.
```

### Miscellaneous operators

1. `MOVE MOVE M(mod), Move MOD words from M to location stored in rI1`
2. `NOP`
3. `HLT` halt the execution.

## MIXAL assembly language

> In an instruction, (mod)|(L:R) choose the memory  
> cell fields to be operated on.

### MIXAL structure

```mixal
* comment
[LABEL] MNEMONIC [OPERAND] [COMMENT]
L   OPCODE M,I(MOD) comment
```

### MIXAL directive

1. `ORIG`
2. `EQU`
3. `CON`
4. `END` end the compilation.
5. `ALF`

### Expressions

1. w-expression
2. A stand-alone asterisk denotes the current memory location.
