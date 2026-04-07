# Assembly Processes

## Overview

Package assembly transforms individual silicon die and substrates into completed, testable packages through a sequence of precision manufacturing steps. Understanding these processes is essential for packaging engineers and is frequently tested in interviews.

---

### Q1. What are the main steps in flip-chip BGA assembly?

**Answer:**

Flip-chip BGA assembly follows a well-defined sequence. Wafer preparation begins with wafer bumping (forming solder bumps or copper pillars on the die pads) if not already done at the foundry, followed by wafer mounting on dicing tape and frame. Wafer dicing uses a diamond blade saw or laser to singulate the wafer into individual die, with typical kerf (cut width) of 20-50 micrometers. Die pick-and-place uses a vacuum collet to pick each known good die from the dicing tape and place it face-down on the substrate with bumps aligned to the substrate pads. Placement accuracy is typically plus or minus 5-10 micrometers for C4 bumps and plus or minus 2-3 micrometers for fine-pitch copper pillars. Mass reflow passes the assembly through a reflow oven with a controlled temperature profile (preheat at 150-200 degrees Celsius for 60-120 seconds, ramp to peak at 240-260 degrees Celsius for 30-60 seconds, then cool-down) to melt the solder and form metallurgical bonds. Flux cleaning removes flux residue using aqueous or semi-aqueous cleaning agents. Underfill dispensing applies capillary underfill along one or two die edges, which flows beneath the die by capillary action. Underfill cure at 150-165 degrees Celsius for 30-120 minutes crosslinks the thermoset epoxy. Lid attach bonds the heat spreader (lid) to the substrate using perimeter adhesive, with TIM1 between die and lid. BGA ball attach places solder balls on the substrate bottom using flux printing, ball placement, and reflow. Marking applies the part number and lot code by laser or ink. Final singulation separates individual packages if the substrate was processed in strip or panel format.

---

### Q2. How does thermocompression bonding differ from mass reflow?

**Answer:**

Thermocompression bonding (TCB) and mass reflow are both methods for creating solder interconnections between die and substrate, but they differ fundamentally in approach. Mass reflow places the die on the substrate with flux, then heats the entire assembly uniformly in a convection oven. All solder joints melt and solidify simultaneously, with self-alignment provided by the surface tension of molten solder. Mass reflow is high-throughput (hundreds of units reflowed simultaneously on a strip or panel) and low-cost, making it the default method for standard flip-chip assembly at bump pitches of 100+ micrometers. TCB uses a specialized bonding tool (heated bond head) that picks up one die, heats it, and presses it onto the substrate with controlled force (10-200 N) and temperature (250-350 degrees Celsius). Only the area beneath the bonding tool is heated, and the force ensures all bumps contact the substrate pads regardless of warpage. TCB is required for fine-pitch bumps (below approximately 50 micrometers) because the reduced solder volume and tight spacing make mass reflow prone to bridging and non-wet opens. TCB offers precise control over bond line thickness (the force maintains a defined standoff), tolerance for substrate warpage (the tool force flattens the local area), and the ability to use pre-applied underfill (non-conductive paste or film that cures during bonding). The primary disadvantage is throughput: TCB bonds one die at a time, with cycle times of 3-15 seconds per die, versus mass reflow of an entire panel. TCB equipment costs $2-5 million per tool. TCB is used for HBM stacking, microbump-based 2.5D assembly, and 3D die-on-die bonding.

---

### Q3. What is the wafer dicing process and what are the options?

**Answer:**

