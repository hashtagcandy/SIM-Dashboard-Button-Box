# SOUL.md — DIY ETS2 Sim Button Box

## Project identity

This is a custom **Euro Truck Simulator 2 (ETS2) sim button box** built around a **Raspberry Pi Pico H / RP2040**.

Primary goals:
- USB keyboard HID output to Windows.
- ETS2 controls assigned to keyboard keys.
- Use a switch matrix so GPIO is not exhausted.
- Use 74HC595 shift registers for indicator LEDs.
- Toggle/latching controls send **one key tap per state change**, not a continuously held/repeating key.
- Momentary controls can behave as true held keys.
- Three-position selectors report their actual selected state.
- Keep hardware, GPIO mapping, firmware behavior, and ETS2 mapping documented.

---

## Current hardware

### Controller
- Raspberry Pi Pico H / RP2040
- Male headers are already soldered.
- Arduino IDE is being used.
- Planned firmware: custom Arduino USB keyboard HID.
- GP2040-CE was tested, but the project is moving to keyboard firmware.
- BOOTSEL is the Pico bootloader button, not an ordinary GPIO input.

### Confirmed tests
- Direct switch: `GP2 -> switch -> GND` works.
- GP3 also works.
- GP0/GP1 are not permanently unusable hardware pins; they are ordinary RP2040 GPIOs, but this project intentionally leaves them unused for simplicity.
- GP25 is the Pico onboard LED GPIO and is intentionally left unused in the main matrix.

---

## Physical control inventory

| Hardware | Quantity | Logical inputs |
|---|---:|---:|
| 3-position rotary selector | 2 | 3 states each; exact electrical encoding must be measured |
| Latching push button | 3 | 3 |
| Momentary push button | 5 | 5 |
| Toggle switch | 6 | 6 |
| Push/pull switch | 2 | 2 |
| Rocker switch | 3 | 6 if both directions are independently used |
| Ignition key | 1 | up to 3 contacts/states |
| Radio knob/encoder | 1 | normally 2 encoder signals; add one more input if it has push-to-click |
| **Approximate total** | | **~31 logical inputs** |

The exact ignition and 3-position selector contacts must be verified with a multimeter before final wiring.

---

## Indicator LEDs

Planned LEDs:

| Control | LEDs |
|---|---:|
| Wiper 3-position selector | 3 |
| Light 3-position selector | 3 |
| Six toggle switches | 6 |
| Two push/pull switches | 2 |
| Ignition key | 1 |
| Eight push buttons | 8 |
| Rockers | 0 |
| Radio knob | 0 |
| **Total** | **23** |

LEDs are from the user's assorted 5 mm LED kit.

### LED rule

A bare 5 mm LED needs its own current-limiting resistor.

Starting value:

`330 ohm per LED`

Do not share one resistor between independent LEDs.

Basic wiring:

```text
74HC595 output -> 330R -> LED anode
LED cathode -> GND
```

---

# Input architecture — 6x6 matrix

A 6x6 matrix gives:

```text
6 rows + 6 columns = 12 GPIO
6 x 6 = 36 matrix positions
```

The project needs about 31 logical switch positions, leaving five spare positions.

### Matrix rule

Each switch connects a row to a column:

```text
ROW GPIO ---- SWITCH ---- COLUMN GPIO
```

Matrix switches do **not** connect directly to GND.

The firmware scans one row at a time and reads the columns.

### Important ghosting note

A bare switch matrix can suffer from ghosting/masking when multiple switches are pressed simultaneously. This may matter in a sim panel where several controls can be active.

If ghosting occurs, add one diode per matrix switch (commonly 1N4148) and implement diode-aware scanning. Do not assume a bare matrix is automatically ghost-free.

DECISION (recorded): initial build will use a bare matrix with NO diodes. The user does not expect to press many switches simultaneously. If ghosting appears in testing, add diodes later (one per switch, 36 max).

---

# Pico GPIO map

## Matrix rows

| GPIO | Assignment |
|---|---|
| GP2 | Row 1 |
| GP3 | Row 2 |
| GP4 | Row 3 |
| GP5 | Row 4 |
| GP6 | Row 5 |
| GP7 | Row 6 |

## Matrix columns

| GPIO | Assignment |
|---|---|
| GP8 | Column 1 |
| GP9 | Column 2 |
| GP10 | Column 3 |
| GP11 | Column 4 |
| GP12 | Column 5 |
| GP13 | Column 6 |

This consumes 12 GPIO.

---

# Logical matrix map

This is the proposed starting map. It should be treated as a logical map until the user confirms the final physical layout.

