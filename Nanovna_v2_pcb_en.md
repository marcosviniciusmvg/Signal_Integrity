# Practical Guide - NanoVNA V2 / NanoVNA-F V2

**Documentation base used in this guide**
- *NanoVNA-F V2 Portable Vector Network Analyzer User Guide Rev. 2.0* (firmware v0.3.0)
- *UG101E NanoVNA-FV2 Menu Structure Map v0.6.0*

**Objective**
This guide is organized for practical bench usage. It explains:
- how NanoVNA V2 works;
- where each menu function is;
- when to use each function;
- how to apply each feature in real measurements;
- practical examples for RF PCB design, validation, and debugging.

---

# 1. What NanoVNA V2 is

NanoVNA V2 is a **2-port vector network analyzer**. It measures how a device behaves over frequency.

In practice, it injects RF and measures:
- how much is reflected back;
- how much passes through to the other port.

Results are shown as S-parameters:
- **S11**: reflection at port 1;
- **S21**: transmission from port 1 to port 2;
- **S22** and **S12**: can be obtained by swapping DUT connections.

## 1.1 Main range and capabilities

According to the guide:
- frequency range: **50 kHz to 3 GHz**;
- up to **201 sweep points**;
- up to **4 traces**;
- up to **4 markers**;
- **7 memory slots** for calibration/configuration.

## 1.2 Best use cases

### S11
Use S11 for:
- antennas;
- matching networks;
- RF PCB inputs;
- 1-port filters;
- stubs;
- connectors;
- cables with TDR;
- impedance and resonance checks.

### S21
Use S21 for:
- 2-port filters;
- PCB RF interconnects;
- attenuators;
- cables;
- couplers and splitters;
- linear stage gain/loss;
- frequency response between two circuit points.

---

# 2. Recommended workflow

1. Define what will be measured.
2. Choose **S11** or **S21**.
3. Set the frequency range.
4. Choose display format.
5. Calibrate on the same range and reference plane.
6. Connect the DUT.
7. Read with markers.
8. Refine span if needed.
9. Save setup or export S1P/S2P/CSV.

## 2.1 Golden rule

**Calibrate exactly where the DUT will be connected.**

If a cable, pigtail, adapter, PCB launch, or fixture exists between VNA and DUT, that section is part of the measurement system and must be included in the reference plane.

---

# 3. Quick interface overview

## 3.1 Main screen elements
- START and STOP frequency;
- markers;
- calibration status;
- trace reference positions;
- marker table;
- trace status box;
- battery voltage;
- left/right scale;
- sweep point count.

## 3.2 Calibration indicators
- **O** = OPEN done;
- **S** = SHORT done;
- **L** = LOAD done;
- **T** = THRU done;
- **C** = calibration applied;
- **\*** = unsaved calibration;
- **c** = interpolated calibration;
- **C0...C6** = loaded memory slot.

## 3.3 Menu access
- touch menu area;
- or press middle side button.

## 3.4 Virtual keyboard
Accepts:
- numbers;
- decimal point;
- units **G**, **M**, **k**;
- **OK**.

---

# 4. Practical S-parameter interpretation

## 4.1 S11 (reflection)

Practical reading:
- lower S11 in dB usually means better match;
- lower VSWR means better match;
- in Smith chart, closer to center means closer to 50 ohm.

PCB use:
- RF front-end input check;
- antenna LC matching;
- LNA/PA adaptation point;
- SMA-to-microstrip launch validation;
- stub and PI/T network assessment.

## 4.2 S21 (transmission)

Practical reading:
- near 0 dB: low loss or near-unity gain;
- negative: insertion loss;
- positive: gain.

PCB use:
- insertion loss of on-board filters;
- compare RF trace lengths;
- validate interconnect between connectors;
- evaluate pass-band and stop-band behavior;
- verify via fence/layer transition impact.

---

# 5. NanoVNA V2 menus by function

## 5.1 DISPLAY
Path:
`DISPLAY -> TRACE / FORMAT / SCALE / REF POS / CHANNEL`

### TRACE
Enable/disable up to 4 traces and choose active trace.

