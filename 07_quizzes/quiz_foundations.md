# Quiz: Foundations of SoC Packaging

Test your knowledge of fundamental packaging concepts. Choose the best answer for each question. Answer key is at the bottom.

---

### Q1. What is the primary reason flip-chip interconnect has lower parasitic inductance than wire bonding?

A) Flip-chip uses gold instead of copper  
B) Flip-chip bumps are shorter than wire bond loops, creating smaller current loops  
C) Flip-chip solder has lower resistivity than bond wire metal  
D) Flip-chip packages always use ceramic substrates  

---

### Q2. What defines a Chip Scale Package (CSP) according to JEDEC?

A) A package with fewer than 100 pins  
B) A package that uses flip-chip interconnect  
C) A package whose area is no more than 1.2 times the die area  
D) A package thinner than 0.5 mm  

---

### Q3. Which package type provides the highest I/O density for a given package footprint?

A) QFP (Quad Flat Package)  
B) QFN (Quad Flat No-lead)  
C) BGA (Ball Grid Array)  
D) SOIC (Small Outline IC)  

---

### Q4. What is the typical parasitic inductance of a single wire bond?

A) 1-5 fH  
B) 10-50 pH  
C) 0.5-1.5 nH  
D) 1-5 uH  

---

### Q5. In a Package-on-Package (PoP) configuration, what is typically in the top package?

A) The application processor  
B) The power management IC  
C) LPDDR memory  
D) The RF transceiver  

---

### Q6. What is the primary advantage of copper wire bonding over gold wire bonding?

A) Higher bonding speed  
B) Lower material cost  
C) No risk of die pad cratering  
D) Better corrosion resistance  

---

### Q7. What is under-bump metallurgy (UBM)?

A) The solder alloy used in flip-chip bumps  
B) A multi-layer metal stack on the die pad that provides adhesion and wettability for bumps  
C) The copper trace beneath each BGA ball  
D) The flux material applied before reflow  

---

### Q8. Which of the following is NOT a function of a semiconductor package?

A) Electrical interconnection to the PCB  
B) Thermal dissipation  
C) Transistor fabrication  
D) Environmental protection  

---

### Q9. What is the typical CTE of silicon?

A) 0.26 ppm/K  
B) 2.6 ppm/K  
C) 14 ppm/K  
D) 26 ppm/K  

---

### Q10. What determines the moisture sensitivity level (MSL) of a package?

A) The operating temperature range  
B) The number of I/O pins  
C) The package's susceptibility to moisture-induced damage during reflow  
D) The maximum storage temperature  

---

### Q11. Which interconnect technology enables the finest pitch (highest density)?

A) Wire bonding  
B) C4 solder bumps  
C) Copper pillar bumps  
D) Hybrid bonding  

---

### Q12. What is the primary purpose of underfill in a flip-chip package?

A) To improve thermal conductivity  
B) To redistribute CTE mismatch stress from individual bumps to the entire interface  
C) To provide electrical insulation between bumps  
D) To prevent oxidation of the solder  

---

### Q13. An FCBGA package with 2500 BGA balls at 1.0 mm pitch would have approximate dimensions of:

A) 25 mm x 25 mm  
B) 35 mm x 35 mm  
C) 50 mm x 50 mm  
D) 75 mm x 75 mm  

---

### Q14. What is the main disadvantage of hermetic packages compared to plastic packages?

A) Lower reliability  
B) Higher moisture absorption  
C) Significantly higher cost (10-100x)  
D) Lower I/O density  

---

### Q15. Which statement about second-level interconnect (package to PCB) is correct?

A) It always uses wire bonding  
B) BGA solder balls are the most common form for high pin-count packages  
C) It operates at finer pitch than first-level interconnect  
D) It does not require thermal cycling qualification  

---

### Q16. What is the primary driver of the industry shift from monolithic SoCs to chiplet architectures?

A) Reduced packaging cost  
B) Yield economics and the ability to mix process nodes  
C) Elimination of thermal management challenges  
D) Simplified testing procedures  

---

## Answer Key

| Question | Answer | Explanation |
|---|---|---|
| Q1 | B | Flip-chip bumps (50-100 um) are much shorter than wire bonds (2-3 mm), creating smaller inductive loops (10-50 pH vs 0.5-1.5 nH). |
| Q2 | C | JEDEC defines CSP as a package with area no greater than 1.2x the die area. |
| Q3 | C | BGA uses an area array of connections, providing far more I/O per unit area than peripheral-only packages (QFP, QFN, SOIC). |
| Q4 | C | A typical wire bond (2-3 mm long, 25 um diameter) has 0.5-1.5 nH inductance. |
| Q5 | C | PoP for mobile devices places the processor in the bottom package and LPDDR memory in the top package. |
| Q6 | B | Copper wire costs far less than gold wire (gold can add several dollars per package), making cost the primary driver. |
| Q7 | B | UBM is a metal stack (Ti/Cu/Ni or similar) deposited on the die pad to enable solder bump or copper pillar attachment. |
| Q8 | C | Transistor fabrication is done in the wafer fab (front-end), not in the package. The package provides interconnect, thermal, mechanical, and environmental functions. |
| Q9 | B | Silicon has a CTE of approximately 2.6 ppm/K, much lower than organic substrates (14-17 ppm/K). |
| Q10 | C | MSL classifies how susceptible a plastic package is to moisture absorption and subsequent damage during SMT reflow. |
| Q11 | D | Hybrid bonding achieves sub-1 um pitch, far finer than copper pillars (~40 um), C4 (~130 um), or wire bond (~50 um pad pitch). |
| Q12 | B | Underfill's primary function is mechanical: redistributing CTE mismatch strain from individual bumps to the entire die-substrate interface, improving fatigue life 10-100x. |
| Q13 | C | 2500 balls at 1.0 mm pitch in a 50x50 array gives a 50 mm x 50 mm BGA. |
| Q14 | C | Hermetic (ceramic/metal) packages cost 10-100x more than plastic packages, limiting their use to military, aerospace, and medical applications. |
| Q15 | B | BGA solder balls are the dominant second-level interconnect for high pin-count packages, providing area-array connections to the PCB. |
| Q16 | B | Chiplet architectures are primarily driven by the yield economics of smaller die and the ability to fabricate different functions at different (optimal) process nodes. |
