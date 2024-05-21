# F8ful CPU

The F8ful CPU is an 8-bit homebrew CPU inspired by both Ben Eater and JDH's homebrew CPUs.
(Despite the name, it is surprisingly not based on the [Fairchild F8](https://en.wikipedia.org/wiki/Fairchild_F8))

## Features
* 8-bit data width
* 16-bit address bus (64KiB of available memory)
* 8 general purpose registers (6 normal + 2 indirect address)
* MMIO for device communication
* 16 instruction RISC achitecture

## Instruction Set:
```asm
0 : add  reg, reg/imm8   => reg = reg + reg/imm8
1 : sub  reg, reg/imm8   => reg = reg - reg/imm8
2 : adc  reg, reg/imm8   => reg = reg + reg/imm8 + C
3 : sbb  reg, reg/imm8   => reg = reg - reg/imm8 - C
4 : nand reg, reg/imm8   -> reg = ~(reg & reg/imm8)
5 : or   reg, reg/imm8   -> reg = reg | reg/imm8
6 : cmp  reg, reg/imm8   => G = reg > reg/imm8, E = reg == reg/imm8, L = reg < reg/imm8
7 : mv   reg, reg/imm8   -> reg = reg/imm8
8 : ld   reg, [HL/imm16] -> reg = [HL/imm16]
9 : st   [HL/imm16], reg -> [HL/imm16] = reg
A : lda  [imm16]         -> HL = imm16
B : lpm  reg, [HL/imm16] -> reg = PROGMEM[HL/imm16]
C : push reg/imm8        -> [SP--] = reg/imm8
D : pop  reg             -> reg = [++SP]
E : jnz  reg/imm8        -> reg/imm8 != 0 ? PC = HL : NOP
F : halt                 => H = 1

=> Modify the Status Register
```

## MMIO
The top 64 locations in memory, from 0xFFC0-0xFFFF are reserved for internal or external devices.
There are a few of these built into the processor itself:

* 0xFFFF : Status Register
* 0xFFFE : Stack Pointer High
* 0xFFFD : Stack Pointer Low
