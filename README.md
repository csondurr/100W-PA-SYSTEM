# 100W-PA-SYSTEM

**Wideband 0.1–10 GHz / 100 W-Class Modular RF Power Amplifier Platform**  
**Hardware Design: Cem Sondur**

This repository contains the fabrication packages, PCB source files, drill data, assembly-position files, DRC reports and supporting manufacturing documentation for a modular **100 W-class RF power amplifier system covering 0.1 GHz to 10 GHz**. The design divides the full operating spectrum into five dedicated RF power-amplifier boards and one separate control/protection board so that each frequency region can be optimized around the most appropriate GaN power transistor technology, matching strategy, power-combining structure, bias conditions and thermal requirements.

> **Important:** the five RF boards are independent band modules. The project is not based on forcing one transistor or one matching network to operate efficiently across the entire 0.1–10 GHz span. The full spectrum is covered by selecting the appropriate PA module for the required operating band.

---

## Repository Package / Frequency Map

The ZIP files in the repository are intentionally separated so that each RF band can be downloaded and manufactured independently.

| Repository file | Internal board | Frequency coverage | Main RF power device | Device count | Function |
|---|---:|---:|---|---:|---|
| **R1.zip** | Complete system package | **0.1–10.0 GHz** | All devices below | — | Complete fabrication release containing B1–B5 and CTRL |
| **R2.zip** | **B1** | **0.1–0.5 GHz** | T1G4020036-FL | 1 package | Low-band 100 W-class PA module |
| **R3.zip** | **B2** | **0.5–1.5 GHz** | T1G4020036-FL | 1 package | Lower-mid-band 100 W-class PA module |
| **R4.zip** | **B3** | **1.5–3.0 GHz** | T1G4020036-FL | 1 package | Mid-band 100 W-class PA module |
| **R5.zip** | **B4** | **3.0–6.0 GHz** | QPD1035 | 8 devices | Multi-device upper-mid-band PA module |
| **R6.zip** | **B5** | **6.0–10.0 GHz** | TGF2979-SM | 12 devices | Multi-device microwave PA module |
| **R7.zip** | **CTRL** | No direct RF band | Control / bias hardware | — | Bias, enable, monitoring and protection interface |

### Quick interpretation

- **R1** is the complete 0.1–10 GHz fabrication package.
- **R2** is the 0.1–0.5 GHz power-amplifier board.
- **R3** is the 0.5–1.5 GHz power-amplifier board.
- **R4** is the 1.5–3.0 GHz power-amplifier board.
- **R5** is the 3.0–6.0 GHz power-amplifier board.
- **R6** is the 6.0–10.0 GHz power-amplifier board.
- **R7** is the common control/protection board and therefore has no RF operating band of its own.

---

# 1. Project Objective

The objective of **100W-PA-SYSTEM** is to create a practical, scalable high-power RF platform capable of covering an exceptionally wide **0.1–10 GHz** spectrum while maintaining a **100 W-class output-power target** at system level for the selected operating band.

A single broadband PA transistor and a single broadband matching network become increasingly difficult to realize efficiently as frequency coverage expands over several octaves. Device parasitics, optimum load impedance, package inductance, transmission-line behavior, gain roll-off, combining loss, thermal density and stability requirements vary strongly with frequency. For that reason this design uses a **band-segmented architecture**.

The complete system is divided into:

- B1: 0.1–0.5 GHz
- B2: 0.5–1.5 GHz
- B3: 1.5–3.0 GHz
- B4: 3.0–6.0 GHz
- B5: 6.0–10.0 GHz
- CTRL: common control, enable, bias and protection interface

This approach allows the RF transistor, device count, matching implementation and power-combining method to be selected specifically for each band rather than accepting the large efficiency and stability compromises of a single 100:1-frequency-ratio amplifier.

---

# 2. System-Level Hardware Architecture

A typical operating path is:

