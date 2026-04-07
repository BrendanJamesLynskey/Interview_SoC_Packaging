# Worked Problem 02: TSV Analysis

## Problem Statement

A silicon interposer for a 2.5D AI accelerator package must accommodate:
- 2 logic die, each with 3000 signal I/Os and 2000 power/ground connections
- 4 HBM3 stacks, each with 1024 data signals plus 200 control/address signals and 1500 power/ground connections
- Interposer size: 55 mm x 40 mm (2200 mm-squared)
- Interposer thickness after thinning: 75 micrometers

Determine:
1. Total TSV count required
2. TSV pitch and density
3. Electrical characteristics of the TSVs
4. Mechanical stress implications

---

## Worked Solution

### Step 1: Calculate Total TSV Count

All connections from die on the top surface must pass through TSVs to reach the C4 bumps on the bottom surface that connect to the organic substrate.

**Logic die (2 die):**
```
Signal TSVs = 2 * 3,000 = 6,000
Power/ground TSVs = 2 * 2,000 = 4,000
Subtotal = 10,000 TSVs
```

**HBM3 stacks (4 stacks):**
```
Signal TSVs = 4 * (1,024 + 200) = 4,896
Power/ground TSVs = 4 * 1,500 = 6,000
Subtotal = 10,896 TSVs
```

**Additional TSVs for routing flexibility and redundancy (10%):**
```
Redundancy = (10,000 + 10,896) * 0.10 = 2,090
```

**Total TSV count:**
```
Total = 10,000 + 10,896 + 2,090 = 22,986 -> approximately 23,000 TSVs
```

### Step 2: Determine TSV Dimensions and Pitch

With 75 um interposer thickness and a target aspect ratio of 10:1:
```
TSV diameter = 75 / 10 = 7.5 um
```

Use 8 um diameter for manufacturability.

Minimum TSV pitch (typically 2-3x diameter for isolation and routing):
```
Minimum pitch = 8 * 3 = 24 um -> use 25 um pitch
```

However, TSVs are not uniformly distributed. They cluster near the die connection areas.

**TSV density check:**
```
Available interposer area = 2,200 mm^2
TSV area at 25 um pitch = 23,000 * (25e-3)^2 = 14.4 mm^2
TSV area fraction = 14.4 / 2,200 = 0.65%
```

This is well within limits. Even allowing for keep-out zones around TSV clusters, the interposer has ample area for the required TSV count.

**Effective TSV density in die attachment regions:**

Assuming TSVs are concentrated under the die areas:
```
Logic die area = 2 * (20 * 18) = 720 mm^2 (estimated)
HBM stack area = 4 * (7.5 * 11) = 330 mm^2 (estimated)
Total active area = 1,050 mm^2
TSV density = 23,000 / 1,050 = 21.9 TSVs/mm^2
```

At 25 um pitch, the maximum TSV density is:
```
Max density = 1 / (25e-3)^2 = 1,600 TSVs/mm^2
```

The required density (22 TSVs/mm^2) is only 1.4% of the maximum, confirming feasibility.

### Step 3: Calculate TSV Electrical Characteristics

**TSV resistance:**
```
R_TSV = rho * L / A
rho_Cu = 1.7e-8 ohm-m
L = 75e-6 m
A = pi * (4e-6)^2 = 50.3e-12 m^2

R_TSV = 1.7e-8 * 75e-6 / 50.3e-12 = 25.3 milliohms
```

**TSV inductance (approximate, isolated TSV):**
```
L_TSV = (mu_0 * h / (2*pi)) * [ln(2*h/r) - 1]
h = 75e-6 m, r = 4e-6 m
L_TSV = (4*pi*1e-7 * 75e-6 / (2*pi)) * [ln(2*75/4) - 1]
L_TSV = 15e-12 * [ln(37.5) - 1]
L_TSV = 15e-12 * [3.624 - 1] = 15e-12 * 2.624
L_TSV = 39.4 pH
```

