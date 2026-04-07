# Chiplet Architectures

## Overview

Chiplet architectures disaggregate a monolithic SoC into multiple smaller die (chiplets) that are interconnected within a package. This approach offers economic, yield, and design flexibility advantages that have made it a dominant strategy for high-performance processors, GPUs, and networking ASICs.

---

### Q1. What is a chiplet and why are chiplet architectures used?

**Answer:**

A chiplet is a small, functionally discrete silicon die designed to be integrated with other chiplets in a multi-die package to collectively deliver the functionality of a system. Rather than fabricating all functions on a single monolithic die, chiplet architectures partition the design into separate die that can be manufactured independently and combined at the packaging level. The motivations are primarily economic and practical. Yield economics provide the strongest driver: die yield decreases exponentially with die area at a given defect density. A monolithic 800 mm-squared die at a defect density of 0.1 defects/cm-squared has a yield of approximately 45 percent, while four 200 mm-squared chiplets at the same defect density each yield approximately 82 percent. Even accounting for assembly yield, the chiplet approach produces more working units per wafer. Process node optimization allows each chiplet to be fabricated at the most cost-effective node for its function: high-performance compute logic at 3-5 nm, I/O and SerDes at 5-7 nm (where analog circuits are better characterized), and HBM DRAM at its own specialized process. Design reuse enables a library of validated chiplet building blocks that can be combined in different configurations to address multiple market segments without full redesign. Time to market improves because chiplets can be designed and validated in parallel by different teams. Supply chain flexibility allows multi-sourcing of individual chiplets.

---

### Q2. How does AMD's chiplet architecture work in EPYC processors?

**Answer:**

AMD's EPYC server processors exemplify chiplet architecture at scale. Starting with the Zen 2 generation (Rome, 2019), AMD separated the CPU cores from the I/O functions into distinct chiplets. The compute core die (CCDs) contain 8 CPU cores each and are fabricated at the leading-edge node (7 nm for Zen 2, 5 nm for Zen 4). The I/O die (IOD) integrates the memory controllers (DDR), PCIe controllers, Infinity Fabric interconnect, and other I/O functions, fabricated at a mature node (14 nm for Zen 2, 6 nm for Zen 4). In the EPYC 9004 (Genoa) processor, up to 12 CCDs surround a central IOD, all assembled on an organic package substrate using flip-chip bumps. The CCDs communicate with the IOD through AMD's Infinity Fabric, a coherent on-package interconnect. This architecture enables AMD to build processors with 16 to 96 cores by varying the number of CCDs populated on the package. Partially functional CCDs (with some defective cores disabled) can still be used, improving effective yield. The Zen 4 EPYC (Genoa) package measures approximately 75 mm x 58 mm and contains up to 13 die. With the Zen 5 generation (Turin), AMD introduced 3D V-Cache variants where additional SRAM cache chiplets are hybrid-bonded on top of the CCDs, adding another dimension of integration. This multi-generation evolution demonstrates how chiplet architecture allows AMD to improve products by upgrading individual chiplets rather than redesigning the entire processor.

---

### Q3. What is UCIe and why is it important?

**Answer:**

Universal Chiplet Interconnect Express (UCIe) is an open industry standard for die-to-die interconnect, published by the UCIe Consortium (founded in 2022 by Intel, AMD, ARM, TSMC, Samsung, and others). UCIe defines the physical layer, protocol layer, and software stack for chiplet communication, aiming to enable interoperability between chiplets from different vendors, much as PCIe standardized board-level interconnect. The physical layer specifies the bump pitch (standard package: 100 micrometer pitch at 25 Gbps/lane; advanced package: 25 micrometer pitch at 32+ Gbps/lane), signaling (NRZ and PAM4 options), channel reach (less than 2 mm for advanced, up to 25 mm for standard), and raw bandwidth density (up to 317 GB/s/mm for advanced package at 32 Gbps). The protocol layer supports multiple upper-layer protocols including CXL (for memory coherency), PCIe (for I/O), and streaming protocols. UCIe 1.0 defines two packaging tiers: standard package (organic substrate, 100-130 micrometer bump pitch) delivering up to 28 GB/s/mm bandwidth density, and advanced package (silicon interposer or bridge, 25-55 micrometer bump pitch) delivering up to 317 GB/s/mm. UCIe is important because it addresses the key barrier to chiplet adoption: without a standard interface, chiplets from one vendor cannot interoperate with chiplets from another, limiting the ecosystem. With UCIe, a company could mix and match compute chiplets, I/O chiplets, and memory chiplets from different suppliers. Adoption is still early, but the breadth of the founding consortium suggests UCIe will become the de facto standard for die-to-die communication.

