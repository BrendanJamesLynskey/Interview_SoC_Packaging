# Signal Integrity in Packages

## Overview

Signal integrity (SI) in packages addresses the preservation of electrical signal quality as signals traverse from the die through bumps, substrate traces, vias, and solder balls to the PCB. As data rates exceed 10 Gbps per lane, package-level SI design becomes a critical differentiator.

---

### Q1. What are the primary package parasitics that affect signal integrity?

**Answer:**

Package parasitics are unintended electrical elements (resistance, inductance, capacitance) introduced by the physical structures of the package. The dominant parasitics include bump inductance and resistance (wire bond: 0.5-1.5 nH, 30-100 milliohms; flip-chip C4: 10-50 pH, 5-20 milliohms; microbump: 5-15 pH, 2-10 milliohms), which create impedance discontinuities and voltage drops. Pad capacitance at bump landing pads (50-200 fF per pad) loads the signal and reduces bandwidth. Substrate trace parasitics include distributed resistance, inductance, and capacitance along the trace length, characterized by per-unit-length parameters and manifested as insertion loss and propagation delay. Via parasitics from microvias and PTH include self-inductance (20-100 pH), capacitance from the anti-pad structure (50-150 fF), and via stub resonance (when the via extends beyond the signal layer, creating a quarter-wave resonant stub that causes notches in the transfer function at specific frequencies). Mutual inductance and capacitance between adjacent traces and vias cause crosstalk, where signal transitions on one net induce noise on neighboring nets. Package lead or ball parasitics (BGA ball: 50-200 pH inductance, 50-100 fF capacitance) add impedance discontinuity at the package-to-PCB interface. These parasitics are modeled using 3D electromagnetic simulation tools (Ansys HFSS, Cadence Clarity 3D, Siemens HyperLynx) and represented as S-parameter models or SPICE-compatible lumped/distributed circuit models for time-domain simulation.

---

### Q2. How is impedance controlled in package substrates?

**Answer:**

Impedance control ensures that the characteristic impedance of package traces matches the system impedance (typically 50 ohms single-ended or 85-100 ohms differential) to minimize signal reflections. The characteristic impedance of a substrate trace depends on its geometry (width, thickness, distance to reference plane) and the dielectric properties (permittivity) of the surrounding material. For a microstrip trace on an ABF dielectric (Dk = 3.3), the impedance is controlled by adjusting trace width relative to the dielectric thickness to the ground reference plane. Typical 50-ohm trace widths are 25-40 micrometers on 25-30 micrometer dielectric. For stripline (trace between two ground planes), the trace is narrower because the surrounding ground planes provide more capacitive coupling. Impedance tolerance is typically plus or minus 10 percent, requiring tight control of trace width (plus or minus 2-3 micrometers), dielectric thickness (plus or minus 3-5 micrometers), and dielectric constant (plus or minus 5 percent). Substrate manufacturers use test coupons with TDR (time-domain reflectometry) structures on each panel to verify impedance compliance. Impedance discontinuities at transitions (bump-to-trace, trace-to-via, via-to-ball) are managed through geometry optimization: matching pad sizes to trace impedance, using anti-pad tuning around vias, and adding ground vias adjacent to signal vias for return current management. Differential pair routing requires matched trace widths and consistent spacing to maintain both differential impedance and common-mode rejection.

---

### Q3. What is crosstalk in packages and how is it managed?

**Answer:**

Crosstalk is the unintended coupling of signal energy from an aggressor net to a victim net through mutual capacitance (electric field coupling) and mutual inductance (magnetic field coupling). In packages, crosstalk occurs between adjacent traces on the same layer (lateral crosstalk), between traces on adjacent layers (vertical crosstalk), between adjacent bumps or solder balls, and within via fields. Near-end crosstalk (NEXT) appears at the same end as the aggressor signal source and is the sum of capacitive and inductive coupling. Far-end crosstalk (FEXT) appears at the opposite end and is the difference between capacitive and inductive coupling. In homogeneous media (stripline), FEXT is theoretically zero because capacitive and inductive coupling cancel. In inhomogeneous media (microstrip), FEXT is non-zero and can be significant. Crosstalk management strategies include maintaining adequate spacing between signal traces (typically 3 times the trace width, or 3W rule), inserting ground traces between sensitive signals (shielding), using differential signaling (which inherently rejects common-mode crosstalk), routing high-speed signals on stripline layers (better shielding than microstrip), avoiding parallel routing of sensitive signals over long distances, and placing ground vias between signal vias in via fields. For multi-gigahertz signaling (PCIe Gen5, DDR5), crosstalk budgets are specified as part of the channel compliance specification, and 3D EM simulation is necessary to accurately predict crosstalk in complex via and trace structures.

---

### Q4. How do via stubs affect signal integrity and how are they mitigated?

**Answer:**