| Matrix | Function |
|---|---|
| R1C1 | Wiper position 1 |
| R1C2 | Wiper position 2 |
| R1C3 | Wiper position 3 |
| R1C4 | Lights position 1 |
| R1C5 | Lights position 2 |
| R1C6 | Lights position 3 |
| R2C1 | High beam |
| R2C2 | AUX |
| R2C3 | Beacon |
| R2C4 | Toggle spare / final assignment |
| R2C5 | Toggle spare / final assignment |
| R2C6 | Toggle spare / final assignment |
| R3C1 | Park brake push/pull |
| R3C2 | Trailer air push/pull |
| R3C3 | Latching push 1 |
| R3C4 | Latching push 2 |
| R3C5 | Latching push 3 |
| R3C6 | Momentary push 1 |
| R4C1 | Momentary push 2 |
| R4C2 | Momentary push 3 |
| R4C3 | Momentary push 4 |
| R4C4 | Momentary push 5 |
| R4C5 | Rocker 1 direction A |
| R4C6 | Rocker 1 direction B |
| R5C1 | Rocker 2 direction A |
| R5C2 | Rocker 2 direction B |
| R5C3 | Rocker 3 direction A |
| R5C4 | Rocker 3 direction B |
| R5C5 | Ignition ACC |
| R5C6 | Ignition IGN |
| R6C1 | Ignition START |
| R6C2 | Radio encoder A |
| R6C3 | Radio encoder B |
| R6C4 | Spare |
| R6C5 | Spare |
| R6C6 | Spare |

### Three-position selector warning

The user's selector has four screw terminals. Do not assume the terminal numbers alone tell us the three states.

Before firmware mapping, measure:

```text
LEFT   -> connected contacts
CENTER -> connected contacts
RIGHT  -> connected contacts
```

Then map those actual combinations.

---

# Switch behavior

## Momentary

Desired:

```text
pressed  -> Keyboard.press(key)
released -> Keyboard.release(key)
```

Useful for controls where holding the key matters, such as a horn if desired.

## Toggle/latching

Desired:

```text
OFF -> ON  : send one key tap
ON  -> OFF : send one key tap
```

Do **not** continuously call `Keyboard.press()` for a toggle.

Use state-change detection and:

```cpp
Keyboard.write(key);
```

once per transition.

Example:

```text
Toggle ON:
H

Toggle stays ON:
nothing

Toggle OFF:
H
```

This prevents `HHHHHHHHHH...` or similar repeated characters.

## Push/pull

Treat according to the ETS2 function:
- state-based function -> one tap on state change
- hold-based function -> press/release

## Three-position selector

Treat it as a mutually exclusive state machine:

```text
LEFT
CENTER
RIGHT
```

Send a command only when the selected state changes.

---

# LED architecture — 74HC595

23 LEDs are planned.

Use:

```text
3 x 74HC595
```

Each provides 8 outputs:

```text
3 x 8 = 24 outputs
```

So 23 LEDs fit with one spare output.

## Pico -> 74HC595

Proposed:

```text
GP14 -> DATA / SER
GP15 -> CLOCK / SRCLK
GP16 -> LATCH / RCLK
```

Shared by all three chips:

```text
CLOCK
LATCH
VCC
GND
```

Serial chain:

```text
Pico DATA
   |
   v
595 #1 -> 595 #2 -> 595 #3
          Q7'       Q7'
```

Q7' of each chip goes to SER of the next chip.

### Power

```text
VCC  -> 3.3 V
GND  -> common GND
OE   -> GND
SRCLR -> 3.3 V
```

Add a 0.1 uF ceramic decoupling capacitor close to each 74HC595 if available.

### Current

The 74HC595 is not a high-current LED driver. Keep LED current conservative. Around 2–5 mA per indicator is a good starting point. If many LEDs must be very bright simultaneously, use a dedicated LED driver or transistor stage instead of heavily loading the 74HC595.

---

# LED numbering

Suggested firmware numbering:

| LED # | Function |
|---:|---|
| 1 | Wiper LEFT |
| 2 | Wiper CENTER |
| 3 | Wiper RIGHT |
| 4 | Lights state 1 |
| 5 | Lights state 2 |
| 6 | Lights state 3 |
| 7 | Toggle 1 |
| 8 | Toggle 2 |
| 9 | Toggle 3 |
| 10 | Toggle 4 |
| 11 | Toggle 5 |
| 12 | Toggle 6 |
| 13 | Park brake |
| 14 | Trailer air |
| 15 | Latching push 1 |
| 16 | Latching push 2 |
| 17 | Latching push 3 |
| 18 | Momentary push 1 |
| 19 | Momentary push 2 |
| 20 | Momentary push 3 |
| 21 | Momentary push 4 |
| 22 | Momentary push 5 |
| 23 | Ignition |

Exact labels can be finalized later.

---

# Ground distribution

Use a common ground bus/terminal block for:

- Pico GND
- 74HC595 GND
- LED cathodes
- other low-voltage ground-referenced circuits

However:

**Matrix switch contacts do not connect to the common GND bus.**

Matrix:

```text
ROW -> switch -> COLUMN
```

LED:

```text
74HC595 -> resistor -> LED -> GND
```

---

# USB keyboard firmware

The firmware is custom Arduino code for RP2040 USB HID.

The software must separate:

### Momentary

```text
pressed  -> key down
released -> key up
```

### Toggle/latching

```text
state changed -> one Keyboard.write()
```

### Selector

```text
state changed -> command for the new state
```

Never repeatedly emit a keyboard key just because a latching switch remains ON.

---

# ETS2 key-binding philosophy

