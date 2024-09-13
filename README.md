# Seven-Segment Hexadecimal Display Stopwatch

This firmware implements a stopwatch functionality for a PICOSOC RISC-V processor with a hardware multiplexed 7-segment display.

## Authors

- CEO: Frank Veldhuizen
- PM: Rodolfo Lopez
- Dev: Samuel Cacnio
- Tester: Tyson Aramaki

## Date

12/8/22

## Features

- Counts time in minutes and seconds
- Start/stop functionality
- Reset to zero
- Lap counter

## Hardware Interface

The firmware interacts with the following hardware components:

- 7-segment display for time output
- UP_DWN button for start/stop
- RESET button to reset the timer to zero
- LAP button to count laps

## GPIO Usage

The firmware uses GPIO for input and output:

- Input:

  - Bit 0: Second toggle (from display hardware)
  - Bit 1: UP_DWN button state
  - Bit 2: RESET button state
  - Bit 3: LAP button state

- Output:
  - Bits 0-3: Seconds ones digit
  - Bits 4-7: Seconds tens digit
  - Bits 8-11: Minutes ones digit
  - Bits 12-15: Lap count

## Main Loop

The main loop of the program:

1. Checks for button presses (UP_DWN, RESET, LAP)
2. Updates the timer if running
3. Handles overflow (resets to 0 after 9:59)
4. Updates the GPIO output with the current time and lap count

## Compilation

Compile this firmware using a RISC-V GCC toolchain. Ensure you're targeting the correct architecture (RV32I or RV32IM) as configured in the PICOSOC.

## Notes

- The timer increments on each second toggle from the display hardware
- The lap counter can count up to 15 laps (4-bit output)
- Time display wraps around from 9:59 to 0:00

## Acknowledgements

The starter code was written by Professor Chuck Pateros. Our team:

- Implemented START/STOP and the new time limit of 9:59 in `firmware.c`
- Added a laps count in `firmware.c`
- Implemented RESET, by looking at the corresponding signal in reg_gpio and set the timer and lap count to 0 when the wire was connected
- Used a boolean lap_counted so that the lap count would only increment once when the wire was connected and not per second when connected
- Changed reg_gpio to display the lap count in the leftmost digit
