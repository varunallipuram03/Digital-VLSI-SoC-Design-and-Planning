# Day 1 Documentation
# OpenLane SKY130 Workshop – Day 1

## Inception of Open-Source EDA, OpenLANE and SKY130 PDK

### Author
**Varun Venkata**

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
1. How Computers Work — The Abstraction Chain
The Core Problem
Humans think in natural language, diagrams, and high-level logic. Processors understand only two voltage levels — 0 and 1. Bridging this gap requires a layered translation system, where each layer speaks to the one below it.

The Abstraction Layers
High-Level Language (C, Python, Java)
            ↓  Compiler
Assembly Language (human-readable mnemonics)
            ↓  Assembler
Machine Code (binary instructions)
            ↓  Processor hardware
Silicon switching (transistors on/off)
Why each layer exists:

High-Level Language lets humans express logic without knowing hardware details. A statement like a = b + c hides dozens of hardware operations.
Compiler translates that statement into architecture-specific instructions, applying optimizations like register allocation and instruction reordering.
Assembly is a 1-to-1 textual representation of machine instructions, useful for debugging and low-level control.
Machine Code is the actual binary that the CPU fetches, decodes, and executes.
ISA (Instruction Set Architecture) is the contract between software and hardware — it defines what instructions exist, what registers are available, and how memory is addressed.
Why This Matters for ASIC Design
When designing a chip, you are building the bottom layer of this stack. The RTL (Register-Transfer Level) code you write must correctly implement the ISA so that all software above it continues to work. A mistake at the silicon level can break everything built on top of it.

Software to Hardware Flow
2. Chip Anatomy — Package, Die, Core, Pads, and IPs
What Is a Chip?
A chip is not just the silicon die. The complete unit includes both the die (the actual silicon) and the package (the protective housing that makes the die usable in the real world).

Chip = Package + Die
The Package
The package serves three critical functions:

Physical Protection — Silicon dies are fragile. The package shields them from mechanical stress, moisture, and contamination.
Electrical Connectivity — Tiny bond wires or solder bumps connect the microscopic die pads to the external pins that can be soldered onto a PCB.
Heat Dissipation — Modern chips generate significant heat. The package provides a thermal path to the environment.
QFN-48 (Quad Flat No-Leads, 48 pins) is a common compact package format where the leads are on the bottom perimeter rather than protruding as legs. This reduces size and improves thermal/electrical performance.

The Die
The die is the actual piece of silicon where all transistors and circuits live. It is fabricated on a silicon wafer and then cut (diced) out. The die contains:

Core — the region where all digital logic (standard cells, macros) is placed
I/O Ring — the ring of pad cells around the core that interface with the outside world
Pads
Pads are the interface between the internal chip signals and the external package pins. Different types of pads serve different roles:

Power Pads (VDD, GND) — bring supply voltage into the chip
Signal Pads (GPIO, clock, data) — carry data in and out
Analog Pads — handle analog signals with special ESD protection
Core
The core is where computation happens. It contains:

Standard Cells — the fundamental logic building blocks (NAND, NOR, DFF, MUX, etc.)
Memories (SRAM) — on-chip storage
Macros — large pre-designed blocks like processor cores, PLLs, or ADCs
Intellectual Property (IP) Blocks
IPs are reusable circuit blocks designed once and instantiated many times.

Type	Description	Examples
Soft IP	Delivered as synthesizable RTL, can be re-synthesized for any process	RISC-V core, UART, SPI
Hard IP	Delivered as fixed layout tied to a specific process node	SRAM compiler, PLL, ADC
Firm IP	Partially fixed — netlist is given but placement is flexible	ARM Cortex-M
Why the distinction matters: Hard IPs cannot be ported to a different process without redesigning them. Soft IPs are portable but may not achieve optimal performance. Choosing the right IP type is a key architecture decision.

SoC Components

3. RISC-V and the ISA Bridge
What Is RISC-V?
RISC-V is an open-source Instruction Set Architecture developed at UC Berkeley. Unlike ARM or x86, RISC-V has no licensing fees and its specification is publicly available.