Do not treat old/default keyboard assignments as immutable.

ETS2 supports customizable keyboard/controller bindings and advanced input configuration. Its official input documentation describes `controls.sii`, controller inputs, keyboard inputs, mixes, and selector-dependent configurations.

The firmware should therefore have a user-editable key map.

A commonly documented ETS2-style starting point is:

| Function | Common key |
|---|---|
| Engine start/stop | E |
| Parking brake | SPACE |
| Engine brake | B |
| Lights mode | L |
| High beam | K |
| Horn | H |
| Wipers | P |
| Cruise control | C |
| Dashboard display mode | I |
| Trailer connect/disconnect | T |
| World map | M |
| Radio/audio player | R |
| Left indicator | [ |
| Right indicator | ] |
| Hazard lights | F |

These are a starting point only. The user's current ETS2 profile should be treated as the source of truth.

ETS2 also supports more advanced custom input configuration in `controls.sii`.

---

# Suggested physical control functions

## 3-position selectors

### WIPER
Potential states:
- OFF/intermittent
- normal
- fast

The exact ETS2 commands available should be verified in the current Controls menu.

### LIGHT
Potential states:
- OFF/position
- low/normal
- high/main

Verify the exact current ETS2 actions before final firmware.

## Toggles

Planned examples:
- High beam
- AUX
- Beacon
- additional truck functions

Remaining toggle assignments should be finalized from the user's intended ETS2 functions.

## Push/pull

- Parking brake
- Trailer air / trailer-related function

## Push buttons

Planned functions include:
- Horn
- Cruise
- Activate
- Display information
- Dashboard
- additional functions to be finalized

## Rockers

Planned functions include:
- Window left/right
- Retarder
- Differential lock

## Ignition

Physical states:

```text
ACC
IGN
START
```

Exact contacts must be measured.

## Radio knob

Planned behavior:

```text
turn left  -> volume down
turn right -> volume up
press      -> play/pause
```

If the encoder has a push switch, reserve one additional matrix position.

---

# Development order

1. **USB keyboard test**
2. **One direct switch**
3. **2x2 / four-switch matrix**
4. **Momentary vs toggle behavior**
5. **Three-position selector measurement and test**
6. **Full 6x6 matrix**
7. **One 74HC595 + 8 LEDs**
8. **Three 74HC595 + all 23 LEDs**
9. **Final ETS2 key map**
10. **Permanent wiring and enclosure**

Do not permanently mount or solder everything until each stage works.

---

# Debugging rules

### Random keyboard characters
Check:
- floating inputs
- pull-up/pull-down configuration
- row/column scanning
- incorrect switch wiring
- debounce
- unintended simultaneous keys

### Toggle repeats
Firmware is probably holding the key.

Use transition detection + `Keyboard.write()`.

### Matrix ghosting
If multiple switches create a false key:
- add per-switch diodes
- verify scan direction
- verify inactive rows are in a safe state
- verify the firmware scans only one active row at a time

### LED problems
Check:
- common GND
- 74HC595 VCC
- OE
- SRCLR
- clock
- latch
- serial chain
- resistor per LED
- decoupling capacitors

### Selector position wrong
Measure the selector with a multimeter before changing firmware.

---

# Current project status

Confirmed:
- Pico H works.
- USB works.
- GP2040-CE was tested.
- GP2 works as an input.
- GP3 works as an input.
- Direct switch wiring works.
- Project direction is Arduino USB keyboard firmware.
- 6x6 switch matrix selected.
- 3 x 74HC595 selected for LEDs.
- 23 indicator LEDs planned.

Not yet finalized:
- exact physical-to-matrix mapping
- exact 3-position selector contact mapping
- final ETS2 key assignments
- LED current/brightness
- whether matrix diodes are required
- exact encoder wiring
- final permanent harness

---

# Rules for future development

1. Do not change the matrix architecture without a specific reason.
2. Measure the 3-position selector; never infer its contacts.
3. A toggle is not a held keyboard key.
4. Separate momentary, toggle, and selector logic.
5. Every bare LED gets its own resistor.
6. Do not overload 74HC595 outputs.
7. Matrix switches do not connect directly to the common GND bus.
8. Keep the GPIO map stable once physical wiring starts.
9. Test small circuits before expanding.
10. Keep ETS2 key assignments editable.
11. Do not assume community/default ETS2 bindings are unchanged.
12. Update this file before major hardware changes.
13. When changing a pin, update both the pin map and firmware.
14. Do not permanently install a component until its electrical behavior has been tested.

---

# Primary objective

The final device should behave like a purpose-built truck dashboard:

```text
Physical switch
      |
      v
6x6 matrix
      |
      v
RP2040
      |
      +---- USB Keyboard HID ----> Windows ----> ETS2
      |
      +---- 74HC595 chain ----> 23 indicator LEDs
```

Target behavior:

```text
Momentary = hold/release
Toggle    = one tap per state change
Selector  = explicit state
Encoder   = directional events
LED       = physical state indication
```

The objective is not merely to make every switch output a key. The objective is to make the panel behave like a coherent ETS2 truck control console.
