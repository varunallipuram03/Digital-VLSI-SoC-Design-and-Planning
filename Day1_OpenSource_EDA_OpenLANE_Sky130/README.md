## Day 1 - Inception of Open-Source EDA, OpenLANE and SKY130 PDK

### Author
**Varun Venkata Allipuram**

### Workshop
VSD OpenLANE SKY130 Workshop

### Day Objectives

- Understand the ASIC Design Flow
- Learn OpenLANE Architecture
- Understand SKY130 PDK
- Study RISC-V and Open-Source Hardware
- Run Synthesis using OpenLANE
- Analyze Synthesis Results
---
## Table of Contents

1. Introduction
2. Software to Hardware Flow
3. Chip Components and SoC Architecture
4. RISC-V ISA
5. Open-Source ASIC Design
6. RTL to GDSII Flow
7. OpenLANE Flow
8. SKY130 PDK
9. Practical Lab
10. Synthesis Results
11. Key Learnings

---
# 1️. Understanding Computer Abstraction Layers

## Why Do We Need Abstraction?

Humans communicate using programming languages, while computers understand only binary values (0 and 1). To bridge this gap, multiple abstraction layers are used, where each layer translates information into a form understandable by the next layer.

## Abstraction Flow

```text
High-Level Language (C, Python, Java)
            ↓
Compiler
            ↓
Assembly Language
            ↓
Assembler
            ↓
Machine Code
            ↓
Processor Hardware
            ↓
Transistor Switching
```

## Role of Each Layer
* **High-Level Language:** Allows programmers to write applications without worrying about hardware details.
* **Compiler:** Converts source code into assembly instructions.
* **Assembly Language:** Human-readable representation of machine instructions.
* **Machine Code:** Binary instructions executed directly by the processor.
* **ISA (Instruction Set Architecture):** Acts as a bridge between software and hardware by defining instructions, registers, and memory operations.
## Key Learning
The ISA is the fundamental interface between software and hardware. As VLSI engineers, our responsibility is to design hardware that correctly implements the ISA specifications.

<p align="center">
  <img src="images/software_to_hardware_flow.png" width="700">
</p>
<p align="center"><b>Figure 1:</b> Software to Hardware Flow</p>

## 2️. Chip Anatomy – Understanding the Building Blocks of a Chip

Before learning the OpenLANE flow, it is important to understand what actually exists inside a semiconductor chip.

A chip is not just a small black component that we see on a PCB. Internally, it consists of a silicon die enclosed inside a package. The package protects the die and provides electrical connections to the external world.

### Chip Structure

**Chip = Die + Package**

### Package

The package acts as the outer covering of the chip. It performs multiple functions:

* Protects the silicon die from physical damage.
* Provides connections between the chip and the PCB.
* Helps in dissipating the heat generated during operation.

One commonly used package type is **QFN (Quad Flat No-Lead)**, which offers compact size and good thermal performance.

### Die

The die is the actual silicon portion where all electronic circuits are fabricated.

Inside the die we can find:

* Core Region
* I/O Cells
* Memory Blocks
* Analog and Digital IPs

The die is manufactured on a silicon wafer and later separated into individual chips through a process called dicing.

### Core

The core is the working area of the chip where most computations take place.

It generally contains:

* Standard Cells
* Sequential Elements (Flip-Flops)
* SRAM Blocks
* Processor Logic
* Custom Macros

This is the region where the actual functionality of the design is implemented.

### Pads

Pads form the interface between the internal circuitry and the outside world.

Common pad types include:

* Power Pads (VDD, VSS)
* Input Pads
* Output Pads
* Bidirectional Pads
* Analog Pads

These pads are arranged around the chip boundary and connect to the package pins.

### Macros and IP Blocks

Modern chips are built using reusable blocks instead of designing everything from scratch.

Some examples include:

* SRAM
* UART
* SPI
* ADC
* PLL
* Processor Cores

These reusable blocks are known as Intellectual Property (IP) blocks.

IPs are generally categorized as:

| IP Type | Description                                                         |
| ------- | ------------------------------------------------------------------- |
| Soft IP | Provided as RTL and can be synthesized for different technologies   |
| Hard IP | Delivered as a fixed layout and optimized for a specific technology |
| Firm IP | Intermediate form with some flexibility in implementation           |

### My Understanding

From this section, I learned that a complete chip is a combination of several smaller functional blocks working together. The processor is only one part of the system; memories, I/O interfaces, analog circuits, and foundry-provided IPs are equally important in building a complete SoC.

<p align="center">
  <img src="images/soc_macros_foundry_ips.png" width="700">
</p>

