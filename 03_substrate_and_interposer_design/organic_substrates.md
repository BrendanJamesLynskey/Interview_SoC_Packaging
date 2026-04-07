# Organic Substrates

## Overview

Organic substrates are the most widely used package substrate technology, forming the foundation of BGA, CSP, and multi-chip packages. They provide signal routing, power distribution, and mechanical support between the die and the PCB.

---

### Q1. What is an organic package substrate and how is it constructed?

**Answer:**

An organic package substrate is a multi-layer circuit board made from alternating layers of copper traces and organic dielectric materials, similar in concept to a PCB but manufactured to much tighter tolerances and finer features. The construction starts with a rigid core (typically 0.1-0.8 mm thick), made from BT (bismaleimide triazine) resin or glass-reinforced epoxy. Build-up layers are added on both sides of the core using Ajinomoto Build-up Film (ABF) or similar materials. Each build-up layer consists of a dielectric film (15-35 micrometers thick) laminated onto the surface, microvias drilled by laser to connect to the layer below, and copper traces patterned by semi-additive processing (SAP) or modified SAP (mSAP). A typical high-performance substrate has a 6-2-6 configuration: 6 build-up layers on top, 2 core layers, and 6 build-up layers on the bottom, for a total of 14 metal layers. The top surface has pads for die attachment (flip-chip bump pads or wire bond pads), and the bottom surface has pads for BGA solder balls. The substrate also contains plated through-holes (PTH) through the core for connections between the top and bottom halves. Surface finishes include ENIG (electroless nickel immersion gold), ENEPIG (electroless nickel electroless palladium immersion gold), or OSP (organic solderability preservative), chosen based on the bump metallurgy and reliability requirements. Substrate fabrication is a complex, capital-intensive process with lead times of 8-16 weeks.

---

### Q2. What is ABF and why is it the dominant build-up dielectric?

**Answer:**

ABF (Ajinomoto Build-up Film) is a proprietary epoxy-based dielectric film manufactured by Ajinomoto Fine-Techno, used as the insulating layer between copper routing layers in build-up substrates. ABF dominates the advanced substrate market (estimated 80+ percent market share for high-performance substrates) due to several properties. It provides excellent planarity after lamination, which is critical for fine-line lithography on subsequent layers. Its dielectric constant (Dk) of approximately 3.2-3.5 at 1 GHz and loss tangent (Df) of 0.015-0.025 provide adequate electrical performance for most applications. The film can be laminated at relatively low temperature and pressure, which is compatible with existing substrate manufacturing equipment. ABF supports laser via drilling with clean via formation, and the cured film has good adhesion to copper. The film is supplied in roll form and laminated using vacuum lamination. ABF has evolved through multiple generations (GX-T, GZ, GX92, GX-92R, etc.) with improving properties: finer line/space capability (current generation supports 2/2 micrometer L/S), lower Dk and Df for better high-frequency performance, and improved dimensional stability for tighter registration. The main limitation of ABF is that its electrical loss is higher than low-loss PCB laminates or silicon dioxide, which becomes significant for very high-frequency applications (above 20 GHz). The global supply of ABF has been a bottleneck during periods of high demand for advanced substrates, as Ajinomoto is essentially a sole-source supplier. Alternative materials from other suppliers exist but have not achieved the same market share.

---

### Q3. What determines the line/space capability of a substrate?

**Answer:**

