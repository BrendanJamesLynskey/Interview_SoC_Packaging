# RDL and Routing

## Overview

Redistribution layers (RDL) reroute electrical connections from die bond pad locations to more convenient positions for external interconnect. RDL is used in wafer-level packages, fan-out packages, and silicon interposers, and its design directly impacts signal integrity, power delivery, and manufacturing yield.

---

### Q1. What is a redistribution layer and why is it needed?

**Answer:**

A redistribution layer (RDL) is a thin-film metal interconnect layer formed on a die surface, interposer, or reconstituted wafer to reroute electrical connections from the original bond pad positions to new locations. RDL is necessary because the bond pads on a die are typically arranged at the die periphery (for wire bond) or in application-specific locations optimized for the on-chip circuit layout, which may not align with the desired solder ball positions on the package bottom. In a fan-in wafer-level package, the RDL reroutes peripheral pads to an area-array grid within the die footprint. In a fan-out package, the RDL extends connections beyond the die edge to a larger ball array. In a silicon interposer, the RDL provides lateral routing between die. The RDL stack typically consists of a polymer dielectric layer (polyimide, PBO, or low-k polymer, 3-15 micrometers thick) deposited on the surface, vias opened to the underlying pads, and a copper trace layer formed by sputtering a seed layer, photolithography, electroplating, and etching. Multiple RDL layers can be stacked (2-6 layers is typical) to provide routing density comparable to a thin-film substrate. Each additional layer adds routing capacity but also adds cost and yield risk. The trace dimensions (line/space) depend on the fabrication method: 2/2 micrometers for advanced silicon interposer RDL using semiconductor lithography, 5/5 to 10/10 micrometers for fan-out RDL using advanced wafer-level processing, and 15/15 micrometers or coarser for conventional substrates.

---

### Q2. What fabrication processes are used to form RDL?

**Answer:**

RDL fabrication uses thin-film metallization processes similar to semiconductor BEOL but typically at coarser features and on different substrates. The primary process flow for each RDL layer involves dielectric deposition, via formation, seed layer deposition, photoresist patterning, copper electroplating, resist strip, and seed etch. The dielectric layer is applied by spin-coating (for polyimide or PBO) or lamination (for dry-film polymers). Vias are formed by photolithography (for photosensitive dielectrics like PBO, the polymer is directly exposed and developed to create via openings) or by laser drilling (for non-photosensitive films). A copper seed layer (typically Ti or TiW adhesion layer plus Cu seed, total 200-500 nm) is deposited by sputtering (PVD) over the entire surface. Photoresist is applied and patterned to define the trace and pad locations. Copper is electroplated into the resist openings to the desired thickness (2-10 micrometers). The resist is stripped and the seed layer is etched away in areas not covered by plated copper. This semi-additive process (SAP) produces traces with well-defined sidewalls and minimal undercut, enabling fine line/space. For silicon interposer RDL, the process is closer to standard BEOL: copper damascene processing where trenches are etched into a deposited dielectric (SiO2), copper is plated to fill the trenches, and CMP planarizes the surface. Damascene produces the finest features (sub-1 micrometer) but is more expensive due to CMP steps. The choice between SAP and damascene depends on the required feature size and the manufacturing platform.

---

### Q3. How does RDL line/space affect routing density and capacity?

**Answer:**

