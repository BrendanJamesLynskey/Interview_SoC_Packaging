# Worked Problem 03: Heterogeneous Integration

## Problem Statement

You are architecting a 5G mmWave base station SoC package that must integrate the following die:

| Die | Function | Process | Size | Power | I/O to other die |
|---|---|---|---|---|---|
| Die A | Digital baseband | 3 nm CMOS | 12x14 mm | 35 W | 2000 to Die B, 500 to Die C |
| Die B | RF transceiver | 22 nm SiGe BiCMOS | 6x8 mm | 8 W | 2000 to Die A, 300 to Die D |
| Die C | Power management | 65 nm BCD | 4x4 mm | 2 W (self) + delivers 45 W | 500 to Die A |
| Die D | mmWave PA | GaAs pHEMT | 3x5 mm | 15 W | 300 to Die B |

The package must also connect to the PCB with 1800 BGA balls for external I/O, power input, and thermal management.

Determine the optimal packaging approach, considering interconnect technology, thermal management, and cost.

---

## Worked Solution

### Step 1: Assess Packaging Technology Options

The key constraint is that four die from four different process technologies must be integrated. Options:

| Approach | Die-to-die routing | Substrate | Pros | Cons |
|---|---|---|---|---|
| 2.5D silicon interposer | Fine-pitch microbumps | CoWoS-like | Highest bandwidth, proven | Very expensive, GaAs die on Si interposer unusual |
| Multi-die FOWLP | RDL (2-5 um L/S) | Fan-out mold + RDL | No substrate cost, thin | 60 W total power hard to cool, GaAs embedding untested |
| EMIB-style bridges | Fine-pitch bridges between die pairs | Organic substrate + bridges | Moderate cost, good bandwidth | Multiple bridge types needed, complex assembly |
| Multi-chip BGA | Organic substrate traces (8-15 um L/S) | Standard organic laminate | Lowest cost, proven | May not support 2000-lane die-to-die bandwidth |

### Step 2: Evaluate Die-to-Die Bandwidth Requirements

**Die A to Die B: 2000 connections**

This is the critical link (baseband to RF transceiver). At 2 Gbps per pin (typical for die-to-die parallel signaling):
```
Bandwidth = 2000 * 2 Gbps = 4 Tbps = 500 GB/s
```

At 8/8 um L/S on organic substrate with 100 um bump pitch:
- Available routing tracks between bumps: 100/16 = 6 tracks per bump pitch
- Can route 2000 signals in ~40 mm of die edge using ~8 routing layers

This is feasible on an advanced organic substrate (10-12 layer build-up).

**Die B to Die D: 300 connections**

Low density, easily handled by any substrate technology.

### Step 3: Select Packaging Approach

Given that the die-to-die bandwidth can be served by an organic substrate (avoiding interposer cost), and the GaAs die makes silicon interposer integration non-standard, the recommended approach is:

**Multi-chip flip-chip BGA on advanced organic substrate with an EMIB-style bridge between Die A and Die B**

Rationale:
- An EMIB bridge between Die A and Die B provides the fine-pitch routing needed for 2000 connections at low power
- Die C and Die D connect to the substrate via standard flip-chip bumps with substrate routing
- The GaAs Die D can be attached using standard die-attach methods (AuSn eutectic solder)
- The organic substrate handles all connections to the 1800-ball BGA

### Step 4: Thermal Management Design

Total power dissipation: 35 + 8 + 2 + 15 = 60 W

The 35 W digital baseband (Die A) and 15 W GaAs PA (Die D) are the thermal hotspots.

**Die A (35 W, 12x14 mm = 168 mm^2):**
```
Power density = 35 / 1.68 = 20.8 W/cm^2
```

This requires a heat spreader and heat sink. A copper lid with TIM1 (indium) gives:
```
theta_JC = 0.03 + 0.002 + 0.006 = 0.038 C/W (die + TIM1 + lid)
```

**Die D (15 W, 3x5 mm = 15 mm^2):**
```
Power density = 15 / 0.15 = 100 W/cm^2
```

This is very high and localized. GaAs has lower thermal conductivity than silicon (46 vs 150 W/m-K), worsening the problem.

```
theta_die_D = t / (k * A) = 0.1e-3 / (46 * 15e-6) = 0.145 C/W
```

Solution: Die D should be placed near the package edge with a dedicated thermal path. A cutout in the lid with a direct copper heat spreader over Die D, or a separate heat sink, is recommended.

**Overall thermal solution:**
- Copper lid spanning Die A and Die B (largest power sources close together)
- Indium TIM1 for Die A, thermal epoxy for Die B
- Die D positioned at package edge with separate thermal management (exposed die pad to PCB or dedicated heat sink)
- Die C (2 W) requires no special thermal treatment

### Step 5: Package Layout

```
Package size: 50 mm x 50 mm FCBGA, 1800 balls at 1.0 mm pitch

+--------------------------------------------------+
|                                                  |
|     +---------+   Bridge   +--------+            |
|     |  Die A  |============|  Die B |            |
|     | (BB)    |   EMIB     | (RF)   |            |
|     | 12x14   |            | 6x8    |            |
|     +---------+            +--------+            |
|                                                  |
|     +------+                +-------+            |
|     |Die C |                | Die D |            |
|     |(PMIC)|                | (PA)  |            |
|     | 4x4  |                | 3x5   |            |
|     +------+                +-------+            |
|                                                  |
+--------------------------------------------------+
```

### Step 6: Substrate Specification

| Parameter | Specification |
|---|---|
| Size | 50 mm x 50 mm |
| Layer count | 12 build-up layers (6-2-6 configuration) |
| Core | 0.4 mm, ABF build-up |
| Line/space (build-up) | 8/8 um (outer layers), 12/12 um (inner layers) |
| Via type | Laser-drilled microvias (50 um diameter) |
| Bridge cavity | 1 cavity for EMIB bridge (8 x 4 mm) |
| Ball pitch | 1.0 mm, 0.5 mm diameter SAC305 balls |
| Surface finish | ENEPIG |

### Step 7: Cost Estimate

| Component | Estimated Cost |
|---|---|
| Organic substrate (12L, 50x50, bridge cavity) | $25-35 |
| EMIB bridge die | $3-5 |
| Assembly (4 die attach, flip-chip, underfill, lid) | $15-20 |
| Lid + TIM | $5-8 |
| Test | $5-10 |
| **Total package cost** | **$53-78** |

For comparison, a full silicon interposer approach would cost $120-180 (the interposer alone would be $50-80).

### Step 8: Risk Assessment

| Risk | Severity | Mitigation |
|---|---|---|
| GaAs die CTE mismatch (5.7 ppm/K vs organic 14-17) | Medium | Specialized underfill, stress simulation |
| EMIB bridge alignment | Medium | Proven Intel process, tight cavity tolerances |
| Thermal coupling between PA and baseband | High | Physical separation, thermal simulation, isolated lid zones |
| Mixed die assembly yield | Medium | KGD testing of all four die, sequential assembly with test |

---

## Key Takeaways

- Heterogeneous integration does not always require the most advanced packaging; organic substrates with bridges can serve many applications.
- GaAs and other III-V materials introduce CTE and thermal challenges that must be addressed explicitly.
- Power density (W/cm-squared) matters more than total power for thermal design -- a small high-power die can be harder to cool than a large moderate-power die.
- EMIB-style bridges provide a good cost-performance tradeoff for localized high-density interconnect.
