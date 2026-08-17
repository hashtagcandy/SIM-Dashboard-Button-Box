# ETS2 Button Box — Box & Panel Measurements

Measurements derived from the chosen layout (`genlayout.png` — "LATEST KEY
SET") and the hole-size conventions in `03 - panel layout design.md`.

Companion file: `13 - drilling template.svg` — printable 1:1 template with
every hole center marked (mm from top-left). Print at 100% scale, no scaling.

---

## 1. Panel outer size

| Dimension | Value |
|---|---|
| Width | 650 mm |
| Height | 342 mm |
| Aspect | ~1.9 : 1 landscape |
| Suggested material | 4-6 mm acrylic / aluminium / wood panel |
| Edge margin | 25 mm around all controls |

Orientation: TOP edge = far edge (display side), BOTTOM edge = front edge
toward the user. Ignition key sits at the bottom-LEFT corner, brakes on the
bottom row — matches the chosen layout.

---

## 2. Hole sizes (drill chart)

| Control | Qty | Hole / cutout |
|---|---:|---|
| 3-position rotary selector | 2 | 20 mm round |
| Toggle switch | 6 | 12 mm round |
| Push/pull switch | 2 | 12 mm round |
| Rocker switch | 2 | 19 x 13 mm rectangle |
| Momentary push button | 7 | 12 mm round (16 mm if LED-ring) |
| Latching button (as momentary) | 3 | 12 mm round (16 mm if LED-ring) |
| Ignition key switch | 1 | 19 mm round (up to 22 mm for the key barrel) |
| EC11 encoder (cruise, radio) | 2 | 10 mm round (6 mm shaft) |
| Indicator LED | 23 | 5 mm round |

Spacing rule satisfied: every control center is >= 25 mm from every other
center (verified programmatically). Finger/knob collisions are avoided.

---

## 3. Control hole centers (mm from panel top-left)

Coordinates are the CENTER of each hole. X grows right, Y grows down
(toward the user).

| Control | X | Y | Hole |
|---|---:|---:|---|
| LIGHT MODES selector | 35 | 48 | 20 round |
| WIPERS selector | 35 | 113 | 20 round |
| WIPERS BACK | 35 | 178 | 12 round |
| DISPLAY MODE (M4) | 35 | 233 | 12 round |
| IGNITION | 35 | 308 | 19 round |
| HIGH BEAM (T1) | 130 | 48 | 12 round |
| BEACON (T2) | 185 | 48 | 12 round |
| HAZARD (T3) | 240 | 48 | 12 round |
| DIFFERENTIAL (T4) | 295 | 48 | 12 round |
| AXLE (T5) | 350 | 48 | 12 round |
| CRUISE (T6) | 405 | 48 | 12 round |
| HORN (M1) | 530 | 48 | 12 round |
| AIR HORN (M2) | 530 | 113 | 12 round |
| LIGHT HORN (M3) | 530 | 178 | 12 round |
| LEFT WINDOW (R1) | 615 | 48 | 19 x 13 rect |
| RIGHT WINDOW (R2) | 615 | 123 | 19 x 13 rect |
| ACTIVATE (M5) | 130 | 233 | 12 round |
| TRAILER ATTACH (M6) | 185 | 233 | 12 round |
| INFOMAINT (M9) | 240 | 233 | 12 round |
| CRUISE KNOB (EC11) | 315 | 233 | 10 round |
| RADIO KNOB (EC11) | 400 | 233 | 10 round |
| AUD PREV (M8) | 465 | 233 | 12 round |
| AUD NEXT (M7) | 520 | 233 | 12 round |
| PARKING BRAKE (PP1) | 195 | 308 | 12 round |
| TRAILER BRAKE (PP2) | 260 | 308 | 12 round |

---

## 4. LED hole centers (5 mm)

LEDs sit 15 mm above their control (or beside, for brakes/ignition).

| LED # | Function | X | Y |
|---|---:|---|---:|---:|
| 1 | Wiper LEFT | 25 | 33 |
| 2 | Wiper CENTER | 35 | 33 |
| 3 | Wiper RIGHT | 45 | 33 |
| 4 | Lights state 1 | 25 | 98 |
| 5 | Lights state 2 | 35 | 98 |
| 6 | Lights state 3 | 45 | 98 |
| 7 | High beam | 130 | 28 |
| 8 | Beacon | 185 | 28 |
| 9 | Hazard | 240 | 28 |
| 10 | Diff lock | 295 | 28 |
| 11 | Axle | 350 | 28 |
| 12 | Cruise | 405 | 28 |
| 13 | Park brake | 180 | 293 |
| 14 | Trailer brake | 245 | 293 |
| 15 | Wipers back | 35 | 158 |
| 16 | Display mode | 35 | 213 |
| 17 | Activate | 130 | 213 |
| 18 | Trailer attach | 185 | 213 |
| 19 | Infomaint | 240 | 213 |
| 20 | Horn | 530 | 28 |
| 21 | Air horn | 530 | 93 |
| 22 | Light horn | 530 | 158 |
| 23 | Ignition | 35 | 288 |

Total: 23 LEDs = 3x 74HC595 capacity (24 outputs, 1 spare).

---

## 5. Box depth / enclosure

| Component | Depth needed |
|---|---|
| Toggle / push button body | ~20-30 mm behind panel |
| 74HC595 breakouts + wiring | ~15-20 mm |
| Pico H | ~15 mm |
| Total internal depth | ~50-60 mm |

Suggested enclosure: panel + box of 60-80 mm internal depth, with 3-5 mm
standoffs for the Pico and 74HC595 boards. Add cable exit at the back or
bottom.

---

## 6. How to use the drilling template

1. Print `13 - drilling template.svg` at 100% scale (check printer scaling
   is OFF).
2. Confirm the printed panel outline measures exactly 650 x 342 mm.
3. Tape the print to the panel material.
4. Center-punch each red circle/rectangle.
5. Drill: pilot holes first, then step-drill to final size
   (5 / 10 / 12 / 19-20 mm; rockers need a 19 x 13 mm slot — drill two
   13 mm holes at the ends and file/jigsaw between).
6. LEDs are the orange circles (5 mm).
7. All coordinates in the tables above are mm from the panel top-left —
   measure twice, drill once.

---

## 7. Open questions

1. Do the momentary buttons have LED rings (16 mm) or plain (12 mm)?
   Affects those hole sizes.
2. Panel material and thickness (acrylic / aluminium / wood / 3D print)?
3. Ignition key barrel diameter — 19 or 22 mm? Measure before drilling.
4. Enclosure: 3D printed box, ready-made project box, or wood frame?
5. Want the panel scaled down (e.g. 500 mm wide) if 650 mm is too big
   for the desk? All coordinates scale linearly.
