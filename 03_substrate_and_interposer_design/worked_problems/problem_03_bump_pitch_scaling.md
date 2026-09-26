# Worked Problem 03: Bump Pitch Scaling

## Problem Statement

Analyze the impact of scaling microbump pitch from 55 um to 36 um to 25 um on a die-to-die interface. The interface must provide 1024 data signals plus 512 power/ground connections across a 10 mm die edge. Evaluate the impact on bump count density, solder volume, current density, and intermetallic compound formation.

---

## Worked Solution

### Step 1: Bump Count and Area Analysis

For each pitch, calculate the bump density and required interface area:

| Parameter | 55 um pitch | 36 um pitch | 25 um pitch |
|---|---|---|---|
| Bumps per mm (linear) | 18.2 | 27.8 | 40.0 |
| Bumps per mm^2 (area) | 330 | 772 | 1,600 |
| Total bumps needed | 1,536 | 1,536 | 1,536 |
| Required area | 4.65 mm^2 | 1.99 mm^2 | 0.96 mm^2 |
| Bumps per row along 10 mm edge | 181 | 277 | 400 |
| Depth from die edge | 0.47 mm (8.5 rows) | 0.20 mm (5.5 rows) | 0.10 mm (3.8 rows) |

At 25 um pitch, the interface occupies less than 0.1 mm depth from the die edge -- nearly invisible in the die floor plan.

### Step 2: Bump Geometry and Solder Volume

Typical bump dimensions scale with pitch:

| Parameter | 55 um pitch | 36 um pitch | 25 um pitch |
|---|---|---|---|
| Pad diameter | 30 um | 20 um | 12 um |
| Cu pillar height | 25 um | 15 um | 8 um |
| Solder cap height | 15 um | 8 um | 4 um |
| Solder cap diameter | 30 um | 20 um | 12 um |
| Solder volume | ~10,600 um^3 | ~2,510 um^3 | ~452 um^3 |

Solder volume (approximate cylinder):
```
V_55 = pi * (15)^2 * 15 = 10,603 um^3
V_36 = pi * (10)^2 * 8 = 2,513 um^3
V_25 = pi * (6)^2 * 4 = 452 um^3
```

The solder volume at 25 um pitch is 23 times smaller than at 55 um pitch.

### Step 3: Intermetallic Compound (IMC) Consumption

During reflow and aging, copper and tin react to form Cu6Sn5 and Cu3Sn intermetallic compounds. The IMC growth rate follows:

```
IMC thickness = h0 + sqrt(D * t)
```

Where D is the diffusion coefficient and t is time. Typical IMC growth after reflow is 1-3 um, and after 1000 hours at 150 degrees C aging, total IMC can reach 5-8 um.

**Fraction of solder consumed by IMC (assuming 2 um IMC from each side = 4 um total consumed):**

```
Solder height consumed = 4 um (2 um from die side + 2 um from substrate side)

55 um pitch: 4/15 = 27% of solder height consumed
36 um pitch: 4/8 = 50% of solder height consumed
25 um pitch: 4/4 = 100% of solder height consumed -- entirely IMC
```

At 25 um pitch, the solder joint becomes an all-IMC joint after minimal aging. IMC joints are functional but more brittle (lower fracture toughness) and have different fatigue behavior than solder joints.

### Step 4: Current Density Analysis

Assume each power bump carries equal current. For a die requiring 40 A through 512 power/ground bumps, the current flows in through the 256 VDD bumps and returns through the 256 VSS bumps, so each bump carries:

```
Current per bump = 40 / 256 = 156 mA
```

Current density at the solder (minimum cross-section):

| Pitch | Solder diameter | Area | Current density |
|---|---|---|---|
| 55 um | 30 um | 707 um^2 | 2.2 x 10^4 A/cm^2 |
| 36 um | 20 um | 314 um^2 | 5.0 x 10^4 A/cm^2 |
| 25 um | 12 um | 113 um^2 | 1.4 x 10^5 A/cm^2 |

The electromigration threshold for SnAg solder is typically 1-5 x 10^4 A/cm^2 (depending on temperature and geometry).

At 36 um pitch the current density (5.0 x 10^4 A/cm^2) is at the top of that range, and at 25 um pitch (1.4 x 10^5 A/cm^2) it **far exceeds the electromigration threshold**. Mitigation options:
- Increase the number of power/ground bumps (e.g., 1024 instead of 512)
- Reduce current per bump by distributing power across more bumps
- Use copper-to-copper direct bonding (no solder, no electromigration in copper at these densities)

### Step 5: Assembly Process Implications

| Aspect | 55 um pitch | 36 um pitch | 25 um pitch |
|---|---|---|---|
| Assembly method | Mass reflow or TCB | TCB required | TCB mandatory |
| Placement accuracy | +/- 5 um | +/- 3 um | +/- 1.5 um |
| Underfill gap | ~25 um | ~15 um | ~8 um |
| Underfill flow | Standard capillary | Challenging | Pre-applied film required |
| Process maturity | High volume | Production ramp | Early production |

### Step 6: Summary and Recommendation

| Metric | 55 um | 36 um | 25 um |
|---|---|---|---|
| Bump density (per mm^2) | 330 | 772 | 1,600 |
| Bandwidth density (GB/s/mm) | ~90 | ~140 | ~200 |
| Solder volume per bump (um^3) | 10,600 | 2,510 | 452 |
| IMC fraction after aging | 27% | 50% | 100% |
| EM current density (A/cm^2) | 2.2e4 | 5.0e4 | 1.4e5 |
| EM risk | Moderate | High (at threshold) | Very high |
| Assembly maturity | HVM | Production | Early |

**Recommendation:** For the described interface (1024 signals, 10 mm edge), 36 um pitch provides the best balance: sufficient density (5.5 rows), manageable IMC formation and production-proven assembly — but at 5.0 x 10^4 A/cm^2 it sits at the EM threshold, so it needs more power/ground bumps (or lower current per bump) to give EM margin. The move to 25 um pitch should be reserved for interfaces where the bandwidth density improvement is essential and where the design can accommodate more power/ground bumps to reduce EM risk. Beyond 25 um, hybrid bonding should be considered.

---

## Key Takeaways

- Solder volume scales as pitch cubed, making IMC consumption the dominant concern at fine pitch.
- Electromigration becomes a critical design constraint below 40 um pitch for high-current applications.
- Thermocompression bonding becomes mandatory below ~50 um pitch.
- The transition from solder-based to hybrid bonding is driven by the physical limits of solder at fine pitch.
