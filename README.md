# Signal Integrity - 50 Ohm PCB Coupons (2L and 4L)

[🇺🇸 English](./README.md) | [🇧🇷 Português](./README-pt.md)

## Index
- [Overview](#overview)
- [Introduction](#introduction)
- [Technical scope](#technical-scope)
- [Stackup impact in this context](#stackup-impact)
- [Why microstrip is harder on 2-layer boards](#microstrip-2l-challenges)
- [Board design details](#board-design-details)
- [2-layer board](#board-2l)
- [4-layer board](#board-4l)
- [NanoVNA-F V2 in this project](#nanovna-f-v2)
- [NanoVNA Saver](#nanovna-saver)
- [Repository structure](#repository-structure)
- [Project links](#project-links)
- [Tests (Script and Report)](#tests-script-report)

<a id="overview"></a>
## Overview
This repository contains the design and validation material for 50 Ohm PCB test coupons focused on RF and high-speed signal integrity analysis.

The project compares line behavior across 2-layer and 4-layer boards, with emphasis on:
- impedance control;
- transmission quality;
- sensitivity to layout discontinuities.

<a id="introduction"></a>
## Introduction
Designing RF and high-speed PCBs with predictable electrical behavior is challenging because real trace impedance does not depend only on trace width. Final behavior is strongly influenced by stackup, distance to the reference plane, effective dielectric constant, copper thickness, solder mask, connector transitions, vias, and return path continuity.

In practice, two visually similar layouts can show very different `S11` and `S21` behavior after fabrication. Small discontinuities, such as a slot in the GND plane or a return-current path change around a via transition, can generate extra reflection, ripple, and transmission notch, especially as frequency increases.

Because of that, impedance matching should be validated by measurement, not only by simulation. In RF chains and reflection-sensitive high-speed links, recurring mismatch degrades power transfer efficiency, reduces signal margin, increases process sensitivity, and may compromise electrical compliance.

This project uses a controlled coupon set and repeatable measurements to correlate layout decisions with measured performance and derive practical design guidelines.

<a id="technical-scope"></a>
## Technical scope
- Boards:
  - 2L (standard FR-4)
  - 4L (JLC04161H-3313 stackup)
- Structures under test:
  - Microstrip 50 Ohm:
    - Basic concept: signal trace on an outer layer referenced to a ground plane below.
    - Advantages: simple routing, lower copper usage, easy implementation, common in mixed-signal and RF boards.
    - Disadvantages: stronger field exposure to air/environment, higher sensitivity to nearby discontinuities, and typically weaker confinement than CPWG.
    - Typical applications: general RF interconnects, moderate high-speed lines, and cost-sensitive designs.
  - CPWG (Coplanar Waveguide with Ground) 50 Ohm:
    - Basic concept: signal trace on an outer layer with coplanar ground on both sides plus a reference plane below.
    - In practice (especially on 2-layer boards), this structure often achieves 50 Ohm with narrower traces than plain microstrip, depending on stackup and manufacturing limits.
    - Advantages: better electromagnetic confinement, improved control of return current near the trace, and often better behavior at transitions/connectors.
    - Disadvantages: requires tighter layout control (trace-gap geometry), can be more sensitive to fabrication tolerance in narrow gaps, and may increase routing area.
    - Typical applications: RF paths requiring stronger field control, launches/connectors, and high-speed channels where discontinuity control is critical.
- Scenarios compared:
  - Baseline
  - Vias
  - Vias + return path
  - GND discontinuity (slot)
  - Matching variant (`matching` = impedance adjustment network/geometry used to reduce reflections and improve power transfer; this is especially important in RF chains and reflection-sensitive high-speed links to preserve signal integrity, minimize return loss, and stabilize frequency response)
- Instrumentation:
  - NanoVNA-F V2
  - NanoVNA Saver

<a id="stackup-impact"></a>
### Stackup impact in this context
- Characteristic impedance is a stackup-dependent parameter, not only a trace-width parameter.
- For the same target (50 Ohm), required geometry changes with:
  - dielectric thickness between signal and reference plane;
  - effective dielectric constant (`Er`);
  - copper thickness and manufacturing tolerances;
  - solder mask and nearby ground geometry.
- Better stackup control generally improves repeatability of `S11/S21` across fabrication lots.

<a id="microstrip-2l-challenges"></a>
### Why microstrip is harder on 2-layer boards
- In 2-layer boards, achieving 50 Ohm microstrip often requires relatively wide traces because the distance to the reference plane is larger.
- Wider traces create practical limitations:
  - larger routing area consumption;
  - limited compatibility with modern package pin pitches, where very wide traces are often difficult to connect directly at pads/escapes without neck-down transitions and associated discontinuities;
  - stronger coupling to nearby discontinuities and connectors;
  - more sensitivity to local stackup and assembly variation.
- In many low-cost 2L processes, dielectric and copper tolerances are less tightly controlled than in controlled 4L stackups, increasing impedance spread.
- CPWG is frequently used in 2L as a mitigation strategy because coplanar grounds improve field confinement and return-current control compared to plain microstrip.

<a id="board-design-details"></a>
## Board design details

<a id="board-2l"></a>
### 2-layer board (`Signal_Integrity_2L_Simplified`)
Main characteristics:
- Cost-oriented 2-layer implementation on standard FR-4, used to quantify how reduced stackup control impacts 50 Ohm routing.
- Mirrors the same measurement philosophy as the 4-layer board for direct comparability:
  - baseline structures;
  - vias and vias + return-path scenarios;
  - GND-discontinuity scenario;
  - matching variant.
- Useful to evaluate tradeoffs between manufacturability/cost and RF/high-speed performance stability.

Coupon variations and objectives:
- `2L_01 - Microstrip 50 Ohm (W2700)`: baseline reference for 2-layer microstrip behavior.
- `2L_02 - CPWG 50 Ohm baseline (W800/G200)`: baseline CPWG reference in 2L.
- `2L_03 - CPWG 50 Ohm (W380/G120)`: CPWG geometry variant to compare confinement and process sensitivity versus G200.
- `2L_04 - CPWG 50 Ohm + matching (W800/G200)`: evaluates matching strategy impact relative to CPWG baseline.
- `2L_05 - CPWG 50 Ohm + vias`: quantifies transition-induced discontinuity and added reflection/ripple.
- `2L_06 - CPWG 50 Ohm + vias + return path`: checks improvement from reinforcing return-current continuity around transitions.
- `2L_07 - CPWG 50 Ohm + GND discontinuity (slot)`: measures degradation caused by intentional return-path interruption.

<a id="board-2l-schematic"></a>
#### Schematic
<img src="./images/sch_2l.png" alt="2-layer schematic" width="700">

<a id="board-2l-stackup"></a>
#### Stackup
<img src="./images/stackup_2l.png" alt="2-layer stackup" width="700">

<a id="board-2l-layout"></a>
#### PCB layout views
<table>
  <tr>
    <td><img src="./images/pcb_2d_top_2l.png" alt="2-layer top" width="340"></td>
    <td><img src="./images/pcb_2d_bot_2l.png" alt="2-layer bottom" width="340"></td>
  </tr>
</table>

<a id="board-2l-3d"></a>
#### 3D view
<img src="./images/pcb_3d_top_2l.png" alt="2-layer 3D top" width="700">

<a id="board-4l"></a>
### 4-layer board (`Signal_Integrity_4L_Simplified`)
Main characteristics:
- Controlled-impedance environment using a 4-layer stackup (`JLC04161H-3313`), improving return-path continuity and field confinement.
- Includes 50 Ohm coupon variants to isolate layout effects:
  - baseline microstrip and CPWG references;
  - via-transition case and via + return-path improvement case;
  - intentional GND-plane discontinuity (slot) to measure degradation;
  - matching variant for comparison against baseline behavior.
- Intended as the higher-control reference platform for RF/high-speed SI measurements.

Coupon variations and objectives:
- `4L_01 - Microstrip 50 Ohm baseline (W350)`: baseline reference for controlled-impedance microstrip on 4L.
- `4L_02 - CPWG 50 Ohm baseline (W285/G200)`: baseline CPWG reference in 4L.
- `4L_03 - CPWG 50 Ohm (W210/G120)`: CPWG geometry variant to compare field confinement and tolerance sensitivity versus G200.
- `4L_04 - CPWG 50 Ohm + matching (W285/G200)`: evaluates matching strategy impact relative to CPWG baseline.
- `4L_05 - Microstrip 50 Ohm + vias`: quantifies via-transition discontinuity impact.
- `4L_06 - Microstrip 50 Ohm + vias + return path`: verifies reflection/ripple reduction with improved return-current path near transitions.
- `4L_07 - Microstrip 50 Ohm + GND discontinuity (slot on L2)`: measures sensitivity to intentional plane interruption in an inner reference layer.

<a id="board-4l-schematic"></a>
#### Schematic
<img src="./images/sch_4l.png" alt="4-layer schematic" width="700">

<a id="board-4l-stackup"></a>
#### Stackup
<img src="./images/stackup_4l.png" alt="4-layer stackup" width="700">

Layer 2 and the bottom layer include copper polygons connected to GND.

<a id="board-4l-layout"></a>
#### PCB layout views
<table>
  <tr>
    <td><img src="./images/pcb_2d_top_4l.png" alt="4-layer top" width="340"></td>
    <td><img src="./images/pcb_2d_ly2_4l.png" alt="4-layer inner layer 2" width="340"></td>
  </tr>
  <tr>
    <td><img src="./images/pcb_2d_ly3_4l.png" alt="4-layer inner layer 3" width="340"></td>
    <td><img src="./images/pcb_2d_bot_4l.png" alt="4-layer bottom" width="340"></td>
  </tr>
</table>

<a id="board-4l-3d"></a>
#### 3D view
<img src="./images/pcb_3d_top_4l.png" alt="4-layer 3D top" width="700">

<a id="nanovna-f-v2"></a>
## NanoVNA-F V2 in this project
The NanoVNA-F V2 is used as a compact vector network analyzer to characterize reflections and transmission through PCB coupons.

Official product page:
- https://www.sysjoint.com/index.php?tpl=product_detail&pid=11&uid=17&id=65&sno=1&list=2&lang=en

Practical NanoVNA V2 guides:
- [NanoVNA V2 Practical Guide (English)](./Nanovna_v2_pcb_en.md)
- [NanoVNA V2 Practical Guide (Portuguese)](./Nanovna_v2_pcb.md)

Device image:
<br>
<img src="./images/vna2.jpg" alt="NanoVNA-F V2 kit view" width="700">

Manufacturer highlights (NanoVNA-F V2):
- Frequency range: 50 kHz to 3 GHz
- Dynamic range:
  - `S11`: 50 dB (<1.5 GHz), 40 dB (<3 GHz)
  - `S21`: 70 dB (<1.5 GHz), 60 dB (<3 GHz)
- RF output power: about -9 dBm
- Display: 4.3-inch IPS (800x480)
- Battery: 5000 mAh (up to around 7 hours)
- Interface: USB Type-C
- Port SWR: <1.1

Key S-parameter concepts used in this project:
- `S11` (input reflection coefficient): indicates how much of the incident signal is reflected back at the input due to impedance mismatch. More negative values in dB usually mean better matching.
- `S21` (forward transmission coefficient): indicates how much signal goes from port 1 to port 2 through the structure under test. Lower loss means `S21` closer to 0 dB.

- Principle of operation:
  - Performs a swept `CW` measurement (one sine tone per frequency point).
  - Uses directional sampling and coherent `I/Q` detection to separate incident, reflected, and transmitted waves.
  - Computes scattering parameters from ratio measurements (`S11` and `S21`).
- Functions used in this test campaign:
  - `SOLT 2-port` calibration at cable ends (reference plane at SMA connectors).
  - `S11 LogMag` for return loss / mismatch analysis.
  - `S21 LogMag` for insertion loss and ripple/notch analysis.
  - Frequency sweep in the selected band (typically 1-3 GHz) with 401 or 801 points.
  - `.s2p` export in NanoVNA Saver for documentation and optional time-domain post-processing.

<a id="nanovna-saver"></a>
## NanoVNA Saver
NanoVNA Saver is the desktop companion software used in this project to acquire, visualize, and export measurements from the NanoVNA.

<img src="./images/nano_vna_saver.png" alt="NanoVNA Saver interface" width="700">

Official page:
- https://nanovna.com/?page_id=90

Main functions used/available (per official page):
- Read measurement data from NanoVNA directly on PC.
- Segment frequency spans to get higher point density than the base sweep (reported use up to >10k points).
- Average sweeps to improve result stability, especially at higher frequencies.
- Display multiple chart formats for `S11` and `S21` (for example: Smith, LogMag, Phase, VSWR).
- Use markers with derived values (impedance, VSWR, Q, equivalent L/C).
- Export and import 1-port/2-port Touchstone files.
- Use active trace and reference trace comparison.
- Live updates, including multi-segment sweeps.
- In-app calibration support and export of plot images.

<a id="repository-structure"></a>
## Repository structure
- `Signal_Integrity_2L_Simplified/`: 2-layer design files.
- `Signal_Integrity_4L_Simplified/`: 4-layer design files.

<a id="project-links"></a>
## Project links
| Project | GitHub files | Altium 365 |
|---|---|---|
| 2 Layers | [Signal_Integrity_2L_Simplified](./Signal_Integrity_2L_Simplified/) | [Open in Altium 365](https://365.altium.com/files/987D5E5E-FB93-4543-97C9-5E7F92589402) |
| 4 Layers | [Signal_Integrity_4L_Simplified](./Signal_Integrity_4L_Simplified/) | [Open in Altium 365](https://365.altium.com/files/C1313425-98CB-430A-858C-CD9CB5CA370C) |

<a id="tests-script-report"></a>
## Tests (Script and Report)
For complete test execution, measurement order, acceptance criteria, and result table, see:

- [README-tests.md](./README-tests.md) (English)
- [README-testes.md](./README-testes.md) (Portuguese)