<p align="center"><b>Figure 2:</b> SoC Architecture Showing Macros and Foundry IPs</p>

## 3. RISC-V and the ISA Bridge
### What is RISC-V?

RISC-V is an open-source Instruction Set Architecture (ISA) developed at UC Berkeley. Unlike proprietary architectures, it can be used and modified freely, making it popular in academia, research, and industry.

### Why RISC-V?

Some reasons for its growing adoption are:

* Open-source and royalty-free
* Modular architecture with optional extensions
* Easy to learn and implement
* Widely used in embedded and SoC designs

### Understanding the ISA

The ISA acts as a bridge between software and hardware. It defines how software communicates with the processor through instructions.

For example:

```assembly
add x5, x6, x7
```

This instruction tells the processor to add the values stored in registers `x6` and `x7` and store the result in `x5`.

### Key Learning

I understood that the ISA provides a common interface between hardware designers and software developers. As long as the processor follows the RISC-V specification, it can execute RISC-V programs correctly.
## 4. From Software to Silicon

One of the most interesting concepts covered in Day 1 was understanding how a software application finally becomes hardware running on silicon.

### Design Flow

```text
Application
    ↓
Compiler
    ↓
Assembly Code
    ↓
Machine Code
    ↓
Processor
    ↓
RTL Design
    ↓
Synthesis
    ↓
Physical Design
    ↓
Fabricated Chip
```

Each stage transforms the design into a lower-level representation until it becomes an actual silicon implementation.

### Key Learning

A mistake at any stage of this flow can affect the final chip. Understanding the complete path helps in debugging and developing reliable hardware systems.

## 5. Open-Source ASIC Design Ecosystem

Traditional ASIC development relied heavily on expensive commercial tools and proprietary process technologies. The open-source hardware movement has changed this by providing accessible design resources.

### Three Important Pillars

#### 1. Open RTL Designs

Design sources are available from platforms such as:

* OpenCores
* LibreCores
* GitHub repositories

#### 2. Open-Source EDA Tools

Some commonly used tools are:

| Tool     | Purpose              |
| -------- | -------------------- |
| Yosys    | RTL Synthesis        |
| ABC      | Logic Optimization   |
| OpenROAD | Physical Design      |
| OpenSTA  | Timing Analysis      |
| Magic    | Layout and DRC       |
| Netgen   | LVS Verification     |
| KLayout  | Layout Visualization |

#### 3. Open PDK

The SKY130 PDK made it possible for students and researchers to design manufacturable chips using an openly available process technology.

### Key Learning

The combination of open RTL, open EDA tools, and an open PDK has made ASIC design more accessible to the engineering community.
## 6. RTL-to-GDSII Design Flow

One of the key topics covered in Day 1 was understanding how a hardware description written in RTL is converted into a manufacturable chip layout. This complete process is known as the **RTL-to-GDSII Flow**.

<p align="center">
  <img src="images/RTL_to_GDS_flow.png" width="700">
</p>

<p align="center"><b>Figure 3:</b> RTL to GDSII Flow IPs</p>

### 1. Synthesis

The design flow starts with RTL written in Verilog. During synthesis, the RTL code is converted into a gate-level netlist using standard cells available in the technology library.

**Output:** Gate-level Netlist

### 2. Floorplanning

In this stage, the overall chip area is defined. Locations for I/O pins, standard cell rows, and large macros such as SRAM are decided.

**Goal:** Create an efficient layout structure for the design.

### 3. Power Planning

A Power Distribution Network (PDN) is created to ensure reliable delivery of VDD and GND throughout the chip.

Major components include:

* Power Rings
* Power Straps
* Standard Cell Power Rails

### 4. Placement

Standard cells from the synthesized netlist are placed inside the floorplanned area.

Placement is generally performed in two steps:

* Global Placement
* Detailed Placement

Proper placement helps reduce routing congestion and improves timing performance.

### 5. Clock Tree Synthesis (CTS)

CTS builds a balanced clock network that distributes the clock signal to all sequential elements in the design.

**Objective:** Minimize clock skew and ensure synchronized operation of flip-flops.

### 6. Routing

Routing creates physical connections between all placed cells using available metal layers.

Routing is performed in two stages:

* Global Routing
* Detailed Routing

At the end of this stage, all nets are physically connected.

### 7. Sign-Off and Verification

Before fabrication, the design undergoes several verification checks:

* **DRC (Design Rule Check)** – verifies manufacturing rules.
* **LVS (Layout Versus Schematic)** – checks whether layout matches the intended design.
* **STA (Static Timing Analysis)** – verifies timing requirements.

### Final Output

After successful verification, the final layout is generated as a **GDSII file**, which is sent to the fabrication facility for chip manufacturing.

