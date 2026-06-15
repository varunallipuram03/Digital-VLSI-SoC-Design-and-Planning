Overview

The focus of Day 2 was understanding the importance of floorplanning in the ASIC physical design flow and studying the characteristics of standard library cells. Floorplanning serves as the foundation for successful placement, routing, timing closure, and power distribution. A well-designed floorplan improves chip performance and reduces congestion, while a poor floorplan can introduce routing challenges, timing violations, and power integrity issues.

This session also introduced standard library cells, their design methodology, characterization process, and their role in modern digital integrated circuit design.

Understanding the Netlist

After synthesis, the RTL description is converted into a gate-level netlist. A netlist represents the complete connectivity information of the design and describes how logic gates, flip-flops, buffers, inputs, and outputs are interconnected.

Unlike RTL code, which describes functionality, a netlist describes the actual implementation structure using standard cells available in the technology library.

Design Connectivity Representation

The generated netlist becomes the primary input for physical design tools and forms the basis for floorplanning and placement.

Key Observations
RTL is transformed into a gate-level netlist after synthesis.
Netlists contain structural connectivity information.
Physical design tools use the netlist for placement and routing.
Introduction to Floorplanning

Floorplanning is the first stage of physical design where the physical dimensions and organization of the chip are defined. During this stage, the die area, core area, macro placement, routing resources, and power planning structures are established.

A good floorplan ensures efficient utilization of silicon area while providing sufficient routing resources and power distribution paths.

Floorplan Structure

Importance of Floorplanning
Defines chip dimensions and layout boundaries.
Determines available placement area.
Establishes routing resources.
Enables efficient power distribution.
Reduces congestion and timing violations.
Core Area and Die Area

The complete silicon chip is referred to as the Die, while the region allocated for standard cell placement is known as the Core Area.

The remaining area is generally used for:

Input and Output pads
Power rings
Routing channels
Clock distribution resources

A proper balance between die area and core area is necessary to achieve optimal utilization and routing efficiency.

Utilization Factor

Utilization factor indicates how much of the available core area is occupied by standard cells.

Utilization Factor = Area Occupied by Standard Cells / Total Core Area

Example
Core Area = 100 units²
Standard Cell Area = 60 units²

Utilization Factor = 60 / 100 = 60%

Utilization Concept

Learning Points
Low utilization wastes silicon area.
High utilization causes routing congestion.
Balanced utilization improves routability and timing closure.
Utilization factor directly impacts placement quality.
Aspect Ratio

Aspect ratio defines the relationship between the height and width of the core area.

Aspect Ratio = Height / Width

Example
Height = 100 μm
Width = 100 μm

Aspect Ratio = 1

Aspect Ratio Illustration

The aspect ratio influences routing complexity, wire length, congestion distribution, and placement quality.

Learning Points
Square floorplans generally have an aspect ratio of 1.
Aspect ratio affects routing resources.
Improper aspect ratio can create congestion hotspots.
It influences timing and routing efficiency.
Good Floorplan vs Bad Floorplan

The quality of a floorplan directly impacts the success of subsequent physical design stages.

Characteristics of a Good Floorplan
Balanced utilization factor.
Uniform cell distribution.
Sufficient routing resources.
Proper macro placement.
Efficient power distribution network.
Reduced congestion hotspots.
Characteristics of a Bad Floorplan
Uneven cell placement.
Excessive routing congestion.
Long interconnect lengths.
Poor macro positioning.
Increased timing and power challenges.
Floorplan Comparison

Learning Outcome

A properly planned floorplan significantly improves timing closure, routing efficiency, and overall chip performance.

Pre-Placed Cells

Certain large blocks cannot be freely moved during placement and must be positioned during floorplanning.

Examples include:

SRAM Macros
PLL Blocks
Analog IPs
Clock Generation Circuits
Macro Placement

The placement of these macros affects routing resources and timing performance across the entire chip.

Decoupling Capacitors (Decap Cells)

