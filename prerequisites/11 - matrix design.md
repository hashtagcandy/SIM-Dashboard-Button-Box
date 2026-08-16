# ETS2 Button Box — Switch Matrix Design

How the 6x6 matrix is wired and scanned. This is the wiring-level design
that turns the logical map (file 01/02) into physical wires on the Pico.

Status: PROPOSAL — no soldering yet. Contains one re-map decision that
needs user approval (section 4).

---

## 1. Scan scheme (how firmware reads switches)

- 6 row GPIO (GP2-GP7) configured as OUTPUT.
- 6 column GPIO (GP8-GP13) configured as INPUT_PULLUP.
- Firmware drives ONE row LOW at a time, reads all 6 columns.
- A closed switch connects row (LOW) to column -> column reads LOW = pressed.
- An open switch leaves the column HIGH (internal pull-up) = not pressed.
- Full matrix scan = 6 row activations x 6 column reads = 36 samples.
- No external pull-up resistors needed: Pico internal pull-ups (~50k ohm)
  are sufficient for a switch matrix.
- Debounce in firmware: re-read after 15-20 ms before accepting a change.

```
          COLUMNS (GP8..GP13, INPUT_PULLUP, LOW = pressed)
          C1    C2    C3    C4    C5    C6
         GP8   GP9   GP10  GP11  GP12  GP13
        +-----+-----+-----+-----+-----+-----+
R1 GP2  | W1  | W2  | W3  | L1  | L2  | L3  |  selectors (3-pos)
R2 GP3  | HB  | BEA | HAZ | DIF | AXL | CRU |  6 toggles
R3 GP4  | PK  | TRL | ATT | NXT | PRV | HRN |  push/pull + latching
R4 GP5  | AH  | LH  | DSH | ACT | WLU | WLD |  momentaries + rocker 1
R5 GP6  | WRU | WRD | CR+ | CR- | INF | SP  |  rocker 2 + cruise + infomaint
R6 GP7  | ST  | ACC | ON  | RAD+| RAD-| RDP |  ignition + radio
        +-----+-----+-----+-----+-----+-----+
        ROWS = OUTPUT, driven LOW one at a time
```

Legend:

| Label | Function | Key | Label | Function | Key |
|---|---|---|---|---|---|
| W1-3 | Wipers pos 1/2/3 | P | WRU | Open Right Window | W |
| L1-3 | Light pos 1/2/3 | L | WRD | Close Right Window | S |
| HB | High Beam | K | CR+ | Cruise speed + | = |
| BEA | Beacon | U | CR- | Cruise speed - | - |
| HAZ | Hazard | F | INF | Infomaint display | G |
| DIF | Diff Lock | D | ST | Ignition START | E |
| AXL | Lift/Drop Axle | J | ACC | Ignition ACC | Shift+E |
| CRU | Cruise Control | C | ON | Ignition ON | E |
| PK | Parking Brake | SPACE | RAD+ | Volume Up | + |
| TRL | Trailer Brake | 7 | RAD- | Volume Down | 8 |
| ATT | Trailer Attach | T | RDP | pause/play | 9 |
| NXT | Audio Next | N | SP | spare | - |
| PRV | Audio Previous | 0 | | | |
| HRN | Horn | H | | | |
| AH | Air Horn | V | | | |
| LH | Light Horn | B | | | |
| DSH | Dashboard Mode | I | | | |
| ACT | Activate | ENTER | | | |
| WLU | Open Left Window | Q | | | |
| WLD | Close Left Window | A | | | |

Keys shown are the RESOLVED map from file 10 (conflicts already fixed).

---

## 2. Wiring per control type

Each switch is wired as: ROW wire -> one lug, COLUMN wire -> other lug.

| Control | Lugs to use | Wiring |
|---|---|---|
| Toggle switch | 2 (common + ON lug) | common -> row, ON lug -> column |
| Push/pull switch | 2 | one side -> row, other side -> column |
| Momentary button | 2 | one terminal -> row, other -> column |
| Latching button | 2 | one terminal -> row, other -> column |
| Rocker (center-off) | 3 | common -> row, UP lug -> column A, DOWN lug -> column B |
| 3-position selector | 4 | common -> row, 3 state lugs -> 3 columns (MUST measure first) |
| EC11 encoder | 5 | common -> row, A -> column CW, B -> column CCW, push -> column |
| Ignition key | 4+ | common (B) -> row, ACC -> column, ON/IGN -> column, ST -> column (MUST measure first) |

### Encoder detail (EC11)

Typical EC11 has 5 pins: A, B, C (common), plus 2 for the push switch.

```
EC11            Matrix
A    ---------> column (CW detent)
B    ---------> column (CCW detent)
C    ---------> row (shared)
push ---------> column (tap on press)
```

