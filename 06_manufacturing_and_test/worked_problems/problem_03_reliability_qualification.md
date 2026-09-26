# Worked Problem 03: Reliability Qualification

## Problem Statement

A new FCBGA package for an automotive ADAS processor must be qualified to AEC-Q100 Grade 1 (-40 to +125 degrees Celsius). The package is 35 mm x 35 mm with 1500 BGA balls at 0.8 mm pitch. Determine the qualification test plan, estimate the acceleration factors, and predict the equivalent field life.

---

## Worked Solution

### Step 1: AEC-Q100 Grade 1 Qualification Matrix

| Test | Condition | Duration/Cycles | Sample Size |
|---|---|---|---|
| Preconditioning | MSL per J-STD-020 + 3x reflow | Before each stress test | All samples |
| Temperature cycling (TC) | -40/+125 C, Condition G | 1000 cycles | 77 units, 0 failures |
| HAST | 130 C, 85% RH, biased | 96 hours | 77 units, 0 failures |
| High-temp storage | 150 C | 1000 hours | 77 units, 0 failures |
| THB | 85 C, 85% RH, biased | 1000 hours | 77 units, 0 failures |
| Power cycling | Delta_T_J = 100 C | 1000 cycles | 77 units, 0 failures |
| Board-level TC | -40/+125 C, on test board | 1000 cycles | 30 boards, 0 failures |
| Board-level drop | JEDEC condition B | 30 drops | 30 boards, 0 failures |

### Step 2: Acceleration Factor for Thermal Cycling

Field conditions (automotive under-hood):
- Temperature range: -20 to +105 C (Delta_T_use = 125 C)
- Cycling frequency: 2 cycles/day (engine on/off)

Test conditions:
- Temperature range: -40 to +125 C (Delta_T_test = 165 C)
- Cycling frequency: ~3 cycles/hour (standard TC oven)

Using modified Coffin-Manson with frequency correction:
```
AF_TC = (Delta_T_test / Delta_T_use)^n * (f_use / f_test)^m
n = 2.5 (SAC305 solder, empirical)
m = 0.33

AF_TC = (165/125)^2.5 * (2/(3*24))^0.33
AF_TC = (1.32)^2.5 * (2/72)^0.33
AF_TC = 2.00 * (0.0278)^0.33
AF_TC = 2.00 * 0.307 = 0.614
```

Wait -- the frequency factor reduces the acceleration because the test cycles faster (less time per cycle for creep). Let's recalculate properly:

Actually, the convention is that slower cycling (more dwell time) causes more creep damage. The use condition (2 cycles/day, long dwell) has more damage per cycle than the test (3 cycles/hr, short dwell). So:

```
AF = (Delta_T_test / Delta_T_use)^n * (t_dwell_test / t_dwell_use)^m
t_dwell_test = 10 min = 600 sec (typical TC oven dwell)
t_dwell_use = 360 min = 21600 sec (6 hours engine on)

AF = (165/125)^2.5 * (600/21600)^0.33
AF = 2.00 * (0.0278)^0.33
AF = 2.00 * 0.307 = 0.614
```

The field condition is actually more damaging per cycle due to longer dwell! This means 1000 test cycles corresponds to:

```
Equivalent field cycles = 1000 / AF = 1000 / 0.614 = 1630 field cycles
```

At 2 field cycles/day: 1630 / (2 * 365) = 2.23 years.

This is insufficient for the 15-year automotive lifetime! More test cycles or design margin is needed.

### Step 3: Revised Qualification Approach

To demonstrate 15-year life (10,950 field cycles at 2/day):
```
Test cycles needed = 10,950 * AF = 10,950 * 0.614 = 6,720 test cycles
```

Options:
1. **Run 6,720 test cycles** (very long, ~93 days at 3 cycles/hr)
2. **Increase test severity** (wider temperature range increases AF)
3. **Design for much higher margin** (ensure the design has characteristic life >> 10,950 cycles)