```text
RF SOURCE / SDR / EXCITER
          │
          ▼
   BAND SELECTION
          │
          ▼
┌─────────────────────────────┐
│ Selected RF PA Module       │
│                             │
│ Input Interface             │
│      ↓                      │
│ Driver / Gain Section       │
│      ↓                      │
│ Input Matching              │
│      ↓                      │
│ GaN Power Stage(s)          │
│      ↓                      │
│ Output Matching             │
│      ↓                      │
│ Power Combiner              │
│ (B4/B5 where required)      │
│      ↓                      │
│ Forward/Reverse Monitoring  │
│      ↓                      │
│ 50 Ω RF Output              │
└─────────────────────────────┘
          │
          ▼
FILTER / RF SWITCH / ANTENNA
or 50 Ω HIGH-POWER LOAD
```

The CTRL board operates alongside the selected PA module and provides the interface required for controlled power-up, band enable, monitoring and system protection.

---

# 3. Band B1 — 0.1 to 0.5 GHz

**Repository package:** `R2.zip`  
**Internal board:** B1  
**Operating range:** **100–500 MHz**  
**Power-device family:** **T1G4020036-FL**  
**Quantity:** **1 package**

B1 is the lowest-frequency power-amplifier board. At these frequencies, electrically long quarter-wave or distributed microwave matching networks quickly become physically large, so the design philosophy differs from the upper microwave bands.

The B1 hardware is intended around a **broadband low-frequency matching approach**, using practical lumped-element / transformer-style matching concepts together with short RF interconnects and a low-inductance return path.

Key hardware priorities in this band are:

- very low impedance in drain-current paths,
- high-current copper distribution,
- minimum common-source inductance,
- high-current RF/DC decoupling,
- broadband impedance transformation,
- stable negative-gate bias,
- controlled drain sequencing,
- high-power output interconnection,
- strong chassis/baseplate grounding.

Because the T1G4020036-FL is a high-power GaN device package, its mechanical mounting and source/flange thermal connection are part of the RF design, not simply mechanical details.

---

# 4. Band B2 — 0.5 to 1.5 GHz

**Repository package:** `R3.zip`  
**Internal board:** B2  
**Operating range:** **500 MHz–1.5 GHz**  
**Power-device family:** **T1G4020036-FL**  
**Quantity:** **1 package**

B2 continues to use the T1G4020036-FL platform but moves into a frequency region where distributed effects become increasingly significant.

The preferred RF matching philosophy is a **low-Q, multi-section broadband transformation**, balancing bandwidth against loss and sensitivity to component or PCB tolerances.

Primary design considerations include:

- broadband input return loss,
- broadband output power match,
- preservation of gain across the complete band,
- suppression of low-frequency instability,
- bias-network isolation from the RF path,
- minimization of drain-feed parasitic inductance,
- low-loss ground return,
- adequate component voltage/current rating.

The B2 board is therefore not simply a frequency-scaled copy of B1; the RF matching and physical layout must account for the shorter electrical wavelength and increased influence of PCB geometry.

---

# 5. Band B3 — 1.5 to 3.0 GHz

**Repository package:** `R4.zip`  
**Internal board:** B3  
**Operating range:** **1.5–3.0 GHz**  
**Power-device family:** **T1G4020036-FL**  
**Quantity:** **1 package**

B3 is the highest-frequency section based on the T1G4020036-FL device family.

At this point the PCB is an increasingly important part of the matching network. Microstrip dimensions, dielectric properties, reference-plane continuity, via placement and package transition geometry have direct impact on the amplifier response.

The intended matching structure is based primarily on **distributed transmission-line transformation**, including stepped microstrip sections and shunt tuning structures where required.

The key B3 hardware challenges are:

- maintaining a continuous 50 Ω environment at interfaces,
- minimizing abrupt transmission-line discontinuities,
- controlling source/flange grounding inductance,
- broadband device stabilization,
- preventing bias-line resonance,
- minimizing package-to-PCB transition parasitics,
- preserving adequate power and efficiency toward 3 GHz.

---

# 6. Band B4 — 3.0 to 6.0 GHz

