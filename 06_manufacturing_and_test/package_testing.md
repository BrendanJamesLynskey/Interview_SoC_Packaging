# Package Testing

## Overview

Package testing verifies that the assembled package functions correctly and meets its electrical, thermal, and reliability specifications. Testing occurs at multiple stages and is a significant contributor to total product cost.

---

### Q1. What is the difference between wafer-level test and package-level test?

**Answer:**

Wafer-level test (wafer sort or wafer probe) is performed on the die while still on the wafer, before singulation and packaging. A probe card with fine needles contacts the die bond pads, and automatic test equipment (ATE) applies test patterns and measures responses. Wafer sort identifies known good die (KGD) for packaging, screens out gross defects (stuck-at faults, open/short circuits, excessive leakage), and bins die by speed grade. However, wafer sort has limitations: probe contact to fine-pitch pads (below 50 micrometers) is challenging, high-speed testing is limited by probe card parasitics, burn-in is not practical on a prober, and the test environment differs from the final package (no controlled impedance, no power delivery network). Package-level test (final test) is performed after the die is assembled into its package. The package is inserted into a test socket on an ATE loadboard, which provides controlled impedance signal paths, calibrated power supplies, and thermal control. Final test exercises all functions including high-speed I/O (SerDes loopback at full data rate), memory interfaces (DDR at speed), parametric measurements (voltage levels, timing margins, leakage currents), and often at-speed functional testing with production workloads. Final test also performs binning (sorting parts into speed/power grades for different product SKUs). For high-reliability applications, burn-in (operating at elevated voltage and temperature for hours to screen infant mortality failures) is performed between wafer sort and final test.

---

### Q2. What is Built-In Self-Test (BIST) and how is it used in packaging?

**Answer:**

Built-In Self-Test (BIST) is on-chip circuitry that generates test patterns and checks responses without requiring external test equipment, enabling the die to test itself. BIST is critical for packaging applications in several ways. Memory BIST (MBIST) tests embedded SRAM, caches, and register files by running march patterns and checkerboard algorithms. MBIST can detect bit cell failures that may have been caused by the packaging process (mechanical stress on thin-film structures). Logic BIST (LBIST) uses a linear feedback shift register (LFSR) to generate pseudo-random patterns, applies them to the logic, and compresses the responses into a signature for comparison. PLL BIST verifies that on-chip clock generation circuits lock correctly at the target frequency after packaging. SerDes BIST generates PRBS (pseudo-random bit sequence) patterns on each transmitter lane and checks for bit errors at each receiver, enabling die-to-die and package-to-board link testing without external test equipment. Die-to-die BIST is particularly important for chiplet packages where the inter-chiplet links cannot be probed externally; the BIST engines on each side of the link test the connection after assembly. BIST reduces ATE test time (because the test logic runs on-chip at full speed, faster than external test can stimulate the device) and test cost. It also enables in-field testing and diagnostics. For advanced packages with thousands of die-to-die connections, BIST is the only practical way to verify interconnect integrity.

---

### Q3. What is Known Good Die (KGD) and why is it critical?

**Answer:**

Known Good Die (KGD) refers to bare die that have been tested to a quality level equivalent to a fully packaged and tested device. The concept is critical for multi-die packaging because assembling an untested or undertested die into an expensive multi-die package and then discovering it is defective wastes all the other good die and packaging materials in that assembly. For a 2.5D package with four die at $200 each plus a $150 interposer and $100 assembly cost, scrapping due to one bad die costs $950. KGD quality targets are typically below 100 DPPM (defective parts per million), meaning fewer than 1 in 10,000 die should escape testing as defective. Achieving KGD quality requires comprehensive wafer-level testing: full scan testing (stuck-at and transition fault patterns with greater than 99 percent fault coverage), functional testing at the target operating frequency, parametric testing (leakage, VDD current, I/O levels), BIST for all memories and PLLs, and optionally burn-in at the wafer level (though this is difficult and expensive). Some companies use wafer-level reliability (WLR) screening to detect early-life failures. The test program must balance thoroughness against test time and cost: a comprehensive KGD test might take 5-30 seconds per die, compared to 1-5 seconds for a basic wafer sort. The additional test cost is justified by the assembly cost savings from not packaging defective die. As multi-die packages become more complex and expensive, KGD quality requirements will continue to tighten.

---

### Q4. How does ATE (Automatic Test Equipment) work for package testing?

**Answer:**

