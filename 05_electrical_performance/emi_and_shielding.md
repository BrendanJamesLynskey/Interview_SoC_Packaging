# EMI and Shielding

## Overview

Electromagnetic interference (EMI) from semiconductor packages can cause regulatory compliance failures and functional interference with other circuits. Package-level EMI mitigation through shielding and grounding is increasingly important as clock frequencies and data rates rise.

---

### Q1. What are the sources of EMI from semiconductor packages?

**Answer:**

EMI from packages originates from high-frequency current loops that radiate electromagnetic energy. The primary sources include clock distribution networks (which carry periodic high-frequency signals with strong harmonic content), high-speed I/O interfaces (SerDes, DDR, PCIe) that create broadband spectral energy from fast signal transitions, power delivery networks (where switching current creates common-mode noise on power/ground planes that radiates from package and board edges), and wire bonds or bond wires acting as antennas (their length of 1-3 mm approaches a significant fraction of wavelength at multi-GHz frequencies). The radiation mechanism involves current loops: any loop of current-carrying conductor acts as a magnetic dipole antenna, with radiated power proportional to the loop area squared and frequency squared. In packages, the primary radiation loops are formed by signal current flowing from the die through the bump, across the substrate trace, through the BGA ball to the PCB, and returning through a different path (ground plane, adjacent ground ball). The larger this loop area, the more radiation. Differential signals radiate less than single-ended because the differential mode current loop is small (confined between the two traces), while common-mode current flows over a larger loop and radiates more efficiently. Package-level EMI becomes significant above approximately 1 GHz, where the package dimensions (10-50 mm) become a meaningful fraction of the electromagnetic wavelength (30 cm at 1 GHz).

---

### Q2. How does package shielding work?

**Answer:**

Package shielding encloses the package in a conductive structure that attenuates electromagnetic radiation. The most common approach for SiP and module packages is a conformal metal shield: a thin metal layer (2-10 micrometers of copper, nickel, or stainless steel) deposited directly on the surface of the molding compound by sputtering or electrolytic plating. This shield connects to the package ground through via fences (rows of ground vias around the package perimeter) or through ground pads on the package surface. The shield effectiveness depends on the conductivity and thickness of the metal layer relative to the skin depth at the frequencies of interest. At 1 GHz, the skin depth in copper is approximately 2 micrometers, so a 5-micrometer copper shield provides approximately 2.5 skin depths of attenuation (over 20 dB). Metal can lids are another shielding approach, used on larger modules: a stamped or machined metal enclosure (steel, aluminum, or mu-metal) is soldered or clipped onto the substrate, enclosing all die and components. Can lids provide 30-60 dB of shielding effectiveness but add height and cost. For compartmentalized shielding (isolating different functional blocks within a SiP), internal fence walls or compartment dividers can be added between die regions. Package-level shielding is most commonly applied in RF modules (Wi-Fi, Bluetooth, cellular) where regulatory compliance (FCC Part 15, ETSI EN 301) requires tight control of radiated emissions.

---

### Q3. What grounding strategies minimize EMI at the package level?

**Answer:**

Effective grounding is the foundation of EMI control in packages. The primary strategy is to provide a continuous, low-impedance ground reference plane throughout the package. In the substrate, at least one and preferably two or more metal layers should be dedicated ground planes, providing a continuous current return path for all signals and a reference for impedance-controlled traces. Ground via fences (rows of closely-spaced ground vias) around the package perimeter connect the ground planes to the BGA ground balls, creating a cage that contains electromagnetic fields within the package. The spacing between ground vias should be less than lambda/20 at the highest frequency of concern to prevent field leakage. For BGA packages, the BGA ball map should include a continuous ring of ground balls around the package perimeter, which connects the package ground plane to the PCB ground plane with minimal inductance. Signal return currents should be managed: every signal should have a nearby ground reference, and signal-ground-signal (SGS) bump and ball patterns maintain tight return current loops. Split ground planes should be avoided because they force return currents to detour around the split, creating large loops that radiate. When multiple power domains require separate ground planes, they should be connected at a single point near the package center to prevent ground loops. For die-to-die connections in multi-die packages, the ground structure between die regions should provide complete shielding to prevent coupling.

---

### Q4. How does the BGA ball map affect EMI?

**Answer:**

The BGA ball map influences EMI through its effect on current return paths and radiation from the package-to-board interface. Ground balls provide the primary return current path for high-speed signals: the signal current flows through a signal ball, traverses the PCB trace, and returns through the nearest ground ball back into the package ground plane. The distance between a signal ball and its nearest ground ball determines the return loop area and hence the radiation. An optimized EMI-conscious ball map places ground balls adjacent to every high-speed signal ball (signal-ground pairs or signal-ground-signal triplets in the ball array). A complete ground ring around the package perimeter provides a low-inductance ground connection that contains fields. Power balls should also be distributed throughout the array (not concentrated in one area) to minimize the power/ground current loop area. The ball pitch affects EMI: finer pitch (0.65 mm or less) allows ground balls to be placed closer to signal balls, reducing loop area. Depopulated ball patterns (where some ball positions are left empty for routing escape or cost reasons) can create gaps in the ground plane connection to the PCB, degrading shielding effectiveness. For packages with known EMI-sensitive signals (high-speed clocks, RF outputs), dedicated ground balls should be assigned adjacent to these signals in the ball map. The ball map design must balance signal routing needs, power delivery requirements, and EMI performance.

