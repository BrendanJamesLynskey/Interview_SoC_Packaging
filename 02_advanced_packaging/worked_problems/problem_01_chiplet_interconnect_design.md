# Worked Problem 01: Chiplet Interconnect Design

## Problem Statement

You are designing a die-to-die interconnect for a chiplet-based AI accelerator. Two compute chiplets must communicate with an aggregate bandwidth of 1.6 TB/s. The chiplets are placed side by side on a silicon interposer with 5 mm die-edge-to-die-edge spacing. The available die edge for the interface is 12 mm on each chiplet.

Determine:
1. The number of signal lanes required for parallel and SerDes-based approaches
2. The bump count and area requirements for each approach
3. The power consumption estimate for each approach
4. The recommended architecture

---

## Worked Solution

### Step 1: Define Interface Options

**Option A -- Wide parallel interface (UCIe advanced package style):**
- Signaling rate: 16 Gbps per lane (NRZ, forwarded clock)
- Bump pitch: 25 micrometers (advanced package UCIe)

**Option B -- Narrower SerDes-based interface:**
- Signaling rate: 112 Gbps per lane (PAM4 SerDes)
- Bump pitch: 100 micrometers (standard package)

### Step 2: Calculate Lane Count

**Option A (16 Gbps parallel):**
```
Lanes_required = Bandwidth / Rate_per_lane
Lanes_required = 1.6e12 * 8 / 16e9 = 800 differential pairs
```

Note: 1.6 TB/s = 12.8 Tbps. Each differential lane carries 16 Gbps.
```
Lanes = 12.8e12 / 16e9 = 800 differential pairs
```

With overhead (clock lanes, redundancy, encoding -- assume 20%):
```
Total_lanes = 800 * 1.2 = 960 differential pairs
```

**Option B (112 Gbps SerDes):**
```
Lanes = 12.8e12 / 112e9 = 114.3 -> 128 differential pairs (nearest power of 2)
```

With overhead (FEC encoding ~7%, redundancy):
```
Total_lanes = 128 * 1.07 ~ 137 differential pairs -> round to 144
```

### Step 3: Calculate Bump Count and Interface Area

**Option A (25 um pitch, parallel):**

Each differential pair requires 2 signal bumps. With ground shielding (1 ground per 2 signal bumps) and power bumps:
```
Signal bumps = 960 * 2 = 1,920
Ground bumps = 960 (1 per diff pair for shielding)
Power bumps = 480 (estimated for I/O circuits)
Clock bumps = 120 (forwarded clocks, one per group of 8 lanes)
Total bumps = ~3,480
```

At 25 um pitch in an area-array arrangement:
```
Area = Total_bumps * pitch^2 = 3,480 * (25e-3)^2 = 2.175 mm^2
```

Along 12 mm die edge, this requires:
```
Rows = Area / (12 * 0.025) = 2.175 / 0.3 = 7.25 -> 8 rows of bumps
Interface depth = 8 * 0.025 = 0.2 mm from die edge
```

This easily fits within the available die edge.

**Option B (100 um pitch, SerDes):**
```
Signal bumps = 144 * 2 = 288
Analog supply/ground = 288 (1:1 ratio for SerDes)
Support bumps = 144
Total bumps = ~720
```

At 100 um pitch:
```
Area = 720 * (100e-3)^2 = 7.2 mm^2
Rows along 12 mm edge = 7.2 / (12 * 0.1) = 6 rows
Interface depth = 0.6 mm from die edge
```

Also fits, but requires much more die area for the SerDes circuits themselves.

### Step 4: Estimate Power Consumption

**Option A (parallel, 16 Gbps):**

UCIe advanced package targets approximately 0.5 pJ/bit:
```
Power = Energy_per_bit * Bandwidth
Power = 0.5e-12 * 12.8e12 = 6.4 W (both directions combined)
Per direction: 3.2 W
```

**Option B (SerDes, 112 Gbps):**

112G PAM4 SerDes typically consumes 5-8 pJ/bit for short reach:
```
Power = 6e-12 * 12.8e12 = 76.8 W (both directions)
Per direction: 38.4 W
```

### Step 5: Comparison Table

| Parameter | Option A: Parallel (UCIe) | Option B: SerDes (112G) |
|---|---|---|
| Lanes | 960 diff pairs | 144 diff pairs |
| Bump count | ~3,480 | ~720 |
| Interface area | 2.2 mm^2 | 7.2 mm^2 |
| Power | 6.4 W total | 76.8 W total |
| Energy efficiency | 0.5 pJ/bit | 6 pJ/bit |
| Latency (PHY) | ~1-2 ns | ~10-20 ns (SerDes pipeline) |
| Interposer routing | Fine-pitch RDL required | Standard routing acceptable |
| Die area for PHY | Small (simple TX/RX) | Large (CDR, EQ, FEC) |

### Step 6: Recommendation

**Recommended: Option A -- Wide parallel interface at 25 um pitch on silicon interposer**

Justification:
- **Power**: 6.4 W versus 76.8 W is a decisive advantage. The 70 W savings is larger than many chiplets' total power budget.
- **Latency**: 1-2 ns PHY latency is critical for cache coherency between compute chiplets. 10-20 ns SerDes latency would severely impact system performance.
- **Die area**: Simple parallel I/O circuits occupy far less die area than 144 high-speed SerDes lanes.
- **Cost**: The silicon interposer adds cost, but the savings in power (reduced cooling infrastructure) and die area (smaller chiplets) offset this for a high-value AI accelerator.

The parallel approach requires a silicon interposer or bridge for the fine-pitch routing, adding $30-50 to the package cost. For an AI accelerator with a total BOM of $1000+, this is acceptable.

---

## Key Takeaways

- For short-reach die-to-die links (under 10 mm), parallel interfaces dramatically outperform SerDes in power and latency.
- Bump pitch determines the packaging technology required: 25 um demands a silicon interposer; 100 um works on organic substrates.
- Power consumption is often the deciding factor -- a 12x difference in energy efficiency cannot be overcome by other advantages.
- Always calculate total bump count including ground, power, and overhead -- signal bumps alone understate the requirement.
