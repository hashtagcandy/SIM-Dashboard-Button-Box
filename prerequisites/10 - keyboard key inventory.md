# ETS2 Button Box — Keyboard Key Inventory

What keys the firmware must send, how many, and where each is used.

Source: `prerequisites/02 - mapping table.csv`
Status: PROPOSAL — all keys must be verified in the ETS2 Controls menu.

---

## 1. Summary

| Item | Count |
|---|---|
| ETS2 functions to bind | 31 |
| Distinct keys needed (after fixing conflicts below) | 30 |
| Keys currently suggested | 25 |
| Conflicts to fix (same key, different functions) | 5 |

Rule of thumb: in ETS2, ONE function = ONE key. The same key may be
reused only when it is literally the same function (e.g. the 3 wiper
selector positions all send the "Wipers" cycle key).

---

## 2. Full key list (what keys, used where)

| Key | ETS2 function | Matrix positions | OK? |
|---|---|---|---|
| P | Wipers (cycle) | R1C1, R1C2, R1C3 | CONFLICT with Audio Previous |
| L | Light modes (cycle) | R1C4, R1C5, R1C6 | OK |
| K | High Beam Headlights | R2C1 | OK |
| U | Beacon | R2C2 | OK |
| F | Hazard Warning | R2C3 | OK |
| D | Differential Lock | R2C4 | OK |
| J | Lift / Drop Axle | R2C5 | OK |
| C | Cruise Control | R2C6 | OK |
| SPACE | Parking Brake | R3C1 | CONFLICT with pause/play |
| 7 | Trailer Brake | R3C2 | OK |
| T | Trailer Attach / Detach | R3C3 | OK |
| N | Audio Player Next | R3C4 | OK |
| P | Audio Player Previous | R3C5 | CONFLICT with Wipers |
| H | Horn | R3C6 | OK |
| V | Air Horn | R4C1 | OK |
| B | Light Horn | R4C2 | OK |
| I | Dashboard Display Mode | R4C3 | CONFLICT with Infomaint |
| ENTER | Activate | R4C4 | OK |
| Q | Open Left Window | R4C5 | OK |
| A | Close Left Window | R4C6 | OK |
| W | Open Right Window | R5C1 | OK |
| S | Close Right Window | R5C2 | OK |
| = | Cruise Speed Increase | R5C3 | OK |
| - | Cruise Speed Decrease | R5C4 | CONFLICT with Volume Down |
| E | Engine Electricity | R6C2 | CONFLICT with Engine Start |
| E | Engine Start | R6C3 | CONFLICT with Engine Electricity |
| E | Engine crank (START) | R6C1 | shares key with Engine Start — OK |
| + | Audio Volume Up | R6C4 | OK |
| - | Audio Volume Down | R6C5 | CONFLICT with Cruise Decrease |
| SPACE | Audio pause/play | R6C6 | CONFLICT with Parking Brake |
| I | Infomaint display mode | R5C5 | CONFLICT with Dashboard |
| (spare) | - | R5C6 | - |

---

## 3. The 5 conflicts to fix

| # | Key | Function A | Function B | Suggested fix |
|---|---|---|---|---|
| 1 | P | Wipers | Audio Previous | Audio Previous -> 0 |
| 2 | SPACE | Parking Brake | Audio pause/play | pause/play -> 9 |
| 3 | I | Dashboard Display | Infomaint | Infomaint -> G |
| 4 | - | Cruise Decrease | Volume Down | Volume Down -> 8 |
| 5 | E | Engine Electricity | Engine Start | Electricity -> Shift+E (ETS2 default style) |

With these fixes the key map becomes:

| Key | Function |
|---|---|
| P | Wipers |
| L | Light modes |
| K | High Beam |
| U | Beacon |
| F | Hazard |
| D | Diff Lock |
| J | Axle |
| C | Cruise Control |
| SPACE | Parking Brake |
| 7 | Trailer Brake |
| T | Trailer Attach |
| N | Audio Next |
| 0 | Audio Previous |
| H | Horn |
| V | Air Horn |
| B | Light Horn |
| I | Dashboard Display |
| ENTER | Activate |
| Q / A | Left Window open / close |
| W / S | Right Window open / close |
| = / - | Cruise speed + / - |
| E | Engine Start (+ crank) |
| Shift+E | Engine Electricity |
| + / 8 | Volume up / down |
| 9 | pause/play |
| G | Infomaint |

Count: 30 distinct keys for 31 functions (crank shares Engine Start's key).

---

## 4. Verify checklist (in ETS2 Controls menu)

- [ ] Engine start/stop = E, engine electricity = Shift+E?
- [ ] Wipers = P, lights = L?
- [ ] High beam = K, beacon = U, hazard = F?
- [ ] Cruise = C, speed +/-, resume?
- [ ] Windows open/close = Q/A/W/S?
- [ ] Horn = H, air horn = V, light horn = B?
- [ ] Audio next/prev/vol/pause keys?
- [ ] Dashboard / Infomaint display keys?
- [ ] Trailer attach = T, park brake = SPACE, trailer brake = 7?

If any default differs, update `02 - mapping table.csv` and this file
together — the firmware key map is generated from that table.