The line/space of the RDL determines how many signal traces can be routed between two adjacent bumps or vias, which directly sets the routing capacity per layer. The routing capacity is calculated by the number of traces that fit in the available space between features. For example, between two microbumps at 40 micrometer pitch with 25 micrometer pad diameter, the available routing channel is 15 micrometers wide. At 2/2 micrometer L/S, this channel can accommodate 3 traces (3 lines at 2 um plus 3 spaces at 2 um = 12 um, fitting within 15 um). At 5/5 micrometer L/S, only 1 trace fits (1 line at 5 um plus 2 spaces at 5 um = 15 um). This 3x difference in traces per channel translates directly to 3x more routing density per RDL layer, or equivalently, 3x fewer RDL layers needed for the same total routing. For a silicon interposer connecting two die with 1000 signals across a 10 mm edge, at 2/2 micrometer L/S with 3 traces per channel, approximately 2 RDL layers suffice. At 5/5 micrometer L/S with 1 trace per channel, 6 RDL layers might be needed. Fewer layers means lower cost and higher yield. However, finer L/S increases the per-layer fabrication cost (higher-resolution lithography, tighter process control) and reduces manufacturing yield (more defects per area at finer features). The optimal L/S balances routing needs against cost and yield, and different applications land at different points on this spectrum. High-bandwidth AI accelerators justify the cost of 2/2 micrometer RDL; mobile fan-out packages use 5-10 micrometer RDL as a cost-performance sweet spot.

---

### Q4. What are the via technologies used in RDL?

**Answer:**

Vias in RDL connect metal layers vertically and come in several types depending on the fabrication method. Photo-defined vias are created by exposing and developing a photosensitive dielectric (PBO, photosensitive polyimide). The polymer is spin-coated, exposed through a mask with the via pattern, developed to remove the exposed (or unexposed, depending on tone) regions, and cured. This produces vias with typical diameter of 5-25 micrometers and tapered sidewalls. Photo-defined vias offer high throughput and good registration since the via locations are defined by the same lithography tools used for traces. Laser-drilled vias are formed by ablating the dielectric with a focused laser beam (UV or CO2). Laser vias can be formed in non-photosensitive dielectrics and offer flexibility in via size and placement. Typical diameter is 10-50 micrometers. Laser drilling is sequential (one via at a time), which limits throughput compared to batch photo-definition. Dry-etch vias (RIE) are formed by plasma etching through a hard mask, analogous to BEOL via formation. This method produces the finest and most controlled via profiles (sub-5 micrometer diameter with vertical sidewalls) but is more expensive and is primarily used in silicon interposer RDL. Via design considerations include capture pad size (must be larger than the via to account for alignment tolerance), via aspect ratio (height-to-diameter ratio, typically limited to 1:1 for reliable copper filling), and via resistance (a 10 micrometer diameter, 10 micrometer deep copper via has resistance of approximately 2 milliohms). Stacked vias through multiple RDL layers save area but require careful process control to ensure complete copper fill at each level.

---

### Q5. How is impedance controlled in RDL traces?

**Answer:**

Impedance control in RDL traces is essential for high-speed signaling but is more challenging than in organic substrates due to the thinner dielectrics and narrower traces. The characteristic impedance of a microstrip trace on RDL is determined by the trace width, dielectric thickness to the ground plane, dielectric constant, and copper thickness. For a microstrip on PBO dielectric (Dk approximately 3.2): a 50-ohm trace on a 10 micrometer thick dielectric requires a trace width of approximately 17 micrometers. On a 3 micrometer thick dielectric (typical for silicon interposer), the required trace width for 50 ohms is approximately 5 micrometers. These dimensions are within fabrication capability, but tolerance control is critical. A 10 percent variation in dielectric thickness or trace width causes approximately 5-8 percent impedance variation, which may exceed the plus or minus 10 percent specification. For differential pairs (100 ohm differential), the trace width and spacing must be simultaneously controlled. Differential pairs are preferred in RDL because the impedance is less sensitive to ground plane distance (the coupling between the pair partially defines the impedance). Stripline configurations (trace between two ground planes) provide better shielding and more stable impedance than microstrip but require additional RDL layers. In practice, many die-to-die links on interposers use single-ended signaling at low swing (200-400 mV) rather than impedance-matched differential signaling, because the short trace lengths (under 10 mm) do not require transmission-line treatment. For these short links, the interconnect acts as a lumped capacitive load rather than a distributed transmission line, and the design priority shifts from impedance matching to minimizing capacitive loading.

---