---

### Q4. How does Intel's Ponte Vecchio GPU use chiplet packaging?

**Answer:**

Intel's Ponte Vecchio (PVC) is one of the most complex chiplet-based products ever built, integrating up to 47 active die (tiles in Intel's terminology) in a single package using multiple packaging technologies simultaneously. The PVC package contains compute tiles (8 Xe-HPC GPU tiles fabricated at Intel 7 process), base tiles (2 tiles at Intel 7 acting as the central interconnect fabric), HBM2e memory stacks (8 stacks with 16 DRAM die each), Rambo cache tiles (multiple SRAM tiles), and I/O tiles. These are assembled using a combination of EMIB bridges (connecting adjacent tiles on the organic substrate with fine-pitch interconnect), Foveros 3D stacking (compute tiles are 3D-bonded face-to-face on top of base tiles using hybrid bonding), and standard flip-chip (tiles to organic substrate). The base tiles serve as the interconnect backbone, containing the coherence fabric and connecting to HBM via EMIB bridges. The Foveros 3D stacking of compute tiles on base tiles provides the highest bandwidth for the most critical interconnect (compute to fabric). The total package contains over 100 billion transistors across all tiles, uses five different process nodes (Intel 7, Intel 4, TSMC N5, and specialized processes for EMIB bridges and HBM), and delivers over 2 TB/s of memory bandwidth. Ponte Vecchio demonstrates the ultimate vision of chiplet architecture: mixing process nodes, packaging technologies, and die from different foundries in a single coherent product. It also illustrates the challenges -- the product was delayed and is extremely expensive to manufacture, highlighting the yield and test complexity of high-tile-count assemblies.

---

### Q5. What are the key die-to-die interface design challenges?

**Answer:**

Die-to-die interfaces in chiplet packages face unique design challenges compared to board-level interfaces. First, the channel is very short (0.5-15 mm depending on packaging technology), which means the channel is not transmission-line dominated; instead, the parasitics of the bumps and pads dominate the electrical behavior. This requires different equalization strategies than long-reach SerDes: simple analog circuits suffice, and forwarded clock architectures are preferred over CDR (clock data recovery) to minimize latency and power. Second, the interconnect density must be extremely high to provide sufficient bandwidth between chiplets. A 512-bit wide parallel bus at 16 Gbps per pin delivers 1 TB/s, but requires 512 signal pairs plus ground and power -- thousands of bumps in total. Routing this many signals in the available die-edge interface area requires careful bump map and RDL planning. Third, latency must be minimized, especially for coherent protocols where cache-line transfers between chiplets must complete within a few nanoseconds to avoid stalling the processor pipeline. The physical latency through bumps and short traces is under 1 ns, but protocol overhead (serialization, error correction, arbitration) adds several nanoseconds. Fourth, power efficiency must be very high: die-to-die links target 0.5-2 pJ/bit, compared to 5-15 pJ/bit for board-level SerDes. This is achieved by using low-swing signaling, minimal equalization, and simple circuit topologies. Fifth, testing die-to-die interfaces before assembly is difficult because the channel does not exist until the die are packaged together. Built-in self-test (BIST), including loopback modes and per-lane PRBS testing, is essential.

---

### Q6. What is heterogeneous integration and how does it differ from homogeneous chiplet designs?

**Answer:**

