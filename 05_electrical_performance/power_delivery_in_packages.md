# Power Delivery in Packages

## Overview

The package power delivery network (PDN) supplies clean, stable voltage from the board to the die. As core voltages decrease and current demands increase, the package PDN becomes a critical design challenge.

---

### Q1. What is the target impedance concept for package PDN design?

**Answer:**

Target impedance is the maximum allowable impedance of the power delivery network across the frequency spectrum, calculated from the voltage tolerance and maximum transient current: Z_target = Delta_V_max / Delta_I_max. For example, a 0.8 V supply with 3 percent voltage tolerance and 200 A maximum transient current: Z_target = (0.03 * 0.8) / 200 = 0.12 milliohms. The PDN must maintain impedance below this target from DC through several GHz. Different components of the PDN are responsible for different frequency ranges. The voltage regulator module (VRM) maintains impedance from DC to approximately 1-10 kHz. Board-level bulk capacitors (10-100 uF) cover 10 kHz to 1 MHz. Board-level MLCC decoupling capacitors (100 nF to 10 uF) cover 1-100 MHz. Package-level decoupling capacitors (100 nF MLCCs or embedded capacitors) cover 10-500 MHz. On-die MOS capacitors provide decoupling above 500 MHz. The package plays a critical role in the 10 MHz to 1 GHz range, which is the most challenging region because the parasitic inductance of the package interconnect (bumps, traces, vias, balls) creates impedance peaks that can exceed the target. The art of package PDN design is providing sufficient decoupling capacitance with low-inductance connections to suppress these peaks. Impedance analysis is performed in the frequency domain using tools that model the complete PDN from VRM to die, plotting the impedance magnitude and comparing it against the flat target impedance line.

---

### Q2. How does the package contribute to IR drop?

**Answer:**

IR drop in the package is the DC voltage loss as current flows from the BGA power balls through the package substrate to the die power bumps. It represents wasted voltage margin that must be accounted for in the voltage delivered to the die. The resistance of the package power path includes BGA solder ball resistance (typically 1-3 milliohms per ball, reduced to microohms in parallel with hundreds of power balls), substrate power plane resistance (dependent on copper thickness, plane shape, and current path length; for a 20 um copper plane at 30 mm path: R = rho * L / (w * t) = 1.7e-8 * 0.03 / (0.06 * 20e-6) = 0.43 milliohms), substrate via resistance (PTH: 0.5-2 milliohms each; microvias: 1-5 milliohms each; reduced by many parallel vias), and flip-chip bump resistance (5-20 milliohms per bump, reduced to microohms in parallel). For a modern processor package delivering 300 A at 0.8 V, the total package IR drop should be kept below 10-20 mV (1-2.5 percent of VDD). This requires total DC resistance below 0.067 milliohms (20 mV / 300 A), which demands hundreds of parallel power bumps and balls, multiple thick copper power planes, and wide routing paths. IR drop analysis is performed using DC power integrity simulation tools (Ansys SIwave, Cadence Sigrity PowerDC) that model the complete package PDN with current distribution and compute the voltage map across the die interface. Hot spots where IR drop exceeds the budget are mitigated by adding power vias, widening traces, or redistributing power bumps.

---

### Q3. What is Ldi/dt noise and how does the package contribute?

**Answer:**

Ldi/dt noise (also called simultaneous switching noise, SSN, or ground bounce) is the voltage fluctuation caused by the inductance of the power delivery path when current changes rapidly. When a circuit block switches, the current drawn from the supply changes by di in a time dt, and the voltage across the parasitic inductance is V = L * di/dt. For a processor with L_package = 20 pH total loop inductance and a current step of 100 A in 100 ps: V = 20e-12 * (100 / 100e-12) = 20 mV. This transient droop (for increasing current) or overshoot (for decreasing current) is one of the primary noise sources in high-performance digital systems. The package is the dominant contributor to this inductance for mid-frequency transients (10-500 MHz). The inductance comes from the power/ground bump pair loop (the current flows out through a VDD bump, through the on-chip circuit, and returns through a VSS bump -- the area enclosed by this current loop determines the inductance), substrate trace and via inductance, and BGA ball inductance. Mitigation strategies include minimizing the VDD-VSS current loop area in the package (alternating VDD and VSS bumps in a checkerboard pattern), increasing the number of power/ground bumps (more parallel paths reduce total inductance), placing on-package decoupling capacitors close to the die (short connections with low inductance), and co-designing the die power grid with the package power distribution. The on-die decoupling capacitance (from MOS transistors and intentional MIM capacitors) is the final defense against high-frequency Ldi/dt noise.

---

### Q4. What types of decoupling capacitors are used on packages?

**Answer:**

