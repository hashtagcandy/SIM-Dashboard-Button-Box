# ETS2 Button Box — Shopping List & Bill of Materials (BOM)

Everything needed for the build, split into:
1. Parts count at a glance
2. Already owned
3. To buy (electrical)
4. To buy (tools)
5. Decide later / optional

Quantities are based on the mapping in `01 - key to physical button mapping.md`.

---

## 0. Parts count at a glance

| # | Part | Qty |
|---|---|---|
| 1 | 3-position rotary selector | 2 |
| 2 | Toggle switch (on/off) | 6 |
| 3 | Push/pull switch | 2 |
| 4 | Rocker switch | 2 (3rd optional spare) |
| 5 | Momentary push button | 7 (5 owned + 2 to buy) |
| 6 | Latching push button (used as momentary) | 3 |
| 7 | Ignition key switch | 1 |
| 8 | Rotary encoder knob (EC11, with push for radio) | 2 (1 owned + 1 to buy) |
| 9 | 5 mm LED | 23 |
| 10 | 330 ohm resistor | 23+ |
| 11 | 0.1 uF capacitor | 3 |
| 12 | Raspberry Pi Pico H | 1 |
| 13 | 74HC595 breakout | 3 |

Total physical switch positions (matrix): 36 of 36 used.

---

## 1. Already owned — do NOT buy

| Qty | Item | Purpose |
|---|---|---|
| 1 | Raspberry Pi Pico H (RP2040, soldered headers) | Main brain |
| 3 | SmartElex 74HC595 shift register breakout | LED driver chain |
| 1 | Pro Micro (Type C, ATmega32U4) | Spare / test board |
| 2 | 3-position rotary selector | Wipers + Lights |
| 6 | Toggle switch (on/off) | High beam, beacon, hazard, diff lock, axle, cruise |
| 2 | Push/pull switch | Parking brake, trailer brake |
| 3 | Rocker switch (only 2 used) | Windows L/R + 1 spare |
| 3 | Latching push button | Momentary 6/7/8 bodies |
| 5 | Momentary push button | Horn, air horn, light horn, dashboard, activate |
| 1 | Ignition key switch | ACC / ON / START |
| 1 | Radio knob / encoder (EC11, if it clicks) | Volume + pause/play |
| 23 | 5 mm LEDs (assorted kit) | Indicator LEDs |
| 1 | Multimeter | Measuring selectors, continuity, testing |

---

## 2. To buy — electrical

| Qty | Item | Purpose / notes |
|---|---|---|
| 1 | EC11 rotary encoder with push button (cruise knob) | Cruise speed +/-, same as radio knob |
| 2 | Momentary push button (12 or 16 mm) | Momentary 9 (Infomaint) + 1 spare |
| 1 | Encoder knob cap (6 mm D-shaft, if not included) | Fits cruise knob |
| 23+ | Resistors 330 ohm (1/4 W) | One per LED; buy 50-pack, cheaper |
| 10+ | Resistors 100-150 ohm (1/4 W) | For blue/white LEDs at 3.3 V if too dim |
| 3 | Ceramic capacitor 0.1 uF | Decoupling next to each 74HC595 |
| 1 roll | Hookup wire, 22 AWG solid core | Matrix rows/columns, LEDs |
| 1 roll | Hookup wire, 22 AWG stranded | Flexible runs, jumpers |
| 1 | Heat-shrink tubing assortment | Insulating solder joints |
| 1 | 5V/3A USB power source (or use PC USB) | Pico power (3.3 V out for logic) |
| 1 | USB cable (USB-A/C to Pico Micro-USB) | Pico to PC |
| 10+ | Jumper wires male-male (dupont) | Breadboard testing phase |

Optional electrical (only if needed):
| Qty | Item | Purpose |
|---|---|---|
| 1 | Stripboard / protoboard (or perfboard) | Permanent soldered matrix instead of breadboard |
| 1 | Screw terminal block (e.g. 2x8) | Common ground bus + LED returns |
| 1 | 3D-printed / acrylic enclosure | Housing; decide with layout |
| 1 | Panel material (acrylic/aluminium/wood) | If no enclosure |

---

## 3. To buy — tools

| Qty | Item | Purpose |
|---|---|---|
| 1 | Soldering iron + solder (60/40 or lead-free) | Wiring the final build |
| 1 | Wire strippers + cutters | Prepping wire |
| 1 | Drill (or Dremel) | Mounting holes for switches |
| 1 set | Drill bits / step drill (3-22 mm) | 5 mm LEDs, 12 mm switches, 20 mm selectors |
| 1 | Digital caliper | Measuring hole centers, parts |
| 1 | Helping hands / third-hand tool | Soldering small assemblies |
| 1 | Multimeter (if not already owned) | Continuity + resistance checks |
| 1 | Screwdriver set (small Phillips + flat) | Mounting terminals, knobs |
| 1 | Hot glue gun + glue sticks | Cable relief, mounting LEDs |
| 1 | Desoldering pump / braid | Fixing mistakes |
| 1 | Small file / deburring tool | Cleaning drilled holes |

---

## 4. Decide later / optional

| Item | When |
|---|---|
| Enclosure + panel material | After choosing layout (A/B/C) |
| Panel labels (engraved/sticker/painted) | After layout + material choice |
| 3rd rocker (spare) | Only if panel has room |
| Extra EC11 knob caps (colored) | Cosmetic |
| Breadboard | Already part of test phase if you have one |

---

## Quick buy checklist (minimum to start building)

- [ ] 1x EC11 encoder (cruise knob)
- [ ] 2x momentary push button
- [ ] 50x 330 ohm resistors
- [ ] 10x 100-150 ohm resistors (blue/white LEDs)
- [ ] 3x 0.1 uF capacitors
- [ ] Wire: 22 AWG solid + stranded
- [ ] Heat-shrink assortment
- [ ] USB cable for Pico
- [ ] Soldering iron + solder
- [ ] Wire strippers
- [ ] Drill + step drill bits
- [ ] Digital caliper
- [ ] Helping hands

---

## Notes

- The 330 ohm starting value is per SOUL.md. At 3.3 V, blue/white LEDs
  (3.0-3.3 V forward voltage) may be dim — keep the 100-150 ohm pack handy.
- Matrix diodes (1N4148, ghosting protection) are SKIPPED by decision —
  the user does not expect to press many buttons simultaneously. If
  ghosting ever shows up, add them later (one diode per switch, 36 max).
- All quantities assume one button box. Double the resistor packs
  if you plan a second box later — bulk is cheaper.
