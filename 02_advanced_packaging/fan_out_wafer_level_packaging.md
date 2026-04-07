# Fan-Out Wafer-Level Packaging

## Overview

Fan-out wafer-level packaging (FOWLP) extends the I/O count of wafer-level packages beyond the die footprint by redistributing connections to an area larger than the die. This substrate-less approach offers thin profiles, good electrical performance, and competitive cost for many applications.

---

### Q1. What is fan-out wafer-level packaging and how does it differ from fan-in WLP?

**Answer:**

Fan-out wafer-level packaging (FOWLP) is a packaging technology that creates a redistribution layer (RDL) extending beyond the die edge, allowing solder balls to be placed over a larger area than the die itself. In fan-in WLP, the solder ball array is confined within the die footprint, limiting the I/O count to what can fit on the die area at the minimum ball pitch. Fan-out overcomes this limitation by embedding the die in a reconstituted wafer (an epoxy mold compound panel or wafer with die placed at a pitch larger than the original wafer dicing pitch), then forming RDL layers that route signals from the die pads outward to solder ball positions beyond the die boundary. The fan-out area typically extends 0.5-2 mm beyond each die edge. This enables significantly more I/O connections than the die area alone would permit. For example, a 4 mm x 4 mm die in fan-in WLP at 0.4 mm ball pitch supports approximately 100 balls (10 x 10 array). The same die in a fan-out package with 1 mm fan-out per side (6 mm x 6 mm package) supports approximately 225 balls (15 x 15 array). FOWLP eliminates the need for a laminate substrate, reducing package height to as little as 0.3-0.5 mm (compared to 0.8-1.2 mm for a CSP with substrate). This thin profile is highly valued in mobile devices. The absence of a substrate also eliminates one level of interconnect, improving electrical performance and potentially reducing cost for high-volume applications.

---

### Q2. What is the reconstituted wafer process in FOWLP?

**Answer:**

The reconstituted wafer (also called reconfigured wafer) is the foundational step of FOWLP manufacturing. Known good die are picked from diced wafers and placed face-down on a temporary carrier (glass or silicon wafer coated with a thermal release adhesive) at a pitch larger than the original wafer pitch, leaving space between adjacent die for the fan-out area. This pick-and-place process must achieve high accuracy (typically plus or minus 5-10 micrometers) because the subsequent RDL lithography must align to the die pad positions. After die placement, an epoxy mold compound is dispensed over and around the die, encapsulating them in a disk-shaped mold that has the same diameter as a standard wafer (200 mm or 300 mm). The mold compound fills the gaps between die and covers the die backsides. After mold curing, the temporary carrier is released (typically by heating the thermal release adhesive), exposing the die active surfaces, which are now flush with or slightly protruding from the mold compound surface. The resulting reconstituted wafer looks like a standard wafer but contains individual die embedded in mold compound instead of a continuous silicon substrate. All subsequent processing (RDL formation, UBM, ball placement) proceeds on this reconstituted wafer using standard wafer-level processing equipment. The main challenges of the reconstitution process are die shift (movement of die during mold compound curing due to flow-induced forces and thermal expansion), which affects RDL alignment, and mold compound warpage due to CTE mismatch between the die, mold compound, and carrier.

---

### Q3. What is the difference between chip-first and chip-last (RDL-first) approaches?

**Answer:**

FOWLP has two fundamentally different process flows that determine when the die is placed relative to the RDL formation. In the chip-first approach, die are placed on the carrier first, embedded in mold compound to form the reconstituted wafer, and then the RDL layers are formed on top of the exposed die surface. This is the original and most widely used FOWLP approach (used in eWLB by Infineon/STATS ChipPAC and in TSMC InFO). Chip-first advantages include simplicity and lower cost for single-die packages. The main challenge is die shift during molding, which requires adaptive lithography (stepper alignment to actual die positions rather than a fixed grid) or relaxed design rules to accommodate misalignment. In the chip-last (RDL-first) approach, the RDL layers are formed first on a carrier wafer, then the die are bonded face-down onto the completed RDL using flip-chip-like attachment (thermocompression or mass reflow to pre-formed pads on the RDL). Mold compound is then applied over the die backsides. RDL-first advantages include better RDL quality (formed on a flat carrier without die topography), the ability to form many fine RDL layers without the cumulative alignment error from die shift, and easier integration of multiple die (the RDL provides the interconnect between die before they are placed). TSMC's InFO-L (InFO with local silicon interconnect) uses elements of RDL-first. The disadvantage is the additional die attachment step and the need for fine-pitch flip-chip bonding onto the RDL surface. RDL-first is generally preferred for multi-die fan-out packages and designs requiring many RDL layers or fine line/space.

---

### Q4. What is TSMC's InFO technology?

**Answer:**

