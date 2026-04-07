# Worked Problem 03: Lid Attach Design

## Problem Statement

Design the lid attachment for a 50 mm x 50 mm FCBGA package with a copper lid. The die is 22 mm x 22 mm, centered on the substrate. The lid must maintain TIM1 bond line thickness of 25 um over the die, withstand 1000 thermal cycles (-40/+125 degrees C), and allow rework (lid removal for die replacement).

Determine lid adhesive requirements, TIM1 selection, and expected stress levels.

---

## Worked Solution

### Step 1: Lid Geometry and CTE Analysis

The lid is attached to the substrate by a perimeter adhesive (lid sealant) and to the die through TIM1.

CTE values:
- Copper lid: 17 ppm/K
- Organic substrate: 16 ppm/K (approximately matched)
- Silicon die: 2.6 ppm/K

The lid-to-substrate CTE mismatch is small (1 ppm/K), so the lid adhesive joint stress is moderate. The lid-to-die CTE mismatch through TIM1 is large (14.4 ppm/K), so TIM1 must be compliant.

### Step 2: TIM1 Selection

The TIM1 must accommodate the CTE mismatch between die and lid. The shear displacement at the TIM1 interface:

```
Delta = CTE_mismatch * Delta_T * half_die_diagonal
Delta = 14.4e-6 * 165 * (22*sqrt(2)/2) * 1e-3
Delta = 14.4e-6 * 165 * 15.56e-3
Delta = 36.9 um
```

This 37 um shear displacement must be absorbed by the TIM1 material without cracking or delaminating.

**Option A: Indium solder TIM1**
- Thermal conductivity: 80 W/m-K
- Shear yield strength: ~5 MPa
- Behavior: Ductile, accommodates strain through plastic deformation
- Shear strain in TIM1: 36.9 / 25 = 1.48 (148%) -- indium can handle this due to its ductility and creep
- Thermal resistance: 25e-6 / (80 * 484e-6) = 0.0006 C/W (excellent)

**Option B: Polymer TIM1 (thermal grease)**
- Thermal conductivity: 5 W/m-K
- Behavior: Viscous, flows to accommodate displacement
- No shear stress concern (liquid/paste)
- Thermal resistance: 25e-6 / (5 * 484e-6) = 0.010 C/W
- Risk: Pump-out (grease squeezed out of the gap over thermal cycling)

**Selection: Indium solder TIM1** for best thermal performance and long-term stability.

### Step 3: Lid Adhesive Design

The lid adhesive bonds the lid perimeter to the substrate. Requirements:
- Compliant enough to absorb lid-substrate CTE mismatch stress
- Strong enough to hold the lid in place during handling and assembly
- Reworkable (removable at moderate temperature)

Lid adhesive properties:
- Material: Silicone-based adhesive (low modulus, high elongation)
- Modulus: 1-10 MPa (very compliant)
- CTE: 200-300 ppm/K (high, but thin bond line limits the effect)
- Adhesion strength: 1-3 MPa (sufficient for lid retention)
- Rework temperature: 200-250 degrees C (adhesive softens, allowing lid peel-off)

Adhesive bond line geometry:
- Width: 3 mm perimeter band
- Thickness: 100 um (controlled by spacer beads in the adhesive)
- Perimeter length: 4 * 50 = 200 mm

### Step 4: Stress in Lid Adhesive

Shear displacement at lid adhesive (lid corner, DNP from center = 35.4 mm):
```
Delta_adhesive = (CTE_lid - CTE_substrate) * Delta_T * DNP
Delta_adhesive = (17 - 16) * 1e-6 * 165 * 35.4e-3 = 5.8 um
```

Shear strain in adhesive:
```
Gamma = 5.8 / 100 = 0.058 (5.8%)
```

For silicone adhesive with 200%+ elongation capability, this 5.8% strain is trivial. The adhesive will survive thousands of thermal cycles.

### Step 5: TIM1 Bond Line Thickness Control

The 25 um TIM1 BLT is controlled by:
1. Lid standoff features (machined bumps or solder balls on the lid that set the gap)
2. Lid adhesive thickness (which sets the lid height above the substrate)
3. Die thickness tolerance (typically +/- 10 um after backgrinding)

The BLT tolerance budget:
```
BLT = Lid_height - Substrate_thickness - Die_height
BLT_tolerance = sqrt(tol_lid^2 + tol_substrate^2 + tol_die^2)
BLT_tolerance = sqrt(10^2 + 15^2 + 10^2) = sqrt(425) = 20.6 um
```

With nominal BLT of 25 um and tolerance of +/- 20.6 um, the BLT ranges from 4.4 to 45.6 um. This is acceptable for indium TIM but requires careful tolerance stack analysis.

### Step 6: Summary

| Parameter | Specification | Value |
|---|---|---|
| TIM1 material | Indium solder | k = 80 W/m-K |
| TIM1 BLT | 25 um nominal | 4-46 um range |
| TIM1 thermal resistance | | 0.0006 C/W |
| Lid adhesive | Silicone, 3 mm width | Modulus 5 MPa |
| Adhesive shear strain | Max at corner | 5.8% (safe) |
| Lid material | Nickel-plated copper | 3 mm thick |
| Rework method | Heat to 250 C, peel lid | |

---

## Key Takeaways

- The lid-to-substrate CTE mismatch is small (Cu to organic), so the adhesive stress is manageable.
- The die-to-lid CTE mismatch is large, but indium TIM accommodates it through ductile deformation.
- BLT tolerance control requires careful stack-up analysis of all component tolerances.
- Reworkability requires a compliant, thermally degradable adhesive (silicone).
