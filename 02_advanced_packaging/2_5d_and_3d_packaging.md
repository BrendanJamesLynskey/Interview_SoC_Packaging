# 2.5D and 3D Packaging

## Overview

2.5D and 3D packaging technologies enable the integration of multiple die with high bandwidth and low latency interconnects that far exceed what traditional package substrates can provide. These approaches are essential for high-performance computing, AI accelerators, and advanced networking devices.

---

### Q1. What is 2.5D packaging and how does it work?

**Answer:**

2.5D packaging places multiple die side by side on a shared silicon interposer, which sits between the die and the organic package substrate. The silicon interposer is a thin piece of silicon (typically 50-100 micrometers thick after thinning) fabricated with through-silicon vias (TSVs), fine-pitch redistribution layers (RDL), and microbump pads. Die are flip-chip bonded face-down onto the interposer using microbumps at 40-55 micrometer pitch. The interposer provides ultra-fine wiring (line/space of 0.4/0.4 to 2/2 micrometers) that connects the die laterally with very short trace lengths and high wire density. The interposer itself is then flip-chip bonded to the organic package substrate using standard C4 bumps at 100-150 micrometer pitch. TSVs through the interposer provide vertical electrical connections from the top-side microbumps to the bottom-side C4 bumps. TSMC's CoWoS (Chip-on-Wafer-on-Substrate) is the most widely deployed 2.5D platform, used in products such as AMD Instinct MI300, NVIDIA H100/H200 GPUs (which integrate the GPU die with HBM memory stacks), and Xilinx/AMD Versal FPGAs. The key advantage of 2.5D is the ability to connect heterogeneous die (logic, memory, I/O) with bandwidth densities of 100-500 GB/s/mm of die edge, far exceeding what organic substrate routing (at 8-15 micrometer line/space) can achieve. The primary disadvantages are cost (the large silicon interposer is expensive to fabricate) and yield risk (a defect on the interposer can affect multiple good die).

---

### Q2. What are through-silicon vias (TSVs) and what are their key parameters?

**Answer:**

Through-silicon vias (TSVs) are vertical copper-filled holes etched through a silicon wafer that provide electrical connections from one side of the silicon to the other. TSVs are the enabling technology for both 2.5D interposers and 3D stacked die. The key parameters of a TSV include diameter (typically 5-10 micrometers for high-density applications, up to 50 micrometers for power delivery), depth (50-100 micrometers for interposers, 30-50 micrometers for thinned 3D stacked die), aspect ratio (depth-to-diameter ratio, typically 5:1 to 10:1), and pitch (the center-to-center spacing, typically 2-10 times the diameter). The fabrication process involves deep reactive ion etching (DRIE, also known as the Bosch process) to create high-aspect-ratio holes, dielectric liner deposition (SiO2, typically 100-500 nm) to isolate the copper from the silicon, barrier/seed layer deposition (Ta/TaN + Cu seed by PVD), copper electroplating to fill the via, and CMP to planarize the surface. TSVs are categorized by when they are formed relative to BEOL processing: via-first (before BEOL, used by some DRAM manufacturers), via-middle (after transistor formation but before BEOL metallization, used by TSMC and Intel), and via-last (after BEOL completion, formed from the wafer backside). Via-middle is the most common approach for logic interposers because it integrates well with standard CMOS processing. TSV electrical characteristics include resistance of 10-100 milliohms, inductance of 5-30 pH, and capacitance of 20-50 fF (dominated by the MOS capacitor formed between copper, oxide liner, and silicon).

---

### Q3. What is Intel's EMIB technology and how does it compare to a full silicon interposer?

**Answer:**