**Repository package:** `R5.zip`  
**Internal board:** B4  
**Operating range:** **3.0–6.0 GHz**  
**Power-device family:** **QPD1035**  
**Quantity:** **8 devices**

B4 changes architecture significantly. Instead of relying on one high-power package, the output requirement is distributed across **eight QPD1035 GaN PA branches**.

The multi-device architecture provides output-power headroom and reduces the requirement placed on any single transistor while enabling operation to 6 GHz.

Conceptually, B4 consists of:

```text
                 ┌─ PA1 ─┐
                 ├─ PA2 ─┤
                 ├─ PA3 ─┤
RF INPUT → SPLIT ├─ PA4 ─┤ COMBINE → RF OUTPUT
                 ├─ PA5 ─┤
                 ├─ PA6 ─┤
                 ├─ PA7 ─┤
                 └─ PA8 ─┘
```

The branch architecture uses a corporate distribution concept so that each PA receives a controlled amplitude and phase relationship.

### B4 power-tree target

The selected architecture is based on a **three-stage binary input distribution structure** and a high-power output combining network. The design target for the final combining network is to keep branch imbalance and insertion loss low enough that the available transistor power is not wasted inside the combiner.

Important RF criteria include:

- input/output return loss target: approximately 15 dB or better,
- branch amplitude imbalance target: ≤ 0.5 dB,
- branch phase imbalance target: ≤ 5°,
- total B4 splitter/combiner insertion-loss target: approximately ≤ 1 dB.

The tolerance study associated with the power tree produced a tighter manufacturing/trim objective of approximately:

- branch amplitude sigma ≤ 0.10 dB,
- branch phase sigma ≤ 1°.

These targets are important because power combining is highly sensitive to phase and amplitude mismatch. An eight-device amplifier only produces useful combined power when the branches arrive at the combining node coherently.

---

# 7. Band B5 — 6.0 to 10.0 GHz

**Repository package:** `R6.zip`  
**Internal board:** B5  
**Operating range:** **6.0–10.0 GHz**  
**Power-device family:** **TGF2979-SM**  
**Quantity:** **12 devices**

B5 is the highest-frequency and most RF-layout-sensitive section of the system.

The architecture uses **twelve TGF2979-SM GaN devices** so that the required 100 W-class system output can be obtained with sufficient combining margin in the 6–10 GHz region.

A direct single-junction twelve-way high-power combiner would impose severe impedance, phase-control, geometry and loss constraints. The system therefore follows a **hierarchical corporate combining architecture**.

A simplified view is:

```text
RF INPUT
   │
   ├── 4-way group → 4 × PA
   │
   ├── 4-way group → 4 × PA
   │
   └── 4-way group → 4 × PA

       12 PA branches
              │
       hierarchical combine
              │
          RF OUTPUT
```

The output concept uses **three groups of four PA branches**, followed by a final combining stage. This makes the physical network more manageable and provides a practical route toward amplitude and phase control.

### B5 power-tree targets

- return loss target: approximately 15 dB or better,
- amplitude imbalance target: ≤ 0.5 dB,
- phase imbalance target: ≤ 5°,
- total splitter/combiner insertion-loss target: approximately ≤ 1.5 dB.

Because electrical wavelength is short at 10 GHz, small changes in line length, dielectric thickness, connector launch, pad geometry or component placement can create meaningful phase error. For this reason B5 is the section where controlled-impedance fabrication and precise mechanical assembly are most critical.

---

# 8. Power Semiconductor Strategy

Three GaN RF power-device families are used across the system:

### T1G4020036-FL
Used in:

- B1: 0.1–0.5 GHz
- B2: 0.5–1.5 GHz
- B3: 1.5–3.0 GHz

The device is used because the lower three bands require very high RF power capability with a comparatively small number of active packages.

### QPD1035
Used in:

- B4: 3.0–6.0 GHz

Eight devices are operated as parallel RF power branches and combined at the output.

### TGF2979-SM
Used in:

- B5: 6.0–10.0 GHz

Twelve devices are used because the high-frequency output capability of an individual microwave transistor is lower than the overall system-level target. Combining multiple devices provides the required total power headroom.