Wafer dicing singulates the processed wafer into individual die. Mechanical blade dicing uses a thin diamond-impregnated blade (25-50 micrometer kerf width) spinning at 30,000-60,000 RPM to cut through the wafer along scribe lines. The wafer is mounted on adhesive dicing tape stretched over a frame, and the saw cuts through the silicon and partially into the tape. Blade dicing is the most mature and cost-effective method, suitable for standard silicon thickness (200-775 micrometers) and die sizes above approximately 1 mm x 1 mm. Laser dicing uses a focused laser beam (typically Nd:YAG at 355 nm) to cut or scribe the wafer. Laser ablation dicing vaporizes material along the cut line and is useful for thin wafers, brittle materials, and complex scribe line layouts. Stealth dicing uses a pulsed laser focused inside the silicon to create a modified layer (internal stress points) without surface damage; the wafer is then mechanically expanded to separate the die along the modified layer. This produces cleaner die edges with less chipping and no kerf loss, but is limited to silicon and certain thicknesses. Plasma dicing (also called DRIE dicing) uses deep reactive ion etching to etch through the wafer. It can achieve zero kerf (no wasted silicon between die), can dice any die shape (not limited to straight lines), and produces smooth die edges. Plasma dicing is ideal for very small die, irregular shapes, and thin wafers (below 100 micrometers). The choice depends on die size, wafer thickness, material, edge quality requirements, and cost. Most high-volume production uses mechanical blade dicing, with laser and plasma dicing used for specialized applications.

---

### Q4. How does the reflow soldering profile work?

**Answer:**

The reflow profile is a precisely controlled temperature-versus-time curve that the assembly follows as it passes through the reflow oven. For lead-free SAC305 solder (melting point 217-220 degrees Celsius), a typical profile has four phases. The preheat ramp raises the assembly temperature from room temperature to approximately 150-200 degrees Celsius at a rate of 1-3 degrees per second. This gradual heating activates the flux, evaporates volatile solvents, and equalizes temperature across the assembly to minimize thermal shock and warpage. The thermal soak holds the temperature at 150-200 degrees Celsius for 60-120 seconds, allowing the flux to clean oxide from the solder and pad surfaces and further equalizing temperature. The reflow ramp raises the temperature above the solder liquidus (217 degrees Celsius) to a peak of 240-260 degrees Celsius. The time above liquidus (TAL) should be 40-90 seconds -- long enough for the solder to wet the pads and form intermetallic compounds, but not so long that excessive IMC growth occurs or components are damaged. The peak temperature should not exceed 260 degrees Celsius to avoid damage to the molding compound, substrate, and temperature-sensitive components. The cool-down phase lowers the temperature at 2-4 degrees per second. Faster cooling produces finer solder grain structure (better fatigue resistance) but increases thermal stress. The reflow profile must be optimized for each package design, considering the thermal mass of the assembly, the component temperature ratings, and the solder alloy characteristics. Convection reflow ovens with 8-12 independently controlled heating zones and 2-3 cooling zones enable precise profile control. Profile verification uses thermocouples attached to the assembly at critical locations (die surface, corner bumps, board edge).

---

### Q5. What is the underfill dispensing process?

**Answer:**

Underfill dispensing fills the gap between the flip-chip die and substrate with thermoset epoxy to improve solder joint reliability. The standard capillary underfill process begins after flux cleaning following reflow. A precision dispense needle (150-300 micrometer inner diameter) deposits a line or L-shape of liquid underfill along one or two edges of the die. The substrate is heated on a hot plate to 60-110 degrees Celsius to reduce the underfill viscosity and accelerate capillary flow. Capillary action draws the underfill beneath the die, filling the gap (typically 30-100 micrometers) between the die and substrate surface, flowing around all solder bumps and filling the spaces between them. The flow front progresses from the dispense edge toward the opposite edge; complete fill typically takes 10-60 seconds depending on die size, gap height, temperature, and underfill viscosity. After the underfill has flowed to fill the gap, additional underfill may be dispensed to form a fillet around the die perimeter, which reduces stress concentration at the die edge. The assembly is then cured in an oven at 150-165 degrees Celsius for 30-120 minutes (snap cure underfills can cure in 5-10 minutes). Key process challenges include achieving complete fill without voids (voids act as stress concentrators and reduce reliability), controlling the fillet shape (too much underfill can encroach on nearby components), and managing dispense-to-cure time (the underfill must not gel before fill is complete). For high-volume production, the dispense-flow-cure cycle adds several minutes per unit, which can be a throughput bottleneck. Pre-applied underfill alternatives (non-conductive paste applied before die placement, or wafer-applied underfill films) eliminate the post-reflow dispense step.