### FORMAT
Main formats and typical usage:
- `LOGMAG`: return loss, insertion loss, gain, ripple, notch;
- `PHASE`: phase behavior;
- `DELAY`: group delay (mainly S21);
- `SMITH R+jX` / `SMITH R+L/C`: matching and reactive behavior (mainly S11);
- `SWR`: quick antenna tuning (S11);
- `Q FACTOR`, `POLAR`, `LINEAR`, `REAL/IMAG`, `RESISTANCE/REACTANCE` for deeper analysis.

### SCALE
Adjust vertical scale (not for Smith/Polar).

### REF POS
Move vertical reference position (not for Smith/Polar).

### CHANNEL
Select active channel:
- `S11 (REFL)`
- `S21 (THRU)`

## 5.2 MARKER
Path:
`MARKER -> SELECT / SEARCH / OPERATE / DRAG ON`

- `SELECT`: enable up to 4 markers.
- `SEARCH`: auto-find `MAXIMUM`, `MINIMUM`, left/right search, and `TRACKING`.
- `OPERATE`: use marker as new `START`, `STOP`, `CENTER`, or `SPAN`.
- `DRAG ON`: move marker table on screen.

## 5.3 STIMULUS
Path:
`STIMULUS -> START / STOP / CENTER / SPAN / CW PULSE / SIGNAL GENERATOR / PAUSE SWEEP`

- `START/STOP/CENTER/SPAN`: define sweep window.
- `CW PULSE`: pulsed RF output mode.
- `SIGNAL GENERATOR`: fixed-frequency output (`RF OUT`, `FREQ`, `0/-3/-6/-9 dB`).
- `PAUSE SWEEP`: freeze/resume measurement.

## 5.4 CAL
Path:
`CAL -> CALIBRATE / RESET / APPLY`

- `APPLY`: enable/disable calibration correction.
- `RESET`: clear current active calibration.
- `CALIBRATE`: run calibration.

### Full calibration flow
1. Set frequency range.
2. Go to `CAL -> CALIBRATE`.
3. Connect **OPEN**, press `OPEN`.
4. Connect **SHORT**, press `SHORT`.
5. Connect **LOAD**, press `LOAD`.
6. Connect `PORT1` to `PORT2`, press `THRU`.
7. Press `DONE`.
8. Save into `SAVE n`.

### Typical mistakes
- calibrating in one range and measuring in another;
- calibrating at VNA connector and later adding cable/adapter;
- forgetting THRU for S21;
- changing adapters after calibration.

## 5.5 RECALL/SAVE
Path:
`RECALL/SAVE -> RECALL / SAVE`

- `RECALL`: load saved setup/calibration.
- `SAVE`: save setup/calibration.

## 5.6 TDR
Path:
`TDR -> TDR ON / LOW PASS IMPULSE / LOW PASS STEP / BANDPASS / WINDOW / VELOCITY FACTOR`

Use TDR for S11 discontinuity visualization in time/distance domain.

Practical rules:
- higher max frequency -> better time resolution;
- smaller point spacing -> more range;
- always a tradeoff between range and resolution.

## 5.7 CONFIG
Path:
`CONFIG -> E-DELAY / L/C MATCH / SWEEP POINTS / (AVERAGE in newer fw) / TOUCH TEST / LANGSET / ABOUT / BRIGHTNESS`

- `E-DELAY`: compensate known electrical delay.
- `L/C MATCH`: propose matching networks at marker frequency.
- `SWEEP POINTS`: 11 to 201 points.
- `AVERAGE`: available in newer menu map (validate behavior on your firmware).
- `TOUCH TEST`, `LANGSET`, `ABOUT`, `BRIGHTNESS`: system utilities.

## 5.8 STORAGE
Paths:
- Rev.2.0: `STORAGE -> S1P / S2P / LIST`
- v0.6.0 map: includes `SAVE CSV`, `CSV LIST`, and related options.

- `S1P`: save 1-port data (S11).
- `S2P`: save 2-port data (S11 and S21).
- `LIST/SNP LIST/CSV LIST`: list stored files.

---

# 6. General usage examples

## 6.1 Measure an antenna
- S11 + LOGMAG/SWR/Smith;
- OPEN/SHORT/LOAD calibration;
- marker + `SEARCH -> MINIMUM`.

## 6.2 Measure an RF filter
- connect filter from `PORT1` to `PORT2`;
- S21 LOGMAG;
- full calibration with THRU;
- markers for center and -3 dB edges.

