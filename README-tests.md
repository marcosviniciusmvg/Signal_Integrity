# Test Script and Report - 50 Ohm Impedance PCBs (2L and 4L)

[🇺🇸 English](./README-tests.md) | [🇧🇷 Português](./README-testes.md)

## Introduction
Designing RF and high-speed PCBs with predictable electrical behavior is challenging because real trace impedance does not depend only on trace width. Final behavior is strongly influenced by stackup, distance to the reference plane, effective dielectric constant, copper thickness, solder mask, connector transitions, vias, and return path continuity.

In practice, two visually similar layouts can show very different `S11` and `S21` behavior after fabrication. Small discontinuities, such as a slot in the GND plane or a return-current path change around a via transition, can generate extra reflection, ripple, and transmission notch, especially as frequency increases.

Because of that, impedance matching must be validated by measurement, not only by calculation or simulation. In RF systems and high-speed interfaces, recurring mismatch degrades power transfer efficiency, reduces signal margin, increases process sensitivity, and can compromise electrical compliance.

This script was created to make testing repeatable and comparable between 2-layer and 4-layer coupons, enabling direct correlation between layout decisions and measured performance. The goal is to produce objective data that supports design decisions, reduces iteration cycles, and improves the robustness of future boards.

---

**Project:** 50 Ohm Impedance Coupons  
**Owner:** Marcos Vinicius Goncalves  
**Date:** YYYY-MM-DD  
**VNA:** NanoVNA-F V2 (50 kHz - 3 GHz)  
**PC Software:** NanoVNA Saver  
**PCB Manufacturer:** JLCPCB  
**Goal:** Validate and compare the behavior of 50 Ohm lines (microstrip/CPWG) in different scenarios (baseline, vias, vias + return path, GND discontinuity, matching), in 2-layer and 4-layer boards.

---

## 1. Scope

### 1.1 Tested boards
- [ ] **4-layer PCB** (stackup: JLC04161H-3313)  
- [ ] **2-layer PCB** (standard FR-4)

### 1.2 Included coupons (50 Ohm only)

#### 4L PCB (as labeled on silkscreen)
1. 50 Ohm microstrip baseline (W=350)
2. 50 Ohm CPWG (W=285, G=200)
3. 50 Ohm CPWG (W=210, G=120)
4. 50 Ohm CPWG + matching (W=285, G=200)
5. 50 Ohm microstrip + vias
6. 50 Ohm microstrip + vias + return path
7. 50 Ohm microstrip + GND discontinuity (slot on L2)

#### 2L PCB (as labeled on silkscreen)
1. 50 Ohm microstrip (W=2700)
2. 50 Ohm CPWG baseline (W=800, G=200)
3. 50 Ohm CPWG (W=380, G=120)
4. 50 Ohm CPWG + matching (W=800, G=200)
5. 50 Ohm CPWG + vias
6. 50 Ohm CPWG + vias + return path
7. 50 Ohm CPWG + GND discontinuity (slot)

---

## 2. Equipment, accessories, and settings

### 2.1 Equipment
- NanoVNA-F V2
- PC with NanoVNA Saver
- SMA male-to-male cables (qty: ___, model: ___)
- SMA calibration kit: Open / Short / Load / Thru

### 2.2 Measurement setup (default)
- **Range:** Start = ___ MHz | Stop = ___ MHz  
- **Points:** ___ (401 or 801)  
- **Observed traces:**
  - S11 LogMag (dB)
  - S21 LogMag (dB)

### 2.3 Calibration
- Type: **SOLT 2-port**
- Calibration location: **cable ends (reference plane at SMA connectors)**
- Calibration slot used: ___
- Notes: ___

### 2.4 Test conditions
- SMA tightening: [ ] consistent hand-tight  [ ] torque (___ N.m)  
- Adapters used: [ ] no  [ ] yes (list: ___)  
- Ambient temperature (approx): ___ degC

---

## 3. Evaluation criteria

### 3.1 Recorded metrics
For each coupon:
- **S11 (Return Loss):**
  - worst S11 (dB) in 1-3 GHz
  - (optional) worst S11 in sub-bands (e.g., 700-1000, 1700-2200, 2500-3000 MHz)
- **S21 (Insertion Loss):**
  - S21 at 1 GHz, 2 GHz, 3 GHz (dB)
  - ripple/notch presence (yes/no + description)

### 3.2 Expectations (hypotheses)
- **Baseline:** best S11 (more negative) and smoother S21.
- **Vias vs Vias+RP:** vias+RP should reduce reflection and ripple.
- **GND discontinuity:** should increase reflection (worse S11) and ripple/notch in S21.
- **CPWG G120 vs G200:** both are 50 Ohm, but G120 tends to stronger confinement (may improve launch / may be more tolerance-sensitive).

---

## 4. Step-by-step procedure (executed)

### 4.1 Pre-test checklist
- [ ] Visual inspection (solder mask, CPW gap, plane/slot, RP vias)
- [ ] SMA connectors properly soldered (no solder invading gap)
- [ ] Cables without mechanical slack (no pulling on SMA)

### 4.2 Measurement sequence