---

### Q6. What is the molding process for semiconductor packages?

**Answer:**

Molding encapsulates the die, wire bonds, and substrate top surface in a protective epoxy molding compound (EMC). The dominant method is transfer molding, used for over 90 percent of molded packages. In transfer molding, the substrate strip with bonded die is placed in a steel mold cavity. A solid EMC pellet is placed in a transfer pot and heated to 170-180 degrees Celsius, where it melts and becomes a viscous liquid. A plunger presses the molten EMC from the pot through runners and gates into the mold cavities, filling around the die and wire bonds. The mold is held at temperature for 60-120 seconds (cure time) while the epoxy crosslinks and solidifies. The mold opens, and the molded strip is removed. Post-mold cure at 175 degrees Celsius for 4-8 hours completes the crosslinking reaction. Compression molding is an alternative used for thin packages and wafer-level molding. A measured amount of liquid or granular EMC is placed on the substrate, and a flat mold platen descends to compress the compound into the desired thickness. Compression molding applies lower flow-induced forces than transfer molding, reducing wire sweep and die shift in sensitive packages. It is commonly used for fan-out reconstituted wafer molding and thin BGA packages. Critical molding parameters include mold temperature, transfer pressure, transfer speed, and compound viscosity. Void-free filling requires careful optimization of gate location and size, vent placement, and transfer profile. Wire sweep (lateral displacement of wires by EMC flow) must be kept below 5-10 percent of wire span to avoid shorts. Mold flash (thin EMC film on the substrate edges or pads) must be removed by deflashing.

---

### Q7. How are BGA solder balls attached?

**Answer:**

BGA solder ball attachment forms the second-level interconnect between the package and the PCB. The process begins with flux printing: a stencil (metal foil with apertures matching the ball pad locations) is aligned over the substrate bottom surface, and solder paste flux is printed through the apertures onto each pad. The flux provides tackiness to hold the balls in place and cleans oxide from the pad surface during reflow. Ball placement uses a ball placement tool with a vacuum template that picks up an array of pre-formed solder spheres (typically SAC305, 0.3-0.76 mm diameter) from a tray and places them onto the fluxed pads. Alternatively, a ball raining process drops balls into apertures in a stencil aligned over the substrate, relying on gravity and vibration to seat one ball per opening. Reflow in a convection oven (using a profile similar to die attach reflow but optimized for ball attachment) melts the solder balls, which wet to the substrate pads and form spherical solder joints. The surface tension of the molten solder provides self-alignment, correcting minor placement errors. Post-reflow inspection uses automated optical inspection (AOI) and X-ray inspection to verify ball presence, shape, and alignment. Critical defects include missing balls (vacuum pickup failure or placement error), bridged balls (adjacent balls touching), misaligned balls, and non-wet balls (ball did not bond to pad due to oxide or contamination). Ball co-planarity (the variation in ball height across the package) must be within specification (typically plus or minus 50-100 micrometers) to ensure all balls contact the PCB pads during board-level reflow.

---

### Q8. What quality controls are applied during assembly?

**Answer:**