---

# 9. 100 W-Class Output Philosophy

The **100 W** designation refers to the intended RF output class of the selected operating band.

The five RF boards should not be interpreted as five simultaneously combined 100 W outputs. They are frequency-specific amplifier modules forming a common wideband platform.

For a practical complete transmitter, the amplifier modules can be integrated with external or system-level:

- band-selection switching,
- preselection filters,
- harmonic filtering,
- RF routing,
- output switching,
- antenna selection,
- directional monitoring,
- protection logic.

This makes the design suitable as a reusable RF power stage rather than a single fixed-frequency amplifier.

---

# 10. RF Input and Output Environment

The system is designed around a **50 Ω RF environment**.

All external RF sources, driver stages, test equipment, filters, switches, loads and antennas connected to the PA should therefore present a properly controlled 50 Ω impedance over the intended operating band.

At 100 W RF power levels, impedance mismatch is not a minor efficiency issue. High VSWR can significantly increase device voltage/current stress and can damage the final transistors.

The amplifier should therefore never be operated at high power without:

- an appropriate 50 Ω high-power dummy load, or
- a validated antenna system with acceptable VSWR.

High-power RF connectors and launches must be selected according to the actual frequency and power requirements of the assembled system.

---

# 11. PCB / Transmission-Line Architecture

The RF boards use a four-layer architecture intended for RF power applications.

The design baseline is based around:

- RF laminate family: **Rogers RO4350B**
- RF dielectric target: approximately **0.508 mm** between the RF layer and immediate reference plane
- top copper target: approximately **35 µm / 1 oz**
- external impedance: **50 Ω**
- controlled-impedance fabrication required for the microwave boards

A preferred RF layer assignment is:

```text
L1  → RF traces, matching networks and RF components
L2  → continuous RF ground reference
L3  → drain/gate power distribution, bias and control
L4  → ground / shielding / thermal return
```

For the nominal dielectric assumptions used during design, the first-order 50 Ω microstrip width is approximately **1.1 mm**. This is a seed value only; the PCB fabricator must calculate the final controlled-impedance line width using its actual pressed dielectric thickness, copper thickness, copper roughness and process data.

---

# 12. Grounding Strategy

Grounding is fundamental to the PA performance.

The ground structure is required to provide:

- low RF return inductance,
- short transistor source-to-ground path,
- low common-source inductance,
- stable matching behavior,
- controlled microstrip impedance,
- reduction of unwanted coupling between branches,
- effective RF shielding,
- thermal transfer path where the package architecture requires it.

Ground-via placement around RF transitions and power-device regions should be dense enough to prevent the PCB reference plane from behaving as an unintended resonant structure.

For microwave bands, ground continuity directly affects phase balance and therefore directly affects multi-device combining efficiency.

---

# 13. Bias Architecture

GaN RF power transistors require controlled bias sequencing. A negative gate voltage must be established before the high positive drain supply is enabled.

The hardware architecture therefore separates:

- drain power,
- negative gate bias,
- enable control,
- monitoring,
- fault control.

The intended power domains are approximately:

- **B1/B2/B3:** 50 V-class drain domain
- **B4:** 50 V-class drain domain
- **B5:** 32 V-class drain domain

The exact gate voltage is not treated as a fixed universal voltage because GaN devices should normally be biased to the required quiescent drain current. Gate voltage is therefore adjusted according to the device operating point.

### Required power-up concept

```text
1. Establish negative gate bias
2. Verify safe gate state
3. Enable drain supply
4. Adjust / confirm quiescent current
5. Apply RF drive
```

### Required power-down concept

```text
1. Remove RF drive
2. Return gate to safe negative state
3. Disable drain supply
4. Remove remaining bias/control power
```

Incorrect sequencing can permanently damage depletion-mode GaN devices.

---

# 14. CTRL Board — R7.zip

The CTRL board is the common system-management PCB.

Unlike B1–B5, it does not process the high-power RF signal directly and therefore does not have an assigned RF operating band.

