# Worked Problem 01: Package Selection Tradeoffs

## Problem Statement

You are the packaging engineer for a new IoT edge-computing SoC with the following requirements:

- 256 signal I/Os plus 64 power/ground connections (320 total)
- Maximum power dissipation: 3.5 W
- Die size: 8 mm x 8 mm
- Maximum package footprint on PCB: 15 mm x 15 mm
- Target board-level reliability: 1000 cycles, -40 to +85 degrees Celsius
- High-speed interfaces: one PCIe Gen4 x4 lane, DDR4-3200
- Cost target: package cost under $1.50 in volume (100K+ units/year)
- Operating environment: industrial (-40 to +85 degrees Celsius)

Evaluate three candidate packages: QFN, wire-bond BGA, and flip-chip CSP. Recommend the best option with justification.

---

## Worked Solution

### Step 1: Evaluate I/O Capacity

| Package Type | Max I/O at Constraint | Feasible? |
|---|---|---|
| QFN (12x12 mm, 0.4 mm pitch) | ~120 peripheral pads | No -- insufficient for 320 I/O |
| QFN (12x12 mm, multi-row 0.5 mm) | ~180 pads with dual row | No -- still insufficient |
| Wire-bond BGA (14x14 mm, 0.8 mm pitch) | ~256 balls (area array) | Marginal -- tight but possible with careful pin assignment |
| Wire-bond BGA (14x14 mm, 0.65 mm pitch) | ~400 balls | Yes |
| Flip-chip CSP (9.5x9.5 mm, 0.5 mm pitch) | ~324 balls (18x18 array) | Yes -- meets requirement within 1.2x die size |

The QFN is eliminated due to insufficient I/O count. Both wire-bond BGA and flip-chip CSP remain.

### Step 2: Evaluate Electrical Performance

PCIe Gen4 operates at 16 GT/s (8 GHz fundamental frequency) and DDR4-3200 has a 1.6 GHz clock. These high-speed interfaces require low parasitic inductance.

- **Wire-bond BGA**: Bond wire inductance of 0.5-1.5 nH creates significant impedance discontinuity at 8 GHz. PCIe Gen4 specification requires channel loss below 8 dB at Nyquist. Wire bond parasitics would consume much of the loss budget. Achieving DDR4-3200 with wire bonds is feasible but requires careful loop optimization.
- **Flip-chip CSP**: Bump inductance of 10-50 pH is negligible at these frequencies. Flip-chip comfortably supports PCIe Gen4 and DDR4-3200 with margin.

Flip-chip CSP is strongly preferred for electrical performance.

### Step 3: Evaluate Thermal Performance

At 3.5 W dissipation over 64 mm-squared die area, the power density is 5.5 W/cm-squared, which is moderate.

- **Wire-bond BGA**: Junction-to-ambient thermal resistance (theta-JA) for a 14 mm BGA is typically 25-35 degrees C/W. At 3.5 W: temperature rise = 3.5 x 30 = 105 degrees C. With 85 degrees C ambient, junction temperature would be 190 degrees C, which exceeds safe limits. An exposed die pad or thermal vias would be needed to bring theta-JA below 20 degrees C/W.
- **Flip-chip CSP**: The die backside is exposed, enabling direct heat spreading. Theta-JA is typically 20-28 degrees C/W. At 3.5 W: temperature rise = 3.5 x 24 = 84 degrees C. Junction temperature = 169 degrees C. Still high -- a small PCB heat sink or thermal pad to the board is needed.

Both packages require thermal enhancements at 3.5 W in an 85 degree C ambient. The flip-chip CSP has a modest advantage.

### Step 4: Evaluate Cost

- **Wire-bond BGA (14x14 mm, 0.65 mm pitch)**: Substrate cost approximately $0.40-0.60. Wire bonding (320 wires at ~$0.002/wire) approximately $0.64. Assembly (die attach, bonding, mold, marking, singulation) approximately $0.30. Total estimated: $1.30-1.50.
- **Flip-chip CSP (9.5x9.5 mm)**: Wafer-level bumping cost approximately $0.15-0.25 per die. Small substrate cost approximately $0.20-0.35. Underfill approximately $0.05. Assembly approximately $0.25. Total estimated: $0.65-0.90.

Flip-chip CSP is less expensive despite the bumping cost, primarily because the substrate is much smaller.

### Step 5: Evaluate Reliability

For board-level reliability at 1000 cycles (-40/+85 degrees C):

- **Wire-bond BGA (14x14 mm, 0.8 mm balls)**: BGA solder joints at this size and pitch are well proven for 1000+ cycles. DNP (distance from neutral point) for corner balls is approximately 10 mm, giving moderate strain.
- **Flip-chip CSP (9.5x9.5 mm, 0.5 mm pitch)**: Smaller balls at finer pitch have less strain capacity. DNP for corner balls is approximately 6.7 mm (smaller, which helps). With proper underfill and optimized pad design, 1000 cycles at -40/+85 is achievable but requires careful design and material selection.

Both packages can meet the reliability target with proper design.

### Step 6: Recommendation

**Recommended package: Flip-chip CSP (9.5 mm x 9.5 mm, 0.5 mm ball pitch)**

| Criterion | Wire-bond BGA | Flip-chip CSP | Winner |
|---|---|---|---|
| I/O count | Marginal | Comfortable | fcCSP |
| Electrical performance | Challenging for PCIe Gen4 | Excellent | fcCSP |
| Thermal | Needs enhancement | Modest advantage | fcCSP |
| Cost | $1.30-1.50 | $0.65-0.90 | fcCSP |
| Board reliability | Proven | Achievable | Tie |
| PCB footprint | 14x14 mm | 9.5x9.5 mm | fcCSP |

The flip-chip CSP meets all requirements with margin, costs less, occupies less board area, and provides superior electrical performance for the high-speed interfaces.

---

## Key Takeaways

- Always start package selection by checking I/O count feasibility -- it eliminates options quickly.
- High-speed interface requirements often drive the choice toward flip-chip over wire bond.
- Smaller packages are not necessarily more expensive; substrate cost dominates, and smaller substrates cost less.
- Thermal analysis must account for worst-case ambient temperature and airflow conditions.
