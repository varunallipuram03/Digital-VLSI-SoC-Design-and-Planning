
# Day 2 - Good Floorplan vs Bad Floorplan and Introduction to Library Cells

## Overview

The focus of Day 2 was understanding the importance of floorplanning in the ASIC physical design flow and studying the characteristics of standard library cells. Floorplanning serves as the foundation for successful placement, routing, timing closure, and power distribution. A well-designed floorplan improves chip performance and reduces congestion, while a poor floorplan can introduce routing challenges, timing violations, and power integrity issues.

This session also introduced standard library cells, their design methodology, characterization process, and their role in modern digital integrated circuit design.

---

## Understanding the Netlist

After synthesis, the RTL description is converted into a gate-level netlist. A netlist represents the complete connectivity information of the design and describes how logic gates, flip-flops, buffers, inputs, and outputs are interconnected.

Unlike RTL code, which describes functionality, a netlist describes the actual implementation structure using standard cells available in the technology library.

### Design Connectivity Representation

<p align="center">
  <img src="images/design_connectivity.png" width="800">
</p>

<p align="center">
  <b>Figure 1:</b> design_connectivity
</p>

The generated netlist becomes the primary input for physical design tools and forms the basis for floorplanning and placement.

### Key Observations

* RTL is transformed into a gate-level netlist after synthesis.
* Netlists contain structural connectivity information.
* Physical design tools use the netlist for placement and routing.

---

## Introduction to Floorplanning

Floorplanning is the first stage of physical design where the physical dimensions and organization of the chip are defined. During this stage, the die area, core area, macro placement, routing resources, and power planning structures are established.

A good floorplan ensures efficient utilization of silicon area while providing sufficient routing resources and power distribution paths.

<p align="center">
  <img src="images/picorv32a.floorplan.def.png" width="800">
</p>

<p align="center">
  <b>Figure 2:</b> picorv32a.floorplan_structure
</p>

### Importance of Floorplanning

* Defines chip dimensions and layout boundaries.
* Determines available placement area.
* Establishes routing resources.
* Enables efficient power distribution.
* Reduces congestion and timing violations.

---

## Core Area and Die Area

The complete silicon chip is referred to as the **Die**, while the region allocated for standard cell placement is known as the **Core Area**.

The remaining area is generally used for:

* Input and Output Pads
* Power Rings
* Routing Channels
* Clock Distribution Resources

A proper balance between die area and core area is necessary to achieve optimal utilization and routing efficiency.

## Utilization Factor

Utilization factor indicates how much of the available core area is occupied by standard cells.

**Formula:**

```text
Utilization Factor = Area Occupied by Standard Cells / Total Core Area
```

### Example

* Core Area = 100 units²
* Standard Cell Area = 60 units²

**Utilization Factor = 60 / 100 = 60%**

### Utilization Concept
### Learning Points

* Low utilization wastes silicon area.
* High utilization causes routing congestion.
* Balanced utilization improves routability and timing closure.
* Utilization factor directly impacts placement quality.

---

## Aspect Ratio

Aspect ratio defines the relationship between the height and width of the core area.

**Formula:**

```text
Aspect Ratio = Height / Width
```

### Example

* Height = 100 μm
* Width = 100 μm

**Aspect Ratio = 1**

### Aspect Ratio Illustration

The aspect ratio influences routing complexity, wire length, congestion distribution, and placement quality.

### Learning Points

* Square floorplans generally have an aspect ratio of 1.
* Aspect ratio affects routing resources.
* Improper aspect ratio can create congestion hotspots.
* It influences timing and routing efficiency.

---

## Good Floorplan vs Bad Floorplan

The quality of a floorplan directly determines the success of later physical design stages such as placement, clock tree synthesis, routing, timing closure, and power optimization. A well-planned floorplan improves routability and performance, whereas a poorly planned floorplan can create severe congestion, timing violations, and power integrity issues.

### Characteristics of a Good Floorplan

A good floorplan is designed to achieve balanced utilization and efficient resource distribution across the chip.

- Uniform standard cell distribution throughout the core area.
- Adequate routing resources available for signal interconnections.
- Proper placement of macros and pre-placed cells.
- Balanced utilization factor to avoid congestion.
- Efficient power distribution network (PDN).
- Reduced wire lengths and improved timing performance.
- Better routability and easier timing closure.

### Characteristics of a Bad Floorplan

A bad floorplan often leads to implementation challenges during placement and routing.

- Uneven distribution of standard cells.
- Excessive routing congestion in specific regions.
- Poor macro placement causing routing blockages.
- Long interconnect lengths resulting in increased delay.
- Increased IR drop and power integrity issues.
- Difficulty in achieving timing closure.
- Higher risk of design rule violations.