### Q6. What are the dielectric materials used in RDL and how do they compare?

**Answer:**

Several dielectric materials are used in RDL, each with different properties suited to different applications. Polyimide (PI) is one of the oldest RDL dielectrics, offering excellent thermal stability (decomposition temperature above 400 degrees Celsius), good mechanical properties (high elongation), and chemical resistance. Its Dk is approximately 3.2-3.5 and Df is 0.010-0.020. Non-photosensitive polyimide requires a separate patterning step (metal hard mask or laser drilling for vias), while photosensitive polyimide (PSPI) can be directly patterned. PBO (polybenzoxazole) is the most widely used RDL dielectric for fan-out and WLP. PBO is inherently photosensitive, enabling direct patterning of vias without a separate mask. It has Dk of approximately 2.9-3.2 and Df of 0.008-0.015, slightly lower loss than polyimide. PBO has good adhesion to copper, low moisture absorption, and can be processed at relatively low temperatures (below 250 degrees Celsius for final cure). Silicon dioxide (SiO2) is used in silicon interposer RDL, deposited by PECVD. SiO2 has Dk of 3.9 and extremely low Df (less than 0.001), providing the best high-frequency performance. However, SiO2 is brittle and requires CMP for planarization. Low-k dielectrics (Dk 2.5-3.0) from the BEOL process library can be used in silicon interposer RDL for further loss reduction. For fan-out applications targeting high-frequency performance (5G mmWave, high-speed SerDes), new low-loss polymer dielectrics with Df below 0.005 are being developed. The dielectric choice involves tradeoffs between electrical performance, processability, cost, and reliability.

---

### Q7. What routing strategies are used for multi-die packages?

**Answer:**

Routing in multi-die packages must connect die-to-die signals, die-to-BGA signals, and power/ground distribution, often in a limited number of RDL or substrate layers. Several strategies address this complexity. Die-to-die signals should be routed on the shortest path between the adjacent die edges, using the finest-pitch layers available. For a silicon interposer, these signals occupy the top 1-2 RDL layers with 0.4-2 micrometer L/S. For fan-out, the innermost RDL layers (closest to the die) handle die-to-die routing at 2-5 micrometer L/S. Escape routing fans out the dense bump array from each die to the sparser routing grid of the substrate or lower RDL layers. This typically requires dogbone patterns (short traces connecting bump pads to nearby vias) that transition signals to lower layers. The escape routing strategy depends on the bump pitch and the number of routing layers available. Power planes should be dedicated layers (full or partial copper planes) to provide low-resistance power distribution. Ground planes serve double duty as power return and electromagnetic shielding for signal layers. Signal-ground-signal (SGS) layer ordering minimizes crosstalk by placing ground planes between signal layers. Routing congestion hotspots typically occur at the die edges where all die-to-die signals converge; this is where the finest L/S is most needed. Automated routing tools adapted for package-level routing (Cadence Allegro Package Designer, Siemens Xpedition) are used for physical design, with manual routing for critical high-speed nets.

---

### Q8. How does bump pitch scaling affect RDL design?

**Answer:**

As bump pitch decreases (from 130 micrometers for C4, to 40 micrometers for microbumps, to sub-10 micrometers for hybrid bonding), the RDL design must adapt correspondingly. At 130 micrometer bump pitch, organic substrate build-up layers at 12/12 micrometer L/S can easily route between bumps: the 130 micrometer pitch with 80 micrometer pad leaves 50 micrometers of routing channel, accommodating 2 traces at 12/12. This allows efficient escape routing and moderate die-to-die connectivity on organic substrates alone. At 40 micrometer microbump pitch, the 40 micrometer pitch with 25 micrometer pad leaves only 15 micrometers of routing channel. At 2/2 micrometer L/S (silicon interposer), 3-4 traces fit per channel. At 5/5 micrometer L/S (advanced fan-out), only 1 trace fits. This means that silicon interposer RDL is necessary for dense routing between fine-pitch bumps. At sub-10 micrometer hybrid bonding pitch, the bond pad size is 3-5 micrometers with approximately 5-10 micrometer pitch. The routing channel is 2-5 micrometers wide, requiring sub-micrometer L/S -- essentially BEOL-level metallization. Only silicon-based processing can achieve this, and the RDL becomes indistinguishable from back-end interconnect layers. The implication is that finer bumps demand finer RDL, which demands more expensive fabrication. The package designer must choose a bump pitch and RDL technology that collectively meet the routing density and bandwidth requirements while staying within cost targets. Often, a heterogeneous approach is optimal: fine-pitch bumps and silicon RDL only at high-bandwidth die-to-die interfaces, with coarser bumps and organic routing elsewhere.

