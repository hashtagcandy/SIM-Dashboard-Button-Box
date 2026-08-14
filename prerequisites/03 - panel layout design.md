# ETS2 Button Box — Panel Layout Design (v1 PROPOSAL)

Reference: prerequisites/01 - key to physical button mapping.md

## 1. Design principles

1. Group by truck function: lights together, brakes together, audio together.
2. Frequent / emergency controls closest to the user (front edge of panel):
   horns + activate.
3. Parking brake gets a prominent position — it is the most-grabbed control
   in a truck.
4. Every stateful control gets an LED right next to it (state at a glance).
5. 3-position selectors get a 3-LED position strip, like a real dash indicator.
6. Ignition key sits in a corner — the "start of the day" control, away from
   controls you touch while driving.

## 2. Default panel size assumption

Landscape panel, approx 300 x 170 mm (can be resized — nothing is cut yet).

Controls are laid out in 6 horizontal rows. Row 5 (momentary buttons) is the
front edge (closest to the user).

## 3. The layout (driver's view, top = far edge)

```
+------------------------------------------------------------------+
|  [IGN]    [WIPER  o o o]   [LIGHT  o o o]             [RADIO]   |
|  KEY      0  1  2           0  1  2                              |
|                                                                  |
|   o   o   o       o   o   o                                      |
|  [HB] [BEA] [HAZ] | [DIFF] [AXLE] [CRUISE]   <- 6 toggles       |
|                                                                  |
|   o                    o                                         |
|  [PARK BRAKE]       [TRAILER BRAKE]          <- 2 push/pull      |
|    (pull handle)      (pull handle)                              |
|                                                                  |
|  [WIN L]   [WIN R]   [CRUISE +/-]            <- 3 rockers       |
|   up/down   up/down   inc/dec                                    |
|                                                                  |
|   o   o   o       o   o                                          |
|  [HORN] [AIR] [LT HORN]  [ACT] [WIPER BACK]  <- 5 momentary     |
|                                                                  |
|   o   o   o                                                      |
|  [TRAILER ATT] [DASH MODE] [AUD PREV]        <- 3 latching      |
|                                                                  |
|                 ^                                                |
|                 front edge (closest to user)                     |
+------------------------------------------------------------------+
```

o = indicator LED next to the control

## 4. LED placement

| LED | Location |
|---|---|
| Wiper pos 1/2/3 | 3-LED strip directly above wiper selector |
| Light pos 1/2/3 | 3-LED strip directly above light selector |
| High beam | above toggle 1 |
| Beacon | above toggle 2 |
| Hazard | above toggle 3 |
| Diff lock | above toggle 4 |
| Lift/Drop axle | above toggle 5 |
| Cruise control | above toggle 6 |
| Park brake | beside park brake handle |
| Trailer brake | beside trailer brake handle |
| Trailer attach | beside latching 1 |
| Dash mode | beside latching 2 |
| Audio prev | beside latching 3 |
| Horn | beside momentary 1 |
| Air horn | beside momentary 2 |
| Light horn | beside momentary 3 |
| Activate | beside momentary 4 |
| Wipers back | beside momentary 5 |
| Ignition | beside ignition key (lights with ACC/ON state) |

Total: 23 LEDs = exactly the 3x 74HC595 capacity (24 outputs, 1 spare).

## 5. Mounting hole sizes (typical)

| Control | Hole / cutout |
|---|---|
| 3-position rotary selector | 20 mm round |
| Toggle switch | 12 mm round |
| Push/pull switch | 12 mm round |
| Rocker switch | 19 x 13 mm rectangle |
| Latching push button | 12 mm round (16 mm if LED-ring type) |
| Momentary push button | 12 mm round (16 mm if LED-ring type) |
| Ignition key switch | 19-22 mm round |
| Radio encoder | 10 mm round (6 mm shaft + nut) |
| Indicator LED (5 mm) | 5 mm round |

Spacing rule: minimum 25 mm center-to-center between adjacent controls,
so fingers/knobs never collide. If the panel is larger, add 30 mm.

## 6. Wiring implications

- All controls stay in the 6x6 matrix — layout does not change the matrix map.
- LED order in the 74HC595 chain follows the LED table above (LED #1 =
  wiper pos 1 ... LED #23 = ignition). See SOUL.md LED numbering.
- The push/pull brake handles and rockers are mounted so "pull" and "up"
  are clearly labelled — add laser-printed or engraved labels on the panel.

## 7. Open questions

1. Actual enclosure size / panel dimensions? (adjust row spacing)
2. Do the momentary buttons have LED rings, or plain? (affects LED count)
3. Material: 3D printed, acrylic, aluminium, or wood?
4. Labels: engraved, printed sticker, or painted?
5. Do you want the ignition key on the LEFT (current) or RIGHT corner?