### Floorplan Comparison

<p align="center">
  <img src="images/good_vs_bad_floorplan.png" width="850">
</p>

<p align="center">
  </b> Figure 3: Comparison between a Good Floorplan and a Bad Floorplan
</p>

### Key Learning Outcome

A properly planned floorplan serves as the foundation of successful ASIC implementation. By optimizing utilization, aspect ratio, macro placement, routing resources, and power planning early in the design flow, designers can significantly improve chip performance, reduce congestion, and achieve reliable timing closure.

## Pre-Placed Cells

Certain large blocks cannot be freely moved during placement and must be positioned during floorplanning.

Examples include:

* SRAM Macros
* PLL Blocks
* Analog IPs
* Clock Generation Circuits

### Macro Placement
The placement of these macros affects routing resources and timing performance across the entire chip.
<p align="center">
  <img src="images/picorv32a.placement.def.png" width="800">
</p>

<p align="center">
  <b>Figure 4:</b> picorv32a.placement_structure
</p>
---
## Decoupling Capacitors (Decap Cells)

Rapid switching activity inside digital circuits causes sudden current demands from the power supply network.

To stabilize the supply voltage, decoupling capacitors are inserted near critical circuit blocks. These capacitors act as local charge reservoirs and provide immediate current whenever a circuit experiences sudden switching activity.

### Benefits of Decoupling Capacitors

* Reduce voltage fluctuations.
* Provide transient current support.
* Improve power integrity.
* Reduce supply noise.
* Improve reliability of operation.

---
## Power Planning

After floorplanning, the next critical step is power planning. Every standard cell, macro, and functional block within the design requires a stable supply voltage to operate correctly. The objective of power planning is to build a robust **Power Distribution Network (PDN)** that can deliver power uniformly across the entire chip while minimizing voltage fluctuations.

A poorly designed power network can result in excessive IR drop, timing violations, signal integrity issues, and even functional failures. Therefore, power planning plays a significant role in ensuring the reliability and performance of the final silicon.

### Power Distribution Network (PDN)

The PDN is composed of several interconnected structures:

* **VDD Rails** – Supply voltage distribution lines.
* **VSS Rails** – Ground distribution lines.
* **Power Rings** – Rings surrounding the core area to provide uniform power delivery.
* **Vertical Power Straps** – Vertical metal routes used for power distribution.
* **Horizontal Power Straps** – Horizontal metal routes used for power distribution.
* **Vias and Contacts** – Connections between different metal layers.

### Power Distribution Network Structure

<p align="center">
  <img src="images/power_planning.png" width="800">
</p>

<p align="center">
  <b>Figure 5:</b> power_planning
</p>
### Benefits of Proper Power Planning

* Improves voltage stability across the chip.
* Reduces IR drop.
* Enhances power integrity.
* Provides uniform current distribution.
* Improves overall design reliability.

---

## Noise Margin

In practical digital circuits, signals are often affected by noise generated from neighboring circuits, power supply fluctuations, or switching activity. Noise margin represents the maximum amount of unwanted noise that a signal can tolerate while still being interpreted correctly as a logic HIGH or logic LOW.

A larger noise margin indicates a more robust and reliable digital system.

### Important Voltage Parameters

* **VOH** – Output High Voltage
* **VOL** – Output Low Voltage
* **VIH** – Minimum Input Voltage recognized as Logic HIGH
* **VIL** – Maximum Input Voltage recognized as Logic LOW

### Noise Margin Representation

<p align="center">
  <img src="images/noise_margin.png" width="800">
</p>

<p align="center">
  <b>Figure 6:</b> noise_margin
</p>
### Why Noise Margin Matters

A small noise margin makes the circuit vulnerable to incorrect logic interpretation, whereas a larger noise margin improves immunity against disturbances and ensures reliable operation under different conditions.

---

## Ground Bounce and Voltage Droop

Modern integrated circuits contain millions of transistors switching simultaneously. These switching events can create temporary disturbances in the power delivery network.

### Ground Bounce

Ground bounce occurs when a large number of transistors switch simultaneously from HIGH to LOW. During this transition, a sudden surge of current flows toward the ground network.

Because the ground path contains parasitic resistance and inductance, the ground potential momentarily rises above its ideal value, creating a phenomenon known as **Ground Bounce**.

### Effects of Ground Bounce

* False switching events
* Logic errors
* Reduced noise margin
* Signal integrity degradation

### Voltage Droop

Voltage droop occurs when many transistors switch simultaneously from LOW to HIGH. This transition requires a large amount of current from the power supply.