Why RISC-V for This Workshop
Open Source — the spec is free, no royalties, no NDAs
Modular — a base integer ISA (RV32I) plus optional extensions (M for multiply, A for atomics, F for floating point, C for compressed)
Industry Traction — used in embedded controllers, storage chips, AI accelerators, and research processors
Excellent Reference Designs — PicoRV32A is a compact, well-tested RISC-V implementation ideal for learning
Register Convention
RISC-V has 32 general-purpose registers (x0–x31). x0 is hardwired to zero. A typical instruction:

add x5, x6, x7   ; x5 = x6 + x7
In binary: 00000000011100110000001010110011

This 32-bit number encodes the opcode, source registers, and destination register. The processor hardware decodes this and routes the right signals to the ALU.

The ISA as a Contract
The ISA defines the boundary between hardware and software. Any correct hardware implementation of RISC-V will run any software written for RISC-V. This separation of concerns is what makes the ecosystem possible — compiler writers, OS developers, and chip designers can all work in parallel.

4. From Software to Silicon — The Full Transformation Path
The Complete Journey
Application (e.g., stopwatch app in C)
            ↓
Operating System (handles I/O, memory allocation, system calls)
            ↓
Compiler (converts C → assembly instructions for the target ISA)
            ↓
Assembler (converts assembly → binary machine code → .exe or ELF)
            ↓
Processor (executes binary — fetches, decodes, executes instructions)
            ↓
RTL Implementation (describes the processor in Verilog/VHDL)
            ↓
Synthesis (converts RTL → gate-level netlist using standard cells)
            ↓
Physical Design (places and routes gates on silicon)
            ↓
Fabrication (photolithography creates actual transistors)
            ↓
Silicon chip running your application
Why Every Layer Matters
A bug anywhere in this chain causes failures that can be very hard to trace. A synthesis bug might produce a chip that passes simulation but fails in silicon. A compiler bug might generate wrong machine code for a specific instruction sequence. Understanding the full chain helps engineers reason about where failures originate.

5. Open-Source ASIC Design — The Three Pillars
Historically, ASIC design required expensive proprietary EDA tools (Synopsys, Cadence, Mentor) and costly PDK licenses. The open-source ASIC movement changes this by providing all three pillars freely.

Pillar 1 — RTL Design Sources
Free RTL can be obtained from:

librecores.org — curated library of open hardware cores
opencores.org — community-contributed IP cores
GitHub — PicoRV32, CVA6, ibex, and hundreds of other open processors
Pillar 2 — Open-Source EDA Tools
Tool	Role in Flow	Why It Exists
Yosys	RTL Synthesis	First open-source synthesis tool capable of handling real designs
ABC	Technology Mapping	Logic optimization and mapping to standard cells
OpenROAD	Place and Route (PnR)	Full automated PnR including floorplan, placement, CTS, routing
OpenSTA	Static Timing Analysis	Analyzes setup/hold timing, critical paths
TritonRoute	Detailed Routing	DRC-aware detailed router
Magic	Layout Viewer, DRC, SPICE extraction	Long-standing open-source VLSI tool
Netgen	LVS (Layout vs Schematic)	Compares extracted netlist to Verilog netlist
Fault	DFT (Design for Test)	Scan insertion, ATPG, fault simulation
KLayout	Layout Visualization	GDS viewer with scripting support
Pillar 3 — Open PDK (SKY130)
Before SKY130, all process design kits were locked behind NDAs. The Google+SkyWater collaboration in 2020 released the first production-grade open PDK. This completes the open-source ASIC trifecta.

6. RTL-to-GDSII Flow — Every Stage Explained
The RTL-to-GDSII flow transforms a hardware description into a physical layout ready for fabrication. Each stage has a specific purpose and produces specific outputs consumed by the next stage.

RTL to GDSII Flow

Stage 1 — Synthesis
What: Converts RTL (Verilog/SystemVerilog) into a gate-level netlist using cells from the Standard Cell Library (SCL).

How it works:

Yosys parses the RTL and builds an internal representation
Logical optimization simplifies the Boolean logic
ABC maps the optimized logic to actual cells from the target library (e.g., sky130_fd_sc_hd__nand2_1)
Why it matters: After synthesis, you have a list of gates and their connections. This is the last point where you can catch logical errors before physical design. All subsequent steps are purely physical.

Output: Gate-level netlist (.v file)

Stage 2 — Floorplanning
What: Defines the die area, places I/O pads around the perimeter, and positions large blocks (macros, SRAMs) before automatic placement.

Two aspects:

Chip Floorplanning — partitions the die between different system building blocks, places I/O pads, and defines power domains
Macro Floorplanning — for individual blocks, defines dimensions, pin locations, and row definitions for standard cell placement
Why it matters: Floorplanning decisions directly affect routability, timing, and power. Poorly placed macros create routing congestion. I/O placement affects PCB layout. Getting floorplanning right is an art requiring experience with the specific design.

Stage 3 — Power Planning
What: Creates the power distribution network (PDN) that delivers VDD and GND to every cell on the chip.

Structure:

Power Pads — connect to the package pins
Power Rings — surround the core, distribute supply around the perimeter
Power Straps — run across the core in both directions (horizontal and vertical), forming a mesh
Standard Cell Rails — the metal1 VDD and GND rails that every standard cell connects to
Why it matters: Insufficient power routing causes IR drop (voltage drop due to resistance), leading to timing failures or functional errors. The PDN must be designed to handle peak current demand with acceptable voltage droop.

Stage 4 — Placement
What: Places all standard cells from the netlist onto the floorplan rows (called "sites") in legal positions.

Two phases:

Global Placement — finds approximate positions for all cells, optimizing for wirelength. Cells may overlap at this stage.
Detailed Placement — legalizes positions so cells don't overlap, align to rows, and respect design rules. Also does local optimizations.
Why it matters: Placement quality directly determines routing success and timing closure. Dense, well-organized placement leads to shorter wires, better timing, and easier routing. Poor placement causes DRC violations during routing.

Stage 5 — Clock Tree Synthesis (CTS)
What: Builds the clock distribution network — a tree of buffers/inverters that delivers the clock signal to all flip-flops with minimal skew.

Why it's hard: In a large design, there may be tens of thousands of flip-flops. A single clock driver cannot drive all of them simultaneously with equal delay. CTS inserts a balanced tree of clock buffers to ensure all flip-flops see the clock at approximately the same time.

Key metric: Clock skew — the difference in arrival time of the clock between the earliest and latest flip-flop. Large skew consumes timing margin that could otherwise be used for logic.

Why it matters: CTS modifies the netlist (inserts cells), so a Logic Equivalence Check (LEC) must be run afterward to verify the design's functionality was not altered.

Stage 6 — Routing
What: Connects all the nets (wires) according to the netlist, using the available metal layers.

Two phases:

Global Routing — divides the routing area into a grid (called GCells), assigns nets to grid tracks, creating routing guides. Fast but approximate.
Detailed Routing — uses the guides from global routing to implement actual metal wires, vias, and contacts. Must obey all DRC rules.
Why it's hard: The routing grid is enormous (millions of tracks). The problem is NP-hard in general. Modern routers use heuristics and machine learning to find DRC-clean solutions efficiently.

Key constraint: Metal layers have preferred directions (horizontal vs. vertical) to avoid coupling and simplify DRC. Routing between layers requires vias, which consume area and add resistance.

Stage 7 — Sign-Off
What: Final verification that the design is correct and ready for tape-out (sending to fabrication).

Three checks:

DRC (Design Rule Check) — verifies that all layout features (wire widths, spacings, via sizes) obey the foundry's manufacturing rules. Magic performs DRC for SKY130.

LVS (Layout vs Schematic) — extracts a circuit netlist from the physical layout and compares it to the original gate-level netlist. Catches missing connections, short circuits, or extra devices. Netgen performs LVS for SKY130.

STA (Static Timing Analysis) — analyzes every path in the design to verify setup time (data arrives before clock edge) and hold time (data is stable after clock edge) are met across all process/voltage/temperature corners. OpenSTA performs this.