With ground TSV proximity (GSG configuration), mutual inductance reduces effective loop inductance:
```
L_loop ~ 15-25 pH (estimated with adjacent ground return)
```

**TSV capacitance (MOS capacitor: Cu-SiO2-Si):**
```
C_TSV = 2 * pi * epsilon_0 * epsilon_r * h / ln(r_outer/r_inner)
epsilon_r(SiO2) = 3.9
r_inner = 4e-6 m (TSV radius)
r_outer = 4.3e-6 m (TSV radius + 0.3 um oxide liner)

C_TSV = 2*pi * 8.85e-12 * 3.9 * 75e-6 / ln(4.3/4.0)
C_TSV = 2*pi * 8.85e-12 * 3.9 * 75e-6 / 0.0723
C_TSV = 16.3e-15 / 0.0723 = 225 fF
```

Note: This is higher than typically quoted because the oxide liner is thin. With a thicker liner (1 um), capacitance drops significantly:
```
C_TSV (1 um liner) = ... / ln(5/4) = ... / 0.223 = 73 fF
```

A more typical value with engineered liner thickness is 30-80 fF per TSV.

### Step 4: Electrical Summary

| Parameter | Value | Impact |
|---|---|---|
| Resistance | 25 milliohms | Negligible for signal; contributes to IR drop for power (total IR drop through 2000 parallel power TSVs at 50 A: V = IR/N = 50 * 0.025 / 2000 = 0.6 mV) |
| Inductance (loop) | 15-25 pH | Low; contributes minimally to Ldi/dt noise |
| Capacitance | 30-80 fF | Adds load to drivers; for 23,000 TSVs total capacitance is significant for power (many pF aggregate) |

### Step 5: Mechanical Stress Analysis

TSVs introduce mechanical stress in the surrounding silicon due to the CTE mismatch between copper (17 ppm/K) and silicon (2.6 ppm/K). During thermal excursions (processing at 200-350 degrees C, operating at 80-100 degrees C):

```
Delta_T (processing) = 300 - 25 = 275 degrees C
Thermal strain = Delta_CTE * Delta_T = (17 - 2.6)e-6 * 275 = 3960 microstrain
```

This strain induces a keep-out zone (KOZ) around each TSV where active transistors should not be placed (the stress affects carrier mobility and threshold voltage). Typical KOZ radius:
```
KOZ = 3-5x TSV radius = 3 * 4 = 12 um from TSV edge
```

For an interposer (no active transistors), this is not a concern. But for 3D stacked die with TSVs through the active silicon, the KOZ reduces the usable die area.

**Interposer warpage from TSV stress:**

The aggregate thermal stress from 23,000 TSVs contributes to interposer warpage. For a 75 um thick interposer with 0.65% TSV area fraction, the warpage contribution is modest (a few tens of micrometers). The dominant warpage sources are the CTE mismatch between die, underfill, mold compound, and the interposer itself.

### Step 6: Design Recommendations

1. **TSV diameter**: 8 um with 1 um SiO2 liner (aspect ratio 9.4:1)
2. **TSV pitch**: 25 um minimum, 40 um for signal TSVs (coarser to allow routing), 25 um for power/ground TSVs (tighter packing for lower resistance)
3. **Redundant TSVs**: Include 10% spare signal TSVs with fuse-programmable rerouting
4. **Power TSV clusters**: Group power/ground TSVs in arrays of 100+ near each die's power bump field to minimize IR drop
5. **Signal TSV placement**: Co-locate with microbump pads using short (under 50 um) RDL connections to minimize stub effects

---

## Key Takeaways

- TSV count in large interposers can reach tens of thousands, but the area fraction remains small (under 2%).
- TSV electrical parasitics are low (milliohms resistance, tens of pH inductance, tens of fF capacitance) -- much better than C4 bumps.
- The CTE mismatch between copper TSVs and silicon creates stress that defines keep-out zones for active devices.
- Power delivery TSVs should be maximized and clustered to minimize IR drop at high currents.