Heterogeneous integration combines die fabricated using fundamentally different technologies, process nodes, or materials into a single package to create a system with capabilities that no single die technology could achieve. This contrasts with homogeneous chiplet designs, where identical chiplets (same function, same process) are replicated for scalability. AMD's EPYC with multiple identical CCDs is primarily a homogeneous chiplet design (the IOD is a different die, adding some heterogeneity). A fully heterogeneous integration might combine a 3 nm CMOS logic die, a 22 nm analog/mixed-signal die, a GaN RF power amplifier, a SiGe BiCMOS transceiver, an MEMS sensor, and HBM DRAM -- all in one package. Each technology is optimized for its function without compromise. Heterogeneous integration is particularly valuable in applications where diverse functionality is needed: 5G base station (digital baseband + RF front-end + power amplifier), automotive ADAS (processor + radar + sensor fusion), and biomedical devices (sensor + processor + wireless). The packaging challenges for heterogeneous integration are more complex than for homogeneous designs. Different die may have different bump pitches, requiring multiple bonding steps or a substrate that accommodates mixed pitch. Thermal management must handle die with vastly different power densities. Different die may have different voltage and power delivery requirements. Testing requires different ATE configurations for different die technologies. Supply chain coordination between multiple foundries and die providers adds logistical complexity. Despite these challenges, heterogeneous integration is considered the most promising path for continued system performance improvement as monolithic scaling slows.

---

### Q7. What bandwidth density can different die-to-die interconnect technologies achieve?

**Answer:**

Bandwidth density is the data throughput per unit length of die edge (typically expressed in GB/s/mm) and is the key metric for comparing die-to-die interconnect approaches. Standard organic substrate routing with C4 bumps at 130 micrometer pitch, running at 8-16 Gbps per lane with NRZ signaling, achieves approximately 5-15 GB/s/mm of die edge. This is sufficient for low-bandwidth die-to-die links but inadequate for compute-to-memory or compute-to-compute interfaces in HPC. EMIB or silicon bridge interconnects with microbumps at 55 micrometer pitch, running at 16-32 Gbps per lane, achieve approximately 30-100 GB/s/mm. This is the range used by Intel EMIB-connected tiles and is suitable for many chiplet applications. Silicon interposer (CoWoS) with microbumps at 40 micrometer pitch and 16 Gbps signaling achieves approximately 100-200 GB/s/mm. CoWoS-based HBM connections typically provide 200-400 GB/s per HBM stack through a die edge of approximately 8-10 mm. UCIe advanced package specification targets up to 317 GB/s/mm at 25 micrometer pitch with 32 Gbps/lane. Hybrid bonding at 9 micrometer pitch with 4-8 Gbps per connection can theoretically achieve over 1 TB/s/mm, far exceeding any bump-based technology. However, hybrid bonding currently requires face-to-face orientation and is limited to two die. For context, an on-chip BEOL interconnect at the global metal layer achieves approximately 10-100 TB/s/mm but at much shorter distances. The packaging industry's roadmap aims to close this gap between on-chip and off-chip bandwidth density through continued pitch scaling and signaling rate improvements.

---

### Q8. How do chiplet architectures impact power delivery design?

**Answer:**

Chiplet architectures create new power delivery challenges compared to monolithic die. Each chiplet may operate at a different voltage domain: compute chiplets at 0.6-0.9 V, I/O chiplets at 0.75-1.2 V, and SerDes at 0.9-1.0 V. The package substrate must route separate power planes or power rails for each voltage domain, increasing substrate layer count and routing complexity. Current delivery requirements scale with the number of chiplets: a package with eight compute chiplets, each drawing 30 A, requires 240 A total current delivery through the package substrate, demanding wide low-resistance power planes and many power/ground bumps. The die-to-die interfaces themselves consume significant power: at 1 pJ/bit across 2 TB/s of aggregate inter-chiplet bandwidth, the die-to-die links consume 2 W -- comparable to a small chiplet's entire power budget. For 2.5D packages with silicon interposers, the power delivery path is longer (through the organic substrate, then through the interposer TSVs to each die), adding resistance and inductance. Interposer TSVs dedicated to power delivery must be numerous and low-resistance; a typical interposer might dedicate 30-50 percent of TSVs to power and ground. Decoupling must be distributed across the package to suppress impedance peaks in the 100 MHz to 1 GHz range. On-package capacitors (silicon trench capacitors in the interposer, or embedded capacitors in the substrate) supplement the on-die decoupling. Integrated voltage regulators (IVRs) on individual chiplets can improve transient response and enable per-chiplet power management, but at the cost of die area and efficiency loss. The package PDN is typically co-designed with the chiplet architecture using impedance simulation tools to ensure that target impedance is met across the frequency spectrum.