Line/space (L/S) refers to the minimum trace width and minimum gap between traces, expressed in micrometers. The L/S capability of a substrate is determined by the patterning process used to form the copper traces. Subtractive etching, the traditional method, starts with a continuous copper foil and etches away unwanted copper. This limits L/S to approximately 15/15 micrometers or coarser because the etch undercut ratio (lateral etch relative to vertical etch) prevents finer features. Semi-additive processing (SAP) starts with a thin copper seed layer, patterns photoresist to define trace locations, electroplates copper into the openings, strips the resist, and flash-etches the seed layer. SAP enables L/S down to approximately 5/5 micrometers because the copper is only deposited where needed, avoiding the undercut problem. Modified SAP (mSAP) is a variant that starts with a thin copper foil (2-5 micrometers) instead of a sputtered seed layer, offering a cost compromise between subtractive and full SAP, with L/S capability of approximately 8/8 to 12/12 micrometers. Advanced SAP with high-resolution photoresist and optimized plating chemistry can achieve 2/2 micrometer L/S, approaching silicon interposer capability. The practical L/S is also limited by the dielectric surface roughness (rough surfaces cause trace edge irregularity), registration accuracy between layers (misalignment between via and trace limits effective routing density), and manufacturing yield (finer features have higher defect rates, increasing cost). Current production substrates for high-end products (server processors, AI accelerators) use 8/8 to 5/5 micrometer L/S, while the most advanced substrates in development target 2/2 micrometers.

---

### Q4. What is the role of microvias in substrates and what types are used?

**Answer:**

Microvias are small holes (typically 50-75 micrometers in diameter at the top, 25-40 micrometers at the bottom) that connect adjacent metal layers in the build-up portion of the substrate. They are the primary vertical interconnect mechanism in build-up substrates and directly impact routing density, electrical performance, and reliability. Microvias are formed by laser drilling using CO2 lasers (for vias larger than 50 micrometers) or UV excimer/Nd:YAG lasers (for finer vias down to 25 micrometers). After drilling, the via is desmeared (cleaned of carbonized residue), metallized with electroless copper (seed layer), and filled with electroplated copper. Microvias can be stacked (directly on top of each other through multiple layers) or staggered (offset from layer to layer). Stacked microvias provide the highest routing density because they create a vertical column through multiple layers, minimizing the XY area consumed. However, stacked vias must be fully copper-filled (not just plated on the sidewalls) to provide a flat surface for the next via to land on, and the reliability of stacked vias is a concern because thermal cycling can cause cracking at the via-to-via interface. Current best practice limits stacked via height to 3-4 layers before requiring a transition to a through-core PTH or a stagger. IPC-6012 specifies reliability requirements for microvias, including resistance to thermal cycling and current capacity. Microvia reliability has improved significantly over the past decade, and 5-6 stacked microvias are now achievable in production for the most advanced substrates.

---

### Q5. How does substrate design affect signal integrity?

**Answer:**

The substrate is a significant contributor to package-level signal integrity because high-speed signals must traverse multiple centimeters of substrate traces between the die and the BGA balls. Key signal integrity factors include impedance control, which requires precise management of trace width, dielectric thickness, and dielectric constant to achieve 50-ohm single-ended or 100-ohm differential impedance. Typical tolerance is plus or minus 10 percent. At 5/5 micrometer L/S on 15-micrometer ABF dielectric, the trace geometry needed for 50 ohms is extremely narrow, making impedance control challenging. Dielectric loss (characterized by the loss tangent, Df) causes frequency-dependent signal attenuation. ABF with Df of 0.015-0.020 at 10 GHz causes approximately 1-2 dB/cm of insertion loss at 10 GHz, which is significant for multi-centimeter traces carrying 25+ Gbps signals. Low-loss ABF variants (Df below 0.010) are available for high-speed applications. Conductor loss from the resistivity of copper traces and skin effect (at high frequency, current flows only in a thin skin at the conductor surface) adds to insertion loss. Surface roughness of the copper-dielectric interface further increases conductor loss by 20-40 percent compared to smooth copper. Crosstalk between adjacent traces, caused by capacitive and inductive coupling, must be managed through spacing rules and ground shielding. Via discontinuities (impedance mismatch at via transitions) cause reflections that degrade return loss; anti-pad design and via stub removal (back-drilling) improve via performance. Substrate designers use 3D electromagnetic simulation (HFSS, CST, Clarity) to model and optimize the complete signal path from die bump to BGA ball.

---

### Q6. What is the difference between coreless and cored substrates?