Why it matters: Tape-out is expensive. A chip that goes to fabrication with DRC, LVS, or timing violations will either not function or degrade over time. Sign-off is the final quality gate.

Output: GDSII file — the industry-standard format for physical layouts, containing all layer geometries used to create photomasks.

7. OpenLANE — Architecture, Goals, and Philosophy
What Is OpenLANE?
OpenLANE is an automated RTL-to-GDSII ASIC flow developed by eFabless. It is not a single tool — it is an orchestration framework that integrates 16+ open-source EDA tools into a cohesive, reproducible pipeline.

OpenLANE ASIC Flow

The Core Goal
Produce a clean GDSII with no human intervention (no-human-in-the-loop)

"Clean" specifically means:

No DRC violations — the layout obeys all foundry manufacturing rules
No LVS violations — the layout matches the intended circuit
Timing closure — all setup and hold constraints are met (targeted)
Why "No Human in the Loop" Is Hard
Traditional ASIC flows require experienced engineers to manually:

Adjust floorplan parameters when placement density is too high
Fix antenna violations after routing
Tune synthesis strategies when timing fails
Re-run routing with different configurations after DRC errors
OpenLANE automates these iterations using design exploration and scripted flows, making silicon accessible to engineers who don't have years of physical design experience.

OpenLANE Detailed Flow
Design RTL (Verilog files)
        ↓
RTL Synthesis (Yosys + ABC)
        ↓
Static Timing Analysis — post-synthesis (OpenSTA)
        ↓
Design for Test — scan insertion (Fault)
        ↓
Floorplanning + Power Planning (OpenROAD)
        ↓
Placement — Global + Detailed (OpenROAD)
        ↓
Fake Antenna Diode Insertion (Custom Script)
        ↓
Clock Tree Synthesis (OpenROAD)
        ↓
Optimization (OpenROAD)
        ↓
Global Routing (OpenROAD)
        ↓
Logic Equivalence Check (Yosys)
        ↓
Detailed Routing (TritonRoute)
        ↓
Fake→Real Antenna Diode Swapping (Custom Script)
        ↓
RC Extraction (DEF2SPEF)
        ↓
Static Timing Analysis — post-routing (OpenSTA / OpenROAD)
        ↓
Physical Verification — DRC + LVS (Magic + Netgen)
        ↓
GDS Streaming (Magic)
        ↓
GDSII Output
Synthesis Exploration
OpenLANE ships with multiple synthesis strategies (combinations of Yosys + ABC optimization sequences). The Synthesis Exploration utility runs all strategies on a design and reports metrics (area, delay, gate count) to help pick the best one for a given design's requirements.

Design Space Exploration (DSE)
OpenLANE has 16 design-specific and 40+ total configuration parameters. Not all combinations produce a clean layout. The DSE utility sweeps parameter combinations and collects ~35 metrics per run, enabling systematic optimization without manual trial-and-error.

Two Modes of Operation
Mode	Use Case
Autonomous	Set parameters, run ./flow.tcl -design <name>, get GDSII. Fully automated.
Interactive	Run each step manually with flow.tcl -interactive. Inspect intermediate results, debug issues, experiment with settings.
Handling Antenna Rule Violations
During fabrication, long metal wires act as antennas. Reactive ion etching during fabrication causes charge to accumulate on these wires, which can damage transistor gate oxides.

OpenLANE's preventive approach:

After placement, insert a Fake Antenna Diode next to every cell input pin
After detailed routing, run the Antenna Checker (Magic) on the routed layout
For each violated input pin, replace the Fake Antenna Diode with a real antenna diode cell from the SCL
This discharges accumulated charge safely during fabrication
The StriVe SoC Family
OpenLANE was originally developed to tape out the striVe family of SoCs on SKY130 — designs where every component (PDK, EDA tools, RTL) is open source. This validated that OpenLANE could produce real, working silicon.

Regression Testing
OpenLANE maintains a regression suite of ~70 designs. After any code change, the suite is re-run and results compared to known-good configurations, ensuring the flow remains reliable across updates.