A via stub is the portion of a through-hole via that extends beyond the signal layer to which it connects. In a typical substrate with a through-core PTH, if the signal connects at a top build-up layer and the via extends through the core to the bottom build-up layers, the unused portion below the signal layer is a stub. This stub acts as a transmission line terminated in an open circuit, creating a resonant structure at frequencies where the stub length is a quarter wavelength. At the resonant frequency, the stub presents a short circuit to the signal trace, causing a deep notch in the insertion loss (often 10-30 dB). The resonant frequency is f = c / (4 * L_stub * sqrt(Dk)), where L_stub is the stub length and Dk is the effective dielectric constant. For a 0.4 mm stub in BT dielectric (Dk = 3.5): f = 3e8 / (4 * 0.4e-3 * sqrt(3.5)) = 100 GHz, which is well above typical signaling frequencies. But for a 2 mm stub (core thickness): f = 3e8 / (4 * 2e-3 * sqrt(3.5)) = 20 GHz, which impacts 56 Gbps PAM4 signaling (Nyquist at 14 GHz). Stub mitigation techniques include back-drilling (mechanically drilling out the stub from the back side of the substrate after PTH plating), using blind or buried vias instead of through-hole vias, designing the layer stackup so that high-speed signals use layers close to the via entry point (minimizing stub length), and coreless substrate designs that avoid through-core vias entirely. For advanced substrates at 56+ Gbps data rates, via stub control is one of the most critical SI design considerations.

---

### Q5. What is insertion loss and what contributes to it in a package?

**Answer:**

Insertion loss is the reduction in signal amplitude as it passes through the package, expressed in decibels (dB) at a given frequency. It is the key metric for evaluating whether a package channel can support a target data rate. Insertion loss has two main components. Conductor loss (also called resistive loss or copper loss) arises from the finite resistivity of copper traces and the skin effect (at high frequencies, current concentrates in a thin skin at the conductor surface, increasing effective resistance). Conductor loss increases proportionally to the square root of frequency. Surface roughness of the copper-dielectric interface further increases conductor loss by 20-50 percent through the Hammerstad or Huray roughness models. Dielectric loss arises from the molecular polarization of the dielectric material in the alternating electric field, quantified by the loss tangent (Df or tan-delta). Dielectric loss increases linearly with frequency. For ABF dielectric (Df = 0.015) at 14 GHz (Nyquist for 28 Gbps NRZ): a 20 mm trace contributes approximately 1.5-2.5 dB of total insertion loss. For 56 Gbps PAM4 (Nyquist at 14 GHz), the package insertion loss budget is typically 3-6 dB, depending on the channel compliance specification. Package designers minimize insertion loss by using low-loss dielectrics (Df less than 0.010), smooth copper (low roughness), short trace lengths, and appropriate trace geometry. The complete package S-parameter model (including bumps, traces, vias, and balls) is extracted from 3D EM simulation and validated against VNA measurements on test vehicles.

---

### Q6. How are eye diagrams used to evaluate package signal integrity?

**Answer:**

An eye diagram is a visualization tool created by overlaying many unit intervals (UIs) of a data signal on the same time axis, producing an "eye-shaped" opening that reveals the signal quality at the receiver input. A wide, open eye indicates good signal integrity with clear distinction between logic levels, adequate timing margin, and low noise. A closed or degraded eye indicates excessive loss, crosstalk, reflections, or jitter that may cause bit errors. Key eye diagram metrics include eye height (the vertical opening at the optimal sampling point, in millivolts), eye width (the horizontal opening, in picoseconds or fraction of UI), jitter (the horizontal variation of signal transitions, comprising deterministic jitter from ISI, crosstalk, and duty cycle distortion, plus random jitter from thermal noise), and signal-to-noise ratio. For NRZ signaling, there is one eye opening; for PAM4, there are three vertically stacked eyes, with the inner eyes being smaller and more susceptible to noise. Package SI engineers generate eye diagrams through simulation using the package S-parameter model combined with the transmitter and receiver circuit models in a channel simulation tool (Keysight ADS, Cadence Sigrity, Ansys EDA). The simulation includes equalization (CTLE, DFE at the receiver, FFE at the transmitter) to open the eye. Compliance is checked against the specification's eye mask, which defines minimum eye height and width. Measured eye diagrams from lab characterization of test vehicles validate the simulation methodology.

---

### Q7. What is return loss and why does it matter for packages?

**Answer:**

Return loss is the ratio of reflected signal power to incident signal power at a port, expressed in decibels: RL = -20 * log10(|S11|). It quantifies the impedance matching quality at a specific point in the channel. A higher return loss (more negative dB value) indicates better matching and less reflection. Package specifications typically require return loss better than -10 to -15 dB across the signaling bandwidth. Poor return loss (excessive reflections) causes several problems: reflected energy creates intersymbol interference (ISI) as reflected pulses arrive at the receiver at delayed times, overlapping with subsequent data bits; reflected energy can re-reflect from other impedance discontinuities, creating resonances; and reflected power is effectively lost signal power, adding to the total insertion loss budget. In packages, the main sources of poor return loss are bump transitions (the impedance change from die-level trace to bump to substrate trace), via transitions (impedance discontinuity at PTH or microvia connections), pad geometries (pads are wider than traces, creating local capacitive loading), and connector-like structures (BGA ball array). Return loss improvement techniques include matching pad dimensions to trace impedance, using anti-pad optimization around vias (tuning the clearance in the reference plane to compensate for via capacitance or inductance), adding ground vias adjacent to signal vias for proper return current flow, and gradual impedance transitions rather than abrupt geometry changes. Time-domain reflectometry (TDR) measurements on test structures identify the specific locations and magnitudes of impedance discontinuities.

