Introduction

On the second day of the workshop, the focus shifted from the basic ASIC design flow to understanding the physical implementation aspects of chip design. The session introduced the concept of floorplanning, which acts as the foundation for all subsequent physical design stages. A well-planned floorplan plays a crucial role in achieving better timing performance, reduced routing congestion, efficient power distribution, and optimal silicon area utilization.

The session also introduced standard library cells, which form the building blocks of digital integrated circuits. Along with floorplanning concepts, discussions included utilization factor, aspect ratio, power planning, noise margin, voltage droop, ground bounce, and the process of standard cell characterization.

Understanding the Netlist

Before entering the floorplanning stage, the synthesized design exists as a gate-level netlist. A netlist is a structural representation of the circuit containing all logic gates, flip-flops, buffers, input ports, output ports, and their interconnections.

Unlike RTL code, which describes the behavior of a circuit, a netlist explicitly specifies how the circuit is physically constructed using standard cells from a library.

Screenshot: Design Connectivity Diagram

(Insert Complete Design Connectivity Image)

From the connectivity diagram, it can be observed that multiple sequential and combinational elements are interconnected to perform the intended functionality. The netlist generated after synthesis serves as the primary input for physical design tools.

Key Learning
RTL is converted into a gate-level netlist after synthesis.
Netlists contain structural information rather than behavioral descriptions.
Physical design tools use netlists for placement and routing.
Introduction to Floorplanning

Floorplanning is the first stage of physical design where the overall structure of the chip is determined. During this stage, the designer defines the die area, core area, placement boundaries, routing resources, and power distribution strategy.

A good floorplan creates a strong foundation for placement and routing, while a poor floorplan can lead to congestion, timing violations, and power delivery issues.

Screenshot: Floorplan Structure

(Insert Floorplan Image)

Importance of Floorplanning
Defines chip dimensions.
Allocates space for standard cells.
Determines routing resources.
Facilitates power distribution planning.
Reduces congestion and timing violations.
Core Area and Die Area

The physical chip is known as the die. Inside the die lies the core area, where standard cells are placed.

Die Area > Core Area

The remaining space around the core is generally used for:

Input/Output pads
Power rings
Routing channels

The relationship between die area and core area directly affects placement efficiency and chip cost.

Utilization Factor

Utilization factor indicates how much of the core area is occupied by standard cells.

Utilization Factor = Area Occupied by Cells / Total Core Area

For example:

If the core area is 100 square units and cells occupy 60 square units,

Utilization Factor = 60/100 = 60%

Screenshot: Utilization Factor Example

(Insert Utilization Factor Image)

Good Utilization

A moderate utilization factor provides sufficient routing resources while maintaining area efficiency.

Poor Utilization

Very high utilization may create routing congestion, while very low utilization wastes silicon area.

Key Learning

The utilization factor is one of the most important metrics during floorplanning because it directly affects routability and timing closure.

Aspect Ratio

Aspect ratio defines the relationship between the height and width of the core.

Aspect Ratio = Height / Width

A square floorplan has an aspect ratio of 1, while rectangular floorplans have values greater or less than 1.

Screenshot: Aspect Ratio Illustration

(Insert Aspect Ratio Image)

The aspect ratio influences routing complexity, wire length, and congestion distribution across the design.

Good Floorplan vs Bad Floorplan

The quality of the floorplan directly affects the success of all subsequent physical design stages.

Characteristics of a Good Floorplan
Balanced utilization factor.
Uniform standard cell distribution.
Adequate routing resources.
Proper macro placement.
Efficient power distribution.
Reduced congestion.
Screenshot: Good Floorplan

(Insert Good Floorplan Image)

Characteristics of a Bad Floorplan
Uneven cell distribution.
Excessive routing congestion.
Long interconnect lengths.
Poor macro placement.
Power delivery challenges.
Screenshot: Bad Floorplan

(Insert Bad Floorplan Image)

Key Learning

A good floorplan significantly improves timing closure, routing quality, and overall chip performance.

Pre-Placed Cells

Certain large blocks cannot be freely moved during placement and are therefore pre-positioned during floorplanning.

Examples include:

Memory macros
PLL blocks
Analog IPs
Clock generation circuits
Screenshot: Pre-Placed Cell Example