---

### Q9. What are the test and debug challenges for chiplet-based products?

**Answer:**

Testing and debugging chiplet-based products is significantly more complex than testing monolithic die because the test must cover individual chiplet functionality, die-to-die interconnect integrity, and system-level interaction between chiplets. At the individual chiplet level, KGD testing at the wafer level must achieve near-zero DPPM escape rates because a single defective chiplet scrap the entire expensive assembly. This requires comprehensive scan testing, BIST for memories and PLLs, parametric testing, and ideally at-speed functional testing -- all through a temporary probe contact rather than a permanent package connection. Die-to-die interface testing is particularly challenging because the interface does not exist until assembly is complete. Pre-assembly, each chiplet can only test its own half of the interface (transmitter output, receiver input) in loopback mode. Post-assembly, BIST engines on each side of the die-to-die link run training sequences to establish communication, then execute pattern-based testing (PRBS, walking ones) and margin testing. Built-in eye-diagram monitoring and bit-error-rate counters help diagnose link quality. System-level testing must verify that the chiplets cooperate correctly: coherence protocols, memory access across chiplets, and workload performance. Debug is complicated by limited visibility into die-to-die signaling (no external probe point), the need for cross-chiplet scan chain stitching, and the challenge of reproducing timing-dependent inter-chiplet bugs. Design-for-test (DFT) features must be planned from the chiplet architecture definition phase, including IEEE 1838 (die-to-die test access standard), boundary scan across die-to-die interfaces, and dedicated test modes for each chiplet.

---

### Q10. What is the future roadmap for chiplet technology?

**Answer:**

The chiplet roadmap over the next five to ten years involves advances along multiple dimensions. Interconnect density will continue to scale: UCIe 2.0 and beyond will target finer bump pitches (below 25 micrometers) and higher signaling rates (64 Gbps and beyond), aiming for bandwidth densities exceeding 1 TB/s/mm. Hybrid bonding will expand from specialized 3D stacking (V-Cache, image sensors) to general-purpose chiplet-to-chiplet bonding, potentially enabling heterogeneous face-to-face integration. Chiplet standardization will mature, with UCIe enabling a marketplace where chiplets from different vendors can be mixed and matched, similar to how standard DRAM modules work today. Photonic chiplets integrating silicon photonics will enable co-packaged optics, providing terabits per second of optical bandwidth directly from the package for data center and AI training applications. Glass substrates will replace organic substrates for advanced chiplet packages, offering better dimensional stability, finer RDL, lower warpage, and higher routing density. Active interposers (silicon interposers with embedded logic, power conversion, or caching) may add functionality to the interconnect layer itself. Chiplet disaggregation will extend beyond processors to accelerators, networking, and even consumer products as costs decrease. System-level design tools will evolve to support chiplet architecture exploration, including automated partitioning of a design into optimal chiplets based on yield, cost, and performance tradeoffs. The semiconductor industry views chiplet architecture as the primary path to continued system scaling beyond the limits of monolithic Moore's Law.

---

## Further Reading

- [2.5D and 3D Packaging](2_5d_and_3d_packaging.md)
- [Fan-Out Wafer-Level Packaging](fan_out_wafer_level_packaging.md)
- [Signal Integrity in Packages](../05_electrical_performance/signal_integrity_in_packages.md)
- [Package Testing](../06_manufacturing_and_test/package_testing.md)