8. SKY130 PDK — Why It Matters
What Is a PDK?
A Process Design Kit is the complete set of files provided by a semiconductor foundry that describes how to design for their specific manufacturing process. Without a PDK, you cannot design a chip for fabrication — you would not know the electrical characteristics, design rules, or available layers.

A PDK includes:

Component	Purpose
Design Rules (DRC)	Minimum feature sizes, spacings, enclosures required by lithography
SPICE Models	Transistor electrical models for simulation
Standard Cell Library	Pre-designed, pre-characterized logic cells
Technology Files	Layer stack, parasitic models, antenna rules
I/O Cells	Pad ring cells with ESD protection
SKY130 — The First Open PDK
SKY130 is a mature 130nm CMOS process jointly developed by SkyWater Technology and made open-source through a partnership with Google. It was released publicly in June 2020.

Why 130nm and not a cutting-edge node?

130nm is mature, well-characterized, and manufacturable without extreme UV lithography
It is sufficient for educational, research, IoT, mixed-signal, and custom digital designs
Making a bleeding-edge node open-source is not feasible due to cost and competitive concerns
The concepts learned at 130nm apply directly to advanced nodes
Standard Cell Libraries in SKY130
sky130_fd_sc_hd    — High Density (most commonly used in OpenLANE)
sky130_fd_sc_hdll  — High Density, Low Leakage
sky130_fd_sc_hs    — High Speed
sky130_fd_sc_ls    — Low Speed
sky130_fd_sc_ms    — Medium Speed
Cell Naming Convention
sky130_fd_sc_hd__nand2_1
  │      │  │    │      └── Drive Strength (1 = smallest/weakest)
  │      │  │    └───────── Cell Function (2-input NAND)
  │      │  └────────────── Library Variant (High Density)
  │      └───────────────── Foundry Device, Standard Cell
  └──────────────────────── SkyWater 130nm process
Drive strength affects the cell's ability to drive capacitive loads. Larger drive strengths use more power and area but can drive longer wires or more fanout.

9. Practical Lab — Setup, Design Prep, and Synthesis
Environment
The workshop runs OpenLANE v1.0.2 inside a Docker container on GitHub Codespaces. Docker ensures a fully reproducible environment — all tools, versions, and configurations are frozen, eliminating "works on my machine" issues.

Step 1 — Enter the Container
cd ~/Desktop/OpenLane
make mount
make mount launches a Docker container with all necessary tools, mounting the local OpenLane directory and PDK inside the container. The PDK is located at /home/vscode/.ciel and the standard cell library used is sky130_fd_sc_hd.

Step 2 — Launch OpenLANE Interactive Mode
./flow.tcl -interactive
This starts the OpenLANE Tcl shell. Interactive mode lets you control each step individually, inspect outputs, and debug.

Step 3 — Load the OpenLANE Package
package require openlane
Expected output: 1.0.2

Step 4 — Prepare the Design
prep -design picorv32a
What happens during prep:

Reads designs/picorv32a/config.tcl — the design-specific configuration file containing clock period, synthesis strategy, PDK settings, etc.
Loads the PDK and resolves library paths
Merges Technology LEF (layer definitions) + Cell LEF (cell abstracts) into a single merged LEF file
Creates the run directory: designs/picorv32a/runs/RUN_<timestamp>/
Sets up subdirectories for logs, reports, results, and temporary files
The run directory structure:

RUN_2026.06.05_01.59.53/
├── logs/          ← per-step tool logs
├── reports/       ← timing, DRC, area reports
├── results/       ← output files per stage (netlist, DEF, GDS)
├── tmp/           ← intermediate files
├── cmds.log       ← record of every command run
├── openlane.log   ← top-level flow log
└── runtime.yaml   ← timing data for each step
Why the run directory matters: Every run creates a timestamped directory. This means you can go back and compare results from different parameter settings without overwriting previous work — essential for design exploration.

Step 5 — Run Synthesis
run_synthesis
What happens internally:

Step 1 — Yosys RTL Synthesis:
Reads picorv32a.v, elaborates the design, performs logical optimization, and produces an initial netlist

Step 2 — ABC Technology Mapping:
Maps the optimized logic to actual SKY130 standard cells using the selected synthesis strategy. ABC optimizes for area, timing, or a balance of both depending on the synthesis strategy.

Step 3 — OpenSTA (Single-Corner STA):
Runs static timing analysis on the synthesized netlist to report setup slack, worst negative slack (WNS), and total negative slack (TNS)

Output artifacts:

results/synthesis/picorv32a.synthesis.v — gate-level netlist
reports/synthesis/ — timing reports, area report, synthesis statistics
Design Preparation and Synthesis

10. Synthesis Results and Analysis
Statistics from the Run
Metric	Value
Number of Wires	17,043
Wire Bits (total)	17,475
Public Wires	1,520
Public Wire Bits	1,908
Total Cells	17,323
D Flip-Flops (DFF)	1,614
DFF Ratio Calculation
DFF Ratio = (DFF Count / Total Cells) × 100
          = (1614 / 17323) × 100
          = 9.32%
Why this metric matters:

The DFF ratio gives a sense of how sequential (stateful) vs. combinational the design is. PicoRV32A at ~9.32% is typical for a processor — most of the logic is combinational (ALU, data path, decoder), with registers for pipeline state, program counter, and register file.

Designs with very high DFF ratios (>50%) tend to be state machines or shift-register-heavy designs. Very low ratios might indicate a purely combinational design (rare in practice).

Reading the Synthesis Reports
Key reports generated at reports/synthesis/:

1-synthesis.AREA_0.stat.rpt — lists every cell type used and its count, total area in µm²
2-sta.min.rpt / 2-sta.max.rpt — setup and hold timing paths, showing worst paths and slack values
yosys_4-opt.stat.rpt — optimization statistics
11. Issues Encountered and Resolutions
Issue 1 — Linux Commands Are Case-Sensitive
Problem: Running CD, LS, PWD produces "command not found" errors.
Root Cause: Linux/Unix systems are case-sensitive. Shell commands are all lowercase.
Resolution: Always use cd, ls, pwd.

Why this is a common trap: Windows and macOS file systems are often case-insensitive by default, training habits that break on Linux. All professional ASIC work happens on Linux.

Issue 2 — Old Docker Image Not Found
Error: Unable to find image 'openlane:rc2'
Root Cause: The workshop uses a specific OpenLANE release. The rc2 tag referred to an older release candidate. Running make without arguments downloads the correct image.
Resolution: Run make to pull the correct Docker image before attempting make mount.

Issue 3 — Running OpenLANE Outside the Container
Error: can't find package json
Root Cause: The OpenLANE Tcl scripts depend on tool binaries and packages that exist only inside the Docker container. Running flow.tcl directly on the host machine will fail because those tools are not installed on the host.
Resolution: Always enter the container first with make mount, then run ./flow.tcl.

Issue 4 — Incorrect prep Command
Error: [ERROR]: No design configuration (config.json/config.tcl) found in /openlane
Root Cause: Running prep design picorv32a (without the - flag) tells OpenLANE to look for a design named picorv32a in the current directory rather than in the designs folder.
Resolution: Use the correct syntax: prep -design picorv32a

12. Interview Questions
Q1: What is a PDK?
A Process Design Kit is a collection of files provided by a semiconductor foundry describing their manufacturing process. It includes design rules (DRC), SPICE transistor models, standard cell libraries, technology files (layer stack, parasitics), and I/O pad cells. Without a PDK, it is impossible to design a manufacturable chip for a specific process node.

Q2: What is OpenLANE?
OpenLANE is an open-source automated RTL-to-GDSII ASIC flow developed by eFabless. It integrates 16+ open-source EDA tools (Yosys, OpenROAD, Magic, TritonRoute, Netgen, OpenSTA, Fault, etc.) into a single pipeline that can take a Verilog design and produce a clean GDSII file ready for fabrication — ideally without manual intervention.

