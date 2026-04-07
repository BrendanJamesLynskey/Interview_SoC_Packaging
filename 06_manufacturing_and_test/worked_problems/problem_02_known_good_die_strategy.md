# Worked Problem 02: Known Good Die Strategy

## Problem Statement

A multi-chiplet AI accelerator integrates 4 compute die ($300 each), 6 HBM3 stacks ($150 each), 1 I/O die ($80), on a silicon interposer ($200). Assembly cost is $250. If a defective die is discovered only at final package test, the total scrap cost per failure is significant.

Calculate the economic justification for comprehensive KGD testing at the wafer level, given that enhanced KGD testing costs $5 per die but improves escape rate from 500 DPPM to 50 DPPM.

---

## Worked Solution

### Step 1: Calculate Total Assembly Value

```
Compute die: 4 * $300 = $1,200
HBM3 stacks: 6 * $150 = $900
I/O die: 1 * $80 = $80
Interposer: $200
Assembly: $250

Total value per package: $2,630
```

### Step 2: Calculate Scrap Cost at Standard KGD (500 DPPM)

With 11 die per package, each at 500 DPPM escape rate, the probability of at least one defective die:

```
P(all good) = (1 - 500e-6)^11 = (0.9995)^11 = 0.9945
P(at least one bad) = 1 - 0.9945 = 0.0055 = 0.55%
```

For production of 100,000 packages/year:
```
Failed packages = 100,000 * 0.0055 = 550 packages
Scrap cost = 550 * $2,630 = $1,446,500/year
```

### Step 3: Calculate Scrap Cost at Enhanced KGD (50 DPPM)

```
P(all good) = (1 - 50e-6)^11 = (0.99995)^11 = 0.99945
P(at least one bad) = 0.00055 = 0.055%
Failed packages = 100,000 * 0.00055 = 55 packages
Scrap cost = 55 * $2,630 = $144,650/year
```

### Step 4: Calculate KGD Testing Investment

Enhanced KGD testing costs $5 per die. Total die tested per year:
```
Die tested = 100,000 packages * 11 die/package = 1,100,000 die
Testing cost = 1,100,000 * $5 = $5,500,000/year
```

But we must subtract the baseline test cost (standard KGD is already performed):
```
Baseline test cost: $2/die * 1,100,000 = $2,200,000/year
Incremental test cost: $5,500,000 - $2,200,000 = $3,300,000/year
```

### Step 5: Net Benefit Calculation

```
Scrap cost reduction = $1,446,500 - $144,650 = $1,301,850/year
Incremental test cost = $3,300,000/year

Net cost of enhanced KGD = $3,300,000 - $1,301,850 = $1,998,150/year ADDITIONAL COST
```

At this volume, enhanced KGD testing costs more than the scrap savings.

### Step 6: Break-Even Analysis

At what package value does enhanced KGD break even?

```
Scrap savings needed = $3,300,000
Scrap reduction per package = ($2,630 * 0.0055) - ($2,630 * 0.00055) = $14.47 - $1.45 = $13.02
Packages needed = $3,300,000 / $13.02 = 253,456 packages
```

Or, at what package value does it break even at 100K volume?

```
Per-package savings needed = $3,300,000 / 100,000 = $33
$33 = scrap_cost_reduction_per_package = Value * (0.0055 - 0.00055)
Value = $33 / 0.00495 = $6,667
```

Enhanced KGD breaks even when the package value exceeds $6,667 per unit.

### Step 7: Consider Rework Instead of Scrap

If failed packages can be reworked (defective die removed and replaced) at a cost of $500 per rework:

```
Standard KGD: 550 reworks * $500 = $275,000/year
Enhanced KGD: 55 reworks * $500 = $27,500/year
Rework savings = $247,500/year
```

With rework, the scrap cost is much lower, making enhanced KGD even harder to justify economically. However, rework success rate is typically only 70-90%, and reworked units have lower reliability.

### Step 8: Summary

| Scenario | KGD Cost/yr | Scrap Cost/yr | Total/yr |
|---|---|---|---|
| Standard KGD (500 DPPM) | $2,200,000 | $1,446,500 | $3,646,500 |
| Enhanced KGD (50 DPPM) | $5,500,000 | $144,650 | $5,644,650 |
| Standard KGD + Rework | $2,200,000 | $275,000 | $2,475,000 |

**At 100K volume and $2,630 package value, standard KGD with rework is the most economical.**

**At higher package values ($6,667+) or higher volumes (253K+), enhanced KGD becomes justified.**

---

## Key Takeaways

- KGD testing is an economic optimization, not a binary decision.
- The value of KGD testing scales with package value and die count per package.
- For the most expensive packages (AI accelerators at $5,000+), enhanced KGD is clearly justified.
- Rework capability reduces the economic penalty of KGD escapes but adds process complexity.
- Always perform the cost-benefit calculation with actual production volumes and package costs.
