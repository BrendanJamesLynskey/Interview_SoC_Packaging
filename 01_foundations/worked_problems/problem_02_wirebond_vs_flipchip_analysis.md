# Worked Problem 02: Wirebond vs Flip-Chip Analysis

## Problem Statement

A mobile application processor die has 1200 I/O pads and must support LPDDR5-6400 memory interfaces and USB4 (40 Gbps). The die is 10 mm x 12 mm. Your team is debating whether wire bonding or flip-chip can meet the electrical requirements. Perform a quantitative comparison of the two approaches for the LPDDR5 interface, considering parasitics, signal integrity, and power delivery.

Assume:
- LPDDR5-6400 data rate: 6400 MT/s (3.2 GHz clock)
- 64-bit data bus with 16 address/command signals
- Maximum allowed channel insertion loss at Nyquist (3.2 GHz): 4 dB
- Maximum allowed VDD droop: 5% of 0.75 V supply (37.5 mV)
- Peak switching current for LPDDR5 interface block: 2.5 A with 100 ps rise time

---

## Worked Solution

### Step 1: Model Wire Bond Parasitics

For a wire bond from die pad to substrate pad:
- Typical wire bond length: 2.5 mm (including loop height)
- Inductance per wire: L = 1.0 nH (typical for 2.5 mm wire at 25 um diameter)
- Resistance per wire: R = 45 milliohms (copper wire, 25 um diameter, 2.5 mm length)
- Capacitance at bond pad: C_pad = 0.3 pF

The impedance of a single wire bond at 3.2 GHz:

```
Z_L = 2 * pi * f * L = 2 * 3.14159 * 3.2e9 * 1.0e-9 = 20.1 ohms
```

This 20.1 ohm inductive impedance in series with a 50-ohm signal is a severe impedance discontinuity. The reflection coefficient:

```
Gamma = Z_L / (2 * Z0 + Z_L) = 20.1 / (100 + 20.1) = 0.167
Return loss = -20 * log10(0.167) = 15.5 dB
Insertion loss contribution from reflection = -10 * log10(1 - Gamma^2) = 0.12 dB per wire bond
```

Total insertion loss for signal path (two wire bonds: die-to-substrate + substrate-to-ball):
- Reflection loss: ~0.24 dB
- Resistive loss: ~0.05 dB
- Substrate trace loss (estimated 15 mm at 0.5 dB/cm at 3.2 GHz): ~0.75 dB
- Total package insertion loss (wire bond): ~1.04 dB

This appears within budget, but the real problem is the time-domain signal quality. The wire bond inductance creates significant ringing and intersymbol interference.

### Step 2: Model Flip-Chip Bump Parasitics

For a copper pillar flip-chip bump:
- Bump height: 45 micrometers
- Inductance per bump: L = 25 pH
- Resistance per bump: R = 5 milliohms
- Capacitance: C = 30 fF

Impedance at 3.2 GHz:

```
Z_L = 2 * pi * 3.2e9 * 25e-12 = 0.50 ohms
```

This 0.5 ohm inductive impedance is negligible compared to the 50-ohm signal impedance.

Total package insertion loss (flip-chip):
- Bump reflection loss: ~0.001 dB (negligible)
- Bump resistive loss: ~0.001 dB
- Substrate trace loss (shorter routing, ~8 mm): ~0.40 dB
- Total package insertion loss (flip-chip): ~0.40 dB

### Step 3: Compare Signal Integrity Metrics

| Parameter | Wire Bond | Flip-Chip | Requirement |
|---|---|---|---|
| Series inductance per I/O | 1.0 nH | 25 pH | -- |
| Package insertion loss at 3.2 GHz | ~1.04 dB | ~0.40 dB | < 4 dB |
| Impedance discontinuity | 20.1 ohms | 0.50 ohms | Minimize |
| Return loss at 3.2 GHz | ~15.5 dB | > 30 dB | > 15 dB |

Both meet the insertion loss budget on paper, but the wire bond has almost no return loss margin and will cause significant reflections in time-domain analysis.

### Step 4: Power Delivery Analysis

For the VDD supply to the LPDDR5 interface block with 2.5 A peak current and 100 ps rise time:

Voltage droop from interconnect inductance (V = L * di/dt):
- Assume 50 power wires/bumps dedicated to LPDDR5 block

**Wire bond:**
```
L_effective = L_per_wire / N_wires = 1.0 nH / 50 = 20 pH
V_droop = L_eff * di/dt = 20e-12 * (2.5 / 100e-12) = 500 mV
```

This 500 mV droop (67% of VDD) is catastrophic. Even with 200 power wires:
```
L_effective = 1.0 nH / 200 = 5 pH
V_droop = 5e-12 * 2.5e10 = 125 mV (17% of VDD) -- still exceeds 5% target
```

**Flip-chip:**
```
L_effective = L_per_bump / N_bumps = 25 pH / 50 = 0.5 pH
V_droop = 0.5e-12 * 2.5e10 = 12.5 mV (1.7% of VDD) -- meets 5% target
```

### Step 5: I/O Density Check

- Die perimeter for wire bond: 2 * (10 + 12) = 44 mm
- At 50 um pad pitch: maximum ~880 peripheral pads
- Need: 1200 pads -- **wire bond cannot support 1200 I/Os on this die**

Flip-chip with area array at 100 um pitch:
- Available area: 10 x 12 = 120 mm-squared
- Bump count at 100 um pitch: ~12,000 positions
- 1200 I/Os easily accommodated with ~10% area utilization

### Step 6: Conclusion

| Criterion | Wire Bond | Flip-Chip | Verdict |
|---|---|---|---|
| I/O count (1200) | Not feasible (880 max) | Easily met | Flip-chip required |
| SI at 3.2 GHz | Marginal return loss | Excellent | Flip-chip far superior |
| Power droop | 125+ mV (fails) | 12.5 mV (passes) | Flip-chip required |
| USB4 at 40 Gbps | Not feasible | Feasible | Flip-chip required |

**Verdict: Flip-chip is mandatory for this design.** Wire bonding fails on three independent criteria: I/O count, power delivery, and high-speed signal quality.

---

## Key Takeaways

- Wire bond inductance (nH range) creates impedance discontinuities that become problematic above ~2 GHz.
- The V = Ldi/dt droop formula is the most important quick calculation for power delivery analysis.
- I/O count often eliminates wire bond before electrical analysis is even necessary.
- Always check multiple criteria; a design that passes on insertion loss may fail on power delivery or I/O count.
