# ETS2 Button Box — Key to Physical Button Mapping

Source file: `rough idea of keys.txt` (29 functions)
Control names verified against: `ATS and ETS2 Key Bindings.xlsx`

Status: PROPOSAL — nothing is soldered yet. Verify keys in the ETS2
Controls menu before finalizing (column "Key" = suggested default only).

---

## 1. Physical controls available

| # | Control | Qty | Notes |
|---|---|---|---|
| A | 3-position rotary selector | 2 | Wiper + Lights (3 states each) |
| B | Toggle switch (on/off) | 6 | stays in position |
| C | Push/pull switch | 2 | pull = on, push = off |
| D | Rocker switch | 3 | spring back to center, 2 directions |
| E | Latching push button | 3 | stays in when pressed |
| F | Momentary push button | 5 | returns when released |
| G | Ignition key switch | 1 | OFF / ACC / ON (+START if present) |
| H | Radio knob / encoder | 1 | rotate CW/CCW + push if it clicks |

Total matrix positions needed: 31 (of 36 available).

---

## 2. The mapping

### Selectors (3-position, one tap per state change)

| Matrix | Control | ETS2 function | Behavior |
|---|---|---|---|
| R1C1 | Wiper selector pos 1 | Wipers OFF | tap key to reach OFF |
| R1C2 | Wiper selector pos 2 | Wipers NORMAL | tap key to reach NORMAL |
| R1C3 | Wiper selector pos 3 | Wipers FAST | tap key to reach FAST |
| R1C4 | Light selector pos 1 | Lights OFF | tap key to reach OFF |
| R1C5 | Light selector pos 2 | Lights PARKING | tap key to reach PARKING |
| R1C6 | Light selector pos 3 | Lights LOW BEAM | tap key to reach LOW BEAM |

NOTE: ETS2 "Wipers" and "Light Modes" are CYCLE keys (one press = next
state). Firmware must count taps: off->normal = 1 tap, off->fast = 2 taps,
fast->off = 3 taps, etc. The box tracks the last known state, so the
physical knob always lands on the same in-game state.

### Toggles (6) — one tap per state change

| Matrix | Control | ETS2 function | Suggested key | Verify |
|---|---|---|---|---|
| R2C1 | Toggle 1 | High Beam Headlights | K | yes |
| R2C2 | Toggle 2 | Beacon | U | yes |
| R2C3 | Toggle 3 | Hazard Warning | F | yes |
| R2C4 | Toggle 4 | Differential Lock | D | yes |
| R2C5 | Toggle 5 | Lift / Drop Axle | J | yes |
| R2C6 | Toggle 6 | Cruise Control | C | yes |

### Push/pull (2) — one tap per state change

| Matrix | Control | ETS2 function | Suggested key | Verify |
|---|---|---|---|---|
| R3C1 | Push/pull 1 | Parking Brake | SPACE | yes |
| R3C2 | Push/pull 2 | Trailer Brake | 7 | yes |

### Latching push (3) — one tap per state change

| Matrix | Control | ETS2 function | Suggested key | Verify |
|---|---|---|---|---|
| R3C3 | Latching 1 | Trailer Attach / Detach | T | yes |
| R3C4 | Latching 2 | Dashboard Display Mode | I | yes |
| R3C5 | Latching 3 | Audio Player Previous Favourite | N | yes |

### Momentary (5) — hold while pressed

| Matrix | Control | ETS2 function | Suggested key | Verify |
|---|---|---|---|---|
| R3C6 | Momentary 1 | Horn | H | yes |
| R4C1 | Momentary 2 | Air Horn | V | yes |
| R4C2 | Momentary 3 | Light Horn | B | yes |
| R4C3 | Momentary 4 | Activate | ENTER | yes |
| R4C4 | Momentary 5 | Wipers Back | O | yes |

### Rockers (3, 6 directions) — hold while pressed

| Matrix | Control | ETS2 function | Suggested key | Verify |
|---|---|---|---|---|
| R4C5 | Rocker 1 up | Open Left Window | Q | yes |
| R4C6 | Rocker 1 down | Close Left Window | A | yes |
| R5C1 | Rocker 2 up | Open Right Window | W | yes |
| R5C2 | Rocker 2 down | Close Right Window | S | yes |
| R5C3 | Rocker 3 up | Cruise Speed Increase | = | yes |
| R5C4 | Rocker 3 down | Cruise Speed Decrease | - | yes |