Package-level decoupling capacitors supplement on-die and board-level capacitors to maintain low PDN impedance. Surface-mount MLCC capacitors (100 nF to 10 uF, 0201 or 0402 size) are soldered to the package substrate top or bottom surface. They are effective in the 10-500 MHz range but are limited by their self-resonant frequency (above which they become inductive) and by the mounting inductance (the inductance of the solder pads, traces, and vias connecting the capacitor to the power planes). Minimizing mounting inductance is critical: a 100 nF capacitor with 200 pH mounting inductance is ineffective above approximately 300 MHz. Land-side capacitors (on the BGA side of the substrate) place the capacitor closer to the board-level power planes. Embedded capacitors are thin-film or thick-film capacitors built into the substrate layers during fabrication. Embedded thin-film capacitors use high-k dielectric materials (barium titanate, Dk of 500-2000) to achieve 10-100 nF/cm-squared capacitance density. They offer very low mounting inductance (because they are directly connected to the power planes with no external solder joints) and are effective at the highest frequencies (up to several GHz). However, they add substrate processing cost and may impact substrate yield. Silicon trench capacitors (deep trench capacitors fabricated in a silicon interposer) provide decoupling directly in the interposer, with very low inductance to the die. MIM (metal-insulator-metal) capacitors can be integrated in the interposer RDL layers. The trend is toward increasing on-package and in-package capacitance to address the inductance gap between board-level capacitors and on-die capacitors.

---

### Q5. How is the power delivery network co-designed between die and package?

**Answer:**

PDN co-design ensures that the combined die-package-board power delivery system meets the target impedance at all frequencies. The design process begins with the die power requirements: maximum current draw, transient current step (di) and timing (dt), and voltage tolerance specification. The target impedance is derived from these. The die design team provides the on-die capacitance (from standard cells, fill capacitors, and intentional decap cells) and the on-die power grid resistance and inductance. The board design team provides the VRM characteristics, board-level decoupling, and board trace impedance. The package design team fills the gap, providing decoupling in the 10 MHz to 1 GHz range and minimizing the inductive loop from board capacitors to die. The co-design flow involves impedance simulation: the complete PDN model (VRM + board + package + die) is assembled, and the impedance from the die perspective is computed across frequency. Resonant peaks that exceed the target impedance are identified, and capacitor values, locations, and quantities are optimized to suppress them. The key package design variables are the number and placement of decoupling capacitors, the power/ground bump map (which determines the power delivery loop inductance), the number and connectivity of power planes in the substrate, and the via count connecting power planes. Iteration between die, package, and board design is often necessary, especially for high-performance processors where the target impedance is sub-milliohm. Tools like Cadence Sigrity, Ansys SIwave, and Keysight ADS PathWave support multi-domain PDN simulation.

---

### Q6. What is the impact of bump map design on power delivery?

**Answer:**

The bump map (the assignment of each flip-chip bump to signal, power, or ground function) directly determines the package PDN quality. The key principle is that the VDD-VSS current loop inductance is proportional to the area enclosed by the current path: VDD flows from a VDD bump through the die circuit and returns through the nearest VSS bump. The closer the VDD and VSS bumps are, the smaller the loop area and the lower the inductance. A checkerboard arrangement (alternating VDD and VSS bumps) provides the lowest inductance but uses 50 percent or more of the bump array for power, leaving fewer bumps for signals. In practice, 40-60 percent of bumps are dedicated to power/ground in high-performance packages. Power bumps should be distributed uniformly across the die area (not concentrated in one region) to provide even current delivery and minimize on-die IR drop gradients. Signal bumps should be placed in the die regions with the highest I/O density (typically at the die edges near SerDes and memory interface circuits). Ground bumps should be placed adjacent to high-speed signal bumps to provide low-inductance return paths and minimize crosstalk. The bump map design is an iterative process involving the die I/O designer (who defines signal bump locations based on the on-chip floor plan), the package designer (who must route all signals through the substrate), and the power integrity engineer (who ensures adequate power delivery). Automated bump map optimization tools can explore millions of configurations to find the arrangement that minimizes PDN impedance while maintaining signal routability.

---

### Q7. How does package-level power delivery differ for chiplet architectures?

**Answer:**

Chiplet architectures introduce multiple power domains and longer power delivery paths compared to monolithic die. Each chiplet may operate at a different voltage (compute at 0.7 V, I/O at 1.0 V, SerDes at 0.9 V), requiring separate power planes or segmented planes in the substrate for each domain. The total current demand is the sum of all chiplets, which can exceed 500 A for large multi-chiplet packages. The power delivery path is longer in 2.5D packages: current flows from the PCB through BGA balls, organic substrate, C4 bumps, silicon interposer TSVs, interposer RDL, microbumps, and finally into the chiplet. Each transition adds resistance and inductance. The interposer TSVs for power delivery are critical: their total resistance must be low enough that IR drop across the interposer is negligible (typically below 1 mV), requiring hundreds to thousands of power TSVs per chiplet. Decoupling in chiplet packages must be distributed: on-package capacitors on the organic substrate provide low-frequency decoupling, while the interposer can incorporate trench capacitors or MIM capacitors for mid-frequency decoupling, and each chiplet provides its own high-frequency on-die decoupling. The die-to-die interface between chiplets also has power delivery implications: the I/O circuits that drive die-to-die links draw significant switching current, and the local power delivery must be robust enough to handle the transients without excessive supply noise that would degrade link margin. Integrated voltage regulators (IVR) on individual chiplets are being explored to improve transient response and enable per-chiplet DVFS (dynamic voltage and frequency scaling).

