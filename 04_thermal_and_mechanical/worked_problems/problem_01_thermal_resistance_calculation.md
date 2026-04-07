# Worked Problem 01: Thermal Resistance Calculation

## Problem Statement

An AI accelerator dissipates 450 W from a 25 mm x 30 mm die in an FCBGA package with a copper lid. The maximum junction temperature is 95 degrees Celsius, and the data center inlet air temperature is 35 degrees Celsius. Determine whether air cooling is feasible, or if liquid cooling is required.

---

## Worked Solution

### Step 1: Maximum Allowable Theta-JA

```
theta-JA_max = (T_J_max - T_ambient) / Power = (95 - 35) / 450 = 0.133 degrees C/W
```

### Step 2: Calculate Theta-JC (Junction to Case/Lid Top)

| Component | Formula | Value |
|---|---|---|
| Die (silicon, 0.5 mm) | 0.5e-3 / (150 * 750e-6) | 0.004 C/W |
| TIM1 (indium, 20 um) | 20e-6 / (80 * 750e-6) | 0.0003 C/W |
| Lid (Cu, 3 mm, 45x45 mm) | 3e-3 / (390 * 2025e-6) | 0.004 C/W |
| Spreading resistance | Estimated (die-to-lid area ratio 750/2025) | 0.012 C/W |
| **Total theta-JC** | | **0.020 C/W** |

### Step 3: TIM2 Resistance

Using high-performance thermal grease (k = 8 W/m-K, BLT = 30 um):
```
theta-TIM2 = 30e-6 / (8 * 2025e-6) = 0.0019 C/W ~ 0.002 C/W
```

### Step 4: Required Heat Sink Resistance

```
theta-HS_max = theta-JA_max - theta-JC - theta-TIM2
theta-HS_max = 0.133 - 0.020 - 0.002 = 0.111 C/W
```

### Step 5: Evaluate Air Cooling Feasibility

Best-case high-performance air heat sink (copper base, heat pipe, 100x100x60 mm, 3 m/s airflow):
```
Typical theta-HS = 0.15 - 0.20 C/W
```

This exceeds the 0.111 C/W requirement. **Air cooling is not feasible at 450 W.**

Even the most aggressive air cooling (80x80x80 mm tower cooler with 5+ m/s airflow):
```
theta-HS ~ 0.10-0.12 C/W -- marginally possible, no margin
```

### Step 6: Liquid Cooling Solution

Cold plate with water cooling at 1 L/min flow rate:
```
theta-HS (cold plate) = 0.03 - 0.06 C/W (typical for 45x45 mm contact area)
```

Using theta-HS = 0.05 C/W:
```
T_J = T_ambient + Power * (theta-JC + theta-TIM2 + theta-HS)
T_J = 35 + 450 * (0.020 + 0.002 + 0.05) = 35 + 450 * 0.072 = 35 + 32.4 = 67.4 C
```

This provides 27.6 degrees C margin below the 95 degree C limit -- comfortable.

### Step 7: Summary

| Solution | Theta-JA (C/W) | T_J (C) | Feasible? |
|---|---|---|---|
| Air cooling (best case) | 0.14 | 98 | No (exceeds 95 C) |
| Cold plate (1 L/min) | 0.072 | 67.4 | Yes (27.6 C margin) |
| Cold plate (0.5 L/min) | 0.092 | 76.4 | Yes (18.6 C margin) |

**Liquid cooling is required for 450 W dissipation.**

---

## Key Takeaways

- At 450 W, the total thermal budget is only 0.133 C/W -- extremely tight for air cooling.
- The package thermal resistance (theta-JC) is small (~0.020 C/W) thanks to indium TIM and copper lid.
- The heat sink/cooling solution dominates the thermal budget even with liquid cooling.
- Modern AI accelerators at 400-1000 W universally require liquid cooling in data centers.