---

### Q9. What are the yield and defect challenges in RDL fabrication?

**Answer:**

RDL yield is determined by the probability of defects in the metal traces, vias, and dielectric layers across the entire RDL area. Common defect types include opens (broken traces due to particles, lithography defects, or underplating), shorts (copper bridges between adjacent traces due to residual seed layer, electroplating overplate, or particles), via voids (incomplete copper fill in vias, causing high resistance or open connections), and dielectric pinholes (thin spots or holes in the polymer that cause shorts between layers). The defect density (defects per unit area) determines the yield through a Poisson or Murphy model: for a given defect density D and total RDL area A, the yield is approximately Y = exp(-D * A). For example, at a defect density of 0.01 defects/cm-squared and an RDL area of 100 cm-squared (a 300 mm wafer), the yield per wafer is approximately 37 percent -- meaning nearly two-thirds of wafers have at least one defect. This is why fine L/S (which increases defect density) and large area (which increases exposure) both degrade yield. Yield improvement requires reducing defect sources: cleaner processing environments, optimized lithography to minimize residue, carefully controlled plating chemistry, and thorough seed etch. Electrical testing of RDL (continuity testing of all traces and isolation testing between adjacent traces) is performed at wafer level before die attachment to identify and map defective areas. Redundant routing (spare traces that can be activated by laser fuse or e-fuse) provides a yield recovery mechanism for critical high-density interconnects.

---

### Q10. How are signal and power routes co-designed in the RDL stack?

**Answer:**

Signal and power routes in the RDL stack are co-designed to minimize mutual interference while efficiently using the available layers. The general principle is to separate high-speed signal layers from power distribution layers using intervening ground planes. A typical 6-layer RDL stack might be organized as: Layer 1 (top, closest to die) -- die-to-die high-speed signals; Layer 2 -- ground plane (reference for Layer 1 signals and shield); Layer 3 -- power plane (VDD distribution); Layer 4 -- ground plane; Layer 5 -- secondary signal routing (escape routes, low-speed signals); Layer 6 (bottom) -- bump pads for connection to substrate. The ground planes between signal and power layers prevent switching noise on the power planes from coupling into signal traces. High-speed differential pairs are routed on the layers closest to the die to minimize trace length. Power planes are designed with sufficient copper width to handle the required current without excessive IR drop; current density in the RDL copper should stay below 1-2 MA/cm-squared for reliability (electromigration limit). Via placement must balance signal routing needs (vias consume routing area) with power delivery needs (many power vias reduce resistance). In multi-die packages, the RDL stack assignment may vary by region: under the die area, more layers are dedicated to die-to-die signals, while in the fan-out region, more layers handle escape routing and power distribution. This zone-based layer assignment is common in fan-out packages where the RDL simultaneously serves die-to-die, die-to-ball, and power distribution functions.

---

## Further Reading

- [Organic Substrates](organic_substrates.md)
- [Silicon Interposers](silicon_interposers.md)
- [Fan-Out Wafer-Level Packaging](../02_advanced_packaging/fan_out_wafer_level_packaging.md)
- [Signal Integrity in Packages](../05_electrical_performance/signal_integrity_in_packages.md)
