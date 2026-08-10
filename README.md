# ETS2 Sim Button Box

Custom Euro Truck Simulator 2 button box built around a Raspberry Pi Pico H (RP2040).

## Overview

- **Input**: 6x6 switch matrix (12 GPIO) -> ~31 logical controls
- **Output**: 23 indicator LEDs driven by 3x 74HC595 shift registers
- **Interface**: USB Keyboard HID to Windows, bound to ETS2 actions
- **Firmware**: Custom Arduino USB keyboard HID sketch

## Repo layout

```
SOUL.md        Project spec, GPIO map, matrix map, LED map, rules
firmware/      Arduino sketches (Pico H)
docs/          Wiring diagrams, ETS2 bindings, measurement notes
```

## Development order

1. USB keyboard test
2. One direct switch
3. 2x2 / four-switch matrix
4. Momentary vs toggle behavior
5. Three-position selector measurement and test
6. Full 6x6 matrix
7. One 74HC595 + 8 LEDs
8. Three 74HC595 + all 23 LEDs
9. Final ETS2 key map
10. Permanent wiring and enclosure

Read `SOUL.md` before changing hardware or firmware. It is the source of truth for pin maps and behavior rules.
