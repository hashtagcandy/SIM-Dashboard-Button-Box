# ETS2 Sim Button Box

Custom Euro Truck Simulator 2 button box built around a Raspberry Pi Pico H (RP2040).

## Overview

- **Input**: 6x6 switch matrix (12 GPIO) -> ~31 logical controls
- **Output**: 23 indicator LEDs driven by 3x 74HC595 shift registers
- **Interface**: USB Keyboard HID to Windows, bound to ETS2 actions
- **Firmware**: Custom Arduino USB keyboard HID sketch

## Hardware inventory

| Hardware | Qty | Use |
|---|---|---|
| Raspberry Pi Pico H (RP2040) | 1 | Main controller (chosen brain) |
| SmartElex 74HC595 shift register breakout | 3 | LED drivers (24 outputs, 23 used) |
| Pro Micro (Type C, ATmega32U4) | 1 | Spare / test board |
| 3-position rotary selector | 2 | Wipers + Lights |
| Toggle switch | 6 | High beam, beacon, hazard, diff lock, axle, cruise |
| Push/pull switch | 2 | Parking brake, trailer brake |
| Rocker switch | 3 | Windows L/R, cruise speed |
| Latching push button | 3 | Trailer attach, dash mode, audio prev |
| Momentary push button | 5 | Horns, activate, wipers back |
| Ignition key switch | 1 | ACC / ON / START |
| Radio knob (encoder) | 1 | Volume, next favourite |

## Repo layout

```
SOUL.md        Project spec, GPIO map, matrix map, LED map, rules
prerequisites/ Planning docs: key mapping, layout designs, measurements
firmware/      Arduino sketches (Pico H)
docs/          Wiring diagrams, ETS2 bindings, measurement notes
```

## Prerequisites (planning stage)

| File | What it is |
|---|---|
| `prerequisites/01 - key to physical button mapping.md` | All 29 ETS2 functions mapped to physical controls + matrix positions |
| `prerequisites/02 - mapping table.csv` | Same mapping as CSV quick reference |
| `prerequisites/03 - panel layout design.md` | Design principles, LED placement, hole sizes |
| `prerequisites/04 - panel layout diagram.svg` | First layout visual (superseded by A/B/C) |
| `prerequisites/05 - layout A - classic dash.svg` | Layout option A (ignition bottom-left) |
| `prerequisites/06 - layout B - split console.svg` | Layout option B (ignition bottom-right) |
| `prerequisites/07 - layout C - cockpit cluster.svg` | Layout option C (ignition bottom-left) |
| `prerequisites/08 - layout comparison.md` | Compares the three layouts, recommendation |
| `prerequisites/09 - shopping list and BOM.md` | Bill of materials: owned / to buy / tools |
| `prerequisites/10 - keyboard key inventory.md` | Key list, counts, conflicts to fix |
| `prerequisites/11 - matrix design.md` | 6x6 matrix wiring design, scan scheme, ignition re-map |
| `prerequisites/12 - box measurements.md` | Panel size, hole sizes, all hole center coordinates (mm) |
| `prerequisites/13 - drilling template.svg` | Printable 1:1 drilling template with every hole |

## Current status

- [x] Key mapping proposed (29 functions -> physical controls)
- [x] Three panel layout options generated (A/B/C)
- [x] Matrix design drafted (scan scheme, wiring, ignition re-map approved)
- [x] Box measurements + 1:1 drilling template generated (650 x 342 mm panel)
- [ ] Layout chosen by user
- [ ] Panel size / enclosure confirmed
- [ ] Selector & ignition contacts measured with multimeter
- [ ] Firmware development

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
