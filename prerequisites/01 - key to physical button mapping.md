# ETS2 Button Box — Key to Physical Button Mapping

Source file: `rough idea of keys.txt` (31 functions)
Control names verified against: `ATS and ETS2 Key Bindings.xlsx`

Status: PROPOSAL — nothing is soldered yet. Keys below are the ACTUAL
keys the user will use (from `02 - mapping table.csv`). Verify keys in
the ETS2 Controls menu before finalizing; any change must be made in
`02 - mapping table.csv` first, then this file.

---

## 1. Physical controls available

| # | Control | Qty | Notes |
|---|---|---|---|
| A | 3-position rotary selector | 2 | Wiper + Lights (3 states each) |
| B | Toggle switch (on/off) | 6 | stays in position |
| C | Push/pull switch | 2 | pull = on, push = off |
| D | Rocker switch | 3 | spring back to center, 2 directions (only 2 used) |
| E | Latching push button | 3 | stays in when pressed; used as momentary-style (one tap per press) |
| F | Momentary push button | 5 + 2 NEW | returns when released |
| G | Ignition key switch | 1 | OFF / ACC / ON (+START if present) |
| H | Radio knob / encoder (EC11 with push) | 1 | rotate CW/CCW + push |
| I | Cruise knob / encoder (EC11) | 1 | rotate CW/CCW for cruise speed (+push optional) |

Total matrix positions used: 36 of 36 (full matrix — no spares left).

---

## 2. The mapping

### Selectors (3-position, one tap per state change)

All three positions send the same cycle key; firmware counts taps to
reach the selected state (off->normal = 1 tap, off->fast = 2 taps,
fast->off = 3 taps, etc.).

| Matrix | Control | ETS2 function | Key | Behavior |
|---|---|---|---|---|
| R1C1 | Wiper selector pos 1 | Wipers OFF | P | tap key to reach OFF |
| R1C2 | Wiper selector pos 2 | Wipers NORMAL | P | tap key to reach NORMAL |
| R1C3 | Wiper selector pos 3 | Wipers FAST | P | tap key to reach FAST |
| R1C4 | Light selector pos 1 | Lights OFF | L | tap key to reach OFF |
| R1C5 | Light selector pos 2 | Lights PARKING | L | tap key to reach PARKING |
| R1C6 | Light selector pos 3 | Lights LOW BEAM | L | tap key to reach LOW BEAM |

NOTE: ETS2 "Wipers" and "Light Modes" are CYCLE keys (one press = next
state). Firmware must count taps: off->normal = 1 tap, off->fast = 2 taps,
fast->off = 3 taps, etc. The box tracks the last known state, so the
physical knob always lands on the same in-game state.

WIPERS BACK: assigned to the wiper selector's spring-back "negative"
position (if the selector has one). If it does not, Wipers Back needs its
own momentary button — see open question 5.

### Toggles (6) — one tap per state change

| Matrix | Control | ETS2 function | Key | Verify |
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

| Matrix | Control | ETS2 function | Key | Verify |
|---|---|---|---|---|
| R3C1 | Push/pull 1 | Parking Brake | SPACE | yes |
| R3C2 | Push/pull 2 | Trailer Brake | 7 | yes |

### Momentary (9 used of 10) — one tap per press

All momentary-style: press = one tap of the key. Functions that ETS2
treats as "press to toggle/cycle" fit this perfectly.

| Matrix | Control | ETS2 function | Key | Verify |
|---|---|---|---|---|
| R3C6 | Momentary 1 | Horn | H | yes |
| R4C1 | Momentary 2 | Air Horn | V | yes |
| R4C2 | Momentary 3 | Light Horn | B | yes |
| R4C3 | Momentary 4 | Dashboard Display Mode | I | yes |
| R4C4 | Momentary 5 | Activate | ENTER | yes |
| R3C3 | Momentary 6 | Trailer Attach / Detach | T | yes |
| R3C4 | Momentary 7 | Audio Player Next | N | yes |
| R3C5 | Momentary 8 | Audio Player Previous | P | yes |
| R6C5 | Momentary 9 | Infomaint display mode | I | yes |
| R6C6 | Momentary 10 | (spare) | - | - |

NOTE: Momentary 6/7/8 use the physical buttons originally bought as
"latching" — they are simply treated as momentary-style in firmware
(one tap per press, ignore the latching state). No extra purchases
beyond the 2 new buttons.

NOTE: Momentary 8 (P) and Momentary 4 (I) share keys with the Wipers (P)
and Infomaint (I) — see the 5 conflicts in `10 - keyboard key inventory.md`.
The firmware must be built with the RESOLVED key map, not this raw list.

### Rockers (2 of 3 used) — hold while pressed