Its role is to provide the interface required for functions such as:

- B1–B5 enable control,
- bias sequencing,
- power-stage interlock,
- drain-voltage monitoring,
- drain-current monitoring,
- temperature monitoring,
- fault indication,
- system shutdown,
- external controller communication,
- protection coordination.

The control system is intentionally separated from the RF power modules so that the high-current/high-field PA environment does not have to share the same physical PCB region as sensitive control electronics.

This modular approach also simplifies serviceability: one RF band can be replaced or modified without redesigning the complete control platform.

---

# 15. Protection Requirements

A practical 100 W-class GaN PA should not depend only on the transistor's intrinsic ruggedness. The surrounding system should monitor the conditions that can destroy the device.

Recommended protection functions include:

- drain over-current detection,
- over-temperature detection,
- reflected-power / VSWR detection,
- gate-bias loss detection,
- drain undervoltage / overvoltage supervision,
- RF-drive interlock,
- startup sequencing,
- emergency shutdown.

A conservative engineering baseline used during development is:

- over-current trip near approximately 120% of the intended nominal operating current,
- thermal warning around 70 °C baseplate temperature,
- thermal shutdown around 80 °C baseplate temperature,
- reflected-power / VSWR supervision appropriate to the final RF load.

These values are system-level engineering starting points and should be finalized during prototype characterization.

---

# 16. Thermal and Mechanical Design

High RF output power also means high DC input power and significant heat generation.

The system must therefore be mounted to a mechanically rigid and thermally conductive structure. The intended mechanical concept uses an **aluminium baseplate**, with forced-air cooling and provision for stronger cooling in the highest-power-density section.

A practical system implementation should provide:

- flat aluminium baseplate,
- controlled transistor-to-baseplate thermal interface,
- low thermal resistance TIM where appropriate,
- appropriate mounting pressure/torque,
- forced-air heat sink,
- direct airflow across the hottest region,
- temperature sensing close to the PA devices,
- optional cold-plate compatibility for B5.

The 6–10 GHz B5 section has the highest device count and therefore requires particular attention to heat spreading.

Thermal design must be validated at the real operating duty cycle. CW operation is substantially more demanding than low-duty-cycle pulsed operation.

---

# 17. Multi-Device Power Combining

B4 and B5 depend on coherent RF power combining.

For an ideal N-way combiner with equal branches, each branch reaches the combining node with the same amplitude and phase. In a real PCB, however, the following cause imbalance:

- microstrip length variation,
- dielectric variation,
- copper etching tolerance,
- transistor gain/phase spread,
- matching-component tolerance,
- connector discontinuity,
- asymmetric grounding,
- temperature gradients.

The B4/B5 architecture therefore treats the power tree as a critical RF component rather than simply PCB routing.

Analytical tolerance evaluation produced the following optimized P95 results for the power-tree concept:

| Band | P95 amplitude imbalance | P95 phase imbalance | P95 insertion loss | Analytical gate |
|---|---:|---:|---:|---|
| B4 | ~0.418 dB | ~4.234° | ~0.510 dB | PASS |
| B5 | ~0.452 dB | ~4.457° | ~0.500 dB | PASS |

These results establish practical amplitude/phase-control requirements for the physical corporate network.

---

# 18. Matching Networks

The matching strategy changes with frequency because the electrical size of the PCB and the transistor parasitics change strongly across 0.1–10 GHz.

### B1 — 0.1–0.5 GHz
Broadband lumped / transformer-style low-frequency matching.

### B2 — 0.5–1.5 GHz
Multi-stage low-Q broadband transformation.

### B3 — 1.5–3.0 GHz
Stepped microstrip and distributed tuning sections.

### B4 — 3.0–6.0 GHz
Distributed branch matching around the QPD1035 PA devices.

### B5 — 6.0–10.0 GHz
Microwave distributed matching around the TGF2979-SM devices, with layout and package parasitics becoming dominant design parameters.

In every band, the output network must perform a **power match**, not merely a small-signal conjugate match. Final RF characterization therefore requires large-signal validation at the intended drain voltage, bias and drive level.