Firmware decodes quadrature (A/B order) to decide CW vs CCW, one key tap
per detent. The push is just another switch on the same row.

### Selector detail

Both 3-position selectors occupy one full row each:

```
Selector (4 terminals)          Matrix
common lug  ----------------> R1
state lug 1 (position 1) ----> C1 / C4
state lug 2 (position 2) ----> C2 / C5
state lug 3 (position 3) ----> C3 / C6
```

Wiper selector: R1C1/R1C2/R1C3. Light selector: R1C4/R1C5/R1C6.

CONTACTS MUST BE MEASURED WITH A MULTIMETER — terminal numbers alone do
not tell you which lug is common or which is position 1/2/3.

---

## 3. Wire plan / color code

Keep 12 matrix wires traceable:

| Wire | Color | Notes |
|---|---|---|
| Rows R1-R6 | RED | tag each wire R1..R6 (heat-shrink label or tape) |
| Columns C1-C6 | BLUE | tag each wire C1..C6 |
| Common GND | BLACK | not used by matrix itself |

Each switch gets exactly two wires: one red (to its row), one blue (to
its column). Multi-position controls (rockers, selectors, encoders,
ignition) share one red wire on the common lug and one blue per column.

Wiring groups (nice for routing):

| Row | Controls sharing the row wire |
|---|---|
| R1 | Wiper selector common + Light selector common |
| R2 | 6 toggle commons |
| R3 | 2 push/pull + 3 latching + horn |
| R4 | 3 momentaries + activate + rocker 1 common |
| R5 | rocker 2 common + cruise encoder common + infomaint |
| R6 | ignition common + radio encoder common |

---

## 4. RE-MAP DECISION — ignition key needs one row (PENDING APPROVAL)

PROBLEM: the current mapping (files 01/02) puts Ignition ACC and ON on
row 5 but START on row 6. A typical ignition key switch has ONE common
lug (Battery). ACC, ON and START all share it, so they must all sit on
the SAME row. Splitting them across R5 and R6 is not physically wireable
with a single-common key.

FIX: move the whole ignition key to row 6, and slide the radio encoder
over to make room. Infomaint moves up to row 5's freed space.

| Position | OLD mapping | NEW mapping |
|---|---|---|
| R5C1 | Rocker 2 up | Rocker 2 up (unchanged) |
| R5C2 | Rocker 2 down | Rocker 2 down (unchanged) |
| R5C3 | Cruise CW | Cruise CW (unchanged) |
| R5C4 | Cruise CCW | Cruise CCW (unchanged) |
| R5C5 | Ignition ACC | **Infomaint** (moved from R6C5) |
| R5C6 | Ignition ON | **spare** (moved from R6C6) |
| R6C1 | Ignition START | Ignition START (unchanged) |
| R6C2 | Radio CW | **Ignition ACC** (moved from R5C5) |
| R6C3 | Radio CCW | **Ignition ON** (moved from R5C6) |
| R6C4 | Radio push | **Radio CW** (moved from R6C2) |
| R6C5 | Infomaint | **Radio CCW** (moved from R6C3) |
| R6C6 | spare | **Radio push** (moved from R6C4) |

Result: ignition = R6C1 (ST) / R6C2 (ACC) / R6C3 (ON), all sharing row 6.
Radio = R6C4/C5/C6, also sharing row 6. Rocker 2 + cruise + infomaint
keep row 5 to themselves. All 36 positions still used, zero new hardware.

If the multimeter later shows the ignition key has SEPARATE commons per
position (rare), the old split could work — but plan for single-common.

IF APPROVED: update `02 - mapping table.csv`, the tables in
`01 - key to physical button mapping.md`, and SOUL.md matrix map together.

---

## 5. Test plan (matrix bring-up)

1. Breadboard a 2x2 mini matrix (2 rows, 2 cols, 4 momentary buttons).
   Verify scan: press each button, confirm correct R/C detected.
2. Test simultaneous presses on the mini matrix (ghosting check).
   Matrix diodes were skipped by decision — confirm no ghosting at 2x2.
3. Build the full 6x6 on breadboard with all switch types (toggle,
   push/pull, rocker, latching, momentary, selectors, encoders, ignition).
4. Test every one of the 36 positions individually (LED or serial print).
5. Test multi-press patterns that would have ghosted before wiring permanent.
6. If ghosting appears: add one 1N4148 diode per switch (36 max), cathode
   toward the column.

---

## 6. Open questions before wiring

1. Ignition key contact layout — single common or separate per position?
   Multimeter: find common, then map ACC/ON/START lugs.
2. Selector contact layout — which lug is common, which is pos 1/2/3?
   Multimeter on all 4 lugs in all 3 positions.
3. Rocker switch type — confirm 3-terminal center-off (common + up + down).
4. EC11 encoder pinout — confirm A/B/common/push pins on the actual part.
