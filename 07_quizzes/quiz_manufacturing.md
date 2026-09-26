# Quiz: Manufacturing and Test

Test your knowledge of assembly processes, testing, yield, and reliability. Answer key is at the bottom.

---

### Q1. What is thermocompression bonding (TCB) primarily used for?

A) Attaching BGA solder balls to the substrate  
B) Bonding fine-pitch die (microbumps) where mass reflow would cause bridging  
C) Welding the heat spreader lid to the substrate  
D) Connecting wire bonds to the leadframe  

---

### Q2. What is the typical peak reflow temperature for lead-free SAC305 solder?

A) 150-180 degrees Celsius  
B) 183 degrees Celsius  
C) 240-260 degrees Celsius  
D) 350-400 degrees Celsius  

---

### Q3. What is the purpose of preconditioning in JEDEC reliability qualification?

A) To prepare the test equipment  
B) To simulate board-level SMT reflow by moisture soaking and reflow cycling  
C) To clean the package before testing  
D) To calibrate the temperature cycling chamber  

---

### Q4. What is the Coffin-Manson equation used to predict?

A) The thermal conductivity of solder  
B) The number of thermal cycles to solder joint fatigue failure  
C) The package warpage at reflow temperature  
D) The electromigration current density threshold  

---

### Q5. What does DPPM stand for and what is a typical target for automotive?

A) Defective Parts Per Million; target < 1 DPPM  
B) Die Placement Per Minute; target > 100  
C) Dielectric Performance Per Material; target > 99%  
D) Data Points Per Measurement; target > 1000  

---

### Q6. What is burn-in used for?

A) Removing excess solder from BGA balls  
B) Screening infant mortality failures by operating at elevated voltage and temperature  
C) Curing the molding compound  
D) Testing high-speed I/O at maximum data rate  

---

### Q7. In a BGA package, what is the difference between SMD and NSMD pads?

A) SMD uses solder; NSMD does not  
B) In SMD, the solder mask defines the pad size; in NSMD, the copper pad defines the size  
C) SMD is for signal; NSMD is for power  
D) SMD is used on the die side; NSMD on the board side  

---

### Q8. What is Known Good Die (KGD)?

A) Die that have been visually inspected  
B) Bare die tested to a quality level equivalent to a fully packaged and tested device  
C) Die from the center of the wafer  
D) Die with no wire bond connections  

---

### Q9. What is the electromigration critical current density for SnAg solder bumps?

A) Approximately 10 A/cm-squared  
B) Approximately 10^4 A/cm-squared  
C) Approximately 10^8 A/cm-squared  
D) There is no current density limit for solder  

---

### Q10. What is scanning acoustic microscopy (SAM) used to detect?

A) Electrical shorts between traces  
B) Delamination and voids at internal package interfaces  
C) Wire bond placement accuracy  
D) Solder ball diameter  

---

### Q11. What is the primary advantage of stealth dicing over blade dicing?

A) Lower cost  
B) Higher throughput  
C) Cleaner die edges with no chipping and no kerf loss  
D) Compatibility with all wafer thicknesses  

---

### Q12. What does AEC-Q100 specify?

A) PCB assembly standards  
B) Reliability qualification requirements for automotive-grade integrated circuits  
C) Test equipment calibration procedures  
D) Solder paste composition standards  

---

### Q13. According to the bathtub curve, what characterizes the "useful life" phase?

A) Rapidly increasing failure rate  
B) Rapidly decreasing failure rate  
C) Approximately constant, low failure rate  
D) Zero failures  

---

### Q14. What is the purpose of the flux in solder reflow?

A) To increase the melting point of solder  
B) To clean oxide from metal surfaces and promote solder wetting  
C) To provide mechanical strength to the joint  
D) To cool the solder after reflow  

---

### Q15. What is wire sweep in molding?

A) A cleaning step to remove wire debris  
B) Lateral displacement of bond wires by mold compound flow  
C) A wire bond technique for connecting multiple pads  
D) The removal of bond wires after molding  

---

### Q16. Why is X-ray inspection important for BGA packages?

A) It measures the electrical resistance of solder balls  
B) It reveals solder joint quality (voids, bridging) hidden beneath the package body  
C) It determines the die thickness  
D) It tests for electromigration  

---

## Answer Key

| Question | Answer | Explanation |
|---|---|---|
| Q1 | B | TCB applies controlled force and temperature to bond one die at a time, required for fine-pitch microbumps (below ~50 um) where mass reflow causes bridging. |
| Q2 | C | SAC305 melts at 217-220 C; peak reflow is typically 240-260 C to ensure complete wetting and joint formation. |
| Q3 | B | Preconditioning (JESD22-A113, using the J-STD-020 moisture sensitivity levels) simulates the moisture exposure and reflow thermal shock that packages experience during board assembly. |
| Q4 | B | Coffin-Manson relates cyclic strain range to the number of cycles to fatigue failure: N_f = C * (Delta_gamma)^(-n). |
| Q5 | A | Defective Parts Per Million; automotive targets < 1 DPPM, far stricter than consumer (50-500 DPPM). |
| Q6 | B | Burn-in operates devices at elevated voltage and temperature to precipitate latent defects before they reach the customer. |
| Q7 | B | SMD (solder-mask-defined) pads are defined by the solder mask opening; NSMD (non-solder-mask-defined) pads are defined by the copper pad, producing a better solder fillet and fatigue life. |
| Q8 | B | KGD are bare die that have been comprehensively tested at wafer level to ensure they function correctly before multi-die assembly. |
| Q9 | B | The EM threshold for SnAg solder is approximately 10^4 A/cm^2, above which void formation leads to open-circuit failure. |
| Q10 | B | SAM uses ultrasonic reflection to image internal interfaces, revealing delamination and voids that are invisible to optical or X-ray inspection. |
| Q11 | C | Stealth dicing creates internal modifications with a laser, then separates die by expansion, producing clean edges with no material loss. |
| Q12 | B | AEC-Q100 is the automotive electronics council standard for stress test qualification of ICs, specifying test conditions, sample sizes, and acceptance criteria. |
| Q13 | C | The useful life phase has a low, approximately constant failure rate from random causes, between the infant mortality and wear-out phases. |
| Q14 | B | Flux is a chemical agent that removes metal oxides from pad and solder surfaces during heating, enabling the molten solder to wet and bond to the pads. |
| Q15 | B | Wire sweep is the lateral displacement of wire bonds caused by the flow of molding compound during transfer molding, potentially causing shorts. |
| Q16 | B | BGA solder joints are hidden beneath the package body, making visual inspection impossible. X-ray imaging reveals internal joint quality, voids, and bridging. |
