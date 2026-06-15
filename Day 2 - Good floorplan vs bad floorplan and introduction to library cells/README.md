# Day 2 - Good Floorplan vs Bad Floorplan and Introduction to Library Cells

## Overview

The focus of Day 2 was understanding the importance of floorplanning in the ASIC physical design flow and studying the characteristics of standard library cells. Floorplanning serves as the foundation for successful placement, routing, timing closure, and power distribution. A well-designed floorplan improves chip performance and reduces congestion, while a poor floorplan can introduce routing challenges, timing violations, and power integrity issues.

This session also introduced standard library cells, their design methodology, characterization process, and their role in modern digital integrated circuit design.

---

## Understanding the Netlist

After synthesis, the RTL description is converted into a gate-level netlist. A netlist represents the complete connectivity information of the design and describes how logic gates, flip-flops, buffers, inputs, and outputs are interconnected.

Unlike RTL code, which describes functionality, a netlist describes the actual implementation structure using standard cells available in the technology library.

### Design Connectivity Representation

![Design Connectivity](images/netlist_connectivity.png)

The generated netlist becomes the primary input for physical design tools and forms the basis for floorplanning and placement.

### Key Observations

* RTL is transformed into a gate-level netlist after synthesis.
* Netlists contain structural connectivity information.
* Physical design tools use the netlist for placement and routing.

---

## Introduction to Floorplanning

Floorplanning is the first stage of physical design where the physical dimensions and organization of the chip are defined. During this stage, the die area, core area, macro placement, routing resources, and power planning structures are established.

A good floorplan ensures efficient utilization of silicon area while providing sufficient routing resources and power distribution paths.

### Floorplan Structure

![Floorplan](images/floorplan_structure.png)

### Importance of Floorplanning

* Defines chip dimensions and layout boundaries.
* Determines available placement area.
* Establishes routing resources.
* Enables efficient power distribution.
* Reduces congestion and timing violations.

---

## Core Area and Die Area

The complete silicon chip is referred to as the **die**, while the region allocated for standard cell placement is known as the **core area**.

The remaining area is generally used for:

* Input and Output pads
* Power rings
* Routing channels
* Clock distribution resources

A proper balance between die area and core area is necessary to achieve optimal utilization and routing efficiency.

---

## Utilization Factor

Utilization factor indicates how much of the available core area is occupied by standard cells.

[
Utilization\ Factor = \frac{Area\ Occupied\ by\ Standard\ Cells}{Total\ Core\ Area}
]

For example:

* Core Area = 100 units²
* Standard Cell Area = 60 units²

Utilization Factor = 60%

### Utilization Concept

![Utilization Factor](images/utilization_factor.png)

### Learning Points

* Low utilization wastes silicon area.
* High utilization causes routing congestion.
* Balanced utilization improves routability and timing closure.

---

## Aspect Ratio

Aspect ratio defines the relationship between the height and width of the core area.

[
Aspect\ Ratio = \frac{Height}{Width}
]

A square floorplan has an aspect ratio equal to 1, while rectangular floorplans have values greater or less than 1.

### Aspect Ratio Illustration

![Aspect Ratio](images/aspect_ratio.png)

The aspect ratio influences routing complexity, wire length, congestion distribution, and placement quality.

---

## Good Floorplan vs Bad Floorplan

The quality of a floorplan directly impacts the success of subsequent physical design stages.

### Characteristics of a Good Floorplan

* Balanced utilization factor.
* Uniform cell distribution.
* Sufficient routing resources.
* Proper macro placement.
* Efficient power distribution network.
* Reduced congestion hotspots.

### Characteristics of a Bad Floorplan

* Uneven cell placement.
* Excessive routing congestion.
* Long interconnect lengths.
* Poor macro positioning.
* Increased timing and power challenges.

### Floorplan Comparison

![Good vs Bad Floorplan](images/good_vs_bad_floorplan.png)

### Learning Outcome

A properly planned floorplan significantly improves timing closure, routing efficiency, and overall chip performance.

---

## Pre-Placed Cells

Certain large blocks cannot be freely moved during placement and must be positioned during floorplanning.

Examples include:

* SRAM Macros
* PLL Blocks
* Analog IPs
* Clock Generation Circuits

### Macro Placement

![Preplaced Cells](images/preplaced_cells.png)

The placement of these macros affects routing resources and timing performance across the entire chip.

---

## Decoupling Capacitors

