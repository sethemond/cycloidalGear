# Cycloidal Gearbox for Robotic Arm

> **Work in Progress** — Phase 1 (Proof of Concept) underway

Design, model, print, and test a cycloidal gearbox using stepper motors I already have on hand. The end goal is a high gear ratio output suitable for a robotic arm joint. This is both a portfolio piece and a real functional component for a larger DIY robotic arm project (TBD).

---

## Objectives

**A. Skills Showcase**

Demonstrate mechanical engineering design workflow end-to-end: parametric CAD (SolidWorks), FEA (ANSYS), DFM/DFA, tolerance analysis & stackup, GD&T (ASME Y14.5), and engineering documentation.

**B. Actual Build**

This parametric gearbox is intended to be the primary gearbox platform for a robotic arm intended for large-format 3D printing, CNC machining, and general home manufacturing use. Needs to be practical, parametric, semi-modular, printable, and very low cost. Ideally, zero to few parts need to be purchased (effective budget: $0).

---

## Constraints

- **Motor**: Fixed — using existing stepper motors from a previous project ([Specs](https://media.digikey.com/pdf/Data%20Sheets/Seeed%20Technology/316020003_Web.pdf)). Input torque is set by this selection.
- **Manufacturing**: Primarily FDM 3D printing. Sheet metal, hardware, and epoxy reinforcement configs may be explored later.
- **Hardware**: On-shelf only (high preference to parts that I already have) — 608 ball bearings, threaded inserts, standard screws, and 5mm binding posts/pins.
- **Modularity**: The housing and output interface must be semi-modular — the arm geometry and connector style are unknown.
- **Design intent**: Parametrically driven. Output torque, gear ratio, and geometry should update from a top-level parameter list.

---

## Parametric Design Drivers

| Parameter | Status | Notes |
|---|---|---|
| Input torque | Fixed | Set by motor selection |
| Input speed | Derived | From motor spec |
| Number of lobes (N) | Variable | Drives gear ratio |
| Gear ratio / output torque | Target | Based on arm mass × length estimate |
| External lobe diameter | Calculated | From bearing + eccentricity geometry |
| Output diameter | Variable | Geometry-dependent |

**Gear ratio**: `I = n / (N - n)` where n = number of output pins


---

## Phase 1 Scope — Proof of Concept

Keeping it simple for the first pass. Single disk, single shaft, single output. Focus on geometry, tolerances, and DFM/DFA.

**Deliberately out of scope for Phase 1:**
- Face-to-face sliding friction analysis
- Vibration balancing (normally solved with 2 disks at 180° or 3 at 120°)
- Complex bearing arrangements — basic 608 ball bearings only, or no bearings

**Phase 1 Key Parameters** *(all dims in mm)*

```
608_OD        = 22 mm
608_ID        = 8 mm
608_h         = 7 mm
Shaft_core_OD = 6 mm
Eccentricity  = 2 mm       (= 608_ID - Shaft_core_OD)
d_i (pin OD)  = 5 mm       (4.9mm measured on binding posts)
N (lobes)     = 15
n (pins)      = 14
Gear ratio    = 14:1
```

Cycloidal disk profile is driven by the parametric equations implemented as a sketch in SolidWorks.


**Current CAD Parameters list:**
*SolidWorks parameters are all linked to an external text file to allow all sub-components/parts to reference the same global parameters list*

```
"608_OD"= 22
"608_ID"= 8
"608_h"= 7
"Shaft_core_OD"= 6
"Eccent"= "608_ID" - "Shaft_core_OD"
"d_i"= 5
"d_h"= "d_i" + 2 * "Eccent"
"d_p"= "608_OD" + "d_h" * 2 + "d_h"
"N_pin"= 15
"n_disk"= 14
"d_r"= 5
"i"= "n_disk" /  ( "N_pin" - "n_disk" ) 
"d_disk"= "d_p" + 2 * "d_h"
"D_pin"= "d_disk" * "N_pin" / "i"
"eccent_sanity"= IIF ( ( "Eccent" < ( ( "D_pin" / "N_pin" ) ) / 2 ) , 1 , 0 )
```

**Solidworks Screenshot**
![Parameters List Screenshot](Images/Parameters%20List%20Screenshot%20-%20May18th.png)




---

## Current Status

**Completed**
- [x] Project parameters defined
- [x] Cycloidal gear equations derived and verified
- [x] Parametric sketch created in SolidWorks
- [x] Housing base part design
- [x] Cycloidal disk part design

**Remaining**
- [ ] Input shaft part design
- [ ] 608 bearing model imported
- [ ] Mechanical mating + basic motion analysis
- [ ] Tolerance / fit review *(tight tolerances to limit backlash are flagged as a concern for 3D printing)*
- [ ] FEA validation (strength + fatigue)
- [ ] GD&T drawings
- [ ] Print, assemble, and test

---

## CAD Preview

![Housing](Images/Housing%20-%20May18th.png)

![Cycloidal Gear Disk](Images/Gear-%20May18th.png)



---

