# Quiz: Electrical Performance

Test your knowledge of signal integrity, power delivery, and EMI in SoC packages. Answer key is at the bottom.

---

### Q1. What is the target impedance for a PDN with 0.8 V supply, 3% voltage tolerance, and 200 A transient current?

A) 0.12 milliohms  
B) 1.2 milliohms  
C) 12 milliohms  
D) 120 milliohms  

---

### Q2. What is the dominant source of insertion loss in package traces at 28 GHz?

A) Dielectric loss  
B) Radiation loss  
C) Conductor loss (including surface roughness effects)  
D) Via stub resonance  

---

### Q3. What causes Ldi/dt voltage droop on the power supply?

A) Resistive loss in copper traces  
B) Parasitic inductance in the power delivery path during fast current transients  
C) Capacitive coupling between signal traces  
D) Dielectric breakdown  

---

### Q4. What is the typical characteristic impedance for single-ended package traces?

A) 10 ohms  
B) 50 ohms  
C) 100 ohms  
D) 300 ohms  

---

### Q5. What is the purpose of anti-pad optimization around package vias?

A) To increase via resistance  
B) To tune the via impedance by adjusting the clearance in the reference plane  
C) To prevent solder from flowing into the via  
D) To improve thermal conductivity  

---

### Q6. What is crosstalk in a package?

A) Communication between two chiplets  
B) Unintended electromagnetic coupling between adjacent signal traces  
C) Noise from the power supply  
D) Signal reflection from impedance discontinuities  

---

### Q7. Which decoupling capacitor type provides the lowest mounting inductance?

A) 0402 surface-mount MLCC  
B) 0201 surface-mount MLCC  
C) Embedded thin-film capacitor in the substrate  
D) Through-hole electrolytic capacitor  

---

### Q8. What is the skin depth of copper at 10 GHz?

A) 0.066 um  
B) 0.66 um  
C) 6.6 um  
D) 66 um  

---

### Q9. What does the S21 parameter represent in a package S-parameter model?

A) Input reflection coefficient (return loss)  
B) Forward transmission coefficient (insertion loss)  
C) Reverse isolation  
D) Output return loss  

---

### Q10. Why does differential signaling provide better EMI performance than single-ended?

A) It uses less current  
B) The equal and opposite currents produce canceling electromagnetic fields  
C) It operates at lower frequency  
D) It requires a wider trace  

---

### Q11. What is the effect of a via stub on signal integrity?

A) It increases signal bandwidth  
B) It creates a resonant notch in the insertion loss at a frequency related to stub length  
C) It improves impedance matching  
D) It reduces crosstalk  

---

### Q12. What frequency range is the package PDN primarily responsible for decoupling?

A) DC to 100 Hz  
B) 100 Hz to 10 kHz  
C) 10 MHz to 1 GHz  
D) 10 GHz to 100 GHz  

---

### Q13. For a flip-chip package delivering 300 A at 0.8 V, what is the approximate maximum acceptable DC resistance of the package power path?

A) 67 micro-ohms (for < 20 mV IR drop)  
B) 6.7 milliohms  
C) 67 milliohms  
D) 0.67 ohms  

---

### Q14. What is conformal shielding in SiP packages?

A) A plastic cover over the package  
B) A thin sputtered metal coating on the molding compound surface  
C) A copper foil wrapped around the substrate  
D) An FR-4 shield board placed over the package  

---

### Q15. What is power plane resonance?

A) The vibration of the package at power-on  
B) Standing electromagnetic waves between power and ground planes at specific frequencies  
C) The oscillation of the VRM output  
D) Acoustic noise from capacitors  

---

### Q16. What is the primary benefit of integrated voltage regulators (IVR) for package design?

A) They eliminate the need for a heat sink  
B) They reduce the input current to the package by converting at higher input voltage  
C) They remove all decoupling capacitor requirements  
D) They eliminate IR drop in the substrate  

---

## Answer Key

| Question | Answer | Explanation |
|---|---|---|
| Q1 | A | Z_target = (0.03 * 0.8) / 200 = 0.00012 ohms = 0.12 milliohms. |
| Q2 | C | At 28 GHz, conductor loss (including surface roughness, which can double the loss) dominates over dielectric loss, typically by 3:1 or more. |
| Q3 | B | V = L * di/dt: the parasitic inductance in the VDD-VSS loop causes voltage droop during fast current transients. |
| Q4 | B | 50 ohms single-ended is the standard impedance for most high-speed interfaces. Differential pairs are typically 85-100 ohms. |
| Q5 | B | Anti-pad (clearance hole in the reference plane around a via) size affects via capacitance; adjusting it tunes the via impedance to minimize reflections. |
| Q6 | B | Crosstalk is electromagnetic coupling (capacitive and inductive) between nearby signal conductors, transferring noise from aggressor to victim. |
| Q7 | C | Embedded thin-film capacitors have sub-10 pH inductance because they are directly connected to power planes within the substrate, with no external solder joints. |
| Q8 | B | delta = sqrt(1.7e-8 / (pi * 10e9 * 4*pi*1e-7)) = 0.66 um. |
| Q9 | B | S21 represents the forward transmission (insertion loss when expressed in dB as 20*log10 of the magnitude of S21). |
| Q10 | B | The equal and opposite currents in a differential pair create canceling far-field radiation, reducing EMI emissions. |
| Q11 | B | A via stub creates a quarter-wave resonance at f = c/(4*L*sqrt(Dk)), causing a deep insertion loss notch at that frequency. |
| Q12 | C | The package provides decoupling in the 10 MHz to 1 GHz range, bridging between board-level capacitors (below 10 MHz) and on-die capacitors (above 1 GHz). |
| Q13 | A | For < 20 mV IR drop at 300 A: R = V/I = 0.020/300 = 67 micro-ohms maximum. |
| Q14 | B | Conformal shielding is a thin (3-10 um) sputtered metal layer applied to the exterior of the molded package for EMI attenuation. |
| Q15 | B | Power and ground planes form a parallel-plate structure that supports standing electromagnetic waves at frequencies determined by the plane dimensions. |
| Q16 | B | IVRs convert from a higher input voltage (e.g., 1.8V) on-die, reducing the input current (P=VI) and thus reducing the number of power bumps and package IR drop needed. |