### Key Learning

I learned that chip design is not limited to writing Verilog code. Several physical design stages such as floorplanning, placement, CTS, routing, and verification are required before a design can be fabricated into silicon.

## 7. Understanding OpenLANE

### What is OpenLANE?

OpenLANE is an open-source digital ASIC implementation flow developed by eFabless. Instead of being a single software tool, it combines multiple open-source EDA tools into a complete RTL-to-GDSII design flow.

The main purpose of OpenLANE is to automate the chip design process and reduce manual intervention during physical design.

### OpenLANE Flow
<p align="center">
  <img src="images/OpenLANE_ASIC_Flow.png" width="700">
</p>
<p align="center"><b>Figure 4:</b> OpenLANE Flow IPs</p>

The flow starts with RTL and gradually converts the design into a manufacturable GDSII layout.

### Main Objective

The primary goal of OpenLANE is to generate a **clean GDSII** by automatically performing all major implementation stages.

A successful design should satisfy:

* No DRC violations
* No LVS mismatches
* Acceptable timing performance

### Major Design Stages

```text
RTL Design
    ↓
Synthesis
    ↓
Floorplanning
    ↓
Placement
    ↓
Clock Tree Synthesis
    ↓
Routing
    ↓
Physical Verification
    ↓
GDSII Generation
```

Each stage prepares the design for the next step until the final layout is produced.

### Operating Modes

OpenLANE can be used in two different ways:

| Mode             | Description                                |
| ---------------- | ------------------------------------------ |
| Interactive Mode | Execute and analyze each step individually |
| Automated Mode   | Run the complete flow automatically        |

Interactive mode is particularly useful for learning and debugging, while automated mode is preferred for full-chip execution.

### Antenna Violation Handling

During fabrication, long metal wires may accumulate electrical charge and damage transistor gates. This issue is known as an antenna violation.

OpenLANE automatically detects such violations and inserts antenna protection diodes wherever required, helping improve manufacturing reliability.

### Why OpenLANE is Important

Before the availability of OpenLANE and SKY130, ASIC design was mainly restricted to companies with access to expensive commercial tools.

OpenLANE has made it possible for students, researchers, and hardware enthusiasts to learn and perform complete ASIC implementation using open-source resources.

### Key Learning

From this section, I learned how OpenLANE integrates several EDA tools into a unified flow and automates the major stages of ASIC physical design. It provides a practical platform for transforming RTL designs into fabrication-ready layouts using open-source technology.

## 8. SKY130 PDK – Foundation of Open-Source Chip Design

### What is a PDK?

A **Process Design Kit (PDK)** is a collection of files and design information provided by a semiconductor foundry. It acts as a bridge between circuit design and chip manufacturing.

Without a PDK, designers would not know the fabrication rules, available layers, transistor behavior, or standard cells required to create a working chip.

### Key Components of a PDK

| Component        | Purpose                                                    |
| ---------------- | ---------------------------------------------------------- |
| Design Rules     | Define manufacturing constraints such as spacing and width |
| SPICE Models     | Used to simulate transistor behavior                       |
| Standard Cells   | Pre-designed logic gates and sequential elements           |
| Technology Files | Describe layers, parasitics, and process information       |
| I/O Cells        | Interface the chip with the external world                 |

### Introduction to SKY130

SKY130 is a 130nm CMOS technology node released through a collaboration between SkyWater Technology and Google. It became one of the first production-grade open-source PDKs available to the VLSI community.

The availability of SKY130 has made practical ASIC design accessible to students, researchers, and open-source hardware developers.

### Why SKY130 is Popular

Some reasons why SKY130 is widely used in learning and research are:

* Completely open-source and freely accessible
* Stable and well-documented technology node
* Supported by OpenLANE and OpenROAD flows
* Suitable for digital, analog, and mixed-signal designs
* Enables real silicon fabrication through open MPW programs

### Standard Cell Libraries

SKY130 provides multiple standard cell libraries optimized for different design requirements.

| Library           | Optimization Target |
| ----------------- | ------------------- |
| sky130_fd_sc_hd   | High Density        |
| sky130_fd_sc_hdll | Low Leakage         |
| sky130_fd_sc_hs   | High Speed          |
| sky130_fd_sc_ls   | Low Speed           |
| sky130_fd_sc_ms   | Medium Speed        |

For most OpenLANE experiments, the **High Density (HD)** library is commonly used.

### Understanding Cell Names

Example:

```text
sky130_fd_sc_hd__nand2_1
```

Breakdown:

