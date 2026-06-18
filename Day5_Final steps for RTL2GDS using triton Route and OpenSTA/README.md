Day 5 - Routing, Design Rule Checking and Parasitic Extraction
Overview

After placement and clock tree synthesis, the design enters one of the most critical stages of the physical design flow: Routing. The purpose of routing is to establish reliable electrical connections between all the placed standard cells, macros, input/output pins, and clock network components while obeying manufacturing constraints.

Routing directly impacts signal integrity, timing closure, power consumption, and overall chip manufacturability. Even a perfectly placed design can fail if routing introduces excessive delay, congestion, or design rule violations.

During this session, I explored the complete routing flow, including maze routing algorithms, detailed routing using TritonRoute, design rule verification, and parasitic extraction techniques used in modern VLSI design.

Understanding Routing in Physical Design

Routing is the process of creating physical metal interconnections between circuit elements.

The router must determine:

Which metal layers to use
The path connecting source and destination
Where vias should be inserted
How to avoid congestion
How to satisfy all design rules

The objective is not simply to connect two points, but to find an optimized path that satisfies electrical and manufacturing requirements.

Maze Routing using Lee's Algorithm

One of the earliest and most influential routing techniques is Lee's Maze Routing Algorithm.

The algorithm treats the routing region as a grid and systematically searches for a path between the source and target nodes.

Routing Procedure
Step 1: Generate Routing Grid

The routing area is divided into discrete grid locations.

Step 2: Define Source and Target

The source and destination points are identified within the routing space.

Step 3: Wave Propagation

The algorithm expands outward from the source node and labels neighboring grid cells with increasing cost values.

Step 4: Backtracking

Once the target is reached, the shortest path is reconstructed by tracing backward through decreasing labels.

Step 5: Path Optimization

Among multiple valid routes, paths with fewer bends are generally preferred because they reduce resistance and routing complexity.

Maze Routing Illustration

The routing wave expands until the destination is reached. The shortest valid route is then selected while avoiding obstacles present in the layout.

Routing Stages

Modern physical design tools divide routing into two major phases.

Global Routing

Global routing determines an approximate path for each net.

Its responsibilities include:

Congestion estimation
Resource allocation
Layer assignment
Generation of routing guides

The output of global routing is not the final wire geometry but a set of routing guides that detailed routing must follow.

Detailed Routing

Detailed routing converts routing guides into exact wire shapes and vias.

Responsibilities include:

Exact track assignment
Via insertion
Pin accessibility verification
Design rule compliance
Connectivity completion

This stage directly generates the final manufacturable interconnect structure.

Detailed Routing using TritonRoute

TritonRoute is the detailed routing engine used in the OpenROAD flow.

Unlike traditional routers that only focus on connectivity, TritonRoute simultaneously considers:

Routing correctness
Design rule compliance
Pin accessibility
Manufacturability
TritonRoute Workflow
Initial Detailed Routing

The router first generates an initial routing solution based on global routing information.

Route Guide Utilization

TritonRoute follows the routing guides generated during global routing and attempts to remain within these guides whenever possible.

Connectivity Verification

Each net must maintain complete connectivity between all associated pins and routing segments.

Search and Repair

Any violations discovered during routing are corrected iteratively through a search-and-repair mechanism.

TritonRoute Architecture

Key observations:

Performs initial detailed routing.
Uses preprocessed routing guides.
Ensures inter-guide connectivity.
Supports parallel and sequential routing strategies across metal layers.
Preprocessed Route Guides

Routing guides generated during global routing undergo preprocessing before being used by TritonRoute.

The preprocessing operations include:

Splitting
Merging
Bridging

The purpose is to create routing regions that are easier for the detailed router to handle.

Requirements of Preprocessed Guides

For efficient routing, guides should:

Maintain unit width
Follow preferred routing directions
Preserve connectivity between routing regions
Minimize routing ambiguity
Preprocessed Route Guide Example

The preprocessing stage converts raw routing information into structured routing regions that improve detailed routing quality.

Inter-Guide Connectivity

A routing guide alone is not sufficient.

Adjacent routing guides belonging to the same net must remain electrically connected.

Inter-guide connectivity ensures that:

Neighboring guides communicate properly
Nets remain continuous across layers
Routing integrity is preserved throughout the layout

Without proper connectivity, the router may generate disconnected wire segments.

Access Points and Connectivity Handling

TritonRoute introduces the concept of Access Points (APs).

Access Point (AP)

An Access Point is an on-grid location used to connect:

Pins
Lower metal layers
Upper metal layers
I/O ports

It acts as an entry or exit location during routing.

Access Point Cluster (APC)

An Access Point Cluster is a collection of access points that originate from the same physical object.

Examples include:

A pin shape
An upper-layer guide
A lower-layer segment

Grouping access points improves routing flexibility and connectivity optimization.

Connectivity Handling

The router selects appropriate access points to establish reliable electrical connections while minimizing routing complexity.

Routing Topology Optimization

After connectivity has been established, the routing topology must be optimized.

The objective is to minimize:

Total wirelength
Via count
Routing congestion
Signal delay

Many routing engines employ graph-based optimization techniques such as Minimum Spanning Trees (MSTs) to determine efficient routing structures.

Routing Topology Algorithm

The optimization stage determines how routing resources are shared among different terminals belonging to the same net.

Design Rule Checking (DRC)

Once routing is completed, the design must be verified against fabrication constraints.

A DRC-clean design is mandatory before tapeout.

Important Wire Rules
Wire Width

Defines the minimum allowable width of a metal segment.

Insufficient width can increase resistance and manufacturing defects.

Wire Spacing

Defines the minimum separation between neighboring wires.

Violations may cause shorts or signal coupling.

Wire Pitch

Represents the center-to-center distance between adjacent routing tracks.

Pitch directly affects routing density.

Additional Via Rules

Routing introduces vertical interconnects through vias.

Therefore two additional rules become important:

Via Width

Controls the physical dimensions of a via.

Via Spacing

Defines the minimum distance between neighboring vias.

Common DRC Violations
Signal Short

Occurs when two unrelated nets become electrically connected.

Spacing Violation

Occurs when wires are placed too close together.

Via Violation

Occurs when via dimensions or spacing fail design rule requirements.

DRC Clean Layout

The final routed design must satisfy all spacing, width, pitch, and via constraints before fabrication.

Parasitic Extraction

Even after routing is completed, the wires themselves introduce unwanted electrical effects.

These effects are known as parasitics.

Sources of Parasitics
Parasitic Resistance

Created by the finite conductivity of metal interconnects.

Parasitic Capacitance

Generated between neighboring wires and between wires and substrate.

Coupling Capacitance

Occurs when adjacent nets influence each other.

Importance of Extraction

Parasitic extraction is performed to:

Estimate realistic delays
Evaluate signal integrity
Analyze power consumption
Improve timing accuracy

Without extraction, timing analysis would be overly optimistic and inaccurate.

Extracted Parasitic Network

The extracted RC network provides a more realistic representation of the routed design and is later used during signoff timing analysis.

Key Takeaways
Routing converts placement information into manufacturable metal interconnections.
Lee's Algorithm provides the foundation for maze-based path finding.
Global routing creates routing guides while detailed routing generates exact wire geometries.
TritonRoute performs detailed routing while ensuring connectivity and design rule compliance.
Access Points and Access Point Clusters improve routing flexibility.
DRC verification ensures manufacturability.
Parasitic extraction models real interconnect behavior for accurate timing analysis.