Q3: What is the difference between LEF and GDS?
Aspect	LEF	GDS (GDSII)
Purpose	Abstract layout for P&R tools	Full physical layout for fabrication
Contents	Cell boundary, pin locations, layer blockages	All polygon geometries on all layers
Size	Small	Large
Usage	Floorplanning, placement, routing	Photomask generation
LEF hides internal cell implementation from the P&R tool (which doesn't need it) while providing the information needed for routing (pin shapes and positions).

Q4: Why run synthesis before physical design?
Synthesis converts the behavioral RTL description (what the circuit should do) into a structural netlist (what gates implement it). Physical design tools (floorplanning, placement, routing) work on gates and their connections — they cannot process RTL directly. Synthesis also provides initial timing estimates and area estimates that guide physical design decisions.

Q5: What is PicoRV32A?
PicoRV32A is a compact, open-source RISC-V processor implementing the RV32IMC ISA. It is included as a reference design in OpenLANE because it is well-understood, synthesizes cleanly, and is complex enough to exercise all stages of the physical design flow without being so large that it takes hours to run.

Q6: What does "clean GDSII" mean?
A clean GDSII has:

No DRC violations — all layout geometries conform to the foundry's manufacturing design rules
No LVS violations — the extracted physical netlist matches the intended schematic
Timing closure — all setup and hold constraints are met at the target frequency
Q7: What is Static Timing Analysis (STA)?
STA is a method of verifying circuit timing without simulation. It traces every combinational path from flip-flop to flip-flop, calculates the delay along each path (sum of cell delays and wire delays), and checks whether:

Setup constraint is met: data arrives before the clock edge (path delay < clock period − setup time)
Hold constraint is met: data doesn't change too soon after the clock edge (path delay > hold time)
STA is fast (no test vectors needed) and exhaustive (analyzes all paths simultaneously).

Q8: Why does CTS require an LEC check afterward?
CTS modifies the netlist by inserting clock buffer/inverter cells and potentially adjusting cell positions. These modifications, while physically necessary, could theoretically introduce logical errors (e.g., clock arriving inverted to some flip-flops). LEC formally verifies that the post-CTS netlist is logically equivalent to the pre-CTS netlist.

13. Key Learnings and Conclusion
Conceptual Foundations Established
Computer-to-Hardware communication chain — understanding how a C program ultimately becomes transistors switching on silicon, and why each layer of abstraction exists, is foundational to being an effective hardware engineer.

Chip anatomy — the distinction between die and package, core and I/O, macros and standard cells, soft and hard IP are concepts that appear constantly in every ASIC project.

The open-source ASIC ecosystem — SKY130 PDK + open-source EDA tools + open RTL designs form a complete, free ecosystem. This democratizes silicon access in a way that was impossible before 2020.

RTL-to-GDSII flow — each stage (synthesis → floorplan → placement → CTS → routing → sign-off) has a specific purpose. Understanding why each stage exists (not just what it does) is essential for debugging failures.

OpenLANE philosophy — the goal of producing a clean GDSII without human intervention requires careful integration of tools, smart handling of inter-stage dependencies (like LEC after CTS), and extensive design space exploration.

Practical Skills Gained
Docker-based tool environment setup and management
Interactive OpenLANE session management (entering container, loading packages)
Design preparation and understanding of what prep -design actually does
Running synthesis and interpreting the output statistics
Navigating the run directory structure to find logs, reports, and results
Calculating and interpreting the DFF ratio
Day 1 Completion Status
Task	Status
Docker environment setup	✅ Complete
OpenLANE container entry	✅ Complete
Interactive mode launch	✅ Complete
PicoRV32A design preparation	✅ Complete
Synthesis run	✅ Complete
Netlist generation	✅ Complete
Synthesis report analysis	✅ Complete
Next: Day 2
Day 2 will cover Floorplanning in depth:

Utilization Factor and Aspect Ratio calculations
Pre-placed cells and their significance
Decoupling Capacitors — why they are needed and where to place them
Power Planning — mesh topology and IR drop
Running run_floorplan in OpenLANE and analyzing results in Magic
