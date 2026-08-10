# firmware/

Arduino sketches for the Raspberry Pi Pico H (RP2040).

Planned sketches, in development order:

1. `usb_keyboard_test/` — verify USB HID enumeration and key output
2. `direct_switch/` — one direct switch (GP2 -> switch -> GND)
3. `matrix_test/` — 2x2 then 6x6 matrix scanning
4. `button_box/` — full firmware: matrix + behaviors + LED chain

Each sketch is a normal Arduino `.ino` folder.

Board setup in Arduino IDE:
- Board: Raspberry Pi Pico (via arduino-pico core: https://github.com/earlephilhower/arduino-pico)
- USB stack: "Pico SDK" (default) — Keyboard works out of the box