---

### Q5. What is the relationship between power integrity and EMI?

**Answer:**

Power integrity and EMI are closely linked because power delivery noise (voltage ripple on VDD and VSS nets) drives common-mode currents that radiate. When a digital circuit switches, it draws a current pulse from the VDD supply and returns it through the VSS path. If the VDD and VSS impedances are not perfectly matched (which they never are in practice), the switching current creates a common-mode voltage that drives current through unintended paths, including radiation from package and board structures. Power plane resonances (standing waves on the power-ground cavity at specific frequencies) create high-impedance points that amplify the common-mode voltage, causing EMI peaks at the resonant frequencies. The PDN edge radiation mechanism occurs because the power-ground plane pair acts as a parallel-plate waveguide, and electromagnetic energy leaks from the open edges of the planes. This edge radiation is proportional to the fringing field strength at the plane edges, which is enhanced by resonances. Mitigation approaches that improve both PI and EMI include placing decoupling capacitors (which damp resonances, reducing both voltage noise and radiation), using continuous ground planes (which reduce both PDN impedance and radiation), minimizing the gap between power and ground planes (which reduces the characteristic impedance of the parallel-plate waveguide, lowering the stored energy), and adding absorptive materials or resistive termination at plane edges. In practice, designs that achieve good power integrity generally also have acceptable EMI performance.

---

### Q6. How are RF packages designed for EMI isolation?

**Answer:**

RF packages (for cellular, Wi-Fi, Bluetooth, radar applications) require exceptional EMI isolation to prevent the RF circuits from being disturbed by digital noise and to prevent RF emissions from violating regulatory limits. The key design strategies include compartmentalized shielding (placing metal walls between the RF section and digital section within the same package, with separate ground planes for each compartment), die-level ground rings (surrounding the RF die with a continuous ring of ground bumps and substrate ground vias), and substrate-level isolation (using dedicated ground layers between the RF and digital routing layers). Signal isolation between RF and digital domains requires physical separation (maintaining several millimeters of distance between RF and digital traces), ground plane continuity (never routing digital signals across RF ground planes or vice versa), and filtered power supplies (each domain receiving its own filtered and regulated power through separate power planes and decoupling). For receive-path sensitivity, the package must achieve 60-80 dB of isolation between the digital aggressor and the RF victim. This typically requires multiple levels of shielding: on-die shielding (guard rings around sensitive LNA circuits), package-level compartmentalization, and an external conformal shield. Antenna integration in the package (common for mmWave 5G and Wi-Fi modules) adds another dimension: the antenna must radiate efficiently in the desired direction while minimizing coupling to other circuits in the package. Antenna placement, ground plane design, and feed network routing are co-optimized using full-wave electromagnetic simulation.

---

### Q7. What is conformal shielding and how is it applied?

**Answer:**

Conformal shielding is a thin metal coating applied directly to the outer surface of a molded semiconductor package, providing EMI shielding without the size and weight penalty of a metal can. The process involves first creating a grounding connection from the shield layer to the package ground network: a trench or groove is cut around the package perimeter (before or after molding) that exposes the substrate ground pads or ground via row. Then a multi-layer metal stack (typically Ti/Cu/stainless steel or Ti/Cu/NiV) is deposited by sputtering over the entire top and sides of the package. The sputtered metal contacts the exposed ground features in the trench, completing the electrical connection from the shield to the internal ground. The metal thickness is typically 3-10 micrometers total. Conformal shielding adds only 10-20 micrometers to the package dimensions (negligible) and 5-15 percent to the package cost. Shielding effectiveness of 20-40 dB is achievable at frequencies from 1-10 GHz, which is sufficient for most consumer wireless applications. Advanced conformal shielding configurations include compartmentalized shielding (where internal trenches are cut between functional blocks, and separate shield segments are deposited for each compartment) and selective shielding (where only specific areas of the package are shielded, reducing cost). Conformal shielding is widely used in mobile phone SiPs, Wi-Fi/Bluetooth combo modules, and cellular front-end modules. The main limitations are that the shield must be grounded through the trench structure (which requires substrate design accommodation) and that the shield can only be applied to the exterior of the package (it cannot shield between die that are side-by-side under the same mold cap).

---

### Q8. How does EMI testing work for semiconductor packages?

**Answer:**

