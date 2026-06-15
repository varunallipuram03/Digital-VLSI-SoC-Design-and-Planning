# Day 2 - Good Floorplan vs Bad Floorplan and Introduction to Library Cells

## Overview

The focus of Day 2 was understanding the importance of floorplanning in the ASIC physical design flow and studying the characteristics of standard library cells. Floorplanning serves as the foundation for successful placement, routing, timing closure, and power distribution. A well-designed floorplan improves chip performance and reduces congestion, while a poor floorplan can introduce routing challenges, timing violations, and power integrity issues.

This session also introduced standard library cells, their design methodology, characterization process, and their role in modern digital integrated circuit design.

---

## Understanding the Netlist

After synthesis, the RTL description is converted into a gate-level netlist. A netlist represents the complete connectivity information of the design and describes how logic gates, flip-flops, buffers, inputs, and outputs are interconnected.

Unlike RTL code, which describes functionality, a netlist describes the actual implementation structure using standard cells available in the technology library.

### Design Connectivity Representation

> Insert the Design Connectivity screenshot here

```markdown
![Design Connectivity](images/design_connectivity.png)
```

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

> Insert the Floorplan Structure screenshot here

```markdown
![Floorplan Structure](images/floorplan_structure.png)
```

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

### Core and Die Representation

> Insert the Core Area and Die Area image here

```markdown
![Core and Die Area](images/core_die_area.png)
```

---

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

> Insert the Utilization Factor image here

```markdown
![Utilization Factor](images/utilization_factor.png)
```

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

> Insert the Aspect Ratio image here

```markdown
![Aspect Ratio](images/aspect_ratio.png)
```

The aspect ratio influences routing complexity, wire length, congestion distribution, and placement quality.

### Learning Points

* Square floorplans generally have an aspect ratio of 1.
* Aspect ratio affects routing resources.
* Improper aspect ratio can create congestion hotspots.
* It influences timing and routing efficiency.

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

> Insert the Good Floorplan vs Bad Floorplan image here

```markdown
![Good vs Bad Floorplan](images/good_vs_bad_floorplan.png)
```

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

> Insert the Pre-Placed Cells image here

```markdown
![Preplaced Cells](images/preplaced_cells.png)
```

The placement of these macros affects routing resources and timing performance across the entire chip.

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

### Decap Placement Concept

> Insert the Decap Cell image here

```markdown
![Decap Cells](images/decap_cells.png)
```

---
