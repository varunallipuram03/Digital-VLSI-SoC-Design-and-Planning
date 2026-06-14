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
# 1️⃣ Understanding Computer Abstraction Layers

## Why Are Abstraction Layers Necessary?

Humans communicate using languages, diagrams, and logical reasoning, whereas digital hardware operates only with binary signals represented by logic **0** and logic **1**. To bridge this gap, modern computing systems rely on multiple layers of abstraction, where each layer translates information into a form understandable by the next layer.

Without these abstraction layers, software developers would need to directly control transistors, making system development practically impossible.

---

## Computing Abstraction Hierarchy

```text
High-Level Programming Languages
(C, C++, Python, Java)
              │
              ▼
Compiler
              │
              ▼
Assembly Language
              │
              ▼
Assembler
              │
              ▼
Machine Instructions
              │
              ▼
Processor Hardware
              │
              ▼
Transistor-Level Switching
```
---
## Role of Each Layer

### High-Level Programming Languages

High-level languages enable developers to write complex programs using human-readable syntax without worrying about hardware implementation details.

Example:

```c
sum = a + b;
```

Although this appears to be a simple statement, the processor performs multiple low-level operations internally to execute it.

---

### Compiler

The compiler converts source code into assembly instructions specific to the target processor architecture.

Major compiler responsibilities include:

* Syntax and semantic analysis
* Code optimization
* Register allocation
* Instruction scheduling
* Architecture-specific code generation

---

### Assembly Language

Assembly language provides a symbolic representation of machine instructions, making processor operations easier for humans to understand and debug.
Example:

```assembly
ADD x5, x6, x7
```

This instruction directs the processor to add the contents of registers `x6` and `x7` and store the result in register `x5`.

---

### Machine Code

Machine code consists of binary patterns that are directly interpreted and executed by the processor.

Example:

```text
00000000011100110000001010110011
```

Each bit field represents information such as:

* Opcode
* Source Registers
* Destination Register
* Function Codes

---

### Instruction Set Architecture (ISA)

The ISA acts as the interface between software and hardware.

It defines:

* Available instructions
* Register organization
* Memory addressing methods
* Data formats
* Processor behavior

The ISA ensures that software developed for a particular architecture can execute correctly on any processor implementing that architecture.

---
## Importance in VLSI Design

From a VLSI perspective, the RTL and physical circuits designed by hardware engineers must faithfully implement the ISA specification.

Any design flaw at the hardware level can propagate through higher abstraction layers and impact software execution.

Understanding this hierarchy is therefore essential before studying the RTL-to-GDSII design flow, as it establishes the connection between software applications and silicon implementation.

---