ATE systems are complex, highly parallel test instruments that apply electrical stimuli to packaged semiconductors and measure their responses to verify functionality and performance. A modern ATE system consists of several subsystems. The digital pin electronics provide high-speed digital stimulus and response capture on hundreds to thousands of channels simultaneously. Each pin can drive digital patterns at rates up to 3-6 Gbps (for current-generation testers) with programmable voltage levels, timing edges, and format (NRZ, RZ, DNRZ). High-speed RF/SerDes instruments provide stimulus and measurement capability at 25-112+ Gbps for testing high-speed serial interfaces. Analog instruments include arbitrary waveform generators, digitizers, and spectrum analyzers for mixed-signal testing. DC parametric measurement units (PMUs) on each pin provide precision current and voltage sourcing and measurement for leakage, continuity, and parametric tests. Power supplies provide regulated, low-noise power at the specified voltages and currents. The device handler or prober mechanically positions the DUT (device under test) in a test socket on the loadboard. The loadboard is a custom PCB designed for each package type, containing the test socket, signal routing, power filtering, and impedance-matched transmission lines from the ATE instruments to the socket pins. Major ATE vendors include Teradyne (UltraFlex, J750), Advantest (V93000, T2000), and Cohu (formerly Xcerra). ATE costs range from $1 million for simple digital testers to $10+ million for advanced mixed-signal and RF testers. Test cost per device is typically $0.01-1.00 depending on test time and tester type.

---

### Q5. What is opens/shorts testing and how is it performed?

**Answer:**

Opens/shorts testing is the most basic electrical test performed on a packaged device, verifying the physical integrity of all signal connections from the die through the package to the external pins. An open is a broken connection (a solder bump that did not wet, a cracked trace, a broken wire bond) that prevents current flow. A short is an unintended connection between two nets (solder bridging between adjacent bumps, metal debris on the substrate) that creates a low-resistance path where none should exist. Opens testing applies a small current (typically 100 microamps to 1 milliamp) through each pin and measures the voltage. An open connection will show a high voltage (the protection diode does not conduct), while a properly connected pin shows a low voltage (the I/O pad has a protection diode to VDD and VSS that clamps the voltage). Shorts testing applies voltage to one pin while monitoring adjacent pins for unexpected current flow. On-chip scan chain testing can also detect opens (stuck-at-1 or stuck-at-0 behavior at the pin receiver) and shorts (two pins responding identically). For flip-chip packages, bump integrity is indirectly tested through the opens/shorts test: a non-wet bump creates an open that is detected. For multi-die packages, die-to-die connection integrity is tested through BIST engines that exercise the inter-die links. Contact testing at the socket level (verifying that each ATE pin makes good contact with the DUT ball through the test socket) is also important and is performed before functional testing to avoid false failures from socket contact issues.

---

### Q6. What is burn-in and when is it used?

**Answer:**

Burn-in is an accelerated stress test that operates devices at elevated voltage (typically 10-20 percent above nominal VDD) and elevated temperature (125-150 degrees Celsius) for a defined duration (typically 12-168 hours) to screen out infant mortality failures. The acceleration factors from voltage and temperature stress cause latent defects (gate oxide defects, marginal transistors, weak vias) to fail during burn-in rather than during field use. Devices that survive burn-in are expected to have significantly longer field life. Burn-in is used when high reliability is required: automotive (AEC-Q100 requires burn-in for Grade 0 and Grade 1), military/aerospace (MIL-STD-883), medical devices, and high-reliability server and networking equipment. Burn-in can be performed at the package level (devices inserted in burn-in sockets on burn-in boards) or at the wafer level (wafer-level burn-in, WLBI, using specialized probe cards). Package-level burn-in is more common because it can apply full operating conditions including high-speed I/O. The burn-in board contains the socket, decoupling capacitors, and connections to the burn-in oven's power and signal infrastructure. Dynamic burn-in applies actual test patterns during burn-in (exercising the device logic) and is more effective at screening defects than static burn-in (which only applies voltage without toggling). The main drawback of burn-in is cost: it requires expensive ovens, burn-in boards, long cycle times, and handling. For high-volume consumer products, burn-in is often replaced by voltage screening (brief high-voltage stress at test) or statistical sampling.

---

### Q7. How are high-speed I/O interfaces tested at the package level?

**Answer:**

High-speed I/O interfaces (SerDes, DDR5, PCIe Gen5/6, USB4) require specialized testing because their data rates exceed the capabilities of standard digital ATE pin electronics. SerDes testing typically uses loopback mode: the ATE configures the device to transmit a known PRBS (pseudo-random bit sequence) pattern on its TX lanes, which are routed on the loadboard to the device's RX lanes (external loopback) or internally looped back on-chip (internal loopback). The device checks for bit errors using its internal BIST and reports the BER (bit error rate). This tests the complete TX and RX circuits including equalization, CDR, and eye opening. ATE high-speed instruments can also drive patterns directly into the RX and capture TX output for analysis, but at 56-112 Gbps, this requires extremely expensive multi-gigahertz instruments and precision loadboard design. DDR5 testing requires the ATE to emulate a memory controller or memory device (depending on which is the DUT). The ATE drives DDR5-compliant commands at full speed and verifies data integrity. Memory interface testing is particularly demanding because of the strict timing relationships between data, strobe, and clock. Jitter testing measures the timing variation of clock and data transitions using high-resolution time interval analyzers or the DUT's own jitter measurement BIST. Shmoo plotting (sweeping voltage and timing margins to find the operating window) maps the device's performance margins, which are affected by the package parasitics and signal integrity. For chiplet-based products, die-to-die interface testing uses on-chip BIST exclusively because these interfaces have no external pins.