Assembly quality is maintained through in-process controls, inspections, and statistical process control (SPC). Die attach quality is verified by post-bond inspection: high-magnification cameras on the bonder verify bump alignment immediately after placement (in-situ metrology). After reflow, X-ray inspection checks for solder bridging, non-wet opens, voiding in bumps, and bump alignment. Scanning acoustic microscopy (SAM) detects delamination between the die, underfill, and substrate by imaging ultrasonic reflection from internal interfaces; voids and delamination appear as bright features against a dark bonded background. Wire bond quality is monitored through bond shear testing (destructive test on sample units, measuring the force to shear a ball bond from the pad; typical specification is greater than 6-12 grams per bond) and wire pull testing (pulling the wire loop until failure; specification is greater than 3-5 grams). Molding quality is checked by visual inspection for voids, flash, and incomplete fill, and by SAM for internal voids and delamination. BGA ball quality is verified by co-planarity measurement, ball diameter measurement, and X-ray for internal voids. SPC charts track critical parameters (bump height, placement accuracy, reflow peak temperature, underfill fillet size) across production lots. Cpk (process capability index) targets are typically greater than 1.33 for critical parameters and greater than 1.67 for high-reliability products. Out-of-control conditions trigger lot hold and root cause investigation. Traceability systems record processing parameters, material lot numbers, and inspection results for each substrate strip, enabling defect tracking and quality improvement.

---

### Q9. How is die preparation performed before assembly?

**Answer:**

Die preparation encompasses all steps between wafer fabrication and die bonding that prepare the die for package assembly. Wafer backgrinding thins the wafer from its as-processed thickness (775 micrometers for 300 mm wafers) to the target die thickness (50-400 micrometers depending on the package type). The wafer is mounted face-down on a protective grinding tape, and the backside is ground using progressively finer abrasive wheels (coarse grind followed by fine grind and optionally polishing). Back-grinding introduces subsurface damage (microcracks extending 5-20 micrometers into the silicon) that must be removed by stress relief processes: dry polishing, wet etching, or plasma etching removes the damaged layer to prevent die cracking. Wafer-level bumping (if not performed at the wafer fab) deposits UBM and solder bumps on the die pads. For copper pillar bumps, the process involves seed layer deposition, photoresist patterning, copper electroplating, solder cap plating, resist strip, and seed etch. Wafer probing tests each die electrically to identify known good die; the test results are stored in a wafer map that guides the die bonder to pick only good die. Dicing singulates the wafer into individual die. After dicing, the die remain on the dicing tape in their wafer positions. The die bonder reads the wafer map, picks each good die from the tape, inspects it optically for cracks and contamination, and places it on the substrate. For some processes, the die are transferred from dicing tape to a carrier tape or waffle pack for interim storage and transport.

---

### Q10. What are the challenges of assembling multi-die packages?

**Answer:**

Multi-die package assembly introduces several challenges beyond single-die packaging. Sequential die bonding requires placing multiple die on the same substrate, often with different bump pitches, die sizes, and bonding conditions. Each subsequent die must be bonded without disturbing previously bonded die, which constrains the reflow temperature and time for each step. For TCB-based assembly, the thermal exposure of the first die during second-die bonding must not cause solder re-melt or underfill degradation. The assembly sequence must be optimized: typically the tallest die are bonded first, and the bonding tool must accommodate height differences. Placement accuracy requirements compound: each die must be accurately placed relative to the substrate alignment features and relative to previously placed die (for die-to-die alignment in interposer-based packages, accuracy of plus or minus 1-2 micrometers is required). Underfill for multi-die can be applied after all die are bonded (if the gaps are accessible) or sequentially after each die bonding. Yield compound risk means that a single defective die or bonding failure scraps all the good die already assembled. This makes KGD testing critical and motivates investment in rework capabilities. For HBM-on-interposer assembly, the HBM stacks (which themselves are pre-assembled 3D stacks) must be bonded to the interposer alongside the logic die; the HBM stacks are tall (approximately 700 micrometers) and the interposer must accommodate the height difference. Warpage management becomes more critical because the asymmetric die placement and sequential thermal steps create complex stress states.

---

## Further Reading

- [Package Testing](package_testing.md)
- [Yield and Reliability](yield_and_reliability.md)
- [Die-to-Package Interconnect](../01_foundations/die_to_package_interconnect.md)
- [Warpage and Stress](../04_thermal_and_mechanical/warpage_and_stress.md)