Due to the resistance and inductance present in the power network, the supply voltage temporarily decreases below its nominal value.

### Effects of Voltage Droop

* Increased propagation delay
* Timing violations
* Reduced circuit performance
* Functional instability

Understanding both ground bounce and voltage droop is essential when designing high-performance and reliable digital systems.

---

## Introduction to Library Cells

Library cells, also known as standard cells, are the fundamental building blocks used to construct digital integrated circuits. Every synthesized digital design is ultimately implemented using these predefined cells.

Standard cells are designed, verified, and characterized in advance so that EDA tools can accurately predict their timing, power consumption, and physical dimensions.

### Common Standard Cells

* Inverters
* Buffers
* NAND Gates
* NOR Gates
* XOR Gates
* Multiplexers
* Flip-Flops
* Latches

Each standard cell contains detailed information regarding:

* Functionality
* Physical layout
* Timing characteristics
* Power characteristics
* Pin definitions

---

## Standard Cell Design Flow

Before a standard cell can be included in a technology library, it must pass through a well-defined design and verification flow.

### Standard Cell Design Flow

<p align="center">
  <img src="images/cell_design_flow.png" width="800">
</p>

<p align="center">
  <b>Figure 7:</b> cell_design_flow
</p>

### Inputs Required

* Process Design Kit (PDK)
* Design Rules
* SPICE Models
* User Specifications

### Design Stages

1. Circuit Design
2. Layout Design
3. Design Rule Check (DRC)
4. Layout Versus Schematic (LVS)
5. Characterization

### Outputs Generated

* GDSII Layout
* CDL Netlist
* LEF File
* Liberty (.lib) File

These outputs become essential inputs for synthesis, placement, routing, and timing analysis tools.

---

## Standard Cell Layout

A standard cell layout represents the physical realization of the transistor-level schematic. The layout must satisfy all manufacturing design rules while maintaining functionality and performance.

### Layout Components

* Diffusion Layers
* Polysilicon Gates
* Contacts
* Vias
* Metal Layers
* Power Rails

### Pin Locations

<p align="center">
  <img src="images/cell_design_flow.png" width="800">
</p>
<p align="center">
  <b>Figure 8:</b> cell_design_flow
</p>

Pin accessibility is an important consideration because it directly affects placement flexibility, routing quality, and timing closure.

---

## Standard Cell Characterization

Once the layout has successfully passed DRC and LVS verification, the standard cell undergoes characterization.

Characterization is the process of extracting electrical information that describes how the cell behaves under different operating conditions.

### Characterization Flow
<p align="center">
  <img src="images/characterization_flow.png" width="800">
</p>

<p align="center">
  <b>Figure 9:</b> characterization_flow
</p>

### Characterization Outputs

* Timing Models
* Power Models
* Noise Models
* Liberty Files (.lib)

These models allow synthesis and timing analysis tools to accurately estimate circuit behavior before fabrication.

---

## Timing Characterization

Timing characterization is performed to determine the delay and transition characteristics of a standard cell.

The extracted timing information is later used by Static Timing Analysis (STA) tools to verify whether the design meets its timing requirements.

### Important Timing Parameters

* Propagation Delay
* Rise Time
* Fall Time
* Input Slew
* Output Slew

### Timing Threshold Definitions

<p align="center">
  <img src="images/timing_thresholds.png" width="800">
</p>

<p align="center">
  <b>Figure 10:</b> timing_thresholds
</p>
Timing measurements are typically calculated using predefined voltage thresholds such as 20%, 50%, and 80% of the supply voltage.

### Propagation Delay Measurement

<p align="center">
  <img src="images/propagation_delay.png" width="800">
</p>

<p align="center">
  <b>Figure 11:</b> propagation_delay
</p>
Propagation delay is calculated as the difference between the output threshold crossing time and the corresponding input threshold crossing time.

Accurate delay measurement is critical because it directly impacts timing closure and overall circuit performance.

---

## Conclusion

Day 2 provided a comprehensive understanding of floorplanning fundamentals and their impact on ASIC implementation. Concepts such as utilization factor, aspect ratio, macro placement, power planning, decoupling capacitors, noise margin, voltage droop, and ground bounce highlighted the importance of proper physical design planning.

The session also introduced standard library cells, their design methodology, layout implementation, and characterization process. Understanding these concepts is essential because standard cells form the foundation of modern digital integrated circuits and directly influence the timing, power, and area characteristics of the final design.
Overall, this session established the groundwork for understanding placement, routing, timing optimization, and advanced physical design concepts in subsequent stages of the ASIC design flow.