Integrated Fan-Out (InFO) is TSMC's proprietary FOWLP platform, first deployed in volume production for the Apple A10 processor in the iPhone 7 (2016). InFO uses a chip-first process flow where the die is embedded in a reconstituted wafer and RDL layers are formed on top. The key innovation of InFO was achieving sufficiently fine RDL (line/space of 5/5 to 2/2 micrometers) and enough RDL layers (up to 5-6 layers) to route a high-pin-count mobile application processor without a traditional package substrate. InFO replaced the conventional flip-chip package-on-package (fcPoP) that Apple had previously used, eliminating the substrate and reducing the total package height by approximately 20 percent. This thinner profile freed space inside the phone for a larger battery. InFO has evolved into several variants. InFO-PoP stacks a memory package (LPDDR) on top of the fan-out logic package, replacing conventional PoP. InFO-oS (on substrate) combines fan-out RDL with a conventional organic substrate for higher I/O count and larger die. InFO-L integrates local silicon interconnect bridges (similar to EMIB) within the fan-out mold compound for high-density die-to-die connection. InFO-SoW (system-on-wafer) enables very large area integration by placing many die on a single reconstituted wafer. InFO remains the highest-volume advanced packaging technology in the world, driven primarily by Apple's use of InFO-PoP for every iPhone and iPad application processor. The technology's combination of thin profile, good electrical performance (no substrate parasitics), and cost competitiveness (no substrate cost) makes it compelling for high-volume mobile applications.

---

### Q5. What is eWLB and who uses it?

**Answer:**

Embedded Wafer-Level Ball Grid Array (eWLB) is a fan-out wafer-level packaging technology originally developed by Infineon Technologies and later commercialized by STATS ChipPAC (now part of JCET) and ASE. eWLB was one of the first FOWLP technologies to reach volume production (circa 2009) and has been widely adopted for mobile baseband processors, RF transceivers, power management ICs, and connectivity chips (Wi-Fi, Bluetooth, GPS). The eWLB process follows the standard chip-first FOWLP flow: die are placed face-down on a carrier, encapsulated in mold compound to form a reconstituted wafer, the carrier is removed, and RDL layers and solder balls are formed on the die-active surface. eWLB typically uses 1-3 RDL layers with line/space of 10/10 to 5/5 micrometers, which is coarser than TSMC InFO but sufficient for many applications. The technology supports package sizes from 3 mm x 3 mm to 15 mm x 15 mm and I/O counts up to several hundred. Multiple die can be integrated in a single eWLB package (multi-die fan-out) for system-in-package applications. eWLB-PoP variants support DRAM stacking on top. The key advantages of eWLB are its availability from multiple OSATs (not foundry-captive like InFO), proven high-volume manufacturability (billions of units shipped), and cost competitiveness with conventional CSP for appropriate die sizes. Qualcomm, MediaTek, and other mobile chipset vendors have used eWLB for baseband and connectivity die.

---

### Q6. What are the RDL design considerations in FOWLP?

**Answer:**

The redistribution layer (RDL) in FOWLP serves the same function as a package substrate but is formed using thin-film wafer-level processes, enabling finer features. Key design considerations include line/space, which determines routing density and is typically 2/2 to 10/10 micrometers for FOWLP (compared to 8/8 to 15/15 micrometers for organic substrate build-up layers). Finer line/space enables more signals to be routed in fewer layers, but increases process cost and defect risk. Layer count must be sufficient to route all signals from die pads to ball positions; simple single-die packages may need 2-3 RDL layers, while multi-die packages with high I/O may need 5-6 layers. Via technology connects between RDL layers; via diameter is typically 5-25 micrometers with either laser-drilled or photo-defined vias. Impedance control for high-speed signals requires controlled trace width and dielectric thickness; RDL dielectrics (polyimide, PBO, or low-loss alternatives) have typical relative permittivity of 2.9-3.5 and loss tangent of 0.005-0.015. The thin dielectric layers (5-10 micrometers) make 50-ohm impedance matching challenging because the trace width must be very narrow or differential pair routing must be used. Stress management is important: the RDL layers must withstand the CTE mismatch between silicon die and mold compound without cracking, and the polymer dielectrics must be flexible enough to absorb strain. Design for manufacturing (DFM) rules must account for die shift tolerances by using oversized capture pads and relaxed alignment rules near the die edge. Power delivery through the thin RDL layers requires wide traces or dedicated power planes to minimize IR drop.

---

### Q7. What are the thermal management considerations for FOWLP?

**Answer:**

FOWLP packages present unique thermal management challenges due to their thin profile, the insulating mold compound surrounding the die, and the absence of a traditional heat spreader or lid. In a fan-out package, the die backside is typically covered by a thin layer of mold compound (in chip-first face-down placement), which has very low thermal conductivity (0.5-1.5 W/m-K). This creates a thermal bottleneck on the primary heat dissipation path. For low-power devices (under 2-3 W), the heat can be conducted through the solder balls to the PCB, where ground planes and thermal vias spread it. The thermal resistance of this path (theta-JB) is typically 10-20 degrees C/W for fan-out packages, depending on ball count and PCB design. For higher-power devices, several enhancements are available. Backside grinding can thin or remove the mold compound over the die, exposing the die silicon for direct thermal contact with a heat sink or thermal pad. Thermal enhancement layers (copper or aluminum films) can be deposited or attached to the package backside to spread heat laterally. Embedded heat spreaders (thin copper foils) can be incorporated into the mold compound during reconstitution. Through-mold thermal vias (copper-filled vias through the mold compound) can provide additional thermal conduction paths. Despite these enhancements, FOWLP is generally limited to applications with moderate power dissipation (under 5-10 W). High-power SoCs (processors, GPUs) requiring 25+ W still use flip-chip BGA with metal lids and heat sinks, where the thermal path is much more effective. The fan-out package's primary advantage is thin profile and small size, not thermal performance.

