# Digital Clock with Adjustable Time and Alarm (FPGA – DE10-Lite)

This project implements a **digital clock with an adjustable time and alarm** on the **Terasic DE10-Lite (Intel MAX10)** FPGA board using Verilog.

The design shows the current time in `HH:MM:SS` on the 7‑segment displays, supports user time adjustment, and includes a configurable alarm with visual indication on the LEDs.

## Features

- **Real-time clock**
  - BCD counters for seconds, minutes, and hours (24-hour format).
  - Uses the 50 MHz on-board clock and a frequency divider to generate a 1 Hz tick.

- **Time adjustment**
  - Switches select which field to adjust (seconds / minutes / hours).
  - Push-button increments the selected field.
  - Clock **holds** while any adjust switch is active, then resumes when released.

- **Alarm**
  - Separate BCD registers store the alarm time (HH:MM:SS).
  - Alarm set mode: display shows alarm time instead of current time.
  - When current time matches alarm time, an alarm event is triggered.
  - **LEDs blink** to indicate the alarm.
  - Auto-shutoff timer (e.g., 20 seconds) plus manual clear button.

- **Robust input handling**
  - Edge-detected (debounced) increment button to avoid multiple counts per press.
  - Synchronous, single-clock design following clean `_Q` / `_D` register style.

## Hardware

- Board: **Terasic DE10-Lite (Intel MAX10 FPGA)**
- Clock: 50 MHz oscillator (`MAX10_CLK1_50`)
- Inputs:
  - `SW[0]–SW[2]`: select seconds / minutes / hours for adjustment
  - `SW[3]`: alarm set mode (show & edit alarm time)
  - `SW[9]`: tick speed select (normal vs fast, for testing)
  - `KEY[1]`: reset (active low)
  - `KEY[0]`: increment / clear alarm (active low)
- Outputs:
  - `HEX0–HEX5`: 7-segment displays for HH:MM:SS
  - `LEDR[9:0]`: blinking LEDs when alarm is active

## File Structure

```text
src/
  DE10_12_3.v   # Top-level module with clock, time adjust, and alarm logic
  Seg.v         # 7-segment display decoder (hex to segments)

docs/           # Presintation 
