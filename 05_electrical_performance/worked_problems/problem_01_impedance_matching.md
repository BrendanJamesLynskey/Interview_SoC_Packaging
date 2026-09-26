# Worked Problem 01: Impedance Matching

## Problem Statement

Design a 50-ohm single-ended microstrip trace and a 100-ohm differential pair on a package substrate with ABF dielectric (Dk = 3.3, Df = 0.015). The dielectric thickness to the ground reference plane is 25 micrometers. The copper thickness is 12 micrometers. Calculate the required trace widths and estimate the insertion loss at 14 GHz (Nyquist frequency for 28 Gbps NRZ) for a 20 mm trace length.

---

## Worked Solution

### Step 1: Single-Ended 50-Ohm Microstrip Width

Using the microstrip impedance formula (Hammerstad approximation):

```
Z0 = (87 / sqrt(Dk + 1.41)) * ln(5.98 * h / (0.8 * w + t))
```

Where h = 25 um (dielectric height), t = 12 um (copper thickness), Dk = 3.3.

```
50 = (87 / sqrt(3.3 + 1.41)) * ln(5.98 * 25 / (0.8w + 12))
50 = (87 / sqrt(4.71)) * ln(149.5 / (0.8w + 12))
50 = 40.08 * ln(149.5 / (0.8w + 12))
ln(149.5 / (0.8w + 12)) = 1.247
149.5 / (0.8w + 12) = 3.48
0.8w + 12 = 42.96
0.8w = 30.96
w = 38.7 um
```

**Single-ended 50-ohm trace width: approximately 39 um.**

### Step 2: Differential 100-Ohm Pair Width and Spacing

For edge-coupled microstrip differential pair:
```
Zdiff = 2 * Z_odd ≈ 2 * Z0 * (1 - 0.48 * exp(-0.96 * s/h))
```

Where s is the trace-to-trace spacing.

For Zdiff = 100 ohm, we need Z_odd = 50 ohm. Starting with narrower traces (higher Z0) and coupling:

Try w = 22 um (Z0_single ≈ 65 ohm for this width):
```
Z0_single = 40.08 * ln(149.5 / (0.8*22 + 12)) = 40.08 * ln(149.5/29.6) = 40.08 * 1.619 = 64.9 ohm
```

With s = 25 um:
```
Zdiff = 2 * 64.9 * (1 - 0.48 * exp(-0.96 * 25/25))
Zdiff = 129.8 * (1 - 0.48 * 0.383)
Zdiff = 129.8 * (1 - 0.184) = 129.8 * 0.816 = 105.9 ohm
```

Try w = 24 um, s = 22 um:
```
Z0_single = 40.08 * ln(149.5/31.2) = 40.08 * 1.567 = 62.8 ohm
Zdiff = 2 * 62.8 * (1 - 0.48 * exp(-0.96 * 22/25))
Zdiff = 125.6 * (1 - 0.48 * 0.430) = 125.6 * 0.794 = 99.7 ohm
```

**Differential 100-ohm pair: w = 24 um, s = 22 um (approximately).**

### Step 3: Insertion Loss Calculation at 14 GHz

**Conductor loss (alpha_c):**

Skin depth at 14 GHz:
```
delta = sqrt(1.7e-8 / (pi * 14e9 * 4*pi*1e-7))
delta = sqrt(1.7e-8 / 5.527e4) = sqrt(3.08e-13) = 0.555 um
```

Effective conductor cross-section (accounting for skin effect -- current flows in a thin skin around the trace perimeter):
```
Perimeter = 2*(w + t) = 2*(39 + 12) = 102 um
Effective area = Perimeter * delta = 102e-6 * 0.555e-6 = 56.6e-12 m^2
R_per_length = rho / A_eff = 1.7e-8 / 56.6e-12 = 300 ohm/m
```

Adding surface roughness factor (Krms = 0.5 um typical for ABF):
```
Roughness correction = 1 + (2/pi) * atan(1.4 * (Krms/delta)^2)
= 1 + 0.637 * atan(1.4 * (0.5/0.555)^2)
= 1 + 0.637 * atan(1.14) = 1 + 0.637 * 0.850 = 1.54
R_corrected = 300 * 1.54 = 463 ohm/m
```

Conductor loss:
```
alpha_c = R / (2 * Z0) = 463 / (2 * 50) = 4.63 Np/m = 40.2 dB/m = 0.402 dB/cm
```

**Dielectric loss (alpha_d):**
```
alpha_d = (pi * f * sqrt(Dk_eff) * Df) / c
Dk_eff = (Dk + 1)/2 = (3.3 + 1)/2 = 2.15 (microstrip approximation)
alpha_d = (pi * 14e9 * sqrt(2.15) * 0.015) / 3e8
= (pi * 14e9 * 1.466 * 0.015) / 3e8
= 968.7e6 / 3e8 = 3.23 Np/m = 28.0 dB/m = 0.280 dB/cm
```

**Total insertion loss for 20 mm trace:**
```
IL_conductor = 0.402 * 2.0 = 0.804 dB
IL_dielectric = 0.280 * 2.0 = 0.560 dB
IL_total_trace = 1.364 dB
```

**Add via and bump transitions (estimated):**
```
2 bump transitions: ~0.3 dB
2 via transitions: ~0.2 dB
IL_total_package = 1.364 + 0.3 + 0.2 = 1.86 dB
```

### Step 4: Summary

| Parameter | Value |
|---|---|
| 50-ohm SE trace width | 39 um |
| 100-ohm diff pair: width/space | 24 um / 22 um |
| Skin depth at 14 GHz | 0.55 um |
| Conductor loss at 14 GHz | 0.40 dB/cm |
| Dielectric loss at 14 GHz | 0.28 dB/cm |
| Total loss at 14 GHz (20 mm) | 1.86 dB (including transitions) |
| Roughness factor | 1.54x increase in conductor loss |

For 28 Gbps NRZ with a typical package insertion loss budget of 4-5 dB, the 1.86 dB from the package leaves 2.1-3.1 dB of margin -- adequate.

---

## Key Takeaways

- Conductor loss exceeds dielectric loss at fine trace widths (about 1.4:1 in this example).
- Surface roughness adds about 50% to the conductor loss here -- smooth copper interfaces are critical for high-speed.
- The skin depth at 14 GHz (0.55 um) is much less than the trace thickness (12 um), confirming strong skin effect.
- 28 Gbps NRZ is comfortable on organic substrates at 20 mm trace length; 56 Gbps PAM4 would require low-loss dielectrics.