---

### Q8. How does differential signaling improve package SI?

**Answer:**

Differential signaling transmits a signal as a pair of complementary voltages on two conductors (positive and negative), with the information encoded in the voltage difference between them. Any noise that couples equally to both conductors (common-mode noise) is rejected by the differential receiver. In packages, differential signaling provides several SI advantages. Common-mode noise rejection eliminates crosstalk from adjacent single-ended signals, power supply noise coupling through the substrate, and electromagnetic interference from external sources. The differential impedance is more stable than single-ended impedance because it depends primarily on the coupling between the two traces rather than their distance to the ground plane, making it less sensitive to dielectric thickness variations. EMI emission is lower because the equal and opposite currents in the differential pair produce canceling electromagnetic fields at far-field distances. The signal swing can be reduced (low-swing differential signaling at 200-400 mV) because the differential receiver has better noise immunity, reducing power consumption. In package substrates, differential pairs are routed with matched trace lengths and consistent spacing to maintain impedance and timing balance. Common applications include DDR5 data signals (pseudo-differential), PCIe (differential), USB (differential), and die-to-die chiplet links (differential). The penalty of differential signaling is that it requires twice the number of signal pins and traces compared to single-ended, doubling the routing resource requirement.

---

### Q9. What is ISI and how does the package contribute to it?

**Answer:**

Intersymbol interference (ISI) occurs when the response of the channel to one data bit extends into adjacent bit periods, causing the received signal to be a superposition of multiple bit responses. ISI is the dominant impairment in high-speed serial links and is caused by the frequency-dependent loss and dispersion of the channel. The package contributes to ISI through several mechanisms. Frequency-dependent insertion loss (conductor loss scaling as sqrt(f) and dielectric loss scaling linearly with f) attenuates high-frequency components of the signal more than low-frequency components, converting sharp digital edges into rounded transitions that spread into adjacent bit times. Reflections from impedance discontinuities (bumps, vias, pads) create delayed copies of the signal that overlap with subsequent bits. Dispersion (frequency-dependent group delay) means that different frequency components of a pulse arrive at the receiver at different times, spreading the pulse. The total ISI from the package is captured in the pulse response of the package S-parameter model: a single ideal pulse input to the package produces a dispersed, ringing output whose tails extend into neighboring bit periods. The amplitude of these tails relative to the main pulse amplitude determines the ISI penalty. Equalization at the transmitter (feed-forward equalization, FFE) pre-compensates for the channel loss by boosting high-frequency content. Equalization at the receiver (CTLE for continuous-time linear equalization, DFE for decision feedback equalization) removes ISI from the received signal. The equalization capability specified for the interface (e.g., PCIe Gen5 specifies 3-tap TX FFE and CTLE+DFE at the receiver) determines how much ISI the package channel is allowed to introduce.

---

### Q10. How are S-parameters used to characterize package electrical performance?

**Answer:**

Scattering parameters (S-parameters) are the standard representation of package electrical behavior across frequency. They describe the linear relationship between incident and reflected/transmitted waves at each port of the package model. For a two-port network (signal input and output), S11 is the input reflection coefficient (return loss), S21 is the forward transmission coefficient (insertion loss), S12 is the reverse transmission coefficient, and S22 is the output reflection coefficient. For a complete package channel with multiple signal paths and coupling between them, the S-parameter matrix is NxN where N is the total number of ports (including victim and aggressor ports for crosstalk analysis). S-parameters are extracted from 3D electromagnetic simulation of the package geometry (bumps, traces, vias, balls) using tools such as HFSS, Clarity 3D Solver, or CST. The simulation divides the package into a mesh of small elements, solves Maxwell's equations at each frequency point, and computes the S-matrix. The resulting S-parameter file (Touchstone format, .snp) is used in circuit simulators for channel analysis: cascading the package S-parameters with the die model and PCB model to simulate the complete signal path. S-parameters are also measured on test vehicles using a vector network analyzer (VNA) with calibrated probes or connectors, providing validation data for the simulation. Key S-parameter-based metrics include insertion loss (|S21| in dB), return loss (|S11| in dB), crosstalk (NEXT: |S31|, FEXT: |S41| for a 4-port model), and impedance profile (derived from time-domain transformation of S11).

---

## Further Reading

- [Power Delivery in Packages](power_delivery_in_packages.md)
- [EMI and Shielding](emi_and_shielding.md)
- [Organic Substrates](../03_substrate_and_interposer_design/organic_substrates.md)
- [Die-to-Package Interconnect](../01_foundations/die_to_package_interconnect.md)