**Answer:**

Cored substrates have a rigid central layer (the core) made from BT resin or glass-reinforced epoxy, typically 0.1-0.8 mm thick, with build-up layers on both sides. The core provides mechanical rigidity, dimensional stability during processing, and through-holes (PTH) for connections between the top and bottom build-up stacks. Cored substrates are the industry standard and are well understood in terms of manufacturing and reliability. Coreless substrates eliminate the central core entirely, consisting only of build-up layers formed sequentially on a temporary carrier that is removed after fabrication. Coreless substrates offer several advantages. The total substrate thickness is reduced (0.2-0.4 mm versus 0.5-1.2 mm for cored), which is valuable for thin packages like PoP and mobile applications. The elimination of PTH (which have larger diameter and pitch than microvias) improves routing density in the central layers. The thinner overall structure reduces the length of vertical interconnects, improving electrical performance. Signal propagation through the substrate is faster because there is no thick core dielectric to traverse. However, coreless substrates present challenges. Without a rigid core, the substrate is more flexible and prone to warpage during processing and reflow. Handling during manufacturing requires careful fixturing. The build-up process on a temporary carrier must maintain tight registration across all layers without the dimensional stability provided by a core. Coreless substrates are used in PoP packages for mobile processors (to minimize height), thin BGA packages for wearables, and some high-performance applications where the electrical advantages justify the manufacturing complexity.

---

### Q7. How is power delivery managed in the substrate?

**Answer:**

The substrate provides the power delivery network (PDN) between the BGA power/ground balls on the bottom and the die power/ground bumps on the top. Effective power delivery requires low DC resistance (to minimize IR drop), low AC impedance (to minimize voltage noise during transient current demand), and sufficient decoupling capacitance. DC resistance is minimized by using wide power planes (often entire metal layers dedicated to VDD or VSS) and multiple parallel vias connecting power planes across layers. A large processor substrate might dedicate 6-8 of its 14 metal layers primarily to power and ground planes. IR drop through the substrate must be kept below a few millivolts; at 100 A current draw, this requires milliohm-level total resistance. AC impedance is managed through the distributed capacitance of the power-ground plane pairs (which act as parallel-plate capacitors, providing decoupling in the GHz range) and discrete decoupling capacitors. Surface-mount decoupling capacitors (MLCC, typically 100 nF to 10 uF) are soldered to the substrate top or bottom surface, providing decoupling in the 10 MHz to 500 MHz range. Some advanced substrates embed thin-film capacitors within the build-up layers to provide additional decoupling without consuming surface area. The target impedance of the substrate PDN is typically 0.5-2 milliohms from DC to several GHz, calculated as the maximum allowed voltage noise divided by the maximum transient current. The substrate PDN is designed in conjunction with the die on-chip PDN and the board-level PDN, using frequency-domain impedance analysis to ensure that no impedance peaks exceed the target across the entire frequency range.

---

### Q8. What are glass substrates and why are they being developed?

**Answer:**

Glass substrates use a glass core instead of the traditional BT resin or organic laminate core. Glass (typically borosilicate or alkali-free display glass) offers several properties that are superior to organic cores for advanced packaging. Dimensional stability is the primary advantage: glass has a CTE of 3-4 ppm/K (close to silicon at 2.6 ppm/K), compared to 12-15 ppm/K for organic core materials. This close CTE match to silicon dramatically reduces warpage during thermal processing and improves layer-to-layer registration, enabling finer features. The better registration allows tighter line/space (potentially 2/2 micrometers or below) and finer via pitch on glass substrates compared to organic substrates of similar layer count. Glass has very low dielectric loss (Df below 0.005 at 10 GHz), superior to ABF, which benefits high-frequency signal integrity. Glass panels are available in large sizes (similar to display glass), potentially enabling panel-level substrate fabrication at lower cost than wafer-level processing. Through-glass vias (TGV) can be formed by laser drilling or photochemical etching, filled with copper, and planarized. The challenges of glass substrates include brittleness (glass is fragile and prone to cracking, especially at edges and via locations), the need for new manufacturing processes and equipment (existing substrate factories are optimized for organic materials), and the current lack of high-volume production infrastructure. Intel, Samsung, and several substrate manufacturers have announced glass substrate development programs targeting production readiness in the 2025-2027 timeframe. Glass substrates are expected to first appear in high-end server and AI accelerator packages, where the performance and routing density benefits justify the higher initial cost.