Rapid switching activity inside digital circuits causes sudden current demands from the power supply network.

To stabilize the supply voltage, decoupling capacitors are inserted near critical circuit blocks.

Benefits of Decoupling Capacitors
Reduce voltage fluctuations.
Provide transient current support.
Improve power integrity.
Reduce supply noise.
Improve reliability of operation.
Decap Placement Concept

Power Planning

Power planning ensures reliable distribution of power throughout the chip.

The Power Distribution Network (PDN) consists of:

VDD Rails
VSS Rails
Power Rings
Vertical Power Straps
Horizontal Power Straps
Vias and Contacts
Power Distribution Network

Benefits
Improved voltage stability.
Reduced IR drop.
Better reliability.
Uniform current distribution.
Enhanced overall chip performance.
Noise Margin

Noise margin represents the maximum amount of noise that a digital signal can tolerate without causing incorrect logic interpretation.

Important voltage parameters include:

VOH (Output High Voltage)
VOL (Output Low Voltage)
VIH (Input High Voltage)
VIL (Input Low Voltage)
Noise Margin Representation

Importance

A larger noise margin improves circuit robustness and ensures reliable operation in noisy environments.

Ground Bounce

Ground bounce occurs when multiple transistors switch simultaneously from HIGH to LOW, causing temporary fluctuations in ground potential.

Effects of Ground Bounce
False switching events.
Logic errors.
Increased noise.
Reduced reliability.

Understanding ground bounce is important when designing power delivery networks and high-speed digital circuits.

Voltage Droop

Voltage droop occurs when a large number of gates switch simultaneously from LOW to HIGH, creating a sudden demand for current.

Effects of Voltage Droop
Increased propagation delay.
Timing violations.
Reduced circuit performance.
Potential functional failures.

Proper power planning and decoupling capacitor insertion help reduce voltage droop.

Introduction to Library Cells

Library cells are the fundamental building blocks used to implement digital circuits.

Typical library cells include:

Inverters
Buffers
NAND Gates
NOR Gates
XOR Gates
Multiplexers
Flip-Flops
Latches

Each cell is carefully designed, verified, and characterized before being included in the standard cell library.

Standard Cell Design Flow

The development of a standard cell follows a structured design methodology.

Standard Cell Design Flow

Inputs
Process Design Kit (PDK)
Design Rules
SPICE Models
User Specifications
Design Stages
Circuit Design
Layout Design
DRC Verification
LVS Verification
Characterization
Outputs
GDSII Layout
CDL Netlist
LEF File
Liberty (.lib) File
Standard Cell Layout

The physical implementation of a standard cell is represented through its layout.

Standard Cell Layout

The layout contains:

Diffusion Layers
Poly Gates
Contacts
Vias
Metal Layers
Power Rails
Pin Locations

Pin accessibility is an important factor because it directly affects routing quality and placement efficiency.

Standard Cell Characterization

After layout verification, characterization is performed to extract timing, power, and noise information.

Characterization Flow

Characterization generates:

Timing Models
Power Models
Noise Models
Liberty Files

These models are later used during synthesis, placement, routing, and static timing analysis.

Timing Characterization

Timing characterization determines important timing parameters used by EDA tools.

Timing Parameters
Propagation Delay
Rise Time
Fall Time
Input Slew
Output Slew
Timing Threshold Definitions

Propagation Delay Measurement

Propagation delay is calculated as the difference between the output threshold crossing time and the corresponding input threshold crossing time.

The extracted timing information is stored in Liberty files and is later used for static timing analysis and timing closure.

Conclusion

Day 2 provided a detailed understanding of floorplanning fundamentals and the role of standard library cells in ASIC implementation. Concepts such as utilization factor, aspect ratio, macro placement, power planning, decoupling capacitors, noise margin, voltage droop, and ground bounce highlighted the importance of proper physical design planning. The session also introduced the standard cell design flow, layout methodology, and characterization process, providing insight into how library cells are developed and prepared for use in modern ASIC design flows.