* **sky130** → SkyWater 130nm technology
* **fd_sc** → Foundry standard cell library
* **hd** → High Density library
* **nand2** → 2-input NAND gate
* **1** → Drive strength

Different drive strengths are available depending on the required performance and load conditions.

### What I Learned

One important takeaway from this section is that the PDK is the backbone of the ASIC design flow. While EDA tools automate the design process, the PDK provides the manufacturing knowledge required to transform a design into a real silicon chip.

The release of SKY130 has significantly contributed to the growth of open-source VLSI design by making professional chip design resources available to everyone.

## 9. Hands-on Lab: OpenLANE Setup and Synthesis Flow

After understanding the theoretical concepts, the next step was to explore the OpenLANE environment and execute the initial stages of the ASIC design flow using the PicoRV32A design.

### Working Environment

The complete flow was executed inside an OpenLANE Docker environment. Using Docker ensures that all required tools, libraries, and dependencies are available in a consistent setup, avoiding configuration issues across different systems.

### Launching the OpenLANE Environment

First, I entered the OpenLANE directory and mounted the Docker container:

```bash
cd ~/Desktop/OpenLane
make mount
```

This command starts the OpenLANE environment and loads the required PDK and standard cell libraries needed for the design flow.

### Starting Interactive Mode

To execute individual stages manually and understand the flow better, I launched OpenLANE in interactive mode:

```bash
./flow.tcl -interactive
```

Interactive mode provides more control over each stage and helps in analyzing intermediate outputs.

### Loading the OpenLANE Package

Inside the OpenLANE shell, the package was loaded using:

```tcl
package require openlane
```

Successful loading confirmed that the OpenLANE environment was ready for execution.

### Preparing the Design

The PicoRV32A design was prepared using:

```tcl
prep -design picorv32a
```

During this stage, OpenLANE:

* Reads the design configuration files
* Loads SKY130 technology information
* Generates the required working directories
* Creates merged LEF files
* Initializes logs, reports, and result folders

This step acts as the foundation for all subsequent design stages.
<p align="center">
  <img src="images/Picorv32_Flow.png" width="700">
</p>

<p align="center"><b>Figure 5:</b> Picorv32_Flow IPs</p>

### Running Synthesis

After preparation, synthesis was executed using:

```tcl
run_synthesis
```
<p align="center">
  <img src="images/prep_design_synthesis.jpeg" width="700">
</p>

<p align="center"><b>Figure 6:</b> prep_design_synthesis IPs</p>

During synthesis:

* RTL code is analyzed by Yosys
* Logic optimization is performed
* Gates are mapped to SKY130 standard cells
* Timing checks are carried out using OpenSTA

The final output is a gate-level netlist which represents the synthesized hardware implementation of the design.

### Key Observation

One interesting aspect of the synthesis stage is that the behavioral Verilog description is transformed into actual standard-cell-based hardware. This is the first stage where the design starts moving from an abstract RTL model toward a manufacturable ASIC implementation.

### Generated Outputs

The synthesis stage produces:

* Gate-level netlist
* Area utilization reports
* Timing analysis reports
* Synthesis statistics

These reports are later used to evaluate design quality before proceeding to floorplanning and physical design stages.

### Learning Outcome

This lab helped me understand how OpenLANE organizes a complete ASIC project and how RTL code is converted into a technology-mapped netlist. It also provided practical exposure to Docker-based EDA environments, OpenLANE commands, and synthesis report generation.

# 10. Synthesis Results and Analysis

After running synthesis on the PicoRV32A design, I examined the generated reports to understand the hardware complexity and resource utilization.

### Synthesis Summary

| Parameter    | Value  |
| ------------ | ------ |
| Total Wires  | 17,043 |
| Wire Bits    | 17,475 |
| Total Cells  | 17,323 |
| D Flip-Flops | 1,614  |

### DFF Percentage

The percentage of flip-flops in the design can be calculated as:

```text
DFF Percentage = (1614 / 17323) × 100
               = 9.32%
```

This indicates that the processor contains a balanced combination of combinational and sequential logic, which is expected for a RISC-V CPU design.

### Observations

* The design contains more combinational logic compared to storage elements.
* A significant number of cells are used for instruction decoding, arithmetic operations, and control logic.
* The synthesis reports provide useful information about area utilization and timing before moving to the physical design stages.

### Important Reports Generated

* Area Report
* Timing Report
* Cell Utilization Report
* Synthesis Statistics Report

These reports help in evaluating the quality of synthesis before floorplanning and placement.

---

# 11. Challenges Faced During Day 1

During the initial setup and execution, I encountered a few common issues.

### Command Case Sensitivity

Linux commands are case-sensitive. Commands such as `CD` and `LS` generated errors because the correct commands are `cd` and `ls`.

