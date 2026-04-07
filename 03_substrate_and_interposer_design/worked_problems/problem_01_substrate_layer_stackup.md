# Worked Problem 01: Substrate Layer Stackup

## Problem Statement

Design the layer stackup for an organic substrate that supports a high-performance server processor with:
- 5000 BGA balls at 1.0 mm pitch (65 mm x 65 mm BGA field)
- 2800 flip-chip bumps at 130 um pitch on a 25 mm x 25 mm die
- 4 channels DDR5-5600 (each 72 bits: 64 data + 8 ECC)
- 2x PCIe Gen5 x16 lanes (64 total lanes)
- Power delivery: 1.0 V core at 250 A, 1.8 V I/O at 20 A
- Target impedance: 50 ohm single-ended, 85 ohm differential
- Maximum 14 metal layers

---

## Worked Solution

### Step 1: Estimate Signal Routing Requirements

**DDR5 signals:**
```
4 channels * 72 data bits = 288 differential pairs (data)
4 channels * 24 address/command = 96 single-ended signals
Total DDR5 signals: ~480
```

**PCIe Gen5 signals:**
```
64 lanes * 2 (TX + RX) = 128 differential pairs
Clock/sideband: ~32 signals
Total PCIe: ~288
```

**Other I/O (USB, management, GPIO, etc.):** ~200 signals

**Total signal routing:** ~970 routable signals

### Step 2: Determine Layer Functions

With 14 layers in a 6-2-6 configuration (6 build-up top, 2 core, 6 build-up bottom):

| Layer | Function | L/S (um) | Cu thickness (um) |
|---|---|---|---|
| M1 (top) | Flip-chip bump pads + escape routing | 8/8 | 12 |
| M2 | High-speed signal (DDR5 data) | 8/8 | 12 |
| M3 | Ground plane (reference for M2, M4) | Full plane | 15 |
| M4 | High-speed signal (PCIe TX/RX) | 8/8 | 12 |
| M5 | Power plane (VDD_CORE) | Full plane | 20 |
| M6 | Ground plane | Full plane | 15 |
| M7 (core top) | Power plane (VDD_IO, VDD_AUX) | Full plane | 35 |
| M8 (core bottom) | Ground plane | Full plane | 35 |
| M9 | Signal (DDR5 address/command) | 10/10 | 12 |
| M10 | Ground plane | Full plane | 15 |
| M11 | Signal (low-speed, GPIO, misc) | 12/12 | 12 |
| M12 | Power plane (VDD_CORE return) | Full plane | 20 |
| M13 | Ground plane | Full plane | 15 |
| M14 (bottom) | BGA ball pads | 12/12 | 15 |

### Step 3: Verify Routing Capacity

**M2 (DDR5 data):** 288 differential pairs across approximately 100 mm of usable routing length at 8/8 L/S. Each differential pair needs 2 traces + 1 space = 24 um. Channel width per pair = 24 um. At 100 mm routing length with 60 mm usable substrate width: 60,000 / 24 = 2,500 pairs per cross-section. 288 pairs easily fit in one layer.

**M4 (PCIe):** 128 differential pairs. Similar calculation: easily fits in one layer.

**M9, M11 (remaining signals):** ~480 remaining signals on two layers at 10-12 um L/S. Well within capacity.

### Step 4: Impedance Calculation

For 50-ohm single-ended microstrip on M2 (referenced to M3 ground):

ABF dielectric (Dk = 3.3), dielectric thickness to ground = 25 um (typical build-up layer):

```
Using microstrip formula:
Z0 = (87 / sqrt(Dk + 1.41)) * ln(5.98 * h / (0.8 * w + t))
For Z0 = 50 ohm, h = 25 um, t = 12 um, Dk = 3.3:

50 = (87 / sqrt(4.71)) * ln(5.98 * 25 / (0.8w + 12))
50 = 40.1 * ln(149.5 / (0.8w + 12))
1.247 = ln(149.5 / (0.8w + 12))
3.48 = 149.5 / (0.8w + 12)
0.8w + 12 = 42.9
w = 38.7 um
```

Trace width for 50-ohm single-ended: approximately 39 um (feasible at 8/8 L/S).

For 85-ohm differential (DDR5 spec):
```
Differential impedance depends on trace width, spacing, and ground distance.
With w = 25 um, s = 20 um, h = 25 um, Dk = 3.3:
Zdiff ~ 85 ohm (verified by field solver)
```

### Step 5: Power Delivery Verification

**VDD_CORE at 250 A:**

Resistance through the substrate power path:
- M5 power plane (20 um Cu, approximately 30 mm path): R = rho * L / (w * t) = 1.7e-8 * 0.03 / (0.06 * 20e-6) = 0.43 milliohm
- PTH vias (core, assume 200 vias at 0.3 mm diameter, 0.4 mm length): R_per_via ~ 0.5 milliohm; R_parallel = 0.5 / 200 = 0.0025 milliohm
- Build-up microvias (stacked, 6 layers of 50 um via, 500 vias): negligible in parallel

Total DC resistance: approximately 0.5 milliohm.
IR drop: V = I * R = 250 * 0.5e-3 = 0.125 mV (negligible).

AC impedance target: Z_target = dV / dI = (0.05 * 1.0) / 250 = 0.2 milliohm.

This requires on-package decoupling capacitors. Surface-mount MLCCs on the substrate bottom (100 x 100 nF + 50 x 1 uF) plus embedded thin-film capacitors are needed to meet 0.2 milliohm impedance in the 100 MHz to 1 GHz range.

### Step 6: Final Stackup Summary

```
Total substrate thickness: ~1.2 mm
Build-up layer thickness: ~25 um dielectric per layer
Core thickness: 0.4 mm
Total layers: 14 (6-2-6)

Layer assignment:
  6 signal/escape layers (M1, M2, M4, M9, M11, M14)
  5 ground planes (M3, M6, M8, M10, M13)
  3 power planes (M5, M7, M12)
```

---

## Key Takeaways

- Signal layers should always be adjacent to ground reference planes for impedance control.
- Power planes should be paired with nearby ground planes to create low-impedance power delivery.
- The thickest copper layers (core layers) are best used for power distribution due to their lower resistance.
- Always verify routing capacity early: if signals do not fit, the layer count or L/S must change.