---

### Q8. What is power plane resonance and how is it managed?

**Answer:**

Power plane resonance occurs when the parasitic inductance and capacitance of the power-ground plane pair in the substrate form a resonant circuit at specific frequencies, causing impedance peaks that can exceed the target impedance. The resonant frequencies are determined by the plane dimensions and dielectric properties: f_mn = (c / (2 * sqrt(Dk))) * sqrt((m/Lx)^2 + (n/Ly)^2), where m and n are mode numbers and Lx, Ly are the plane dimensions. For a 50 mm x 50 mm plane on ABF dielectric (Dk = 3.3): the fundamental resonance (m=1, n=0) is f_10 = (3e8 / (2 * sqrt(3.3))) * (1/50e-3) = 1.65 GHz. At this frequency, standing waves create voltage maxima and minima across the plane surface, and the impedance peaks can be 10-100 times the DC impedance. Resonance management techniques include placing decoupling capacitors at the voltage maxima (anti-nodes) of each resonant mode, which damps the resonance. Multiple capacitors distributed across the plane surface are more effective than a few capacitors at specific locations. Increasing the dielectric loss (higher Df) naturally damps resonances but at the expense of signal integrity for traces on adjacent layers. Using embedded thin-film capacitor layers (which provide distributed capacitance across the entire plane area) is the most effective damping approach. Power plane segmentation (dividing a large plane into smaller zones with narrow connections) shifts the resonant frequencies and can reduce the peak impedance, but must be done carefully to avoid creating new resonances. Package-level PDN simulation across the full frequency range identifies resonant peaks and guides capacitor placement for damping.

---

### Q9. How do integrated voltage regulators affect package design?

**Answer:**

Integrated voltage regulators (IVR) place the voltage regulation function on the die itself (or on a dedicated die in the package) rather than on the motherboard, dramatically shortening the power delivery path and improving transient response. With a conventional external VRM on the motherboard, the current must traverse 10+ centimeters of board trace, through the package, to reach the die. The inductance of this long path limits the transient response bandwidth to approximately 1-10 MHz. With an on-die IVR, the regulator is microns away from the load circuits, providing regulation bandwidth in the hundreds of MHz range. This enables much tighter voltage regulation, reducing the required voltage guardband and allowing the die to operate at lower voltage (saving power) or higher frequency (improving performance). IVRs impact package design in several ways. The input voltage to the package increases (e.g., from 0.8 V to 1.8 V) because the IVR performs the final down-conversion on-die. Higher input voltage means lower input current (P = V * I), reducing the number of power bumps and the IR drop in the package. The package PDN design is simpler because the external supply is regulated at a higher, more tolerant voltage. However, IVRs consume die area (5-15 percent of die area for a switched-capacitor or buck converter IVR) and introduce conversion efficiency loss (typically 85-95 percent, meaning 5-15 percent of the power is wasted as heat in the regulator). The additional heat from IVR inefficiency must be managed by the package thermal solution. Intel has deployed IVRs in some client processor products, and they are being evaluated for data center processors and chiplet architectures where per-chiplet voltage regulation is valuable.

---

### Q10. How is power integrity validated in package design?

**Answer:**

Power integrity (PI) validation verifies that the package PDN meets the target impedance and voltage noise specifications through a combination of simulation and measurement. Simulation-based validation uses extracted models of the package PDN (from layout tools like Cadence Sigrity APD, Ansys SIwave) combined with die and board models to compute impedance versus frequency and time-domain voltage noise under representative switching current profiles. The impedance plot is checked against the target impedance specification, and the time-domain voltage waveform is checked against the minimum and maximum voltage bounds. Measurement-based validation uses test vehicles (packages built with dedicated PDN characterization structures) and impedance measurement techniques. VNA-based impedance measurement probes the package PDN at specific locations (typically at the die bump interface, accessed through probe pads) and measures the impedance from DC to several GHz. The measured impedance is compared against simulation for correlation. On-die voltage sensors (embedded in the production die or on test chips) measure the supply voltage waveform during operation, capturing droop events, ringing, and noise. Power delivery validation testing includes worst-case workload testing (running the most power-intensive application to generate maximum di/dt), power virus testing (synthetic worst-case switching patterns), and temperature cycling (to verify that solder joint aging does not degrade the PDN resistance). The ultimate validation is the product operating correctly at the minimum specified voltage across all frequencies and temperatures, with adequate margin for manufacturing variation.

---

## Further Reading

- [Signal Integrity in Packages](signal_integrity_in_packages.md)
- [EMI and Shielding](emi_and_shielding.md)
- [Silicon Interposers](../03_substrate_and_interposer_design/silicon_interposers.md)
- [Chiplet Architectures](../02_advanced_packaging/chiplet_architectures.md)