Intel's Embedded Multi-die Interconnect Bridge (EMIB) is a localized interconnect technology that embeds small silicon bridge die within cavities in the organic package substrate. Instead of using a large silicon interposer spanning the entire package, EMIB places small bridge chips (approximately 8 mm x 4 mm) only at the locations where two adjacent die need high-density interconnection. The bridge die contains fine-pitch RDL (2-4 micrometer line/space) and microbump pads on its top surface, but no TSVs -- the bridge is thin enough that connections are made only on the top surface. Die are bonded to both the substrate and the bridge simultaneously during assembly. EMIB offers several advantages over full silicon interposers. Cost is significantly lower because the bridge is much smaller than a full interposer (perhaps 30 mm-squared versus 2000+ mm-squared), reducing silicon area and eliminating the need for TSVs. The organic substrate provides the global routing and power delivery, which it does well and inexpensively, while the bridge provides localized high-density interconnect only where needed. Package size is not limited by interposer reticle size (a major constraint for full interposers). Disadvantages include the complexity of embedding bridges in the substrate (cavity formation, bridge placement accuracy, co-planarity with the substrate surface), potentially higher substrate cost, and the limitation that die must be placed close together for the bridge to span the gap (typically less than 4 mm die edge-to-edge spacing). Intel has used EMIB in products including Stratix 10 FPGAs, Ponte Vecchio GPU, and Sapphire Rapids with HBM.

---

### Q4. What is 3D packaging and what are the main approaches?

**Answer:**

3D packaging stacks die vertically, one on top of another, with direct electrical connections between the stacked layers. Unlike 2.5D (side-by-side on an interposer), 3D stacking shortens the interconnect distance to the die thickness (tens of micrometers) rather than the lateral die spacing (millimeters). The main approaches to 3D packaging are microbump-based stacking, hybrid bonding, and through-silicon via stacking. In microbump-based 3D, the top die is thinned to 30-50 micrometers and has TSVs connecting its BEOL layers to microbump pads on the backside. The thinned die is bonded face-up (backside down) onto the bottom die using thermocompression bonding, with microbumps at 40-55 micrometer pitch providing the electrical connection. This approach is used extensively for HBM (High Bandwidth Memory), where 4-12 DRAM die are stacked using TSVs and microbumps on a base logic die. In hybrid bonding-based 3D, die are bonded face-to-face using direct copper-to-copper and oxide-to-oxide bonding at pitches below 10 micrometers (down to sub-1 micrometer). This enables dramatically higher bandwidth density and is used in AMD 3D V-Cache and Sony CMOS image sensors. Emerging approaches include sequential 3D integration, where a second transistor layer is fabricated directly on top of the first (monolithic 3D), but this remains in research. 3D stacking challenges include thermal management (heat from upper die must pass through lower die), testing of thinned die before stacking (KGD), and yield compound loss (stacking N die with yield Y each gives combined yield Y^N).

---

### Q5. How does HBM (High Bandwidth Memory) use 2.5D and 3D packaging?

**Answer:**

HBM is a high-bandwidth DRAM standard that combines 3D die stacking (within the HBM stack) with 2.5D integration (HBM stack to logic die). An HBM stack consists of 4 to 12 DRAM die stacked vertically on a base logic die (buffer die) using TSVs and microbumps. Each DRAM die is thinned to approximately 30-40 micrometers and has thousands of TSVs connecting through the die. The base die interfaces with the logic chip through the silicon interposer. The vertical stack provides massive parallel data bus width: HBM3 uses a 1024-bit wide interface per stack, and HBM3E increases this further. Each HBM3 stack provides 256-512 GB/s of bandwidth, and a package with four to six stacks can deliver 2-5 TB/s of aggregate memory bandwidth. The HBM stacks and the logic die (GPU, AI accelerator, or network processor) are mounted side-by-side on a 2.5D silicon interposer, connected through the interposer's fine-pitch wiring. The interposer provides thousands of signal connections between the logic die and each HBM stack with short trace lengths (typically 5-15 mm), enabling operation at high data rates (up to 9.6 Gbps per pin for HBM3E) with low power consumption (approximately 3-5 pJ/bit). HBM has become the dominant memory solution for AI training accelerators (NVIDIA H100/H200/B200, AMD MI300X) because the bandwidth density is 5-10 times greater than GDDR6 while consuming significantly less power per bit. The cost per bit is higher than GDDR, but the performance-per-watt advantage justifies the premium in data center applications.