| Matrix | Control | ETS2 function | Key | Verify |
|---|---|---|---|---|
| R4C5 | Rocker 1 up | Open Left Window | Q | yes |
| R4C6 | Rocker 1 down | Close Left Window | A | yes |
| R5C1 | Rocker 2 up | Open Right Window | W | yes |
| R5C2 | Rocker 2 down | Close Right Window | S | yes |

Rocker 3 (R5C3/R5C4) is no longer used for cruise speed — replaced by
the cruise knob. The rocker itself becomes a spare physical control.

### Cruise knob (encoder) — like a second volume knob

| Matrix | Control | ETS2 function | Key | Behavior |
|---|---|---|---|---|
| R5C3 | Cruise knob CW | Cruise Control Speed Increase | = | one tap per detent |
| R5C4 | Cruise knob CCW | Cruise Control Speed Decrease | - | one tap per detent |

Optional: if the cruise knob also has a push, use it for Cruise Control
Resume (position R5C3 or a spare if the matrix frees up).

### Ignition key (3 positions)

| Matrix | Control | ETS2 function | Key | Behavior |
|---|---|---|---|---|
| R5C5 | Key ACC | Start / Stop Engine Electricity | E | tap on entering ACC |
| R5C6 | Key ON | Start / Stop Engine | E | tap on entering ON |
| R6C1 | Key START | (engine crank, if present) | E | momentary |

NOTE: all three ignition positions send E (ETS2 ignition sequence:
press E = electricity, press again = engine). See the E/E conflict in
`10 - keyboard key inventory.md`.

### Radio knob (encoder with push)

| Matrix | Control | ETS2 function | Key | Behavior |
|---|---|---|---|---|
| R6C2 | Knob CW | Audio Player Volume Up | + | one tap per detent |
| R6C3 | Knob CCW | Audio Player Volume Down | - | one tap per detent |
| R6C4 | Knob push | Audio Player pause/play | SPACE | tap on press (if push exists) |

NOTE: SPACE (pause/play) conflicts with Parking Brake (SPACE) — see the
5 conflicts in `10 - keyboard key inventory.md`.

---

## 3. Function coverage check

All 31 keys.txt functions are covered:

- Start / Stop Engine .................... Ignition key ON (E)
- Start / Stop Engine Electricity ........ Ignition key ACC (E)
- Parking Brake .......................... Push/pull 1 (SPACE)
- Trailer Brake .......................... Push/pull 2 (7)
- Lift / Drop Axle ....................... Toggle 5 (J)
- Differential Lock ...................... Toggle 4 (D)
- Hazard Warning ......................... Toggle 3 (F)
- Light Modes ............................ Light selector (L, 3 pos)
- High Beam Headlights ................... Toggle 1 (K)
- Beacon ................................. Toggle 2 (U)
- Horn ................................... Momentary 1 (H)
- Air Horn ............................... Momentary 2 (V)
- Light Horn ............................. Momentary 3 (B)
- Wipers ................................. Wiper selector (P, 3 pos)
- Wipers Back ............................ Wiper selector negative (or own button)
- Cruise Control ......................... Toggle 6 (C)
- Cruise Control Speed Increase .......... Cruise knob CW (=)
- Cruise Control Speed Decrease .......... Cruise knob CCW (-)
- Dashboard Display Mode ................. Momentary 4 (I)
- Open Right Window ...................... Rocker 2 up (W)
- Close Right Window ..................... Rocker 2 down (S)
- Open Left Window ....................... Rocker 1 up (Q)
- Close Left Window ...................... Rocker 1 down (A)
- Activate ............................... Momentary 5 (ENTER)
- Trailer Attach / Detach ................ Momentary 6 (T)
- Audio Player Volume Up ................. Radio knob CW (+)
- Audio Player Volume Down ............... Radio knob CCW (-)
- Audio Player Next ...................... Momentary 7 (N)
- Audio Player Previous .................. Momentary 8 (P)
- Audio Player pause/play ................ Radio knob push (SPACE)
- Infomaint display mode ................. Momentary 9 (I)

CONFLICT NOTE: 5 keys are currently shared between two functions
(P = Wipers/Audio Previous, SPACE = Parking Brake/pause-play,
I = Dashboard/Infomaint, - = Cruise Decrease/Volume Down,
E = Electricity/Engine Start). See `10 - keyboard key inventory.md`
for the resolved final key map (0, 9, G, 8, Shift+E).

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
| [TRAILER ATT] [DASH MODE] [AUD PREV]  <- momentary       |
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
6. Cruise knob — if it has a push, there is no free matrix position for
   it (matrix is 36/36). Skip the push or swap something out.
7. Confirm: 2 extra momentary buttons to buy (R6C5 = Momentary 9
   Infomaint, R6C6 = Momentary 10 spare). If the radio knob has no push,
   one more button is needed for pause/play.
8. Resolve the 5 key conflicts (see `10 - keyboard key inventory.md`)
   before writing firmware: P, SPACE, I, -, E.