## 6.3 Measure cable loss
- cable between `PORT1` and `PORT2`;
- full calibration;
- read S21 at marker.

## 6.4 Perform matching
- measure S11;
- place marker at target frequency;
- use Smith + `L/C MATCH`;
- implement network and re-measure.

## 6.5 Use TDR on a cable
- cable on `PORT1`;
- open or short at far end;
- `TDR ON` + correct velocity factor;
- read reflection position.

---

# 7. PCB-focused examples

- Validate SMA-to-microstrip launch quality in S11.
- Tune printed antennas (433/868/915 MHz/2.4 GHz).
- Validate on-board LC filter response in S21.
- Compare layout revisions (Rev.A vs Rev.B) using same setup.
- Measure loss between RF blocks with proper access fixtures.
- Investigate layer-transition discontinuities with S11/S21 + TDR support.
- Use NanoVNA during RF board bring-up and export S1P/S2P evidence.

---

# 8. Recommended PCB procedures

## 8.1 Design-for-measurement
- reserve RF test access;
- include DNP tuning footprints;
- define measurement reference plane;
- allow block isolation with 0-ohm links/jumpers;
- keep close GND access near test points.

## 8.2 Bench best practices
- use short and consistent cables;
- avoid mechanical stress on board SMA connectors;
- recalibrate after relevant setup changes;
- document sweep, firmware, cable, range, and DUT state.

## 8.3 NanoVNA limitations
NanoVNA is highly useful, but does not replace lab-grade instruments for high-precision edge cases near upper frequency limits, tight controlled-impedance verification, or complex active-device characterization.

---

# 9. PC software

The manual indicates PC software support and NanoVNA-Saver compatibility.

Typical PC workflow:
1. connect USB-C;
2. open software;
3. select COM port;
4. connect;
5. capture traces, markers, and screenshots.

---

# 10. Serial console

Useful for automation and scripted data capture.

Common commands:
`help`, `reset`, `cwfreq`, `data`, `frequencies`, `scan`, `sweep`, `pause`, `resume`, `trace`, `marker`, `save`, `recall`, `edelay`, `capture`.

---

# 11. Firmware update

Basic process:
1. connect USB-C to PC;
2. hold middle button;
3. power on;
4. enter virtual U-disk mode;
5. copy `update.bin`;
6. reboot.

Note: older versions may require an intermediate `update.all` step.

---

# 12. Ready-to-use bench scripts

## 12.1 PCB antenna
S11 + SWR/Smith, calibrate, search minimum, adjust LC, save S1P.

## 12.2 PCB filter
S21 LOGMAG, calibrate with THRU, place markers, save S2P.

## 12.3 SMA launch
S11 LOGMAG + Smith, calibrate on correct plane, compare revisions.

## 12.4 RF interconnect between blocks
S21 LOGMAG, sweep target band, compare topologies, add PHASE if needed.

---

# 13. Common mistakes

1. Not calibrating.
2. Calibrating on wrong reference plane.
3. Using S11 when issue is transmission (S21).
4. Too few points on narrow filters.
5. Reading Smith chart without marker frequency context.
6. Comparing boards with different setups.
7. Trusting L/C MATCH blindly without re-measurement.

---

# 14. Final quick summary by function

- `DISPLAY`: view setup.
- `TRACE`: trace management.
- `FORMAT`: representation mode.
- `SCALE/REF POS`: visualization tuning.
- `CHANNEL`: S11/S21 selection.
- `MARKER/SEARCH`: precise point analysis.
- `STIMULUS`: frequency and excitation settings.
- `CAL`: measurement reliability.
- `RECALL/SAVE`: workflow repeatability.
- `TDR`: discontinuity/time-domain insight.
- `CONFIG`: compensation, matching, system settings.
- `STORAGE`: data export and documentation.

---

# 15. Conclusion

NanoVNA V2 is highly valuable in RF PCB development for quick and low-cost validation of matching, insertion loss, filter tuning, layout revision comparison, and documentation.

With disciplined calibration, consistent setup, and correct interpretation, it becomes an effective tool for RF board bring-up and debugging.

---

# 16. Firmware/menu note

This guide combines details from Rev.2.0 manual text with the v0.6.0 menu map. Menu names and options may vary slightly depending on firmware version.
