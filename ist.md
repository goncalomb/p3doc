---
layout: default
title: At IST
---

# P3 CPU at IST #

At IST, the P3 is studied in a simulated environment or implemented on an FPGA.

The I/O devices are based on the (now retired) Digilent DIO5 Peripheral Board. They can also be simulated on the p3js and other simulators.

There are 256 addresses reserved for I/O devices (FF00h to FFFFh), only 15 are used.

## I/O Devices ##

Address | Direction | Device | Description
--------|-----------|--------|------------
FF00h to<br>FFEFh | | _Not Connected_ |
FFF0h | write | 7 segment display 1 | Controls the first digit of the 7 segment display (lower 4 bits)
FFF1h | write | 7 segment display 2 | Controls the second digit of the 7 segment display (lower 4 bits)
FFF2h | write | 7 segment display 3 | Controls the third digit of the 7 segment display (lower 4 bits)
FFF3h | write | 7 segment display 4 | Controls the fourth digit of the 7 segment display (lower 4 bits)
FFF4h | write | LCD (control) | Write control data to the LCD
FFF5h | write | LCD (data) | Write ASCII character to the LCD
FFF6h | read/write | Timer (value) | Timer value
FFF7h | read/write | Timer (status) | Timer start/stop
FFF8h | write | LEDs | Controls the 16 LEDs
FFF9h | read | Switches | Read the 8 switches (lower 8 bits)
FFFAh | read/write | Interrupt Mask | Interrupt mask for the first 16 interrupt vectors
FFFBh | | _Not Connected_ |
FFFCh | write | Terminal (cursor) | Controls the position of the cursor
FFFDh | read | Terminal (status) | Test for pending characters on the terminal
FFFEh | write | Terminal (write) | Write ASCII character to the terminal
FFFFh | read | Terminal (read) | Read ASCII character from the terminal

### 7 Segment Display ###

Addresses FFF0h, FFF1h, FFF2h and FFF3h are used to control the 7 segment display (lower 4 bits).

### LCD ###

The LCD (16 columns, 2 rows) is controlled by the addresses FFF4h and FFF5h.

The address FFF4h is a control address:

| Bit    | Action
|--------|-------
| 15     | Turn the LCD on/off
| 5      | Clear the LCD
| 4      | Set the cursor rows
| 3 to 0 | Set the cursor column

Writing to address FFF5h outputs an ASCII character at current cursor position. The cursor doesn't move automatically.

### Timer ###

The timer counts in real time intervals of 100ms. Writing to address FFF6h sets the current value of the timer (e.g. 20 = 2 seconds). The least significant bit of the address FFF7h controls the timer state (on/off).

The timer triggers interrupt 15.

### LEDs ###

Writing to address FFF8h controls the 16 available LEDs.

### Switches ###

Reading from address FFF9h fetches the state of the 8 switches (lower 8 bits).

### Terminal ###

The terminal screen is 80x24 characters in size and starts in scroll mode (the cursor move automatically when writing).

Writing to address FFFCh sets the position of the cursor on the screen. The lower 8 bits control the column (0 to 79) and the higher 8 bits control the line (0 to 23). Writing FFFFh to this address will clear the screen and change it to character mode (the cursor will stop moving automatically).

Reading from address FFFDh returns 1 if there is a pending key press to be handled and 0 otherwise.

Writing to address FFFEh outputs an ASCII character at current cursor position. The cursor moves automatically when the screen is in scroll mode.

Reading from address FFFFh returns the code for the last key press. Only one key is stored, subsequent reads will return 0 before another key is pressed.
