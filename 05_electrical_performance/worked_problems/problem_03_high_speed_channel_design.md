# Worked Problem 03: High-Speed Channel Design

## Problem Statement

Design the package channel for a 112 Gbps PAM4 SerDes link (PCIe Gen6 / 800GbE). The signal path goes from the die bump through the package substrate to the BGA ball. The total package channel insertion loss budget is 3.5 dB at the Nyquist frequency (28 GHz for 56 GBaud PAM4). Determine the maximum allowable trace length and the substrate material requirements.

---

## Worked Solution

### Step 1: Allocate the Insertion Loss Budget

Total budget: 3.5 dB at 28 GHz.

Allocate to channel segments:
```
Bump transition (die-to-substrate): 0.3 dB
Substrate trace: X dB (to determine)
Via transitions (2 layer transitions): 0.4 dB
BGA ball transition: 0.3 dB
Remaining for trace: 3.5 - 0.3 - 0.4 - 0.3 = 2.5 dB
```

### Step 2: Calculate Loss per cm for Standard ABF

Standard ABF (Dk = 3.3, Df = 0.015):

**Conductor loss at 28 GHz (w = 24 um diff pair on 25 um dielectric):**
```
delta = sqrt(1.7e-8 / (pi * 28e9 * 4*pi*1e-7)) = 0.220 um
Perimeter = 2*(24 + 12) = 72 um
A_eff = 72e-6 * 0.220e-6 = 15.8e-12 m^2
R = 1.7e-8 / 15.8e-12 = 1076 ohm/m

Roughness correction (Krms = 0.5 um):
Factor = 1 + 0.637 * atan(1.4 * (0.5/0.220)^2) = 1 + 0.637 * atan(7.23) = 1 + 0.637 * 1.43 = 1.91
R_corrected = 1076 * 1.91 = 2055 ohm/m

alpha_c = 2055 / (2 * 50) = 20.55 Np/m = 1.78 dB/cm
```

**Dielectric loss at 28 GHz:**
```
alpha_d = pi * 28e9 * sqrt(2.15) * 0.015 / 3e8 = 6.45 Np/m = 0.56 dB/cm
```

**Total loss per cm (standard ABF):** 1.78 + 0.56 = 2.34 dB/cm

**Maximum trace length:** 2.5 / 2.34 = 1.07 cm = 10.7 mm

This is very short -- challenging for package routing that may need 15-25 mm.

### Step 3: Calculate Loss per cm for Low-Loss ABF

Low-loss ABF (Dk = 3.0, Df = 0.007, smooth copper Krms = 0.2 um):

**Conductor loss at 28 GHz:**
```
Roughness correction: 1 + 0.637 * atan(1.4 * (0.2/0.220)^2) = 1 + 0.637 * atan(1.16) = 1 + 0.637 * 0.86 = 1.55
R_corrected = 1076 * 1.55 = 1668 ohm/m
alpha_c = 1668 / 100 = 16.68 Np/m = 1.45 dB/cm
```

**Dielectric loss at 28 GHz:**
```
alpha_d = pi * 28e9 * sqrt(2.0) * 0.007 / 3e8 = 2.90 Np/m = 0.252 dB/cm
```

**Total loss per cm (low-loss ABF):** 1.45 + 0.252 = 1.70 dB/cm

**Maximum trace length:** 2.5 / 1.70 = 1.47 cm = 14.7 mm

Better, but still challenging for large packages.

### Step 4: Calculate Loss per cm for Ultra-Low-Loss Material

Ultra-low-loss substrate (Dk = 2.8, Df = 0.003, smooth copper Krms = 0.15 um):

**Conductor loss at 28 GHz:**
```
Roughness correction: 1 + 0.637 * atan(1.4 * (0.15/0.220)^2) = 1 + 0.637 * atan(0.65) = 1 + 0.637 * 0.576 = 1.37
R_corrected = 1076 * 1.37 = 1474 ohm/m
alpha_c = 1474 / 100 = 14.74 Np/m = 1.28 dB/cm
```

**Dielectric loss at 28 GHz:**
```
alpha_d = pi * 28e9 * sqrt(1.9) * 0.003 / 3e8 = 1.21 Np/m = 0.105 dB/cm
```

**Total loss per cm:** 1.28 + 0.105 = 1.39 dB/cm

**Maximum trace length:** 2.5 / 1.39 = 1.80 cm = 18.0 mm

### Step 5: Comparison Table

| Material | Dk | Df | Krms (um) | Loss/cm (dB) | Max Length (mm) |
|---|---|---|---|---|---|
| Standard ABF | 3.3 | 0.015 | 0.5 | 2.34 | 10.7 |
| Low-loss ABF | 3.0 | 0.007 | 0.2 | 1.70 | 14.7 |
| Ultra-low-loss | 2.8 | 0.003 | 0.15 | 1.39 | 18.0 |
| Glass substrate (SiO2) | 3.9 | 0.001 | 0.05 | 0.95 | 26.3 |

### Step 6: Design Recommendations for 112G PAM4

1. **Material selection**: Low-loss or ultra-low-loss ABF is mandatory. Standard ABF cannot support 112G PAM4 at typical package trace lengths.

2. **Trace length minimization**: Route SerDes signals on the shortest possible path. Use BGA breakout patterns that place SerDes balls near the die edge.

3. **Via optimization**: Anti-pad tuning and back-drilling to eliminate stubs. Each via transition must be individually optimized by 3D EM simulation.

4. **Return loss**: Target < -15 dB to 28 GHz. This requires careful impedance control at every transition.

5. **Crosstalk**: Maintain > 3W spacing between adjacent 112G diff pairs. Use ground shielding vias between pairs.

6. **Glass substrates**: For next-generation 224G PAM4 (Nyquist at 56 GHz), glass substrates with ultra-low Df may become necessary.

---

## Key Takeaways

- At 112G PAM4 (28 GHz Nyquist), package substrate loss becomes the critical bottleneck.
- Conductor loss dominates dielectric loss, and surface roughness is the #1 contributor to excess conductor loss.
- Low-loss dielectric and smooth copper are mandatory for 112G; standard ABF is inadequate.
- Trace length must be minimized: every millimeter matters at 2+ dB/cm loss.
- Glass substrates will likely be needed for 224G and beyond.
