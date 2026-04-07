# Worked Problem 02: Decoupling Strategy

## Problem Statement

A server processor operates at 0.85 V with a maximum current of 300 A and a transient current step of 150 A in 200 ps. The voltage tolerance is 3 percent. Design the package-level decoupling capacitor strategy to meet the target impedance from 10 MHz to 2 GHz.

---

## Worked Solution

### Step 1: Calculate Target Impedance

```
Z_target = (Tolerance * VDD) / I_max
Z_target = (0.03 * 0.85) / 300 = 0.085 milliohms
```

This extremely low target impedance must be maintained from DC to several GHz.

### Step 2: Frequency Domain Responsibility

| Frequency Range | Responsible Component |
|---|---|
| DC - 10 kHz | VRM on motherboard |
| 10 kHz - 10 MHz | Board bulk capacitors (10-100 uF) |
| 10 MHz - 500 MHz | **Package decoupling capacitors** |
| 500 MHz - 2 GHz | **Package embedded capacitors + die capacitors** |
| > 2 GHz | On-die decoupling (MOS + MIM caps) |

### Step 3: Estimate Package Loop Inductance

The package power delivery path from BGA ball to die bump has a total loop inductance:
```
L_ball ~ 30 pH (BGA solder ball)
L_substrate ~ 20 pH (via + plane path)
L_bump ~ 15 pH (flip-chip bump pair)
L_total ~ 65 pH
```

The impedance due to this inductance at frequency f:
```
Z_L = 2 * pi * f * L
At 100 MHz: Z_L = 2*pi * 100e6 * 65e-12 = 0.041 milliohms (OK)
At 500 MHz: Z_L = 2*pi * 500e6 * 65e-12 = 0.204 milliohms (exceeds target!)
At 1 GHz: Z_L = 2*pi * 1e9 * 65e-12 = 0.408 milliohms (far exceeds target!)
```

The package inductance causes the impedance to rise above 85 micro-ohms at approximately 210 MHz. Above this frequency, capacitors alone cannot reduce the impedance -- the inductance of the capacitor mounting path limits effectiveness.

### Step 4: Package Surface-Mount Capacitor Design

**Low-frequency package decoupling (10-200 MHz):**

Using 0201 MLCCs (100 nF, ESR = 10 milliohms, ESL = 50 pH):

Self-resonant frequency: f_SR = 1 / (2*pi*sqrt(L*C)) = 1 / (2*pi*sqrt(50e-12 * 100e-9)) = 71 MHz

At resonance, each capacitor presents its ESR (10 milliohms). To achieve Z_target = 0.085 milliohms at 71 MHz:
```
N_caps = ESR / Z_target = 10e-3 / 85e-6 = 118 capacitors
```

This is a large number. Practical considerations:
- Available area on substrate surface for 0201 caps: approximately 120-200 positions on a 50x50 mm substrate
- Use 150 capacitors of 100 nF, distributed across the substrate surface

**Mid-frequency decoupling (100-500 MHz):**

Using smaller MLCC (10 nF, ESR = 15 milliohms, ESL = 40 pH):

f_SR = 1 / (2*pi*sqrt(40e-12 * 10e-9)) = 252 MHz

At 252 MHz, each capacitor presents 15 milliohms:
```
N_caps = 15e-3 / 85e-6 = 176 capacitors (not practical on package surface)
```

This shows that surface-mount capacitors alone cannot meet the target at higher frequencies.

### Step 5: Embedded Capacitor Strategy

Embedded thin-film capacitors provide distributed decoupling with very low inductance (sub-10 pH mounting):

```
Capacitance density: 50 nF/cm^2 (typical high-k thin film)
Available area under die: 20 mm x 20 mm = 4 cm^2
Total embedded capacitance: 4 * 50 = 200 nF
Effective ESL per unit area: 2 pH (embedded, very low)
```

Self-resonant frequency: f_SR = 1 / (2*pi*sqrt(2e-12 * 200e-9)) = 252 MHz

But with only 2 pH ESL, the impedance at 1 GHz:
```
Z = 2*pi * 1e9 * 2e-12 = 0.013 milliohms -- below target!
```

Embedded capacitors with low ESL can maintain sub-target impedance to high frequencies.

### Step 6: Complete Decoupling Strategy

| Component | Quantity | Value | Frequency Coverage |
|---|---|---|---|
| 0402 MLCC (top surface) | 50 | 1 uF | 10-50 MHz |
| 0201 MLCC (top surface) | 100 | 100 nF | 30-200 MHz |
| 0201 MLCC (bottom/land side) | 100 | 100 nF | 30-200 MHz |
| Embedded thin-film cap | 4 cm^2 | 200 nF total | 100 MHz - 1 GHz |
| On-die MOS + MIM caps | N/A | ~100 nF | 500 MHz - 10 GHz |

### Step 7: Verify Ldi/dt Droop

With the complete PDN (package inductance partially bypassed by on-package capacitors):

Effective inductance seen by die for a 150 A step in 200 ps:
```
For the fastest transients (200 ps), only capacitors with ESL < ~50 pH respond in time.
Effective inductance with embedded caps: ~5-10 pH
V_droop = L_eff * di/dt = 10e-12 * (150/200e-12) = 7.5 mV
```

As percentage of VDD: 7.5 / 850 = 0.88%. This is well within the 3% budget.

For slower transients (5 ns step): the MLCC capacitors also contribute, and the droop is dominated by the charge delivered by capacitors:
```
Q = C * V = I * t
V_droop = I * t / C_total = 150 * 5e-9 / (250e-9 + 200e-9 + 100e-9) = 750e-9 / 550e-9 = 1.36 V
```

This would exceed VDD, indicating that the board-level capacitors and VRM must respond within 5 ns. The actual droop is limited by the combination of capacitance and inductance in the network, and frequency-domain impedance analysis gives a more accurate prediction than this simple time-domain estimate.

### Step 8: Summary

| Metric | Requirement | Achieved |
|---|---|---|
| Target impedance | 85 micro-ohms | Met from 10 MHz to 2 GHz (with embedded caps) |
| Fast droop (200 ps) | < 25.5 mV | 7.5 mV (estimated) |
| Package cap count | N/A | ~250 surface-mount + embedded |
| Cost adder for decoupling | N/A | ~$3-5 (MLCCs) + $5-10 (embedded caps) |

---

## Key Takeaways

- Sub-milliohm target impedance is extremely challenging and requires hundreds of package-level capacitors.
- Surface-mount MLCCs are limited by their mounting inductance (ESL) above approximately 300 MHz.
- Embedded capacitors with very low ESL bridge the gap between package MLCCs and on-die capacitors.
- The Ldi/dt calculation provides a quick sanity check; full frequency-domain impedance analysis is needed for design sign-off.
