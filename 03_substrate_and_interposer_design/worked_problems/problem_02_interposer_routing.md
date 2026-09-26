# Worked Problem 02: Interposer Routing

## Problem Statement

A silicon interposer must route 2048 parallel die-to-die signal connections between a GPU die and an HBM3 stack. The GPU die edge facing the HBM is 20 mm long. The HBM stack interface edge is 10 mm long. The die-to-die spacing (edge to edge) is 3 mm.

The interposer has 4 RDL layers with 0.8/0.8 um line/space and 2 um dielectric thickness. Microbump pitch is 36 um on both die.

Determine:
1. Whether 4 RDL layers provide sufficient routing capacity
2. The trace length and estimated signal propagation delay
3. Insertion loss at the HBM3 data rate of 9.6 Gbps

---

## Worked Solution

### Step 1: Routing Density Analysis

**Available routing channels between microbumps:**

At 36 um microbump pitch with approximately 20 um bump pad diameter, the routing channel between adjacent bumps is:
```
Channel width = pitch - pad_diameter = 36 - 20 = 16 um
```

At 0.8/0.8 um L/S, traces per channel:
```
Traces = floor(channel_width / (line + space)) = floor(16 / 1.6) = 10
```

Practically, we leave margin for manufacturing tolerance. Use 7 traces per channel.

**Routing capacity per layer along the GPU die edge (20 mm):**
```
Bump rows along 20 mm at 36 um pitch: 20,000 / 36 = 555 bump positions
Channels between bumps: 554
Traces per channel: 7
Traces per layer along die edge: 554 * 7 = 3,878
```

**Total routing capacity (4 layers):**
```
Total = 4 * 3,878 = 15,512 traces
```

We need 2048 signals. With differential signaling (2 traces per signal) plus ground shields (1 ground per differential pair):
```
Traces needed = 2048 * 3 = 6,144 (signal-ground-signal)
```

**Capacity check: 15,512 available > 6,144 needed.** Sufficient margin.

However, we must also check the HBM side, which is only 10 mm:
```
Bump rows along 10 mm: 10,000 / 36 = 277
Channels: 276
Traces per layer: 276 * 7 = 1,932
Total 4 layers: 7,728 traces
7,728 > 6,144: still sufficient
```

### Step 2: Routing Topology

The 2048 signals must fan out from the 10 mm HBM edge to the 20 mm GPU edge over a 3 mm span. This requires lateral spreading:

```
Fan-out ratio = GPU_edge / HBM_edge = 20 / 10 = 2x
Lateral spread per trace (maximum, at edge): (20 - 10) / 2 = 5 mm per side
```

The traces from the center of the HBM interface route straight across (3 mm). Traces from the edges must route diagonally to reach their corresponding GPU bumps.

Maximum trace length (corner-to-corner routing):
```
L_max = sqrt(3^2 + 5^2) = sqrt(9 + 25) = sqrt(34) = 5.83 mm
```

Average trace length (most signals):
```
L_avg ~ 3.5 - 4.0 mm
```

### Step 3: Signal Propagation Delay

For a trace on SiO2 dielectric (Dk = 3.9):
```
Velocity = c / sqrt(Dk_eff)
Dk_eff for microstrip ~ 0.5 * (Dk + 1) = 0.5 * (3.9 + 1) = 2.45
Velocity = 3e8 / sqrt(2.45) = 3e8 / 1.565 = 1.917e8 m/s
```

Propagation delay per mm:
```
Delay = 1 / velocity = 1 / (1.917e8 * 1e-3) = 5.22 ps/mm
```

For average trace length of 4 mm:
```
Total delay = 4 * 5.22 = 20.9 ps
```

For maximum trace length of 5.83 mm:
```
Total delay = 5.83 * 5.22 = 30.4 ps
```

HBM3 at 9.6 Gbps has a unit interval (UI) of:
```
UI = 1 / 9.6e9 = 104.2 ps
```