Using condition B (-55/+125 C, Delta_T = 180 C):
```
AF = (180/125)^2.5 * (600/21600)^0.33 = (1.44)^2.5 * 0.307 = 2.49 * 0.307 = 0.763
Test cycles = 10,950 * 0.763 = 8,350 (still long)
```

The practical approach: demonstrate that the characteristic life (Weibull eta) is much larger than 1000 cycles, providing confidence that the ~6,700 required test-equivalent cycles are well within the wear-out margin.

### Step 4: BGA Solder Joint Fatigue Estimation

For a 35 mm x 35 mm BGA with corner ball DNP:
```
DNP = (35 * sqrt(2)) / 2 = 24.7 mm
Delta_gamma = CTE_mismatch * Delta_T * DNP / h_ball
CTE_mismatch (package to PCB) ~ 3 ppm/K (well-matched)
Delta_T = 165 C (test condition)
h_ball = 0.4 mm (0.8 mm pitch ball)

Delta_gamma = 3e-6 * 165 * 24.7e-3 / 0.4e-3 = 0.031 (3.1%)
```

Coffin-Manson fatigue life:
```
N_f = 40 * (0.031)^(-2.0) = 40 / 9.6e-4 = 41,667 cycles
```

This characteristic life (41,667 cycles) far exceeds the 1000 test cycles and the ~6,700 test-equivalent cycles for 15 years, providing a safety factor of:
```
SF = 41,667 / 6,720 = 6.2x
```

### Step 5: HAST Acceleration Factor

Field conditions: 60 C, 60% RH, 15 years.
Test conditions: 130 C, 85% RH, 96 hours.

Peck model:
```
AF_HAST = (RH_test/RH_use)^n * exp(Ea/k * (1/T_use - 1/T_test))
n = 2.66 (typical for moisture-driven failures)
Ea = 0.7 eV
k = 8.617e-5 eV/K

AF_HAST = (85/60)^2.66 * exp(0.7/8.617e-5 * (1/333 - 1/403))
= (1.417)^2.66 * exp(8123 * (0.003003 - 0.002481))
= 2.53 * exp(8123 * 0.000522)
= 2.53 * exp(4.24)
= 2.53 * 69.2 = 175
```

Equivalent field life:
```
Field hours = 96 * 175 = 16,800 hours = 1.92 years
```

For 15-year life at 60 C/60% RH, need:
```
Test hours = 15 * 8760 / 175 = 751 hours
```

The 96-hour HAST demonstrates only 1.9 years of field life. For automotive, extend HAST to 264 hours (AEC-Q100 specifies this for some grades):
```
Field hours = 264 * 175 = 46,200 hours = 5.3 years
```

Still less than 15 years. The automotive qualification relies on multiple tests covering different failure mechanisms, with the expectation that no single test demonstrates the full 15-year life but the combination of tests and design margin provides adequate confidence.

### Step 6: Summary

| Test | Test Duration | AF | Field Equivalent |
|---|---|---|---|
| TC (-40/+125) | 1000 cycles | 0.614 | 2.2 years |
| HAST (130/85) | 264 hours | 175 | 5.3 years |
| HTS (150 C) | 1000 hours | ~50 | 5.7 years |
| THB (85/85) | 1000 hours | ~25 | 2.9 years |

No single test demonstrates 15-year life, but the package design has a solder fatigue life safety factor of 6.2x, providing confidence for long-term reliability.

---

## Key Takeaways

- Automotive thermal cycling is more damaging per cycle than standard JEDEC tests due to long dwell times.
- Passing 1000 TC cycles does not automatically guarantee 15-year automotive life; acceleration factor analysis is essential.
- Design margin (characteristic life >> required life) is the primary assurance of long-term reliability.
- Multiple tests cover different failure mechanisms; no single test validates the complete product lifetime.
