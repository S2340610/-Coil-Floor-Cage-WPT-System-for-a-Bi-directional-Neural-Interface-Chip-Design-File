# Coil-Floor Wireless Power Transfer System for a Bi-directional Neural Interface Chip

Open-hardware design files for the 156 kHz inductive WPT system described in:

> X. Liu and S. Wang, "Development and Evaluation of a Coil-Floor Cage Wireless Power Transfer System for a Bi-directional Neural Interface Chip," \*IEEE Journal of Solid-State Circuits\*, 2025.

The system supplements the battery of a 32-channel bi-directional ECoG headstage during freely moving murine electrophysiology, covering a 63 × 63 cm² open-field arena with a 3 × 3 modular coil-tile floor. All design files are released to allow replication outside specialist fabrication facilities.

\---

## Repository Structure

```
.
├── Coil\_housing\_model/       # OpenSCAD parametric former for the primary coil tiles
├── Receiver\_1N5819/          # Altium PCB project — receiver pipeline
│   ├── Receiver\_1n5819.PcbDoc
│   ├── Sheet2.SchDoc
│   └── Wireless\_RX\_Li-charger.PrjPcb
└── Transmitter\_H/            # Altium PCB project — H-bridge transmitter
    ├── Transmitter\_Hbridge.PcbDoc
    ├── Sheet2.SchDoc
    └── Transmitter\_H.PrjPcb
```

\---

## System Overview

|Parameter|Value|
|-|-|
|Carrier frequency|156 kHz|
|Arena coverage|63 × 63 cm² (3 × 3 tile array)|
|Tile size|21 × 21 cm²|
|Peak power transfer efficiency|60.1% at 6 cm vertical separation|
|Tilt retention at 45°|74.2% of peak PTE|
|Operational volume|56.6% of per-tile volume above coil floor|
|Headstage power budget|5 mW|
|Total system cost|≈ £90 (commodity parts)|

\---

## Hardware

### Transmitter

* **Oscillator:** TLC555 astable multivibrator, 50% duty cycle, tunable 29.8–328 kHz
* **Inverter:** CD74HC4049E hex inverter generating complementary drive signals
* **Gate drivers:** IRS2113 × 2 half-bridge drivers with bootstrap supply
* **H-bridge:** IRFZ44N × 4 N-channel MOSFETs, V\_pp = 38 V across primary tank
* **Primary tank:** Hand-wound 0.8 mm enamelled copper wire on 3D-printed PLA former, L₁ = 85.5 µH, C₀ = 12.2 nF
* **Supply:** 19 V DC adapter; on-board LM7812 (12 V gate rail) and L7805 (5 V logic rail)

### Receiver (Headstage)

* **Secondary tank:** Würth WE-WPCC 760308101303 litz-wound coil, L₂ = 47 µH, C₂ = 22.2 nF
* **Rectifier:** 1N5819 Schottky full-bridge, 2V\_f ≈ 0.68 V drop
* **LDO:** LDL1117S50R, V\_out = 5.0 V (requires V\_in ≥ 6.2 V)
* **Charger:** MCP73831T-4ADI/OT, 1 mA CC/CV profile (R\_prog = 1 MΩ)
* **Battery:** LIR1220 lithium-polymer coin cell

\---

## Replication Notes

**Coil winding:** All tiles must be wound in the same direction. Shared-edge currents then flow in opposition, so tile fields add constructively at every inter-tile boundary. A checkerboard winding creates flux nulls between tiles and degrades the operational envelope.

**Resonance tuning:** The tank capacitance follows the Thomson relation C = 1/(4π²f²L). Re-tune C₀ if the measured inductance of your hand-wound coil deviates from 85.5 µH. A single capacitor value applies to all tiles in an array of any size.

**Scaling:** Adding tiles requires only printing additional PLA formers, winding extra coils, and adjusting C₀. Drive electronics, supply voltage, and operating frequency are unchanged. The 56.6% operational volume ratio is preserved for any array size under uniform winding.

**Ferrite backing:** Self-adhesive ferrite sheet bonded to the underside of each tile raises bare-coil inductance from 76 µH to 85.5 µH (+12%) without altering the winding geometry.

\---

## Software / Tools Required

* [Altium Designer](https://www.altium.com/) — for PCB and schematic files (`.PcbDoc`, `.SchDoc`, `.PrjPcb`)
* [OpenSCAD](https://openscad.org/) — for the parametric coil former (`.scad`)

Gerber outputs and BOM are available under `Receiver\_1N5819/Project Outputs for Wireless\_RX\_Li-charger/` and `Transmitter\_H/Project Outputs for Transmitter\_H/`.

\---

## Citation

If you use these design files in your work, please cite:

```bibtex
@article{liu2025wpt,
  author  = {Liu, Xiankang and Wang, Shiwei},
  title   = {Development and Evaluation of a Coil-Floor Cage Wireless Power Transfer System for a Bi-directional Neural Interface Chip},
  note   = {Manuscript submitted for publication},

&#x20; year    = {2025}
}
```

\---

## Acknowledgement

The authors thank the Edinburgh Microelectronics group for access to bench equipment and the headstage chip used for sizing the link budget.

\---

## Licence

Hardware design files are released under [CERN Open Hardware Licence v2 – Permissive (CERN-OHL-P)](https://ohwr.org/cern_ohl_p_v2.txt).