### Docker Environment Issues

Initially, the required OpenLANE image was not available locally. Pulling the correct Docker image resolved the issue.

### Running OpenLANE Outside Docker

Some commands failed because OpenLANE tools were executed outside the container environment. Running everything inside Docker fixed the problem.

### Design Preparation Error

An incorrect `prep` command caused configuration errors. Using:

```tcl
prep -design picorv32a
```

resolved the issue.

---

# 12. Quick Interview Questions

### What is a PDK?

A PDK (Process Design Kit) contains all technology files, design rules, libraries, and models required to design a chip for a specific fabrication process.

### What is OpenLANE?

OpenLANE is an open-source RTL-to-GDSII flow that automates ASIC design using multiple open-source EDA tools.

### Why is synthesis important?

Synthesis converts RTL code into a gate-level representation that can be physically implemented on silicon.

### What is PicoRV32A?

PicoRV32A is a lightweight open-source RISC-V processor commonly used as a reference design in OpenLANE tutorials and workshops.

### What is STA?

Static Timing Analysis (STA) is used to verify whether timing requirements are satisfied without running functional simulations.

### What is a Clean GDSII?

A clean GDSII is a layout that passes DRC, LVS, and timing verification checks before fabrication.

---

# 13. Key Takeaways from Day 1

Day 1 provided a solid introduction to the open-source ASIC design ecosystem. I learned how software instructions eventually become hardware implementations, understood the importance of the SKY130 PDK, and explored the OpenLANE design flow.

### Skills Gained

* Understanding ASIC design fundamentals
* Working with Docker-based OpenLANE setup
* Preparing a design for implementation
* Running synthesis on a RISC-V processor
* Reading synthesis reports and statistics
* Understanding the overall RTL-to-GDSII flow

### Day 1 Status

| Activity            | Status |
| ------------------- | ------ |
| OpenLANE Setup      | ✅      |
| Docker Environment  | ✅      |
| Design Preparation  | ✅      |
| Synthesis Execution | ✅      |
| Report Analysis     | ✅      |

### Looking Ahead

The next stage focuses on **Floorplanning**, where the chip area, macro placement, power planning, and utilization parameters are explored before placement and routing.








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

> Insert the Power Planning Grid image here

```markdown
![Power Planning](images/power_planning.png)
```

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

> Insert the Noise Margin image here

```markdown
![Noise Margin](images/noise_margin.png)
```

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

> Insert the Standard Cell Design Flow image here

```markdown
![Cell Design Flow](images/cell_design_flow.png)
```

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

### Standard Cell Layout

> Insert the Standard Cell Layout image here

```markdown
![Standard Cell Layout](images/standard_cell_layout.png)
```

### Layout Components

* Diffusion Layers
* Polysilicon Gates
* Contacts
* Vias
* Metal Layers
* Power Rails

### Pin Locations

> Insert the Pin Location image here

```markdown
![Pin Locations](images/pin_locations.png)
```

Pin accessibility is an important consideration because it directly affects placement flexibility, routing quality, and timing closure.

---

## Standard Cell Characterization

Once the layout has successfully passed DRC and LVS verification, the standard cell undergoes characterization.

Characterization is the process of extracting electrical information that describes how the cell behaves under different operating conditions.

### Characterization Flow

> Insert the Characterization Flow image here

```markdown
![Characterization Flow](images/characterization_flow.png)
```

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

> Insert the Timing Threshold image here

```markdown
![Timing Thresholds](images/timing_thresholds.png)
```

Timing measurements are typically calculated using predefined voltage thresholds such as 20%, 50%, and 80% of the supply voltage.

### Propagation Delay Measurement

> Insert the Propagation Delay image here

```markdown
![Propagation Delay](images/propagation_delay.png)
```

Propagation delay is calculated as the difference between the output threshold crossing time and the corresponding input threshold crossing time.

Accurate delay measurement is critical because it directly impacts timing closure and overall circuit performance.

---

## Conclusion

Day 2 provided a comprehensive understanding of floorplanning fundamentals and their impact on ASIC implementation. Concepts such as utilization factor, aspect ratio, macro placement, power planning, decoupling capacitors, noise margin, voltage droop, and ground bounce highlighted the importance of proper physical design planning.

The session also introduced standard library cells, their design methodology, layout implementation, and characterization process. Understanding these concepts is essential because standard cells form the foundation of modern digital integrated circuits and directly influence the timing, power, and area characteristics of the final design.

Overall, this session established the groundwork for understanding placement, routing, timing optimization, and advanced physical design concepts in subsequent stages of the ASIC design flow.