---

### Q9. How does substrate warpage affect package assembly and reliability?

**Answer:**

Substrate warpage is the deviation of the substrate surface from a flat plane, caused by CTE mismatch between the copper traces, dielectric layers, and core material during thermal processing. Warpage is typically specified as the maximum out-of-plane displacement across the substrate surface at a given temperature. For a 50 mm x 50 mm substrate, typical warpage specifications are 50-150 micrometers at room temperature and must be controlled through the reflow temperature profile (peak at 240-260 degrees Celsius). Warpage affects package assembly because the die attachment process (flip-chip or wire bond) requires a sufficiently flat substrate surface. If the substrate is excessively warped at the die attach temperature, the bump contacts at the die corners may not engage, causing open connections (non-wet opens). For flip-chip packages with thousands of bumps at fine pitch, the acceptable warpage during reflow is typically less than 50-75 micrometers across the die area. Similarly, at the board-level SMT reflow, the package must be flat enough for all BGA solder balls to contact the PCB pads simultaneously. Excessive warpage causes opens at the corners (if the package is convex, "smiling" shape) or at the center (if concave, "crying" shape). Warpage is managed through symmetric substrate design (equal layer count and copper density on top and bottom), material selection (matched CTE build-up materials), core thickness optimization, and copper balance (adjusting copper pattern density per layer to balance internal stress). Simulation tools (ANSYS, Abaqus) predict warpage versus temperature profiles, and the design is iterated until the warpage at critical temperatures (die attach, BGA reflow, operating temperature) meets specifications.

---

### Q10. What are the current supply chain challenges for advanced substrates?

**Answer:**

The advanced substrate supply chain has experienced significant challenges since 2020, driven by surging demand from AI accelerators, high-performance computing, and 5G infrastructure. ABF supply constraints were among the most publicized: Ajinomoto's near-monopoly on ABF film meant that any increase in demand for advanced substrates directly strained ABF availability. Lead times for ABF substrates extended from the typical 8-12 weeks to 30-50 weeks during peak demand periods. Geographic concentration is another risk: the majority of advanced substrates are manufactured in Japan (Ibiden, Shinko), South Korea (Samsung Electro-Mechanics, Daeduck), and Taiwan (Unimicron, ASE/Kinsus). This concentration creates geopolitical and natural disaster risks. The capital intensity of substrate manufacturing (a new advanced substrate factory costs $1-3 billion) creates barriers to rapid capacity expansion. The technology complexity limits the number of suppliers capable of producing the most advanced substrates (5/5 micrometer L/S, 14+ layers, bridge cavities): only a handful of companies worldwide can manufacture these. Cost escalation has been significant: advanced substrates for AI accelerators can cost $50-100 or more per unit, representing a growing fraction of total package cost. The industry is responding with capacity expansion (multiple new substrate factories announced in Japan, South Korea, and Southeast Asia), diversification of dielectric materials (alternatives to ABF from Mitsubishi Gas Chemical, Sekisui Chemical, and others), and development of glass substrates and panel-level processing to reduce cost and expand capacity. Despite these efforts, substrate supply is expected to remain tight for high-end applications through at least 2027.

---

## Further Reading

- [Silicon Interposers](silicon_interposers.md)
- [RDL and Routing](rdl_and_routing.md)
- [Signal Integrity in Packages](../05_electrical_performance/signal_integrity_in_packages.md)
- [Warpage and Stress](../04_thermal_and_mechanical/warpage_and_stress.md)