(Insert Macro Placement Image)

These blocks create placement constraints that must be considered during floorplanning.

Decoupling Capacitors

Modern integrated circuits experience rapid switching activity, causing sudden current demands from the power supply.

To stabilize the power network, decoupling capacitors are placed near critical circuit blocks.

Functions of Decoupling Capacitors
Reduce voltage fluctuations.
Supply transient current.
Minimize power supply noise.
Improve reliability.
Screenshot: Decoupling Capacitor Concept

(Insert Decap Image)

Power Planning

After floorplanning, the Power Distribution Network (PDN) is constructed.

The PDN distributes:

VDD (Power Supply)
VSS (Ground)

to every cell in the design.

Screenshot: Power Planning Grid

(Insert PDN Image)

The power grid consists of horizontal and vertical power straps connected through vias and contacts.

Benefits
Reduces IR drop.
Improves reliability.
Ensures uniform current distribution.
Noise Margin

Noise margin indicates the amount of unwanted noise a digital signal can tolerate without causing incorrect logic interpretation.

The important voltage parameters are:

VOH
VOL
VIH
VIL
Screenshot: Noise Margin Diagram

(Insert Noise Margin Image)

A larger noise margin results in greater circuit reliability.

Ground Bounce

Ground bounce occurs when multiple transistors switch simultaneously from logic high to logic low.

The sudden discharge current flowing through the ground network causes temporary fluctuations in the ground voltage.

Effects
False switching
Logic errors
Reduced reliability
Screenshot: Ground Bounce Illustration

(Insert Ground Bounce Image)

Voltage Droop

Voltage droop occurs when many gates switch simultaneously from logic low to logic high.

The large current demand causes a temporary reduction in the supply voltage.

Effects
Increased delay
Timing violations
Reduced performance
Screenshot: Voltage Droop Illustration

(Insert Voltage Droop Image)

Introduction to Library Cells

Library cells are the fundamental building blocks used to construct digital circuits.

A standard cell library typically contains:

Inverters
Buffers
NAND Gates
NOR Gates
XOR Gates
Flip-Flops
Multiplexers

Each cell is carefully designed, verified, and characterized before being added to the library.

Standard Cell Design Flow

The creation of a standard cell involves several stages.

Screenshot: Standard Cell Design Flow

(Insert Cell Design Flow Image)

Inputs
Process Design Kit (PDK)
Design Rules
SPICE Models
User Specifications
Design Flow
Circuit Design
Layout Design
DRC Verification
LVS Verification
Characterization
Outputs
GDSII Layout
LEF File
CDL Netlist
Liberty File
Standard Cell Layout

The layout is the physical representation of a standard cell.

Screenshot: Standard Cell Layout

(Insert Metal Layer Layout Image)

The layout contains:

Diffusion Regions
Poly Gates
Contacts
Vias
Metal Layers
Power Rails
Screenshot: Pin Locations

(Insert Pin Location Image)

Pin accessibility is an important consideration because it affects routing efficiency.

Standard Cell Characterization

After layout verification, characterization is performed to determine the electrical properties of the cell.

Screenshot: Characterization Flow

(Insert GUNA Characterization Flow Image)

Characterization generates:

Timing Models
Power Models
Noise Models
Liberty Files

These files are later used by synthesis and timing analysis tools.

Timing Characterization

Timing characterization determines parameters such as:

Propagation Delay
Rise Time
Fall Time
Input Slew
Output Slew
Screenshot: Timing Threshold Definitions

(Insert Timing Threshold Image)

Screenshot: Propagation Delay Measurement

(Insert Delay Graph Image)

Propagation delay is measured as the difference between the output threshold crossing time and the input threshold crossing time.

These values are stored inside Liberty files and are used during static timing analysis.

Conclusion

Day 2 provided a comprehensive understanding of floorplanning and its impact on ASIC implementation. The session highlighted the characteristics of good and bad floorplans, utilization factor, aspect ratio, power planning, and signal integrity considerations such as voltage droop and ground bounce. Additionally, the introduction to library cells, standard cell layout, and characterization offered valuable insight into how digital building blocks are created and prepared for use in modern ASIC design flows. This knowledge forms the foundation for understanding placement, routing, timing optimization, and physical verification in subsequent stages of chip design.