Rapid switching activity inside digital circuits causes sudden current demands from the power supply network.

To stabilize the supply voltage, decoupling capacitors are inserted near critical circuit blocks.

### Benefits of Decoupling Capacitors

* Reduce voltage fluctuations.
* Provide transient current support.
* Improve power integrity.
* Reduce supply noise.

### Decap Placement Concept

![Decap Cells](images/decap_cells.png)

---

## Power Planning

Power planning ensures reliable distribution of power throughout the chip.

The Power Distribution Network (PDN) consists of:

* VDD Rails
* VSS Rails
* Power Rings
* Vertical Power Straps
* Horizontal Power Straps
* Vias and Contacts

### Power Distribution Network

![Power Planning](images/power_planning.png)

A robust PDN minimizes IR drop and ensures reliable circuit operation.

### Benefits

* Improved voltage stability.
* Reduced IR drop.
* Better reliability.
* Uniform current distribution.

---

## Noise Margin

Noise margin represents the maximum amount of noise that a digital signal can tolerate without causing incorrect logic interpretation.

Important voltage parameters include:

* VOH
* VOL
* VIH
* VIL

### Noise Margin Representation

![Noise Margin](images/noise_margin.png)

A larger noise margin improves circuit robustness and reliability.

---

## Ground Bounce and Voltage Droop

### Ground Bounce

Ground bounce occurs when multiple transistors switch simultaneously from HIGH to LOW, causing temporary fluctuations in the ground potential.

Effects include:

* False switching events.
* Logic errors.
* Reduced reliability.

### Voltage Droop

Voltage droop occurs when a large number of gates switch simultaneously from LOW to HIGH, creating a sudden demand for current.

Effects include:

* Increased delay.
* Timing violations.
* Reduced performance.

Understanding these effects is essential for designing reliable power delivery networks.

---

## Introduction to Library Cells

Library cells are the fundamental building blocks used to implement digital circuits.

Typical library cells include:

* Inverters
* Buffers
* NAND Gates
* NOR Gates
* XOR Gates
* Multiplexers
* Flip-Flops
* Latches

Each cell is carefully designed, verified, and characterized before being included in the standard cell library.

---

## Standard Cell Design Flow

The development of a standard cell follows a structured design methodology.

### Standard Cell Design Flow

![Cell Design Flow](images/cell_design_flow.png)

### Inputs

* Process Design Kit (PDK)
* Design Rules
* SPICE Models
* User Specifications

### Design Stages

1. Circuit Design
2. Layout Design
3. DRC Verification
4. LVS Verification
5. Characterization

### Outputs

* GDSII Layout
* CDL Netlist
* LEF File
* Liberty (.lib) File

---

## Standard Cell Layout

The physical implementation of a standard cell is represented through its layout.

### Standard Cell Layout

![Standard Cell Layout](images/standard_cell_layout.png)

The layout contains:

* Diffusion Layers
* Poly Gates
* Contacts
* Vias
* Metal Layers
* Power Rails

### Pin Locations

![Pin Locations](images/pin_locations.png)

Pin accessibility is an important factor because it directly affects routing quality.

---

## Standard Cell Characterization

After layout verification, characterization is performed to extract timing, power, and noise information.

### Characterization Flow

![Characterization Flow](images/characterization_flow.png)

Characterization generates:

* Timing Models
* Power Models
* Noise Models
* Liberty Files

These models are later used during synthesis, placement, routing, and static timing analysis.

---

## Timing Characterization

Timing characterization determines important timing parameters used by EDA tools.

### Timing Parameters

* Propagation Delay
* Rise Time
* Fall Time
* Input Slew
* Output Slew

### Timing Threshold Definitions

![Timing Thresholds](images/timing_thresholds.png)

### Propagation Delay Measurement

![Propagation Delay](images/propagation_delay.png)

Propagation delay is calculated as the difference between the output threshold crossing time and the corresponding input threshold crossing time.

The extracted timing information is stored in Liberty files and is used for static timing analysis.

---

## Conclusion

Day 2 provided a detailed understanding of floorplanning fundamentals and the role of standard library cells in ASIC implementation. Concepts such as utilization factor, aspect ratio, macro placement, power planning, decoupling capacitors, noise margin, voltage droop, and ground bounce highlighted the importance of proper physical design planning. The session also introduced the standard cell design flow, layout methodology, and characterization process, providing insight into how library cells are developed and prepared for use in modern ASIC design flows.