**4L PCB** (order):
- [ ] 4L_01 Microstrip baseline
- [ ] 4L_02 CPWG W285 G200
- [ ] 4L_03 CPWG W210 G120
- [ ] 4L_05 Microstrip + vias
- [ ] 4L_06 Microstrip + vias + RP
- [ ] 4L_07 Microstrip + GND disc (L2)
- [ ] 4L_04 CPWG + matching (bypass)

**2L PCB** (order):
- [ ] 2L_01 Microstrip
- [ ] 2L_02 CPWG baseline W800 G200
- [ ] 2L_03 CPWG W380 G120
- [ ] 2L_05 CPWG + vias
- [ ] 2L_06 CPWG + vias + RP
- [ ] 2L_07 CPWG + GND disc
- [ ] 2L_04 CPWG + matching (bypass)

### 4.3 Repeatability (recommended)
Select 3 coupons per board and repeat reconnection:
- [ ] Baseline (R1, R2)
- [ ] Vias (R1, R2)
- [ ] GND disc (R1, R2)

---

## 5. Results - Master table

> Fill this table with values extracted from NanoVNA Saver (or manually).

| PCB | Coupon | Structure | W / G | Scenario | .s2p file | Worst S11 1-3GHz (dB) | S21 @1GHz (dB) | S21 @2GHz (dB) | S21 @3GHz (dB) | Ripple/Notch (yes/no) | Notes |
|---|---:|---|---|---|---|---:|---:|---:|---:|---|---|
| 4L | 01 | Microstrip | W350 | Baseline | 4L_01_....s2p |  |  |  |  |  |  |
| 4L | 02 | CPWG | W285/G200 | Baseline | 4L_02_....s2p |  |  |  |  |  |  |
| 4L | 03 | CPWG | W210/G120 | Baseline | 4L_03_....s2p |  |  |  |  |  |  |
| 4L | 05 | Microstrip | W350/W300 | Vias | 4L_05_....s2p |  |  |  |  |  |  |
| 4L | 06 | Microstrip | W350/W300 | Vias + RP | 4L_06_....s2p |  |  |  |  |  |  |
| 4L | 07 | Microstrip | W350 | GND disc (L2) | 4L_07_....s2p |  |  |  |  |  |  |
| 4L | 04 | CPWG | W285/G200 | Matching (bypass) | 4L_04_....s2p |  |  |  |  |  |  |
| 2L | 01 | Microstrip | W2700 | Baseline | 2L_01_....s2p |  |  |  |  |  |  |
| 2L | 02 | CPWG | W800/G200 | Baseline | 2L_02_....s2p |  |  |  |  |  |  |
| 2L | 03 | CPWG | W380/G120 | Baseline | 2L_03_....s2p |  |  |  |  |  |  |
| 2L | 05 | CPWG | W800 | Vias | 2L_05_....s2p |  |  |  |  |  |  |
| 2L | 06 | CPWG | W800 | Vias + RP | 2L_06_....s2p |  |  |  |  |  |  |
| 2L | 07 | CPWG | W800 | GND disc | 2L_07_....s2p |  |  |  |  |  |  |
| 2L | 04 | CPWG | W800/G200 | Matching (bypass) | 2L_04_....s2p |  |  |  |  |  |  |

---

## 6. Comparisons and analysis (fill after measurements)

### 6.1 Baseline - 4L vs 2L
- **4L microstrip baseline vs 2L microstrip**  
  Notes: ___  
- **4L CPWG baseline vs 2L CPWG baseline**  
  Notes: ___

### 6.2 CPWG: G200 vs G120 (same stackup)
- 4L: (W285/G200) vs (W210/G120)  
  Observed: ___
- 2L: (W800/G200) vs (W380/G120)  
  Observed: ___

### 6.3 Vias vs Vias + Return Path
- 4L microstrip: vias vs vias+RP  
  Observed: ___  
- 2L CPWG: vias vs vias+RP  
  Observed: ___  

### 6.4 Baseline vs GND discontinuity
- 4L (slot on L2): baseline vs GND disc  
  Observed: ___  
- 2L (slot): baseline vs GND disc  
  Observed: ___  

### 6.5 Matching (bypass)
- 4L CPWG + matching (bypass) vs CPWG baseline  
  Observed: ___  
- 2L CPWG + matching (bypass) vs CPWG baseline  
  Observed: ___  

---

## 7. Conclusions

### 7.1 Main findings
- ___
- ___
- ___

### 7.2 Layout recommendations (based on data)
- For 4L:
  - ___
- For 2L:
  - ___

### 7.3 Next steps
- [ ] Run 801-point measurements on key coupons (baseline/vias/slot)
- [ ] Perform "time-domain/TDR via VNA" on PC (optional)
- [ ] Matching adjustment (if applicable)
- [ ] Repeat measurements with another PCB sample (if available)

---

## 8. Attachments

### 8.1 Generated files
- Results folder: `tests_50ohm/YYYY-MM-DD/`
- Included .s2p list: ___

### 8.2 Setup photos
- Photo 1: cables and connected VNA (___)
- Photo 2: soldered SMA detail (___)
- Photo 3: CPW gap / via fence detail (___)

---

## 9. Report change log
| Date | Author | Change |
|---|---|---|
| YYYY-MM-DD | Marcos | Template creation |
|  |  |  |