---

### Q8. How does FOWLP cost compare to flip-chip CSP and traditional BGA?

**Answer:**

FOWLP cost competitiveness depends on die size, I/O count, and production volume. For small die (less than 5 mm x 5 mm) at high volume, FOWLP is typically less expensive than flip-chip CSP because it eliminates the package substrate, which is the dominant cost element in CSP. A flip-chip CSP requires a laminate substrate ($0.20-0.50 per unit), bumping, flip-chip assembly, underfill, and substrate processing. A FOWLP package requires reconstitution (die placement in mold compound), RDL formation, and ball placement, with an amortized cost of $0.10-0.30 per unit at high volume. However, the cost equation shifts for larger die. The reconstituted wafer has a fixed diameter (200 mm or 300 mm), so the number of packages per wafer decreases as die size increases (because each package is larger than the die). For die larger than approximately 8 mm x 8 mm, the wafer-level process becomes less efficient and FOWLP cost approaches or exceeds CSP cost. For very large die (15 mm+), traditional BGA is more economical. Multi-die FOWLP adds cost for additional RDL layers and the second die but can still be competitive with SiP approaches using substrate-based packaging. Capital equipment cost for FOWLP is significant (RDL lithography equipment, molding equipment, reconstitution tools) but is amortized over high volumes. FOWLP is most cost-effective in the sweet spot of small-to-medium die, moderate I/O count, and high volume -- which describes many mobile and IoT components perfectly.

---

### Q9. What is panel-level fan-out packaging?

**Answer:**

Panel-level fan-out packaging (PLFOP) extends the FOWLP concept from round wafer formats (200 mm or 300 mm diameter) to large rectangular panels (typically 300 mm x 300 mm up to 600 mm x 600 mm). The motivation is purely economic: a larger panel area accommodates more packages per manufacturing cycle, reducing the per-unit cost. A 300 mm round wafer has an area of approximately 70,700 mm-squared, while a 600 mm x 600 mm panel has an area of 360,000 mm-squared -- over 5 times larger. This ratio directly translates to throughput improvement for the RDL formation, ball placement, and other wafer-level steps, which are typically the cost bottleneck. The technical challenges of panel-level processing are significant. Panel warpage is more severe than wafer warpage due to the larger dimensions and the asymmetric stress distribution (no circular symmetry). RDL lithography equipment designed for round wafers must be adapted or replaced with equipment designed for panel formats; panel-compatible lithography, sputtering, electroplating, and molding tools have been developed by equipment suppliers. Die shift in reconstituted panels can be worse than in wafers due to the larger dimensions. Panel handling and automation infrastructure is different from wafer handling. Despite these challenges, several companies (Samsung Electro-Mechanics, ASE, JCET, and others) have established panel-level fan-out production lines. The cost reduction from panel-level processing is estimated at 20-40 percent compared to wafer-level fan-out, making it increasingly attractive for high-volume, cost-sensitive applications such as IoT devices, wearables, and automotive sensors.

---

### Q10. How does fan-out packaging support heterogeneous integration?

**Answer:**

Fan-out packaging is a natural platform for heterogeneous integration because the reconstituted wafer can embed die of different sizes, functions, and process technologies in a shared mold compound, with the RDL providing high-density interconnection between them. Multi-die fan-out (also called fan-out system-in-package or FO-SiP) places two or more die side by side in the reconstituted wafer, routes inter-die connections through the shared RDL, and brings all external connections to a common solder ball array on the package bottom. Examples include integrating a digital processor with an RF transceiver, power management IC, or memory die in a single fan-out package. Compared to 2.5D silicon interposer integration, multi-die fan-out is less expensive (no silicon interposer needed), but the RDL line/space (2-10 micrometers) is coarser than what a silicon interposer provides (0.4-2 micrometers), limiting the die-to-die bandwidth density. For applications where moderate die-to-die bandwidth is sufficient (mobile, IoT, automotive), multi-die fan-out is highly attractive. The RDL-first (chip-last) process variant is particularly well suited for multi-die integration because the RDL is formed on a flat carrier before die are placed, enabling precise interconnect routing between die positions. TSMC's InFO-L and InFO-SoW extend this concept by incorporating local silicon bridges for high-density interconnect between selected die pairs while using fan-out RDL for global routing. Fan-out packaging also supports embedded passive components (capacitors, inductors formed in the RDL layers), further increasing system integration density without increasing package area.

---

## Further Reading

- [2.5D and 3D Packaging](2_5d_and_3d_packaging.md)
- [Chiplet Architectures](chiplet_architectures.md)
- [RDL and Routing](../03_substrate_and_interposer_design/rdl_and_routing.md)
- [Assembly Processes](../06_manufacturing_and_test/assembly_processes.md)
