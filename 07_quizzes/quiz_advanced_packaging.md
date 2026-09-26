# Quiz: Advanced Packaging

Test your knowledge of 2.5D, 3D, chiplet, and fan-out packaging technologies. Answer key is at the bottom.

---

### Q1. In TSMC's CoWoS technology, what does the silicon interposer connect?

A) The die to the heat sink  
B) Multiple die on top to the organic substrate below, using TSVs and fine-pitch RDL  
C) The PCB to the socket  
D) The molding compound to the lid  

---

### Q2. What is the primary advantage of Intel's EMIB over a full silicon interposer?

A) Higher interconnect density  
B) Lower cost due to small bridge size and no TSVs  
C) Better thermal performance  
D) Compatibility with wire bonding  

---

### Q3. What is the typical TSV diameter for high-density silicon interposers?

A) 0.5-1 um  
B) 5-10 um  
C) 50-100 um  
D) 500-1000 um  

---

### Q4. In an HBM3 stack, how many DRAM die are typically stacked?

A) 2  
B) 4-8  
C) 8-12  
D) 32-64  

---

### Q5. What is hybrid bonding?

A) A combination of wire bonding and flip-chip in the same package  
B) Direct copper-to-copper and oxide-to-oxide bonding without solder  
C) Bonding using both gold and aluminum wires  
D) A combination of TCB and mass reflow in the same assembly  

---

### Q6. What is reticle stitching used for?

A) Connecting wire bonds to substrate pads  
B) Creating silicon interposers larger than a single lithographic field  
C) Joining two package substrates side by side  
D) Repairing cracks in molding compound  

---

### Q7. What is the primary benefit of fan-out WLP over fan-in WLP?

A) Lower cost  
B) Higher I/O count by extending connections beyond the die footprint  
C) Better thermal performance  
D) Thicker package profile  

---

### Q8. In the reconstituted wafer process for FOWLP, what is the main purpose of the mold compound?

A) To provide electrical shielding  
B) To embed the die and create a wafer-like format for subsequent RDL processing  
C) To improve thermal conductivity  
D) To replace the silicon substrate  

---

### Q9. What is UCIe?

A) A package substrate material standard  
B) An open standard for die-to-die interconnect in chiplet architectures  
C) A thermal interface material specification  
D) A test equipment calibration protocol  

---

### Q10. What is "die shift" in fan-out packaging?

A) The intentional offset of the die from package center  
B) Unintended movement of die during mold curing, affecting RDL alignment  
C) The thermal expansion of the die during operation  
D) Displacement of the die during board-level reflow  

---

### Q11. Which company first deployed hybrid bonding in high-volume manufacturing?

A) Intel for processors  
B) Sony for CMOS image sensors  
C) TSMC for mobile SoCs  
D) AMD for GPUs  

---

### Q12. What is the key limitation of face-to-face (F2F) 3D stacking?

A) It cannot achieve fine pitch  
B) It is limited to two die (cannot stack more than two layers)  
C) It requires TSVs in both die  
D) It only works with memory die  

---

### Q13. In AMD's EPYC chiplet architecture, what function does the I/O die (IOD) perform?

A) Only CPU computation  
B) Memory controllers, PCIe, and Infinity Fabric interconnect  
C) RF transmission and reception  
D) Power regulation only  

---

### Q14. What is the approximate bandwidth density of UCIe advanced package specification?

A) 1 GB/s/mm  
B) 28 GB/s/mm  
C) Up to about 1,317 GB/s/mm  
D) 10 TB/s/mm  

---

### Q15. What differentiates chip-first from chip-last (RDL-first) fan-out processing?

A) Chip-first uses organic substrates; chip-last uses silicon  
B) In chip-first, die are placed before RDL formation; in chip-last, RDL is formed first  
C) Chip-first is only for single die; chip-last is only for multi-die  
D) Chip-first uses wire bonds; chip-last uses flip-chip  

---

### Q16. What is the compound yield problem in multi-die packaging?

A) Chemical compounds in the mold degrade over time  
B) The overall yield is the product of individual die yields, decreasing exponentially with die count  
C) Multiple die generate more heat, reducing thermal yield  
D) Testing multiple die requires compound test equipment  

---

## Answer Key

| Question | Answer | Explanation |
|---|---|---|
| Q1 | B | The silicon interposer in CoWoS provides fine-pitch interconnect between die on top and routes to the organic substrate below via TSVs. |
| Q2 | B | EMIB uses small bridge die (~30 mm^2) embedded in the substrate, much cheaper than a full interposer (2000+ mm^2) and requires no TSVs. |
| Q3 | B | High-density TSVs are typically 5-10 um diameter with 5:1 to 10:1 aspect ratios. |
| Q4 | C | HBM3 stacks typically contain 8-12 DRAM die on a base logic die. |
| Q5 | B | Hybrid bonding creates direct Cu-Cu metallic bonds and oxide-oxide dielectric bonds at sub-10 um pitch without solder. |
| Q6 | B | Reticle stitching combines multiple lithographic exposures to pattern interposers larger than the ~858 mm^2 single-reticle field size. |
| Q7 | B | Fan-out extends the redistribution layer beyond the die edge, enabling more solder ball positions than the die area alone allows. |
| Q8 | B | The mold compound encapsulates die on the carrier, creating a reconstituted wafer that can be processed with standard wafer-level equipment. |
| Q9 | B | UCIe (Universal Chiplet Interconnect Express) standardizes die-to-die interfaces for chiplet interoperability. |
| Q10 | B | Die shift is the unintended movement of die during mold compound dispensing and curing, requiring adaptive lithography for subsequent RDL. |
| Q11 | B | Sony deployed hybrid bonding in volume production for stacked CMOS image sensors before any logic application. |
| Q12 | B | F2F bonds two die face-to-face, consuming both front sides; a third die can only be added through TSVs in one of the pair (a F2F plus F2B stack), so pure F2F is a two-die structure. |
| Q13 | B | The IOD integrates all I/O functions (DDR controllers, PCIe, Infinity Fabric) while CCDs contain only CPU cores and cache. |
| Q14 | C | UCIe 1.0 gives 165-1317 GB/s/mm of shoreline bandwidth for the advanced package (25-55 um bump pitch; the top figure is at 25 um and 32 GT/s). The standard package gives 28-224 GB/s/mm, so option B is the standard-package low end. |
| Q15 | B | Chip-first places die before RDL; chip-last (RDL-first) forms the RDL on a carrier first, then places die onto the completed RDL. |
| Q16 | B | With N die each at yield Y, the package yield is approximately Y^N, which decreases rapidly as N increases. |