---

# 19. Fabrication Package Contents

Each RF-board ZIP contains the information required to inspect and manufacture the PCB geometry.

Typical directory structure:

```text
ASSEMBLY/
DOCS/
DRILL/
GERBER/
SHA256_MANIFEST.csv
```

### `GERBER/`
Contains PCB photoplot data, including the copper, mask, silkscreen and board-outline layers.

Typical files include:

```text
*-F_Cu.gtl          Top copper
*-In1_Cu.g1         Inner copper layer 1
*-In2_Cu.g2         Inner copper layer 2
*-B_Cu.gbl          Bottom copper
*-F_Mask.gts        Top solder mask
*-B_Mask.gbs        Bottom solder mask
*-F_Silkscreen.gto  Top silkscreen
*-B_Silkscreen.gbo  Bottom silkscreen
*-Edge_Cuts.gm1     Board profile
*-job.gbrjob        Gerber job information
```

### `DRILL/`
Contains Excellon drilling information and drill-map documentation.

### `ASSEMBLY/`
Contains component-position information such as the CSV placement file.

### `DOCS/`
Contains design-reference files including the KiCad PCB file, DRC data and PCB PDF documentation.

### `SHA256_MANIFEST.csv`
Contains file hashes that can be used to check that fabrication data has not been unintentionally modified after release.

---

# 20. Complete Package — R1.zip

`R1.zip` is the system-level package.

It contains the fabrication data for:

```text
B1  0.1–0.5 GHz
B2  0.5–1.5 GHz
B3  1.5–3.0 GHz
B4  3.0–6.0 GHz
B5  6.0–10.0 GHz
CTRL control/protection board
```

It also contains system-level documentation such as DRC results and export information.

If the goal is to archive or review the complete PA hardware release, download **R1.zip**. If only one particular frequency module is required, use **R2–R7** according to the frequency table at the beginning of this README.

---

# 21. Design Rule Check Status

The repository includes `reports/DRC_SUMMARY.json`.

The recorded KiCad DRC status for the released six-board set is:

| Board | Errors | Warnings |
|---|---:|---:|
| B1 | 0 | 0 |
| B2 | 0 | 0 |
| B3 | 0 | 0 |
| B4 | 0 | 0 |
| B5 | 0 | 0 |
| CTRL | 0 | 0 |

This confirms that the released PCB geometry passes the defined KiCad design-rule checks.

DRC should not be confused with RF performance validation. DRC verifies PCB geometrical/rule consistency; RF output power, matching, stability, efficiency, thermal performance and high-power ruggedness must still be verified on the manufactured hardware under controlled laboratory conditions.

---

# 22. Recommended Prototype Bring-Up Procedure

A newly assembled PA should never be powered immediately at full RF drive.

A controlled bring-up sequence is recommended:

### Stage 1 — Visual / assembly inspection

Check:

- transistor orientation,
- solder joints,
- gate/drain isolation,
- connector soldering,
- heatsink/baseplate contact,
- mounting hardware,
- shorts between supply rails and ground.

### Stage 2 — Unpowered measurements

Verify:

- drain-to-ground resistance,
- gate-to-ground resistance,
- RF input/output DC isolation where expected,
- continuity of all ground connections.

### Stage 3 — Gate bias only

Apply the negative gate supply first and verify that the expected gate bias reaches each transistor branch.

### Stage 4 — Drain supply with RF disabled

Current-limit the laboratory supply and enable the drain voltage. Adjust bias until the intended quiescent current is obtained.

### Stage 5 — Small-signal RF test

Before high power, characterize:

- S11,
- S21,
- S22,
- gain flatness,
- unexpected oscillation,
- out-of-band instability.

### Stage 6 — Controlled power increase

Use a high-power 50 Ω load and increase RF drive gradually while monitoring:

- output power,
- drain voltage,
- drain current,
- gain compression,
- PAE / drain efficiency,
- harmonic content,
- reflected power,
- transistor/baseplate temperature.