### Ignition key (3 positions)

| Matrix | Control | ETS2 function | Behavior |
|---|---|---|---|
| R5C5 | Key ACC | Start / Stop Engine Electricity | tap on entering ACC |
| R5C6 | Key ON | Start / Stop Engine | tap on entering ON |
| R6C1 | Key START | (engine crank, if present) | momentary |

### Radio knob (encoder)

| Matrix | Control | ETS2 function | Behavior |
|---|---|---|---|
| R6C2 | Knob CW | Audio Player Volume Up | one tap per detent |
| R6C3 | Knob CCW | Audio Player Volume Down | one tap per detent |
| R6C4 | Knob push | Audio Player Next Favourite | tap on press (if push exists) |

### Spare (now assigned — needs 2 extra physical buttons)

| Matrix | Control | ETS2 function | Behavior |
|---|---|---|---|
| R6C5 | NEW button 1 | Audio Player pause/play | tap on press (momentary recommended) |
| R6C6 | NEW button 2 | Infomaint display mode | tap on press (momentary recommended) |

NOTE: these two positions need physical switches that are NOT in the
original inventory. DECIDED: user will buy 2 extra momentary push buttons
for R6C5/R6C6. If the radio encoder turns out to have a push-to-click,
we can reshuffle later (push = pause/play, move Next Favourite here).

---

## 3. Function coverage check

All 29 keys.txt functions are covered:

- Start / Stop Engine .................... Ignition key ON
- Start / Stop Engine Electricity ........ Ignition key ACC
- Parking Brake .......................... Push/pull 1
- Trailer Brake .......................... Push/pull 2
- Lift / Drop Axle ....................... Toggle 5
- Differential Lock ...................... Toggle 4
- Hazard Warning ......................... Toggle 3
- Light Modes ............................ Light selector (3 pos)
- High Beam Headlights ................... Toggle 1
- Beacon ................................. Toggle 2
- Horn ................................... Momentary 1
- Air Horn ............................... Momentary 2
- Light Horn ............................. Momentary 3
- Wipers ................................. Wiper selector (3 pos)
- Wipers Back ............................ Momentary 5
- Cruise Control ......................... Toggle 6
- Cruise Control Speed Increase .......... Rocker 3 up
- Cruise Control Speed Decrease .......... Rocker 3 down
- Dashboard Display Mode ................. Latching 2
- Open Right Window ...................... Rocker 2 up
- Close Right Window ..................... Rocker 2 down
- Open Left Window ....................... Rocker 1 up
- Close Left Window ...................... Rocker 1 down
- Activate ............................... Momentary 4
- Trailer Attach / Detach ................ Latching 1
- Audio Player Volume Up ................. Knob CW
- Audio Player Volume Down ............... Knob CCW
- Audio Player Next Favourite ............ Knob push
- Audio Player Previous Favourite ........ Latching 3
- Audio Player pause/play ................ NEW button 1 (R6C5)
- Infomaint display mode ................. NEW button 2 (R6C6)

---

## 4. Suggested panel layout (rough)

```
+----------------------------------------------------------+
| [IGN KEY]   [WIPER 0-1-2]        [LIGHT 0-1-2]           |
|                                                          |
| [HB][BEA][HAZ]  [DIFF][AXLE][CRUISE]    <- 6 toggles    |
|                                                          |
| [PARK BRAKE PULL]  [TRAILER BRAKE PULL]  <- push/pull   |
|                                                          |
| [HORN][AIR][LIGHT HORN] [ACTIVATE][WIPER BACK]           |
|                                                          |
| [R1 WIN UP/DN]  [R2 WIN UP/DN]  [R3 CRUISE + / -]       |
|                                                          |
| [TRAILER ATT] [DASH MODE] [AUD PREV]  <- latching       |
|                                                          |
|          [RADIO KNOB]                                    |
+----------------------------------------------------------+
```

---

## 5. Open questions before build

1. Ignition key — does START spring back? How many contacts does it have?
2. Radio knob — does it have a push-to-click switch?
3. Are the wiper/light selectors OK with tap-counting, or should we try
   advanced controls.sii bindings instead?
4. Final keys must be verified against the actual ETS2 Controls menu.
5. Do you want AUX as an extra function on one of the spare positions?
6. NEW: The two new functions (Audio pause/play, Infomaint display mode)
   need 2 extra physical buttons. Do you have spares, or should the radio
   knob push take one of them?