The maximum trace delay (30.4 ps) is 29% of the UI. This is within the timing budget, but length matching between traces is important to minimize skew within a byte lane.

**Skew between shortest and longest trace:**
```
Skew = (5.83 - 3.0) * 5.22 = 14.8 ps
```

This must be compensated through serpentine routing (adding length to shorter traces).

### Step 4: Insertion Loss Estimation

At 9.6 Gbps NRZ, the Nyquist frequency is 4.8 GHz.

**Conductor loss (microstrip, 0.8 um wide trace, 2 um thick Cu):**

Skin depth at 4.8 GHz:
```
delta = sqrt(rho / (pi * f * mu_0)) = sqrt(1.7e-8 / (pi * 4.8e9 * 4*pi*1e-7))
delta = sqrt(1.7e-8 / 1.895e4) = sqrt(8.97e-13) = 0.95 um
```

The skin depth (0.95 um) is comparable to the trace dimensions (0.8 um width, 2 um height), so current flows in nearly the full cross-section.

Approximate conductor resistance:
```
R_per_length ~ rho / (w * t_eff) where t_eff ~ min(t, 2*delta) ~ 1.9 um
R_per_length = 1.7e-8 / (0.8e-6 * 1.9e-6) = 11,200 ohm/m = 11.2 ohm/mm
```

This is far from a low-loss line. A 0.8 um line over 2 um of SiO2 has Z0 ≈ 57 ohm (microstrip formula) and series inductive reactance of only about 9 ohm/mm at 4.8 GHz, so R > wL: the line is in the RC regime and the low-loss form alpha_c = R / (2 * Z0) (0.85 dB/mm here) is not accurate. Solving the full propagation constant gamma = sqrt((R + jwL) * jwC) with C ≈ 0.09 pF/mm gives:
```
alpha ≈ 0.086 Np/mm = 0.75 dB/mm
```

**Dielectric loss (SiO2, Df < 0.001):**
```
alpha_d = pi * f * sqrt(Dk_eff) * Df / c
alpha_d = pi * 4.8e9 * sqrt(2.45) * 0.001 / 3e8
alpha_d = pi * 4.8e9 * 1.565 * 0.001 / 3e8 = 0.0000789 Np/mm = 0.000685 dB/mm
```

**Total insertion loss at 4.8 GHz for 4 mm average trace:**
```
IL_conductor = 0.75 * 4 = 3.0 dB   (4.4 dB for the 5.83 mm longest trace)
IL_dielectric = 0.000685 * 4 = 0.0027 dB
IL_total_trace = 3.0 dB
```

**Bump and via losses (estimated):**
```
2 microbumps + 2 RDL vias: ~0.05 dB
```

**Total insertion loss: approximately 3 dB**

The dielectric loss is negligible, but the conductor loss of 0.8 um-wide RDL is not: fine-line interposer wiring behaves as an RC line, and it is the conductor resistance that limits reach and data rate on the interposer.

### Step 5: Summary

| Parameter | Value | Margin |
|---|---|---|
| Routing capacity (4 layers) | 7,728 traces | 1.26x needed (6,144) |
| Average trace length | 4.0 mm | -- |
| Propagation delay (avg) | 20.9 ps | 80 ps margin in 104 ps UI |
| Trace-to-trace skew (max) | 14.8 ps | Must be length-matched to < 5 ps |
| Insertion loss at 4.8 GHz | ~3 dB (4 mm) | Conductor (RC) loss dominates |

---

## Key Takeaways

- Silicon interposer routing capacity is very high -- 4 layers at 0.8/0.8 L/S provide far more capacity than needed for most HBM interfaces.
- The SiO2 dielectric loss is negligible, but the fine-line RDL conductor loss is not (~0.75 dB/mm at 4.8 GHz for 0.8 um x 2 um lines).
- Trace length matching (serpentine routing) is more important than absolute trace length for timing.
- The fan-out from a smaller die (HBM) to a larger die (GPU) requires diagonal routing that increases maximum trace length.
