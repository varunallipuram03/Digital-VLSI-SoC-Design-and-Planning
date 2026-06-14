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

<p align="center">
  <img src="images/software_to_hardware_flow.png" width="700">
</p>

## 2. Chip Anatomy – Understanding the Building Blocks of a Chip

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

<p align="center"><b>Figure 1:</b> Software to Hardware Flow</p>

