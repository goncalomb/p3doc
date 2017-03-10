---
layout: default
title: Technical Description
---

# P3 CPU Technical Description #

The P3 is a 16-bit CPU with 16-bit address and data bus.

## Specifications ##

* 16-bit architecture (16-bit address and data bus)
* CISC design (microcode)
* 8 general purpose registers
* 45 instructions (default)
* 4 addressing modes
* 1 interrupt signal

## Registers ##

The P3 has 8 general purpose registers, R0-R7. R0 is hardwired to always contain 0x0000 (cannot the changed).

Two more registers are available to the programmer, SP (stack pointer) and PC (program counter).

## Opcodes ##

The opcodes are 6-bit long.

|          | x000  | x001  | x010  | x011  | x100  | x101  | x110  | x111  |
|----------|-------|-------|-------|-------|-------|-------|-------|-------|
| **000x** | NOP   | ENI   | DSI   | STC   | CLC   | CMC   | RET   | RTI   |
| **001x** | INT   | RETN  |       |       |       |       |       |       |
| **010x** | NEG   | INC   | DEC   | COM   | PUSH  | POP   |       |       |
| **011x** | SHR   | SHL   | SHRA  | SHLA  | ROR   | ROL   | RORC  | ROLC  |
| **100x** | CMP   | ADD   | ADDC  | SUB   | SUBB  | MUL   | DIV   | TEST  |
| **101x** | AND   | OR    | XOR   | MOV   | MVBH  | MVBL  | XCH   |       |
| **110x** | JMP   | JMP.  | CALL  | CALL. |       |       |       |       |
| **111x** | BR    | BR.   |       |       |       |       |       |       |

## Interrupts ##

The P3 has only one external interrupt signal (INT). It uses "interrupt acknowledge" to handle interrupts, the interrupt number is read from the data bus (lower 8 bits, 256 interrupts). Check the book for more information.

## Memory ##

With a 16-bit address bus, the P3 can directly access 64K 16-bit words of memory.

Addresses from 0xFE00 to 0xFEFF are reserved for the interrupt vector table.

Addresses from 0xFF00 to 0xFFFF are reserved for memory mapped I/O devices.

## ROMs ##

The P3 has 3 internal ROMs that control the CPU at the microcode level. Check the book for more information.

### ROM A (64 x 9 bits) ###

ROM A maps opcodes to microcode addresses on ROM C. This ROM selects the microcode that runs for a particular instruction.

### ROM B (16 x 9 bits) ###

ROM B maps addressing modes to microcode addresses on ROM C. This ROM selects the microcode that is used to fetch/store operands for a particular addressing mode.

### ROM C (512 x 32 bits) ###

ROM C (Control ROM) contains all the microcode (instructions, operand fetch/store and interrupt handling).
