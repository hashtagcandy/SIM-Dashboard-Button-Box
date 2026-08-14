# ETS2 Button Box — Key to Physical Button Mapping

Source file: `rough idea of keys.txt` (31 functions)
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
| D | Rocker switch | 3 | spring back to center, 2 directions (only 2 used) |
| E | Latching push button | 3 | stays in when pressed |
| F | Momentary push button | 5 + 2 NEW | returns when released |
| G | Ignition key switch | 1 | OFF / ACC / ON (+START if present) |
| H | Radio knob / encoder (EC11 with push) | 1 | rotate CW/CCW + push |
| I | Cruise knob / encoder (EC11) | 1 | rotate CW/CCW for cruise speed (+push optional) |

Total matrix positions used: 36 of 36 (full matrix — no spares left).

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

WIPERS BACK: assigned to the wiper selector's spring-back "negative"
position (if the selector has one). If it does not, Wipers Back needs its
own momentary button — see open question 5.

### Toggles (6) — one tap per state change

| Matrix | Control | ETS2 function | Suggested key | Verify |
|---|---|---|---|---|
| R2C1 | Toggle 1 | High Beam Headlights | K | yes |
| R2C2 | Toggle 2 | Beacon | U | yes |
| R2C3 | Toggle 3 | Hazard Warning | F | yes |
| R2C4 | Toggle 4 | Differential Lock | D | yes |
| R2C5 | Toggle 5 | Lift / Drop Axle | J | yes |
| R2C6 | Toggle 6 | Cruise Control | C | yes |

Cruise Control is ON/OFF via Toggle 6. Speed is adjusted with the
cruise knob below.

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

### Momentary (5 + 2 NEW) — hold while pressed

| Matrix | Control | ETS2 function | Suggested key | Verify |
|---|---|---|---|---|
| R3C6 | Momentary 1 | Horn | H | yes |
| R4C1 | Momentary 2 | Air Horn | V | yes |
| R4C2 | Momentary 3 | Light Horn | B | yes |
| R4C3 | Momentary 4 | Activate | ENTER | yes |
| R4C4 | Momentary 5 | Audio Player Next Favourite | N | yes |
| R6C5 | NEW button 1 | Audio Player Previous | P | yes |
| R6C6 | NEW button 2 | Infomaint display mode | I | yes |

### Rockers (2 of 3 used) — hold while pressed

| Matrix | Control | ETS2 function | Suggested key | Verify |
|---|---|---|---|---|
| R4C5 | Rocker 1 up | Open Left Window | Q | yes |
| R4C6 | Rocker 1 down | Close Left Window | A | yes |
| R5C1 | Rocker 2 up | Open Right Window | W | yes |
| R5C2 | Rocker 2 down | Close Right Window | S | yes |

Rocker 3 (R5C3/R5C4) is no longer used for cruise speed — replaced by
the cruise knob. The rocker itself becomes a spare physical control.

### Cruise knob (encoder) — like a second volume knob

| Matrix | Control | ETS2 function | Behavior |
|---|---|---|---|
| R5C3 | Cruise knob CW | Cruise Control Speed Increase | one tap per detent |
| R5C4 | Cruise knob CCW | Cruise Control Speed Decrease | one tap per detent |

Optional: if the cruise knob also has a push, use it for Cruise Control
Resume (position R5C3 or a spare if the matrix frees up).

### Ignition key (3 positions)

| Matrix | Control | ETS2 function | Behavior |
|---|---|---|---|
| R5C5 | Key ACC | Start / Stop Engine Electricity | tap on entering ACC |
| R5C6 | Key ON | Start / Stop Engine | tap on entering ON |
| R6C1 | Key START | (engine crank, if present) | momentary |

### Radio knob (encoder with push)

| Matrix | Control | ETS2 function | Behavior |
|---|---|---|---|
| R6C2 | Knob CW | Audio Player Volume Up | one tap per detent |
| R6C3 | Knob CCW | Audio Player Volume Down | one tap per detent |
| R6C4 | Knob push | Audio Player pause/play | tap on press (if push exists) |

---

## 3. Function coverage check

All 31 keys.txt functions are covered:

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
- Wipers Back ............................ Wiper selector negative (or own button)
- Cruise Control ......................... Toggle 6
- Cruise Control Speed Increase .......... Cruise knob CW
- Cruise Control Speed Decrease .......... Cruise knob CCW
- Dashboard Display Mode ................. Latching 2
- Open Right Window ...................... Rocker 2 up
- Close Right Window ..................... Rocker 2 down
- Open Left Window ....................... Rocker 1 up
- Close Left Window ...................... Rocker 1 down
- Activate ............................... Momentary 4
- Trailer Attach / Detach ................ Latching 1
- Audio Player Volume Up ................. Radio knob CW
- Audio Player Volume Down ............... Radio knob CCW
- Audio Player Next ...................... Momentary 5
- Audio Player Previous .................. NEW button 1
- Audio Player pause/play ................ Radio knob push
- Infomaint display mode ................. NEW button 2

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
| [HORN][AIR][LIGHT HORN] [ACTIVATE][AUD NEXT]             |
|                                                          |
| [WIN L UP/DN]  [WIN R UP/DN]   [CRUISE KNOB]            |
|                                                          |
| [TRAILER ATT] [DASH MODE] [AUD PREV]  <- latching       |
|                                                          |
| [RADIO KNOB]            [AUD PREV NEW][INFO NEW]         |
+----------------------------------------------------------+
```

Layouts A/B/C in 05-07 SVG files still show a CRUISE +/- rocker — they
need to be re-rendered with a cruise knob once the layout is chosen.

---

## 5. Open questions before build

1. Ignition key — does START spring back? How many contacts does it have?
2. Radio knob — does it have a push-to-click switch? (pause/play depends on it)
3. Are the wiper/light selectors OK with tap-counting, or should we try
   advanced controls.sii bindings instead?
4. Final keys must be verified against the actual ETS2 Controls menu.
5. Does the wiper selector have a spring-back "negative" position for
   Wipers Back? If not, Wipers Back needs its own momentary button
   (would push the matrix over 36 — need to free a position or add one).
6. Cruise knob — does it have a push? (optional Cruise Resume)
7. Confirm: 2 extra momentary buttons to buy (R6C5, R6C6). If the radio
   knob has no push, one more button is needed for pause/play.