---

### Q6. What is the difference between face-to-face and face-to-back 3D stacking?

**Answer:**

Face-to-face (F2F) stacking bonds two die with their active (BEOL) surfaces facing each other. The top die is flipped so that the metal layers of both die are in direct proximity, connected through microbumps or hybrid bonds. This orientation provides the shortest possible interconnect path between the two die because signals do not need to traverse through the silicon substrate or TSVs. The interconnect pitch can be as fine as the back-end metal pitch (sub-1 micrometer for hybrid bonding). F2F stacking is limited to two die and requires that at least one die has its backside exposed for heat extraction and external connections (via TSVs through the bottom die). Applications include CMOS image sensors (photodiode array face-to-face with readout logic), AMD 3D V-Cache (SRAM cache face-to-face with processor), and high-performance logic-on-logic stacking. Face-to-back (F2B) stacking bonds the active surface of the top die to the backside (silicon surface) of the bottom die. This requires TSVs through the bottom die to connect its BEOL layers to the bonding surface on its backside. F2B enables stacking of more than two die (N-high stacking) because each die in the stack has the same orientation. HBM memory uses F2B stacking for the DRAM die above the base die. The interconnect pitch is limited by the TSV pitch (typically 40-55 micrometers for microbump-based F2B). F2B adds the thermal resistance of the silicon substrate between the active layers of adjacent die, which is a disadvantage for high-power stacks. The choice between F2F and F2B depends on the number of stacking layers needed, the required interconnect density, and thermal constraints.

---

### Q7. What are the yield challenges in multi-die packaging?

**Answer:**

Yield is one of the most critical challenges in 2.5D and 3D packaging because the compound yield effect multiplies losses across all components. If a package contains N die, each with individual yield Y, and the assembly yield is Y_asm, the package yield is approximately Y^N times Y_asm (assuming independent failure modes). For example, four die each at 95 percent yield with 98 percent assembly yield gives a package yield of 0.95^4 times 0.98 = 0.795, meaning 20 percent scrap even with individually high-yield components. For expensive logic die, this yield loss is extremely costly. The primary mitigation strategy is Known Good Die (KGD): each die is fully tested before assembly to ensure only functional die enter the packaging process. However, testing bare die (unpackaged wafer-level testing) is inherently more difficult than testing packaged parts due to probe contact challenges at fine pad pitch, inability to perform burn-in at the wafer level (for most processes), and the impossibility of testing high-speed I/O without the package environment. Interposer yield is another concern: a large interposer (e.g., 2500 mm-squared) has significant defect exposure. Die-level redundancy (spare TSVs, spare I/O lanes) can mitigate some yield loss. Chiplet architectures partially address yield by using smaller die (which have higher individual yield) and by enabling redundancy at the chiplet level. For example, AMD's EPYC processors can disable defective core chiplets and sell the product with reduced core count rather than scrapping the entire assembly. Future improvements in test methodology, including built-in self-test (BIST) for die-to-die interfaces, will be essential for scaling multi-die packaging to higher die counts.

---

### Q8. What thermal challenges arise in 3D stacking?

**Answer:**

3D stacking creates unique thermal challenges because stacked die generate heat in multiple vertically separated layers, and the upper die are thermally insulated from the heat sink by the lower die. In a conventional 2D package, heat flows vertically from a single die through the lid to the heat sink. In a 3D stack, the heat from upper die must flow down through the intervening die, adhesive layers, and bump arrays to reach the bottom of the stack and ultimately the heat sink. Each die layer adds thermal resistance: approximately 0.02-0.05 degrees C/W for the silicon itself plus 0.05-0.2 degrees C/W for the microbump and underfill interface between layers. For an 8-high HBM stack dissipating 10-15 W total, the top DRAM die can be 10-20 degrees Celsius hotter than the bottom die. For high-power logic-on-logic stacking, the thermal challenge is more severe. If each logic die dissipates 50-100 W, the temperature gradient through the stack can exceed safe operating limits. Potential solutions include through-silicon thermal vias (large copper-filled TSVs dedicated to heat conduction rather than electrical signaling), microfluidic cooling channels etched between stacked die, thermally conductive underfill materials, thinning die to reduce the thermal path length, and careful power management to avoid simultaneous peak power on all layers. The choice of stacking orientation also matters: F2F stacking places the active layers of both die closest together, concentrating heat at the bonding interface. Thermal simulation using finite element analysis (FEA) is essential for 3D stack design, and thermal constraints often limit the number of die that can be stacked and the power per die.

