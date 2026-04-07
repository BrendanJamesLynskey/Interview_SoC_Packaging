# Worked Problem 02: CTE Mismatch Analysis

## Problem Statement

Estimate the thermal fatigue life of corner solder bumps for a 20 mm x 20 mm flip-chip die on an organic substrate, with and without underfill. Temperature cycling condition: -40 to +125 degrees Celsius.

Given:
- Die CTE: 2.6 ppm/K
- Substrate CTE: 16 ppm/K
- Bump pitch: 150 um, bump height (standoff): 80 um
- Corner bump DNP: 14.1 mm (diagonal/2)
- SAC305 solder, Coffin-Manson constants: C = 40, n = 2.0

---

## Worked Solution

### Step 1: Shear Strain Without Underfill

```
Delta_CTE = 16 - 2.6 = 13.4 ppm/K
Delta_T = 125 - (-40) = 165 C
DNP = 14.1 mm = 14.1e-3 m

Delta_gamma = Delta_CTE * Delta_T * DNP / h_joint
Delta_gamma = 13.4e-6 * 165 * 14.1e-3 / 80e-6
Delta_gamma = 31.16e-6 / 80e-6 = 0.389 (38.9%)
```

### Step 2: Fatigue Life Without Underfill

```
N_f = C * (Delta_gamma)^(-n)
N_f = 40 * (0.389)^(-2.0)
N_f = 40 / 0.1513 = 264 cycles
```

**Without underfill: approximately 264 cycles to failure -- fails the 1000-cycle requirement.**

### Step 3: Shear Strain With Underfill

Underfill constrains the die-substrate system, reducing the effective shear strain on individual bumps. The strain reduction depends on the underfill modulus and CTE. A simplified model uses a coupling factor (CF):

```
CF = 1 / (1 + (E_uf * A_uf) / (E_bump * A_bump * N_bumps))
```

With typical underfill (E_uf = 8 GPa, covering 20x20 mm area = 400 mm^2) and solder bumps (E_solder ~ 40 GPa, ~2000 bumps of 80 um diameter):

```
A_uf = 400 mm^2 (minus bump areas, negligible correction)
A_bump_total = 2000 * pi * (40e-3)^2 = 2000 * 5.03e-3 = 10.05 mm^2
```

The underfill area (400 mm^2) far exceeds the bump area (10 mm^2), and the underfill modulus (8 GPa) is significant. The underfill effectively constrains the system so that the local shear strain on each bump is reduced by approximately 5-10x compared to the no-underfill case.

Using a strain reduction factor of 7x (typical for well-designed underfill):
```
Delta_gamma_uf = 0.389 / 7 = 0.0556 (5.56%)
```

### Step 4: Fatigue Life With Underfill

```
N_f = 40 * (0.0556)^(-2.0)
N_f = 40 / 0.00309 = 12,945 cycles
```

**With underfill: approximately 12,945 cycles -- far exceeds the 1000-cycle requirement.**

### Step 5: Comparison

| Condition | Shear Strain | Fatigue Life | Passes 1000 cycles? |
|---|---|---|---|
| No underfill | 38.9% | 264 | No |
| With underfill (7x reduction) | 5.56% | 12,945 | Yes |
| With underfill (5x reduction) | 7.78% | 6,604 | Yes |
| With underfill (10x reduction) | 3.89% | 26,427 | Yes |

### Step 6: Sensitivity Analysis

The fatigue life is extremely sensitive to strain because of the power-law relationship (n = 2):

| Strain reduction factor | Strain (%) | Life (cycles) | Improvement |
|---|---|---|---|
| 1x (no underfill) | 38.9 | 264 | 1x |
| 3x | 13.0 | 2,374 | 9x |
| 5x | 7.8 | 6,604 | 25x |
| 7x | 5.6 | 12,945 | 49x |
| 10x | 3.9 | 26,427 | 100x |

The fatigue life improves as the square of the strain reduction factor (since n = 2).

---

## Key Takeaways

- Without underfill, a 20 mm flip-chip die on organic substrate fails thermal cycling in ~264 cycles -- completely inadequate.
- Underfill improves fatigue life by 25-100x depending on its effectiveness, making it mandatory for flip-chip reliability.
- The Coffin-Manson power law means small reductions in strain yield large improvements in life.
- The DNP model is a quick estimation tool; FEA is needed for accurate strain calculation.
