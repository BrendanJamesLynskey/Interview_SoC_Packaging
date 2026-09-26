# Worked Problem 01: Assembly Flow Optimization

## Problem Statement

A 2.5D package assembly requires bonding 2 compute chiplets and 4 HBM3 stacks onto a silicon interposer, which is then bonded to an organic substrate. The current assembly flow has a cycle time of 45 minutes per unit. The target production rate is 500 units per day. Identify bottlenecks and optimize the assembly flow.

---

## Worked Solution

### Step 1: Map the Current Assembly Flow

| Step | Process | Time per Unit | Equipment |
|---|---|---|---|
| 1 | Interposer wafer preparation | 0 (batch, amortized) | -- |
| 2 | Chiplet 1 TCB bond | 8 sec | TCB bonder |
| 3 | Chiplet 2 TCB bond | 8 sec | TCB bonder |
| 4 | HBM stack 1 TCB bond | 12 sec | TCB bonder |
| 5 | HBM stack 2 TCB bond | 12 sec | TCB bonder |
| 6 | HBM stack 3 TCB bond | 12 sec | TCB bonder |
| 7 | HBM stack 4 TCB bond | 12 sec | TCB bonder |
| 8 | Underfill dispense (6 die) | 120 sec | Dispense tool |
| 9 | Underfill cure | 30 min (1800 sec) | Batch oven |
| 10 | Interposer backgrind | Batch (amortized ~60 sec) | Grinder |
| 11 | C4 bump formation | Batch (amortized ~30 sec) | Reflow/plating |
| 12 | Interposer singulation | 30 sec | Dicing saw |
| 13 | Interposer-to-substrate FC bond | 15 sec | FC bonder |
| 14 | Mass reflow | 5 min (300 sec) | Reflow oven |
| 15 | Substrate underfill | 90 sec | Dispense tool |
| 16 | Substrate underfill cure | 30 min (1800 sec) | Batch oven |
| 17 | Lid attach | 60 sec | Lid attach tool |
| 18 | Ball attach + reflow | Batch (amortized ~60 sec) | Ball attach line |
| 19 | Singulation | 30 sec | Saw |
| 20 | Final inspection | 120 sec | AOI + X-ray |

Total serial time: the table sums to ~4,580 sec, about 76 minutes (dominated by the two underfill cure steps at 30 min each; the two cures alone exceed the 45 minutes quoted in the problem statement).

### Step 2: Identify the Bottleneck

The two underfill cure steps dominate the cycle time:
- Die-on-interposer UF cure: 30 min
- Interposer-on-substrate UF cure: 30 min

However, curing is a batch process: a large oven can cure 50-100 units simultaneously. So the effective time per unit is:
```
Cure time per unit = 30 min / batch_size
For batch of 50: 36 seconds per unit
```

The actual bottleneck shifts to the sequential TCB bonding steps:
```
Total TCB time = 8 + 8 + 12 + 12 + 12 + 12 = 64 seconds per unit
```

With a single TCB bonder, throughput is limited to:
```
Units/day = (24 * 3600) / 64 = 1,350 (theoretical max, 3 shifts)
With 80% utilization: 1,080 units/day per bonder
```

For 500 units/day, one TCB bonder is sufficient with good margin.

The next bottleneck is underfill dispensing:
```
Total dispense time = 120 + 90 = 210 seconds per unit
```

One dispense tool gives:
```
Units/day = 86400 * 0.8 / 210 = 329 units/day -- INSUFFICIENT
```

### Step 3: Optimization

**Solution 1: Parallel dispense tools**

Add a second underfill dispense tool to achieve:
```
Units/day = 329 * 2 = 658 -- sufficient
```

**Solution 2: Switch to pre-applied underfill (NCP or NCF)**

Use non-conductive paste (NCP) applied during TCB bonding, eliminating the separate dispense and cure steps. This changes the TCB bond time (adding NCP dispense before each die):
```
TCB + NCP per die: 15-20 sec (vs 8-12 sec for bare TCB)
Total TCB+NCP: 6 * 18 = 108 sec per unit
```

But eliminates 120 sec dispense + 1800 sec cure for die-on-interposer.

**Solution 3: Snap-cure underfill**

Replace 30-minute cure underfill with snap-cure formulation (5-minute cure):
```
Batch cure: 5 min / 50 units = 6 sec per unit
```

### Step 4: Optimized Flow

| Step | Process | Time per Unit |
|---|---|---|
| 1-7 | TCB bond (6 die with NCP) | 108 sec |
| 8 | NCP cure (batch, 50 units) | 6 sec effective |
| 9-11 | Backgrind, C4 bump, singulation | 120 sec (batch, amortized) |
| 12 | Interposer-to-substrate bond | 15 sec |
| 13 | Mass reflow | 6 sec effective (batch) |
| 14 | Substrate underfill (snap-cure) | 90 sec + 6 sec cure |
| 15-18 | Lid, ball, singulation, inspect | 270 sec (batch, amortized) |
| **Total effective serial time** | | **~10 min** |

### Step 5: Equipment Requirements for 500 Units/Day

```
TCB bonder: 108 sec/unit * 500 / (0.8 * 86400) = 0.78 -> 1 bonder
Dispense tool: 90 sec/unit * 500 / (0.8 * 86400) = 0.65 -> 1 tool
Cure oven: Batch 50, 5 min cycle -> 6 cycles/hr * 50 = 300/hr -> 1 oven
Reflow oven: Batch, continuous -> 1 oven
```

---

## Key Takeaways

- Batch processes (oven cure, reflow) rarely limit throughput due to parallel processing.
- Sequential single-unit processes (TCB bonding, dispensing) are the actual throughput bottlenecks.
- Pre-applied underfill (NCP/NCF) eliminates a major process step but increases TCB bond time.
- Snap-cure underfill reduces oven time from 30 minutes to 5 minutes, greatly improving flow.