---

### Q8. What is the test socket and why is its design important?

**Answer:**

The test socket is the mechanical and electrical interface between the packaged device and the ATE loadboard. It must reliably contact every pin (or ball) of the DUT while maintaining signal integrity, power delivery quality, and thermal control. For BGA packages, the socket contains an array of spring-loaded contact pins (pogo pins, cantilever contacts, or elastomer contacts) that press against the BGA solder balls when the device is inserted. Socket design considerations include contact resistance (less than 30-50 milliohms per pin), which must remain stable over millions of insertions. Bandwidth must be sufficient for the fastest interfaces on the DUT: a socket for 56 Gbps SerDes testing must provide less than 1 dB insertion loss at 28 GHz, requiring carefully impedance-matched contact elements and minimal parasitic structures. Contact force per pin (typically 5-30 grams) multiplied by pin count gives the total clamping force (for a 5000-ball BGA at 20 g/pin: 100 N total force), which must be applied uniformly without damaging the DUT. Thermal management may be integrated into the socket (thermal interface from the DUT lid to a cooling plate) to maintain the junction temperature during high-power testing. Socket durability is measured in insertion cycles (typically rated for 100,000 to 500,000 cycles before contact wear degrades performance). Socket cost ranges from $5,000 for simple QFN sockets to $50,000+ for high-pin-count, high-bandwidth BGA sockets. Socket design is a specialized discipline, with companies like Sensata/Wells, Smiths Interconnect, and Ironwood Electronics providing custom solutions. Poor socket design is a common source of test yield loss (false failures from intermittent contact, impedance mismatch causing SerDes BER degradation).

---

### Q9. How is test coverage measured and why does it matter?

**Answer:**

Test coverage quantifies the fraction of potential defects that the test program can detect. Fault coverage is the most common metric, defined as the ratio of detected faults to total possible faults in a fault model. Stuck-at fault coverage (the percentage of stuck-at-0 and stuck-at-1 faults at all circuit nodes that the test patterns detect) is the baseline metric; modern test programs target greater than 99 percent stuck-at coverage. Transition fault coverage (detecting delay defects) targets greater than 95-98 percent. Bridge fault coverage (detecting shorts between adjacent wires) is increasingly important. Achieving high fault coverage requires design-for-test (DFT) structures: scan chains that convert sequential logic into combinational logic for easy test access, BIST for memories and datapaths, and boundary scan (JTAG) for board-level interconnect testing. Defect per million (DPM) or DPPM (defective parts per million) measures the rate of defective devices that escape to the customer. The relationship between fault coverage and DPM depends on the defect density and defect distribution: higher fault coverage drives lower DPM. A rough guideline is that each 0.1 percent increase in fault coverage above 99 percent reduces DPM by approximately 10-30 percent. For automotive and data center products, DPM targets of 1-10 DPM are common, requiring greater than 99.5 percent fault coverage combined with burn-in or voltage screening. Undertesting (low coverage) risks customer returns and field failures; overtesting (excessive test time) increases cost. Test engineers optimize the test program to maximize coverage per unit test time, often using test point analysis and fault simulation tools.

---

### Q10. What test challenges are unique to advanced packages?

**Answer:**

Advanced packages (2.5D, 3D, chiplet, fan-out) introduce test challenges beyond conventional single-die packages. Die-to-die interconnect testing is the most significant: the connections between chiplets are internal to the package with no external probe access. Testing relies entirely on BIST engines built into each chiplet, following standards like IEEE 1838 (3D test access standard). Pre-bond testing of individual die for KGD must achieve very high quality (less than 100 DPPM) because the cost of scrapping a multi-die assembly is extremely high. Post-bond testing must verify all die-to-die connections after assembly, adding test time. Multi-die test logistics are complex: each die type may require different test equipment, different test programs, and different test conditions. Coordinating the test flow for a product with 4-8 different die types is a significant engineering challenge. Fine-pitch probing for wafer-level test at bump pitches below 50 micrometers requires advanced probe card technology (MEMS-based probes, cantilever probes) that is expensive and has limited contact reliability. Known good stack (KGS) testing for 3D stacked die (e.g., testing a partial HBM stack before adding more die) is desirable but mechanically challenging. Test time accumulation is a concern: if each die requires 5 seconds of test and there are 8 die, the total test time per package is 40+ seconds, significantly increasing test cost. Design-for-test planning must begin at the chiplet architecture definition phase, not as an afterthought, to ensure testability of all inter-die connections and to minimize total test cost.

---

## Further Reading

- [Assembly Processes](assembly_processes.md)
- [Yield and Reliability](yield_and_reliability.md)
- [Chiplet Architectures](../02_advanced_packaging/chiplet_architectures.md)
- [2.5D and 3D Packaging](../02_advanced_packaging/2_5d_and_3d_packaging.md)