EMI testing for semiconductor packages is performed at the board or system level, not on the bare package, because radiation depends on the complete current loop including the PCB. The primary regulatory standards are FCC Part 15 (USA), ETSI EN 301 489 (Europe), and CISPR 22/32 (international), which specify maximum radiated emission levels at specified distances (typically 3 or 10 meters) across frequency bands from 30 MHz to 40 GHz. Testing is performed in an anechoic chamber (semi-anechoic or fully anechoic) with calibrated antennas and a spectrum analyzer or EMI receiver. The device under test is mounted on a test board that exercises the package under realistic operating conditions (clocks running, data buses active, I/O toggling). The test board is rotated and the antenna height is varied to find the maximum emission at each frequency. Near-field scanning uses a small magnetic or electric field probe scanned across the package surface at close range (1-10 mm) to map the electromagnetic emission sources spatially. This does not provide regulatory compliance data but is invaluable for identifying radiation hot spots (which package structure is the primary emitter) and guiding design improvements. TEM cell measurements can be used for relative comparisons between package design variants. Package designers also use simulation (3D EM tools with full-wave solvers) to predict radiation from the package model before fabrication, enabling pre-silicon EMI optimization. SI/PI co-simulation that captures both differential-mode signal transmission and common-mode noise conversion is essential for accurate EMI prediction.

---

### Q9. What is the skin effect and how does it relate to shielding?

**Answer:**

The skin effect is the tendency of alternating current to flow in a thin layer (the skin depth) at the surface of a conductor, rather than uniformly through its cross-section. The skin depth decreases with increasing frequency: delta = sqrt(rho / (pi * f * mu)), where rho is resistivity, f is frequency, and mu is permeability. For copper at 1 GHz: delta = sqrt(1.7e-8 / (pi * 1e9 * 4*pi*1e-7)) = 2.1 micrometers. At 10 GHz: delta = 0.66 micrometers. The skin effect is fundamental to electromagnetic shielding: a conductive shield attenuates electromagnetic fields by reflection (at the air-metal interface) and absorption (exponential decay of fields within the metal). Each skin depth of metal thickness provides approximately 8.7 dB of absorption loss. The total shielding effectiveness of a metal sheet is: SE = 20 * log10(e^(t/delta)) + reflection loss = 8.7 * (t/delta) + R_dB. For a 5 micrometer copper shield at 1 GHz: absorption = 8.7 * (5/2.1) = 20.7 dB; reflection adds another 10-20 dB depending on wave impedance, giving total SE of 30-40 dB. Shielding is more effective at higher frequencies (where skin depth is smaller, so more absorption per unit thickness) and for thicker shields. For conformal package shields, the 3-10 micrometer metal thickness provides adequate shielding at GHz frequencies. At lower frequencies (below 100 MHz), the skin depth is much larger (21 micrometers at 100 MHz for copper), and thin conformal shields become less effective; thicker metal lids or board-level shielding may be needed.

---

### Q10. How do grounding and shielding differ for analog versus digital sections in a mixed-signal SoC package?

**Answer:**

Mixed-signal SoC packages contain both digital circuits (processors, logic, memories) and analog circuits (ADCs, DACs, PLLs, RF transceivers) that have fundamentally different noise sensitivities and noise generation characteristics. Digital circuits generate high-amplitude switching noise (hundreds of milliamps of di/dt) but are relatively tolerant of supply noise (50-100 mV ripple is typically acceptable). Analog circuits generate minimal noise but are extremely sensitive to supply noise (even 1-5 mV of ripple on an ADC supply can degrade dynamic range by 10-20 dB). The grounding strategy must isolate the noisy digital ground from the quiet analog ground while maintaining a single system ground reference. Star grounding connects both ground domains at a single point (typically at the package-to-board interface) to prevent digital ground current from flowing through the analog ground path. In the package substrate, separate ground planes for analog and digital domains may be used, connected by a narrow bridge at the star point. Alternatively, a single continuous ground plane with careful current flow management (ensuring digital return currents do not flow beneath analog circuits) can be effective and simpler. Power supply isolation uses separate power planes for analog and digital domains, with ferrite bead or RC filter connections between them to prevent digital supply noise from reaching analog circuits. Physical separation between analog and digital die sections (or separate die in a SiP) provides spatial isolation. Shield walls (ground via fences) between analog and digital regions in the substrate provide electromagnetic isolation. The bump map should place a row of ground bumps between the analog and digital bump fields. The key principle is that analog ground must be kept quiet: no digital current should flow through any conductors in the analog signal path.

---

## Further Reading

- [Signal Integrity in Packages](signal_integrity_in_packages.md)
- [Power Delivery in Packages](power_delivery_in_packages.md)
- [Assembly Processes](../06_manufacturing_and_test/assembly_processes.md)
- [Package Testing](../06_manufacturing_and_test/package_testing.md)
