# Worked Problem 03: Package Thermal Estimation

## Problem Statement

A networking SoC dissipates 25 W and is housed in a 35 mm x 35 mm FCBGA package with a copper lid. The package will be used in a telecom line card with forced airflow (2 m/s) and an ambient temperature of 55 degrees Celsius. The maximum allowed junction temperature is 105 degrees Celsius.

Determine:
1. The required total junction-to-ambient thermal resistance (theta-JA)
2. Whether a standard FCBGA with lid meets the requirement
3. The heat sink specification if an external heat sink is needed

Given parameters:
- Die size: 15 mm x 15 mm
- TIM1 (die to lid): indium-based, thermal conductivity k = 60 W/m-K, bond line thickness (BLT) = 25 um
- Copper lid: k = 390 W/m-K, thickness = 2 mm, footprint = 30 mm x 30 mm
- TIM2 (lid to heat sink): thermal grease, k = 5 W/m-K, BLT = 50 um
- Package substrate: theta-JB (junction to board) = 8 degrees C/W

---

## Worked Solution

### Step 1: Determine Required Theta-JA

```
theta-JA_required = (T_junction_max - T_ambient) / Power
theta-JA_required = (105 - 55) / 25 = 2.0 degrees C/W
```

The total thermal resistance from junction to ambient must be 2.0 degrees C/W or less.

### Step 2: Calculate the Lid-Path Thermal Resistance (Junction to Lid Top)

The primary heat flow path is: junction -> die backside -> TIM1 -> lid bottom -> lid top.

**Die thermal resistance (silicon, backside):**
```
theta_die = t_die / (k_Si * A_die)
theta_die = 0.75e-3 / (150 * 225e-6)    [assuming 0.75 mm thick die, 15x15 mm area]
theta_die = 0.75e-3 / 0.0338 = 0.022 degrees C/W
```

**TIM1 thermal resistance:**
```
theta_TIM1 = BLT / (k_TIM1 * A_die)
theta_TIM1 = 25e-6 / (60 * 225e-6)
theta_TIM1 = 25e-6 / 0.0135 = 0.0019 degrees C/W
```

Indium TIM is excellent -- nearly negligible thermal resistance.

**Lid thermal resistance (conduction through lid):**
```
theta_lid = t_lid / (k_Cu * A_lid)
theta_lid = 2e-3 / (390 * 900e-6)
theta_lid = 2e-3 / 0.351 = 0.0057 degrees C/W
```

**Total junction to lid-top resistance:**
```
theta_JC = theta_die + theta_TIM1 + theta_lid
theta_JC = 0.022 + 0.0019 + 0.0057 = 0.030 degrees C/W
```

### Step 3: Determine Lid-to-Ambient Requirement

```
theta_CA_required = theta_JA_required - theta_JC
theta_CA_required = 2.0 - 0.030 = 1.97 degrees C/W
```

The lid-top to ambient path must have thermal resistance of 1.97 degrees C/W or less.

### Step 4: Check Natural Convection from Lid (No Heat Sink)

For a 30 mm x 30 mm exposed lid in still air:
```
theta_lid_to_air = 1 / (h * A_lid)
```

Natural convection coefficient for a small horizontal plate: h ~ 10-15 W/m2-K.

```
theta_lid_to_air = 1 / (12 * 900e-6) = 92.6 degrees C/W
```

This is far too high. An external heat sink is mandatory.

### Step 5: Specify the Heat Sink

With TIM2 between lid and heat sink:
```
theta_TIM2 = BLT / (k_TIM2 * A_contact)
theta_TIM2 = 50e-6 / (5 * 900e-6) = 0.011 degrees C/W
```

Required heat sink thermal resistance:
```
theta_HS = theta_CA_required - theta_TIM2
theta_HS = 1.97 - 0.011 = 1.96 degrees C/W
```

For forced airflow at 2 m/s over a heat sink with base 40 mm x 40 mm:

Using a typical aluminum extruded heat sink with fins, the forced convection coefficient is approximately h = 40-60 W/m2-K for 2 m/s airflow. A finned heat sink provides an effective area of 5-10 times the base area.

Required effective heat sink area:
```
A_eff = 1 / (h * theta_HS) = 1 / (50 * 1.96) = 10.2e-3 m^2 = 102 cm^2
```

A heat sink with a 40 mm x 40 mm base and 10 fins (each 40 mm x 20 mm x 1 mm thick, with 3 mm spacing) has:
```
Fin area = 2 * 10 * (40e-3 * 20e-3) = 16,000 mm^2
Base exposed area = 40 * 40 - 10 * 40 * 1 = 1200 mm^2
Total effective area = 16,000 + 1,200 = 17,200 mm^2 = 172 cm^2
```

Accounting for fin efficiency (approximately 85% for thin aluminum fins):
```
Effective area = 0.85 * 16,000 + 1,200 = 14,800 mm^2 = 148 cm^2
theta_HS_actual = 1 / (50 * 148e-4) = 1.35 degrees C/W
```

### Step 6: Verify Total Thermal Resistance

```
theta_JA_total = theta_JC + theta_TIM2 + theta_HS_actual
theta_JA_total = 0.030 + 0.011 + 1.35 = 1.39 degrees C/W
```

Junction temperature:
```
T_J = T_ambient + Power * theta_JA_total
T_J = 55 + 25 * 1.39 = 89.8 degrees C
```

This provides 15.2 degrees C margin below the 105 degrees C limit.

### Step 7: Summary

| Component | Thermal Resistance (C/W) | Percentage of Total |
|---|---|---|
| Die (silicon) | 0.022 | 1.6% |
| TIM1 (indium) | 0.002 | 0.1% |
| Lid (copper) | 0.006 | 0.4% |
| TIM2 (grease) | 0.011 | 0.8% |
| Heat sink (forced air) | 1.350 | 97.1% |
| **Total theta-JA** | **1.391** | **100%** |

The heat sink dominates the thermal resistance chain, as is typical for air-cooled systems.

**Heat sink specification:**
- Base: 40 mm x 40 mm x 3 mm aluminum
- Fins: 10 fins, 20 mm tall, 1 mm thick, 3 mm spacing
- Material: aluminum 6063 (k = 200 W/m-K)
- Attachment: spring clips or push pins
- Required airflow: minimum 2 m/s across fins

---

## Key Takeaways

- The heat sink is almost always the bottleneck in air-cooled thermal solutions (>90% of total thermal resistance).
- Indium TIM provides dramatically better performance than polymer TIM (60 vs 5 W/m-K), but costs significantly more.
- Always verify the junction temperature with worst-case ambient, not typical ambient.
- A quick sanity check: 25 W with theta-JA of 2 degrees C/W gives 50 degrees C rise, which with 55 degrees C ambient reaches 105 degrees C -- right at the limit with no margin.