### Stage 7 — Full-power validation

Only after stable operation at reduced power should the system be tested toward the full 100 W-class operating point.

---

# 23. Recommended RF Laboratory Equipment

A serious validation setup for the system should include:

- vector network analyzer,
- RF signal generator / SDR exciter,
- calibrated RF power meter,
- high-power directional coupler,
- spectrum analyzer,
- calibrated high-power attenuators,
- 50 Ω dummy load with adequate power rating,
- current-limited DC power supply,
- negative gate-bias supply,
- oscilloscope,
- thermal camera or thermocouples,
- appropriate RF cables/connectors for each band.

Testing a 100 W microwave PA directly into sensitive measurement equipment without the required attenuation/coupling network can destroy the test equipment.

---

# 24. Safety

This project involves potentially hazardous RF and DC power levels.

The hardware can include:

- tens of volts of high-current DC supply,
- high RF voltages and currents,
- components capable of reaching damaging temperatures,
- RF power levels capable of damaging receivers, instruments and improperly rated loads.

Always:

- remove RF drive before changing connections,
- power down before touching the PCB,
- use a properly rated 50 Ω load,
- verify connector power handling,
- provide forced cooling,
- use current-limited supplies during first power-up,
- follow correct GaN gate/drain sequencing,
- maintain safe RF exposure practices.

---

# 25. Engineering Status

This repository should be treated as an **engineering prototype hardware/fabrication release**.

The PCB fabrication data and DRC reports provide a concrete hardware baseline, while final product qualification requires physical RF validation. In particular, a production-qualified version should be supported by measured or validated data for:

- large-signal matching across every band,
- output power across the full operating range,
- efficiency / PAE,
- unconditional or application-appropriate stability,
- B4/B5 multiport power-tree behavior,
- harmonic performance,
- reflected-power tolerance,
- thermal performance at duty cycle,
- device-to-device variation,
- final controlled-impedance stack-up,
- long-duration reliability.

The repository therefore provides a transparent hardware baseline without implying that a DRC result alone constitutes RF power qualification.

---

# 26. System Summary

```text
PROJECT              100W-PA-SYSTEM
DESIGN                Cem Sondur
ARCHITECTURE          Modular RF Power Amplifier Platform
FULL COVERAGE         0.1–10.0 GHz
OUTPUT CLASS          100 W-class / selected band
RF IMPEDANCE          50 Ω

B1 / R2               0.1–0.5 GHz
DEVICE                1 × T1G4020036-FL package

B2 / R3               0.5–1.5 GHz
DEVICE                1 × T1G4020036-FL package

B3 / R4               1.5–3.0 GHz
DEVICE                1 × T1G4020036-FL package

B4 / R5               3.0–6.0 GHz
DEVICE                8 × QPD1035
ARCHITECTURE          Multi-branch corporate split/combine

B5 / R6               6.0–10.0 GHz
DEVICE                12 × TGF2979-SM
ARCHITECTURE          Hierarchical multi-branch split/combine

CTRL / R7             Common bias / enable / monitoring / protection board

R1                    Complete B1+B2+B3+B4+B5+CTRL fabrication package
```

---

## Author

**Cem Sondur**  
Electrical & Electronics Engineering  
RF / Microwave Engineering · Signal Processing · PCB & System Design

---

### Repository note

For anyone reviewing the design for the first time: **start with the Repository Package / Frequency Map at the top of this README, then inspect the individual R2–R7 packages for the specific PA module of interest. R1.zip contains the complete system fabrication release.**


## Repository maintenance

**Evidence boundary:** Design and DRC package only. The 100 W-class claims are system targets until demonstrated by calibrated, load-rated, thermally monitored hardware tests.

- [Validation status](docs/VALIDATION.md)
- [Contribution guide](CONTRIBUTING.md)
- [Safety and security](SECURITY.md)
- [Citation metadata](CITATION.cff)

## License

Copyright (c) 2026 Cem Sondur. Distributed under the [MIT License](LICENSE). Component models and other third-party material remain subject to their original licenses.