---

### Q9. How does TSMC's CoWoS platform work?

**Answer:**

TSMC's Chip-on-Wafer-on-Substrate (CoWoS) is the most commercially successful 2.5D packaging platform. CoWoS integrates multiple die on a silicon interposer that is subsequently packaged on an organic substrate. The process flow involves several major steps. First, the silicon interposer wafer is fabricated with TSVs, BEOL metallization (fine-pitch RDL), and microbump pads on the top surface and C4 bump pads on the bottom surface. The interposer is fabricated at TSMC's wafer fabs using a specialized process (not a standard logic node, but using similar lithographic tools for fine RDL patterning). Second, the die (compute die, HBM stacks) are flip-chip bonded onto the interposer wafer (chip-on-wafer step) using thermocompression bonding. Underfill is applied. Third, the interposer wafer with bonded die is thinned from the backside (revealing the TSVs), and C4 bumps are formed on the backside. Fourth, the interposer is singulated and flip-chip bonded to an organic BGA substrate (wafer-on-substrate step). CoWoS has evolved through several generations: CoWoS-S (standard, silicon interposer), CoWoS-R (RDL interposer using organic-like materials instead of silicon, lower cost), and CoWoS-L (local silicon interconnect bridges embedded in an organic interposer, similar in concept to EMIB). CoWoS-S supports interposer sizes up to approximately 2500 mm-squared (2x reticle stitching) in CoWoS-S with Gen5 expansion to even larger sizes. The platform is used in NVIDIA A100, H100, H200, and B200 GPUs, AMD MI250X and MI300 accelerators, and Broadcom networking ASICs.

---

### Q10. What is the role of known good die (KGD) testing in advanced packaging?

**Answer:**

Known Good Die (KGD) testing ensures that only fully functional die are assembled into expensive multi-die packages, where scrapping the package due to a single defective die results in significant cost loss. KGD testing involves wafer-level probe testing of every die, typically on automatic test equipment (ATE) using fine-pitch probe cards. The goal is to replicate as much of the final package test as possible while the die are still on the wafer. This includes parametric testing (leakage currents, threshold voltages, supply current), functional testing (scan-based stuck-at and transition fault tests, BIST for memories and PLLs), and in some cases, at-speed testing of critical paths. The challenge is that certain tests are difficult or impossible at the wafer level. High-speed I/O testing (SerDes, DDR) requires impedance-matched probing and is limited by probe card parasitics. Burn-in (elevated voltage and temperature stress) is impractical on a probe station for most products. Some defects (marginal timing, thermal-dependent failures) may only manifest under package-level conditions. To mitigate residual defects, advanced packaging flows often include post-assembly testing and die-level redundancy. For HBM, DRAM die are individually tested and burned in before stacking. For logic chiplets, comprehensive at-speed scan testing at the wafer level identifies most defects. The cost of KGD testing (probe card, test time, yield loss from false rejects) is justified by the much higher cost of scrapping a multi-die assembly. As die counts per package increase and package costs rise (a large AI accelerator package can cost thousands of dollars), KGD quality requirements become increasingly stringent, typically targeting defect levels below 100 DPPM (defective parts per million).

---

## Further Reading

- [Chiplet Architectures](chiplet_architectures.md)
- [Silicon Interposers](../03_substrate_and_interposer_design/silicon_interposers.md)
- [Die-to-Package Interconnect](../01_foundations/die_to_package_interconnect.md)
- [Thermal Management](../04_thermal_and_mechanical/thermal_management.md)
