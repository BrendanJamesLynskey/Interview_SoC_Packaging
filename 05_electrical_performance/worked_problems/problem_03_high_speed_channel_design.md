# Worked Problem 03: High-Speed Channel Design

## Problem Statement

Design the package channel for a 112 Gbps PAM4 SerDes link (e.g. 800GbE with 100G lanes; PCIe Gen6 is 64 GT/s with a 16 GHz Nyquist). The signal path goes from the die bump through the package substrate to the BGA ball. The total package channel insertion loss budget is 3.5 dB at the Nyquist frequency (28 GHz for 56 GBaud PAM4). Determine the maximum allowable trace length and the substrate material requirements.

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
delta = sqrt(1.7e-8 / (pi * 28e9 * 4*pi*1e-7)) = 0.392 um
Perimeter = 2*(24 + 12) = 72 um
A_eff = 72e-6 * 0.392e-6 = 28.2e-12 m^2
R = 1.7e-8 / 28.2e-12 = 602 ohm/m

Roughness correction (Krms = 0.5 um):
Factor = 1 + 0.637 * atan(1.4 * (0.5/0.392)^2) = 1 + 0.637 * atan(2.28) = 1 + 0.637 * 1.157 = 1.74
R_corrected = 602 * 1.74 = 1045 ohm/m

alpha_c = 1045 / (2 * 50) = 10.45 Np/m = 0.91 dB/cm
```

**Dielectric loss at 28 GHz:**
```
alpha_d = pi * 28e9 * sqrt(2.15) * 0.015 / 3e8 = 6.45 Np/m = 0.56 dB/cm
```

**Total loss per cm (standard ABF):** 0.91 + 0.56 = 1.47 dB/cm

**Maximum trace length:** 2.5 / 1.47 = 1.70 cm = 17.0 mm

This is short -- marginal for package routing that may need 15-25 mm.

### Step 3: Calculate Loss per cm for Low-Loss ABF

Low-loss ABF (Dk = 3.0, Df = 0.007, smooth copper Krms = 0.2 um):

**Conductor loss at 28 GHz:**
```
Roughness correction: 1 + 0.637 * atan(1.4 * (0.2/0.392)^2) = 1 + 0.637 * atan(0.364) = 1 + 0.637 * 0.349 = 1.22
R_corrected = 602 * 1.22 = 736 ohm/m
alpha_c = 736 / 100 = 7.36 Np/m = 0.64 dB/cm
```

**Dielectric loss at 28 GHz:**
```
alpha_d = pi * 28e9 * sqrt(2.0) * 0.007 / 3e8 = 2.90 Np/m = 0.252 dB/cm
```

**Total loss per cm (low-loss ABF):** 0.64 + 0.252 = 0.89 dB/cm

**Maximum trace length:** 2.5 / 0.89 = 2.80 cm = 28.0 mm

This covers typical package routes.

### Step 4: Calculate Loss per cm for Ultra-Low-Loss Material

Ultra-low-loss substrate (Dk = 2.8, Df = 0.003, smooth copper Krms = 0.15 um):

**Conductor loss at 28 GHz:**
```
Roughness correction: 1 + 0.637 * atan(1.4 * (0.15/0.392)^2) = 1 + 0.637 * atan(0.205) = 1 + 0.637 * 0.202 = 1.13
R_corrected = 602 * 1.13 = 680 ohm/m
alpha_c = 680 / 100 = 6.80 Np/m = 0.59 dB/cm
```

**Dielectric loss at 28 GHz:**
```
alpha_d = pi * 28e9 * sqrt(1.9) * 0.003 / 3e8 = 1.21 Np/m = 0.105 dB/cm
```

**Total loss per cm:** 0.59 + 0.105 = 0.70 dB/cm

**Maximum trace length:** 2.5 / 0.70 = 3.59 cm = 35.9 mm

### Step 5: Comparison Table

| Material | Dk | Df | Krms (um) | Loss/cm (dB) | Max Length (mm) |
|---|---|---|---|---|---|
| Standard ABF | 3.3 | 0.015 | 0.5 | 1.47 | 17.0 |
| Low-loss ABF | 3.0 | 0.007 | 0.2 | 0.89 | 28.0 |
| Ultra-low-loss | 2.8 | 0.003 | 0.15 | 0.70 | 35.9 |
| Glass substrate (SiO2) | 3.9 | 0.001 | 0.05 | 0.57 | 43.8 |

(All rows use the same conductor and dielectric model as Steps 2-4.)

### Step 6: Design Recommendations for 112G PAM4

1. **Material selection**: Low-loss or ultra-low-loss ABF is strongly preferred. Standard ABF reaches only ~17 mm, which is marginal at typical package trace lengths.

2. **Trace length minimization**: Route SerDes signals on the shortest possible path. Use BGA breakout patterns that place SerDes balls near the die edge.

3. **Via optimization**: Anti-pad tuning and back-drilling to eliminate stubs. Each via transition must be individually optimized by 3D EM simulation.

4. **Return loss**: Target < -15 dB to 28 GHz. This requires careful impedance control at every transition.

5. **Crosstalk**: Maintain > 3W spacing between adjacent 112G diff pairs. Use ground shielding vias between pairs.

6. **Glass substrates**: For next-generation 224G PAM4 (Nyquist at 56 GHz), glass substrates with ultra-low Df may become necessary.

---

## Key Takeaways

- At 112G PAM4 (28 GHz Nyquist), package substrate loss becomes the critical bottleneck.
- Conductor loss dominates dielectric loss, and surface roughness is the #1 contributor to excess conductor loss.
- Low-loss dielectric and smooth copper are strongly preferred for 112G; standard ABF is marginal (~17 mm).
- Trace length must be minimized: every millimeter matters at ~1-1.5 dB/cm loss.
- Glass substrates will likely be needed for 224G and beyond.
